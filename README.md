# 🌌 Gargantua.js

> An interactive, dual-hand tracked 3D particle physics engine built with Three.js and MediaPipe. Inspired by the gravity-bending physics of Nolan's *Interstellar*.

I built this as an experiment to see how fluid and responsive browser-based motion tracking could feel when paired with thousands of particles. Instead of just making a passive visualizer, I wanted a digital space you could actually "touch" and manipulate with real-world physics.

If your camera fails, is blocked, or you just want to test it out quickly without granting permissions, the engine will automatically fall back to a mouse-controlled physics mode after a 4-second timeout.

## ✨ Features & Controls

The engine uses a custom scale-invariant math model. This means your gestures trigger accurately whether you're sitting 1 foot or 5 feet away from your webcam.

**Single Hand Tracking:**
* 🤏 **Pinch & Drag:** Grab and freely drag the gravity center around the screen.
* ✊ **Fist (Gravity Well):** Condenses the entire galaxy into a dense, glowing singularity. Release your fist to trigger an explosive supernova scatter.
* 👋 **Swipe Left/Right:** A quick horizontal swipe morphs the particles into a new mathematical shape.

**Dual Hand Tracking:**
* 🤲 **Link:** Bring both hands into frame to calculate the center of gravity between them.
* ↔️ **Pull/Push:** Spread your hands apart to scale the galaxy up; push them together to shrink it.
* 🏎️ **Twist:** Rotate your hands like a steering wheel to physically twist the galaxy on its Z-axis.

**Current Morph Targets:**
- Nebula
- DNA Helix
- Galactic Core
- 3D Torus Knot

## 🛠️ Under the Hood

* **Three.js & Post-Processing:** Handles the 3D rendering pipeline and the `UnrealBloomPass` for that cinematic neon glow.
* **Custom WebGL Shaders:** Standard Three.js point materials were tanking the framerate when pushing past 10,000 particles. I ripped them out and wrote a custom Vertex and Fragment shader to render mathematically perfect, anti-aliased glowing spheres. It looks drastically better and runs at a buttery 60fps.
* **MediaPipe Hands:** Google's lightweight machine learning model runs the skeletal hand tracking purely on the client side. No video data is ever sent to a server.

## 🚀 How to Run It Locally

Because this project uses your webcam and modern ES modules, **you cannot just double-click the `index.html` file**. Browsers will silently block the camera for security reasons if the URL starts with `file:///`.

You need to serve it over localhost.

1. Clone this repo:
   ```bash
   git clone [https://github.com/YOUR-USERNAME/Gargantua.git](https://github.com/YOUR-USERNAME/Gargantua.git)