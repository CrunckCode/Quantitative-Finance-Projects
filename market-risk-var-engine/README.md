# Market Risk VaR Engine

One-day Value-at-Risk for a 1,000,000 portfolio of JPMorgan (40%), Morgan Stanley (30%) and Bank of America (30%), computed three ways and backtested. Data: Yahoo Finance adjusted closes, 2023 to 2024 (`prices_JPM_MS_BAC_2023_2024.csv`, 501 prices, 500 log returns).

## Workbook sheets (`Market_Risk_VaR_Engine.xlsx`)
- **Summary:** VaR by method and confidence level, plus backtest results.
- **Inputs:** notional, weights, window length.
- **Prices, Returns:** data and log returns (formulas), portfolio return and P&L.
- **Parametric:** 3 by 3 covariance matrix of the latest 250 returns, portfolio variance `w' Sigma w`, VaR as `z x sigma x notional` (zero mean).
- **Historical:** VaR as the empirical percentile of the latest 250 daily P&L values.
- **MC_Shocks, MonteCarlo:** 1,000 fixed standard-normal draws turned into correlated asset returns with a Cholesky factor built by formula from the covariance matrix, then percentile VaR.
- **Backtest:** rolling 250-day backtest at 99% over 250 out-of-sample days. Each day's VaR uses only the 250 returns before it. Kupiec proportion-of-failures LR test against the 5% chi-square critical value (3.841) and the Basel traffic light zone (Green 0 to 4, Yellow 5 to 9, Red 10 or more).

## Results
| Confidence | Variance-covariance | Historical | Monte Carlo |
|---|---|---|---|
| 99% | 31,216 | 31,363 | 29,875 |
| 95% | 22,071 | 19,926 | 19,595 |
| 90% | 17,196 | 11,829 | 15,356 |

Backtest at 99% over 250 days: 4 exceptions for both historical and parametric VaR against 2.5 expected. Kupiec LR = 0.77, so the models are not rejected; Basel zone Green.

## Checks
VaR figures and the exception counts were reproduced independently in Python (numpy percentile, scipy normal quantile, numpy Cholesky) and match.

## Limitations
- Three equities and two years of data, with constant weights.
- Parametric and Monte Carlo VaR assume normal returns and zero mean, so fat tails are understated (visible at 90%, where the historical figure is much lower).
- The Monte Carlo VaR is a function of one fixed set of draws.
- No Expected Shortfall or Christoffersen independence test yet.
