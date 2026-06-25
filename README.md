# Bitcoin Price Forecasting with ARIMA (Box-Jenkins Methodology)

Time series forecasting project applying the Box-Jenkins methodology to daily Bitcoin (BTC/USD) closing prices. Built as part of my statistics coursework, then cleaned up as a standalone portfolio project.

## Project Summary

I modeled 730 days of daily BTC/USD closing prices (Jan 2021 – Dec 2022) — a period that includes a strong bull run, the all-time high, and the 2022 crash (Terra/Luna, FTX). The goal: test whether a classical ARIMA model can produce well-calibrated forecasts on an asset this volatile, and be honest about where it breaks down.

**Result:** ARIMA(2,1,1), selected by AIC, achieved a MAPE of 11.6% on a 146-day out-of-sample test set, with 100% empirical coverage of the 95% confidence interval. Residual diagnostics (Ljung-Box, Q-Q plot) confirmed the model captured the available autocorrelation structure, with only the expected fat-tail deviation typical of financial returns.

## Methodology

1. **Stationarity check** — Augmented Dickey-Fuller test on log-prices (non-stationary, p > 0.05) and on log-returns (stationary, p < 0.05). First-order differencing was sufficient.
2. **Model identification** — ACF/PACF analysis on the stationary log-returns series pointed to low-order AR/MA components.
3. **Model selection** — Compared 6 candidate ARIMA(p,1,q) specifications by AIC/BIC. ARIMA(2,1,1) won, balancing fit and parsimony.
4. **Estimation** — Parameters estimated via maximum likelihood (Nelder-Mead optimization).
5. **Validation** — Residuals checked for autocorrelation (Ljung-Box), normality (Q-Q plot), and white-noise behavior. No significant autocorrelation remained.
6. **Forecasting & evaluation** — 146-day-ahead forecast on the held-out test set, evaluated with MAE, RMSE, MAPE, and confidence interval coverage.

## Key Results

| Metric | Value |
|---|---|
| Selected model | ARIMA(2,1,1) |
| AIC | -1991.51 |
| MAE | 3,408 USD |
| RMSE | 5,309 USD |
| MAPE | 11.6% |
| 95% CI empirical coverage | 100% |
| Ljung-Box p-value (residuals) | 0.85 (no remaining autocorrelation) |

## What This Project Shows (and Its Limits)

ARIMA is a strong, interpretable baseline — it's transparent about its assumptions and easy to validate rigorously, which matters more to me than chasing a flashier model blindly. But it assumes constant variance in the errors, which doesn't hold for crypto: forecast accuracy degrades sharply during high-volatility stretches (the errors in this project visibly spike right around the 2022 crash period). The natural next step would be ARIMA-GARCH, to explicitly model that time-varying volatility, or an LSTM for comparison.

I'd rather show a model that's correctly validated with known limitations than a more complex one I can't fully justify.

## Tools

Python 3 — numpy, scipy, matplotlib, statsmodels equivalents (manual MLE via Nelder-Mead for the estimation step)

## Files

- `report.pdf` — full write-up with all figures (stationarity tests, ACF/PACF, model comparison table, residual diagnostics, forecast plots)

## Author

Mohamed EL ALAMI — Statistics & Applied Economics student (INSEA)
(academic coursework project, Time Series module)
