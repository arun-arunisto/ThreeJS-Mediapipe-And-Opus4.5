# Point Cloud Mode

Static point cloud visualization of 3D models with full gesture controls.

## Features

- **Static Point Cloud**: Model vertices rendered as glowing points
- **Cyan-Blue Gradient**: Color varies based on Y-position
- **Pinch Scaling**: Scale model with right hand pinch gesture
- **Rotation Control**: Both hands open to rotate freely
- **Auto-Rotation**: Continuous slow rotation when not manually controlling
- **Simple Controls**: Fist to show/hide, 3 fingers to cycle

## Gesture Controls

| Gesture | Hand | Action |
|---------|------|--------|
| ✊ Fist (0 fingers) | Left | Show/Hide point cloud |
| ✋ 3 Fingers | Left | Cycle to next model |
| 🤏 Pinch (thumb + index) | Right | Scale model size |
| 🖐️🖐️ Both Open (5+5 fingers) | Both | Free rotation control |

## Running

```bash
cd "Hand Gesture 3D Models/point-cloud"
python3 -m http.server 8000
# Open http://localhost:8000
```

Or from parent directory:
```bash
cd "Hand Gesture 3D Models"
python3 -m http.server 8000
# Open http://localhost:8000/point-cloud/
```

## Technical Details

### Point Cloud Generation
- Vertices extracted from GLB model geometry
- World matrix applied for correct positioning
- Additive blending for glow effect

### Color Scheme
```javascript
// Cyan to blue gradient based on Y position
const t = (vertex.y + 1.5) / 3;
colors.push(0.0, 0.8 + t * 0.2, 1.0);
```

### Scaling & Rotation
- **Pinch Scaling**: 5-frame moving average smoothing + lerp (0.15)
- **Rotation Control**: Both hands track average delta movement
- **Auto-Rotate Speed**: 0.003 radians/frame when idle

### 3D Models
Located in `../image_models/`:
- Turbine Engine
- Orbital Station Complex
- International Space Station
