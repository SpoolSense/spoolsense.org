# Wiring - ESP32 + PN5180

The PN5180 connects to the ESP32 via SPI. 8 wires total.

## Recommended module

The [Elechouse PN5180 NFC module](https://www.elechouse.com/product/pn5180-nfc-module/) is the recommended reader: it's a higher-quality build than the common clones, noticeably smaller, and has a cleaner antenna layout. Its header naming differs slightly from the generic boards:

| Elechouse pin | Connect to | Notes |
|---------------|-----------|-------|
| PD/CE | **leave unconnected** | Power-down/chip-enable. The board enables the chip by default; wiring this to a driven GPIO will power-cycle the reader unexpectedly (a chip-select line here caused intermittent NFC failures on our bench) |
| 5V | 5V | Board supply |
| PVDD | leave unconnected | Pad supply — provided on-board |
| GND | GND | Ground |
| NSS / MOSI / MISO / SCK / BUSY / RST | as tables below | SPI logic is **3.3V** (marked `SPI=3V3` on the board) despite the 5V supply |

The separate 4-pin auxiliary header (GPO1, IRQ, AUX1, REQ) is not needed for SpoolSense — leave it unconnected. `REQ` is the firmware-download pin; never tie it high.

!!! info "Remapping pins at runtime — new in v1.8.3+"
    The six NFC reader pins (RST, NSS/SS, BUSY, SCK, MOSI, MISO) can be reassigned from the scanner's config page (`/config` > **NFC Reader Pins (advanced)**) — no recompile needed. Leave a field **blank** to keep that board's default. Invalid or conflicting pins are rejected and revert to the previous value. Changes apply after the post-save restart. The PN532 uses the same slots (its BUSY field is unused). The pinouts below remain the recommended defaults.

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

## ESP32-C6-DevKitC-1 Pinout (shared SPI)

The C6 has one general-purpose SPI controller, shared between the PN5180 and the optional TFT (v1.9.0+). SCK and MOSI fan out to both devices; MISO goes to the PN5180 only (the TFT is write-only).

| PN5180 Pin | ESP32-C6 GPIO | Function |
|------------|--------------|----------|
| RST | GPIO 0 | Hardware reset (active low) |
| NSS | GPIO 10 | SPI chip select — **add a 10kΩ pull-up to 3V3** |
| MOSI | GPIO 7 | SPI data out (shared with TFT SDA) |
| MISO | GPIO 2 | SPI data in (PN5180 only) |
| SCK | GPIO 6 | SPI clock (shared with TFT SCL) |
| BUSY | GPIO 1 | Busy signal (input) |
| 5V | 5V | Power |
| GND | GND | Ground |

IRQ/GPIO/AUX are not connected on this board. If you add the TFT, see [TFT wiring](tft.md) — its CS (GPIO 3) also needs a 10kΩ pull-up so neither device is selected during reset.

!!! note "Strap pins on the C6"
    GPIO 4, 5, 9, and 15 are strap pins and GPIO 12/13 are the native USB pins — the pin map avoids all of them. Keep them free if you re-wire.

## ESP32-C5-DevKitC-1 Pinout (shared SPI)

Same shared-bus layout as the C6, different GPIO numbers. New in v1.9.0.

| PN5180 Pin | ESP32-C5 GPIO | Function |
|------------|--------------|----------|
| RST | GPIO 0 | Hardware reset (active low) |
| NSS | GPIO 10 | SPI chip select — **add a 10kΩ pull-up to 3V3** |
| MOSI | GPIO 8 | SPI data out (shared with TFT SDA) |
| MISO | GPIO 9 | SPI data in (PN5180 only) |
| SCK | GPIO 6 | SPI clock (shared with TFT SCL) |
| BUSY | GPIO 1 | Busy signal (input) |
| 5V | 5V | Power |
| GND | GND | Ground |

!!! warning "The header pin labeled RX is GPIO 12"
    On the C5-DevKitC-1 the UART pins are silkscreened TX/RX (GPIO 11/12), not by GPIO number. Don't wire peripherals to them, and avoid the strap pins (GPIO 2, 3, 7, 25, 26, 28) and the native USB pins (GPIO 13/14).

## Wiring Tips

!!! note "PN5180 uses 5V power"
    Connect 5V to the ESP32's 5V pin (not 3.3V). The SPI data lines are 3.3V logic and are level-shifted on the PN5180 board.

- Keep SPI wires short (under 15cm) for reliable communication
- The AITRIP and Elechouse PN5180 modules have labeled pins matching the tables above (the Elechouse board adds PD/CE and PVDD — see "Recommended module")
- GPIO/IRQ/AUX pins are defined in firmware but not currently required for basic operation

!!! warning "Using a different board or module?"
    The pinout and voltage above are verified for the Freenove ESP32-WROOM and AITRIP PN5180. If you're using a different ESP32 or PN5180 module, check the datasheet for your specific boards to confirm pin labels and voltage levels before wiring.

<!-- TODO: Add wiring diagram photo -->
