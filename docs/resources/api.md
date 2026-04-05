# API Reference

The scanner exposes a REST API at `http://spoolsense.local` (port 80).

## Endpoints

### GET /api/status

Returns current scanner state, scanned spool info, and Spoolman enrichment data.

```json
{
  "device_id": "ecb338",
  "firmware_version": "1.6.9",
  "present": true,
  "uid": "040A9DAE8F6181",
  "tag_data_valid": false,
  "tag_kind": "OpenTag3D",
  "opentag3d": {
    "material": "PLA",
    "modifiers": "HF",
    "manufacturer": "AzureFilm",
    "color_hex": "#F5EC00",
    "target_weight_g": 1000,
    "density": 1.24,
    "diameter_um": 1750,
    "print_temp_c": 210,
    "bed_temp_c": 60
  },
  "spoolman": {
    "spool_id": 132,
    "remaining_g": 850,
    "bed_temp": 60,
    "extruder_temp": 210,
    "density": 1.24,
    "diameter_mm": 1.75
  }
}
```

The `spoolman` sub-object appears when a Spoolman UID lookup matches the scanned tag. It persists after tag removal until the next scan.

### GET /api/diagnostics

Returns device health info.

```json
{
  "device_id": "ecb338",
  "firmware_version": "1.6.9",
  "wifi": { "connected": true, "ssid": "MyNetwork", "rssi_dbm": -45, "ip": "192.168.1.100" },
  "mqtt": { "enabled": true, "broker": "192.168.1.50", "connected": true },
  "spoolman": { "enabled": true, "url": "http://192.168.1.100:7912", "reachable": true },
  "nfc": { "ok": true, "reader": "PN5180 v4.0" },
  "memory": { "free_bytes": 80000, "total_bytes": 327680 }
}
```

### GET /api/config

Returns current configuration (passwords masked).

### POST /api/config

Updates configuration and reboots. Accepts JSON body with the same fields as GET.

### POST /api/write-tag

Write OpenPrintTag data to the current tag.

### POST /api/format-tag

Format a blank ISO15693 tag with the OpenPrintTag NDEF structure before writing.

### POST /api/write-tigertag

Write TigerTag binary data to the current tag.

### POST /api/write-opentag3d

Write OpenTag3D NDEF data to the current tag.

### POST /api/write-openspool

Write OpenSpool NDEF JSON data to the current tag.

### POST /api/register-uid

Register the current tag's UID in Spoolman with material/manufacturer/color metadata. Creates vendor, filament, and spool entries.

### GET /api/spoolman/spools

Proxy to Spoolman — returns all active (unarchived) spools. Used by the spool picker on writer pages.

### POST /api/spoolman/link

Link or re-assign an NFC tag UID to a Spoolman spool.

```json
{ "spool_id": 42, "nfc_id": "04A651AD8F6180", "old_spool_id": 10 }
```

### GET /api/spoolman/find-vendor

Search for a Spoolman vendor by name (case-insensitive).

```
GET /api/spoolman/find-vendor?name=Sunlu
```

Returns `{"found": true, "id": 1, "name": "Sunlu"}` or `{"found": false}`.

### GET /api/spoolman/find-filament

Search for a Spoolman filament by vendor, material, and optional color.

```
GET /api/spoolman/find-filament?vendor_id=1&material=PLA&color_hex=FF0000
```

Returns `{"found": true, "id": 68, "name": "PLA", "material": "PLA", "color_hex": "FF0000"}` or `{"found": false}`.

### POST /api/spoolman/save-enrichment

Save enrichment data to Spoolman after writing a tag. Creates or updates the vendor → filament → spool hierarchy.

```json
{
  "uid": "04A651AD8F6180",
  "manufacturer": "Sunlu",
  "material": "PLA",
  "color_hex": "FF0000",
  "remaining_g": 850,
  "bed_temp": 60,
  "nozzle_temp": 210,
  "diameter_mm": 1.75,
  "density": 1.24,
  "vendor_id": -1,
  "filament_id": -1
}
```

`vendor_id` and `filament_id` can be set to a positive ID (use existing), `-1` (search and match), or `-2` (user declined match, create new).

Returns `{"success": true, "spool_id": 42, "filament_id": 68}`.

## Web UI Pages

| Path | Description |
|------|-------------|
| `/` | Landing page with device info |
| `/reader` | Tag reader (auto-detect format, Spoolman enrichment badges) |
| `/writer/openprinttag` | OpenPrintTag writer |
| `/writer/tigertag` | TigerTag writer |
| `/writer/opentag3d` | OpenTag3D writer |
| `/writer/openspool` | OpenSpool writer |
| `/register/uid` | NFC+ UID registration in Spoolman |
| `/config` | Device configuration |
| `/update` | OTA firmware update |
| `/troubleshooting` | Diagnostics and troubleshooting |
