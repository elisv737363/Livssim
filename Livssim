<!DOCTYPE html>
<html lang="no">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LivsSim Bank – Sparebanken</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background:#f0f4f8; margin:0; padding:0; color:#111827;}
header { position:relative; text-align:center; color:white; margin-bottom:10px;}
header img.logo { height:50px; margin-top:10px; z-index:2; position:relative;}
header h1 { position:absolute; bottom:10px; left:50%; transform:translateX(-50%); font-size:24px; text-shadow:2px 2px 4px #000;}
header img.banner { width:100%; max-height:200px; object-fit:cover; border-radius:0 0 12px 12px;}
.container { padding:15px; }
.card { background:white; padding:15px; margin:10px 0; border-radius:12px; box-shadow:0 3px 6px rgba(0,0,0,0.15);}
button { padding:10px; margin:5px 0; background:#2563eb; border:none; color:white; font-size:16px; border-radius:8px;}
input { padding:10px; margin:5px 0; width:100%; box-sizing:border-box; border-radius:6px; border:1px solid #cbd5e1;}
.tab { margin:5px; cursor:pointer; display:inline-block; padding:6px 12px; background:#e0f2fe; border-radius:6px; font-weight:bold; color:#2563eb; transition:0.2s;}
.tab:hover { background:#bae6fd; }
.hidden { display:none; }
</style>
</head>
<body>

<header>
  <!-- SpareBank 1-logo fra Wikimedia -->
  <img class="logo" src="https://upload.wikimedia.org/wikipedia/commons/2/28/SpareBank_1_logo.svg" alt="SpareBank 1 Logo">
  <h1>LivsSim Bank</h1>
  <!-- Banner -->
  <img class="banner" src="https://cdn.pixabay.com/photo/2017/05/10/15/15/coins-2306043_1280.jpg" alt="Bank Banner">
</header>

<div class="container" id="login">
  <h2>Logg inn</h2>
  <input id="username" placeholder="Skriv navnet ditt">
  <button onclick="login()">Start</button>
</div>

<div class="container hidden" id="app">
<h2 id="welcome"></h2>
<p id="jobInfo"></p>

<div>
  <span class="tab" onclick="showTab('konto')">💳 Konto</span>
  <span class="tab" onclick="showTab('butikk')">🛒 Butikk</span>
  <span class="tab" onclick="showTab('sparefond')">💰 Sparing & Fond</span>
  <span class="tab" onclick="showTab('forsikring')">🛡️ Forsikring</span>
  <span class="tab" onclick="showTab('credit')">💳 Kredittkort</span>
</div>

<!-- Konto -->
<div id="konto" class="tabContent">
  <div class="card">
    <p id="balance"></p>
    <p id="savings"></p>
    <p id="fundValue"></p>
    <p id="salary"></p>
    <p id="tax"></p>
    <p id="debt"></p>
    <p id="interest"></p>
  </div>
  <div class="card" style="height:250px;">
    <h3>Økonomi</h3>
    <canvas id="chart"></canvas>
  </div>
  <button onclick="nextMonth()">Neste måned</button>
</div>

<!-- Butikk -->
<div id="butikk" class="tabContent hidden">
  <div class="card">
    <h3>Kjøp ting</h3>
    <button onclick="buyItem('Mat',500,1)">Mat 500kr</button>
    <button onclick="buyItem('Husleie',8000,0)">Husleie 8000kr</button>
    <button onclick="buyItem('Bil',10000,0)">Bil 10 000kr</button>
    <button onclick="buyItem('Gadgets',3000,2)">Gadgets 3000kr</button>
    <button onclick="buyItem('Klær',1500,2)">Klær 1500kr</button>
  </div>
</div>

<!-- Sparing & Fond -->
<div id="sparefond" class="tabContent hidden">
  <div class="card">
    <h3>Sparing</h3>
    <input id="saveAmount" placeholder="Beløp å spare">
    <button onclick="saveMoney()">Spar</button>
    <h3>Fond</h3>
    <input id="fundAmount" placeholder="Beløp å investere i fond">
    <button onclick="buyFund()">Kjøp fond</button>
    <p id="fundStatus">Fondverdi: 0 kr</p>
  </div>
</div>

<!-- Forsikring -->
<div id="forsikring" class="tabContent hidden">
  <div class="card">
    <h3>Kjøp forsikring</h3>
    <button onclick="buyInsurance('Helse',1000)">Helse 1000kr</button>
    <button onclick="buyInsurance('Bil',500)">Bil 500kr</button>
    <button onclick="buyInsurance('Bolig',700)">Bolig 700kr</button>
    <button onclick="buyInsurance('Innbo',400)">Innbo 400kr</button>
    <button onclick="buyInsurance('Reise',300)">Reise 300kr</button>
    <p id="insuranceStatus">Ingen forsikringer kjøpt</p>
  </div>
</div>

<!-- Kredittkort -->
<div id="credit" class="tabContent hidden">
  <div class="card">
    <p id="creditStatus">Ingen kort</p>
    <button onclick="orderCredit()">Bestill kredittkort</button>
  </div>
</div>

</div>

<script>
let user;
let chart;

const jobs = [
  {title:"Lærer", salary:45000},
  {title:"IT-utvikler", salary:55000},
  {title:"Sykepleier", salary:42000},
  {title:"Elektriker", salary:48000},
  {title:"Designer", salary:43000},
  {title:"Markedsfører", salary:46000}
];

function login() {
  const name = document.getElementById("username").value || "Bruker";
  const job = jobs[Math.floor(Math.random()*jobs.length)];
  user = {
    name, job: job.title, salary: job.salary,
    balance:0, savings:0, fund:0,
    income:job.salary, expenses:Math.floor(job.salary*0.5),
    tax:Math.floor(job.salary*0.25),
    debt:0, interest:0, credit:false,
    insurance:[], transactions:[]
  };
  document.getElementById("login").classList.add("hidden");
  document.getElementById("app").classList.remove("hidden");
  document.getElementById("jobInfo").innerText = `Yrke: ${user.job} | Månedslønn: ${user.salary} kr`;
  updateUI(); drawChart();
}

function nextMonth(){
  const netIncome = user.income - user.tax;
  user.balance += netIncome;
  user.transactions.push(`+${netIncome} kr (lønn etter skatt)`);
  user.balance -= user.expenses;
  user.transactions.push(`-${user.expenses} kr (utgifter)`);
  user.fund = Math.round(user.fund * 1.02);
  if(user.balance<0){
    user.debt = Math.abs(user.balance);
    user.interest = Math.ceil(user.debt*0.05);
    user.balance=0;
    alert("Du har brukt for mye! Gjeld og rente øker.");
  } else { user.debt=0; user.interest=0; }
  updateUI(); drawChart();
}

function buyItem(name,price,priority){
  if(priority===1 && user.balance<price){ alert("Prioriter viktige ting først!"); return; }
  if(user.balance>=price){
    user.balance -= price;
    user.transactions.push(`-${price} kr (${name})`);
    alert(`${name} kjøpt!`);
  } else {
    const deficit = price - user.balance;
    user.balance=0; user.debt+=deficit; user.interest+=Math.ceil(deficit*0.05);
    user.transactions.push(`-${price} kr (${name}) -> gjeld ${deficit}`);
    alert(`${name} kjøpt på kreditt!`);
  }
  updateUI(); drawChart();
}

function saveMoney(){
  const amount = Number(document.getElementById("saveAmount").value);
  if(amount>0 && user.balance>=amount){ user.balance-=amount; user.savings+=amount; alert(`Satt ${amount} kr på sparekonto`);}
  else{alert("Ikke nok penger!");}
  updateUI(); drawChart();
}

function buyFund(){
  const amount = Number(document.getElementById("fundAmount").value);
  if(amount>0 && user.balance>=amount){ user.balance-=amount; user.fund+=amount; alert(`Kjøpt fond for ${amount} kr`);}
  else{alert("Ikke nok penger!");}
  updateUI(); drawChart();
}

function buyInsurance(type,cost){
  if(user.balance>=cost){ user.balance-=cost; if(!user.insurance.includes(type)) user.insurance.push(type); alert(`${type} forsikring kjøpt!`);}
  else{alert("Ikke nok penger!");}
  document.getElementById("insuranceStatus").innerText = `Forsikringer: ${user.insurance.join(", ") || "Ingen"}`;
  updateUI(); drawChart();
}

function orderCredit(){
  if(user.credit){ alert("Du har allerede kredittkort"); }
  else{ user.credit=true; alert("Kredittkort bestilt!"); document.getElementById("creditStatus").innerText="Kredittkort aktivt";}
  updateUI();
}

function updateUI(){
  document.getElementById("welcome").innerText="Hei "+user.name;
  document.getElementById("balance").innerText=`Saldo: ${user.balance} kr`;
  document.getElementById("savings").innerText=`Sparing: ${user.savings} kr`;
  document.getElementById("fundValue").innerText=`Fondverdi: ${user.fund} kr`;
  document.getElementById("salary").innerText=`Månedslønn: ${user.salary} kr`;
  document.getElementById("tax").innerText=`Skatt: ${user.tax} kr`;
  document.getElementById("debt").innerText=`Gjeld: ${user.debt} kr`;
  document.getElementById("interest").innerText=`Rente: ${user.interest} kr`;
}

function drawChart(){
  const ctx=document.getElementById("chart");
  if(chart) chart.destroy();
  chart=new Chart(ctx,{
    type:"bar",
    data:{
      labels:["Inntekt","Utgifter","Gjeld","Rente","Sparing","Fond"],
      datasets:[{label:"Økonomi",data:[user.income,user.expenses,user.debt,user.interest,user.savings,user.fund],backgroundColor:["#2563eb","#3b82f6","#ef4444","#f97316","#10b981","#8b5cf6"]}]
    },
    options:{responsive:true, maintainAspectRatio:false}
  });
}

function showTab(tab){ ["konto","butikk","sparefond","forsikring","credit"].forEach(t=>document.getElementById(t).classList.add("hidden")); document.getElementById(tab).classList.remove("hidden");}
</script>

</body>
</html>
