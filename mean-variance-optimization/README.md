# Mean-Variance Optimization

Markowitz optimization on JPMorgan, Goldman Sachs and Morgan Stanley using 2024 daily adjusted closes (Yahoo Finance, 251 returns). Risk-free rate is an assumed 4.2%.

## Workbook sheets (`Mean_Variance_Optimization.xlsx`)
- **Stats:** annualized mean, volatility and Sharpe per stock, the annualized covariance and correlation matrices, the covariance inverse (array formula `MINVERSE`), and closed-form portfolios:
  - global minimum variance: `w = Sigma^-1 1 / (1' Sigma^-1 1)`
  - tangency (maximum Sharpe): `w = Sigma^-1 (mu - rf) / sum`
  - portfolio return `w' mu` and volatility `sqrt(w' Sigma w)` for each.
- **Prices, Returns:** data and log returns by formula.
- **Frontier:** 5,000 random long-only portfolios (weights from stored uniform draws, normalized) with return, volatility and Sharpe, a scatter chart, and the best random portfolios.

## Results
| | JPM | GS | MS |
|---|---|---|---|
| Annualized return | 35.7% | 41.4% | 32.9% |
| Annualized volatility | 23.3% | 25.5% | 26.0% |
| Sharpe | 1.35 | 1.46 | 1.10 |

| Portfolio | JPM | GS | MS | Return | Vol | Sharpe |
|---|---|---|---|---|---|---|
| Global minimum variance | 62.0% | 5.5% | 32.5% | 35.1% | 22.1% | 1.39 |
| Tangency | 38.0% | 73.1% | -11.1% | 40.2% | 23.9% | 1.51 |
| Equal weight | 33.3% | 33.3% | 33.3% | 36.7% | 22.6% | 1.43 |

The best of the 5,000 random long-only portfolios has Sharpe 1.50 and the lowest random volatility is 22.1%, both close to the closed-form answers, which confirms the formulas. The tangency portfolio shorts Morgan Stanley, so it is not long-only; the random frontier is long-only.

## Limitations
- One year of strongly rising bank stocks: the inputs are in-sample, the returns are unusually high, and the optimizer simply chases the best 2024 performer. Estimated means are very noisy.
- Only three highly correlated assets, no constraints, transaction costs or shrinkage of the covariance matrix.
- The unconstrained closed forms allow shorting. A constrained version would need a numerical optimizer.
