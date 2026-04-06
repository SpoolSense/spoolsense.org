# Optional - LCD Display

A 16x2 I2C LCD shows spool info directly on the scanner.

## What You Need

- 16x2 character LCD with I2C backpack (address 0x27)
- 2 jumper wires (SDA, SCL) + VCC + GND

## Wiring

| LCD Pin | ESP32-WROOM | ESP32-S3-Zero | ESP32-S3-DevKitC |
|---------|------------|---------------|-----------------|
| SDA | GPIO 23 | GPIO 1 | GPIO 17 |
| SCL | GPIO 22 | GPIO 2 | GPIO 18 |
| VCC | 5V or 3.3V | 5V or 3.3V | 5V or 3.3V |
| GND | GND | GND | GND |

!!! note
    Most I2C LCD backpacks work with both 3.3V and 5V. If the display is dim on 3.3V, try 5V for the backlight.

## What It Shows

- **Boot:** "SpoolSense" + firmware version
- **WiFi:** Connection status and IP address
- **Spool scanned:** Material type, remaining weight
- **Keypad entry:** Tool number being typed
- **Write operations:** Progress and result

## Enable in Firmware

The LCD is enabled via the installer or web config page (`/config` > Hardware > LCD Display toggle). No recompile needed.

<!-- TODO: Add photo of LCD showing spool info -->
