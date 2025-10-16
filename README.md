<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Logística & Inventario</title>
<style>
  :root{
    --bg: #f5f7fb;
    --card: #ffffff;
    --text: #0f1724;
    --accent: #0ea5a4;
    --muted: #6b7280;
    --glass: rgba(255,255,255,0.6);
  }
  [data-theme="dark"]{
    --bg:#0b1220;
    --card:#071226;
    --text:#e6eef6;
    --accent:#22d3ee;
    --muted:#9aa6b2;
    --glass: rgba(255,255,255,0.03);
  }
  *{box-sizing:border-box;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;}
  body{margin:0;background:linear-gradient(180deg,var(--bg),#e6eef6);color:var(--text);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px;transition:background .4s ease}
  .app {
    width:100%;
    max-width:980px;
    margin:24px;
    background:linear-gradient(180deg, rgba(255,255,255,0.6), var(--card));
    border-radius:18px;
    box-shadow: 0 10px 30px rgba(12,22,38,0.12);
    overflow:hidden;
    transform:translateY(0);
    animation: entrance .6s ease both;
  }
  @keyframes entrance{from{opacity:0;transform:translateY(8px) scale(.995)} to{opacity:1;transform:none}}
  header{
    display:flex;align-items:center;justify-content:space-between;padding:22px 28px;border-bottom:1px solid rgba(0,0,0,0.04);
    gap:12px;background:linear-gradient(90deg, rgba(255,255,255,0.2), transparent);
  }
  .title{display:flex;gap:14px;align-items:center}
  .logo{
    width:54px;height:54px;border-radius:12px;background:linear-gradient(135deg,var(--accent), #7c3aed);display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:20px;box-shadow: 0 6px 18px rgba(14,165,164,0.18);
  }
  h1{font-size:20px;margin:0}
  p.subtitle{margin:0;color:var(--muted);font-size:13px}
  .controls{display:flex;gap:12px;align-items:center}
  .toggle{
    display:inline-flex;align-items:center;gap:8px;padding:8px 12px;border-radius:12px;background:var(--glass);cursor:pointer;border:1px solid rgba(0,0,0,0.04)
  }

  main{padding:28px;display:grid;grid-template-columns:1fr 420px;gap:20px}
  @media (max-width:900px){ main{grid-template-columns:1fr;}}
  .panel{
    background:linear-gradient(180deg, rgba(255,255,255,0.5), transparent);
    border-radius:14px;padding:18px;box-shadow: 0 6px 20px rgba(8,12,20,0.04);
  }

  form.row{display:flex;flex-direction:column;gap:12px}
  label{font-size:13px;color:var(--muted)}
  select,input,textarea{
    padding:12px;border-radius:10px;border:1px solid rgba(11,22,38,0.06);font-size:15px;background:transparent;outline:none;
    appearance: none; 
    -webkit-appearance: none;
  }
  .small {font-size:13px;color:var(--muted);}

  .actions{display:flex;gap:12px;align-items:center;margin-top:8px}
  .btn{
    padding:12px 16px;border-radius:12px;border:none;cursor:pointer;font-weight:600;background:linear-gradient(90deg,var(--accent), #7c3aed);color:white;box-shadow: 0 8px 20px rgba(14,165,164,0.12);
  }
  .btn.ghost{background:transparent;color:var(--text);border:1px solid rgba(0,0,0,0.06);}
  .result-list{display:flex;flex-direction:column;gap:12px}
  .hint{font-size:13px;color:var(--muted);padding:8px;border-radius:10px;background:rgba(0,0,0,0.02);}
  .footer{padding:12px 18px;text-align:center;color:var(--muted);font-size:13px;border-top:1px solid rgba(0,0,0,0.04)}

  /* modal */
  .modal-backdrop{position:fixed;inset:0;background:rgba(2,6,23,0.45);display:flex;align-items:center;justify-content:center;z-index:60;opacity:0;pointer-events:none;transition:all .22s ease}
  .modal-backdrop.show{opacity:1;pointer-events:auto}
  .modal-card{
    width: min(780px, 94%);
    border-radius:14px;padding:18px;background:var(--card);box-shadow:0 30px 80px rgba(3,6,23,0.45);transform:translateY(10px);transition:transform .22s ease,opacity .22s ease;opacity:0;
  }
  .modal-backdrop.show .modal-card{transform:none;opacity:1}
  .card-row{display:flex;gap:12px;align-items:center;flex-wrap:wrap}
  .field{flex:1;min-width:160px;background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);padding:12px;border-radius:10px;border:1px solid rgba(0,0,0,0.04)}
  .field strong{display:block;font-size:13px;color:var(--muted)}
  .value{font-weight:700;font-size:15px;margin-top:6px}
  .meta{font-size:12px;color:var(--muted);margin-top:8px}

  .copy-btn{padding:8px 12px;border-radius:10px;border:none;background:var(--glass);cursor:pointer}

  .loader{width:36px;height:36px;border-radius:50%;border:4px solid rgba(0,0,0,0.08);border-top-color:var(--accent);animation:spin .9s linear infinite}
  @keyframes spin{to{transform:rotate(360deg)}}

  /* small form for adding documents */
  .add-doc {
    margin-top: 16px;
    display:flex;
    flex-direction:column;
    gap:10px; 
  }
  .mini {
    font-size:13px;
    color:var(--muted);
    /* Corregido: Separación vertical */
    margin-bottom: 4px; 
    margin-top: 10px;
    display: block;
  }
</style>
</head>
<body data-theme="light">
<div class="app" role="application" aria-label="Logística e Inventario">
  <header>
    <div class="title">
      <div class="logo">LI</div>
      <div>
        <h1>Logística & Inventario</h1>
        <p class="subtitle">Consulta rápida por código — busca en hojas y trae el registro más reciente</p>
      </div>
    </div>

    <div class="controls">
      <div class="toggle" id="themeToggle" title="Alternar modo oscuro">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" style="opacity:.9"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z" stroke="currentColor" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/></svg>
        <span id="themeLabel">Modo claro</span>
      </div>
    </div>
  </header>

  <main>
    <section class="panel">
      <form id="searchForm" class="row" autocomplete="off">
        <div>
          <label for="docSelect">Documento (elige uno)</label>
          <select id="docSelect" required>
            <option value="" disabled selected>-- Selecciona documento --</option> 
          </select>
        </div>

        <div>
          <label for="codeInput">Código (numérico)</label>
          <input id="codeInput" type="text" inputmode="numeric" pattern="[0-9]*" placeholder="Ej: 12345" required />
          <div class="small">Introduce sólo números. El sistema los tratará como texto internamente.</div>
        </div>

        <div class="actions">
          <button class="btn" type="submit" id="searchBtn">Buscar</button>
          <button type="button" class="btn ghost" id="clearBtn">Limpiar</button>
          <div id="spinner" style="display:none;margin-left:8px"><div class="loader" aria-hidden="true"></div></div>
        </div>

        <div id="message" class="hint" style="display:none;margin-top:12px"></div>
      </form>

      <div style="margin-top:16px">
        <div class="small">Puedes agregar más documentos abajo sin editar el HTML.</div>

        <form class="add-doc" id="addDocForm">
          <label class="mini" for="newName">Añadir / editar documento (nombre, id, hojas, columnas)</label>
          <input id="newName" placeholder="Nombre del documento (ej: UPI CORO)" />
          <input id="newId" placeholder="ID del documento (ej: 1n6HJ2...)" />
          <textarea id="newSheets" rows="2" placeholder="Hojas (separadas por coma) — orden de búsqueda: CONTEO GENERAL,CONTABILIZADO ..."></textarea>
          <textarea id="newColumns" rows="2" placeholder="Columnas a mostrar (separadas por coma) — ej: CODIGO,DESCRIPCION,UM,..."></textarea>
          <div style="display:flex;gap:8px">
            <input id="newCodeCol" placeholder="Nombre columna código (ej: CODIGO)" />
            <input id="newDateCol" placeholder="Nombre columna fecha (ej: FECHA ULTIMO CONTEO)" />
          </div>
          <div style="display:flex;gap:8px">
            <button class="btn ghost" type="button" id="addDocBtn">Agregar documento</button>
            <button class="btn ghost" type="button" id="resetDocsBtn">Reset a lista original</button>
          </div>
          <div class="mini">Añadirá el documento al selector. Si quieres editar remueve y vuelve a añadir (sencillo).</div>
        </form>
      </div>
    </section>

    <aside class="panel">
      <h3 style="margin:0 0 8px 0">Últimos resultados</h3>
      <div id="results" class="result-list">
        <div class="hint">Aquí aparecerá la tarjeta del resultado (si se encuentra).</div>
      </div>
    </aside>
  </main>

  <div class="footer">Hecho con ❤️ — Interfaz responsive y preparada para múltiples usuarios</div>
</div>

<div id="modal" class="modal-backdrop" aria-hidden="true" role="dialog" aria-modal="true">
  <div class="modal-card">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
      <div>
        <strong id="modalTitle">Resultado</strong>
        <div class="meta" id="modalMeta"></div>
      </div>
      <div style="display:flex;gap:8px;align-items:center">
        <button class="copy-btn" id="copyAllBtn">Copiar todo</button>
        <button class="copy-btn" id="closeModalBtn" aria-label="Cerrar">Cerrar</button>
      </div>
    </div>
    <div id="cardContent" style="display:grid;grid-template-columns:repeat(2,1fr);gap:12px"></div>
  </div>
</div>

<script>
/* ===========================
   CONFIG: reemplaza webAppUrl por la URL de tu Web App (Apps Script).
   =========================== */
const FRONTEND_CONFIG_DEFAULT = {
  webAppUrl: 'https://script.google.com/macros/s/AKfycbwaET0tqiF6ZiJhoujo2wUG0Gh9U6d40gc7tapW-8_-Fyk2dLfuHXYMkSeQID7XfyHz3Q/exec',
  documents: [
    { name: 'UPI CORO', id: '1n6HJ2sNwxPE68ldvtBaVVTcKYMRx_t8fKEeTyM6Ms7I',
      sheetNames: ['CONTEO GENERAL','CONTABILIZADO CICLICO AGOS 2025','CONTABILIZADO','DIFERENCIAS.'],
      columns: ['CODIGO','DESCRIPCION','UM','UBICACION FISICA GENERAL','CONTEO GENERAL','FECHA ULTIMO CONTEO','ULTIMO AUDITOR EN CONTAR','UBICACION EXHIB','CONTEO EXHIB','OBSERVACIONES','VER IMAGEN','DIFERENCIA GENERAL','DIFERENCIA EXHIB'],
      codeColumn: 'CODIGO',
      dateColumn: 'FECHA ULTIMO CONTEO'
    },
    { name: 'KACOSA', id: '1F_ajgkaY7hjdn081xt8ZQ6AdIjKB6NFka7CY24WlgVA',
      sheetNames: ['CONTEO GENERAL','CONTABILIZADO CICLICO AGOS 2025','CONTABILIZADO','DIFERENCIAS.'],
      columns: ['CODIGO','DESCRIPCION','UM','UBICACION FISICA GENERAL','CONTEO GENERAL','FECHA ULTIMO CONTEO','ULTIMO AUDITOR EN CONTAR','UBICACION EXHIB','CONTEO EXHIB','OBSERVACIONES','VER IMAGEN','DIFERENCIA GENERAL','DIFERENCIA EXHIB'],
      codeColumn: 'CODIGO',
      dateColumn: 'FECHA ULTIMO CONTEO'
    },
    { name: 'KACOSA', id: '1F_ajgkaY7hjdn081xt8ZQ6AdIjKB6NFka7CY24WlgVA',
      sheetNames: ['CONTEO GENERAL','CONTABILIZADO CICLICO AGOS 2025','CONTABILIZADO','DIFERENCIAS.'],
      columns: ['CODIGO','DESCRIPCION','UM','UBICACION FISICA GENERAL','CONTEO GENERAL','FECHA ULTIMO CONTEO','ULTIMO AUDITOR EN CONTAR','UBICACION EXHIB','CONTEO EXHIB','OBSERVACIONES','VER IMAGEN','DIFERENCIA GENERAL','DIFERENCIA EXHIB'],
      codeColumn: 'CODIGO',
      dateColumn: 'FECHA ULTIMO CONTEO'
    },
    { name: 'KACOSA 2', id: '15ip1JXvR0fPHCLrsXJFSi7gLKNT0rTwQt88B9IuSdRE',
      sheetNames: ['CONTEO GENERAL','CONTABILIZADO CICLICO AGOS 2025','CONTABILIZADO','DIFERENCIAS.'],
      columns: ['CODIGO','DESCRIPCION','UM','UBICACION FISICA GENERAL','CONTEO GENERAL','FECHA ULTIMO CONTEO','ULTIMO AUDITOR EN CONTAR','UBICACION EXHIB','CONTEO EXHIB','OBSERVACIONES','VER IMAGEN','DIFERENCIA GENERAL','DIFERENCIA EXHIB'],
      codeColumn: 'CODIGO',
      dateColumn: 'FECHA ULTIMO CONTEO'
    },
    { name: 'KACOSA 2', id: '15ip1JXvR0fPHCLrsXJFSi7gLKNT0rTwQt88B9IuSdRE',
      sheetNames: ['CONTEO GENERAL','CONTABILIZADO CICLICO AGOS 2025','CONTABILIZADO','DIFERENCIAS.'],
      columns: ['CODIGO','DESCRIPCION','UM','UBICACION FISICA GENERAL','CONTEO GENERAL','FECHA ULTIMO CONTEO','ULTIMO AUDITOR EN CONTAR','UBICACION EXHIB','CONTEO EXHIB','OBSERVACIONES','VER IMAGEN','DIFERENCIA GENERAL','DIFERENCIA EXHIB'],
      codeColumn: 'CODIGO',
      dateColumn: 'FECHA ULTIMO CONTEO'
    },
    { name: 'KACOSA 2', id: '15ip1JXvR0fPHCLrsXJFSi7gLKNT0rTwQt88B9IuSdRE.',
      sheetNames: ['CONTEO GENERAL','CONTABILIZADO CICLICO AGOS 2025','CONTABILIZADO','DIFERENCIAS.'],
      columns: ['CODIGO','DESCRIPCION','UM','UBICACION FISICA GENERAL','CONTEO GENERAL','FECHA ULTIMO CONTEO','ULTIMO AUDITOR EN CONTAR','UBICACION EXHIB','CONTEO EXHIB','OBSERVACIONES','VER IMAGEN','DIFERENCIA GENERAL','DIFERENCIA EXHIB'],
      codeColumn: 'CODIGO',
      dateColumn: 'FECHA ULTIMO CONTEO'
    }
  ]
};

// copia de configuración en ejecución (mutable)
let FRONTEND_CONFIG = JSON.parse(JSON.stringify(FRONTEND_CONFIG_DEFAULT));

/* ===========================
   END CONFIG
   =========================== */

const docSelect = document.getElementById('docSelect');
const codeInput = document.getElementById('codeInput');
const searchForm = document.getElementById('searchForm');
const searchBtn = document.getElementById('searchBtn');
const clearBtn = document.getElementById('clearBtn');
const message = document.getElementById('message');
const spinner = document.getElementById('spinner');
const resultsDiv = document.getElementById('results');

const modal = document.getElementById('modal');
const modalTitle = document.getElementById('modalTitle');
const modalMeta = document.getElementById('modalMeta');
const cardContent = document.getElementById('cardContent');
const copyAllBtn = document.getElementById('copyAllBtn');
const closeModalBtn = document.getElementById('closeModalBtn');

const themeToggle = document.getElementById('themeToggle');
const themeLabel = document.getElementById('themeLabel');

const addDocBtn = document.getElementById('addDocBtn');
const resetDocsBtn = document.getElementById('resetDocsBtn');
const newName = document.getElementById('newName');
const newId = document.getElementById('newId');
const newSheets = document.getElementById('newSheets');
const newColumns = document.getElementById('newColumns');
const newCodeCol = document.getElementById('newCodeCol');
const newDateCol = document.getElementById('newDateCol');

// init
function init() {
  populateDocSelect();
  searchForm.addEventListener('submit', onSubmit);
  clearBtn.addEventListener('click', onClear);
  closeModalBtn.addEventListener('click', closeModal);
  copyAllBtn.addEventListener('click', copyAll);
  themeToggle.addEventListener('click', toggleTheme);
  addDocBtn.addEventListener('click', onAddDoc);
  resetDocsBtn.addEventListener('click', onResetDocs);

  document.body.style.transition = 'background .6s ease';
  setTimeout(()=>document.body.style.background='linear-gradient(180deg,var(--bg), #dfeffe)', 120);

  const prefersDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
  setTheme(prefersDark ? 'dark' : 'light');
}

function populateDocSelect() {
  docSelect.innerHTML = '';
  const placeholder = document.createElement('option');
  placeholder.value = '';
  placeholder.textContent = '-- Selecciona documento --';
  placeholder.disabled = true;
  placeholder.selected = true; 
  docSelect.appendChild(placeholder);

  FRONTEND_CONFIG.documents.forEach((d, idx) => {
    const opt = document.createElement('option');
    opt.value = idx;
    opt.textContent = `${d.name} — ${d.id}`;
    docSelect.appendChild(opt);
  });
}

function setTheme(t) {
  document.body.setAttribute('data-theme', t);
  themeLabel.textContent = t === 'dark' ? 'Modo oscuro' : 'Modo claro';
}

function toggleTheme() {
  const cur = document.body.getAttribute('data-theme') || 'light';
  setTheme(cur === 'light' ? 'dark' : 'light');
}

function onClear() {
  codeInput.value = '';
  message.style.display = 'none';
  resultsDiv.innerHTML = '<div class="hint">Aquí aparecerá la tarjeta del resultado (si se encuentra).</div>';
}

async function onSubmit(evt) {
  evt.preventDefault();
  const docIdx = docSelect.value;
  if (docIdx === '' || docSelect.options[docSelect.selectedIndex].disabled) { 
    showMessage('Selecciona un documento válido.', true); 
    return; 
  }

  const doc = FRONTEND_CONFIG.documents[docIdx];
  const codeVal = codeInput.value.trim();
  if (!codeVal) { showMessage('Introduce un código numérico.', true); return; }

  if (!/^\d+$/.test(codeVal)) {
    showMessage('El código debe contener sólo dígitos 0-9.', true);
    return;
  }

  if (!FRONTEND_CONFIG.webAppUrl || FRONTEND_CONFIG.webAppUrl.includes('REPLACE_WITH_YOUR_WEBAPP_URL')) {
    showMessage('Debe configurar la URL del Web App de Apps Script en FRONTEND_CONFIG.webAppUrl', true);
    return;
  }

  showSpinner(true);
  showMessage('', false);

  const payload = {
    documentId: doc.id,
    code: codeVal,
    sheetNames: doc.sheetNames,
    columns: doc.columns,
    codeColumn: doc.codeColumn,
    dateColumn: doc.dateColumn
  };

  try {
    const res = await fetch(FRONTEND_CONFIG.webAppUrl, {
      method: 'POST',
      headers: {'Content-Type': 'application/json'},
      body: JSON.stringify(payload)
    });

    if (!res.ok) throw new Error('Fallo de red: ' + res.status);

    const json = await res.json();
    if (json.ok) {
      showResultCard(json.sheet, json.data, doc);
    } else {
      showMessage(json.message || 'No se encontró información.', true);
      resultsDiv.innerHTML = `<div class="hint">${escapeHtml(json.message || 'No se encontró información.')}</div>`;
    }
  } catch (err) {
    console.error(err);
    showMessage('Error en la petición: ' + err.message, true);
    resultsDiv.innerHTML = `<div class="hint">Error en la petición: ${escapeHtml(err.message)}</div>`;
  } finally {
    showSpinner(false);
  }
}

function showSpinner(on) {
  spinner.style.display = on ? 'block' : 'none';
  searchBtn.disabled = on;
}

function showMessage(txt, isError) {
  if (!txt) { message.style.display = 'none'; return; }
  message.style.display = 'block';
  message.textContent = txt;
  message.style.borderLeft = isError ? '4px solid #ef4444' : '4px solid var(--accent)';
}

function showResultCard(sheetName, dataObj, doc) {
  cardContent.innerHTML = '';
  modalTitle.textContent = `${doc.name} — Código ${dataObj[doc.codeColumn] || ''}`;
  modalMeta.textContent = `Hoja: ${sheetName} • Fecha: ${dataObj._dateFound ? new Date(dataObj._dateFound).toLocaleString() : 'N/D'}`;

  doc.columns.forEach(col => {
    const f = document.createElement('div');
    f.className = 'field';
    const label = document.createElement('strong');
    label.textContent = col;
    const val = document.createElement('div');
    val.className = 'value';
    const text = dataObj.hasOwnProperty(col) ? String(dataObj[col] ?? '') : '';
    val.textContent = text;
    const btn = document.createElement('button');
    btn.className = 'copy-btn';
    btn.textContent = 'Copiar';
    btn.style.marginTop = '8px';
    btn.onclick = () => {
      navigator.clipboard.writeText(text).then(()=> {
        btn.textContent = 'Copiado';
        setTimeout(()=>btn.textContent='Copiar',1200);
      });
    };

    f.appendChild(label);
    f.appendChild(val);
    f.appendChild(btn);
    cardContent.appendChild(f);
  });

  modal.classList.add('show');
  modal.setAttribute('aria-hidden', 'false');
}

function closeModal() {
  modal.classList.remove('show');
  modal.setAttribute('aria-hidden', 'true');
}

function copyAll() {
  const nodes = Array.from(cardContent.querySelectorAll('.field'));
  const pairs = nodes.map(n => {
    const k = n.querySelector('strong').textContent;
    const v = n.querySelector('.value').textContent;
    return `${k}: ${v}`;
  });
  const txt = pairs.join('\n');
  navigator.clipboard.writeText(txt).then(()=> {
    copyAllBtn.textContent = 'Copiado';
    setTimeout(()=>copyAllBtn.textContent='Copiar todo', 1200);
  });
}

function escapeHtml(s) {
  return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}

/* ========== Añadir documento dinámicamente ========== */
function onAddDoc(e) {
  e.preventDefault();
  const name = (newName.value || '').trim();
  const id = (newId.value || '').trim();
  const sheets = (newSheets.value || '').split(',').map(s=>s.trim()).filter(Boolean);
  const cols = (newColumns.value || '').split(',').map(s=>s.trim()).filter(Boolean);
  const codeCol = (newCodeCol.value || '').trim() || 'CODIGO';
  const dateCol = (newDateCol.value || '').trim() || 'FECHA ULTIMO CONTEO';

  if (!name || !id) {
    showMessage('Nombre e ID son obligatorios para añadir documento.', true);
    return;
  }

  FRONTEND_CONFIG.documents.push({
    name, id,
    sheetNames: sheets.length ? sheets : ['CONTEO GENERAL','CONTABILIZADO CICLICO AGOS 2025','CONTABILIZADO','DIFERENCIAS.'],
    columns: cols.length ? cols : FRONTEND_CONFIG.documents[0].columns,
    codeColumn: codeCol,
    dateColumn: dateCol
  });

  populateDocSelect();
  newName.value = ''; newId.value = ''; newSheets.value = ''; newColumns.value = ''; newCodeCol.value=''; newDateCol.value='';
  showMessage('Documento añadido correctamente.', false);
}

function onResetDocs(e) {
  e.preventDefault();
  FRONTEND_CONFIG = JSON.parse(JSON.stringify(FRONTEND_CONFIG_DEFAULT));
  populateDocSelect();
  showMessage('Lista de documentos reiniciada a la configuración original.', false);
}

init();
</script>
</body>
</html>
