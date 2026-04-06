# Workflow Diagrams

Visual flow diagrams showing how SpoolSense handles filament tracking for different tag types and printer configurations.

---

## Single Toolhead

### Tags That Track Filament Usage

**Supported tags:** OpenPrintTag, OpenTag3D

These tags have writable weight fields. After each print, the `UPDATE_TAG` macro (in your `PRINT_END`) triggers the middleware to calculate filament used from Moonraker's print history. The deduction is sent to the scanner and stored persistently. The next time that spool is scanned, the scanner writes the updated weight to the tag before proceeding with the normal scan flow.

The tag is the source of truth — Spoolman is updated from the tag data on each scan.

**Key points:**

- `UPDATE_TAG` fires automatically at the end of each print
- Deductions accumulate on the scanner if multiple prints occur before the next scan
- The scanner writes the updated weight to the tag on the next scan, then clears the deduction
[![OpenPrintTag / OpenTag3D workflow](diagrams/update-tag-openprinttag.svg)](diagrams/update-tag-openprinttag.svg)

---

### Tags That Do Not Track Filament Usage

**Supported tags:** UID-only (plain NTAG), TigerTag, OpenSpool

These tags have no writable weight field. Filament tracking is handled entirely by Moonraker's built-in Spoolman integration, which syncs usage in real-time during each print.

`UPDATE_TAG` still fires in `PRINT_END` but the middleware detects the tag type and skips — no action is taken. Spoolman already has the correct weight from Moonraker's real-time sync.

**Key points:**

- Requires `[spoolman]` configured in `moonraker.conf` for filament tracking
- SpoolSense only handles the initial scan (telling Moonraker which spool is active)
- Moonraker and Spoolman handle all filament usage tracking automatically
- `UPDATE_TAG` is a no-op for these tag types — no errors, just a clean skip

[![UID-Only / TigerTag / OpenSpool workflow](diagrams/update-tag-uid-only.svg)](diagrams/update-tag-uid-only.svg)

---

## Multi-Toolhead (Toolchanger)

### Tags That Track Filament Usage

**Supported tags:** OpenPrintTag, OpenTag3D

Same concept as single toolhead, but the middleware reads the per-tool `filament_weights` array from the last completed job. Each tool's usage is sent as a separate deduction to the scanner, keyed by that tool's active spool UID.

**Key points:**

- `UPDATE_TAG` fires once in PRINT_END — handles all tools in one pass
- Moonraker provides `filament_weights` per tool index (T0, T1, T2, T3)
- Middleware maps tool index to active spool UID and sends one deduction per spool
- Scanner accumulates deductions per UID independently
- Tools with zero usage in a job are skipped

[![Multi-Toolhead workflow](diagrams/update-tag-multi-toolhead.svg)](diagrams/update-tag-multi-toolhead.svg)

### Tags That Do Not Track Filament Usage

**Supported tags:** UID-only (plain NTAG), TigerTag, OpenSpool

The flow is identical to the single toolhead case — `UPDATE_TAG` is a no-op and Moonraker handles filament tracking for all tools via its built-in Spoolman integration. See the [Single Toolhead — Tags That Do Not Track Filament Usage](#tags-that-do-not-track-filament-usage) section above.

---

## AFC (Box Turtle / Armored Turtle)

### Tags That Track Filament Usage

**Supported tags:** OpenPrintTag, OpenTag3D

AFC already tracks per-lane remaining weight in real-time using extruder position monitoring. When `UPDATE_TAG` fires in PRINT_END, the middleware reads the current weight per lane from AFC's status endpoint — no print history calculation needed. The difference between the initial tag weight and AFC's current weight is the deduction.

**Key points:**

- AFC tracks weight per lane in real-time (extruder position → mm → grams using filament density)
- `UPDATE_TAG` reads AFC weights, not Moonraker print history — more accurate than slicer estimates
- Middleware calculates deduction per lane: initial tag weight minus AFC current weight
- One deduction sent per active spool UID, same scanner persistence as other setups
- Handles multi-lane prints where AFC swaps lanes mid-print

[![AFC workflow](diagrams/update-tag-afc.svg)](diagrams/update-tag-afc.svg)

### Tags That Do Not Track Filament Usage

**Supported tags:** UID-only (plain NTAG), TigerTag, OpenSpool

Same as single toolhead — `UPDATE_TAG` is a no-op. AFC handles Spoolman tracking via its own `spool_id` integration per lane. See the [Single Toolhead — Tags That Do Not Track Filament Usage](#tags-that-do-not-track-filament-usage) section above.

---

## Bambu Lab AMS

Bambu Lab support uses Home Assistant blueprints — no middleware and no `UPDATE_TAG` macro involved. Two blueprints handle the full flow:

1. **SpoolSense → Bambu AMS** — Scan a spool, load it into an AMS tray within 5 minutes, and the blueprint auto-pushes material, color, and temperature data to that tray via `bambu_lab.set_filament`. Also stores the spool UID and Spoolman ID in HA input helpers for deduction tracking.

2. **SpoolSense → Spoolman Deduction** — When a print finishes or is canceled, reads the per-tray weight breakdown from the Bambu print weight sensor, looks up the spool UID per tray from the stored helpers, and calls `spoolman.use_spool_filament` to deduct usage.

**Key points:**

- No middleware, no Moonraker, no Klipper macros — entirely HA-driven
- Per-tray weight deduction from Bambu's own print weight sensor
- Requires: HA Bambu Lab integration, spoolman-homeassistant integration, SpoolSense MQTT sensor
- Supports up to 16 trays across 4 AMS units
- Physical NFC tag weight is **not** updated — deduction goes to Spoolman only

[![Bambu Lab AMS workflow](diagrams/update-tag-bambu.svg)](diagrams/update-tag-bambu.svg)
