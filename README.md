# Initiative-Cafe-ID-Scanner
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Library ID Scanner</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&family=IBM+Plex+Sans:wght@400;500&display=swap" rel="stylesheet" />
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg:        #f5f4f0;
    --surface:   #ffffff;
    --surface2:  #eeece7;
    --border:    #d8d5cc;
    --text:      #1a1a18;
    --muted:     #6b6960;
    --hint:      #a09d96;
    --green-bg:  #e8f5ec;
    --green-txt: #1a6630;
    --green-bdr: #a8d8b5;
    --amber-bg:  #fdf3dc;
    --amber-txt: #7a4f00;
    --amber-bdr: #f0d080;
    --red-bg:    #fdecea;
    --red-txt:   #8b1f1f;
    --red-bdr:   #f0aaaa;
    --radius:    10px;
    --radius-sm: 6px;
    --mono:      'IBM Plex Mono', monospace;
    --sans:      'IBM Plex Sans', sans-serif;
  }

  body {
    font-family: var(--sans);
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    align-items: flex-start;
    justify-content: center;
    padding: 3rem 1rem 4rem;
  }

  .container {
    width: 100%;
    max-width: 560px;
  }

  header {
    margin-bottom: 2rem;
  }

  header h1 {
    font-size: 22px;
    font-weight: 500;
    letter-spacing: -0.02em;
    margin-bottom: 4px;
  }

  header p {
    font-size: 13px;
    color: var(--muted);
    line-height: 1.5;
  }

  header p code {
    font-family: var(--mono);
    font-size: 12px;
    background: var(--surface2);
    padding: 1px 5px;
    border-radius: 4px;
    border: 0.5px solid var(--border);
  }

  .input-row {
    display: flex;
    gap: 8px;
    margin-bottom: 1.25rem;
  }

  input[type=text] {
    flex: 1;
    height: 44px;
    font-family: var(--mono);
    font-size: 15px;
    padding: 0 14px;
    border-radius: var(--radius-sm);
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--text);
    outline: none;
    transition: border-color 0.15s;
  }

  input[type=text]::placeholder { color: var(--hint); }
  input[type=text]:focus { border-color: #888; }

  button.scan-btn {
    height: 44px;
    padding: 0 20px;
    font-family: var(--sans);
    font-size: 14px;
    font-weight: 500;
    border-radius: var(--radius-sm);
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--text);
    cursor: pointer;
    white-space: nowrap;
    transition: background 0.12s;
  }

  button.scan-btn:hover { background: var(--surface2); }
  button.scan-btn:active { transform: scale(0.98); }

  .alert {
    padding: 11px 14px;
    border-radius: var(--radius-sm);
    font-size: 13px;
    font-weight: 500;
    margin-bottom: 1.25rem;
    line-height: 1.45;
    display: none;
    border: 1px solid transparent;
  }
  .alert.show { display: block; }
  .alert.first { background: var(--green-bg); color: var(--green-txt); border-color: var(--green-bdr); }
  .alert.dupe  { background: var(--amber-bg); color: var(--amber-txt); border-color: var(--amber-bdr); }
  .alert.err   { background: var(--red-bg);   color: var(--red-txt);   border-color: var(--red-bdr); }

  .metrics {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 10px;
    margin-bottom: 1.5rem;
  }

  .metric {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 14px;
    text-align: center;
  }

  .metric-label {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: .06em;
    color: var(--muted);
    margin-bottom: 6px;
  }

  .metric-val {
    font-size: 28px;
    font-weight: 500;
    font-family: var(--mono);
    letter-spacing: -0.02em;
    color: var(--text);
  }

  .log-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 8px;
  }

  .log-title {
    font-size: 13px;
    font-weight: 500;
    color: var(--text);
    display: flex;
    align-items: center;
    gap: 8px;
  }

  .date-pill {
    font-size: 11px;
    padding: 2px 9px;
    border-radius: 20px;
    background: var(--surface2);
    color: var(--muted);
    border: 0.5px solid var(--border);
    font-weight: 400;
  }

  .reset-btn {
    font-size: 12px;
    color: var(--hint);
    background: none;
    border: none;
    cursor: pointer;
    padding: 4px 8px;
    border-radius: var(--radius-sm);
    font-family: var(--sans);
    transition: color 0.12s, background 0.12s;
  }

  .reset-btn:hover { background: var(--red-bg); color: var(--red-txt); }

  .log {
    border: 1px solid var(--border);
    border-radius: var(--radius);
    overflow: hidden;
    background: var(--surface);
  }

  .log-empty {
    padding: 28px;
    text-align: center;
    font-size: 13px;
    color: var(--hint);
  }

  .log-item {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 10px 14px;
    border-bottom: 1px solid var(--border);
    font-size: 13px;
  }

  .log-item:last-child { border-bottom: none; }

  .cname {
    font-family: var(--mono);
    font-size: 13px;
    font-weight: 500;
    color: var(--text);
    min-width: 80px;
  }

  .badge {
    font-size: 11px;
    font-weight: 500;
    padding: 2px 8px;
    border-radius: 20px;
    white-space: nowrap;
    flex-shrink: 0;
  }

  .badge.first { background: var(--green-bg); color: var(--green-txt); }
  .badge.dupe  { background: var(--amber-bg); color: var(--amber-txt); }

  .first-time {
    font-size: 12px;
    color: var(--muted);
  }

  .scan-time {
    margin-left: auto;
    font-size: 12px;
    color: var(--hint);
    font-family: var(--mono);
    white-space: nowrap;
  }

  footer {
    margin-top: 2rem;
    font-size: 12px;
    color: var(--hint);
    text-align: center;
  }
</style>
</head>
<body>
<div class="container">
  <header>
    <h1>Library ID Scanner</h1>
    <p>Format: <code>c</code> + year (26–29) + 2 initials + optional number &nbsp;·&nbsp; e.g. <code>c26cs</code>, <code>c28jd1</code><br>
    Scans reset automatically each day.</p>
  </header>

  <div class="input-row">
    <input type="text" id="cname-input" placeholder="e.g. c26cs or c28jd1" autocomplete="off" spellcheck="false" />
    <button class="scan-btn" onclick="handleScan()">Scan ID</button>
  </div>

  <div class="alert" id="alert"></div>

  <div class="metrics">
    <div class="metric">
      <div class="metric-label">Total scans</div>
      <div class="metric-val" id="total-scans">0</div>
    </div>
    <div class="metric">
      <div class="metric-label">Unique IDs</div>
      <div class="metric-val" id="unique-ids">0</div>
    </div>
    <div class="metric">
      <div class="metric-label">Duplicates</div>
      <div class="metric-val" id="dupes">0</div>
    </div>
  </div>

  <div class="log-header">
    <span class="log-title">
      Scan log
      <span class="date-pill" id="today-label"></span>
    </span>
    <button class="reset-btn" onclick="confirmReset()">Reset day</button>
  </div>
  <div class="log" id="log">
    <div class="log-empty">No scans yet today</div>
  </div>

  <footer>Scans are stored locally in your browser and reset each day.</footer>
</div>

<script>
  // Valid: c + year 26-29 + exactly 2 lowercase letters + optional single digit
  const CNAME_RE = /^c(2[6-9])[a-z]{2}\d?$/i;
  const STORAGE_KEY = 'lib_scanner_v2';

  function validate(raw) {
    if (!raw) return 'Please enter a C-name.';
    const v = raw.trim().toLowerCase();
    if (!v.startsWith('c')) return 'Must start with the letter "c" (e.g. c26cs).';
    const yearMatch = v.match(/^c(\d{2})/);
    if (!yearMatch) return 'Missing graduation year after "c" (e.g. c26cs).';
    const year = parseInt(yearMatch[1], 10);
    if (year < 26 || year > 29) return `Year "${yearMatch[1]}" is out of range — must be 26, 27, 28, or 29.`;
    const after = v.slice(3);
    if (after.length < 2) return 'Need at least 2 initials after the year (e.g. c26cs).';
    if (!CNAME_RE.test(v)) return 'Invalid format. Only 2 letters for initials and an optional trailing digit allowed.';
    return null;
  }

  function getTodayKey() {
    const d = new Date();
    return `${d.getFullYear()}-${d.getMonth() + 1}-${d.getDate()}`;
  }

  function loadState() {
    try {
      const raw = localStorage.getItem(STORAGE_KEY);
      if (!raw) return null;
      const s = JSON.parse(raw);
      if (s.date !== getTodayKey()) return null;
      return s;
    } catch { return null; }
  }

  function saveState(s) {
    try { localStorage.setItem(STORAGE_KEY, JSON.stringify(s)); } catch {}
  }

  function freshState() {
    return { date: getTodayKey(), log: [], seen: {} };
  }

  let state = loadState() || freshState();

  function fmt(ts) {
    return new Date(ts).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  }

  function render() {
    const total = state.log.length;
    const unique = Object.keys(state.seen).length;
    const dupes = total - unique;
    document.getElementById('total-scans').textContent = total;
    document.getElementById('unique-ids').textContent = unique;
    document.getElementById('dupes').textContent = dupes;

    const logEl = document.getElementById('log');
    if (state.log.length === 0) {
      logEl.innerHTML = '<div class="log-empty">No scans yet today</div>';
      return;
    }

    logEl.innerHTML = [...state.log].reverse().map(e => `
      <div class="log-item">
        <span class="cname">${e.cname}</span>
        <span class="badge ${e.type}">${e.type === 'first' ? 'First scan' : 'Duplicate'}</span>
        ${e.type === 'dupe' ? `<span class="first-time">first at ${fmt(e.firstTs)}</span>` : ''}
        <span class="scan-time">${fmt(e.ts)}</span>
      </div>
    `).join('');
  }

  function showAlert(msg, type) {
    const el = document.getElementById('alert');
    el.textContent = msg;
    el.className = 'alert show ' + type;
    clearTimeout(el._t);
    el._t = setTimeout(() => el.className = 'alert', 4000);
  }

  function handleScan() {
    const raw = document.getElementById('cname-input').value.trim();
    document.getElementById('cname-input').value = '';
    document.getElementById('cname-input').focus();

    const err = validate(raw);
    if (err) { showAlert(err, 'err'); return; }

    const key = raw.trim().toLowerCase();
    const ts = Date.now();

    if (state.seen[key]) {
      state.log.push({ cname: key, type: 'dupe', ts, firstTs: state.seen[key].firstTs });
      showAlert(`Already scanned today: ${key} — first scanned at ${fmt(state.seen[key].firstTs)}`, 'dupe');
    } else {
      state.seen[key] = { firstTs: ts };
      state.log.push({ cname: key, type: 'first', ts });
      showAlert(`First scan of the day for ${key}`, 'first');
    }

    saveState(state);
    render();
  }

  document.getElementById('cname-input').addEventListener('keydown', e => {
    if (e.key === 'Enter') handleScan();
  });

  const d = new Date();
  document.getElementById('today-label').textContent = d.toLocaleDateString([], {
    weekday: 'short', month: 'short', day: 'numeric'
  });

  render();
</script>
</body>
</html>
