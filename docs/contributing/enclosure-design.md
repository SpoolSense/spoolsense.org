# Enclosure Design

We need printable cases for the SpoolSense scanner. If you have CAD skills, your contribution would be a huge help to the community.

## Designs Needed

### Standalone Scanner Case (ESP32-WROOM + PN5180)

- Wall or desk mount
- Flat NFC placement area on top (PN5180 antenna close to surface)
- USB port accessible for power
- Optional cutouts for 16x2 LCD and keypad

### Standalone Scanner Case (ESP32-S3-Zero + PN5180)

- Same requirements, smaller footprint
- No LCD cutout needed (S3 + LCD is uncommon)

### Standalone Scanner Case (ESP32-WROOM + PN532)

- Similar to PN5180 case but sized for PN532 module

### BoxTurtle AFC Lane Mount

- Bracket or tray that positions the NFC antenna alongside a BoxTurtle lane
- Antenna should face the spool tag position when a spool is loaded
- Must not interfere with filament path

## Design Guidelines

- **Material:** PLA or PETG, 0.2mm layer height, 15-20% infill
- **Antenna placement:** NFC reader antenna as close to the scanning surface as possible. Avoid thick plastic layers between the antenna and the tag.
- **Mounting:** Consider both desk-standing and wall-mount options
- **Access:** USB port must be accessible. Consider a slot rather than a hole for easy cable insertion.
- **Ventilation:** The ESP32 runs warm. A few vent slots are helpful.

## How to Submit

1. Fork the [spoolsense.org](https://github.com/SpoolSense/spoolsense.org) repo
2. Add your design files to `docs/builds/cad/your-design-name/`
3. Include:
    - STL files (oriented for printing)
    - STEP file (for remixing)
    - Photos of the printed result
    - Print settings and assembly notes
4. Open a PR targeting `main`

Or if you prefer, [open an issue](https://github.com/SpoolSense/spoolsense_scanner/issues) with photos and download links.
