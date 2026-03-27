<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GARGANTUA 🌌 Interactive Particle Physics Engine</title>
    <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;800&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --bg-dark: #0a0a0f;
            --bg-darker: #050508;
            --accent-cyan: #00f0ff;
            --accent-lime: #00ff88;
            --accent-purple: #a78bfa;
            --accent-pink: #ff3d7f;
            --text-primary: #e0e0ff;
            --text-secondary: #8b8ba8;
            --border-color: rgba(0, 240, 255, 0.15);
            --glow-cyan: 0 0 30px rgba(0, 240, 255, 0.3);
            --glow-lime: 0 0 30px rgba(0, 255, 136, 0.3);
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            background: linear-gradient(135deg, var(--bg-darker) 0%, #0f0620 100%);
            color: var(--text-primary);
            font-family: 'Syne', sans-serif;
            overflow-x: hidden;
            position: relative;
        }

        /* Animated background grid */
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-image: 
                linear-gradient(90deg, rgba(0, 240, 255, 0.05) 1px, transparent 1px),
                linear-gradient(0deg, rgba(0, 240, 255, 0.05) 1px, transparent 1px);
            background-size: 50px 50px;
            pointer-events: none;
            z-index: 0;
            animation: gridShift 20s linear infinite;
        }

        @keyframes gridShift {
            0% { transform: translate(0, 0); }
            100% { transform: translate(50px, 50px); }
        }

        /* Floating orbs background */
        body::after {
            content: '';
            position: fixed;
            top: -50%;
            right: -10%;
            width: 600px;
            height: 600px;
            background: radial-gradient(circle, rgba(0, 255, 136, 0.08) 0%, transparent 70%);
            border-radius: 50%;
            z-index: 0;
            animation: float 15s ease-in-out infinite;
            pointer-events: none;
        }

        @keyframes float {
            0%, 100% { transform: translate(0, 0) scale(1); }
            50% { transform: translate(30px, -40px) scale(1.1); }
        }

        /* Main container */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 40px;
            position: relative;
            z-index: 1;
        }

        /* Header / Navigation */
        header {
            padding: 40px 0;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
            backdrop-filter: blur(10px);
            background: rgba(10, 10, 15, 0.5);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .logo {
            font-size: 28px;
            font-weight: 800;
            font-family: 'JetBrains Mono', monospace;
            letter-spacing: 3px;
            background: linear-gradient(135deg, var(--accent-cyan), var(--accent-lime));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 0 0 40px rgba(0, 240, 255, 0.4);
            animation: logoPulse 3s ease-in-out infinite;
        }

        @keyframes logoPulse {
            0%, 100% { filter: drop-shadow(0 0 10px rgba(0, 240, 255, 0.3)); }
            50% { filter: drop-shadow(0 0 30px rgba(0, 255, 136, 0.5)); }
        }

        .nav-links {
            display: flex;
            gap: 30px;
            list-style: none;
        }

        .nav-links a {
            color: var(--text-secondary);
            text-decoration: none;
            font-size: 13px;
            font-weight: 600;
            letter-spacing: 1px;
            text-transform: uppercase;
            transition: all 0.3s ease;
            position: relative;
        }

        .nav-links a::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: linear-gradient(90deg, var(--accent-cyan), var(--accent-lime));
            transition: width 0.3s ease;
        }

        .nav-links a:hover::after {
            width: 100%;
        }

        .nav-links a:hover {
            color: var(--accent-cyan);
        }

        /* Hero Section */
        .hero {
            padding: 120px 0;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .hero::before {
            content: '';
            position: absolute;
            top: -100px;
            left: 50%;
            transform: translateX(-50%);
            width: 500px;
            height: 500px;
            background: radial-gradient(circle, rgba(167, 139, 250, 0.15) 0%, transparent 70%);
            border-radius: 50%;
            z-index: -1;
            animation: orbitSlow 25s linear infinite;
        }

        @keyframes orbitSlow {
            0% { transform: translateX(-50%) rotate(0deg); }
            100% { transform: translateX(-50%) rotate(360deg); }
        }

        .hero-badge {
            display: inline-block;
            padding: 10px 20px;
            background: rgba(0, 240, 255, 0.1);
            border: 1px solid var(--accent-cyan);
            border-radius: 100px;
            font-size: 12px;
            letter-spacing: 2px;
            color: var(--accent-cyan);
            margin-bottom: 30px;
            animation: slideUp 0.8s ease 0.2s backwards;
            font-weight: 600;
        }

        .hero h1 {
            font-size: clamp(48px, 10vw, 96px);
            font-weight: 800;
            line-height: 1.1;
            margin-bottom: 20px;
            letter-spacing: -2px;
            animation: slideUp 0.8s ease 0.3s backwards;
        }

        .hero h1 span {
            background: linear-gradient(135deg, var(--accent-cyan) 0%, var(--accent-lime) 50%, var(--accent-purple) 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            display: block;
        }

        .hero p {
            font-size: 18px;
            color: var(--text-secondary);
            max-width: 600px;
            margin: 0 auto 40px;
            line-height: 1.6;
            animation: slideUp 0.8s ease 0.4s backwards;
        }

        .cta-group {
            display: flex;
            gap: 20px;
            justify-content: center;
            flex-wrap: wrap;
            animation: slideUp 0.8s ease 0.5s backwards;
        }

        .cta-btn {
            padding: 15px 40px;
            border: none;
            border-radius: 8px;
            font-size: 14px;
            font-weight: 700;
            letter-spacing: 1px;
            text-transform: uppercase;
            cursor: pointer;
            transition: all 0.3s ease;
            font-family: 'JetBrains Mono', monospace;
            position: relative;
            overflow: hidden;
        }

        .cta-btn.primary {
            background: linear-gradient(135deg, var(--accent-cyan), var(--accent-lime));
            color: var(--bg-darker);
            box-shadow: var(--glow-cyan);
        }

        .cta-btn.primary:hover {
            transform: translateY(-4px);
            box-shadow: 0 0 50px rgba(0, 240, 255, 0.6), 0 0 80px rgba(0, 255, 136, 0.4);
        }

        .cta-btn.secondary {
            background: transparent;
            border: 2px solid var(--accent-cyan);
            color: var(--accent-cyan);
            box-shadow: inset 0 0 20px rgba(0, 240, 255, 0.05);
        }

        .cta-btn.secondary:hover {
            background: rgba(0, 240, 255, 0.1);
            transform: translateY(-4px);
            box-shadow: inset 0 0 20px rgba(0, 240, 255, 0.15), var(--glow-cyan);
        }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(40px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Scroll indicator */
        .scroll-hint {
            position: absolute;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            animation: fadeInOut 2s ease-in-out infinite;
        }

        .scroll-hint span {
            font-size: 12px;
            letter-spacing: 2px;
            color: var(--text-secondary);
            text-transform: uppercase;
        }

        .scroll-dot {
            width: 8px;
            height: 8px;
            background: var(--accent-cyan);
            border-radius: 50%;
            animation: bounce 2s ease-in-out infinite;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); opacity: 1; }
            50% { transform: translateY(10px); opacity: 0.3; }
        }

        @keyframes fadeInOut {
            0%, 100% { opacity: 0; }
            50% { opacity: 1; }
        }

        /* Stats Section */
        .stats {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 80px 0;
            padding: 40px;
            background: rgba(0, 240, 255, 0.03);
            border: 1px solid var(--border-color);
            border-radius: 12px;
        }

        .stat-item {
            text-align: center;
            padding: 20px;
            border-radius: 8px;
            background: rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(0, 240, 255, 0.1);
            animation: statReveal 0.6s ease backwards;
        }

        .stat-item:nth-child(1) { animation-delay: 0.1s; }
        .stat-item:nth-child(2) { animation-delay: 0.2s; }
        .stat-item:nth-child(3) { animation-delay: 0.3s; }
        .stat-item:nth-child(4) { animation-delay: 0.4s; }
        .stat-item:nth-child(5) { animation-delay: 0.5s; }

        @keyframes statReveal {
            from {
                opacity: 0;
                transform: translateY(20px) scale(0.9);
            }
            to {
                opacity: 1;
                transform: translateY(0) scale(1);
            }
        }

        .stat-num {
            font-size: 32px;
            font-weight: 800;
            color: var(--accent-lime);
            font-family: 'JetBrains Mono', monospace;
            margin-bottom: 8px;
        }

        .stat-label {
            font-size: 12px;
            color: var(--text-secondary);
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 600;
        }

        /* Features Section */
        .section {
            padding: 100px 0;
            border-top: 1px solid var(--border-color);
        }

        .section-title {
            font-size: clamp(32px, 6vw, 56px);
            font-weight: 800;
            margin-bottom: 20px;
            letter-spacing: -1px;
            line-height: 1.2;
        }

        .section-subtitle {
            font-size: 16px;
            color: var(--text-secondary);
            max-width: 600px;
            margin-bottom: 60px;
            line-height: 1.7;
        }

        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 30px;
        }

        .feature-card {
            padding: 40px;
            border: 1px solid var(--border-color);
            border-radius: 12px;
            background: linear-gradient(135deg, rgba(0, 240, 255, 0.02) 0%, rgba(0, 255, 136, 0.02) 100%);
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            overflow: hidden;
            animation: cardFadeIn 0.6s ease backwards;
        }

        .feature-card:nth-child(1) { animation-delay: 0.1s; }
        .feature-card:nth-child(2) { animation-delay: 0.2s; }
        .feature-card:nth-child(3) { animation-delay: 0.3s; }
        .feature-card:nth-child(4) { animation-delay: 0.4s; }
        .feature-card:nth-child(5) { animation-delay: 0.5s; }
        .feature-card:nth-child(6) { animation-delay: 0.6s; }

        @keyframes cardFadeIn {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .feature-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 2px;
            background: linear-gradient(90deg, transparent, var(--accent-cyan), transparent);
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .feature-card:hover::before {
            opacity: 1;
        }

        .feature-card:hover {
            background: linear-gradient(135deg, rgba(0, 240, 255, 0.08) 0%, rgba(0, 255, 136, 0.08) 100%);
            border-color: var(--accent-cyan);
            transform: translateY(-8px);
            box-shadow: var(--glow-cyan);
        }

        .feature-icon {
            font-size: 36px;
            margin-bottom: 20px;
            display: block;
        }

        .feature-title {
            font-size: 18px;
            font-weight: 700;
            margin-bottom: 12px;
            color: var(--accent-cyan);
        }

        .feature-desc {
            font-size: 14px;
            color: var(--text-secondary);
            line-height: 1.6;
        }

        .feature-tag {
            display: inline-block;
            margin-top: 16px;
            padding: 6px 12px;
            background: rgba(0, 240, 255, 0.1);
            border: 1px solid rgba(0, 240, 255, 0.2);
            border-radius: 4px;
            font-size: 11px;
            color: var(--accent-cyan);
            font-family: 'JetBrains Mono', monospace;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 600;
        }

        /* Tech Stack */
        .tech-stack {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 40px;
        }

        .tech-item {
            padding: 20px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            background: rgba(0, 240, 255, 0.02);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .tech-item::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0, 240, 255, 0.1), transparent);
            transition: left 0.5s ease;
        }

        .tech-item:hover::after {
            left: 100%;
        }

        .tech-item:hover {
            border-color: var(--accent-lime);
            background: rgba(0, 255, 136, 0.05);
            box-shadow: var(--glow-lime);
        }

        .tech-name {
            font-weight: 700;
            color: var(--accent-lime);
            margin-bottom: 8px;
            font-size: 14px;
        }

        .tech-desc {
            font-size: 13px;
            color: var(--text-secondary);
            line-height: 1.5;
        }

        /* Code Block */
        .code-block {
            background: rgba(0, 0, 0, 0.6);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            overflow: hidden;
            margin-top: 40px;
            animation: slideUp 0.8s ease 0.3s backwards;
        }

        .code-header {
            padding: 16px 24px;
            background: rgba(0, 240, 255, 0.05);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            gap: 8px;
            align-items: center;
        }

        .code-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
        }

        .code-dot.red { background: #ff5f57; }
        .code-dot.yellow { background: #febc2e; }
        .code-dot.green { background: #28c840; }

        .code-title {
            margin-left: 12px;
            font-size: 12px;
            color: var(--text-secondary);
            font-family: 'JetBrains Mono', monospace;
        }

        .code-body {
            padding: 24px;
            font-family: 'JetBrains Mono', monospace;
            font-size: 13px;
            line-height: 1.8;
            overflow-x: auto;
            color: var(--text-primary);
        }

        .code-body code {
            display: block;
            white-space: pre;
        }

        .keyword { color: var(--accent-purple); }
        .string { color: var(--accent-pink); }
        .function { color: var(--accent-lime); }
        .comment { color: var(--text-secondary); opacity: 0.7; }

        /* Getting Started */
        .getting-started {
            background: linear-gradient(135deg, rgba(0, 240, 255, 0.03) 0%, rgba(0, 255, 136, 0.03) 100%);
            border: 1px solid var(--border-color);
            border-radius: 12px;
            padding: 60px;
            margin-top: 60px;
        }

        .steps {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 40px;
            margin-top: 40px;
        }

        .step {
            position: relative;
            padding-left: 50px;
        }

        .step::before {
            content: attr(data-step);
            position: absolute;
            left: 0;
            top: 0;
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, var(--accent-cyan), var(--accent-lime));
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 800;
            color: var(--bg-darker);
            font-family: 'JetBrains Mono', monospace;
            font-size: 18px;
        }

        .step-title {
            font-size: 16px;
            font-weight: 700;
            margin-bottom: 8px;
            color: var(--accent-cyan);
        }

        .step-desc {
            font-size: 14px;
            color: var(--text-secondary);
            line-height: 1.6;
        }

        /* Footer */
        footer {
            border-top: 1px solid var(--border-color);
            padding: 60px 0;
            margin-top: 100px;
            text-align: center;
        }

        .footer-content {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 40px;
            margin-bottom: 40px;
        }

        .footer-section h3 {
            font-size: 14px;
            font-weight: 700;
            color: var(--accent-cyan);
            margin-bottom: 16px;
            letter-spacing: 1px;
            text-transform: uppercase;
        }

        .footer-section ul {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .footer-section a {
            color: var(--text-secondary);
            text-decoration: none;
            font-size: 13px;
            transition: color 0.3s ease;
        }

        .footer-section a:hover {
            color: var(--accent-lime);
        }

        .footer-bottom {
            padding-top: 40px;
            border-top: 1px solid var(--border-color);
            color: var(--text-secondary);
            font-size: 12px;
        }

        .footer-bottom a {
            color: var(--accent-cyan);
            text-decoration: none;
            transition: color 0.3s ease;
        }

        .footer-bottom a:hover {
            color: var(--accent-lime);
        }

        /* Responsive */
        @media (max-width: 768px) {
            .container {
                padding: 0 20px;
            }

            header {
                flex-direction: column;
                gap: 20px;
                align-items: flex-start;
            }

            .nav-links {
                gap: 15px;
                font-size: 11px;
            }

            .hero {
                padding: 60px 0;
            }

            .hero h1 {
                font-size: 40px;
            }

            .stats {
                grid-template-columns: repeat(2, 1fr);
            }

            .getting-started {
                padding: 40px 20px;
            }

            .steps {
                grid-template-columns: 1fr;
            }
        }

        /* Smooth scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(0, 240, 255, 0.05);
        }

        ::-webkit-scrollbar-thumb {
            background: var(--accent-cyan);
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: var(--accent-lime);
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header>
            <div class="logo">◆ GARGANTUA</div>
            <nav>
                <ul class="nav-links">
                    <li><a href="#features">Features</a></li>
                    <li><a href="#tech">Tech</a></li>
                    <li><a href="#started">Getting Started</a></li>
                    <li><a href="https://gargantua3d.vercel.app/" target="_blank">Live Demo</a></li>
                    <li><a href="https://github.com/Uditpandya07/Gargantua" target="_blank">GitHub</a></li>
                </ul>
            </nav>
        </header>

        <!-- Hero Section -->
        <section class="hero">
            <div class="hero-badge">🌌 Interactive Particle Physics</div>
            <h1>
                <span>GARGANTUA</span>
                <span style="font-size: 0.7em; color: var(--text-secondary);">Interactive 3D Particle Physics Engine</span>
            </h1>
            <p>Gravity-bending particles. Dual-hand tracking. Custom WebGL shaders. 15,000 particles dancing in real-time. Inspired by Nolan's Interstellar.</p>
            
            <div class="cta-group">
                <button class="cta-btn primary" onclick="window.open('https://gargantua3d.vercel.app/', '_blank')">
                    Launch Live Demo
                </button>
                <button class="cta-btn secondary" onclick="window.open('https://github.com/Uditpandya07/Gargantua', '_blank')">
                    View Source
                </button>
            </div>

            <div class="scroll-hint">
                <span>Scroll to explore</span>
                <div class="scroll-dot"></div>
            </div>
        </section>

        <!-- Stats Section -->
        <section class="stats">
            <div class="stat-item">
                <div class="stat-num">15K</div>
                <div class="stat-label">Particles</div>
            </div>
            <div class="stat-item">
                <div class="stat-num">60</div>
                <div class="stat-label">FPS Target</div>
            </div>
            <div class="stat-item">
                <div class="stat-num">4</div>
                <div class="stat-label">Morph Shapes</div>
            </div>
            <div class="stat-item">
                <div class="stat-num">2</div>
                <div class="stat-label">Hands Tracked</div>
            </div>
            <div class="stat-item">
                <div class="stat-num">0</div>
                <div class="stat-label">Backend Needed</div>
            </div>
        </section>

        <!-- Features Section -->
        <section id="features" class="section">
            <h2 class="section-title">Core <span style="color: var(--accent-lime);">Capabilities</span></h2>
            <p class="section-subtitle">Every technical decision optimizes for visual fidelity and responsiveness at 60fps with thousands of particles in motion.</p>

            <div class="features-grid">
                <div class="feature-card">
                    <span class="feature-icon">⚡</span>
                    <h3 class="feature-title">Custom GLSL Shaders</h3>
                    <p class="feature-desc">Hand-written vertex and fragment shaders render anti-aliased glowing spheres with minimal GPU overhead.</p>
                    <span class="feature-tag">WebGL</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">✋</span>
                    <h3 class="feature-title">Scale-Invariant Gestures</h3>
                    <p class="feature-desc">Mathematical normalization enables pinch, fist, and swipe gestures to work accurately at any distance from camera.</p>
                    <span class="feature-tag">MediaPipe</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🌀</span>
                    <h3 class="feature-title">Real-Time Physics Loop</h3>
                    <p class="feature-desc">Each particle lerps toward its shape target. Velocity dampens at 0.82×. Fist release creates explosive impulse.</p>
                    <span class="feature-tag">Physics</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">💫</span>
                    <h3 class="feature-title">UnrealBloom Post-Process</h3>
                    <p class="feature-desc">Three.js EffectComposer with UnrealBloomPass delivers signature cinematic neon glow at strength 1.6.</p>
                    <span class="feature-tag">Three.js</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🔒</span>
                    <h3 class="feature-title">Zero Server, Full Privacy</h3>
                    <p class="feature-desc">MediaPipe ML model runs entirely client-side. No video frames, no biometric data ever transmitted.</p>
                    <span class="feature-tag">Privacy</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🖱️</span>
                    <h3 class="feature-title">Mouse Fallback Mode</h3>
                    <p class="feature-desc">If camera access fails, seamlessly switches to full-featured mouse-controlled physics mode.</p>
                    <span class="feature-tag">Fallback</span>
                </div>
            </div>
        </section>

        <!-- Interactions Section -->
        <section class="section">
            <h2 class="section-title">Gesture <span style="color: var(--accent-lime);">Library</span></h2>
            <p class="section-subtitle">Two-handed real-time tracking enables a tactile space you can physically touch, scale, twist, and collapse.</p>

            <div class="features-grid">
                <div class="feature-card">
                    <span class="feature-icon">🤏</span>
                    <h3 class="feature-title">Pinch & Drag</h3>
                    <p class="feature-desc">Grab and freely drag the gravity center across the screen with intuitive thumb-to-finger control.</p>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">✊</span>
                    <h3 class="feature-title">Fist — Gravity Well</h3>
                    <p class="feature-desc">Condense the entire galaxy to a singularity. Release for an explosive supernova effect.</p>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">👋</span>
                    <h3 class="feature-title">Swipe Left/Right</h3>
                    <p class="feature-desc">Quick horizontal swipe morphs particles to the next shape in sequence.</p>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">↔️</span>
                    <h3 class="feature-title">Pull to Scale</h3>
                    <p class="feature-desc">Spread both hands apart to expand the galaxy; push together to shrink it.</p>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🏎️</span>
                    <h3 class="feature-title">Twist Rotation</h3>
                    <p class="feature-desc">Rotate both hands like a steering wheel to spin the galaxy on the Z-axis.</p>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🤲</span>
                    <h3 class="feature-title">Center of Gravity</h3>
                    <p class="feature-desc">The midpoint between both hands becomes the attractor origin for all particles.</p>
                </div>
            </div>
        </section>

        <!-- Shapes Section -->
        <section class="section">
            <h2 class="section-title">Morph <span style="color: var(--accent-lime);">Targets</span></h2>
            <p class="section-subtitle">Four mathematically precise universes. 15,000 particles distributed across parametric equations.</p>

            <div class="features-grid">
                <div class="feature-card">
                    <span class="feature-icon">☁️</span>
                    <h3 class="feature-title">Nebula</h3>
                    <p class="feature-desc">Random spherical distribution with radial density gradient. The default home state where particles rest.</p>
                    <span class="feature-tag">Default</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🧬</span>
                    <h3 class="feature-title">DNA Helix</h3>
                    <p class="feature-desc">Double helix structure with parametric strand offset and biological base-pair cross-links.</p>
                    <span class="feature-tag">cos(t) + cos(t+π)</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🌌</span>
                    <h3 class="feature-title">Galactic Core</h3>
                    <p class="feature-desc">Logarithmic spiral arms with exponential density falloff from the central black hole singularity.</p>
                    <span class="feature-tag">r·e^(b·θ)</span>
                </div>

                <div class="feature-card">
                    <span class="feature-icon">🔗</span>
                    <h3 class="feature-title">Torus Knot</h3>
                    <p class="feature-desc">A complex (2,3) torus knot with volumetric particle distribution surrounding the topological curve.</p>
                    <span class="feature-tag">(R+r·cos(nt))cos(t)</span>
                </div>
            </div>
        </section>

        <!-- Tech Stack Section -->
        <section id="tech" class="section">
            <h2 class="section-title">Tech <span style="color: var(--accent-lime);">Stack</span></h2>
            <p class="section-subtitle">A single HTML file with no bundler, no framework, no dependencies. Pure ES modules loaded from CDN.</p>

            <div class="tech-stack">
                <div class="tech-item">
                    <div class="tech-name">Three.js (r158)</div>
                    <div class="tech-desc">3D rendering pipeline, camera controls, and post-processing effects.</div>
                </div>

                <div class="tech-item">
                    <div class="tech-name">Custom GLSL Shaders</div>
                    <div class="tech-desc">Hand-written vertex and fragment shaders for particle rendering.</div>
                </div>

                <div class="tech-item">
                    <div class="tech-name">MediaPipe Hands</div>
                    <div class="tech-desc">Client-side ML model for skeletal hand tracking (runs entirely in browser).</div>
                </div>

                <div class="tech-item">
                    <div class="tech-name">UnrealBloomPass</div>
                    <div class="tech-desc">Three.js EffectComposer post-processing for cinematic glow effects.</div>
                </div>

                <div class="tech-item">
                    <div class="tech-name">ES Modules</div>
                    <div class="tech-desc">Native browser module imports. No webpack, no bundler required.</div>
                </div>

                <div class="tech-item">
                    <div class="tech-name">WebGL</div>
                    <div class="tech-desc">Low-level GPU access for custom vertex/fragment shader pipeline.</div>
                </div>
            </div>

            <div class="code-block">
                <div class="code-header">
                    <span class="code-dot red"></span>
                    <span class="code-dot yellow"></span>
                    <span class="code-dot green"></span>
                    <span class="code-title">shader.glsl — particle rendering</span>
                </div>
                <div class="code-body"><code><span class="comment">// Vertex Shader</span>
<span class="keyword">uniform</span> <span class="keyword">float</span> <span class="function">time</span>;
<span class="keyword">attribute</span> <span class="keyword">vec3</span> <span class="function">color</span>;
<span class="keyword">varying</span> <span class="keyword">vec3</span> <span class="function">vColor</span>;

<span class="keyword">void</span> <span class="function">main</span>() {
    <span class="function">vColor</span> = <span class="function">color</span>;
    <span class="keyword">vec4</span> mvPos = modelViewMatrix * <span class="keyword">vec4</span>(position, <span class="string">1.0</span>);
    gl_PointSize = <span class="string">3.0</span> * (<span class="string">300.0</span> / -mvPos.z);
    gl_Position = projectionMatrix * mvPos;
}

<span class="comment">// Fragment Shader</span>
<span class="keyword">void</span> <span class="function">main</span>() {
    <span class="keyword">vec2</span> uv = gl_PointCoord - <span class="string">0.5</span>;
    <span class="keyword">float</span> d = length(uv);
    <span class="keyword">if</span> (d > <span class="string">0.5</span>) discard;
    <span class="keyword">float</span> glow = <span class="string">1.0</span> - smoothstep(<span class="string">0.0</span>, <span class="string">0.5</span>, d);
    gl_FragColor = <span class="keyword">vec4</span>(<span class="function">vColor</span>, glow * glow);
}</code></div>
            </div>
        </section>

        <!-- Getting Started Section -->
        <section id="started" class="getting-started">
            <h2 class="section-title">Getting <span style="color: var(--accent-lime);">Started</span></h2>
            <p class="section-subtitle" style="margin-bottom: 30px;">Browsers block webcam access for <code style="background: rgba(0,240,255,0.1); padding: 2px 8px; border-radius: 3px; color: var(--accent-cyan); font-family: 'JetBrains Mono', monospace;">file://</code> URLs. Use any local server below.</p>

            <div class="steps">
                <div class="step" data-step="1">
                    <div class="step-title">Clone Repository</div>
                    <div class="step-desc">Pull the project from GitHub to your local machine.</div>
                </div>

                <div class="step" data-step="2">
                    <div class="step-title">Spin Up Server</div>
                    <div class="step-desc">Use Python's http.server, Node's http-server, or VS Code Live Server on port 8000.</div>
                </div>

                <div class="step" data-step="3">
                    <div class="step-title">Open & Allow Camera</div>
                    <div class="step-desc">Navigate to <code style="background: rgba(0,240,255,0.1); padding: 2px 8px; border-radius: 3px; color: var(--accent-cyan); font-family: 'JetBrains Mono', monospace;">localhost:8000</code> and grant camera access. Boots in ~4 seconds.</div>
                </div>

                <div class="step" data-step="4">
                    <div class="step-title">Sculpt the Galaxy</div>
                    <div class="step-desc">Hold up your hands and begin. Mouse mode activates automatically if camera unavailable.</div>
                </div>
            </div>

            <div class="code-block" style="margin-top: 40px;">
                <div class="code-header">
                    <span class="code-dot red"></span>
                    <span class="code-dot yellow"></span>
                    <span class="code-dot green"></span>
                    <span class="code-title">bash — quick setup</span>
                </div>
                <div class="code-body"><code><span class="function">$</span> git clone https://github.com/Uditpandya07/Gargantua.git
<span class="function">$</span> cd Gargantua

<span class="comment"># Option A — Python 3</span>
<span class="function">$</span> python3 -m http.server 8000

<span class="comment"># Option B — Node</span>
<span class="function">$</span> npx http-server -p 8000

<span class="comment"># Then open in browser</span>
<span class="function">$</span> open http://localhost:8000</code></div>
            </div>
        </section>

        <!-- Footer -->
        <footer>
            <div class="footer-content">
                <div class="footer-section">
                    <h3>Project</h3>
                    <ul>
                        <li><a href="https://gargantua3d.vercel.app/">Live Demo</a></li>
                        <li><a href="https://github.com/Uditpandya07/Gargantua">GitHub Repository</a></li>
                        <li><a href="https://github.com/Uditpandya07">Creator</a></li>
                    </ul>
                </div>
                <div class="footer-section">
                    <h3>Resources</h3>
                    <ul>
                        <li><a href="https://threejs.org/">Three.js Docs</a></li>
                        <li><a href="https://mediapipe.dev/">MediaPipe</a></li>
                        <li><a href="https://www.khronos.org/opengl/wiki/OpenGL_Shading_Language">GLSL Reference</a></li>
                    </ul>
                </div>
                <div class="footer-section">
                    <h3>Inspiration</h3>
                    <ul>
                        <li><a href="#">Interstellar (Nolan)</a></li>
                        <li><a href="#">Particle Physics</a></li>
                        <li><a href="#">WebGL Innovation</a></li>
                    </ul>
                </div>
            </div>

            <div class="footer-bottom">
                <p>Built with 🌌 by <a href="https://github.com/Uditpandya07">@Uditpandya07</a> — Inspired by Interstellar</p>
                <p style="margin-top: 12px; font-size: 11px; opacity: 0.6;">© 2024 GARGANTUA. All rights reserved.</p>
            </div>
        </footer>
    </div>

    <script>
        // Smooth scroll for nav links
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                const href = this.getAttribute('href');
                if (href !== '#') {
                    e.preventDefault();
                    const element = document.querySelector(href);
                    if (element) {
                        element.scrollIntoView({ behavior: 'smooth' });
                    }
                }
            });
        });

        // Add scroll animation for elements
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -100px 0px'
        };

        const observer = new IntersectionObserver(function(entries) {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);

        document.querySelectorAll('.feature-card, .tech-item, .step').forEach(el => {
            el.style.opacity = '0';
            el.style.transform = 'translateY(20px)';
            el.style.transition = 'opacity 0.6s ease, transform 0.6s ease';
            observer.observe(el);
        });
    </script>
</body>
</html>