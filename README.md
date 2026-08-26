<p align="center">
  <img src="https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/pandas-Data%20Analysis-150458?logo=pandas&logoColor=white" alt="pandas"/>
  <img src="https://img.shields.io/badge/NumPy-Scientific%20Computing-013243?logo=numpy&logoColor=white" alt="NumPy"/>
  <img src="https://img.shields.io/badge/scikit--learn-Clustering-F7931E?logo=scikit-learn&logoColor=white" alt="scikit-learn"/>
  <img src="https://img.shields.io/badge/statsmodels-Factor%20Models-4D8C57" alt="statsmodels"/>
  <img src="https://img.shields.io/badge/PyPortfolioOpt-Optimization-491F59" alt="PyPortfolioOpt"/>
  <img src="https://img.shields.io/badge/yfinance-Market%20Data-2E7D32" alt="yfinance"/>
</p>

# Unsupervised Trading Strategy on the S&P 500

An end-to-end systematic equity strategy: cluster the S&P 500 each month with K-means, follow the highest-momentum cluster into the next month, size the book with a max-Sharpe optimizer net of trading costs, and measure it against both the cap-weighted (SPY) and equal-weighted (RSP) index.

The emphasis is on running the backtest realistically — point-in-time features, realistic costs, and a fair benchmark — rather than chasing a headline number. The universe is the *current* S&P 500 membership, so the results carry survivorship bias (see [Caveats](#caveats)); read them as a research exercise, not a live track record.

---

## Results (mid-2018 to 2025, net of 10 bps turnover cost)

![Strategy vs. SPY and RSP — cumulative growth of $1](output.png)

| Metric | **Strategy** | SPY (cap-weight) | RSP (equal-weight) |
|---|---:|---:|---:|
| CAGR | **21.8%** | 13.6% | 9.0% |
| Sharpe | **0.80** | 0.72 | 0.51 |
| Sortino | 1.03 | 0.88 | 0.63 |
| Volatility | 30.8% | 20.6% | 21.1% |
| Max drawdown | −40.1% | −33.7% | −39.0% |
| CAPM alpha vs. SPY (single-factor) | **+8.1%** | — | — |
| Beta vs. SPY | 1.10 | — | — |

On a risk-adjusted basis the strategy beats both benchmarks — clearly against equal-weight RSP (0.80 vs 0.51), narrowly against cap-weight SPY (0.80 vs 0.72) — but it does so as a higher-risk, more concentrated book: roughly 1.5× the index volatility and a deeper drawdown. 70 of 90 monthly rebalances solved the max-Sharpe optimization; the rest fell back to equal weight.

The evaluation starts in mid-2018 because when the trailing features are fully warmed up.

---

## How it works

1. **Universe & prices.** Current S&P 500 membership from Wikipedia, then ten years of daily bars from Yahoo Finance.
2. **Features.** Per-stock technical signals — Garman–Klass volatility, RSI, Bollinger bands, a rolling-window standardized ATR and MACD — plus dollar volume for the liquidity screen. All features are point-in-time (trailing/lagged only).
3. **Monthly panel.** Daily data collapsed to month-end (last value for signals, mean for dollar volume).
4. **Liquidity screen.** Keep the 150 most-traded names each month, ranked on a 12-month average of dollar volume.
5. **Momentum.** Multi-horizon trailing returns plus the classic **12-minus-1** momentum used for selection; skipping the most recent month avoids short-term reversal.
6. **Factor exposure.** Rolling betas to the Fama–French five factors, lagged one month.
7. **Clustering.** Each month's cross-section is standardized and split into four groups with K-means; the cluster with the strongest 12-1 momentum is held next month.
8. **Portfolio.** Max-Sharpe optimization on the trailing year, capped at 10% per name, with an equal-weight fallback when infeasible. A 10 bps cost is charged on turnover each rebalance.
9. **Evaluation.** Backtested against SPY and RSP, with a full metrics table and a cumulative growth chart. A final cell runs a future-masking test that confirms no feature uses forward-looking data.

---

## Caveats

- **Survivorship bias** — the universe is today's S&P 500, so only index survivors are tradable; this flatters returns, momentum most of all. Removing it needs point-in-time constituent data.
- **Optimistic costs** — a flat 10 bps on turnover ignores bid–ask, slippage, and market impact.

---

## Running it

```bash
pip install yfinance pandas-datareader statsmodels scikit-learn PyPortfolioOpt matplotlib lxml requests pandas numpy
```

Open `Unsupervised_Trading_Strategy.ipynb` and run **Kernel → Restart & Run All**. Every download (constituents, prices, Fama–French factors, SPY, RSP) is cached to `data_cache/` on the first run, so later runs read from disk and work offline. Delete `data_cache/` for a clean refetch.

---

## Tech stack

Python · pandas · NumPy · scikit-learn · statsmodels · PyPortfolioOpt · yfinance · pandas-datareader · Matplotlib

---

## 👨‍💻 Author
**Saeed Mohseni seh deh**
Graduate Researcher, Institute for Advanced Computing
Virginia Tech, VA, USA

🌐 [Website](https://saeedmohseni.netlify.app/) | 📫 [saeedmohseni@vt.edu](mailto:saeedmohseni@vt.edu)
