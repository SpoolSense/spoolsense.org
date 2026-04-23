# Optional - LCD Display

A 16x2 I2C LCD shows spool info directly on the scanner.

## What You Need

- 16x2 character LCD with I2C backpack (address 0x27)
- 2 jumper wires (SDA, SCL) + VCC + GND

## Wiring

| LCD Pin | ESP32-WROOM | ESP32-S3-Zero | ESP32-S3-DevKitC | ESP32-C3 |
|---------|------------|---------------|-----------------|----------|
| SDA | GPIO 23 | GPIO 1 | GPIO 17 | GPIO 8 |
| SCL | GPIO 22 | GPIO 2 | GPIO 18 | GPIO 9 |
| VCC | 5V or 3.3V | 5V or 3.3V | 5V or 3.3V | 5V or 3.3V |
| GND | GND | GND | GND | GND |

!!! note
    Most I2C LCD backpacks work with both 3.3V and 5V. If the display is dim on 3.3V, try 5V for the backlight.

!!! warning "ESP32-C3: strap pin requirement"
    GPIO 8 and 9 are boot strap pins on the C3. A standard PCF8574 I2C backpack provides external pull-ups that idle both lines HIGH, which satisfies the POR strap condition. Don't wire devices to GPIO 8 or 9 that can drive either line LOW before firmware finishes startup, or the board will fail to boot from flash.

## What It Shows

- **Boot:** "SpoolSense" + firmware version
- **WiFi:** Connection status and IP address
- **Spool scanned:** Material type, remaining weight
- **Keypad entry:** Tool number being typed
- **Write operations:** Progress and result

## Enable in Firmware

The LCD is enabled via the installer or web config page (`/config` > Hardware > LCD Display toggle). No recompile needed.

<!-- TODO: Add photo of LCD showing spool info -->
