# ARIMA-Based Forecasting of Malaysia's Electrical & Electronics Exports

Forecasting Malaysia's monthly Electrical & Electronics (E&E) export values using a rigorously identified ARIMA model, validated on 35 years of data and deployed through an interactive Power BI dashboard.

## Overview

Malaysia's E&E sector is a major contributor to national export earnings, but no prior study had built and properly validated a monthly-resolution ARIMA model for it. This project closes that gap: it develops an ARIMA forecasting model following the full Box-Jenkins methodology, validates it two separate ways, and delivers the results through a stakeholder-facing dashboard.

## Data

- **423 monthly observations**, January 1990 – March 2025
- Source: DOSM (Department of Statistics Malaysia), FRED, Ministry of Economy Malaysia
- Target variable: Malaysia's monthly E&E export value (RM million)
- Three macroeconomic series (CPI inflation, MYR/USD exchange rate, U.S. Industrial Production Index) were analyzed descriptively for economic context but **not used as model inputs** — ARIMA is univariate by design

## Methodology

1. **Stationarity testing** — Augmented Dickey-Fuller test on the raw series (ADF = 0.31, p = 0.98 → non-stationary) and after first differencing (ADF = −6.79, p < 0.0001 → stationary). Established d = 1.
2. **Model identification** — Six candidate ARIMA(p,1,q) specifications compared via AIC. **ARIMA(2,1,2)** selected (AIC = 5289.81).
3. **Diagnostic checking** — Ljung-Box test on residuals.
4. **Validation, two ways:**
   - *Walk-forward expanding-window evaluation* across a 7-year historical test partition (Mar 2017–Dec 2023), at 1-, 2-, and 3-month horizons
   - *Out-of-sample validation* against genuinely unseen Jan–Mar 2025 data
5. **Deployment** — Python for modeling; forecasts and metrics exported to Power BI for an interactive one-page dashboard with KPI cards, historical trend + forecast interval chart, and horizon-level accuracy visuals.

## Results

| Horizon | RMSE (RM million) | R² |
|---|---|---|
| 1-month | 2,800.45 | 0.874 |
| 2-month | 2,971.24 | 0.858 |
| 3-month | 3,259.24 | 0.827 |

**Out-of-sample (Jan–Mar 2025):** MAE = RM2,739.45M, RMSE = RM3,758.89M. January and March forecasts landed within the 95% interval (errors of 2.6% and 1.7%); February missed by 17.5% — diagnosed as likely evidence of an unmodeled seasonal trough, consistent with the Ljung-Box residual test flagging remaining temporal structure.

## Key takeaway

A properly identified, non-seasonal ARIMA model gives strong, honest short-term forecasting performance for this series, with accuracy declining only gradually from 1- to 3-month horizons. Its main weakness — no seasonal component — is directly visible in the one out-of-sample month where it missed.

## Tech stack

Python (statsmodels, pandas), Power BI

## Limitations & future work

- No seasonal component (SARIMA is the natural next step)
- Out-of-sample validation window is small (n=3)
- Univariate — can't anticipate shocks driven by external events

See the full project report (`Project Report.pdf`) for the complete methodology, EDA, and diagnostics.

## Related project

A companion study benchmarks this ARIMA model against tuned Random Forest, SVR, and XGBoost models on the same data: [Comparative ML Forecasting](https://github.com/FR14-KJ/malaysia-ee-export-ml-comparison) 
