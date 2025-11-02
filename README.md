<!doctype html>

<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1" />
  <title>Mohammed & Eman — Our Story</title>
  <meta name="theme-color" content="#6b21a8" />
  <style>
    /* Mobile-first simple styles (self-contained) */
    :root{
      --bg1:#6b21a8; /* purple */
      --bg2:#8b5cf6;
      --txt:#fff;
      --glass: rgba(255,255,255,0.08);
      --accent: #ff77dd;
    }
    html,body{height:100%;margin:0;font-family:system-ui, -apple-system, 'Segoe UI', Roboto, 'Noto Sans', 'Helvetica Neue', Arial;}
    body{background:linear-gradient(180deg,var(--bg1),var(--bg2));color:var(--txt);display:flex;align-items:center;justify-content:center;padding:20px}
    .container{width:100%;max-width:420px;border-radius:18px;overflow:hidden;box-shadow:0 10px 40px rgba(0,0,0,0.35);background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));}/* Password screen */
.pw-screen{min-height:600px;padding:28px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:18px}
.logo{font-weight:700;font-size:20px;letter-spacing:0.6px}
.card{width:100%;background:var(--glass);backdrop-filter: blur(6px);border-radius:14px;padding:14px}
input[type=password]{width:100%;padding:12px;border-radius:10px;border:1px solid rgba(255,255,255,0.08);background:transparent;color:var(--txt);font-size:16px}
button{width:100%;padding:12px;border-radius:10px;border:0;background:#00000022;color:var(--txt);font-weight:600}
.muted{opacity:0.9;font-size:13px}

/* Main story screen */
.story{padding:18px;display:none;flex-direction:column;align-items:center;gap:14px}
.photo{width:calc(100% - 36px);height:340px;border-radius:14px;background:#ffffff22;background-size:cover;background-position:center;box-shadow:0 8px 30px rgba(0,0,0,0.35)}
.names{font-size:20px;font-weight:700}
.quote{font-size:14px;opacity:0.95}
.counter{font-weight:800;font-size:34px;margin-top:6px}
.meta{font-size:13px;opacity:0.9}
.controls{display:flex;gap:10px;width:100%;align-items:center}
.btn-next{flex:1;padding:12px;border-radius:10px;border:0;background:linear-gradient(90deg,var(--accent),#ffcccc);color:#111;font-weight:700}
.small{font-size:12px;opacity:0.9}

/* responsive tweaks */
@media(min-width:520px){body{padding:40px}.container{max-width:420px}}

  </style>
</head>
<body>
  <div class="container">
    <!-- Password screen -->
    <div id="pwScreen" class="pw-screen">
      <div class="logo">Mohammed & Eman</div>
      <div class="card">
        <div class="muted">الحماية بكلمة سر — ادخل الرقم عشان تشوف القصة</div>
        <form id="pwForm" style="margin-top:10px;display:flex;flex-direction:column;gap:10px;">
          <input id="pwInput" type="password" inputmode="numeric" pattern="[0-9]*" placeholder="اكتب كلمة السر" autocomplete="one-time-code" />
          <button id="pwBtn" type="submit">Unlock</button>
          <div class="muted small">مثال: 2572005</div>
        </form>
      </div>
      <div class="muted small">2024 — Our Story</div>
    </div><!-- Story screen (hidden until unlocked) -->
<div id="storyScreen" class="story" role="main">
  <div id="photo" class="photo" aria-label="photo"></div>
  <div class="names">Mohammed & Eman</div>
  <div class="quote">We’ve been together for 💜 <span id="daysDisplay">0</span> days…</div>
  <div class="counter" id="daysCounter">0</div>
  <div class="meta">منذ 13 / 1 / 2024</div>

  <div class="controls">
    <button id="nextBtn" class="btn-next">Next 💕</button>
  </div>

  <div style="height:18px"></div>
</div>

  </div>  <!-- Audio element (leave src empty for now) -->  <!-- To enable audio: replace AUDIO_SRC variable in the script below with a direct .mp3 URL or set src="yourfile.mp3" on this audio element --><audio id="bgAudio" preload="auto"></audio>

  <script>
    /* Configuration (edit these values if you edit the file locally) */
    const PASSWORD = '2572005';
    const START_DATE = new Date('2024-01-13T00:00:00'); // YYYY-MM-DD
    const IMAGE_FILENAME = 'photo.jpg'; // put your uploaded image next to index.html and keep this name
    const AUDIO_SRC = ''; // <-- optional: paste direct MP3 URL here to enable autoplay after unlock

    // Elements
    const pwScreen = document.getElementById('pwScreen');
    const storyScreen = document.getElementById('storyScreen');
    const pwForm = document.getElementById('pwForm');
    const pwInput = document.getElementById('pwInput');
    const daysDisplay = document.getElementById('daysDisplay');
    const daysCounter = document.getElementById('daysCounter');
    const photoEl = document.getElementById('photo');
    const audio = document.getElementById('bgAudio');

    // Put the image as background (expects file named photo.jpg in same folder)
    photoEl.style.backgroundImage = `url('${IMAGE_FILENAME}')`;

    // Password handling
    function unlock() {
      pwScreen.style.display = 'none';
      storyScreen.style.display = 'flex';
      sessionStorage.setItem('story_unlocked','1');
      // try to play audio if provided
      if (AUDIO_SRC) {
        audio.src = AUDIO_SRC;
        audio.loop = true;
        // autoplay may be blocked by browser — this attempts to play
        audio.play().catch(()=>{/* ignore autoplay block */});
      }
    }

    pwForm.addEventListener('submit', function(e){
      e.preventDefault();
      const val = pwInput.value.trim();
      if (val === PASSWORD) {
        unlock();
      } else {
        alert('كلمة السر خطأ');
        pwInput.value = '';
        pwInput.focus();
      }
    });

    // keep unlocked for session
    if (sessionStorage.getItem('story_unlocked') === '1') unlock();

    // Days counter
    function updateCounter(){
      const now = new Date();
      const diff = now - START_DATE;
      const days = Math.max(0, Math.floor(diff / (1000*60*60*24)));
      daysDisplay.textContent = days;
      daysCounter.textContent = days;
    }
    updateCounter();
    setInterval(updateCounter, 1000*60);

    // Next button (for future multi-photo support) — currently just a gentle animation
    document.getElementById('nextBtn').addEventListener('click', ()=>{
      // small pulse animation
      const el = document.getElementById('photo');
      el.animate([{transform:'scale(1)'},{transform:'scale(0.995)'},{transform:'scale(1)'}],{duration:240});
    });

    // Accessibility: focus password input on load
    window.addEventListener('load', ()=> pwInput.focus());

  </script></body>
</html>
