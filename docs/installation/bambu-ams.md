# Bambu Lab AMS Integration

Automatically push spool data from SpoolSense into your Bambu Lab AMS tray using Home Assistant.

When you scan a spool with SpoolSense and load it into an AMS tray, the blueprint sets the tray's filament type, color, and temperature settings — no manual entry needed.

## Prerequisites

- Bambu printer set to **LAN Only Mode** with **Developer Mode** enabled (required for local MQTT access)
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

## Automatic Weight Deduction (Optional)

Track filament usage automatically — when a print finishes on your Bambu printer, the weight consumed is deducted from the spool in Spoolman.

### Additional Prerequisites

- SpoolSense scanner firmware v1.6.18+ (or dev branch)
- Spoolman configured on the scanner (the scanner handles all Spoolman updates — no spoolman-homeassistant integration needed)

### Create Input Text Helpers

The two blueprints share state through HA helpers. Create one pair of `input_text` helpers per AMS tray:

1. Go to **Settings → Devices & Services → Helpers → + Create Helper → Text**
2. For each AMS tray, create two helpers:

    | AMS Tray Entity | UID Helper Name | Spoolman ID Helper Name |
    |---|---|---|
    | `sensor.p1s_ams_tray_1` | `spoolsense_p1s_ams_tray_1_uid` | `spoolsense_p1s_ams_tray_1_spoolman_id` |
    | `sensor.p1s_ams_tray_2` | `spoolsense_p1s_ams_tray_2_uid` | `spoolsense_p1s_ams_tray_2_spoolman_id` |
    | `sensor.p1s_ams_tray_3` | `spoolsense_p1s_ams_tray_3_uid` | `spoolsense_p1s_ams_tray_3_spoolman_id` |
    | `sensor.p1s_ams_tray_4` | `spoolsense_p1s_ams_tray_4_uid` | `spoolsense_p1s_ams_tray_4_spoolman_id` |

    Replace `p1s` with your printer's entity prefix.

### Install the Deduction Blueprint

1. In Home Assistant, go to **Settings → Automations & Scenes → Blueprints**
2. Click **Import Blueprint**
3. Paste this URL:

    ```
    https://github.com/SpoolSense/spoolsense_scanner/blob/main/homeassistant/blueprints/spoolsense_bambu_deduction.yaml
    ```

4. Click **Preview** then **Import Blueprint**

### Create the Deduction Automation

1. Click **Create Automation** on the blueprint
2. Configure:

    | Input | What to select |
    |-------|----------------|
    | **Bambu Printer** | Your Bambu printer device |
    | **Print Weight Sensor** | Your print weight sensor (e.g., `sensor.p1s_print_weight`) |
    | **AMS Tray Sensors** | Same tray sensors as the loading blueprint |
    | **SpoolSense Scanner Device ID** | Your scanner's device ID (shown on the scanner landing page, e.g., `4d9620`) |

3. Click **Save**

### How It Works

1. You scan a spool and load it into an AMS tray (handled by the loading blueprint)
2. The loading blueprint stores the spool's UID and Spoolman ID in the helpers
3. You start a print
4. When the print finishes (or is canceled), the deduction blueprint:
    - Reads how many grams each tray consumed from the print weight sensor
    - Looks up the stored UID for each tray from the input helpers
    - Sends an MQTT deduction command to the SpoolSense scanner (`cmd/deduct/{uid}`)
5. The scanner receives the deduction and:
    - If the tag is on the scanner and writable (OpenPrintTag/OpenTag3D) — writes the new weight to the NFC tag
    - Updates Spoolman directly with the new remaining weight
    - If Spoolman is unreachable, stores the deduction in NVS for retry on next scan

### Deduction Troubleshooting

**Weight not deducted after print**

- Check that the loading blueprint stored the UID: go to **Developer Tools → States**, search for `input_text.spoolsense_*_uid` — it should show the tag UID
- Check that the print weight sensor has per-tray attributes after a print: **Developer Tools → States** → `sensor.p1s_print_weight`
- Verify the scanner is connected to MQTT and receiving commands — check `spoolsense.local/logs` for deduction events
- Verify Spoolman is configured and reachable from the scanner

**Wrong amount deducted**

- The deduction uses the weight reported by the Bambu printer, which is estimated from g-code metadata — not measured. Small variations are normal.

**Multi-material prints**

- Each tray's usage is deducted independently from its mapped spool. Multi-color and multi-material prints are supported.
