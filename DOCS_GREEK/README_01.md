# 01_Data_Collection_and_EDA.ipynb
### Data Collection & Preprocessing

---

## Ελληνικά

### Περιγραφή
Αυτό το Jupyter Notebook αποτελεί το πρώτο και θεμελιώδες στάδιο της πτυχιακής εργασίας (**Μήνας 1**). Είναι υπεύθυνο για την αυτοματοποιημένη άντληση, τον καθαρισμό και την προεπεξεργασία των ιστορικών δεδομένων (daily close prices) για 10 κορυφαία κρυπτονομίσματα, τα οποία θα αποτελέσουν τους κόμβους (**nodes**) στα Χωροχρονικά Νευρωνικά Δίκτυα Γράφων (ST-GNNs).

### Πηγές Δεδομένων (APIs)
Το notebook αντλεί δεδομένα από τρεις (3) διαφορετικές πηγές, προκειμένου να εξασφαλιστεί η αξιοπιστία των μοντέλων και να καταστούν δυνατοί οι μετέπειτα έλεγχοι ανθεκτικότητας (Robustness Checks):

* **Yahoo Finance** (`yfinance`): Κύριο (**baseline**) dataset, με ιστορικό από `2018-01-01` (~2.986 ημέρες log-returns).
* **Binance API:** Εναλλακτική πηγή για τα τελευταία ~1.000 ημερήσια candles (~999 log-returns).
* **Coinbase API:** Πηγή ελέγχου ανθεκτικότητας (~349 log-returns), με ελλιπή δεδομένα για ορισμένα νομίσματα (βλ. παρακάτω).

### Επιλεγμένα Κρυπτονομίσματα
Τα 10 κρυπτονομίσματα που εξετάζονται (και θα διαμορφώσουν τον γράφο των συσχετίσεων) είναι:

* **BTC-USD** (Bitcoin)
* **ETH-USD** (Ethereum)
* **XRP-USD** (Ripple)
* **LTC-USD** (Litecoin)
* **ADA-USD** (Cardano)
* **BNB-USD** (Binance Coin)
* **SOL-USD** (Solana)
* **DOGE-USD** (Dogecoin)
* **TRX-USD** (Tron)
* **LINK-USD** (Chainlink)

### Βήματα Προεπεξεργασίας
* **Λήψη Δεδομένων:** Κλήση των αντίστοιχων APIs και εξαγωγή των ημερήσιων τιμών κλεισίματος (**Close Prices**).
* **Συγχώνευση (Merging):** Δημιουργία ενός ενιαίου DataFrame ανά πηγή, με ευθυγράμμιση ημερομηνιών και κοινή ονομασία στηλών (Yahoo format).
* **Καθαρισμός (Data Cleaning):** Χειρισμός ελλιπών τιμών (Missing Values) μέσω **Forward Fill (`ffill`)** για Yahoo και Binance, ώστε να αποφευχθεί data leakage. Στο Coinbase, τα NaNs διατηρούνται όπου η πηγή δεν παρέχει δεδομένα.
* **Υπολογισμός Αποδόσεων:** Μετατροπή των τιμών σε ημερήσιες λογαριθμικές αποδόσεις (**Log-Returns**) με τύπο `ln(P_t / P_{t-1})`.

### Ελλιπή Δεδομένα ανά Πηγή
| Πηγή | Νόμισμα | Κατάσταση |
|---|---|---|
| Yahoo Finance | SOL-USD | ~830 NaNs (καθυστερημένη λίστα) |
| Coinbase | TRX-USD | Πλήρως ελλιπές (349 NaNs) |
| Coinbase | BNB-USD | Μερικώς ελλιπές (~214 NaNs) |

Τα επόμενα notebooks χειρίζονται αυτά τα κενά με `fillna(0)` όπου απαιτείται.

### Εξαγόμενα Αρχεία (Outputs)
Η εκτέλεση του notebook παράγει τρία (3) αρχεία CSV:

1. `crypto_log_returns_yahoo.csv` — κύριο dataset (2.986 γραμμές × 10 στήλες)
2. `crypto_log_returns_binance.csv` — Binance (999 γραμμές × 10 στήλες)
3. `crypto_log_returns_coinbase.csv` — Coinbase (349 γραμμές × 10 στήλες)

### Απαιτούμενες Βιβλιοθήκες (Dependencies)
* `pandas`, `numpy` — Διαχείριση δεδομένων και υπολογισμοί
* `yfinance` — Άντληση δεδομένων από Yahoo Finance
* `requests` — REST API κλήσεις για Binance/Coinbase

**Εγκατάσταση:**
```bash
pip install pandas numpy yfinance requests
```