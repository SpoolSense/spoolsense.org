# Home Assistant Integration

SpoolSense auto-discovers in Home Assistant via MQTT. No manual configuration needed.

## Prerequisites

- MQTT broker running (Mosquitto or HA add-on)
- MQTT configured in the scanner (host, port, user, password via web config or installer)

## Auto-Discovery

When the scanner boots with MQTT configured, it publishes Home Assistant discovery messages. Your scanner appears as a device with these entities:

| Entity | Type | Description |
|--------|------|-------------|
| Spool | Sensor | JSON sensor with material, color, weight, manufacturer, UID, temps, Spoolman ID |
| Printer Warning | Sensor | Filament mismatch and temperature warnings |
| Set Remaining Filament | Number | Write remaining weight to tag from HA |
| Set Initial Spool Weight | Number | Write initial weight to tag |
| Set Spoolman ID | Number | Write Spoolman ID to tag |
| Set Material Type | Select | Change material type on tag |
| Set Manufacturer | Text | Change manufacturer on tag |

The spool sensor retains the last scanned spool data even after the tag is removed, so dashboards always show the most recent scan.

## Dashboard Card

Add a markdown card to show the currently (or last) scanned spool with filament color, temps, and weight:

1. Go to your dashboard → Edit → Add Card
2. Search for **Markdown** → click **Show Code Editor** (bottom left)
3. Paste:

```yaml
type: markdown
content: >-
  {% set s = states.sensor.spoolsense_DEVICEID_spool %}
  {% set a = s.attributes %}
  {% if a.uid %}
  **{{ a.manufacturer }} {{ a.material_name }}**

  <font color="{{ a.color }}">⬤</font> {{ a.color }}

  Remaining: {{ a.remaining_g }}g / {{ a.initial_weight_g }}g

  {% if a.min_print_temp %}Nozzle: {{ a.min_print_temp }}–{{ a.max_print_temp }}°C{% endif %}

  {% if a.min_bed_temp %}Bed: {{ a.min_bed_temp }}–{{ a.max_bed_temp }}°C{% endif %}

  {% if a.present %}🟢 Tag on reader{% else %}⚪ Last scanned: {{ s.last_changed | as_timestamp | timestamp_custom('%b %d, %I:%M %p') }}{% endif %}
  {% else %}
  *No spool scanned yet*
  {% endif %}
```

!!! note
    Replace `DEVICEID` with your scanner's device ID (shown on the landing page at `spoolsense.local`, e.g. `ecb338`).

## Automations

### Low Filament Notification

Get a phone alert when remaining filament drops below 100g:

```yaml
automation:
  - alias: Low filament warning
    trigger:
      - platform: template
        value_template: >-
          {{ state_attr('sensor.spoolsense_DEVICEID_spool', 'remaining_g') | float < 100
             and state_attr('sensor.spoolsense_DEVICEID_spool', 'remaining_g') | float > 0 }}
    action:
      - service: notify.mobile_app
        data:
          title: "Filament Low"
          message: >-
            {{ state_attr('sensor.spoolsense_DEVICEID_spool', 'manufacturer') }}
            {{ state_attr('sensor.spoolsense_DEVICEID_spool', 'material_name') }}
            — {{ state_attr('sensor.spoolsense_DEVICEID_spool', 'remaining_g') }}g remaining
```

### Spool Scanned Notification

Get notified whenever a new spool is scanned:

```yaml
automation:
  - alias: Spool scanned
    trigger:
      - platform: state
        entity_id: sensor.spoolsense_DEVICEID_spool
        to: "present"
    action:
      - service: notify.mobile_app
        data:
          title: "Spool Scanned"
          message: >-
            {{ state_attr('sensor.spoolsense_DEVICEID_spool', 'manufacturer') }}
            {{ state_attr('sensor.spoolsense_DEVICEID_spool', 'material_name') }}
            — {{ state_attr('sensor.spoolsense_DEVICEID_spool', 'remaining_g') }}g
```

## Spool Sensor Attributes

The spool sensor publishes these attributes:

| Attribute | Type | Description |
|-----------|------|-------------|
| `uid` | string | NFC tag UID |
| `present` | bool | Tag currently on reader |
| `tag_data_valid` | bool | Tag has parseable filament data |
| `material_type` | string | Material name |
| `material_name` | string | Material name (e.g., "PLA", "PETG") |
| `color` | string | Hex color (e.g., "#FF0000") |
| `manufacturer` | string | Brand name |
| `remaining_g` | float | Remaining weight in grams |
| `initial_weight_g` | float | Initial spool weight in grams |
| `spoolman_id` | int | Spoolman spool ID (-1 if not set) |
| `min_print_temp` | int | Min nozzle temp °C (0 if not set) |
| `max_print_temp` | int | Max nozzle temp °C (0 if not set) |
| `min_bed_temp` | int | Min bed temp °C (0 if not set) |
| `max_bed_temp` | int | Max bed temp °C (0 if not set) |
| `density` | float | Filament density g/cm³ (0 if not set) |
| `diameter_mm` | float | Filament diameter mm (0 if not set) |

## MQTT Topics

| Topic | Purpose |
|-------|---------|
| `spoolsense/<deviceId>/tag/state` | Spool data JSON (retained) |
| `spoolsense/<deviceId>/tag/attributes` | Spool attributes mirror |
| `spoolsense/<deviceId>/cmd/update_remaining/<uid>` | Write remaining weight to tag |
| `spoolsense/<deviceId>/cmd/write_tag/<uid>` | Write full tag data |
| `homeassistant/sensor/spoolsense_*/config` | HA discovery configs |
| `homeassistant/number/spoolsense_*/config` | HA number entity discovery |
| `homeassistant/select/spoolsense_*/config` | HA select entity discovery |
| `homeassistant/text/spoolsense_*/config` | HA text entity discovery |
