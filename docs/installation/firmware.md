# Firmware Install

The SpoolSense installer handles everything: firmware download, configuration, and flashing.

## Prerequisites

- Python 3.9 or newer
- Git
- ESP32 connected via USB

## Quick Install

Connect your ESP32 via USB, then run:

```bash
curl -sL https://raw.githubusercontent.com/SpoolSense/spoolsense-installer/main/install.sh | bash
```

The installer will:

1. Check Python and git are installed
2. Download the latest firmware from GitHub Releases
3. Walk you through configuration (WiFi, MQTT, Spoolman, hardware options)
4. Generate an NVS partition with your settings
5. Flash the firmware + config to your ESP32

## What the Installer Asks

| Setting | Description | Required |
|---------|-------------|----------|
| Board | ESP32-WROOM or S3-Zero | Yes |
| WiFi SSID | Your network name | Yes |
| WiFi Password | Your network password | Yes |
| MQTT Host | Broker IP for Home Assistant | Optional |
| Spoolman URL | Your Spoolman instance URL | Optional |
| NFC Reader | PN5180 or PN532 | Yes |
| LCD | Enable 16x2 I2C LCD | Optional |
| LED | Enable status LED | Optional |
| Keypad | Enable 3x4 matrix keypad | Optional |
| Moonraker URL | For keypad tool assignment | Optional |

## After Flashing

1. The ESP32 reboots automatically
2. It connects to your WiFi network
3. Open `http://spoolsense.local` in your browser
4. You should see the SpoolSense landing page with device info

!!! tip
    If `spoolsense.local` doesn't resolve, check your router's DHCP client list for the ESP32's IP address and access it directly.

## Updating Firmware

### Over-the-Air (OTA)

Once running, go to `http://spoolsense.local/update`. The page checks GitHub for new releases and offers one-click OTA update.

### USB Re-flash

Run the installer again with the same settings. Your NVS configuration is preserved unless you explicitly re-configure.

## Building from Source

If you prefer to compile yourself:

```bash
git clone https://github.com/SpoolSense/spoolsense_scanner.git
cd spoolsense_scanner
cp include/UserConfig.example.h include/UserConfig.h
# Edit UserConfig.h with your settings
pio run -e esp32dev      # or esp32s3zero
pio run -e esp32dev -t upload
```

Then run the installer in config-only mode to write NVS settings:

```bash
curl -sL https://raw.githubusercontent.com/SpoolSense/spoolsense-installer/main/install.sh | bash -- --config-only
```
