---

## English

### Description
This Jupyter Notebook is the second stage of the research (**Month 2**) and focuses on **univariate** time series analysis. Its main purpose is implementing the traditional econometric **GARCH(1,1)** model as the reference **baseline** for volatility evaluation.

The notebook demonstrates that traditional models capture **volatility clustering**, but fail at multivariate forecasting due to **mean reversion**.

### Input Data
* `crypto_log_returns_yahoo.csv` — daily log-returns for 10 cryptocurrencies
* The model is trained on **BTC-USD** as a representative asset

### Methodology & Implementation Steps
* **Stationarity Test:** Augmented Dickey-Fuller (**ADF Test**) to confirm log-return stationarity.
* **Autocorrelation Analysis:** Visualization via **ACF** (Autocorrelation Function).
* **Hybrid Modeling:** **ARIMA** for the mean equation and **GARCH(1,1)** for the conditional variance. Both Constant Mean and AR(1) mean specifications are tested.
* **Train/Test Split:** Chronological **80% / 20%** split.
* **Forecasting:** Predictions on the held-out test set.
* **Metrics:** **RMSE**, **MAE**, and **QLIKE** (Quasi-Likelihood).

### Stage Conclusions
* **GARCH** performs well at risk quantification and shows strong QLIKE behavior.
* As a **univariate model**, it cannot account for spillovers from the other 9 coins — motivating the multivariate networks in later notebooks.

### Dependencies
* `pandas`, `numpy` — Data management
* `matplotlib` — Visualizations
* `statsmodels` — ADF, ARIMA, ACF
* `arch` — GARCH(1,1)
* `scikit-learn` — RMSE, MAE

**Installation:**
```bash
pip install arch statsmodels scikit-learn pandas numpy matplotlib
```
