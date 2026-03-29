# Printer Compatibility

SpoolSense works with any printer that runs Klipper with Moonraker. Direct integrations for other firmware types are listed below.

## Support Matrix

| Printer / Firmware | Spool Scan | Active Spool | Filament Tracking | Lane / Tool Assignment | Notes |
|---|---|---|---|---|---|
| **Klipper + Moonraker** | ✅ | ✅ | ✅ | ✅ | Full support via middleware |
| **AFC (BoxTurtle, NightOwl)** | ✅ | ✅ | ✅ | ✅ | Per-lane assignment, `publish_lane_data` for Orca Slicer |
| **Toolchanger (Klipper)** | ✅ | ✅ | ✅ | ✅ | `ASSIGN_SPOOL TOOL=T{n}` macro or keypad |
| **Prusa (PrusaLink)** | ✅ | ✅ | ⚠️ Experimental | ❌ | See below |
| **Creality K1/K1 Max (rooted)** | ✅ | ✅ | ✅ | ✅ | Requires Moonraker via community script |
| **Creality K1/K1 Max (stock)** | ✅ | ❌ | ❌ | ❌ | Scan only — no API to push spool data |

**Legend:** ✅ Supported &nbsp;|&nbsp; ⚠️ Partial / Experimental &nbsp;|&nbsp; ❌ Not supported

---

## Klipper / Moonraker

Full support. Any Klipper printer with Moonraker works out of the box with the SpoolSense middleware. This includes:

- Voron, Trident, 0.2
- Bambu converted to Klipper
- Any DIY Klipper build

See [Middleware Setup](../installation/middleware.md) to get started.

---

## Prusa (PrusaLink)

**Status: Experimental**

Prusa printers running PrusaLink (MK4, XL, MINI+) are supported directly in the scanner firmware — no middleware needed.

- Monitors print start/stop via PrusaLink API
- Pre-print filament mismatch and temperature warnings
- Multi-tool support for the XL

Enable in the scanner web UI at `spoolsense.local/config` under the Printer section.

!!! warning "Looking for testers"
    PrusaLink integration is experimental. If you have a Prusa printer, please try it and [report issues](https://github.com/SpoolSense/spoolsense_scanner/issues).

---

## Creality (Rooted — K1, K1 Max, K1C, Ender-3 V3)

**Status: Compatible via Moonraker**

Root your printer and install Moonraker using one of these community scripts, then follow the standard [Middleware Setup](../installation/middleware.md) guide:

- **[Guilouz Creality Helper Script](https://github.com/Guilouz/Creality-Helper-Script)** — supports K1 Series, KE Series, Ender-3 V3 Series. Adds Moonraker, Fluidd, and Mainsail.
- **[SimpleAF by Pellcorp](https://github.com/pellcorp/creality)** — K1 series focus, replaces Creality's Klipper with stock Klipper.

!!! info "2025 K1C"
    The 2025 revision of the K1C removed the root option from the settings menu. Rooting is still possible via a community exploit but carries more risk. Proceed with caution.

Once Moonraker is running on your Creality printer, SpoolSense treats it identically to any other Klipper machine.

---

## Creality (Stock Firmware)

**Status: Scan only**

Stock Creality firmware does not expose Moonraker. The scanner can still read NFC tags and publish to MQTT or Home Assistant, but there is no way to push spool data back to the printer without Moonraker.
