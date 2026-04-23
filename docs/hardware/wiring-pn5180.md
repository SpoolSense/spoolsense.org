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
| 5V | 5V | Power |
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
| 5V | 5V | Power |
| GND | GND | Ground |

## ESP32-S3-DevKitC Pinout

The S3-DevKitC uses SPI2 (FSPI) for the PN5180, leaving SPI3 free for the TFT display.

| PN5180 Pin | ESP32-S3 GPIO | Function |
|------------|--------------|----------|
| RST | GPIO 7 | Hardware reset (active low) |
| NSS | GPIO 10 | SPI chip select |
| MOSI | GPIO 11 | SPI data out (FSPID) |
| MISO | GPIO 9 | SPI data in (FSPIQ) |
| SCK | GPIO 12 | SPI clock (FSPICLK) |
| BUSY | GPIO 6 | Busy signal (input) |
| 5V | 5V | Power |
| GND | GND | Ground |

## ESP32-C3 SuperMini Pinout

The C3 has a single usable SPI controller, so this variant is scoped to the NFC reader + I2C LCD + WS2812 status LED. TFT and keypad are **not supported** on the C3.

| PN5180 Pin | ESP32-C3 GPIO | Function |
|------------|--------------|----------|
| RST | GPIO 0 | Hardware reset (active low) |
| NSS | GPIO 7 | SPI chip select |
| MOSI | GPIO 6 | SPI data out |
| MISO | GPIO 5 | SPI data in |
| SCK | GPIO 4 | SPI clock |
| BUSY | GPIO 3 | Busy signal (input) |
| IRQ | GPIO 10 | Interrupt (optional) |
| 5V | 5V | Power |
| GND | GND | Ground |

!!! warning "Do **not** use GPIO 2 for PN5180 RST"
    GPIO 2 is a boot strap pin on the C3 and must be HIGH at power-on to boot from flash. Some PN5180 modules drive RST LOW before firmware initializes, which would force the C3 into download mode. `PIN_PN5180_RST` is wired to GPIO 0 (non-strap) specifically to avoid this. If you swap in a different module or re-wire the reader yourself, keep RST off GPIO 2.

!!! note "Strap pins to be aware of on the C3"
    GPIO 8 and 9 are also strap pins (they also happen to be LCD I2C lines — see [LCD wiring](lcd.md)). A standard PCF8574 I2C backpack provides external pull-ups that idle both lines HIGH, which satisfies the POR requirement. Don't wire anything to GPIO 8/9 that can drive them LOW before firmware finishes startup.

## Wiring Tips

!!! note "PN5180 uses 5V power"
    Connect 5V to the ESP32's 5V pin (not 3.3V). The SPI data lines are 3.3V logic and are level-shifted on the PN5180 board.

- Keep SPI wires short (under 15cm) for reliable communication
- The AITRIP PN5180 module has labeled pins matching the table above
- GPIO/IRQ/AUX pins are defined in firmware but not currently required for basic operation

!!! warning "Using a different board or module?"
    The pinout and voltage above are verified for the Freenove ESP32-WROOM and AITRIP PN5180. If you're using a different ESP32 or PN5180 module, check the datasheet for your specific boards to confirm pin labels and voltage levels before wiring.

<!-- TODO: Add wiring diagram photo -->
