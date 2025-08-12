<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Veggie Cooking Timer — One Page</title>

  <!-- Tailwind (via CDN) -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Font -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">

  <style>
    :root { --max-width: 1100px; }
    html,body { height:100%; }
    body { font-family: 'Inter', ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background: #f7fafc; }
    .toast-enter { opacity: 0; transform: translateY(-6px); }
    .toast-leave { opacity: 0; transform: translateY(-6px); }
  </style>
</head>
<body class="min-h-screen flex items-center justify-center p-6">
  <div class="w-full max-w-[var(--max-width)] bg-white shadow-lg rounded-2xl overflow-hidden">
    <div class="md:flex">
      <!-- Left: selection -->
      <div class="md:w-1/2 p-6 border-r">
        <h1 class="text-2xl font-bold mb-2">Veggie Cooking Timer</h1>
        <p class="text-sm text-slate-500 mb-4">Select the vegetables you'll cook together — the app will tell you when to add each so they all finish at the same time.</p>

        <div id="veg-list" class="space-y-3">
          <!-- Rows inserted by JS -->
        </div>

        <div class="mt-4 flex items-center gap-3">
          <button id="add-custom-btn" class="px-3 py-2 bg-indigo-600 text-white rounded-md hover:bg-indigo-700">+ Add custom</button>
          <input id="custom-name" class="px-3 py-2 border rounded-md w-32" placeholder="Name (e.g. Pea)" />
          <input id="custom-min" type="number" min="0" class="px-3 py-2 border rounded-md w-24" placeholder="mins" />
        </div>

        <div class="mt-6 flex gap-3">
          <button id="start-btn" class="flex-1 px-4 py-3 bg-green-600 text-white rounded-md disabled:opacity-60" disabled>Start Cooking</button>
          <button id="stop-btn" class="px-4 py-3 bg-red-500 text-white rounded-md">Stop</button>
        </div>

        <div class="text-sm text-slate-500 mt-4">
          <strong>Tip:</strong> You can edit a vegetable's minutes inline by clicking the minutes number.
        </div>
      </div>

      <!-- Right: timer -->
      <div class="md:w-1/2 p-6">
        <div class="flex items-center justify-between mb-4">
          <div>
            <div class="text-sm text-slate-500">Overall remaining time</div>
            <div id="countdown" class="text-4xl font-extrabold mt-1">00:00</div>
          </div>
          <div class="text-right">
            <div class="text-sm text-slate-500">Status</div>
            <div id="status" class="text-lg font-semibold text-slate-700 mt-1">Idle</div>
          </div>
        </div>

        <div class="mb-4">
          <div class="flex gap-3">
            <button id="pause-btn" class="px-4 py-2 bg-yellow-500 text-white rounded-md" disabled>Pause</button>
            <button id="resume-btn" class="px-4 py-2 bg-blue-600 text-white rounded-md" disabled>Resume</button>
            <button id="reset-btn" class="px-4 py-2 bg-slate-200 rounded-md">Reset</button>
          </div>
        </div>

        <h3 class="font-semibold mt-4 mb-2">Upcoming steps</h3>
        <div id="steps" class="space-y-2 max-h-[40vh] overflow-auto">
          <!-- step rows -->
        </div>

        <div class="mt-6 text-sm text-slate-500">Notifications: will show when a step triggers (grant permission when asked).</div>
      </div>
    </div>
  </div>

  <!-- Toast container -->
  <div id="toasts" class="fixed top-6 right-6 w-80 pointer-events-none z-50"></div>

  <script type="module">
    // -------- CONFIG & DATA ----------
    const defaultVeggies = [
      { name: 'Potato', minutes: 12 },
      { name: 'Carrot', minutes: 8 },
      { name: 'Broccoli', minutes: 3 },
      { name: 'Green beans', minutes: 5 },
      { name: 'Onion', minutes: 10 },
      { name: 'Corn', minutes: 7 },
    ];

    // ------- STATE ----------
    let veggies = JSON.parse(localStorage.getItem('veggies_v1')) || defaultVeggies.slice();
    let selected = new Set();
    let schedule = []; // {name, durationSec, addAtSec, fired}
    let totalSec = 0;
    let remainingSec = 0;
    let timerId = null;
    let paused = false;
    let audioCtx = null;

    // ------- HELPERS ----------
    const $ = id => document.getElementById(id);
    function saveVeggies() { localStorage.setItem('veggies_v1', JSON.stringify(veggies)); }
    function fmtSS(s) {
      if (s < 0) s = 0;
      const mm = Math.floor(s / 60).toString().padStart(2,'0');
      const ss = (s % 60).toString().padStart(2,'0');
      return `${mm}:${ss}`;
    }
    function beep(duration = 0.18, freq = 880, vol = 0.25) {
      try {
        if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        const o = audioCtx.createOscillator();
        const g = audioCtx.createGain();
        o.type = 'sine';
        o.frequency.value = freq;
        g.gain.value = vol;
        o.connect(g);
        g.connect(audioCtx.destination);
        o.start();
        g.gain.exponentialRampToValueAtTime(0.0001, audioCtx.currentTime + duration);
        setTimeout(() => { o.stop(); }, duration * 1000 + 20);
      } catch (e) {
        // fallback: no sound
        console.warn('Audio failed', e);
      }
    }

    // Toasts (in-page messages)
    function toast(message, opts = {}) {
      const toasts = $('toasts');
      const el = document.createElement('div');
      el.className = 'bg-white shadow-md rounded-lg p-3 mb-2 pointer-events-auto transition transform';
      el.style.minWidth = '240px';
      el.innerHTML = `<div class="text-sm font-semibold">${escapeHtml(message)}</div>`;
      toasts.appendChild(el);
      setTimeout(() => el.classList.add('opacity-0','translate-y-[-6px]'), 3000);
      setTimeout(() => el.remove(), 3600);
    }
    function escapeHtml(str){ return String(str).replace(/[&<>"']/g, m=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":"&#39;"}[m])); }

    // System notification
    async function notify(title, body) {
      // in-page toast always
      toast(`${title} — ${body}`);
      // try system notification
      if (!('Notification' in window)) return;
      if (Notification.permission === 'granted') {
        try {
          new Notification(title, { body });
        } catch (e) {
          console.warn('Notification fail', e);
        }
      } else if (Notification.permission !== 'denied') {
        try {
          const perm = await Notification.requestPermission();
          if (perm === 'granted') {
            new Notification(title, { body });
          }
        } catch (e) { /* ignore */ }
      }
    }

    // ---------- UI Rendering ----------
    function renderVegList() {
      const container = $('veg-list');
      container.innerHTML = '';
      veggies.forEach((v, idx) => {
        const row = document.createElement('div');
        row.className = 'flex items-center gap-3 p-2 rounded-md hover:bg-slate-50';

        const cb = document.createElement('input');
        cb.type = 'checkbox';
        cb.checked = selected.has(idx);
        cb.className = 'h-4 w-4';
        cb.addEventListener('change', () => {
          if (cb.checked) selected.add(idx); else selected.delete(idx);
          updateStartButton();
        });

        const name = document.createElement('div');
        name.className = 'flex-1 text-sm';
        name.innerText = v.name;

        const minutes = document.createElement('div');
        minutes.className = 'px-3 py-1 border rounded-md text-sm cursor-pointer select-none';
        minutes.title = 'Click to edit minutes';
        minutes.innerText = `${v.minutes} min`;
        minutes.addEventListener('click', () => {
          const val = prompt('Set minutes for ' + v.name, String(v.minutes));
          if (val !== null) {
            const n = parseFloat(val);
            if (!isNaN(n) && n >= 0) {
              v.minutes = Math.max(0, Math.round(n));
              saveVeggies();
              renderVegList();
            } else {
              alert('Enter a valid non-negative number');
            }
          }
        });

        const edit = document.createElement('button');
        edit.className = 'text-xs text-indigo-600 px-2 py-1 rounded-md';
        edit.innerText = 'Edit';
        edit.addEventListener('click', () => {
          const newName = prompt('Rename vegetable', v.name);
          if (newName) { v.name = newName.trim(); saveVeggies(); renderVegList(); }
        });

        const del = document.createElement('button');
        del.className = 'text-xs text-red-500 px-2 py-1 rounded-md';
        del.innerText = 'Delete';
        del.addEventListener('click', () => {
          if (confirm('Delete ' + v.name + '?')) {
            veggies.splice(idx,1);
            saveVeggies();
            // rebuild selected indices set (shift indices)
            const newSet = new Set();
            selected.forEach(i => {
              if (i < idx) newSet.add(i);
              else if (i > idx) newSet.add(i-1);
            });
            selected = newSet;
            renderVegList(); updateStartButton();
          }
        });

        row.appendChild(cb);
        row.appendChild(name);
        row.appendChild(minutes);
        row.appendChild(edit);
        row.appendChild(del);
        container.appendChild(row);
      });
    }

    function renderSteps() {
      const container = $('steps');
      container.innerHTML = '';
      schedule.forEach((s, i) => {
        const row = document.createElement('div');
        row.className = 'flex items-center justify-between p-3 rounded-md border';
        const left = document.createElement('div');
        left.innerHTML = `<div class="font-medium">${escapeHtml(s.name)}</div><div class="text-xs text-slate-500">Cook ${Math.round(s.durationSec/60)} min — add at ${fmtSS(s.addAtSec)}</div>`;
        const right = document.createElement('div');
        right.className = 'text-right';
        const timeUntil = s.fired ? 'Added' : fmtSS(Math.max(0, s.addAtSec - (totalSec - remainingSec)));
        right.innerHTML = `<div class="text-sm ${s.fired ? 'text-green-600' : 'text-slate-700'}">${s.fired ? '✓' : timeUntil}</div>`;
        row.appendChild(left);
        row.appendChild(right);
        container.appendChild(row);
      });
    }

    function updateStartButton() {
      $('start-btn').disabled = selected.size === 0;
    }

    // ---------- Scheduling / Timer Logic ----------
    function computeScheduleFromSelected() {
      const selectedList = Array.from(selected).map(i => {
        const v = veggies[i];
        return { name: v.name, durationSec: Math.round((v.minutes || 0) * 60) };
      }).filter(x => x.durationSec > 0);

      if (selectedList.length === 0) return { tMax: 0, schedule: [] };
      const durations = selectedList.map(s => s.durationSec);
      const tMax = Math.max(...durations);
      const sched = selectedList.map(s => {
        return { name: s.name, durationSec: s.durationSec, addAtSec: tMax - s.durationSec, fired: false };
      }).sort((a,b) => a.addAtSec - b.addAtSec);
      return { tMax, schedule: sched };
    }

    function startTimer() {
      if (timerId) clearInterval(timerId);
      const cs = computeScheduleFromSelected();
      totalSec = cs.tMax;
      schedule = cs.schedule;
      if (totalSec <= 0) { alert('No valid durations selected (minutes must be > 0).'); return; }
      remainingSec = totalSec;
      paused = false;
      $('status').innerText = 'Cooking';
      $('pause-btn').disabled = false;
      $('resume-btn').disabled = true;
      renderSteps();
      // start ticking every 1s
      timerId = setInterval(tick, 1000);
      // show initial state
      updateCountdownUI();
      notify('Cooking started', `Total time ${fmtSS(totalSec)}`);
    }

    function tick() {
      if (paused) return;
      remainingSec = Math.max(0, remainingSec - 1);
      const elapsed = totalSec - remainingSec;
      // Fire steps where elapsed >= addAtSec and not yet fired
      schedule.forEach(step => {
        if (!step.fired && elapsed >= step.addAtSec) {
          step.fired = true;
          onStepTrigger(step);
        }
      });
      updateCountdownUI();
      renderSteps();
      if (remainingSec <= 0) {
        clearInterval(timerId);
        timerId = null;
        onFinish();
      }
    }

    function onStepTrigger(step) {
      const when = fmtSS(Math.max(0, step.addAtSec));
      const title = `Add ${step.name}`;
      const body = `Add ${step.name} now (scheduled at ${when}).`;
      notify(title, body);
      beep(); // play sound
    }

    function onFinish() {
      notify('Cooking done', 'All vegetables should be ready.');
      beep(0.35, 680, 0.35);
      $('status').innerText = 'Done';
      $('pause-btn').disabled = true;
      $('resume-btn').disabled = true;
    }

    function updateCountdownUI() {
      $('countdown').innerText = fmtSS(remainingSec);
      // status update
      if (timerId && !paused) $('status').innerText = 'Cooking';
      else if (timerId && paused) $('status').innerText = 'Paused';
      else if (!timerId && remainingSec === 0 && schedule.length>0) $('status').innerText = 'Done';
      else $('status').innerText = 'Idle';
    }

    // Controls: pause / resume / stop / reset
    function pauseTimer() {
      if (!timerId) return;
      paused = true;
      $('pause-btn').disabled = true;
      $('resume-btn').disabled = false;
      $('status').innerText = 'Paused';
    }
    function resumeTimer() {
      if (!timerId) return;
      paused = false;
      $('pause-btn').disabled = false;
      $('resume-btn').disabled = true;
      $('status').innerText = 'Cooking';
    }
    function stopTimer() {
      if (timerId) { clearInterval(timerId); timerId = null; }
      schedule = [];
      totalSec = 0;
      remainingSec = 0;
      paused = false;
      selected.clear();
      saveVeggies();
      renderVegList();
      renderSteps();
      updateCountdownUI();
      updateStartButton();
      notify('Stopped', 'Timer stopped and reset.');
    }
    function resetTimer() {
      if (!schedule || schedule.length === 0) {
        // nothing to reset
        remainingSec = 0;
      } else {
        // reset to start values
        schedule.forEach(s => s.fired = false);
        remainingSec = totalSec;
      }
      if (timerId) clearInterval(timerId);
      timerId = null;
      paused = false;
      updateCountdownUI();
      renderSteps();
      $('pause-btn').disabled = true;
      $('resume-btn').disabled = true;
      $('status').innerText = 'Idle';
      notify('Reset', 'Timer reset.');
    }

    // ---------- event wiring ----------
    document.addEventListener('DOMContentLoaded', () => {
      renderVegList();
      renderSteps();
      updateStartButton();

      $('add-custom-btn').addEventListener('click', () => {
        const name = $('custom-name').value.trim();
        const mins = parseFloat($('custom-min').value);
        if (!name || isNaN(mins) || mins < 0) {
          alert('Enter a name and valid minutes (>=0)');
          return;
        }
        veggies.push({ name, minutes: Math.max(0, Math.round(mins)) });
        saveVeggies();
        $('custom-name').value = ''; $('custom-min').value = '';
        renderVegList();
      });

      $('start-btn').addEventListener('click', () => {
        // compute schedule first; require notification permission optionally
        if (Notification && Notification.permission !== 'granted' && Notification.permission !== 'denied') {
          Notification.requestPermission().catch(()=>{}); // ask but don't block
        }
        startTimer();
      });

      $('stop-btn').addEventListener('click', () => {
        if (confirm('Stop cooking and reset?')) stopTimer();
      });

      $('pause-btn').addEventListener('click', pauseTimer);
      $('resume-btn').addEventListener('click', resumeTimer);
      $('reset-btn').addEventListener('click', () => {
        if (confirm('Reset current timer?')) resetTimer();
      });

      // disable default behavior for buttons etc.
      document.body.addEventListener('click', (e) => {
        // ensure start disabled logic
        updateStartButton();
      });
    });
  </script>
</body>
</html>
