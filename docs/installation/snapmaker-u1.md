# Snapmaker U1 Integration

The SpoolSense scanner posts scan data directly to a Snapmaker U1's Moonraker via the extended firmware's `filament_detect/set` endpoint.

All six SpoolSense tag formats are supported on the U1:

- OpenSpool
- OpenPrintTag
- TigerTag
- OpenTag3D
- Bambu UID
- NFC+ (generic UID)

This matches the tag coverage of the Voron / generic Klipper integration.

!!! warning "Required: paxx12 Extended Firmware"
    This integration requires the [paxx12 Snapmaker U1 Extended Firmware](https://github.com/paxx12/SnapmakerU1-Extended-Firmware/tree/develop) (develop branch). The stock Snapmaker firmware does not expose the API.

---

## How It Works

```
NFC Tag → SpoolSense Scanner → /printer/filament_detect/set → U1 channel state → Fluidd
```

Each scanner is bound to one toolhead channel (0–3), stored in the scanner's NVS. When you scan a tag, the scanner posts the tag's data — vendor, material, color, temps, UID — to the U1 over Moonraker. The extended firmware writes that into the corresponding channel's state, and Fluidd reflects it.

For a 4-toolhead U1, deploy four scanners — one per channel. For single-color use, one scanner is enough.

---

## Setup

### 1. Flash the SpoolSense Scanner

If your scanner isn't already running v1.7.5 or later, flash it first using the [Web Flasher](web-flasher.md). The U1 Integration section in the scanner's config page only appears in v1.7.5 and later.

### 2. Install the Extended Firmware on the U1

Follow paxx12's instructions: [paxx12/SnapmakerU1-Extended-Firmware](https://github.com/paxx12/SnapmakerU1-Extended-Firmware/tree/develop) (develop branch).

### 3. Enable External Filament Detection on the U1

After the U1 reboots:

1. Open `http://<printer-ip>/firmware-config/`
2. Set **Filament Detection** to **External**
3. Save

### 4. Configure the Scanner

On each SpoolSense scanner (`http://<scanner-hostname>.local/config`):

1. Under **Klipper / Moonraker**, set the Moonraker URL to your U1 (e.g. `http://192.168.1.72:7125`)
2. Under **Snapmaker U1 Integration**, toggle **Enable U1 Integration** on
3. Pick the **Toolhead Channel** (0–3) this scanner serves
4. Save & Reboot

The channel binding is written to NVS and persists across reboots and OTA updates.

### 5. Test

Scan a tag. In Fluidd, check the channel state for the bound toolhead. The vendor, material, color, and temperatures should reflect the scanned tag.

---

## Multi-Scanner Setup

For a fully-loaded U1, deploy one scanner per toolhead. Each scanner has its own NVS, so the channel and hostname are configured independently.

| Scanner hostname | Channel |
|---|---|
| `spoolsense-t0.local` | 0 |
| `spoolsense-t1.local` | 1 |
| `spoolsense-t2.local` | 2 |
| `spoolsense-t3.local` | 3 |

When you select a channel in the config page, the hostname field defaults to `spoolsense-tN` (overridable).

!!! info "Coming soon: stage mode"
    A future release will add **stage mode** — one SpoolSense scanner serving all 4 U1 channels. Scanning a tag stages the spool; you then pick the target channel via keypad, web UI tap, or auto-detect when filament inserts into a toolhead. Lower hardware cost for casual / single-color users at the cost of one extra step per scan.

    The current per-toolhead mode (covered above) is the only mode shipping in v1.7.5.

---

## Per-Material Temperature Defaults

When a tag is missing temperature data, the scanner fills in defaults based on the material name:

| Material | Hotend min / max | Bed |
|---|---|---|
| PLA | 200 / 220°C | 60°C |
| PETG | 230 / 250°C | 70°C |
| ABS | 240 / 260°C | 100°C |
| ASA | 240 / 260°C | 100°C |
| TPU | 220 / 240°C | 50°C |
| PVA | 190 / 210°C | 60°C |
| PC | 260 / 290°C | 110°C |
| PA (Nylon) | 250 / 280°C | 90°C |

Defaults apply only to the recognized material types above. Other materials (PEEK, niche blends) require explicit temps on the tag or in Spoolman.

OpenSpool tags don't carry bed temp on-tag, so the bed-temp default is the most common path. If Spoolman has a record for the tag with bed temp set, the scanner does a follow-up POST after the Spoolman lookup to use that value instead of the default.

---

## Troubleshooting

**Channel state doesn't update after a scan.**

1. Check Filament Detection is set to External in `http://<printer-ip>/firmware-config/`
2. Check the Moonraker URL in the scanner config points to the U1
3. Open the scanner's serial log at `http://<scanner-hostname>.local/logs` and look for `U1Manager: publishFromDetection` — the HTTP code should be 200
4. HTTP code -1 or other negative values mean the scanner can't reach the U1. The scanner backs off for 30 seconds after a transport failure.

**Two scanners post to the same channel.**

Each channel binding is stored in the scanner's own NVS. Open the config page on the duplicate scanner and change its channel.

---

## Optional: Running the Middleware

The U1 integration above works scanner-direct — no middleware needed. But if
you also want middleware features (Spoolman weight writeback, the MQTT event
stream for Home Assistant automations, the mobile app), the U1 itself can't
host the middleware. Run it in Docker on any other machine on your LAN
instead — see [Middleware Setup → Run in Docker](middleware.md#run-in-docker).
