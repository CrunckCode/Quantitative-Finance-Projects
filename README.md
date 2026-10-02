# Quantitative Finance Projects

Six small quantitative finance models, five as Excel workbooks with live formulas and one as a Python notebook. They cover the topics from my Quant Finance and Linear Algebra bootcamps: VaR, short-rate models, derivative pricing, swap valuation, portfolio optimization and yield-curve PCA.

All of them use real market data (Yahoo Finance and FRED). The workbooks are formula driven, so you can change an input and everything recalculates. Random draws are fixed (seed 42) and stored in their own sheets, so results are reproducible.

**How these were made:** implemented with AI assistance (Claude Code) as a build-and-learn exercise following the bootcamp topics, then checked against independent Python calculations. Each README lists the limitations.

| Folder | What it is | Headline result |
|---|---|---|
| [market-risk-var-engine](market-risk-var-engine) | 1-day VaR by variance-covariance, historical simulation and Monte Carlo on JPM, MS and BAC, with Kupiec and Basel traffic light backtests | 99% VaR about 31k on 1m; 4 backtest exceptions in 250 days (Green) |
| [short-rate-models-vasicek-cir](short-rate-models-vasicek-cir) | Vasicek and CIR calibrated by maximum likelihood on 25 years of 3M T-bill yields, with Monte Carlo paths | Vasicek a = 0.103, b = 1.42%; CIR a = 1.16, b = 1.81% |
| [monte-carlo-derivative-pricing](monte-carlo-derivative-pricing) | One SPY call and put priced by Black-Scholes-Merton, a binomial tree and Monte Carlo, plus Asian and barrier options | Call 16.60 (BS) versus 15.72 (MC, SE 1.01) |
| [interest-rate-swap-valuation](interest-rate-swap-valuation) | 3-year swap valued by the forward-rate and bond methods, par rate, DV01, and a USD/JPY cross-currency swap | Par rate 4.31%, DV01 about 2,830 on 10m |
| [mean-variance-optimization](mean-variance-optimization) | Markowitz optimization on JPM, GS and MS: closed-form minimum variance and tangency portfolios plus 5,000 random portfolios | Max Sharpe 1.51 (tangency) |
| [pca-treasury-yield-curve](pca-treasury-yield-curve) | PCA of daily Treasury yield changes (1Y to 30Y, 2015 to 2024) | PC1 84.9%, PC2 11.0%, PC3 2.2% |
