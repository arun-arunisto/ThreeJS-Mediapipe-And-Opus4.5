# Point Cloud Mode

Static point cloud visualization of 3D models.

## Features

- **Static Point Cloud**: Model vertices rendered as glowing points
- **Cyan-Blue Gradient**: Color varies based on Y-position
- **Auto-Rotation**: Continuous slow rotation when displayed
- **Simple Controls**: Fist to show/hide, 3 fingers to cycle

## Gesture Controls

| Gesture | Hand | Action |
|---------|------|--------|
| ✊ Fist (0 fingers) | Left | Show/Hide point cloud |
| ✋ 3 Fingers | Left | Cycle to next model |

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

### 3D Models
Located in `../image_models/`:
- Turbine Engine
- Orbital Station Complex
- International Space Station
