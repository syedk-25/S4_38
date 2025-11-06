<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>PhishScan • Friendly Anti-Phishing</title>
<style>
  :root{
    --bg:#f6fbff; --card:#ffffff; --muted:#6b7280;
    --accent1:#7dd3fc; --accent2:#c7b8ff; --accent3:#fde68a;
    --safe:#16a34a; --warn:#f59e0b; --danger:#ef4444;
    --glass: rgba(2,6,23,0.04);
  }
  *{box-sizing:border-box;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;color:#0b1220}
  html,body{height:100%;margin:0;background:linear-gradient(180deg,var(--bg),#eaf6ff);-webkit-font-smoothing:antialiased}
  .wrap{max-width:980px;margin:28px auto;padding:18px;}
  header{display:flex;gap:16px;align-items:center}
  .logo{width:64px;height:64px;border-radius:12px;background:linear-gradient(135deg,var(--accent1),var(--accent2));display:flex;align-items:center;justify-content:center;font-weight:800;font-size:20px;color:#04345a;box-shadow:0 8px 30px rgba(2,6,23,0.06)}
  .title h1{margin:0;font-size:20px}
  .title p{margin:0;color:var(--muted);font-size:13px}
  /* GREETING PAGE */
  .greet-card{background:var(--card);padding:20px;border-radius:14px;box-shadow:0 8px 40px rgba(2,6,23,0.05);margin-top:18px;display:flex;gap:14px;align-items:center}
  .greet-left{flex:1}
  .greet-right{min-width:200px;text-align:center}
  .btn{background:linear-gradient(90deg,var(--accent1),var(--accent2));padding:10px 14px;border-radius:10px;border:none;color:#04202c;font-weight:700;cursor:pointer;box-shadow:0 10px 30px rgba(3,105,161,0.08)}
  .btn.secondary{background:transparent;border:1px solid rgba(3,12,23,0.06);color:var(--accent2);font-weight:700}
  main{display:grid;grid-template-columns:1fr 360px;gap:18px;margin-top:16px}
  .card{background:var(--card);padding:16px;border-radius:12px;box-shadow:0 8px 24px rgba(2,6,23,0.04)}
  .input-row{display:flex;gap:8px;margin-top:10px}
  input[type="text"]{flex:1;padding:12px;border-radius:10px;border:1px solid rgba(2,6,23,0.06);background:transparent;color:inherit}
  .small{font-size:13px;color:var(--muted);margin-top:8px}
  .result-panel{height:220px;border-radius:10px;padding:12px;display:flex;flex-direction:column;gap:8px;align-items:stretch;justify-content:center;text-align:center;position:relative}
  .score{font-size:44px;font-weight:800;margin:0;color:#05202a}
  .verdict{font-weight:800}
  .reasons{font-size:13px;color:var(--muted);text-align:left;overflow:auto;max-height:120px;padding:6px}
  .logs{max-height:360px;overflow:auto;padding:6px}
  .log-item{padding:8px;border-radius:8px;margin-bottom:8px;background:linear-gradient(180deg, rgba(2,6,23,0.02), transparent);display:flex;justify-content:space-between;gap:8px}
  footer{margin-top:18px;text-align:center;color:var(--muted);font-size:13px}
  video{width:100%;border-radius:8px;background:#f0fbff;border:1px solid rgba(2,6,23,0.03)}
  .muted-small{color:var(--muted);font-size:12px}
  .badge{display:inline-block;padding:6px 10px;border-radius:999px;background:rgba(3,12,23,0.03);font-weight:700;color:#053a4f}
  .sensitive-note{font-size:13px;color:#0f1724;background:linear-gradient(90deg, rgba(253,230,138,0.25), transparent);padding:8px;border-radius:8px;margin-top:8px}
  #confettiCanvas{position:fixed;left:0;top:0;width:100%;height:100%;pointer-events:none;z-index:9999}
  @media (max-width:900px){main{grid-template-columns:1fr}}
</style>
</head>
<body>
<canvas id="confettiCanvas"></canvas>
<div class="wrap" id="app">
  <header>
    <div class="logo">PS</div>
    <div class="title">
      <h1>PhishScan</h1>
      <p>Friendly anti-phishing — lightweight, local, and sensitive</p>
    </div>
    <div style="margin-left:auto" class="badge">Welcome, Kulsum 👋</div>
  </header>

  <!-- GREETING PAGE -->
  <section id="greetingPage" class="greet-card">
    <div class="greet-left">
      <h2 style="margin:0 0 8px 0;color:#08394a">Great job being cautious — we appreciate your awareness!</h2>
      <p style="margin:0;color:var(--muted)">Phishing is tricky. This tool helps you scan links & QR codes, gives a sensitive threat score and human-friendly reasons, and keeps training tips based on your scans. We err on the side of caution: even small suspicious signs will be treated seriously.</p>
      <div class="small" style="margin-top:12px">This app runs fully in your browser and stores logs only on your device. You can export or clear them anytime.</div>
      <div class="sensitive-note">
        <strong>Strict mode active:</strong> any sign of suspicious behavior (keywords, IP hosts, missing HTTPS, obfuscated characters, etc.) will mark a link as <em>Highly suspicious</em>. You can change this behavior later in settings.
      </div>
    </div>
    <div class="greet-right">
      <img src="data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='120' height='120'><rect rx='20' width='120' height='120' fill='%23c7b8ff'/><g transform='translate(12 18)' fill='%23042b3a'><path d='M24 0c-6.6 0-12 5.4-12 12v6H0v10h12v26h12V28h12V18H24v-6c0-3.3 2.7-6 6-6 0 0 0 0 0 0V0z'/></g></svg>" alt="shield" style="max-width:100%"/>
      <div style="margin-top:12px">
        <button class="btn" id="continueBtn">Thanks — take me to scanner</button>
      </div>
    </div>
  </section>

  <!-- ACTION PAGE (hidden until continue) -->
  <main id="actionPage" style="display:none">
    <section class="card">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <h3 style="margin:0">Scan or Enter URL</h3>
        <div class="muted-small">Paste a URL or scan a QR</div>
      </div>

      <div style="margin-top:12px" class="input-row">
        <input id="urlInput" type="text" placeholder="Paste URL here (e.g. example.com/path)" />
        <button class="btn" id="analyzeBtn">Analyze</button>
      </div>

      <div style="margin-top:10px;display:flex;gap:8px;align-items:center">
        <input type="file" id="qrFile" accept="image/*" />
        <button class="btn secondary" id="startScanBtn">Start Camera Scan</button>
        <button class="btn secondary" id="stopCameraBtn" style="display:none">Stop Camera</button>
      </div>

      <div style="margin-top:14px" class="card">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div style="font-weight:700">Live QR Scanner</div>
          <div class="muted-small">Camera may ask permission</div>
        </div>
        <video id="video" playsinline></video>
        <div style="display:flex;gap:8px;justify-content:space-between;margin-top:6px">
          <div class="muted-small">Detected: <span id="detectedText">—</span></div>
          <div><button class="btn secondary" id="useDetectedBtn">Use Detected</button></div>
        </div>
      </div>

      <div style="margin-top:14px" id="result" class="card result-panel">
        <div id="visual" style="transition:all .3s;padding:10px;border-radius:10px;background:linear-gradient(90deg,#f0fff7,transparent)">
          <div class="muted-small">Threat score (0–100)</div>
          <div class="score" id="scoreVal">—</div>
          <div class="verdict" id="verdictText" style="color:#05344a">No analysis yet</div>
        </div>
        <div class="reasons" id="reasonBox">Paste a URL or scan a QR to get a human-friendly explanation of why the scanner gave this score, plus recommended precautions.</div>
      </div>
    </section>

    <aside>
      <div class="card">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <h3 style="margin:0">Logs</h3>
          <div class="muted-small" id="logCount">0 saved</div>
        </div>
        <div class="logs" id="logsContainer" style="margin-top:8px"></div>
        <div style="display:flex;gap:8px;margin-top:8px">
          <button class="btn" id="exportLogs">Export Logs</button>
          <button class="btn secondary" id="clearLogs">Clear</button>
        </div>
      </div>

      <div class="card" style="margin-top:12px">
        <h3 style="margin:0">Awareness Training</h3>
        <div class="small" style="margin-top:6px">Tips adapt to your logs to build bite-sized awareness.</div>
        <div style="margin-top:10px" id="trainingBox" class="training">
          <div class="tip" id="trainingTip">No logs yet. Scan some URLs and we'll tailor tips for you.</div>
        </div>
      </div>

      <div class="card" style="margin-top:12px">
        <h3 style="margin:0">Quick Checklist</h3>
        <ul style="margin:8px 0 0 18px;color:var(--muted)">
          <li>Check domain spelling and sender identity</li>
          <li>Prefer saved/bookmarked domains</li>
          <li>Never enter OTPs or passwords after following a link</li>
          <li>Verify by calling the sender if in doubt</li>
        </ul>
      </div>
    </aside>
  </main>

  <footer class="muted-small">Made with care • Runs locally in your browser. Not an enterprise replacement.</footer>
</div>

<script>
/* ----------------------------
   LocalStorage & UI helpers
   ---------------------------- */
const logsKey = 'phishscan_logs_v2';
function loadLogs(){try{return JSON.parse(localStorage.getItem(logsKey)||'[]')}catch(e){return []}}
function saveLog(entry){ const logs = loadLogs(); logs.unshift(entry); localStorage.setItem(logsKey, JSON.stringify(logs.slice(0,300))); renderLogs(); renderTraining(); }
function clearLogsLocal(){ localStorage.removeItem(logsKey); renderLogs(); renderTraining(); }

function escapeHtml(s){ return String(s||'').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c])); }

/* ----------------------------
   Analysis (stricter by default)
   - Runs only in browser, no network calls
   - Very sensitive: any suspicious flag escalates to Highly suspicious
   ---------------------------- */
function analyzeURLString(raw){
  const input = (raw||'').trim();
  if(!input) return {score:0,reasons:['No URL provided'],precautions:['Please provide a URL or scan a QR.'],url:input};

  const reasons = [];
  let score = 0;
  let strictTriggers = [];

  // add scheme if missing for parsing convenience (note we don't trust it)
  let working = input;
  if(!/^https?:\/\//i.test(working)){ working = 'https://' + working; reasons.push('No scheme provided; assumed https for analysis.'); score += 2; }

  try {
    const u = new URL(working);
    const host = u.hostname.toLowerCase();
    const path = (u.pathname||'') + (u.search||'') + (u.hash||'');

    // length
    if(working.length > 100){ score += 18; reasons.push('Very long URL — often used to conceal malicious parts.'); strictTriggers.push('long_url'); }
    else if(working.length > 70){ score += 8; reasons.push('Long URL — inspect carefully.'); }

    // IP address hosts
    if(/^\d{1,3}(\.\d{1,3}){3}$/.test(host) || /^\[?[a-f0-9:]+\]?$/i.test(host) && host.indexOf(':')>-1){
      score += 28; reasons.push('Direct IP address used instead of domain — commonly used by attackers.'); strictTriggers.push('ip_host');
    }

    // @ symbol
    if(/@/.test(working)){ score += 25; reasons.push('URL contains "@" — can mask real destination.'); strictTriggers.push('at_symbol'); }

    // suspicious keywords (credential/company lookalikes)
    const suspiciousWords = ['secure','account','update','verify','login','bank','confirm','ebay','paypal','amazon','invoice','reset','webscr','signin','confirm','security'];
    const found = suspiciousWords.filter(w => working.toLowerCase().includes(w));
    if(found.length > 0){ score += Math.min(40, found.length * 12); reasons.push('Contains credential/urgent words: ' + found.join(', ') + '.'); strictTriggers.push('keywords'); }

    // risky TLDs
    const riskyTlds = ['.xyz','.top','.club','.review','.bid','.work','.download','.loan','.win','.gq'];
    riskyTlds.forEach(t => { if(host.endsWith(t)){ score += 10; reasons.push('Uses uncommon TLD: ' + t); strictTriggers.push('risky_tld'); } });

    // many hyphens
    const hyphenCount = (host.match(/-/g)||[]).length;
    if(hyphenCount >= 2){ score += 8; reasons.push('Multiple hyphens in domain — could be deceptive.'); strictTriggers.push('hyphens'); }

    // many subdomains
    const subCount = host.split('.').length - 2;
    if(subCount >= 3){ score += 10; reasons.push('Many subdomains — attackers sometimes hide malicious hosts behind subdomains.'); strictTriggers.push('subdomains'); }

    // missing https
    if(u.protocol !== 'https:'){ score += 22; reasons.push('Not using HTTPS — connection may be insecure.'); strictTriggers.push('no_https'); } else { reasons.push('Uses HTTPS — good, but HTTPS alone is not proof of safety.'); score = Math.max(0, score - 1); }

    // many encoded sequences
    if((path.match(/%[0-9A-Fa-f]{2}/g)||[]).length > 2){ score += 10; reasons.push('URL has many encoded characters — could be obfuscated.'); strictTriggers.push('encoded_chars'); }

    // suspicious punctuation
    if((path.match(/\./g)||[]).length > 3){ score += 6; reasons.push('Path contains many periods — may indicate redirects or nested services.'); strictTriggers.push('many_dots'); }

    // short random-looking hostname (no vowels)
    if(host.length <=6 && /[a-z0-9]{5,}/i.test(host) && !/[aeiou]/i.test(host)){
      score += 8; reasons.push('Short random-looking host — possible auto-generated domain.'); strictTriggers.push('random_host');
    }

    // clamp intermediate score
    score = Math.max(0, Math.min(100, Math.round(score)));

    // --- STRICT POLICY: if ANY strict trigger exists, escalate to HIGHLY SUSPICIOUS
    if(strictTriggers.length > 0){
      const bumpReasons = ['One or more suspicious signs detected: ' + strictTriggers.join(', ') + '.'];
      reasons.unshift(...bumpReasons);
      score = Math.max(score, 88); // escalate to highly suspicious range by default
    }

    // final human precautions
    const precautions = [];
    if(score <= 35){
      precautions.push('Looks low-risk under current checks. Still verify sender before sharing sensitive info.');
    } else if(score <= 75){
      precautions.push('Potentially malicious. Avoid entering credentials. Verify the sender by another channel.');
    } else {
      precautions.push('Highly suspicious. Do NOT open. If opened, run device scans and change important passwords.');
    }

    return {score, reasons, precautions, host, url:working, triggers: strictTriggers};
  } catch (err) {
    // parsing failed -> suspicious
    return {score:95, reasons:['Could not parse URL — malformed or intentionally obfuscated.'], precautions:['Do not open malformed links.'], url:working};
  }
}

/* ----------------------------
   UI - results, logs, confetti, sound
   ---------------------------- */
const scoreVal = document.getElementById('scoreVal');
const verdictText = document.getElementById('verdictText');
const reasonBox = document.getElementById('reasonBox');
const visual = document.getElementById('visual');
const logsContainer = document.getElementById('logsContainer');
const logCount = document.getElementById('logCount');
const trainingTip = document.getElementById('trainingTip');

function renderResult(obj){
  const s = obj.score;
  scoreVal.innerText = s;
  let verdict = 'Unknown';
  if(s <= 35){
    verdict = 'Safe';
    visual.style.background = 'linear-gradient(90deg,#ecfdf5,transparent)';
    verdictText.innerText = 'Safe — file likely safe';
    verdictText.style.color = '#064e3b';
    playHappyTone(); confettiBurst(80);
  } else if(s <= 75){
    verdict = 'Maybe malicious';
    visual.style.background = 'linear-gradient(90deg,#fff7ed,transparent)';
    verdictText.innerText = 'Maybe malicious — be careful';
    verdictText.style.color = '#92400e';
  } else {
    verdict = 'Highly suspicious';
    visual.style.background = 'linear-gradient(90deg,#fff1f2,transparent)';
    verdictText.innerText = 'Don’t open! Highly suspicious';
    verdictText.style.color = '#9f1239';
    playWarningTone();
  }

  reasonBox.innerHTML = '<strong>Why:</strong><br/>' + (obj.reasons||[]).map(r=>`<div>• ${escapeHtml(r)}</div>`).join('') + '<hr style="opacity:.08"/><strong>Precautions:</strong><br/>' + (obj.precautions||[]).map(p=>`<div>• ${escapeHtml(p)}</div>`).join('');
  saveLog({ id: Date.now(), time:new Date().toISOString(), input: obj.url || '', score: obj.score, verdict });
}

/* ----------------------------
   Logs & simple training tips
   ---------------------------- */
function renderLogs(){
  const logs = loadLogs();
  logCount.innerText = logs.length + ' saved';
  logsContainer.innerHTML = logs.map(l=>`<div class="log-item"><div style="flex:1"><div style="font-weight:700">${escapeHtml(l.input)}</div><div class="muted-small">${new Date(l.time).toLocaleString()}</div></div><div style="min-width:90px;text-align:right"><div style="font-weight:800">${l.score}</div><div class="muted-small">${l.verdict}</div></div></div>`).join('') || '<div class="muted-small">No logs yet</div>';
}
function renderTraining(){
  const logs = loadLogs();
  if(logs.length===0){ trainingTip.innerText = 'No logs yet. Scan some URLs and we\'ll tailor tips for you.'; return; }
  const recent = logs.slice(0,20);
  const high = recent.filter(r=>r.score>75).length;
  const maybe = recent.filter(r=>r.score>35 && r.score<=75).length;
  if(high >= Math.ceil(recent.length*0.2)){
    trainingTip.innerHTML = `<strong>Urgent:</strong> Several recent scans are high-risk. Actions: 1) Do not open these links. 2) Verify senders separately. 3) If opened — run device scans and change passwords.`;
  } else if(maybe > Math.ceil(recent.length*0.25)){
    trainingTip.innerHTML = `<strong>Heads up:</strong> A number of borderline links. Tip: open only in sandbox or VM, don't enter credentials.`;
  } else {
    trainingTip.innerHTML = `<strong>Nice!</strong> Most recent scans look low-risk. Keep checking senders and avoid sharing OTPs.`;
  }
}

/* ----------------------------
   Confetti & sounds
   ---------------------------- */
const confettiCanvas = document.getElementById('confettiCanvas');
const confettiCtx = confettiCanvas.getContext && confettiCanvas.getContext('2d');
function setupConfettiCanvas(){ confettiCanvas.width = window.innerWidth; confettiCanvas.height = window.innerHeight; }
setupConfettiCanvas(); window.addEventListener('resize', setupConfettiCanvas);
function confettiBurst(count=100){
  if(!confettiCtx) return;
  const pieces = [];
  const colors = ['#7dd3fc','#c7b8ff','#fde68a','#86efac','#fca5a5'];
  for(let i=0;i<count;i++){
    pieces.push({ x: Math.random()*confettiCanvas.width, y: -20 - Math.random()*200, vx: (Math.random()-0.5)*6, vy: 2 + Math.random()*6, r: 6 + Math.random()*8, rot: Math.random()*360, color: colors[Math.floor(Math.random()*colors.length)]});
  }
  let t=0;
  const anim = setInterval(()=>{
    t++;
    confettiCtx.clearRect(0,0,confettiCanvas.width,confettiCanvas.height);
    pieces.forEach(p=>{
      p.x += p.vx; p.y += p.vy; p.vy += 0.06; p.rot += 6;
      confettiCtx.save();
      confettiCtx.translate(p.x,p.y);
      confettiCtx.rotate(p.rot*Math.PI/180);
      confettiCtx.fillStyle = p.color;
      confettiCtx.fillRect(-p.r/2,-p.r/2,p.r,p.r*0.6);
      confettiCtx.restore();
    });
    if(t>140){ clearInterval(anim); confettiCtx.clearRect(0,0,confettiCanvas.width,confettiCanvas.height); }
  },16);
}
function playHappyTone(){
  try{ const ctx=new (window.AudioContext||window.webkitAudioContext)(); const o=ctx.createOscillator(); const g=ctx.createGain(); o.type='sine'; o.frequency.value=880; g.gain.value=0.001; o.connect(g); g.connect(ctx.destination); o.start(); g.gain.linearRampToValueAtTime(0.04, ctx.currentTime+0.02); g.gain.linearRampToValueAtTime(0.0, ctx.currentTime+0.32); o.stop(ctx.currentTime+0.35); }catch(e){}
}
function playWarningTone(){
  try{ const ctx=new (window.AudioContext||window.webkitAudioContext)(); const o=ctx.createOscillator(); const g=ctx.createGain(); o.type='sawtooth'; o.frequency.value=220; g.gain.value=0.001; o.connect(g); g.connect(ctx.destination); o.start(); g.gain.linearRampToValueAtTime(0.06, ctx.currentTime+0.02); g.gain.linearRampToValueAtTime(0.0, ctx.currentTime+0.5); o.stop(ctx.currentTime+0.6); }catch(e){}
}

/* ----------------------------
   QR Camera scanning (BarcodeDetector or jsQR)
   ---------------------------- */
const video = document.getElementById('video');
let streamRef = null;
let barcodeDetector = null;
const detectedText = document.getElementById('detectedText');
const useDetectedBtn = document.getElementById('useDetectedBtn');

async function startCamera(){
  stopCamera();
  try{
    const constraints = { video: { facingMode: 'environment', width: { ideal: 1280 }, height: { ideal: 720 } } };
    const s = await navigator.mediaDevices.getUserMedia(constraints);
    streamRef = s; video.srcObject = s; await video.play();
    document.getElementById('stopCameraBtn').style.display='inline-block';
    if('BarcodeDetector' in window){
      try{
        const supported = await BarcodeDetector.getSupportedFormats();
        barcodeDetector = new BarcodeDetector({formats: supported});
      }catch(e){ barcodeDetector = null; }
    } else { barcodeDetector = null; if(!window.jsQR) await loadScript('https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.min.js'); }
    scanLoop();
  }catch(err){
    alert('Camera unavailable: ' + (err.message||err));
  }
}
function stopCamera(){ if(streamRef){ streamRef.getTracks().forEach(t=>t.stop()); streamRef=null; video.pause(); video.srcObject=null; } document.getElementById('stopCameraBtn').style.display='none'; detectedText.innerText='—'; scanning=false; }
let scanning = false;
async function scanLoop(){
  scanning = true;
  const canvas = document.createElement('canvas'); const ctx = canvas.getContext('2d');
  while(scanning && streamRef){
    try{
      canvas.width = video.videoWidth; canvas.height = video.videoHeight;
      ctx.drawImage(video,0,0,canvas.width,canvas.height);
      let code = null;
      if(barcodeDetector && typeof barcodeDetector.detect === 'function'){
        try{ const barcodes = await barcodeDetector.detect(canvas); if(barcodes && barcodes.length) code = barcodes[0].rawValue || barcodes[0].rawText; }catch(e){}
      } else if(window.jsQR){
        const imd = ctx.getImageData(0,0,canvas.width,canvas.height);
        const qr = jsQR(imd.data, imd.width, imd.height);
        if(qr) code = qr.data;
      }
      if(code) detectedText.innerText = code;
    }catch(e){}
    await new Promise(r=>setTimeout(r, 300));
  }
}

/* ----------------------------
   Helpers & events
   ---------------------------- */
function loadScript(src){ return new Promise((res,rej)=>{ const s=document.createElement('script'); s.src=src; s.onload=res; s.onerror=rej; document.head.appendChild(s); }); }

document.getElementById('continueBtn').addEventListener('click', ()=>{
  document.getElementById('greetingPage').style.display='none';
  document.getElementById('actionPage').style.display='grid';
});

document.getElementById('analyzeBtn').addEventListener('click', ()=>{
  const raw = document.getElementById('urlInput').value.trim();
  if(!raw){ alert('Please paste a URL or scan a QR code first.'); return; }
  const out = analyzeURLString(raw);
  renderResult(out);
});

document.getElementById('startScanBtn').addEventListener('click', ()=> startCamera());
document.getElementById('stopCameraBtn').addEventListener('click', ()=> stopCamera());
useDetectedBtn.addEventListener('click', ()=>{
  const txt = detectedText.innerText;
  if(!txt || txt==='—') return alert('No QR detected yet.');
  document.getElementById('urlInput').value = txt;
  const out = analyzeURLString(txt);
  renderResult(out);
});

document.getElementById('qrFile').addEventListener('change', async (e)=>{
  const f = e.target.files[0]; if(!f) return;
  const img = new Image();
  img.onload = async ()=> {
    const c = document.createElement('canvas'); c.width=img.width; c.height=img.height;
    const cx = c.getContext('2d'); cx.drawImage(img,0,0);
    if(!window.jsQR) await loadScript('https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.min.js');
    const imd = cx.getImageData(0,0,c.width,c.height);
    const qr = jsQR(imd.data, imd.width, imd.height);
    if(qr){ detectedText.innerText = qr.data; document.getElementById('urlInput').value = qr.data; const out = analyzeURLString(qr.data); renderResult(out); }
    else alert('No QR code found in image.');
  };
  img.onerror = ()=> alert('Could not read image file.');
  img.src = URL.createObjectURL(f);
});

document.getElementById('clearLogs').addEventListener('click', ()=> { if(confirm('Clear all saved logs from this browser?')) clearLogsLocal(); });
document.getElementById('exportLogs').addEventListener('click', ()=>{
  const logs = loadLogs(); if(logs.length===0){ alert('No logs to export.'); return; }
  const blob = new Blob([JSON.stringify(logs,null,2)],{type:'application/json'}); const url = URL.createObjectURL(blob);
  const a=document.createElement('a'); a.href=url; a.download='phishscan-logs.json'; a.click(); URL.revokeObjectURL(url);
});

/* initial UI */
renderLogs(); renderTraining();
document.getElementById('urlInput').placeholder = 'e.g. https://secure-example.example.xyz/verify';
</script>
</body>
</html>
