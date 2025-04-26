<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Finance Fundamentals Trainer</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f9f9ff; margin: 0; padding: 0; }
    header { background: #4CAF50; color: white; padding: 1rem; text-align: center; font-size: 1.8rem; }
    nav { display: flex; flex-wrap: wrap; background: #333; justify-content: center; }
    nav button { background: #333; color: white; border: none; margin: 5px; padding: 10px 15px; font-size: 1rem; cursor: pointer; }
    nav button:hover { background: #555; }
    section { padding: 20px; max-width: 900px; margin: auto; }
    .hidden { display: none; }
    .quiz button { margin: 5px 0; padding: 10px; width: 100%; font-size: 1rem; cursor: pointer; }
    .correct { background: #c8f7c5; }
    .incorrect { background: #f7c5c5; }
    #confetti { position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 9999; }
    #progress { text-align: center; margin: 10px; font-weight: bold; }
    #certificate { display: none; background: white; padding: 2rem; margin: 2rem auto; text-align: center; border: 2px solid #4CAF50; }
  </style>
</head>
<body>

<canvas id="confetti"></canvas>
<header>Finance Fundamentals Trainer</header>

<nav>
  <button onclick="showSection('module1')">Basics</button>
  <button onclick="showSection('module2')">Statements</button>
  <button onclick="showSection('module3')">Indicators</button>
  <button onclick="showSection('module4')">DCF</button>
  <button onclick="showSection('calculator')">DCF Calculator</button>
  <button onclick="showSection('caseStudies')">Case Studies</button>
  <button onclick="showSection('finalQuiz')">Final Quiz</button>
</nav>

<div id="progress">Progress: 0%</div>

<section id="module1">
  <h2>Module 1: Basics</h2>
  <p>Understand the key financial statements: P&L, Balance Sheet, and Cash Flow.</p>
  <div class="quiz">
    <p>Which statement shows \"Assets = Liabilities + Equity\"?</p>
    <button onclick="answer(this,true)">Balance Sheet</button>
    <button onclick="answer(this,false)">Income Statement</button>
    <button onclick="answer(this,false)">Cash Flow</button>
  </div>
</section>

<section id="module2" class="hidden">
  <h2>Module 2: Financial Statements</h2>
  <p><b>Example Tesla:</b> Assets $100B | Liabilities $70B | Equity $30B.</p>
  <div class="quiz">
    <p>What is an Asset?</p>
    <button onclick="answer(this,true)">Cash</button>
    <button onclick="answer(this,false)">Loan</button>
  </div>
</section>

<section id="module3" class="hidden">
  <h2>Module 3: Key Indicators</h2>
  <p><b>EBIT:</b> Earnings Before Interest and Taxes.<br><b>EBITDA:</b> EBIT plus Depreciation & Amortization.<br><b>FCF:</b> Free Cash Flow = Cash after capital expenses.</p>
  <div class="quiz">
    <p>What does EBITDA add back?</p>
    <button onclick="answer(this,true)">Depreciation & Amortization</button>
    <button onclick="answer(this,false)">Dividends</button>
  </div>
</section>

<section id="module4" class="hidden">
  <h2>Module 4: DCF</h2>
  <p>DCF (Discounted Cash Flow) values a company based on future cash flow estimates.</p>
  <div class="quiz">
    <p>Why discount cash flows?</p>
    <button onclick="answer(this,true)">Time value of money</button>
    <button onclick="answer(this,false)">Tax reasons</button>
  </div>
</section>

<section id="calculator" class="hidden">
  <h2>DCF Calculator</h2>
  <p>Enter values below to estimate the company's value:</p>
  <input id="fcf" type="number" placeholder="Free Cash Flow (€)" style="width: 100%; padding: 8px; margin: 5px 0;">
  <input id="growth" type="number" placeholder="Growth Rate (%)" style="width: 100%; padding: 8px; margin: 5px 0;">
  <input id="discount" type="number" placeholder="Discount Rate (%)" style="width: 100%; padding: 8px; margin: 5px 0;">
  <button onclick="calculateDCF()" style="width:100%; padding:10px; margin-top:10px;">Calculate DCF Value</button>
  <h3 id="dcfResult"></h3>
</section>

<section id="caseStudies" class="hidden">
  <h2>Case Studies</h2>
  <h3>Airbus</h3>
  <p>Revenue: €65B | EBIT: €5.3B | Equity: €25B</p>
  <h3>L'Oréal</h3>
  <p>Revenue: €38B | EBITDA: €9B | Equity: €30B</p>
</section>

<section id="finalQuiz" class="hidden">
  <h2>Final Quiz</h2>
  <div class="quiz">
    <p>1. FCF Formula?</p>
    <button onclick="answer(this,true)">Operating Cash Flow - CapEx</button>
    <button onclick="answer(this,false)">Net Income - Dividends</button>
    <p>2. What shows profitability?</p>
    <button onclick="answer(this,true)">Income Statement</button>
    <button onclick="answer(this,false)">Balance Sheet</button>
    <p>3. Which company had €5.3B EBIT?</p>
    <button onclick="answer(this,true)">Airbus</button>
    <button onclick="answer(this,false)">L'Oréal</button>
  </div>
</section>

<div id="certificate">
  <h1>🎓 Certificate of Completion 🎓</h1>
  <p>Congratulations! You've completed the Finance Fundamentals Trainer.</p>
</div>

<script>
let progress = localStorage.getItem('progress') ? parseInt(localStorage.getItem('progress')) : 0;
updateProgress();

function showSection(id) {
  document.querySelectorAll('section').forEach(sec => sec.classList.add('hidden'));
  document.getElementById(id).classList.remove('hidden');
}

function answer(button, correct) {
  if (button.classList.contains('correct') || button.classList.contains('incorrect')) return;
  if (correct) {
    button.classList.add('correct');
    progress += 5;
    if (progress >= 100) {
      showCertificate();
      launchConfetti();
    }
  } else {
    button.classList.add('incorrect');
  }
  updateProgress();
}

function updateProgress() {
  document.getElementById('progress').innerText = `Progress: ${progress}%`;
  localStorage.setItem('progress', progress);
}

function calculateDCF() {
  const fcf = parseFloat(document.getElementById('fcf').value);
  const growth = parseFloat(document.getElementById('growth').value) / 100;
  const discount = parseFloat(document.getElementById('discount').value) / 100;
  if (discount <= growth) {
    document.getElementById('dcfResult').innerText = \"Error: Discount rate must be greater than growth rate.\";
    return;
  }
  const value = fcf / (discount - growth);
  document.getElementById('dcfResult').innerText = `Estimated Value: €${value.toFixed(2)}M`;
}

function showCertificate() {
  document.getElementById('certificate').style.display = 'block';
}

function launchConfetti() {
  const canvas = document.getElementById('confetti');
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  const pieces = Array.from({length: 300}, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    size: Math.random() * 5 + 2,
    speed: Math.random() * 3 + 2,
    color: `hsl(${Math.random() * 360}, 100%, 50%)`
  }));

  function update() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    for (const p of pieces) {
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
      ctx.fillStyle = p.color;
      ctx.fill();
      p.y += p.speed;
      if (p.y > canvas.height) p.y = 0;
    }
    requestAnimationFrame(update);
  }
  update();
  setTimeout(() => ctx.clearRect(0, 0, canvas.width, canvas.height), 5000);
}
</script>

</body>
</html>
