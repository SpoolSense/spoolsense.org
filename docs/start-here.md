# Build Your First SpoolSense Scanner

This is the shortest path from no hardware to your first successful spool scan. You do not need a display, keypad, middleware, Home Assistant, or a special printer to get started.

<div class="quick-facts" markdown>
  <div><strong>Cost</strong><span>About $8–20</span></div>
  <div><strong>Build time</strong><span>30–60 minutes</span></div>
  <div><strong>Difficulty</strong><span>Beginner-friendly</span></div>
  <div><strong>Soldering</strong><span>Usually not required</span></div>
</div>

## Before you buy anything

SpoolSense can always scan and identify compatible NFC tags. Connecting the scan to a printer depends on your printer and firmware.

[Check printer compatibility →](resources/compatibility.md){ .button .button--secondary }

## The recommended beginner build

You can choose among several supported boards and readers later. For a first build, use this combination:

| Part | Recommended choice | Why |
|---|---|---|
| Microcontroller | **ESP32-WROOM DevKit** | Common, inexpensive, and enough pins for future extras |
| NFC reader | **PN5180** | Supports every SpoolSense tag format |
| First tags | **NTAG215 stickers** | Cheap and supported by both reader types |
| Connections | **8 female-to-female jumper wires** | No breadboard required |
| Power | **A data-capable USB cable** | Used for flashing and power |

!!! tip "Already own different hardware?"
    That may work too. See [all supported boards](getting-started/choose-board.md) and [reader options](getting-started/choose-reader.md) before buying replacements.

## Your path to the first scan

<div class="journey-list">
  <div class="journey-item">
    <span>1</span>
    <div>
      <h3>Get the five parts above</h3>
      <p>Use the <a href="../getting-started/shopping-list/">full shopping list</a> for search terms and optional alternatives.</p>
    </div>
  </div>
  <div class="journey-item">
    <span>2</span>
    <div>
      <h3>Connect the PN5180 to the ESP32</h3>
      <p>Follow the pin-by-pin <a href="../hardware/wiring-pn5180/">PN5180 wiring guide</a>. Double-check 3.3V and ground before connecting USB.</p>
    </div>
  </div>
  <div class="journey-item">
    <span>3</span>
    <div>
      <h3>Flash and configure the scanner</h3>
      <p>Open the <a href="../installation/web-flasher/">web flasher</a> in Chrome or Edge, select ESP32-WROOM, and follow the Wi-Fi setup.</p>
    </div>
  </div>
  <div class="journey-item">
    <span>4</span>
    <div>
      <h3>Scan your first tag</h3>
      <p>Place an NTAG215 sticker over the reader. The scanner web interface will show its UID and let you register it.</p>
    </div>
  </div>
</div>

## Choose what to connect next

Your scanner is useful on its own. Pick only the next step that matches what you want to accomplish.

<div class="path-grid">
  <a class="path-card" href="../installation/spoolman/">
    <span class="path-card__label">Inventory</span>
    <strong>Track spools with Spoolman</strong>
    <p>Register tags and keep filament details and remaining weight in one place.</p>
    <span>Set up Spoolman →</span>
  </a>
  <a class="path-card" href="../installation/home-assistant/">
    <span class="path-card__label">Dashboard</span>
    <strong>Add Home Assistant</strong>
    <p>Expose material, color, weight, and scanner status as dashboard sensors.</p>
    <span>Connect Home Assistant →</span>
  </a>
  <a class="path-card" href="../installation/middleware/">
    <span class="path-card__label">Printer</span>
    <strong>Connect Klipper or AFC</strong>
    <p>Assign spools to tools or lanes and send filament information to your printer.</p>
    <span>Set up printer integration →</span>
  </a>
</div>

## Add extras only when you want them

A TFT display, character LCD, status LED, and keypad are optional. Once the basic scanner works, use the [extras guide](getting-started/choose-extras.md) to decide whether any of them improve your workflow.

!!! question "Need help?"
    Visit [Troubleshooting](resources/troubleshooting.md), ask in the [SpoolSense Discord](https://discord.gg/pbXJhKpzd2), or open an issue on [GitHub](https://github.com/SpoolSense).
