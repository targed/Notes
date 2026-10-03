## 1. The Global Taxonomy of Image Transformations (Slides 13–14)
  
  In computer vision and digital image processing, operations are classified by the **spatial support** required to compute a single output pixel:
  
  ```
                          [ Image Transformations ]
                                      │
       ┌──────────────────────────────┼──────────────────────────────┐
       ▼                              ▼                              ▼
  [ Point Operations ]          [ Local / Neighborhood ]       [ Global / Geometric ]
  • Support: 1 pixel             • Support: N × N window        • Support: Entire image
  • No spatial context           • Filtering, Convolutions      • Warping, Homographies,
  • g(x, y) = T( f(x, y) )       • Edge detection, Blurring       Affine transforms
  ```
  
  ```
   Point Operation                          Neighborhood Filtering
  ┌───┬───┬───┐     ┌───┐                  ┌───┬───┬───┐     ┌───┐
  │   │   │   │     │   │                  │ x │ x │ x │     │   │
  ├───┼───┼───┤     ├───┤                  ├───┼───┼───┤     ├───┤
  │   │ p │   │ ──► │ p'│                  │ x │ p │ x │ ──► │ p'│
  ├───┼───┼───┤     ├───┤                  ├───┼───┼───┤     ├───┤
  │   │   │   │     │   │                  │ x │ x │ x │     │   │
  └───┴───┴───┘     └───┘                  └───┴───┴───┘     └───┘
  1 Input Pixel ──► 1 Output Pixel         N × N Input Patch ──► 1 Output Pixel
  (Context Independent)                    (Context Dependent)
  ```
  
  1. **Point Processing (Slide 15):** The output value at coordinate $(x, y)$ depends **exclusively** on the input intensity value at the exact same coordinate $(x, y)$. The transformation has zero spatial memory of neighboring pixels.
  2. **Neighborhood Operations (Filtering — Slide 14, 38):** The output value at $(x, y)$ is a function of pixel values within a localized spatial window centered at $(x, y)$.
  3. **Geometric Transformations (Warping — Slide 13):** The coordinates themselves are warped: $g(x, y) = f(x', y')$.
  
  ---
## 2. Mathematical Formalism of Point Processing (Slide 15)
  
  Formally, a spatially invariant point operator is a scalar mapping function:
  
  $$T: \mathcal{D}_{\text{in}} \to \mathcal{D}_{\text{out}}$$
  
  where $\mathcal{D}_{\text{in}}, \mathcal{D}_{\text{out}} \subseteq [0, 1]$ (for normalized float) or $\{0, 1, \dots, 255\}$ (for 8-bit unsigned integer). 
  
  For an input image $f(x, y)$, the output image $g(x, y)$ is defined pointwise:
  
  $$g(x, y) = T\big(f(x, y)\big)$$
### High-Performance Optimization: Look-Up Tables (LUTs)
  Because point operations are memoryless and depend only on intensity, evaluating an expensive analytical function (e.g., fractional powers, trigonometric curves, logarithms) over millions of pixels is redundant.
  
  For an 8-bit image, the domain contains only $256$ possible values ($\{0, \dots, 255\}$). A production-grade vision pipeline pre-evaluates $T(r)$ into a **Look-Up Table (LUT)** of size 256, turning an $\mathcal{O}(H \times W)$ series of complex floating-point computations into an $\mathcal{O}(H \times W)$ direct array-indexing operation:
  
  ```python
  import numpy as np
  
  def apply_point_operator_lut(img_uint8: np.ndarray, transform_func) -> np.ndarray:
    """Applies an arbitrary point operator via an O(1) 256-element LUT."""
    # Precompute the transfer curve for all 256 possible byte values
    lut = np.array([transform_func(v) for v in range(256)], dtype=np.uint8)
    
    # Direct index-based vectorized memory substitution
    return lut[img_uint8]
  ```
  
  ---
## 3. Linear Point Transformations: Gain and Bias (Slides 16–20, 26–28)
  
  The simplest family of point operations is the **linear affine intensity transformation**, parametrized by **gain** ($\alpha$) and **bias** ($\beta$):
  
  $$g(x, y) = \alpha \cdot f(x, y) + \beta$$
  
  * **$\alpha$ (Gain):** Controls image **contrast** (dynamic range scaling).
  * **$\beta$ (Bias):** Controls image **brightness** (global level shift).
  
  ```
   Intensity Transfer Functions (T(r) vs. r)
   
     T(r) ^                         T(r) ^
      1.0 │        / (α > 1: Raise   1.0 │       / (β > 0: Lighten)
          │       /   Contrast)          │      /
          │      /                       │     /
          │     /                        │    /
          │    /                         │   /
          │   / (α = 1)                  │  /
          │  /                           │ /  (β = 0)
          │ / (α < 1: Lower              │/
          │/   Contrast)                 │ 
        0 └──────────────► r           0 └──────────────► r
          0              1.0             0              1.0
            GAIN VARIATION                  BIAS VARIATION
  ```
  
  ---
### A. Darken and Lighten (Bias Shifts) (Slides 18, 26, 30)
  Assuming normalized intensities $f(x, y) \in [0.0, 1.0]$:
  
  * **Darken (Slide 18):** 
  
  $$g(x, y) = f(x, y) - 0.5$$
  
  Shifts the entire histogram leftward. All values where $f(x, y) < 0.5$ underflow and must be clamped to $0.0$, collapsing shadow detail into solid black.
  * **Lighten (Slide 26, 30):** 
  
  $$g(x, y) = f(x, y) + 0.5$$
  
  Shifts the entire histogram rightward. All values where $f(x, y) > 0.5$ overflow and clamp to $1.0$, blowing out highlight detail into pure white.
  
  ---
### B. Contrast Scaling (Gain Shifts) (Slides 20, 28)
  * **Lower Contrast (Linear — Slide 20):** 
  
  $$g(x, y) = \frac{1}{2} f(x, y) = 0.5 \cdot f(x, y)$$
  
  Compresses the dynamic range from $[0, 1]$ to $[0, 0.5]$. The histogram narrows by half; dark pixels stay near 0, but highlights become mid-grays, creating a washed-out, muddy appearance.
  * **Raise Contrast (Linear — Slide 28):** 
  
  $$g(x, y) = 2 \cdot f(x, y)$$
  
  Doubles the slope ($\alpha = 2$). Features with subtle gradient differences become distinct, but any pixel originally above $0.5$ saturates to $1.0$.
  
  ---
### C. The Clamping / Saturation Operator
  Linear scaling and shifting easily drive values outside valid sensor representation bounds. Therefore, all physical point operators incorporate a non-linear **clamping operator** (rectification):
  
  $$\text{clamp}(v, v_{\min}, v_{\max}) = \min\big(v_{\max}, \, \max(v_{\min}, v)\big)$$
  
  For normalized floating-point signals:
  
  $$g(x, y) = \text{clamp}\big(\alpha f(x, y) + \beta, \, 0.0, \, 1.0\big)$$
  
  ---
## 4. Nonlinear Point Transformations & Perceptual Tone Curves (Slides 21–25, 29–30)
  
  Linear transformations shift and scale all intensities uniformly, which often causes clipping. **Nonlinear point transformations** allow selective expansion or compression of specific tonal ranges (shadows, midtones, highlights).
  
  ```
   Non-linear Transfer Curves:
   
     T(r) ^
      1.0 │       ╭──────────── γ < 1 (e.g., r^(1/3)) - Expands shadows / Brightens
          │     ╭─╯
          │   ╭─╯
          │ ╭─╯
          │╭╯
          │/ ────────── Linear: T(r) = r
          ││
          ││           ╭───── γ > 1 (e.g., r^2) - Compresses shadows / Darkens
          ││         ╭─╯
        0 └┴─────────┴────────► r
          0                   1.0
  ```
  
  ---
### A. Inversion / Photographic Negative (Slide 24)
  $$g(x, y) = 1.0 - f(x, y) \quad \big(\text{or } 255 - I[r, c] \text{ for 8-bit}\big)$$
  
  * **Physical Meaning:** Reflects the intensity profile across the central midtone axis ($0.5$). 
  * Shadows become highlights, highlights become shadows. Used in medical imaging (e.g., clarifying high-density bone structures on X-rays) and document text binarization.
  
  ---
### B. Power-Law (Gamma) Transformations (Slides 22, 30)
  The general gamma correction model is defined as:
  
  $$g(x, y) = c \cdot f(x, y)^\gamma$$
  
  where $c > 0$ and $\gamma > 0$ (typically $c = 1$ when $f \in [0, 1]$).
#### 1. Case $\gamma < 1$: Expanding Shadows (Nonlinear Lower/Softened Contrast — Slide 22)
  * **Equation:** $g(x, y) = f(x, y)^{1/3}$ (Cubic root)
  * **Derivative:** 
  
  $$\frac{dT}{df} = \frac{1}{3} f^{-2/3} \implies \lim_{f \to 0^+} \frac{dT}{df} = \infty$$
  
  * **Mechanism:** The slope is extremely steep near dark values ($f \to 0$) and flattens out as $f \to 1$. 
  * **Perceptual Effect:** Dark regions and underexposed shadows are stretched across a wider range of display values, recovering shadow detail. Midtones brighten substantially while highlights are compressed without clipping to pure white.
#### 2. Case $\gamma > 1$: Compressing Shadows (Nonlinear Raised Contrast — Slide 30)
  * **Equation:** $g(x, y) = f(x, y)^2$ (Quadratic)
  * **Derivative:** 
  
  $$\frac{dT}{df} = 2f \implies \lim_{f \to 0} \frac{dT}{df} = 0$$
  
  * **Mechanism:** The slope is nearly flat near 0 and steepens near 1.
  * **Perceptual Effect:** Compresses dark tones toward 0, deepening shadows and midtone gloom while expanding contrast exclusively in the upper highlight register.
#### 3. Why Gamma Exists in Real Sensors & Displays:
  * **The Biological Human Visual System (Stevens' Power Law):** Human perception of brightness is nonlinear; we are far more sensitive to subtle differences in dark intensities than in bright intensities.
  * **CRT/Monitor Non-Linearity:** Early CRT electron guns had a physical response of $I_{\text{display}} \propto V^{2.2}$.
  * Modern image formats (sRGB) pre-apply **gamma encoding** ($\gamma \approx 1/2.2$) before storage so that quantization bins are allocated where the human eye has the highest differential sensitivity.
  
  ---
## 5. Advanced Point Processing: Histograms, Tone Management, & Stylization (Slides 31–33)
  
  ---
### A. The Image Histogram: Formal Definition
  Let $I$ be an 8-bit grayscale image of size $H \times W$. The discrete histogram $h$ is an array of size $L = 256$, where each bin $k$ counts occurrences of intensity $k$:
  
  $$h(k) = \sum_{r=0}^{H-1} \sum_{c=0}^{W-1} \delta\big(I[r, c] - k\big), \quad k \in \{0, 1, \dots, 255\}$$
  
  where $\delta(z)$ is the Kronecker delta ($\delta(0) = 1$, otherwise $0$).
  
  * **Normalized Histogram (Empirical Probability Mass Function):**
  
  $$p(k) = \frac{h(k)}{H \times W}, \quad \sum_{k=0}^{L-1} p(k) = 1$$
  
  * **Cumulative Distribution Function (CDF):**
  
  $$C(k) = \sum_{j=0}^k p(j)$$
  
  ```
     Input Dark Image Histogram             Cumulative Distribution Function C(k)
        Number of Pixels                         Probability
     ^                                        ^
     │ █                                    1 │                  ╭─────
     │ █ █                                    │                ╭─╯
     │ █ █ █                                  │             ╭──╯
     │ █ █ █ █                                │          ╭──╯
     │ █ █ █ █ █                              │       ╭──╯
     └──────────────► Intensity               └───────┴──────────────► Intensity
     0             255                        0                     255
  ```
  
  ---
### B. Global Histogram Equalization
  The goal of histogram equalization is to find a monotonic point transformation $s = T(r)$ such that the output probability density $p_s(s)$ is **uniform across the entire dynamic range**:
  
  $$p_s(s) = \frac{1}{L-1}$$
  
  Using continuous probability theory, for a strictly increasing transformation:
  
  $$p_s(s) \, ds = p_r(r) \, dr \implies \frac{ds}{dr} = \frac{p_r(r)}{p_s(s)} = (L-1) \, p_r(r)$$
  
  Integrating both sides yields the equalization transformation:
  
  $$s = T(r) = (L - 1) \int_0^r p_r(w) \, dw = (L - 1) \, C(r)$$
  
  > **Rule:** To equalize an image, the point transformation function $T$ is simply the **Cumulative Distribution Function (CDF)** of the input image scaled by the maximum gray-level value!
  
  ---
### C. Histogram Matching & Photographic Look Transfer: Bae et al. (Slides 31–32)
  Slides 31 and 32 reference *Bae, Paris, and Durand (SIGGRAPH 2006)*: *"Two-scale Tone Management for Photographic Look"*.
  
  ```
   Target Style Photo ──► Extract Tone Curve / Histogram
                                   │
   Input Base Image   ─────────────┼──► Histogram Matching ──► Stylized Output
                                   │
                                   ▼
                   Problem with Direct Histogram Transfer:
                   Spurious gradient reversals, halos, noise amplification!
                   (Bae et al. solved this by separating BASE contrast 
                    from DETAIL textures via bilateral filtering).
  ```
  
  * **Direct Histogram Specification:** Given an input image $A$ and a reference target photo $B$, can we force $A$ to match the histogram of $B$?
  1. Compute the CDF of input $A$: $s = C_A(r)$.
  2. Compute the CDF of target $B$: $z = C_B(t)$.
  3. Map intensities by inverting the target CDF: 
  
  $$\hat{r} = C_B^{-1}\big(C_A(r)\big)$$
  
  * **The Artifact Problem (Slide 31c vs 31d):** Slide 31 shows that **pure point-based histogram transfer** (31c) causes visual artifacts: sky regions develop posterization banding, noise in flat regions is amplified, and cloud borders display halo fringes. Bae et al. resolve this by decomposing the image into base and detail layers, applying point tone curves only to the large-scale base layer.
  
  ---
### D. Stylized Point Curves in Consumer Apps: Instagram Filters (Slide 33)
  Slide 33 showcases Instagram filters (*Clarendon, Gingham*). 
  * Commercial photographic filters are implemented as a cascade of independent **per-channel 1D Look-Up Tables** (S-curves) combined with color-tint cross-talk:
  
  $$\begin{pmatrix} R_{\text{out}} \\ G_{\text{out}} \\ B_{\text{out}} \end{pmatrix} = \begin{pmatrix} T_R(R_{\text{in}}) \\ T_G(G_{\text{in}}) \\ T_B(B_{\text{in}}) \end{pmatrix}$$
  
  * **S-Curve Tone Transfer:** 
  * Compresses deep shadows.
  * Steepens the slope in midtone skin registers (boosting perceived local contrast).
  * Rolls off specular highlights to prevent clipping.
  
  ---
## 6. The Core Assumption and Breakdown of Point Processing (Slides 34–38)
  
  Slides 34 through 37 emphasize how assumptions dictate algorithmic limits:
  
  > **The Point Processing Axiom:** 
  > An individual pixel contains sufficient information to determine its transformation; spatial context is completely unimportant.
  
  $$g(x, y) = T\big(f(x, y)\big) \iff \text{Neighbors } f(x \pm \Delta x, y \pm \Delta y) \text{ have ZERO influence.}$$
  
  ---
### Why Point Processing Fails: The Triptych Proof (Slide 37)
  Slide 37 displays three versions of a smooth 3D rounded cube:
  1. **Original Image**
  2. **Spatially Blurred Image**
  3. **Relit Image (Moved Light Source)**
  
  ```
             Original Cube                     Blurred Cube                       Relit Cube
         ┌───────────────────┐             ┌───────────────────┐             ┌───────────────────┐
         │       ▲           │             │       ░           │             │         ░         │
         │     /   \         │             │     ░░░░░         │             │       /   \       │
         │   / Sharp \       │             │   ░ Blurry ░      │             │     / Diffuse\    │
         │  /  Edge   \      │             │  ░  Boundary░     │             │    /  Shadow  \   │
         │ ◄───────────►     │             │ ░░░░░░░░░░░░░     │             │   ◄───────────►   │
         │   Shadow Step     │             │   Soft Gradient   │             │   New Normal Angle│
         └───────────────────┘             └───────────────────┘             └───────────────────┘
  ```
#### 1. Why No Point Operator Can Blur an Image:
  Consider two pixels in the Original Image:
  * Pixel $A$ is on a flat white facet: $f(x_A, y_A) = 0.8$.
  * Pixel $B$ is on a sharp edge where a white facet meets a dark shadow: $f(x_B, y_B) = 0.8$.
  
  In the **Blurred Image**:
  * Pixel $A$ remains inside the white facet: $g(x_A, y_A) \approx 0.8$.
  * Pixel $B$ averages with the adjacent shadow: $g(x_B, y_B) \approx 0.4$.
  
  For a point operator $T$ to exist, it must satisfy:
  
  $$T(0.8) = 0.8 \quad \text{AND} \quad T(0.8) = 0.4$$
  
  This violates the mathematical definition of a function (a single input cannot map to multiple distinct outputs). 
  
  $$\text{Blurring is fundamentally non-injective in intensity space; it requires spatial differential context!}$$
#### 2. Why No Point Operator Can Relight an Image:
  * Moving the light source changes the angle $\theta = \mathbf{n} \cdot \mathbf{l}$ across surface normals.
  * Two points on different facets of the cube with identical albedo and intensity under the original lighting will receive completely different irradiance values when the illumination vector rotates.
  * Point processing cannot reconstruct 3D surface orientation from an isolated scalar intensity.
  
  ---
## Master Comparison Matrix: Point Processing Operations
  
  | Operation | Mathematical Form ($f \in [0, 1]$) | Histogram Effect | Primary Use Case | Critical Failure Mode |
  | :--- | :--- | :--- | :--- | :--- |
  | **Linear Darken** | $g = \text{clamp}(f - \beta, 0, 1)$ | Rigid left shift | Dimming overexposed images | Destroys dark details via zero-clamping |
  | **Linear Lighten**| $g = \text{clamp}(f + \beta, 0, 1)$ | Rigid right shift | Brightening dark images | Satures highlights to pure white |
  | **Linear Contrast**| $g = \text{clamp}(\alpha f, 0, 1)$ | Uniform spread/squeeze | Stretching flat histograms | Crushes both extremes when $\alpha > 1$ |
  | **Inversion** | $g = 1.0 - f$ | Complete horizontal reflection | Negative film, X-ray analysis | Inverts semantic lighting expectations |
  | **Gamma Expand** | $g = f^\gamma \quad (\gamma < 1)$ | Expands lower tail (shadows) | Dark shadow detail recovery | Can wash out midtone saturation |
  | **Gamma Compress**| $g = f^\gamma \quad (\gamma > 1)$ | Compresses lower tail | Toning down specular burn | Deepens and obscures shadow details |
  | **Hist. Equalize**| $g = (L-1) \cdot \text{CDF}(f)$ | Flattens PDF to uniform | Autonomous baseline normalization | Amplifies background sensor noise |
  | **Spatial Blur** | $\mathbf{g = f * h}$ | **Impossible via Point Ops** | Noise removal, anti-aliasing | **Requires spatial neighborhood!** |
  
  ---
## The Bridge to Module M1.4: Neighborhood Filtering (Slide 38)
  
  Because point operations are mathematically incapable of:
  1. Distinguishing isolated noise spikes from genuine uniform surfaces,
  2. Detecting spatial edges, boundaries, or contours,
  3. Altering spatial frequencies (blurring high frequencies or enhancing fine textures),
  
  the next module (**M1.4: Spatial Filtering & Convolutions**) expands our mathematical support window from a single coordinate $(x, y)$ to an $N \times N$ local neighborhood:
  
  $$g(x, y) = \sum_{i=-k}^k \sum_{j=-k}^k h(i, j) \cdot f(x - i, \, y - j)$$
  
  ---