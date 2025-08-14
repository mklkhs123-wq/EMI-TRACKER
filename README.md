# Emi-tracker
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Finance Master — Multi Calculator with Monthly Breakdown + News</title>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
  *{box-sizing:border-box}
  body{font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto; margin:0; background:#f3f6fb; color:#132; -webkit-font-smoothing:antialiased}
  header{background:linear-gradient(135deg,#4facfe,#00f2fe); color:#fff; padding:16px; text-align:center; font-weight:700; box-shadow:0 4px 14px rgba(0,0,0,0.08)}
  nav{display:flex; gap:6px; padding:10px; overflow-x:auto; background:#fff; border-bottom:1px solid #e6eef9}
  nav button{flex:0 0 auto; padding:10px 12px; border-radius:10px; border:none; background:#fff; color:#345; font-weight:600; cursor:pointer}
  nav button.active{background:#e8f6ff; color:#0b74d1; box-shadow:0 6px 18px rgba(11,116,209,0.08); transform:translateY(-2px)}
  main{max-width:980px;margin:18px auto;padding:0 14px}
  .card{background:#fff;border-radius:12px;padding:14px;margin-bottom:14px;box-shadow:0 6px 24px rgba(14,50,87,0.05);border:1px solid rgba(14,50,87,0.04)}
  h2{margin:0 0 8px;font-size:18px;color:#0b2f4a}
  label{display:block;font-size:13px;color:#356;border-radius:6px;margin-top:8px}
  input,select{width:100%;padding:10px;margin-top:6px;border-radius:8px;border:1px solid #e6eef9;background:#fbfdff}
  .row{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
  .btn{display:inline-block;background:linear-gradient(90deg,#4facfe,#00f2fe);color:#fff;padding:10px 12px;border-radius:10px;border:none;font-weight:700;cursor:pointer;margin-top:10px}
  .btn.sec{background:#f3f6fb;color:#0b2f4a;border:1px solid #e6eef9}
  .kpi{display:flex;gap:10px;margin-top:12px}
  .kpi .box{flex:1;background:linear-gradient(180deg,#fff,#fbfdff);padding:10px;border-radius:8px;text-align:center;border:1px solid #eef6ff}
  .kpi .label{font-size:12px;color:#6b7a86}
  .kpi .value{font-size:16px;font-weight:800;color:#123}
  table{width:100%;border-collapse:collapse;margin-top:10px;font-size:13px}
  th,td{padding:8px;border-bottom:1px dashed #e9f2fb;text-align:right}
  th:nth-child(1),td:nth-child(1){text-align:left}
  .table-wrap{max-height:300px;overflow:auto;border-radius:8px;padding-bottom:6px}
  .note{font-size:13px;color:#5b6b7a;margin-top:8px}
  .news-item{padding:8px;border-bottom:1px solid #eef6ff}
  .news-item a{color:#0b74d1;text-decoration:none}
  .news-controls{display:flex;gap:8px;align-items:center;margin-bottom:8px}
  @media (max-width:800px){ .row{grid-template-columns:1fr} .kpi{flex-direction:column} table{font-size:12px} .table-wrap{max-height:220px} }
</style>
</head>
<body>
<header>💼 Finance Master — Multi Calculator</header>

<nav id="tabs">
  <button class="active" data-tab="emi"><i class="fa-solid fa-calculator"></i>&nbsp;EMI</button>
  <button data-tab="fd"><i class="fa-solid fa-piggy-bank"></i>&nbsp;FD</button>
  <button data-tab="rd"><i class="fa-solid fa-hand-holding-dollar"></i>&nbsp;RD</button>
  <button data-tab="sip"><i class="fa-solid fa-chart-line"></i>&nbsp;SIP</button>
  <button data-tab="gst"><i class="fa-solid fa-percent"></i>&nbsp;GST</button>
  <button data-tab="fx"><i class="fa-solid fa-money-bill-transfer"></i>&nbsp;Currency</button>
  <button data-tab="news"><i class="fa-solid fa-newspaper"></i>&nbsp;News</button>
</nav>

<main>
  <!-- EMI -->
  <section id="emi" class="card">
    <h2>Loan EMI Calculator</h2>
    <div class="row">
      <div>
        <label>Loan Amount</label>
        <input id="emiP" type="number" value="1000000" />
      </div>
      <div>
        <label>Annual Interest Rate (%)</label>
        <input id="emiR" type="number" step="0.01" value="8.5" />
      </div>
    </div>
    <div class="row">
      <div>
        <label>Tenure (months)</label>
        <input id="emiN" type="number" value="240" />
      </div>
      <div>
        <label>Currency</label>
        <select id="emiCur">
          <option>INR</option><option>USD</option><option>EUR</option><option>GBP</option>
        </select>
      </div>
    </div>

    <div style="margin-top:10px">
      <button class="btn" onclick="calcEMI()">Calculate EMI & Show Schedule</button>
    </div>

    <div class="kpi">
      <div class="box"><div class="label">Monthly EMI</div><div id="emi_k_emi" class="value">-</div></div>
      <div class="box"><div class="label">Total Interest</div><div id="emi_k_int" class="value">-</div></div>
      <div class="box"><div class="label">Total Payment</div><div id="emi_k_tot" class="value">-</div></div>
    </div>

    <div class="note">Below is the month-by-month amortization schedule (interest, principal, balance).</div>
    <div class="table-wrap">
      <table id="emi_table">
        <thead><tr><th>Month</th><th>EMI</th><th>Interest</th><th>Principal</th><th>Balance</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </section>

  <!-- FD -->
  <section id="fd" class="card" style="display:none">
    <h2>Fixed Deposit (FD) Calculator</h2>
    <div class="row">
      <div>
        <label>Principal</label>
        <input id="fdP" type="number" value="50000" />
      </div>
      <div>
        <label>Annual Rate (%)</label>
        <input id="fdR" type="number" step="0.01" value="6.5" />
      </div>
    </div>
    <div class="row">
      <div>
        <label>Time (years)</label>
        <input id="fdT" type="number" step="0.1" value="5" />
      </div>
      <div>
        <label>Compounding (per year)</label>
        <select id="fdN">
          <option value="12" selected>Monthly</option>
          <option value="4">Quarterly</option>
          <option value="2">Half-yearly</option>
          <option value="1">Yearly</option>
        </select>
      </div>
    </div>

    <div style="margin-top:10px">
      <button class="btn" onclick="calcFD()">Calculate FD & Show Monthly</button>
    </div>

    <div class="kpi">
      <div class="box"><div class="label">Maturity Value</div><div id="fd_k_mv" class="value">-</div></div>
      <div class="box"><div class="label">Total Interest</div><div id="fd_k_int" class="value">-</div></div>
      <div class="box"><div class="label">Effective Yield</div><div id="fd_k_yield" class="value">-</div></div>
    </div>

    <div class="note">Monthly growth table (shows balance at the end of each month).</div>
    <div class="table-wrap">
      <table id="fd_table">
        <thead><tr><th>Month</th><th>Balance</th><th>Interest This Month</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </section>

  <!-- RD -->
  <section id="rd" class="card" style="display:none">
    <h2>Recurring Deposit (RD) Calculator</h2>
    <div class="row">
      <div>
        <label>Monthly Installment</label>
        <input id="rdP" type="number" value="5000" />
      </div>
      <div>
        <label>Annual Rate (%)</label>
        <input id="rdR" type="number" step="0.01" value="7.0" />
      </div>
    </div>
    <div class="row">
      <div>
        <label>Tenure (months)</label>
        <input id="rdN" type="number" value="36" />
      </div>
      <div>
        <label>Compounding per year</label>
        <select id="rdC">
          <option value="12" selected>Monthly</option>
          <option value="4">Quarterly</option>
          <option value="2">Half-yearly</option>
        </select>
      </div>
    </div>

    <div style="margin-top:10px">
      <button class="btn" onclick="calcRD()">Calculate RD & Show Monthly</button>
    </div>

    <div class="kpi">
      <div class="box"><div class="label">Maturity Value</div><div id="rd_k_mv" class="value">-</div></div>
      <div class="box"><div class="label">Total Deposits</div><div id="rd_k_dep" class="value">-</div></div>
      <div class="box"><div class="label">Interest Earned</div><div id="rd_k_int" class="value">-</div></div>
    </div>

    <div class="note">Monthly ledger: deposit, interest earned, cumulative balance.</div>
    <div class="table-wrap">
      <table id="rd_table">
        <thead><tr><th>Month</th><th>Deposit</th><th>Interest</th><th>Balance</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </section>

  <!-- SIP -->
  <section id="sip" class="card" style="display:none">
    <h2>SIP Calculator</h2>
    <div class="row">
      <div>
        <label>Monthly SIP Amount</label>
        <input id="sipP" type="number" value="5000" />
      </div>
      <div>
        <label>Expected Annual Return (%)</label>
        <input id="sipR" type="number" step="0.01" value="12" />
      </div>
    </div>
    <div class="row">
      <div>
        <label>Duration (years)</label>
        <input id="sipT" type="number" step="0.1" value="10" />
      </div>
      <div>
        <label>Annual Step-up (%)</label>
        <input id="sipStep" type="number" step="0.1" value="0" />
      </div>
    </div>

    <div style="margin-top:10px">
      <button class="btn" onclick="calcSIP()">Calculate SIP & Show Monthly</button>
    </div>

    <div class="kpi">
      <div class="box"><div class="label">Total Invested</div><div id="sip_k_inv" class="value">-</div></div>
      <div class="box"><div class="label">Estimated Value</div><div id="sip_k_val" class="value">-</div></div>
      <div class="box"><div class="label">Gain</div><div id="sip_k_gain" class="value">-</div></div>
    </div>

    <div class="note">Monthly growth: month, invested this month, value at month end.</div>
    <div class="table-wrap">
      <table id="sip_table">
        <thead><tr><th>Month</th><th>Invested</th><th>Value</th></tr></thead>
        <tbody></tbody>
      </table>
    </div>
  </section>

  <!-- GST (no monthly breakdown) -->
  <section id="gst" class="card" style="display:none">
    <h2>GST Calculator</h2>
    <div class="row">
      <div>
        <label>Amount</label>
        <input id="gstAmt" type="number" value="1000" />
      </div>
      <div>
        <label>GST Rate (%)</label>
        <input id="gstR" type="number" value="18" />
      </div>
    </div>
    <div style="margin-top:10px">
      <button class="btn" onclick="calcGST()">Calculate</button>
    </div>
    <div class="kpi">
      <div class="box"><div class="label">GST Amount</div><div id="gst_k_gst" class="value">-</div></div>
      <div class="box"><div class="label">Net Amount</div><div id="gst_k_net" class="value">-</div></div>
      <div class="box"><div class="label">Gross Amount</div><div id="gst_k_gross" class="value">-</div></div>
    </div>
  </section>

  <!-- Currency -->
  <section id="fx" class="card" style="display:none">
    <h2>Currency Converter (Live + Offline fallback)</h2>
    <div class="row">
      <div>
        <label>Amount</label>
        <input id="fxAmt" type="number" value="100" />
      </div>
      <div>
        <label>From</label>
        <select id="fxFrom">
          <option>USD</option><option>INR</option><option>EUR</option><option>GBP</option><option>AED</option><option>JPY</option>
        </select>
      </div>
    </div>
    <div class="row">
      <div>
        <label>To</label>
        <select id="fxTo">
          <option>INR</option><option>USD</option><option>EUR</option><option>GBP</option><option>AED</option><option>JPY</option>
        </select>
      </div>
      <div>
        <label>Rate (1 From = X To)</label>
        <input id="fxRate" type="number" readonly />
      </div>
    </div>

    <div style="margin-top:10px">
      <button class="btn" onclick="fetchLiveRate()">Fetch Live Rate</button>
      <button class="btn" onclick="calcFX()">Convert</button>
    </div>

    <div class="kpi">
      <div class="box"><div class="label">Converted</div><div id="fx_k_out" class="value">-</div></div>
      <div class="box"><div class="label">Inverse Rate</div><div id="fx_k_inv" class="value">-</div></div>
      <div class="box"><div class="label">Source</div><div id="fx_k_src" class="value">-</div></div>
    </div>
    <div class="note">If API fails or there is no internet, offline rates are used (developer-maintained). USD→INR default = 87.</div>
  </section>

  <!-- News -->
  <section id="news" class="card" style="display:none">
    <h2>Latest Finance & Loan News</h2>
    <div class="news-controls">
      <label style="margin-right:10px">Source:</label>
      <select id="newsSource">
        <option value="https://www.moneycontrol.com/rss/latestnews.xml">Moneycontrol - Latest</option>
        <option value="https://economictimes.indiatimes.com/feeds/newsdefaultfeeds.cms">EconomicTimes - Latest</option>
        <option value="https://www.livemint.com/rss/news">LiveMint - Latest</option>
      </select>
      <button class="btn sec" onclick="loadNews()">Refresh</button>
    </div>
    <div id="newsList"><em>Loading news…</em></div>
    <div class="note">Shows latest headlines from selected source. We fetch RSS via a free CORS proxy.</div>
  </section>

</main>

<script>
/* -------------------------
   Tabs
   ------------------------- */
const tabButtons = document.querySelectorAll('#tabs button');
tabButtons.forEach(btn=>{
  btn.addEventListener('click', ()=> {
    tabButtons.forEach(b=>b.classList.remove('active'));
    btn.classList.add('active');
    const idx = Array.from(tabButtons).indexOf(btn);
    const sections = document.querySelectorAll('main section');
    sections.forEach((s,i)=> s.style.display = (i===idx) ? '' : 'none');
  });
});
// initialize: show first (EMI)
document.querySelectorAll('main section').forEach((s,i)=> s.style.display = (i===0)? '': 'none');

/* -------------------------
   Utilities
   ------------------------- */
const fmt = (v, cur) => {
  if(v===null || v===undefined) return '-';
  const n = Number(v);
  if(isNaN(n)) return '-';
  return (cur==='INR' ? '₹ ' : '') + n.toLocaleString(undefined,{maximumFractionDigits:2});
};
const round2 = x => Math.round((+x + Number.EPSILON)*100)/100;

/* -------------------------
   EMI Calculator + Schedule
   ------------------------- */
function calcEMI(){
  const P = +document.getElementById('emiP').value;
  const R = +document.getElementById('emiR').value;
  const N = +document.getElementById('emiN').value;
  const cur = document.getElementById('emiCur').value || 'INR';

  if(!(P>0 && N>0 && R>=0)){ alert('Please enter valid values'); return; }

  const r = R/12/100; // monthly rate
  let emi;
  if(r===0) emi = P/N;
  else {
    const pow = Math.pow(1+r,N);
    emi = P * r * pow / (pow - 1);
  }
  emi = round2(emi);

  // Build amortization schedule month-by-month
  const tbody = document.querySelector('#emi_table tbody');
  tbody.innerHTML = '';
  let bal = P;
  let totalInt = 0;
  for(let m=1; m<=N; m++){
    const interest = round2(bal * r);
    let principal = round2(emi - interest);
    // last month adjust
    if(m===N) principal = round2(bal);
    const payment = round2(interest + principal);
    bal = round2(bal - principal);
    totalInt += interest;
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${m}</td>
      <td>${fmt(payment,cur)}</td>
      <td>${fmt(interest,cur)}</td>
      <td>${fmt(principal,cur)}</td>
      <td>${fmt(bal,cur)}</td>`;
    tbody.appendChild(tr);
  }

  document.getElementById('emi_k_emi').textContent = fmt(emi, cur);
  document.getElementById('emi_k_int').textContent = fmt(round2(totalInt), cur);
  document.getElementById('emi_k_tot').textContent = fmt(round2(emi*N), cur);
}

/* -------------------------
   FD Calculator monthly table
   ------------------------- */
function calcFD(){
  const P = +document.getElementById('fdP').value;
  const annualR = +document.getElementById('fdR').value;
  const years = +document.getElementById('fdT').value;
  const compPerYear = +document.getElementById('fdN').value; // e.g., 12 monthly
  if(!(P>0 && years>0)) { alert('Enter valid FD inputs'); return; }

  const months = Math.round(years * 12);
  const monthlyRate = Math.pow(1 + annualR/100, 1/12) - 1; // monthly effective rate (approx)
  const tbody = document.querySelector('#fd_table tbody');
  tbody.innerHTML = '';
  let balance = P;
  let totalInt = 0;
  for(let m=1; m<=months; m++){
    const interest = round2(balance * monthlyRate);
    balance = round2(balance + interest);
    totalInt += interest;
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${m}</td><td>${fmt(balance,'INR')}</td><td>${fmt(interest,'INR')}</td>`;
    tbody.appendChild(tr);
  }
  document.getElementById('fd_k_mv').textContent = fmt(balance,'INR');
  document.getElementById('fd_k_int').textContent = fmt(round2(totalInt),'INR');
  // effective annual yield (approx)
  const eay = (Math.pow(1 + monthlyRate, 12) - 1) * 100;
  document.getElementById('fd_k_yield').textContent = round2(eay) + '%';
}

/* -------------------------
   RD Calculator monthly ledger
   ------------------------- */
function calcRD(){
  const monthlyDeposit = +document.getElementById('rdP').value;
  const annualR = +document.getElementById('rdR').value;
  const months = +document.getElementById('rdN').value;
  if(!(monthlyDeposit>0 && months>0)){ alert('Enter valid RD inputs'); return; }

  // We'll use monthly compounding (approx). For RD exact banks use quarterly; this is an approximation.
  const monthlyRate = Math.pow(1 + annualR/100, 1/12) - 1;
  const tbody = document.querySelector('#rd_table tbody');
  tbody.innerHTML = '';
  let balance = 0;
  let totalInterest = 0;
  for(let m=1; m<=months; m++){
    // deposit at start of month (common convention), then interest for the month applies
    balance = round2(balance + monthlyDeposit);
    const interest = round2(balance * monthlyRate);
    balance = round2(balance + interest);
    totalInterest += interest;
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${m}</td><td>${fmt(monthlyDeposit,'INR')}</td><td>${fmt(interest,'INR')}</td><td>${fmt(balance,'INR')}</td>`;
    tbody.appendChild(tr);
  }
  document.getElementById('rd_k_mv').textContent = fmt(balance,'INR');
  document.getElementById('rd_k_dep').textContent = fmt(monthlyDeposit * months,'INR');
  document.getElementById('rd_k_int').textContent = fmt(round2(totalInterest),'INR');
}

/* -------------------------
   SIP Calculator monthly ledger
   ------------------------- */
function calcSIP(){
  let P = +document.getElementById('sipP').value;
  const annualR = +document.getElementById('sipR').value;
  const years = +document.getElementById('sipT').value;
  const stepPct = +document.getElementById('sipStep').value;
  const months = Math.round(years * 12);
  if(!(P>0 && months>0)){ alert('Enter valid SIP inputs'); return; }

  const monthlyRate = Math.pow(1 + annualR/100, 1/12) - 1;
  const tbody = document.querySelector('#sip_table tbody');
  tbody.innerHTML = '';
  let fv = 0;
  let invested = 0;
  let currentP = P;
  for(let m=1; m<=months; m++){
    // apply monthly contribution then growth for month
    invested += currentP;
    fv = round2((fv + currentP) * (1 + monthlyRate));
    // apply annual step-up at each 12th month (after year's contributions)
    if(m % 12 === 0 && stepPct !== 0){
      currentP = round2(currentP * (1 + stepPct/100));
    }
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${m}</td><td>${fmt((m%12===1?currentP:currentP),'INR')}</td><td>${fmt(fv,'INR')}</td>`;
    tbody.appendChild(tr);
  }
  document.getElementById('sip_k_inv').textContent = fmt(invested,'INR');
  document.getElementById('sip_k_val').textContent = fmt(fv,'INR');
  document.getElementById('sip_k_gain').textContent = fmt(round2(fv - invested),'INR');
}

/* -------------------------
   GST
   ------------------------- */
function calcGST(){
  const amt = +document.getElementById('gstAmt').value;
  const rate = +document.getElementById('gstR').value;
  if(!(amt>=0)) { alert('Enter valid amount'); return; }
  const gst = round2(amt * rate / 100);
  const gross = round2(amt + gst);
  document.getElementById('gst_k_gst').textContent = fmt(gst,'INR');
  document.getElementById('gst_k_net').textContent = fmt(amt,'INR');
  document.getElementById('gst_k_gross').textContent = fmt(gross,'INR');
}

/* -------------------------
   Currency converter (live + offline fallback)
   ------------------------- */
const offlineRates = {
  "USD": { "INR": 87, "EUR": 0.92, "GBP": 0.78, "AED": 3.67, "JPY": 149 },
  "INR": { "USD": 1/87, "EUR": 0.0106, "GBP": 0.0090, "AED": 0.042, "JPY": 0.0115 },
  "EUR": { "USD": 1.09, "INR": 94.5, "GBP": 0.85, "AED": 4.0, "JPY": 162 },
  "GBP": { "USD": 1.28, "INR": 110.5, "EUR": 1.18, "AED": 4.7, "JPY": 190 },
  "AED": { "USD": 0.27, "INR": 23.67, "EUR": 0.25, "GBP": 0.21, "JPY": 40 },
  "JPY": { "USD": 0.0067, "INR": 0.61, "EUR": 0.0062, "GBP": 0.0053, "AED": 0.025 }
};

async function fetchLiveRate(){
  const from = document.getElementById('fxFrom').value;
  const to = document.getElementById('fxTo').value;
  if(from === to){ alert('Select two different currencies'); return; }
  // try API
  try {
    const resp = await fetch(`https://api.exchangerate.host/latest?base=${from}&symbols=${to}`);
    const data = await resp.json();
    if(data && data.rates && data.rates[to]){
      const rate = Number(data.rates[to]);
      document.getElementById('fxRate').value = rate.toFixed(6);
      document.getElementById('fx_k_src').textContent = 'exchangerate.host';
      calcFX(); // auto-calc
      return;
    }
  } catch(e){}
  // fallback offline:
  const r = (offlineRates[from] && offlineRates[from][to]) ? offlineRates[from][to] : null;
  if(r){
    document.getElementById('fxRate').value = Number(r).toFixed(6);
    document.getElementById('fx_k_src').textContent = 'offline';
    calcFX();
  } else {
    alert('Rate not available (and API failed).');
  }
}

function calcFX(){
  const amt = +document.getElementById('fxAmt').value;
  const from = document.getElementById('fxFrom').value;
  const to = document.getElementById('fxTo').value;
  const rate = +document.getElementById('fxRate').value;
  if(!(amt>=0)) { alert('Enter valid amount'); return; }
  if(!(rate>0)){ alert('Please fetch rate first'); return; }
  const out = round2(amt * rate);
  document.getElementById('fx_k_out').textContent = out + ' ' + to;
  document.getElementById('fx_k_inv').textContent = '1 ' + to + ' = ' + (round2(1/rate)) + ' ' + from;
}

/* -------------------------
   RSS News fetch (via public CORS proxy)
   ------------------------- */
async function loadNews(){
  const list = document.getElementById('newsList');
  list.innerHTML = '<em>Fetching latest headlines…</em>';
  const rssUrl = document.getElementById('newsSource').value;
  try {
    // Use AllOrigins as free CORS proxy
    const proxy = 'https://api.allorigins.win/get?url=' + encodeURIComponent(rssUrl);
    const res = await fetch(proxy);
    if(!res.ok) throw new Error('Proxy fetch failed');
    const payload = await res.json();
    const parser = new DOMParser();
    const xml = parser.parseFromString(payload.contents, "application/xml");
    const items = Array.from(xml.querySelectorAll('item')).slice(0, 25); // top 25
    // Filter for loan/bank/home/offer/interest keywords and also keep other finance news
    const keywords = ['loan','home','interest','bank','offer','rate','EMI','personal loan','housing','housing loan','mortgage','lending'];
    const scored = items.map(it => {
      const title = it.querySelector('title') ? it.querySelector('title').textContent : '';
      const link = it.querySelector('link') ? it.querySelector('link').textContent : (it.querySelector('guid') ? it.querySelector('guid').textContent : '#');
      const pub = it.querySelector('pubDate') ? it.querySelector('pubDate').textContent : '';
      const desc = it.querySelector('description') ? it.querySelector('description').textContent : '';
      const text = (title + ' ' + desc).toLowerCase();
      let score = 0;
      for(const k of keywords) if(text.includes(k)) score += 2;
      return {title, link, pub, score};
    });
    // sort by score desc then original order
    scored.sort((a,b)=> b.score - a.score);
    // show top 12 (prioritizing loan-related)
    const top = scored.slice(0,12);
    if(top.length===0) { list.innerHTML = '<p>No news found.</p>'; return; }
    list.innerHTML = top.map(it => {
      const dateStr = it.pub ? `<small style="color:#7b8590;margin-left:6px">${it.pub}</small>` : '';
      return `<div class="news-item"><a href="${it.link}" target="_blank" rel="noopener noreferrer">${escapeHtml(it.title)}</a>${dateStr}</div>`;
    }).join('');
  } catch(err){
    console.error(err);
    list.innerHTML = '<p>Error loading news. Try "Refresh" or check your connection.</p>';
  }
}

// small helper to avoid injecting raw HTML from feed
function escapeHtml(s){
  return s.replace(/[&<>"']/g, function(m){ return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m]; });
}

/* -------------------------
   Initialization
   ------------------------- */
window.addEventListener('load', ()=> {
  // init calculators quickly
  try{ calcEMI(); }catch(e){}
  try{ calcFD(); }catch(e){}
  try{ calcRD(); }catch(e){}
  try{ calcSIP(); }catch(e){}
  // default currency options prefilled (already in markup)
  // load news for selected source
  loadNews();
  // attach change handler for news source to reload
  document.getElementById('newsSource').addEventListener('change', loadNews);
});
</script>
</body>
</html>
