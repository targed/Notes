## Problem C1: Leak Detective (Scenarios a–g)
  
  Slide/Page 3 prompts:
  $$\mathbf{\text{"For each scenario: Leak or No leak? If it leaks, name the type (preprocessing, target, temporal, or test-set reuse) and give a one-line fix."}}$$
  
  ```
                     The Leak Detective Diagnostic Grid
  ┌──────────┬───────────┬──────────────────────────┬────────────────────────────────────────────────────────┐
  │ Scenario │ Verdict   │ Leakage Category         │ Root Cause & Methodological Failure                    │
  ├──────────┼───────────┼──────────────────────────┼────────────────────────────────────────────────────────┤
  │ (a)      │ LEAK      │ Target Leakage           │ cancellation_reason only exists AFTER a customer churns│
  ├──────────┼───────────┼──────────────────────────┼────────────────────────────────────────────────────────┤
  │ (b)      │ LEAK      │ Preprocessing Leakage    │ StandardScaler computes μ, σ across full dataset       │
  ├──────────┼───────────┼──────────────────────────┼────────────────────────────────────────────────────────┤
  │ (c)      │ LEAK      │ Preprocessing/Selection  │ SelectKBest evaluates test labels y_test to rank feats │
  ├──────────┼───────────┼──────────────────────────┼────────────────────────────────────────────────────────┤
  │ (d)      │ LEAK      │ Temporal Leakage         │ Shuffling time series allows future data to predict past│
  ├──────────┼───────────┼──────────────────────────┼────────────────────────────────────────────────────────┤
  │ (e)      │ LEAK      │ Test-Set Reuse (Snooping)│ Tuning hyperparameter k on test set invalidates proxy  │
  ├──────────┼───────────┼──────────────────────────┼────────────────────────────────────────────────────────┤
  │ (f)      │ NO LEAK   │ None (Methodologically   │ Fit strictly on X_train; transform X_train and X_test   │
  │          │           │ Sound)                   │ using frozen training parameters                       │
  ├──────────┼───────────┼──────────────────────────┼────────────────────────────────────────────────────────┤
  │ (g)      │ LEAK      │ Target Leakage           │ Insulin prescribed because diabetes already diagnosed  │
  └──────────┴───────────┴──────────────────────────┴────────────────────────────────────────────────────────┘
  ```
  
  ---
### Detailed Analysis and One-Line Fixes
#### Scenario (a)
  > *To predict which customers will churn next month, the model uses `cancellation_reason` as a feature.*
  
  * **Verdict:** **LEAK**
  * **Leakage Type:** **Target Leakage** (Post-Hoc Causal Proxy)
  * **Underlying Mechanism:** A customer only logs a `cancellation_reason` *after* they have made the decision to terminate their contract. At live prediction time $T_0$, active customers who are at risk of churning have not canceled yet; their `cancellation_reason` is unpopulated/null. The model learns a trivial historical shortcut that does not exist during real-world inference.
  * **One-Line Fix:** 
  ```python
  df = df.drop(columns=['cancellation_reason'])
  ```
  
  ---
#### Scenario (b)
  > `scaled = StandardScaler().fit_transform(X)`, then `train_test_split(scaled, y, test_size=0.2)`.
  
  * **Verdict:** **LEAK**
  * **Leakage Type:** **Preprocessing Leakage** (Distributional Snooping)
  * **Underlying Mechanism:** `StandardScaler.fit()` computes the empirical mean $\mu$ and standard deviation $\sigma$ across all $N$ samples simultaneously. The test set's distribution parameters are baked into the normalized coordinates of the training samples prior to splitting, violating out-of-sample isolation.
  * **One-Line Fix:**
  ```python
  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2); X_train = scaler.fit_transform(X_train); X_test = scaler.transform(X_test)
  ```
  *(Or encapsulate via `make_pipeline(StandardScaler(), model)`).*
  
  ---
#### Scenario (c)
  > `SelectKBest(f_classif, k=10).fit(X, y)` on all 10,000 rows; keep the 10 selected columns; then split and train.
  
  * **Verdict:** **LEAK**
  * **Leakage Type:** **Preprocessing / Feature Selection Leakage**
  * **Underlying Mechanism:** Supervised feature selection evaluates the correlation/ANOVA $F$-statistic between each feature and the target labels across the **entire dataset** ($y_{\text{train}} \cup y_{\text{test}}$). As proven in Lecture 9, this picks random noise features that happen to correlate with test labels by chance, fabricating an illusion of high accuracy.
  * **One-Line Fix:**
  ```python
  X_tr, X_te, y_tr, y_te = train_test_split(X, y, test_size=0.2); X_tr = selector.fit_transform(X_tr, y_tr); X_te = selector.transform(X_te)
  ```
  
  ---
#### Scenario (d)
  > *Daily store sales from 2020–2025 are split with `train_test_split(shuffle=True)` to forecast next week's sales.*
  
  * **Verdict:** **LEAK**
  * **Leakage Type:** **Temporal Leakage** (Arrow-of-Time Inversion)
  * **Underlying Mechanism:** Shuffling longitudinal/time-series data destroys chronological order. The training set is populated with future data points ($t+1$) used to predict historical past events ($t$). In production forecasting, models cannot observe future macro-trends or holiday sales spikes.
  * **One-Line Fix:**
  ```python
  # Split strictly by chronological time horizon without shuffling
  X_train, X_test = X[date < '2025-01-01'], X[date >= '2025-01-01']
  ```
  *(Or use `sklearn.model_selection.TimeSeriesSplit`)*.
  
  ---
#### Scenario (e)
  > *$k$ for $k$-NN is chosen by trying $k = 1, 3, \dots, 31$ and keeping the value with the best TEST accuracy; that accuracy is reported.*
  
  * **Verdict:** **LEAK**
  * **Leakage Type:** **Test-Set Reuse / Meta-Overfitting (Data Snooping)**
  * **Underlying Mechanism:** The test set must be vaulted in cold storage and evaluated **exactly once**. Evaluating multiple hyperparameter choices against the test set and selecting the best one turns the test set into a surrogate validation set. The final model overfits to the specific noise profile of the test set, producing an overly optimistic estimate of generalization error.
  * **One-Line Fix:**
  ```python
  # Tune k using cross-validation strictly on the training set, then score test once
  grid = GridSearchCV(KNeighborsClassifier(), {'n_neighbors': range(1, 32, 2)}, cv=5).fit(X_train, y_train)
  ```
  
  ---
#### Scenario (f)
  > *Split first. The median of age and the one-hot categories are computed from `X_train` and then applied to both `X_train` and `X_test`.*
  
  * **Verdict:** **NO LEAK**
  * **Leakage Type:** None (Methodologically Sound)
  * **Underlying Mechanism:** This represents the gold standard of data hygiene (Lectures 7–9). The raw data is partitioned first; all state parameters (medians, category vocabularies) are learned strictly from `X_train` via `.fit()`, and then applied to `X_test` via frozen `.transform()`. Zero test information enters the feature pipeline.
  * **One-Line Fix:** 
  $$\text{N/A (Pipeline is already leak-free)}$$
  
  ---
#### Scenario (g)
  > *A hospital model predicts whether a patient will be diagnosed with diabetes; one feature is `on_insulin_prescription`.*
  
  * **Verdict:** **LEAK**
  * **Leakage Type:** **Target Leakage** (Downstream Clinical Action / Proxy Trap)
  * **Underlying Mechanism:** Clinicians prescribe insulin *because* they have already identified and diagnosed diabetes. Including this downstream treatment intervention means the model is predicting historical physician actions rather than future physiological disease onset. For undiagnosed patients in production screening, this feature will be unpopulated.
  * **One-Line Fix:**
  ```python
  df = df.drop(columns=['on_insulin_prescription'])
  ```
  
  ---
## Problem C2: Batch, Mini-Batch, or Stochastic?
  
  Slide/Page 4 prompts:
  $$\mathbf{\text{"The training set has } n = 10{,}000 \text{ samples. For each item, which type of gradient descent is used: batch, mini-batch, or stochastic?"}}$$
  
  ```
  ┌──────────────────────────────────────┬────────────────────────┬──────────────────────────────────────────┐
  │ Item Code / Description              │ Gradient Regime        │ Operational & Mathematical Justification │
  ├──────────────────────────────────────┼────────────────────────┼──────────────────────────────────────────┤
  │ (a) grad = (2/n) * X.T @ (X @ w - y) │ BATCH Gradient Descent │ Uses all n = 10,000 rows simultaneously  │
  │     w -= lr * grad                   │                        │ to compute the exact population gradient.│
  ├──────────────────────────────────────┼────────────────────────┼──────────────────────────────────────────┤
  │ (b) DataLoader(batch_size=1)         │ STOCHASTIC GD (SGD)    │ batch_size = 1 updates weights after     │
  │     optimizer.step()                 │                        │ every single individual training sample. │
  ├──────────────────────────────────────┼────────────────────────┼──────────────────────────────────────────┤
  │ (c) DataLoader(batch_size=128)       │ MINI-BATCH GD          │ 1 < batch_size < n (B = 128); balances   │
  │     optimizer.step()                 │                        │ variance reduction with GPU parallelism. │
  ├──────────────────────────────────────┼────────────────────────┼──────────────────────────────────────────┤
  │ (d) DataLoader(batch_size=10000)     │ BATCH Gradient Descent │ batch_size = n (10,000); processes the   │
  │                                      │                        │ entire dataset in a single update step.  │
  ├──────────────────────────────────────┼────────────────────────┼──────────────────────────────────────────┤
  │ (e) "Uses random subset of 256"      │ MINI-BATCH GD          │ Evaluates a subset B = 256 samples per   │
  │                                      │                        │ parameter update.                        │
  └──────────────────────────────────────┴────────────────────────┴──────────────────────────────────────────┘
  ```
  
  ---
### In-Depth Analysis of Items (a)–(e)
  
  * **Item (a):**
  ```python
  for it in range(200):
      grad = (2/n) * X.T @ (X @ w - y)  # uses every row
      w -= lr * grad
  ```
  * **Answer:** **Batch Gradient Descent**
  * **Systems Proof:** The matrix multiplication `X.T @ (X @ w - y)` evaluates the full design matrix $X \in \mathbb{R}^{10{,}000 \times d}$. All $10{,}000$ sample residuals are accumulated into a single gradient vector before executing the weight update `w -= lr * grad`.
  
  * **Item (b):**
  ```python
  loader = DataLoader(train_ds, batch_size=1, shuffle=True)
  for xb, yb in loader:
      ...; optimizer.step()
  ```
  * **Answer:** **Stochastic Gradient Descent (Online SGD)**
  * **Systems Proof:** Setting `batch_size=1` means the loop iterates $10{,}000$ times per epoch. The optimizer adjusts parameters after evaluating a single instance.
  
  * **Item (c):**
  ```python
  loader = DataLoader(train_ds, batch_size=128, shuffle=True)
  for xb, yb in loader:
      ...; optimizer.step()
  ```
  * **Answer:** **Mini-Batch Gradient Descent**
  * **Systems Proof:** $B = 128$ is the canonical mini-batch regime ($1 < B < n$). An epoch consists of $\lceil 10{,}000 / 128 \rceil = 79$ parameter updates.
  
  * **Item (d):**
  ```python
  DataLoader(train_ds, batch_size=10000, shuffle=True)
  ```
  * **Answer:** **Batch Gradient Descent**
  * **Systems Proof:** Because the training set contains exactly $n = 10{,}000$ samples, setting `batch_size=10000` evaluates all data in a single batch, executing exactly $1$ update per epoch.
  
  * **Item (e):**
  > *"Each update uses a random subset of 256 training samples."*
  * **Answer:** **Mini-Batch Gradient Descent**
  * **Systems Proof:** Sampling a subset of $B = 256$ samples falls directly into the mini-batch definition ($1 < 256 < 10{,}000$).
  
  ---
### Item (f): Loss Curve Topology Matching
  
  > **Problem Statement:** *Three training-loss curves are plotted against the number of updates:*
  > * *Curve 1 is perfectly smooth and decreasing;*
  > * *Curve 2 is very jagged but trends downward;*
  > * *Curve 3 is slightly noisy and trends downward.*
  > 
  > *Match each curve to batch, stochastic, and mini-batch GD, and explain.*
  
  ```
                       Loss Curve Trajectories (Problem C2f)
    Training Loss
      ▲
      │  \  Curve 2: Very Jagged / High Variance (Stochastic GD, B = 1)
      │   /\  /\  /\
      │  /  \/  \/  \    Curve 3: Slightly Noisy Corridor (Mini-Batch GD, B = 128)
      │ /   /\   /\  \  /~~~\_
      │/   /  \_/  \__\/      \__
      │
      │   Curve 1: Perfectly Smooth Monotonic Descent (Batch GD, B = n)
      │  \
      │   \
      │    ╰──────────────────────────────────────────
      └────────────────────────────────────────────────────────▶ Number of Updates
  ```
#### The Official Matchings & Theoretical Explanations:
  
  1. **Curve 1 = Batch Gradient Descent (BGD)**
   * **Theoretical Justification:** Batch GD evaluates the exact population gradient $\nabla J(w) = \frac{1}{n}\sum_{i=1}^n \nabla \mathcal{L}_i$ over all $10{,}000$ instances. Under a stable learning rate ($\eta < \frac{2}{\lambda_{\max}}$), the cost function on a convex loss surface is **monotonically decreasing** at every update step ($J(w^{(t+1)}) \le J(w^{(t)})$). Because there is zero sampling error, the trajectory exhibits **zero stochastic noise**, producing a perfectly smooth curve.
  2. **Curve 2 = Stochastic Gradient Descent (SGD)**
   * **Theoretical Justification:** SGD updates parameters using a single sample ($B = 1$). The variance of the single-sample gradient estimator is maximal:
     $$\text{Var}(\nabla \mathcal{L}_i) = \Sigma$$
     Individual observations may have conflicting gradients (e.g., mislabeled samples, outliers, or opposing classes). Consequently, steps frequently move in directions counter to the global minimum, producing an **extremely jagged, noisy random-walk trajectory**.
  3. **Curve 3 = Mini-Batch Gradient Descent (MBGD)**
   * **Theoretical Justification:** Mini-batch GD computes the gradient over an intermediate subset of size $B$ (e.g., $B = 128$). By the **Law of Large Numbers**, the variance of the sample mean gradient is compressed by a factor of $B$:
     $$\text{Var}\left( \frac{1}{B}\sum_{i \in \mathcal{B}} \nabla \mathcal{L}_i \right) = \frac{\Sigma}{B}$$
     This variance reduction dampens the erratic noise of SGD while retaining slight stochastic fluctuations compared to Batch GD, resulting in a **slightly noisy, guided downward corridor**.
  
  ---
## Summary Review Checkpoints for Section 3
  
  1. *Why does tuning a hyperparameter (like $k$ in $k$-NN or $\alpha$ in Ridge) directly on the test set invalidate the test set as an unbiased proxy for generalization?*
  2. *Under what operational condition does PyTorch's `DataLoader` execute pure Batch Gradient Descent?*
  3. *If an algorithm encounters an error surface with multiple shallow local minima, why does the jagged loss trajectory of Stochastic/Mini-Batch GD often find a better solution than the smooth trajectory of Batch GD?*
  
  ---