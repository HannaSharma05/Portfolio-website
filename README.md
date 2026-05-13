# Portfolio-website
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Portfolio</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;600;700;900&family=Syne:wght@300;400;500;700&family=JetBrains+Mono:wght@300;400;500&display=swap" rel="stylesheet"/>
<style>
  :root {
    --void: #03040a;
    --deep: #14162b;
    --surface: #0c0e24;
    --panel: rgba(17, 22, 67, 0.75);
    --border: rgba(100,130,255,0.15);
    --cyan: #89dfdf;
    --blue: #adb9e4;
    --violet: #bba4eb;
    --gold: #e4c484;
    --star: #dae5f7;
    --text: #e0e4ee;
    --muted: #6070a0;
    --glow-c: rgba(74,240,240,0.18);
    --glow-v: rgba(168,126,255,0.18);
  }
 
  *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
 
  html { scroll-behavior: smooth; font-size: 16px; }
 
  body {
    background: var(--void);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    font-weight: 400;
    overflow-x: hidden;
    cursor: none;
  }
 
  /* ── CUSTOM CURSOR ── */
  #cursor {
    position: fixed; top:0; left:0; width:10px; height:10px;
    background: var(--cyan); border-radius:50%; pointer-events:none;
    z-index:9999; transform: translate(-50%,-50%);
    transition: transform 0.08s, background 0.2s;
    mix-blend-mode: screen;
  }
  
 
  /* ── STAR CANVAS ── */
  #starfield { position: fixed; inset:0; z-index:0; pointer-events:none; }
 
  /* ── NEBULA BLOBS ── */
  .nebula {
    position: fixed; border-radius:50%; filter: blur(90px);
    opacity: 0.12; pointer-events:none; z-index:0;
  }
  .nebula-1 { width:600px; height:600px; background: radial-gradient(circle, #5b7fff 0%, transparent 70%); top:-100px; left:-150px; animation: drift1 30s ease-in-out infinite alternate; }
  .nebula-2 { width:500px; height:500px; background: radial-gradient(circle, #a87eff 0%, transparent 70%); bottom:10%; right:-100px; animation: drift2 38s ease-in-out infinite alternate; }
  .nebula-3 { width:400px; height:400px; background: radial-gradient(circle, #4af0f0 0%, transparent 70%); top:40%; left:30%; animation: drift3 26s ease-in-out infinite alternate; opacity:0.07; }
 
  @keyframes drift1 { from{transform:translate(0,0) scale(1)} to{transform:translate(60px,80px) scale(1.15)} }
  @keyframes drift2 { from{transform:translate(0,0) scale(1)} to{transform:translate(-80px,-50px) scale(1.1)} }
  @keyframes drift3 { from{transform:translate(0,0) scale(1)} to{transform:translate(40px,-60px) scale(1.08)} }
 
  /* ── LAYOUT ── */
  .wrapper { position: relative; z-index:1; }
 
  /* ── NAV ── */
  nav {
    position: fixed; top:0; left:0; right:0; z-index:100;
    display:flex; align-items:center; justify-content:space-between;
    padding: 0 5vw; height:64px;
    background: rgba(3,4,10,0.7);
    backdrop-filter: blur(16px);
    border-bottom: 1px solid var(--border);
  }
  .nav-logo {
    font-family:'Orbitron',sans-serif; font-weight:700; font-size:1rem;
    letter-spacing:0.15em; color:var(--cyan); text-decoration:none;
  }
  .nav-logo span { color:var(--violet); }
  .nav-links { display:flex; gap:2rem; list-style:none; }
  .nav-links a {
    font-family:'JetBrains Mono',monospace; font-size:0.72rem; letter-spacing:0.1em;
    color:var(--muted); text-decoration:none; text-transform:uppercase;
    transition: color 0.2s;
  }
  .nav-links a:hover { color:var(--cyan); }
 
  /* ── HERO ── */
  #hero {
    min-height:100vh; display:flex; flex-direction:column;
    justify-content:center; align-items:flex-start;
    padding: 0 8vw; padding-top:64px; position:relative; overflow:hidden;
  }
  .hero-tag {
    font-family:'JetBrains Mono',monospace; font-size:0.75rem;
    color:var(--cyan); letter-spacing:0.2em; text-transform:uppercase;
    margin-bottom:1.4rem; opacity:0; animation: fadeUp 0.8s 0.3s forwards;
  }
  .hero-tag::before { content:'> '; color: var(--violet); }
  .hero-name {
    font-family:'Orbitron',sans-serif; font-weight:900;
    font-size: clamp(3rem, 8vw, 7rem); line-height:1;
    letter-spacing:-0.02em; color:#fff;
    opacity:0; animation: fadeUp 0.9s 0.5s forwards;
  }
  .hero-name .glow {
    color: transparent;
    background: linear-gradient(135deg, var(--cyan) 0%, var(--violet) 100%);
    -webkit-background-clip: text; background-clip:text;
  }
  .hero-sub {
    margin-top:1.5rem; font-size:1.4rem; font-weight:300; color:rgb(214, 208, 208);
    max-width:900px; line-height:1.7;
    opacity:0; animation: fadeUp 0.9s 0.7s forwards;
  }
  .hero-links {
    margin-top:2.5rem; display:flex; gap:1rem; flex-wrap:wrap;
    opacity:0; animation: fadeUp 0.9s 0.9s forwards;
  }
  .btn {
    display:inline-flex; align-items:center; gap:0.5rem;
    padding:0.65rem 1.5rem; border-radius:4px;
    font-family:'JetBrains Mono',monospace; font-size:0.78rem;
    letter-spacing:0.1em; text-transform:uppercase;
    text-decoration:none; transition: all 0.25s; cursor:none;
  }
  .btn-primary {
    background: linear-gradient(135deg, var(--cyan), var(--blue));
    color: var(--void); font-weight:500;
  }
  .btn-primary:hover { transform:translateY(-2px); box-shadow:0 6px 30px rgba(74,240,240,0.35); }
  .btn-ghost {
    border:1px solid var(--border); color:var(--text);
    background: transparent;
  }
  .btn-ghost:hover { border-color: var(--cyan); color:var(--cyan); transform:translateY(-2px); }
 
  .scroll-indicator {
    position:absolute; bottom:2.5rem; left:50%; transform:translateX(-50%);
    display:flex; flex-direction:column; align-items:center; gap:0.4rem;
    opacity:0; animation: fadeUp 1s 1.2s forwards;
  }
  .scroll-indicator span {
    font-family:'JetBrains Mono',monospace; font-size:0.62rem;
    color:var(--muted); letter-spacing:0.15em; text-transform:uppercase;
  }
  .scroll-line {
    width:1px; height:50px;
    background: linear-gradient(to bottom, var(--cyan), transparent);
    animation: scrollPulse 2s ease-in-out infinite;
  }
  @keyframes scrollPulse { 0%,100%{opacity:0.3} 50%{opacity:1} }
 
  /* ── SECTION SHARED ── */
  section { padding:6rem 8vw; position:relative; }
 
  .section-label {
    font-family:'JetBrains Mono',monospace; font-size:0.7rem;
    color:var(--cyan); letter-spacing:0.25em; text-transform:uppercase;
    margin-bottom:0.8rem; opacity:0.8;
  }
  .section-label::before { content:'// '; color:var(--violet); }
  .section-title {
    font-family:'Orbitron',sans-serif; font-weight:700;
    font-size: clamp(1.7rem, 4vw, 2.8rem); color:#fff;
    margin-bottom:3rem; line-height:1.2;
  }
 
  .divider {
    width:100%; height:1px;
    background: linear-gradient(90deg, transparent, var(--border), transparent);
    margin: 0;
  }
 
  /* ── SKILLS ── */
  .skills-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(280px,1fr)); gap:1.5rem; }
  .skill-card {
    background: var(--panel); border:1px solid var(--border);
    border-radius:9px; padding:1.8rem;
    backdrop-filter: blur(10px);
    transition: border-color 0.3s, transform 0.3s, box-shadow 0.3s;
    position:relative; overflow:hidden;
    
  }
  .skill-card::before {
    content:''; position:absolute; inset:0;
    background: linear-gradient(135deg, var(--glow-c), transparent);
    opacity:0; transition:opacity 0.3s;
  }
  .skill-card:hover { border-color:rgba(74,240,240,0.4); transform:translateY(-4px); box-shadow:0 12px 40px rgba(74,240,240,0.1); }
  .skill-card:hover::before { opacity:1; }
  .skill-card-title {
    font-family:'Orbitron',sans-serif; font-size:0.72rem; font-weight:600;
    letter-spacing:0.18em; text-transform:uppercase; color:var(--cyan);
    margin-bottom:1.2rem;
  }
  .skill-tags { display:flex; flex-wrap:wrap; gap:0.5rem; }
  .tag {
    padding:0.3rem 0.75rem; border-radius:3px;
    font-family:'JetBrains Mono',monospace; font-size:0.72rem;
    border:1px solid var(--border); color:var(--text); background:rgba(255,255,255,0.03);
    transition:all 0.2s;
  }
  .tag:hover { border-color:var(--cyan); color:var(--cyan); background:rgba(74,240,240,0.07); }
  .tag.violet { border-color:rgba(168,126,255,0.25); color:var(--violet); }
  .tag.gold { border-color:rgba(240,192,96,0.25); color:var(--gold); }
 
  /* ── PROJECTS ── */
  .projects-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(340px,1fr)); gap:2rem; }
  .project-card {
    background: var(--panel); border:1px solid var(--border);
    border-radius:10px; padding:2rem;
    backdrop-filter:blur(10px);
    transition:all 0.3s; position:relative; overflow:hidden;
    display:flex; flex-direction:column;
  }
  .project-card::after {
    content:''; position:absolute; top:0; left:0; right:0; height:2px;
    background: linear-gradient(90deg, var(--cyan), var(--violet));
    transform: scaleX(0); transform-origin:left; transition:transform 0.4s;
  }
  .project-card:hover { transform:translateY(-6px); border-color:rgba(91,127,255,0.35); box-shadow:0 20px 60px rgba(91,127,255,0.12); }
  .project-card:hover::after { transform:scaleX(1); }
  .project-num {
    font-family:'Orbitron',sans-serif; font-size:0.65rem; letter-spacing:0.2em;
    color:var(--muted); margin-bottom:1rem;
  }
  .project-title {
    font-family:'Orbitron',sans-serif; font-weight:700; font-size:1.05rem;
    color:#fff; margin-bottom:0.5rem; line-height:1.3;
  }
  .project-date {
    font-family:'JetBrains Mono',monospace; font-size:0.68rem;
    color:var(--muted); margin-bottom:1.2rem;
  }
  .project-desc { font-size:0.88rem; line-height:1.7; color:var(--text); flex:1; }
  .project-desc li { margin-bottom:0.5rem; padding-left:0.5rem; }
  .project-desc li::marker { color:var(--cyan); }
  .project-footer { margin-top:1.5rem; display:flex; align-items:center; justify-content:space-between; flex-wrap:wrap; gap:0.75rem; }
  .tech-stack { display:flex; flex-wrap:wrap; gap:0.4rem; }
  .tech-tag {
    padding:0.2rem 0.6rem; border-radius:3px;
    font-family:'JetBrains Mono',monospace; font-size:0.65rem;
    background:rgba(91,127,255,0.12); border:1px solid rgba(91,127,255,0.25); color:var(--blue);
  }
  .project-link {
    font-family:'JetBrains Mono',monospace; font-size:0.7rem;
    color:var(--cyan); text-decoration:none; letter-spacing:0.1em;
    transition:color 0.2s;
  }
  .project-link:hover { color:#fff; }
 
  /* ── TIMELINE (training/education) ── */
  .timeline { position:relative; padding-left:2rem; }
  .timeline::before {
    content:''; position:absolute; left:0; top:0; bottom:0; width:1px;
    background: linear-gradient(to bottom, var(--cyan), var(--violet), transparent);
  }
  .tl-item {
    position:relative; margin-bottom:2.5rem; padding-bottom:2rem;
    border-bottom:1px solid var(--border);
    opacity:0; transform:translateX(-20px);
    transition:all 0.6s;
  }
  .tl-item.visible { opacity:1; transform:translateX(0); }
  .tl-item:last-child { border-bottom:none; margin-bottom:0; }
  .tl-dot {
    position:absolute; left:-2.4rem; top:0.3rem;
    width:10px; height:10px; border-radius:50%;
    background:var(--cyan); box-shadow:0 0 12px var(--cyan);
  }
  .tl-date {
    font-family:'JetBrains Mono',monospace; font-size:0.7rem;
    color:var(--violet); letter-spacing:0.1em; margin-bottom:0.4rem;
  }
  .tl-title { font-family:'Orbitron',sans-serif; font-weight:600; font-size:0.95rem; color:#fff; margin-bottom:0.3rem; line-height:1.4; }
  .tl-org { font-size:0.8rem; color:var(--cyan); margin-bottom:0.8rem; font-style:italic; }
  .tl-body { font-size:0.85rem; line-height:1.7; color:var(--muted); }
  .tl-body li { margin-bottom:0.4rem; padding-left:0.4rem; }
  .tl-body li::marker { color:var(--violet); }
 
  /* ── CERTS ── */
  .certs-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(240px,1fr)); gap:1.2rem; }
  .cert-card {
    background:var(--panel); border:1px solid var(--border);
    border-radius:8px; padding:1.4rem 1.6rem;
    display:flex; flex-direction:column; gap:0.5rem;
    transition:all 0.3s; position:relative; overflow:hidden;
    cursor:default;
  }
  .cert-card::before {
    content:''; position:absolute; left:0; top:0; bottom:0; width:3px;
    background: linear-gradient(to bottom, var(--gold), var(--violet));
  }
  .cert-card:hover { transform:translateY(-3px); border-color:rgba(240,192,96,0.3); box-shadow:0 10px 30px rgba(240,192,96,0.08); }
  .cert-name { font-weight:600; font-size:0.88rem; color:#fff; line-height:1.4; }
  .cert-issuer { font-family:'JetBrains Mono',monospace; font-size:0.68rem; color:var(--gold); }
  .cert-date { font-family:'JetBrains Mono',monospace; font-size:0.65rem; color:var(--muted); }
 
  /* ── EXTRA CURRICULAR ── */
  .ec-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(280px,1fr)); gap:1.2rem; }
  .ec-card {
    background:var(--panel); border:1px solid var(--border);
    border-radius:8px; padding:1.4rem 1.6rem;
    transition:all 0.3s; position:relative; overflow:hidden;
  }
  .ec-card:hover { transform:translateY(-3px); border-color:rgba(168,126,255,0.4); box-shadow:0 10px 30px rgba(168,126,255,0.1); }
  .ec-badge {
    display:inline-block; padding:0.2rem 0.7rem; border-radius:20px;
    font-family:'JetBrains Mono',monospace; font-size:0.62rem; letter-spacing:0.08em;
    margin-bottom:0.6rem;
  }
  .badge-gold { background:rgba(240,192,96,0.12); border:1px solid rgba(240,192,96,0.35); color:var(--gold); }
  .badge-silver { background:rgba(200,210,255,0.08); border:1px solid rgba(200,210,255,0.25); color:#c8d8f8; }
  .ec-title { font-size:0.88rem; color:#fff; font-weight:500; line-height:1.4; margin-bottom:0.4rem; }
  .ec-date { font-family:'JetBrains Mono',monospace; font-size:0.65rem; color:var(--muted); }
 
  /* ── EDUCATION ── */
  .edu-grid { display:grid; grid-template-columns:repeat(auto-fit,minmax(300px,1fr)); gap:1.5rem; }
  .edu-card {
    background:var(--panel); border:1px solid var(--border);
    border-radius:10px; padding:2rem;
    transition:all 0.3s; position:relative; overflow:hidden;
  }
  .edu-card:hover { border-color:rgba(74,240,240,0.3); transform:translateY(-4px); }
  .edu-degree { font-family:'Orbitron',sans-serif; font-weight:700; font-size:0.95rem; color:#fff; margin-bottom:0.4rem; }
  .edu-school { font-size:0.88rem; color:var(--cyan); margin-bottom:0.3rem; }
  .edu-location { font-size:0.78rem; color:var(--muted); margin-bottom:0.8rem; }
  .edu-score { display:inline-flex; align-items:center; gap:0.4rem; }
  .edu-score-val {
    font-family:'Orbitron',sans-serif; font-size:1.5rem; font-weight:700;
    background: linear-gradient(135deg, var(--cyan), var(--gold));
    -webkit-background-clip:text; background-clip:text; color:transparent;
  }
  .edu-score-label { font-family:'JetBrains Mono',monospace; font-size:0.7rem; color:var(--muted); }
  .edu-period { font-family:'JetBrains Mono',monospace; font-size:0.68rem; color:var(--violet); margin-top:0.5rem; }
 
  /* ── CONTACT STRIP ── */
  #contact {
    padding:5rem 8vw;
    background: linear-gradient(135deg, rgba(91,127,255,0.06), rgba(168,126,255,0.06));
    border-top:1px solid var(--border);
    display:flex; flex-direction:column; align-items:center; text-align:center;
  }
  .contact-title {
    font-family:'Orbitron',sans-serif; font-weight:700;
    font-size:clamp(1.4rem,3vw,2.2rem); color:#fff; margin-bottom:0.6rem;
  }
  .contact-sub { font-size:0.95rem; color:var(--muted); margin-bottom:2.5rem; }
  .contact-links { display:flex; flex-wrap:wrap; gap:1.2rem; justify-content:center; }
  .contact-item {
    display:flex; align-items:center; gap:0.6rem;
    font-family:'JetBrains Mono',monospace; font-size:0.8rem; color:var(--text);
    text-decoration:none; padding:0.65rem 1.2rem;
    border:1px solid var(--border); border-radius:6px;
    transition:all 0.25s; backdrop-filter:blur(8px);
  }
  .contact-item:hover { border-color:var(--cyan); color:var(--cyan); transform:translateY(-2px); box-shadow:0 6px 20px rgba(74,240,240,0.12); }
  .contact-icon { font-size:1rem; }
 
  /* ── FOOTER ── */
  footer {
    padding:1.5rem 8vw; text-align:center;
    font-family:'JetBrains Mono',monospace; font-size:0.68rem;
    color:var(--muted); border-top:1px solid var(--border);
    letter-spacing:0.1em;
  }
  footer span { color:var(--cyan); }
 
  /* ── ANIMATIONS ── */
  @keyframes fadeUp {
    from { opacity:0; transform:translateY(24px); }
    to   { opacity:1; transform:translateY(0); }
  }
 
  .reveal {
    opacity:0; transform:translateY(30px);
    transition: opacity 0.7s, transform 0.7s;
  }
  .reveal.visible { opacity:1; transform:translateY(0); }
 
  /* ── MOBILE ── */
  @media(max-width:700px){
    .nav-links { display:none; }
    .hero-name { font-size:2.8rem; }
    section { padding:4rem 6vw; }
  }
 
  /* ── SHOOTING STAR ── */
  .shooting-star {
    position:fixed; width:120px; height:1px;
    background: linear-gradient(90deg, transparent, var(--cyan), transparent);
    border-radius:50%; pointer-events:none; z-index:0;
    animation: shoot linear forwards;
    opacity:0;
  }
  @keyframes shoot {
    0%   { opacity:0; transform:translateX(0) translateY(0); }
    10%  { opacity:1; }
    80%  { opacity:0.6; }
    100% { opacity:0; transform:translateX(300px) translateY(300px); }
  }
 
  /* ── PLANET DECO ── */
  .planet {
    position:absolute; border-radius:50%; pointer-events:none;
  }
  .planet-1 {
    width:180px; height:180px; right:-60px; top:50px;
    background: radial-gradient(circle at 35% 35%, #4a5090, #0d0f2a 70%);
    border:1px solid rgba(91,127,255,0.2);
    box-shadow: -8px 0 40px rgba(91,127,255,0.15) inset, 0 0 60px rgba(91,127,255,0.05);
    animation: planetFloat 12s ease-in-out infinite alternate;
    opacity:0.55;
  }
  .planet-ring {
    position:absolute; width:260px; height:60px;
    border:1.5px solid rgba(91,127,255,0.25);
    border-radius:50%; top:110px; right:-100px;
    transform:rotateX(65deg);
    animation:planetFloat 12s ease-in-out infinite alternate;
    opacity:0.3;
  }
  @keyframes planetFloat { from{transform:translateY(0) rotateX(65deg)} to{transform:translateY(-20px) rotateX(65deg)} }
  .planet-1-anim { animation:planetFloatSimple 12s ease-in-out infinite alternate; }
  @keyframes planetFloatSimple { from{transform:translateY(0)} to{transform:translateY(-20px)} }
</style>
</head>
<body>
 
<!-- Custom cursor -->
<div id="cursor"></div>
<div id="cursor-ring"></div>
 
<!-- Nebula layers -->
<div class="nebula nebula-1"></div>
<div class="nebula nebula-2"></div>
<div class="nebula nebula-3"></div>
 
<!-- Star canvas -->
<canvas id="starfield"></canvas>
 
<!-- NAV -->
<nav>
  
  <ul class="nav-links">
    <li><a href="#skills">Skills</a></li>
    <li><a href="#projects">Projects</a></li>
    <li><a href="#training">Training</a></li>
    <li><a href="#certs">Certs</a></li>
    <li><a href="#education">Education</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
</nav>
 
<div class="wrapper">
 wxz
  <!-- HERO -->
  <section id="hero">
    <div class="planet-1 planet-1-anim"></div>
    <div class="planet-ring"></div>
    
    <h1 class="hero-name">
      <span class="glow">Hanna</span><br>Sharma
    </h1>
    <p class="hero-sub">
      <b>A highly motivated B.Tech Computer Science and Engineering 3rd year student with specialization in Cloud Computing.
        Proficient in C++, Python and it's libraries(NumPy, Pandas) with hands-on experience in modern web technologies and algorithmic problem solving.
         Passionate about cloud infrastructure, DevOps and building scalable tech solutions. </b>
    </p>
    <div class="hero-links">
      <a href="https://www.linkedin.com/in/hanna-sharma/" target="_blank" class="btn btn-primary">LinkedIn ↗</a>
      <a href="https://github.com/HannaSharma05" target="_blank" class="btn btn-primary">GitHub ↗</a>
      <a href="mailto:hanna5@gmail.com" class="btn btn-primary">Email ↗</a>
      <a href="https://drive.google.com/file/d/1sQNibQu9Og5eZRaqxQRTXd7sSy7cXUQv/view?usp=sharing" class="btn btn-primary">Resume ↗</a>
    </div>
    
  </section>
 
  <div class="divider"></div>
 
  <!-- SKILLS -->
  <section id="skills">
    
    <h2 class="section-title">Skills &amp; Tools</h2>
    <div class="skills-grid reveal">
      <div class="skill-card">
        <div class="skill-card-title">Languages</div>
        <div class="skill-tags">
          <span class="tag">C++</span>
          <span class="tag">Python</span>
          <span class="tag">Flask</span>
          <span class="tag">HTML</span>
          <span class="tag">CSS</span>
          <span class="tag">JavaScript</span>
        </div>
      </div>
      <div class="skill-card">
        <div class="skill-card-title">Tools &amp; Platforms</div>
        <div class="skill-tags">
          <span class="tag violet">MySQL</span>
          <span class="tag violet">AWS</span>
          <span class="tag violet">Linux</span>
          <span class="tag violet">GitHub</span>
          <span class="tag violet">NumPy</span>
          <span class="tag violet">Pandas</span>
          <span class="tag violet">Docker</span>
          <span class="tag violet">Kubernetes</span>
          <span class="tag violet">Terraform</span>
          <span class="tag violet">MS Excel</span>
        </div>
      </div>
      <div class="skill-card">
        <div class="skill-card-title">Soft Skills</div>
        <div class="skill-tags">
          <span class="tag gold">Communication</span>
          <span class="tag gold">Problem-Solving</span>
          <span class="tag gold">Agility</span>
          <span class="tag gold">Adaptability</span>
        </div>
      </div>
    </div>
  </section>
 
  <div class="divider"></div>
 
  <!-- PROJECTS -->
  <section id="projects">
    
    <h2 class="section-title">Projects</h2>
    <div class="projects-grid">
      <div class="project-card reveal">
       
        <div class="project-title">AI Virtual Art History Guide</div>
        
        <ul class="project-desc">
          <li>Engineered a full-stack AI art historian chatbot using Flask and Gemini API, implementing multilingual conversational memory with DOM-based data binding.</li>
          <li>Applied sliding-window conversation management to optimize API costs while maintaining contextual coherence.</li>
          <li>Built cross-browser voice recognition with Web Speech API integration.</li>
        </ul>
        <div class="project-footer">
          <div class="tech-stack">
    
            <span class="tech-tag">Flask</span>
            <span class="tech-tag">HTML5</span>
            <span class="tech-tag">CSS3</span>
            <span class="tech-tag">JS</span>
            
          </div>
          <a href="#" class="project-link">View ↗</a>
        </div>
      </div>
      <div class="project-card reveal">
      <div class="project-title">Cloud Based User Orchestrator </div>
        
        <ul class="project-desc">
          <li>Created a web application that fetches random user profiles 
            from a global cloud API, allowing nationality filtering and auto refresh.</li>
          <li>Demonstrates a fluid frontend layout alongwith asynchronous state handling.</li>
            <li> It demonstrates 
              real‑time data integration and responsive UI design without any external dependencies.</li>
          
        </ul>
        <div class="project-footer">
          <div class="tech-stack">
    
            
            <span class="tech-tag">HTML5</span>
            <span class="tech-tag">CSS3</span><span class="tech-tag">Fetch, Random User API</span>
            <span class="tech-tag">vanilla JavaScript</span>
            
          </div>
          <a href="#" class="project-link">View ↗</a>
        </div>
      </div>
      </div>
      <div class="project-card reveal" style="transition-delay:0.15s">
        
        <div class="project-title">Hospital Management System</div>
        
        <ul class="project-desc">
          <li>Developed a C++ OOP-based Hospital Management System utilizing advanced data structures for efficient appointment scheduling and priority-based emergency processing.</li>
          <li>Implemented polymorphism, inheritance, and STL containers to streamline patient care operations with proper error handling.</li>
        </ul>
        <div class="project-footer">
          <div class="tech-stack">
            <span class="tech-tag">C++ (OOPs)</span>
            <span class="tech-tag">STL</span>
          </div>
          <a href="#" class="project-link">View ↗</a>
        </div>
      </div>
      
    </div>
  </section>
 
  <div class="divider"></div>
 
  <!-- TRAINING -->
  <section id="training">
    
    <h2 class="section-title">Training</h2>
    <div class="timeline">
      <div class="tl-item">
        <div class="tl-dot"></div>
        <div class="tl-date">Jun '25 – Jul '25</div>
        <div class="tl-title">C++ Programming: OOPs and DSA</div>
        <div class="tl-org">CSE PATHSHALA</div>
        <ul class="tl-body">
          <li>Gained comprehensive clarity on OOP concepts and Data Structures &amp; Algorithms using C++.</li>
          <li>Implemented efficient data structures like vectors, maps, and priority queues for optimized operations.</li>
          <li>Developed systematic problem-solving approaches to translate real-world challenges into algorithmic solutions.</li>
        </ul>
      </div>
      <div class="tl-item">
        <div class="tl-dot" style="background:var(--violet); box-shadow:0 0 12px var(--violet);"></div>
        <div class="tl-date">Feb '24 – May '24</div>
        <div class="tl-title">Cyber Security Associate (Beginner) Training</div>
        <div class="tl-org">PreGrad</div>
        <ul class="tl-body">
          <li>Completed an intensive mentorship program covering foundational cyber security principles, tools, and practices.</li>
          <li>Built strong foundations in network security, threat detection, and system protection techniques.</li>
        </ul>
      </div>
    </div>
  </section>
 
  <div class="divider"></div>
 
  <!-- CERTIFICATES -->
  <section id="certs">
    
    <h2 class="section-title">Certificates</h2>
    <div class="certs-grid reveal">
      <div class="cert-card">
        <div class="cert-name">AWS Academy Graduate — Cloud Architecting</div>
        <div class="cert-issuer">AWS Academy</div>
        <div class="cert-date">Jan '26</div>
      </div>
      <div class="cert-card">
        <div class="cert-name">Privacy and Security in Online Social Media</div>
        <div class="cert-issuer">NPTEL</div>
        <div class="cert-date">Nov '25</div>
      </div>
      <div class="cert-card">
        <div class="cert-name">EnglishEdge: Sharpen your Grammar and Vocabulary</div>
        <div class="cert-issuer">Lovely Professional University</div>
        <div class="cert-date">Oct '25</div>
      </div>
      <div class="cert-card">
        <div class="cert-name">AWS Certified Cloud Practitioner</div>
        <div class="cert-issuer">GeeksforGeeks</div>
        <div class="cert-date">Jun '25</div>
      </div>
      <div class="cert-card">
        <div class="cert-name">Mastering Generative AI</div>
        <div class="cert-issuer">GeeksforGeeks</div>
        <div class="cert-date">Jun '25</div>
      </div>
      <div class="cert-card">
        <div class="cert-name">Digital Systems</div>
        <div class="cert-issuer">Coursera</div>
        <div class="cert-date">Sep '24</div>
      </div>
      <div class="cert-card">
        <div class="cert-name">Introduction to Hardware and Operating Systems</div>
        <div class="cert-issuer">Coursera</div>
        <div class="cert-date">Sep '24</div>
      </div>
      
      <div class="cert-card">
        <div class="cert-name">Bits and Bytes of Computer Networking</div>
        <div class="cert-issuer">Coursera</div>
        <div class="cert-date">Sep '24</div>
      </div>
    </div>
  </section>
 
  <div class="divider"></div>
 
  <!-- EXTRA-CURRICULAR -->
  <section id="extra">
    
    <h2 class="section-title">Extra-Curricular</h2>
    <div class="ec-grid reveal">
      <div class="ec-card">
        <div class="ec-badge badge-gold">🏆 Winner</div>
        <div class="ec-title">Snapshots of Imagination — Picture Perception Competition</div>
        <div class="ec-date">Dec '25</div>
      </div>
      <div class="ec-card">
        <div class="ec-badge badge-silver">🥉 2nd Runner-up</div>
        <div class="ec-title">The Argumentative Engineer — Competitive Debate on Societal Issues</div>
        <div class="ec-date">Oct '24</div>
      </div>
      <div class="ec-card">
        <div class="ec-badge badge-silver">🥉 2nd Runner-up</div>
        <div class="ec-title">Battle of the Brains — Grammar Olympiad</div>
        <div class="ec-date">Sep '24</div>
      </div>
      <div class="ec-card">
        <div class="ec-badge badge-gold">🏆 Winner</div>
        <div class="ec-title">Immersion Reader — Presentation Skills Competition</div>
        <div class="ec-date">Sep '24</div>
      </div>
    </div>
  </section>
 
  <div class="divider"></div>
 
  <!-- EDUCATION -->
  <section id="education">
    <h2 class="section-title">Education</h2>
    <div class="edu-grid reveal">
      <div class="edu-card">
        <div class="edu-degree">B.Tech — Computer Science &amp; Engineering</div>
        <div class="edu-school">Lovely Professional University</div>
        <div class="edu-location">Phagwara, Punjab</div>
        <div class="edu-score">
          <span class="edu-score-val">8.0</span>
          <span class="edu-score-label">CGPA</span>
        </div>
        <div class="edu-period">Aug '23 — Present</div>
      </div>
      <div class="edu-card">
        <div class="edu-degree">Class 12 — PCM</div>
        <div class="edu-school">MGN Public School</div>
        <div class="edu-location">Jalandhar, Punjab</div>
        <div class="edu-score">
          <span class="edu-score-val">82%</span>
          <span class="edu-score-label">Score</span>
        </div>
        <div class="edu-period">Mar '22 — May '23</div>
      </div>
      <div class="edu-card">
        <div class="edu-degree">Class 10</div>
        <div class="edu-school">MGN Public School</div>
        <div class="edu-location">Jalandhar, Punjab</div>
        <div class="edu-score">
          <span class="edu-score-val">95.4%</span>
          <span class="edu-score-label">Score</span>
        </div>
        <div class="edu-period">Mar '20 — May '21</div>
      </div>
    </div>
  </section>
 
  <!-- CONTACT -->
  <section id="contact">
    
    <h2 class="contact-title">Contact Me</h2>
    <div class="contact-links">
      <a href="mailto:hanna5@gmail.com" class="contact-item">
        <span class="contact-icon">✉</span> hannasharma05@gmail.com
      </a>
      <a href="tel:+919876876787" class="contact-item">
        <span class="contact-icon">📞</span> +91 7087787147
      </a>
      <a href="https://www.linkedin.com/in/hanna-sharma055/" target="_blank" class="contact-item">
        <span class="contact-icon">💼</span> LinkedIn
      </a>
      <a href="https://github.com/HannaSharma05" target="_blank" class="contact-item">
        <span class="contact-icon">⚡</span> GitHub
      </a>
    </div>
  </section>
 
  
 
</div><!-- end wrapper -->
 
<script>
// ─── CURSOR ───────────────────────────────────────────
const cursor = document.getElementById('cursor');
const ring   = document.getElementById('cursor-ring');
let mx=0, my=0, rx=0, ry=0;
document.addEventListener('mousemove', e => { mx=e.clientX; my=e.clientY; cursor.style.left=mx+'px'; cursor.style.top=my+'px'; });
(function animRing(){
  rx += (mx-rx)*0.12; ry += (my-ry)*0.12;
  ring.style.left=rx+'px'; ring.style.top=ry+'px';
  requestAnimationFrame(animRing);
})();
document.querySelectorAll('a,button').forEach(el=>{
  el.addEventListener('mouseenter',()=>{ ring.style.width='50px'; ring.style.height='50px'; cursor.style.transform='translate(-50%,-50%) scale(1.8)'; cursor.style.background='var(--violet)'; });
  el.addEventListener('mouseleave',()=>{ ring.style.width='34px'; ring.style.height='34px'; cursor.style.transform='translate(-50%,-50%) scale(1)'; cursor.style.background='var(--cyan)'; });
});
 
// ─── STARFIELD ────────────────────────────────────────
const canvas = document.getElementById('starfield');
const ctx    = canvas.getContext('2d');
let W, H, stars=[];
 
function initStars(){
  W=canvas.width=window.innerWidth; H=canvas.height=window.innerHeight;
  stars=[];
  const count=Math.floor(W*H/3500);
  for(let i=0;i<count;i++){
    stars.push({
      x:Math.random()*W, y:Math.random()*H,
      r:Math.random()*1.4+0.2,
      o:Math.random()*0.8+0.2,
      s:Math.random()*0.5+0.1,
      twinkle:Math.random()*Math.PI*2
    });
  }
}
 
function drawStars(ts){
  ctx.clearRect(0,0,W,H);
  stars.forEach(s=>{
    s.twinkle += 0.008;
    const alpha = s.o * (0.6+0.4*Math.sin(s.twinkle));
    ctx.beginPath();
    ctx.arc(s.x,s.y,s.r,0,Math.PI*2);
    ctx.fillStyle=`rgba(212,228,255,${alpha})`;
    ctx.fill();
  });
  requestAnimationFrame(drawStars);
}
window.addEventListener('resize',initStars);
initStars(); requestAnimationFrame(drawStars);
 
// ─── SHOOTING STARS ───────────────────────────────────
function shootingStar(){
  const el=document.createElement('div');
  el.className='shooting-star';
  const x=Math.random()*window.innerWidth;
  const y=Math.random()*window.innerHeight*0.5;
  el.style.cssText=`left:${x}px;top:${y}px;animation-duration:${1.2+Math.random()*1.5}s;transform:rotate(${30+Math.random()*30}deg)`;
  document.body.appendChild(el);
  setTimeout(()=>el.remove(), 3000);
}
setInterval(shootingStar, 4500);
setTimeout(shootingStar, 1000);
 
// ─── SCROLL REVEAL ────────────────────────────────────
const revealEls = document.querySelectorAll('.reveal, .tl-item');
const observer  = new IntersectionObserver(entries=>{
  entries.forEach(e=>{ if(e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold:0.1 });
revealEls.forEach(el=>observer.observe(el));
 
// ─── ACTIVE NAV HIGHLIGHT ─────────────────────────────
const sections  = document.querySelectorAll('section[id]');
const navLinks  = document.querySelectorAll('.nav-links a');
const secObs = new IntersectionObserver(entries=>{
  entries.forEach(e=>{
    if(e.isIntersecting){
      navLinks.forEach(a=>a.style.color='');
      const link=document.querySelector(`.nav-links a[href="#${e.target.id}"]`);
      if(link) link.style.color='var(--cyan)';
    }
  });
},{threshold:0.4});
sections.forEach(s=>secObs.observe(s));
 
// ─── PARALLAX NEBULAE ─────────────────────────────────
window.addEventListener('mousemove',e=>{
  const nx=(e.clientX/window.innerWidth-0.5)*20;
  const ny=(e.clientY/window.innerHeight-0.5)*20;
  document.querySelector('.nebula-1').style.transform=`translate(${nx}px,${ny}px)`;
  document.querySelector('.nebula-2').style.transform=`translate(${-nx*0.6}px,${-ny*0.6}px)`;
});
</script>
</body>
</html>
