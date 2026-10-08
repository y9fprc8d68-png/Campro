<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#111827">
<title>CầmPro</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#f3f4f6;color:#111827;font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Arial,sans-serif}
button,input,select,textarea{font:inherit}
button{border:0;cursor:pointer}
header{background:#111827;color:white;padding:17px 15px;position:sticky;top:0;z-index:20}
header h1{margin:0;font-size:23px}
header small{opacity:.7}
nav{display:flex;overflow:auto;background:white;position:sticky;top:68px;z-index:19;border-bottom:1px solid #ddd}
nav button{padding:13px 14px;background:white;color:#6b7280;white-space:nowrap}
nav button.active{color:#111827;font-weight:700;border-bottom:3px solid #111827}
main{max-width:1100px;margin:auto;padding:15px}
.page{display:none}.page.active{display:block}
.grid{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
.card,.ticket{background:white;border-radius:14px;padding:14px;margin-bottom:10px;box-shadow:0 1px 5px #0000000d}
.stat span,.muted{font-size:13px;color:#6b7280}
.stat b{display:block;font-size:20px;margin-top:5px}
.title{display:flex;justify-content:space-between;align-items:center;margin:15px 0 10px}
.title h2{font-size:18px;margin:0}
.btn{background:#111827;color:white;padding:10px 13px;border-radius:9px;font-weight:700}
.green{background:#15803d}.red{background:#dc2626}.blue{background:#2563eb}
.orange{background:#ea580c}.gray{background:#e5e7eb;color:#111827}
.toolbar{display:flex;gap:8px;margin-bottom:12px}
.toolbar input{flex:1}
input,select,textarea{width:100%;padding:11px;border:1px solid #d1d5db;border-radius:9px;background:white;outline:0}
label{font-size:13px;font-weight:700;display:block;margin-bottom:5px}
.form{display:grid;grid-template-columns:repeat(2,1fr);gap:11px}
.full{grid-column:1/-1}
.row{display:flex;justify-content:space-between;gap:10px;margin:7px 0;flex-wrap:wrap}
.actions{display:flex;gap:7px;flex-wrap:wrap;margin-top:12px}
.badge{display:inline-block;padding:4px 8px;border-radius:99px;font-size:12px;font-weight:700;background:#dcfce7;color:#166534}
.badge.red{background:#fee2e2;color:#991b1b}
.badge.orange{background:#ffedd5;color:#9a3412}
.badge.gray{background:#e5e7eb;color:#374151}
.ticket{border-left:4px solid #111827}
.ticket.overdue{border-left-color:#dc2626}
.ticket.redeemed{border-left-color:#16a34a}
.ticket.liquidated{border-left-color:#ea580c}
.tickethead{display:flex;justify-content:space-between;gap:10px}
.empty{text-align:center;padding:35px;color:#6b7280;background:white;border-radius:14px}
.notice{background:#eff6ff;color:#1e40af;padding:12px;border-radius:10px;font-size:13px;margin-bottom:12px}
.quick{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
.quick button{background:white;padding:15px;border-radius:12px;text-align:left}
.modal{display:none;position:fixed;inset:0;background:#0008;z-index:100;padding:15px;overflow:auto}
.modal.show{display:block}
.modalbox{background:white;max-width:650px;margin:25px auto;padding:18px;border-radius:16px}
.modalhead{display:flex;justify-content:space-between;align-items:center;margin-bottom:15px}
.modalhead h2{margin:0;font-size:19px}
.close{width:34px;height:34px;border-radius:50%;background:#eee}
.preview{background:#f8fafc;border-radius:12px;padding:13px;margin-top:5px}
.preview .total{font-size:20px;font-weight:800}
@media(min-width:700px){
.grid{grid-template-columns:repeat(4,1fr)}
.quick{grid-template-columns:repeat(4,1fr)}
}
</style>
</head>

<body>

<header>
<h1>💰 CầmPro</h1>
<small>Quản lý cầm đồ</small>
</header>

<nav>
<button class="active" onclick="go('home',this)">🏠 Tổng quan</button>
<button onclick="go('tickets',this)">📄 Phiếu cầm</button>
<button onclick="go('redeem',this)">💵 Chuộc đồ</button>
<button onclick="go('customers',this)">👤 Khách</button>
<button onclick="go('assets',this)">📦 Tài sản</button>
<button onclick="go('overdue',this)">⚠️ Quá hạn</button>
<button onclick="go('liquidation',this)">🏷️ Thanh lý</button>
<button onclick="go('cash',this)">💰 Thu chi</button>
<button onclick="go('report',this)">📊 Báo cáo</button>
</nav>

<main>

<section id="home" class="page active">
<div class="title">
<h2>Tổng quan</h2>
<button class="btn" onclick="openNew()">+ Phiếu cầm</button>
</div>

<div class="grid">
<div class="card stat"><span>Đang cầm</span><b id="a1">0</b></div>
<div class="card stat"><span>Vốn đang nằm</span><b id="a2">0đ</b></div>
<div class="card stat"><span>Quá hạn</span><b id="a3">0</b></div>
<div class="card stat"><span>Khách hàng</span><b id="a4">0</b></div>
</div>

<div class="grid">
<div class="card stat"><span>Lãi đã thu</span><b id="a5">0đ</b></div>
<div class="card stat"><span>Lãi thanh lý</span><b id="a6">0đ</b></div>
<div class="card stat"><span>Đã chuộc</span><b id="a7">0</b></div>
<div class="card stat"><span>Tài sản giữ</span><b id="a8">0</b></div>
</div>

<div class="title"><h2>Thao tác nhanh</h2></div>
<div class="quick">
<button onclick="openNew()"><b>➕ Tạo phiếu</b><br><span class="muted">Nhận đồ cầm</span></button>
<button onclick="go('redeem')"><b>💵 Chuộc đồ</b><br><span class="muted">Tính tiền chuộc</span></button>
<button onclick="go('overdue')"><b>⚠️ Quá hạn</b><br><span class="muted">Kiểm tra phiếu</span></button>
<button onclick="go('cash')"><b>💰 Thu chi</b><br><span class="muted">Dòng tiền</span></button>
</div>

<div class="title"><h2>Phiếu mới nhất</h2></div>
<div id="recent"></div>
</section>

<section id="tickets" class="page">
<div class="title"><h2>📄 Phiếu cầm</h2><button class="btn" onclick="openNew()">+ Tạo</button></div>
<div class="toolbar"><input id="searchTicket" placeholder="Tìm mã, khách, SĐT, tài sản..." oninput="renderTickets()"></div>
<div id="ticketList"></div>
</section>

<section id="redeem" class="page">
<div class="title"><h2>💵 Chuộc đồ</h2></div>
<div class="notice">Tiền chuộc = tiền cầm + lãi + phí dịch vụ + phí quá hạn.</div>
<div class="toolbar"><input id="searchRedeem" placeholder="Nhập mã phiếu / tên khách..." oninput="renderRedeem()"></div>
<div id="redeemList"></div>
</section>

<section id="customers" class="page">
<div class="title"><h2>👤 Khách hàng</h2></div>
<div class="toolbar"><input id="searchCustomer" placeholder="Tên / SĐT / CCCD..." oninput="renderCustomers()"></div>
<div id="customerList"></div>
</section>

<section id="assets" class="page">
<div class="title"><h2>📦 Tài sản đang giữ</h2></div>
<div id="assetList"></div>
</section>

<section id="overdue" class="page">
<div class="title"><h2>⚠️ Quá hạn</h2></div>
<div id="overdueList"></div>
</section>

<section id="liquidation" class="page">
<div class="title"><h2>🏷️ Thanh lý</h2></div>
<div class="notice">Chọn phiếu quá hạn để thanh lý. Lãi/lỗ = giá bán - tiền cầm - chi phí.</div>
<div id="liquidList"></div>
</section>

<section id="cash" class="page">
<div class="title"><h2>💰 Thu chi</h2><button class="btn" onclick="openCash()">+ Ghi</button></div>
<div class="grid">
<div class="card stat"><span>Tổng thu</span><b id="ci">0đ</b></div>
<div class="card stat"><span>Tổng chi</span><b id="co">0đ</b></div>
<div class="card stat"><span>Chênh lệch</span><b id="cb">0đ</b></div>
<div class="card stat"><span>Giao dịch</span><b id="cc">0</b></div>
</div>
<div id="cashList"></div>
</section>

<section id="report" class="page">
<div class="title"><h2>📊 Báo cáo</h2></div>
<div class="grid">
<div class="card stat"><span>Tổng tiền cầm</span><b id="rLoan">0đ</b></div>
<div class="card stat"><span>Lãi đã thu</span><b id="rInt">0đ</b></div>
<div class="card stat"><span>Lãi thanh lý</span><b id="rLiq">0đ</b></div>
<div class="card stat"><span>Vốn đang nằm</span><b id="rCap">0đ</b></div>
</div>
<div class="card">
<h3>Thống kê</h3>
<div class="row"><span>Tổng phiếu</span><b id="rt">0</b></div>
<div class="row"><span>Đang cầm</span><b id="ra">0</b></div>
<div class="row"><span>Quá hạn</span><b id="ro">0</b></div>
<div class="row"><span>Đã chuộc</span><b id="rr">0</b></div>
<div class="row"><span>Đã thanh lý</span><b id="rl">0</b></div>
</div>

<div class="card">
<h3>💾 Sao lưu dữ liệu</h3>
<p class="muted">Nên sao lưu thường xuyên để tránh mất dữ liệu khi đổi máy hoặc xóa trình duyệt.</p>
<div class="actions">
<button class="btn blue" onclick="backup()">⬇️ Sao lưu</button>
<button class="btn gray" onclick="document.getElementById('restore').click()">⬆️ Khôi phục</button>
<input id="restore" type="file" accept=".json" style="display:none" onchange="restoreData(event)">
</div>
</div>
</section>

</main>

<!-- TẠO PHIẾU -->
<div id="newModal" class="modal">
<div class="modalbox">
<div class="modalhead"><h2>➕ Tạo phiếu cầm</h2><button class="close" onclick="closeM('newModal')">✕</button></div>

<form onsubmit="createTicket(event)">
<div class="form">

<div>
<label>Tên khách *</label>
<input id="name" required>
</div>

<div>
<label>Số điện thoại</label>
<input id="phone" inputmode="tel">
</div>

<div>
<label>CCCD</label>
<input id="cccd">
</div>

<div>
<label>Loại tài sản *</label>
<select id="type" required>
<option value="">Chọn loại</option>
<option>Điện thoại</option>
<option>Xe máy</option>
<option>Ô tô</option>
<option>Laptop</option>
<option>Máy ảnh</option>
<option>Vàng</option>
<option>Khác</option>
</select>
</div>

<div class="full">
<label>Mô tả tài sản *</label>
<input id="asset" placeholder="VD: iPhone 17 Pro Max 256GB..." required>
</div>

<div>
<label>Giá định giá</label>
<input id="value" type="number" min="0" step="1000">
</div>

<div>
<label>Tiền cầm *</label>
<input id="loan" type="number" min="0" step="1000" required>
</div>

<div>
<label>Lãi suất / tháng (%)</label>
<input id="rate" type="number" min="0" step=".01" value="3">
</div>

<div>
<label>Ngày cầm</label>
<input id="pawnDate" type="date" required>
</div>

<div>
<label>Thời hạn (ngày)</label>
<input id="term" type="number" min="1" value="30">
</div>

<div>
<label>Phí dịch vụ</label>
<input id="serviceFee" type="number" min="0" step="1000" value="0">
</div>

<div>
<label>Phí quá hạn / ngày</label>
<input id="lateFee" type="number" min="0" step="1000" value="0">
</div>

<div class="full">
<label>Ghi chú</label>
<textarea id="note" rows="3"></textarea>
</div>

<div class="full preview">
<div class="row"><span>💰 Tiền cầm</span><b id="pLoan">0đ</b></div>
<div class="row"><span>📈 Lãi dự kiến</span><b id="pInterest">0đ</b></div>
<div class="row"><span>🧾 Phí dịch vụ</span><b id="pService">0đ</b></div>
<div class="row"><span>⚠️ Phí quá hạn</span><b id="pLate">0đ</b></div>
<div class="row total"><span>TỔNG TIỀN CHUỘC</span><b id="pTotal">0đ</b></div>
</div>

</div>

<div class="actions">
<button type="button" class="btn gray" onclick="closeM('newModal')">Hủy</button>
<button class="btn green">💾 Lưu phiếu</button>
</div>
</form>
</div>
</div>

<!-- GIA HẠN -->
<div id="extendModal" class="modal">
<div class="modalbox">
<div class="modalhead"><h2>🔄 Gia hạn</h2><button class="close" onclick="closeM('extendModal')">✕</button></div>
<input type="hidden" id="extendId">
<p id="extendText"></p>
<label>Gia hạn thêm ngày</label>
<input id="extendDays" type="number" min="1" value="30">
<div class="actions">
<button class="btn gray" onclick="closeM('extendModal')">Hủy</button>
<button class="btn blue" onclick="doExtend()">Gia hạn</button>
</div>
</div>
</div>

<!-- THANH LÝ -->
<div id="liquidModal" class="modal">
<div class="modalbox">
<div class="modalhead"><h2>🏷️ Thanh lý</h2><button class="close" onclick="closeM('liquidModal')">✕</button></div>
<input type="hidden" id="liquidId">
<div id="liquidInfo" class="notice"></div>
<label>Giá bán</label>
<input id="sale" type="number" min="0" step="1000" oninput="liquidPreview()">
<br>
<label>Chi phí thanh lý</label>
<input id="liquidCost" type="number" min="0" step="1000" value="0" oninput="liquidPreview()">
<div id="liqPreview" class="notice" style="margin-top:12px"></div>
<div class="actions">
<button class="btn gray" onclick="closeM('liquidModal')">Hủy</button>
<button class="btn orange" onclick="doLiquid()">Xác nhận thanh lý</button>
</div>
</div>
</div>

<!-- THU CHI -->
<div id="cashModal" class="modal">
<div class="modalbox">
<div class="modalhead"><h2>💰 Ghi thu/chi</h2><button class="close" onclick="closeM('cashModal')">✕</button></div>
<form onsubmit="createCash(event)">
<label>Loại</label>
<select id="cashType"><option value="in">Thu</option><option value="out">Chi</option></select>
<br>
<label>Số tiền</label>
<input id="cashAmount" type="number" min="0" step="1000" required>
<br>
<label>Danh mục</label>
<input id="cashCat" placeholder="VD: tiền điện, tiền thuê...">
<br>
<label>Ngày</label>
<input id="cashDate" type="date" required>
<br>
<label>Ghi chú</label>
<textarea id="cashNote"></textarea>
<div class="actions">
<button type="button" class="btn gray" onclick="closeM('cashModal')">Hủy</button>
<button class="btn green">Lưu</button>
</div>
</form>
</div>
</div>

<script>
const KEY="CAMPRO_FULL_V3";

let db=JSON.parse(localStorage.getItem(KEY)||"null")||{
 tickets:[],
 cash:[]
};

const $=id=>document.getElementById(id);

function save(){
 localStorage.setItem(KEY,JSON.stringify(db));
 renderAll();
}

function money(n){
 return Number(n||0).toLocaleString("vi-VN")+"đ";
}

function today(){
 return new Date().toISOString().slice(0,10);
}

function vn(d){
 if(!d)return "";
 return new Date(d+"T00:00:00").toLocaleDateString("vi-VN");
}

function days(a,b){
 return Math.max(0,Math.ceil(
  (new Date(b+"T00:00:00")-new Date(a+"T00:00:00"))/86400000
 ));
}

function addDays(date,n){
 let d=new Date(date+"T00:00:00");
 d.setDate(d.getDate()+Number(n));
 return d.toISOString().slice(0,10);
}

function lateDays(t,end=today()){
 return t.due<end?days(t.due,end):0;
}

function calc(t,end=today()){
 let d=days(t.pawn,end);
 let interest=t.loan*(t.rate/100)*(d/30);
 let late=lateDays(t,end)*Number(t.lateFee||0);

 return {
  days:d,
  interest,
  service:Number(t.serviceFee||0),
  late,
  total:t.loan+interest+Number(t.serviceFee||0)+late
 };
}

function status(t){
 if(t.status==="redeemed")return "redeemed";
 if(t.status==="liquidated")return "liquidated";
 if(t.due<today())return "overdue";
 return "active";
}

function badge(t){
 let s=status(t);
 if(s==="active")return '<span class="badge">Đang cầm</span>';
 if(s==="overdue")return '<span class="badge red">Quá hạn</span>';
 if(s==="redeemed")return '<span class="badge gray">Đã chuộc</span>';
 return '<span class="badge orange">Đã thanh lý</span>';
}

function esc(x){
 return String(x??"")
 .replace(/&/g,"&amp;")
 .replace(/</g,"&lt;")
 .replace(/>/g,"&gt;")
 .replace(/"/g,"&quot;");
}

function go(id,btn){
 document.querySelectorAll(".page").forEach(x=>x.classList.remove("active"));
 $(id).classList.add("active");
 document.querySelectorAll("nav button").forEach(x=>x.classList.remove("active"));
 if(btn)btn.classList.add("active");
 renderAll();
 window.scrollTo(0,0);
}

function closeM(id){$(id).classList.remove("show")}
function openM(id){$(id).classList.add("show")}

function openNew(){
 $("pawnDate").value=today();
 updatePreview();
 openM("newModal");
}

function nextId(){
 let max=0;
 db.tickets.forEach(t=>{
  let n=parseInt(String(t.id).replace(/\D/g,""))||0;
  max=Math.max(max,n);
 });
 return "CD-"+String(max+1).padStart(5,"0");
}

function updatePreview(){
 let loan=Number($("loan").value||0);
 let rate=Number($("rate").value||0);
 let term=Number($("term").value||0);
 let service=Number($("serviceFee").value||0);

 let interest=loan*(rate/100)*(term/30);
 let total=loan+interest+service;

 $("pLoan").textContent=money(loan);
 $("pInterest").textContent=money(interest);
 $("pService").textContent=money(service);
 $("pLate").textContent="0đ";
 $("pTotal").textContent=money(total);
}

["loan","rate","term","serviceFee","lateFee"].forEach(id=>{
 $(id).addEventListener("input",updatePreview);
});

function createTicket(e){
 e.preventDefault();

 let pawn=$("pawnDate").value;
 let term=Number($("term").value||30);
 let loan=Number($("loan").value||0);

 let t={
  id:nextId(),
  customer:{
   name:$("name").value.trim(),
   phone:$("phone").value.trim(),
   cccd:$("cccd").value.trim()
  },
  asset:{
   type:$("type").value,
   desc:$("asset").value.trim(),
   value:Number($("value").value||0)
  },
  loan,
  rate:Number($("rate").value||0),
  pawn,
  term,
  due:addDays(pawn,term),
  serviceFee:Number($("serviceFee").value||0),
  lateFee:Number($("lateFee").value||0),
  note:$("note").value.trim(),
  status:"active",
  redeemed:null,
  liquidation:null,
  created:new Date().toISOString()
 };

 db.tickets.unshift(t);

 db.cash.unshift({
  id:Date.now(),
  date:pawn,
  type:"out",
  amount:loan,
  category:"Giải ngân",
  ref:t.id,
  note:"Cầm "+t.asset.desc
 });

 save();
 closeM("newModal");
 e.target.reset();

 alert("Đã tạo phiếu "+t.id);
}

function card(t){
 let s=status(t);
 let c=calc(t);

 return `
 <div class="ticket ${s}">
  <div class="tickethead">
   <div>
    <b>${t.id}</b><br>
    <b>${esc(t.customer.name)}</b>
    <div class="muted">${esc(t.customer.phone||"")}</div>
   </div>
   ${badge(t)}
  </div>

  <div class="row">
   <span>📦 ${esc(t.asset.type)} - ${esc(t.asset.desc)}</span>
  </div>

  <div class="row"><span>Tiền cầm</span><b>${money(t.loan)}</b></div>
  <div class="row"><span>Ngày cầm</span><span>${vn(t.pawn)}</span></div>
  <div class="row"><span>Đến hạn</span><span>${vn(t.due)}</span></div>

  ${(s==="active"||s==="overdue")?`
  <div class="row"><span>Lãi hiện tại</span><b>${money(c.interest)}</b></div>
  <div class="row"><span>Phí quá hạn</span><b>${money(c.late)}</b></div>
  <div class="row" style="font-size:17px">
   <span>Tạm tính chuộc</span><b>${money(c.total)}</b>
  </div>
  `:""}

  ${s==="overdue"?`
  <div class="row"><span>Quá hạn</span><b style="color:#dc2626">${lateDays(t)} ngày</b></div>
  `:""}

  ${s==="redeemed"?`
  <div class="row"><span>Đã chuộc</span><b>${vn(t.redeemed.date)}</b></div>
  <div class="row"><span>Đã thu</span><b>${money(t.redeemed.total)}</b></div>
  `:""}

  ${s==="liquidated"?`
  <div class="row"><span>Giá bán</span><b>${money(t.liquidation.sale)}</b></div>
  <div class="row"><span>Lãi/lỗ</span><b>${money(t.liquidation.profit)}</b></div>
  `:""}

  <div class="actions">
  ${(s==="active"||s==="overdue")?`
   <button class="btn green" onclick="redeem('${t.id}')">💵 Chuộc</button>
   <button class="btn blue" onclick="openExtend('${t.id}')">🔄 Gia hạn</button>
  `:""}
  ${s==="overdue"?`
   <button class="btn orange" onclick="openLiquid('${t.id}')">🏷️ Thanh lý</button>
  `:""}
   <button class="btn gray" onclick="printTicket('${t.id}')">🖨️ In</button>
  </div>
 </div>`;
}

function renderTickets(){
 let q=($("searchTicket")?.value||"").toLowerCase();

 let arr=db.tickets.filter(t=>
 [t.id,t.customer.name,t.customer.phone,t.customer.cccd,t.asset.desc]
 .join(" ").toLowerCase().includes(q)
 );

 $("ticketList").innerHTML=arr.length?
 arr.map(card).join(""):'<div class="empty">Chưa có phiếu.</div>';
}

function renderRedeem(){
 let q=($("searchRedeem")?.value||"").toLowerCase();

 let arr=db.tickets.filter(t=>{
  let s=status(t);
  return (s==="active"||s==="overdue") &&
  [t.id,t.customer.name,t.customer.phone,t.asset.desc]
  .join(" ").toLowerCase().includes(q);
 });

 $("redeemList").innerHTML=arr.length?arr.map(t=>{
  let c=calc(t);
  return `
  <div class="ticket ${status(t)}">
   <div class="tickethead">
    <div><b>${t.id}</b><br>${esc(t.customer.name)}</div>
    ${badge(t)}
   </div>
   <div class="row"><span>Tiền cầm</span><b>${money(t.loan)}</b></div>
   <div class="row"><span>Số ngày</span><b>${c.days} ngày</b></div>
   <div class="row"><span>Lãi</span><b>${money(c.interest)}</b></div>
   <div class="row"><span>Phí dịch vụ</span><b>${money(c.service)}</b></div>
   <div class="row"><span>Phí quá hạn</span><b>${money(c.late)}</b></div>
   <div class="row" style="font-size:21px">
    <span>TỔNG CHUỘC</span><b>${money(c.total)}</b>
   </div>
   <div class="actions">
    <button class="btn green" onclick="redeem('${t.id}')">💵 Xác nhận chuộc</button>
   </div>
  </div>`;
 }).join(""):'<div class="empty">Không có phiếu đang cầm.</div>';
}

function redeem(id){
 let t=db.tickets.find(x=>x.id===id);
 if(!t)return;

 let c=calc(t);

 if(!confirm(
  "PHIẾU "+t.id+
  "\\nTiền cầm: "+money(t.loan)+
  "\\nLãi: "+money(c.interest)+
  "\\nPhí dịch vụ: "+money(c.service)+
  "\\nPhí quá hạn: "+money(c.late)+
  "\\n----------------"+
  "\\nTỔNG CHUỘC: "+money(c.total)+
  "\\n\\nXác nhận khách đã chuộc?"
 ))return;

 t.status="redeemed";
 t.redeemed={
  date:today(),
  days:c.days,
  interest:c.interest,
  service:c.service,
  late:c.late,
  total:c.total
 };

 db.cash.unshift({
  id:Date.now(),
  date:today(),
  type:"in",
  amount:c.total,
  category:"Tiền chuộc",
  ref:t.id,
  note:"Khách "+t.customer.name+" chuộc đồ"
 });

 save();
 alert("Đã đóng phiếu "+t.id);
}

function openExtend(id){
 let t=db.tickets.find(x=>x.id===id);
 $("extendId").value=id;
 $("extendText").innerHTML=
 "Phiếu <b>"+t.id+"</b><br>Hạn hiện tại: <b>"+vn(t.due)+"</b>";
 openM("extendModal");
}

function doExtend(){
 let id=$("extendId").value;
 let n=Number($("extendDays").value||0);
 let t=db.tickets.find(x=>x.id===id);
 if(!t||n<=0)return;

 t.due=addDays(t.due,n);
 t.term+=n;

 save();
 closeM("extendModal");
 alert("Đã gia hạn thêm "+n+" ngày.");
}

function renderOverdue(){
 let arr=db.tickets.filter(t=>status(t)==="overdue");

 $("overdueList").innerHTML=arr.length?
 arr.map(card).join(""):'<div class="empty">🎉 Không có phiếu quá hạn.</div>';
}

function openLiquid(id){
 let t=db.tickets.find(x=>x.id===id);

 $("liquidId").value=id;
 $("sale").value="";
 $("liquidCost").value=0;

 $("liquidInfo").innerHTML=
 "<b>"+t.id+"</b> - "+esc(t.asset.desc)+
 "<br>Tiền cầm: <b>"+money(t.loan)+"</b>";

 liquidPreview();
 openM("liquidModal");
}

function liquidPreview(){
 let t=db.tickets.find(x=>x.id===$("liquidId").value);
 if(!t)return;

 let sale=Number($("sale").value||0);
 let cost=Number($("liquidCost").value||0);
 let profit=sale-t.loan-cost;

 $("liqPreview").innerHTML=
 "Lãi/lỗ thanh lý: <b>"+money(profit)+"</b>";
}

function doLiquid(){
 let t=db.tickets.find(x=>x.id===$("liquidId").value);
 if(!t)return;

 let sale=Number($("sale").value||0);
 let cost=Number($("liquidCost").value||0);
 let profit=sale-t.loan-cost;

 if(!confirm(
 "Giá bán: "+money(sale)+
 "\\nChi phí: "+money(cost)+
 "\\nLãi/lỗ: "+money(profit)+
 "\\n\\nXác nhận thanh lý?"
 ))return;

 t.status="liquidated";
 t.liquidation={
  date:today(),
  sale,
  cost,
  profit
 };

 db.cash.unshift({
  id:Date.now(),
  date:today(),
  type:"in",
  amount:sale,
  category:"Bán thanh lý",
  ref:t.id,
  note:t.asset.desc
 });

 if(cost>0){
  db.cash.unshift({
   id:Date.now()+1,
   date:today(),
   type:"out",
   amount:cost,
   category:"Chi phí thanh lý",
   ref:t.id,
   note:"Chi phí thanh lý"
  });
 }

 save();
 closeM("liquidModal");
 alert("Đã thanh lý "+t.id);
}

function renderLiquid(){
 let arr=db.tickets.filter(t=>status(t)==="overdue");

 $("liquidList").innerHTML=arr.length?
 arr.map(t=>`
 <div class="card">
  <b>${t.id}</b> - ${esc(t.customer.name)}
  <div class="muted">${esc(t.asset.desc)}</div>
  <div class="row"><span>Tiền cầm</span><b>${money(t.loan)}</b></div>
  <button class="btn orange" onclick="openLiquid('${t.id}')">🏷️ Thanh lý</button>
 </div>
 `).join(""):'<div class="empty">Không có phiếu chờ thanh lý.</div>';
}

function customers(){
 let m={};

 db.tickets.forEach(t=>{
  let k=t.customer.phone||t.customer.cccd||t.customer.name;
  if(!m[k])m[k]={
   name:t.customer.name,
   phone:t.customer.phone,
   cccd:t.customer.cccd,
   count:0,
   active:0,
   loan:0
  };

  m[k].count++;

  if(status(t)==="active"||status(t)==="overdue"){
   m[k].active++;
   m[k].loan+=t.loan;
  }
 });

 return Object.values(m);
}

function renderCustomers(){
 let q=($("searchCustomer")?.value||"").toLowerCase();

 let arr=customers().filter(c=>
 [c.name,c.phone,c.cccd].join(" ").toLowerCase().includes(q)
 );

 $("customerList").innerHTML=arr.length?
 arr.map(c=>`
 <div class="card">
  <b>👤 ${esc(c.name)}</b>
  <div class="muted">${esc(c.phone||"")} ${c.cccd?"• CCCD "+esc(c.cccd):""}</div>
  <div class="row"><span>Số phiếu</span><b>${c.count}</b></div>
  <div class="row"><span>Đang cầm</span><b>${c.active}</b></div>
  <div class="row"><span>Vốn đang cầm</span><b>${money(c.loan)}</b></div>
 </div>
 `).join(""):'<div class="empty">Chưa có khách hàng.</div>';
}

function renderAssets(){
 let arr=db.tickets.filter(t=>status(t)==="active"||status(t)==="overdue");

 $("assetList").innerHTML=arr.length?
 arr.map(t=>`
 <div class="card">
  <div class="tickethead">
   <div><b>📦 ${esc(t.asset.desc)}</b><div class="muted">${t.asset.type}</div></div>
   ${badge(t)}
  </div>
  <div class="row"><span>Khách</span><b>${esc(t.customer.name)}</b></div>
  <div class="row"><span>Định giá</span><b>${money(t.asset.value)}</b></div>
  <div class="row"><span>Tiền cầm</span><b>${money(t.loan)}</b></div>
  <div class="row"><span>Đến hạn</span><b>${vn(t.due)}</b></div>
 </div>
 `).join(""):'<div class="empty">Không có tài sản đang giữ.</div>';
}

function openCash(){
 $("cashDate").value=today();
 openM("cashModal");
}

function createCash(e){
 e.preventDefault();

 db.cash.unshift({
  id:Date.now(),
  date:$("cashDate").value,
  type:$("cashType").value,
  amount:Number($("cashAmount").value||0),
  category:$("cashCat").value||"Khác",
  ref:"",
  note:$("cashNote").value
 });

 save();
 closeM("cashModal");
 e.target.reset();
 alert("Đã ghi giao dịch.");
}

function renderCash(){
 let income=0,out=0;

 db.cash.forEach(x=>{
  if(x.type==="in")income+=x.amount;
  else out+=x.amount;
 });

 $("ci").textContent=money(income);
 $("co").textContent=money(out);
 $("cb").textContent=money(income-out);
 $("cc").textContent=db.cash.length;

 $("cashList").innerHTML=db.cash.length?
 db.cash.map(x=>`
 <div class="card">
  <div class="tickethead">
   <div>
    <b>${x.type==="in"?"🟢 Thu":"🔴 Chi"} - ${esc(x.category)}</b>
    <div class="muted">${vn(x.date)} ${x.ref?"• "+x.ref:""}</div>
   </div>
   <b>${x.type==="in"?"+":"-"}${money(x.amount)}</b>
  </div>
  ${x.note?'<div class="muted">'+esc(x.note)+'</div>':""}
 </div>
 `).join(""):'<div class="empty">Chưa có giao dịch.</div>';
}

function renderHome(){
 let active=db.tickets.filter(t=>status(t)==="active"||status(t)==="overdue");
 let overdue=db.tickets.filter(t=>status(t)==="overdue").length;
 let redeemed=db.tickets.filter(t=>t.status==="redeemed").length;

 let capital=active.reduce((s,t)=>s+t.loan,0);
 let interest=db.tickets.reduce((s,t)=>s+(t.redeemed?.interest||0),0);
 let liquid=db.tickets.reduce((s,t)=>s+(t.liquidation?.profit||0),0);

 $("a1").textContent=active.length;
 $("a2").textContent=money(capital);
 $("a3").textContent=overdue;
 $("a4").textContent=customers().length;
 $("a5").textContent=money(interest);
 $("a6").textContent=money(liquid);
 $("a7").textContent=redeemed;
 $("a8").textContent=active.length;

 let recent=db.tickets.slice(0,5);
 $("recent").innerHTML=recent.length?
 recent.map(card).join(""):'<div class="empty">Chưa có phiếu.</div>';
}

function renderReport(){
 let totalLoan=db.tickets.reduce((s,t)=>s+t.loan,0);
 let interest=db.tickets.reduce((s,t)=>s+(t.redeemed?.interest||0),0);
 let liquid=db.tickets.reduce((s,t)=>s+(t.liquidation?.profit||0),0);
 let active=db.tickets.filter(t=>status(t)==="active").length;
 let overdue=db.tickets.filter(t=>status(t)==="overdue").length;
 let capital=db.tickets.filter(t=>status(t)==="active"||status(t)==="overdue")
 .reduce((s,t)=>s+t.loan,0);

 $("rLoan").textContent=money(totalLoan);
 $("rInt").textContent=money(interest);
 $("rLiq").textContent=money(liquid);
 $("rCap").textContent=money(capital);

 $("rt").textContent=db.tickets.length;
 $("ra").textContent=active;
 $("ro").textContent=overdue;
 $("rr").textContent=db.tickets.filter(t=>t.status==="redeemed").length;
 $("rl").textContent=db.tickets.filter(t=>t.status==="liquidated").length;
}

function backup(){
 let blob=new Blob([JSON.stringify(db,null,2)],{type:"application/json"});
 let a=document.createElement("a");
 a.href=URL.createObjectURL(blob);
 a.download="CamPro-"+today()+".json";
 a.click();
}

function restoreData(e){
 let file=e.target.files[0];
 if(!file)return;

 let reader=new FileReader();

 reader.onload=()=>{
  try{
   let x=JSON.parse(reader.result);

   if(!x.tickets||!x.cash)throw 1;

   if(confirm("Khôi phục sẽ thay dữ liệu hiện tại. Tiếp tục?")){
    db=x;
    save();
    alert("Khôi phục thành công.");
   }
  }catch{
   alert("File sao lưu không hợp lệ.");
  }
 };

 reader.readAsText(file);
}

function printTicket(id){
 let t=db.tickets.find(x=>x.id===id);
 if(!t)return;

 let c=calc(t);

 let w=window.open("","_blank");

 w.document.write(`
 <!doctype html>
 <html>
 <head>
 <title>${t.id}</title>
 <style>
 body{font-family:Arial;padding:25px;max-width:600px;margin:auto}
 h1,h2{text-align:center}
 .line{padding:8px 0;border-bottom:1px solid #ddd}
 .total{font-size:22px;font-weight:bold}
 </style>
 </head>
 <body>
 <h1>💰 CẦMPRO</h1>
 <h2>PHIẾU CẦM ĐỒ</h2>

 <div class="line"><b>Mã:</b> ${t.id}</div>
 <div class="line"><b>Khách:</b> ${esc(t.customer.name)}</div>
 <div class="line"><b>SĐT:</b> ${esc(t.customer.phone)}</div>
 <div class="line"><b>CCCD:</b> ${esc(t.customer.cccd)}</div>
 <div class="line"><b>Tài sản:</b> ${esc(t.asset.type)} - ${esc(t.asset.desc)}</div>
 <div class="line"><b>Giá định giá:</b> ${money(t.asset.value)}</div>
 <div class="line"><b>Tiền cầm:</b> ${money(t.loan)}</div>
 <div class="line"><b>Lãi:</b> ${t.rate}%/tháng</div>
 <div class="line"><b>Ngày cầm:</b> ${vn(t.pawn)}</div>
 <div class="line"><b>Đến hạn:</b> ${vn(t.due)}</div>
 <div class="line"><b>Phí dịch vụ:</b> ${money(t.serviceFee)}</div>
 <div class="line total">Tiền chuộc dự kiến: ${money(c.total)}</div>

 <br><br>
 Khách hàng ký: ______________________
 <br><br>
 Cửa hàng ký: ________________________

 <script>window.print()<\/script>
 </body>
 </html>
 `);

 w.document.close();
}

function renderAll(){
 renderHome();
 renderTickets();
 renderRedeem();
 renderCustomers();
 renderAssets();
 renderOverdue();
 renderLiquid();
 renderCash();
 renderReport();
}

$("pawnDate").value=today();
$("cashDate").value=today();

renderAll();
</script>

</body>
</html>
