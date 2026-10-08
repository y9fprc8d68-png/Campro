<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<meta name="theme-color" content="#111827">
<title>CầmPro - Quản lý cầm đồ</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Arial,sans-serif;background:#f3f4f6;color:#111827}
header{background:#111827;color:white;padding:16px;position:sticky;top:0;z-index:20}
header h1{margin:0;font-size:22px}
header small{opacity:.7}
nav{display:flex;gap:6px;overflow-x:auto;background:#fff;padding:8px;position:sticky;top:65px;z-index:19;border-bottom:1px solid #ddd}
nav button{border:0;background:#f3f4f6;padding:9px 12px;border-radius:9px;white-space:nowrap}
nav button.active{background:#111827;color:white}
main{max-width:1100px;margin:auto;padding:12px}
section{display:none}
section.active{display:block}
.grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px}
.card{background:#fff;border-radius:14px;padding:14px;margin-bottom:12px;box-shadow:0 1px 4px #0001}
.stat b{font-size:21px;display:block;margin-top:5px}
.stat span{color:#6b7280;font-size:13px}
button,.btn{cursor:pointer;border:0;border-radius:9px;padding:10px 13px;font-weight:600}
.primary{background:#111827;color:white}
.green{background:#16a34a;color:white}
.red{background:#dc2626;color:white}
.orange{background:#ea580c;color:white}
.blue{background:#2563eb;color:white}
.gray{background:#e5e7eb}
.actions{display:flex;gap:7px;flex-wrap:wrap;margin-top:10px}
input,select,textarea{width:100%;padding:11px;border:1px solid #d1d5db;border-radius:9px;font-size:15px;background:white}
textarea{min-height:70px}
label{font-size:13px;font-weight:600;display:block;margin:8px 0 5px}
.formgrid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
table{width:100%;border-collapse:collapse;background:#fff}
th,td{padding:9px;border-bottom:1px solid #eee;text-align:left;font-size:13px}
th{background:#f9fafb}
.tablewrap{overflow:auto;border-radius:12px}
.badge{display:inline-block;padding:4px 8px;border-radius:20px;font-size:11px;font-weight:700}
.badge.active{background:#dcfce7;color:#166534}
.badge.overdue{background:#fee2e2;color:#991b1b}
.badge.redeemed{background:#dbeafe;color:#1e40af}
.badge.liquidated{background:#fef3c7;color:#92400e}
.search{margin-bottom:10px}
.empty{text-align:center;color:#777;padding:25px}
.modal{position:fixed;inset:0;background:#0008;display:none;align-items:flex-end;justify-content:center;z-index:50}
.modal.show{display:flex}
.modalbox{background:white;width:100%;max-width:650px;max-height:92vh;overflow:auto;border-radius:18px 18px 0 0;padding:16px}
.modalhead{display:flex;justify-content:space-between;align-items:center}
.modalhead h2{margin:0}
.close{background:#eee;border-radius:50%;width:34px;height:34px}
hr{border:0;border-top:1px solid #eee;margin:15px 0}
.big{font-size:25px;font-weight:800}
.danger{color:#dc2626}
.success{color:#16a34a}
.muted{color:#6b7280}
@media(max-width:700px){
 .grid{grid-template-columns:repeat(2,1fr)}
 .formgrid{grid-template-columns:1fr}
 th,td{white-space:nowrap}
}
</style>
</head>

<body>

<header>
  <h1>💰 CầmPro</h1>
  <small>Quản lý cầm đồ • Lãi tính theo tuần</small>
</header>

<nav>
<button onclick="go('home')">Tổng quan</button>
<button onclick="go('tickets')">Phiếu cầm</button>
<button onclick="go('interest')">Đóng lời</button>
<button onclick="go('redeem')">Chuộc đồ</button>
<button onclick="go('customers')">Khách hàng</button>
<button onclick="go('assets')">Tài sản</button>
<button onclick="go('overdue')">Quá hạn</button>
<button onclick="go('liquid')">Thanh lý</button>
<button onclick="go('cash')">Thu chi</button>
<button onclick="go('report')">Báo cáo</button>
<button onclick="go('settings')">Cài đặt</button>
</nav>

<main>

<section id="home" class="active">
<h2>Tổng quan</h2>
<div class="grid">
<div class="card stat"><span>Vốn đang cầm</span><b id="sCapital">0đ</b></div>
<div class="card stat"><span>Phiếu đang cầm</span><b id="sActive">0</b></div>
<div class="card stat"><span>Lãi đã thu</span><b id="sInterest">0đ</b></div>
<div class="card stat"><span>Quá hạn</span><b id="sOverdue">0</b></div>
</div>
<div class="card">
<h3>Thao tác nhanh</h3>
<div class="actions">
<button class="primary" onclick="openNew()">➕ Nhận cầm</button>
<button class="green" onclick="go('interest')">💰 Đóng lời</button>
<button class="blue" onclick="go('redeem')">💵 Chuộc đồ</button>
<button class="orange" onclick="go('overdue')">⚠️ Quá hạn</button>
</div>
</div>
<div class="card">
<h3>Phiếu gần đây</h3>
<div id="homeRecent"></div>
</div>
</section>

<section id="tickets">
<h2>Phiếu cầm</h2>
<input class="search" id="ticketSearch" oninput="renderTickets()" placeholder="🔎 Tìm mã phiếu, khách, SĐT, tài sản...">
<div id="ticketList"></div>
</section>

<section id="interest">
<h2>💰 Khách đóng lời</h2>
<div class="card">
<p class="muted">Chọn phiếu → nhập số tuần khách đóng → hệ thống chỉ thu phần lãi chưa đóng.</p>
<input class="search" id="interestSearch" oninput="renderInterest()" placeholder="🔎 Tìm khách / mã phiếu">
<div id="interestList"></div>
</div>
</section>

<section id="redeem">
<h2>💵 Chuộc đồ</h2>
<input class="search" id="redeemSearch" oninput="renderRedeem()" placeholder="🔎 Tìm khách / mã phiếu / tài sản">
<div id="redeemList"></div>
</section>

<section id="customers">
<h2>👤 Khách hàng</h2>
<div id="customerList"></div>
</section>

<section id="assets">
<h2>📦 Tài sản đang cầm</h2>
<div id="assetList"></div>
</section>

<section id="overdue">
<h2>⚠️ Phiếu quá hạn</h2>
<div id="overdueList"></div>
</section>

<section id="liquid">
<h2>🔨 Thanh lý</h2>
<div id="liquidList"></div>
</section>

<section id="cash">
<h2>💵 Thu chi</h2>
<div class="actions">
<button class="primary" onclick="openCash()">➕ Thêm thu/chi</button>
</div>
<div class="grid" style="margin-top:10px">
<div class="card stat"><span>Tổng thu</span><b id="cashIn">0đ</b></div>
<div class="card stat"><span>Tổng chi</span><b id="cashOut">0đ</b></div>
<div class="card stat"><span>Dòng tiền</span><b id="cashNet">0đ</b></div>
</div>
<div id="cashList"></div>
</section>

<section id="report">
<h2>📊 Báo cáo</h2>
<div id="reportBox"></div>
</section>

<section id="settings">
<h2>⚙️ Cài đặt</h2>
<div class="card">
<label>Cách tính tuần</label>
<select id="weekMode">
<option value="actual">Theo ngày thực tế (7 ngày = 1 tuần)</option>
<option value="round">Làm tròn tuần lên</option>
</select>

<label>Lãi mặc định / tuần (%)</label>
<input id="defaultRate" type="number" step="0.01">

<label>Phí dịch vụ mặc định</label>
<input id="defaultService" type="number">

<label>Phạt quá hạn / ngày</label>
<input id="defaultLate" type="number">

<div class="actions">
<button class="primary" onclick="saveSettings()">💾 Lưu cài đặt</button>
<button class="blue" onclick="backup()">⬇️ Sao lưu dữ liệu</button>
<button class="gray" onclick="document.getElementById('restoreFile').click()">⬆️ Khôi phục</button>
<input id="restoreFile" type="file" accept=".json" style="display:none" onchange="restoreData(event)">
</div>
</div>
</section>

</main>

<!-- MODAL NHẬN CẦM -->
<div class="modal" id="newModal">
<div class="modalbox">
<div class="modalhead"><h2>➕ Nhận cầm</h2><button class="close" onclick="closeModal('newModal')">×</button></div>

<div class="formgrid">
<div><label>Tên khách *</label><input id="fName"></div>
<div><label>Số điện thoại</label><input id="fPhone" inputmode="tel"></div>
<div><label>CCCD</label><input id="fCCCD"></div>
<div><label>Loại tài sản</label>
<select id="fType">
<option>Xe máy</option>
<option>Ô tô</option>
<option>Điện thoại</option>
<option>Laptop</option>
<option>Máy ảnh</option>
<option>Vàng</option>
<option>Khác</option>
</select></div>
</div>

<label>Mô tả tài sản *</label>
<input id="fAsset">

<div class="formgrid">
<div><label>Giá định giá</label><input id="fValue" type="number"></div>
<div><label>Số tiền cho vay *</label><input id="fLoan" type="number"></div>
<div><label>Lãi / tuần (%)</label><input id="fRate" type="number" step="0.01"></div>
<div><label>Ngày cầm</label><input id="fDate" type="date"></div>
<div><label>Kỳ hạn (ngày)</label><input id="fTerm" type="number"></div>
<div><label>Phí dịch vụ</label><input id="fService" type="number"></div>
<div><label>Phạt quá hạn / ngày</label><input id="fLate" type="number"></div>
</div>

<label>Ghi chú</label>
<textarea id="fNote"></textarea>

<div class="actions">
<button class="primary" onclick="createTicket()">💾 Lưu phiếu cầm</button>
<button class="gray" onclick="closeModal('newModal')">Hủy</button>
</div>
</div>
</div>

<!-- MODAL ĐÓNG LỜI -->
<div class="modal" id="interestModal">
<div class="modalbox">
<div class="modalhead"><h2>💰 Đóng lời</h2><button class="close" onclick="closeModal('interestModal')">×</button></div>
<div id="interestInfo"></div>
<label>Số tuần khách đóng</label>
<input id="iWeeks" type="number" min="1" value="1" oninput="previewInterest()">
<div id="interestPreview" class="card"></div>
<label>Ghi chú</label>
<input id="iNote">
<div class="actions">
<button class="green" onclick="saveInterest()">💰 Xác nhận đã thu</button>
</div>
</div>
</div>

<!-- MODAL CHUỘC -->
<div class="modal" id="redeemModal">
<div class="modalbox">
<div class="modalhead"><h2>💵 Chuộc đồ</h2><button class="close" onclick="closeModal('redeemModal')">×</button></div>
<div id="redeemInfo"></div>
<div class="card" id="redeemCalc"></div>
<div class="actions">
<button class="green" onclick="doRedeem()">✅ Xác nhận chuộc</button>
</div>
</div>
</div>

<!-- MODAL GIA HẠN -->
<div class="modal" id="extendModal">
<div class="modalbox">
<div class="modalhead"><h2>📅 Gia hạn</h2><button class="close" onclick="closeModal('extendModal')">×</button></div>
<div id="extendInfo"></div>
<label>Gia hạn thêm (ngày)</label>
<input id="extendDays" type="number" value="7" min="1">
<div class="actions">
<button class="blue" onclick="doExtend()">📅 Gia hạn</button>
</div>
</div>
</div>

<!-- MODAL THANH LÝ -->
<div class="modal" id="liquidModal">
<div class="modalbox">
<div class="modalhead"><h2>🔨 Thanh lý</h2><button class="close" onclick="closeModal('liquidModal')">×</button></div>
<div id="liquidInfo"></div>
<label>Giá bán thanh lý</label>
<input id="lSale" type="number" oninput="updateLiquid()">
<label>Chi phí thanh lý</label>
<input id="lCost" type="number" value="0" oninput="updateLiquid()">
<div id="liquidPreview" class="card"></div>
<div class="actions">
<button class="orange" onclick="doLiquid()">🔨 Xác nhận thanh lý</button>
</div>
</div>
</div>

<!-- MODAL THU CHI -->
<div class="modal" id="cashModal">
<div class="modalbox">
<div class="modalhead"><h2>💵 Thu / Chi</h2><button class="close" onclick="closeModal('cashModal')">×</button></div>
<label>Loại</label>
<select id="cType"><option value="in">Thu</option><option value="out">Chi</option></select>
<label>Số tiền</label>
<input id="cAmount" type="number">
<label>Nội dung</label>
<input id="cNote">
<div class="actions">
<button class="primary" onclick="createCash()">💾 Lưu</button>
</div>
</div>
</div>

<script>
const KEY="CAMPRO_WEEKLY_V2";

let db=JSON.parse(localStorage.getItem(KEY)||"null")||{
 tickets:[],
 cash:[],
 settings:{
  weekMode:"actual",
  defaultRate:2,
  defaultService:0,
  defaultLate:0
 }
};

function $(id){return document.getElementById(id)}
function save(){localStorage.setItem(KEY,JSON.stringify(db))}
function money(n){return Number(n||0).toLocaleString("vi-VN")+"đ"}
function today(){return new Date().toISOString().slice(0,10)}
function esc(s){
 return String(s??"").replace(/[&<>"']/g,m=>({
 "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"
 }[m]));
}
function parseDate(s){
 let d=new Date(s+"T00:00:00");
 return isNaN(d)?new Date():d;
}
function fmtDate(s){
 if(!s)return "";
 let d=parseDate(s);
 return d.toLocaleDateString("vi-VN");
}
function addDays(date,n){
 let d=parseDate(date);
 d.setDate(d.getDate()+Number(n));
 return d.toISOString().slice(0,10);
}
function daysBetween(a,b){
 let x=parseDate(a),y=parseDate(b);
 return Math.max(0,Math.floor((y-x)/86400000));
}
function weeksFor(days){
 if(days<=0)return 0;
 return db.settings.weekMode==="round"
  ? Math.ceil(days/7)
  : days/7;
}
function nextCode(){
 let max=0;
 db.tickets.forEach(t=>{
  let m=String(t.code||"").match(/(\d+)$/);
  if(m)max=Math.max(max,Number(m[1]));
 });
 return "CD"+String(max+1).padStart(5,"0");
}

/* =========================
   TÍNH TIỀN
========================= */

function calcTicket(t,endDate=today()){
 let days=daysBetween(t.pawnDate,endDate);
 let weeks=weeksFor(days);

 let totalInterest=Number(t.loan)*Number(t.rate)/100*weeks;

 let paidInterest=(t.interestPayments||[])
  .reduce((s,p)=>s+Number(p.amount||0),0);

 let unpaidInterest=Math.max(0,totalInterest-paidInterest);

 let lateDays=0;
 if(endDate>t.dueDate){
  lateDays=daysBetween(t.dueDate,endDate);
 }

 let lateFee=lateDays*Number(t.lateFee||0);

 let service=Number(t.serviceFee||0);

 return {
  days,
  weeks,
  totalInterest,
  paidInterest,
  unpaidInterest,
  lateDays,
  lateFee,
  service,
  redeemTotal:Number(t.loan)+unpaidInterest+service+lateFee
 };
}

function status(t){
 if(t.status==="redeemed")return "redeemed";
 if(t.status==="liquidated")return "liquidated";
 if(today()>t.dueDate)return "overdue";
 return "active";
}

function badge(s){
 let name={
  active:"Đang cầm",
  overdue:"Quá hạn",
  redeemed:"Đã chuộc",
  liquidated:"Đã thanh lý"
 }[s]||s;
 return `<span class="badge ${s}">${name}</span>`;
}

/* =========================
   ĐIỀU HƯỚNG
========================= */

function go(id){
 document.querySelectorAll("section").forEach(x=>x.classList.remove("active"));
 $(id).classList.add("active");

 document.querySelectorAll("nav button").forEach(b=>{
  b.classList.toggle("active",b.getAttribute("onclick")?.includes("'"+id+"'"));
 });

 renderAll();
 window.scrollTo(0,0);
}

/* =========================
   MODAL
========================= */

function openModal(id){$(id).classList.add("show")}
function closeModal(id){$(id).classList.remove("show")}

document.querySelectorAll(".modal").forEach(m=>{
 m.addEventListener("click",e=>{
  if(e.target===m)m.classList.remove("show");
 });
});

/* =========================
   NHẬN CẦM
========================= */

function openNew(){
 $("fDate").value=today();
 $("fRate").value=db.settings.defaultRate;
 $("fService").value=db.settings.defaultService;
 $("fLate").value=db.settings.defaultLate;
 $("fTerm").value=7;
 $("fName").value="";
 $("fPhone").value="";
 $("fCCCD").value="";
 $("fAsset").value="";
 $("fValue").value="";
 $("fLoan").value="";
 $("fNote").value="";
 openModal("newModal");
}

function createTicket(){
 let name=$("fName").value.trim();
 let asset=$("fAsset").value.trim();
 let loan=Number($("fLoan").value);

 if(!name||!asset||loan<=0){
  alert("Nhập tên khách, tài sản và số tiền cho vay.");
  return;
 }

 let pawnDate=$("fDate").value||today();
 let term=Number($("fTerm").value)||7;

 let t={
  id:crypto.randomUUID?crypto.randomUUID():Date.now()+"",
  code:nextCode(),
  customer:{
   name,
   phone:$("fPhone").value.trim(),
   cccd:$("fCCCD").value.trim()
  },
  asset:{
   type:$("fType").value,
   desc:asset,
   value:Number($("fValue").value||0)
  },
  loan,
  rate:Number($("fRate").value||0),
  pawnDate,
  dueDate:addDays(pawnDate,term),
  termDays:term,
  serviceFee:Number($("fService").value||0),
  lateFee:Number($("fLate").value||0),
  note:$("fNote").value.trim(),
  status:"active",
  interestPayments:[],
  extensions:[],
  createdAt:new Date().toISOString()
 };

 db.tickets.unshift(t);

 db.cash.unshift({
  id:Date.now()+"",
  date:today(),
  type:"out",
  amount:loan,
  note:"Giải ngân cầm đồ "+t.code,
  ticketId:t.id
 });

 save();
 closeModal("newModal");
 alert("Đã tạo phiếu "+t.code);
 renderAll();
}

/* =========================
   PHIẾU CẦM
========================= */

function ticketCard(t){
 let s=status(t);
 let c=calcTicket(t);

 return `
 <div class="card">
  <div style="display:flex;justify-content:space-between;gap:10px">
   <div>
    <b>${esc(t.code)}</b><br>
    <b>${esc(t.customer.name)}</b>
   </div>
   <div>${badge(s)}</div>
  </div>

  <hr>

  <div>
   📱 ${esc(t.customer.phone||"-")}<br>
   🪪 CCCD: ${esc(t.customer.cccd||"-")}<br>
   📦 ${esc(t.asset.type)} - ${esc(t.asset.desc)}<br>
   💰 Vốn: <b>${money(t.loan)}</b><br>
   📈 Lãi: <b>${t.rate}% / tuần</b><br>
   📅 ${fmtDate(t.pawnDate)} → ${fmtDate(t.dueDate)}<br>
   💵 Đã đóng lời: <b class="success">${money(c.paidInterest)}</b><br>
   🔥 Lãi phát sinh hiện tại: <b>${money(c.totalInterest)}</b>
  </div>

  <div class="actions">
   ${s==="active"||s==="overdue"?`
   <button class="green" onclick="openInterest('${t.id}')">💰 Đóng lời</button>
   <button class="blue" onclick="openRedeem('${t.id}')">💵 Chuộc</button>
   <button class="gray" onclick="openExtend('${t.id}')">📅 Gia hạn</button>
   `:""}

   ${s==="overdue"?`
   <button class="orange" onclick="openLiquid('${t.id}')">🔨 Thanh lý</button>
   `:""}

   <button class="gray" onclick="printTicket('${t.id}')">🖨️ In</button>
  </div>
 </div>`;
}

function renderTickets(){
 let q=($("ticketSearch")?.value||"").toLowerCase();

 let arr=db.tickets.filter(t=>{
  let s=JSON.stringify(t).toLowerCase();
  return s.includes(q);
 });

 $("ticketList").innerHTML=arr.length
 ?arr.map(ticketCard).join("")
 :`<div class="empty">Chưa có phiếu.</div>`;
}

/* =========================
   ĐÓNG LỜI
========================= */

let interestId=null;

function openInterest(id){
 interestId=id;
 let t=db.tickets.find(x=>x.id===id);
 if(!t)return;

 let c=calcTicket(t);

 $("interestInfo").innerHTML=`
 <div class="card">
 <b>${esc(t.code)} - ${esc(t.customer.name)}</b><br>
 Vốn: <b>${money(t.loan)}</b><br>
 Lãi: <b>${t.rate}% / tuần</b><br>
 Đã đóng lời: <b class="success">${money(c.paidInterest)}</b><br>
 Lãi phát sinh tới hôm nay: <b>${money(c.totalInterest)}</b><br>
 Còn lãi chưa đóng: <b>${money(c.unpaidInterest)}</b>
 </div>`;

 $("iWeeks").value=1;
 $("iNote").value="";
 previewInterest();
 openModal("interestModal");
}

function previewInterest(){
 let t=db.tickets.find(x=>x.id===interestId);
 if(!t)return;

 let weeks=Number($("iWeeks").value||0);
 let amount=Number(t.loan)*Number(t.rate)/100*weeks;

 $("interestPreview").innerHTML=`
 <b>Khách đóng ${weeks} tuần</b><br>
 Tiền lãi: <span class="big">${money(amount)}</span>
 `;
}

function saveInterest(){
 let t=db.tickets.find(x=>x.id===interestId);
 if(!t)return;

 let weeks=Number($("iWeeks").value||0);
 if(weeks<=0){
  alert("Nhập số tuần.");
  return;
 }

 let amount=Number(t.loan)*Number(t.rate)/100*weeks;

 t.interestPayments=t.interestPayments||[];

 t.interestPayments.push({
  id:Date.now()+"",
  date:today(),
  weeks,
  amount,
  note:$("iNote").value.trim()
 });

 db.cash.unshift({
  id:Date.now()+"",
  date:today(),
  type:"in",
  amount,
  note:"Khách đóng lời "+t.code+" - "+weeks+" tuần",
  ticketId:t.id
 });

 save();
 closeModal("interestModal");

 alert("Đã thu "+money(amount)+" tiền lời.");
 renderAll();
}

function renderInterest(){
 let q=($("interestSearch")?.value||"").toLowerCase();

 let arr=db.tickets.filter(t=>{
  let s=status(t);
  if(s!=="active"&&s!=="overdue")return false;

  return JSON.stringify(t).toLowerCase().includes(q);
 });

 $("interestList").innerHTML=arr.length
 ?arr.map(t=>{
   let c=calcTicket(t);

   return `
   <div class="card">
    <b>${esc(t.code)} - ${esc(t.customer.name)}</b>
    ${badge(status(t))}
    <hr>
    Vốn: <b>${money(t.loan)}</b><br>
    Lãi: <b>${t.rate}%/tuần</b><br>
    Lãi phát sinh: <b>${money(c.totalInterest)}</b><br>
    Đã đóng: <b class="success">${money(c.paidInterest)}</b><br>
    Chưa đóng: <b class="danger">${money(c.unpaidInterest)}</b>

    <div class="actions">
     <button class="green" onclick="openInterest('${t.id}')">💰 Đóng lời</button>
    </div>

    ${(t.interestPayments||[]).length?`
    <details>
     <summary>Lịch sử đóng lời</summary>
     ${t.interestPayments.map(p=>`
      <p>📅 ${fmtDate(p.date)} — ${p.weeks} tuần — <b>${money(p.amount)}</b></p>
     `).join("")}
    </details>`:""}
   </div>`;
 }).join("")
 :`<div class="empty">Không có phiếu.</div>`;
}

/* =========================
   CHUỘC
========================= */

let redeemId=null;

function openRedeem(id){
 redeemId=id;

 let t=db.tickets.find(x=>x.id===id);
 if(!t)return;

 let c=calcTicket(t);

 $("redeemInfo").innerHTML=`
 <div class="card">
 <b>${esc(t.code)} - ${esc(t.customer.name)}</b><br>
 Tài sản: ${esc(t.asset.desc)}<br>
 Vốn: <b>${money(t.loan)}</b><br>
 Lãi: ${t.rate}%/tuần<br>
 Ngày cầm: ${fmtDate(t.pawnDate)}<br>
 Hôm nay: ${fmtDate(today())}
 </div>`;

 $("redeemCalc").innerHTML=redeemHTML(t);

 openModal("redeemModal");
}

function redeemHTML(t){
 let c=calcTicket(t);

 return `
 📅 Số ngày: <b>${c.days}</b><br>
 📆 Số tuần tính lãi: <b>${c.weeks.toFixed(2)}</b><br>
 💰 Tổng lãi phát sinh: <b>${money(c.totalInterest)}</b><br>
 ✅ Lãi đã đóng: <b class="success">${money(c.paidInterest)}</b><br>
 🔥 Lãi còn phải thu: <b>${money(c.unpaidInterest)}</b><br>
 🧾 Phí dịch vụ: <b>${money(c.service)}</b><br>
 ⚠️ Phạt quá hạn: <b>${money(c.lateFee)}</b>
 <hr>
 <span class="big">💵 Tổng chuộc: ${money(c.redeemTotal)}</span>
 `;
}

function doRedeem(){
 let t=db.tickets.find(x=>x.id===redeemId);
 if(!t)return;

 let c=calcTicket(t);

 if(!confirm("Xác nhận khách chuộc "+t.code+" với số tiền "+money(c.redeemTotal)+"?"))return;

 t.status="redeemed";

 t.redeemed={
  date:today(),
  days:c.days,
  weeks:c.weeks,
  totalInterest:c.totalInterest,
  paidInterest:c.paidInterest,
  remainingInterest:c.unpaidInterest,
  service:c.service,
  lateFee:c.lateFee,
  total:c.redeemTotal
 };

 db.cash.unshift({
  id:Date.now()+"",
  date:today(),
  type:"in",
  amount:c.redeemTotal,
  note:"Khách chuộc "+t.code,
  ticketId:t.id
 });

 save();
 closeModal("redeemModal");

 alert("Đã chuộc phiếu "+t.code);
 renderAll();
}

/* =========================
   GIA HẠN
========================= */

let extendId=null;

function openExtend(id){
 extendId=id;

 let t=db.tickets.find(x=>x.id===id);
 if(!t)return;

 $("extendInfo").innerHTML=`
 <div class="card">
 <b>${t.code} - ${esc(t.customer.name)}</b><br>
 Hạn hiện tại: <b>${fmtDate(t.dueDate)}</b>
 </div>`;

 $("extendDays").value=7;
 openModal("extendModal");
}

function doExtend(){
 let t=db.tickets.find(x=>x.id===extendId);
 if(!t)return;

 let days=Number($("extendDays").value||0);
 if(days<=0)return;

 let old=t.dueDate;
 t.dueDate=addDays(t.dueDate,days);
 t.termDays+=days;

 t.extensions=t.extensions||[];
 t.extensions.push({
  date:today(),
  days,
  oldDue:old,
  newDue:t.dueDate
 });

 save();
 closeModal("extendModal");

 alert("Đã gia hạn đến "+fmtDate(t.dueDate));
 renderAll();
}

/* =========================
   QUÁ HẠN
========================= */

function renderOverdue(){
 let arr=db.tickets.filter(t=>status(t)==="overdue");

 $("overdueList").innerHTML=arr.length
 ?arr.map(t=>{
   let c=calcTicket(t);

   return `
   <div class="card">
    ${badge("overdue")}
    <h3>${esc(t.code)} - ${esc(t.customer.name)}</h3>
    📦 ${esc(t.asset.desc)}<br>
    💰 Vốn: <b>${money(t.loan)}</b><br>
    📅 Hạn: <b>${fmtDate(t.dueDate)}</b><br>
    ⚠️ Quá: <b class="danger">${c.lateDays} ngày</b><br>
    💵 Lãi chưa đóng: <b>${money(c.unpaidInterest)}</b><br>
    Phạt: <b>${money(c.lateFee)}</b>
    <div class="actions">
     <button class="green" onclick="openInterest('${t.id}')">💰 Đóng lời</button>
     <button class="blue" onclick="openRedeem('${t.id}')">💵 Chuộc</button>
     <button class="orange" onclick="openLiquid('${t.id}')">🔨 Thanh lý</button>
    </div>
   </div>`;
 }).join("")
 :`<div class="empty">🎉 Không có phiếu quá hạn.</div>`;
}

/* =========================
   THANH LÝ
========================= */

let liquidId=null;

function openLiquid(id){
 liquidId=id;

 let t=db.tickets.find(x=>x.id===id);
 if(!t)return;

 $("liquidInfo").innerHTML=`
 <div class="card">
 <b>${t.code} - ${esc(t.customer.name)}</b><br>
 Tài sản: ${esc(t.asset.desc)}<br>
 Vốn: <b>${money(t.loan)}</b>
 </div>`;

 $("lSale").value="";
 $("lCost").value=0;
 updateLiquid();
 openModal("liquidModal");
}

function updateLiquid(){
 let t=db.tickets.find(x=>x.id===liquidId);
 if(!t)return;

 let sale=Number($("lSale").value||0);
 let cost=Number($("lCost").value||0);
 let result=sale-Number(t.loan)-cost;

 $("liquidPreview").innerHTML=`
 Giá bán: <b>${money(sale)}</b><br>
 Vốn: <b>${money(t.loan)}</b><br>
 Chi phí: <b>${money(cost)}</b><hr>
 Kết quả:
 <span class="${result>=0?"success":"danger"} big">${money(result)}</span>
 `;
}

function doLiquid(){
 let t=db.tickets.find(x=>x.id===liquidId);
 if(!t)return;

 let sale=Number($("lSale").value||0);
 let cost=Number($("lCost").value||0);

 if(sale<=0){
  alert("Nhập giá bán.");
  return;
 }

 let profit=sale-t.loan-cost;

 t.status="liquidated";

 t.liquidation={
  date:today(),
  sale,
  cost,
  profit
 };

 db.cash.unshift({
  id:Date.now()+"",
  date:today(),
  type:"in",
  amount:sale,
  note:"Bán thanh lý "+t.code,
  ticketId:t.id
 });

 if(cost>0){
  db.cash.unshift({
   id:Date.now()+"2",
   date:today(),
   type:"out",
   amount:cost,
   note:"Chi phí thanh lý "+t.code,
   ticketId:t.id
  });
 }

 save();
 closeModal("liquidModal");

 alert("Đã thanh lý. Kết quả: "+money(profit));
 renderAll();
}

function renderLiquid(){
 let arr=db.tickets.filter(t=>status(t)==="overdue");

 $("liquidList").innerHTML=arr.length
 ?arr.map(t=>`
 <div class="card">
 <b>${t.code} - ${esc(t.customer.name)}</b><br>
 Tài sản: ${esc(t.asset.desc)}<br>
 Vốn: ${money(t.loan)}
 <div class="actions">
 <button class="orange" onclick="openLiquid('${t.id}')">🔨 Thanh lý</button>
 </div>
 </div>`).join("")
 :`<div class="empty">Không có phiếu chờ thanh lý.</div>`;
}

/* =========================
   KHÁCH HÀNG
========================= */

function renderCustomers(){
 let map={};

 db.tickets.forEach(t=>{
  let key=t.customer.phone||t.customer.cccd||t.customer.name;
  if(!map[key])map[key]={
   customer:t.customer,
   count:0,
   loan:0,
   interest:0
  };

  map[key].count++;

  if(t.status==="active"||t.status==="overdue")
   map[key].loan+=Number(t.loan);

  map[key].interest+=(t.interestPayments||[])
   .reduce((s,p)=>s+Number(p.amount||0),0);
 });

 let arr=Object.values(map);

 $("customerList").innerHTML=arr.length
 ?arr.map(x=>`
 <div class="card">
  <b>${esc(x.customer.name)}</b><br>
  📱 ${esc(x.customer.phone||"-")}<br>
  🪪 ${esc(x.customer.cccd||"-")}<br>
  Số phiếu: <b>${x.count}</b><br>
  Vốn đang cầm: <b>${money(x.loan)}</b><br>
  Lãi đã đóng: <b class="success">${money(x.interest)}</b>
 </div>`).join("")
 :`<div class="empty">Chưa có khách.</div>`;
}

/* =========================
   TÀI SẢN
========================= */

function renderAssets(){
 let arr=db.tickets.filter(t=>status(t)==="active"||status(t)==="overdue");

 $("assetList").innerHTML=arr.length
 ?arr.map(t=>`
 <div class="card">
  <b>${esc(t.asset.type)} - ${esc(t.asset.desc)}</b><br>
  Chủ: ${esc(t.customer.name)}<br>
  Mã phiếu: <b>${t.code}</b><br>
  Định giá: ${money(t.asset.value)}<br>
  Đang cầm: <b>${money(t.loan)}</b><br>
  Hạn: ${fmtDate(t.dueDate)} ${badge(status(t))}
 </div>`).join("")
 :`<div class="empty">Không có tài sản đang cầm.</div>`;
}

/* =========================
   THU CHI
========================= */

function openCash(){
 $("cAmount").value="";
 $("cNote").value="";
 openModal("cashModal");
}

function createCash(){
 let amount=Number($("cAmount").value||0);
 let note=$("cNote").value.trim();

 if(amount<=0)return;

 db.cash.unshift({
  id:Date.now()+"",
  date:today(),
  type:$("cType").value,
  amount,
  note:note||"Thu chi khác"
 });

 save();
 closeModal("cashModal");
 renderAll();
}

function renderCash(){
 let ins=db.cash
  .filter(x=>x.type==="in")
  .reduce((s,x)=>s+Number(x.amount),0);

 let outs=db.cash
  .filter(x=>x.type==="out")
  .reduce((s,x)=>s+Number(x.amount),0);

 $("cashIn").textContent=money(ins);
 $("cashOut").textContent=money(outs);
 $("cashNet").textContent=money(ins-outs);

 $("cashList").innerHTML=db.cash.length
 ?db.cash.map(x=>`
 <div class="card">
  <b class="${x.type==="in"?"success":"danger"}">
   ${x.type==="in"?"THU":"CHI"} ${money(x.amount)}
  </b><br>
  📅 ${fmtDate(x.date)}<br>
  ${esc(x.note)}
 </div>`).join("")
 :`<div class="empty">Chưa có giao dịch.</div>`;
}

/* =========================
   BÁO CÁO
========================= */

function renderReports(){
 let active=db.tickets.filter(t=>status(t)==="active"||status(t)==="overdue");

 let capital=active.reduce((s,t)=>s+Number(t.loan),0);

 let interestPaid=db.tickets.reduce((s,t)=>
  s+(t.interestPayments||[]).reduce((a,p)=>a+Number(p.amount||0),0),0);

 let redeemRevenue=db.tickets
  .filter(t=>t.status==="redeemed")
  .reduce((s,t)=>s+Number(t.redeemed?.total||0),0);

 let liquidationProfit=db.tickets
  .filter(t=>t.status==="liquidated")
  .reduce((s,t)=>s+Number(t.liquidation?.profit||0),0);

 let totalIn=db.cash.filter(x=>x.type==="in")
  .reduce((s,x)=>s+Number(x.amount),0);

 let totalOut=db.cash.filter(x=>x.type==="out")
  .reduce((s,x)=>s+Number(x.amount),0);

 $("reportBox").innerHTML=`
 <div class="grid">
  <div class="card stat"><span>Vốn đang cầm</span><b>${money(capital)}</b></div>
  <div class="card stat"><span>Lãi đã thu</span><b>${money(interestPaid)}</b></div>
  <div class="card stat"><span>Tiền chuộc</span><b>${money(redeemRevenue)}</b></div>
  <div class="card stat"><span>Lãi thanh lý</span><b>${money(liquidationProfit)}</b></div>
 </div>

 <div class="card">
  <h3>📊 Tổng quan dòng tiền</h3>
  Tổng thu: <b class="success">${money(totalIn)}</b><br>
  Tổng chi: <b class="danger">${money(totalOut)}</b><br>
  Dòng tiền ròng: <b>${money(totalIn-totalOut)}</b>
 </div>

 <div class="card">
  <h3>📋 Thống kê phiếu</h3>
  Đang cầm: <b>${db.tickets.filter(t=>status(t)==="active").length}</b><br>
  Quá hạn: <b>${db.tickets.filter(t=>status(t)==="overdue").length}</b><br>
  Đã chuộc: <b>${db.tickets.filter(t=>t.status==="redeemed").length}</b><br>
  Đã thanh lý: <b>${db.tickets.filter(t=>t.status==="liquidated").length}</b>
 </div>
 `;
}

/* =========================
   TỔNG QUAN
========================= */

function renderHome(){
 let active=db.tickets.filter(t=>status(t)==="active"||status(t)==="overdue");

 $("sCapital").textContent=money(
  active.reduce((s,t)=>s+Number(t.loan),0)
 );

 $("sActive").textContent=active.length;

 $("sInterest").textContent=money(
  db.tickets.reduce((s,t)=>
   s+(t.interestPayments||[])
    .reduce((a,p)=>a+Number(p.amount||0),0),0)
 );

 $("sOverdue").textContent=db.tickets
  .filter(t=>status(t)==="overdue").length;

 let recent=db.tickets.slice(0,5);

 $("homeRecent").innerHTML=recent.length
 ?recent.map(t=>`
  <div style="padding:9px 0;border-bottom:1px solid #eee">
   <b>${t.code}</b> - ${esc(t.customer.name)}
   ${badge(status(t))}
   <br>${money(t.loan)}
  </div>`).join("")
 :`<div class="empty">Chưa có dữ liệu.</div>`;
}

/* =========================
   CÀI ĐẶT
========================= */

function loadSettings(){
 $("weekMode").value=db.settings.weekMode;
 $("defaultRate").value=db.settings.defaultRate;
 $("defaultService").value=db.settings.defaultService;
 $("defaultLate").value=db.settings.defaultLate;
}

function saveSettings(){
 db.settings.weekMode=$("weekMode").value;
 db.settings.defaultRate=Number($("defaultRate").value||0);
 db.settings.defaultService=Number($("defaultService").value||0);
 db.settings.defaultLate=Number($("defaultLate").value||0);

 save();
 alert("Đã lưu cài đặt.");
 renderAll();
}

/* =========================
   SAO LƯU / KHÔI PHỤC
========================= */

function backup(){
 let blob=new Blob(
  [JSON.stringify(db,null,2)],
  {type:"application/json"}
 );

 let a=document.createElement("a");
 a.href=URL.createObjectURL(blob);
 a.download="CamPro_Backup_"+today()+".json";
 a.click();
 URL.revokeObjectURL(a.href);
}

function restoreData(e){
 let file=e.target.files[0];
 if(!file)return;

 let reader=new FileReader();

 reader.onload=function(){
  try{
   let x=JSON.parse(reader.result);

   if(!x.tickets||!x.cash||!x.settings)
    throw new Error();

   if(confirm("Khôi phục dữ liệu sẽ ghi đè dữ liệu hiện tại. Tiếp tục?")){
    db=x;
    save();
    alert("Khôi phục thành công.");
    renderAll();
    loadSettings();
   }
  }catch(err){
   alert("File backup không hợp lệ.");
  }
 };

 reader.readAsText(file);
}

/* =========================
   IN PHIẾU
========================= */

function printTicket(id){
 let t=db.tickets.find(x=>x.id===id);
 if(!t)return;

 let c=calcTicket(t);

 let w=window.open("","_blank");

 w.document.write(`
 <!doctype html>
 <html>
 <head>
 <title>${t.code}</title>
 <style>
 body{font-family:Arial;padding:20px;max-width:700px;margin:auto}
 h1{text-align:center}
 table{width:100%;border-collapse:collapse}
 td{padding:8px;border-bottom:1px solid #ddd}
 .total{font-size:22px;font-weight:bold}
 </style>
 </head>
 <body>
 <h1>PHIẾU CẦM ĐỒ</h1>
 <h2>${esc(t.code)}</h2>

 <table>
 <tr><td>Khách hàng</td><td>${esc(t.customer.name)}</td></tr>
 <tr><td>SĐT</td><td>${esc(t.customer.phone||"")}</td></tr>
 <tr><td>CCCD</td><td>${esc(t.customer.cccd||"")}</td></tr>
 <tr><td>Tài sản</td><td>${esc(t.asset.type)} - ${esc(t.asset.desc)}</td></tr>
 <tr><td>Tiền cầm</td><td>${money(t.loan)}</td></tr>
 <tr><td>Lãi</td><td>${t.rate}% / tuần</td></tr>
 <tr><td>Ngày cầm</td><td>${fmtDate(t.pawnDate)}</td></tr>
 <tr><td>Ngày hạn</td><td>${fmtDate(t.dueDate)}</td></tr>
 <tr><td>Lãi đã đóng</td><td>${money(c.paidInterest)}</td></tr>
 </table>

 <h2>Tình trạng hiện tại</h2>
 <p>Lãi phát sinh: ${money(c.totalInterest)}</p>
 <p>Lãi chưa đóng: ${money(c.unpaidInterest)}</p>
 <p class="total">Tiền chuộc hiện tại: ${money(c.redeemTotal)}</p>

 <br><br>
 <table>
 <tr><td>Khách ký</td><td>Nhân viên ký</td></tr>
 <tr><td><br><br><br></td><td></td></tr>
 </table>

 <script>window.print()<\/script>
 </body>
 </html>
 `);

 w.document.close();
}

/* =========================
   RENDER TẤT CẢ
========================= */

function renderAll(){
 renderHome();
 renderTickets();
 renderInterest();
 renderRedeem();
 renderCustomers();
 renderAssets();
 renderOverdue();
 renderLiquid();
 renderCash();
 renderReports();
 loadSettings();
}

/* =========================
   REDEEM LIST
========================= */

function renderRedeem(){
 let q=($("redeemSearch")?.value||"").toLowerCase();

 let arr=db.tickets.filter(t=>{
  let s=status(t);
  if(s!=="active"&&s!=="overdue")return false;
  return JSON.stringify(t).toLowerCase().includes(q);
 });

 $("redeemList").innerHTML=arr.length
 ?arr.map(t=>{
  let c=calcTicket(t);

  return `
  <div class="card">
   <b>${t.code} - ${esc(t.customer.name)}</b>
   ${badge(status(t))}
   <hr>
   📦 ${esc(t.asset.desc)}<br>
   💰 Vốn: <b>${money(t.loan)}</b><br>
   📈 Lãi chưa đóng: <b>${money(c.unpaidInterest)}</b><br>
   ⚠️ Phạt: <b>${money(c.lateFee)}</b><br>
   💵 Chuộc hiện tại: <b class="big">${money(c.redeemTotal)}</b>

   <div class="actions">
    <button class="blue" onclick="openRedeem('${t.id}')">💵 Chuộc</button>
   </div>
  </div>`;
 }).join("")
 :`<div class="empty">Không có phiếu đang cầm.</div>`;
}

/* =========================
   KHỞI ĐỘNG
========================= */

document.addEventListener("DOMContentLoaded",()=>{
 renderAll();
});
</script>

</body>
</html>
