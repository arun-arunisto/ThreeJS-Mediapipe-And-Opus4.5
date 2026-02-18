# Cyberpunk Interactive Particle System

---
## Prompt

```
Act as an expert frontend creative developer specializing in Three.js, WebGL, and Computer Vision (MediaPipe Hands). Your task is to create a high-performance, single-file HTML application featuring a cyberpunk interactive particle system.
Visual Style:
• Theme: Hardcore Cyberpunk.
• Background: Deep black with subtle moving grid/scanlines and a vignette effect.
• HUD: Minimalist data display in the four corners (FPS, Particle Count, Hand Status), using the Orbitron or monospaced font in Neon Cyan.
• Material: Particles must use AdditiveBlending for a glowing, neon look.
Core Parameters:
• Particle Count: 12,000.
• Particle Size: 2.4.
• Physics: Fast return speed (lerp factor 0.16). Strong and snappy repulsion force.
Interaction Logic (Detailed Requirements):
1. Left Hand (Command Controller):
Detect the number of extended fingers to switch the target shape and color of the particles:
• 1 Finger: Text "Hello World" | Color: Neon Blue (0x00FFFF).
• 2 Fingers: Text "Iam Arun Arunisto" | Color: Neon Yellow (0xFFFF00).
• 3 Fingers: Text "Welcome To The Future" | Color: Neon Pink (0xFF00FF).
• 4 Fingers: Text "Good Bye" | Color: Neon Green (0x00FF88).
• 5 Fingers (Open Palm): Enter "Catch Mode" (acts as a destination container).
2. Right Hand (Physics Interactor):
• Tracking Point: Always track the Index Finger Tip (Landmark 8).
• State A (Pointing/Fist - Default):
• Interaction: When touching text particles, trigger a Strong Scatter effect.
• Constraint: Pure planar repulsion (XY axis). Do NOT apply any Z-axis bulge or distortion (keep it flat).
• State B (Open Hand - 5 Fingers):
• Trigger: "Nebula Mode". Particles scatter to fill the entire screen 3D space.
• Interaction: Applying a "Water Ripple" (sine wave) effect when the hand moves through the particles.
3. Dual Hand Combo (Ultimate Effect):
• Condition: When Right Hand is Open (Nebula active) AND Left Hand is Open (Catch active) simultaneously.
• Effect Logic:
• All full-screen particles are instantly attracted to the Left Hand Palm center.
• Shape: Particles form a Rotating 3D Basketball in the left hand (Orange color, using Fibonacci sphere distribution, with calculated black lines to simulate ball texture).
• Motion Trajectory: Particles must fly towards the left hand with a Bouncing/Jumping trajectory (high-frequency Y-axis sine wave), looking like thousands of energetic bouncing beads.
Technical Implementation:
• Use Canvas pixel scanning to generate text coordinates (Font size: 75px, bold).
• Configure MediaPipe Hands to detect a maximum of 2 hands.
• No mobile-specific constraints (use default webcam settings).
• Ensure the code is self-contained in a single index.html file (CSS, JS, HTML combined).
```

A high-performance interactive particle system featuring 12,000 particles controlled by hand gestures using MediaPipe Hands and Three.js.

![Theme: Hardcore Cyberpunk](https://img.shields.io/badge/Theme-Cyberpunk-ff00ff)
![Particles: 12,000](https://img.shields.io/badge/Particles-12%2C000-00ffff)

## Features

- **Real-time hand tracking** using MediaPipe Hands
- **12,000 particles** with additive blending for neon glow
- **Webcam background** with cyberpunk overlays (grid, scanlines, vignette)
- **Multiple interaction modes** controlled by hand gestures
- **HUD display** showing FPS, particle count, and hand status

## Requirements

- Modern web browser (Chrome, Firefox, Edge)
- Webcam
- JavaScript enabled

## How to Run

1. Open `index.html` in a web browser
2. Allow webcam access when prompted
3. Position yourself so your hands are visible to the camera

## Hand Controls

### Left Hand - Shape Controller

Control the text shape and color by extending different numbers of fingers:

| Fingers               | Text                    | Color       |
| --------------------- | ----------------------- | ----------- |
| 1 finger              | "Hello World"           | Neon Blue   |
| 2 fingers             | "Iam Arun Arunisto"     | Neon Yellow |
| 3 fingers             | "Welcome To The Future" | Neon Pink   |
| 4 fingers             | "Good Bye"              | Neon Green  |
| 5 fingers (open palm) | **Catch Mode**    | -           |

### Right Hand - Physics Interactor

The index finger tip is tracked for interaction:

| Gesture               | Mode                   | Effect                                                   |
| --------------------- | ---------------------- | -------------------------------------------------------- |
| Pointing/Fist         | **Scatter Mode** | Strong XY repulsion on particles (flat, no Z distortion) |
| Open Palm (5 fingers) | **Nebula Mode**  | Particles scatter to 3D space with water ripple effect   |

### Dual Hand Combo - Ultimate Effect

| Condition                        | Effect                                                                                |
| -------------------------------- | ------------------------------------------------------------------------------------- |
| Both hands open (5 fingers each) | **Basketball Mode** - Particles form a rotating 3D basketball in your left hand |

## Basketball Mode Details

When activated:

- Particles attract to left palm center
- Forms a **Fibonacci sphere** distribution
- **Orange color** with black seam lines (realistic basketball texture)
- Particles follow **bouncing trajectory** (energetic jumping motion)
- Ball **rotates** continuously

## HUD Display

The corners of the screen show real-time information:

- **Top Left**: FPS (frames per second)
- **Top Right**: Particle count
- **Bottom Left**: Left hand status & finger count
- **Bottom Right**: Right hand status & current mode

## Technical Specs

| Parameter           | Value     |
| ------------------- | --------- |
| Particle Count      | 12,000    |
| Particle Size       | 2.4       |
| Return Speed (lerp) | 0.16      |
| Repulsion Radius    | 150px     |
| Text Font Size      | 75px bold |
| Max Hands Tracked   | 2         |

## Tips for Best Experience

1. **Lighting**: Ensure good, even lighting on your hands
2. **Background**: A plain background helps hand detection accuracy
3. **Distance**: Keep hands 1-3 feet from camera
4. **Gestures**: Make clear, deliberate finger extensions
5. **Performance**: Close other browser tabs for better FPS

## Troubleshooting

| Issue                | Solution                                          |
| -------------------- | ------------------------------------------------- |
| Hands not detected   | Check webcam permissions, improve lighting        |
| Low FPS              | Close other applications, reduce browser zoom     |
| Particles not moving | Ensure hands are visible in webcam preview        |
| Wrong hand detected  | MediaPipe mirrors the image - left shows as right |

## Technologies Used

- **Three.js** - 3D rendering and particle system
- **MediaPipe Hands** - Real-time hand tracking
- **Canvas API** - Text-to-coordinate conversion
- **CSS3** - Cyberpunk visual effects (grid, scanlines, vignette)

---

*Created with ❤️ Claude Opus 4.5*
