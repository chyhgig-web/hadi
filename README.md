<!doctype html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1" />
  <title>فنيي — واجهة موبايل مع تصدير/استيراد</title>
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;600;700;900&display=swap" rel="stylesheet">
  <style>
    :root{
      --bg:#07161a;
      --surface:#0f2628;
      --card:#112b2f;
      --muted:#9fb4b8;
      --accent:#4ee0b0;
      --accent-2:#2fd1a0;
      --glass:rgba(255,255,255,0.03);
      --radius:14px;
      font-family:"Tajawal",system-ui,-apple-system,Segoe UI,Roboto,"Noto Sans Arabic",sans-serif;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;background:linear-gradient(180deg,var(--bg),#041219);color:#eaf6f0}
    body{padding:12px;font-size:16px;-webkit-font-smoothing:antialiased}
    header{display:flex;gap:10px;align-items:center;justify-content:space-between;margin-bottom:10px}
    .brand{display:flex;gap:10px;align-items:center}
    .logo{width:44px;height:44;border-radius:10px;background:linear-gradient(135deg,#0f5a4b,#093f36);display:flex;align-items:center;justify-content:center;font-weight:900}
    h1{font-size:18px;margin:0}
    .sub{font-size:13px;color:var(--muted)}
    .controls{display:flex;gap:8px;align-items:center;width:100%}
    .search-row{display:flex;gap:8px;flex:1}
    input[type="search"], input[type="text"], textarea{
      flex:1;min-width:0;padding:12px 14px;border-radius:12px;background:var(--surface);border:1px solid var(--glass);color:#eaf6f0;font-size:15px;
    }

    main{display:flex;flex-direction:column;gap:10px}
    .list{display:flex;flex-direction:column;gap:10px}
    .card{display:flex;gap:12px;align-items:center;background:linear-gradient(180deg,var(--card),rgba(255,255,255,0.01));padding:12px;border-radius:12px;border:1px solid var(--glass)}
    .thumb{width:52px;height:52;border-radius:10px;background:linear-gradient(60deg,#0d5b48,#0a4738);display:flex;align-items:center;justify-content:center;font-weight:900}
    .meta{flex:1;min-width:0}
    .title{font-weight:800;font-size:16px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
    .tiny{font-size:13px;color:var(--muted);margin-top:6px;display:flex;gap:8px;align-items:center}
    .progress{height:8px;background:rgba(255,255,255,0.02);border-radius:999px;margin-top:8px;overflow:hidden}
    .progress b{display:block;height:100%;background:linear-gradient(90deg,var(--accent),var(--accent-2));width:0%}
    .actions{display:flex;gap:8px}
    .btn{
      background:linear-gradient(180deg,var(--accent),var(--accent-2));
      color:#04221b;border:none;padding:10px 12px;border-radius:10px;font-weight:800;font-size:15px;
      box-shadow:0 6px 18px rgba(46,216,169,0.08);
    }
    .btn.ghost{background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted)}
    .icon-btn{background:transparent;border:1px solid rgba(255,255,255,0.04);color:var(--muted);padding:8px;border-radius:10px}

    /* modal / sheet */
    .overlay{position:fixed;inset:0;background:rgba(0,0,0,0.5);display:none;align-items:flex-end;justify-content:center;padding:0;z-index:9999}
    .modal{width:100%;max-width:100%;background:var(--card);border-radius:16px 16px 0 0;padding:14px;box-shadow:0 -10px 30px rgba(0,0,0,0.6);max-height:92vh;overflow:auto}
    .modal .header{display:flex;justify-content:space-between;align-items:center;gap:8px}
    .modal .body{margin-top:12px;display:flex;flex-direction:column;gap:12px}
    .modal .footer{display:flex;gap:10px;justify-content:space-between;align-items:center;margin-top:8px}
    label{font-weight:700;color:#eaf6f0;font-size:14px;margin-bottom:6px;display:block}
    .tags{display:flex;gap:8px;flex-wrap:wrap;padding:8px;border-radius:10px;background:var(--surface);border:1px solid var(--glass)}
    .tag{background:rgba(255,255,255,0.03);padding:8px 10px;border-radius:999px;display:flex;gap:8px;align-items:center;font-weight:700}
    .tag button{background:transparent;border:none;color:var(--muted);cursor:pointer;padding:0 6px}
    .parts{display:flex;flex-direction:column;gap:8px}
    .part{background:var(--surface);padding:12px;border-radius:10px;border:1px solid var(--glass);display:flex;align-items:center;justify-content:space-between;gap:8px;font-weight:800}

    .muted{color:var(--muted);font-size:13px}

    /* floating add bottom-left */
    .fab{position:fixed;left:14px;bottom:80px;z-index:999;background:linear-gradient(180deg,var(--accent),var(--accent-2));color:#04221b;border:none;padding:14px 16px;border-radius:999px;font-size:22px;font-weight:900;box-shadow:0 10px 30px rgba(0,0,0,0.6)}

    /* bottom center bar for export/import */
    .bottom-bar{
      position:fixed;
      left:50%;
      transform:translateX(-50%);
      bottom:14px;
      display:flex;
      gap:12px;
      align-items:center;
      background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      padding:8px;
      border-radius:999px;
      border:1px solid var(--glass);
      z-index:1000;
      backdrop-filter: blur(6px);
    }
    .bottom-btn{
      display:flex;
      gap:8px;
      align-items:center;
      padding:8px 12px;
      border-radius:999px;
      cursor:pointer;
      background:transparent;
      border:none;
      color:var(--muted);
      font-weight:800;
    }
    .bottom-btn.primary{
      background:linear-gradient(90deg,var(--accent),var(--accent-2));
      color:#04221b;
      box-shadow:0 8px 20px rgba(46,216,169,0.08);
    }
    .icon{
      width:18px;height:18px;display:inline-flex;align-items:center;justify-content:center;font-size:14px;
    }

    /* search item */
    .search-item{display:flex;justify-content:space-between;align-items:center;padding:10px;border-radius:10px;background:var(--card);border:1px solid var(--glass)}
    .search-left{display:flex;flex-direction:column;gap:6px}
    .badge{background:rgba(255,255,255,0.03);padding:4px 8px;border-radius:8px;color:var(--muted);font-weight:700;font-size:13px}

    @media(min-width:700px){
      body{padding:18px}
      .modal{width:640px;border-radius:12px}
      .fab{left:40px;bottom:40px}
      .bottom-bar{bottom:24px}
    }
  </style>
</head>
<body>
  <header>
    <div class="brand">
      <div class="logo">ف</div>
      <div>
        <h1>فنيي</h1>
        <div class="sub">واجهة موبايل — تصدير/استيراد أسفل الوسط</div>
      </div>
    </div>

    <div class="controls" style="flex:1;justify-content:flex-end;">
      <div class="search-row" style="max-width:420px;width:100%;">
        <input id="globalSearch" type="search" placeholder="ابحث (اكتب ما تريد: مثال 'شاشة' أو 'كفر')" aria-label="بحث" />
      </div>
    </div>
  </header>

  <main>
    <section class="list" id="devicesList" aria-live="polite"></section>
    <div id="empty" class="muted" style="text-align:center;display:none;padding:12px;background:var(--card);border-radius:12px">لا توجد أجهزة</div>

    <section style="margin-top:8px">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div class="muted" style="font-weight:800">نتائج البحث</div>
      </div>
      <div id="searchResults" style="margin-top:8px;display:flex;flex-direction:column;gap:8px"></div>
    </section>
  </main>

  <!-- Add modal -->
  <div class="overlay" id="addOverlay" onclick="overlayClick(event,'addOverlay')">
    <div class="modal" role="dialog" aria-modal="true">
      <div class="header">
        <div style="font-weight:800">أضف جهاز</div>
        <div><button id="closeAdd" class="icon-btn">✕</button></div>
      </div>

      <div class="body">
        <div>
          <label for="modalModel">الجهاز</label>
          <input id="modalModel" type="text" placeholder="مثال: 51 Samsung" />
        </div>

        <div>
          <label>أضف قطع (اضغط Enter أو فاصلة)</label>
          <div id="modalTags" class="tags"><div class="muted" style="padding:6px">لا توجد قطع</div></div>
          <input id="modalPartInput" type="text" placeholder="مثال شاشة" />
        </div>

        <div>
          <label>إكسسوارات (كفر، لاصق ...)</label>
          <div id="modalAccessoryTags" class="tags"><div class="muted" style="padding:6px">لا توجد إكسسوارات</div></div>
          <input id="modalAccessoryInput" type="text" placeholder="أضف: كفر أو لاصق ثم Enter" />
        </div>

        <div>
          <label for="modalIcs">ICs (مفصولة بفواصل)</label>
          <input id="modalIcs" type="text" placeholder="مثال: MT6357, PM8941" />
        </div>

        <div class="footer">
          <div style="display:flex;gap:8px">
            <button id="modalSave" class="btn">حفظ</button>
            <button id="modalClear" class="btn ghost">مسح</button>
          </div>
          <div class="muted">يُحفظ محلياً (localStorage)</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Parts modal -->
  <div class="overlay" id="partsOverlay" onclick="overlayClick(event,'partsOverlay')">
    <div class="modal" role="dialog" aria-modal="true">
      <div class="header">
        <div style="display:flex;flex-direction:column;align-items:flex-end">
          <div id="partsTitle" style="font-weight:900"></div>
          <div id="partsSub" class="muted" style="font-size:13px"></div>
        </div>
        <div><button id="closeParts" class="icon-btn">✕</button></div>
      </div>

      <div class="body">
        <div id="partsGrid" class="parts"></div>

        <div style="display:flex;gap:8px">
          <input id="addPartInput" type="text" placeholder="أضف قطعة جديدة..." />
          <button id="addPartBtn" class="btn ghost">أضف</button>
        </div>

        <div style="margin-top:8px">
          <label>الإكسسوارات</label>
          <div id="accessoryList" class="tags"><div class="muted" style="padding:6px">لا توجد إكسسوارات</div></div>
          <div style="display:flex;gap:8px;margin-top:8px">
            <input id="addAccessoryInput" type="text" placeholder="مثال: كفر شفاف" />
            <button id="addAccessoryBtn" class="btn ghost">أضف</button>
          </div>
        </div>

        <div class="footer">
          <div style="display:flex;gap:8px">
            <button id="saveParts" class="btn">حفظ</button>
            <button id="resetParts" class="btn ghost">إعادة متاحة</button>
          </div>
          <div class="muted">محلي</div>
        </div>
      </div>
    </div>
  </div>

  <!-- floating add -->
  <button class="fab" id="fab" aria-label="أضف جهاز">＋</button>

  <!-- bottom center export/import -->
  <div class="bottom-bar" role="region" aria-label="تصدير واستيراد">
    <button id="exportBtn" class="bottom-btn primary" title="تصدير JSON">
      <span class="icon">⬇</span><span>تصدير</span>
    </button>
    <button id="importOpenBtn" class="bottom-btn" title="استيراد JSON">
      <span class="icon">⬆</span><span>استيراد</span>
    </button>
    <button id="clearAllBtn" class="bottom-btn" title="حذف كل البيانات">
      <span class="icon">🗑</span><span>حذف الكل</span>
    </button>
  </div>

  <input id="importFile" type="file" accept="application/json" style="display:none" />

  <script>
    // ثابت وحالة
    const LSKEY = 'repair_mobile_ui_export_import_v2';
    const STATUS_ORDER = ['available','damaged','sold'];
    const STATUS_ICON = { available:'✅', damaged:'❌', sold:'🛒' };
    const STATUS_CLASS = { available:'available', damaged:'damaged', sold:'sold' };
    const DEFAULT_PARTS = ["شاشة","بوردة شحن","بطارية","كاميرا أمامية","كاميرات خلفية","سماعة","جرس","هزاز","فلاتة باور","فلاتة بصمة","غطاء خلفي"];

    // عناصر DOM
    const globalSearch = document.getElementById('globalSearch');
    const devicesList = document.getElementById('devicesList');
    const searchResults = document.getElementById('searchResults');
    const emptyEl = document.getElementById('empty');

    const fab = document.getElementById('fab');
    const addOverlay = document.getElementById('addOverlay');
    const closeAdd = document.getElementById('closeAdd');
    const modalModel = document.getElementById('modalModel');
    const modalPartInput = document.getElementById('modalPartInput');
    const modalTags = document.getElementById('modalTags');
    const modalAccessoryInput = document.getElementById('modalAccessoryInput');
    const modalAccessoryTags = document.getElementById('modalAccessoryTags');
    const modalIcs = document.getElementById('modalIcs');
    const modalSave = document.getElementById('modalSave');
    const modalClear = document.getElementById('modalClear');

    const partsOverlay = document.getElementById('partsOverlay');
    const partsTitle = document.getElementById('partsTitle');
    const partsSub = document.getElementById('partsSub');
    const partsGrid = document.getElementById('partsGrid');
    const closeParts = document.getElementById('closeParts');
    const addPartInput = document.getElementById('addPartInput');
    const addPartBtn = document.getElementById('addPartBtn');
    const saveParts = document.getElementById('saveParts');
    const resetParts = document.getElementById('resetParts');

    const accessoryListEl = document.getElementById('accessoryList');
    const addAccessoryInput = document.getElementById('addAccessoryInput');
    const addAccessoryBtn = document.getElementById('addAccessoryBtn');

    const exportBtn = document.getElementById('exportBtn');
    const importOpenBtn = document.getElementById('importOpenBtn');
    const importFile = document.getElementById('importFile');
    const clearAllBtn = document.getElementById('clearAllBtn');

    // حالة التطبيق
    let devices = loadDevices();
    let currentDeviceId = null;
    let modalTagsList = [];
    let modalAccessoryList = [];

    // تهيئة العرض
    renderModalTags(); renderModalAccessories(); renderDevices(''); renderSearchResults('');

    // البحث
    globalSearch.addEventListener('input', ()=> {
      const q = (globalSearch.value||'').trim().toLowerCase();
      renderDevices(q); renderSearchResults(q);
    });

    // زر + يفتح النموذج
    fab.addEventListener('click', ()=> { openOverlay('addOverlay'); setTimeout(()=> modalModel.focus(), 120); });
    closeAdd.addEventListener('click', ()=> closeOverlay('addOverlay'));
    modalClear.addEventListener('click', ()=> {
      modalModel.value=''; modalIcs.value=''; modalPartInput.value=''; modalAccessoryInput.value='';
      modalTagsList=[]; modalAccessoryList=[]; renderModalTags(); renderModalAccessories(); modalModel.focus();
    });

    // tags للقطع داخل المودال
    modalPartInput.addEventListener('keydown', e => {
      if (e.key === 'Enter' || e.key === ',') { e.preventDefault(); const v = modalPartInput.value.trim().replace(/,$/,''); if (v) addModalTag(v); modalPartInput.value=''; }
    });
    modalPartInput.addEventListener('blur', ()=> { const v = modalPartInput.value.trim(); if (v) { addModalTag(v); modalPartInput.value=''; }});
    function addModalTag(t){ if(!t) return; if (modalTagsList.some(x=>x.toLowerCase()===t.toLowerCase())) return; modalTagsList.push(t); renderModalTags(); }
    function removeModalTag(i){ modalTagsList.splice(i,1); renderModalTags(); }
    function renderModalTags(){ modalTags.innerHTML=''; if (modalTagsList.length===0){ modalTags.innerHTML = '<div class="muted" style="padding:6px">لا توجد قطع</div>'; return; } modalTagsList.forEach((t,i)=>{ const el=document.createElement('div'); el.className='tag'; el.innerHTML=`<div>${t}</div><button aria-label="حذف">✕</button>`; el.querySelector('button').addEventListener('click', ()=> removeModalTag(i)); modalTags.appendChild(el); }); }

    // tags للإكسسوارات داخل المودال
    modalAccessoryInput.addEventListener('keydown', e => {
      if (e.key === 'Enter' || e.key === ',') { e.preventDefault(); const v = modalAccessoryInput.value.trim().replace(/,$/,''); if (v) addModalAccessory(v); modalAccessoryInput.value=''; }
    });
    modalAccessoryInput.addEventListener('blur', ()=> { const v = modalAccessoryInput.value.trim(); if (v) { addModalAccessory(v); modalAccessoryInput.value=''; }});
    function addModalAccessory(t){ if(!t) return; if (modalAccessoryList.some(x=>x.toLowerCase()===t.toLowerCase())) return; modalAccessoryList.push(t); renderModalAccessories(); }
    function removeModalAccessory(i){ modalAccessoryList.splice(i,1); renderModalAccessories(); }
    function renderModalAccessories(){ modalAccessoryTags.innerHTML=''; if (modalAccessoryList.length===0){ modalAccessoryTags.innerHTML = '<div class="muted" style="padding:6px">لا توجد إكسسوارات</div>'; return; } modalAccessoryList.forEach((a,i)=>{ const el=document.createElement('div'); el.className='tag'; el.innerHTML=`<div>${a}</div><button aria-label="حذف">✕</button>`; el.querySelector('button').addEventListener('click', ()=> removeModalAccessory(i)); modalAccessoryTags.appendChild(el); }); }

    // حفظ جهاز من المودال
    modalSave.addEventListener('click', ()=> {
      const model = (modalModel.value||'').trim();
      if (!model) return alert('ادخل اسم الجهاز (مثال: 51 Samsung)');
      const partsList = modalTagsList.length ? modalTagsList.slice() : DEFAULT_PARTS.slice();
      const partsObj = {}; partsList.forEach(p=> partsObj[p] = 'available');
      const accessories = modalAccessoryList.slice();
      const ics = (modalIcs.value||'').trim() ? modalIcs.value.split(',').map(s=>s.trim()).filter(Boolean) : [];
      const dev = { id:'d-'+Date.now().toString(36), model, parts:partsObj, accessories, ics };
      devices.unshift(dev); saveDevices();
      modalModel.value=''; modalIcs.value=''; modalPartInput.value=''; modalAccessoryInput.value=''; modalTagsList=[]; modalAccessoryList=[];
      renderModalTags(); renderModalAccessories(); closeOverlay('addOverlay');
      toast('تم الحفظ محلياً');
      const q = (globalSearch.value||'').trim().toLowerCase(); renderDevices(q); renderSearchResults(q);
    });

    // فتح نافذة القطع
    function openPartsOnly(id){
      currentDeviceId = id;
      const d = devices.find(x=>x.id===id); if (!d) return;
      partsTitle.textContent = d.model || '—';
      partsSub.textContent = computePct(d) + '% · ' + (d.accessories ? d.accessories.length + ' إكسسوارات' : '0 إكسسوار');
      renderParts(d); renderAccessoryList(d);
      openOverlay('partsOverlay'); setTimeout(()=> addPartInput.focus(),120);
    }

    // عرض القطع
    function renderParts(dev){
      partsGrid.innerHTML=''; const keys = Object.keys(dev.parts||{}); if (keys.length===0){ partsGrid.innerHTML = '<div class="muted">لا توجد قطع</div>'; return; }
      keys.forEach(k=>{ const st = dev.parts[k]||'damaged'; const el=document.createElement('div'); el.className='part';
        const left=document.createElement('div'); left.textContent=k;
        const btn=document.createElement('button'); btn.className='status '+STATUS_CLASS[st]; btn.textContent=STATUS_ICON[st];
        btn.addEventListener('click', ()=> { const cur=dev.parts[k]||'damaged'; const ni=STATUS_ORDER[(STATUS_ORDER.indexOf(cur)+1)%STATUS_ORDER.length]; dev.parts[k]=ni; renderParts(dev); });
        const del=document.createElement('button'); del.className='icon-btn'; del.textContent='✕'; del.addEventListener('click', ()=> { if(!confirm('حذف القطعة؟')) return; delete dev.parts[k]; renderParts(dev); });
        const right=document.createElement('div'); right.style.display='flex'; right.style.gap='8px'; right.appendChild(btn); right.appendChild(del);
        el.appendChild(left); el.appendChild(right); partsGrid.appendChild(el);
      });
    }

    // عرض إكسسوارات داخل نافذة القطع
    function renderAccessoryList(dev){
      accessoryListEl.innerHTML=''; const arr = dev.accessories || [];
      if (arr.length===0){ accessoryListEl.innerHTML = '<div class="muted" style="padding:6px">لا توجد إكسسوارات</div>'; return; }
      arr.forEach((a,i)=>{ const el=document.createElement('div'); el.className='tag'; el.innerHTML = `<div>${a}</div><button aria-label="حذف">✕</button>`; el.querySelector('button').addEventListener('click', ()=> { if(!confirm('حذف الإكسسوار؟')) return; dev.accessories.splice(i,1); saveDevices(); renderAccessoryList(dev); partsSub.textContent = computePct(dev) + '% · ' + (dev.accessories ? dev.accessories.length + ' إكسسوارات' : '0 إكسسوار'); }); accessoryListEl.appendChild(el); });
    }

    // إضافة قطعة من نافذة القطع
    addPartBtn.addEventListener('click', ()=> {
      const v = addPartInput.value.trim(); if (!v || !currentDeviceId) return;
      const dev = devices.find(x=>x.id===currentDeviceId); if (!dev) return;
      if (!dev.parts) dev.parts={};
      if (dev.parts[v]){ alert('القطعة موجودة'); addPartInput.value=''; return; }
      dev.parts[v] = 'available'; addPartInput.value=''; renderParts(dev);
    });
    addPartInput.addEventListener('keydown', e => { if (e.key==='Enter'){ e.preventDefault(); addPartBtn.click(); }});

    // إضافة إكسسوار من نافذة القطع
    addAccessoryBtn.addEventListener('click', ()=> {
      const v = addAccessoryInput.value.trim(); if (!v || !currentDeviceId) return;
      const dev = devices.find(x=>x.id===currentDeviceId); if (!dev) return;
      if (!dev.accessories) dev.accessories=[];
      if (dev.accessories.some(x=>x.toLowerCase()===v.toLowerCase())){ alert('مكرر'); addAccessoryInput.value=''; return; }
      dev.accessories.push(v); addAccessoryInput.value=''; renderAccessoryList(dev); partsSub.textContent = computePct(dev) + '% · ' + dev.accessories.length + ' إكسسوارات';
    });
    addAccessoryInput.addEventListener('keydown', e => { if (e.key==='Enter'){ e.preventDefault(); addAccessoryBtn.click(); }});

    // حفظ من نافذة القطع
    saveParts.addEventListener('click', ()=> {
      if (!currentDeviceId) return;
      saveDevices(); const q=(globalSearch.value||'').trim().toLowerCase(); renderDevices(q); renderSearchResults(q); closeOverlay('partsOverlay'); toast('تم الحفظ محلياً');
    });

    resetParts.addEventListener('click', ()=> {
      if (!currentDeviceId) return; if (!confirm('اجعل كل القطع متاحة؟')) return;
      const dev = devices.find(x=>x.id===currentDeviceId); if (!dev || !dev.parts) return; Object.keys(dev.parts).forEach(k=> dev.parts[k]='available'); renderParts(dev);
    });

    closeParts.addEventListener('click', ()=> closeOverlay('partsOverlay'));

    // عرض الأجهزة
    function renderDevices(query=''){
      devicesList.innerHTML=''; const q=(query||'').toLowerCase();
      let list = devices.slice();
      if (q) list = list.filter(d => {
        if ((d.model||'').toLowerCase().includes(q)) return true;
        if (d.parts && Object.keys(d.parts).some(p=>p.toLowerCase().includes(q))) return true;
        if ((d.ics||[]).some(ic=>ic.toLowerCase().includes(q))) return true;
        if ((d.accessories||[]).some(a=>a.toLowerCase().includes(q))) return true;
        return false;
      });
      emptyEl.style.display = list.length ? 'none' : 'block';
      list.forEach(dev => {
        const card = document.createElement('div'); card.className='card';
        const left = document.createElement('div'); left.style.display='flex'; left.style.gap='12px'; left.style.alignItems='center';
        const thumb = document.createElement('div'); thumb.className='thumb'; thumb.textContent = (dev.model||'').split(' ')[0] || dev.model;
        const meta = document.createElement('div'); meta.className='meta';
        const title = document.createElement('div'); title.className='title'; title.textContent = dev.model || '—';
        const tiny = document.createElement('div'); tiny.className='tiny';
        const pct = computePct(dev);
        tiny.innerHTML = `${pct}% · <span class="muted">${(dev.accessories||[]).length} إكسسوارات</span>`;
        const prog = document.createElement('div'); prog.className='progress'; const b=document.createElement('b'); b.style.width = pct + '%'; prog.appendChild(b);
        meta.appendChild(title); meta.appendChild(tiny); meta.appendChild(prog);
        left.appendChild(thumb); left.appendChild(meta);

        const actions = document.createElement('div'); actions.className='actions';
        const openBtn = document.createElement('button'); openBtn.className='btn'; openBtn.textContent='فتح'; openBtn.addEventListener('click', ()=> openPartsOnly(dev.id));
        const delBtn = document.createElement('button'); delBtn.className='icon-btn'; delBtn.textContent='🗑'; delBtn.addEventListener('click', ()=> { if(!confirm('حذف الجهاز؟')) return; devices = devices.filter(x=>x.id!==dev.id); saveDevices(); const q=(globalSearch.value||'').trim().toLowerCase(); renderDevices(q); renderSearchResults(q); });
        actions.appendChild(openBtn); actions.appendChild(delBtn);

        card.appendChild(left); card.appendChild(actions); devicesList.appendChild(card);
      });
    }

    // عرض نتائج البحث (يظهر ما كتبت مع المكان)
    function renderSearchResults(query=''){
      searchResults.innerHTML=''; const q=(query||'').toLowerCase();
      if (!q){ searchResults.innerHTML = '<div class="muted">اكتب كلمة للبحث</div>'; return; }
      const matches = [];
      devices.forEach(dev => {
        if ((dev.model||'').toLowerCase().includes(q)) matches.push({type:'موديل', term: dev.model, device: dev});
        Object.keys(dev.parts || {}).forEach(p => { if (p.toLowerCase().includes(q)) matches.push({type:'قطعة', term: p, device: dev}); });
        (dev.accessories || []).forEach(a => { if (a.toLowerCase().includes(q)) matches.push({type:'إكسسوار', term: a, device: dev}); });
        (dev.ics || []).forEach(ic => { if (ic.toLowerCase().includes(q)) matches.push({type:'IC', term: ic, device: dev}); });
      });
      const seen = new Set(); const uniq = [];
      matches.forEach(m => { const key = `${m.type}||${m.term}||${m.device.id}`; if (!seen.has(key)){ seen.add(key); uniq.push(m); } });
      if (uniq.length === 0){ searchResults.innerHTML = '<div class="muted">لا يوجد نتائج</div>'; return; }
      uniq.forEach(m => {
        const row = document.createElement('div'); row.className='search-item';
        const left = document.createElement('div'); left.className='search-left';
        const termLine = document.createElement('div'); termLine.style.display='flex'; termLine.style.gap='8px'; termLine.style.alignItems='center';
        const badge = document.createElement('div'); badge.className='badge'; badge.textContent = m.type;
        const termText = document.createElement('div'); termText.style.fontWeight='800'; termText.textContent = m.term;
        termLine.appendChild(badge); termLine.appendChild(termText);
        const subLine = document.createElement('div'); subLine.className='muted'; subLine.style.fontSize='13px'; subLine.textContent = m.device.model || '—';
        left.appendChild(termLine); left.appendChild(subLine);
        const openBtn = document.createElement('button'); openBtn.className='btn'; openBtn.textContent='فتح'; openBtn.addEventListener('click', ()=> openPartsOnly(m.device.id));
        row.appendChild(left); row.appendChild(openBtn);
        searchResults.appendChild(row);
      });
    }

    // الحفظ/التحميل المحلي
    function saveDevices(){ try { localStorage.setItem(LSKEY, JSON.stringify(devices)); } catch(e){ console.error(e); alert('فشل الحفظ'); } }
    function loadDevices(){ try { const raw = localStorage.getItem(LSKEY); return raw ? JSON.parse(raw) : sampleIfEmpty(); } catch(e) { console.error(e); return sampleIfEmpty(); } }
    function sampleIfEmpty(){ return [ {id:'d-001',model:'51 Samsung',parts:{"شاشة":"available","بوردة شحن":"available"},accessories:['كفر شفاف'],ics:['MT6357']} ]; }
    function computePct(dev){ const total = Object.keys(dev.parts||{}).length || 1; const avail = Object.values(dev.parts||{}).filter(s=>s==='available').length; return Math.round((avail/total)*100); }

    // export / import (bottom center)
    exportBtn.addEventListener('click', ()=> {
      try {
        const data = JSON.stringify(devices, null, 2);
        const blob = new Blob([data], {type:'application/json'});
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a'); a.href = url; a.download = 'devices-backup.json'; a.click(); URL.revokeObjectURL(url);
        toast('تم تصدير الملف');
      } catch(e){ alert('فشل التصدير'); }
    });
    importOpenBtn.addEventListener('click', ()=> importFile.click());
    importFile.addEventListener('change', (e)=> {
      const f = e.target.files[0]; if (!f) return;
      const r = new FileReader(); r.onload = ()=> { try { const d = JSON.parse(r.result); if (!Array.isArray(d)) throw new Error('الملف يجب أن يحتوي مصفوفة'); devices = d; saveDevices(); const q=(globalSearch.value||'').trim().toLowerCase(); renderDevices(q); renderSearchResults(q); toast('تم الاستيراد'); } catch(err){ alert('فشل الاستيراد: '+err.message); } };
      r.readAsText(f); importFile.value = '';
    });

    // clear all data
    clearAllBtn.addEventListener('click', ()=> {
      if (!confirm('حذف كل البيانات من المتصفح؟')) return;
      devices = []; saveDevices(); renderDevices(''); renderSearchResults(''); toast('تم حذف كل البيانات محلياً');
    });

    // overlays helpers
    function openOverlay(id){ const el = document.getElementById(id); if (!el) return; el.style.display = 'flex'; document.body.style.overflow='hidden'; }
    function closeOverlay(id){ const el = document.getElementById(id); if (!el) return; el.style.display = 'none'; document.body.style.overflow='auto'; if (id==='addOverlay'){ modalModel.value=''; modalIcs.value=''; modalPartInput.value=''; modalAccessoryInput.value=''; modalTagsList=[]; modalAccessoryList=[]; renderModalTags(); renderModalAccessories(); } if (id==='partsOverlay'){ currentDeviceId=null; partsGrid.innerHTML=''; accessoryListEl.innerHTML=''; addPartInput.value=''; addAccessoryInput.value=''; } }
    function overlayClick(e,id){ if (e.target.id === id) closeOverlay(id); }

    // small toast (temporary)
    function toast(msg){
      const t = document.createElement('div'); t.textContent = msg;
      Object.assign(t.style, {position:'fixed',left:'50%',transform:'translateX(-50%)',bottom:'110px',background:'rgba(0,0,0,0.7)',color:'#fff',padding:'10px 14px',borderRadius:'10px',zIndex:2000,opacity:0,transition:'opacity .2s'});
      document.body.appendChild(t);
      requestAnimationFrame(()=> t.style.opacity = 1);
      setTimeout(()=> { t.style.opacity = 0; setTimeout(()=> t.remove(),300); }, 1400);
    }

    // make openPartsOnly accessible
    window.openPartsOnly = openPartsOnly;

    // load initial state
    devices = loadDevices(); renderDevices(''); renderSearchResults('');
  </script>
</body>
</html>
```
