## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why Arbitrary Integer Encoding on Nominal Data Harms Linear Estimators:**
  * A linear model evaluates predictions through an inner product: $\hat{y} = \sum_{j=1}^d w_j x_j + b$.
  * If nominal categories (e.g., `city` $\in \{\text{Austin: 1, Boston: 2, Chicago: 3}\}$) are mapped to integers, the model is mathematically forced to assume:
   $$\frac{\partial \hat{y}}{\partial x_{\text{city}}} = w_j = \text{Constant}$$
  * This imposes a false metric topology: it forces the step from Austin to Boston ($\Delta = 1$) to have the exact same predictive impact as the step from Boston to Chicago ($\Delta = 1$), while asserting that Chicago is three times "larger" than Austin. Linear models cannot bend this axis independently for each city without separate orthogonal columns.
  2. **The Dummy Variable Trap in Ordinary Least Squares:**
  * When a nominal category with $K$ levels is one-hot encoded into $K$ binary vectors $v_1, \dots, v_K$, their row-wise sum is identically equal to the vector of ones:
   $$\sum_{k=1}^K v_k = \mathbf{1} = [1, 1, \dots, 1]^T$$
  * In an unregularized linear model with a bias/intercept term $w_0 \mathbf{1}$, the design matrix $X$ contains an exact linear combination ($v_K = \mathbf{1} - \sum_{k=1}^{K-1} v_k$). 
  * Consequently, the Gram matrix $X^T X$ is singular (determinant is zero) and cannot be inverted to compute the closed-form OLS solution $\hat{\beta} = (X^T X)^{-1} X^T y$. Dropping one category (`drop='first'`) restores full column rank.
  3. **Behavior of `handle_unknown='error'` vs. `handle_unknown='ignore'` in Production:**
  * **`handle_unknown='error'` (Default):** If real-time inference receives an input containing a novel category not present during training, the encoder raises an unhandled `ValueError`, crashing the active web service or batch pipeline.
  * **`handle_unknown='ignore'`:** Encodes the unseen category as an **all-zero binary vector** ($[0, 0, \dots, 0]$). The instance simply receives no contribution from that categorical feature's weights, allowing inference to complete without pipeline interruption.
  
  ---
## 2. The Scaler Rule: The Cardinal Invariant of Pipeline Hygiene (Slide 5)
  
  Slide 5 formalizes the single most heavily weighted rubric requirement across all assignments in this course:
  
  $$\mathbf{\text{"The test set's mean and standard deviation are information from the future."}}$$
  
  ```
                       The Scaler Rule (Slide 5)
  ┌────────────────────────────────────────────────────────────────────────┐
  │  Golden Rule:                                                          │
  │  Compute scaler.fit() STRICTLY on the training set.                    │
  │  Apply scaler.transform() to both training and test sets.              │
  ├────────────────────────────────────────────────────────────────────────┤
  │  CORRECT EXECUTION:                                                    │
  │    scaler.fit(X_train)                                                 │
  │    X_train_scaled = scaler.transform(X_train)                          │
  │    X_test_scaled  = scaler.transform(X_test)                           │
  ├────────────────────────────────────────────────────────────────────────┤
  │  FATAL ANTI-PATTERN (AUTOMATIC ZERO CREDIT):                           │
  │    scaler.fit_transform(X)  <-- BEFORE TRAIN/TEST SPLIT!               │
  │                                                                        │
  │  Why? Computing scaler.fit() across all data leaks test distribution   │
  │  parameters (μ_test, σ_test) into the training feature coordinates.    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Dataset Ingestion & Feature Slicing (Slides 10–11)
  
  Slides 10 and 11 initiate the live-coding relay on the OpenML Titanic benchmark:
  
  ```python
  import pandas as pd
  from sklearn.datasets import fetch_openml
  from sklearn.model_selection import train_test_split
  from sklearn.preprocessing import StandardScaler, OneHotEncoder
  from sklearn.compose import ColumnTransformer
  from sklearn.linear_model import LogisticRegression
  from sklearn.metrics import accuracy_score
  
  # Ingest complete OpenML Titanic manifest (N = 1,309)
  titanic = fetch_openml("titanic", version=1, as_frame=True)
  df = titanic.frame
  ```
  
  Slide 11 extracts a clean, five-feature design matrix $X$ and binary target vector $y$:
  
  ```python
  # Feature subspace isolation
  X = df[["age", "fare", "sex", "embarked", "pclass"]]
  y = df["survived"].astype(int)
  ```
  
  ```
                     Feature Space Metadata & Typing (Slide 11)
  ┌──────────┬──────────────┬──────────────┬─────────────────────────────────────────┐
  │ Feature  │ Domain Role  │ Data Type    │ Category Mapping / Value Spectrum       │
  ├──────────┼──────────────┼──────────────┼─────────────────────────────────────────┤
  │ age      │ Continuous   │ float64      │ Physical age in years (contains NaNs)   │
  │ fare     │ Continuous   │ float64      │ Ticket price paid (contains 1 NaN)      │
  │ sex      │ Binary       │ category     │ {'female', 'male'}                      │
  │ embarked │ Nominal      │ category     │ {'C': Cherbourg, 'Q': Queenstown,       │
  │          │              │              │  'S': Southampton}                      │
  │ pclass   │ Ordinal/Cat  │ int64        │ {1: First Class, 2: Second, 3: Third}   │
  ├──────────┼──────────────┼──────────────┼─────────────────────────────────────────┤
  │ survived │ Target (y)   │ int64        │ {0: Perished, 1: Survived}              │
  └──────────┴──────────────┴──────────────┴─────────────────────────────────────────┘
  ```
  
  **Methodological Note:** By explicitly selecting these five features, Slide 11 discards identifiers (`name`, `ticket`) that would trigger memorization, as well as downstream target proxies (`boat`, `body`) that would introduce target leakage.
  
  ---
## 4. Dataset Partitioning (Slide 12)
  
  Slide 12 enforces the strict separation barrier:
  
  ```python
  X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
  )
  
  print("Training set:", X_train.shape)  # (1047, 5)
  print("Test set:    ", X_test.shape)   # (262, 5)
  ```
  
  ```
                    The Train/Test Partition Boundary
                         Full Dataset: N = 1,309
  ┌────────────────────────────────────────────────────────┬───────────────────┐
  │                 Training Set (80%)                     │  Test Set (20%)   │
  │                N_train = 1,047 Samples                 │ N_test = 262 Samp │
  └────────────────────────────────────────────────────────┴───────────────────┘
  ```
  
  ---
## 5. The `ColumnTransformer` Architecture (Slide 13)
  
  Slide 13 introduces the industry-standard pattern for applying disparate preprocessing routines to heterogeneous feature subsets:
  
  ```python
  num = ["age", "fare"]
  cat = ["sex", "embarked", "pclass"]
  
  ct = ColumnTransformer([
    ("num", StandardScaler(), num),
    ("cat", OneHotEncoder(handle_unknown="ignore"), cat)
  ])
  
  # Fit strictly on the training partition
  ct.fit(X_train)
  
  # Transform both partitions using frozen parameters
  X_train_processed = ct.transform(X_train)
  X_test_processed  = ct.transform(X_test)
  
  print("Original features: ", X_train.shape[1])            # 5
  print("Processed features:", X_train_processed.shape[1])  # Expanded dimension!
  ```
  
  ```
                   ColumnTransformer Execution Graph (Slide 13)
                                 Input Matrix X (5 Columns)
                                             │
                     ┌───────────────────────┴───────────────────────┐
                     ▼ [num]                                         ▼ [cat]
            ['age', 'fare']                            ['sex', 'embarked', 'pclass']
                     │                                               │
                     ▼                                               ▼
            StandardScaler()                        OneHotEncoder(handle_unknown='ignore')
      • Learns μ_num, σ_num on Train                 • Learns active levels on Train
      • Computes z = (x - μ) / σ                     • Expands to binary indicator vectors
                     │                                               │
                     ▼ (2 Columns)                                   ▼ (8 to 10 Columns)
            [num__age, num__fare]                           [cat__sex_*, cat__emb_*, cat__pclass_*]
                     │                                               │
                     └───────────────────────┬───────────────────────┘
                                             │
                                             ▼ Column-Wise Concatenation
                               Processed Design Matrix (10 to 12 Columns)
  ```
  
  ---
### Calculating the Dimension Expansion ($5 \longrightarrow 10+$ Features)
  Slide 13 shows feature expansion: `X_train.shape[1]` goes from **$5$ original columns** to **$10$ (or more) processed columns**. Why?
  
  1. **Numerical Stream (`num`):**
   * Output width = **$2$ columns** (`age`, `fare`).
  2. **Categorical Stream (`cat`):**
   * `sex` has 2 levels: `'female'`, `'male'` $\implies \mathbf{2 \text{ columns}}$.
   * `embarked` has 3 levels: `'C'`, `'Q'`, `'S'` (plus optional missing category handling) $\implies \mathbf{3 \text{ columns}}$.
   * `pclass` has 3 levels: `1`, `2`, `3` $\implies \mathbf{3 \text{ columns}}$.
   * Total Categorical Columns: $2 + 3 + 3 = \mathbf{8 \text{ binary columns}}$.
  3. **Total Concatenated Width:**
   $$\text{Total Processed Width} = 2 \text{ (numerical)} + 8 \text{ (categorical)} = \mathbf{10 \text{ features}}$$
  
  ---
## 6. Estimator Training & Feature Introspection (Slide 14)
  
  Slide 14 inspects the expanded feature names and optimizes a Logistic Regression classifier:
  
  ```python
  # Extract the auto-generated column names from the ColumnTransformer
  feature_names = ct.get_feature_names_out()
  print("Processed Feature Names:\n", feature_names)
  
  # Instantiate and fit Logistic Regression
  model = LogisticRegression(max_iter=1000)
  model.fit(
    X_train_processed,
    y_train
  )
  ```
### Deconstructing the Output of `ct.get_feature_names_out()`:
  ```python
  [
    'num__age',
    'num__fare',
    'cat__sex_female',
    'cat__sex_male',
    'cat__embarked_C',
    'cat__embarked_Q',
    'cat__embarked_S',
    'cat__pclass_1',
    'cat__pclass_2',
    'cat__pclass_3'
  ]
  ```
### Why `max_iter=1000` Matters in Practice:
  * The default optimization budget in scikit-learn's `LogisticRegression` is `max_iter=100`.
  * The underlying numerical optimizer (L-BFGS) approximates the inverse Hessian via iterative rank-2 updates:
  $$H_{k+1}^{-1} = (I - \rho_k s_k y_k^T) H_k^{-1} (I - \rho_k y_k s_k^T) + \rho_k s_k s_k^T$$
  * When one-hot features introduce sparsity or correlated subsets, the optimization landscape requires more iterations to satisfy the gradient tolerance ($\|\nabla J(w)\|_\infty < \text{tol}$). Increasing `max_iter=1000` prevents premature termination and eliminates `ConvergenceWarning` alerts.
  
  ---
## 7. Generalization Evaluation (Slide 15)
  
  Slide 15 evaluates the model against the held-out test split:
  
  ```python
  y_pred = model.predict(X_test_processed)
  accuracy = accuracy_score(y_test, y_pred)
  print("Test Accuracy:", round(accuracy, 3))
  ```
  
  ```
                   Production Evaluation Metrics
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Empirical Generalization Accuracy : ~0.782 to 0.798 (78.2% - 79.8%)    │
  │ Baseline Majority Predictor        : ~0.616 (61.6% - Always Perished)   │
  │ Net Predictive Value Over Baseline : +17.0% Absolute Generalization    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  The resulting model demonstrates real predictive signal, outperforming the majority-class baseline by **$+17\%$** while adhering to strict data hygiene:
  1. Categorical variables were mapped without imposing false ordinal distances.
  2. Continuous variables were centered and standardized without leaking test statistics.
  3. The evaluation was computed on unseen instances passed through frozen training parameters.
  
  ---
## Summary Review Questions for Section 4
  
  1. *Why does the design matrix output by `ColumnTransformer` expand from 5 columns to 10 columns, and what would happen to that width if an unseen embarkation port appeared in the test set under `handle_unknown='ignore'`?*
  2. *Why does scikit-learn's `LogisticRegression` require setting `max_iter=1000` when trained on one-hot encoded matrices, whereas simple low-dimensional datasets often converge in under 100 iterations?*
  3. *If you deploy this trained pipeline to a live web server to score a single incoming passenger ($N=1$), why is having pre-computed training parameters ($\mu_{\text{train}}, \sigma_{\text{train}}$) structurally necessary for `ct.transform()` to work?*
  
  ---
## Complete Lecture 8 Synthesis Reference
  
  | Topic | Mathematical & Algorithmic Principle | Systems Failure Mode & Prevention |
  | :--- | :--- | :--- |
  | **Why Scale?** | Euclidean distances ($\|x - x'\|_2$) scale quadratically with numerical magnitude. | Large-variance features dominate $k$-NN, $k$-Means, and SVM-RBF kernels. Always rescale. |
  | **Hessian Curvature** | Loss surface condition number $\kappa = \lambda_{\max}/\lambda_{\min}$. | Unscaled features cause severe gradient descent zig-zagging. Scaling restores spherical contours. |
  | **Regularization Bias** | Weight magnitudes scale inversely with feature scale ($w_j \propto 1/\text{Scale}$). | $L_1/L_2$ penalties over-penalize small-scale features. Standardization ensures fair shrinkage. |
  | **Tree Indifference** | Greedy splits maximize $\Delta I(j, \theta)$ based strictly on sample rank order. | Decision trees, Random Forests, and Gradient Boosted Trees are **invariant** to monotonic scaling. |
  | **StandardScaler** | Z-score standardization: $z = \frac{x - \mu}{\sigma}$. Changes scale, not shape! | Does not remove skewness or bound data. Vulnerable to outlier corruption. |
  | **MinMaxScaler** | Affine compression to a compact domain: $x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$. | Outliers compress normal inliers into an unusable narrow band near zero. |
  | **RobustScaler** | Order-statistic scaling: $x_{\text{robust}} = \frac{x - \text{Median}}{\text{IQR}}$. | Essential for heavy-tailed data; offers high breakdown point against extreme anomalies. |
  | **PowerTransformer** | Non-linear warps (Box-Cox, Yeo-Johnson) to enforce Gaussianity. | Resolves heteroscedasticity and heavy skewness before linear modeling. |
  | **The Scaler Rule** | Strict temporal separation: compute `.fit()` on train, apply `.transform()` to test. | Fitting globally before splitting leaks future distribution parameters ($\mu_{\text{te}}, \sigma_{\text{te}}$). |
  | **Categorical Types** | Nominal (no order), Ordinal (strict order), Binary (2 levels), High-Cardinality ($K \gg 100$). | Never assign arbitrary integers to nominal data; forces false arithmetic relationships. |
  | **One-Hot Encoding** | Maps nominal levels to standard orthonormal basis vectors ($e_k \in \mathbb{R}^K$). | Use `handle_unknown='ignore'` to output all-zero vectors on unseen production levels. |
  | **ColumnTransformer** | Encapsulates heterogeneous feature pipelines into a single execution graph. | Automatically manages dimension expansion and coordinates frozen transformations. |
  
  ---