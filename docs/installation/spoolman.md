# Spoolman Integration

[Spoolman](https://github.com/Donkie/Spoolman) is an open-source spool management system. SpoolSense syncs scanned spool data to Spoolman automatically.

## What is Spoolman?

Spoolman tracks your filament inventory: how many spools you own, what material and color each one is, and how much filament is left. It integrates with Klipper via Moonraker so your printer knows what's loaded.

SpoolSense adds the NFC layer. Scan a spool, and Spoolman is updated automatically. No manual entry, no guessing which spool is on the printer.

## Do I Need Spoolman?

Spoolman is optional but strongly recommended. Without it:

- Smart tags (TigerTag, OpenPrintTag, OpenTag3D) still work. Data comes from the tag itself.
- NFC+ tags don't work. They rely on Spoolman for all spool data.
- No centralized inventory tracking.

With it, you get a full spool management system that SpoolSense keeps updated every time you scan.

## Installing Spoolman

Spoolman runs as a lightweight Docker container or standalone service. Most Klipper hosts (Raspberry Pi 4, CB1, similar) have plenty of power to run it alongside Klipper.

### Recommended: Run on Your Printer Host

The simplest setup is running Spoolman on the same machine as Klipper/Moonraker. This keeps everything on one box and avoids cross-network latency.

**Docker (recommended):**

```bash
mkdir -p ~/spoolman && cd ~/spoolman
cat > docker-compose.yml << 'EOF'
services:
  spoolman:
    image: ghcr.io/donkie/spoolman:latest
    restart: unless-stopped
    ports:
      - "7912:8000"
    volumes:
      - ./data:/home/app/.local/share/spoolman
EOF
docker compose up -d
```

Spoolman is now running at `http://your-printer-ip:7912`.

**Without Docker:**

See the [Spoolman installation docs](https://github.com/Donkie/Spoolman#installation) for standalone install options.

### Alternative: Separate Server

If you prefer to keep your printer host lean, run Spoolman on any machine on your network (NAS, home server, etc.). Just make sure the scanner and middleware can reach it over HTTP.

### Moonraker Integration

Add Spoolman to your Moonraker config so Fluidd/Mainsail can display spool info:

```ini
# moonraker.conf
[spoolman]
server: http://localhost:7912
sync_rate: 5
```

Restart Moonraker after adding this.

## Connecting SpoolSense to Spoolman

During scanner setup (installer or web config), enter your Spoolman URL:

- Same machine: `http://localhost:7912`
- Different machine: `http://192.168.x.x:7912`

The scanner tests the connection on boot and shows the result on the troubleshooting page.

## How Sync Works

When a tag is scanned:

**Smart tags** (OpenPrintTag, TigerTag, OpenTag3D) contain filament data on the tag. The scanner searches Spoolman for a matching spool by UID. If no match is found, a new spool entry is created automatically. If a match exists, the entry is updated with the latest tag data.

**NFC+ tags** (plain NTAG with UID only) carry no data. The scanner searches Spoolman by `nfc_id`. If found, the spool data (material, color, weight, manufacturer) is pulled from Spoolman and displayed on the LCD, web reader, and Home Assistant.

A sync cache prevents redundant API calls when the same spool is scanned repeatedly.

## Registering NFC+ Tags

For plain NTAG tags with no data written:

1. Scan the tag on the reader
2. Open `http://spoolsense.local/reader`
3. The UID is displayed. Click "Register in Spoolman"
4. Fill in material, manufacturer, and color
5. The spool is created in Spoolman with the tag's UID as `nfc_id`

Future scans of this tag will automatically look up the spool data from Spoolman.

## Spoolman Extra Field

SpoolSense uses the `nfc_id` extra field in Spoolman to link physical NFC tags to spool records. The installer creates this field automatically. To create it manually:

```bash
curl -X POST http://your-spoolman:7912/api/v1/field \
  -H "Content-Type: application/json" \
  -d '{"name": "nfc_id", "entity_type": "spool", "field_type": "text"}'
```

## Verifying the Connection

Check the scanner's troubleshooting page at `http://spoolsense.local/troubleshoot`. It shows:

- Whether Spoolman is reachable
- The Spoolman version
- The configured URL

Or check via API:

```bash
curl -s http://spoolsense.local/api/diagnostics | python3 -m json.tool
```

Look for `"spoolman": {"enabled": true, "reachable": true}`.
