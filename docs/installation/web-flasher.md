---
hide:
  - toc
---

<style>
.md-content h1 { display: none; }

.flasher-hero {
  text-align: center;
  padding: 2rem 1rem 1rem;
}

.flasher-hero img {
  width: 200px;
  margin-bottom: 0.5rem;
}

.flasher-hero h2 {
  font-size: 1.8rem;
  font-weight: 800;
  margin: 0.5rem 0 0.25rem;
}

.flasher-hero .subtitle {
  font-size: 1.05rem;
  opacity: 0.7;
  margin-bottom: 1.5rem;
}

.flash-btn-wrap {
  text-align: center;
  margin: 1.5rem 0 2rem;
}

esp-web-install-button button {
  background: linear-gradient(180deg, #ef4444, #dc2626);
  color: #fff;
  padding: 1rem 3rem;
  border-radius: 14px;
  font-weight: 800;
  font-size: 1.2rem;
  border: none;
  cursor: pointer;
  transition: opacity 0.15s, transform 0.1s;
}

esp-web-install-button button:hover {
  opacity: 0.9;
  transform: translateY(-2px);
}

.flash-steps {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 14px;
  max-width: 800px;
  margin: 0 auto 2rem;
  padding: 0 1rem;
}

.flash-step {
  text-align: center;
  padding: 20px 14px;
  border: 1px solid #2a2e36;
  border-radius: 14px;
  background: linear-gradient(180deg, #141519, #1a1c21);
}

.flash-step .step-num {
  display: inline-block;
  background: #ef4444;
  color: #fff;
  width: 32px;
  height: 32px;
  line-height: 32px;
  border-radius: 50%;
  font-weight: 800;
  font-size: 0.9rem;
  margin-bottom: 8px;
}

.flash-step .step-icon {
  font-size: 28px;
  margin-bottom: 6px;
}

.flash-step .step-title {
  font-weight: 700;
  font-size: 0.95rem;
  margin-bottom: 4px;
}

.flash-step .step-desc {
  font-size: 0.82rem;
  opacity: 0.65;
}

.flash-info {
  max-width: 700px;
  margin: 0 auto 2rem;
  padding: 0 1rem;
}

.flash-info .info-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px;
}

.flash-info .info-card {
  padding: 16px;
  border: 1px solid #2a2e36;
  border-radius: 12px;
  background: #141519;
}

.flash-info .info-card h4 {
  margin: 0 0 6px;
  font-size: 0.9rem;
  color: #ef4444;
}

.flash-info .info-card p {
  margin: 0;
  font-size: 0.85rem;
  opacity: 0.75;
}

.not-supported {
  display: none;
  max-width: 600px;
  margin: 1rem auto;
  padding: 1rem;
  background: #7f1d1d;
  border-radius: 12px;
  text-align: center;
  font-weight: 600;
}

.browser-note {
  text-align: center;
  font-size: 0.82rem;
  opacity: 0.5;
  margin-top: 0.5rem;
}

@media (max-width: 600px) {
  .flash-info .info-grid {
    grid-template-columns: 1fr;
  }
}
</style>

# Web Flasher

<div class="flasher-hero">
  <img src="https://raw.githubusercontent.com/SpoolSense/spoolsense_middleware/master/docs/spoolsense-logo.png" alt="SpoolSense" />
  <h2>Web Flasher</h2>
  <p class="subtitle">Flash SpoolSense Scanner firmware directly from your browser. No installs needed.</p>
</div>

<div class="flash-steps">
  <div class="flash-step">
    <div class="step-icon">&#128268;</div>
    <div class="step-num">1</div>
    <div class="step-title">Plug In</div>
    <div class="step-desc">Connect your ESP32 via USB</div>
  </div>
  <div class="flash-step">
    <div class="step-icon">&#9889;</div>
    <div class="step-num">2</div>
    <div class="step-title">Flash</div>
    <div class="step-desc">Click the button, pick your port</div>
  </div>
  <div class="flash-step">
    <div class="step-icon">&#128246;</div>
    <div class="step-num">3</div>
    <div class="step-title">Connect</div>
    <div class="step-desc">Join the SpoolSense WiFi hotspot</div>
  </div>
  <div class="flash-step">
    <div class="step-icon">&#9989;</div>
    <div class="step-num">4</div>
    <div class="step-title">Configure</div>
    <div class="step-desc">Enter WiFi, MQTT, and Spoolman settings</div>
  </div>
</div>

<div class="flash-btn-wrap">
  <esp-web-install-button manifest="manifest.json">
    <button slot="activate">Flash SpoolSense Scanner</button>
  </esp-web-install-button>
  <div class="not-supported" id="notSupported">
    Your browser does not support Web Serial. Use <strong>Chrome</strong> or <strong>Edge</strong> on desktop.
  </div>
  <div class="browser-note">Requires Chrome or Edge on desktop. Not supported on Safari or mobile.</div>
</div>

<div class="flash-info">
  <div class="info-grid">
    <div class="info-card">
      <h4>Auto-Detect</h4>
      <p>The flasher detects your board automatically. Works with ESP32-WROOM and ESP32-S3-Zero.</p>
    </div>
    <div class="info-card">
      <h4>Always Latest</h4>
      <p>Pulls the newest firmware from GitHub Releases. No need to download files manually.</p>
    </div>
    <div class="info-card">
      <h4>AP Mode Setup</h4>
      <p>After flashing, the scanner starts a WiFi hotspot. Your phone auto-opens the config page.</p>
    </div>
    <div class="info-card">
      <h4>Already Installed?</h4>
      <p>Use OTA updates at <code>spoolsense.local/update</code> instead. No USB needed after first flash.</p>
    </div>
  </div>
</div>

<script type="module" src="https://unpkg.com/esp-web-tools@10/dist/web/install-button.js?module"></script>

<script>
if (!("serial" in navigator)) {
  document.getElementById("notSupported").style.display = "block";
}
</script>
