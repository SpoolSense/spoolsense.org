# Adding a Board Variant

The scanner firmware supports a fixed set of boards, each with its own compile-time pin map and PlatformIO build environment. If your ESP32 board isn't on the list, you can add it yourself and submit a PR. This page walks through every file a new variant touches.

Current variants: `esp32dev` (WROOM), `esp32s3zero`, `esp32s3devkitc`, `esp32c3`, `esp32c6`, `esp32c5`.

## Before you start

- **Check whether an existing variant already works.** Boards with the same chip and compatible free GPIOs can often run an existing build unmodified. Wire it per the closest variant's [wiring guide](../hardware/wiring-pn5180.md) and test before adding a new env. Since v1.8.3 the six NFC bus pins (RST, NSS, BUSY, SCK, MOSI, MISO) can also be remapped at runtime from the config page's advanced section ([#201](https://github.com/SpoolSense/spoolsense_scanner/issues/201)) — often enough to run a near-miss board on an existing build without a reflash.
- `BoardPins.h` still defines each variant's compile-time defaults, and a new variant needs safe ones: a wrong default bus pin produces a scanner that won't boot cleanly. The status LED pin is runtime-configurable too ([#203](https://github.com/SpoolSense/spoolsense_scanner/issues/203)).

## Pin selection rules

Every chip family has pins you must avoid:

| Chip | Avoid | Reason |
|---|---|---|
| ESP32 (WROOM) | GPIO 0, 2, 12, 15 as outputs to peripherals that drive them at boot | Strap pins |
| ESP32 (WROOM) | GPIO 34-39 for outputs | Input-only |
| ESP32-S3 (N16R8 / octal PSRAM) | GPIO 33-37 | Reserved by PSRAM |
| ESP32-S3 | GPIO 0, 3, 45, 46 | Strap pins |
| ESP32-C3 | GPIO 2, 8, 9 | Strap pins |
| ESP32-C6 | GPIO 4, 5, 8, 9, 15 | Strap pins (8 drives the onboard WS2812 — safe only because nothing external can pull it at reset) |
| ESP32-C5 | GPIO 2, 3, 7, 25, 26, 28 | Strap pins |
| All | GPIO 6-11 (classic ESP32) / flash pins | Connected to SPI flash |

A peripheral that can hold a strap pin low at power-on (like an NFC reader's RST line) will put the board into download mode. This is why the C3 variant wires PN5180 RST to GPIO 0 (not a strap pin on C3) instead of GPIO 2.

## Files to change

### 1. `include/BoardPins.h` (scanner repo)

Add an `#elif defined(BOARD_YOUR_BOARD)` block defining the full pin set. Copy the closest existing variant as a starting point. Pins for unsupported peripherals get `-1` (see the C3 block, which disables TFT and keypad).

### 2. `platformio.ini`

Add a new `[env:yourboard]` section. Set `board`, flash size/mode, and a `-DBOARD_YOUR_BOARD=1` build flag that selects your BoardPins block. Keep the `platform =` line inherited from `[env]` — it pins an exact [pioarduino](https://github.com/pioarduino/platform-espressif32) release (Arduino-ESP32 3.x / ESP-IDF 5.x), and the official `espressif32` platform cannot build the C6/C5 targets. Boards with a single general-purpose SPI controller that should still drive a TFT need `-DBOARD_SHARED_SPI=1` (see the C6/C5 envs — NFC and TFT share the bus).

### 3. CI and release workflows

Two workflows build each env explicitly:

- `.github/workflows/ci.yml` — add your env to the build matrix (`env: [...]`).
- `.github/workflows/release.yml` — copy an existing per-env step: it builds, runs `scripts/check_flash_budget.py`, and copies firmware/bootloader/partitions into `release/` **in the same step**. Keep that shape — building everything first and copying at the end loses artifacts on the runner (that failure shipped a broken release once). Checksums are generated for whatever lands in `release/`, so nothing else needs editing.

### 4. Wiring documentation (this site)

Add a pinout section for your board to [wiring-pn5180.md](../hardware/wiring-pn5180.md) and/or [wiring-pn532.md](../hardware/wiring-pn532.md), matching the format of the existing tables. Note any scoping decisions (e.g. C3 has no TFT — its single SPI controller is reserved for NFC; C6/C5 run NFC and TFT on a shared bus instead).

### 5. Optional: web flasher

Web-flasher manifests live in this site's repo (`docs/installation/manifest-*.json`) and are updated by the release workflow. Mention in your PR that the board should be added to the flasher and we'll wire it up.

### 6. Optional: installer

The interactive installer keeps its own board list (`spoolsense_installer/constants.py` in the installer repo — display name, chip ID, firmware suffix, minimum flash, bootloader offset). The original ESP32 uses offset `0x1000`; every newer chip (S3, C3, C6, C5) uses `0x0`.

## Testing checklist

Before opening the PR:

- [ ] Firmware builds: `pio run -e yourboard`
- [ ] Board boots and joins WiFi (or starts AP mode)
- [ ] NFC reader initializes and reads a tag (`http://spoolsense.local/logs` shows the detection)
- [ ] Any scoped-in peripherals (LCD, LED) work
- [ ] Cold boot works from a plain power supply, not just USB-from-computer

## Submitting

Open a PR against `dev` in [spoolsense_scanner](https://github.com/SpoolSense/spoolsense_scanner) with the firmware changes, and mention the wiring-doc addition in the PR description (docs live in this site's repo — we can take the wiring table from your PR text). Include what hardware you tested on.
