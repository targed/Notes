## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Why Decision Trees Are Invariant to Monotonic Scaling:**
  * Decision trees optimize split thresholds based on the **rank order** of sample coordinates along each feature axis to maximize Information Gain or minimize Gini Impurity:
   $$\arg\max_{j, \, \theta} \Delta I(j, \theta)$$
  * Any strictly monotonic transformation $g(x)$ preserves order relations: $a < b \iff g(a) < g(b)$.
  * The partition of instances into the left and right subsets ($D_{\text{left}}, D_{\text{right}}$) remains identical for any threshold $\theta' = g(\theta)$. The calculated impurities, tree topology, depth, and predictions are completely unchanged.
  2. **Hessian Condition Number Calculation & Gradient Descent Impact:**
  * Given variances $\sigma_1^2 = 10{,}000$ and $\sigma_2^2 = 0.01$, the eigenvalues of the Hessian $H \approx \frac{1}{N}X^T X$ for uncorrelated features are $\lambda_{\max} \approx 10{,}000$ and $\lambda_{\min} \approx 0.01$.
  * The **Condition Number** is:
   $$\kappa = \frac{\lambda_{\max}}{\lambda_{\min}} = \frac{10{,}000}{0.01} = \mathbf{1{,}000{,}000} \quad (\mathbf{10^6})$$
  * **Gradient Descent Impact:** The loss surface forms an extremely steep, narrow canyon. To prevent numerical divergence, the learning rate must satisfy $\alpha < \frac{2}{\lambda_{\max}} = 0.0002$. At this tiny step size, progress along the flat axis ($\lambda_{\min} = 0.01$) becomes negligible, requiring tens of thousands of iterations to converge.
  3. **Why $L_1$ Lasso Regularization Discriminates Against Small-Scale Features:**
  * To maintain an identical effect on a prediction ($w_j x_j$), weights must scale inversely with feature magnitude: $w_j \propto \frac{1}{\text{Scale}(X_j)}$.
  * Features with small numerical ranges require large parameter weights ($w_{\text{small}}$), whereas features with large numerical ranges require small parameter weights ($w_{\text{large}}$).
  * Because the $L_1$ penalty $\lambda \sum |w_j|$ applies a uniform linear shrinkage rate, the large coefficient $w_{\text{small}}$ is heavily penalized and driven to zero by the soft-thresholding operator ($S_{\lambda}(w) = \text{sign}(w)\max(|w| - \lambda, 0)$), while $w_{\text{large}}$ easily escapes truncation.
  
  ---
## 2. The Four Canonical Scalers: Mathematical Formulations (Slide 4)
  
  Slide 4 categorizes the primary scaling transformers in modern applied machine learning:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │                        The Four Scalers (Slide 4)                      │
  │                                                                        │
  │   StandardScaler          MinMaxScaler           RobustScaler          │
  │   (x - μ) / σ             [0, 1] Bounded         Median & IQR          │
  │   • Zero mean, unit var   • Rigid bounds [0, 1]  • High breakdown pt   │
  │   • Symmetric data        • Bounded inputs (NN)  • Outlier-heavy data  │
  │                                                                        │
  │                              PowerTransformer                          │
  │                              Box-Cox / Yeo-Johnson                     │
  │                              • Reshapes non-Gaussian distributions     │
  │                              • Stabilizes variance / heteroscedasticity│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### 1. `StandardScaler` (Z-Score Standardization)
  Standardizes continuous features by removing the empirical mean and scaling to unit variance:
  
  $$z = \frac{x - \mu}{\sigma}$$
  
  * **Fitted Parameters Learned on Training Data ($\theta_{\text{prep}}$):**
  $$\hat{\mu} = \frac{1}{N_{\text{tr}}} \sum_{i=1}^{N_{\text{tr}}} x_i, \quad \hat{\sigma} = \sqrt{\frac{1}{N_{\text{tr}}} \sum_{i=1}^{N_{\text{tr}}} (x_i - \hat{\mu})^2}$$
  * **Mathematical Properties:**
  $$\mathbb{E}[Z] = 0, \quad \text{Var}(Z) = 1$$
  * **Best Use Case:** Roughly symmetric, bell-shaped distributions without extreme leverage points.
  * **Limitations:** 
  * Does not produce a bounded range; values can extend beyond $\pm 3\sigma$.
  * Extremely sensitive to outliers, which artificially inflate $\hat{\sigma}$ and distort $\hat{\mu}$.
  
  ---
### 2. `MinMaxScaler` (Bounded Affine Compression)
  Compresses all values into a strictly bounded compact interval, typically $[0, 1]$:
  
  $$x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}} \cdot (\text{max} - \text{min}) + \text{min}$$
  
  For the standard target range $[0, 1]$:
  
  $$x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$
  
  * **Fitted Parameters Learned on Training Data ($\theta_{\text{prep}}$):**
  $$x_{\min} = \min_{i \in \text{train}} x_i, \quad x_{\max} = \max_{i \in \text{train}} x_i$$
  * **Best Use Case:** 
  * Image processing (normalizing 8-bit integer pixel channels $[0, 255] \to [0.0, 1.0]$).
  * Neural network inputs where activation functions require bounded domains (e.g., Sigmoid, Tanh).
  * **Failure Mode (The Outlier Collapse):**
  * If a single extreme outlier exists ($x_{\max} = 1{,}000{,}000$ when inliers sit in $[0, 100]$):
    $$x'_{\text{inliers}} = \frac{x}{1{,}000{,}000} \in [0.0000, \; 0.0001]$$
  * The inliers are compressed into an infinitesimal fraction of the unit interval, destroying the model's ability to resolve variance across normal instances.
  
  ---
### 3. `RobustScaler` (Order-Statistic Rescaling)
  Centers and rescales features using **order statistics (percentiles)** rather than parametric sample moments:
  
  $$x_{\text{robust}} = \frac{x - \text{median}(X)}{\text{IQR}(X)} = \frac{x - Q_2(X)}{Q_3(X) - Q_1(X)}$$
  
  * **Fitted Parameters Learned on Training Data ($\theta_{\text{prep}}$):**
  $$Q_1 = F^{-1}(0.25), \quad Q_2 = \text{Median} = F^{-1}(0.50), \quad Q_3 = F^{-1}(0.75)$$
  $$\text{IQR} = Q_3 - Q_1$$
  * **Robustness & Breakdown Point:**
  * The sample mean and variance have a **breakdown point of $0\%$** (a single corrupted observation can shift $\mu$ or $\sigma$ to infinity).
  * The median has a **breakdown point of $50\%$**, and the interquartile range has a **breakdown point of $25\%$**.
  * **Best Use Case:** Features containing genuine, heavy-tailed extreme values (e.g., financial transaction volumes, website page hits, sensor surges) where outliers must not distort the scaling of the core distribution.
  
  ---
### 4. `PowerTransformer` (Variance-Stabilizing Non-Linear Warps)
  Unlike the previous three linear affine scalers, `PowerTransformer` applies a **non-linear parametric mapping** to reshape skewed, non-normal distributions into Gaussian-like distributions.
  
  Slide 4 lists two primary mathematical formulations:
#### A. Box-Cox Transformation (Box & Cox, 1964)
  * **Strict Constraint:** Requires strictly positive data ($x > 0$).
  * **Mathematical Formulation:**
  $$x^{(\lambda)} = \begin{cases} \frac{x^\lambda - 1}{\lambda} & \text{if } \lambda \neq 0 \\ \ln(x) & \text{if } \lambda = 0 \end{cases}$$
  * The hyperparameter $\lambda$ is estimated via **Maximum Likelihood Estimation (MLE)** by maximizing the Gaussian profile log-likelihood:
  $$L(\lambda) = -\frac{N}{2} \ln(\hat{\sigma}^2(\lambda)) + (\lambda - 1) \sum_{i=1}^N \ln(x_i)$$
#### B. Yeo-Johnson Transformation (Yeo & Johnson, 2000)
  * **Advantage:** Extends the Box-Cox framework to support zero and negative numbers ($x \in \mathbb{R}$).
  * **Mathematical Formulation:**
  $$x^{(\lambda)} = \begin{cases} \frac{(x + 1)^\lambda - 1}{\lambda} & \text{if } \lambda \neq 0, \; x \ge 0 \\ \ln(x + 1) & \text{if } \lambda = 0, \; x \ge 0 \\ -\frac{(-x + 1)^{2 - \lambda} - 1}{2 - \lambda} & \text{if } \lambda \neq 2, \; x < 0 \\ -\ln(-x + 1) & \text{if } \lambda = 2, \; x < 0 \end{cases}$$
  * **Best Use Case:** Resolving **heteroscedasticity** (non-constant variance) and heavy tail asymmetry in linear regression pipelines.
  
  ---
## 3. The Core Axiom: Standardization Changes Scale, Not Shape (Slide 9)
  
  Slide 9 emphasizes an essential statistical principle:
  
  $$\mathbf{\text{"Standardization }\longrightarrow\text{ changes scale, not shape!"}}$$
  
  ```
                       Affine Invariance of Shape
        Raw Feature X (Log-Normal Skewed)             Standardized Feature Z = (X - μ) / σ
    Density                                       Density
      ▲   ╭─╮                                       ▲   ╭─╮
      │  ╭╯ ╰─╮                                     │  ╭╯ ╰─╮
      │ ╭╯    ╰──╮                                  │ ╭╯    ╰──╮
      │╭╯        ╰───────                           │╭╯        ╰─────── (STILL SKEWED!)
      └────────────────────────▶ x                  └────────────────────────▶ z
      0            50         100                  -1            0          3
      • Mean = 24.1, Std = 18.2                    • Mean = 0.0, Std = 1.0
      • Skewness γ₁ = 2.45                         • Skewness γ₁ = 2.45 (IDENTICAL!)
  ```
### Mathematical Proof of Shape Invariance:
  Let $Z = aX + b$ be an affine linear transformation (for `StandardScaler`, $a = \frac{1}{\sigma}$ and $b = -\frac{\mu}{\sigma}$).
  
  Inspect the **Fisher-Pearson Skewness** of the standardized variable $Z$:
  
  $$\gamma_1(Z) = \mathbb{E}\left[ \left(\frac{Z - \mathbb{E}[Z]}{\sigma_Z}\right)^3 \right] = \mathbb{E}\left[ \left(\frac{(aX + b) - (a\mu + b)}{a\sigma}\right)^3 \right] = \mathbb{E}\left[ \left(\frac{a(X - \mu)}{a\sigma}\right)^3 \right]$$
  
  Because scalar $a > 0$ cancels cleanly from the fraction:
  
  $$\gamma_1(Z) = \mathbb{E}\left[ \left(\frac{X - \mu}{\sigma}\right)^3 \right] \equiv \mathbf{\gamma_1(X)}$$
  
  **Takeaway:** Applying `StandardScaler` to a skewed distribution shifts the origin to $0$ and compresses the horizontal axis to unit variance, but the **distributional shape, skewness, and tail proportions remain identical**. Standardizing does **not** make data Gaussian.
  
  ---
## 4. Transforming Skewed Features (Slide 9)
  
  Slide 9 illustrates the morphological mechanics for normalizing asymmetric variables:
  
  ```
                   Transforming Distributional Skew (Slide 9)
         Right-Skewed (Positive Tail)                    Left-Skewed (Negative Tail)
    Density                                         Density
      ▲   ╭──╮                                        ▲            ╭──╮
      │  ╭╯  ╰─╮                                      │          ╭─╯  ╰╮
      │ ╭╯     ╰──╮                                   │       ╭──╯     ╰╮
      │╭╯         ╰───────                            │───────╯         ╰╮
      └────────────────────────▶ x                    └────────────────────────▶ x
      Tail extends toward POSITIVE values             Tail extends toward NEGATIVE values
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Transformation Strategies:                                             │
  │                                                                        │
  │ • Right-Skewed Data: Compress large values relative to small values.   │
  │   1. Logarithmic:  y = log(x)  or  log(x + 1) [np.log1p]               │
  │   2. Square Root:   y = sqrt(x)  (Milder tail compression)             │
  │   3. Reciprocal:    y = 1 / x    (Aggressive tail inversion)           │
  │                                                                        │
  │ • Left-Skewed Data: Expand large values or reflect.                   │
  │   1. Reflection:    x_reflected = (x_max + 1) - x ──▶ Apply log        │
  │   2. Power Warps:   y = x^k  (where k > 1, e.g., x², x³)               │
  │   3. Yeo-Johnson:   Automates negative tail parameter optimization.    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. Decision Matrix: Selecting the Appropriate Scaler
  
  ```
  ┌───────────────────────┬─────────────────────────┬──────────────────────────────────────────┐
  │ Scaler                │ Robust to Outliers?     │ Impact on Distributional Shape           │
  ├───────────────────────┼─────────────────────────┼──────────────────────────────────────────┤
  │ StandardScaler        │ NO (μ, σ are corrupted) │ None (Pure affine scale shift; retains   │
  │                       │                         │ original skewness and kurtosis).         │
  ├───────────────────────┼─────────────────────────┼──────────────────────────────────────────┤
  │ MinMaxScaler          │ NO (Outliers compress   │ None (Rigid linear mapping to [0, 1];    │
  │                       │ inliers to near-zero)   │ retains original skewness).              │
  ├───────────────────────┼─────────────────────────┼──────────────────────────────────────────┤
  │ RobustScaler          │ YES (Median & IQR have  │ None (Robust linear mapping; retains     │
  │                       │ high breakdown points)  │ original skewness).                      │
  ├───────────────────────┼─────────────────────────┼──────────────────────────────────────────┤
  │ PowerTransformer      │ MODERATE (MLE fits      │ HIGH (Non-linear warp; actively reshapes │
  │ (Box-Cox/Yeo-Johnson) │ normal distribution)    │ skewed densities into bell curves).      │
  └───────────────────────┴─────────────────────────┴──────────────────────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 2
  
  1. *Why does applying `StandardScaler` to a right-skewed log-normal distribution fail to convert it into a bell-shaped Gaussian distribution?*
  2. *If a dataset contains a feature with extreme measurement outliers ($x_{\max} = 50{,}000$ when typical samples range between $[10, 50]$), what happens to normal inlier samples if you process this feature using `MinMaxScaler`?*
  3. *What is the mathematical limitation of the Box-Cox power transformation, and how does the Yeo-Johnson formulation resolve this constraint?*
  
  ---