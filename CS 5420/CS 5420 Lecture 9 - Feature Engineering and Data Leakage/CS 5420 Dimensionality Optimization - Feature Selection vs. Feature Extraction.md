## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Why Projecting `month` onto $[\sin(2\pi m / 12), \cos(2\pi m / 12)]$ Resolves Seasonal Discontinuity:**
  * In a linear 1D integer encoding ($m \in \{1, \dots, 12\}$), December ($12$) and January ($1$) have a numerical distance of $|12 - 1| = 11$, creating an artificial step discontinuity.
  * Mapping $m$ onto the unit circle $\mathbb{S}^1$ via trigonometric coordinates yields:
   $$\|v(12) - v(1)\|_2 = \sqrt{\left(\sin\left(\frac{24\pi}{12}\right) - \sin\left(\frac{2\pi}{12}\right)\right)^2 + \left(\cos\left(\frac{24\pi}{12}\right) - \cos\left(\frac{2\pi}{12}\right)\right)^2}$$
   $$\|v(12) - v(1)\|_2 = \sqrt{(0 - 0.5)^2 + (1.0 - 0.866)^2} = \sqrt{0.25 + 0.0179} \approx \mathbf{0.5176}$$
  * The distance between January ($m=1$) and February ($m=2$) evaluates to the exact same value ($\approx 0.5176$). The boundary jump is mathematically eliminated, enabling linear models and distance metrics to process seasonal cycles continuously.
  2. **Why Sine Alone Is Insufficient (Phase Ambiguity):**
  * The sine function is symmetric about $\pi/2$ over the interval $[0, \pi]$: $\sin(\theta) = \sin(\pi - \theta)$.
  * For a 24-hour cycle, $\sin\left(\frac{2\pi \cdot 2}{24}\right) = \sin\left(\frac{\pi}{6}\right) = 0.50$ and $\sin\left(\frac{2\pi \cdot 10}{24}\right) = \sin\left(\frac{5\pi}{6}\right) = 0.50$.
  * Using sine alone causes **02:00 AM** and **10:00 AM** to map to the identical 1D scalar coordinate, creating an unresolvable collision. Cosine provides the orthogonal quadrature coordinate ($\cos(\pi/6) = +0.866$ vs. $\cos(5\pi/6) = -0.866$), uniquely identifying every point on the 24-hour manifold.
  3. **Mathematical Proof of Context-Dependent Slope in Interaction Models:**
  * Given $y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 (x_1 x_2)$, compute the partial derivative with respect to $x_1$:
   $$\frac{\partial y}{\partial x_1} = \frac{\partial}{\partial x_1} \Big[ \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 (x_1 x_2) \Big] = \mathbf{\beta_1 + \beta_3 x_2}$$
  * The instantaneous marginal effect of $x_1$ on $y$ is not a global constant $\beta_1$; it is explicitly parameterized by the current value of $x_2$. If $\beta_3 \neq 0$, the slope along $x_1$ varies dynamically across subsets of $x_2$.
  
  ---
## 2. Selection vs. Extraction: The Fundamental Architectural Divide (Slide 6)
  
  Slide 6 formalizes the two primary paradigms for reducing the feature dimension of a design matrix from $X \in \mathbb{R}^{N \times d}$ to a lower-dimensional representation $Z \in \mathbb{R}^{N \times k}$ (where $k \ll d$):
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │                   Dimensionality Optimization (Slide 6)                │
  │                                                                        │
  │   FEATURE SELECTION (Subset Pruning)     FEATURE EXTRACTION (Manifold) │
  │   X_sub ⊂ {X₁, X₂, ..., X_d}             Z = Φ(X)                      │
  │   • Chooses subset of original features  • Constructs new composite    │
  │   • Retains physical units/semantics       coordinate representations  │
  │   • High human interpretability          • Lower direct interpretability│
  │   • Techniques: Filter, Wrapper,         • Techniques: PCA, SVD,       │
  │     Embedded (Lasso, Tree Importance)      Autoencoders                │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Comparison Matrix: Selection vs. Extraction (Slide 6)
  
  | Architectural Aspect | **Feature Selection** | **Feature Extraction** |
  | :--- | :--- | :--- |
  | **Operational Mechanism** | Selects an optimal index subset $\mathcal{S} \subset \{1, \dots, d\}$ of existing columns. | Projects $d$-dimensional space into a $k$-dimensional latent manifold via linear/non-linear mappings: $Z = g(X)$. |
  | **Example Formulation** | Retain strictly `size` and `lot_size` from a housing dataset. | Construct a composite coordinate: $\text{PC}_1 = 0.71 \cdot \text{size} + 0.70 \cdot \text{lot\_size}$. |
  | **Output Feature Matrix** | Columns are an exact subset of the original inputs ($X_{\mathcal{S}} \in \mathbb{R}^{N \times k}$). | Columns are dense synthetic transformations ($Z \in \mathbb{R}^{N \times k}$). |
  | **Primary Methodologies** | **Filter** ($\chi^2$, ANOVA, MI), **Wrapper** (RFE), **Embedded** ($L_1$ Lasso, Decision Trees). | **Linear:** PCA, Truncated SVD. <br>**Non-Linear:** Kernel PCA, UMAP, Deep Autoencoders. |
  | **Semantic Interpretability** | **High:** Original units, sensor metrics, and clinical variables are preserved. | **Low:** Dimensions represent linear combinations or latent representations; physical units are lost. |
  | **Primary Engineering Goal** | Prune irrelevant noise and redundant collinear features. | Maximize information/variance retention while compressing dimensionality. |
  
  ---
## 3. The Triad of Feature Selection Methodologies (Slide 6)
  
  Slide 6 categorizes feature selection into three algorithmic tiers: **Filter**, **Wrapper**, and **Embedded** methods.
  
  ```
                      The Three Selection Paradigms
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Filter Methods (Statistical Independence Tests)                     │
  │    Features ──▶ [ Statistical Ranking: S(X_j, Y) ] ──▶ Top k Subset   │
  │    • Fast, univariate, completely model-agnostic.                      │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Wrapper Methods (Iterative Model Black-Box Search)                  │
  │    Subset ──▶ [ Train Model ] ──▶ [ Score on Val Fold ] ──▶ Update Sub │
  │    • High computational cost (O(2^d)); captures feature interactions.  │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. Embedded Methods (Regularized Objective Functions)                  │
  │    Loss Optimization: min_w ℒ(y, Xw) + λ||w||₁ ──▶ Sparsity baked in  │
  │    • Blends computational efficiency of filters with model awareness.  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### A. Filter Methods: Univariate Statistical Ranking
  Filter methods evaluate the statistical relationship between each feature $X_j$ and the target label $y$ **independently of any learning algorithm**.
  
  1. **ANOVA $F$-Score (`f_classif` for Categorical $y$, Continuous $X_j$):**
   Computes the ratio of between-group variance to within-group variance across $C$ target classes:
   $$F = \frac{\text{Between-Class Variance}}{\text{Within-Class Variance}} = \frac{\sum_{c=1}^C N_c (\bar{x}_c - \bar{x})^2 / (C - 1)}{\sum_{c=1}^C \sum_{i=1}^{N_c} (x_{ic} - \bar{x}_c)^2 / (N - C)}$$
   *High $F$-values indicate that class means are well-separated relative to internal dispersion.*
  2. **Mutual Information (`mutual_info_classif` / `mutual_info_regression`):**
   Quantifies the reduction in uncertainty of target $Y$ given knowledge of feature $X_j$ using Shannon entropy:
   $$I(X_j; Y) = \iint p(x_j, y) \log \left( \frac{p(x_j, y)}{p(x_j)p(y)} \right) dx_j \, dy$$
   *Unlike Pearson correlation, Mutual Information captures non-linear, non-monotonic dependencies.*
  3. **The Systems Trade-Off:**
   * **Advantage:** Computational complexity is linear in features ($\mathcal{O}(d)$); runs in seconds over millions of columns.
   * **Fatal Blindness:** **Filter methods evaluate features in isolation (univariate).** They cannot detect multi-feature interactions. For example, in an XOR relationship ($Y = X_1 \oplus X_2$), both $X_1$ and $X_2$ have an individual correlation and mutual information of zero with $Y$, causing filter methods to discard them despite being jointly deterministic.
  
  ---
### B. Wrapper Methods: Search-Driven Feature Selection
  Wrapper methods treat the machine learning algorithm as an evaluation oracle, using a search heuristic to identify the subset that maximizes validation performance.
  
  1. **Recursive Feature Elimination (RFE):**
   * Step 1: Fit the estimator on all $d$ features.
   * Step 2: Extract feature importance scores (e.g., parameter magnitudes $|w_j|$ in linear models, or Gini importances in trees).
   * Step 3: Prune the $p$ features with the smallest scores.
   * Step 4: Re-fit the estimator on the remaining features. Repeat recursively until $k$ features remain.
  2. **Forward Selection vs. Backward Elimination:**
   * *Forward Selection:* Starts with $\emptyset$; iteratively adds the single feature that maximizes cross-validation score until gains plateau.
   * *Backward Elimination:* Starts with all $d$ features; iteratively prunes the least valuable feature.
  3. **The Systems Trade-Off:**
   * **Advantage:** Captures complex cross-feature interactions and tailors the selected subset to the exact inductive bias of the chosen model family.
   * **Disadvantage:** Computationally prohibitive. Evaluating all subsets requires exploring an exponential power set ($2^d$). Greedy approximations require fitting hundreds of distinct models ($\mathcal{O}(d^2)$ complexity), which risks overfitting the validation set on small datasets ($N \ll d$).
  
  ---
### C. Embedded Methods: Intrinsic Optimization Sparsity
  Embedded methods incorporate feature selection directly into the mathematical objective function:
  
  1. **$L_1$ Regularization (Lasso / Sparse Penalties):**
   $$J(w) = \frac{1}{N}\sum_{i=1}^N \mathcal{L}(y_i, w^T x_i) + \lambda \sum_{j=1}^d |w_j|$$
   * Because the $L_1$ unit ball has sharp vertices on the coordinate axes, minimizing the objective forces non-essential coefficients to be **identically zero**. Lasso simultaneously performs parameter estimation and feature selection in a single convex optimization pass.
  2. **Tree-Based Feature Importance (MDI / MDA):**
   * *Mean Decrease in Impurity (MDI):* Sums the total reduction in Gini or Entropy across all splits where feature $j$ was selected. Features with near-zero total impurity reduction are pruned.
  
  ---
## 4. Feature Extraction: Projective Manifolds (Slide 6)
  
  When the primary objective is **reconstruction fidelity and variance maximization** rather than interpretability, feature extraction projects $X$ into an orthogonal latent space.
  
  ```
                    Principal Component Extraction
    Original Space (Correlated Coordinates)       Extracted Space (Orthogonal Manifold)
    x_2                                           PC_2
      ▲          .::'                               ▲
      │       .::'  (High Covariance)               │          ...
      │    .::'                                     │       .:::::::..
      │ .::'   ──▶ PC_1 (Max Variance)              │        ':::::'
      │::'                                          │
      └────────────────────────▶ x_1                └────────────────────────▶ PC_1
                                                    (Zero Covariance: Cov(PC_1, PC_2) = 0)
  ```
### 1. Principal Component Analysis (PCA)
  * Seeks an orthonormal projection matrix $V_k \in \mathbb{R}^{d \times k}$ that maximizes the variance of the projected data $Z = X V_k$:
  $$\max_{V_k} \text{Tr}\left( V_k^T \Sigma V_k \right) \quad \text{subject to } V_k^T V_k = I$$
  * Solved via the **Spectral Eigendecomposition** of the empirical covariance matrix $\Sigma = \frac{1}{N-1}X^T X$, where the columns of $V_k$ are the $k$ eigenvectors corresponding to the largest eigenvalues ($\lambda_1 \ge \lambda_2 \ge \dots \ge \lambda_k$).
### 2. Singular Value Decomposition (SVD)
  * Decomposes the centered data matrix directly without materializing $X^T X$:
  $$X = U \Sigma V^T$$
  Truncating the factorization to the top $k$ singular values provides the optimal low-rank matrix approximation (Eckart-Young-Mirsky Theorem).
### 3. Deep Autoencoders
  * A symmetric neural network parameterized by an encoder $z = \sigma(W_e x + b_e)$ and a decoder $\hat{x} = \sigma(W_d z + b_d)$ trained to minimize reconstruction loss:
  $$\mathcal{L}_{\text{reconstruction}} = \frac{1}{N}\sum_{i=1}^N \|x_i - \hat{x}_i\|_2^2$$
  * Provides non-linear dimensionality reduction, capturing curved manifolds (such as Swiss roll geometries) that linear PCA collapses.
  
  ---
## 5. Decision Framework: When to Select vs. When to Extract
  
  ```
  ┌───────────────────────────────────────┬──────────────────────────────────────────┐
  │ Scenario / Engineering Constraint     │ Optimal Paradigm & Selected Technique    │
  ├───────────────────────────────────────┼──────────────────────────────────────────┤
  │ Clinical trial / Regulatory audit     │ FEATURE SELECTION (Filter or Lasso)      │
  │ (Physicians must justify biomarker)   │ • Retains original units (e.g., mg/dL).  │
  ├───────────────────────────────────────┼──────────────────────────────────────────┤
  │ High-dimensional multicollinear data  │ FEATURE EXTRACTION (PCA / SVD)           │
  │ (e.g., Spectrometry, thermal sensors) │ • Eliminates multicollinearity (Cov = 0).│
  ├───────────────────────────────────────┼──────────────────────────────────────────┤
  │ Massive feature space (d > 50,000)    │ FEATURE SELECTION (Filter Methods)       │
  │ (e.g., Genomics, Bag-of-Words NLP)    │ • Univariate O(d) pruning to top k.      │
  ├───────────────────────────────────────┼──────────────────────────────────────────┤
  │ Complex, non-linear interaction space │ WRAPPER (RFE) or EMBEDDED (Trees)        │
  │ (Tabular data with moderate d < 100)  │ • Evaluates combinations and splits.     │
  └───────────────────────────────────────┴──────────────────────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 2
  
  1. *Why will univariate filter methods (such as ANOVA $F$-test or Pearson correlation) completely fail to select features that govern an exclusive-OR (XOR) target relationship?*
  2. *From a computational complexity perspective, why is Recursive Feature Elimination (RFE) impractical when the initial feature dimensionality $d$ is on the order of $100{,}000$?*
  3. *If you compress a customer credit dataset using Principal Component Analysis down to $k=3$ components, what major operational challenge arises when a loan applicant asks why their application was rejected?*
  
  ---