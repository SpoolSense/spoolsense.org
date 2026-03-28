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

```bash
# ESP32-WROOM
pio run -e esp32dev
pio run -e esp32dev -t upload

# ESP32-S3-Zero
pio run -e esp32s3zero
pio run -e esp32s3zero -t upload
```

### Run Tests

```bash
# Native unit tests
make -C test/native test

# OpenPrintTag library tests
./test/native/test_openprinttag_runner
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
cp config.example.yaml config.yaml
```

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
