## 1. The Residual Concept: Decomposing Detail from Structure (Slides 44–46)
  
  In **Part 2**, we saw that applying the Burt-Adelson $\text{REDUCE}$ operator smooths an image, attenuating its high spatial frequencies. 
  
  Slide 44 poses a fundamental question:
  > **"So how does smoothing the image change it? What if we take the difference between the original and the smoothed version?"**
  
  ```
    Original Image (f₀)             Smoothed Image (l₀)           Difference / Residual (h₀)
  ┌───────────────────────┐       ┌───────────────────────┐       ┌───────────────────────┐
  │ ╭────────╮            │       │ ╭────────╮            │       │ ░░░░░░░░░░            │
  │ │ Hibiscus Detail     │   -   │ │ Blurry Hibiscus     │   =   │ ░ Fine Edges, Veins,  │
  │ │ Stamen, Petal Veins │       │ │ Low Frequencies Only│       │ ░ High-Freq Residual  │
  │ ╰────────╯            │       │ ╰────────╯            │       │ ░ (Centered around 0) │
  └───────────────────────┘       └───────────────────────┘       └───────────────────────┘
        Full Signal                      Base Signal                     Detail Signal
  ```
  
  ---
### A. The Signal Processing Decomposition
  Any digital image $f[m, n]$ can be decomposed into two orthogonal frequency components:
  1. **Low-Frequency Base Signal ($f_{\text{low}}$):** Smooth color fields, gradual lighting variations, and broad geometric shapes.
  2. **High-Frequency Detail Signal ($f_{\text{high}}$):** Sharp boundaries, fine textures, surface scratches, and edges.
  
  $$f[m, n] = f_{\text{low}}[m, n] + f_{\text{high}}[m, n]$$
  
  $$f_{\text{high}}[m, n] = f[m, n] - f_{\text{low}}[m, n]$$
  
  ---
### B. Statistical Properties of Image Residuals
  Why is storing the difference $f_{\text{high}}$ useful?
  * **Zero-Mean Distribution:** While the original image $f$ has pixel intensities spanning $[0, 255]$, the residual $f - f_{\text{low}}$ is **concentrated tightly around zero**.
  * **Laplacian / Exponential PDF:** The histogram of an image residual follows a sharp peak at zero with heavy tails (a Laplacian probability distribution):
  
  $$P(x) \approx \frac{1}{2b} \exp\left(-\frac{|x|}{b}\right)$$
  
  ```
     Original Image Histogram                   Residual Image Histogram
          (High Entropy)                             (Low Entropy)
       ^                                          ^
       │   ╭─╮     ╭─╮                            │        █
       │ ╭─╯ ╰─╮ ╭─╯ ╰─╮                          │        █
       │╭╯     ╰─╯     ╰─                         │       ███
       └──────────────────► Intensity             │      █████
       0                 255                      └───────┴─┴───────► Value
                                                -128      0       +127
  ```
  
  * **Compression Efficiency:** Because most pixel values in the residual are near zero, the signal has **low Shannon entropy**. It can be quantized and compressed (via Huffman coding, Run-Length Encoding, or Arithmetic coding) with high efficiency.
  
  ---
## 2. The `EXPAND` Operator (Slides 46–47)
  
  To compute the difference between level $g_l$ and its downsampled version $g_{l+1}$, we cannot subtract them directly because their array dimensions do not match:
  
  $$\text{Size of } g_l = M \times M \quad \text{vs.} \quad \text{Size of } g_{l+1} = \frac{M}{2} \times \frac{M}{2}$$
  
  We must first **interpolate** the lower-resolution image $g_{l+1}$ back up to the size of $g_l$ using the **$\text{EXPAND}$ operator**:
  
  $$g_l^{\text{predicted}} = \text{EXPAND}(g_{l+1})$$
  
  ```
                   The EXPAND Pipeline (Upsampling by 2)
                   
   Coarse Image g_{l+1} ──► [ Zero-Insertion (↑ 2) ] ──► [ Interpolation Filter 2·w ] ──► Expanded Image
   (Size: M/2 × M/2)         Inserts zeros at odd         Convolves with doubled           (Size: M × M)
                             rows and columns             Burt-Adelson kernel!
  ```
  
  ---
### A. Mathematical Derivation in 1D (Slide 47)
  Consider interpolating a 1D coarse signal $g_{l+1}$ back to fine coordinates $y[i]$:
  
  $$y[i] = \text{EXPAND}(g_{l+1})[i] = 2 \sum_{k=-2}^2 w[k] \cdot g_{l+1}\left[\frac{i - k}{2}\right] \quad \text{for integer } \frac{i - k}{2}$$
  
  Slide 47 shows the sampling geometry for both even and odd indices:
  
  ```
   Coarse Grid g_{l+1}:       C₀                      C₁                      C₂
                              │                       │                       │
   Upsample by 2 (↑ 2):       C₀          0           C₁          0           C₂
                              │           │           │           │           │
   Fine Coordinates:          x₀          x₁          x₂          x₃          x₄
                            (Even)      (Odd)       (Even)      (Odd)       (Even)
  ```
#### 1. Interpolating EVEN Output Pixels ($i = 2k$):
  When evaluating an even coordinate $x_{2k}$, non-zero coarse samples only fall on **even kernel taps** ($k = -2, 0, +2$):
  * Center tap: $w_0 = a$
  * Left tap: $w_{-2} = c$
  * Right tap: $w_2 = c$
  
  $$\text{Total Interpolation Weight}_{\text{even}} = 2 \cdot (c + a + c) = 2 \cdot (a + 2c)$$
#### 2. Interpolating ODD Output Pixels ($i = 2k + 1$):
  When evaluating an odd coordinate $x_{2k+1}$, non-zero coarse samples only fall on **odd kernel taps** ($k = -1, +1$):
  * Left tap: $w_{-1} = b$
  * Right tap: $w_1 = b$
  
  $$\text{Total Interpolation Weight}_{\text{odd}} = 2 \cdot (b + b) = 2 \cdot (2b)$$
  
  ---
### B. Why Multiply the Kernel by 2?
  Recall the **Equal Contribution Criterion** from Part 2:
  
  $$a + 2c = 2b = \frac{1}{2} = 0.5$$
  
  Substituting this into our even and odd interpolation weights:
  
  $$\text{Weight}_{\text{even}} = 2 \cdot (a + 2c) = 2 \cdot (0.5) = \mathbf{1.0}$$
  
  $$\text{Weight}_{\text{odd}} = 2 \cdot (2b) = 2 \cdot (0.5) = \mathbf{1.0}$$
  
  > **The Equal Contribution Guarantee:** 
  > Because Burt and Adelson enforced $a + 2c = 2b = 0.5$, multiplying the kernel taps by a factor of $2$ guarantees that **both even and odd interpolated samples receive a total weight sum of exactly $1.0$**. 
  > 
  > The interpolated signal maintains constant DC gain, preventing artificial brightness oscillations between adjacent pixels.
  
  ---
### C. The 2D `EXPAND` Operator Formulation
  For a 2D image, the operation is applied separably across both axes:
  
  $$g_l^{\text{expanded}}[i, j] = 4 \sum_{m=-2}^2 \sum_{n=-2}^2 w[m] \cdot w[n] \cdot g_{l+1}\left[ \frac{i - m}{2}, \, \frac{j - n}{2} \right]$$
  
  where the summation is restricted strictly to integer values of $\frac{i - m}{2}$ and $\frac{j - n}{2}$. The factor of $4$ ($2 \times 2$) accounts for 2D zero-insertion, where only $1$ out of every $4$ pixels in the upsampled grid contains a non-zero sample.
  
  ---
## 3. Construction and Exact Reconstruction of the Laplacian Pyramid (Slides 45, 50–51)
  
  The **Laplacian Pyramid** is a multi-scale sequence of band-pass error images:
  
  $$\{L_0, L_1, L_2, \dots, L_{K-1}\}$$
  
  where each level stores the structural details that were discarded during downsampling.
  
  ```
                  Decomposition: Analysis (Slide 50)
                  
   g₀ (Original) ──► [ REDUCE ] ─────────────► g₁ ──► [ REDUCE ] ──► g₂ (Base)
         │                                      │                     │
         ▼                                      ▼                     ▼
         -                                      -                  L₂ = g₂
         ▲                                      ▲
         │                                      │
   [ EXPAND ] ◄─────────────────────────── [ EXPAND ]
         │                                      │
         ▼                                      ▼
     L₀ = Residual                          L₁ = Residual
  ```
  
  ---
### A. The Decomposition Algorithm (Pyramid Analysis — Slide 50)
  Given an initial image $g_0$ and a target depth $K$:
  1. Build the Gaussian Pyramid levels $g_0, g_1, \dots, g_{K-1}$ by iteratively applying $\text{REDUCE}$:
  
  $$g_{l+1} = \text{REDUCE}(g_l) \quad \text{for } l = 0, 1, \dots, K-2$$
  
  2. Compute the Laplacian band-pass levels $L_l$ by subtracting the expanded version of the next coarser level from the current level:
  
  $$L_l = g_l - \text{EXPAND}(g_{l+1}) \quad \text{for } l = 0, 1, \dots, K-2$$
  
  3. The highest pyramid level contains no residual. It stores the coarse base image:
  
  $$L_{K-1} = g_{K-1}$$
  
  ---
### B. The Lossless Reconstruction Algorithm (Pyramid Synthesis — Slide 51)
  Slide 46 asks:
  > **"Can we reconstruct the original image perfectly using these differences?"**
  > 
  > *Answer:* **Yes.** The decomposition is completely invertible and information-lossless.
  
  ```
                   Reconstruction: Synthesis (Slide 51)
                   
   L₂ (Coarse Base) ──► g₂
                         │
                         ▼
                    [ EXPAND ]
                         │
                         ▼
   L₁ (Mid Residual) ──►(+) ──► g₁
                                 │
                                 ▼
                            [ EXPAND ]
                                 │
                                 ▼
   L₀ (Fine Residual) ─────────►(+) ──► g₀ (Exact Original Reconstructed!)
  ```
#### Reconstruction Pipeline:
  1. Initialize the top reconstructed level with the base image:
  
  $$g_{K-1} = L_{K-1}$$
  
  2. For each level $l$ from $K-2$ down to $0$, reconstruct the finer Gaussian level by adding the Laplacian residual back to the expanded coarser level:
  
  $$g_l = L_l + \text{EXPAND}(g_{l+1})$$
  
  3. The final result $g_0$ is the **exact, pixel-perfect original image**.
#### Mathematical Proof of Lossless Reconstruction:
  Substitute the definition of $L_l$ directly into the reconstruction equation:
  
  $$g_l = L_l + \text{EXPAND}(g_{l+1})$$
  
  $$g_l = \Big( g_l - \text{EXPAND}(g_{l+1}) \Big) + \text{EXPAND}(g_{l+1})$$
  
  $$g_l = g_l + \Big( \text{EXPAND}(g_{l+1}) - \text{EXPAND}(g_{l+1}) \Big) = g_l$$
  
  The terms cancel algebraically at every stage. Even if the low-pass filter $w(m)$ used in $\text{REDUCE}$ is non-ideal, **any error introduced during smoothing is captured in the residual $L_l$ and added back during reconstruction**.
  
  ---
## 4. Why is it Called the "Laplacian" Pyramid? (Slides 48–49)
  
  Slide 48 reminds us of the **Laplacian of Gaussian (LoG)** operator from Module M1.4:
  
  $$\text{LoG}(x, y) = \nabla^2 \big(f(x, y) * G_\sigma(x, y)\big)$$
  
  Why does subtracting a smoothed image from its original approximate the Laplacian operator?
  
  ```
          Unit Impulse δ                     Gaussian Blur G_σ                Difference (δ - G_σ)
                ^                                    ^                                  ^
            1.0 │    |                           1.0 │     ╭─╮                      1.0 │    |
                │    |                               │   ╭─╯ ╰─╮                        │    |
                │    |                               │ ╭─╯     ╰─╮                      │ ──╭┴╮──
            0.0 ┴────┴────►                      0.0 ┴─┴─────────┴─►                0.0 ┴───┴─┴───►
                                                                                       ╭─╯     ╰─╮
                                                                                       ╰─────────╯
                                                                                    Inverted "Mexican Hat"
                                                                                    Approximates -∇²G !
  ```
  
  ---
### A. The Heat Diffusion Equation Connection
  In mathematical physics, continuous Gaussian blurring is equivalent to simulating the **heat diffusion equation** over time, where scale $\sigma$ corresponds to diffusion time $t = \frac{\sigma^2}{2}$:
  
  $$\frac{\partial f}{\partial t} = \nabla^2 f \iff \frac{\partial f}{\partial \sigma} = \sigma \nabla^2 f$$
  
  Using a finite difference to approximate the derivative with respect to scale $\sigma$:
  
  $$\frac{\partial f}{\partial \sigma} \approx \frac{f(x, y, \sigma + \Delta \sigma) - f(x, y, \sigma)}{\Delta \sigma}$$
  
  Equating the two expressions:
  
  $$f(x, y, \sigma + \Delta \sigma) - f(x, y, \sigma) \approx \Delta \sigma \cdot \sigma \nabla^2 f$$
  
  When $\sigma \to 0$, $f(x, y, 0)$ is the unfiltered original image $f$, and $f(x, y, \Delta \sigma)$ is the smoothed image $f * G_{\Delta \sigma}$:
  
  $$f - (f * G_{\Delta \sigma}) \approx -\kappa \nabla^2 f$$
  
  > **Key Result (Slide 49):** 
  > Subtracting a Gaussian-smoothed image from the original image (or subtracting two images blurred at different scales) directly approximates the **Laplacian of Gaussian ($\nabla^2 G$)**. 
  > 
  > The Laplacian Pyramid is a discrete, computationally efficient approximation of continuous scale-space second derivatives.
  
  ---
## 5. Practical Applications of Laplacian Pyramids (Slides 52–53)
  
  ---
### A. Progressive Image Transmission & Streaming (Slide 52)
  Slide 52 demonstrates progressive image rendering (used in Progressive JPEG and web image streaming over low-bandwidth channels):
  
  ```
     TRANSMITTER                                                 RECEIVER
     ───────────                                                 ────────
     1. Transmit coarsest base g_K  ───────────────► 1. Display low-res thumbnail:
        (Tiny file size, fast!)                         d₁ = EXPAND^K(g_K)
     
     2. Transmit residual L_{K-1}   ───────────────► 2. Add detail:
                                                        d₂ = d₁ + EXPAND^{K-1}(L_{K-1})
     
     3. Transmit residual L_{K-2}   ───────────────► 3. Display sharper image...
     
     4. Transmit residual L₀        ───────────────► 4. Display final lossless image:
                                                        d_{K} = Exact original!
  ```
  
  * **User Experience:** The user sees a recognizable (though blurry) image almost instantly, which sharpens progressively as detail packets arrive, rather than waiting for scanlines to load from top to bottom.
  
  ---
### B. Multi-Band Image Blending (Burt & Adelson, 1983)
  When stitching two photographs together (e.g., in panoramic photography), seam lines and lighting differences create visible boundaries:
  * **The Problem:** 
  * Blending across a **wide boundary** makes sharp edges look blurry or double-exposed (ghosting).
  * Blending across a **narrow boundary** creates a visible, harsh seam where exposure changes abruptly.
  * **The Laplacian Pyramid Solution:**
  1. Decompose both images ($A$ and $B$) into Laplacian pyramids: $\{L_l^A\}$ and $\{L_l^B\}$.
  2. Construct a Gaussian pyramid of the blending mask $M$: $\{G_l^M\}$.
  3. Blend each frequency band independently:
  
  $$L_l^{\text{blended}} = G_l^M \cdot L_l^A + (1 - G_l^M) \cdot L_l^B$$
  
  4. Reconstruct the final image using the synthesis algorithm.
  * **Why it works:** Fine spatial details (high-frequency levels) are blended over a narrow transition strip to prevent ghosting, while ambient background illumination (low-frequency levels) is blended over a wide region to eliminate visible seams.
  
  ---
### C. Continuous Scale-Space & SIFT Foundations (Slide 53)
  Slide 53 references **David Lowe’s SIFT paper (2004)** [2]:
  
  ```
   Octave 2 (1/2 Resolution)
   ┌───┐  ┌───┐  ┌───┐  ┌───┐         Difference of Adjacent Blur Levels
   │   │  │   │  │   │  │   │   ──►   yields the Difference-of-Gaussians (DoG)
   └───┘  └───┘  └───┘  └───┘         DoG Pyramid!
     σ₁     σ₂     σ₃     σ₄
   
   Octave 1 (Full Resolution)
   ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐
   │       │ │       │ │       │ │       │
   └───────┘ └───────┘ └───────┘ └───────┘
     σ₁       σ₂       σ₃       σ₄
  ```
  
  * **Octaves:** An octave corresponds to doubling the Gaussian scale $\sigma$ (halving spatial resolution).
  * **Scales per Octave ($S$):** To detect features that appear at intermediate sizes (e.g., a scale factor of $1.2$ or $1.4$), SIFT generates multiple blur levels within each octave before subsampling.
  * **Difference of Gaussians (DoG):** Subtracting adjacent smoothed images within an octave produces a DoG scale-space volume. Local extrema (comparing a pixel to its 8 spatial neighbors and 18 scale neighbors across adjacent levels) yield **scale-invariant keypoints**.
  
  ---
## 6. Complete Python Implementation: Building and Reconstructing a Laplacian Pyramid
  
  ```python
  import numpy as np
  import scipy.signal as signal
  
  def get_burt_adelson_kernel(a: float = 0.4) -> np.ndarray:
    """Generates the 1D 5-tap Burt & Adelson generating kernel."""
    b = 0.25
    c = 0.25 - 0.5 * a
    return np.array([c, b, a, b, c], dtype=np.float32)
  
  def reduce_level(image: np.ndarray, a: float = 0.4) -> np.ndarray:
    """Low-pass filters and downsamples an image by factor 2."""
    k1d = get_burt_adelson_kernel(a)
    # Separable filtering with reflective boundaries
    low_pass = signal.convolve2d(image, k1d[np.newaxis, :], mode='same', boundary='symm')
    low_pass = signal.convolve2d(low_pass, k1d[:, np.newaxis], mode='same', boundary='symm')
    return low_pass[::2, ::2]
  
  def expand_level(image: np.ndarray, target_shape: tuple, a: float = 0.4) -> np.ndarray:
    """
    Upsamples an image by factor 2 and interpolates with doubled kernel (weight sum = 2).
    target_shape: (H_target, W_target) to handle odd boundary dimensions.
    """
    H_in, W_in = image.shape
    H_out, W_out = target_shape
    
    # Step 1: Up-sample by zero-insertion
    upsampled = np.zeros((H_out, W_out), dtype=np.float32)
    upsampled[0:H_in*2:2, 0:W_in*2:2] = image
    
    # Step 2: Interpolate using the doubled kernel (weights * 2)
    k1d = get_burt_adelson_kernel(a) * 2.0
    
    interpolated = signal.convolve2d(upsampled, k1d[np.newaxis, :], mode='same', boundary='symm')
    interpolated = signal.convolve2d(interpolated, k1d[:, np.newaxis], mode='same', boundary='symm')
    return interpolated
  
  def build_laplacian_pyramid(image: np.ndarray, num_levels: int, a: float = 0.4) -> list:
    """Decomposes an image into a Laplacian Pyramid."""
    # Step 1: Build Gaussian Pyramid
    gauss_pyr = [image.astype(np.float32)]
    for _ in range(num_levels - 1):
        gauss_pyr.append(reduce_level(gauss_pyr[-1], a=a))
        
    # Step 2: Build Laplacian residuals
    laplacian_pyr = []
    for l in range(num_levels - 1):
        expanded = expand_level(gauss_pyr[l + 1], target_shape=gauss_pyr[l].shape, a=a)
        residual = gauss_pyr[l] - expanded
        laplacian_pyr.append(residual)
        
    # Top level stores the coarsest base image
    laplacian_pyr.append(gauss_pyr[-1])
    return laplacian_pyr
  
  def reconstruct_image(laplacian_pyr: list, a: float = 0.4) -> np.ndarray:
    """Synthesizes the exact original image from its Laplacian residuals."""
    num_levels = len(laplacian_pyr)
    current_reconstruction = laplacian_pyr[-1]
    
    for l in range(num_levels - 2, -1, -1):
        expanded = expand_level(current_reconstruction, target_shape=laplacian_pyr[l].shape, a=a)
        current_reconstruction = laplacian_pyr[l] + expanded
        
    return current_reconstruction
  
  # Verification of lossless reconstruction
  original_img = np.random.rand(65, 65).astype(np.float32)
  lap_pyr = build_laplacian_pyramid(original_img, num_levels=4, a=0.4)
  reconstructed = reconstruct_image(lap_pyr, a=0.4)
  
  max_error = np.max(np.abs(original_img - reconstructed))
  print(f"Maximum Reconstruction Error: {max_error:.2e}")
  assert np.isclose(max_error, 0.0, atol=1e-5), "Reconstruction must be lossless!"
  ```
  
  ---
## Master Comparison Matrix: Gaussian vs. Laplacian Pyramids
  
  | Attribute | Gaussian Pyramid | Laplacian Pyramid |
  | :--- | :--- | :--- |
  | **Spectral Content** | **Low-Pass** representation at each scale | **Band-Pass** representation across scales |
  | **Pixel Value Range** | Positive intensity values $[0, 255]$ | High-frequency differences centered near zero |
  | **Probability Distribution** | Multi-modal, scene-dependent | Sharp Laplacian peak at 0 (low entropy) |
  | **Invertibility** | Lossy (subsampling discards high frequencies) | **Lossless** (exact reconstruction via addition) |
  | **Generating Equation** | $g_{l+1} = \text{REDUCE}(g_l)$ | $L_l = g_l - \text{EXPAND}(g_{l+1})$ |
  | **Primary Use Cases** | Coarse-to-fine search, optical flow, scale-space matching | Image compression, multi-band blending, progressive streaming |
  
  ---
## Module M2.1 Complete Summary
  
  1. **Scale is a nuisance parameter:** Physical objects change pixel dimensions with distance, making multi-scale pyramids necessary for robust visual matching.
  2. **Subsampling requires low-pass filtering:** The Nyquist theorem requires attenuating frequencies above the new Nyquist limit before decimation to prevent Moiré patterns and aliasing.
  3. **The Burt & Adelson kernel enforces Equal Contribution:** Setting $a + 2c = 2b = 0.5$ for a symmetric, normalized 5-tap kernel guarantees that all input pixels contribute equally during `REDUCE` and interpolate cleanly during `EXPAND`.
  4. **The Laplacian Pyramid enables lossless multi-band analysis:** By storing residuals between Gaussian levels, the pyramid provides a compact, compressible representation that reconstructs the original image losslessly.
  
  ---