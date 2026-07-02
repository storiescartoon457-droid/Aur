<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<title>AURA — AI Face & Hair Analysis</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&display=swap');
:root {
--bg: #0A0A0F;
--surface: #111118;
--surface2: #1A1A24;
--surface3: #22222F;
--border: rgba(255,255,255,0.06);
--border2: rgba(255,255,255,0.12);
--text: #F0EFF8;
--text2: #8E8DA8;
--text3: #5A5975;
--accent: #7C6DFF;
--accent2: #A855F7;
--accent3: #06B6D4;
--gold: #F59E0B;
--rose: #FB7185;
--green: #10B981;
--gradient: linear-gradient(135deg, #7C6DFF 0%, #A855F7 50%, #06B6D4 100%);
--radius: 20px;
--radius-sm: 12px;
}
* { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; }
html { scroll-behavior:smooth; }
body { font-family:'Inter',sans-serif; background:var(--bg); color:var(--text); line-height:1.6; overflow-x:hidden; }

/* BOTTOM NAV */
.bottom-nav {
position:fixed; bottom:0; left:0; right:0; z-index:1000;
background:rgba(17,17,24,0.95); backdrop-filter:blur(20px);
border-top:1px solid var(--border2);
display:flex; justify-content:space-around; align-items:center;
padding:10px 0 20px; height:70px;
}
.nav-item {
display:flex; flex-direction:column; align-items:center; gap:3px;
cursor:pointer; opacity:0.4; transition:all 0.2s;
padding:0 16px;
}
.nav-item.active { opacity:1; }
.nav-item.active .nav-icon { background:var(--gradient); -webkit-background-clip:text; -webkit-text-fill-color:transparent; }
.nav-icon { font-size:22px; }
.nav-label { font-size:9px; font-weight:600; letter-spacing:0.5px; color:var(--text3); }
.nav-item.active .nav-label { color:var(--accent); }

/* SCREENS */
.screen { display:none; min-height:100vh; padding-bottom:80px; }
.screen.active { display:block; }

/* HOME SCREEN */
.home-header {
background:linear-gradient(180deg, rgba(124,109,255,0.1) 0%, transparent 100%);
padding:50px 20px 30px;
}
.home-greeting { font-size:13px; color:var(--text3); margin-bottom:4px; }
.home-name { font-family:'Playfair Display',serif; font-size:28px; font-weight:700; margin-bottom:20px; }
.score-card {
background:var(--surface); border:1px solid var(--border2);
border-radius:24px; padding:24px; margin:0 20px 20px;
box-shadow:0 20px 60px rgba(0,0,0,0.5);
}
.score-row { display:flex; align-items:center; gap:20px; margin-bottom:20px; }
.big-ring {
width:90px; height:90px; flex-shrink:0; border-radius:50%;
background:conic-gradient(#7C6DFF 0% 82%, var(--surface3) 82% 100%);
display:flex; align-items:center; justify-content:center; position:relative;
}
.big-ring::after { content:''; position:absolute; inset:10px; border-radius:50%; background:var(--surface); }
.ring-inner { position:relative; z-index:1; text-align:center; }
.ring-num { font-size:24px; font-weight:700; }
.ring-lbl { font-size:9px; color:var(--text3); }
.score-info h3 { font-size:18px; font-weight:700; margin-bottom:4px; }
.score-info p { font-size:12px; color:var(--text3); }
.score-badge {
display:inline-block; background:rgba(16,185,129,0.15);
color:var(--green); font-size:11px; font-weight:600;
padding:3px 10px; border-radius:100px; margin-top:6px;
}
.metrics-row { display:grid; grid-template-columns:1fr 1fr; gap:12px; }
.metric-item { }
.metric-label { font-size:10px; color:var(--text3); margin-bottom:4px; }
.metric-bar-bg { height:5px; background:var(--surface3); border-radius:3px; }
.metric-bar { height:100%; border-radius:3px; background:var(--gradient); transition:width 1s; }
.metric-val { font-size:11px; font-weight:600; color:var(--text2); text-align:right; margin-top:2px; }

.section-title {
font-size:16px; font-weight:700; padding:0 20px; margin-bottom:12px;
}
.scan-btn {
margin:0 20px 20px;
background:var(--gradient); border-radius:16px; padding:18px;
text-align:center; font-size:16px; font-weight:700; color:white;
cursor:pointer; box-shadow:0 8px 30px rgba(124,109,255,0.4);
transition:all 0.2s; border:none; width:calc(100% - 40px);
display:block;
}
.scan-btn:active { transform:scale(0.97); }

.quick-stats {
display:grid; grid-template-columns:1fr 1fr 1fr;
gap:12px; padding:0 20px; margin-bottom:20px;
}
.stat-card {
background:var(--surface); border:1px solid var(--border);
border-radius:16px; padding:16px; text-align:center;
}
.stat-num {
font-size:22px; font-weight:700;
background:var(--gradient); -webkit-background-clip:text; -webkit-text-fill-color:transparent;
}
.stat-label { font-size:10px; color:var(--text3); margin-top:2px; }

.challenge-card {
margin:0 20px 20px;
background:var(--surface); border:1px solid var(--border);
border-radius:20px; padding:20px;
}
.challenge-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:14px; }
.challenge-title { font-size:15px; font-weight:600; }
.streak-badge {
background:rgba(245,158,11,0.15); color:var(--gold);
font-size:12px; font-weight:700; padding:4px 12px; border-radius:100px;
}
.challenge-item {
display:flex; align-items:center; gap:12px; padding:10px 0;
border-bottom:1px solid var(--border);
}
.challenge-item:last-child { border-bottom:none; }
.challenge-check {
width:24px; height:24px; border-radius:50%;
border:2px solid var(--border2); display:flex;
align-items:center; justify-content:center; cursor:pointer;
flex-shrink:0; transition:all 0.2s;
}
.challenge-check.done { background:var(--green); border-color:var(--green); }
.challenge-text { font-size:13px; color:var(--text2); }
.challenge-text.done { text-decoration:line-through; color:var(--text3); }
</style><style>
/* SCAN SCREEN */
.scan-screen { background:var(--bg); }
.scan-header { padding:50px 20px 20px; text-align:center; }
.scan-header h2 { font-family:'Playfair Display',serif; font-size:26px; font-weight:700; margin-bottom:8px; }
.scan-header p { font-size:13px; color:var(--text3); }
.camera-area {
margin:0 20px; border-radius:24px; overflow:hidden;
background:var(--surface); border:2px dashed var(--border2);
aspect-ratio:3/4; display:flex; flex-direction:column;
align-items:center; justify-content:center; gap:16px;
position:relative;
}
.face-oval {
width:160px; height:200px; border-radius:80px;
border:3px solid var(--accent);
box-shadow:0 0 30px rgba(124,109,255,0.3);
display:flex; align-items:center; justify-content:center;
flex-direction:column; gap:8px;
}
.face-oval-icon { font-size:48px; }
.face-oval-text { font-size:11px; color:var(--accent); font-weight:500; text-align:center; }
.scan-tips {
position:absolute; bottom:16px; left:0; right:0;
text-align:center; font-size:11px; color:var(--text3);
}
.scan-options { display:grid; grid-template-columns:1fr 1fr; gap:12px; padding:16px 20px; }
.scan-option {
background:var(--surface); border:1px solid var(--border2);
border-radius:16px; padding:16px; text-align:center;
cursor:pointer; transition:all 0.2s;
}
.scan-option:active { transform:scale(0.97); border-color:var(--accent); }
.scan-option-icon { font-size:28px; margin-bottom:8px; }
.scan-option-label { font-size:13px; font-weight:600; margin-bottom:4px; }
.scan-option-sub { font-size:11px; color:var(--text3); }
.upload-btn {
margin:0 20px 16px;
background:transparent; border:2px solid var(--accent);
border-radius:16px; padding:16px; text-align:center;
font-size:15px; font-weight:600; color:var(--accent);
cursor:pointer; width:calc(100% - 40px); transition:all 0.2s;
}
.upload-btn:active { background:rgba(124,109,255,0.1); }
.hidden-input { display:none; }

/* ANALYSIS SCREEN */
.analysis-header {
padding:50px 20px 20px;
background:linear-gradient(180deg,rgba(124,109,255,0.08) 0%,transparent 100%);
}
.analysis-header h2 { font-family:'Playfair Display',serif; font-size:24px; font-weight:700; margin-bottom:4px; }
.analysis-date { font-size:12px; color:var(--text3); margin-bottom:16px; }
.analysis-score-card {
background:var(--surface); border:1px solid var(--border2);
border-radius:20px; padding:20px; margin:0 20px 16px;
}
.disclaimer {
background:rgba(245,158,11,0.08); border:1px solid rgba(245,158,11,0.2);
border-radius:12px; padding:12px 14px; margin:0 20px 16px;
font-size:11px; color:var(--gold); line-height:1.5;
}
.results-tabs {
display:flex; gap:8px; padding:0 20px; margin-bottom:16px; overflow-x:auto;
}
.results-tab {
padding:8px 16px; border-radius:100px; font-size:13px; font-weight:600;
border:1px solid var(--border2); color:var(--text3); cursor:pointer;
white-space:nowrap; transition:all 0.2s; background:var(--surface);
}
.results-tab.active { background:var(--gradient); color:white; border-color:transparent; }
.result-section { padding:0 20px; display:none; }
.result-section.active { display:block; }
.result-card {
background:var(--surface); border:1px solid var(--border);
border-radius:16px; padding:16px; margin-bottom:12px;
}
.result-card-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:10px; }
.result-card-title { font-size:14px; font-weight:600; }
.result-score {
font-size:13px; font-weight:700; padding:3px 10px;
border-radius:100px;
}
.score-good { background:rgba(16,185,129,0.15); color:var(--green); }
.score-ok { background:rgba(245,158,11,0.15); color:var(--gold); }
.score-bad { background:rgba(251,113,133,0.15); color:var(--rose); }
.result-bar-bg { height:6px; background:var(--surface3); border-radius:3px; margin-bottom:8px; }
.result-bar { height:100%; border-radius:3px; }
.result-desc { font-size:12px; color:var(--text3); line-height:1.5; }
.strengths-list { list-style:none; }
.strengths-list li {
font-size:13px; color:var(--text2); padding:8px 0;
border-bottom:1px solid var(--border);
display:flex; gap:10px; align-items:flex-start;
}
.strengths-list li:last-child { border-bottom:none; }
.li-icon { flex-shrink:0; }

/* PROGRESS SCREEN */
.progress-header { padding:50px 20px 20px; }
.progress-header h2 { font-family:'Playfair Display',serif; font-size:26px; font-weight:700; margin-bottom:4px; }
.progress-header p { font-size:13px; color:var(--text3); }
.progress-stats {
display:grid; grid-template-columns:1fr 1fr 1fr;
gap:12px; padding:0 20px; margin-bottom:20px;
}
.p-stat {
background:var(--surface); border:1px solid var(--border);
border-radius:16px; padding:14px; text-align:center;
}
.p-stat-num {
font-size:20px; font-weight:700;
background:var(--gradient); -webkit-background-clip:text; -webkit-text-fill-color:transparent;
}
.p-stat-label { font-size:10px; color:var(--text3); margin-top:3px; }
.chart-card {
background:var(--surface); border:1px solid var(--border);
border-radius:20px; padding:20px; margin:0 20px 16px;
}
.chart-title { font-size:14px; font-weight:600; margin-bottom:16px; color:var(--text2); }
.chart-bars { display:flex; align-items:flex-end; gap:8px; height:80px; margin-bottom:8px; }
.chart-bar-wrap { flex:1; display:flex; flex-direction:column; align-items:center; gap:4px; height:100%; justify-content:flex-end; }
.chart-bar { width:100%; border-radius:4px 4px 0 0; background:var(--gradient); min-height:4px; transition:height 1s; }
.chart-bar-label { font-size:9px; color:var(--text3); }
.before-after {
background:var(--surface); border:1px solid var(--border);
border-radius:20px; padding:20px; margin:0 20px 16px;
}
.ba-title { font-size:15px; font-weight:600; margin-bottom:14px; }
.ba-row { display:flex; gap:12px; }
.ba-item { flex:1; text-align:center; }
.ba-box {
background:var(--surface2); border-radius:12px; aspect-ratio:3/4;
display:flex; align-items:center; justify-content:center;
font-size:32px; margin-bottom:8px;
}
.ba-label { font-size:12px; color:var(--text3); }
.ba-score { font-size:16px; font-weight:700; color:var(--accent); }
.export-btn {
margin:0 20px 16px;
background:var(--surface); border:1px solid var(--border2);
border-radius:16px; padding:16px; text-align:center;
font-size:15px; font-weight:600; color:var(--text);
cursor:pointer; width:calc(100% - 40px);
}

/* COACH SCREEN */
.coach-header {
padding:50px 20px 16px;
background:linear-gradient(180deg,rgba(124,109,255,0.08) 0%,transparent 100%);
display:flex; align-items:center; gap:12px;
}
.coach-avatar {
width:44px; height:44px; border-radius:50%;
background:var(--gradient); display:flex;
align-items:center; justify-content:center;
font-size:20px; flex-shrink:0;
}
.coach-info h2 { font-size:18px; font-weight:700; }
.coach-status { font-size:12px; color:var(--green); }
.chat-container {
padding:0 16px; overflow-y:auto;
height:calc(100vh - 220px); margin-bottom:80px;
}
.chat-msg { display:flex; gap:10px; margin-bottom:14px; }
.chat-msg.user { flex-direction:row-reverse; }
.chat-avatar-sm {
width:32px; height:32px; border-radius:50%; flex-shrink:0;
display:flex; align-items:center; justify-content:center; font-size:14px;
}
.ai-av { background:var(--gradient); }
.user-av { background:var(--surface2); border:1px solid var(--border2); }
.chat-bubble {
max-width:75%; background:var(--surface2);
border-radius:18px 18px 18px 4px;
padding:12px 14px; font-size:13px; color:var(--text2); line-height:1.5;
}
.chat-msg.user .chat-bubble {
background:rgba(124,109,255,0.2); border-radius:18px 18px 4px 18px; color:var(--text);
}
.quick-questions { padding:0 16px; margin-bottom:12px; overflow-x:auto; display:flex; gap:8px; }
.quick-q {
background:var(--surface2); border:1px solid var(--border2);
border-radius:100px; padding:8px 14px; font-size:12px; color:var(--text2);
white-space:nowrap; cursor:pointer; flex-shrink:0; transition:all 0.2s;
}
.quick-q:active { background:rgba(124,109,255,0.15); border-color:var(--accent); }
.chat-input-area {
position:fixed; bottom:70px; left:0; right:0;
background:rgba(17,17,24,0.95); backdrop-filter:blur(20px);
padding:10px 16px; border-top:1px solid var(--border);
display:flex; gap:8px; align-items:center;
}
.chat-input {
flex:1; background:var(--surface2); border:1px solid var(--border2);
border-radius:100px; padding:10px 16px; color:var(--text);
font-size:14px; outline:none; font-family:inherit;
}
.chat-send {
width:40px; height:40px; border-radius:50%; background:var(--gradient);
border:none; cursor:pointer; display:flex; align-items:center;
justify-content:center; font-size:18px; flex-shrink:0;
}

/* PROFILE SCREEN */
.profile-header {
padding:50px 20px 30px; text-align:center;
background:linear-gradient(180deg,rgba(124,109,255,0.1) 0%,transparent 100%);
}
.profile-avatar {
width:80px; height:80px; border-radius:50%; margin:0 auto 12px;
background:var(--gradient); display:flex; align-items:center;
justify-content:center; font-size:36px;
border:3px solid rgba(124,109,255,0.3);
}
.profile-name { font-family:'Playfair Display',serif; font-size:22px; font-weight:700; margin-bottom:4px; }
.profile-plan {
display:inline-block; background:rgba(124,109,255,0.15);
color:var(--accent); font-size:12px; font-weight:600;
padding:4px 14px; border-radius:100px; margin-bottom:16px;
}
.profile-stats {
display:grid; grid-template-columns:1fr 1fr 1fr;
gap:12px; padding:0 20px; margin-bottom:20px;
}
.profile-menu { padding:0 20px; }
.menu-section { margin-bottom:20px; }
.menu-section-title { font-size:11px; font-weight:700; letter-spacing:1px; color:var(--text3); margin-bottom:8px; }
.menu-item {
background:var(--surface); border:1px solid var(--border);
border-radius:14px; padding:14px 16px; margin-bottom:8px;
display:flex; align-items:center; gap:12px; cursor:pointer;
transition:all 0.2s;
}
.menu-item:active { background:var(--surface2); }
.menu-icon { font-size:20px; width:36px; height:36px; border-radius:10px; background:var(--surface2); display:flex; align-items:center; justify-content:center; flex-shrink:0; }
.menu-text { flex:1; }
.menu-title { font-size:14px; font-weight:500; }
.menu-sub { font-size:11px; color:var(--text3); margin-top:2px; }
.menu-arrow { color:var(--text3); font-size:16px; }
.upgrade-card {
margin:0 20px 20px;
background:linear-gradient(135deg,rgba(124,109,255,0.2) 0%,rgba(168,85,247,0.2) 100%);
border:1px solid rgba(124,109,255,0.3);
border-radius:20px; padding:20px; text-align:center;
}
.upgrade-title { font-size:18px; font-weight:700; margin-bottom:6px; }
.upgrade-sub { font-size:13px; color:var(--text2); margin-bottom:16px; }
.upgrade-btn {
background:var(--gradient); color:white; border:none;
border-radius:12px; padding:14px 24px; font-size:15px; font-weight:700;
cursor:pointer; width:100%;
}
</style></head>
<body>

<!-- HOME SCREEN -->
<div class="screen active" id="screen-home">
<div class="home-header">
<div class="home-greeting">Good morning ☀️</div>
<div class="home-name">AURA</div>
</div>
<div class="score-card">
<div class="score-row">
<div class="big-ring">
<div class="ring-inner">
<div class="ring-num">82</div>
<div class="ring-lbl">/100</div>
</div>
</div>
<div class="score-info">
<h3>Overall Score</h3>
<p>Last scan: Today</p>
<div class="score-badge">↑4 this week</div>
</div>
</div>
<div class="metrics-row">
<div class="metric-item">
<div class="metric-label">Skin Clarity</div>
<div class="metric-bar-bg"><div class="metric-bar" style="width:78%"></div></div>
<div class="metric-val">78</div>
</div>
<div class="metric-item">
<div class="metric-label">Hair Health</div>
<div class="metric-bar-bg"><div class="metric-bar" style="width:85%"></div></div>
<div class="metric-val">85</div>
</div>
<div class="metric-item">
<div class="metric-label">Symmetry</div>
<div class="metric-bar-bg"><div class="metric-bar" style="width:88%"></div></div>
<div class="metric-val">88</div>
</div>
<div class="metric-item">
<div class="metric-label">Grooming</div>
<div class="metric-bar-bg"><div class="metric-bar" style="width:74%"></div></div>
<div class="metric-val">74</div>
</div>
</div>
</div>

<button class="scan-btn" onclick="showScreen('scan')">✦ Start New AI Scan</button>

<div class="quick-stats">
<div class="stat-card">
<div class="stat-num">7</div>
<div class="stat-label">Total Scans</div>
</div>
<div class="stat-card">
<div class="stat-num">🔥14</div>
<div class="stat-label">Day Streak</div>
</div>
<div class="stat-card">
<div class="stat-num">+12</div>
<div class="stat-label">Skin Pts</div>
</div>
</div>

<div class="section-title">Daily Challenges</div>
<div class="challenge-card">
<div class="challenge-header">
<div class="challenge-title">Today's Goals</div>
<div class="streak-badge">🔥 14 days</div>
</div>
<div class="challenge-item">
<div class="challenge-check done" onclick="toggleChallenge(this)">✓</div>
<div class="challenge-text done">Drink 8 glasses of water</div>
</div>
<div class="challenge-item">
<div class="challenge-check" onclick="toggleChallenge(this)"></div>
<div class="challenge-text">Apply morning skincare routine</div>
</div>
<div class="challenge-item">
<div class="challenge-check" onclick="toggleChallenge(this)"></div>
<div class="challenge-text">Use SPF 50 sunscreen</div>
</div>
<div class="challenge-item">
<div class="challenge-check" onclick="toggleChallenge(this)"></div>
<div class="challenge-text">Sleep before 11 PM</div>
</div>
<div class="challenge-item">
<div class="challenge-check" onclick="toggleChallenge(this)"></div>
<div class="challenge-text">Oil hair for 30 minutes</div>
</div>
</div>
</div>

<!-- SCAN SCREEN -->
<div class="screen" id="screen-scan">
<div class="scan-header">
<h2>AI Face Scan</h2>
<p>Position your face clearly in good lighting</p>
</div>
<div class="camera-area">
<div class="face-oval">
<div class="face-oval-icon">👤</div>
<div class="face-oval-text">Position face<br>in oval</div>
</div>
<div class="scan-tips">Hold steady · Good lighting · Face forward</div>
</div>
<div class="scan-options">
<div class="scan-option" onclick="document.getElementById('photoInput').click()">
<div class="scan-option-icon">📷</div>
<div class="scan-option-label">Take Photo</div>
<div class="scan-option-sub">Use camera</div>
</div>
<div class="scan-option" onclick="document.getElementById('galleryInput').click()">
<div class="scan-option-icon">🖼️</div>
<div class="scan-option-label">From Gallery</div>
<div class="scan-option-sub">Upload photo</div>
</div>
</div>
<input type="file" id="photoInput" class="hidden-input" accept="image/*" capture="user" onchange="startAnalysis()">
<input type="file" id="galleryInput" class="hidden-input" accept="image/*" onchange="startAnalysis()">
<button class="upload-btn" onclick="startAnalysis()">✦ Analyse My Face & Hair</button>
</div>

<!-- ANALYSIS SCREEN -->
<div class="screen" id="screen-analysis">
<div class="analysis-header">
<h2>Your Results</h2>
<div class="analysis-date">Analysed today · 2 photos</div>
</div>
<div class="disclaimer">
⚠️ Scores are for entertainment and grooming guidance only. Beauty is subjective and these scores do not reflect real-world attractiveness or self-worth.
</div>
<div class="analysis-score-card">
<div class="score-row">
<div class="big-ring">
<div class="ring-inner">
<div class="ring-num">82</div>
<div class="ring-lbl">/100</div>
</div>
</div>
<div class="score-info">
<h3>Overall: Good</h3>
<p>Oval face · Combination skin</p>
<div class="score-badge">↑4 from last scan</div>
</div>
</div>
</div>
<div class="results-tabs">
<div class="results-tab active" onclick="showTab('face')">Face</div>
<div class="results-tab" onclick="showTab('hair')">Hair</div>
<div class="results-tab" onclick="showTab('recommendations')">Tips</div>
<div class="results-tab" onclick="showTab('style')">Style</div>
</div>

<!-- FACE TAB -->
<div class="result-section active" id="tab-face">
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Face Shape</div>
<div class="result-score score-good">Oval</div>
</div>
<div class="result-desc">Oval face shape is the most versatile — almost every hairstyle and glasses frame suits you.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Skin Type</div>
<div class="result-score score-ok">Combination</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:65%;background:var(--gold)"></div></div>
<div class="result-desc">Oily T-zone with normal to dry cheeks. Use a gentle gel cleanser and lightweight moisturiser.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Dark Circles</div>
<div class="result-score score-ok">Mild</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:40%;background:var(--gold)"></div></div>
<div class="result-desc">Mild under-eye darkness detected. Improve sleep quality and use caffeine eye cream.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Pigmentation</div>
<div class="result-score score-ok">Light</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:30%;background:var(--gold)"></div></div>
<div class="result-desc">Light hyperpigmentation on cheeks. Consistent SPF 50 use will help significantly.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Acne</div>
<div class="result-score score-good">Clear</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:10%;background:var(--green)"></div></div>
<div class="result-desc">No active acne detected. Keep up your current cleansing routine.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Age Estimate</div>
<div class="result-score score-good">22–26</div>
</div>
<div class="result-desc">Your skin appears youthful with good elasticity and minimal fine lines.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">💪 Strengths</div>
</div>
<ul class="strengths-list">
<li><span class="li-icon">✅</span>Excellent facial symmetry</li>
<li><span class="li-icon">✅</span>Clear skin with no active acne</li>
<li><span class="li-icon">✅</span>Youthful appearance for age range</li>
<li><span class="li-icon">✅</span>Well defined beard line</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">🎯 Areas to Improve</div>
</div>
<ul class="strengths-list">
<li><span class="li-icon">→</span>Reduce dark circles with better sleep</li>
<li><span class="li-icon">→</span>Use SPF daily for pigmentation</li>
<li><span class="li-icon">→</span>Add eye cream to night routine</li>
<li><span class="li-icon">→</span>Stay hydrated — drink more water</li>
</ul>
</div>
</div>

<!-- HAIR TAB -->
<div class="result-section" id="tab-hair">
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Hair Type</div>
<div class="result-score score-good">Straight</div>
</div>
<div class="result-desc">Type 1A — Fine and straight. Your hair has natural shine and low frizz tendency.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Hair Health</div>
<div class="result-score score-good">85/100</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:85%;background:var(--green)"></div></div>
<div class="result-desc">Your hair is in good health. Strong shine and low breakage detected.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Shine Level</div>
<div class="result-score score-good">High</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:88%;background:var(--green)"></div></div>
<div class="result-desc">Excellent natural shine. Your hair cuticles are well aligned.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Frizz Level</div>
<div class="result-score score-good">Low</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:20%;background:var(--green)"></div></div>
<div class="result-desc">Minimal frizz detected. Your hair retains moisture well.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Split Ends</div>
<div class="result-score score-ok">Minor</div>
</div>
<div class="result-bar-bg"><div class="result-bar" style="width:25%;background:var(--gold)"></div></div>
<div class="result-desc">Minor split ends at tips. Apply argan oil to ends 2x weekly.</div>
</div>
<div class="result-card">
<div class="result-card-header">
<div class="result-card-title">Scalp Condition</div>
<div class="result-score score-good">Healthy</div>
</div>
<div class="result-desc">Scalp appears healthy with no visible dandruff or excessive oiliness.</div>
</div>
</div>

<!-- RECOMMENDATIONS TAB -->
<div class="result-section" id="tab-recommendations">
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">🧴 Morning Skincare</div></div>
<ul class="strengths-list">
<li><span class="li-icon">1️⃣</span>Gentle gel face wash</li>
<li><span class="li-icon">2️⃣</span>Vitamin C serum</li>
<li><span class="li-icon">3️⃣</span>Lightweight moisturiser</li>
<li><span class="li-icon">4️⃣</span>SPF 50 sunscreen — every day</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">🌙 Night Skincare</div></div>
<ul class="strengths-list">
<li><span class="li-icon">1️⃣</span>Micellar water or oil cleanser</li>
<li><span class="li-icon">2️⃣</span>Niacinamide serum for pigmentation</li>
<li><span class="li-icon">3️⃣</span>Retinol 0.1% eye cream</li>
<li><span class="li-icon">4️⃣</span>Heavy moisturiser or face oil</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">💇 Hair Routine</div></div>
<ul class="strengths-list">
<li><span class="li-icon">Mon</span>Shampoo with sulfate-free formula</li>
<li><span class="li-icon">Wed</span>Deep conditioning mask — 20 min</li>
<li><span class="li-icon">Fri</span>Argan oil on tips before bed</li>
<li><span class="li-icon">Sun</span>Scalp massage with coconut oil</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">🥗 Diet & Lifestyle</div></div>
<ul class="strengths-list">
<li><span class="li-icon">💧</span>Drink 8 glasses of water daily</li>
<li><span class="li-icon">😴</span>Sleep 7–8 hours before midnight</li>
<li><span class="li-icon">🥦</span>Add zinc and biotin rich foods</li>
<li><span class="li-icon">☀️</span>Avoid peak sun 12PM–3PM</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">🌿 Home Remedies</div></div>
<ul class="strengths-list">
<li><span class="li-icon">✓</span>Aloe vera gel for dark circles</li>
<li><span class="li-icon">✓</span>Turmeric mask for pigmentation</li>
<li><span class="li-icon">✓</span>Onion juice for hair growth</li>
<li><span class="li-icon">✓</span>Green tea toner for oily skin</li>
</ul>
</div>
</div>

<!-- STYLE TAB -->
<div class="result-section" id="tab-style">
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">💈 Best Hairstyles for You</div></div>
<ul class="strengths-list">
<li><span class="li-icon">✓</span>Wolf cut — suits oval face perfectly</li>
<li><span class="li-icon">✓</span>Textured fringe with layers</li>
<li><span class="li-icon">✓</span>Side part with volume on top</li>
<li><span class="li-icon">✓</span>Curtain bangs — very trendy</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">🕶️ Best Glasses Frames</div></div>
<ul class="strengths-list">
<li><span class="li-icon">✓</span>Aviator — classic choice</li>
<li><span class="li-icon">✓</span>Round frames — artistic look</li>
<li><span class="li-icon">✓</span>Square frames — structured look</li>
<li><span class="li-icon">✗</span>Avoid oversized frames</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">🧔 Beard Styles</div></div>
<ul class="strengths-list">
<li><span class="li-icon">✓</span>Short stubble — your current look</li>
<li><span class="li-icon">✓</span>Extended goatee</li>
<li><span class="li-icon">✓</span>Full beard — medium length</li>
<li><span class="li-icon">✓</span>Clean shave also works well</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">👕 Clothing Necklines</div></div>
<ul class="strengths-list">
<li><span class="li-icon">✓</span>V-neck — elongates neck</li>
<li><span class="li-icon">✓</span>Crew neck — classic clean look</li>
<li><span class="li-icon">✓</span>Collared shirts — sharp appearance</li>
<li><span class="li-icon">✓</span>Turtleneck — modern style</li>
</ul>
</div>
<div class="result-card">
<div class="result-card-header"><div class="result-card-title">⭐ Celebrity Matches</div></div>
<div class="result-desc" style="margin-bottom:10px">* Approximate matches based on facial geometry only — for fun!</div>
<ul class="strengths-list">
<li><span class="li-icon">🎬</span>Zayn Malik — similar bone structure</li>
<li><span class="li-icon">🎬</span>Timothée Chalamet — face shape</li>
<li><span class="li-icon">🎬</span>Noah Centineo — hair texture</li>
</ul>
</div>
</div>
</div>

<!-- PROGRESS SCREEN -->
<div class="screen" id="screen-progress">
<div class="progress-header">
<h2>Your Progress</h2>
<p>Tracking since Day 1</p>
</div>
<div class="progress-stats">
<div class="p-stat"><div class="p-stat-num">7</div><div class="p-stat-label">Scans Done</div></div>
<div class="p-stat"><div class="p-stat-num">🔥14</div><div class="p-stat-label">Day Streak</div></div>
<div class="p-stat"><div class="p-stat-num">+12</div><div class="p-stat-label">Skin Score</div></div>
</div>
<div class="chart-card">
<div class="chart-title">Skin Score — Last 7 Scans</div>
<div class="chart-bars">
<div class="chart-bar-wrap"><div class="chart-bar" style="height:55%"></div><div class="chart-bar-label">W1</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:58%"></div><div class="chart-bar-label">W2</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:63%"></div><div class="chart-bar-label">W3</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:68%"></div><div class="chart-bar-label">W4</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:72%"></div><div class="chart-bar-label">W5</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:75%"></div><div class="chart-bar-label">W6</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:78%"></div><div class="chart-bar-label">W7</div></div>
</div>
</div>
<div class="chart-card">
<div class="chart-title">Hair Score — Last 7 Scans</div>
<div class="chart-bars">
<div class="chart-bar-wrap"><div class="chart-bar" style="height:70%;background:var(--accent3)"></div><div class="chart-bar-label">W1</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:72%;background:var(--accent3)"></div><div class="chart-bar-label">W2</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:75%;background:var(--accent3)"></div><div class="chart-bar-label">W3</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:78%;background:var(--accent3)"></div><div class="chart-bar-label">W4</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:80%;background:var(--accent3)"></div><div class="chart-bar-label">W5</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:83%;background:var(--accent3)"></div><div class="chart-bar-label">W6</div></div>
<div class="chart-bar-wrap"><div class="chart-bar" style="height:85%;background:var(--accent3)"></div><div class="chart-bar-label">W7</div></div>
</div>
</div>
<div class="before-after">
<div class="ba-title">Before & After</div>
<div class="ba-row">
<div class="ba-item">
<div class="ba-box">📸</div>
<div class="ba-label">Week 1</div>
<div class="ba-score">70</div>
</div>
<div class="ba-item" style="display:flex;align-items:center;justify-content:center;font-size:24px;">→</div>
<div class="ba-item">
<div class="ba-box">🤩</div>
<div class="ba-label">Now</div>
<div class="ba-score" style="color:var(--green)">82</div>
</div>
</div>
</div>
<button class="export-btn">📄 Export PDF Report (Pro)</button>
</div>

<!-- COACH SCREEN -->
<div class="screen" id="screen-coach">
<div class="coach-header">
<div class="coach-avatar">✦</div>
<div class="coach-info">
<h2>AURA Coach</h2>
<div class="coach-status">● Online — Context aware</div>
</div>
</div>
<div class="chat-container" id="chatContainer">
<div class="chat-msg">
<div class="chat-avatar-sm ai-av">✦</div>
<div class="chat-bubble">Hi! I've reviewed your latest scan. You have combination skin and your dark circles need attention. I also noticed your hair health is excellent at 85/100. What would you like to work on first?</div>
</div>
<div class="chat-msg">
<div class="chat-avatar-sm ai-av">✦</div>
<div class="chat-bubble">You can ask me anything about your face, hair, skincare routine, products, or lifestyle. I know your full analysis history!</div>
</div>
</div>
<div class="quick-questions">
<div class="quick-q" onclick="askQuestion(this)">Why is my hair falling?</div>
<div class="quick-q" onclick="askQuestion(this)">Best products for me?</div>
<div class="quick-q" onclick="askQuestion(this)">How to remove dark circles?</div>
<div class="quick-q" onclick="askQuestion(this)">What hairstyle suits me?</div>
<div class="quick-q" onclick="askQuestion(this)">Morning routine steps?</div>
</div>
<div class="chat-input-area">
<input class="chat-input" id="chatInput" placeholder="Ask anything about your face or hair..." onkeydown="if(event.key==='Enter')sendMessage()">
<button class="chat-send" onclick="sendMessage()">↑</button>
</div>
</div>

<!-- PROFILE SCREEN -->
<div class="screen" id="screen-profile">
<div class="profile-header">
<div class="profile-avatar">👤</div>
<div class="profile-name">Your Profile</div>
<div class="profile-plan">Free Plan</div>
</div>
<div class="profile-stats">
<div class="p-stat"><div class="p-stat-num">7</div><div class="p-stat-label">Scans</div></div>
<div class="p-stat"><div class="p-stat-num">82</div><div class="p-stat-label">Best Score</div></div>
<div class="p-stat"><div class="p-stat-num">🔥14</div><div class="p-stat-label">Streak</div></div>
</div>
<div class="upgrade-card">
<div class="u
