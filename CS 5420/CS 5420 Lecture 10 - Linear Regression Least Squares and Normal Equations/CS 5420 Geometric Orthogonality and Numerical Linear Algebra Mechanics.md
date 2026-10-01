## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Orthogonality of Residuals and Zero Correlation with Predictions:**
  * Setting the gradient of the loss function to zero yields the Normal Equations:
   $$\nabla_w J(w) = -2 X^T (y - Xw^*) = \mathbf{0} \implies X^T r = \mathbf{0}$$
  * The matrix product $X^T r$ evaluates the dot product between every individual column vector of $X$ (denoted $x_{(j)}$) and the residual vector $r$:
   $$\langle x_{(j)}, \, r \rangle = x_{(j)}^T r = 0 \quad \forall \; j \in \{0, 1, \dots, d\}$$
  * Because predictions are a linear combination of the columns of $X$ ($\hat{y} = Xw^*$), the inner product between predictions and residuals is:
   $$\langle \hat{y}, \, r \rangle = (Xw^*)^T r = (w^*)^T (X^T r) = (w^*)^T \mathbf{0} = 0$$
  * Furthermore, because the intercept column ensures $\bar{r} = 0$, the empirical covariance is:
   $$\text{Cov}(\hat{y}, r) = \frac{1}{n}\sum_{i=1}^n (\hat{y}_i - \bar{\hat{y}})(r_i - 0) = \frac{1}{n} \hat{y}^T r - \bar{\hat{y}}\bar{r} = 0 - 0 = 0$$
   Predictions and residuals have **identically zero Pearson correlation**.
  2. **Failure of Strict Positive Definiteness in the Hessian:**
  * The Hessian matrix $H = 2 X^T X \in \mathbb{R}^{(d+1) \times (d+1)}$ fails to be strictly positive definite when $X^T X$ is singular (rank-deficient).
  * This occurs when:
   1. The sample size is smaller than the feature dimension ($n < d+1$, the $p > n$ regime).
   2. Features are linearly dependent (e.g., exact multicollinearity, the dummy variable trap).
  * In this setting, $H$ is only positive semi-definite ($H \succeq 0$), meaning it possesses at least one zero eigenvalue ($\lambda = 0$). 
  * **Geometric & Optimization Impact:** The loss surface ceases to be a strictly convex bowl and becomes an elongated flat-bottomed parabolic valley. The global minimum is no longer unique; there exist infinitely many parameter vectors $w$ that achieve the identical minimal empirical loss.
  3. **The Centroid Passing Theorem:**
  * The regression line is guaranteed to pass through $(\bar{x}, \bar{y})$ because differentiating with respect to the intercept $w_0$ enforces:
   $$\frac{\partial J}{\partial w_0} = -2\sum_{i=1}^n (y_i - w_0 - w_1 x_i) = 0 \implies \bar{y} - w_0 - w_1 \bar{x} = 0$$
   $$\mathbf{w_0 = \bar{y} - w_1 \bar{x}}$$
  * Evaluating the fitted model at $x = \bar{x}$:
   $$\hat{y}(\bar{x}) = w_0 + w_1 \bar{x} = (\bar{y} - w_1 \bar{x}) + w_1 \bar{x} \equiv \bar{y}$$
  * The intercept parameter $w_0$ functions as an adaptive offset that anchors the regression hyperplane to the center of mass of the observed data.
  
  ---
## 2. The Geometric Meaning of "Normal": Orthogonal Projections (Slide 14)
  
  Slide 14 clarifies the geometric terminology:
  
  $$\mathbf{\text{"'Normal' here means perpendicular, not ordinary: the residual vector } y - Xw \text{ is orthogonal to every column of } X\text{."}}$$
  
  ```
                      The Subspace Projection Geometry (Slide 14)
            Observation Vector y (in ℝⁿ)
                      ▲
                      │ \
                      │   \  Residual Vector r = y - ŷ
                      │     \  (PERPENDICULAR / NORMAL to the plane!)
                      │       \
                      │         ▼
   ───────────────────┼──────────●──────────────────────────────────── Column Space Col(X)
                      │          ŷ = Xw* (Orthogonal Projection)       (Subspace in ℝⁿ)
                      │         /
                      │       /
                      │     /
                      ▼   ▼
                     x₍₁₎  x₍₂₎ (Column Vectors spanning the subspace)
  ```
  
  ---
### The Vector Space Formulation:
  To understand linear regression geometrically, we must invert our mental perspective from sample space ($\mathbb{R}^d$) to observation space ($\mathbb{R}^n$):
  * The ground-truth target vector $y \in \mathbb{R}^n$ is a single point in an $n$-dimensional coordinate space.
  * The columns of the design matrix $X = [x_{(0)}, x_{(1)}, \dots, x_{(d)}]$ are vectors in $\mathbb{R}^n$.
  * The set of all possible model predictions forms the **Column Space of $X$** (denoted $\text{Col}(X)$), which is a $(d+1)$-dimensional linear subspace embedded inside $\mathbb{R}^n$:
  $$\text{Col}(X) = \left\{ \hat{y} \in \mathbb{R}^n \;\middle|\; \hat{y} = Xw = \sum_{j=0}^d w_j x_{(j)}, \quad w \in \mathbb{R}^{d+1} \right\}$$
#### Why the System Cannot Be Solved Exactly:
  Because the number of observations far exceeds the number of parameters ($n \gg d+1$), the column space $\text{Col}(X)$ is a low-dimensional flat hyperplane cutting through a massive $n$-dimensional space. 
  * The observed vector $y$ almost never lies directly inside $\text{Col}(X)$.
  * The exact equation $Xw = y$ is an **overdetermined linear system** with zero solutions.
#### The Orthogonal Projection Theorem:
  The least-squares objective seeks the parameter vector $w$ that minimizes the Euclidean distance between $y$ and the subspace:
  
  $$\min_{w} \|y - Xw\|_2$$
  
  By the **Hilbert Projection Theorem**, the shortest distance from an external point $y$ to a closed subspace $\text{Col}(X)$ is achieved by the **orthogonal projection** of $y$ onto $\text{Col}(X)$:
  1. The prediction $\hat{y} = Xw^*$ is the unique shadow cast by $y$ perpendicularly onto the subspace.
  2. The error vector $r = y - Xw^*$ must be orthogonal (normal) to every vector lying inside $\text{Col}(X)$.
  3. Because every column vector $x_{(j)}$ lies in $\text{Col}(X)$, their inner products with the residual must vanish:
   $$x_{(j)}^T (y - Xw^*) = 0 \quad \forall \; j \implies \mathbf{X^T(y - Xw) = \mathbf{0} \implies X^TXw = X^Ty}$$
  
  $$\mathbf{\text{The Normal Equations are simply the algebraic statement that the error vector is orthogonal to the model's subspace.}}$$
  
  ---
### The Hat Matrix (Orthogonal Projector)
  Substituting the optimal weights $w^* = (X^T X)^{-1} X^T y$ back into the prediction equation:
  
  $$\hat{y} = X w^* = X (X^T X)^{-1} X^T y$$
  
  We define the **Hat Matrix** (or Projection Matrix) $H \in \mathbb{R}^{n \times n}$:
  
  $$\mathbf{H = X(X^T X)^{-1} X^T} \implies \mathbf{\hat{y} = H y}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Algebraic Properties of the Hat Matrix H:                              │
  │                                                                        │
  │ 1. Symmetric:                                                          │
  │    Hᵀ = (X(XᵀX)⁻¹Xᵀ)ᵀ = X((XᵀX)⁻¹)ᵀXᵀ = X(XᵀX)⁻¹Xᵀ = H                │
  │                                                                        │
  │ 2. Idempotent (Projection Invariance):                                 │
  │    H² = H · H = [X(XᵀX)⁻¹Xᵀ] [X(XᵀX)⁻¹Xᵀ]                             │
  │       = X(XᵀX)⁻¹ [(XᵀX)(XᵀX)⁻¹] Xᵀ = X(XᵀX)⁻¹ [I] Xᵀ = H               │
  │    Projecting an already-projected vector changes nothing: H(Hy) = Hy. │
  │                                                                        │
  │ 3. Residual Projection Operator:                                       │
  │    r = y - ŷ = y - Hy = (I - H)y                                       │
  │    (I - H) is also symmetric and idempotent, projecting y onto the     │
  │    orthogonal complement subspace Col(X)ᗮ.                             │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Matrix Calculation: The "Code Smell" of `inv()` (Slide 15)
  
  Slide 15 outlines a common software defect in applied machine learning:
  
  $$\mathbf{\text{"Writing } (X^T X)^{-1} X^T y \text{ on paper is correct. Typing np.linalg.inv(X.T@X) @ X.T @ y is a code smell."}}$$
  
  ```
                       The Inversion vs. Solver Divide (Slide 15)
              AVOID (Anti-Pattern)                          DEFAULT (Production Standard)
    ┌──────────────────────────────────────┐    ┌──────────────────────────────────────┐
    │ np.linalg.inv(A) @ b                 │    │ np.linalg.solve(A, b)                │
    ├──────────────────────────────────────┤    ├──────────────────────────────────────┤
    │ • Forms the full explicit inverse    │    │ • LU / Cholesky factorization        │
    │ • Numerically worse conditioned      │    │ • Better conditioned; highly stable  │
    │ • ~2x to 3x slower (computes 2n³ FLOPs│   │ • Faster (~2/3 n³ FLOPs)             │
    │ • Signals lack of numerical rigor    │    │ • Solves linear system directly      │
    └──────────────────────────────────────┘    └──────────────────────────────────────┘
  ```
  
  ---
### The Three Numerical Pitfalls of `np.linalg.inv`:
#### 1. Unnecessary Floating-Point Operations ($\approx 3\times$ Compute Penalty)
  * To compute $w = A^{-1} b$ where $A = X^T X \in \mathbb{R}^{m \times m}$ (with $m = d+1$):
  * Computing the explicit matrix inverse $A^{-1}$ via Gauss-Jordan elimination requires approximately **$2 m^3$ floating-point operations (FLOPs)**.
  * Performing the subsequent matrix-vector multiplication $A^{-1} b$ requires an additional **$2 m^2$ FLOPs**.
  * By contrast, Gaussian elimination with partial pivoting (**LU Decomposition**) solves $A w = b$ directly in:
  $$\text{FLOPs}_{\text{solve}} \approx \mathbf{\frac{2}{3} m^3}$$
  * Computing the explicit inverse does **three times more arithmetic work** than solving the system directly.
#### 2. Numerical Precision Loss & Condition Number Squaring
  In finite-precision computer systems (IEEE 754 floating-point), rounding errors are governed by the condition number $\kappa(A) = \|A\| \|A^{-1}\|$.
  * When computing $A^{-1}$ explicitly, small round-off errors in matrix elements can produce large errors in the inverted entries.
  * The standard perturbation bound for solving $Ax = b$ via backward-stable factorization satisfies:
  $$\frac{\|\hat{w} - w^*\|}{\|w^*\|} \le \mathcal{O}\left( \kappa(A) \cdot \epsilon_{\text{machine}} \right)$$
  * Explicit matrix inversion introduces intermediate rounding steps that can amplify this numerical error, leading to inaccurate solutions on ill-conditioned data.
#### 3. Unnecessary Memory Allocation
  * `np.linalg.inv(A)` allocates a brand new $(d+1) \times (d+1)$ contiguous floating-point array on the heap to store $A^{-1}$.
  * After computing the dot product with $b$, that matrix is discarded. `np.linalg.solve` operates in-place or uses minimal scratch space.
  
  ---
## 4. The Three Solvers: `inv` vs. `solve` vs. `lstsq` (Slide 15)
  
  Slide 15 introduces three ways to solve linear systems in Python:
  
  ```
  ┌────────────────────┬─────────────────────────┬──────────────────┬────────────────────────────┐
  │ Implementation     │ Underlying Algorithm    │ Computational Cost│ Behavior on Singular XᵀX   │
  ├────────────────────┼─────────────────────────┼──────────────────┼────────────────────────────┤
  │ np.linalg.inv(A)@b │ Gauss-Jordan Inversion  │ ~2 m³ FLOPs      │ Raises LinAlgError         │
  │                    │                         │ (Slowest)        │ (or emits extreme noise)   │
  ├────────────────────┼─────────────────────────┼──────────────────┼────────────────────────────┤
  │ np.linalg.solve(A,b│ LU Decomposition        │ ~2/3 m³ FLOPs    │ Raises LinAlgError         │
  │                    │ with Partial Pivoting   │ (Fastest)        │ (Strict full-rank check)   │
  ├────────────────────┼─────────────────────────┼──────────────────┼────────────────────────────┤
  │ np.linalg.lstsq(X,y│ Singular Value          │ ~2 n m² + m³     │ SURVIVES! Silently returns │
  │                    │ Decomposition (SVD)     │ (Robust standard)│ minimum L2-norm solution   │
  └────────────────────┴─────────────────────────┴──────────────────┴────────────────────────────┘
  ```
  
  ---
### The Third Option: `np.linalg.lstsq` (The Industrial Standard)
  Slide 15 notes:
  $$\mathbf{\text{"Third option: np.linalg.lstsq — survives singular } X^T X \text{ by returning the minimum-norm solution."}}$$
  
  What happens if $X^T X$ is not invertible (e.g., $n < d+1$, or two features are collinear)?
  * Both `np.linalg.inv` and `np.linalg.solve` will crash, throwing a fatal `LinAlgError: Singular matrix`.
  * `np.linalg.lstsq(X, y)` bypasses forming $X^T X$ entirely. Instead, it computes the **Singular Value Decomposition (SVD)** of the design matrix $X$:
  $$X = U \Sigma V^T$$
  * It calculates the **Moore-Penrose Pseudoinverse** ($X^+$):
  $$X^+ = V \Sigma^+ U^T$$
  where $\Sigma^+$ is formed by taking the reciprocal of non-zero singular values ($\frac{1}{\sigma_i}$) and setting all zero (or sub-threshold) singular values strictly to $0$.
  * **The Mathematical Guarantee:** If infinitely many solutions exist, `lstsq` selects the unique parameter vector $w^*$ that minimizes parameter norm:
  $$\arg\min_w \|w\|_2 \quad \text{subject to } w \in \arg\min_w \|y - Xw\|_2^2$$
  This is why production libraries (including `scipy.linalg.lstsq` and `sklearn.linear_model.LinearRegression`) use SVD routines under the hood.
  
  ---