<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="theme-color" content="#111827">
<title>CầmPro - Quản lý cầm đồ</title>

<style>
*{box-sizing:border-box}
body{
  margin:0;
  font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Arial,sans-serif;
  background:#f3f4f6;color:#111827;
}
button,input,select,textarea{font:inherit}
button{cursor:pointer;border:0}
.header{
  background:#111827;color:white;padding:18px 16px;
  position:sticky;top:0;z-index:20;
}
.header h1{margin:0;font-size:23px}
.header small{opacity:.7}
.nav{
  display:flex;overflow-x:auto;background:white;border-bottom:1px solid #ddd;
  position:sticky;top:72px;z-index:19;
}
.nav button{
  flex:0 0 auto;padding:13px 14px;background:white;color:#6b7280;
  border-bottom:3px solid transparent;
}
.nav button.active{color:#111827;border-color:#111827;font-weight:700}
main{padding:15px;max-width:1100px;margin:auto}
.page{display:none}.page.active{display:block}

.grid{
  display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin-bottom:15px
}
.card{
  background:white;border-radius:14px;padding:15px;
  box-shadow:0 1px 5px #0000000d;
}
.stat b{font-size:20px;display:block;margin-top:5px}
.stat span{font-size:13px;color:#6b7280}

.section-title{
  display:flex;justify-content:space-between;align-items:center;
  margin:18px 0 10px;
}
.section-title h2{font-size:18px;margin:0}

.btn{
  padding:11px 14px;border-radius:10px;font-weight:700;
  background:#111827;color:white;
}
.btn.green{background:#15803d}
.btn.red{background:#dc2626}
.btn.orange{background:#ea580c}
.btn.gray{background:#e5e7eb;color:#111827}
.btn.blue{background:#2563eb}

input,select,textarea{
  width:100%;padding:11px;border:1px solid #d1d5db;border-radius:9px;
  background:white;outline:none;
}
input:focus,select:focus,textarea:focus{border-color:#111827}
label{font-size:13px;font-weight:600;margin-bottom:5px;display:block}
.form-grid{
  display:grid;grid-template-columns:repeat(2,1fr);gap:11px
}
.full{grid-column:1/-1}
.form-group{margin-bottom:2px}

.toolbar{
  display:flex;gap:8px;margin-bottom:12px
}
.toolbar input{flex:1}

.ticket{
  background:white;border-radius:14px;padding:14px;margin-bottom:10px;
  border-left:4px solid #111827;
  box-shadow:0 1px 5px #0000000d;
}
.ticket.overdue{border-left-color:#dc2626}
.ticket.redeemed{border-left-color:#15803d}
.ticket.liquidated{border-left-color:#ea580c}

.ticket-head{
  display:flex;justify-content:space-between;gap:10px;
}
.ticket-code{font-weight:800}
.muted{color:#6b7280;font-size:13px}
.money{font-weight:800}
.row{
  display:flex;justify-content:space-between;gap:10px;
  margin:7px 0;flex-wrap:wrap;
}
.actions{display:flex;gap:7px;flex-wrap:wrap;margin-top:12px}

.badge{
  display:inline-block;padding:4px 8px;border-radius:999px;
  font-size:12px;font-weight:700;background:#dcfce7;color:#166534;
}
.badge.red{background:#fee2e2;color:#991b1b}
.badge.orange{background:#ffedd5;color:#9a3412}
.badge.gray{background:#e5e7eb;color:#374151}

.empty{
  text-align:center;padding:35px 10px;color:#6b7280;background:white;border-radius:14px
}

.modal{
  display:none;position:fixed;inset:0;background:#0008;z-index:100;
  padding:15px;overflow:auto;
}
.modal.show{display:block}
.modal-box{
  background:white;max-width:650px;margin:30px auto;padding:18px;
  border-radius:16px;
}
.modal-head{
  display:flex;justify-content:space-between;align-items:center;margin-bottom:15px
}
.modal-head h2{margin:0;font-size:19px}
.close{background:#eee;border-radius:50%;width:34px;height:34px}

table{width:100%;border-collapse:collapse;background:white}
th,td{padding:10px 8px;border-bottom:1px solid #eee;text-align:left;font-size:13px}
th{background:#f9fafb}

.notice{
  padding:12px;border-radius:10px;background:#eff6ff;color:#1e40af;
  font-size:13px;margin-bottom:12px;
}

.quick{
  display:grid;grid-template-columns:repeat(2,1fr);gap:10px
}
.quick button{
  padding:15px;border-radius:12px;background:white;text-align:left;
  box-shadow:0 1px 5px #0000000d;
}
.quick b{display:block;margin-bottom:4px}

@media(min-width:700px){
  .grid{grid-template-columns:repeat(4,1fr)}
  .quick{grid-template-columns:repeat(4,1fr)}
}
</style>
</head>

<body>

<header class="header">
  <h1>💰 CầmPro</h1>
  <small>Quản lý cầm đồ</small>
</header>

<nav class="nav">
  <button class="active" onclick="page('home',this)">🏠 Tổng quan</button>
  <button onclick="page('tickets',this)">📄 Phiếu cầm</button>
  <button onclick="page('redeem',this)">💵 Chuộc đồ</button>
  <button onclick="page('customers',this)">👤 Khách hàng</button>
  <button onclick="page('assets',this)">📦 Tài sản</button>
  <button onclick="page('overdue',this)">⚠️ Quá hạn</button>
  <button onclick="page('liquidation',this)">🏷️ Thanh lý</button>
  <button onclick="page('cash',this)">💰 Thu chi</button>
  <button onclick="page('reports',this)">📊 Báo cáo</button>
</nav>

<main>

<!-- TỔNG QUAN -->
<section id="home" class="page active">
  <div class="section-title">
    <h2>Tổng quan</h2>
    <button class="btn" onclick="openTicket()">+ Phiếu cầm</button>
  </div>

  <div class="grid">
    <div class="card stat"><span>Đang cầm</span><b id="sActive">0</b></div>
    <div class="card stat"><span>Vốn đang nằm</span><b id="sCapital">0đ</b></div>
    <div class="card stat"><span>Quá hạn</span><b id="sOverdue">0</b></div>
    <div class="card stat"><span>Đã chuộc</span><b id="sRedeemed">0</b></div>
  </div>

  <div class="grid">
    <div class="card stat"><span>Tiền lãi đã thu</span><b id="sInterest">0đ</b></div>
    <div class="card stat"><span>Lãi thanh lý</span><b id="sLiquidProfit">0đ</b></div>
    <div class="card stat"><span>Khách hàng</span><b id="sCustomers">0</b></div>
    <div class="card stat"><span>Tài sản đang giữ</span><b id="sAssets">0</b></div>
  </div>

  <div class="section-title"><h2>Thao tác nhanh</h2></div>
  <div class="quick">
    <button onclick="openTicket()"><b>➕ Tạo phiếu</b><span class="muted">Nhận đồ cầm</span></button>
    <button onclick="pageById('redeem')"><b>💵 Chuộc đồ</b><span class="muted">Tính tiền chuộc</span></button>
    <button onclick="pageById('overdue')"><b>⚠️ Quá hạn</b><span class="muted">Kiểm tra phiếu quá hạn</span></button>
    <button onclick="pageById('cash')"><b>💰 Thu chi</b><span class="muted">Theo dõi dòng tiền</span></button>
  </div>

  <div class="section-title"><h2>Phiếu gần đây</h2></div>
  <div id="recent"></div>
</section>

<!-- PHIẾU -->
<section id="tickets" class="page">
  <div class="section-title">
    <h2>Phiếu cầm</h2>
    <button class="btn" onclick="openTicket()">+ Tạo phiếu</button>
  </div>
  <div class="toolbar">
    <input id="ticketSearch" placeholder="Tìm mã, tên, SĐT, tài sản..." oninput="renderTickets()">
  </div>
  <div id="ticketList"></div>
</section>

<!-- CHUỘC -->
<section id="redeem" class="page">
  <div class="section-title"><h2>💵 Chuộc đồ</h2></div>
  <div class="notice">
    Nhập mã phiếu, tên khách hoặc tài sản để tìm. Tiền lãi được tính theo số ngày thực tế.
  </div>
  <div class="toolbar">
    <input id="redeemSearch" placeholder="VD: CD-0001 hoặc tên khách..." oninput="renderRedeem()">
  </div>
  <div id="redeemList"></div>
</section>

<!-- KHÁCH -->
<section id="customers" class="page">
  <div class="section-title"><h2>👤 Khách hàng</h2></div>
  <div class="toolbar">
    <input id="customerSearch" placeholder="Tìm tên, SĐT, CCCD..." oninput="renderCustomers()">
  </div>
  <div id="customerList"></div>
</section>

<!-- TÀI SẢN -->
<section id="assets" class="page">
  <div class="section-title"><h2>📦 Tài sản đang giữ</h2></div>
  <div id="assetList"></div>
</section>

<!-- QUÁ HẠN -->
<section id="overdue" class="page">
  <div class="section-title">
    <h2>⚠️ Phiếu quá hạn</h2>
  </div>
  <div id="overdueList"></div>
</section>

<!-- THANH LÝ -->
<section id="liquidation" class="page">
  <div class="section-title"><h2>🏷️ Thanh lý</h2></div>
  <div class="notice">
    Chỉ thanh lý những phiếu đang quá hạn. Nhập giá bán và chi phí thanh lý để tính lãi/lỗ.
  </div>
  <div id="liquidationList"></div>
</section>

<!-- THU CHI -->
<section id="cash" class="page">
  <div class="section-title">
    <h2>💰 Thu chi</h2>
    <button class="btn" onclick="openCash()">+ Ghi thu/chi</button>
  </div>

  <div class="grid">
    <div class="card stat"><span>Tổng thu</span><b id="cashIn">0đ</b></div>
    <div class="card stat"><span>Tổng chi</span><b id="cashOut">0đ</b></div>
    <div class="card stat"><span>Chênh lệch</span><b id="cashBalance">0đ</b></div>
    <div class="card stat"><span>Số giao dịch</span><b id="cashCount">0</b></div>
  </div>

  <div id="cashList"></div>
</section>

<!-- BÁO CÁO -->
<section id="reports" class="page">
  <div class="section-title"><h2>📊 Báo cáo</h2></div>

  <div class="grid">
    <div class="card">
      <span class="muted">Tổng tiền đã giải ngân</span>
      <div class="money" id="rLoan">0đ</div>
    </div>
    <div class="card">
      <span class="muted">Tiền lãi đã thu</span>
      <div class="money" id="rInterest">0đ</div>
    </div>
    <div class="card">
      <span class="muted">Lãi thanh lý</span>
      <div class="money" id="rLiquid">0đ</div>
    </div>
    <div class="card">
      <span class="muted">Vốn đang nằm</span>
      <div class="money" id="rCapital">0đ</div>
    </div>
  </div>

  <div class="card">
    <h3>📌 Thống kê phiếu</h3>
    <div class="row"><span>Tổng số phiếu</span><b id="rTotal">0</b></div>
    <div class="row"><span>Đang cầm</span><b id="rActive">0</b></div>
    <div class="row"><span>Quá hạn</span><b id="rOverdue">0</b></div>
    <div class="row"><span>Đã chuộc</span><b id="rRedeemed">0</b></div>
    <div class="row"><span>Đã thanh lý</span><b id="rLiquidated">0</b></div>
  </div>

  <div class="section-title"><h2>Sao lưu dữ liệu</h2></div>
  <div class="card">
    <p class="muted">
      Dữ liệu được lưu trên trình duyệt của máy này. Nên sao lưu định kỳ để tránh mất dữ liệu.
    </p>
    <div class="actions">
      <button class="btn blue" onclick="backup()">⬇️ Sao lưu</button>
      <button class="btn gray" onclick="document.getElementById('restoreFile').click()">⬆️ Khôi phục</button>
      <input id="restoreFile" type="file" accept=".json" style="display:none" onchange="restore(event)">
    </div>
  </div>
</section>

</main>

<!-- MODAL TẠO PHIẾU -->
<div id="ticketModal" class="modal">
<div class="modal-box">
  <div class="modal-head">
    <h2>➕ Tạo phiếu cầm</h2>
    <button class="close" onclick="closeModal('ticketModal')">✕</button>
  </div>

  <form onsubmit="createTicket(event)">
    <div class="form-grid">

      <div class="form-group">
        <label>Tên khách hàng *</label>
        <input id="fName" required>
      </div>

      <div class="form-group">
        <label>Số điện thoại</label>
        <input id="fPhone" inputmode="tel">
      </div>

      <div class="form-group">
        <label>CCCD</label>
        <input id="fId">
      </div>

      <div class="form-group">
        <label>Loại tài sản *</label>
        <select id="fType" required>
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

      <div class="form-group full">
        <label>Mô tả tài sản *</label>
        <input id="fAsset" placeholder="VD: iPhone 17 Pro Max 256GB..." required>
      </div>

      <div class="form-group">
        <label>Giá định giá</label>
        <input id="fValue" type="number" min="0" step="1000" placeholder="0">
      </div>

      <div class="form-group">
        <label>Số tiền cầm *</label>
        <input id="fLoan" type="number" min="0" step="1000" required placeholder="0">
      </div>

      <div class="form-group">
        <label>Lãi suất / tháng (%)</label>
        <input id="fRate" type="number" min="0" step="0.01" value="3">
      </div>

      <div class="form-group">
        <label>Ngày cầm *</label>
        <input id="fDate" type="date" required>
      </div>

      <div class="form-group">
        <label>Thời hạn (ngày)</label>
        <input id="fDays" type="number" min="1" value="30">
      </div>

      <div class="form-group full">
        <label>Ghi chú</label>
        <textarea id="fNote" rows="3"></textarea>
      </div>

    </div>

    <div class="actions">
      <button class="btn gray" type="button" onclick="closeModal('ticketModal')">Hủy</button>
      <button class="btn green" type="submit">Lưu phiếu</button>
    </div>
  </form>
</div>
</div>

<!-- MODAL GIA HẠN -->
<div id="extendModal" class="modal">
<div class="modal-box">
  <div class="modal-head">
    <h2>🔄 Gia hạn phiếu</h2>
    <button class="close" onclick="closeModal('extendModal')">✕</button>
  </div>

  <form onsubmit="extendTicket(event)">
    <input type="hidden" id="extendId">
    <p id="extendInfo"></p>

    <label>Gia hạn thêm bao nhiêu ngày?</label>
    <input id="extendDays" type="number" min="1" value="30" required>

    <div class="actions">
      <button class="btn gray" type="button" onclick="closeModal('extendModal')">Hủy</button>
      <button class="btn blue" type="submit">Gia hạn</button>
    </div>
  </form>
</div>
</div>

<!-- MODAL THANH LÝ -->
<div id="liquidModal" class="modal">
<div class="modal-box">
  <div class="modal-head">
    <h2>🏷️ Thanh lý tài sản</h2>
    <button class="close" onclick="closeModal('liquidModal')">✕</button>
  </div>

  <form onsubmit="liquidate(event)">
    <input type="hidden" id="liquidId">

    <div id="liquidInfo"></div>

    <label>Giá bán thanh lý *</label>
    <input id="salePrice" type="number" min="0" step="1000" required>

    <br>

    <label>Chi phí thanh lý</label>
    <input id="liquidCost" type="number" min="0" step="1000" value="0">

    <div id="liquidPreview" class="notice" style="margin-top:12px"></div>

    <div class="actions">
      <button class="btn gray" type="button" onclick="closeModal('liquidModal')">Hủy</button>
      <button class="btn orange" type="submit">Xác nhận thanh lý</button>
    </div>
  </form>
</div>
</div>

<!-- MODAL THU CHI -->
<div id="cashModal" class="modal">
<div class="modal-box">
  <div class="modal-head">
    <h2>💰 Ghi thu / chi</h2>
    <button class="close" onclick="closeModal('cashModal')">✕</button>
  </div>

  <form onsubmit="createCash(event)">
    <div class="form-grid">

      <div>
        <label>Loại</label>
        <select id="cType">
          <option value="in">Thu</option>
          <option value="out">Chi</option>
        </select>
      </div>

      <div>
        <label>Số tiền *</label>
        <input id="cAmount" type="number" min="0" step="1000" required>
      </div>

      <div>
        <label>Danh mục</label>
        <input id="cCategory" placeholder="VD: Tiền điện, tiền thuê...">
      </div>

      <div>
        <label>Ngày</label>
        <input id="cDate" type="date" required>
      </div>

      <div class="full">
        <label>Ghi chú</label>
        <textarea id="cNote" rows="3"></textarea>
      </div>

    </div>

    <div class="actions">
      <button class="btn gray" type="button" onclick="closeModal('cashModal')">Hủy</button>
      <button class="btn green" type="submit">Lưu</button>
    </div>
  </form>
</div>
</div>

<script>
const KEY="campro_data_v2";

let data=JSON.parse(localStorage.getItem(KEY)||'null') || {
  tickets:[],
  cash:[]
};

function save(){
  localStorage.setItem(KEY,JSON.stringify(data));
  renderAll();
}

function money(n){
  return Number(n||0).toLocaleString("vi-VN")+"đ";
}

function today(){
  return new Date().toISOString().slice(0,10);
}

function dateVN(d){
  if(!d)return "";
  const x=new Date(d+"T00:00:00");
  return x.toLocaleDateString("vi-VN");
}

function addDays(date,days){
  const d=new Date(date+"T00:00:00");
  d.setDate(d.getDate()+Number(days));
  return d.toISOString().slice(0,10);
}

function daysBetween(a,b){
  const x=new Date(a+"T00:00:00");
  const y=new Date(b+"T00:00:00");
  return Math.max(0,Math.ceil((y-x)/86400000));
}

function interest(t,end=today()){
  const days=daysBetween(t.pawnDate,end);
  return t.loan*(Number(t.rate||0)/100)*(days/30);
}

function status(t){
  if(t.status==="redeemed")return "redeemed";
  if(t.status==="liquidated")return "liquidated";
  if(t.dueDate<today())return "overdue";
  return "active";
}

function statusText(t){
  const s=status(t);
  if(s==="active")return '<span class="badge">Đang cầm</span>';
  if(s==="overdue")return '<span class="badge red">Quá hạn</span>';
  if(s==="redeemed")return '<span class="badge gray">Đã chuộc</span>';
  return '<span class="badge orange">Đã thanh lý</span>';
}

function page(id,btn){
  document.querySelectorAll(".page").forEach(x=>x.classList.remove("active"));
  document.getElementById(id).classList.add("active");

  document.querySelectorAll(".nav button").forEach(x=>x.classList.remove("active"));
  if(btn)btn.classList.add("active");

  renderAll();
  window.scrollTo(0,0);
}

function pageById(id){
  const btn=[...document.querySelectorAll(".nav button")]
    .find(x=>x.getAttribute("onclick")?.includes("'"+id+"'"));
  page(id,btn);
}

function openModal(id){
  document.getElementById(id).classList.add("show");
}
function closeModal(id){
  document.getElementById(id).classList.remove("show");
}

function openTicket(){
  document.getElementById("fDate").value=today();
  openModal("ticketModal");
}

function nextCode(){
  let max=0;
  data.tickets.forEach(t=>{
    const n=parseInt(String(t.id).replace(/\D/g,""))||0;
    if(n>max)max=n;
  });
  return "CD-"+String(max+1).padStart(4,"0");
}

function createTicket(e){
  e.preventDefault();

  const loan=Number(document.getElementById("fLoan").value||0);
  const days=Number(document.getElementById("fDays").value||30);
  const pawnDate=document.getElementById("fDate").value;

  const t={
    id:nextCode(),
    customer:{
      name:document.getElementById("fName").value.trim(),
      phone:document.getElementById("fPhone").value.trim(),
      cccd:document.getElementById("fId").value.trim()
    },
    asset:{
      type:document.getElementById("fType").value,
      desc:document.getElementById("fAsset").value.trim(),
      value:Number(document.getElementById("fValue").value||0)
    },
    loan,
    rate:Number(document.getElementById("fRate").value||0),
    pawnDate,
    termDays:days,
    dueDate:addDays(pawnDate,days),
    note:document.getElementById("fNote").value.trim(),
    status:"active",
    redeemedDate:null,
    redeemedTotal:0,
    extensionDays:0,
    liquidation:null,
    createdAt:new Date().toISOString()
  };

  data.tickets.unshift(t);

  data.cash.unshift({
    id:Date.now(),
    date:pawnDate,
    type:"out",
    category:"Giải ngân cầm đồ",
    amount:loan,
    ref:t.id,
    note:"Giải ngân cho "+t.customer.name
  });

  save();
  closeModal("ticketModal");
  e.target.reset();

  alert("Đã tạo phiếu "+t.id);
}

function ticketCard(t,buttons=true){
  const s=status(t);
  return `
  <div class="ticket ${s}">
    <div class="ticket-head">
      <div>
        <div class="ticket-code">${t.id}</div>
        <b>${esc(t.customer.name)}</b>
        <div class="muted">${esc(t.customer.phone||"")} ${t.customer.cccd?"• CCCD "+esc(t.customer.cccd):""}</div>
      </div>
      <div>${statusText(t)}</div>
    </div>

    <div class="row">
      <span>📦 ${esc(t.asset.type)} - ${esc(t.asset.desc)}</span>
    </div>

    <div class="row">
      <span>Tiền cầm</span>
      <b>${money(t.loan)}</b>
    </div>

    <div class="row">
      <span>Ngày cầm</span>
      <span>${dateVN(t.pawnDate)}</span>
    </div>

    <div class="row">
      <span>Đến hạn</span>
      <span>${dateVN(t.dueDate)}</span>
    </div>

    ${s==="overdue"?`
      <div class="row">
        <span>Quá hạn</span>
        <b style="color:#dc2626">${daysBetween(t.dueDate,today())} ngày</b>
      </div>
    `:""}

    ${buttons?`
    <div class="actions">
      ${s==="active"||s==="overdue"?`
        <button class="btn green" onclick="redeem('${t.id}')">💵 Chuộc</button>
        <button class="btn blue" onclick="openExtend('${t.id}')">🔄 Gia hạn</button>
      `:""}

      ${s==="overdue"?`
        <button class="btn orange" onclick="openLiquid('${t.id}')">🏷️ Thanh lý</button>
      `:""}

      <button class="btn gray" onclick="printTicket('${t.id}')">🖨️ In</button>
    </div>
    `:""}
  </div>`;
}

function renderTickets(){
  const q=(document.getElementById("ticketSearch")?.value||"").toLowerCase();
  let arr=data.tickets.filter(t=>
    [t.id,t.customer.name,t.customer.phone,t.customer.cccd,t.asset.type,t.asset.desc]
    .join(" ").toLowerCase().includes(q)
  );

  document.getElementById("ticketList").innerHTML=
    arr.length?arr.map(t=>ticketCard(t)).join(""):`<div class="empty">Chưa có phiếu nào.</div>`;
}

function renderRedeem(){
  const q=(document.getElementById("redeemSearch")?.value||"").toLowerCase();

  const arr=data.tickets.filter(t=>{
    const s=status(t);
    return (s==="active"||s==="overdue") &&
      [t.id,t.customer.name,t.customer.phone,t.asset.desc]
      .join(" ").toLowerCase().includes(q);
  });

  document.getElementById("redeemList").innerHTML=arr.length?
    arr.map(t=>{
      const days=daysBetween(t.pawnDate,today());
      const it=interest(t);
      const total=t.loan+it;

      return `
      <div class="ticket ${status(t)}">
        <div class="ticket-head">
          <div><b>${t.id}</b><br>${esc(t.customer.name)}</div>
          ${statusText(t)}
        </div>
        <div class="row"><span>Tiền cầm</span><b>${money(t.loan)}</b></div>
        <div class="row"><span>Số ngày tính lãi</span><b>${days} ngày</b></div>
        <div class="row"><span>Lãi tạm tính</span><b>${money(it)}</b></div>
        <div class="row"><span>Tổng tiền chuộc</span><b style="font-size:20px">${money(total)}</b></div>
        <div class="actions">
          <button class="btn green" onclick="redeem('${t.id}')">Xác nhận chuộc</button>
        </div>
      </div>`;
    }).join("")
    :`<div class="empty">Không tìm thấy phiếu đang cầm.</div>`;
}

function redeem(id){
  const t=data.tickets.find(x=>x.id===id);
  if(!t)return;

  const it=interest(t);
  const total=t.loan+it;

  if(!confirm(
    "Phiếu "+t.id+
    "\\nSố ngày: "+daysBetween(t.pawnDate,today())+
    "\\nTiền cầm: "+money(t.loan)+
    "\\nTiền lãi: "+money(it)+
    "\\nTỔNG CHUỘC: "+money(total)+
    "\\n\\nXác nhận khách đã chuộc?"
  ))return;

  t.status="redeemed";
  t.redeemedDate=today();
  t.redeemedTotal=total;

  data.cash.unshift({
    id:Date.now(),
    date:today(),
    type:"in",
    category:"Tiền chuộc",
    amount:total,
    ref:t.id,
    note:"Khách "+t.customer.name+" chuộc đồ"
  });

  save();
  alert("Đã đóng phiếu "+t.id);
}

function openExtend(id){
  const t=data.tickets.find(x=>x.id===id);
  if(!t)return;

  document.getElementById("extendId").value=id;
  document.getElementById("extendInfo").innerHTML=
    `<b>${t.id}</b> - Hạn hiện tại: <b>${dateVN(t.dueDate)}</b>`;

  openModal("extendModal");
}

function extendTicket(e){
  e.preventDefault();

  const id=document.getElementById("extendId").value;
  const days=Number(document.getElementById("extendDays").value||0);
  const t=data.tickets.find(x=>x.id===id);

  if(!t||days<=0)return;

  t.dueDate=addDays(t.dueDate,days);
  t.extensionDays=(t.extensionDays||0)+days;

  save();
  closeModal("extendModal");

  alert("Đã gia hạn thêm "+days+" ngày.");
}

function renderOverdue(){
  const arr=data.tickets.filter(t=>status(t)==="overdue");

  document.getElementById("overdueList").innerHTML=
    arr.length?arr.map(t=>ticketCard(t)).join("")
    :`<div class="empty">🎉 Không có phiếu quá hạn.</div>`;
}

function openLiquid(id){
  const t=data.tickets.find(x=>x.id===id);
  if(!t)return;

  document.getElementById("liquidId").value=id;
  document.getElementById("salePrice").value="";
  document.getElementById("liquidCost").value=0;

  document.getElementById("liquidInfo").innerHTML=
    `<div class="notice">
      <b>${t.id}</b> - ${esc(t.asset.desc)}<br>
      Tiền cầm: <b>${money(t.loan)}</b>
    </div>`;

  updateLiquidPreview();
  openModal("liquidModal");
}

document.getElementById("salePrice").addEventListener("input",updateLiquidPreview);
document.getElementById("liquidCost").addEventListener("input",updateLiquidPreview);

function updateLiquidPreview(){
  const id=document.getElementById("liquidId").value;
  const t=data.tickets.find(x=>x.id===id);
  if(!t)return;

  const sale=Number(document.getElementById("salePrice").value||0);
  const cost=Number(document.getElementById("liquidCost").value||0);
  const profit=sale-t.loan-cost;

  document.getElementById("liquidPreview").innerHTML=
    `Lãi/lỗ thanh lý: <b>${money(profit)}</b>`;
}

function liquidate(e){
  e.preventDefault();

  const id=document.getElementById("liquidId").value;
  const t=data.tickets.find(x=>x.id===id);

  if(!t)return;

  const sale=Number(document.getElementById("salePrice").value||0);
  const cost=Number(document.getElementById("liquidCost").value||0);
  const profit=sale-t.loan-cost;

  if(!confirm(
    "Xác nhận thanh lý "+t.id+
    "?\\nGiá bán: "+money(sale)+
    "\\nChi phí: "+money(cost)+
    "\\nLãi/lỗ: "+money(profit)
  ))return;

  t.status="liquidated";
  t.liquidation={
    date:today(),
    salePrice:sale,
    cost,
    profit
  };

  data.cash.unshift({
    id:Date.now(),
    date:today(),
    type:"in",
    category:"Tiền bán thanh lý",
    amount:sale,
    ref:t.id,
    note:"Thanh lý "+t.asset.desc
  });

  if(cost>0){
    data.cash.unshift({
      id:Date.now()+1,
      date:today(),
      type:"out",
      category:"Chi phí thanh lý",
      amount:cost,
      ref:t.id,
      note:"Chi phí thanh lý"
    });
  }

  save();
  closeModal("liquidModal");

  alert("Đã thanh lý "+t.id);
}

function renderLiquidation(){
  const arr=data.tickets.filter(t=>status(t)==="overdue");

  document.getElementById("liquidationList").innerHTML=
    arr.length?arr.map(t=>`
      <div class="ticket overdue">
        <div class="ticket-head">
          <div>
            <b>${t.id}</b><br>
            ${esc(t.customer.name)}
          </div>
          <span class="badge red">Quá hạn</span>
        </div>

        <div class="row">
          <span>Tài sản</span>
          <b>${esc(t.asset.desc)}</b>
        </div>

        <div class="row">
          <span>Tiền cầm</span>
          <b>${money(t.loan)}</b>
        </div>

        <div class="actions">
          <button class="btn orange" onclick="openLiquid('${t.id}')">
            🏷️ Thanh lý
          </button>
        </div>
      </div>
    `).join("")
    :`<div class="empty">Không có tài sản chờ thanh lý.</div>`;
}

function getCustomers(){
  const map={};

  data.tickets.forEach(t=>{
    const key=t.customer.phone||t.customer.cccd||t.customer.name.toLowerCase();

    if(!map[key]){
      map[key]={
        name:t.customer.name,
        phone:t.customer.phone,
        cccd:t.customer.cccd,
        count:0,
        active:0,
        loan:0
      };
    }

    map[key].count++;

    if(status(t)==="active"||status(t)==="overdue"){
      map[key].active++;
      map[key].loan+=t.loan;
    }
  });

  return Object.values(map);
}

function renderCustomers(){
  const q=(document.getElementById("customerSearch")?.value||"").toLowerCase();

  const arr=getCustomers().filter(c=>
    [c.name,c.phone,c.cccd].join(" ").toLowerCase().includes(q)
  );

  document.getElementById("customerList").innerHTML=arr.length?
    arr.map(c=>`
      <div class="card" style="margin-bottom:10px">
        <b>👤 ${esc(c.name)}</b>
        <div class="muted">${esc(c.phone||"")} ${c.cccd?"• CCCD "+esc(c.cccd):""}</div>
        <div class="row"><span>Tổng số phiếu</span><b>${c.count}</b></div>
        <div class="row"><span>Đang cầm</span><b>${c.active}</b></div>
        <div class="row"><span>Vốn đang cầm</span><b>${money(c.loan)}</b></div>
      </div>
    `).join("")
    :`<div class="empty">Chưa có khách hàng.</div>`;
}

function renderAssets(){
  const arr=data.tickets.filter(t=>{
    const s=status(t);
    return s==="active"||s==="overdue";
  });

  document.getElementById("assetList").innerHTML=arr.length?
    arr.map(t=>`
      <div class="card" style="margin-bottom:10px">
        <div class="ticket-head">
          <div>
            <b>📦 ${esc(t.asset.desc)}</b>
            <div class="muted">${esc(t.asset.type)}</div>
          </div>
          ${statusText(t)}
        </div>
        <div class="row"><span>Khách</span><b>${esc(t.customer.name)}</b></div>
        <div class="row"><span>Giá định giá</span><b>${money(t.asset.value)}</b></div>
        <div class="row"><span>Tiền cầm</span><b>${money(t.loan)}</b></div>
        <div class="row"><span>Đến hạn</span><b>${dateVN(t.dueDate)}</b></div>
      </div>
    `).join("")
    :`<div class="empty">Không có tài sản đang giữ.</div>`;
}

function openCash(){
  document.getElementById("cDate").value=today();
  openModal("cashModal");
}

function createCash(e){
  e.preventDefault();

  data.cash.unshift({
    id:Date.now(),
    date:document.getElementById("cDate").value,
    type:document.getElementById("cType").value,
    category:document.getElementById("cCategory").value||"Khác",
    amount:Number(document.getElementById("cAmount").value||0),
    ref:"",
    note:document.getElementById("cNote").value
  });

  save();
  closeModal("cashModal");
  e.target.reset();

  alert("Đã ghi giao dịch.");
}

function renderCash(){
  let inTotal=0,outTotal=0;

  data.cash.forEach(x=>{
    if(x.type==="in")inTotal+=Number(x.amount);
    else outTotal+=Number(x.amount);
  });

  document.getElementById("cashIn").textContent=money(inTotal);
  document.getElementById("cashOut").textContent=money(outTotal);
  document.getElementById("cashBalance").textContent=money(inTotal-outTotal);
  document.getElementById("cashCount").textContent=data.cash.length;

  document.getElementById("cashList").innerHTML=data.cash.length?
    data.cash.map(x=>`
      <div class="card" style="margin-bottom:8px">
        <div class="ticket-head">
          <div>
            <b>${x.type==="in"?"🟢 Thu":"🔴 Chi"} - ${esc(x.category)}</b>
            <div class="muted">${dateVN(x.date)} ${x.ref?"• "+x.ref:""}</div>
          </div>
          <b>${x.type==="in"?"+":"-"}${money(x.amount)}</b>
        </div>
        ${x.note?`<div class="muted" style="margin-top:5px">${esc(x.note)}</div>`:""}
      </div>
    `).join("")
    :`<div class="empty">Chưa có giao dịch.</div>`;
}

function renderReports(){
  let loan=0,interestTotal=0,liquid=0,capital=0;

  data.tickets.forEach(t=>{
    loan+=t.loan;

    if(t.status==="redeemed"){
      interestTotal+=Math.max(0,(t.redeemedTotal||0)-t.loan);
    }

    if(t.status==="liquidated"){
      liquid+=Number(t.liquidation?.profit||0);
    }

    const s=status(t);
    if(s==="active"||s==="overdue")capital+=t.loan;
  });

  const active=data.tickets.filter(t=>status(t)==="active").length;
  const overdue=data.tickets.filter(t=>status(t)==="overdue").length;
  const redeemed=data.tickets.filter(t=>t.status==="redeemed").length;
  const liquidated=data.tickets.filter(t=>t.status==="liquidated").length;

  document.getElementById("rLoan").textContent=money(loan);
  document.getElementById("rInterest").textContent=money(interestTotal);
  document.getElementById("rLiquid").textContent=money(liquid);
  document.getElementById("rCapital").textContent=money(capital);

  document.getElementById("rTotal").textContent=data.tickets.length;
  document.getElementById("rActive").textContent=active;
  document.getElementById("rOverdue").textContent=overdue;
  document.getElementById("rRedeemed").textContent=redeemed;
  document.getElementById("rLiquidated").textContent=liquidated;
}

function renderHome(){
  const active=data.tickets.filter(t=>{
    const s=status(t);return s==="active"||s==="overdue";
  });

  const overdue=data.tickets.filter(t=>status(t)==="overdue").length;
  const redeemed=data.tickets.filter(t=>t.status==="redeemed").length;

  let capital=0,interestTotal=0,liquidProfit=0;

  active.forEach(t=>capital+=t.loan);

  data.tickets.forEach(t=>{
    if(t.status==="redeemed")
      interestTotal+=Math.max(0,t.redeemedTotal-t.loan);

    if(t.status==="liquidated")
      liquidProfit+=Number(t.liquidation?.profit||0);
  });

  document.getElementById("sActive").textContent=active.length;
  document.getElementById("sCapital").textContent=money(capital);
  document.getElementById("sOverdue").textContent=overdue;
  document.getElementById("sRedeemed").textContent=redeemed;
  document.getElementById("sInterest").textContent=money(interestTotal);
  document.getElementById("sLiquidProfit").textContent=money(liquidProfit);
  document.getElementById("sCustomers").textContent=getCustomers().length;
  document.getElementById("sAssets").textContent=active.length;

  const recent=data.tickets.slice(0,5);

  document.getElementById("recent").innerHTML=recent.length?
    recent.map(t=>ticketCard(t)).join("")
    :`<div class="empty">Chưa có phiếu. Bấm "+ Phiếu cầm" để bắt đầu.</div>`;
}

function backup(){
  const blob=new Blob([JSON.stringify(data,null,2)],{type:"application/json"});
  const a=document.createElement("a");
  a.href=URL.createObjectURL(blob);
  a.download="CamPro-backup-"+today()+".json";
  a.click();
  URL.revokeObjectURL(a.href);
}

function restore(e){
  const file=e.target.files[0];
  if(!file)return;

  const reader=new FileReader();

  reader.onload=function(){
    try{
      const x=JSON.parse(reader.result);

      if(!x.tickets||!x.cash)throw new Error();

      if(confirm("Khôi phục dữ liệu sẽ thay dữ liệu hiện tại. Tiếp tục?")){
        data=x;
        save();
        alert("Khôi phục thành công.");
      }
    }catch(err){
      alert("File sao lưu không hợp lệ.");
    }
  };

  reader.readAsText(file);
}

function printTicket(id){
  const t=data.tickets.find(x=>x.id===id);
  if(!t)return;

  const it=interest(t);
  const total=t.loan+it;

  const w=window.open("","_blank");

  w.document.write(`
  <html>
  <head>
  <title>${t.id}</title>
  <style>
  body{font-family:Arial;padding:25px;max-width:600px;margin:auto}
  h1{text-align:center}
  .line{border-bottom:1px solid #ddd;padding:8px 0}
  .total{font-size:22px;font-weight:bold}
  </style>
  </head>
  <body>
  <h1>💰 CẦMPRO</h1>
  <h3>PHIẾU CẦM ĐỒ</h3>

  <div class="line"><b>Mã phiếu:</b> ${t.id}</div>
  <div class="line"><b>Khách hàng:</b> ${esc(t.customer.name)}</div>
  <div class="line"><b>SĐT:</b> ${esc(t.customer.phone||"")}</div>
  <div class="line"><b>CCCD:</b> ${esc(t.customer.cccd||"")}</div>
  <div class="line"><b>Tài sản:</b> ${esc(t.asset.type)} - ${esc(t.asset.desc)}</div>
  <div class="line"><b>Giá định giá:</b> ${money(t.asset.value)}</div>
  <div class="line"><b>Tiền cầm:</b> ${money(t.loan)}</div>
  <div class="line"><b>Lãi suất:</b> ${t.rate}%/tháng</div>
  <div class="line"><b>Ngày cầm:</b> ${dateVN(t.pawnDate)}</div>
  <div class="line"><b>Ngày đến hạn:</b> ${dateVN(t.dueDate)}</div>
  <div class="line"><b>Lãi tạm tính:</b> ${money(it)}</div>
  <div class="line total">Tạm tính chuộc: ${money(total)}</div>

  <br><br>
  <div>Khách hàng ký: __________________</div>
  <br>
  <div>Cửa hàng ký: ____________________</div>

  <script>window.print()<\/script>
  </body>
  </html>
  `);

  w.document.close();
}

function esc(x){
  return String(x??"")
    .replace(/&/g,"&amp;")
    .replace(/</g,"&lt;")
    .replace(/>/g,"&gt;")
    .replace(/"/g,"&quot;");
}

function renderAll(){
  renderHome();
  renderTickets();
  renderRedeem();
  renderCustomers();
  renderAssets();
  renderOverdue();
  renderLiquidation();
  renderCash();
  renderReports();
}

document.getElementById("fDate").value=today();
document.getElementById("cDate").value=today();

renderAll();
</script>

</body>
</html>
