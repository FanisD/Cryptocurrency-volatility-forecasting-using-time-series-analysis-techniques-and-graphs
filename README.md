---

## English

### About the Thesis
This B.Sc. thesis investigates **volatility forecasting** in cryptocurrency markets. Given the extreme instability and 24/7 operation of this market, traditional univariate models struggle to estimate risk correctly because they ignore cross-asset dependencies and volatility **spillovers**.

The research proposes a shift toward **Spatio-Temporal Graph Neural Networks (ST-GNNs)**, comparing a range of baseline (econometric and deep learning) models against advanced **Adaptive Graph Learning** algorithms, with the goal of more accurate financial risk management.

### Research Objectives
* **Systematic Evaluation:** Compare traditional statistical models (`GARCH`) with modern deep learning architectures (`LSTM`, `GRU`, `TCN`, `Transformers`).
* **Graph Networks:** Model the market as a network (graph), where correlations act as **edges** and coins as **nodes**.
* **Adaptive ST-GNNs:** Address **oversmoothing** through networks that learn hidden market structure **end-to-end**, without static adjacency matrices.
* **Robustness Checks:** Validate generalization across multiple data sources (Yahoo Finance, Binance, Coinbase).

### Technologies & Libraries
The project was developed entirely in **Python 3** using:

* **Deep Learning & GNNs:** `PyTorch`, `PyTorch Geometric`
* **Econometrics & Statistics:** `statsmodels` (VAR, ADF), `arch` (GARCH(1,1)), `scipy` (Diebold-Mariano Test)
* **Data Management & Visualization:** `pandas`, `numpy`, `networkx`, `matplotlib`, `seaborn`
* **Data Engineering:** API-based data extraction (`yfinance`, Yahoo Finance / Binance / Coinbase).

### Repository Structure
The code is organized into 5 sequential **Jupyter Notebooks**. Each notebook has its own README:

| Notebook | README | Description |
|---|---|---|
| `01_Data_Collection_and_EDA.ipynb` | [README_01.md](README_01.md) | Data collection, cleaning, and log-return computation for 10 coins |
| `02_Baseline_Models_GARCH.ipynb` | [README_02.md](README_02.md) | Traditional GARCH(1,1) econometric baseline |
| `03_Deep_Learning_Baselines.ipynb` | [README_03.md](README_03.md) | Univariate DL models (LSTM, GRU, TCN, Transformer) |
| `04_Static_Graph_Construction_GNNs.ipynb` | [README_04.md](README_04.md) | Static graphs (Pearson & VAR) and hybrid ST-GNNs |
| `05_Adaptive_STGNN_and_Robustness.ipynb` | [README_05.md](README_05.md) | Adaptive ST-GNN, Diebold-Mariano test, and robustness checks |

**Data files (outputs of Notebook 01):**
* `crypto_log_returns_yahoo.csv` — main dataset (~2,986 days × 10 coins)
* `crypto_log_returns_binance.csv` — Binance (~999 days × 10 coins)
* `crypto_log_returns_coinbase.csv` — Coinbase (~349 days × 10 coins)

### Key Findings
* **Deep Learning models (LSTM/GRU)** reduced average error (RMSE) but were penalized by the QLIKE metric during sudden market crashes due to delayed adaptation.
* **Traditional GARCH** retained strong risk-awareness but failed at multivariate forecasting because of **mean reversion**.
* The **Adaptive ST-GNN** outperformed the naive persistence baseline with **p-value < 0.05** in the Diebold-Mariano test. It autonomously discovered hidden risk-transmission topology and showed strong robustness on new data, even with missing observations.

**Implementation Context**
This research was conducted as a B.Sc. Thesis at **Harokopio University**.
