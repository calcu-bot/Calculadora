<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Calculadora de Inversión – Memorial</title>
<style>
  :root{
    --bg:#0f172a; --card:#111827; --muted:#9ca3af; --accent:#22c55e; --accent-2:#06b6d4; --danger:#ef4444; --text:#e5e7eb;
    --radius:14px;
  }
  *{box-sizing:border-box;font-family:system-ui,-apple-system,Segoe UI,Roboto,Inter,Arial}
  body{margin:0;background:linear-gradient(135deg,#0b1224,#0f172a 60%);color:var(--text)}
  header{padding:28px 20px 10px;max-width:1100px;margin:0 auto}
  header h1{margin:0 0 6px;font-size:28px}
  header p{margin:0;color:var(--muted)}
  .container{max-width:1100px;margin:18px auto 60px;display:grid;grid-template-columns:1.05fr 1fr;gap:20px;padding:0 20px}
  .card{background:rgba(17,24,39,.7);backdrop-filter: blur(6px);border:1px solid #1f2937;border-radius:var(--radius);padding:18px}
  .card h2{margin:0 0 12px;font-size:18px;color:#cbd5e1}
  .row{display:grid;grid-template-columns:1fr 1fr;gap:12px}
  label{display:block;font-size:13px;color:#cbd5e1;margin:8px 0 6px}
  input,select{width:100%;padding:12px;border-radius:10px;border:1px solid #334155;background:#0b1221;color:#e5e7eb}
  input[type="number"]{appearance:textfield}
  .help{font-size:12px;color:var(--muted);margin-top:4px}
  .mode{display:flex;gap:8px;margin:8px 0 2px}
  .mode button{flex:1;padding:10px;border-radius:10px;border:1px solid #334155;background:#0b1221;color:#e5e7eb;cursor:pointer}
  .mode button.active{border-color:var(--accent);box-shadow:0 0 0 2px rgba(34,197,94,.25) inset}
  .pill{display:inline-block;padding:6px 10px;border-radius:999px;border:1px solid #334155;color:#cbd5e1;margin:2px 6px 0 0;font-size:12px}
  .accent{color:#bbf7d0}
  .actions{display:flex;gap:10px;margin-top:16px}
  .btn{padding:12px 14px;border-radius:12px;border:1px solid #1f2937;background:linear-gradient(180deg,#16a34a,#15803d);color:white;font-weight:600;cursor:pointer}
  .btn.secondary{background:linear-gradient(180deg,#0891b2,#0e7490)}
  .btn.ghost{background:#0b1221}
  .summary{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin-top:12px}
  .kpi{background:#0b1221;border:1px solid #1f2937;border-radius:12px;padding:12px}
  .kpi .label{font-size:12px;color:#94a3b8}
  .kpi .value{font-size:20px;margin-top:4px}
  table{width:100%;border-collapse:collapse;margin-top:14px;border:1px solid #1f2937}
  th,td{padding:10px;border-bottom:1px solid #1f2937;text-align:right}
  th{text-align:right;color:#cbd5e1;background:#0b1221;position:sticky;top:0}
  td:first-child,th:first-child{text-align:left}
  .footer-note{font-size:12px;color:#9ca3af;margin-top:8px}
  @media (max-width:930px){
    .container{grid-template-columns:1fr}
    .row{grid-template-columns:1fr}
    .summary{grid-template-columns:1fr}
  }
</style>
</head>
<body>
<header>
  <h1>Calculadora de Inversión – Memorial</h1>
  <p>Cotiza en <span class="accent">RD$</span> usando el <strong>Método Memorial</strong> (factor por años) o <strong>Amortización estándar</strong>.</p>
</header>

<div class="container">
  <section class="card" id="formCard">
    <h2>Datos de la cotización</h2>

    <div class="row">
      <div>
        <label>Producto</label>
        <select id="producto"></select>
        <div class="help">Catálogo editable (precios de referencia).</div>
      </div>

      <div>
        <label>Cantidad de productos</label>
        <input type="number" id="cantidadProductos" min="1" step="1" value="1" />
        <div class="help">Aplica descuento por volumen si corresponde.</div>
      </div>
    </div>

    <div class="row">
      <div>
        <label>Enganche (RD$)</label>
        <input type="number" id="enganche" min="0" step="1000" value="0" />
      </div>
      <div>
        <label>Plazo (años)</label>
        <select id="plazo">
          <option value="1">1 año</option>
          <option value="2">2 años</option>
          <option value="3">3 años</option>
          <option value="4">4 años</option>
          <option value="5" selected>5 años (recomendado)</option>
        </select>
      </div>
    </div>

    <div>
      <label>Modo de cálculo</label>
      <div class="mode">
        <button type="button" id="modoMemorial" class="active">Método Memorial</button>
        <button type="button" id="modoAmort">Amortización estándar</button>
      </div>
      <span class="pill">Cuotas mensuales</span>
      <span class="pill">Resumen + Cronograma</span>
    </div>

    <div id="panelMemorial">
      <div class="row">
        <div>
          <label>Factor por 1 año</label>
          <input type="number" id="f1" step="0.01" value="1.10" />
        </div>
        <div>
          <label>Factor por 2 años</label>
          <input type="number" id="f2" step="0.01" value="1.20" />
        </div>
      </div>
      <div class="row">
        <div>
          <label>Factor por 3 años</label>
          <input type="number" id="f3" step="0.01" value="1.30" />
        </div>
        <div>
          <label>Factor por 4 años</label>
          <input type="number" id="f4" step="0.01" value="1.40" />
        </div>
      </div>
      <div class="row">
        <div>
          <label>Factor por 5 años</label>
          <input type="number" id="f5" step="0.01" value="1.50" />
        </div>
        <div>
          <label>“Punto común” (opcional)</label>
          <input type="number" id="puntoComun" step="0.01" value="1.00" />
          <div class="help">Si lo usas, multiplica la cuota base por este punto.</div>
        </div>
      </div>
    </div>

    <div id="panelAmort" style="display:none">
      <div class="row">
        <div>
          <label>Tasa nominal anual (%)</label>
          <input type="number" id="tasaAnual" step="0.01" value="24.00" />
        </div>
        <div>
          <label>Cuotas por año</label>
          <input type="number" id="cuotasPorAnio" min="1" step="1" value="12" />
        </div>
      </div>
    </div>

    <div class="actions">
      <button class="btn" id="btnCalcular">Calcular</button>
      <button class="btn secondary" id="btnLimpiar">Limpiar</button>
      <button class="btn ghost" id="btnCompartir">Compartir cotización</button>
    </div>

    <div class="footer-note">Nota: los precios de catálogo son demostrativos; ajusta a tu tarifario real de Memorial.</div>
  </section>

  <section class="card" id="resultadoCard">
    <h2>Resumen</h2>
    <div class="summary" id="summary">
      <!-- KPIs -->
    </div>

    <h2 style="margin-top:18px">Cronograma de pagos</h2>
    <div style="max-height:360px;overflow:auto;border-radius:12px">
      <table id="tabla">
        <thead>
          <tr>
            <th>#</th>
            <th>Cuota</th>
            <th>Interés</th>
            <th>Capital</th>
            <th>Saldo</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
    <div class="footer-note" id="notaModo"></div>
  </section>
</div>

<script>
  // ===== Utilidades =====
  const fmt = new Intl.NumberFormat('es-DO',{style:'currency',currency:'DOP',maximumFractionDigits:2});
  const $ = sel => document.querySelector(sel);

  // ===== Catálogo de productos (edítalo a tu gusto) =====
  const productos = [
    { id:'serv-comfort', nombre:'Servicio Comfort', precio:185000 },
    { id:'serv-elegance', nombre:'Servicio Elegance', precio:260000 },
    { id:'serv-platinum', nombre:'Servicio Platinum', precio:350000 },
    { id:'prop-basica', nombre:'Propiedad Básica (Lote)', precio:350000 },
    { id:'prop-premium', nombre:'Propiedad Premium (Lote)', precio:550000 }
  ];

  // Descuentos por volumen (ejemplo): 2 productos -2%, 3 -3.5%, 4+ -5%
  function descuentoPorVolumen(n){
    if(n>=4) return 0.05;
    if(n===3) return 0.035;
    if(n===2) return 0.02;
    return 0;
  }

  // ===== Poblado inicial =====
  function initProducto(){
    const sel = $('#producto');
    productos.forEach(p=>{
      const o = document.createElement('option');
      o.value = p.id; o.textContent = `${p.nombre} — ${fmt.format(p.precio)}`;
      sel.appendChild(o);
    });
  }

  // ===== Modo de cálculo (UI) =====
  let modo = 'memorial';
  $('#modoMemorial').onclick = ()=>{
    modo='memorial';
    $('#modoMemorial').classList.add('active');
    $('#modoAmort').classList.remove('active');
    $('#panelMemorial').style.display='';
    $('#panelAmort').style.display='none';
  };
  $('#modoAmort').onclick = ()=>{
    modo='amort';
    $('#modoAmort').classList.add('active');
    $('#modoMemorial').classList.remove('active');
    $('#panelAmort').style.display='';
    $('#panelMemorial').style.display='none';
  };

  // ===== Lógica de cálculo =====
  function obtenerPrecioBase(){
    const id = $('#producto').value;
    const prod = productos.find(p=>p.id===id);
    const cantidad = Math.max(1, parseInt($('#cantidadProductos').value||'1',10));
    const desc = descuentoPorVolumen(cantidad);
    const bruto = prod.precio * cantidad;
    const neto = bruto * (1 - desc);
    return { bruto, desc, neto, cantidad, prodNombre: prod.nombre };
  }

  function factors(){
    return {
      1: parseFloat($('#f1').value||'1'),
      2: parseFloat($('#f2').value||'1'),
      3: parseFloat($('#f3').value||'1'),
      4: parseFloat($('#f4').value||'1'),
      5: parseFloat($('#f5').value||'1'),
      punto: parseFloat($('#puntoComun').value||'1')
    };
  }

  function calcularMemorial(totalNeto, enganche, plazoAnos){
    const saldo = Math.max(0, totalNeto - enganche);
    const facs = factors();
    const factor = facs[plazoAnos] ?? 1;
    const cuotasAnio = 12;
    const n = plazoAnos * cuotasAnio;

    // Interpretación del “método Memorial” que nos diste:
    // cuota = (saldo / n) * puntoComun * factorPorAños
    const cuota = (saldo / n) * (isFinite(facs.punto)?facs.punto:1) * factor;

    // En este método tratamos todo como "capital" y "interés" implícito en el factor.
    // Para mostrar un cronograma amigable, partimos la cuota en:
    // interés aproximado = (factor-1) proporcional; capital = resto.
    const interesUnit = (factor - 1)/factor;
    const capitalUnit = 1/factor;

    const rows = [];
    let saldoRestante = saldo;
    for(let k=1;k<=n;k++){
      const interes = cuota*interesUnit;
      const capital = Math.min(cuota*capitalUnit, saldoRestante);
      saldoRestante = Math.max(0, saldoRestante - capital);
      rows.push({i:k, cuota, interes, capital, saldo: saldoRestante});
    }

    return { cuota, n, saldoInicial: saldo, rows, nota:
      'Método Memorial: cuota = (Saldo / Nº de cuotas) × Punto común × Factor(plazo). El interés se muestra como parte implícita del factor.'
    };
  }

  function calcularAmortizacion(totalNeto, enganche, plazoAnos, tasaAnual, cuotasPorAnio){
    const saldo = Math.max(0, totalNeto - enganche);
    const n = plazoAnos * cuotasPorAnio;
    const i = (tasaAnual/100) / cuotasPorAnio;

    let cuota = 0;
    if(i===0){ cuota = saldo / n; }
    else { cuota = saldo * (i*Math.pow(1+i,n)) / (Math.pow(1+i,n)-1); }

    const rows = [];
    let saldoRestante = saldo;
    for(let k=1;k<=n;k++){
      const interes = saldoRestante * i;
      const capital = Math.min(cuota - interes, saldoRestante);
      saldoRestante = Math.max(0, saldoRestante - capital);
      rows.push({i:k, cuota, interes, capital, saldo: saldoRestante});
    }

    return { cuota, n, saldoInicial: saldo, rows, nota:
      `Amortización estándar con tasa nominal anual ${tasaAnual.toFixed(2)}% y ${cuotasPorAnio} cuotas por año.`
    };
  }

  function kpi(label,value){
    const d = document.createElement('div'); d.className='kpi';
    d.innerHTML = `<div class="label">${label}</div><div class="value">${value}</div>`;
    return d;
  }

  function mostrarResultado(ctx, base){
    const sum = $('#summary');
    sum.innerHTML = '';
    sum.appendChild(kpi('Producto(s)', `${base.prodNombre} × ${base.cantidad}`));
    sum.appendChild(kpi('Precio bruto', fmt.format(base.bruto)));
    sum.appendChild(kpi('Descuento por volumen', `-${(base.desc*100).toFixed(1)}%`));
    sum.appendChild(kpi('Total neto', fmt.format(base.neto)));

    const eng = parseFloat($('#enganche').value||'0');
    sum.appendChild(kpi('Enganche', fmt.format(eng)));
    sum.appendChild(kpi('Saldo a financiar', fmt.format(ctx.saldoInicial)));

    const plazo = parseInt($('#plazo').value,10);
    sum.appendChild(kpi('Plazo', `${plazo} año(s) – ${ctx.n} cuotas`));
    sum.appendChild(kpi('Cuota estimada', fmt.format(ctx.cuota)));

    const tbody = $('#tabla tbody');
    tbody.innerHTML = '';
    ctx.rows.forEach(r=>{
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td>${r.i}</td>
        <td>${fmt.format(r.cuota)}</td>
        <td>${fmt.format(r.interes)}</td>
        <td>${fmt.format(r.capital)}</td>
        <td>${fmt.format(r.saldo)}</td>
      `;
      tbody.appendChild(tr);
    });

    $('#notaModo').textContent = ctx.nota;
  }

  // ===== Eventos =====
  $('#btnCalcular').onclick = ()=>{
    // Lectura de base
    const base = obtenerPrecioBase();
    const enganche = Math.max(0, parseFloat($('#enganche').value||'0'));
    const plazo = parseInt($('#plazo').value||'5',10);

    if(enganche > base.neto){
      alert('El enganche no puede ser mayor que el total neto.'); return;
    }

    let ctx;
    if(modo==='memorial'){
      ctx = calcularMemorial(base.neto, enganche, plazo);
    }else{
      const tasa = parseFloat($('#tasaAnual').value||'0');
      const m = Math.max(1, parseInt($('#cuotasPorAnio').value||'12',10));
      ctx = calcularAmortizacion(base.neto, enganche, plazo, tasa, m);
    }
    mostrarResultado(ctx, base);
  };

  $('#btnLimpiar').onclick = ()=>{
    $('#cantidadProductos').value = 1;
    $('#enganche').value = 0;
    $('#plazo').value = 5;
    $('#f1').value = 1.10; $('#f2').value=1.20; $('#f3').value=1.30; $('#f4').value=1.40; $('#f5').value=1.50; $('#puntoComun').value=1.00;
    $('#tasaAnual').value = 24.00; $('#cuotasPorAnio').value=12;
    $('#tabla tbody').innerHTML = ''; $('#summary').innerHTML = ''; $('#notaModo').textContent='';
  };

  // Compartir por URL (params simples)
  $('#btnCompartir').onclick = ()=>{
    const params = new URLSearchParams({
      p: $('#producto').value,
      q: $('#cantidadProductos').value,
      e: $('#enganche').value,
      z: $('#plazo').value,
      m: (modo==='memorial'?'1':'0'),
      f1: $('#f1').value, f2: $('#f2').value, f3: $('#f3').value, f4: $('#f4').value, f5: $('#f5').value, pc: $('#puntoComun').value,
      t: $('#tasaAnual').value, cpa: $('#cuotasPorAnio').value
    }).toString();
    const share = location.origin+location.pathname+'?'+params;
    navigator.clipboard.writeText(share).then(()=>alert('Enlace copiado al portapapeles.'));
  };

  // Cargar desde URL
  function loadFromURL(){
    const u = new URL(location.href); const g = k=>u.searchParams.get(k);
    if(g('p')) $('#producto').value = g('p');
    if(g('q')) $('#cantidadProductos').value = g('q');
    if(g('e')) $('#enganche').value = g('e');
    if(g('z')) $('#plazo').value = g('z');

    if(g('m')==='0'){ $('#modoAmort').click(); } else { $('#modoMemorial').click(); }

    const ids=['f1','f2','f3','f4','f5','pc','t','cpa'];
    ids.forEach(id=>{
      const v = g(id);
      if(v!==null){
        const map = {pc:'puntoComun', t:'tasaAnual', cpa:'cuotasPorAnio'};
        const target = map[id] ? '#'+map[id] : '#'+id;
        document.querySelector(target).value = v;
      }
    });
  }

  // Init
  initProducto();
  loadFromURL();
</script>
</body>
</html>