# Supported Tags

SpoolSense reads multiple NFC tag formats. Each format stores filament data differently.

## Tag Format Comparison

| Format | Tag Type | Protocol | Data Stored | Write Support |
|--------|----------|----------|-------------|---------------|
| **OpenPrintTag** | ICODE SLIX2 or NTAG | ISO15693 / ISO14443A | Full CBOR filament profile | Yes |
| **TigerTag** | NTAG213/215/216 | ISO14443A | Binary material/color/weight | Yes |
| **OpenTag3D** | NTAG | ISO14443A | CBOR material/manufacturer/color | Read only |
| **Generic UID** | NTAG215 or similar | ISO14443A | UID only (data in Spoolman) | N/A |
| **Bambu Lab** | MIFARE Classic | ISO14443A | UID only (encrypted) | No |

## Which Tags Should I Buy?

**For most users: NTAG215 tags.**

- Cheap (~$0.10 each)
- Works with both PN5180 and PN532 readers
- Register the UID in Spoolman and you're done
- Can also be written as TigerTag format for offline use

**If you want rich offline data: TigerTag on NTAG213/215.**

- Material, color, weight, temps stored directly on the tag
- Works without Spoolman
- Community-maintained material database

**If you have OpenPrintTag or OpenTag3D tags:**

- Full support for reading and writing (OpenPrintTag)
- OpenTag3D read support
- These are less common but fully supported

## NFC Reader Compatibility

| Tag Format | PN5180 | PN532 |
|------------|--------|-------|
| OpenPrintTag (ICODE SLIX2) | Yes | No (ISO15693 not supported) |
| OpenPrintTag (NTAG) | Yes | Yes |
| TigerTag | Yes | Yes |
| OpenTag3D | Yes | Yes |
| Generic UID | Yes | Yes |
| Bambu Lab (UID only) | Yes | Yes |

!!! note
    The PN532 only supports ISO14443A. If you have OpenPrintTag data on ICODE SLIX2 tags (ISO15693), you need the PN5180.
