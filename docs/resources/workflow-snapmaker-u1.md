# Workflow — Snapmaker U1 (Direct Mode)

The Snapmaker U1 integration uses the extended firmware's `filament_detect/set` endpoint. The scanner posts scan data directly to Moonraker.

For setup, see the [Snapmaker U1 Integration guide](../installation/snapmaker-u1.md).

---

## Smart Tag Flow (OpenSpool, OpenPrintTag, TigerTag, OpenTag3D, Bambu)

1. User scans a tag. The scanner reads vendor, material, color, temps from the tag.
2. The scanner POSTs to `http://<u1-ip>:7125/printer/filament_detect/set`:
   ```json
   {
     "channel": 0,
     "info": {
       "VENDOR": "Polymaker",
       "MAIN_TYPE": "PLA",
       "SUB_TYPE": "Matte",
       "RGB_1": 16737792,
       "ALPHA": 255,
       "HOTEND_MIN_TEMP": 200,
       "HOTEND_MAX_TEMP": 220,
       "BED_TEMP": 60,
       "CARD_UID": [4, 161, 178, 195]
     }
   }
   ```
3. The extended firmware writes the data to channel 0's state.
4. Fluidd reflects the new spool.
5. If the tag is missing fields and Spoolman is configured, the scanner does a second POST after Spoolman lookup to fill in any gaps.

---

## Generic UID Tag Flow (NFC+)

1. User scans a UID-only tag.
2. The scanner queries Spoolman with the UID.
3. Spoolman returns the spool's data.
4. The scanner POSTs the data to the U1.

If the user removes the tag during the lookup, the scanner skips the POST.

---

## Comparison

| Workflow | Middleware | Klipper macros | Tag formats | Per-toolhead |
|---|---|---|---|---|
| Klipper / AFC | Required | None (uses AFC) | All 6 | Per-lane |
| Klipper toolchanger | Required | `ASSIGN_SPOOL` | All 6 | Per-tool via keypad |
| Bambu Lab AMS | None (HA-driven) | None | All 6 | Per-tray |
| Snapmaker U1 | None | None | All 6 | One scanner per channel (stage mode coming) |
