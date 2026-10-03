## 1. What is an Edge? The Image as a Continuous Surface (Slide 65)
  
  In physical scenes, **edges** mark fundamental geometric and radiometric boundaries:
  1. **Depth Discontinuities:** The silhouette contour where an object occludes a distant background.
  2. **Surface Normal Discontinuities:** Sharp geometric creases (e.g., the junction between two facets of a cube).
  3. **Reflectance / Albedo Discontinuities:** Sudden changes in surface material or pigmentation (e.g., printed text on paper).
  4. **Illumination Discontinuities:** Cast shadow boundaries (penumbras and umbras).
  
  ```
  2D Pixel Lattice I[r, c]                 Continuous 3D Manifold Surface z = f(x, y)
  ┌───┬───┬───┐                                      ^ Intensity z
  │ 10│ 10│ 90│                                  90  │             ╭────────── High Plateau
  ├───┼───┼───┤                                      │             │
  │ 10│ 10│ 90│              ────────►               │         ╭───╯ Steep Gradient Slope:
  ├───┼───┼───┤                                      │         │     ||∇f|| is MAXIMAL here!
  │ 10│ 10│ 90│                                  10  ├─────────╯
  └───┴───┴───┘                                      └─────────────────────► Spatial x
                                                    0          x_edge
  ```
  
  To detect edges computationally, we treat the discrete image array as samples drawn from an underlying continuous intensity surface $z = f(x, y)$. 
  * Where the surface is flat (uniform illumination or texture), the spatial rate of change is zero.
  * Where an edge occurs, the surface forms a steep slope. 
  * To locate edges, we compute the **spatial derivatives** of the image function.
  
  ---
## 2. First-Order Discrete Differentiation: Finite Differences (Slides 66–69)
  
  In classical continuous calculus, the derivative of a 1D function $f(x)$ is defined via an infinitesimal limit:
  
  $$f'(x) = \lim_{h \to 0} \frac{f(x + h) - f(x)}{h}$$
  
  In digital imaging, space is quantized into discrete pixels; the smallest non-zero spatial step is one pixel ($h = 1$). We cannot evaluate limits as $h \to 0$. Instead, we must approximate derivatives using **finite differences**.
  
  ---
### A. Forward, Backward, and Central Differences
  Using Taylor series expansions:
  
  $$f(x + 1) = f(x) + f'(x) + \frac{1}{2}f''(x) + \frac{1}{6}f'''(x) + \mathcal{O}(h^4)$$
  
  $$f(x - 1) = f(x) - f'(x) + \frac{1}{2}f''(x) - \frac{1}{6}f'''(x) + \mathcal{O}(h^4)$$
  
  1. **Forward Difference:**
  
  $$f_{\text{forward}}'(x) \approx \frac{f(x + 1) - f(x)}{1} \quad \big[\text{Truncation Error: } \mathcal{O}(h)\big]$$
  
  2. **Backward Difference:**
  
  $$f_{\text{backward}}'(x) \approx \frac{f(x) - f(x - 1)}{1} \quad \big[\text{Truncation Error: } \mathcal{O}(h)\big]$$
  
  3. **Central Difference (Slide 67):**
  Subtracting $f(x - 1)$ from $f(x + 1)$ eliminates all even-order error terms ($f''(x)$ cancels out):
  
  $$f(x + 1) - f(x - 1) = 2 f'(x) + \frac{1}{3} f'''(x) + \dots$$
  
  $$f_{\text{central}}'(x) \approx \frac{f(x + 1) - f(x - 1)}{2} \quad \big[\text{Truncation Error: } \mathcal{O}(h^2)\big]$$
  
  > **Why Central Differences are Preferred:** The central difference is second-order accurate ($\mathcal{O}(h^2)$) and symmetrically centered around coordinate $x$, preventing sub-pixel phase shifts.
  
  ---
### B. Representing the Derivative as a Convolution Kernel (Slides 68–69)
  Slide 68 asks: *"Which of these is the 1D derivative filter in convolution?"*
  
  $$\frac{1}{2}\begin{bmatrix} -1 & 0 & 1 \end{bmatrix} \quad \text{versus} \quad \frac{1}{2}\begin{bmatrix} 1 & 0 & -1 \end{bmatrix}$$
#### Derivation via the Convolution Equation:
  The discrete 1D convolution of a signal $f$ with a 3-tap kernel $\mathbf{k} = [k_{-1}, k_0, k_1]$ is:
  
  $$(f * \mathbf{k})[x] = \sum_{i=-1}^1 f[x - i] \cdot k[i] = f[x + 1]\cdot k[-1] + f[x]\cdot k[0] + f[x - 1]\cdot k[1]$$
  
  We want this expression to evaluate to the central difference:
  
  $$(f * \mathbf{k})[x] = \frac{1}{2} f[x + 1] - \frac{1}{2} f[x - 1]$$
  
  Equating coefficients:
  * For the $f[x + 1]$ term: $k[-1] = +\frac{1}{2}$
  * For the $f[x]$ term: $k[0] = 0$
  * For the $f[x - 1]$ term: $k[+1] = -\frac{1}{2}$
  
  Writing these taps in array order (from left index $-1$ to right index $+1$):
  
  $$\mathbf{k}_{\text{conv}} = \frac{1}{2}\begin{bmatrix} 1 & 0 & -1 \end{bmatrix} \quad \text{(Slide 69)}$$
  
  * If using **cross-correlation**, the kernel is $\frac{1}{2}[-1, 0, 1]$.
  * If using **true convolution** (which reflects coordinates by $180^\circ$), the kernel is $\frac{1}{2}[1, 0, -1]$.
  
  ---
## 3. The 2D Image Gradient Vector (Slides 74–76, 82)
  
  For a 2D scalar image function $f(x, y)$, spatial derivatives exist along both dimensions:
  * Horizontal derivative: $\frac{\partial f}{\partial x}$
  * Vertical derivative: $\frac{\partial f}{\partial y}$
  
  Combining these partial derivatives yields the **Image Gradient**, a vector-valued quantity defined at every single pixel:
  
  $$\nabla f(x, y) = \begin{bmatrix} \frac{\partial f}{\partial x} \\[4pt] \frac{\partial f}{\partial y} \end{bmatrix} \in \mathbb{R}^2$$
  
  ```
                               ^ y (Rows)
                               │         Gradient Vector ∇f
                               │        /|
                               │       / |  ∂f/∂y
                               │      /θ |
                               │     /___|
                               │    0    ∂f/∂x
                               └──────────────────────► x (Cols)
                               
   Edge Direction (Tangential Contour) ───────┐
                                              │ Perpendicular: θ_edge = θ_grad + 90°
   Gradient Vector ∇f (Max Rate of Change) ───┘
  ```
  
  ---
### Key Properties of the Gradient Vector:
  1. **Direction of Maximum Ascent (Slide 75):**
   The gradient vector $\nabla f$ points in the direction of the **most rapid increase in pixel intensity**.
  2. **Orthogonality to Edge Contours:**
   Along a physical edge boundary, intensity remains approximately constant along the contour tangent. Because the directional derivative along an isoline is zero, the gradient vector points **perpendicular (normal) to the edge**.
  
  ---
### Mathematical Metrics: Magnitude and Orientation (Slide 75, 82)
#### 1. Gradient Magnitude (Edge Strength):
  Measures the steepness of the intensity transition:
  
  $$\|\nabla f\| = \sqrt{\left(\frac{\partial f}{\partial x}\right)^2 + \left(\frac{\partial f}{\partial y}\right)^2}$$
  
  * For fast integer computation, it is often approximated via absolute sums (Manhattan metric):
  
  $$\|\nabla f\|_1 \approx \left|\frac{\partial f}{\partial x}\right| + \left|\frac{\partial f}{\partial y}\right|$$
#### 2. Gradient Orientation (Slide 75):
  Gives the geometric angle of the normal to the edge:
  
  $$\theta = \text{arctan2}\left(\frac{\partial f}{\partial y}, \, \frac{\partial f}{\partial x}\right) \in [-\pi, \pi]$$
  
  > **Always use `arctan2(dy, dx)` instead of `arctan(dy / dx)`:**
  > The standard single-argument arctangent cannot distinguish between an angle of $45^\circ$ ($+dy, +dx$) and an angle of $-135^\circ$ ($-dy, -dx$), and divides by zero when $dx = 0$. The two-argument `arctan2` checks the signs of both components to resolve the quadrant across the full $360^\circ$ circle.
  
  ---
## 4. The Sobel Operator: Separable Smoothing + Differentiation (Slides 70–73)
  
  Evaluating pure finite differences ($[1, 0, -1]$) across a single scanline makes the operation vulnerable to pixel noise. The **Sobel Operator** addresses this by combining **finite-difference differentiation** in one direction with **triangular smoothing** in the orthogonal direction.
  
  ```
       Horizontal Sobel Filter S_x                     Vertical Sobel Filter S_y
   (Detects Vertical Intensity Edges)              (Detects Horizontal Intensity Edges)
          ┌───┬───┬───┐                                   ┌───┬───┬───┐
          │ 1 │ 0 │-1 │                                   │ 1 │ 2 │ 1 │
    S_x = ├───┼───┼───┤                             S_y = ├───┼───┼───┤
          │ 2 │ 0 │-2 │                                   │ 0 │ 0 │ 0 │
          ├───┼───┼───┤                                   ├───┼───┼───┤
          │ 1 │ 0 │-1 │                                   │-1 │-2 │-1 │
          └───┴───┴───┘                                   └───┴───┴───┘
  ```
  
  ---
### A. Factorization & Separability of the Sobel Filters (Slide 70)
  Both Sobel operators are rank-1 matrices and can be factored into 1D smoothing and derivative passes:
#### Horizontal Gradient $S_x$:
  
  $$\mathbf{S}_x = \begin{bmatrix} 1 & 0 & -1 \\ 2 & 0 & -2 \\ 1 & 0 & -1 \end{bmatrix} = \begin{bmatrix} 1 \\ 2 \\ 1 \end{bmatrix} \cdot \begin{bmatrix} 1 & 0 & -1 \end{bmatrix} = \mathbf{u}_{\text{smooth}} \cdot \mathbf{v}_{\text{diff}}^T$$
  
  * **Vertical Axis:** Convolves with $\begin{bmatrix} 1 & 2 & 1 \end{bmatrix}^T$ (a $1\text{D}$ triangular/binomial low-pass filter to suppress noise).
  * **Horizontal Axis:** Convolves with $\begin{bmatrix} 1 & 0 & -1 \end{bmatrix}$ (a $1\text{D}$ central-difference high-pass filter).
#### Vertical Gradient $S_y$:
  
  $$\mathbf{S}_y = \begin{bmatrix} 1 & 2 & 1 \\ 0 & 0 & 0 \\ -1 & -2 & -1 \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \\ -1 \end{bmatrix} \cdot \begin{bmatrix} 1 & 2 & 1 \end{bmatrix} = \mathbf{u}_{\text{diff}} \cdot \mathbf{v}_{\text{smooth}}^T$$
  
  * **Vertical Axis:** Convolves with $\begin{bmatrix} 1 & 0 & -1 \end{bmatrix}^T$ (differentiates along rows).
  * **Horizontal Axis:** Convolves with $\begin{bmatrix} 1 & 2 & 1 \end{bmatrix}$ (smooths along columns).
  
  ---
### B. Visual Evidence: Directional Selectivity (Slides 72–73)
  * **High-Rise Skyscraper (Slide 72):**
  * The **Horizontal Sobel ($S_x$)** responds strongly to the vertical mullions and vertical pillars running up the face of the building, while ignoring horizontal floor lines.
  * The **Vertical Sobel ($S_y$)** responds strongly to the horizontal window sills and floor dividers, while producing low responses on the vertical pillars.
  * **Brick Wall (Slide 73):**
  * Applying $S_y$ extracts the horizontal mortar courses running cleanly across the facade.
  * Applying $S_x$ extracts the staggered vertical joints between adjacent bricks.
  
  ---
## 5. Noise Sensitivity & The Derivative of Gaussian (DoG) (Slides 77–79)
  
  Slide 77 illustrates why taking derivatives of raw digital signals is problematic.
  
  ```
       Ideal Step Edge                     Noisy Sensor Signal              Raw Derivative of Noisy Signal
            ^                                      ^                                      ^
        1.0 │          ╭──────                 1.0 │   /\  /\  ╭───               1.0 │  || | | ||||| | | |||
            │          │                           │  /  \/  \/    \                  │  ||||||||||||||||||||
            │          │                           │                \                 │  ||||||||||||||||||||
        0.0 └──────────┴──────►                0.0 └────────────────►             0.0 └──┴──┴──┴──┴──┴──┴──┴─►
            Clean Discontinuity                   High-frequency noise                NOISE COMPLETELY MASKS
                                                  superimposed on step                THE UNDERLYING EDGE PEAK!
  ```
  
  ---
### A. The Mathematics of High-Frequency Noise Amplification
  Suppose an image signal $f(x)$ is corrupted by additive sinusoidal noise of frequency $\omega$ and amplitude $\epsilon$:
  
  $$f_{\text{noisy}}(x) = f(x) + \epsilon \sin(\omega x)$$
  
  Taking the analytical derivative:
  
  $$\frac{d}{dx} f_{\text{noisy}}(x) = f'(x) + \epsilon \cdot \omega \cos(\omega x)$$
  
  * As frequency $\omega$ increases, the noise derivative is **scaled linearly by $\omega$**.
  * High-frequency sensor noise (even with small amplitude $\epsilon \ll 1$) dominates the output derivative, burying the true signal derivative $f'(x)$.
  
  ---
### B. The Solution: Low-Pass Filter Before Differentiation (Slide 78)
  To obtain reliable derivatives from noisy inputs:
  1. **Smooth the noisy signal** with a continuous Gaussian low-pass filter $g_\sigma$:
  
  $$f_{\text{smooth}}(x) = \big(f * g_\sigma\big)(x)$$
  
  2. **Differentiate the smoothed signal:**
  
  $$f_{\text{edge}}(x) = \frac{d}{dx} \big(f * g_\sigma\big)(x)$$
  
  As shown in Slide 78, smoothing eliminates high-frequency noise oscillations, allowing differentiation to yield a single, stable peak at the true edge boundary.
  
  ---
### C. The Convolution Derivative Theorem (Slide 79)
  Performing a Gaussian convolution followed by a separate numerical derivative requires two filtering passes. 
  
  Using the **Derivative Theorem of Convolution**:
  
  $$\frac{d}{dx}\big(f * g\big) = f * \left(\frac{d}{dx} g\right) = \left(\frac{d}{dx} f\right) * g$$
  
  ```
   Two-Stage Approach (Slow):
   Input f ──► [ Convolve with Gaussian g_σ ] ──► [ Differentiate d/dx ] ──► Edge Map
   
   Optimized Single-Stage Approach via Derivative Theorem:
   Input f ──► [ Convolve with Precomputed (d/dx g_σ) ] ─────────────────► Edge Map
  ```
  
  Instead of smoothing the image and then differentiating, we can **differentiate the Gaussian function analytically beforehand**, creating the **Derivative of Gaussian (DoG)** filter:
  
  $$g_\sigma(x) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left(-\frac{x^2}{2\sigma^2}\right)$$
  
  $$\frac{d}{dx} g_\sigma(x) = -\frac{x}{\sqrt{2\pi}\sigma^3} \exp\left(-\frac{x^2}{2\sigma^2}\right)$$
  
  * **Computational Advantage:** Rather than executing two consecutive 2D convolutions, we convolve the raw input image directly with the pre-evaluated Derivative of Gaussian kernel.
  
  ---
## 6. Second-Order Derivatives & The 2D Laplacian (Slides 80–83)
  
  First derivatives produce an **intensity peak (local extremum)** over edge transitions. **Second-order derivatives** provide an alternative approach: locating edges by finding where the second derivative passes through zero.
  
  ```
       Image Intensity Step f(x)             1st Derivative f'(x)              2nd Derivative f''(x)
            ^                                      ^                                      ^
        1.0 │          ╭──────                 1.0 │          ╭─╮                     1.0 │       ╭─╮  <-- Positive Peak
            │          │                           │        ╭─╯ ╰─╮                       │      ╭╯ ╰╮
            │          │                           │      ╭─╯     ╰─╮                     │ ─────┼───┼─────► 0
        0.0 └──────────┴──────►                0.0 └──────┴─────────┴─►               -1.0│     ╭╯   ╰╮
               Step Discontinuity                     PEAK AT EDGE                        │    ─╯     ╰─  <-- Negative Peak
                                                                                          ZERO-CROSSING AT EDGE!
  ```
  
  ---
### A. 1D Second Finite Difference Derivation (Slide 80)
  From the Taylor expansions for $f(x + 1)$ and $f(x - 1)$:
  
  $$f(x + 1) + f(x - 1) = 2f(x) + f''(x) + \mathcal{O}(h^4)$$
  
  Solving directly for the second derivative $f''(x)$:
  
  $$f''(x) \approx f(x + 1) - 2f(x) + f(x - 1)$$
  
  Expressed as a discrete 1D convolution kernel:
  
  $$\mathbf{k}_{\text{laplace}}^{1\text{D}} = \begin{bmatrix} 1 & -2 & 1 \end{bmatrix}$$
  
  ---
### B. The 2D Continuous Laplacian Operator ($\nabla^2$) (Slide 81, 83)
  The continuous Laplacian $\nabla^2$ is the divergence of the gradient vector—a scalar differential operator invariant to coordinate rotations:
  
  $$\nabla^2 f = \nabla \cdot (\nabla f) = \frac{\partial^2 f}{\partial x^2} + \frac{\partial^2 f}{\partial y^2}$$
  
  ---
### C. Discrete 2D Laplacian Kernels (Slide 81, 83)
#### 1. 4-Connected (Cross-Shaped) Laplacian:
  Approximating second derivatives along horizontal and vertical axes:
  
  $$\frac{\partial^2 I}{\partial x^2} \approx I(x+1, y) - 2I(x, y) + I(x-1, y)$$
  
  $$\frac{\partial^2 I}{\partial y^2} \approx I(x, y+1) - 2I(x, y) + I(x, y-1)$$
  
  Summing both axes:
  
  $$\nabla^2 I \approx I(x+1, y) + I(x-1, y) + I(x, y+1) + I(x, y-1) - 4I(x, y)$$
  
  $$\mathbf{K}_{\nabla^2}^{4\text{-conn}} = \begin{bmatrix} 0 & 1 & 0 \\ 1 & -4 & 1 \\ 0 & 1 & 0 \end{bmatrix} \quad \text{or inverted: } \begin{bmatrix} 0 & -1 & 0 \\ -1 & 4 & -1 \\ 0 & -1 & 0 \end{bmatrix}$$
#### 2. 8-Connected (Omnidirectional) Laplacian:
  Including diagonal neighbors gives a more rotationally isotropic approximation:
  
  $$\mathbf{K}_{\nabla^2}^{8\text{-conn}} = \begin{bmatrix} 1 & 1 & 1 \\ 1 & -8 & 1 \\ 1 & 1 & 1 \end{bmatrix} \quad \text{or inverted: } \begin{bmatrix} -1 & -1 & -1 \\ -1 & 8 & -1 \\ -1 & -1 & -1 \end{bmatrix}$$
  
  > **Key Invariant: All Laplacian Kernels Sum to Exactly 0.**
  > Because the second derivative of any constant function is zero, the sum of all elements in any valid discrete Laplacian kernel must equal zero:
  >
  > $$\sum_{i, j} K_{\nabla^2}[i, j] = 0$$
  >
  > In flat, constant intensity regions, the filter produces an output of exactly zero.
  
  ---
## 7. The Laplacian of Gaussian (LoG / Mexican Hat) (Slides 83–86)
  
  Because the Laplacian computes second-order spatial derivatives, it is **doubly sensitive** to high-frequency noise. Direct application of a discrete $3 \times 3$ Laplacian kernel to real images yields noisy, unusable results.
  
  To make it practical, we combine the Laplacian operator with a Gaussian smoothing filter:
  
  $$\text{LoG}(x, y) = \nabla^2 \big( f(x, y) * G_\sigma(x, y) \big) = f(x, y) * \big(\nabla^2 G_\sigma(x, y)\big)$$
  
  ---
### A. Analytical Derivation of the LoG Function (Slide 83)
  Starting from the continuous 2D rotationally symmetric Gaussian:
  
  $$G_\sigma(x, y) = \frac{1}{2\pi\sigma^2} \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right)$$
  
  Let $r^2 = x^2 + y^2$. Taking second partial derivatives with respect to $x$ and $y$ and summing:
  
  $$\nabla^2 G_\sigma(x, y) = \frac{\partial^2 G_\sigma}{\partial x^2} + \frac{\partial^2 G_\sigma}{\partial y^2} = -\frac{1}{\pi\sigma^4} \left( 1 - \frac{x^2 + y^2}{2\sigma^2} \right) \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right)$$
  
  ```
     The 2D "Mexican Hat" Profile (LoG Kernel)
                       ^ Intensity
                  0.5  │           ╭─╮           <-- Positive Central Peak (Sharp)
                       │         ╭─╯ ╰─╮
                   0.0 ┼─────────╯─────╰─────────► Radius r
                       │     ╭───╯     ╰───╮     <-- Negative Surrounding Ring
                 -0.25 │    ╭╯             ╰╮
                       └────┴───────────────┴────
  ```
  
  * **Shape:** A sharp positive central lobe surrounded by an inverted circular negative ring, resembling an inverted sombrero ("Mexican Hat").
  * **Properties:**
  * Integrates to exactly 0: $\iint \text{LoG}(x, y) \, dx \, dy = 0$.
  * Acts as a **band-pass filter**, attenuating low frequencies (uniform flat areas) as well as extreme high frequencies (sensor noise).
  
  ---
### B. Detecting Edges via Zero-Crossings (Slides 84–86)
  First-order derivative methods (such as Sobel) require thresholding gradient magnitude peaks:
  
  $$\text{Edge}(x, y) \iff \|\nabla f(x, y)\| > \tau$$
  
  Choosing an arbitrary global threshold $\tau$ often causes broken or fragmented contours.
  
  The Laplacian of Gaussian locates edges through **Zero-Crossings**:
  
  $$\text{Edge}(x, y) \iff \nabla^2(G_\sigma * f)(x, y) \text{ changes sign between opposite neighbors}$$
  
  ```
   Across a Step Edge:
   Adjacent Pixel A: +24.2  (Positive)
   Adjacent Pixel B: -18.7  (Negative)
   ───────────────────────────────────
   Sign change detected (+ to -) ──► An edge exists precisely on the boundary!
  ```
  
  * **Brain MRI Case Study (Slide 85):**
  Slide 85 demonstrates an axial brain MRI scan processed with the LoG filter. Taking zero-crossings extracts closed, single-pixel-wide anatomical contours across both high-contrast skull boundaries and subtle soft-tissue brain structures.
  * **Leaf Vein Case Study (Slide 86):**
  Slide 86 compares the Derivative of Gaussian against the Laplacian of Gaussian:
  * Derivative of Gaussian: Generates a wide, diffuse peak across the leaf vein.
  * Laplacian of Gaussian: Generates a distinct **zero-crossing** line right down the center of the vein.
  
  ---
## 8. Summary Comparison: First vs. Second Derivative Edge Detection
  
  | Metric / Property | First Derivative ($\nabla f$, Sobel, DoG) | Second Derivative ($\nabla^2 f$, LoG) |
  | :--- | :--- | :--- |
  | **Mathematical Formulation** | $\nabla f = [\partial f/\partial x, \, \partial f/\partial y]^T$ | $\nabla^2 f = \partial^2 f/\partial x^2 + \partial^2 f/\partial y^2$ |
  | **Edge Indicator** | **Peak / Local Maximum** in magnitude $\|\nabla f\|$ | **Zero-Crossing** (sign change across zero) |
  | **Kernel Sum** | $\sum K = 0$ (Differential operator) | $\sum K = 0$ (Differential operator) |
  | **Directional Sensitivity** | Directional ($S_x$ detects vertical edges, $S_y$ horizontal) | Isotropic / Omnidirectional (scalar operator) |
  | **Noise Vulnerability** | Moderate ($\propto \omega$ noise amplification) | Severe ($\propto \omega^2$ amplification; requires Gaussian) |
  | **Edge Contour Properties** | Can yield thick, multi-pixel ridges requiring NMS | Naturally yields thin, continuous single-pixel closed loops |
  
  ---## 9. What Linear Filtering Cannot Do: The Transition to Warping (Slides 87–88)
  
  Slides 87 and 88 show the complete triad of image operations:
  1. **Point Processing:** Modifies dynamic range and tone curves (intensity-space mappings).
  2. **Neighborhood Filtering:** Modifies spatial frequencies, smooths noise, and detects differential edges (linear combinations of local neighborhoods).
  
  Slide 88 highlights the remaining corner of the triad:
  
  > **What can't we do with filtering? Warping!**
  
  ```
     Filtering Cannot Change Spatial Coordinates:
     
     ┌───────────┐                     ┌───────────┐
     │           │   Linear Filter     │   ░░░░░   │  The output pixels remain locked
     │  Object   │ ──────────────────► │  Blurred  │  on the exact same (x, y) 
     │           │   g = f * K         │   Edge    │  coordinate lattice!
     └───────────┘                     └───────────┘
     
     Warping Modifies the Coordinate Lattice Itself:
     
     ┌───────────┐                     ┌───────────────┐
     │           │   Geometric Warp    │      /       /│  Rotates, scales, skews, 
     │  Object   │ ──────────────────► │     / Object/ │  or perspective-projects
     │           │  g(x, y) = f(x', y')│    /       /  │  coordinates across space!
     └───────────┘                     └───┴───────┴───┘
  ```
  
  Linear filtering cannot:
  * Rotate an image by $45^\circ$,
  * Perspective-warp a flat document on a desk into a front-facing scan,
  * Stitch two overlapping photographs into a panorama.
  
  These tasks require **Geometric Image Transformations & Warping**, the topic of the next module.
  
  ---