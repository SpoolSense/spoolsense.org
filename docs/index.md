---
hide:
  - navigation
  - toc
---

<style>
.md-content__inner {
  padding-top: 0 !important;
  margin-top: 0 !important;
}

.md-tabs__link--active[href="."] {
  display: none;
}

/* Hide the page title "Home" on the landing page */
.md-content h1 {
  display: none;
}

.hero {
  text-align: center;
  padding: 0 2rem 2rem;
}

.hero .logo img {
  width: 380px;
  margin-bottom: 0;
}

.hero .tagline {
  font-size: 1.1rem;
  opacity: 0.75;
  margin-top: 0.25rem;
  margin-bottom: 1.5rem;
}

.hero .cta {
  display: inline-block;
  background: linear-gradient(180deg, #ef4444, #dc2626);
  color: #fff;
  padding: 0.7rem 2rem;
  border-radius: 12px;
  font-weight: 800;
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
  border: 2px solid #ef4444;
  color: #ef4444;
}

/* Card grid — matches scanner web UI tool-grid / tool-card */
.tool-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 14px;
  padding: 0 2rem 2rem;
  max-width: 900px;
  margin: 0 auto;
}

.tool-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  padding: 24px 18px;
  border: 1px solid #2a2e36;
  border-radius: 16px;
  background: linear-gradient(180deg, #141519, #1a1c21);
  box-shadow: 0 16px 40px rgba(0,0,0,.35);
  text-decoration: none;
  color: #f4f4f5;
  transition: border-color .15s, transform .1s;
}

.tool-card:hover {
  border-color: #ef4444;
  transform: translateY(-2px);
  text-decoration: none;
}

.tool-card .tool-icon {
  font-size: 32px;
}

.tool-card .tool-name {
  font-size: 16px;
  font-weight: 800;
}

.tool-card .tool-desc {
  font-size: 13px;
  color: #a1a1aa;
  text-align: center;
}
</style>

<div class="hero">
  <div class="logo">
    <img src="https://raw.githubusercontent.com/SpoolSense/spoolsense_middleware/master/docs/spoolsense-logo.png" alt="SpoolSense" />
  </div>
  <p class="tagline">Smart filament tracking for your 3D printer</p>
  <a href="intro/" class="cta">Get Started</a>
  <a href="https://github.com/SpoolSense" class="cta cta-secondary" target="_blank">GitHub</a>
</div>

<div class="tool-grid">
  <a href="intro/" class="tool-card">
    <div class="tool-icon">&#128270;</div>
    <div class="tool-name">Scan and Track</div>
    <div class="tool-desc">Place a spool on the reader. Material, color, weight, and manufacturer identified instantly from the NFC tag.</div>
  </a>

  <a href="intro/supported-tags/" class="tool-card">
    <div class="tool-icon">&#127991;</div>
    <div class="tool-name">Multiple Tag Formats</div>
    <div class="tool-desc">Reads OpenPrintTag, TigerTag, OpenTag3D, and NFC+ tags. Auto-detects the format.</div>
  </a>

  <a href="installation/spoolman/" class="tool-card">
    <div class="tool-icon">&#128230;</div>
    <div class="tool-name">Spoolman Integration</div>
    <div class="tool-desc">Syncs spool data automatically. Register NFC+ tags, track remaining filament, manage inventory.</div>
  </a>

  <a href="installation/home-assistant/" class="tool-card">
    <div class="tool-icon"><svg width="36" height="36" viewBox="0 0 240 240" fill="none" xmlns="http://www.w3.org/2000/svg"><path d="M120 0C53.7 0 0 53.7 0 120s53.7 120 120 120 120-53.7 120-120S186.3 0 120 0" fill="#18bcf2"/><path d="M120 40.5L42 100.5v78c0 8.3 6.7 15 15 15h30v-54h66v54h30c8.3 0 15-6.7 15-15v-78L120 40.5z" fill="#fff"/></svg></div>
    <div class="tool-name">Home Assistant</div>
    <div class="tool-desc">MQTT auto-discovery. Dashboard sensors for material, color, weight, and scanner status. Build automations.</div>
  </a>

  <a href="installation/middleware/" class="tool-card">
    <div class="tool-icon"><svg width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="18" cy="18" r="16" fill="#b4202a"/><path d="M18 6l-1.5 8.5L9 12l5.5 5-8.5 1.5L14 22l-5 5.5 8.5-1.5L15 33l3-8 3 8-2.5-7L27 27.5 22 22l8-3.5L21.5 17 27 12l-7.5 2.5L18 6z" fill="#fff"/></svg></div>
    <div class="tool-name">Klipper Ready</div>
    <div class="tool-desc">Middleware integrates with AFC, toolchangers, and Moonraker. Assign spools to tools via keypad or macro.</div>
  </a>

  <a href="getting-started/shopping-list/" class="tool-card">
    <div class="tool-icon"><svg width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg"><circle cx="18" cy="18" r="16" fill="none" stroke="#22c55e" stroke-width="2.5"/><circle cx="18" cy="14" r="5" fill="none" stroke="#22c55e" stroke-width="2.5"/><path d="M18 19v11" stroke="#22c55e" stroke-width="2.5"/><path d="M13 25h10" stroke="#22c55e" stroke-width="2.5"/></svg></div>
    <div class="tool-name">Open Source</div>
    <div class="tool-desc">ESP32 firmware, Python middleware, and interactive installer. All GPL-3.0. Build it yourself for under $15.</div>
  </a>
</div>
