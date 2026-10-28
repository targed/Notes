#### Problem 1 (Standard Direct Mirror)
  A machine learning training dataset contains $n = 1{,}200$ rows.
  * **(a)** With $6$-fold cross-validation, how many rows does each surrogate model train on and validate on?
  * **(b)** How many total model fits are executed to complete the $6$-fold CV process?
  * **(c)** How many fits does Leave-One-Out Cross-Validation (LOOCV) require for this dataset? On how many rows does each individual LOOCV fit train and validate?
  
  ---
#### Problem 2 (Holdout Partitioning Combined with 10-Fold CV)
  A raw dataset has $N = 2{,}000$ observations. An engineer first executes an $80/20$ train/test split. The $80\%$ training partition is then evaluated using $10$-fold cross-validation.
  * **(a)** How many rows are vaulted in the independent test set?
  * **(b)** During each fold rotation of the $10$-fold CV, how many rows does the surrogate model train on and validate on?
  * **(c)** Across the entire end-to-end workflow (including hyperparameter tuning via CV, refitting the winner on all training data, and final test evaluation), exactly how many total models are trained from scratch?
  
  ---
#### Problem 3 (Repeated Stratified $k$-Fold Bookkeeping)
  A biomedical dataset of $n = 800$ instances is evaluated using:
  ```python
  cv = RepeatedStratifiedKFold(n_splits=5, n_repeats=10, random_state=0)
  scores = cross_val_score(model, X, y, cv=cv)
  ```
  * **(a)** For any individual fold split, how many rows are used for training and how many are used for validation?
  * **(b)** Exactly how many total model fits are executed across the entire repeated CV procedure?
  * **(c)** How many total out-of-fold validation scores are stored in `scores`, and how many distinct $5$-fold mean scores can be computed?
  
  ---
#### Problem 4 (Uneven Dataset Slicing & Remainder Allocation)
  A clinical trial dataset contains $n = 503$ patient records. An analyst configures standard $5$-fold cross-validation (`KFold(n_splits=5)`).
  * **(a)** Explain mathematically why all 5 validation folds cannot have the exact same number of rows.
  * **(b)** How does scikit-learn allocate the remainder across the 5 validation folds? State the exact row count of each of the 5 validation folds.
  * **(c)** For a fold of the larger size, exactly how many rows are used by the surrogate model for training?
  
  ---
#### Problem 5 (Stratified Class Allocation Bookkeeping)
  A bank dataset containing $n = 1{,}000$ loan applicants has an imbalanced default rate: exactly $200$ applicants defaulted ($Y = 1$, positive class), and $800$ applicants repaid ($Y = 0$, negative class). The pipeline is evaluated with $5$-fold `StratifiedKFold`.
  * **(a)** In each of the 5 validation folds, exactly how many positive and negative cases are present?
  * **(b)** In each corresponding training fold, exactly how many positive and negative cases are used to train the surrogate model?
  * **(c)** If unstratified `KFold(n_splits=5, shuffle=False)` were mistakenly applied to the dataset sorted by label (all 800 negatives followed by all 200 positives), how many positive cases would be in Folds 1–4 versus Fold 5?
  
  ---
#### Problem 6 (GridSearchCV Total Fit Counts)
  An analyst tunes a regularized Logistic Regression pipeline on $n = 600$ training instances using:
  ```python
  grid = {
    'logisticregression__C': np.logspace(-2, 3, 6),  # 6 values
    'logisticregression__penalty': ['l1', 'l2'],  # 2 values
    'logisticregression__solver': ['liblinear', 'saga'],  # 2 values
  }
  gs = GridSearchCV(pipe, grid, cv=5, refit=True)
  gs.fit(X_train, y_train)
  ```
  * **(a)** How many unique hyperparameter configurations are in the Cartesian search space?
  * **(b)** How many model fits are performed strictly during the cross-validation search phase?
  * **(c)** Accounting for the final refitted model on the entire training set, what is the grand total of models trained from scratch?
  
  ---
#### Problem 7 (Nested Cross-Validation Bookkeeping)
  A genomic study with $N = 100$ patient profiles uses **Nested Cross-Validation** to tune an SVM while estimating generalization error without an independent holdout set:
  * **Outer Loop:** $5$-fold cross-validation (evaluates generalization error).
  * **Inner Loop:** $4$-fold cross-validation (evaluates $C \in \{0.1, \; 1.0, \; 10.0\}$ inside each outer training fold).
  * **(a)** In any outer fold, how many samples are in the outer test fold, and how many are in the outer training set?
  * **(b)** Inside each inner CV run, how many samples are in each inner validation fold, and how many are used to train the inner candidate models?
  * **(c)** How many total model fits are executed across the entire nested CV experiment (inner tuning fits + outer refits)?
  
  ---
#### Problem 8 (Multi-Metric Scoring Evaluations)
  A classifier is evaluated on $n = 900$ samples using $3$-fold cross-validation with four metrics:
  ```python
  scoring = ['accuracy', 'precision', 'recall', 'f1']
  results = cross_validate(clf, X, y, cv=3, scoring=scoring)
  ```
  * **(a)** How many rows are used to train and validate on each fold rotation?
  * **(b)** How many total model fits are executed by scikit-learn?
  * **(c)** How many individual test metric score values are computed and stored in the `results` dictionary?
  
  ---
#### Problem 9 (GroupKFold Clustered Patient Bookkeeping)
  An arrhythmia detection dataset contains $n = 600$ electrocardiogram (ECG) recordings collected from $60$ unique hospital patients, where each patient contributes exactly $10$ distinct recordings. To prevent entity leakage, the engineer configures `GroupKFold(n_splits=5)` grouped by `patient_id`.
  * **(a)** In each validation fold, exactly how many unique patients and how many total ECG rows are present?
  * **(b)** In each training fold, exactly how many unique patients and how many total ECG rows are used to train the model?
  * **(c)** What severe methodological defect would occur if the engineer used standard `KFold(n_splits=5)` instead?
  
  ---
#### Problem 10 (TimeSeriesSplit Walk-Forward Allocation)
  A financial forecasting pipeline evaluates $n = 500$ consecutive trading days of market data using `TimeSeriesSplit(n_splits=4)`.
  * **(a)** Under scikit-learn's default formulation, how many samples are allocated to each validation/test split?
  * **(b)** For Split 1, Split 2, Split 3, and Split 4, state the exact number of training samples used.
  * **(c)** Explain why the training set size expands with each split while the test set size remains constant.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C2
### Standard Bookkeeping Formulas:
  * **Validation Fold Size:** $N_{\text{val}} = \frac{n}{k}$
  * **Training Fold Size:** $N_{\text{train}} = \left(\frac{k - 1}{k}\right)n = n - N_{\text{val}}$
  * **Leave-One-Out (LOOCV):** $k = n \implies n$ fits, where each fit trains on $n - 1$ rows and validates on $1$ row.
  * **GridSearchCV Fits:** $N_{\text{fits}} = (\text{Number of Settings}) \times k_{\text{folds}} \; (+1 \text{ if } \texttt{refit=True})$.
  
  ---
### Problem 1 Solution
  * **(a) Row Allocation per Fit ($n = 1{,}200, k = 6$):**
  $$\text{Validation Rows per Fit} = \frac{n}{k} = \frac{1{,}200}{6} = \mathbf{200 \text{ rows}}$$
  $$\text{Training Rows per Fit} = n - N_{\text{val}} = 1{,}200 - 200 = \mathbf{1{,}000 \text{ rows}} \quad \left(\text{or } \frac{5}{6} \times 1{,}200 = 1{,}000\right)$$
  * **(b) Total 6-Fold Fits:**
  Each fold serves as the validation set once $\implies \mathbf{6 \text{ model fits}}$.
  * **(c) Leave-One-Out Cross-Validation (LOOCV):**
  * Number of fits: $k = n = \mathbf{1{,}200 \text{ fits}}$.
  * Training rows per fit: $n - 1 = 1{,}200 - 1 = \mathbf{1{,}199 \text{ rows}}$.
  * Validation rows per fit: exactly $\mathbf{1 \text{ row}}$. `[L18, Review C2]`
  
  ---
### Problem 2 Solution
  * **(a) Vaulted Test Set Size ($N = 2{,}000$, 80/20 Split):**
  $$N_{\text{test}} = 0.20 \times 2{,}000 = \mathbf{400 \text{ rows}}$$
  $$\text{Training Set Available } (N_{\text{train}}) = 0.80 \times 2{,}000 = 1{,}600 \text{ rows}$$
  * **(b) Row Allocation in 10-Fold CV ($n = 1{,}600, k = 10$):**
  $$\text{Validation Rows per Fold} = \frac{1{,}600}{10} = \mathbf{160 \text{ rows}}$$
  $$\text{Training Rows per Fold} = 1{,}600 - 160 = \mathbf{1{,}440 \text{ rows}} \quad \left(\text{or } \frac{9}{10} \times 1{,}600 = 1{,}440\right)$$
  * **(c) Total Models Trained from Scratch:**
  * 10 surrogate models trained during the cross-validation search ($1$ per fold).
  * 1 final model trained on **$100\%$ of the training data** ($1{,}600$ rows) using the winning hyperparameter.
  * *(The test set is used only to evaluate the final model; it trains zero models).*
  $$\text{Total Models Trained} = 10 + 1 = \mathbf{11 \text{ models}}$$ `[L18, L20]`
  
  ---
### Problem 3 Solution
  * **(a) Row Allocation per Fold ($n = 800, k = 5$):**
  $$\text{Validation Rows} = \frac{800}{5} = \mathbf{160 \text{ rows}}$$
  $$\text{Training Rows} = 800 - 160 = \mathbf{640 \text{ rows}}$$
  * **(b) Total Model Fits:**
  $$\text{Total Fits} = n\_splits \times n\_repeats = 5 \times 10 = \mathbf{50 \text{ model fits}}$$
  * **(c) Scores Generated:**
  * Total out-of-fold scores stored: $\mathbf{50 \text{ scores}}$ (one per fold rotation).
  * Distinct 5-fold mean scores: $\mathbf{10 \text{ mean scores}}$ (one average score per 5-fold repeat, calculated via `scores.reshape(10, 5).mean(axis=1)`). `[L18]`
  
  ---
### Problem 4 Solution
  * **(a) Why Identical Folds Are Impossible:**
  $$n = 503, \quad k = 5 \implies \frac{503}{5} = 100.6$$
  Because sample counts must be discrete integers, $503$ is not evenly divisible by $5$ ($503 = 5 \times 100 + 3$).
  * **(b) Remainder Allocation Across Folds:**
  Scikit-learn distributes the remainder ($3$) by adding $+1$ sample to the first $3$ folds:
  * Fold 1: $100 + 1 = \mathbf{101 \text{ rows}}$
  * Fold 2: $100 + 1 = \mathbf{101 \text{ rows}}$
  * Fold 3: $100 + 1 = \mathbf{101 \text{ rows}}$
  * Fold 4: $\mathbf{100 \text{ rows}}$
  * Fold 5: $\mathbf{100 \text{ rows}}$
  * *Check:* $101 + 101 + 101 + 100 + 100 = 503$.
  * **(c) Training Rows for a Larger Fold:**
  When a fold of size $101$ is held out for validation:
  $$\text{Training Rows} = 503 - 101 = \mathbf{402 \text{ rows}}$$
  *(When a fold of size $100$ is held out, the model trains on $503 - 100 = 403$ rows).* `[L18]`
  
  ---
### Problem 5 Solution
  * **(a) Validation Fold Class Distribution ($n = 1{,}000, k = 5$):**
  `StratifiedKFold` forces each fold to reflect the exact $20\% / 80\%$ population ratio:
  $$\text{Positives per Fold} = \frac{200}{5} = \mathbf{40 \text{ positive instances}}$$
  $$\text{Negatives per Fold} = \frac{800}{5} = \mathbf{160 \text{ negative instances}}$$
  *Total per validation fold:* $40 + 160 = 200 \text{ rows}$.
  * **(b) Training Fold Class Distribution:**
  $$\text{Positives in Training Fold} = 200 - 40 = \mathbf{160 \text{ positive instances}}$$
  $$\text{Negatives in Training Fold} = 800 - 160 = \mathbf{640 \text{ negative instances}}$$
  *Total per training fold:* $160 + 640 = 800 \text{ rows}$.
  * **(c) Unstratified Sorted Breakdown (The Slicing Defect):**
  If rows $1\text{--}800$ are negative and rows $801\text{--}1{,}000$ are positive, slicing into 5 contiguous blocks of 200 rows yields:
  * Folds 1, 2, 3, 4: **$0$ positive cases** ($100\%$ negative). Recall is undefined ($\frac{0}{0}$).
  * Fold 5: **$200$ positive cases** ($100\%$ positive). Zero negative cases. `[L18]`
  
  ---
### Problem 6 Solution
  * **(a) Cartesian Search Space Size ($|\mathcal{G}|$):**
  $$|\mathcal{G}| = |G_C| \times |G_{\text{penalty}}| \times |G_{\text{solver}}| = 6 \times 2 \times 2 = \mathbf{24 \text{ unique configurations}}$$
  * **(b) Model Fits During Search Phase:**
  $$N_{\text{CV\_fits}} = |\mathcal{G}| \times k_{\text{folds}} = 24 \times 5 = \mathbf{120 \text{ model fits}}$$
  * **(c) Grand Total Models Trained:**
  Because `refit=True`, after completing the 120 CV fits and selecting $\lambda^*$, scikit-learn trains **$1$ final model** on all $600$ training instances:
  $$\text{Grand Total Fits} = 120 + 1 = \mathbf{121 \text{ models trained from scratch}}$$ `[L20]`
  
  ---
### Problem 7 Solution
  * **(a) Outer Loop Split ($N = 100, K_{\text{outer}} = 5$):**
  $$\text{Outer Test Fold Size} = \frac{100}{5} = \mathbf{20 \text{ samples}}$$
  $$\text{Outer Training Set Size} = 100 - 20 = \mathbf{80 \text{ samples}}$$
  * **(b) Inner Loop Split ($N_{\text{outer\_train}} = 80, K_{\text{inner}} = 4$):**
  $$\text{Inner Validation Fold Size} = \frac{80}{4} = \mathbf{20 \text{ samples}}$$
  $$\text{Inner Training Fold Size} = 80 - 20 = \mathbf{60 \text{ samples}}$$
  * **(c) Total Model Fits in Nested CV:**
  1. *Inner Tuning Fits:* Across 5 outer folds, each inner loop evaluates 3 hyperparameter settings across 4 inner folds:
     $$\text{Inner Fits} = 5 \times (3 \text{ settings} \times 4 \text{ folds}) = 5 \times 12 = 60 \text{ fits}$$
  2. *Outer Refits:* For each outer fold, the winning setting is refit once on all 80 outer training samples:
     $$\text{Outer Fits} = 5 \times 1 = 5 \text{ fits}$$
  $$\text{Grand Total Fits} = 60 + 5 = \mathbf{65 \text{ model fits}}$$ `[L18, L20]`
  
  ---
### Problem 8 Solution
  * **(a) Row Allocation ($n = 900, k = 3$):**
  $$\text{Validation Rows per Fold} = \frac{900}{3} = \mathbf{300 \text{ rows}}$$
  $$\text{Training Rows per Fold} = 900 - 300 = \mathbf{600 \text{ rows}}$$
  * **(b) Total Model Fits:**
  Scikit-learn fits the model **once per fold rotation**, regardless of how many metrics are requested. The single fitted model generates predictions once, and all four metrics are calculated from those predictions:
  $$\text{Total Fits} = k = \mathbf{3 \text{ model fits}}$$
  * **(c) Score Values Returned:**
  $$4 \text{ metrics} \times 3 \text{ folds} = \mathbf{12 \text{ metric score values}}$$
  *(Stored in keys `test_accuracy`, `test_precision`, `test_recall`, `test_f1`, each containing an array of length 3).* `[L18]`
  
  ---
### Problem 9 Solution
  * **(a) Validation Allocation ($60$ Patients, $n = 600$, $5$ Folds):**
  `GroupKFold` splits by unique groups (patients), not by rows:
  $$\text{Patients per Validation Fold} = \frac{60}{5} = \mathbf{12 \text{ unique patients}}$$
  $$\text{Rows per Validation Fold} = 12 \text{ patients} \times 10 \text{ recordings} = \mathbf{120 \text{ rows}}$$
  * **(b) Training Allocation:**
  $$\text{Patients per Training Fold} = 60 - 12 = \mathbf{48 \text{ unique patients}}$$
  $$\text{Rows per Training Fold} = 48 \text{ patients} \times 10 \text{ recordings} = \mathbf{480 \text{ rows}}$$
  * **(c) Methodological Failure of Standard `KFold`:**
  Standard `KFold` shuffles rows independently. Recordings from the *same patient* would appear in both the training fold and the validation fold (**Group / Entity Leakage**). The model would memorize patient-specific ECG baselines rather than learning generalizable arrhythmia patterns, producing an overly optimistic validation score that fails on new patients. `[L09, L18]`
  
  ---
### Problem 10 Solution
  * **(a) Validation/Test Size per Split ($n = 500, k = 4$):**
  In `TimeSeriesSplit(n_splits=k)`, scikit-learn allocates a fixed test size across all splits:
  $$\text{Test Size} = \left\lfloor \frac{n}{k + 1} \right\rfloor = \left\lfloor \frac{500}{4 + 1} \right\rfloor = \frac{500}{5} = \mathbf{100 \text{ samples}}$$
  * **(b) Training Sample Allocation per Split:**
  * **Split 1:** Trains on days $1\text{--}100$ ($\mathbf{100 \text{ samples}}$) $\to$ tests on days $101\text{--}200$ ($100$ samples)
  * **Split 2:** Trains on days $1\text{--}200$ ($\mathbf{200 \text{ samples}}$) $\to$ tests on days $201\text{--}300$ ($100$ samples)
  * **Split 3:** Trains on days $1\text{--}300$ ($\mathbf{300 \text{ samples}}$) $\to$ tests on days $301\text{--}400$ ($100$ samples)
  * **Split 4:** Trains on days $1\text{--}400$ ($\mathbf{400 \text{ samples}}$) $\to$ tests on days $401\text{--}500$ ($100$ samples)
  * **(c) Why Training Size Expands:**
  Time-series forecasting enforces the **arrow of time** to prevent temporal leakage: training data must strictly precede testing data ($t_{\text{train}} < t_{\text{test}}$). In a walk-forward evaluation, as time advances into the future, historical data accumulates, allowing the training window to expand while holding the forward-looking forecast horizon ($100$ days) constant. `[L09, L18]`
  
  ---