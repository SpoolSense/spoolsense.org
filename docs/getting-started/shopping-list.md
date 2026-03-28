# Shopping List

Everything you need to build a SpoolSense scanner. Total cost: **$10-20** depending on options.

## Required

| Component | Options | Est. Cost |
|-----------|---------|-----------|
| **ESP32 Board** | ESP32-WROOM DevKit (if you want extras like LCD/keypad) or ESP32-S3-Zero (smaller builds) | $3-8 |
| **NFC Reader** | PN5180 (recommended, all tag formats) or PN532 (ISO14443A only, SLIX2/OpenPrintTag not supported) | $3-6 |
| **NFC Tags** | NTAG215 stickers for UID-based tracking, or ICODE SLIX2 for OpenPrintTag (PN5180 required) | $5-8 |
| **Jumper Wires** | Female-to-female dupont wires (8 minimum, more if adding extras) | $2 |
| **USB Cable** | USB-C (most boards) or Micro-USB (older WROOM variants) | $1 |

## Optional

| Component | Use Case | Est. Cost |
|-----------|----------|-----------|
| **16x2 I2C LCD** | Display spool info directly on the scanner | $3-5 |
| **SK6812 RGBW LED** | Status indicator (WROOM only, S3 has onboard LED) | $1 |
| **3x4 Matrix Keypad** | Toolchanger users: assign spools to tools by typing the tool number | $2-3 |

## NFC Tags: Which to Buy?

| Tag Type | Best For | Works With | Price |
|----------|----------|------------|-------|
| **NTAG215 stickers** | Most users. Register UID in Spoolman, write TigerTag or OpenTag3D format, or use UID-only. | PN5180 and PN532 | ~$0.10 each |
| **ICODE SLIX2** | OpenPrintTag format with full CBOR filament data on the tag | PN5180 only | ~$0.30 each |

!!! tip
    Start with **NTAG215 stickers**. They're cheap, work with both readers, and cover the most common workflow (UID lookup in Spoolman). You can always add SLIX2 tags later if you want OpenPrintTag.

## Where to Buy

All components are widely available on Amazon, AliExpress, and electronics suppliers. Search for:

- "ESP32 WROOM DevKit" or "ESP32-S3-Zero Waveshare"
- "AITRIP PN5180 NFC module" or "PN532 NFC module SPI"
- "NTAG215 NFC stickers" or "ICODE SLIX2 NFC tags"
- "16x2 I2C LCD 0x27"
- "3x4 membrane keypad Arduino"

!!! tip
    Buy the **AITRIP PN5180** module specifically. It has the correct pinout and antenna for spool scanning. Generic PN5180 breakout boards may have different pin layouts.
