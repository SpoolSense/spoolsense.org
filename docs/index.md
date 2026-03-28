---
hide:
  - navigation
  - toc
---

<style>
.hero {
  text-align: center;
  padding: 4rem 2rem 2rem;
}

.hero .logo img {
  width: 180px;
  margin-bottom: 1rem;
}

.hero h1 {
  font-size: 2.8rem;
  font-weight: 800;
  margin-bottom: 0.5rem;
}

.hero .tagline {
  font-size: 1.25rem;
  opacity: 0.75;
  margin-bottom: 2rem;
}

.hero .cta {
  display: inline-block;
  background: var(--md-primary-fg-color);
  color: var(--md-primary-bg-color);
  padding: 0.7rem 2rem;
  border-radius: 2rem;
  font-weight: 600;
  font-size: 1rem;
  text-decoration: none;
  margin: 0 0.5rem;
}

.hero .cta:hover {
  opacity: 0.9;
  text-decoration: none;
}

.hero .cta-secondary {
  background: transparent;
  border: 2px solid var(--md-primary-fg-color);
  color: var(--md-primary-fg-color);
}

.features {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1.5rem;
  padding: 2rem;
  max-width: 900px;
  margin: 0 auto;
}

.feature {
  padding: 1.5rem;
  border-radius: 12px;
  background: var(--md-code-bg-color);
}

.feature h3 {
  margin-top: 0;
  font-size: 1.1rem;
}

.feature p {
  opacity: 0.8;
  font-size: 0.95rem;
  margin-bottom: 0;
}
</style>

<div class="hero">
  <div class="logo">
    <img src="https://raw.githubusercontent.com/SpoolSense/spoolsense_middleware/master/docs/spoolsense-logo.png" alt="SpoolSense" />
  </div>
  <h1>SpoolSense</h1>
  <p class="tagline">Smart filament tracking for your 3D printer</p>
  <a href="intro/" class="cta">Get Started</a>
  <a href="https://github.com/SpoolSense" class="cta cta-secondary" target="_blank">GitHub</a>
</div>

<div class="features">
  <div class="feature">
    <h3>Scan and Track</h3>
    <p>Place a spool on the reader. Material, color, weight, and manufacturer are identified instantly from the NFC tag.</p>
  </div>
  <div class="feature">
    <h3>Open Source</h3>
    <p>ESP32 firmware, Python middleware, and interactive installer. All GPL-3.0. Build it yourself for under $15.</p>
  </div>
  <div class="feature">
    <h3>Multiple Tag Formats</h3>
    <p>Reads OpenPrintTag, TigerTag, OpenTag3D, and plain UID tags. Auto-detects the format.</p>
  </div>
  <div class="feature">
    <h3>Spoolman Integration</h3>
    <p>Syncs spool data automatically. Register UID tags, track remaining filament, and manage your inventory.</p>
  </div>
  <div class="feature">
    <h3>Home Assistant</h3>
    <p>MQTT auto-discovery. Dashboard sensors for material, color, weight, and scanner status. Build automations.</p>
  </div>
  <div class="feature">
    <h3>Klipper Ready</h3>
    <p>Middleware integrates with AFC, toolchangers, and Moonraker. Assign spools to tools via keypad or macro.</p>
  </div>
</div>
