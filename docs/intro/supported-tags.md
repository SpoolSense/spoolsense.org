# Supported Tags

SpoolSense reads multiple NFC tag formats. Each format stores filament data differently.

## Tag Format Comparison

| Format | Tag Type | Protocol | Data Stored | Write Support |
|--------|----------|----------|-------------|---------------|
| **OpenPrintTag** | ICODE SLIX2 or NTAG | ISO15693 / ISO14443A | Full CBOR filament profile | Yes |
| **TigerTag** | NTAG213/215/216 | ISO14443A | Binary material/color/weight | Yes |
| **OpenTag3D** | NTAG215/216 | ISO14443A | CBOR material/manufacturer/color | Yes |
| **OpenSpool** | NTAG215/216 | ISO14443A | NDEF JSON brand/material/color/temps | Yes |
| **NFC+** | NTAG213/215/216 | ISO14443A | UID only (data in Spoolman) | N/A |
| **Bambu Lab** | MIFARE Classic | ISO14443A | UID only (encrypted) | No |

!!! info "What is NFC+?"
    **NFC+** is SpoolSense's term for a plain NTAG tag paired with a Spoolman entry. The tag itself stores only a UID. All spool data (material, color, weight, manufacturer) lives in Spoolman and is linked to the tag via the `nfc_id` extra field. Scan the tag, Spoolman provides the data. Simple, cheap, and no special tag format required.

## Which Tags Should I Buy?

**For most users: NTAG215 tags (NFC+).**

- Cheap (~$0.10 each)
- Works with both PN5180 and PN532 readers
- Register the UID in Spoolman and you're done
- Can also be written as TigerTag, OpenTag3D, or OpenSpool format for offline use

**If you want rich offline data: TigerTag on NTAG213/215.**

- Material, color, weight, temps stored directly on the tag
- Works without Spoolman
- Community-maintained material database

**If you want OpenPrintTag format: ICODE SLIX2 tags.**

- Full CBOR filament profile stored on tag (material, color, weight, temps, density, diameter)
- Requires PN5180 reader (ISO15693 not supported on PN532)
- ~$0.30 each

**If you have OpenTag3D or OpenSpool tags:**

- Full read and write support for both formats
- Same NTAG215/216 tags used for NFC+ and TigerTag

## NFC Reader Compatibility

| Tag Format | PN5180 | PN532 |
|------------|--------|-------|
| OpenPrintTag (ICODE SLIX2) | Yes | No (ISO15693 not supported) |
| OpenPrintTag (NTAG) | Yes | Yes |
| TigerTag | Yes | Yes |
| OpenTag3D | Yes | Yes |
| OpenSpool | Yes | Yes |
| NFC+ (UID + Spoolman) | Yes | Yes |
| Bambu Lab (UID only) | Yes | Yes |

!!! note
    The PN532 only supports ISO14443A. If you have OpenPrintTag data on ICODE SLIX2 tags (ISO15693), you need the PN5180.
