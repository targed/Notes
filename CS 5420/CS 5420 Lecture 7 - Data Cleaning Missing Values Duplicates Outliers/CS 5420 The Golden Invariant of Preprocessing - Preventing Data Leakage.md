## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **The Masking Effect in Classical $Z$-Scores:**
  * Severe outliers dramatically inflate the sample variance $s^2 = \frac{1}{N-1}\sum (x_i - \bar{x})^2$ because deviations are squared.
  * Because the sample standard deviation $s$ sits in the denominator of the $Z$-score formula:
   $$Z_i = \frac{x_i - \bar{x}}{s}$$
   an inflated $s$ suppresses the calculated $Z$-scores of all observations in the dataset. Consequently, true secondary anomalies have their $Z$-scores artificially compressed below the threshold (e.g., $|Z_i| < 3.0$), causing the test to **mask** their presence.
  2. **Asymptotic Gaussian Mapping of Tukey’s 1.5 Multiplier:**
  * For a normal distribution $\mathcal{N}(\mu, \sigma^2)$, the quartiles reside at $Q_1 = \mu - 0.6745\sigma$ and $Q_3 = \mu + 0.6745\sigma$, yielding an interquartile range of $\text{IQR} = 1.349\sigma$.
  * Evaluating the upper fence:
   $$\text{Upper Fence} = Q_3 + 1.5 \cdot \text{IQR} = (\mu + 0.6745\sigma) + 1.5(1.349\sigma) \approx \mathbf{\mu + 2.698\sigma}$$
  * The multiplier $1.5$ corresponds asymptotically to **$\mu \pm 2.7\sigma$**, encompassing $\approx 99.3\%$ of standard Gaussian observations and leaving $\approx 0.7\%$ in the extreme tail fences, while maintaining a high breakdown point ($25\%$) against corrupted data.
  3. **Distance-Free Anomaly Scoring in Isolation Forests:**
  * An Isolation Forest isolates observations by recursively selecting a random feature and a random orthogonal split point between that feature's minimum and maximum values.
  * Because anomalies reside in sparse, low-density regions of the feature space, they require very few random cuts to isolate into an individual leaf node (yielding a **short path length $h(x)$**). Normal points clustered in dense manifolds require many recursive partitions (yielding a **long path length $h(x)$**).
  * The algorithm calculates an exponential anomaly score $s(x, n) = 2^{-\frac{\mathbb{E}[h(x)]}{c(n)}}$ based on tree depth rather than computing computationally prohibitive $\mathcal{O}(N^2)$ pairwise Euclidean distance matrices.
  
  ---
## 2. The Mechanics of Preprocessing Leakage (Slide 9)
  
  Slide 9 outlines the single most common methodological error in applied machine learning:
  
  $$\mathbf{\text{"Common Mistake: Computing the mean/median on the full dataset BEFORE splitting leaks future information."}}$$
  
  ```
                The Preprocessing Leakage Pathology (Slide 9)
                                Full Dataset 𝒟
                     (N_total = N_train + N_test Samples)
                                      │
                     ❌ THE DEADLY METHODOLOGICAL TRAP ❌
                 Global Preprocessing Applied to Entire Dataset:
                 • Global Mean:   μ_global = (1 / N_total) ∑ x_i
                 • Global Median: M_global = Median(x_1, ..., x_N)
                 • Global Impute: Fill NaNs using μ_global
                                      │
                     ┌────────────────┴────────────────┐
                     ▼                                 ▼
             Training Split (80%)              Test Split (20%)
         [Contaminated with μ_global]      [Contaminated with μ_global]
  ```
### The Mathematical Proof of Contamination:
  Let the dataset $\mathcal{D} = \{x_1, \dots, x_N\}$ be partitioned into a training set $\mathcal{D}_{\text{train}}$ of size $N_{\text{tr}}$ and a test set $\mathcal{D}_{\text{test}}$ of size $N_{\text{te}}$.
  
  If a practitioner standardizes features on the full dataset prior to splitting, the empirical mean is calculated across all observations:
  
  $$\hat{\mu}_{\text{global}} = \frac{1}{N_{\text{tr}} + N_{\text{te}}} \left( \sum_{i \in \text{train}} x_i + \sum_{j \in \text{test}} x_j \right)$$
  
  Now, inspect the standardized feature value for a training instance $x_k \in \mathcal{D}_{\text{train}}$:
  
  $$\tilde{x}_k = \frac{x_k - \hat{\mu}_{\text{global}}}{\hat{\sigma}_{\text{global}}} = \frac{x_k - \frac{1}{N} \left( \sum_{i \in \text{train}} x_i + \mathbf{\sum_{j \in \text{test}} x_j} \right)}{\hat{\sigma}_{\text{global}}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Consequence: Every single training feature coordinate x̃_k now       │
  │ contains an explicit mathematical summation over the held-out test set!│
  │                                                                        │
  │ • The training features contain distributional knowledge of the test   │
  │   partition (its center μ_test and dispersion σ_test).                 │
  │ • The test partition is no longer an independent, out-of-sample proxy. │
  │ • Generalization error is artificially suppressed; the model produces  │
  │   overly optimistic metrics that will fail under production shift.     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. The Correct Workflow: The Split-First Rule (Slide 9)
  
  Slide 9 formalizes the absolute engineering invariant of machine learning pipelines:
  
  $$\mathbf{\text{"Split FIRST! Fit every operation on training data only. Apply .transform() to test set."}}$$
  
  ```
                       The Leak-Free Execution Graph
                                Raw Dataset 𝒟
                                      │
                                      ▼
                        train_test_split(𝒟, test_size=0.2)
                                      │
               ┌──────────────────────┴──────────────────────┐
               ▼                                             ▼
       Training Set 𝒟_train                          Test Set 𝒟_test
     (Strict Isolation Barrier)                    (VAULTED IN STORAGE)
               │                                             │
               ▼                                             │
      fit_transform(X_train)                                 │
   • Learn μ_train, σ_train                                  │
   • Compute Median_train                                    │
   • Freeze state parameters θ_prep                          │
               │                                             │
               ▼                                             │
       Train Estimator                                       │
     model.fit(X_train_scaled)                               │
               │                                             │
               ▼                                             ▼
      Validation Complete                            transform(X_test)
                                                • Apply FROZEN parameters:
                                                  (X_test - μ_train) / σ_train
                                                • Impute via Median_train
                                                             │
                                                             ▼
                                                    Generalization Metric
                                                    model.score(X_test_scaled)
  ```
### The State Parameter Invariant:
  1. **The Training Phase (`.fit()` / `.fit_transform()`):**
   State parameters (means $\mu_{\text{tr}}$, variances $\sigma_{\text{tr}}^2$, medians $M_{\text{tr}}$, categorical frequency vocabularies) are learned **strictly and exclusively** from $\mathcal{D}_{\text{train}}$.
  2. **The Evaluation Phase (`.transform()`):**
   The test set $\mathcal{D}_{\text{test}}$ is transformed using the **frozen parameters of the training set**:
   $$X_{\text{test}}^{\text{scaled}} = \frac{X_{\text{test}} - \mu_{\text{train}}}{\sigma_{\text{train}}}$$
   $$\text{Missing } X_{\text{test}} \longleftarrow M_{\text{train}}$$
   **You never invoke `.fit()` on test data.** Computing $\mu_{\text{test}}$ or imputing test values using test medians assumes knowledge of the test set's global distribution, which cannot exist in real-world single-sample production inference.
  
  ---
## 4. Cross-Validation & The Scikit-Learn Pipeline Firewall
  
  A critical vulnerability occurs when applying the "Split First" rule to **$K$-Fold Cross-Validation**.
### The Cross-Validation Leakage Trap:
  ```python
  # ANTI-PATTERN: Cross-Validation Leakage
  scaler = StandardScaler()
  X_scaled = scaler.fit_transform(X_train) # Scaled across all K folds!
  
  # When cross_val_score splits X_scaled into K folds:
  # Fold k's validation split was used to calculate the mean and variance of X_scaled!
  scores = cross_val_score(model, X_scaled, y_train, cv=5)
  ```
  
  In every iteration of the cross-validation loop, the validation fold leaks into the remaining $K-1$ training folds through the shared scaling parameters ($\mu, \sigma$).
  
  ---
### The Architectural Solution: `Pipeline` and `ColumnTransformer`
  
  To guarantee that transformations are fitted strictly within the training folds during cross-validation, scikit-learn encapsulates preprocessing and estimation into a single execution graph via **`sklearn.pipeline.Pipeline`**:
  
  ```python
  from sklearn.model_selection import train_test_split, cross_val_score
  from sklearn.pipeline import Pipeline
  from sklearn.compose import ColumnTransformer
  from sklearn.impute import SimpleImputer
  from sklearn.preprocessing import StandardScaler, OneHotEncoder
  from sklearn.ensemble import RandomForestClassifier
  
  # 1. Physical Separation of the Final Evaluation Hold-Out
  X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.20, random_state=42, stratify=y
  )
  
  # 2. Sub-Pipelines for Heterogeneous Column Types
  num_pipeline = Pipeline([
    ('imputer', SimpleImputer(strategy='median')), # Learns median on train fold
    ('scaler', StandardScaler())                   # Learns μ, σ on train fold
  ])
  
  cat_pipeline = Pipeline([
    ('imputer', SimpleImputer(strategy='most_frequent')),
    ('encoder', OneHotEncoder(handle_unknown='ignore')) # Handles unseen test classes
  ])
  
  # 3. Column-Level Dispatcher
  preprocessor = ColumnTransformer(transformers=[
    ('num', num_pipeline, numeric_features),
    ('cat', cat_pipeline, categorical_features)
  ])
  
  # 4. End-to-End Pipeline Execution Graph
  full_pipeline = Pipeline([
    ('preprocessor', preprocessor),
    ('classifier', RandomForestClassifier(random_state=42))
  ])
  
  # 5. Leak-Free Cross-Validation
  # At every fold split, the Pipeline fits the imputer/scaler strictly on the K-1
  # training folds and applies .transform() to the hold-out validation fold!
  cv_scores = cross_val_score(full_pipeline, X_train, y_train, cv=5, scoring='roc_auc')
  
  # 6. Final Fit and Vaulted Test Evaluation
  full_pipeline.fit(X_train, y_train)
  final_test_score = full_pipeline.score(X_test, y_test)
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Why Pipeline Is Mandatory in Production:                              │
  │ • Encapsulates state parameters inside estimator objects.              │
  │ • Guarantees that .fit() executes only on training folds during CV.   │
  │ • Eliminates human error: calling full_pipeline.predict(X_test)       │
  │   automatically transforms test inputs using frozen training scales.   │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 3
  
  1. *If a test instance contains a feature value $x_{\text{test}}$ that is significantly larger than any value observed in the training set, should you recompute the scaler's maximum or mean to include this observation before transforming it? Why or why not?*
  2. *Why does performing target encoding (replacing categorical strings with the mean of the target variable $y$) outside of a `Pipeline` cause catastrophic data leakage during cross-validation?*
  3. *What is the difference between invoking `pipeline.fit_transform(X_train)` versus invoking `pipeline.transform(X_test)` with respect to the internal state parameters of the transformers?*
  
  ---