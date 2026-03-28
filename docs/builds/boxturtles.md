# Build Guide: BoxTurtle AFC Mount

Mount the NFC reader alongside an AFC BoxTurtle lane so spools are scanned automatically when loaded.

## Concept

The NFC antenna is positioned next to the BoxTurtle lane entry. When a spool with an NFC tag is loaded into the lane, the scanner reads the tag and assigns it to that lane via the middleware.

## Parts Needed

- ESP32 board (WROOM or S3-Zero)
- NFC reader (PN5180 recommended for best range)
- Mounting hardware (bracket or tray)
- USB cable for power

## Middleware Configuration

For AFC lane scanning, the middleware config should use `afc_stage` or `afc_lane` action:

```yaml
scanners:
  your_scanner_id:
    action: afc_stage
    publish_lane_data: true
```

See [Middleware Setup](../installation/middleware.md) for full configuration.

<!-- TODO: Add CAD renders and assembly photos -->
<!-- TODO: Add wiring diagram for BoxTurtle mounting -->

!!! info "Mount designs needed"
    No BoxTurtle mount designs exist yet. If you design one, see [Contributing > Enclosure Design](../contributing/enclosure-design.md).
