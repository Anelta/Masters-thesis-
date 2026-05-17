# ESG Regulation and Market Efficiency in BRICS+ Countries

Replication notebook for the master's thesis *"The Link Between ESG Regulation and
Market Efficiency in BRICS+ Countries"* (Anelia Kurakina, HSE University, 2026).

## Overview

The notebook (`Kurakina_diploma_clean.ipynb`) contains the full empirical pipeline:
data merging, market-model estimation, panel regressions, robustness checks, event
studies, and the figures reported in the thesis.

The final regression-ready panel covers:

- **9 BRICS+ countries** — China, India, Brazil, South Africa, Russia, United Arab
  Emirates, Saudi Arabia, Egypt, Indonesia
- **2017–2024**
- **28,647 firm-year observations** on **7,389 non-financial firms**

## Data sources

| Source | File expected in `DATA_DIR` | Description |
|---|---|---|
| S&P Trucost Environmental (WRDS) | `gxpvubiao4zdn1ro.csv` | Weighted Environmental Disclosure score (`di_329704`) and pillar variables |
| Compustat Global Fundamentals Annual (WRDS) | `diplom_findata.csv` | Annual financial statement items for controls |
| Compustat Global Security Daily (WRDS) | `securitydaily.csv` | Daily prices for market-model proxies |
| World Bank Worldwide Governance Indicators | `wgi.xlsx` | `rule_of_law`, `regulatory_quality`, and four other dimensions |

The underlying data are proprietary (WRDS subscription required) and are not
redistributed in this repository.

## Running the notebook

### Setup

```bash
# Clone the repository
git clone https://github.com/<your-username>/diploma-esg-brics.git
cd diploma-esg-brics

# Create a virtual environment (recommended)
python3 -m venv venv
source venv/bin/activate    # on Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Data

The four source files are proprietary (WRDS subscription required for Trucost and
Compustat; World Bank open data for WGI) and are **not redistributed** in this
repository. Researchers with WRDS access can replicate the data extraction using
the queries described in the thesis (Chapter 2, Sections 2.1.2–2.1.4).

Once obtained, place the four source files in a `./data/` directory in the repo root:

```
diploma-esg-brics/
├── data/
│   ├── gxpvubiao4zdn1ro.csv     # Trucost Environmental
│   ├── diplom_findata.csv        # Compustat Fundamentals Annual
│   ├── securitydaily.csv         # Compustat Security Daily (~1.8 GB)
│   └── wgi.xlsx                  # WGI from World Bank
```

The `data/` directory is `.gitignore`d so source data is never accidentally committed.

### Run

Open `Kurakina_diploma_clean.ipynb` in Jupyter and run the cells sequentially.
By default `DATA_DIR` points at `./data/`; edit Section 1 of the notebook if your
data lives elsewhere.

## Notebook structure

| Section | Content |
|---|---|
| 1 | Configuration (`DATA_DIR`, imports) |
| 2 | Trucost Environmental: exploratory analysis |
| 3 | Sample construction (9 BRICS+ countries, 2017–2024) |
| 4 | Compustat & Security Daily: load and parquet conversion |
| 5 | Three-way intersection of firms across data sources |
| 6 | Daily log returns and country market return |
| 7 | Market-model metrics: Volatility, Synchronicity, IdioVol |
| 8 | Panel assembly and `Mandatory` indicator |
| 9 | Worldwide Governance Indicators |
| 10 | Winsorisation and the regression-ready panel |
| 11 | Diagnostics: VIF and baseline pooled OLS |
| 12 | Main fixed-effects regressions |
| 13 | Robustness sub-samples |
| 14 | Event-time DiD (staggered design) |
| 15 | Country-by-country regressions |
| 16 | H3: triple interaction with rule of law |
| 17 | Country event studies (China 2024, India 2022) |
| 18 | Lag specification |
| 19 | Summary table |
| 20 | Pillar regressions and Figures 1–9 |
| 21 | Balanced-panel disclosure heatmap |
| 22 | Lag-specification R² check |

## Key definitions

- **Synchronicity** = $\ln(R^2 / (1 - R^2))$, the log-odds of the market-model $R^2$
  following Morck, Yeung and Yu (2000). Lower synchronicity is interpreted as
  higher informational efficiency.
- **Volatility** = standard deviation of daily log returns within the firm-year.
- **Idiosyncratic Volatility** = standard deviation of the residuals from the
  market-model regression.
- **Mandatory** = 1 for firm-year observations in jurisdictions and years where a
  mandatory environmental disclosure regime is in effect for listed firms.
  Treatment years used: China 2024, India 2022, Indonesia 2020, Egypt 2021, UAE 2024.

## Citation

Kurakina, A. (2026). *The Link Between ESG Regulation and Market Efficiency in
BRICS+ Countries.* Master's thesis, HSE University, Faculty of Economic Sciences.
