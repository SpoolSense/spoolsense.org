# Bambu Lab AMS Integration

Automatically push spool data from SpoolSense into your Bambu Lab AMS tray using Home Assistant.

When you scan a spool with SpoolSense and load it into an AMS tray, the blueprint sets the tray's filament type, color, and temperature settings — no manual entry needed.

## Prerequisites

- SpoolSense scanner connected to Home Assistant via MQTT ([setup guide](home-assistant.md))
- [Bambu Lab Home Assistant integration](https://github.com/greghesp/ha-bambulab) installed and configured
- Bambu printer with AMS connected and visible in HA

## Install the Blueprint

1. In Home Assistant, go to **Settings → Automations & Scenes → Blueprints**
2. Click **Import Blueprint** (bottom right)
3. Paste this URL:

    ```
    https://github.com/SpoolSense/spoolsense_scanner/blob/main/homeassistant/blueprints/spoolsense_bambu_ams.yaml
    ```

4. Click **Preview** then **Import Blueprint**

## Create the Automation

1. After importing, click **Create Automation** on the blueprint
2. Configure the inputs:

    | Input | What to select |
    |-------|----------------|
    | **SpoolSense Spool Sensor** | Your SpoolSense spool sensor (e.g., `sensor.spoolsense_ecb338_spool`) |
    | **AMS Tray Sensors** | All AMS tray sensors you want to monitor (e.g., `sensor.x1c_ams_1_tray_1` through `tray_4`) |
    | **Scan-to-Load Timeout** | How long to wait after a scan for a tray load (default: 300 seconds) |

3. Click **Save**

## How to Use

1. **Scan** your spool's NFC tag on the SpoolSense scanner
2. **Load** the filament into an AMS tray within 5 minutes (or your configured timeout)
3. The blueprint automatically sets the tray's material, color, and nozzle temperatures

That's it. The AMS tray now knows what filament is loaded.

## What Data Gets Set

| From SpoolSense | To Bambu AMS | Notes |
|-----------------|-------------|-------|
| Material type (PLA, PETG, etc.) | Tray filament type | Direct mapping |
| Color (hex RGB) | Tray color | Alpha channel added automatically |
| Min print temp | Nozzle temp min | Only set if tag has temp data |
| Max print temp | Nozzle temp max | Only set if tag has temp data |

## Troubleshooting

**Blueprint doesn't trigger after scanning**

- Check that your SpoolSense sensor shows `present` in HA when you scan a tag
- Verify MQTT is working: **Developer Tools → States**, search for your `sensor.spoolsense_*` entity

**AMS tray not detected**

- Make sure you selected the correct tray sensor entities in the automation config
- The tray sensor's `empty` attribute must change from `true` to `false` when filament is loaded
- Check your Bambu Lab integration is online: **Settings → Devices → Bambu Lab**

**Material type not recognized by Bambu**

- SpoolSense uses standard material names (PLA, PETG, ABS, ASA, TPU, PC, Nylon, PVA, HIPS)
- If your tag has a non-standard material name, the Bambu integration may reject it
- You can check what the tag reports at `http://spoolsense.local/reader`

**Timeout expired (nothing happened)**

- The default timeout is 300 seconds (5 minutes) — you must load the filament within this window after scanning
- You must scan first, then load. Loading before scanning is not detected
- Increase the timeout in the automation settings if needed
