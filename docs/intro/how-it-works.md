# How It Works

SpoolSense has three layers: the **scanner** (hardware), the **middleware** (optional, for Klipper integration), and your **spool manager** (Spoolman).

## Scan Flow

```
  NFC Tag on Spool
        |
        v
  +-----------+     WiFi/MQTT      +------------+
  |  Scanner  | -----------------> | Middleware  |
  |  (ESP32)  |                    | (Klipper)  |
  +-----------+                    +------------+
        |                                |
        | HTTP                           | HTTP
        v                                v
  +-----------+                    +-----------+
  | Spoolman  |                    | Moonraker |
  +-----------+                    +-----------+
        |
        v
  +----------------+
  | Home Assistant |
  +----------------+
```

## Step by Step

### 1. Tag Scanned

Place a spool with an NFC tag on the scanner. The ESP32 reads the tag over SPI using either a PN5180 or PN532 NFC reader. The read happens in a continuous scan loop running in its own FreeRTOS task, so detection is near-instant.

### 2. Tag Classified

The firmware auto-detects the tag format based on the NFC protocol and tag contents:

- **ISO15693** (8-byte UID) ... OpenPrintTag on ICODE SLIX2 (PN5180 only)
- **ISO14443A** (4 or 7-byte UID) ... the firmware reads the tag pages and checks for known formats:
    - **TigerTag** ... binary layout starting at page 4 with a known version signature
    - **OpenTag3D** ... CBOR-encoded data with OpenTag3D MIME type
    - **Bambu Lab** ... MIFARE Classic detected by SAK byte (UID-only, encrypted data)
    - **Generic UID** ... plain NTAG with no recognized data format

Each format has its own parser that extracts material, color, weight, manufacturer, temperatures, and other fields.

### 3. Spoolman Synced

If Spoolman is configured, the scanner syncs with it over HTTP:

**Smart tags** (OpenPrintTag, TigerTag, OpenTag3D) contain filament data directly on the tag. The scanner searches Spoolman for a matching spool by UID. If no match is found, a new spool entry is created automatically with the material, color, weight, and manufacturer from the tag. If a match exists, the scanner updates the existing entry with the latest tag data.

**Basic UID tags** (NTAG215 with no data written) carry only a UID. The scanner searches Spoolman for a spool with a matching `nfc_id` extra field. If found, the spool's data (material, color, weight, manufacturer) is pulled from Spoolman and displayed on the LCD, web reader, and Home Assistant sensors. If not found, the tag is shown as unregistered and the user can register it via the web UI.

A sync cache prevents redundant API calls when the same spool is scanned repeatedly without changes.

### 4. LED + LCD Feedback

If a status LED is enabled, the scanner shows the spool's filament color on the LED. The actual hex color from the tag or Spoolman is used, so a red PLA spool lights up red. Black filament shows as dim white (so you can tell a spool is scanned vs. no spool). The LED also flashes white on tag detection, green on successful writes, and red on errors.

If an LCD is attached, it displays the material type, remaining weight, and status messages (WiFi, Spoolman sync, write progress, keypad entry).

### 5. MQTT Published

The scanner publishes spool state to MQTT. Home Assistant auto-discovers the scanner as a device with sensors for material, color, weight, manufacturer, and scanner status. All sensors update in real time as spools are scanned and removed.

### 6. Middleware Routes (Optional)

If running the middleware on your Klipper host, it receives the MQTT scan event and handles printer-side integration:

- **Assign the spool to an AFC lane or toolhead** ... the middleware sends gcode commands to Klipper (SET_SPOOL_ID, SET_ACTIVE_SPOOL, ASSIGN_SPOOL) and saves the assignment to Klipper variables so it persists across reboots.
- **Set AFC lane colors** ... when a spool is assigned to an AFC lane (BoxTurtle, NightOwl), the middleware sends the filament color to the lane's LED via SET_COLOR gcode. The lane lights up to match the loaded filament.
- **Set scanner LED color** ... for UID-only tags that have no color on the tag itself, the middleware looks up the color from Spoolman and sends it back to the scanner via MQTT. The scanner LED then shows the correct filament color.
- **Write spool data to Moonraker's lane_data** ... slicer integration for Orca Slicer and others. Tool info (color, material, weight, temperatures) is written to Moonraker's database so the slicer auto-populates tool settings.
- **Update the NFC tag with remaining weight** ... after a print, the middleware can write the updated remaining weight back to the NFC tag so the tag stays accurate even offline. (Opt-in feature, disabled by default.)

## With vs Without Middleware

| Feature | Scanner Only | Scanner + Middleware |
|---------|:---:|:---:|
| Read NFC tags | :white_check_mark: | :white_check_mark: |
| Spoolman sync | :white_check_mark: | :white_check_mark: |
| Home Assistant sensors | :white_check_mark: | :white_check_mark: |
| Scanner LED filament color | :white_check_mark: from tag | :white_check_mark: from tag or Spoolman |
| LCD display | :white_check_mark: | :white_check_mark: |
| AFC lane assignment | :x: | :white_check_mark: |
| AFC lane color LEDs | :x: | :white_check_mark: |
| Toolchanger support | :x: | :white_check_mark: |
| Keypad tool assignment | :white_check_mark: direct to Moonraker | :white_check_mark: also via middleware |
| Tag weight writeback | :x: | :white_check_mark: |
| Slicer integration (Orca) | :x: | :white_check_mark: |

The scanner works standalone for basic spool tracking, Spoolman sync, and Home Assistant. Add the middleware when you need Klipper/AFC integration, lane colors, or tag writeback.
