# FE535 Project 1: Performance Evaluation, Mean-Variance Frontier and Risk Modeling

Team project for **FE 535 Introduction to Financial Risk Management** (Stevens Institute of Technology, Fall 2026).
Due **Sunday, Nov 8, 2026, 11:59 PM**.

The project has three computational problems, based on 19 Fidelity Select sector mutual funds and the SPY and XLK ETFs. The handout also lists a fourth section, "Dynamic Games via Monte Carlo", which currently reads "To be completed…".

## Status

| Problem | Topic | Points | Status |
|---|---|---|---|
| 1 | Performance summary (absolute and relative, SPY and XLK benchmarks) | 65 | Done |
| 2 | Mean-variance efficient frontier (in-sample, out-of-sample, shrinkage) | 75 | Planned: Sun Nov 1 |
| 3 | Risk modeling (GBM calibration, Monte Carlo, VaR) | 75 | Planned: Sun Nov 1 and Sun Nov 8 |
| 4 | Dynamic games via Monte Carlo | – | Not released in the handout |

The detailed schedule, open decisions and per-part plan are in the report (section "Work Plan for Remaining Problems").

## Repository contents

| File | Description |
|---|---|
| `project1.ipynb` | Source code: data download and all computations |
| `FE535_Project1_Report.docx` | Working report (Word). The final submission is the PDF export |
| `FE535_Project_01.pdf` | Project handout from the instructor |
| `fig_1_4.png` | Figure for Problem 1.4, written by the notebook at 300 dpi |

## Setup

Python 3.10 or newer is recommended.

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install yfinance pandas numpy matplotlib jupyter
jupyter notebook project1.ipynb
```

Run the notebook from top to bottom (**Kernel → Restart & Run All**). It downloads prices from Yahoo Finance, so an internet connection is required.

## Data

* **Source:** Yahoo Finance via `yfinance`, using the `Adj Close` column (`auto_adjust=False`).
* **Tickers:** the 19 funds `FSPTX, FSELX, FSCSX, FSPHX, FBIOX, FSMEX, FPHAX, FIDSX, FSRBX, FSPCX, FSENX, FSDAX, FSRFX, FSCPX, FSAVX, FSRPX, FDFAX, FSTCX, FWRLX`, plus `SPY` (market) and `XLK` (alternative benchmark).
* **Sample:** Jan 2015 to Sep 2026, giving 2,952 daily log returns per ticker.
* **Returns:** r_t = ln(P_t / P_{t−1}). The risk-free rate is zero, as the handout requires.
* Problem 3 also needs monthly SPY prices from Jan 1994.
* Yahoo data can be revised, so numbers may differ slightly depending on the download date.

## Methodology (Problem 1)

All measures are annualized with 252 trading days. For fund *i* and benchmark *M*:

| Measure | Definition |
|---|---|
| Mean return | 252 · mean(r_i) |
| Volatility | √252 · sd(r_i) |
| Sharpe ratio | mean / volatility |
| Beta | Cov(r_i, r_M) / Var(r_M) |
| Jensen's alpha | μ_i − β · μ_M |
| Treynor ratio | μ_i / β |
| Tracking error | √252 · sd(r_i − r_M) |
| Information ratio | (μ_i − μ_M) / tracking error |

Headline results (19 funds, benchmark SPY):

* Only FSELX, FSPTX and FSCSX beat SPY's mean return of 12.9%, and each does so with higher volatility.
* The highest alphas are FSELX (6.4%) and FSPTX (4.7%). Both are technology-related and have beta above 1.
* Expense ratios are negatively correlated with mean return (−0.55) and with alpha (−0.54).
* Against XLK, the alpha of the technology funds mostly disappears, so alpha depends heavily on the benchmark.

## Planned work

**Problem 2.** We use FSPTX, FIDSX and FSCPX on monthly data, split in two halves. The efficient set is built with closed-form matrix algebra only, with no optimization package: w(A) = w₀ + (1/A)·w₁. We compare the in-sample frontier with the out-of-sample frontier and with a frontier that shrinks the covariance matrix to a diagonal.

**Problem 3.** We calibrate a geometric Brownian motion to 392 monthly SPY log returns and simulate 1000 paths. We then compute the 99% VaR of a 100-share position, run a σ-sweep from 0.10 to 0.50, and compare historical with parametric monthly VaR.

## Known issues and to-do

* The Part 1 and Part 3(a) summary tables in the notebook are computed over all 21 columns of `log_returns`, which include SPY and XLK. The report tables use the 19 funds only. Restrict `mean_returns`, `volatilities`, `alphas`, `betas` and the related series to `funds` before taking the summaries. A sign of the problem is a minimum tracking error of 0.0000, which is SPY compared with itself.
* The Part 2 markdown comment in the notebook names `FSLX`. The ticker is `FSELX`.
* The report refers to the source file as `Project 1.py`. It should name `project1.ipynb`.
* The report currently uses 1.15 line spacing. The handout requires 11 pt font, 1.5 line spacing, 1 in margins and at most 15 pages.
* Submission: PDF of the report plus the source file.

## Team

Christopher Torello, Jinhong Chen, Riccardo Baccelliere
