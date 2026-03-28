# Community Mods

Community-contributed enclosures, mounts, and modifications for the SpoolSense scanner. These live in the [`usermods/`](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods) directory of the scanner repo.

## Available Mods

### Jim's Scanner Enclosure v2

A 3-piece printable enclosure for the **ESP32-S3-Zero + PN5180** configuration.

| File | Description |
|------|-------------|
| `enclosure_2-BASE.stl` | Bottom piece |
| `enclosure_2-MIDDLE.stl` | Middle section |
| `enclosure_2-INSET_A.stl` | Inset panel |

**Print settings:** PLA or PETG, 0.2mm layer height, 15-20% infill

[:material-download: Download STLs from GitHub](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods/jims_enclosure){ .md-button }

---

### LinuxGangster's Scanner Mods

#### Standalone Case (ESP32-S3-Zero + PN532)

A two-piece case (body + lid) for the ESP32-S3-Zero with PN532 NFC reader.

| File | Description |
|------|-------------|
| `nfc-case-esp32s3zero.3mf` | Main case body |
| `NFC+Reader+Lid+v1.0.stl` | Lid |

**Print settings:** PLA or PETG, 0.2mm layer height, 15-20% infill

#### BoxTurtle PN532 Tray

A modified BoxTurtle tray that holds a PN532 reader alongside an AFC lane. For multi-scanner setups with per-lane spool detection.

| File | Description |
|------|-------------|
| `tray_plain_pn532.stl` | Modified BoxTurtle tray for PN532 |

[:material-download: Download from GitHub](https://github.com/SpoolSense/spoolsense_scanner/tree/main/usermods/linuxgangster){ .md-button }

---

## Submit Your Own

Have a case, mount, or mod? Add it to the scanner repo's `usermods/` directory:

1. Fork [spoolsense_scanner](https://github.com/SpoolSense/spoolsense_scanner)
2. Create a directory under `usermods/` (e.g., `usermods/my_mount/`)
3. Include:
    - STL/3MF/STEP files
    - A `README.md` with description, print settings, and photos
4. Open a PR targeting `main`

Each contributor owns and maintains their mod. See the [usermods README](https://github.com/SpoolSense/spoolsense_scanner/blob/main/usermods/README.md) for details.
