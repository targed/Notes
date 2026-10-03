## 1. General Affine Transformation Alignment (Slides 35–42)
  
  In **Part 1**, we solved for pure 2D translation (2 DOF). However, as established in **Module: Image Transformations**, camera motion typically introduces in-plane rotation, scale changes, and shear.
  
  Slide 35 introduces the estimation of a full **Affine Transformation**:
  
  $$\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} a & b & c \\ d & e & f \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}$$
  
  $$\begin{cases} x' = ax + by + c \\ y' = dx + ey + f \end{cases}$$
  
  ```
                 The 6 Degrees of Freedom of an Affine Map
                 
        ┌── a     b   │   c ──┐   ◄── x-transformation parameters (a, b, c)
        │   d     e   │   f   │   ◄── y-transformation parameters (d, e, f)
        └── 0     0   │   1 ──┘
            └─────┬───┘   └──┬┘
                  │          │
       Scale, Rotation,      Translation
           Shear               (t_x, t_y)
  ```
  
  ---
### A. Free Parameters vs. Minimal Correspondences (Slides 36–37)
  1. **How many free parameters do we have? (Slide 36):**
   * There are **6 unknowns**: $[a, b, c, d, e, f]^T$.
  2. **How many point matches do we need? (Slide 37):**
   * Each 2D-to-2D point correspondence $(x_i, y_i) \leftrightarrow (x'_i, y'_i)$ supplies **two independent linear scalar constraints**:
     * Constraint 1: $ax_i + by_i + c = x'_i$
     * Constraint 2: $dx_i + ey_i + f = y'_i$
   * To solve for 6 unknowns:
  
  $$\text{Minimum Matches Needed} = \frac{6 \text{ unknowns}}{2 \text{ equations / match}} = \mathbf{3 \text{ Non-Collinear Point Matches}}$$
  
  Three non-collinear point matches provide $2 \times 3 = 6$ equations, uniquely determining an affine transformation.
  
  ---
### B. Defining the Affine Residuals and Objective Function (Slides 38–39)
  In practice, feature detectors extract tens or hundreds of noisy matches ($N \gg 3$). 
  
  Slide 38 defines the coordinate residuals for match $i$:
  
  $$r_{x_i}(a, b, c) = (ax_i + by_i + c) - x'_i$$
  
  $$r_{y_i}(d, e, f) = (dx_i + ey_i + f) - y'_i$$
  
  Slide 39 defines the global objective cost function as the **Sum of Squared Residuals**:
  
  $$C(a, b, c, d, e, f) = \sum_{i=1}^n \left[ \big(r_{x_i}(a, b, c)\big)^2 + \big(r_{y_i}(d, e, f)\big)^2 \right]$$
  
  $$C = \sum_{i=1}^n \left[ (ax_i + by_i + c - x'_i)^2 + (dx_i + ey_i + f - y'_i)^2 \right]$$
  
  * **Decoupling Property:**
  Because the parameters governing $x'$ ($a, b, c$) do not appear in the $y'$ residual ($d, e, f$), this optimization can be solved as two independent $3 \times 3$ linear systems or unified into a single $2N \times 6$ matrix equation.
  
  ---
### C. The Inhomogeneous Matrix Form: $\mathbf{A} \mathbf{t} = \mathbf{b}$ (Slide 40)
  Slide 40 expresses all $2N$ linear constraints in matrix form:
  
  ```
                ┌────────────────────────────────────────────────────────┐
                │          Overdetermined Affine Linear System           │
                │                                                        │
                │     ┌ x₁   y₁   1    0    0    0 ┐         ┌ a ┐       ┌ x'₁ ┐
                │     │  0    0   0   x₁   y₁    1 │         │ b │       │ y'₁ │
                │     │ x₂   y₂   1    0    0    0 │         │ c │       │ x'₂ │
                │     │  0    0   0   x₂   y₂    1 │    ·    │ d │   =   │ y'₂ │
                │     │  :    :   :    :    :    : │         │ e │       │  :  │
                │     │ x_n  y_n  1    0    0    0 │         └ f ┘       │ x'_n│
                │     └  0    0   0   x_n  y_n   1 ┘                     └ y'_n┘
                │                    A                        t             b   
                │                 (2n × 6)                 (6 × 1)       (2n × 1)
                └────────────────────────────────────────────────────────┘
  ```
#### Solving via the Normal Equations:
  This system is an **inhomogeneous linear least squares** problem ($\mathbf{b} \neq \mathbf{0}$). 
  From **Module M5.0**, the solution vector $\hat{\mathbf{t}} = [a, b, c, d, e, f]^T$ that minimizes $\|\mathbf{A}\mathbf{t} - \mathbf{b}\|_2^2$ is computed via:
  
  $$\mathbf{A}^T \mathbf{A} \mathbf{t} = \mathbf{A}^T \mathbf{b} \implies \hat{\mathbf{t}} = (\mathbf{A}^T \mathbf{A})^{-1} \mathbf{A}^T \mathbf{b} = \mathbf{A}^+ \mathbf{b}$$
  
  ---
### D. Python Implementation: Affine Transformation Estimator (Slides 41–42)
  Slide 42 provides starter code for `compute_affine_transform`:
  
  ```python
  import numpy as np
  
  def compute_affine_transform(matches: list) -> np.ndarray:
    """
    Computes the optimal 2D affine transformation matrix from N >= 3 matches
    using Inhomogeneous Least Squares (QR/SVD-backed pseudoinverse).
    
    Parameters:
        matches: List of match vectors [x, y, x_prime, y_prime]
        
    Returns:
        3x3 homogeneous transformation matrix T_affine
    """
    N = len(matches)
    if N < 3:
        raise ValueError("At least 3 point matches are required to fit an affine transformation.")
        
    # Allocate system matrix A (2N x 6) and target vector b (2N x 1)
    A = np.zeros((2 * N, 6), dtype=np.float64)
    b = np.zeros((2 * N, 1), dtype=np.float64)
    
    for i, m in enumerate(matches):
        x, y, x_prime, y_prime = m
        
        # Row 2*i: x-constraint -> a*x + b*y + c = x'
        A[2 * i, :] = [x, y, 1.0, 0.0, 0.0, 0.0]
        b[2 * i, 0] = x_prime
        
        # Row 2*i + 1: y-constraint -> d*x + e*y + f = y'
        A[2 * i + 1, :] = [0.0, 0.0, 0.0, x, y, 1.0]
        b[2 * i + 1, 0] = y_prime
        
    # Solve overdetermined system via NumPy's robust SVD/QR solver
    t, residuals, rank, s = np.linalg.lstsq(A, b, rcond=None)
    
    a, b_param, c, d, e, f = t.flatten()
    
    # Assemble 3x3 homogeneous matrix
    T_affine = np.array([
        [a,   b_param, c],
        [d,   e,       f],
        [0.0, 0.0,     1.0]
    ], dtype=np.float64)
    
    return T_affine
  ```
  
  ---
## 2. Solving for Planar Homographies: The Non-Linear Dilemma (Slides 43–45)
  
  Slide 43 revisits planar rectification (unwarping a receding floor in a hallway). 
  
  Slide 44 and 45 introduce a mathematical roadblock:
  > **The Problem with Homographies (Slide 44–45):**
  > 
  > $$\begin{bmatrix} x'_i \\ y'_i \\ 1 \end{bmatrix} \sim \begin{bmatrix} h_{00} & h_{01} & h_{02} \\ h_{10} & h_{11} & h_{12} \\ h_{20} & h_{21} & h_{22} \end{bmatrix} \begin{bmatrix} x_i \\ y_i \\ 1 \end{bmatrix}$$
  > 
  > Converting to Cartesian coordinates requires dividing by the projective scale factor $w' = h_{20}x_i + h_{21}y_i + h_{22}$:
  > 
  > $$x'_i = \frac{h_{00}x_i + h_{01}y_i + h_{02}}{h_{20}x_i + h_{21}y_i + h_{22}}, \quad y'_i = \frac{h_{10}x_i + h_{11}y_i + h_{12}}{h_{20}x_i + h_{21}y_i + h_{22}}$$
  > 
  > **The unknown parameters $h_{20}, h_{21}, h_{22}$ appear in the denominator!**
  > This makes the relationship rational and non-linear. We cannot set up a standard $\mathbf{A}\mathbf{t} = \mathbf{b}$ equation directly.
  
  ---
## 3. The Direct Linear Transform (DLT) Algorithm (Slides 46–47)
  
  Slide 46 shows how to linearize this system without approximations.
  
  ```
       Non-Linear Rational Equation                      Linear Algebraic Formulation
       
       x'_i = (h₀₀x + h₀₁y + h₀₂) / (h₂₀x + h₂₁y + h₂₂) ──► Multiply by Denominator!
                                                        ──► x'_i (h₂₀x + h₂₁y + h₂₂) = h₀₀x + h₀₁y + h₀₂
                                                        ──► Recovers an EXACT linear equation!
  ```
  
  ---
### A. Algebraic Derivation of the Linear Constraints (Slide 46)
  Multiply both sides of the coordinate equations by the denominator:
  
  $$x'_i (h_{20}x_i + h_{21}y_i + h_{22}) = h_{00}x_i + h_{01}y_i + h_{02}$$
  
  $$y'_i (h_{20}x_i + h_{21}y_i + h_{22}) = h_{10}x_i + h_{11}y_i + h_{12}$$
  
  Rearranging all terms to one side:
  
  $$h_{00}x_i + h_{01}y_i + h_{02} - x'_i h_{20}x_i - x'_i h_{21}y_i - x'_i h_{22} = 0$$
  
  $$h_{10}x_i + h_{11}y_i + h_{12} - y'_i h_{20}x_i - y'_i h_{21}y_i - y'_i h_{22} = 0$$
  
  Flatten the $3 \times 3$ matrix $\mathbf{H}$ into a 9-dimensional parameter vector:
  
  $$\mathbf{h} = \begin{bmatrix} h_{00} & h_{01} & h_{02} & h_{10} & h_{11} & h_{12} & h_{20} & h_{21} & h_{22} \end{bmatrix}^T \in \mathbb{R}^9$$
  
  We can express these two equations as a matrix-vector product for match $i$ (Slide 46):
  
  $$\begin{bmatrix} 
  x_i & y_i & 1 & 0 & 0 & 0 & -x'_i x_i & -x'_i y_i & -x'_i \\[6pt]
  0 & 0 & 0 & x_i & y_i & 1 & -y'_i x_i & -y'_i y_i & -y'_i 
  \end{bmatrix} \begin{bmatrix} h_{00} \\ h_{01} \\ h_{02} \\ h_{10} \\ h_{11} \\ h_{12} \\ h_{20} \\ h_{21} \\ h_{22} \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$$
  
  $$\mathbf{A}_i \, \mathbf{h} = \mathbf{0}$$
  
  Each matched point correspondence provides **2 linearly independent constraints** on the 9 entries of $\mathbf{h}$.
  
  ---
### B. Stacking $N$ Correspondences: The Full Linear System (Slide 47)
  Stacking the $2 \times 9$ constraint blocks across all $N$ point correspondences yields a homogeneous linear system:
  
  $$\begin{bmatrix} 
  x_1 & y_1 & 1 & 0 & 0 & 0 & -x'_1 x_1 & -x'_1 y_1 & -x'_1 \\
  0 & 0 & 0 & x_1 & y_1 & 1 & -y'_1 x_1 & -y'_1 y_1 & -y'_1 \\
  \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots \\
  x_n & y_n & 1 & 0 & 0 & 0 & -x'_n x_n & -x'_n y_n & -x'_n \\
  0 & 0 & 0 & x_n & y_n & 1 & -y'_n x_n & -y'_n y_n & -y'_n 
  \end{bmatrix} \begin{bmatrix} h_{00} \\ h_{01} \\ h_{02} \\ h_{10} \\ h_{11} \\ h_{12} \\ h_{20} \\ h_{21} \\ h_{22} \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \\ \vdots \\ 0 \\ 0 \end{bmatrix}$$
  
  $$\mathbf{A}_{2N \times 9} \, \mathbf{h}_{9 \times 1} = \mathbf{0}_{2N \times 1}$$
  
  ---
### C. Degrees of Freedom vs. Minimal Points (Slide 49–50)
  Slide 50 works through the math:
  1. $\mathbf{H}$ has 9 raw parameters.
  2. A homography is defined only **up to an overall global scale** (multiplying $\mathbf{H}$ by scalar $\lambda \neq 0$ cancels out in $x'/w'$ and $y'/w'$).
  3. Therefore, $\mathbf{H}$ has **8 Degrees of Freedom (DOF)**.
  4. Each match supplies 2 independent equations:
  
  $$\text{Minimum Matches Needed} = \frac{8 \text{ DOF}}{2 \text{ equations / match}} = \mathbf{4 \text{ Point Matches}}$$
  
  > **The 4-Point Collinearity Constraint:** 
  > To solve for a homography, you must have at least **4 matching point pairs**, with the requirement that **no three points are collinear** (they cannot lie on the same line in either image).
  
  ---
## 4. Solving Homogeneous Systems via SVD (Slides 48–50)
  
  Slide 48 and 49 formalize the optimization:
  
  $$\min_{\mathbf{h}} \| \mathbf{A}\mathbf{h} - \mathbf{0} \|_2^2 \iff \min_{\mathbf{h}} \| \mathbf{A}\mathbf{h} \|_2^2 \quad \text{subject to } \| \mathbf{h} \|_2 = 1$$
  
  ```
       Why the Pseudo-Inverse FAILS on Homogeneous Systems:
       
       • Inhomogeneous: A·x = b (b ≠ 0)  ──► x = (A^T A)⁻¹ A^T b  (Works!)
       • Homogeneous:   A·h = 0          ──► h = (A^T A)⁻¹ A^T 0 = 0 (TRIVIAL SOLUTION!)
       
       We MUST constrain ||h|| = 1 to prevent collapsing to the zero vector.
       This is solved via SINGULAR VALUE DECOMPOSITION (SVD)!
  ```
  
  ---
### Step-by-Step SVD Solution (Slide 49–50)
  From the Rayleigh Quotient theorem in **Module M5.0 (Part 2)**:
  1. Construct the $2N \times 9$ measurement matrix $\mathbf{A}$.
  2. Compute the Singular Value Decomposition (SVD) of $\mathbf{A}$:
  
  $$\mathbf{A} = \mathbf{U} \mathbf{\Sigma} \mathbf{V}^T$$
  
   where $\mathbf{\Sigma} = \text{diag}(\sigma_1, \sigma_2, \dots, \sigma_9)$ with $\sigma_1 \ge \sigma_2 \ge \dots \ge \sigma_9 \ge 0$.
  3. The solution vector $\mathbf{h}^*$ that minimizes $\|\mathbf{A}\mathbf{h}\|_2^2$ subject to $\|\mathbf{h}\|_2 = 1$ is the **singular vector corresponding to the smallest singular value $\sigma_9$**:
  
  $$\mathbf{h}^* = \mathbf{v}_9 \quad \text{(The 9th column of } \mathbf{V} \text{, or the last row of } \mathbf{V}^T \text{)}$$
  
  4. Reshape the 9-element vector $\mathbf{h}^*$ back into the $3 \times 3$ matrix $\mathbf{H}$:
  
  $$\mathbf{H} = \begin{bmatrix} h_1 & h_2 & h_3 \\ h_4 & h_5 & h_6 \\ h_7 & h_8 & h_9 \end{bmatrix}$$
  
  5. (Optional) Normalize $\mathbf{H}$ by dividing by its bottom-right entry ($H \leftarrow H / H_{2, 2}$).
  
  ---
## 5. Critical Practical Enhancement: Hartley's Data Normalization
  
  In raw pixel space, image coordinates span values from $[0, 1000]$ or higher:
  * The first columns of $\mathbf{A}$ contain raw terms: $x_i \approx 10^3$.
  * The last columns of $\mathbf{A}$ contain cross-products: $x'_i x_i \approx 10^3 \times 10^3 = 10^6$.
  * The constant columns contain: $1 = 10^0$.
  
  ```
       Conditioning Disaster in Raw DLT Matrix A:
       
       ┌  x_i     y_i     1     0     0     0    -x'x    -x'y    -x' ┐
       │ ~10³    ~10³   ~10⁰    0     0     0    ~10⁶    ~10⁶   ~10³ │
       └─────────────────────────────────────────────────────────────┘
       Matrix entries span 6 ORDERS OF MAGNITUDE!
       Results in severe numerical instability and poor condition numbers.
  ```
  
  In his paper *"In Defense of the Point Normalization"* (1997) [1], Richard Hartley showed that solving raw DLT without preconditioning yields inaccurate homographies due to floating-point truncation.
  
  ---
### The Normalized DLT Algorithm (Hartley Preconditioning)
  Before assembling matrix $\mathbf{A}$, apply an isotropic coordinate normalization to both point sets:
  
  ```
                           Hartley's 2-Step Normalization Rule
                           
   1. Translate points so their CENTROID is at the origin:  (x̄, ȳ) = (0, 0)
   2. Scale coordinates so the AVERAGE DISTANCE to origin is: d_avg = √2
  ```
  
  1. **Compute Normalization Matrices $\mathbf{T}$ and $\mathbf{T}'$:**
  
  $$\mathbf{T} = \begin{bmatrix} s & 0 & -s \bar{x} \\ 0 & s & -s \bar{y} \\ 0 & 0 & 1 \end{bmatrix}, \quad \text{where } s = \frac{\sqrt{2}}{\frac{1}{N}\sum_{i=1}^N \sqrt{(x_i - \bar{x})^2 + (y_i - \bar{y})^2}}$$
  
  2. **Normalize Points:** $\tilde{\mathbf{x}}_i = \mathbf{T} \mathbf{x}_i$ and $\tilde{\mathbf{x}}'_i = \mathbf{T}' \mathbf{x}'_i$.
  3. **Run DLT via SVD:** Solve $\tilde{\mathbf{A}} \tilde{\mathbf{h}} = \mathbf{0}$ using normalized coordinates, obtaining $\tilde{\mathbf{H}}$.
  4. **Denormalize the Homography:** 
  
  $$\mathbf{H} = (\mathbf{T}')^{-1} \, \tilde{\mathbf{H}} \, \mathbf{T}$$
  
  ---
## 6. Complete Python Implementation: Normalized DLT Homography Estimator
  
  ```python
  import numpy as np
  
  def compute_homography_dlt(matches: list, normalize: bool = True) -> np.ndarray:
    """
    Fits an 8-DOF Planar Homography matrix H using the Direct Linear Transform (DLT)
    via SVD, with optional Hartley isotropic coordinate preconditioning.
    
    Parameters:
        matches: List of [x, y, x_prime, y_prime]
        normalize: If True, applies Hartley coordinate normalization (recommended).
        
    Returns:
        (3, 3) projective transformation matrix H.
    """
    pts_src = np.array([[m[0], m[1]] for m in matches], dtype=np.float64) # (N, 2)
    pts_dst = np.array([[m[2], m[3]] for m in matches], dtype=np.float64) # (N, 2)
    N = pts_src.shape[0]
    
    if N < 4:
        raise ValueError("At least 4 point correspondences are required to estimate a homography.")
        
    if normalize:
        # ---------------------------------------------------------
        # Step 1: Compute Hartley Normalization Transform Matrices
        # ---------------------------------------------------------
        def get_norm_matrix(pts):
            centroid = np.mean(pts, axis=0)
            shifted = pts - centroid
            mean_dist = np.mean(np.sqrt(np.sum(shifted**2, axis=1)))
            scale = np.sqrt(2.0) / (mean_dist + 1e-12)
            
            T = np.array([
                [scale, 0.0,   -scale * centroid[0]],
                [0.0,   scale, -scale * centroid[1]],
                [0.0,   0.0,    1.0]
            ])
            return T
            
        T_src = get_norm_matrix(pts_src)
        T_dst = get_norm_matrix(pts_dst)
        
        # Apply normalization: [x, y, 1]^T -> T @ [x, y, 1]^T
        pts_src_h = np.hstack([pts_src, np.ones((N, 1))])
        pts_dst_h = np.hstack([pts_dst, np.ones((N, 1))])
        
        norm_src = (T_src @ pts_src_h.T).T
        norm_dst = (T_dst @ pts_dst_h.T).T
        
        x_s, y_s = norm_src[:, 0], norm_src[:, 1]
        x_d, y_d = norm_dst[:, 0], norm_dst[:, 1]
    else:
        x_s, y_s = pts_src[:, 0], pts_src[:, 1]
        x_d, y_d = pts_dst[:, 0], pts_dst[:, 1]
        
    # ---------------------------------------------------------
    # Step 2: Construct the (2N x 9) Measurement Matrix A
    # ---------------------------------------------------------
    A = np.zeros((2 * N, 9), dtype=np.float64)
    for i in range(N):
        xi, yi = x_s[i], y_s[i]
        ui, vi = x_d[i], y_d[i]
        
        # Row 2*i:   [ xi, yi, 1,   0,   0, 0,  -ui*xi, -ui*yi, -ui ]
        A[2 * i, :]     = [xi, yi, 1.0, 0.0, 0.0, 0.0, -ui * xi, -ui * yi, -ui]
        # Row 2*i+1: [  0,  0, 0,  xi,  yi, 1,  -vi*xi, -vi*yi, -vi ]
        A[2 * i + 1, :] = [0.0, 0.0, 0.0, xi, yi, 1.0, -vi * xi, -vi * yi, -vi]
        
    # ---------------------------------------------------------
    # Step 3: Solve Homogeneous System via SVD (Slide 49)
    # ---------------------------------------------------------
    # A = U @ diag(S) @ Vt
    U, S, Vt = np.linalg.svd(A)
    
    # Solution h is the last row of Vt (smallest singular value)
    h_norm = Vt[-1, :]
    H_matrix = h_norm.reshape((3, 3))
    
    # ---------------------------------------------------------
    # Step 4: Denormalization (Hartley)
    # ---------------------------------------------------------
    if normalize:
        H_matrix = np.linalg.inv(T_dst) @ H_matrix @ T_src
        
    # Normalize so H[2, 2] = 1.0
    if np.abs(H_matrix[2, 2]) > 1e-12:
        H_matrix /= H_matrix[2, 2]
        
    return H_matrix
  ```
  
  ---
## 7. The Outlier Frontier: Why Least Squares Breaks Down (Slide 51)
  
  Slide 51 presents a visual reality check:
  * When feature descriptors (such as SIFT) are matched across cluttered desktop scenes (e.g., matching the comic book cover against a messy table):
  * **Inliers (Green Oval):** Most correspondences lock onto true physical correspondences on the cover.
  * **Outliers (Red Circles):** A few correspondences match corners on the comic to unrelated objects (e.g., an arbitrary high-contrast reflection on a water bottle cap).
  
  ```
                            The Outlier Vulnerability of Least Squares
                            
             ^ Residual Error r_i²
             │                                              • Outlier Match (Distance r = 200px)
       40000 │                                                Squared Error: r² = 40,000!
             │                                                COMPLETELY DOMINATES
             │                                                AND DISTORTS THE FIT!
             │
         100 │  • Inlier Match (r = 10px, r² = 100)
           1 │  • Inlier Match (r = 1px,  r² = 1)
           0 ┴──┴───────────────────────────────────────────► Match Index
  ```
### The Breakdown:
  * **The Quadratic Penalty Problem:** 
  Standard least squares minimizes $\sum r_i^2$. 
  An inlier might have a localization error of $1\text{ pixel}$ ($r^2 = 1$). 
  A single false outlier match across the room has an error of $200\text{ pixels}$ ($r^2 = 40,000$).
  * **The Result:** The optimizer compromises the fit of hundreds of good inlier points to reduce the quadratic residual of a single bad outlier. The resulting homography warps into an unusable shape.
  
  > **Slide 51 Takeaway:** 
  > Least squares works well under Gaussian measurement noise, but **breaks down in the presence of false correspondences**. 
  > Robust estimation requires an algorithm capable of identifying and rejecting outliers: **RANSAC (Random Sample Consensus)**.
  
  ---
## Summary Matrix: The 2D Alignment Hierarchy
  
  | Motion Model | Unknowns | Equation Type | Minimal Points | Objective Formulation | Solver Engine |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | **Translation** | 2 ($t_x, t_y$) | Linear | 1 Point | $\min \|\mathbf{A}\mathbf{t} - \mathbf{b}\|_2^2$ | Centroid displacement or Normal Equations |
  | **Affine** | 6 ($a, b, c, d, e, f$) | Linear | 3 Points | $\min \|\mathbf{A}\mathbf{t} - \mathbf{b}\|_2^2$ | Inhomogeneous Least Squares (`np.linalg.lstsq`) |
  | **Homography** | 8 ($h_{00} \dots h_{22}$) | Rational / Non-Linear | **4 Points** | $\min \|\mathbf{A}\mathbf{h}\|_2^2 \text{ s.t. } \|\mathbf{h}\|=1$ | **Direct Linear Transform (DLT) via SVD** |
  
  ---