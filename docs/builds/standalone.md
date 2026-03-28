# Build Guide: Standalone Scanner

A standalone scanner sits on your desk or mounts to a wall. Place a spool on the NFC reader to scan it.

## Parts Needed

- ESP32 board (WROOM or S3-Zero)
- NFC reader (PN5180 or PN532)
- USB cable for power
- (Optional) LCD, LED, keypad

## Assembly

<!-- TODO: Add step-by-step photos with CAD model renders -->

!!! info "Enclosure designs needed"
    As of March 2026 only a few printable cases exist. We need the community's help! If you design one, see [Contributing > Enclosure Design](../contributing/enclosure-design.md) for how to submit it.

### Basic Build (No Enclosure)

1. Wire the NFC reader to the ESP32 per the [wiring guide](../hardware/wiring-pn5180.md)
2. Connect optional peripherals (LCD, LED, keypad)
3. Flash firmware via the [installer](../installation/firmware.md)
4. Power via USB

The NFC reader antenna should face upward so you can place spools on top of it.

### With Enclosure

Enclosure designs will be linked here as they become available. The ideal standalone case:

- Flat top surface for placing spool tags
- PN5180/PN532 antenna close to the surface
- USB port accessible for power
- Optional cutouts for LCD and keypad
