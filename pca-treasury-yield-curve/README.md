# PCA on the Treasury Yield Curve

Principal component analysis of daily changes in US Treasury par yields at eight tenors (1Y, 2Y, 3Y, 5Y, 7Y, 10Y, 20Y, 30Y), FRED, 2 January 2015 to 31 December 2024 (2,501 days, `treasury_par_yields_2015_2024.csv`). The notebook `PCA_Treasury_Yield_Curve.ipynb` is saved with outputs and runs offline from the CSV.

## Method
1. Daily yield changes in basis points (levels are non-stationary).
2. `sklearn` PCA with 3 components on the covariance of changes (centered, not rescaled).
3. Loadings table and plot, reconstruction of the changes from 3 components with a residual check, and a split-sample stability check.

## Results
| Component | Variance explained | Pattern |
|---|---|---|
| PC1 | 84.9% | Level: positive loadings at every tenor (largest at 5Y and 7Y) |
| PC2 | 11.0% | Slope: negative at 1Y to 5Y, positive at 7Y to 30Y |
| PC3 | 2.2% | Curvature: large positive at 1Y and 30Y, negative in the belly |

Three components explain 98.2% of daily curve variance. Reconstruction RMSE is under 1.1 bp at every tenor (about 1.8% of variance left over). Split sample: PC1 87.7% and PC2 7.9% in 2015 to 2019, versus PC1 84.5% and PC2 11.7% in 2020 to 2024, so the slope factor mattered more during the recent hiking cycle.

## Limitations
- Par yields rather than zero rates, and only eight tenors, so the curvature factor is coarse.
- The PCA is unscaled, so high-volatility tenors dominate early components.
- Components describe historical covariance only, and the signs of eigenvectors are arbitrary.
