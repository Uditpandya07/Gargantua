# 🌌 GARGANTUA
### Interactive Particle Physics Engine

> Gravity-bending particles. Dual-hand tracking. Custom WebGL shaders. 15,000 particles dancing in real-time. Inspired by Nolan's Interstellar.

<div align="center">

<!-- Animated Particle Visualization -->
<svg width="500" height="300" viewBox="0 0 500 300" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes orbit1 { 0% { transform: rotate(0deg) translateX(80px) rotate(0deg); } 100% { transform: rotate(360deg) translateX(80px) rotate(-360deg); } }
      @keyframes orbit2 { 0% { transform: rotate(0deg) translateX(120px) rotate(0deg); } 100% { transform: rotate(-360deg) translateX(120px) rotate(360deg); } }
      @keyframes orbit3 { 0% { transform: rotate(0deg) translateX(150px) rotate(0deg); } 100% { transform: rotate(360deg) translateX(150px) rotate(-360deg); } }
      @keyframes pulse { 0%, 100% { r: 3; opacity: 1; } 50% { r: 5; opacity: 0.6; } }
      @keyframes glow { 0%, 100% { filter: drop-shadow(0 0 2px #00ff88); } 50% { filter: drop-shadow(0 0 8px #00f0ff); } }
      .particle { animation: pulse 2s ease-in-out infinite; }
      .center-core { animation: glow 1.5s ease-in-out infinite; }
      .ring1 { transform-origin: 250px 150px; animation: orbit1 20s linear infinite; }
      .ring2 { transform-origin: 250px 150px; animation: orbit2 30s linear infinite; }
      .ring3 { transform-origin: 250px 150px; animation: orbit3 25s linear infinite; }
    </style>
    
    <radialGradient id="glowGradient" cx="40%" cy="40%">
      <stop offset="0%" style="stop-color:#00ff88;stop-opacity:0.8" />
      <stop offset="100%" style="stop-color:#00f0ff;stop-opacity:0" />
    </radialGradient>
  </defs>
  
  <!-- Background -->
  <rect width="500" height="300" fill="#0a0a0f"/>
  
  <!-- Grid -->
  <defs>
    <pattern id="grid" width="40" height="40" patternUnits="userSpaceOnUse">
      <path d="M 40 0 L 0 0 0 40" fill="none" stroke="#00f0ff" stroke-width="0.5" opacity="0.1"/>
    </pattern>
  </defs>
  <rect width="500" height="300" fill="url(#grid)" />
  
  <!-- Outer Glow -->
  <circle cx="250" cy="150" r="200" fill="url(#glowGradient)" opacity="0.3"/>
  
  <!-- Ring 1 -->
  <g class="ring1">
    <circle cx="250" cy="50" r="4" class="particle" fill="#00ff88"/>
    <circle cx="330" cy="90" r="4" class="particle" fill="#00ff88"/>
    <circle cx="350" cy="150" r="4" class="particle" fill="#00ff88"/>
    <circle cx="330" cy="210" r="4" class="particle" fill="#00ff88"/>
    <circle cx="250" cy="250" r="4" class="particle" fill="#00ff88"/>
    <circle cx="170" cy="210" r="4" class="particle" fill="#00ff88"/>
    <circle cx="150" cy="150" r="4" class="particle" fill="#00ff88"/>
    <circle cx="170" cy="90" r="4" class="particle" fill="#00ff88"/>
  </g>
  
  <!-- Ring 2 -->
  <g class="ring2">
    <circle cx="250" cy="30" r="3" class="particle" fill="#00f0ff"/>
    <circle cx="355" cy="75" r="3" class="particle" fill="#00f0ff"/>
    <circle cx="370" cy="150" r="3" class="particle" fill="#00f0ff"/>
    <circle cx="355" cy="225" r="3" class="particle" fill="#00f0ff"/>
    <circle cx="250" cy="270" r="3" class="particle" fill="#00f0ff"/>
    <circle cx="145" cy="225" r="3" class="particle" fill="#00f0ff"/>
    <circle cx="130" cy="150" r="3" class="particle" fill="#00f0ff"/>
    <circle cx="145" cy="75" r="3" class="particle" fill="#00f0ff"/>
  </g>
  
  <!-- Ring 3 -->
  <g class="ring3">
    <circle cx="250" cy="10" r="2.5" class="particle" fill="#a78bfa"/>
    <circle cx="375" cy="65" r="2.5" class="particle" fill="#a78bfa"/>
    <circle cx="390" cy="150" r="2.5" class="particle" fill="#a78bfa"/>
    <circle cx="375" cy="235" r="2.5" class="particle" fill="#a78bfa"/>
    <circle cx="250" cy="290" r="2.5" class="particle" fill="#a78bfa"/>
    <circle cx="125" cy="235" r="2.5" class="particle" fill="#a78bfa"/>
    <circle cx="110" cy="150" r="2.5" class="particle" fill="#a78bfa"/>
    <circle cx="125" cy="65" r="2.5" class="particle" fill="#a78bfa"/>
  </g>
  
  <!-- Center Core -->
  <circle cx="250" cy="150" r="6" class="center-core" fill="#ff3d7f" opacity="0.8"/>
</svg>

[![Live Demo](https://img.shields.io/badge/🚀%20LIVE%20DEMO-gargantua3d.vercel.app-00f0ff?style=for-the-badge&labelColor=0a0a0f&color=00f0ff)](https://gargantua3d.vercel.app/)
[![GitHub](https://img.shields.io/badge/📦%20SOURCE%20CODE-GitHub-00ff88?style=for-the-badge&labelColor=0a0a0f&color=00ff88)](https://github.com/Uditpandya07/Gargantua)
[![License](https://img.shields.io/badge/LICENSE-MIT-a78bfa?style=for-the-badge&labelColor=0a0a0f&color=a78bfa)](LICENSE)

</div>

---

## 📊 Key Metrics

<table>
  <tr>
    <td align="center"><strong>15K</strong><br/>Particles</td>
    <td align="center"><strong>60</strong><br/>FPS Target</td>
    <td align="center"><strong>4</strong><br/>Morph Shapes</td>
    <td align="center"><strong>2</strong><br/>Hands Tracked</td>
    <td align="center"><strong>0ms</strong><br/>Server Latency</td>
  </tr>
</table>

---

## ⚡ Core Features

<table>
  <tr>
    <td width="50%">
      <h3>🖥️ Custom GLSL Shaders</h3>
      <p>Hand-written vertex and fragment shaders render anti-aliased glowing spheres with minimal GPU overhead. Ditched Three.js PointsMaterial entirely.</p>
      <code>✓ WebGL</code> <code>✓ 60fps</code>
    </td>
    <td width="50%">
      <h3>✋ Scale-Invariant Gestures</h3>
      <p>Mathematical normalization enables accurate tracking whether you're 1 foot or 5 feet from your webcam.</p>
      <code>✓ MediaPipe</code> <code>✓ Adaptive</code>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <h3>🌀 Real-Time Physics</h3>
      <p>Each particle lerps toward its shape target. Velocity dampens at 0.82×. Fist release injects impulse into all 15k particles.</p>
      <code>✓ Per-frame</code> <code>✓ Smooth</code>
    </td>
    <td width="50%">
      <h3>💫 UnrealBloom FX</h3>
      <p>Three.js EffectComposer with cinematic glow at strength 1.6, radius 0.8, threshold 0.2.</p>
      <code>✓ Strength 1.6</code> <code>✓ Cinematic</code>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <h3>🔒 100% Client-Side</h3>
      <p>MediaPipe runs entirely in your browser. No video transmission, no backend, full privacy.</p>
      <code>✓ Local ML</code> <code>✓ Private</code>
    </td>
    <td width="50%">
      <h3>🖱️ Mouse Fallback</h3>
      <p>Camera denied? Seamlessly switch to mouse-controlled physics mode after 4-second timeout.</p>
      <code>✓ Resilient</code> <code>✓ Fallback</code>
    </td>
  </tr>
</table>

---

## 🎮 Interactive Gestures

<div align="center">

### Single-Hand Controls

<svg width="600" height="200" viewBox="0 0 600 200" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes float-up { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-10px); } }
      @keyframes wiggle { 0%, 100% { transform: rotate(0deg); } 25% { transform: rotate(-5deg); } 75% { transform: rotate(5deg); } }
      .gesture-box { animation: float-up 3s ease-in-out infinite; }
      .emoji { font-size: 48px; animation: wiggle 2s ease-in-out infinite; }
    </style>
  </defs>
  
  <rect width="600" height="200" fill="#0a0a0f" stroke="#00f0ff" stroke-width="1" opacity="0.3" rx="10"/>
  
  <!-- Pinch -->
  <g class="gesture-box" style="animation-delay: 0s;">
    <rect x="20" y="40" width="140" height="120" fill="#00f0ff" fill-opacity="0.05" stroke="#00ff88" stroke-width="2" rx="8"/>
    <text x="90" y="90" font-size="32" text-anchor="middle" fill="#00ff88" font-weight="bold">🤏</text>
    <text x="90" y="135" font-size="12" text-anchor="middle" fill="#8b8ba8" font-family="monospace">PINCH</text>
  </g>
  
  <!-- Fist -->
  <g class="gesture-box" style="animation-delay: 0.5s;">
    <rect x="230" y="40" width="140" height="120" fill="#00f0ff" fill-opacity="0.05" stroke="#a78bfa" stroke-width="2" rx="8"/>
    <text x="300" y="90" font-size="32" text-anchor="middle" fill="#a78bfa" font-weight="bold">✊</text>
    <text x="300" y="135" font-size="12" text-anchor="middle" fill="#8b8ba8" font-family="monospace">GRAVITY WELL</text>
  </g>
  
  <!-- Swipe -->
  <g class="gesture-box" style="animation-delay: 1s;">
    <rect x="440" y="40" width="140" height="120" fill="#00f0ff" fill-opacity="0.05" stroke="#ff3d7f" stroke-width="2" rx="8"/>
    <text x="510" y="90" font-size="32" text-anchor="middle" fill="#ff3d7f" font-weight="bold">👋</text>
    <text x="510" y="135" font-size="12" text-anchor="middle" fill="#8b8ba8" font-family="monospace">SWIPE</text>
  </g>
</svg>

### Dual-Hand Controls

<svg width="600" height="160" viewBox="0 0 600 160" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes pulse-scale { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.1); } }
      .dual-box { animation: pulse-scale 2.5s ease-in-out infinite; }
    </style>
  </defs>
  
  <rect width="600" height="160" fill="#0a0a0f" stroke="#00f0ff" stroke-width="1" opacity="0.3" rx="10"/>
  
  <!-- Scale -->
  <g class="dual-box" style="animation-delay: 0s; transform-origin: 150px 80px;">
    <rect x="50" y="20" width="200" height="120" fill="#00ff88" fill-opacity="0.05" stroke="#00ff88" stroke-width="2" rx="8"/>
    <text x="150" y="70" font-size="28" text-anchor="middle" fill="#00ff88" font-weight="bold">↔️</text>
    <text x="150" y="110" font-size="11" text-anchor="middle" fill="#8b8ba8" font-family="monospace">PULL/PUSH</text>
  </g>
  
  <!-- Twist -->
  <g class="dual-box" style="animation-delay: 0.7s; transform-origin: 350px 80px;">
    <rect x="250" y="20" width="200" height="120" fill="#a78bfa" fill-opacity="0.05" stroke="#a78bfa" stroke-width="2" rx="8"/>
    <text x="350" y="70" font-size="28" text-anchor="middle" fill="#a78bfa" font-weight="bold">🏎️</text>
    <text x="350" y="110" font-size="11" text-anchor="middle" fill="#8b8ba8" font-family="monospace">TWIST</text>
  </g>
  
  <!-- Center -->
  <g class="dual-box" style="animation-delay: 1.4s; transform-origin: 550px 80px;">
    <rect x="450" y="20" width="100" height="120" fill="#ff3d7f" fill-opacity="0.05" stroke="#ff3d7f" stroke-width="2" rx="8"/>
    <text x="500" y="70" font-size="28" text-anchor="middle" fill="#ff3d7f" font-weight="bold">🤲</text>
    <text x="500" y="110" font-size="10" text-anchor="middle" fill="#8b8ba8" font-family="monospace">CENTER</text>
  </g>
</svg>

</div>

---

## 🌌 Four Morph Universes

<div align="center">

<svg width="600" height="300" viewBox="0 0 600 300" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <style>
      @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
      @keyframes float-in { 0% { opacity: 0; transform: scale(0.8); } 100% { opacity: 1; transform: scale(1); } }
      .shape-container { animation: float-in 0.8s ease-out; transform-origin: center; }
      .nebula { animation: spin 20s linear infinite; }
      .dna { animation: spin 15s linear infinite reverse; }
      .galaxy { animation: spin 25s linear infinite; }
      .torus { animation: spin 18s linear infinite reverse; }
    </style>
  </defs>
  
  <rect width="600" height="300" fill="#0a0a0f"/>
  
  <!-- Nebula -->
  <g class="shape-container" style="animation-delay: 0s; transform-origin: 150px 75px;">
    <g class="nebula" style="transform-origin: 150px 75px; transform-box: fill-box;">
      <circle cx="150" cy="30" r="2" fill="#00ff88" opacity="0.8"/>
      <circle cx="175" cy="45" r="2" fill="#00ff88" opacity="0.7"/>
      <circle cx="180" cy="75" r="2" fill="#00ff88" opacity="0.8"/>
      <circle cx="175" cy="105" r="2" fill="#00ff88" opacity="0.7"/>
      <circle cx="150" cy="120" r="2" fill="#00ff88" opacity="0.8"/>
      <circle cx="125" cy="105" r="2" fill="#00ff88" opacity="0.7"/>
      <circle cx="120" cy="75" r="2" fill="#00ff88" opacity="0.8"/>
      <circle cx="125" cy="45" r="2" fill="#00ff88" opacity="0.7"/>
    </g>
    <text x="150" y="155" font-size="14" text-anchor="middle" fill="#00ff88" font-weight="bold">NEBULA</text>
  </g>
  
  <!-- DNA Helix -->
  <g class="shape-container" style="animation-delay: 0.2s; transform-origin: 300px 75px;">
    <g class="dna" style="transform-origin: 300px 75px; transform-box: fill-box;">
      <circle cx="300" cy="30" r="2" fill="#00f0ff" opacity="0.8"/>
      <circle cx="310" cy="50" r="2" fill="#00f0ff" opacity="0.7"/>
      <circle cx="300" cy="75" r="2.5" fill="#00f0ff" opacity="0.8"/>
      <circle cx="310" cy="100" r="2" fill="#00f0ff" opacity="0.7"/>
      <circle cx="300" cy="120" r="2" fill="#00f0ff" opacity="0.8"/>
      <circle cx="290" cy="100" r="2" fill="#00f0ff" opacity="0.7"/>
      <circle cx="300" cy="75" r="1.5" fill="#00f0ff" opacity="0.5"/>
      <circle cx="290" cy="50" r="2" fill="#00f0ff" opacity="0.7"/>
    </g>
    <text x="300" y="155" font-size="14" text-anchor="middle" fill="#00f0ff" font-weight="bold">DNA HELIX</text>
  </g>
  
  <!-- Galactic Core -->
  <g class="shape-container" style="animation-delay: 0.4s; transform-origin: 450px 75px;">
    <g class="galaxy" style="transform-origin: 450px 75px; transform-box: fill-box;">
      <circle cx="450" cy="30" r="2" fill="#a78bfa" opacity="0.8"/>
      <circle cx="465" cy="40" r="2" fill="#a78bfa" opacity="0.7"/>
      <circle cx="470" cy="60" r="2" fill="#a78bfa" opacity="0.8"/>
      <circle cx="465" cy="90" r="2" fill="#a78bfa" opacity="0.7"/>
      <circle cx="450" cy="110" r="2" fill="#a78bfa" opacity="0.8"/>
      <circle cx="435" cy="90" r="2" fill="#a78bfa" opacity="0.7"/>
      <circle cx="430" cy="60" r="2" fill="#a78bfa" opacity="0.8"/>
      <circle cx="435" cy="40" r="2" fill="#a78bfa" opacity="0.7"/>
    </g>
    <text x="450" y="155" font-size="14" text-anchor="middle" fill="#a78bfa" font-weight="bold">GALACTIC</text>
  </g>
  
  <!-- Torus Knot -->
  <g class="shape-container" style="animation-delay: 0.6s;">
    <!-- Placeholder for Torus (complex to draw) -->
    <rect x="535" y="20" width="50" height="120" fill="none" stroke="#ff3d7f" stroke-width="1" opacity="0.3" rx="4"/>
    <g class="torus" style="transform-origin: 560px 80px;">
      <circle cx="560" cy="35" r="2" fill="#ff3d7f" opacity="0.8"/>
      <circle cx="570" cy="50" r="2" fill="#ff3d7f" opacity="0.7"/>
      <circle cx="575" cy="75" r="2" fill="#ff3d7f" opacity="0.8"/>
      <circle cx="570" cy="110" r="2" fill="#ff3d7f" opacity="0.7"/>
      <circle cx="560" cy="120" r="2" fill="#ff3d7f" opacity="0.8"/>
    </g>
    <text x="560" y="155" font-size="14" text-anchor="middle" fill="#ff3d7f" font-weight="bold">TORUS</text>
  </g>
</svg>

</div>

---

## 🔧 Technology Stack

<div align="center">

| Tech | Purpose | Badge |
|------|---------|-------|
| **Three.js** | 3D Rendering | ![Three.js](https://img.shields.io/badge/Three.js-r158-00f0ff?style=flat-square&labelColor=0a0a0f) |
| **GLSL** | Custom Shaders | ![GLSL](https://img.shields.io/badge/GLSL-Shaders-00ff88?style=flat-square&labelColor=0a0a0f) |
| **MediaPipe** | Hand Tracking | ![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands%20v1-a78bfa?style=flat-square&labelColor=0a0a0f) |
| **WebGL** | Graphics API | ![WebGL](https://img.shields.io/badge/WebGL-GPU%20Access-ff3d7f?style=flat-square&labelColor=0a0a0f) |
| **ES Modules** | No Build Step | ![ES6](https://img.shields.io/badge/ES6%20Modules-Native-00f0ff?style=flat-square&labelColor=0a0a0f) |

</div>

```glsl
// ⚡ Fragment Shader - Anti-aliased Particle Rendering
void main() {
    vec2 uv = gl_PointCoord - 0.5;
    float d = distance(gl_PointCoord, vec2(0.5));
    
    if (d > 0.5) discard;
    
    float alpha = smoothstep(0.5, 0.1, d) * 0.9;
    gl_FragColor = vec4(vColor, alpha);
}
```

---

## 🚀 Quick Start

<div align="center">

### 1️⃣ Clone Repository
```bash
git clone https://github.com/Uditpandya07/Gargantua.git
cd Gargantua
```

### 2️⃣ Start Local Server

**Python 3:**
```bash
python3 -m http.server 8000
```

**Node.js:**
```bash
npx http-server -p 8000
```

**VS Code:**
Right-click `index.html` → "Open with Live Server"

### 3️⃣ Open & Grant Camera
```
→ http://localhost:8000
→ Allow camera access
→ Wait ~4 seconds for boot
```

### 4️⃣ Sculpt the Galaxy
Bring your hands into frame and start interacting!

</div>

---

## 📊 Performance Breakdown

<table>
  <tr>
    <th>Metric</th>
    <th>Target</th>
    <th>Actual</th>
    <th>Status</th>
  </tr>
  <tr>
    <td><strong>Particle Count</strong></td>
    <td>15,000</td>
    <td>15,000</td>
    <td>✅ Target Met</td>
  </tr>
  <tr>
    <td><strong>Frame Rate</strong></td>
    <td>60 FPS</td>
    <td>60 FPS</td>
    <td>✅ Smooth</td>
  </tr>
  <tr>
    <td><strong>Hand Latency</strong></td>
    <td>&lt;50ms</td>
    <td>~30ms</td>
    <td>✅ Responsive</td>
  </tr>
  <tr>
    <td><strong>Boot Time</strong></td>
    <td>&lt;5s</td>
    <td>~4s</td>
    <td>✅ Quick</td>
  </tr>
  <tr>
    <td><strong>Mobile Support</strong></td>
    <td>Yes</td>
    <td>Yes</td>
    <td>✅ Supported</td>
  </tr>
</table>

---

## 🌐 Browser Support

<div align="center">

![Chrome](https://img.shields.io/badge/Chrome-✅%20Full-4285f4?style=for-the-badge&labelColor=0a0a0f)
![Firefox](https://img.shields.io/badge/Firefox-✅%20Full-ff7139?style=for-the-badge&labelColor=0a0a0f)
![Safari](https://img.shields.io/badge/Safari-✅%20Full-1472ba?style=for-the-badge&labelColor=0a0a0f)
![Edge](https://img.shields.io/badge/Edge-✅%20Full-00a4ef?style=for-the-badge&labelColor=0a0a0f)

</div>

---

## 🔐 Privacy & Security

✅ **100% Client-Side** - All computation in your browser  
✅ **No Video Recording** - Camera stream never stored  
✅ **Local ML Model** - MediaPipe runs on-device  
✅ **Zero Data Collection** - No analytics, no tracking  
✅ **Deploy Anywhere** - No backend infrastructure  

---

## 📦 Project Structure

```
Gargantua/
├── index.html          # ⭐ Everything in one file
└── README.md           # You are here
```

**Single-File Architecture:**
- HTML structure
- Embedded CSS
- Vanilla JavaScript (ES modules)
- Three.js scene setup
- MediaPipe integration
- Custom GLSL shaders

---

## 🎯 What Makes This Special

<div align="center">

| Aspect | Detail |
|--------|--------|
| **Performance** | 15,000 particles @ 60fps with custom WebGL shaders |
| **Interaction** | Scale-invariant gesture recognition - distance adaptive |
| **Privacy** | Zero backend - all ML runs client-side |
| **Tech** | No build step, no dependencies, pure ES modules |
| **Experience** | Inspired by Interstellar's gravity visualization |

</div>

---

## 🔗 Links & Resources

<div align="center">

### 🎨 Try It Now
[![Launch Demo](https://img.shields.io/badge/🚀%20LAUNCH%20LIVE%20DEMO-Click%20Here-00f0ff?style=for-the-badge&labelColor=0a0a0f&link=https://gargantua3d.vercel.app/)](https://gargantua3d.vercel.app/)

### 📚 Learn More
- [Three.js Documentation](https://threejs.org/)
- [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands)
- [WebGL & GLSL](https://www.khronos.org/opengl/wiki/OpenGL_Shading_Language)

### 👨‍💻 Creator
**[@Uditpandya07](https://github.com/Uditpandya07)**

Inspired by *Interstellar* (2014) - Where gravity bends light and time. Now it bends particles too. 🌌

</div>

---

<div align="center">

## ⭐ Show Your Support

If you find this project interesting:
- **Star** this repository
- **Share** with others
- **Fork** and experiment
- **Try** the live demo

---

### Made with 💜 and 🚀

**GARGANTUA** © 2024  
*"Gravity is the most powerful force in the universe. Let it move you."*

</div>
