# 05_Adaptive_STGNN_and_Robustness.ipynb
### Adaptive ST-GNN & Robustness Checks

---

## Ελληνικά

### Περιγραφή
Αυτό το Jupyter Notebook αποτελεί το τελικό και πιο προηγμένο στάδιο της πτυχιακής εργασίας (**Μήνας 5**). Σκοπός του είναι η επίλυση του προβλήματος **oversmoothing** που παρατηρήθηκε με στατικούς γράφους.

Υλοποιείται ένα **Adaptive Spatio-Temporal Graph Neural Network (Adaptive ST-GNN)**, εμπνευσμένο από αρχιτεκτονικές όπως το *Graph WaveNet* και το *MTGNN*. Το μοντέλο μαθαίνει **end-to-end** την τοπολογία της αγοράς και ανακαλύπτει αυτόνομα τις κρυφές διασταυρούμενες συσχετίσεις.

### Αρχιτεκτονική
Το Adaptive ST-GNN συνδυάζει τρία στοιχεία:

1. **TCN (Temporal Convolution):** Εξαγωγή χρονικών μοτίβων (Conv2d πάνω στη διάσταση Time × Nodes).
2. **Adaptive Graph Convolution:** **Node Embeddings** για κάθε νόμισμα → δυναμικός πίνακας γειτνίασης (ReLU + Softmax).
3. **GRU:** Σύνδεση χωροχρονικών πληροφοριών για την τελική πρόβλεψη.

**Target:** Πολυμεταβλητή πρόβλεψη absolute log-returns για 10 νομίσματα, sliding window **14 ημερών**, split **80% / 20%**. Τα missing values συμπληρώνονται με `fillna(0)`.

### Οπτικοποίηση
Ο μαθημένος πίνακας γειτνίασης εξάγεται και οπτικοποιείται ως **heatmap** (seaborn), αποκαλύπτοντας τη δομή μετάδοσης κινδύνου που «αντιλήφθηκε» το δίκτυο.

### Έλεγχοι Ανθεκτικότητας (Robustness Checks)
Πραγματοποιείται **Frozen Inference** για επιβεβαίωση γενίκευσης:

1. Τα βάρη (εκπαιδευμένα στο Yahoo Finance) «κλειδώνονται».
2. Το μοντέλο αξιολογείται σε **Binance** και **Coinbase** χωρίς retraining.
3. Ελέγχεται η συμπεριφορά σε ελλιπή δεδομένα (NaNs στο Coinbase dataset).

### Στατιστική Αξιολόγηση (Diebold-Mariano Test)
Εκτελείται ο έλεγχος **Diebold-Mariano (DM Test)** για στατιστική σημαντικότητα:

* **Σύγκριση:** Adaptive ST-GNN vs **Naive Persistence Baseline** (η αυριανή μεταβλητότητα = η σημερινή).
* **Loss function:** Squared error (RMSE-based differential).
* **Αποτέλεσμα:** p-value < 0.05 → στατιστικά σημαντική βελτίωση.

### Απαιτούμενες Βιβλιοθήκες (Dependencies)
* `torch`, `torch.nn` — Αρχιτεκτονική νευρωνικών δικτύων
* `seaborn`, `matplotlib` — Heatmaps
* `scipy.stats` — Diebold-Mariano test
* `pandas`, `numpy`, `scikit-learn` — Διαχείριση δεδομένων

**Εγκατάσταση:**
```bash
pip install torch seaborn scipy scikit-learn pandas numpy matplotlib
```