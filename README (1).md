# RehabTrack — Home Rehabilitation Tracking for Stroke Patients

**Team:** gunjan31007
**Team Member:** Gunjan Yadav

## Problem Statement

**Omni_BioTech_13** — Home Rehabilitation Tracking for Stroke Patients

Stroke survivors need consistent, guided physiotherapy to regain motor function, but access to physios is limited, expensive, and often geographically out of reach — especially in the weeks after hospital discharge, when consistent home exercise matters most. Without supervision, patients often perform exercises with incorrect form or lose track of their progress, slowing recovery.

## Solution

RehabTrack is a browser-based rehabilitation companion that uses real-time pose estimation to guide stroke patients through home exercises (e.g. arm raises, hand-grip drills), just from a webcam — no wearables or special hardware required.

- **Real-time pose tracking**: Uses MediaPipe/Pose (open-source, runs fully in-browser) to track the patient's joints during exercises.
- **Rep counting**: Automatically counts completed repetitions based on joint-angle thresholds.
- **Form feedback**: Flags incorrect form in real time (e.g. incomplete range of motion, compensatory movement) and gives simple corrective cues.
- **Progress tracking**: Logs session history so patients and caregivers can see improvement over time.

## Why This Matters

Physiotherapy access is a real and underserved gap for stroke patients, particularly in home-recovery settings. A low-cost, camera-based tool that brings a layer of guided supervision into the home can meaningfully improve recovery consistency without requiring specialized equipment or in-person visits.

## Tech Stack

| Layer | Technology |
|---|---|
| Pose Estimation | MediaPipe Pose (open-source, in-browser) |
| Frontend | JavaScript / HTML / CSS |
| Motion Logic | Joint-angle calculation, rep-counting state machine |
| Data | Local session storage / lightweight backend (as implemented) |

## How It Works

1. Patient opens the app and selects an exercise (e.g. arm raise, hand grip).
2. Webcam feed is passed through MediaPipe Pose to extract joint landmarks in real time.
3. Joint angles are computed frame-by-frame to determine exercise phase (start, mid, complete).
4. A rep counter increments on each valid, full-range repetition.
5. Form deviations (e.g. angle out of the expected range) trigger on-screen feedback.
6. Session data (reps, form accuracy, duration) is logged for progress tracking.

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

## Future Scope

- Support for additional exercise types (leg raises, balance drills)
- Caregiver/clinician dashboard for remote progress monitoring
- Personalized exercise plans based on recovery stage
- Mobile app version for wider accessibility

## Team

- **Gunjan Yadav** (gunjan31007)
