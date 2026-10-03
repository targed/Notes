## 1. Geometric Slicing & The Uncertainty Ellipse (Slides 35, 38–39)
  
  In **Part 2**, we established that the local auto-correlation error $E(u, v)$ for a spatial displacement $\mathbf{u} = \begin{bmatrix} u & v \end{bmatrix}^T$ is modeled by the quadratic form:
  
  $$E(u, v) \approx \mathbf{u}^T \mathbf{H} \, \mathbf{u} = A u^2 + 2 B u v + C v^2$$
  
  where $\mathbf{H}$ is the symmetric $2 \times 2$ Structure Tensor:
  
  $$\mathbf{H} = \begin{bmatrix} A & B \\ B & C \end{bmatrix} = \sum_{(x, y) \in W} \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}$$
  
  Slide 35 introduces an intuitive geometric perspective: 
  > **"What happens if we take a horizontal cross-sectional slice of the 3D error bowl at a constant height $E(u, v) = \text{const}$?"**
  
  ```
   3D Paraboloid Error Surface E(u, v)                Horizontal Cross-Section at E(u, v) = 1
             ^ z (Error)
             │        )                                           ^ v
             │       (   )                                        │      ╭──────────────╮
             │  ───────X─────── Slice: E = 1                      │    ╭─╯  Major Axis  ╰─╮
             │      (     )                                       │   ╭╯ (Slowest Change) ╰╮
       ──────┴─────(_______)─────► u                              └───┼────────────────────┼───► u
            /                                                         ╰╮  Minor Axis      ╭╯
           v                                                           ╰─╮(Fastest Change)╭╯
                                                                         ╰──────────────╯
                                                                         Iso-Error Ellipse
  ```
  
  ---
### A. The Iso-Error Contour Equation (Slide 35)
  Setting the error surface to a constant threshold (e.g., $E(u, v) = 1$) yields an implicit 2D quadratic curve:
  
  $$\begin{bmatrix} u & v \end{bmatrix} \mathbf{H} \begin{bmatrix} u \\ v \end{bmatrix} = A u^2 + 2 B u v + C v^2 = 1$$
  
  Because $\mathbf{H}$ is a real, symmetric, positive semi-definite matrix, this equation defines a centered **ellipse** in the displacement plane $(u, v)$.
  
  ---
### B. Spectral Decomposition & The Geometry of the Ellipse (Slides 35–37)
  By the **Spectral Theorem** of linear algebra, any real symmetric matrix $\mathbf{H}$ can be factorized via orthogonal diagonalization:
  
  $$\mathbf{H} = \mathbf{V} \boldsymbol{\Lambda} \mathbf{V}^T = \begin{bmatrix} \mathbf{v}_1 & \mathbf{v}_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} \begin{bmatrix} \mathbf{v}_1^T \\ \mathbf{v}_2^T \end{bmatrix}$$
  
  where:
  * $\lambda_1, \lambda_2$ are the real, non-negative eigenvalues ($\lambda_{\max} \ge \lambda_{\min} \ge 0$).
  * $\mathbf{v}_1, \mathbf{v}_2 \in \mathbb{R}^2$ are the mutually perpendicular, orthonormal eigenvectors ($\mathbf{v}_1 \perp \mathbf{v}_2$).
  
  ```
                            Geometry of the Error Ellipse (Slide 35, 38)
                            
                                                Direction of Fastest Change
                                                    (Eigenvector v_max)
                                                             ^
                                                             │
                                                          ╭──┼──╮
                                                      ╭───╯  │  ╰───╮
                                  ───────────────────╭╯──────┼──────╰╮───────────────────► Direction of Slowest Change
                                                     │◄──────┼─────►││                     (Eigenvector v_min)
                                                     ╰───╮   │  ╭───╯│
                                                         ╰───┼──╯    │
                                                             │       │
                                                             │◄─────►│
                                                       Radius r_min = 1 / √(λ_max)
                                                       
                                                     │◄──────────────►│
                                                       Radius r_max = 1 / √(λ_min)
  ```
  
  ---
### C. Why Axis Lengths Scale as $\lambda^{-1/2}$ (The Inverse Relationship)
  In coordinate axes aligned with the eigenvectors $(\tilde{u}, \tilde{v})$, the quadratic equation decouples into canonical form:
  
  $$\lambda_1 \tilde{u}^2 + \lambda_2 \tilde{v}^2 = 1 \iff \frac{\tilde{u}^2}{\left(\frac{1}{\sqrt{\lambda_1}}\right)^2} + \frac{\tilde{v}^2}{\left(\frac{1}{\sqrt{\lambda_2}}\right)^2} = 1$$
  
  From standard conic geometry, an ellipse $\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1$ has semi-axis lengths $a$ and $b$:
  
  $$r_1 = \frac{1}{\sqrt{\lambda_1}} = \frac{1}{\sqrt{\lambda_{\max}}}, \quad r_2 = \frac{1}{\sqrt{\lambda_2}} = \frac{1}{\sqrt{\lambda_{\min}}}$$
#### Physical Intuition:
  * **Along $\mathbf{v}_{\max}$ (Fastest Change):** 
  Because $\lambda_{\max}$ is large, the error surface $E$ climbs steeply. Only a tiny physical displacement $\Delta u = \frac{1}{\sqrt{\lambda_{\max}}}$ is needed to reach an error of $1.0$. **High curvature in error space corresponds to a short axis in the ellipse.**
  * **Along $\mathbf{v}_{\min}$ (Slowest Change):** 
  Because $\lambda_{\min}$ is small, the error climbs very slowly. A large physical displacement $\Delta v = \frac{1}{\sqrt{\lambda_{\min}}}$ is required before the error reaches $1.0$. **Low curvature in error space corresponds to a long axis in the ellipse.**
  
  ---
## 2. The Eigenvalue Classification Space (Slides 40–44)
  
  The eigenvalues $(\lambda_1, \lambda_2)$ of the Structure Tensor $\mathbf{H}$ provide a compact, coordinate-invariant summary of local image structure.
  
  Slide 43 charts the fundamental **Eigenvalue Phase Plane**:
  
  ```
                       The (λ₁, λ₂) Classification Plane (Slide 43)
                       
              λ₂ ^
                 │
                 │   EDGE REGION
                 │   λ₂ >> λ₁
                 │   (Gradients strictly
                 │    along vertical axis)
                 │                                        CORNER REGION
                 │                                        λ₁ >> 0  AND  λ₂ >> 0
                 │                                        λ₁ ≈ λ₂
                 │                                        (Significant changes in ALL directions;
                 │                                         error bowl climbs steeply everywhere!)
                 │
                 │   FLAT REGION
                 │   λ₁ ≈ 0, λ₂ ≈ 0
                 │   (No contrast in patch)               EDGE REGION
                 │                                        λ₁ >> λ₂
                 │                                        (Gradients strictly along horizontal axis)
                 └────────────────────────────────────────────────────────► λ₁
                 0
  ```
  
  ---
### A. The Three Canonical Visual Regimes
  1. **Flat Regions:**
   * Both eigenvalues are near zero: $\lambda_1 \approx 0, \, \lambda_2 \approx 0$.
   * The ellipse is infinitely large in all directions.
   * Shifting the window anywhere incurs negligible error.
  2. **Straight Edge Regions (The Aperture Problem):**
   * One eigenvalue is large, while the other is approximately zero: 
  
  $$\lambda_1 \gg \lambda_2 \approx 0 \quad \text{or} \quad \lambda_2 \gg \lambda_1 \approx 0$$
  
   * The ellipse is stretched into an elongated, needle-like shape.
   * Shifting orthogonal to the edge produces large errors, but shifting along the edge produces zero resistance.
  3. **Corner / Vertex Regions:**
   * **Both eigenvalues are distinctly large:** 
  
  $$\lambda_1 \gg 0 \quad \text{and} \quad \lambda_2 \gg 0$$
  
   * The ellipse is compact and roughly circular ($\lambda_1 \approx \lambda_2$).
   * Shifting the window in **any** direction produces immediate, significant error.
  
  ---
### B. The Checkerboard Experiment: $\lambda_{\max}$ vs. $\lambda_{\min}$ (Slides 40–42)
  Slides 40–42 demonstrate a numerical experiment on a high-contrast binary checkerboard:
  
  ```
        Input Checkerboard (Slide 40)           λ_max Response (Slide 41)               λ_min Response (Slide 42)
        ┌──────┬──────┬──────┐                  ┌──────┬──────┬──────┐                  ┌──────┬──────┬──────┐
        │  ██  │      │  ██  │                  │  ██──┼──────┼──██  │                  │      │   •  │      │
        ├──────┼──────┼──────┤                  ├──┼───┼──────┼───┼──┤                  ├──────┼──────┼──────┤
        │      │  ██  │      │  ────────►       │  │   │  ██──┼───┼──│  ────────►       │   •  │      │   •  │
        ├──────┼──────┼──────┤                  ├──┼───┼──────┼───┼──┤                  ├──────┼──────┼──────┤
        │  ██  │      │  ██  │                  │  ██──┼──────┼──██  │                  │      │   •  │      │
        └──────┴──────┴──────┘                  └──────┴──────┴──────┘                  └──────┴──────┴──────┘
               Image I                           Responds to EDGES & CORNERS             Responds ONLY to CORNERS!
  ```
  
  * **The $\lambda_{\max}$ Image (Slide 41):** 
  Shows a dense, connected grid of bright lines covering **both straight edges and corner junctions**. Because an edge has high contrast in at least one direction, its dominant eigenvalue $\lambda_{\max}$ is large everywhere along the grid lines.
  * **The $\lambda_{\min}$ Image (Slide 42):** 
  The grid lines vanish completely, leaving only isolated, bright pinpoint dots precisely at the **checkerboard vertices**. 
  
  > **The Fundamental Corner Principle (Slide 42):**
  > A corner exists if and only if the **minimal eigenvalue $\lambda_{\min}$ is high**. 
  > If the direction of least resistance still produces a large intensity change, the patch cannot be slid in any direction without altering its appearance.
  
  ---
## 3. The Shi-Tomasi Scoring Function (Slide 42)
  
  In 1994, Jianbo Shi and Carlo Tomasi published their influential paper, *"Good Features to Track"* [2], which formalized feature selection for the Kanade-Lucas-Tomasi (KLT) tracking algorithm.
### A. Mathematical Definition
  Shi and Tomasi proposed measuring cornerness directly using the minimum eigenvalue:
  
  $$R_{\text{Shi-Tomasi}} = \min(\lambda_1, \, \lambda_2)$$
  
  A point $(x, y)$ is selected as a corner feature if:
  
  $$\min(\lambda_1, \, \lambda_2) > \tau_{\text{threshold}}$$
  
  ---
### B. Why Shi-Tomasi is Geometrically Optimal for Tracking
  When tracking an image patch between frames $t$ and $t+1$, the optical flow displacement vector $\mathbf{d} = \begin{bmatrix} \Delta x & \Delta y \end{bmatrix}^T$ is computed by solving the linear system:
  
  $$\mathbf{H} \mathbf{d} = \mathbf{e}$$
  
  The numerical conditioning and stability of this inversion depend on the eigenvalues of $\mathbf{H}$. 
  * If $\lambda_{\min} \approx 0$ (an edge), the matrix $\mathbf{H}$ is nearly singular ($\text{cond}(\mathbf{H}) \to \infty$), causing tracking estimates to explode along the aperture direction.
  * By ensuring that $\lambda_{\min} > \tau$, Shi-Tomasi guarantees that $\mathbf{H}$ is well-conditioned and invertibility errors are bounded, maximizing tracker stability.
  
  ---
## 4. The Computational Bottleneck: Why Exact Eigenvalues Were Avoided (Slides 36–37, 45–46)
  
  If $\min(\lambda_1, \lambda_2)$ is mathematically ideal, why didn't Chris Harris and Mike Stephens use it in 1988?
### A. The Cost of Solving the Characteristic Polynomial (Slide 37, 46)
  To compute eigenvalues $\lambda_1, \lambda_2$ of $\mathbf{H} = \begin{bmatrix} A & B \\ B & C \end{bmatrix}$, we must solve the characteristic equation:
  
  $$\det(\mathbf{H} - \lambda \mathbf{I}) = \det\left( \begin{bmatrix} A - \lambda & B \\ B & C - \lambda \end{bmatrix} \right) = 0$$
  
  $$(A - \lambda)(C - \lambda) - B^2 = 0 \implies \lambda^2 - (A + C)\lambda + (AC - B^2) = 0$$
  
  Applying the quadratic formula (Slide 37):
  
  $$\lambda_{1, 2} = \frac{(A + C) \pm \sqrt{(A + C)^2 - 4(AC - B^2)}}{2} = \frac{(A + C) \pm \sqrt{(A - C)^2 + 4B^2}}{2}$$
  
  * **The Problem (Slide 46):** 
  Calculating this explicit formula requires computing a **square root** ($\sqrt{\cdot}$) for every single pixel in an image. In hardware environments of the late 1980s (and even in modern battery-limited real-time embedded systems), per-pixel square root evaluations are computationally expensive.
  
  ---
### B. The Linear Algebra Shortcuts: Determinant and Trace (Slide 45, 46)
  Harris and Stephens recognized that the coefficients of the characteristic polynomial relate directly to two easily computed matrix invariants:
  
  1. **The Determinant ($\det$):** Equal to the **product** of the eigenvalues:
  
  $$\det(\mathbf{H}) = AC - B^2 = \lambda_1 \lambda_2$$
  
  2. **The Trace ($\text{tr}$):** Equal to the **sum** of the eigenvalues:
  
  $$\text{tr}(\mathbf{H}) = A + C = \lambda_1 + \lambda_2$$
  
  Both quantities require only three simple multiplications, two additions, and one subtraction per pixel—**with zero square roots**.
  
  ---
## 5. The Harris & Stephens Corner Operator (Slides 45–46)
  
  ---
### Formulation 1: The Classic Harris-Stephens Measure (1988)
  To score corners without evaluating square roots, Harris and Stephens [1] formulated a combined response function $R$:
  
  $$R = \det(\mathbf{H}) - k \cdot \big(\text{trace}(\mathbf{H})\big)^2$$
  
  In terms of eigenvalues:
  
  $$R = (\lambda_1 \lambda_2) - k \cdot (\lambda_1 + \lambda_2)^2$$
  
  where $k$ is an empirical tunable sensitivity constant, typically set to:
  
  $$k \in [0.04, \, 0.06]$$
  
  ---
### Mathematical Analysis of the Harris Operator Across Regimes:
  
  ```
  ┌───────────────────────────┬───────────────────────────────┬─────────────────────────────────────────┐
  │ Region Type               │ Eigenvalue Relation           │ Harris Response Value R                 │
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **Flat Region**           │ $\lambda_1 \approx 0, \, \lambda_2 \approx 0$  │ $R \approx 0$ (Near zero)               │
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **Edge Region**           │ $\lambda_1 \gg \lambda_2 \approx 0$           │ $R < 0$ (**Large Negative Number**)     │
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **Corner Region**         │ $\lambda_1 \gg 0, \, \lambda_2 \gg 0, \, \lambda_1 \approx \lambda_2$ │ $R > 0$ (**Large Positive Number**)     │
  └───────────────────────────┴───────────────────────────────┴─────────────────────────────────────────┘
  ```
#### Why $R < 0$ Along Edges:
  Let $\lambda_1 = 100$ and $\lambda_2 = 1$ (a typical straight edge profile), with $k = 0.05$:
  
  $$\det(\mathbf{H}) = 100 \times 1 = 100$$
  
  $$\text{tr}(\mathbf{H})^2 = (100 + 1)^2 = 101^2 = 10,201$$
  
  $$R = 100 - 0.05(10,201) = 100 - 510.05 = \mathbf{-410.05} \quad (\text{Heavily Negative})$$
  
  The trace term dominates, driving the response deeply negative along edges.
#### Why $R \gg 0$ at Corners:
  Let $\lambda_1 = 100$ and $\lambda_2 = 100$ (an isotropic corner profile), with $k = 0.05$:
  
  $$\det(\mathbf{H}) = 100 \times 100 = 10,000$$
  
  $$\text{tr}(\mathbf{H})^2 = (100 + 100)^2 = 200^2 = 40,000$$
  
  $$R = 10,000 - 0.05(40,000) = 10,000 - 2,000 = \mathbf{+8,000} \quad (\text{Large Positive Peak})$$
  
  Corners produce prominent positive local maxima that are easily isolated via thresholding.
  
  ---
### Formulation 2: The Noble / Harmonic Mean Operator (Slides 45–46)
  Slide 45 highlights an elegant alternative proposed by Alison Noble (1988) [3]:
  
  $$f_{\text{Noble}} = \frac{\det(\mathbf{H})}{\text{trace}(\mathbf{H}) + \epsilon} = \frac{\lambda_1 \lambda_2}{\lambda_1 + \lambda_2 + \epsilon}$$
  
  (where $\epsilon$ is a machine precision constant to prevent division by zero in flat regions).
#### Why the Noble Operator is Powerful:
  * **No Empirical $k$ Parameter:** Unlike the classic Harris measure, which requires tuning $k$ based on sensor noise and contrast, Noble's formula is **parameter-free**.
  * **Proportional to the Harmonic Mean:** The harmonic mean of two numbers is defined as:
  
  $$\mathcal{H}(\lambda_1, \lambda_2) = \frac{2}{\frac{1}{\lambda_1} + \frac{1}{\lambda_2}} = \frac{2 \lambda_1 \lambda_2}{\lambda_1 + \lambda_2} = 2 \cdot f_{\text{Noble}}$$
  
  * **Behavioral Match to Shi-Tomasi:** The harmonic mean is heavily dominated by its smallest argument:
  
  $$\lim_{\lambda_2 \to 0} \left( \frac{\lambda_1 \lambda_2}{\lambda_1 + \lambda_2} \right) = 0, \quad \frac{\lambda_1 \lambda_2}{\lambda_1 + \lambda_2} \approx \min(\lambda_1, \lambda_2) \quad \text{when } \lambda_1 \gg \lambda_2$$
  
  Noble's operator closely mimics the behavior of Shi-Tomasi's $\min(\lambda_1, \lambda_2)$ while remaining computationally cheap and avoiding square roots.
  
  ---
## 6. Python Implementation: Evaluating Corner Metrics
  
  ```python
  import numpy as np
  
  def compute_corner_responses(A: np.ndarray, B: np.ndarray, C: np.ndarray, k: float = 0.05):
    """
    Computes corner response maps given pre-integrated structure tensor elements:
    A = sum(Ix^2), B = sum(Ix * Iy), C = sum(Iy^2)
    """
    # 1. Compute Matrix Invariants via cheap array arithmetic
    det_H = (A * C) - (B ** 2)
    trace_H = A + C
    
    # 2. Classic Harris & Stephens Operator (1988)
    # R = det(H) - k * (trace(H))^2
    R_harris = det_H - k * (trace_H ** 2)
    
    # 3. Noble / Harmonic Mean Operator (Slide 45)
    # f = det(H) / (trace(H) + epsilon)
    eps = 1e-12
    R_noble = det_H / (trace_H + eps)
    
    # 4. Exact Shi-Tomasi Minimum Eigenvalue Operator (1994)
    # lambda_min = 0.5 * [ (A+C) - sqrt((A-C)^2 + 4B^2) ]
    trace_diff = A - C
    sqrt_discriminant = np.sqrt(trace_diff ** 2 + 4.0 * (B ** 2))
    lambda_min = 0.5 * (trace_H - sqrt_discriminant)
    
    return R_harris, R_noble, lambda_min
  ```
  
  ---
## Summary Comparison: Corner Scoring Operators
  
  | Method | Mathematical Equation | Parameter Required? | Computational Cost | Operational Behavior |
  | :--- | :--- | :--- | :--- | :--- |
  | **Shi-Tomasi** | $R = \min(\lambda_1, \lambda_2)$ | No | Expensive ($\sqrt{\cdot}$ per pixel) | Optimal tracker conditioning; sharp corner peaks |
  | **Harris-Stephens** | $R = \det(\mathbf{H}) - k \cdot \text{tr}(\mathbf{H})^2$ | Yes ($k \approx 0.04\text{--}0.06$) | Very Fast (Only additions & products) | Deep negative response at edges, high positive peaks at corners |
  | **Noble** | $R = \frac{\det(\mathbf{H})}{\text{tr}(\mathbf{H}) + \epsilon}$ | No (Parameter-free) | Fast (Single division per pixel) | Harmonic mean approximation of $\min(\lambda_1, \lambda_2)$ |
  
  ---