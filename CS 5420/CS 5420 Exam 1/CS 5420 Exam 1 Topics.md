# Module 1 & 2: Foundations, Environments & Scientific Reproducibility (Lectures 1–4)
### 1. Paradigm Shift & Mathematical Framing
  * **Sequence Transducers vs. Active Policies:** From passive conditional language modeling $P(x_{n+1} \mid x_{\le n})$ to closed-loop agents interacting with environments.
  * **The POMDP Formulation:** True state $\mathcal{S}$, action space $\mathcal{A}$, observation space $\mathcal{O}$, transition probability $\mathcal{T}$, and context trajectory buffer $h_t = (o_0, a_0, \dots, o_t)$.
  * **Core ML Taxonomy:** 
  * Supervised learning (pairs $(x, y) \sim P(X, Y)$; continuous regression vs. categorical classification).
  * Unsupervised learning (data $\{x\} \sim P(X)$; clustering, manifold projection, density estimation; no external oracle $y$).
  * Reinforcement learning (delayed reward $r_t$, policy $\pi(a \mid s)$, credit assignment, exploration vs. exploitation).
  * **The Three Learning Styles:**
  * Hand-engineered features ($\phi(x)$) + shallow convex models (high inductive bias, low sample complexity).
  * End-to-end representation learning ($\phi(x; \theta)$) + backpropagation (weak inductive bias, high sample complexity).
### 2. Generalization Theory & The Bias-Variance Trade-Off
  * **Expected Risk vs. Empirical Risk:**
  $$R(f) = \mathbb{E}_{(x, y) \sim \mathcal{D}}[\mathcal{L}(y, f(x))] \quad \text{vs.} \quad \hat{R}_N(f) = \frac{1}{N}\sum_{i=1}^N \mathcal{L}(y_i, f(x_i))$$
  * **The Generalization Gap:** $\text{gap}(f) = |R(f) - \hat{R}_N(f)|$; why training loss is trivially gameable via memorization.
  * **Underfitting vs. Overfitting:**
  * High Train Loss, High Test Loss $\implies$ Underfitting (High Bias, restricted hypothesis space $\mathcal{H}$).
  * Low Train Loss, High Test Loss $\implies$ Overfitting (High Variance, capacity interpolates sample noise).
  * **Bias-Variance Decomposition Derivation (under Squared Error):**
  $$\mathbb{E}_{\mathcal{D}, \epsilon}\left[(y - \hat{f}(x))^2\right] = \underbrace{\left(f(x) - \mathbb{E}[\hat{f}(x)]\right)^2}_{\text{Bias}^2} + \underbrace{\mathbb{E}\left[\left(\hat{f}(x) - \mathbb{E}[\hat{f}(x)]\right)^2\right]}_{\text{Variance}} + \underbrace{\sigma^2}_{\text{Irreducible Noise}}$$
  * **The Irreducible Error Bound ($\sigma^2$):** Aleatoric uncertainty inherent in the data-generating process; cannot be reduced by any model.
### 3. Scientific Reproducibility & The ACM Triad
  * **The ACM Artifact Definitions:**
  * **Repeatability:** Same experimental team, same measurement setup/code, same machine $\implies$ same result.
  * **Reproducibility:** *Different* team (e.g., TA/auditor), same code and data on an independent system $\implies$ same result.
  * **Replicability:** Different team, *different* implementation/new data $\implies$ same scientific conclusion.
  * **Sources of Irreproducibility:**
  * Software randomness (decoupled PRNG state vectors across Python, NumPy, PyTorch CPU, PyTorch CUDA).
  * Dependency drift (unpinned `pip` packages, silent Colab base container image updates).
  * Interactive kernel memory persistence (hidden-state traps, out-of-order bracket execution `In [ ]`).
  * **The Global Seeding Protocol:**
  ```python
  def seed_all(s=42):
      random.seed(s)
      np.random.seed(s)
      torch.manual_seed(s)
      torch.cuda.manual_seed_all(s)
  ```
  * **Hardware Non-Determinism (IEEE 754):** Why floating-point non-associativity ($(a+b)+c \neq a+(b+c)$) in parallel GPU atomic adds (`atomicAdd`) causes bitwise numerical drift across runs.
### 4. Computational Runtimes & Hardware Architectures
  * **CPU vs. GPU Microarchitecture:**
  * CPU: Latency-optimized, few heavy cores, complex branch predictors, large caches; excels at branching, trees, sequential data parsing.
  * GPU: Throughput-optimized, thousands of small ALUs executing Single Instruction, Multiple Threads (SIMT); excels at dense GEMM matrix operations.
  * **CUDA Asynchronous Dispatch & Benchmarking:**
  * GPU kernel dispatches are non-blocking to the CPU thread.
  * Explicit barriers (`torch.cuda.synchronize()`) are mandatory before and after timers to measure actual work rather than queue latency.
  * **The Hardware Crossover Frontier:**
  * When $N < 256$, CPUs outperform GPUs due to kernel launch overhead ($\approx 5\text{–}10\,\mu\text{s}$) and L1/L2 cache locality.
  * When $N > 512$, GPUs dominate ($30\times\text{–}70\times$ faster) by saturating streaming multiprocessors with parallel matrix operations.
  
  ---
# Module 3 & 4: Data Structures, Preprocessing & Exploratory Auditing (Lectures 5–7)
### 1. Vectorized Computing: NumPy & Pandas Architecture
  * **NumPy `ndarray` Internal Layout:** Data pointer to contiguous unboxed C buffer, `dtype` descriptor, `shape` tuple, and `strides` tuple.
  * **Strides & $\mathcal{O}(1)$ Transformations:**
  * Transposing (`A.T`) or reshaping alters only `shape` and `strides` metadata without copying underlying bytes.
  * Memory order: Row-major C-contiguous (`order='C'`) vs. Column-major Fortran (`order='F'`).
  * **Memory Precision:** `float64` (8 bytes) vs. `float32` (4 bytes); halving precision doubles cache-line occupancy and SIMD register capacity.
  * **Why Python Loops Are Slow:** Dynamic type checking per iteration, object boxing/unboxing overhead (`PyFloatObject` = 24 bytes), pointer indirection, cache misses, and the Global Interpreter Lock (GIL).
  * **NumPy Broadcasting Rules:**
  1. Align shapes starting from the rightmost (trailing) dimensions.
  2. Dimensions are compatible if they are equal, or if one of them is $1$.
  3. Prepend leading singleton dimensions to the shorter rank array.
  * **The Axis Reduction Invariant:** *"An axis argument names the axis that disappears."*
  * `X.sum(axis=0)` on shape `(N, d)` collapses rows $\implies$ returns `(d,)` (column summaries).
  * `X.sum(axis=1)` collapses columns $\implies$ returns `(N,)` (row summaries).
  * **Pandas Indexing Semantics:**
  * `.iloc[]`: Integer-positional, half-open interval $[start, stop)$ (**stop is EXCLUSIVE**).
  * `.loc[]`: Label-based, closed interval $[start, stop]$ (**stop is INCLUSIVE**).
  * Boolean vector logic: Bitwise operators (`&`, `|`, `~`) with explicit parentheses are mandatory; Python's `and`/`or` fail with ambiguous truth value errors.
  * Avoiding `SettingWithCopyWarning`: Use unified 2D assignment (`df.loc[mask, 'col'] = val`) instead of chained indexing (`df[mask]['col'] = val`).
### 2. Exploratory Data Analysis (EDA) & The Ingestion Protocol
  * **The Pre-Modeling Contract:**
  * Read data dictionaries first: distinguish **Identifiers** (drop immediately to prevent trivial memorization), **Features**, and **Targets**.
  * The 5-minute diagnostic suite: `.shape`, `.head()`, `.info()`, `.describe()`, `.isna().sum()`, `.value_counts()`.
  * **Limits of Parametric Moments (Anscombe's Quartet & Datasaurus Dozen):**
  * First- and second-order moments ($\mu, \sigma, \rho$) are sufficient statistics *only* for unimodal Gaussian distributions.
  * Radically different topologies (clean lines, quadratic curves, extreme outliers, multimodal clusters) can share identical summary statistics.
### 3. Outlier Detection & The Deletion Fallacy
  * **$Z$-Score (Parametric):**
  * $Z_i = \frac{x_i - \bar{x}}{s}$; assumes Gaussian distribution.
  * Masking effect: Extreme outliers inflate $s$ quadratically, shrinking their own $Z$-scores below the threshold.
  * Robust alternative: Modified $Z$-score using Median Absolute Deviation ($\text{MAD} = \text{median}(|x_i - \tilde{x}|)$):
    $$M_i = \frac{0.6745(x_i - \tilde{x})}{\text{MAD}}$$
  * **Tukey’s IQR Rule (Non-Parametric):**
  * $\text{IQR} = Q_3 - Q_1$; acceptance bounds: $[Q_1 - 1.5 \cdot \text{IQR}, \; Q_3 + 1.5 \cdot \text{IQR}]$.
  * The $1.5$ multiplier asymptotically corresponds to $\mu \pm 2.7\sigma$ under a Gaussian distribution, offering a $25\%$ breakdown point.
  * **Isolation Forest (High-Dimensional Space Partitioning):**
  * Recursively isolates points using random orthogonal cuts.
  * Anomalies are "few and different" and isolate near the root (short average path length $h(x) \to 0$, anomaly score $s \to 1.0$).
  * **The Deletion Fallacy:** Never delete outliers reflexively. Differentiate corrupted measurement errors (delete/correct) from genuine heavy-tailed phenomena (fraud, equipment failure; retain and use robust estimators).
### 4. Missing Data Mechanisms (Rubin’s Taxonomy)
  * **MCAR (Missing Completely at Random):** Missingness is entirely independent of both observed and unobserved data ($P(M \mid Y_{\text{obs}}, Y_{\text{mis}}) = P(M)$); global imputation is valid.
  * **MAR (Missing at Random):** Missingness depends on observed features, but not on the missing value itself ($P(M \mid Y_{\text{obs}}, Y_{\text{mis}}) = P(M \mid Y_{\text{obs}})$); conditional/grouped imputation is valid.
  * **MNAR (Missing Not at Random):** Missingness depends on the unobserved value itself ($P(M \mid Y_{\text{obs}}, Y_{\text{mis}}) \neq P(M \mid Y_{\text{obs}})$); requires adding explicit missingness indicators (`age_was_missing`).
  
  ---
# Module 4: Feature Engineering, Scaling & The Leakage Firewall (Lectures 8–9)
### 1. The Geometry of Feature Scaling
  * **Why Scale Features?**
  * Distance-based models ($k$-NN, $k$-Means, SVM-RBF): Large-scale features quadratically dominate Euclidean metrics ($\|x - x'\|_2^2 = \sum (x_j - x_j')^2$).
  * Gradient descent optimization: Unscaled features produce ill-conditioned, elongated Hessian canyons ($\kappa = \frac{\lambda_{\max}}{\lambda_{\min}} \gg 1$), causing erratic oscillations.
  * Regularization: $L_1/L_2$ penalties shrink weights uniformly; unscaled features with small magnitudes require large weights and get unfairly over-penalized.
  * **Tree Invariance:** Decision trees and tree ensembles are mathematically **invariant** to strictly monotonic feature scaling ($x_j \le \theta \iff g(x_j) \le g(\theta)$).
  * **The Four Scalers:**
  * `StandardScaler`: $z = \frac{x - \mu}{\sigma}$; zero mean, unit variance. **Changes scale, not shape!** (Skewness $\gamma_1$ is invariant to positive affine transforms).
  * `MinMaxScaler`: $x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$; compact $[0, 1]$ interval. Extreme outliers compress inliers into near-zero bounds. Does not clip out-of-bounds test points by default.
  * `RobustScaler`: $x_{\text{robust}} = \frac{x - \text{median}}{\text{IQR}}$; order-statistic scaling; high breakdown point against extreme outliers.
  * `PowerTransformer` (Box-Cox for $x > 0$, Yeo-Johnson for all real $x$): Non-linear parametric warps that actively reshape skewed distributions into Gaussian distributions.
### 2. Categorical Representations
  * **The Categorical Spectrum:**
  * Binary: $K = 2 \implies$ Single $0/1$ indicator.
  * Nominal: Unordered categories $\implies$ One-Hot Encoding (OHE).
  * Ordinal: Ordered categories $\implies$ Integer rank encoding preserving order ($S < M < L \to 0, 1, 2$).
  * High-Cardinality: $K \gg 100 \implies$ Target encoding with Bayesian smoothing, feature hashing, or frequency encoding.
  * **The Nominal Integer Fallacy:** Mapping nominal categories to arbitrary integers ($0, 1, 2, \dots$) imposes false numerical ordering and distance spacing, distorting linear models and nearest-neighbor rankings.
  * **The Dummy Variable Trap:** When an intercept is included, summing all $K$ one-hot columns equals $\mathbf{1}$, creating perfect multicollinearity ($\det(X^TX) = 0$). Drop one column (`drop='first'`) for unregularized OLS; keep all $K$ for regularized models.
  * **The Production Parameter:** `OneHotEncoder(handle_unknown='ignore')` maps novel test categories to all-zero vectors ($[0, 0, \dots, 0]$), preventing pipeline crashes.
### 3. Feature Engineering Techniques
  * **Cyclical Trigonometric Projections:**
  * Linear integer encodings of periodic cycles (hour 0–23, month 1–12) create an artificial step discontinuity across the boundary ($|23 - 0| = 23$).
  * Mapping to the 2D unit circle restores smooth periodic metric distance:
    $$x_{\sin} = \sin\left(\frac{2\pi t}{T}\right), \quad x_{\cos} = \cos\left(\frac{2\pi t}{T}\right)$$
  * Both sine and cosine are required to resolve phase collisions ($\sin(\theta) = \sin(\pi - \theta)$).
  * **Multiplicative Cross-Product Interactions:**
  * Additive models ($\beta_1 x_1 + \beta_2 x_2$) enforce parallel regression slopes across subsets.
  * Multiplicative terms ($\beta_3(x_1 x_2)$) make the marginal slope of $x_1$ a dynamic linear function of $x_2$:
    $$\frac{\partial y}{\partial x_1} = \beta_1 + \beta_3 x_2$$
### 4. Dimensionality Reduction: Selection vs. Extraction
  * **Feature Selection:** Pruning an index subset ($\mathcal{S} \subset \{1, \dots, d\}$); preserves physical units and interpretability.
  * *Filter Methods:* Model-agnostic univariate ranking ($\chi^2$, ANOVA $F$-test, Mutual Information); linear $\mathcal{O}(d)$ speed, but blind to multi-feature interactions (e.g., XOR).
  * *Wrapper Methods:* Model-dependent search (Recursive Feature Elimination - RFE); captures interactions, but computationally expensive ($\mathcal{O}(d^2)$).
  * *Embedded Methods:* In-model regularization ($L_1$ Lasso sparsity, tree impurity decrease).
  * **Feature Extraction:** Constructing new synthetic coordinate manifolds ($Z = \Phi(X)$; PCA, SVD, Autoencoders); maximizes variance/information retention, but destroys interpretability.
### 5. The Formal Data Leakage Taxonomy
  * **Definition:** Conditioning the training distribution on information that will not exist at prediction time ($P(Y \mid \mathcal{I}_{\text{leaky}}) \neq P(Y \mid \mathcal{I}_{T_0})$).
  * **The Four Leakage Categories:**
  1. *Preprocessing Leakage:* Fitting scalers, imputers, normalizers, or vocabularies across the full dataset prior to splitting.
  2. *Target Leakage:* Including features that are causal consequences or post-hoc artifacts of the outcome (e.g., `cancellation_reason` for churn, `body_id` for Titanic, `collections_assigned`).
  3. *Temporal Leakage:* Shuffling longitudinal/time-series data, using future states ($t+1$) to predict past states ($t$).
  4. *Group / Entity Leakage:* Placing correlated observations from the same entity/patient into both train and test splits.
  5. *Test-Set Reuse / Meta-Overfitting:* Repeatedly tuning hyperparameters ($k$, $\alpha$, $\tau$) against test-set scores.
  * **The $p \gg n$ Selection Simulation:** Running supervised feature selection (`SelectKBest`) before splitting uses test labels to pick spurious random noise features, fabricating an artificial $99\%$ AUC on pure white noise that collapses to $50\%$ in production.
  * **The Structural Solution:** Split first, vault the test set, and encapsulate transformations inside `sklearn.pipeline.Pipeline` and `ColumnTransformer`.
  
  ---
# Module 5: Parametric Linear Models, Optimization & Regularization (Lectures 10–12)
### 1. Ordinary Least Squares (OLS) & Normal Equations
  * **Linear Hypothesis:** $\hat{y} = w^T \tilde{x}$; homogeneous coordinate trick appends a column of ones ($\mathbf{1}$) to absorb the scalar intercept $w_0$ into $X \in \mathbb{R}^{n \times (d+1)}$.
  * **"Linear in Parameters, Not Features":** Linearity requires $\frac{\partial f(x; w)}{\partial w_j} = \phi_j(x)$; accommodates polynomial, spline, and interaction expansions.
  * **Loss Function Duality ($L_2$ vs. $L_1$):**
  * $L_2$ Squared Error: Smooth, $C^\infty$ differentiable everywhere, fits the **conditional mean** $\mathbb{E}[Y \mid X]$, equivalent to **MLE under Gaussian noise**. Penalties grow quadratically ($30^2 = 900$), making it vulnerable to high-leverage outliers.
  * $L_1$ Absolute Error: Non-differentiable kink at $r = 0$, fits the **conditional median** $\text{Median}(Y \mid X)$, equivalent to **MLE under Laplace noise**. Robust to outliers.
  * **The Normal Equations Derivation:**
  $$J(w) = (y - Xw)^T (y - Xw) = y^Ty - 2w^T X^Ty + w^T X^T Xw$$
  $$\nabla_w J(w) = -2X^Ty + 2X^TXw = \mathbf{0} \implies \mathbf{X^TXw = X^Ty} \implies \mathbf{w^* = (X^TX)^{-1}X^Ty}$$
  * **Geometric Orthogonality of OLS:**
  * "Normal" means perpendicular ($\perp$): the residual vector $r = y - Xw^*$ is orthogonal to the column space of the design matrix: $X^T r = \mathbf{0}$.
  * The Hat (Projection) Matrix: $H = X(X^TX)^{-1}X^T$; symmetric ($H^T = H$) and idempotent ($H^2 = H$). Residual projector: $r = (I - H)y$.
  * Trace Theorem: $\text{Tr}(H) = \text{rank}(X) = d+1$.
  * Zero-Sum Invariant: The intercept forces residuals to sum to zero on the training set ($\sum r_i = 0 \implies \bar{r} = 0$).
  * Centroid Passing Property: The fitted line passes through the data centroid: $w_0 = \bar{y} - w_1 \bar{x} \implies \hat{y}(\bar{x}) = \bar{y}$.
  * **Numerical Solvers:**
  * `np.linalg.inv(X.T@X) @ X.T@y` is an anti-pattern ($\approx 2m^3$ FLOPs, ill-conditioned).
  * `np.linalg.solve(X.T@X, X.T@y)` uses LU decomposition with partial pivoting ($\approx \frac{2}{3}m^3$ FLOPs, $3\times$ faster, stable).
  * `scipy.linalg.lstsq(X, y)` uses SVD ($X = U\Sigma V^T$) to compute the Moore-Penrose pseudoinverse ($w^+ = X^+ y$); survives singular/rank-deficient matrices by returning the minimum-norm solution.
### 2. First-Order Optimization: Gradient Descent Regimes
  * **Update Step:** $w^{(t+1)} = w^{(t)} - \eta \nabla J(w^{(t)})$. The negative gradient $-\nabla J(w)$ is the direction of steepest descent.
  * **Convergence Bounds:**
  * Gradient descent on OLS converges if and only if:
    $$0 < \eta < \frac{2}{\lambda_{\max}(H)} = \frac{2}{\frac{2}{n}\lambda_{\max}(X^T X)}$$
  * If $\eta \ge \frac{2}{\lambda_{\max}}$, updates oscillate with expanding amplitude and diverge to `NaN`.
  * If $\eta \approx 0$, updates stall in a slow linear crawl.
  * **The Three Gradient Regimes:**
  * **Batch GD ($B = n$):** Computes exact gradient over all $n$ samples. Exactly 1 update per epoch. Smooth monotonic loss curve; memory-bound on large datasets.
  * **Stochastic GD ($B = 1$):** Computes gradient on 1 random sample. Exactly $n$ updates per epoch. Unbiased, but high variance ($\text{Var} = \Sigma$). Jagged random-walk trajectory; escapes shallow non-convex minima, but under-utilizes GPU parallel hardware and requires learning rate decay to settle into a minimum.
  * **Mini-Batch GD ($32 \le B \le 512$):** Updates parameters over small random subsets. Exactly $\lceil n / B \rceil$ updates per epoch. Compresses variance by factor of $B$ ($\text{Var} = \frac{\Sigma}{B}$). Smooth-noisy guided corridor; maximizes GPU SIMT/GEMM throughput.
### 3. Regularization Theory: Ridge ($L_2$) vs. Lasso ($L_1$)
  * **Why Regularize?** When $d \approx n$ or $d > n$, unconstrained OLS interpolates training noise, resulting in parameter explosion and high variance ($\text{Var}(\hat{w}) \to \infty$). Regularization adds a complexity penalty $\Omega(w)$ to balance the bias-variance trade-off.
  * **Ridge Regression ($L_2$ Penalty / Tikhonov):**
  $$J(w) = \|y - Xw\|_2^2 + \alpha \|w\|_2^2 \implies \mathbf{w_{\text{Ridge}}^* = (X^T X + \alpha I)^{-1} X^T y}$$
  * Spectral shift: Eigenvalues shift uniformly by $+\alpha$ ($\lambda_i + \alpha \ge \alpha > 0$). Guarantees strict positive definiteness and invertibility even if $d \gg n$ or features are collinear.
  * SVD shrinkage: Components are scaled by $f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha}$. High-variance directions are preserved ($f_j \approx 1$); low-variance collinear noise directions are suppressed ($f_j \to 0$).
  * Smooth decay: Weights shrink toward zero asymptotically, but **never reach zero identically** (no feature selection).
  * **Lasso Regression ($L_1$ Penalty / Tibshirani 1996):**
  $$J(w) = \|y - Xw\|_2^2 + \alpha \|w\|_1$$
  * No closed-form matrix solution due to non-differentiable kink at $w_j = 0$; solved iteratively via Cyclic Coordinate Descent.
  * Soft-Thresholding Operator:
    $$w_j^* = \mathcal{S}_\alpha(\rho_j) = \text{sign}(\rho_j) \max(0, \; |\rho_j| - \alpha)$$
    If the partial residual correlation is weaker than the threshold ($|\rho_j| \le \alpha$), the parameter is **snapped strictly to zero** (exact sparsity / automatic feature selection).
  * **Dueling Geometries:**
  * $L_2$ constraint ($w_1^2 + w_2^2 \le c$) is a smooth circle; expanding loss ellipses touch at generic non-axis tangent points (both weights non-zero).
  * $L_1$ constraint ($|w_1| + |w_2| \le c$) is a diamond with sharp vertices on the coordinate axes; expanding loss ellipses hit vertices first, forcing parameters to exact zero.
  * **Tuning $\alpha$:**
  * Must evaluate on validation sets via cross-validation (`RidgeCV`, `LassoCV`), **never on training error** (training error is monotonically increasing in $\alpha$ and always picks $\alpha = 0$).
  * Use log-spaced grids (`np.logspace(-4, 4, 50)`) to evaluate geometric orders of magnitude.
  * Features must be standardized prior to fitting because $L_1/L_2$ penalties lack scale invariance.
### 4. Interpreting Regression Coefficients & Causation Pathologies
  * **Reading Coefficients Correctly:**
  $$w_j = \frac{\partial \, \mathbb{E}[Y \mid X]}{\partial x_j}$$
  The predicted change in $y$ per unit increase in $x_j$, **holding all other features fixed (*ceteris paribus*)**.
  * **Rates vs. Importance:** $w_0$ shifts the plane vertically; $w_j$ pivots the plane. A coefficient is an exchange rate dependent on physical units, not an intrinsic importance score.
  * **The Four Negations:** A coefficient is NOT a causal effect, NOT an importance score on its own, NOT stable under collinearity, and NOT meaningful if the fit is bad.
  * **Omitted Variable Bias (OVB) Formula:**
  $$\tilde{\beta}_1 = \beta_1 + \beta_2 \cdot \gamma_{21}, \quad \text{where } \gamma_{21} = \frac{\text{Cov}(x_1, x_2)}{\text{Var}(x_1)}$$
  Omitting a correlated predictor $x_2$ causes the short regression slope $\tilde{\beta}_1$ to absorb the indirect effect of $x_2$, which can inflate, deflate, or flip the algebraic sign of the coefficient.
  * **Frisch-Waugh-Lovell (FWL) Theorem:** $w_j$ is the simple bivariate slope obtained by regressing the residualized target $\tilde{y} = (I - H_{-j})y$ on the residualized feature $\tilde{x}_j = (I - H_{-j})x_j$.
  * **Variance Inflation Factor (VIF):** $\text{VIF}_j = \frac{1}{1 - R_j^2}$; measures parameter variance inflation due to multicollinearity ($\text{VIF} > 5$ indicates instability).
  * **Correlation $\neq$ Causation DAG Structures (Pearl):**
  1. *Confounding (The Fork: $X \leftarrow Z \to Y$):* Common cause induces non-causal association; resolved by conditioning on $Z$.
  2. *Reverse Causation ($X \leftarrow Y$):* Outcome causes feature.
  3. *Selection Bias (The Collider: $X \to [S] \leftarrow Y$):* Conditioning on a common effect/collider opens a spurious association between marginally independent variables (Berkson’s Fallacy).
  * **Simpson’s Paradox:** A statistical association present in every subgroup can reverse direction in the aggregate when subgroups are pooled (e.g., UC Berkeley admissions). Settling the paradox requires specifying the causal DAG (confounder vs. mediator).
  * **Legitimate Causal Language:** Permissible *only* under Randomized Controlled Trials (RCTs), natural experiments / instrumental variables (IV), or explicit structural causal models. Otherwise, use "is associated with," never "causes" or "leads to."
  * **Residual Diagnostics:**
  * Funnel/fan shape $\implies$ Heteroscedasticity (non-constant variance $\sigma_i^2$; fixes: $\ln(y)$, WLS, White robust standard errors).
  * Parabolic curvature $\implies$ Functional misspecification (missing non-linear/polynomial terms).
  * Q-Q plot heavy tails $\implies$ Non-normal errors (invalidates small-sample $p$-values and confidence intervals).
  * Cook’s Distance:
    $$D_i = \frac{(r_i^*)^2}{d+1}\left(\frac{h_{ii}}{1 - h_{ii}}\right)$$
    Combines target outlierness ($r_i^*$) with feature leverage ($h_{ii} = x_i^T(X^TX)^{-1}x_i$) to identify influential observations pulling the hyperplane ($D_i > 4/n$).
  
  ---
# Module 6: Classification, Decision Surfaces, $k$-NN & Naive Bayes (Lectures 13–15)
### 1. Decision Boundaries & Logistic Regression
  * **Hyperplane Boundary Geometry:**
  * Decision boundary is the locus of points where $P(Y=1 \mid x) = 0.50 \iff \sigma(w^T x + b) = 0.50 \iff \mathbf{w^T x + b = 0}$.
  * $w$ is the normal vector pointing into the positive Class 1 half-space.
  * Signed Euclidean distance from query $x$ to boundary: $d_\perp(x) = \frac{w^T x + b}{\|w\|_2}$.
  * **The Sigmoid / Logistic Function:**
  $$\sigma(z) = \frac{1}{1 + e^{-z}} \in (0, 1), \quad \text{with derivative } \mathbf{\sigma'(z) = \sigma(z)(1 - \sigma(z))}$$
  * **Logit Link & Odds Representation:**
  $$\text{Odds} = \frac{p}{1 - p} = e^z, \quad \text{Logit}(p) = \ln\left(\frac{p}{1 - p}\right) = w^T x + b$$
  Logistic regression is a Generalized Linear Model that is linear in the log-odds; the probability curve is non-linear, but the decision boundary in feature space is a flat linear hyperplane.
  * **Binary Cross-Entropy (BCE) Loss:**
  $$J(w) = -\frac{1}{n}\sum_{i=1}^n \Big[ y_i \ln \hat{p}_i + (1 - y_i) \ln(1 - \hat{p}_i) \Big]$$
  Equivalent to the **Negative Log-Likelihood of a Bernoulli process**. Confident errors incur asymptotically infinite penalties.
  * **The Failure of Squared Error (MSE) in Classification:**
  * Loss flattens into a plateau at $1.0$ when the model is confidently wrong ($y=1, z \to -\infty$).
  * Gradient contains the vanishing dampener $\hat{p}(1 - \hat{p})$:
    $$\frac{\partial \mathcal{L}_{\text{MSE}}}{\partial z} = 2(\hat{p} - y)\hat{p}(1 - \hat{p}) \longrightarrow \mathbf{0.0} \quad \text{as } z \to -\infty$$
    The worst mistakes receive the weakest corrective update ("the gradient goes silent").
  * BCE derivative cancels the sigmoid dampener, providing a steep, linear restoring force:
    $$\frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z} = \hat{p} - y \longrightarrow \mathbf{-1.0} \quad \text{as } z \to -\infty$$
  * Convexity: The Hessian of BCE is positive semi-definite everywhere ($H_{\text{BCE}} = \frac{1}{n} X^T D X \succeq 0$, where $D_{ii} = \hat{p}_i(1-\hat{p}_i) > 0$), guaranteeing zero non-optimal local minima. MSE under sigmoid is non-convex.
  * **Unified Gradient & Lack of Closed Form:**
  * Gradient: $\nabla_w J(w) = \frac{1}{n} X^T (\hat{p} - y)$ (same matrix shape as linear regression).
  * Setting gradient to zero yields the transcendental equation $X^T \sigma(Xw) = X^T y$; parameters cannot be isolated algebraically. Must solve iteratively via gradient descent, Newton-Raphson / IRLS, or L-BFGS.
  * **Multiclass Softmax Extension:**
  $$P(Y = k \mid x) = \frac{e^{w_k^T x + b_k}}{\sum_{j=1}^K e^{w_j^T x + b_j}}$$
  Pairwise decision boundaries ($z_j = z_k$) are linear hyperplanes: $(w_j - w_k)^T x + (b_j - b_k) = 0$. Partitions multiclass space into a convex Voronoi-like tessellation of piecewise linear boundaries.
### 2. $k$-Nearest Neighbors ($k$-NN) Classifier
  * **Non-Parametric Instance-Based / Lazy Learning:** Training is free ($\mathcal{O}(1)$ time; stores dataset); prediction is memory-bound and computationally heavy ($\mathcal{O}(n \cdot d)$ distance evaluations per query point).
  * **Bias-Variance as a Function of $k$:**
  * Effective degrees of freedom: $\text{DoF} \approx \frac{n}{k}$.
  * $k = 1$: Maximum capacity, low bias, extreme variance, Voronoi polygonal boundary, training error $= 0.0$, sensitive to noise.
  * $k \to n$: Zero capacity, high bias, zero variance, predicts global majority class.
  * Odd $k$ values eliminate voting deadlocks in binary classification.
  * **The Cover-Hart Theorem (1967):**
  $$\lim_{n \to \infty} R_{1\text{NN}} \le 2 R^* (1 - R^*) \le 2 R^*$$
  The asymptotic error rate of a $1$-NN classifier is bounded by at most twice the irreducible Bayes optimal error rate $R^*$.
  * **Minkowski Distance Metric Family:**
  $$D_p(x, y) = \left( \sum_{j=1}^d |x_j - y_j|^p \right)^{1/p}$$
  * $p = 1$: Manhattan ($L_1$, taxicab grid, diamond unit ball).
  * $p = 2$: Euclidean ($L_2$, straight-line, rotationally invariant circular unit ball).
  * $p \to \infty$: Chebyshev ($L_\infty$, maximum coordinate deviation, square unit ball).
  * "Nearest" depends on the metric: changing $p$ re-orders neighbor rankings.
  * **The Curse of Dimensionality in Metric Spaces:**
  * *Empty Space Phenomenon:* To capture volume fraction $r$ in $d$ dimensions, hypercube edge length is $e = r^{1/d}$. In $d = 50$, capturing $10\%$ of data requires spanning $e = (0.10)^{1/50} \approx 95.5\%$ of every coordinate axis (neighborhoods are no longer local).
  * *Distance Concentration (Beyer et al., 1999):* As $d \to \infty$, $\frac{D_{\max} - D_{\min}}{D_{\min}} \to 0$. Relative contrast vanishes; all points become equidistant, rendering Euclidean distance uninformative.
  * Feature scaling is mandatory: unscaled features allow variables with large ranges to dominate Euclidean distance.
### 3. Naive Bayes Classifier (Generative)
  * **Generative vs. Discriminative:** Models the joint probability $P(X, Y) = P(X \mid Y)P(Y)$ and inverts via Bayes' Rule:
  $$P(Y = c \mid X) = \frac{P(X \mid Y = c) P(Y = c)}{P(X)}$$
  * **The "Naive" Class-Conditional Independence Assumption:**
  $$P(X_1, \dots, X_d \mid Y = c) \equiv \prod_{j=1}^d P(X_j \mid Y = c)$$
  Features are assumed mutually independent *given the class label*. Reduces parameter complexity from $\mathcal{O}(2^d)$ to $\mathcal{O}(d \cdot C)$.
  * **MAP Decision Rule (in Log-Space):**
  $$y^* = \arg\max_c \left[ \ln P(Y = c) + \sum_{j=1}^d \ln P(X_j = x_j \mid Y = c) \right]$$
  * **The Zero-Frequency Problem & Laplace Smoothing:**
  * If a feature value never appears with a class in training ($N_{jc} = 0$), the likelihood is $0.0$, wiping out the entire product.
  * Laplace (Add-$\alpha$) Smoothing:
    $$\hat{P}(X_j = v \mid Y = c) = \frac{N_{jc} + \alpha}{N_c + \alpha K_j}$$
    where $K_j$ is the cardinality of feature $j$, and $\alpha = 1$ adds virtual pseudocounts.
  * **Why Naive Bayes Works Despite Correlated Features (Domingos & Pazzani 1997):**
  * Under **$0-1$ classification loss**, only the $\arg\max$ class decision matters, not probability calibration.
  * Correlated features bias posterior probabilities toward $0.0$ and $1.0$, but the correct class ranking is preserved as long as the true odds ratio stays on the correct side of the decision threshold.
  * **Gaussian Naive Bayes (`GaussianNB`):** Evaluates continuous features via univariate normal densities $P(X_j \mid Y = c) \sim \mathcal{N}(\mu_{jc}, \sigma_{jc}^2)$; mathematically invariant to linear feature scaling.
  
  ---
# Cross-Cutting Mathematical Computations & Formulas
  
  Review and commit these computational procedures to memory for hand calculation questions:
  
  ```
  ┌──────────────────────────────────────┬────────────────────────────────────────────────────────────────────────┐
  │ Competency / Technique               │ Mathematical Formula / Step-by-Step Execution                          │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ 1D OLS Slope (w₁)                    │ w₁ = ∑(x_i - x̄)(y_i - ȳ) / ∑(x_i - x̄)² = Cov(x, y) / Var(x)            │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ 1D OLS Intercept (w₀)                │ w₀ = ȳ - w₁x̄  (Guarantees line passes through centroid (x̄, ȳ))        │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Normal Equations                     │ w* = (XᵀX)⁻¹ Xᵀy  (Derived via ∇_w J(w) = -2Xᵀ(y - Xw) = 0)            │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Ridge Normal Equations               │ w_Ridge = (XᵀX + αI)⁻¹ Xᵀy  (Guarantees positive definite inverse)     │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ 1 Batch GD Update Step               │ w^(1) = w^(0) - η · (2/n) Xᵀ(X w^(0) - y)                              │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ 1 SGD Update Step (sample i)         │ w^(1) = w^(0) - η · 2 x_i (x_iᵀ w^(0) - y_i)                           │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Logistic Logit Score                 │ z = wᵀx + b = ∑ w_j x_j + b                                            │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Logistic Probability                 │ p = σ(z) = 1 / (1 + e⁻ᶻ) = Odds / (1 + Odds)                           │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Odds and Log-Odds                    │ Odds = p / (1 - p) = eᶻ;  Log-Odds = ln(Odds) = z                      │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Decision Boundary Equation           │ Set z = 0 ──▶ wᵀx + b = 0 ──▶ x₂ = (-w₁/w₂)x₁ - (b/w₂)                 │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Softmax Probability                  │ p̂_k = e^(z_k) / ∑ e^(z_j)  (Shift-invariant: softmax(z + c) = softmax)│
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Accuracy                             │ (TP + TN) / (TP + FP + FN + TN)                                        │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Precision (PPV)                      │ TP / (TP + FP)  (Penalizes False Alarms / Type I errors)               │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Recall / Sensitivity (TPR)           │ TP / (TP + FN)  (Penalizes Missed Positives / Type II errors)          │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Specificity (TNR)                    │ TN / (TN + FP)  (Inlier protection rate)                               │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ False Positive Rate (FPR)            │ FP / (TN + FP) = 1 - Specificity                                       │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ F₁-Score                             │ 2 · (Precision · Recall) / (Precision + Recall) = 2TP / (2TP + FP + FN)│
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Balanced Accuracy                    │ (Sensitivity + Specificity) / 2                                        │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Negative Predictive Value (NPV)      │ TN / (TN + FN)                                                         │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ StandardScaler                       │ z = (x - μ) / σ  where σ = √( (1/N) ∑(x_i - μ)² ) (ddof = 0 in sklearn)│
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ MinMaxScaler (Default [0, 1])        │ x' = (x - x_min) / (x_max - x_min)                                     │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ MinMaxScaler (Custom [a, b])         │ x' = a + [ (x - x_min) / (x_max - x_min) ] · (b - a)                  │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ RobustScaler                         │ x_robust = (x - Median) / IQR  where IQR = Q₃ - Q₁                     │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Naive Bayes Posterior (Binary)       │ P(Y=1|x) = P(Y=1, x) / [ P(Y=1, x) + P(Y=0, x) ]                       │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Laplace Smoothing                    │ P̂(X_j = v | Y = c) = (N_jc + 1) / (N_c + K_j)                          │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Omitted Variable Bias (OVB)          │ β̃₁ = β₁ + β₂ · [ Cov(x₁, x₂) / Var(x₁) ]                               │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Variance Inflation Factor (VIF)      │ VIF_j = 1 / (1 - R_j²)                                                 │
  ├──────────────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Linear Gradient Updates per Epoch    │ Updates/Epoch = ⌈N / B⌉;  Total Updates = (N / B) · Epochs             │
  └──────────────────────────────────────┴────────────────────────────────────────────────────────────────────────┘
  ```