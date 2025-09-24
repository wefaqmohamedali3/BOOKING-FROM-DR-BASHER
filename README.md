[booking.html](https://github.com/user-attachments/files/22522742/booking.html)
<!doctype html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>عيادة الدكتور بشير محمد طاهر – نظام الحجز الإلكتروني</title>
<link rel="icon" href="data:;base64,iVBORw0KGgo=">
<style>
@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600&display=swap');
:root{--primary:#ec4899;--accent:#1e40af;--bg1:#fff5fb;--bg2:#e6f5ff;--card:#ffffff;--muted:#64748b}
*{box-sizing:border-box}
body{margin:0;font-family:'Cairo',Tahoma,Arial,sans-serif;background:linear-gradient(135deg,var(--bg1),var(--bg2));color:#0f172a;-webkit-font-smoothing:antialiased}
.wrap{max-width:1150px;margin:18px auto;padding:14px}
.header{background:rgba(255,255,255,0.95);border-radius:12px;padding:12px 16px;margin-bottom:14px;display:flex;gap:12px;align-items:center;box-shadow:0 6px 22px rgba(2,6,23,0.06)}
.logo{width:74px;height:74px;border-radius:12px;background:linear-gradient(135deg,rgba(236,72,153,0.12),rgba(30,64,175,0.08));display:flex;align-items:center;justify-content:center;font-size:28px;color:var(--primary)}
.hdrTxt h1{margin:0;font-size:20px;color:var(--primary)}.hdrTxt p{margin:6px 0 0;color:var(--muted);font-size:13px}
.grid{display:grid;grid-template-columns:1fr 400px;gap:12px}
.card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 6px 18px rgba(2,6,23,0.04)}
h2{margin:0 0 8px;color:var(--accent);font-size:18px}
label{display:block;margin:8px 0 6px;color:#374151;font-size:14px}
.input,input,select,textarea{width:100%;padding:10px;border-radius:10px;border:1px solid #e6eef8;font-size:14px;font-family:inherit}
textarea{min-height:80px;resize:vertical}
.button{padding:10px 14px;border-radius:10px;border:0;cursor:pointer;font-weight:600}
.btn-primary{background:var(--primary);color:#fff}
.btn-secondary{background:#e2e8f0;color:#0f172a}
.slots{display:flex;flex-wrap:wrap;gap:8px;margin-top:8px}
.slot{padding:8px 10px;border-radius:8px;border:1px solid #e6eef8;cursor:pointer}
.slot.disabled{opacity:0.45;cursor:not-allowed}
.slot.selected{outline:3px solid rgba(59,130,246,0.15);border-color:#60a5fa}
.table{width:100%;border-collapse:collapse;margin-top:8px}
.table th,.table td{padding:10px;border-bottom:1px solid #eef2f7;text-align:center;font-size:14px}
.small{font-size:13px;color:var(--muted)}
.stats{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:8px}
.stat{flex:1;background:linear-gradient(180deg,#fff,#fbfdff);padding:10px;border-radius:10px;text-align:center}
.stat b{display:block;font-size:20px;margin-top:6px;color:var(--accent)}
.footer{margin-top:12px;text-align:center;color:var(--muted);font-size:13px}
.badge{display:inline-block;padding:6px 10px;border-radius:999px;background:#eef2ff;color:#3730a3}
.modal{position:fixed;left:0;top:0;width:100%;height:100%;display:flex;align-items:center;justify-content:center;background:rgba(2,6,23,0.45);visibility:hidden;opacity:0;transition:all .18s}
.modal.show{visibility:visible;opacity:1}.modal .panel{background:#fff;padding:16px;border-radius:12px;min-width:320px;max-width:600px}
@media (max-width:960px){.grid{grid-template-columns:1fr}.logo{width:58px;height:58px}}

/* printable ticket layout for A4 centered */
.printable-ticket{display:none}
@media print{
  body{background:#fff}
  .header,.grid,.footer,.modal,.button{display:none}
  .printable-ticket{display:block;padding:40px;box-sizing:border-box;width:210mm;height:297mm}
  .ticket-inner{max-width:170mm;margin:60mm auto;text-align:center;border:1px dashed #e6eef8;padding:18px;border-radius:8px}
  .ticket-inner h1{font-size:26px;color:var(--primary);margin-bottom:10px}
  .ticket-inner .code{font-size:40px;font-weight:800;color:#102a7a;margin:12px 0}
  .ticket-inner p{font-size:18px;margin:6px 0}
}
</style>
</head>
<body>
<div class="wrap">
  <div class="header">
    <div class="logo">بش</div>
    <div class="hdrTxt"><h1>عيادة الدكتور بشير محمد طاهر</h1><p>نظام الحجز الإلكتروني</p></div>
  </div>

  <div class="grid">
    <div id="mainArea" class="card"></div>

    <div id="sidebar" class="card">
      <h2>الإعدادات & الإحصاءات</h2>
      <label>ساعات العمل (بداية)</label><input id="workStart" type="time" value="09:00" class="input">
      <label>ساعات العمل (نهاية)</label><input id="workEnd" type="time" value="18:00" class="input">
      <label>مدة الشريحة (دقائق)</label><select id="slotMinutes" class="input"><option>10</option><option>15</option><option>20</option></select>
      <label>أقصى حجوزات في اليوم</label><input id="maxPerDay" type="number" value="100" min="1" max="500" class="input">
      <div style="margin-top:10px;display:flex;gap:8px">
        <button id="saveCfg" class="button btn-primary">حفظ الإعداد</button>
        <button id="resetAll" class="button btn-secondary">إعادة ضبط</button>
      </div>

      <div class="stats" style="margin-top:12px">
        <div class="stat"><div class="small">متاح اليوم</div><b id="statAvailable">0</b></div>
        <div class="stat"><div class="small">محجوز اليوم</div><b id="statBooked">0</b></div>
        <div class="stat"><div class="small">منفّذ اليوم</div><b id="statDone">0</b></div>
      </div>

      <div style="margin-top:8px">
        <label>اختر تاريخ للعرض/التصدير</label><input id="viewDate" type="date" value="" class="input">
        <div style="margin-top:8px;display:flex;gap:8px">
          <button id="exportDay" class="button btn-primary">تصدير يوم (XLSX)</button>
          <button id="exportAll" class="button btn-secondary">تصدير كل (XLSX)</button>
        </div>
      </div>

      <hr>
      <div class="small" style="margin-top:8px"><b>Google Sheet</b>: ضع رابط Web App لسكربت Apps Script لحفظ الحجوزات تلقائياً.</div>
      <label style="margin-top:8px">Google Apps Script WebApp URL</label><input id="gsUrl" class="input" placeholder="https://script.google.com/macros/s/....../exec">
      <div style="margin-top:8px;display:flex;gap:8px"><button id="testGs" class="button btn-primary">اختبار الحفظ إلى Google Sheet</button></div>
    </div>
  </div>

  <div class="footer">مخرجات النظام: رقم الحجز، تصدير Excel/Sheet، تقرير يومي/أسبوعي.</div>
</div>

<!-- printable layout (hidden except when printing) -->
<div id="printTicket" class="printable-ticket" aria-hidden="true">
  <div class="ticket-inner" id="ticketInner"></div>
</div>

<!-- Modal edit -->
<div id="modal" class="modal"><div class="panel"><h3 id="modalTitle">تعديل الحجز</h3><div id="modalBody"></div><div style="margin-top:10px;text-align:left"><button id="saveEdit" class="button btn-primary">حفظ</button><button id="closeModal" class="button btn-secondary">إغلاق</button></div></div></div>

<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
<script>
const STORAGE_KEY = 'clinic_booking_final_v1';
function loadState(){ const raw = localStorage.getItem(STORAGE_KEY); if(!raw) return {config:{workStart:'09:00',workEnd:'18:00',slotMinutes:10,maxPerDay:100}, bookings:[]}; try{return JSON.parse(raw);}catch(e){return {config:{workStart:'09:00',workEnd:'18:00',slotMinutes:10,maxPerDay:100}, bookings:[]}} }
function saveState(s){ localStorage.setItem(STORAGE_KEY, JSON.stringify(s)); }
function uid(len=8){ const a=new Uint8Array(len); crypto.getRandomValues(a); return Array.from(a).map(b=>b.toString(36).padStart(2,'0')).join('').slice(0,len); }
function genCode(){ const chars='ABCDEFGHJKLMNPQRSTUVWXYZ23456789'; const arr=new Uint8Array(6); crypto.getRandomValues(arr); let p=''; for(let i=0;i<4;i++) p+=chars[arr[i]%chars.length]; const day=String(new Date().getDate()).padStart(2,'0'); const icons=['★','✿','❤','✧','⚕︎']; const icon=icons[arr[5]%icons.length]; return `${day}-${p}-${icon}`; }
function parseTime(t){ const [hh,mm]=t.split(':').map(Number); return hh*60+mm; }
function fmtTime(m){ const hh=Math.floor(m/60); const mm=m%60; return String(hh).padStart(2,'0')+':'+String(mm).padStart(2,'0'); }
function getSlots(cfg,date){ const start=parseTime(cfg.workStart); const end=parseTime(cfg.workEnd); const slots=[]; for(let m=start; m+cfg.slotMinutes<=end && slots.length<cfg.maxPerDay; m+=cfg.slotMinutes) slots.push(fmtTime(m)); return slots; }
let state = loadState();
if(!state.config) state.config={workStart:'09:00',workEnd:'18:00',slotMinutes:10,maxPerDay:100};

function addBooking({nameParts, phone, date, time, notes}){ const code=genCode(); const b={id:Date.now().toString(36)+'-'+uid(4), code, nameParts, phone, date, time, notes:notes||'', status:'confirmed', createdAt:new Date().toISOString()}; state.bookings.push(b); saveState(state); trySendToGS(b); return b; }
function updateBooking(id,data){ const idx=state.bookings.findIndex(x=>x.id===id); if(idx>=0){ state.bookings[idx] = {...state.bookings[idx], ...data}; saveState(state); return state.bookings[idx]; } return null; }
function cancelBooking(id){ const b=state.bookings.find(x=>x.id===id); if(b){ b.status='cancelled'; saveState(state); } }
function markDone(id){ const b=state.bookings.find(x=>x.id===id); if(b){ b.status='done'; saveState(state); } }
function bookingsForDate(d){ return state.bookings.filter(b=>b.date===d && b.status!=='cancelled'); }
function bookingsForMonth(ym){ return state.bookings.filter(b=>b.date && b.date.slice(0,7)===ym && b.status!=='cancelled'); }
function isSlotAvailable(date,time){ return !state.bookings.some(b=>b.date===date && b.time===time && b.status!=='cancelled'); }
function exportXLSX(rows,filename='bookings.xlsx'){ const ws=XLSX.utils.json_to_sheet(rows); const wb=XLSX.utils.book_new(); XLSX.utils.book_append_sheet(wb,ws,'حجوزات'); XLSX.writeFile(wb,filename); }
function getGSUrl(){ return document.getElementById('gsUrl').value.trim(); }
function trySendToGS(booking){ const url=getGSUrl(); if(!url) return; fetch(url,{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify({code:booking.code,nameParts:booking.nameParts,phone:booking.phone,date:booking.date,time:booking.time,notes:booking.notes,status:booking.status,createdAt:booking.createdAt})}).then(r=>r.json()).then(j=>console.log('GS save',j)).catch(e=>console.warn('GS err',e)); }

const main=document.getElementById('mainArea'); function getRole(){ const p=new URLSearchParams(location.search); return p.get('role')||'admin'; }

function render(){ state=loadState(); document.getElementById('workStart').value=state.config.workStart; document.getElementById('workEnd').value=state.config.workEnd; document.getElementById('slotMinutes').value=state.config.slotMinutes; document.getElementById('maxPerDay').value=state.config.maxPerDay; const today=(new Date()).toISOString().slice(0,10); const slots=getSlots(state.config); document.getElementById('statAvailable').innerText=Math.max(0,slots.length - bookingsForDate(today).length); document.getElementById('statBooked').innerText=bookingsForDate(today).length; document.getElementById('statDone').innerText=state.bookings.filter(b=>b.date===today && b.status==='done').length; const role=getRole(); // hide sidebar for booker
  if(role==='booker'){ document.getElementById('sidebar').style.display='none'; } else { document.getElementById('sidebar').style.display='block'; }
  if(role==='booker'){ renderBooker(); } else { renderAdmin(); } }

function renderBooker(){ const cfg=state.config; const today=(new Date()).toISOString().slice(0,10); const slots=getSlots(cfg); main.innerHTML=`<h2>حجز موعد لمراجعة الطبيب</h2><div style="margin-top:10px" class="small">الحقول المطلوبة: الاسم الثلاثي، رقم الجوال (حتى 10 أرقام)، اختر التاريخ والوقت.</div><div class="card" style="margin-top:12px"><label>الاسم الثلاثي</label><input id="uName" class="input" placeholder="مثال: سارة أحمد محمد"><label>رقم الجوال</label><input id="uPhone" class="input" placeholder="05xxxxxxxx" maxlength="10"><label>ملاحظات إضافية (اختياري)</label><textarea id="uNotes" class="input"></textarea><label>التاريخ</label><input id="uDate" type="date" class="input" value="${today}"><label>الأوقات المتاحة</label><div id="slotsArea" class="slots"></div><div style="margin-top:12px;display:flex;gap:8px"><button id="doBook" class="button btn-primary">حجز الآن</button><button id="clearForm" class="button btn-secondary">مسح</button></div><div id="bookResult" style="margin-top:12px"></div></div>`; const slotsArea=document.getElementById('slotsArea'); function refreshSlots(){ const date=document.getElementById('uDate').value; const all=getSlots(cfg); const html=all.filter(s=>isSlotAvailable(date,s)).map(s=>`<div class="slot" data-time="${s}">${s}</div>`).join(''); slotsArea.innerHTML=html; slotsArea.querySelectorAll('.slot').forEach(el=>el.onclick=()=>{ slotsArea.querySelectorAll('.slot').forEach(x=>x.classList.remove('selected')); el.classList.add('selected'); slotsArea.dataset.selected=el.dataset.time; }); } refreshSlots(); document.getElementById('uDate').onchange=refreshSlots; document.getElementById('clearForm').onclick=()=>{ document.getElementById('uName').value=''; document.getElementById('uPhone').value=''; document.getElementById('uNotes').value=''; document.getElementById('bookResult').innerHTML=''; slotsArea.querySelectorAll('.slot').forEach(x=>x.classList.remove('selected')); }; document.getElementById('doBook').onclick=()=>{ const name=document.getElementById('uName').value.trim(); const phone=document.getElementById('uPhone').value.trim(); const notes=document.getElementById('uNotes').value.trim(); const date=document.getElementById('uDate').value; const time=slotsArea.dataset.selected; if(!name||name.split(/\s+/).length<3){ alert('الاسم الثلاثي مطلوب'); return; } if(!phone||!/^\d{1,10}$/.test(phone.replace(/^0+/,''))){ alert('رقم الجوال مطلوب وبحد أقصى 10 أرقام'); return; } if(!date||!time){ alert('اختر التاريخ والوقت'); return; } if(!isSlotAvailable(date,time)){ alert('هذه الشريحة لم تعد متاحة'); return; } const nameParts=name.split(/\s+/).slice(0,3); const booking=addBooking({nameParts,phone,date,time,notes}); showTicket(booking); render(); }; }

function renderAdmin(){ const cfg=state.config; const today=(new Date()).toISOString().slice(0,10); const rows=state.bookings.slice().sort((a,b)=>{ if(a.date===b.date) return a.time.localeCompare(b.time); return a.date.localeCompare(b.date); }); const tableRows=rows.map(b=>{ const name=(b.nameParts||[]).join(' '); return `<tr><td>${name}</td><td>${b.phone||''}</td><td><span class="badge">${b.code}</span></td><td>${b.date||''} ${b.time||''}</td><td>${b.notes||''}</td><td>${b.status}</td><td><button onclick="openEdit('${b.id}')" class="button btn-primary">تعديل</button> <button onclick="confirmCancel('${b.id}')" class="button btn-secondary">إلغاء</button> <button onclick="confirmDone('${b.id}')" class="button" style="background:#10b981;color:#fff">تم</button> <button onclick="printExisting('${b.id}')" class="button" style="background:#2563eb;color:#fff">طباعة</button></td></tr>` }).join(''); main.innerHTML=`<h2>لوحة الإدارة</h2><div class="small">عرض الحجوزات وإدارتها — التصدير إلى Excel وGoogle Sheet متاح.</div><div style="margin-top:12px" class="card"><div style="display:flex;gap:12px;flex-wrap:wrap;align-items:center"><div class="small"><b>مجموع الحجوزات:</b> ${state.bookings.length}</div><div class="small"><b>حجوزات اليوم:</b> ${bookingsForDate(today).length}</div><div class="small"><b>منفّذ اليوم:</b> ${state.bookings.filter(x=>x.date===today && x.status==='done').length}</div></div><div style="margin-top:10px" class="small">إضافة حجز يدوي:</div><div style="margin-top:8px"><label>الاسم الثلاثي</label><input id="manName" class="input" placeholder="مثال: سارة أحمد محمد"><label>جوال</label><input id="manPhone" class="input" placeholder="05xxxxxxxx"><label>ملاحظات (اختياري)</label><textarea id="manNotes" class="input"></textarea><label>التاريخ</label><input id="manDate" type="date" class="input"><label>الوقت (مثال 09:10)</label><input id="manTime" class="input" placeholder="09:10"><div style="margin-top:8px;display:flex;gap:8px"><button id="addManual" class="button btn-primary">إضافة حجز يدوي</button></div></div></div><div style="margin-top:12px" class="card"><div style="margin-top:8px" class="small">قائمة الحجوزات</div><div style="margin-top:8px;overflow:auto"><table class="table"><thead><tr><th>الاسم</th><th>جوال</th><th>رمز</th><th>التاريخ/الوقت</th><th>ملاحظات</th><th>الحالة</th><th>إجراءات</th></tr></thead><tbody>${tableRows}</tbody></table></div></div>`; document.getElementById('addManual').onclick=()=>{ const name=document.getElementById('manName').value.trim(); const phone=document.getElementById('manPhone').value.trim(); const notes=document.getElementById('manNotes').value.trim(); const date=document.getElementById('manDate').value; const time=document.getElementById('manTime').value.trim(); if(!name||name.split(/\s+/).length<3){ alert('الاسم الثلاثي مطلوب'); return; } if(!phone){ alert('الجوال مطلوب'); return; } if(!date||!time){ alert('التاريخ والوقت مطلوبان'); return; } const nameParts=name.split(/\s+/).slice(0,3); addBooking({nameParts,phone,date,time,notes}); alert('تمت إضافة الحجز'); render(); } }

const modal=document.getElementById('modal'); const modalBody=document.getElementById('modalBody'); const modalTitle=document.getElementById('modalTitle'); function openEdit(id){ const b=state.bookings.find(x=>x.id===id); if(!b) return; modalTitle.innerText='تعديل الحجز'; modalBody.innerHTML=`<label>الاسم الثلاثي</label><input id="eName" class="input" value="${(b.nameParts||[]).join(' ')}"><label>جوال</label><input id="ePhone" class="input" value="${b.phone||''}"><label>ملاحظات</label><textarea id="eNotes" class="input">${b.notes||''}</textarea><label>التاريخ</label><input id="eDate" type="date" class="input" value="${b.date||''}"><label>الوقت</label><input id="eTime" class="input" value="${b.time||''}"><label>الحالة</label><select id="eStatus" class="input"><option value="confirmed">مؤكد</option><option value="cancelled">ملغي</option><option value="done">منفذ</option></select>`; modal.classList.add('show'); document.getElementById('eStatus').value=b.status; document.getElementById('closeModal').onclick=()=>{ modal.classList.remove('show'); }; document.getElementById('saveEdit').onclick=()=>{ const name=document.getElementById('eName').value.trim(); const phone=document.getElementById('ePhone').value.trim(); const notes=document.getElementById('eNotes').value.trim(); const date=document.getElementById('eDate').value; const time=document.getElementById('eTime').value.trim(); const status=document.getElementById('eStatus').value; if(!name||name.split(/\s+/).length<3){ alert('الاسم الثلاثي مطلوب'); return; } const nameParts=name.split(/\s+/).slice(0,3); updateBooking(id,{nameParts,phone,notes,date,time,status}); modal.classList.remove('show'); render(); } }

function confirmCancel(id){ if(confirm('تأكيد إلغاء الحجز؟')){ cancelBooking(id); render(); } }
function confirmDone(id){ if(confirm('تأكيد تمييز الحجز كمنفذ؟')){ markDone(id); render(); } }

document.getElementById('saveCfg').onclick=()=>{ const cfg=state.config||{}; cfg.workStart=document.getElementById('workStart').value||'09:00'; cfg.workEnd=document.getElementById('workEnd').value||'18:00'; cfg.slotMinutes=parseInt(document.getElementById('slotMinutes').value||10,10); cfg.maxPerDay=parseInt(document.getElementById('maxPerDay').value||100,10); state.config=cfg; saveState(state); alert('تم حفظ الإعدادات'); render(); }
document.getElementById('resetAll').onclick=()=>{ if(confirm('هل تريد حذف كل البيانات؟')){ localStorage.removeItem(STORAGE_KEY); state=loadState(); render(); } }
document.getElementById('exportDay').onclick=()=>{ const vd=document.getElementById('viewDate').value|| (new Date()).toISOString().slice(0,10); const rows=state.bookings.filter(b=>b.date===vd).filter(b=>b.status!=='cancelled').map(b=>({رمز:b.code,الاسم:(b.nameParts||[]).join(' '),جوال:b.phone,تاريخ:b.date,وقت:b.time,ملاحظات:b.notes,حالة:b.status,مضاف_في:b.createdAt})); if(rows.length===0){ alert('لا توجد حجوزات لليوم'); return; } exportXLSX(rows, `Bookings_${vd}.xlsx`); }
document.getElementById('exportAll').onclick=()=>{ const rows=state.bookings.filter(b=>b.status!=='cancelled').map(b=>({رمز:b.code,الاسم:(b.nameParts||[]).join(' '),جوال:b.phone,تاريخ:b.date,وقت:b.time,ملاحظات:b.notes,حالة:b.status,مضاف_في:b.createdAt})); if(rows.length===0){ alert('لا توجد بيانات للتصدير'); return; } exportXLSX(rows, `All_Bookings.xlsx`); }

document.getElementById('testGs').onclick=()=>{ const url=getGSUrl(); if(!url){ alert('أدخل رابط Web App الخاص بجوجل'); return; } const sample={code:'TEST-000',nameParts:['تجربة','نموذج','اختبار'],phone:'0500000000',date:(new Date()).toISOString().slice(0,10),time:'09:00',notes:'اختبار حفظ',status:'confirmed',createdAt:new Date().toISOString()}; fetch(url,{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(sample)}).then(r=>r.json()).then(j=>{ alert('تم الإرسال، استجابة الخادم: '+JSON.stringify(j)); }).catch(e=>{ alert('فشل الإرسال: '+e.message); }); }

function showTicket(booking){ // prepare printable ticket content
  const inner=document.getElementById('ticketInner'); inner.innerHTML=`<h1>عيادة الدكتور بشير محمد طاهر</h1><p>تم تأكيد حجزك</p><div class="code">${booking.code}</div><p><b>الاسم:</b> ${(booking.nameParts||[]).join(' ')}</p><p><b>الجوال:</b> ${booking.phone}</p><p><b>التاريخ والوقت:</b> ${booking.date} ${booking.time}</p><p>${booking.notes||''}</p>`; // show printable area and trigger print
  document.getElementById('printTicket').style.display='block'; setTimeout(()=>{ window.print(); setTimeout(()=>{ document.getElementById('printTicket').style.display='none'; },500); },200); }

function printExisting(id){ const b=state.bookings.find(x=>x.id===id); if(!b){ alert('لم يتم العثور على الحجز'); return; } showTicket(b); }

function init(){ if(!state.config) state.config={workStart:'09:00',workEnd:'18:00',slotMinutes:10,maxPerDay:100}; document.getElementById('viewDate').value=(new Date()).toISOString().slice(0,10); render(); window.openEdit=openEdit; window.confirmCancel=confirmCancel; window.confirmDone=confirmDone; window.printExisting=printExisting; }
init();
</script>
</body>
</html>
