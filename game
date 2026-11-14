<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Super Question Quest v3 — MEGA</title>
<style>
  * { margin:0; padding:0; box-sizing:border-box; font-family:'Segoe UI',Tahoma,sans-serif; }
  body {
    background: radial-gradient(circle at 20% 20%, #2a1a4e, #0f0f23 60%, #050510);
    min-height:100vh; color:#fff; display:flex; justify-content:center; align-items:center;
    padding:16px; overflow-x:hidden;
  }
  #game {
    width:100%; max-width:1000px;
    background:rgba(255,255,255,0.04);
    border-radius:24px; padding:24px;
    box-shadow:0 20px 80px rgba(120,0,255,0.25), inset 0 0 40px rgba(255,215,0,0.05);
    border:2px solid rgba(255,215,0,0.25);
    position:relative;
  }
  h1 {
    text-align:center; font-size:1.8em; margin-bottom:4px;
    background:linear-gradient(90deg,#ffd700,#ff6b6b,#a78bfa,#4ade80,#ffd700);
    background-size:400% 100%;
    -webkit-background-clip:text; -webkit-text-fill-color:transparent;
    animation: shine 5s linear infinite;
  }
  @keyframes shine { to { background-position:400% 0; } }
  .subtitle { text-align:center; color:#9a8fb8; margin-bottom:14px; font-size:0.85em; letter-spacing:1px; }

  #hud {
    display:flex; justify-content:space-between; align-items:center;
    background:rgba(0,0,0,0.4); padding:10px 16px; border-radius:14px;
    margin-bottom:14px; flex-wrap:wrap; gap:8px; font-size:0.9em;
    border:1px solid rgba(255,215,0,0.15);
  }
  .hud-item { font-weight:bold; }
  .hud-item span { color:#ffd700; }
  #livesBox span { color:#ff6b6b; }
  #streakBox { color:#4ade80; }

  .screen { display:none; }
  .screen.active { display:block; animation: fadeIn 0.4s ease; }
  @keyframes fadeIn { from { opacity:0; transform:translateY(10px); } to { opacity:1; transform:none; } }

  /* MAP */
  #map { display:grid; grid-template-columns:repeat(auto-fill,minmax(140px,1fr)); gap:10px; }
  .mission-card {
    border:2px solid #333; border-radius:14px; padding:12px 8px;
    background:rgba(255,255,255,0.04); text-align:center; cursor:not-allowed;
    transition:all 0.3s; position:relative; min-height:110px;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
  }
  .mission-card .icon { font-size:1.8em; margin-bottom:4px; }
  .mission-card .name { font-size:0.78em; font-weight:bold; line-height:1.2; }
  .mission-card .tagline { font-size:0.65em; color:#888; margin-top:3px; }
  .mission-card.unlocked {
    border-color:#ffd700; cursor:pointer; color:#fff;
    background:linear-gradient(135deg,rgba(255,215,0,0.12),rgba(167,139,250,0.12));
    box-shadow:0 0 18px rgba(255,215,0,0.2);
  }
  .mission-card.unlocked:hover { transform:translateY(-3px) scale(1.03); box-shadow:0 8px 25px rgba(255,215,0,0.4); }
  .mission-card.completed { border-color:#4ade80; background:rgba(74,222,128,0.12); color:#fff; cursor:pointer; }
  .mission-card.completed::after { content:'✓'; position:absolute; top:6px; right:10px; color:#4ade80; font-size:1.2em; }
  .mission-card.locked { opacity:0.45; }

  /* INTRO */
  .intro-slide { background:rgba(0,0,0,0.4); border-radius:18px; padding:22px; border:2px solid rgba(255,215,0,0.3); }
  .intro-slide h2 { font-size:1.4em; margin-bottom:4px; }
  .intro-slide .type-badge { display:inline-block; padding:3px 10px; border-radius:8px; font-size:0.75em; font-weight:bold; color:#1a1a2e; margin-bottom:10px; }
  .b-general { background:#4ade80; }
  .b-special { background:#60a5fa; }
  .b-alt { background:#f472b6; }
  .b-tag { background:#c084fc; }
  .example-box {
    background:rgba(255,255,255,0.06); border-left:4px solid #ffd700;
    padding:12px 16px; border-radius:10px; margin:10px 0; font-size:1em; line-height:1.5;
  }
  .example-box .q-en { color:#ffd700; font-weight:bold; }
  .example-box .answer { color:#4ade80; font-size:0.9em; margin-top:3px; }
  .example-box .why { color:#9a8fb8; font-size:0.82em; margin-top:5px; font-style:italic; }
  .nav-btns { display:flex; gap:10px; margin-top:16px; }
  .btn { flex:1; padding:12px; border:none; border-radius:12px; font-size:1em; font-weight:bold; cursor:pointer; transition:transform 0.15s; }
  .btn:active { transform:scale(0.97); }
  .btn-primary { background:linear-gradient(90deg,#ffd700,#ff9500); color:#1a1a2e; }
  .btn-primary:hover { transform:scale(1.02); }
  .btn-ghost { background:transparent; border:2px solid #555; color:#aaa; }
  .btn-ghost:hover { color:#fff; border-color:#fff; }
  .btn:disabled { opacity:0.3; cursor:not-allowed; }

  .mini-check { margin-top:12px; padding:12px; background:rgba(96,165,250,0.1); border-radius:10px; border:1px dashed #60a5fa; }
  .mini-check p { font-size:0.9em; margin-bottom:8px; color:#93c5fd; }
  .mini-opt {
    display:inline-block; margin:4px 6px 0 0; padding:8px 14px; border-radius:8px;
    background:rgba(255,255,255,0.08); border:1px solid #444; cursor:pointer; font-size:0.9em;
    transition:all 0.2s;
  }
  .mini-opt:hover { background:rgba(96,165,250,0.3); border-color:#60a5fa; }
  .mini-opt.ok { background:#4ade80; color:#1a1a2e; border-color:#4ade80; }
  .mini-opt.bad { background:#ff6b6b; color:#fff; border-color:#ff6b6b; }

  /* RULE */
  .rule-builder { background:rgba(0,0,0,0.4); border-radius:18px; padding:22px; }
  .rule-row {
    background:rgba(255,255,255,0.05); border-radius:12px; padding:12px 14px;
    margin-bottom:10px; border-left:4px solid #a78bfa;
  }
  .rule-row .type { font-weight:bold; margin-bottom:6px; font-size:0.95em; }
  .rule-row .slots { display:flex; gap:6px; flex-wrap:wrap; align-items:center; font-size:0.95em; }
  .slot {
    min-width:80px; min-height:32px; padding:4px 10px; border:2px dashed #666;
    border-radius:8px; text-align:center; cursor:pointer; transition:all 0.2s;
    display:inline-flex; align-items:center; justify-content:center; font-size:0.9em;
  }
  .slot.filled { border-style:solid; border-color:#4ade80; background:rgba(74,222,128,0.15); color:#4ade80; font-weight:bold; }
  .slot:hover:not(.filled) { border-color:#ffd700; background:rgba(255,215,0,0.1); }
  .chip-pool {
    display:flex; flex-wrap:wrap; gap:8px; margin-top:14px; padding:12px;
    background:rgba(0,0,0,0.3); border-radius:12px; min-height:55px;
  }
  .chip {
    padding:7px 14px; background:linear-gradient(135deg,#ffd700,#ff9500); color:#1a1a2e;
    border-radius:10px; font-weight:bold; cursor:pointer; user-select:none; transition:transform 0.15s;
    font-size:0.95em;
  }
  .chip:hover { transform:scale(1.05); }
  .chip.used { opacity:0.3; pointer-events:none; }

  /* QUIZ */
  .quiz-header { display:flex; justify-content:space-between; align-items:center; margin-bottom:10px; gap:8px; }
  .timer-wrap { flex:1; height:8px; background:rgba(0,0,0,0.4); border-radius:4px; overflow:hidden; }
  #timerBar { height:100%; width:100%; background:linear-gradient(90deg,#4ade80,#ffd700); transition:width 0.1s linear; }
  #timerBar.low { background:linear-gradient(90deg,#ff6b6b,#ff9500); }
  .progress-wrap { height:6px; background:rgba(0,0,0,0.4); border-radius:3px; overflow:hidden; margin-bottom:14px; }
  #progressFill { height:100%; background:linear-gradient(90deg,#a78bfa,#ffd700); width:0%; transition:width 0.4s; }
  .question-box { background:rgba(0,0,0,0.4); border-radius:16px; padding:20px; border:2px solid rgba(167,139,250,0.25); }
  #questionText { font-size:1.2em; line-height:1.6; margin-bottom:16px; min-height:50px; }
  #options { display:grid; gap:9px; }
  .option-btn {
    padding:12px 16px; border:2px solid #444; border-radius:12px;
    background:rgba(255,255,255,0.05); color:#fff; font-size:1em; cursor:pointer;
    text-align:left; transition:all 0.2s;
  }
  .option-btn:hover:not(:disabled) { background:rgba(167,139,250,0.15); border-color:#a78bfa; transform:translateX(5px); }
  .option-btn.correct { background:rgba(74,222,128,0.25); border-color:#4ade80; }
  .option-btn.wrong { background:rgba(255,107,107,0.25); border-color:#ff6b6b; }
  .option-btn:disabled { cursor:not-allowed; }
  #feedback { margin-top:12px; font-size:0.95em; font-weight:bold; text-align:center; min-height:44px; line-height:1.5; }
  .fb-correct { color:#4ade80; }
  .fb-wrong { color:#ff6b6b; }

  /* WORD CHAOS */
  .word-chaos-zone {
    display:flex; flex-wrap:wrap; gap:8px; padding:14px; background:rgba(0,0,0,0.3);
    border-radius:12px; min-height:60px; margin-bottom:12px; border:2px dashed #555;
  }
  .word-chip {
    padding:9px 14px; background:linear-gradient(135deg,#a78bfa,#60a5fa); color:#fff;
    border-radius:10px; font-weight:bold; cursor:pointer; transition:all 0.2s; font-size:0.95em;
  }
  .word-chip:hover { transform:scale(1.06); }
  .word-chip.placed { background:linear-gradient(135deg,#ffd700,#ff9500); color:#1a1a2e; }
  .sentence-zone {
    min-height:60px; padding:14px; background:rgba(255,255,255,0.05);
    border-radius:12px; border:2px solid #4ade80; margin-bottom:12px;
    display:flex; flex-wrap:wrap; gap:8px; align-items:center;
  }
  .sentence-zone .placeholder { color:#666; font-style:italic; font-size:0.9em; }

  /* MATCH */
  .match-grid { display:grid; grid-template-columns:1fr 1fr; gap:12px; margin-top:10px; }
  .match-col { display:flex; flex-direction:column; gap:8px; }
  .match-item {
    padding:12px 14px; border:2px solid #444; border-radius:10px;
    background:rgba(255,255,255,0.05); cursor:pointer; transition:all 0.2s; font-size:0.92em;
  }
  .match-item:hover { border-color:#ffd700; }
  .match-item.selected { border-color:#ffd700; background:rgba(255,215,0,0.2); }
  .match-item.matched { border-color:#4ade80; background:rgba(74,222,128,0.15); cursor:default; opacity:0.6; }
  .match-item.wrong-flash { animation: wrongFlash 0.4s; }
  @keyframes wrongFlash { 0%,100% { background:rgba(255,255,255,0.05); } 50% { background:rgba(255,107,107,0.4); } }

  /* DRAGON */
  #dragonArea { text-align:center; margin-bottom:14px; }
  #dragonEmoji { font-size:4em; display:inline-block; transition:transform 0.3s; }
  #dragonEmoji.hit { animation: shake 0.4s; }
  @keyframes shake {
    0%,100% { transform:translateX(0); }
    25% { transform:translateX(-14px) rotate(-6deg); }
    75% { transform:translateX(14px) rotate(6deg); }
  }
  #dragonHPBar { height:16px; background:rgba(0,0,0,0.5); border-radius:10px; overflow:hidden; margin:8px auto; max-width:400px; border:2px solid #ff6b6b; }
  #dragonHPFill { height:100%; background:linear-gradient(90deg,#ff6b6b,#ff0000); width:100%; transition:width 0.5s; }

  /* MODAL */
  #modal {
    display:none; position:fixed; inset:0; background:rgba(0,0,0,0.85);
    justify-content:center; align-items:center; z-index:100; padding:20px;
    backdrop-filter:blur(6px);
  }
  #modalContent {
    background:linear-gradient(135deg,#1a1a3e,#0f0f23); padding:30px;
    border-radius:22px; border:3px solid #ffd700; text-align:center;
    max-width:560px; box-shadow:0 0 60px rgba(255,215,0,0.5);
    max-height:90vh; overflow-y:auto; animation: popIn 0.3s ease;
  }
  @keyframes popIn { from { transform:scale(0.85); opacity:0; } to { transform:scale(1); opacity:1; } }
  #modalContent h2 { color:#ffd700; margin-bottom:12px; font-size:1.5em; }
  #modalContent p { margin-bottom:16px; font-size:1em; line-height:1.6; }
  #modalBtn { padding:12px 28px; font-size:1em; font-weight:bold; border:none; border-radius:12px; background:linear-gradient(90deg,#ffd700,#ff9500); color:#1a1a2e; cursor:pointer; }
  #modalBtn:hover { transform:scale(1.05); }

  .float-fb {
    position:fixed; font-size:2em; font-weight:bold; pointer-events:none;
    animation: floatUp 1s ease forwards; z-index:200;
  }
  @keyframes floatUp {
    from { opacity:1; transform:translateY(0) scale(1); }
    to { opacity:0; transform:translateY(-80px) scale(1.6); }
  }

  @media (max-width:600px) {
    #map { grid-template-columns:repeat(2,1fr); }
    h1 { font-size:1.3em; }
    #game { padding:14px; }
    .match-grid { grid-template-columns:1fr; }
  }
</style>
</head>
<body>
<div id="game">
  <h1>🏰 Super Question Quest v3</h1>
  <p class="subtitle">MEGA • Types of Questions • have got / was / were</p>

  <div id="hud">
    <div class="hud-item">🪙 <span id="coinCount">0</span></div>
    <div class="hud-item" id="livesBox">❤️ <span id="livesCount">3</span></div>
    <div class="hud-item" id="streakBox">🔥 x<span id="streakCount">0</span></div>
    <div class="hud-item">🎯 <span id="scoreCount">0</span>/<span id="scoreTotal">0</span></div>
  </div>

  <div class="screen active" id="screen-map"><div id="map"></div></div>
  <div class="screen" id="screen-intro"><div class="intro-slide" id="introSlideContent"></div></div>
  <div class="screen" id="screen-rule"><div class="rule-builder" id="ruleBuilder"></div></div>

  <div class="screen" id="screen-quiz">
    <div class="quiz-header">
      <button class="btn btn-ghost" style="flex:0;padding:8px 14px;font-size:0.85em;" id="quitBtn">← Карта</button>
      <div class="timer-wrap"><div id="timerBar"></div></div>
      <div style="font-size:0.85em;color:#9a8fb8;">⏱ <span id="timerText">0</span>s</div>
    </div>
    <div class="progress-wrap"><div id="progressFill"></div></div>
    <div id="dragonArea" style="display:none;">
      <span id="dragonEmoji">🐉</span>
      <div id="dragonHPBar"><div id="dragonHPFill"></div></div>
    </div>
    <div class="question-box">
      <div id="questionText"></div>
      <div id="options"></div>
      <div id="feedback"></div>
      <button class="btn btn-primary" id="nextBtn" style="display:none;margin-top:12px;">Далее →</button>
    </div>
  </div>

  <div class="screen" id="screen-chaos">
    <div class="quiz-header">
      <button class="btn btn-ghost" style="flex:0;padding:8px 14px;font-size:0.85em;" id="chaosQuitBtn">← Карта</button>
      <div style="flex:1;text-align:center;font-size:0.9em;color:#ffd700;" id="chaosTask"></div>
    </div>
    <div class="progress-wrap"><div id="chaosProgress"></div></div>
    <div class="question-box">
      <div id="chaosInstruction" style="margin-bottom:12px;font-size:0.92em;color:#9a8fb8;"></div>
      <div class="sentence-zone" id="sentenceZone"><span class="placeholder">Клик по словам ниже, чтобы собрать вопрос…</span></div>
      <div class="word-chaos-zone" id="wordChaosZone"></div>
      <div id="chaosFeedback" style="text-align:center;min-height:26px;margin-top:6px;font-weight:bold;"></div>
      <button class="btn btn-primary" id="chaosCheckBtn">Проверить</button>
    </div>
  </div>

  <div class="screen" id="screen-match">
    <div class="quiz-header">
      <button class="btn btn-ghost" style="flex:0;padding:8px 14px;font-size:0.85em;" id="matchQuitBtn">← Карта</button>
      <div style="flex:1;text-align:center;font-size:0.9em;color:#ffd700;" id="matchTask"></div>
    </div>
    <div class="progress-wrap"><div id="matchProgress"></div></div>
    <div class="question-box">
      <div style="margin-bottom:10px;font-size:0.92em;color:#9a8fb8;">Клик на вопрос → клик на ответ. Найди пару!</div>
      <div class="match-grid">
        <div class="match-col" id="matchLeft"></div>
        <div class="match-col" id="matchRight"></div>
      </div>
      <div id="matchFeedback" style="text-align:center;min-height:26px;margin-top:10px;font-weight:bold;"></div>
    </div>
  </div>
</div>

<div id="modal">
  <div id="modalContent">
    <h2 id="modalTitle"></h2>
    <p id="modalText"></p>
    <button id="modalBtn">Ок</button>
  </div>
</div>

<script>
// ============================================================
// UTILS
// ============================================================
function shuffle(arr) {
  const a = arr.slice();
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]];
  }
  return a;
}
function shuffleWithCorrect(question) {
  // перемешать opts и пересчитать correct
  const idxMap = question.opts.map((_, i) => i);
  const shuffledIdx = shuffle(idxMap);
  const newOpts = shuffledIdx.map(i => question.opts[i]);
  const newCorrect = shuffledIdx.indexOf(question.correct);
  return { ...question, opts: newOpts, correct: newCorrect };
}

// ============================================================
// SOUND
// ============================================================
let audioCtx = null;
function playTone(freq, dur=0.12, type='sine', vol=0.08) {
  try {
    if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.type = type; osc.frequency.value = freq;
    gain.gain.setValueAtTime(vol, audioCtx.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + dur);
    osc.connect(gain); gain.connect(audioCtx.destination);
    osc.start(); osc.stop(audioCtx.currentTime + dur);
  } catch(e) {}
}
function sfxCorrect() { playTone(660,0.1); setTimeout(()=>playTone(880,0.15),80); }
function sfxWrong() { playTone(200,0.2,'sawtooth'); }
function sfxCombo() { playTone(880,0.08); setTimeout(()=>playTone(1100,0.08),70); setTimeout(()=>playTone(1320,0.15),140); }
function sfxWin() { [523,659,784,1047].forEach((f,i)=>setTimeout(()=>playTone(f,0.15),i*100)); }

// ============================================================
// STATE
// ============================================================
const STORAGE_KEY = 'sqq_v3';
let state = JSON.parse(localStorage.getItem(STORAGE_KEY) || 'null') || {
  coins: 0, lives: 3, streak: 0, bestStreak: 0,
  score: 0, total: 0, completedMissions: []
};
function save() { localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

function updateHUD() {
  document.getElementById('coinCount').textContent = state.coins;
  document.getElementById('livesCount').textContent = '❤️'.repeat(Math.max(0,state.lives)) + '🖤'.repeat(Math.max(0,3-state.lives));
  document.getElementById('streakCount').textContent = state.streak;
  document.getElementById('scoreCount').textContent = state.score;
  document.getElementById('scoreTotal').textContent = state.total;
}
updateHUD();

function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

function floatFeedback(text, color) {
  const el = document.createElement('div');
  el.className = 'float-fb';
  el.textContent = text;
  el.style.color = color;
  el.style.left = (window.innerWidth/2 - 40) + 'px';
  el.style.top = '40%';
  document.body.appendChild(el);
  setTimeout(() => el.remove(), 1000);
}

// ============================================================
// MISSION DEFINITIONS
// ============================================================
const MISSIONS = [
  { id:'intro', icon:'📚', name:'Meet the 4 Types', tagline:'Знакомство' },
  { id:'rule',  icon:'🧠', name:'Build the Rule',    tagline:'Правило' },
  { id:'m3',    icon:'🔍', name:'Question Detective',tagline:'Определи тип' },
  { id:'m4',    icon:'🧩', name:'Word Chaos',        tagline:'Собери вопрос' },
  { id:'m5',    icon:'🟢', name:'Have got Builder',  tagline:'have got' },
  { id:'m6',    icon:'🟣', name:'Was/Were Builder',  tagline:'was / were' },
  { id:'m7',    icon:'🕵️', name:'Find the Mistake',  tagline:'Найди ошибку' },
  { id:'m8',    icon:'🔗', name:'Match Q↔A',         tagline:'Соедини пары' },
  { id:'m9',    icon:'⚔️', name:'Ask & Answer Duel', tagline:'Вопрос→ответ' },
  { id:'m10',   icon:'🐉', name:'Dragon Battle',     tagline:'Финальная битва' },
];

function renderMap() {
  const map = document.getElementById('map');
  map.innerHTML = '';
  MISSIONS.forEach((m, i) => {
    const done = state.completedMissions.includes(m.id);
    const unlocked = i === 0 || state.completedMissions.includes(MISSIONS[i-1].id);
    const card = document.createElement('div');
    card.className = 'mission-card ' + (done ? 'completed' : unlocked ? 'unlocked' : 'locked');
    card.innerHTML = `<div class="icon">${m.icon}</div><div class="name">${m.name}</div><div class="tagline">${m.tagline}</div>`;
    if (unlocked) card.onclick = () => startMission(m.id);
    else if (done) card.onclick = () => showModal('✅ Пройдено', `Миссия "${m.name}" уже выполнена!`, closeModal);
    map.appendChild(card);
  });
  if (state.completedMissions.length === MISSIONS.length) {
    const win = document.createElement('div');
    win.style.gridColumn = '1 / -1';
    win.style.textAlign = 'center';
    win.style.marginTop = '18px';
    win.innerHTML = `<h2 style="color:#ffd700;">🏆 ПОБЕДА! Question Master!</h2>
      <p style="color:#aaa;margin-top:6px;">Все 10 миссий пройдены • 🪙 ${state.coins} • 🎯 ${state.score}/${state.total}</p>`;
    map.appendChild(win);
  }
  showScreen('screen-map');
}

function startMission(id) {
  if (id === 'intro') startIntro();
  else if (id === 'rule') startRule();
  else if (id === 'm3') startQuiz(MISSION_M3);
  else if (id === 'm4') startChaos();
  else if (id === 'm5') startQuiz(MISSION_M5);
  else if (id === 'm6') startQuiz(MISSION_M6);
  else if (id === 'm7') startQuiz(MISSION_M7);
  else if (id === 'm8') startMatch();
  else if (id === 'm9') startQuiz(MISSION_M9);
  else if (id === 'm10') startQuiz(MISSION_M10);
}

function completeMission(id, bonusCoins=8) {
  if (!state.completedMissions.includes(id)) {
    state.completedMissions.push(id);
    state.coins += bonusCoins;
    save(); updateHUD();
    sfxWin();
  }
  renderMap();
}

// ============================================================
// MODAL
// ============================================================
let modalCb = null;
function showModal(title, text, cb) {
  document.getElementById('modalTitle').innerHTML = title;
  document.getElementById('modalText').innerHTML = text;
  modalCb = cb;
  document.getElementById('modal').style.display = 'flex';
}
function closeModal() {
  document.getElementById('modal').style.display = 'none';
  if (modalCb) { const f = modalCb; modalCb = null; f(); }
}
document.getElementById('modalBtn').onclick = closeModal;

// ============================================================
// MISSION 1: INTRO
// ============================================================
const INTRO_SLIDES = [
  {
    badge:'b-general', badgeText:'GENERAL — общий',
    title:'General Question',
    rule:'Начинается с <b>Have / Has / Was / Were</b>. Ответ: <b>Yes / No</b>.',
    examples:[
      { q:'Have you got a pet?', a:'Yes, I have. / No, I haven\'t.', why:'Have в начале → да/нет' },
      { q:'Was he at home yesterday?', a:'Yes, he was. / No, he wasn\'t.', why:'Was в начале → да/нет' },
    ],
    check:{ q:'Какой ответ к <i>Have they got a car?</i>', opts:['Yes, they have.','They have got a car.','Yes, they got.'], correct:0 }
  },
  {
    badge:'b-special', badgeText:'SPECIAL — специальный',
    title:'Special Question',
    rule:'Начинается с <b>What / Where / When / Who / Why / How</b>. Ответ — <b>полный</b>.',
    examples:[
      { q:'What have you got in your bag?', a:'I have got a book and a pen.', why:'What → о вещи' },
      { q:'Where were you yesterday?', a:'I was at school.', why:'Where → о месте' },
    ],
    check:{ q:'Какой ответ к <i>Why was she sad?</i>', opts:['Because she was ill.','Yes, she was.','She was sad or ill.'], correct:0 }
  },
  {
    badge:'b-alt', badgeText:'ALTERNATIVE — альтернативный',
    title:'Alternative Question',
    rule:'Всегда есть <b>or</b>. Ответ — <b>выбор</b>, без Yes/No.',
    examples:[
      { q:'Have you got a cat or a dog?', a:'I have got a dog.', why:'or → выбираем' },
      { q:'Were you at home or at school?', a:'I was at school.', why:'or → выбираем' },
    ],
    check:{ q:'Ответ к <i>Has she got a brother or a sister?</i>', opts:['She has got a brother.','Yes, she has.','No, she hasn\'t.'], correct:0 }
  },
  {
    badge:'b-tag', badgeText:'TAG — разделительный',
    title:'Tag Question',
    rule:'Утверждение + <b>«хвостик»</b>: haven\'t/hasn\'t/wasn\'t/weren\'t + pronoun?',
    examples:[
      { q:'You have got a brother, haven\'t you?', a:'Yes, I have.', why:'have → haven\'t you?' },
      { q:'They were at school, weren\'t they?', a:'Yes, they were.', why:'were → weren\'t they?' },
    ],
    check:{ q:'Хвостик: <i>He was at home, ___?</i>', opts:['wasn\'t he?','didn\'t he?','was he?'], correct:0 }
  },
];

let introIndex = 0;
function startIntro() { introIndex = 0; renderIntroSlide(); showScreen('screen-intro'); }

function renderIntroSlide() {
  const s = INTRO_SLIDES[introIndex];
  const box = document.getElementById('introSlideContent');
  const examplesHtml = s.examples.map(e => `
    <div class="example-box">
      <div class="q-en">${e.q}</div>
      <div class="answer">→ ${e.a}</div>
      <div class="why">${e.why}</div>
    </div>`).join('');
  // Перемешиваем варианты мини-проверки!
  const shuffled = shuffleWithCorrect(s.check);
  const checkOpts = shuffled.opts.map((o,i) => `<span class="mini-opt" data-i="${i}">${o}</span>`).join('');

  box.innerHTML = `
    <span class="type-badge ${s.badge}">${s.badgeText}</span>
    <h2>${s.title}</h2>
    <p style="color:#9a8fb8;font-size:0.92em;margin-bottom:6px;">${s.rule}</p>
    ${examplesHtml}
    <div class="mini-check">
      <p>🎯 Мини-проверка: ${shuffled.q}</p>
      <div>${checkOpts}</div>
    </div>
    <div class="nav-btns">
      <button class="btn btn-ghost" id="introPrev" ${introIndex===0?'disabled':''}>← Назад</button>
      <button class="btn btn-primary" id="introNext">${introIndex === INTRO_SLIDES.length-1 ? 'К миссии 2 →' : 'Далее →'}</button>
    </div>
  `;

  box.querySelectorAll('.mini-opt').forEach(opt => {
    opt.onclick = () => {
      const i = +opt.dataset.i;
      if (i === shuffled.correct) {
        opt.classList.add('ok'); sfxCorrect();
        state.coins += 1; updateHUD(); save();
      } else {
        opt.classList.add('bad'); sfxWrong();
        box.querySelector(`.mini-opt[data-i="${shuffled.correct}"]`).classList.add('ok');
      }
    };
  });

  document.getElementById('introPrev').onclick = () => { if (introIndex > 0) { introIndex--; renderIntroSlide(); } };
  document.getElementById('introNext').onclick = () => {
    if (introIndex < INTRO_SLIDES.length-1) { introIndex++; renderIntroSlide(); }
    else { completeMission('intro'); startMission('rule'); }
  };
}

// ============================================================
// MISSION 2: RULE
// ============================================================
const RULE_TASKS = [
  { type:'GENERAL', color:'#4ade80', pattern:['___','you','got','a pet?'], answer:'Have', chips:['Have','What','or','wasn\'t'] },
  { type:'SPECIAL', color:'#60a5fa', pattern:['___','were','you','yesterday?'], answer:'Where', chips:['Where','Have','or','wasn\'t'] },
  { type:'ALTERNATIVE', color:'#f472b6', pattern:['Have you got a cat','___','a dog?'], answer:'or', chips:['or','Have','What','wasn\'t'] },
  { type:'TAG', color:'#c084fc', pattern:['They were at school,','___','they?'], answer:'weren\'t', chips:['weren\'t','didn\'t','was','Have'] },
];

let ruleAnswers = {};
function startRule() { ruleAnswers = {}; renderRule(); showScreen('screen-rule'); }

function renderRule() {
  const box = document.getElementById('ruleBuilder');
  let html = `<h2 style="margin-bottom:14px;color:#ffd700;">🧠 Собери правило!</h2>
    <p style="color:#9a8fb8;margin-bottom:14px;font-size:0.92em;">Кликни на пропуск, затем выбери слово из банка.</p>`;

  RULE_TASKS.forEach((t, i) => {
    const slots = t.pattern.map(p => {
      if (p === '___') {
        const filled = ruleAnswers[i] || '';
        return `<span class="slot ${filled?'filled':''}" data-task="${i}">${filled || '?'}</span>`;
      }
      return `<span>${p}</span>`;
    }).join('');
    html += `<div class="rule-row" style="border-left-color:${t.color};">
      <div class="type" style="color:${t.color};">${t.type}</div>
      <div class="slots">${slots}</div>
    </div>`;
  });

  const allChips = shuffle([...new Set(RULE_TASKS.flatMap(t => t.chips))]);
  const usedChips = Object.values(ruleAnswers);
  html += `<div class="chip-pool">` +
    allChips.map(c => `<span class="chip ${usedChips.includes(c)?'used':''}" data-word="${c}">${c}</span>`).join('') +
    `</div>`;
  html += `<div class="nav-btns" style="margin-top:14px;"><button class="btn btn-primary" id="ruleCheck">Проверить ✓</button></div>`;

  box.innerHTML = html;

  box.querySelectorAll('.chip').forEach(chip => {
    chip.onclick = () => {
      const word = chip.dataset.word;
      for (let i = 0; i < RULE_TASKS.length; i++) {
        if (ruleAnswers[i] === undefined) {
          ruleAnswers[i] = word;
          renderRule(); return;
        }
      }
    };
  });
  box.querySelectorAll('.slot.filled').forEach(slot => {
    slot.onclick = () => { delete ruleAnswers[+slot.dataset.task]; renderRule(); };
  });

  document.getElementById('ruleCheck').onclick = () => {
    let allCorrect = RULE_TASKS.every((t,i) => ruleAnswers[i] === t.answer);
    if (allCorrect) {
      sfxWin();
      showModal('🏆 Правило собрано!', 'Ты сам сформулировал правило!<br><br>General → Have/Has/Was/Were<br>Special → What/Where/When…<br>Alternative → … or …<br>Tag → …, haven\'t you?', () => {
        closeModal(); state.coins += 10; save(); updateHUD();
        completeMission('rule'); startMission('m3');
      });
    } else {
      sfxWrong();
      showModal('🤔 Не совсем', 'Проверь ещё раз:<br>• General → Have/Has/Was/Were<br>• Special → What/Where/When…<br>• Alternative → or<br>• Tag → weren\'t they? (were → weren\'t)', closeModal);
    }
  };
}

// ============================================================
// QUIZ MISSIONS
// ============================================================
const MISSION_M3 = {
  id:'m3', title:'🔍 Question Detective', timer:18,
  questions:[
    { q:'Have you got a pet?', opts:['General','Special','Alternative','Tag'], correct:0, why:'Have → да/нет → General.' },
    { q:'Where were you yesterday?', opts:['General','Special','Alternative','Tag'], correct:1, why:'Where → Special.' },
    { q:'Has she got a cat or a dog?', opts:['General','Special','Alternative','Tag'], correct:2, why:'or → Alternative.' },
    { q:'You were at school, weren\'t you?', opts:['General','Special','Alternative','Tag'], correct:3, why:'Хвостик → Tag.' },
    { q:'What have you got in your bag?', opts:['General','Special','Alternative','Tag'], correct:1, why:'What → Special.' },
    { q:'Was he at home yesterday?', opts:['General','Special','Alternative','Tag'], correct:0, why:'Was → General.' },
    { q:'How many friends have you got?', opts:['General','Special','Alternative','Tag'], correct:1, why:'How many → Special.' },
    { q:'They were at home, weren\'t they?', opts:['General','Special','Alternative','Tag'], correct:3, why:'Tag.' },
    { q:'Have they got a car or a bike?', opts:['General','Special','Alternative','Tag'], correct:2, why:'or → Alternative.' },
    { q:'Who was at the door?', opts:['General','Special','Alternative','Tag'], correct:1, why:'Who → Special.' },
    { q:'Was she at home or at school?', opts:['General','Special','Alternative','Tag'], correct:2, why:'or → Alternative.' },
    { q:'You have got a bike, haven\'t you?', opts:['General','Special','Alternative','Tag'], correct:3, why:'haven\'t you → Tag.' },
  ]
};

const MISSION_M5 = {
  id:'m5', title:'🟢 Have got Builder', timer:18,
  questions:[
    { q:'«У тебя есть ручка?»', opts:['Have you got a pen?','You have got a pen?','Got you have a pen?','Have you a pen got?'], correct:0, why:'General: Have + you + got + …?' },
    { q:'«Что у неё в рюкзаке?»', opts:['What has she got in her backpack?','Has she got what in her backpack?','What she has got in her backpack?','Has she got in her backpack what?'], correct:0, why:'What + has + she + got?' },
    { q:'«У него есть велосипед или машина?»', opts:['Has he got a bike or a car?','Have he got a bike or a car?','He has got a bike or a car?','Got he has a bike or a car?'], correct:0, why:'he → has, or → Alternative.' },
    { q:'«У них есть собака, не так ли?»', opts:['They have got a dog, haven\'t they?','They have got a dog, have they?','Have they got a dog, they?','They got a dog, haven\'t they?'], correct:0, why:'they have → haven\'t they?' },
    { q:'«Сколько у тебя монет?»', opts:['How many coins have you got?','How many you have got coins?','Have you got how many coins?','How many coins got you have?'], correct:0, why:'How many + coins + have + you + got?' },
    { q:'«У неё есть кошка?»', opts:['Has she got a cat?','Have she got a cat?','She has got a cat?','Got she has a cat?'], correct:0, why:'she → has.' },
    { q:'«Что у тебя есть?»', opts:['What have you got?','What you have got?','Have you got what?','What got you have?'], correct:0, why:'What + have + you + got?' },
    { q:'«У них есть машина или велосипед?»', opts:['Have they got a car or a bike?','They have got a car or a bike?','Has they got a car or a bike?','Have they got a car and a bike?'], correct:0, why:'they → have, or → Alternative.' },
  ]
};

const MISSION_M6 = {
  id:'m6', title:'🟣 Was/Were Builder', timer:18,
  questions:[
    { q:'«Ты был дома вчера?»', opts:['Were you at home yesterday?','Was you at home yesterday?','You were at home yesterday?','At home you were yesterday?'], correct:0, why:'you → were.' },
    { q:'«Где ты был вчера?»', opts:['Where were you yesterday?','Where was you yesterday?','Where you were yesterday?','Were you where yesterday?'], correct:0, why:'Where + were + you?' },
    { q:'«Она была в школе или дома?»', opts:['Was she at school or at home?','Were she at school or at home?','She was at school or at home?','At school or at home was she?'], correct:0, why:'she → was, or → Alternative.' },
    { q:'«Они были в парке, не так ли?»', opts:['They were in the park, weren\'t they?','They were in the park, didn\'t they?','They was in the park, weren\'t they?','Were they in the park, they?'], correct:0, why:'they were → weren\'t they?' },
    { q:'«Почему ты был грустный?»', opts:['Why were you sad?','Why was you sad?','Why you were sad?','Were you why sad?'], correct:0, why:'Why + were + you?' },
    { q:'«Он был в школе вчера?»', opts:['Was he at school yesterday?','Were he at school yesterday?','He was at school yesterday?','At school he was yesterday?'], correct:0, why:'he → was.' },
    { q:'«Ты была в Минске, не так ли?»', opts:['You were in Minsk, weren\'t you?','You was in Minsk, weren\'t you?','Were you in Minsk, you?','You were in Minsk, didn\'t you?'], correct:0, why:'you were → weren\'t you?' },
    { q:'«Кто был дома?»', opts:['Who was at home?','Who were at home?','Who at home was?','Was who at home?'], correct:0, why:'Who + was (ед.ч.)?' },
  ]
};

const MISSION_M7 = {
  id:'m7', title:'🕵️ Find the Mistake', timer:20,
  questions:[
    { q:'Найди ошибку: <b>Was you at home?</b>', opts:['Were you at home?','Was you at the home?','Are you at home?','Is you at home?'], correct:0, why:'you → were, не was.' },
    { q:'Найди ошибку: <b>Have he got a bike?</b>', opts:['Has he got a bike?','Have he got the bike?','Does he have got a bike?','Has he a bike got?'], correct:0, why:'he → has, не have.' },
    { q:'Найди ошибку: <b>What you have got?</b>', opts:['What have you got?','What you got have?','Have what you got?','What have got you?'], correct:0, why:'В вопросе have перед you.' },
    { q:'Найди ошибку: <b>Yes, I were.</b>', opts:['Yes, I was.','Yes, I been.','Yes, I am.','Yes, I have.'], correct:0, why:'I → was.' },
    { q:'Найди ошибку: <b>Has they got a car?</b>', opts:['Have they got a car?','Has they get a car?','Do they have got a car?','Have they a car got?'], correct:0, why:'they → have.' },
    { q:'Найди ошибку: <b>Was they at school?</b>', opts:['Were they at school?','Was they at the school?','They was at school?','Did they was at school?'], correct:0, why:'they → were.' },
    { q:'Найди ошибку: <b>You have got a cat, didn\'t you?</b>', opts:['You have got a cat, haven\'t you?','You have got a cat, did you?','Have you got a cat, you?','You got a cat, haven\'t you?'], correct:0, why:'have → haven\'t you?' },
    { q:'Найди ошибку: <b>Why you were sad?</b>', opts:['Why were you sad?','Why you sad were?','Were you why sad?','Why was you sad?'], correct:0, why:'Why + were + you?' },
    { q:'Найди ошибку: <b>Were he at home?</b>', opts:['Was he at home?','Were he at the home?','Is he at home?','Was he the home?'], correct:0, why:'he → was.' },
    { q:'Найди ошибку: <b>Have you got a cat or a dog?</b> (это НЕ ошибка — но какой это тип?)', opts:['Ошибки нет — это Alternative','Ошибки нет — это Tag','Ошибки нет — это Special','Ошибки нет — это General'], correct:0, why:'or → Alternative. Ловушка!' },
  ]
};

const MISSION_M9 = {
  id:'m9', title:'⚔️ Ask & Answer Duel', timer:18,
  questions:[
    { q:'Have you got a pet?', opts:['Yes, I have. I\'ve got a dog.','Yes, I has got a dog.','Yes, I got a dog.','Yes, I am a dog.'], correct:0, why:'I → have.' },
    { q:'Were you at the seaside last summer?', opts:['Yes, I was.','Yes, I were.','Yes, I did.','Yes, I am.'], correct:0, why:'you → was в ответе.' },
    { q:'Has she got a brother or a sister?', opts:['She has got a brother.','Yes, she has.','No, she hasn\'t.','She got a brother.'], correct:0, why:'Alternative → выбор.' },
    { q:'Where were they yesterday?', opts:['They were at the park.','Yes, they were.','They was at the park.','At the park or at home.'], correct:0, why:'Special → полный ответ.' },
    { q:'You have got a bike, haven\'t you?', opts:['Yes, I have.','Yes, I got.','Yes, I am.','No, I don\'t.'], correct:0, why:'Tag → Yes, I have.' },
    { q:'Why was he sad?', opts:['Because he was ill.','Yes, he was sad.','He was sad or ill.','He were ill.'], correct:0, why:'Why → Because.' },
    { q:'Have they got a car?', opts:['Yes, they have.','Yes, they got.','Yes, they has.','They have got a car or a bike.'], correct:0, why:'they → have.' },
    { q:'Was she at home or at school?', opts:['She was at school.','Yes, she was.','No, she wasn\'t.','She were at school.'], correct:0, why:'Alternative → выбор.' },
    { q:'Who was at the door?', opts:['My friend was.','Yes, he was.','He was or she was.','Was my friend.'], correct:0, why:'Who → полный ответ с подлежащим.' },
    { q:'Have you got a cat or a dog?', opts:['I have got a cat.','Yes, I have.','No, I haven\'t.','I have got a cat and a dog.'], correct:0, why:'or → выбор.' },
  ]
};

const MISSION_M10 = {
  id:'m10', title:'🐉 Dragon Battle', timer:14, isBoss:true,
  questions:[
    { q:'Какой тип: <b>Have they got a car?</b>', opts:['General','Special','Alternative','Tag'], correct:0, why:'Have → General.' },
    { q:'Какой тип: <b>When were you at school?</b>', opts:['General','Special','Alternative','Tag'], correct:1, why:'When → Special.' },
    { q:'Собери: «У него есть велосипед?»', opts:['Has he got a bike?','Have he got a bike?','He has got a bike?','Got he has a bike?'], correct:0, why:'he → has.' },
    { q:'Хвостик: <i>You were at home, ___?</i>', opts:['weren\'t you?','wasn\'t you?','didn\'t you?','were you?'], correct:0, why:'you were → weren\'t you?' },
    { q:'Ответь: <b>Were you at home yesterday?</b>', opts:['Yes, I was.','Yes, I were.','Yes, I did.','Yes, I have.'], correct:0, why:'I → was.' },
    { q:'Какой тип: <b>Have you got a cat or a dog?</b>', opts:['General','Special','Alternative','Tag'], correct:2, why:'or → Alternative.' },
    { q:'Собери: «Что у тебя в рюкзаке?»', opts:['What have you got in your backpack?','What you have got in your backpack?','Have you got what in your backpack?','What got you have in your backpack?'], correct:0, why:'What + have + you + got?' },
    { q:'Ответь: <b>Has she got a pet?</b>', opts:['Yes, she has.','Yes, she have.','Yes, she got.','Yes, she is.'], correct:0, why:'she → has.' },
    { q:'Найди ошибку: <i>Was they at school?</i>', opts:['Were they at school?','Was they at the school?','They was at school?','Did they was at school?'], correct:0, why:'they → were.' },
    { q:'Найди ошибку: <i>What you have got?</i>', opts:['What have you got?','What you got have?','Have what you got?','What have got you?'], correct:0, why:'have перед you.' },
    { q:'Какой тип: <b>Who was at the door?</b>', opts:['General','Special','Alternative','Tag'], correct:1, why:'Who → Special.' },
    { q:'Хвостик: <i>She has got a cat, ___?</i>', opts:['hasn\'t she?','doesn\'t she?','has she?','isn\'t she?'], correct:0, why:'has → hasn\'t she?' },
  ]
};

let quiz = null, quizTimer = null, quizTimeLeft = 0;

function startQuiz(mission) {
  // Перемешиваем опции в каждом вопросе + перемешиваем порядок вопросов
  const shuffledQs = shuffle(mission.questions).map(q => shuffleWithCorrect(q));
  quiz = { ...mission, questions: shuffledQs, idx:0, answered:false };
  if (mission.isBoss) {
    quiz.bossHP = 10;
    document.getElementById('dragonArea').style.display = 'block';
    document.getElementById('dragonHPFill').style.width = '100%';
  } else {
    document.getElementById('dragonArea').style.display = 'none';
  }
  showScreen('screen-quiz');
  renderQuizQuestion();
}

function renderQuizQuestion() {
  const q = quiz.questions[quiz.idx];
  if (!q) return finishQuiz();
  quiz.answered = false;
  document.getElementById('progressFill').style.width = (quiz.idx / quiz.questions.length * 100) + '%';
  document.getElementById('questionText').innerHTML = `<div style="color:#a78bfa;font-size:0.72em;margin-bottom:6px;">${quiz.title} • ${quiz.idx+1}/${quiz.questions.length}</div>${q.q}`;
  const optsEl = document.getElementById('options');
  optsEl.innerHTML = '';
  q.opts.forEach((o, i) => {
    const b = document.createElement('button');
    b.className = 'option-btn';
    b.textContent = o;
    b.onclick = () => answerQuiz(i, q);
    optsEl.appendChild(b);
  });
  document.getElementById('feedback').textContent = '';
  document.getElementById('feedback').className = '';
  document.getElementById('nextBtn').style.display = 'none';
  startTimer(quiz.timer);
}

function startTimer(seconds) {
  clearInterval(quizTimer);
  quizTimeLeft = seconds;
  const bar = document.getElementById('timerBar');
  const txt = document.getElementById('timerText');
  bar.classList.remove('low');
  bar.style.width = '100%';
  txt.textContent = seconds;
  quizTimer = setInterval(() => {
    quizTimeLeft -= 0.1;
    bar.style.width = Math.max(0, quizTimeLeft / seconds * 100) + '%';
    txt.textContent = Math.max(0, Math.ceil(quizTimeLeft));
    if (quizTimeLeft <= seconds * 0.3) bar.classList.add('low');
    if (quizTimeLeft <= 0) {
      clearInterval(quizTimer);
      if (!quiz.answered) timeOutAnswer();
    }
  }, 100);
}

function timeOutAnswer() {
  quiz.answered = true;
  const q = quiz.questions[quiz.idx];
  document.querySelectorAll('#options .option-btn').forEach((b, i) => {
    b.disabled = true;
    if (i === q.correct) b.classList.add('correct');
  });
  const fb = document.getElementById('feedback');
  fb.innerHTML = `⏰ Время!<br><span style="font-size:0.85em;color:#aaa;font-weight:normal;">${q.why}</span>`;
  fb.className = 'fb-wrong';
  state.total++; state.lives = Math.max(0, state.lives - 1); state.streak = 0;
  updateHUD(); save(); sfxWrong();
  if (state.lives <= 0) return gameOver();
  document.getElementById('nextBtn').style.display = 'block';
}

function answerQuiz(sel, q) {
  if (quiz.answered) return;
  quiz.answered = true;
  clearInterval(quizTimer);
  const btns = document.querySelectorAll('#options .option-btn');
  btns.forEach((b, i) => {
    b.disabled = true;
    if (i === q.correct) b.classList.add('correct');
    if (i === sel && sel !== q.correct) b.classList.add('wrong');
  });
  state.total++;
  const fb = document.getElementById('feedback');
  if (sel === q.correct) {
    state.streak++;
    state.bestStreak = Math.max(state.bestStreak, state.streak);
    const points = 2 + Math.floor(state.streak / 2);
    state.coins += points; state.score++;
    sfxCorrect();
    if (state.streak >= 3) { sfxCombo(); floatFeedback(`🔥 x${state.streak}!`, '#ffd700'); }
    else floatFeedback(`+${points}`, '#4ade80');
    fb.innerHTML = `✅ +${points} 🪙 (стрик x${state.streak})<br><span style="font-size:0.85em;color:#aaa;font-weight:normal;">${q.why}</span>`;
    fb.className = 'fb-correct';
    if (quiz.isBoss && quiz.bossHP > 0) {
      quiz.bossHP--;
      document.getElementById('dragonHPFill').style.width = (quiz.bossHP/10*100) + '%';
      const d = document.getElementById('dragonEmoji');
      d.classList.add('hit'); setTimeout(() => d.classList.remove('hit'), 400);
    }
  } else {
    state.streak = 0; state.lives = Math.max(0, state.lives - 1);
    sfxWrong(); floatFeedback('❌', '#ff6b6b');
    fb.innerHTML = `❌ Неправильно.<br><span style="font-size:0.85em;color:#aaa;font-weight:normal;">${q.why}</span>`;
    fb.className = 'fb-wrong';
  }
  updateHUD(); save();
  if (state.lives <= 0) return gameOver();
  if (quiz.isBoss && quiz.bossHP <= 0) return setTimeout(() => finishQuiz(), 800);
  document.getElementById('nextBtn').style.display = 'block';
}

document.getElementById('nextBtn').onclick = () => {
  quiz.idx++;
  if (quiz.idx >= quiz.questions.length) finishQuiz();
  else renderQuizQuestion();
};
document.getElementById('quitBtn').onclick = () => { clearInterval(quizTimer); renderMap(); };

function finishQuiz() {
  clearInterval(quizTimer);
  const id = quiz.id;
  state.coins += quiz.isBoss ? 25 : 8; save(); updateHUD();
  completeMission(id);
  if (quiz.isBoss) sfxWin();
  showModal(quiz.isBoss ? '🐉 ДРАКОН ПОБЕЖДЁН!' : '🎉 Миссия пройдена!',
    `Отлично!<br><br>🪙 Монет: <b>${state.coins}</b><br>🎯 Правильно: <b>${state.score}/${state.total}</b><br>🔥 Лучший стрик: <b>x${state.bestStreak}</b>`,
    () => { closeModal(); renderMap(); });
}

function gameOver() {
  clearInterval(quizTimer);
  showModal('💔 Жизни закончились', `Попробуй ещё раз!<br><br>🎯 Правильно: ${state.score}/${state.total}`,
    () => { closeModal(); state.lives = 3; state.streak = 0; save(); updateHUD(); renderMap(); });
}

// ============================================================
// MISSION 4: WORD CHAOS
// ============================================================
const CHAOS_TASKS = [
  { prompt:'«У тебя есть собака?» (General)', words:['Have','you','got','a','dog?'], hint:'Have/Has + subject + got + …?' },
  { prompt:'«Что у тебя в сумке?» (Special)', words:['What','have','you','got','in','your','bag?'], hint:'What + have/has + subject + got + …?' },
  { prompt:'«У неё есть кошка или собака?» (Alternative)', words:['Has','she','got','a','cat','or','a','dog?'], hint:'Has + she + got + A or B?' },
  { prompt:'«Ты был дома вчера?» (General)', words:['Were','you','at','home','yesterday?'], hint:'Was/Were + subject + …?' },
  { prompt:'«Они были в парке, не так ли?» (Tag)', words:['They','were','in','the','park,','weren\'t','they?'], hint:'Утверждение + хвостик' },
  { prompt:'«Почему она была грустная?» (Special)', words:['Why','was','she','sad?'], hint:'Why + was/were + subject + …?' },
  { prompt:'«У него есть велосипед?» (General)', words:['Has','he','got','a','bike?'], hint:'he → has' },
  { prompt:'«Где ты был вчера?» (Special)', words:['Where','were','you','yesterday?'], hint:'Where + were + you?' },
  { prompt:'«Ты был в Минске или в Гомеле?» (Alternative)', words:['Were','you','in','Minsk','or','in','Gomel?'], hint:'Were + you + A or B?' },
  { prompt:'«Он был в школе, не так ли?» (Tag)', words:['He','was','at','school,','wasn\'t','he?'], hint:'he was → wasn\'t he?' },
];

let chaos = null;
function startChaos() { chaos = { idx:0, placed:[], order: shuffle(CHAOS_TASKS.map((_,i)=>i)) }; renderChaos(); showScreen('screen-chaos'); }

function renderChaos() {
  const taskIdx = chaos.order[chaos.idx];
  const t = CHAOS_TASKS[taskIdx];
  if (!t) return finishChaos();

  document.getElementById('chaosProgress').style.width = (chaos.idx / CHAOS_TASKS.length * 100) + '%';
  document.getElementById('chaosTask').textContent = `🧩 ${chaos.idx+1}/${CHAOS_TASKS.length}`;
  document.getElementById('chaosInstruction').innerHTML = `${t.prompt}<br><span style="font-size:0.82em;color:#888;">Подсказка: ${t.hint}</span>`;

  const sZone = document.getElementById('sentenceZone');
  if (chaos.placed.length === 0) sZone.innerHTML = '<span class="placeholder">Клик по словам ниже…</span>';
  else {
    sZone.innerHTML = chaos.placed.map((w,i) => `<span class="word-chip placed" data-idx="${i}">${w}</span>`).join('');
    sZone.querySelectorAll('.placed').forEach(el => {
      el.onclick = () => { chaos.placed.splice(+el.dataset.idx, 1); renderChaos(); };
    });
  }

  // Показываем слова в перемешанном порядке, скрывая уже использованные
  const available = t.words.slice();
  chaos.placed.forEach(p => {
    const idx = available.indexOf(p);
    if (idx !== -1) available.splice(idx, 1);
  });
  const wZone = document.getElementById('wordChaosZone');
  wZone.innerHTML = '';
  shuffle(available).forEach(w => {
    const c = document.createElement('span');
    c.className = 'word-chip';
    c.textContent = w;
    c.onclick = () => { chaos.placed.push(w); renderChaos(); };
    wZone.appendChild(c);
  });

  document.getElementById('chaosFeedback').textContent = '';
  document.getElementById('chaosCheckBtn').onclick = checkChaos;
}

function checkChaos() {
  const taskIdx = chaos.order[chaos.idx];
  const t = CHAOS_TASKS[taskIdx];
  const user = chaos.placed.join(' ');
  const correct = t.words.join(' ');
  const fb = document.getElementById('chaosFeedback');
  if (user === correct) {
    fb.textContent = '✅ Правильно! +3 🪙';
    fb.style.color = '#4ade80';
    sfxCorrect();
    state.coins += 3; state.score++; state.total++;
    state.streak++; state.bestStreak = Math.max(state.bestStreak, state.streak);
    updateHUD(); save();
    setTimeout(() => {
      chaos.idx++; chaos.placed = [];
      if (chaos.idx >= CHAOS_TASKS.length) finishChaos();
      else renderChaos();
    }, 900);
  } else {
    fb.textContent = '❌ Не так. Попробуй ещё!';
    fb.style.color = '#ff6b6b';
    sfxWrong();
    state.lives = Math.max(0, state.lives - 1); state.streak = 0;
    updateHUD(); save();
    if (state.lives <= 0) gameOver();
  }
}

function finishChaos() {
  state.coins += 8; save(); updateHUD();
  completeMission('chaos'); // legacy id — но у нас m4
  completeMission('m4');
  showModal('🎉 Word Chaos пройден!', `Все вопросы собраны!<br><br>🪙 Монет: <b>${state.coins}</b>`, () => {
    closeModal(); renderMap();
  });
}
document.getElementById('chaosQuitBtn').onclick = () => renderMap();

// ============================================================
// MISSION 8: MATCH Q↔A
// ============================================================
const MATCH_PAIRS = [
  { q:'Have you got a pet?', a:'Yes, I have. I\'ve got a cat.' },
  { q:'Where were you yesterday?', a:'I was at school.' },
  { q:'Has she got a brother or a sister?', a:'She has got a brother.' },
  { q:'You were at home, weren\'t you?', a:'Yes, I was.' },
  { q:'Why was he sad?', a:'Because he was ill.' },
  { q:'How many friends have you got?', a:'I have got five friends.' },
  { q:'Were they at the park?', a:'No, they weren\'t.' },
  { q:'Who was at the door?', a:'My friend was.' },
];

let match = null;
function startMatch() {
  const pairs = shuffle(MATCH_PAIRS).slice(0, 6);
  match = {
    pairs,
    left: shuffle(pairs.map((p,i) => ({ text:p.q, id:i }))),
    right: shuffle(pairs.map((p,i) => ({ text:p.a, id:i }))),
    selectedLeft: null, selectedRight: null,
    matched: 0, total: pairs.length
  };
  renderMatch();
  showScreen('screen-match');
}

function renderMatch() {
  document.getElementById('matchTask').textContent = `🔗 ${match.matched}/${match.total}`;
  document.getElementById('matchProgress').style.width = (match.matched / match.total * 100) + '%';

  const leftEl = document.getElementById('matchLeft');
  const rightEl = document.getElementById('matchRight');
  leftEl.innerHTML = '';
  rightEl.innerHTML = '';

  match.left.forEach(item => {
    const el = document.createElement('div');
    el.className = 'match-item' + (item.matched ? ' matched' : '') + (match.selectedLeft === item ? ' selected' : '');
    el.textContent = item.text;
    if (!item.matched) el.onclick = () => { match.selectedLeft = item; renderMatch(); tryMatch(); };
    leftEl.appendChild(el);
  });
  match.right.forEach(item => {
    const el = document.createElement('div');
    el.className = 'match-item' + (item.matched ? ' matched' : '') + (match.selectedRight === item ? ' selected' : '');
    el.textContent = item.text;
    if (!item.matched) el.onclick = () => { match.selectedRight = item; renderMatch(); tryMatch(); };
    rightEl.appendChild(el);
  });
}

function tryMatch() {
  if (!match.selectedLeft || !match.selectedRight) return;
  const L = match.selectedLeft, R = match.selectedRight;
  const fb = document.getElementById('matchFeedback');
  if (L.id === R.id) {
    L.matched = true; R.matched = true;
    match.matched++;
    state.coins += 2; state.score++; state.total++;
    state.streak++; state.bestStreak = Math.max(state.bestStreak, state.streak);
    sfxCorrect();
    fb.textContent = '✅ Пара найдена! +2 🪙';
    fb.style.color = '#4ade80';
    updateHUD(); save();
    match.selectedLeft = null; match.selectedRight = null;
    setTimeout(() => {
      if (match.matched >= match.total) finishMatch();
      else renderMatch();
    }, 600);
  } else {
    sfxWrong();
    state.lives = Math.max(0, state.lives - 1);
    state.streak = 0; state.total++;
    fb.textContent = '❌ Не та пара. −❤️';
    fb.style.color = '#ff6b6b';
    updateHUD(); save();
    const Lidx = match.left.findIndex(x => x === L);
    const Ridx = match.right.findIndex(x => x === R);
    const leftEl = document.getElementById('matchLeft').children[Lidx];
    const rightEl = document.getElementById('matchRight').children[Ridx];
    leftEl.classList.add('wrong-flash');
    rightEl.classList.add('wrong-flash');
    match.selectedLeft = null; match.selectedRight = null;
    setTimeout(() => { renderMatch(); }, 500);
    if (state.lives <= 0) gameOver();
  }
}

function finishMatch() {
  state.coins += 8; save(); updateHUD();
  completeMission('m8');
  showModal('🎉 Match пройден!', `Все пары найдены!<br><br>🪙 Монет: <b>${state.coins}</b>`, () => {
    closeModal(); renderMap();
  });
}
document.getElementById('matchQuitBtn').onclick = () => renderMap();

// ============================================================
// RESET
// ============================================================
document.querySelector('h1').ondblclick = () => {
  if (confirm('Сбросить прогресс?')) { localStorage.removeItem(STORAGE_KEY); location.reload(); }
};

// INIT
renderMap();
</script>
</body>
</html>
