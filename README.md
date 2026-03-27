# 🌌 GARGANTUA
### Interactive Particle Physics Engine

> Gravity-bending particles. Dual-hand tracking. Custom WebGL shaders. 15,000 particles dancing in real-time. Inspired by Nolan's Interstellar.

[![Live Demo](https://img.shields.io/badge/🚀%20Live%20Demo-gargantua3d.vercel.app-00f0ff?style=for-the-badge&logo=vercel)](https://gargantua3d.vercel.app/)
[![GitHub](https://img.shields.io/badge/View%20Source-GitHub-00ff88?style=for-the-badge&logo=github)](https://github.com/Uditpandya07/Gargantua)
[![License](https://img.shields.io/badge/License-MIT-a78bfa?style=for-the-badge)](LICENSE)

---

## 📊 Key Stats

| Metric | Value |
|--------|-------|
| **Particles** | 15,000 |
| **FPS Target** | 60 |
| **Morph Shapes** | 4 |
| **Hands Tracked** | 2 |
| **Backend Required** | 0 |

---

## ⚡ Core Capabilities

### 🖥️ **Custom GLSL Shaders**
Hand-written vertex and fragment shaders render anti-aliased glowing spheres with minimal GPU overhead. Ditched Three.js PointsMaterial entirely for maximum performance.
- **Technology:** WebGL
- **Performance:** 15K particles at 60fps

### ✋ **Scale-Invariant Gestures**
Mathematical normalization enables pinch, fist, and swipe gestures to work accurately whether you're 1 foot or 5 feet from your webcam. No distance recalibration needed.
- **Technology:** MediaPipe
- **Accuracy:** Distance-independent tracking

### 🌀 **Real-Time Physics Loop**
Each particle lerps toward its shape target + gravity center per frame. Velocity dampens at 0.82×. Fist release injects impulse into all 15k particles simultaneously.
- **Technology:** Physics
- **Update Rate:** Per-frame

### 💫 **UnrealBloom Post-Process**
Three.js EffectComposer with UnrealBloomPass at strength 1.6, radius 0.8, threshold 0.2 delivers the signature cinematic neon galaxy glow.
- **Technology:** Three.js
- **Effect:** Strength 1.6, Radius 0.8

### 🔒 **Zero Server, Full Privacy**
MediaPipe runs the ML hand-skeleton model entirely client-side. No video frames, no biometric data, nothing is ever transmitted to any server.
- **Technology:** Client-only
- **Privacy:** 100% local

### 🖱️ **Graceful Mouse Fallback**
After a 4-second timeout, if camera access fails or is denied, the engine seamlessly switches to a full-featured mouse-controlled physics mode.
- **Technology:** Fallback
- **UX:** Seamless transition

---

## 🎮 Gesture Library

### Single-Hand Gestures

| Gesture | Action | Description |
|---------|--------|-------------|
| 🤏 **Pinch** | Drag | Grab and freely drag the gravity center across the screen |
| ✊ **Fist** | Gravity Well | Condense the galaxy to a singularity; release for supernova |
| 👋 **Swipe** | Morph | Quick horizontal swipe morphs particles to next shape |

### Dual-Hand Gestures

| Gesture | Action | Description |
|---------|--------|-------------|
| ↔️ **Pull/Push** | Scale | Spread hands apart to expand; push together to shrink |
| 🏎️ **Twist** | Rotate | Rotate both hands like a steering wheel to spin the galaxy |
| 🤲 **Center** | Gravity Origin | Midpoint between both hands becomes the attractor |

---

## 🌌 Morph Targets

Four mathematically precise universes. 15,000 particles distributed across parametric equations.

### ☁️ Nebula
Random spherical distribution with radial density gradient. The default home state.
```
r·sin(φ)·cos(θ)
```

### 🧬 DNA Helix
Double helix with parametric strand offset and base-pair cross-links.
```
cos(t) + cos(t+π)
```

### 🌌 Galactic Core
Logarithmic spiral arms with density falloff from the central singularity.
```
r·e^(b·θ)
```

### 🔗 Torus Knot
A (2,3) torus knot with volumetric particle distribution around the curve.
```
(R+r·cos(nt))cos(t)
```

---

## 🔧 Tech Stack

| Technology | Role | Details |
|------------|------|---------|
| **Three.js** | 3D Rendering | r158 with EffectComposer |
| **GLSL Shaders** | Particle Rendering | Custom vertex + fragment pair |
| **MediaPipe** | Hand Tracking | Hands v1, client-side ML |
| **UnrealBloomPass** | Post-Processing | Cinematic glow effect |
| **ES Modules** | Module System | Native imports, no bundler |
| **WebGL** | Graphics API | Low-level GPU access |

### Code Sample: Custom Fragment Shader

```glsl
// Fragment Shader - Anti-aliased particle rendering
void main() {
    vec2 uv = gl_PointCoord - 0.5;
    float d = distance(gl_PointCoord, vec2(0.5));
    
    if (d > 0.5) discard;
    
    float alpha = smoothstep(0.5, 0.1, d) * 0.9;
    gl_FragColor = vec4(vColor, alpha);
}
```

### Architecture Highlights

- ✅ **Zero Build Step** - Pure ES modules from CDN
- ✅ **Single HTML File** - No framework dependencies
- ✅ **Client-Side ML** - MediaPipe runs in browser
- ✅ **No Backend** - Fully static deployment
- ✅ **Full Privacy** - No data transmitted

---

## 🚀 Getting Started

### Prerequisites
- Modern browser with WebGL support
- Webcam (optional - mouse fallback available)
- Local server (required for camera access)

### Installation

#### Option A: Python 3
```bash
git clone https://github.com/Uditpandya07/Gargantua.git
cd Gargantua
python3 -m http.server 8000
```

#### Option B: Node.js
```bash
git clone https://github.com/Uditpandya07/Gargantua.git
cd Gargantua
npx http-server -p 8000
```

#### Option C: VS Code Live Server
1. Clone the repository
2. Open folder in VS Code
3. Right-click `index.html` → "Open with Live Server"

### Run It

1. **Start Server** - Run one of the commands above
2. **Open Browser** - Navigate to `http://localhost:8000`
3. **Grant Camera** - Allow camera access when prompted
4. **Sculpt** - Hold up your hands and begin!

**⏱️ Boot Time:** ~4 seconds from page load to tracking live

---

## 🎯 Performance Metrics

| Metric | Target | Achieved |
|--------|--------|----------|
| **Particle Count** | 15,000 | ✅ 15,000 |
| **Frame Rate** | 60 FPS | ✅ 60 FPS |
| **Hand Latency** | <50ms | ✅ ~30ms |
| **Boot Time** | <5s | ✅ ~4s |
| **Mobile Support** | Yes | ✅ Yes |

---

## 🎨 Browser Compatibility

| Browser | Support | Notes |
|---------|---------|-------|
| **Chrome** | ✅ Full | Optimal performance |
| **Firefox** | ✅ Full | Same as Chrome |
| **Safari** | ✅ Full | iOS 14.5+ |
| **Edge** | ✅ Full | Chromium-based |
| **Mobile Safari** | ⚠️ Limited | Camera access limited |

---

## 🔐 Privacy & Security

- 🔒 **Client-Side Only** - All computation happens in your browser
- 📹 **No Video Recording** - Camera stream never stored or transmitted
- 🧠 **Local ML Model** - MediaPipe runs entirely on-device
- 📊 **No Analytics** - Zero data collection
- 🌐 **Deployable Anywhere** - No backend infrastructure needed

---

## 📦 Project Structure

```
Gargantua/
├── index.html              # Main interactive experience
├── README.md               # This file
└── (No dependencies!)      # Pure browser code
```

### Single-File Architecture

Everything is contained in `index.html`:
- HTML structure
- Embedded CSS (styled-components pattern)
- Vanilla JavaScript (ES modules)
- Three.js scene setup
- MediaPipe integration
- Custom shader code

---

## 🎓 Learning Resources

### Related Technologies
- [Three.js Documentation](https://threejs.org/)
- [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands)
- [WebGL & GLSL Reference](https://www.khronos.org/opengl/wiki/OpenGL_Shading_Language)
- [Custom Shaders in Three.js](https://threejs.org/docs/#api/en/materials/ShaderMaterial)

### Inspiration
- Nolan's *Interstellar* (2014) - Gravity & particle effects
- Particle physics visualization
- Real-time gesture recognition
- WebGL creative coding

---

## 🤝 Contributing

This is a showcase project, but feedback and ideas are welcome!

Feel free to:
- 🐛 Report bugs or issues
- 💡 Suggest new gesture interactions
- 🎨 Propose visual improvements
- 📢 Share your experience

---

## 📄 License

MIT License - See [LICENSE](LICENSE) for details

---

## 👨‍💻 Built By

**[@Uditpandya07](https://github.com/Uditpandya07)**

Inspired by the grandeur of *Interstellar* and the beauty of real-time particle physics.

---

## 🌟 Show Your Support

If you found this project interesting:
- ⭐ **Star the repository**
- 🔗 **Share with others**
- 💬 **Leave feedback**
- 📱 **Try the live demo**

---

## 🔗 Links

| Link | Purpose |
|------|---------|
| [🚀 Live Demo](https://gargantua3d.vercel.app/) | Try it now in your browser |
| [📦 GitHub Repo](https://github.com/Uditpandya07/Gargantua) | Source code |
| [👨‍💻 Creator](https://github.com/Uditpandya07) | More projects |

---

<div align="center">

### Made with 🌌 and 💻

**GARGANTUA** © 2024 — Gravity bends light and time. Let it bend your fingers too.

</div>
