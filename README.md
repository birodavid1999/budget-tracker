<!DOCTYPE html>
<html lang="hu">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, maximum-scale=1.0, user-scalable=no">
<title>Budget Tracker</title>

<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Budget">
<meta name="mobile-web-app-capable" content="yes">
<meta name="theme-color" content="#111827">

<link rel="manifest" href="manifest.json">

<link rel="apple-touch-icon" href="icon-192.png">
<link rel="apple-touch-icon" sizes="192x192" href="icon-192.png">
<link rel="apple-touch-icon" sizes="512x512" href="icon-512.png">

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css">
<style>
:root {
  --font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --color-text-primary: #111827;
  --color-text-secondary: #6B7280;
  --color-text-tertiary: #9CA3AF;
  --color-text-success: #15803d;
  --color-text-danger: #dc2626;
  --color-background-primary: #ffffff;
  --color-background-secondary: #F9FAFB;
  --color-border-secondary: #E5E7EB;
  --color-border-tertiary: #F3F4F6;
  --color-border-danger: #fca5a5;
  --border-radius-md: 8px;

  /* iOS safe area */
  --safe-top: env(safe-area-inset-top, 0px);
  --safe-bottom: env(safe-area-inset-bottom, 0px);
  --safe-left: env(safe-area-inset-left, 0px);
  --safe-right: env(safe-area-inset-right, 0px);

  /* Tab bar magasság */
  --tab-bar-h: calc(60px + var(--safe-bottom));
}

* { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

html, body {
  height: 100dvh;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background: var(--color-background-primary);
}

body {
  position: fixed;
  width: 100%;
  height: 100dvh;
  font-family: var(--font-sans);
}

/* ── Fő layout ── */
.app {
  display: flex;
  flex-direction: column;
  height: 100dvh;
  width: 100%;
}

/* ── Header (státuszsor alá igazítva) ── */
.app-header {
  flex-shrink: 0;
  padding-top: calc(var(--safe-top) + 12px);
  padding-bottom: 12px;
  padding-left: calc(var(--safe-left) + 16px);
  padding-right: calc(var(--safe-right) + 16px);
  background: var(--color-background-primary);
  border-bottom: 0.5px solid var(--color-border-secondary);
  z-index: 10;
}

.app-header h1 {
  margin: 0;
  font-size: 17px;
  font-weight: 600;
  color: var(--color-text-primary);
  text-align: center;
  letter-spacing: -0.3px;
}

/* ── Scrollozható tartalom ── */
.scroll-area {
  flex: 1;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  padding: 0;
  padding-bottom: var(--tab-bar-h);
}

.wrap {
  padding: 1rem 1rem;
  max-width: 640px;
  margin: 0 auto;
}

/* ── Bottom tab bar ── */
.tab-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  height: var(--tab-bar-h);
  background: rgba(255, 255, 255, 0.94);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  backdrop-filter: blur(20px) saturate(180%);
  border-top: 0.5px solid var(--color-border-secondary);
  display: flex;
  align-items: flex-start;
  justify-content: space-around;
  padding-top: 10px;
  padding-left: var(--safe-left);
  padding-right: var(--safe-right);
  z-index: 100;
}

.tab-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  flex: 1;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  -webkit-appearance: none;
  appearance: none;
  outline: none;
  color: var(--color-text-tertiary);
  transition: color 0.15s;
  font-family: var(--font-sans);
}

.tab-item i {
  font-size: 24px;
  line-height: 1;
}

.tab-item span {
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.01em;
}

.tab-item.active {
  color: var(--color-text-primary);
}

/* ── Oldalak ── */
.page { display: none; }
.page.active { display: block; }

/* ── Eredeti stílusok javítva ── */
.entry-row { display: flex; gap: 8px; align-items: center; margin-bottom: 10px; }

/* 16px font-size megelőzi a nemkívánatos iOS Safari automatikus zoomolást */
select, input[type=number] {
  height: 44px;
  border: 0.5px solid var(--color-border-secondary);
  border-radius: var(--border-radius-md);
  background: var(--color-background-primary);
  color: var(--color-text-primary);
  font-size: 16px;
  padding: 0 10px;
  outline: none;
  -webkit-appearance: none;
  appearance: none;
}
select {
  flex: 1.4;
  cursor: pointer;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%239CA3AF' stroke-width='1.5' fill='none' stroke-linecap='round' stroke-linejoin='round'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 10px center;
  padding-right: 28px;
}
input[type=number] { flex: 1; text-align: right; }
select:focus, input:focus { border-color: #9CA3AF; }

.add-btn {
  height: 44px; padding: 0 14px;
  border: 0.5px solid var(--color-border-secondary);
  border-radius: var(--border-radius-md);
  background: transparent; color: var(--color-text-primary);
  font-size: 14px; cursor: pointer;
  display: flex; align-items: center; gap: 6px;
  white-space: nowrap; transition: background 0.15s;
  font-family: var(--font-sans);
  -webkit-appearance: none; appearance: none;
}
.add-btn:active { background: var(--color-background-secondary); transform: scale(0.97); }

.section-label {
  font-size: 11px; font-weight: 500;
  color: var(--color-text-tertiary);
  letter-spacing: 0.07em; text-transform: uppercase;
  margin: 0 0 8px;
}

.ledger { width: 100%; border-collapse: collapse; font-size: 13px; margin-top: 4px; }
.ledger th {
  text-align: left; padding: 6px 10px;
  color: var(--color-text-tertiary); font-weight: 500;
  border-bottom: 0.5px solid var(--color-border-secondary); font-size: 12px;
}
.ledger td {
  padding: 11px 10px;
  border-bottom: 0.5px solid var(--color-border-tertiary);
  color: var(--color-text-primary); vertical-align: middle;
}
.ledger tr:last-child td { border-bottom: none; }
.ledger tr:active td { background: var(--color-background-secondary); }

.del-btn {
  background: none; border: none; cursor: pointer;
  color: var(--color-text-tertiary); font-size: 18px;
  padding: 4px 6px; border-radius: 6px; transition: color 0.15s;
  -webkit-appearance: none; appearance: none;
}
.del-btn:active { color: var(--color-text-danger); }

.cat-badge {
  display: inline-flex; align-items: center; gap: 5px;
  padding: 3px 9px; border-radius: 999px;
  font-size: 12px; font-weight: 500;
}

.metric-grid {
  display: grid; grid-template-columns: 1fr 1fr;
  gap: 10px; margin-bottom: 1.5rem;
}
.metric { background: var(--color-background-secondary); border-radius: var(--border-radius-md); padding: 14px 16px; }
.metric p { margin: 0; }
.metric .lbl { font-size: 12px; color: var(--color-text-secondary); margin-bottom: 4px; }
.metric .val { font-size: 20px; font-weight: 600; }

.bar-wrap { margin-bottom: 1.2rem; }
.bar-row { display: flex; align-items: center; gap: 10px; margin-bottom: 8px; }
.bar-lbl { width: 85px; font-size: 12px; color: var(--color-text-secondary); text-align: right; flex-shrink: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.bar-track { flex: 1; height: 20px; background: var(--color-background-secondary); border-radius: 4px; overflow: hidden; }
.bar-fill { height: 100%; border-radius: 4px; transition: width 0.4s; }
.bar-val { width: 58px; font-size: 12px; color: var(--color-text-secondary); text-align: right; flex-shrink: 0; }

.empty-state { text-align: center; padding: 2rem 1rem; color: var(--color-text-tertiary); font-size: 13px; }
.saved-flash { display: none; font-size: 12px; color: var(--color-text-success); align-items: center; gap: 4px; }
.saved-flash.show { display: flex; }
.divider { border: none; border-top: 0.5px solid var(--color-border-tertiary); margin: 1.2rem 0; }

/* PWA install banner */
#install-banner {
  display: none;
  background: var(--color-background-secondary);
  border: 0.5px solid var(--color-border-secondary);
  border-radius: var(--border-radius-md);
  padding: 12px 14px;
  margin-bottom: 14px;
  font-size: 13px;
  color: var(--color-text-secondary);
  align-items: center;
  gap: 10px;
}
#install-banner.show { display: flex; }
#install-banner i { font-size: 20px; color: var(--color-text-primary); flex-shrink: 0; }
#install-banner .install-text { flex: 1; }
#install-banner .install-text strong { display: block; color: var(--color-text-primary); margin-bottom: 2px; }
#close-banner { background: none; border: none; cursor: pointer; color: var(--color-text-tertiary); padding: 4px; font-size: 16px; }
</style>
</head>
<body>
<div class="app">

  <div class="app-header">
    <h1>Budget Tracker</h1>
  </div>

  <div class="scroll-area">
    <div class="wrap">

      <div class="page active" id="page-input">

        <div id="install-banner">
          <i class="ti ti-share"></i>
          <div class="install-text">
            <strong>Tedd a főképernyőre!</strong>
            Nyomd meg a <strong>Megosztás</strong> ikont, majd válaszd a <strong>„Főképernyőhöz adás"</strong> lehetőséget.
          </div>
          <button id="close-banner" onclick="closeBanner()"><i class="ti ti-x"></i></button>
        </div>

        <p class="section-label">Bevétel hozzáadása</p>
        <div class="entry-row">
          <select id="inc-cat">
            <option value="Munkaber">Munkabér</option>
            <option value="Szabaduszo">Szabadúszó</option>
            <option value="Egyeb bevetel">Egyéb bevétel</option>
          </select>
          <input type="number" id="inc-amt" placeholder="összeg" min="0" inputmode="decimal" />
          <button class="add-btn" onclick="addEntry('income')">
            <i class="ti ti-plus"></i> Mentés
          </button>
          <span class="saved-flash" id="flash-income"><i class="ti ti-check"></i> Mentve</span>
        </div>

        <hr class="divider">

        <p class="section-label">Kiadás hozzáadása</p>
        <div class="entry-row">
          <select id="exp-cat">
            <option value="Kocsi">Kocsi</option>
            <option value="Biztositas">Biztosítás</option>
            <option value="Groceries">Élelmiszer</option>
            <option value="Szorakozas">Szórakozás</option>
          </select>
          <input type="number" id="exp-amt" placeholder="összeg" min="0" inputmode="decimal" />
          <button class="add-btn" onclick="addEntry('expense')">
            <i class="ti ti-plus"></i> Mentés
          </button>
          <span class="saved-flash" id="flash-expense"><i class="ti ti-check"></i> Mentve</span>
        </div>

        <hr class="divider">
        <p class="section-label">Legutóbbi tételek</p>
        <div id="recent-list"></div>
      </div>

      <div class="page" id="page-list">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;">
          <p class="section-label" style="margin:0">Összes tétel</p>
          <button class="add-btn" onclick="clearAll()" style="font-size:12px;color:var(--color-text-danger);border-color:var(--color-border-danger)">
            <i class="ti ti-trash"></i> Törlés
          </button>
        </div>
        <div id="full-list"></div>
      </div>

      <div class="page" id="page-stats">
        <div class="metric-grid" id="metrics"></div>
        <p class="section-label">Kiadások kategóriánként</p>
        <div class="bar-wrap" id="exp-bars"></div>
        <p class="section-label">Bevételek kategóriánként</p>
        <div class="bar-wrap" id="inc-bars"></div>
      </div>

    </div>
  </div>

  <nav class="tab-bar">
    <button class="tab-item active" id="tab-input" onclick="switchTab('input')">
      <i class="ti ti-pencil"></i>
      <span>Rögzítés</span>
    </button>
    <button class="tab-item" id="tab-list" onclick="switchTab('list')">
      <i class="ti ti-list"></i>
      <span>Tételek</span>
    </button>
    <button class="tab-item" id="tab-stats" onclick="switchTab('stats')">
      <i class="ti ti-chart-bar"></i>
      <span>Összesítő</span>
    </button>
  </nav>

</div>

<script>
// Service Worker regisztrálása
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('./sw.js').catch(() => {});
  });
}

// iOS install banner megjelenítése (ha nem standalone módban van)
window.addEventListener('load', () => {
  const isIOS = /iphone|ipad|ipod/i.test(navigator.userAgent);
  const isStandalone = window.navigator.standalone === true;
  const bannerDismissed = localStorage.getItem('install_banner_dismissed');

  if (isIOS && !isStandalone && !bannerDismissed) {
    document.getElementById('install-banner').classList.add('show');
  }
});

function closeBanner() {
  document.getElementById('install-banner').classList.remove('show');
  localStorage.setItem('install_banner_dismissed', '1');
}

const CAT_COLORS = {
  'Kocsi':          { bg: '#E6F1FB', text: '#0C447C', bar: '#378ADD' },
  'Biztositas':     { bg: '#EEEDFE', text: '#3C3489', bar: '#7F77DD' },
  'Groceries':      { bg: '#E1F5EE', text: '#085041', bar: '#1D9E75' },
  'Szorakozas':     { bg: '#FBEAF0', text: '#72243E', bar: '#D4537E' },
  'Munkaber':       { bg: '#EAF3DE', text: '#27500A', bar: '#639922' },
  'Szabaduszo':     { bg: '#FAEEDA', text: '#633806', bar: '#BA7517' },
  'Egyeb bevetel':  { bg: '#F1EFE8', text: '#444441', bar: '#888780' },
};

const STORAGE_KEY = 'budget_entries';
let entries = [];

function load() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (raw) entries = JSON.parse(raw);
  } catch(e) { entries = []; }
  renderRecent();
}

function save() {
  try { localStorage.setItem(STORAGE_KEY, JSON.stringify(entries)); } catch(e) {}
}

function addEntry(type) {
  const catEl = document.getElementById(type === 'income' ? 'inc-cat' : 'exp-cat');
  const amtEl = document.getElementById(type === 'income' ? 'inc-amt' : 'exp-amt');
  const amt = parseFloat(amtEl.value);
  if (!amt || amt <= 0) { amtEl.focus(); return; }
  entries.push({ id: Date.now(), type, cat: catEl.value, amt, date: new Date().toLocaleDateString('hu-HU') });
  save();
  amtEl.value = '';
  amtEl.blur(); // billentyűzet bezárása iOS-en mentés után
  const flash = document.getElementById('flash-' + type);
  flash.classList.add('show');
  setTimeout(() => flash.classList.remove('show'), 1600);
  renderRecent();
}

document.addEventListener('keydown', e => {
  if (e.key === 'Enter') {
    if (document.activeElement.id === 'inc-amt') addEntry('income');
    if (document.activeElement.id === 'exp-amt') addEntry('expense');
  }
});

function badge(cat) {
  const c = CAT_COLORS[cat] || { bg: '#F1EFE8', text: '#444441' };
  const labels = {
    'Kocsi': 'Kocsi', 'Biztositas': 'Biztosítás', 'Groceries': 'Élelmiszer',
    'Szorakozas': 'Szórakozás', 'Munkaber': 'Munkabér',
    'Szabaduszo': 'Szabadúszó', 'Egyeb bevetel': 'Egyéb bevétel'
  };
  return `<span class="cat-badge" style="background:${c.bg};color:${c.text}">${labels[cat] || cat}</span>`;
}

function renderRecent() {
  const el = document.getElementById('recent-list');
  const recent = [...entries].reverse().slice(0, 5);
  if (!recent.length) { el.innerHTML = '<div class="empty-state">Még nincs tétel</div>'; return; }
  el.innerHTML = `<table class="ledger">
    <thead><tr><th>Kategória</th><th>Típus</th><th style="text-align:right">Összeg</th><th>Dátum</th></tr></thead>
    <tbody>${recent.map(e => `<tr>
      <td>${badge(e.cat)}</td>
      <td style="color:${e.type==='income'?'var(--color-text-success)':'var(--color-text-danger)'};font-size:12px">${e.type==='income'?'bevétel':'kiadás'}</td>
      <td style="text-align:right;font-weight:500">${e.amt.toLocaleString('hu-HU')}</td>
      <td style="color:var(--color-text-tertiary);font-size:12px">${e.date}</td>
    </tr>`).join('')}</tbody>
  </table>`;
}

function renderList() {
  const el = document.getElementById('full-list');
  if (!entries.length) { el.innerHTML = '<div class="empty-state">Még nincs tétel</div>'; return; }
  el.innerHTML = `<table class="ledger">
    <thead><tr><th>Kategória</th><th>Típus</th><th style="text-align:right">Összeg</th><th>Dátum</th><th></th></tr></thead>
    <tbody>${[...entries].reverse().map(e => `<tr>
      <td>${badge(e.cat)}</td>
      <td style="color:${e.type==='income'?'var(--color-text-success)':'var(--color-text-danger)'};font-size:12px">${e.type==='income'?'bevétel':'kiadás'}</td>
      <td style="text-align:right;font-weight:500">${e.amt.toLocaleString('hu-HU')}</td>
      <td style="color:var(--color-text-tertiary);font-size:12px">${e.date}</td>
      <td><button class="del-btn" onclick="deleteEntry(${e.id})"><i class="ti ti-x"></i></button></td>
    </tr>`).join('')}</tbody>
  </table>`;
}

function deleteEntry(id) {
  entries = entries.filter(e => e.id !== id);
  save();
  renderList();
  renderRecent();
}

function clearAll() {
  if (!confirm('Biztosan törlöd az összes tételt?')) return;
  entries = [];
  save();
  renderList();
  renderRecent();
}

function renderStats() {
  const totalInc = entries.filter(e=>e.type==='income').reduce((s,e)=>s+e.amt,0);
  const totalExp = entries.filter(e=>e.type==='expense').reduce((s,e)=>s+e.amt,0);
  const balance = totalInc - totalExp;
  const savPct = totalInc > 0 ? Math.round(balance / totalInc * 100) : null;

  document.getElementById('metrics').innerHTML = `
    <div class="metric"><p class="lbl">Összes bevétel</p><p class="val" style="color:var(--color-text-success)">${totalInc.toLocaleString('hu-HU')}</p></div>
    <div class="metric"><p class="lbl">Összes kiadás</p><p class="val" style="color:var(--color-text-danger)">${totalExp.toLocaleString('hu-HU')}</p></div>
    <div class="metric"><p class="lbl">Egyenleg</p><p class="val" style="color:${balance>=0?'var(--color-text-success)':'var(--color-text-danger)'}">${(balance>=0?'+':'')+balance.toLocaleString('hu-HU')}</p></div>
    <div class="metric"><p class="lbl">Megtakarítás</p><p class="val">${savPct!==null?savPct+'%':'-'}</p></div>`;

  function bars(type, containerId) {
    const cats = {};
    entries.filter(e=>e.type===type).forEach(e=>{ cats[e.cat]=(cats[e.cat]||0)+e.amt; });
    const total = Object.values(cats).reduce((s,v)=>s+v,0);
    const el = document.getElementById(containerId);
    if (!total) { el.innerHTML = '<div class="empty-state" style="padding:0.5rem 0">Nincs adat</div>'; return; }
    const labels = {
      'Kocsi': 'Kocsi', 'Biztositas': 'Biztosítás', 'Groceries': 'Élelmiszer',
      'Szorakozas': 'Szórakozás', 'Munkaber': 'Munkabér',
      'Szabaduszo': 'Szabadúszó', 'Egyeb bevetel': 'Egyéb bevétel'
    };
    el.innerHTML = Object.entries(cats).sort((a,b)=>b[1]-a[1]).map(([cat,amt])=>{
      const pct = Math.round(amt/total*100);
      const c = CAT_COLORS[cat] || { bar: '#888780' };
      return `<div class="bar-row">
        <div class="bar-lbl">${labels[cat] || cat}</div>
        <div class="bar-track"><div class="bar-fill" style="width:${pct}%;background:${c.bar}"></div></div>
        <div class="bar-val">${amt.toLocaleString('hu-HU')}</div>
      </div>`;
    }).join('');
  }
  bars('expense','exp-bars');
  bars('income','inc-bars');
}

function switchTab(name) {
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.tab-item').forEach(t=>t.classList.remove('active'));
  document.getElementById('page-'+name).classList.add('active');
  document.getElementById('tab-'+name).classList.add('active');
  // Scroll vissza a tetejére lapváltáskor
  document.querySelector('.scroll-area').scrollTop = 0;
  if (name==='list') renderList();
  if (name==='stats') renderStats();
}

load();
</script>
</body>
</html>
