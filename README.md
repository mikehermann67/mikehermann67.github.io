
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bald Birds Brewing — Menu Editor</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Libre+Baskerville:ital,wght@0,400;0,700;1,400&family=Barlow+Condensed:wght@400;600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --ink: #000000;
    --gold: #000000;
    --gold-light: #333333;
    --cream: #ffffff;
    --cream-dark: #f0f0f0;
    --rule: #000000;
    --muted: #333333;
    --section-bg: #ffffff;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: #cccccc;
    font-family: 'Libre Baskerville', Georgia, serif;
    min-height: 100vh;
    padding: 0;
  }

  /* ── EDITOR TOOLBAR ── */
  #toolbar {
    background: #ffffff;
    border-bottom: 2px solid #000000;
    padding: 12px 24px;
    display: flex;
    align-items: center;
    gap: 12px;
    flex-wrap: wrap;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: 0 4px 20px rgba(0,0,0,0.15);
  }

  #toolbar h1 {
    font-family: 'Playfair Display', serif;
    color: #000000;
    font-size: 1.1rem;
    letter-spacing: 0.05em;
    margin-right: 8px;
    white-space: nowrap;
  }

  .tb-sep { width: 1px; height: 28px; background: #cccccc; }

  .tb-btn {
    background: #ffffff;
    border: 1px solid #000000;
    color: #000000;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.82rem;
    font-weight: 600;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding: 6px 14px;
    cursor: pointer;
    border-radius: 3px;
    transition: all 0.15s;
    white-space: nowrap;
  }
  .tb-btn:hover { background: #000000; color: #ffffff; }
  .tb-btn.primary { background: #000000; color: #ffffff; border-color: #000000; font-weight: 700; }
  .tb-btn.primary:hover { background: #333333; }
  .tb-btn.danger { border-color: #cc0000; color: #cc0000; }
  .tb-btn.danger:hover { background: #cc0000; color: white; border-color: #cc0000; }
  .tb-btn.muted { border-color: #888; color: #555; }
  .tb-btn.muted:hover { background: #555; color: white; border-color: #555; }

  .tb-label {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.75rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: #555555;
  }

  #venue-name-input {
    background: #ffffff;
    border: 1px solid #000000;
    color: #000000;
    font-family: 'Playfair Display', serif;
    font-size: 0.9rem;
    padding: 5px 10px;
    border-radius: 3px;
    width: 200px;
  }
  #venue-name-input:focus { outline: none; border-color: #000000; }

  /* ── EDITOR CANVAS ── */
  #editor-wrap {
    width: fit-content;
    margin: 30px auto;
    padding: 0 220px 60px 40px;
  }

  /* ── MENU PAGE ── */
  .menu-page {
    background: var(--cream);
    box-shadow: 0 4px 20px rgba(0,0,0,0.2), inset 0 0 0 1px rgba(0,0,0,0.1);
    padding: 0.5in;
    margin-bottom: 40px;
    position: relative;
    width: 8.5in;
    min-height: 11in;
    height: 11in;
    overflow: hidden;
  }

  /* Flow indicator — shown when page is being used as a flow container */
  .flow-page-label {
    position: absolute;
    top: 6px;
    right: 12px;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.65rem;
    letter-spacing: 0.1em;
    color: var(--muted);
    opacity: 0.5;
  }

  .menu-page::before {
    content: '';
    position: absolute;
    inset: 8px;
    border: 1px solid rgba(201,168,76,0.25);
    pointer-events: none;
    z-index: 1;
  }

  /* ── HEADER ── */
  .menu-header {
    text-align: center;
    margin-bottom: 16px;
    padding-bottom: 12px;
    border-bottom: 2px solid var(--gold);
    position: relative;
  }

  .venue-logo-area {
    margin-bottom: 2px;
  }

  .logo-circle {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 90px;
    height: 90px;
    border-radius: 50%;
    border: 3px solid #000000;
    background: #ffffff;
    margin-bottom: 4px;
  }

  .logo-circle svg { width: 60px; height: 60px; }

  .venue-logo-img {
    width: 90px;
    height: 90px;
    object-fit: contain;
    display: block;
    margin: 0 auto 4px;
  }


  .venue-name {
    font-family: 'Playfair Display', serif;
    font-size: 1.9rem;
    font-weight: 900;
    color: var(--ink);
    letter-spacing: 0.08em;
    text-transform: uppercase;
    line-height: 1;
    margin-top: 6px;
  }

  .menu-title {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.75rem;
    font-weight: 600;
    letter-spacing: 0.25em;
    text-transform: uppercase;
    color: var(--muted);
    margin-top: 4px;
  }

  .menu-tagline {
    font-family: 'Libre Baskerville', serif;
    font-size: 0.78rem;
    color: var(--muted);
    margin-top: 6px;
  }

  /* ── SECTION ── */
  .menu-section {
    margin-bottom: 16px;
    position: relative;
  }

  .section-header {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 4px;
  }

  .section-name {
    font-family: 'Playfair Display', serif;
    font-weight: 700;
    font-size: 1.0rem;
    color: var(--ink);
    text-transform: uppercase;
    letter-spacing: 0.05em;
    flex: 1;
  }

  .section-subtitle {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.72rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 8px;
  }

  .section-divider {
    height: 2px;
    background: var(--ink);
    margin-bottom: 10px;
  }

  /* ── MENU ITEM ROW ── */
  .menu-item {
    display: flex;
    align-items: center;
    gap: 0;
    padding: 4px 0;
  }

  .item-name {
    font-family: 'Libre Baskerville', serif;
    font-weight: 700;
    font-size: 0.82rem;
    color: var(--ink);
    flex-shrink: 0;
    max-width: 260px;
  }

  .new-badge {
    display: inline-block;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.6rem;
    font-weight: 800;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    background: #000;
    color: #fff;
    padding: 1px 5px 2px;
    border-radius: 2px;
    margin-right: 6px;
    vertical-align: middle;
    flex-shrink: 0;
    position: relative;
    top: -1px;
  }

  .item-desc {
    font-family: 'Libre Baskerville', serif;
    font-weight: 400;
    font-size: 0.75rem;
    color: var(--muted);
    flex-shrink: 0;
    margin-left: 6px;
    max-width: 200px;
  }

  .item-dots {
    flex: 1;
    border-bottom: 1px dotted var(--rule);
    margin: 0 8px;
    min-width: 20px;
    align-self: center;
    height: 0;
    margin-bottom: 3px;
  }

  .item-size {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.75rem;
    letter-spacing: 0.08em;
    color: var(--muted);
    flex-shrink: 0;
    width: 80px;
    text-align: right;
    margin-right: 2px;
  }

  .item-price {
    font-family: 'Libre Baskerville', serif;
    font-weight: 700;
    font-size: 0.82rem;
    color: var(--ink);
    flex-shrink: 0;
    width: 52px;
    text-align: right;
  }

  .item-price-col {
    display: flex;
    align-items: baseline;
    flex-shrink: 0;
    gap: 0;
  }

  /* ── FOOTER ── */
  .menu-footer {
    margin-top: 24px;
    padding-top: 12px;
    border-top: 1px solid var(--rule);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .footer-date {
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.7rem;
    letter-spacing: 0.1em;
    color: var(--muted);
  }

  /* ── EDIT OVERLAYS (hidden in print) ── */
  .edit-controls {
    display: none;
    flex-direction: row;
    gap: 3px;
    margin-left: 8px;
    flex-shrink: 0;
    align-items: center;
  }

  .menu-item:hover .edit-controls {
    display: flex;
  }

  .edit-controls-section {
    display: flex;
    flex-direction: row;
    gap: 3px;
    margin-top: 4px;
    opacity: 0.4;
    transition: opacity 0.15s;
  }

  .menu-section:hover .edit-controls-section { opacity: 1; }

  .ec-btn {
    background: #ffffff;
    color: #000000;
    border: 1px solid #000000;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.68rem;
    font-weight: 600;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    padding: 3px 8px;
    cursor: pointer;
    border-radius: 2px;
    white-space: nowrap;
    transition: all 0.12s;
  }
  .ec-btn:hover { background: #000000; color: #ffffff; }
  .ec-btn.del { border-color: #cc0000; color: #cc0000; }
  .ec-btn.del:hover { background: #cc0000; color: white; }
  .ec-btn.hide-btn { border-color: #888; color: #888; }
  .ec-btn.hide-btn:hover { background: #888; color: white; }

  /* Hidden items — dimmed in editor, gone in print */
  .menu-item.item-hidden {
    opacity: 0.35;
    text-decoration: line-through;
    text-decoration-color: #999;
  }
  .menu-item.item-hidden .item-name,
  .menu-item.item-hidden .item-desc,
  .menu-item.item-hidden .item-price,
  .menu-item.item-hidden .item-size {
    color: #999 !important;
  }

  /* ── INLINE EDIT ── */
  .editable {
    cursor: text;
    border-bottom: 1px dashed transparent;
    transition: border-color 0.15s;
    outline: none;
    min-width: 10px;
    display: inline-block;
  }
  .editable:hover { border-bottom-color: #000000; }
  .editable:focus { border-bottom-color: #000000; background: rgba(0,0,0,0.05); border-radius: 2px; }

  .add-item-row {
    text-align: center;
    padding: 6px 0 2px;
  }

  .add-item-btn {
    background: none;
    border: 1px dashed #999999;
    color: #555555;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.72rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    padding: 4px 16px;
    cursor: pointer;
    border-radius: 2px;
    transition: all 0.15s;
  }
  .add-item-btn:hover { border-color: #000000; color: #000000; background: rgba(0,0,0,0.05); }

  .add-section-row {
    text-align: center;
    margin-top: 8px;
  }

  /* ── MODAL ── */
  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.5);
    z-index: 200;
    align-items: center;
    justify-content: center;
  }
  .modal-overlay.open { display: flex; }

  .modal {
    background: #ffffff;
    border: 1px solid #000000;
    padding: 28px 32px;
    width: 460px;
    max-width: 95vw;
    box-shadow: 0 20px 60px rgba(0,0,0,0.3);
  }

  .modal h2 {
    font-family: 'Playfair Display', serif;
    color: #000000;
    font-size: 1.2rem;
    margin-bottom: 20px;
    letter-spacing: 0.05em;
  }

  .form-row {
    margin-bottom: 14px;
  }

  .form-row label {
    display: block;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.72rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: #555555;
    margin-bottom: 5px;
  }

  .form-row input, .form-row select {
    width: 100%;
    background: #ffffff;
    border: 1px solid #000000;
    color: #000000;
    font-family: 'Libre Baskerville', serif;
    font-size: 0.85rem;
    padding: 7px 10px;
    border-radius: 3px;
  }
  .form-row input:focus, .form-row select:focus { outline: none; border-color: #000000; }

  .form-row select option { background: #ffffff; color: #000000; }

  .modal-btns {
    display: flex;
    gap: 10px;
    margin-top: 20px;
    justify-content: flex-end;
  }

  /* ── FONT SIZE CONTROLS ── */
  .page-wrapper {
    position: relative;
    width: fit-content;
  }
  .font-size-panel {
    position: absolute;
    top: 0;
    left: calc(8.5in + 12px);
    background: #ffffff;
    border: 1px solid #000000;
    padding: 8px 12px;
    display: flex;
    flex-direction: column;
    gap: 6px;
    z-index: 10;
    font-family: 'Barlow Condensed', sans-serif;
    font-size: 0.68rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    box-shadow: 0 2px 8px rgba(0,0,0,0.15);
    min-width: 160px;
  }
  .fsp-title {
    font-weight: 700;
    font-size: 0.72rem;
    color: #000;
    border-bottom: 1px solid #000;
    padding-bottom: 4px;
    margin-bottom: 2px;
  }
  .fsp-row {
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .fsp-label { flex: 1; color: #555; }
  .fsp-val { width: 36px; text-align: center; font-weight: 700; color: #000; }
  .fsp-btn {
    background: #fff;
    border: 1px solid #000;
    color: #000;
    width: 22px;
    height: 22px;
    cursor: pointer;
    font-size: 1rem;
    line-height: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0;
    flex-shrink: 0;
  }
  .fsp-btn:hover { background: #000; color: #fff; }



  /* PAGE 1 — On Draft (13 dual-price rows) + Can Pours + Flights: tight */
  .page-pi-0 .menu-item         { padding: 2px 0; }
  .page-pi-0 .item-name         { font-size: 0.76rem; }
  .page-pi-0 .item-desc         { font-size: 0.68rem; }
  .page-pi-0 .item-size         { font-size: 0.68rem; width: 72px; }
  .page-pi-0 .item-price        { font-size: 0.76rem; width: 46px; }
  .page-pi-0 .section-name      { font-size: 0.88rem; }
  .page-pi-0 .section-subtitle  { font-size: 0.65rem; margin-bottom: 5px; }
  .page-pi-0 .section-divider   { margin-bottom: 6px; }
  .page-pi-0 .menu-section      { margin-bottom: 10px; }
  .page-pi-0 .menu-header       { margin-bottom: 10px; padding-bottom: 8px; }

  /* PAGE 2 — Seasonal + Classic + Canned Cocktails: tight to fit on one page */
  .page-pi-1 .menu-item         { padding: 2px 0; }
  .page-pi-1 .item-name         { font-size: 0.76rem; }
  .page-pi-1 .item-desc         { font-size: 0.68rem; }
  .page-pi-1 .item-size         { font-size: 0.68rem; width: 72px; }
  .page-pi-1 .item-price        { font-size: 0.76rem; width: 46px; }
  .page-pi-1 .section-name      { font-size: 0.88rem; }
  .page-pi-1 .section-subtitle  { font-size: 0.62rem; margin-bottom: 4px; }
  .page-pi-1 .section-divider   { margin-bottom: 5px; }
  .page-pi-1 .menu-section      { margin-bottom: 8px; }
  .page-pi-1 .menu-header       { margin-bottom: 8px; padding-bottom: 6px; }

  /* PAGE 3 — Liquor + Shots + Wine Pours: comfortable */
  .page-pi-2 .menu-item         { padding: 5px 0; }
  .page-pi-2 .item-name         { font-size: 0.86rem; }
  .page-pi-2 .item-price        { font-size: 0.86rem; }
  .page-pi-2 .section-name      { font-size: 1.05rem; }
  .page-pi-2 .menu-section      { margin-bottom: 20px; }
  .page-pi-2 .menu-header       { margin-bottom: 20px; }

  /* PAGE 4 — Soda only: very spacious */
  .page-pi-3 .menu-item         { padding: 8px 0; }
  .page-pi-3 .item-name         { font-size: 0.92rem; }
  .page-pi-3 .item-price        { font-size: 0.92rem; }
  .page-pi-3 .section-name      { font-size: 1.1rem; }
  .page-pi-3 .section-subtitle  { font-size: 0.78rem; margin-bottom: 12px; }
  .page-pi-3 .section-divider   { margin-bottom: 14px; }
  .page-pi-3 .menu-section      { margin-bottom: 28px; }
  .page-pi-3 .menu-header       { margin-bottom: 28px; }
  @media print {
    @page { margin: 0; size: 8.5in 11in portrait; }
    body { background: white; padding: 0; margin: 0; }
    #toolbar { display: none !important; }
    #editor-wrap { width: auto; margin: 0; padding: 0; }
    .menu-page {
      box-shadow: none !important;
      outline: none !important;
      page-break-after: always;
      break-after: page;
      margin: 0 !important;
      width: 8.5in !important;
      min-height: 11in !important;
      max-height: none !important;
      overflow: visible !important;
      padding: 0.5in !important;
    }
    .menu-page::before { display: none; }
    .overflow-warning { display: none !important; }
    .edit-controls, .edit-controls-section, .add-item-row, .add-section-row { display: none !important; }
    .menu-item.item-hidden { display: none !important; }
    .editable { border: none !important; }
    .menu-item { break-inside: avoid; }
    .font-size-panel { display: none !important; }
    .page-wrapper { display: block; }
  }
</style>
</head>
<body>

<!-- TOOLBAR -->
<div id="toolbar">
  <h1>🍺 Menu Editor</h1>
  <div class="tb-sep"></div>
  <span class="tb-label">Venue:</span>
  <input type="text" id="venue-name-input" placeholder="Venue Name" value="Bald Birds Brewing Co.">
  <div class="tb-sep"></div>
  <button class="tb-btn" onclick="addSection()">+ Add Section</button>
  <button class="tb-btn" onclick="addPage()">+ Add Page</button>
  <div class="tb-sep"></div>
  <button class="tb-btn primary" onclick="window.print()">🖨 Print Menu</button>
  <div class="tb-sep"></div>
  <button class="tb-btn" onclick="exportJSON()">⬇ Local Backup</button>
  <button class="tb-btn" onclick="document.getElementById('import-input').click()">⬆ Local Restore</button>
  <input type="file" id="import-input" accept=".json" style="display:none" onchange="importJSON(event)">
  <div class="tb-sep"></div>
  <span id="cloud-status" style="font-family:'Barlow Condensed',sans-serif;font-size:0.75rem;letter-spacing:0.08em;color:#888;white-space:nowrap;"></span>
</div>

<!-- SETUP BANNER (shown when no script URL configured) -->
<div id="setup-banner" style="display:none;background:#2a1a00;border-bottom:2px solid var(--gold);padding:10px 24px;color:var(--cream);font-family:'Barlow Condensed',sans-serif;font-size:0.82rem;letter-spacing:0.05em;">
  ⚙️ <strong>Cloud Save not configured.</strong> &nbsp;Paste your Apps Script Web App URL here:
  <input type="text" id="script-url-input" placeholder="https://script.google.com/macros/s/YOUR_ID/exec"
    style="background:#1a1208;border:1px solid var(--gold);color:var(--gold-light);font-family:'Libre Baskerville',serif;font-size:0.8rem;padding:4px 10px;border-radius:3px;width:420px;margin:0 8px;">
  <button onclick="saveScriptUrl()" style="background:var(--gold);color:var(--ink);border:none;font-family:'Barlow Condensed',sans-serif;font-weight:700;font-size:0.8rem;letter-spacing:0.08em;padding:5px 14px;border-radius:3px;cursor:pointer;">Connect</button>
  <button onclick="document.getElementById('setup-banner').style.display=\'none\'" style="background:transparent;border:1px solid #555;color:#888;font-family:\'Barlow Condensed\',sans-serif;font-size:0.8rem;padding:5px 10px;border-radius:3px;cursor:pointer;margin-left:6px;">Dismiss</button>
</div>

<!-- EDITOR CANVAS -->
<div id="editor-wrap"></div>

<!-- MODAL -->
<div class="modal-overlay" id="modal">
  <div class="modal">
    <h2 id="modal-title">Add Item</h2>
    <div class="form-row">
      <label>Drink Name</label>
      <input type="text" id="f-name" placeholder="e.g. Blackberry Margarita">
    </div>
    <div class="form-row">
      <label>Description (optional)</label>
      <input type="text" id="f-desc" placeholder="e.g. Notes of chocolate and vanilla">
    </div>
    <div class="form-row">
      <label>Beer Style (optional)</label>
      <input type="text" id="f-style" placeholder="e.g. Stout - Coffee">
    </div>
    <div class="form-row">
      <label>ABV (optional)</label>
      <input type="text" id="f-abv" placeholder="e.g. 6.1" oninput="formatAbvInput(this)">
    </div>
    <div class="form-row">
      <label>Serving Size</label>
      <select id="f-size">
        <option value="Serving">Serving</option>
        <option value="10oz Draft">10oz Draft</option>
        <option value="16oz Draft">16oz Draft</option>
        <option value="12oz Can">12oz Can</option>
        <option value="16oz Can">16oz Can</option>
        <option value="19.2oz Can">19.2oz Can</option>
      </select>
    </div>
    <div class="form-row">
      <label>Price</label>
      <input type="text" id="f-price" placeholder="e.g. 12.00" oninput="formatPriceInput(this)">
    </div>
    <div class="form-row" id="f-price2-row" style="display:none">
      <label>2nd Size (optional)</label>
      <select id="f-size2">
        <option value="Serving">Serving</option>
        <option value="10oz Draft">10oz Draft</option>
        <option value="16oz Draft">16oz Draft</option>
        <option value="12oz Can">12oz Can</option>
        <option value="16oz Can">16oz Can</option>
        <option value="19.2oz Can">19.2oz Can</option>
      </select>
    </div>
    <div class="form-row" id="f-price2-val-row" style="display:none">
      <label>2nd Price</label>
      <input type="text" id="f-price2" placeholder="e.g. 6.00" oninput="formatPriceInput(this)">
    </div>
    <label style="color:var(--muted);font-family:'Barlow Condensed',sans-serif;font-size:0.72rem;letter-spacing:0.1em;text-transform:uppercase;display:flex;align-items:center;gap:8px;margin-bottom:10px;">
      <input type="checkbox" id="f-dual" onchange="toggleDual()"> Two sizes / prices
    </label>
    <label style="color:var(--muted);font-family:'Barlow Condensed',sans-serif;font-size:0.72rem;letter-spacing:0.1em;text-transform:uppercase;display:flex;align-items:center;gap:8px;margin-bottom:10px;">
      <input type="checkbox" id="f-new"> Mark as <strong style="color:#000;margin-left:4px;">NEW!</strong>
    </label>
    <div class="modal-btns">
      <button class="tb-btn" onclick="closeModal()">Cancel</button>
      <button class="tb-btn muted" id="modal-hide-btn" onclick="modalToggleHidden()" style="display:none">👁 Hide Item</button>
      <button class="tb-btn danger" id="modal-delete-btn" onclick="modalDeleteItem()" style="display:none">✕ Delete</button>
      <button class="tb-btn primary" id="modal-save-btn" onclick="saveModal()">Add Item</button>
    </div>
  </div>
</div>

<script>
// ─── DATA ───────────────────────────────────────────────────────────────────
let menuData = {
  venue: "Bald Birds Brewing Co.",
  pages: [
    {
      // PAGE 1: On Draft, Can Pours, Flights
      title: "Draft & Can Menu",
      tagline: "$2 OFF 16oz Drafts During Happy Hour! Mon. & Wed.: Open\u2013Close  \u00b7  Tue., Thur. & Fri: 4\u20136pm",
      sections: [
        {
          name: "On Draft",
          subtitle: "$2 OFF 16oz DRAFTS DURING HAPPY HOUR!",
          items: [
            { name: "Bald Birds \u2022 Strawberry Noir", desc: "Stout - Pastry \u2022 6.1% ABV", size: "16oz Draft", price: "$8.00", size2: "10oz Draft", price2: "$7.00" },
            { name: "Bald Birds \u2022 Penn Pals", desc: "Brown Ale - Other \u2022 4.8% ABV", size: "16oz Draft", price: "$8.00", size2: "10oz Draft", price2: "$7.00" },
            { name: "Bald Birds \u2022 Wunderkind", desc: "Pilsner - German \u2022 4.5% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Bald Birds \u2022 Fade", desc: "IPA - American \u2022 6.4% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Bald Birds \u2022 Chirp", desc: "Lager - American Light \u2022 4% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Bald Birds \u2022 Perch Pilsner", desc: "Pilsner - Other \u2022 5.2% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Bald Birds \u2022 Foggy Noggin", desc: "IPA - Imperial \u2022 8.2% ABV", size: "16oz Draft", price: "$8.00", size2: "10oz Draft", price2: "$7.00" },
            { name: "Bald Birds \u2022 Morning Joe Coffee Stout", desc: "Stout - Coffee \u2022 6.1% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Bald Birds \u2022 Trojan Horse Amber Lager", desc: "M\u00e4rzen \u2022 5.8% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Bald Birds \u2022 Overlook", desc: "IPA - American \u2022 6.5% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Bald Birds \u2022 Goldfinch Helles Lager", desc: "Lager - Helles \u2022 5% ABV", size: "16oz Draft", price: "$7.00", size2: "10oz Draft", price2: "$6.00" },
            { name: "Workhorse \u2022 Prickly Pear Margarita", desc: "Sour - Gose \u2022 4.9% ABV", size: "16oz Draft", price: "$5.00", size2: "10oz Draft", price2: "$4.00" },
            { name: "Workhorse \u2022 Root Beer", desc: "Root Beer", size: "16oz Draft", price: "$4.00" },
          ]
        },
        {
          name: "Can Pours",
          subtitle: "",
          items: [
            { name: "Bald Birds \u2022 Cloudy Judgment", desc: "IPA - New England \u2022 6.3% ABV", size: "12oz Can", price: "$6.00" },
            { name: "Bald Birds \u2022 Faux Hawk", desc: "Non-Alcoholic IPA \u2022 0.5% ABV", size: "12oz Can", price: "$6.00" },
            { name: "Workhorse \u2022 Sip Tripp", desc: "IPA - Imperial \u2022 10% ABV", size: "19.2oz Can", price: "$3.00" },
          ]
        },
        {
          name: "Flights",
          subtitle: "CHOOSE ANY 4 DRAFTS \u00b7 4oz POURS",
          items: [
            { name: "Flight of 4", desc: "", size: "4 \u00d7 4oz", price: "$14.00" },
          ]
        }
      ]
    },
    {
      // PAGE 2: Seasonal Cocktails, Classic Cocktails, Canned Cocktails
      title: "Cocktails & Spirits",
      tagline: "Mon & Wed All Day Happy Hour \u00b7 Tues, Thurs, Fri: 4\u20136  \u00b7  All Cocktails $5 Off During Happy Hour",
      sections: [
        {
          name: "Seasonal Cocktails",
          subtitle: "ALL COCKTAILS $5 OFF DURING HAPPY HOUR! ALL DAY MON & WEDS / TUES, THURS, FRI: 4-6",
          items: [
            { name: "Blackberry Margarita", desc: "", size: "Serving", price: "$14.00" },
            { name: "Lavender Lemonade", desc: "", size: "Serving", price: "$14.00" },
            { name: "Pineapple Upside Down Cake", desc: "", size: "Serving", price: "$14.00" },
            { name: "Spiked Cherry Cola", desc: "", size: "Serving", price: "$14.00" },
            { name: "Strawberry Dream", desc: "", size: "Serving", price: "$14.00" },
            { name: "Winter\u2019s Old Fashioned", desc: "", size: "Serving", price: "$14.00" },
          ]
        },
        {
          name: "Classic Cocktails",
          subtitle: "ALL COCKTAILS $5 OFF DURING HAPPY HOUR! ALL DAY MON & WEDS / TUES, THURS, FRI: 4-6",
          items: [
            { name: "Cosmo", desc: "", size: "Serving", price: "$12.00" },
            { name: "Margarita", desc: "", size: "Serving", price: "$12.00" },
            { name: "Old Fashioned", desc: "", size: "Serving", price: "$12.00" },
            { name: "Vodka Mule", desc: "", size: "Serving", price: "$12.00" },
            { name: "Whiskey Sour", desc: "", size: "Serving", price: "$12.00" },
          ]
        },
        {
          name: "Canned Cocktails",
          subtitle: "View our wide selection of canned cocktail options!",
          items: [
            { name: "Stateside Iced Tea", desc: "", size: "Serving", price: "$11.00" },
            { name: "Stateside Surfside Iced Tea & Lemonade", desc: "", size: "Serving", price: "$11.00" },
            { name: "Stateside Green Tea and Vodka", desc: "", size: "Serving", price: "$11.00" },
            { name: "Stateside Orange Seltzer", desc: "", size: "Serving", price: "$11.00" },
          ]
        }
      ]
    },
    {
      // PAGE 3: Liquor, Wine Pours, Soda
      title: "Spirits, Wine & Non-Alcoholic",
      tagline: "Mon & Wed All Day Happy Hour \u00b7 Tues, Thurs, Fri: 4\u20136",
      sections: [
        {
          name: "Liquor",
          subtitle: "Build your own cocktail or shot.",
          items: [
            { name: "Hidden Still Gin", desc: "", size: "Serving", price: "$8.00" },
            { name: "Four Birds Rum", desc: "", size: "Serving", price: "$8.00" },
            { name: "Four Birds Agave Spirit", desc: "", size: "Serving", price: "$8.00" },
            { name: "Four Birds Bourbon", desc: "", size: "Serving", price: "$8.00" },
            { name: "Four Birds Vodka", desc: "", size: "Serving", price: "$8.00" },
          ]
        },
        {
          name: "Wine Pours",
          subtitle: "",
          items: [
            { name: "Penns Woods Carbernet Franc", desc: "", size: "Serving", price: "$11.00" },
            { name: "Penns Woods Chardonnay", desc: "", size: "Serving", price: "$11.00" },
            { name: "Penns Woods Sauvignon Blanc", desc: "", size: "Serving", price: "$11.00" },
            { name: "Penns Woods Willow White", desc: "", size: "Serving", price: "$11.00" },
            { name: "Penns Woods Pinot Rose", desc: "", size: "Serving", price: "$11.00" },
          ]
        },
        {
          name: "Soda",
          subtitle: "FOR REFILLS PLEASE ASK SERVER OR BARTENDER",
          items: [
            { name: "Coca-Cola", desc: "", size: "Serving", price: "$3.00" },
            { name: "Diet Coke", desc: "", size: "Serving", price: "$3.00" },
            { name: "Ginger Ale", desc: "", size: "Serving", price: "$3.00" },
            { name: "Tonic", desc: "", size: "Serving", price: "$3.00" },
            { name: "Lemonade", desc: "", size: "Serving", price: "$3.00" },
            { name: "Cranberry Juice", desc: "", size: "Serving", price: "$3.00" },
            { name: "Club Soda", desc: "", size: "Serving", price: "$0.00" },
            { name: "Ginger Beer", desc: "", size: "Serving", price: "$3.00" },
          ]
        }
      ]
    }
  ]
};

// ─── MODAL STATE ─────────────────────────────────────────────────────────────
let modalMode = null; // 'add-item' | 'edit-item' | 'add-section'
let modalCtx  = {};

// ─── RENDER ──────────────────────────────────────────────────────────────────
// Page content height in pixels at 96dpi: 11in * 96 = 1056px, minus padding 0.5in*2*96 = 96+96=192
// Usable content height: 1056 - 192 = 864px
// We use a slightly smaller budget to be safe
const PAGE_HEIGHT_PX = 1056;
const PAGE_PADDING_PX = 96; // 0.5in top + 0.5in bottom at 96dpi each = 48+48
const USABLE_HEIGHT = PAGE_HEIGHT_PX - (PAGE_PADDING_PX * 2);

// Build page header HTML (shared across all pages)
function buildHeaderHTML(pi, pageTitle, pageTagline) {
  return `
    <div class="menu-header">
      <div class="venue-logo-area">
        <img src="data:image/png;base64,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" class="venue-logo-img" alt="Bald Birds Brewing Co.">
      </div>
      <div class="menu-title" contenteditable="true" oninput="menuData.pages[${pi}].title=this.innerText;scheduleSave()">${pageTitle}</div>
      <div class="menu-tagline" contenteditable="true" oninput="menuData.pages[${pi}].tagline=this.innerText;scheduleSave()">${pageTagline}</div>
    </div>`;
}

function buildFooterHTML(pageNum, totalPages) {
  return `<div class="menu-footer"><span class="footer-date">Page ${pageNum} of ${totalPages}</span></div>`;
}

function buildItemHTML(pi, si, ii, item) {
  // Build the description string from separate style/abv fields, or fall back to legacy desc
  let descDisplay = '';
  if (item.style || item.abv) {
    const parts = [];
    if (item.style) parts.push(item.style);
    if (item.abv) parts.push(item.abv + '% ABV');
    if (item.desc) parts.unshift(item.desc);
    descDisplay = parts.join(' • ');
  } else if (item.desc) {
    descDisplay = item.desc;
  }
  let priceCol = '';
  if (item.size2 && item.price2) {
    priceCol = `
      <span class="item-size">${item.size}</span><span class="item-price">${item.price}</span>
      <span class="item-size" style="margin-left:10px">${item.size2}</span><span class="item-price">${item.price2}</span>`;
  } else {
    priceCol = `<span class="item-size" style="width:auto;margin-right:8px">${item.size}</span><span class="item-price">${item.price}</span>`;
  }
  const hiddenClass = item.hidden ? ' item-hidden' : '';
  const hideLabel = item.hidden ? '👁 Show' : '👁 Hide';
  return `
    <div class="menu-item${hiddenClass}" data-pi="${pi}" data-si="${si}" data-ii="${ii}">
      ${item.isNew ? '<span class="new-badge">New!</span>' : ''}
      <span class="item-name editable" contenteditable="true" oninput="menuData.pages[${pi}].sections[${si}].items[${ii}].name=this.innerText;scheduleSave()">${item.name}</span>
      ${descDisplay ? `<span class="item-desc editable" contenteditable="true" oninput="menuData.pages[${pi}].sections[${si}].items[${ii}].desc=this.innerText;scheduleSave()">• ${descDisplay}</span>` : ''}
      <span class="item-dots"></span>
      ${priceCol}
      <div class="edit-controls">
        <button class="ec-btn" onclick="openEditItem(${pi},${si},${ii})">Edit</button>
        <button class="ec-btn" onclick="moveItem(${pi},${si},${ii},-1)">▲</button>
        <button class="ec-btn" onclick="moveItem(${pi},${si},${ii},1)">▼</button>
      </div>
    </div>`;
}

function buildSectionHeaderHTML(pi, si, sec) {
  return `
    <div class="section-header">
      <span class="section-name editable" contenteditable="true" oninput="menuData.pages[${pi}].sections[${si}].name=this.innerText;scheduleSave()">${sec.name}</span>
    </div>
    ${sec.subtitle ? `<div class="section-subtitle editable" contenteditable="true" oninput="menuData.pages[${pi}].sections[${si}].subtitle=this.innerText;scheduleSave()">${sec.subtitle}</div>` : ''}
    <div class="section-divider"></div>`;
}

function render() {
  document.getElementById('venue-name-input').value = menuData.venue;
  const wrap = document.getElementById('editor-wrap');
  wrap.innerHTML = '';

  // ── STEP 2 & 3 & 4: Render one page per pi — no overflow splitting
  menuData.pages.forEach((page, pi) => {
    const wrapperEl = document.createElement('div');
    wrapperEl.className = 'page-wrapper';

    const pageEl = document.createElement('div');
    pageEl.className = `menu-page page-pi-${pi}`;

    // Font size control panel (outside page to avoid overflow:hidden clipping)
    const fs = getFontSizes(pi);
    const fspEl = document.createElement('div');
    fspEl.className = 'font-size-panel';
    fspEl.innerHTML = `
      <div class="fsp-title">Page ${pi + 1} Font Sizes</div>
      <div class="fsp-row">
        <span class="fsp-label">Section</span>
        <button class="fsp-btn" onclick="adjustFontSize(${pi},'section',-0.05)">−</button>
        <span class="fsp-val" id="fsp-section-${pi}">${fs.section.toFixed(2)}</span>
        <button class="fsp-btn" onclick="adjustFontSize(${pi},'section',0.05)">+</button>
      </div>
      <div class="fsp-row">
        <span class="fsp-label">Items</span>
        <button class="fsp-btn" onclick="adjustFontSize(${pi},'item',-0.05)">−</button>
        <span class="fsp-val" id="fsp-item-${pi}">${fs.item.toFixed(2)}</span>
        <button class="fsp-btn" onclick="adjustFontSize(${pi},'item',0.05)">+</button>
      </div>
    `;
    wrapperEl.appendChild(fspEl);

    // Header
    pageEl.innerHTML = buildHeaderHTML(pi, page.title, page.tagline);

    // All sections and items for this page
    page.sections.forEach((sec, si) => {
      const secEl = document.createElement('div');
      secEl.className = 'menu-section';

      const shEl = document.createElement('div');
      shEl.innerHTML = buildSectionHeaderHTML(pi, si, sec);
      secEl.appendChild(shEl);

      sec.items.forEach((item, ii) => {
        const iEl = document.createElement('div');
        iEl.innerHTML = buildItemHTML(pi, si, ii, item);
        secEl.appendChild(iEl);
      });

      const addItemEl = document.createElement('div');
      addItemEl.innerHTML = `<div class="add-item-row"><button class="add-item-btn" onclick="openAddItem(${pi},${si})">+ Add Item</button></div>`;
      secEl.appendChild(addItemEl);

      const ctrlEl = document.createElement('div');
      ctrlEl.innerHTML = `<div class="edit-controls-section">
        <button class="ec-btn" onclick="openEditSection(${pi},${si})">✎ Edit</button>
        <button class="ec-btn" onclick="moveSection(${pi},${si},-1)">▲ Move Up</button>
        <button class="ec-btn" onclick="moveSection(${pi},${si},1)">▼ Move Down</button>
        <button class="ec-btn del" onclick="deleteSection(${pi},${si})">✕ Delete Section</button>
      </div>`;
      secEl.appendChild(ctrlEl);

      pageEl.appendChild(secEl);
    });

    const addSecEl = document.createElement('div');
    addSecEl.innerHTML = `<div class="add-section-row"><button class="add-item-btn" onclick="openAddSection(${pi})">+ Add Section to This Page</button></div>`;
    pageEl.appendChild(addSecEl);

    // Footer
    const footer = document.createElement('div');
    footer.className = 'menu-footer';
    footer.innerHTML = `<span class="footer-date">Page ${pi + 1} of ${menuData.pages.length}</span>`;
    pageEl.appendChild(footer);

    wrapperEl.appendChild(pageEl);
    wrap.appendChild(wrapperEl);
  });

  // ── AUTO-SCALE page 3 (pi=2): shrink font size until content fits
  autoscalePage(2);

  // Apply any manual font size overrides
  applyFontSizeStyles();

  // ── STEP 5: Global "Add New Page" button
  const addPageDiv = document.createElement('div');
  addPageDiv.style.cssText = 'text-align:center;padding:20px 0;';
  addPageDiv.innerHTML = `<button class="add-item-btn" style="font-size:0.8rem;padding:8px 24px;" onclick="addPage()">+ Add New Page</button>`;
  wrap.appendChild(addPageDiv);
}

function autoscalePage(pi) {
  const CONTENT_HEIGHT = 1056 - 96 * 2 - 32; // PAGE - padding - footer = 832px
  const pageEl = document.querySelector(`.page-pi-${pi}`);
  if (!pageEl) return;

  // Remove any previous autoscale style for this page
  const styleId = `autoscale-pi-${pi}`;
  let styleEl = document.getElementById(styleId);
  if (styleEl) styleEl.remove();

  // Measure only printable content — exclude editor buttons
  function printableHeight() {
    let h = 0;
    pageEl.querySelectorAll('.menu-header, .section-header, .menu-item').forEach(el => h += el.offsetHeight);
    return h;
  }

  // Check if it already fits
  if (printableHeight() <= CONTENT_HEIGHT) return;

  // Binary search for the largest font scale that fits
  let lo = 0.5, hi = 1.0;
  for (let i = 0; i < 20; i++) {
    const mid = (lo + hi) / 2;
    applyPageScale(pi, mid);
    pageEl.offsetHeight; // force reflow
    if (printableHeight() <= CONTENT_HEIGHT) lo = mid;
    else hi = mid;
  }
  applyPageScale(pi, lo);
}

function applyPageScale(pi, scale) {
  const styleId = `autoscale-pi-${pi}`;
  let styleEl = document.getElementById(styleId);
  if (!styleEl) {
    styleEl = document.createElement('style');
    styleEl.id = styleId;
    document.head.appendChild(styleEl);
  }
  styleEl.textContent = `
    .page-pi-${pi} .item-name         { font-size: ${(0.86 * scale).toFixed(3)}rem !important; }
    .page-pi-${pi} .item-desc         { font-size: ${(0.76 * scale).toFixed(3)}rem !important; }
    .page-pi-${pi} .item-size         { font-size: ${(0.76 * scale).toFixed(3)}rem !important; }
    .page-pi-${pi} .item-price        { font-size: ${(0.86 * scale).toFixed(3)}rem !important; }
    .page-pi-${pi} .section-name      { font-size: ${(1.05 * scale).toFixed(3)}rem !important; }
    .page-pi-${pi} .section-subtitle  { font-size: ${(0.72 * scale).toFixed(3)}rem !important; }
    .page-pi-${pi} .menu-item         { padding: ${Math.max(1, Math.round(5 * scale))}px 0 !important; }
    .page-pi-${pi} .menu-section      { margin-bottom: ${Math.max(4, Math.round(20 * scale))}px !important; }
    .page-pi-${pi} .menu-header       { margin-bottom: ${Math.max(4, Math.round(20 * scale))}px !important; }
  `;
}



// ─── FONT SIZE CONTROLS ──────────────────────────────────────────────────────
const DEFAULT_FONT_SIZES = {
  0: { section: 0.88, item: 0.76 },
  1: { section: 0.88, item: 0.76 },
  2: { section: 1.05, item: 0.86 },
  3: { section: 1.10, item: 0.92 },
};

function getFontSizes(pi) {
  if (!menuData.pages[pi].fontSizes) {
    const d = DEFAULT_FONT_SIZES[pi] || { section: 1.0, item: 0.82 };
    menuData.pages[pi].fontSizes = { section: d.section, item: d.item };
  }
  return menuData.pages[pi].fontSizes;
}

function adjustFontSize(pi, type, delta) {
  const fs = getFontSizes(pi);
  fs[type] = Math.round((fs[type] + delta) * 100) / 100;
  fs[type] = Math.max(0.5, Math.min(2.0, fs[type]));
  // Update display values without full re-render
  const secEl = document.getElementById(`fsp-section-${pi}`);
  const itemEl = document.getElementById(`fsp-item-${pi}`);
  if (secEl) secEl.textContent = fs.section.toFixed(2);
  if (itemEl) itemEl.textContent = fs.item.toFixed(2);
  applyFontSizeStyles();
  scheduleSave();
}

function applyFontSizeStyles() {
  let styleEl = document.getElementById('font-size-overrides');
  if (!styleEl) {
    styleEl = document.createElement('style');
    styleEl.id = 'font-size-overrides';
    document.head.appendChild(styleEl);
  }
  const css = menuData.pages.map((page, pi) => {
    const fs = getFontSizes(pi);
    return `
      .page-pi-${pi} .section-name     { font-size: ${fs.section.toFixed(3)}rem !important; }
      .page-pi-${pi} .section-subtitle { font-size: ${(fs.section * 0.72).toFixed(3)}rem !important; }
      .page-pi-${pi} .item-name        { font-size: ${fs.item.toFixed(3)}rem !important; }
      .page-pi-${pi} .item-price       { font-size: ${fs.item.toFixed(3)}rem !important; }
      .page-pi-${pi} .item-desc        { font-size: ${(fs.item * 0.88).toFixed(3)}rem !important; }
      .page-pi-${pi} .item-size        { font-size: ${(fs.item * 0.88).toFixed(3)}rem !important; }
    `;
  }).join('\n');
  styleEl.textContent = css;
}

// ─── CRUD OPERATIONS ─────────────────────────────────────────────────────────
function deleteItem(pi, si, ii) {
  if (!confirm('Remove this item?')) return;
  menuData.pages[pi].sections[si].items.splice(ii, 1);
  render(); scheduleSave();
}

function toggleItemHidden(pi, si, ii) {
  const item = menuData.pages[pi].sections[si].items[ii];
  item.hidden = !item.hidden;
  render(); scheduleSave();
}

// Called from the modal's Hide/Show button
function modalToggleHidden() {
  if (modalMode !== 'edit-item') return;
  const { pi, si, ii } = modalCtx;
  const item = menuData.pages[pi].sections[si].items[ii];
  item.hidden = !item.hidden;
  closeModal(); render(); scheduleSave();
}

// Called from the modal's Delete button
function modalDeleteItem() {
  if (modalMode !== 'edit-item') return;
  const { pi, si, ii } = modalCtx;
  if (!confirm('Permanently delete this item?')) return;
  menuData.pages[pi].sections[si].items.splice(ii, 1);
  closeModal(); render(); scheduleSave();
}

function moveItem(pi, si, ii, dir) {
  const arr = menuData.pages[pi].sections[si].items;
  const ni = ii + dir;
  if (ni < 0 || ni >= arr.length) return;
  [arr[ii], arr[ni]] = [arr[ni], arr[ii]];
  render(); scheduleSave();
}

function deleteSection(pi, si) {
  if (!confirm('Delete this entire section?')) return;
  menuData.pages[pi].sections.splice(si, 1);
  render(); scheduleSave();
}

function moveSection(pi, si, dir) {
  const arr = menuData.pages[pi].sections;
  const ni = si + dir;
  if (ni < 0 || ni >= arr.length) return;
  [arr[si], arr[ni]] = [arr[ni], arr[si]];
  render(); scheduleSave();
}

function addPage() {
  menuData.pages.push({ title: "New Menu Page", tagline: "Add tagline here", sections: [] });
  render(); scheduleSave();
}

// ─── MODAL ───────────────────────────────────────────────────────────────────
function openAddItem(pi, si) {
  modalMode = 'add-item';
  modalCtx = { pi, si };
  document.getElementById('modal-title').innerText = 'Add New Item';
  document.getElementById('modal-save-btn').innerText = 'Add Item';
  document.getElementById('modal-hide-btn').style.display = 'none';
  document.getElementById('modal-delete-btn').style.display = 'none';
  clearForm();
  document.getElementById('modal').classList.add('open');
}

function openEditItem(pi, si, ii) {
  modalMode = 'edit-item';
  modalCtx = { pi, si, ii };
  const item = menuData.pages[pi].sections[si].items[ii];
  document.getElementById('modal-title').innerText = 'Edit Item';
  document.getElementById('modal-save-btn').innerText = 'Save Changes';
  // Show hide/delete buttons for existing items
  const hideBtn = document.getElementById('modal-hide-btn');
  hideBtn.style.display = '';
  hideBtn.innerText = item.hidden ? '👁 Show Item' : '👁 Hide Item';
  document.getElementById('modal-delete-btn').style.display = '';
  document.getElementById('f-name').value = item.name || '';

  // If item has explicit style/abv fields use them, otherwise try parsing legacy desc
  let style = item.style || '';
  let abv = item.abv || '';
  let desc = item.desc || '';
  if (!style && !abv && desc) {
    // Legacy format: "Style Name • 6.1% ABV" or "Style Name \u2022 6.1% ABV"
    const abvMatch = desc.match(/•\s*([\d.]+)%\s*ABV/);
    if (abvMatch) {
      abv = abvMatch[1];
      style = desc.replace(/\s*•\s*[\d.]+%\s*ABV/, '').trim();
      desc = '';
    }
  }

  document.getElementById('f-desc').value = desc;
  document.getElementById('f-style').value = style;
  document.getElementById('f-abv').value = abv ? abv + '%' : '';
  document.getElementById('f-size').value = item.size || 'Serving';
  document.getElementById('f-price').value = item.price || '';
  document.getElementById('f-size2').value = item.size2 || 'Serving';
  document.getElementById('f-price2').value = item.price2 || '';
  const hasDual = !!(item.size2 && item.price2);
  document.getElementById('f-dual').checked = hasDual;
  document.getElementById('f-price2-row').style.display = hasDual ? '' : 'none';
  document.getElementById('f-price2-val-row').style.display = hasDual ? '' : 'none';
  document.getElementById('f-new').checked = !!(item.isNew);
  document.getElementById('modal').classList.add('open');
}

function openEditSection(pi, si) {
  modalMode = 'edit-section';
  modalCtx = { pi, si };
  const sec = menuData.pages[pi].sections[si];
  document.getElementById('modal-title').innerText = 'Edit Section';
  document.getElementById('modal-save-btn').innerText = 'Save Section';
  document.getElementById('modal-hide-btn').style.display = 'none';
  document.getElementById('modal-delete-btn').style.display = 'none';
  clearForm();
  document.getElementById('f-name').value = sec.name || '';
  document.getElementById('f-desc').value = sec.subtitle || '';
  document.getElementById('modal').classList.add('open');
}

function openAddSection(pi) {
  modalMode = 'add-section';
  modalCtx = { pi };
  document.getElementById('modal-title').innerText = 'Add New Section';
  document.getElementById('modal-save-btn').innerText = 'Add Section';
  document.getElementById('modal-hide-btn').style.display = 'none';
  document.getElementById('modal-delete-btn').style.display = 'none';
  clearForm();
  // Repurpose name field as section name
  document.querySelector('label[for]');
  document.getElementById('modal').classList.add('open');
}

function clearForm() {
  ['f-name','f-desc','f-style','f-abv','f-price','f-price2'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('f-size').value = 'Serving';
  document.getElementById('f-size2').value = 'Serving';
  document.getElementById('f-dual').checked = false;
  document.getElementById('f-new').checked = false;
  document.getElementById('f-price2-row').style.display = 'none';
  document.getElementById('f-price2-val-row').style.display = 'none';
}

function formatPriceInput(el) {
  const cursorPos = el.selectionStart;
  const oldVal = el.value;
  let val = oldVal.replace(/[^0-9.]/g, '');
  if (!val) { el.value = ''; return; }
  el.value = '$' + val;
  // Keep cursor offset by 1 for the $ sign
  const hadDollar = oldVal.startsWith('$');
  const newPos = hadDollar ? cursorPos : cursorPos + 1;
  el.setSelectionRange(Math.min(newPos, el.value.length), Math.min(newPos, el.value.length));
}

function formatAbvInput(el) {
  const cursorPos = el.selectionStart;
  const oldVal = el.value;
  let val = oldVal.replace(/[^0-9.]/g, '');
  if (!val) { el.value = ''; return; }
  el.value = val + '%';
  // Keep cursor before the % sign
  const newPos = Math.min(cursorPos, val.length);
  el.setSelectionRange(newPos, newPos);
}

function toggleDual() {
  const show = document.getElementById('f-dual').checked;
  document.getElementById('f-price2-row').style.display = show ? '' : 'none';
  document.getElementById('f-price2-val-row').style.display = show ? '' : 'none';
}

function closeModal() {
  document.getElementById('modal').classList.remove('open');
}

function saveModal() {
  if (modalMode === 'edit-section') {
    const sec = menuData.pages[modalCtx.pi].sections[modalCtx.si];
    sec.name = document.getElementById('f-name').value.trim() || sec.name;
    sec.subtitle = document.getElementById('f-desc').value.trim();
    closeModal(); render(); scheduleSave(); return;
  }

  if (modalMode === 'add-section') {
    const name = document.getElementById('f-name').value.trim() || 'New Section';
    const subtitle = document.getElementById('f-desc').value.trim();
    menuData.pages[modalCtx.pi].sections.push({ name, subtitle, items: [] });
    closeModal(); render(); scheduleSave(); return;
  }

  const rawPrice = document.getElementById('f-price').value.trim();
  const rawAbv = document.getElementById('f-abv').value.trim();
  const item = {
    name: document.getElementById('f-name').value.trim() || 'Item Name',
    desc: document.getElementById('f-desc').value.trim(),
    style: document.getElementById('f-style').value.trim(),
    abv: rawAbv.replace(/[^0-9.]/g, ''),
    size: document.getElementById('f-size').value.trim() || 'Serving',
    price: rawPrice ? (rawPrice.startsWith('$') ? rawPrice : '$' + rawPrice) : '$0.00',
    isNew: document.getElementById('f-new').checked,
  };
  if (document.getElementById('f-dual').checked) {
    const rawPrice2 = document.getElementById('f-price2').value.trim();
    item.size2 = document.getElementById('f-size2').value.trim();
    item.price2 = rawPrice2 ? (rawPrice2.startsWith('$') ? rawPrice2 : '$' + rawPrice2) : '';
  }

  if (modalMode === 'add-item') {
    menuData.pages[modalCtx.pi].sections[modalCtx.si].items.push(item);
  } else if (modalMode === 'edit-item') {
    menuData.pages[modalCtx.pi].sections[modalCtx.si].items[modalCtx.ii] = item;
  }

  closeModal(); render(); scheduleSave();
}

function addSection() {
  if (menuData.pages.length === 0) addPage();
  openAddSection(menuData.pages.length - 1);
}

// ─── VENUE NAME SYNC ─────────────────────────────────────────────────────────
document.getElementById('venue-name-input').addEventListener('input', function() {
  menuData.venue = this.value;
  render(); scheduleSave();
});

// ─── CLOUD CONFIGURATION ─────────────────────────────────────────────────────
const SCRIPT_URL_KEY = 'baldbirdsmenu_script_url';
let autoSaveTimer = null;
let isSaving = false;
let pendingSave = false;

function getScriptUrl() {
  return localStorage.getItem(SCRIPT_URL_KEY) || '';
}

function saveScriptUrl() {
  const url = document.getElementById('script-url-input').value.trim();
  if (!url.startsWith('https://script.google.com/')) {
    alert('Please enter a valid Google Apps Script URL (starts with https://script.google.com/)');
    return;
  }
  localStorage.setItem(SCRIPT_URL_KEY, url);
  document.getElementById('setup-banner').style.display = 'none';
  setStatus('⏳ Connecting — loading menu from Drive…', '#c9a84c');
  loadFromCloud();
}

function setStatus(msg, color) {
  const el = document.getElementById('cloud-status');
  el.innerText = msg;
  el.style.color = color || '#888';
}

// ─── AUTO-SAVE (debounced) ────────────────────────────────────────────────────
// Called after every change. Waits 1.5s of inactivity before sending to Drive.
function scheduleSave() {
  if (!getScriptUrl()) return;
  setStatus('✏️ Unsaved changes…', '#c9a84c');
  clearTimeout(autoSaveTimer);
  autoSaveTimer = setTimeout(flushSave, 1500);
}

async function flushSave() {
  const url = getScriptUrl();
  if (!url) return;

  // If a save is already in flight, queue another one for when it finishes
  if (isSaving) { pendingSave = true; return; }

  isSaving = true;
  setStatus('☁ Saving…', '#c9a84c');

  try {
    const resp = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'text/plain' },
      body: JSON.stringify(menuData, null, 2)
    });
    const result = await resp.json();
    if (result.status === 'ok') {
      const ts = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      setStatus(`✅ Saved at ${ts}`, '#7abf7a');
    } else {
      throw new Error(result.error || 'Unknown error');
    }
  } catch (err) {
    setStatus('❌ Auto-save failed — ' + err.message, '#e07070');
    console.error('Auto-save error:', err);
  } finally {
    isSaving = false;
    // If a change came in while we were saving, save again immediately
    if (pendingSave) {
      pendingSave = false;
      flushSave();
    }
  }
}

// ─── AUTO-LOAD on open ────────────────────────────────────────────────────────
async function loadFromCloud() {
  const url = getScriptUrl();
  if (!url) {
    document.getElementById('setup-banner').style.display = 'block';
    setStatus('⚠️ Not connected to Drive', '#c9a84c');
    return;
  }

  setStatus('⏳ Loading from Drive…', '#c9a84c');

  try {
    const resp = await fetch(url + '?action=load');
    const result = await resp.json();
    if (result.status === 'ok' && result.data) {
      menuData = result.data;
      render();
      const ts = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
      setStatus(`✅ Loaded at ${ts}`, '#7abf7a');
    } else if (result.status === 'empty') {
      setStatus('ℹ️ No saved menu yet — changes will save automatically', '#c9a84c');
    } else {
      throw new Error(result.error || 'Unknown error');
    }
  } catch (err) {
    setStatus('❌ Load failed — ' + err.message, '#e07070');
    console.error('Load error:', err);
  }
}

// ─── LOCAL BACKUP / RESTORE ──────────────────────────────────────────────────
function exportJSON() {
  const blob = new Blob([JSON.stringify(menuData, null, 2)], { type: 'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'menu-data-backup.json';
  a.click();
}

function importJSON(event) {
  const file = event.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = e => {
    try {
      menuData = JSON.parse(e.target.result);
      render();
      scheduleSave();
    } catch { alert('Invalid JSON file.'); }
  };
  reader.readAsText(file);
  event.target.value = '';
}

// Close modal on overlay click
document.getElementById('modal').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});

// ─── INIT ─────────────────────────────────────────────────────────────────────
// Auto-load on page open; show setup banner if not yet configured
loadFromCloud();
</script>
</body>
</html>
