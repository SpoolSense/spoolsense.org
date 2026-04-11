# Workflow — Multi-Toolhead (Toolchanger)

Flow diagrams for toolchanger setups with multiple extruders (T0, T1, T2, T3).

---

## Tags That Track Filament Usage

**Supported tags:** OpenPrintTag, OpenTag3D

Same concept as single toolhead, but the middleware reads the per-tool `filament_weights` array from the last completed job. Each tool's usage is sent as a separate deduction to the scanner, keyed by that tool's active spool UID.

**Key points:**

- `UPDATE_TAG` fires once in PRINT_END — handles all tools in one pass
- Moonraker provides `filament_weights` per tool index (T0, T1, T2, T3)
- Middleware maps tool index to active spool UID and sends one deduction per spool
- Scanner accumulates deductions per UID independently
- Tools with zero usage in a job are skipped

[![Multi-Toolhead workflow](../diagrams/update-tag-multi-toolhead.svg)](../diagrams/update-tag-multi-toolhead.svg)

---

## Tags That Do Not Track Filament Usage

**Supported tags:** UID-only (plain NTAG), TigerTag, OpenSpool

The flow is identical to the single toolhead case — `UPDATE_TAG` is a no-op and Moonraker handles filament tracking for all tools via its built-in Spoolman integration. See the [Single Toolhead workflow](workflow-single-toolhead.md#tags-that-do-not-track-filament-usage) for details.
