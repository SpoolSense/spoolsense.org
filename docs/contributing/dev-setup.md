# Development Setup

How to set up a local development environment for each SpoolSense component.

## Scanner Firmware

### Prerequisites

- [PlatformIO](https://platformio.org/) (CLI or VS Code extension)
- ESP32 connected via USB

### Setup

```bash
git clone https://github.com/SpoolSense/spoolsense_scanner.git
cd spoolsense_scanner
cp include/UserConfig.example.h include/UserConfig.h
```

Edit `include/UserConfig.h` with your WiFi credentials and preferences.

### Build and Flash

One environment per board:

| Board | Environment |
|---|---|
| ESP32-WROOM DevKit | `esp32dev` |
| ESP32-S3-Zero | `esp32s3zero` |
| ESP32-S3-DevKitC-1 (N16R8) | `esp32s3devkitc` |
| ESP32-C3 SuperMini / DevKitM-1 | `esp32c3` |
| ESP32-C6-DevKitC-1 | `esp32c6` |
| ESP32-C5-DevKitC-1 | `esp32c5` |

```bash
pio run -e <environment>              # build
pio run -e <environment> -t upload    # build + flash over USB
```

The platform is pinned to a specific [pioarduino](https://github.com/pioarduino/platform-espressif32) release (Arduino-ESP32 3.x / ESP-IDF 5.x) in `platformio.ini` — PlatformIO downloads it automatically on first build. Don't change the pin; the official `espressif32` platform cannot build the C6/C5 targets.

### Run Tests

```bash
# Native unit tests (parsers, diagnostics, layout, write guard)
make -C test/native test
```

## Middleware

### Prerequisites

- Python 3.9+
- MQTT broker (Mosquitto)
- Spoolman (optional)

### Setup

```bash
git clone https://github.com/SpoolSense/spoolsense_middleware.git
cd spoolsense_middleware/middleware
pip3 install -r requirements.txt
cp config.example.single.yaml config.yaml
```

Config examples exist per setup type — copy the one that matches yours:
`config.example.single.yaml` (single toolhead), `.afc.yaml` (AFC), `.toolchanger.yaml`,
`.happy_hare.yaml`, `.indx.yaml`.

### Run

```bash
python3 spoolsense.py
```

### Run Tests

```bash
python3 -m pytest tests/ -v
```

## Documentation Site

### Prerequisites

- Python 3.9+

### Setup

```bash
git clone https://github.com/SpoolSense/spoolsense.org.git
cd spoolsense.org
pip3 install -r requirements.txt
```

### Local Preview

```bash
mkdocs serve
```

Opens at `http://localhost:8000` with live reload.

### Build

```bash
mkdocs build
```

Output goes to `site/` directory.
