<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Calculator</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;700;800&family=JetBrains+Mono:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0f0f0f;
    --surface: #1a1a1a;
    --surface2: #242424;
    --border: #2e2e2e;
    --accent: #e8ff47;
    --accent2: #ff6b35;
    --text: #f0f0f0;
    --text-dim: #888;
    --btn-num: #1e1e1e;
    --btn-op: #2a2a1a;
    --btn-eq: #e8ff47;
    --shadow: 0 0 40px rgba(232,255,71,0.07);
    --radius: 18px;
  }

  body {
    min-height: 100vh;
    background: var(--bg);
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'JetBrains Mono', monospace;
    background-image: radial-gradient(ellipse at 30% 20%, rgba(232,255,71,0.04) 0%, transparent 60%),
                      radial-gradient(ellipse at 70% 80%, rgba(255,107,53,0.04) 0%, transparent 60%);
  }

  .calculator {
    width: 360px;
    background: var(--surface);
    border-radius: 28px;
    border: 1px solid var(--border);
    box-shadow: var(--shadow), 0 40px 80px rgba(0,0,0,0.6), inset 0 1px 0 rgba(255,255,255,0.05);
    overflow: hidden;
    animation: rise 0.5s cubic-bezier(0.16,1,0.3,1) both;
  }

  @keyframes rise {
    from { opacity: 0; transform: translateY(30px) scale(0.97); }
    to   { opacity: 1; transform: translateY(0) scale(1); }
  }

  /* Header */
  .calc-header {
    padding: 20px 24px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .calc-label {
    font-family: 'Syne', sans-serif;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    color: var(--text-dim);
  }

  .mode-dots {
    display: flex;
    gap: 6px;
  }

  .mode-dots span {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--border);
    transition: background 0.2s;
  }

  .mode-dots span.active { background: var(--accent); }

  /* Display */
  .display {
    padding: 16px 24px 20px;
    text-align: right;
    min-height: 110px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    gap: 4px;
  }

  .expression {
    font-size: 13px;
    color: var(--text-dim);
    min-height: 18px;
    letter-spacing: 0.05em;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    direction: rtl;
    transition: opacity 0.15s;
  }

  .result {
    font-size: 42px;
    font-weight: 300;
    color: var(--text);
    letter-spacing: -0.02em;
    line-height: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    direction: rtl;
    transition: color 0.15s;
  }

  .result.accent { color: var(--accent); }

  /* Divider */
  .divider {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--border), transparent);
    margin: 0 24px;
  }

  /* Button grid */
  .buttons {
    padding: 20px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
  }

  button {
    border: none;
    border-radius: var(--radius);
    font-family: 'JetBrains Mono', monospace;
    font-size: 18px;
    font-weight: 400;
    cursor: pointer;
    height: 70px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
    transition: transform 0.08s, box-shadow 0.08s, filter 0.08s;
    outline: none;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
  }

  button::after {
    content: '';
    position: absolute;
    inset: 0;
    background: rgba(255,255,255,0);
    transition: background 0.12s;
    border-radius: inherit;
  }

  button:hover::after { background: rgba(255,255,255,0.05); }
  button:active { transform: scale(0.93); filter: brightness(0.92); }

  /* Ripple */
  .ripple {
    position: absolute;
    border-radius: 50%;
    transform: scale(0);
    animation: ripple-anim 0.45s linear;
    background: rgba(255,255,255,0.12);
    pointer-events: none;
  }

  @keyframes ripple-anim {
    to { transform: scale(4); opacity: 0; }
  }

  /* Number buttons */
  .btn-num {
    background: var(--btn-num);
    color: var(--text);
    border: 1px solid var(--border);
    box-shadow: 0 2px 8px rgba(0,0,0,0.3), inset 0 1px 0 rgba(255,255,255,0.04);
    font-size: 20px;
    font-weight: 400;
  }

  /* Operator buttons */
  .btn-op {
    background: var(--btn-op);
    color: var(--accent);
    border: 1px solid rgba(232,255,71,0.15);
    box-shadow: 0 2px 8px rgba(0,0,0,0.3), inset 0 1px 0 rgba(232,255,71,0.06);
    font-size: 22px;
  }

  /* Function buttons (AC, ±, %) */
  .btn-fn {
    background: var(--surface2);
    color: var(--text);
    border: 1px solid var(--border);
    box-shadow: 0 2px 8px rgba(0,0,0,0.3), inset 0 1px 0 rgba(255,255,255,0.04);
    font-size: 16px;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    letter-spacing: 0.03em;
  }

  /* Equals button */
  .btn-eq {
    background: var(--btn-eq);
    color: #0f0f0f;
    border: none;
    box-shadow: 0 4px 20px rgba(232,255,71,0.3), 0 2px 8px rgba(0,0,0,0.2);
    font-size: 26px;
    font-weight: 600;
    grid-row: span 2;
    height: 150px;
    align-self: start;
    transition: transform 0.08s, box-shadow 0.08s, filter 0.08s, background 0.1s;
  }

  .btn-eq:hover { box-shadow: 0 6px 28px rgba(232,255,71,0.45), 0 2px 8px rgba(0,0,0,0.2); }
  .btn-eq:active { box-shadow: 0 2px 10px rgba(232,255,71,0.2); }

  /* Zero spans 2 cols */
  .btn-zero {
    grid-column: span 2;
    justify-content: flex-start;
    padding-left: 26px;
  }

  /* History strip */
  .history-strip {
    padding: 0 20px 18px;
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }

  .hist-tag {
    font-size: 10px;
    color: var(--text-dim);
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 6px;
    padding: 3px 8px;
    letter-spacing: 0.05em;
    cursor: pointer;
    transition: border-color 0.15s, color 0.15s;
    font-family: 'JetBrains Mono', monospace;
  }

  .hist-tag:hover { border-color: var(--accent); color: var(--accent); }
</style>
</head>
<body>

<div class="calculator">
  <div class="calc-header">
    <span class="calc-label">Calc</span>
    <div class="mode-dots">
      <span class="active"></span>
      <span></span>
      <span></span>
    </div>
  </div>

  <div class="display">
    <div class="expression" id="expression"></div>
    <div class="result" id="result">0</div>
  </div>

  <div class="divider"></div>

  <div class="buttons">
    <!-- Row 1 -->
    <button class="btn-fn" data-action="clear">AC</button>
    <button class="btn-fn" data-action="sign">±</button>
    <button class="btn-fn" data-action="percent">%</button>
    <button class="btn-op" data-action="operator" data-value="÷">÷</button>

    <!-- Row 2 -->
    <button class="btn-num" data-action="number" data-value="7">7</button>
    <button class="btn-num" data-action="number" data-value="8">8</button>
    <button class="btn-num" data-action="number" data-value="9">9</button>
    <button class="btn-op" data-action="operator" data-value="×">×</button>

    <!-- Row 3 -->
    <button class="btn-num" data-action="number" data-value="4">4</button>
    <button class="btn-num" data-action="number" data-value="5">5</button>
    <button class="btn-num" data-action="number" data-value="6">6</button>
    <button class="btn-op" data-action="operator" data-value="−">−</button>

    <!-- Row 4 -->
    <button class="btn-num" data-action="number" data-value="1">1</button>
    <button class="btn-num" data-action="number" data-value="2">2</button>
    <button class="btn-num" data-action="number" data-value="3">3</button>
    <button class="btn-op" data-action="operator" data-value="+">+</button>

    <!-- Row 5 -->
    <button class="btn-num btn-zero" data-action="number" data-value="0">0</button>
    <button class="btn-num" data-action="decimal">.</button>
    <button class="btn-eq" data-action="equals">=</button>
  </div>

  <div class="history-strip" id="historyStrip"></div>
</div>

<script>
  const resultEl   = document.getElementById('result');
  const exprEl     = document.getElementById('expression');
  const histStrip  = document.getElementById('historyStrip');

  let current   = '0';
  let prev      = '';
  let operator  = null;
  let freshResult = false;
  let history   = [];

  function updateDisplay(flash = false) {
    resultEl.textContent = formatNumber(current);
    if (flash) {
      resultEl.classList.add('accent');
      setTimeout(() => resultEl.classList.remove('accent'), 300);
    }
  }

  function formatNumber(val) {
    const n = parseFloat(val);
    if (isNaN(n)) return val;
    if (Math.abs(n) >= 1e12 || (Math.abs(n) < 1e-6 && n !== 0)) {
      return n.toExponential(4);
    }
    // Show up to 10 sig digits but strip trailing zeros
    let s = parseFloat(n.toPrecision(10)).toString();
    return s;
  }

  function setExpression() {
    if (operator && prev !== '') {
      exprEl.textContent = `${formatNumber(prev)} ${operator}`;
    } else {
      exprEl.textContent = '';
    }
  }

  function addHistory(expr, res) {
    const entry = `${expr} = ${res}`;
    history.unshift(entry);
    if (history.length > 3) history.pop();
    renderHistory();
  }

  function renderHistory() {
    histStrip.innerHTML = '';
    history.forEach(h => {
      const tag = document.createElement('div');
      tag.className = 'hist-tag';
      tag.textContent = h;
      tag.title = 'Click to restore result';
      tag.addEventListener('click', () => {
        const res = h.split('=').pop().trim();
        current = res;
        freshResult = true;
        operator = null; prev = '';
        exprEl.textContent = '';
        updateDisplay();
      });
      histStrip.appendChild(tag);
    });
  }

  function handleClear() {
    current = '0'; prev = ''; operator = null; freshResult = false;
    exprEl.textContent = '';
    updateDisplay();
  }

  function handleSign() {
    if (current === '0') return;
    current = (parseFloat(current) * -1).toString();
    updateDisplay();
  }

  function handlePercent() {
    current = (parseFloat(current) / 100).toString();
    updateDisplay();
  }

  function handleNumber(val) {
    if (freshResult) { current = val; freshResult = false; }
    else current = current === '0' ? val : current + val;
    if (current.length > 12) return; // cap length
    updateDisplay();
  }

  function handleDecimal() {
    if (freshResult) { current = '0.'; freshResult = false; updateDisplay(); return; }
    if (!current.includes('.')) { current += '.'; updateDisplay(); }
  }

  function handleOperator(op) {
    if (operator && !freshResult) { calculate(false); }
    prev = current;
    operator = op;
    freshResult = true;
    setExpression();
  }

  function calculate(doFlash = true) {
    if (!operator || prev === '') return;
    const a = parseFloat(prev), b = parseFloat(current);
    let res;
    switch (operator) {
      case '+': res = a + b; break;
      case '−': res = a - b; break;
      case '×': res = a * b; break;
      case '÷': res = b === 0 ? 'Error' : a / b; break;
      default: return;
    }
    const expr = `${formatNumber(prev)} ${operator} ${formatNumber(current)}`;
    addHistory(expr, res === 'Error' ? 'Error' : formatNumber(res.toString()));
    exprEl.textContent = expr + ' =';
    current = res === 'Error' ? 'Error' : res.toString();
    operator = null; prev = ''; freshResult = true;
    updateDisplay(doFlash);
  }

  // Keyboard support
  document.addEventListener('keydown', e => {
    if (e.key >= '0' && e.key <= '9') handleNumber(e.key);
    else if (e.key === '.') handleDecimal();
    else if (e.key === '+') handleOperator('+');
    else if (e.key === '-') handleOperator('−');
    else if (e.key === '*') handleOperator('×');
    else if (e.key === '/') { e.preventDefault(); handleOperator('÷'); }
    else if (e.key === 'Enter' || e.key === '=') calculate();
    else if (e.key === 'Escape') handleClear();
    else if (e.key === 'Backspace') {
      if (current.length > 1) current = current.slice(0,-1);
      else current = '0';
      updateDisplay();
    }
    else if (e.key === '%') handlePercent();
  });

  // Button clicks
  document.querySelectorAll('button').forEach(btn => {
    btn.addEventListener('click', e => {
      // Ripple
      const rect = btn.getBoundingClientRect();
      const rip = document.createElement('span');
      rip.className = 'ripple';
      const size = Math.max(rect.width, rect.height);
      rip.style.cssText = `width:${size}px;height:${size}px;left:${e.clientX-rect.left-size/2}px;top:${e.clientY-rect.top-size/2}px`;
      btn.appendChild(rip);
      rip.addEventListener('animationend', () => rip.remove());

      const action = btn.dataset.action;
      const val    = btn.dataset.value;
      if (action === 'number')   handleNumber(val);
      if (action === 'operator') handleOperator(val);
      if (action === 'equals')   calculate();
      if (action === 'clear')    handleClear();
      if (action === 'sign')     handleSign();
      if (action === 'percent')  handlePercent();
      if (action === 'decimal')  handleDecimal();
    });
  });

  updateDisplay();
</script>
</body>
</html>
