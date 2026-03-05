# Hand Gesture 3D Model Controller

An interactive web application that uses hand gesture recognition to control and manipulate 3D models in real-time. Built with MediaPipe hand tracking and Three.js for WebGL rendering.

![Demo](https://img.shields.io/badge/Demo-Interactive-brightgreen) ![Tech](https://img.shields.io/badge/Tech-WebGL%20%7C%20MediaPipe-blue) ![License](https://img.shields.io/badge/License-MIT-yellow)

## 🎯 Overview

This project creates an immersive augmented reality experience where users can:

- Summon 3D models using hand gestures
- Watch models form from magical particle effects
- Rotate, scale, and switch between multiple models
- All running locally in a web browser with just a webcam

## 🛠️ Technologies Used

### Core Technologies

| Technology                   | Purpose                                        | Version        |
| ---------------------------- | ---------------------------------------------- | -------------- |
| **MediaPipe Hands**    | Real-time hand tracking and landmark detection | 0.4.1675469240 |
| **Three.js**           | 3D rendering and WebGL abstraction             | 0.160.0        |
| **Vanilla JavaScript** | Core application logic                         | ES6+ Modules   |
| **HTML5 Canvas**       | Hand landmark visualization overlay            | -              |
| **WebRTC**             | Webcam video capture                           | -              |

### Libraries & CDN Resources

```
MediaPipe Hands - Hand landmark detection (21 points per hand)
MediaPipe Camera Utils - Webcam stream management
MediaPipe Drawing Utils - Landmark visualization helpers
Three.js Core - 3D scene, camera, renderer
Three.js GLTFLoader - Loading .glb 3D model files
```

## ✨ Features

### 1. **Particle Formation Effect**

- 2000 glowing particles animate from a dispersed sphere into the model's shape
- Smooth ease-in-out cubic easing with swirling motion
- 2-second formation animation with progress indicator

### 2. **Hand Gesture Controls**

- Real-time finger counting algorithm
- Support for both left and right hand tracking
- Visual feedback with color-coded indicators

### 3. **3D Model Manipulation**

- Load and display GLB/GLTF 3D models
- Smooth scale interpolation with pinch gestures
- Dual-hand rotation control
- Auto-rotation when idle

### 4. **Mirrored Webcam Feed**

- Full-screen webcam display (mirrored for natural interaction)
- Transparent Three.js overlay for 3D content
- Hand landmark skeleton visualization

## 🎮 Gesture Controls

| Gesture                                    | Hand(s) | Action                                | Visual Indicator |
| ------------------------------------------ | ------- | ------------------------------------- | ---------------- |
| ✊**Fist** (0 fingers)               | Left    | Show first model with particle effect | Green            |
| ✋**3 Fingers**                      | Left    | Cycle to next model                   | Gold             |
| 🤏**Pinch** (thumb + index)          | Right   | Scale model (spread = bigger)         | Gold line        |
| 🖐️🖐️**Open Palms** (5+5 fingers) | Both    | Rotate model by moving hands          | Purple           |

## 🔧 Technical Implementation

### Hand Landmark Detection

MediaPipe Hands detects 21 landmarks per hand:

```
        8   12  16  20
        |   |   |   |
    4   7   11  15  19
    |   |   |   |   |
    3   6   10  14  18
    |   |   |   |   |
    2   5   9   13  17
     \  |   |   |   /
      \ |   |   |  /
        +---+---+
            |
            0 (wrist)
```

**Finger Counting Algorithm:**

```javascript
// A finger is "extended" if its tip is above its PIP joint
// Index: tip (8) above PIP (6)
if (landmarks[8].y < landmarks[6].y) count++;

// Thumb uses horizontal distance from palm instead
if (Math.abs(thumbTip.x - thumbMCP.x) > Math.abs(thumbIP.x - thumbMCP.x)) count++;
```

### Particle System

The particle formation effect works in three phases:

1. **Idle State**: Particles swirl in a sphere (radius 3-6 units)
2. **Formation**: Particles move toward model vertices using lerp interpolation
3. **Completion**: Particles fade, real model appears

```javascript
// Easing function for smooth animation
const eased = progress < 0.5
    ? 4 * progress * progress * progress
    : 1 - Math.pow(-2 * progress + 2, 3) / 2;

// Interpolate with swirl effect
position = start + (target - start) * eased + swirlOffset;
```

### Scale Smoothing

To prevent jittery scaling, two smoothing techniques are applied:

1. **Moving Average**: Last 5 pinch distances averaged
2. **Lerp Interpolation**: `currentScale += (targetScale - currentScale) * 0.15`

### Rotation Control

Both hands open (5 fingers each) activates rotation mode:

- Horizontal hand movement → Y-axis rotation
- Vertical hand movement → X-axis rotation
- Average of both hands' positions for stability

## 📁 Project Structure

```
Hand Gesture 3D Models/
├── index.html          # Main application (single file)
├── README.md           # This documentation
└── image_models/       # 3D model assets
    ├── turbine__turbofan_engine__jet_engine.glb
    ├── ksp_primitive_orbital_station_complex.glb
    └── iss.glb
```

## 🚀 Getting Started

### Prerequisites

- Modern web browser (Chrome, Firefox, Edge recommended)
- Webcam
- Local web server (required for loading 3D models)

### Running Locally

1. **Clone or download** the project
2. **Start a local server** (choose one):

   ```bash
   # Python 3
   cd "Hand Gesture 3D Models"
   python3 -m http.server 8000

   # Node.js (if http-server installed)
   npx http-server -p 8000

   # PHP
   php -S localhost:8000
   ```
3. **Open in browser**: Navigate to `http://localhost:8000`
4. **Allow camera access** when prompted
5. **Make a fist** with your left hand to summon the first model!

## ⚙️ Configuration

Key parameters can be adjusted in the code:

```javascript
// Particle System
const particleCount = 2000;           // Number of particles
const formationDuration = 2.0;        // Seconds for formation animation

// Smoothing
const scaleSmoothingFactor = 0.15;    // Scale lerp speed (0-1)
const rotationSmoothingFactor = 0.12; // Rotation lerp speed (0-1)
const pinchHistorySize = 5;           // Moving average window

// Gesture Thresholds
const pinchStartThreshold = 0.08;     // Pinch detection distance
const pinchEndThreshold = 0.25;       // Pinch release distance

// Model Scale Limits
const minScale = 0.3;
const maxScale = 5.0;
```

## 🎨 Visual Indicators

| Color               | Meaning                            |
| ------------------- | ---------------------------------- |
| 🔴 Red (#FF6B6B)    | Left hand detected (default)       |
| 🔵 Teal (#4ECDC4)   | Right hand detected                |
| 🟢 Green (#00FF00)  | Fist detected, ready to summon     |
| 🟡 Gold (#FFD700)   | Active gesture (switching/scaling) |
| 🟣 Purple (#9B59B6) | Rotation mode active               |
| 🔵 Cyan (#00FFFF)   | Particle formation in progress     |

## 🔄 Application Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    APPLICATION START                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  1. Initialize Three.js scene, camera, renderer             │
│  2. Load all 3D models (hidden)                             │
│  3. Create particle system (visible, swirling)              │
│  4. Initialize MediaPipe hand tracking                      │
│  5. Start webcam feed                                       │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    WAITING STATE                             │
│  - Particles swirl in idle animation                        │
│  - Status: "Make a FIST to start"                           │
└─────────────────────────────────────────────────────────────┘
                              │
                    Left Fist Detected
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                 PARTICLE FORMATION                           │
│  - Extract vertices from target model                       │
│  - Animate particles toward vertices (2 seconds)            │
│  - Status: "FORMING... XX%"                                 │
└─────────────────────────────────────────────────────────────┘
                              │
                    Formation Complete
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    ACTIVE STATE                              │
│  - Model visible with auto-rotation                         │
│  - Ready for gestures: scale, rotate, cycle                 │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              │               │               │
     Left 3 Fingers    Right Pinch    Both Hands Open
              │               │               │
              ▼               ▼               ▼
        Cycle Model     Scale Model    Rotate Model
```

## 🐛 Troubleshooting

| Issue                   | Solution                                             |
| ----------------------- | ---------------------------------------------------- |
| Camera not working      | Check browser permissions, ensure HTTPS or localhost |
| Models not loading      | Must run from local server, not file:// protocol     |
| Hand not detected       | Ensure good lighting, keep hands in frame            |
| Laggy performance       | Close other tabs, reduce `particleCount`           |
| Gestures not recognized | Keep fingers clearly separated, palm facing camera   |

## 📝 Browser Compatibility

| Browser         | Support                                |
| --------------- | -------------------------------------- |
| Chrome 90+      | ✅ Full support                        |
| Firefox 85+     | ✅ Full support                        |
| Edge 90+        | ✅ Full support                        |
| Safari 15+      | ⚠️ May need camera permission tweaks |
| Mobile browsers | ⚠️ Performance varies                |

## 🔮 Future Enhancements

- [ ] Voice commands integration
- [ ] Model upload functionality
- [ ] Gesture recording and playback
- [ ] Multi-user collaboration
- [ ] AR mode with WebXR
- [ ] Custom particle effects per model

## 🙏 Acknowledgments

- [MediaPipe](https://mediapipe.dev/) by Google for hand tracking
- [Three.js](https://threejs.org/) for 3D rendering
- 3D models from [Sketchfab](https://sketchfab.com/) (check individual model licenses)

---

**Made with ✨ gesture magic and 💙 JavaScript**
