## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why $\alpha I$ Guarantees Invertibility in Ridge Regression:**
  * By the Spectral Theorem, the Gram matrix $X^T X \in \mathbb{R}^{m \times m}$ is symmetric positive semi-definite with non-negative real eigenvalues:
   $$\lambda_i(X^T X) \ge 0 \quad \forall \; i \in \{1, \dots, m\}$$
  * When regularized via $\alpha I$ ($\alpha > 0$), the eigenvalues shift uniformly:
   $$\lambda_i(X^T X + \alpha I) = \lambda_i(X^T X) + \alpha \ge \alpha > 0$$
  * Because every eigenvalue is strictly positive, the determinant is strictly positive:
   $$\det(X^T X + \alpha I) = \prod_{i=1}^m (\lambda_i + \alpha) \ge \alpha^m > 0$$
  * The matrix is guaranteed to be strictly positive definite and invertible, even if $d \gg n$ or features are perfectly collinear.
  2. **SVD Shrinkage of Low-Variance vs. High-Variance Directions:**
  * The Ridge parameter expansion scales components by the factor $f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha}$, where $\sigma_j$ is the singular value of the $j$-th principal data direction.
  * For **high-variance informative directions** ($\sigma_j^2 \gg \alpha$), $f_j \approx \frac{\sigma_j^2}{\sigma_j^2} = 1.0$, preserving the signal with negligible shrinkage.
  * For **low-variance collinear directions** ($\sigma_j^2 \ll \alpha$), $f_j \approx \frac{\sigma_j^2}{\alpha} \to 0$, suppressing the components that would otherwise cause parameter explosion in unconstrained OLS.
  3. **Why Ridge Cannot Perform Feature Selection:**
  * For any finite regularization strength $\alpha > 0$ and non-zero data singular value $\sigma_j > 0$, the shrinkage factor is strictly non-zero:
   $$f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha} > 0$$
  * Parameter weights are decayed asymptotically toward zero, but **never equal zero identically**. Every input feature remains active with a non-zero weight, meaning Ridge cannot perform sparse feature selection.
  
  ---
## 2. Lasso Regression: The $L_1$ Penalty (Slide 10)
  
  Slide 10 introduces the Least Absolute Shrinkage and Selection Operator (**Lasso**, Tibshirani 1996):
  
  $$\mathbf{J(w) = \|y - Xw\|_2^2 + \alpha \|w\|_1 = \sum_{i=1}^n (y_i - x_i^T w)^2 + \alpha \sum_{j=1}^d |w_j|}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Core Characteristics of Lasso (Slide 10):                              │
  │                                                                        │
  │ 1. No Closed-Form Solution:                                            │
  │    The absolute value penalty |w_j| introduces non-differentiable      │
  │    corners at w_j = 0; cannot be solved via a single matrix inverse.   │
  │                                                                        │
  │ 2. Solved via Coordinate Descent:                                      │
  │    Optimized iteratively along one parameter coordinate at a time.     │
  │                                                                        │
  │ 3. Automatic Feature Selection (Exact Sparsity):                       │
  │    Drives a subset of coefficients EXACTLY to zero (w_j = 0.0).        │
  │                                                                        │
  │ 4. Geometric Mechanism:                                                │
  │    The L1 constraint region forms sharp vertices on the axes.          │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Dueling Geometries: Why $L_1$ Yields Sparsity and $L_2$ Does Not (Slide 11)
  
  Slide 11 illustrates the geometric argument explaining why Lasso produces exact zeros while Ridge produces smooth shrinkage:
  
  ```
                      Two Penalties, Two Geometries (Slide 11)
               L1 Penalty (Lasso)                              L2 Penalty (Ridge)
               |w₁| + |w₂| ≤ c                                 w₁² + w₂² ≤ c
          w_2                                             w_2
           ▲                                               ▲
           │          ( ( ( ● ) ) )                        │          ( ( ( ● ) ) )
           │         /    w_OLS                            │         /    w_OLS
           │        /                                      │        /
           ● w* (TANGENCY AT CORNER!)                      │       /
          /│\                                              │    ╭──●──╮ w* (TANGENCY ON SMOOTH CURVE!)
         / │ \   w₁* = 0                                   │   │   │   │  Both w₁*, w₂* ≠ 0
        /  │  \                                            │    ╰──┴──╯
       ────┼────▶ w_1                                     ─────┼─────▶ w_1
        \  │  /                                                │
         \ │ /   Constraint Region:                             │    Constraint Region:
          \│/    Cross-Polytope / Diamond                      │    Hypersphere / Disk
  ```
  
  ---
### The Constrained Optimization Equivalence (KKT Formulations)
  By Lagrangian duality, the unconstrained penalized loss functions can be re-written as constrained optimization problems with budget parameter $c > 0$:
  
  $$\text{Ridge: } \min_w \|y - Xw\|_2^2 \quad \text{subject to } \mathbf{\sum_{j=1}^d w_j^2 \le c}$$
  $$\text{Lasso: } \min_w \|y - Xw\|_2^2 \quad \text{subject to } \mathbf{\sum_{j=1}^d |w_j| \le c}$$
  
  There is a one-to-one monotonic inverse mapping between the penalty weight $\alpha$ and the budget $c$:
  $$\alpha \to 0 \iff c \to \infty \quad \text{and} \quad \alpha \to \infty \iff c \to 0$$
  
  ---
### The Geometric Tangency Argument:
  1. **The Unconstrained Target ($w_{\text{OLS}}$):**
   * The unconstrained OLS minimum $w_{\text{OLS}} = (X^T X)^{-1} X^T y$ sits outside the constrained region when regularization is active ($c < \|w_{\text{OLS}}\|_p$).
   * The level sets of the squared error loss form **concentric hyper-ellipsoids** centered at $w_{\text{OLS}}$:
     $$\|y - Xw\|_2^2 = k$$
  2. **The Constrained Optimum ($w^*$):**
   * As we expand the loss contour from $w_{\text{OLS}}$, the optimal regularized solution $w^*$ occurs at the **first point of contact (tangency)** between the expanding loss ellipse and the constraint boundary.
  3. **Why $L_2$ Yields Non-Zero Weights (The Smooth Boundary):**
   * The $L_2$ boundary $w_1^2 + w_2^2 \le c$ is a smooth hypersphere with continuous tangent vectors everywhere.
   * The expanding loss ellipse will almost certainly touch the sphere at a point where the surface normal aligns with the loss gradient.
   * Touching the constraint boundary precisely at the coordinate intersection where an axis crosses the circle ($w_1 = 0$ or $w_2 = 0$) is a **measure-zero event**. Both coefficients remain non-zero.
  4. **Why $L_1$ Yields Exact Zeros (The Sharp Vertices):**
   * The $L_1$ boundary $|w_1| + |w_2| \le c$ forms a diamond (cross-polytope) featuring **sharp corners (vertices) positioned directly on the coordinate axes**.
   * At these vertices, the boundary is non-differentiable; it supports a wide fan of subgradient normal vectors.
   * As the loss ellipse expands, it is geometrically far more likely to make contact with a sharp protruding corner on an axis than with a flat edge.
   * Contacting the vertex on the $w_2$-axis sets **$w_1 \equiv 0.0$ identically**, eliminating that feature from the model.
  
  ---
## 4. Solving Lasso: Coordinate Descent & The Soft-Thresholding Operator (Slide 10)
  
  Slide 10 states:
  $$\mathbf{\text{"No closed form; solved by coordinate descent."}}$$
  
  Because the $L_1$ norm is non-differentiable whenever any $w_j = 0$, the gradient does not exist. However, the $L_1$ norm is **separable across coordinates**:
  
  $$\|w\|_1 = \sum_{j=1}^d |w_j| = |w_1| + |w_2| + \dots + |w_d|$$
  
  This separability allows us to solve Lasso using **Cyclic Coordinate Descent** (Friedman et al., 2007; the core algorithm inside `sklearn.linear_model.Lasso` and `glmnet`).
  
  ---
### Coordinate Descent Mechanics:
  Instead of updating all weights simultaneously via gradient descent, coordinate descent minimizes the objective with respect to **one parameter $w_j$ at a time**, holding all other $d-1$ parameters fixed:
  
  $$\min_{w_j} J(w_j \mid w_1, \dots, w_{j-1}, w_{j+1}, \dots, w_d)$$
  
  Assuming features are standardized ($\frac{1}{n} x_j^T x_j = 1$), isolate the contribution of feature $j$:
  
  $$r_{-j} = y - \sum_{k \neq j} w_k x_k \quad (\text{Partial Residual without feature } j)$$
  
  The objective for coordinate $j$ simplifies to:
  
  $$J(w_j) = \frac{1}{2} (w_j - \rho_j)^2 + \alpha |w_j|$$
  
  where $\rho_j = x_j^T r_{-j}$ represents the correlation between feature $j$ and the partial residual.
  
  ---
### Deriving the Soft-Thresholding Operator:
  Because $|w_j|$ has a kink at zero, we evaluate the **subgradient optimality condition**:
  
  $$0 \in \partial_{w_j} J(w_j) \implies 0 \in (w_j - \rho_j) + \alpha \partial |w_j|$$
  
  where the subdifferential of the absolute value function is:
  
  $$\partial |w_j| = \begin{cases} \{+1\} & \text{if } w_j > 0 \\ [-1, 1] & \text{if } w_j = 0 \\ \{-1\} & \text{if } w_j < 0 \end{cases}$$
  
  ```
  Evaluating the Three Cases:
  Case 1: If w_j > 0:
        (w_j - ρ_j) + α(1) = 0   ──▶   w_j = ρ_j - α   (Valid only if ρ_j > α)
  
  Case 2: If w_j < 0:
        (w_j - ρ_j) + α(-1) = 0  ──▶   w_j = ρ_j + α   (Valid only if ρ_j < -α)
  
  Case 3: If w_j = 0:
        (0 - ρ_j) + α[-1, 1] ∋ 0 ──▶   |ρ_j| ≤ α        (Snaps to EXACT ZERO!)
  ```
  
  Combining these cases defines the **Soft-Thresholding Operator** ($\mathcal{S}_\alpha$):
  
  $$\mathbf{w_j^* = \mathcal{S}_\alpha(\rho_j) = \text{sign}(\rho_j) \max(0, \; |\rho_j| - \alpha)}$$
  
  ```
                   The Soft-Thresholding Function S_α(ρ)
         w_j*
          ▲                                    / Slope = 1
        2 │                                   /
        1 │                                  /
        0 ┼───────────────[═══ DEAD ZONE ═══]───────────────▶ Partial Correlation ρ_j
       -1 │              -α                 α
       -2 │             /
          │            / Slope = 1
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The "Dead Zone" Feature Selection Mechanism:                           │
  │                                                                        │
  │ • If the correlation between feature j and the residual is weaker      │
  │   than the penalty threshold (|ρ_j| ≤ α):                              │
  │                               w_j* = 0.0                               │
  │   The feature is snapped strictly to zero and eliminated!              │
  │                                                                        │
  │ • If |ρ_j| > α:                                                        │
  │   The weight is activated, but its magnitude is shrunk toward zero     │
  │   by an absolute constant α: w_j* = sign(ρ_j)(|ρ_j| - α).              │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. Architectural Guide: Ridge vs. Lasso vs. ElasticNet
  
  ```
  ┌─────────────────────┬──────────────────────────┬──────────────────────────┬────────────────────────┐
  │ Dimension           │ Ridge Regression (L2)    │ Lasso Regression (L1)    │ ElasticNet (L1 + L2)   │
  ├─────────────────────┼──────────────────────────┼──────────────────────────┼────────────────────────┤
  │ Mathematical Penalty│ α ||w||₂²                │ α ||w||₁                 │ α ρ ||w||₁             │
  │                     │                          │                          │ + ½ α (1 - ρ) ||w||₂²  │
  ├─────────────────────┼──────────────────────────┼──────────────────────────┼────────────────────────┤
  │ Computational Solve │ Closed-form Normal Eq:   │ Iterative Coordinate     │ Iterative Coordinate   │
  │                     │ (XᵀX + αI)⁻¹ Xᵀy         │ Descent (Soft Threshold) │ Descent                │
  ├─────────────────────┼──────────────────────────┼──────────────────────────┼────────────────────────┤
  │ Weight Effect       │ Smooth decay;            │ Sparse decay;            │ Combines sparsity with │
  │                     │ w_j ──▶ 0 (Never exact)  │ w_j = 0.0 (Exact zeros)  │ grouping stability.    │
  ├─────────────────────┼──────────────────────────┼──────────────────────────┼────────────────────────┤
  │ Multicollinear Data │ Distributes weights      │ Arbitrarily picks one    │ Retains the entire     │
  │ Behavior            │ equally across group.    │ feature; drops the rest. │ group of correlated    │
  │                     │                          │                          │ features together.     │
  ├─────────────────────┼──────────────────────────┼──────────────────────────┼────────────────────────┤
  │ Primary Best        │ Dense signals;           │ Sparse signals;          │ High-dimensional data  │
  │ Use Case            │ multicollinearity,       │ feature selection,       │ with groups of         │
  │                     │ general variance control.│ biomarker discovery.     │ correlated features.   │
  └─────────────────────┴──────────────────────────┴──────────────────────────┴────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 4
  
  1. *Why does the presence of sharp vertices on the coordinate axes of the $L_1$ constraint region cause expanding loss contours to hit those vertices first, producing exact zeros?*
  2. *Under the Soft-Thresholding operator $w_j^* = \text{sign}(\rho_j)\max(0, |\rho_j| - \alpha)$, what happens to a feature whose partial residual correlation satisfies $|\rho_j| \le \alpha$?*
  3. *If two features are perfectly correlated ($x_1 = x_2$), how do Ridge Regression and Lasso Regression handle their weights differently?*
  
  ---