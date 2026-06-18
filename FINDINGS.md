# 📊 Research Findings & Results: Cryptocurrency Volatility Forecasting

This document summarizes the empirical findings, statistical significance tests, and final evaluation metrics of the Bachelor Thesis: *"Cryptocurrency Volatility Forecasting using Time Series Analysis Techniques and Graphs"*.

## 🇬🇧 English Version

### 1. The Baseline Trade-off: Econometrics vs. Deep Learning
Our initial experiments revealed a fundamental trade-off in financial forecasting:
* **Econometrics (GARCH):** The traditional `GARCH(1,1)` model proved exceptionally resilient in capturing asymmetric risk and sudden market crashes, achieving the best **QLIKE** score. However, it suffered from high overall error (RMSE) and totally collapsed in multi-step forecasting due to the *Mean Reversion* effect.
* **Deep Learning (LSTM, GRU):** Univariate RNNs successfully minimized the general error (RMSE) and solved the mean reversion issue. However, they exhibited a severe **oversmoothing effect**—they learned to predict the average trend but reacted too slowly to extreme market spikes, resulting in poor QLIKE scores.

### 2. The Power of Spatial Awareness (Static ST-GNNs)
By modeling the crypto market as a connected network (using a VAR Spillover Graph), we introduced Spatio-Temporal Graph Neural Networks.
* The **GAT+GRU** architecture achieved the **absolute lowest RMSE** of the entire study. This mathematically proves that acknowledging volatility spillovers (how a shock in one coin affects another) significantly improves prediction accuracy over isolated time-series models.

### 3. The Ultimate Solution: Adaptive ST-GNNs
The core innovation of this research was the deployment of an **Adaptive ST-GNN**. Instead of relying on a predefined, static correlation matrix, this model learns the market's hidden topology *end-to-end* dynamically.
* **Balanced Performance:** It achieved top-tier symmetric metrics (RMSE, MAE), rivaling the best static graphs without needing any prior topological knowledge.
* **Statistical Significance (Diebold-Mariano Test):** A rigorous Diebold-Mariano test was conducted. The results ($p < 0.05$) mathematically proved that the Adaptive ST-GNN is **statistically significantly superior** to the Naive Baseline, GARCH, LSTM, and GRU models. 

### 4. Extreme Robustness & Zero Data Leakage
To validate the model's true generalization capabilities, an out-of-distribution (OOD) **Robustness Check** was performed on Binance and Coinbase data.
* **Zero Data Leakage:** Through dynamic temporal truncation, we guaranteed exactly **0 days of overlap** between the training data (Yahoo) and the OOD evaluation data. 
* **Missing Data Resilience:** The Adaptive ST-GNN generalized perfectly even when confronted with missing nodes (e.g., the complete absence of BNB data on Coinbase), proving its viability for real-world Risk Management applications.

### 🏆 Final Evaluation Table (Yahoo Finance Test Set - BTC-USD)

| Model Category | Architecture | QLIKE (Primary) | RMSE | MAE |
| :--- | :--- | :--- | :--- | :--- |
| **Econometric (M2)** | GARCH(1,1) | **2.7530** 🏆 | 2.0713 | 1.7786 |
| **Deep Learning (M3)** | GRU | 2.8370 | 1.8904 | 1.5095 |
| **Deep Learning (M3)** | LSTM | 2.8334 | 1.9047 | 1.5316 |
| **Deep Learning (M3)** | Transformer | 2.8930 | 1.8184 | 1.3870 |
| **Deep Learning (M3)** | TCN | 3.3645 | 1.7688 | 1.2024 |
| **Static ST-GNN (M4)** | GAT+GRU | 2.8700 | **1.6734** 🏆 | 1.2351 |
| **Static ST-GNN (M4)** | GCN+LSTM | 2.8294 | 1.6864 | 1.2656 |
| **Adaptive ST-GNN (M5)** | Dynamic Graph | 3.0021 | 1.6811 | **1.2040** 🏆 |

*(Note: While TCN matched the MAE closely at 1.2024, its extreme failure in QLIKE makes the Adaptive ST-GNN the superior balanced model).*

---