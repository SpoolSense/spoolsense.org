# Choose Your Extras

All extras are optional. The scanner works with just an ESP32 + NFC reader.

## 16x2 I2C LCD Display

Shows spool info directly on the scanner: material, color, remaining weight, and status messages.

| Spec | Value |
|------|-------|
| Type | 16x2 character LCD with I2C backpack |
| I2C Address | 0x27 (most common) |
| Wiring | 2 pins (SDA + SCL) + VCC + GND |
| Board Compatibility | All boards (WROOM, S3-Zero, S3-DevKitC) |

**When to add it:**

- You want to see spool info without opening a browser
- Your scanner is mounted away from a screen
- You use the keypad (LCD shows tool assignment feedback)

**When to skip it:**

- You only check spools via Home Assistant or the web UI
- You want the smallest possible build

## Status LED (SK6812 RGBW)

Shows scanner state with color-coded feedback: white during boot, blue when ready, filament color when a spool is scanned, flashing on write operations.

| Spec | Value |
|------|-------|
| Type | SK6812 RGBW (WROOM) or WS2812 RGB (S3-Zero/S3-DevKitC onboard) |
| Wiring | 1 data pin + VCC + GND (WROOM only) |
| Board Compatibility | External on WROOM, built-in on S3-Zero and S3-DevKitC |

**When to add it:**

- You're using an ESP32-WROOM and want visual feedback at the scanner

**When to skip it:**

- **ESP32-S3-Zero / S3-DevKitC** — have onboard WS2812 RGB LEDs built in, no external LED needed
- **AFC BoxTurtle users** — the middleware sends filament colors directly to the AFC lane LEDs, so you already get color feedback at the printer without a scanner LED

## 3x4 Matrix Keypad

Enables scan-to-tool assignment for toolchanger and multi-tool setups. Scan a spool, type the tool number, press # to confirm.

| Spec | Value |
|------|-------|
| Type | 3x4 membrane matrix keypad |
| Wiring | 7 pins (4 rows + 3 columns) |
| Board Compatibility | WROOM or S3-DevKitC recommended |

**Workflow:** Scan spool > Type tool number (e.g. `12`) > Press `#` to confirm > Scanner sends `ASSIGN_SPOOL TOOL=T12` to Moonraker

**When to add it:**

- You have a toolchanger or multi-tool setup (AFC, tradrack, etc.)
- You want to assign spools to tools without using Klipper screen or web UI

**When to skip it:**

- Single-tool printer
- You assign spools through Home Assistant or macros

!!! warning
    Using LCD + keypad together on the ESP32-S3-Zero is not recommended due to limited GPIO...it can be done but soldering to the exposed pins on the back is tough. Use the WROOM board for LCD + keypad builds.

## Summary

| Extra | Pins Used | WROOM | S3-Zero | S3-DevKitC | Recommended For |
|-------|-----------|-------|---------|------------|-----------------|
| LCD | 2 (I2C) | Yes | Yes | Yes | Visual feedback without browser |
| LED | 1 (data) | External | Built-in | Built-in | Status at a glance |
| Keypad | 7 (matrix) | Yes | Limited | Yes | Toolchanger/multi-tool |
