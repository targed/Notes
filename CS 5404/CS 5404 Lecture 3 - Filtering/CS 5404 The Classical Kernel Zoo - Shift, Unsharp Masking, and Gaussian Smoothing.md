## 1. The Kernel Zoo: Elementary Filters (Slides 43–47)
  
  By designing specific weight matrices, we can use the same 2D convolution engine to perform diverse operations such as passing, translating, sharpening, and smoothing signals.
  
  ---
### A. The Identity Filter (Slides 43–44)
  The simplest non-trivial linear shift-invariant filter is the **discrete Kronecker delta impulse** $\delta[m, n]$:
  
  $$\mathbf{K}_{\text{id}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 0 \end{bmatrix}$$
  
  ```
   ┌───┬───┬───┐
   │ 0 │ 0 │ 0 │
   ├───┼───┼───┤
   │ 0 │ 1 │ 0 │  ──► (f * K_id)[m, n] = f[m, n]
   ├───┼───┼───┤      Leaves the input completely unaltered.
   │ 0 │ 0 │ 0 │
   └───┴───┴───┘
  ```
#### Mathematical Verification:
  Evaluating 2D discrete convolution with $\mathbf{K}_{\text{id}}$:
  
  $$(f * \mathbf{K}_{\text{id}})[m, n] = \sum_{i=-1}^1 \sum_{j=-1}^1 f[m - i, \, n - j] \cdot K_{\text{id}}[i, j]$$
  
  Since $K_{\text{id}}[i, j] = 0$ everywhere except at $(i, j) = (0, 0)$, the summation collapses to:
  
  $$(f * \mathbf{K}_{\text{id}})[m, n] = f[m - 0, \, n - 0] \cdot 1 = f[m, n]$$
  
  $\mathbf{K}_{\text{id}}$ acts as the **multiplicative identity element** in the convolution algebra: $f * \delta = f$.
  
  ---
### B. The Right-Shift Filter & The Coordinate Flip Paradox (Slides 45–47)
  Slide 45 introduces the following asymmetric kernel:
  
  $$\mathbf{K}_{\text{shift}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 1 \\ 0 & 0 & 0 \end{bmatrix}$$
  
  Notice that the non-zero weight ($1$) is located at index $(i=0, j=+1)$—i.e., **one column to the right** of the center. 
  
  Slide 46 asks a classic interview/exam question: 
  > **"What will this filter do, and why is the resulting image shifted to the right?"**
  
  ```
     Filter Kernel K           Convolution Reflection (K flipped)         Output Behavior
   ┌───┬───┬───┐                   ┌───┬───┬───┐                      ┌───┬───┬───┐
   │ 0 │ 0 │ 0 │                   │ 0 │ 0 │ 0 │                      │   │   │   │
   ├───┼───┼───┤    180° Flip      ├───┼───┼───┤                      ├───┼───┼───┤
   │ 0 │ 0 │ 1 │ ───────────────►  │ 1 │ 0 │ 0 │  ───────────────►    │   │P' │   │ = P_left
   ├───┼───┼───┤   (Convolution)   ├───┼───┼───┤                      ├───┼───┼───┤
   │ 0 │ 0 │ 0 │                   │ 0 │ 0 │ 0 │                      │   │   │   │
   └───┴───┴───┘                   └───┴───┴───┘                      └───┴───┴───┘
    Weight at (0, +1)               Weight at (0, -1)                  Output at [m, n] takes
                                                                       value from [m, n-1]!
  ```
#### The Mathematical Proof:
  1. **Under Cross-Correlation (No Flip):**
  
  $$(f \otimes \mathbf{K}_{\text{shift}})[m, n] = \sum_{i} \sum_{j} f[m + i, \, n + j] \cdot K[i, j] = f[m + 0, \, n + 1]$$
  
   * To compute the output at $(m, n)$, cross-correlation samples the pixel to the **right** ($n + 1$).
   * As a result, features from the right move into the current pixel, shifting the visible scene **one pixel to the left**.
  
  2. **Under True Convolution (With Coordinate Flip):**
  
  $$(f * \mathbf{K}_{\text{shift}})[m, n] = \sum_{i} \sum_{j} f[m - i, \, n - j] \cdot K[i, j]$$
  
   * Since $K[i, j] = 1$ at $(i = 0, j = +1)$:
  
  $$(f * \mathbf{K}_{\text{shift}})[m, n] = f[m - 0, \, n - (+1)] = f[m, \, n - 1]$$
  
   * To compute the output at $(m, n)$, convolution samples the pixel to the **left** ($n - 1$).
   * The pixel originally located at $(m, n - 1)$ is moved forward into coordinate $(m, n)$, shifting the entire visual image **one pixel to the right**.
  
  > **Key Rule:** Convolution reflects the kernel by $180^\circ$. A weight placed at offset $(+\Delta x, +\Delta y)$ shifts the reconstructed image by $(+\Delta x, +\Delta y)$.
  
  ---
## 2. Image Sharpening & Unsharp Masking (Slides 48–51)
  
  Slide 48 presents the following composite kernel:
  
  $$\mathbf{K}_{\text{sharp}} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 2 & 0 \\ 0 & 0 & 0 \end{bmatrix} - \frac{1}{9} \begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix} = 2 \cdot \mathbf{K}_{\text{id}} - \mathbf{K}_{\text{box}}$$
  
  $$\mathbf{K}_{\text{sharp}} = \begin{bmatrix} 
  -1/9 & -1/9 & -1/9 \\ 
  -1/9 & 17/9 & -1/9 \\ 
  -1/9 & -1/9 & -1/9 
  \end{bmatrix} \approx \frac{1}{9} \begin{bmatrix} 
  -1 & -1 & -1 \\ 
  -1 & 17 & -1 \\ 
  -1 & -1 & -1 
  \end{bmatrix}$$
  
  Slide 49 notes: *"A Sharpening Filter: accentuates edges, yet leaves flat regions unchanged."*
  
  ```
   Original Signal (Edge)               Blurred (Low-Pass)               Detail (High-Pass = Orig - Blur)
            ^                                  ^                                  ^
        1.0 │      ╭───────                1.0 │        ╭─────                0.5 │        ╭─╮
            │      │                           │       ╭╯                         │        │ ╰─
            │      │                           │      ╭╯                          │        │
        0.0 └──────┴───────►               0.0 └──────┴───────►               0.0 └──────┬─┴──────►
                                                                              -0.5│   ─╮ │
                                                                                      ╰─╯
                                               │                                  │
                                               └─────────────────┬────────────────┘
                                                                 ▼
                                                  Sharpened = Orig + Detail
                                                  (Overshoots on high side,
                                                   undershoots on low side)
  ```
  
  ---
### A. The Principle of Unsharp Masking
  This operation originates from darkroom photography. By subtracting a blurred (defocused) copy of an image from the original, we isolate its **high-frequency spatial details**:
  
  $$\text{Detail}[m, n] = f[m, n] - f_{\text{smooth}}[m, n] = (f * \mathbf{K}_{\text{id}})[m, n] - (f * \mathbf{K}_{\text{smooth}})[m, n]$$
  
  Scaling and adding this high-frequency detail back to the original yields the **unsharp masked image**:
  
  $$f_{\text{sharp}}[m, n] = f[m, n] + \alpha \cdot \text{Detail}[m, n]$$
  
  $$f_{\text{sharp}}[m, n] = f[m, n] + \alpha \big(f[m, n] - f_{\text{smooth}}[m, n]\big) = (1 + \alpha) f[m, n] - \alpha f_{\text{smooth}}[m, n]$$
  
  Setting the boosting coefficient $\alpha = 1$:
  
  $$f_{\text{sharp}} = 2 f - f_{\text{smooth}} = f * \big(2 \cdot \mathbf{K}_{\text{id}} - \mathbf{K}_{\text{box}}\big)$$
  
  This matches the kernel expression on Slide 48.
  
  ---
### B. Why Do Flat Regions Remain Unchanged?
  Evaluating the kernel's total energy (sum of weights):
  
  $$\sum_{i=-1}^1 \sum_{j=-1}^1 K_{\text{sharp}}[i, j] = 2 \sum K_{\text{id}} - \sum K_{\text{box}} = 2(1.0) - 1.0 = \mathbf{1.0}$$
  
  Because the sum of weights equals $1.0$, the filter has **unity DC gain**. 
  In a flat, uniform region where every neighboring pixel equals constant $C$:
  
  $$f_{\text{sharp}}[m, n] = 2(C) - \frac{1}{9}(9C) = 2C - C = C$$
  
  Flat surfaces are unaffected; the filter responds only where local spatial variance is non-zero (i.e., across edges).
  
  ---
### C. The Cost of Oversharpening: Halo and Ringing Artifacts (Slide 51)
  Slide 51 shows a red-tailed hawk processed with increasing sharpening strengths:
  1. **Original:** Natural optical transition at feather boundaries.
  2. **Sharpened:** Crisp visual definition; micro-textures on feathers stand out.
  3. **Oversharpened:** Noticeable synthetic artifacts:
   * **Halos:** Bright fringes appear on the light side of high-contrast boundaries, and dark bands appear on the shadowed side (Gibbs-like ringing phenomenon).
   * **Noise Amplification:** Sensor noise in smooth background skies—originally imperceptible—is treated as high-frequency edge detail and amplified into grain.
  
  ---
## 3. The Gaussian Blur Filter (Slides 52–60)
  
  While the Box Filter is computationally simple, it produces blocky, axis-aligned artifacts. The **Gaussian Filter** is the standard smoothing operator across both biological and computer vision.
  
  ```
       Box Filter vs. Gaussian Spatial Profile
       
       Box Kernel (Discontinuous Step):          Gaussian Kernel (Smooth Isotropic Bell):
            ┌─────────┐                                      ╭─────╮
            │         │                                    ╭─╯     ╰─╮
            │         │                                  ╭─╯         ╰─╮
       ─────┘         └─────                        ─────┴─────────────┴─────
       Hard, abrupt cutoff                          Smooth, asymptotic decay to 0
  ```
  
  ---
### A. Continuous Formulation & The Scale Parameter $\sigma$ (Slides 53, 57)
  The continuous 2D rotationally symmetric (isotropic) Gaussian distribution with zero mean is defined as:
  
  $$G_\sigma(x, y) = \frac{1}{2\pi\sigma^2} \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right)$$
  
  * **$\sigma$ (Standard Deviation):** The scale parameter of the filter. 
  * Controls the spatial spread (width) of the smoothing window.
  * Larger $\sigma$ values attenuate finer image details, retaining only coarse, low-frequency scene structures (Slide 57).
  
  ---
### B. Discrete Approximation: The Binomial Kernel (Slide 52)
  On a discrete $3 \times 3$ grid, the Gaussian can be approximated using coefficients from Pascal's triangle (the Binomial distribution):
  
  $$\mathbf{K}_{\text{gauss}}^{3 \times 3} = \frac{1}{16} \begin{bmatrix} 1 & 2 & 1 \\ 2 & 4 & 2 \\ 1 & 2 & 1 \end{bmatrix}$$
  
  * **Weight Distribution:**
  * Center pixel $[0, 0]$ receives weight $4/16 = 25\%$.
  * Direct horizontal/vertical neighbors receive $2/16 = 12.5\%$ each.
  * Diagonal corner neighbors receive $1/16 = 6.25\%$ each.
  * **Energy Normalization:**
  
  $$\sum_{i,j} K[i, j] = \frac{1 + 2 + 1 + 2 + 4 + 2 + 1 + 2 + 1}{16} = \frac{16}{16} = \mathbf{1.0}$$
  
  ---
### C. Analytical Proof of Gaussian Separability (Slides 53–56)
  Slide 54 asks: *"Is the Gaussian Blur separable?"* **Yes.**
#### Algebraic Derivation:
  Exploiting the exponential identity $\exp(A + B) = \exp(A)\exp(B)$:
  
  $$G_\sigma(x, y) = \frac{1}{2\pi\sigma^2} \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right) = \left[ \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{x^2}{2\sigma^2} \right) \right] \cdot \left[ \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{y^2}{2\sigma^2} \right) \right]$$
  
  $$G_\sigma(x, y) = G_{1\text{D},\sigma}(x) \cdot G_{1\text{D},\sigma}(y)$$
  
  A 2D Gaussian is the product of two independent 1D Gaussians.
#### Discrete Outer Product Verification (Slide 54):
  
  $$\mathbf{K}_{\text{gauss}}^{3 \times 3} = \frac{1}{16} \begin{bmatrix} 1 & 2 & 1 \\ 2 & 4 & 2 \\ 1 & 2 & 1 \end{bmatrix} = \left( \frac{1}{4} \begin{bmatrix} 1 \\ 2 \\ 1 \end{bmatrix} \right) \cdot \left( \frac{1}{4} \begin{bmatrix} 1 & 2 & 1 \end{bmatrix} \right) = \mathbf{u} \cdot \mathbf{v}^T$$
#### The Rank-1 Test (Slides 55–56):
  Slide 55 highlights the linear algebra rule: 
  > **A 2D filter matrix is separable if and only if $\text{rank}(\mathbf{K}) = 1$.**
  
  Inspecting $\mathbf{K}_{\text{gauss}}^{3 \times 3}$:
  * Row 2 is exactly $2 \times \text{Row 1}$.
  * Row 3 is identical to Row 1.
  * The rows are linearly dependent; the column space is 1-dimensional ($\text{rank} = 1$). 
  
  Therefore, 2D Gaussian filtering on an $M \times M$ image with a kernel of size $N \times N$ can always be accelerated from $\mathcal{O}(N^2 M^2)$ to $\mathcal{O}(2N M^2)$ without any loss of precision.
  
  ---
### D. Practical Sizing: The $6\sigma$ Support Rule
  A continuous Gaussian has infinite support ($G(x, y) > 0$ for all $x, y \in \mathbb{R}$). For practical computation, we truncate the filter to a finite discrete window of size $W \times W$.
  
  ```
     Continuous Gaussian Profile: Truncation at ±3σ
                       ^
                       │          ╭─────╮
                       │        ╭─╯     ╰─╮
                       │      ╭─╯         ╰─╮
              ─────────┴──────┴─────────────┴──────┴─────────► x
                     -3σ     -2σ     0     +2σ    +3σ
                      └──────────────┬──────────────┘
                       Captures 99.73% of total area
  ```
  
  By the empirical rule of statistics:
  * $[-1\sigma, +1\sigma]$ encompasses $68.27\%$ of the distribution's energy.
  * $[-2\sigma, +2\sigma]$ encompasses $95.45\%$ of the energy.
  * $[-3\sigma, +3\sigma]$ encompasses **$99.73\%$ of the energy**.
  
  Truncating at $\pm 3\sigma$ discards less than $0.27\%$ of the tail energy, avoiding abrupt edge-cutoff artifacts:
  
  $$\text{Kernel Half-Width: } k = \lceil 3\sigma \rceil$$
  
  $$\text{Total Window Dimension: } W = 2k + 1 = 2\lceil 3\sigma \rceil + 1$$
  
  * For $\sigma = 1.0 \implies W = 2(3) + 1 = 7 \times 7$.
  * For $\sigma = 5.0 \implies W = 2(15) + 1 = 31 \times 31$.
  
  ---
## 4. Mean (Box) Filter vs. Gaussian Blur: The Spectral Tradeoff (Slides 57–60)
  
  Slides 59 and 60 compare the Box Filter and Gaussian Blur side-by-side on images of a rounded cube and a brick wall.
  
  ```
       Spatial Domain                                Frequency Domain (Fourier Spectrum)
       
       Box Filter h(x):                              Sinc Function H(ω):
            ┌─────────┐                                       ^
            │         │                                       │   ╭─╮
            │         │                         ──────────────┼──╭╯ ╰╮──╭╮────────► ω
       ─────┘         └─────                                  │ ╭╯   ╰╮╭╯╰╮ (Sidelobes cause
                                                              └─╯     ╰╯   ringing!)
       
       Gaussian Filter g(x):                         Gaussian Spectrum G(ω):
                 ╭─╮                                          ^
               ╭─╯ ╰─╮                                        │   ╭─╮
             ╭─╯     ╰─╮                        ──────────────┼──╭╯ ╰╮────────────► ω
       ──────┴─────────┴─────                                 │ ╭╯   ╰╮  (Monotonic dropoff,
                                                              └─╯     ╰─  zero sidelobes!)
  ```
  
  ---
### Why the Gaussian Looks Visually Superior:
  1. **Rotational Invariance (Isotropy):**
   * The Box Filter averages over a square grid. It smooths more along the diagonals ($\text{distance} = \sqrt{2}$) than along the horizontal/vertical axes ($\text{distance} = 1$). This introduces subtle **axis-aligned grid artifacts** on organic curves.
   * The 2D Gaussian depends only on Euclidean distance from the origin $r^2 = x^2 + y^2$. It is **rotationally invariant** (circularly symmetric), treating all edge orientations equally.
  2. **Frequency Response & The Sinc Phenomenon:**
   * By the Fourier Transform, the frequency response of a spatial rectangular box filter is a **sinc function**:
  
  $$\mathcal{F}\{\text{rect}(x)\} \propto \text{sinc}(\omega) = \frac{\sin(\pi\omega)}{\pi\omega}$$
  
   * The oscillatory secondary lobes of the sinc function pass inverted high frequencies back into the output, producing **visual ringing and grid lines** (readily visible in the brick mortar on Slide 60).
   * In contrast, the Fourier Transform of a Gaussian is **another Gaussian**:
  
  $$\mathcal{F}\left\{ \exp\left(-\frac{x^2}{2\sigma^2}\right) \right\} \propto \exp\left( -\frac{\sigma^2 \omega^2}{2} \right)$$
  
   * A Gaussian has no sidelobes. It acts as an **ideal low-pass filter** that smoothly and monotonically attenuates higher frequencies without phase reversals or ringing.
  
  ---
## 5. The Fundamental Assumptions of LSI Filtering (Slides 61–64)
  
  Slides 61 and 62 focus on the core assumption underlying linear spatial filtering:
  
  > **The Shift-Invariance Assumption:** 
  > We assume that the physical location of an object in an image does not determine how we process it. A filter kernel $\mathbf{K}$ is held constant across all coordinates $(x, y)$.
  
  ---
### Where Shift-Invariance Breaks: Non-Euclidean Sensors (Slide 63)
  Slide 63 displays images captured with **omnidirectional and extreme fisheye lenses**:
  
  ```
        Planar Perspective Projection                  Extreme Fisheye / Omnidirectional Lens
       ───────────────────────────────                ────────────────────────────────────────
       • Constant focal geometry.                     • Radial barrel distortion scales with radius r.
       • Standard LSI filtering works everywhere.     • Center: High angular resolution.
                                                      • Periphery: Heavily compressed / warped.
                                                      • A stationary 5×5 kernel smooths features 
                                                        unequally across the field of view!
  ```
  
  1. **Varying Spatial Resolution:** In a fisheye lens, the angular magnification decreases non-linearly with distance from the optical axis. Applying a static $5 \times 5$ Gaussian kernel blurs fine features near the periphery far more aggressively than at the center.
  2. **Biological Vision (Foveation):** The human retina does not process light with spatial shift-invariance. The central **fovea** possesses dense cone packing (high visual acuity), while the peripheral retina has low photoreceptor density and wide receptive fields.
  3. **Space-Variant Filtering:** In advanced optics and non-rectilinear imaging systems, the shift-invariance assumption breaks down, requiring **coordinate-dependent kernels** $\mathbf{K}(x, y)$ that adapt based on optical position.
  
  ---
### Hand-Crafted Kernels vs. Learned Representations (Slide 64)
  Slide 64 anticipates upcoming material in the course:
  * **The Classical Paradigm (Units 1 & 2):** Human engineers derive filter kernels by hand using calculus and geometry (e.g., Box for averaging, Gaussian for scale-space smoothing, Sobel for gradients).
  * **The Deep Learning Paradigm (Unit 3):** Rather than hand-tuning kernel weights, we initialize parameterized convolutional kernels with random values and optimize their weights directly from data via backpropagation:
  
  $$\mathbf{K}^* = \arg\min_{\mathbf{K}} \mathcal{L}\big(f * \mathbf{K}, \, y_{\text{target}}\big)$$
  
  This allows networks to learn specialized, multi-scale feature extractors tailored to specific tasks.
  
  ---
## Comparison Matrix: Linear Smoothing & Detail Filters
  
  | Kernel Type | Analytical Form / Matrix | Normalization $\sum K$ | Separable? ($\text{rank} = 1$) | Primary Visual Effect | Typical Artifacts |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | **Identity** | Center $= 1$, rest $= 0$ | $1.0$ | Yes | Exact replication | None |
  | **Shift** | Offset index $= 1$ | $1.0$ | Yes | Translates image by $(\Delta x, \Delta y)$ | Boundary clipping |
  | **Box (Mean)** | Uniform weights $\frac{1}{N^2}$ | $1.0$ | Yes | Rough local smoothing | Blocky square edges, sinc ringing |
  | **Sharpening** | $(1+\alpha)\mathbf{K}_{\text{id}} - \alpha\mathbf{K}_{\text{smooth}}$ | $1.0$ | No ($\text{rank} > 1$) | Enhances edge transitions | Halos, amplified noise |
  | **Gaussian** | $\propto \exp\left(-\frac{x^2+y^2}{2\sigma^2}\right)$ | $1.0$ | Yes | Smooth, isotropic blur | Loss of sharp edge localization |
  
  ---