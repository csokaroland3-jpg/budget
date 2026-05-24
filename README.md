<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="theme-color" content="#f2f2f7" media="(prefers-color-scheme: light)">
<meta name="theme-color" content="#000000" media="(prefers-color-scheme: dark)">
<title>Budget</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
:root{
  --ink:#1c1c1e;--ink2:#3a3a3c;--ink3:#636366;--ink4:#8e8e93;--ink5:#c7c7cc;
  --fill0:#ffffff;--fill1:#f2f2f7;--fill2:#e5e5ea;--fill3:#d1d1d6;
  --sep:rgba(60,60,67,.13);--sep2:rgba(60,60,67,.22);
  --grn:#34c759;--grn-bg:rgba(52,199,89,.13);--grn-deep:#1a7a32;
  --red:#ff3b30;--red-bg:rgba(255,59,48,.12);--red-deep:#b02820;
  --blue:#007aff;--blue-bg:rgba(0,122,255,.11);
  --orange:#ff9500;--purple:#af52de;
  font-family:'Inter',system-ui,-apple-system,sans-serif;
}
@media(prefers-color-scheme:dark){
  :root{
    --ink:#ffffff;--ink2:rgba(235,235,245,.82);--ink3:rgba(235,235,245,.6);--ink4:rgba(235,235,245,.3);--ink5:rgba(235,235,245,.18);
    --fill0:#1c1c1e;--fill1:#2c2c2e;--fill2:#3a3a3c;--fill3:#48484a;
    --sep:rgba(84,84,88,.55);--sep2:rgba(84,84,88,.8);
    --grn:#30d158;--grn-bg:rgba(48,209,88,.15);--grn-deep:#30d158;
    --red:#ff453a;--red-bg:rgba(255,69,58,.15);--red-deep:#ff453a;
    --blue:#0a84ff;--blue-bg:rgba(10,132,255,.15);
  }
  body{background:#000}
}

body{
  background:var(--fill1);
  color:var(--ink);
  padding-bottom:calc(72px + env(safe-area-inset-bottom,0px));
  min-height:100vh;
}

/* ── HEADER ── */
.status-spacer{height:env(safe-area-inset-top,44px);background:var(--fill1)}
.header{
  background:rgba(242,242,247,0.9);
  -webkit-backdrop-filter:saturate(180%) blur(20px);
  backdrop-filter:saturate(180%) blur(20px);
  border-bottom:0.5px solid var(--sep2);
  padding:10px 18px 12px;
  position:sticky;top:0;z-index:100;
}
@media(prefers-color-scheme:dark){
  .header{background:rgba(28,28,30,0.9)}
}
.header-row{display:flex;align-items:center;justify-content:space-between;gap:8px}
.header-logo{display:flex;align-items:center;gap:7px;font-size:17px;font-weight:700;color:var(--ink);letter-spacing:-.4px}
.logo-badge{width:30px;height:30px;border-radius:8px;background:var(--blue);display:flex;align-items:center;justify-content:center;flex-shrink:0}
.mnav{display:flex;align-items:center;gap:4px}
.mnav-btn{
  width:30px;height:30px;border-radius:50%;border:none;
  background:var(--fill2);color:var(--ink3);
  cursor:pointer;display:flex;align-items:center;justify-content:center;
  transition:background .1s;
}
.mnav-btn:active{background:var(--fill3);transform:scale(.92)}
.mnav-label{font-size:13px;font-weight:600;color:var(--ink);min-width:120px;text-align:center}

/* ── CONTENT ── */
.content{padding:10px 16px}
.page{display:none}.page.active{display:block}

/* ── KPI ── */
.kpi-row{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:12px}
.kpi{background:var(--fill0);border-radius:16px;padding:12px 12px 10px}
.kpi-lbl{font-size:9.5px;font-weight:600;color:var(--ink4);text-transform:uppercase;letter-spacing:.05em;margin-bottom:5px;display:flex;align-items:center;gap:3px}
.kpi-val{font-size:15px;font-weight:700;letter-spacing:-.4px;line-height:1}
.kpi-val.grn{color:var(--grn)}
.kpi-val.red{color:var(--red)}
.kpi-val.blue{color:var(--blue)}

/* ── CARD ── */
.card{background:var(--fill0);border-radius:20px;padding:16px 16px;margin-bottom:12px}
.card-title{font-size:13px;font-weight:600;color:var(--ink3);margin-bottom:14px;display:flex;align-items:center;gap:7px}
.card-title svg{width:16px;height:16px;stroke:var(--blue);fill:none;flex-shrink:0}

/* ── CHART ── */
.chart-box{position:relative;height:196px}
.no-data-msg{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:8px;font-size:13px;color:var(--ink4)}

/* ── LEGEND ── */
.leg-sec{font-size:10.5px;font-weight:700;text-transform:uppercase;letter-spacing:.06em;margin:10px 0 5px;display:flex;align-items:center;gap:5px}
.leg-sec:first-child{margin-top:0}
.leg-row{display:flex;align-items:center;gap:9px;padding:7px 0;border-bottom:0.5px solid var(--sep)}
.leg-row:last-child{border-bottom:none}
.leg-dot{width:10px;height:10px;border-radius:3px;flex-shrink:0}
.leg-name{flex:1;font-size:13px;color:var(--ink2)}
.leg-pct{font-size:11px;color:var(--ink4);margin-right:6px}
.leg-val{font-size:13px;font-weight:600;color:var(--ink)}

/* ── BAR ── */
.bar-track{height:8px;border-radius:999px;overflow:hidden;display:flex;background:var(--fill2);margin:10px 0 7px}
.bar-grn{background:var(--grn);transition:width .6s cubic-bezier(.4,0,.2,1)}
.bar-red{background:var(--red);transition:width .6s cubic-bezier(.4,0,.2,1)}
.bar-sub{display:flex;justify-content:space-between;font-size:12px;font-weight:500}

/* ── ADD FORM ── */
.type-seg{display:flex;background:var(--fill1);border-radius:11px;padding:3px;margin-bottom:14px;gap:3px}
.type-btn{
  flex:1;padding:10px;border-radius:9px;border:none;
  background:transparent;cursor:pointer;
  font-size:14px;font-weight:600;color:var(--ink4);
  transition:all .18s;font-family:inherit;
  display:flex;align-items:center;justify-content:center;gap:5px;
}
.type-btn svg{width:15px;height:15px;stroke:currentColor;fill:none;flex-shrink:0}
.type-btn.active-i{background:var(--grn-bg);color:var(--grn-deep)}
.type-btn.active-e{background:var(--red-bg);color:var(--red-deep)}

.fg{margin-bottom:12px}
.fg label{font-size:11px;font-weight:700;color:var(--ink4);display:block;margin-bottom:6px;text-transform:uppercase;letter-spacing:.05em}
.fg input,.fg select{
  width:100%;height:46px;
  border-radius:11px;
  border:0.5px solid var(--sep2);
  background:var(--fill1);color:var(--ink);
  padding:0 14px;font-size:15px;
  font-family:inherit;outline:none;
  -webkit-appearance:none;appearance:none;
}
.fg input:focus,.fg select:focus{border-color:var(--blue);border-width:1.5px}
.sel-wrap{position:relative}
.sel-wrap::after{
  content:'';position:absolute;right:14px;top:50%;transform:translateY(-50%);
  width:0;height:0;
  border-left:5px solid transparent;border-right:5px solid transparent;
  border-top:6px solid var(--ink4);pointer-events:none;
}

.add-btn{
  width:100%;height:50px;border-radius:13px;border:none;cursor:pointer;
  font-size:15px;font-weight:700;font-family:inherit;
  display:flex;align-items:center;justify-content:center;gap:7px;
  margin-top:6px;transition:opacity .15s;
}
.add-btn:active{opacity:.78;transform:scale(.98)}
.add-btn svg{width:18px;height:18px;stroke:currentColor;fill:none}
.btn-i{background:var(--grn);color:#fff}
.btn-e{background:var(--red);color:#fff}

/* ── TRANSACTIONS ── */
.tx-list{padding-top:4px}
.tx-empty{font-size:14px;color:var(--ink4);text-align:center;padding:2rem 0;display:flex;flex-direction:column;align-items:center;gap:8px}
.tx-row{display:flex;align-items:center;gap:11px;padding:10px 0;border-bottom:0.5px solid var(--sep)}
.tx-row:last-child{border-bottom:none}
.cat-icon{width:38px;height:38px;border-radius:11px;display:flex;align-items:center;justify-content:center;flex-shrink:0}
.cat-icon svg{width:18px;height:18px;stroke:currentColor;fill:none;stroke-width:1.8}
.tx-body{flex:1;min-width:0}
.tx-name{font-size:14px;font-weight:500;color:var(--ink);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.tx-cat{font-size:11px;color:var(--ink4);margin-top:1px}
.tx-amt{font-size:14px;font-weight:700;white-space:nowrap}
.tx-amt.i{color:var(--grn)}
.tx-amt.e{color:var(--red)}
.tx-del{background:none;border:none;cursor:pointer;color:var(--ink5);padding:4px 2px;display:flex;align-items:center}
.tx-del:active{color:var(--red)}
.tx-del svg{width:18px;height:18px;stroke:currentColor;fill:none}

/* ── CATEGORIE PAGE ── */
.cat-section-title{font-size:13px;font-weight:700;color:var(--ink3);text-transform:uppercase;letter-spacing:.05em;margin:4px 0 10px;display:flex;align-items:center;gap:6px}
.cat-section-title svg{width:15px;height:15px;stroke:currentColor;fill:none}
.cat-chip-list{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:14px}
.cat-chip{
  display:flex;align-items:center;gap:6px;
  padding:7px 12px;border-radius:20px;
  border:1px solid var(--sep2);background:var(--fill0);
  font-size:13px;font-weight:500;color:var(--ink);
  cursor:default;
}
.cat-chip.custom{border-style:dashed}
.cat-chip-del{background:none;border:none;cursor:pointer;color:var(--ink4);display:flex;align-items:center;padding:0 0 0 2px}
.cat-chip-del:active{color:var(--red)}
.cat-chip-del svg{width:14px;height:14px;stroke:currentColor;fill:none}
.cat-add-row{display:flex;gap:8px;margin-top:8px}
.cat-add-row input{flex:1;height:44px;border-radius:11px;border:0.5px solid var(--sep2);background:var(--fill1);color:var(--ink);padding:0 14px;font-size:14px;font-family:inherit;outline:none}
.cat-add-row input:focus{border-color:var(--blue);border-width:1.5px}
.cat-add-btn{height:44px;padding:0 18px;border-radius:11px;border:none;background:var(--blue);color:#fff;font-size:14px;font-weight:600;cursor:pointer;font-family:inherit;white-space:nowrap;display:flex;align-items:center;gap:5px}
.cat-add-btn:active{opacity:.8}
.cat-add-btn svg{width:16px;height:16px;stroke:currentColor;fill:none}
.cat-type-seg{display:flex;background:var(--fill1);border-radius:10px;padding:3px;margin-bottom:14px;gap:3px}
.cat-type-btn{flex:1;padding:8px;border-radius:8px;border:none;background:transparent;cursor:pointer;font-size:13px;font-weight:600;color:var(--ink4);font-family:inherit;transition:all .18s}
.cat-type-btn.active-i{background:var(--grn-bg);color:var(--grn-deep)}
.cat-type-btn.active-e{background:var(--red-bg);color:var(--red-deep)}
.info-note{font-size:12px;color:var(--ink4);background:var(--fill1);border-radius:10px;padding:10px 12px;margin-top:8px;line-height:1.5}

/* ── TAB BAR ── */
.tab-bar{
  position:fixed;bottom:0;left:0;right:0;
  background:rgba(242,242,247,.88);
  -webkit-backdrop-filter:saturate(180%) blur(20px);
  backdrop-filter:saturate(180%) blur(20px);
  border-top:0.5px solid var(--sep2);
  padding:8px 0 calc(8px + env(safe-area-inset-bottom,0px));
  display:flex;justify-content:space-around;z-index:100;
}
@media(prefers-color-scheme:dark){
  .tab-bar{background:rgba(28,28,30,.88)}
}
.tab-btn{
  display:flex;flex-direction:column;align-items:center;gap:3px;
  color:var(--ink4);font-size:10px;font-weight:500;
  cursor:pointer;min-width:64px;border:none;background:transparent;padding:0;
  font-family:inherit;transition:color .15s;
}
.tab-btn.active{color:var(--blue)}
.tab-btn svg{width:24px;height:24px;stroke:currentColor;fill:none;stroke-width:1.7}
.tab-btn.active svg{stroke-width:2.2}
</style>
</head>
<body>

<div class="status-spacer"></div>

<header class="header">
  <div class="header-row">
    <div class="header-logo">
      <div class="logo-badge">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="#fff" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M20 7H4a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2z"/>
          <circle cx="12" cy="12" r="2"/>
          <path d="M6 12h.01M18 12h.01"/>
        </svg>
      </div>
      Budget
    </div>
    <div class="mnav">
      <button class="mnav-btn" id="prevM" aria-label="Mese precedente">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"/></svg>
      </button>
      <div class="mnav-label" id="mLabel">—</div>
      <button class="mnav-btn" id="nextM" aria-label="Mese successivo">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 18 15 12 9 6"/></svg>
      </button>
    </div>
  </div>
</header>

<div class="content">

  <!-- HOME -->
  <div id="pgHome" class="page active">
    <div class="kpi-row">
      <div class="kpi">
        <div class="kpi-lbl">
          <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="var(--grn)" stroke-width="2.5" stroke-linecap="round"><line x1="12" y1="19" x2="12" y2="5"/><polyline points="5 12 12 5 19 12"/></svg>
          Entrate
        </div>
        <div class="kpi-val grn" id="kInc">€0</div>
      </div>
      <div class="kpi">
        <div class="kpi-lbl">
          <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="var(--red)" stroke-width="2.5" stroke-linecap="round"><line x1="12" y1="5" x2="12" y2="19"/><polyline points="19 12 12 19 5 12"/></svg>
          Spese
        </div>
        <div class="kpi-val red" id="kExp">€0</div>
      </div>
      <div class="kpi">
        <div class="kpi-lbl">
          <svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="var(--blue)" stroke-width="2.5" stroke-linecap="round"><line x1="12" y1="3" x2="12" y2="21"/><path d="M8 7l4-4 4 4M8 17l4 4 4-4"/></svg>
          Saldo
        </div>
        <div class="kpi-val blue" id="kBal">€0</div>
      </div>
    </div>

    <div class="card">
      <div class="card-title">
        <svg viewBox="0 0 24 24" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M21.21 15.89A10 10 0 1 1 8 2.83"/><path d="M22 12A10 10 0 0 0 12 2v10z"/></svg>
        Distribuzione mensile
      </div>
      <div class="chart-box">
        <canvas id="mc" role="img" aria-label="Grafico a torta entrate e spese per categoria">Nessuna voce registrata.</canvas>
        <div class="no-data-msg" id="noData">
          <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="var(--ink4)" stroke-width="1.5" stroke-linecap="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/></svg>
          Nessuna voce questo mese
        </div>
      </div>
      <div id="legend"></div>
    </div>

    <div class="card">
      <div class="card-title">
        <svg viewBox="0 0 24 24" stroke-width="1.8" stroke-linecap="round"><line x1="4" y1="12" x2="20" y2="12"/><polyline points="4 8 4 16"/><polyline points="20 8 20 16"/></svg>
        Entrate vs spese
      </div>
      <div class="bar-track">
        <div class="bar-grn" id="bG" style="width:50%"></div>
        <div class="bar-red" id="bR" style="width:50%"></div>
      </div>
      <div class="bar-sub">
        <span id="bLi" style="color:var(--grn)">— %</span>
        <span id="bLe" style="color:var(--red)">— %</span>
      </div>
    </div>
  </div>

  <!-- AGGIUNGI -->
  <div id="pgAdd" class="page">
    <div class="card">
      <div class="card-title">
        <svg viewBox="0 0 24 24" stroke-width="1.8" stroke-linecap="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
        Nuova voce
      </div>
      <div class="type-seg">
        <button class="type-btn active-i" id="tI" onclick="setMode('i')">
          <svg viewBox="0 0 24 24" stroke-linecap="round"><line x1="12" y1="19" x2="12" y2="5"/><polyline points="5 12 12 5 19 12"/></svg>
          Entrata
        </button>
        <button class="type-btn" id="tE" onclick="setMode('e')">
          <svg viewBox="0 0 24 24" stroke-linecap="round"><line x1="12" y1="5" x2="12" y2="19"/><polyline points="19 12 12 19 5 12"/></svg>
          Spesa
        </button>
      </div>
      <div class="fg">
        <label>Descrizione</label>
        <input id="xD" type="text" placeholder="es. Stipendio, Affitto, Netflix…" autocomplete="off">
      </div>
      <div class="fg">
        <label>Categoria</label>
        <div class="sel-wrap"><select id="xC"></select></div>
      </div>
      <div class="fg">
        <label>Importo €</label>
        <input id="xA" type="number" inputmode="decimal" min="0" step="0.01" placeholder="0,00">
      </div>
      <button class="add-btn btn-i" id="xBtn" onclick="addTx()">
        <svg viewBox="0 0 24 24" stroke-linecap="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
        Aggiungi
      </button>
    </div>
  </div>

  <!-- MOVIMENTI -->
  <div id="pgTx" class="page">
    <div class="card">
      <div class="card-title">
        <svg viewBox="0 0 24 24" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/></svg>
        Movimenti del mese
      </div>
      <div class="tx-list" id="txList"></div>
    </div>
  </div>

  <!-- CATEGORIE -->
  <div id="pgCat" class="page">
    <div class="card">
      <div class="card-title">
        <svg viewBox="0 0 24 24" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>
        Gestisci categorie
      </div>

      <div class="cat-type-seg">
        <button class="cat-type-btn active-i" id="ctI" onclick="setCatMode('i')">Entrate</button>
        <button class="cat-type-btn" id="ctE" onclick="setCatMode('e')">Spese</button>
      </div>

      <div class="cat-section-title" id="catSecTitle">
        <svg viewBox="0 0 24 24" stroke-linecap="round"><line x1="12" y1="19" x2="12" y2="5"/><polyline points="5 12 12 5 19 12"/></svg>
        Categorie entrate
      </div>
      <div class="cat-chip-list" id="catChips"></div>

      <div class="cat-section-title" style="margin-top:4px">
        <svg viewBox="0 0 24 24" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
        Aggiungi categoria
      </div>
      <div class="cat-add-row">
        <input id="newCatInput" type="text" placeholder="Nome categoria…" autocomplete="off">
        <button class="cat-add-btn" onclick="addCustomCat()">
          <svg viewBox="0 0 24 24" stroke-linecap="round"><line x1="12" y1="5" x2="12" y2="19"/><line x1="5" y1="12" x2="19" y2="12"/></svg>
          Aggiungi
        </button>
      </div>
      <div class="info-note">Le categorie di default non possono essere eliminate. Le categorie personalizzate (tratteggiate) possono essere rimosse con la X.</div>
    </div>
  </div>

</div><!-- /content -->

<!-- TAB BAR -->
<nav class="tab-bar">
  <button class="tab-btn active" id="tabHome" onclick="goPage('home')">
    <svg viewBox="0 0 24 24" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/></svg>
    Home
  </button>
  <button class="tab-btn" id="tabAdd" onclick="goPage('add')">
    <svg viewBox="0 0 24 24" stroke-linecap="round"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
    Aggiungi
  </button>
  <button class="tab-btn" id="tabTx" onclick="goPage('tx')">
    <svg viewBox="0 0 24 24" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"/><line x1="8" y1="12" x2="21" y2="12"/><line x1="8" y1="18" x2="21" y2="18"/><line x1="3" y1="6" x2="3.01" y2="6"/><line x1="3" y1="12" x2="3.01" y2="12"/><line x1="3" y1="18" x2="3.01" y2="18"/></svg>
    Movimenti
  </button>
  <button class="tab-btn" id="tabCat" onclick="goPage('cat')">
    <svg viewBox="0 0 24 24" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/></svg>
    Categorie
  </button>
</nav>

<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<script>
// ── DATI DEFAULT ──
const IC_DEFAULT = ['Stipendio','Freelance','Investimenti','Affitti','Bonus','Rimborsi','Altro'];
const EC_DEFAULT = [
  'Spese alimentari','Spese casa','Abbonamenti','Shopping',
  'Carburante auto','Auto','Vacanze','Salute','Trasporti',
  'Utenze','Svago','Istruzione','Risparmio','Altro'
];

const IC_COLORS = ['#34c759','#30d158','#5ac87e','#1a7a32','#096b26','#0a5020','#248a3d'];
const EC_COLORS = ['#ff9500','#ff6b35','#af52de','#ff2d55','#007aff','#5ac8fa','#ffcc00','#ff3b30','#32ade6','#64d2ff','#30d158','#ff9f0a','#5856d6','#8e8e93'];

const MN = ['Gennaio','Febbraio','Marzo','Aprile','Maggio','Giugno','Luglio','Agosto','Settembre','Ottobre','Novembre','Dicembre'];
const isDark = matchMedia('(prefers-color-scheme:dark)').matches;
const BORDER = isDark ? '#1c1c1e' : '#ffffff';

// ── STATO ──
let mode = 'i', catMode = 'i', curDate = new Date(), db = {}, customCats = {i:[], e:[]};

function loadData() {
  try { db = JSON.parse(localStorage.getItem('bm_db') || '{}'); } catch(e) { db = {}; }
  try { customCats = JSON.parse(localStorage.getItem('bm_cats') || '{"i":[],"e":[]}'); } catch(e) { customCats = {i:[],e:[]}; }
}
function saveDB() { try { localStorage.setItem('bm_db', JSON.stringify(db)); } catch(e) {} }
function saveCats() { try { localStorage.setItem('bm_cats', JSON.stringify(customCats)); } catch(e) {} }

function getCats(m) { return m === 'i' ? [...IC_DEFAULT, ...customCats.i] : [...EC_DEFAULT, ...customCats.e]; }
function isDefault(m, name) { return m === 'i' ? IC_DEFAULT.includes(name) : EC_DEFAULT.includes(name); }

function mkey(d) { return d.getFullYear() + '-' + d.getMonth(); }
function mdata() { return db[mkey(curDate)] || {tx:[]}; }
function fmt(v) { return '€' + v.toLocaleString('it-IT', {minimumFractionDigits:2, maximumFractionDigits:2}); }
function fmtS(v) { return v >= 10000 ? '€' + (v/1000).toFixed(1) + 'k' : '€' + Math.round(v).toLocaleString('it-IT'); }

// Icone per categoria
const CAT_ICONS = {
  'Stipendio': '<path d="M12 1v22M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/>',
  'Freelance': '<rect x="2" y="7" width="20" height="14" rx="2"/><path d="M16 21V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v16"/>',
  'Investimenti': '<polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/>',
  'Affitti': '<path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/>',
  'Bonus': '<circle cx="12" cy="12" r="10"/><path d="M8.56 2.75c4.37 6.03 6.02 9.42 8.03 17.72m2.54-15.38c-3.72 4.35-8.94 5.66-16.88 5.85m19.5 1.9c-3.5-.93-6.63-.82-8.94 0-2.58.92-5.01 2.86-7.44 6.32"/>',
  'Rimborsi': '<polyline points="23 4 23 10 17 10"/><path d="M20.49 15a9 9 0 1 1-.37-4.03"/>',
  'Spese alimentari': '<path d="M6 2v6a6 6 0 0 0 12 0V2"/><line x1="6" y1="2" x2="6" y2="22"/><line x1="18" y1="2" x2="18" y2="22"/>',
  'Spese casa': '<path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9 22 9 12 15 12 15 22"/>',
  'Abbonamenti': '<path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"/>',
  'Shopping': '<path d="M6 2L3 6v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2V6l-3-4z"/><line x1="3" y1="6" x2="21" y2="6"/><path d="M16 10a4 4 0 0 1-8 0"/>',
  'Carburante auto': '<path d="M3 22V8l7-6 7 6v14H3z"/><line x1="3" y1="22" x2="21" y2="22"/><rect x="8" y="14" width="6" height="8"/><path d="M17 8h2a2 2 0 0 1 2 2v2a2 2 0 0 1-2 2h-2V8z"/>',
  'Auto': '<rect x="1" y="3" width="15" height="13" rx="2"/><path d="M16 8h4l3 3v5h-7V8z"/><circle cx="5.5" cy="18.5" r="2.5"/><circle cx="18.5" cy="18.5" r="2.5"/>',
  'Vacanze': '<circle cx="12" cy="12" r="10"/><line x1="2" y1="12" x2="22" y2="12"/><path d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"/>',
  'Salute': '<path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/>',
  'Trasporti': '<rect x="1" y="3" width="15" height="13" rx="2"/><path d="M16 8h4l3 3v5h-7V8z"/><circle cx="5.5" cy="18.5" r="2.5"/><circle cx="18.5" cy="18.5" r="2.5"/>',
  'Utenze': '<path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/>',
  'Svago': '<circle cx="12" cy="12" r="10"/><path d="M8.56 2.75c4.37 6.03 6.02 9.42 8.03 17.72M10.5 2.5c4 7 5.5 10 8.5 16.5m-9-16.5c3 5.5 4 9 4 13.5m-4-13.5c1.5 4 2 7 2 10.5"/>',
  'Istruzione': '<path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/>',
  'Risparmio': '<line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/>',
  'Altro': '<circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/>',
};
function getCatIcon(name) {
  const paths = CAT_ICONS[name] || CAT_ICONS['Altro'];
  return `<svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round">${paths}</svg>`;
}

// ── NAVIGAZIONE ──
function goPage(p) {
  const pages = ['pgHome','pgAdd','pgTx','pgCat'];
  const tabs  = ['tabHome','tabAdd','tabTx','tabCat'];
  const map   = {home:'pgHome', add:'pgAdd', tx:'pgTx', cat:'pgCat'};
  const tmap  = {home:'tabHome', add:'tabAdd', tx:'tabTx', cat:'tabCat'};
  pages.forEach(id => document.getElementById(id).classList.remove('active'));
  tabs.forEach(id  => document.getElementById(id).classList.remove('active'));
  document.getElementById(map[p]).classList.add('active');
  document.getElementById(tmap[p]).classList.add('active');
  window.scrollTo(0,0);
}

// ── MODALITÀ ENTRATA/SPESA ──
function setMode(m) {
  mode = m;
  const cats = getCats(m);
  const sel = document.getElementById('xC');
  sel.innerHTML = cats.map(c => `<option>${c}</option>`).join('');
  document.getElementById('tI').className = 'type-btn' + (m === 'i' ? ' active-i' : '');
  document.getElementById('tE').className = 'type-btn' + (m === 'e' ? ' active-e' : '');
  const btn = document.getElementById('xBtn');
  btn.className = 'add-btn ' + (m === 'i' ? 'btn-i' : 'btn-e');
}

// ── AGGIUNGI TRANSAZIONE ──
function addTx() {
  const desc = document.getElementById('xD').value.trim();
  const cat  = document.getElementById('xC').value;
  const amt  = parseFloat(document.getElementById('xA').value);
  if (!desc || isNaN(amt) || amt <= 0) { alert('Inserisci descrizione e importo valido.'); return; }
  const k = mkey(curDate);
  if (!db[k]) db[k] = {tx:[]};
  db[k].tx.push({id: Date.now(), t: mode, d: desc, c: cat, a: amt});
  saveDB();
  document.getElementById('xD').value = '';
  document.getElementById('xA').value = '';
  render();
  goPage('home');
}

// ── ELIMINA TRANSAZIONE ──
function delTx(id) {
  const k = mkey(curDate);
  if (!db[k]) return;
  db[k].tx = db[k].tx.filter(t => t.id !== id);
  saveDB(); render();
}

// ── GESTIONE CATEGORIE ──
let catModeState = 'i';
function setCatMode(m) {
  catModeState = m;
  document.getElementById('ctI').className = 'cat-type-btn' + (m === 'i' ? ' active-i' : '');
  document.getElementById('ctE').className = 'cat-type-btn' + (m === 'e' ? ' active-e' : '');
  const titleEl = document.getElementById('catSecTitle');
  if (m === 'i') {
    titleEl.innerHTML = `<svg viewBox="0 0 24 24" stroke-linecap="round" style="width:15px;height:15px;stroke:var(--grn);fill:none"><line x1="12" y1="19" x2="12" y2="5"/><polyline points="5 12 12 5 19 12"/></svg> Categorie entrate`;
  } else {
    titleEl.innerHTML = `<svg viewBox="0 0 24 24" stroke-linecap="round" style="width:15px;height:15px;stroke:var(--red);fill:none"><line x1="12" y1="5" x2="12" y2="19"/><polyline points="19 12 12 19 5 12"/></svg> Categorie spese`;
  }
  renderCatChips();
}

function renderCatChips() {
  const m = catModeState;
  const defaults = m === 'i' ? IC_DEFAULT : EC_DEFAULT;
  const customs  = customCats[m];
  const all = [...defaults.map(n => ({n, custom:false})), ...customs.map(n => ({n, custom:true}))];
  const color = m === 'i' ? 'var(--grn)' : 'var(--red)';
  document.getElementById('catChips').innerHTML = all.map(({n, custom}) => `
    <div class="cat-chip ${custom ? 'custom' : ''}" style="border-color:${custom ? color : 'var(--sep2)'}">
      ${getCatIcon(n)}
      <span style="font-size:13px">${n}</span>
      ${custom ? `<button class="cat-chip-del" onclick="delCat('${m}','${n}')" aria-label="Elimina categoria ${n}">
        <svg viewBox="0 0 24 24" stroke-linecap="round"><line x1="18" y1="6" x2="6" y2="18"/><line x1="6" y1="6" x2="18" y2="18"/></svg>
      </button>` : ''}
    </div>
  `).join('');
}

function addCustomCat() {
  const input = document.getElementById('newCatInput');
  const name = input.value.trim();
  if (!name) return;
  const m = catModeState;
  const all = getCats(m);
  if (all.map(c => c.toLowerCase()).includes(name.toLowerCase())) {
    alert('Categoria già esistente.');
    return;
  }
  customCats[m].push(name);
  saveCats();
  input.value = '';
  renderCatChips();
  setMode(mode);
}

function delCat(m, name) {
  customCats[m] = customCats[m].filter(c => c !== name);
  saveCats();
  renderCatChips();
  setMode(mode);
}

// ── RENDER ──
let chart = null;
function render() {
  const tx = mdata().tx;
  const inc = tx.filter(t => t.t === 'i').reduce((s, t) => s + t.a, 0);
  const exp = tx.filter(t => t.t === 'e').reduce((s, t) => s + t.a, 0);
  const bal = inc - exp;

  document.getElementById('mLabel').textContent = MN[curDate.getMonth()] + ' ' + curDate.getFullYear();
  document.getElementById('kInc').textContent = fmtS(inc);
  document.getElementById('kExp').textContent = fmtS(exp);
  const bEl = document.getElementById('kBal');
  bEl.textContent = (bal >= 0 ? '+' : '-') + fmtS(Math.abs(bal));
  bEl.className = 'kpi-val ' + (bal >= 0 ? 'grn' : 'red');

  const tot = inc + exp;
  const ip = tot > 0 ? Math.round(inc / tot * 100) : 50;
  const ep = 100 - ip;
  document.getElementById('bG').style.width = ip + '%';
  document.getElementById('bR').style.width = ep + '%';
  document.getElementById('bLi').textContent = 'Entrate ' + ip + '%';
  document.getElementById('bLe').textContent = 'Spese ' + ep + '%';

  // Raggruppa per categoria
  const im = {}, em = {};
  tx.forEach(t => { if (t.t === 'i') im[t.c] = (im[t.c]||0) + t.a; else em[t.c] = (em[t.c]||0) + t.a; });

  const labs = [], vals = [], cols = [];
  const allIC = getCats('i'), allEC = getCats('e');
  Object.entries(im).forEach(([c,v]) => { const i = allIC.indexOf(c); labs.push(c); vals.push(v); cols.push(IC_COLORS[i >= 0 ? i % IC_COLORS.length : 0]); });
  Object.entries(em).forEach(([c,v]) => { const i = allEC.indexOf(c); labs.push(c); vals.push(v); cols.push(EC_COLORS[i >= 0 ? i % EC_COLORS.length : 0]); });

  const noD = document.getElementById('noData');
  if (chart) chart.destroy();

  if (vals.length) {
    noD.style.display = 'none';
    chart = new Chart(document.getElementById('mc'), {
      type: 'doughnut',
      data: { labels: labs, datasets: [{ data: vals, backgroundColor: cols, borderWidth: 2.5, borderColor: BORDER }] },
      options: {
        responsive: true, maintainAspectRatio: false, cutout: '62%',
        plugins: {
          legend: { display: false },
          tooltip: { callbacks: { label: ctx => ` ${ctx.label}: ${fmt(ctx.parsed)}` }, padding: 10, cornerRadius: 8 }
        }
      }
    });

    const ie = Object.entries(im), ee = Object.entries(em);
    let h = '';
    if (ie.length) {
      const iTotal = ie.reduce((s,[,v]) => s+v, 0);
      h += `<div class="leg-sec" style="color:var(--grn)"><svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="var(--grn)" stroke-width="2.5" stroke-linecap="round"><line x1="12" y1="19" x2="12" y2="5"/><polyline points="5 12 12 5 19 12"/></svg>Entrate</div>`;
      ie.forEach(([c,v]) => {
        const idx = allIC.indexOf(c);
        const col = IC_COLORS[idx >= 0 ? idx % IC_COLORS.length : 0];
        const pct = iTotal > 0 ? Math.round(v/iTotal*100) : 0;
        h += `<div class="leg-row"><span class="leg-dot" style="background:${col}"></span><span class="leg-name">${c}</span><span class="leg-pct">${pct}%</span><span class="leg-val">${fmt(v)}</span></div>`;
      });
    }
    if (ee.length) {
      const eTotal = ee.reduce((s,[,v]) => s+v, 0);
      h += `<div class="leg-sec" style="color:var(--red);margin-top:12px"><svg width="11" height="11" viewBox="0 0 24 24" fill="none" stroke="var(--red)" stroke-width="2.5" stroke-linecap="round"><line x1="12" y1="5" x2="12" y2="19"/><polyline points="19 12 12 19 5 12"/></svg>Spese</div>`;
      ee.forEach(([c,v]) => {
        const idx = allEC.indexOf(c);
        const col = EC_COLORS[idx >= 0 ? idx % EC_COLORS.length : 0];
        const pct = eTotal > 0 ? Math.round(v/eTotal*100) : 0;
        h += `<div class="leg-row"><span class="leg-dot" style="background:${col}"></span><span class="leg-name">${c}</span><span class="leg-pct">${pct}%</span><span class="leg-val">${fmt(v)}</span></div>`;
      });
    }
    document.getElementById('legend').innerHTML = h;
  } else {
    noD.style.display = 'flex';
    document.getElementById('legend').innerHTML = '';
  }

  // Lista movimenti
  const el = document.getElementById('txList');
  if (!tx.length) {
    el.innerHTML = `<div class="tx-empty">
      <svg width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="var(--ink4)" stroke-width="1.4" stroke-linecap="round"><rect x="2" y="3" width="20" height="14" rx="2"/><line x1="8" y1="21" x2="16" y2="21"/><line x1="12" y1="17" x2="12" y2="21"/></svg>
      Nessun movimento questo mese
    </div>`;
    return;
  }
  const allEC2 = getCats('e'), allIC2 = getCats('i');
  el.innerHTML = [...tx].sort((a,b) => b.id - a.id).map(t => {
    const isI = t.t === 'i';
    const allCats = isI ? allIC2 : allEC2;
    const colArr = isI ? IC_COLORS : EC_COLORS;
    const idx = allCats.indexOf(t.c);
    const col = colArr[idx >= 0 ? idx % colArr.length : 0];
    const iconSvg = getCatIcon(t.c);
    return `<div class="tx-row">
      <div class="cat-icon" style="background:${col}22;color:${col}">${iconSvg}</div>
      <div class="tx-body">
        <div class="tx-name">${t.d}</div>
        <div class="tx-cat">${t.c}</div>
      </div>
      <div class="tx-amt ${t.t}">${isI ? '+' : '-'}${fmt(t.a)}</div>
      <button class="tx-del" onclick="delTx(${t.id})" aria-label="Elimina voce">
        <svg viewBox="0 0 24 24" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/><path d="M10 11v6M14 11v6"/><path d="M9 6V4a1 1 0 0 1 1-1h4a1 1 0 0 1 1 1v2"/></svg>
      </button>
    </div>`;
  }).join('');
}

// ── INIT ──
document.getElementById('prevM').onclick = () => { curDate = new Date(curDate.getFullYear(), curDate.getMonth()-1, 1); render(); };
document.getElementById('nextM').onclick = () => { curDate = new Date(curDate.getFullYear(), curDate.getMonth()+1, 1); render(); };

// Enter su form aggiungi
document.getElementById('xA').addEventListener('keydown', e => { if (e.key === 'Enter') addTx(); });
document.getElementById('newCatInput').addEventListener('keydown', e => { if (e.key === 'Enter') addCustomCat(); });

loadData();
setMode('i');
setCatMode('i');
render();
</script>
</body>
</html>
