---
hide:
  - toc
---

# Web Flasher

Flash SpoolSense Scanner firmware directly from your browser. No software to install.

<style>
.flash-container {
  text-align: center;
  padding: 2rem 1rem;
}

.flash-container img {
  width: 200px;
  margin-bottom: 1rem;
}

.flash-steps {
  max-width: 600px;
  margin: 2rem auto;
  text-align: left;
}

.flash-steps ol li {
  margin-bottom: 1rem;
  font-size: 1.05rem;
}

esp-web-install-button {
  display: inline-block;
  margin: 1.5rem 0;
}

esp-web-install-button button {
  background: linear-gradient(180deg, #ef4444, #dc2626);
  color: #fff;
  padding: 0.8rem 2.5rem;
  border-radius: 12px;
  font-weight: 800;
  font-size: 1.1rem;
  border: none;
  cursor: pointer;
}

esp-web-install-button button:hover {
  opacity: 0.9;
}

.flash-note {
  max-width: 600px;
  margin: 0 auto;
  font-size: 0.9rem;
  opacity: 0.7;
}

.not-supported {
  display: none;
  max-width: 600px;
  margin: 1rem auto;
  padding: 1rem;
  background: #7f1d1d;
  border-radius: 12px;
  text-align: center;
}
</style>

<div class="flash-container">

<div class="flash-steps">

1. **Plug in** your ESP32 via USB
2. **Click** the flash button below
3. **Select** the serial port when prompted
4. **Wait** for the flash to complete (~30 seconds)
5. **Connect** to the `SpoolSense-XXXX` WiFi network that appears
6. **Configure** WiFi and other settings in the setup page that opens

</div>

<esp-web-install-button manifest="manifest.json">
  <button slot="activate">Flash SpoolSense Scanner</button>
</esp-web-install-button>

<div class="not-supported" id="notSupported">
  Your browser does not support Web Serial. Use <strong>Chrome</strong> or <strong>Edge</strong> on desktop.
</div>

<div class="flash-note">
  <p>The flasher auto-detects your board (ESP32-WROOM or ESP32-S3). Always flashes the latest release.</p>
  <p>Requires <strong>Chrome</strong> or <strong>Edge</strong> on desktop. Web Serial is not supported on mobile browsers or Safari.</p>
</div>

</div>

<script type="module" src="https://unpkg.com/esp-web-tools@10/dist/web/install-button.js?module"></script>

<script>
if (!("serial" in navigator)) {
  document.getElementById("notSupported").style.display = "block";
}
</script>
