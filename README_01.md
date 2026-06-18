---

## English

### Description
This Jupyter Notebook is the first foundational stage of the thesis (**Month 1**). It handles automated data collection, cleaning, and preprocessing of historical daily close prices for 10 major cryptocurrencies, which serve as **nodes** in the subsequent Spatio-Temporal Graph Neural Networks (ST-GNNs).

### Data Sources (APIs)
The notebook pulls data from three sources to ensure model reliability and enable later robustness checks:

* **Yahoo Finance** (`yfinance`): Main **baseline** dataset, history from `2018-01-01` (~2,986 log-return days).
* **Binance API:** Alternative source for the most recent ~1,000 daily candles (~999 log-returns).
* **Coinbase API:** Robustness-check source (~349 log-returns), with intentionally incomplete data for some coins (see below).

### Selected Cryptocurrencies
The 10 cryptocurrencies examined (forming the correlation graph nodes) are:

* **BTC-USD** (Bitcoin) · **ETH-USD** (Ethereum) · **XRP-USD** (Ripple)
* **LTC-USD** (Litecoin) · **ADA-USD** (Cardano) · **BNB-USD** (Binance Coin)
* **SOL-USD** (Solana) · **DOGE-USD** (Dogecoin) · **TRX-USD** (Tron) · **LINK-USD** (Chainlink)

### Preprocessing Steps
* **Data Retrieval:** API calls to fetch daily close prices.
* **Merging:** Unified DataFrame per source with aligned dates and common column naming (Yahoo format).
* **Data Cleaning:** Missing values handled via **forward fill (`ffill`)** for Yahoo and Binance to avoid data leakage. On Coinbase, NaNs are preserved where the source provides no data.
* **Return Computation:** Conversion to daily **log-returns** using `ln(P_t / P_{t-1})`.

### Missing Data by Source
| Source | Coin | Status |
|---|---|---|
| Yahoo Finance | SOL-USD | ~830 NaNs (late listing) |
| Coinbase | TRX-USD | Fully missing (349 NaNs) |
| Coinbase | BNB-USD | Partially missing (~214 NaNs) |

Later notebooks handle these gaps with `fillna(0)` where required.

### Output Files
Running the notebook produces three CSV files:

1. `crypto_log_returns_yahoo.csv` — main dataset (2,986 rows × 10 columns)
2. `crypto_log_returns_binance.csv` — Binance (999 rows × 10 columns)
3. `crypto_log_returns_coinbase.csv` — Coinbase (349 rows × 10 columns)

### Dependencies
* `pandas`, `numpy` — Data management and computation
* `yfinance` — Yahoo Finance data retrieval
* `requests` — REST API calls for Binance/Coinbase

**Installation:**
```bash
pip install pandas numpy yfinance requests
```
