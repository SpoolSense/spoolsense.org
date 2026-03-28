# Middleware Setup

The SpoolSense middleware bridges the scanner to Klipper/AFC. It typically runs on your Klipper host (Raspberry Pi), but can run on any machine on your network that can reach the MQTT broker and Moonraker (a separate Pi, a Linux VM, a NAS, etc.).

## When Do I Need This?

You need the middleware if you want:

- AFC lane assignment (BoxTurtle, tradrack)
- Toolchanger spool assignment
- Tag weight writeback after prints
- Slicer integration (Orca Slicer lane data)

If you only need Spoolman sync and Home Assistant, the scanner handles that directly. No middleware needed.

## Install

### Easy Mode (Voron / Klipper Users)

If you're running Klipper on a Raspberry Pi (or similar host), plug the ESP32 into one of your printer's USB ports. Then SSH into your printer and run:

```bash
ssh pi@your-printer-ip
curl -sL https://raw.githubusercontent.com/SpoolSense/spoolsense-installer/main/install.sh | bash
```

Choose **"Both"** when asked what to install. The installer will:

1. Flash the scanner firmware to the ESP32 (auto-detects the USB port)
2. Install the middleware on the same machine
3. Generate `config.yaml` with your settings
4. Set up the systemd service

After it finishes, unplug the ESP32 from USB and power it separately (USB power adapter or a permanent USB port). The scanner communicates over WiFi, not USB. The USB connection is only needed for the initial flash.

!!! tip
    This is the recommended setup for most Klipper users. Everything runs on one machine, the installer handles it all, and you're done in a few minutes.

### Manual Install

```bash
git clone https://github.com/SpoolSense/spoolsense_middleware.git ~/SpoolSense
cd ~/SpoolSense/middleware
pip3 install -r requirements.txt
cp config.example.yaml config.yaml
# Edit config.yaml with your settings
```

## Configuration

### Finding Your Scanner's Device ID

The middleware identifies each scanner by its **device ID**. You need this to configure the `scanners:` section.

To find it:

1. Power on the scanner and connect to `http://spoolsense.local`
2. The device ID is displayed on the landing page (e.g. `ecb008`)
3. It's also available at `http://spoolsense.local/api/status` in the `device_id` field

### Edit config.yaml

```yaml
mqtt:
  host: localhost
  port: 1883

spoolman:
  url: http://localhost:7912

moonraker:
  url: http://localhost:7125

scanners:
  ecb008:                    # Replace with YOUR scanner's device ID
    action: toolhead          # or afc_lane, afc_stage, toolhead_stage
    target: T0                # Tool or lane name
    publish_lane_data: true   # Write to Moonraker DB for slicer integration
```

!!! tip
    If you have multiple scanners, add each one as a separate entry under `scanners:` with its own device ID and action.

## Actions

| Action | Use Case |
|--------|----------|
| `toolhead` | Single toolhead. Scan assigns spool directly. |
| `toolhead_stage` | Toolchanger. Scan stages spool, ASSIGN_SPOOL macro assigns to tool. |
| `afc_lane` | AFC, one scanner per lane. Scan assigns to that lane. |
| `afc_stage` | AFC, shared scanner. Scan stages spool, lane load triggers assignment. |

## Running as a Service

```bash
sudo cp ~/SpoolSense/middleware/spoolsense.service /etc/systemd/system/
sudo systemctl enable spoolsense
sudo systemctl start spoolsense
```

Check status:

```bash
sudo systemctl status spoolsense
journalctl -u spoolsense -f
```
