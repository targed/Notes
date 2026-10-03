## 1. Why Outliers Exist: Non-Gaussian Error Distributions (Slides 56–60)
  
  In **Parts 1 and 2**, we relied on **RANSAC**—a stochastic algorithm that generates discrete hypotheses from minimal subsets of data. 
  
  Slide 56 introduces an alternative, deterministic question:
  > **Can we formulate continuous optimization algorithms that are inherently resistant to outliers without random sampling?**
  
  ```
     Gaussian Error Distribution                      Heavy-Tailed Laplace Distribution
          ^ P(r)                                           ^ P(r)
      0.4 │        ╭─╮                                 0.5 │         |
          │      ╭─╯ ╰─╮                                   │        / \
          │    ╭─╯     ╰─╮                                 │       /   \
      0.0 ┴────┴─────────┴─────► Residual r            0.0 ┴──────/─────\──────► Residual r
          Tail drops off exponentially:                   Tail decays much slower:
          P(r) ∝ exp(-r² / 2σ²)                           P(r) ∝ exp(-|r| / σ)
          Treats large errors as IMPOSSIBLE.              Allows large errors as EXPECTED.
  ```
  
  ---
### A. The Flaw in the Gaussian Noise Model (Slides 57–58)
  Recall that Ordinary Least Squares (OLS) minimizes the squared residual norm:
  
  $$\min_{\boldsymbol{\beta}} \sum_{i=1}^n r_i^2$$
  
  From Maximum Likelihood Estimation (MLE), minimizing squared errors is statistically optimal **if and only if** the measurement noise is normally distributed:
  
  $$P(r_i) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{r_i^2}{2\sigma^2} \right)$$
  
  $$\log L(\boldsymbol{\beta}) = -\frac{n}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}\sum_{i=1}^n r_i^2$$
  
  In real-world computer vision, however, errors do not follow a pure Gaussian curve:
  1. **Gaussian Inliers:** Fine feature localization jitter ($r \approx 0.5\text{--}1.5\text{ pixels}$).
  2. **Gross Outliers:** Ambiguous SIFT descriptor mismatches (such as the water bottle cap in Slide 4) where errors are tens or hundreds of pixels wide.
  
  Real matching errors follow a **heavy-tailed distribution** where large errors occur with much higher probability than a Gaussian predicts.
  
  ---
### B. The Laplace Distribution & $L_1$ Optimization (Slides 59–60)
  Slide 59 considers data drawn from a **Laplace (Double-Exponential) Distribution**:
  
  $$f(y_i \mid \mu, \sigma) = \frac{1}{2\sigma} \exp\left( -\frac{|y_i - \mu|}{\sigma} \right)$$
  
  Deriving the Maximum Likelihood Estimate for the center $\mu$:
  
  $$\log L(\mu) = -n\log(2\sigma) - \frac{1}{\sigma}\sum_{i=1}^n |y_i - \mu|$$
  
  Maximizing log-likelihood corresponds to minimizing the **$L_1$ norm (Least Absolute Deviations)**:
  
  $$\hat{\mu}_{\text{MLE}} = \arg\min_\mu \sum_{i=1}^n |y_i - \mu| \iff \mathbf{\hat{\mu} = \text{Median}(Y)}$$
  
  > **The Takeaway (Slide 60):** 
  > If errors follow a heavy-tailed distribution, **Least Squares is NOT the Maximum Likelihood Estimator**. 
  > The MLE under Laplace noise minimizes absolute errors ($L_1$), which yields the sample **median**. The median is fundamentally robust to extreme outliers because its derivative does not grow with the magnitude of the residual.
  
  ---
## 2. The General M-Estimator Framework (Slides 61–65)
  
  Slide 61 generalizes this principle into **M-Estimation** (Maximum-likelihood-type Estimation), introduced by statistician Peter J. Huber in 1964 [2]:
  
  $$\hat{\boldsymbol{\beta}} = \arg\min_{\boldsymbol{\beta}} \sum_{i=1}^n \rho\left( \frac{r_i(\boldsymbol{\beta})}{s} \right)$$
  
  where:
  * $r_i(\boldsymbol{\beta}) = y_i - \mathbf{x}_i^T \boldsymbol{\beta}$ is the residual error of measurement $i$.
  * $s$ is a robust scale estimate of the inlier noise (typically computed via the **Median Absolute Deviation (MAD)**: $s = 1.4826 \cdot \text{median}(|r_i - \text{median}(r)|)$).
  * $\rho(u)$ is a symmetric, continuous **loss function** ($\rho(u) \ge 0, \rho(0) = 0$).
  
  ---
### A. The Influence Function $\psi(u)$ (Slides 61–62)
  To understand how an M-estimator responds to gross outliers, we differentiate the loss function with respect to the normalized residual $u = r/s$. 
  The derivative is the **Influence Function $\psi(u)$**:
  
  $$\psi(u) = \rho'(u) = \frac{d\rho(u)}{du}$$
  
  Slide 62 asks:
  > **"What is $\psi$ for a Gaussian (Least Squares)?"**
  > 
  > *Answer:* For standard Least Squares:
  > 
  > $$\rho_{\text{OLS}}(u) = \frac{1}{2} u^2 \implies \psi_{\text{OLS}}(u) = \mathbf{u}$$
  
  ```
                               The Influence Function ψ(u) = ρ'(u)
                               
        Standard Least Squares (Slide 62)                  Robust M-Estimator (Huber, Slide 63)
               ^ ψ(u)                                             ^ ψ(u)
               │          /                                       │        ┌──────── (Saturates at k!)
               │         /                                      k ┼───────╭╯
               │        /                                         │      ╭╯
           0.0 ┼───────/───────► Residual u                   0.0 ┼─────/─────► Residual u
               │      /                                           │   ╭╯
               │     /                                         -k ┼──╯
               │    / (UNBOUNDED INFLUENCE!)                      └──────── (Saturates at -k!)
  ```
  
  * **The Problem with Least Squares:** The influence function $\psi_{\text{OLS}}(u) = u$ is **unbounded**. If an outlier has an error of $u = 1000$, it exerts **$1000\times$ more influence** on the optimization gradient than an inlier with $u = 1$.
  * **The Goal of Robust Estimators:** Design a loss function $\rho$ whose influence $\psi(u)$ **remains bounded** or **decays back to zero** for large residuals.
  
  ---
### B. The Gallery of Robust Loss Functions (Slides 63–65)
  
  ```
  ┌───────────────────────────┬───────────────────────────────────────────┬────────────────────────────────────────┐
  │ Estimator Name            │ Loss Function $\rho(u)$                   │ Influence Function $\psi(u) = \rho'(u)$│
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Least Squares ($L_2$)** │ $\frac{1}{2} u^2$                         │ $u$ (Unbounded linear)                 │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Absolute Error ($L_1$)**│ $|u|$                                     │ $\text{sign}(u)$ (Constant bounded)   │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Huber Loss (Slide 63)** │ $\begin{cases} \frac{1}{2}u^2 & |u| \le k \\ k|u| - \frac{1}{2}k^2 & |u| > k \end{cases}$ │ $\begin{cases} u & |u| \le k \\ k\cdot\text{sign}(u) & |u| > k \end{cases}$ │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Hampel (Slide 64)**     │ Piecewise quadratic / linear              │ Ascending, plateau, descends to $0$    │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Tukey's Biweight**      │ $\begin{cases} \frac{c^2}{6}[1 - (1-(u/c)^2)^3] & |u|\le c \\ \frac{c^2}{6} & |u|>c \end{cases}$ │ $\begin{cases} u[1 - (u/c)^2]^2 & |u| \le c \\ 0 & |u| > c \end{cases}$ │
  └───────────────────────────┴───────────────────────────────────────────┴────────────────────────────────────────┘
  ```
#### 1. Huber Loss (Slide 63):
  * Acts as smooth $L_2$ least squares for small inlier residuals ($|u| \le k$, typically $k = 1.345$).
  * Transitions to linear $L_1$ penalty for large residuals ($|u| > k$).
  * **Advantage:** The loss function is **convex**, guaranteeing a unique global minimum with no local traps.
#### 2. Tukey's Biweight / Bi-square (Slide 65):
  * A **redescending M-estimator**: for residuals beyond cutoff $c$ (typically $c = 4.685$), the influence function drops **identically to zero** ($\psi(u) = 0$).
  * **Advantage:** Outliers beyond the threshold have zero influence on the solution.
  
  ---
## 3. Iteratively Reweighted Least Squares (IRLS) (Slides 66–68)
  
  Because robust loss functions $\rho(u)$ are non-linear, we cannot solve for $\boldsymbol{\beta}$ using a single closed-form matrix equation. 
  
  Slide 67 shows how to linearize this system into **Iteratively Reweighted Least Squares (IRLS)**.
  
  ---
### A. Weighted Least Squares (WLS) Formulation (Slide 66)
  If each observation $i$ has a known confidence weight $w_i > 0$, the weighted objective function is:
  
  $$J_W(\boldsymbol{\beta}) = \sum_{i=1}^n w_i \big(y_i - \mathbf{x}_i^T \boldsymbol{\beta}\big)^2 = (\mathbf{y} - \mathbf{X}\boldsymbol{\beta})^T \mathbf{W} (\mathbf{y} - \mathbf{X}\boldsymbol{\beta})$$
  
  where $\mathbf{W} = \text{diag}(w_1, w_2, \dots, w_n) \in \mathbb{R}^{n \times n}$.
  Setting the gradient with respect to $\boldsymbol{\beta}$ to zero yields the **Weighted Normal Equations**:
  
  $$\mathbf{X}^T \mathbf{W} \mathbf{X} \boldsymbol{\beta} = \mathbf{X}^T \mathbf{W} \mathbf{y} \implies \hat{\boldsymbol{\beta}}_W = (\mathbf{X}^T \mathbf{W} \mathbf{X})^{-1} \mathbf{X}^T \mathbf{W} \mathbf{y}$$
  
  ---
### B. Transforming M-Estimation into Weighted Normal Equations (Slide 67)
  Differentiating the general M-estimator objective $\sum \rho(r_i / s)$ with respect to parameter $\beta_j$:
  
  $$\sum_{i=1}^n \frac{\partial r_i}{\partial \beta_j} \cdot \frac{1}{s} \cdot \rho'\left( \frac{r_i}{s} \right) = 0$$
  
  Since $r_i = y_i - \mathbf{x}_i^T \boldsymbol{\beta}$, its derivative is $\frac{\partial r_i}{\partial \beta_j} = -X_{ij}$:
  
  $$\sum_{i=1}^n X_{ij} \cdot \psi\left( \frac{r_i}{s} \right) = 0$$
  
  Now, multiply and divide the influence term by the normalized residual $u_i = r_i/s$:
  
  $$\sum_{i=1}^n X_{ij} \cdot \left[ \frac{\psi(u_i)}{u_i} \right] \cdot \left(\frac{r_i}{s}\right) = 0$$
  
  Define the **Weight Function** $w(u_i)$ (Slide 67):
  
  $$w(u_i) = \frac{\psi(u_i)}{u_i} = \frac{\rho'(u_i)}{u_i}$$
  
  The condition becomes:
  
  $$\sum_{i=1}^n X_{ij} \cdot w(u_i) \cdot \big(y_i - \mathbf{x}_i^T \boldsymbol{\beta}\big) = 0 \iff \mathbf{X}^T \mathbf{W} (\mathbf{y} - \mathbf{X}\boldsymbol{\beta}) = \mathbf{0}$$
  
  $$\mathbf{X}^T \mathbf{W} \mathbf{X} \boldsymbol{\beta} = \mathbf{X}^T \mathbf{W} \mathbf{y}$$
  
  ---
### C. The IRLS Algorithmic Loop (Slide 68)
  Because the weights $w_i = w(r_i)$ depend on the residuals—which in turn depend on the unknown parameters $\boldsymbol{\beta}$—we solve the system **iteratively**:
  
  ```
  Algorithm: Iteratively Reweighted Least Squares (IRLS)
  ─────────────────────────────────────────────────────────────────────────────
  1. Initialize parameter vector β^(0) using standard OLS:
       β^(0) = (X^T X)⁻¹ X^T y
  2. Loop for iteration k = 0, 1, 2, ... until convergence:
       a. Compute current residuals:
              r_i^(k) = y_i - x_i^T β^(k)
       b. Compute scale estimate s (e.g., s = 1.4826 · MAD(r))
       c. Compute dynamic weights for every sample:
              w_i^(k) = ψ(r_i^(k) / s) / (r_i^(k) / s)
              W^(k) = diag(w_1^(k), w_2^(k), ..., w_n^(k))
       d. Update parameter vector by solving Weighted Normal Equations:
              β^(k+1) = (X^T W^(k) X)⁻¹ X^T W^(k) y
       e. Check convergence:
              If ||β^(k+1) - β^(k)|| < ε, stop and return β^(k+1).
  ─────────────────────────────────────────────────────────────────────────────
  ```
  
  ---
## 4. Modern Non-Convex Optimization in SLAM: Graduated Non-Convexity (Slides 69–70)
  
  Slides 69 and 70 reference recent research from MIT:
  > **"Graduated Non-Convexity (GNC) for Robust Spatial Perception: From Non-Minimal Solvers to Global Outlier Rejection"** (Heng Yang, Pasquale Antonante, Vasileios Tzoumas, Luca Carlone, 2020) [3].
  
  ```
       Non-Convex Optimization Trap                     Graduated Non-Convexity (GNC) Annealing
            ^ Cost                                           ^ Cost
            │   ╭─╮                                          │               ╭─╮
            │ ╭─╯ ╰─╮ Local Minimum                          │             ╭─╯ ╰─╮ Convex surrogate (μ = ∞)
            │╭╯     ╰─╮                                      │           ╭─╯     ╰─╮
            ││   •    │ Global Minimum                       │        ╭──╯   •   ╰──╮ True non-convex
        0.0 ┴┴───┴────┴───────────────►                      0.0 ┴────────┴─────────┴────────► loss (μ → 0)
            Standard IRLS gets stuck                         GNC slowly morphs convex curve into non-convex
            in local minimum traps!                          loss, converging to the global minimum!
  ```
### Why GNC is Used in Spatial Perception:
  1. **The Non-Convexity Problem:** Highly robust loss functions (such as Tukey's biweight or Geman-McClure) are **non-convex**. If standard IRLS is initialized poorly, it gets trapped in sub-optimal local minima.
  2. **The GNC Solution:** GNC introduces a continuous annealing parameter $\mu$. 
   * It starts with a convex surrogate loss ($\mu \to \infty$, equivalent to Huber/least squares), where optimization is guaranteed to converge globally.
   * It gradually anneals $\mu \to 0$, morphing the cost surface step-by-step into the true non-convex robust loss.
  3. **Performance (Slide 70):** 
   Slide 70 shows 3D mesh vehicle registration operating with **$70\%$ outliers**. While standard RANSAC fails or requires millions of iterations, GNC converges deterministically to the true 6-DOF camera pose.
  
  ---
## 5. The Complete Automated Image Stitching Pipeline (Slides 71–73)
  
  Slide 72 summarizes the multi-stage alignment pipeline constructed throughout this unit:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │                        The Automated Image Stitching Pipeline                          │
  └────────────────────────────────────────────────────────────────────────────────────────┘
                                           │
                                           ▼
                 ┌──────────────────────────────────────────────────┐
                 │     Step 1: FEATURE DETECTION (Modules M3.1/3.2) │
                 │ Extract scale-space keypoints (Harris, LoG, DoG) │
                 │ across Image 1 and Image 2.                      │
                 └─────────────────────────┬────────────────────────┘
                                           │
                                           ▼
                 ┌──────────────────────────────────────────────────┐
                 │     Step 2: FEATURE DESCRIPTION (Module M4.2)    │
                 │ Compute 128D SIFT descriptors or 64D MOPS        │
                 │ vectors for all detected keypoints.              │
                 └─────────────────────────┬────────────────────────┘
                                           │
                                           ▼
                 ┌──────────────────────────────────────────────────┐
                 │     Step 3: FEATURE MATCHING (Module M4.2)       │
                 │ Match descriptors using Lowe's Ratio Test        │
                 │ (r = d₁ / d₂ ≤ 0.75) to prune ambiguous matches. │
                 └─────────────────────────┬────────────────────────┘
                                           │
                                           ▼
                 ┌──────────────────────────────────────────────────┐
                 │     Step 4: ROBUST HOMOGRAPHY (Module M5.1/5.2)  │
                 │ Run Adaptive RANSAC with 4-point DLT to          │
                 │ reject outliers and estimate H_best.             │
                 │ Re-fit H on all inliers via SVD.                 │
                 └─────────────────────────┬────────────────────────┘
                                           │
                                           ▼
                 ┌──────────────────────────────────────────────────┐
                 │     Step 5: WARPING & COMPOSITING (Module M2.1)  │
                 │ Inverse-warp Image 2 onto Image 1's canvas via   │
                 │ bilinear interpolation; blend using Laplacians.  │
                 └──────────────────────────────────────────────────┘
  ```
  
  ---
## 6. The 360° Panorama Singularity: Why Planar Homographies Fail (Slides 73–76)
  
  Slide 73 poses a concluding question:
  > **"So what can't we do? How about a $360^\circ$ panorama?"**
  
  Slide 75 provides the answer:
  > **"Can we make a $360^\circ$ panorama with homographies? No! Remember: homographies preserve straight lines."**
  
  ```
                  The Planar Homography Projection Singularity (Slide 75)
                  
                                           x = f · tan(θ)
                                           
                                           θ = 0°  ──► x = 0
                                           θ = 45° ──► x = f
                                           θ = 85° ──► x = 11.4 f
                                           θ → 90° ──► x → ∞ !  (SINGULARITY)
                                           
                                                Flat Image Canvas (Plane)
                                            ───────────────────────────────────► ∞
                                                     \           |           /
                                                      \          |          /
                                                       \         |         /
                                                        \        |        /
                                                         \  θ    |       /
                                                          \      |      /
                                                           \     |     /
                                                            \    |    /
                                                             \   |   /
                                                              \  |  /
                                                                 • Camera Center
  ```
  
  ---
### A. The Planar Projection Singularity
  A planar homography maps rays from the camera's center of projection onto a **flat 2D plane**:
  
  $$x = f \cdot \tan(\theta)$$
  
  where $\theta$ is the horizontal angle from the optical axis.
  * As the camera rotates toward $\pm 90^\circ$, the tangent function diverges to infinity:
  
  $$\lim_{\theta \to 90^\circ} \tan(\theta) = \infty$$
  
  * **The Problem:** 
  A flat plane cannot represent a wide field of view ($> 120^\circ$). Beyond $120^\circ$, perimeter pixels stretch into infinity, causing severe geometric distortion.
  * A $360^\circ$ scene completely surrounds the camera; rays behind the camera ($\theta > 90^\circ$) never intersect a front-facing image plane.
  
  ---
### B. The Transition to Cylindrical & Spherical Projection (Slide 76)
  To construct full $360^\circ$ panoramas without infinite stretching, we must discard planar projection surfaces in favor of curved surfaces:
  
  ```
        Planar Projection (Homographies)                  Cylindrical Projection (360° Panoramas)
     ┌──────────────────────────────────┐               ┌──────────────────────────────────┐
     │                                  │               │                                  │
     │      Stretches to infinity       │               │      ╭────────────────────╮      │
     │      at field edges! (θ → 90°)   │               │     ╭╯                    ╰╮     │
     │                                  │               │     │  Wraps uniformly    │      │
     │ ──────────────────────────────── │               │     ╰╮ around 360° circle ╭╯     │
     │                                  │               │      ╰────────────────────╯      │
     └──────────────────────────────────┘               └──────────────────────────────────┘
  ```
  
  1. **Cylindrical Projection:**
   Project each camera ray onto an internal cylinder of radius $f$:
  
  $$\theta = \arctan\left(\frac{x}{f}\right), \quad h = \frac{y}{\sqrt{x^2 + f^2}}$$
  
  $$x_{\text{cyl}} = f \cdot \theta, \quad y_{\text{cyl}} = f \cdot h$$
  
   * Horizontal angles map linearly: $x_{\text{cyl}} \in [0, \, 2\pi f]$.
   * Straight horizontal lines become curved sine waves, but **the $360^\circ$ view wraps around seamlessly**.
  2. **Spherical Projection:**
   Projects rays onto a unit sphere, parameterized by longitude $\theta$ and latitude $\phi$ (equirectangular projection), commonly used in VR headsets and $360^\circ$ environmental mapping (Slide 74).
  
  ---
## 7. Python Implementation: Iteratively Reweighted Least Squares (IRLS)
  
  The following NumPy program demonstrates robust M-estimation via IRLS, comparing standard Least Squares against **Huber Loss** and **Tukey's Biweight Loss** on data with heavy outliers:
  
  ```python
  import numpy as np
  
  def huber_weight(u: np.ndarray, k: float = 1.345) -> np.ndarray:
    """Computes Huber weight: w(u) = psi(u) / u."""
    abs_u = np.abs(u)
    # w(u) = 1 if |u| <= k else k / |u|
    weights = np.where(abs_u <= k, 1.0, k / (abs_u + 1e-12))
    return weights
  
  def tukey_weight(u: np.ndarray, c: float = 4.685) -> np.ndarray:
    """Computes Tukey Biweight: w(u) = (1 - (u/c)^2)^2 if |u| <= c else 0."""
    abs_u = np.abs(u)
    weights = np.where(abs_u <= c, (1.0 - (u / c)**2)**2, 0.0)
    return weights
  
  def fit_line_irls(x: np.ndarray, 
                  y: np.ndarray, 
                  loss_type: str = 'huber', 
                  max_iters: int = 50, 
                  tol: float = 1e-5):
    """
    Fits y = beta_0 + beta_1 * x using Iteratively Reweighted Least Squares (IRLS).
    """
    N = len(x)
    X = np.column_stack([np.ones(N), x])  # Design matrix (N x 2)
    
    # 1. Initialize using Ordinary Least Squares (OLS)
    beta = np.linalg.lstsq(X, y, rcond=None)[0]
    
    for iteration in range(max_iters):
        # Compute residuals
        residuals = y - X @ beta
        
        # Robust scale estimate via Median Absolute Deviation (MAD)
        med = np.median(residuals)
        mad = np.median(np.abs(residuals - med))
        s = 1.4826 * mad + 1e-12
        
        normalized_residuals = residuals / s
        
        # Compute weights based on M-estimator loss
        if loss_type == 'huber':
            w = huber_weight(normalized_residuals, k=1.345)
        elif loss_type == 'tukey':
            w = tukey_weight(normalized_residuals, c=4.685)
        else:
            w = np.ones(N)
            
        # Weighted Normal Equations: (X^T W X) beta = X^T W y
        # We can write X_w = sqrt(w) * X and y_w = sqrt(w) * y
        sqrt_w = np.sqrt(w)[:, np.newaxis]
        X_w = X * sqrt_w
        y_w = y * sqrt_w.flatten()
        
        beta_new = np.linalg.lstsq(X_w, y_w, rcond=None)[0]
        
        # Check convergence
        if np.linalg.norm(beta_new - beta) < tol:
            break
            
        beta = beta_new
        
    return beta
  ```
  
  ---
## Complete Module Summary: M5.2 RANSAC & Robust Estimation
  
  1. **Least squares is non-robust:** The quadratic penalty ($r^2$) gives outliers disproportionate influence, causing a single false match to disrupt the estimated model.
  2. **RANSAC optimizes consensus:** Generating candidate models from **minimal sample sets** ($s$) and counting inliers within tolerance $d$ isolates true structure from heavy background clutter.
  3. **Inlier thresholds follow the $\chi^2$ distribution:** Assuming Gaussian localization noise $\sigma$, the 95% confidence bound is $d^2 \le 3.84\sigma^2$ for 1D residuals and $d^2 \le 5.99\sigma^2$ for 2D homography transfer error.
  4. **Sample size $s$ drives iteration complexity:** The required iterations $N = \frac{\ln(1 - p)}{\ln(1 - (1 - e)^s)}$ grow exponentially with $s$, requiring minimal solvers for hypothesis generation.
  5. **M-estimators replace sampling with continuous weighting:** IRLS minimizes robust loss functions (Huber, Tukey) by updating sample weights $w_i = \psi(u_i)/u_i$ and solving weighted normal equations iteratively.
  6. **Homographies cannot represent $360^\circ$ views:** Planar projections diverge to infinity as $\theta \to 90^\circ$, requiring cylindrical or spherical camera models for full panoramic fields of view.
  
  ---