# API Reference

The scanner exposes a REST API at `http://spoolsense.local` (port 80).

## Endpoints

### GET /api/status

Returns current scanner state and scanned spool info.

```json
{
  "device_id": "ecb008",
  "firmware_version": "1.5.9",
  "present": true,
  "uid": "04A651AD8F6180",
  "tag_data_valid": true,
  "tag_kind": "TigerTag",
  "material_name": "PLA",
  "manufacturer": "Sunlu",
  "color": "#FF0000",
  "remaining_g": 850,
  "spoolman_id": 42
}
```

### GET /api/diagnostics

Returns device health info.

```json
{
  "device_id": "ecb008",
  "firmware_version": "1.5.9",
  "wifi": { "connected": true, "ssid": "MyNetwork", "rssi_dbm": -45, "ip": "192.168.1.100" },
  "mqtt": { "enabled": true, "broker": "192.168.1.50", "connected": true },
  "spoolman": { "enabled": true, "url": "http://192.168.1.100:7912", "reachable": true },
  "nfc": { "ok": true, "reader": "PN5180 v3.4" },
  "memory": { "free_bytes": 180000, "total_bytes": 303000 }
}
```

### GET /api/config

Returns current configuration (passwords masked).

### POST /api/config

Updates configuration and reboots. Accepts JSON body with the same fields as GET.

### POST /api/write-tag

Write OpenPrintTag data to the current tag. See the web writer UI at `/writer/openprinttag`.

### POST /api/write-tigertag

Write TigerTag binary data to the current tag. See the web writer UI at `/writer/tigertag`.

### POST /api/register-uid

Register the current tag's UID in Spoolman with material/manufacturer/color metadata.

## Web UI Pages

| Path | Description |
|------|-------------|
| `/` | Landing page with device info |
| `/reader` | Tag reader (auto-detect format, display all data) |
| `/writer/openprinttag` | OpenPrintTag writer |
| `/writer/tigertag` | TigerTag writer |
| `/writer/opentag3d` | OpenTag3D writer |
| `/config` | Device configuration |
| `/update` | OTA firmware update |
