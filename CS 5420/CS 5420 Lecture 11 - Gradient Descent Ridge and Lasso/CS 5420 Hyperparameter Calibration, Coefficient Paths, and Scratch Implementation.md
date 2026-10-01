## 1. Quick Review: Answers to Section 4 Checkpoints
  
  1. **Why Expanding Loss Ellipses Hit $L_1$ Vertices First (Exact Sparsity):**
  * The $L_1$ constraint region $|w_1| + |w_2| \le c$ forms a cross-polytope (diamond) with non-differentiable, sharp vertices positioned directly on the coordinate axes ($w_1 = 0$ or $w_2 = 0$).
  * At each vertex, the boundary is non-smooth; it supports a wide cone of subgradient normal vectors rather than a single unique normal line.
  * As the elliptical level sets of the squared error loss expand outward from the unconstrained OLS solution $w_{\text{OLS}}$, it is geometrically far more probable for an expanding ellipse to make first contact at a protruding corner than along a flat face. Tangency at an axis vertex forces the orthogonal coordinate to be **identically zero ($w_j^* = 0.0$)**, producing exact feature selection.
  * In contrast, the $L_2$ boundary $w_1^2 + w_2^2 \le c$ is a smooth hypersphere with continuous tangent vectors everywhere. The probability of an expanding ellipse touching the circle precisely on an axis is a measure-zero event, meaning both weights remain non-zero.
  2. **The Soft-Thresholding "Dead Zone":**
  * Under coordinate descent, the analytical update for parameter $w_j$ is governed by the soft-thresholding operator:
   $$w_j^* = \mathcal{S}_\alpha(\rho_j) = \text{sign}(\rho_j)\max(0, \; |\rho_j| - \alpha)$$
   where $\rho_j = x_j^T (y - \sum_{k \neq j} w_k x_k)$ is the correlation between feature $j$ and the partial residual.
  * If the magnitude of this correlation is weaker than the regularization penalty ($|\rho_j| \le \alpha$), the function falls into the **"Dead Zone."**
  * The operator evaluates to $\mathcal{S}_\alpha(\rho_j) = 0.0$. The feature's marginal predictive power is insufficient to overcome the penalty threshold $\alpha$, so its weight is snapped strictly to zero and eliminated from the model.
  3. **Collinear Features in Ridge vs. Lasso ($x_1 = x_2$):**
  * **Ridge Regression ($L_2$):** Splits the weight equally between the two features:
   $$w_1 = w_2 = \frac{1}{2} w_{\text{total}}$$
   Because the penalty is quadratic ($w_1^2 + w_2^2$), minimizing the penalty for a fixed sum $w_1 + w_2 = S$ requires $w_1 = w_2$ (e.g., $1^2 + 1^2 = 2$, whereas $2^2 + 0^2 = 4$). Ridge stabilizes collinear groups by shrinking them together.
  * **Lasso Regression ($L_1$):** Exhibits group instability. The penalty $|w_1| + |w_2| = S$ is constant for any non-negative combination along the line $w_1 + w_2 = S$. In practice, minor numerical noise or floating-point rounding causes Lasso to **arbitrarily select one feature** ($w_1 = S, w_2 = 0$) and set the other to zero, making feature attribution unstable under multicollinearity.
  
  ---
## 2. Principles of Hyperparameter Calibration ($\alpha$) (Slide 12)
  
  Slide 12 outlines four foundational rules for tuning the regularization strength $\alpha$:
  
  ```
                         Rules for Choosing α (Slide 12)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. The Training Error Trap:                                            │
  │    "NEVER by looking at training error, it always prefers α = 0."      │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Log-Spaced Search Grids:                                            │
  │    "Cross-validation over a log-spaced grid: np.logspace(-4, 4, 50)"   │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. Coefficient Path Auditing:                                          │
  │    "Plot the coefficient path: coefficient value against α"            │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 4. The Feature Scaling Prerequisite:                                   │
  │    "SCALE YOUR FEATURES FIRST, the penalty is not scale-invariant."    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Deep Dive into the Four Rules:
#### 1. Why Training Error Always Prefers $\alpha = 0$
  Consider the regularized objective function:
  
  $$J(w) = \mathcal{L}_{\text{train}}(w) + \alpha \Omega(w)$$
  
  * Unconstrained Ordinary Least Squares ($\alpha = 0$) minimizes $\mathcal{L}_{\text{train}}(w)$ directly without restrictions on parameter magnitude.
  * For any $\alpha > 0$, the penalty term forces weights away from the unconstrained empirical minimum, trading a higher training error for lower variance.
  * Consequently, **training error is a monotonically increasing function of $\alpha$**. Evaluating candidate values of $\alpha$ on the training set will always select $\alpha = 0$, completely defeating the purpose of regularization. Hyperparameter tuning must be evaluated strictly on out-of-fold validation splits via **$K$-Fold Cross-Validation** (`RidgeCV`, `LassoCV`).
#### 2. Why Search Grids Must Be Log-Spaced (`np.logspace`)
  Slide 12 specifies: `np.logspace(-4, 4, 50)`.
  * Regularization parameters operate over **orders of magnitude**, spanning from $10^{-4}$ ($0.0001$, near-OLS behavior) to $10^{+4}$ ($10{,}000$, heavy shrinkage).
  * If you use a linear grid (`np.linspace(0.0001, 10000, 50)`), $45$ of your $50$ candidate points will sit above $1{,}000$, where all weights are already crushed to near-zero. You would test only one or two values in the critical transition region ($[0.001, 10]$).
  * A logarithmic grid samples uniformly across exponential scales:
  $$\alpha \in \{10^{-4}, \; 10^{-3.84}, \; \dots, \; 10^0, \; \dots, \; 10^{3.84}, \; 10^4\}$$
  ensuring fine-grained resolution across both weak and strong regularization regimes.
  
  ---
### 3. Coefficient Paths: Visualizing Shrinkage Dynamics (Slide 12)
  A **Coefficient Path** plots the trajectory of each parameter weight $w_j$ as a function of the regularization strength $\log(\alpha)$:
  
  ```
                     Coefficient Path Trajectories
         Ridge Coefficient Path (L2)                    Lasso Coefficient Path (L1)
    w_j                                            w_j
     ▲                                              ▲
  10 │  ───╮                                     10 │  ───╮
     │      ╰──────                                 │      ╰───╮
   5 │  ───────────╰──────                        5 │  ─────────╰───● (Dropped to 0!)
     │                                              │               │
   0 ┼───────────────────────────▶ log(α)           0 ┼─────────────┴───────────▶ log(α)
  -5 │  ───────────╭──────                         -5 │  ───────● (Dropped to 0!)
     │      ╭──────                                 │          /
  -10 │  ───╯                                    -10 │  ───╭───╯
       Low α                  High α                  Low α                  High α
       (Smooth asymptotic decay to 0)                 (Piecewise linear; hits EXACT 0)
  ```
  
  * **Ridge Path:** Curves decay smoothly toward the zero axis, tapering off asymptotically without intersecting zero at any finite $\alpha$.
  * **Lasso Path:** Curves are piecewise linear. Individual parameters hit zero at distinct critical thresholds ($\alpha_{\text{critical}}$), remaining clamped to zero as $\alpha$ increases. The order in which features drop to zero provides a natural ranking of feature importance.
#### 4. Why Feature Scaling Is Mandatory
  As derived in Lecture 8, regularizers penalize parameter sizes uniformly:
  
  $$\Omega_{\text{Ridge}}(w) = \sum_{j=1}^d w_j^2, \quad \Omega_{\text{Lasso}}(w) = \sum_{j=1}^d |w_j|$$
  
  Because weight magnitudes scale inversely with feature variance ($w_j \propto 1/\sigma_j$), unscaled features with small numerical ranges require large coefficients. The regularizer penalizes these small-scale features much more heavily than large-scale features, shrinking them to zero regardless of their predictive value. **Features must be standardized ($\mu=0, \sigma=1$) prior to regularized fitting.**
  
  ---
## 3. Data Preparation & Feature Setup (Slide 13)
  
  Slide 13 sets up the live-coding environment on the Diabetes benchmark:
  
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  from sklearn.datasets import load_diabetes
  from sklearn.model_selection import train_test_split
  from sklearn.preprocessing import StandardScaler
  
  # Ingest benchmark (N = 442, d = 10 clinical features)
  data = load_diabetes()
  X = data.data
  y = data.target
  
  print("X shape:", X.shape)
  print("Feature names:", data.feature_names)
  
  # Standardize features to guarantee scale invariance
  scaler = StandardScaler()
  X = scaler.fit_transform(X)
  
  # Add homogeneous bias column of ones for intercept w0
  X = np.c_[np.ones(X.shape[0]), X]
  
  # Partition into training and testing sets
  X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
  )
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Pedagogical vs. Production Note on Slide 13:                           │
  │                                                                        │
  │ Notice that in Slide 13's live-coding demonstration script,            │
  │ scaler.fit_transform(X) is executed across the entire dataset before   │
  │ calling train_test_split().                                            │
  │                                                                        │
  │ • In classroom demos: Instructors often use this shortcut to keep code │
  │   concise within a 5-minute live-coding block.                         │
  │ • In graded assignments & production (Lectures 7–9): This is           │
  │   PREPROCESSING LEAKAGE. In your homework and project, you must        │
  │   split first, fit the scaler on X_train, and apply .transform() to    │
  │   X_test, or encapsulate the workflow inside a Pipeline!               │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Batch Gradient Descent Implementation from Scratch (Slide 14)
  
  Slide 14 provides the complete Python implementation of Batch Gradient Descent for linear regression:
  
  ```python
  def batch_gd(X, y, eta=0.01, n_iter=1000):
    n_samples, n_features = X.shape
    
    # 1. Initialize weight vector to zeros: w^(0) = [0, 0, ..., 0]ᵀ
    w = np.zeros(n_features)
    losses = []
    
    for i in range(n_iter):
        # 2. Forward Pass: Compute predictions ŷ = Xw
        y_pred = X @ w
        
        # 3. Compute Residual Error Vector: e = ŷ - y
        error = y_pred - y
        
        # 4. Track Mean Squared Error Loss: J(w) = (1 / n) ∑ (ŷ_i - y_i)²
        loss = np.mean(error ** 2)
        losses.append(loss)
        
        # 5. Backward Pass: Compute Analytical Gradient ∇J(w) = (2 / n) Xᵀ(Xw - y)
        gradient = (2 / n_samples) * (X.T @ error)
        
        # 6. Parameter Update Step: w^(t+1) = w^(t) - η ∇J(w)
        w = w - eta * gradient
        
    return w, losses
  ```
  
  ---
### Mathematical Alignment of the Code:
  Notice the gradient calculation in step 5:
  ```python
  gradient = (2 / n_samples) * X.T @ error
  ```
  
  Let the cost function be the sample mean squared error:
  
  $$J(w) = \frac{1}{n} \|Xw - y\|_2^2 = \frac{1}{n} (Xw - y)^T (Xw - y)$$
  
  Taking the vector derivative with respect to $w$:
  
  $$\nabla_w J(w) = \frac{2}{n} X^T (Xw - y) = \mathbf{\frac{2}{n} X^T (\hat{y} - y)}$$
  
  This matches the line `gradient = (2 / n_samples) * X.T @ error` exactly. The multiplication by $X^T$ evaluates the inner product between each feature column and the residual vector, computing the exact slope along each coordinate axis.
  
  ---
## 5. Empirical Learning Rate Evaluation (Slide 14)
  
  Slide 14 executes the optimizer across three learning rates ($\eta \in \{0.001, 0.01, 0.1\}$):
  
  ```python
  learning_rates = [0.001, 0.01, 0.1]
  plt.figure(figsize=(8, 5))
  
  for eta in learning_rates:
    w, losses = batch_gd(
        X_train,
        y_train,
        eta=eta,
        n_iter=1000
    )
    plt.plot(losses, label=f"eta = {eta}")
  
  plt.xlabel("Iteration")
  plt.ylabel("MSE Loss")
  plt.title("Batch Gradient Descent")
  plt.legend()
  plt.grid(True)
  plt.show()
  ```
  
  ```
                 Loss Trajectories Across Learning Rates (Slide 14)
    MSE Loss
       ▲
  30,000│ ──●
       │   │\
  25,000│   │ \
       │   │  \
  20,000│   │   \  η = 0.001 (Too Low): Stalls; drops very slowly
       │   │    \
  15,000│   │     ╰──────────────────────────────────────────
       │   │
  10,000│   │      η = 0.01 (Moderate): Steady descent, but needs >1,000 iterations
       │   │\
  5,000│   │ ╰──────────────────────────────────────────────
       │   │  η = 0.1 (Optimal): Rapid exponential drop; converges by iteration ~200
  2,800│   └───--------------------------------------------- (Minimum Bowl Floor)
       └────────────────────────────────────────────────────▶ Iterations
       0     200      400      600      800     1000
  ```
  
  ---
### Empirical Behavior Analysis:
  1. **$\eta = 0.001$ (Too Small):**
   * The step size is conservative. After $1{,}000$ iterations, the loss has only dropped from $\approx 29{,}000$ down to $\approx 15{,}000$. It is still far from the global minimum, requiring tens of thousands of additional steps.
  2. **$\eta = 0.01$ (Moderate):**
   * Smooth, stable descent. It reaches the low-loss basin around iteration $800$, but has not completely settled into the flat minimum floor.
  3. **$\eta = 0.1$ (Optimal):**
   * Exhibits fast convergence. Within the first $150$ iterations, the loss drops from $29{,}000$ down to the minimum floor ($\approx 2{,}880$), remaining flat and stable for the remaining $850$ steps without oscillating or diverging.
  4. **What Would Happen at $\eta = 1.0$?**
   * Because $\eta$ would exceed the stability bound ($\eta > \frac{2}{\lambda_{\max}}$), the updates would overshoot the opposite wall of the quadratic bowl, causing loss values to oscillate wildly and diverge to infinity (`NaN`).
  
  ---
## Summary Review Questions for Section 5
  
  1. *Why does selecting the regularization parameter $\alpha$ based on training set error always result in choosing $\alpha = 0$?*
  2. *Why is searching across a log-spaced grid (`np.logspace(-4, 4, 50)`) statistically and computationally superior to searching across a linearly spaced grid (`np.linspace(0.0001, 10000, 50)`)?*
  3. *In the `batch_gd()` implementation on Slide 14, explain why the gradient formula contains the factor `(2 / n_samples) * X.T @ error` rather than simply `X.T @ error`.*
  
  ---
## Complete Lecture 11 Synthesis Reference
  
  | Optimization & Regularization Concept | Mathematical Formulation | Systems & Production Impact |
  | :--- | :--- | :--- |
  | **Gradient Descent** | $w^{(t+1)} = w^{(t)} - \eta \nabla J(w^{(t)})$ | First-order iterative descent down the loss surface; converges to global minimum on convex surfaces. |
  | **Learning Rate Diagnostics** | $\eta$ too high (oscillates/diverges); $\eta$ too low (stalls); optimal (smooth exponential decay). | Governed by Hessian curvature: must satisfy $\eta < \frac{2}{\lambda_{\max}(H)}$ to prevent divergence. |
  | **Batch GD ($B = n$)** | Full expectation over all $n$ samples. Stable, but $\mathcal{O}(n \cdot d)$ per step. | Computationally prohibitive for large datasets; does not fit in GPU memory. |
  | **Stochastic GD ($B = 1$)** | Single-sample update. Unbiased estimator with extreme gradient variance. | High variance escapes shallow non-convex minima; poor GPU hardware utilization. |
  | **Mini-Batch GD ($B = 32\text{–}512$)** | Approximates gradient over subset $\mathcal{B}$. Standard in PyTorch `DataLoader`. | Balances variance reduction ($\text{Var} \propto \Sigma/B$) with peak SIMT GPU matrix throughput (GEMM). |
  | **The Need to Regularize** | High capacity ($d \approx n$) interpolates noise, causing parameter explosion and high variance. | Augments loss with complexity penalty $\Omega(w)$; balances the bias-variance trade-off. |
  | **Ridge Regression ($L_2$)** | $J(w) = \|y - Xw\|_2^2 + \alpha \|w\|_2^2 \implies w^* = (X^T X + \alpha I)^{-1} X^T y$. | $\alpha I$ shifts eigenvalues ($\lambda_i + \alpha > 0$), guaranteeing invertibility even when $d \gg n$. |
  | **Lasso Regression ($L_1$)** | $J(w) = \|y - Xw\|_2^2 + \alpha \|w\|_1$. Solved via Cyclic Coordinate Descent. | Sharp vertices on coordinate axes hit expanding loss ellipses first, forcing weights to **exact zero**. |
  | **Soft-Thresholding** | $w_j^* = \text{sign}(\rho_j)\max(0, |\rho_j| - \alpha)$. | Features with correlation $|\rho_j| \le \alpha$ fall into the dead zone and are pruned (feature selection). |
  | **Hyperparameter Grid** | Logarithmic search: `np.logspace(-4, 4, 50)`. | Evaluates geometric orders of magnitude; must be tuned via cross-validation, never training loss. |
  | **Scaling Prerequisite** | Standardize features ($\mu=0, \sigma=1$) before applying Ridge or Lasso. | Unscaled features distort regularization penalties, unfairly penalizing small-scale variables. |
  
  ---