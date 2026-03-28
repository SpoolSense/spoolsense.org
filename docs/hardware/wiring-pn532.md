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

## Wiring Tips

!!! warning "Use 3.3V, not 5V"
    The ESP32 is 3.3V logic. Connecting VCC to 5V may damage the board.

- Keep SPI wires short (under 15cm)
- The IRQ pin is optional (not currently used by the firmware)
- Make sure the DIP switch is set to SPI mode before powering on

<!-- TODO: Add wiring diagram photo -->
