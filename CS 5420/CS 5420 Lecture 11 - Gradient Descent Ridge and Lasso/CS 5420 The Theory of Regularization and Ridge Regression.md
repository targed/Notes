## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Why Single-Sample SGD ($B = 1$) Under-Utilizes GPUs:**
  * GPUs achieve acceleration through massive hardware parallelism via **Single Instruction, Multiple Threads (SIMT)** executed across Streaming Multiprocessors (SMs) organized in warps of 32 threads.
  * With a batch size of $B = 1$, the operation collapses to a single vector-vector inner product. Compute intensity is tiny, and the GPU sits idle while waiting for memory transfers over the PCIe bus.
  * A mini-batch of $B = 64$ to $512$ reformulates the forward and backward passes into **dense matrix-matrix multiplications (GEMM)**, fully populating CUDA tensor cores and amortizing kernel dispatch latencies.
  2. **Mathematical Proof of the Unbiased Mini-Batch Gradient:**
  * Let $\mathcal{B} \subset \{1, \dots, n\}$ be a random mini-batch of size $B$ sampled uniformly without replacement from $\{1, \dots, n\}$.
  * Evaluating the expectation of the mini-batch sample mean gradient:
   $$\mathbb{E}_{\mathcal{B}}\left[ \frac{1}{B} \sum_{i \in \mathcal{B}} \nabla_w \mathcal{L}(y_i, f(x_i; w)) \right] = \frac{1}{B} \sum_{i \in \mathcal{B}} \mathbb{E}_{i \sim \mathcal{U}(1, n)}\left[ \nabla_w \mathcal{L}(y_i, f(x_i; w)) \right]$$
   $$= \frac{1}{B} \cdot B \cdot \left( \frac{1}{n} \sum_{j=1}^n \nabla_w \mathcal{L}(y_j, f(x_j; w)) \right) \equiv \mathbf{\nabla J(w)}$$
  * The expected value of the mini-batch gradient is identical to the full-batch population gradient.
  3. **Escaping Non-Convex Minima (BGD vs. MBGD):**
  * Batch Gradient Descent computes the exact gradient $\nabla J(w)$. If updates reach a saddle point or shallow local minimum where $\nabla J(w) \approx \mathbf{0}$, the deterministic step size collapses ($\Delta w \to \mathbf{0}$), permanently trapping the optimizer.
  * Mini-Batch Gradient Descent introduces stochastic gradient noise with covariance $\frac{\Sigma}{B}$. This noise acts as an internal thermal perturbation (simulated annealing), allowing parameters to kick out of shallow, high-variance local basins and settle into broader, flatter minima with superior generalization properties.
  
  ---
## 2. The Over-Fitting Pathology: Why Regularize? (Slide 8)
  
  Slide 8 introduces the core motivation behind regularized empirical risk minimization:
  
  $$\mathbf{\text{"When features } d \text{ are large relative to samples } n\text{, least squares can perfectly memorize (interpolate) training noise."}}$$
  $$\mathbf{\text{"Perform very well on training sets, but catastrophic generalization failure on test sets."}}$$
  
  ```
                       The Regularization Objective (Slide 8)
         Over-Fitting (Unconstrained OLS)             Appropriate-Fitting (Regularized)
    y                                            y
    ▲      x                                     ▲      x
    │     / \        x                           │     /        x
    │    /   \      / \                          │    /       /
    │   x     \    /   x                         │   x       /     x
    │          \  /                              │          /
    │           x                                │         x
    └────────────────────────▶ x                 └────────────────────────▶ x
    • Low training error (Memorized noise)       • Balances training fit with simplicity
    • Wild parameter oscillations                • Constrained parameter magnitudes
    • High generalization error (High Variance)  • Optimal generalization on test data
  ```
  
  ---
### The Anatomy of Over-Parameterization ($d \approx n$ or $d > n$)
  In unregularized Ordinary Least Squares (OLS), the hypothesis space has $d+1$ degrees of freedom.
  1. **Perfect Interpolation:** If $d \approx n$, the design matrix $X$ has enough capacity to fit through every single noisy data point ($y_i = x_i^T w + \epsilon_i$), achieving $\hat{R}_N(w) \approx 0$.
  2. **Exploding Weight Vectors:** To fit training noise, parameters oscillate wildly to offset collinear feature signals. One feature receives a weight of $+10{,}000$ while a correlated neighbor receives $-9{,}998$.
  3. **The Bias-Variance Trade-Off:** The model achieves near-zero structural bias, but its parameter variance explodes ($\text{Var}(\hat{w}) \to \infty$). Small perturbations in unseen test inputs generate massive fluctuations in predictions $\hat{y}$.
  
  Slide 8 formalizes the universal regularized loss:
  
  $$\mathbf{J(w) = \|y - Xw\|_2^2 + \lambda \cdot \text{penalty}(w)}$$
  
  Regularization imposes a mathematical **complexity penalty** on the size of the parameters, operationalizing **Occam’s Razor**: among competing models that fit the data, prefer the one with smaller, smoother weights.
  
  ---
## 3. Ridge Regression ($L_2$ Penalty / Tikhonov Regularization) (Slide 9)
  
  Slide 9 formalizes Ridge Regression, which penalizes the squared Euclidean ($L_2$) norm of the weight vector:
  
  $$\mathbf{J(w) = \|y - Xw\|_2^2 + \alpha \|w\|_2^2 = \sum_{i=1}^n (y_i - x_i^T w)^2 + \alpha \sum_{j=1}^d w_j^2}$$
  
  *(Note: Dr. Yu denotes the regularization strength parameter as $\alpha$, matching scikit-learn's `Ridge(alpha=...)` syntax).*
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Regularization Parameter (α ≥ 0):                                  │
  │                                                                        │
  │ • When α = 0:                                                          │
  │   J_Ridge(w) = ||y - Xw||² ──▶ Reverts to unconstrained Ordinary       │
  │                               Least Squares.                           │
  │                                                                        │
  │ • As α ──▶ ∞:                                                          │
  │   The penalty term dominates. To avoid infinite cost, all weights      │
  │   are forced toward zero: w_j ──▶ 0 (Hypothesis collapses to ŷ = w₀).  │
  │                                                                        │
  │ • Moderate α:                                                          │
  │   Trades a small increase in training bias for a massive reduction in  │
  │   parameter variance, minimizing overall test error.                   │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Analytical Derivation of the Ridge Normal Equations (Slide 9)
  
  Slide 9 provides the analytical closed-form solution for Ridge regression:
  
  $$\mathbf{w_{\text{Ridge}}^* = (X^T X + \alpha I)^{-1} X^T y}$$
  
  ---
### Step-by-Step Matrix Calculus Derivation:
  Express the Ridge loss function in matrix notation:
  
  $$J(w) = (y - Xw)^T (y - Xw) + \alpha w^T w$$
  
  Expand the quadratic data term:
  
  $$J(w) = y^T y - 2 w^T X^T y + w^T X^T X w + \alpha w^T I w$$
  
  Combine the two quadratic forms into a single matrix operator:
  
  $$J(w) = y^T y - 2 w^T X^T y + w^T (X^T X + \alpha I) w$$
  
  Compute the vector gradient $\nabla_w J(w)$ with respect to $w$:
  
  $$\nabla_w J(w) = \mathbf{0} - 2 X^T y + 2 (X^T X + \alpha I) w$$
  
  Setting the gradient vector to zero to locate the stationary point:
  
  $$-2 X^T y + 2 (X^T X + \alpha I) w = \mathbf{0}$$
  
  $$(X^T X + \alpha I) w = X^T y$$
  
  Multiplying both sides by the inverse matrix:
  
  $$\mathbf{w_{\text{Ridge}}^* = (X^T X + \alpha I)^{-1} X^T y}$$
  
  ---
## 5. The Invertibility Guarantee: Why $\alpha I$ Resolves Rank Deficiency (Slide 9)
  
  Slide 9 emphasizes a major numerical systems advantage of Ridge regression:
  
  $$\mathbf{\text{"Closed form: } w = (X^T X + \alpha I)^{-1} X^T y\text{, the } \alpha I \text{ makes it invertible even when } X^T X \text{ is not."}}$$
  
  ```
                       The Spectral Shift of Ridge Regression
            Unregularized Gram Matrix: XᵀX            Regularized Ridge Matrix: (XᵀX + αI)
  ┌───────────────────────────────────────────────┐ ┌───────────────────────────────────────────────┐
  │ • If d > n or features collinear:             │ │ • Spectrum shifted uniformly by +α:           │
  │   λ_min(XᵀX) = 0                              │ │   λ_i(XᵀX + αI) = λ_i(XᵀX) + α               │
  │ • det(XᵀX) = ∏ λ_i = 0                        │ │ • det(XᵀX + αI) = ∏ (λ_i + α) > 0             │
  │ • SVD condition number κ ──▶ ∞                │ │ • Minimum eigenvalue λ_min ≥ α > 0            │
  │ • NON-INVERTIBLE (Singular Matrix Crash!)     │ │ • STRICTLY POSITIVE DEFINITE & INVERTIBLE     │
  └───────────────────────────────────────────────┘ └───────────────────────────────────────────────┘
  ```
### The Spectral Eigendecomposition Proof:
  Let $X^T X \in \mathbb{R}^{m \times m}$ (where $m = d+1$) be the symmetric real Gram matrix. By the Spectral Theorem, it can be factored as:
  
  $$X^T X = Q \Lambda Q^T$$
  
  where $Q$ is an orthonormal matrix of eigenvectors, and $\Lambda = \text{diag}(\lambda_1, \lambda_2, \dots, \lambda_m)$ contains non-negative eigenvalues ($\lambda_i \ge 0$).
  
  When adding the identity matrix scaled by $\alpha > 0$:
  
  $$X^T X + \alpha I = Q \Lambda Q^T + \alpha Q I Q^T = Q (\Lambda + \alpha I) Q^T$$
  
  The eigenvalues of the regularized matrix are shifted directly:
  
  $$\lambda_i(X^T X + \alpha I) = \mathbf{\lambda_i(X^T X) + \alpha}$$
  
  * Even if $X^T X$ has rank $1$ and possesses dozens of zero eigenvalues ($\lambda_i = 0$), the regularized eigenvalues satisfy:
  $$\lambda_i + \alpha \ge \alpha > \mathbf{0} \quad \forall \; i \in \{1, \dots, m\}$$
  * Because all eigenvalues are strictly positive real numbers, the determinant is strictly positive:
  $$\det(X^T X + \alpha I) = \prod_{i=1}^m (\lambda_i + \alpha) \ge \alpha^m > 0$$
  
  $$\mathbf{\text{Ridge regression is guaranteed to have a unique, well-conditioned inverse, even when } d \gg n\text{!}}$$
  
  ---
## 6. SVD Shrinkage Mechanics: Smooth Decay (Slide 9)
  
  Slide 9 states the functional behavior of the $L_2$ penalty:
  
  $$\mathbf{\text{"Shrinks all coefficients smoothly toward zero; almost never exactly to zero."}}$$
  
  To understand why Ridge shrinks weights without setting them to zero, we express the solution via the **Singular Value Decomposition (SVD)** of the centered design matrix $X = U \Sigma V^T$:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Comparison of OLS vs. Ridge Parameter Expansions via SVD:              │
  │                                                                        │
  │ • Unconstrained OLS Solution:                                          │
  │             w_OLS   = ∑ [ 1 / σ_j ] · (u_jᵀ y) · v_j                   │
  │                                                                        │
  │ • Regularized Ridge Solution:                                          │
  │             w_Ridge = ∑ [ σ_j / (σ_j² + α) ] · (u_jᵀ y) · v_j          │
  │                                                                        │
  │ Multiplying and dividing by σ_j reveals the explicit SHRINKAGE FACTOR: │
  │                                                                        │
  │   w_Ridge = ∑ ( σ_j² / (σ_j² + α) ) · [ 1 / σ_j · (u_jᵀ y) · v_j ]    │
  │                                                                        │
  │             w_Ridge = ∑ f_j · w_OLS,j                                  │
  │                                                                        │
  │   where the Ridge Shrinkage Factor is:                                 │
  │                                                                        │
  │                      f_j = σ_j² / (σ_j² + α)                           │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### The Dual Shrinkage Dynamics:
  The factor $f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha} \in (0, 1)$ governs how much each component is scaled:
  
  1. **High-Variance Principal Directions ($\sigma_j^2 \gg \alpha$):**
   * Features or directions with large singular values represent strong, dominant data signals.
   * The shrinkage factor approaches one:
     $$f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha} \approx \mathbf{1.0}$$
   * Informative, high-variance components are preserved with minimal shrinkage.
  2. **Low-Variance Collinear Directions ($\sigma_j^2 \ll \alpha$):**
   * Directions with small singular values represent noise, collinear artifacts, or low-variance directions where unconstrained OLS inflates weights by $\frac{1}{\sigma_j}$.
   * The shrinkage factor approaches zero:
     $$f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha} \longrightarrow \mathbf{0}$$
   * Noise components are suppressed, preventing parameter explosion.
#### Why Ridge Does Not Perform Feature Selection:
  Because $\sigma_j^2 > 0$ and $\alpha$ is finite, the shrinkage factor is **strictly positive**:
  $$f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha} > 0 \quad \forall \; j$$
  Every weight is smoothly scaled down toward zero, but **it never equals zero identically**. Ridge regression keeps all $d$ features in the model, making it unsuitable for applications requiring strict feature sparsity.
  
  ---
## Summary Review Questions for Section 3
  
  1. *Why does adding the term $\alpha I$ to $X^T X$ guarantee that the matrix $(X^T X + \alpha I)$ is invertible, even if the dataset has far more features than samples ($d \gg n$)?*
  2. *Using the Ridge shrinkage factor $f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha}$, explain why Ridge regression shrinks low-variance collinear directions much more aggressively than high-variance principal directions.*
  3. *Why is Ridge regression incapable of performing automatic feature selection compared to Lasso regression?*
  
  ---