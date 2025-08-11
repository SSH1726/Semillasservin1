<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Chiles y Cereales Servín</title>
<link rel="icon" href="https://a8df956abb.cbaul-cdnwnd.com/f87890b711a8506804ac4dab45807d6c/200000000-0f1d8101b8/700/servin%20logo.jpg?ph=a8df956abb">
<style>
:root{
  /* colores base */
  --vino:#800000;   /* header */
  --crema:#fff5e1;  /* chips */
  --crema2:#e6d5c1;
  --verde:#25d366;  /* whatsapp */
  --naranja:#ff9800;/* botón carrito */
  --texto:#2b2b2b;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,Helvetica,sans-serif;background:#f7f7fb;color:var(--texto);padding-top:70px}

/* header */
header{position:fixed;top:0;left:0;width:100%;z-index:1000;background:var(--vino);color:#fff;display:flex;align-items:center;padding:10px 14px;gap:14px}
header h1{flex:1;text-align:center;font-size:1.5em;white-space:nowrap}
.menu-toggle{font-size:26px;background:none;border:none;color:#fff;cursor:pointer}
.logo{height:48px}
.btn-carrito{background:var(--naranja);color:#fff;border:none;padding:8px 12px;border-radius:5px;font-weight:bold;cursor:pointer;white-space:nowrap}
.menu{display:none;position:absolute;top:100%;left:0;background:#fff;width:200px;box-shadow:0 2px 6px rgba(0,0,0,.25)}
.menu a{display:block;padding:10px;font-weight:bold;color:var(--vino);text-decoration:none;border-bottom:1px solid #eee}
.menu a:hover{background:#f7f7f7}

/* buscador + categorías */
.buscador{display:flex;justify-content:center;margin:20px 0}
#search{width:90%;max-width:420px;padding:10px;border:1px solid #ccc;border-radius:6px}
.categorias{display:flex;flex-wrap:wrap;justify-content:center;gap:10px;margin-bottom:10px}
.categorias button{background:var(--crema);border:1px solid #ccc;border-radius:999px;padding:8px 14px;font-weight:bold;cursor:pointer}
.categorias button.active{background:var(--crema2);border-color:var(--vino)}

/* productos */
.productos{display:flex;flex-wrap:wrap;gap:20px;justify-content:center;padding:0 10px 80px}
.producto{width:220px;background:#fff;border:1px solid #e7e7e7;border-radius:12px;padding:14px;text-align:center;box-shadow:2px 2px 6px rgba(0,0,0,.06)}
.producto img{max-width:100%;border-radius:8px;margin-bottom:8px;object-fit:cover}
.producto h3{font-size:16px;min-height:40px;margin-bottom:6px}
.precio{color:#178c2e;font-weight:bold;margin:.3em 0}
select{width:100%;padding:8px;margin:8px 0;border:1px solid #ccc;border-radius:6px}
.btn-agregar{background:var(--vino);color:#fff;border:none;padding:10px;border-radius:8px;font-weight:bold;cursor:pointer;width:100%}

/* carrito lateral */
#carrito{display:none;position:fixed;top:70px;right:20px;background:#fff;max-width:320px;max-height:65vh;overflow:auto;box-shadow:0 6px 16px rgba(0,0,0,.2);border-radius:10px;padding:18px;z-index:999}
#carrito h3{margin-bottom:8px}
.carrito-item{display:flex;justify-content:space-between;gap:8px;margin-bottom:8px;border-bottom:1px dashed #eee;padding-bottom:6px}
.carrito-item button{background:none;border:1px solid #e76c6c;color:#c62828;border-radius:6px;padding:2px 8px;font-weight:bold;cursor:pointer}
.total{margin-top:8px;font-weight:bold;text-align:right}

/* botón WhatsApp flotante */
.whatsapp{position:fixed;bottom:20px;right:20px;background:var(--verde);color:#fff;padding:14px 20px;border-radius:50px;font-weight:bold;text-decoration:none;box-shadow:0 2px 6px rgba(0,0,0,.25)}

/* responsive */
@media(max-width:768px){.producto{max-width:45%}}
@media(max-width:600px){.producto{width:90%}}
@media(max-width:480px){
  header h1{font-size:1.2em}
  .producto{max-width:100%}
  .productos{gap:14px;padding-bottom:90px}
  .btn-agregar{padding:14px 0;font-size:16px}
}
</style>
</head>
<body>

<header>
  <button class="menu-toggle" onclick="menu.style.display=menu.style.display==='block'?'none':'block'">☰</button>
  <img src="https://a8df956abb.cbaul-cdnwnd.com/f87890b711a8506804ac4dab45807d6c/200000000-0f1d8101b8/700/servin%20logo.jpg?ph=a8df956abb" class="logo" onclick="location='index.html'" alt="logo">
  <h1>Chiles y Cereales Servín</h1>
  <button class="btn-carrito" onclick="toggleCart()">Carrito (<span id="cartCount">0</span>)</button>
  <nav id="menu" class="menu">
    <a href="index.html">Haz tu pedido!</a>
    <a href="about.html">Sobre Nosotros</a>
    <a href="about.html#comentarios">Comentarios</a>
  </nav>
</header>

<div class="buscador">
  <input id="search" placeholder="Buscar producto..." oninput="filter()">
</div>
<div id="catBtns" class="categorias"></div>
<div id="productos" class="productos"><p>Cargando…</p></div>

<!-- Carrito -->
<div id="carrito">
  <h3>Carrito</h3>
  <div id="cartItems"></div>
  <div class="total">Total: $<span id="cartTotal">0.00</span></div>
  <button onclick="sendWhatsApp()" style="margin-top:10px;width:100%">Enviar pedido</button>
</div>

<a class="whatsapp" href="https://wa.me/524611267217" target="_blank">WhatsApp</a>

<script>
/* ---------- CONFIG ---------- */
const SHEET_ID = '1V3idgBFfpsIJdBs3Z6B8FmXOc9e6rGtF45Y_heR96B0'; // tu ID de Google Sheet
// Columnas: A nombre, B categoría, C precio/kg, D imagen, E precio costal,
// F por kg, G por costal, H peso costal, I por pz, J precio pz,
// K por caja, L precio caja, M por cubeta, N precio cubeta.
const QUERY   = 'select A,B,C,D,E,F,G,H,I,J,K,L,M,N';
/* -------------------------------- */

const $ = s => document.querySelector(s);
const $$ = s => document.querySelectorAll(s);
const norm = s => (s||'').toString().toLowerCase()
  .normalize('NFD').replace(/[\u0300-\u036f]/g,'').trim();

const cart    = () => JSON.parse(localStorage.getItem('cart')||'[]');
const save    = c  => localStorage.setItem('cart',JSON.stringify(c));
const updateBadge = () => $('#cartCount').textContent = cart().length;

/* Cargar productos */
fetch(`https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tq=${encodeURIComponent(QUERY)}`)
  .then(r=>r.text()).then(t=>{
    const data = JSON.parse(t.substr(47).slice(0,-2));
    if(!data.table || !data.table.rows){
      $('#productos').innerHTML = '<p>Error cargando productos</p>';
      return;
    }
    render(data.table.rows);
  }).catch(()=>{
    $('#productos').innerHTML = '<p>Error cargando productos</p>';
  });

function render(rows){
  const cats = new Set(['todas']);
  const cont = $('#productos'); cont.innerHTML = '';

  rows.forEach(r=>{ if(r.c[1]) cats.add(r.c[1].v.trim()); });

  /* Botones de categoría */
  const cb = $('#catBtns'); cb.innerHTML = '';
  cats.forEach(cat=>{
    const b = document.createElement('button');
    b.textContent = cat==='todas' ? 'Ver todos' : cat;
    b.onclick = ()=>{ currCat=norm(cat); setActive(b); filter(); };
    if(cat==='todas'){ b.classList.add('active'); currCat='todas'; }
    cb.appendChild(b);
  });

  /* Tarjetas de producto */
  rows.forEach(r=>{
    const [n,cat,p,img,pc,kgTf,costTf,peso,cantPz,pz,cajaTf,cajaP,cubTf,cubP] = r.c.map(c=>c?.v||'');
    const div = document.createElement('div'); div.className='producto';
    div.dataset.cat  = norm(cat);
    div.dataset.name = norm(n);

    // Precios por unidad
    div.dataset.pk  = p || 0;       // precio por kg
    div.dataset.pc  = pc || 0;      // precio costal
    div.dataset.peso = peso || 0;   // kg del costal
    div.dataset.pp  = pz || 0;      // precio pieza
    // Si no tienes L (precio por caja) en tu hoja, iguala pcj a pc:
    div.dataset.pcj = (cajaP!==''?cajaP:pc) || 0;
    div.dataset.pcb = cubP || 0;    // precio cubeta

    // Select dinámico
    const isTrue = v => v===true || v==='TRUE' || v==='true';
    let opts = '<select onchange="calcPrecio(this)">';
    let tieneAlgo = false;

    if(isTrue(kgTf)){
      opts += `
        <option value="250">250 g</option>
        <option value="500">500 g</option>
        <option value="750">750 g</option>
        <option value="1000" selected>1 kg</option>`;
      tieneAlgo = true;
    }
    if(isTrue(costTf)){ opts += `<option value="costal">Costal (${div.dataset.peso} kg)</option>`; tieneAlgo = true; }
    if(isTrue(cantPz)){ opts += `<option value="pieza">Pieza</option>`; tieneAlgo = true; }
    if(isTrue(cajaTf)){ opts += `<option value="caja">Caja</option>`; tieneAlgo = true; }
    if(isTrue(cubTf)){  opts += `<option value="cubeta">Cubeta</option>`; tieneAlgo = true; }
    if(!tieneAlgo){     opts += `<option value="unica" selected>Unidad única</option>`; }

    opts += '</select>';

    div.innerHTML = `
      <img src="${img||'https://via.placeholder.com/320x220?text=Sin+imagen'}" alt="${n}">
      <h3>${n}</h3>
      <p class="precio">$${(+p||0).toFixed(2)}/kg</p>
      ${opts}
      <button class="btn-agregar" onclick="addToCart(this)">Agregar</button>
    `;
    cont.appendChild(div);
  });

  filter();
}

/* Filtro */
let currCat = 'todas';
function setActive(btn){
  $$('.categorias button').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
}
function filter(){
  const term = norm($('#search').value);
  $$('.producto').forEach(p=>{
    const okCat = (currCat==='todas'||p.dataset.cat===currCat);
    const okTxt = (term==='' || p.dataset.name.includes(term));
    p.style.display = (okCat && okTxt) ? 'block' : 'none';
  });
}

/* Cálculo de precio por unidad seleccionada */
function calcPrecio(sel){
  const p = sel.closest('.producto');
  const v = sel.value;
  const pk = +p.dataset.pk, pc = +p.dataset.pc, pp = +p.dataset.pp,
        pcj = +p.dataset.pcj, pcb = +p.dataset.pcb;
  let precio = pk;  // por defecto 1 kg

  if(v==='750'||v==='500'||v==='250')      precio = pk*(+v/1000);
  else if(v==='costal')                    precio = pc;
  else if(v==='pieza')                     precio = pp;
  else if(v==='caja')                      precio = pcj;
  else if(v==='cubeta')                    precio = pcb;

  p.querySelector('.precio').textContent = '$'+precio.toFixed(2);
}

/* Carrito: agregar y acumular */
function addToCart(btn){
  const p   = btn.closest('.producto');
  const sel = p.querySelector('select').value;
  const precioUnit = +p.querySelector('.precio').textContent.replace(/[^0-9.]/g,'');
  const nombre     = p.querySelector('h3').textContent;
  const ct = sel==='costal'?`Costal (${p.dataset.peso} kg)`:
             sel==='pieza' ? 'Pieza' :
             sel==='caja'  ? 'Caja'  :
             sel==='cubeta'? 'Cubeta':
             sel==='unica' ? 'Unidad': sel+' g';

  const c   = cart();
  const idx = c.findIndex(x=> x.nombre===nombre && x.ct===ct);

  if (idx > -1) {
    c[idx].qty    += 1;
    c[idx].precio += precioUnit;   // subtotal acumulado
  } else {
    c.push({nombre, ct, qty:1, precio: precioUnit});
  }
  save(c); updateBadge();

  // Feedback en el botón (sin alert)
  btn.textContent = '✓ Agregado';
  btn.disabled = true;
  setTimeout(()=>{ btn.textContent='Agregar'; btn.disabled=false; }, 900);
}

/* Carrito UI */
function toggleCart(){
  const c = $('#carrito');
  c.style.display = c.style.display==='block' ? 'none' : 'block';
  renderCart();
}
function renderCart(){
  const items = cart();
  const ci = $('#cartItems'); ci.innerHTML='';
  let total = 0;

  items.forEach((it,i)=>{
    total += it.precio;
    ci.insertAdjacentHTML('beforeend',`
      <div class="carrito-item">
        <span>${it.nombre} (${it.ct}) <strong>x${it.qty}</strong></span>
        <span>$${it.precio.toFixed(2)}
          <button onclick="removeItem(${i})">x</button>
        </span>
      </div>
    `);
  });
  $('#cartTotal').textContent = total.toFixed(2);
}
function removeItem(i){
  const c = cart(); if(!c[i]) return;
  // Resta una unidad
  const qtyPrev = c[i].qty;
  c[i].qty -= 1;
  // Recalcula subtotal proporcionalmente
  c[i].precio = +(c[i].precio / (qtyPrev) * (c[i].qty)).toFixed(2);
  if(c[i].qty<=0) c.splice(i,1);
  save(c); updateBadge(); renderCart();
}

/* WhatsApp con cantidades */
function sendWhatsApp(){
  const c = cart(); if(!c.length){ alert('Carrito vacío'); return; }
  let msg = '¡Hola! Quiero hacer un pedido:%0A';
  c.forEach(it=>{
    msg += `- ${it.nombre} (${it.ct}) x${it.qty}: $${it.precio.toFixed(2)}%0A`;
  });
  const total = c.reduce((sum,it)=>sum+it.precio,0).toFixed(2);
  msg += `%0ATotal: $${total}`;
  window.open(`https://wa.me/524611267217?text=${msg}`,'_blank');
}

updateBadge();
</script>
</body>
</html>
