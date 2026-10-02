# Interest Rate Swap Valuation

A 3-year semiannual pay-fixed swap on 10,000,000 notional, valued off a discount curve, plus a fixed-for-fixed USD/JPY cross-currency swap. Curve date 2024-12-31.

## Curve
Treasury par yields from FRED (6M 4.24%, 1Y 4.16%, 2Y 4.25%, 3Y 4.27%) are used as a continuously compounded zero-curve proxy, with the 1.5Y and 2.5Y points as linear averages of neighbours. This is a simplification: a production valuation would use SOFR OIS pillars and a proper bootstrap.

## Workbook sheets (`Interest_Rate_Swap_Valuation.xlsx`)
- **Inputs:** pillars, notional, fixed rate (4.00%), payment frequency, bump size, USD/JPY spot (157.37, FRED), cross-currency coupons.
- **Swap_Valuation:** six payment dates with zero rates, discount factors `exp(-z t)`, forward rates `(DF(t-1)/DF(t) - 1)/tau`, fixed and floating cash flows, and a second set of columns for a +1bp parallel bump.
- **CrossCurrency:** the USD bond and JPY bond valued separately, each with the final notional exchange, JPY converted at spot.

## Results
| Item | Value |
|---|---|
| PV fixed leg (4.00%) | 1,114,880 |
| PV floating leg, forward-rate method | 1,202,346 |
| PV floating leg, bond method `N (1 - DF_N)` | 1,202,346 (check = 0) |
| Par swap rate | 4.314% |
| Value to fixed payer | +87,466 |
| DV01 (+1bp parallel) | +2,830 |
| Annuity | 2.787 |

The swap is worth money to the fixed payer because 4.00% is below the 4.31% par rate. The two floating-leg methods agree exactly, which is a consistency check on the curve and forwards.

Cross-currency swap (pay USD 4% on 10m, receive JPY 1% on 1,573.7m yen): USD bond PV 9,912,534, JPY bond PV 10,088,542 in USD, value to the USD payer +176,008.

## Limitations
- Single-curve valuation: discounting and projection use the same curve, whereas current practice uses OIS discounting with separate projection curves.
- The JPY discount rate (0.7%) and JPY coupon (1.0%) are illustrative assumptions, as is the swap fixed rate; there is no cross-currency basis.
- No day-count or business-day conventions, and no credit or funding adjustments.
