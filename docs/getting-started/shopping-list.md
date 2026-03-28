# Shopping List

Everything you need to build a SpoolSense scanner. Total cost: **$10-20** depending on options.

## Required

| Component | Options | Est. Cost |
|-----------|---------|-----------|
| **ESP32 Board** | ESP32-WROOM DevKit (recommended) or ESP32-S3-Zero | $3-8 |
| **NFC Reader** | PN5180 (recommended) or PN532 | $3-6 |
| **NFC Tags** | NTAG215 stickers (pack of 50) | $5-8 |
| **Jumper Wires** | Female-to-female dupont wires (6-8 needed) | $2 |
| **USB Cable** | Micro-USB (WROOM) or USB-C (S3-Zero) | $1 |

## Optional

| Component | Purpose | Est. Cost |
|-----------|---------|-----------|
| **16x2 I2C LCD** | Display spool info on the scanner | $3-5 |
| **SK6812 RGBW LED** | Status indicator (WROOM only, S3 has onboard LED) | $1 |
| **3x4 Matrix Keypad** | Assign spools to tools by number | $2-3 |

## Where to Buy

All components are widely available on Amazon, AliExpress, and electronics suppliers. Search for:

- "ESP32 WROOM DevKit" or "ESP32-S3-Zero Waveshare"
- "PN5180 NFC module" or "PN532 NFC module"
- "NTAG215 NFC stickers"
- "16x2 I2C LCD 0x27"
- "3x4 membrane keypad Arduino"

!!! tip
    Buy the **AITRIP PN5180** module specifically. It has the correct pinout and antenna for spool scanning. Generic PN5180 breakout boards may have different pin layouts.
