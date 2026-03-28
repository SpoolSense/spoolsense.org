# What is SpoolSense

SpoolSense is an open-source NFC-based filament tracking system for 3D printers. It uses an ESP32 microcontroller and an NFC reader to scan spool tags, identify filament, and keep your spool management system up to date automatically.

## The Problem

Every 3D printer owner eventually asks: "How much filament is left on this spool?" and "Which spool is loaded right now?" Manual tracking breaks down fast, especially with multiple printers or tool changers.

## The Solution

Stick an NFC tag on each spool. When you load a spool, the scanner reads the tag and:

- Identifies the filament (material, color, manufacturer, weight)
- Updates your spool manager (Spoolman) automatically
- Publishes status to Home Assistant for dashboards and automations
- Sends spool data to Klipper/Moonraker via the optional middleware for AFC lane assignment, toolchanger support, and slicer integration (Orca Slicer, etc.)
- Tracks remaining filament as you print

No cloud. No subscriptions. Everything runs on your local network.

## What You Need

- An **ESP32** board (WROOM or S3-Zero)
- An **NFC reader** (PN5180 or PN532)
- **NFC tags** on your spools (NTAG215, TigerTag, OpenPrintTag, or OpenTag3D)
- **Spoolman** running on your network (optional but recommended)

That's it. Total hardware cost is under $15.

## Project Repos

| Repository | Description |
|------------|-------------|
| [spoolsense_scanner](https://github.com/SpoolSense/spoolsense_scanner) | ESP32 firmware (Arduino/PlatformIO) |
| [spoolsense_middleware](https://github.com/SpoolSense/spoolsense_middleware) | Klipper/AFC integration service |
| [spoolsense-installer](https://github.com/SpoolSense/spoolsense-installer) | Interactive setup CLI |
