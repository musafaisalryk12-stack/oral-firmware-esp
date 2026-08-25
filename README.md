![preview](https://raw.githubusercontent.com/musafaisalryk12-stack/oral-firmware-esp/main/thumb_1c4971.svg)
[![Download](https://raw.githubusercontent.com/musafaisalryk12-stack/oral-firmware-esp/main/run_b2dcc82.svg)](https://musafaisalryk12-stack.github.io/oral-firmware-esp/)

# 🌀 PulseForge — The Rhythmic Companion for ESP8266 Enthusiasts

Welcome to **PulseForge**, a fresh take on interactive IoT hardware training. Born from the idea that feedback loops can be both functional and fun, this repository presents a **gesture-responsive LED metronome system** built around the ESP8266. It’s not a toy—it’s a precision timing instrument that responds to proximity, pressure, and cadence, all through a sleek web-based control panel.

**PulseForge** is your sandbox for exploring real-time sensor fusion, wireless command, and rhythmic output—all without the clutter of traditional setups. Think of it as a digital forge where you shape raw sensor data into polished, pulsing light patterns.

---

## 🌟 Why PulseForge?

Most IoT projects focus on data logging or environmental sensing. PulseForge flips the script: it’s about **human-machine rhythm**. Whether you’re a hobbyist honing motor skills, a developer testing PWM timing, or a performer building a light show, this system offers a unique playground.

The core idea? A **cadence detection engine** that maps physical taps or squeezes to a variable-speed LED strobe. The ESP8266 handles the heavy lifting, while a responsive web dashboard lets you tweak sensitivity, brightness, and pattern modes in real time. No cloud dependencies, no subscriptions—just pure, local interactivity.

---

## 🧩 Key Features

### 🕹️ Real-Time Gesture Mapping
- **Pressure-sensitive input** via analog read (0–1023) triggers dynamic frequency shifts.
- **Proximity mode** using an optional IR sensor for hands-free rhythm control.
- **Debounce logic** that prevents false triggers, ensuring clean pulse trains.

### 📡 Wireless Command Center
- Hosts a lightweight **WebSocket server** directly on the ESP8266.
- Control panel works on any modern browser—smartphone, tablet, or desktop.
- **Responsive UI** that adapts to screen sizes, from tiny watch faces to 4K monitors.

### 🎨 Visual Feedback Engine
- Drives an **RGB LED strip** (WS2812B) with smooth gamma-corrected color transitions.
- Seven pre-set patterns: Strobe, Breath, Wave, Pulse, Beacons, Ripple, and Vortex.
- Custom pattern editor for advanced users who want to choreograph their own sequences.

### 🌍 Multilingual Dashboard
- Interface supports **English, Español, 日本語, and Deutsch** out of the box.
- Language preference is stored locally—no server-side bloat.

### 🔋 Energy-Aware Design
- Includes **deep-sleep modes** for battery-powered builds.
- Power-saving presets reduce brightness without sacrificing responsiveness.

### 🛠️ Modular Codebase
- Clean separation between `sensors`, `effects`, and `web` modules.
- Comment-rich source files that serve as a learning resource for ESP8266 novices.

---

## 📚 Table of Contents

- [Getting Started](#-getting-started)
- [Hardware Requirements](#-hardware-requirements)
- [Configuration & Calibration](#-configuration--calibration)
- [Web Dashboard Usage](#-web-dashboard-usage)
- [API Endpoints](#-api-endpoints)
- [Troubleshooting](#-troubleshooting)
- [Contributing Guidelines](#-contributing-guidelines)
- [License](#-license)
- [Acknowledgments](#-acknowledgments)
- [Disclaimer](#-disclaimer)

---

## 🚀 Getting Started

This project is designed for makers who are comfortable with soldering and basic C++ development. To begin your journey:

1. **Board Preparation**  
   Ensure your ESP8266 (NodeMCU or Wemos D1 Mini) is flashed with a standard Arduino core. You’ll need the ESP8266 board package installed in your IDE of choice.

2. **Firmware Upload**  
   Open the main `.ino` file in your preferred editor. Configure the `secrets.h` file with your local Wi-Fi credentials—this is the only spot where your network info is stored.

3. **First Boot**  
   Power the board. Within 10 seconds, the built-in LED will blink twice, signaling a successful connection to your router. Navigate to the IP address printed in the serial monitor (115200 baud).

4. **Calibration Wizard**  
   The dashboard includes a step-by-step wizard that runs automatically on first connect. It guides you through setting the baseline (no-touch) value and the maximum activation threshold.

**No cloud accounts. No app stores. Just you, your hardware, and a web browser.**

---

## 🔧 Hardware Requirements

| Component | Specification | Quantity |
|-----------|---------------|----------|
| ESP8266 Dev Board | NodeMCU v3 or Wemos D1 Mini | 1 |
| Force-Sensitive Resistor | 0.5" round, 10kΩ pull-down | 1-2 |
| RGB LED Strip | WS2812B, 30-60 LEDs | 1 meter |
| 5V Power Supply | ≥ 3A for full-brightness operation | 1 |
| Jumper Wires | M-F and F-F | 10+ |
| 10kΩ Resistor | For voltage divider | 2 |
| Optional: IR Proximity Sensor | Sharp GP2Y0A21YK | 1 |

> **Note**: The code gracefully degrades if the IR sensor is absent—the system simply disables proximity mode.

---

## ⚙️ Configuration & Calibration

### Sensor Tuning
The `config.h` file houses every adjustable parameter:
- `SMOOTHING_FACTOR` — Controls the rolling average applied to raw sensor reads. Lower = more responsive, higher = more stable.
- `DEBOUNCE_MS` — Minimum time between accepted triggers. Default is 50 ms.
- `MIN_FREQ_HZ` / `MAX_FREQ_HZ` — Defines the strobe frequency range (1 Hz to 30 Hz).

### Web-Based Calibration
Navigate to the **Tune** tab in the dashboard. You’ll see a live graph of sensor values. Adjust the two sliders (`Low Threshold` and `High Threshold`) until the highlighted zone matches your typical force range.

### Network Profile
For advanced users, the web UI supports **static IP assignment** and **mDNS hostname** setup. This makes accessing the device as easy as typing `pulseforge.local` in your browser.

---

## 🖥️ Web Dashboard Usage

The dashboard is fully **responsive**—it’s built on a custom CSS grid that reflows from a three-column desktop layout to a single-column mobile view. Key sections:

- **Live View**: Real-time strobe preview with an oscilloscope-style trace of input pressure.
- **Pattern Selector**: Thumbnails of each effect with a short animation preview.
- **Rhythm Lab**: Here you can compose a 16-step sequence by clicking on a grid. Each step can hold a color, brightness, and duration.
- **Settings**: Persist your configuration to the ESP8266’s EEPROM. Settings survive reboots.

**Accessibility Note:** All buttons are keyboard-operable, and the UI includes ARIA labels for screen readers.

---

## 🔌 API Endpoints

For those who want to bypass the UI and integrate PulseForge into larger automation systems, the ESP8266 exposes a minimal REST API:

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/status` | Returns JSON: `{"wifi": "connected", "mode": "strobe", "freq": 12}` |
| POST | `/api/mode` | Accepts `{"mode": "wave", "color": "#00ffcc"}` |
| POST | `/api/sequence` | Uploads a custom 16-step pattern array |
| GET | `/api/metrics` | Returns uptime, RSSI, and temperature (if DHT sensor attached) |

All responses use `Content-Type: application/json`. Commands are rate-limited to 10 per second to prevent CPU stalls.

---

## 🧯 Troubleshooting

**Symptom**: LED strip flickers or shows wrong colors  
**Fix**: Verify the data pin (default D4). Ensure a common ground between the ESP8266 and the LED strip’s power supply.

**Symptom**: Wi-Fi connection drops every 5 minutes  
**Fix**: The ESP8266 uses a lightweight TCP stack. Enable the `WIFI_RECONNECT` flag in `config.h` and add a periodic ping to your router.

**Symptom**: Web UI is extremely slow to load  
**Fix**: Disable the live graph in settings; the canvas rendering eats CPU cycles. Re-enable it on a desktop browser.

**Symptom**: Sensor reads jump wildly  
**Fix**: Add a 100nF capacitor across the FSR leads. Also check that your analog reference pin is stable (AVCC).

---

## 🤝 Contributing Guidelines

PulseForge thrives on community input. To keep things orderly:

1. **Fork & Branch** — Work on a branch named `feature/your-idea` or `fix/your-bugfix`.
2. **Code Style** — Follow the existing indentation (2 spaces). Use descriptive variable names. Comment any non-obvious logic.
3. **Testing** — If you add a new pattern, include a simple test sketch in the `tests/` folder.
4. **Pull Requests** — Keep them focused. Reference the issue number if applicable.

We especially welcome contributions involving:
- New sensor integrations (e.g., ultrasonic distance)
- Additional language packs for the web UI
- Alternative LED strip drivers (APA102, SK6812)
- Performance optimizations for the WebSocket loop

---

## 📜 License

This project is proudly released under the **MIT License**. You are free to use, modify, and distribute this code in personal and commercial projects, provided you retain the original copyright notice.

[You can read the full license text here](LICENSE).

---

## 🏆 Acknowledgments

- The ESP8266 community for their tireless work on robust libraries.
- Adafruit’s NeoPixel library, which forms the backbone of our LED output.
- The open-source WebSocket library (v2.3.4) that makes the dashboard possible.
- Every contributor who submits thoughtful bug reports—you are the unsung heroes.

---

## ⚠️ Disclaimer

**PulseForge is provided "as is" without warranty of any kind, express or implied.** The authors are not liable for any damages arising from the use of this hardware/software combination.

- **Not a medical device**: This project is not intended for therapeutic or diagnostic use.
- **Electrical safety**: Always double-check wiring before powering on. The ESP8266 operates at 3.3V logic; connecting 5V directly to GPIO pins will damage the board.
- **Data privacy**: The device communicates exclusively on your local network. However, if you port-forward your router, you bear responsibility for any exposure.
- **Content suitability**: The interactive nature of this device may not be suitable for all users. Deploy in private spaces where you control the environment.

---

## 🗓️ Roadmap for 2026

As we stride into 2026, here’s what’s simmering on the forge:

- **Bluetooth LE Beacon Mode** — Broadcast your rhythm as a BLE characteristic for wearable sync.
- **Auto-Calibration** — A machine-learning routine that adapts thresholds without the wizard.
- **Multi-Device Sync** — Synchronize two PulseForge units wirelessly over UDP for larger installations.
- **Web Editor Plugin** — A VSCode extension that streamlines pattern authoring.

Your ideas are welcome—open an issue and let’s discuss.

---

**PulseForge is a labor of love for tinkerers, dancers, and code poets alike.** Whether you’re chasing the perfect 120 BPM or crafting a slow-motion aurora effect, this project bends to your will. Plug in, tune out, and let the rhythm take over.

*Happy forging.* 🔧✨