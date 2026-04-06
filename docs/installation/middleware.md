# Middleware Setup

The SpoolSense middleware bridges the scanner to Klipper/AFC. It typically runs on your Klipper host (Raspberry Pi), but can run on any machine on your network that can reach the MQTT broker and Moonraker.

## Do I Need This?

You need the middleware if you want:

- AFC lane assignment (BoxTurtle, tradrack)
- Toolchanger spool assignment
- Tag weight writeback after prints
- Slicer integration (Orca Slicer lane data)

If you only need Spoolman sync and Home Assistant, the scanner handles that directly. No middleware needed.

## Install

SSH into your Klipper host and run:

```bash
ssh pi@your-printer-ip
curl -sL https://raw.githubusercontent.com/SpoolSense/spoolsense-installer/main/install.sh | bash
```

Choose **"Middleware only"** when asked what to install. Here's what the installer looks like:

<div style="background:#0b0b0d; border:1px solid #2a2e36; border-radius:12px; padding:20px; font-family:'JetBrains Mono',monospace; font-size:13px; line-height:1.7; overflow-x:auto; color:#f4f4f5">
<pre style="margin:0; background:transparent; color:inherit">
<span style="color:#06b6d4;font-weight:bold">╔══════════════════════════════════════╗
║       SpoolSense Installer           ║
╚══════════════════════════════════════╝</span>

What do you want to install?
  1) Scanner + Middleware (recommended)
  2) Scanner only
  3) <span style="color:#fff;font-weight:bold">Middleware only</span>
  4) Config only (source builds)
Choice [1]: <span style="color:#fff">3</span>

<span style="color:#06b6d4;font-weight:bold">── Connection Settings ─────────────────</span>

  (These must match your scanner's config)

MQTT broker host: <span style="color:#fff">192.168.1.50</span>
MQTT port [1883]:
MQTT username []:
MQTT password []:

<span style="color:#06b6d4;font-weight:bold">── Middleware Configuration ────────────</span>

Scanner setup:
  1) AFC shared scanner (scan spool, load any lane)
  2) AFC per-lane scanners (one scanner per lane)
  3) Toolchanger shared scanner (scan spool, assign via macro or keypad)
  4) Toolchanger per-toolhead scanners (one scanner per tool)
  5) Single toolhead (one scanner, one extruder)
Choice [1]: <span style="color:#fff">1</span>

  <span style="color:#f59e0b">Note:</span> After flashing your scanner, find its device ID
  from the MQTT topic: spoolsense/&lt;device_id&gt;/tag/state

Moonraker URL [http://localhost]: <span style="color:#fff">http://localhost</span>

  <span style="color:#f59e0b">Slicer integration:</span> Slicers like Orca Slicer can auto-populate
  tool colors, materials, and temps from your scanned spools.

Enable slicer integration for toolheads? [y/N]: <span style="color:#fff">y</span>

<span style="color:#06b6d4;font-weight:bold">── Installing Middleware ────────────────</span>

  Cloning SpoolSense middleware...
  <span style="color:#22c55e">✓</span> Repository ready
  Installing Python dependencies...
  <span style="color:#22c55e">✓</span> Dependencies installed
  <span style="color:#22c55e">✓</span> Config written to ~/SpoolSense/middleware/config.yaml
  Creating systemd service...
  <span style="color:#22c55e">✓</span> Service created and enabled

<span style="color:#22c55e">══════════════════════════════════════
  SpoolSense is installed!

  Middleware: systemctl status spoolsense
  Config:     ~/SpoolSense/middleware/config.yaml
══════════════════════════════════════</span>
</pre>
</div>

### Manual Install (Advanced)

!!! warning "For advanced users only"
    Most users should use the installer above.

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

## Klipper Macros

SpoolSense includes Klipper macros for Spoolman spool tracking. These live in the middleware repo at `middleware/klipper/` and need to be copied to your Klipper config directory.

### spoolman_macros.cfg — Required for all Klipper setups

Provides `SET_ACTIVE_SPOOL`, `CLEAR_ACTIVE_SPOOL`, and automatic spool restore after Klipper restart.

```bash
cp ~/SpoolSense/middleware/klipper/spoolman_macros.cfg ~/printer_data/config/
```

Add to your `printer.cfg`:

```ini
[include spoolman_macros.cfg]

[save_variables]
filename: ~/printer_data/config/variables.cfg
```

!!! tip
    Add `CLEAR_ACTIVE_SPOOL` to your `END_PRINT` macro so Spoolman stops tracking filament after a print completes.

### spoolsense.cfg — Required for toolchanger setups

Provides the `ASSIGN_SPOOL` macro used by the middleware and the 3x4 keypad to assign scanned spools to toolheads.

```bash
cp ~/SpoolSense/middleware/klipper/spoolsense.cfg ~/printer_data/config/
```

Add to your `printer.cfg`:

```ini
[include spoolsense.cfg]
```

### toolhead_macros_example.cfg — Example for toolchanger users

Example T0–T3 toolchange macros with per-toolhead spool tracking. Each toolchange calls `SET_ACTIVE_SPOOL` so Spoolman tracks the correct spool per tool. Includes `RESTORE_SPOOL_IDS` to restore all toolhead spool assignments after Klipper restart.

```bash
cp ~/SpoolSense/middleware/klipper/toolhead_macros_example.cfg ~/printer_data/config/
```

!!! warning
    This is an **example** — adapt the T0–T3 macros to match your existing toolchange configuration. Don't replace your working macros. Add `variable_spool_id: None` and the `SET_ACTIVE_SPOOL` call to your existing T0–T3 macros.

### Which macros do I need?

| Setup | spoolman_macros.cfg | spoolsense.cfg | toolhead_macros_example.cfg |
|-------|:---:|:---:|:---:|
| Single toolhead | ✅ | — | — |
| Toolchanger (shared scanner) | ✅ | ✅ | ✅ (adapt) |
| Toolchanger (per-toolhead scanners) | ✅ | — | ✅ (adapt) |
| AFC (BoxTurtle, tradrack) | ✅ | — | — |

## Running as a Service

!!! note
    The installer sets this up automatically. These commands are only needed if you installed manually.

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

## Automatic Updates via Mainsail

Add SpoolSense to Moonraker's update manager so you get notified of new versions in Mainsail's update panel.

Add this to your `moonraker.conf`:

```ini
[update_manager spoolsense]
type: git_repo
path: ~/SpoolSense
origin: https://github.com/SpoolSense/spoolsense_middleware.git
primary_branch: master
managed_services: spoolsense
```

Then restart Moonraker:

```bash
sudo systemctl restart moonraker
```

SpoolSense will now appear in **Machine → Update Manager** in Mainsail. When a new version is available, you can update with one click — Moonraker pulls the latest code and restarts the service automatically.
