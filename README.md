# Monitor de Inteligencia Comercial Colombia–Canadá (MIC-CC)
## Colombia–Canada Commercial Intelligence Monitor

**Observatory of Economics and Markets (ODEM) · Universidad EAN · Bogotá, Colombia**

![Phase](https://img.shields.io/badge/Phase-3%20Logistics%20(in%20progress)-blue)
![Python](https://img.shields.io/badge/Python-3.10%2B-green)
![Status](https://img.shields.io/badge/Status-Active%20Research-brightgreen)
![License](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey)

---

## Abstract

This project develops a quantitative commercial intelligence monitor for the Colombia–Canada bilateral trade corridor, integrating three analytical dimensions: **commercial trade flows** (Revealed Comparative Advantage and export concentration), **logistics costs** (maritime freight, port handling, and domestic transport for two competing routes), and **financial risk** (COP/CAD exchange rate volatility and scenario simulation).

Using official data from UN Comtrade, SPRC, Port of Montreal, GCT Vancouver, and central bank APIs, the monitor identifies the Top 10 Colombian export products with sustained comparative advantage toward Canada (2020–2024), models the full export cost under FOB, CIF, and DAP Incoterms for the Cartagena–Montreal and Cartagena–Vancouver routes, and simulates the impact of three exchange rate scenarios on net exporter margins.

The final deliverables include an interactive Power BI dashboard, a fully documented Python pipeline, and a Working Paper published under the ODEM–EAN institutional seal. The project is designed to support SME exporters, trade policy makers (MinCIT, ProColombia), and serves as a research portfolio for the Mitacs GRI program application.

**Research question:** *Which Colombian export products hold a real and sustained comparative advantage toward Canada, what is their total logistics cost by route and Incoterm, and how does COP/CAD exchange rate volatility affect their net profitability?*

---

## Project Structure

```
MIC-CC/
│
├── README.md                          ← This file
├── requirements.txt                   ← Python dependencies
├── .gitignore
│
├── notebooks/
│   ├── 01_data_collection.ipynb       ← F1: UN Comtrade API + data consolidation
│   ├── 02_commercial_analysis.ipynb   ← F2: VCR Balassa + HHI + Top 10
│   ├── 03_logistics_model.ipynb       ← F3: Freight + ports + transport (in progress)
│   └── 04_financial_model.ipynb       ← F4: COP/CAD time series + VaR (pending)
│
├── data/
│   ├── raw/
│   │   ├── comtrade/                  ← Original CSV downloads from UN Comtrade
│   │   └── logistics/
│   │       ├── A1_fletes/             ← Maritime freight rates (SeaRates)
│   │       ├── A2_puertos/            ← Port tariffs and capacity reports
│   │       └── A3_transporte_interno/ ← Colombia domestic transport quotes
│   └── processed/
│       ├── commercial/                ← VCR results, HHI, Top 10 CSV
│       └── logistics/                 ← Normalized logistics datasets
│
├── outputs/
│   ├── figures/                       ← Charts and visualizations (PNG/SVG)
│   └── tables/                        ← Final tables for Working Paper
│
├── docs/
│   ├── methodology/                   ← Methodological notes and decisions
│   └── working_paper/                 ← Working Paper drafts
│
└── src/
    └── utils.py                       ← Shared helper functions
```

---

## Phase Status

| Phase | Period | Description | Status | Notebook |
|-------|--------|-------------|--------|----------|
| **F1** | May 1–15 | Data collection: UN Comtrade API + master dataset construction | ✅ Complete | `01_data_collection.ipynb` |
| **F2** | May 16–31 | Commercial analysis: VCR Balassa + HHI + Top 10 selection | ✅ Complete | `02_commercial_analysis.ipynb` |
| **F3** | Jun 1–20 | Logistics model: freight rates, port costs, domestic transport | 🔄 In Progress | `03_logistics_model.ipynb` |
| **F4** | Jun 21–Jul 10 | Financial model: COP/CAD time series, VaR, scenario simulation | ⏳ Pending | `04_financial_model.ipynb` |
| **F5** | Jul 11–25 | Power BI dashboard: 3 modules (Trade · Logistics · Finance) | ⏳ Pending | — |
| **F6** | Jul 26–Aug 15 | Working Paper: full draft + ODEM director review | ⏳ Pending | — |
| **F7** | Aug 16–31 | Publication: GitHub release + ODEM + Mitacs executive summary | ⏳ Pending | — |

### Phase 3 Sub-task Status

| Task | Description | Status |
|------|-------------|--------|
| A1 | Maritime freight rates (CTG–MTL, CTG–YVR) | ✅ Complete |
| A2 | Port tariffs and capacity (CTG, MTL, YVR) | ✅ Complete |
| A3 | Colombia domestic transport (plant → Port of Cartagena) | ⏳ Pending |
| A4 | Transit times (Cartagena to final destination) | ⏳ Pending |
| A5 | Cargo insurance (CIF standard rates by product) | ⏳ Pending |

---

## Key Findings: Phase 2 (Commercial Analysis)

### Top 10 Products — Revealed Comparative Advantage (VCR Balassa, 2020–2024)

| Rank | HS6 | Product | VCR Avg | Years VCR>1 | Avg Value (USD MM) |
|------|-----|---------|---------|-------------|-------------------|
| 1 | 090111 | Unroasted coffee | 4.95 | 5/5 | 235.79 |
| 2 | 270112 | Bituminous coal | 1.43 | 5/5 | 137.17 |
| 3 | 080450 | Guavas, mangoes, mangosteens | 27.18 | 5/5 | 5.44 |
| 4 | 060319 | Fresh flowers (other) | 1.61 | 5/5 | 27.60 |
| 5 | 300432 | Corticosteroid medications | 15.01 | 5/5 | 3.96 |
| 6 | 611596 | Synthetic fiber hosiery | 16.06 | 5/5 | 1.80 |
| 7 | 730792 | Iron pipe fittings | 9.99 | 5/5 | 8.59 |
| 8 | 030482 | Frozen trout fillets | 13.09 | 5/5 | 0.89 |
| 9 | 200819 | Processed nuts and seeds | 12.29 | 5/5 | 0.54 |
| 10 | 060311 | Fresh roses | 1.98 | 5/5 | 11.34 |

*Source: UN Comtrade Plus. Own calculations.*

### Main Commercial Findings

- Canada absorbs only **1.3%–2.0%** of total Colombian exports despite 15 years of the FTA (TLCC, in force since 2011).
- Products with VCR > 15 (mangoes, medications, hosiery) collectively represent **less than 2%** of total exports — a structural paradox: high comparative advantage, low market participation.
- The HHI concentration index fell from **0.236 (2020) to 0.154 (2023)**, then rose to **0.203 (2024)**. The apparent diversification was driven by petroleum growth, not by value-added manufactured goods.
- Pharmaceutical exports showed the strongest trend: VCR grew from **9.5 (2020) to 29.0 (2024)**.

### Phase 3 Preliminary Findings (Logistics)

- **CTG–YVR anomaly (verified):** Hapag-Lloyd's 40ft container (USD 4,610) is cheaper than 20ft (USD 5,281) on the transpacific route. Confirmed by re-verification. Explanation: low 20ft demand creates a capacity premium on that format in the Pacific corridor. At normalized USD/TEU rates: 40ft = USD 2,712/TEU vs 20ft = USD 5,281/TEU — the 40ft is strictly preferable on CTG–YVR.
- **Port utilization 2024:** Montreal at **58.6%** capacity (1.46M/2.5M TEU), Vancouver at **62.0%** (3.47M/5.6M TEU). Both ports operate with available capacity — no congestion surcharge risk for Colombian exports.
- **Destination handling differential:** YVR (GCT 2026 official) is **USD 485/TEU** (20ft) vs MTL proxy **USD 340/TEU** (20ft) — Montreal is approximately 30% cheaper in terminal handling.
- **Free time advantage:** Montreal offers 5 calendar days of free storage vs Vancouver's 3 working days (~4.2 calendar days equivalent), giving the Montreal route more operational flexibility in the supply chain.

---

## Data Sources

| Dimension | Source | Variables | Period | Access |
|-----------|--------|-----------|--------|--------|
| Commercial | UN Comtrade Plus | HS6 bilateral exports (COL→CAN, COL→World) | 2020–2024 | Public API |
| Logistics | SeaRates | Maritime freight rates CTG–MTL, CTG–YVR | Jun 2026 | Free (limited) |
| Logistics | SPRC (Puerto Cartagena) | Export handling tariffs 2026 | 2026 | Public PDF |
| Logistics | Port of Montreal / MGT | Capacity, throughput, storage tariffs | 2021/2024 | Public |
| Logistics | GCT Deltaport (Vancouver) | Terminal Services Tariff | Apr 2026 | Public PDF |
| Logistics | VFPA (Vancouver) | Port statistics 2024 | 2024 | Public PDF |
| Financial | Banco de la República | COP/USD TRM daily series | 2020–2024 | Public API |
| Financial | Bank of Canada (Valet API) | CAD/USD daily series | 2020–2024 | Public API |
| Financial | DIAN / MinCIT | TLCC preferential tariffs, Drawback | 2026 | Public |

---

## Methodological Decisions Log

All methodological adaptations are documented in `docs/methodology/`. Key decisions:

| ID | Decision | Rationale |
|----|----------|-----------|
| MD-01 | VCR filters: USD 500K minimum + VCR > 1 in ≥ 3/5 years | Eliminates episodic exports without economic relevance |
| MD-02 | TEU normalization factor: 1.7 for 40ft containers | Industry standard (1 40ft ≈ 1.7 TEU equivalent) |
| MD-03 | Minimum 3 carriers per route relaxed to 1+ (documented) | SeaRates and Freightos require corporate registration for multi-carrier access |
| MD-04 | CTG–YVR 40ft tariff inversion: treated as logistics finding, not error | Verified twice; explained by low 20ft demand on transpacific corridor |
| MD-05 | MTL handling: proxy = 70% of GCT–YVR 2026 rate | MGT does not publish current tariffs; MTL has lower congestion and labor costs than YVR |
| MD-06 | MTL storage: MGT 2021 tariff adjusted by Canadian CPI 2021–2026 (+21.8%) | No 2026 public tariff available; inflation adjustment = factor 1.218 |
| MD-07 | Data quality labels: OFICIAL / OFICIAL_BASE / PROXY_DERIVADO / PROXY_HISTORICO_AJUSTADO | Full traceability standard for Working Paper academic review |
| MD-08 | TRM COP/USD: 4,200 (proxy Jun 2026); CAD/USD: 0.735 (proxy Jun 2026) | Will be updated with official series in Phase 4 (BanRep + Bank of Canada APIs) |

---

## Installation & Reproduction

### Requirements

```bash
python >= 3.10
```

### Setup

```bash
# Clone the repository
git clone https://github.com/[username]/MIC-CC.git
cd MIC-CC

# Install dependencies
pip install -r requirements.txt
```

### Running the Notebooks

The notebooks are designed for **Google Colab**. Each notebook begins with a Drive mount cell. To run locally, replace the `FASE_PATH` variable with a local directory path.

```python
# Google Colab (default)
from google.colab import drive
drive.mount('/content/drive')
BASE_PATH = '/content/drive/MyDrive/MIC_CC'

# Local alternative
BASE_PATH = './data'
```

**Execution order:** `01` → `02` → `03` → `04`

Each notebook is self-contained and produces the input files required by the next notebook.

---

## Team

| Researcher | Role | Responsibilities |
|------------|------|-----------------|
| **Carlos** | Lead — Quantitative Analysis & Data | Python pipeline, VCR/HHI calculation, financial modeling (VaR, COP/CAD series), GitHub management, WP methodology section |
| **Manuela** | Lead — Logistics & Visualization | Power BI dashboard architecture, logistics cost model, LPI analysis, WP logistics section |
| **Laura** | Research Support | Data collection, source validation, auxiliary table construction, evidence documentation |
| **ODEM Director** | Academic Advisor | Methodological review, institutional validation, ODEM–EAN publication seal |

**Observatory:** ODEM — Línea de Negocios Internacionales · Universidad EAN  
**Period:** May – August 2026

---

## Citing This Work

```
Carlos & Manuela. (2026). Monitor de Inteligencia Comercial Colombia–Canadá (MIC-CC):
Análisis de flujos comerciales, costos logísticos y riesgo cambiario en el corredor
bilateral 2020–2024. Working Paper ODEM–Universidad EAN. Bogotá, Colombia.
https://github.com/clemuss14430/MIC-CC
```

---

## License

This project is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).  
You are free to share and adapt the material for any purpose, provided appropriate credit is given.

---

*ODEM — Observatory of Economics and Markets · Universidad EAN · Bogotá, Colombia · 2026*
