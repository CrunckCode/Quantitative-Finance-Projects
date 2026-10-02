# Monte Carlo Derivative Pricing

One SPY option priced three ways and cross-checked, plus path-dependent payoffs and a correlated two-asset simulation. Valuation date 2024-12-31, spot 574.79 (SPY close), strike 575, maturity 0.25 years, risk-free 4.37% (3M T-bill), dividend yield 1.2% (an assumption), volatility 12.59% (annualized realized 2024 volatility). The contract is illustrative, not a quoted option.

## Workbook sheets (`Monte_Carlo_Derivative_Pricing.xlsx`)
- **Inputs, Data:** parameters and 2024 SPY and JPM daily closes with log returns.
- **BlackScholes:** Black-Scholes-Merton with a continuous dividend yield: d1, d2, call, put, put-call parity check, delta, gamma and vega.
- **Binomial:** 20-step Cox-Ross-Rubinstein tree laid out cell by cell: stock tree, European call values and American put values (with early exercise).
- **MC_Shocks, MC_Paths, MC_Results:** 500 risk-neutral GBM paths of 63 daily steps using 250 draws and their antithetic mirror. Prices: European call and put, arithmetic-average Asian call, and an up-and-out barrier call (barrier 603.75, knocked out if any step reaches it). Standard errors are reported.
- **Correlated_GBM:** SPY and JPM paths driven by a 2 by 2 Cholesky factor of their realized 2024 correlation (0.43).

## Results
| Instrument | Black-Scholes | Binomial (20 steps) | Monte Carlo (500 paths) |
|---|---|---|---|
| European call | 16.60 | 16.44 | 15.72 (SE 1.01) |
| European put | 12.28 | n/a | 11.49 (SE 0.82) |
| American put | n/a | 12.60 | n/a |
| Asian call | n/a | n/a | 9.45 (SE 0.59) |
| Up-and-out call | n/a | n/a | 1.81 (SE 0.23) |

Put-call parity holds to machine precision. The American put is worth 0.31 more than the European put (early-exercise premium). Delta 0.559, gamma 0.0109, vega 113.0 per 1.00 of volatility. The Monte Carlo prices are within about one standard error of Black-Scholes.

## Checks
Black-Scholes values were reproduced independently in Python (scipy) and match to 4 decimals.

## Limitations
- 500 paths is small; the Monte Carlo standard error is about 6% of the call price. The reported standard error treats antithetic paths as independent, so it is conservative.
- The tree has only 20 steps, so it differs from Black-Scholes in the second decimal place.
- Constant volatility and dividend yield; no smile.
- No closed-form benchmark is included for the Asian and barrier options.
