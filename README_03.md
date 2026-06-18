---

## English

### Description
This Jupyter Notebook corresponds to **Month 3** of the research and implements deep learning models as alternative baselines. The stage moves from statistical modeling to recurrent, convolutional, and attention-based architectures.

**Note:** This is **univariate** forecasting — **BTC-USD** volatility (absolute log-returns) is predicted from 14-day historical windows.

### Input Data
* `crypto_log_returns_yahoo.csv` — main training set
* `crypto_log_returns_binance.csv` & `crypto_log_returns_coinbase.csv` — generalization checks (Frozen Inference)

### Methodology & Data Preparation
* **Scaling:** `MinMaxScaler` to `[0, 1]`.
* **Sliding Windows:** **14-day** window → next-day prediction.
* **Train/Test Split:** Chronological **80% / 20%** split.
* **DataLoaders:** PyTorch `TensorDataset` and `DataLoader` with batch size 32.
* **Early Stopping:** Patience of 15 epochs to prevent overfitting.

### Model Architectures
Four architectures are implemented and compared:

| Model | Description |
|---|---|
| **LSTM** | 2-layer LSTM with 0.2 dropout |
| **GRU** | 2-layer GRU — lighter alternative |
| **TCN** | Temporal Convolutional Network with dilated convolutions |
| **Transformer** | Time-series Transformer with positional encoding |

### Evaluation
* **Metrics:** **RMSE**, **MAE**, and **QLIKE** — compared against GARCH(1,1) from Notebook 02.
* **Pearson Correlation:** BTC-USD correlation check across Yahoo, Binance, and Coinbase.
* **Robustness (Frozen Inference):** Trained weights evaluated without retraining on Binance and Coinbase.

### Dependencies
* `torch` — Deep learning framework
* `numpy`, `pandas` — Data processing
* `scikit-learn` — `MinMaxScaler`, metrics
* `matplotlib` — Learning curves

**Installation:**
```bash
pip install torch scikit-learn pandas numpy matplotlib
```
