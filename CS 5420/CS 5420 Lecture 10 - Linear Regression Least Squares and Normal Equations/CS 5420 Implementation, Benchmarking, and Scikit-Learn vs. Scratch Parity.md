## 1. Quick Review: Answers to Section 4 Checkpoints
  
  1. **Geometric Necessity of Orthogonal Residuals:**
  * In observation space $\mathbb{R}^n$, the set of all possible model predictions forms the subspace $\text{Col}(X)$. 
  * By the Hilbert Projection Theorem, the distance $\|y - \hat{y}\|_2$ between the target vector $y$ and the subspace is minimized **if and only if** the residual vector $r = y - \hat{y}$ is strictly perpendicular to that subspace:
   $$r \perp \text{Col}(X) \iff X^T r = \mathbf{0}$$
  * If $r$ had any non-zero projection onto $\text{Col}(X)$, we could adjust the weight vector $w$ along that projection vector to further reduce the Euclidean error, contradicting the premise of optimality.
  2. **Idempotency of the Hat Matrix ($H^2 = H$):**
  * Using the algebraic definition $H = X(X^T X)^{-1} X^T$:
   $$H^2 = \Big[X(X^T X)^{-1} X^T\Big] \Big[X(X^T X)^{-1} X^T\Big] = X(X^T X)^{-1} \Big[(X^T X)(X^T X)^{-1}\Big] X^T = X(X^T X)^{-1} [I] X^T = H$$
  * **Physical Meaning:** $H$ is an **orthogonal projection operator**. It projects any vector in $\mathbb{R}^n$ onto the column space $\text{Col}(X)$. Once a vector is already inside $\text{Col}(X)$ (such as the prediction $\hat{y}$), projecting it a second time changes nothing:
   $$H \hat{y} = H(Hy) = H^2 y = Hy = \hat{y}$$
  3. **Computational Speedup of `solve()` over `inv()`:**
  * Computing the explicit matrix inverse $A^{-1}$ via Gauss-Jordan elimination requires approximately **$2 m^3$ floating-point operations (FLOPs)**, followed by an additional **$2 m^2$ FLOPs** for the matrix-vector multiplication $A^{-1} b$.
  * `np.linalg.solve(A, b)` computes the solution directly via **LU decomposition with partial pivoting** ($PA = LU$) followed by forward and backward substitution, requiring only **$\frac{2}{3} m^3$ FLOPs**.
  * Taking the operational ratio:
   $$\frac{\text{Work}_{\text{inv}}}{\text{Work}_{\text{solve}}} \approx \frac{2 m^3}{\frac{2}{3} m^3} = \mathbf{3\times \text{ faster}}$$
  * Direct solving avoids intermediate rounding steps, is better conditioned, and eliminates unnecessary heap allocations.
  
  ---
## 2. Coding Lab A: The Scikit-Learn Reference Pipeline (Slides 16–18)
  
  Slides 16–18 establish the reference baseline on the **Diabetes dataset** (Efron et al., 2004):
  
  ```python
  import numpy as np
  import pandas as pd
  from sklearn.datasets import load_diabetes
  from sklearn.model_selection import train_test_split
  from sklearn.linear_model import LinearRegression
  from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
  
  # 1. Ingest Data (N = 442, d = 10 continuous clinical features)
  X, y = load_diabetes(return_X_y=True, as_frame=True)
  
  # 2. Strict Partition Barrier: Split FIRST before any modeling
  X_tr, X_te, y_tr, y_te = train_test_split(
    X, y, test_size=0.2, random_state=42
  )
  
  print(f"Training Matrix: {X_tr.shape} | Test Matrix: {X_te.shape}")
  # Outputs: (353, 10), (89, 10)
  ```
  
  ```
                     The Diabetes Benchmark Topology (Slide 16)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Dataset Origin: 442 diabetes patients measured across 10 baseline      │
  │                 demographic and clinical covariates.                   │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Sample Volume (N):     442 observations (353 Train / 89 Test)          │
  │ Feature Dimension (d): 10 features: age, sex, bmi, bp, s1..s6          │
  │ Pre-Conditioning:      Features are pre-standardized to unit variance  │
  │                        and zero mean (mean = 0, sum of squares = 1).   │
  │ Target Variable (y):   Quantitative continuous measure of disease      │
  │                        progression one year post-baseline.             │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Estimator Training & Multi-Metric Evaluation (Slide 18)
  
  ```python
  # Fit standard reference estimator
  model = LinearRegression().fit(X_tr, y_tr)
  pred = model.predict(X_te)
  
  print("intercept w0 :", round(model.intercept_, 3))
  print("RMSE         :", round(mean_squared_error(y_te, pred) ** 0.5, 2))
  print("MAE          :", round(mean_absolute_error(y_te, pred), 2))
  print("R^2          :", round(r2_score(y_te, pred), 4))
  ```
  
  ```
                 Scikit-Learn Regression Diagnostics (Slide 18)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Parameter / Metric             Evaluated Value   Interpretation        │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Intercept (w₀)                 151.346           Baseline progression  │
  │                                                  at average covariate  │
  │                                                  values.               │
  │                                                                        │
  │ Root Mean Squared Error (RMSE) 53.85             Standard deviation of │
  │                                                  unexplained residuals.│
  │                                                                        │
  │ Mean Absolute Error (MAE)      42.79             Expected magnitude of │
  │                                                  absolute deviation.   │
  │                                                                        │
  │ Coefficient of Det. (R²)       0.4526            Explains 45.26% of    │
  │                                                  variance over mean.   │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Mathematical Deconstruction of $R^2$ (Goodness of Fit)
  Slide 18 highlights $R^2 = 0.4526$. What does this metric represent?
  
  $$R^2 = 1 - \frac{\text{SS}_{\text{res}}}{\text{SS}_{\text{tot}}} = 1 - \frac{\sum_{i=1}^n (y_i - \hat{y}_i)^2}{\sum_{i=1}^n (y_i - \bar{y})^2}$$
  
  * **Total Sum of Squares ($\text{SS}_{\text{tot}}$):** The variance of the data around the baseline "predict-the-mean" model ($\hat{y} = \bar{y}$).
  * **Residual Sum of Squares ($\text{SS}_{\text{res}}$):** The unexplained variance remaining after fitting the OLS hyperplane.
  * **The Diagnostic Verdict:** An $R^2$ of **$0.4526$** indicates that the 10 clinical features reduce the prediction error variance by **$45.26\%$** compared to a naive model predicting the mean score ($151.35$) for every patient.
  
  ---
## 3. Coding Lab B: Building Least Squares from Scratch (Slides 19–21)
  
  Slide 19 outlines the implementation challenge:
  
  $$\mathbf{\text{"Same data. Same split. No sklearn model. Four lines of NumPy."}}$$
  $$\mathbf{\text{"If your answer matches sklearn to }\sim 10^{-10}\text{, you have proved that LinearRegression is the algebra you derived."}}$$
  
  ---
### The Four-Line NumPy Implementation (Slide 21)
  
  ```python
  def fit_least_squares(X, y):
    """
    Solves the Normal Equations (XᵀX) w = Xᵀy from scratch in 4 lines.
    Accepts Pandas DataFrames or raw NumPy arrays.
    """
    # 1. Augment design matrix with homogeneous column of ones for intercept w₀
    A = np.c_[np.ones(len(X)), np.asarray(X)]  # Shape: (n, d + 1)
    
    # 2. Compute the Gram matrix XᵀX
    XtX = A.T @ A                              # Shape: (d + 1, d + 1)
    
    # 3. Compute the projection vector Xᵀy
    Xty = A.T @ np.asarray(y)                  # Shape: (d + 1,)
    
    # 4. Solve the linear system directly via LU Decomposition (NEVER inv!)
    return np.linalg.solve(XtX, Xty)
  
  def predict(X, w):
    """
    Computes ŷ = Xw over unseen instances using the fitted weight vector.
    """
    A = np.c_[np.ones(len(X)), np.asarray(X)]
    return A @ w
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Why This Implementation Is Minimal & Robust:                           │
  │                                                                        │
  │ • np.asarray(X): Strips DataFrame headers and metadata, operating      │
  │   directly over contiguous C memory buffers with zero copy overhead.   │
  │                                                                        │
  │ • np.c_: Column-stacks the vector of ones without manual broadcasting. │
  │                                                                        │
  │ • @ Operator: Dispatches directly to compiled BLAS GEMM / GEMV         │
  │   routines (OpenBLAS / Intel MKL).                                     │
  │                                                                        │
  │ • np.linalg.solve: Executes LAPACK _gesv (LU factorization with partial│
  │   pivoting), avoiding the computational and numerical penalties of inv.│
  │                                                                        │
  │ • Notice what is NOT here: No learning rate (α), no epoch loops, no   │
  │   convergence tolerances. A single, exact analytical solve.            │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. The Parity Test: Verification to Eleven Decimals (Slide 22)
  
  Slide 22 executes the verification script comparing the scratch solver to `scikit-learn`:
  
  ```python
  # 1. Fit custom implementation on identical training data
  w = fit_least_squares(X_tr, y_tr)
  
  # 2. Evaluate predictions on test data
  test_preds = predict(X_te, w)
  
  print("my w0  :", round(w[0], 6))
  print("my R^2 :", round(r2_score(y_te, test_preds), 6))
  
  # 3. Calculate absolute maximum deviation across all parameter weights
  sk_w = np.r_[model.intercept_, model.coef_]
  max_diff = np.max(np.abs(w - sk_w))
  print(f"Maximum Weight Discrepancy: {max_diff:.2e}")
  ```
  
  ```
                  Scratch vs. Scikit-Learn Verification (Slide 22)
  ┌──────────────────────────────┬────────────────────────┬────────────────┐
  │ Model Parameter / Metric     │ Scikit-Learn           │ Scratch NumPy  │
  ├──────────────────────────────┼────────────────────────┼────────────────┤
  │ Intercept (w₀)               │ 151.345605             │ 151.345605     │
  │ Test Generalization (R²)     │ 0.452603               │ 0.452603       │
  ├──────────────────────────────┴────────────────────────┴────────────────┤
  │ Maximum Parameter Error:     max |w_scratch - w_sk| ≈ 7.11 × 10⁻¹¹     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Why Isn't the Difference Identically Zero? (Slide 22)
  Slide 22 notes:
  $$\mathbf{\text{"Not zero — floating point never gives you zero. But eleven digits of agreement is proof of the same estimator."}}$$
  
  In IEEE 754 double-precision arithmetic, floating-point numbers have 53 bits of significand precision ($\approx 15\text{–}17$ significant decimal digits, machine epsilon $\epsilon_{\text{mach}} \approx 2.22 \times 10^{-16}$).
  
  The residual discrepancy ($\approx 7 \times 10^{-11}$) occurs because `scikit-learn` and our NumPy script execute different numerical algorithms under the hood:
  * Our scratch code executes **LU Decomposition** on the square Gram matrix $A^T A$ via `np.linalg.solve`.
  * `scikit-learn` centers the data internally and computes a **Singular Value Decomposition (SVD)** directly on the data matrix via `scipy.linalg.lstsq`.
  * Because floating-point addition is non-associative, differing sequences of arithmetic operations produce minute discrepancies in the least significant bits. Eleven decimal places of agreement proves that `LinearRegression` implements the exact Normal Equations derived in Section 3.
  
  ---
## 5. Architectural Comparison: Package vs. Scratch (Slide 23)
  
  Slide 23 provides a comparative summary of production package mechanics vs. our scratch implementation:
  
  ```
  ┌─────────────────┬────────────────────────────┬────────────────────────────┬────────────────────────┐
  │ Dimension       │ sklearn.LinearRegression   │ your fit_least_squares     │ Graduate Systems       │
  │                 │                            │                            │ Engineering Verdict    │
  ├─────────────────┼────────────────────────────┼────────────────────────────┼────────────────────────┤
  │ What It         │ ||y - Xw||²                │ ||y - Xw||²                │ IDENTICAL              │
  │ Optimizes       │ (Sum of Squared Errors)    │ (Sum of Squared Errors)    │ Mathematical loss is   │
  │                 │                            │                            │ the exact same bowl.   │
  ├─────────────────┼────────────────────────────┼────────────────────────────┼────────────────────────┤
  │ How It          │ scipy.linalg.lstsq         │ np.linalg.solve            │ DIFFERENT ROUTE        │
  │ Solves          │ (SVD Factorization)        │ (LU Decomposition)         │ SVD avoids forming     │
  │                 │                            │                            │ XᵀX explicitly.        │
  ├─────────────────┼────────────────────────────┼────────────────────────────┼────────────────────────┤
  │ Behavior on     │ Silently returns           │ Raises LinAlgError         │ SKLEARN IS SAFER       │
  │ Singular XᵀX    │ minimum-norm solution w    │ (or outputs extreme noise) │ SVD handles rank       │
  │                 │ via Moore-Penrose pseudo.  │                            │ deficiency gracefully. │
  ├─────────────────┼────────────────────────────┼────────────────────────────┼────────────────────────┤
  │ Intercept       │ fit_intercept=True         │ Prepends column of 1s      │ SAME EFFECT            │
  │ Handling        │ (Centers data internally)  │ directly into matrix A     │ Mathematically equal;  │
  │                 │                            │                            │ sklearn saves memory.  │
  ├─────────────────┼────────────────────────────┼────────────────────────────┼────────────────────────┤
  │ Agreement       │ —                          │ —                          │ max |diff| ≈ 7 × 10⁻¹¹ │
  │ Bound           │                            │                            │ (Machine precision).   │
  └─────────────────┴────────────────────────────┴────────────────────────────┴────────────────────────┘
  ```
  
  ---
### Deep Dive: How Scikit-Learn Handles Intercepts Efficiently
  Why does `LinearRegression` use `fit_intercept=True` rather than prepending a column of ones?
  1. **Condition Number Optimization:** Adding a column of ones to an uncentered matrix can worsen the condition number $\kappa(X^T X)$, making the system more sensitive to numerical noise.
  2. **Mean-Centering Equivalence:** `scikit-learn` centers the training features and targets first:
   $$\tilde{X}_j = X_j - \bar{x}_j, \quad \tilde{y} = y - \bar{y}$$
  3. It solves the centered system without an intercept:
   $$w_{1:d} = (\tilde{X}^T \tilde{X})^{-1} \tilde{X}^T \tilde{y}$$
  4. It recovers the intercept analytically via the Centroid Theorem (derived in Section 3):
   $$w_0 = \bar{y} - \sum_{j=1}^d w_j \bar{x}_j$$
   This produces identical predictions while avoiding the need to allocate an augmented column of ones in memory.
  
  ---
## Summary Review Questions for Section 5
  
  1. *Why does `scikit-learn` use `scipy.linalg.lstsq` (SVD) rather than `np.linalg.solve` (LU decomposition) as its internal default solver for Ordinary Least Squares?*
  2. *If a design matrix contains two perfectly collinear features ($x_{(2)} = 3 x_{(1)}$), what will happen if you attempt to fit the model using `fit_least_squares()` vs. using `sklearn.linear_model.LinearRegression`?*
  3. *Explain why an $R^2$ score can theoretically drop below $0.0$ on a test set, and what that indicates about the model's predictive performance relative to the training set mean.*
  
  ---
## Complete Lecture 10 Synthesis Reference
  
  | Core Analytical Concept | Mathematical Formulation | Computational & Systems Reality |
  | :--- | :--- | :--- |
  | **Residual Definition** | $r_i = y_i - \hat{y}_i = y_i - x_i^T w$ | The signed vertical error between ground truth and the estimated hyperplane. |
  | **Least Squares Rule** | $\min_w \sum_{i=1}^n r_i^2 = \|y - Xw\|_2^2$ | A mathematical choice of loss function; optimal under Gaussian measurement noise. |
  | **Homogeneous Coordinate** | $\tilde{x} = [1, x_1, \dots, x_d]^T$ | Absorbs the affine scalar intercept $w_0$ into the weight vector: $\hat{y} = Xw$. |
  | **Linearity Definition** | $\frac{\partial f(x; w)}{\partial w_j} = \phi_j(x)$ | Linear in the parameters $w$, not features; accommodates polynomial/spline bases. |
  | **$L_2$ vs. $L_1$ Trade-Off** | $L_2$ (Smooth, fits mean, closed-form) vs. $L_1$ (Kink at 0, fits median, robust). | Squared loss penalties grow quadratically ($30^2 = 900$), making OLS vulnerable to outliers. |
  | **Loss Surface Convexity** | $H = \nabla_w^2 J(w) = 2 X^T X \succeq 0$ | Strictly convex bowl when $\text{rank}(X) = d+1$; guarantees that stationary points are global minima. |
  | **Zero-Sum Residuals** | $\sum_{i=1}^n r_i = 0 \implies \bar{r} = 0$ | Enforced by $\frac{\partial J}{\partial w_0} = 0$; OLS residuals sum to zero on the training set. |
  | **Centroid Passing Theorem** | $w_0 = \bar{y} - w_1 \bar{x} \implies \hat{y}(\bar{x}) = \bar{y}$ | The fitted OLS line is mathematically guaranteed to pass through $(\bar{x}, \bar{y})$. |
  | **Orthogonality of Residuals**| $X^T (y - Xw) = \mathbf{0} \implies X^T r = \mathbf{0}$ | The residual vector is perpendicular to every column vector in $\text{Col}(X)$. |
  | **The Normal Equations** | $X^T X w = X^T y$ | The analytical solution for Ordinary Least Squares; $d+1$ equations in $d+1$ unknowns. |
  | **Hat Matrix Operator** | $H = X(X^T X)^{-1} X^T$ | Symmetric and idempotent ($H^2 = H$); projects $y$ orthogonally onto $\text{Col}(X)$. |
  | **Numerical Solvers** | `solve` (LU, $\frac{2}{3}m^3$) vs. `inv` (Gauss-Jordan, $2m^3$). | Inverting $X^T X$ directly is a code smell. Use `solve` for full rank; use `lstsq` for singular systems. |
  | **Numerical Parity** | $\max \|w_{\text{scratch}} - w_{\text{sklearn}}\| \approx 7 \times 10^{-11}$ | Agreement across 11 decimal places confirms empirical implementation of the Normal Equations. |
  
  ---