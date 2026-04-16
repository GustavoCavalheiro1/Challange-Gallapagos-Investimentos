# Challange-Gallapagos-Investimentos
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Eneva S.A. — Pricing Debênture Lei 12.431</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Mono:wght@400;500&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0e14;
    --surface: #111620;
    --surface2: #161d29;
    --border: #1e2a3a;
    --accent: #00c6a2;
    --accent2: #f0b429;
    --accent3: #e05c5c;
    --text: #e8edf5;
    --muted: #6b7a90;
    --green: #00c6a2;
    --red: #e05c5c;
    --yellow: #f0b429;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Outfit', sans-serif;
    font-weight: 400;
    min-height: 100vh;
    padding: 40px 32px;
  }

  /* HEADER */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 40px;
    padding-bottom: 24px;
    border-bottom: 1px solid var(--border);
  }

  .logo-area h1 {
    font-family: 'DM Serif Display', serif;
    font-size: 2rem;
    color: var(--text);
    letter-spacing: -0.02em;
  }

  .logo-area h1 span {
    color: var(--accent);
  }

  .logo-area p {
    font-size: 0.78rem;
    color: var(--muted);
    margin-top: 4px;
    font-family: 'DM Mono', monospace;
    letter-spacing: 0.08em;
    text-transform: uppercase;
  }

  .badge {
    background: rgba(0, 198, 162, 0.1);
    border: 1px solid rgba(0, 198, 162, 0.3);
    color: var(--accent);
    font-size: 0.72rem;
    font-family: 'DM Mono', monospace;
    padding: 6px 14px;
    border-radius: 4px;
    letter-spacing: 0.1em;
    text-transform: uppercase;
  }

  /* SECTION TITLE */
  .section-label {
    font-family: 'DM Mono', monospace;
    font-size: 0.65rem;
    color: var(--accent);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    margin-bottom: 16px;
  }

  /* TERM SHEET */
  .termsheet {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    margin-bottom: 28px;
    overflow: hidden;
  }

  .termsheet-header {
    background: var(--surface2);
    padding: 16px 24px;
    border-bottom: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .termsheet-header h2 {
    font-size: 0.95rem;
    font-weight: 600;
    color: var(--text);
    letter-spacing: 0.01em;
  }

  .termsheet-header span {
    font-family: 'DM Mono', monospace;
    font-size: 0.7rem;
    color: var(--muted);
  }

  .term-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0;
  }

  .term-item {
    padding: 18px 24px;
    border-right: 1px solid var(--border);
    border-bottom: 1px solid var(--border);
  }

  .term-item:nth-child(3n) { border-right: none; }

  .term-label {
    font-size: 0.65rem;
    color: var(--muted);
    letter-spacing: 0.12em;
    text-transform: uppercase;
    margin-bottom: 6px;
    font-family: 'DM Mono', monospace;
  }

  .term-value {
    font-size: 1.05rem;
    font-weight: 600;
    color: var(--text);
  }

  .term-value.highlight { color: var(--accent); }
  .term-value.warn { color: var(--yellow); }

  .term-sub {
    font-size: 0.7rem;
    color: var(--muted);
    margin-top: 3px;
  }

  /* PRICING TABLE */
  .pricing-block {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin-bottom: 28px;
  }

  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
  }

  .card-header {
    background: var(--surface2);
    padding: 14px 20px;
    border-bottom: 1px solid var(--border);
    font-size: 0.8rem;
    font-weight: 600;
    color: var(--text);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .card-header .tag {
    font-family: 'DM Mono', monospace;
    font-size: 0.65rem;
    color: var(--accent);
    background: rgba(0,198,162,0.08);
    padding: 3px 8px;
    border-radius: 3px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
  }

  th {
    font-family: 'DM Mono', monospace;
    font-size: 0.62rem;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    padding: 10px 16px;
    text-align: left;
    border-bottom: 1px solid var(--border);
    background: rgba(255,255,255,0.015);
  }

  td {
    font-size: 0.82rem;
    padding: 10px 16px;
    border-bottom: 1px solid rgba(30,42,58,0.6);
    color: var(--text);
  }

  tr:last-child td { border-bottom: none; }

  td.mono { font-family: 'DM Mono', monospace; font-size: 0.78rem; }
  td.accent { color: var(--accent); font-weight: 600; font-family: 'DM Mono', monospace; }
  td.green { color: var(--green); font-family: 'DM Mono', monospace; }
  td.red { color: var(--red); font-family: 'DM Mono', monospace; }
  td.yellow { color: var(--yellow); font-family: 'DM Mono', monospace; }

  tr.highlight-row td { background: rgba(0,198,162,0.05); }
  tr.highlight-row td.accent { background: rgba(0,198,162,0.05); }

  /* SENSITIVITY */
  .sensitivity-section {
    margin-bottom: 28px;
  }

  .sens-table-wrap {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
  }

  .sens-table th {
    background: var(--surface2);
    font-family: 'DM Mono', monospace;
    font-size: 0.62rem;
    color: var(--muted);
    padding: 10px 14px;
    text-align: center;
    border-bottom: 1px solid var(--border);
    border-right: 1px solid var(--border);
  }

  .sens-table td {
    text-align: center;
    font-family: 'DM Mono', monospace;
    font-size: 0.78rem;
    padding: 9px 14px;
    border-right: 1px solid rgba(30,42,58,0.6);
    border-bottom: 1px solid rgba(30,42,58,0.6);
  }

  .sens-table tr:last-child td { border-bottom: none; }

  .cell-base { background: rgba(0,198,162,0.12); color: var(--accent); font-weight: 600; }
  .cell-good { background: rgba(0,198,162,0.05); color: #7ee3ce; }
  .cell-ok { background: rgba(240,180,41,0.05); color: var(--yellow); }
  .cell-bad { background: rgba(224,92,92,0.06); color: var(--red); }

  .row-label {
    font-family: 'DM Mono', monospace;
    font-size: 0.7rem;
    color: var(--muted);
    text-align: left !important;
    background: var(--surface2);
    border-right: 1px solid var(--border) !important;
    white-space: nowrap;
  }

  /* COVENANT */
  .covenant-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 12px;
    margin-bottom: 28px;
  }

  .cov-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 16px 18px;
  }

  .cov-label {
    font-family: 'DM Mono', monospace;
    font-size: 0.62rem;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 8px;
  }

  .cov-value {
    font-size: 1.4rem;
    font-weight: 600;
    font-family: 'DM Mono', monospace;
  }

  .cov-limit {
    font-size: 0.7rem;
    color: var(--muted);
    margin-top: 4px;
  }

  .cov-status {
    display: inline-block;
    margin-top: 8px;
    font-size: 0.65rem;
    font-family: 'DM Mono', monospace;
    padding: 2px 8px;
    border-radius: 3px;
  }

  .ok-badge { background: rgba(0,198,162,0.12); color: var(--accent); }
  .warn-badge { background: rgba(240,180,41,0.12); color: var(--yellow); }

  /* COMPARABLES */
  .comparables {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
    margin-bottom: 28px;
  }

  /* WATERFALL */
  .waterfall {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 0;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow: hidden;
    margin-bottom: 28px;
  }

  .wf-item {
    padding: 18px 16px;
    border-right: 1px solid var(--border);
    text-align: center;
  }
  .wf-item:last-child { border-right: none; }

  .wf-label {
    font-size: 0.62rem;
    font-family: 'DM Mono', monospace;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 10px;
  }

  .wf-arrow {
    font-size: 1.2rem;
    color: var(--muted);
    margin: 0 4px;
  }

  .wf-val {
    font-size: 1.1rem;
    font-weight: 600;
    font-family: 'DM Mono', monospace;
  }

  .wf-sub {
    font-size: 0.68rem;
    color: var(--muted);
    margin-top: 4px;
  }

  .wf-plus { color: var(--green); }
  .wf-equals { color: var(--accent); border-left: 2px solid var(--accent); }

  /* FOOTER */
  .footer {
    border-top: 1px solid var(--border);
    margin-top: 32px;
    padding-top: 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .footer p {
    font-size: 0.65rem;
    color: var(--muted);
    font-family: 'DM Mono', monospace;
  }

  .disclaimer {
    font-size: 0.65rem;
    color: var(--muted);
    font-family: 'DM Mono', monospace;
    background: rgba(255,255,255,0.02);
    border: 1px solid var(--border);
    border-radius: 4px;
    padding: 10px 14px;
    margin-bottom: 28px;
    line-height: 1.6;
  }

  .wide { grid-column: span 2; }

  /* BAR CHART */
  .bar-wrap {
    padding: 16px 20px;
  }
  .bar-row {
    display: flex;
    align-items: center;
    gap: 12px;
    margin-bottom: 10px;
  }
  .bar-name {
    font-size: 0.72rem;
    color: var(--muted);
    font-family: 'DM Mono', monospace;
    width: 80px;
    flex-shrink: 0;
  }
  .bar-track {
    flex: 1;
    height: 22px;
    background: rgba(255,255,255,0.04);
    border-radius: 3px;
    overflow: hidden;
    position: relative;
  }
  .bar-fill {
    height: 100%;
    border-radius: 3px;
    display: flex;
    align-items: center;
    padding-left: 8px;
  }
  .bar-fill span {
    font-family: 'DM Mono', monospace;
    font-size: 0.68rem;
    color: #000;
    font-weight: 600;
  }
  .bar-eneva { background: var(--accent); }
  .bar-celse { background: #4a9eff; }
  .bar-rumo { background: var(--yellow); }
  .bar-taee { background: #a78bfa; }
  .bar-isa { background: #fb923c; }

</style>
</head>
<body>

<!-- HEADER -->
<div class="header">
  <div class="logo-area">
    <h1>ENEVA S.A. <span>—</span> Pricing de Emissão</h1>
    <p>Debênture Incentivada · Lei 12.431/2011 · Série Única · Abril 2026</p>
  </div>
  <div class="badge">Confidencial · Grupo AGK</div>
</div>

<!-- TERM SHEET -->
<div class="section-label">01 — Term Sheet Proposto</div>
<div class="termsheet">
  <div class="termsheet-header">
    <h2>Estrutura da Emissão</h2>
    <span>ENEV3 · Rating AA- (S&P) · Setor Prioritário: Geração de Energia</span>
  </div>
  <div class="term-grid">
    <div class="term-item">
      <div class="term-label">Emissor</div>
      <div class="term-value">Eneva S.A.</div>
      <div class="term-sub">ou CELSE (SPE Sergipe)</div>
    </div>
    <div class="term-item">
      <div class="term-label">Volume Total</div>
      <div class="term-value highlight">R$ 3,0 bi</div>
      <div class="term-sub">60% refin. + 40% solar/GD</div>
    </div>
    <div class="term-item">
      <div class="term-label">Prazo</div>
      <div class="term-value">15 anos</div>
      <div class="term-sub">Vencimento: abril/2041</div>
    </div>
    <div class="term-item">
      <div class="term-label">Indexador</div>
      <div class="term-value highlight">IPCA +</div>
      <div class="term-sub">Vedado CDI (Lei 12.431)</div>
    </div>
    <div class="term-item">
      <div class="term-label">Spread Alvo</div>
      <div class="term-value highlight">6,75% a.a.</div>
      <div class="term-sub">Range: 6,50% – 7,25%</div>
    </div>
    <div class="term-item">
      <div class="term-label">Carência Amort.</div>
      <div class="term-value warn">5 anos</div>
      <div class="term-sub">Juros pagos semestralmente</div>
    </div>
    <div class="term-item">
      <div class="term-label">Amortização</div>
      <div class="term-value">Bullet</div>
      <div class="term-sub">ou price no ano 6–15</div>
    </div>
    <div class="term-item">
      <div class="term-label">Benefício PF</div>
      <div class="term-value highlight">IR 0%</div>
      <div class="term-sub">Isenção ao investidor PF</div>
    </div>
    <div class="term-item">
      <div class="term-label">Enquadramento</div>
      <div class="term-value">Geração Elétrica</div>
      <div class="term-sub">Setor prioritário · Art. 2º</div>
    </div>
  </div>
</div>

<!-- PRICING WATERFALL -->
<div class="section-label">02 — Composição do Custo de Captação</div>
<div class="waterfall">
  <div class="wf-item">
    <div class="wf-label">NTN-B 2041</div>
    <div class="wf-val wf-plus">~7,20%</div>
    <div class="wf-sub">Tesouro IPCA+ base</div>
  </div>
  <div class="wf-item">
    <div class="wf-label">+ Credit Spread</div>
    <div class="wf-val wf-plus">+0,80%</div>
    <div class="wf-sub">Risco Eneva vs. soberano</div>
  </div>
  <div class="wf-item">
    <div class="wf-label">+ Illiquidity Prem.</div>
    <div class="wf-val wf-plus">+0,30%</div>
    <div class="wf-sub">Prazo 15 anos</div>
  </div>
  <div class="wf-item">
    <div class="wf-label">- Benefício IR</div>
    <div class="wf-val" style="color:var(--red)">−1,55%</div>
    <div class="wf-sub">Equivalência gross-up PF</div>
  </div>
  <div class="wf-item wf-equals">
    <div class="wf-label">= Custo Líquido</div>
    <div class="wf-val highlight">IPCA+6,75%</div>
    <div class="wf-sub">Estimativa bookbuilding</div>
  </div>
</div>

<!-- PRICING TABLE + COMPARABLES -->
<div class="pricing-block">

  <!-- COMPARABLES -->
  <div class="card">
    <div class="card-header">
      Transações Comparáveis — Setor Elétrico
      <span class="tag">2022–2025</span>
    </div>
    <table>
      <thead>
        <tr>
          <th>Emissor</th>
          <th>Ticker</th>
          <th>Prazo</th>
          <th>Taxa</th>
          <th>Ano</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Eneva (ENEV19)</td>
          <td class="mono">ENEV19</td>
          <td class="mono">6 anos</td>
          <td class="accent">IPCA+6,50%</td>
          <td class="mono">2023</td>
        </tr>
        <tr>
          <td>Eneva (Santander)</td>
          <td class="mono">ENEV19</td>
          <td class="mono">6 anos</td>
          <td class="accent">IPCA+7,08%</td>
          <td class="mono">2023</td>
        </tr>
        <tr>
          <td>CELSE (SPE Sergipe)</td>
          <td class="mono">CELS</td>
          <td class="mono">10 anos</td>
          <td class="accent">~IPCA+6,80%</td>
          <td class="mono">2023</td>
        </tr>
        <tr>
          <td>Rumo S.A.</td>
          <td class="mono">RUMO</td>
          <td class="mono">7 anos</td>
          <td class="green">IPCA+6,35%</td>
          <td class="mono">2023</td>
        </tr>
        <tr>
          <td>TAESA (10 anos)</td>
          <td class="mono">TAEE</td>
          <td class="mono">10 anos</td>
          <td class="green">IPCA+5,87%</td>
          <td class="mono">2023</td>
        </tr>
        <tr>
          <td>TAESA (12 anos)</td>
          <td class="mono">TAEE</td>
          <td class="mono">12 anos</td>
          <td class="green">IPCA+6,06%</td>
          <td class="mono">2023</td>
        </tr>
        <tr>
          <td>ISA CTEEP</td>
          <td class="mono">CTEE</td>
          <td class="mono">8 anos</td>
          <td class="green">IPCA+6,25%</td>
          <td class="mono">2023</td>
        </tr>
        <tr class="highlight-row">
          <td><strong>Eneva — Proposta</strong></td>
          <td class="mono">—</td>
          <td class="mono"><strong>15 anos</strong></td>
          <td class="accent"><strong>IPCA+6,75%</strong></td>
          <td class="mono">2026</td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- SPREAD POR PRAZO BENCHMARK -->
  <div class="card">
    <div class="card-header">
      Spread por Prazo — Curva de Crédito Eneva
      <span class="tag">Referência Mercado</span>
    </div>
    <div class="bar-wrap">
      <div class="bar-row">
        <div class="bar-name">ENEV 6a</div>
        <div class="bar-track">
          <div class="bar-fill bar-eneva" style="width:78%"><span>IPCA+6,50%</span></div>
        </div>
      </div>
      <div class="bar-row">
        <div class="bar-name">CELSE 10a</div>
        <div class="bar-track">
          <div class="bar-fill bar-celse" style="width:82%"><span>IPCA+6,80%</span></div>
        </div>
      </div>
      <div class="bar-row">
        <div class="bar-name">ENEV 15a ▶</div>
        <div class="bar-track">
          <div class="bar-fill bar-eneva" style="width:81%;opacity:0.8"><span>IPCA+6,75%</span></div>
        </div>
      </div>
      <div class="bar-row">
        <div class="bar-name">RUMO 7a</div>
        <div class="bar-track">
          <div class="bar-fill bar-rumo" style="width:76%"><span>IPCA+6,35%</span></div>
        </div>
      </div>
      <div class="bar-row">
        <div class="bar-name">TAEE 10a</div>
        <div class="bar-track">
          <div class="bar-fill bar-taee" style="width:70%"><span>IPCA+5,87%</span></div>
        </div>
      </div>
      <div class="bar-row">
        <div class="bar-name">ISA CTEEP</div>
        <div class="bar-track">
          <div class="bar-fill bar-isa" style="width:75%"><span>IPCA+6,25%</span></div>
        </div>
      </div>
    </div>
    <div style="padding:0 20px 14px; font-size:0.68rem; color:var(--muted); font-family:'DM Mono',monospace; line-height:1.6;">
      Racional: Eneva tem risco maior que TAESA/ISA (transmissoras) e comparável à CELSE.<br>
      Premium de prazo (+0,25–0,30 bps vs. 6a) justifica spread 6,75% no bookbuilding.
    </div>
  </div>
</div>

<!-- SENSIBILIDADE -->
<div class="sensitivity-section">
  <div class="section-label">03 — Análise de Sensibilidade: IRR do Investidor × Custo Eneva</div>
  <div class="sens-table-wrap">
    <div style="padding: 14px 20px; background: var(--surface2); border-bottom: 1px solid var(--border); font-size: 0.8rem; font-weight: 600;">
      IRR Real Líquido (Investidor PF — Isento IR) × Spread de Emissão
    </div>
    <table class="sens-table">
      <thead>
        <tr>
          <th class="row-label" style="text-align:left">IPCA Realiz. →<br>Spread Emissão ↓</th>
          <th>IPCA 3,5%</th>
          <th>IPCA 4,5%</th>
          <th>IPCA 5,5% (base)</th>
          <th>IPCA 6,5%</th>
          <th>IPCA 7,5%</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="row-label">IPCA + 5,50%</td>
          <td class="cell-good">9,00%</td>
          <td class="cell-good">10,00%</td>
          <td class="cell-good">11,00%</td>
          <td class="cell-good">12,00%</td>
          <td class="cell-good">13,00%</td>
        </tr>
        <tr>
          <td class="row-label">IPCA + 6,00%</td>
          <td class="cell-good">9,50%</td>
          <td class="cell-good">10,50%</td>
          <td class="cell-good">11,50%</td>
          <td class="cell-good">12,50%</td>
          <td class="cell-good">13,50%</td>
        </tr>
        <tr>
          <td class="row-label">IPCA + 6,50%</td>
          <td class="cell-good">10,00%</td>
          <td class="cell-good">11,00%</td>
          <td class="cell-good">11,80%</td>
          <td class="cell-good">12,80%</td>
          <td class="cell-good">14,00%</td>
        </tr>
        <tr>
          <td class="row-label" style="color:var(--accent)">IPCA + 6,75% ◀ BASE</td>
          <td class="cell-good">10,25%</td>
          <td class="cell-good">11,25%</td>
          <td class="cell-base">12,25%</td>
          <td class="cell-good">13,25%</td>
          <td class="cell-good">14,25%</td>
        </tr>
        <tr>
          <td class="row-label">IPCA + 7,00%</td>
          <td class="cell-ok">10,50%</td>
          <td class="cell-ok">11,50%</td>
          <td class="cell-ok">12,50%</td>
          <td class="cell-ok">13,50%</td>
          <td class="cell-ok">14,50%</td>
        </tr>
        <tr>
          <td class="row-label">IPCA + 7,25%</td>
          <td class="cell-bad">10,75%</td>
          <td class="cell-bad">11,75%</td>
          <td class="cell-bad">12,75%</td>
          <td class="cell-bad">13,75%</td>
          <td class="cell-bad">14,75%</td>
        </tr>
      </tbody>
    </table>
    <div style="padding: 12px 20px; font-size:0.68rem; color:var(--muted); font-family:'DM Mono',monospace; background:var(--surface2); border-top:1px solid var(--border); line-height:1.7;">
      Verde: zona de demanda robusta PF (IRR real &gt; 10% isento). | Amarelo: zona limite, demanda institucional. | Vermelho: zona de custo elevado para Eneva, evitar. 
      Base: IPCA 5,5% (meta Copom 2026E) + spread 6,75% = 12,25% nominal isento ≅ CDI+2,0% equiv. tributado.
    </div>
  </div>
</div>

<!-- COVENANTS -->
<div class="section-label">04 — Covenants Financeiros Propostos e Headroom Atual</div>
<div class="covenant-grid">
  <div class="cov-card">
    <div class="cov-label">Dív. Líq. / EBITDA</div>
    <div class="cov-val" style="color:var(--yellow)">2,6x</div>
    <div class="cov-limit">Covenant máx: 3,5x</div>
    <div class="cov-status ok-badge">Headroom: 0,9x ✓</div>
  </div>
  <div class="cov-card">
    <div class="cov-label">DSCR (Serv. Dívida)</div>
    <div class="cov-val" style="color:var(--green)">1,8x</div>
    <div class="cov-limit">Covenant mín: 1,2x</div>
    <div class="cov-status ok-badge">Confortável ✓</div>
  </div>
  <div class="cov-card">
    <div class="cov-label">Caixa Mínimo</div>
    <div class="cov-val" style="color:var(--green)">R$4,8bi</div>
    <div class="cov-limit">Threshold: R$ 800 mm</div>
    <div class="cov-status ok-badge">Ampla folga ✓</div>
  </div>
  <div class="cov-card">
    <div class="cov-label">Margem EBITDA</div>
    <div class="cov-val" style="color:var(--yellow)">34,5%</div>
    <div class="cov-limit">Covenant mín: 25%</div>
    <div class="cov-status warn-badge">Monitorar ⚠</div>
  </div>
</div>

<!-- USO DE RECURSOS -->
<div class="section-label">05 — Uso dos Recursos e Cronograma</div>
<div class="pricing-block">
  <div class="card">
    <div class="card-header">Alocação dos R$ 3,0 bi</div>
    <table>
      <thead>
        <tr><th>Destinação</th><th>Volume (R$ mm)</th><th>% Total</th><th>Prazo Exec.</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>Refinanciamento dívidas 2025–2027</td>
          <td class="accent">1.800</td>
          <td class="mono">60%</td>
          <td class="mono">Imediato</td>
        </tr>
        <tr>
          <td>Geração Distribuída e Solar</td>
          <td class="green">900</td>
          <td class="mono">30%</td>
          <td class="mono">12–24 meses</td>
        </tr>
        <tr>
          <td>Capex Upstream (Parnaíba/Azulão)</td>
          <td class="yellow">300</td>
          <td class="mono">10%</td>
          <td class="mono">24–36 meses</td>
        </tr>
        <tr class="highlight-row">
          <td><strong>Total</strong></td>
          <td class="accent"><strong>3.000</strong></td>
          <td class="mono"><strong>100%</strong></td>
          <td class="mono">—</td>
        </tr>
      </tbody>
    </table>
  </div>

  <div class="card">
    <div class="card-header">Impacto Pós-Emissão — Estrutura de Capital</div>
    <table>
      <thead>
        <tr><th>Métrica</th><th>Antes (1T25)</th><th>Pós-Emissão</th><th>Δ</th></tr>
      </thead>
      <tbody>
        <tr>
          <td>Dívida Bruta (R$ bi)</td>
          <td class="mono">~19,2</td>
          <td class="mono">~21,4</td>
          <td class="red">+2,2</td>
        </tr>
        <tr>
          <td>Duration Média (anos)</td>
          <td class="mono">~6</td>
          <td class="mono">~9</td>
          <td class="green">+3</td>
        </tr>
        <tr>
          <td>Dív. Líq. / EBITDA (2025E)</td>
          <td class="mono">2,6x</td>
          <td class="mono">~2,8x</td>
          <td class="yellow">+0,2x</td>
        </tr>
        <tr>
          <td>Custo Médio Dívida</td>
          <td class="mono">IPCA+7,5% est.</td>
          <td class="mono">IPCA+7,1%</td>
          <td class="green">−0,4%</td>
        </tr>
        <tr>
          <td>Amort. Concentrada 25–27</td>
          <td class="red">Alto</td>
          <td class="green">Reduzida</td>
          <td class="green">✓ Aliviado</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<!-- TESE DE CRÉDITO -->
<div class="section-label">06 — Tese de Crédito Consolidada</div>
<div class="comparables">
  <div class="card-header" style="background:var(--surface2); padding:14px 20px;">
    Fatores de Suporte e Riscos — Visão do Investidor
  </div>
  <div style="display:grid; grid-template-columns:1fr 1fr; gap:0;">
    <div style="padding:16px 20px; border-right:1px solid var(--border);">
      <div style="font-size:0.68rem; font-family:'DM Mono',monospace; color:var(--accent); letter-spacing:0.1em; text-transform:uppercase; margin-bottom:12px;">✓ Suportes</div>
      <table>
        <tbody>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0; color:var(--text)">Receita majoritariamente fixa (PPAs de longo prazo)</td></tr>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0;">Caixa R$ 4,8 bi — liquidez robusta</td></tr>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0;">EBITDA recorde 1T25: R$ 1,53 bi (+40% a/a)</td></tr>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0;">Modelo R2W reduz risco de suprimento</td></tr>
          <tr><td style="font-size:0.8rem; padding:8px 0;">Ativo prioritário no despacho térmico (ONS)</td></tr>
        </tbody>
      </table>
    </div>
    <div style="padding:16px 20px;">
      <div style="font-size:0.68rem; font-family:'DM Mono',monospace; color:var(--red); letter-spacing:0.1em; text-transform:uppercase; margin-bottom:12px;">⚠ Riscos</div>
      <table>
        <tbody>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0; color:var(--text)">Pressão de margem: EBITDA% cai 58%→25% (2021–2030)</td></tr>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0;">CAPEX elevado e recorrente (~R$1,75 bi/ano)</td></tr>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0;">Duration mismatch: ativos 25a vs. dívida ~6a</td></tr>
          <tr><td style="font-size:0.8rem; border-bottom:1px solid rgba(30,42,58,0.4); padding:8px 0;">Sensibilidade a spreads de crédito (Selic elevada)</td></tr>
          <tr><td style="font-size:0.8rem; padding:8px 0;">Carvão (~725 MW) com risco regulatório ESG</td></tr>
        </tbody>
      </table>
    </div>
  </div>
</div>

<div class="disclaimer">
  DISCLAIMER: Este material foi elaborado pelo Grupo AGK para fins acadêmicos do Galapagos Capital Challenge Program. Não constitui oferta, recomendação de investimento ou prospecto de emissão.
  Spreads e taxas são estimativas baseadas em transações comparáveis públicas e dados de mercado disponíveis. Dados reais: Eneva 1T25 (ADVFN/Infomoney, mai/2025). Comparáveis: XP/Santander/Infomoney (2023).
</div>

<div class="footer">
  <p>Grupo AGK · Adrian Raposo · Gustavo Cavalheiro · Kauê Souza</p>
  <p>Galapagos Capital — Challenge Program · Abril 2026</p>
</div>

</body>
</html>
