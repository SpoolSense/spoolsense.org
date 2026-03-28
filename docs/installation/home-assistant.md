# Home Assistant Integration

SpoolSense auto-discovers in Home Assistant via MQTT. No manual configuration needed.

## Prerequisites

- MQTT broker running (Mosquitto or HA add-on)
- MQTT configured in the scanner (host, port, user, password)

## Auto-Discovery

When the scanner boots with MQTT configured, it publishes Home Assistant discovery messages. Your scanner appears as a device with these entities:

| Entity | Type | Description |
|--------|------|-------------|
| Spool Material | Sensor | Current filament type (PLA, PETG, ABS, etc.) |
| Spool Color | Sensor | Filament color hex code |
| Spool Remaining | Sensor | Remaining weight in grams |
| Spool Manufacturer | Sensor | Filament brand |
| Scanner Status | Sensor | Current state (ready, scanning, writing) |
| NFC UID | Sensor | Current tag UID |

## Dashboard

Create a dashboard card showing your current spool:

```yaml
type: entities
title: SpoolSense Scanner
entities:
  - entity: sensor.spoolsense_ecb008_material
  - entity: sensor.spoolsense_ecb008_remaining
  - entity: sensor.spoolsense_ecb008_color
  - entity: sensor.spoolsense_ecb008_manufacturer
```

## Automations

Example: notify when a spool drops below 100g:

```yaml
automation:
  - alias: Low filament warning
    trigger:
      - platform: numeric_state
        entity_id: sensor.spoolsense_ecb008_remaining
        below: 100
    action:
      - service: notify.mobile_app
        data:
          message: "Filament running low: {{ states('sensor.spoolsense_ecb008_remaining') }}g remaining"
```

## MQTT Topics

The scanner publishes to these topics:

| Topic | Purpose |
|-------|---------|
| `spoolsense/<deviceId>/tag/state` | Current spool data (JSON) |
| `spoolsense/<deviceId>/status` | Scanner online/offline |
| `homeassistant/sensor/spoolsense_*/config` | HA discovery |
