# Animated Particles Mode

Dynamic particle visualization with custom GLSL shaders and full gesture controls.

## Features

- **Breathing Effect**: Particles expand and contract rhythmically
- **Wave Motion**: Sinusoidal displacement for flowing movement
- **Sparkle Animation**: Random alpha flickering for shimmer
- **Rainbow Gradient**: Colors based on vertex angle
- **Pinch Scaling**: Scale model with right hand pinch gesture
- **Rotation Control**: Both hands open to rotate freely
- **Auto-Rotation**: Continuous rotation when not manually controlling

## Gesture Controls

| Gesture | Hand | Action |
|---------|------|--------|
| ✊ Fist (0 fingers) | Left | Show animated particles |
| ✊ Fist (0 fingers) | Right | Hide animated particles |
| ✋ 3 Fingers | Left | Cycle to next model |
| 🤏 Pinch (thumb + index) | Right | Scale model size |
| 🖐️🖐️ Both Open (5+5 fingers) | Both | Free rotation control |

## Running

```bash
cd "Hand Gesture 3D Models/animated-particles"
python3 -m http.server 8000
# Open http://localhost:8000
```

Or from parent directory:
```bash
cd "Hand Gesture 3D Models"
python3 -m http.server 8000
# Open http://localhost:8000/animated-particles/
```

## Technical Details

### Custom GLSL Shaders

**Vertex Shader Effects:**
```glsl
// Breathing - expand/contract
float breathe = sin(time * 2.0 + offset) * breatheAmount;
pos += normalize(position) * breathe;

// Wave - sinusoidal displacement
float wave = sin(time * 1.5 + position.y * 3.0 + offset) * waveAmount;
pos.x += wave;
```

**Fragment Shader Effects:**
```glsl
// Sparkle - alpha flickering
vAlpha = 0.6 + sin(time * 5.0 + offset * 10.0) * 0.4;

// Soft circular particle with glow
float glow = pow(1.0 - dist * 2.0, 1.5);
```

### Animation Parameters
- Breathing speed: 2.0
- Breathing amount: 0.03
- Wave speed: 1.5
- Wave amount: 0.02

### Scaling & Rotation
- **Pinch Scaling**: 5-frame moving average smoothing + lerp (0.15)
- **Rotation Control**: Both hands track average delta movement
- **Auto-Rotate Speed**: 0.003 radians/frame when idle

### 3D Models
Located in `../image_models/`:
- Turbine Engine
- Orbital Station Complex
- International Space Station
