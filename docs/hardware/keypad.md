# Optional - Keypad

A 3x4 matrix keypad enables scan-to-tool assignment for toolchanger and multi-tool setups.

## What You Need

- 3x4 membrane matrix keypad ([this one on Amazon](https://www.amazon.com/dp/B0DZ26VVR7) is confirmed working)
- 7 jumper wires

## Wiring

The keypad ribbon cable has 7 pins. The labels printed on the ribbon (left to right) are:

```
C2  R1  C1  R4  C3  R3  R2
```

Wire each pin by its label, not its position on the ribbon.

### ESP32-WROOM

| Ribbon Label | ESP32 GPIO | Function |
|-------------|-----------|----------|
| R1 | GPIO 19 | Row 1 |
| R2 | GPIO 15 | Row 2 |
| R3 | GPIO 16 | Row 3 |
| R4 | GPIO 5 | Row 4 |
| C1 | GPIO 18 | Column 1 |
| C2 | GPIO 21 | Column 2 |
| C3 | GPIO 17 | Column 3 |

!!! warning "Pin assignments changed in v1.6.4"
    If you wired your keypad before v1.6.4, you'll need to re-wire to match the new pin assignments above. The pins were rearranged to match the physical order of the keypad ribbon connector.

### ESP32-S3-Zero

| Ribbon Label | ESP32-S3 GPIO | Function |
|-------------|--------------|----------|
| R1 | GPIO 38 | Row 1 |
| R2 | GPIO 39 | Row 2 |
| R3 | GPIO 40 | Row 3 |
| R4 | GPIO 41 | Row 4 |
| C1 | GPIO 17 | Column 1 |
| C2 | GPIO 18 | Column 2 |
| C3 | GPIO 42 | Column 3 |

### ESP32-S3-DevKitC

| Ribbon Label | ESP32-S3 GPIO | Function |
|-------------|--------------|----------|
| R1 | GPIO 39 | Row 1 |
| R2 | GPIO 40 | Row 2 |
| R3 | GPIO 41 | Row 3 |
| R4 | GPIO 42 | Row 4 |
| C1 | GPIO 47 | Column 1 |
| C2 | GPIO 1 | Column 2 |
| C3 | GPIO 8 | Column 3 |

!!! tip
    The ribbon labels are not in order (C2, R1, C1, R4, C3, R3, R2). Match each label to the correct GPIO — don't wire them left to right.

!!! warning
    Using LCD + keypad together on the ESP32-S3-Zero is not recommended due to limited GPIO...it can be done but soldering to the exposed pins on the back is tough. Use the WROOM or S3-DevKitC board for LCD + keypad builds.

## How It Works

1. Scan a spool on the NFC reader
2. Type the tool number on the keypad (e.g. `12` for T12)
3. Press `#` to confirm
4. Scanner sends `ASSIGN_SPOOL TOOL=T12` to Moonraker
5. Press `*` to clear and start over

## Requirements

- **Moonraker URL** must be configured (web config or installer)
- **ASSIGN_SPOOL** macro must be defined in your Klipper config (see [Klipper Macros](../installation/middleware.md#klipper-macros))
- Keypad enabled via installer or web config (`/config` > Hardware > Keypad toggle)

<!-- TODO: Add photo of keypad wired to ESP32 -->
