# Prusa MK4 / MMU3 Integration

Track which spool is in each MMU3 slot (or your MK4), get a heads-up when the gcode wants a different material than what you've bound, and have Spoolman deducted automatically when prints finish — all orchestrated by Home Assistant.

## What You Get

- **Per-slot binding** — scan a spool at a slot's scanner, the spool's UID is bound to that slot in HA
- **Pre-print check** — when a print starts, HA compares the gcode's expected per-tool material to what's bound to each slot and notifies you on mismatch
- **Auto-deduction** — when a print finishes, HA reads the slicer's per-tool grams estimate and publishes a deduction to the scanner, which updates Spoolman

## Hard Limits — Read These First

Prusa users have a meaningfully different experience than Bambu users because of how the PrusaLink local API works:

- **Deduction uses the slicer's pre-print estimate, not actual measured usage.** Bambu reports actual; Prusa cannot. Expect ~1g of drift per clean print, more on prints with heavy stringing or aborted retractions.
- **Canceled prints are prorated by progress %.** If you cancel at 60%, HA deducts 60% of the slicer estimate. If the progress reading is unavailable at the moment of cancel, deduction is 0g and you'll need to adjust Spoolman manually.
- **There is no printer-side detection of "spool was loaded into slot N."** PrusaLink doesn't expose that information. If you scan a spool at slot 2 but physically load it into slot 3, the system has no way to catch the mistake.
- **The Prusa LCD will not show the scanned spool.** PrusaLink has no write API for slot/material binding. Nothing on the printer side reflects what's bound in HA.
- **Material name matching is permissive.** The mismatch check uses bidirectional substring matching (uppercased on both sides). This forgives common cases like gcode `"PLA"` vs Spoolman `"PRUSAMENT PLA"`, but it can also miss real mismatches when one material name is a substring of another (e.g. gcode `"PETG-CF"` vs slot `"PETG"` won't be flagged). Keep the default **Notify only** mismatch action until you've validated your slicer profile + Spoolman naming.

If those tradeoffs are deal-breakers, this integration isn't for you. Most HA users find the Spoolman auto-deduction worth the slicer-estimate drift.

## Required Components

| Component | Where it runs | Required version |
|---|---|---|
| Home Assistant | Your HA host | 2024.6 or newer |
| MQTT broker | HA add-on (Mosquitto) or external | Any current |
| **MQTT** integration | HA, built-in | n/a |
| **PrusaLink** integration | HA, built-in | n/a |
| **RESTful Sensor** integration | HA, built-in (`sensor` platform) | n/a |
| Spoolman | Service on your network | 0.20.0 or newer |
| SpoolSense scanner firmware | Per scanner | **v1.6.14 or newer** (v1.7.x recommended) |
| PrusaLink HTTP API | On the printer or on a Raspberry Pi (see below) | API v1 endpoints (Buddy 4.4+ or current RPi PrusaLink) |

The MQTT, PrusaLink, and RESTful Sensor pieces are all part of HA core — no HACS or custom integrations to install. Only the MQTT broker (Mosquitto) is an add-on you may need to enable separately.

### How PrusaLink is hosted

PrusaLink is the HTTP API this integration depends on. Where it runs depends on the printer:

- **Built into the printer** on Buddy firmware 4.4 or newer. Covers MK4 / MK4S, MK3.5, MK3.9, XL, and MINI/+. Available out of the box at `http://YOUR_PRUSA_IP/`.
- **On an external Raspberry Pi** for older Marlin-based printers (MK3S+, MK3, MK2.5S). Install [PrusaLink for RPi](https://github.com/prusa3d/Prusa-Link), connect the Pi to the printer over USB. The Pi exposes the same HTTP API at the same endpoints.

Both deployment paths expose the same `/api/v1/job`, `/api/v1/status`, and `/api/v1/info` endpoints with the same response shape, so the rest of this guide is identical for either setup. Use whichever IP serves the API.

## Step 1: Add the REST Sensor

The HA PrusaLink integration doesn't surface gcode metadata as an entity attribute, so you need a manual REST sensor that polls `/api/v1/job` and exposes `file.meta`. This is what makes per-tool deduction possible.

Add this block to your `configuration.yaml`. The recommended path uses the same HTTP Digest credentials your PrusaLink integration already uses — simpler than the API key path and consistent with the rest of HA's PrusaLink wiring.

```yaml
sensor:
  - platform: rest
    name: prusa_job_meta
    resource: http://YOUR_PRUSA_IP/api/v1/job
    authentication: digest
    username: maker
    password: !secret prusa_password
    scan_interval: 10
    json_attributes_path: "$.file"
    json_attributes:
      - meta
    value_template: "{{ value_json.state | default('unknown') }}"
```

Then in `secrets.yaml`:

```yaml
prusa_password: "your-prusalink-password-here"
```

### About the credentials

PrusaLink's HTTP API uses HTTP Digest authentication. On Buddy firmware **before 5.0**, the credentials are a username (default `maker`) plus an API key shown during the PrusaLink setup wizard. On Buddy firmware **5.0 and newer**, the API key is renamed to "Password" and shown in the printer's **Settings → Network → PrusaLink** screen — the username is still `maker` unless you explicitly changed it. Whatever credentials let your PrusaLink HA integration connect will work here.

### Alternative: API key in header (no digest)

PrusaLink also accepts an `X-Api-Key` header. This isn't documented in the OpenAPI spec but works on current firmware. Use this if HTTP Digest gives trouble:

```yaml
sensor:
  - platform: rest
    name: prusa_job_meta
    resource: http://YOUR_PRUSA_IP/api/v1/job
    headers:
      X-Api-Key: !secret prusa_api_key
    scan_interval: 10
    json_attributes_path: "$.file"
    json_attributes:
      - meta
    value_template: "{{ value_json.state | default('unknown') }}"
```

### What "unavailable" looks like

PrusaLink returns **HTTP 204 No Content** when there is no print queued or running. The REST sensor will show as `unavailable` in HA during idle — that's expected, not a bug. As soon as you queue or start a print, the response becomes JSON and the `meta` attribute populates. Both blueprints below trigger off the **printer state** sensor (not the REST sensor's state), so the REST sensor being unavailable while idle does not break anything.

After restart, **Developer Tools → States** should show `sensor.prusa_job_meta` populating with a `meta` attribute (containing keys like `filament_type per tool` and `filament used [g] per tool`) once a print is queued or running.

## Step 2: Create the Slot Helpers

In HA, go to **Settings → Devices & Services → Helpers → Create Helper → Text**.

For **MK4**: create one helper named `prusa_slot_0`.

For **MMU3**: create five helpers named `prusa_slot_0` through `prusa_slot_4`.

Set **Maximum length** to **120** for each helper (the binding payload is JSON with UID, spoolman ID, full material name, and color hex — vendor-prefixed material names like "Prusament PLA Vanilla White" can push past 100 chars).

## Step 3: Import the Blueprints

Go to **Settings → Automations & Scenes → Blueprints → Import Blueprint** and paste each URL.

**1. Slot Binding** — runs once per scan to update a slot's helper

```
https://github.com/SpoolSense/spoolsense_scanner/blob/main/homeassistant/blueprints/spoolsense_prusa_binding.yaml
```

**2. Print Lifecycle** — runs on print start/finish/cancel for pre-check + deduction

```
https://github.com/SpoolSense/spoolsense_scanner/blob/main/homeassistant/blueprints/spoolsense_prusa_lifecycle.yaml
```

## Step 4: Create the Binding Automations

Click **Create Automation** on the **SpoolSense → Prusa Slot Binding** blueprint. Create one instance **per slot** you're tracking.

| Input | What to select |
|-------|----------------|
| **SpoolSense Spool Sensor** | The spool sensor for the scanner mounted at this slot (e.g., `sensor.spoolsense_4d9620_spool`) |
| **Slot Helper** | The matching `input_text.prusa_slot_<N>` helper |
| **Slot Label** | Optional human label for notifications (e.g., "Slot 2") |
| **Notify on Bind** | Whether to send a notification when a spool is bound |

**MK4**: one automation, scanner sensor → `prusa_slot_0`.

**MMU3**: five automations, one per scanner, each pointing at the matching slot helper.

## Step 5: Create the Lifecycle Automation

Click **Create Automation** on the **SpoolSense → Prusa Print Lifecycle** blueprint. Create **one instance per printer**.

| Input | What to select |
|-------|----------------|
| **PrusaLink State Sensor** | Your printer's main state sensor (e.g., `sensor.prusa_mk4`) |
| **PrusaLink Progress Sensor** | The progress sensor (e.g., `sensor.prusa_mk4_progress`) |
| **Prusa Job Meta REST Sensor** | The REST sensor from Step 1 (`sensor.prusa_job_meta`) |
| **Slot Helpers (in slot order)** | All slot helpers in physical order — for MK4 just one, for MMU3 all five from slot 0 to slot 4 |
| **SpoolSense Scanner Device ID** | Any one of your scanner device IDs (used as MQTT topic prefix; the deduction message carries the spool UID for resolution) |
| **Material Mismatch Action** | Default: notify only. Switch to "Notify + pause" only if your slicer profiles match Spoolman material names exactly. |
| **PrusaLink Pause Button** | Required only if you chose "Notify + pause" — the printer's pause button entity (e.g., `button.prusa_mk4_pause_job`) |

!!! warning "Slot order matters"
    The position of each slot helper in the **Slot Helpers** list determines which gcode tool index it maps to. Position 0 = tool 0 in the gcode = MMU slot 0, etc.

## How to Use

### MK4 Single-Spool Workflow

1. Scan a spool's NFC tag on the scanner
2. The binding automation writes the spool's UID + material to `prusa_slot_0`
3. Physically load the filament into the MK4
4. Start the print from PrusaLink web UI / USB / printer LCD
5. When the print starts, HA refreshes the REST sensor, then checks if the gcode's expected material matches what's bound — notifies on mismatch
6. When the print finishes, HA deducts the slicer-estimated grams from the bound spool in Spoolman

### MMU3 Multi-Color Workflow (5-Scanner Mode)

1. At each lane that's getting a new spool, scan the spool's NFC tag on **that lane's scanner**
2. Each scanner's binding automation only matches its own lane, so each scan ends up in the correct slot helper
3. Physically load each filament into its matching MMU3 lane
4. Start your multi-color print
5. When the print starts, HA reads the gcode's `filament_type per tool` array and notifies you of any mismatched slots
6. When the print finishes, HA reads the per-tool grams array and publishes one deduction per slot (only slots that used filament are deducted)

### Canceled Prints

If you cancel mid-print, HA reads the last known progress %, prorates the deduction estimate accordingly, and sends a notification telling you to adjust Spoolman manually if needed. If progress is unavailable at the moment of cancel, deduction is 0g.

## Troubleshooting

**REST sensor shows `unknown`/`unavailable` while no print is queued**

That's normal — PrusaLink returns 204 No Content when idle. The sensor populates the moment you queue or start a print.

**REST sensor shows `unavailable` even during a print**

- Confirm the URL works: `curl --digest -u maker:YOUR_PASSWORD http://YOUR_PRUSA_IP/api/v1/job`
- If digest fails on Buddy 5.x firmware, switch to the `X-Api-Key` header alternative shown above
- Confirm your firmware exposes the v1 endpoints (Buddy 4.4+)

**Pre-print check sends false-positive mismatches**

- Compare what's in the gcode vs what's in Spoolman: **Developer Tools → States** → `sensor.prusa_job_meta` → look at the `meta` attribute's `filament_type per tool` array
- Compare to your slot helpers' state values (the `mat` field in the JSON)
- The match logic is permissive bidirectional substring (uppercased). False positives usually mean one side has a totally different family (PLA vs PETG). False negatives are most common when one material name is a substring of another (PETG inside PETG-CF).
- Stick with **Notify only** mismatch action until you've confirmed your strings line up

**Deduction not happening on print finish**

- Check the lifecycle automation's trace: **Settings → Automations** → click the lifecycle automation → **Traces**
- Verify the REST sensor's `meta["filament used [g] per tool"]` is populated post-print
- Check that the slot helper's stored UID matches a spool the scanner has seen (the scanner stores UID → spool mappings in NVS)

**Wrong slot got deducted**

- The system has no way to detect physical-load mismatches. If you scanned spool X at slot 2 but physically loaded spool Y, slot 2 will be deducted under spool X. Adjust Spoolman manually.

**Pause button doesn't fire on mismatch**

- "Notify + pause" requires you to select the printer's Pause button entity in the lifecycle automation config
- The pause action only works if the print is in `printing` state — the integration won't fire pause from `paused` or `idle`

## FAQ

**Can I use one scanner for all 5 MMU3 slots?**

Yes, but it's the fallback path, not the default. You'd need a way to tell HA which slot the next scan is for — either a dashboard `input_select` helper or the scanner's optional 3x4 keypad. Walking around to scan at the right lane with 5 scanners is the smoother UX.

**Does this work without Home Assistant?**

No. The whole orchestration lives in HA. PrusaLink's local API has no slot-binding write endpoints, so there's no scanner-direct path equivalent to the Snapmaker U1 integration.

**Will the spool show up on the Prusa LCD?**

No. The Prusa LCD is read-only from outside the printer; PrusaLink has no API to set what it displays. This is a Prusa-side limit.

**Why is deduction off by a few grams?**

Because it's the slicer's pre-print estimate, not measured usage. The estimate doesn't account for stringing, purges, retraction failures, or aborted prints (cancellation is prorated, but only by progress %). For most users, the drift is acceptable; for high-precision tracking, the Bambu integration is more accurate because Bambu reports measured usage.
