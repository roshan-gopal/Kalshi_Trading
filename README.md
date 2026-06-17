# Cross-Market Momentum on Kalshi NBA Playoffs

**Roshan Gopal · 2026**

Quantitative research and live execution project testing whether **time-series momentum** in NBA *game* markets predicts moves in correlated *series* markets on [Kalshi](https://kalshi.com)—and whether that edge survives fees, out-of-sample validation, and real trading.

## Research Question

> Does momentum in individual playoff game prices (Team A vs. Team B) lead series prices (Will Team A/B win the series)?

## Approach

| Stage | What I did |
|---|---|
| **Data** | Pulled 1-minute candlesticks from Kalshi API v2 for 2026 ECF, WCF, and NBA Finals game + series markets |
| **Signal** | Rolling mean of game returns → directional position in the corresponding series market when \|signal\| exceeds threshold |
| **Optimization** | Grid search over lookback windows (1–10 min) and signal thresholds; Sharpe surface analysis per team |
| **Validation** | Held out WCF Game 6 and NBA Finals Games 1–4; reported hit rate, drawdown, and t-stats |
| **Execution** | Dollar-neutral weighting, limit-order logic to reduce fees, cash-aware sizing, and live rebalancing |

**Selected parameters:** 3-minute lookback, 0.01 threshold (chosen for fill reliability under limit orders, not just in-sample Sharpe).

## Results

| Metric | In-sample | Out-of-sample | Live (4 games) |
|---|---:|---:|---:|
| Sharpe | 2.25 | 2.0 | — |
| Avg return / game | — | ~30% | ~33% |
| Total P&L | — | — | **+$716** |

Live deployment spanned WCF Game 6 and NBA Finals Games 1–3 (sized at $20–$300 per game).

## Why This Matters

- End-to-end quant workflow: **hypothesis → data pipeline → parameter search → OOS test → production constraints → live trading**
- Cross-market arbitrage-style thinking applied to **prediction markets**, not just equities
- Explicit treatment of **transaction costs and fill risk**—a common gap in student projects

## Stack

Python · pandas · NumPy · SciPy · Matplotlib · Kalshi Trade API v2

## Notebook

Full write-up with signal math, Sharpe heatmaps, fee analysis, and live trader logic:

[`notebooks/KalshiMomentumTS_NBA_Project.ipynb`](notebooks/KalshiMomentumTS_NBA_Project.ipynb)

## Limitations

Kalshi historical data is 1-minute granularity; limit orders do not always fill; series markets can close abruptly when a team clinches. Results are promising but sample size is small—extension to other correlated event markets (e.g. World Cup match vs. winner futures) is discussed in the notebook.
