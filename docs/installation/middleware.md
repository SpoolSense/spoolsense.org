# Middleware Setup

The SpoolSense middleware runs on your Klipper host (Raspberry Pi) and bridges the scanner to Klipper/AFC.

## When Do I Need This?

You need the middleware if you want:

- AFC lane assignment (BoxTurtle, tradrack)
- Toolchanger spool assignment
- Tag weight writeback after prints
- Slicer integration (Orca Slicer lane data)

If you only need Spoolman sync and Home Assistant, the scanner handles that directly. No middleware needed.

## Install

The SpoolSense installer can set up both the scanner firmware and the middleware in one pass:

```bash
curl -sL https://raw.githubusercontent.com/SpoolSense/spoolsense-installer/main/install.sh | bash
```

Choose "Both" when asked what to install.

### Manual Install

```bash
git clone https://github.com/SpoolSense/spoolsense_middleware.git ~/SpoolSense
cd ~/SpoolSense/middleware
pip3 install -r requirements.txt
cp config.example.yaml config.yaml
# Edit config.yaml with your settings
```

## Configuration

Edit `config.yaml`:

```yaml
mqtt:
  host: localhost
  port: 1883

spoolman:
  url: http://localhost:7912

moonraker:
  url: http://localhost:7125

scanners:
  ecb008:                    # Your scanner's device ID (shown on the web UI)
    action: toolhead          # or afc_lane, afc_stage, toolhead_stage
    target: T0                # Tool or lane name
    publish_lane_data: true   # Write to Moonraker DB for slicer integration
```

## Actions

| Action | Use Case | How It Works |
|--------|----------|--------------|
| `toolhead` | Single toolhead | Scan assigns spool directly to the tool |
| `toolhead_stage` | Toolchanger | Scan stages spool, ASSIGN_SPOOL macro assigns to tool |
| `afc_lane` | AFC with dedicated scanner per lane | Scan assigns spool to a specific lane |
| `afc_stage` | AFC with shared scanner | Scan stages spool, lane load triggers assignment |

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
