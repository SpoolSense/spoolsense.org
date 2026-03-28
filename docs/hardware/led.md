# Optional - Status LED

A status LED provides visual feedback about the scanner's state.

## ESP32-S3-Zero: Built-in

The S3-Zero has an onboard WS2812 RGB LED on GPIO 21. No external wiring needed. It's always available.

## ESP32-WROOM: External SK6812 RGBW

| LED Pin | ESP32 GPIO | Function |
|---------|-----------|----------|
| DIN | GPIO 4 | Data in |
| VCC | 5V | Power (5V recommended for full brightness) |
| GND | GND | Ground |

## LED States

| Color | Meaning |
|-------|---------|
| White | Booting |
| Cyan | WiFi connected |
| Blue | Ready, waiting for tag |
| Filament color | Spool scanned (shows the filament's actual color) |
| White flash (3x) | Tag detected |
| Green flash | Write success |
| Red flash | Write failure or parse error |
| Yellow flash (3x) | Warning |
| Dim white | Black filament (substituted so LED is visibly lit) |

## Enable in Firmware

The LED is enabled via the installer or web config page (`/config` > Hardware > Status LED toggle).

<!-- TODO: Add photo of LED in different states -->
