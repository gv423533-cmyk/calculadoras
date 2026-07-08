[index.html](https://github.com/user-attachments/files/29780508/index.html)
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#0b1420">
<title>Calculadoras de Instrumentação</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
  html, body { height: 100%; }

  :root {
    --bg:           #0a0f18;
    --bg-soft:      #0e1622;
    --panel:        #121b29;
    --panel-2:      #16202f;
    --line:         #22314770;
    --line-strong:  #2c405a;
    --ink:          #e7edf5;
    --ink-dim:      #93a3ba;
    --ink-faint:    #5c6d85;
    --accent:       #00d3ff;
    --accent-dim:   #0090b8;
    --accent-glow:  #00d3ff33;
    --yellow:       #ffb020;
    --yellow-bg:    #ffb02018;
    --yellow-border:#7a5a1c;
    --yellow-glow:  #ffb02033;
    --red:          #ff5470;
    --red-bg:       #ff547018;
    --red-border:   #7a2536;
    --green:        #33d17a;
    --green-bg:     #33d17a18;
    --radius:       10px;
    --radius-lg:    16px;
    --font-ui:      -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    --font-mono:    "SF Mono", "Cascadia Code", "Consolas", "Roboto Mono", ui-monospace, monospace;
  }

  body {
    font-family: var(--font-ui);
    background: var(--bg);
    color: var(--ink);
    min-height: 100vh;
    -webkit-font-smoothing: antialiased;
  }

  .app-shell { max-width: 640px; margin: 0 auto; padding: 24px 16px 48px; }

  /* ===== MENU ===== */
  .menu-card {
    background: linear-gradient(155deg, var(--panel-2), var(--panel));
    border: 1px solid var(--line-strong);
    border-radius: var(--radius-lg);
    overflow: hidden;
  }
  .menu-header {
    background: linear-gradient(180deg, #0c1f28, #081319);
    color: var(--ink);
    padding: 30px 24px 24px;
    text-align: center;
    border-bottom: 1px solid var(--line);
  }
  .menu-header h1 { font-size: 1.35rem; font-weight: 700; color: var(--accent); text-shadow: 0 0 10px var(--accent-glow); }
  .menu-header p { margin-top: 8px; font-size: 0.82rem; color: var(--ink-dim); }
  .menu-body { padding: 20px; }
  .calc-option {
    display: flex; align-items: center; gap: 14px;
    width: 100%; text-align: left;
    background: linear-gradient(155deg, var(--panel-2), var(--panel));
    border: 1px solid var(--line-strong);
    border-radius: var(--radius-lg);
    padding: 14px 16px;
    margin-bottom: 10px;
    cursor: pointer;
    transition: transform 0.15s, border-color 0.15s, background 0.15s;
    font-family: var(--font-ui);
    position: relative;
    overflow: hidden;
  }
  .calc-option::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(120px 80px at 15% 50%, var(--accent-glow), transparent 70%);
    opacity: 0;
    transition: opacity 0.15s;
  }
  .calc-option:hover { border-color: var(--accent-dim); transform: translateY(-1px); }
  .calc-option:hover::before { opacity: 1; }
  .calc-icon {
    flex-shrink: 0; width: 42px; height: 42px;
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 20px; background: var(--accent-glow); color: var(--accent);
  }
  .icon-vazao   { background: #1D6FB822; color: #60b8ff; }
  .icon-corrente{ background: #00B0F022; color: #00d3ff; }
  .icon-rtd     { background: var(--red-bg); color: var(--red); }
  .icon-rotacao { background: #7C3AED22; color: #c084fc; }
  .icon-vpressao{ background: var(--green-bg); color: var(--green); }
  .icon-pv      { background: #f9731622; color: #fb923c; }
  .icon-conv    { background: #0891b222; color: #22d3ee; font-size:18px; font-weight:700; font-family:var(--font-mono); }
  .calc-text .calc-title { font-size: 14.5px; font-weight: 700; color: var(--ink); margin-bottom: 2px; }
  .calc-text .calc-desc  { font-size: 12px; color: var(--ink-dim); }
  .calc-arrow { margin-left: auto; color: var(--ink-faint); font-size: 18px; flex-shrink: 0; }
  .menu-footer {
    text-align: center; padding: 14px;
    font-size: 0.75rem; color: var(--ink-faint);
    border-top: 1px solid var(--line);
  }

  /* ===== BOTÃO VOLTAR ===== */
  .back-bar { display: flex; align-items: center; margin-bottom: 14px; }
  .back-btn {
    background: var(--panel); color: var(--accent);
    border: 1px solid var(--line-strong);
    border-radius: var(--radius); padding: 8px 14px;
    font-size: 13px; font-weight: 600; cursor: pointer;
    display: flex; align-items: center; gap: 6px;
    transition: background 0.15s, border-color 0.15s;
    font-family: var(--font-ui);
  }
  .back-btn:hover { background: var(--panel-2); border-color: var(--accent-dim); }

  /* ===== CABEÇALHO DE TELA ===== */
  .header { margin-bottom: 1.25rem; }
  .header h1 { font-size: 18px; font-weight: 700; color: var(--accent); margin-bottom: 4px; text-shadow: 0 0 8px var(--accent-glow); }
  .header p  { font-size: 12.5px; color: var(--ink-dim); }

  /* ===== CARDS ===== */
  .card {
    background: linear-gradient(155deg, var(--panel-2), var(--panel));
    border: 1px solid var(--line-strong);
    border-radius: var(--radius-lg);
    padding: 1.2rem;
    margin-bottom: 0.9rem;
  }
  .card-header {
    display: flex; align-items: center; gap: 8px;
    margin-bottom: 1rem; padding-bottom: 0.75rem;
    border-bottom: 1px solid var(--line);
  }
  .card-title { font-size: 14px; font-weight: 700; color: var(--ink); }
  .badge { font-size: 11px; font-weight: 700; padding: 3px 9px; border-radius: 20px; font-family: var(--font-mono); }
  .badge-yellow { background: var(--yellow-bg); color: var(--yellow); border: 1px solid var(--yellow-border); }
  .badge-red    { background: var(--red-bg);    color: var(--red);    border: 1px solid var(--red-border); }

  /* ===== CAMPOS EDITÁVEIS ===== */
  .container { max-width: 640px; margin: 0 auto; }
  .fields { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 12px; }
  .field { display: flex; flex-direction: column; gap: 5px; }
  .field label { font-size: 11.5px; font-weight: 600; color: var(--ink-dim); text-transform: uppercase; letter-spacing: 0.03em; }
  .field input {
    border: 1.5px solid var(--yellow-border);
    border-radius: var(--radius);
    padding: 9px 11px;
    font-size: 16px; font-weight: 500;
    background: var(--yellow-bg); color: var(--ink);
    outline: none; width: 100%;
    transition: border-color 0.15s, box-shadow 0.15s;
    font-family: var(--font-mono);
  }
  .field input:focus { border-color: var(--yellow); box-shadow: 0 0 0 3px var(--yellow-glow); }
  .field input:disabled { background: var(--bg-soft); border-color: var(--line-strong); color: var(--ink-faint); cursor: not-allowed; }

  /* ===== RESULTADOS ===== */
  .results-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 10px; margin-bottom: 1rem; }
  .result-item { background: var(--bg-soft); border: 1px solid var(--line-strong); border-radius: var(--radius); padding: 0.75rem 1rem; }
  .result-label { font-size: 10.5px; color: var(--ink-faint); font-weight: 600; margin-bottom: 4px; text-transform: uppercase; letter-spacing: 0.04em; }
  .result-value { font-size: 20px; font-weight: 700; color: var(--ink); font-family: var(--font-mono); }
  .result-unit  { font-size: 11px; color: var(--ink-faint); margin-top: 2px; }
  .formula      { font-size: 11px; font-family: var(--font-mono); color: var(--ink-faint); background: #ffffff0a; border: 1px solid var(--line); border-radius: 4px; padding: 2px 6px; display: inline-block; margin-top: 5px; }

  .corrente-block { margin-bottom: 1rem; }
  .corrente-label { font-size: 12px; color: var(--ink-dim); margin-bottom: 2px; text-transform: uppercase; letter-spacing: 0.04em; }
  .corrente-value { display: flex; align-items: baseline; gap: 8px; }
  .corrente-num   { font-size: 38px; font-weight: 700; color: var(--accent); font-family: var(--font-mono); text-shadow: 0 0 12px var(--accent-glow); }
  .corrente-unit  { font-size: 16px; color: var(--ink-dim); }
  .divider { height: 1px; background: var(--line); margin: 1rem 0; }

  .summary .row { display: flex; justify-content: space-between; align-items: center; font-size: 13px; color: var(--ink-dim); padding: 6px 0; border-bottom: 1px solid var(--line); }
  .summary .row:last-child { border-bottom: none; }
  .summary .row .val { font-weight: 600; color: var(--ink); font-family: var(--font-mono); font-size: 13px; }
  .summary .row.accent-row .val { color: var(--accent); font-size: 15px; text-shadow: 0 0 8px var(--accent-glow); }

  .footer { text-align: center; font-size: 11px; color: var(--ink-faint); margin-top: 1.5rem; }

  /* ===== TOGGLE FATOR K ===== */
  .k-question { font-size: 12.5px; color: var(--ink-dim); margin-bottom: 8px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.03em; }
  .k-toggle { display: flex; gap: 8px; margin-bottom: 10px; }
  .k-btn {
    flex: 1; padding: 8px 10px; font-size: 12.5px; font-weight: 600;
    border-radius: var(--radius); border: 1px solid var(--line-strong);
    background: var(--bg-soft); color: var(--ink-dim); cursor: pointer;
    transition: all 0.15s; text-align: center; font-family: var(--font-ui);
  }
  .k-btn.active { background: var(--accent-glow); color: var(--accent); border-color: var(--accent-dim); }
  .k-btn:hover:not(.active) { border-color: var(--ink-faint); color: var(--ink); }
  .k-auto-info {
    font-size: 11.5px; color: var(--ink-faint); background: #ffffff08;
    border: 1px solid var(--line); border-radius: var(--radius); padding: 7px 10px; margin-top: 6px;
    font-family: var(--font-mono);
  }

  /* ===== CARDS SIMPLES (corrente de processo / RTD / rotação) ===== */
  .simple-card {
    background: linear-gradient(155deg, var(--panel-2), var(--panel));
    border: 1px solid var(--line-strong);
    border-radius: var(--radius-lg);
    overflow: hidden;
    max-width: 460px;
    margin: 0 auto;
  }
  .simple-card header {
    background: linear-gradient(180deg, #0c1f28, #081319);
    color: var(--ink);
    padding: 26px 24px 20px;
    text-align: center;
    border-bottom: 1px solid var(--line);
  }
  .simple-card header h1 { font-size: 1.35rem; color: var(--accent); text-shadow: 0 0 10px var(--accent-glow); }
  .simple-card header p { margin-top: 8px; font-size: 0.82rem; color: var(--ink-dim); }
  .simple-body { padding: 24px; }
  .sfield { margin-bottom: 16px; }
  .sfield label { display: block; font-size: 11.5px; font-weight: 700; color: var(--ink-dim); margin-bottom: 6px; text-transform: uppercase; letter-spacing: 0.04em; }
  .input-with-unit { position: relative; display: flex; align-items: center; }
  .sfield input {
    width: 100%; padding: 12px 44px 12px 14px; font-size: 1.05rem;
    border: 1.5px solid var(--line-strong); border-radius: var(--radius);
    background: var(--bg-soft); color: var(--ink);
    outline: none; transition: border-color 0.15s, box-shadow 0.15s;
    font-family: var(--font-mono);
  }
  .sfield input:focus { border-color: var(--accent-dim); box-shadow: 0 0 0 3px var(--accent-glow); }
  .unit-tag { position: absolute; right: 14px; font-size: 0.82rem; font-weight: 600; color: var(--ink-faint); pointer-events: none; font-family: var(--font-mono); }
  .syellow input { background: var(--yellow-bg); border-color: var(--yellow-border); color: var(--ink); }
  .syellow input:focus { border-color: var(--yellow); box-shadow: 0 0 0 3px var(--yellow-glow); }
  .syellow label::before { content: "● "; color: var(--yellow); }
  .sred input { background: var(--red-bg); border-color: var(--red-border); color: var(--ink-dim); }
  .sred label::before { content: "● "; color: var(--red); }
  .sresult {
    margin-top: 8px; margin-bottom: 12px;
    background: var(--accent-glow);
    border: 1px solid var(--accent-dim);
    border-radius: var(--radius); padding: 18px; text-align: center;
  }
  .sresult .lbl { font-size: 0.82rem; color: var(--ink-dim); text-transform: uppercase; letter-spacing: 0.06em; }
  .sresult .val { font-size: 2.4rem; font-weight: 700; margin-top: 4px; color: var(--accent); font-family: var(--font-mono); text-shadow: 0 0 12px var(--accent-glow); }
  .sresult .unit { font-size: 1rem; font-weight: 400; color: var(--ink-dim); }
  .sformula { margin-top: 16px; font-size: 0.78rem; color: var(--ink-faint); text-align: center; line-height: 1.6; font-family: var(--font-mono); }
  .slegend { display: flex; gap: 14px; justify-content: center; margin-top: 14px; font-size: 0.72rem; color: var(--ink-dim); flex-wrap: wrap; }
  .slegend span::before { content: "■ "; }
  .sly::before { color: var(--yellow); }
  .slr::before { color: var(--red); }
  .slb::before { color: var(--accent); }
  .simple-card footer { text-align: center; padding: 14px; font-size: 0.75rem; color: var(--ink-faint); border-top: 1px solid var(--line); }

  /* resultado roxo da rotação hex */
  .sresult-purple {
    margin-top: 8px; margin-bottom: 12px;
    background: #7C3AED22;
    border: 1px solid #7C3AED55;
    border-radius: var(--radius); padding: 18px; text-align: center;
  }
  .sresult-purple .lbl { font-size: 0.82rem; color: var(--ink-dim); text-transform: uppercase; letter-spacing: 0.06em; }
  .sresult-purple .val { font-size: 2.4rem; font-weight: 700; margin-top: 4px; color: #c084fc; font-family: var(--font-mono); text-shadow: 0 0 12px #7C3AED55; }

  .screen { display: none; }
  .screen.active { display: block; }

  /* ===== SELECT UNIDADE ===== */
  .sel-unidade {
    width: 100%;
    padding: 10px 14px;
    font-size: 1rem;
    font-weight: 600;
    border-radius: var(--radius);
    border: 1.5px solid var(--yellow-border);
    background: #0e1f14;
    color: var(--yellow);
    outline: none;
    font-family: var(--font-mono);
    cursor: pointer;
    appearance: none;
    -webkit-appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%23ffb020' stroke-width='1.8' fill='none' stroke-linecap='round'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right 12px center;
    padding-right: 36px;
    transition: border-color 0.15s, box-shadow 0.15s;
  }
  .sel-unidade:focus {
    border-color: var(--yellow);
    box-shadow: 0 0 0 3px var(--yellow-glow);
  }
  .sel-unidade option {
    background: #0e1f14;
    color: var(--ink);
    font-family: var(--font-mono);
    padding: 8px 12px;
  }
</style>
<link rel="manifest" href="data:application/json,{%22name%22:%22Calculadoras%20de%20Instrumenta%C3%A7%C3%A3o%22,%22short_name%22:%22Calc%20Instr%22,%22display%22:%22standalone%22,%22background_color%22:%22%230a0f18%22,%22theme_color%22:%22%2300d3ff%22,%22icons%22:[{%22src%22:%22data:image/svg+xml,%3Csvg%20xmlns%3D%27http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%27%20viewBox%3D%270%200%20100%20100%27%3E%3Crect%20width%3D%27100%27%20height%3D%27100%27%20rx%3D%2720%27%20fill%3D%27%230a0f18%27%2F%3E%3Ctext%20x%3D%2750%27%20y%3D%2765%27%20font-size%3D%2750%27%20text-anchor%3D%27middle%27%3E%E2%9A%A1%3C%2Ftext%3E%3C%2Fsvg%3E%22,%22sizes%22:%22192x192%22,%22type%22:%22image/svg+xml%22}]}">
</head>
<body>
<div class="app-shell">

  <!-- ===================== TELA DE MENU ===================== -->
  <div class="screen active" id="menu-screen">
    <div class="menu-card">
      <div class="menu-header">
        <h1>Calculadoras de Instrumentação</h1>
        <p>Escolha a calculadora que deseja utilizar</p>
      </div>
      <div class="menu-body">
        <button class="calc-option" onclick="showScreen('calc-vpressao')">
          <div class="calc-icon icon-vpressao">🔄</div>
          <div class="calc-text">
            <div class="calc-title">Conversão Vazão → Pressão</div>
            <div class="calc-desc">Vazão para Pressão com saída 4–20 mA</div>
          </div>
          <div class="calc-arrow">›</div>
        </button>

        <button class="calc-option" onclick="showScreen('calc-pvazao')">
          <div class="calc-icon icon-vazao">🌊</div>
          <div class="calc-text">
            <div class="calc-title">Conversão Pressão → Vazão</div>
            <div class="calc-desc">TRM pressão diferencial — vazão real e saída 4–20 mA</div>
          </div>
          <div class="calc-arrow">›</div>
        </button>

        <button class="calc-option" onclick="showScreen('calc-corrente')">
          <div class="calc-icon icon-corrente">⚡</div>
          <div class="calc-text">
            <div class="calc-title">PV × Corrente</div>
            <div class="calc-desc">Converte variável de processo para corrente 4–20 mA</div>
          </div>
          <div class="calc-arrow">›</div>
        </button>

        <button class="calc-option" onclick="showScreen('calc-pv')">
          <div class="calc-icon icon-pv">📡</div>
          <div class="calc-text">
            <div class="calc-title">Corrente × PV</div>
            <div class="calc-desc">Converte corrente 4–20 mA para variável de processo</div>
          </div>
          <div class="calc-arrow">›</div>
        </button>

        <button class="calc-option" onclick="showScreen('calc-rtd')">
          <div class="calc-icon icon-rtd">🌡️</div>
          <div class="calc-text">
            <div class="calc-title">Calculadora RTD</div>
            <div class="calc-desc">Temperatura a partir da leitura do PT100</div>
          </div>
          <div class="calc-arrow">›</div>
        </button>

        <button class="calc-option" onclick="showScreen('calc-rotacao')">
          <div class="calc-icon icon-rotacao">⚙️</div>
          <div class="calc-text">
            <div class="calc-title">Rotação do Motor</div>
            <div class="calc-desc">Conversão de velocidade nominal para RPM e decimal</div>
          </div>
          <div class="calc-arrow">›</div>
        </button>

        <button class="calc-option" onclick="showScreen('calc-conv')">
          <div class="calc-icon icon-conv">⇄</div>
          <div class="calc-text">
            <div class="calc-title">Conversão de Pressão</div>
            <div class="calc-desc">Converte entre 15 unidades de pressão conforme tabela de referência</div>
          </div>
          <div class="calc-arrow">›</div>
        </button>
      </div>
      <div class="menu-footer">Desenvolvido por <strong>Gabriel Vinicius Martins Oliveira</strong></div>
    </div>
  </div>

  <!-- ===================== CALC 2: PV X CORRENTE ===================== -->
  <div class="screen" id="calc-corrente">
    <div class="back-bar"><button class="back-btn" onclick="showScreen('menu-screen')">‹ Voltar ao menu</button></div>
    <div class="simple-card">
      <header>
        <h1>PV × Corrente</h1>
        <p>Criada por Gabriel Vinicius Martins Oliveira</p>
      </header>
      <div class="simple-body">

        <div class="sfield syellow">
          <label for="pv2-unidade">Unidade de medida do processo</label>
          <select id="pv2-unidade" onchange="syncUnidade('pv2')" class="sel-unidade">
            <option selected value="mmH2O">mmH2O &#8212; Milímetros de coluna d&#39;água</option>
            <option value="MCA">MCA &#8212; Metros de coluna d&#39;água</option>
            <option value="MPa">MPa &#8212; Megapascal</option>
            <option value="kPa">kPa &#8212; Kilopascal</option>
            <option value="Pa">Pa &#8212; Pascal</option>
            <option value="Bar">Bar &#8212; Bar</option>
            <option value="mbar">mbar &#8212; Milibar</option>
            <option value="kgf/cm²">kgf/cm² &#8212; Quilograma-força por centímetro quadrado</option>
            <option value="PSI">PSI &#8212; Libra-força por polegada quadrada</option>
            <option value="atm">atm &#8212; Atmosfera padrão</option>
            <option value="inH2O">inH&#8322;O &#8212; Polegada de coluna d&#39;água</option>
            <option value="cmH2O">cmH&#8322;O &#8212; Centímetro de coluna d&#39;água</option>
            <option value="inHg">inHg &#8212; Polegada de mercúrio</option>
            <option value="mmHg">mmHg &#8212; Milímetro de mercúrio</option>
            <option value="cmHg">cmHg &#8212; Centímetro de mercúrio</option>
            <option value="°C">°C &#8212; Grau Celsius</option>
            <option value="°F">°F &#8212; Grau Fahrenheit</option>
            <option value="K">K &#8212; Kelvin</option>
            <option value="m³/h">m³/h &#8212; Metro cúbico por hora</option>
            <option value="m³/min">m³/min &#8212; Metro cúbico por minuto</option>
            <option value="m³/s">m³/s &#8212; Metro cúbico por segundo</option>
            <option value="l/h">l/h &#8212; Litro por hora</option>
            <option value="l/min">l/min &#8212; Litro por minuto</option>
            <option value="l/s">l/s &#8212; Litro por segundo</option>
            <option value="ton/h">ton/h &#8212; Tonelada por hora</option>
            <option value="kg/h">kg/h &#8212; Quilograma por hora</option>
            <option value="kg/min">kg/min &#8212; Quilograma por minuto</option>
            <option value="kg/s">kg/s &#8212; Quilograma por segundo</option>
            <option value="S/m">S/m &#8212; Siemens por metro</option>
            <option value="µS/m">µS/m &#8212; Microsiemens por metro</option>
            <option value="µS/cm">µS/cm &#8212; Microsiemens por centímetro</option>
            <option value="mS/m">mS/m &#8212; Milisiemens por metro</option>
            <option value="mS/cm">mS/cm &#8212; Milisiemens por centímetro</option>
            <option value="%">% &#8212; Percentual</option>
          </select>
        </div>

        <div class="sfield syellow">
          <label for="pv-atual">PV atual</label>
          <div class="input-with-unit">
            <input type="number" id="pv-atual" value="50" step="any" style="padding-right:70px;" inputmode="decimal">
            <span class="unit-tag" id="pv2-tag-atual">mmH2O</span>
          </div>
        </div>
        <div class="sfield syellow">
          <label for="pv2-min">PV mínimo (0% da escala)</label>
          <div class="input-with-unit">
            <input type="number" id="pv2-min" value="0" step="any" style="padding-right:70px;" inputmode="decimal">
            <span class="unit-tag" id="pv2-tag-min">mmH2O</span>
          </div>
        </div>
        <div class="sfield syellow">
          <label for="pv2-max">PV máximo (100% da escala)</label>
          <div class="input-with-unit">
            <input type="number" id="pv2-max" value="100" step="any" style="padding-right:70px;" inputmode="decimal">
            <span class="unit-tag" id="pv2-tag-max">mmH2O</span>
          </div>
        </div>
        <div class="sfield sred">
          <label for="pv2-imin">Corrente mínima — fixo</label>
          <div class="input-with-unit">
            <input type="number" id="pv2-imin" value="4" readonly>
            <span class="unit-tag">mA</span>
          </div>
        </div>
        <div class="sfield sred">
          <label for="pv2-imax">Corrente máxima — fixo</label>
          <div class="input-with-unit">
            <input type="number" id="pv2-imax" value="20" readonly>
            <span class="unit-tag">mA</span>
          </div>
        </div>

        <div class="sresult">
          <div class="lbl">Corrente de saída</div>
          <div class="val"><span id="out-proc">--</span> <span class="unit">mA</span></div>
        </div>

        <div class="sformula">
          I = (PV_atual − PV_min) × (I_max − I_min) ÷ (PV_max − PV_min) + I_min
        </div>
        <div class="slegend">
          <span class="sly">Editável</span>
          <span class="slr">Fixo</span>
          <span class="slb">Resultado</span>
        </div>
      </div>
      <footer>© Gabriel Vinicius Martins Oliveira</footer>
    </div>
  </div>

  <!-- ===================== CALC 3: RTD ===================== -->
  <div class="screen" id="calc-rtd">
    <div class="back-bar"><button class="back-btn" onclick="showScreen('menu-screen')">‹ Voltar ao menu</button></div>
    <div class="simple-card">
      <header>
        <h1>Calculadora RTD</h1>
        <p>Criada por Gabriel Vinicius Martins Oliveira</p>
      </header>
      <div class="simple-body">
        <div class="sfield syellow">
          <label for="rtd">RTD — valor medido no multímetro</label>
          <div class="input-with-unit">
            <input type="number" id="rtd" value="138.5" step="any" inputmode="decimal">
            <span class="unit-tag">Ω</span>
          </div>
        </div>
        <div class="sfield sred">
          <label for="r0">Resistência a 0°C — fixo</label>
          <div class="input-with-unit">
            <input type="number" id="r0" value="100" readonly>
            <span class="unit-tag">Ω</span>
          </div>
        </div>
        <div class="sfield sred">
          <label for="fator">Fator de conversão — fixo</label>
          <div class="input-with-unit">
            <input type="number" id="fator" value="0.385" readonly>
          </div>
        </div>

        <div class="sresult">
          <div class="lbl">Temperatura</div>
          <div class="val"><span id="out-rtd">--</span> <span class="unit">°C</span></div>
        </div>

        <div class="sformula">
          Temperatura = (RTD − Resistência a 0°C) ÷ Fator de conversão
        </div>
        <div class="slegend">
          <span class="sly">Editável</span>
          <span class="slr">Fixo</span>
          <span class="slb">Resultado</span>
        </div>
      </div>
      <footer>© Gabriel Vinicius Martins Oliveira</footer>
    </div>
  </div>

  <!-- ===================== CALC 4: ROTAÇÃO DO MOTOR ===================== -->
  <div class="screen" id="calc-rotacao">
    <div class="back-bar"><button class="back-btn" onclick="showScreen('menu-screen')">‹ Voltar ao menu</button></div>
    <div class="simple-card">
      <header>
        <h1>Calculadora de Rotação do Motor</h1>
        <p>Criada por Gabriel Vinicius Martins Oliveira</p>
      </header>
      <div class="simple-body">
        <div class="sfield syellow">
          <label for="ref">Referência</label>
          <input type="number" id="ref" value="100" step="any" inputmode="decimal">
        </div>
        <div class="sfield syellow">
          <label for="velnom">Velocidade Nominal (Placa do motor)</label>
          <div class="input-with-unit">
            <input type="number" id="velnom" value="1800" step="any" inputmode="decimal">
            <span class="unit-tag">RPM</span>
          </div>
        </div>
        <div class="sfield sred">
          <label for="velnomdec">Velocidade nominal (Decimal) — fixo</label>
          <input type="number" id="velnomdec" value="8192" readonly>
        </div>

        <div class="sresult" style="margin-bottom:12px;">
          <div class="lbl">Velocidade em Decimal</div>
          <div class="val"><span id="out-veldec">--</span></div>
        </div>
        <div class="sresult-purple" style="margin-bottom:12px;">
          <div class="lbl">Velocidade em Hexadecimal</div>
          <div class="val"><span id="out-velhex">--</span></div>
        </div>
        <div class="sresult">
          <div class="lbl">Velocidade em RPM</div>
          <div class="val"><span id="out-velrpm">--</span> <span class="unit">RPM</span></div>
        </div>

        <div class="sformula">
          Velocidade em Decimal = Referência × Velocidade nominal (Decimal) ÷ 100<br>
          Velocidade em Hexadecimal = conversão do valor decimal para hex<br>
          Velocidade em RPM = Referência × Velocidade Nominal (Placa) ÷ 100
        </div>
        <div class="slegend">
          <span class="sly">Editável</span>
          <span class="slr">Fixo</span>
          <span class="slb">Resultado</span>
        </div>
      </div>
      <footer>© Gabriel Vinicius Martins Oliveira</footer>
    </div>
  </div>

  <!-- ===================== CALC 5: CONVERSÃO VAZÃO → PRESSÃO ===================== -->
  <div class="screen" id="calc-vpressao">
    <div class="back-bar"><button class="back-btn" onclick="showScreen('menu-screen')">‹ Voltar ao menu</button></div>
    <div class="container">
      <div class="header">
        <h1>Conversão Vazão → Pressão</h1>
        <p>Preencha os campos amarelos — os resultados são calculados automaticamente.</p>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge badge-yellow">Editável</span>
          <span class="card-title">Parâmetros de entrada</span>
        </div>
        <div class="fields">
          <div class="field" style="grid-column:1/-1;">
            <label for="v2p-unidade">Unidade de medida do processo</label>
            <select id="v2p-unidade" onchange="syncUnidade('v2p')" class="sel-unidade">
            <option selected value="mmH2O">mmH2O &#8212; Milímetros de coluna d&#39;água</option>
            <option value="MCA">MCA &#8212; Metros de coluna d&#39;água</option>
            <option value="MPa">MPa &#8212; Megapascal</option>
            <option value="kPa">kPa &#8212; Kilopascal</option>
            <option value="Pa">Pa &#8212; Pascal</option>
            <option value="Bar">Bar &#8212; Bar</option>
            <option value="mbar">mbar &#8212; Milibar</option>
            <option value="kgf/cm²">kgf/cm² &#8212; Quilograma-força por centímetro quadrado</option>
            <option value="PSI">PSI &#8212; Libra-força por polegada quadrada</option>
            <option value="atm">atm &#8212; Atmosfera padrão</option>
            <option value="inH2O">inH&#8322;O &#8212; Polegada de coluna d&#39;água</option>
            <option value="cmH2O">cmH&#8322;O &#8212; Centímetro de coluna d&#39;água</option>
            <option value="inHg">inHg &#8212; Polegada de mercúrio</option>
            <option value="mmHg">mmHg &#8212; Milímetro de mercúrio</option>
            <option value="cmHg">cmHg &#8212; Centímetro de mercúrio</option>
            <option value="°C">°C &#8212; Grau Celsius</option>
            <option value="°F">°F &#8212; Grau Fahrenheit</option>
            <option value="K">K &#8212; Kelvin</option>
            <option value="m³/h">m³/h &#8212; Metro cúbico por hora</option>
            <option value="m³/min">m³/min &#8212; Metro cúbico por minuto</option>
            <option value="m³/s">m³/s &#8212; Metro cúbico por segundo</option>
            <option value="l/h">l/h &#8212; Litro por hora</option>
            <option value="l/min">l/min &#8212; Litro por minuto</option>
            <option value="l/s">l/s &#8212; Litro por segundo</option>
            <option value="ton/h">ton/h &#8212; Tonelada por hora</option>
            <option value="kg/h">kg/h &#8212; Quilograma por hora</option>
            <option value="kg/min">kg/min &#8212; Quilograma por minuto</option>
            <option value="kg/s">kg/s &#8212; Quilograma por segundo</option>
            <option value="S/m">S/m &#8212; Siemens por metro</option>
            <option value="µS/m">µS/m &#8212; Microsiemens por metro</option>
            <option value="µS/cm">µS/cm &#8212; Microsiemens por centímetro</option>
            <option value="mS/m">mS/m &#8212; Milisiemens por metro</option>
            <option value="mS/cm">mS/cm &#8212; Milisiemens por centímetro</option>
            <option value="%">% &#8212; Percentual</option>
          </select>
          </div>
          <div class="field">
            <label for="vp-q">Vazão medida</label>
            <div class="input-with-unit">
              <input type="number" id="vp-q" value="75" step="any" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="v2p-tag-q">mmH2O</span>
            </div>
          </div>
          <div class="field">
            <label for="vp-v2p-span">Span (Pressão diferencial máxima)</label>
            <div class="input-with-unit">
              <input type="number" id="vp-v2p-span" value="5000" step="any" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="v2p-tag-span">mmH2O</span>
            </div>
          </div>
          <div class="field">
            <label for="vp-v2p-qmax">Span Vazão (Vazão máxima)</label>
            <div class="input-with-unit">
              <input type="number" id="vp-v2p-qmax" value="150" step="any" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="v2p-tag-qmax">mmH2O</span>
            </div>
          </div>
          <div class="field" style="grid-column: 1 / -1;">
            <div class="k-question">Você possui o Fator K de projeto?</div>
            <div class="k-toggle">
              <button class="k-btn active" id="vp-v2p-btn-manual" onclick="setKModeVP('v2p','manual')">Sim — inserir manualmente</button>
              <button class="k-btn"        id="vp-v2p-btn-auto"   onclick="setKModeVP('v2p','auto')">Não — calcular automaticamente</button>
            </div>
            <label for="vp-v2p-k">Fator K</label>
            <input type="number" id="vp-v2p-k" value="6" step="0.1" inputmode="decimal">
            <div class="k-auto-info" id="vp-v2p-k-info" style="display:none;">K = Span Vazão ÷ √(Span Pressão)</div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge badge-red">Calculado</span>
          <span class="card-title">Resultados</span>
        </div>
        <div class="corrente-block">
          <div class="corrente-label">Saída de corrente</div>
          <div class="corrente-value">
            <span class="corrente-num" id="vp-v2p-corrente">—</span>
            <span class="corrente-unit">mA</span>
          </div>
          <span class="formula">= (Vazão / SpanVazão) × 16 + 4</span>
        </div>
        <div class="divider"></div>
        <div class="results-grid">
          <div class="result-item">
            <div class="result-label">Pressão calculada</div>
            <div class="result-value" id="vp-v2p-pressao">—</div>
            <div class="result-unit">mesma unid. Span Pressão</div>
            <span class="formula">(Vazão / K)²</span>
          </div>
          <div class="result-item">
            <div class="result-label">Fator K utilizado</div>
            <div class="result-value" id="vp-v2p-k-out">—</div>
            <div class="result-unit">adimensional</div>
            <span class="formula">SpanVazão ÷ √(Span)</span>
          </div>
        </div>
      </div>

      <div class="card summary">
        <div class="card-header" style="margin-bottom:0.75rem;"><span class="card-title">Resumo</span></div>
        <div class="row"><span>Vazão medida</span><span class="val" id="vp-v2p-s-q">—</span></div>
        <div class="row"><span>Span Pressão</span><span class="val" id="vp-v2p-s-span">—</span></div>
        <div class="row"><span>Span Vazão</span><span class="val" id="vp-v2p-s-qmax">—</span></div>
        <div class="row"><span>Fator K</span><span class="val" id="vp-v2p-s-k">—</span></div>
        <div class="row"><span>Pressão calculada</span><span class="val" id="vp-v2p-s-pressao">—</span></div>
        <div class="row accent-row"><span>Corrente de saída</span><span class="val" id="vp-v2p-s-ma">—</span></div>
      </div>

      <div class="footer">Desenvolvido por <strong>Gabriel Vinicius Martins Oliveira</strong></div>
    </div>
  </div>

  <!-- ===================== CALC 6: CONVERSÃO PRESSÃO → VAZÃO ===================== -->
  <div class="screen" id="calc-pvazao">
    <div class="back-bar"><button class="back-btn" onclick="showScreen('menu-screen')">‹ Voltar ao menu</button></div>
    <div class="container">
      <div class="header">
        <h1>Conversão Pressão → Vazão</h1>
        <p>Preencha os campos amarelos — os resultados são calculados automaticamente.</p>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge badge-yellow">Editável</span>
          <span class="card-title">Parâmetros de entrada</span>
        </div>
        <div class="fields">
          <div class="field" style="grid-column:1/-1;">
            <label for="pvz-unidade">Unidade de medida do processo</label>
            <select id="pvz-unidade" onchange="syncUnidade('pvz')" class="sel-unidade">
            <option selected value="mmH2O">mmH2O &#8212; Milímetros de coluna d&#39;água</option>
            <option value="MCA">MCA &#8212; Metros de coluna d&#39;água</option>
            <option value="MPa">MPa &#8212; Megapascal</option>
            <option value="kPa">kPa &#8212; Kilopascal</option>
            <option value="Pa">Pa &#8212; Pascal</option>
            <option value="Bar">Bar &#8212; Bar</option>
            <option value="mbar">mbar &#8212; Milibar</option>
            <option value="kgf/cm²">kgf/cm² &#8212; Quilograma-força por centímetro quadrado</option>
            <option value="PSI">PSI &#8212; Libra-força por polegada quadrada</option>
            <option value="atm">atm &#8212; Atmosfera padrão</option>
            <option value="inH2O">inH&#8322;O &#8212; Polegada de coluna d&#39;água</option>
            <option value="cmH2O">cmH&#8322;O &#8212; Centímetro de coluna d&#39;água</option>
            <option value="inHg">inHg &#8212; Polegada de mercúrio</option>
            <option value="mmHg">mmHg &#8212; Milímetro de mercúrio</option>
            <option value="cmHg">cmHg &#8212; Centímetro de mercúrio</option>
            <option value="°C">°C &#8212; Grau Celsius</option>
            <option value="°F">°F &#8212; Grau Fahrenheit</option>
            <option value="K">K &#8212; Kelvin</option>
            <option value="m³/h">m³/h &#8212; Metro cúbico por hora</option>
            <option value="m³/min">m³/min &#8212; Metro cúbico por minuto</option>
            <option value="m³/s">m³/s &#8212; Metro cúbico por segundo</option>
            <option value="l/h">l/h &#8212; Litro por hora</option>
            <option value="l/min">l/min &#8212; Litro por minuto</option>
            <option value="l/s">l/s &#8212; Litro por segundo</option>
            <option value="ton/h">ton/h &#8212; Tonelada por hora</option>
            <option value="kg/h">kg/h &#8212; Quilograma por hora</option>
            <option value="kg/min">kg/min &#8212; Quilograma por minuto</option>
            <option value="kg/s">kg/s &#8212; Quilograma por segundo</option>
            <option value="S/m">S/m &#8212; Siemens por metro</option>
            <option value="µS/m">µS/m &#8212; Microsiemens por metro</option>
            <option value="µS/cm">µS/cm &#8212; Microsiemens por centímetro</option>
            <option value="mS/m">mS/m &#8212; Milisiemens por metro</option>
            <option value="mS/cm">mS/cm &#8212; Milisiemens por centímetro</option>
            <option value="%">% &#8212; Percentual</option>
          </select>
          </div>
          <div class="field">
            <label for="pv-pv">PV — Pressão de processo</label>
            <div class="input-with-unit">
              <input type="number" id="pv-pv" value="625" step="1" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="pvz-tag-pv">mmH2O</span>
            </div>
          </div>
          <div class="field">
            <label for="pv-spam">Pressão diferencial máxima</label>
            <div class="input-with-unit">
              <input type="number" id="pv-spam" value="3000" step="1" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="pvz-tag-span">mmH2O</span>
            </div>
          </div>
          <div class="field" style="grid-column: 1 / -1;">
            <div class="k-question">Você possui o Fator K de projeto?</div>
            <div class="k-toggle">
              <button class="k-btn active" id="pv-btn-manual" onclick="setKModePV('manual')">Sim — inserir manualmente</button>
              <button class="k-btn"        id="pv-btn-auto"   onclick="setKModePV('auto')">Não — calcular automaticamente</button>
            </div>
            <label for="pv-fatorK">Fator K</label>
            <input type="number" id="pv-fatorK" value="6" step="0.1" inputmode="decimal">
            <div class="k-auto-info" id="pv-k-auto-info" style="display:none;">K = Vazão máxima ÷ √(Pressão diferencial máxima)</div>
          </div>
          <div class="field">
            <label for="pv-qmax">Vazão máxima</label>
            <div class="input-with-unit">
              <input type="number" id="pv-qmax" value="300" step="1" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="pvz-tag-qmax">mmH2O</span>
            </div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge badge-red">Calculado</span>
          <span class="card-title">Saída de corrente (4–20 mA)</span>
        </div>
        <div class="corrente-block">
          <div class="corrente-label">Corrente de saída</div>
          <div class="corrente-value">
            <span class="corrente-num" id="pv-out-corrente">—</span>
            <span class="corrente-unit">mA</span>
          </div>
          <span class="formula">= (Raiz / 100 × 16) + 4</span>
        </div>
        <div class="divider"></div>
        <div class="results-grid">
          <div class="result-item">
            <div class="result-label">Percentual ΔP</div>
            <div class="result-value" id="pv-out-perc">—</div>
            <div class="result-unit">adimensional</div>
            <span class="formula">PV / Span</span>
          </div>
          <div class="result-item">
            <div class="result-label">Extração raiz quadrada</div>
            <div class="result-value" id="pv-out-raiz">—</div>
            <div class="result-unit">adimensional</div>
            <span class="formula">Const × √(ΔP%) × 10</span>
          </div>
          <div class="result-item">
            <div class="result-label">Constante de conversão</div>
            <div class="result-value">10</div>
            <div class="result-unit">fixo</div>
            <span class="formula">G2 = 10</span>
          </div>
          <div class="result-item">
            <div class="result-label">Vazão real</div>
            <div class="result-value" id="pv-out-vazao">—</div>
            <div class="result-unit">unid/h</div>
            <span class="formula">K × √(PV)</span>
          </div>
        </div>
      </div>

      <div class="card summary">
        <div class="card-header" style="margin-bottom:0.75rem;"><span class="card-title">Resumo</span></div>
        <div class="row"><span>PV</span><span class="val" id="pv-s-pv">—</span></div>
        <div class="row"><span>Pressão diferencial máxima</span><span class="val" id="pv-s-spam">—</span></div>
        <div class="row"><span>Fator K</span><span class="val" id="pv-s-k">—</span></div>
        <div class="row"><span>Vazão máxima</span><span class="val" id="pv-s-qmax">—</span></div>
        <div class="row accent-row"><span>Corrente de saída</span><span class="val" id="pv-s-ma">—</span></div>
      </div>

      <div class="footer">Desenvolvido por <strong>Gabriel Vinicius Martins Oliveira</strong></div>
    </div>
  </div>

  <!-- ===================== CALC 7: CORRENTE X PV ===================== -->
  <div class="screen" id="calc-pv">
    <div class="back-bar"><button class="back-btn" onclick="showScreen('menu-screen')">‹ Voltar ao menu</button></div>
    <div class="container">
      <div class="header">
        <h1>Corrente × PV</h1>
        <p>Preencha os campos amarelos — o PV é calculado automaticamente.</p>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge badge-yellow">Editável</span>
          <span class="card-title">Parâmetros de entrada</span>
        </div>
        <div class="fields">
          <div class="field" style="grid-column:1/-1;">
            <label for="cpv-unidade">Unidade de medida do processo</label>
            <select id="cpv-unidade" onchange="syncUnidade('cpv')" class="sel-unidade">
            <option selected value="mmH2O">mmH2O &#8212; Milímetros de coluna d&#39;água</option>
            <option value="MCA">MCA &#8212; Metros de coluna d&#39;água</option>
            <option value="MPa">MPa &#8212; Megapascal</option>
            <option value="kPa">kPa &#8212; Kilopascal</option>
            <option value="Pa">Pa &#8212; Pascal</option>
            <option value="Bar">Bar &#8212; Bar</option>
            <option value="mbar">mbar &#8212; Milibar</option>
            <option value="kgf/cm²">kgf/cm² &#8212; Quilograma-força por centímetro quadrado</option>
            <option value="PSI">PSI &#8212; Libra-força por polegada quadrada</option>
            <option value="atm">atm &#8212; Atmosfera padrão</option>
            <option value="inH2O">inH&#8322;O &#8212; Polegada de coluna d&#39;água</option>
            <option value="cmH2O">cmH&#8322;O &#8212; Centímetro de coluna d&#39;água</option>
            <option value="inHg">inHg &#8212; Polegada de mercúrio</option>
            <option value="mmHg">mmHg &#8212; Milímetro de mercúrio</option>
            <option value="cmHg">cmHg &#8212; Centímetro de mercúrio</option>
            <option value="°C">°C &#8212; Grau Celsius</option>
            <option value="°F">°F &#8212; Grau Fahrenheit</option>
            <option value="K">K &#8212; Kelvin</option>
            <option value="m³/h">m³/h &#8212; Metro cúbico por hora</option>
            <option value="m³/min">m³/min &#8212; Metro cúbico por minuto</option>
            <option value="m³/s">m³/s &#8212; Metro cúbico por segundo</option>
            <option value="l/h">l/h &#8212; Litro por hora</option>
            <option value="l/min">l/min &#8212; Litro por minuto</option>
            <option value="l/s">l/s &#8212; Litro por segundo</option>
            <option value="ton/h">ton/h &#8212; Tonelada por hora</option>
            <option value="kg/h">kg/h &#8212; Quilograma por hora</option>
            <option value="kg/min">kg/min &#8212; Quilograma por minuto</option>
            <option value="kg/s">kg/s &#8212; Quilograma por segundo</option>
            <option value="S/m">S/m &#8212; Siemens por metro</option>
            <option value="µS/m">µS/m &#8212; Microsiemens por metro</option>
            <option value="µS/cm">µS/cm &#8212; Microsiemens por centímetro</option>
            <option value="mS/m">mS/m &#8212; Milisiemens por metro</option>
            <option value="mS/cm">mS/cm &#8212; Milisiemens por centímetro</option>
            <option value="%">% &#8212; Percentual</option>
          </select>
          </div>
          <div class="field">
            <label for="pv-i-atual">I atual (corrente medida)</label>
            <div class="input-with-unit" style="position:relative;">
              <input type="number" id="pv-i-atual" value="12" step="any" inputmode="decimal" style="padding-right:44px;">
              <span class="unit-tag">mA</span>
            </div>
          </div>
          <div class="field">
            <label for="pv-min">PV mínimo (0% da escala)</label>
            <div class="input-with-unit" style="position:relative;">
              <input type="number" id="pv-min" value="0" step="any" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="cpv-tag-min">mmH2O</span>
            </div>
          </div>
          <div class="field">
            <label for="pv-max">PV máximo (100% da escala)</label>
            <div class="input-with-unit" style="position:relative;">
              <input type="number" id="pv-max" value="100" step="any" inputmode="decimal" style="padding-right:70px;">
              <span class="unit-tag" id="cpv-tag-max">mmH2O</span>
            </div>
          </div>
        </div>
        <div style="margin-top:14px; display:grid; grid-template-columns:1fr 1fr; gap:12px;">
          <div class="field">
            <label style="color:var(--red);">● I mínima — fixo</label>
            <div class="input-with-unit" style="position:relative;">
              <input type="number" value="4" readonly style="background:var(--red-bg); border-color:var(--red-border); color:var(--ink-dim); border-radius:var(--radius); padding:9px 44px 9px 11px; font-size:16px; font-family:var(--font-mono); border-width:1.5px; border-style:solid; width:100%; cursor:not-allowed;">
              <span class="unit-tag">mA</span>
            </div>
          </div>
          <div class="field">
            <label style="color:var(--red);">● I máxima — fixo</label>
            <div class="input-with-unit" style="position:relative;">
              <input type="number" value="20" readonly style="background:var(--red-bg); border-color:var(--red-border); color:var(--ink-dim); border-radius:var(--radius); padding:9px 44px 9px 11px; font-size:16px; font-family:var(--font-mono); border-width:1.5px; border-style:solid; width:100%; cursor:not-allowed;">
              <span class="unit-tag">mA</span>
            </div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge" style="background:#f9731622; color:#fb923c; border:1px solid #c2410c55;">Resultado</span>
          <span class="card-title">Variável de Processo (PV)</span>
        </div>
        <div class="corrente-block">
          <div class="corrente-label">PV calculado</div>
          <div class="corrente-value">
            <span class="corrente-num" id="pv-out" style="color:#fb923c; text-shadow:0 0 12px #f9731633;">—</span>
            <div class="corrente-unit" id="pv-out-unit">mmH2O</span>
          </div>
          <span class="formula">= (I_atual − I_min) × (PV_max − PV_min) / (I_max − I_min) + PV_min</span>
        </div>
        <div class="divider"></div>
        <div class="results-grid">
          <div class="result-item">
            <div class="result-label">Percentual de escala</div>
            <div class="result-value" id="cpv2-out-perc">—</div>
            <div class="result-unit">%</div>
            <span class="formula">(I_atual − 4) / 16 × 100</span>
          </div>
          <div class="result-item">
            <div class="result-label">Span da corrente</div>
            <div class="result-value">16</div>
            <div class="result-unit">mA (fixo)</div>
            <span class="formula">I_max − I_min</span>
          </div>
          <div class="result-item">
            <div class="result-label">Span do processo</div>
            <div class="result-value" id="pv-out-span">—</div>
            <div class="result-unit" id="cpv-span-unit">mmH2O</div>
            <span class="formula">PV_max − PV_min</span>
          </div>
        </div>
      </div>

      <div class="card summary">
        <div class="card-header" style="margin-bottom:0.75rem;"><span class="card-title">Resumo</span></div>
        <div class="row"><span>I atual</span><span class="val" id="pv-s-i">—</span></div>
        <div class="row"><span>PV mínimo</span><span class="val" id="pv-s-min">—</span></div>
        <div class="row"><span>PV máximo</span><span class="val" id="pv-s-max">—</span></div>
        <div class="row"><span>Percentual</span><span class="val" id="pv-s-perc">—</span></div>
        <div class="row accent-row" style="--accent:#fb923c;"><span>PV calculado</span><span class="val" id="cpv2-s-pv" style="color:#fb923c;">—</span></div>
      </div>

      <div class="footer">Desenvolvido por <strong>Gabriel Vinicius Martins Oliveira</strong></div>
    </div>
  </div>

    </div>

  <!-- ===================== CALC 8: CONVERSÃO DE PRESSÃO ===================== -->
  <div class="screen" id="calc-conv">
    <div class="back-bar"><button class="back-btn" onclick="showScreen('menu-screen')">&#8249; Voltar ao menu</button></div>
    <div class="container">
      <div class="header">
        <h1>Conversão de Pressão</h1>
        <p>Selecione as unidades, insira o valor e veja o resultado com a fórmula.</p>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge badge-yellow">Editável</span>
          <span class="card-title">Parâmetros de conversão</span>
        </div>
        <div class="fields">
          <div class="field" style="grid-column:1/-1;">
            <label for="conv-de">Unidade de origem</label>
            <select id="conv-de" class="sel-unidade" onchange="calcConversao()">
              <option value="PSI">PSI — Libra-força por polegada quadrada</option>
              <option value="atm">atm — Atmosfera padrão</option>
              <option value="inH2O">inH&#8322;O — Polegada de coluna d&#39;água</option>
              <option value="mmH2O">mm H&#8322;O — Milímetro de coluna d&#39;água</option>
              <option value="cmH2O">cmH&#8322;O — Centímetro de coluna d&#39;água</option>
              <option value="MCA">MCA — Metro de coluna d&#39;água</option>
              <option value="ozin2">oz/in&#178; — Onça-força por polegada quadrada</option>
              <option value="kgfcm2">kgf/cm&#178; — Quilograma-força por centímetro quadrado</option>
              <option value="inHg">inHg — Polegada de mercúrio</option>
              <option value="mmHg">mmHg — Milímetro de mercúrio (Torr)</option>
              <option value="cmHg">cmHg — Centímetro de mercúrio</option>
              <option value="mbar">mbar — Milibar</option>
              <option value="bar">bar — Bar</option>
              <option value="Pa">Pa — Pascal</option>
              <option value="kPa">kPa — Kilopascal</option>
              <option value="MPa">MPa — Megapascal</option>
            </select>
          </div>
          <div class="field" style="grid-column:1/-1;">
            <label for="conv-para">Unidade de destino</label>
            <select id="conv-para" class="sel-unidade" onchange="calcConversao()">
              <option value="PSI">PSI — Libra-força por polegada quadrada</option>
              <option value="atm">atm — Atmosfera padrão</option>
              <option value="inH2O">inH&#8322;O — Polegada de coluna d&#39;água</option>
              <option value="mmH2O">mm H&#8322;O — Milímetro de coluna d&#39;água</option>
              <option value="cmH2O">cmH&#8322;O — Centímetro de coluna d&#39;água</option>
              <option value="MCA">MCA — Metro de coluna d&#39;água</option>
              <option value="ozin2">oz/in&#178; — Onça-força por polegada quadrada</option>
              <option value="kgfcm2">kgf/cm&#178; — Quilograma-força por centímetro quadrado</option>
              <option value="inHg">inHg — Polegada de mercúrio</option>
              <option value="mmHg">mmHg — Milímetro de mercúrio (Torr)</option>
              <option value="cmHg">cmHg — Centímetro de mercúrio</option>
              <option value="mbar">mbar — Milibar</option>
              <option value="bar">bar — Bar</option>
              <option value="Pa">Pa — Pascal</option>
              <option value="kPa">kPa — Kilopascal</option>
              <option value="MPa">MPa — Megapascal</option>
            </select>
          </div>
          <div class="field" style="grid-column:1/-1;">
            <label for="conv-valor">Valor de entrada</label>
            <div class="input-with-unit">
              <input type="number" id="conv-valor" value="1" step="any" inputmode="decimal" style="padding-right:90px;" oninput="calcConversao()">
              <span class="unit-tag" id="conv-tag-de">PSI</span>
            </div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-header">
          <span class="badge" style="background:#0891b222; color:#22d3ee; border:1px solid #0891b255;">Resultado</span>
          <span class="card-title">Valor convertido</span>
        </div>
        <div class="corrente-block">
          <div class="corrente-label">Resultado</div>
          <div class="corrente-value">
            <span class="corrente-num" id="conv-out" style="color:#22d3ee; text-shadow:0 0 12px #0891b233;">—</span>
            <span class="corrente-unit" id="conv-tag-para">—</span>
          </div>
        </div>
        <div class="divider"></div>
        <div class="result-item" style="background:var(--bg-soft); border:1px solid var(--line-strong); border-radius:var(--radius); padding:1rem;">
          <div class="result-label">Fórmula aplicada</div>
          <div id="conv-formula" style="font-family:var(--font-mono); font-size:13px; color:var(--ink-dim); margin-top:6px; line-height:1.9; white-space:pre-wrap;">—</div>
        </div>
      </div>

      <div class="footer">Desenvolvido por <strong>Gabriel Vinicius Martins Oliveira</strong></div>
    </div>
  </div>

</div>

<script>
/* ===== NAVEGAÇÃO ENTRE TELAS ===== */
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(function(el) {
    el.classList.remove('active');
  });
  document.getElementById(id).classList.add('active');
  window.scrollTo(0, 0);
}

/* ===== UNIDADES DE MEDIDA ===== */
function syncUnidade(prefixo) {
  var u = document.getElementById(prefixo + '-unidade').value;
  var mapa = {
    'pv2':  ['pv2-tag-atual','pv2-tag-min','pv2-tag-max'],
    'cpv':  ['cpv-tag-min','cpv-tag-max','pv-out-unit','cpv-span-unit'],
    'v2p':  ['v2p-tag-q','v2p-tag-span','v2p-tag-qmax'],
    'pvz':  ['pvz-tag-pv','pvz-tag-span','pvz-tag-qmax']
  };
  if (mapa[prefixo]) {
    mapa[prefixo].forEach(function(id) {
      var el = document.getElementById(id);
      if (el) el.textContent = u;
    });
  }
  if (prefixo === 'cpv') calcCorrentePV();
}

/* ===== CALC 2: PV X CORRENTE ===== */
function calcProcesso() {
  var pvAtual = parseFloat(document.getElementById('pv-atual').value);
  var pvMin   = parseFloat(document.getElementById('pv2-min').value);
  var pvMax   = parseFloat(document.getElementById('pv2-max').value);
  var iMin    = parseFloat(document.getElementById('pv2-imin').value);
  var iMax    = parseFloat(document.getElementById('pv2-imax').value);
  var out = document.getElementById('out-proc');
  if ([pvAtual, pvMin, pvMax, iMin, iMax].some(function(v){ return isNaN(v); })) { out.textContent = '--'; return; }
  if (pvMax === pvMin) { out.textContent = '÷0'; return; }
  var r = (pvAtual - pvMin) * (iMax - iMin) / (pvMax - pvMin) + iMin;
  out.textContent = r.toFixed(2);
}
['pv-atual','pv2-min','pv2-max','pv2-imin','pv2-imax'].forEach(function(id) {
  document.getElementById(id).addEventListener('input', calcProcesso);
});
calcProcesso();

/* ===== CALC 3: RTD ===== */
function calcRTD() {
  var rtd = parseFloat(document.getElementById('rtd').value);
  var r0 = parseFloat(document.getElementById('r0').value);
  var fator = parseFloat(document.getElementById('fator').value);
  var out = document.getElementById('out-rtd');
  if ([rtd, r0, fator].some(function(v){ return isNaN(v); })) { out.textContent = '--'; return; }
  if (fator === 0) { out.textContent = '÷0'; return; }
  var r = (rtd - r0) / fator;
  out.textContent = r.toFixed(2);
}
['rtd','r0','fator'].forEach(function(id) {
  document.getElementById(id).addEventListener('input', calcRTD);
});
calcRTD();

/* ===== CALC 4: ROTAÇÃO DO MOTOR ===== */
function calcRotacao() {
  var ref = parseFloat(document.getElementById('ref').value);
  var velnom = parseFloat(document.getElementById('velnom').value);
  var velnomdec = parseFloat(document.getElementById('velnomdec').value);
  var outDec = document.getElementById('out-veldec');
  var outRpm = document.getElementById('out-velrpm');

  if ([ref, velnom, velnomdec].some(function(v){ return isNaN(v); })) {
    outDec.textContent = '--';
    document.getElementById('out-velhex').textContent = '--';
    outRpm.textContent = '--';
    return;
  }

  var veldec = Math.trunc(ref * velnomdec / 100);
  var velrpm = ref * velnom / 100;

  outDec.textContent = veldec;
  document.getElementById('out-velhex').textContent = veldec.toString(16).toUpperCase().padStart(4, '0') + 'h';
  outRpm.textContent = velrpm.toFixed(2);
}
['ref','velnom','velnomdec'].forEach(function(id) {
  document.getElementById(id).addEventListener('input', calcRotacao);
});
calcRotacao();

/* ===== CALC 5: CONVERSÃO VAZÃO → PRESSÃO ===== */
var vpKMode = { v2p: 'manual' };

function setKModeVP(modo, kmode) {
  vpKMode[modo] = kmode;
  var btnManual = document.getElementById('vp-' + modo + '-btn-manual');
  var btnAuto   = document.getElementById('vp-' + modo + '-btn-auto');
  var input     = document.getElementById('vp-' + modo + '-k');
  var info      = document.getElementById('vp-' + modo + '-k-info');
  btnManual.classList.toggle('active', kmode === 'manual');
  btnAuto.classList.toggle('active',   kmode === 'auto');
  if (kmode === 'auto') {
    input.disabled = true;
    info.style.display = 'block';
    updateAutoKVP(modo);
  } else {
    input.disabled = false;
    info.style.display = 'none';
  }
  calcV2P();
}

function updateAutoKVP(modo) {
  var span = parseFloat(document.getElementById('vp-v2p-span').value) || 0;
  var qmax = parseFloat(document.getElementById('vp-v2p-qmax').value) || 0;
  var k = span > 0 ? qmax / Math.sqrt(span) : 0;
  document.getElementById('vp-v2p-k').value = k.toFixed(4);
}

function calcV2P() {
  var q    = parseFloat(document.getElementById('vp-q').value);
  var span = parseFloat(document.getElementById('vp-v2p-span').value);
  var qmax = parseFloat(document.getElementById('vp-v2p-qmax').value);
  if (vpKMode.v2p === 'auto') updateAutoKVP('v2p');
  var K    = parseFloat(document.getElementById('vp-v2p-k').value) || 0;
  var f = function(n,d){ return isNaN(n)?'—':n.toFixed(d); };
  if ([q, span, qmax].some(isNaN) || qmax === 0 || K === 0) {
    ['vp-v2p-corrente','vp-v2p-pressao','vp-v2p-k-out','vp-v2p-s-ma','vp-v2p-s-pressao'].forEach(function(id){
      document.getElementById(id).textContent = '—';
    });
    return;
  }
  var corrente = q / qmax * 16 + 4;
  var pressao  = Math.pow(q / K, 2);
  document.getElementById('vp-v2p-corrente').textContent = f(corrente, 2);
  document.getElementById('vp-v2p-pressao').textContent  = f(pressao, 2);
  document.getElementById('vp-v2p-k-out').textContent    = f(K, 4);
  document.getElementById('vp-v2p-s-q').textContent       = q;
  document.getElementById('vp-v2p-s-span').textContent    = span;
  document.getElementById('vp-v2p-s-qmax').textContent    = qmax;
  document.getElementById('vp-v2p-s-k').textContent       = f(K, 4);
  document.getElementById('vp-v2p-s-pressao').textContent = f(pressao, 2);
  document.getElementById('vp-v2p-s-ma').textContent      = f(corrente, 2) + ' mA';
}
['vp-q','vp-v2p-span','vp-v2p-qmax','vp-v2p-k'].forEach(function(id){
  document.getElementById(id).addEventListener('input', calcV2P);
});
calcV2P();

/* ===== CALC 6: CONVERSÃO PRESSÃO → VAZÃO ===== */
var pvKMode = 'manual';

function setKModePV(mode) {
  pvKMode = mode;
  var input = document.getElementById('pv-fatorK');
  var info  = document.getElementById('pv-k-auto-info');
  document.getElementById('pv-btn-manual').classList.toggle('active', mode === 'manual');
  document.getElementById('pv-btn-auto').classList.toggle('active',   mode === 'auto');
  if (mode === 'auto') {
    input.disabled = true;
    info.style.display = 'block';
    updateAutoKPV();
  } else {
    input.disabled = false;
    info.style.display = 'none';
  }
  calcPressaoVazao();
}

function updateAutoKPV() {
  if (pvKMode !== 'auto') return;
  var qmax = parseFloat(document.getElementById('pv-qmax').value) || 0;
  var spam = parseFloat(document.getElementById('pv-spam').value) || 0;
  var k = spam > 0 ? qmax / Math.sqrt(spam) : 0;
  document.getElementById('pv-fatorK').value = k.toFixed(4);
}

function calcPressaoVazao() {
  var PV   = parseFloat(document.getElementById('pv-pv').value)   || 0;
  var SPAM = parseFloat(document.getElementById('pv-spam').value)  || 0;
  var QMAX = parseFloat(document.getElementById('pv-qmax').value)  || 0;
  if (pvKMode === 'auto') updateAutoKPV();
  var K = parseFloat(document.getElementById('pv-fatorK').value) || 0;
  var CONST    = 10;
  var percDP   = SPAM > 0 ? PV / SPAM : 0;
  var raiz     = CONST * Math.sqrt(percDP) * 10;
  var corrente = raiz / 100 * 16 + 4;
  var vazao    = K * Math.sqrt(PV);
  var f = function(n, d) { return isNaN(n) ? '—' : n.toFixed(d); };
  document.getElementById('pv-out-corrente').textContent = f(corrente, 2);
  document.getElementById('cpv2-out-perc').textContent     = f(percDP, 5);
  document.getElementById('pv-out-raiz').textContent     = f(raiz, 3);
  document.getElementById('pv-out-vazao').textContent    = f(vazao, 3);
  document.getElementById('cpv2-s-pv').textContent    = PV;
  document.getElementById('pv-s-spam').textContent   = SPAM;
  document.getElementById('pv-s-k').textContent      = f(K, 4);
  document.getElementById('pv-s-qmax').textContent   = QMAX;
  document.getElementById('pv-s-ma').textContent     = f(corrente, 2) + ' mA';
}
['pv-pv','pv-spam','pv-fatorK','pv-qmax'].forEach(function(id){
  document.getElementById(id).addEventListener('input', calcPressaoVazao);
});
calcPressaoVazao();

/* ===== CALC 7: CORRENTE X PV ===== */
function calcCorrentePV() {
  var I     = parseFloat(document.getElementById('pv-i-atual').value);
  var pvMin = parseFloat(document.getElementById('pv-min').value);
  var pvMax = parseFloat(document.getElementById('pv-max').value);
  var Imin  = 4;
  var Imax  = 20;
  var f = function(n, d) { return isNaN(n) ? '—' : n.toFixed(d); };

  if ([I, pvMin, pvMax].some(isNaN)) {
    ['pv-out','cpv2-out-perc','pv-out-span','pv-s-i','pv-s-min','pv-s-max','pv-s-perc','cpv2-s-pv'].forEach(function(id){
      document.getElementById(id).textContent = '—';
    });
    return;
  }

  var pv    = (I - Imin) * (pvMax - pvMin) / (Imax - Imin) + pvMin;
  var perc  = (I - Imin) / (Imax - Imin) * 100;
  var span  = pvMax - pvMin;

  document.getElementById('pv-out').textContent      = f(pv, 2);
  document.getElementById('cpv2-out-perc').textContent = f(perc, 2);
  document.getElementById('pv-out-span').textContent = f(span, 2);

  document.getElementById('pv-s-i').textContent    = f(I, 2) + ' mA';
  document.getElementById('pv-s-min').textContent   = pvMin;
  document.getElementById('pv-s-max').textContent   = pvMax;
  document.getElementById('pv-s-perc').textContent  = f(perc, 2) + ' %';
  document.getElementById('cpv2-s-pv').textContent    = f(pv, 2);
}
['pv-i-atual','pv-min','pv-max'].forEach(function(id){
  document.getElementById(id).addEventListener('input', calcCorrentePV);
});
calcCorrentePV();/* ===== CALC 8: CONVERSÃO DE PRESSÃO ===== */

// Fatores para converter cada unidade → Pascal (base)
// Extraídos da tabela de referência fornecida
var PRESSAO_PA = {
  'PSI'    : 6894.757,
  'atm'    : 101325,
  'inH2O'  : 249.0889,    // 1 PSI = 27.71 inH2O → 6894.757/27.71 = 248.85 ≈ tabela usa 249.089
  'mmH2O'  : 9.80665,     // 1 PSI = 703.8 mmH2O → 6894.757/703.8 = 9.797 ≈ tabela
  'cmH2O'  : 98.0665,     // 10x mmH2O
  'MCA'    : 9806.65,     // 1000x mmH2O
  'ozin2'  : 430.922,     // 1 PSI = 16 oz/in² → 6894.757/16 = 430.92
  'kgfcm2' : 98066.5,     // 1 kgf/cm² = 98066.5 Pa
  'inHg'   : 3386.389,    // 1 inHg = 3386.389 Pa
  'mmHg'   : 133.322,     // 1 mmHg = 133.322 Pa
  'cmHg'   : 1333.22,     // 10x mmHg
  'mbar'   : 100,         // 1 mbar = 100 Pa
  'bar'    : 100000,      // 1 bar = 100000 Pa
  'Pa'     : 1,
  'kPa'    : 1000,
  'MPa'    : 1000000
};

// Nomes de exibição curtos para a fórmula
var PRESSAO_NOME = {
  'PSI'    : 'PSI',
  'atm'    : 'atm',
  'inH2O'  : 'in H\u2082O',
  'mmH2O'  : 'mm H\u2082O',
  'cmH2O'  : 'cm H\u2082O',
  'MCA'    : 'MCA',
  'ozin2'  : 'oz/in\u00B2',
  'kgfcm2' : 'kgf/cm\u00B2',
  'inHg'   : 'inHg',
  'mmHg'   : 'mmHg',
  'cmHg'   : 'cmHg',
  'mbar'   : 'mbar',
  'bar'    : 'bar',
  'Pa'     : 'Pa',
  'kPa'    : 'kPa',
  'MPa'    : 'MPa'
};

function fmtNum(n) {
  if (n === 0) return '0';
  var abs = Math.abs(n);
  if (abs < 0.000001 || abs >= 1e10) return n.toExponential(6);
  if (abs < 0.001)  return parseFloat(n.toPrecision(6)).toString();
  if (abs < 1)      return parseFloat(n.toPrecision(7)).toString();
  if (abs < 1000)   return parseFloat(n.toPrecision(8)).toString();
  return parseFloat(n.toPrecision(8)).toString();
}

function calcConversao() {
  var de   = document.getElementById('conv-de').value;
  var para = document.getElementById('conv-para').value;
  var v    = parseFloat(document.getElementById('conv-valor').value);

  var nDe   = PRESSAO_NOME[de]   || de;
  var nPara = PRESSAO_NOME[para] || para;

  document.getElementById('conv-tag-de').textContent   = nDe;
  document.getElementById('conv-tag-para').textContent = nPara;

  var outEl = document.getElementById('conv-out');
  var frmEl = document.getElementById('conv-formula');

  if (isNaN(v)) {
    outEl.textContent = '\u2014';
    frmEl.textContent = '\u2014';
    return;
  }

  if (de === para) {
    outEl.textContent = fmtNum(v);
    frmEl.textContent = nDe + ' = ' + nPara + ' (mesma unidade)\nResultado = ' + fmtNum(v);
    return;
  }

  var fatorDePa   = PRESSAO_PA[de];
  var fatorParaPa = PRESSAO_PA[para];
  var fator       = fatorDePa / fatorParaPa;
  var resultado   = v * fator;

  outEl.textContent = fmtNum(resultado);

  frmEl.textContent =
    'Passo 1: Converter ' + nDe + ' para Pascal\n' +
    '  1 ' + nDe + ' = ' + fmtNum(fatorDePa) + ' Pa\n' +
    '  ' + fmtNum(v) + ' ' + nDe + ' x ' + fmtNum(fatorDePa) + ' = ' + fmtNum(v * fatorDePa) + ' Pa\n\n' +
    'Passo 2: Converter Pascal para ' + nPara + '\n' +
    '  1 Pa = ' + fmtNum(1 / fatorParaPa) + ' ' + nPara + '\n' +
    '  ' + fmtNum(v * fatorDePa) + ' Pa \u00F7 ' + fmtNum(fatorParaPa) + ' = ' + fmtNum(resultado) + ' ' + nPara + '\n\n' +
    'Fator direto: 1 ' + nDe + ' = ' + fmtNum(fator) + ' ' + nPara;
}

document.getElementById('conv-de').addEventListener('change', calcConversao);
document.getElementById('conv-para').addEventListener('change', calcConversao);
document.getElementById('conv-valor').addEventListener('input', calcConversao);
document.getElementById('conv-para').value = 'bar';
calcConversao();
</script>
</body>
</html>
