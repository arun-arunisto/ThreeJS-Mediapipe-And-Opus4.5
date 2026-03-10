# Hand Gesture 3D Model Controller

An interactive web application suite that uses **hand gesture recognition** to control **3D GLB models** in real-time. Built with MediaPipe Hand Tracking, Three.js, and vanilla JavaScript.

![Status](https://img.shields.io/badge/Status-Working-brightgreen) ![Three.js](https://img.shields.io/badge/Three.js-0.160.0-blue) ![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands-orange) ![Vanilla JS](https://img.shields.io/badge/JavaScript-ES6+-yellow)

---

## 🎯 Project Overview

This project demonstrates real-time hand tracking combined with 3D visualization. Use natural hand gestures to summon, manipulate, and dismiss 3D models displayed as solid objects or particle effects.

---

## 📁 Project Structure

```
Hand Gesture 3D Models/
├── README.md                     ← You are here (Project Overview)
│
├── main/                         ← Full-featured application
│   ├── index.html
│   └── README.md
│
├── point-cloud/                  ← Static point cloud visualization
│   ├── index.html
│   └── README.md
│
├── animated-particles/           ← Animated particles with shaders
│   ├── index.html
│   └── README.md
│
├── toggle-mode/                  ← Switch between solid & particles
│   ├── index.html
│   └── README.md
│
└── image_models/                 ← 3D model assets
    ├── turbine__turbofan_engine__jet_engine.glb
    ├── ksp_primitive_orbital_station_complex.glb
    └── iss.glb
```

---

## 🎮 Available Modes

| Mode | Description | Key Features | Link |
|------|-------------|--------------|------|
| **Main** | Full-featured experience | Particle formation/scatter effects, scaling, rotation | [main/](main/) |
| **Point Cloud** | Static particle visualization | Models rendered as colored point clouds | [point-cloud/](point-cloud/) |
| **Animated Particles** | Dynamic particle effects | GLSL shaders with breathing, wave, sparkle | [animated-particles/](animated-particles/) |
| **Toggle Mode** | Solid ↔ Particle switch | Compare both rendering styles | [toggle-mode/](toggle-mode/) |

---

## 🖐️ Gesture Controls Summary

### Universal Gestures (All Modes)

| Gesture | Hand | Action |
|---------|------|--------|
| ✊ **Fist** (0 fingers) | Left | Show model |
| ✊ **Fist** (0 fingers) | Right | Hide model |
| ✋ **3 Fingers** | Left | Cycle through models |
| 🤏 **Pinch** (thumb + index) | Right | Scale model size |
| 🙌 **Both Open** (5+5 fingers) | Both | Free rotation control |

### Main Mode Only

| Gesture | Hand | Action |
|---------|------|--------|
| ✊ **Fist** (no model) | Left | Summon with particle formation |
| ✊ **Fist** (model active) | Right | Dismiss with particle scatter |

### Toggle Mode Only

| Gesture | Hand | Action |
|---------|------|--------|
| 🖐️ **4 Fingers** | Left | Toggle Solid ↔ Particle view |

---

## 🛠️ Technologies Used

| Technology | Version | Purpose |
|------------|---------|---------|
| [MediaPipe Hands](https://mediapipe.dev/) | 0.4.1675469240 | Real-time hand landmark detection |
| [Three.js](https://threejs.org/) | 0.160.0 | 3D rendering & GLB model loading |
| [GLTFLoader](https://threejs.org/docs/#examples/en/loaders/GLTFLoader) | Three.js addon | Loading .glb/.gltf files |
| WebGL / WebGL2 | - | Hardware-accelerated graphics |
| GLSL Shaders | - | Custom particle animations |
| Canvas API | - | 2D hand landmark overlay |
| Vanilla JavaScript | ES6+ | Application logic |

---

## 🎨 3D Models Included

| # | Model | Description |
|---|-------|-------------|
| 1 | `turbine__turbofan_engine__jet_engine.glb` | Jet engine turbine |
| 2 | `ksp_primitive_orbital_station_complex.glb` | Orbital space station |
| 3 | `iss.glb` | International Space Station |

### Adding Custom Models

1. Place your `.glb` file in `image_models/`
2. Add the path to the `modelPaths` array:

```javascript
const modelPaths = [
    '../image_models/turbine__turbofan_engine__jet_engine.glb',
    '../image_models/ksp_primitive_orbital_station_complex.glb',
    '../image_models/iss.glb',
    '../image_models/your_model.glb'  // Add here
];
```

---

## 🚀 Getting Started

### Prerequisites

- Modern web browser (Chrome 90+, Firefox 88+, Edge 90+)
- Webcam
- Local HTTP server (required for ES6 modules)

### Quick Start

1. **Navigate to project folder:**
   ```bash
   cd "Hand Gesture 3D Models"
   ```

2. **Start a local server:**
   ```bash
   # Python 3
   python3 -m http.server 8000
   
   # Node.js
   npx http-server -p 8000
   
   # PHP
   php -S localhost:8000
   ```

3. **Open in browser:**

   | Mode | URL |
   |------|-----|
   | Main | http://localhost:8000/main/ |
   | Point Cloud | http://localhost:8000/point-cloud/ |
   | Animated Particles | http://localhost:8000/animated-particles/ |
   | Toggle Mode | http://localhost:8000/toggle-mode/ |

4. **Allow camera access** when prompted

---

## 🔧 Technical Highlights

### Hand Landmark Detection

MediaPipe detects 21 landmarks per hand:

```
        8   12  16  20      ← Fingertips
        |   |   |   |
    4   7   11  15  19
    |   |   |   |   |
    3   6   10  14  18
    |   |   |   |   |
    2   5   9   13  17
     \   \  |  /   /
      \   \ | /   /
       ---- 0 ----          ← Wrist
```

### Finger Counting Logic

```javascript
function countFingers(landmarks) {
    let count = 0;
    
    // Thumb: check horizontal extension
    if (Math.abs(landmarks[4].x - landmarks[2].x) > 
        Math.abs(landmarks[3].x - landmarks[2].x) * 1.2) count++;
    
    // Index: tip above PIP joint
    if (landmarks[8].y < landmarks[6].y) count++;
    
    // Middle, Ring, Pinky: same logic
    if (landmarks[12].y < landmarks[10].y) count++;
    if (landmarks[16].y < landmarks[14].y) count++;
    if (landmarks[20].y < landmarks[18].y) count++;
    
    return count;
}
```

### Point Cloud Generation

```javascript
function extractVertices(model) {
    const positions = [];
    model.traverse((child) => {
        if (child.isMesh && child.geometry) {
            const posAttr = child.geometry.attributes.position;
            for (let i = 0; i < posAttr.count; i++) {
                positions.push(
                    posAttr.getX(i),
                    posAttr.getY(i),
                    posAttr.getZ(i)
                );
            }
        }
    });
    return positions;
}
```

### GLSL Particle Animation (Animated Particles Mode)

```glsl
// Vertex Shader
uniform float time;
attribute float offset;

void main() {
    float breathe = sin(time * 2.0 + offset) * 0.03;
    float wave = sin(time * 1.5 + position.y * 3.0) * 0.02;
    
    vec3 pos = position + normalize(position) * breathe;
    pos.x += wave;
    
    gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
}
```

---

## 🎨 Visual Indicators

| Color | Meaning |
|-------|---------|
| 🔴 Red `#FF6B6B` | Left hand landmarks |
| 🔵 Teal `#4ECDC4` | Right hand landmarks |
| 🟢 Green `#00FF00` | Fist detected (summon) |
| 🟡 Gold `#FFD700` | 3 fingers / pinching |
| 🟣 Purple `#9B59B6` | Rotation mode |
| 🔵 Cyan `#00FFFF` | Particle formation |
| 🟠 Orange `#FF9500` | Fist detected (dismiss) |
| 💜 Magenta `#FF00FF` | 4 fingers (toggle) |

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Camera not starting | Check browser permissions, use localhost or HTTPS |
| Hand not detected | Improve lighting, keep hand 1-3 feet from camera |
| Models not loading | Check console for errors, verify file paths |
| Gestures not working | Ensure fingers are clearly visible, slow movements |
| Laggy performance | Reduce particle count, lower `modelComplexity` to 0 |
| Shader errors | Update browser, check WebGL2 support |

---

## 🌐 Browser Compatibility

| Browser | Status |
|---------|--------|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Safari 15+ | ⚠️ May need permission reset |
| Mobile | ⚠️ Limited performance |

---

## 📊 Mode Comparison

| Feature | Main | Point Cloud | Animated | Toggle |
|---------|:----:|:-----------:|:--------:|:------:|
| Solid 3D Model | ✅ | ❌ | ❌ | ✅ |
| Static Particles | ❌ | ✅ | ❌ | ✅ |
| Animated Particles | ❌ | ❌ | ✅ | ❌ |
| Formation Effect | ✅ | ❌ | ❌ | ❌ |
| Scatter Effect | ✅ | ❌ | ❌ | ❌ |
| Pinch Scaling | ✅ | ✅ | ✅ | ✅ |
| Rotation Control | ✅ | ✅ | ✅ | ✅ |
| Mode Toggle | ❌ | ❌ | ❌ | ✅ |
| GLSL Shaders | ❌ | ❌ | ✅ | ❌ |

---

## 📖 Documentation Links

- [Main Mode Documentation](main/README.md)
- [Point Cloud Documentation](point-cloud/README.md)
- [Animated Particles Documentation](animated-particles/README.md)
- [Toggle Mode Documentation](toggle-mode/README.md)

---

## 🙏 Acknowledgments

- [MediaPipe](https://mediapipe.dev/) by Google for hand tracking
- [Three.js](https://threejs.org/) for 3D rendering
- [Sketchfab](https://sketchfab.com/) for 3D model resources

---

**Made with ✨ gesture magic and 💙 JavaScript**
