## 1. The Dual Perspectives on Interpolation: Push vs. Pull (Slides 17–19)
  
  Slide 18 highlights an architectural distinction in how interpolation is formulated:
  > **"There are two ways to think about interpolation:**
  > 1. *We 'un-discretize' an image by filling in the gaps between the values we are given.*
  > 2. *We use the same filter kernel to compute a weighted sum of nearby pixels.*
  > 
  > *Instead of convolving each pixel (the inputs), we use the filter at each 'output location' to compute the contribution of each input. These are mathematically equivalent. You'll be using #2 in your assignment."*
  
  ```
   Perspective 1: Forward Mapping ("Push" / Splatting)
   Each input sample spreads its energy into continuous space via h(x):
   
   Sample f[n] ──► [ Center h(x - n) at n ] ──► Accumulate onto output canvas
   
   ────────────────────────────────────────────────────────────────────────
   Perspective 2: Backward Mapping ("Pull" / Query-Based Sampling)
   For a desired non-integer query point x_q, evaluate kernel weights on nearby samples:
   
   Query Coordinate x_q ──► [ Place h centered at x_q ] ──► Weighted sum of neighbors:
                                                            f̃(x_q) = Σ w_k · f[n_k]
  ```
  
  ---
### A. Mathematical Equivalence of Push and Pull
  Recall the continuous reconstruction equation from **Part 1**:
  
  $$\tilde{f}(x) = \sum_{n=-\infty}^\infty f[n] \cdot h(x - n)$$
  
  * **The "Push" View (Forward Convolution):**
  We treat each discrete sample $f[n]$ as an active source. We center the continuous kernel $h$ at integer coordinate $n$ and project its curve outward across $x$.
  * **The "Pull" View (Backward Query / Slide 18):**
  We choose a target query point $x_q \in \mathbb{R}$. We center the kernel directly at $x_q$:
  
  $$w_n = h(x_q - n)$$
  
  Because all standard reconstruction kernels are **spatially symmetric** around the origin ($h(-u) = h(u)$):
  
  $$h(x_q - n) = h\big(-(n - x_q)\big) = h(n - x_q)$$
  
  Evaluating a kernel centered at input $n$ at point $x_q$ is mathematically identical to evaluating an inverted kernel centered at $x_q$ at sample point $n$.
  
  > **Why the Assignment Uses Method #2 ("Pull"):**
  > In digital image processing, our output is a discrete array of memory buffers. We iterate over every target pixel in the output image and **pull** (sample) the interpolated intensity from continuous coordinates in the source image. This guarantees that **every output pixel receives exactly one calculated value**, avoiding the holes, cracks, and overlaps that occur when "pushing" forward.
  
  ---
### B. Worked Example: Evaluating a 1D Query Point (Slide 19)
  Slide 19 walks through a numerical example of querying a non-integer point using the **triangular kernel $\Lambda(x)$**:
  
  ```
                                 Target Query: x_q = 5.25
                                            │
               Integer Grid:                │
      ... ───┬──────────────┬───────────────▼──────────────┬──────────────┬─── ...
             4              5                            6              7
                            │◄── d₅ = 0.25 ─►│◄── d₆ = 0.75──►│
  ```
  
  Let the target coordinate be $x_q = 5.25$. The relevant neighbors for a triangle filter of support $[-1, 1]$ are the integer floors and ceilings: $n = 5$ and $n = 6$.
  
  1. **Distance to Left Neighbor ($n = 5$):**
  
  $$d_5 = |5.25 - 5| = 0.25 \implies w_5 = \Lambda(0.25) = 1.0 - 0.25 = \mathbf{0.75}$$
  
  2. **Distance to Right Neighbor ($n = 6$):**
  
  $$d_6 = |5.25 - 6| = 0.75 \implies w_6 = \Lambda(0.75) = 1.0 - 0.75 = \mathbf{0.25}$$
  
  3. **Inner Product (Weighted Sum):**
  
  $$\tilde{f}(5.25) = w_5 \cdot f[5] + w_6 \cdot f[6] = 0.75 \cdot f[5] + 0.25 \cdot f[6]$$
  
  * **Normalization Check (Partition of Unity):**
  
  $$\sum_k w_k = 0.75 + 0.25 = 1.0$$
  
  The weights sum to unity, ensuring that local brightness is preserved without bias.
  
  ---
## 2. 2D Interpolation Mechanics: Bilinear & Bicubic (Slides 20–21)
  
  Slide 20 introduces the extension of interpolation to two dimensions:
  > **"For now, we can think of 2D interpolation as 2 1D interpolations:**
  > *We can interpolate first along one axis, and then along the other.*
  > *Alternatively, you can use a single 2D kernel to represent the 2D operation with a 'single application.'"*
  
  ---
### A. Bilinear Interpolation: Mathematical Derivation
  Suppose we want to estimate the intensity at an arbitrary continuous coordinate $(x, y)$ situated inside a unit grid cell defined by four integer pixel corners:
  
  ```
        (x₀, y₀+1) = f₀₁                         (x₀+1, y₀+1) = f₁₁
              ┌─────────────────────────────────────────┐
              │                                         │
              │                                         │
              │            • (x, y)                     │  1 - β
              │            Intensity f(x, y) = ?        │
              │                                         │
              │                                         │
              └─────────────────────────────────────────┘
        (x₀, y₀) = f₀₀   │◄────── α ──────►│     (x₀+1, y₀) = f₁₀
                         │◄────────── 1.0 ─────────────►│
  ```
  
  Let:
  * $x_0 = \lfloor x \rfloor, \quad y_0 = \lfloor y \rfloor$ (integer indices of the top-left or bottom-left corner).
  * $\alpha = x - x_0 \in [0, 1)$ (fractional horizontal offset).
  * $\beta = y - y_0 \in [0, 1)$ (fractional vertical offset).
#### Method 1: The 2-Pass 1D Approach (Separable Evaluation)
  1. **Horizontal Interpolation (along rows):**
   Interpolate along the bottom row between $f_{00}$ and $f_{10}$:
  
  $$f(x, y_0) = (1 - \alpha) \cdot f_{00} + \alpha \cdot f_{10}$$
  
   Interpolate along the top row between $f_{01}$ and $f_{11}$:
  
  $$f(x, y_0 + 1) = (1 - \alpha) \cdot f_{01} + \alpha \cdot f_{11}$$
  
  2. **Vertical Interpolation (along columns):**
   Interpolate vertically between the two intermediate horizontal results using fractional weight $\beta$:
  
  $$f(x, y) = (1 - \beta) \cdot f(x, y_0) + \beta \cdot f(x, y_0 + 1)$$
  
  ---
#### Method 2: The Unified 2D Tensor Formulation
  Substituting the horizontal equations directly into the vertical equation yields the single-pass 2D bilinear formula:
  
  $$f(x, y) = (1 - \alpha)(1 - \beta) \cdot f_{00} + \alpha(1 - \beta) \cdot f_{10} + (1 - \alpha)\beta \cdot f_{01} + \alpha\beta \cdot f_{11}$$
  
  * **Geometric Interpretation (Area-Weighted Averages):**
  Notice that the weight assigned to each corner pixel corresponds to the **area of the diagonally opposite rectangle** within the unit cell:
  
  ```
                  ┌──────────────┬────────┐ ^
                  │   Area:      │ Area:  │ │ β
                  │ (1-α)·β      │ α·β    │ │
                  │ (Weights f₀₁)│(Wts f₁₁)│ v
                  ├──────────────┼────────┤
                  │   Area:      │ Area:  │ ^
                  │ (1-α)·(1-β)  │ α·(1-β)│ │ 1 - β
                  │ (Weights f₀₀)│(Wts f₁₀)│ │
                  └──────────────┴────────┘ v
                  │◄── 1 - α ───►│◄─ α ──►│
  ```
#### The Polynomial Nature of Bilinear Interpolation:
  Expanding the terms into standard polynomial form:
  
  $$f(x, y) = a_0 + a_1 x + a_2 y + a_3 x y$$
  
  * Along lines parallel to the coordinate axes ($x = \text{const}$ or $y = \text{const}$), the cross-term $xy$ collapses to a linear function, meaning the surface is **piecewise linear along rows and columns**.
  * Along diagonal directions, the product $xy$ introduces quadratic curvature. Geometrically, the interpolated surface within each grid cell forms a **hyperbolic paraboloid** (a smooth saddle surface).
  
  ---
### B. Bicubic Interpolation in 2D
  For higher visual fidelity, **Bicubic Interpolation** expands the spatial support window to a $4 \times 4$ neighborhood ($16$ neighboring pixels):
  
  $$f(x, y) = \sum_{i=-1}^2 \sum_{j=-1}^2 f\big(x_0 + i, \, y_0 + j\big) \cdot h_{\text{cubic}}(x - (x_0 + i)) \cdot h_{\text{cubic}}(y - (y_0 + j))$$
  
  where $h_{\text{cubic}}$ is Keys' cubic convolution kernel from Part 1 ($a = -0.5$).
  
  ```
        ┌─────┬─────┬─────┬─────┐
        │ P₋₁₂│ P₀₂ │ P₁₂ │ P₂₂ │   Row 2
        ├─────┼─────┼─────┼─────┤
        │ P₋₁₁│ P₀₁ │ P₁₁ │ P₂₁ │   Row 1
        ├─────┼─────┼─────┼─────┤
        │ P₋₁₀│ P₀₀ │ P₁₀ │ P₂₀ │   Row 0      • Target (x, y) evaluated across
        ├─────┼─────┼─────┼─────┤                ALL 16 sample points!
        │ P₋₁₋│ P₀₋ │ P₁₋ │ P₂₋ │   Row -1
        └─────┴─────┴─────┴─────┘
         Col-1 Col 0 Col 1 Col 2
  ```
  
  * **Separable Execution:**
  1. Evaluate $4$ horizontal 1D cubic interpolations (one for each row), yielding $4$ vertical intermediate values.
  2. Evaluate $1$ vertical 1D cubic interpolation across those $4$ intermediate values.
  * **Result:** Reconstructs continuous surface gradients ($C^1$ continuity), preventing the visible slope creases characteristic of bilinear interpolation.
  
  ---
## 3. Frequency-Domain Upsampling: Spectral Zero-Padding (Slide 22)
  
  Slide 22 outlines an alternative method for image upsampling using the **2D Discrete Fourier Transform (DFT)**:
  
  ```
                Frequency-Domain (Spectral) Upsampling Pipeline
                
   Small Image (M × N) ──► [ 2D Fast Fourier Transform (FFT) ]
                                          │
                                          ▼
                             [ Center Spectrum (fftshift) ]
                                          │
                                          ▼
                      [ Zero-Pad High Frequencies to M' × N' ]
                                          │
                                          ▼
                            [ Inverse Center (ifftshift) ]
                                          │
                                          ▼
                        [ 2D Inverse FFT + Normalization ] ──► Large Image (M' × N')
  ```
  
  ---
### A. Mathematical Justification: Sinc Interpolation via the DFT
  Recall that convolution in the spatial domain corresponds to point-wise multiplication in the frequency domain:
  
  $$f(x) * \text{sinc}(x) \iff F(\omega) \cdot \text{rect}\left(\frac{\omega}{2\omega_c}\right)$$
  
  In the spatial domain, true continuous sinc interpolation is computationally expensive because the sinc function has infinite spatial extent. 
  In the frequency domain, however, multiplying by a rectangular window $\text{rect}(\omega)$ is simple: it corresponds to **padding the spectrum with zeros**!
  
  ```
     Original Spectrum F[u, v] (M × N)         Zero-Padded Spectrum F_padded[u, v] (M' × N')
     
             ┌──────────────┐                        ┌──────────────────────────────┐
             │              │                        │000000000000000000000000000000│
             │ High Freqs   │                        │000000000000000000000000000000│
             │     ╭──╮     │       Zero-Pad         │0000   ┌──────────────┐   0000│
             │     │DC│     │    High Frequencies    │0000   │  Preserved   │   0000│
             │     ╰──╯     │   ─────────────────►   │0000   │  Baseband    │   0000│
             │ Low Freqs    │                        │0000   │  Spectrum    │   0000│
             │              │                        │0000   └──────────────┘   0000│
             └──────────────┘                        │000000000000000000000000000000│
                 Centered                                └──────────────────────────────┘
  ```
  
  1. We take an $M \times N$ discrete image and compute its 2D DFT.
  2. We shift the zero-frequency DC component to the center of the matrix (`fftshift`).
  3. We embed this $M \times N$ spectrum into the center of a larger $M' \times N'$ matrix, filling all newly created high-frequency bins with **zeros**.
  4. Taking the Inverse 2D DFT transforms the signal back to the spatial domain with dimensions $M' \times N'$.
  
  ---
### B. Implementation Nuances: Centering and Normalization (Slide 22)
  1. **The Energy Scaling Factor:**
   When transforming an $M \times N$ array into an $M' \times N'$ array via IDFT, the total energy must be preserved. The output spatial array must be scaled by the ratio of the areas:
  
  $$\text{Scale Factor} = \frac{M' \cdot N'}{M \cdot N}$$
  
  2. **Boundary Ringing (Gibbs Phenomenon):**
   The Discrete Fourier Transform inherently assumes that the input image is **spatially periodic** (infinitely tiled across space). If the left edge of an image has a different intensity than the right edge, this creates a steep step discontinuity across the periodic boundary, causing **high-frequency ringing ripples** to propagate into the image interior.
  
  ---
## 4. Python Implementation: Bilinear Interpolation vs. Fourier Upsampling
  
  ```python
  import numpy as np
  
  def bilinear_interpolate_2d(image: np.ndarray, target_shape: tuple) -> np.ndarray:
    """
    Vectorized Pull-Based Bilinear Interpolation.
    Maps an input image to arbitrary target_shape (H_out, W_out).
    """
    H_in, W_in = image.shape
    H_out, W_out = target_shape
    
    # 1. Generate normalized continuous coordinate grids for output image
    # Aligning coordinate spaces such that corner pixel centers match
    scale_y = (H_in - 1) / (H_out - 1) if H_out > 1 else 0
    scale_x = (W_in - 1) / (W_out - 1) if W_out > 1 else 0
    
    out_y, out_x = np.indices((H_out, W_out), dtype=np.float32)
    
    # Map output grid coordinates back to continuous source coordinates (Pull)
    src_y = out_y * scale_y
    src_x = out_x * scale_x
    
    # 2. Extract integer bounds and fractional offsets
    y0 = np.floor(src_y).astype(np.int32)
    x0 = np.floor(src_x).astype(np.int32)
    y1 = np.minimum(y0 + 1, H_in - 1)
    x1 = np.minimum(x0 + 1, W_in - 1)
    
    alpha = src_x - x0
    beta = src_y - y0
    
    # 3. Pull 4 neighbor corner values
    f00 = image[y0, x0]
    f10 = image[y0, x1]
    f01 = image[y1, x0]
    f11 = image[y1, x1]
    
    # 4. Evaluate 2D tensor product equation
    interpolated = (1.0 - alpha) * (1.0 - beta) * f00 + \
                   alpha * (1.0 - beta) * f10 + \
                   (1.0 - alpha) * beta * f01 + \
                   alpha * beta * f11
                   
    return interpolated
  
  def fourier_upsample_2d(image: np.ndarray, target_shape: tuple) -> np.ndarray:
    """
    Spectral Zero-Padding Upsampling via 2D Fast Fourier Transform.
    Equivalent to continuous Whittaker-Shannon sinc interpolation.
    """
    H_in, W_in = image.shape
    H_out, W_out = target_shape
    
    # Step 1: Forward 2D FFT & shift DC to center
    F = np.fft.fftshift(np.fft.fft2(image))
    
    # Step 2: Allocate zero-padded frequency matrix
    F_padded = np.zeros((H_out, W_out), dtype=complex)
    
    # Compute center offsets
    start_y = (H_out - H_in) // 2
    start_x = (W_out - W_in) // 2
    
    # Step 3: Insert original baseband spectrum into the center
    F_padded[start_y : start_y + H_in, start_x : start_x + W_in] = F
    
    # Step 4: Inverse shift, Inverse 2D FFT, and energy normalization
    F_unshifted = np.fft.ifftshift(F_padded)
    upsampled = np.fft.ifft2(F_unshifted)
    
    scale_factor = (H_out * W_out) / (H_in * W_in)
    return np.real(upsampled) * scale_factor
  ```
  
  ---
## 5. The Transition to Geometric Image Warping (Slides 21, 23)
  
  Slide 21 and 23 connect interpolation to general spatial transformations:
  > **"Note: for image warping (a generalization of this idea), when we rotate and stretch and distort the image, we will not be able to easily 'handle' each dimension separately."**
  > 
  > *Slide 23 displays a photograph of a bumblebee rotated, sheared, and perspective-projected onto an arbitrary quadrilateral plane.*
  
  ```
       Standard Upsampling / Resizing                   General Image Warping (Slide 23)
       ──────────────────────────────                   ────────────────────────────────
       • Target coordinates remain aligned              • Coordinate axes are coupled:
         with Cartesian rows and columns:                 x' = f(x, y),   y' = g(x, y)
         x_src = s_x · x_dst                            • Non-separable; requires 2D continuous
         y_src = s_y · y_dst                              query evaluation at skewed coordinates.
       • Separable 2-pass 1D interpolation works.       • Must use inverse warping to prevent holes!
  ```
  
  ---
### A. Why Separable Interpolation Fails Under General Warping
  When simply resizing an image, horizontal coordinate mapping is independent of vertical coordinate mapping:
  
  $$\begin{pmatrix} x_{\text{src}} \\ y_{\text{src}} \end{pmatrix} = \begin{bmatrix} s_x & 0 \\ 0 & s_y \end{bmatrix} \begin{pmatrix} x_{\text{dst}} \\ y_{\text{dst}} \end{pmatrix}$$
  
  Because the transformation matrix is diagonal, we can interpolate horizontally across rows, then vertically across columns.
  
  In general **geometric warping** (such as rotation by angle $\theta$):
  
  $$\begin{pmatrix} x_{\text{src}} \\ y_{\text{src}} \end{pmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix} \begin{pmatrix} x_{\text{dst}} \\ y_{\text{dst}} \end{pmatrix}$$
  
  * The input coordinates $x_{\text{src}}$ and $y_{\text{src}}$ depend on **both** target coordinates $x_{\text{dst}}$ and $y_{\text{dst}}$.
  * The source coordinates no longer align with horizontal or vertical pixel scanlines. 
  * Consequently, we cannot separate the operation into independent 1D passes. We must evaluate **true 2D continuous query interpolation** (Method #2 / Pull) directly at arbitrary floating-point coordinates $(x_{\text{src}}, y_{\text{src}})$.
  
  ---
### B. Preview: Forward Warping (Splatting) vs. Backward Warping (Inverse Mapping)
  
  ```
        Forward Warping (Push)                          Backward Warping (Pull)
     ┌───────────────────────────┐                   ┌───────────────────────────┐
     │ •   •   •   •   •   •   • │                   │ •   •   •   •   •   •   • │
     │   •   HOLE   •     •      │                   │ •   •   •   •   •   •   • │  Every pixel
     │ •   •   •   OVERLAP •   • │                   │ •   •   •   •   •   •   • │  is queried
     └───────────────────────────┘                   └───────────────────────────┘  via interpolation!
      Prone to holes & aliasing                       Guaranteed complete, gap-free
  ```
  
  1. **Forward Mapping (Push):**
   * Maps each input pixel $(x, y)$ to an output position $(x', y') = \mathbf{T}(x, y)$.
   * When an image is enlarged or stretched, rounding $(x', y')$ to the nearest integer leaves **empty gaps (holes)** where no source pixels land.
  2. **Backward Mapping (Pull / Inverse Warping):**
   * Iterates through every discrete coordinate $(x', y')$ of the **output canvas**.
   * Applies the inverse transformation to determine the continuous source coordinate:
  
  $$(x, y) = \mathbf{T}^{-1}(x', y')$$
  
   * Evaluates the continuous image function at $(x, y)$ using **Bilinear or Bicubic interpolation**.
   * **Result:** Every output pixel is populated without holes or gaps.
  
  ---
## Complete Module Summary: M2.2 Image Upsampling
  
  1. **Pixel replication is insufficient:** Repeating rows and columns creates $C^{-1}$ jump discontinuities and visible checkerboard blocking artifacts.
  2. **Upsampling is continuous reconstruction followed by resampling:** Modeling discrete samples as a Dirac impulse train convolved with a continuous kernel $h(x)$ enables evaluation at arbitrary non-integer coordinates.
  3. **The Four Cardinal Kernels provide distinct trade-offs:**
   * **Box ($\Pi$):** Nearest-neighbor; discontinuous, fast, blocky.
   * **Triangle ($\Lambda$):** Bilinear; $C^0$ continuous, attenuates high-frequency sharpness.
   * **Keys Cubic:** Bicubic; $C^1$ continuous with negative sidelobes for edge preservation.
   * **Sinc:** Theoretically ideal low-pass brick wall ($\text{rect}(\omega)$), but practically unusable in spatial domains due to infinite support and Gibbs ringing.
  4. **Pull-based evaluation is the standard:** Modern systems place the reconstruction kernel at target query coordinates and compute weighted sums of adjacent source samples, ensuring partition of unity ($\sum w_k = 1.0$).
  5. **Bilinear interpolation forms a hyperbolic paraboloid:** The 2D tensor formulation combines four area-weighted corners, maintaining linearity along scanlines while curving quadratically along diagonals.
  6. **Interpolation is the core engine of image warping:** Non-rigid geometric transformations (rotations, shears, homographies) couple spatial axes, requiring pull-based continuous 2D interpolation to generate gap-free transformed imagery.
  
  ---