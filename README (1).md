# NeuroRehab AI

**Home rehabilitation tracking for stroke patients — powered by real-time, on-device pose tracking.**

Live demo: `https://yourusername.github.io/neurorehab-ai/` *(update with your actual GitHub Pages link)*

---

## The Problem

Stroke survivors need consistent, guided exercise to regain movement, but regular physiotherapy is often costly, far away, or simply unavailable. In low- and middle-income countries, there are roughly 10 rehabilitation professionals per million people — far below what's needed. Even patients who know rehab exists often stop because services aren't accessible on a regular basis.

## The Solution

NeuroRehab AI turns any laptop or phone camera into a home physiotherapy assistant. It watches a patient perform an arm-raise exercise, counts reps, checks their form in real time, and gives encouraging feedback — all running entirely in the browser, with no video or pose data ever leaving the device.

## Features

- **Live pose tracking** — uses MediaPipe Pose to track body landmarks from the webcam feed in real time
- **Works for either arm** — tracking logic is parameterized by side, not hardcoded to one arm; adding a third exercise is a config change, not a rewrite
- **Calibration step before tracking starts** — shows a positioning outline so the patient is correctly framed before the session begins, instead of guessing and hoping
- **Automatic rep counting** — detects arm-raise repetitions using shoulder-elbow-wrist angle tracking
- **Real-time form feedback** — flags posture issues as they happen, not after the session
- **Voice guidance** — announces rep counts and encouragement out loud (Web Speech API), so the patient doesn't need to keep looking at the screen mid-exercise; can be toggled off in Account settings
- **Real session history** — sessions are saved to the browser's local storage as they're completed, so the weekly chart and history list reflect actual usage, not static placeholder numbers
- **Weekly progress view** — a simple activity chart showing session consistency and form quality
- **Privacy by design** — all pose tracking happens on-device; nothing is uploaded or recorded

## Project Structure

```
index.html    — markup + app logic (kept together deliberately, see note below)
styles.css    — all styling, organized into commented sections
```

**Why the JS lives inside index.html instead of its own file:** browsers block loading external JS module files (`<script type="module" src="...">`) when a page is opened directly from disk (`file://`) — this isn't a quirk of this app, it's a hard security rule with no workaround short of running a local server. Since this prototype needs to be reliably double-click-able for demos, the JS stays embedded as a `<script type="module">` block, organized into clearly labeled, commented sections (persistence, navigation, calibration, pose tracking, voice feedback). CSS has no such restriction, so it's cleanly separated into `styles.css`.

MediaPipe itself is loaded with a **dynamic** `import()` at the moment someone starts an exercise, not a static import at the top of the file — so a flaky connection or blocked CDN only affects the camera feature, not the rest of the app (navigation, progress, account settings all keep working).

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3, vanilla JavaScript (ES modules) — single file, no build step |
| Pose estimation | [MediaPipe Tasks Vision](https://ai.google.dev/edge/mediapipe) — PoseLandmarker (lite model), runs on-device via WASM/GPU |
| Rendering | HTML5 Canvas for the live skeleton overlay |
| Camera access | Browser `MediaDevices.getUserMedia` API |

## Running Locally

No build step needed.

1. Download `index.html` and `styles.css` into the same folder
2. Open `index.html` directly in a modern browser (Chrome or Edge recommended)
3. Allow camera access when prompted, line up with the on-screen guide, then tap "I'm ready"
4. Tap **Start today's session** and follow the on-screen prompts

> Requires an internet connection on first exercise start, to fetch the MediaPipe model from Google's CDN. After that, all tracking runs locally — nothing is sent anywhere. If the connection drops, the rest of the app (navigation, progress, settings) keeps working — only the live tracking screen is affected.

## Design Notes

The visual design deliberately avoids the typical dark, neon "AI app" look. Given the audience — stroke patients doing rehab at home, often tired or frustrated — the interface uses a warm, calm, paper-like palette (rust, forest green, ochre on parchment) instead of clinical white or high-contrast dark mode, to feel more like a supportive companion than a piece of medical tech.

## Roadmap

- [ ] Multi-exercise library (hand grip, shoulder rotation, balance)
- [ ] Caregiver-linked accounts with shareable weekly reports
- [ ] Cloud sync so history survives a cleared browser or a new device (currently stored in `localStorage`, so it's real but device-local)
- [ ] Clinical pilot with physiotherapists and stroke patients
- [ ] Native mobile app

## Team

Built by **Gunjan Yadav** ([@gunjan31007](https://github.com/gunjan31007)) for the Omnikon National Hackathon.
