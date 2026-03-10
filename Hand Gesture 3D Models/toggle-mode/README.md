# Toggle Mode

Switch between solid 3D model and particle visualization with full gesture controls.

## Features

- **Dual Display Modes**: Solid 3D model or particle point cloud
- **Instant Toggle**: 4-finger gesture switches modes without reloading
- **Visual Feedback**: Header label shows current mode (SOLID/PARTICLE)
- **Pinch Scaling**: Scale model with right hand pinch gesture
- **Rotation Control**: Both hands open to rotate freely
- **Auto-Rotation**: Both modes rotate when not manually controlling
- **Gradient Colors**: Cyan-purple particle coloring
- **Transform Sync**: Scale/rotation preserved when toggling modes

## Gesture Controls

| Gesture | Hand | Action |
|---------|------|--------|
| ✊ Fist (0 fingers) | Left | Show/Hide model |
| ✋ 3 Fingers | Left | Cycle to next model |
| 🖐️ 4 Fingers | Left | Toggle Solid ↔ Particles |
| 🤏 Pinch (thumb + index) | Right | Scale model size |
| 🖐️🖐️ Both Open (5+5 fingers) | Both | Free rotation control |

## Running

```bash
cd "Hand Gesture 3D Models/toggle-mode"
python3 -m http.server 8000
# Open http://localhost:8000
```

Or from parent directory:
```bash
cd "Hand Gesture 3D Models"
python3 -m http.server 8000
# Open http://localhost:8000/toggle-mode/
```

## Technical Details

### Display Mode Toggle
```javascript
function toggleDisplayMode() {
    hideCurrentModel();
    displayMode = displayMode === 'solid' ? 'particle' : 'solid';
    showCurrentModel();
    
    // Sync transforms between modes
    const activeObj = getActiveObject();
    activeObj.scale.setScalar(currentScale);
    activeObj.rotation.x = currentRotationX;
    activeObj.rotation.y = currentRotationY;
    
    // Update UI label
    modeLabel.textContent = `🔄 TOGGLE MODE - ${displayMode.toUpperCase()}`;
}
```

### Scaling & Rotation
- **Pinch Scaling**: 5-frame moving average smoothing + lerp (0.15)
- **Rotation Control**: Both hands track average delta movement
- **Auto-Rotate Speed**: 0.003 radians/frame when idle
- **Transform Sync**: Scale and rotation preserved when switching modes

### Four-Finger Detection
```javascript
function countFingers(landmarks) {
    let count = 0;
    // Thumb horizontal check
    if (Math.abs(landmarks[4].x - landmarks[2].x) > 
        Math.abs(landmarks[3].x - landmarks[2].x) * 1.2) count++;
    // Index, Middle, Ring, Pinky - tip above PIP
    if (landmarks[8].y < landmarks[6].y) count++;
    if (landmarks[12].y < landmarks[10].y) count++;
    if (landmarks[16].y < landmarks[14].y) count++;
    if (landmarks[20].y < landmarks[18].y) count++;
    return count;
}
```

### 3D Models
Located in `../image_models/`:
- Turbine Engine
- Orbital Station Complex
- International Space Station

## Use Cases

- Compare model detail vs particle representation
- Demonstrate different visualization techniques
- Create visual effects by switching rapidly
