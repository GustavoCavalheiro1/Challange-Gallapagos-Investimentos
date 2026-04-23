"""
ENEVA S.A. | AGK Research Dashboard
Galapagos Capital — Challenge Program
Grupo AGK: Adrian Raposo · Gustavo Cavalheiro · Kauê Balbino
"""

import streamlit as st
import pandas as pd
import numpy as np
import plotly.graph_objects as go
import plotly.express as px
from plotly.subplots import make_subplots

# ─── CONFIG ────────────────────────────────────────────────────────────────────
st.set_page_config(
    page_title="ENEVA | AGK Research",
    page_icon="⚡",
    layout="wide",
    initial_sidebar_state="collapsed",
)

# ─── THEME ─────────────────────────────────────────────────────────────────────
st.markdown("""
<style>
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600&family=IBM+Plex+Sans:wght@300;400;500;600&display=swap');

/* Reset & base */
html, body, [class*="css"] {
    font-family: 'IBM Plex Sans', sans-serif !important;
    background-color: #07090c !important;
    color: #c8d4e0 !important;
}
.main { background-color: #07090c !important; }
.block-container { padding: 1.2rem 2rem 2rem !important; max-width: 100% !important; }
header[data-testid="stHeader"] { background: transparent !important; }

/* Sidebar */
section[data-testid="stSidebar"] {
    background: #0c0f14 !important;
    border-right: 1px solid #1a2030 !important;
}

/* ── KPI cards ── */
.kpi-card {
    background: #0c0f14;
    border: 1px solid #1a2030;
    border-radius: 4px;
    padding: 14px 16px 12px;
    position: relative;
    overflow: hidden;
    height: 90px;
}
.kpi-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0;
    width: 2px; height: 100%;
}
.kpi-card.blue::before   { background: #3b82f6; }
.kpi-card.green::before  { background: #10b981; }
.kpi-card.red::before    { background: #ef4444; }
.kpi-card.amber::before  { background: #f59e0b; }
.kpi-card.teal::before   { background: #06b6d4; }

.kpi-label {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px; font-weight: 500;
    text-transform: uppercase; letter-spacing: .12em;
    color: #3a4a5c; margin-bottom: 5px;
}
.kpi-value {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 20px; font-weight: 600;
    color: #e2eaf4; letter-spacing: -.02em;
    line-height: 1.1;
}
.kpi-sub {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px; margin-top: 4px;
}
.kpi-sub.pos { color: #10b981; }
.kpi-sub.neg { color: #ef4444; }
.kpi-sub.neu { color: #3a4a5c; }

/* ── Section headers ── */
.sec-hdr {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px; font-weight: 500;
    text-transform: uppercase; letter-spacing: .15em;
    color: #3b82f6; padding-bottom: 7px;
    border-bottom: 1px solid #1a2030;
    margin-bottom: 14px; margin-top: 6px;
}

/* ── Top bar ── */
.topbar {
    display: flex; align-items: center;
    justify-content: space-between;
    padding: 8px 0 16px;
    border-bottom: 1px solid #1a2030;
    margin-bottom: 18px;
}
.topbar-logo {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 15px; font-weight: 600;
    color: #e2eaf4; letter-spacing: .03em;
}
.topbar-logo .accent { color: #3b82f6; }
.topbar-meta {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 10px; color: #2a3a4c; text-align: right; line-height: 1.7;
}
.dot { display:inline-block; width:6px; height:6px;
       background:#10b981; border-radius:50%; margin-right:4px;
       animation: blink 2s infinite; }
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:.3} }

/* ── Data table ── */
.stDataFrame { border: 1px solid #1a2030 !important; border-radius: 4px !important; }
.stDataFrame thead th {
    background: #0c0f14 !important;
    color: #3a4a5c !important;
    font-family: 'IBM Plex Mono', monospace !important;
    font-size: 9px !important;
    text-transform: uppercase !important;
    letter-spacing: .08em !important;
}

/* ── Chip badges ── */
.chip {
    display: inline-block;
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px; font-weight: 500;
    text-transform: uppercase; letter-spacing: .08em;
    padding: 2px 8px; border-radius: 2px;
}
.chip-g { background:#042f22; color:#10b981; border:1px solid #064e3b; }
.chip-r { background:#300a0a; color:#ef4444; border:1px solid #7f1d1d; }
.chip-b { background:#0f1f3d; color:#3b82f6; border:1px solid #1e3a5f; }
.chip-a { background:#2c1600; color:#f59e0b; border:1px solid #78350f; }

/* ── Info boxes ── */
.info-box {
    border-radius: 3px; padding: 10px 12px;
    font-size: 11px; line-height: 1.7; margin-bottom: 8px;
}
.info-box.blue  { background:#0f1f3d; border:1px solid #1e3a5f; color:#93c5fd; }
.info-box.green { background:#042f22; border:1px solid #064e3b; color:#6ee7b7; }
.info-box.red   { background:#300a0a; border:1px solid #7f1d1d; color:#fca5a5; }
.info-box.amber { background:#2c1600; border:1px solid #78350f; color:#fcd34d; }

.info-box .lbl {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 9px; text-transform: uppercase;
    letter-spacing: .1em; margin-bottom: 5px;
    display: block;
}

/* ── Nav tabs ── */
div[data-testid="stHorizontalBlock"] { gap: 0 !important; }
.stTabs [data-baseweb="tab-list"] {
    background: #0c0f14 !important;
    border-bottom: 1px solid #1a2030 !important;
    gap: 0 !important;
}
.stTabs [data-baseweb="tab"] {
    font-family: 'IBM Plex Mono', monospace !important;
    font-size: 10px !important; font-weight: 500 !important;
    text-transform: uppercase !important; letter-spacing: .08em !important;
    color: #3a4a5c !important;
    padding: 8px 20px !important;
    border-radius: 0 !important;
    background: transparent !important;
}
.stTabs [aria-selected="true"] {
    color: #3b82f6 !important;
    border-bottom: 2px solid #3b82f6 !important;
    background: #0f1f3d !important;
}
.stTabs [data-baseweb="tab-panel"] {
    background: transparent !important;
    padding-top: 20px !important;
}

/* ── WACC row ── */
.wacc-row {
    display: flex; justify-content: space-between;
    align-items: center; padding: 7px 0;
    border-bottom: 1px solid #0e1219;
    font-family: 'IBM Plex Mono', monospace;
}
.wacc-key   { font-size: 10px; color: #8896a8; }
.wacc-desc  { font-size: 9px; color: #2a3a4c; flex: 1; text-align: center; }
.wacc-val   { font-size: 12px; font-weight: 600; color: #e2eaf4; }

/* ── Duration bar ── */
.dur-bar { height: 22px; border-radius: 2px; display:flex; align-items:center; padding: 0 10px; margin-bottom: 8px; }
.dur-label { font-family:'IBM Plex Mono',monospace; font-size:10px; color:#3a4a5c; margin-bottom: 5px; }

/* ── Risk item ── */
.risk-item {
    display: flex; gap: 12px; align-items: flex-start;
    padding: 10px 0; border-bottom: 1px solid #0e1219;
}
.risk-icon {
    width: 30px; height: 30px; border-radius: 3px;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px; flex-shrink: 0;
}
.risk-title { font-family:'IBM Plex Mono',monospace; font-size:10px; font-weight:600; margin-bottom:4px; }
.risk-desc  { font-size: 10px; color: #3a4a5c; line-height: 1.6; }

/* Plotly charts transparent */
.js-plotly-plot .plotly { background: transparent !important; }

/* Streamlit elements overrides */
div[data-testid="metric-container"] { display: none !important; }
.stSelectbox > div > div { 
    background: #0c0f14 !important; 
    border: 1px solid #1a2030 !important; 
    color: #c8d4e0 !important;
    font-family: 'IBM Plex Mono', monospace !important;
    font-size: 11px !important;
}
</style>
""", unsafe_allow_html=True)

# ─── DATA ───────────────────────────────────────────────────────────────────────
YEARS = ['1T21','1T22','1T23','1T24','1T25','1T26E','1T27E','1T28E','1T29E','1T30E']

PL = {
    'Receita Líquida': [759, 951, 2459, 2005, 4424, 5088, 5851, 6728, 7738, 8898],
    'EBITDA':          [442, 474, 1169, 1089, 1528, 1846, 1973, 2089, 2185, 2252],
    'Margem EBITDA %': [58.2,49.8,47.5,54.3,34.5,36.3,33.7,31.1,28.2,25.3],
}

CF = {
    'EBITDA':          [442,  474,  1169, 1089, 1528, 1757, 2021, 2324, 2672, 3073],
    'Δ Capital Giro':  [265, -194,  -353,   52, -395,  -80,  -95, -110, -130, -150],
    'Impostos':        [-20,  -14,  -138,  -46,  -99, -180, -210, -245, -285, -330],
    'Outros':          [-58,   -9,  -104,    9,  -15,  -18,  -20,  -23,  -27,  -31],
    'CFO':             [629,  257,   574, 1105, 1018, 1479, 1696, 1946, 2230, 2562],
    'CAPEX':          [-443,-2314,  -341, -602, -916,-1000,-1150,-1320,-1520,-1750],
    'FCL':             [186,-2057,   233,  503,  102,  479,  546,  626,  710,  812],
    'Captação':        [160, 1699,    31,   38, 1794,  500,  300,  200,  100,    0],
    'Amort. Principal':[ -4,  -11,   -26,  -76, -234, -300, -400, -500, -600, -700],
    'Juros':           [-45,  -52,  -317, -480, -384, -350, -320, -290, -260, -230],
    'Caixa Final':    [-934, -934,   344, 2388, 4766, 4945, 4951, 4887, 4747, 4549],
}

WACC_COMP = [
    ('Risk Free (Brasil)',    '10,50%', 'NTN-B longa'),
    ('Beta Alavancado',       '0,90x',  'Risco sistêmico utilities'),
    ('Equity Risk Premium',   '5,50%',  'Prêmio de risco de mercado'),
    ('Cost of Equity (Ke)',   '15,45%', 'Retorno exigido acionistas'),
    ('Cost of Debt (Kd)',     '11,50%', 'Custo nominal da dívida'),
    ('After-tax Cost of Debt','7,59%',  'Benefício fiscal 34%'),
    ('WACC',                  '11,68%', 'Taxa de desconto ponderada'),
]

SENS = {
    'g':   [2.5, 3.0, 3.5, 4.0, 4.5],
    10.68: [41.26, 43.74, 46.58, 49.83, 53.62],
    11.18: [38.06, 40.22, 42.65, 45.42, 48.60],
    11.68: [35.22, 37.10, 39.20, 41.58, 44.29],
    12.18: [32.68, 34.33, 36.16, 38.22, 40.54],
    12.68: [30.39, 31.84, 33.45, 35.24, 37.25],
}

LEVERAGE = [2.1, 8.4, 3.2, 2.8, 4.1, 3.6, 3.0, 2.5, 2.0, 1.6]

PLOT_LAYOUT = dict(
    paper_bgcolor='#0c0f14',
    plot_bgcolor='#0c0f14',
    font=dict(family='IBM Plex Mono', size=10, color='#4a5a6c'),
    margin=dict(l=8, r=8, t=10, b=8),
    legend=dict(bgcolor='transparent', font=dict(size=10)),
    xaxis=dict(gridcolor='#111820', showline=False, tickfont=dict(size=9)),
    yaxis=dict(gridcolor='#111820', showline=False, tickfont=dict(size=9)),
    hovermode='x unified',
)

def bar_color(vals, pos='rgba(16,185,129,0.35)', neg='rgba(239,68,68,0.35)'):
    return [pos if v >= 0 else neg for v in vals]

def border_color(vals, pos='#10b981', neg='#ef4444'):
    return [pos if v >= 0 else neg for v in vals]

# ─── TOPBAR ─────────────────────────────────────────────────────────────────────
st.markdown("""
<div class="topbar">
  <div class="topbar-logo">
    AGK<span class="accent">Research</span>
    &nbsp;·&nbsp;
    <span style="font-size:12px;color:#2a3a4c">ENEVA S.A. · ENEV3 · BM&FBOVESPA</span>
  </div>
  <div class="topbar-meta">
    <span class="dot"></span>Galapagos Capital Challenge &nbsp;|&nbsp; R$ MM<br>
    Adrian Raposo &nbsp;·&nbsp; Gustavo Cavalheiro &nbsp;·&nbsp; Kauê Balbino
  </div>
</div>
""", unsafe_allow_html=True)

# ─── TABS ───────────────────────────────────────────────────────────────────────
tabs = st.tabs(["⚡ Overview", "💰 Cash Flow", "📊 Valuation", "⚠️ Riscos", "🏦 Debênture"])


# ══════════════════════════════════════════════════════════════════════════════
# TAB 1 — OVERVIEW
# ══════════════════════════════════════════════════════════════════════════════
with tabs[0]:
    st.markdown('<div class="sec-hdr">Eneva S.A. — Visão Geral · Modelo R2W · Projeção 2021–2030E</div>', unsafe_allow_html=True)

    # KPIs
    k1,k2,k3,k4,k5 = st.columns(5)
    kpis = [
        (k1, "blue",  "Receita 2030E",     "R$ 8,9bi",  "+11x vs 2021",        "pos"),
        (k2, "green", "EBITDA 2030E",       "R$ 2,25bi", "+5,1x vs 2021",       "pos"),
        (k3, "red",   "Margem EBITDA 2030E","25,3%",     "vs 58,2% em 2021",    "neg"),
        (k4, "amber", "Capacidade",         "~7,2 GW",   "Predom. gás natural", "neu"),
        (k5, "teal",  "Preço-Alvo",         "R$ 39,20",  "Upside +43,7%",       "pos"),
    ]
    for col, color, lbl, val, sub, sub_class in kpis:
        with col:
            st.markdown(f"""
            <div class="kpi-card {color}">
              <div class="kpi-label">{lbl}</div>
              <div class="kpi-value">{val}</div>
              <div class="kpi-sub {sub_class}">{sub}</div>
            </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)

    # Charts row
    c1, c2 = st.columns([3, 1])
    with c1:
        st.markdown('<div class="sec-hdr">Receita Líquida & EBITDA (R$ MM)</div>', unsafe_allow_html=True)
        fig = go.Figure()
        fig.add_trace(go.Bar(
            name='Receita Líquida', x=YEARS, y=PL['Receita Líquida'],
            marker_color='rgba(59,130,246,0.25)', marker_line_color='#3b82f6', marker_line_width=1,
        ))
        fig.add_trace(go.Bar(
            name='EBITDA', x=YEARS, y=PL['EBITDA'],
            marker_color='rgba(16,185,129,0.35)', marker_line_color='#10b981', marker_line_width=1,
        ))
        fig.update_layout(**PLOT_LAYOUT, height=220, barmode='group')
        st.plotly_chart(fig, use_container_width=True, config={'displayModeBar': False})

    with c2:
        st.markdown('<div class="sec-hdr">Margem EBITDA (%)</div>', unsafe_allow_html=True)
        fig2 = go.Figure()
        fig2.add_trace(go.Scatter(
            x=YEARS, y=PL['Margem EBITDA %'],
            mode='lines+markers',
            line=dict(color='#f59e0b', width=2),
            marker=dict(size=5, color='#f59e0b'),
            fill='tozeroy', fillcolor='rgba(245,158,11,0.07)',
            name='Margem EBITDA',
        ))
        fig2.update_layout(**PLOT_LAYOUT, height=220,
            yaxis=dict(gridcolor='#111820', ticksuffix='%', tickfont=dict(size=9)))
        st.plotly_chart(fig2, use_container_width=True, config={'displayModeBar': False})

    # P&L Table
    st.markdown('<div class="sec-hdr">Demonstrativo de Resultados — 2021–2030E (R$ MM)</div>', unsafe_allow_html=True)
    df_pl = pd.DataFrame(PL, index=YEARS).T
    df_pl.index.name = 'Métrica'

    def style_pl(df):
        styles = pd.DataFrame('', index=df.index, columns=df.columns)
        for col in df.columns:
            for row in df.index:
                v = df.loc[row, col]
                if row == 'Margem EBITDA %':
                    if isinstance(v, float) and v < 35:
                        styles.loc[row, col] = 'color: #ef4444'
                    else:
                        styles.loc[row, col] = 'color: #10b981'
        return styles

    st.dataframe(
        df_pl.style.apply(style_pl, axis=None)
             .format(lambda x: f"{x:.1f}%" if isinstance(x, float) else f"{int(x):,}"),
        use_container_width=True, height=130
    )

    # Peers + Model
    p1, p2 = st.columns(2)
    with p1:
        st.markdown('<div class="sec-hdr">Modelo R2W — Principais Ativos</div>', unsafe_allow_html=True)
        st.markdown("""
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px">
          <div style="background:#0c0f14;border:1px solid #1a2030;border-radius:3px;padding:10px">
            <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;text-transform:uppercase;letter-spacing:.1em;color:#3b82f6;margin-bottom:6px">Complexos Principais</div>
            <div style="font-size:11px;color:#8896a8;line-height:1.9">
              Complexo Parnaíba (MA)<br>Complexo Sergipe (SE)<br>UTE Azulão (AM)<br>UTE Jaguatirica II (AM)
            </div>
          </div>
          <div style="background:#0c0f14;border:1px solid #1a2030;border-radius:3px;padding:10px">
            <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;text-transform:uppercase;letter-spacing:.1em;color:#3b82f6;margin-bottom:6px">Diversificação</div>
            <div style="font-size:11px;color:#8896a8;line-height:1.9">
              UTE Itaqui (MA)<br>UTE Pecém II (CE)<br>Presença: MA·SE·AM·CE·ES·BA<br>~7,2 GW capacidade total
            </div>
          </div>
        </div>
        <div class="info-box green" style="margin-top:8px">
          <span class="lbl" style="color:#10b981">▶ Tese</span>
          Modelo integrado Rock-to-Wire (R2W) aumenta previsibilidade de fluxo de caixa e controle de margens ao longo de toda a cadeia produtiva.
        </div>
        """, unsafe_allow_html=True)

    with p2:
        st.markdown('<div class="sec-hdr">Comparativo de Pares — EV/EBITDA Forward</div>', unsafe_allow_html=True)
        peers = {
            'Empresa': ['Engie Brasil','Eletrobras','Auren Energia','Eneva (implícito)'],
            'Ticker':  ['EGIE3','ELET3','AURE3','ENEV3'],
            'EV/EBITDA': ['9,3x','8,5x','7,5x','12,1x'],
            'Perfil': ['Alta visib.','Turnaround','Renováveis','R2W + Contr.'],
        }
        df_peers = pd.DataFrame(peers)
        st.dataframe(df_peers, use_container_width=True, hide_index=True, height=175)
        st.markdown("""
        <div class="info-box blue">
          <span class="lbl" style="color:#3b82f6">▶ Valuation</span>
          Prêmio de múltiplo justificado pelo modelo integrado, contratos de longo prazo (PPAs) e crescimento estrutural de receita 11x no período.
        </div>
        """, unsafe_allow_html=True)


# ══════════════════════════════════════════════════════════════════════════════
# TAB 2 — CASH FLOW
# ══════════════════════════════════════════════════════════════════════════════
with tabs[1]:
    st.markdown('<div class="sec-hdr">Dinâmica de Geração de Caixa e Intensidade de Capital · 2021–2030E</div>', unsafe_allow_html=True)

    # KPIs
    k1,k2,k3,k4,k5 = st.columns(5)
    for col, color, lbl, val, sub, sc in [
        (k1,'green','CFO 2030E',    'R$2,56bi', 'vs R$629mm em 2021', 'pos'),
        (k2,'red',  'CAPEX Pico',   '(R$2,31bi)','2022 · growth-driven','neg'),
        (k3,'amber','FCL 2022',     '(R$2,06bi)','Pior ano do ciclo',   'neg'),
        (k4,'blue', 'FCL 2030E',    'R$812mm',  'Normalização gradual','pos'),
        (k5,'teal', 'Caixa 2030E',  'R$4,55bi', 'Conforto estrutural', 'pos'),
    ]:
        with col:
            st.markdown(f"""
            <div class="kpi-card {color}">
              <div class="kpi-label">{lbl}</div>
              <div class="kpi-value">{val}</div>
              <div class="kpi-sub {sc}">{sub}</div>
            </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)

    c1, c2 = st.columns([3, 1])
    with c1:
        st.markdown('<div class="sec-hdr">CFO · CAPEX · FCL (R$ MM)</div>', unsafe_allow_html=True)
        fig = go.Figure()
        fig.add_trace(go.Bar(
            name='CFO', x=YEARS, y=CF['CFO'],
            marker_color='rgba(16,185,129,0.3)', marker_line_color='#10b981', marker_line_width=1))
        fig.add_trace(go.Bar(
            name='CAPEX', x=YEARS, y=CF['CAPEX'],
            marker_color='rgba(239,68,68,0.3)', marker_line_color='#ef4444', marker_line_width=1))
        fig.add_trace(go.Scatter(
            name='FCL', x=YEARS, y=CF['FCL'],
            mode='lines+markers', line=dict(color='#3b82f6', width=2),
            marker=dict(size=5)))
        fig.update_layout(**PLOT_LAYOUT, height=220, barmode='group')
        st.plotly_chart(fig, use_container_width=True, config={'displayModeBar': False})

    with c2:
        st.markdown('<div class="sec-hdr">Caixa Final (R$ MM)</div>', unsafe_allow_html=True)
        fig2 = go.Figure()
        fig2.add_trace(go.Scatter(
            x=YEARS, y=CF['Caixa Final'],
            mode='lines+markers',
            line=dict(color='#06b6d4', width=2),
            marker=dict(size=5, color=[('#06b6d4' if v>=0 else '#ef4444') for v in CF['Caixa Final']]),
            fill='tozeroy', fillcolor='rgba(6,182,212,0.06)',
        ))
        fig2.update_layout(**PLOT_LAYOUT, height=220)
        st.plotly_chart(fig2, use_container_width=True, config={'displayModeBar': False})

    # Full table
    st.markdown('<div class="sec-hdr">Cash Flow Bridge Completo (R$ MM)</div>', unsafe_allow_html=True)
    df_cf = pd.DataFrame(CF, index=YEARS).T
    df_cf.index.name = 'Linha'

    HIGHLIGHT_ROWS = {'CFO', 'FCL'}

    def style_cf(df):
        styles = pd.DataFrame('', index=df.index, columns=df.columns)
        for row in df.index:
            for col in df.columns:
                v = df.loc[row, col]
                if isinstance(v, (int, float)):
                    if row in HIGHLIGHT_ROWS:
                        styles.loc[row, col] = 'color: #e2eaf4; font-weight: 600'
                    elif v > 0:
                        styles.loc[row, col] = 'color: #10b981'
                    elif v < 0:
                        styles.loc[row, col] = 'color: #ef4444'
        return styles

    def fmt_cf(v):
        if isinstance(v, (int, float)):
            if v < 0:
                return f"({abs(int(v)):,})"
            return f"{int(v):,}"
        return str(v)

    st.dataframe(
        df_cf.style.apply(style_cf, axis=None).format(fmt_cf),
        use_container_width=True, height=380,
    )

    # 4 leituras
    st.markdown('<div class="sec-hdr" style="margin-top:20px">Leituras Estruturais</div>', unsafe_allow_html=True)
    l1, l2 = st.columns(2)
    with l1:
        st.markdown("""
        <div class="risk-item">
          <div class="risk-icon" style="background:#0f1f3d;font-size:13px">①</div>
          <div>
            <div class="risk-title" style="color:#3b82f6">Escala operacional com crescimento estrutural</div>
            <div class="risk-desc">EBITDA 442→3.073 · CFO 629→2.562 — expansão consistente da geração operacional. Ativo com forte capacidade de caixa recorrente.</div>
          </div>
        </div>
        <div class="risk-item">
          <div class="risk-icon" style="background:#2c1600;font-size:13px">②</div>
          <div>
            <div class="risk-title" style="color:#f59e0b">Modelo intensivo em capital (growth-driven CAPEX)</div>
            <div class="risk-desc">Pico -2.314 (2022), normaliza para ~-1.750 em 2030. Crescimento ancorado em expansão contínua de base de ativos.</div>
          </div>
        </div>
        """, unsafe_allow_html=True)
    with l2:
        st.markdown("""
        <div class="risk-item">
          <div class="risk-icon" style="background:#300a0a;font-size:13px">③</div>
          <div>
            <div class="risk-title" style="color:#ef4444">Conversão de caixa cíclica</div>
            <div class="risk-desc">FCF: -2.057 (2022) → ~+812 (2030). Forte volatilidade no curto prazo com recuperação gradual dependente do ciclo de investimento.</div>
          </div>
        </div>
        <div class="risk-item" style="border:none">
          <div class="risk-icon" style="background:#042f22;font-size:13px">④</div>
          <div>
            <div class="risk-title" style="color:#10b981">Funding como variável estrutural</div>
            <div class="risk-desc">Captações relevantes recorrentes (ex: R$1,79bi em 2025). Estrutura de capital dependente de acesso contínuo ao mercado de dívida.</div>
          </div>
        </div>
        """, unsafe_allow_html=True)


# ══════════════════════════════════════════════════════════════════════════════
# TAB 3 — VALUATION
# ══════════════════════════════════════════════════════════════════════════════
with tabs[2]:
    st.markdown('<div class="sec-hdr">DCF Valuation · WACC · Análise de Sensibilidade</div>', unsafe_allow_html=True)

    # Val summary strip
    v1,v2,v3,v4,v5 = st.columns(5)
    for col, color, lbl, val, sub, sc in [
        (v1,'blue',  'Enterprise Value (EV)', 'R$78,9bi',  'DCF base case',          'neu'),
        (v2,'red',   'Dívida Líquida 4T25',   '(R$16,9bi)','Bridge EV→Equity',       'neg'),
        (v3,'green', 'Equity Value',           'R$61,9bi',  'EV menos dívida líq.',   'pos'),
        (v4,'amber', 'Preço-Alvo',             'R$ 39,20',  'por ação · base case',   'neu'),
        (v5,'teal',  'Upside vs Cotação',      '+43,7%',    'Recomendação: COMPRA',   'pos'),
    ]:
        with col:
            st.markdown(f"""
            <div class="kpi-card {color}">
              <div class="kpi-label">{lbl}</div>
              <div class="kpi-value">{val}</div>
              <div class="kpi-sub {sc}">{sub}</div>
            </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)

    w1, w2 = st.columns(2)

    with w1:
        st.markdown('<div class="sec-hdr">Composição do WACC (11,68%)</div>', unsafe_allow_html=True)
        rows_html = ""
        for key, val, desc in WACC_COMP:
            is_wacc = key == 'WACC'
            is_ke   = key == 'Cost of Equity (Ke)'
            style = "background:#0f1f3d;border-radius:3px;padding:8px;margin-top:4px;border:1px solid #1e3a5f;" if is_wacc else ""
            vcolor = "#3b82f6" if (is_wacc or is_ke) else "#e2eaf4"
            rows_html += f"""
            <div class="wacc-row" style="{style}">
              <span class="wacc-key">{key}</span>
              <span class="wacc-desc">{desc}</span>
              <span class="wacc-val" style="color:{vcolor}">{val}</span>
            </div>"""
        st.markdown(f'<div style="background:#0c0f14;border:1px solid #1a2030;border-radius:4px;padding:12px 14px">{rows_html}</div>', unsafe_allow_html=True)

    with w2:
        st.markdown('<div class="sec-hdr">Sensibilidade — Preço-Alvo (WACC × g)</div>', unsafe_allow_html=True)
        wacc_rows = [10.68, 11.18, 11.68, 12.18, 12.68]
        df_sens = pd.DataFrame(
            {f"g={g}%" : SENS[w] for g, w in zip(SENS['g'], wacc_rows)},
            index=[f"{w}%" for w in wacc_rows]
        )
        df_sens.index.name = 'WACC \\ g'

        def style_sens(df):
            styles = pd.DataFrame('', index=df.index, columns=df.columns)
            for r in df.index:
                for c in df.columns:
                    v = df.loc[r, c]
                    if abs(v - 39.20) < 0.01:
                        styles.loc[r, c] = 'background-color: #172554; color: #93c5fd; font-weight: 700'
                    elif float(r.replace('%','')) >= 12.18:
                        styles.loc[r, c] = 'color: #ef4444'
                    elif v > 42:
                        styles.loc[r, c] = 'color: #10b981'
            return styles

        st.dataframe(
            df_sens.style.apply(style_sens, axis=None).format("R${:.2f}"),
            use_container_width=True, height=220,
        )
        st.markdown('<p style="font-family:IBM Plex Mono,monospace;font-size:9px;color:#2a3a4c;margin-top:4px">Azul = preço-alvo base R$39,20 · Vermelho = cenário stress</p>', unsafe_allow_html=True)

    # Stress scenarios
    st.markdown('<div class="sec-hdr" style="margin-top:16px">Cenários de Stress</div>', unsafe_allow_html=True)
    df_stress = pd.DataFrame({
        'Variável':      ['Spread de Crédito', 'CAPEX Anual', 'Preço-Alvo (stress)'],
        'Cenário Base':  ['160 bps', 'R$1,35bi (médio)', 'R$39,20'],
        'Cenário Stress':['360 bps (+200bps)', 'R$1,55bi (+15%)', 'R$30,24'],
        'Impacto':       ['WACC → 12,72%', 'FCL anual -R$500MM', 'Queda de 22,8%'],
        'Severidade':    ['Alto', 'Médio', 'Alto'],
    })
    st.dataframe(df_stress, use_container_width=True, hide_index=True, height=140)

    # EV Bridge
    st.markdown('<div class="sec-hdr" style="margin-top:16px">EV → Equity Value Bridge (R$ MM)</div>', unsafe_allow_html=True)
    bridge_vals = [78895, -16955, 61940]
    bridge_lbls = ['Enterprise Value (EV)', '(-) Dívida Líquida 4T25', 'Equity Value']
    fig_b = go.Figure(go.Waterfall(
        name='Bridge', orientation='v',
        measure=['absolute','relative','total'],
        x=bridge_lbls, y=bridge_vals,
        connector=dict(line=dict(color='#1a2030', width=1)),
        increasing=dict(marker_color='rgba(59,130,246,0.4)', marker_line_color='#3b82f6', marker_line_width=1),
        decreasing=dict(marker_color='rgba(239,68,68,0.4)', marker_line_color='#ef4444', marker_line_width=1),
        totals=dict(marker_color='rgba(16,185,129,0.4)', marker_line_color='#10b981', marker_line_width=1),
        text=[f"R${abs(v/1000):.1f}bi" for v in bridge_vals],
        textposition='outside', textfont=dict(family='IBM Plex Mono', size=11, color='#e2eaf4'),
    ))
    fig_b.update_layout(**PLOT_LAYOUT, height=240, showlegend=False,
        yaxis=dict(gridcolor='#111820', tickformat=',', tickfont=dict(size=9)))
    st.plotly_chart(fig_b, use_container_width=True, config={'displayModeBar': False})


# ══════════════════════════════════════════════════════════════════════════════
# TAB 4 — RISCOS
# ══════════════════════════════════════════════════════════════════════════════
with tabs[3]:
    st.markdown('<div class="sec-hdr">Riscos Estruturais — Eneva S.A.</div>', unsafe_allow_html=True)

    r1, r2 = st.columns(2)

    with r1:
        st.markdown('<div class="sec-hdr">Matriz de Riscos</div>', unsafe_allow_html=True)
        risks = [
            ("⚡","#300a0a","#ef4444","Sensibilidade ao custo de funding","Alto",
             "CAPEX recorrente (~R$1,75bi em 2030) implica WACC estruturalmente relevante. Pequenas variações em juros/spreads têm impacto material no valor presente."),
            ("🔄","#300a0a","#ef4444","Risco de refinanciamento","Alto",
             "Dependência de captações relevantes (ex: R$1,79bi em 2025). Fechamento de mercado pode forçar uso do caixa acumulado."),
            ("⏱","#2c1600","#f59e0b","Mismatch de duration","Médio",
             "Ativos 15–20 anos vs. dívida ~6 anos. Gera dependência de rolagem e renegociação contínua de passivos."),
            ("📉","#2c1600","#f59e0b","Descompasso EBITDA × CAPEX","Médio",
             "Geração operacional cresce com defasagem vs. desembolso imediato. Pressão sobre Dívida Líquida/EBITDA e covenants."),
        ]
        for icon, bg, color, title, sev, desc in risks:
            st.markdown(f"""
            <div class="risk-item">
              <div class="risk-icon" style="background:{bg}">{icon}</div>
              <div>
                <div class="risk-title" style="color:{color}">{title} &nbsp;
                  <span class="chip {'chip-r' if sev=='Alto' else 'chip-a'}">{sev}</span>
                </div>
                <div class="risk-desc">{desc}</div>
              </div>
            </div>""", unsafe_allow_html=True)

    with r2:
        st.markdown('<div class="sec-hdr">EBITDA vs CAPEX (Absoluto) — R$ MM</div>', unsafe_allow_html=True)
        fig = go.Figure()
        fig.add_trace(go.Scatter(
            name='EBITDA', x=YEARS, y=CF['EBITDA'],
            mode='lines+markers', line=dict(color='#10b981', width=2),
            marker=dict(size=4)))
        fig.add_trace(go.Scatter(
            name='|CAPEX|', x=YEARS, y=[abs(v) for v in CF['CAPEX']],
            mode='lines+markers', line=dict(color='#ef4444', width=2, dash='dot'),
            marker=dict(size=4)))
        fig.update_layout(**PLOT_LAYOUT, height=190)
        st.plotly_chart(fig, use_container_width=True, config={'displayModeBar': False})

        st.markdown('<div class="sec-hdr">Dívida Líquida / EBITDA — Alavancagem</div>', unsafe_allow_html=True)
        colors_lev = ['rgba(239,68,68,0.5)' if v > 4 else 'rgba(245,158,11,0.5)' if v > 3 else 'rgba(16,185,129,0.5)' for v in LEVERAGE]
        borders_lev= ['#ef4444' if v > 4 else '#f59e0b' if v > 3 else '#10b981' for v in LEVERAGE]
        fig2 = go.Figure()
        fig2.add_trace(go.Bar(
            x=YEARS, y=LEVERAGE,
            marker_color=colors_lev, marker_line_color=borders_lev, marker_line_width=1,
            name='DL/EBITDA',
        ))
        fig2.add_hline(y=3.5, line_dash='dot', line_color='#f59e0b',
                       annotation_text='Covenant ref.', annotation_font_size=9)
        fig2.update_layout(**PLOT_LAYOUT, height=160,
            yaxis=dict(gridcolor='#111820', ticksuffix='x', tickfont=dict(size=9)))
        st.plotly_chart(fig2, use_container_width=True, config={'displayModeBar': False})

        # Duration mismatch
        st.markdown('<div class="sec-hdr">Duration Mismatch — Ativo vs Passivo</div>', unsafe_allow_html=True)
        st.markdown("""
        <div class="dur-label">Duration Ativos/CAPEX</div>
        <div class="dur-bar" style="background:#3b82f6;width:100%">
          <span style="font-family:'IBM Plex Mono',monospace;font-size:10px;color:#fff;font-weight:600">25 anos</span>
        </div>
        <div class="dur-label">Duration Dívida Atual</div>
        <div class="dur-bar" style="background:#ef4444;width:24%">
          <span style="font-family:'IBM Plex Mono',monospace;font-size:10px;color:#fff;font-weight:600">6 anos</span>
        </div>
        <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#2a3a4c;margin-top:4px">→ Gap de 19 anos → risco estrutural de rolagem</div>
        """, unsafe_allow_html=True)


# ══════════════════════════════════════════════════════════════════════════════
# TAB 5 — DEBÊNTURE
# ══════════════════════════════════════════════════════════════════════════════
with tabs[4]:
    st.markdown('<div class="sec-hdr">Otimização da Estrutura de Capital · Debênture de Infraestrutura · Lei 12.431</div>', unsafe_allow_html=True)

    # KPIs
    k1,k2,k3,k4,k5 = st.columns(5)
    for col, color, lbl, val, sub, sc in [
        (k1,'blue',  'Prazo da Emissão',  '20–30 anos','Alinhado à duration dos ativos','neu'),
        (k2,'green', 'Carência',          '5 anos',    'Compatível com ramp-up',         'pos'),
        (k3,'amber', 'Indexação',         'IPCA+',     'Alinhado às receitas PPAs',      'neu'),
        (k4,'teal',  'Uso — Refinanc.',   '~60%',      'Dívidas 2025–2027',              'neu'),
        (k5,'green', 'Uso — Investim.',   '~40%',      'Geração distr. + solar',         'pos'),
    ]:
        with col:
            st.markdown(f"""
            <div class="kpi-card {color}">
              <div class="kpi-label">{lbl}</div>
              <div class="kpi-value">{val}</div>
              <div class="kpi-sub {sc}">{sub}</div>
            </div>""", unsafe_allow_html=True)

    st.markdown("<br>", unsafe_allow_html=True)

    d1, d2 = st.columns([1, 2])

    with d1:
        st.markdown('<div class="sec-hdr">Contexto · Problema · Solução</div>', unsafe_allow_html=True)
        st.markdown("""
        <div class="info-box blue">
          <span class="lbl" style="color:#3b82f6">Contexto</span>
          CAPEX 2025–2030: R$1,0–1,75bi/ano<br>
          EBITDA 2030: ~R$3,1bi (vs R$442mm em 2021)<br>
          FCL positivo a partir de 2025: ~R$100–800mm/ano
        </div>
        <div class="info-box red">
          <span class="lbl" style="color:#ef4444">Problema</span>
          Descasamento entre investimentos de longo prazo e dívida de médio prazo.<br>
          Pressão de liquidez 2025–2027 com concentração de vencimentos.<br>
          Amortizações antecipadas reduzem eficiência do caixa no período de expansão.
        </div>
        <div class="info-box green">
          <span class="lbl" style="color:#10b981">Solução — Lei 12.431</span>
          Prazo longo (20–30 anos) · IPCA + spread<br>
          Carência de 5 anos · Alinhado às receitas de energia (PPAs)<br>
          Reduz risco de refinanciamento e melhora previsibilidade financeira.
        </div>
        """, unsafe_allow_html=True)

    with d2:
        st.markdown('<div class="sec-hdr">Duration Proposta vs Estrutura Atual (anos)</div>', unsafe_allow_html=True)
        fig_dur = go.Figure(go.Bar(
            x=['Dívida Atual', 'Debenture\n(mínimo)', 'Debenture\n(máximo)', 'Duration\nAtivo/CAPEX'],
            y=[6, 20, 30, 25],
            orientation='v',
            marker_color=['rgba(239,68,68,0.4)','rgba(59,130,246,0.4)','rgba(16,185,129,0.4)','rgba(245,158,11,0.4)'],
            marker_line_color=['#ef4444','#3b82f6','#10b981','#f59e0b'],
            marker_line_width=1,
            text=['6a','20a','30a','25a'],
            textposition='outside',
            textfont=dict(family='IBM Plex Mono', size=11, color='#e2eaf4'),
        ))
        fig_dur.update_layout(**PLOT_LAYOUT, height=220, showlegend=False,
            yaxis=dict(gridcolor='#111820', ticksuffix='a', tickfont=dict(size=9)))
        st.plotly_chart(fig_dur, use_container_width=True, config={'displayModeBar': False})

        st.markdown("""
        <div class="info-box blue">
          <span class="lbl" style="color:#3b82f6">▶ Tese de Crédito</span>
          A estrutura reduz o risco de refinanciamento ao alongar o passivo e alinhar melhor o prazo da dívida ao ciclo dos ativos, diminuindo a volatilidade do fluxo de caixa e melhorando a previsibilidade financeira da companhia para o período de expansão 2025–2030.
        </div>
        """, unsafe_allow_html=True)

        d2a, d2b = st.columns(2)
        with d2a:
            st.markdown("""
            <div style="background:#300a0a;border:1px solid #7f1d1d;border-radius:3px;padding:10px;text-align:center">
              <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#ef4444">Duration Atual</div>
              <div style="font-family:'IBM Plex Mono',monospace;font-size:18px;font-weight:700;color:#ef4444;margin-top:4px">~6 anos</div>
            </div>""", unsafe_allow_html=True)
        with d2b:
            st.markdown("""
            <div style="background:#042f22;border:1px solid #064e3b;border-radius:3px;padding:10px;text-align:center">
              <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#10b981">Duration Proposta</div>
              <div style="font-family:'IBM Plex Mono',monospace;font-size:18px;font-weight:700;color:#10b981;margin-top:4px">20–30 anos</div>
            </div>""", unsafe_allow_html=True)

    # Footer
    st.markdown("<br>", unsafe_allow_html=True)
    st.markdown("""
    <div style="background:#0c0f14;border:1px solid #1a2030;border-radius:4px;padding:14px 18px;display:flex;gap:32px;align-items:center;flex-wrap:wrap">
      <div>
        <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#2a3a4c;margin-bottom:3px">Pricing de Emissão — Site do Grupo AGK</div>
        <a href="https://sites.google.com/view/gustavo-cavalheiro-challange/in%C3%ADcio" target="_blank"
           style="font-family:'IBM Plex Mono',monospace;font-size:11px;color:#3b82f6;text-decoration:none">
           sites.google.com/view/gustavo-cavalheiro-challange ↗
        </a>
      </div>
      <div>
        <div style="font-family:'IBM Plex Mono',monospace;font-size:9px;color:#2a3a4c;margin-bottom:3px">Contatos — Grupo AGK</div>
        <div style="font-size:10px;color:#4a5a6c;line-height:1.9">
          Adrian Raposo · contate.raposo@gmail.com<br>
          Gustavo Cavalheiro · gustavovences01@gmail.com<br>
          Kauê Balbino · kauewallacy46@gmail.com
        </div>
      </div>
    </div>
    """, unsafe_allow_html=True)
