<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Weekly Project Count Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@600;700;800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#1c2430;
    --paper:#fdfdfd;
    --line:#e1ddd2;
    --accent:#0f766e;
    --accent2:#7c3aed;
    --accent-soft:#e6f4f2;
    --gold:#c8933f;
    --grp-coastline:#ffffff;
  }
  *{box-sizing:border-box;}
  html,body{height:100%;}
  body{
    margin:0;
    font-family:'Inter',-apple-system,'Segoe UI',Helvetica,Arial,sans-serif;
    color:var(--ink);
    background-color:var(--paper);
    background-image:
      radial-gradient(circle at 10% 10%, rgba(124,58,237,0.10) 0%, rgba(124,58,237,0) 38%),
      radial-gradient(circle at 90% 6%, rgba(200,147,63,0.16) 0%, rgba(200,147,63,0) 40%),
      radial-gradient(circle at 88% 92%, rgba(15,118,110,0.14) 0%, rgba(15,118,110,0) 45%),
      radial-gradient(circle at 4% 85%, rgba(15,118,110,0.10) 0%, rgba(15,118,110,0) 40%),
      linear-gradient(120deg, rgba(28,36,48,0.045) 1px, transparent 1px),
      linear-gradient(30deg, rgba(28,36,48,0.045) 1px, transparent 1px);
    background-size: auto, auto, auto, auto, 30px 30px, 30px 30px;
    background-attachment: fixed;
    background-repeat: no-repeat, no-repeat, no-repeat, no-repeat, repeat, repeat;
  }
  #app{
    padding:0 24px 60px;
    max-width:100%;
    margin:0;
  }
  .toolbar, .table-wrap{
    box-shadow:0 8px 24px rgba(28,36,48,0.08);
  }
  .hero{
    margin:0 -24px 22px;
    padding:26px 30px 30px;
    border-radius:0 0 22px 22px;
    background:linear-gradient(120deg,#0f766e 0%,#125e70 45%,#3a2d6e 100%);
    color:#fff;
    box-shadow:0 14px 30px rgba(15,50,50,0.28);
    position:relative;
    overflow:hidden;
  }
  .hero::after{
    content:"";
    position:absolute; inset:0;
    background-image:radial-gradient(circle, rgba(255,255,255,0.16) 1.5px, transparent 1.5px);
    background-size:16px 16px;
    opacity:.35;
    pointer-events:none;
  }
  .hero-inner{position:relative; z-index:1;}
  h1{
    display:flex;
    align-items:center;
    gap:12px;
    font-family:'Poppins',sans-serif;
  }
  h1::before{
    content:"";
    width:30px;height:30px;
    display:inline-block;
    background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'><rect x='3' y='3' width='18' height='18' rx='5' fill='white' fill-opacity='0.95'/><path d='M7 9h10M7 13h10M7 17h6' stroke='%230f766e' stroke-width='1.8' stroke-linecap='round'/></svg>");
    background-size:contain;
    background-repeat:no-repeat;
    filter:drop-shadow(0 2px 4px rgba(0,0,0,.15));
  }
  h1{
    font-size:24px;
    letter-spacing:.2px;
    margin:0 0 6px;
    font-weight:700;
  }
  .sub{color:rgba(255,255,255,0.85);font-size:13.5px;margin-bottom:0;}

  .toolbar{
    display:flex;
    flex-wrap:wrap;
    align-items:center;
    gap:10px;
    margin:-30px 0 18px;
    position:relative;
    z-index:2;
    background:rgba(255,255,255,0.82);
    backdrop-filter:blur(10px);
    border:1px solid rgba(255,255,255,0.6);
    border-radius:14px;
    padding:12px 16px;
  }
  .toolbar label{font-size:12px;color:#555;margin-right:4px;font-weight:600;}
  .toolbar select, .toolbar button, .toolbar input[type=text]{
    font-size:13px;
    padding:7px 12px;
    border-radius:8px;
    border:1px solid var(--line);
    background:#fff;
    color:var(--ink);
  }
  .toolbar button{
    cursor:pointer;
    background:linear-gradient(135deg,var(--accent),#0a3d3a);
    color:#fff;
    border:none;
    font-weight:600;
    box-shadow:0 3px 8px rgba(15,118,110,0.3);
    transition:transform .12s ease, box-shadow .12s ease;
  }
  .toolbar button.secondary{
    background:#fff;
    color:var(--ink);
    border:1px solid var(--line);
    box-shadow:none;
    font-weight:500;
  }
  .toolbar button.savebtn{
    background:linear-gradient(135deg,var(--gold),#a9762a);
    box-shadow:0 3px 8px rgba(200,147,63,0.35);
  }
  .toolbar button:hover{transform:translateY(-1px); box-shadow:0 6px 14px rgba(15,118,110,0.32);}
  .toolbar button.secondary:hover{box-shadow:0 3px 8px rgba(28,36,48,0.12);}
  .spacer{flex:1;}
  #status{font-size:12px;color:var(--accent);min-width:170px;font-weight:600;}
  #status.err{color:#b91c1c;}

  .table-wrap{
    overflow:auto;
    max-height:74vh;
    width:100%;
    border:1px solid var(--line);
    border-radius:14px;
    background:#fff;
  }
  table{border-collapse:separate;border-spacing:0;font-size:12px;width:max-content;}
  th,td{border-bottom:1px solid var(--line);border-right:1px solid var(--line);padding:0;white-space:nowrap;}

  thead th{
    position:sticky;top:0;
    background:linear-gradient(180deg,#232f3d,#1c2430);
    color:#fff;
    padding:9px 6px;
    font-weight:600;
    z-index:3;
    text-align:center;
    font-family:'Poppins',sans-serif;
    font-size:11.5px;
  }
  thead th.name-col{
    left:0;position:sticky;z-index:4;
    text-align:left;padding-left:10px;
    min-width:280px;
  }
  th.total-col{background:linear-gradient(180deg,#0f766e,#0a3d3a);}

  td.name-cell{
    position:sticky;left:0;z-index:2;
    padding:5px 8px 5px 10px;
    font-size:12.5px;
    min-width:280px;
    max-width:340px;
    overflow:hidden;text-overflow:ellipsis;
    display:flex;align-items:center;justify-content:space-between;gap:6px;
    background:inherit;
  }
  td.name-cell span.label{overflow:hidden;text-overflow:ellipsis;}
  td.name-cell .name-edit-form{display:flex;gap:4px;align-items:center;width:100%;}
  td.name-cell .name-edit-form input{
    font-size:11.5px;
    padding:3px 5px;
    border:1px solid var(--accent);
    border-radius:4px;
    width:100%;
  }
  td.name-cell .name-edit-form input.group-input{max-width:70px;flex:0 0 auto;}
  td.name-cell .name-edit-form button{
    border:none;background:var(--accent);color:#fff;border-radius:4px;
    font-size:10px;padding:3px 6px;cursor:pointer;flex:0 0 auto;
  }
  td.name-cell .name-edit-form button.cancel{background:#999;}
  td.name-cell button.pmode{
    border:none;background:transparent;cursor:pointer;
    font-size:12px;opacity:.4;flex:0 0 auto;
  }
  td.name-cell button.pmode.active{opacity:1;}
  td.name-cell button.edit{
    border:none;background:transparent;cursor:pointer;opacity:.45;
    font-size:12px;flex:0 0 auto;
  }
  td.name-cell button.edit:hover{opacity:1;}
  td.name-cell button.del{
    border:none;background:transparent;cursor:pointer;color:#b91c1c;opacity:.5;
    font-size:12px;flex:0 0 auto;
  }
  td.name-cell button.del:hover{opacity:1;}

  tr.group-header td{
    position:sticky;left:0;
    font-weight:700;
    font-size:12px;
    text-transform:uppercase;
    letter-spacing:.5px;
    padding:8px 10px 8px 14px;
    background:linear-gradient(90deg,#eae7de,#f3f1ea);
    color:#3a3a3a;
    cursor:pointer;
    z-index:2;
    border-left:4px solid var(--accent);
  }
  tr.group-header.collapsed td{opacity:.7;}
  tr.group-header:hover td{background:linear-gradient(90deg,#e2ded2,#eeebe1);}


  td.datacell{padding:2px;text-align:center;}
  td.datacell input{
    width:44px;
    border:1px solid transparent;
    background:transparent;
    text-align:center;
    font-size:12px;
    padding:5px 2px;
    border-radius:4px;
    color:var(--ink);
  }
  td.datacell.pmode input{width:30px;}
  td.datacell.pmode .pwrap{display:flex;align-items:center;gap:2px;justify-content:center;}
  td.datacell.pmode .pwrap span{color:#9aa0a6;font-size:10px;}
  td.datacell input:focus{
    border-color:var(--accent);
    background:#fff;
    outline:none;
  }
  td.datacell input:hover{background:#fbfbfa;}

  td.total-cell{
    text-align:center;font-weight:700;
    background:linear-gradient(180deg,#e6f4f2,#d9f0ec);
    color:#0a3d3a;
    padding:6px 8px;
  }

  tr.subtotal td{
    font-weight:700;background:linear-gradient(90deg,#efece3,#f5f3ec);border-top:2px solid var(--gold);
    color:#333;
  }
  tr.subtotal td.name-cell{background:linear-gradient(90deg,#efece3,#f5f3ec);}
  tr.subtotal td.total-cell{background:linear-gradient(180deg,#c8933f,#a9762a); color:#fff;}

  .weekend{background:#00000006;}

  .footer-note{font-size:11.5px;color:#8a8a8a;margin-top:10px;line-height:1.5;}
  .add-row-form{display:flex;gap:6px;align-items:center;}
  .add-row-form select, .add-row-form input{font-size:12px;}
</style>
</head>
<body>
<div id="app">
  <div class="hero">
    <div class="hero-inner">
      <h1>Weekly Project Count Tracker</h1>
      <div class="sub">Enter counts per project, per day. Data saves automatically and is private to you.</div>
    </div>
  </div>

  <div id="storageBanner" style="display:none;background:#fde8e8;border:1px solid #f3b4b4;color:#7a1f1f;border-radius:10px;padding:10px 14px;margin:-30px 0 18px;position:relative;z-index:2;font-size:12.5px;line-height:1.5;">
    ⚠️ Auto-save to Claude's server isn't reachable in this view, so entries only live in this browser tab and will be lost if you close it. Use <strong>Download backup</strong> often, and if you reopen this later, use <strong>Restore backup</strong> to load your data back in.
  </div>

  <div class="toolbar">
    <label>Month</label>
    <select id="monthSel"></select>
    <label>Year</label>
    <select id="yearSel"></select>

    <label style="margin-left:14px;">From day</label>
    <select id="fromDaySel"></select>
    <label>To day</label>
    <select id="toDaySel"></select>
    <button class="secondary" id="applyFilterBtn">Apply filter</button>
    <button class="secondary" id="resetFilterBtn">Show full month</button>

    <div class="spacer"></div>

    <div class="add-row-form">
      <input type="text" id="newRowGroup" list="existingGroups" placeholder="Project ID (e.g. 1136934)" style="width:170px;">
      <datalist id="existingGroups"></datalist>
      <input type="text" id="newRowName" placeholder="New project name" style="width:180px;">
      <button class="secondary" id="addRowBtn">+ Add row</button>
    </div>

    <button class="savebtn" id="saveNowBtn">💾 Save</button>
    <button class="secondary" id="exportBtn">Export CSV</button>
    <button class="secondary" id="backupBtn">⬇ Download backup</button>
    <button class="secondary" id="restoreBtn">⬆ Restore backup</button>
    <input type="file" id="restoreFile" accept="application/json" style="display:none;">
    <button class="secondary" id="clearMonthBtn">Clear month</button>
    <span id="status"></span>
  </div>

  <div class="table-wrap">
    <table id="mainTable"></table>
  </div>

  <div class="footer-note">
    📄 toggles between a plain count and a count + pages format. ✎ edits a process's name and/or Project ID. Click a group heading to collapse it. Subtotal rows only appear for a Project ID with 2 or more processes. Entries auto-save a moment after you type; click 💾 Save to store immediately.
  </div>
</div>

<script>
(function(){
  // Pastel palette cycled across project-ID groups, in the order groups first appear
  const PALETTE = ['#ffffff','#e8f1ff','#fdeaf3','#eee6fb','#eaf7ee','#fff4e0','#e5f6f7','#f6f1e6','#fbe9e7','#eef1ec'];

  const DEFAULT_ROWS = [
    ['1136934 - Coastline Loggins','1136934'],
    ['1136934 - Coastline LPR Mails','1136934'],
    ['1136934 - Coastline LPR Gmail','1136934'],
    ['1136934 - Coastline Hold & Close','1136934'],
    ['1136934 - Coastline Approved Transport & Signed Delivery Documents','1136934'],
    ['1136934 - Coastline Blue Notes','1136934'],
    ['1136934 - Coastline Cars Arrive','1136934'],
    ['1136934 - Coastline CDispatch','1136934'],
    ['1136934 - Coastline Run Buggy','1136934'],
    ['1136934 - Cleardata Lot & Personal Property','1136934'],
    ['1136934 - Coastline Info Mail','1136934'],
    ['1136934 - Coastline Key Approved','1136934'],
    ['1136934 - Coastline Key Receipt','1136934'],
    ['1136934 - Coastline Key Request','1136934'],
    ['1136934 - Coastline Scheduled Property Appointments','1136934'],
    ['1136934 - Coastline Scheduling Reedem Appointments','1136934'],
    ['1136934 - Coastline Scheduling Transport on Cleardata','1136934'],
    ['1136934 - Coastline Approved Property','1136934'],
    ['1136934 - Coastline Approved Redemption','1136934'],
    ['1136934 - Coastline Double Posting','1136934'],
    ['1136934 - Coastline IBEAM Portal','1136934'],
    ['1136934 - Coastline IBeam Update','1136934'],
    ['1136934 - Coastline Generic Updates','1136934'],

    ['Recovery Global Clients','Recovery Global Clients'],

    ['1792317 / 1008087 - Data Entry Services (CVs)','1792317 / 1008087'],
    ['1032632 - Andrew Welsh','1032632',true],
    ['1197801 - Nicki - CV Formatting','1197801',true],
    ['1136986 - Ben Resume','1136986',true],
    ['1788166 - Jack Resume','1788166',true],
    ['1801123 - Brendan CV Formatting','1801123',true],
    ['1326095 - Resume Formatting Services (CG)','1326095',true],
    ['1348906 - Resume Formatting Services (Rev1)','1348906',true],

    ['1168734 - HS Recovery Invoice','1168734'],
    ['1168734 - HS Recovery_ALS','1168734'],
    ['1168734 - HS Recovery_RDN','1168734'],
    ['WTAR - Recovery Services','WTAR'],
    ['1520266 - City Recovery Agency','1520266'],
    ['1136956 - Allstar - updates','1136956'],
    ['1025833 - Allcar - Updates - Data Entry Operator for Alcar','1025833'],

    ['1368472 - Stampmore - Order Processing','1368472'],
    ['1790539 - Data Validation','1790539'],
    ['1272527 - PDF Extraction from Websites','1272527'],
    ['1363321 - Simple creation of vCards','1363321'],
    ['1091089 - Image Resize','1091089'],
    ['1086443 - BuildingInfo - VA','1086443'],
    ['1086443 - BuildingInfo - VA - Commencement','1086443'],
    ['1086443 - BuildingInfo - VA - Material','1086443'],

    ['1028812 - Web Research Requirement','1028812'],
    ['1041850 - Tim Cook - Tuesday','1041850'],
    ['1346522 - Data Entry Jobs','1346522'],
    ['1041850 - Tim Cook - UK & Ireland','1041850'],
    ['1788302 - Tim Cook - France Belgium Luxemburg','1041850'],
    ['1788302 - Tim Cook - Germany Austria Switzerland','1041850'],
  
  ];

  function slug(name){
    return name.toLowerCase().replace(/[^a-z0-9]+/g,'-').replace(/(^-|-$)/g,'');
  }

  // Returns the ordered list of distinct project-ID groups, in first-seen order
  function orderedGroups(){
    const seen = [];
    const seenSet = new Set();
    ROWS.forEach(r=>{
      if(!seenSet.has(r.group)){ seenSet.add(r.group); seen.push(r.group); }
    });
    return seen;
  }

  function colorForGroup(groupId){
    const idx = orderedGroups().indexOf(groupId);
    return PALETTE[idx % PALETTE.length];
  }

  let ROWS = DEFAULT_ROWS.map(([name,group,pmode])=>({
    id: slug(name), name, group, pmode: !!pmode, custom:false, edited:false
  }));

  let collapsed = {};
  let year, month; // month 0-11
  let filterFrom = 1, filterTo = 31;
  let grid = {}; // rowId -> { day: value }  value = number OR {c,p}
  let saveTimer = null;
  let dirty = false; // true if there are unsaved-to-server changes pending
  let editingRowId = null; // row currently in inline-edit mode

  const monthSel = document.getElementById('monthSel');
  const yearSel = document.getElementById('yearSel');
  const fromDaySel = document.getElementById('fromDaySel');
  const toDaySel = document.getElementById('toDaySel');
  const statusEl = document.getElementById('status');

  function sleep(ms){ return new Promise(res=>setTimeout(res, ms)); }

  // True once we've confirmed window.storage isn't usable in this environment
  // (e.g. the page was opened outside Claude's live artifact pane). In that
  // case retrying is pointless, so we stop hammering it and fall back to
  // local-only mode instead.
  let storageUnavailable = false;

  function storageMissing(){
    return typeof window.storage === 'undefined' || window.storage === null;
  }

  // Retry wrapper around window.storage calls. A thrown error OR a falsy/null
  // result both count as failure and are retried with a short backoff before
  // we give up and surface it to the user. If storage itself is missing
  // (not just erroring), we skip retries entirely — they can't succeed.
  async function withRetry(fn, label, retries=2){
    if(storageMissing()){
      storageUnavailable = true;
      console.error('[storage] '+label+': window.storage is not available in this environment');
      return null;
    }
    let lastErr = null;
    for(let attempt=0; attempt<=retries; attempt++){
      try{
        const res = await fn();
        if(res) return res;
        lastErr = new Error('storage call returned empty result');
      }catch(e){
        lastErr = e;
        if(storageMissing()){
          storageUnavailable = true;
          console.error('[storage] '+label+': window.storage disappeared mid-call');
          return null;
        }
      }
      console.warn('[storage] '+label+' attempt '+(attempt+1)+' failed', lastErr);
      if(attempt < retries) await sleep(300 * (attempt+1));
    }
    console.error('[storage] '+label+' failed after retries', lastErr);
    return null;
  }

  function downloadJSON(obj, filename){
    const blob = new Blob([JSON.stringify(obj, null, 2)], {type:'application/json'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    a.click();
    URL.revokeObjectURL(url);
  }

  function backupAll(){
    const customRows = ROWS.filter(r=>r.custom);
    const pageModeOverrides = {};
    const rowOverrides = {};
    ROWS.forEach(r=>{
      pageModeOverrides[r.id] = r.pmode;
      if(!r.custom && r.edited){
        rowOverrides[r.id] = { name: r.name, group: r.group };
      }
    });
    downloadJSON(
      { config: {customRows, pageModeOverrides, collapsed, rowOverrides}, sheet: grid, sheetKey: sheetKey(year,month) },
      'tracker-backup-'+MONTH_NAMES[month]+'-'+year+'.json'
    );
  }

  function populateDayFilters(){
    const nDays = daysInMonth(year, month);
    fromDaySel.innerHTML = '';
    toDaySel.innerHTML = '';
    for(let d=1; d<=nDays; d++){
      const o1 = document.createElement('option'); o1.value=d; o1.textContent=d;
      fromDaySel.appendChild(o1);
      const o2 = document.createElement('option'); o2.value=d; o2.textContent=d;
      toDaySel.appendChild(o2);
    }
    filterFrom = 1;
    filterTo = nDays;
    fromDaySel.value = filterFrom;
    toDaySel.value = filterTo;
  }

  const MONTH_NAMES = ['January','February','March','April','May','June','July','August','September','October','November','December'];
  MONTH_NAMES.forEach((m,i)=>{
    const o = document.createElement('option');
    o.value=i; o.textContent=m;
    monthSel.appendChild(o);
  });
  const thisYear = new Date().getFullYear();
  for(let y=thisYear-2; y<=thisYear+2; y++){
    const o=document.createElement('option');
    o.value=y; o.textContent=y;
    yearSel.appendChild(o);
  }

  function daysInMonth(y,m){ return new Date(y, m+1, 0).getDate(); }

  function sheetKey(y,m){ return 'sheet:'+y+'-'+String(m+1).padStart(2,'0'); }

  async function loadConfig(){
    try{
      const res = await window.storage.get('config', false);
      if(res && res.value){
        const cfg = JSON.parse(res.value);
        if(cfg.customRows){
          cfg.customRows.forEach(r=>{
            if(!ROWS.find(x=>x.id===r.id)) ROWS.push(r);
          });
        }
        if(cfg.rowOverrides){
          Object.keys(cfg.rowOverrides).forEach(id=>{
            const row = ROWS.find(r=>r.id===id && !r.custom);
            if(row){
              row.name = cfg.rowOverrides[id].name;
              row.group = cfg.rowOverrides[id].group;
              row.edited = true;
            }
          });
        }
        if(cfg.pageModeOverrides){
          Object.keys(cfg.pageModeOverrides).forEach(id=>{
            const row = ROWS.find(r=>r.id===id);
            if(row) row.pmode = cfg.pageModeOverrides[id];
          });
        }
        if(cfg.collapsed) collapsed = cfg.collapsed;
      }
    }catch(e){ /* no config yet */ }
  }

  async function saveConfig(){
    const customRows = ROWS.filter(r=>r.custom);
    const pageModeOverrides = {};
    const rowOverrides = {};
    ROWS.forEach(r=>{
      pageModeOverrides[r.id] = r.pmode;
      if(!r.custom && r.edited){
        rowOverrides[r.id] = { name: r.name, group: r.group };
      }
    });
    const payload = JSON.stringify({customRows, pageModeOverrides, collapsed, rowOverrides});
    const res = await withRetry(
      ()=>window.storage.set('config', payload, false),
      'saveConfig'
    );
    return !!res;
  }

  async function loadSheet(){
    grid = {};
    try{
      const res = await window.storage.get(sheetKey(year,month), false);
      if(res && res.value) grid = JSON.parse(res.value);
    }catch(e){ grid = {}; }
  }

  async function saveSheetNow(){
    const payload = JSON.stringify(grid);
    const key = sheetKey(year,month);
    const res = await withRetry(
      ()=>window.storage.set(key, payload, false),
      'saveSheet:'+key
    );
    return !!res;
  }

  function queueSave(){
    dirty = true;
    statusEl.classList.remove('err');
    statusEl.textContent = 'Unsaved changes…';
    clearTimeout(saveTimer);
    saveTimer = setTimeout(async ()=>{
      const ok = await saveSheetNow();
      if(ok){
        dirty = false;
        statusEl.classList.remove('err');
        statusEl.textContent = 'Saved ✓';
        setTimeout(()=>{ if(statusEl.textContent==='Saved ✓') statusEl.textContent=''; }, 1500);
      } else {
        statusEl.classList.add('err');
        if(storageUnavailable){
          statusEl.textContent = 'Local only — use Download backup';
          showStorageBanner();
        } else {
          statusEl.textContent = 'Save failed — click 💾 Save';
        }
      }
    }, 400);
  }

  function showStorageBanner(){
    const b = document.getElementById('storageBanner');
    if(b) b.style.display = 'block';
  }

  async function saveAllNow(){
    clearTimeout(saveTimer);
    statusEl.classList.remove('err');
    statusEl.textContent = 'Saving…';
    const [sheetOk, configOk] = await Promise.all([saveSheetNow(), saveConfig()]);
    if(sheetOk && configOk){
      dirty = false;
      statusEl.classList.remove('err');
      statusEl.textContent = 'Saved ✓';
      setTimeout(()=>{ if(statusEl.textContent==='Saved ✓') statusEl.textContent=''; }, 1500);
    } else if(storageUnavailable){
      statusEl.classList.add('err');
      statusEl.textContent = 'Local only — use Download backup';
      showStorageBanner();
    } else {
      statusEl.classList.add('err');
      const what = !sheetOk && !configOk ? 'data and settings' : (!sheetOk ? 'entries' : 'settings');
      statusEl.textContent = 'Save failed ('+what+') — try again';
    }
  }

  function getCellValue(rowId, day){
    const r = grid[rowId];
    if(!r) return null;
    return r[day] != null ? r[day] : null;
  }

  function setCellValue(rowId, day, value){
    if(!grid[rowId]) grid[rowId] = {};
    grid[rowId][day] = value;
    queueSave();
  }

  function countOf(val){
    if(val == null) return 0;
    if(typeof val === 'object') return Number(val.c)||0;
    return Number(val)||0;
  }

  function pagesOf(val){
    if(val == null) return 0;
    if(typeof val === 'object') return Number(val.p)||0;
    return 0;
  }

  function rowTotal(rowId){
    let t=0;
    for(let d=filterFrom; d<=filterTo; d++) t += countOf(getCellValue(rowId,d));
    return t;
  }

  function rowTotalPages(rowId){
    let t=0;
    for(let d=filterFrom; d<=filterTo; d++) t += pagesOf(getCellValue(rowId,d));
    return t;
  }

  function groupDayTotal(groupId, day){
    let t=0;
    ROWS.filter(r=>r.group===groupId).forEach(r=>{ t += countOf(getCellValue(r.id,day)); });
    return t;
  }

  function groupGrandTotal(groupId){
    let t=0;
    for(let d=filterFrom; d<=filterTo; d++) t += groupDayTotal(groupId,d);
    return t;
  }

  function isWeekend(y,m,day){
    const dow = new Date(y,m,day).getDay();
    return dow===0 || dow===6;
  }

  function render(){
    const nDays = daysInMonth(year, month);
    const table = document.getElementById('mainTable');
    table.innerHTML = '';

    // HEAD
    const thead = document.createElement('thead');
    const trh = document.createElement('tr');
    const thName = document.createElement('th');
    thName.className='name-col';
    thName.textContent='Process';
    trh.appendChild(thName);
    for(let d=filterFrom; d<=filterTo; d++){
      const th = document.createElement('th');
      th.textContent = d;
      if(isWeekend(year,month,d)) th.style.background = '#33414f';
      trh.appendChild(th);
    }
    const thTotal = document.createElement('th');
    thTotal.className='total-col';
    thTotal.textContent = (filterFrom===1 && filterTo===nDays) ? 'Total' : 'Total (day '+filterFrom+'–'+filterTo+')';
    trh.appendChild(thTotal);
    thead.appendChild(trh);
    table.appendChild(thead);

    // BODY
    const tbody = document.createElement('tbody');

    const groupIds = orderedGroups();

    groupIds.forEach(gid=>{
      const rowsInGroup = ROWS.filter(r=>r.group===gid);
      if(rowsInGroup.length===0) return;
      const bg = colorForGroup(gid);

      const isSingle = rowsInGroup.length < 2;

      if(!isSingle){
        const trg = document.createElement('tr');
        trg.className = 'group-header' + (collapsed[gid] ? ' collapsed':'');
        const tdg = document.createElement('td');
        tdg.colSpan = nDays + 2;
        tdg.textContent = (collapsed[gid] ? '▸ ' : '▾ ') + 'Project ID: ' + gid + '  (' + rowsInGroup.length + ')';
        tdg.addEventListener('click', ()=>{
          collapsed[gid] = !collapsed[gid];
          saveConfig();
          render();
        });
        trg.appendChild(tdg);
        tbody.appendChild(trg);
      }

      if(isSingle || !collapsed[gid]){
        rowsInGroup.forEach(row=>{
          const tr = document.createElement('tr');
          tr.style.background = bg;

          const tdName = document.createElement('td');
          tdName.className='name-cell';
          tdName.style.background = bg;

          if(editingRowId === row.id){
            // Inline edit form: process name + Project ID
            const form = document.createElement('div');
            form.className = 'name-edit-form';

            const nameInput = document.createElement('input');
            nameInput.type = 'text';
            nameInput.value = row.name;
            nameInput.className = 'edit-name-input';

            const groupInput = document.createElement('input');
            groupInput.type = 'text';
            groupInput.value = row.group;
            groupInput.className = 'group-input';
            groupInput.title = 'Project ID';

            const saveBtn = document.createElement('button');
            saveBtn.textContent = '✓';
            saveBtn.title = 'Save';
            saveBtn.addEventListener('click', (e)=>{
              e.stopPropagation();
              const newName = nameInput.value.trim();
              const newGroup = groupInput.value.trim();
              if(newName){
                row.name = newName;
                row.group = newGroup || row.group;
                if(!row.custom) row.edited = true;
                saveConfig();
                populateGroupDatalist();
              }
              editingRowId = null;
              render();
            });

            const cancelBtn = document.createElement('button');
            cancelBtn.textContent = '✕';
            cancelBtn.title = 'Cancel';
            cancelBtn.className = 'cancel';
            cancelBtn.addEventListener('click', (e)=>{
              e.stopPropagation();
              editingRowId = null;
              render();
            });

            nameInput.addEventListener('keydown', (e)=>{
              if(e.key === 'Enter') saveBtn.click();
              if(e.key === 'Escape') cancelBtn.click();
            });
            groupInput.addEventListener('keydown', (e)=>{
              if(e.key === 'Enter') saveBtn.click();
              if(e.key === 'Escape') cancelBtn.click();
            });

            form.appendChild(nameInput);
            form.appendChild(groupInput);
            form.appendChild(saveBtn);
            form.appendChild(cancelBtn);
            tdName.appendChild(form);
            tdName.addEventListener('click', (e)=>e.stopPropagation());

          } else {
            const span = document.createElement('span');
            span.className='label';
            span.textContent = row.name;
            span.title = row.name;
            tdName.appendChild(span);

            const btnGroup = document.createElement('div');
            btnGroup.style.display='flex';
            btnGroup.style.gap='2px';

            const editBtn = document.createElement('button');
            editBtn.className = 'edit';
            editBtn.textContent = '✎';
            editBtn.title = 'Edit process name / Project ID';
            editBtn.addEventListener('click', (e)=>{
              e.stopPropagation();
              editingRowId = row.id;
              render();
            });
            btnGroup.appendChild(editBtn);

            const pBtn = document.createElement('button');
            pBtn.className = 'pmode' + (row.pmode ? ' active':'');
            pBtn.textContent = '📄';
            pBtn.title = 'Toggle count+pages format';
            pBtn.addEventListener('click', (e)=>{
              e.stopPropagation();
              row.pmode = !row.pmode;
              saveConfig();
              render();
            });
            btnGroup.appendChild(pBtn);

            if(row.custom){
              const delBtn = document.createElement('button');
              delBtn.className='del';
              delBtn.textContent='✕';
              delBtn.title='Remove row';
              delBtn.addEventListener('click', (e)=>{
                e.stopPropagation();
                if(confirm('Remove "'+row.name+'"?')){
                  ROWS = ROWS.filter(r=>r.id!==row.id);
                  delete grid[row.id];
                  saveConfig();
                  queueSave();
                  render();
                }
              });
              btnGroup.appendChild(delBtn);
            }

            tdName.appendChild(btnGroup);
          }

          tr.appendChild(tdName);

          for(let d=filterFrom; d<=filterTo; d++){
            const td = document.createElement('td');
            td.className='datacell' + (row.pmode?' pmode':'') + (isWeekend(year,month,d)?' weekend':'');
            const val = getCellValue(row.id, d);

            if(row.pmode){
              const wrap = document.createElement('div');
              wrap.className='pwrap';
              const cInput = document.createElement('input');
              cInput.type='number'; cInput.min='0'; cInput.placeholder='0';
              cInput.value = (val && typeof val==='object') ? (val.c ?? '') : (val ?? '');
              const span2 = document.createElement('span');
              span2.textContent='/';
              const pInput = document.createElement('input');
              pInput.type='number'; pInput.min='0'; pInput.placeholder='pg';
              pInput.value = (val && typeof val==='object') ? (val.p ?? '') : '';

              function commit(){
                const c = cInput.value===''?0:Number(cInput.value);
                const p = pInput.value===''?0:Number(pInput.value);
                if(c===0 && p===0) setCellValue(row.id, d, null);
                else setCellValue(row.id, d, {c,p});
              }
              cInput.addEventListener('change', commit);
              pInput.addEventListener('change', commit);
              wrap.appendChild(cInput);
              wrap.appendChild(span2);
              wrap.appendChild(pInput);
              td.appendChild(wrap);
            } else {
              const input = document.createElement('input');
              input.type='number'; input.min='0'; input.placeholder='';
              input.value = (val==null) ? '' : (typeof val==='object'? (val.c??'') : val);
              input.addEventListener('change', ()=>{
                const v = input.value==='' ? null : Number(input.value);
                setCellValue(row.id, d, v);
              });
              td.appendChild(input);
            }
            tr.appendChild(td);
          }

          const tdTotal = document.createElement('td');
          tdTotal.className='total-cell';
          tdTotal.textContent = row.pmode
            ? (rowTotal(row.id) + ' / ' + rowTotalPages(row.id))
            : rowTotal(row.id);
          tr.appendChild(tdTotal);

          tbody.appendChild(tr);
        });

        // subtotal row — only shown when this Project ID has 2+ processes
        if(rowsInGroup.length >= 2){
          const trSub = document.createElement('tr');
          trSub.className='subtotal';
          const tdSubName = document.createElement('td');
          tdSubName.className='name-cell';
          tdSubName.textContent = 'Project ID ' + gid + ' — subtotal';
          trSub.appendChild(tdSubName);
          for(let d=filterFrom; d<=filterTo; d++){
            const td = document.createElement('td');
            td.style.textAlign='center';
            td.textContent = groupDayTotal(gid, d) || '';
            trSub.appendChild(td);
          }
          const tdSubTotal = document.createElement('td');
          tdSubTotal.className='total-cell';
          tdSubTotal.textContent = groupGrandTotal(gid);
          trSub.appendChild(tdSubTotal);
          tbody.appendChild(trSub);
        }
      }
    });

    table.appendChild(tbody);
  }

  function exportCSV(){
    let lines = [];
    let header = ['Process'];
    for(let d=filterFrom; d<=filterTo; d++) header.push(String(d));
    header.push('Total');
    lines.push(header.join(','));

    orderedGroups().forEach(gid=>{
      const rowsInGroup = ROWS.filter(r=>r.group===gid);
      if(rowsInGroup.length===0) return;
      lines.push('"Project ID: '+gid+'"');
      rowsInGroup.forEach(row=>{
        const cells = [ '"'+row.name.replace(/"/g,'""')+'"' ];
        for(let d=filterFrom; d<=filterTo; d++){
          const val = getCellValue(row.id, d);
          if(val==null) cells.push('');
          else if(typeof val==='object') cells.push((val.c||0)+' ('+(val.p||0)+' Pages)');
          else cells.push(String(val));
        }
        cells.push(String(rowTotal(row.id)));
        lines.push(cells.join(','));
      });
      if(rowsInGroup.length >= 2){
        const subCells = ['"Project ID '+gid+' — subtotal"'];
        for(let d=filterFrom; d<=filterTo; d++) subCells.push(String(groupDayTotal(gid,d)||0));
        subCells.push(String(groupGrandTotal(gid)));
        lines.push(subCells.join(','));
      }
    });

    const blob = new Blob([lines.join('\n')], {type:'text/csv'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'weekly-report-'+MONTH_NAMES[month]+'-'+year+'.csv';
    a.click();
    URL.revokeObjectURL(url);
  }

  async function init(){
    const now = new Date();
    year = now.getFullYear();
    month = now.getMonth();
    monthSel.value = month;
    yearSel.value = year;

    if(storageMissing()){
      storageUnavailable = true;
      showStorageBanner();
    }

    await loadConfig();
    await loadSheet();
    populateDayFilters();
    render();
    populateGroupDatalist();

    monthSel.addEventListener('change', async ()=>{
      if(dirty) await saveAllNow();
      month = Number(monthSel.value);
      await loadSheet();
      populateDayFilters();
      render();
    });
    yearSel.addEventListener('change', async ()=>{
      if(dirty) await saveAllNow();
      year = Number(yearSel.value);
      await loadSheet();
      populateDayFilters();
      render();
    });

    document.getElementById('applyFilterBtn').addEventListener('click', ()=>{
      let f = Number(fromDaySel.value);
      let t = Number(toDaySel.value);
      if(f > t){ const tmp=f; f=t; t=tmp; }
      filterFrom = f;
      filterTo = t;
      render();
    });
    document.getElementById('resetFilterBtn').addEventListener('click', ()=>{
      populateDayFilters();
      render();
    });

    document.getElementById('saveNowBtn').addEventListener('click', saveAllNow);

    document.getElementById('exportBtn').addEventListener('click', exportCSV);

    document.getElementById('backupBtn').addEventListener('click', backupAll);

    const restoreFileInput = document.getElementById('restoreFile');
    document.getElementById('restoreBtn').addEventListener('click', ()=>{
      restoreFileInput.value = '';
      restoreFileInput.click();
    });
    restoreFileInput.addEventListener('change', async ()=>{
      const file = restoreFileInput.files && restoreFileInput.files[0];
      if(!file) return;
      try{
        const text = await file.text();
        const data = JSON.parse(text);
        if(data.sheet) grid = data.sheet;
        if(data.config){
          const cfg = data.config;
          if(cfg.customRows){
            cfg.customRows.forEach(r=>{
              if(!ROWS.find(x=>x.id===r.id)) ROWS.push(r);
            });
          }
          if(cfg.rowOverrides){
            Object.keys(cfg.rowOverrides).forEach(id=>{
              const row = ROWS.find(r=>r.id===id);
              if(row){
                row.name = cfg.rowOverrides[id].name;
                row.group = cfg.rowOverrides[id].group;
                row.edited = true;
              }
            });
          }
          if(cfg.pageModeOverrides){
            Object.keys(cfg.pageModeOverrides).forEach(id=>{
              const row = ROWS.find(r=>r.id===id);
              if(row) row.pmode = cfg.pageModeOverrides[id];
            });
          }
          if(cfg.collapsed) collapsed = cfg.collapsed;
        }
        render();
        populateGroupDatalist();
        if(!storageUnavailable) queueSave();
        statusEl.classList.remove('err');
        statusEl.textContent = 'Backup restored ✓';
        setTimeout(()=>{ if(statusEl.textContent==='Backup restored ✓') statusEl.textContent=''; }, 2000);
      }catch(e){
        console.error('restore failed', e);
        alert('Could not read that backup file. Make sure it\'s a JSON file downloaded from this tracker\'s "Download backup" button.');
      }
    });

    document.getElementById('clearMonthBtn').addEventListener('click', async ()=>{
      if(confirm('Clear all entries for '+MONTH_NAMES[month]+' '+year+'? This cannot be undone.')){
        grid = {};
        await saveSheetNow();
        render();
      }
    });

    document.getElementById('addRowBtn').addEventListener('click', ()=>{
      const nameInput = document.getElementById('newRowName');
      const groupInput = document.getElementById('newRowGroup');
      const name = nameInput.value.trim();
      if(!name) return;
      const groupVal = groupInput.value.trim() || 'Ungrouped';
      let id = slug(name);
      if(ROWS.find(r=>r.id===id)) id = id + '-' + Date.now();
      ROWS.push({id, name, group: groupVal, pmode:false, custom:true, edited:false});
      nameInput.value='';
      groupInput.value='';
      saveConfig();
      render();
      populateGroupDatalist();
    });

    // Best-effort flush before the tab closes or is navigated away from
    window.addEventListener('beforeunload', ()=>{
      if(dirty){
        saveSheetNow();
        saveConfig();
      }
    });
  }

  function populateGroupDatalist(){
    const dl = document.getElementById('existingGroups');
    if(!dl) return;
    dl.innerHTML = '';
    orderedGroups().forEach(gid=>{
      const opt = document.createElement('option');
      opt.value = gid;
      dl.appendChild(opt);
    });
  }

  init();
})();
</script>
</body>
</html>
