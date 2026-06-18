# 04_Static_Graph_Construction_GNNs.ipynb
### Static Graph Construction & Spatio-Temporal GNNs

---

## Ελληνικά

### Περιγραφή
Αυτό το Jupyter Notebook (**Μήνας 4**) αποτελεί τον πυρήνα της καινοτομίας της πτυχιακής εργασίας. Σηματοδοτεί τη μετάβαση από τα μονομεταβλητά baselines στα **Χωροχρονικά Νευρωνικά Δίκτυα Γράφων (Spatio-Temporal GNNs)**.

Η αγορά κρυπτονομισμάτων μοντελοποιείται ως δίκτυο (γράφημα): τα νομίσματα είναι κόμβοι (**nodes**) και οι συσχετίσεις ακμές (**edges**). Η πρόβλεψη γίνεται **πολυμεταβλητά** για και τα 10 νομίσματα ταυτόχρονα.

### Δημιουργία Στατικών Γράφων (Graph Construction)
Κατασκευάζονται και οπτικοποιούνται (μέσω `networkx`) δύο στατικοί γράφοι:

* **Graph A (Correlation Graph):** Μη-κατευθυνόμενος γράφος με βάρη από **Pearson correlation** (~84 ακμές).
* **Graph B (Volatility Spillover Graph):** Κατευθυνόμενος γράφος μέσω **VAR(1)** (Diebold & Yılmaz, 2014) (~24 ακμές).

### Αρχιτεκτονικές & Πειράματα
Μετά τον κανονικοποιημένο πίνακα γειτνίασης, εκπαιδεύονται δύο υβριδικές χωροχρονικές αρχιτεκτονικές:

* **GCN + LSTM (T-GCN):** Graph Convolutional Networks για χωρικά χαρακτηριστικά + **LSTM** για τη χρονική διάσταση.
* **GAT + GRU:** Graph Attention Networks με δυναμικά βάρη γειτόνων + **GRU**.

**Target:** Absolute log-returns (realized volatility proxy) για και τα 10 νομίσματα, με sliding window **14 ημερών** και split **80% / 20%**.

### Αξιολόγηση και Ευρήματα
* Εκπαίδευση στο κύριο dataset (**Yahoo Finance**).
* Μετρικές: **RMSE**, **MAE** και **QLIKE** (αξιολόγηση κυρίως στο BTC-USD node).
* **Frozen Inference:** Τα εκπαιδευμένα βάρη αξιολογούνται σε Binance και Coinbase χωρίς retraining.
* **Key Finding:** Φαινόμενο **υπερ-εξομάλυνσης (oversmoothing)** — ο στατικός γράφος αναγκάζει τα νομίσματα να μοιράζονται υπερβολικά πολλές πληροφορίες, μειώνοντας την ατομική ακρίβεια. Αυτό οδηγεί στο Adaptive ST-GNN του Notebook 05.

### Απαιτούμενες Βιβλιοθήκες (Dependencies)
* `torch` & `torch_geometric` — GCN/GAT layers
* `networkx` — Δημιουργία και οπτικοποίηση γράφων
* `statsmodels` — VAR spillover matrix
* `pandas`, `numpy`, `matplotlib`, `scikit-learn` — Δεδομένα και οπτικοποίηση

**Εγκατάσταση:**
```bash
pip install torch torch_geometric networkx statsmodels scikit-learn pandas numpy matplotlib
```