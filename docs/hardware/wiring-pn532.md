# Wiring - ESP32 + PN532

The PN532 connects to the ESP32 via SPI. It shares the same SPI pins as the PN5180 (only one reader is active at runtime).

## Before Wiring: Set SPI Mode

The PN532 module has a mode selector (DIP switch or solder jumpers). Set it to **SPI mode** before wiring:

| Mode | SEL0 | SEL1 |
|------|------|------|
| **SPI** | **0** | **1** |
| I2C | 1 | 0 |
| UART | 0 | 0 |

Check your specific board. Some have a 2-position DIP switch, others have solder pads.

## ESP32-WROOM Pinout

| PN532 Pin | ESP32 GPIO | Function |
|-----------|-----------|----------|
| SCK | GPIO 25 | SPI clock |
| MOSI | GPIO 27 | SPI data out |
| MISO | GPIO 26 | SPI data in |
| SS (CS) | GPIO 14 | SPI chip select |
| RST | GPIO 13 | Reset |
| VCC | 3.3V | Power |
| GND | GND | Ground |

## ESP32-S3-Zero Pinout

| PN532 Pin | ESP32-S3 GPIO | Function |
|-----------|--------------|----------|
| SCK | GPIO 8 | SPI clock |
| MOSI | GPIO 6 | SPI data out |
| MISO | GPIO 7 | SPI data in |
| SS (CS) | GPIO 5 | SPI chip select |
| RST | GPIO 4 | Reset |
| VCC | 3.3V | Power |
| GND | GND | Ground |

## ESP32-S3-DevKitC Pinout

Shares the same SPI2 pins as the PN5180 on the S3-DevKitC.

| PN532 Pin | ESP32-S3 GPIO | Function |
|-----------|--------------|----------|
| SCK | GPIO 12 | SPI clock |
| MOSI | GPIO 11 | SPI data out |
| MISO | GPIO 9 | SPI data in |
| SS (CS) | GPIO 10 | SPI chip select |
| RST | GPIO 7 | Reset |
| VCC | 3.3V | Power |
| GND | GND | Ground |

## ESP32-C3 SuperMini Pinout

The PN532 shares the same SPI pins as the PN5180 on the C3 — only one reader is active at runtime. The C3 variant is scoped to NFC reader + I2C LCD + WS2812 only (no TFT or keypad).

| PN532 Pin | ESP32-C3 GPIO | Function |
|-----------|--------------|----------|
| SCK | GPIO 4 | SPI clock |
| MOSI | GPIO 6 | SPI data out |
| MISO | GPIO 5 | SPI data in |
| SS (CS) | GPIO 7 | SPI chip select |
| RST | GPIO 0 | Reset |
| IRQ | GPIO 10 | Interrupt (optional) |
| VCC | 3.3V | Power |
| GND | GND | Ground |

!!! note "RST on GPIO 0 is intentional"
    GPIO 2 is a C3 boot strap pin. RST is wired to GPIO 0 (non-strap) so the reader cannot hold the board in download mode at power-on.

## Wiring Tips

!!! note "PN532 uses 3.3V power"
    The PN532 module runs on 3.3V. Connect VCC to the ESP32's 3.3V pin (not 5V).

- Keep SPI wires short (under 15cm)
- The IRQ pin is optional (not currently used by the firmware)
- Make sure the DIP switch is set to SPI mode before powering on

<!-- TODO: Add wiring diagram photo -->
