---

## English

### Description
This Jupyter Notebook is the final and most advanced stage of the thesis (**Month 5**). It addresses the **oversmoothing** problem observed with static graphs.

An **Adaptive Spatio-Temporal Graph Neural Network (Adaptive ST-GNN)** is implemented, inspired by architectures such as *Graph WaveNet* and *MTGNN*. The model learns market topology **end-to-end** and autonomously discovers hidden cross-asset correlations.

### Architecture
The Adaptive ST-GNN combines three components:

1. **TCN (Temporal Convolution):** Extracts temporal patterns (Conv2d over Time × Nodes).
2. **Adaptive Graph Convolution:** **Node Embeddings** per coin → dynamic adjacency matrix (ReLU + Softmax).
3. **GRU:** Fuses spatio-temporal information for final prediction.

**Target:** Multivariate absolute log-return forecasting for 10 coins, **14-day** sliding window, **80% / 20%** split. Missing values are filled with `fillna(0)`.

### Visualization
The learned adjacency matrix is exported and visualized as a **heatmap** (seaborn), revealing the risk-transmission structure learned during training.

### Robustness Checks
**Frozen Inference** validates generalization:

1. Weights (trained on Yahoo Finance) are frozen.
2. The model is evaluated on **Binance** and **Coinbase** without retraining.
3. Behavior under missing data is tested (NaNs in the Coinbase dataset).

### Statistical Evaluation (Diebold-Mariano Test)
The **Diebold-Mariano (DM Test)** confirms statistical significance:

* **Comparison:** Adaptive ST-GNN vs **Naive Persistence Baseline** (tomorrow's volatility = today's).
* **Loss function:** Squared error (RMSE-based differential).
* **Result:** p-value < 0.05 → statistically significant improvement.

### Dependencies
* `torch`, `torch.nn` — Neural network architecture
* `seaborn`, `matplotlib` — Heatmaps
* `scipy.stats` — Diebold-Mariano test
* `pandas`, `numpy`, `scikit-learn` — Data management

**Installation:**
```bash
pip install torch seaborn scipy scikit-learn pandas numpy matplotlib
```
