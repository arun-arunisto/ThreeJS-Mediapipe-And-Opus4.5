# Main Mode - Full Gesture Control

The main application with complete particle formation and scatter effects.

## Features

- **Particle Formation**: 2000 particles swirl and form into the model shape
- **Particle Scatter**: Explosive dismissal effect when closing fist
- **Scale Control**: Pinch gesture with right hand
- **Rotation Control**: Both hands open with 5 fingers each
- **Model Cycling**: 3 fingers to switch between models

## Gesture Controls

| Gesture | Hand | Action |
|---------|------|--------|
| ✊ Fist (no model) | Left | Summon model with particle formation |
| ✊ Fist (model active) | Right | Dismiss with scatter effect |
| ✋ 3 Fingers | Left | Cycle to next model |
| 🤏 Pinch (thumb + index) | Right | Scale model |
| 🖐️🖐️ Open Palms (5+5) | Both | Rotate model by moving hands |

## Running

```bash
cd "Hand Gesture 3D Models/main"
python3 -m http.server 8000
# Open http://localhost:8000
```

Or from parent directory:
```bash
cd "Hand Gesture 3D Models"
python3 -m http.server 8000
# Open http://localhost:8000/main/
```

## Technical Details

### Particle System
- 2000 particles with additive blending
- Formation duration: 2 seconds with ease-in-out cubic easing
- Scatter duration: 1.5 seconds with outward velocity

### Smoothing
- Scale: Moving average (5 frames) + lerp interpolation (0.15)
- Rotation: Lerp interpolation (0.12)

### 3D Models
Located in `../image_models/`:
- Turbine Engine
- Orbital Station Complex
- International Space Station
