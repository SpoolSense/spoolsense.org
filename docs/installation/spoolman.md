# Spoolman Integration

SpoolSense syncs scanned spool data to [Spoolman](https://github.com/Donkie/Spoolman) automatically. This page covers what's needed to connect them and how the integration works.

## Requirements

- Spoolman running and reachable over HTTP from the scanner

If you don't have Spoolman yet, see the [Spoolman installation docs](https://github.com/Donkie/Spoolman#installation).

!!! note "Spoolman is optional"
    Smart tags (TigerTag, OpenPrintTag, OpenTag3D, OpenSpool) work without Spoolman — data comes from the tag itself. NFC+ tags (UID-only) require Spoolman for spool data lookup. Note that only OpenPrintTag and OpenTag3D store remaining weight on the tag, so filament usage can be tracked without Spoolman. TigerTag and OpenSpool don't store remaining weight — Spoolman is needed for usage tracking with those formats.

## Connecting SpoolSense to Spoolman

During scanner setup (installer or web config), enter your Spoolman URL:

- Same machine as scanner: `http://localhost:7912`
- Different machine: `http://192.168.x.x:7912`

The scanner tests the connection on boot and shows the result on the troubleshooting page (`/troubleshoot`).

## How Sync Works

When a tag is scanned, the scanner syncs with Spoolman over HTTP:

- **Smart tags** contain filament data on the tag. The scanner searches Spoolman for a matching spool by UID. If no match exists, a new vendor, filament, and spool entry are created automatically. If a match exists, the entry is updated.
- **NFC+ tags** carry only a UID. The scanner searches Spoolman by the `nfc_id` extra field. If found, the spool data is pulled from Spoolman for display and Home Assistant.

A sync cache prevents redundant API calls when the same spool is scanned repeatedly.

## Moonraker Integration

Add Spoolman to your Moonraker config so Klipper tracks filament usage and Fluidd/Mainsail can display spool info:

```ini
# moonraker.conf
[spoolman]
server: http://localhost:7912
sync_rate: 5
```

Restart Moonraker after adding this.

!!! tip "Enable the Spoolman panel in Mainsail"
    **Settings (gear icon) → Dashboard → scroll to "Spoolman" → toggle it on.** In Fluidd, the panel appears automatically.

## Extra Fields

SpoolSense uses extra fields in Spoolman to link NFC tags to spool records. The scanner auto-registers these on first sync (v1.6.6+):

| Field | Purpose |
|-------|---------|
| `nfc_id` | Links a physical NFC tag UID to a spool record |
| `tag_format` | Stores the tag format (TigerTag, OpenTag3D, OpenSpool, etc.) |

To create them manually (only needed if setting up Spoolman before flashing):

```bash
curl -X POST http://your-spoolman:7912/api/v1/field/spool/nfc_id \
  -H "Content-Type: application/json" \
  -d '{"name": "nfc_id", "field_type": "text", "default_value": "\"\""}'
```

## Registering NFC+ Tags

For plain NTAG tags with no data written:

1. Scan the tag on the reader
2. Open `http://spoolsense.local/reader`
3. The UID is displayed. Click "Register in Spoolman"
4. Fill in material, manufacturer, and color
5. The spool is created in Spoolman with the tag's UID as `nfc_id`

Future scans of this tag will automatically look up the spool data from Spoolman.

## Cleaning Up Duplicates

If you have duplicate vendors, filaments, or spools from earlier firmware versions, use the cleanup script:

```bash
cd ~/SpoolSense/middleware
python3 scripts/spoolman-cleanup.py http://your-spoolman:7912

# Preview only (no deletions):
python3 scripts/spoolman-cleanup.py http://your-spoolman:7912 --dry-run
```

!!! warning "Filaments with linked spools"
    Spoolman won't delete a filament that has spools using it. Reassign those spools first, then re-run the cleanup.
