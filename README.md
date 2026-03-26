<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GARGANTUA — Interactive Particle Physics Engine</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;700;900&family=Outfit:wght@300;400;500;600&family=Space+Mono:ital@0;1&display=swap" rel="stylesheet">
<style>
  :root {
    --void: #020204;
    --deep: #07070f;
    --surface: #0e0e1a;
    --card: #111120;
    --border: rgba(255,255,255,0.06);
    --border-glow: rgba(0,255,170,0.2);
    --accent: #00ffaa;
    --accent2: #7c6bff;
    --accent3: #ff3d7f;
    --star: #c8d8ff;
    --text: #e8eaf6;
    --muted: #6b7280;
    --glow: 0 0 40px rgba(0,255,170,0.15);
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--void);
    color: var(--text);
    font-family: 'Outfit', sans-serif;
    font-weight: 400;
    line-height: 1.7;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  .cursor {
    width: 10px; height: 10px;
    background: var(--accent);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%, -50%);
    transition: transform 0.1s, width 0.2s, height 0.2s, opacity 0.2s;
    box-shadow: 0 0 20px var(--accent), 0 0 40px rgba(0,255,170,0.4);
  }
  .cursor-ring {
    width: 36px; height: 36px;
    border: 1px solid rgba(0,255,170,0.4);
    border-radius: 50%;
    position: fixed;
    pointer-events: none;
    z-index: 9998;
    transform: translate(-50%, -50%);
    transition: transform 0.15s ease-out, width 0.2s, height 0.2s;
  }

  /* Stars canvas */
  #starsCanvas {
    position: fixed;
    inset: 0;
    z-index: 0;
    pointer-events: none;
  }

  /* Noise overlay */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 1;
    opacity: 0.4;
  }

  /* ── NAV ── */
  nav {
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    padding: 18px 60px;
    display: flex; align-items: center; justify-content: space-between;
    background: rgba(2,2,4,0.6);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--border);
  }
  .nav-logo {
    font-family: 'Orbitron', monospace;
    font-size: 13px; font-weight: 700;
    letter-spacing: 5px;
    color: var(--accent);
    text-transform: uppercase;
    text-shadow: 0 0 20px rgba(0,255,170,0.6);
  }
  .nav-links {
    display: flex; gap: 36px; list-style: none;
  }
  .nav-links a {
    font-size: 12px; letter-spacing: 2px; text-transform: uppercase;
    color: var(--muted); text-decoration: none;
    transition: color 0.3s;
    font-family: 'Space Mono', monospace;
  }
  .nav-links a:hover { color: var(--accent); }
  .nav-cta {
    padding: 9px 22px;
    border: 1px solid rgba(0,255,170,0.4);
    border-radius: 4px;
    color: var(--accent) !important;
    font-size: 11px !important;
    letter-spacing: 2px !important;
    transition: background 0.3s, box-shadow 0.3s !important;
  }
  .nav-cta:hover {
    background: rgba(0,255,170,0.1) !important;
    box-shadow: 0 0 20px rgba(0,255,170,0.2) !important;
  }

  /* ── SECTIONS ── */
  section { position: relative; z-index: 2; }

  /* ── HERO ── */
  .hero {
    min-height: 100vh;
    display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    text-align: center;
    padding: 120px 40px 80px;
    position: relative;
  }
  .hero-eyebrow {
    font-family: 'Space Mono', monospace;
    font-size: 11px; letter-spacing: 5px; text-transform: uppercase;
    color: var(--accent); margin-bottom: 28px;
    opacity: 0; animation: fadeUp 0.8s 0.2s forwards;
  }
  .hero-eyebrow span {
    display: inline-block;
    width: 40px; height: 1px;
    background: var(--accent);
    vertical-align: middle;
    margin: 0 12px;
    box-shadow: 0 0 10px var(--accent);
  }
  .hero-title {
    font-family: 'Orbitron', monospace;
    font-size: clamp(64px, 12vw, 160px);
    font-weight: 900;
    line-height: 0.9;
    letter-spacing: -2px;
    margin-bottom: 12px;
    opacity: 0; animation: fadeUp 0.9s 0.4s forwards;
  }
  .hero-title .line1 {
    display: block;
    background: linear-gradient(135deg, #ffffff 0%, rgba(255,255,255,0.5) 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .hero-title .line2 {
    display: block;
    background: linear-gradient(135deg, var(--accent) 0%, var(--accent2) 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    filter: drop-shadow(0 0 30px rgba(0,255,170,0.4));
  }
  .hero-sub {
    font-size: 16px; color: var(--muted); max-width: 560px;
    margin: 28px auto 48px;
    opacity: 0; animation: fadeUp 0.9s 0.6s forwards;
    line-height: 1.8;
  }
  .hero-actions {
    display: flex; gap: 16px; justify-content: center;
    opacity: 0; animation: fadeUp 0.9s 0.8s forwards;
  }
  .btn-primary {
    padding: 14px 36px;
    background: var(--accent);
    color: #000; font-weight: 700;
    font-family: 'Orbitron', monospace;
    font-size: 11px; letter-spacing: 3px; text-transform: uppercase;
    border: none; border-radius: 4px;
    text-decoration: none;
    transition: box-shadow 0.3s, transform 0.2s;
    cursor: none;
  }
  .btn-primary:hover {
    box-shadow: 0 0 40px rgba(0,255,170,0.5), 0 0 80px rgba(0,255,170,0.2);
    transform: translateY(-2px);
  }
  .btn-outline {
    padding: 14px 36px;
    background: transparent;
    color: var(--text);
    font-family: 'Orbitron', monospace;
    font-size: 11px; letter-spacing: 3px; text-transform: uppercase;
    border: 1px solid var(--border);
    border-radius: 4px;
    text-decoration: none;
    transition: border-color 0.3s, color 0.3s, transform 0.2s;
    cursor: none;
  }
  .btn-outline:hover {
    border-color: rgba(255,255,255,0.3);
    color: #fff;
    transform: translateY(-2px);
  }

  /* Orbital ring decoration */
  .orbital-ring {
    position: absolute;
    border-radius: 50%;
    border: 1px solid;
    pointer-events: none;
    animation: orbit 20s linear infinite;
  }
  .ring1 {
    width: 500px; height: 500px;
    border-color: rgba(0,255,170,0.06);
    top: 50%; left: 50%;
    transform: translate(-50%, -50%) rotateX(75deg);
    animation-duration: 25s;
  }
  .ring2 {
    width: 700px; height: 700px;
    border-color: rgba(124,107,255,0.05);
    top: 50%; left: 50%;
    transform: translate(-50%, -50%) rotateX(75deg);
    animation-duration: 40s;
    animation-direction: reverse;
  }
  .ring3 {
    width: 300px; height: 300px;
    border-color: rgba(0,255,170,0.08);
    top: 50%; left: 50%;
    transform: translate(-50%, -50%) rotateX(75deg);
    animation-duration: 15s;
  }
  @keyframes orbit { to { transform: translate(-50%, -50%) rotateX(75deg) rotateZ(360deg); } }

  /* Hero glow */
  .hero-glow {
    position: absolute;
    width: 600px; height: 600px;
    background: radial-gradient(circle, rgba(0,255,170,0.06) 0%, transparent 70%);
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    pointer-events: none;
  }

  /* Scroll indicator */
  .scroll-hint {
    position: absolute; bottom: 36px; left: 50%;
    transform: translateX(-50%);
    display: flex; flex-direction: column; align-items: center; gap: 8px;
    opacity: 0; animation: fadeUp 1s 1.2s forwards;
  }
  .scroll-hint span {
    font-family: 'Space Mono', monospace;
    font-size: 10px; letter-spacing: 3px; color: var(--muted);
    text-transform: uppercase;
  }
  .scroll-line {
    width: 1px; height: 50px;
    background: linear-gradient(to bottom, var(--accent), transparent);
    animation: scrollPulse 2s ease-in-out infinite;
  }
  @keyframes scrollPulse { 0%,100%{opacity:0.4;transform:scaleY(1)} 50%{opacity:1;transform:scaleY(1.1)} }

  /* ── STATS BAR ── */
  .stats-bar {
    border-top: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
    background: var(--deep);
    padding: 28px 60px;
    display: flex; justify-content: center; gap: 80px;
  }
  .stat {
    display: flex; flex-direction: column; align-items: center; gap: 4px;
    opacity: 0; transform: translateY(20px);
    transition: opacity 0.6s, transform 0.6s;
  }
  .stat.visible { opacity: 1; transform: none; }
  .stat-num {
    font-family: 'Orbitron', monospace;
    font-size: 28px; font-weight: 700;
    color: var(--accent);
    text-shadow: 0 0 20px rgba(0,255,170,0.4);
  }
  .stat-label {
    font-size: 11px; letter-spacing: 2px;
    text-transform: uppercase; color: var(--muted);
    font-family: 'Space Mono', monospace;
  }

  /* ── SECTION LAYOUT ── */
  .section-wrap {
    max-width: 1200px; margin: 0 auto;
    padding: 100px 60px;
  }
  .section-tag {
    font-family: 'Space Mono', monospace;
    font-size: 10px; letter-spacing: 4px; text-transform: uppercase;
    color: var(--accent);
    display: flex; align-items: center; gap: 12px;
    margin-bottom: 20px;
  }
  .section-tag::before {
    content: '';
    display: block; width: 24px; height: 1px;
    background: var(--accent);
    box-shadow: 0 0 8px var(--accent);
  }
  .section-title {
    font-family: 'Orbitron', monospace;
    font-size: clamp(32px, 4vw, 52px);
    font-weight: 700; line-height: 1.1;
    margin-bottom: 20px;
    letter-spacing: -1px;
  }
  .section-title em {
    font-style: normal;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .section-desc {
    color: var(--muted); font-size: 16px; max-width: 520px;
    line-height: 1.8; margin-bottom: 60px;
  }

  /* ── FEATURES ── */
  .features-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2px;
    background: var(--border);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
  }
  .feature-card {
    background: var(--card);
    padding: 40px 36px;
    position: relative;
    overflow: hidden;
    transition: background 0.3s;
  }
  .feature-card::before {
    content: '';
    position: absolute; inset: 0;
    background: radial-gradient(circle at 50% 0%, rgba(0,255,170,0.06) 0%, transparent 60%);
    opacity: 0; transition: opacity 0.4s;
  }
  .feature-card:hover { background: #131325; }
  .feature-card:hover::before { opacity: 1; }
  .feature-card:hover .feature-icon { transform: scale(1.1); }

  .feature-number {
    font-family: 'Orbitron', monospace;
    font-size: 11px; font-weight: 700;
    color: rgba(0,255,170,0.3);
    letter-spacing: 3px;
    margin-bottom: 24px;
  }
  .feature-icon {
    font-size: 36px; margin-bottom: 20px;
    display: block;
    transition: transform 0.3s;
    filter: drop-shadow(0 0 12px rgba(0,255,170,0.3));
  }
  .feature-title {
    font-family: 'Orbitron', monospace;
    font-size: 14px; font-weight: 600;
    letter-spacing: 1px; margin-bottom: 12px;
    color: #fff;
  }
  .feature-desc {
    font-size: 13px; color: var(--muted);
    line-height: 1.7;
  }
  .feature-tag {
    display: inline-block;
    margin-top: 16px;
    padding: 4px 10px;
    background: rgba(0,255,170,0.08);
    border: 1px solid rgba(0,255,170,0.15);
    border-radius: 3px;
    font-size: 10px; letter-spacing: 2px;
    color: rgba(0,255,170,0.7);
    font-family: 'Space Mono', monospace;
    text-transform: uppercase;
  }

  /* ── CONTROLS SECTION ── */
  .controls-layout {
    display: grid; grid-template-columns: 1fr 1fr; gap: 60px; align-items: start;
  }
  .controls-visual {
    position: relative;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    aspect-ratio: 4/3;
    display: flex; align-items: center; justify-content: center;
  }
  .controls-visual canvas {
    position: absolute; inset: 0; width: 100%; height: 100%;
  }
  .controls-visual-label {
    position: absolute; bottom: 16px; right: 16px;
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: rgba(0,255,170,0.4);
    letter-spacing: 2px;
  }

  .gestures-list { display: flex; flex-direction: column; gap: 3px; }
  .gesture-item {
    display: flex; align-items: center; gap: 16px;
    padding: 16px 20px;
    border: 1px solid transparent;
    border-radius: 8px;
    transition: background 0.3s, border-color 0.3s, transform 0.2s;
    cursor: default;
  }
  .gesture-item:hover {
    background: rgba(255,255,255,0.02);
    border-color: var(--border-glow);
    transform: translateX(6px);
  }
  .gesture-emoji {
    font-size: 26px; width: 40px; text-align: center;
    flex-shrink: 0;
  }
  .gesture-info { flex: 1; }
  .gesture-name {
    font-family: 'Orbitron', monospace;
    font-size: 12px; font-weight: 600;
    color: #fff; margin-bottom: 2px;
    letter-spacing: 0.5px;
  }
  .gesture-desc { font-size: 12px; color: var(--muted); }
  .gesture-mode {
    font-family: 'Space Mono', monospace;
    font-size: 9px; padding: 3px 8px;
    border-radius: 3px; letter-spacing: 1px;
    text-transform: uppercase; flex-shrink: 0;
  }
  .mode-single { background: rgba(0,255,170,0.08); color: rgba(0,255,170,0.6); border: 1px solid rgba(0,255,170,0.15); }
  .mode-dual { background: rgba(124,107,255,0.08); color: rgba(124,107,255,0.7); border: 1px solid rgba(124,107,255,0.2); }

  /* ── SHAPES SECTION ── */
  .shapes-section { background: var(--deep); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
  .shapes-grid {
    display: grid; grid-template-columns: repeat(4, 1fr); gap: 2px;
    background: var(--border);
    border-radius: 12px; overflow: hidden;
    border: 1px solid var(--border);
  }
  .shape-card {
    background: var(--card);
    padding: 40px 28px 32px;
    display: flex; flex-direction: column; gap: 16px;
    position: relative; overflow: hidden;
    transition: background 0.3s;
  }
  .shape-card:hover { background: #131325; }
  .shape-canvas-wrap {
    width: 100%; aspect-ratio: 1;
    display: flex; align-items: center; justify-content: center;
    margin-bottom: 8px;
    position: relative;
  }
  .shape-canvas-wrap canvas {
    display: block;
  }
  .shape-index {
    position: absolute; top: 14px; right: 14px;
    font-family: 'Orbitron', monospace;
    font-size: 10px; color: rgba(255,255,255,0.12);
    letter-spacing: 2px;
  }
  .shape-title {
    font-family: 'Orbitron', monospace;
    font-size: 13px; font-weight: 600;
    color: #fff; letter-spacing: 1px;
  }
  .shape-desc { font-size: 12px; color: var(--muted); line-height: 1.6; }
  .shape-formula {
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: rgba(0,255,170,0.4);
    margin-top: auto;
  }

  /* ── TECH STACK ── */
  .tech-layout {
    display: grid; grid-template-columns: 1fr 1.4fr; gap: 80px; align-items: center;
  }
  .tech-list { display: flex; flex-direction: column; gap: 2px; }
  .tech-item {
    display: flex; align-items: center;
    padding: 20px 24px; gap: 20px;
    border: 1px solid transparent;
    border-radius: 8px;
    transition: all 0.3s;
    cursor: default;
  }
  .tech-item:hover {
    background: var(--card);
    border-color: var(--border-glow);
  }
  .tech-dot {
    width: 8px; height: 8px; border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 12px var(--accent);
    flex-shrink: 0;
    transition: transform 0.3s;
  }
  .tech-item:hover .tech-dot { transform: scale(1.5); }
  .tech-name {
    font-family: 'Orbitron', monospace;
    font-size: 13px; font-weight: 600;
    color: #fff; flex: 1;
  }
  .tech-role { font-size: 12px; color: var(--muted); flex: 2; }
  .tech-badge {
    font-family: 'Space Mono', monospace;
    font-size: 9px; padding: 4px 10px;
    background: rgba(255,255,255,0.04);
    border: 1px solid var(--border);
    border-radius: 3px; color: var(--muted);
    letter-spacing: 1px; text-transform: uppercase;
  }
  .code-block {
    background: #080812;
    border: 1px solid var(--border);
    border-radius: 12px; overflow: hidden;
  }
  .code-header {
    padding: 14px 20px;
    border-bottom: 1px solid var(--border);
    background: rgba(255,255,255,0.02);
    display: flex; align-items: center; gap: 8px;
  }
  .code-dot { width: 10px; height: 10px; border-radius: 50%; }
  .cd1 { background: #ff5f57; }
  .cd2 { background: #febc2e; }
  .cd3 { background: #28c840; }
  .code-title {
    font-family: 'Space Mono', monospace;
    font-size: 11px; color: var(--muted);
    margin-left: 8px; letter-spacing: 1px;
  }
  .code-body {
    padding: 28px;
    font-family: 'Space Mono', monospace;
    font-size: 12px; line-height: 2;
    overflow-x: auto;
  }
  .c-keyword { color: #7c6bff; }
  .c-string { color: #ff9161; }
  .c-fn { color: #00ffaa; }
  .c-comment { color: #3d3d5c; font-style: italic; }
  .c-var { color: #c8d8ff; }
  .c-num { color: #ff3d7f; }

  /* ── HOW TO RUN ── */
  .run-layout {
    display: grid; grid-template-columns: 1fr 1fr; gap: 60px;
  }
  .steps-list { display: flex; flex-direction: column; gap: 0; }
  .step {
    display: flex; gap: 24px;
    padding-bottom: 36px;
    position: relative;
  }
  .step:not(:last-child)::after {
    content: '';
    position: absolute;
    left: 19px; top: 42px; bottom: 0;
    width: 1px;
    background: linear-gradient(to bottom, rgba(0,255,170,0.3), transparent);
  }
  .step-num {
    width: 40px; height: 40px; flex-shrink: 0;
    border-radius: 50%;
    border: 1px solid rgba(0,255,170,0.3);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Orbitron', monospace;
    font-size: 12px; font-weight: 700; color: var(--accent);
    background: rgba(0,255,170,0.05);
    position: relative; z-index: 1;
  }
  .step-content { flex: 1; padding-top: 8px; }
  .step-title {
    font-family: 'Orbitron', monospace;
    font-size: 14px; font-weight: 600;
    color: #fff; margin-bottom: 6px;
  }
  .step-desc { font-size: 13px; color: var(--muted); line-height: 1.7; }
  .terminal {
    background: #030308;
    border: 1px solid var(--border);
    border-radius: 10px; overflow: hidden;
  }
  .terminal-header {
    padding: 12px 16px;
    background: rgba(255,255,255,0.02);
    border-bottom: 1px solid var(--border);
    display: flex; align-items: center; gap: 8px;
  }
  .t-dot { width: 10px; height: 10px; border-radius: 50%; }
  .td1{background:#ff5f57} .td2{background:#febc2e} .td3{background:#28c840}
  .terminal-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: var(--muted);
    margin-left: 8px; letter-spacing: 1px;
  }
  .terminal-body { padding: 24px 20px; }
  .terminal-line {
    font-family: 'Space Mono', monospace;
    font-size: 12px; line-height: 2;
    display: flex; gap: 10px;
    white-space: nowrap; overflow-x: auto;
  }
  .t-prompt { color: var(--accent); }
  .t-cmd { color: #e8eaf6; }
  .t-comment { color: #3d3d5c; }
  .t-out { color: var(--muted); padding-left: 20px; }

  /* ── FOOTER ── */
  footer {
    border-top: 1px solid var(--border);
    padding: 60px;
    display: flex; align-items: center; justify-content: space-between;
    position: relative; z-index: 2;
    background: var(--deep);
  }
  .footer-logo {
    font-family: 'Orbitron', monospace;
    font-size: 18px; font-weight: 900;
    letter-spacing: 4px;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
  }
  .footer-links {
    display: flex; gap: 32px; list-style: none;
  }
  .footer-links a {
    font-family: 'Space Mono', monospace;
    font-size: 11px; letter-spacing: 2px; text-transform: uppercase;
    color: var(--muted); text-decoration: none;
    transition: color 0.3s;
  }
  .footer-links a:hover { color: var(--accent); }
  .footer-credit {
    font-size: 12px; color: var(--muted);
    font-family: 'Space Mono', monospace;
  }
  .footer-credit a { color: var(--accent); text-decoration: none; }

  /* ── DIVIDER ── */
  .cosmic-divider {
    height: 1px;
    background: linear-gradient(to right, transparent, var(--accent), transparent);
    opacity: 0.2;
    margin: 0;
    position: relative; z-index: 2;
  }

  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
  }
  .reveal {
    opacity: 0; transform: translateY(40px);
    transition: opacity 0.7s ease, transform 0.7s ease;
  }
  .reveal.visible { opacity: 1; transform: none; }
  .reveal-d1 { transition-delay: 0.1s; }
  .reveal-d2 { transition-delay: 0.2s; }
  .reveal-d3 { transition-delay: 0.3s; }

  /* Glow separator */
  .glow-orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(80px);
    pointer-events: none; z-index: 0;
  }

  /* Live badge */
  .live-badge {
    display: inline-flex; align-items: center; gap: 8px;
    padding: 8px 16px;
    background: rgba(0,255,170,0.07);
    border: 1px solid rgba(0,255,170,0.2);
    border-radius: 100px;
    font-family: 'Space Mono', monospace;
    font-size: 11px; color: var(--accent);
    letter-spacing: 1px;
    margin-bottom: 32px;
    opacity: 0; animation: fadeUp 0.8s 0.1s forwards;
  }
  .live-dot {
    width: 6px; height: 6px;
    background: var(--accent); border-radius: 50%;
    box-shadow: 0 0 8px var(--accent);
    animation: pulse 2s infinite;
  }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.3} }

  /* Thin horizontal scan line */
  .scan-line {
    position: fixed; left: 0; right: 0; height: 1px;
    background: linear-gradient(to right, transparent, rgba(0,255,170,0.08), transparent);
    z-index: 50; pointer-events: none;
    animation: scanMove 8s linear infinite;
  }
  @keyframes scanMove {
    0% { top: -2px; opacity: 0; }
    5% { opacity: 1; }
    95% { opacity: 1; }
    100% { top: 100vh; opacity: 0; }
  }

  /* scrollbar */
  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: var(--void); }
  ::-webkit-scrollbar-thumb { background: rgba(0,255,170,0.2); border-radius: 2px; }

  @media (max-width: 900px) {
    nav { padding: 16px 24px; }
    .nav-links { display: none; }
    .section-wrap { padding: 70px 24px; }
    .features-grid { grid-template-columns: 1fr; }
    .controls-layout { grid-template-columns: 1fr; }
    .tech-layout { grid-template-columns: 1fr; }
    .run-layout { grid-template-columns: 1fr; }
    .shapes-grid { grid-template-columns: 1fr 1fr; }
    .stats-bar { gap: 40px; padding: 24px; flex-wrap: wrap; }
    footer { flex-direction: column; gap: 32px; text-align: center; }
    .footer-links { justify-content: center; }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>
<div class="scan-line"></div>
<canvas id="starsCanvas"></canvas>

<!-- NAV -->
<nav>
  <div class="nav-logo">Gargantua</div>
  <ul class="nav-links">
    <li><a href="#features">Features</a></li>
    <li><a href="#controls">Controls</a></li>
    <li><a href="#shapes">Shapes</a></li>
    <li><a href="#tech">Tech</a></li>
    <li><a href="#run">Get Started</a></li>
    <li><a href="https://gargantua3d.vercel.app/" target="_blank" class="nav-cta">Live Demo →</a></li>
  </ul>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="orbital-ring ring1"></div>
  <div class="orbital-ring ring2"></div>
  <div class="orbital-ring ring3"></div>
  <div class="hero-glow"></div>

  <div class="live-badge">
    <span class="live-dot"></span>
    Live at gargantua3d.vercel.app
  </div>
  <p class="hero-eyebrow"><span></span>Interactive 3D Particle Physics Engine<span></span></p>
  <h1 class="hero-title">
    <span class="line1">GARGAN</span>
    <span class="line2">TUA.JS</span>
  </h1>
  <p class="hero-sub">Gravity-bending particle physics in your browser. 15,000 particles. Dual-hand tracking. Custom WebGL shaders. No backend. Inspired by Nolan's Interstellar.</p>
  <div class="hero-actions">
    <a href="https://gargantua3d.vercel.app/" target="_blank" class="btn-primary">Launch Experience</a>
    <a href="https://github.com/Uditpandya07/Gargantua" target="_blank" class="btn-outline">View Source</a>
  </div>

  <div class="scroll-hint">
    <span>Scroll to explore</span>
    <div class="scroll-line"></div>
  </div>
</section>

<!-- STATS BAR -->
<div class="stats-bar" id="statsBar">
  <div class="stat"><span class="stat-num" data-target="15000">0</span><span class="stat-label">Particles</span></div>
  <div class="stat"><span class="stat-num">60</span><span class="stat-label">FPS Target</span></div>
  <div class="stat"><span class="stat-num">4</span><span class="stat-label">Morph Shapes</span></div>
  <div class="stat"><span class="stat-num">2</span><span class="stat-label">Hands Tracked</span></div>
  <div class="stat"><span class="stat-num">0</span><span class="stat-label">Backend / Server</span></div>
</div>

<div class="cosmic-divider"></div>

<!-- FEATURES -->
<section id="features">
  <div class="section-wrap">
    <p class="section-tag reveal">Core Architecture</p>
    <h2 class="section-title reveal reveal-d1">Built for <em>Performance</em><br>& Precision</h2>
    <p class="section-desc reveal reveal-d2">Every technical decision was made to maximize visual fidelity and responsiveness at 60fps with thousands of particles in motion.</p>

    <div class="features-grid reveal reveal-d3">
      <div class="feature-card">
        <span class="feature-number">01</span>
        <span class="feature-icon">⚡</span>
        <h3 class="feature-title">Custom GLSL Shaders</h3>
        <p class="feature-desc">Standard PointsMaterial was dropped entirely. Custom Vertex and Fragment shaders render anti-aliased glowing spheres with a fraction of the GPU overhead.</p>
        <span class="feature-tag">WebGL</span>
      </div>
      <div class="feature-card">
        <span class="feature-number">02</span>
        <span class="feature-icon">✋</span>
        <h3 class="feature-title">Scale-Invariant Gestures</h3>
        <p class="feature-desc">A custom math model normalizes for camera distance, so pinch, fist, and swipe gestures trigger accurately whether you're 1 foot or 5 feet from your webcam.</p>
        <span class="feature-tag">MediaPipe</span>
      </div>
      <div class="feature-card">
        <span class="feature-number">03</span>
        <span class="feature-icon">🌀</span>
        <h3 class="feature-title">Particle Physics Loop</h3>
        <p class="feature-desc">Each particle lerps toward its shape target + gravity center per frame. Velocity dampens at 0.82×. Fist release injects impulse into all 15k particles simultaneously.</p>
        <span class="feature-tag">Real-time</span>
      </div>
      <div class="feature-card">
        <span class="feature-number">04</span>
        <span class="feature-icon">💫</span>
        <h3 class="feature-title">UnrealBloom Post-Process</h3>
        <p class="feature-desc">Three.js EffectComposer with UnrealBloomPass at strength 1.6, radius 0.8, threshold 0.2 delivers the signature cinematic neon galaxy glow.</p>
        <span class="feature-tag">Three.js</span>
      </div>
      <div class="feature-card">
        <span class="feature-number">05</span>
        <span class="feature-icon">🔒</span>
        <h3 class="feature-title">Zero Server, Full Privacy</h3>
        <p class="feature-desc">MediaPipe runs the ML hand-skeleton model entirely client-side. No video frames, no biometric data, nothing is ever transmitted to any server.</p>
        <span class="feature-tag">Client-only</span>
      </div>
      <div class="feature-card">
        <span class="feature-number">06</span>
        <span class="feature-icon">🖱️</span>
        <h3 class="feature-title">Graceful Mouse Fallback</h3>
        <p class="feature-desc">After a 4-second timeout, if camera access fails or is denied, the engine seamlessly switches to a full-featured mouse-controlled physics mode.</p>
        <span class="feature-tag">Resilient</span>
      </div>
    </div>
  </div>
</section>

<div class="cosmic-divider"></div>

<!-- CONTROLS -->
<section id="controls">
  <div class="section-wrap">
    <div class="controls-layout">
      <div>
        <p class="section-tag reveal">Interaction System</p>
        <h2 class="section-title reveal reveal-d1">Gestures That <em>Matter</em></h2>
        <p class="section-desc reveal reveal-d2">Two-handed real-time tracking enables a tactile space you can physically touch, scale, twist, and collapse.</p>
        <div class="gestures-list reveal reveal-d3">
          <div class="gesture-item">
            <span class="gesture-emoji">🤏</span>
            <div class="gesture-info">
              <div class="gesture-name">Pinch & Drag</div>
              <div class="gesture-desc">Grab and freely drag the gravity center across the screen</div>
            </div>
            <span class="gesture-mode mode-single">Single</span>
          </div>
          <div class="gesture-item">
            <span class="gesture-emoji">✊</span>
            <div class="gesture-info">
              <div class="gesture-name">Fist — Gravity Well</div>
              <div class="gesture-desc">Condense the galaxy to a singularity; release for supernova</div>
            </div>
            <span class="gesture-mode mode-single">Single</span>
          </div>
          <div class="gesture-item">
            <span class="gesture-emoji">👋</span>
            <div class="gesture-info">
              <div class="gesture-name">Swipe Left / Right</div>
              <div class="gesture-desc">Quick horizontal swipe morphs particles to next shape</div>
            </div>
            <span class="gesture-mode mode-single">Single</span>
          </div>
          <div class="gesture-item">
            <span class="gesture-emoji">↔️</span>
            <div class="gesture-info">
              <div class="gesture-name">Pull / Push to Scale</div>
              <div class="gesture-desc">Spread hands apart to expand; push together to shrink</div>
            </div>
            <span class="gesture-mode mode-dual">Dual</span>
          </div>
          <div class="gesture-item">
            <span class="gesture-emoji">🏎️</span>
            <div class="gesture-info">
              <div class="gesture-name">Twist — Z Rotation</div>
              <div class="gesture-desc">Rotate both hands like a steering wheel to spin the galaxy</div>
            </div>
            <span class="gesture-mode mode-dual">Dual</span>
          </div>
          <div class="gesture-item">
            <span class="gesture-emoji">🤲</span>
            <div class="gesture-info">
              <div class="gesture-name">Center of Gravity</div>
              <div class="gesture-desc">Midpoint between both hands becomes the attractor origin</div>
            </div>
            <span class="gesture-mode mode-dual">Dual</span>
          </div>
        </div>
      </div>
      <div>
        <div class="controls-visual reveal reveal-d2">
          <canvas id="gestureCanvas"></canvas>
          <span class="controls-visual-label">LIVE SIMULATION</span>
        </div>
      </div>
    </div>
  </div>
</section>

<div class="cosmic-divider"></div>

<!-- SHAPES -->
<section id="shapes" class="shapes-section">
  <div class="section-wrap">
    <p class="section-tag reveal">Morph Targets</p>
    <h2 class="section-title reveal reveal-d1">Four Mathematical <em>Universes</em></h2>
    <p class="section-desc reveal reveal-d2">Each morph shape is defined by precise parametric equations that distribute 15,000 particles across 3D space.</p>
    <div class="shapes-grid reveal reveal-d3">
      <div class="shape-card">
        <div class="shape-canvas-wrap"><canvas class="shape-preview" data-shape="nebula" width="120" height="120"></canvas></div>
        <span class="shape-index">01</span>
        <div class="shape-title">Nebula</div>
        <div class="shape-desc">Random spherical distribution with radial density gradient. The default home state.</div>
        <div class="shape-formula">r·sin(φ)·cos(θ)</div>
      </div>
      <div class="shape-card">
        <div class="shape-canvas-wrap"><canvas class="shape-preview" data-shape="dna" width="120" height="120"></canvas></div>
        <span class="shape-index">02</span>
        <div class="shape-title">DNA Helix</div>
        <div class="shape-desc">Double helix with parametric strand offset and base-pair cross-links.</div>
        <div class="shape-formula">cos(t) + cos(t+π)</div>
      </div>
      <div class="shape-card">
        <div class="shape-canvas-wrap"><canvas class="shape-preview" data-shape="galactic" width="120" height="120"></canvas></div>
        <span class="shape-index">03</span>
        <div class="shape-title">Galactic Core</div>
        <div class="shape-desc">Logarithmic spiral arms with density falloff from the central singularity.</div>
        <div class="shape-formula">r·e^(b·θ)</div>
      </div>
      <div class="shape-card">
        <div class="shape-canvas-wrap"><canvas class="shape-preview" data-shape="torus" width="120" height="120"></canvas></div>
        <span class="shape-index">04</span>
        <div class="shape-title">Torus Knot</div>
        <div class="shape-desc">A (2,3) torus knot with volumetric particle distribution around the curve.</div>
        <div class="shape-formula">(R+r·cos(nt))cos(t)</div>
      </div>
    </div>
  </div>
</section>

<div class="cosmic-divider"></div>

<!-- TECH STACK -->
<section id="tech">
  <div class="section-wrap">
    <div class="tech-layout">
      <div>
        <p class="section-tag reveal">Under the Hood</p>
        <h2 class="section-title reveal reveal-d1">Zero Build Step.<br><em>Pure Craft.</em></h2>
        <p class="section-desc reveal reveal-d2">A single HTML file with no bundler, no framework, no dependencies to install. Just ES modules loaded from CDN.</p>
        <div class="tech-list reveal reveal-d3">
          <div class="tech-item">
            <span class="tech-dot"></span>
            <span class="tech-name">Three.js</span>
            <span class="tech-role">3D rendering pipeline & post-processing</span>
            <span class="tech-badge">r158</span>
          </div>
          <div class="tech-item">
            <span class="tech-dot" style="background:var(--accent2);box-shadow:0 0 12px var(--accent2)"></span>
            <span class="tech-name">GLSL Shaders</span>
            <span class="tech-role">Custom vertex + fragment shader pair</span>
            <span class="tech-badge">WebGL</span>
          </div>
          <div class="tech-item">
            <span class="tech-dot" style="background:var(--accent3);box-shadow:0 0 12px var(--accent3)"></span>
            <span class="tech-name">MediaPipe</span>
            <span class="tech-role">Client-side skeletal hand tracking ML</span>
            <span class="tech-badge">Hands v1</span>
          </div>
          <div class="tech-item">
            <span class="tech-dot"></span>
            <span class="tech-name">UnrealBloomPass</span>
            <span class="tech-role">Cinematic glow post-processing effect</span>
            <span class="tech-badge">EffectComposer</span>
          </div>
          <div class="tech-item">
            <span class="tech-dot" style="background:var(--accent2);box-shadow:0 0 12px var(--accent2)"></span>
            <span class="tech-name">ES Modules</span>
            <span class="tech-role">Native browser module imports, no bundler</span>
            <span class="tech-badge">importmap</span>
          </div>
        </div>
      </div>
      <div class="code-block reveal reveal-d2">
        <div class="code-header">
          <span class="code-dot cd1"></span>
          <span class="code-dot cd2"></span>
          <span class="code-dot cd3"></span>
          <span class="code-title">shader.glsl — particle renderer</span>
        </div>
        <div class="code-body">
<span class="c-comment">// Vertex Shader</span><br>
<span class="c-keyword">uniform</span> <span class="c-var">float</span> <span class="c-fn">time</span>;<br>
<span class="c-keyword">attribute</span> <span class="c-var">vec3</span> <span class="c-fn">color</span>;<br>
<span class="c-keyword">varying</span> <span class="c-var">vec3</span> <span class="c-fn">vColor</span>;<br>
<br>
<span class="c-keyword">void</span> <span class="c-fn">main</span>() {<br>
&nbsp;&nbsp;<span class="c-fn">vColor</span> = <span class="c-fn">color</span>;<br>
&nbsp;&nbsp;<span class="c-var">vec4</span> mvPos = modelViewMatrix<br>
&nbsp;&nbsp;&nbsp;&nbsp;* <span class="c-var">vec4</span>(position, <span class="c-num">1.0</span>);<br>
&nbsp;&nbsp;gl_PointSize = <span class="c-num">3.0</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;* (<span class="c-num">300.0</span> / -mvPos.z);<br>
&nbsp;&nbsp;gl_Position = projectionMatrix * mvPos;<br>
}<br><br>
<span class="c-comment">// Fragment Shader</span><br>
<span class="c-keyword">void</span> <span class="c-fn">main</span>() {<br>
&nbsp;&nbsp;<span class="c-var">vec2</span> uv = gl_PointCoord - <span class="c-num">0.5</span>;<br>
&nbsp;&nbsp;<span class="c-var">float</span> d = length(uv);<br>
&nbsp;&nbsp;<span class="c-keyword">if</span> (d > <span class="c-num">0.5</span>) discard;<br>
&nbsp;&nbsp;<span class="c-var">float</span> glow = <span class="c-num">1.0</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;- smoothstep(<span class="c-num">0.0</span>, <span class="c-num">0.5</span>, d);<br>
&nbsp;&nbsp;gl_FragColor = <span class="c-var">vec4</span>(<span class="c-fn">vColor</span>,<br>
&nbsp;&nbsp;&nbsp;&nbsp;glow * glow);<br>
}
        </div>
      </div>
    </div>
  </div>
</section>

<div class="cosmic-divider"></div>

<!-- RUN -->
<section id="run">
  <div class="section-wrap">
    <p class="section-tag reveal">Quick Start</p>
    <h2 class="section-title reveal reveal-d1">Run It in <em>60 Seconds</em></h2>
    <p class="section-desc reveal reveal-d2">Requires a local server — browsers block webcam access for <code style="font-family:'Space Mono',monospace;font-size:13px;color:var(--accent)">file://</code> URLs. Use any of the options below.</p>
    <div class="run-layout">
      <div class="steps-list reveal">
        <div class="step">
          <div class="step-num">1</div>
          <div class="step-content">
            <div class="step-title">Clone the Repository</div>
            <div class="step-desc">Pull the single-file project from GitHub to your local machine.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">2</div>
          <div class="step-content">
            <div class="step-title">Spin Up a Local Server</div>
            <div class="step-desc">Use Python's built-in server, Node's http-server, or VS Code Live Server — anything that serves over <code style="font-family:'Space Mono',monospace;color:var(--accent)">localhost</code>.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">3</div>
          <div class="step-content">
            <div class="step-title">Open & Grant Camera</div>
            <div class="step-desc">Navigate to <code style="font-family:'Space Mono',monospace;color:var(--accent)">localhost:8000</code> and allow camera access when prompted. The engine boots in ~4 seconds.</div>
          </div>
        </div>
        <div class="step">
          <div class="step-num">4</div>
          <div class="step-content">
            <div class="step-title">Hold Up Your Hands</div>
            <div class="step-desc">Bring one or two hands into frame and start sculpting the galaxy. Mouse mode activates automatically if no camera is detected.</div>
          </div>
        </div>
      </div>
      <div class="terminal reveal reveal-d2">
        <div class="terminal-header">
          <span class="t-dot td1"></span>
          <span class="t-dot td2"></span>
          <span class="t-dot td3"></span>
          <span class="terminal-label">bash — ~/projects</span>
        </div>
        <div class="terminal-body">
          <div class="terminal-line"><span class="t-prompt">$</span><span class="t-cmd">git clone https://github.com/</span></div>
          <div class="terminal-line"><span class="t-cmd">&nbsp;&nbsp;Uditpandya07/Gargantua.git</span></div>
          <div class="terminal-line" style="margin-top:4px"><span class="t-out">Cloning into 'Gargantua'...</span></div>
          <div class="terminal-line"><span class="t-out">done.</span></div>
          <div class="terminal-line" style="margin-top:12px"><span class="t-prompt">$</span><span class="t-cmd">cd Gargantua</span></div>
          <div class="terminal-line" style="margin-top:12px"><span class="t-comment"># Option A — Python</span></div>
          <div class="terminal-line"><span class="t-prompt">$</span><span class="t-cmd">python3 -m http.server 8000</span></div>
          <div class="terminal-line" style="margin-top:12px"><span class="t-comment"># Option B — Node</span></div>
          <div class="terminal-line"><span class="t-prompt">$</span><span class="t-cmd">npx http-server -p 8000</span></div>
          <div class="terminal-line" style="margin-top:12px"><span class="t-comment"># Then open in browser</span></div>
          <div class="terminal-line"><span class="t-prompt">$</span><span class="t-cmd">open http://localhost:8000</span></div>
          <div class="terminal-line" style="margin-top:8px"><span class="t-out" style="color:var(--accent)">✓ Gargantua initializing...</span></div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer>
  <div class="footer-logo">GARGANTUA</div>
  <ul class="footer-links">
    <li><a href="https://github.com/Uditpandya07/Gargantua" target="_blank">GitHub</a></li>
    <li><a href="https://gargantua3d.vercel.app/" target="_blank">Live Demo</a></li>
  </ul>
  <p class="footer-credit">Built by <a href="https://github.com/Uditpandya07" target="_blank">@Uditpandya07</a> — Inspired by Interstellar</p>
</footer>

<script>
  // ── CURSOR ──
  const cursor = document.getElementById('cursor');
  const ring = document.getElementById('cursorRing');
  let mx = 0, my = 0, rx = 0, ry = 0;
  document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; cursor.style.left = mx+'px'; cursor.style.top = my+'px'; });
  function animRing() { rx += (mx-rx)*0.12; ry += (my-ry)*0.12; ring.style.left=rx+'px'; ring.style.top=ry+'px'; requestAnimationFrame(animRing); }
  animRing();
  document.querySelectorAll('a,button').forEach(el => {
    el.addEventListener('mouseenter', () => { cursor.style.width='16px'; cursor.style.height='16px'; ring.style.width='48px'; ring.style.height='48px'; });
    el.addEventListener('mouseleave', () => { cursor.style.width='10px'; cursor.style.height='10px'; ring.style.width='36px'; ring.style.height='36px'; });
  });

  // ── STARS ──
  const starsCanvas = document.getElementById('starsCanvas');
  const sc = starsCanvas.getContext('2d');
  starsCanvas.width = window.innerWidth; starsCanvas.height = window.innerHeight;
  const stars = Array.from({length: 300}, () => ({
    x: Math.random()*window.innerWidth, y: Math.random()*window.innerHeight,
    r: Math.random()*1.2+0.2, a: Math.random(), speed: Math.random()*0.003+0.001
  }));
  function drawStars() {
    sc.clearRect(0,0,starsCanvas.width,starsCanvas.height);
    stars.forEach(s => {
      s.a += s.speed; if(s.a > 1) s.a = 0;
      const alpha = Math.sin(s.a * Math.PI) * 0.8 + 0.1;
      sc.beginPath(); sc.arc(s.x, s.y, s.r, 0, Math.PI*2);
      sc.fillStyle = `rgba(200,216,255,${alpha})`; sc.fill();
    });
    requestAnimationFrame(drawStars);
  }
  drawStars();
  window.addEventListener('resize', () => { starsCanvas.width = window.innerWidth; starsCanvas.height = window.innerHeight; });

  // ── SCROLL REVEALS ──
  const reveals = document.querySelectorAll('.reveal, .stat');
  const io = new IntersectionObserver(entries => {
    entries.forEach(e => { if(e.isIntersecting) e.target.classList.add('visible'); });
  }, {threshold: 0.1});
  reveals.forEach(el => io.observe(el));

  // ── STAT COUNTER ──
  document.querySelectorAll('[data-target]').forEach(el => {
    const target = +el.dataset.target;
    const ioStat = new IntersectionObserver(entries => {
      if(entries[0].isIntersecting) {
        let cur = 0; const step = target/60;
        const t = setInterval(() => { cur = Math.min(cur+step, target); el.textContent = Math.floor(cur).toLocaleString(); if(cur>=target) clearInterval(t); }, 16);
        ioStat.disconnect();
      }
    });
    ioStat.observe(el);
  });

  // ── GESTURE DEMO CANVAS ──
  const gc = document.getElementById('gestureCanvas');
  if(gc) {
    gc.width = gc.offsetWidth || 400; gc.height = gc.offsetHeight || 300;
    const gx = gc.getContext('2d');
    const pts = Array.from({length:300}, () => ({
      x: Math.random()*gc.width, y: Math.random()*gc.height,
      vx: (Math.random()-.5)*0.4, vy: (Math.random()-.5)*0.4,
      hue: Math.random()*0.4
    }));
    let gtime = 0;
    function drawGesture() {
      gx.clearRect(0,0,gc.width,gc.height);
      gtime += 0.015;
      const cx = gc.width/2 + Math.sin(gtime*0.7)*60;
      const cy = gc.height/2 + Math.cos(gtime*0.5)*40;
      pts.forEach((p,i) => {
        const tx = cx + Math.cos(gtime + i*0.02)*20*(i%5+1);
        const ty = cy + Math.sin(gtime*1.1 + i*0.03)*20*(i%4+1);
        p.x += (tx - p.x) * 0.04 + p.vx;
        p.y += (ty - p.y) * 0.04 + p.vy;
        p.vx *= 0.95; p.vy *= 0.95;
        const h = (p.hue + gtime*0.04) % 1;
        const r = Math.floor(h<0.5 ? (1-2*h)*255 : 0);
        const g2 = Math.floor(h<0.5 ? 255 : (2-(2*h))*255);
        const b2 = Math.floor(h>0.5 ? 255 : (2*h)*255);
        gx.beginPath(); gx.arc(p.x,p.y,1.5,0,Math.PI*2);
        gx.fillStyle = `rgba(0,255,170,${0.4+Math.sin(gtime+i)*0.3})`;
        gx.fill();
      });
      requestAnimationFrame(drawGesture);
    }
    drawGesture();
    new ResizeObserver(() => { gc.width=gc.offsetWidth; gc.height=gc.offsetHeight; }).observe(gc);
  }

  // ── SHAPE PREVIEWS ──
  const shapePreviews = document.querySelectorAll('.shape-preview');
  shapePreviews.forEach(canvas => {
    const ctx = canvas.getContext('2d');
    const shape = canvas.dataset.shape;
    const N = 600;
    let t2 = 0;
    function drawShape() {
      ctx.clearRect(0,0,canvas.width,canvas.height);
      const cx = canvas.width/2, cy = canvas.height/2, R = 44;
      t2 += 0.012;
      for(let i=0; i<N; i++) {
        let x=0,y=0;
        const u = (i/N)*Math.PI*2;
        if(shape==='nebula') {
          const r = R*(0.3+Math.random()*0.7)*Math.pow(Math.random(),0.5);
          const a = Math.random()*Math.PI*2;
          x = Math.cos(a+t2*0.1)*r; y = Math.sin(a+t2*0.08)*r*0.5;
        } else if(shape==='dna') {
          const t = (i/N)*8*Math.PI;
          const strand = i%2===0?1:-1;
          x = (Math.cos(t+t2)*R*0.5 + strand*8) * (Math.abs(Math.sin(t))*0.3+0.7);
          y = (t/(8*Math.PI)-0.5)*R*1.8;
        } else if(shape==='galactic') {
          const arm = (i%3)*(Math.PI*2/3);
          const r2 = (i/N)*R;
          const spiral = arm + (i/N)*4;
          x = Math.cos(spiral+t2*0.2)*r2; y = Math.sin(spiral+t2*0.2)*r2*0.4;
        } else if(shape==='torus') {
          const phi = u; const theta = (i/N)*Math.PI*6;
          const r3 = 0.5; const tube = 0.25;
          x = Math.cos(phi+t2*0.3)*(R*r3 + R*tube*Math.cos(theta));
          y = Math.sin(phi*2+t2*0.3)*R*tube*Math.sin(theta)*2 + Math.sin(phi+t2*0.3)*(R*r3*0.4);
        }
        const hue = (i/N*240 + t2*20) % 360;
        ctx.beginPath(); ctx.arc(cx+x, cy+y, 1, 0, Math.PI*2);
        ctx.fillStyle = `hsla(${hue},100%,65%,0.7)`; ctx.fill();
      }
      requestAnimationFrame(drawShape);
    }
    drawShape();
  });
</script>
</body>
</html>