# Troubleshooting

## Scanner won't connect to WiFi

- Verify SSID and password are correct (re-run installer if needed)
- Check that your network is 2.4GHz (ESP32 does not support 5GHz)
- Try accessing by IP instead of `spoolsense.local` (check router DHCP list)

## NFC reader not detected

- Check SPI wiring matches the [wiring guide](../hardware/wiring-pn5180.md)
- Verify proper voltage (5V for PN5180, 3.3V for PN532)
- Open `http://spoolsense.local/troubleshooting` for diagnostics
- Check serial output for "PN5180: No response" or "PN532: No response"

## PN532 specific

- Ensure DIP switch is set to **SPI mode** (SEL0=0, SEL1=1)
- If intermittent read failures, the SPI clock may need reducing (firmware uses 1MHz by default)

## Tags not detected

- Hold the tag flat against the NFC reader antenna
- Try different positions. The sweet spot is directly over the antenna coil
- Check `http://spoolsense.local/reader` to see if a UID appears
- ISO15693 tags (ICODE SLIX2) are not supported on the PN532

## Spoolman sync not working

- Verify Spoolman URL in config (`http://spoolsense.local/config`)
- Test connectivity: `curl http://your-spoolman:7912/api/v1/health`
- Check that the `nfc_id` extra field exists in Spoolman

## MQTT / Home Assistant not discovering

- Verify MQTT broker is running and reachable
- Check MQTT credentials in config
- Restart Home Assistant after the scanner first connects
- Check MQTT Explorer for `homeassistant/sensor/spoolsense_*` topics

## OTA update fails

- Ensure stable WiFi connection during update
- The update page at `/update` shows progress
- If the update fails mid-flash, re-flash via USB using the installer

## Web UI not loading

- Try clearing browser cache or using incognito mode
- Check that port 80 is not blocked by your network
- Try accessing by IP address instead of mDNS hostname

## Viewing Serial Output

Serial output is the best way to debug boot issues, WiFi failures, and NFC errors. Plug the ESP32 into your computer or printer host via USB and open a terminal.

**Linux (Raspberry Pi, printer host):**

```bash
# Find the port
ls /dev/ttyUSB* /dev/ttyACM* 2>/dev/null

# Open serial monitor (115200 baud)
screen /dev/ttyUSB0 115200
```

Press `Ctrl+A` then `K` then `Y` to exit screen.

**macOS:**

```bash
# Find the port
ls /dev/cu.usbserial-* /dev/cu.usbmodem* 2>/dev/null

# Open serial monitor
screen /dev/cu.usbserial-310 115200
```

**PlatformIO (any OS):**

```bash
pio device monitor -b 115200
```

**What to look for:**

- `WiFi connected` / `WiFi failed` — network issues
- `PN5180: No response` / `PN532: No response` — wiring or voltage problem
- `NFC reader: PN5180 v3.4` or `NFC reader: PN532 v1.6` — reader detected successfully
- `SpoolmanManager: sync OK` / `sync failed` — Spoolman connectivity
- `MQTT connected` / `MQTT failed` — broker issues

!!! tip
    Press the **RST** button on the ESP32 to reboot and see the full startup sequence from the beginning.
