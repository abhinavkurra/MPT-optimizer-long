# 📈 Long-Only Modern Portfolio Optimisation (MPO) — Indian Equities

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/SciPy-SLSQP%20Optimiser-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Plotly-Interactive%20Charts-3F4F75?style=for-the-badge&logo=plotly&logoColor=white"/>
  <img src="https://img.shields.io/badge/NSE-Indian%20Equities-FF6B35?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge"/>
</p>

<p align="center">
  A production-ready quantitative finance toolkit that applies <strong>Harry Markowitz's Modern Portfolio Theory</strong> to a curated basket of NSE-listed blue-chip Indian equities. The engine maximises the <strong>Sharpe Ratio</strong> under long-only constraints using <strong>Sequential Least-Squares Quadratic Programming (SLSQP)</strong>, and delivers a full suite of interactive visualisations.
</p>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Mathematical Foundation](#-mathematical-foundation)
- [Asset Universe](#-asset-universe)
- [Project Structure](#-project-structure)
- [Requirements & Installation](#-requirements--installation)
- [Usage](#-usage)
- [Code Architecture](#-code-architecture)
- [Outputs & Visualisations](#-outputs--visualisations)
- [Configuration](#-configuration)
- [Results Interpretation](#-results-interpretation)
- [Limitations & Disclaimer](#-limitations--disclaimer)
- [License](#-license)

---

## 🔭 Project Overview

Modern Portfolio Theory (MPT), introduced by Harry Markowitz in 1952, provides a mathematical framework for assembling a portfolio of assets such that the **expected return is maximised for a given level of risk** (or equivalently, risk is minimised for a given expected return). This project implements the **Maximum Sharpe Ratio** variant — the tangency portfolio — which yields the single best risk-adjusted allocation from the efficient frontier.

### Key Objectives

| Goal | Approach |
|---|---|
| Maximise risk-adjusted return | Optimise the Sharpe Ratio via SLSQP |
| Respect investment constraints | Long-only bounds `[0, 1]` + full-investment equality `Σwᵢ = 1` |
| Use real market data | `yfinance` daily adjusted closing prices (2019–2025) |
| Produce actionable insights | Weights, performance metrics, and 4 visual outputs |

---

## 📐 Mathematical Foundation

### 1. Returns & Covariance

Daily simple returns are computed as:

$$r_{i,t} = \frac{P_{i,t}}{P_{i,t-1}} - 1$$

These are then annualised over $T = 252$ trading days:

$$\mu_i = \bar{r}_i \times T \qquad \Sigma = \text{Cov}(R_{\text{daily}}) \times T$$

### 2. Portfolio Statistics

For a weight vector $\mathbf{w} \in \mathbb{R}^n$:

| Metric | Formula |
|---|---|
| **Expected Return** | $E[R_p] = \mathbf{w}^\top \boldsymbol{\mu}$ |
| **Portfolio Variance** | $\sigma_p^2 = \mathbf{w}^\top \Sigma \mathbf{w}$ |
| **Portfolio Volatility** | $\sigma_p = \sqrt{\mathbf{w}^\top \Sigma \mathbf{w}}$ |
| **Sharpe Ratio** | $SR = \dfrac{E[R_p] - R_f}{\sigma_p}$ |

### 3. Optimisation Problem

$$\max_{\mathbf{w}} \quad SR(\mathbf{w}) = \frac{\mathbf{w}^\top \boldsymbol{\mu} - R_f}{\sqrt{\mathbf{w}^\top \Sigma \mathbf{w}}}$$

$$\text{subject to} \quad \sum_{i=1}^{n} w_i = 1 \quad \text{(Full Investment)}$$

$$\quad 0 \leq w_i \leq 1 \quad \forall i \quad \text{(Long-Only)}$$

The problem is passed to `scipy.optimize.minimize` as a **minimisation of the negative Sharpe Ratio** using the **SLSQP** method, which natively handles both equality constraints and box bounds.

---

## 🏦 Asset Universe

Seven large-cap NSE-listed equities spanning multiple high-growth sectors of the Indian economy:

| Ticker | Company | Sector |
|---|---|---|
| `RELIANCE.NS` | Reliance Industries Ltd. | Energy / Conglomerate |
| `HDFCBANK.NS` | HDFC Bank Ltd. | Private Banking |
| `BHARTIARTL.NS` | Bharti Airtel Ltd. | Telecommunications |
| `SBIN.NS` | State Bank of India | Public Sector Banking |
| `ICICIBANK.NS` | ICICI Bank Ltd. | Private Banking |
| `TCS.NS` | Tata Consultancy Services Ltd. | Information Technology |
| `LT.NS` | Larsen & Toubro Ltd. | Engineering & Infrastructure |

> **Backtest Period:** 1 January 2019 → 1 January 2025 (≈ 1,500 trading days)
> **Risk-Free Rate:** 6.0% per annum (aligned with Indian Government T-bill yields)

---

## 📁 Project Structure

```
mpo-indian-equities/
│
├── main.py                      # Primary script (linear pipeline)
│
├── outputs/                     # Auto-generated on execution
│   ├── correlation_heatmap.png  # Seaborn EDA heatmap (static)
│   ├── efficient_frontier.html  # Plotly interactive frontier
│   ├── optimal_allocation.html  # Plotly interactive donut chart
│   └── cumulative_performance.html  # Plotly interactive growth chart
│
├── requirements.txt             # Python dependency list
├── README.md                    # This file
└── LICENSE                      # MIT License
```

---

## ⚙️ Requirements & Installation

### Prerequisites

- Python **3.9** or higher
- `pip` package manager

### Step 1 — Clone the Repository

```bash
git clone https://github.com/your-username/mpo-indian-equities.git
cd mpo-indian-equities
```

### Step 2 — Create a Virtual Environment (Recommended)

```bash
# Create
python -m venv venv

# Activate (macOS / Linux)
source venv/bin/activate

# Activate (Windows)
venv\Scripts\activate
```

### Step 3 — Install Dependencies

```bash
pip install -r requirements.txt
```

### `requirements.txt`

```
numpy>=1.24.0
pandas>=2.0.0
yfinance>=0.2.36
matplotlib>=3.7.0
seaborn>=0.12.0
plotly>=5.18.0
scipy>=1.11.0
```

---

## 🚀 Usage

### Run as a Python Script

```bash
python main.py
```

### Run in a Jupyter Notebook

```bash
pip install jupyterlab
jupyter lab
# Then open main.py or paste code into a notebook cell
```

### Expected Console Output

```
=================================================================
  Long-Only Modern Portfolio Optimisation — Indian Equities
=================================================================

[1/5] Downloading price data (2019-01-01 → 2025-01-01) …
    ✓ 7 tickers | 1,487 trading days loaded
    Date range: 2019-01-02 → 2024-12-31

[2/5] Running SLSQP optimisation …
    ✓ Optimisation converged successfully

[3/5] Simulating 5,000 random portfolios for frontier plot …
    ✓ Simulation complete

=================================================================
  OPTIMAL PORTFOLIO — Maximum Sharpe Ratio
=================================================================
  Expected Annual Return :    22.47%
  Annual Volatility      :    18.93%
  Maximum Sharpe Ratio   :    0.8696
-----------------------------------------------------------------
  Asset Allocation (weights ≥ 0.1%):
    BHARTIARTL.NS       38.21%  ███████████████
    TCS.NS              27.54%  ███████████
    ICICIBANK.NS        19.83%  ████████
    RELIANCE.NS         14.42%  █████
=================================================================

[4/5] Generating visualisations …
    ✓ Correlation heatmap saved → correlation_heatmap.png
    ✓ Efficient frontier chart saved → efficient_frontier.html
    ✓ Allocation donut chart saved → optimal_allocation.html

[5/5] Building cumulative performance chart …
    ✓ Performance chart saved → cumulative_performance.html
```

> ⚠️ Actual weights and metrics will vary with live data pulled from Yahoo Finance.

---

## 🏗️ Code Architecture

The script follows a strict **linear pipeline** with an **OOP core**:

```
Imports → Constants → Data Fetching → LongOnlyOptimizer → Execution → Visualisations
```

### `LongOnlyOptimizer` Class

The heart of the project. Encapsulates all quantitative logic:

```
LongOnlyOptimizer
├── __init__(prices, risk_free_rate, trading_days)
│     ├── Computes daily_returns via pct_change()
│     ├── Annualises mu  (μ = mean × T)
│     ├── Annualises cov (Σ = Cov × T)
│     └── Computes corr matrix for EDA
│
├── portfolio_return(w)       →  wᵀμ
├── portfolio_volatility(w)   →  √(wᵀΣw)
├── sharpe_ratio(w)           →  (wᵀμ − Rf) / σ_p
├── _neg_sharpe(w)            →  −SR(w)  [objective function]
│
├── optimise()
│     ├── Initialise w₀ = [1/n, …, 1/n]
│     ├── Define bounds: (0.0, 1.0) per asset
│     ├── Define constraint: Σwᵢ = 1
│     ├── Call scipy.optimize.minimize (SLSQP, ftol=1e-12)
│     ├── Zero weights < 0.1% threshold
│     └── Re-normalise and return summary dict
│
└── simulate_random_portfolios(n)
      └── Dirichlet-sampled weights → (Return, Volatility, Sharpe)
```

### Data Flow

```
yfinance raw OHLCV
      │
      ▼
  Adj Close Prices  ──► ffill() ──► dropna()
      │
      ▼
  Daily Returns (pct_change)
      │
      ├──► Annualised μ  (expected returns vector)
      ├──► Annualised Σ  (covariance matrix)
      └──► Correlation matrix
                │
                ▼
          SLSQP Optimiser
                │
                ▼
        Optimal Weights w*
                │
      ┌─────────┼──────────┐
      ▼         ▼          ▼
  Donut Chart  Frontier  Cum. Returns
```

---

## 📊 Outputs & Visualisations

### 1. Correlation Heatmap (`correlation_heatmap.png`)

A lower-triangle Seaborn heatmap displaying pairwise Pearson correlations of daily returns. Used during **Exploratory Data Analysis (EDA)** to identify diversification opportunities — lower inter-asset correlations imply greater risk reduction through portfolio construction.

### 2. Efficient Frontier (`efficient_frontier.html`)

An interactive Plotly scatter plot of 5,000 Monte-Carlo simulated portfolios, each generated by Dirichlet-sampled random weights. Portfolios are coloured by Sharpe Ratio on a Viridis scale. The **Maximum Sharpe Portfolio** is highlighted with a red star (⭐), visually demonstrating its position on the frontier boundary.

### 3. Optimal Allocation Donut Chart (`optimal_allocation.html`)

An interactive Plotly donut chart rendering the final post-optimisation weight distribution. The central annotation displays the achieved Sharpe Ratio for quick reference. Hover tooltips show exact percentages per asset.

### 4. Cumulative Performance Chart (`cumulative_performance.html`)

Tracks the growth of a hypothetical **$1 investment** made on 2019-01-01, rebalanced daily to the optimal static weights:

$$V_t = \prod_{\tau=1}^{t} \left(1 + \sum_{i} w_i^* \cdot r_{i,\tau}\right)$$

Individual constituent lines are rendered at low opacity for context, with the optimal portfolio highlighted in red. A range slider enables time-range selection.

---

## 🔧 Configuration

All key parameters are defined in the `Constants` section at the top of `main.py` and can be modified without touching any other code:

```python
# --- 2. CONSTANTS -------------------------------------------------------------

TICKERS = [
    'RELIANCE.NS', 'HDFCBANK.NS', 'BHARTIARTL.NS',
    'SBIN.NS', 'ICICIBANK.NS', 'TCS.NS', 'LT.NS'
]

START_DATE           = '2019-01-01'   # Backtest start date
END_DATE             = '2025-01-01'   # Backtest end date
RISK_FREE_RATE       = 0.06           # 6% p.a. (Indian T-bill proxy)
TRADING_DAYS         = 252            # Annualisation factor
WEIGHT_THRESHOLD     = 0.001          # Drop weights below 0.1%
N_RANDOM_PORTFOLIOS  = 5_000          # Monte-Carlo frontier portfolios
RANDOM_SEED          = 42             # Reproducibility
```

### Customisation Examples

**Swap in US equities:**
```python
TICKERS = ['AAPL', 'MSFT', 'GOOGL', 'AMZN', 'NVDA']
RISK_FREE_RATE = 0.05   # US Fed Funds Rate proxy
```

**Extend the backtest window:**
```python
START_DATE = '2015-01-01'
END_DATE   = '2025-01-01'
```

**Increase frontier resolution:**
```python
N_RANDOM_PORTFOLIOS = 20_000
```

---

## 📖 Results Interpretation

| Metric | Interpretation |
|---|---|
| **Expected Annual Return** | Arithmetic mean of portfolio returns, projected over one year assuming historical return patterns persist. |
| **Annual Volatility** | Annualised standard deviation of portfolio returns — measures total risk. Lower is better for the same return. |
| **Sharpe Ratio** | Excess return per unit of risk above the risk-free rate. A value `> 1.0` is generally considered strong; `> 2.0` is exceptional. |
| **Asset Weights** | The exact capital allocation proportions. A zero weight means the optimiser found that asset detrimental to the Sharpe Ratio given its correlation structure. |

### Reading the Efficient Frontier

- Each **dot** represents a valid long-only portfolio.
- The **left edge** of the cloud traces the efficient frontier (minimum variance for each return level).
- The **red star** marks the tangency portfolio — the point where a line from the risk-free rate is tangent to the frontier, maximising the Sharpe Ratio.
- Portfolios **below and right** of the star are sub-optimal (same risk, lower return).

---

## ⚠️ Limitations & Disclaimer

> **This project is for educational and research purposes only. It does not constitute financial advice. Past performance is not indicative of future results.**

### Known Limitations

| Limitation | Description |
|---|---|
| **Estimation Error** | Sample mean returns are notoriously noisy estimators. Small changes in the estimation window can significantly shift optimal weights. |
| **Static Weights** | The optimisation assumes fixed weights over the entire period. No rebalancing frequency is modelled. |
| **No Transaction Costs** | Bid-ask spreads, brokerage fees, STT, and market impact are not accounted for. |
| **Survivorship Bias** | The selected tickers are all currently listed; companies that were delisted over the period are excluded. |
| **Normal Returns Assumption** | The mean-variance framework implicitly assumes returns are normally distributed, ignoring fat tails and skewness. |
| **Single-Period Model** | MPT is a single-period model; it does not account for changing correlations or regime shifts (e.g., COVID-19 crash in March 2020). |

### Potential Enhancements

- **Shrinkage Estimators** — Ledoit-Wolf covariance shrinkage to reduce estimation error
- **Black-Litterman Model** — Incorporate investor views into the expected returns prior
- **Rolling Optimisation** — Walk-forward backtesting with periodic rebalancing
- **Risk Parity** — Alternative objective: equalise risk contribution across assets
- **CVaR Optimisation** — Replace variance with Conditional Value-at-Risk for tail-risk control

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for full details.

```
MIT License  Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

---

## 🙏 Acknowledgements

- **Harry Markowitz** — Nobel Laureate, father of Modern Portfolio Theory (1952)
- **[yfinance](https://github.com/ranaroussi/yfinance)** — Market data retrieval
- **[SciPy](https://scipy.org/)** — SLSQP optimisation engine
- **[Plotly](https://plotly.com/)** — Interactive visualisation framework
- **[Seaborn](https://seaborn.pydata.org/)** — Statistical data visualisation

---

<p align="center">
  Built with 🐍 Python &nbsp;|&nbsp; Quantitative Finance &nbsp;|&nbsp; NSE Indian Equities
</p>