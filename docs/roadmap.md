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
  font-size: 1.5rem;
  font-weight: 800;
  margin: 0 0 0.25rem;
}

.roadmap-hero .subtitle {
  font-size: 0.85rem;
  opacity: 0.6;
  margin-bottom: 1rem;
}

/* Stats bar */
.roadmap-stats {
  display: flex;
  justify-content: center;
  gap: 24px;
  margin-bottom: 1.5rem;
  flex-wrap: wrap;
}

.roadmap-stat {
  text-align: center;
}

.roadmap-stat .num {
  font-size: 1.6rem;
  font-weight: 800;
  line-height: 1;
}

.roadmap-stat .label {
  font-size: 0.72rem;
  opacity: 0.5;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.stat-done .num { color: #22c55e; }
.stat-progress .num { color: #4ade80; }
.stat-planned .num { color: #f59e0b; }
.stat-exploring .num { color: #60a5fa; }

/* Section badges */
.badge {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 20px;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.02em;
  vertical-align: middle;
}
.badge-exploring { background: #1e3a5f; color: #60a5fa; }
.badge-planned   { background: #4a2c0a; color: #f59e0b; }
.badge-progress  { background: #1a3a1a; color: #4ade80; }
.badge-done      { background: #14532d; color: #22c55e; }

/* Layout */
.roadmap-section {
  max-width: 860px;
  margin: 0 auto 1.5rem;
  padding: 0 1rem;
}

.roadmap-section h3 {
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
}

/* Cards for exploring */
.explore-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
  margin-bottom: 0.5rem;
}

.explore-card {
  padding: 14px;
  border: 1px solid #2a2e36;
  border-radius: 12px;
  background: linear-gradient(180deg, #141519, #1a1c21);
  transition: border-color 0.15s;
}

.explore-card:hover {
  border-color: #60a5fa;
}

.explore-card .card-title {
  font-weight: 700;
  font-size: 0.82rem;
  margin-bottom: 4px;
}

.explore-card .card-repo {
  display: inline-block;
  padding: 1px 6px;
  border-radius: 4px;
  font-size: 0.65rem;
  font-weight: 600;
  background: #1e2028;
  color: #a1a1aa;
  margin-bottom: 4px;
}

.explore-card .card-desc {
  font-size: 0.75rem;
  opacity: 0.6;
  line-height: 1.4;
}

/* Tables */
.roadmap-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.82rem;
}

.roadmap-table th {
  text-align: left;
  padding: 6px 10px;
  border-bottom: 2px solid #2a2e36;
  font-size: 0.72rem;
  opacity: 0.6;
  text-transform: uppercase;
  letter-spacing: 0.03em;
}

.roadmap-table td {
  padding: 8px 10px;
  border-bottom: 1px solid #1e2028;
}

.roadmap-table tr:hover td {
  background: #141519;
}

.repo-tag {
  display: inline-block;
  padding: 1px 6px;
  border-radius: 4px;
  font-size: 0.68rem;
  font-weight: 600;
  background: #1e2028;
  color: #a1a1aa;
}

.version-tag {
  display: inline-block;
  padding: 1px 6px;
  border-radius: 4px;
  font-size: 0.68rem;
  font-weight: 600;
  background: #14532d;
  color: #22c55e;
}

/* Completed: muted rows */
.roadmap-table.completed td {
  opacity: 0.65;
}
.roadmap-table.completed tr:hover td {
  opacity: 1;
}
</style>

# Roadmap

<div class="roadmap-hero">
  <h2>Roadmap</h2>
  <p class="subtitle">What we're building, what's next, and what's shipped.</p>
</div>

<div class="roadmap-stats">
  <div class="roadmap-stat stat-exploring"><div class="num">7</div><div class="label">Exploring</div></div>
  <div class="roadmap-stat stat-planned"><div class="num">17</div><div class="label">Planned</div></div>
  <div class="roadmap-stat stat-progress"><div class="num">2</div><div class="label">In Progress</div></div>
  <div class="roadmap-stat stat-done"><div class="num">20</div><div class="label">Shipped</div></div>
</div>

<div class="roadmap-section">

### <span class="badge badge-exploring">Exploring</span>

<div class="explore-grid">
  <div class="explore-card">
    <div class="card-repo">Scanner</div>
    <div class="card-title">Direct Moonraker Mode</div>
    <div class="card-desc">Scanner talks to Moonraker directly, no middleware needed</div>
  </div>
  <div class="explore-card">
    <div class="card-repo">Scanner</div>
    <div class="card-title">Bambu Lab AMS via MQTT</div>
    <div class="card-desc">Direct MQTT integration with Bambu printers</div>
  </div>
  <div class="explore-card">
    <div class="card-repo">Middleware</div>
    <div class="card-title">Bambu Lab Printer Support</div>
    <div class="card-desc">Local MQTT bridge for Bambu spool tracking</div>
  </div>
  <div class="explore-card">
    <div class="card-repo">Scanner</div>
    <div class="card-title">MIFARE Classic Auth</div>
    <div class="card-desc">Read Creality CFS and Bambu encrypted tags</div>
  </div>
  <div class="explore-card">
    <div class="card-repo">Scanner</div>
    <div class="card-title">ST7789 TFT Color Display</div>
    <div class="card-desc">Replace 16x2 LCD with 240x240 color TFT</div>
  </div>
  <div class="explore-card">
    <div class="card-repo">Scanner</div>
    <div class="card-title">Multi-Tag Detection</div>
    <div class="card-desc">Read two spools simultaneously (dual antenna)</div>
  </div>
  <div class="explore-card">
    <div class="card-repo">Middleware</div>
    <div class="card-title">Bondtech INDX</div>
    <div class="card-desc">Support up to 8 toolheads (retail Q2 2026)</div>
  </div>
</div>

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
<tr><td>Klipper error alerts via LED (per-toolhead)</td><td><span class="repo-tag">Scanner</span></td><td>—</td></tr>
<tr><td>Smarter Spoolman lookups (filter by NFC ID)</td><td><span class="repo-tag">Middleware</span></td><td>—</td></tr>
<tr><td>Low spool push notification (HA)</td><td><span class="repo-tag">Middleware</span></td><td>—</td></tr>
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

### <span class="badge badge-done">Shipped</span>

<table class="roadmap-table completed">
<tr><th>Feature</th><th>Repo</th><th>Version</th></tr>
<tr><td>AP mode fallback + captive portal</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.10</span></td></tr>
<tr><td>Web flasher (browser-based flash)</td><td><span class="repo-tag">Docs</span></td><td><span class="version-tag">v1.5.10</span></td></tr>
<tr><td>Tag writer dry temp/time auto-populate</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.10</span></td></tr>
<tr><td>3x4 matrix keypad (tool assignment)</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.9</span></td></tr>
<tr><td>PN532 NFC reader support</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.9</span></td></tr>
<tr><td>Tag writeback (remaining weight sync)</td><td><span class="repo-tag">Middleware</span></td><td><span class="version-tag">v1.5.5</span></td></tr>
<tr><td>Write loop prevention</td><td><span class="repo-tag">Middleware</span></td><td><span class="version-tag">v1.5.5</span></td></tr>
<tr><td>Atomic toolhead activation</td><td><span class="repo-tag">Middleware</span></td><td><span class="version-tag">v1.5.5</span></td></tr>
<tr><td>OpenTag3D read/write</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.5</span></td></tr>
<tr><td>Orca Slicer lane data integration</td><td><span class="repo-tag">Middleware</span></td><td><span class="version-tag">v1.5.4</span></td></tr>
<tr><td>TigerTag read/write</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.0</span></td></tr>
<tr><td>NFC+ UID registration (Spoolman)</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.0</span></td></tr>
<tr><td>Klipper/AFC middleware</td><td><span class="repo-tag">Middleware</span></td><td><span class="version-tag">v1.5.0</span></td></tr>
<tr><td>Status LED (SK6812 / WS2812)</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.0</span></td></tr>
<tr><td>Web-based tag writer (all formats)</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.5.0</span></td></tr>
<tr><td>16x2 I2C LCD display</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.4.0</span></td></tr>
<tr><td>OTA firmware updates</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.3.0</span></td></tr>
<tr><td>Spoolman auto-sync</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.2.0</span></td></tr>
<tr><td>PN5180 NFC reader support</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.0.0</span></td></tr>
<tr><td>Home Assistant MQTT discovery</td><td><span class="repo-tag">Scanner</span></td><td><span class="version-tag">v1.0.0</span></td></tr>
</table>

</div>
