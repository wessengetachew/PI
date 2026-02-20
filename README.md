<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Polar vs Cartesian · Gap Analysis</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,400;0,600;1,400&family=JetBrains+Mono:wght@300;400;700&family=DM+Sans:wght@300;400;500&display=swap');

/* ═══════════════════════════════════════════
   DESIGN SYSTEM
   Mathematical clarity · Educational depth
   ═══════════════════════════════════════════ */
:root {
  /* Core palette */
  --bg:        #0e1119;
  --bg2:       #121722;
  --surface:   rgba(255,255,255,0.028);
  --surface2:  rgba(255,255,255,0.045);

  /* Blue — primary */
  --blue:      #5b9cf6;
  --blue-dim:  rgba(91,156,246,0.18);
  --blue-faint:rgba(91,156,246,0.07);
  --border:    rgba(91,156,246,0.18);

  /* Gold — secondary */
  --gold:      #e8c547;
  --gold-dim:  rgba(232,197,71,0.18);
  --gold-faint:rgba(232,197,71,0.06);
  --gold-border:rgba(232,197,71,0.2);

  /* Semantic */
  --green:     #5ebd78;
  --teal:      #4ecdc4;
  --violet:    #b48eff;
  --red:       #f87171;
  --orange:    #fb923c;

  /* Text */
  --text:      #dde5f4;
  --text-dim:  rgba(180,200,235,0.5);
  --text-faint:rgba(150,175,215,0.35);

  /* Pink kept for legacy JS color references but softened */
  --pink: #5b9cf6;
}

* { margin:0; padding:0; box-sizing:border-box; }

body {
  background: var(--bg);
  color: var(--text);
  font-family: 'DM Sans', sans-serif;
  min-height: 100vh;
  overflow-x: hidden;
  /* Subtle grid-paper background */
  background-image:
    linear-gradient(rgba(91,156,246,0.025) 1px, transparent 1px),
    linear-gradient(90deg, rgba(91,156,246,0.025) 1px, transparent 1px);
  background-size: 32px 32px;
  background-position: center center;
}

/* ═══════════════════════════════════════════
   DESKTOP LAYOUT — 3-column dashboard
   ═══════════════════════════════════════════ */
.page-shell {
  display: grid;
  grid-template-columns: 320px 1fr;
  grid-template-rows: auto 1fr;
  grid-template-areas:
    "topbar topbar"
    "left   center";
  min-height: 100vh;
  max-width: 1920px;
  margin: 0 auto;
}

/* Top bar */
.topbar {
  grid-area: topbar;
  display: flex;
  align-items: center;
  gap: 16px;
  padding: 10px 24px;
  border-bottom: 1px solid rgba(91,156,246,0.12);
  background: rgba(14,17,25,0.96);
  backdrop-filter: blur(16px);
  position: sticky; top: 0; z-index: 100;
}
.topbar-title {
  font-family: 'Lora', serif;
  font-size: clamp(1rem, 2vw, 1.4rem);
  color: var(--blue);
  letter-spacing: -0.01em;
  white-space: nowrap;
}
.topbar-author {
  font-size: .56rem; text-transform: uppercase; letter-spacing: .22em;
  color: rgba(232,197,71,.4); white-space: nowrap; font-family:'JetBrains Mono',monospace;
}
.topbar-actions { display: flex; gap: 8px; align-items: center; flex-shrink: 0; }
.topbar-kb {
  font-size: .5rem; color: rgba(91,156,246,.3);
  letter-spacing: .1em; white-space: nowrap; font-family:'JetBrains Mono',monospace;
}

/* Left sidebar */
.col-left {
  grid-area: left;
  border-right: 1px solid rgba(91,156,246,.08);
  overflow-y: auto;
  padding: 16px 14px 40px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  background: rgba(14,17,25,.7);
}

/* Center — main visualization */
.col-center {
  grid-area: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px 40px 40px;
  gap: 14px;
  overflow-y: auto;
  min-width: 0;
}

/* Center all direct children with consistent max-width */
.col-center > * {
  width: 100%;
  max-width: 900px;
}
.col-center > .sphere-wrap,
.col-center > .dual-wave-row,
.col-center > #liveStats {
  max-width: 900px;
}

/* Right sidebar — collapsed (content moved to center) */
.col-right { display: none; }

/* Sidebar section headers */
.sidebar-section {
  font-size: .48rem; text-transform: uppercase; letter-spacing: .24em;
  color: rgba(91,156,246,.35); padding: 0 2px; margin-top: 4px;
  font-family:'JetBrains Mono',monospace;
}
.sidebar-section.gold { color: rgba(232,197,71,.35); }

/* Remove old max-width constraints from panels inside sidebars */
.col-left .panel,
.col-left .harm-panel,
.col-left .coord-panel,
.col-right .panel,
.col-right .gap-panel,
.col-right .harm-panel {
  max-width: none;
  width: 100%;
  margin-top: 0;
}

/* Preset dropdown in topbar context */
.topbar .preset-drop-wrap {
  max-width: 260px;
  margin: 0;
  flex-shrink: 0;
}
.topbar .preset-drop-header {
  padding: 7px 12px;
}

/* Intro block in left col */
.col-left .intro {
  max-width: none;
  width: 100%;
  margin-bottom: 0;
  border-left: 2px solid rgba(91,156,246,.2);
  padding-left: 12px;
}
.col-left .intro h1 { display: none; }

/* Stats in center */
.col-center .live-stats {
  grid-template-columns: repeat(4, 1fr);
}

/* ═══════════════════════════════════════════
   MOBILE — single column
   ═══════════════════════════════════════════ */
@media (max-width: 900px) {
  .page-shell {
    grid-template-columns: 1fr;
    grid-template-areas:
      "topbar"
      "center"
      "left";
  }
  .topbar {
    grid-template-columns: 1fr auto;
    padding: 8px 14px;
    gap: 10px;
  }
  .col-left {
    border: none;
    border-top: 1px solid rgba(91,156,246,.08);
    padding: 14px 12px 28px;
  }
  .col-center { padding: 14px 12px 20px; }
  .col-center .live-stats { grid-template-columns: repeat(2,1fr); }
  .dual-wave-row { flex-direction: column !important; }
  .dual-wave-row > * { max-width: 100% !important; }
}

@media (max-width: 1200px) and (min-width: 901px) {
  .page-shell {
    grid-template-columns: 280px 1fr;
  }
}

/* ── Presets ─────────────────────── */
.presets-wrap { max-width: 580px; width: 100%; margin-bottom: 16px; }
.pg-label {
  font-size:.52rem; text-transform:uppercase; letter-spacing:.18em;
  color:rgba(91,156,246,.5); margin-bottom:5px; margin-top:8px; margin-left:2px;
  font-family:'JetBrains Mono',monospace;
}
.pg-label.gold { color:rgba(232,197,71,.6); }
.presets { display:flex; flex-wrap:wrap; gap:5px; }
.preset-btn {
  font-family:'JetBrains Mono',monospace; font-size:.6rem;
  letter-spacing:.07em; text-transform:uppercase;
  padding:4px 10px; border-radius:4px; cursor:pointer;
  border:1px solid rgba(91,156,246,.2); background:rgba(255,255,255,.02);
  color:rgba(180,200,235,.55); transition:all .15s;
}
.preset-btn:hover { border-color:var(--blue); color:var(--blue); }
.preset-btn.active { background:var(--blue); border-color:var(--blue); color:#000; }
.preset-btn.gap-active { background:var(--gold); border-color:var(--gold); color:#000; }

/* ── Sphere canvas ───────────────── */
.sphere-wrap {
  width:100%;
  display:flex; flex-direction:column; align-items:center; gap:10px;
}
#polarCanvas {
  display:block; border-radius:50%;
  box-shadow: 0 0 80px rgba(91,156,246,.1), 0 0 200px rgba(91,156,246,.04),
              0 4px 32px rgba(0,0,0,.5);
  cursor:grab;
}
#polarCanvas:active { cursor:grabbing; }

.formula-box {
  border:1px solid var(--gold); border-radius:6px;
  padding:8px 18px; background:rgba(232,197,71,.05);
  font-family:'JetBrains Mono',monospace;
  font-size:clamp(.72rem,2.3vw,.92rem); color:var(--text);
  text-align:center;
}
.formula-box em { color:var(--gold); font-style:normal; font-weight:700; }
#cartCanvas { display:block; border-radius:4px; }

/* ── Live Stats Bar ──────────────── */
.live-stats {
  width:100%;
  background:rgba(232,197,71,.03); border:1px solid rgba(232,197,71,.12);
  border-radius:8px; padding:8px 14px;
  display:grid; grid-template-columns:repeat(4,1fr); gap:6px 10px;
}
.stat-cell { display:flex; flex-direction:column; gap:2px; }
.stat-lbl { font-size:.46rem; text-transform:uppercase; letter-spacing:.12em; color:rgba(232,197,71,.4); font-family:'JetBrains Mono',monospace; }
.stat-val { font-size:.68rem; color:var(--gold); font-weight:700; font-family:'JetBrains Mono',monospace; }

/* ── Rotation controls ───────────── */
.rot-controls {
  display:flex; align-items:center; gap:12px;
  flex-wrap:wrap; justify-content:center; margin-top:2px;
}
.rot-hint { font-size:.58rem; color:rgba(91,156,246,.45); letter-spacing:.07em; font-family:'JetBrains Mono',monospace; }

/* ── Panels ──────────────────────── */
.panels { display:flex; flex-direction:column; gap:10px; width:100%; }
.panel { background:var(--surface); border:1px solid var(--border); border-radius:8px; padding:14px 16px; }
.panel.gold-panel { border-color:var(--gold-border); }
.panel-title {
  font-size:.52rem; text-transform:uppercase; letter-spacing:.2em;
  color:var(--blue); margin-bottom:12px;
  display:flex; align-items:center; gap:8px;
  font-family:'JetBrains Mono',monospace;
}
.panel-title.gold { color:var(--gold); }
.panel-title::after { content:''; flex:1; height:1px; background:var(--border); }
.panel-title.gold::after { background:var(--gold-border); }
.grid-2 { display:grid; grid-template-columns:1fr 1fr; gap:10px 18px; }
.cg { display:flex; flex-direction:column; gap:4px; }
label { font-size:.58rem; color:rgba(180,200,235,.65); text-transform:uppercase; letter-spacing:.1em; font-family:'JetBrains Mono',monospace; }
input[type=range] {
  -webkit-appearance:none; width:100%; height:2px;
  background:rgba(91,156,246,.2); border-radius:2px; outline:none; cursor:pointer;
}
input[type=range]::-webkit-slider-thumb {
  -webkit-appearance:none; width:12px; height:12px; border-radius:50%;
  background:var(--blue); cursor:pointer;
}
.vd { font-size:.68rem; color:var(--gold); text-align:right; font-family:'JetBrains Mono',monospace; }

/* ── Playback ────────────────────── */
.pb-row { display:grid; grid-template-columns:1fr auto; gap:14px; align-items:end; }
.btn-row { display:flex; gap:8px; flex-wrap:wrap; }
button.ctrl {
  font-family:'JetBrains Mono',monospace; font-size:.65rem;
  letter-spacing:.08em; text-transform:uppercase;
  padding:7px 14px; border-radius:4px; cursor:pointer;
  transition:all .15s; border:1px solid rgba(91,156,246,.35);
  background:transparent; color:var(--blue); white-space:nowrap;
}
button.ctrl:hover, button.ctrl.on { background:var(--blue); color:#000; }
button.ctrl.gold-ctrl { border-color:rgba(232,197,71,.4); color:var(--gold); }
button.ctrl.gold-ctrl:hover, button.ctrl.gold-ctrl.on { background:var(--gold); color:#000; }
.speed-sub { font-size:.54rem; color:rgba(232,197,71,.45); margin-top:-2px; font-family:'JetBrains Mono',monospace; }
.qrow { display:flex; gap:5px; flex-wrap:wrap; margin-top:10px; align-items:center; }
.qlabel { font-size:.55rem; color:rgba(150,175,215,.5); text-transform:uppercase; letter-spacing:.1em; font-family:'JetBrains Mono',monospace; }

/* loop toggle */
.loop-row {
  display:flex; align-items:center; gap:10px; margin-top:8px;
  padding-top:8px; border-top:1px solid rgba(91,156,246,.08);
}
.loop-check {
  -webkit-appearance:none; width:30px; height:16px; border-radius:8px;
  background:rgba(91,156,246,.12); border:1px solid rgba(91,156,246,.25);
  cursor:pointer; position:relative; transition:background .2s; flex-shrink:0;
}
.loop-check:checked { background:var(--blue); border-color:var(--blue); }
.loop-check::after {
  content:''; position:absolute; top:2px; left:2px;
  width:10px; height:10px; border-radius:50%; background:#fff;
  transition:transform .2s; box-shadow:0 1px 4px rgba(0,0,0,.4);
}
.loop-check:checked::after { transform:translateX(14px); }
.loop-lbl { font-size:.58rem; color:rgba(180,200,235,.65); text-transform:uppercase; letter-spacing:.1em; font-family:'JetBrains Mono',monospace; }
.loop-state { font-size:.54rem; color:rgba(232,197,71,.4); font-family:'JetBrains Mono',monospace; }

/* ── Gap Decomposition Panel ─────── */
.gap-panel {
  width:100%; margin-top:0;
  background:var(--surface); border:1px solid var(--gold-border);
  border-radius:8px; overflow:hidden;
}
.gap-header {
  display:flex; align-items:center; justify-content:space-between;
  padding:12px 16px; cursor:pointer; user-select:none;
  border-bottom:1px solid transparent; transition:border-color .2s;
}
.gap-header.open { border-bottom-color:var(--gold-border); }
.gap-header-title {
  font-size:.54rem; text-transform:uppercase; letter-spacing:.2em; color:var(--gold);
  font-family:'JetBrains Mono',monospace;
}
.gap-chevron { font-size:.7rem; color:var(--gold); transition:transform .25s; }
.gap-chevron.open { transform:rotate(180deg); }
.gap-body { display:none; padding:14px 16px 16px; }
.gap-body.open { display:block; }

/* Gap controls grid */
.gap-ctrl-grid { display:grid; grid-template-columns:1fr 1fr; gap:10px 18px; margin-bottom:12px; }
.gap-ctrl-grid label { color:rgba(180,200,235,.6); }
.gap-ctrl-grid input[type=range] { background:rgba(232,197,71,.15); }
.gap-ctrl-grid input[type=range]::-webkit-slider-thumb { background:var(--gold); }
.gap-vd { font-size:.68rem; color:var(--gold); text-align:right; font-family:'JetBrains Mono',monospace; }

.gap-filter-row { display:flex; gap:7px; flex-wrap:wrap; align-items:center; margin-bottom:10px; }
.gap-filter-lbl { font-size:.54rem; color:rgba(232,197,71,.45); text-transform:uppercase; letter-spacing:.12em; font-family:'JetBrains Mono',monospace; }
.gap-filter-btn {
  font-family:'JetBrains Mono',monospace; font-size:.54rem; letter-spacing:.06em;
  text-transform:uppercase; padding:3px 8px; border-radius:4px; cursor:pointer;
  border:1px solid rgba(232,197,71,.2); background:transparent; color:rgba(150,175,100,.7); transition:all .12s;
}
.gap-filter-btn:hover { border-color:var(--gold); color:var(--gold); }
.gap-filter-btn.active { background:var(--gold); color:#000; border-color:var(--gold); }

.gap-run-row { display:flex; gap:7px; margin-bottom:12px; flex-wrap:wrap; }

/* Status */
.gap-status {
  font-size:.58rem; color:rgba(150,200,150,.8); min-height:1.4em;
  border-left:2px solid var(--gold); padding-left:8px;
  margin-bottom:10px; line-height:1.7; font-family:'JetBrains Mono',monospace;
}

/* Progress bar */
.gap-progress { height:2px; background:rgba(232,197,71,.1); border-radius:2px; margin-bottom:10px; overflow:hidden; display:none; }
.gap-progress-fill { height:100%; background:var(--gold); border-radius:2px; width:0%; transition:width .1s; }

/* Live stats grid for gap */
.gap-live-stats {
  display:grid; grid-template-columns:repeat(3,1fr); gap:5px; margin-bottom:12px;
}
.gap-stat {
  background:rgba(232,197,71,.03); border:1px solid rgba(232,197,71,.08);
  border-radius:5px; padding:5px 7px;
}
.gap-stat-lbl { font-size:.44rem; text-transform:uppercase; letter-spacing:.1em; color:rgba(232,197,71,.38); margin-bottom:2px; font-family:'JetBrains Mono',monospace; }
.gap-stat-val { font-size:.64rem; color:var(--gold); font-weight:700; word-break:break-all; font-family:'JetBrains Mono',monospace; }

/* Chart tabs */
.gap-tabs { display:flex; gap:4px; margin-bottom:10px; flex-wrap:wrap; }
.gap-tab {
  font-family:'JetBrains Mono',monospace; font-size:.54rem; letter-spacing:.06em;
  text-transform:uppercase; padding:4px 9px; border-radius:4px; cursor:pointer;
  border:1px solid rgba(232,197,71,.2); background:transparent; color:rgba(150,175,100,.7); transition:all .12s;
}
.gap-tab:hover { border-color:var(--gold); color:var(--gold); }
.gap-tab.active { background:var(--gold); color:#000; border-color:var(--gold); }

#gapCanvas { display:block; border-radius:4px; width:100%; background:#0e1119; }

.gap-legend { display:flex; flex-wrap:wrap; gap:5px; margin-top:7px; }
.gap-leg { display:flex; align-items:center; gap:4px; font-size:.52rem; color:rgba(150,175,115,.7); font-family:'JetBrains Mono',monospace; }
.gap-leg-sw { width:8px; height:8px; border-radius:2px; flex-shrink:0; }

/* Wave scrubber slider */
.wave-scrubber-wrap {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 7px;
  padding: 7px 10px;
  background: rgba(91,156,246,.05);
  border: 1px solid rgba(91,156,246,.14);
  border-radius: 6px;
}
.wave-scrubber-label {
  font-size: .48rem;
  text-transform: uppercase;
  letter-spacing: .14em;
  color: rgba(91,156,246,.5);
  font-family: 'JetBrains Mono', monospace;
  white-space: nowrap;
  flex-shrink: 0;
}
.wave-scrubber-wrap input[type=range] {
  flex: 1;
  accent-color: #5b9cf6;
  cursor: pointer;
  height: 4px;
}
.wave-scrubber-pct {
  font-size: .55rem;
  color: var(--blue);
  font-family: 'JetBrains Mono', monospace;
  min-width: 38px;
  text-align: right;
  flex-shrink: 0;
}
.probe-color-swatch {
  display: inline-block;
  width: 10px; height: 10px;
  border-radius: 2px;
  vertical-align: middle;
  margin-right: 4px;
  flex-shrink: 0;
}

/* Export row — styled like reference file */
.gd-export-row {
  display: flex;
  gap: 7px;
  flex-wrap: wrap;
  margin-top: 12px;
  padding-top: 12px;
  border-top: 1px solid rgba(232,197,71,.12);
}
.gd-export-btn {
  flex: 1;
  min-width: 80px;
  padding: 8px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: .6rem;
  font-weight: 700;
  font-family: 'JetBrains Mono', monospace;
  letter-spacing: .06em;
  text-transform: uppercase;
  transition: all .2s;
}
.gd-export-btn:hover { transform: translateY(-1px); box-shadow: 0 3px 10px rgba(0,0,0,.4); }
.gd-export-btn.csv  { background: linear-gradient(135deg,#64ff64,#32aa32); color:#000; }
.gd-export-btn.csv:hover  { box-shadow: 0 2px 10px rgba(100,255,100,.3); }
.gd-export-btn.dom  { background: linear-gradient(135deg,#3b82f6,#1e40af); color:#fff; }
.gd-export-btn.dom:hover  { box-shadow: 0 2px 10px rgba(59,130,246,.35); }
.gd-export-btn.cvs  { background: linear-gradient(135deg,#ef4444,#991b1b); color:#fff; }
.gd-export-btn.cvs:hover  { box-shadow: 0 2px 10px rgba(239,68,68,.35); }

/* Standalone Gap Ratio Analysis Panel */
.gd-ratio-panel {
  width: 100%;
  max-width: 900px;
  background: rgba(232,197,71,.025);
  border: 1px solid rgba(232,197,71,.2);
  border-radius: 10px;
  overflow: hidden;
  display: none; /* shown after gap data is computed */
}
.gd-ratio-panel-header {
  padding: 14px 18px 10px;
  border-bottom: 1px solid rgba(232,197,71,.12);
}
.gd-ratio-panel-title {
  font-family: 'Lora', serif;
  font-size: clamp(.9rem,1.8vw,1.15rem);
  color: var(--gold);
  margin-bottom: 6px;
}
.gd-ratio-panel-desc {
  background: rgba(232,197,71,.07);
  border-left: 3px solid rgba(232,197,71,.4);
  border-radius: 4px;
  padding: 10px 14px;
  font-size: .68rem;
  color: rgba(200,210,180,.75);
  line-height: 1.7;
  font-family: 'DM Sans', sans-serif;
}
.gd-ratio-canvas-wrap {
  width: 100%;
  padding: 12px 14px 0;
}
.gd-ratio-canvas-wrap canvas {
  display: block;
  width: 100%;
  height: auto;
  border-radius: 6px;
  background: #060608;
}
.gd-ratio-table-wrap {
  padding: 12px 14px 16px;
}
.gd-ratio-table-title {
  font-size: .52rem;
  text-transform: uppercase;
  letter-spacing: .18em;
  color: rgba(232,197,71,.45);
  font-family: 'JetBrains Mono', monospace;
  margin-bottom: 8px;
}
.gd-ratio-table {
  width: 100%;
  border-collapse: collapse;
  font-size: .6rem;
  font-family: 'JetBrains Mono', monospace;
}
.gd-ratio-table th {
  text-align: left;
  padding: 5px 8px;
  color: rgba(232,197,71,.7);
  border-bottom: 1px solid rgba(232,197,71,.15);
  font-size: .55rem;
  text-transform: uppercase;
  letter-spacing: .1em;
}
.gd-ratio-table td {
  padding: 5px 8px;
  color: rgba(200,215,185,.75);
  border-bottom: 1px solid rgba(255,255,255,.03);
}
.gd-ratio-table tr:hover td { background: rgba(232,197,71,.04); }
.gd-ratio-val-good { color: #5ebd78 !important; font-weight: 700; }
.gd-ratio-val-warn { color: var(--gold) !important; font-weight: 700; }
.gd-ratio-val-bad  { color: #f87171 !important; }



/* Gap presets inside the gap panel */
.gap-wave-presets { margin-bottom:12px; }
.gap-wave-presets .pg-label { font-size:.5rem; margin-top:0; margin-bottom:5px; color:rgba(232,197,71,.45); }
.gap-wave-row { display:flex; flex-wrap:wrap; gap:4px; }
.gap-wave-btn {
  font-family:'JetBrains Mono',monospace; font-size:.54rem; letter-spacing:.06em;
  text-transform:uppercase; padding:3px 8px; border-radius:4px; cursor:pointer;
  border:1px solid rgba(232,197,71,.18); background:rgba(232,197,71,.02);
  color:rgba(150,175,100,.7); transition:all .15s;
}
.gap-wave-btn:hover { border-color:var(--gold); color:var(--gold); }
.gap-wave-btn.active { background:var(--gold); border-color:var(--gold); color:#000; }
.gap-wave-btn .n-badge { font-size:.45rem; opacity:.65; margin-left:3px; }

/* ── Multi-gap selector ──────────── */
.gap-selector-wrap { margin-bottom:12px; }
.gap-selector-label {
  font-size:.52rem; text-transform:uppercase; letter-spacing:.14em;
  color:rgba(232,197,71,.5); margin-bottom:5px;
  display:flex; align-items:center; gap:10px; flex-wrap:wrap;
  font-family:'JetBrains Mono',monospace;
}
.gap-selector-label span { opacity:.6; font-size:.48rem; }
.gap-chips { display:flex; flex-wrap:wrap; gap:4px; }
.gap-chip {
  font-family:'JetBrains Mono',monospace; font-size:.58rem;
  letter-spacing:.06em; padding:3px 8px; border-radius:4px;
  cursor:pointer; border:1px solid transparent;
  background:rgba(255,255,255,.03); color:rgba(130,150,120,.7);
  transition:all .12s; position:relative; user-select:none;
}
.gap-chip:hover { color:rgba(200,220,160,.9); border-color:rgba(255,255,255,.12); }
.gap-chip.selected { color:#000 !important; font-weight:700; }
.gap-chip .chip-dot {
  display:inline-block; width:5px; height:5px; border-radius:50%;
  margin-right:4px; vertical-align:middle; background:currentColor;
}

/* ── Thickness slider ─────────────── */
.thick-row {
  display:flex; align-items:center; gap:10px; margin-bottom:10px;
  padding:7px 9px; background:rgba(232,197,71,.02);
  border:1px solid rgba(232,197,71,.08); border-radius:6px;
}
.thick-row label { color:rgba(180,200,135,.7); font-size:.58rem; white-space:nowrap; flex-shrink:0; }
.thick-row input[type=range] { flex:1; background:rgba(232,197,71,.15); }
.thick-row input[type=range]::-webkit-slider-thumb { background:var(--gold); }
.thick-val { font-size:.66rem; color:var(--gold); min-width:28px; text-align:right; font-family:'JetBrains Mono',monospace; }

/* ── All-gaps stats table ─────────── */
.gap-table-wrap {
  margin-top:12px; overflow-x:auto;
  border:1px solid rgba(232,197,71,.1); border-radius:6px;
}
.gap-table-controls {
  display:flex; gap:8px; flex-wrap:wrap; align-items:center;
  padding:7px 10px; background:rgba(232,197,71,.02);
  border-bottom:1px solid rgba(232,197,71,.08);
}
.gap-table-controls label { font-size:.52rem; color:rgba(150,175,135,.8); white-space:nowrap; font-family:'JetBrains Mono',monospace; }
.gap-table-controls select, .gap-table-controls input {
  font-family:'JetBrains Mono',monospace; font-size:.58rem;
  background:#0e1119; border:1px solid rgba(232,197,71,.2); color:var(--gold);
  padding:2px 5px; border-radius:3px; cursor:pointer;
}
#gdTableWrap table {
  width:100%; border-collapse:collapse; font-size:.58rem;
  font-family:'JetBrains Mono',monospace;
}
#gdTableWrap th {
  position:sticky; top:0; background:#0e1119;
  color:rgba(232,197,71,.65); font-size:.48rem; text-transform:uppercase;
  letter-spacing:.1em; padding:5px 8px; text-align:right;
  border-bottom:1px solid rgba(232,197,71,.15); white-space:nowrap;
}
#gdTableWrap th:first-child { text-align:left; }
#gdTableWrap td {
  padding:4px 8px; text-align:right; border-bottom:1px solid rgba(232,197,71,.04);
  white-space:nowrap; color:rgba(150,180,140,.8);
}
#gdTableWrap td:first-child { text-align:left; }
#gdTableWrap tr:hover td { background:rgba(232,197,71,.04); }
#gdTableWrap tr.gap-row-selected td { background:rgba(232,197,71,.05); }
#gdTableWrap td.gap-id { font-weight:700; }
#gdTableWrap { max-height:340px; overflow-y:auto; }

/* ── Convergence dropdown table ───── */
.conv-dropdown-wrap {
  margin-top:12px;
  border:1px solid rgba(232,197,71,.12); border-radius:6px; overflow:hidden;
}
.conv-dropdown-header {
  display:flex; align-items:center; justify-content:space-between;
  padding:8px 12px; background:rgba(232,197,71,.03); cursor:pointer;
  user-select:none;
}
.conv-dropdown-title { font-size:.54rem; text-transform:uppercase; letter-spacing:.15em; color:var(--gold); font-family:'JetBrains Mono',monospace; }
.conv-dropdown-chevron { font-size:.7rem; color:var(--gold); transition:transform .2s; }
.conv-dropdown-chevron.open { transform:rotate(180deg); }
.conv-dropdown-body { display:none; }
.conv-dropdown-body.open { display:block; }
.conv-table-controls {
  display:flex; gap:10px; flex-wrap:wrap; align-items:center;
  padding:7px 10px; background:rgba(232,197,71,.02);
  border-bottom:1px solid rgba(232,197,71,.07);
}
.conv-table-controls label { font-size:.52rem; color:rgba(150,175,135,.8); white-space:nowrap; font-family:'JetBrains Mono',monospace; }
.conv-table-controls select, .conv-table-controls input[type=number] {
  font-family:'JetBrains Mono',monospace; font-size:.58rem;
  background:#0e1119; border:1px solid rgba(232,197,71,.2); color:var(--gold);
  padding:2px 5px; border-radius:3px; width:56px;
}
.conv-table-controls select { width:auto; }
#gdConvTableWrap { max-height:400px; overflow-y:auto; }
#gdConvTableWrap table { width:100%; border-collapse:collapse; font-size:.56rem; font-family:'JetBrains Mono',monospace; }
#gdConvTableWrap th {
  position:sticky; top:0; background:#0e1119;
  color:rgba(232,197,71,.6); font-size:.47rem; text-transform:uppercase;
  letter-spacing:.1em; padding:5px 7px; text-align:right;
  border-bottom:1px solid rgba(232,197,71,.15); white-space:nowrap;
}
#gdConvTableWrap th:first-child, #gdConvTableWrap th:nth-child(2) { text-align:left; }
#gdConvTableWrap td {
  padding:3px 7px; text-align:right; border-bottom:1px solid rgba(232,197,71,.04);
  white-space:nowrap;
}
#gdConvTableWrap td:first-child, #gdConvTableWrap td:nth-child(2) { text-align:left; }
#gdConvTableWrap tr:hover td { background:rgba(232,197,71,.04); }
.conv-row-improving td { color:#5ebd78; }
.conv-row-worsening td { color:#f87171; }
.conv-row-best td { color:var(--gold) !important; font-weight:700; }
.conv-cell-product { color:#4ecdc4; }
.conv-cell-pi { color:#5b9cf6; }
.conv-cell-err { color:#fb923c; }
.conv-cell-logerr { color:#b48eff; }
.conv-cell-gap { color:var(--gold); font-weight:700; }

/* ── Harmonics Panel ─────────────────── */
.harm-panel {
  max-width:560px; width:100%; margin-top:0;
  background:var(--surface); border:1px solid rgba(91,156,246,.16);
  border-radius:8px; overflow:hidden;
}
.harm-header {
  display:flex; align-items:center; justify-content:space-between;
  padding:12px 16px; cursor:pointer; user-select:none;
  border-bottom:1px solid transparent; transition:border-color .2s;
}
.harm-header.open { border-bottom-color:rgba(91,156,246,.16); }
.harm-header-title { font-size:.54rem; text-transform:uppercase; letter-spacing:.2em; color:var(--blue); font-family:'JetBrains Mono',monospace; }
.harm-chevron { font-size:.7rem; color:var(--blue); transition:transform .25s; }
.harm-chevron.open { transform:rotate(180deg); }
.harm-body { display:none; padding:12px 16px 16px; }
.harm-body.open { display:block; }

.harm-section-title {
  font-size:.48rem; text-transform:uppercase; letter-spacing:.15em;
  color:rgba(91,156,246,.5); margin-bottom:7px; margin-top:10px;
  font-family:'JetBrains Mono',monospace;
}
.harm-section-title:first-child { margin-top:0; }

/* Interval grid */
.harm-interval-grid { display:flex; flex-wrap:wrap; gap:4px; margin-bottom:8px; }
.harm-interval-btn {
  font-family:'JetBrains Mono',monospace; font-size:.57rem;
  letter-spacing:.06em; padding:3px 9px; border-radius:4px;
  cursor:pointer; border:1px solid rgba(91,156,246,.2);
  background:rgba(91,156,246,.03); color:rgba(91,156,246,.45);
  transition:all .12s;
}
.harm-interval-btn:hover { border-color:var(--blue); color:var(--blue); }
.harm-interval-btn.active {
  background:var(--blue); border-color:var(--blue); color:#000; font-weight:700;
}
.harm-interval-btn .ratio-badge { font-size:.45rem; opacity:.65; margin-left:3px; }

/* Consonance color key */
.harm-consonance-key { display:flex; gap:7px; flex-wrap:wrap; margin-bottom:8px; }
.harm-cons-pill { font-size:.5rem; padding:2px 7px; border-radius:4px; display:flex; align-items:center; gap:4px; }
.harm-cons-dot { width:6px; height:6px; border-radius:50%; flex-shrink:0; }

/* Audio row */
.harm-audio-row { display:grid; grid-template-columns:1fr 1fr; gap:9px; margin-bottom:8px; }
.harm-freq-display {
  font-size:.85rem; color:var(--blue); font-weight:700; text-align:center;
  padding:7px; background:rgba(91,156,246,.06); border:1px solid rgba(91,156,246,.15);
  border-radius:5px; letter-spacing:.05em; font-family:'JetBrains Mono',monospace;
}
.harm-note-display {
  font-size:.65rem; color:var(--green); text-align:center;
  padding:7px; background:rgba(94,189,120,.05); border:1px solid rgba(94,189,120,.12);
  border-radius:5px; font-family:'JetBrains Mono',monospace;
}

.harm-btn-row { display:flex; gap:7px; flex-wrap:wrap; margin-bottom:7px; }
.harm-btn {
  font-family:'JetBrains Mono',monospace; font-size:.6rem;
  letter-spacing:.07em; text-transform:uppercase; padding:6px 12px;
  border-radius:4px; cursor:pointer; border:1px solid rgba(91,156,246,.3);
  background:transparent; color:var(--blue); transition:all .15s;
}
.harm-btn:hover, .harm-btn.on { background:var(--blue); color:#000; }

/* Harmonic mode toggle */
.harm-mode-row {
  display:flex; align-items:center; gap:9px; margin-top:7px;
  padding-top:7px; border-top:1px solid rgba(91,156,246,.08);
}
.harm-toggle {
  -webkit-appearance:none; width:30px; height:16px; border-radius:8px;
  background:rgba(91,156,246,.12); border:1px solid rgba(91,156,246,.25);
  cursor:pointer; position:relative; transition:background .2s; flex-shrink:0;
}
.harm-toggle:checked { background:var(--blue); border-color:var(--blue); }
.harm-toggle::after {
  content:''; position:absolute; top:2px; left:2px;
  width:10px; height:10px; border-radius:50%; background:#fff;
  transition:transform .2s; box-shadow:0 1px 4px rgba(0,0,0,.4);
}
.harm-toggle:checked::after { transform:translateX(14px); }
.harm-lbl { font-size:.58rem; color:rgba(150,180,180,.7); text-transform:uppercase; letter-spacing:.1em; font-family:'JetBrains Mono',monospace; }
.harm-state { font-size:.54rem; color:rgba(91,156,246,.4); font-family:'JetBrains Mono',monospace; }

/* Arnold tongue */
.arnold-row { display:flex; gap:9px; align-items:center; margin-bottom:7px; }
.arnold-val { font-size:.62rem; color:#e879f9; min-width:30px; text-align:right; font-family:'JetBrains Mono',monospace; }

/* ── Zoom & Pan overlay controls ─── */
.zoom-bar {
  display:flex; align-items:center; gap:7px; margin-top:5px;
  justify-content:center; flex-wrap:wrap;
}
.zoom-btn {
  font-family:'JetBrains Mono',monospace; font-size:.7rem;
  width:28px; height:28px; border-radius:50%; cursor:pointer;
  border:1px solid rgba(91,156,246,.3); background:rgba(91,156,246,.06);
  color:var(--blue); display:flex; align-items:center; justify-content:center;
  transition:all .15s; flex-shrink:0; font-weight:700;
}
.zoom-btn:hover { background:var(--blue); color:#000; }
.zoom-level-lbl {
  font-size:.58rem; color:rgba(232,197,71,.65); min-width:40px; text-align:center;
  letter-spacing:.08em; font-family:'JetBrains Mono',monospace;
}
.zoom-reset-btn {
  font-family:'JetBrains Mono',monospace; font-size:.55rem;
  padding:3px 9px; border-radius:4px; cursor:pointer;
  border:1px solid rgba(232,197,71,.25); background:transparent;
  color:rgba(232,197,71,.65); transition:all .15s;
}
.zoom-reset-btn:hover { background:var(--gold); color:#000; }

/* canvas pan cursor */
#polarCanvas.panning { cursor:move !important; }


/* ── Coord Mode panel ─────────────── */
.coord-panel {
  max-width:560px; width:100%; margin-top:0;
  background:var(--surface); border:1px solid rgba(94,189,120,.15);
  border-radius:8px; overflow:hidden;
}
.coord-header {
  display:flex; align-items:center; justify-content:space-between;
  padding:12px 16px; cursor:pointer; user-select:none;
  border-bottom:1px solid transparent; transition:border-color .2s;
}
.coord-header.open { border-bottom-color:rgba(94,189,120,.15); }
.coord-header-title { font-size:.54rem; text-transform:uppercase; letter-spacing:.2em; color:var(--green); font-family:'JetBrains Mono',monospace; }
.coord-chevron { font-size:.7rem; color:var(--green); transition:transform .25s; }
.coord-chevron.open { transform:rotate(180deg); }
.coord-body { display:none; padding:12px 16px 16px; }
.coord-body.open { display:block; }
.coord-mode-grid { display:flex; flex-wrap:wrap; gap:5px; margin-bottom:10px; }
.coord-mode-btn {
  font-family:'JetBrains Mono',monospace; font-size:.58rem; letter-spacing:.06em;
  text-transform:uppercase; padding:4px 10px; border-radius:4px; cursor:pointer;
  border:1px solid rgba(94,189,120,.2); background:rgba(94,189,120,.03);
  color:rgba(94,189,120,.6); transition:all .14s;
}
.coord-mode-btn:hover { border-color:var(--green); color:var(--green); }
.coord-mode-btn.active { background:var(--green); color:#000; border-color:var(--green); }

/* ── Play button glow pulse ──────── */
@keyframes playGlow {
  0%,100% { box-shadow: 0 0 0px rgba(94,189,120,0); }
  50%      { box-shadow: 0 0 14px 4px rgba(94,189,120,0.75), 0 0 28px 8px rgba(94,189,120,0.35); }
}
#btnPlay.on {
  animation: playGlow 1.4s ease-in-out infinite;
  border-color: var(--green) !important;
  color: var(--green) !important;
}

/* ── Blend slider flash ──────────── */
@keyframes blendFlash {
  0%,100% { box-shadow: none; }
  30%,70% { box-shadow: 0 0 0 3px rgba(94,189,120,0.7), 0 0 14px 4px rgba(94,189,120,0.45); }
}
#coordBlend.flash {
  animation: blendFlash 0.8s ease-in-out;
}

/* ── Param number input boxes ─────── */
.param-row {
  display: flex; align-items: center; gap: 6px;
}
.param-row input[type=range] {
  flex: 1; min-width: 0;
}
.param-num {
  width: 54px; flex-shrink: 0;
  padding: 3px 5px;
  background: rgba(255,255,255,.05);
  border: 1px solid rgba(91,156,246,.2);
  border-radius: 4px;
  color: var(--gold);
  font-family: 'JetBrains Mono', monospace;
  font-size: .7rem;
  text-align: right;
}
.param-num:focus {
  outline: none; border-color: var(--gold);
}

/* ── Keyboard shortcuts hint ──────── */
.kb-hint {
  font-size:.5rem; color:rgba(91,156,246,.3); text-align:center;
  letter-spacing:.1em; margin-top:4px; font-family:'JetBrains Mono',monospace;
}

/* ── GIF / WebM Export Modal ──────── */
.gif-modal-backdrop {
  position: fixed; inset: 0; z-index: 2000;
  background: rgba(6,6,8,.85);
  backdrop-filter: blur(12px);
  display: flex; align-items: center; justify-content: center;
  opacity: 0; pointer-events: none;
  transition: opacity .22s;
}
.gif-modal-backdrop.open { opacity: 1; pointer-events: all; }
.gif-modal {
  background: #10131c;
  border: 1px solid rgba(232,197,71,.35);
  border-radius: 14px;
  padding: 28px 32px 24px;
  width: min(540px, 96vw);
  box-shadow: 0 0 60px rgba(232,197,71,.12);
  display: flex; flex-direction: column; gap: 18px;
}
.gif-modal-title {
  font-family: 'Lora', serif;
  font-size: 1.25rem;
  color: var(--gold);
  display: flex; align-items: center; gap: 10px;
}
.gif-modal-close {
  margin-left: auto; background: none; border: none;
  color: rgba(232,197,71,.5); font-size: 1.2rem; cursor: pointer;
  transition: color .15s;
}
.gif-modal-close:hover { color: var(--gold); }
.gif-opt-grid {
  display: grid; grid-template-columns: 1fr 1fr; gap: 12px 20px;
}
.gif-opt { display: flex; flex-direction: column; gap: 5px; }
.gif-opt label {
  font-size: .56rem; text-transform: uppercase; letter-spacing: .18em;
  color: rgba(232,197,71,.55); font-family: 'JetBrains Mono', monospace;
}
.gif-opt select, .gif-opt input[type=number], .gif-opt input[type=range] {
  background: rgba(255,255,255,.04);
  border: 1px solid rgba(232,197,71,.2);
  border-radius: 5px; color: var(--text);
  font-family: 'JetBrains Mono', monospace;
  font-size: .72rem; padding: 6px 9px;
}
.gif-opt select:focus, .gif-opt input:focus {
  outline: none; border-color: var(--gold);
}
.gif-axis-row {
  display: flex; gap: 8px;
}
.gif-axis-btn {
  flex: 1; padding: 7px 4px; border-radius: 5px; cursor: pointer;
  border: 1px solid rgba(232,197,71,.25);
  background: rgba(232,197,71,.04);
  color: rgba(232,197,71,.6);
  font-family: 'JetBrains Mono', monospace;
  font-size: .7rem; font-weight: 700; text-align: center;
  transition: all .13s;
}
.gif-axis-btn:hover { border-color: var(--gold); color: var(--gold); }
.gif-axis-btn.active { background: var(--gold); color: #0e1119; border-color: var(--gold); }
.gif-progress-wrap {
  height: 6px; background: rgba(232,197,71,.1); border-radius: 3px; overflow: hidden;
  display: none;
}
.gif-progress-bar {
  height: 100%; background: var(--gold); border-radius: 3px;
  width: 0%; transition: width .1s linear;
}
.gif-status {
  font-size: .6rem; font-family: 'JetBrains Mono', monospace;
  color: rgba(232,197,71,.55); min-height: 16px;
}
.gif-btn-row { display: flex; gap: 10px; justify-content: flex-end; }
.gif-btn {
  padding: 9px 22px; border-radius: 6px; cursor: pointer;
  font-family: 'JetBrains Mono', monospace; font-size: .72rem;
  font-weight: 700; border: none; transition: all .15s;
}
.gif-btn.cancel { background: rgba(255,255,255,.06); color: rgba(200,210,235,.6); }
.gif-btn.cancel:hover { background: rgba(255,255,255,.12); }
.gif-btn.go {
  background: linear-gradient(135deg, #e8c547, #b89020);
  color: #0e1119;
}
.gif-btn.go:hover { filter: brightness(1.1); }
.gif-btn.go:disabled { opacity: .45; cursor: not-allowed; filter: none; }

/* ── Fullscreen mode ─────────────────────── */
:fullscreen .page-shell,
:-webkit-full-screen .page-shell,
:-moz-full-screen .page-shell {
  max-width: 100vw;
  width: 100vw;
  height: 100vh;
}
:fullscreen body,
:-webkit-full-screen body,
:-moz-full-screen body {
  overflow: hidden;
}
/* In fullscreen: sidebar stays, center gets all remaining space */
:fullscreen .col-left,
:-webkit-full-screen .col-left,
:-moz-full-screen .col-left {
  height: 100vh;
  overflow-y: auto;
}
:fullscreen .col-center,
:-webkit-full-screen .col-center,
:-moz-full-screen .col-center {
  height: 100vh;
  overflow-y: auto;
}
#btnFullscreen.active { background: var(--blue); color: #000; }

.intro-body {
  font-size:.62rem; line-height:1.8; color:rgba(180,200,235,.45);
  font-family:'JetBrains Mono',monospace;
}
.intro-body p { margin-bottom:.5em; }
.intro-body p:last-child { margin-bottom:0; }

/* ── Side cards & labels ──────── */
.sidebar-label {
  font-size:.46rem; text-transform:uppercase; letter-spacing:.26em;
  color:rgba(91,156,246,.38); padding:0 2px; margin-top:6px;
  font-family:'JetBrains Mono',monospace;
}
.sidebar-label.gold { color:rgba(232,197,71,.38); }
.sidebar-label:first-child { margin-top:0; }

.side-card {
  background:var(--surface); border:1px solid var(--border);
  border-radius:8px; padding:12px 13px; width:100%;
}
.side-card.gap-card { border-color:var(--gold-border); padding:0; overflow:hidden; }
.side-card.gap-card .gap-panel { border:none; border-radius:0; }
.intro-card { border-color:rgba(91,156,246,.1); }

/* Topbar brand */
.topbar-brand { display:flex; flex-direction:column; gap:2px; flex-shrink:0; }

/* Center canvas sizing */
.col-center .sphere-wrap {
  width:100%; max-width:900px;
  display:flex; flex-direction:column; align-items:center; gap:10px;
}
.col-center #polarCanvas { width:100% !important; height:auto !important; }
.col-center #cartCanvas  { width:100% !important; height:auto !important; max-width:900px; border-radius:4px; }
.col-center .live-stats  { max-width:900px; width:100%; grid-template-columns:repeat(4,1fr); }

/* ── Preset dropdown ────────────────── */
.preset-drop-wrap { width:100%; margin-bottom:0; }
.preset-drop-header {
  display:flex; align-items:center; justify-content:space-between;
  padding:8px 11px; cursor:pointer; user-select:none;
  background:rgba(91,156,246,.03); border:1px solid rgba(91,156,246,.14);
  border-radius:6px; transition:border-color .2s;
}
.preset-drop-header.open { border-radius:6px 6px 0 0; border-color:rgba(91,156,246,.28); }
.preset-drop-title { font-size:.52rem; text-transform:uppercase; letter-spacing:.2em; color:var(--blue); font-family:'JetBrains Mono',monospace; }
.preset-active-name { font-size:.52rem; color:rgba(232,197,71,.65); letter-spacing:.06em; font-family:'JetBrains Mono',monospace; }
.preset-drop-chevron { font-size:.65rem; color:var(--blue); transition:transform .22s; }
.preset-drop-chevron.open { transform:rotate(180deg); }
.preset-drop-body {
  display:none; padding:11px 11px 13px;
  background:rgba(91,156,246,.015); border:1px solid rgba(91,156,246,.14);
  border-top:none; border-radius:0 0 6px 6px;
}
.preset-drop-body.open { display:block; }


/* ── Dual wave row (cart + gap side by side) ── */
.dual-wave-row {
  display: flex;
  gap: 12px;
  width: 100%;
  max-width: 900px;
  align-items: flex-start;
}
.wave-panel {
  display: flex;
  flex-direction: column;
  gap: 6px;
  flex: 1;
  min-width: 0;
}
.wave-label {
  font-size: .5rem; text-transform: uppercase; letter-spacing: .2em;
  color: rgba(91,156,246,.4); font-family: 'JetBrains Mono', monospace;
  padding: 0 2px;
}
.wave-label.gold { color: rgba(232,197,71,.45); }
.wave-panel.gap-hidden { display: none; }

/* ── Stacked accordion panels ── */
.center-bottom {
  width: 100%;
  max-width: 900px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

/* Shared accordion shell */
.accordion {
  width: 100%;
  border-radius: 8px;
  overflow: hidden;
}
.accordion-header {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 16px;
  cursor: pointer;
  user-select: none;
  border-bottom: 1px solid transparent;
  transition: border-color .2s, background .2s;
}
.accordion-header:hover { background: rgba(255,255,255,.02); }
.accordion-header.open { border-bottom-color: var(--accordion-border, rgba(91,156,246,.15)); }
.accordion-title {
  font-size: .56rem; text-transform: uppercase; letter-spacing: .2em;
  font-family: 'JetBrains Mono', monospace;
  flex: 1;
}
.accordion-chevron {
  font-size: .65rem;
  transition: transform .22s;
  flex-shrink: 0;
}
.accordion-chevron.open { transform: rotate(180deg); }
.accordion-body {
  display: none;
  padding: 14px 16px 16px;
}
.accordion-body.open { display: block; }

/* Harmonics accordion — blue */
.accordion.harm-accordion {
  background: var(--surface);
  border: 1px solid rgba(91,156,246,.18);
  --accordion-border: rgba(91,156,246,.18);
}
.accordion.harm-accordion .accordion-title { color: var(--blue); }
.accordion.harm-accordion .accordion-chevron { color: var(--blue); }

/* Gap accordion — gold */
.accordion.gap-accordion {
  background: var(--surface);
  border: 1px solid rgba(232,197,71,.2);
  --accordion-border: rgba(232,197,71,.2);
}
.accordion.gap-accordion .accordion-title { color: var(--gold); }
.accordion.gap-accordion .accordion-chevron { color: var(--gold); }

/* On/Off toggle inside accordion header */
.acc-onoff-wrap {
  display: flex; align-items: center; gap: 8px; flex-shrink: 0;
  padding: 2px 0;
}
.acc-onoff-lbl {
  font-size: .5rem; text-transform: uppercase; letter-spacing: .12em;
  font-family: 'JetBrains Mono', monospace;
}
.acc-onoff-state {
  font-size: .48rem; font-family: 'JetBrains Mono', monospace;
  min-width: 24px;
}
/* prevent toggle click from bubbling to accordion open/close */
.acc-onoff-wrap * { pointer-events: auto; }

/* Gold toggle variant for gap channel */
#gapChannelOn {
  background: rgba(232,197,71,.12);
  border-color: rgba(232,197,71,.3);
}
#gapChannelOn:checked {
  background: var(--gold);
  border-color: var(--gold);
}
/* Scrollbar styling — mathematical dark theme */
::-webkit-scrollbar { width:4px; height:4px; }
::-webkit-scrollbar-track { background:rgba(255,255,255,.02); }
::-webkit-scrollbar-thumb { background:rgba(91,156,246,.2); border-radius:2px; }
::-webkit-scrollbar-thumb:hover { background:rgba(91,156,246,.4); }

/* Select / input theming */
select {
  font-family:'JetBrains Mono',monospace; font-size:.58rem;
  background:#0e1119; border:1px solid rgba(91,156,246,.2);
  color:var(--blue); padding:3px 5px; border-radius:4px;
}

</style>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
</head>
<body>
<div class="page-shell">

  <!-- ══ TOP BAR ══════════════════════════════════════════════════ -->
  <header class="topbar">
    <div class="topbar-brand">
      <div class="topbar-title">Polar vs Cartesian</div>
      <div class="topbar-author">Wessen Getachew</div>
    </div>

    <div class="topbar-actions">
      <button class="ctrl" id="btnPlay">Play</button>
      <button class="ctrl" id="btnReset">↺ Reset</button>
      <button class="ctrl" id="btnSkipComplete" title="Skip drawing — show completed state instantly"> Complete</button>
      <button class="ctrl gold-ctrl" id="btnExport4K" title="Export 4K PNG"> 4K</button>
      <button class="ctrl gold-ctrl" id="btnExportMulti" title="Export Multiple Angles"> Multi</button>
      <button class="ctrl gold-ctrl" id="btnExportGif" title="Export Rotation GIF/WebM">🎞 GIF</button>
      <button class="ctrl" id="btnAutoRot">⟳ Rotate</button>
      <button class="ctrl" id="btnFullscreen" title="Toggle fullscreen"> Full</button>
    </div>
  </header>

  <!-- ══ LEFT SIDEBAR — Controls ═══════════════════════════════════ -->
  <aside class="col-left">

    <!-- Formula Parameters -->
    <div class="sidebar-label">Formula Parameters</div>
    <div class="side-card">
      <div style="display:flex;flex-direction:column;gap:10px;">
        <div class="cg">
          <label>A — amplitude offset</label>
          <div class="param-row">
            <input type="range" id="sA" min="0.1" max="8" step="0.05" value="2">
            <input type="number" class="param-num" id="nA" min="0.1" max="8" step="0.05" value="2">
          </div>
          <div class="vd" id="dA">2</div>
        </div>
        <div class="cg">
          <label>B — outer frequency</label>
          <div class="param-row">
            <input type="range" id="sB" min="0.5" max="24" step="0.5" value="1">
            <input type="number" class="param-num" id="nB" min="0.5" max="24" step="0.5" value="1">
          </div>
          <div class="vd" id="dB">1</div>
        </div>
        <div class="cg">
          <label>C — inner frequency</label>
          <div class="param-row">
            <input type="range" id="sC" min="0.5" max="24" step="0.5" value="3">
            <input type="number" class="param-num" id="nC" min="0.5" max="24" step="0.5" value="3">
          </div>
          <div class="vd" id="dC">3</div>
        </div>
        <div class="cg">
          <label>D — modulation depth</label>
          <div class="param-row">
            <input type="range" id="sD" min="1" max="256" step="1" value="32">
            <input type="number" class="param-num" id="nD" min="1" max="256" step="1" value="32">
          </div>
          <div class="vd" id="dD">32</div>
        </div>
        <div class="cg">
          <label>θ range (× π)</label>
          <div class="param-row">
            <input type="range" id="sTheta" min="2" max="600" step="1" value="64">
            <input type="number" class="param-num" id="nTheta" min="2" max="600" step="1" value="64">
          </div>
          <div class="vd" id="dTheta">64π</div>
        </div>
      </div>
    </div>

    <!-- Playback Speed -->
    <div class="sidebar-label" style="margin-top:10px">Playback Speed</div>
    <div class="side-card">
      <div class="cg" style="margin-bottom:10px">
        <label>Draw speed</label>
        <input type="range" id="sSpeed" min="0.05" max="40" step="0.05" value="8">
        <div style="display:flex;justify-content:space-between;align-items:center">
          <div class="speed-sub" id="speedDesc">normal</div>
          <div class="vd" id="dSpeed">8.0</div>
        </div>
      </div>
      <div class="loop-row">
        <input type="checkbox" class="loop-check" id="cbLoop">
        <span class="loop-lbl">Loop</span>
        <span class="loop-state" id="loopState">off — stops at end</span>
      </div>
      <div class="qrow" style="margin-top:10px">
        <span class="qlabel">Quick:</span>
        <button class="ctrl" style="font-size:.55rem;padding:3px 6px" onclick="setSpeed(0.1)">×0.1</button>
        <button class="ctrl" style="font-size:.55rem;padding:3px 6px" onclick="setSpeed(2)">Slow</button>
        <button class="ctrl" style="font-size:.55rem;padding:3px 6px" onclick="setSpeed(8)">Normal</button>
        <button class="ctrl" style="font-size:.55rem;padding:3px 6px" onclick="setSpeed(20)">Fast</button>
        <button class="ctrl" style="font-size:.55rem;padding:3px 6px" onclick="setSpeed(40)">Max</button>
      </div>
    </div>

    <!-- Coordinate Mode -->
    <div class="sidebar-label" style="margin-top:10px">Coordinate Mode</div>
    <div class="side-card" id="coordPanel">
      <div class="coord-mode-grid" id="coordModeGrid">
        <button class="coord-mode-btn" data-mode="polar">⊙ Polar</button>
        <button class="coord-mode-btn active" data-mode="logcylinder"> Log-Cyl</button>
        <button class="coord-mode-btn" data-mode="logspiral">↺ Spiral</button>
        <button class="coord-mode-btn" data-mode="poincare">◎ Poincaré</button>
        <button class="coord-mode-btn" data-mode="toroidal">⬡ Toroidal</button>
        <button class="coord-mode-btn" data-mode="fibonacci">🌀 Fibonacci</button>
        <button class="coord-mode-btn" data-mode="klein">∞ Klein</button>
      </div>
      <div style="font-size:.56rem;color:rgba(94,189,120,.45);line-height:1.6;margin:7px 0;font-family:'JetBrains Mono',monospace" id="coordModeDesc">Log-Cylinder: u = ln(θ+1), v = f(θ). Compresses the θ axis logarithmically, revealing fine structure near origin.</div>
      <div class="cg">
        <label style="color:rgba(150,180,150,.7)">Blend</label>
        <input type="range" id="coordBlend" min="0" max="1" step="0.01" value="1" style="background:rgba(94,189,120,.18)">
        <div style="font-size:.62rem;color:var(--green);text-align:right;font-family:'JetBrains Mono',monospace" id="coordBlendVal">100%</div>
      </div>
    </div>

    <!-- Presets -->
    <div class="sidebar-label" style="margin-top:10px">Presets</div>
    <div class="side-card">
      <div class="preset-drop-wrap" style="max-width:none;margin:0">
        <div class="preset-drop-header open" id="presetDropHeader">
          <div class="preset-drop-title">Select Preset</div>
          <span class="preset-active-name" id="presetActiveName">Original</span>
          <span class="preset-drop-chevron open" id="presetDropChev">▼</span>
        </div>
        <div class="preset-drop-body open" id="presetDropBody">
          <div class="pg-label">Classics</div><div class="presets" id="pgClassic"></div>
          <div class="pg-label">Complex</div><div class="presets" id="pgComplex"></div>
          <div class="pg-label">Organic</div><div class="presets" id="pgOrganic"></div>
          <div class="pg-label">Geometric</div><div class="presets" id="pgGeo"></div>
          <div class="pg-label">Ethereal</div><div class="presets" id="pgEth"></div>
        </div>
      </div>
    </div>

    <!-- Gap Decomposition — controls only in sidebar, output below harmonics in center -->
    <div class="sidebar-label" style="margin-top:10px;color:rgba(232,197,71,.38)">Gap Decomposition · ζ(s)</div>
    <div class="side-card">

      <!-- Channel toggle -->
      <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;padding-bottom:10px;border-bottom:1px solid rgba(232,197,71,.1)">
        <input type="checkbox" class="harm-toggle" id="gapChannelOn"
          style="background:rgba(232,197,71,.12);border-color:rgba(232,197,71,.3);"
          onchange="toggleGapChannel(this.checked)">
        <span class="acc-onoff-lbl" style="color:var(--gold)">Channel</span>
        <span class="acc-onoff-state" style="color:rgba(232,197,71,.45)" id="gapChannelState">off</span>
      </div>

      <div class="gap-ctrl-grid">
        <div class="cg">
          <label>Max N (primes up to)</label>
          <div class="param-row">
            <input type="range" id="gdN" min="10000" max="400000000" step="10000" value="10000000"
              style="flex:1;min-width:0">
            <input type="number" id="gdNNum" min="10000" max="400000000" step="1000000" value="10000000"
              class="param-num" style="width:80px">
          </div>
          <div class="gap-vd" id="gdND">10,000,000</div>
          <!-- Quick N presets -->
          <div style="display:flex;gap:4px;flex-wrap:wrap;margin-top:4px;">
            <button class="ctrl" style="font-size:.5rem;padding:2px 6px" onclick="setGdN(1000000)">1M</button>
            <button class="ctrl" style="font-size:.5rem;padding:2px 6px" onclick="setGdN(10000000)">10M</button>
            <button class="ctrl" style="font-size:.5rem;padding:2px 6px" onclick="setGdN(50000000)">50M</button>
            <button class="ctrl" style="font-size:.5rem;padding:2px 6px" onclick="setGdN(100000000)">100M</button>
            <button class="ctrl" style="font-size:.5rem;padding:2px 6px" onclick="setGdN(200000000)">200M</button>
            <button class="ctrl gold-ctrl" style="font-size:.5rem;padding:2px 6px" onclick="setGdN(400000000)">400M</button>
          </div>
        </div>
        <div class="cg">
          <label>ζ exponent s</label>
          <div class="param-row">
            <input type="range" id="gdS" min="1.01" max="10" step="0.01" value="2" style="flex:1;min-width:0">
            <input type="number" id="gdSNum" min="1.01" max="10" step="0.01" value="2" class="param-num">
          </div>
          <div class="gap-vd" id="gdSD" style="color:var(--gold)">s = 2.00</div>
        </div>
      </div>

      <div class="gap-filter-row">
        <span class="gap-filter-lbl">Gap filter:</span>
        <button class="gap-filter-btn active" data-filter="all" onclick="setGapFilter(this,'all')">All</button>
        <button class="gap-filter-btn" data-filter="tiny" onclick="setGapFilter(this,'tiny')">2–10</button>
        <button class="gap-filter-btn" data-filter="small" onclick="setGapFilter(this,'small')">2–30</button>
        <button class="gap-filter-btn" data-filter="medium" onclick="setGapFilter(this,'medium')">20–80</button>
        <button class="gap-filter-btn" data-filter="large" onclick="setGapFilter(this,'large')">50–200</button>
      </div>

      <div class="gap-run-row" style="margin-top:8px">
        <button class="ctrl gold-ctrl" id="gdRunBtn" onclick="gdRun()">Compute</button>
        <button class="ctrl" id="gdAbortBtn" onclick="gdAbort()" style="display:none;font-size:.6rem;padding:6px 10px;background:rgba(248,113,113,.12);border-color:rgba(248,113,113,.35);color:rgba(248,113,113,.85)">⏹ Stop</button>
        <button class="ctrl gold-ctrl" onclick="gdClear()" style="font-size:.6rem;padding:6px 10px"> Clear</button>
      </div>

      <div class="thick-row">
        <label>Trace Thickness</label>
        <input type="range" id="gdThick" min="0.5" max="8" step="0.5" value="1.35">
        <span class="thick-val" id="gdThickVal">1.35px</span>
      </div>

      <div class="gap-progress" id="gdProgress" style="margin-top:8px"><div class="gap-progress-fill" id="gdProgressFill"></div></div>
      <div class="gap-status" id="gdStatus" style="margin-top:4px;font-size:.54rem;color:rgba(232,197,71,.45);font-family:'JetBrains Mono',monospace;line-height:1.5">Run to analyze prime gap contributions to ζ(s).</div>

    </div><!-- /.side-card -->

  </aside>

  <!-- ══ CENTER — Visualization ═════════════════════════════════ -->
  <main class="col-center">

    <!-- Featured Formula Box -->
    <div style="width:100%;margin-bottom:16px;">
      <div style="background:rgba(232,197,71,0.06);border:2px solid rgba(232,197,71,0.3);border-radius:10px;padding:16px 20px;text-align:center;">
        <div style="font-family:'JetBrains Mono',monospace;font-size:clamp(1.1rem,2.2vw,1.6rem);color:var(--gold);font-weight:bold;">
          f(θ) = <span id="fA">2</span> + sin(<span id="fB">1</span>θ) · sin(<span id="fC">3</span>θ / <span id="fD">32</span>)
        </div>
      </div>
    </div>

    <!-- Sphere canvas -->
    <div class="sphere-wrap" style="position:relative;">
      <canvas id="polarCanvas"></canvas>

      <div class="rot-controls">
        <span class="rot-hint">drag to rotate · shift+drag to pan</span>
        <button class="ctrl" id="btnResetRot" style="font-size:.58rem;padding:4px 9px">↺ Reset View</button>
      </div>

      <div class="zoom-bar">
        <button class="zoom-btn" id="btnZoomOut">−</button>
        <span class="zoom-level-lbl" id="zoomLbl">1.0×</span>
        <button class="zoom-btn" id="btnZoomIn">+</button>
        <button class="zoom-reset-btn" id="btnZoomReset">Reset Zoom</button>
      </div>

      <!-- Manual Rotation Controls -->
      <div style="margin-top:12px;margin-bottom:8px;">
        <div class="pg-label" style="margin-bottom:6px;">rotation (degrees)</div>
        <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:8px;">
          <div>
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:3px;">
              <label style="font-size:.48rem;color:var(--text-dim);font-family:'JetBrains Mono',monospace;text-transform:uppercase;letter-spacing:.1em;">X°</label>
              <input type="number" class="rotation-input-small" id="rotX" value="90" min="0" max="360" step="1" style="width:50px;padding:3px 5px;background:var(--surface);border:1px solid var(--border);border-radius:3px;color:var(--text);font-family:'JetBrains Mono',monospace;font-size:.65rem;text-align:center;">
            </div>
            <input type="range" class="slider" id="rotXSlider" min="0" max="360" step="1" value="90" style="width:100%;">
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:3px;">
              <label style="font-size:.48rem;color:var(--text-dim);font-family:'JetBrains Mono',monospace;text-transform:uppercase;letter-spacing:.1em;">Y°</label>
              <input type="number" class="rotation-input-small" id="rotY" value="0" min="0" max="360" step="1" style="width:50px;padding:3px 5px;background:var(--surface);border:1px solid var(--border);border-radius:3px;color:var(--text);font-family:'JetBrains Mono',monospace;font-size:.65rem;text-align:center;">
            </div>
            <input type="range" class="slider" id="rotYSlider" min="0" max="360" step="1" value="0" style="width:100%;">
          </div>
          <div>
            <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:3px;">
              <label style="font-size:.48rem;color:var(--text-dim);font-family:'JetBrains Mono',monospace;text-transform:uppercase;letter-spacing:.1em;">Z°</label>
              <input type="number" class="rotation-input-small" id="rotZ" value="0" min="0" max="360" step="1" style="width:50px;padding:3px 5px;background:var(--surface);border:1px solid var(--border);border-radius:3px;color:var(--text);font-family:'JetBrains Mono',monospace;font-size:.65rem;text-align:center;">
            </div>
            <input type="range" class="slider" id="rotZSlider" min="0" max="360" step="1" value="0" style="width:100%;">
          </div>
        </div>
      </div>

      <div class="kb-hint">Space=Play · R=Rotate · F=Fullscreen · S=Screenshot · ↑↓=Zoom · ←→=Spin</div>
    </div>

    <!-- Dual wave row: Cartesian + Gap side by side -->
    <div class="dual-wave-row" id="dualWaveRow">
      <!-- Left: Cartesian wave -->
      <div class="wave-panel" id="cartWavePanel">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:4px;">
          <div class="wave-label">f(θ) — Cartesian</div>
          <div style="display:flex;align-items:center;gap:10px;">
            <span style="font-size:.44rem;color:rgba(91,156,246,.35);font-family:'JetBrains Mono',monospace;letter-spacing:.08em">click to probe</span>
            <label style="font-size:.48rem;color:rgba(91,156,246,.5);text-transform:uppercase;letter-spacing:.1em;font-family:'JetBrains Mono',monospace;">Extend −a</label>
            <input type="checkbox" class="loop-check" id="cbNegA" title="Show negative extent below zero">
          </div>
        </div>
        <canvas id="cartCanvas" style="cursor:crosshair"></canvas>

        <!-- Wave scrubber slider — drag to scan any drawn point -->
        <div class="wave-scrubber-wrap">
          <span class="wave-scrubber-label">⟵ Wave Scrub ⟶</span>
          <input type="range" id="waveScrubber" min="0" max="10000" step="1" value="0"
                 oninput="onWaveScrub(this.value)"
                 title="Drag to highlight any drawn point on both canvases">
          <span class="wave-scrubber-pct" id="waveScrubPct">0%</span>
        </div>

        <!-- Probe detail panel — shown after click or scrub -->
        <div id="cartProbePanel" style="display:none;margin-top:8px;background:rgba(6,6,8,.85);border:1px solid rgba(91,156,246,.18);border-radius:6px;padding:10px 14px;">
          <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:8px;">
            <span style="font-size:.5rem;text-transform:uppercase;letter-spacing:.2em;color:rgba(91,156,246,.5);font-family:'JetBrains Mono',monospace">Point Probe</span>
            <button onclick="clearCartProbe()" style="background:none;border:none;color:rgba(91,156,246,.4);font-size:.65rem;cursor:pointer;padding:0 2px"></button>
          </div>
          <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:6px 12px;" id="cartProbeGrid">
            <!-- filled by JS -->
          </div>
          <!-- Connection to gap decomp -->
          <div id="cartProbeGap" style="display:none;margin-top:8px;padding-top:8px;border-top:1px solid rgba(232,197,71,.12);">
            <div style="font-size:.44rem;text-transform:uppercase;letter-spacing:.18em;color:rgba(232,197,71,.4);font-family:'JetBrains Mono',monospace;margin-bottom:4px">Prime Gap at this θ</div>
            <div id="cartProbeGapDetail" style="font-family:'JetBrains Mono',monospace;font-size:.62rem;color:rgba(232,197,71,.85);line-height:1.9"></div>
          </div>
          <!-- Sphere highlight indicator -->
          <div id="cartProbeSphereHint" style="margin-top:8px;padding:5px 9px;background:rgba(91,156,246,.06);border-radius:4px;font-size:.48rem;color:rgba(91,156,246,.55);font-family:'JetBrains Mono',monospace;text-align:center;display:none">
            Point highlighted on sphere above
          </div>
        </div>

        <!-- Connection explainer — always shown once gap data loaded -->
        <div id="cartGapRelation" style="display:none;margin-top:8px;padding:9px 13px;background:rgba(232,197,71,.04);border:1px solid rgba(232,197,71,.1);border-radius:6px;">
          <div style="font-size:.44rem;text-transform:uppercase;letter-spacing:.18em;color:rgba(232,197,71,.35);font-family:'JetBrains Mono',monospace;margin-bottom:5px">How f(θ) and Gap Families Connect</div>
          <div id="cartGapRelationText" style="font-size:.54rem;color:rgba(200,185,150,.6);font-family:'JetBrains Mono',monospace;line-height:1.8"></div>
        </div>
      </div>
      <!-- Right: Gap ζ(s) chart — hidden until data loaded -->
      <div class="wave-panel gap-hidden" id="gapWavePanel">
        <div class="wave-label gold">Gap Families — ζ(s)</div>
        <canvas id="gapCanvas" height="300"></canvas>
        <div class="gap-legend" id="gdLegend"></div>
      </div>
    </div>

    <!-- Live stats bar -->
    <div class="live-stats" id="liveStats" style="max-width:900px">
      <div class="stat-cell"><div class="stat-lbl">Progress</div><div class="stat-val" id="stProg">0%</div></div>
      <div class="stat-cell"><div class="stat-lbl">θ current</div><div class="stat-val" id="stTheta">0</div></div>
      <div class="stat-cell"><div class="stat-lbl">f(θ) now</div><div class="stat-val" id="stFTheta">—</div></div>
      <div class="stat-cell"><div class="stat-lbl">f(θ) min</div><div class="stat-val" id="stFMin">—</div></div>
      <div class="stat-cell"><div class="stat-lbl">f(θ) max</div><div class="stat-val" id="stFMax">—</div></div>
      <div class="stat-cell"><div class="stat-lbl">f(θ) mean</div><div class="stat-val" id="stFMean">—</div></div>
      <div class="stat-cell"><div class="stat-lbl">Pts drawn</div><div class="stat-val" id="stPts">0</div></div>
      <div class="stat-cell"><div class="stat-lbl">θ total</div><div class="stat-val" id="stThetaMax">64π</div></div>
    </div>

    <!-- Harmonics accordion (center only) -->
    <div class="center-bottom">
      <div class="accordion harm-accordion" id="harmPanelWrap">
        <div class="accordion-header" id="harmAccHeader" onclick="toggleHarmAccordion(event)">
          <span class="accordion-title"> Harmonics · Farey Intervals</span>
          <div class="acc-onoff-wrap" onclick="event.stopPropagation()">
            <input type="checkbox" class="harm-toggle" id="harmSyncOn"
              style="background:rgba(91,156,246,.12);border-color:rgba(91,156,246,.3);"
              onchange="toggleHarmSync(this.checked)">
            <span class="acc-onoff-lbl" style="color:var(--blue)">Sound</span>
            <span class="acc-onoff-state" style="color:rgba(91,156,246,.45)" id="harmSyncState">off</span>
          </div>
          <span class="accordion-chevron" id="harmAccChev">▼</span>
        </div>
        <div class="accordion-body" id="harmAccBody">
          <div class="harm-mode-row" style="margin-top:0;padding-top:0;border-top:none;margin-bottom:10px">
            <input type="checkbox" class="harm-toggle" id="harmColorMode">
            <span class="harm-lbl">Sphere Color Mode</span>
            <span class="harm-state" id="harmModeState">off</span>
          </div>
          <div class="harm-section-title">Farey Intervals</div>
          <div class="harm-interval-grid" id="harmIntervalGrid"></div>
          <div class="harm-consonance-key" style="margin-top:5px">
            <div class="harm-cons-pill"><div class="harm-cons-dot" style="background:#5ebd78"></div><span style="color:#5ebd78;font-size:.48rem">1:1</span></div>
            <div class="harm-cons-pill"><div class="harm-cons-dot" style="background:#5b9cf6"></div><span style="color:#5b9cf6;font-size:.48rem">q≤4</span></div>
            <div class="harm-cons-pill"><div class="harm-cons-dot" style="background:#e8c547"></div><span style="color:#e8c547;font-size:.48rem">q≤16</span></div>
            <div class="harm-cons-pill"><div class="harm-cons-dot" style="background:#fb923c"></div><span style="color:#fb923c;font-size:.48rem">q≤64</span></div>
            <div class="harm-cons-pill"><div class="harm-cons-dot" style="background:#b48eff"></div><span style="color:#b48eff;font-size:.48rem">q>64</span></div>
          </div>
          <div class="harm-audio-row" style="margin-top:10px">
            <div class="harm-freq-display" id="harmFreqDisplay">— Hz</div>
            <div class="harm-note-display" id="harmNoteDisplay">— · —</div>
          </div>
          <div class="harm-section-title" style="margin-top:12px">Audio
            <span id="harmModeIndicator" style="margin-left:8px;font-size:.48rem;color:rgba(91,156,246,.5);text-transform:none;letter-spacing:.04em;font-family:'DM Sans',sans-serif">· sound is off</span>
          </div>
          <div class="harm-btn-row">
            <button class="harm-btn" id="harmPlayBtn" onclick="harmPlay()">Play once</button>
            <button class="harm-btn" id="harmChordBtn" onclick="harmChord()"> Chord</button>
            <button class="harm-btn" id="harmArpBtn" onclick="harmArp()">∿ Arp</button>
            <button class="harm-btn" onclick="harmStop()">■ Stop</button>
          </div>
          <div style="display:flex;gap:9px;margin-top:8px">
            <div class="cg" style="flex:1">
              <label style="font-size:.52rem;color:rgba(150,175,200,.6)">Base Hz</label>
              <input type="range" id="harmBaseHz" min="110" max="880" step="1" value="220" style="background:rgba(91,156,246,.18)">
              <div class="vd" id="harmBaseHzVal" style="color:var(--blue);font-size:.58rem">220 Hz</div>
            </div>
            <div class="cg" style="flex:1">
              <label style="font-size:.52rem;color:rgba(150,175,200,.6)">Volume</label>
              <input type="range" id="harmVol" min="0" max="5" step="0.05" value="0.35" style="background:rgba(91,156,246,.18)">
              <div class="vd" id="harmVolVal" style="color:var(--blue);font-size:.58rem">35%</div>
            </div>
            <div class="cg" style="flex:1">
              <label style="font-size:.52rem;color:rgba(150,175,200,.6)">Wave</label>
              <select id="harmWave" style="font-family:'JetBrains Mono',monospace;font-size:.55rem;background:#0e1119;border:1px solid rgba(91,156,246,.2);color:var(--blue);padding:2px 3px;border-radius:4px;width:100%">
                <option value="sine">Sine</option>
                <option value="triangle">Triangle</option>
                <option value="square">Square</option>
                <option value="sawtooth">Saw</option>
              </select>
            </div>
          </div>
          <div class="harm-section-title" style="margin-top:12px">Arnold Tongue · Warp</div>
          <div class="arnold-row">
            <label style="font-size:.52rem;color:rgba(150,175,200,.6);flex-shrink:0">K</label>
            <input type="range" id="harmArnold" min="0" max="2" step="0.01" value="0" style="flex:1;background:rgba(232,121,249,.18)">
            <span class="arnold-val" id="harmArnoldVal">0.00</span>
            <button class="harm-btn" style="font-size:.5rem;padding:3px 6px" onclick="applyArnoldWarp()">Apply</button>
            <button class="harm-btn" style="font-size:.5rem;padding:3px 6px" onclick="resetArnold()">Reset</button>
          </div>
        </div><!-- /.accordion-body -->
      </div><!-- /.harm-accordion -->

      <!-- ── Gap Decomposition Results Accordion ── -->
      <div class="accordion gap-accordion" id="gapDecompWrap">
        <div class="accordion-header" id="gapAccHeader" onclick="toggleGapAccordion(event)">
          <span class="accordion-title">Gap Decomposition · ζ(s)</span>
          <div class="acc-onoff-wrap" style="display:flex;gap:8px;align-items:center">
            <button class="ctrl gold-ctrl" id="gdExportPNG" onclick="gdExport4K();event.stopPropagation()" style="display:none;font-size:.5rem;padding:3px 8px">4K PNG</button>
            <button class="ctrl gold-ctrl" id="gdExportCSV" onclick="gdExportCSV();event.stopPropagation()" style="display:none;font-size:.5rem;padding:3px 8px">CSV</button>
            <button class="ctrl gold-ctrl" id="gdExportTXT" onclick="gdExportTXT();event.stopPropagation()" style="display:none;font-size:.5rem;padding:3px 8px">Report</button>
          </div>
          <span class="accordion-chevron" id="gapAccChev">▼</span>
        </div>
        <div class="accordion-body" id="gapAccBody">

          <!-- Gap selector chips -->
          <div class="gap-selector-wrap" id="gdSelectorWrap" style="display:none">
            <div class="gap-selector-label">
              Click gaps to highlight on sphere &amp; cart
              <span>— each gets its own color; deselect to remove</span>
              <button class="gap-filter-btn" onclick="gdClearSelection()" style="margin-left:auto"> Clear all</button>
            </div>
            <div class="gap-chips" id="gdChips"></div>
          </div>

          <!-- Stats grid with screenshot -->
          <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:6px;margin-top:2px">
            <span style="font-size:.48rem;text-transform:uppercase;letter-spacing:.18em;color:rgba(232,197,71,.4);font-family:'JetBrains Mono',monospace">Statistics</span>
            <button class="ctrl gold-ctrl" onclick="screenshotSection('gdStatsGrid','gap_stats')" style="font-size:.48rem;padding:2px 7px;opacity:.7">Stats PNG</button>
          </div>
          <div class="gap-live-stats" id="gdStatsGrid" style="display:none">
            <div class="gap-stat"><div class="gap-stat-lbl">Total primes</div><div class="gap-stat-val" id="gst0">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Gap families</div><div class="gap-stat-val" id="gst1">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Max gap</div><div class="gap-stat-val" id="gst2">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Target ζ(s)</div><div class="gap-stat-val" id="gst3">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Computed product</div><div class="gap-stat-val" id="gst4">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Rel. error</div><div class="gap-stat-val" id="gst5">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Twin primes (g=2)</div><div class="gap-stat-val" id="gst6">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Cousin primes (g=4)</div><div class="gap-stat-val" id="gst7">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Sexy primes (g=6)</div><div class="gap-stat-val" id="gst8">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Most common gap</div><div class="gap-stat-val" id="gst9">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Twin ratio (g2/g4)</div><div class="gap-stat-val" id="gst10">—</div></div>
            <div class="gap-stat"><div class="gap-stat-lbl">Calculated π</div><div class="gap-stat-val" id="gst11">—</div></div>
          </div>

          <!-- Chart canvas with screenshot -->
          <div style="display:flex;align-items:center;justify-content:space-between;margin-top:12px;margin-bottom:6px">
            <div class="gap-tabs" id="gdTabs" style="display:none;flex:1">
              <button class="gap-tab active" onclick="gdSwitchTab(this,'contrib')">Contributions</button>
              <button class="gap-tab" onclick="gdSwitchTab(this,'dist')">Distribution</button>
              <button class="gap-tab" onclick="gdSwitchTab(this,'conv')">Convergence</button>
              <button class="gap-tab" onclick="gdSwitchTab(this,'ratio')">Gap Ratios</button>
              <button class="gap-tab" onclick="gdSwitchTab(this,'pct')">% Share</button>
            </div>
            <button class="ctrl gold-ctrl" id="gdChartScreenshot" onclick="screenshotCanvas(gdCanvas,'gap_chart')" style="font-size:.48rem;padding:2px 7px;opacity:.7;display:none;margin-left:8px;flex-shrink:0">Chart PNG</button>
          </div>
          <!-- Chart canvas (re-rendered here from the center wave panel, shown large) -->
          <div id="gdChartArea" style="width:100%;display:none">
            <canvas id="gdChartBig" style="width:100%;border-radius:4px;background:#060608;display:block"></canvas>
            <!-- Export row matching reference file style -->
            <div class="gd-export-row">
              <button class="gd-export-btn csv" onclick="gdExportChartCSV()">Export CSV</button>
              <button class="gd-export-btn dom" onclick="gdExportDOM()">DOM Export PNG</button>
              <button class="gd-export-btn cvs" onclick="gdExportCanvasPNG()">Canvas Export PNG</button>
              <button class="gd-export-btn" style="background:linear-gradient(135deg,#e8c547,#b8960f);color:#000" onclick="gdExport4K();event.stopPropagation()">Full 4K Report PNG</button>
            </div>
          </div>

          <!-- Contributions table with screenshot -->
          <div class="gap-table-wrap" id="gdTableSection" style="display:none">
            <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:6px">
              <div class="gap-table-controls" style="flex:1">
                <label>Sort by:
                  <select id="gdTableSort" onchange="gdBuildTable()">
                    <option value="gap">Gap ↑</option>
                    <option value="count_desc">Count ↓</option>
                    <option value="logshare_desc">Log-ζ share ↓</option>
                    <option value="product_desc">Product ↓</option>
                    <option value="cumulative">Cumulative ↑</option>
                  </select>
                </label>
                <label>Decimals:
                  <input type="number" id="gdTableDec" min="2" max="20" value="8" style="width:46px" onchange="gdBuildTable()">
                </label>
                <label>Show top:
                  <select id="gdTableTop" onchange="gdBuildTable()">
                    <option value="0">All</option>
                    <option value="10">10</option>
                    <option value="25">25</option>
                    <option value="50">50</option>
                    <option value="100">100</option>
                  </select>
                </label>
                <label><input type="checkbox" id="gdTableHighlight" checked onchange="gdBuildTable()"> Highlight selected</label>
              </div>
              <button class="ctrl gold-ctrl" onclick="screenshotSection('gdTableWrap','gap_table')" style="font-size:.48rem;padding:2px 7px;opacity:.7;margin-left:8px;flex-shrink:0"> Table PNG</button>
            </div>
            <div id="gdTableWrap"></div>
          </div>

          <!-- Convergence table with screenshot -->
          <div class="conv-dropdown-wrap" id="gdConvSection" style="display:none">
            <div class="conv-dropdown-header" id="convDropHeader">
              <div class="conv-dropdown-title">⟶ Step-by-step convergence to π²/6 = ζ(2)</div>
              <button class="ctrl gold-ctrl" onclick="screenshotSection('gdConvTableWrap','gap_convergence');event.stopPropagation()" style="font-size:.48rem;padding:2px 7px;opacity:.7;margin-right:8px"> Conv PNG</button>
              <span class="conv-dropdown-chevron" id="convDropChev">▼</span>
            </div>
            <div class="conv-dropdown-body" id="convDropBody">
              <div class="conv-table-controls">
                <label>Sort:
                  <select id="convSort" onchange="gdBuildConvTable()">
                    <option value="gap_asc">Gap ↑ (natural order)</option>
                    <option value="logshare_desc">Log-share ↓</option>
                    <option value="cumulative_asc">Cumulative product ↑</option>
                    <option value="relerr_asc">Rel. error ↑ (best first)</option>
                  </select>
                </label>
                <label>Decimals:
                  <input type="number" id="convDec" min="2" max="20" value="12" onchange="gdBuildConvTable()">
                </label>
                <label>Show top:
                  <select id="convTop" onchange="gdBuildConvTable()">
                    <option value="0">All steps</option>
                    <option value="10">10</option>
                    <option value="25">25</option>
                    <option value="50">50</option>
                  </select>
                </label>
                <label><input type="checkbox" id="convShowPi" checked onchange="gdBuildConvTable()"> Show π estimate</label>
                <label><input type="checkbox" id="convShowLog" checked onchange="gdBuildConvTable()"> log₁₀ error</label>
                <label><input type="checkbox" id="convShowDelta" checked onchange="gdBuildConvTable()"> Δ product</label>
              </div>
              <div id="gdConvTableWrap"></div>
            </div>
          </div>

        </div><!-- /.accordion-body #gapAccBody -->
      </div><!-- /.gap-accordion -->

      <!-- ── Standalone Gap Ratio Analysis: Twin Prime Conjecture Evidence ── -->
      <div class="gd-ratio-panel" id="gdRatioPanel">
        <div class="gd-ratio-panel-header">
          <div class="gd-ratio-panel-title">Gap Ratio Analysis: Twin Prime Conjecture Evidence</div>
          <div class="gd-ratio-panel-desc">
            Tracks Count(gap=2)/Count(gap=4) and other gap ratios. Hardy-Littlewood predicts twin primes (gap 2) and
            cousin primes (gap 4) have <em>identical</em> singular series constants, so their ratio should approach
            <strong style="color:var(--gold)">1.0</strong> as N→∞. Gap 6 (sexy primes) has a 2× larger constant
            due to the p=3 factor — so it is asymptotically <em>twice as frequent</em> as gap 2 or gap 4, not equal.
          </div>
        </div>
        <div class="gd-ratio-canvas-wrap">
          <canvas id="gdRatioChartBig" width="3840" height="2160"></canvas>
        </div>
        <div class="gd-ratio-table-wrap">
          <div class="gd-ratio-table-title">Gap Ratio Data</div>
          <table class="gd-ratio-table" id="gdRatioTable">
            <thead>
              <tr>
                <th>Metric</th>
                <th>Value</th>
                <th>Target</th>
                <th>Status</th>
                <th>Interpretation</th>
              </tr>
            </thead>
            <tbody id="gdRatioTableBody">
              <tr><td colspan="5" style="color:rgba(180,200,180,.4);padding:10px 8px">Run Gap Decomposition to populate…</td></tr>
            </tbody>
          </table>
          <div class="gd-export-row">
            <button class="gd-export-btn csv" onclick="gdExportRatioCSV()"> Export Table CSV</button>
            <button class="gd-export-btn dom" onclick="gdExportRatioDOM()">DOM Export PNG</button>
            <button class="gd-export-btn cvs" onclick="gdExportRatioCanvas()">Canvas Export PNG</button>
          </div>
        </div>
      </div>

      <!-- ── Riemann Zeta ζ(½+it) Accordion ── -->
      <div class="accordion" id="zetaAccordionWrap" style="border-color:rgba(180,100,255,.25);">
        <div class="accordion-header" id="zetaAccHeader" onclick="toggleZetaAccordion(event)"
             style="border-bottom-color:rgba(180,100,255,.15)">
          <span class="accordion-title" style="color:rgba(200,140,255,.85)">ζ Riemann Zeta · ζ(½+it) Critical Line</span>
          <div style="display:flex;gap:6px;align-items:center;margin-right:8px">
            <button class="ctrl" id="zetaRunBtn" onclick="zetaRun();event.stopPropagation()"
              style="font-size:.5rem;padding:3px 10px;background:rgba(180,100,255,.12);border-color:rgba(180,100,255,.3);color:rgba(200,140,255,.85)">Compute</button>
            <button class="ctrl" id="zetaScreenshotBtn" onclick="zetaExport4K();event.stopPropagation()"
              style="display:none;font-size:.48rem;padding:2px 7px;background:rgba(180,100,255,.08);border-color:rgba(180,100,255,.2);color:rgba(200,140,255,.6)">4K PNG</button>
          </div>
          <span class="accordion-chevron" id="zetaAccChev" style="color:rgba(180,100,255,.5)">▼</span>
        </div>
        <div class="accordion-body" id="zetaAccBody">

          <!-- Controls row -->
          <div style="display:flex;flex-wrap:wrap;gap:14px;align-items:flex-end;margin-bottom:12px;padding-bottom:12px;border-bottom:1px solid rgba(180,100,255,.1)">
            <div style="flex:1;min-width:140px">
              <label style="font-size:.48rem;text-transform:uppercase;letter-spacing:.15em;color:rgba(180,100,255,.5);font-family:'JetBrains Mono',monospace;display:block;margin-bottom:3px">t range (0 → T<sub>max</sub>)</label>
              <input type="range" id="zetaTMax" min="10" max="200" step="1" value="50" style="width:100%">
              <div style="font-size:.55rem;color:rgba(200,140,255,.7);font-family:'JetBrains Mono',monospace;margin-top:2px">T = <span id="zetaTMaxVal">50</span></div>
            </div>
            <div style="flex:1;min-width:120px">
              <label style="font-size:.48rem;text-transform:uppercase;letter-spacing:.15em;color:rgba(180,100,255,.5);font-family:'JetBrains Mono',monospace;display:block;margin-bottom:3px">Resolution (pts)</label>
              <input type="range" id="zetaRes" min="500" max="8000" step="100" value="3000" style="width:100%">
              <div style="font-size:.55rem;color:rgba(200,140,255,.7);font-family:'JetBrains Mono',monospace;margin-top:2px">N = <span id="zetaResVal">3000</span></div>
            </div>
            <div>
              <label style="font-size:.48rem;text-transform:uppercase;letter-spacing:.15em;color:rgba(180,100,255,.5);font-family:'JetBrains Mono',monospace;display:block;margin-bottom:3px">Sync with wave</label>
              <input type="checkbox" class="harm-toggle" id="zetaSyncWave" style="background:rgba(180,100,255,.12);border-color:rgba(180,100,255,.3);" checked>
            </div>
          </div>

          <!-- Mode tabs -->
          <div style="display:flex;gap:4px;margin-bottom:10px">
            <button class="gap-tab active" id="zetaTab0" onclick="zetaSwitchMode(0)"
              style="--tab-col:rgba(180,100,255,.7)">Spiral 2D</button>
            <button class="gap-tab" id="zetaTab1" onclick="zetaSwitchMode(1)"
              style="--tab-col:rgba(180,100,255,.7)">Zeros on Sphere</button>
            <button class="gap-tab" id="zetaTab2" onclick="zetaSwitchMode(2)"
              style="--tab-col:rgba(180,100,255,.7)">3D Curve</button>
          </div>

          <!-- Status line -->
          <div id="zetaStatus" style="font-size:.5rem;color:rgba(180,100,255,.45);font-family:'JetBrains Mono',monospace;margin-bottom:8px;min-height:14px">
            Set T<sub>max</sub> and click Compute to draw ζ(½+it).
          </div>

          <!-- Spiral canvas (mode 0) -->
          <div id="zetaMode0" style="width:100%">
            <canvas id="zetaCanvas" style="width:100%;border-radius:6px;background:#070510;display:block"></canvas>
            <!-- Zero list -->
            <div id="zetaZeroList" style="display:none;margin-top:8px;padding:8px 12px;background:rgba(180,100,255,.04);border:1px solid rgba(180,100,255,.1);border-radius:6px">
              <div style="font-size:.44rem;text-transform:uppercase;letter-spacing:.18em;color:rgba(180,100,255,.4);font-family:'JetBrains Mono',monospace;margin-bottom:5px">Zeros found in range</div>
              <div id="zetaZeroItems" style="font-size:.56rem;color:rgba(200,160,255,.7);font-family:'JetBrains Mono',monospace;line-height:2;display:flex;flex-wrap:wrap;gap:4px 12px"></div>
            </div>
          </div>

          <!-- Sphere zeros note (mode 1) -->
          <div id="zetaMode1" style="display:none;padding:10px 14px;background:rgba(180,100,255,.04);border:1px solid rgba(180,100,255,.12);border-radius:6px">
            <div style="font-size:.54rem;color:rgba(200,160,255,.7);font-family:'JetBrains Mono',monospace;line-height:1.9">
              Zero t-values are overlaid on the sphere above as glowing rings.<br>
              Each ring corresponds to a known non-trivial zero of ζ(½+it).<br>
              <span id="zetaSphereZeroCount" style="color:rgba(200,140,255,.9)">Compute to activate.</span>
            </div>
          </div>

          <!-- 3D curve canvas (mode 2) -->
          <div id="zetaMode2" style="display:none;width:100%">
            <canvas id="zeta3dCanvas" style="width:100%;border-radius:6px;background:#070510;display:block"></canvas>
            <div style="font-size:.48rem;color:rgba(180,100,255,.35);font-family:'JetBrains Mono',monospace;margin-top:5px;text-align:center">
              ζ(½+it) plotted as 3D tube — drag to rotate
            </div>
          </div>

        </div><!-- /.accordion-body #zetaAccBody -->
      </div><!-- /.zeta-accordion -->

    </div><!-- /.center-bottom -->

  </main>

  <!-- right col removed — content in sidebar and center -->

</div><!-- /.page-shell -->

<!-- ══ GIF / WebM Export Modal ════════════════════════════════ -->
<div class="gif-modal-backdrop" id="gifModalBackdrop">
  <div class="gif-modal" role="dialog" aria-modal="true" aria-label="Export Rotation GIF">
    <div class="gif-modal-title">
      🎞 Export Rotation Animation
      <button class="gif-modal-close" id="gifModalClose" title="Close">✕</button>
    </div>

    <!-- Rotation axis selection -->
    <div class="gif-opt">
      <label>Rotation Axis</label>
      <div class="gif-axis-row">
        <button class="gif-axis-btn active" data-axis="Y">Y axis</button>
        <button class="gif-axis-btn" data-axis="X">X axis</button>
        <button class="gif-axis-btn" data-axis="Z">Z axis</button>
        <button class="gif-axis-btn" data-axis="XYZ">X→Y→Z</button>
      </div>
    </div>

    <div class="gif-opt-grid">
      <!-- Format -->
      <div class="gif-opt">
        <label>Format</label>
        <select id="gifFormat">
          <option value="webm">WebM video (full colour, best quality)</option>
          <option value="gif">Animated GIF (256 colours)</option>
        </select>
      </div>

      <!-- Resolution -->
      <div class="gif-opt">
        <label>Resolution</label>
        <select id="gifRes">
          <option value="1920">1920 × 1920 (2K)</option>
          <option value="2560">2560 × 2560 (2.5K)</option>
          <option value="3840" selected>3840 × 3840 (4K)</option>
        </select>
      </div>

      <!-- Duration -->
      <div class="gif-opt">
        <label>Duration (seconds)</label>
        <input type="number" id="gifDuration" min="2" max="30" step="1" value="6">
      </div>

      <!-- FPS -->
      <div class="gif-opt">
        <label>Frame Rate (fps)</label>
        <select id="gifFps">
          <option value="15">15 fps</option>
          <option value="24" selected>24 fps</option>
          <option value="30">30 fps</option>
          <option value="60">60 fps</option>
        </select>
      </div>

      <!-- Quality (GIF only) -->
      <div class="gif-opt" id="gifQualityWrap">
        <label>GIF Quality (1=best, 10=fast)</label>
        <input type="number" id="gifQuality" min="1" max="20" step="1" value="3">
      </div>

      <!-- Rotation speed -->
      <div class="gif-opt">
        <label>Rotation Speed (°/frame)</label>
        <input type="number" id="gifRotSpeed" min="0.5" max="12" step="0.5" value="2">
      </div>
    </div>

    <!-- Info note -->
    <div style="font-size:.58rem;color:rgba(180,200,180,.45);font-family:'JetBrains Mono',monospace;line-height:1.7;background:rgba(255,255,255,.02);border-radius:5px;padding:8px 12px;">
      <strong style="color:rgba(232,197,71,.6)">WebM:</strong> rendered at full resolution with MediaRecorder — download and convert to GIF with any video tool.<br>
      <strong style="color:rgba(232,197,71,.6)">GIF:</strong> pure-JS encoder runs in-browser; large resolutions may take a minute to encode.
    </div>

    <!-- Progress -->
    <div class="gif-progress-wrap" id="gifProgressWrap">
      <div class="gif-progress-bar" id="gifProgressBar"></div>
    </div>
    <div class="gif-status" id="gifStatus"></div>

    <div class="gif-btn-row">
      <button class="gif-btn cancel" id="gifCancelBtn">Cancel</button>
      <button class="gif-btn go" id="gifGoBtn">▶ Export</button>
    </div>
  </div>
</div>

<script>
// ═══════════════════════════════════════════════════════════════
//  PRESETS
// ═══════════════════════════════════════════════════════════════
const PRESET_GROUPS = {
  pgClassic: [
    { name:'Original',   A:1.9, B:3,  C:3,  D:32,  theta:64,  trail:6  },
    { name:'Halo',       A:2,   B:1,  C:1,  D:128, theta:4,   trail:1  },
    { name:'Daisy',      A:1.0, B:5,  C:5,  D:20,  theta:40,  trail:6  },
    { name:'Sunflower',  A:1.0, B:5,  C:5,  D:21,  theta:42,  trail:5  },
    { name:'Rose',       A:2.0, B:4,  C:4,  D:16,  theta:32,  trail:6  },
    { name:'Lily',       A:1.5, B:6,  C:6,  D:18,  theta:36,  trail:6  },
  ],
  pgComplex: [
    { name:'Hypnotic',   A:0.5, B:7,  C:7,  D:16,  theta:64,  trail:8  },
    { name:'Galaxy',     A:1.5, B:9,  C:1,  D:64,  theta:128, trail:4  },
    { name:'Fractal',    A:0.8, B:11, C:11, D:43,  theta:86,  trail:8  },
    { name:'Mandala',    A:1.2, B:8,  C:8,  D:40,  theta:80,  trail:6  },
    { name:'Deep Space', A:0.6, B:12, C:3,  D:96,  theta:128, trail:3  },
    { name:'Cosmos',     A:1.0, B:10, C:2,  D:80,  theta:128, trail:4  },
    { name:'Nebula',     A:1.3, B:9,  C:4,  D:72,  theta:108, trail:5  },
    { name:'Quasar',     A:0.7, B:11, C:7,  D:55,  theta:110, trail:3  },
  ],
  pgOrganic: [
    { name:'Butterfly',  A:2.0, B:2,  C:4,  D:8,   theta:32,  trail:10 },
    { name:'Jellyfish',  A:1.8, B:3,  C:2,  D:12,  theta:48,  trail:8  },
    { name:'Fern',       A:1.1, B:2,  C:5,  D:10,  theta:40,  trail:7  },
    { name:'Coral',      A:1.4, B:4,  C:2,  D:28,  theta:56,  trail:6  },
    { name:'Anemone',    A:1.6, B:5,  C:3,  D:15,  theta:60,  trail:6  },
    { name:'Seashell',   A:2.2, B:3,  C:1,  D:24,  theta:48,  trail:5  },
    { name:'Vine',       A:1.0, B:3,  C:7,  D:14,  theta:56,  trail:6  },
    { name:'Mycelium',   A:0.9, B:6,  C:2,  D:36,  theta:72,  trail:4  },
  ],
  pgGeo: [
    { name:'Crystal',    A:2.5, B:4,  C:4,  D:24,  theta:48,  trail:6  },
    { name:'Spiral',     A:1.2, B:1,  C:6,  D:12,  theta:96,  trail:5  },
    { name:'Hexagon',    A:2.0, B:6,  C:6,  D:36,  theta:72,  trail:6  },
    { name:'Star-12',    A:1.8, B:12, C:12, D:48,  theta:96,  trail:7  },
    { name:'Diamond',    A:2.1, B:4,  C:8,  D:20,  theta:40,  trail:6  },
    { name:'Pinwheel',   A:1.3, B:3,  C:9,  D:18,  theta:54,  trail:6  },
    { name:'Web',        A:0.9, B:7,  C:3,  D:42,  theta:84,  trail:5  },
    { name:'Lattice',    A:1.7, B:5,  C:7,  D:35,  theta:70,  trail:5  },
  ],
  pgEth: [
    { name:'Whisper',    A:3.5, B:2,  C:1,  D:16,  theta:8,   trail:2  },
    { name:'Phantom',    A:0.3, B:8,  C:8,  D:32,  theta:64,  trail:2  },
    { name:'Aurora',     A:1.9, B:4,  C:1,  D:48,  theta:96,  trail:3  },
    { name:'Mirage',     A:2.3, B:5,  C:2,  D:30,  theta:60,  trail:4  },
    { name:'Specter',    A:1.5, B:7,  C:2,  D:56,  theta:112, trail:2  },
    { name:'Wraith',     A:0.5, B:10, C:5,  D:100, theta:100, trail:1  },
    { name:'Solstice',   A:2.8, B:3,  C:3,  D:64,  theta:64,  trail:5  },
    { name:'Eclipse',    A:1.9, B:2,  C:6,  D:12,  theta:24,  trail:3  },
  ],
};

// ═══════════════════════════════════════════════════════════════
//  GAP COLOR PALETTE (shared everywhere)
// ═══════════════════════════════════════════════════════════════
const GAP_PALETTE = {
  2:'#ffd700', 4:'#4ecdc4', 6:'#a855f7', 8:'#f97316',
  10:'#22c55e', 12:'#38bdf8', 14:'#fb7185', 18:'#e879f9',
  24:'#facc15', 30:'#34d399', 36:'#60a5fa', 42:'#f87171',
  48:'#a3e635', 72:'#fb923c', 86:'#c084fc',
};
const FALLBACK_COLORS = [
  '#ffd700','#4ecdc4','#a855f7','#f97316','#22c55e','#38bdf8',
  '#fb7185','#e879f9','#facc15','#34d399','#60a5fa','#f87171',
  '#a3e635','#fb923c','#c084fc','#ff6b9d','#00d2ff','#7fff00',
];
function gapColor(g, idx){ return GAP_PALETTE[g] || FALLBACK_COLORS[idx % FALLBACK_COLORS.length]; }

// ═══════════════════════════════════════════════════════════════
//  MULTI-GAP SELECTION STATE
// ═══════════════════════════════════════════════════════════════
// Map: gapValue → color string (when selected)
let selectedGaps = new Map();
// Lookup: gapValue → index in allGaps array (for color assignment)
let gapIndexMap = {};

// ═══════════════════════════════════════════════════════════════
//  VISUALIZATION STATE
// ═══════════════════════════════════════════════════════════════
let A=2, B=1, C=3, D=32, thetaTotal=64*Math.PI, trailFade=6;
let speed=8, paused=true, looping=false;
let traceThickness = 1.35;

const TOTAL_PTS = 8000;
let precomputed = [];
let drawProgress = 0;
let lastCartProg = 0;

// Precomputed stats
let fMin=Infinity, fMax=-Infinity, fSum=0;

// 3D rotation
let rotMat = [1,0,0, 0,1,0, 0,0,1];
let autoRotate = false;
let seqRotateOnComplete = false; // true when preset triggers auto-rotate on finish
let isDrag=false, lastMX=0, lastMY=0;

// Manual rotation angles (in degrees)
let manualRotX = 90;
let manualRotY = 0;
let manualRotZ = 0;

// Wrap angle to 0-360 range
function wrapAngle(angle) {
  angle = angle % 360;
  if (angle < 0) angle += 360;
  return angle;
}

// Function to build rotation matrix from manual angles
function updateManualRotation() {
  const rx = manualRotX * Math.PI / 180;
  const ry = manualRotY * Math.PI / 180;
  const rz = manualRotZ * Math.PI / 180;
  // Apply rotations in order: Z, Y, X
  rotMat = mmul(mrotX(rx), mmul(mrotY(ry), mrotZ(rz)));
}

// Function to extract Euler angles from rotation matrix (XYZ order)
function extractEulerAngles() {
  // Extract angles from rotation matrix
  // Assuming rotation order: Z, Y, X
  const sy = Math.sqrt(rotMat[0] * rotMat[0] + rotMat[3] * rotMat[3]);
  
  let x, y, z;
  
  if (sy > 1e-6) {
    x = Math.atan2(rotMat[7], rotMat[8]);
    y = Math.atan2(-rotMat[6], sy);
    z = Math.atan2(rotMat[3], rotMat[0]);
  } else {
    x = Math.atan2(-rotMat[5], rotMat[4]);
    y = Math.atan2(-rotMat[6], sy);
    z = 0;
  }
  
  return {
    x: wrapAngle(x * 180 / Math.PI),
    y: wrapAngle(y * 180 / Math.PI),
    z: wrapAngle(z * 180 / Math.PI)
  };
}

// Function to update input fields and sliders from rotation matrix
function updateRotationInputs() {
  const angles = extractEulerAngles();
  manualRotX = angles.x;
  manualRotY = angles.y;
  manualRotZ = angles.z;
  
  const roundedX = Math.round(angles.x);
  const roundedY = Math.round(angles.y);
  const roundedZ = Math.round(angles.z);
  
  document.getElementById('rotX').value = roundedX;
  document.getElementById('rotY').value = roundedY;
  document.getElementById('rotZ').value = roundedZ;
  
  document.getElementById('rotXSlider').value = roundedX;
  document.getElementById('rotYSlider').value = roundedY;
  document.getElementById('rotZSlider').value = roundedZ;
}

// Initialize rotation matrix with default angles
updateManualRotation();

const PAD = {l:38,r:14,t:12,b:26};

// ═══════════════════════════════════════════════════════════════
//  CANVASES
// ═══════════════════════════════════════════════════════════════
const pCv = document.getElementById('polarCanvas');
const pCtx = pCv.getContext('2d');
const cCv = document.getElementById('cartCanvas');
const cCtx = cCv.getContext('2d');
let cBuf;

function setSize() {
  const leftSidebar = window.innerWidth > 900 ? 320 : 0;
  const sphereW = Math.min(window.innerWidth - leftSidebar - 80, 900);
  pCv.width = pCv.height = sphereW;
  const gapVisible = document.getElementById('gapWavePanel') &&
    !document.getElementById('gapWavePanel').classList.contains('gap-hidden');
  const cartW = gapVisible ? Math.floor((sphereW - 12) / 2) : sphereW;

  // Base height + extra per selected gap lane (each lane needs ~52px min to be readable)
  const baseH = Math.round(cartW * 0.42);
  const nGaps = selectedGaps ? selectedGaps.size : 0;
  const LANE_PX = 52; // min px per lane
  const neededH = nGaps > 0 ? Math.max(baseH, baseH * 0.55 + nGaps * LANE_PX) : baseH;
  cCv.width = cartW;
  cCv.height = Math.round(neededH);
}
setSize();
// ── Robust resize handling ────────────────────────────────────────
// Mobile browsers fire 'resize' when the address bar hides/shows during scroll.
// We use visualViewport.width (stable during scroll) and only reset when the
// LAYOUT width actually changes by more than 8px.
let _lastLayoutW = 0;

function onLayoutResize() {
  const vvw = (window.visualViewport ? window.visualViewport.width : window.innerWidth);
  const newW = Math.round(vvw);
  if (Math.abs(newW - _lastLayoutW) < 8) return;
  _lastLayoutW = newW;

  const prevW = pCv.width;
  setSize();
  if (pCv.width !== prevW) {
    hardReset();
  }
  gdResizeCanvas();
}

if (window.visualViewport) {
  window.visualViewport.addEventListener('resize', onLayoutResize);
}
window.addEventListener('resize', onLayoutResize);

// ═══════════════════════════════════════════════════════════════
//  3D MATH
// ═══════════════════════════════════════════════════════════════
function mmul(a, b) {
  return [
    a[0]*b[0]+a[1]*b[3]+a[2]*b[6], a[0]*b[1]+a[1]*b[4]+a[2]*b[7], a[0]*b[2]+a[1]*b[5]+a[2]*b[8],
    a[3]*b[0]+a[4]*b[3]+a[5]*b[6], a[3]*b[1]+a[4]*b[4]+a[5]*b[7], a[3]*b[2]+a[4]*b[5]+a[5]*b[8],
    a[6]*b[0]+a[7]*b[3]+a[8]*b[6], a[6]*b[1]+a[7]*b[4]+a[8]*b[7], a[6]*b[2]+a[7]*b[5]+a[8]*b[8]
  ];
}
function mrotX(a) { const c=Math.cos(a),s=Math.sin(a); return [1,0,0, 0,c,-s, 0,s,c]; }
function mrotY(a) { const c=Math.cos(a),s=Math.sin(a); return [c,0,s, 0,1,0, -s,0,c]; }
function mrotZ(a) { const c=Math.cos(a),s=Math.sin(a); return [c,-s,0, s,c,0, 0,0,1]; }
function applyM(m, x, y, z) {
  return [m[0]*x+m[1]*y+m[2]*z, m[3]*x+m[4]*y+m[5]*z, m[6]*x+m[7]*y+m[8]*z];
}
function proj(rx, ry, rz) {
  const fov = 2.8, scale = fov / (fov - rz);
  const R = pCv.width * 0.37;
  return [pCv.width/2 + rx*R*scale, pCv.height/2 - ry*R*scale];
}

// ═══════════════════════════════════════════════════════════════
//  FORMULA & SPHERE MAPPING
// ═══════════════════════════════════════════════════════════════
function f(t) { return A + Math.sin(B*t) * Math.sin(C*t/D); }

function toSphere(t, r) {
  const lon = t;
  const polar = (t / thetaTotal) * Math.PI;
  const rn = r / (A + 1.2);
  const x = rn * Math.sin(polar) * Math.cos(lon);
  const y = rn * Math.cos(polar);
  const z = rn * Math.sin(polar) * Math.sin(lon);
  return [x, y, z];
}

function updateFormula() {
  document.getElementById('fA').textContent = A;
  document.getElementById('fB').textContent = B;
  document.getElementById('fC').textContent = C;
  document.getElementById('fD').textContent = D;
  // Sync topbar formula
}

// ═══════════════════════════════════════════════════════════════
//  PRECOMPUTE — now stores gap-class identity per point
// ═══════════════════════════════════════════════════════════════
function precompute() {
  precomputed = new Array(TOTAL_PTS);
  fMin=Infinity; fMax=-Infinity; fSum=0;

  // Build a fast lookup: for each point index → which gap class does it visually correspond to
  // We derive this from the gdData if available: primes falling inside [t, t+dt] map to gap families.
  // For the sphere rendering, gap assignment is computed from visualization parameters (B, C, D)
  // We use a simpler approach: tag each point by the nearest gap class from gapFamilies if data exists.
  // Without data, gap = null (no coloring).
  let gapAtPoint = null;
  if (gdData) {
    gapAtPoint = buildGapAtPoint();
  }

  for (let i = 0; i < TOTAL_PTS; i++) {
    const t = (i / TOTAL_PTS) * thetaTotal;
    const r = f(t);
    if (r < fMin) fMin = r;
    if (r > fMax) fMax = r;
    fSum += r;
    const [x,y,z] = harmArnoldK > 0 ? toSphereWarped(t, r) : toSphere(t, r);
    const hue = harmColorMode
      ? null  // will be set to harmonic color below
      : 295 + (i/TOTAL_PTS)*40;
    const gap = gapAtPoint ? gapAtPoint[i] : null;
    const harmCol = harmColorMode ? harmColorForProgress(i/TOTAL_PTS) : null;
    precomputed[i] = [x, y, z, hue, gap, harmCol];
  }
  document.getElementById('stFMin').textContent = fMin.toFixed(4);
  document.getElementById('stFMax').textContent = fMax.toFixed(4);
  document.getElementById('stFMean').textContent = (fSum/TOTAL_PTS).toFixed(4);
  document.getElementById('stThetaMax').textContent = Math.round(thetaTotal/Math.PI);
}

// Map each point index to a gap class by finding which prime gap the θ value falls within.
// When raw prime array is unavailable (large N), we return null so caller handles gracefully.
function buildGapAtPoint() {
  const { gapFamilies, _noRawPrimes } = gdData;
  // If we don't have raw prime values, gap-point mapping is unavailable
  if (_noRawPrimes) return null;
  const primes = gdData.primes;
  if (!primes || !Array.isArray(primes) || primes.length < 2) return null;
  const maxP = primes[primes.length-1];

  // Build sorted list of [prime, gap] pairs for binary search
  const pArr = primes; // already sorted
  const result = new Array(TOTAL_PTS).fill(null);

  for (let i = 0; i < TOTAL_PTS; i++) {
    const t = (i / TOTAL_PTS) * thetaTotal;
    // Map t (0..thetaTotal) → prime index using t/thetaTotal * primes.length
    const pIdx = Math.min(pArr.length - 2, Math.floor((t / thetaTotal) * (pArr.length - 1)));
    if (pIdx >= 0 && pIdx < pArr.length - 1) {
      result[i] = pArr[pIdx+1] - pArr[pIdx];
    }
  }
  return result;
}

// ═══════════════════════════════════════════════════════════════
//  CARTESIAN BUFFER
// ═══════════════════════════════════════════════════════════════
function makeCartBuf() {
  cBuf = document.createElement('canvas');
  cBuf.width = cCv.width; cBuf.height = cCv.height;
  drawCartGrid(cBuf.getContext('2d'));
  lastCartProg = 0;
}

// Whether to show negative extent (toggled by #cbNegA)
let cartNegA = true;

function getCartYRange() {
  const yMax = Math.ceil(A + 1.2);
  // When A is near 0, the wave f(θ)=A+sin*sin can dip below 0 — always show negative if A<0.5
  const showNeg = cartNegA || A < 0.5;
  // yMin: if negative shown, extend downward by same amplitude (sin amplitude ≤ 1)
  const yMin = showNeg ? -(Math.min(A, 1) + 0.2) : 0;
  return { yMin, yMax };
}

function drawCartGrid(ctx) {
  const w=ctx.canvas.width, h=ctx.canvas.height;
  const {l,r:pr,t,b}=PAD, pw=w-l-pr, ph=h-t-b;
  const { yMin, yMax } = getCartYRange();
  const yRange = yMax - yMin;
  // Y-axis origin pixel position
  const yZeroPy = h - b - ((-yMin) / yRange) * ph;

  ctx.fillStyle='#060608'; ctx.fillRect(0,0,w,h);
  ctx.font=`${Math.max(8,w*.021)}px JetBrains Mono,monospace`;

  // Horizontal grid lines (integer ticks in range)
  const yStep = yRange > 4 ? 2 : 1;
  for(let yv = Math.ceil(yMin); yv <= Math.floor(yMax); yv += yStep){
    const py = h - b - ((yv - yMin) / yRange) * ph;
    ctx.strokeStyle = yv === 0 ? 'rgba(91,156,246,0.18)' : '#182018';
    ctx.lineWidth = yv === 0 ? 1 : 0.7;
    ctx.beginPath(); ctx.moveTo(l,py); ctx.lineTo(w-pr,py); ctx.stroke();
    ctx.fillStyle = yv < 0 ? '#7040a0' : '#3a7040';
    ctx.textAlign='right'; ctx.textBaseline='middle';
    ctx.fillText(yv, l-5, py);
  }

  // Vertical grid (theta)
  const tmPi=Math.round(thetaTotal/Math.PI);
  [4,8,16,32,48,64,96,128,160,192,240,300].filter(v=>v<=tmPi).forEach(tv=>{
    const px=l+(tv/tmPi)*pw;
    if(px<w-pr){
      ctx.strokeStyle='#182018'; ctx.lineWidth=0.7;
      ctx.beginPath(); ctx.moveTo(px,t); ctx.lineTo(px,h-b); ctx.stroke();
      ctx.fillStyle='#3a7040'; ctx.textAlign='center'; ctx.textBaseline='top';
      ctx.fillText(tv+'π',px,h-b+4);
    }
  });

  // Axes
  ctx.strokeStyle='var(--blue,#5b9cf6)'; ctx.fillStyle='var(--blue,#5b9cf6)'; ctx.lineWidth=1.5;
  // Y axis
  ctx.beginPath(); ctx.moveTo(l,t); ctx.lineTo(l,h-b+6); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(l-4,t+4); ctx.lineTo(l,t-5); ctx.lineTo(l+4,t+4); ctx.fill();
  // X axis at y=0 line
  ctx.beginPath(); ctx.moveTo(l-6,yZeroPy); ctx.lineTo(w-pr+6,yZeroPy); ctx.stroke();
  ctx.beginPath(); ctx.moveTo(w-pr+2,yZeroPy-4); ctx.lineTo(w-pr+7,yZeroPy); ctx.lineTo(w-pr+2,yZeroPy+4); ctx.fill();
}

function cartXY(t, rv) {
  const {l,r:pr,t:pt,b}=PAD, pw=cCv.width-l-pr, ph=cCv.height-pt-b;
  const { yMin, yMax } = getCartYRange();
  const yRange = yMax - yMin;
  return [l+(t/thetaTotal)*pw, cCv.height-b-((rv - yMin)/yRange)*ph];
}

// ═══════════════════════════════════════════════════════════════
//  SPHERE WIREFRAME
// ═══════════════════════════════════════════════════════════════
function drawSphereGrid(ctx) {
  ctx.lineWidth=0.55;
  for(let li=-2; li<=2; li++){
    const lat=(li/3)*Math.PI/2;
    const cosLat=Math.cos(lat), sinLat=Math.sin(lat);
    ctx.beginPath(); let first=true;
    for(let lo=0; lo<=Math.PI*2+0.05; lo+=0.07){
      const x=cosLat*Math.cos(lo), y=sinLat, z=cosLat*Math.sin(lo);
      const [rx,ry,rz]=applyM(rotMat,x,y,z);
      const [px,py]=proj(rx,ry,rz);
      ctx.globalAlpha=Math.max(0.05, 0.06+(rz+1)*0.12);
      ctx.strokeStyle='#3a7040';
      if(first){ctx.moveTo(px,py);first=false;}else ctx.lineTo(px,py);
    }
    ctx.stroke();
  }
  for(let lo=0; lo<8; lo++){
    const lon=(lo/8)*Math.PI*2;
    const cosLon=Math.cos(lon), sinLon=Math.sin(lon);
    ctx.beginPath(); let first=true;
    for(let la=-Math.PI/2; la<=Math.PI/2+0.05; la+=0.08){
      const x=Math.cos(la)*cosLon, y=Math.sin(la), z=Math.cos(la)*sinLon;
      const [rx,ry,rz]=applyM(rotMat,x,y,z);
      const [px,py]=proj(rx,ry,rz);
      ctx.globalAlpha=Math.max(0.05, 0.06+(rz+1)*0.12);
      ctx.strokeStyle='#3a7040';
      if(first){ctx.moveTo(px,py);first=false;}else ctx.lineTo(px,py);
    }
    ctx.stroke();
  }
  ctx.globalAlpha=1;
}

// ═══════════════════════════════════════════════════════════════
//  RENDER — sphere base + per-gap isolated paths
// ═══════════════════════════════════════════════════════════════
const HUE_BANDS = 24;

function renderSphere() {
  pCtx.fillStyle='#060608';
  pCtx.fillRect(0,0,pCv.width,pCv.height);
  drawSphereGrid(pCtx);

  const end = Math.floor(drawProgress);
  if (end < 2) return;

  // Always draw from the beginning (index 1) to keep full trail visible
  const startIdx = 1;
  const hasGapSel = selectedGaps.size > 0;
  const dimBase = hasGapSel;

  // ── Base continuous curve with Catmull-Rom smoothing ──
  // Sub-divide each segment to preserve smoothness at high speed
  const N_SUB = Math.max(1, Math.min(8, Math.ceil(speed / 4)));
  const bands = [];
  for(let b=0; b<HUE_BANDS; b++) bands.push({segs:[], bright:0, n:0});

  // Catmull-Rom interpolation in 3D sphere-space (before projection)
  function crPoint(p0, p1, p2, p3, t) {
    // p0..p3 are [x,y,z,...] arrays; t in [0,1]
    const t2=t*t, t3=t2*t;
    return [
      0.5*((-p0[0]+3*p1[0]-3*p2[0]+p3[0])*t3+(2*p0[0]-5*p1[0]+4*p2[0]-p3[0])*t2+(-p0[0]+p2[0])*t+2*p1[0]),
      0.5*((-p0[1]+3*p1[1]-3*p2[1]+p3[1])*t3+(2*p0[1]-5*p1[1]+4*p2[1]-p3[1])*t2+(-p0[1]+p2[1])*t+2*p1[1]),
      0.5*((-p0[2]+3*p1[2]-3*p2[2]+p3[2])*t3+(2*p0[2]-5*p1[2]+4*p2[2]-p3[2])*t2+(-p0[2]+p2[2])*t+2*p1[2]),
    ];
  }

  for(let i=startIdx; i<end; i++){
    const p0=precomputed[Math.max(0,i-1)];
    const p1=precomputed[i];
    const p2=precomputed[Math.min(TOTAL_PTS-1,i+1)];
    const p3=precomputed[Math.min(TOTAL_PTS-1,i+2)];
    if(!p0||!p1||!p2||!p3) continue;
    // Color bands based on position in the full drawn range
    const bi=Math.min(HUE_BANDS-1, Math.floor((i-startIdx)/(end-startIdx)*HUE_BANDS));
    // Walk N_SUB sub-intervals
    for(let s=0; s<N_SUB; s++){
      const ta=s/N_SUB, tb=(s+1)/N_SUB;
      const [ax,ay,az]=crPoint(p0,p1,p2,p3,ta);
      const [bx,by,bz]=crPoint(p0,p1,p2,p3,tb);
      const [rxa,rya,rza]=applyM(rotMat,ax,ay,az);
      const [rxb,ryb,rzb]=applyM(rotMat,bx,by,bz);
      const [pxa,pya]=proj(rxa,rya,rza);
      const [pxb,pyb]=proj(rxb,ryb,rzb);
      const depth=(rza+rzb)*0.5;
      bands[bi].segs.push(pxa,pya,pxb,pyb);
      bands[bi].bright+=depth; bands[bi].n++;
    }
  }

  if(dimBase){
    for(let bi=0; bi<HUE_BANDS; bi++){
      const bd=bands[bi]; if(!bd.segs.length) continue;
      const avgZ=bd.n>0?bd.bright/bd.n:0;
      pCtx.strokeStyle=`rgba(55,75,55,${Math.max(0.04,0.08+avgZ*0.06)})`;
      pCtx.lineWidth=traceThickness*0.5; pCtx.globalAlpha=1; pCtx.shadowBlur=0;
      pCtx.beginPath();
      for(let j=0;j<bd.segs.length;j+=4){ pCtx.moveTo(bd.segs[j],bd.segs[j+1]); pCtx.lineTo(bd.segs[j+2],bd.segs[j+3]); }
      pCtx.stroke();
    }
  } else {
    pCtx.lineWidth=traceThickness; pCtx.shadowColor='var(--blue,#5b9cf6)'; pCtx.shadowBlur=4;
    for(let bi=0; bi<HUE_BANDS; bi++){
      const bd=bands[bi]; if(!bd.segs.length) continue;
      const hue=295+(bi/HUE_BANDS)*40;
      const avgZ=bd.n>0?bd.bright/bd.n:0;
      const bright=48+avgZ*22;
      // Use harmonic color if mode active, else standard hue
      const harmBandCol = harmColorMode ? (harmColorForProgress(bi/HUE_BANDS)||`hsl(${hue},100%,${bright}%)`) : `hsl(${hue},100%,${bright}%)`;
      pCtx.strokeStyle = harmBandCol;
      pCtx.globalAlpha=Math.max(0.2, 0.4+avgZ*0.55);
      pCtx.beginPath();
      for(let j=0;j<bd.segs.length;j+=4){ pCtx.moveTo(bd.segs[j],bd.segs[j+1]); pCtx.lineTo(bd.segs[j+2],bd.segs[j+3]); }
      pCtx.stroke();
    }
    pCtx.globalAlpha=1; pCtx.shadowBlur=0;
  }

  // ── Per-gap smooth Catmull-Rom arcs on sphere ──
  if(hasGapSel){
    selectedGaps.forEach((col, gv) => {
      const pts = [];
      for(let i=startIdx; i<end; i++){
        if(precomputed[i] && precomputed[i][4]===gv) pts.push(i);
      }
      if(!pts.length) return;

      // Glowing dots at each occurrence
      pCtx.shadowColor=col; pCtx.shadowBlur=10; pCtx.globalAlpha=0.9;
      pCtx.fillStyle=col;
      pts.forEach(i=>{
        const p=precomputed[i];
        const [rx,ry,rz]=applyM(rotMat,p[0],p[1],p[2]);
        if(rz < -0.5) return;
        const [px,py]=proj(rx,ry,rz);
        pCtx.beginPath(); pCtx.arc(px,py,Math.max(2,traceThickness),0,Math.PI*2); pCtx.fill();
      });
      pCtx.shadowBlur=0; pCtx.globalAlpha=1;

      // Catmull-Rom smooth arc connecting occurrences
      if(pts.length >= 2){
        pCtx.strokeStyle=col; pCtx.lineWidth=traceThickness*1.3;
        pCtx.shadowColor=col; pCtx.shadowBlur=7; pCtx.globalAlpha=0.75;
        pCtx.beginPath();
        let penDown=false;
        const GAP_THRESH=60; // max index gap to connect
        for(let k=0; k<pts.length-1; k++){
          const i=pts[k], j=pts[k+1];
          if(j-i > GAP_THRESH){ penDown=false; continue; }
          // Use 4 control points for Catmull-Rom
          const pi0=precomputed[Math.max(0,k>0?pts[k-1]:i)];
          const pi1=precomputed[i];
          const pi2=precomputed[j];
          const pi3=precomputed[Math.min(TOTAL_PTS-1,k+2<pts.length?pts[k+2]:j)];
          const CR_STEPS=8;
          for(let s=0; s<=CR_STEPS; s++){
            const t=s/CR_STEPS;
            const t2=t*t, t3=t2*t;
            const x=0.5*((-pi0[0]+3*pi1[0]-3*pi2[0]+pi3[0])*t3+(2*pi0[0]-5*pi1[0]+4*pi2[0]-pi3[0])*t2+(-pi0[0]+pi2[0])*t+2*pi1[0]);
            const y=0.5*((-pi0[1]+3*pi1[1]-3*pi2[1]+pi3[1])*t3+(2*pi0[1]-5*pi1[1]+4*pi2[1]-pi3[1])*t2+(-pi0[1]+pi2[1])*t+2*pi1[1]);
            const z=0.5*((-pi0[2]+3*pi1[2]-3*pi2[2]+pi3[2])*t3+(2*pi0[2]-5*pi1[2]+4*pi2[2]-pi3[2])*t2+(-pi0[2]+pi2[2])*t+2*pi1[2]);
            const [rx,ry,rz]=applyM(rotMat,x,y,z);
            if(rz<-0.6){ penDown=false; continue; }
            const [px,py]=proj(rx,ry,rz);
            if(!penDown){ pCtx.moveTo(px,py); penDown=true; } else pCtx.lineTo(px,py);
          }
        }
        pCtx.stroke();
        pCtx.globalAlpha=1; pCtx.shadowBlur=0;
      }
    });
  }

  // Moving dot
  const cur=precomputed[Math.max(0,end-1)];
  if(cur){
    const [cx,cy,cz]=cur;
    const [rx,ry,rz]=applyM(rotMat,cx,cy,cz);
    const [px,py]=proj(rx,ry,rz);
    pCtx.globalAlpha=Math.max(0.5, 0.5+(rz+1)*0.4);
    pCtx.beginPath(); pCtx.arc(px,py,3.5,0,Math.PI*2);
    pCtx.fillStyle='#fff'; pCtx.shadowColor='#fff'; pCtx.shadowBlur=14;
    pCtx.fill(); pCtx.shadowBlur=0; pCtx.globalAlpha=1;
  }

  // Center glow
  const [crx,cry,crz]=applyM(rotMat,0,0,0);
  const [cpx,cpy]=proj(crx,cry,crz);
  pCtx.beginPath(); pCtx.arc(cpx,cpy,3,0,Math.PI*2);
  pCtx.fillStyle='#ffd700'; pCtx.shadowColor='#ffd700'; pCtx.shadowBlur=16;
  pCtx.fill(); pCtx.shadowBlur=0;

  // Probe flash — highlighted point from cart click
  drawProbeFlash(pCtx, pCv.width);
}

// ═══════════════════════════════════════════════════════════════
//  RENDER CART — smooth base + FFT-style per-gap stacked lanes
// ═══════════════════════════════════════════════════════════════

// FFT canvas: drawn on top of main cart, shows gap waveform lanes
let fftCanvas = null;

function ensureFftCanvas(){
  if(!fftCanvas || fftCanvas.width !== cCv.width || fftCanvas.height !== cCv.height){
    fftCanvas = document.createElement('canvas');
    fftCanvas.width = cCv.width; fftCanvas.height = cCv.height;
  }
}

// Build FFT lane display: for each selected gap, show a local density
// waveform in a horizontal lane. Lane height = cartCanvas.height / nGaps.
// X axis = θ (same as cart), Y axis = local occurrence density in that lane.
function drawFftLanes(end){
  if(!selectedGaps.size || !precomputed.length) return;
  ensureFftCanvas();
  const ctx = fftCanvas.getContext('2d');
  const w=fftCanvas.width, h=fftCanvas.height;
  ctx.clearRect(0,0,w,h);

  const {l,r:pr,t:pt,b}=PAD;
  const pw=w-l-pr, ph=h-pt-b;
  const gapList=[...selectedGaps.entries()]; // [[gv,col],...]
  const nGaps=gapList.length;
  const laneH=Math.floor(ph/nGaps);
  const BINS=Math.min(pw, 256); // FFT-style resolution bins

  // For each gap, count occurrences per θ-bin up to current progress
  const maxIdx=Math.min(end, TOTAL_PTS-1);

  gapList.forEach(([gv,col],li)=>{
    const laneTop=pt + li*laneH;
    const laneMid=laneTop+laneH/2;
    const laneAmp=laneH*0.42;

    // Count density in each bin
    const bins=new Float32Array(BINS).fill(0);
    for(let i=1; i<=maxIdx; i++){
      if(precomputed[i]&&precomputed[i][4]===gv){
        const binIdx=Math.floor((i/maxIdx)*BINS);
        if(binIdx<BINS) bins[binIdx]++;
      }
    }

    // Smooth bins with 3-tap gaussian
    const smooth=new Float32Array(BINS);
    for(let b=0;b<BINS;b++){
      smooth[b]=(bins[Math.max(0,b-1)]*0.25+bins[b]*0.5+bins[Math.min(BINS-1,b+1)]*0.25);
    }
    const maxBin=Math.max(1,...smooth);

    // Draw lane background
    ctx.fillStyle=`${col}0d`;
    ctx.fillRect(l,laneTop,pw,laneH);
    // Lane label
    ctx.fillStyle=col+'aa';
    ctx.font=`${Math.max(8,w*0.018)}px JetBrains Mono,monospace`;
    ctx.textAlign='left'; ctx.textBaseline='middle';
    ctx.fillText(`g=${gv}`,l+4,laneMid);
    // Baseline
    ctx.strokeStyle=col+'22'; ctx.lineWidth=0.5;
    ctx.beginPath(); ctx.moveTo(l,laneMid); ctx.lineTo(l+pw,laneMid); ctx.stroke();

    // Draw FFT-style waveform using Catmull-Rom smooth path
    ctx.strokeStyle=col; ctx.lineWidth=traceThickness;
    ctx.shadowColor=col; ctx.shadowBlur=6; ctx.globalAlpha=0.9;
    ctx.beginPath();
    let first=true;
    for(let b=0;b<BINS;b++){
      const x=l+(b/BINS)*pw;
      const amp=(smooth[b]/maxBin)*laneAmp;
      const y=laneMid-amp;
      if(first){ ctx.moveTo(x,y); first=false; } else ctx.lineTo(x,y);
    }
    ctx.stroke();

    // Mirror (filled area below baseline = symmetric FFT look)
    ctx.globalAlpha=0.35;
    ctx.fillStyle=col;
    ctx.beginPath();
    ctx.moveTo(l,laneMid);
    for(let b=0;b<BINS;b++){
      const x=l+(b/BINS)*pw;
      const amp=(smooth[b]/maxBin)*laneAmp;
      ctx.lineTo(x,laneMid-amp);
    }
    for(let b=BINS-1;b>=0;b--){
      const x=l+(b/BINS)*pw;
      const amp=(smooth[b]/maxBin)*laneAmp;
      ctx.lineTo(x,laneMid+amp);
    }
    ctx.closePath(); ctx.fill();
    ctx.globalAlpha=1; ctx.shadowBlur=0;

    // Vertical cursor at drawProgress
    if(end<TOTAL_PTS){
      const cx2=l+(end/TOTAL_PTS)*pw;
      ctx.strokeStyle=col; ctx.lineWidth=1; ctx.globalAlpha=0.6;
      ctx.setLineDash([3,3]);
      ctx.beginPath(); ctx.moveTo(cx2,laneTop); ctx.lineTo(cx2,laneTop+laneH); ctx.stroke();
      ctx.setLineDash([]); ctx.globalAlpha=1;
    }

    // Lane separator
    ctx.strokeStyle='rgba(255,255,255,0.04)'; ctx.lineWidth=0.5;
    ctx.beginPath(); ctx.moveTo(l,laneTop+laneH); ctx.lineTo(l+pw,laneTop+laneH); ctx.stroke();
  });
}

function renderCart() {
  const newEnd=Math.floor(drawProgress);
  const hasGapSel = selectedGaps.size > 0;

  if(newEnd===0 && lastCartProg>0){ makeCartBuf(); }

  // Draw base f(θ) curve with sub-step smoothing
  if(newEnd > lastCartProg){
    const cbCtx=cBuf.getContext('2d');
    const C_SUB=Math.max(1,Math.min(8,Math.ceil(speed/4)));

    for(let i=lastCartProg; i<newEnd; i++){
      for(let s=0; s<C_SUB; s++){
        const ta=((i+s/C_SUB)/TOTAL_PTS)*thetaTotal;
        const tb=((i+(s+1)/C_SUB)/TOTAL_PTS)*thetaTotal;
        const ra=f(ta), rb=f(tb);
        const [cxa,cya]=cartXY(ta,ra);
        const [cxb,cyb]=cartXY(tb,rb);
        const fracA=(i+s/C_SUB)/TOTAL_PTS;
        const hue=295+fracA*40;

        // Match the sphere's color logic exactly
        const pc = precomputed[i];
        const gap = pc ? pc[4] : null;
        const selCol = (gap !== null && gap !== undefined) ? selectedGaps.get(gap) : undefined;

        if(selCol){
          // Gap is selected — use exact gap color (same as sphere)
          cbCtx.strokeStyle = selCol;
          cbCtx.lineWidth = traceThickness;
          cbCtx.shadowColor = selCol;
          cbCtx.shadowBlur = 4;
        } else if(hasGapSel){
          // Gap decomp active but this point's gap is not selected — dim it
          cbCtx.strokeStyle = 'rgba(30,50,30,0.10)';
          cbCtx.lineWidth = traceThickness * 0.3;
          cbCtx.shadowBlur = 0;
        } else if(harmColorMode){
          // Harmonic consonance color mode
          const hc = harmColorForProgress(fracA);
          cbCtx.strokeStyle = hc || `hsl(${hue},100%,62%)`;
          cbCtx.lineWidth = traceThickness;
          cbCtx.shadowColor = cbCtx.strokeStyle;
          cbCtx.shadowBlur = 3;
        } else {
          // Default hue gradient — matches sphere's `hsl(295+frac*40, 100%, bright%)`
          cbCtx.strokeStyle = `hsl(${hue},100%,62%)`;
          cbCtx.lineWidth = traceThickness;
          cbCtx.shadowColor = 'var(--blue,#5b9cf6)';
          cbCtx.shadowBlur = 3;
        }
        cbCtx.beginPath(); cbCtx.moveTo(cxa,cya); cbCtx.lineTo(cxb,cyb); cbCtx.stroke();
        cbCtx.shadowBlur = 0;
      }
    }
    lastCartProg=newEnd;
  }

  cCtx.clearRect(0,0,cCv.width,cCv.height);
  cCtx.drawImage(cBuf,0,0);

  // Overlay FFT gap lanes (always redrawn since they show full history up to cursor)
  if(hasGapSel){ drawFftLanes(newEnd); cCtx.drawImage(fftCanvas,0,0); }
}

// When gap selection changes, we need to redraw the cart buffer from scratch
function invalidateCartBuf(){
  makeCartBuf();
  lastCartProg = 0;
  fftCanvas = null; // force FFT redraw
}

// ═══════════════════════════════════════════════════════════════
//  LIVE STATS
// ═══════════════════════════════════════════════════════════════
function updateLiveStats() {
  const end = Math.floor(drawProgress);
  const pct = TOTAL_PTS > 0 ? ((end / TOTAL_PTS) * 100).toFixed(1) : '0.0';
  const t = (end / TOTAL_PTS) * thetaTotal;
  const tPi = (t / Math.PI).toFixed(2);
  const fv = end > 0 ? f(t).toFixed(4) : '—';
  document.getElementById('stProg').textContent = pct + '%';
  document.getElementById('stTheta').textContent = tPi + 'π';
  document.getElementById('stFTheta').textContent = fv;
  document.getElementById('stPts').textContent = end.toLocaleString();
}

// ═══════════════════════════════════════════════════════════════
//  SEQUENTIAL ROTATION STATE  (X→Y→Z each 360°, then stop)
// ═══════════════════════════════════════════════════════════════
// autoRotate = true → continuous Y-spin (manual button / legacy)
// seqRotate  = true → sequential X/Y/Z sweep triggered at wave end
let seqRotate = false;
let seqAxis   = 0;           // 0=X, 1=Y, 2=Z
let seqAngle  = 0;           // radians accumulated for current axis
const SEQ_SPEED = 0.012;     // radians per frame (~60fps → ~30s per axis)

function startSeqRotate() {
  seqRotate = true;
  seqAxis   = 0;
  seqAngle  = 0;
  // seqRotate and autoRotate are independent — seqRotate pauses when autoRotate is on
  // Show active state on Complete button
  const cBtn = document.getElementById('btnSkipComplete');
  if (cBtn) cBtn.classList.add('on');
  // Sync autoRotate button label if it's on
  const rBtn = document.getElementById('btnAutoRot');
  if (rBtn) {
    rBtn.classList.toggle('on', autoRotate);
    rBtn.textContent = autoRotate ? '⟳ Rotating...' : '⟳ Rotate';
  }
}

function stopSeqRotate() {
  seqRotate = false;
  const cBtn = document.getElementById('btnSkipComplete');
  if (cBtn) cBtn.classList.remove('on');
  // Restore Auto-Rotate button to reflect actual autoRotate state
  const rBtn = document.getElementById('btnAutoRot');
  if (rBtn) {
    rBtn.classList.toggle('on', autoRotate);
    rBtn.textContent = autoRotate ? '⟳ Rotating...' : '⟳ Rotate';
  }
}

// ═══════════════════════════════════════════════════════════════
//  ANIMATION LOOP
// ═══════════════════════════════════════════════════════════════
function loop() {
  requestAnimationFrame(loop);

  if (seqRotate) {
    const step = SEQ_SPEED;
    if (seqAxis === 0)      rotMat = mmul(mrotX(step), rotMat);
    else if (seqAxis === 1) rotMat = mmul(mrotY(step), rotMat);
    else                    rotMat = mmul(mrotZ(step), rotMat);
    seqAngle += step;
    if (seqAngle >= Math.PI * 2) {
      seqAngle = 0;
      seqAxis++;
      if (seqAxis > 2) stopSeqRotate(); // done with Z
    }
    if (Math.random() < 0.08) updateRotationInputs();
  } else if (autoRotate) {
    rotMat = mmul(mrotY(0.008), rotMat);
    if (Math.random() < 0.1) updateRotationInputs();
  }

  if(!paused){
    drawProgress += speed;
    if(drawProgress >= TOTAL_PTS){
      if(looping){
        drawProgress = 0;
        harmSyncScheduledUntil = 0;
        harmSyncLastPtIdx = -1;
      } else {
        drawProgress = TOTAL_PTS; paused = true;
        const btn = document.getElementById('btnPlay');
        btn.textContent = '▶ Play'; btn.classList.remove('on');
        harmSyncStop();
        stopBlendFlash();
        // Start sequential X→Y→Z rotation after preset finishes drawing
        if (seqRotateOnComplete) { startSeqRotate(); }
      }
    }
    harmSyncTick();
  }
  renderSphere();
  renderCart();
  updateLiveStats();
}

// ═══════════════════════════════════════════════════════════════
//  HARD RESET
// ═══════════════════════════════════════════════════════════════
function hardReset() {
  drawProgress=0; lastCartProg=0;
  seqRotateOnComplete=false;
  harmSyncStop();
  seqRotate = false; seqAxis = 0; seqAngle = 0;
  // Clear Complete button active state
  const cBtn=document.getElementById('btnSkipComplete');
  if(cBtn) cBtn.classList.remove('on');
  // Preserve autoRotate — it's a persistent toggle
  precompute(); makeCartBuf(); updateFormula();
}

// ═══════════════════════════════════════════════════════════════
//  DRAG ROTATION
// ═══════════════════════════════════════════════════════════════
let _autoRotateBeforeDrag = false;
let _seqRotateBeforeDrag  = false;
function onDragStart(ex,ey){ 
  isDrag=true; 
  lastMX=ex; 
  lastMY=ey; 
  // Pause rotation during drag — resume on drag end
  _autoRotateBeforeDrag = autoRotate;
  _seqRotateBeforeDrag  = seqRotate;
  autoRotate = false;
  seqRotate  = false;
}
function onDragMove(ex,ey){
  if(!isDrag) return;
  const dx=ex-lastMX, dy=ey-lastMY;
  lastMX=ex; lastMY=ey;
  const s=0.012;
  rotMat=mmul(mrotY(dx*s), rotMat);
  rotMat=mmul(mrotX(dy*s), rotMat);
  updateRotationInputs();
}
function onDragEnd(){ 
  isDrag=false; 
  // Restore rotation state after drag
  autoRotate = _autoRotateBeforeDrag;
  seqRotate  = _seqRotateBeforeDrag;
  updateRotationInputs();
}

pCv.addEventListener('mousedown', e=>{ e.preventDefault(); onDragStart(e.clientX,e.clientY); });
window.addEventListener('mousemove', e=>onDragMove(e.clientX,e.clientY));
window.addEventListener('mouseup', onDragEnd);
pCv.addEventListener('touchstart', e=>{ e.preventDefault(); const t=e.touches[0]; onDragStart(t.clientX,t.clientY); },{passive:false});
pCv.addEventListener('touchmove',  e=>{ e.preventDefault(); const t=e.touches[0]; onDragMove(t.clientX,t.clientY); },{passive:false});
pCv.addEventListener('touchend',   onDragEnd);

// ═══════════════════════════════════════════════════════════════
//  ROTATION BUTTONS
// ═══════════════════════════════════════════════════════════════
document.getElementById('btnAutoRot').addEventListener('click',function(){
  // If seqRotate is running, stop it — user is taking control
  if (seqRotate) stopSeqRotate();
  autoRotate=!autoRotate;
  this.classList.toggle('on',autoRotate);
  this.textContent=autoRotate?'⟳ Rotating...':'⟳ Rotate';
});
document.getElementById('btnResetRot').addEventListener('click',()=>{
  manualRotX = 90;
  manualRotY = 0;
  manualRotZ = 0;
  document.getElementById('rotX').value = 90;
  document.getElementById('rotY').value = 0;
  document.getElementById('rotZ').value = 0;
  document.getElementById('rotXSlider').value = 90;
  document.getElementById('rotYSlider').value = 0;
  document.getElementById('rotZSlider').value = 0;
  updateManualRotation();
  autoRotate=false; seqRotate=false; seqAxis=0; seqAngle=0;
  const rBtn=document.getElementById('btnAutoRot');
  rBtn.classList.remove('on'); rBtn.textContent='⟳ Rotate';
  const cBtn=document.getElementById('btnSkipComplete');
  cBtn.classList.remove('on');
});

// Manual rotation X - slider
document.getElementById('rotXSlider').addEventListener('input', function() {
  const val = wrapAngle(parseFloat(this.value) || 0);
  manualRotX = val;
  document.getElementById('rotX').value = Math.round(val);
  updateManualRotation();
});

// Manual rotation X - input (with wrapping)
document.getElementById('rotX').addEventListener('input', function() {
  const val = wrapAngle(parseFloat(this.value) || 0);
  manualRotX = val;
  this.value = Math.round(val);
  document.getElementById('rotXSlider').value = Math.round(val);
  updateManualRotation();
});

// Manual rotation Y - slider
document.getElementById('rotYSlider').addEventListener('input', function() {
  const val = wrapAngle(parseFloat(this.value) || 0);
  manualRotY = val;
  document.getElementById('rotY').value = Math.round(val);
  updateManualRotation();
});

// Manual rotation Y - input (with wrapping)
document.getElementById('rotY').addEventListener('input', function() {
  const val = wrapAngle(parseFloat(this.value) || 0);
  manualRotY = val;
  this.value = Math.round(val);
  document.getElementById('rotYSlider').value = Math.round(val);
  updateManualRotation();
});

// Manual rotation Z - slider
document.getElementById('rotZSlider').addEventListener('input', function() {
  const val = wrapAngle(parseFloat(this.value) || 0);
  manualRotZ = val;
  document.getElementById('rotZ').value = Math.round(val);
  updateManualRotation();
});

// Manual rotation Z - input (with wrapping)
document.getElementById('rotZ').addEventListener('input', function() {
  const val = wrapAngle(parseFloat(this.value) || 0);
  manualRotZ = val;
  this.value = Math.round(val);
  document.getElementById('rotZSlider').value = Math.round(val);
  updateManualRotation();
});

// ═══════════════════════════════════════════════════════════════
//  SPEED & THICKNESS
// ═══════════════════════════════════════════════════════════════
function setSpeed(v){
  speed=v;
  document.getElementById('sSpeed').value=v;
  document.getElementById('dSpeed').textContent=v.toFixed(2);
  updateSpeedDesc(v);
}
function updateSpeedDesc(v){
  const el=document.getElementById('speedDesc');
  if(v<0.2) el.textContent='ultra slow';
  else if(v<1) el.textContent='crawl';
  else if(v<4) el.textContent='slow';
  else if(v<12) el.textContent='normal';
  else if(v<25) el.textContent='fast';
  else el.textContent='maximum';
}

// Thickness slider
document.getElementById('gdThick').addEventListener('input', function(){
  traceThickness = parseFloat(this.value);
  document.getElementById('gdThickVal').textContent = traceThickness.toFixed(2)+'px';
  // Invalidate cart buf so it redraws with new thickness
  invalidateCartBuf();
});

// ═══════════════════════════════════════════════════════════════
//  LOOP TOGGLE
// ═══════════════════════════════════════════════════════════════
document.getElementById('cbLoop').addEventListener('change', function(){
  looping = this.checked;
  document.getElementById('loopState').textContent = looping ? 'on — loops forever' : 'off — stops at end';
});

// Negative-A extension toggle — default ON
document.getElementById('cbNegA').checked = true;
document.getElementById('cbNegA').addEventListener('change', function(){
  cartNegA = this.checked;
  invalidateCartBuf();
});

// ═══════════════════════════════════════════════════════════════
//  SLIDER BINDINGS
// ═══════════════════════════════════════════════════════════════
function bind(id,dId,setter,fmt,noReset){
  document.getElementById(id).addEventListener('input',function(){
    const v=parseFloat(this.value);
    setter(v);
    document.getElementById(dId).textContent=fmt?fmt(v):v;
    if(!noReset) hardReset();
  });
}
bind('sA','dA',v=>{
  A=v;
  const el=document.getElementById('nA'); if(el) el.value=v;
  // When A < 0.5, the wave can dip below zero — auto-check the Extend -a toggle
  const cbNeg = document.getElementById('cbNegA');
  if (cbNeg && v < 0.5 && !cbNeg.checked) {
    cbNeg.checked = true; cartNegA = true;
  }
});
bind('sB','dB',v=>{B=v; const el=document.getElementById('nB'); if(el) el.value=v;});
bind('sC','dC',v=>{C=v; const el=document.getElementById('nC'); if(el) el.value=v;});
bind('sD','dD',v=>{D=v; const el=document.getElementById('nD'); if(el) el.value=v;});
bind('sTheta','dTheta',v=>{thetaTotal=v*Math.PI; const el=document.getElementById('nTheta'); if(el) el.value=v;},v=>v+'π');
bind('sSpeed','dSpeed',v=>{speed=v;updateSpeedDesc(v); if(!paused && harmSyncEnabled) harmSyncStart(); },v=>v.toFixed(2),true);

// ── Number input ↔ slider two-way sync ──
['A','B','C','D','Theta'].forEach(p=>{
  const ni = document.getElementById('n'+p);
  const si = document.getElementById('s'+p);
  if(!ni||!si) return;
  ni.addEventListener('change',function(){
    const v = Math.max(parseFloat(si.min), Math.min(parseFloat(si.max), parseFloat(this.value)||0));
    this.value = v;
    si.value = v;
    si.dispatchEvent(new Event('input'));
  });
});

document.getElementById('btnPlay').addEventListener('click',function(){
  onUserInteraction(); // switches to loop mode after intro
  paused=!paused;
  this.textContent=paused?'▶ Play':'⏸ Pause';
  this.classList.toggle('on',!paused);
  if(!paused){ if(harmSyncEnabled) harmSyncStart(); }
  else { harmSyncPause(); }
});
document.getElementById('btnReset').addEventListener('click',()=>{ harmSyncStop(); hardReset(); });

// ── Blend slider flash every 3 seconds while playing ──
// Pulses the Coordinate Mode Blend slider green to draw attention to it.
let _blendFlashInterval = null;

function startBlendFlash(){
  if(_blendFlashInterval) return;
  _blendFlashInterval = setInterval(()=>{
    if(paused){ stopBlendFlash(); return; }
    const sl = document.getElementById('coordBlend');
    if(!sl) return;
    sl.classList.remove('flash');
    void sl.offsetWidth; // force reflow to restart animation
    sl.classList.add('flash');
    setTimeout(()=>{ if(sl) sl.classList.remove('flash'); }, 800);
  }, 3000);
}

function stopBlendFlash(){
  clearInterval(_blendFlashInterval);
  _blendFlashInterval = null;
}

// Hook into play button to start/stop flash
const _origPlayClick = document.getElementById('btnPlay').onclick;
document.getElementById('btnPlay').addEventListener('click',function(){
  if(!paused) startBlendFlash();
  else stopBlendFlash();
});

// ═══════════════════════════════════════════════════════════════
//  FULLSCREEN
// ═══════════════════════════════════════════════════════════════
function toggleFullscreen() {
  const btn = document.getElementById('btnFullscreen');
  const el = document.documentElement; // fullscreen the whole page

  if (!document.fullscreenElement && !document.webkitFullscreenElement && !document.mozFullScreenElement) {
    // Enter fullscreen
    const req = el.requestFullscreen || el.webkitRequestFullscreen || el.mozRequestFullScreen;
    if (req) req.call(el);
  } else {
    // Exit fullscreen
    const ex = document.exitFullscreen || document.webkitExitFullscreen || document.mozCancelFullScreen;
    if (ex) ex.call(document);
  }
}

function onFullscreenChange() {
  const btn = document.getElementById('btnFullscreen');
  const isFs = !!(document.fullscreenElement || document.webkitFullscreenElement || document.mozFullScreenElement);
  btn.textContent = isFs ? ' Exit' : ' Full';
  btn.classList.toggle('active', isFs);
  // Resize canvases to fit new dimensions
  setTimeout(() => { setSize(); hardReset(); }, 120);
}

document.getElementById('btnFullscreen').addEventListener('click', toggleFullscreen);
document.addEventListener('fullscreenchange', onFullscreenChange);
document.addEventListener('webkitfullscreenchange', onFullscreenChange);
document.addEventListener('mozfullscreenchange', onFullscreenChange);

// Skip to completed state — instantly draw everything, then start seq rotation
document.getElementById('btnSkipComplete').addEventListener('click', () => {
  // Ensure precomputed is up to date
  if (!precomputed.length) precompute();
  // Draw entire cart buffer at once
  drawProgress = TOTAL_PTS;
  lastCartProg = 0;
  makeCartBuf();
  // Fill the buffer with all points
  const cbCtx = cBuf.getContext('2d');
  for (let i = 1; i < TOTAL_PTS; i++) {
    const ta = ((i - 1) / TOTAL_PTS) * thetaTotal;
    const tb = (i / TOTAL_PTS) * thetaTotal;
    const ra = f(ta), rb = f(tb);
    const [cxa, cya] = cartXY(ta, ra);
    const [cxb, cyb] = cartXY(tb, rb);
    const frac = (i - 1) / TOTAL_PTS;
    const hue = 295 + frac * 40;
    cbCtx.strokeStyle = `hsl(${hue},100%,62%)`;
    cbCtx.lineWidth = traceThickness;
    cbCtx.beginPath(); cbCtx.moveTo(cxa, cya); cbCtx.lineTo(cxb, cyb); cbCtx.stroke();
  }
  lastCartProg = TOTAL_PTS;
  paused = true;
  const btn = document.getElementById('btnPlay');
  btn.textContent = '▶ Play'; btn.classList.remove('on');
  harmSyncStop();
  startSeqRotate();
});

// ═══════════════════════════════════════════════════════════════
//  4K SPHERE EXPORT
// ═══════════════════════════════════════════════════════════════
document.getElementById('btnExport4K').addEventListener('click', exportSphere4K);
document.getElementById('btnExportMulti').addEventListener('click', exportMultiAngle);

function exportSphere4K() {
  // ── Composite 4K export: sphere + formula + cart canvas + stats bar ──
  // Target output: 3840 × 2160 (16:9)
  const W = 3840, H = 2160;
  const cv = document.createElement('canvas');
  cv.width = W; cv.height = H;
  const ctx = cv.getContext('2d');

  // Background
  ctx.fillStyle = '#060608';
  ctx.fillRect(0, 0, W, H);

  // ── Layout constants (scaled to 4K) ──
  const SCALE   = W / pCv.width;       // how much bigger 4K is vs live canvas
  const PADDING = 80;                   // outer padding px at 4K
  const SPHERE_W = Math.round(W * 0.54);
  const SPHERE_H = SPHERE_W;           // sphere is square
  const RIGHT_X  = SPHERE_W + PADDING * 2;
  const RIGHT_W  = W - RIGHT_X - PADDING;

  // ══ LEFT COLUMN: sphere (scaled up from live canvas) ══
  // Draw sphere at 4K res using the same geometry
  const sphereOff = document.createElement('canvas');
  sphereOff.width = sphereOff.height = SPHERE_W;
  const sc = sphereOff.getContext('2d');
  sc.fillStyle = '#060608';
  sc.fillRect(0, 0, SPHERE_W, SPHERE_W);

  // Wireframe grid scaled to SPHERE_W
  const sphereR4K = SPHERE_W * 0.37;
  function proj4K(rx, ry, rz){
    const fov=2.8, scale=fov/(fov-rz);
    return [SPHERE_W/2 + rx*sphereR4K*scale*zoomLevel + panX*SCALE*0.5,
            SPHERE_W/2 - ry*sphereR4K*scale*zoomLevel + panY*SCALE*0.5];
  }
  sc.lineWidth = 1.2;
  for(let li=-2; li<=2; li++){
    const lat=(li/3)*Math.PI/2, cosLat=Math.cos(lat), sinLat=Math.sin(lat);
    sc.beginPath(); let first=true;
    for(let lo=0; lo<=Math.PI*2+0.05; lo+=0.04){
      const x=cosLat*Math.cos(lo), y=sinLat, z=cosLat*Math.sin(lo);
      const [rx,ry,rz]=applyM(rotMat,x,y,z);
      const [px,py]=proj4K(rx,ry,rz);
      sc.globalAlpha=Math.max(0.05,0.06+(rz+1)*0.12); sc.strokeStyle='#3a7040';
      if(first){sc.moveTo(px,py);first=false;}else sc.lineTo(px,py);
    } sc.stroke();
  }
  for(let lo=0; lo<8; lo++){
    const lon=(lo/8)*Math.PI*2, cosLon=Math.cos(lon), sinLon=Math.sin(lon);
    sc.beginPath(); let first=true;
    for(let la=-Math.PI/2; la<=Math.PI/2+0.05; la+=0.04){
      const x=Math.cos(la)*cosLon, y=Math.sin(la), z=Math.cos(la)*sinLon;
      const [rx,ry,rz]=applyM(rotMat,x,y,z);
      const [px,py]=proj4K(rx,ry,rz);
      sc.globalAlpha=Math.max(0.05,0.06+(rz+1)*0.12); sc.strokeStyle='#3a7040';
      if(first){sc.moveTo(px,py);first=false;}else sc.lineTo(px,py);
    } sc.stroke();
  }
  sc.globalAlpha=1;

  // Curve trace on sphere
  const end=Math.floor(drawProgress);
  if(end>=2){
    // Always draw from beginning to keep full trail visible
    const startIdx=1;
    sc.lineWidth=traceThickness*2.2;
    for(let i=startIdx;i<end;i++){
      const p0=precomputed[i-1],p1=precomputed[i];
      if(!p0||!p1) continue;
      const [rx1,ry1,rz1]=applyM(rotMat,p0[0],p0[1],p0[2]);
      const [rx2,ry2,rz2]=applyM(rotMat,p1[0],p1[1],p1[2]);
      const [px1,py1]=proj4K(rx1,ry1,rz1);
      const [px2,py2]=proj4K(rx2,ry2,rz2);
      const gap=p1[4];
      const selCol=gap!==null?selectedGaps.get(gap):undefined;
      const frac=i/TOTAL_PTS;
      if(selCol){
        sc.strokeStyle=selCol; sc.shadowColor=selCol; sc.shadowBlur=14;
      } else if(harmColorMode){
        const hc=harmColorForProgress(frac);
        sc.strokeStyle=hc||`hsl(${295+frac*40},100%,55%)`;
        sc.shadowColor=sc.strokeStyle; sc.shadowBlur=6;
      } else {
        const bi=Math.min(HUE_BANDS-1,Math.floor((i-startIdx)/histLen*HUE_BANDS));
        const hue=295+(bi/HUE_BANDS)*40;
        const avgZ=(rz1+rz2)*0.5, bright=48+avgZ*22;
        sc.strokeStyle=`hsl(${hue},100%,${bright}%)`;
        sc.shadowColor='var(--blue,#5b9cf6)'; sc.shadowBlur=8;
      }
      sc.globalAlpha=Math.max(0.2,0.5);
      sc.beginPath(); sc.moveTo(px1,py1); sc.lineTo(px2,py2); sc.stroke();
    }
    sc.shadowBlur=0; sc.globalAlpha=1;

    // Center glow
    const [crx,cry,crz]=applyM(rotMat,0,0,0);
    const [cpx,cpy]=proj4K(crx,cry,crz);
    sc.beginPath(); sc.arc(cpx,cpy,5,0,Math.PI*2);
    sc.fillStyle='#ffd700'; sc.shadowColor='#ffd700'; sc.shadowBlur=20;
    sc.fill(); sc.shadowBlur=0;
  }

  // Blit sphere onto main 4K canvas (vertically centered in left col)
  const SPHERE_TOP = Math.round((H - SPHERE_H) / 2);
  ctx.drawImage(sphereOff, PADDING, SPHERE_TOP, SPHERE_W, SPHERE_H);

  // ══ RIGHT COLUMN ══
  const COL_Y = PADDING;
  let ry = COL_Y;

  // ── Formula box ──
  const FORMULA_H = 160;
  // Box
  ctx.save();
  ctx.fillStyle='rgba(255,215,0,0.05)';
  ctx.strokeStyle='#ffd700'; ctx.lineWidth=3;
  ctx.shadowColor='rgba(255,215,0,0.35)'; ctx.shadowBlur=28;
  ctx.beginPath(); ctx.roundRect(RIGHT_X, ry, RIGHT_W, FORMULA_H, 16);
  ctx.fill(); ctx.stroke(); ctx.restore();

  // Draw formula with gold numbers, white text
  ctx.textBaseline='middle';
  ctx.font=`bold 54px 'JetBrains Mono', monospace`;
  const FY = ry + FORMULA_H / 2;
  // Measure total width so we can centre it
  const parts = [
    { text:'f(θ) = ',      color:'#d8f0d8' },
    { text:`${A}`,         color:'#ffd700' },
    { text:' + sin(',      color:'#d8f0d8' },
    { text:`${B}`,         color:'#ffd700' },
    { text:'θ) · sin(',    color:'#d8f0d8' },
    { text:`${C}`,         color:'#ffd700' },
    { text:'θ / ',         color:'#d8f0d8' },
    { text:`${D}`,         color:'#ffd700' },
    { text:')',             color:'#d8f0d8' },
  ];
  const totalW = parts.reduce((s,p)=>s+ctx.measureText(p.text).width,0);
  let fx = RIGHT_X + (RIGHT_W - totalW) / 2;
  ctx.textAlign='left';
  parts.forEach(p=>{ ctx.fillStyle=p.color; ctx.fillText(p.text,fx,FY); fx+=ctx.measureText(p.text).width; });
  ry += FORMULA_H + 48;

  // ── Cartesian canvas (scaled up) ──
  const CART_H = Math.round(RIGHT_W * (cCv.height / cCv.width));
  ctx.save();
  ctx.shadowColor='rgba(91,156,246,0.15)'; ctx.shadowBlur=30;
  ctx.drawImage(cCv, RIGHT_X, ry, RIGHT_W, CART_H);
  ctx.restore();
  // Border around cart
  ctx.strokeStyle='rgba(91,156,246,0.25)'; ctx.lineWidth=2;
  ctx.strokeRect(RIGHT_X, ry, RIGHT_W, CART_H);
  ry += CART_H + 48;

  // ── Live stats bar ──
  const STATS = [
    { lbl:'PROGRESS',      val: `${((drawProgress/TOTAL_PTS)*100).toFixed(1)}%` },
    { lbl:'θ CURRENT',     val: `${((drawProgress/TOTAL_PTS)*thetaTotal/Math.PI).toFixed(2)}π` },
    { lbl:'F(θ) CURRENT',  val: end>0 ? f((drawProgress/TOTAL_PTS)*thetaTotal).toFixed(4) : '—' },
    { lbl:'F(θ) MIN',      val: fMin.toFixed(4) },
    { lbl:'F(θ) MAX',      val: fMax.toFixed(4) },
    { lbl:'F(θ) MEAN',     val: (fSum/TOTAL_PTS).toFixed(4) },
    { lbl:'PTS DRAWN',     val: Math.floor(drawProgress).toLocaleString() },
    { lbl:'θ TOTAL (×Π)',  val: Math.round(thetaTotal/Math.PI).toString() },
  ];
  const STATS_H = 220;
  const cellW   = RIGHT_W / 4;
  const cellH   = STATS_H / 2;

  ctx.fillStyle='rgba(255,215,0,0.04)';
  ctx.strokeStyle='rgba(255,215,0,0.18)'; ctx.lineWidth=2;
  ctx.beginPath(); ctx.roundRect(RIGHT_X, ry, RIGHT_W, STATS_H, 12); ctx.fill(); ctx.stroke();

  STATS.forEach((s,i)=>{
    const col=i%4, row=Math.floor(i/4);
    const cx=RIGHT_X+col*cellW, cy=ry+row*cellH;
    ctx.textAlign='left'; ctx.textBaseline='top';
    ctx.fillStyle='rgba(255,215,0,0.45)';
    ctx.font=`500 22px 'JetBrains Mono', monospace`;
    ctx.fillText(s.lbl, cx+18, cy+18);
    ctx.fillStyle='#ffd700';
    ctx.font=`bold 40px 'JetBrains Mono', monospace`;
    ctx.fillText(s.val, cx+18, cy+52);
  });
  ry += STATS_H + 48;

  // ── Parameter Legend (Bottom-Right) ──
  const LEGEND_X = RIGHT_X;
  const LEGEND_Y = H - 580;  // Position from bottom
  const LEGEND_W = RIGHT_W;
  const LEGEND_H = 500;
  
  // Background panel
  ctx.fillStyle='rgba(91,156,246,0.04)';
  ctx.strokeStyle='rgba(91,156,246,0.18)'; ctx.lineWidth=2;
  ctx.beginPath(); ctx.roundRect(LEGEND_X, LEGEND_Y, LEGEND_W, LEGEND_H, 12); ctx.fill(); ctx.stroke();
  
  // Title
  ctx.textAlign='left'; ctx.textBaseline='top';
  ctx.fillStyle='rgba(91,156,246,0.7)';
  ctx.font=`bold 32px 'JetBrains Mono', monospace`;
  ctx.fillText('PARAMETERS', LEGEND_X + 24, LEGEND_Y + 20);
  
  let py = LEGEND_Y + 70;
  const lineH = 36;
  const indent = LEGEND_X + 24;
  
  // Parameter details
  const params = [
    { label: 'Formula Parameters:', value: '', color: 'rgba(255,215,0,0.6)' },
    { label: '  A (Offset):', value: A.toString(), color: '#ffd700' },
    { label: '  B (Amplitude):', value: B.toString(), color: '#ffd700' },
    { label: '  C (Frequency):', value: C.toString(), color: '#ffd700' },
    { label: '  D (Phase):', value: D.toString(), color: '#ffd700' },
    { label: '', value: '', color: '' },
    { label: 'Visualization:', value: '', color: 'rgba(91,156,246,0.6)' },
    { label: '  Color Mode:', value: harmColorMode ? 'Harmonic Consonance' : 'Hue Gradient', color: harmColorMode ? '#5ebd78' : '#5b9cf6' },
    { label: '  Rotation X:', value: `${Math.round(manualRotX)}°`, color: '#5b9cf6' },
    { label: '  Rotation Y:', value: `${Math.round(manualRotY)}°`, color: '#5b9cf6' },
    { label: '  Rotation Z:', value: `${Math.round(manualRotZ)}°`, color: '#5b9cf6' },
    { label: '  Zoom Level:', value: `${zoomLevel.toFixed(2)}×`, color: '#5b9cf6' },
    { label: '  Trace Thickness:', value: traceThickness.toString(), color: '#5b9cf6' },
    { label: '  Speed:', value: speed.toString(), color: '#5b9cf6' },
  ];
  
  ctx.font=`500 24px 'JetBrains Mono', monospace`;
  params.forEach(p => {
    if (p.label === '' && p.value === '') {
      py += lineH * 0.3; // Small gap
    } else if (p.value === '') {
      // Section header
      ctx.fillStyle = p.color;
      ctx.font=`bold 26px 'JetBrains Mono', monospace`;
      ctx.fillText(p.label, indent, py);
      ctx.font=`500 24px 'JetBrains Mono', monospace`;
      py += lineH;
    } else {
      // Parameter line
      ctx.fillStyle = 'rgba(180,200,235,0.5)';
      ctx.fillText(p.label, indent, py);
      ctx.fillStyle = p.color;
      const labelW = ctx.measureText(p.label).width;
      ctx.fillText(p.value, indent + labelW + 12, py);
      py += lineH;
    }
  });

  // Add harmonic intervals if color mode is on
  if (harmColorMode && harmSelectedIntervals.size > 0) {
    py += lineH * 0.3; // Small gap
    ctx.fillStyle = 'rgba(94,189,120,0.6)';
    ctx.font=`bold 26px 'JetBrains Mono', monospace`;
    ctx.fillText('Harmonic Intervals:', indent, py);
    ctx.font=`500 22px 'JetBrains Mono', monospace`;
    py += lineH;
    
    const sel = [...harmSelectedIntervals].sort((a,b) => a-b);
    const intervalStr = sel.map(i => HARM_INTERVALS[i].name).join(', ');
    ctx.fillStyle = '#5ebd78';
    ctx.fillText(`  ${intervalStr}`, indent, py);
    py += lineH;
  }

  // ── Gap Decomp overlay on sphere export ──
  if(gdData && selectedGaps.size > 0){
    // Add selected gap labels along the bottom of the sphere area
    const selList = [...selectedGaps.entries()];
    let gx = PADDING;
    const gy = SPHERE_TOP + SPHERE_H + 28;
    ctx.font = `bold 26px 'JetBrains Mono', monospace`;
    ctx.fillStyle = 'rgba(255,215,0,.55)';
    ctx.textAlign = 'left'; ctx.textBaseline = 'top';
    ctx.fillText('Gap selection: ', gx, gy);
    gx += ctx.measureText('Gap selection: ').width;
    selList.forEach(([g, col]) => {
      ctx.fillStyle = col;
      const gt = `g=${g}  `;
      ctx.fillText(gt, gx, gy);
      gx += ctx.measureText(gt).width;
    });
  }

  // If gap data active, add a gap summary box above the parameters panel
  if(gdData){
    const selList = [...selectedGaps.entries()];
    const GAP_BOX_Y = LEGEND_Y - 180;
    ctx.fillStyle = 'rgba(232,197,71,.06)';
    ctx.strokeStyle = 'rgba(232,197,71,.2)'; ctx.lineWidth = 1;
    ctx.beginPath(); ctx.roundRect(LEGEND_X, GAP_BOX_Y, LEGEND_W, 160, 8);
    ctx.fill(); ctx.stroke();
    ctx.fillStyle = '#ffd700'; ctx.font = `bold 26px 'JetBrains Mono', monospace`;
    ctx.textAlign = 'left'; ctx.textBaseline = 'top';
    ctx.fillText(`Gap Decomp  ·  ζ(${gdData.s.toFixed(2)})  ·  N=${gdData.maxN.toLocaleString()}`, LEGEND_X+20, GAP_BOX_Y+16);
    ctx.fillStyle = '#88cc88'; ctx.font = `22px 'JetBrains Mono', monospace`;
    ctx.fillText(`${gdData.primes.length.toLocaleString()} primes  ·  ${gdData.gaps.length} families  ·  err=${(gdData.finalError*100).toFixed(8)}%`, LEGEND_X+20, GAP_BOX_Y+52);
    // Selected gaps with colors
    let sx = LEGEND_X+20;
    ctx.font = `bold 24px 'JetBrains Mono', monospace`;
    ctx.fillStyle = 'rgba(255,255,255,.4)';
    ctx.fillText('Selected: ', sx, GAP_BOX_Y+88);
    sx += ctx.measureText('Selected: ').width;
    selList.forEach(([g, col]) => {
      ctx.fillStyle = col;
      const gt = `g=${g}  `;
      ctx.fillText(gt, sx, GAP_BOX_Y+88);
      sx += ctx.measureText(gt).width;
    });
    if(selList.length === 0){
      ctx.fillStyle = 'rgba(255,255,255,.25)';
      ctx.fillText('none', sx, GAP_BOX_Y+88);
    }
    // g2/g4 ratio
    if(gdData.c4 > 0){
      ctx.fillStyle = 'rgba(150,200,150,.6)'; ctx.font = `22px 'JetBrains Mono', monospace`;
      ctx.fillText(`g2/g4=${( gdData.c2/gdData.c4).toFixed(6)}  g2/g6=${gdData.c6>0?(gdData.c2/gdData.c6).toFixed(6):'N/A'}  [HL→1.0, 0.5]`, LEGEND_X+20, GAP_BOX_Y+124);
    }
  }

  // ── Watermark ──
  ctx.textAlign='left'; ctx.textBaseline='bottom';
  ctx.fillStyle='rgba(91,156,246,0.35)';
  ctx.font=`500 28px 'JetBrains Mono', monospace`;
  ctx.fillText('Polar vs Cartesian · Wessen Getachew · @7dview', PADDING, H - PADDING);

  // ── θ range label top-right ──
  ctx.textAlign='right'; ctx.textBaseline='top';
  ctx.fillStyle='rgba(255,215,0,0.4)';
  ctx.font=`500 26px 'JetBrains Mono', monospace`;
  ctx.fillText(`θ ∈ [0, ${Math.round(thetaTotal/Math.PI)}π]`, W - PADDING, PADDING);

  // ── Download ──
  const link = document.createElement('a');
  const selSfx = selectedGaps.size > 0 ? '_g'+[...selectedGaps.keys()].join('_g') : '';
  link.download = `polar_cart_4K_A${A}_B${B}_C${C}_D${D}${selSfx}.png`;
  link.href = cv.toDataURL('image/png');
  link.click();
}

// ═══════════════════════════════════════════════════════════════
//  MULTI-ANGLE EXPORT
// ═══════════════════════════════════════════════════════════════
function exportMultiAngle() {
  // Prompt user for number of angles
  const countStr = prompt('How many angle views do you want? (2-12)', '4');
  const count = parseInt(countStr);
  
  if (!count || count < 2 || count > 12) {
    alert('Please enter a number between 2 and 12');
    return;
  }
  
  // Ask if user wants individual files or combined
  const exportMode = confirm('Click OK for COMBINED image\nClick Cancel for INDIVIDUAL images');
  
  // Generate evenly distributed rotation angles
  // Always start with X=0, Y=0, Z=0, then rotate Y axis
  const angles = [];
  for (let i = 0; i < count; i++) {
    const yAngle = Math.round((360 / count) * i);
    angles.push({ x: 0, y: yAngle, z: 0 });
  }
  
  // Size for each canvas (high quality)
  const SIZE = 2048; // 2K per image
  const cvTemp = document.createElement('canvas');
  cvTemp.width = cvTemp.height = SIZE;
  const ctx = cvTemp.getContext('2d');
  
  // Save current rotation
  const savedRotX = manualRotX;
  const savedRotY = manualRotY;
  const savedRotZ = manualRotZ;
  const savedRotMat = [...rotMat];
  
  // Generate each angle
  const images = [];
  angles.forEach((angle, idx) => {
    // Set rotation
    manualRotX = angle.x;
    manualRotY = angle.y;
    manualRotZ = angle.z;
    const rx = manualRotX * Math.PI / 180;
    const ry = manualRotY * Math.PI / 180;
    const rz = manualRotZ * Math.PI / 180;
    rotMat = mmul(mrotX(rx), mmul(mrotY(ry), mrotZ(rz)));
    
    // Clear canvas
    ctx.fillStyle = '#060608';
    ctx.fillRect(0, 0, SIZE, SIZE);
    
    // Draw grid
    const R = SIZE * 0.37;
    function proj2K(rx, ry, rz) {
      const fov = 2.8, scale = fov / (fov - rz);
      return [SIZE / 2 + rx * R * scale * zoomLevel, SIZE / 2 - ry * R * scale * zoomLevel];
    }
    
    ctx.lineWidth = 1.5;
    // Latitude lines
    for (let li = -2; li <= 2; li++) {
      const lat = (li / 3) * Math.PI / 2, cosLat = Math.cos(lat), sinLat = Math.sin(lat);
      ctx.beginPath();
      let first = true;
      for (let lo = 0; lo <= Math.PI * 2 + 0.05; lo += 0.04) {
        const x = cosLat * Math.cos(lo), y = sinLat, z = cosLat * Math.sin(lo);
        const [rx, ry, rz] = applyM(rotMat, x, y, z);
        const [px, py] = proj2K(rx, ry, rz);
        ctx.globalAlpha = Math.max(0.05, 0.06 + (rz + 1) * 0.12);
        ctx.strokeStyle = '#3a7040';
        if (first) { ctx.moveTo(px, py); first = false; } else ctx.lineTo(px, py);
      }
      ctx.stroke();
    }
    // Longitude lines
    for (let lo = 0; lo < 8; lo++) {
      const lon = (lo / 8) * Math.PI * 2, cosLon = Math.cos(lon), sinLon = Math.sin(lon);
      ctx.beginPath();
      let first = true;
      for (let la = -Math.PI / 2; la <= Math.PI / 2 + 0.05; la += 0.04) {
        const x = Math.cos(la) * cosLon, y = Math.sin(la), z = Math.cos(la) * sinLon;
        const [rx, ry, rz] = applyM(rotMat, x, y, z);
        const [px, py] = proj2K(rx, ry, rz);
        ctx.globalAlpha = Math.max(0.05, 0.06 + (rz + 1) * 0.12);
        ctx.strokeStyle = '#3a7040';
        if (first) { ctx.moveTo(px, py); first = false; } else ctx.lineTo(px, py);
      }
      ctx.stroke();
    }
    ctx.globalAlpha = 1;
    
    // Draw curve
    const end = Math.floor(drawProgress);
    if (end >= 2) {
      ctx.lineWidth = traceThickness * 2.5;
      for (let i = 1; i < end; i++) {
        const p1 = precomputed[i], p0 = precomputed[i - 1];
        if (!p0 || !p1) continue;
        const [rx1, ry1, rz1] = applyM(rotMat, p0[0], p0[1], p0[2]);
        const [rx2, ry2, rz2] = applyM(rotMat, p1[0], p1[1], p1[2]);
        const [px1, py1] = proj2K(rx1, ry1, rz1);
        const [px2, py2] = proj2K(rx2, ry2, rz2);
        const frac = i / TOTAL_PTS;
        ctx.strokeStyle = harmColorMode ? (harmColorForProgress(frac) || `hsl(${200 + frac * 80},90%,60%)`) : `hsl(${200 + frac * 80},90%,60%)`;
        ctx.globalAlpha = 0.9;
        ctx.beginPath();
        ctx.moveTo(px1, py1);
        ctx.lineTo(px2, py2);
        ctx.stroke();
      }
    }
    
    // Add title
    ctx.globalAlpha = 1;
    ctx.textAlign = 'center';
    ctx.textBaseline = 'top';
    ctx.fillStyle = '#ffd700';
    ctx.font = `bold 48px 'JetBrains Mono', monospace`;
    ctx.fillText(`f(θ) = ${A} + sin(${B}θ) · sin(${C}θ / ${D})`, SIZE / 2, 40);
    
    // Add rotation angles at bottom
    ctx.textBaseline = 'bottom';
    ctx.fillStyle = '#5b9cf6';
    ctx.font = `500 36px 'JetBrains Mono', monospace`;
    ctx.fillText(`X: ${angle.x}°  Y: ${angle.y}°  Z: ${angle.z}°`, SIZE / 2, SIZE - 40);
    
    // Store image
    images.push({
      dataUrl: cvTemp.toDataURL('image/png'),
      canvas: cvTemp.toDataURL('image/png'),
      angle: angle
    });
  });
  
  // Restore rotation
  manualRotX = savedRotX;
  manualRotY = savedRotY;
  manualRotZ = savedRotZ;
  rotMat = savedRotMat;
  
  if (exportMode) {
    // COMBINED MODE — balanced grid: find most-square layout
    // For n items, find cols×rows where cols*rows >= n and |cols-rows| is minimized
    function bestGrid(n) {
      let bestCols = n, bestRows = 1, bestDiff = n - 1;
      for (let c = 1; c <= n; c++) {
        const r = Math.ceil(n / c);
        if (c * r >= n) {
          const diff = Math.abs(c - r);
          if (diff < bestDiff || (diff === bestDiff && c <= r)) {
            bestDiff = diff;
            bestCols = c;
            bestRows = r;
          }
        }
      }
      return { cols: bestCols, rows: bestRows };
    }
    const { cols, rows } = bestGrid(count);

    const CELL_PAD = 32;    // padding between cells
    const MARGIN = 60;      // outer margin
    const LABEL_H = 100;    // title area at top
    const FOOT_H = 60;      // footer
    const AXIS_L = 48;      // left axis label area per cell
    const AXIS_B = 32;      // bottom axis label area per cell

    // Each cell is square: SIZE x SIZE, plus axis areas
    const CELL_W = SIZE + AXIS_L;
    const CELL_H = SIZE + AXIS_B;

    const gridW = MARGIN * 2 + cols * CELL_W + (cols - 1) * CELL_PAD;
    const gridH = MARGIN + LABEL_H + rows * CELL_H + (rows - 1) * CELL_PAD + FOOT_H;

    const gridCanvas = document.createElement('canvas');
    gridCanvas.width = gridW;
    gridCanvas.height = gridH;
    const gCtx = gridCanvas.getContext('2d');

    // Background
    gCtx.fillStyle = '#060608';
    gCtx.fillRect(0, 0, gridW, gridH);

    // Subtle grid paper
    gCtx.strokeStyle = 'rgba(91,156,246,0.04)';
    gCtx.lineWidth = 1;
    for (let x = 0; x < gridW; x += 32) { gCtx.beginPath(); gCtx.moveTo(x,0); gCtx.lineTo(x,gridH); gCtx.stroke(); }
    for (let y = 0; y < gridH; y += 32) { gCtx.beginPath(); gCtx.moveTo(0,y); gCtx.lineTo(gridW,y); gCtx.stroke(); }

    // Title
    gCtx.textAlign = 'center'; gCtx.textBaseline = 'middle';
    gCtx.fillStyle = '#e8c547';
    gCtx.font = `bold 52px 'JetBrains Mono', monospace`;
    gCtx.fillText(`f(θ) = ${A} + sin(${B}θ) · sin(${C}θ / ${D})`, gridW / 2, MARGIN + LABEL_H * 0.38);
    // Grid descriptor
    gCtx.fillStyle = 'rgba(91,156,246,0.6)';
    gCtx.font = `400 28px 'JetBrains Mono', monospace`;
    gCtx.fillText(`${cols}×${rows} view array · ${count} rotation angles`, gridW / 2, MARGIN + LABEL_H * 0.72);

    // Footer
    gCtx.textAlign = 'right'; gCtx.textBaseline = 'bottom';
    gCtx.fillStyle = 'rgba(232,197,71,0.3)';
    gCtx.font = `300 22px 'JetBrains Mono', monospace`;
    gCtx.fillText(`θ ∈ [0, ${Math.round(thetaTotal/Math.PI)}π]   A=${A}  B=${B}  C=${C}  D=${D}`, gridW - MARGIN, gridH - 20);

    // Draw each image in grid
    let loadedCount = 0;
    const imageElements = [];

    images.forEach((img, idx) => {
      const imgEl = new Image();
      imageElements.push(imgEl);

      imgEl.onload = () => {
        loadedCount++;

        const col = idx % cols;
        const row = Math.floor(idx / cols);
        const cellX = MARGIN + col * (CELL_W + CELL_PAD) + AXIS_L;
        const cellY = MARGIN + LABEL_H + row * (CELL_H + CELL_PAD);

        // Cell background border
        gCtx.strokeStyle = 'rgba(91,156,246,0.15)';
        gCtx.lineWidth = 1;
        gCtx.strokeRect(cellX - 1, cellY - 1, SIZE + 2, SIZE + 2);

        // Draw sphere image
        gCtx.drawImage(imgEl, cellX, cellY, SIZE, SIZE);

        // ── Y-axis label (left of cell) ──
        gCtx.save();
        gCtx.translate(cellX - AXIS_L + 12, cellY + SIZE / 2);
        gCtx.rotate(-Math.PI / 2);
        gCtx.textAlign = 'center'; gCtx.textBaseline = 'middle';
        gCtx.fillStyle = 'rgba(94,189,120,0.7)';
        gCtx.font = `400 20px 'JetBrains Mono', monospace`;
        gCtx.fillText('Y (θ)', 0, 0);
        gCtx.restore();

        // ── X-axis label (below cell) ──
        gCtx.textAlign = 'center'; gCtx.textBaseline = 'top';
        gCtx.fillStyle = 'rgba(94,189,120,0.7)';
        gCtx.font = `400 20px 'JetBrains Mono', monospace`;
        gCtx.fillText('X (φ)', cellX + SIZE / 2, cellY + SIZE + 6);

        // ── View label inside image (top-center) ──
        gCtx.textAlign = 'center'; gCtx.textBaseline = 'top';
        gCtx.fillStyle = 'rgba(232,197,71,0.85)';
        gCtx.font = `bold 28px 'JetBrains Mono', monospace`;
        gCtx.fillText(`X:${images[idx].angle.x}° Y:${images[idx].angle.y}° Z:${images[idx].angle.z}°`, cellX + SIZE / 2, cellY + 12);

        // ── View number badge ──
        gCtx.textAlign = 'left'; gCtx.textBaseline = 'top';
        gCtx.fillStyle = 'rgba(91,156,246,0.5)';
        gCtx.font = `500 22px 'JetBrains Mono', monospace`;
        gCtx.fillText(`#${idx + 1}`, cellX + 10, cellY + 10);

        if (loadedCount === count) {
          const link = document.createElement('a');
          link.download = `polar_${cols}x${rows}grid_${count}views_A${A}_B${B}_C${C}_D${D}.png`;
          link.href = gridCanvas.toDataURL('image/png');
          link.click();
        }
      };

      imgEl.src = img.canvas;
    });
    
  } else {
    // INDIVIDUAL MODE - Download separately
    images.forEach((img, idx) => {
      const link = document.createElement('a');
      link.download = `polar_A${A}_B${B}_C${C}_D${D}_angle${idx + 1}_X${img.angle.x}_Y${img.angle.y}_Z${img.angle.z}.png`;
      link.href = img.dataUrl;
      link.click();
    });
    alert(`Exported ${count} individual angle views!`);
  }
}

// ═══════════════════════════════════════════════════════════════
//  BUILD PRESETS
// ═══════════════════════════════════════════════════════════════
let activePresetBtn = null;

// Preset dropdown toggle
document.getElementById('presetDropHeader').addEventListener('click',()=>{
  document.getElementById('presetDropBody').classList.toggle('open');
  document.getElementById('presetDropChev').classList.toggle('open');
  document.getElementById('presetDropHeader').classList.toggle('open');
});

function buildPresets(){
  Object.entries(PRESET_GROUPS).forEach(([gid, list])=>{
    const row = document.getElementById(gid);
    if(!row) return;
    list.forEach((p,i)=>{
      const btn = document.createElement('button');
      btn.className = 'preset-btn';
      btn.textContent = p.name;
      if(gid==='pgClassic'&&i===0){ btn.classList.add('active'); activePresetBtn=btn; }
      btn.onclick = ()=> applyPreset(p, btn, gid==='pgGapWave');
      row.appendChild(btn);
    });
  });
}

function applyPreset(p, btn, isGap){
  onUserInteraction(); // first preset click switches to loop mode
  A=p.A; B=p.B; C=p.C; D=p.D; thetaTotal=p.theta*Math.PI; trailFade=p.trail||6;
  const sl=(id,v,fmt)=>{ document.getElementById(id).value=v; document.getElementById('d'+id.slice(1)).textContent=fmt?fmt(v):v; };
  sl('sA',A); sl('sB',B); sl('sC',C); sl('sD',D);
  sl('sTheta',p.theta,v=>v+'π');
  // Sync number inputs too
  ['A','B','C','D'].forEach(k=>{ const ni=document.getElementById('n'+k); if(ni) ni.value=p[k.toLowerCase()]; });
  const ni=document.getElementById('nTheta'); if(ni) ni.value=p.theta;
  if(activePresetBtn) activePresetBtn.classList.remove('active','gap-active');
  if(btn){ btn.classList.add(isGap?'gap-active':'active'); activePresetBtn=btn; }

  // Update active name in dropdown header
  const nameEl=document.getElementById('presetActiveName');
  if(nameEl) nameEl.textContent=p.name;

  // Loop OFF (as requested)
  looping=false;
  const cbLoop=document.getElementById('cbLoop');
  if(cbLoop){ cbLoop.checked=false; }
  document.getElementById('loopState').textContent='off — stops at end';

  // Fast speed
  setSpeed(20);

  // Auto-rotate: preserve user's current toggle state; seqRotate fires when wave ends
  seqRotateOnComplete = true;

  // Reset then play
  hardReset();
  paused=false;
  const playBtn=document.getElementById('btnPlay');
  if(playBtn){ playBtn.textContent='⏸ Pause'; playBtn.classList.add('on'); }
  startBlendFlash();
  // Sync harmonics with wave
  harmSyncStart();
}

// ═══════════════════════════════════════════════════════════════
//  GAP DECOMPOSITION MODULE
// ═══════════════════════════════════════════════════════════════

// ═══════════════════════════════════════════════════════════════
//  GAP DECOMPOSITION — Inputs, Worker, Sieve
// ═══════════════════════════════════════════════════════════════

// ── Slider ↔ number input two-way sync for N and s ──
(function(){
  function syncPair(sliderId, numId, displayId, fmt){
    const sl  = document.getElementById(sliderId);
    const num = document.getElementById(numId);
    const dsp = document.getElementById(displayId);
    if(!sl || !num) return;

    function update(v){
      v = Math.max(parseFloat(sl.min), Math.min(parseFloat(sl.max), v));
      sl.value  = v;
      num.value = v;
      if(dsp) dsp.textContent = fmt ? fmt(v) : v.toLocaleString();
    }

    sl.addEventListener('input',  ()=> update(parseFloat(sl.value)));
    num.addEventListener('change', ()=> update(parseFloat(num.value)));
    num.addEventListener('blur',   ()=> update(parseFloat(num.value)));
  }

  syncPair('gdN', 'gdNNum', 'gdND',
    v => parseInt(v).toLocaleString());
  syncPair('gdS', 'gdSNum', 'gdSD',
    v => 's = ' + parseFloat(v).toFixed(2));
})();

// Quick N setter (called by preset buttons)
function setGdN(n){
  const sl  = document.getElementById('gdN');
  const num = document.getElementById('gdNNum');
  const dsp = document.getElementById('gdND');
  if(sl)  sl.value  = n;
  if(num) num.value = n;
  if(dsp) dsp.textContent = n.toLocaleString();
}

let gdData   = null;
let gdTab    = 'contrib';
let gdFilter = 'all';
const gdCanvas = document.getElementById('gapCanvas');
let gdCtx = gdCanvas.getContext('2d');
let gdActiveWaveBtn = null;

// ── Web Worker source (inline blob) ──────────────────────────
// The worker receives { maxN, s } and posts back progress and result.
// It uses a BITPACKED segmented sieve: 1 bit per odd number → 8×
// less memory than Uint8Array; only ~6 MB for sieve of 400M.
// Gap families store COUNTS only (not prime values) → ~1 KB total.

const WORKER_SRC = `
'use strict';

// ── Helpers ───────────────────────────────────────────────────
function knownZeta(s){
  const t={2:Math.PI*Math.PI/6, 4:Math.PI**4/90, 6:Math.PI**6/945,
           8:Math.PI**8/9450, 10:Math.PI**10/93555};
  const key=Math.round(s*100)/100;
  return t[key]||null;
}

// ── Bitpacked segmented sieve of Eratosthenes ─────────────────
// Returns gap counts {gapSize: count} and total prime count.
// Does NOT store prime values to keep memory tiny.
function bitpackedSieve(max, s){
  const sqrtMax = Math.ceil(Math.sqrt(max));

  // ── Phase 1: small sieve up to sqrt(max) ──
  // Regular Uint8Array is fine here — sqrt(400M) ≈ 20000 numbers
  const smallArr = new Uint8Array(sqrtMax+1).fill(1);
  smallArr[0]=smallArr[1]=0;
  for(let i=2;i*i<=sqrtMax;i++) if(smallArr[i]) for(let j=i*i;j<=sqrtMax;j+=i) smallArr[j]=0;
  const smallPrimes=[];
  for(let i=2;i<=sqrtMax;i++) if(smallArr[i]) smallPrimes.push(i);

  // ── Phase 2: segmented sieve, collect stats only ──
  const SEG  = 1 << 19;  // 512K numbers per segment (512KB bitfield = 64KB)
  // Bitfield: bit k represents odd number (segLo + 2k+1), index 0 = 2 (special)
  // We handle 2 separately.

  let gapCounts = {};       // gap -> count
  let totalPrimes = 0;
  let logSum = {};          // gap -> sum of log(p^s/(p^s-1)) for ζ product
  let logTotal = 0;

  let prevPrime = 2;        // last prime seen across segments
  totalPrimes = 1;          // count 2

  // Prime 2 doesn't form a gap yet — next prime will close gap from 2

  for(let lo = 3; lo <= max; lo += SEG){
    const hi  = Math.min(lo + SEG - 1, max);
    const len = (hi - lo) >> 1;  // number of odd numbers in [lo,hi]

    // Bitfield: 1 = prime candidate, 0 = composite
    // We use Uint32Array for speed
    const words = (len >> 5) + 1;
    const sieveArr = new Uint32Array(words).fill(0xFFFFFFFF);

    // Mark composites
    for(let pi=0; pi<smallPrimes.length; pi++){
      const p = smallPrimes[pi];
      if(p === 2) continue;
      // First odd multiple of p >= lo
      let startVal = Math.ceil(lo / p) * p;
      if((startVal & 1) === 0) startVal += p;  // must be odd
      if(startVal < p*p) startVal = p*p;
      if((startVal & 1) === 0) startVal += p;
      for(let v = startVal; v <= hi; v += 2*p){
        const idx = (v - lo) >> 1;
        sieveArr[idx >> 5] &= ~(1 << (idx & 31));
      }
    }

    // Sweep the bitfield and record gaps
    for(let k = 0; k <= len; k++){
      if(sieveArr[k >> 5] & (1 << (k & 31))){
        const p = lo + (k << 1);
        if(p > max) break;
        totalPrimes++;
        const gap = p - prevPrime;
        gapCounts[gap] = (gapCounts[gap]||0) + 1;
        // Accumulate log factor for ζ product
        const ps = Math.pow(p, s);
        const lf = Math.log(ps / (ps - 1));
        logSum[gap] = (logSum[gap]||0) + lf;
        logTotal += lf;
        prevPrime = p;
      }
    }

    // Progress report every segment
    const pct = ((hi / max) * 100) | 0;
    self.postMessage({ type:'progress', pct, cur:hi, max,
                       primes:totalPrimes, gaps: Object.keys(gapCounts).length });
  }

  // Factor in prime 2 into ζ product
  const p2s = Math.pow(2, s);
  const lf2 = Math.log(p2s / (p2s - 1));

  return { gapCounts, logSum, logTotal: logTotal + lf2, totalPrimes };
}

// ── Analysis pass: compute ζ product and convergence ─────────
function analyse(gapCounts, logSum, logTotal, s, filter, totalPrimes){
  let target = knownZeta(s);
  // If not a known value, estimate from the log product we accumulated
  const expProduct = Math.exp(logTotal);
  if(!target) target = expProduct;

  let gaps = Object.keys(gapCounts).map(Number).sort((a,b)=>a-b);

  if(filter==='tiny')   gaps=gaps.filter(g=>g>=2&&g<=10);
  if(filter==='small')  gaps=gaps.filter(g=>g>=2&&g<=30);
  if(filter==='medium') gaps=gaps.filter(g=>g>=20&&g<=80);
  if(filter==='large')  gaps=gaps.filter(g=>g>=50&&g<=200);

  // For each gap, compute individual product from stored log sums
  const gapProducts = {};
  gaps.forEach(g=>{
    const cnt  = gapCounts[g]||0;
    const lp   = logSum[g]||0;
    const prod = Math.exp(lp);
    gapProducts[g] = { product:prod, logProduct:lp, count:cnt };
  });

  // Cumulative ζ product over gaps (convergence steps)
  const conv = [];
  let cum = 1;
  // Start with the contribution of prime 2
  const p2s = Math.pow(2, s);
  cum = p2s / (p2s - 1);

  gaps.forEach((g, idx)=>{
    const gp = gapProducts[g];
    if(!gp) return;
    const prevCum = cum;
    cum *= gp.product;
    const relErr = Math.abs(cum - target) / target;
    const pi_est = (s>=1.99&&s<=2.01) ? Math.sqrt(6*cum) : null;
    conv.push({ gap:g, value:cum, relErr,
                pi_est, delta:cum-prevCum,
                logProduct:gp.logProduct, step:idx+1 });
  });

  const c2 = gapCounts[2]||0;
  const c4 = gapCounts[4]||0;
  const c6 = gapCounts[6]||0;
  let mostCommon=gaps[0]||2, mcCount=0;
  gaps.forEach(g=>{ if((gapCounts[g]||0)>mcCount){ mcCount=gapCounts[g]; mostCommon=g; }});

  const finalProduct = cum;
  const finalError   = Math.abs(cum - target) / target;
  const pi_calc      = (s>=1.99&&s<=2.01) ? Math.sqrt(6*cum) : null;
  const totalLog     = Object.values(logSum).reduce((a,b)=>a+b, 0);

  return { gaps, gapCounts, gapProducts, conv, target, totalLog,
           finalProduct, finalError, s,
           c2, c4, c6, mostCommon, mcCount, pi_calc, totalPrimes };
}

// ── Worker message handler ────────────────────────────────────
self.onmessage = function(e){
  const { maxN, s, filter } = e.data;
  try {
    self.postMessage({ type:'status', msg:'Sieving primes…' });
    const { gapCounts, logSum, logTotal, totalPrimes } =
          bitpackedSieve(maxN, s);

    self.postMessage({ type:'status', msg:'Analysing gaps…' });
    const result = analyse(gapCounts, logSum, logTotal, s, filter, totalPrimes);
    result.maxN = maxN;

    self.postMessage({ type:'done', result });
  } catch(err){
    self.postMessage({ type:'error', msg: err.message });
  }
};
`;

// ── Worker lifecycle ──────────────────────────────────────────
let _gdWorker = null;

function gdAbort(){
  if(_gdWorker){ _gdWorker.terminate(); _gdWorker=null; }
  document.getElementById('gdRunBtn').disabled = false;
  document.getElementById('gdAbortBtn').style.display = 'none';
  document.getElementById('gdProgress').style.display = 'none';
  document.getElementById('gdStatus').textContent = 'Computation stopped.';
}

function gdRun(){
  const maxN = parseInt(document.getElementById('gdN').value);
  const s    = parseFloat(document.getElementById('gdS').value);
  const filter = gdFilter;

  if(maxN > 400000000){
    document.getElementById('gdStatus').textContent = '⚠ Max N is 400,000,000.';
    return;
  }
  if(maxN < 100){
    document.getElementById('gdStatus').textContent = '⚠ N must be at least 100.';
    return;
  }

  // Abort any running worker
  if(_gdWorker){ _gdWorker.terminate(); _gdWorker=null; }

  const statusEl  = document.getElementById('gdStatus');
  const runBtn    = document.getElementById('gdRunBtn');
  const abortBtn  = document.getElementById('gdAbortBtn');
  const progWrap  = document.getElementById('gdProgress');
  const progFill  = document.getElementById('gdProgressFill');

  runBtn.disabled = true;
  abortBtn.style.display = 'inline-block';
  progWrap.style.display = 'block';
  progFill.style.width = '0%';
  statusEl.textContent = 'Starting worker…';

  // Build worker from blob URL
  const blob   = new Blob([WORKER_SRC], { type:'application/javascript' });
  const url    = URL.createObjectURL(blob);
  _gdWorker    = new Worker(url);
  URL.revokeObjectURL(url);  // safe to revoke immediately

  _gdWorker.onmessage = function(e){
    const d = e.data;
    if(d.type === 'progress'){
      const pct = Math.min(99, d.pct);
      progFill.style.width = pct+'%';
      statusEl.textContent =
        `Sieving ${pct}%  — ${d.cur.toLocaleString()} / ${d.max.toLocaleString()}`+
        `  | primes: ${d.primes.toLocaleString()}`+
        `  | gap families: ${d.gaps}`;
    } else if(d.type === 'status'){
      statusEl.textContent = d.msg;
    } else if(d.type === 'done'){
      _gdWorker.terminate(); _gdWorker=null;
      runBtn.disabled = false;
      abortBtn.style.display = 'none';
      progFill.style.width = '100%';
      setTimeout(()=>{ progWrap.style.display='none'; }, 500);

      // Unpack result — worker sends counts only, no prime arrays
      const r = d.result;

      // Rebuild gapFamilies shim: downstream code reads .length from gapFamilies[g]
      // We store a lightweight proxy: { length: count } per gap
      const gapFamilies = {};
      r.gaps.forEach(g=>{
        gapFamilies[g] = { length: r.gapCounts[g]||0 };
      });

      gdData = {
        primes:        { length: r.totalPrimes },  // shim — no prime array stored
        totalPrimes:   r.totalPrimes,              // direct field for convenience
        gaps:          r.gaps,
        gapFamilies,
        gapProducts:   r.gapProducts,
        conv:          r.conv,
        target:        r.target,
        totalLog:      r.totalLog,
        finalProduct:  r.finalProduct,
        finalError:    r.finalError,
        s:             r.s,
        maxN:          r.maxN,
        c2:            r.c2,
        c4:            r.c4,
        c6:            r.c6,
        mostCommon:    r.mostCommon,
        mcCount:       r.mcCount,
        pi_calc:       r.pi_calc,
        gapCounts:     r.gapCounts,
        _noRawPrimes:  true   // flag: raw prime array not available
      };

      // Rebuild gapIndexMap
      gapIndexMap = {};
      r.gaps.forEach((g,i)=>{ gapIndexMap[g]=i; });

      const errPct = (r.finalError*100).toFixed(8);
      statusEl.textContent =
        ` N=${r.maxN.toLocaleString()} · ${r.totalPrimes.toLocaleString()} primes · `+
        `${r.gaps.length} gap families · `+
        `ζ(${r.s.toFixed(2)}) target=${r.target.toFixed(8)} · `+
        `computed=${r.finalProduct.toFixed(8)} · err=${errPct}%`;

      updateGdStats();

      ['gdExportPNG','gdExportCSV','gdExportTXT'].forEach(id=>{
        const el=document.getElementById(id); if(el) el.style.display='inline-block';
      });
      const tabs=document.getElementById('gdTabs');
      if(tabs) tabs.style.display='flex';
      const sg=document.getElementById('gdStatsGrid');
      if(sg) sg.style.display='grid';

      const chartArea=document.getElementById('gdChartArea');
      if(chartArea) chartArea.style.display='block';
      const chartBtn=document.getElementById('gdChartScreenshot');
      if(chartBtn) chartBtn.style.display='inline-block';

      const accBody=document.getElementById('gapAccBody');
      const accChev=document.getElementById('gapAccChev');
      const accHdr =document.getElementById('gapAccHeader');
      if(accBody && !accBody.classList.contains('open')){
        accBody.classList.add('open');
        if(accChev) accChev.classList.add('open');
        if(accHdr)  accHdr.classList.add('open');
      }

      buildGapChips();
      gdBuildTable();
      gdBuildConvTable();
      const ts=document.getElementById('gdTableSection');
      if(ts) ts.style.display='block';
      const cs=document.getElementById('gdConvSection');
      if(cs) cs.style.display='block';

      // Sphere recompute (gap channel colours only — no raw primes needed)
      precompute();
      invalidateCartBuf();
      gdResizeCanvas();
      gdDraw();
      gdUpdateRatioPanel();

    } else if(d.type === 'error'){
      _gdWorker.terminate(); _gdWorker=null;
      runBtn.disabled = false;
      abortBtn.style.display = 'none';
      progWrap.style.display = 'none';
      statusEl.textContent = 'Error: '+d.msg;
      console.error('Worker error:', d.msg);
    }
  };

  _gdWorker.onerror = function(err){
    _gdWorker=null;
    runBtn.disabled = false;
    abortBtn.style.display = 'none';
    progWrap.style.display = 'none';
    statusEl.textContent = 'Worker error: '+err.message;
  };

  _gdWorker.postMessage({ maxN, s, filter });
}

function updateGdStats(){
  if(!gdData) return;
  const d = gdData;
  const pi_str = d.pi_calc ? d.pi_calc.toFixed(10) : 'n/a (s≠2)';
  const vals = [
    d.primes.length.toLocaleString(), d.gaps.length,
    Math.max(...d.gaps), d.target.toFixed(10), d.finalProduct.toFixed(10),
    (d.finalError*100).toFixed(10)+'%', d.c2.toLocaleString(),
    d.c4.toLocaleString(), d.c6.toLocaleString(),
    `${d.mostCommon} (${d.mcCount.toLocaleString()}×)`,
    d.c4>0 ? (d.c2/d.c4).toFixed(6) : 'n/a', pi_str,
  ];
  vals.forEach((v,i)=>{ const el=document.getElementById('gst'+i); if(el) el.textContent=v; });
}

function gdClear(){
  gdData = null; selectedGaps.clear(); gapIndexMap = {};
  gdCtx.clearRect(0,0,gdCanvas.width,gdCanvas.height);
  // Clear big accordion chart
  const bigCv = document.getElementById('gdChartBig');
  if(bigCv){ bigCv.getContext('2d').clearRect(0,0,bigCv.width,bigCv.height); }
  const chartArea = document.getElementById('gdChartArea');
  if(chartArea) chartArea.style.display='none';
  const chartBtn = document.getElementById('gdChartScreenshot');
  if(chartBtn) chartBtn.style.display='none';

  document.getElementById('gdStatus').textContent='Run decomposition to analyze prime gap contributions to ζ(s).';
  ['gdExportPNG','gdExportCSV','gdExportTXT'].forEach(id=>document.getElementById(id).style.display='none');
  document.getElementById('gdTabs').style.display='none';
  document.getElementById('gdStatsGrid').style.display='none';
  document.getElementById('gdSelectorWrap').style.display='none';
  document.getElementById('gdTableSection').style.display='none';
  document.getElementById('gdConvSection').style.display='none';
  document.getElementById('gdLegend').innerHTML='';
  document.getElementById('gdChips').innerHTML='';
  precompute(); invalidateCartBuf();
}

function gdSwitchTab(btn, tab){
  gdTab = tab;
  document.querySelectorAll('.gap-tab').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  if(gdData) gdDraw();
}

// ═══════════════════════════════════════════════════════════════
//  GAP CHIP SELECTOR (multi-select)
// ═══════════════════════════════════════════════════════════════
function buildGapChips(){
  if(!gdData) return;

  // Build chips in the panel selector only
  const row = document.getElementById('gdChips');
  const wrap = document.getElementById('gdSelectorWrap');
  row.innerHTML = '';
  gdData.gaps.forEach((g, i) => {
    const col = gapColor(g, i);
    const chip = document.createElement('div');
    chip.className = 'gap-chip';
    chip.dataset.gap = g;
    chip.innerHTML = `<span class="chip-dot" style="background:${col}"></span>g=${g}`;
    chip.title = `Gap ${g}: ${(gdData.gapProducts[g]?.count||0).toLocaleString()} primes`;
    chip.addEventListener('click', ()=> toggleGapChipAll(g, i, col));
    row.appendChild(chip);
  });
  wrap.style.display = 'block';
}

// Toggle all chips with matching gap value across both containers
function toggleGapChipAll(g, idx, col){
  onUserInteraction();
  const isSelected = selectedGaps.has(g);
  if(isSelected){
    selectedGaps.delete(g);
  } else {
    selectedGaps.set(g, col);
  }
  // Update ALL chips for this gap
  document.querySelectorAll(`.gap-chip[data-gap="${g}"]`).forEach(chip => {
    if(selectedGaps.has(g)){
      chip.classList.add('selected');
      chip.style.background = col;
      chip.style.borderColor = col;
      chip.style.color = '#000';
    } else {
      chip.classList.remove('selected');
      chip.style.background = '';
      chip.style.borderColor = '';
      chip.style.color = '';
    }
  });
  invalidateCartBuf();
  // Resize cart canvas height to accommodate new lane count
  setSize(); makeCartBuf(); lastCartProg = 0;
}

function toggleGapChip(g, idx, chip, col){
  toggleGapChipAll(g, idx, col);
}

function gdClearSelection(){
  selectedGaps.clear();
  document.querySelectorAll('.gap-chip.selected').forEach(c=>{
    c.classList.remove('selected');
    c.style.background = '';
    c.style.borderColor = '';
    c.style.color = '';
  });
  setSize(); invalidateCartBuf();
  if(gdData) gdDraw();
}

// ═══════════════════════════════════════════════════════════════
//  ALL-GAPS STATS TABLE
// ═══════════════════════════════════════════════════════════════
function gdBuildTable(){
  if(!gdData) return;
  const {gaps, gapProducts, gapFamilies, totalLog, target, s} = gdData;
  const dec = Math.max(2, Math.min(20, parseInt(document.getElementById('gdTableDec').value)||8));
  const sortBy = document.getElementById('gdTableSort').value;
  const topN = parseInt(document.getElementById('gdTableTop').value)||0;
  const hlSel = document.getElementById('gdTableHighlight').checked;

  let rows = gaps.filter(g=>gapProducts[g]).map((g,i)=>{
    const gp = gapProducts[g];
    const logShare = gp.logProduct / (totalLog||1) * 100;
    let cum=1;
    gaps.forEach(gg=>{ if(gg<=g && gapProducts[gg]) cum*=gapProducts[gg].product; });
    return { g, count:gp.count, product:gp.product, logProduct:gp.logProduct,
             logShare, cumulative:cum, relErr:Math.abs(cum-target)/target,
             pctPrimes:gp.count/((gdData.totalPrimes||gdData.primes.length)||1)*100, i };
  });

  if(sortBy==='count_desc') rows.sort((a,b)=>b.count-a.count);
  else if(sortBy==='logshare_desc') rows.sort((a,b)=>b.logShare-a.logShare);
  else if(sortBy==='product_desc') rows.sort((a,b)=>b.product-a.product);
  else if(sortBy==='cumulative') rows.sort((a,b)=>a.cumulative-b.cumulative);
  else rows.sort((a,b)=>a.g-b.g);

  if(topN>0) rows = rows.slice(0, topN);

  const fmt = (v, d) => typeof v === 'number' ? v.toFixed(d) : v;
  const fmtE = (v, d) => v < 1e-4 ? v.toExponential(d) : v.toFixed(d);

  let html = `<table>
    <thead><tr>
      <th>Gap</th>
      <th>Count</th>
      <th>% of primes</th>
      <th>Product</th>
      <th>log(Product)</th>
      <th>log-ζ share %</th>
      <th>Cumulative</th>
      <th>Rel. error</th>
      <th>Abs. error</th>
    </tr></thead><tbody>`;

  rows.forEach(r=>{
    const col = gapColor(r.g, r.i);
    const isSel = hlSel && selectedGaps.has(r.g);
    const selClass = isSel ? 'gap-row-selected' : '';
    const abErr = Math.abs(r.cumulative - target);
    html += `<tr class="${selClass}">
      <td class="gap-id" style="color:${col}">${r.g}</td>
      <td>${r.count.toLocaleString()}</td>
      <td>${fmt(r.pctPrimes, dec)}%</td>
      <td>${fmt(r.product, dec)}</td>
      <td>${fmt(r.logProduct, dec)}</td>
      <td>${fmt(r.logShare, dec)}%</td>
      <td>${fmt(r.cumulative, dec)}</td>
      <td>${fmtE(r.relErr, dec)}</td>
      <td>${fmtE(abErr, dec)}</td>
    </tr>`;
  });
  html += '</tbody></table>';
  document.getElementById('gdTableWrap').innerHTML = html;
}

// ═══════════════════════════════════════════════════════════════
//  CONVERGENCE TO π²/6 TABLE (dropdown)
// ═══════════════════════════════════════════════════════════════
document.getElementById('convDropHeader').addEventListener('click', ()=>{
  document.getElementById('convDropBody').classList.toggle('open');
  document.getElementById('convDropChev').classList.toggle('open');
});

function gdBuildConvTable(){
  if(!gdData) return;
  const {conv, target, s, gaps, gapProducts, totalLog} = gdData;
  const dec = Math.max(2, Math.min(20, parseInt(document.getElementById('convDec').value)||12));
  const sortKey = document.getElementById('convSort').value;
  const topN = parseInt(document.getElementById('convTop').value)||0;
  const showPi = document.getElementById('convShowPi').checked;
  const showLog = document.getElementById('convShowLog').checked;
  const showDelta = document.getElementById('convShowDelta').checked;

  const isZeta2 = s >= 1.99 && s <= 2.01;
  const TARGET_PI2_6 = Math.PI**2/6;

  // Build full row data: one row per gap step
  let rows = conv.map((c, i)=>{
    const gp = gapProducts[c.gap];
    const logShare = gp ? gp.logProduct/(totalLog||1)*100 : 0;
    const logErr = c.relErr > 0 ? Math.log10(c.relErr) : -Infinity;
    const improving = i>0 ? c.relErr < conv[i-1].relErr : true;
    return { ...c, logShare, logErr, improving, step:i+1 };
  });

  if(sortKey==='logshare_desc') rows = [...rows].sort((a,b)=>b.logShare-a.logShare);
  else if(sortKey==='cumulative_asc') rows = [...rows].sort((a,b)=>a.value-b.value);
  else if(sortKey==='relerr_asc') rows = [...rows].sort((a,b)=>a.relErr-b.relErr);
  // else natural gap order

  if(topN>0) rows = rows.slice(0, topN);

  // Best row (minimum relative error)
  const bestRelErr = Math.min(...rows.map(r=>r.relErr));

  const fmt = (v, d) => typeof v === 'number' ? v.toFixed(d) : '—';
  const fmtE = (v, d) => (v===null||v===undefined) ? '—' : v < 1e-4 ? v.toExponential(d) : v.toFixed(d);

  let ths = `<th>Step</th><th>Gap</th><th>log-ζ %</th>
    <th class="conv-cell-product">Cumulative product</th>
    <th>Target ζ(${s.toFixed(2)})</th>
    <th class="conv-cell-err">Rel. error</th>
    <th>Abs. error</th>`;
  if(showLog) ths += `<th class="conv-cell-logerr">log₁₀(err)</th>`;
  if(showDelta) ths += `<th>Δ product</th>`;
  if(showPi && isZeta2) ths += `<th class="conv-cell-pi">√(6·prod) ≈ π</th><th class="conv-cell-pi">|π est − π|</th>`;

  let html = `<table><thead><tr>${ths}</tr></thead><tbody>`;

  rows.forEach(r=>{
    const isBest = r.relErr === bestRelErr;
    const rowClass = isBest ? 'conv-row-best' : r.improving ? 'conv-row-improving' : 'conv-row-worsening';
    const col = gapColor(r.gap, gapIndexMap[r.gap]||0);
    const abErr = Math.abs(r.value - target);
    const pi_est = (showPi && isZeta2 && r.value > 0) ? Math.sqrt(6*r.value) : null;
    const pi_err = pi_est !== null ? Math.abs(pi_est - Math.PI) : null;

    let tds = `
      <td>${r.step}</td>
      <td class="conv-cell-gap" style="color:${col}">g=${r.gap}</td>
      <td>${fmt(r.logShare, dec)}%</td>
      <td class="conv-cell-product">${fmt(r.value, dec)}</td>
      <td>${fmt(target, dec)}</td>
      <td class="conv-cell-err">${fmtE(r.relErr, dec)}</td>
      <td>${fmtE(abErr, dec)}</td>`;
    if(showLog) tds += `<td class="conv-cell-logerr">${isFinite(r.logErr)?fmt(r.logErr,dec):'—'}</td>`;
    if(showDelta) tds += `<td>${fmtE(Math.abs(r.delta), dec)}</td>`;
    if(showPi && isZeta2){
      tds += `<td class="conv-cell-pi">${pi_est!==null?fmt(pi_est,dec):'—'}</td>`;
      tds += `<td class="conv-cell-pi">${pi_err!==null?fmtE(pi_err,dec):'—'}</td>`;
    }

    html += `<tr class="${rowClass}">${tds}</tr>`;
  });
  html += '</tbody></table>';
  document.getElementById('gdConvTableWrap').innerHTML = html;
}

// ═══════════════════════════════════════════════════════════════
//  REFINED WAVE PRESETS
// ═══════════════════════════════════════════════════════════════

function buildRefinedWavePresets(){}

// ═══════════════════════════════════════════════════════════════
//  CANVAS SIZING
// ═══════════════════════════════════════════════════════════════
function gdResizeCanvas(){
  const leftSidebar = window.innerWidth > 900 ? 320 : 0;
  const sphereW = Math.min(window.innerWidth - leftSidebar - 80, 900);
  const gapPanel = document.getElementById('gapWavePanel');

  if(!gapPanel || gapPanel.classList.contains('gap-hidden')){
    gdCanvas.width = Math.max(280, 400);
  } else {
    gdCanvas.width = Math.max(280, Math.floor((sphereW - 12) / 2));
  }
  gdCanvas.height = Math.max(200, Math.round(
    document.getElementById('cartCanvas') ? document.getElementById('cartCanvas').height || 200 : 200
  ));

  // Big accordion chart canvas
  const bigCv = document.getElementById('gdChartBig');
  if(bigCv){
    bigCv.width  = Math.max(400, sphereW);
    bigCv.height = Math.round(bigCv.width * 0.45);
  }

  if(gdData) gdDraw();
}
gdResizeCanvas();

// ═══════════════════════════════════════════════════════════════
//  CHART DRAWING
// ═══════════════════════════════════════════════════════════════
function gdDraw(offCtx, offW, offH){
  if(!gdData) return;
  // Support optional offscreen drawing for exports
  const useCtx = offCtx || gdCtx;
  const w = offW || gdCanvas.width;
  const h = offH || gdCanvas.height;

  useCtx.fillStyle='#060608'; useCtx.fillRect(0,0,w,h);

  // Temporarily swap gdCtx so draw functions see the right context
  const _saved = gdCtx;
  if(offCtx){ gdCtx = offCtx; }

  if(gdTab==='contrib') gdDrawContrib(w,h);
  else if(gdTab==='dist') gdDrawDist(w,h);
  else if(gdTab==='conv') gdDrawConv(w,h);
  else if(gdTab==='ratio') gdDrawRatio(w,h);
  else if(gdTab==='pct') gdDrawPct(w,h);

  if(offCtx){ gdCtx = _saved; }

  // Mirror to big accordion chart canvas (only for live draw)
  if(!offCtx){
    const bigCv = document.getElementById('gdChartBig');
    if(bigCv && bigCv.width > 0 && bigCv.height > 0){
      const bigCtx = bigCv.getContext('2d');
      bigCtx.fillStyle='#060608'; bigCtx.fillRect(0,0,bigCv.width,bigCv.height);
      bigCtx.drawImage(gdCanvas, 0, 0, bigCv.width, bigCv.height);
    }
  }
}

function gPad(){ return {l:60,r:14,t:28,b:42}; }
function gdTitle(ctx, w, txt){
  ctx.fillStyle='#ffd700'; ctx.textAlign='left';
  ctx.font=`bold ${Math.max(9,w*.022)}px JetBrains Mono,monospace`;
  ctx.fillText(txt, 60, 18);
}

function gdDrawContrib(w, h){
  const {gaps, gapProducts, totalLog, s} = gdData;
  if(!gaps.length) return;
  const p=gPad(), pw=w-p.l-p.r, ph=h-p.t-p.b;
  const ctx=gdCtx;
  const shares = gaps.map(g=>(gapProducts[g]?.logProduct||0)/totalLog*100);
  const maxS = Math.max(...shares);
  const barW = pw/gaps.length;

  ctx.font=`${Math.max(7,w*.018)}px JetBrains Mono,monospace`;
  gaps.forEach((g,i)=>{
    const pct=shares[i], bh=pct/maxS*ph;
    const x=p.l+i*barW, y=p.t+ph-bh;
    const col=gapColor(g,i);
    const isSel = selectedGaps.has(g);
    ctx.fillStyle=col+(isSel?'bb':'33'); ctx.fillRect(x+1,y,barW-2,bh);
    ctx.strokeStyle=col; ctx.lineWidth=isSel?2.5:1.2;
    ctx.strokeRect(x+1,y,barW-2,bh);
    if(barW>18){ ctx.fillStyle=col; ctx.textAlign='center'; ctx.fillText(''+g, x+barW/2, p.t+ph+16); }
    if(barW>28&&pct>0.5){ ctx.fillStyle='#fff'; ctx.globalAlpha=0.8; ctx.fillText(pct.toFixed(1)+'%', x+barW/2, y-3); ctx.globalAlpha=1; }
  });
  [0,25,50,75,100].forEach(pct=>{
    if(pct<=maxS*1.1){ const y=p.t+ph-pct/maxS*ph;
      ctx.strokeStyle='#182018'; ctx.lineWidth=0.5;
      ctx.beginPath(); ctx.moveTo(p.l,y); ctx.lineTo(w-p.r,y); ctx.stroke();
      ctx.fillStyle='#3a7040'; ctx.textAlign='right'; ctx.textBaseline='middle';
      ctx.fillText(pct+'%', p.l-4, y);
    }
  });
  gdTitle(ctx, w, `log-ζ(${s.toFixed(2)}) contribution % by gap`);
  gdBuildLegend(gaps.slice(0,16));
}

function gdDrawDist(w, h){
  const {gaps, gapProducts, primes, s, maxN, target, finalProduct, finalError} = gdData;
  if(!gaps.length) return;
  const ctx = gdCtx;

  // Layout: header stats strip + chart + x-axis label
  const PAD_L=72, PAD_R=24, PAD_T=80, PAD_B=52;
  const pw = w - PAD_L - PAD_R;
  const ph = h - PAD_T - PAD_B;

  // ── Title ──
  ctx.fillStyle = '#ffd700';
  ctx.font = `bold ${Math.max(11,w*.024)}px JetBrains Mono,monospace`;
  ctx.textAlign = 'center';
  ctx.fillText('Prime Distribution by Gap Class', w/2, 22);
  ctx.fillStyle = 'rgba(91,156,246,.5)';
  ctx.font = `${Math.max(8,w*.016)}px JetBrains Mono,monospace`;
  ctx.fillText('by Wessen Getachew', w/2, 38);

  // ── Stats header strip ──
  const statsH = 36;
  ctx.fillStyle = 'rgba(20,30,20,.9)';
  ctx.fillRect(PAD_L, PAD_T - statsH - 2, pw, statsH);
  const statsItems = [
    {lbl:'N Range',         val: maxN.toLocaleString()},
    {lbl:'Total Primes',    val: primes.length.toLocaleString()},
    {lbl:'Gap Families',    val: gaps.length.toString()},
    {lbl:`Target ζ(${s.toFixed(0)})`, val: target.toFixed(8)},
    {lbl:'Final Product',   val: finalProduct.toFixed(8)},
    {lbl:'Rel. Error',      val: (finalError*100).toFixed(6)+'%'},
  ];
  const iw = pw / statsItems.length;
  statsItems.forEach(({lbl,val}, i) => {
    const ix = PAD_L + i * iw + iw/2;
    ctx.fillStyle = 'rgba(91,156,246,.5)';
    ctx.font = `${Math.max(6,w*.013)}px JetBrains Mono,monospace`;
    ctx.textAlign = 'center';
    ctx.fillText(lbl, ix, PAD_T - statsH + 11);
    ctx.fillStyle = '#ffd700';
    ctx.font = `bold ${Math.max(7,w*.016)}px JetBrains Mono,monospace`;
    ctx.fillText(val, ix, PAD_T - statsH + 25);
  });

  // ── Chart section title ──
  ctx.fillStyle = '#ffd700';
  ctx.font = `bold ${Math.max(9,w*.019)}px JetBrains Mono,monospace`;
  ctx.textAlign = 'center';
  ctx.fillText('Number of Primes per Gap Family', w/2, PAD_T - 5);

  const counts = gaps.map(g => gapProducts[g]?.count || 0);
  const maxC = Math.max(...counts, 1);
  const barW = Math.max(2, pw / gaps.length);

  // ── Y-axis gridlines + labels ──
  const yTicks = 6;
  for(let i = 0; i <= yTicks; i++){
    const frac = i / yTicks;
    const v = Math.round(frac * maxC);
    const y = PAD_T + ph - frac * ph;
    ctx.strokeStyle = i === 0 ? 'rgba(255,255,255,.15)' : 'rgba(255,255,255,.05)';
    ctx.lineWidth = i === 0 ? 1 : 0.5;
    ctx.beginPath(); ctx.moveTo(PAD_L, y); ctx.lineTo(PAD_L + pw, y); ctx.stroke();
    ctx.fillStyle = 'rgba(150,200,150,.7)';
    ctx.font = `${Math.max(6,w*.013)}px JetBrains Mono,monospace`;
    ctx.textAlign = 'right';
    ctx.fillText(v >= 1e6 ? (v/1e6).toFixed(1)+'M' : v >= 1e3 ? (v/1e3).toFixed(0)+'k' : v.toString(), PAD_L - 5, y + 4);
  }

  // Y-axis label
  ctx.save();
  ctx.fillStyle = 'rgba(150,200,150,.6)';
  ctx.font = `${Math.max(7,w*.014)}px JetBrains Mono,monospace`;
  ctx.translate(14, PAD_T + ph/2);
  ctx.rotate(-Math.PI/2);
  ctx.textAlign = 'center';
  ctx.fillText('Prime Count', 0, 0);
  ctx.restore();

  // ── Bars ──
  gaps.forEach((g,i) => {
    const c = counts[i];
    const bh = (c / maxC) * ph;
    const x = PAD_L + i * barW;
    const y = PAD_T + ph - bh;
    const col = gapColor(g, i);
    const isSel = selectedGaps.has(g);

    // Bar fill - bright green like Image 3 for non-selected, highlighted for selected
    ctx.fillStyle = isSel ? col + 'ee' : '#22cc44aa';
    ctx.fillRect(x + 0.5, y, barW - 1, bh);
    if(isSel){
      ctx.strokeStyle = col;
      ctx.lineWidth = 2;
      ctx.strokeRect(x + 0.5, y, barW - 1, bh);
    }

    // Value label on top for larger bars
    if(bh > 20 && barW > 12){
      ctx.fillStyle = '#fff';
      ctx.globalAlpha = 0.85;
      ctx.font = `${Math.max(6,w*.013)}px JetBrains Mono,monospace`;
      ctx.textAlign = 'center';
      ctx.fillText(c >= 1e3 ? (c/1e3).toFixed(0)+'k' : c.toString(), x + barW/2, y - 2);
      ctx.globalAlpha = 1;
    }
  });

  // ── X-axis gap labels ──
  const labelStep = Math.max(1, Math.floor(gaps.length / 40));
  ctx.fillStyle = 'rgba(150,180,150,.7)';
  ctx.font = `${Math.max(5.5,w*.012)}px JetBrains Mono,monospace`;
  ctx.textAlign = 'center';
  gaps.forEach((g,i) => {
    if(i % labelStep === 0){
      ctx.fillText(g.toString(), PAD_L + (i + 0.5) * barW, PAD_T + ph + 14);
    }
  });

  // X-axis label
  ctx.fillStyle = 'rgba(150,180,150,.6)';
  ctx.font = `${Math.max(8,w*.015)}px JetBrains Mono,monospace`;
  ctx.textAlign = 'center';
  ctx.fillText('Gap Size', w/2, h - 4);
}

function gdDrawConv(w, h){
  const {conv, target, s} = gdData;
  if(conv.length<2) return;
  const p=gPad(), pw=w-p.l-p.r, ph=h-p.t-p.b;
  const ctx=gdCtx;
  const vals=conv.map(d=>d.value);
  const minV=Math.min(target*0.8,...vals), maxV=Math.max(target*1.1,...vals);
  const xS=pw/(conv.length-1), yS=ph/(maxV-minV);
  ctx.font=`${Math.max(7,w*.018)}px JetBrains Mono,monospace`;
  const ty=p.t+ph-(target-minV)*yS;
  ctx.strokeStyle='#ffd70066'; ctx.lineWidth=1; ctx.setLineDash([6,4]);
  ctx.beginPath(); ctx.moveTo(p.l,ty); ctx.lineTo(w-p.r,ty); ctx.stroke();
  ctx.setLineDash([]);
  ctx.fillStyle='#ffd700'; ctx.textAlign='right';
  ctx.fillText(`ζ(${s})=${target.toFixed(6)}`, w-p.r-2, ty-5);
  ctx.strokeStyle='var(--blue,#5b9cf6)'; ctx.lineWidth=2;
  ctx.shadowColor='var(--blue,#5b9cf6)'; ctx.shadowBlur=6;
  ctx.beginPath();
  conv.forEach((d,i)=>{ const x=p.l+i*xS, y=p.t+ph-(d.value-minV)*yS; i===0?ctx.moveTo(x,y):ctx.lineTo(x,y); });
  ctx.stroke(); ctx.shadowBlur=0;
  for(let i=0;i<=5;i++){ const frac=i/5, v=minV+frac*(maxV-minV), y=p.t+ph-frac*ph;
    ctx.strokeStyle='#182018'; ctx.lineWidth=0.5;
    ctx.beginPath(); ctx.moveTo(p.l,y); ctx.lineTo(w-p.r,y); ctx.stroke();
    ctx.fillStyle='#3a7040'; ctx.textAlign='right'; ctx.textBaseline='middle';
    ctx.fillText(v.toFixed(4), p.l-4, y);
  }
  ctx.fillStyle='#88cc88'; ctx.textAlign='center';
  const step=Math.max(1,Math.floor(conv.length/8));
  conv.forEach((d,i)=>{ if(i%step===0){ ctx.fillText('g'+d.gap,p.l+i*xS,p.t+ph+18); }});
  gdTitle(ctx, w, `Cumulative Euler product → ζ(${s.toFixed(2)})`);
  gdBuildLegend([]);
}

function gdDrawRatio(w, h){
  // Use gapProducts as the single source of truth for all gap counts.
  // c2/c4/c6 on gdData are only set when gap 2/4/6 are in the current filter;
  // falling back to gapProducts covers all filter modes safely.
  const { primes, s, maxN, gapProducts: gp } = gdData;
  const c2   = gp[2]?.count  || gdData.c2  || 0;
  const c4   = gp[4]?.count  || gdData.c4  || 0;
  const c6   = gp[6]?.count  || gdData.c6  || 0;
  const c10  = gp[10]?.count || 0;
  const c12_v = gp[12]?.count || 0;
  const ctx = gdCtx;
  const FONT = `JetBrains Mono,monospace`;

  // ── Info box header (no duplicate title — it's in the HTML panel header above) ──
  const titleSz = Math.max(11, Math.round(w * 0.019));

  // Info box — starts near top of canvas
  const infoFontSz = Math.max(8, Math.round(w * 0.012));
  const infoLineH  = infoFontSz * 1.45;
  const infoLines  = 2; // two lines of description
  const infoH      = infoLines * infoLineH + 10;
  const infoY      = 4;
  ctx.fillStyle = 'rgba(232,197,71,.06)';
  ctx.strokeStyle = 'rgba(232,197,71,.2)';
  ctx.lineWidth = 1;
  ctx.beginPath(); ctx.roundRect(12, infoY, w - 24, infoH, 4); ctx.fill(); ctx.stroke();
  ctx.fillStyle = 'rgba(200,210,180,.72)';
  ctx.font = `${infoFontSz}px ${FONT}`;
  ctx.textAlign = 'left';
  ctx.textBaseline = 'top';
  const infoText = `H-L predicts twin primes (gap 2) ≡ cousin primes (gap 4) — ratio → 1.0 as N→∞.  Gap 6 (sexy primes) has S(6)=2·S(2) so it is asymptotically twice as frequent.`;
  wrapText(ctx, infoText, 18, infoY + 5, w - 36, infoLineH);

  // Three sub-panels — start below header
  const PANEL_Y = infoY + infoH + 8;
  const PANEL_H = h - PANEL_Y - 16;
  const pW1 = Math.floor(w * 0.27);
  const pW2 = Math.floor(w * 0.39);
  const pW3 = w - pW1 - pW2 - 24;
  const p1x = 0, p2x = pW1 + 10, p3x = pW1 + pW2 + 20;

  // ─── Panel 1: Gap Count Ratios ───
  {
    const px = p1x + 10, py = PANEL_Y, pw = pW1 - 20, ph = PANEL_H;
    const lblSz   = Math.max(7, Math.round(w * 0.013));
    const valSz   = Math.max(8, Math.round(w * 0.014));
    const tickSz  = Math.max(6, Math.round(w * 0.010));
    const xLblH   = tickSz * 2.6;  // height reserved below chart for 2-line x-labels
    const topPad  = lblSz + 10;    // header
    const chartTop = py + topPad;
    const chartH   = ph - topPad - xLblH - 4;

    // Panel header
    ctx.fillStyle = 'rgba(232,197,71,.6)';
    ctx.font = `bold ${lblSz}px ${FONT}`;
    ctx.textAlign = 'center';
    ctx.textBaseline = 'alphabetic';
    ctx.fillText('Gap Count Ratios', px + pw/2, py + lblSz);

    const ratios = [
      { lbl:['Gap 2','Gap 4'], val: c4>0 ? c2/c4 : null, tgt:1.0, col:'#22cc44' },
      { lbl:['Gap 2','Gap 6'], val: c6>0 ? c2/c6 : null, tgt:0.5, col:'#5b9cf6' },
      { lbl:['Gap 4','Gap 6'], val: c6>0 ? c4/c6 : null, tgt:0.5, col:'#cc88ff' },
    ];
    const bw = Math.floor(pw / ratios.length) - 6;
    const maxR = 1.5;
    const toY = v => chartTop + chartH - Math.min(Math.max(v,0)/maxR, 1) * chartH;

    // Y gridlines + labels
    [0, 0.5, 1.0, 1.5].forEach(v => {
      const gy = toY(v);
      ctx.strokeStyle = v === 1.0 ? 'rgba(255,255,255,.18)' : 'rgba(255,255,255,.07)';
      ctx.lineWidth = 0.5;
      ctx.beginPath(); ctx.moveTo(px, gy); ctx.lineTo(px + pw, gy); ctx.stroke();
      ctx.fillStyle = 'rgba(160,190,160,.65)';
      ctx.font = `${tickSz}px ${FONT}`;
      ctx.textAlign = 'right';
      ctx.fillText(v.toFixed(1), px - 3, gy + tickSz * 0.35);
    });

    // Ratio = 1.0 dashed
    const y1 = toY(1.0);
    ctx.strokeStyle = 'rgba(255,255,150,.28)';
    ctx.lineWidth = 1; ctx.setLineDash([4, 3]);
    ctx.beginPath(); ctx.moveTo(px, y1); ctx.lineTo(px + pw, y1); ctx.stroke();
    ctx.setLineDash([]);

    ratios.forEach(({ lbl, val, tgt, col }, i) => {
      if (val === null) return;
      const bx = px + i * (bw + 6);
      const by = toY(val);
      const bh = chartTop + chartH - by;

      // Bar body
      ctx.fillStyle = col + '44';
      ctx.fillRect(bx, by, bw, Math.max(bh, 1));
      ctx.strokeStyle = col; ctx.lineWidth = 1.5;
      ctx.strokeRect(bx, by, bw, Math.max(bh, 1));

      // Value label — above bar if there's room, inside if not
      ctx.fillStyle = col;
      ctx.font = `bold ${valSz}px ${FONT}`;
      ctx.textAlign = 'center';
      const labelY = by > chartTop + valSz + 4 ? by - 4 : by + valSz + 4;
      ctx.fillText(val.toFixed(4), bx + bw/2, labelY);

      // Target dashed line
      const ty = toY(tgt);
      ctx.strokeStyle = col + '88'; ctx.lineWidth = 1; ctx.setLineDash([3, 3]);
      ctx.beginPath(); ctx.moveTo(bx - 2, ty); ctx.lineTo(bx + bw + 2, ty); ctx.stroke();
      ctx.setLineDash([]);

      // X labels — 2 lines, below chart area
      ctx.fillStyle = 'rgba(180,200,180,.75)';
      ctx.font = `${tickSz}px ${FONT}`;
      const lblBaseY = chartTop + chartH + tickSz + 2;
      lbl.forEach((l, li) => ctx.fillText(l, bx + bw/2, lblBaseY + li * (tickSz + 2)));
    });
  }

  // ─── Panel 2: Gap % of Total (bar chart) ───
  {
    const px = p2x + 6, py = PANEL_Y, pw = pW2 - 12, ph = PANEL_H;
    const lblSz   = Math.max(7, Math.round(w * 0.013));
    const valSz   = Math.max(8, Math.round(w * 0.014));
    const cntSz   = Math.max(6, Math.round(w * 0.011));
    const xLblH   = cntSz + 4;
    const topPad  = lblSz + 10;
    const chartTop = py + topPad;
    const chartH   = ph - topPad - xLblH - 4;

    // Panel header
    ctx.fillStyle = '#ffd700';
    ctx.font = `bold ${lblSz}px ${FONT}`;
    ctx.textAlign = 'center';
    ctx.textBaseline = 'alphabetic';
    ctx.fillText('Gap Ratio Convergence: Twin Prime Conjecture Evidence', px + pw/2, py + lblSz);

    const total = primes.length || 1;
    const pcts = [
      { cnt: c2,    col: '#ff6688', lbl: 'Gap 2'  },
      { cnt: c4,    col: '#5b9cf6', lbl: 'Gap 4'  },
      { cnt: c6,    col: '#22cc44', lbl: 'Gap 6'  },
      { cnt: c10,   col: '#cc88ff', lbl: 'Gap 10' },
      { cnt: c12_v, col: '#ffaa44', lbl: 'Gap 12' },
    ].map(d => ({ ...d, pct: d.cnt / total * 100 }));

    const bw = Math.floor(pw / pcts.length) - 8;
    const maxPct = Math.max(...pcts.map(d => d.pct)) * 1.18 || 20;
    const toY2 = v => chartTop + chartH - (v / maxPct) * chartH;

    // Y gridlines
    [0, maxPct * 0.33, maxPct * 0.66, maxPct].forEach(v => {
      const gy = toY2(v);
      ctx.strokeStyle = 'rgba(255,255,255,.06)';
      ctx.lineWidth = 0.5;
      ctx.beginPath(); ctx.moveTo(px, gy); ctx.lineTo(px + pw, gy); ctx.stroke();
      ctx.fillStyle = 'rgba(160,190,160,.5)';
      ctx.font = `${Math.max(6, Math.round(w * 0.010))}px ${FONT}`;
      ctx.textAlign = 'right';
      ctx.fillText(v.toFixed(1) + '%', px - 3, gy + 3);
    });

    pcts.forEach(({ pct, col, lbl, cnt }, i) => {
      const bx = px + 4 + i * (bw + 8);
      const by = toY2(pct);
      const bh = chartTop + chartH - by;

      ctx.fillStyle = col + '88';
      ctx.fillRect(bx, by, bw, Math.max(bh, 1));
      ctx.strokeStyle = col; ctx.lineWidth = 1.5;
      ctx.strokeRect(bx, by, bw, Math.max(bh, 1));

      // Count label above % label — stack them only if both fit
      const pctLabelY = by - 4;
      const cntLabelY = pctLabelY - valSz - 2;

      ctx.fillStyle = '#fff';
      ctx.globalAlpha = 0.92;
      ctx.font = `bold ${valSz}px ${FONT}`;
      ctx.textAlign = 'center';
      ctx.fillText(pct.toFixed(2) + '%', bx + bw/2, pctLabelY);
      ctx.globalAlpha = 1;

      if (cntLabelY > chartTop + 2) {
        ctx.fillStyle = col + 'cc';
        ctx.font = `${cntSz}px ${FONT}`;
        ctx.fillText(cnt >= 1e6 ? (cnt/1e6).toFixed(1)+'M' : cnt >= 1e3 ? (cnt/1e3).toFixed(0)+'k' : cnt.toString(), bx + bw/2, cntLabelY);
      }

      // X label below chart
      ctx.fillStyle = 'rgba(180,200,180,.75)';
      ctx.font = `${cntSz}px ${FONT}`;
      ctx.fillText(lbl, bx + bw/2, chartTop + chartH + cntSz + 2);
    });

    // Footer note
    ctx.fillStyle = 'rgba(150,170,150,.5)';
    ctx.font = `${Math.max(6, Math.round(w * 0.010))}px ${FONT}`;
    ctx.textAlign = 'center';
    ctx.fillText(`${primes.length.toLocaleString()} primes up to ${maxN.toLocaleString()}`, px + pw/2, py + ph - 2);
  }

  // ─── Panel 3: Hardy-Littlewood Accuracy ───
  {
    const px = p3x + 6, py = PANEL_Y, pw = pW3 - 12, ph = PANEL_H;
    const lblSz  = Math.max(7, Math.round(w * 0.013));
    const valSz  = Math.max(8, Math.round(w * 0.013));
    const noteSz = Math.max(6, Math.round(w * 0.010));
    const xLblH  = noteSz * 3;   // two x-label lines + gap
    const errH   = noteSz * 4;   // error stat + diff note at bottom
    const topPad = lblSz + 10;
    const chartH = ph - topPad - xLblH - errH - 6;
    const chartTop = py + topPad;

    // Panel header
    ctx.fillStyle = 'rgba(232,197,71,.6)';
    ctx.font = `bold ${lblSz}px ${FONT}`;
    ctx.textAlign = 'center';
    ctx.textBaseline = 'alphabetic';
    ctx.fillText('Hardy-Littlewood Accuracy', px + pw/2, py + lblSz);

    const C2 = 0.6601618158468696;
    const N = maxN;
    const lnN = Math.log(N);
    const li2 = (N / (lnN * lnN)) * (1 + 2/lnN + 6/(lnN*lnN) + 24/(lnN*lnN*lnN));
    const hlPred = Math.round(2 * C2 * li2);
    const diff = c2 - hlPred;
    const errPct = c2 > 0 ? Math.abs(diff) / c2 * 100 : 0;

    const vals = [hlPred, c2];
    const maxV = Math.max(...vals) * 1.15 || 1;
    const bw2  = Math.floor(pw * 0.36);
    const gap2 = Math.floor((pw - bw2 * 2) / 3);

    [
      [hlPred, '#ffd700', ['Predicted', '(H-L)']],
      [c2,     '#22cc44', ['Observed',  '(Actual)']],
    ].forEach(([v, col, lblLines], i) => {
      const bx = px + gap2 + i * (bw2 + gap2);
      const bh = (v / maxV) * chartH;
      const by = chartTop + chartH - bh;

      ctx.fillStyle = col + '88';
      ctx.fillRect(bx, by, bw2, Math.max(bh, 1));
      ctx.strokeStyle = col; ctx.lineWidth = 1.5;
      ctx.strokeRect(bx, by, bw2, Math.max(bh, 1));

      // Value label above bar
      ctx.fillStyle = col;
      ctx.font = `bold ${valSz}px ${FONT}`;
      ctx.textAlign = 'center';
      ctx.fillText(v.toLocaleString(), bx + bw2/2, by - 4);

      // X labels below chart
      ctx.fillStyle = 'rgba(180,200,180,.75)';
      ctx.font = `${noteSz}px ${FONT}`;
      lblLines.forEach((l, li) => {
        ctx.fillText(l, bx + bw2/2, chartTop + chartH + noteSz + 2 + li * (noteSz + 2));
      });
    });

    // Error stats — below x-labels
    const errBaseY = chartTop + chartH + xLblH + noteSz;
    ctx.fillStyle = errPct < 1 ? '#22cc44' : errPct < 5 ? '#ffd700' : '#ff6688';
    ctx.font = `bold ${Math.max(7, Math.round(w * 0.012))}px ${FONT}`;
    ctx.textAlign = 'center';
    ctx.fillText(`Error: ${errPct.toFixed(4)}%`, px + pw/2, errBaseY);
    ctx.fillStyle = 'rgba(150,170,150,.55)';
    ctx.font = `${noteSz}px ${FONT}`;
    const diffTxt = diff >= 0
      ? `+${diff.toLocaleString()} more twins`
      : `${diff.toLocaleString()} fewer twins`;
    ctx.fillText(diffTxt, px + pw/2, errBaseY + noteSz + 3);
  }

  // Summary footer
  const hlC2 = 0.6601618158468696;
  const _lnN = Math.log(maxN);
  const _li2footer = (maxN / (_lnN * _lnN)) * (1 + 2/_lnN + 6/(_lnN*_lnN) + 24/(_lnN*_lnN*_lnN));
  const footerSz = Math.max(6, Math.round(w * 0.010));
  ctx.fillStyle = 'rgba(150,170,150,.45)';
  ctx.font = `${footerSz}px ${FONT}`;
  ctx.textAlign = 'center';
  ctx.textBaseline = 'bottom';
  ctx.fillText(
    `Twin Prime Constant C₂ = ${hlC2.toFixed(13)} | H-L Predicted (li₂): ${Math.round(2*hlC2*_li2footer).toLocaleString()} twins`,
    w/2, h - 2
  );
  ctx.textBaseline = 'alphabetic';

  gdBuildLegend([]);
}

// Helper: wrap text on canvas
function wrapText(ctx, text, x, y, maxW, lineH){
  const words = text.split(' ');
  let line = '';
  let cy = y;
  for(let n=0; n<words.length; n++){
    const test = line + words[n] + ' ';
    if(ctx.measureText(test).width > maxW && n > 0){
      ctx.fillText(line, x, cy);
      line = words[n] + ' ';
      cy += lineH;
    } else {
      line = test;
    }
  }
  ctx.fillText(line, x, cy);
}

function gdDrawPct(w, h){
  const {gaps, gapProducts, totalLog, s} = gdData;
  if(!gaps.length) return;
  const p=gPad(), pw=w-p.l-p.r, ph=h-p.t-p.b;
  const ctx=gdCtx;
  const sorted=gaps.filter(g=>gapProducts[g])
    .map((g,i)=>({g, share:gapProducts[g].logProduct/totalLog, i}))
    .sort((a,b)=>b.share-a.share);
  let y=p.t;
  sorted.forEach(({g,share,i})=>{
    const bh=share*ph, col=gapColor(g,i), isSel=selectedGaps.has(g);
    ctx.fillStyle=col+(isSel?'cc':'66'); ctx.fillRect(p.l,y,pw*0.35,bh);
    ctx.strokeStyle=col; ctx.lineWidth=isSel?2:1; ctx.strokeRect(p.l,y,pw*0.35,bh);
    if(bh>10){
      ctx.fillStyle='#fff'; ctx.globalAlpha=0.85; ctx.textAlign='left'; ctx.textBaseline='middle';
      ctx.font=`${Math.max(7,w*.017)}px JetBrains Mono,monospace`;
      ctx.fillText(`g=${g}  ${(share*100).toFixed(2)}%`, p.l+pw*0.35+8, y+bh/2);
      ctx.globalAlpha=1;
    }
    y+=bh;
  });
  gdTitle(ctx, w, `% share of log-ζ(${s.toFixed(2)}) by gap class`);
  gdBuildLegend(sorted.slice(0,12).map(({g})=>g));
}

function gdBuildLegend(gapList){
  const el=document.getElementById('gdLegend');
  el.innerHTML=gapList.map((g,i)=>{
    const col=gapColor(g,i);
    const isSel=selectedGaps.has(g);
    return `<div class="gap-leg" style="${isSel?'font-weight:700;':''}" onclick="toggleGapChipByVal(${g})" style="cursor:pointer">
      <div class="gap-leg-sw" style="background:${col};${isSel?'outline:2px solid '+col:''}"></div>
      gap ${g}${isSel?' ':''}
    </div>`;
  }).join('');
}

function toggleGapChipByVal(g){
  const chip = document.querySelector(`.gap-chip[data-gap="${g}"]`);
  if(chip){ const i = gapIndexMap[g]||0; toggleGapChip(g, i, chip, gapColor(g,i)); gdDraw(); }
}

// ═══════════════════════════════════════════════════════════════
//  EXPORTS
// ═══════════════════════════════════════════════════════════════
function gdExport4K(){
  if(!gdData) return;
  const FONT = 'JetBrains Mono,monospace';
  const { gaps, gapProducts, totalLog, primes: ps, s, maxN, target, finalProduct, finalError } = gdData;

  // ── Layout ──
  const W       = 3840;
  const HDR_H   = 130;
  const CHART_H = 1400;
  const COL_HDR = 54;
  const ROW_H   = 40;
  const TOTALS_H = 50;
  const STATS_H  = 60;
  const PAD      = 40;

  // Compute how many table rows fit
  const maxRows = gaps.length;
  const TABLE_H = COL_HDR + maxRows * ROW_H + TOTALS_H + 4;
  const H = HDR_H + CHART_H + STATS_H + TABLE_H + PAD;

  const cv  = document.createElement('canvas');
  cv.width  = W;
  cv.height = H;
  const ctx = cv.getContext('2d');

  // ── Background ──
  ctx.fillStyle = '#060608';
  ctx.fillRect(0, 0, W, H);

  // ══ HEADER ══
  const selList = [...selectedGaps.entries()]; // [[gapVal, color], ...]
  ctx.textBaseline = 'alphabetic';

  // Title segments - base info
  ctx.font = `bold 62px ${FONT}`;
  let titleX = PAD;
  const TY = HDR_H * 0.46;

  const baseTitleText = `Gap Decomp  ·  ζ(${s.toFixed(2)})  ·  N=${maxN.toLocaleString()}  ·  ${gdTab.toUpperCase()}`;
  ctx.fillStyle = '#ffd700';
  ctx.fillText(baseTitleText, titleX, TY);
  titleX += ctx.measureText(baseTitleText).width;

  // Selected gaps in their own colors
  if(selList.length > 0){
    ctx.fillStyle = 'rgba(255,255,255,.45)';
    ctx.font = `42px ${FONT}`;
    const sepText = '   Selected:';
    ctx.fillText(sepText, titleX, TY);
    titleX += ctx.measureText(sepText).width;
    selList.forEach(([g, col]) => {
      ctx.fillStyle = col;
      ctx.font = `bold 48px ${FONT}`;
      const gt = `  g=${g}`;
      ctx.fillText(gt, titleX, TY);
      titleX += ctx.measureText(gt).width;
    });
  }

  // Subtitle
  ctx.fillStyle = '#88cc88';
  ctx.font = `34px ${FONT}`;
  ctx.textAlign = 'left';
  ctx.fillText(
    `${ps.length.toLocaleString()} primes  ·  ${gaps.length} families  ·  err=${(finalError*100).toFixed(8)}%  ·  computed=${finalProduct.toFixed(10)}`,
    PAD, HDR_H * 0.80
  );
  ctx.fillStyle = 'rgba(91,156,246,.5)';
  ctx.font = `30px ${FONT}`;
  ctx.textAlign = 'right';
  ctx.fillText('Wessen Getachew · @7dview', W - PAD, HDR_H * 0.80);
  ctx.textAlign = 'left';

  // ══ CHART at full 4K width ══
  const chartCv  = document.createElement('canvas');
  chartCv.width  = W;
  chartCv.height = CHART_H;
  const chartCtx = chartCv.getContext('2d');
  chartCtx.fillStyle = '#060608';
  chartCtx.fillRect(0, 0, W, CHART_H);
  gdDraw(chartCtx, W, CHART_H);
  ctx.drawImage(chartCv, 0, HDR_H, W, CHART_H);

  // ══ STATS STRIP ══
  const SY = HDR_H + CHART_H;
  ctx.fillStyle = 'rgba(10,20,10,.95)';
  ctx.fillRect(0, SY, W, STATS_H);
  const statItems = [
    {lbl:'N Range',      val: maxN.toLocaleString()},
    {lbl:'Total Primes', val: ps.length.toLocaleString()},
    {lbl:'Gap Families', val: gaps.length.toString()},
    {lbl:`Target ζ(${s.toFixed(1)})`, val: target.toFixed(8)},
    {lbl:'Final Product', val: finalProduct.toFixed(8)},
    {lbl:'Rel. Error',   val: (finalError*100).toFixed(8)+'%'},
  ];
  const iw = W / statItems.length;
  statItems.forEach(({lbl, val}, i) => {
    const ix = i*iw + iw/2;
    ctx.fillStyle = 'rgba(91,156,246,.5)'; ctx.font = `22px ${FONT}`; ctx.textAlign = 'center';
    ctx.fillText(lbl, ix, SY + 18);
    ctx.fillStyle = '#ffd700'; ctx.font = `bold 28px ${FONT}`;
    ctx.fillText(val, ix, SY + 46);
  });

  // ══ TABLE ══
  const TBY = SY + STATS_H;

  // Column definitions matching Image 3
  const colDefs = [
    {lbl:'Gap',          w:0.055, align:'left'},
    {lbl:'Count',        w:0.115, align:'right'},
    {lbl:'% of Total',   w:0.135, align:'right'},
    {lbl:'Cumulative',   w:0.125, align:'right'},
    {lbl:'Cumulative %', w:0.135, align:'right'},
  ];
  // Adjust: remaining width for extra cols if not ratio tab
  if(gdTab !== 'dist'){
    colDefs.push({lbl:'log-ζ Product', w:0.145, align:'right'});
    colDefs.push({lbl:'log-ζ Share %', w:0.135, align:'right'});
    colDefs.push({lbl:'Euler Product', w:0.155, align:'right'});
  }

  // Column header row
  ctx.fillStyle = 'rgba(20,50,30,.98)';
  ctx.fillRect(0, TBY, W, COL_HDR);
  let cxStart = PAD;
  const colWidths = colDefs.map(c => Math.floor((W - 2*PAD) * c.w));
  colDefs.forEach(({lbl,align}, ci) => {
    const cw = colWidths[ci];
    ctx.fillStyle = '#ffd700'; ctx.font = `bold 28px ${FONT}`;
    ctx.textAlign = align;
    ctx.fillText(lbl, align==='right' ? cxStart+cw-8 : cxStart+8, TBY + COL_HDR - 14);
    cxStart += cw;
  });

  // Separator under header
  ctx.strokeStyle = 'rgba(232,197,71,.3)'; ctx.lineWidth = 1;
  ctx.beginPath(); ctx.moveTo(0,TBY+COL_HDR); ctx.lineTo(W,TBY+COL_HDR); ctx.stroke();

  // Data rows
  let cumCount = 0, cumPct = 0;
  let rowY = TBY + COL_HDR + 2;
  let cumEuler = 1;

  gaps.forEach((g, ri) => {
    const gp = gapProducts[g]; if(!gp) return;
    cumCount += gp.count;
    const pct      = gp.count / (ps.length || 1) * 100;
    cumPct        += pct;
    cumEuler      *= gp.product;
    const logShare = gp.logProduct / (totalLog || 1) * 100;
    const col      = gapColor(g, ri);
    const isSel    = selectedGaps.has(g);

    // Row background
    ctx.fillStyle = isSel ? col + '22' : ri%2===0 ? 'rgba(255,255,255,.015)' : 'transparent';
    ctx.fillRect(0, rowY, W, ROW_H);

    // Selected highlight bar on left edge
    if(isSel){
      ctx.fillStyle = col;
      ctx.fillRect(0, rowY, 6, ROW_H);
    }

    const vals = [
      {v: g.toString(),            col: col, bold: true},
      {v: gp.count.toLocaleString()},
      {v: pct.toFixed(8)+'%'},
      {v: cumCount.toLocaleString()},
      {v: cumPct.toFixed(8)+'%'},
    ];
    if(gdTab !== 'dist'){
      vals.push({v: gp.logProduct.toFixed(10)});
      vals.push({v: logShare.toFixed(8)+'%'});
      vals.push({v: cumEuler.toFixed(10)});
    }

    let vx = PAD;
    colDefs.forEach(({align}, ci) => {
      const cw = colWidths[ci];
      const d  = vals[ci] || {v:''};
      ctx.fillStyle = d.col || (isSel ? col+'cc' : 'rgba(170,200,170,.8)');
      ctx.font = `${d.bold ? 'bold ' : ''}24px ${FONT}`;
      ctx.textAlign = align;
      ctx.fillText(d.v, align==='right' ? vx+cw-8 : vx+8, rowY + ROW_H - 11);
      vx += cw;
    });

    // Row separator
    ctx.strokeStyle = 'rgba(255,255,255,.03)'; ctx.lineWidth = 0.5;
    ctx.beginPath(); ctx.moveTo(0,rowY+ROW_H); ctx.lineTo(W,rowY+ROW_H); ctx.stroke();
    rowY += ROW_H;
  });

  // Displayed / Limit rows (like Image 3)
  const displayedY = rowY;
  ctx.fillStyle = 'rgba(91,156,246,.18)'; ctx.fillRect(0,displayedY,W,TOTALS_H/2);
  ctx.fillStyle = '#5b9cf6'; ctx.font=`bold 24px ${FONT}`; ctx.textAlign='left';
  ctx.fillText('Displayed', PAD+8, displayedY+TOTALS_H/2-10);
  ctx.textAlign='right';
  const c1w = colWidths[0], c2w=colWidths[1], c3w=colWidths[2], c4w=colWidths[3], c5w=colWidths[4];
  ctx.fillText(cumCount.toLocaleString(),       PAD+c1w+c2w-8,                  displayedY+TOTALS_H/2-10);
  ctx.fillText(cumPct.toFixed(8)+'%',           PAD+c1w+c2w+c3w-8,              displayedY+TOTALS_H/2-10);
  ctx.fillText(cumCount.toLocaleString(),       PAD+c1w+c2w+c3w+c4w-8,          displayedY+TOTALS_H/2-10);
  ctx.fillText(cumPct.toFixed(8)+'%',           PAD+c1w+c2w+c3w+c4w+c5w-8,     displayedY+TOTALS_H/2-10);

  const limitY = displayedY + TOTALS_H/2;
  ctx.fillStyle = 'rgba(91,156,246,.1)'; ctx.fillRect(0,limitY,W,TOTALS_H/2);
  ctx.fillStyle = '#88aacc'; ctx.font=`bold 24px ${FONT}`; ctx.textAlign='left';
  ctx.fillText('Limit', PAD+8, limitY+TOTALS_H/2-10);
  ctx.textAlign='right';
  ctx.fillText(ps.length.toLocaleString(),       PAD+c1w+c2w-8,                 limitY+TOTALS_H/2-10);
  ctx.fillText('100.0%',                          PAD+c1w+c2w+c3w-8,             limitY+TOTALS_H/2-10);

  // Footer note
  ctx.fillStyle = 'rgba(150,170,150,.45)'; ctx.font=`22px ${FONT}`; ctx.textAlign='center';
  ctx.fillText(
    `Showing ${gaps.length} rows  |  Analysis: ${ps.length.toLocaleString()} primes up to ${maxN.toLocaleString()}`,
    W/2, limitY + TOTALS_H/2 - 10
  );

  // ── Download ──
  const selSuffix = selList.length > 0 ? '_g'+selList.map(([g])=>g).join('_g') : '';
  const link = document.createElement('a');
  link.download = `gap_decomp_4K_N${maxN}_s${s.toFixed(2)}_${gdTab}${selSuffix}.png`;
  link.href = cv.toDataURL('image/png');
  link.click();

  // Redraw live canvas
  gdDraw();
}

// ── New Chart export functions ────────────────────────────────────────────

function gdExportChartCSV(){
  // Re-use existing CSV exporter but named clearly for the export row button
  gdExportCSV();
}

function gdExportDOM(){
  // DOM-screenshot of the entire accordion chart area
  const wrap = document.getElementById('gdChartArea');
  if(!wrap){ alert('No chart data yet — run decomposition first.'); return; }
  if(typeof html2canvas === 'undefined'){ alert('html2canvas not loaded.'); return; }
  html2canvas(wrap, {
    scale: 2,
    backgroundColor: '#060608',
    logging: false,
    useCORS: true,
    allowTaint: true
  }).then(cv => {
    const a = document.createElement('a');
    a.download = `gap_decomp_DOM_${gdData?.maxN||''}_${gdTab}.png`;
    a.href = cv.toDataURL('image/png');
    a.click();
  }).catch(e => alert('Export failed: ' + e));
}

function gdExportCanvasPNG(){
  // Direct canvas export at 4K quality
  const bigCv = document.getElementById('gdChartBig');
  if(!bigCv || !gdData){ alert('Run decomposition first.'); return; }
  // Ensure 4K redraw
  const bCtx = bigCv.getContext('2d');
  bCtx.fillStyle='#060608'; bCtx.fillRect(0,0,bigCv.width,bigCv.height);
  const _saved = gdCtx; gdCtx = bCtx;
  if(gdTab==='contrib') gdDrawContrib(bigCv.width,bigCv.height);
  else if(gdTab==='dist') gdDrawDist(bigCv.width,bigCv.height);
  else if(gdTab==='conv') gdDrawConv(bigCv.width,bigCv.height);
  else if(gdTab==='ratio') gdDrawRatio(bigCv.width,bigCv.height);
  else if(gdTab==='pct') gdDrawPct(bigCv.width,bigCv.height);
  gdCtx = _saved;
  const a = document.createElement('a');
  a.download = `gap_decomp_canvas_4K_${gdData.maxN}_${gdTab}.png`;
  a.href = bigCv.toDataURL('image/png');
  a.click();
}

// ── Standalone Gap Ratio Panel ───────────────────────────────────────────

function gdUpdateRatioPanel(){
  if(!gdData) return;
  const panel = document.getElementById('gdRatioPanel');
  if(panel) panel.style.display = 'block';

  // Double-rAF: first frame triggers reflow, second reads correct offsetWidth.
  requestAnimationFrame(function(){
    requestAnimationFrame(function(){
      const cv = document.getElementById('gdRatioChartBig');
      if(!cv) return;
      const panelWrap = cv.parentElement;
      const dpr = window.devicePixelRatio || 1;
      // CSS display width; fall back to 860 if panel hasn't reflowed yet
      const cssW  = panelWrap ? Math.max(400, panelWrap.offsetWidth || 860) : 860;
      const cssH  = Math.round(cssW * 0.55);   // taller than before (was 0.42)

      // Physical pixel resolution = CSS size × devicePixelRatio → sharp on HiDPI
      cv.width  = Math.round(cssW * dpr);
      cv.height = Math.round(cssH * dpr);
      cv.style.width  = cssW  + 'px';
      cv.style.height = cssH  + 'px';

      const rCtx = cv.getContext('2d');
      rCtx.scale(dpr, dpr);          // all draw calls now use CSS pixels
      rCtx.fillStyle = '#060608';
      rCtx.fillRect(0, 0, cssW, cssH);

      // Swap gdCtx so gdDrawRatio renders into ratio canvas (in CSS px space)
      const _saved = gdCtx;
      gdCtx = rCtx;
      gdDrawRatio(cssW, cssH);
      gdCtx = _saved;

      // Populate the ratio table
      gdBuildRatioTable();
    });
  });
}

function gdBuildRatioTable(){
  if(!gdData) return;
  const d = gdData;
  const c2  = d.c2  || d.gapProducts[2]?.count  || 0;
  const c4  = d.c4  || d.gapProducts[4]?.count  || 0;
  const c6  = d.c6  || d.gapProducts[6]?.count  || 0;
  const c10 = d.gapProducts[10]?.count || 0;
  const c12 = d.gapProducts[12]?.count || 0;
  const total = d.primes.length || 1;
  const N = d.maxN;

  const C2 = 0.6601618158468696;
  const lnN = Math.log(N);
  const li2 = N/(lnN*lnN) * (1 + 2/lnN + 6/(lnN*lnN) + 24/(lnN*lnN*lnN));
  function singS(h){ let m=1; for(let p=3;p<=h;p++){ if(h%p===0){let ip=true;for(let d=2;d*d<=p;d++) if(p%d===0){ip=false;break;} if(ip) m*=(p-1)/(p-2);}} return 2*C2*m; }
  const pred2 = singS(2)*li2, pred4 = singS(4)*li2, pred6 = singS(6)*li2;
  const r24   = c4>0 ? c2/c4 : 0;
  const r26   = c6>0 ? c2/c6 : 0;
  const r46   = c6>0 ? c4/c6 : 0;
  const hlErr2 = c2>0 ? Math.abs(c2-pred2)/c2*100 : 0;
  const hlErr4 = c4>0 ? Math.abs(c4-pred4)/c4*100 : 0;
  const hlErr6 = c6>0 ? Math.abs(c6-pred6)/c6*100 : 0;

  const fmt = (v, dec=6) => v.toFixed(dec);
  const pct = v => (v/total*100).toFixed(4)+'%';

  const statusCls = (v, tgt, tol=0.02) => Math.abs(v-tgt)<tol*tgt ? 'gd-ratio-val-good' : Math.abs(v-tgt)<0.1*tgt ? 'gd-ratio-val-warn' : 'gd-ratio-val-bad';

  const rows = [
    // Gap counts
    { m:'Gap 2 (Twin) count',        v:c2.toLocaleString(),           tgt:'—',     sc:'', interp:'Pairs (p, p+2)' },
    { m:'Gap 4 (Cousin) count',      v:c4.toLocaleString(),           tgt:'—',     sc:'', interp:'Pairs (p, p+4)' },
    { m:'Gap 6 (Sexy) count',        v:c6.toLocaleString(),           tgt:'~2×g2', sc:'', interp:'Pairs (p, p+6); S(6)=2×S(2)' },
    { m:'Gap 10 count',              v:c10.toLocaleString(),          tgt:'—',     sc:'', interp:'Pairs (p, p+10)' },
    { m:'Gap 12 count',              v:c12.toLocaleString(),          tgt:'—',     sc:'', interp:'Pairs (p, p+12)' },
    // Percentages
    { m:'Gap 2 % of total',          v:pct(c2),                       tgt:'—',     sc:'', interp:'Fraction of all primes in twin pairs' },
    { m:'Gap 4 % of total',          v:pct(c4),                       tgt:'~g2%',  sc:'', interp:'Should ≈ gap 2 % asymptotically' },
    { m:'Gap 6 % of total',          v:pct(c6),                       tgt:'~2×g2%',sc:'', interp:'Should be ~2× gap 2 % asymptotically' },
    // Ratios
    { m:'Ratio gap2/gap4',           v:fmt(r24,6),                    tgt:'→ 1.0', sc:statusCls(r24,1.0), interp:'H-L: identical constants → converges to 1' },
    { m:'Ratio gap2/gap6',           v:fmt(r26,6),                    tgt:'→ 0.5', sc:statusCls(r26,0.5), interp:'H-L: S(6)=2S(2) → gap2/gap6 → 0.5' },
    { m:'Ratio gap4/gap6',           v:fmt(r46,6),                    tgt:'→ 0.5', sc:statusCls(r46,0.5), interp:'H-L: S(6)=2S(4) → gap4/gap6 → 0.5' },
    // H-L predictions
    { m:'H-L predicted gap 2',       v:Math.round(pred2).toLocaleString(), tgt:c2.toLocaleString(), sc:'', interp:`Error: ${hlErr2.toFixed(4)}%` },
    { m:'H-L predicted gap 4',       v:Math.round(pred4).toLocaleString(), tgt:c4.toLocaleString(), sc:'', interp:`Error: ${hlErr4.toFixed(4)}%` },
    { m:'H-L predicted gap 6',       v:Math.round(pred6).toLocaleString(), tgt:c6.toLocaleString(), sc:'', interp:`Error: ${hlErr6.toFixed(4)}%` },
    // H-L accuracy
    { m:'H-L error (gap 2)',         v:hlErr2.toFixed(6)+'%',         tgt:'< 1%',  sc:hlErr2<1?'gd-ratio-val-good':hlErr2<5?'gd-ratio-val-warn':'gd-ratio-val-bad', interp:'Convergence to H-L constant C₂' },
    { m:'H-L error (gap 4)',         v:hlErr4.toFixed(6)+'%',         tgt:'< 1%',  sc:hlErr4<1?'gd-ratio-val-good':hlErr4<5?'gd-ratio-val-warn':'gd-ratio-val-bad', interp:'Convergence to H-L constant C₂' },
    { m:'Twin prime constant C₂',    v:'0.6601618158…',               tgt:'exact',sc:'', interp:'Product over primes p≥3 of p(p-2)/(p-1)²' },
    { m:'li₂(N) estimate',           v:Math.round(li2).toLocaleString(), tgt:'—', sc:'', interp:`4-term asymptotic series at N=${N.toLocaleString()}` },
  ];

  const tbody = document.getElementById('gdRatioTableBody');
  if(!tbody) return;
  tbody.innerHTML = rows.map(({m,v,tgt,sc,interp}) =>
    `<tr>
      <td>${m}</td>
      <td class="${sc}" style="font-weight:${sc?700:400};text-align:right">${v}</td>
      <td style="color:rgba(180,200,180,.45);text-align:center">${tgt}</td>
      <td style="text-align:center">${
        sc==='gd-ratio-val-good' ? '<span style="color:#5ebd78"> Good</span>' :
        sc==='gd-ratio-val-warn' ? '<span style="color:#e8c547">≈ Close</span>' :
        sc==='gd-ratio-val-bad'  ? '<span style="color:#f87171"> Far</span>' : '—'
      }</td>
      <td style="color:rgba(160,180,155,.55)">${interp}</td>
    </tr>`
  ).join('');
}

function gdExportRatioCSV(){
  if(!gdData) return;
  const d = gdData;
  const c2  = d.c2  || d.gapProducts[2]?.count  || 0;
  const c4  = d.c4  || d.gapProducts[4]?.count  || 0;
  const c6  = d.c6  || d.gapProducts[6]?.count  || 0;
  const c10 = d.gapProducts[10]?.count || 0;
  const c12 = d.gapProducts[12]?.count || 0;
  const total = d.primes.length || 1;
  const N = d.maxN;
  const C2 = 0.6601618158468696;
  const lnN = Math.log(N);
  const li2 = N/(lnN*lnN) * (1 + 2/lnN + 6/(lnN*lnN) + 24/(lnN*lnN*lnN));
  function singS(h){ let m=1; for(let p=3;p<=h;p++){ if(h%p===0){let ip=true;for(let d=2;d*d<=p;d++) if(p%d===0){ip=false;break;} if(ip) m*=(p-1)/(p-2);}} return 2*C2*m; }
  const pred2 = singS(2)*li2, pred4 = singS(4)*li2, pred6 = singS(6)*li2;
  let csv = 'Metric,Value\n';
  csv += `N Range,${N}\nTotal Primes,${total}\n`;
  csv += `Gap 2 Count,${c2}\nGap 4 Count,${c4}\nGap 6 Count,${c6}\nGap 10 Count,${c10}\nGap 12 Count,${c12}\n`;
  csv += `Gap2/Gap4 Ratio,${c4>0?c2/c4:0}\nGap2/Gap6 Ratio,${c6>0?c2/c6:0}\nGap4/Gap6 Ratio,${c6>0?c4/c6:0}\n`;
  csv += `HL Predicted Gap2,${Math.round(pred2)}\nHL Predicted Gap4,${Math.round(pred4)}\nHL Predicted Gap6,${Math.round(pred6)}\n`;
  csv += `HL Error Gap2 (%),${c2>0?Math.abs(c2-pred2)/c2*100:0}\nHL Error Gap4 (%),${c4>0?Math.abs(c4-pred4)/c4*100:0}\nHL Error Gap6 (%),${c6>0?Math.abs(c6-pred6)/c6*100:0}\n`;
  csvDownload(csv, `gap_ratio_analysis_N${N}.csv`);
}

function gdExportRatioDOM(){
  const wrap = document.getElementById('gdRatioPanel');
  if(!wrap || !gdData){ alert('Run decomposition first.'); return; }
  if(typeof html2canvas === 'undefined'){ alert('html2canvas not loaded.'); return; }
  html2canvas(wrap, {
    scale: 2,
    backgroundColor: '#0e1119',
    logging: false,
    useCORS: true,
    allowTaint: true
  }).then(cv => {
    const a = document.createElement('a');
    a.download = `gap_ratio_analysis_DOM_${gdData?.maxN||''}.png`;
    a.href = cv.toDataURL('image/png');
    a.click();
  }).catch(e => alert('DOM Export failed: ' + e));
}

function gdExportRatioCanvas(){
  if(!gdData){ alert('Run decomposition first.'); return; }
  // Always export at 3840px wide for 4K quality
  const EXP_W = 3840;
  const EXP_H = Math.round(EXP_W * 0.55);
  const expCv = document.createElement('canvas');
  expCv.width  = EXP_W;
  expCv.height = EXP_H;
  const expCtx = expCv.getContext('2d');
  expCtx.fillStyle = '#060608';
  expCtx.fillRect(0, 0, EXP_W, EXP_H);
  const _s = gdCtx; gdCtx = expCtx;
  gdDrawRatio(EXP_W, EXP_H);
  gdCtx = _s;
  const a = document.createElement('a');
  a.download = `gap_ratio_analysis_4K_N${gdData.maxN}.png`;
  a.href = expCv.toDataURL('image/png');
  a.click();
}

function gdExportCSV(){
  if(!gdData) return;
  const d=gdData;
  let csv='Gap,Count,PctPrimes,Product,LogProduct,LogShare(%),CumulativeProduct,RelativeError,AbsoluteError\n';
  let cum=1;
  d.gaps.forEach(g=>{
    const gp=d.gapProducts[g]; if(!gp) return;
    cum*=gp.product;
    const logShare=(gp.logProduct/(d.totalLog||1)*100).toFixed(10);
    const relErr=Math.abs(cum-d.target)/d.target;
    const abErr=Math.abs(cum-d.target);
    const pctPrimes=(gp.count/(d.primes.length||1)*100).toFixed(10);
    csv+=`${g},${gp.count},${pctPrimes},${gp.product.toFixed(15)},${gp.logProduct.toFixed(15)},${logShare},${cum.toFixed(15)},${relErr.toFixed(15)},${abErr.toFixed(15)}\n`;
  });
  csvDownload(csv, `gap_decomp_N${d.maxN}_s${d.s.toFixed(2)}.csv`);
}

function gdExportTXT(){
  if(!gdData) return;
  const d=gdData, now=new Date().toISOString();
  const pi_str=d.pi_calc?`Calculated π  = ${d.pi_calc.toFixed(15)}  (|err|=${Math.abs(d.pi_calc-Math.PI).toExponential(6)})`:'π: only valid at s=2';
  let txt=`
════════════════════════════════════════════════════════════
  PRIME GAP DECOMPOSITION REPORT — ζ(s) Euler Analysis
════════════════════════════════════════════════════════════
Generated : ${now}  |  Author: Wessen Getachew · @7dview

PARAMETERS
  Max N         = ${d.maxN.toLocaleString()}
  Exponent s    = ${d.s.toFixed(4)}
  Gap filter    = ${gdFilter}

PRIME STATISTICS
  Total primes  = ${d.primes.length.toLocaleString()}
  Max prime     = ${d._noRawPrimes ? `~${d.maxN.toLocaleString()} (approx)` : d.primes[d.primes.length-1].toLocaleString()}
  Gap families  = ${d.gaps.length}
  Largest gap   = ${Math.max(...d.gaps)}
  Most common g = ${d.mostCommon} (${d.mcCount.toLocaleString()} times)

TWIN / COUSIN / SEXY
  Gap 2 (twin)  = ${d.c2.toLocaleString()}
  Gap 4 (cousin)= ${d.c4.toLocaleString()}
  Gap 6 (sexy)  = ${d.c6.toLocaleString()}
  g2/g4 ratio   = ${d.c4>0?(d.c2/d.c4).toFixed(12):'N/A'}  [HL: 1.0]
  g2/g6 ratio   = ${d.c6>0?(d.c2/d.c6).toFixed(12):'N/A'}  [HL: 0.5]
  g4/g6 ratio   = ${d.c6>0?(d.c4/d.c6).toFixed(12):'N/A'}  [HL: 0.5]

ZETA PRODUCT
  Target ζ(${d.s.toFixed(2)}) = ${d.target.toFixed(15)}
  Computed      = ${d.finalProduct.toFixed(15)}
  Abs error     = ${Math.abs(d.finalProduct-d.target).toExponential(10)}
  Rel error     = ${(d.finalError*100).toFixed(12)}%
  ${pi_str}

GAP CLASS TABLE
${'Gap'.padEnd(8)}${'Count'.padEnd(14)}${'Pct%'.padEnd(12)}${'Product'.padEnd(22)}${'LogProd'.padEnd(22)}${'log-ζ%'.padEnd(16)}${'Cumulative'.padEnd(22)}RelErr%
${'─'.repeat(120)}
`;
  let cum=1;
  d.gaps.forEach(g=>{
    const gp=d.gapProducts[g]; if(!gp) return;
    cum*=gp.product;
    const relErr=Math.abs(cum-d.target)/d.target*100;
    const logShare=gp.logProduct/(d.totalLog||1)*100;
    const pct=gp.count/(d.primes.length||1)*100;
    txt+=`${(''+g).padEnd(8)}${gp.count.toString().padEnd(14)}${pct.toFixed(6).padEnd(12)}${gp.product.toFixed(12).padEnd(22)}${gp.logProduct.toFixed(12).padEnd(22)}${logShare.toFixed(6).padEnd(16)}${cum.toFixed(12).padEnd(22)}${relErr.toFixed(10)}\n`;
  });
  txt+=`\n${'═'.repeat(60)}\n`;
  const blob=new Blob([txt],{type:'text/plain'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob); a.download=`gap_decomp_report_N${d.maxN}_s${d.s.toFixed(2)}.txt`;
  a.click();
}

function csvDownload(csv, filename){
  const blob=new Blob([csv],{type:'text/csv'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob); a.download=filename; a.click();
}

// ── Section / Canvas Screenshot helpers ──────────────────────────
// screenshotCanvas: save a canvas element directly as PNG
// For the gap chart, routes to the full 4K export with chart+table
function screenshotCanvas(canvas, filename){
  if(!canvas) return;
  // Gap chart button → full 4K export (chart + table)
  if(filename === 'gap_chart' || canvas === gdCanvas || canvas === document.getElementById('gdChartBig')){
    if(gdData) gdExport4K();
    return;
  }
  if(canvas.width === 0) return;
  const a = document.createElement('a');
  a.href = canvas.toDataURL('image/png');
  a.download = (filename||'screenshot') + '.png';
  a.click();
}

// screenshotSection: render an HTML table/stats section as PNG using canvas drawing
function screenshotSection(elementId, filename){
  if(!gdData) return;

  // Route to the appropriate canvas-based export
  if(elementId === 'gdStatsGrid'){
    exportStatsGrid(filename);
    return;
  }
  if(elementId === 'gdTableWrap'){
    exportTableCanvas(filename);
    return;
  }
  if(elementId === 'gdConvTableWrap'){
    exportConvTableCanvas(filename);
    return;
  }

  // Fallback: try drawing the chart
  gdExport4K();
}

// ── Canvas-based table export ──
function exportStatsGrid(filename){
  if(!gdData) return;
  const d = gdData;
  const W = 2400, FONT = 'JetBrains Mono,monospace';
  const items = [
    { lbl:'Total Primes', val: d.primes.length.toLocaleString() },
    { lbl:'Gap Families', val: d.gaps.length.toString() },
    { lbl:`ζ(${d.s.toFixed(2)}) Target`, val: d.target.toFixed(10) },
    { lbl:'Computed Product', val: d.finalProduct.toFixed(10) },
    { lbl:'Relative Error', val: (d.finalError*100).toFixed(10)+'%' },
    { lbl:'Max N', val: d.maxN.toLocaleString() },
    { lbl:'Largest Gap', val: Math.max(...d.gaps).toString() },
    { lbl:'Most Common Gap', val: `g=${d.mostCommon} (${d.mcCount.toLocaleString()})` },
    { lbl:'Gap 2 (Twin)', val: d.c2.toLocaleString() },
    { lbl:'Gap 4 (Cousin)', val: d.c4.toLocaleString() },
    { lbl:'Gap 6 (Sexy)', val: d.c6.toLocaleString() },
    { lbl:'g2/g4 Ratio', val: d.c4>0?(d.c2/d.c4).toFixed(8):'N/A' },
    { lbl:'g2/g6 Ratio', val: d.c6>0?(d.c2/d.c6).toFixed(8):'N/A' },
    { lbl:'π approx', val: d.pi_calc ? d.pi_calc.toFixed(10) : 'n/a (s≠2)' },
  ];
  const COLS = 2, cellW = W/COLS, cellH = 72;
  const H = Math.ceil(items.length/COLS)*cellH + 100;
  const cv = document.createElement('canvas');
  cv.width = W; cv.height = H;
  const ctx = cv.getContext('2d');
  ctx.fillStyle = '#070810';
  ctx.fillRect(0,0,W,H);
  ctx.fillStyle = '#ffd700';
  ctx.font = `bold 40px ${FONT}`;
  ctx.textAlign = 'left';
  ctx.fillText(`Gap Decomp Stats · ζ(${d.s.toFixed(2)}) · N=${d.maxN.toLocaleString()}`, 30, 48);
  items.forEach(({lbl,val},i)=>{
    const col=i%COLS, row=Math.floor(i/COLS);
    const x=col*cellW, y=72+row*cellH;
    ctx.fillStyle = row%2===0?'rgba(255,255,255,.03)':'rgba(0,0,0,0)';
    ctx.fillRect(x,y,cellW,cellH);
    ctx.fillStyle='rgba(91,156,246,.5)';
    ctx.font=`28px ${FONT}`;
    ctx.textAlign='left';
    ctx.fillText(lbl, x+20, y+22);
    ctx.fillStyle='#ffd700';
    ctx.font=`bold 36px ${FONT}`;
    ctx.fillText(val, x+20, y+56);
    ctx.strokeStyle='rgba(255,255,255,.05)';
    ctx.lineWidth=0.5;
    ctx.strokeRect(x,y,cellW,cellH);
  });
  const a=document.createElement('a');
  a.download=(filename||'gap_stats')+'.png';
  a.href=cv.toDataURL('image/png'); a.click();
}

function exportTableCanvas(filename){
  if(!gdData) return;
  // Composite: Distribution histogram (top) + full data table (bottom) — matches Image 3
  const { gaps, gapProducts, totalLog, primes: ps, s, maxN, target, finalProduct, finalError } = gdData;
  const FONT = 'JetBrains Mono,monospace';
  const W = 3840;
  const CHART_H = 1500;
  const COL_HDR = 56, ROW_H = 40, STATS_H = 70, TOTALS_H = 50;
  const TABLE_H = COL_HDR + gaps.length * ROW_H + TOTALS_H + 10;
  const H = CHART_H + STATS_H + TABLE_H + 30;

  const cv = document.createElement('canvas');
  cv.width = W; cv.height = H;
  const ctx = cv.getContext('2d');
  ctx.fillStyle = '#060608'; ctx.fillRect(0,0,W,H);

  // Draw Distribution chart at 4K using offscreen context
  const savedTab = gdTab;
  gdTab = 'dist';
  const chartCv = document.createElement('canvas');
  chartCv.width = W; chartCv.height = CHART_H;
  const chartCtx = chartCv.getContext('2d');
  chartCtx.fillStyle = '#060608'; chartCtx.fillRect(0,0,W,CHART_H);
  gdDraw(chartCtx, W, CHART_H);
  gdTab = savedTab;
  ctx.drawImage(chartCv, 0, 0, W, CHART_H);

  // Stats strip
  const SY = CHART_H;
  ctx.fillStyle = 'rgba(10,20,10,.98)'; ctx.fillRect(0,SY,W,STATS_H);
  const statItems = [
    {lbl:'N Range',       val:maxN.toLocaleString()},
    {lbl:'Total Primes',  val:ps.length.toLocaleString()},
    {lbl:'Gap Families',  val:gaps.length.toString()},
    {lbl:`Target z(${Math.round(s)})`, val:target.toFixed(8)},
    {lbl:'Final Product', val:finalProduct.toFixed(8)},
    {lbl:'Rel. Error',    val:(finalError*100).toFixed(6)+'%'},
  ];
  const iw = W/statItems.length;
  statItems.forEach(({lbl,val},i)=>{
    const ix = i*iw+iw/2;
    ctx.fillStyle='rgba(91,156,246,.55)'; ctx.font=`22px ${FONT}`; ctx.textAlign='center';
    ctx.fillText(lbl, ix, SY+20);
    ctx.fillStyle='#ffd700'; ctx.font=`bold 30px ${FONT}`;
    ctx.fillText(val, ix, SY+50);
  });

  // Table
  const TBY = SY + STATS_H;
  const PAD = 40;
  const colDefs = [
    {lbl:'Gap',          w:0.06, align:'left'},
    {lbl:'Count',        w:0.14, align:'right'},
    {lbl:'% of Total',   w:0.15, align:'right'},
    {lbl:'Cumulative',   w:0.14, align:'right'},
    {lbl:'Cumulative %', w:0.15, align:'right'},
  ];
  const colWidths = colDefs.map(c => Math.floor((W - 2*PAD)*c.w));

  ctx.fillStyle='rgba(20,50,30,.98)'; ctx.fillRect(0,TBY,W,COL_HDR);
  let cxS=PAD;
  colDefs.forEach(({lbl,align},ci)=>{
    const cw=colWidths[ci];
    ctx.fillStyle='#ffd700'; ctx.font=`bold 28px ${FONT}`; ctx.textAlign=align;
    ctx.fillText(lbl, align==='right'?cxS+cw-8:cxS+8, TBY+COL_HDR-14);
    cxS+=cw;
  });
  ctx.strokeStyle='rgba(232,197,71,.3)'; ctx.lineWidth=1;
  ctx.beginPath(); ctx.moveTo(0,TBY+COL_HDR); ctx.lineTo(W,TBY+COL_HDR); ctx.stroke();

  let cumCount=0, cumPct=0;
  let rowY=TBY+COL_HDR+2;
  gaps.forEach((g,ri)=>{
    const gp=gapProducts[g]; if(!gp) return;
    cumCount+=gp.count;
    const pct=gp.count/(ps.length||1)*100;
    cumPct+=pct;
    const col=gapColor(g,ri);
    const isSel=selectedGaps.has(g);
    ctx.fillStyle=isSel?col+'22':ri%2===0?'rgba(255,255,255,.015)':'transparent';
    ctx.fillRect(0,rowY,W,ROW_H);
    if(isSel){ ctx.fillStyle=col; ctx.fillRect(0,rowY,6,ROW_H); }
    const vals=[
      {v:g.toString(),col,bold:true},
      {v:gp.count.toLocaleString()},
      {v:pct.toFixed(8)+'%'},
      {v:cumCount.toLocaleString()},
      {v:cumPct.toFixed(8)+'%'},
    ];
    let vx=PAD;
    colDefs.forEach(({align},ci)=>{
      const cw=colWidths[ci],d=vals[ci]||{v:''};
      ctx.fillStyle=d.col||(isSel?col+'cc':'rgba(170,200,170,.8)');
      ctx.font=`${d.bold?'bold ':''}24px ${FONT}`; ctx.textAlign=align;
      ctx.fillText(d.v, align==='right'?vx+cw-8:vx+8, rowY+ROW_H-11);
      vx+=cw;
    });
    ctx.strokeStyle='rgba(255,255,255,.03)'; ctx.lineWidth=0.5;
    ctx.beginPath(); ctx.moveTo(0,rowY+ROW_H); ctx.lineTo(W,rowY+ROW_H); ctx.stroke();
    rowY+=ROW_H;
  });

  const c1w=colWidths[0],c2w=colWidths[1],c3w=colWidths[2],c4w=colWidths[3],c5w=colWidths[4];
  const displayedY=rowY;
  ctx.fillStyle='rgba(91,156,246,.18)'; ctx.fillRect(0,displayedY,W,TOTALS_H/2);
  ctx.fillStyle='#5b9cf6'; ctx.font=`bold 24px ${FONT}`; ctx.textAlign='left';
  ctx.fillText('Displayed', PAD+8, displayedY+TOTALS_H/2-10);
  ctx.textAlign='right';
  ctx.fillText(cumCount.toLocaleString(),PAD+c1w+c2w-8,displayedY+TOTALS_H/2-10);
  ctx.fillText(cumPct.toFixed(8)+'%',PAD+c1w+c2w+c3w-8,displayedY+TOTALS_H/2-10);
  ctx.fillText(cumCount.toLocaleString(),PAD+c1w+c2w+c3w+c4w-8,displayedY+TOTALS_H/2-10);
  ctx.fillText(cumPct.toFixed(8)+'%',PAD+c1w+c2w+c3w+c4w+c5w-8,displayedY+TOTALS_H/2-10);

  const limitY=displayedY+TOTALS_H/2;
  ctx.fillStyle='rgba(30,50,80,.5)'; ctx.fillRect(0,limitY,W,TOTALS_H/2);
  ctx.fillStyle='#88aacc'; ctx.font=`bold 24px ${FONT}`; ctx.textAlign='left';
  ctx.fillText('Limit', PAD+8, limitY+TOTALS_H/2-10);
  ctx.textAlign='right';
  ctx.fillText(ps.length.toLocaleString(),PAD+c1w+c2w-8,limitY+TOTALS_H/2-10);
  ctx.fillText('100.0%',PAD+c1w+c2w+c3w-8,limitY+TOTALS_H/2-10);
  ctx.fillStyle='rgba(150,170,150,.5)'; ctx.font=`22px ${FONT}`; ctx.textAlign='center';
  ctx.fillText(`Showing ${gaps.length} rows  |  Analysis: ${ps.length.toLocaleString()} primes up to ${maxN.toLocaleString()}`, W/2, limitY+TOTALS_H/2-10);

  const a=document.createElement('a');
  a.download=(filename||'gap_distribution')+`_Canvas_${maxN}_${new Date().toISOString().slice(0,19).replace(/:/g,'-')}.png`;
  a.href=cv.toDataURL('image/png'); a.click();
}

function exportConvTableCanvas(filename){
  if(!gdData) return;
  const { conv, target, s, maxN } = gdData;
  const FONT = 'JetBrains Mono,monospace';
  const W=2800, ROW_H=38, HDR_H=90;
  const H=HDR_H+ROW_H+conv.length*ROW_H+50;
  const cv=document.createElement('canvas');
  cv.width=W; cv.height=Math.min(H,5000);
  const ctx=cv.getContext('2d');
  ctx.fillStyle='#070810'; ctx.fillRect(0,0,W,cv.height);
  ctx.fillStyle='#ffd700'; ctx.font=`bold 44px ${FONT}`; ctx.textAlign='left';
  ctx.fillText(`Convergence Table · ζ(${s.toFixed(2)}) · N=${maxN.toLocaleString()}`, 30, 55);
  const cols=[
    {lbl:'Gap',              w:.06,align:'left'},
    {lbl:'Cumulative Product',w:.18,align:'right'},
    {lbl:'Rel. Error %',     w:.15,align:'right'},
    {lbl:'Log Product',      w:.18,align:'right'},
    {lbl:'log-ζ Step %',     w:.15,align:'right'},
    {lbl:'π Estimate',       w:.18,align:'right'},
    {lbl:'Δ from π',         w:.10,align:'right'},
  ];
  ctx.fillStyle='rgba(30,50,30,.9)'; ctx.fillRect(0,HDR_H,W,ROW_H+4);
  let cx=0;
  cols.forEach(({lbl,w,align})=>{
    const cw=Math.floor(W*w);
    ctx.fillStyle='#ffd700'; ctx.font=`bold 26px ${FONT}`; ctx.textAlign=align;
    ctx.fillText(lbl, align==='right'?cx+cw-12:cx+12, HDR_H+ROW_H-8);
    cx+=cw;
  });
  conv.forEach(({gap,value,relErr,logProduct,pi_est,delta},ri)=>{
    const ry=HDR_H+ROW_H+4+ri*ROW_H;
    if(ry+ROW_H>cv.height-44) return;
    const col=gapColor(gap, ri);
    ctx.fillStyle=ri%2===0?'rgba(255,255,255,.02)':'rgba(0,0,0,0)';
    ctx.fillRect(0,ry,W,ROW_H);
    const vals=[
      {v:gap.toString(),col,bold:true},
      {v:value.toFixed(12)},
      {v:(relErr*100).toFixed(10)+'%', col:relErr<0.001?'#22cc44':relErr<0.01?'#ffd700':'#ff8888'},
      {v:logProduct.toFixed(12)},
      {v:((logProduct/gdData.totalLog)*100).toFixed(8)+'%'},
      {v:pi_est?pi_est.toFixed(12):'n/a'},
      {v:delta!=null?delta.toExponential(6):'—'},
    ];
    let vx=0;
    cols.forEach(({w,align},ci)=>{
      const cw=Math.floor(W*w),d=vals[ci]||{v:''};
      ctx.fillStyle=d.col||'rgba(180,210,180,.75)';
      ctx.font=`${d.bold?'bold ':''}24px ${FONT}`; ctx.textAlign=align;
      ctx.fillText(d.v, align==='right'?vx+cw-12:vx+12, ry+ROW_H-10);
      vx+=cw;
    });
    ctx.strokeStyle='rgba(255,255,255,.04)'; ctx.lineWidth=0.5;
    ctx.beginPath(); ctx.moveTo(0,ry+ROW_H); ctx.lineTo(W,ry+ROW_H); ctx.stroke();
  });
  const a=document.createElement('a');
  a.download=(filename||'gap_convergence')+'.png'; a.href=cv.toDataURL('image/png'); a.click();
}

// ═══════════════════════════════════════════════════════════════
//  HARMONICS MODULE
// ═══════════════════════════════════════════════════════════════

// Farey harmonic intervals: {ratio, name, p, q} where ratio=p/q
const HARM_INTERVALS = [
  { name:'1:1',   p:1,  q:1,  cents:0 },
  { name:'16:15', p:16, q:15, cents:112 },
  { name:'9:8',   p:9,  q:8,  cents:204 },
  { name:'6:5',   p:6,  q:5,  cents:316 },
  { name:'5:4',   p:5,  q:4,  cents:386 },
  { name:'4:3',   p:4,  q:3,  cents:498 },
  { name:'45:32', p:45, q:32, cents:590 },
  { name:'3:2',   p:3,  q:2,  cents:702 },
  { name:'8:5',   p:8,  q:5,  cents:814 },
  { name:'5:3',   p:5,  q:3,  cents:884 },
  { name:'9:5',   p:9,  q:5,  cents:1018 },
  { name:'15:8',  p:15, q:8,  cents:1088 },
  { name:'2:1',   p:2,  q:1,  cents:1200 },
  { name:'5:2',   p:5,  q:2,  cents:1586 },
  { name:'3:1',   p:3,  q:1,  cents:1902 },
  { name:'4:1',   p:4,  q:1,  cents:2400 },
];

function harmConsonanceColor(p, q){
  const tenney = p * q;
  if(p===1&&q===1) return '#22c55e';
  if(tenney<=4) return '#60a5fa';
  if(tenney<=16) return '#ffd700';
  if(tenney<=64) return '#f97316';
  return '#a855f7';
}

let harmSelectedIntervals = new Set(); // indices into HARM_INTERVALS
let harmCurrentIntervalIndex = 0; // Current interval being played in sequence
let harmColorMode = false;
let harmArnoldK = 0;
let harmBaseArnold = 0; // store original to reset

let audioCtx = null;
let harmOscillators = [];

function harmGetCtx(){
  if(!audioCtx) audioCtx = new(window.AudioContext||window.webkitAudioContext)();
  if(audioCtx.state==='suspended') audioCtx.resume();
  return audioCtx;
}

function harmStop(){
  harmSyncStop();
  harmOscillators.forEach(o=>{ try{o.stop();}catch(e){} });
  harmOscillators=[];
  ['harmPlayBtn','harmChordBtn','harmArpBtn'].forEach(id=>document.getElementById(id)?.classList.remove('on'));
}

function harmGetBaseHz(){ return parseFloat(document.getElementById('harmBaseHz').value)||220; }
function harmGetVol(){ return parseFloat(document.getElementById('harmVol').value)||0.35; }
function harmGetWave(){ return document.getElementById('harmWave').value||'sine'; }

function harmPlayFreq(hz, duration=1.5){
  const ctx=harmGetCtx();
  const osc=ctx.createOscillator();
  const gain=ctx.createGain();
  osc.connect(gain); gain.connect(ctx.destination);
  osc.type=harmGetWave(); osc.frequency.value=hz;
  const vol=harmGetVol();
  gain.gain.setValueAtTime(vol, ctx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime+duration);
  osc.start(); osc.stop(ctx.currentTime+duration);
  harmOscillators.push(osc);
  return osc;
}

function harmPlay(){
  harmStop();
  const base=harmGetBaseHz();
  const sel=[...harmSelectedIntervals];
  if(!sel.length){ harmPlayFreq(base); return; }
  const iv=HARM_INTERVALS[sel[sel.length-1]];
  harmPlayFreq(base * iv.p / iv.q);
  document.getElementById('harmPlayBtn').classList.add('on');
}

function harmChord(){
  harmStop();
  const base=harmGetBaseHz();
  const sel=[...harmSelectedIntervals];
  if(!sel.length) sel.push(0);
  sel.forEach(idx=>{
    const iv=HARM_INTERVALS[idx];
    harmPlayFreq(base*iv.p/iv.q, 2.0);
  });
  document.getElementById('harmChordBtn').classList.add('on');
}

function harmArp(){
  harmStop();
  const base=harmGetBaseHz();
  const sel=[...harmSelectedIntervals];
  if(!sel.length) sel.push(0);
  const ctx=harmGetCtx();
  sel.forEach((idx,i)=>{
    const iv=HARM_INTERVALS[idx];
    const hz=base*iv.p/iv.q;
    const osc=ctx.createOscillator();
    const gain=ctx.createGain();
    osc.connect(gain); gain.connect(ctx.destination);
    osc.type=harmGetWave(); osc.frequency.value=hz;
    const t=ctx.currentTime+i*0.35;
    gain.gain.setValueAtTime(0, t);
    gain.gain.linearRampToValueAtTime(harmGetVol(), t+0.05);
    gain.gain.exponentialRampToValueAtTime(0.0001, t+0.6);
    osc.start(t); osc.stop(t+0.65);
    harmOscillators.push(osc);
  });
  document.getElementById('harmArpBtn').classList.add('on');
}

function noteFromHz(hz){
  const notes=['C','C#','D','D#','E','F','F#','G','G#','A','A#','B'];
  const n=Math.round(12*Math.log2(hz/440))+69;
  if(n<0||n>127) return '—';
  return notes[n%12]+(Math.floor(n/12)-1);
}

function buildHarmIntervalGrid(){
  const grid=document.getElementById('harmIntervalGrid');
  grid.innerHTML='';
  HARM_INTERVALS.forEach((iv,i)=>{
    const col=harmConsonanceColor(iv.p,iv.q);
    const btn=document.createElement('button');
    btn.className='harm-interval-btn';
    btn.style.borderColor=col+'55';
    btn.style.color=col+'aa';
    btn.innerHTML=iv.name;
    btn.title=`${iv.name} · ${iv.cents}¢ · Tenney height: ${iv.p*iv.q}`;
    btn.onclick=()=>toggleHarmInterval(i,btn,iv,col);
    grid.appendChild(btn);
  });
}

function toggleHarmInterval(i, btn, iv, col){
  if(harmSelectedIntervals.has(i)){
    harmSelectedIntervals.delete(i);
    btn.classList.remove('active');
    btn.style.background='';
    btn.style.color=col+'aa';
  } else {
    harmSelectedIntervals.add(i);
    btn.classList.add('active');
    btn.style.background=col;
    btn.style.color='#000';
  }
  updateHarmDisplay();
  if(harmColorMode) precompute(); // recolor sphere
}

function updateHarmDisplay(){
  const sel=[...harmSelectedIntervals];
  if(!sel.length){
    document.getElementById('harmFreqDisplay').textContent='— Hz';
    document.getElementById('harmNoteDisplay').textContent='— · —';
    return;
  }
  const last=HARM_INTERVALS[sel[sel.length-1]];
  const base=harmGetBaseHz();
  const hz=base*last.p/last.q;
  document.getElementById('harmFreqDisplay').textContent=hz.toFixed(2)+' Hz';
  document.getElementById('harmNoteDisplay').textContent=last.name+' · '+noteFromHz(hz);
}

// Arnold tongue warp: perturb the sphere rendering with K·sin(2π·ratio)
function applyArnoldWarp(){
  harmArnoldK=parseFloat(document.getElementById('harmArnold').value)||0;
  precompute(); // rebuilds precomputed with warp applied
}
function resetArnold(){
  harmArnoldK=0;
  document.getElementById('harmArnold').value=0;
  document.getElementById('harmArnoldVal').textContent='0.00';
  precompute();
}

// Harmonic color mode: sphere hue based on consonance of selected intervals
// The trail hue cycles through the consonance colors of selected intervals
function harmColorForProgress(frac){
  const sel=[...harmSelectedIntervals];
  if(!sel.length) return null;
  // Cycle through selected intervals by position on curve
  const idx=Math.floor(frac*sel.length)%sel.length;
  const iv=HARM_INTERVALS[sel[idx]];
  return harmConsonanceColor(iv.p,iv.q);
}

// Hook into precompute to apply Arnold warp if K>0
const _origPrecompute = precompute;
// We'll re-override toSphere to apply warp
const _origToSphere = toSphere;

function toSphereWarped(t, r){
  let lon = t;
  if(harmArnoldK > 0){
    // Arnold circle map perturbation: lon += K·sin(2π·lon/(2π))
    lon = t + harmArnoldK * Math.sin(t);
  }
  const polar = (t / thetaTotal) * Math.PI;
  const rn = r / (A + 1.2);
  const x = rn * Math.sin(polar) * Math.cos(lon);
  const y = rn * Math.cos(polar);
  const z = rn * Math.sin(polar) * Math.sin(lon);
  return [x, y, z];
}

// Patch toSphere globally
// (call toSphereWarped from precompute instead of toSphere)

// Harmonic color panel — always open in dashboard layout (no toggle needed)

document.getElementById('harmColorMode').addEventListener('change',function(){
  harmColorMode=this.checked;
  document.getElementById('harmModeState').textContent=harmColorMode
    ?'on — sphere colored by harmonic consonance'
    :'off — sphere uses default colors';
  precompute();
});

document.getElementById('harmBaseHz').addEventListener('input',function(){
  document.getElementById('harmBaseHzVal').textContent=this.value+' Hz';
  updateHarmDisplay();
});
document.getElementById('harmVol').addEventListener('input',function(){
  document.getElementById('harmVolVal').textContent=Math.round(this.value*100)+'%';
});
document.getElementById('harmArnold').addEventListener('input',function(){
  document.getElementById('harmArnoldVal').textContent=parseFloat(this.value).toFixed(2);
});

// ═══════════════════════════════════════════════════════════════
//  HARMONIC SYNC ENGINE  (real-time lookahead scheduler)
//
//  Mode A — Gap OFF (pure wave):
//    Pitch follows f(θ) at the current draw cursor in real time.
//    base_hz * (normalized f value 0.5..2.0) gives a melodic contour
//    that literally traces the wave shape as sound.
//
//  Mode B — Gap ON (gap-aware):
//    The gap family at the current draw position determines:
//      • Root pitch: smaller gaps (twin primes g=2) → higher consonant tones
//        Larger gaps → lower, more dissonant, denser harmonics
//      • Timbre: gap families get distinct waveforms
//        g=2: sine (pure)   g=4: triangle   g=6: sawtooth   g≥8: square
//      • Density: common gaps play more quietly; rare large gaps accent
//
//  Scheduler pattern: every animation frame, schedule ~100ms of audio
//  ahead using Web Audio's currentTime as the reference clock.
// ═══════════════════════════════════════════════════════════════

let harmSyncNodes = [];
let harmSyncScheduledUntil = 0;  // ctx.currentTime already scheduled up to
let harmSyncLastPtIdx = -1;      // last drawProgress index scheduled
let harmSyncActive = false;
const LOOKAHEAD_SEC  = 0.12;     // how far ahead to schedule each frame
const NOTE_DUR_BASE  = 0.18;     // base note duration (seconds)

function harmSyncStop(){
  harmSyncActive = false;
  harmSyncScheduledUntil = 0;
  harmSyncLastPtIdx = -1;
  harmSyncNodes.forEach(n=>{ try{n.stop();}catch(e){} });
  harmSyncNodes = [];
  ['harmPlayBtn','harmChordBtn','harmArpBtn'].forEach(id=>{
    const el=document.getElementById(id);
    if(el) el.classList.remove('on');
  });
}

function harmSyncPause(){ harmSyncStop(); }

function harmSyncStart(){
  if(!harmSyncEnabled) return; // respect the on/off toggle
  harmSyncStop();
  const ctx = harmGetCtx();
  if(ctx.state==='suspended') ctx.resume();
  harmSyncActive = true;
  harmSyncScheduledUntil = ctx.currentTime;
  harmSyncLastPtIdx = Math.floor(drawProgress) - 1;
}

// Called every animation frame when harmSyncActive and not paused
function harmSyncTick(){
  if(!harmSyncEnabled || !harmSyncActive || paused) return;
  const ctx = harmGetCtx();
  if(ctx.state==='suspended') ctx.resume();

  const now = ctx.currentTime;
  const scheduleUpTo = now + LOOKAHEAD_SEC;
  if(harmSyncScheduledUntil >= scheduleUpTo) return; // already ahead

  const base   = harmGetBaseHz();
  const vol    = harmGetVol();
  const sel    = [...harmSelectedIntervals];
  const gapOn  = document.getElementById('gapChannelOn')?.checked && gdData;

  // pts per second at current speed (~60fps assumed)
  const ptsPerSec  = speed * 60;
  // time per point in seconds
  const secPerPt   = ptsPerSec > 0 ? 1 / ptsPerSec : 0.001;

  // Figure out which point index corresponds to harmSyncScheduledUntil
  // ctx time → drawProgress offset → point index
  const ptAtScheduled = Math.floor(drawProgress +
    (harmSyncScheduledUntil - now) * ptsPerSec);

  const ptAtHorizon = Math.floor(drawProgress +
    (scheduleUpTo - now) * ptsPerSec);

  // Note density: one note per N points (target ~8–12 notes/sec)
  const noteEveryPts = Math.max(1, Math.floor(ptsPerSec / 10));

  for(let pi = Math.max(harmSyncLastPtIdx + 1, ptAtScheduled);
         pi <= ptAtHorizon && pi < TOTAL_PTS;
         pi += noteEveryPts){

    if(pi <= harmSyncLastPtIdx) continue;
    harmSyncLastPtIdx = pi;

    // Time this note should start in ctx time
    const ptDelta = pi - Math.floor(drawProgress);
    const tNote   = now + ptDelta * secPerPt;
    if(tNote < now - 0.01) continue; // in the past
    if(tNote > now + LOOKAHEAD_SEC + 0.05) break;

    // Get f(θ) value at this point
    const fval = precomputed[pi] ? f((pi / TOTAL_PTS) * thetaTotal) : (A || 1);
    // Normalize fval to 0..1 using precomputed min/max
    const fNorm = (fMax > fMin) ? (fval - fMin) / (fMax - fMin) : 0.5;

    let freq, waveType, noteVol, noteDur;

    if(!gapOn){
      // ── MODE A: Musical harmonic intervals following wave contour ──
      // Each selected interval gets its own "zone" in the wave
      // Within each zone, pitch modulates with f(θ) for expressiveness
      
      if(sel.length > 0){
        // Determine which interval zone we're in
        const zoneSize = TOTAL_PTS / sel.length;
        const zoneIndex = Math.floor(pi / zoneSize) % sel.length;
        const intervalIdx = sel[zoneIndex];
        const interval = HARM_INTERVALS[intervalIdx];
        
        // Base frequency for this interval
        const intervalFreq = base * (interval.p / interval.q);
        
        // Modulate slightly with f(θ) value for musical expression
        // ±12% range (about ±2 semitones) for subtle pitch bending
        const bendRange = 0.12;
        const bend = 1 + (fNorm - 0.5) * bendRange;
        freq = intervalFreq * bend;
        
        // Volume follows wave amplitude for dynamics
        noteVol = vol * (0.5 + 0.5 * fNorm);
        
      } else {
        // No intervals selected - play base frequency with wide range
        const octaveRange = 1.5;
        freq = base * Math.pow(2, (fNorm - 0.5) * octaveRange);
        noteVol = vol * (0.4 + 0.6 * fNorm);
      }

      waveType = harmGetWave();
      noteDur  = NOTE_DUR_BASE;

    } else {
      // ── MODE B: gap-aware ──────────────────────────────────────
      const gapVal = (precomputed[pi] && precomputed[pi][4]) ? precomputed[pi][4] : 2;

      // Pitch: smaller gaps → higher, more consonant
      // g=2 maps to base*2, g=4→base*1.5, g=6→base*1.25, larger→lower
      const gapPitchMap = {
        2:  base * 2,
        4:  base * 1.5,
        6:  base * 1.25,
        8:  base * 1.0,
        10: base * 0.9,
        12: base * 0.8,
        14: base * 0.75,
        18: base * 0.667,
        24: base * 0.6,
        30: base * 0.5,
      };
      freq = gapPitchMap[gapVal] || base * Math.max(0.4, 2 / Math.max(1, Math.log2(gapVal)));

      // Also modulate by f(θ) value within the gap tier (±quarter tone)
      freq *= Math.pow(2, (fNorm - 0.5) * 0.25);

      // Timbre: gap family → waveform
      if      (gapVal <= 2)  waveType = 'sine';
      else if (gapVal <= 4)  waveType = 'triangle';
      else if (gapVal <= 6)  waveType = 'sawtooth';
      else if (gapVal <= 12) waveType = 'square';
      else                   waveType = 'sawtooth';

      // Volume: selected gaps accent loudly; common small gaps quieter
      const isSelected = selectedGaps.has(gapVal);
      const rarity = Math.min(1, Math.log2(gapVal) / 6); // rarer = louder accent
      noteVol  = vol * (isSelected ? 0.9 : 0.25 + rarity * 0.35);

      // Duration: large rare gaps get longer notes (they're dramatic)
      noteDur  = NOTE_DUR_BASE * (1 + rarity * 1.5);
    }

    // Clamp freq to audible range
    freq = Math.max(55, Math.min(4400, freq));

    scheduleNote(ctx, freq, waveType, noteVol, noteDur, tNote);
    harmSyncScheduledUntil = tNote + noteDur;
  }

  // Clean up finished nodes
  if(harmSyncNodes.length > 200){
    harmSyncNodes = harmSyncNodes.filter(n => {
      try { return n.context.state !== 'closed'; } catch(e){ return false; }
    });
  }
}

function scheduleNote(ctx, freq, waveType, vol, dur, startTime){
  const osc  = ctx.createOscillator();
  const gain = ctx.createGain();
  osc.connect(gain); gain.connect(ctx.destination);
  osc.type = waveType;
  osc.frequency.setValueAtTime(freq, startTime);
  gain.gain.setValueAtTime(0.0001, startTime);
  gain.gain.linearRampToValueAtTime(vol, startTime + dur * 0.15);
  gain.gain.exponentialRampToValueAtTime(0.0001, startTime + dur);
  osc.start(startTime);
  osc.stop(startTime + dur + 0.02);
  harmSyncNodes.push(osc);
}

// ═══════════════════════════════════════════════════════════════
//  ZOOM & PAN
// ═══════════════════════════════════════════════════════════════
let zoomLevel = 1.0;
let panX = 0, panY = 0;
let isPan = false, panStartX=0, panStartY=0, panStartPX=0, panStartPY=0;

function setZoom(z){
  zoomLevel = Math.max(0.15, Math.min(10, z));
  document.getElementById('zoomLbl').textContent = zoomLevel.toFixed(1)+'×';
}

document.getElementById('btnZoomIn').addEventListener('click', ()=>{ onUserInteraction(); setZoom(zoomLevel*1.25); });
document.getElementById('btnZoomOut').addEventListener('click', ()=>{ onUserInteraction(); setZoom(zoomLevel/1.25); });
document.getElementById('btnZoomReset').addEventListener('click', ()=>{ setZoom(1); panX=0; panY=0; });

// Mouse-wheel zoom on canvas
pCv.addEventListener('wheel', e=>{
  e.preventDefault();
  setZoom(zoomLevel * (e.deltaY < 0 ? 1.12 : 1/1.12));
}, { passive:false });

// Pinch-to-zoom
let lastPinchDist = null;
pCv.addEventListener('touchstart', e=>{
  if(e.touches.length===2){
    const dx=e.touches[0].clientX-e.touches[1].clientX;
    const dy=e.touches[0].clientY-e.touches[1].clientY;
    lastPinchDist=Math.sqrt(dx*dx+dy*dy);
  }
},{passive:true});
pCv.addEventListener('touchmove', e=>{
  if(e.touches.length===2 && lastPinchDist!==null){
    e.preventDefault();
    const dx=e.touches[0].clientX-e.touches[1].clientX;
    const dy=e.touches[0].clientY-e.touches[1].clientY;
    const d=Math.sqrt(dx*dx+dy*dy);
    setZoom(zoomLevel*(d/lastPinchDist));
    lastPinchDist=d;
  }
},{passive:false});
pCv.addEventListener('touchend',()=>{ lastPinchDist=null; });

// Shift+drag = pan
pCv.addEventListener('mousedown', e=>{
  if(e.shiftKey){
    isPan=true; pCv.style.cursor='move';
    panStartX=e.clientX; panStartY=e.clientY;
    panStartPX=panX; panStartPY=panY;
    e.stopImmediatePropagation();
  }
}, true);
window.addEventListener('mousemove', e=>{
  if(isPan){ panX=panStartPX+(e.clientX-panStartX); panY=panStartPY+(e.clientY-panStartY); }
});
window.addEventListener('mouseup', ()=>{ isPan=false; pCv.style.cursor=''; });

// Override proj() to incorporate zoom + pan
function proj(rx, ry, rz){
  const fov=2.8, scale=fov/(fov-rz);
  const R=pCv.width*0.37*zoomLevel;
  return [pCv.width/2 + rx*R*scale + panX, pCv.height/2 - ry*R*scale + panY];
}

// ═══════════════════════════════════════════════════════════════
//  CART CANVAS — CLICK TO PROBE
// ═══════════════════════════════════════════════════════════════
let cartProbeIdx = -1;   // precomputed index of probed point
let cartProbePx  = -1;   // pixel X of probe line

function cartCanvasClick(e) {
  const rect = cCv.getBoundingClientRect();
  const px   = (e.clientX - rect.left) * (cCv.width / rect.width);
  const {l, r: pr, t: pt, b} = PAD;
  const pw = cCv.width - l - pr;

  if (px < l || px > cCv.width - pr) return; // outside plot area

  const frac = (px - l) / pw;
  const idx  = Math.min(TOTAL_PTS - 1, Math.max(0, Math.round(frac * TOTAL_PTS)));

  // Only probe drawn region
  if (idx > Math.floor(drawProgress)) return;

  cartProbeIdx = idx;
  cartProbePx  = px;
  updateCartProbe();
}

function updateCartProbe() {
  if (cartProbeIdx < 0) return;
  const i   = cartProbeIdx;
  const t   = (i / TOTAL_PTS) * thetaTotal;
  const rv  = f(t);
  const [cx, cy] = cartXY(t, rv);

  // Compute the actual point color — same logic as sphere + renderCart
  const pc = precomputed[i];
  const gap = pc ? pc[4] : null;
  const selCol = (gap !== null && gap !== undefined) ? selectedGaps.get(gap) : undefined;
  const frac = i / TOTAL_PTS;
  const hue  = 295 + frac * 40;
  let ptColor;
  if (selCol) {
    ptColor = selCol;
  } else if (harmColorMode) {
    ptColor = harmColorForProgress(frac) || `hsl(${hue},100%,62%)`;
  } else {
    ptColor = `hsl(${hue},100%,62%)`;
  }

  // ── Draw crosshair overlay ──
  renderCart(); // redraw base
  const ctx = cCtx;
  const {l, r: pr, t: pt, b} = PAD;

  // Vertical probe line — colored to match the point
  ctx.save();
  ctx.strokeStyle = ptColor;
  ctx.lineWidth = 1.5;
  ctx.globalAlpha = 0.6;
  ctx.setLineDash([4, 3]);
  ctx.beginPath();
  ctx.moveTo(cx, pt);
  ctx.lineTo(cx, cCv.height - b);
  ctx.stroke();
  ctx.setLineDash([]);
  ctx.globalAlpha = 1;

  // Dot on curve — colored to match the point
  ctx.beginPath();
  ctx.arc(cx, cy, 6, 0, Math.PI * 2);
  ctx.fillStyle = ptColor;
  ctx.shadowColor = ptColor;
  ctx.shadowBlur = 14;
  ctx.fill();
  // White ring
  ctx.beginPath();
  ctx.arc(cx, cy, 6, 0, Math.PI * 2);
  ctx.strokeStyle = 'rgba(255,255,255,0.75)';
  ctx.lineWidth = 1.5;
  ctx.stroke();
  ctx.shadowBlur = 0;

  // θ label on axis
  ctx.fillStyle = ptColor;
  ctx.font = `${Math.max(8, cCv.width * 0.018)}px JetBrains Mono,monospace`;
  ctx.textAlign = cx > cCv.width * 0.75 ? 'right' : 'left';
  ctx.textBaseline = 'top';
  const thetaLabel = `θ=${(t/Math.PI).toFixed(3)}π`;
  ctx.fillText(thetaLabel, cx + (ctx.textAlign === 'left' ? 6 : -6), cCv.height - b + 2);
  ctx.restore();

  // ── Update scrubber to match ──
  const scrub = document.getElementById('waveScrubber');
  const pctEl = document.getElementById('waveScrubPct');
  if (scrub) {
    scrub.max = TOTAL_PTS;
    scrub.value = i;
    scrub.style.accentColor = ptColor;
  }
  if (pctEl) pctEl.style.color = ptColor;
  if (pctEl) pctEl.textContent = (frac * 100).toFixed(1) + '%';

  // ── Probe detail panel ──
  const panel    = document.getElementById('cartProbePanel');
  const grid     = document.getElementById('cartProbeGrid');
  const gapSec   = document.getElementById('cartProbeGap');
  const gapDet   = document.getElementById('cartProbeGapDetail');
  const sphHint  = document.getElementById('cartProbeSphereHint');

  panel.style.display = 'block';

  const nearPrime = (gdData && !gdData._noRawPrimes && Array.isArray(gdData.primes))
    ? gdData.primes[Math.round((i / TOTAL_PTS) * (gdData.primes.length - 1))]
    : null;

  const cells = [
    { lbl: 'θ',          val: `${(t / Math.PI).toFixed(5)}π` },
    { lbl: 'f(θ)',       val: rv.toFixed(6) },
    { lbl: 'progress',   val: `${(frac * 100).toFixed(2)}%` },
    { lbl: 'pt index',   val: i.toLocaleString() },
    { lbl: 'hue',        val: harmColorMode ? 'harmonic' : `${Math.round(hue)}°` },
    { lbl: 'near prime', val: nearPrime ? nearPrime.toLocaleString() : '—' },
  ];

  grid.innerHTML = cells.map(c =>
    `<div style="font-family:'JetBrains Mono',monospace;">
       <div style="font-size:.42rem;text-transform:uppercase;letter-spacing:.15em;color:rgba(150,175,200,.4);margin-bottom:1px">${c.lbl}</div>
       <div style="font-size:.65rem;color:${ptColor};display:flex;align-items:center;gap:3px">
         ${c.lbl==='hue'||c.lbl==='progress' ? `<span class="probe-color-swatch" style="background:${ptColor}"></span>` : ''}
         ${c.val}
       </div>
     </div>`
  ).join('');

  // Gap section
  if (gap && gdData) {
    gapSec.style.display = 'block';
    const gapIdx = gdData.gaps.indexOf(gap);
    const gCol = gapColor(gap, gapIdx);
    const gp = gdData.gapProducts[gap];
    const share = gp ? ((gp.logProduct / gdData.totalLog) * 100).toFixed(4) : '—';
    const cnt   = gp ? gp.count.toLocaleString() : '—';
    const isSel = selectedGaps.has(gap);
    gapDet.innerHTML =
      `<span style="display:inline-flex;align-items:center;gap:5px">` +
      `<span class="probe-color-swatch" style="background:${gCol};width:12px;height:12px;border-radius:3px;box-shadow:0 0 6px ${gCol}"></span>` +
      `<span style="color:${gCol};font-weight:700">g = ${gap}</span>` +
      `${isSel ? ' <span style="color:rgba(255,255,255,.4);font-size:.5rem">selected</span>' : ''}` +
      `</span>  count = ${cnt}  ·  log-ζ share = ${share}%<br>` +
      `<span style="color:rgba(200,180,120,.5);font-size:.52rem">` +
      `This θ falls in a region where consecutive primes differ by ${gap}. ` +
      `The gap family g=${gap} contributes ${share}% of ζ(${gdData.s.toFixed(2)}) via Euler product.</span>`;
  } else if (gdData && !gap) {
    gapSec.style.display = 'block';
    gapDet.innerHTML = `<span style="color:rgba(200,180,120,.4);font-size:.52rem">No gap data for this point. Run Compute with a larger N to cover this θ range.</span>`;
  } else {
    gapSec.style.display = 'none';
  }

  // Sphere highlight — persistent while panel is open
  highlightSphereProbe(i);
  sphHint.style.display = 'block';
  sphHint.style.color = ptColor;
  sphHint.style.borderColor = ptColor + '44';
  sphHint.textContent = ` Point highlighted on sphere  (${gap ? `gap g=${gap}` : `hue ${Math.round(hue)}°`})`;
}

// ── Wave scrubber handler ──────────────────────────────────────────
function onWaveScrub(val) {
  const maxDrawn = Math.floor(drawProgress);
  let idx = Math.min(maxDrawn, Math.max(0, parseInt(val)));
  if (idx <= 0 && maxDrawn <= 0) return;
  // Clamp to drawn region
  idx = Math.min(idx, maxDrawn > 0 ? maxDrawn - 1 : 0);
  cartProbeIdx = idx;
  cartProbePx  = -1; // not click-based
  updateCartProbe();
}

// Keep scrubber max in sync with drawProgress
function syncScrubberMax() {
  const s = document.getElementById('waveScrubber');
  if (s) {
    s.max = TOTAL_PTS;
    // Only update value if user isn't actively scrubbing and panel is closed
    if (document.getElementById('cartProbePanel').style.display === 'none') {
      s.value = Math.floor(drawProgress);
      const pct = document.getElementById('waveScrubPct');
      if (pct) { pct.textContent = ((drawProgress/TOTAL_PTS)*100).toFixed(1)+'%'; pct.style.color=''; }
    }
  }
}



// Highlight the probed point on the 3D sphere with a persistent bright dot
let probeFlashFrame = 0;
let probePersist = false; // true when probe panel is open
function highlightSphereProbe(idx) {
  probeFlashFrame = 999; // large value — decrements each frame but keepProbeAlive tops it up
  probePersist = true;
  _probeIdx = idx;
}
let _probeIdx = -1;

// Called inside renderSphere() at the very end
function drawProbeFlash(ctx, w) {
  // Probe flash — highlighted cart click / scrub point
  if (probeFlashFrame > 0 && _probeIdx >= 0) {
    if (!probePersist) probeFlashFrame--;
    const pt = precomputed[_probeIdx];
    if (pt) {
      const [rx, ry, rz] = applyM(rotMat, pt[0], pt[1], pt[2]);
      if (rz >= 0) {
        const scale = w * 0.38;
        const cx    = w / 2 + rx * scale;
        const cy    = w / 2 - ry * scale;
        // Compute point color for the sphere dot too
        const pc = precomputed[_probeIdx];
        const gap = pc ? pc[4] : null;
        const selCol = (gap !== null && gap !== undefined) ? selectedGaps.get(gap) : undefined;
        const frac = _probeIdx / TOTAL_PTS;
        const hue  = 295 + frac * 40;
        let dotColor;
        if (selCol) dotColor = selCol;
        else if (harmColorMode) dotColor = harmColorForProgress(frac) || `hsl(${hue},100%,62%)`;
        else dotColor = `hsl(${hue},100%,70%)`;

        const alpha = probePersist ? 1 : (probeFlashFrame / 12);
        ctx.save();
        // Outer glow ring
        ctx.beginPath();
        ctx.arc(cx, cy, 13, 0, Math.PI * 2);
        ctx.fillStyle = dotColor.replace(')', `,${alpha * 0.2})`).replace('hsl(', 'hsla(').replace('rgb(', 'rgba(').replace('#', 'rgba(') ;
        // Simpler approach:
        ctx.fillStyle = `rgba(255,255,255,${alpha * 0.15})`;
        ctx.shadowColor = dotColor;
        ctx.shadowBlur = 28;
        ctx.fill();
        // Inner dot
        ctx.beginPath();
        ctx.arc(cx, cy, 7, 0, Math.PI * 2);
        ctx.fillStyle = dotColor;
        ctx.shadowColor = dotColor;
        ctx.shadowBlur = 22;
        ctx.fill();
        // White center
        ctx.beginPath();
        ctx.arc(cx, cy, 3, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(255,255,255,${alpha * 0.9})`;
        ctx.shadowBlur = 0;
        ctx.fill();
        // Ring outline
        ctx.beginPath();
        ctx.arc(cx, cy, 10, 0, Math.PI * 2);
        ctx.strokeStyle = `rgba(255,255,255,${alpha * 0.6})`;
        ctx.lineWidth = 1.5;
        ctx.stroke();
        ctx.restore();
      }
    }
  }
  // Zeta zero rings overlay (mode 1)
  if (typeof drawZetaZeroRings === 'function') drawZetaZeroRings(ctx, w);
}

// Keep probe dot alive while panel is open
function keepProbeAlive() {
  const panelOpen = document.getElementById('cartProbePanel').style.display !== 'none';
  if (cartProbeIdx >= 0 && panelOpen) {
    probePersist = true;
    probeFlashFrame = 999;
    _probeIdx = cartProbeIdx;
  } else {
    probePersist = false;
  }
  syncScrubberMax();
  setTimeout(keepProbeAlive, 500);
}
setTimeout(keepProbeAlive, 800);

function clearCartProbe() {
  cartProbeIdx = -1;
  cartProbePx  = -1;
  _probeIdx = -1;
  probeFlashFrame = 0;
  probePersist = false;
  document.getElementById('cartProbePanel').style.display = 'none';
  const pct = document.getElementById('waveScrubPct');
  if (pct) { pct.textContent = '0%'; pct.style.color = ''; }
  const scrub = document.getElementById('waveScrubber');
  if (scrub) { scrub.value = 0; scrub.style.accentColor = ''; }
  renderCart(); // remove crosshair
}

cCv.addEventListener('click', cartCanvasClick);
cCv.addEventListener('mousemove', e => {
  // Hover: show crosshair cursor in plot area
  const rect = cCv.getBoundingClientRect();
  const px = (e.clientX - rect.left) * (cCv.width / rect.width);
  const { l, r: pr } = PAD;
  cCv.style.cursor = (px >= l && px <= cCv.width - pr) ? 'crosshair' : 'default';
});

// Show the connection explainer once gap data is available
function showCartGapRelation() {
  const el = document.getElementById('cartGapRelation');
  const txt = document.getElementById('cartGapRelationText');
  if (!gdData || !el) return;
  const N = gdData.primes.length.toLocaleString();
  const s = gdData.s.toFixed(2);
  txt.innerHTML =
    `Each point on this wave corresponds to a specific θ value.<br>` +
    `θ is mapped proportionally to a prime index across ${N} primes.<br>` +
    `The prime gap at that index (p[i+1]−p[i]) assigns a <b>gap family</b>.<br>` +
    `Selecting a gap (g=2 twins, g=4 cousins…) colors the sphere arcs<br>` +
    `where those primes live — revealing how prime structure rhythmically<br>` +
    `deforms the shape of f(θ).<br>` +
    `<span style="color:rgba(232,197,71,.6)">ζ(${s}) = Σ gap contributions via Euler product factorization.</span>`;
  el.style.display = 'block';
}

// ═══════════════════════════════════════════════════════════════
//  KEYBOARD SHORTCUTS
// ═══════════════════════════════════════════════════════════════
document.addEventListener('keydown', e=>{
  if(['INPUT','SELECT','TEXTAREA'].includes(e.target.tagName)) return;
  switch(e.code){
    case 'Space':
      e.preventDefault();
      document.getElementById('btnPlay').click();
      break;
    case 'KeyR':
      autoRotate=!autoRotate;
      document.getElementById('btnAutoRot').classList.toggle('on',autoRotate);
      document.getElementById('btnAutoRot').textContent=autoRotate?'⟳ Rotating...':'⟳ Rotate';
      break;
    case 'KeyS':
      exportSphere4K();
      break;
    case 'KeyF':
      e.preventDefault();
      toggleFullscreen();
      break;
    case 'ArrowUp':
      e.preventDefault(); setZoom(zoomLevel*1.1); break;
    case 'ArrowDown':
      e.preventDefault(); setZoom(zoomLevel/1.1); break;
    case 'ArrowLeft':
      rotMat=mmul(mrotY(-0.08),rotMat); break;
    case 'ArrowRight':
      rotMat=mmul(mrotY(0.08),rotMat); break;
    case 'Escape':
      setZoom(1); panX=0; panY=0;
      rotMat=[1,0,0,0,1,0,0,0,1];
      break;
  }
});


// ═══════════════════════════════════════════════════════════════
//  COORDINATE SYSTEM MODES
// ═══════════════════════════════════════════════════════════════
let coordMode = 'logcylinder';
let coordBlend = 1.0;

const COORD_DESCS = {
  polar:       'Traditional polar/spherical — f(θ) mapped onto sphere surface as latitude × longitude.',
  logcylinder: 'Log-Cylinder: u = ln(θ+1), v = f(θ). Compresses the θ axis logarithmically, revealing fine structure near origin.',
  logspiral:   'Logarithmic Spiral: radius grows as e^(f(θ)/A), angle = θ. Creates an outward unwinding helix on the sphere.',
  poincare:    'Poincaré Disk (hyperbolic): maps sphere points through the unit-disk model. Distances compress toward the boundary.',
  toroidal:    'Toroidal: wraps the curve onto a torus surface. Major radius = f(θ)/A, minor radius tracks inner angle. Reveals periodicity.',
  fibonacci:   'Fibonacci / Phyllotaxis: uses the golden angle φ = 137.5° to distribute points on sphere — same pattern as sunflower seeds.',
  klein:       'Klein Bottle projection: non-orientable surface where the curve self-intersects. Visualises the global topology of the trace.'
};

// Coord mode panel — always open in dashboard layout

document.getElementById('coordModeGrid').addEventListener('click', e=>{
  const btn=e.target.closest('.coord-mode-btn');
  if(!btn) return;
  onUserInteraction();
  coordMode=btn.dataset.mode;
  document.querySelectorAll('.coord-mode-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  document.getElementById('coordModeDesc').textContent=COORD_DESCS[coordMode];
  precompute();
});

document.getElementById('coordBlend').addEventListener('input',function(){
  coordBlend=parseFloat(this.value);
  document.getElementById('coordBlendVal').textContent=Math.round(coordBlend*100)+'%';
  precompute();
});

// Override toSphere to apply coordinate mode transform
function toSphere(t, r){
  // Default polar
  const lon=t;
  const polar=(t/thetaTotal)*Math.PI;
  const rn=r/(A+1.2);
  const px=rn*Math.sin(polar)*Math.cos(lon);
  const py=rn*Math.cos(polar);
  const pz=rn*Math.sin(polar)*Math.sin(lon);
  if(coordMode==='polar' || coordBlend===0) return [px,py,pz];

  let tx=px, ty=py, tz=pz;

  if(coordMode==='logcylinder'){
    const u=Math.log(t+1)/Math.log(thetaTotal+1); // 0→1
    const v=r/(A+1.2);
    const lon2=u*Math.PI*2;
    tx=v*Math.cos(lon2); ty=(u*2-1); tz=v*Math.sin(lon2);
  } else if(coordMode==='logspiral'){
    const spiralR=Math.exp(r/A - 1)*0.6;
    const polar2=(t/thetaTotal)*Math.PI;
    const lon2=t*0.5;
    tx=spiralR*Math.sin(polar2)*Math.cos(lon2);
    ty=spiralR*Math.cos(polar2);
    tz=spiralR*Math.sin(polar2)*Math.sin(lon2);
    // clamp
    const mag=Math.sqrt(tx*tx+ty*ty+tz*tz)||1;
    if(mag>1.2){tx/=mag*0.85;ty/=mag*0.85;tz/=mag*0.85;}
  } else if(coordMode==='poincare'){
    // Project sphere point through Poincaré disk: disk coords from (px,py,pz)
    const denom=1-pz*0.8;
    const dx=(denom>0.01)?px/denom:px;
    const dy=(denom>0.01)?py/denom:py;
    const dr=Math.sqrt(dx*dx+dy*dy)||1;
    const clamp=Math.min(1,dr)/Math.max(dr,0.001);
    tx=dx*clamp; ty=dy*clamp; tz=pz*0.3;
  } else if(coordMode==='toroidal'){
    // Torus: major radius R, minor radius r_m
    // θ_major = t/thetaTotal * 4π (wraps around twice),  θ_minor = f(θ) scaled
    const tMaj = (t / thetaTotal) * Math.PI * 4;
    const tMin = (r / (A + 1.2)) * Math.PI * 2;
    const R = 0.6, rm = 0.35;
    tx = (R + rm * Math.cos(tMin)) * Math.cos(tMaj);
    tz = (R + rm * Math.cos(tMin)) * Math.sin(tMaj);
    ty = rm * Math.sin(tMin);
    // normalise to unit sphere scale
    const mag2 = Math.sqrt(tx*tx+ty*ty+tz*tz)||1;
    tx/=mag2; ty/=mag2; tz/=mag2;
  } else if(coordMode==='fibonacci'){
    // Fibonacci / golden-angle phyllotaxis on sphere
    // i-th point: θ = i * goldenAngle, φ = arccos(1 - 2i/N)
    const goldenAngle = Math.PI * (3 - Math.sqrt(5)); // ≈ 2.3998 rad = 137.5°
    const frac = t / thetaTotal;                       // 0→1
    const phi  = Math.acos(Math.max(-1, Math.min(1, 1 - 2 * frac)));
    const lam  = t * goldenAngle;
    const scale = r / (A + 1.2);
    tx = scale * Math.sin(phi) * Math.cos(lam);
    ty = scale * Math.cos(phi);
    tz = scale * Math.sin(phi) * Math.sin(lam);
  } else if(coordMode==='klein'){
    // Immersed Klein bottle (Figure-8 immersion)
    // u ∈ [0, 2π], v ∈ [0, 2π] — map t → u, r → v
    const u = (t / thetaTotal) * Math.PI * 2;
    const v = (r / (A + 1.2)) * Math.PI * 2;
    const cu=Math.cos(u), su=Math.sin(u), cv=Math.cos(v), sv=Math.sin(v);
    const cu2=Math.cos(u/2), su2=Math.sin(u/2);
    // Standard figure-8 Klein immersion scaled to unit sphere
    tx = (2/15)*cu*(3*cv - 30*su + 90*Math.pow(cu,4)*su - 60*Math.pow(cu,6)*su + 5*cu*cv*su)*0.04;
    ty = (2/15)*su*(3*cv - 3*Math.pow(cu,2)*cv - 48*Math.pow(cu,4)*cv + 48*Math.pow(cu,6)*cv - 60*su + 5*cu*cv*su - 5*Math.pow(cu,3)*cv*su - 80*Math.pow(cu,5)*cv*su + 80*Math.pow(cu,7)*cv*su)*0.04;
    tz = (2/15)*(-3 + 30*Math.pow(cu,2) - 15*Math.pow(cu,4) + 35*Math.pow(cu,5)*su - Math.pow(cu,2)*sv)*0.04 + sv * su2 * 0.3;
    // clamp
    const magK=Math.sqrt(tx*tx+ty*ty+tz*tz)||1;
    if(magK>1.1){tx/=magK;ty/=magK;tz/=magK;}
  }

  // Blend between polar and transformed
  return [
    px*(1-coordBlend)+tx*coordBlend,
    py*(1-coordBlend)+ty*coordBlend,
    pz*(1-coordBlend)+tz*coordBlend
  ];
}

buildHarmIntervalGrid();

// ═══════════════════════════════════════════════════════════════
//  INIT
// ═══════════════════════════════════════════════════════════════

// Track whether user has interacted yet (for intro→loop behavior)
let introPlayed = false;
let userHasInteracted = false;

// Track user interaction (loop stays off — user controls it manually)
function onUserInteraction(){
  if(userHasInteracted) return;
  userHasInteracted = true;
  // Loop stays as-is; user toggles it themselves
}

// Patch preset buttons, play btn, gap chips, etc. to trigger interaction
(function patchInteractionListeners(){
  // We'll intercept at a high level — any click on the page after intro triggers loop
  document.addEventListener('click', function onFirstClick(e){
    // Only activate after intro has started, and ignore the very first auto-play clicks
    if(introPlayed){ onUserInteraction(); }
  }, true);
})();

buildPresets();
hardReset();
loop();

// ── Select initial harmonic intervals (octave, fifth, fourth, major third) ──
function selectInitialHarmIntervals(){
  const grid = document.getElementById('harmIntervalGrid');
  // Select indices: 0 (2:1 octave), 1 (3:2 fifth), 2 (4:3 fourth), 3 (5:4 major third)
  const initialIndices = [0, 1, 2, 3];
  
  initialIndices.forEach(i => {
    if(i < HARM_INTERVALS.length){
      harmSelectedIntervals.add(i);
      const btn = grid.children[i];
      if(btn){
        const iv = HARM_INTERVALS[i];
        const col = harmConsonanceColor(iv.p, iv.q);
        btn.classList.add('active');
        btn.style.background = col;
        btn.style.color = '#000';
      }
    }
  });
  updateHarmDisplay();
}
selectInitialHarmIntervals();

// ── Harmonic Color Mode ON by default ────────────────────────
harmColorMode = true;
document.getElementById('harmColorMode').checked = true;
document.getElementById('harmModeState').textContent = 'on — sphere colored by harmonic consonance';
precompute();

// ── Auto-run zeta decomposition with default N=10M ──────────────
function autoRunZeta(callback){
  const maxN = 500000; // lighter default so it loads fast
  document.getElementById('gdStatus').textContent = 'Auto-computing prime gaps…';
  document.getElementById('gdRunBtn').disabled = true;
  document.getElementById('gdProgress').style.display = 'block';
  document.getElementById('gdProgressFill').style.width = '0%';

  setTimeout(()=>{
    try {
      const s = parseFloat(document.getElementById('gdS').value);
      const progressCb = (cur, max)=>{
        const pct = Math.min(99,(cur/max*100)).toFixed(0);
        document.getElementById('gdProgressFill').style.width = pct+'%';
        document.getElementById('gdStatus').textContent = `Sieving… ${pct}%`;
      };

      gdData = gdCompute(maxN, s, 'all', progressCb);

      document.getElementById('gdProgressFill').style.width = '100%';
      setTimeout(()=>document.getElementById('gdProgress').style.display='none', 400);
      document.getElementById('gdStatus').textContent =
        ` Auto-loaded: ${gdData.primes.length.toLocaleString()} primes · ${gdData.gaps.length} gap families`;

      updateGdStats();
      ['gdExportPNG','gdExportCSV','gdExportTXT'].forEach(id=>document.getElementById(id).style.display='inline-block');
      document.getElementById('gdTabs').style.display='flex';
      document.getElementById('gdStatsGrid').style.display='grid';

      // Show big chart
      const chartArea = document.getElementById('gdChartArea');
      if(chartArea) chartArea.style.display='block';
      const chartBtn = document.getElementById('gdChartScreenshot');
      if(chartBtn) chartBtn.style.display='inline-block';

      buildGapChips();
      gdBuildTable();
      gdBuildConvTable();
      document.getElementById('gdTableSection').style.display='block';
      document.getElementById('gdConvSection').style.display='block';
      // Do NOT call enableGapChannel or hardReset — preserve current draw state
      precompute();
      invalidateCartBuf();
      showCartGapRelation();
      gdResizeCanvas();
      gdDraw();

      // Auto-select gap=2
      if(gdData.gaps.includes(2)){
        const idx = gdData.gaps.indexOf(2);
        const col = gapColor(2, idx);
        selectedGaps.set(2, col);
        document.querySelectorAll('.gap-chip[data-gap="2"]').forEach(chip=>{
          chip.classList.add('selected');
          chip.style.background = col;
          chip.style.borderColor = col;
          chip.style.color = '#000';
        });
        invalidateCartBuf();
      }

    } catch(e){
      document.getElementById('gdStatus').textContent = 'Auto-compute failed: '+e.message;
      console.error(e);
    }
    document.getElementById('gdRunBtn').disabled = false;
    if(callback) callback();
  }, 50);
}



// ═══════════════════════════════════════════════════════════════
//  ACCORDION TOGGLES
// ═══════════════════════════════════════════════════════════════
function toggleHarmAccordion(e){
  const body = document.getElementById('harmAccBody');
  const chev = document.getElementById('harmAccChev');
  const hdr  = document.getElementById('harmAccHeader');
  const open = body.classList.toggle('open');
  chev.classList.toggle('open', open);
  hdr.classList.toggle('open', open);
}

function toggleGapAccordion(e){
  const body = document.getElementById('gapAccBody');
  const chev = document.getElementById('gapAccChev');
  const hdr  = document.getElementById('gapAccHeader');
  const open = body.classList.toggle('open');
  chev.classList.toggle('open', open);
  hdr.classList.toggle('open', open);
}

// ── Harmonic sound on/off toggle ──────────────────────────────
// Separate from the accordion open/close.
// When OFF: stop the sync engine. When ON: start it if playing.
let harmSyncEnabled = false;

function toggleHarmSync(enabled){
  harmSyncEnabled = enabled;
  const state     = document.getElementById('harmSyncState');
  const indicator = document.getElementById('harmModeIndicator');
  if(enabled){
    const gapOn = document.getElementById('gapChannelOn')?.checked && gdData;
    if(state) state.textContent = 'on';
    if(indicator){
      indicator.textContent = gapOn
        ? '· gap-aware · pitch + timbre follow gap families'
        : '· following f(θ) wave';
      indicator.style.color = gapOn ? 'rgba(232,197,71,.75)' : 'rgba(91,156,246,.6)';
    }
    if(!paused) harmSyncStart();
  } else {
    if(state) state.textContent = 'off';
    if(indicator){ indicator.textContent = '· sound is off'; indicator.style.color = 'rgba(91,156,246,.35)'; }
    harmSyncStop();
  }
}

// ═══════════════════════════════════════════════════════════════
//  GAP CHANNEL ON/OFF TOGGLE
// ═══════════════════════════════════════════════════════════════
function toggleGapChannel(enabled){
  const gapWavePanel = document.getElementById('gapWavePanel');
  const state = document.getElementById('gapChannelState');
  const indicator = document.getElementById('harmModeIndicator');
  if(enabled && gdData){
    gapWavePanel.classList.remove('gap-hidden');
    if(state) state.textContent = 'on — gap chart visible';
    if(indicator) indicator.textContent = '· gap-aware · pitch + timbre follow gap families';
    if(indicator) indicator.style.color = 'rgba(232,197,71,.75)';
  } else {
    gapWavePanel.classList.add('gap-hidden');
    if(state) state.textContent = enabled ? 'compute first to enable' : 'off';
    if(enabled && !gdData){
      const cb = document.getElementById('gapChannelOn');
      if(cb) cb.checked = false;
    }
    if(indicator) indicator.textContent = '· following f(θ) wave';
    if(indicator) indicator.style.color = 'rgba(91,156,246,.5)';
  }
  // Resize gap canvas without touching sphere/cart canvas (avoids reset)
  gdResizeCanvas();
  invalidateCartBuf();
  if(harmSyncEnabled && !paused){
    harmSyncStart();
  }
}

// Called after gap data loads — auto-enable the channel toggle
function enableGapChannel(){
  const cb = document.getElementById('gapChannelOn');
  if(cb){ cb.checked = true; toggleGapChannel(true); }
  // Update mode indicator
  const indicator = document.getElementById('harmModeIndicator');
  if(indicator){
    indicator.textContent = '· gap-aware · pitch + timbre follow gap families';
    indicator.style.color = 'rgba(232,197,71,.75)';
  }
}

// ── Welcome / intro sequence ──────────────────────────────────
// 1. Play the wave once (no loop) as a welcoming animation + sound
// 2. After wave finishes, mark intro done → next user action enables loop
function startIntro(){
  // Ensure NOT looping for intro
  looping = false;
  const cbLoop = document.getElementById('cbLoop');
  if(cbLoop) cbLoop.checked = false;
  document.getElementById('loopState').textContent = 'off — stops at end';

  // Set a welcoming speed (not too fast, not too slow)
  setSpeed(12);

  // Auto-rotate OFF by default on first load
  autoRotate = false;
  const rotBtn = document.getElementById('btnAutoRot');
  if(rotBtn){ rotBtn.classList.remove('on'); rotBtn.textContent = '⟳ Rotate'; }

  // Start playback immediately — gap decomposition runs only when user clicks Compute
  introPlayed = true;
  hardReset();
  paused = false;
  const playBtn = document.getElementById('btnPlay');
  if(playBtn){ playBtn.textContent = '⏸ Pause'; playBtn.classList.add('on'); }

  // Play the welcome harmonic sweep (single pass)
  harmSyncStart();
}

// Small delay so DOM is fully ready
setTimeout(startIntro, 300);

// ═══════════════════════════════════════════════════════════════
//  RIEMANN ZETA MODULE — ζ(½+it) Critical Line
// ═══════════════════════════════════════════════════════════════

// ── Known non-trivial zeros of ζ(½+it), t-values (first 60) ──
const ZETA_ZEROS = [
  14.1347251417,20.0239241220,25.0108575801,30.4248761259,32.9350615877,
  37.5861781588,40.9187190121,43.3270732810,48.0051508812,49.7738324777,
  52.9703214777,56.4462476971,59.3470440026,60.8317785246,65.1125440481,
  67.0798105294,69.5464017112,72.0671576745,75.7046906990,77.1448400689,
  79.3373750202,82.9103808541,84.7354929805,87.4252746132,88.8091112076,
  92.4918992706,94.6513440405,95.8706342282,98.8311942182,101.317851006,
  103.725538040,105.446623052,107.168611184,111.029535543,111.874659177,
  114.320220915,116.226680321,118.790782866,121.370125002,122.946829294,
  124.256818554,127.516683880,129.578704200,131.087688531,133.497737203,
  134.756509753,138.116042055,139.736208952,141.123707404,143.111845808,
  146.000982487,147.422765343,150.053527420,150.925257612,153.024693811,
  156.112909294,157.597591818,158.849988477,161.188964138,163.030709687
];

// ── Canvas references ──
const zetaCv   = document.getElementById('zetaCanvas');
const zetaCtx  = zetaCv.getContext('2d');
const zeta3dCv = document.getElementById('zeta3dCanvas');
const zeta3dCtx= zeta3dCv.getContext('2d');

// ── State ──
let zetaMode      = 0;       // 0=spiral, 1=sphere zeros, 2=3D curve
let zetaData      = null;    // {re[], im[], tArr[], tMax, N}
let zetaDrawProg  = 0;       // animation progress index
let zetaAnimId    = null;
let zetaZerosInRange = [];
let zetaSphereZerosActive = false;
// 3D canvas drag
let z3Drag=false, z3LastX=0, z3LastY=0;
let z3RotMat=[1,0,0, 0,1,0, 0,0,1];

// ── Riemann-Siegel approximation of ζ(½+it) ──
// Accurate for moderate t; uses the Euler-Maclaurin main sum + correction term
function zetaRS(t) {
  if (t < 2) return { re: 1, im: 0 };
  // Main sum: ζ ≈ Σ_{n=1}^{N} n^{-½-it} + χ·Σ_{n=1}^{N} n^{-½+it}
  const N = Math.max(3, Math.floor(Math.sqrt(t / (2 * Math.PI))));
  let re = 0, im = 0;
  for (let n = 1; n <= N; n++) {
    const logN = Math.log(n);
    const mag  = Math.pow(n, -0.5);        // n^{-½}
    const phase = -t * logN;               // -t·ln(n)
    re += mag * Math.cos(phase);
    im += mag * Math.sin(phase);
  }
  // Functional equation factor χ(½+it) = π^{it} · Γ((¼-it/2)/½) / Γ((¼+it/2)/½) ... approximated:
  // χ ≈ (2π/t)^{½} · e^{i(t - π/8 - π/4)} using Stirling
  const logFactor = 0.5 * Math.log(t / (2 * Math.PI));
  const chiMag    = Math.exp(logFactor); // magnitude of χ
  const chiPhase  = t * Math.log(t / (2 * Math.PI * Math.E)) - Math.PI / 8;
  // conjugate sum Σ n^{-½+it}
  let re2 = 0, im2 = 0;
  for (let n = 1; n <= N; n++) {
    const mag  = Math.pow(n, -0.5);
    const phase = t * Math.log(n);
    re2 += mag * Math.cos(phase);
    im2 += mag * Math.sin(phase);
  }
  // χ · conj_sum
  const cRe = chiMag * (Math.cos(chiPhase) * re2 - Math.sin(chiPhase) * im2);
  const cIm = chiMag * (Math.cos(chiPhase) * im2 + Math.sin(chiPhase) * re2);
  return { re: re + cRe, im: im + cIm };
}

// ── Compute all points ──
function zetaCompute() {
  const tMax = parseFloat(document.getElementById('zetaTMax').value);
  const N    = parseInt(document.getElementById('zetaRes').value);
  const re   = new Float32Array(N);
  const im   = new Float32Array(N);
  const tArr = new Float32Array(N);

  document.getElementById('zetaStatus').textContent = 'Computing ζ(½+it)…';
  for (let i = 0; i < N; i++) {
    const t    = (i / (N - 1)) * tMax;
    tArr[i]    = t;
    const z    = zetaRS(t);
    re[i]      = z.re;
    im[i]      = z.im;
  }

  // Find zeros in range (from known list)
  zetaZerosInRange = ZETA_ZEROS.filter(z => z <= tMax);
  zetaData = { re, im, tArr, tMax, N };

  const zCount = zetaZerosInRange.length;
  document.getElementById('zetaStatus').textContent =
    `ζ(½+it) · t ∈ [0, ${tMax}] · ${N} pts · ${zCount} zero${zCount!==1?'s':''} in range`;
  document.getElementById('zetaScreenshotBtn').style.display = 'inline-block';

  // Zero list
  const zeroList = document.getElementById('zetaZeroList');
  const zeroItems = document.getElementById('zetaZeroItems');
  if (zetaZerosInRange.length > 0) {
    zeroList.style.display = 'block';
    zeroItems.innerHTML = zetaZerosInRange.map((t, i) =>
      `<span style="background:rgba(180,100,255,.12);border:1px solid rgba(180,100,255,.2);
        border-radius:3px;padding:1px 6px;cursor:pointer;font-size:.52rem"
        title="t = ${t.toFixed(6)}" onclick="zetaHighlightZero(${t})">
        t<sub>${i+1}</sub>=${t.toFixed(4)}
      </span>`
    ).join('');
  }

  // Sphere zeros
  zetaSphereZerosActive = true;
  document.getElementById('zetaSphereZeroCount').textContent =
    `${zCount} zero rings active on sphere · toggle mode to see them.`;

  return zetaData;
}

// ── Main run ──
function zetaRun() {
  if (zetaAnimId) { cancelAnimationFrame(zetaAnimId); zetaAnimId = null; }
  zetaSizeCanvases();
  zetaCompute();
  zetaDrawProg = 0;
  zetaAnimate();
}

function zetaSizeCanvases() {
  const wrap = document.getElementById('zetaAccBody');
  const W = Math.max(320, wrap.clientWidth - 28);
  zetaCv.width   = W;
  zetaCv.height  = Math.round(W * 0.55);
  zeta3dCv.width  = W;
  zeta3dCv.height = Math.round(W * 0.55);
}

// Export zeta spiral at 4K resolution
function zetaExport4K() {
  if (!zetaData) { alert('Run Compute first.'); return; }
  // Save live canvas size
  const liveW = zetaCv.width, liveH = zetaCv.height;
  // Render at 4K
  const EXP_W = 3840, EXP_H = Math.round(3840 * 0.55);
  zetaCv.width  = EXP_W;
  zetaCv.height = EXP_H;
  zetaDrawProg  = zetaData.N;  // draw fully
  zetaDrawSpiral();
  const a = document.createElement('a');
  a.download = `zeta_spiral_4K_T${zetaData.tMax}.png`;
  a.href = zetaCv.toDataURL('image/png');
  a.click();
  // Restore live canvas
  zetaCv.width  = liveW;
  zetaCv.height = liveH;
  zetaDrawSpiral();
}

// ── Animated draw ──
function zetaAnimate() {
  if (!zetaData) return;
  if (zetaMode === 0) zetaDrawSpiral();
  else if (zetaMode === 2) zetaDraw3D();

  const syncWave = document.getElementById('zetaSyncWave').checked;
  const targetProg = syncWave
    ? Math.floor((drawProgress / TOTAL_PTS) * zetaData.N)
    : zetaData.N;

  if (zetaDrawProg < targetProg) {
    zetaDrawProg = Math.min(zetaDrawProg + Math.max(4, Math.floor(zetaData.N / 300)), targetProg);
    zetaAnimId = requestAnimationFrame(zetaAnimate);
  } else {
    zetaDrawProg = targetProg;
    if (zetaMode === 0) zetaDrawSpiral();
    else if (zetaMode === 2) zetaDraw3D();
  }
}

// ── Mode 0: 2D Spiral ──
function zetaDrawSpiral() {
  if (!zetaData) return;
  const { re, im, N, tMax } = zetaData;
  const w = zetaCv.width, h = zetaCv.height;
  const pad = 32;

  // Compute axis bounds
  let minRe=-3, maxRe=3.5, minIm=-2.5, maxIm=2.5;
  for (let i = 0; i < N; i++) {
    if (re[i] < minRe) minRe = re[i];
    if (re[i] > maxRe) maxRe = re[i];
    if (im[i] < minIm) minIm = im[i];
    if (im[i] > maxIm) maxIm = im[i];
  }
  const margin = 0.3;
  minRe -= margin; maxRe += margin; minIm -= margin; maxIm += margin;

  // Keep aspect ratio square-ish
  const reRange = maxRe - minRe, imRange = maxIm - minIm;
  const scale = Math.min((w - pad*2) / reRange, (h - pad*2) / imRange);
  const cx = w/2, cy = h/2;
  const midRe = (minRe + maxRe)/2, midIm = (minIm + maxIm)/2;

  const toX = r => cx + (r - midRe) * scale;
  const toY = i => cy - (i - midIm) * scale;

  // Background
  zetaCtx.fillStyle = '#070510';
  zetaCtx.fillRect(0, 0, w, h);

  // Grid
  zetaCtx.strokeStyle = 'rgba(180,100,255,.07)';
  zetaCtx.lineWidth = 0.5;
  for (let v = Math.ceil(minRe); v <= Math.floor(maxRe); v++) {
    zetaCtx.beginPath(); zetaCtx.moveTo(toX(v), pad); zetaCtx.lineTo(toX(v), h-pad); zetaCtx.stroke();
  }
  for (let v = Math.ceil(minIm); v <= Math.floor(maxIm); v++) {
    zetaCtx.beginPath(); zetaCtx.moveTo(pad, toY(v)); zetaCtx.lineTo(w-pad, toY(v)); zetaCtx.stroke();
  }

  // Axes
  zetaCtx.strokeStyle = 'rgba(180,100,255,.22)';
  zetaCtx.lineWidth = 1;
  zetaCtx.beginPath(); zetaCtx.moveTo(toX(0), pad); zetaCtx.lineTo(toX(0), h-pad); zetaCtx.stroke();
  zetaCtx.beginPath(); zetaCtx.moveTo(pad, toY(0)); zetaCtx.lineTo(w-pad, toY(0)); zetaCtx.stroke();

  // Axis labels
  zetaCtx.fillStyle = 'rgba(180,100,255,.45)';
  zetaCtx.font = `${Math.max(8, w * 0.022)}px JetBrains Mono,monospace`;
  zetaCtx.textAlign = 'center';
  zetaCtx.fillText('Re ζ(½+it)', w - 60, toY(0) + 14);
  zetaCtx.save(); zetaCtx.translate(toX(0) - 14, h/2);
  zetaCtx.rotate(-Math.PI/2);
  zetaCtx.fillText('Im ζ(½+it)', 0, 0);
  zetaCtx.restore();

  // Title
  zetaCtx.fillStyle = 'rgba(200,140,255,.6)';
  zetaCtx.font = `bold ${Math.max(9, w * 0.025)}px JetBrains Mono,monospace`;
  zetaCtx.textAlign = 'left';
  zetaCtx.fillText(`Polar graph of ζ(½+it),  t ∈ [0, ${zetaData.tMax}]`, pad, 18);

  // Curve — color by t progress (purple→cyan→magenta spiral)
  const end = Math.min(zetaDrawProg, N - 1);
  zetaCtx.lineWidth = 1.5;
  zetaCtx.shadowBlur = 4;
  for (let i = 1; i <= end; i++) {
    const frac = i / N;
    const hue  = 270 + frac * 120; // purple → cyan
    const lum  = 55 + frac * 15;
    zetaCtx.strokeStyle = `hsl(${hue},90%,${lum}%)`;
    zetaCtx.shadowColor = `hsl(${hue},90%,${lum}%)`;
    zetaCtx.beginPath();
    zetaCtx.moveTo(toX(re[i-1]), toY(im[i-1]));
    zetaCtx.lineTo(toX(re[i]),   toY(im[i]));
    zetaCtx.stroke();
  }
  zetaCtx.shadowBlur = 0;

  // Origin marker
  zetaCtx.beginPath();
  zetaCtx.arc(toX(0), toY(0), 4, 0, Math.PI*2);
  zetaCtx.fillStyle = 'rgba(255,255,255,.3)';
  zetaCtx.fill();

  // Zero markers — glowing white dots at each known zero location
  zetaZerosInRange.forEach((tz, idx) => {
    const zi   = Math.round((tz / zetaData.tMax) * (N - 1));
    if (zi > end) return;
    const zx   = toX(re[zi]);
    const zy   = toY(im[zi]);
    zetaCtx.save();
    zetaCtx.beginPath();
    zetaCtx.arc(zx, zy, 6, 0, Math.PI*2);
    zetaCtx.fillStyle   = 'rgba(255,255,255,.9)';
    zetaCtx.shadowColor = '#fff';
    zetaCtx.shadowBlur  = 14;
    zetaCtx.fill();
    zetaCtx.strokeStyle = 'rgba(200,140,255,.9)';
    zetaCtx.lineWidth   = 1.5;
    zetaCtx.stroke();
    zetaCtx.restore();
    // Label
    zetaCtx.fillStyle  = 'rgba(220,180,255,.8)';
    zetaCtx.font       = `${Math.max(7,w*0.017)}px JetBrains Mono,monospace`;
    zetaCtx.textAlign  = 'left';
    zetaCtx.fillText(`t${idx+1}`, zx + 8, zy - 2);
  });

  // Moving cursor dot
  if (end < N - 1) {
    zetaCtx.beginPath();
    zetaCtx.arc(toX(re[end]), toY(im[end]), 5, 0, Math.PI*2);
    zetaCtx.fillStyle   = '#fff';
    zetaCtx.shadowColor = '#ccc';
    zetaCtx.shadowBlur  = 12;
    zetaCtx.fill();
    zetaCtx.shadowBlur  = 0;
  }
}

// ── Highlight a specific zero on the spiral (click on chip) ──
let zetaHighlightT = null;
function zetaHighlightZero(tz) {
  zetaHighlightT = tz;
  if (zetaData) {
    zetaDrawProg = Math.round((tz / zetaData.tMax) * zetaData.N);
    zetaDrawSpiral();
    // Flash the zero
    const { re, im, tMax, N } = zetaData;
    const zi = Math.round((tz / tMax) * (N - 1));
    setTimeout(() => { zetaDrawProg = N; zetaDrawSpiral(); }, 600);
  }
}

// ── Mode 2: 3D Curve ──
// Maps ζ(½+it) → 3D: longitude = t*2π/tMax, lat = atan2(im,re), r = |ζ|
function zetaDraw3D() {
  if (!zetaData) return;
  const { re, im, N, tMax } = zetaData;
  const w = zeta3dCv.width, h = zeta3dCv.height;

  zeta3dCtx.fillStyle = '#070510';
  zeta3dCtx.fillRect(0, 0, w, h);

  const end = Math.min(zetaDrawProg, N - 1);
  const R = w * 0.3;  // sphere radius in px

  // Project 3D point via z3RotMat
  const proj3 = (x, y, z) => {
    const rx = z3RotMat[0]*x + z3RotMat[1]*y + z3RotMat[2]*z;
    const ry = z3RotMat[3]*x + z3RotMat[4]*y + z3RotMat[5]*z;
    const rz = z3RotMat[6]*x + z3RotMat[7]*y + z3RotMat[8]*z;
    const fov = 3, s = fov / (fov - rz * 0.8);
    return [w/2 + rx * R * s, h/2 - ry * R * s, rz];
  };

  // Wire sphere
  zeta3dCtx.strokeStyle = 'rgba(180,100,255,.06)';
  zeta3dCtx.lineWidth = 0.5;
  for (let a = 0; a < Math.PI*2; a += Math.PI/6) {
    zeta3dCtx.beginPath();
    for (let b = 0; b <= Math.PI*2; b += 0.1) {
      const [px, py] = proj3(Math.cos(a)*Math.cos(b), Math.sin(b), Math.sin(a)*Math.cos(b));
      b===0 ? zeta3dCtx.moveTo(px,py) : zeta3dCtx.lineTo(px,py);
    }
    zeta3dCtx.stroke();
  }
  for (let lat = -Math.PI/2; lat <= Math.PI/2; lat += Math.PI/6) {
    zeta3dCtx.beginPath();
    for (let lon = 0; lon <= Math.PI*2; lon += 0.1) {
      const [px,py] = proj3(Math.cos(lat)*Math.cos(lon),Math.sin(lat),Math.cos(lat)*Math.sin(lon));
      lon===0 ? zeta3dCtx.moveTo(px,py) : zeta3dCtx.lineTo(px,py);
    }
    zeta3dCtx.stroke();
  }

  // Build 3D path points
  const pts = [];
  for (let i = 0; i <= end; i++) {
    const t   = (i / (N - 1)) * tMax;
    const mag = Math.sqrt(re[i]*re[i] + im[i]*im[i]);
    const rn  = Math.min(1.5, mag / 4);  // normalized radius
    const lon = (t / tMax) * Math.PI * 6; // multiple loops
    const lat = Math.atan2(im[i], re[i]) * 0.4; // latitude from phase
    const x   = rn * Math.cos(lat) * Math.cos(lon);
    const y   = rn * Math.sin(lat);
    const z   = rn * Math.cos(lat) * Math.sin(lon);
    pts.push({ x, y, z, frac: i/N });
  }

  // Sort by z for painter's algo
  const segs = [];
  for (let i = 1; i < pts.length; i++) {
    const [ax,ay,az] = proj3(pts[i-1].x, pts[i-1].y, pts[i-1].z);
    const [bx,by,bz] = proj3(pts[i].x, pts[i].y, pts[i].z);
    segs.push({ ax,ay,bx,by,z:(az+bz)/2, frac:pts[i].frac });
  }
  segs.sort((a,b)=>a.z-b.z);

  segs.forEach(({ ax, ay, bx, by, z, frac }) => {
    const hue = 270 + frac * 120;
    const alpha = 0.4 + z * 0.5;
    zeta3dCtx.strokeStyle = `hsla(${hue},90%,65%,${Math.max(0.1,alpha)})`;
    zeta3dCtx.lineWidth = 1.5 + z * 0.5;
    zeta3dCtx.shadowColor = `hsl(${hue},90%,65%)`;
    zeta3dCtx.shadowBlur = 3;
    zeta3dCtx.beginPath();
    zeta3dCtx.moveTo(ax, ay);
    zeta3dCtx.lineTo(bx, by);
    zeta3dCtx.stroke();
  });
  zeta3dCtx.shadowBlur = 0;

  // Zero rings on 3D sphere
  zetaZerosInRange.forEach(tz => {
    const zi = Math.round((tz / tMax) * (N - 1));
    if (zi > end) return;
    const t   = tz;
    const mag = Math.sqrt(re[zi]*re[zi] + im[zi]*im[zi]);
    const rn  = Math.min(1.5, mag / 4);
    const lon = (t / tMax) * Math.PI * 6;
    const lat = Math.atan2(im[zi], re[zi]) * 0.4;
    const x   = rn * Math.cos(lat) * Math.cos(lon);
    const y   = rn * Math.sin(lat);
    const z   = rn * Math.cos(lat) * Math.sin(lon);
    const [px, py, pz] = proj3(x, y, z);
    if (pz < -0.5) return;
    zeta3dCtx.beginPath();
    zeta3dCtx.arc(px, py, 6, 0, Math.PI*2);
    zeta3dCtx.fillStyle   = 'rgba(255,255,255,.85)';
    zeta3dCtx.shadowColor = '#fff';
    zeta3dCtx.shadowBlur  = 16;
    zeta3dCtx.fill();
    zeta3dCtx.shadowBlur  = 0;
  });

  // Labels
  zeta3dCtx.fillStyle  = 'rgba(200,140,255,.5)';
  zeta3dCtx.font       = `${Math.max(8,w*0.022)}px JetBrains Mono,monospace`;
  zeta3dCtx.textAlign  = 'left';
  zeta3dCtx.fillText('ζ(½+it) — 3D tube', 12, 18);
}

// ── Sphere overlay: draw zero rings on the main polar sphere ──
// Called from renderSphere() — draws glowing latitude rings at zero t-values
function drawZetaZeroRings(ctx, w) {
  if (!zetaSphereZerosActive || zetaMode !== 1) return;
  if (!zetaZerosInRange.length) return;

  const end = Math.floor(drawProgress);
  zetaZerosInRange.forEach((tz, idx) => {
    // Map t → point index
    const pIdx = Math.round((tz / (zetaData ? zetaData.tMax : 50)) * TOTAL_PTS);
    if (pIdx > end || pIdx >= TOTAL_PTS) return;
    if (!precomputed[pIdx]) return;

    const [x0, y0, z0] = precomputed[pIdx];
    // Draw a small ring of dots around the sphere at this point's latitude
    const lat = Math.asin(Math.max(-1, Math.min(1, y0)));
    const ringR = Math.cos(lat) * 1.02;

    ctx.save();
    for (let a = 0; a < Math.PI * 2; a += Math.PI / 18) {
      const rx = ringR * Math.cos(a);
      const rz_w = ringR * Math.sin(a);
      const [rrx, rry, rrz] = applyM(rotMat, rx, lat > 0 ? Math.abs(y0)*1.02 : y0*1.02, rz_w);
      if (rrz < -0.1) continue;
      const scale = w * 0.38;
      const px = w/2 + rrx * scale;
      const py = w/2 - rry * scale;
      const alpha = 0.3 + rrz * 0.5;
      ctx.beginPath();
      ctx.arc(px, py, 2.5, 0, Math.PI*2);
      ctx.fillStyle   = `rgba(200,140,255,${Math.max(0.1, alpha)})`;
      ctx.shadowColor = 'rgba(200,140,255,.8)';
      ctx.shadowBlur  = 8;
      ctx.fill();
    }
    ctx.restore();

    // Label at the point itself
    const [rrx, rry, rrz] = applyM(rotMat, x0, y0, z0);
    if (rrz > 0) {
      const scale = w * 0.38;
      const px = w/2 + rrx * scale;
      const py = w/2 - rry * scale;
      ctx.fillStyle  = 'rgba(220,180,255,.7)';
      ctx.font       = `${Math.max(7, w*0.015)}px JetBrains Mono,monospace`;
      ctx.textAlign  = 'center';
      ctx.fillText(`t${idx+1}`, px, py - 10);
    }
  });
}

// ── Mode switcher ──
function zetaSwitchMode(mode) {
  zetaMode = mode;
  [0,1,2].forEach(i => {
    document.getElementById(`zetaTab${i}`).classList.toggle('active', i===mode);
    document.getElementById(`zetaMode${i}`).style.display = i===mode ? '' : 'none';
  });
  if (mode === 0 && zetaData) zetaDrawSpiral();
  if (mode === 2 && zetaData) zetaDraw3D();
}

// ── Accordion toggle ──
function toggleZetaAccordion(e) {
  const body = document.getElementById('zetaAccBody');
  const chev = document.getElementById('zetaAccChev');
  const hdr  = document.getElementById('zetaAccHeader');
  const open = body.classList.toggle('open');
  chev.classList.toggle('open', open);
  hdr.classList.toggle('open', open);
  if (open && zetaData) zetaSizeCanvases();
}

// ── Slider bindings ──
document.getElementById('zetaTMax').addEventListener('input', function() {
  document.getElementById('zetaTMaxVal').textContent = this.value;
});
document.getElementById('zetaRes').addEventListener('input', function() {
  document.getElementById('zetaResVal').textContent = parseInt(this.value).toLocaleString();
});

// ── 3D canvas drag rotation ──
zeta3dCv.addEventListener('mousedown', e => {
  z3Drag=true; z3LastX=e.clientX; z3LastY=e.clientY;
});
window.addEventListener('mousemove', e => {
  if (!z3Drag) return;
  const dx=(e.clientX-z3LastX)*0.012, dy=(e.clientY-z3LastY)*0.012;
  z3LastX=e.clientX; z3LastY=e.clientY;
  const Rx=[1,0,0, 0,Math.cos(dy),-Math.sin(dy), 0,Math.sin(dy),Math.cos(dy)];
  const Ry=[Math.cos(dx),0,Math.sin(dx), 0,1,0, -Math.sin(dx),0,Math.cos(dx)];
  z3RotMat = mmul(Ry, mmul(Rx, z3RotMat));
  if (zetaData) zetaDraw3D();
});
window.addEventListener('mouseup', () => { z3Drag=false; });

// ── Keep zeta in sync with wave animation ──
// Re-render spiral/3D every N frames when sync is on
let _zetaSyncFrame = 0;
const _origLoop = loop;
// (We can't easily patch loop, so we use a separate rAF watcher)
function zetaSyncTick() {
  requestAnimationFrame(zetaSyncTick);
  if (!zetaData) return;
  const syncWave = document.getElementById('zetaSyncWave').checked;
  if (!syncWave) return;
  _zetaSyncFrame++;
  if (_zetaSyncFrame % 4 !== 0) return; // every 4 frames
  const target = Math.floor((drawProgress / TOTAL_PTS) * zetaData.N);
  if (target !== zetaDrawProg) {
    zetaDrawProg = target;
    if (zetaMode === 0) zetaDrawSpiral();
    else if (zetaMode === 2) zetaDraw3D();
  }
}
zetaSyncTick();

// ═══════════════════════════════════════════════════════════════
//  GIF / WebM ROTATION EXPORT ENGINE
// ═══════════════════════════════════════════════════════════════

(function gifExportModule() {
"use strict";

// ─── state ────────────────────────────────────────────────────
let gifAxis      = 'Y';
let gifExporting = false;
let gifAborted   = false;
let _mrChunks    = [];
let _mrec        = null;

// ─── modal open/close ─────────────────────────────────────────
function openModal(){
  document.getElementById('gifModalBackdrop').classList.add('open');
  setStatus(''); resetProg();
}
function closeModal(){
  if(gifExporting) return;
  document.getElementById('gifModalBackdrop').classList.remove('open');
}

document.getElementById('btnExportGif').addEventListener('click', openModal);
document.getElementById('gifModalClose').addEventListener('click', closeModal);
document.getElementById('gifModalBackdrop').addEventListener('click', e=>{
  if(e.target===document.getElementById('gifModalBackdrop') && !gifExporting) closeModal();
});
document.getElementById('gifCancelBtn').addEventListener('click', ()=>{
  if(gifExporting){
    gifAborted = true;
    try{ if(_mrec && _mrec.state!=='inactive') _mrec.stop(); }catch(e){}
  }
  closeModal();
});

// ─── axis buttons ─────────────────────────────────────────────
document.querySelectorAll('.gif-axis-btn').forEach(btn=>{
  btn.addEventListener('click', ()=>{
    document.querySelectorAll('.gif-axis-btn').forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    gifAxis = btn.dataset.axis;
  });
});

// hide/show GIF quality when format changes
document.getElementById('gifFormat').addEventListener('change', function(){
  document.getElementById('gifQualityWrap').style.display = this.value==='gif' ? '' : 'none';
});

// ─── UI helpers ───────────────────────────────────────────────
function setStatus(msg){ document.getElementById('gifStatus').textContent = msg; }
function setProg(pct){
  document.getElementById('gifProgressBar').style.width = Math.min(100,pct).toFixed(1)+'%';
}
function showProg(){ document.getElementById('gifProgressWrap').style.display = ''; }
function resetProg(){ setProg(0); document.getElementById('gifProgressWrap').style.display='none'; }
function setGoBtn(dis, lbl){ const b=document.getElementById('gifGoBtn'); b.disabled=dis; b.textContent=lbl; }
function yieldFrame(){ return new Promise(r=>setTimeout(r,0)); }

// ─── main export dispatcher ───────────────────────────────────
document.getElementById('gifGoBtn').addEventListener('click', async()=>{
  if(gifExporting) return;
  gifExporting = true;
  gifAborted   = false;

  const format    = document.getElementById('gifFormat').value;
  const res       = parseInt(document.getElementById('gifRes').value);
  const dur       = Math.max(2, parseInt(document.getElementById('gifDuration').value)||6);
  const fps       = parseInt(document.getElementById('gifFps').value);
  const quality   = Math.max(1, parseInt(document.getElementById('gifQuality').value)||4);
  const rotDeg    = parseFloat(document.getElementById('gifRotSpeed').value)||2;
  const axis      = gifAxis;

  setGoBtn(true,'⏳ Rendering…');
  showProg();
  setStatus(`Preparing ${res}×${res} @ ${fps}fps…`);

  try {
    if(format==='webm') await doWebM(res, fps, dur, rotDeg, axis);
    else                await doGIF (res, fps, dur, rotDeg, axis, quality);
  } catch(err){
    if(!gifAborted) setStatus('Error: '+err.message);
    console.error(err);
  } finally {
    gifExporting = false;
    setGoBtn(false,'▶ Export');
    if(!gifAborted) setStatus('✓ Done — check your downloads!');
  }
});

// ═══════════════════════════════════════════════════════════════
//  FRAME RENDERER — draws one sphere frame into a fresh canvas
// ═══════════════════════════════════════════════════════════════
function buildFrame(res, mat){
  const cv = document.createElement('canvas');
  cv.width = cv.height = res;
  const ctx = cv.getContext('2d');
  const mid = res/2;
  const SR  = res * 0.42;   // sphere radius in px

  // dark background
  ctx.fillStyle = '#060608';
  ctx.fillRect(0,0,res,res);

  // perspective projection into frame
  function proj(rx,ry,rz){
    const fov=2.8, s=fov/(fov-rz);
    return [mid + rx*SR*s*zoomLevel, mid - ry*SR*s*zoomLevel];
  }

  // ── grid ──
  for(let li=-2; li<=2; li++){
    const lat=(li/3)*Math.PI/2, cl=Math.cos(lat), sl=Math.sin(lat);
    const belt=(li===0);
    ctx.lineWidth = belt ? 1.6 : 0.7;
    ctx.beginPath(); let first=true;
    for(let lo=0; lo<=Math.PI*2+0.05; lo+=0.05){
      const [rx,ry,rz]=applyM(mat, cl*Math.cos(lo), sl, cl*Math.sin(lo));
      const [px,py]=proj(rx,ry,rz);
      ctx.globalAlpha=Math.max(0.07,0.08+(rz+1)*0.14);
      ctx.strokeStyle=belt?'#5ebd78':'#3a7040';
      first?(ctx.moveTo(px,py),first=false):ctx.lineTo(px,py);
    }
    ctx.stroke();
  }
  ctx.lineWidth=0.7;
  for(let lo=0;lo<8;lo++){
    const lon=(lo/8)*Math.PI*2, cl2=Math.cos(lon), sl2=Math.sin(lon);
    ctx.beginPath(); let first=true;
    for(let la=-Math.PI/2; la<=Math.PI/2+0.05; la+=0.06){
      const [rx,ry,rz]=applyM(mat, Math.cos(la)*cl2, Math.sin(la), Math.cos(la)*sl2);
      const [px,py]=proj(rx,ry,rz);
      ctx.globalAlpha=Math.max(0.06,0.07+(rz+1)*0.12);
      ctx.strokeStyle='#3a7040';
      first?(ctx.moveTo(px,py),first=false):ctx.lineTo(px,py);
    }
    ctx.stroke();
  }
  ctx.globalAlpha=1;

  // ── curve ──
  const end=Math.floor(drawProgress);
  if(end>=2){
    ctx.lineWidth=Math.max(1.2, traceThickness*(res/600));
    for(let i=1;i<end;i++){
      const p0=precomputed[i-1], p1=precomputed[i];
      if(!p0||!p1) continue;
      const [rx1,ry1,rz1]=applyM(mat,p0[0],p0[1],p0[2]);
      const [rx2,ry2,rz2]=applyM(mat,p1[0],p1[1],p1[2]);
      const [px1,py1]=proj(rx1,ry1,rz1);
      const [px2,py2]=proj(rx2,ry2,rz2);
      const frac=i/TOTAL_PTS;
      const gap=p1[4];
      const selCol=gap!==null?selectedGaps.get(gap):undefined;
      if(selCol){ ctx.strokeStyle=selCol; }
      else if(harmColorMode){ ctx.strokeStyle=harmColorForProgress(frac)||`hsl(${295+frac*40},100%,55%)`; }
      else { const avgZ=(rz1+rz2)*0.5; ctx.strokeStyle=`hsl(${295+frac*40},100%,${48+avgZ*22}%)`; }
      ctx.globalAlpha=0.84;
      ctx.beginPath(); ctx.moveTo(px1,py1); ctx.lineTo(px2,py2); ctx.stroke();
    }
    // centre glow
    const [crx,cry,crz]=applyM(mat,0,0,0);
    const [cx,cy]=proj(crx,cry,crz);
    ctx.globalAlpha=1;
    ctx.beginPath(); ctx.arc(cx,cy,Math.max(3,res*0.004),0,Math.PI*2);
    ctx.fillStyle='#ffd700'; ctx.shadowColor='#ffd700'; ctx.shadowBlur=20;
    ctx.fill(); ctx.shadowBlur=0;
  }

  // ── watermark ──
  ctx.fillStyle='rgba(232,197,71,.3)';
  ctx.font=`${Math.round(res*0.014)}px JetBrains Mono,monospace`;
  ctx.textAlign='right'; ctx.textBaseline='bottom'; ctx.globalAlpha=1;
  ctx.fillText(`f(θ)=${A}+sin(${B}θ)·sin(${C}θ/${D}) · Wessen Getachew`, res-16, res-10);

  return cv;
}

// ─── Build rotation matrix for a given axis+cumulative degrees ─
function rotatedMat(base, axis, cumDeg){
  const r = cumDeg * Math.PI / 180;
  if(axis==='X') return mmul(mrotX(r), base);
  if(axis==='Y') return mmul(mrotY(r), base);
  if(axis==='Z') return mmul(mrotZ(r), base);
  return base; // should not happen — XYZ handled per-frame
}

// ─── axis sequence for XYZ mode ───────────────────────────────
function axisForFrame(axis, f, total){
  if(axis!=='XYZ') return axis;
  const seg = Math.floor(total/3);
  if(f < seg)         return 'X';
  if(f < seg*2)       return 'Y';
  return 'Z';
}

// ═══════════════════════════════════════════════════════════════
//  WebM EXPORT  (full colour, MediaRecorder)
// ═══════════════════════════════════════════════════════════════
async function doWebM(res, fps, dur, rotDeg, axis){
  if(!window.MediaRecorder){ throw new Error('MediaRecorder not supported in this browser. Try Chrome/Edge.'); }

  const totalFrames = Math.ceil(fps * dur);
  const offCv = document.createElement('canvas');
  offCv.width = offCv.height = res;
  const offCtx = offCv.getContext('2d');

  const types=['video/webm;codecs=vp9','video/webm;codecs=vp8','video/webm'];
  const mime  = types.find(t=>MediaRecorder.isTypeSupported(t))||'video/webm';

  const stream = offCv.captureStream(fps);
  _mrChunks=[]; _mrec = new MediaRecorder(stream,{mimeType:mime, videoBitsPerSecond:60_000_000});
  _mrec.ondataavailable = e=>{ if(e.data.size>0) _mrChunks.push(e.data); };

  const whenDone = new Promise((ok,fail)=>{
    _mrec.onstop  = ok;
    _mrec.onerror = e=>fail(e.error||new Error('MediaRecorder error'));
  });
  _mrec.start(100); // collect chunks every 100 ms

  const baseM = rotMat.slice();
  let cumDeg = 0;

  for(let f=0; f<totalFrames; f++){
    if(gifAborted) break;
    const ax  = axisForFrame(axis, f, totalFrames);
    const mat = rotatedMat(baseM, ax, cumDeg);
    cumDeg += rotDeg;
    const frame = buildFrame(res, mat);
    offCtx.clearRect(0,0,res,res);
    offCtx.drawImage(frame,0,0);
    setStatus(`WebM: frame ${f+1} / ${totalFrames}`);
    setProg((f+1)/totalFrames*88);
    await yieldFrame();
  }

  _mrec.stop();
  setStatus('Finalising WebM…'); setProg(92);
  await whenDone;

  if(gifAborted) return;
  const blob = new Blob(_mrChunks,{type:mime});
  const url  = URL.createObjectURL(blob);
  const a    = Object.assign(document.createElement('a'),{href:url, download:`sphere_rot_${axis}_${res}p.webm`});
  a.click();
  setTimeout(()=>URL.revokeObjectURL(url),8000);
  setProg(100);
}

// ═══════════════════════════════════════════════════════════════
//  GIF EXPORT  (pure-JS, no network required)
// ═══════════════════════════════════════════════════════════════
async function doGIF(res, fps, dur, rotDeg, axis, quality){
  // Cap frames — GIF at 4K with many frames is huge; warn + cap
  const maxFrames = 120;
  const totalFrames = Math.min(Math.ceil(fps*dur), maxFrames);
  const delayCS = Math.round(100/fps); // centiseconds per frame

  setStatus(`Building ${totalFrames} frames…`);
  const baseM = rotMat.slice();
  const pixelData = [];
  let cumDeg = 0;

  for(let f=0; f<totalFrames; f++){
    if(gifAborted) return;
    const ax  = axisForFrame(axis, f, totalFrames);
    const mat = rotatedMat(baseM, ax, cumDeg);
    cumDeg += rotDeg;
    const cv  = buildFrame(res, mat);
    pixelData.push(cv.getContext('2d').getImageData(0,0,res,res).data);
    setProg((f+1)/totalFrames*55);
    setStatus(`Frame ${f+1}/${totalFrames}`);
    if(f%5===4) await yieldFrame();
  }

  if(gifAborted) return;
  setStatus('Quantising colours…'); setProg(58);
  await yieldFrame();

  const bytes = buildGIF89a(pixelData, res, res, delayCS, quality);
  setProg(98);

  if(gifAborted) return;
  const blob = new Blob([bytes],{type:'image/gif'});
  const url  = URL.createObjectURL(blob);
  const a    = Object.assign(document.createElement('a'),{href:url, download:`sphere_rot_${axis}_${res}p.gif`});
  a.click();
  setTimeout(()=>URL.revokeObjectURL(url),8000);
  setProg(100);
}

// ═══════════════════════════════════════════════════════════════
//  GIF89a ENCODER  (median-cut quantiser + LZW)
// ═══════════════════════════════════════════════════════════════
function buildGIF89a(frames, W, H, delayCS, quality){
  const N = 256; // palette size

  // ── Median-cut colour quantiser ─────────────────────────────
  function quantiseFrame(rgba){
    const nPx = W*H;
    // Sample every (quality)th pixel for speed
    const step = Math.max(1, quality);
    const sample = [];
    for(let i=0;i<nPx;i+=step){
      const o=i*4;
      sample.push([rgba[o],rgba[o+1],rgba[o+2]]);
    }

    // Build palette via median cut
    function medianCut(colors, depth){
      if(depth===0 || colors.length===0) return [averageColor(colors)];
      // Find channel with greatest range
      let minR=255,maxR=0,minG=255,maxG=0,minB=255,maxB=0;
      for(const [r,g,b] of colors){
        if(r<minR)minR=r; if(r>maxR)maxR=r;
        if(g<minG)minG=g; if(g>maxG)maxG=g;
        if(b<minB)minB=b; if(b>maxB)maxB=b;
      }
      const rR=maxR-minR, rG=maxG-minG, rB=maxB-minB;
      let ch=0;
      if(rG>rR&&rG>=rB) ch=1;
      else if(rB>rR&&rB>rG) ch=2;
      colors.sort((a,b)=>a[ch]-b[ch]);
      const mid=colors.length>>1;
      return [
        ...medianCut(colors.slice(0,mid),depth-1),
        ...medianCut(colors.slice(mid),  depth-1)
      ];
    }
    function averageColor(colors){
      if(!colors.length) return [0,0,0];
      let r=0,g=0,b=0;
      for(const [cr,cg,cb] of colors){r+=cr;g+=cg;b+=cb;}
      const n=colors.length;
      return [r/n|0, g/n|0, b/n|0];
    }

    const depth = Math.log2(N)|0; // 8 for 256 colours
    const palette = medianCut(sample, depth);
    // Ensure exactly N entries
    while(palette.length<N) palette.push([0,0,0]);
    palette.length=N;

    // Build index map: nearest-neighbour lookup via kd-approximation
    // For speed, use a flat 3D grid (6 bits per channel = 18 bits)
    const BITS=6, BINS=1<<BITS;
    const grid = new Int16Array(BINS*BINS*BINS).fill(-1);
    function gridIdx(r,g,b){ return ((r>>2)*BINS+(g>>2))*BINS+(b>>2); }

    palette.forEach(([r,g,b],i)=>{ grid[gridIdx(r,g,b)]=i; });

    function nearest(r,g,b){
      let best=1e9, bi=0;
      for(let i=0;i<N;i++){
        const [pr,pg,pb]=palette[i];
        const d=(r-pr)**2+(g-pg)**2+(b-pb)**2;
        if(d<best){best=d;bi=i;if(d===0)break;}
      }
      return bi;
    }

    // Map pixels to indices
    const indices = new Uint8Array(nPx);
    for(let i=0;i<nPx;i++){
      const o=i*4;
      indices[i]=nearest(rgba[o],rgba[o+1],rgba[o+2]);
    }

    return {palette, indices};
  }

  // ── LZW encoder ─────────────────────────────────────────────
  function lzwEncode(indices){
    const minLZW = 8; // GIF spec: min code size for 256-colour palette
    const clearCode = 1<<minLZW, eoi = clearCode+1;
    let codeSize = minLZW+1, maxCode = 1<<codeSize;
    const out=[]; let buf=0, bits=0;

    function emit(code){
      buf|=code<<bits; bits+=codeSize;
      while(bits>=8){ out.push(buf&0xff); buf>>=8; bits-=8; }
    }
    function reset(){
      table={}; nextCode=eoi+1; codeSize=minLZW+1; maxCode=1<<codeSize;
    }

    let table={}, nextCode=eoi+1;
    emit(clearCode);
    let prefix=indices[0];

    for(let i=1;i<indices.length;i++){
      const cur=indices[i];
      const key=(prefix<<8)|cur;
      if(table[key]!==undefined){
        prefix=table[key];
      } else {
        emit(prefix);
        if(nextCode<4096){
          table[key]=nextCode++;
          if(nextCode>maxCode && codeSize<12){ codeSize++; maxCode<<=1; }
        } else {
          emit(clearCode); reset();
        }
        prefix=cur;
      }
    }
    emit(prefix);
    emit(eoi);
    if(bits>0) out.push(buf&0xff);
    return out;
  }

  // ── Assemble GIF bytes ───────────────────────────────────────
  const out=[];
  const push=b=>out.push(b);
  const pushArr=a=>a.forEach(b=>out.push(b));
  const pushStr=s=>{ for(let i=0;i<s.length;i++) out.push(s.charCodeAt(i)); };
  const word=v=>[v&0xff,(v>>8)&0xff];

  // Header
  pushStr('GIF89a');
  pushArr(word(W)); pushArr(word(H));
  push(0x70); push(0); push(0); // no global colour table; bg=0; aspect=0

  // NETSCAPE looping extension (loop forever)
  pushArr([0x21,0xff,0x0b]); pushStr('NETSCAPE2.0');
  pushArr([0x03,0x01,0x00,0x00,0x00]);

  // Frames
  for(let fi=0;fi<frames.length;fi++){
    const {palette, indices} = quantiseFrame(frames[fi]);

    // Graphics control extension (delay + disposal)
    push(0x21); push(0xf9); push(0x04);
    push(0x04); // disposal=2 (restore to bg), no user input, no transparent
    pushArr(word(delayCS));
    push(0); push(0); // transparent index (unused)

    // Image descriptor
    push(0x2c);
    pushArr([0,0,0,0]); // left=0,top=0
    pushArr(word(W)); pushArr(word(H));
    push(0x80|7); // local colour table present, size=256 (2^(7+1)=256)

    // Local colour table (R,G,B × 256)
    for(let i=0;i<N;i++){ const [r,g,b]=palette[i]; push(r);push(g);push(b); }

    // Image data: min LZW code size, then blocks
    push(8); // minLZW=8
    const lzw = lzwEncode(indices);
    for(let i=0;i<lzw.length;i+=255){
      const chunk=lzw.slice(i,i+255);
      push(chunk.length);
      pushArr(chunk);
    }
    push(0); // block terminator
  }

  push(0x3b); // GIF trailer
  return new Uint8Array(out);
}

})(); // end gifExportModule

</script>
</body>
</html>
