# Choose Your NFC Reader

SpoolSense supports two NFC reader modules. Both connect via SPI and share the same wiring pins.

## PN5180 (Recommended)

| Spec | Value |
|------|-------|
| Protocols | ISO15693 + ISO14443A |
| Tags Supported | All (ICODE SLIX2, NTAG, MIFARE Classic) |
| Interface | SPI |
| OpenPrintTag on SLIX2 | Yes |
| Price | ~$4-6 |

**Pros:**

- Supports all NFC tag formats including ISO15693 (ICODE SLIX2)
- Full OpenPrintTag compatibility
- Better RF sensitivity and range

**Cons:**

- Slightly more expensive
- Larger module for cheap clones (Elechouse makes a compact PN5180 board closer to the PN532 size, but it's harder to find and more expensive)

## PN532

| Spec | Value |
|------|-------|
| Protocols | ISO14443A only |
| Tags Supported | NTAG, MIFARE Classic (no ISO15693) |
| Interface | SPI (must set DIP switch) |
| OpenPrintTag on SLIX2 | No |
| Price | ~$3-4 |

**Pros:**

- Cheaper and more widely available
- Works great with NFC+ (NTAG + Spoolman), TigerTag, and OpenTag3D
- Smaller module

**Cons:**

- No ISO15693 support (cannot read ICODE SLIX2 OpenPrintTag tags)
- May need SPI clock reduction on some ESP32 boards

## Which Should I Choose?

| If you... | Choose |
|-----------|--------|
| Want maximum tag compatibility | **PN5180** |
| Only use NFC+/TigerTag/OpenTag3D tags | **PN532** |
| Have OpenPrintTag SLIX2 tags | **PN5180** (required) |
| Want the cheapest option | **PN532** |

!!! important
    The NFC reader is selected at install time via the installer. Both readers are compiled into the same firmware binary. You choose which one to use during setup, and can switch later via the web config page.
