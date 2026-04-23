# Choose Your Board

SpoolSense supports four ESP32 board variants.

## ESP32-WROOM DevKit (Recommended)

The most common and beginner-friendly option.

| Spec | Value |
|------|-------|
| Board | Freenove ESP32-WROOM (recommended) |
| Chip | ESP32-D0WD-V3, dual-core 240MHz |
| Flash | 4MB |
| USB | USB-C |
| GPIO | 30+ available pins |
| Status LED | External SK6812 RGBW (optional wiring) |
| Price | ~$3-5 (2-pack available) |

**Pros:**

- Widely available, well-documented
- Plenty of GPIO for all peripherals (LCD + keypad + NFC reader)
- Cheaper

**Cons:**

- Larger form factor
- No onboard RGB LED

## ESP32-S3-Zero / S3-Zero-M (Waveshare)

A compact option with USB-C and onboard RGB LED. Available in two variants:

- **ESP32-S3-Zero** — requires soldering pin headers yourself
- **ESP32-S3-Zero-M** — comes with pre-soldered pin headers (ready to use with dupont wires)

| Spec | Value |
|------|-------|
| Chip | ESP32-S3 |
| Flash | 4MB |
| USB | USB-C |
| GPIO | Limited (USB uses GPIO 19/20) |
| Status LED | Onboard WS2812 RGB (no wiring needed) |
| Price | ~$6-8 |

**Pros:**

- Very small form factor
- Onboard RGB LED (always available, no wiring)

**Cons:**

- Fewer available GPIO pins
- LCD + keypad together is not recommended (pin conflicts)
- Slightly more expensive

## ESP32-S3-DevKitC-1 (Espressif)

A full-featured development board with 16MB flash and 8MB PSRAM. Best for builds that want separate SPI buses for NFC and TFT.

| Spec | Value |
|------|-------|
| Board | ESP32-S3-DevKitC-1-N16R8 |
| Chip | ESP32-S3, dual-core 240MHz |
| Flash | 16MB |
| PSRAM | 8MB (octal SPI) |
| USB | USB-C (two ports: UART + USB) |
| GPIO | 30+ available pins |
| Status LED | Onboard WS2812 RGB on GPIO 48 |
| Price | ~$8-12 |

**Pros:**

- 16MB flash + 8MB PSRAM for future features
- Separate SPI buses — PN5180 on SPI2, TFT on SPI3 (no bus contention)
- Onboard RGB LED (no external wiring)
- Plenty of GPIO for all peripherals

**Cons:**

- Larger than S3-Zero
- Two USB ports can be confusing (use the UART port for serial output)

## ESP32-C3 SuperMini

A tiny, inexpensive RISC-V board (HORNAXYS, Waveshare, and other clones). Scoped build: NFC reader + I2C LCD + WS2812 status LED only.

| Spec | Value |
|------|-------|
| Chip | ESP32-C3 (single-core RISC-V, 160 MHz) |
| Flash | 4MB |
| PSRAM | None |
| USB | USB-C (native USB-Serial/JTAG) |
| GPIO | 13 exposed pins (0–10, 20, 21) |
| Status LED | External WS2812 (onboard LED is single-color blue, not RGB) |
| Price | ~$2-4 |

**Pros:**

- Cheapest option by a wide margin
- Very small footprint (about the size of a US quarter)
- No soldering required on most clones (pre-tinned headers)

**Cons:**

- **No TFT support** — single usable SPI controller is dedicated to the NFC reader
- **No keypad support** — pin budget too tight
- No PSRAM, less flash headroom for future features
- Strap pins on GPIO 2, 8, 9 require care when wiring (see [PN5180 wiring](../hardware/wiring-pn5180.md))

## Which Should I Choose?

| If you want... | Choose |
|----------------|--------|
| Cheapest build, smallest footprint | **ESP32-C3 SuperMini** |
| Simplest build, most GPIO | **ESP32-WROOM** |
| LCD + keypad + NFC reader | **ESP32-WROOM** |
| Smallest possible scanner (LCD + NFC) | **ESP32-S3-Zero** or **ESP32-C3** |
| Onboard LED (no wiring) | **ESP32-S3-Zero** or **S3-DevKitC** |
| PN5180 + TFT on separate SPI buses | **ESP32-S3-DevKitC** |
| Most flash + PSRAM | **ESP32-S3-DevKitC** |

!!! note
    Each board has its own firmware binary. The installer and web flasher handle board selection automatically.

!!! tip "Have a different ESP32?"
    If you have a different ESP32 board and want it supported, [open an issue](https://github.com/SpoolSense/spoolsense_scanner/issues) with the board name and specs. Most ESP32 variants can be added with just a pin mapping update.
