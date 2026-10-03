## 1. The Forward Warping Dilemma ("Push" / Splatting) (Slides 43–48)
  
  Once we have computed a coordinate transformation matrix (e.g., an Affine matrix $\mathbf{A}$ or a Projective Homography $\mathbf{H}$), we must generate the actual warped digital image.
  
  Slide 43 presents the most direct implementation idea:
  > **The Forward Mapping Approach ("Push"):**
  > Iterate over every discrete pixel $(x, y)$ in the source image $f$, apply the forward coordinate transformation $(x', y') = T(x, y)$, and write the pixel's color into the target canvas $g$ at $(x', y')$.
  
  ```
                    Forward Warping Pipeline ("Push")
                    
  Source Image f(x, y) ──► Compute (x', y') = T(x, y) ──► Deposit Color into g(x', y')
  Iterate over INPUT                                      Target grid is PASSIVE
  ```
  
  ---
### A. The Discretization Paradox: Why Forward Warping Fails (Slides 46–47)
  The continuous transformation $T$ maps points in $\mathbb{R}^2$ to points in $\mathbb{R}^2$. However, digital images are stored as **discrete integer lattices** ($\mathbb{Z}^2$). 
  
  Slide 47 presents a 1D example demonstrating the breakdown of forward mapping under magnification:
  * Let the spatial transformation be a simple $2\times$ enlargement: 
  
  $$x' = T(x) = 2x$$
  
  * We iterate through the integer pixels of the source image $x \in \{0, 1, 2, 3\}$:
  
  ```
   Source Image f(x):        x = 0       x = 1       x = 2       x = 3
                             •           •           •           •
                             │           │           │           │
   Forward Map x' = 2x:      ▼           ▼           ▼           ▼
   Target Canvas g(x'):      x' = 0      x' = 2      x' = 4      x' = 6
                             •     ?     •     ?     •     ?     •
                                   ▲           ▲           ▲
                             Target pixels x' = 1, 3, 5 are NEVER visited!
                             THEY REMAIN EMPTY GAPS (HOLES)!
  ```
#### The Pathological Artifacts of Forward Warping:
  1. **Holes and Cracks (Under-sampling / Enlargement — Slides 46–47):**
   When an image is scaled up, rotated, or perspective-stretched, the projected source points spread apart. Because the target canvas is populated discretely, many target pixels receive **zero source samples**, leaving empty black cracks throughout the image.
  2. **Collisions and Overlaps (Over-sampling / Shrinking):**
   When an image is scaled down, multiple distinct source pixels $(x_1, y_1)$ and $(x_2, y_2)$ map to the same target integer coordinate $(x', y')$. The system must decide whether to overwrite pixels or average them, leading to aliasing.
  3. **Fractional Coordinates:**
   Transformed coordinates $(x', y')$ are continuous floating-point numbers (e.g., $(14.28, 91.73)$). Rounding them to the nearest integer ($\lfloor x' + 0.5 \rfloor, \lfloor y' + 0.5 \rfloor$) introduces spatial jitter, aliasing, and moiré fringes.
  
  ---
### B. Forward "Splatting" and Its Limitations (Slides 44, 48)
  Slide 44 and 48 discuss a technique designed to mitigate holes in forward mapping: **Splatting**.
  
  ```
                           The Concept of Splatting
                                          ^ y'
                                          │
                                       ┌──┼──┐
                                       │ ░│░ │  <-- Continuous Kernel Footprint
                                    ───┼──•──┼───► x'  (Centered at non-integer (x', y'))
                                       │ ░│░ │
                                       └──┼──┘
                               Distributed energy accumulates onto adjacent discrete pixels!
  ```
  
  * **The Splatting Mechanism:** Instead of treating each source sample as an infinitesimal point, splatting treats it as a continuous spatial footprint (e.g., a small circular pillbox or Gaussian disk). 
  * When source pixel $(x, y)$ maps to continuous coordinate $(x', y')$, it distributes a fraction of its intensity/color to all neighboring target pixels using a weighting kernel.
  * **Why Splatting is Rarely Used for 2D Image Warping (Slide 48):**
  * It requires maintaining a separate **normalization accumulation buffer** to divide out overlapping weights:
  
  $$g(x', y') = \frac{\sum_i w_i f(x_i, y_i)}{\sum_i w_i}$$
  
  * If a target pixel falls between wide footprints, it still produces a hole (division by zero).
  * It is computationally expensive and introduces blur.
  
  ---
## 2. The Inverse Warping Standard ("Pull" / Backward Mapping) (Slides 49–53)
  
  Slide 49 introduces the standard paradigm used across modern computer vision and computer graphics:
  > **The Inverse Warping Paradigm ("Pull"):**
  > **Do not ask:** *"Where does this original pixel go?"*
  > **Instead, ask:** *"For this target pixel, where should its color come from?"*
  
  ```
                      Inverse Warping Pipeline ("Pull")
                      
   Target Canvas g(x', y') ──► Query Source (x, y) = T⁻¹(x', y') ──► Sample f(x, y) via Interpolation
   Iterate over OUTPUT                                               Guaranteed ZERO holes!
  ```
  
  ---
### A. The Central Equation of Inverse Warping (Slide 49)
  Instead of looping over the source image, we loop systematically through every integer coordinate $(x', y')$ of the **target canvas**. 
  
  Using the **inverse transformation matrix** $T^{-1}$:
  
  $$(x, y) = T^{-1}(x', y')$$
  
  We sample the intensity of the source image $f$ at continuous coordinate $(x, y)$:
  
  $$g(x', y') = f\big(T^{-1}(x', y')\big)$$
  
  ---
### B. Why Inverse Warping is Guaranteed to be Hole-Free (Slide 50)
  Revisiting our $2\times$ scaling example under inverse mapping:
  
  $$x' = 2x \iff x = T^{-1}(x') = \frac{x'}{2}$$
  
  Slide 50 steps through the target coordinates:
  
  ```
  ┌──────────────────┬─────────────────────────────┬────────────────────────────────────────────┐
  │ Target Pixel x'  │ Inverse Source Position x   │ Evaluation Strategy                        │
  ├──────────────────┼─────────────────────────────┼────────────────────────────────────────────┤
  │ $x' = 0$         │ $x = 0.0$                   │ Direct integer sample: $g(0) = f(0)$       │
  ├──────────────────┼─────────────────────────────┼────────────────────────────────────────────┤
  │ $x' = 1$         │ $x = 0.5$                   │ **Interpolate** between $f(0)$ and $f(1)$  │
  ├──────────────────┼─────────────────────────────┼────────────────────────────────────────────┤
  │ $x' = 2$         │ $x = 1.0$                   │ Direct integer sample: $g(2) = f(1)$       │
  ├──────────────────┼─────────────────────────────┼────────────────────────────────────────────┤
  │ $x' = 3$         │ $x = 1.5$                   │ **Interpolate** between $f(1)$ and $f(2)$  │
  ├──────────────────┼─────────────────────────────┼────────────────────────────────────────────┤
  │ $x' = 4$         │ $x = 2.0$                   │ Direct integer sample: $g(4) = f(2)$       │
  └──────────────────┴─────────────────────────────┴────────────────────────────────────────────┘
  ```
  
  Slide 50 emphasizes the core result:
  > **"Now every target pixel receives a value. That's why there are no holes."**
  
  Because we iterate over the destination grid, every single target pixel is evaluated by definition. Holes, cracks, and overlapping collisions are completely avoided.
  
  ---
## 3. Sub-Pixel Resampling and Interpolation (Slides 50, 51, 54)
  
  Slide 50 highlights a fundamental property of inverse mapping:
  > *"Suppose the inverse transformation gives $T^{-1}(x', y') = (20.3, \, 15.7)$. But pixels in the original image exist only at integer positions: $(20, 15), (21, 15), (20, 16), (21, 16)$. There is no actual pixel at $(20.3, 15.7)$! So we estimate its value using interpolation."*
  
  ```
        (20, 15)                                    (21, 15)
           ┌───────────────────────────────────────────┐
           │                                           │
           │                                           │
           │              • (20.3, 15.7)               │
           │                Fractional Query Point     │
           │                                           │
           │                                           │
           └───────────────────────────────────────────┘
        (20, 16)                                    (21, 16)
        
        The continuous intensity at (20.3, 15.7) is reconstructed 
        from the 4 surrounding discrete integer samples!
  ```
  
  ---
### A. Cardinal Interpolation Kernels (Slide 54)
  Slide 54 reviews the four classical continuous reconstruction filters (introduced in **Module M2.2**):
  
  ```
  ┌───────────────────────────┬───────────────────────────────────────────┬────────────────────────────────────────┐
  │ Filter Kernel             │ Mathematical Form $h(x)$                  │ Performance / Artifact Profile         │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Nearest-Neighbor**      │ $\Pi(x) = \begin{cases} 1 & |x| < 0.5 \\ 0 & \text{otherwise} \end{cases}$ │ Fastest, but severe blocky pixelation  │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Bilinear**              │ $\Lambda(x) = \begin{cases} 1 - |x| & |x| < 1 \\ 0 & \text{otherwise} \end{cases}$ │ $C^0$ continuous; standard GPU default │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Bicubic (Keys Spline)** │ Piecewise Cubic Polynomial over $[-2, +2]$│ $C^1$ smooth; preserves edge sharpness │
  ├───────────────────────────┼───────────────────────────────────────────┼────────────────────────────────────────┤
  │ **Sinc / Lanczos**        │ $h(x) = \frac{\sin(\pi x)}{\pi x} \cdot \text{Window}(x)$ │ Sharpest frequency cutoff; slow        │
  └───────────────────────────┴───────────────────────────────────────────┴────────────────────────────────────────┘
  ```
  
  ---
### B. Bilinear Interpolation Formula (Review)
  For query coordinate $(x, y) = (x_0 + \alpha, \, y_0 + \beta)$, where $x_0 = \lfloor x \rfloor, y_0 = \lfloor y \rfloor$, and fractional offsets $\alpha, \beta \in [0, 1)$:
  
  $$f(x, y) = (1 - \alpha)(1 - \beta) f(x_0, y_0) + \alpha(1 - \beta) f(x_0 + 1, y_0) + (1 - \alpha)\beta f(x_0, y_0 + 1) + \alpha\beta f(x_0 + 1, y_0 + 1)$$
  
  ---
## 4. The Complete 3-Step Image Warping Procedure (Slides 51–53)
  
  Slide 52 formalizes the standard production pipeline for executing an arbitrary planar image transformation:
  
  ```
               ┌────────────────────────────────────────────────────────┐
               │         For Every Discrete Target Pixel (x', y'):      │
               └───────────────────────────┬────────────────────────────┘
                                           │
                                           ▼
               ┌────────────────────────────────────────────────────────┐
               │    Step 1: Compute Continuous Source Coordinate        │
               │            via Inverted Homography Matrix:             │
               │                                                        │
               │        ┌ x ┐          ┌ x' ┐                           │
               │        │ y │ =  H⁻¹ · │ y' │                           │
               │        └ w ┘          └ 1  ┘                           │
               │                                                        │
               │        x_src = x / w,      y_src = y / w               │
               └───────────────────────────┬────────────────────────────┘
                                           │
                                           ▼
               ┌────────────────────────────────────────────────────────┐
               │      Step 2: Boundary Checking & Out-of-Bounds Test    │
               │    Is (x_src, y_src) within source image boundaries?   │
               │         0 ≤ x_src ≤ W_src - 1                          │
               │         0 ≤ y_src ≤ H_src - 1                          │
               └───────────────────────────┬────────────────────────────┘
                                           │
                             ┌─────────────┴─────────────┐
                         YES │                           │ NO
                             ▼                           ▼
               ┌───────────────────────────┐ ┌──────────────────────────┐
               │  Step 3: Sample & Write   │ │   Fill with Background   │
               │ Query f(x_src, y_src) via │ │   Default (e.g., black   │
               │ Bilinear Interpolation:   │ │   or transparent alpha): │
               │   g(x', y') = sampled_val │ │   g(x', y') = 0          │
               └───────────────────────────┘ └──────────────────────────┘
  ```
  
  ---
### Why Inverting a Homography is Trivial
  A key benefit of working with homogeneous coordinates is matrix algebra:
  If the forward projective transformation mapping the source image to the target canvas is $\mathbf{H}$, then the backward mapping function required for inverse warping is simply the **standard matrix inverse $\mathbf{H}^{-1}$**:
  
  $$\mathbf{p}_{\text{src}} \sim \mathbf{H}^{-1} \mathbf{p}_{\text{dst}}$$
  
  Because $\mathbf{H}$ is a small $3 \times 3$ matrix, computing $\mathbf{H}^{-1}$ takes microsecond-level time via analytical Cramer's rule.
  
  ---
## 5. Practical Pipeline Engineering: Canvas Sizing and Bounding Boxes
  
  Before looping through pixels $(x', y')$, an algorithm must determine:
  * **How large should the target canvas $g$ be?**
  * **Where should the origin $(0, 0)$ be placed so the warped image doesn't get clipped?**
  
  ```
       Source Image Corners                              Warped Image Extents on Target Canvas
       ┌───────────────────────────┐                         ┌────────────────────────────────────────┐
       │ (0,0)             (W-1, 0)│                         │ (x_min, y_min)                         │
       │   •───────────────────•   │                         │       ┌───────────────────────┐        │
       │   │                   │   │    Forward Map H        │       │      /               /│        │
       │   │   Source Image    │   │  ─────────────────►     │       │     /   Warped      / │        │
       │   │                   │   │   on 4 Corners Only     │       │    /    Image      /  │        │
       │   •───────────────────•   │                         │       │   /               /   │        │
       │ (0, H-1)       (W-1, H-1) │                         │       └──/───────────────/────┘        │
       └───────────────────────────┘                         │                         (x_max, y_max) │
                                                             └────────────────────────────────────────┘
  ```
  
  ---
### The 4-Corner Bounding Box Algorithm:
  1. Extract the four extreme boundary corner coordinates of the source image:
  
  $$\mathcal{C} = \big\{ (0, 0), \, (W_{\text{src}} - 1, 0), \, (W_{\text{src}} - 1, H_{\text{src}} - 1), \, (0, H_{\text{src}} - 1) \big\}$$
  
  2. Map these four points through the **forward** homography $\mathbf{H}$:
  
  $$\mathbf{c}_i' \sim \mathbf{H} \begin{bmatrix} x_i \\ y_i \\ 1 \end{bmatrix} \implies (x_i', y_i') = \left( \frac{x_i'}{w_i'}, \, \frac{y_i'}{w_i'} \right)$$
  
  3. Compute the bounding rectangle enclosing the transformed corners:
  
  $$x_{\min} = \lfloor \min_i x_i' \rfloor, \quad x_{\max} = \lceil \max_i x_i' \rceil$$
  
  $$y_{\min} = \lfloor \min_i y_i' \rfloor, \quad y_{\max} = \lceil \max_i y_i' \rceil$$
  
  4. The target canvas dimensions are:
  
  $$W_{\text{dst}} = x_{\max} - x_{\min}, \quad H_{\text{dst}} = y_{\max} - y_{\min}$$
  
  5. Adjust the homography to shift the warped image onto the canvas:
   Because $x_{\min}$ and $y_{\min}$ can be negative, compose $\mathbf{H}$ with a 2D translation matrix:
  
  $$\mathbf{H}_{\text{adjusted}} = \begin{bmatrix} 1 & 0 & -x_{\min} \\ 0 & 1 & -y_{\min} \\ 0 & 0 & 1 \end{bmatrix} \cdot \mathbf{H}$$
  
  The final inverse mapping matrix used for sampling is:
  
  $$\mathbf{M}_{\text{pull}} = \mathbf{H}_{\text{adjusted}}^{-1}$$
  
  ---
## 6. Vectorized Python Implementation: Inverse Homography Warping
  
  Evaluating inverse warping using nested Python loops (`for x in ... for y in ...`) is too slow for large images. 
  The production-grade implementation below uses **vectorized NumPy grid generation and bilinear interpolation**:
  
  ```python
  import numpy as np
  
  def warp_image_homography(image: np.ndarray, H: np.ndarray, output_shape: tuple = None) -> np.ndarray:
    """
    Executes high-performance inverse image warping under a 3x3 Homography H.
    
    Parameters:
        image: Source image array of shape (H_src, W_src) or (H_src, W_src, C).
        H: (3, 3) projective transformation matrix mapping source -> destination.
        output_shape: Optional tuple (H_dst, W_dst). If None, calculated via corner bounding box.
        
    Returns:
        warped_image: Destination canvas with interpolated source pixels.
    """
    H_src, W_src = image.shape[:2]
    is_color = (image.ndim == 3)
    
    # -------------------------------------------------------------
    # Step 1: Compute target canvas extents via forward corner mapping
    # -------------------------------------------------------------
    src_corners = np.array([
        [0.0, 0.0, 1.0],
        [W_src - 1.0, 0.0, 1.0],
        [W_src - 1.0, H_src - 1.0, 1.0],
        [0.0, H_src - 1.0, 1.0]
    ]).T  # Shape: (3, 4)
    
    # Project corners forward
    projected_corners = H @ src_corners
    projected_corners /= projected_corners[2:3, :]  # Perspective divide
    
    x_coords = projected_corners[0, :]
    y_coords = projected_corners[1, :]
    
    x_min, x_max = int(np.floor(np.min(x_coords))), int(np.ceil(np.max(x_coords)))
    y_min, y_max = int(np.floor(np.min(y_coords))), int(np.ceil(np.max(y_coords)))
    
    if output_shape is None:
        W_dst = x_max - x_min
        H_dst = y_max - y_min
        # Offset translation matrix to place top-left corner at (0, 0)
        T_offset = np.array([
            [1.0, 0.0, -x_min],
            [0.0, 1.0, -y_min],
            [0.0, 0.0, 1.0]
        ])
        H_forward = T_offset @ H
    else:
        H_dst, W_dst = output_shape
        H_forward = H
        
    # -------------------------------------------------------------
    # Step 2: Compute the Inverse Homography for pull-sampling
    # -------------------------------------------------------------
    H_inv = np.linalg.inv(H_forward)
    
    # Generate complete target pixel coordinate grid
    dst_y, dst_x = np.indices((H_dst, W_dst), dtype=np.float32)
    dst_coords_flat = np.stack([dst_x.ravel(), dst_y.ravel(), np.ones(H_dst * W_dst, dtype=np.float32)])
    
    # -------------------------------------------------------------
    # Step 3: Project target coordinates backward into source space
    # -------------------------------------------------------------
    src_projected = H_inv @ dst_coords_flat
    
    # Perspective division
    src_w = src_projected[2, :]
    src_w = np.where(np.abs(src_w) < 1e-12, 1e-12, src_w)
    src_x = src_projected[0, :] / src_w
    src_y = src_projected[1, :] / src_w
    
    # Reshape back to destination grid
    src_x = src_x.reshape(H_dst, W_dst)
    src_y = src_y.reshape(H_dst, W_dst)
    
    # -------------------------------------------------------------
    # Step 4: Bilinear Interpolation
    # -------------------------------------------------------------
    x0 = np.floor(src_x).astype(np.int32)
    y0 = np.floor(src_y).astype(np.int32)
    x1 = x0 + 1
    y1 = y0 + 1
    
    # Identify valid in-bounds pixels
    valid_mask = (x0 >= 0) & (x1 < W_src) & (y0 >= 0) & (y1 < H_src)
    
    # Fractional weights
    alpha = src_x - x0
    beta = src_y - y0
    
    # Initialize output array
    if is_color:
        channels = image.shape[2]
        warped = np.zeros((H_dst, W_dst, channels), dtype=image.dtype)
        # Expand weights for multi-channel broadcasting
        alpha = alpha[..., np.newaxis]
        beta = beta[..., np.newaxis]
        valid_mask_c = valid_mask[..., np.newaxis]
    else:
        warped = np.zeros((H_dst, W_dst), dtype=image.dtype)
        valid_mask_c = valid_mask
        
    # Clip coordinates to prevent index out of bounds on padded invalid regions
    x0_safe = np.clip(x0, 0, W_src - 1)
    x1_safe = np.clip(x1, 0, W_src - 1)
    y0_safe = np.clip(y0, 0, H_src - 1)
    y1_safe = np.clip(y1, 0, H_src - 1)
    
    f00 = image[y0_safe, x0_safe]
    f10 = image[y0_safe, x1_safe]
    f01 = image[y1_safe, x0_safe]
    f11 = image[y1_safe, x1_safe]
    
    # Evaluate 2D tensor bilinear equation
    interpolated = (1.0 - alpha) * (1.0 - beta) * f00 + \
                   alpha * (1.0 - beta) * f10 + \
                   (1.0 - alpha) * beta * f01 + \
                   alpha * beta * f11
                   
    warped = np.where(valid_mask_c, interpolated, 0)
    return warped
  ```
  
  ---
## 7. Bridge to the Next Topic: Describing Features (Slide 56)
  
  Slide 56 concludes the module and links our transformation framework to feature description:
  > **"Next Time: Describing Features.**
  > *We want to be able to 'match' features between images.*
  > *Our warping and transformation procedures will be essential for this."*
  
  ```
                            The Complete Recognition Pipeline
                            
      Feature Detection             Scale/Affine Warping             Feature Description
      (Modules M3.1 & M3.2)         (This Module)                   (Upcoming Module)
      ─────────────────────         ────────────────────             ───────────────────
      Find distinct interest        Extract patch around            Compute canonical 
      points (Harris, LoG, DoG).    keypoint and WARP it into       orientation, build SIFT
                                    canonical scale & rotation!     gradient histograms!
  ```
  
  * **Why Warping is Essential for Descriptors:**
  When a feature is detected with a characteristic scale $\sigma^*$ and a dominant gradient orientation $\theta$, we don't match the raw, arbitrarily oriented image patch directly.
  Instead, we use our **inverse warping machinery** to:
  1. Translate the keypoint to the origin.
  2. Rotate the patch by $-\theta$ to normalize its orientation.
  3. Scale the patch by $1/\sigma^*$ to normalize its size.
  * The resulting **canonical, upright patch** can then be encoded into an invariant descriptor (such as the SIFT descriptor), completing the feature detection, transformation, and matching pipeline.
  
  ---