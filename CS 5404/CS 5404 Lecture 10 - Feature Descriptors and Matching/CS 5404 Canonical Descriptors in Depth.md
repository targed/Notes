## 1. The Canonical Orientation Principle (Slide 28)
  
  In **Part 1**, we saw that while spatial grid histograms preserve local geometry, they are sensitive to in-plane image rotation. If an object rotates by $90^\circ$, its spatial cells shift indices, breaking vector comparison.
  
  Slide 28 introduces the standard geometric remedy:
  > **The Canonical Orientation Principle:**
  > Instead of describing a patch relative to the arbitrary horizontal and vertical axes of the camera sensor, we **assign an intrinsic canonical orientation $\theta^*$ to the keypoint based on its local gradient distribution**, and describe the patch relative to $\theta^*$.
  
  ```
     Patch in Arbitrary Orientation                     Canonical Rotated Frame
  ┌───────────────────────────┐                      ┌───────────────────────────┐
  │              ^ θ*         │                      │             ^ y_canonical │
  │             /             │     Rotate Patch     │             │             │
  │            / Dominant     │    by -θ* around     │             │             │
  │           /  Gradient     │       Keypoint       │             •────────►    │
  │          •   Direction    │   ──────────────►    │             (x, y)    x_canonical
  │                           │                      │                           │
  │                           │                      │   Dominant gradient now   │
  └───────────────────────────┘                      │   points strictly UPWARD! │
                                                     └───────────────────────────┘
  ```
  
  ---
### A. How Canonical Orientation $\theta^*$ is Computed (Slide 28)
  Let $\mathbf{p} = (x_0, y_0)$ be a detected keypoint with characteristic scale $\sigma$.
  1. **Compute Local Gradients:** 
   Evaluate spatial image derivatives $I_x, I_y$ within a Gaussian-weighted neighborhood centered at $\mathbf{p}$:
  
  $$m(x, y) = \sqrt{I_x(x, y)^2 + I_y(x, y)^2}$$
  
  $$\theta(x, y) = \text{arctan2}\big(I_y(x, y), \, I_x(x, y)\big) \in [-\pi, \pi]$$
  
  2. **Accumulate Orientation Histogram:** 
   Form a 36-bin orientation histogram covering the full $360^\circ$ circle ($10^\circ$ per bin). Each pixel $(x, y)$ in the neighborhood votes into its corresponding angular bin.
   * **Vote Weight:** The vote is scaled by both the pixel's **gradient magnitude** $m(x, y)$ and a **Gaussian spatial falloff** centered on the keypoint:
  
  $$w(x, y) = m(x, y) \cdot \exp\left( -\frac{(x - x_0)^2 + (y - y_0)^2}{2 (1.5 \sigma)^2} \right)$$
  
  3. **Peak Detection:**
   The highest peak in the orientation histogram defines the **canonical orientation $\theta^*$**.
   * *Multi-Orientation Handling:* If another local histogram peak reaches at least $80\%$ of the maximum peak's height, an additional, distinct keypoint is created at the exact same location and scale with this secondary orientation.
  
  ---
## 2. Multi-Scale Oriented Patches (MOPS) (Slides 29–30, 40–43)
  
  Introduced by **Matthew Brown, Richard Szeliski, and Simon Winder (CVPR 2005)** [2], **Multi-Scale Oriented Patches (MOPS)** provides a normalized intensity-based descriptor designed for panorama stitching.
  
  ```
                         The MOPS Feature Extraction Pipeline (Slide 29)
                         
   1. Sample 40×40 Patch at       2. Rotate Patch by -θ* to      3. Pre-Filter & Downsample
      Detected Pyramid Level         Align to Canonical Axis        to 8×8 Matrix (64 pixels)
      ┌───────────────────────┐      ┌───────────────────────┐      ┌───┬───┬───┬───┐
      │       /───────/       │      │ ┌───────────────────┐ │      │   │   │   │   │
      │      / Patch /        │ ───► │ │  Rotated Upright  │ │ ───► ├───┼───┼───┼───┤
      │     /_______/         │      │ │  40×40 Grid       │ │      │   │   │   │   │
      └───────────────────────┘      │ └───────────────────┘ │      └───┴───┴───┴───┘
                                     └───────────────────────┘              │
                                                                            ▼
                                                               4. Intensity Normalization:
                                                                  z = (I - μ) / σ
                                                                  Output: 64D Vector!
  ```
  
  ---
### Step-by-Step MOPS Pipeline (Slide 29):
  1. **Multi-Scale Detection:** 
   Detect multi-scale Harris corners across an octave-spaced image pyramid.
  2. **Region Extraction:** 
   Extract a large physical window of **$40 \times 40\text{ pixels}$** centered on the keypoint at the pyramid level where it was detected.
  3. **Canonical Rotation Alignment:** 
   Rotate the patch by $-\theta^*$ using bilinear inverse warping so that its dominant local gradient aligns with the horizontal axis.
  4. **Anti-Aliasing & Decimation (Downsampling):** 
   Convolve the $40 \times 40$ rotated patch with a Gaussian smoothing filter and downsample it by a factor of $5$, producing a compact **$8 \times 8\text{ pixel patch}$**.
   * *Why downsample to $8 \times 8$?* Discarding high-frequency details provides tolerance to small non-rigid surface deformations and localization errors while reducing dimensionality from $1600$ values down to $64$.
  5. **Photometric Bias and Gain Normalization (Z-Score Normalization):** 
   To achieve invariance to affine illumination changes ($I' = aI + b$), normalize the 64 intensity values:
  
  $$\mu = \frac{1}{64} \sum_{i=1}^{64} I_i, \quad \sigma = \sqrt{\frac{1}{64} \sum_{i=1}^{64} (I_i - \mu)^2}$$
  
  $$\tilde{I}_i = \frac{I_i - \mu}{\sigma}$$
  
   * Subtracting the mean $\mu$ cancels out additive brightness shifts ($+b$).
   * Dividing by the standard deviation $\sigma$ cancels out multiplicative contrast gain ($\times a$).
  6. **Final Descriptor Output:** 
   The flattened $8 \times 8$ normalized array yields a **$64\text{-dimensional}$ real-valued vector** $\mathbf{f}_{\text{MOPS}} \in \mathbb{R}^{64}$.
  
  ---
## 3. SIFT: The Scale-Invariant Feature Transform (Slides 31–36)
  
  Developed by **David G. Lowe (2004)** [1], **SIFT** is one of the most widely used local feature descriptors in classical computer vision. 
  
  Slide 32 summarizes its core architecture:
  > **The SIFT Descriptor:**
  > Takes a $16 \times 16$ pixel region surrounding a detected keypoint, divides it into a $4 \times 4$ grid of spatial cells, and computes an 8-bin gradient orientation histogram within each cell:
  > 
  > $$\text{Total Dimensionality} = 4 \times 4 \text{ cells} \times 8 \text{ orientation bins} = \mathbf{128\text{ dimensions}}$$
  
  ```
                       The Complete SIFT Descriptor Pipeline
                       
        16×16 Local Gradient Patch                          4×4 Cell Grid Representation
     ┌───────────────────────────────┐                    ┌───────┬───────┬───────┬───────┐
     │ ↗   ↑   ↖   ↑   ↗   →   ↘   ↓ │                    │   *   │   *   │   *   │   *   │
     │ →   ↗   ↑   ↑   ↗   →   ↘   ↓ │                    ├───────┼───────┼───────┼───────┤
     │ →   →   ↗   ↑   ↑   ↗   →   ↘ │                    │   *   │   *   │   *   │   *   │
     │ ↘   →   →   ↗   ↑   ↗   →   → │   ────────►        ├───────┼───────┼───────┼───────┤
     │ ↓   ↘   →   →   •   ↗   →   ↗ │                    │   *   │   *   │   *   │   *   │
     │ ↙   ↓   ↘   →   →   ↗   ↑   ↑ │                    ├───────┼───────┼───────┼───────┤
     │ ←   ↙   ↓   ↘   →   →   ↗   ↑ │                    │   *   │   *   │   *   │   *   │
     │ ↖   ←   ↙   ↓   ↘   →   →   ↗ │                    └───────┴───────┴───────┴───────┘
     └───────────────────────────────┘                    Each cell accumulates an 8-bin
     Rotated relative to canonical θ*                     orientation histogram (star plots)
     Weighted by circular Gaussian window                 4 × 4 × 8 = 128-element vector!
  ```
  
  ---
### Step-by-Step SIFT Descriptor Derivation (Slides 33–36):
#### Step 1: Scale-Dependent Region Selection (Slide 33, 36)
  Unlike MOPS (which extracts a fixed $40 \times 40$ window at an image pyramid octave), SIFT defines its sampling footprint directly in continuous scale-space.
  * The physical width of the window is chosen proportional to the keypoint's detected scale $\sigma$:
  
  $$\text{Window Width} = 16 \cdot \sigma$$
  
  * This guarantees that a feature covering $20\text{ pixels}$ on a distant object and $80\text{ pixels}$ on a close-up object sample the **exact same physical 3D surface area**.
  
  ---
#### Step 2: Canonical Rotation Alignment (Slide 33, 36)
  To achieve rotation invariance, coordinate positions $(x, y)$ and individual gradient orientations $\theta(x, y)$ are rotated relative to the keypoint's dominant orientation $\theta^*$:
  
  $$\begin{bmatrix} x_{\text{rot}} \\ y_{\text{rot}} \end{bmatrix} = \begin{bmatrix} \cos\theta^* & \sin\theta^* \\ -\sin\theta^* & \cos\theta^* \end{bmatrix} \begin{bmatrix} x - x_0 \\ y - y_0 \end{bmatrix}$$
  
  $$\theta_{\text{canonical}}(x, y) = \theta(x, y) - \theta^*$$
  
  ---
#### Step 3: Gradient Evaluation & Gaussian Spatial Weighting (Slide 33)
  At every sample point in the rotated patch:
  1. Compute the local gradient magnitude $m(x, y)$ and relative orientation $\theta_{\text{canonical}}(x, y)$.
  2. Apply an isotropic Gaussian weighting function with standard deviation $\sigma_w = 0.5 \times \text{window width} = 8\sigma$ centered on the keypoint:
  
  $$w_{\text{spatial}}(x, y) = m(x, y) \cdot \exp\left( -\frac{x_{\text{rot}}^2 + y_{\text{rot}}^2}{2 (8\sigma)^2} \right)$$
  
  * **Why Gaussian Weighting is Critical:** Gradients located near the outer perimeter of the patch are assigned smaller weights. This prevents abrupt changes in the descriptor if the patch shifts slightly, while emphasizing stable gradients near the keypoint center.
  
  ---
#### Step 4: Spatial Sub-division into $4 \times 4$ Cells (Slide 33)
  The $16 \times 16$ scale-normalized support region is partitioned into a regular **$4 \times 4$ grid of spatial sub-regions (cells)**. Each cell has an area of $4\sigma \times 4\sigma$.
  
  ---
#### Step 5: Constructing 8-Bin Orientation Histograms (Slide 34)
  Within each of the 16 cells, an orientation histogram with **8 angular bins** is formed.
  * Each bin covers an arc of $\frac{360^\circ}{8} = 45^\circ$ ($0^\circ, 45^\circ, 90^\circ, 135^\circ, 180^\circ, 225^\circ, 270^\circ, 315^\circ$).
  * Every sample point contributes its Gaussian-weighted gradient magnitude $w_{\text{spatial}}(x, y)$ to its respective angular bin.
#### Trilinear Interpolation (Soft Binning):
  To prevent boundary artifacts (where a gradient near a cell edge or bin boundary abruptly switches bins under small perturbations), SIFT uses **trilinear interpolation**:
  * A gradient sample distributes its weight across the **$2 \times 2$ adjacent spatial cells** and the **$2$ adjacent orientation bins** using linear distance interpolation weights:
  
  $$\text{Weight} \propto (1 - d_x)(1 - d_y)(1 - d_\theta)$$
  
  ---
#### Step 6: Descriptor Assembly & Vector Normalization (Slide 34)
  Concatenating the 16 8-bin histograms yields a 128-dimensional raw vector:
  
  $$\mathbf{v} = \begin{bmatrix} \mathbf{h}_{1}^T & \mathbf{h}_{2}^T & \dots & \mathbf{h}_{16}^T \end{bmatrix}^T \in \mathbb{R}^{128}$$
  
  To make the descriptor robust to photometric variations:
  1. **L2 Normalization (Contrast Invariance):**
   Divide by the vector's Euclidean norm:
  
  $$\mathbf{v}' = \frac{\mathbf{v}}{\| \mathbf{v} \|_2} = \frac{\mathbf{v}}{\sqrt{\sum_{i=1}^{128} v_i^2}}$$
  
   * Because contrast scaling multiplies all gradients uniformly ($\nabla (aI) = a \nabla I$), dividing by the L2 norm cancels out $a$, providing **full contrast invariance**.
  2. **Non-Linear Saturation Clamping (Non-Linear Photometric Robustness):**
   Non-linear illumination changes (such as 3D surface specularities or sensor saturation) cause localized spikes in gradient magnitudes without changing surrounding texture.
   * To prevent a few large gradients from dominating the descriptor, **all values in $\mathbf{v}'$ exceeding $0.2$ are clamped to $0.2$**:
  
  $$v''_i = \min(v'_i, \, 0.2)$$
  
  3. **Final Re-Normalization:**
   After clamping, re-normalize the vector to unit length:
  
  $$\mathbf{f}_{\text{SIFT}} = \frac{\mathbf{v}''}{\| \mathbf{v}'' \|_2}$$
  
   The resulting vector $\mathbf{f}_{\text{SIFT}}$ is unit length ($\|\mathbf{f}_{\text{SIFT}}\|_2 = 1.0$) and bounded within $[0.0, 0.2]$.
  
  ---
## 4. Comprehensive Comparison: MOPS vs. SIFT (Slides 40–43)
  
  Slides 40–43 provide an explicit comparison between the two canonical descriptors.
  
  ```
  ┌───────────────────────────┬───────────────────────────────────────────┬───────────────────────────────────────────┐
  │ Evaluation Aspect         │ MOPS (Brown et al., 2005)                 │ SIFT (Lowe, 2004)                         │
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Descriptor Family**     │ **Intensity-Based Patch** Descriptor      │ **Gradient-Based Histogram** Descriptor   │
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Feature Region Size**   │ Fixed **$40 \times 40$ pixels** at octave │ Scale-dependent: **$16\sigma \times 16\sigma$** continuous│
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Data Representation**   │ Downsampled **$8 \times 8$ intensity grid**│ **$4 \times 4$ cells $\times$ 8-bin histograms**         │
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Total Vector Length**   │ **64 Dimensions** ($8 \times 8 = 64$)     │ **128 Dimensions** ($4 \times 4 \times 8 = 128$)          │
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Orientation Alignment** │ Rotates raw patch by dominant angle       │ Rotates coordinates; gradients are relative│
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Illumination Handling** │ Z-score normalize: $(I - \mu) / \sigma$   │ L2 norm + Clamping at $0.2$ + Re-norm     │
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Discriminative Power**  │ Moderate                                  │ **Very High**                             │
  ├───────────────────────────┼───────────────────────────────────────────┼───────────────────────────────────────────┤
  │ **Computational Cost**    │ Faster, simpler                           │ Heavier, but highly optimized             │
  └───────────────────────────┴───────────────────────────────────────────┴───────────────────────────────────────────┘
  ```
  
  ---
### The Fundamental Nuance: "Multi-Scale" vs. "Scale-Invariant" (Slide 43)
  Slide 43 highlights a theoretical distinction:
  > **1. MOPS is Multi-Scale (on the Detection Side):**
  > * MOPS detects features across an image pyramid. It finds features at small, medium, or large octave levels.
  > * *However, MOPS is NOT fully scale-invariant on the descriptor side:* Once a feature is detected, it extracts a fixed $40 \times 40$ patch at that octave. It does not scale the patch to the exact continuous geometric scale $\sigma^*$.
  > 
  > **2. SIFT is Truly Scale-Invariant:**
  > * SIFT detects the continuous sub-octave characteristic scale $\sigma^*$ via Difference-of-Gaussians (DoG).
  > * Its descriptor sampling grid expands and contracts dynamically with $\sigma^*$, meaning the descriptor vector looks identical regardless of zoom level.
  
  ---
## 5. Real-World Performance & Robustness (Slides 37–39)
  
  Slide 37 highlights the empirical performance of SIFT:
  * **Out-of-Plane 3D Viewpoint Tolerance:** Handles perspective viewpoint changes of **up to $\sim 60^\circ$**. Although SIFT is designed for 2D similarity transformations (scale and rotation), its local gradient histograms tolerate the mild affine shear produced by perspective foreshortening.
  * **Photometric Robustness:** Handles day-to-night lighting transitions, diffuse shadows, and sensor gain changes.
  * **Computational Speed:** Highly optimized implementations run in real-time on modern CPUs and GPUs.
  * **Ubiquity:** SIFT serves as the primary visual engine behind large-scale 3D reconstructions, such as **Building Rome in a Day** (Slide 48).
  
  ---
## 6. Python Implementation: Building a SIFT-like 128D Descriptor
  
  The following self-contained NumPy implementation details the steps of SIFT descriptor extraction (canonical rotation, spatial cell binning, and saturation normalization):
  
  ```python
  import numpy as np
  
  def extract_sift_descriptor(image: np.ndarray, 
                            x0: float, 
                            y0: float, 
                            sigma: float, 
                            canonical_theta: float) -> np.ndarray:
    """
    Extracts a 128-dimensional SIFT descriptor at keypoint (x0, y0)
    with scale sigma and canonical orientation canonical_theta (in radians).
    """
    # 1. Configuration parameters
    d_cells = 4          # 4x4 spatial cell grid
    n_bins = 8           # 8 orientation bins per cell
    cell_width = 3.0 * sigma  # Width of each cell in pixels
    window_radius = int(np.round(2.0 * cell_width * np.sqrt(2.0)))
    
    # 2. Extract bounding sub-image with boundary checks
    H, W = image.shape
    x_min = max(0, int(np.floor(x0 - window_radius)))
    x_max = min(W - 1, int(np.ceil(x0 + window_radius)))
    y_min = max(0, int(np.floor(y0 - window_radius)))
    y_max = min(H - 1, int(np.ceil(y0 + window_radius)))
    
    # 3. Compute spatial gradients via central differences
    sub_img = image[y_min : y_max + 1, x_min : x_max + 1].astype(np.float32)
    gy, gx = np.gradient(sub_img)
    mag = np.sqrt(gx**2 + gy**2)
    ori = np.arctan2(gy, gx)  # Radians in [-pi, pi]
    
    # 4. Initialize 128D histogram tensor: shape (4, 4, 8)
    hist_tensor = np.zeros((d_cells, d_cells, n_bins), dtype=np.float32)
    
    cos_t = np.cos(canonical_theta)
    sin_t = np.sin(canonical_theta)
    gaussian_sigma = 0.5 * (d_cells * cell_width)
    
    # 5. Populate cells using rotated relative coordinates
    for dy in range(sub_img.shape[0]):
        for dx in range(sub_img.shape[1]):
            # Absolute spatial offset from keypoint center
            rx = (x_min + dx) - x0
            ry = (y_min + dy) - y0
            
            # Rotate coordinates into canonical orientation frame
            x_rot = (cos_t * rx + sin_t * ry) / cell_width
            y_rot = (-sin_t * rx + cos_t * ry) / cell_width
            
            # Spatial cell indices (offset so center is at 0.0)
            c_x = x_rot + (d_cells / 2.0) - 0.5
            c_y = y_rot + (d_cells / 2.0) - 0.5
            
            # Check if sample lands within the 4x4 cell grid bounds
            if -1.0 < c_x < d_cells and -1.0 < c_y < d_cells:
                # Relative gradient orientation
                theta_rel = ori[dy, dx] - canonical_theta
                # Normalize angle into [0, 2*pi)
                theta_rel = np.mod(theta_rel, 2.0 * np.pi)
                bin_idx = theta_rel / (2.0 * np.pi / n_bins)
                
                # Gaussian spatial weight
                weight = mag[dy, dx] * np.exp(-(rx**2 + ry**2) / (2.0 * gaussian_sigma**2))
                
                # Trilinear interpolation: distribute weight to 8 adjacent bins
                x0_idx, y0_idx, b0_idx = int(np.floor(c_x)), int(np.floor(c_y)), int(np.floor(bin_idx))
                x_frac, y_frac, b_frac = c_x - x0_idx, c_y - y0_idx, bin_idx - b0_idx
                
                for di in [0, 1]:
                    for dj in [0, 1]:
                        for db in [0, 1]:
                            xi = x0_idx + di
                            yj = y0_idx + dj
                            bi = (b0_idx + db) % n_bins
                            
                            if 0 <= xi < d_cells and 0 <= yj < d_cells:
                                w_trilinear = (x_frac if di == 1 else (1.0 - x_frac)) * \
                                              (y_frac if dj == 1 else (1.0 - y_frac)) * \
                                              (b_frac if db == 1 else (1.0 - b_frac))
                                hist_tensor[yj, xi, bi] += weight * w_trilinear
                                
    # 6. Flatten to 128D vector
    descriptor = hist_tensor.flatten()
    
    # 7. Normalization Pipeline (L2 -> Clamp 0.2 -> Re-normalize)
    norm = np.linalg.norm(descriptor)
    if norm > 1e-7:
        descriptor /= norm
        descriptor = np.clip(descriptor, 0.0, 0.2)
        descriptor /= np.linalg.norm(descriptor)
    else:
        descriptor = np.zeros(128, dtype=np.float32)
        
    return descriptor
  ```
  
  ---
## Summary Matrix: The Handcrafted Descriptor Spectrum
  
  | Descriptor | Input Data Type | Cell Layout | Normalization Strategy | Feature Vector Size | Invariance Properties |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | **Raw Pixel Patch** | Intensities | Unstructured | None | $N^2$ (e.g., 256) | None |
  | **Spatial Histogram**| Intensities / Colors | $3 \times 3$ grid | Independent cell sums | $K \times B$ | Small translations |
  | **MOPS** | Gradients & Intensities | Rotated $8 \times 8$ | Z-score: $(I - \mu)/\sigma$ | **64D** | Scale (octaves), Rotation, Affine lighting |
  | **SIFT** | Gradients & Angles | $4 \times 4$ cells $\times$ 8 bins | L2 norm $\to$ Clip 0.2 $\to$ L2 norm | **128D** | **Full Scale, Rotation, Affine lighting, 3D Tilt ($\sim 60^\circ$)** |
  
  ---