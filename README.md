
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Simple Calculator</title>
  <style>
    :root{
      --bg:#f4f7fb;
      --panel:#ffffff;
      --accent:#2b6ef6;
      --muted:#6b7280;
      --btn:#e6eefc;
      --danger:#ef4444;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      font-family:Inter,Segoe UI,Roboto,Arial,sans-serif;
      background:linear-gradient(180deg,#eef2ff 0%,var(--bg) 100%);
      display:flex;
      align-items:center;
      justify-content:center;
      height:100vh;
      padding:20px;
    }
    .calculator{
      width:320px;
      background:var(--panel);
      border-radius:12px;
      box-shadow:0 10px 30px rgba(20,30,60,0.12);
      overflow:hidden;
    }
    .display{
      background:linear-gradient(180deg,#f8fbff,#ffffff);
      padding:18px;
      text-align:right;
      font-size:28px;
      color:#0b1220;
      letter-spacing:0.6px;
    }
    .subdisplay{
      font-size:12px;
      color:var(--muted);
      height:16px;
      margin-bottom:6px;
      overflow:hidden;
      white-space:nowrap;
      text-overflow:ellipsis;
    }
    .screen{
      font-weight:600;
      min-height:34px;
    }
    .keys{
      display:grid;
      grid-template-columns:repeat(4,1fr);
      gap:10px;
      padding:16px;
      background:linear-gradient(180deg,#fbfdff,#f7f9fc);
    }
    button{
      height:56px;
      border-radius:8px;
      border:0;
      font-size:18px;
      cursor:pointer;
      transition:transform .06s ease, box-shadow .06s;
      box-shadow:0 4px 10px rgba(16,24,40,0.06);
    }
    button:active{transform:translateY(1px)}
    .btn-operator{background:var(--btn); color:#0b1220}
    .btn-equal{background:var(--accent); color:#fff; grid-column:span 2}
    .btn-clear{background:var(--danger); color:#fff}
    .btn-wide{grid-column:span 2}
    .btn-digit{background:#fff}
    .small{font-size:14px}
    footer{font-size:12px;color:var(--muted);text-align:center;padding:10px 0}
    @media (max-width:360px){
      .calculator{width:100%}
      button{height:50px;font-size:16px}
    }
  </style>
</head>
<body>
  <div class="calculator" role="application" aria-label="Simple calculator">
    <div class="display" aria-live="polite">
      <div class="subdisplay" id="history"></div>
      <div class="screen" id="display">0</div>
    </div>

    <div class="keys" id="keys">
      <button class="btn-clear small" data-action="clear">C</button>
      <button class="btn-operator small" data-action="back">⌫</button>
      <button class="btn-operator small" data-action="percent">%</button>
      <button class="btn-operator" data-action="/">÷</button>

      <button class="btn-digit" data-digit="7">7</button>
      <button class="btn-digit" data-digit="8">8</button>
      <button class="btn-digit" data-digit="9">9</button>
      <button class="btn-operator" data-action="*">×</button>

      <button class="btn-digit" data-digit="4">4</button>
      <button class="btn-digit" data-digit="5">5</button>
      <button class="btn-digit" data-digit="6">6</button>
      <button class="btn-operator" data-action="-">−</button>

      <button class="btn-digit" data-digit="1">1</button>
      <button class="btn-digit" data-digit="2">2</button>
      <button class="btn-digit" data-digit="3">3</button>
      <button class="btn-operator" data-action="+">+</button>

      <button class="btn-digit btn-wide" data-digit="0">0</button>
      <button class="btn-digit" data-digit=".">.</button>
      <button class="btn-equal" data-action="=">=</button>
    </div>

    <footer>Keyboard supported: numbers, + - * / . Enter = Backspace Esc</footer>
  </div>

  <script>
    (function(){
      const displayEl = document.getElementById('display');
      const historyEl = document.getElementById('history');
      const keys = document.getElementById('keys');

      let current = '0';
      let previous = null;
      let operator = null;
      let justEvaluated = false;

      function updateDisplay(){
        displayEl.textContent = current;
        historyEl.textContent = previous !== null ? `${previous} ${operator || ''}` : '';
      }

      function inputDigit(d){
        if (justEvaluated) { current = '0'; justEvaluated = false; }
        if (d === '.' && current.includes('.')) return;
        current = (current === '0' && d !== '.') ? d : current + d;
        updateDisplay();
      }

      function clearAll(){
        current = '0'; previous = null; operator = null; justEvaluated = false;
        updateDisplay();
      }

      function backspace(){
        if (justEvaluated) { clearAll(); return; }
        current = current.length > 1 ? current.slice(0,-1) : '0';
        updateDisplay();
      }

      function applyPercent(){
        current = String(parseFloat(current) / 100);
        updateDisplay();
      }

      function chooseOperator(op){
        if (operator && !justEvaluated) {
          evaluate();
        }
        previous = current;
        operator = op;
        current = '0';
        updateDisplay();
      }

      function evaluate(){
        if (!operator || previous === null) return;
        const a = parseFloat(previous);
        const b = parseFloat(current);
        let res = 0;
        switch(operator){
          case '+': res = a + b; break;
          case '-': res = a - b; break;
          case '*': res = a * b; break;
          case '/':
            if (b === 0) { alert('Error: Division by zero'); clearAll(); return; }
            res = a / b; break;
          default: return;
        }
        current = String(Number.isFinite(res) ? res : 0);
        previous = null;
        operator = null;
        justEvaluated = true;
        updateDisplay();
      }

      keys.addEventListener('click', (e) => {
        const btn = e.target.closest('button');
        if (!btn) return;
        if (btn.dataset.digit) {
          inputDigit(btn.dataset.digit);
          return;
        }
        const action = btn.dataset.action;
        if (!action) return;
        if (action === 'clear') clearAll();
        else if (action === 'back') backspace();
        else if (action === 'percent') applyPercent();
        else if (action === '=') evaluate();
        else chooseOperator(action);
      });

      // Keyboard support
      window.addEventListener('keydown', (e) => {
        if (e.key >= '0' && e.key <= '9') { inputDigit(e.key); e.preventDefault(); return; }
        if (e.key === '.') { inputDigit('.'); e.preventDefault(); return; }
        if (e.key === 'Enter' || e.key === '=') { evaluate(); e.preventDefault(); return; }
        if (e.key === 'Backspace') { backspace(); e.preventDefault(); return; }
        if (e.key === 'Escape') { clearAll(); e.preventDefault(); return; }
        if (['+','-','*','/'].includes(e.key)) { chooseOperator(e.key); e.preventDefault(); return; }
      });

      // Initialize
      updateDisplay();
    })();
  </script>
</body>
</html>
