## 1. 3D Scale-Space Extrema Detection (Slides 48–50)
  
  In **Part 2**, we proved that multiplying the continuous Laplacian of Gaussian by $\sigma^2$ yields an amplitude-stable response across spatial scales:
  
  $$\text{LoG}_{\text{norm}}(x, y, \sigma) = \sigma^2 \big( -\nabla^2 G_\sigma(x, y) \big)$$
  
  Slide 50 outlines the core procedure for turning this response into discrete, scale-invariant feature coordinates.
  
  ```
                Scale-Space Volume Representation (Slide 50)
                
        Scale σ
          ^
          │       Scale Slice k+1 (σ_{k+1}):   ┌───┬───┬───┐
          │       9 Neighbors Checked Above    │ × │ × │ × │
          │                                    ├───┼───┼───┤
          │                                    │ × │ × │ × │
          │                                    └───┴───┴───┘
          │
          │       Current Scale Slice k (σ_k): ┌───┬───┬───┐
          │       8 Spatial Neighbors          │ × │ × │ × │
          │       + 1 Candidate Point          ├───┼───┼───┤
          │                                    │ × │ • │ × │  <-- Candidate (x, y, σ)
          │                                    ├───┼───┼───┤
          │                                    │ × │ × │ × │
          │                                    └───┴───┴───┘
          │
          │       Scale Slice k-1 (σ_{k-1}):   ┌───┬───┬───┐
          │       9 Neighbors Checked Below    │ × │ × │ × │
          │                                    ├───┼───┼───┤
          │                                    │ × │ × │ × │
          │                                    └───┴───┴───┘
          └────────────────────────────────────────────────────────► (x, y) Image Plane
                   TOTAL COMPARISON: 8 + 9 + 9 = 26 SPATIOTEMPORAL NEIGHBORS!
  ```
  
  ---
### A. The 26-Neighborhood Non-Maximum Suppression (Slide 50)
  In standard 2D corner detection (Module M3.1), Non-Maximum Suppression (NMS) compares a candidate pixel against its $8$ spatial neighbors within a $3 \times 3$ grid.
  
  In **Scale-Space Feature Detection**, the search space expands to three dimensions: $(x, y, \sigma) \in \mathbb{R}^3$.
  * To determine whether a candidate point $(x^*, y^*, \sigma^*)$ is an interest point, its response $F(x^*, y^*, \sigma^*)$ must be a **local extremum** (strictly greater than or strictly less than) all adjacent samples in both space **and** scale.
  * The local neighborhood consists of **$26$ surrounding points**:
  * $8$ spatial neighbors in the current scale slice: $(x^* \pm \{0, 1\}, \, y^* \pm \{0, 1\}, \, \sigma^*)$.
  * $9$ neighbors in the adjacent coarser scale slice: $(x^* \pm \{0, 1\}, \, y^* \pm \{0, 1\}, \, \sigma^* + \Delta \sigma)$.
  * $9$ neighbors in the adjacent finer scale slice: $(x^* \pm \{0, 1\}, \, y^* \pm \{0, 1\}, \, \sigma^* - \Delta \sigma)$.
  
  $$\text{Keypoint at } (x^*, y^*, \sigma^*) \iff \big| F(x^*, y^*, \sigma^*) \big| > \big| F(x, y, \sigma) \big| \quad \forall (x, y, \sigma) \in \mathcal{N}_{26}$$
  
  ---
### B. What an Extremum Represents
  1. **Spatial Extremum in $(x, y)$:** Confirms that the feature is centered on a localized visual structure (blob or vertex) rather than lying along an extended, ambiguous edge.
  2. **Scale Extremum in $\sigma$:** Confirms that the filter's spatial extent matches the feature's natural physical size (its **characteristic scale**), satisfying the $\sigma = r/\sqrt{2}$ resonance condition.
  
  ---
## 2. Visualizing and Interpreting Scale-Aware Blobs (Slides 49, 51–54)
  
  Slide 49 references Tony Lindeberg’s foundational work on scale selection [1]:
  > **Feature Representation:**
  > Each detected scale-space feature is represented as a tuple:
  > 
  > $$\mathbf{k}_i = \big(x_i, \, y_i, \, \sigma_i^*\big)$$
  > 
  > * $(x_i, y_i)$ defines the **spatial center** of the feature.
  > * $\sigma_i^*$ defines the **scale** of the feature.
  
  ```
       Converting Scale Parameter to Geometry:
       
          Detected Keypoint (x*, y*, σ*)           Visual Representation: Circle
          ┌───────────────────────────┐            ┌───────────────────────────┐
          │                           │            │           ╭───╮           │
          │             •             │    ──►     │         ╭─╯   ╰─╮         │
          │        (x*, y*, σ*)       │            │        │  (x*,y*) │ Radius  │
          │                           │            │         ╰─╮   ╭─╯    r    │
          │                           │            │           ╰───╯           │
          └───────────────────────────┘            └───────────────────────────┘
                                                   Radius: r = √2 · σ*
  ```
  
  ---
### A. The Circular Footprint: $r = \sqrt{2}\sigma^*$ (Slide 49)
  To visualize detected features on an image, we plot circles centered at $(x^*, y^*)$. 
  
  What should the circle's radius $r$ be?
  From the zero-crossing derivation in **Part 2**, the maximum response for a circular disk of radius $r$ occurs when:
  
  $$r = \sqrt{2} \cdot \sigma^* \approx 1.414 \cdot \sigma^*$$
  
  Plotting a circle of radius $r = \sqrt{2}\sigma^*$ draws a boundary that directly outlines the physical transition where the feature meets its background (Slide 49).
  
  ---
### B. Case Study: The Butterfly and the Sunflower (Slides 49, 51–54)
  * **The Sunflower (Slide 49):** 
  The dark center disc of the sunflower produces a single, isolated peak in the scale signature curve at $\sigma \approx 4.8$. Plotting a circle with $r = \sqrt{2}(4.8) \approx 6.8\text{ mm}$ bounds the disc.
  * **The Butterfly on Striped Foliage (Slides 51–54):**
  * Slide 52 displays a single 2D scale slice of the LoG response at $\sigma = 11.99$. Only features whose physical size resonates with that specific scale appear bright.
  * Slides 53 and 54 show the full multi-scale detection across all levels:
    * **Large circles** lock onto the broad white wing spots and central body.
    * **Medium circles** outline smaller peripheral wing markings.
    * **Small circles** capture fine texture details along the leaf veins.
  
  ---
## 3. The Computational Bottleneck: Why Convolving with LoG is Expensive (Slide 55)
  
  While scale-normalized LoG provides clean scale-space extrema, Slide 55 highlights an operational barrier:
  > *"Taking the LoG many times over can be expensive. Difference of Gaussians can be used instead of LoG which can be more efficient to evaluate. Save on computation!"*
  
  ```
     The Computational Cost of Scale-Normalized LoG
     
     Scale σ₁:  Image  *  [ LoG Kernel (σ₁) ]  ──► Expensive 2D non-separable convolution
     Scale σ₂:  Image  *  [ LoG Kernel (σ₂) ]  ──► Expensive 2D non-separable convolution
     Scale σ₃:  Image  *  [ LoG Kernel (σ₃) ]  ──► Expensive 2D non-separable convolution
     ... Repeated across 30+ scales!
  ```
  
  1. **Non-Separability of the 2D Laplacian:** 
   While the 2D Gaussian $G(x, y)$ is separable, the Laplacian of Gaussian:
  
  $$\nabla^2 G = \frac{1}{\pi\sigma^4}\left(1 - \frac{x^2+y^2}{2\sigma^2}\right)e^{-\frac{x^2+y^2}{2\sigma^2}}$$
  
   has a matrix rank of 2. It **cannot be factored into two 1D passes** ($u \cdot v^T$). Evaluating large LoG kernels at dozens of fine scale increments requires millions of floating-point operations per frame.
  2. **Redundant Filtering:**
   If we filter an image independently with an LoG kernel at every scale, we discard the intermediate smoothed representations without reusing them.
  
  ---
## 4. The SIFT Breakthrough: Difference of Gaussians (DoG) (Slides 55–56)
  
  In 1999–2004, **David G. Lowe** introduced the **Scale-Invariant Feature Transform (SIFT)** [2], introducing a mathematical approximation that replaced the LoG operator with the **Difference of Gaussians (DoG)**.
  
  ```
       Laplacian of Gaussian (LoG) vs. Difference of Gaussians (DoG) (Slide 55)
       
            ^ Spatial Response Profile
        0.2 │          ╭─╮                ╭─╮
            │        ╭─╯ ╰─╮            ╭─╯ ╰─╮
        0.0 ┼──────╭─╯─────╰─╮────────╭─╯─────╰─╮──────► Radius r
            │    ╭─╯         ╰─╮    ╭─╯         ╰─╮
       -0.2 │  ╭─╯             ╰────╯             ╰─╮
            │ ╭╯                                   ╰╮
       -0.4 ├─╯                                     ╰─
            │   ─── Blue: Scale-Normalized LoG: σ² ∇²G
            │   ─── Red:  Difference of Gaussians: G(kσ) - G(σ)
            └──────────────────────────────────────────────────
            THE TWO PROFILES MATCH ALMOST IDENTICALLY!
  ```
  
  ---
### A. Mathematical Proof: Approximating LoG via the Heat Equation
  Why does subtracting two blurred images approximate the scale-normalized Laplacian?
  
  Recall from **Module M2.1** that Gaussian smoothing corresponds to physical heat diffusion over time $t$, where scale relates to time via $t = \frac{\sigma^2}{2}$:
  
  $$\frac{\partial G}{\partial t} = \nabla^2 G$$
  
  Expressing the derivative with respect to $\sigma$ using the chain rule:
  
  $$\frac{\partial G}{\partial \sigma} = \frac{\partial G}{\partial t} \frac{\partial t}{\partial \sigma} = (\nabla^2 G) \cdot \left( \frac{\partial}{\partial \sigma} \left[\frac{\sigma^2}{2}\right] \right) = \sigma \nabla^2 G$$
  
  Now, consider approximating this derivative using a forward finite difference across a multiplicative scale ratio $k$:
  
  $$\frac{\partial G}{\partial \sigma} = \lim_{k \to 1} \frac{G(x, y, k\sigma) - G(x, y, \sigma)}{k\sigma - \sigma}$$
  
  Equating the continuous derivative to the discrete ratio:
  
  $$\frac{G(x, y, k\sigma) - G(x, y, \sigma)}{(k - 1)\sigma} \approx \sigma \nabla^2 G$$
  
  Multiplying both sides by $(k - 1)\sigma$:
  
  $$G(x, y, k\sigma) - G(x, y, \sigma) \approx (k - 1) \cdot \sigma^2 \nabla^2 G$$
  
  $$\text{DoG}(x, y, \sigma) \equiv G(x, y, k\sigma) - G(x, y, \sigma) \propto \text{LoG}_{\text{norm}}(x, y, \sigma)$$
  
  ---
### B. Why This Result is Critical for Computational Efficiency
  1. **Inherent Scale Normalization:**
   Notice the term $\mathbf{\sigma^2}$ on the right-hand side. 
   Subtracting $G(k\sigma)$ from $G(\sigma)$ **automatically includes the $\sigma^2$ scale-normalization factor**. We do not need to compute or multiply by $\sigma^2$ manually; it emerges naturally from the subtraction!
  2. **Constant Factor Invariance:**
   The term $(k - 1)$ is a fixed constant across all scales. Because constant scaling does not shift the spatial coordinates or scale coordinates of local extrema:
  
  $$\arg\max_{(x, y, \sigma)} \big| \text{DoG}(x, y, \sigma) \big| \equiv \arg\max_{(x, y, \sigma)} \big| \text{LoG}_{\text{norm}}(x, y, \sigma) \big|$$
  
  ---
## 5. The SIFT Scale-Space Architecture: Octaves and Scale Stacks (Slide 56)
  
  Slide 56 displays the complete multi-scale SIFT pyramid architecture:
  
  ```
                                  SIFT Scale-Space Hierarchy
                                  
     Octave 2: (Downsampled by 2× via REDUCE)
     ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐
     │ G_2,1 │ │ G_2,2 │ │ G_2,3 │ │ G_2,4 │ │ G_2,5 │   <-- Blurred Image Stack (Gaussian)
     └───────┘ └───────┘ └───────┘ └───────┘ └───────┘
         │         │         │         │         │
         └───┬─────┴───┬─────┴───┬─────┴───┬─────┘
             ▼         ▼         ▼         ▼
          ┌─────┐   ┌─────┐   ┌─────┐   ┌─────┐
          │DoG₁ │   │DoG₂ │   │DoG₃ │   │DoG₄ │            <-- Subtractions between neighbors
          └─────┘   └─────┘   └─────┘   └─────┘
             │         ▲         ▲         │
             └─────────┼─────────┼─────────┘
                       NMS over 26 neighbors evaluated on middle DoG slices!
     ────────────────────────────────────────────────────────────────────────
     Octave 1: (Full Original Resolution)
     ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌───────────┐
     │   G_1,1   │ │   G_1,2   │ │   G_1,3   │ │   G_1,4   │ │   G_1,5   │
     └───────────┘ └───────────┘ └───────────┘ └───────────┘ └───────────┘
  ```
  
  ---
### A. The Octave Sizing Parameters
  * **Scale Multiplier ($k$):**
  If each octave is divided into $S$ scale intervals, the multiplicative step factor between adjacent Gaussian images is:
  
  $$k = 2^{1/S}$$
  
  For $S = 3$ scale intervals per octave: $k = 2^{1/3} \approx 1.2599$.
  * **Number of Images Required:**
  To detect extrema across $S$ scale levels, the NMS algorithm requires one slice above and one slice below each candidate level. 
  * Detecting extrema across $S$ levels requires **$S + 2$ DoG images**.
  * Generating $S + 2$ DoG images requires **$S + 3$ Gaussian-blurred images** per octave.
  
  ---
### B. Algorithmic Efficiency: Why SIFT is Fast
  1. **Reusable Separable Convolutions:** 
   Gaussian smoothing is 1D separable ($\mathcal{O}(2M)$ per pixel). Each level $G_{k+1}$ is computed simply by applying a small 1D Gaussian blur to the preceding level $G_k$.
  2. **Subtraction Replaces 2D Convolution:**
   Generating the DoG volume requires **no convolutions at all**—it is computed by subtracting one already-blurred image from another:
  
  $$\text{DoG}_k = G_{k+1} - G_k$$
  
   Matrix subtraction requires only a single machine instruction per pixel.
  3. **Decimation by Octave:**
   After completing an octave of scale $2\sigma$, the image is downsampled by factor 2 (using our $\text{REDUCE}$ operator from Module M2.1). The next octave processes only $25\%$ of the pixels, keeping overall memory and runtime bounded.
  
  ---
## 6. Complete Python Implementation: Scale-Space Blob Detector
  
  The following self-contained Python program implements multi-scale blob detection using scale-normalized LoG and 3D Non-Maximum Suppression:
  
  ```python
  import numpy as np
  import scipy.ndimage as ndimage
  
  def detect_scale_space_blobs(image: np.ndarray, 
                             num_scales: int = 10, 
                             sigma_min: float = 1.5, 
                             k_factor: float = 1.25, 
                             threshold: float = 0.03) -> list:
    """
    Detects scale-invariant blobs using Scale-Normalized Laplacian of Gaussian (LoG).
    
    Parameters:
        image: 2D grayscale array normalized to [0.0, 1.0].
        num_scales: Number of continuous scale slices to generate.
        sigma_min: Smallest Gaussian standard deviation.
        k_factor: Multiplicative step factor between adjacent scales (sigma_i = sigma_min * k^i).
        threshold: Absolute response threshold for extremum pruning.
        
    Returns:
        List of detected keypoint tuples: (row, col, characteristic_scale_sigma, physical_radius)
    """
    H, W = image.shape
    
    # 1. Allocate 3D Scale-Space Volume: Shape (num_scales, H, W)
    scale_space = np.zeros((num_scales, H, W), dtype=np.float32)
    sigmas = [sigma_min * (k_factor ** i) for i in range(num_scales)]
    
    # 2. Populate Scale-Space using Scale-Normalized LoG: σ² · (-∇²G)
    for i, sigma in enumerate(sigmas):
        # We can implement -σ² ∇²(I * G) using Gaussian blur + Laplacian
        # Smooth image at scale sigma
        smoothed = ndimage.gaussian_filter(image, sigma=sigma, mode='reflect')
        # Apply standard 2D discrete Laplacian
        laplace = ndimage.laplace(smoothed, mode='reflect')
        # Multiply by -sigma^2 to achieve scale-normalization & positive peak polarity
        scale_space[i] = -(sigma ** 2) * laplace
  
    # 3. 3D Non-Maximum Suppression across 26 spatiotemporal neighbors
    keypoints = []
    
    # Iterate across intermediate scale slices (must have a neighbor above and below)
    for s in range(1, num_scales - 1):
        # Extract 3x3x3 local volumetric neighborhood
        # ndimage.maximum_filter over the 3D volume (size=(3, 3, 3))
        volume_slice = scale_space[s - 1 : s + 2, :, :]
        local_max = ndimage.maximum_filter(volume_slice, size=(3, 3, 3), mode='constant', cval=0.0)
        
        # An extremum must be equal to the 3D maximum AND exceed the threshold
        is_extremum = (scale_space[s] == local_max[1]) & (scale_space[s] > threshold)
        
        # Extract spatial indices
        rows, cols = np.where(is_extremum)
        
        current_sigma = sigmas[s]
        physical_radius = np.sqrt(2.0) * current_sigma
        
        for r, c in zip(rows, cols):
            keypoints.append((r, c, current_sigma, physical_radius))
            
    return keypoints
  ```
  
  ---
## 7. Master Synthesis: Resolving the Scale Challenge (Slide 57)
  
  Slide 57 revisits the opening challenge:
  > **"Summary: We have a tool to handle changes in scale."**
  
  ```
       Original Scale Ambiguity (Slide 3)             Resolved via Scale-Space Selection
     ┌───────────────────────────────┐              ┌───────────────────────────────┐
     │   ? What is the vase size?    │              │   • Vase 1: σ* = 4.2  (Close) │
     │   ? Derivatives fail          │   ───────►   │   • Vase 2: σ* = 1.4  (Far)   │
     │   ? Curvatures fail           │              │   Normalized to 32×32 patches │
     │   ? Harris features fail      │              │   MATCHES PERFECTLY!          │
     └───────────────────────────────┘              └───────────────────────────────┘
  ```
  
  By searching across scale space $(x, y, \sigma)$, our detection framework now handles the full range of geometric transformations:
  
  $$\text{Translation Equivariance } (\checkmark) \quad \text{Rotation Equivariance } (\checkmark) \quad \text{Scale Equivariance } (\mathbf{\checkmark})$$
  
  ---
## Summary Comparison: Local Feature Detectors
  
  | Dimension / Property | Classical Harris (M3.1) | Scale-Normalized LoG (M3.2) | SIFT DoG Detector (M3.2) |
  | :--- | :--- | :--- | :--- |
  | **Feature Primitive** | **Corners** (Intersecting boundaries) | **Blobs** (Regional extrema) | **Blobs / Corners** (Scale-space extrema) |
  | **Mathematical Operator**| $R = \det(\mathbf{H}) - k \cdot \text{tr}(\mathbf{H})^2$ | $\sigma^2 \nabla^2(I * G_\sigma)$ | $D(x, y, \sigma) = G(k\sigma) - G(\sigma)$ |
  | **Scale Mechanism** | Fixed integration scale $\sigma_i$ | Continuous search over $\sigma$ | Multi-scale octave pyramid |
  | **Search Space** | 2D Spatial plane $(x, y)$ | 3D Scale-space $(x, y, \sigma)$ | 3D Octave-space $(x, y, \sigma)$ |
  | **NMS Scope** | 8 Spatial neighbors ($3 \times 3$) | 26 Spatiotemporal neighbors | 26 Spatiotemporal neighbors |
  | **Scale Invariance** | $\times$ **Fails** (Scale-variant) | $\mathbf{\checkmark}$ **Full Scale Equivariance** | $\mathbf{\checkmark}$ **Full Scale Equivariance** |
  | **Computational Cost**| Extremely Fast ($\mathcal{O}(1)$ scale) | Slow (Non-separable 2D LoG) | **Fast (Separable blurs + subtractions)** |
  
  ---