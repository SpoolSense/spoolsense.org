# Workflow — AFC (Box Turtle / Armored Turtle)

Flow diagrams for AFC multi-lane filament changers with per-lane weight tracking.

---

## Tags That Track Filament Usage

**Supported tags:** OpenPrintTag, OpenTag3D

AFC already tracks per-lane remaining weight in real-time using extruder position monitoring. When `UPDATE_TAG` fires in PRINT_END, the middleware reads the current weight per lane from AFC's status endpoint — no print history calculation needed. The difference between the initial tag weight and AFC's current weight is the deduction.

**Key points:**

- AFC tracks weight per lane in real-time (extruder position → mm → grams using filament density)
- `UPDATE_TAG` reads AFC weights, not Moonraker print history — more accurate than slicer estimates
- Middleware calculates deduction per lane: initial tag weight minus AFC current weight
- One deduction sent per active spool UID, same scanner persistence as other setups
- Handles multi-lane prints where AFC swaps lanes mid-print

[![AFC workflow](../diagrams/update-tag-afc.svg)](../diagrams/update-tag-afc.svg)

---

## Tags That Do Not Track Filament Usage

**Supported tags:** UID-only (plain NTAG), TigerTag, OpenSpool

Same as single toolhead — `UPDATE_TAG` is a no-op. AFC handles Spoolman tracking via its own `spool_id` integration per lane. See the [Single Toolhead workflow](workflow-single-toolhead.md#tags-that-do-not-track-filament-usage) for details.
