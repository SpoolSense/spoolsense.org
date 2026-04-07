# Build Guide: Standalone Scanner

## Parts Needed

- ESP32 board (WROOM, S3-Zero, or S3-DevKitC)
- NFC reader (PN5180 or PN532)
- USB cable for power
- (Optional) LCD, LED, keypad

## Assembly

<!-- TODO: Add step-by-step photos with CAD model renders -->

!!! info "Enclosure designs needed"
    As of March 2026 only a few printable cases exist. We need the community's help! If you design one, see [Contributing > Enclosure Design](../contributing/enclosure-design.md) for how to submit it.

### Basic Build

1. Wire the NFC reader to the ESP32 per the [wiring guide](../hardware/wiring-pn5180.md)
2. Connect optional peripherals (LCD, LED, keypad)
3. Flash firmware via the [installer](../installation/firmware.md)
4. Power via USB

The NFC reader antenna should face upward so you can place spools on top of it.

### With Enclosure

| Enclosure | Hardware | Author |
|-----------|----------|--------|
| [Jim's Enclosure v2](community-mods.md#jims-scanner-enclosure-v2) | ESP32-S3-Zero + PN5180 | Jim |
| [LinuxGangster's Case](community-mods.md#standalone-case-esp32-s3-zero-pn532) | ESP32-S3-Zero + PN532 | LinuxGangster |

See [Community Mods](community-mods.md) for all available designs, or [contribute your own](../contributing/enclosure-design.md).
