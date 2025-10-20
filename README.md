<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Logística & Inventario</title>
  <style>
    :root{
      --red:#c62828; --black:#121212; --bg:#0f0f0f;
      --text:#fafafa; --muted:#bdbdbd;
    }
    *{box-sizing:border-box;margin:0;padding:0}
    html,body{height:100%}
    body{font-family:Inter, system-ui, Arial;background:linear-gradient(180deg,var(--bg),#070707);color:var(--text);min-height:100vh}
    .auth-screen{display:flex;align-items:center;justify-content:center;height:100vh;padding:16px}
    .card.auth-card{background:linear-gradient(180deg,#1a1a1a,#0f0f0f);border-radius:12px;padding:28px;width:100%;max-width:420px;box-shadow:0 10px 30px rgba(0,0,0,.6);border:1px solid rgba(255,255,255,0.04);text-align:center}
    .auth-card h1{margin-bottom:6px;font-size:22px}.muted{color:var(--muted)}
    .auth-card input{width:100%;padding:10px 12px;margin:8px 0;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:rgba(255,255,255,0.02);color:var(--text)}
    .row{display:flex;gap:8px;justify-content:center;flex-wrap:wrap}
    button{background:var(--red);border:0;padding:10px 14px;border-radius:10px;color:white;cursor:pointer;transition:transform .12s, box-shadow .12s}
    button.alt{background:transparent;border:1px solid rgba(255,255,255,0.08)}button.google{background:#fff;color:#222}
    button:hover{transform:translateY(-3px);box-shadow:0 10px 20px rgba(198,40,40,0.12)}
    .hidden{display:none}

    /* Topbar / nav re-worked to avoid overlaps en móvil */
    .topbar{display:flex;align-items:center;justify-content:space-between;padding:12px 18px;background:linear-gradient(90deg,rgba(0,0,0,0.6),rgba(18,18,18,0.9));border-bottom:1px solid rgba(255,255,255,0.03)}
    .brand{font-weight:700;color:var(--red);margin-right:10px}
    .nav{display:flex;align-items:center;gap:16px;flex:1;justify-content:space-between;flex-wrap:wrap}
    .nav .menu{display:flex;gap:12px;flex-wrap:wrap;align-items:center}
    .nav .menu a{color:var(--muted);text-decoration:none;padding:4px 6px}
    .nav .menu a.active{color:var(--text);border-bottom:2px solid var(--red);padding-bottom:6px}
    .nav .user-info{display:flex;align-items:center;gap:10px}
    .user-info span{color:var(--muted);font-size:0.95rem;max-width:220px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
    .ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px 10px;border-radius:8px;color:var(--muted)}

    .container{padding:20px;display:grid;grid-template-columns:1fr;gap:18px}
    .section{display:none}.active-section{display:block}
    .search-card{background:linear-gradient(180deg,#151515,#0f0f0f);padding:16px;border-radius:12px;border:1px solid rgba(255,255,255,0.03)}
    .search-card input{flex:1;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:var(--text)}
    .search-card .row{display:flex;gap:8px;flex-wrap:wrap}
    .results{margin-top:12px;background:rgba(255,255,255,0.02);padding:10px;border-radius:8px;min-height:40px}
    .results table{width:100%;border-collapse:collapse;margin-top:10px}
    .results th,.results td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.08);text-align:left;vertical-align:middle}
    .results th{color:var(--red);white-space:nowrap}
    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:12px}
    .card-item{background:rgba(255,255,255,0.02);padding:18px;border-radius:12px;text-align:center;min-height:180px;display:flex;flex-direction:column;justify-content:center}
    .card-item img{max-width:100%;border-radius:8px;height:140px;object-fit:cover;background:#111}
    .news-card{margin-top:12px;padding:12px;background:linear-gradient(180deg,#1b0f0f,#120909);border-radius:12px;border:1px solid rgba(198,40,40,0.08)}
    .news-card img{max-width:100%;border-radius:8px;display:block;margin-bottom:8px}
    input[type=file]{color:var(--muted);margin:8px 0}
    .news-list .item{padding:10px;border-radius:8px;margin:8px 0;background:rgba(255,255,255,0.02)}
    .contact-buttons a{display:inline-block;margin:6px 12px 6px 0;padding:10px 14px;background:var(--red);border-radius:10px;color:#fff;text-decoration:none}

    /* placeholder para imagen si no carga */
    .placeholder {
      width:100%;
      height:140px;
      display:flex;
      align-items:center;
      justify-content:center;
      background:#0b0b0b;
      color:var(--muted);
      border-radius:8px;
      font-size:14px;
    }

    @media(min-width:900px){
      .container{grid-template-columns:1fr 420px}
      .news-card{grid-column:2}
    }

    /* Mobile tweaks to avoid overlap */
    @media(max-width:640px){
      .nav{flex-direction:column;align-items:flex-start;gap:8px}
      .nav .user-info{width:100%;justify-content:space-between}
      .card-item{min-height:160px;padding:14px}
      .results th{font-size:13px}
    }

  </style>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
</head>
<body>
<div id="app">
  <!-- LOGIN -->
  <div class="auth-screen" id="authScreen">
    <div class="card auth-card">
      <h1>Logística & Inventario</h1>
      <p class="muted">Accede para información</p>
      <input id="name" placeholder="Nombre" />
      <input id="email" placeholder="Correo" type="email" />
      <input id="password" placeholder="Contraseña" type="password" />
      <div class="row">
        <button id="btnRegister">Registrarse</button>
        <button id="btnLogin" class="alt">Iniciar sesión</button>
      </div>
      <br>
      <div class="row">
        <button id="btnGoogle" class="google">👤Iniciar con Google</button>
      </div>
      <br>
      <p class="muted small">Los datos se guardarán en el sistema.</p>
    </div>
  </div>

  <!-- PANEL PRINCIPAL -->
  <div id="mainApp" class="hidden">
    <header class="topbar">
      <div style="display:flex;align-items:center;gap:10px">
        <div class="brand">Logística & Inventario</div>
      </div>

      <nav class="nav" aria-label="main navigation">
        <div class="menu">
          <a href="#" data-section="home" class="active">Inicio</a>
          <a href="#" data-section="documents">Documentos</a>
          <a href="#" data-section="images">Imágenes</a>
          <a href="#" data-section="news">Noticias</a>
          <a href="#" data-section="contact">Contacto</a>
        </div>
        <div class="user-info">
          <span id="userEmail"></span>
          <button id="btnLogout" class="ghost">Cerrar sesión</button>
        </div>
      </nav>
    </header>

    <main class="container">
      <!-- INICIO -->
      <section id="home" class="section active-section">
        <div class="search-card">
          <h2>Buscar código en inventario</h2>
          <div class="row" style="margin-top:8px">
            <input id="searchCode" placeholder="Ingrese código..." />
            <button id="btnSearch">Buscar</button>
          </div>
          <div id="searchResults" class="results"></div>
        </div>
        <div class="news-card" id="mainNews"></div>
      </section>

      <!-- DOCUMENTOS -->
      <section id="documents" class="section">
        <h2>Documentos</h2>
        <input type="file" id="docFile" />
        <input id="docName" placeholder="Nombre del archivo (obligatorio)" />
        <br>
        <br>
        <button id="btnUploadDoc">Subir documento</button>
        <div id="docStatus"></div>
        <h3 style="margin-top:20px;">Archivos subidos</h3>
        <input id="filterName" placeholder="Filtrar por nombre de usuario" />
        <input id="filterEmail" placeholder="Filtrar por correo" />
        <input id="filterDate" placeholder="Filtrar por fecha (dd/mm/yyyy)" />
        <button id="btnFilter">Filtrar</button>
        <div id="docList" class="grid"></div>
      </section>

      <!-- IMÁGENES -->
      <section id="images" class="section">
        <h2>Imágenes</h2>
        <input type="file" id="imgFile" accept="image/*" />
        <input id="imgName" placeholder="Nombre de la imagen (obligatorio)" />
        <br>
        <br>
        <button id="btnUploadImg">Subir imagen</button>
        <div id="imgStatus"></div>
        <h3 style="margin-top:20px;">Imágenes subidas</h3>
        <input id="filterName" placeholder="Filtrar por nombre de usuario" />
        <input id="filterEmail" placeholder="Filtrar por correo" />
        <input id="filterDate" placeholder="Filtrar por fecha (dd/mm/yyyy)" />
        <button id="btnFilter">Filtrar</button>
        <div id="imgList" class="grid"></div>
      </section>

      <!-- NOTICIAS -->
      <section id="news" class="section">
        <h2>Noticias</h2>
        <div id="newsList" class="news-list"></div>
      </section>

      <!-- CONTACTO -->
      <section id="contact" class="section">
        <h2>Contacto</h2>
        <div class="contact-buttons">
          <a id="waBtn" target="_blank">WhatsApp</a>
          <a id="mailBtn" href="#">Correo</a>
        </div>
      </section>
    </main>
  </div>
</div>
<!-- Aquí va la Parte 2 (script) -->
  <script>
/* ====== CONFIG ====== */
const API_URL = 'https://script.google.com/macros/s/AKfycbwpJDMExutOwgXORLv2gDzogf6abTQ9ICBRWvtmQu733KD4RjaruAB31Th34ZrPwyYBwA/exec';

const firebaseConfig = {
  apiKey: "AIzaSyDpBoHiKKC7MLQHiWV9psdE3eJ4YuR66GU",
  authDomain: "pagina-dda30.firebaseapp.com",
  projectId: "pagina-dda30",
  appId: "1:14427407275:web:8cba04677fdeef39a088ee"
};
firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();

/* ====== Helpers ====== */

// Convierte una URL de Drive (file.getUrl / open?id=ID / drive.google.com/uc?...) a un enlace embebible directo
function getDriveDirectUrl(url) {
  if (!url) return '';
  try {
    // Si ya es el formato directo, devuélvelo
    if (url.includes('uc?export=view') || url.includes('=export')) return url;
    // buscar /d/FILE_ID/
    const m1 = url.match(/\/d\/([a-zA-Z0-9_-]+)/);
    if (m1 && m1[1]) return `https://drive.google.com/uc?export=view&id=${m1[1]}`;
    // buscar open?id=FILE_ID
    const m2 = url.match(/[?&]id=([a-zA-Z0-9_-]+)/);
    if (m2 && m2[1]) return `https://drive.google.com/uc?export=view&id=${m2[1]}`;
    // si es una url normal, intentar devolverla (puede funcionar)
    return url;
  } catch (e) { return url; }
}

// safeString: devuelve string seguro (evita undefined/null)
function safeString(v){ return v === undefined || v === null ? '' : String(v); }

// Busca una propiedad en objeto con variantes de nombres (case-insensitive)
function getField(obj, ...keys) {
  for (const k of keys) {
    if (obj[k] !== undefined && obj[k] !== null) return obj[k];
  }
  // lowercase keys
  const lowerMap = {};
  for (const p in obj) lowerMap[p.toLowerCase()] = obj[p];
  for (const k of keys) {
    const low = k.toLowerCase();
    if (lowerMap[low] !== undefined) return lowerMap[low];
  }
  return '';
}

/* ====== AUTH ====== */
auth.onAuthStateChanged(user => {
  if (user) {
    document.getElementById('authScreen').classList.add('hidden');
    document.getElementById('mainApp').classList.remove('hidden');
    document.getElementById('userEmail').textContent = user.email || user.displayName || '';
    loadNews();
    listUploads();
  } else {
    document.getElementById('authScreen').classList.remove('hidden');
    document.getElementById('mainApp').classList.add('hidden');
  }
});

/* Registro / Login / Google */
document.getElementById('btnRegister').addEventListener('click', async () => {
  const name = document.getElementById('name').value.trim();
  const email = document.getElementById('email').value.trim();
  const pass = document.getElementById('password').value;
  if (!name || !email || !pass) return alert('Completa todos los campos.');
  try {
    const userCred = await auth.createUserWithEmailAndPassword(email, pass);
    await userCred.user.updateProfile({ displayName: name });
    await callApi('registerUser', { user: { name, email, uid: userCred.user.uid } });
    alert('Registrado correctamente');
  } catch (err) { alert(err.message); }
});
document.getElementById('btnLogin').addEventListener('click', async () => {
  const email = document.getElementById('email').value.trim();
  const pass = document.getElementById('password').value;
  try { await auth.signInWithEmailAndPassword(email, pass); } catch (err) { alert(err.message); }
});
document.getElementById('btnGoogle').addEventListener('click', async () => {
  const provider = new firebase.auth.GoogleAuthProvider();
  try {
    const res = await auth.signInWithPopup(provider);
    const user = res.user;
    await callApi('registerUser', { user: { name: user.displayName, email: user.email, uid: user.uid } });
  } catch (err) { alert(err.message); }
});
document.getElementById('btnLogout').addEventListener('click', () => auth.signOut());

/* ====== API Helpers ====== */
async function getApi(action, params = {}) {
  const q = new URLSearchParams(Object.assign({ action }, params)).toString();
  const res = await fetch(API_URL + '?' + q);
  return res.json();
}
async function callApi(action, payload = {}) {
  const isUpload = action === 'uploadFile';
  const body = JSON.stringify(Object.assign({ action }, payload));
  if (isUpload) {
    // Uploads: backend no permite CORS headers, usamos no-cors (opaco) y confirmamos con listUploads
    await fetch(API_URL, { method: 'POST', body, mode: 'no-cors' });
    return { ok: true, opaque: true };
  } else {
    const res = await fetch(API_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body
    });
    return res.json();
  }
}

/* ====== BUSCAR CÓDIGO (robusta) ====== */
document.getElementById('btnSearch').addEventListener('click', async () => {
  const code = document.getElementById('searchCode').value.trim();
  if (!code) return alert('Ingrese un código.');
  const data = await getApi('search', { code });
  const out = document.getElementById('searchResults');

  if (data && data.results && data.results.length) {
    const rows = data.results.map(r => {
      // reconozco múltiples variantes y aseguro string
      const codigo = safeString(getField(r,'codigo','Código','code')) || '';
      const nombre = safeString(getField(r,'nombre','Nombre','name')) || '';
      const descripcion = safeString(getField(r,'descripcion','Descripción','description')) || '';
      const um = safeString(getField(r,'um','UM')) || '';
      const ubic = safeString(getField(r,'ubicacionFisicaGeneral','ubicacion fisica general','ubicacion','ubicaciongeneral')) || '';
      const cantidad = safeString(getField(r,'cantidad','Cant')) || '';
      const ubicExhib = safeString(getField(r,'ubicacionExhib','ubicacion exhib')) || '';
      const conteo = safeString(getField(r,'conteoExhb','conteo exhb','conteo_exhb')) || '';
      const fechaUlt = safeString(getField(r,'fechaUltimoConteo','fecha ultimo conteo','fecha')) || '';
      const auditor = safeString(getField(r,'ultimoAuditor','ultimo auditor en contar','ultimo auditor')) || '';
      const difG = safeString(getField(r,'difGeneral','dif_general')) || '';
      const difE = safeString(getField(r,'difExhib','dif_exhib')) || '';

      return `<tr>
        <td style="min-width:120px">${codigo}</td>
        <td>${nombre}</td>
        <td>${descripcion}</td>
        <td>${um}</td>
        <td>${ubic}</td>
        <td>${cantidad}</td>
        <td>${ubicExhib}</td>
        <td>${conteo}</td>
        <td>${fechaUlt}</td>
        <td>${auditor}</td>
        <td>${difG}</td>
        <td>${difE}</td>
      </tr>`;
    }).join('');

    out.innerHTML = `<table>
      <tr>
        <th>Código</th><th>Nombre</th><th>Descripción</th><th>UM</th>
        <th>Ubicación Física</th><th>Cantidad</th><th>Ubicación Exhib</th><th>Conteo Exhib</th>
        <th>Fecha Último Conteo</th><th>Auditor</th><th>DIF General</th><th>DIF Exhib</th>
      </tr>${rows}</table>`;
  } else {
    out.innerHTML = '<div class="muted">Sin resultados</div>';
  }
});

/* ====== UPLOAD helpers ====== */
async function uploadFile(file, filenameOpt) {
  const base64 = await new Promise(res => {
    const r = new FileReader();
    r.onload = e => res(e.target.result.split(',')[1]);
    r.readAsDataURL(file);
  });
  const uploaderName = auth.currentUser?.displayName || "";
  const uploaderEmail = auth.currentUser?.email || "";
  return callApi('uploadFile', {
    filename: filenameOpt || file.name,
    mimeType: file.type,
    base64,
    uploaderName,
    uploaderEmail
  });
}

/* Subir documento */
document.getElementById('btnUploadDoc').addEventListener('click', async () => {
  const f = document.getElementById('docFile');
  const name = document.getElementById('docName').value.trim();
  if (!f.files.length) return alert('Selecciona un archivo');
  if (!name) return alert('El nombre del documento es obligatorio');
  document.getElementById('docStatus').textContent = 'Subiendo...';
  try {
    await uploadFile(f.files[0], name);
    await listUploads(true, name);
  } catch (e) {
    document.getElementById('docStatus').textContent = 'Error al subir';
  }
});

/* Subir imagen */
document.getElementById('btnUploadImg').addEventListener('click', async () => {
  const f = document.getElementById('imgFile');
  const name = document.getElementById('imgName').value.trim();
  if (!f.files.length) return alert('Selecciona una imagen');
  if (!name) return alert('El nombre de la imagen es obligatorio');
  document.getElementById('imgStatus').textContent = 'Subiendo...';
  try {
    await uploadFile(f.files[0], name);
    await listUploads(true, name);
  } catch (e) {
    document.getElementById('imgStatus').textContent = 'Error al subir';
  }
});

/* ====== Listado / Render archivos e imágenes ====== */
document.getElementById('btnFilter').addEventListener('click', () => listUploads());

async function listUploads(findAndShowName=false, findName='') {
  const res = await getApi('listUploads');
  const docs = res.docs || [];
  const imgs = res.imgs || [];

  const nameFilter = (document.getElementById('filterName')?.value || '').trim().toLowerCase();
  const emailFilter = (document.getElementById('filterEmail')?.value || '').trim().toLowerCase();
  const dateFilter = (document.getElementById('filterDate')?.value || '').trim();

  const filterFn = x => {
    const n = (x.uploaderName || '').toString().toLowerCase();
    const e = (x.uploaderEmail || '').toString().toLowerCase();
    return (!nameFilter || n.includes(nameFilter)) && (!emailFilter || e.includes(emailFilter)) && (!dateFilter || x.date === dateFilter);
  };

  const docList = document.getElementById('docList');
  const imgList = document.getElementById('imgList');

  const filteredDocs = docs.filter(filterFn);
  const filteredImgs = imgs.filter(filterFn);

  // Render documentos
  if (filteredDocs.length) {
    docList.innerHTML = filteredDocs.map(d => {
      const direct = getDriveDirectUrl(d.url);
      return `<div class="card-item">
        <div style="flex:0 0 auto">
          <a href="${d.url}" target="_blank" style="font-weight:bold;color:#cce">${d.name}</a>
        </div>
        <div style="font-size:12px;color:var(--muted);margin-top:8px">
          📅 ${safeString(d.date)}<br>👤 ${safeString(d.uploaderName)||'Sin nombre'}<br>✉️ ${safeString(d.uploaderEmail)||'Sin correo'}
        </div>
      </div>`;
    }).join('');
  } else {
    docList.innerHTML = '<p class="muted">Sin documentos</p>';
  }

  // Render imágenes (usar direct link para mostrar la miniatura)
  if (filteredImgs.length) {
    imgList.innerHTML = filteredImgs.map(i => {
      const direct = getDriveDirectUrl(i.url);
      // img con lazy + placeholder onerror
      return `<div class="card-item">
        <a href="${i.url}" target="_blank" style="display:block">
          <img loading="lazy" src="${direct}" alt="${i.name}" onerror="this.onerror=null;this.style.display='none';this.insertAdjacentHTML('afterend','<div class=&quot;placeholder&quot;>Sin miniatura</div>');" />
        </a>
        <div style="margin-top:10px;font-weight:bold;color:#ccc">${i.name}</div>
        <div style="font-size:12px;color:var(--muted);margin-top:6px">
          📅 ${safeString(i.date)}<br>👤 ${safeString(i.uploaderName)||'Sin nombre'}<br>✉️ ${safeString(i.uploaderEmail)||'Sin correo'}
        </div>
      </div>`;
    }).join('');
  } else {
    imgList.innerHTML = '<p class="muted">Sin imágenes</p>';
  }

  // Mostrar enlace del archivo recién subido (si aplica)
  if (findAndShowName && findName) {
    const found = [...docs,...imgs].find(x => x.name === findName) || (docs.concat(imgs))[0];
    if (found) {
      const isImg = (found.mime && found.mime.toLowerCase().startsWith('image/')) || /\.(jpg|jpeg|png|gif|webp)$/i.test(found.name);
      if (isImg) document.getElementById('imgStatus').innerHTML = `Subida: <a href="${found.url}" target="_blank">Abrir</a>`;
      else document.getElementById('docStatus').innerHTML = `Subido: <a href="${found.url}" target="_blank">Abrir</a>`;
    } else {
      document.getElementById('imgStatus').textContent = '';
      document.getElementById('docStatus').textContent = '';
    }
  }
}

/* ====== Noticias (usa getDriveDirectUrl para imagenes) ====== */
async function loadNews(){
  const res = await getApi('listNews');
  const main = document.getElementById('mainNews');
  const list = document.getElementById('newsList');
  if (res && res.news && res.news.length) {
    const n = res.news[0];
    const imgUrl = getDriveDirectUrl(safeString(n.imagenUrl));
    main.innerHTML = `<h3 style="margin-bottom:6px">${safeString(n.titulo)}</h3>${imgUrl ? `<img loading="lazy" src="${imgUrl}" alt="noticia" onerror="this.style.display='none'">` : ''}<p style="margin-top:10px">${safeString(n.contenido)}</p>`;
    list.innerHTML = res.news.map(x => {
      const thumb = getDriveDirectUrl(safeString(x.imagenUrl));
      return `<div class="item" style="margin-bottom:8px">
        <strong>${safeString(x.titulo)}</strong><br><small>${safeString(x.fecha)}</small>
        ${thumb ? `<div style="margin-top:6px"><img loading="lazy" src="${thumb}" alt="${safeString(x.titulo)}" style="max-width:100%;border-radius:6px" onerror="this.style.display='none'"></div>` : ''}
      </div>`;
    }).join('');
  } else {
    main.innerHTML = '<p class="muted">No hay noticias.</p>';
    list.innerHTML = '';
  }
}

/* ====== Navegación (mantener) ====== */
document.querySelectorAll('.nav a').forEach(a => {
  a.addEventListener('click', e => {
    e.preventDefault();
    document.querySelectorAll('.nav a').forEach(n => n.classList.remove('active'));
    a.classList.add('active');
    const target = a.dataset.section;
    document.querySelectorAll('.section').forEach(s => s.id === target ? s.classList.add('active-section') : s.classList.remove('active-section'));
    if (target === 'documents' || target === 'images') listUploads();
  });
});

/* ====== Contacto */
document.getElementById('waBtn').href = "https://wa.me/4129915255?text=Hola%20quiero%20información";
document.getElementById('mailBtn').href = "mailto:derwinrojas351@gmail.com";

/* ====== Inicial ====== */
// Si el usuario ya está logueado, onAuthStateChanged lo cargará.
// Pero también podemos intentar listar (por si el auth tarda)
setTimeout(()=>{ if (auth.currentUser) { listUploads(); loadNews(); } }, 800);
</script>
</body>
</html>
