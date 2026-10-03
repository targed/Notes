## 1. Motivation: The Persistence of the Scale Problem (Slides 2–4)
  
  In **Module M3.1**, we derived the Harris Corner Detector to find distinctive 2D interest points. While the Harris operator is robust to image noise, Slide 2 revisits the multi-view matching scenario:  
  
  ```
     Image 1 (Close-Up View)                        Image 2 (Distant / Zoomed-Out View)
  ┌───────────────────────────┐                  ┌───────────────────────────┐
  │          ╭─────╮          │                  │                           │
  │          │     │          │                  │            ╭─╮            │
  │          │  *  │          │                  │            ╰─╯            │
  │          ╰─────╯          │                  │                           │
  └───────────────────────────┘                  └───────────────────────────┘
       Large Sensor Footprint                         Small Sensor Footprint
       Scale σ_large                                  Scale σ_small
  ```
  
  Slide 2 details the core problem:  
  > **The Core Problem:**  
  When matching features (such as corners, edges, or blobs) across two images, the same physical real-world structure may appear at completely different scales because:
  >	1. The camera focal length (zoom) has changed.
  2. The distance $Z$ between the object and the camera sensor has varied.
  3. The digital sensor resolution differs across capture devices.
  
  > *If we do not explicitly compensate for scale, local features cannot be matched reliably.*
### Why First and Second Derivatives Fail Across Scale (Slide 3)
  Slide 3 asks: *"What if we use derivatives or curvatures?"*  
  * Under a spatial magnification factor $s$ where $I_{\text{scaled}}(x, y) = I(s \cdot x, \, s \cdot y)$ :
  $$\nabla I_{\text{scaled}} = s \cdot \nabla I, \quad \nabla^2 I_{\text{scaled}} = s^2 \cdot \nabla^2 I$$
  * Gradients and second-order curvatures scale algebraically with $s$ .
  * As a result, the Harris Structure Tensor $\mathbf{H}$ and its eigenvalues are directly tied to the scale of the sensor grid.
  ---
## 2. Invariance vs. Equivariance (Covariance): Clarifying the Terminology (Slides 5–8)
  
  Computer vision literature often uses the terms **invariance** and **covariance** loosely (as noted on Slide 8). We need to distinguish between what happens to a feature's **spatial location** versus what happens to its **numerical descriptor**.  
  
  ```
                           Robustness to Image Transformations
                                            │
         ┌──────────────────────────────────┴──────────────────────────────────┐
         ▼                                                                     ▼
  [ INVARIANCE ]                                                        [ EQUIVARIANCE / COVARIANCE ]
  The OUTPUT value does NOT change when                                 The OUTPUT coordinates TRANSFORM 
  the input changes:                                                    identically with the input:
  
         g( T(I) ) = g(I)                                                      D( T(I) ) = T( D(I) )
  
  • Desirable for: DESCRIPTORS                                          • Desirable for: DETECTORS
  • A feature vector (e.g., SIFT descriptor)                            • The detected corner coordinates (x, y)
    must evaluate to the EXACT SAME numbers                               must rotate, translate, and scale 
    regardless of lighting or angle!                                      with the physical object!
  ```
  
  ---
### A. Formal Mathematical Definitions (Slide 6, 8)
  Let $I: \mathbb{R}^2 \to \mathbb{R}$ be an image, and let $T: \mathbb{R}^2 \to \mathbb{R}^2$ be a spatial coordinate transformation (such as a translation, rotation, or scaling).
#### 1. Equivariance (Covariance):
  A feature detector $\mathcal{D}$ that outputs a set of 2D coordinates $\{\mathbf{x}_i\} \subset \mathbb{R}^2$ is **equivariant (covariant)** with respect to transformation $T$ if:  
  
  $$\mathcal{D}\big(I \circ T^{-1}\big) = T\big(\mathcal{D}(I)\big)$$  
  * **In Plain English:** If an object in an image moves $20\text{ pixels}$ to the right, the detector must output keypoint coordinates that have also moved $20\text{ pixels}$ to the right.
  * If a detector were truly *invariant* to position, its output coordinates would never change, always pointing to the same fixed $(x, y)$ coordinate regardless of where the object moved!
#### 2. Invariance:
  A feature descriptor $\mathbf{f}(\mathbf{x})$ that extracts an embedding vector $\mathbf{v} \in \mathbb{R}^d$ around coordinate $\mathbf{x}$ is **invariant** with respect to transformation $T$ if:  
  
  $$\mathbf{f}_{T(I)}\big(T(\mathbf{x})\big) = \mathbf{f}_I(\mathbf{x})$$  
  * **In Plain English:** The numerical fingerprint describing a patch must remain constant even when the patch is rotated, scaled, or photometrically altered.
   > **Summary of the Terminology (Slide 8):**
   >		* **Detectors** should be **equivariant (covariant)**: their detected locations must track the geometric transformation of the scene.
  * **Descriptors** should be **invariant**: their extracted representations must be independent of the transformation.
  
  ---
## 3. Invariance and Equivariance Profile of the Harris Detector (Slides 9–14)
  
  Let's evaluate how the Harris Corner Detector behaves across the four fundamental image transformations: translation, rotation, affine lighting shifts, and spatial scaling.  
  
  ---
### A. Translation: Equivariant (Slides 9–10)
  Slide 9 and 10 ask: *"Harris corners under image translation: Invariant or Equivariant?"*  
  
  ```
       Original Image I(x, y)                         Translated Image I'(x, y) = I(x - x₀, y - y₀)
     ┌───────────────────────────┐                  ┌───────────────────────────┐
     │                           │                  │                           │
     │      ▲ (x_c, y_c)         │   Shift by       │                           │
     │     / \ Corner Detected!  │  (x₀, y₀)        │                  ▲ (x_c + x₀, y_c + y₀)
     │    /   \                  │ ────────────►    │                 / \ Corner Detected at
     │   /_____\                 │                  │                /   \ NEW coordinate!
     │                           │                  │               /_____\
     └───────────────────────────┘                  └───────────────────────────┘
  ```
  
  * **Proof:**
  1. The spatial derivative operator is a Linear Shift-Invariant (LSI) convolution:
  $$\nabla \big( I(x - x_0, \, y - y_0) \big) = (\nabla I)(x - x_0, \, y - y_0)$$
  2. The elements of the Structure Tensor $\mathbf{H}$ are formed via pointwise products and Gaussian convolutions, which are also shift-invariant:
  $$\mathbf{H}_{\text{trans}}(x, y) = \mathbf{H}(x - x_0, \, y - y_0)$$
  3. The scalar corner response satisfies:
  $$R_{\text{trans}}(x, y) = R(x - x_0, \, y - y_0)$$
  * **Conclusion (Slide 10):** The response map shifts along with the image. The detected corner coordinates move to $(x_c + x_0, y_c + y_0)$ . The detector is **strictly equivariant to translation**.
  ---
### B. In-Plane 2D Rotation: Equivariant (Slides 11–12)
  Slide 11 and 12 present a blue polygon rotated in-plane and inspect the resulting error ellipse:  
  
  ```
        Original Orientation                                Rotated by Angle θ
     ┌───────────────────────────┐                      ┌───────────────────────────┐
     │                           │                      │                           │
     │            ▲              │      Rotate by θ     │            /              │
     │           / \             │     ─────────────►   │           / \             │
     │          /   \            │                      │          /   \            │
     │         /_____\           │                      │         /_____\           │
     └───────────────────────────┘                      └───────────────────────────┘
                   │                                                  │
                   ▼                                                  ▼
           Horizontal Ellipse                                  Rotated Ellipse
                ╭─────╮                                             ╭───╮
              ╭─╯     ╰─╮                                         ╭─╯   ╰─╮
              ╰─╮     ╭─╯                                         ╰─╮   ╭─╯
                ╰─────╯                                             ╰───╯
              Axes: (v₁, v₂)                                     Axes: (R_θ v₁, R_θ v₂)
              Eigenvalues: λ₁, λ₂                                Eigenvalues: λ₁, λ₂ (UNCHANGED!)
  ```
  
  * **Mathematical Proof:**  
  Let $\mathbf{R}_\theta = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$ be an orthogonal rotation matrix ( $\mathbf{R}_\theta^T \mathbf{R}_\theta = \mathbf{I}$ ).
  1. Under rotation, the spatial gradient vector transforms as:
  $$\nabla I_{\text{rot}}(\mathbf{x}) = \mathbf{R}_\theta \nabla I(\mathbf{R}_\theta^T \mathbf{x})$$
  2. Integrating with an isotropic circular Gaussian kernel $g(x, y) = g(\|\mathbf{x}\|)$ , the Structure Tensor transforms via a similarity transformation:
  $$\mathbf{H}_{\text{rot}} = \mathbf{R}_\theta \, \mathbf{H} \, \mathbf{R}_\theta^T$$
  3. Recall from **Part 3** that the eigenvalues $\lambda_1, \lambda_2$ of a matrix are invariant under orthogonal similarity transformations:
  $$\det(\mathbf{H}_{\text{rot}} - \lambda \mathbf{I}) = \det\big(\mathbf{R}_\theta (\mathbf{H} - \lambda \mathbf{I}) \mathbf{R}_\theta^T\big) = \det(\mathbf{R}_\theta) \det(\mathbf{H} - \lambda \mathbf{I}) \det(\mathbf{R}_\theta^T) = \det(\mathbf{H} - \lambda \mathbf{I})$$
  * **Conclusion (Slide 12):**
  * The **eigenvectors rotate** by angle $\theta$ , aligning with the new rotated edge directions.
  * The **eigenvalues remain identical**: $\lambda_1^{\text{rot}} = \lambda_1$ and $\lambda_2^{\text{rot}} = \lambda_2$ .
  * Because the response $R = \det(\mathbf{H}) - k \cdot \text{tr}(\mathbf{H})^2$ depends solely on the eigenvalues, the scalar response field rotates with the image:
  $$R_{\text{rot}}(\mathbf{x}) = R(\mathbf{R}_\theta^T \mathbf{x})$$ The Harris corner detector is **equivariant to 2D image rotation**.
  ---
### C. Affine Illumination Changes: Partially Invariant (Slides 13–14)
  Slide 13 and 14 consider an affine intensity transformation:  
  
  $$I_{\text{new}}(x, y) = a \cdot I(x, y) + b$$  
  where $a > 0$ represents a multiplicative contrast/gain change, and $b$ represents an additive brightness shift.  
  
  ```
       Response Profile under Gain a = 1              Response Profile under Gain a > 1
             ^ R(x)                                         ^ R(x)
             │      • Peak A                                │          • Peak A (Scaled by a⁴!)
             │     / \     • Peak B                         │         / \
         τ ──┼────/───\───/─\────────                   τ ──┼────────/───\───• Peak B
             │   /     \ /   \                              │       /     \ / \
             └──┴───────┴─────┴──► x                        └──────┴───────┴───┴──► x
             Both peaks exceed threshold!                   Peak B drops BELOW threshold if a < 1!
  ```
  
  1. **Additive Offset ($b$): Fully Invariant.**  
  Because spatial differentiation eliminates constant offsets ( $\nabla(I + b) = \nabla I$ ), the Structure Tensor $\mathbf{H}$ and corner response $R$ are unaffected by $b$ .
  2. **Multiplicative Contrast ($a$): Partially Invariant.**
  * Gradients scale linearly: $\nabla I_{\text{new}} = a \cdot \nabla I$ .
  * The Structure Tensor scales quadratically: $\mathbf{H}_{\text{new}} = a^2 \mathbf{H}$ .
  * The corner response scales to the fourth power:
  $$R_{\text{new}} = \det(a^2 \mathbf{H}) - k \cdot \text{tr}(a^2 \mathbf{H})^2 = a^4 \cdot R$$
  * **Why it is only "Partially Invariant" (Slide 14):**  
  While the spatial locations of the local maxima do not move, the peak amplitudes scale by $a^4$ . If an image dims ( $a < 1$ ), corner responses drop sharply. With a static threshold $\tau$ , valid corners fall below the threshold and are missed. To achieve full invariance, the threshold $\tau$ must be adjusted relative to the image's dynamic range.
  ---
## 4. Why Scale Breaks the Harris Detector (Slides 15–17)
  
  Slide 17 delivers a key finding:  
  > **"Harris Corners/Features are NEITHER invariant NOR equivariant under scale."**  
  
  ```
       Physical Object at Small Scale                     Physical Object at Large Scale (Zoomed In)
       (Camera Far Away)                                  (Camera Up-Close)
       ┌───────────────────────────┐                      ┌───────────────────────────┐
       │             │             │                      │                     ╭───  │
       │             │             │                      │                   ╭─╯     │
       │        ─────┘ Corner      │                      │                 ╭─╯       │
       │               Detected!   │                      │                ╭╯ Smooth  │
       │                           │                      │               ╭╯  Contour │
       └───────────────────────────┘                      └───────────────┴───────────┘
       Window W covers BOTH edges.                        Window W sees ONLY a locally straight arc!
       Both λ₁ and λ₂ are large:                          λ_max >> 0, but λ_min ≈ 0:
       CLASSIFIED AS A CORNER!                            CLASSIFIED AS AN EDGE! (Aperture Problem)
  ```
  
  ---
### The Geometric Origin of the Failure (Slide 17)
  Consider a sharp right-angled corner. In reality, physical corners are rarely mathematical singular points; they have small, rounded transitions with a finite radius of curvature $r_{\text{corner}}$ .  
  
  1. **At a Distant Scale (Zoomed Out):**
  * The fixed integration window $W$ (defined by Gaussian scale $\sigma_i$ ) is large relative to the radius of curvature.
  * Both intersecting boundary edges fall completely within $W$ .
  * Gradients point in two distinct directions; both eigenvalues $\lambda_1$ and $\lambda_2$ are large.
  * **Result:** The point is classified as a **Corner**.  
  2. **At a Close Scale (Zoomed In / Enlarged):**
  * The image is magnified, but the integration window $W$ remains fixed in pixel coordinates.
  * The window now covers only a tiny sub-segment of the rounded corner boundary.
  * Within this small aperture, the boundary looks like a **locally straight, flat edge**.
  * Gradients point in only one dominant direction: $\lambda_{\max} \gg 0$ , but $\lambda_{\min} \approx 0$ .
  * **Result:** The vertex is classified as an **Edge**. The corner response collapses ( $R < 0$ ).
    > **Core Takeaway (Slide 17):**  
    Because classical corner detectors use a **fixed spatial integration scale $\sigma_i$**, their detections are not scale-equivariant. To match features across varying viewpoints and distances, we need a mechanism that **automatically detects the intrinsic scale of each local structure**.  
  ---
## Summary Matrix: Transformation Properties of the Harris Detector
  
  | Transformation | Mathematical Model | Detector Behavior | Physical Consequence |
  |---|---|---|---|
  | **Translation** | $I'(x, y) = I(x - x_0, y - y_0)$ | **Equivariant** | Corners move by $(x_0, y_0)$ identically with the scene |
  | **In-Plane Rotation** | $I'(\mathbf{x}) = I(\mathbf{R}_\theta \mathbf{x})$ | **Equivariant** | Eigenvectors rotate by $\theta$ ; eigenvalues and $R$ are invariant |
  | **Additive Lighting** | $I'(x, y) = I(x, y) + b$ | **Invariant** | Derivatives eliminate constant offsets: $\nabla(I + b) = \nabla I$ |
  | **Contrast Gain** | $I'(x, y) = a \cdot I(x, y)$ | **Partially Invariant** | Peak positions hold, but response scales by $a^4$ ; requires adaptive $\tau$ |
  | **Scale Variation** | $I'(x, y) = I(s \cdot x, s \cdot y)$ | **FAILS (Neither)** | Corners turn into locally straight edges, vanishing under fixed $\sigma_i$ |
  
  ---