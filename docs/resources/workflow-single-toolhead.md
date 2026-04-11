# Workflow — Single Toolhead

Flow diagrams for standard Klipper printers with a single extruder.

---

## Tags That Track Filament Usage

**Supported tags:** OpenPrintTag, OpenTag3D

These tags have writable weight fields. After each print, the `UPDATE_TAG` macro (in your `PRINT_END`) triggers the middleware to calculate filament used from Moonraker's print history. The deduction is sent to the scanner and stored persistently. The next time that spool is scanned, the scanner writes the updated weight to the tag before proceeding with the normal scan flow.

The tag is the source of truth — Spoolman is updated from the tag data on each scan.

**Key points:**

- `UPDATE_TAG` fires automatically at the end of each print
- Deductions accumulate on the scanner if multiple prints occur before the next scan
- The scanner writes the updated weight to the tag on the next scan, then clears the deduction
[![OpenPrintTag / OpenTag3D workflow](diagrams/update-tag-openprinttag.svg)](diagrams/update-tag-openprinttag.svg)

---

## Tags That Do Not Track Filament Usage

**Supported tags:** UID-only (plain NTAG), TigerTag, OpenSpool

These tags have no writable weight field. Filament tracking is handled entirely by Moonraker's built-in Spoolman integration, which syncs usage in real-time during each print.

`UPDATE_TAG` still fires in `PRINT_END` but the middleware detects the tag type and skips — no action is taken. Spoolman already has the correct weight from Moonraker's real-time sync.

**Key points:**

- Requires `[spoolman]` configured in `moonraker.conf` for filament tracking
- SpoolSense only handles the initial scan (telling Moonraker which spool is active)
- Moonraker and Spoolman handle all filament usage tracking automatically
- `UPDATE_TAG` is a no-op for these tag types — no errors, just a clean skip

[![UID-Only / TigerTag / OpenSpool workflow](diagrams/update-tag-uid-only.svg)](diagrams/update-tag-uid-only.svg)
