# Choose Your Board

SpoolSense supports two ESP32 board variants.

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

## ESP32-S3-Zero (Waveshare)

A compact option with USB-C and onboard RGB LED.

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

## Which Should I Choose?

| If you want... | Choose |
|----------------|--------|
| Simplest build, most GPIO | **ESP32-WROOM** |
| LCD + keypad + NFC reader | **ESP32-WROOM** |
| Smallest possible scanner | **ESP32-S3-Zero** |
| Onboard LED (no wiring) | **ESP32-S3-Zero** |

!!! note
    Both boards run the same firmware binary. The installer handles board selection automatically.
