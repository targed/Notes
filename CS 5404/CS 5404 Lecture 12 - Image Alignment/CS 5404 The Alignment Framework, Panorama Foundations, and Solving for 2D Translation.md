## 1. The Image Alignment Objective & Panorama Motivation (Slides 2–13)
  
  Slide 2 and Slide 5 summarize the core problem of computational photography and geometric vision:  
  > **The Image Alignment Problem:**  
  Given two or more images observing overlapping regions of a scene, determine the geometric transformation $\mathbf{T}$ that warps one image so that its visual features align with corresponding features in the other.  
  
  ```
     Image 1 (Reference Frame)                        Image 2 (Target Frame)
  ┌───────────────────────────┐                    ┌───────────────────────────┐
  │          • p_1            │                    │                     • p'_1│
  │          • p_2            │                    │                     • p'_2│
  │          • p_3            │   Feature Matches  │                     • p'_3│
  │                           │ ─────────────────► │                           │
  │          • p_N            │   p_i <---> p'_i   │                     • p'_N│
  └───────────────────────────┘                    └───────────────────────────┘
                │                                                │
                └───────────────────────┬────────────────────────┘
                                        ▼
                       [ Geometric Model Fitting Engine ]
                       Solves for transformation T such that:
                                  p'_i ≈ T(p_i)
  ```
  
  ---
### A. How Panoramas Are Created: Hardware vs. Software (Slides 7–12)
  Slide 7 asks: *"Let's say we want to make a panorama. Fortunately, my smartphone does this automatically... but how?"*  
  
  To capture an ultra-wide field of view (FOV), an engineer has two options:  
  
  ```
                            Wide-Field Imaging Strategies
                                          │
         ┌────────────────────────────────┴────────────────────────────────┐
         ▼                                                                 ▼
  [ HARDWARE APPROACH (Slide 8–10) ]                               [ SOFTWARE APPROACH (Slide 11–12) ]
  • Use specialized wide-angle / fisheye optics.                   • Capture multiple standard narrow-FOV images.
  • Example: Ricoh Theta 360° dual-lens camera.                    • Rotate camera across the scene.
  • Problems: Extreme barrel distortion, uneven pixel              • Align overlapping images via feature matching
    resolution, expensive optics, thermal calibration drift.         and blend into a seamless composite mosaic.
  ```
#### 1. Hardware Limitations (Slides 8–10):
  * **Optical Aberrations:** Ultra-wide fisheye lenses introduce extreme barrel distortion, compressing perimeter pixels non-linearly.
  * **Cost & Vignetting:** High-quality wide-angle glass is physically bulky, expensive, and suffers from radial light falloff (vignetting).
  * **Thermal Calibration Drift (Slide 10):** Slide 10 highlights vertical seam stitching artifacts captured by a Ricoh Theta dual-lens camera. Internal thermal dissipation inside compact camera bodies causes optical sensor mounts to expand by micrometers over time, shifting pre-calibrated intrinsic lens parameters and breaking factory stitching models.
#### 2. The Software Solution: Multi-Image Alignment (Slides 11–12):
  Instead of relying on specialized lenses, a user captures a sequence of regular, narrow-FOV photos by panning a handheld camera or phone.   
  By detecting local features (SIFT/Harris, Module M4.2) across adjacent overlapping frames, an algorithm solves for the inter-image transformation matrix and warps the sequence onto a unified panoramic canvas (Slide 12).  
  
  ---
### B. The Center of Projection (COP) Invariant
  Why does multi-image panoramic alignment work cleanly in urban settings or indoor rooms (Slide 11–12)?  
  * **The No-Parallax Condition:**  
  If a camera rotates strictly around its physical **Center of Projection (optical center)**, the translation vector between views is zero ( $\mathbf{t} = \mathbf{0}$ ).
  * Because there is zero camera translation, **there is zero motion parallax**.
  * Foreground objects (e.g., the balcony railing in Slide 12) do not shift relative to background structures (e.g., the distant atrium skylight). The scene transforms as a pure projective homography, allowing software alignment without knowing 3D scene depth.
  ---
## 2. Review of 2D Transformation Models (Slides 14–16)
  
  Before computing transformations from matched features, we review the hierarchy of 2D coordinate models established in **Module: Image Transformations**:  
  
  ```
                       The 2D Parametric Transformation Spectrum
                       
    Model           Degrees of Freedom (DOF)   Preserves                 Min. Matches Needed
    ────────────────────────────────────────────────────────────────────────────────────────
    Translation                2               Orientation, Lengths      1 Point Match
    Similarity                 4               Angles, Aspect Ratio      2 Point Matches
    Affine                     6               Parallelism, Midpoints    3 Point Matches
    Homography                 8               Collinearity, Cross-Ratio 4 Point Matches
  ```
  
  * **Affine Transformation (Slide 14–15):**
  $$\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} a & b & c \\ d & e & f \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} = \begin{bmatrix} \mathbf{A}_{2 \times 2} & \mathbf{t}_{2 \times 1} \\ \mathbf{0}^T & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$$
  * **Planar Homography / Projective Transformation (Slide 16):**
  $$\begin{bmatrix} x' \\ y' \\ w' \end{bmatrix} = \begin{bmatrix} h_{00} & h_{01} & h_{02} \\ h_{10} & h_{11} & h_{12} \\ h_{20} & h_{21} & h_{22} \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} \implies (x', y') = \left( \frac{h_{00}x + h_{01}y + h_{02}}{h_{20}x + h_{21}y + h_{22}}, \, \frac{h_{10}x + h_{11}y + h_{12}}{h_{20}x + h_{21}y + h_{22}} \right)$$
  ---
## 3. The Simplest Case: Solving for 2D Translation (Slides 17–24)
  
  Slides 17–24 construct the estimation framework using the simplest geometric motion model: **pure 2D spatial translation**.  
  
  ```
                           Pure Translation Alignment
                           
       Reference Image I(x, y)                         Shifted Image I'(x', y')
    ┌───────────────────────────┐                   ┌───────────────────────────┐
    │                           │                   │                           │
    │         • (x_i, y_i)      │     Shift by      │                           │
    │        /                  │    (t_x, t_y)     │                  • (x'_i, y'_i)
    │       / Boat              │ ────────────────► │                 /
    │      /_______             │                   │                / Boat
    │                           │                   │               /_______
    └───────────────────────────┘                   └───────────────────────────┘
  ```
  
  ---
### A. Degrees of Freedom vs. Number of Constraints (Slides 20–24)
  The translation transformation matrix in homogeneous coordinates is (Slide 17):  
  
  $$\mathbf{T}_{\text{translate}} = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}$$  
  1. **How many free parameters? (Slide 20–21):**
  * There are **2 free parameters**: the horizontal shift $t_x$ and the vertical shift $t_y$ .
  2. **How many feature matches do you need? (Slide 22–24):**
  * Each feature correspondence $(x_i, y_i) \leftrightarrow (x'_i, y'_i)$ supplies **two independent scalar equations**:
  $$x'_i = x_i + t_x$$ $$y'_i = y_i + t_y$$
  * Since each match yields 2 equations and we have 2 unknowns:
  $$\text{Minimum Matches Needed} = \frac{2 \text{ unknowns}}{2 \text{ equations / match}} = \mathbf{1 \text{ Match}}$$ A single point correspondence $(x_1, y_1) \leftrightarrow (x'_1, y'_1)$ is mathematically sufficient to solve for translation:
  $$t_x = x'_1 - x_1, \quad t_y = y'_1 - y_1$$
  ---
## 4. The Overdetermined Translation Problem & Inhomogeneous Least Squares (Slides 25–30)
  
  Slide 25 and 26 introduce a practical limitation:  
  > *"In practice, using only a single feature match is asking for trouble: any errors in the matching process will result in incorrect transformation parameters.*  
  *Let's use more matches ($N \gg 1$) and solve for the optimal parameters as a Least Squares optimization problem."*  
  
  ```
       Exact Fit (1 Match, Slide 24)                     Overdetermined Fit (N Matches, Slide 25–26)
    ┌──────────────────────────────────┐               ┌──────────────────────────────────┐
    │                                  │               │             • ───────► •         │
    │        • ──────────────► •       │               │        • ────────► •             │
    │        (x₁, y₁)          (x'₁, y'₁)│               │              • ────────► •       │
    │                                  │               │     Every match has slightly     │
    │    Subject to sensor noise,      │               │     different displacements      │
    │    feature jitter, or mismatch!  │               │     due to measurement noise!    │
    └──────────────────────────────────┘               └──────────────────────────────────┘
  ```
  
  ---
### A. Defining the Residual Errors (Slide 27)
  Let $\{(x_i, y_i) \leftrightarrow (x'_i, y'_i)\}_{i=1}^n$ be a set of $n$ noisy feature matches.  
  For any candidate translation vector $\mathbf{t} = \begin{bmatrix} t_x & t_y \end{bmatrix}^T$ , the coordinate residuals $r_{x_i}$ and $r_{y_i}$ are:  
  
  $$r_{x_i}(t_x) = (x_i + t_x) - x'_i$$  
  $$r_{y_i}(t_y) = (y_i + t_y) - y'_i$$  
  * If the matches were noise-free and the model held identically, $r_{x_i} = r_{y_i} = 0$ for all $i$ .
  * Under measurement noise, residuals are non-zero.
  ---
### B. The Least Squares Cost Function (Slide 28)
  Slide 28 defines the global cost function as the **Sum of Squared Residuals (SSE)**:  
  
  $$C(t_x, t_y) = \sum_{i=1}^n \left[ \big(r_{x_i}(t_x)\big)^2 + \big(r_{y_i}(t_y)\big)^2 \right] = \sum_{i=1}^n \left[ (x_i + t_x - x'_i)^2 + (y_i + t_y - y'_i)^2 \right]$$  
  ---
### C. Analytical Minimization: Deriving the Average Displacement (Slide 28)
  Because the horizontal residuals depend only on $t_x$ and the vertical residuals depend only on $t_y$ , the optimization decouples into two independent scalar convex quadratic problems.
#### 1. Minimizing with respect to $t_x$ :
  Take the partial derivative of $C$ with respect to $t_x$ and set it to $0$ :  
  
  $$\frac{\partial C}{\partial t_x} = \sum_{i=1}^n 2(x_i + t_x - x'_i)(1) = 0$$  
  $$\sum_{i=1}^n (x_i + t_x - x'_i) = 0 \implies \sum_{i=1}^n t_x = \sum_{i=1}^n (x'_i - x_i)$$  
  Since $t_x$ is constant across the summation, $\sum_{i=1}^n t_x = n \cdot t_x$ :  
  
  $$n \cdot t_x = \sum_{i=1}^n (x'_i - x_i) \implies \mathbf{t_x = \frac{1}{n} \sum_{i=1}^n (x'_i - x_i)}$$
#### 2. Minimizing with respect to $t_y$ :
  Applying the same differentiation with respect to $t_y$ :  
  
  $$\frac{\partial C}{\partial t_y} = \sum_{i=1}^n 2(y_i + t_y - y'_i)(1) = 0 \implies \mathbf{t_y = \frac{1}{n} \sum_{i=1}^n (y'_i - y_i)}$$  
  > **Key Rule (Slide 28):**   
  For a pure translation model, the optimal least squares shift $[\hat{t}_x, \hat{t}_y]^T$ is simply the **sample mean (centroid) of the displacement vectors** across all point correspondences.  
  
  ---
## 5. Matrix Formulation & The Normal Equations (Slides 29–30)
  
  While pure translation can be solved using scalar averages, translating the problem into a matrix equation establishes the template for estimating more complex affine transformations and homographies.  
  
  Slide 29 sets up the matrix equation:  
  
  $$\mathbf{A} \mathbf{t} = \mathbf{b}$$  
  ```
                ┌────────────────────────────────────────────────────────┐
                │        Overdetermined Translation Linear System        │
                │                                                        │
                │        ┌  1   0 ┐                  ┌ x'₁ - x₁ ┐        │
                │        │  0   1 │                  │ y'₁ - y₁ │        │
                │        │  1   0 │                  │ x'₂ - x₂ │        │
                │        │  0   1 │  ·  ┌ t_x ┐   =  │ y'₂ - y₂ │        │
                │        │  :   : │     └ t_y ┘      │    :     │        │
                │        │  1   0 │                  │ x'_n - x_n│       │
                │        └  0   1 ┘                  └ y'_n - y_n┘       │
                │             A             t              b             │
                │          (2n × 2)       (2 × 1)       (2n × 1)         │
                └────────────────────────────────────────────────────────┘
  ```
  
  ---
### A. Derivation of the Matrix Normal Equations (Slide 30)
  We seek the vector $\mathbf{t} = \begin{bmatrix} t_x & t_y \end{bmatrix}^T$ that minimizes the squared Euclidean residual norm:  
  
  $$\min_{\mathbf{t}} \| \mathbf{A}\mathbf{t} - \mathbf{b} \|_2^2$$  
  From **Module M5.0 (Part 1)**, differentiating with respect to $\mathbf{t}$ yields the **Normal Equations**:  
  
  $$\mathbf{A}^T \mathbf{A} \mathbf{t} = \mathbf{A}^T \mathbf{b}$$  
  $$\mathbf{t} = (\mathbf{A}^T \mathbf{A})^{-1} \mathbf{A}^T \mathbf{b}$$  
  ---
### B. Analytical Verification of Matrix Equivalence
  Evaluate the matrix products explicitly to confirm equivalence with our scalar derivation:
#### 1. Evaluate $\mathbf{A}^T \mathbf{A}$ :
  
  $$\mathbf{A}^T \mathbf{A} = \begin{bmatrix} 1 & 0 & 1 & 0 & \dots & 1 & 0 \\ 0 & 1 & 0 & 1 & \dots & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 \\ 0 & 1 \\ 1 & 0 \\ 0 & 1 \\ \vdots & \vdots \\ 1 & 0 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} \sum_{i=1}^n 1 & 0 \\ 0 & \sum_{i=1}^n 1 \end{bmatrix} = \begin{bmatrix} n & 0 \\ 0 & n \end{bmatrix} = n \mathbf{I}_{2 \times 2}$$  
  Notice that the inverse is straightforward:  
  
  $$(\mathbf{A}^T \mathbf{A})^{-1} = \frac{1}{n} \mathbf{I}_{2 \times 2} = \begin{bmatrix} \frac{1}{n} & 0 \\ 0 & \frac{1}{n} \end{bmatrix}$$
#### 2. Evaluate $\mathbf{A}^T \mathbf{b}$ :
  
  $$\mathbf{A}^T \mathbf{b} = \begin{bmatrix} 1 & 0 & 1 & 0 & \dots & 1 & 0 \\ 0 & 1 & 0 & 1 & \dots & 0 & 1 \end{bmatrix} \begin{bmatrix} x'_1 - x_1 \\ y'_1 - y_1 \\ x'_2 - x_2 \\ y'_2 - y_2 \\ \vdots \\ x'_n - x_n \\ y'_n - y_n \end{bmatrix} = \begin{bmatrix} \sum_{i=1}^n (x'_i - x_i) \\[4pt] \sum_{i=1}^n (y'_i - y_i) \end{bmatrix}$$
#### 3. Compute $\mathbf{t} = (\mathbf{A}^T \mathbf{A})^{-1} \mathbf{A}^T \mathbf{b}$ :
  
  $$\mathbf{t} = \begin{bmatrix} \frac{1}{n} & 0 \\ 0 & \frac{1}{n} \end{bmatrix} \begin{bmatrix} \sum_{i=1}^n (x'_i - x_i) \\[4pt] \sum_{i=1}^n (y'_i - y_i) \end{bmatrix} = \begin{bmatrix} \frac{1}{n} \sum_{i=1}^n (x'_i - x_i) \\[4pt] \frac{1}{n} \sum_{i=1}^n (y'_i - y_i) \end{bmatrix}$$  
  The general matrix normal equation reproduces the analytical scalar mean derivation.  
  
  ---
## 6. Python Implementation: Estimating Translation Transforms (Slides 31–32)
  
  Slide 32 reviews the implementation of translation alignment using `numpy.linalg.lstsq`.  
  
  ```python
  import numpy as np
  
  def compute_translation_transform_linalg(matches: list) -> np.ndarray:
    """
    Computes optimal 2D translation matrix using Inhomogeneous Least Squares.
    
    Parameters:
        matches: List of match pairs, where each element is:
                 [x, y, x_prime, y_prime]
                 
    Returns:
        3x3 homogeneous transformation matrix T_translate
    """
    N = len(matches)
    if N < 1:
        raise ValueError("At least 1 feature correspondence is required.")
        
    # Allocate system matrix A (2N x 2) and observation vector b (2N x 1)
    A = np.zeros((2 * N, 2), dtype=np.float64)
    b = np.zeros((2 * N, 1), dtype=np.float64)
    
    for i, m in enumerate(matches):
        x, y, x_prime, y_prime = m
        
        # Populate x-constraint row: 1*tx + 0*ty = (x' - x)
        A[2 * i, :] = [1.0, 0.0]
        b[2 * i]    = x_prime - x
        
        # Populate y-constraint row: 0*tx + 1*ty = (y' - y)
        A[2 * i + 1, :] = [0.0, 1.0]
        b[2 * i + 1]    = y_prime - y
        
    # Solve overdetermined system via QR / SVD-backed solver
    # Slide 30 note: "This is a built-in function in numpy (so don't do (A^T A)^-1 directly)"
    t, residuals, rank, s = np.linalg.lstsq(A, b, rcond=None)
    
    tx = t[0, 0]
    ty = t[1, 0]
    
    # Construct 3x3 homogeneous translation matrix (Slide 32)
    T_matrix = np.array([
        [1.0, 0.0, tx],
        [0.0, 1.0, ty],
        [0.0, 0.0, 1.0]
    ], dtype=np.float64)
    
    return T_matrix
  
  def compute_translation_analytical(matches: list) -> np.ndarray:
    """
    Optimized O(N) analytical solver using the closed-form displacement mean.
    """
    matches_arr = np.array(matches, dtype=np.float64) # (N, 4)
    
    # Extract coordinate displacements: delta_x = x' - x, delta_y = y' - y
    dx = matches_arr[:, 2] - matches_arr[:, 0]
    dy = matches_arr[:, 3] - matches_arr[:, 1]
    
    # Closed-form sample mean
    tx = np.mean(dx)
    ty = np.mean(dy)
    
    return np.array([
        [1.0, 0.0, tx],
        [0.0, 1.0, ty],
        [0.0, 0.0, 1.0]
    ])
  
  # -------------------------------------------------------------
  # Verification on Noisy Feature Matches
  # -------------------------------------------------------------
  np.random.seed(42)
  true_tx, true_ty = 42.5, -18.3
  
  # Generate 100 synthetic matches corrupted by Gaussian localization noise
  x_coords = np.random.uniform(0, 500, 100)
  y_coords = np.random.uniform(0, 500, 100)
  noise_x = np.random.normal(0, 0.75, 100)
  noise_y = np.random.normal(0, 0.75, 100)
  
  synth_matches = [
    [x, y, x + true_tx + nx, y + true_ty + ny]
    for x, y, nx, ny in zip(x_coords, y_coords, noise_x, noise_y)
  ]
  
  T_lstsq = compute_translation_transform_linalg(synth_matches)
  T_mean  = compute_translation_analytical(synth_matches)
  
  print(f"Ground Truth: tx = {true_tx:.2f}, ty = {true_ty:.2f}")
  print(f"NumPy lstsq:  tx = {T_lstsq[0, 2]:.4f}, ty = {T_lstsq[1, 2]:.4f}")
  print(f"Closed Mean:  tx = {T_mean[0, 2]:.4f}, ty = {T_mean[1, 2]:.4f}")
  assert np.allclose(T_lstsq, T_mean), "Linear solver and analytical mean must match identically!"
  ```
  
  ---
## Summary Matrix: The 2D Translation Model
  
  | Dimension | Analytical Formulation | Matrix Form | Numerical Role |
  |---|---|---|---|
  | **Model Parameters** | $t_x, t_y$ | $\mathbf{t} = [t_x, t_y]^T \in \mathbb{R}^2$ | 2 Degrees of Freedom |
  | **Minimal Data** | 1 Correspondence | 2 Scalar Equations | Exact algebraic solution |
  | **Overdetermined Data** | $N > 1$ Correspondences | $\mathbf{A}_{2N \times 2} \mathbf{t}_{2 \times 1} \approx \mathbf{b}_{2N \times 1}$ | Filters feature localization noise |
  | **Optimal Solution** | Sample mean of displacements | $\mathbf{t} = (\mathbf{A}^T \mathbf{A})^{-1} \mathbf{A}^T \mathbf{b}$ | Minimizes vertical/horizontal $L_2$ errors |
  | **Geometric Limit** | Pure 2D shift | Preserves all lengths, angles, and orientations | Fails under camera rotation or zoom |
  
  ---