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

1. **Tag scanned** -- Place a spool with an NFC tag on the scanner. The ESP32 reads the tag via SPI (PN5180 or PN532).

2. **Tag classified** -- The firmware auto-detects the tag format: OpenPrintTag, TigerTag, OpenTag3D, or plain UID. Each format has its own parser.

3. **Spoolman synced** -- If Spoolman is configured, the scanner sends the spool data via HTTP. For UID-only tags, it looks up the spool by `nfc_id`.

4. **MQTT published** -- The scanner publishes spool state to MQTT. Home Assistant auto-discovers the scanner as a device with sensors for material, color, weight, and status.

5. **Middleware routes** (optional) -- If running the middleware on your Klipper host, it receives the MQTT event and can:
    - Assign the spool to an AFC lane or toolhead
    - Write spool data to Moonraker's lane_data for slicer integration
    - Update the NFC tag with remaining weight after a print

## With vs Without Middleware

| Feature | Scanner Only | Scanner + Middleware |
|---------|-------------|---------------------|
| Read NFC tags | Yes | Yes |
| Spoolman sync | Yes | Yes |
| Home Assistant | Yes | Yes |
| AFC lane assignment | No | Yes |
| Toolchanger support | No | Yes |
| Tag weight writeback | No | Yes |
| Slicer integration (Orca) | No | Yes |

The scanner works standalone for basic spool tracking. Add the middleware when you need Klipper/AFC integration.
