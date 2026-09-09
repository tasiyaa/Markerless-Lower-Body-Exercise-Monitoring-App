# Leg-Rehabilitation-Web-App
# 🌱 Mirror Garden

**A browser-based physical rehabilitation game that uses real-time pose detection to turn body movements into a magical, growing garden.**

Players mirror the on-screen gardener's movements — squatting, stepping, balancing — while the app tracks their skeleton through the webcam and scores each exercise. Good movement earns *Garden Energy* that progressively transforms a barren farm plot into a thriving, fully-cropped landscape.

---

## ✨ Features

| Feature | Description |
|---|---|
| **Real-time pose estimation** | Uses [Human.js](https://github.com/vladmandic/human) with the BlazePose model for full-body 3D keypoint tracking via webcam |
| **Mirror skeleton overlay** | Live skeleton drawn over the camera feed, colour-coded green (≥ 70 % match) or blue (< 70 %) |
| **Animated gardener avatar** | Procedurally drawn character with visible joint markers and knee-angle readouts that mirrors the target pose |
| **4 mini-games** | Plant Seeds · Water the Plants · Scarecrow Balance · Flower Dance |
| **Biomechanical metrics** | Pose accuracy, balance stability, movement smoothness, and rep/time counters displayed in a live HUD |
| **Knee valgus detection** | 3D frontal-plane check warns when knees collapse inward during squats |
| **Progressive garden scene** | Canvas-rendered farm grows through 30 levels — soil → sprouts → crops → trees → pond |
| **Persistent progress** | Garden energy and session count saved via `window.storage` (with graceful in-memory fallback) |
| **Adjustable difficulty** | Scarecrow Balance offers Beginner (5 s), Intermediate (10 s), and Advanced (20 s) hold targets |
| **Fully client-side** | No server required — runs entirely in the browser from a single HTML file |

---

## 🎮 Mini-Games

### 🌱 Plant Seeds — Squat Repetitions
Squat down below 110° knee flexion to "plant a seed," then stand back up above 155° to complete the rep. Depth and valgus quality are scored per repetition.

### 💧 Water the Plants — Lateral Weight Shifts
Step side-to-side, shifting your centre of mass past a normalised threshold. Each side reached waters one plant.

### 🦩 Scarecrow Balance — Single-Leg Hold
Lift one foot and hold still for the target duration. Choose 5 s / 10 s / 20 s difficulty. The timer resets if you put your foot down.

### 🌸 Flower Dance — Chained Sequence
A guided flow that cycles through: **Squat → Stand → Step Left → Step Right → Balance**. Completing the full sequence earns a bonus energy reward.

---

## 🚀 Getting Started

### Prerequisites
- A modern browser with **WebGL** support (Chrome, Edge, Firefox, Safari)
- A **webcam** (built-in or external)
- An internet connection on first load (to fetch the Human.js library and BlazePose model weights from CDN)

### Running
1. Open `mirror-garden.html` in your browser.
2. Click **Start Camera** and grant camera permissions.
3. Stand back so your full body is visible in the frame.
4. Select a mini-game and follow the on-screen gardener!

> **Tip:** For the best experience, ensure good lighting and stand roughly 2–3 metres from the camera.

---

## 🏗️ Architecture

The entire application is a **single self-contained HTML file** (~1 044 lines) structured as:

```
mirror-garden.html
├── <style>           — CSS custom properties, responsive grid, card/button styles
├── <body>            — Semantic HTML layout (header, stage, HUD, controls, garden)
└── <script>          — IIFE containing all logic
    ├── State management
    ├── Persistent storage (load/save garden)
    ├── Garden renderer (canvas, 30-level progression)
    ├── Gardener avatar (procedural limb drawing + joint markers)
    ├── Pose detection (Human.js config, angle/valgus computation)
    ├── Mini-game modules (seeds, water, balance, dance)
    └── Game loop & UI wiring
```

### Key Technical Details

- **Pose model:** BlazePose via Human.js (`blazepose.json`) — provides x, y, **z** per keypoint, enabling 3D joint-angle and valgus calculations.
- **Angle computation:** `angleAt(a, b, c)` computes the 3D angle at joint *b* using the vectors to *a* and *c*.
- **Knee valgus:** `kneeValgusOffset()` measures the perpendicular distance of the knee from the hip → ankle line in 3D, normalised by leg length.
- **Rendering:** Three separate `<canvas>` elements for the webcam skeleton overlay, the gardener avatar, and the garden scene — each driven by its own `requestAnimationFrame` loop.
- **Mirroring:** The webcam feed is drawn with `scale(-1, 1)` so the skeleton overlay matches the user's intuitive left/right.

---

## 📁 Project Files

| File | Description |
|---|---|
| `mirror-garden.html` | Main application (single-file, self-contained) |
| `index.html` | Alternate / earlier version of the app |
| `hand.html` | Separate hand-tracking experiment |
| `MirrorGarden-6.pptx` | Project presentation / slide deck |
| `*.pdf` | Reference literature (biomechanics glossary, anthropometrics, segment lengths) |

---

## 🔧 Technologies

- **[Human.js](https://github.com/vladmandic/human)** v3.3.6 — Pose estimation (BlazePose model)
- **HTML5 Canvas** — All rendering (avatar, skeleton overlay, garden scene)
- **Vanilla JavaScript** — No frameworks, no build step
- **CSS Custom Properties** — Theming with a warm earth-tone palette

---

## ⚠️ Limitations & Disclaimers

- The **knee valgus threshold** (0.12 normalised offset) is a starting heuristic and has **not been clinically validated**. It would require calibration against goniometry / motion-capture data before use in a clinical setting.
- Pose accuracy depends heavily on **lighting**, **camera angle**, and **how much of the body is visible**. Performance may degrade in low-light or cluttered backgrounds.
- The app is designed for **desktop/laptop** use with a forward-facing webcam. Mobile support is limited.

---

## 📄 License

This project is part of a research / lab exercise. Please refer to your institution's policies for usage and distribution rights.
