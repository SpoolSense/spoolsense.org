# Enclosure Design

Cases and mounts contributed by the community, listed by configuration. Gaps without a case yet are at the bottom.

## Available

### Standalone Scanners

| Configuration | Designer | Notes |
|---|---|---|
| ESP32-S3-Zero + PN5180 | [Jim](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods/jims_enclosure) | 3-piece print |
| ESP32-S3-Zero + PN5180 + 16x2 LCD | [PlasticSnake](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods/plasticsnake) | 4-part with LED diffuser and optional TPU feet. Photos included. |
| ESP32-S3-Zero + PN532 | [LinuxGangster](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods/linuxgangster) | 2-piece case |
| ESP32-S3-SuperMini + PN5180 | [roomonthethird](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods/roomonthethird) | Snap-fit, Fusion 360 source included |
| ESP32-DevKitC (WROOM) or ESP32-S3-Zero | [Poseidon](https://www.printables.com/model/1716784-spoolsense-solo-reader) | Interchangeable mount plates for both boards. Hosted on Printables. |

### MMU / Multi-Lane Mounts

| Configuration | Designer | Notes |
|---|---|---|
| BoxTurtle + PN532 | [LinuxGangster](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods/linuxgangster) | Modified tray |
| ArmoredTurtle + PN532 + ESP32 WROOM | [zoldemberg](https://www.printables.com/model/1699546-armoredturtle-tray-with-pn532-rfid-mount-and-esp32) | Hosted on Printables (CC BY 4.0) |

See [Community Mods](../builds/community-mods.md) for file lists and assembly notes.

## Not Yet Built

These don't have a case yet:

- **Standalone case for ESP32-WROOM with confirmed reader support.** Poseidon's case above supports the WROOM form factor but the reader compatibility (PN5180 vs PN532) isn't explicitly stated. A case with explicit reader-fit confirmation would still be useful.
- **Keypad cutouts (3x4 matrix).** None of the existing cases include a keypad opening. A remix of any standalone case above would work.

## Design Guidelines

- **Material:** PLA or PETG, 0.2mm layer height, 15-20% infill.
- **Antenna placement:** NFC reader antenna as close to the scanning surface as possible. Avoid thick plastic layers between the antenna and the tag.
- **Mounting:** Both desk-standing and wall-mount options where possible.
- **Access:** USB port accessible. A slot is easier to use than a round hole.
- **Ventilation:** A few vent slots — the ESP32 runs warm.
- Several existing cases include CAD source files (STEP, Fusion `.f3d`). Remixing is fine.

## How to Submit

Enclosure designs live in the [`usermods/`](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods) directory of the scanner repo.

1. Fork [spoolsense_scanner](https://github.com/SpoolSense/spoolsense_scanner).
2. Create a directory under `usermods/` (e.g., `usermods/my_enclosure/`).
3. Include:
    - STL files (oriented for printing).
    - STEP or `.f3d` source so others can remix.
    - Photos of the printed result if possible.
    - A `README.md` with print settings and assembly notes.
4. Open a PR targeting `main`.

Each contributor owns and maintains their mod. See the [usermods README](https://github.com/SpoolSense/spoolsense_scanner/blob/main/usermods/README.md) for details.

If you don't want to open a PR, [open an issue](https://github.com/SpoolSense/spoolsense_scanner/issues) with photos and download links instead.
