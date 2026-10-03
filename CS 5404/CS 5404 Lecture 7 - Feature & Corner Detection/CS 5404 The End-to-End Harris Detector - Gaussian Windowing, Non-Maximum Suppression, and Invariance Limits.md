## 1. Window Weighting: Box vs. Gaussian Integration (Slide 55)
  
  In **Part 2**, we derived the Structure Tensor $\mathbf{H}$ using a generic window summation:
  
  $$\mathbf{H} = \sum_{(x, y) \in W} w(x, y) \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}$$
  
  Slide 55 addresses how we choose the spatial window weighting $w(x, y)$:
  > *"In practice, we weight the derivatives (since a uniform sum/average doesn't work too well). We can weight the pixels based on their distance from the center pixel. A Gaussian Kernel is a common choice. Closer gradients are more relevant to whether THIS pixel is a corner."*
  
  ```
     Uniform Box Window w(x, y)                     Gaussian Window w(x, y)
          ┌───────────────┐                                  ╭─────╮
          │ 1   1   1   1 │                                ╭─╯     ╰─╮
          │ 1   1   1   1 │                              ╭─╯         ╰─╮
          │ 1   1   1   1 │                         ─────┴─────────────┴─────
          └───────────────┘                         • Continuous isotropic falloff
          • Anisotropic (axis-aligned corners)      • Rotational invariance guaranteed
          • Sharp cutoffs introduce noise           • Gradients near center have highest weight
  ```
  
  ---
### A. Why a Uniform Box Window Fails
  If $w(x, y)$ is an unweighted rectangular box ($w = 1$ inside, $0$ outside):
  1. **Anisotropy:** A square window treats diagonal neighbors differently from horizontal/vertical neighbors (the corners of the square reach out to a distance of $\sqrt{2}r$, while the edges stop at $r$). If the image is rotated, different pixels enter the corners of the window, causing the computed eigenvalues to fluctuate arbitrarily.
  2. **Abrupt Boundaries:** Pixels at the boundary abruptly jump into and out of the summation as the window slides, making the response map noisy and sensitive to single-pixel shifts.
  
  ---
### B. The Gaussian Window Formulation (Slide 55)
  Using a 2D continuous isotropic Gaussian filter for integration:
  
  $$w(x, y) = g_{\sigma_i}(x, y) = \frac{1}{2\pi\sigma_i^2} \exp\left( -\frac{x^2 + y^2}{2\sigma_i^2} \right)$$
  
  Applying this weighting allows the components of the Structure Tensor to be computed via **separable 2D convolutions**:
  
  $$A = g_{\sigma_i} * I_x^2, \quad B = g_{\sigma_i} * (I_x I_y), \quad C = g_{\sigma_i} * I_y^2$$
  
  $$\mathbf{H}(x, y) = \begin{bmatrix} A(x, y) & B(x, y) \\ B(x, y) & C(x, y) \end{bmatrix}$$
  
  ---
### C. The Two Fundamental Scales of Feature Detection
  The Harris detector incorporates two distinct, decoupled spatial scales:
  
  ```
                      The Two Scales of the Harris Detector
                      
   Input Image I ──► [ Gaussian Blur G_σ_d ] ──► [ Derivative Filter ] ──► Gradients (Ix, Iy)
                     Differentiation Scale σ_d
                     (Noise suppression scale)
                                                                                  │
                                                                                  ▼
   Structure Tensor H ◄── [ Gaussian Blur G_σ_i ] ◄── Outer Products (Ix², Iy², Ix·Iy)
                          Integration Scale σ_i
                          (Neighborhood size scale)
  ```
  
  1. **Differentiation Scale ($\sigma_d$):**
   * Controls the scale of the low-pass filter used prior to gradient computation ($I_x = \frac{\partial}{\partial x}(I * G_{\sigma_d})$).
   * Suppresses high-frequency sensor noise and eliminates micro-texture fluctuations.
  2. **Integration Scale ($\sigma_i$, where $\sigma_i > \sigma_d$):**
   * Controls the standard deviation of the Gaussian window $w(x, y)$ that accumulates local gradient energy.
   * Defines the physical neighborhood size over which cornerness is evaluated.
   * **Rule of Thumb:** In practical pipelines, the integration scale is set to:
  
  $$\sigma_i \approx 1.5 \cdot \sigma_d \quad \text{to} \quad 2.0 \cdot \sigma_d$$
  
  ---
## 2. The Complete 5-Step Harris Corner Detection Pipeline (Slides 48–53)
  
  Slide 53 summarizes the end-to-end algorithmic flow:
  
  $$I \longrightarrow (I_x, I_y) \longrightarrow \mathbf{H} \longrightarrow R = \det(\mathbf{H}) - k \cdot \text{tr}(\mathbf{H})^2 \longrightarrow \text{Threshold} \longrightarrow \text{Non-Maximum Suppression} \longrightarrow \text{Corners}$$
  
  ```
                ┌─────────────────────────────────────────────────────────┐
                │          Step 1: Compute Spatial Derivatives            │
                │     Convolve image with horizontal & vertical Sobel:    │
                │          Ix = I * S_x,      Iy = I * S_y                │
                └────────────────────────────┬────────────────────────────┘
                                             │
                                             ▼
                ┌─────────────────────────────────────────────────────────┐
                │        Step 2: Form Element-Wise Tensor Products        │
                │    Compute pointwise squared & cross derivatives:       │
                │         Ixx = Ix²,    Iyy = Iy²,    Ixy = Ix · Iy       │
                └────────────────────────────┬────────────────────────────┘
                                             │
                                             ▼
                ┌─────────────────────────────────────────────────────────┐
                │     Step 3: Gaussian Integration (Structure Tensor)     │
                │  Smooth the products using integration Gaussian G_σ_i:  │
                │       A = G_σ_i * Ixx,  B = G_σ_i * Ixy,  C = G_σ_i * Iyy│
                └────────────────────────────┬────────────────────────────┘
                                             │
                                             ▼
                ┌─────────────────────────────────────────────────────────┐
                │        Step 4: Compute Corner Response Map R            │
                │          det(H) = A·C - B²,     tr(H) = A + C           │
                │             R = det(H) - k · (tr(H))²                   │
                └────────────────────────────┬────────────────────────────┘
                                             │
                                             ▼
                ┌─────────────────────────────────────────────────────────┐
                │       Step 5: Thresholding & Non-Maximum Suppression    │
                │ 1. Threshold: Discard all points where R(x, y) < τ      │
                │ 2. NMS: Retain only pixels that are LOCAL MAXIMA in 3×3 │
                └─────────────────────────────────────────────────────────┘
  ```
  
  ---
### Detailed Examination of Step 5: Thresholding & Non-Maximum Suppression (NMS)
  
  ```
        Raw Response Map R > τ                           After 3×3 Non-Maximum Suppression
     ┌───────────────────────────┐                         ┌───────────────────────────┐
     │          ░░███░░          │                         │                           │
     │         ░███████░         │     Keep ONLY the       │                           │
     │          ░░███░░          │ ──► single highest  ──► │             •             │
     │                           │     local peak pixel!   │                           │
     │                           │                         │                           │
     └───────────────────────────┘                         └───────────────────────────┘
      Thick multi-pixel cluster                             Single, sub-pixel accurate point
  ```
  
  1. **Global Thresholding (Slide 52):**
   A candidate pixel must have a response exceeding an absolute or relative threshold:
  
  $$\text{Candidate}(x, y) \iff R(x, y) > \tau_{\text{threshold}}$$
  
   * A common adaptive heuristic sets $\tau = \alpha \cdot \max_{(u, v)} R(u, v)$, with $\alpha \in [0.01, 0.05]$.
  2. **Non-Maximum Suppression (NMS — Slide 53):**
   Because Gaussian smoothing spreads energy across a neighborhood, a single physical corner produces a multi-pixel cluster of high $R$ values. 
   * To prevent duplicate detections, NMS tests whether a candidate pixel is strictly greater than all $8$ of its immediate spatial neighbors:
  
  $$\text{Corner}(x, y) \iff R(x, y) > \tau \quad \text{AND} \quad R(x, y) \ge \max_{(i, j) \in \mathcal{N}_8(x, y)} R(x + i, \, y + j)$$
  
  ---
## 3. Numerical Issues & Visualization (Slide 54)
  
  Slide 54 includes an important practical note:
  > *"Sometimes the value of $f$ may be undefined (if the denominator is zero) or your plot of $\log(f)$ may look odd (since $\log(0) = -\infty$). As long as it doesn't change where your corners come from, don't worry about it."*
  
  ```
       Visualizing Wide Dynamic Range: The Logarithmic Compression
       
       Raw Linear Response Map R:                        Log-Compressed Response:
       Corner peaks:   R ~ 10⁸                           log(1 + max(0, R))
       Edges:          R ~ -10⁷                          Compresses 8 orders of magnitude
       Flat regions:   R ~ 0                             into a smooth, visible heatmap!
  ```
  
  ---
### A. Preventing Division by Zero in the Noble Operator
  Recall Noble’s harmonic mean corner operator from **Part 3**:
  
  $$f_{\text{Noble}} = \frac{\det(\mathbf{H})}{\text{tr}(\mathbf{H})} = \frac{AC - B^2}{A + C}$$
  
  * In flat, uniform regions (such as empty skies or solid walls), $I_x \approx 0$ and $I_y \approx 0 \implies A + C = 0$.
  * Evaluating $\frac{0}{0}$ returns a floating-point `NaN` (Not a Number) or `Inf`.
  * **The Numerical Fix:** Add a small machine-epsilon stabilization constant $\epsilon$ to the denominator:
  
  $$f_{\text{Noble}} = \frac{AC - B^2}{(A + C) + \epsilon}, \quad \text{where } \epsilon = 10^{-12}$$
  
  ---
### B. Dynamic Range Compression for Visualization (Slide 51, 54)
  The raw Harris response $R$ has a massive dynamic range because it involves fourth-order powers of image gradients:
  
  $$R \sim (\nabla I)^4$$
  
  * While flat regions evaluate near $0$, high-contrast corners easily reach values of $10^8$ or higher. Displaying $R$ directly on a standard monitor ($[0, 255]$) leaves all details black except for a few isolated white pixels.
  * **The Log Transform:** To visualize the response field (as shown on Slides 51 and 57):
  
  $$R_{\text{vis}}(x, y) = \log\big( 1.0 + \max(0, \, R(x, y)) \big)$$
  
  ---
## 4. Empirical Performance: Multi-View Repeatability (Slides 56–60)
  
  Slides 56–60 illustrate the Harris detector running on two photographs of a spotted toy cow taken from different viewpoints, camera distances, and poses:
  
  ```
        Frame 1: Cow Standing Upright                   Frame 2: Cow Rotated / View Shifted
     ┌──────────────────────────────────┐            ┌──────────────────────────────────┐
     │         (•)                      │            │               (•)                │
     │        / | \   • Ear Corner      │            │              / | \   • Ear Corner│
     │       (  O  )                    │            │             (  O  )              │
     │       /  |  \                    │            │             /  |  \              │
     │      •   •   • Spots             │            │            •   •   • Spots       │
     │     /    │    \                  │            │           /    │    \            │
     │    •     •     • Hooves          │            │          •     •     • Hooves    │
     └──────────────────────────────────┘            └──────────────────────────────────┘
      Detected 45 Harris Corners                      Detected 43 Harris Corners
      Repeatable matches: ~85% of detected points lock onto identical physical features!
  ```
  
  Slide 60 highlights the result:
  > **"Notice that the features are in similar locations between the two images."**
### A. The Repeatability Metric
  The standard benchmark for evaluating a local feature detector across camera motion is **Repeatability**:
  
  $$\text{Repeatability} = \frac{\text{Number of Mutual Inlier Correspondences}}{\min(N_1, N_2)}$$
  
  A detected point in Image 1 is considered repeatable if its projection through the true geometric ground-truth transformation falls within an $\epsilon$-radius (typically $\epsilon \le 1.5\text{ pixels}$) of a detected point in Image 2.
  
  ---
## 5. Invariance Analysis of the Harris Detector
  
  For a feature detector to support reliable visual odometry, SLAM, and panorama stitching, its detections must remain **invariant** to geometric and photometric transformations.
  
  ```
                         Invariance Profile of the Harris Detector
                                             │
      ┌──────────────────────┬───────────────┴───────────────┬──────────────────────┐
      ▼                      ▼                               ▼                      ▼
  Translation             Rotation                  Affine Illumination           Scale
  INVARIANT              INVARIANT                   MOSTLY INVARIANT           NOT INVARIANT!
  ```
  
  ---
### A. Translation Invariance: $\checkmark$ Fully Invariant
  * Convolutions, spatial derivatives, and Gaussian window summations are **Linear Shift-Invariant (LSI)** operations.
  * Shifting an image by $(\Delta x, \Delta y)$ shifts the gradient fields and the resulting response map $R(x, y)$ by the exact same offset:
  
  $$I_{\text{trans}}(x, y) = I(x - x_0, \, y - y_0) \implies R_{\text{trans}}(x, y) = R(x - x_0, \, y - y_0)$$
  
  Local maxima move with the image, preserving corner locations.
  
  ---
### B. Rotation Invariance: $\checkmark$ Fully Invariant
  Consider rotating the input image by an angle $\theta$:
  
  $$I_{\text{rot}}(\mathbf{x}) = I(\mathbf{R}_\theta \mathbf{x}), \quad \text{where } \mathbf{R}_\theta = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$$
  
  The spatial gradient vector rotates by $\mathbf{R}_\theta$:
  
  $$\nabla I_{\text{rot}} = \mathbf{R}_\theta \nabla I$$
  
  The new Structure Tensor $\mathbf{H}_{\text{rot}}$ is:
  
  $$\mathbf{H}_{\text{rot}} = \sum w \cdot \big(\mathbf{R}_\theta \nabla I\big)\big(\mathbf{R}_\theta \nabla I\big)^T = \mathbf{R}_\theta \left( \sum w \cdot \nabla I \nabla I^T \right) \mathbf{R}_\theta^T = \mathbf{R}_\theta \mathbf{H} \mathbf{R}_\theta^T$$
  
  Recall that the determinant and trace of a matrix are **similarity invariants**:
  
  $$\det(\mathbf{H}_{\text{rot}}) = \det(\mathbf{R}_\theta \mathbf{H} \mathbf{R}_\theta^T) = \det(\mathbf{R}_\theta) \det(\mathbf{H}) \det(\mathbf{R}_\theta^T) = (1) \cdot \det(\mathbf{H}) \cdot (1) = \det(\mathbf{H})$$
  
  $$\text{tr}(\mathbf{H}_{\text{rot}}) = \text{tr}(\mathbf{R}_\theta \mathbf{H} \mathbf{R}_\theta^T) = \text{tr}(\mathbf{R}_\theta^T \mathbf{R}_\theta \mathbf{H}) = \text{tr}(\mathbf{I} \mathbf{H}) = \text{tr}(\mathbf{H})$$
  
  Because both $\det(\mathbf{H})$ and $\text{tr}(\mathbf{H})$ are invariant under orthogonal rotations:
  
  $$R_{\text{rot}} = \det(\mathbf{H}_{\text{rot}}) - k \cdot \text{tr}(\mathbf{H}_{\text{rot}})^2 \equiv R$$
  
  > **The Rotation Invariance Condition:** 
  > The Harris response map is **strictly rotation invariant** *if and only if* the integration window $w(x, y)$ is an **isotropic Gaussian**. If a square box filter is used, rotation invariance is broken.
  
  ---
### C. Illumination Invariance: Partial Invariance
  Consider an affine radiometric illumination transformation: $I_{\text{new}}(x, y) = a \cdot I(x, y) + b$.
  1. **Additive Offset Bias ($b$):**
   * Gradients measure differences: $\nabla (I + b) = \nabla I$.
   * **Fully invariant** to additive global illumination shifts (e.g., turning on a uniform room light).
  2. **Multiplicative Gain Scale ($a$):**
   * Gradients scale linearly: $\nabla (a I) = a \nabla I$.
   * The Structure Tensor scales quadratically: $\mathbf{H}_{\text{new}} = a^2 \mathbf{H}$.
   * The response scalar scales quartically:
  
  $$R_{\text{new}} = \det(a^2 \mathbf{H}) - k \cdot \text{tr}(a^2 \mathbf{H})^2 = a^4 \det(\mathbf{H}) - k a^4 \text{tr}(\mathbf{H})^2 = a^4 R$$
  
   * While the relative positions of local maxima remain unchanged, a static fixed threshold $\tau$ will fail if $a$ drops significantly. The threshold must be scaled dynamically with image variance.
  
  ---
### D. Scale Invariance: $\times$ The Fundamental Vulnerability (Slide 61)
  Slide 61 concludes with an essential question:
  > **"Next Time: Considering feature scale. What if we zoom in or zoom out? Can we match features if they have changed size?"**
  
  ```
       The Scale Breakdown of the Harris Detector
       
       Image at Normal Scale:                           Zoomed In (Large Scale):
            ┌───────────────────┐                            ┌───────────────────┐
            │         │         │                            │        )          │
            │         │ Corner  │                            │       /           │
            │    ─────┘ Peak    │                            │      /   Smooth   │
            │         Detected! │                            │     /     Edge    │
            │                   │                            │    (      Contour │
            └───────────────────┘                            └───────────────────┘
            Window W covers both intersecting edges.         Window W sees only a smooth,
            Both eigenvalues are large: CORNER.              locally straight arc: EDGE!
  ```
  
  * **Why the Harris Detector Fails Across Scales:**
  The Harris detector relies on a fixed Gaussian integration scale $\sigma_i$.
  * Up close (zoomed in), a sharp 90-degree corner has rounded geometry at the pixel level. Within a fixed small window $W$, the boundary appears as a smooth, gently curving contour. 
  * The detector observes gradients in only one dominant direction at a time, classifying the vertex as an **edge** rather than a corner.
  * **Conclusion:** The classical Harris detector is **not scale invariant**. Resolving this limitation requires computing features across a continuous scale-space, leading directly to the **SIFT (Scale-Invariant Feature Transform)** algorithm.
  
  ---
## 6. Complete Python Implementation: Production-Grade Harris Detector
  
  ```python
  import numpy as np
  import scipy.ndimage as ndimage
  
  def harris_corner_detector(image: np.ndarray, 
                           sigma_d: float = 1.0, 
                           sigma_i: float = 2.0, 
                           k: float = 0.05, 
                           rel_threshold: float = 0.01) -> list:
    """
    End-to-end implementation of the Harris & Stephens (1988) Corner Detector.
    
    Parameters:
        image: 2D grayscale float array [0.0, 1.0].
        sigma_d: Differentiation scale (pre-smoothing Gaussian standard deviation).
        sigma_i: Integration scale (neighborhood accumulation Gaussian standard deviation).
        k: Harris empirical sensitivity constant (typically 0.04 - 0.06).
        rel_threshold: Relative threshold factor multiplied by max(R).
        
    Returns:
        List of (row, col) coordinates of detected corner features.
    """
    # -------------------------------------------------------------
    # Step 1: Pre-smooth and compute spatial image gradients
    # -------------------------------------------------------------
    # Applying Gaussian pre-filter at differentiation scale sigma_d
    smoothed_img = ndimage.gaussian_filter(image, sigma=sigma_d, mode='reflect')
    
    # Compute horizontal and vertical derivatives using central differences (Sobel)
    # Note: ndimage.sobel computes axis=0 (rows/y) and axis=1 (cols/x)
    Iy = ndimage.sobel(smoothed_img, axis=0, mode='reflect') / 8.0
    Ix = ndimage.sobel(smoothed_img, axis=1, mode='reflect') / 8.0
    
    # -------------------------------------------------------------
    # Step 2: Form element-wise quadratic products
    # -------------------------------------------------------------
    Ixx = Ix * Ix
    Iyy = Iy * Iy
    Ixy = Ix * Iy
    
    # -------------------------------------------------------------
    # Step 3: Gaussian integration across window W (Scale sigma_i)
    # -------------------------------------------------------------
    A = ndimage.gaussian_filter(Ixx, sigma=sigma_i, mode='reflect')
    C = ndimage.gaussian_filter(Iyy, sigma=sigma_i, mode='reflect')
    B = ndimage.gaussian_filter(Ixy, sigma=sigma_i, mode='reflect')
    
    # -------------------------------------------------------------
    # Step 4: Compute the Harris corner response map R
    # -------------------------------------------------------------
    # det(H) = AC - B^2,  tr(H) = A + C
    det_H = (A * C) - (B ** 2)
    trace_H = A + C
    R = det_H - k * (trace_H ** 2)
    
    # -------------------------------------------------------------
    # Step 5: Thresholding and Non-Maximum Suppression (NMS)
    # -------------------------------------------------------------
    # Absolute threshold
    tau = rel_threshold * np.max(R)
    thresholded_mask = R > tau
    
    # Non-Maximum Suppression: A pixel must equal the local 3x3 maximum
    # ndimage.maximum_filter finds the maximum in a 3x3 sliding neighborhood
    local_maxima = ndimage.maximum_filter(R, size=(3, 3), mode='constant', cval=0.0)
    
    # A true corner must exceed the threshold AND be the strictly dominant local peak
    corner_mask = (R == local_maxima) & thresholded_mask
    
    # Extract row and column coordinates
    corner_coords = np.argwhere(corner_mask)
    
    return corner_coords
  ```
  
  ---
## Complete Module Summary: M3.1 Feature & Corner Detection
  
  ```
  ┌───────────────────────────┬────────────────────────────────────────────────────────────────────────┐
  │ Stage                     │ Core Mathematical Equation / Operation                                 │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Shift Error**           │ $E(u, v) = \sum w(x, y) [I(x+u, y+v) - I(x, y)]^2$                     │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Taylor Linearization**  │ $I(x+u, y+v) \approx I(x, y) + I_x u + I_y v$                          │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Structure Tensor**      │ $\mathbf{H} = \sum w(x, y) \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}$ │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Spectral Geometry**     │ Iso-error ellipse axes scale as $\lambda_{\max}^{-1/2}$ and $\lambda_{\min}^{-1/2}$ │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Harris Response**       │ $R = \det(\mathbf{H}) - k \cdot \text{tr}(\mathbf{H})^2$               │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Noble Response**        │ $f = \frac{\det(\mathbf{H})}{\text{tr}(\mathbf{H}) + \epsilon}$ (Harmonic mean of eigenvalues) │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Shi-Tomasi Response**   │ $R = \min(\lambda_1, \lambda_2)$                                       │
  ├───────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ **Invariance Limits**     │ Invariant to translation, rotation, and affine lighting; **FAILS on scale**│
  └───────────────────────────┴────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
  
  ---