---

## English

### Description
This Jupyter Notebook (**Month 4**) is the core innovation of the thesis. It marks the transition from univariate baselines to **Spatio-Temporal Graph Neural Networks (ST-GNNs)**.

The cryptocurrency market is modeled as a network (graph): coins are **nodes** and correlations are **edges**. Forecasting is **multivariate** across all 10 coins simultaneously.

### Static Graph Construction
Two static graphs are built and visualized (via `networkx`):

* **Graph A (Correlation Graph):** Undirected graph with **Pearson correlation** edge weights (~84 edges).
* **Graph B (Volatility Spillover Graph):** Directed graph via **VAR(1)** (Diebold & Yılmaz, 2014) (~24 edges).

### Architectures & Experiments
After constructing the normalized adjacency matrix, two hybrid spatio-temporal architectures are trained:

* **GCN + LSTM (T-GCN):** Graph Convolutional Networks for spatial features + **LSTM** for temporal dynamics.
* **GAT + GRU:** Graph Attention Networks with dynamic neighbor weights + **GRU**.

**Target:** Absolute log-returns (realized volatility proxy) for all 10 coins, with a **14-day** sliding window and **80% / 20%** train/test split.

### Evaluation & Findings
* Training on the main dataset (**Yahoo Finance**).
* Metrics: **RMSE**, **MAE**, and **QLIKE** (evaluated primarily on the BTC-USD node).
* **Frozen Inference:** Trained weights evaluated on Binance and Coinbase without retraining.
* **Key Finding:** **Oversmoothing** — the static graph forces coins to share excessive information, reducing individual-level accuracy. This motivates the Adaptive ST-GNN in Notebook 05.

### Dependencies
* `torch` & `torch_geometric` — GCN/GAT layers
* `networkx` — Graph construction and visualization
* `statsmodels` — VAR spillover matrix
* `pandas`, `numpy`, `matplotlib`, `scikit-learn` — Data and visualization

**Installation:**
```bash
pip install torch torch_geometric networkx statsmodels scikit-learn pandas numpy matplotlib
```
