# Decomposing Economic Time Series

## Objective
I decomposed U.S. retail sales, tested U.S. real GDP for stationarity, and ran change-point detection on GDP growth to separate trend, seasonality, and structural breaks before forecasting.

## Methodology
* Pulled live FRED series with `fredapi`: U.S. retail sales, not seasonally adjusted (`RSXFSN`, 321 months), and quarterly real GDP (`GDPC1`, from 1990).
* Ran a classical multiplicative decomposition on retail sales to extract its twelve fixed monthly seasonal factors and residual spread.
* Ran robust STL decomposition on log retail sales to allow the seasonal pattern to drift over time while keeping one-time shocks out of the trend.
* Tested real GDP in levels and after first-differencing with both ADF and KPSS tests, applying the 2x2 decision table at the 5% level.
* Detected structural breaks in quarterly GDP growth using PELT (`ruptures`) with the `rbf` cost model and penalty 1, and checked sensitivity across penalties of 0.5 and 2.
* Completed a `seasonal_index()` function in pandas that rebuilds the classical multiplicative seasonal factors for any monthly series and compared December seasonality across two decades.

## Key Findings
* In classical multiplicative decomposition of retail sales, December had the highest seasonal factor at 1.1466 and February was the lowest month at 0.8886.
* Robust STL on log retail sales isolated the 2020 pandemic shock inside the residual panel, giving a residual standard deviation of 0.0237 compared with 0.0204 for classical decomposition.
* On real GDP from 1990, the ADF and KPSS 2x2 table concluded non-stationary (difference it) in levels and stationary (model it as it is) after first-differencing.
* PELT with the `rbf` cost model and penalty 1 detected 4 structural breaks in quarterly GDP growth at 1991-04-01, 2000-01-01, 2020-01-01, and 2021-04-01.
* Using `seasonal_index()`, December's seasonal factor declined from 1.1637 in 2000-2009 to 1.1274 in 2015-2024, reflecting a flatter holiday spike over time.
