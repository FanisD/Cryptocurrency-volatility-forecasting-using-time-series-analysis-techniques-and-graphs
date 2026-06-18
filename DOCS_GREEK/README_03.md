# 03_Deep_Learning_Baselines.ipynb
### Deep Learning Baselines (LSTM, GRU, TCN, Transformer)

---

## Ελληνικά

### Περιγραφή
Αυτό το Jupyter Notebook αντιστοιχεί στον **Μήνα 3** της έρευνας και επικεντρώνεται στην υλοποίηση και αξιολόγηση μοντέλων Βαθιάς Μάθησης (Deep Learning) ως εναλλακτικά baselines. Το στάδιο αυτό μεταβαίνει από τη στατιστική μοντελοποίηση σε αναδρομικές, συνελικτικές και attention-based αρχιτεκτονικές.

**Σημείωση:** Πρόκειται για **μονομεταβλητή** πρόβλεψη — η μεταβλητότητα του **BTC-USD** (absolute log-returns) προβλέπεται από ιστορικά παράθυρα 14 ημερών.

### Δεδομένα Εισόδου (Input Data)
* `crypto_log_returns_yahoo.csv` — κύριο σύνολο εκπαίδευσης
* `crypto_log_returns_binance.csv` & `crypto_log_returns_coinbase.csv` — έλεγχος γενίκευσης (Frozen Inference)

### Μεθοδολογία & Προετοιμασία
* **Κλιμάκωση (Scaling):** `MinMaxScaler` στο διάστημα `[0, 1]`.
* **Κυλιόμενα Παράθυρα (Sliding Windows):** Παράθυρο **14 ημερών** → πρόβλεψη της επόμενης ημέρας.
* **Train/Test Split:** Χρονολογικός διαχωρισμός **80% / 20%**.
* **DataLoaders:** `TensorDataset` και `DataLoader` (PyTorch) με batch size 32.
* **Early Stopping:** Patience 15 epochs για αποφυγή υπερπροσαρμογής.

### Αρχιτεκτονικές Μοντέλων
Υλοποιούνται και συγκρίνονται **4 αρχιτεκτονικές**:

| Μοντέλο | Περιγραφή |
|---|---|
| **LSTM** | 2-layer LSTM με dropout 0.2 |
| **GRU** | 2-layer GRU — ελαφρύτερη εναλλακτική |
| **TCN** | Temporal Convolutional Network με dilated convolutions |
| **Transformer** | Time-series Transformer με positional encoding |

### Αξιολόγηση
* **Μετρικές:** **RMSE**, **MAE** και **QLIKE** — σύγκριση και με το GARCH(1,1) του Notebook 02.
* **Pearson Correlation:** Έλεγχος συσχέτισης BTC-USD μεταξύ Yahoo, Binance και Coinbase.
* **Robustness (Frozen Inference):** Τα εκπαιδευμένα βάρη αξιολογούνται χωρίς retraining σε Binance και Coinbase.

### Απαιτούμενες Βιβλιοθήκες (Dependencies)
* `torch` — Πλαίσιο Βαθιάς Μάθησης
* `numpy`, `pandas` — Επεξεργασία δεδομένων
* `scikit-learn` — `MinMaxScaler`, μετρικές
* `matplotlib` — Learning curves

**Εγκατάσταση:**
```bash
pip install torch scikit-learn pandas numpy matplotlib
```