---
hide:
  - toc
---

<style>
.md-content h1 { display: none; }

.roadmap-hero {
  text-align: center;
  padding: 1.5rem 1rem 0.5rem;
}

.roadmap-hero h2 {
  font-size: 1.8rem;
  font-weight: 800;
  margin: 0 0 0.25rem;
}

.roadmap-hero .subtitle {
  font-size: 1rem;
  opacity: 0.6;
  margin-bottom: 1.5rem;
}

.badge {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 20px;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.02em;
}
.badge-exploring { background: #1e3a5f; color: #60a5fa; }
.badge-planned   { background: #4a2c0a; color: #f59e0b; }
.badge-progress  { background: #1a3a1a; color: #4ade80; }
.badge-done      { background: #14532d; color: #22c55e; }

.roadmap-section {
  max-width: 900px;
  margin: 0 auto 2rem;
  padding: 0 1rem;
}

.roadmap-section h3 {
  margin-bottom: 0.75rem;
}

.roadmap-table {
  width: 100%;
  border-collapse: collapse;
}

.roadmap-table th {
  text-align: left;
  padding: 8px 12px;
  border-bottom: 2px solid #2a2e36;
  font-size: 0.85rem;
  opacity: 0.7;
}

.roadmap-table td {
  padding: 10px 12px;
  border-bottom: 1px solid #1e2028;
  font-size: 0.92rem;
}

.roadmap-table tr:hover {
  background: #141519;
}

.repo-tag {
  display: inline-block;
  padding: 1px 8px;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  background: #1e2028;
  color: #a1a1aa;
}
</style>

# Roadmap

<div class="roadmap-hero">
  <h2>Roadmap</h2>
  <p class="subtitle">What we're building, what's next, and what's done.</p>
</div>

<div class="roadmap-section">

### <span class="badge badge-exploring">Exploring</span>

<table class="roadmap-table">
<tr><th>Feature</th><th>Repo</th><th>Description</th></tr>
<tr><td>Direct Moonraker mode</td><td><span class="repo-tag">Scanner</span></td><td>Scanner talks to Moonraker directly, no middleware needed</td></tr>
<tr><td>Bambu Lab AMS via MQTT</td><td><span class="repo-tag">Scanner</span></td><td>Direct MQTT integration with Bambu printers</td></tr>
<tr><td>Bambu Lab printer support</td><td><span class="repo-tag">Middleware</span></td><td>Local MQTT bridge for Bambu spool tracking</td></tr>
<tr><td>MIFARE Classic authentication</td><td><span class="repo-tag">Scanner</span></td><td>Read Creality CFS and Bambu encrypted tags</td></tr>
<tr><td>ST7789 TFT color display</td><td><span class="repo-tag">Scanner</span></td><td>Replace 16x2 LCD with 240x240 color TFT</td></tr>
<tr><td>Multi-tag detection</td><td><span class="repo-tag">Scanner</span></td><td>Read two spools simultaneously (dual antenna)</td></tr>
<tr><td>Spoolman write to tag</td><td><span class="repo-tag">Middleware</span></td><td>Write spool data from Spoolman directly to a tag</td></tr>
</table>

### <span class="badge badge-planned">Planned</span>

<table class="roadmap-table">
<tr><th>Feature</th><th>Repo</th><th>Target</th></tr>
<tr><td>TFT display support</td><td><span class="repo-tag">Scanner</span></td><td>v1.6.0</td></tr>
<tr><td>WiFi reconnection logic</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>Tag writer: populate from Spoolman</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>NTAG variant detection (GET_VERSION)</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>TigerTag partial write (changed fields only)</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>PN5180 Phase 2 reliability</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>HTTP connection reuse for Spoolman</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>HA publish queue fix (silent drops)</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>Tag writer auto-populate from scanned tag</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>Nozzle/bed temps to AFC lane_data</td><td><span class="repo-tag">Middleware</span></td><td>—</td></tr>
<tr><td>Resync AFC lock state on MQTT reconnect</td><td><span class="repo-tag">Middleware</span></td><td>—</td></tr>
<tr><td>Moonraker websocket (replace polling)</td><td><span class="repo-tag">Middleware</span></td><td>—</td></tr>
<tr><td>Wiring photos and assembly guides</td><td><span class="repo-tag">Docs</span></td><td>—</td></tr>
<tr><td>More community enclosure designs</td><td><span class="repo-tag">Docs</span></td><td>—</td></tr>
</table>

### <span class="badge badge-progress">In Progress</span>

<table class="roadmap-table">
<tr><th>Feature</th><th>Repo</th><th>Notes</th></tr>
<tr><td>Prusa PrusaLink integration</td><td><span class="repo-tag">Scanner</span></td><td>Experimental, looking for testers</td></tr>
<tr><td>Creality rooted printer guide</td><td><span class="repo-tag">Docs</span></td><td>Compatible via Moonraker</td></tr>
</table>

### <span class="badge badge-done">Completed</span>

<table class="roadmap-table">
<tr><th>Feature</th><th>Repo</th><th>Version</th></tr>
<tr><td>AP mode fallback + captive portal</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.10</td></tr>
<tr><td>Web flasher (browser-based flash)</td><td><span class="repo-tag">Docs</span></td><td>v1.5.10</td></tr>
<tr><td>Tag writer dry temp/time auto-populate</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.10</td></tr>
<tr><td>3x4 matrix keypad (tool assignment)</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.9</td></tr>
<tr><td>PN532 NFC reader support</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.9</td></tr>
<tr><td>OpenTag3D read/write</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.5</td></tr>
<tr><td>TigerTag read/write</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.0</td></tr>
<tr><td>NFC+ UID registration (Spoolman)</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.0</td></tr>
<tr><td>Klipper/AFC middleware</td><td><span class="repo-tag">Middleware</span></td><td>v1.5.0</td></tr>
<tr><td>Status LED (SK6812 / WS2812)</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.0</td></tr>
<tr><td>Web-based tag writer (all formats)</td><td><span class="repo-tag">Scanner</span></td><td>v1.5.0</td></tr>
<tr><td>Orca Slicer lane data integration</td><td><span class="repo-tag">Middleware</span></td><td>v1.5.4</td></tr>
<tr><td>Write loop prevention</td><td><span class="repo-tag">Middleware</span></td><td>v1.5.5</td></tr>
<tr><td>Atomic toolhead activation</td><td><span class="repo-tag">Middleware</span></td><td>v1.5.5</td></tr>
<tr><td>16x2 I2C LCD display</td><td><span class="repo-tag">Scanner</span></td><td>v1.4.0</td></tr>
<tr><td>OTA firmware updates</td><td><span class="repo-tag">Scanner</span></td><td>v1.3.0</td></tr>
<tr><td>Spoolman auto-sync</td><td><span class="repo-tag">Scanner</span></td><td>v1.2.0</td></tr>
<tr><td>PN5180 NFC reader support</td><td><span class="repo-tag">Scanner</span></td><td>v1.0.0</td></tr>
<tr><td>OpenPrintTag read/write</td><td><span class="repo-tag">Scanner</span></td><td>v1.0.0</td></tr>
<tr><td>Home Assistant MQTT discovery</td><td><span class="repo-tag">Scanner</span></td><td>v1.0.0</td></tr>
</table>

</div>
