# rawroot
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NeonTrack | Expense Dashboard</title>
<style>
:root {
  --bg: #050816;
  --panel: #0b1330;
  --border: #1b2b59;
  --blue: #19baff;
  --purple: #9b5cff;
  --text: #f0f5ff;
  --muted: #92a5d4;
}
* { box-sizing: border-box; }
body {
  margin: 0;
  background: radial-gradient(ellipse at top right,#151044,var(--bg) 55%);
  color: var(--text);
  font-family: Inter,system-ui,Arial,sans-serif;
}
.app { display: flex; min-height: 100vh; }
aside {
  width: 225px;
  padding: 28px 18px;
  border-right: 1px solid var(--border);
  background: #080d22;
}
.logo { font-size: 24px; font-weight: 800; }
.logo span { color: var(--blue); }
.sub { color: var(--muted); font-size: 12px; margin: 5px 0 35px; }
nav button {
  display: block; width: 100%; text-align: left;
  margin: 8px 0; padding: 13px;
  background: transparent; color: #b9c9f3;
  border: 1px solid transparent; border-radius: 12px;
  cursor: pointer; font-size: 14px;
}
nav button:hover,nav button.active {
  color: white; background: #132354;
  border-color: #2864ff; box-shadow: 0 0 18px #1353ff28;
}
main { flex: 1; min-width: 0; padding: 30px; }
header { display:flex; justify-content:space-between; gap:12px; align-items:center; margin-bottom:25px; }
h1 { margin:0; font-size: clamp(23px,3vw,32px); }
p { color:var(--muted); }
header p { margin:8px 0 0; }
.date { color:#b5c7ef; font-size:13px; }
.cards { display:grid; grid-template-columns:repeat(3,minmax(0,1fr)); gap:16px; }
.card,.panel {
  background:linear-gradient(140deg,#101a3c,#080f25);
  border:1px solid var(--border); border-radius:17px;
  padding:21px; box-shadow:0 10px 35px #0002;
}
.card { transition:.25s; }
.card:hover { transform:translateY(-4px); border-color:var(--blue); }
.card.blue { border-color:#126bc2; }
.card.purple { border-color:#7940cb; }
.card.green { border-color:#087e81; }
.card-label { color:#a9bbe8; font-size:13px; }
.amount { font-size:clamp(20px,2.5vw,29px); font-weight:750; margin:12px 0; overflow-wrap:anywhere; }
.change { color:#28e6b2; font-size:12px; }
.content { display:grid; grid-template-columns:1.1fr 1fr; gap:17px; margin-top:18px; }
h2 { font-size:17px; margin:0 0 22px; }
.chart-area { display:flex; align-items:center; justify-content:center; gap:24px; flex-wrap:wrap; }
.donut {
  width:160px; height:160px; border-radius:50%;
  background:conic-gradient(#c84cf7 0 35%,#1e9dff 35% 60%,#28dfae 60% 75%,#ffc845 75% 90%,#7855ff 90%);
  display:grid; place-items:center; flex-shrink:0;
  box-shadow:0 0 28px #865aff22;
}
.donut-inner {
  width:105px; height:105px; border-radius:50%;
  background:#0b1330; display:grid; place-content:center; text-align:center;
}
.donut-inner strong { font-size:19px; }
.donut-inner small { color:var(--muted); margin-top:5px; }
.legend { display:grid; gap:14px; font-size:12px; min-width:125px; }
.legend div { display:flex; gap:8px; align-items:center; justify-content:space-between; }
.dot { width:9px; height:9px; border-radius:50%; display:inline-block; margin-right:4px; }
.transactions { display:grid; gap:0; }
.transaction {
  display:flex; justify-content:space-between; gap:12px;
  padding:15px 0; border-bottom:1px solid #1b2b50;
}
.transaction:last-child { border-bottom:0; }
.tx-name { font-size:13px; font-weight:650; }
.tx-detail { font-size:11px; color:var(--muted); margin-top:5px; }
.tx-right { text-align:right; font-size:13px; }
.tx-right strong { color:#39e8c1; }
.view { display:none; }
.view.active { display:block; }
.action {
  background:linear-gradient(100deg,#148eff,#873cff);
  border:0; border-radius:10px; padding:12px 18px;
  color:white; font-weight:700; cursor:pointer;
}
.action:hover { filter:brightness(1.15); }
input,select {
  width:100%; padding:13px; margin:8px 0 17px;
  border:1px solid #263c72; background:#080f25;
  color:white; border-radius:9px; outline:none;
}
input:focus,select:focus { border-color:var(--blue); }
label { color:#b8c8eb; font-size:13px; }
.form-panel { max-width:540px; }
.search { max-width:350px; }
footer { color:#8296c7; text-align:center; font-size:12px; padding:25px 5px 5px; }
@media(max-width:850px) {
  aside { width:175px; padding:20px 10px; }
  main { padding:20px 14px; }
  .cards { grid-template-columns:1fr; }
  .content { grid-template-columns:1fr; }
}
@media(max-width:520px) {
  .app { display:block; }
  aside { width:100%; padding:16px; border-right:0; border-bottom:1px solid var(--border); }
  .sub { margin-bottom:12px; }
  nav { display:flex; gap:5px; overflow-x:auto; }
  nav button { width:auto; white-space:nowrap; margin:0; padding:10px; }
  main { padding:20px 13px; }
  header { align-items:flex-start; }
  .date { font-size:11px; text-align:right; }
  .chart-area { gap:20px; }
  .donut { width:140px; height:140px; }
  .donut-inner { width:92px; height:92px; }
}
</style>
</head>
<body>
<div class="app">
  <aside>
    <div class="logo">◈ Neon<span>Track</span></div>
    <div class="sub">PERSONAL FINANCE SYSTEM</div>
    <nav>
      <button class="active" data-view="dashboard">⌂ &nbsp; Dashboard</button>
      <button data-view="add">＋ &nbsp; Add Expense</button>
      <button data-view="history">☷ &nbsp; View Expenses</button>
      <button data-view="summary">◉ &nbsp; Summary</button>
    </nav>
    <p style="margin-top:45px;font-size:12px">✦ Small steps,<br> &nbsp;&nbsp; bigger goals.</p>
  </aside>

  <main>
    <header>
      <div>
        <h1 id="heading">Welcome Back!</h1>
        <p id="subtitle">Track your expenses. Build a better tomorrow.</p>
      </div>
      <div class="date" id="today"></div>
    </header>

    <section class="view active" id="dashboard">
      <div class="cards">
        <div class="card blue">
          <div class="card-label">◉ &nbsp; Total Expenses</div>
          <div class="amount" id="total">₹ 4,850.00</div>
          <div class="change">↗ Stay in control</div>
        </div>
        <div class="card purple">
          <div class="card-label">▦ &nbsp; This Month</div>
          <div class="amount" id="monthly">₹ 4,850.00</div>
          <div class="change">↗ Your financial overview</div>
        </div>
        <div class="card green">
          <div class="card-label">☷ &nbsp; Transactions</div>
          <div class="amount" id="count">5</div>
          <div class="change">✓ Records tracked</div>
        </div>
      </div>

      <div class="content">
        <div class="panel">
          <h2>Spending by Category</h2>
          <div class="chart-area">
            <div class="donut" id="donut">
              <div class="donut-inner">
                <strong id="chartTotal">₹4,850</strong>
                <small>Total spent</small>
              </div>
            </div>
            <div class="legend" id="legend"></div>
          </div>
        </div>
        <div class="panel">
          <h2>Recent Expenses</h2>
          <div id="recent" class="transactions"></div>
        </div>
      </div>
    </section>

    <section class="view" id="add">
      <div class="panel form-panel">
        <h2>＋ Add New Expense</h2>
        <form id="expenseForm">
          <label>Expense Name</label>
          <input id="name" placeholder="e.g. Coffee" required>
          <label>Category</label>
          <select id="category">
            <option>Food</option><option>Transport</option>
            <option>Shopping</option><option>Bills</option>
            <option>Education</option><option>Other</option>
          </select>
          <label>Amount (₹)</label>
          <input id="amountInput" type="number" min="0.01" step="0.01" placeholder="e.g. 250" required>
          <label>Date</label>
          <input id="dateInput" type="date" required>
          <button class="action" type="submit">▣ &nbsp; Save Expense</button>
        </form>
      </div>
    </section>

    <section class="view" id="history">
      <div class="panel">
        <h2>Expense History</h2>
        <input class="search" id="search" placeholder="⌕ Search expenses...">
        <div id="allExpenses" class="transactions"></div>
      </div>
    </section>

    <section class="view" id="summary">
      <div class="panel">
        <h2>Your Financial Summary</h2>
        <div class="cards">
          <div><div class="card-label">Average Expense</div><div class="amount" id="average">₹970</div></div>
          <div><div class="card-label">Largest Expense</div><div class="amount" id="largest">₹700</div></div>
          <div><div class="card-label">Categories Used</div><div class="amount" id="categoryCount">5</div></div>
        </div>
        <h2 style="margin-top:30px">Category Breakdown</h2>
        <div id="summaryList" class="transactions"></div>
        <p>💡 Tip: Review your spending weekly and set a budget for each category.</p>
      </div>
    </section>

    <footer>NEONTRACK · SIMPLE · CLEAN · USEFUL<br><br>TRACK TODAY / GROW TOMORROW</footer>
  </main>
</div>

<script>
const initial = [
  {name:"Dinner",category:"Food",amount:450,date:"2026-10-25"},
  {name:"Bus Ticket",category:"Transport",amount:120,date:"2026-10-25"},
  {name:"Clothes",category:"Shopping",amount:700,date:"2026-10-24"},
  {name:"Lunch",category:"Food",amount:320,date:"2026-10-24"},
  {name:"Electricity",category:"Bills",amount:700,date:"2026-10-23"}
];

let expenses;
try {
  expenses = JSON.parse(localStorage.getItem("neontrack-expenses")) || initial;
} catch {
  expenses = initial;
}

const colors = ["#c84cf7","#1e9dff","#28dfae","#ffc845","#7855ff","#ff729d"];
const money = n => "₹" + Number(n).toLocaleString("en-IN",{maximumFractionDigits:2});
const dateText = d => new Date(d+"T12:00:00").toLocaleDateString("en-IN",{day:"2-digit",month:"short",year:"numeric"});

document.getElementById("today").textContent =
  new Date().toLocaleDateString("en-IN",{weekday:"short",day:"2-digit",month:"short",year:"numeric"});
document.getElementById("dateInput").value =
  new Date().toLocaleDateString("en-CA");

function save() {
  localStorage.setItem("neontrack-expenses",JSON.stringify(expenses));
}

function transactionHTML(e) {
  return `<div class="transaction">
    <div><div class="tx-name">${escapeHTML(e.name)}</div>
    <div class="tx-detail">${escapeHTML(e.category)} · ${dateText(e.date)}</div></div>
    <div class="tx-right"><strong>${money(e.amount)}</strong></div>
  </div>`;
}

function escapeHTML(s) {
  return String(s).replace(/[&<>"']/g,c=>({
    "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"
  })[c]);
}

function render() {
  const total = expenses.reduce((s,e)=>s+Number(e.amount),0);
  const now = new Date();
  const monthTotal = expenses.filter(e=>{
    const d = new Date(e.date+"T12:00:00");
    return d.getMonth()===now.getMonth() && d.getFullYear()===now.getFullYear();
  }).reduce((s,e)=>s+Number(e.amount),0);

  document.getElementById("total").textContent=money(total);
  document.getElementById("monthly").textContent=money(monthTotal);
  document.getElementById("count").textContent=expenses.length;
  document.getElementById("chartTotal").textContent=money(total);
  document.getElementById("average").textContent=money(expenses.length?total/expenses.length:0);
  document.getElementById("largest").textContent=money(Math.max(0,...expenses.map(e=>e.amount)));
  const groups={};
  expenses.forEach(e=>groups[e.category]=(groups[e.category]||0)+Number(e.amount));
  const entries=Object.entries(groups).sort((a,b)=>b[1]-a[1]);
  document.getElementById("categoryCount").textContent=entries.length;

  let cumulative=0;
  const segments=entries.map(([name,value],i)=>{
    const start=cumulative;
    cumulative+=total?value/total*100:0;
    return `${colors[i%colors.length]} ${start}% ${cumulative}%`;
  });
  document.getElementById("donut").style.background =
    total ? `conic-gradient(${segments.join(",")})` : "#263453";

  document.getElementById("legend").innerHTML=entries.map(([name,value],i)=>
    `<div><span><i class="dot" style="background:${colors[i%colors.length]}"></i>${escapeHTML(name)}</span><b>${total?(value/total*100).toFixed(0):0}%</b></div>`
  ).join("");

  document.getElementById("recent").innerHTML =
    [...expenses].slice(-5).reverse().map(transactionHTML).join("") || "No expenses yet";

  document.getElementById("allExpenses").innerHTML =
    [...expenses].reverse().map(transactionHTML).join("") || "No expenses yet";

  document.getElementById("summaryList").innerHTML=entries.map(([name,value],i)=>
    `<div class="transaction"><span>${escapeHTML(name)}</span><strong style="color:${colors[i%colors.length]}">${money(value)} · ${total?(value/total*100).toFixed(1):0}%</strong></div>`
  ).join("") || "Add your first expense to see a summary.";
}

document.querySelectorAll("nav button").forEach(button=>{
  button.addEventListener("click",()=>{
    document.querySelectorAll("nav button").forEach(b=>b.classList.remove("active"));
    document.querySelectorAll(".view").forEach(v=>v.classList.remove("active"));
    button.classList.add("active");
    document.getElementById(button.dataset.view).classList.add("active");
    const titles={
      dashboard:["Welcome Back!","Track your expenses. Build a better tomorrow."],
      add:["Add Expense","Record a new transaction."],
      history:["Expense History","Every transaction, in one place."],
      summary:["Spending Summary","Understand where your money goes."]
    };
    document.getElementById("heading").textContent=titles[button.dataset.view][0];
    document.getElementById("subtitle").textContent=titles[button.dataset.view][1];
  });
});

document.getElementById("expenseForm").addEventListener("submit",event=>{
  event.preventDefault();
  const name=document.getElementById("name").value.trim();
  const amount=Number(document.getElementById("amountInput").value);
  if(!name || !Number.isFinite(amount) || amount<=0) return;
  expenses.push({
    name,
    category:document.getElementById("category").value,
    amount,
    date:document.getElementById("dateInput").value
  });
  save();
  render();
  event.target.reset();
  document.getElementById("dateInput").value=new Date().toLocaleDateString("en-CA");
  document.querySelector('[data-view="dashboard"]').click();
});

document.getElementById("search").addEventListener("input",event=>{
  const q=event.target.value.toLowerCase();
  document.getElementById("allExpenses").innerHTML=
    [...expenses].reverse()
      .filter(e=>(e.name+" "+e.category).toLowerCase().includes(q))
      .map(transactionHTML).join("") || "No matching expenses";
});

render();
</script>
</body>
</html>
