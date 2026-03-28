# Wiring - ESP32 + PN5180

The PN5180 connects to the ESP32 via SPI. 8 wires total.

## ESP32-WROOM Pinout

| PN5180 Pin | ESP32 GPIO | Function |
|------------|-----------|----------|
| RST | GPIO 13 | Hardware reset (active low) |
| NSS | GPIO 14 | SPI chip select |
| MOSI | GPIO 27 | SPI data out |
| MISO | GPIO 26 | SPI data in |
| SCK | GPIO 25 | SPI clock |
| BUSY | GPIO 33 | Busy signal (input) |
| VCC | 3.3V | Power |
| GND | GND | Ground |

## ESP32-S3-Zero Pinout

| PN5180 Pin | ESP32-S3 GPIO | Function |
|------------|--------------|----------|
| RST | GPIO 4 | Hardware reset (active low) |
| NSS | GPIO 5 | SPI chip select |
| MOSI | GPIO 6 | SPI data out |
| MISO | GPIO 7 | SPI data in |
| SCK | GPIO 8 | SPI clock |
| BUSY | GPIO 9 | Busy signal (input) |
| VCC | 3.3V | Power |
| GND | GND | Ground |

## Wiring Tips

!!! warning "Use 3.3V, not 5V"
    The ESP32 is 3.3V logic. Connecting VCC to 5V may damage the board.

- Keep SPI wires short (under 15cm) for reliable communication
- The AITRIP PN5180 module has labeled pins matching the table above
- GPIO/IRQ/AUX pins are defined in firmware but not currently required for basic operation

<!-- TODO: Add wiring diagram photo -->
