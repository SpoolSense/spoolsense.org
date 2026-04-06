# Releases

SpoolSense has three components, each versioned independently.

| Component | Latest | Downloads | Changelog |
|-----------|--------|-----------|-----------|
| **Scanner Firmware** | [![GitHub Release](https://img.shields.io/github/v/release/SpoolSense/spoolsense_scanner)](https://github.com/SpoolSense/spoolsense_scanner/releases/latest) | [Firmware binaries](https://github.com/SpoolSense/spoolsense_scanner/releases/latest) | [Scanner changelog](scanner.md) |
| **Middleware** | [![GitHub Release](https://img.shields.io/github/v/release/SpoolSense/spoolsense_middleware)](https://github.com/SpoolSense/spoolsense_middleware/releases/latest) | [Source](https://github.com/SpoolSense/spoolsense_middleware/releases/latest) | [Middleware changelog](middleware.md) |
| **Installer** | [![GitHub Release](https://img.shields.io/github/v/release/SpoolSense/spoolsense-installer)](https://github.com/SpoolSense/spoolsense-installer/releases/latest) | [Source](https://github.com/SpoolSense/spoolsense-installer/releases/latest) | [Installer changelog](installer.md) |

## Quick Install

The installer downloads the latest firmware automatically:

```bash
curl -sL https://raw.githubusercontent.com/SpoolSense/spoolsense-installer/main/install.sh | bash
```

## Firmware Binaries

Each scanner release includes pre-built firmware for all three boards:

| File | Board |
|------|-------|
| `spoolsense_scanner_esp32dev.bin` | ESP32-WROOM DevKit |
| `spoolsense_scanner_esp32s3zero.bin` | ESP32-S3-Zero |
| `spoolsense_scanner_esp32s3devkitc.bin` | ESP32-S3-DevKitC-1 |

Plus matching bootloader and partition table binaries. The installer handles flashing all of these automatically.

## Updating

- **OTA:** Open `http://spoolsense.local/update` to check for and install updates over WiFi
- **USB:** Re-run the installer with your ESP32 connected
- **Middleware:** `cd ~/SpoolSense && git pull` and restart the service
