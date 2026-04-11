# Bambu Lab AMS Integration

Automatically push spool data from SpoolSense into your Bambu Lab AMS, display a live tray dashboard on the TFT, and track filament usage after prints.

## What You Get

- **Tray loading** — scan a spool, load it into the AMS, and the tray's filament type, color, and temperatures are set automatically
- **TFT dashboard** — persistent colored grid on the scanner's TFT showing what's in each AMS tray (material, color, weight)
- **Weight tracking** — after each print, filament usage is deducted from the spool in Spoolman

## Prerequisites

- SpoolSense scanner with firmware **v1.7.0+** and a TFT display connected
- Bambu printer set to **LAN Only Mode** with **Developer Mode** enabled
- SpoolSense scanner connected to Home Assistant via MQTT ([setup guide](home-assistant.md))
- [Bambu Lab Home Assistant integration](https://github.com/greghesp/ha-bambulab) installed and configured
- Spoolman configured on the scanner ([setup guide](spoolman.md))

## Step 1: Enable the Dashboard on the Scanner

1. Open `http://spoolsense.local/config` in your browser
2. Under **Hardware**, make sure **TFT Display** is enabled
3. Enable **Bambu AMS Dashboard (TFT)**
4. Click **Save** — the scanner will reboot

After reboot, the TFT will show "SpoolSense / AMS Ready" until tray data arrives.

## Step 2: Install the Blueprints

You need three blueprints. Import each one in Home Assistant:

Go to **Settings → Automations & Scenes → Blueprints → Import Blueprint** and paste each URL:

**1. Loading Blueprint** — pushes scanned spool data into the AMS tray

```
https://github.com/SpoolSense/spoolsense_scanner/blob/main/homeassistant/blueprints/spoolsense_bambu_ams.yaml
```

**2. Dashboard Blueprint** — keeps the TFT dashboard in sync with AMS tray state

```
https://github.com/SpoolSense/spoolsense_scanner/blob/main/homeassistant/blueprints/spoolsense_bambu_dashboard.yaml
```

**3. Deduction Blueprint** — deducts filament usage from Spoolman after prints

```
https://github.com/SpoolSense/spoolsense_scanner/blob/main/homeassistant/blueprints/spoolsense_bambu_deduction.yaml
```

## Finding Your Scanner Device ID and Entities

Before creating the automations, you'll need two pieces of information:

**Scanner Device ID** — This is the 6-character hex ID shown on the scanner's landing page at `http://spoolsense.local`. It's also the last 6 characters of the scanner's MAC address and appears in the MQTT topic prefix (e.g., `4d9620`).

**SpoolSense Spool Sensor** — Once the scanner is connected to HA via MQTT, it auto-creates a sensor entity. To find it: go to **Developer Tools → States** and search for `spoolsense`. You'll see an entity like `sensor.spoolsense_4d9620_spool` — the middle part is your device ID.

**AMS Tray Sensors** — These are created by the Bambu Lab HA integration. Search for `ams_tray` in **Developer Tools → States** to find entities like `sensor.p1s_ams_tray_1` through `sensor.p1s_ams_tray_4`. The prefix (e.g., `p1s`) matches your printer name in HA.

## Step 3: Create the Automations

After importing, create an automation from each blueprint.

### Loading Automation

Click **Create Automation** on the "SpoolSense → Bambu AMS" blueprint:

| Input | What to select |
|-------|----------------|
| **SpoolSense Spool Sensor** | Your SpoolSense spool sensor (e.g., `sensor.spoolsense_4d9620_spool`) |
| **AMS Tray Sensors** | All AMS tray sensors in order (e.g., `sensor.p1s_ams_tray_1` through `tray_4`) |
| **Scan-to-Load Timeout** | How long to wait after a scan for a tray load (default: 300 seconds) |
| **SpoolSense Scanner Device ID** | Your scanner's device ID (e.g., `4d9620`) |

### Dashboard Automation

Click **Create Automation** on the "SpoolSense → Bambu AMS Dashboard" blueprint:

| Input | What to select |
|-------|----------------|
| **SpoolSense Scanner Device ID** | Same device ID as above |
| **AMS Tray Sensors** | Same tray sensors, same order |

### Deduction Automation

Click **Create Automation** on the "SpoolSense → Bambu Deduction" blueprint:

| Input | What to select |
|-------|----------------|
| **Bambu Printer** | Your Bambu printer device |
| **Print Weight Sensor** | Your print weight sensor (e.g., `sensor.p1s_print_weight`) |
| **AMS Tray Sensors** | Same tray sensors, same order |
| **SpoolSense Scanner Device ID** | Same device ID as above |

!!! warning "Tray order matters"
    Select your AMS tray sensors in the correct order across all three automations. The first entity is tray index 0, second is index 1, etc. For multiple AMS units, select all trays across units in order (AMS 1 trays 1-4, then AMS 2 trays 1-4).

## How to Use

1. **Scan** your spool's NFC tag on the SpoolSense scanner
2. **Load** the filament into an AMS tray within 5 minutes
3. The loading blueprint sets the tray's material, color, and temperatures on the Bambu printer
4. The dashboard blueprint detects the tray change and updates the TFT display
5. If the spool is in Spoolman, the remaining weight appears on the dashboard

After a print finishes or is canceled, the deduction blueprint reads how much filament each tray used and sends the deduction to the scanner, which updates Spoolman automatically.

## What Data Flows Where

| From SpoolSense | To Bambu AMS | Notes |
|-----------------|-------------|-------|
| Material type (PLA, PETG, etc.) | Tray filament type | Mapped to Bambu filament profile IDs |
| Color (hex RGB) | Tray color | Alpha channel added automatically |
| Min/max print temp | Nozzle temp range | Only set if tag has temp data |

The scanner also stores which spool UID is in which tray. This lets the deduction blueprint work without any HA helpers — the scanner resolves tray to spool automatically.

## Dashboard Details

The TFT dashboard auto-detects the grid layout from the number of loaded trays:

| Loaded trays | Grid | Cell size |
|-------------|------|-----------|
| 1-4 | 2x2 | ~118x118px |
| 5-8 | 2x4 | ~118x58px |
| 9-16 | 4x4 | ~58x58px |

Each cell shows the filament color as the background, material type, and remaining weight in grams. Text color automatically switches between white and black for readability based on the background color.

The dashboard state is cached in NVS — it persists across reboots and shows instantly on power-on.

When you scan a tag, the dashboard temporarily shows the spool details for 5 seconds, then returns to the tray view.

## Troubleshooting

**Dashboard shows "AMS Ready" but no trays**

- The dashboard blueprint hasn't triggered yet. Check **Settings → Automations** and verify the dashboard automation is enabled
- Manually trigger the automation: click the three dots → **Run**
- Check MQTT connectivity — the scanner must be connected to the same broker as HA

**Blueprint doesn't trigger after scanning**

- Check that your SpoolSense sensor shows `present` in HA: **Developer Tools → States**, search for `sensor.spoolsense_*`
- Verify MQTT is working between the scanner and HA

**AMS tray not detected after loading filament**

- The tray sensor's `empty` attribute must change from `true` to `false` when filament is loaded
- Check your Bambu Lab integration is online: **Settings → Devices → Bambu Lab**
- Make sure you selected the correct tray sensor entities in the automation config

**Weight shows ?g on dashboard**

- The spool needs a matching entry in Spoolman with the NFC tag UID
- Verify Spoolman is configured and reachable from the scanner at `http://spoolsense.local/config`
- Check that the spool was scanned via SpoolSense before loading (this stores the UID-to-tray mapping)

**Weight not deducted after print**

- Check that the print weight sensor has per-tray attributes after a print: **Developer Tools → States** → `sensor.p1s_print_weight`
- Verify the scanner received the deduction — check `http://spoolsense.local/logs` for deduction events
- The spool must have been loaded via the SpoolSense loading blueprint (so the scanner knows which UID is in which tray)

**Screen blank on cold boot**

- Wire the TFT display's RST pin to the ESP32's RST/EN pin. See the [TFT wiring guide](../hardware/tft.md) for your board.

**Multi-material prints**

- Each tray's usage is deducted independently. Multi-color and multi-material prints are fully supported.

## Upgrading from v1.6.x

If you had the previous Bambu AMS blueprints installed:

1. Delete the old automations in HA (**Settings → Automations** → find SpoolSense Bambu automations → delete)
2. Delete the old blueprints (**Settings → Automations → Blueprints** tab → delete SpoolSense blueprints)
3. Delete any `input_text.spoolsense_*_uid` and `input_text.spoolsense_*_spoolman_id` helpers if you created them — they are no longer needed
4. Follow the setup steps above to install the new blueprints
