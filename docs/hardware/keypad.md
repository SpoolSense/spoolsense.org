# Optional - Keypad

A 3x4 matrix keypad enables scan-to-tool assignment for toolchanger and multi-tool setups.

## What You Need

- 3x4 membrane matrix keypad (standard Arduino type)
- 7 jumper wires

## Wiring

### ESP32-WROOM

| Keypad Pin | ESP32 GPIO | Function |
|------------|-----------|----------|
| ROW1 | GPIO 15 | Row 1 |
| ROW2 | GPIO 16 | Row 2 |
| ROW3 | GPIO 17 | Row 3 |
| ROW4 | GPIO 18 | Row 4 |
| COL1 | GPIO 19 | Column 1 |
| COL2 | GPIO 21 | Column 2 |
| COL3 | GPIO 5 | Column 3 |

### ESP32-S3-Zero

| Keypad Pin | ESP32-S3 GPIO | Function |
|------------|--------------|----------|
| ROW1 | GPIO 38 | Row 1 |
| ROW2 | GPIO 39 | Row 2 |
| ROW3 | GPIO 40 | Row 3 |
| ROW4 | GPIO 41 | Row 4 |
| COL1 | GPIO 17 | Column 1 |
| COL2 | GPIO 18 | Column 2 |
| COL3 | GPIO 42 | Column 3 |

!!! warning
    Using LCD + keypad together on the ESP32-S3-Zero is not recommended due to limited GPIO...it can be done but soldering to the exposed pins on the back is tough. Use the WROOM board for LCD + keypad builds.

## How It Works

1. Scan a spool on the NFC reader
2. Type the tool number on the keypad (e.g. `12` for T12)
3. Press `#` to confirm
4. Scanner sends `ASSIGN_SPOOL TOOL=T12` to Moonraker
5. Press `*` to clear and start over

## Requirements

- **Moonraker URL** must be configured (web config or installer)
- **ASSIGN_SPOOL** macro must be defined in your Klipper config
- Keypad enabled via installer or web config (`/config` > Hardware > Keypad toggle)

<!-- TODO: Add photo of keypad wired to ESP32 -->
