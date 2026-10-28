## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why the $k$ Surrogate Models Are Discarded and Refitted on 100% of Training Data:**
   * In $k$-fold cross-validation, the surrogate models are diagnostic artifacts whose purpose is to estimate the generalization performance of the **modeling procedure** and identify the optimal hyperparameters $\lambda^*$.
   * Each surrogate model is trained on only $\frac{k-1}{k}$ of the training data (e.g., $80\%$ of $80\% = 64\%$ of the total dataset). 
   * Once $\lambda^*$ is selected, we discard the $k$ surrogate models and retrain a single final model on **$100\%$ of the training partition** ($N_{\text{train}}$). Training on the full training partition leverages the maximum available statistical power, reducing parameter variance and yielding a more accurate final model.
  2. **Total Number of Models Fitted in 5-Fold CV + Final Evaluation:**
   * **5 models** are trained during cross-validation (one per fold rotation).
   * **1 final model** is trained on the entire training set using the selected hyperparameter.
   * **Total:** Exactly **6 models** are trained from scratch. (The vaulted test set is evaluated against the 6th model; it trains zero models).
  3. **The Mechanism of Meta-Overfitting (Data Snooping):**
   * Even if a model's internal optimization algorithm (e.g., gradient descent) never directly computes gradients on the test set, **the engineer acts as an outer optimization loop**.
   * By iteratively evaluating different models, feature sets, or regularizers against the test set and choosing the one with the highest test score, the engineer selects configurations that exploit the sample-specific noise of that test split. 
   * Under multiple hypothesis testing, spurious correlations in the test partition are rewarded, destroying the test set's independence and yielding an overly optimistic performance estimate that fails in production.
  
  ---
## 2. Practice P1: Cross-Validation with an Encapsulated Pipeline (Slides 8–9)
  
  Slide 9 demonstrates the gold-standard scikit-learn workflow on the Breast Cancer Wisconsin dataset ($N = 569, d = 30$):
  
  ```python
  from sklearn.datasets import load_breast_cancer
  from sklearn.linear_model import LogisticRegression
  from sklearn.model_selection import StratifiedKFold, cross_val_score, train_test_split
  from sklearn.pipeline import make_pipeline
  from sklearn.preprocessing import StandardScaler
  
  # 1. Ingest Data and Flip Label (1 = Malignant, 0 = Benign)
  X, y = load_breast_cancer(return_X_y=True)
  y = 1 - y
  
  # 2. Vault the Test Set (20% Holdout)
  X_tr, X_te, y_tr, y_te = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
  )
  
  # 3. Everything that learns from data goes INSIDE the pipeline
  pipe = make_pipeline(StandardScaler(), LogisticRegression(max_iter=1000))
  
  # 4. Enforce Stratification Across 5 Folds
  cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=0)
  
  # 5. Evaluate Accuracy and Recall via Out-of-Fold Cross-Validation
  scores = cross_val_score(pipe, X_tr, y_tr, cv=cv)
  print(f"Accuracy: {scores.mean():.3f} ± {scores.std():.3f}")
  
  rec = cross_val_score(pipe, X_tr, y_tr, cv=cv, scoring="recall")
  print(f"Recall:   {rec.mean():.3f} ± {rec.std():.3f}")
  ```
  
  ```
                     Inside Each Fold Execution Graph (Slide 9)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ For Fold i ∈ {1, 2, 3, 4, 5}:                                          │
  │                                                                        │
  │   Step A: Slicing (Raw Data)                                           │
  │   • X_tr_fold (4 folds, 364 rows)    • X_val_fold (1 fold, 91 rows)    │
  │   • y_tr_fold (4 folds, 364 labels)  • y_val_fold (1 fold, 91 labels)  │
  │                                                                        │
  │   Step B: Pipeline Fit (Training Folds Only)                           │
  │   • StandardScaler computes μ_fold, σ_fold strictly on X_tr_fold.      │
  │   • X_tr_fold is standardized ──▶ LogisticRegression fits parameters w.│
  │                                                                        │
  │   Step C: Pipeline Transform & Score (Validation Fold Only)            │
  │   • X_val_fold is transformed using FROZEN μ_fold, σ_fold.             │
  │   • Transformed validation rows are scored: s_i = Metric(f̂, X_val_fold)│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 9 emphasizes:
  $$\mathbf{\text{"Pass raw } X\text{; the pipeline refits every step per fold. Pre-processed } X \text{ is the most common leak."}}$$
  
  If an engineer executes `StandardScaler().fit_transform(X_tr)` *before* passing the matrix to `cross_val_score()`, the mean and variance of each validation fold are leaked into the training folds of that rotation. Wrapping the scaler and estimator inside `make_pipeline()` guarantees that feature scaling parameters are computed strictly inside the training folds.
  
  ---
## 3. Practice P2: Single Split vs. Repeated Cross-Validation (Slide 10)
  
  Slide 10 runs an experiment comparing **10 independent single train/test splits** against **Repeated Stratified 5-Fold Cross-Validation** ($10 \times 5$-fold CV = 50 total fits):
  
  ```python
  from sklearn.model_selection import RepeatedStratifiedKFold
  
  # 1. Evaluate 10 Single 80/20 Splits (Varying the Random Seed)
  accs = []
  for s in range(10):
    a, b, c, d = train_test_split(
        X, y, test_size=0.2, random_state=s, stratify=y
    )
    accs.append(pipe.fit(a, c).score(b, d))
  print(f"Single splits range: {min(accs):.3f} - {max(accs):.3f}")
  
  # 2. Evaluate Repeated Stratified CV (10 repeats of 5-fold CV = 50 scores)
  rcv = RepeatedStratifiedKFold(n_splits=5, n_repeats=10, random_state=0)
  scores_50 = cross_val_score(pipe, X, y, cv=rcv)
  
  # Reshape into a (10, 5) matrix: 10 repeats (rows), 5 folds per repeat (columns)
  repeat_means = scores_50.reshape(10, 5).mean(axis=1)
  print(f"10 x 5-fold CV means: {repeat_means.min():.3f} - {repeat_means.max():.3f}")
  ```
  
  ```
                   Empirical Variance Reduction Profile (Slide 10)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Single 80/20 Splits (10 Seeds):                                        │
  │   Range: 0.947 - 0.991  ──▶ (A 4.4-PERCENTAGE-POINT SPREAD!)           │
  │                                                                        │
  │ Repeated 5-Fold CV Means (10 Repeats of 5 Folds):                      │
  │   Range: 0.972 - 0.979  ──▶ (COMPRESSED TO A 0.7-PERCENTAGE-POINT BAND)│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### The Mathematics of Repeated Cross-Validation:
  Why does `RepeatedStratifiedKFold` compress the spread from $4.4\%$ down to $0.7\%$?
  * In a single $k$-fold cross-validation run, the score $\hat{\mu}_{\text{CV}}$ still contains a small amount of **Monte Carlo partition variance** depending on how the initial shuffle divided the folds.
  * Repeating a $5$-fold cross-validation $M = 10$ times with different random seeds generates $M$ independent estimates of the mean: $\bar{s}_1, \bar{s}_2, \dots, \bar{s}_{10}$.
  * Slicing and reshaping via `scores_50.reshape(10, 5).mean(axis=1)` computes the average score of each 5-fold repeat.
  * By averaging out partition noise across $M$ distinct fold assignments, repeated cross-validation provides an extremely stable estimate of model performance, which is valuable when comparing two closely performing architectures.
  
  ---
## 4. Practice P3: Intentional Leakage Lab ($p \gg n$) (Slide 11)
  
  Slide 11 proves the catastrophic impact of data leakage using a synthetic dataset composed **entirely of random Gaussian white noise**:
  
  ```python
  import numpy as np
  from sklearn.feature_selection import SelectKBest, f_classif
  from sklearn.linear_model import LogisticRegression
  from sklearn.model_selection import StratifiedKFold, cross_val_score
  from sklearn.pipeline import make_pipeline
  
  # Generate pure random Gaussian noise (Zero real relationship to y!)
  rng = np.random.RandomState(0)
  X = rng.randn(100, 5000)  # 100 samples, 5,000 noise features (p >> n)
  y = np.array([0, 1] * 50)
  rng.shuffle(y)  # Completely random binary labels
  
  cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=0)
  lr = LogisticRegression(max_iter=1000)
  
  # FLAGGED PIPELINE: Supervised Feature Selection BEFORE Cross-Validation
  X20 = SelectKBest(f_classif, k=20).fit_transform(X, y)
  leaky_score = cross_val_score(lr, X20, y, cv=cv).mean()
  print(f"Leaky CV Score:  {leaky_score:.3f}")
  
  # HONEST PIPELINE: Feature Selection INSIDE the Pipeline
  pipe = make_pipeline(SelectKBest(f_classif, k=20), lr)
  honest_score = cross_val_score(pipe, X, y, cv=cv).mean()
  print(f"Honest CV Score: {honest_score:.3f}")
  ```
  
  ```
                   Output of the Intentional Leak (Slide 11)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Leaky CV Score  : 0.890 (89.0% Accuracy on Pure White Noise!)          │
  │ Honest CV Score : 0.490 (49.0% Accuracy — Matches Random Coin Flip)    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Deconstructing the Leakage Mechanism:
  
  ```
               The Anatomy of Pre-CV Selection Contamination
  Raw Noise Matrix X (100 × 5,000)                Target Vector y (100 Labels)
  ┌────────────────────────────────────────┐     ┌────────────────────────────┐
  │ Fold 1 (Validation Rows: 20 samples)   │     │ y_val (Validation Labels)  │
  │                                        │ and │                            │
  │ Folds 2–5 (Training Rows: 80 samples)  │     │ y_train (Training Labels)  │
  └──────────────────┬─────────────────────┘     └─────────────┬──────────────┘
                   │                                         │
                   └────────────────────┬────────────────────┘
                                        ▼
                         SelectKBest.fit_transform(X, y)
    Evaluates correlation of all 5,000 noise features against y_val!
    Selects 20 features that match validation labels by random chance.
                                        │
                                        ▼
                           cross_val_score(lr, X20, y)
    SURPRISE! When Fold 1 is held out, the model achieves 89% accuracy
    because Fold 1's labels were already used to choose the features!
  ```
  
  Slide 11 delivers the concluding insight:
  
  $$\mathbf{\text{"The gap is the size of the leak. Selection leaks a lot; scaling leaks a little."}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Why Does Selection Leak "A Lot" While Scaling Leaks "A Little"?        │
  │                                                                        │
  │ 1. Why Scaling Leaks "A Little":                                       │
  │    • StandardScaler is UNSUPERVISED: it computes μ and σ strictly on   │
  │      features X, ignoring target labels y.                             │
  │    • Leaking test feature means slightly shifts coordinate origins,    │
  │      but does not give the model direct information about class labels.│
  │    • On Breast Cancer, scaling before splitting shifts accuracy by     │
  │      only a fraction of a percent (e.g., 96.5% vs. 96.4%).             │
  │                                                                        │
  │ 2. Why Supervised Selection Leaks "A Lot":                             │
  │    • SelectKBest is SUPERVISED: it queries the target labels y.        │
  │    • In high dimensions (p >> n, 5,000 features vs. 100 rows), random   │
  │      chance guarantees that dozens of features will correlate with y.  │
  │    • Pre-split selection uses validation labels to pick those spurious  │
  │      correlations, fabricating an 89% accuracy out of pure noise.      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## Complete Lecture 18 Synthesis Reference
  
  | Core Cross-Validation Concept | Mathematical / Systems Mechanism | Production Impact / Diagnostic Safeguard |
  | :--- | :--- | :--- |
  | **The Single-Split Lottery** | Hold-out scores fluctuate widely ($\approx 7\%$ swing) due to sample partitioning noise. | Never rely on a single 80/20 split on small datasets ($N < 5{,}000$). Use $k$-fold cross-validation. |
  | **$k$-Fold Estimator** | $\text{CV} = \frac{1}{k}\sum_{i=1}^k s_i$; averages across $k$ rotating surrogate models. | Compresses variance and provides an unbiased estimate of generalization error. |
  | **Variance Reporting** | Report mean $\pm$ standard deviation ($\hat{\mu}_{\text{CV}} \pm \hat{\sigma}_{\text{CV}}$). | Measures algorithmic stability; distinguishes stable models from those sensitive to sample perturbations. |
  | **Pessimistic Bias** | Surrogate models train on $\frac{k-1}{k}N$ rows instead of full sample size $N$. | Small $k$ (e.g., $k=2$) underestimates model capability; $k=5$ or $k=10$ strikes the optimal balance. |
  | **Leave-One-Out (LOOCV)** | $k = n$; fits $N$ models on $N-1$ rows with binary fold scores ($s_i \in \{0, 1\}$). | Zero pessimistic bias, but computationally heavy ($\mathcal{O}(n)$ fits) and high variance under $0\text{–}1$ loss. |
  | **Stratified $k$-Fold** | Preserves class proportions in each fold: $\frac{N_{i, c}}{N_i} \approx \frac{N_c}{N}$. | Mandatory for classification; prevents zero-positive folds where Recall becomes undefined ($0/0$). |
  | **The Full Protocol** | Cross-validation is for choosing/tuning; the test set is vaulted for the final score. | Refit the winning model on **$100\%$ of training data** before final evaluation on the vaulted test set. |
  | **Test-Set Snooping** | Iterating model decisions against test scores causes meta-overfitting. | Contaminates the test set via human optimization; invalidates test scores as out-of-sample proxies. |
  | **Nested Cross-Validation** | Outer loop evaluates generalization; inner loop tunes hyperparameters. | Prevents optimization bias on small sample cohorts ($N < 100$) where a separate test set cannot be spared. |
  | **Pipeline Encapsulation** | `make_pipeline(Transformer(), Estimator())` passed to `cross_val_score`. | Re-fits all preprocessing transformations strictly inside training folds, eliminating data leakage. |
  | **Leakage Disparity** | Supervised selection leaks a lot; unsupervised scaling leaks a little. | Pre-split selection fabricates $90\%$ accuracy out of pure white noise by mining target label correlations. |
  
  ---
## Summary Review Questions for Section 4
  
  1. *Why does executing `cross_val_score(pipe, X_tr, y_tr)` with `pipe = make_pipeline(StandardScaler(), LogisticRegression())` prevent preprocessing data leakage across cross-validation folds?*
  2. *In Practice P2, why does evaluating 10 distinct 5-fold cross-validation averages (`RepeatedStratifiedKFold`) reduce variance compared to running 10 single 80/20 train/test splits?*
  3. *Why does running `SelectKBest` on raw Gaussian white noise before cross-validation yield an $89\%$ accuracy score, whereas running it inside an `sklearn.pipeline.Pipeline` yields approximately $50\%$ accuracy?*
  
  ---