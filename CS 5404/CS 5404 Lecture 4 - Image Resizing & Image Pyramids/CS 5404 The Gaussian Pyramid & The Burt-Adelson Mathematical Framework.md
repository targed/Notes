## 1. The Multi-Scale Paradigm (Slides 25–27, 31)
  
  In **Part 1**, we established that differential visual operators (gradients, laplacians, curvatures) are tied to a fixed pixel scale. When an object recedes into the distance, its projected pixel footprint shrinks, causing single-scale feature detectors to miss matches.
  
  Rather than searching for scale-invariant operators over a single static image, we compute a **multi-scale representation**—a structured stack of images resampled at geometrically decreasing resolutions.
  
  ```
     Image Matching Across Scale:
     
     Image A (Target Object Up-Close)            Image B (Target Object Far Away)
     ┌───────────────────────────┐               ┌───────────────────────────┐
     │         ╭───────╮         │               │                           │
     │         │ VASE  │         │               │          ╭───╮            │
     │         │ (200px│         │               │          │VASE (50px)     │
     │         ╰───────╯         │               │          ╰───╯            │
     └───────────────────────────┘               └───────────────────────────┘
                   │                                           │
                   ▼                                           ▼
     Requires a 200px detector                   Requires a 50px detector
     
  Solution: Build a Pyramid!
  Downsample Image A by 4× ──► Target vase becomes 50px ──► Matched using identical 50px template!
  ```
  
  ---
## 2. Structural Architecture of the Gaussian Pyramid (Slides 28–30)
  
  Introduced in the seminal work by **Peter J. Burt and Edward H. Adelson (1983)** (*"The Laplacian Pyramid as a Compact Image Code"*), the **Gaussian Pyramid** is a sequence of low-pass filtered, subsampled images:
  
  $$\{g_0, g_1, g_2, \dots, g_{K-1}\}$$
  
  ```
                                  Level 3: g₃  (1/8 Res)
                                     ┌───┐
                               Level 2: g₂  (1/4 Res)
                                  ┌───────┐
                            Level 1: g₁  (1/2 Res)
                               ┌─────────────┐
                         Level 0: g₀  (Full Resolution: Original Image)
                            ┌───────────────────┐
                            │                   │
                            │                   │
                            └───────────────────┘
  ```
  
  ---
### A. The Sizing Progression: Why $(2^N + 1) \times (2^N + 1)$? (Slide 30)
  Slide 30 specifies that the input image $g_0$ has dimensions:
  
  $$\text{Dimensions of } g_0 = (2^N + 1) \times (2^N + 1)$$
  
  $$\text{Dimensions of } g_l = (2^{N-l} + 1) \times (2^{N-l} + 1)$$
  
  * For $N = 8$:
  * Level $0$ ($g_0$): $(2^8 + 1) \times (2^8 + 1) = \mathbf{257 \times 257}$
  * Level $1$ ($g_1$): $(2^7 + 1) \times (2^7 + 1) = \mathbf{129 \times 129}$
  * Level $2$ ($g_2$): $(2^6 + 1) \times (2^6 + 1) = \mathbf{65 \times 65}$
  * Level $3$ ($g_3$): $(2^5 + 1) \times (2^5 + 1) = \mathbf{33 \times 33}$
  * Level $4$ ($g_4$): $(2^4 + 1) \times (2^4 + 1) = \mathbf{17 \times 17}$
#### Why the $+1$ Padding Matters:
  In discrete signal decimation, downsampling an arbitrary even dimension (e.g., $256$) by $2$ requires choosing whether boundary pixels align to grid centers or grid edges. 
  * Sizing the grid as $M = 2^N + 1$ guarantees that the boundary samples of level $l+1$ fall **exactly on top of existing grid points** of level $l$:
  
  $$\frac{(2^N + 1) - 1}{2} + 1 = 2^{N-1} + 1$$
  
  This ensures consistent coordinate centers across all pyramid levels, simplifying pyramid reconstruction and multi-grid numerical methods.
  
  ---
### B. Total Memory Footprint: The Geometric Series
  A common concern with multi-resolution representations is memory overhead. 
  Because spatial resolution drops by a factor of $2$ along both horizontal and vertical axes, the total number of pixels at level $l$ decreases by a factor of $4$:
  
  $$\text{Area}(g_l) = \left(\frac{1}{4}\right)^l \cdot \text{Area}(g_0)$$
  
  Summing the infinite geometric series across all pyramid levels:
  
  $$\text{Total Memory} = \sum_{l=0}^\infty \left(\frac{1}{4}\right)^l \cdot \text{Area}(g_0) = \frac{1}{1 - \frac{1}{4}} \cdot \text{Area}(g_0) = \frac{4}{3} \cdot \text{Area}(g_0) \approx \mathbf{1.333 \times \text{Area}(g_0)}$$
  
  > **Key Result:** Storing an entire infinite Gaussian Pyramid requires only **$33.3\%$ more memory** than storing the original full-resolution image alone.
  
  ---
## 3. The Burt & Adelson Kernel Design Framework (Slides 32–40)
  
  To generate level $g_{l+1}$ from $g_l$, we must apply a low-pass filter before decimating by $2$. 
  Slide 32 asks:
  > **"Does the smoothing kernel even need to be Gaussian?"**
  > 
  > *Answer:* Not necessarily. Burt and Adelson derived an optimal 1D, 5-tap generating kernel based on three physical and mathematical constraints.
  
  ---
### The Derivation of the 5-Tap Generating Kernel (Slides 33–39)
  Let the 1D, 5-element smoothing filter be parameterized as:
  
  $$\mathbf{w} = \begin{bmatrix} w_{-2} & w_{-1} & w_0 & w_1 & w_2 \end{bmatrix}$$
  
  ```
                          w₀ (Center Weight)
                               ^
                               │
                       w₋₁     │      w₁
                        ^      │      ^
                        │      │      │
                w₋₂     │      │      │     w₂
                 ^      │      │      │      ^
                 │      │      │      │      │
              ───┴──────┴──────┴──────┴──────┴───► Spatial Index
                -2     -1      0     +1     +2
  ```
  
  ---
### Constraint 1: Spatial Symmetry (Slide 34–35)
  To prevent phase distortion and directional bias:
  
  $$w_{-i} = w_i \quad \text{for } i \in \{1, 2\}$$
  
  Letting $w_0 = a$, $w_1 = w_{-1} = b$, and $w_2 = w_{-2} = c$:
  
  $$\mathbf{w} = \begin{bmatrix} c & b & a & b & c \end{bmatrix}$$
  
  This reduces the parameter space from $5$ independent variables to $3$ ($a, b, c$).
  
  ---
### Constraint 2: Normalization / Energy Conservation (Slides 36–37)
  To ensure that uniform, constant image regions ($f(x) = C$) remain unchanged (DC gain $= 1$):
  
  $$\sum_{i=-2}^2 w_i = 1.0 \implies a + 2b + 2c = 1.0$$
  
  ---
### Constraint 3: The Equal Contribution Criterion (Slides 38–39)
  This constraint ensures balanced signal downsampling:
  
  > **The Equal Contribution Criterion:**
  > When downsampling by a factor of 2, every pixel in the higher-resolution input image must contribute with **equal total weight** to the next coarser level.
  
  ```
   High-Res Grid g_l:    x₀    x₁    x₂    x₃    x₄    x₅    x₆    x₇    x₈
                         │     │     │     │     │     │     │     │     │
                         ▼           ▼           ▼           ▼           ▼
   Coarse Grid g_{l+1}:  y₀          y₁          y₂          y₃          y₄
                         (Even)      (Even)      (Even)      (Even)      (Even)
  ```
  
  Because downsampling drops every other sample, coarse grid points $y_i$ sample only from **even indices** of the fine grid ($2i$). 
  
  Consider how even and odd input pixels contribute to the coarse outputs:
  
  1. **Even Input Pixels ($x_0, x_2, x_4, \dots$):**
   * When an output sample lands directly above an even pixel, it weights it by $w_0 = a$.
   * When adjacent output samples ($y_{i-1}$ or $y_{i+1}$) sample across it, they weight it by $w_2 = c$ or $w_{-2} = c$.
   * Total contribution weight of an even pixel to all coarse outputs:
  
  $$\text{Weight}_{\text{even}} = a + 2c$$
  
  2. **Odd Input Pixels ($x_1, x_3, x_5, \dots$):**
   * Output samples never align directly above an odd pixel.
   * Adjacent coarse outputs sample across it using only the odd taps $w_{-1} = b$ and $w_1 = b$.
   * Total contribution weight of an odd pixel to all coarse outputs:
  
  $$\text{Weight}_{\text{odd}} = 2b$$
  
  For both classes of pixels to contribute equally:
  
  $$\text{Weight}_{\text{even}} = \text{Weight}_{\text{odd}} \implies a + 2c = 2b$$
  
  ---
### Solving the Algebraic System (Slides 39–40)
  We now have two linear equations with three unknowns ($a, b, c$):
  
  $$\begin{cases} a + 2b + 2c = 1.0 & \text{(Normalization)} \\[6pt] a + 2c = 2b & \text{(Equal Contribution)} \end{cases}$$
  
  Substitute the second equation into the first:
  
  $$(a + 2c) + 2b = 1.0 \implies 2b + 2b = 1.0 \implies 4b = 1.0$$
  
  $$b = \frac{1}{4} = \mathbf{0.25}$$
  
  Now solve for $c$ in terms of the free parameter $a$:
  
  $$a + 2c = 2b = 2(0.25) = 0.5 \implies 2c = 0.5 - a$$
  
  $$c = \frac{1}{4} - \frac{a}{2} = \mathbf{0.25 - 0.5a}$$
  
  ---
### The Unified Parametric Burt-Adelson Kernel (Slide 40)
  Every valid 5-tap generating kernel that satisfies symmetry, energy conservation, and equal contribution can be expressed as a function of the single free parameter $a$:
  
  $$\mathbf{w}(a) = \begin{bmatrix} 
  \frac{1}{4} - \frac{a}{2} \\[4pt] 
  \frac{1}{4} \\[4pt] 
  a \\[4pt] 
  \frac{1}{4} \\[4pt] 
  \frac{1}{4} - \frac{a}{2} 
  \end{bmatrix}$$
  
  ---
## 4. Analyzing the Free Parameter $a$ (Slide 40)
  
  Slide 40 notes:
  > *"a can be any value, though it's typically between 0.3 and 0.6."*
  
  Different values of $a$ yield different classical filter profiles:
  
  ```
    a = 0.4 (Gaussian-like)             a = 0.5 (Triangular)            a = 0.375 (B-Spline)
           ^                                     ^                                ^
       0.4 │     ╭─╮                         0.5 │     /\                     0.375│     ╭─╮
           │   ╭─╯ ╰─╮                           │    /  \                         │   ╭─╯ ╰─╮
      0.25 │ ╭─╯     ╰─╮                    0.25 │  ╭╯    ╰╮                  0.25 │ ╭─╯     ╰─╮
      0.05 ├─╯         ╰─                   0.00 ┴──┴──────┴──                0.06 ├─╯         ╰─
           └──────────────►                      └────────────►                    └──────────────►
       [0.05, 0.25, 0.4, ...]               [0, 0.25, 0.5, ...]              [1/16, 4/16, 6/16, ...]
  ```
  
  ---
### A. Case 1: $a = 0.4$ (The Standard Gaussian Approximation)
  * Center weight: $a = 0.4$
  * Neighbor weight: $b = 0.25$
  * Outer weight: $c = 0.25 - 0.5(0.4) = 0.05$
  
  $$\mathbf{w}_{0.4} = \begin{bmatrix} 0.05 & 0.25 & 0.40 & 0.25 & 0.05 \end{bmatrix}$$
  
  * **Why it's preferred:** This profile closely approximates a continuous Gaussian with standard deviation $\sigma \approx 1.0$. It minimizes high-frequency ripple while providing smooth spatial decay.
  
  ---
### B. Case 2: $a = 0.5$ (The Triangular / 3-Tap Collapse)
  * Center weight: $a = 0.5$
  * Neighbor weight: $b = 0.25$
  * Outer weight: $c = 0.25 - 0.5(0.5) = 0.0$
  
  $$\mathbf{w}_{0.5} = \begin{bmatrix} 0.0 & 0.25 & 0.50 & 0.25 & 0.0 \end{bmatrix} = \frac{1}{4}\begin{bmatrix} 1 & 2 & 1 \end{bmatrix}$$
  
  * **Characteristics:** The outer taps drop to zero, reducing the filter to a compact 3-tap binomial kernel. It is computationally lightweight, though it provides slightly less anti-aliasing attenuation near the Nyquist boundary.
  
  ---
### C. Case 3: $a = 0.375 = \frac{3}{8}$ (The Cubic B-Spline)
  * Center weight: $a = 6/16$
  * Neighbor weight: $b = 4/16$
  * Outer weight: $c = 1/16$
  
  $$\mathbf{w}_{3/8} = \frac{1}{16} \begin{bmatrix} 1 & 4 & 6 & 4 & 1 \end{bmatrix}$$
  
  * **Characteristics:** Corresponds to the binomial expansion $\left(\frac{1}{2} + \frac{1}{2}\right)^4$. It matches the continuous cubic B-spline sampling basis, maximizing smoothness across reconstructed boundaries.
  
  ---
### D. Why the Box Filter Fails the Equal Contribution Criterion (Slide 40)
  For a 5-tap uniform box filter:
  
  $$\mathbf{w}_{\text{box}} = \begin{bmatrix} 0.2 & 0.2 & 0.2 & 0.2 & 0.2 \end{bmatrix}$$
  
  * Here, $a = 0.2$, $b = 0.2$, and $c = 0.2$.
  * Sum of even weights: $a + 2c = 0.2 + 2(0.2) = \mathbf{0.6}$
  * Sum of odd weights: $2b = 2(0.2) = \mathbf{0.4}$
  
  $$\text{Weight}_{\text{even}} \ (0.6) \neq \text{Weight}_{\text{odd}} \ (0.4)$$
  
  Because even and odd pixels contribute unequally ($60\%$ vs. $40\%$), downsampling with a box filter introduces a **periodic grid-modulation artifact** that produces synthetic striping across the image.
  
  ---
## 5. The 2D Separable `REDUCE` Operator (Slide 42)
  
  Generating level $g_{l+1}$ from $g_l$ is formalized by the **$\text{REDUCE}$ operator**:
  
  $$g_{l+1} = \text{REDUCE}(g_l)$$
  
  ```
     Input Grid g_l                         Separable Low-Pass Filter            Subsampled Output Grid g_{l+1}
   ┌───┬───┬───┬───┬───┐                     w(m, n) = w(m) · w(n)                     ┌───────┬───────┐
   │   │   │   │   │   │                                                               │       │       │
   ├───┼───┼───┼───┼───┤                                                               ├───────┼───────┤
   │   │   │   │   │   │    ──►  Convolve with 5×5 ──► Stride by 2 ──►                 │       │       │
   ├───┼───┼───┼───┼───┤                                                               └───────┴───────┘
   │   │   │   │   │   │                                                            Half Resolution
   └───┴───┴───┴───┴───┘
  ```
  
  ---
### A. Mathematical Definition
  The 2D filter kernel $\mathbf{W} \in \mathbb{R}^{5 \times 5}$ is the separable outer product of the 1D generating kernel:
  
  $$\mathbf{W} = \mathbf{w} \cdot \mathbf{w}^T \iff W[m, n] = w[m] \cdot w[n]$$
  
  The intensity at coordinate $(i, j)$ of the reduced image $g_{l+1}$ is computed via **stride-2 cross-correlation**:
  
  $$g_{l+1}[i, j] = \sum_{m=-2}^2 \sum_{n=-2}^2 w[m] \cdot w[n] \cdot g_l[2i + m, \, 2j + n]$$
  
  * The terms $2i$ and $2j$ implement **decimation by 2** directly inside the convolution summation, avoiding the need to allocate intermediate full-resolution arrays.
  
  ---
### B. Python / NumPy Implementation
  ```python
  import numpy as np
  import scipy.signal as signal
  
  def generate_1d_kernel(a: float = 0.4) -> np.ndarray:
    """Generates the 5-tap Burt-Adelson generating kernel."""
    b = 0.25
    c = 0.25 - 0.5 * a
    return np.array([c, b, a, b, c], dtype=np.float32)
  
  def reduce_level(image: np.ndarray, a: float = 0.4) -> np.ndarray:
    """
    Applies the Burt-Adelson REDUCE operator:
    1. Low-pass filters with separable 5x5 kernel W = w * w^T
    2. Subsamples by a factor of 2 along both axes
    """
    k1d = generate_1d_kernel(a)
    
    # Exploit separability: filter horizontally, then vertically
    # Boundary handling: 'reflect' prevents artificial dark edge fringes
    filtered_h = signal.convolve2d(image, k1d[np.newaxis, :], mode='same', boundary='symm')
    filtered_hv = signal.convolve2d(filtered_h, k1d[:, np.newaxis], mode='same', boundary='symm')
    
    # Decimate by slicing with stride 2
    return filtered_hv[::2, ::2]
  
  def build_gaussian_pyramid(image: np.ndarray, levels: int, a: float = 0.4) -> list:
    """Builds a full Gaussian Pyramid stack {g_0, g_1, ..., g_{levels-1}}."""
    pyramid = [image.astype(np.float32)]
    for l in range(1, levels):
        pyramid.append(reduce_level(pyramid[-1], a=a))
    return pyramid
  ```
  
  ---
## 6. Applications of the Gaussian Pyramid (Slide 31)
  
  Why build a Gaussian Pyramid?
  
  ```
                         [ Gaussian Pyramid Uses ]
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
  Coarse-to-Fine Search       Scale-Space Features       Accelerated Matching
  • Optical Flow (Lucas-Kanade)• SIFT (Scale-Invariant     • Fast normalized cross-
  • Stereo Disparity Matching   Feature Transform)           correlation
  • Large-displacement tracking• Blobs & Multi-scale       • Evaluates top levels
                                Corners                     first to prune candidates
  ```
  
  1. **Coarse-to-Fine Motion Estimation (Optical Flow):**
   * High-resolution optical flow methods fail when pixel displacement exceeds $1\text{–}2\text{ pixels}$.
   * At level $4$ of a pyramid, a large motion of $32\text{ pixels}$ shrinks to a manageable displacement of:
  
  $$\frac{32}{2^4} = 2\text{ pixels}$$
  
   * The algorithm solves for coarse displacement at the top of the pyramid and projects the estimate downward as an initialization for finer levels.
  
  2. **Multi-Scale Feature Detection (SIFT):**
   * Keypoints (corners, blobs) are identified across all pyramid levels, making feature matching robust to camera zoom and distance changes.
  
  3. **Accelerated Object Recognition & Template Matching:**
   * Evaluating a sliding-window detector across a $4\text{K}$ image is computationally expensive.
   * Testing candidates on coarser pyramid levels first ($g_3$ or $g_2$) allows unlikely candidate regions to be pruned before running full evaluation on finer levels.
  
  ---
## Summary Matrix: The Burt-Adelson Framework
  
  | Property | Mathematical Form | Operational Role |
  | :--- | :--- | :--- |
  | **Generating Kernel** | $\mathbf{w} = [c, b, a, b, c]^T$ | 1D separable building block |
  | **Symmetry Constraint** | $w_{-i} = w_i$ | Zero phase shift; isotropic filtering |
  | **Energy Conservation** | $a + 2b + 2c = 1.0$ | Unity DC gain ($H(0) = 1$); constant intensity preservation |
  | **Equal Contribution** | $a + 2c = 2b = 0.5$ | Prevents grid-modulation / spatial striping artifacts |
  | **Free Parameter $a$** | Typically $0.4$ (Gaussian) or $0.375$ (B-Spline) | Tunes spatial bandwidth vs. side-lobe attenuation |
  | **`REDUCE` Complexity** | $\mathcal{O}(2 \times 5 \times M^2)$ | Separable convolution + decimation |
  
  ---