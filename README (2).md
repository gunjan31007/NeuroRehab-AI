# NeuroRehab AI — Real-Time Computer Vision Telerehabilitation

**Theme / Domain:** BioTech & Health Tech
**Problem Statement:** Omni_BioTech_13 — Home Rehabilitation Tracking for Stroke Patients
**Team:** gunjan31007
**Team Member:** Gunjan Yadav

## Overview

NeuroRehab AI is an in-browser, sensorless physiotherapy tracking system powered by computer vision through any standard webcam. It provides real-time spatial tracking of upper-limb landmarks (shoulder, elbow, wrist) to monitor stroke rehabilitation routines, with dynamic range-of-motion (ROM) calculation and instant visual feedback to correct compensatory posture.

## Problem It Solves

Stroke survivors need consistent, guided physiotherapy to regain motor function, but:
- Transportation and high clinic visit costs are major barriers to regular physical therapy.
- Rural and tier-2/tier-3 regions lack access to specialized neuro-physiotherapists.
- Without supervision, patients often perform exercises with incorrect form, slowing recovery.

NeuroRehab AI bridges this gap by delivering objective, continuous home-session data directly to remote physiotherapists — no clinic visit required.

## Key Differentiators

- **Zero-Hardware Dependency** — runs on everyday consumer webcams; no wearable sensors or IMUs needed.
- **Edge-Only Privacy Architecture** — video frames are analyzed locally in-browser via WebAssembly; no patient video is streamed to external servers.
- **Adaptive Recovery Calibration** — baseline movement angles automatically adapt to each patient's motor impairment level instead of enforcing rigid, generalized standards.

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend & Vision Engine | MediaPipe Pose API, WebGL, HTML5 Canvas, Vanilla JavaScript / TypeScript |
| Kinematics Engine | 3D Euclidean vector algebra for real-time joint-angle calculation |
| Backend & Telemetry | Node.js, Express, Firebase Firestore (encrypted metric storage, clinician dashboards) |

## How It Works

### Joint Angle Calculation

For any joint vertex **B** (e.g. elbow) between adjacent landmarks **A** (shoulder) and **C** (wrist):

```
u = A - B = (xA - xB, yA - yB, zA - zB)
v = C - B = (xC - xB, yC - yB, zC - zB)

θ = arccos( (u · v) / (‖u‖ ‖v‖) ) × (180° / π)
```

### Repetition Counting State Pipeline

1. **State 0 — Rest Baseline:** joint angle θ < θ_threshold_low
2. **State 1 — Ascent & Form Validation:** angle increases toward target θ_target; trunk stability is checked to flag compensatory shoulder-shrug errors
3. **State 2 — Peak Hold:** θ ≥ θ_target maintained for ≥ 200 ms; peak extension logged
4. **State 3 — Descent & Rep Completion:** angle returns below θ_threshold_low → repetition count incremented (R = R + 1) and telemetry dispatched

## Feasibility & Viability

- Runs on lightweight browser-level ML pipelines, executing in under 15 ms per frame on standard laptop and smartphone hardware.
- Requires zero installation or driver configuration — important for non-technical, elderly stroke patients.

### Challenges & Mitigations

| Challenge | Mitigation |
|---|---|
| Partial occlusions / poor camera angles | Smart framing calibration — visual boundary bounding boxes ensure full upper-torso visibility before tracking starts |
| Lighting fluctuations reducing landmark confidence | Confidence-weighted filtering — frames with keypoint confidence < 0.7 are discarded |
| Compensatory movements (spine twist, shoulder hike) | Bi-axial trunk isolation — tracks clavicle and hip alignment simultaneously to detect and flag trunk compensation |

## Impact & Benefits

**Target Audience:** Post-stroke patients regaining upper-extremity motor skills, clinical physiotherapists, and home-care providers.

**Clinical & Social Impact:**
- Promotes neuroplasticity through consistent, guided daily repetitions without hospital visits.
- Delivers accessible telerehabilitation to rural and underserved regions.

**Economic Viability:**
- Drastically reduces out-of-pocket travel and continuous clinical consultation expenses.
- Multiplies therapist efficiency — one clinician can oversee dozens of recovering patients via asynchronous metric tracking.

## Getting Started

```bash
# Clone the repository
git clone <your-repo-url>
cd <repo-folder>

# Install dependencies
npm install

# Run locally
npm start
```

Then open the app in a browser with webcam access enabled.

## Research & References

- Lugassy, M., et al. (2020). *Computer vision-based telerehabilitation systems for stroke recovery: Clinical validation and kinematic accuracy.*
- Bazarevsky, V., et al., Google Research (2020). *MediaPipe: Fast, on-device real-time pose and hand landmark estimation.*
- World Health Organization (WHO). *Global guidelines on physical activity and sedentary behaviour: Rehabilitation for neurological conditions.*
- Clinical Assessment Framework: Functional scoring aligned with the Upper Extremity Fugl-Meyer Assessment (FMA-UE) scale.

## Team

- **Gunjan Yadav** (gunjan31007)
