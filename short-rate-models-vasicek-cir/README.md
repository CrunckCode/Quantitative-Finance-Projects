# Short-Rate Models: Vasicek and CIR

Maximum likelihood calibration and Monte Carlo simulation of two one-factor short-rate models on the US 3-month Treasury yield (FRED `DGS3MO`, daily, 2000 to 2024, 6,253 observations).

## Models
- **Vasicek:** `dr = a (b - r) dt + sigma dW`. The MLE uses the exact Gaussian transition density, so each row of the likelihood is `LN(NORMDIST(r(t+1), mean, sd, FALSE))` with `mean = b + (r - b) e^{-a dt}` and variance `sigma^2 (1 - e^{-2 a dt}) / (2 a)`.
- **CIR:** `dr = a (b - r) dt + sigma sqrt(r) dW`. The MLE uses the Euler Gaussian approximation, variance `sigma^2 r dt` (with a tiny floor for rates at zero).

## Workbook sheets (`Short_Rate_Models_Vasicek_CIR.xlsx`)
- **Summary:** parameters, total log-likelihood, Feller condition, and the Vasicek closed-form 1-year zero-coupon bond price.
- **Data:** the yield series. **Vasicek_MLE, CIR_MLE:** one log-density per day, summed to the likelihood.
- **Shocks:** fixed standard-normal draws (seed 42). **Vasicek_Sim, CIR_Sim:** 20 paths of 252 daily steps from the last observed rate (4.37%).

## Results
| | a | b | sigma | Log-likelihood |
|---|---|---|---|---|
| Vasicek | 0.103 | 1.42% | 0.73% | 39,139 |
| CIR | 1.156 | 1.81% | 12.4% | 37,590 |

The Vasicek 1-year zero-coupon price is 0.9587 (yield 4.22%). Over 252 days the 20 simulated paths end at a mean of 4.10% (Vasicek) and 2.52% (CIR). CIR passes the Feller condition. The two log-likelihoods use different densities (exact versus Euler), so they should not be compared as a model-selection test.

## How the parameters were found
Parameters in `Summary!B7:C9` were obtained by maximizing the likelihood in Python (scipy L-BFGS-B with bounds), then entered as values. In Excel you can re-optimize with Solver: maximize `G2` on each MLE sheet by changing the three parameter cells.

## Limitations
- A single year of data gives an unidentified mean-reversion speed (the 2024 series simply trends), so the full 2000 to 2024 history is used. Even so, `a` is estimated with wide uncertainty and the zero-lower-bound years distort both fits.
- One-factor models fit only the short rate and do not match the observed term structure.
- The simulation starts at the last observed rate with only 20 paths, so the path statistics are illustrative.
