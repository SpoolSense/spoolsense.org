# Spoolman Integration

[Spoolman](https://github.com/Donkie/Spoolman) is an open-source spool management system. SpoolSense syncs scanned spool data to Spoolman automatically.

## Setup

1. Install and run Spoolman (see [Spoolman docs](https://github.com/Donkie/Spoolman#installation))
2. During scanner setup, enter your Spoolman URL (e.g. `http://192.168.1.32:7912`)
3. Enable Spoolman in the scanner config

## How Sync Works

When a tag is scanned:

- **OpenPrintTag / TigerTag / OpenTag3D:** Scanner creates or updates the spool in Spoolman with material, color, weight, and manufacturer from the tag.
- **Generic UID tag:** Scanner looks up the spool by `nfc_id` in Spoolman's extra fields. If found, it displays the Spoolman data on the LCD and web UI.

## Registering UID-Only Tags

For plain NTAG215 tags with no data:

1. Scan the tag on the reader
2. Open `http://spoolsense.local/reader`
3. The UID is displayed. Click "Register in Spoolman"
4. Fill in material, manufacturer, and color
5. The spool is created in Spoolman with the tag's UID as `nfc_id`

Future scans of this tag will automatically look up the spool data from Spoolman.

## Spoolman Extra Field

SpoolSense uses the `nfc_id` extra field in Spoolman to link physical NFC tags to spool records. The installer creates this field automatically. To create it manually:

```bash
curl -X POST http://your-spoolman:7912/api/v1/field \
  -H "Content-Type: application/json" \
  -d '{"name": "nfc_id", "entity_type": "spool", "field_type": "text"}'
```
