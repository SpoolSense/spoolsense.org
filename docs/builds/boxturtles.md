# Build Guide: BoxTurtle AFC Mount

Mount the NFC reader alongside an AFC BoxTurtle lane so spools are scanned automatically when loaded.

## Concept

The NFC antenna is positioned next to the BoxTurtle lane entry. When a spool with an NFC tag is loaded into the lane, the scanner reads the tag and assigns it to that lane via the middleware.

## Parts Needed

- ESP32 board (WROOM, S3-Zero, or S3-DevKitC)
- NFC reader (PN5180 recommended for best range)
- Mounting hardware (bracket or tray)
- USB cable for power

## Middleware Configuration

The middleware action depends on your scanner setup:

- **One scanner per lane** — use `afc_lane` with a `target` for each lane. The scanner auto-assigns the scanned spool to its lane.
- **One shared scanner** — use `afc_stage`. Scan a spool on the shared scanner, then load it into any lane. The middleware assigns the spool when AFC detects the lane load.

```yaml
# Per-lane scanner example
scanners:
  abc123:
    action: afc_lane
    target: lane1

# Shared scanner example
scanners:
  abc123:
    action: afc_stage
```

See [Middleware Setup](../installation/middleware.md) for full configuration and all action types.

<!-- TODO: Add CAD renders and assembly photos -->
<!-- TODO: Add wiring diagram for BoxTurtle mounting -->

## Available Mounts

| Mount | NFC Reader | Author |
|-------|-----------|--------|
| [PN532 Tray](community-mods.md#boxturtle-pn532-tray) | PN532 | LinuxGangster |

See [Community Mods](community-mods.md) for all designs, or [contribute your own](../contributing/enclosure-design.md).
