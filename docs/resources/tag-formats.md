# Tag Formats

Technical details on the NFC tag formats supported by SpoolSense.

## OpenPrintTag

An open specification for storing filament data on NFC tags using CBOR encoding over NDEF.

- **Spec:** [openprinttag.org](https://openprinttag.org)
- **Tag types:** ICODE SLIX2 (ISO15693) or NTAG213/215/216 (ISO14443A)
- **Encoding:** CBOR in NDEF MIME record (`application/vnd.openprinttag`)
- **Data:** Material type, color, weight, manufacturer, density, diameter, temperatures, brand

SpoolSense implements full read and write support via the `openprinttag_lib` C library.

## TigerTag

A community-driven binary format for NTAG213/215/216 tags.

- **Spec:** [TigerTag RFID Guide](https://github.com/TigerTag-Project/TigerTag-RFID-Guide)
- **Tag types:** NTAG213/215/216 (ISO14443A)
- **Encoding:** Fixed 40-byte binary layout starting at page 4
- **Data:** Material ID, brand ID, color RGBA, weight, nozzle/bed temps, diameter

Material IDs map to a community-maintained database with ~100 materials.

## OpenTag3D

A newer format using CBOR encoding on NTAG tags.

- **Tag types:** NTAG215/216 (ISO14443A)
- **Encoding:** CBOR
- **Data:** Base material, modifiers, manufacturer, color, weight, density, temps

SpoolSense supports full read and write for OpenTag3D via the web UI at `/writer/opentag3d`.

## NFC+

SpoolSense's term for a plain NTAG tag paired with a Spoolman entry. The tag stores only a UID. All spool data (material, color, weight, manufacturer) lives in Spoolman and is linked via the `nfc_id` extra field.

- **Tag types:** NTAG213/215/216 (ISO14443A)
- **Data on tag:** None (UID only)
- **Data source:** Spoolman database

This is the simplest and cheapest approach. Buy NTAG stickers, register UIDs in Spoolman.

## Bambu Lab

MIFARE Classic tags used by Bambu Lab printers. The tag data is encrypted with manufacturer keys.

- **Tag types:** MIFARE Classic 1K (ISO14443A)
- **SpoolSense support:** UID read only (encrypted data not accessible)
- **SAK:** 0x08 (used for auto-detection)
