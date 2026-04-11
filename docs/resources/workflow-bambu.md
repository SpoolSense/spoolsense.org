# Workflow — Bambu Lab AMS

Bambu Lab support uses Home Assistant blueprints — no middleware and no `UPDATE_TAG` macro involved. Three blueprints handle the full flow: spool loading, dashboard display, and filament deduction.

For setup instructions, see the [Bambu Lab AMS Integration guide](../installation/bambu-ams.md).

---

## Full Data Flow

<iframe src="../diagrams/bambu-dashboard-flow.html" width="100%" height="1400" frameborder="0" style="border-radius: 12px; background: #1a1a2e;"></iframe>

<small>[Open full diagram](../diagrams/bambu-dashboard-flow.html){ target="_blank" }</small>

---

## How It Works

### Phase 1 — Spool Loading

1. Scan a spool's NFC tag on the SpoolSense scanner
2. The scanner publishes tag data (UID, material, color, temps) to MQTT and queries Spoolman for weight
3. The **loading blueprint** triggers in HA when the scanner sensor shows "present"
4. You load the spool into an AMS tray within 5 minutes
5. The blueprint detects the tray change and:
    - Sends `bambu_lab.set_filament` to the printer (sets material, color, temps on the tray)
    - Sends `cmd/tray_assign` to the scanner (stores which UID is in which tray)
6. The scanner queries Spoolman for the spool's remaining weight

### Phase 2 — Dashboard Update

1. The Bambu printer reports tray state changes to HA via the Bambu Lab integration
2. The **dashboard blueprint** triggers on any tray state change
3. It reads the current state of all tray entities (material, color, empty/loaded)
4. Publishes the tray array to the scanner via `cmd/tray_update`
5. The scanner renders the colored grid on the TFT display

### Phase 3 — Weight Deduction

1. A print finishes or is canceled on the Bambu printer
2. The **deduction blueprint** triggers and reads per-tray weight usage from the print weight sensor
3. For each tray that consumed filament, it sends `cmd/deduct_tray` to the scanner
4. The scanner resolves the tray index to a spool UID (from the mapping stored in Phase 1)
5. The scanner deducts the weight from Spoolman and updates the dashboard display

---

## Key Points

- No middleware, no Moonraker, no Klipper macros — entirely HA-driven
- Per-tray weight deduction from Bambu's own print weight sensor
- The scanner stores UID-to-tray mappings — no HA helpers needed
- Supports up to 16 trays across 4 AMS units
- Multi-color and multi-material prints are fully supported (each tray deducted independently)
- Dashboard state is cached in NVS — persists across reboots
- Writable tags (OpenPrintTag/OpenTag3D) get weight updated on next scan

---

## Previous SVG Diagram

For reference, the original deduction-only workflow diagram:

[![Bambu Lab AMS workflow](../diagrams/update-tag-bambu.svg)](../diagrams/update-tag-bambu.svg)
