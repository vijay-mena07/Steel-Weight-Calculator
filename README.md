<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Steel Weight Calculator Pro</title>
<meta name="description" content="Free online steel weight calculator for MS sheets, angles, pipes, channels, beams and more.">
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
:root{
  --primary:#667eea;--primary2:#764ba2;--dark:#2c3e50;--dark2:#34495e;
  --green:#4caf50;--green-bg:#e8f5e9;--red:#e74c3c;
}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Segoe UI',Tahoma,sans-serif;background:linear-gradient(135deg,#667eea,#764ba2);min-height:100vh;padding:20px}
.container{max-width:900px;margin:0 auto;background:#fff;border-radius:20px;box-shadow:0 20px 60px rgba(0,0,0,.3);overflow:hidden}
header{background:linear-gradient(135deg,#2c3e50,#34495e);color:#fff;padding:24px;text-align:center}
header h1{font-size:1.9em;margin-bottom:6px}
header p{opacity:.85;margin-bottom:16px;font-size:.95em}
.header-controls{display:flex;gap:16px;justify-content:center;flex-wrap:wrap}
.control-group{display:flex;flex-direction:column;text-align:left;min-width:240px}
.control-group label{font-size:.85em;margin-bottom:4px;opacity:.9}
.control-group select{padding:8px;border-radius:6px;border:none;font-size:.95em}
.tabs{display:flex;background:#ecf0f1;overflow-x:auto}
.tab-btn{flex:1;padding:13px 16px;border:none;background:transparent;cursor:pointer;font-weight:600;color:#555;white-space:nowrap;min-width:105px;font-size:.92em;transition:.2s}
.tab-btn.active{background:#fff;color:var(--primary);border-bottom:3px solid var(--primary)}
.tab-btn:hover{background:#d5dbdb}
.section{display:none;padding:28px}
.section.active{display:block}
.section h2{color:var(--dark);margin-bottom:20px;font-size:1.4em}
.field{margin-bottom:15px}
.field label{display:block;margin-bottom:6px;color:#555;font-weight:600;font-size:.92em}
.field input,.field select{width:100%;padding:11px 14px;border:2px solid #ddd;border-radius:8px;font-size:1em;transition:border .2s}
.field input:focus,.field select:focus{outline:none;border-color:var(--primary)}
.calc-btn{width:100%;padding:14px;background:linear-gradient(135deg,#667eea,#764ba2);color:#fff;border:none;border-radius:8px;font-size:1.05em;font-weight:600;cursor:pointer;margin-top:8px;transition:.2s}
.calc-btn:hover{transform:translateY(-2px);box-shadow:0 5px 20px rgba(102,126,234,.4)}
.result{margin-top:18px;padding:16px;background:var(--green-bg);border-left:4px solid var(--green);border-radius:8px;display:none}
.result.show{display:block;animation:slide .3s}
@keyframes slide{from{opacity:0;transform:translateY(-8px)}to{opacity:1;transform:translateY(0)}}
.result p{margin:6px 0;line-height:1.5;font-size:.98em}
.result strong{color:#2e7d32}
.action-buttons{display:flex;gap:10px;margin-top:12px}
.action-btn{flex:1;padding:11px;background:var(--dark2);color:#fff;border:none;border-radius:6px;cursor:pointer;font-weight:600;transition:.2s}
.action-btn:hover{background:var(--dark);transform:translateY(-1px)}
.saved{padding:24px 28px;background:#f8f9fa;border-top:2px solid #ecf0f1}
.saved h3{color:var(--dark);margin-bottom:14px}
.clear-btn{padding:7px 14px;background:var(--red);color:#fff;border:none;border-radius:5px;cursor:pointer;margin-bottom:14px;font-weight:600}
.clear-btn:hover{background:#c0392b}
.saved-item{background:#fff;padding:13px;border-radius:8px;border-left:4px solid var(--primary);display:flex;justify-content:space-between;align-items:center;box-shadow:0 2px 5px rgba(0,0,0,.08);margin-bottom:10px;gap:10px}
.saved-item h4{color:var(--dark);font-size:.95em;margin-bottom:4px}
.saved-item p{color:#666;font-size:.85em;margin:2px 0}
.delete-btn{padding:7px 11px;background:var(--red);color:#fff;border:none;border-radius:5px;cursor:pointer;font-size:.85em}
footer{background:#f8f9fa;padding:16px;text-align:center;color:#666;border-top:2px solid #ecf0f1;font-size:.88em}
footer p{margin:3px 0}
@media(max-width:600px){
  .section{padding:18px}
  header h1{font-size:1.4em}
  .header-controls{flex-direction:column}
  .control-group{min-width:100%;width:100%}
  .saved{padding:18px}
}
</style>
</head>
<body>
<div class="container">
  <header>
    <h1>🏗️ Steel Weight Calculator Pro</h1>
    <p>Calculate weight for MS Sheets, Angles, Bars, Pipes, Channels & Beams</p>
    <div class="header-controls">
      <div class="control-group">
        <label>Material</label>
        <select id="material">
          <option value="7850">Mild Steel (MS) — 7850 kg/m³</option>
          <option value="7850">Carbon Steel — 7850 kg/m³</option>
          <option value="8000">Stainless Steel (SS 304) — 8000 kg/m³</option>
          <option value="2700">Aluminum — 2700 kg/m³</option>
          <option value="8900">Copper — 8900 kg/m³</option>
          <option value="8500">Brass — 8500 kg/m³</option>
          <option value="7200">Cast Iron — 7200 kg/m³</option>
        </select>
      </div>
      <div class="control-group">
        <label>Units</label>
        <select id="unit" onchange="onUnitChange()">
          <option value="metric">Metric (mm, kg)</option>
          <option value="imperial">Imperial (inch, lbs)</option>
        </select>
      </div>
    </div>
  </header>

  <div class="tabs">
    <button class="tab-btn active" onclick="showTab('sheet',this)">Sheet/Plate</button>
    <button class="tab-btn" onclick="showTab('angle',this)">Angle</button>
    <button class="tab-btn" onclick="showTab('round',this)">Round Bar</button>
    <button class="tab-btn" onclick="showTab('square',this)">Square Bar</button>
    <button class="tab-btn" onclick="showTab('pipe',this)">Pipe</button>
    <button class="tab-btn" onclick="showTab('tube',this)">Square Tube</button>
    <button class="tab-btn" onclick="showTab('channel',this)">Channel</button>
    <button class="tab-btn" onclick="showTab('beam',this)">I-Beam</button>
  </div>

  <!-- SHEET -->
  <div id="sheet" class="section active">
    <h2>Sheet / Plate Weight Calculator</h2>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="sheet-l" placeholder="e.g. 2400"></div>
    <div class="field"><label>Width (<span class="u">mm</span>)</label><input type="number" id="sheet-w" placeholder="e.g. 1200"></div>
    <div class="field"><label>Thickness (<span class="u">mm</span>)</label><input type="number" id="sheet-t" step="0.1" placeholder="e.g. 6"></div>
    <div class="field"><label>Quantity</label><input type="number" id="sheet-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcSheet()">Calculate Weight</button>
    <div class="result" id="sheet-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('sheet')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('sheet')">📄 Export PDF</button>
    </div>
  </div>

  <!-- ANGLE -->
  <div id="angle" class="section">
    <h2>Angle Weight Calculator</h2>
    <div class="field"><label>Standard Size</label>
      <select id="angle-std" onchange="fillAngle()">
        <option value="">-- Select standard size --</option>
        <option value="25,25,3">25 × 25 × 3 mm</option>
        <option value="25,25,5">25 × 25 × 5 mm</option>
        <option value="40,40,6">40 × 40 × 6 mm</option>
        <option value="50,50,6">50 × 50 × 6 mm</option>
        <option value="65,65,6">65 × 65 × 6 mm</option>
        <option value="75,75,6">75 × 75 × 6 mm</option>
        <option value="100,100,10">100 × 100 × 10 mm</option>
        <option value="150,150,12">150 × 150 × 12 mm</option>
        <option value="custom">Custom size...</option>
      </select>
    </div>
    <div class="field"><label>Side A (<span class="u">mm</span>)</label><input type="number" id="angle-a" placeholder="e.g. 50"></div>
    <div class="field"><label>Side B (<span class="u">mm</span>)</label><input type="number" id="angle-b" placeholder="e.g. 50"></div>
    <div class="field"><label>Thickness (<span class="u">mm</span>)</label><input type="number" id="angle-t" step="0.1" placeholder="e.g. 6"></div>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="angle-l" placeholder="e.g. 6000"></div>
    <div class="field"><label>Quantity</label><input type="number" id="angle-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcAngle()">Calculate Weight</button>
    <div class="result" id="angle-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('angle')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('angle')">📄 Export PDF</button>
    </div>
  </div>

  <!-- ROUND BAR -->
  <div id="round" class="section">
    <h2>Round Bar Weight Calculator</h2>
    <div class="field"><label>Diameter (<span class="u">mm</span>)</label><input type="number" id="round-d" placeholder="e.g. 12"></div>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="round-l" placeholder="e.g. 6000"></div>
    <div class="field"><label>Quantity</label><input type="number" id="round-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcRound()">Calculate Weight</button>
    <div class="result" id="round-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('round')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('round')">📄 Export PDF</button>
    </div>
  </div>

  <!-- SQUARE BAR -->
  <div id="square" class="section">
    <h2>Square Bar Weight Calculator</h2>
    <div class="field"><label>Side (<span class="u">mm</span>)</label><input type="number" id="sq-s" placeholder="e.g. 20"></div>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="sq-l" placeholder="e.g. 6000"></div>
    <div class="field"><label>Quantity</label><input type="number" id="sq-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcSquare()">Calculate Weight</button>
    <div class="result" id="square-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('square')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('square')">📄 Export PDF</button>
    </div>
  </div>

  <!-- PIPE -->
  <div id="pipe" class="section">
    <h2>Round Pipe Weight Calculator</h2>
    <div class="field"><label>Outer Diameter (<span class="u">mm</span>)</label><input type="number" id="pipe-od" placeholder="e.g. 60"></div>
    <div class="field"><label>Wall Thickness (<span class="u">mm</span>)</label><input type="number" id="pipe-t" step="0.1" placeholder="e.g. 3"></div>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="pipe-l" placeholder="e.g. 6000"></div>
    <div class="field"><label>Quantity</label><input type="number" id="pipe-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcPipe()">Calculate Weight</button>
    <div class="result" id="pipe-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('pipe')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('pipe')">📄 Export PDF</button>
    </div>
  </div>

  <!-- SQUARE TUBE -->
  <div id="tube" class="section">
    <h2>Square / Rectangular Tube Weight Calculator</h2>
    <div class="field"><label>Width (<span class="u">mm</span>)</label><input type="number" id="tube-w" placeholder="e.g. 50"></div>
    <div class="field"><label>Height (<span class="u">mm</span>)</label><input type="number" id="tube-h" placeholder="e.g. 50"></div>
    <div class="field"><label>Wall Thickness (<span class="u">mm</span>)</label><input type="number" id="tube-t" step="0.1" placeholder="e.g. 3"></div>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="tube-l" placeholder="e.g. 6000"></div>
    <div class="field"><label>Quantity</label><input type="number" id="tube-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcTube()">Calculate Weight</button>
    <div class="result" id="tube-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('tube')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('tube')">📄 Export PDF</button>
    </div>
  </div>

  <!-- CHANNEL -->
  <div id="channel" class="section">
    <h2>Channel (ISMC) Weight Calculator</h2>
    <div class="field"><label>Standard Size</label>
      <select id="ch-std" onchange="fillChannel()">
        <option value="">-- Select standard size --</option>
        <option value="75,40,4.8,7.5">ISMC 75 (75×40×4.8)</option>
        <option value="100,50,5,7.5">ISMC 100 (100×50×5)</option>
        <option value="125,65,5.3,8.1">ISMC 125 (125×65×5.3)</option>
        <option value="150,75,5.7,8.5">ISMC 150 (150×75×5.7)</option>
        <option value="200,75,6.2,9.2">ISMC 200 (200×75×6.2)</option>
        <option value="custom">Custom size...</option>
      </select>
    </div>
    <div class="field"><label>Height (<span class="u">mm</span>)</label><input type="number" id="ch-h" placeholder="e.g. 100"></div>
    <div class="field"><label>Flange Width (<span class="u">mm</span>)</label><input type="number" id="ch-w" placeholder="e.g. 50"></div>
    <div class="field"><label>Web Thickness (<span class="u">mm</span>)</label><input type="number" id="ch-tw" step="0.1" placeholder="e.g. 5"></div>
    <div class="field"><label>Flange Thickness (<span class="u">mm</span>)</label><input type="number" id="ch-tf" step="0.1" placeholder="e.g. 7.5"></div>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="ch-l" placeholder="e.g. 6000"></div>
    <div class="field"><label>Quantity</label><input type="number" id="ch-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcChannel()">Calculate Weight</button>
    <div class="result" id="channel-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('channel')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('channel')">📄 Export PDF</button>
    </div>
  </div>

  <!-- BEAM -->
  <div id="beam" class="section">
    <h2>I-Beam (ISMB) Weight Calculator</h2>
    <div class="field"><label>Standard Size</label>
      <select id="bm-std" onchange="fillBeam()">
        <option value="">-- Select standard size --</option>
        <option value="100,75,4,7.2">ISMB 100</option>
        <option value="150,80,4.8,9">ISMB 150</option>
        <option value="200,100,5.7,10.8">ISMB 200</option>
        <option value="250,125,6.5,12.5">ISMB 250</option>
        <option value="300,140,7.5,13.1">ISMB 300</option>
        <option value="custom">Custom size...</option>
      </select>
    </div>
    <div class="field"><label>Height (<span class="u">mm</span>)</label><input type="number" id="bm-h" placeholder="e.g. 200"></div>
    <div class="field"><label>Flange Width (<span class="u">mm</span>)</label><input type="number" id="bm-w" placeholder="e.g. 100"></div>
    <div class="field"><label>Web Thickness (<span class="u">mm</span>)</label><input type="number" id="bm-tw" step="0.1" placeholder="e.g. 5.7"></div>
    <div class="field"><label>Flange Thickness (<span class="u">mm</span>)</label><input type="number" id="bm-tf" step="0.1" placeholder="e.g. 10.8"></div>
    <div class="field"><label>Length (<span class="u">mm</span>)</label><input type="number" id="bm-l" placeholder="e.g. 6000"></div>
    <div class="field"><label>Quantity</label><input type="number" id="bm-q" value="1" min="1"></div>
    <button class="calc-btn" onclick="calcBeam()">Calculate Weight</button>
    <div class="result" id="beam-r"></div>
    <div class="action-buttons">
      <button class="action-btn" onclick="saveCalc('beam')">💾 Save</button>
      <button class="action-btn" onclick="exportPDF('beam')">📄 Export PDF</button>
    </div>
  </div>

  <!-- SAVED -->
  <div class="saved">
    <h3>📋 Saved Calculations</h3>
    <button class="clear-btn" onclick="clearSaved()">Clear All</button>
    <div id="saved-list"></div>
  </div>

  <footer>
    <p>📊 Theoretical density-based results</p>
    <p>⚠️ Actual weight may vary ±2–3%</p>
  </footer>
</div>

<script>
/* ============ STATE ============ */
const lastResults = {};

/* ============ HELPERS ============ */
function density(){ return parseFloat(document.getElementById('material').value); }
function isImperial(){ return document.getElementById('unit').value === 'imperial'; }
function dim(id){
  const v = parseFloat(document.getElementById(id).value);
  if (isNaN(v)) return 0;
  return isImperial() ? v * 25.4 : v;   // convert inches -> mm
}
function qty(id){ return Math.max(1, parseInt(document.getElementById(id).value) || 1); }
function fmt(kg){ return isImperial() ? [kg * 2.20462, 'lbs'] : [kg, 'kg']; }

/* ============ TABS ============ */
function showTab(id, btn){
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  btn.classList.add('active');
}

/* ============ UNIT SWITCH ============ */
function onUnitChange(){
  const imp = isImperial();
  document.querySelectorAll('.u').forEach(el => el.textContent = imp ? 'in' : 'mm');
}

/* ============ STANDARD SIZE FILLERS ============ */
function fillAngle(){
  const v = document.getElementById('angle-std').value;
  if (!v || v === 'custom') return;
  const [a,b,t] = v.split(',');
  document.getElementById('angle-a').value = a;
  document.getElementById('angle-b').value = b;
  document.getElementById('angle-t').value = t;
}
function fillChannel(){
  const v = document.getElementById('ch-std').value;
  if (!v || v === 'custom') return;
  const [h,w,tw,tf] = v.split(',');
  document.getElementById('ch-h').value = h;
  document.getElementById('ch-w').value = w;
  document.getElementById('ch-tw').value = tw;
  document.getElementById('ch-tf').value = tf;
}
function fillBeam(){
  const v = document.getElementById('bm-std').value;
  if (!v || v === 'custom') return;
  const [h,w,tw,tf] = v.split(',');
  document.getElementById('bm-h').value = h;
  document.getElementById('bm-w').value = w;
  document.getElementById('bm-tw').value = tw;
  document.getElementById('bm-tf').value = tf;
}

/* ============ CALCULATORS ============ */
function calcSheet(){
  const L=dim('sheet-l'), W=dim('sheet-w'), T=dim('sheet-t');
  if(!L||!W||!T) return alert('Please fill all fields');
  render('sheet','Sheet/Plate', L*W*T, qty('sheet-q'));
}
function calcAngle(){
  const a=dim('angle-a'), b=dim('angle-b'), t=dim('angle-t'), L=dim('angle-l');
  if(!a||!b||!t||!L) return alert('Please fill all fields');
  render('angle','Angle', (a+b-t)*t*L, qty('angle-q'));
}
function calcRound(){
  const d=dim('round-d'), L=dim('round-l');
  if(!d||!L) return alert('Please fill all fields');
  render('round','Round Bar', Math.PI*Math.pow(d/2,2)*L, qty('round-q'));
}
function calcSquare(){
  const s=dim('sq-s'), L=dim('sq-l');
  if(!s||!L) return alert('Please fill all fields');
  render('square','Square Bar', s*s*L, qty('sq-q'));
}
function calcPipe(){
  const od=dim('pipe-od'), t=dim('pipe-t'), L=dim('pipe-l');
  if(!od||!t||!L) return alert('Please fill all fields');
  const idia = od - 2*t;
  if(idia <= 0) return alert('Wall thickness is too large');
  const area = Math.PI*(Math.pow(od/2,2) - Math.pow(idia/2,2));
  render('pipe','Round Pipe', area*L, qty('pipe-q'));
}
function calcTube(){
  const w=dim('tube-w'), h=dim('tube-h'), t=dim('tube-t'), L=dim('tube-l');
  if(!w||!h||!t||!L) return alert('Please fill all fields');
  if(w<=2*t || h<=2*t) return alert('Wall thickness is too large');
  const area = w*h - (w-2*t)*(h-2*t);
  render('tube','Square Tube', area*L, qty('tube-q'));
}
function calcChannel(){
  const h=dim('ch-h'), w=dim('ch-w'), tw=dim('ch-tw'), tf=dim('ch-tf'), L=dim('ch-l');
  if(!h||!w||!tw||!tf||!L) return alert('Please fill all fields');
  const area = (h-2*tf)*tw + 2*w*tf;
  render('channel','Channel', area*L, qty('ch-q'));
}
function calcBeam(){
  const h=dim('bm-h'), w=dim('bm-w'), tw=dim('bm-tw'), tf=dim('bm-tf'), L=dim('bm-l');
  if(!h||!w||!tw||!tf||!L) return alert('Please fill all fields');
  const area = (h-2*tf)*tw + 2*w*tf;
  render('beam','I-Beam', area*L, qty('bm-q'));
}

/* ============ RENDER RESULT ============ */
function render(key, shape, volumeMM3, quantity){
  // Weight in kg = volume(mm³) × density(kg/m³) ÷ 1e9
  const kgPerPiece = volumeMM3 * density() / 1e9;
  const [wpp, unit] = fmt(kgPerPiece);
  const [total]   = fmt(kgPerPiece * quantity);
  const matName = document.getElementById('material').selectedOptions[0].text;

  let html = `<p><strong>Material:</strong> ${matName}</p>
              <p><strong>Weight per piece:</strong> ${wpp.toFixed(3)} ${unit}</p>`;
  if (quantity > 1){
    html += `<p><strong>Quantity:</strong> ${quantity} pcs</p>
             <p><strong>Total weight:</strong> ${total.toFixed(3)} ${unit}</p>`;
  }
  if (!isImperial() && kgPerPiece * quantity >= 1000){
    html += `<p><strong>Total:</strong> ${(kgPerPiece*quantity/1000).toFixed(3)} Metric Tons</p>`;
  }

  const el = document.getElementById(key + '-r');
  el.innerHTML = html;
  el.classList.add('show');

  lastResults[key] = {
    shape, material: matName,
    wpp: wpp.toFixed(3), total: total.toFixed(3),
    qty: quantity, unit,
    date: new Date().toLocaleString()
  };
}

/* ============ SAVE / LOAD ============ */
function saveCalc(key){
  if(!lastResults[key]) return alert('Please calculate first!');
  let saved = JSON.parse(localStorage.getItem('steelSaved') || '[]');
  saved.unshift({ id: Date.now(), ...lastResults[key] });
  saved = saved.slice(0, 20);
  localStorage.setItem('steelSaved', JSON.stringify(saved));
  loadSaved();
  alert('✅ Calculation saved!');
}
function loadSaved(){
  const saved = JSON.parse(localStorage.getItem('steelSaved') || '[]');
  const el = document.getElementById('saved-list');
  if(!saved.length){
    el.innerHTML = '<p style="color:#999;text-align:center;padding:8px">No saved calculations yet</p>';
    return;
  }
  el.innerHTML = saved.map(s => `
    <div class="saved-item">
      <div>
        <h4>${s.shape} — ${s.material}</h4>
        <p>Per piece: ${s.wpp} ${s.unit} • Total: ${s.total} ${s.unit} (×${s.qty})</p>
        <p style="color:#aaa;font-size:.78em">${s.date}</p>
      </div>
      <button class="delete-btn" onclick="delSaved(${s.id})">✕</button>
    </div>`).join('');
}
function delSaved(id){
  const saved = JSON.parse(localStorage.getItem('steelSaved') || '[]').filter(s => s.id !== id);
  localStorage.setItem('steelSaved', JSON.stringify(saved));
  loadSaved();
}
function clearSaved(){
  if (confirm('Clear all saved calculations?')){
    localStorage.removeItem('steelSaved');
    loadSaved();
  }
}

/* ============ PDF EXPORT ============ */
function exportPDF(key){
  const r = lastResults[key];
  if(!r) return alert('Please calculate first!');
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF();

  doc.setFontSize(20);
  doc.text('Steel Weight Calculator', 20, 22);
  doc.setFontSize(12);
  doc.text(`Section: ${r.shape}`, 20, 42);
  doc.text(`Material: ${r.material}`, 20, 52);
  doc.text(`Date: ${r.date}`, 20, 62);

  doc.setFontSize(14);
  doc.text('Results', 20, 82);
  doc.setFontSize(12);
  doc.text(`Weight per piece: ${r.wpp} ${r.unit}`, 20, 94);
  doc.text(`Quantity: ${r.qty} pcs`, 20, 104);
  doc.text(`Total weight: ${r.total} ${r.unit}`, 20, 114);

  doc.setFontSize(10);
  doc.setTextColor(120);
  doc.text('Note: Approximate values only. Actual weight may vary ±2-3%.', 20, 140);
  doc.text('Generated by Steel Weight Calculator Pro', 20, 150);

  doc.save(`${r.shape.replace(/[^a-z0-9]/gi,'_')}_${Date.now()}.pdf`);
}

/* ============ INIT ============ */
document.addEventListener('DOMContentLoaded', loadSaved);
</script>
</body>
</html>
