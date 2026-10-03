## 1. Motivation: The Correspondence Problem (Slides 2, 9–11)
  
  A core task in computer vision is finding **correspondences**: identifying whether two or more images observe the exact same physical 3D scene point or object.
  
  ```
     Image 1 (Base View)                            Image 2 (Moved / Rotated View)
  ┌───────────────────────────┐                  ┌───────────────────────────┐
  │         ╭───────╮         │                  │                           │
  │         │ VASE  │         │                  │            ╭───╮          │
  │         │ (200px│         │                  │            │VASE (Rotated)│
  │         ╰───────╯         │                  │            ╰───╯          │
  └───────────────────────────┘                  └───────────────────────────┘
               │                                              │
               └──────────────────────┬───────────────────────┘
                                      ▼
                   [ The Correspondence Problem ]
                   "Which point in Image 1 corresponds 
                    to which point in Image 2?"
  ```
  
  Establishing robust geometric correspondences is the foundation for:
  1. **Visual Odometry & SLAM:** Tracking camera motion over time by locking onto fixed landmarks.
  2. **Structure from Motion (SfM) & 3D Reconstruction:** Triangulating 3D point clouds from collections of uncalibrated tourist photos (*Building Rome in a Day*).
  3. **Panorama Stitching:** Aligning and blending overlapping photos via projective homographies.
  4. **Target Tracking & Pose Estimation:** Following a robotic end-effector or a vehicle across a video stream.
  
  ---
## 2. Why Global Template Matching / Matched Filters Fail (Slides 3–8)
  
  The most intuitive way to locate an object in a scene is **Template Matching** (using a **Matched Filter**).
### A. Classical Cross-Correlation Matching
  Given an input image $I$ and a target template $T$ (such as Margaret Hamilton’s face, Slide 3):
  1. Treat the template $T$ as a convolution/correlation kernel.
  2. Slide $T$ across every possible spatial offset $(u, v)$ in image $I$.
  3. Compute the raw inner product (Cross-Correlation):
  
  $$R_{\text{raw}}(u, v) = \sum_{x} \sum_{y} I(u + x, \, v + y) \cdot T(x, y)$$
  
  ---
### B. The Brightness Trap: Why Raw Correlation Fails (Slide 5)
  Slide 5 works through a numeric example demonstrating why raw cross-correlation is fundamentally flawed for object recognition:
  
  Suppose our target template is a tiny $2 \times 2$ patch $T$:
  
  $$T = \begin{bmatrix} 10 & 12 \\ 14 & 16 \end{bmatrix}, \quad \mu_T = \frac{10 + 12 + 14 + 16}{4} = 13$$
  
  Now, slide this template across an image containing two distinct candidates:
  * **Candidate Patch A (A uniform, blank white region):**
  
  $$A = \begin{bmatrix} 200 & 200 \\ 200 & 200 \end{bmatrix}$$
  
  * **Candidate Patch B (A structured pattern matching the template's contrast):**
  
  $$B = \begin{bmatrix} 50 & 60 \\ 70 & 80 \end{bmatrix}$$
#### Evaluating Raw Correlation:
  * Correlation with Uniform Bright Region $A$:
  
  $$R_A = (10 \times 200) + (12 \times 200) + (14 \times 200) + (16 \times 200) = 52 \times 200 = \mathbf{10,400}$$
  
  * Correlation with Structured Target Region $B$:
  
  $$R_B = (10 \times 50) + (12 \times 60) + (14 \times 70) + (16 \times 80) = 500 + 720 + 980 + 1280 = \mathbf{3,480}$$
  
  > **The Pathological Failure:** 
  > Even though Patch $B$ matches the structural gradient of the template, **Patch $A$ produces an output score nearly $3\times$ higher simply because it is uniformly bright**. 
  > Raw cross-correlation does not search for shape or structure; it acts as an energy detector biased toward high luminance.
  
  ---
### C. The Mean-Shifted Solution: Zero-Mean Cross-Correlation (Slides 4–6)
  To eliminate sensitivity to ambient illumination, we subtract the template's average brightness $\mu_T$ from each of its pixels, creating a **Zero-Mean Filter** $T'$:
  
  $$T'(x, y) = T(x, y) - \mu_T$$
  
  For our template:
  
  $$T' = \begin{bmatrix} 10-13 & 12-13 \\ 14-13 & 16-13 \end{bmatrix} = \begin{bmatrix} -3 & -1 \\ +1 & +3 \end{bmatrix}, \quad \sum T' = 0$$
#### Re-evaluating the Matches (Slide 5):
  * Response over Uniform Bright Region $A$:
  
  $$R'_A = (-3 \times 200) + (-1 \times 200) + (+1 \times 200) + (+3 \times 200) = 200 \times (-3 - 1 + 1 + 3) = \mathbf{0}$$
  
  * Response over Structured Target Region $B$:
  
  $$R'_B = (-3 \times 50) + (-1 \times 60) + (+1 \times 70) + (+3 \times 80) = -150 - 60 + 70 + 240 = \mathbf{+100}$$
  
  * **Visual Proof (Slide 4, 6):** When the mean-subtracted template of Margaret Hamilton's face is correlated across the exact same image, a bright, localized correlation peak appears over her face.
  
  ---
### D. The Real-World Breakdown: Why Template Matching Cannot Generalize (Slides 7–9)
  Slide 6 asks: *"The bright spot corresponds to the filter. Are we done?"*
  
  Slide 7 tests this model: what happens if we search for Margaret Hamilton in a **different photograph** (e.g., inside the Apollo spacecraft simulator)?
  
  ```
        Template (Face Crop)                           Target Image (Apollo Cockpit)
     ┌──────────────────────────┐                   ┌────────────────────────────────────────┐
     │         ╭─────╮          │                   │                                        │
     │        ( • _ • )         │                   │             .-.                        │
     │         ╰─────╯          │                   │            ( -_•) <-- Head tilted 45°  │
     │  Upright, direct lighting│                   │             `-'       Shadowed, smaller│
     └──────────────────────────┘                   └────────────────────────────────────────┘
                   │                                             │
                   └──────────────────────┬──────────────────────┘
                                          ▼
                      [ Cross-Correlation Output Matrix ]
                      Flat, noisy, completely dispersed response!
                      NO localized peak exists! MATCH FAILS.
  ```
  
  As shown on Slide 8, template matching fails because templates are rigid pixel grids. A template fails to match whenever the target undergoes real-world transformations:
  1. **In-Plane and Out-of-Plane Rotation:** Tilting the head by even $15^\circ$ completely misaligns the pixel rows.
  2. **Scale / Distance Changes:** Moving closer or farther alters the template footprint (as seen in Module M2.1).
  3. **Non-Rigid Articulation:** Changes in facial expression (smiling vs. neutral).
  4. **3D Perspective Warping:** Viewing an object from an oblique angle foreshortens surface geometry.
  
  > **Takeaway (Slides 9–11):** We cannot match complex objects using rigid global templates. Instead, we must detect small, **local, generic interest points (features)** that can be localized independently and tracked across varying viewpoints.
  
  ---
## 3. What Makes a "Good" Feature? (Slides 13–16)
  
  Slide 13 shows a panoramic cityscape of Sydney, Australia. If an autonomous agent or 3D reconstruction pipeline needs to find landmarks to determine its camera pose, which pixels should it choose?
  
  ```
                     Desirable Properties of Local Features
                     
     [ SALIENCY / UNIQUENESS ]                      [ REPEATABILITY ]
     The point must be visually distinct            The detector must find the exact same
     from its surrounding background.               physical point despite noise, blur,
                                                    or illumination changes.
                                  │
                                  ├────────────────────────┐
                                  ▼                        ▼
                      [ PRECISE LOCALIZATION ]    [ COMPACT DESCRIPTIVENESS ]
                      Moving a small window in    The local patch around the point
                      ANY direction must produce  must be easily encoded into a 
                      a dramatic signal change.   distinctive feature vector.
  ```
  
  ---
## 4. The Taxonomy of Local Image Patches (Slides 17–24)
  
  Slides 17–23 present a clean geometric thought experiment: consider a blue polygon with sharp vertices resting on a white canvas. If we place a small inspection window $W$ over different regions, what happens to our ability to localize and track the object?
  
  ```
        Case 1: Flat Interior               Case 2: Straight Edge                 Case 3: Corner / Vertex
             (Slide 18)                          (Slide 20)                              (Slide 21)
        ┌───────────────────┐               ┌───────────────────┐                   ┌───────────────────┐
        │ ░░░░░░░░░░░░░░░░░ │               │ ░░░░░░░░░░│       │                   │ ░░░░░░░░│         │
        │ ░░░░┌───────┐░░░░ │               │ ░░░░┌───┐ │       │                   │ ░░░░┌───┴──┐      │
        │ ░░░░│   W   │░░░░ │               │ ░░░░│ W │ │       │                   │ ░░░░│  W   │      │
        │ ░░░░└───────┘░░░░ │               │ ░░░░└───┘ │       │                   │ ░░░░└──────┘      │
        │ ░░░░░░░░░░░░░░░░░ │               │ ░░░░░░░░░░│       │                   │ ░░░░│             │
        └───────────────────┘               └───────────────────┘                   └───────────────────┘
        • Zero gradients                    • Gradient in 1 direction               • Gradients in ≥ 2 directions
        • Indistinguishable everywhere      • "Slides" along edge                   • Uniquely constrained in (x, y)
  ```
  
  ---
### A. Flat Regions (Slides 17–18)
  * **The Visual Test:** Place a window $W$ entirely inside the blue interior or out on the white background.
  * **Perturbation Response:** Shift the window by an arbitrary displacement $(\Delta x, \Delta y)$ in any direction.
  * **The Result:** The pixel intensities inside the window remain completely identical:
  
  $$I(x + \Delta x, \, y + \Delta y) \approx I(x, y)$$
  
  * **Tracking Diagnostic:** **Unusable.** An algorithm cannot determine whether the window stayed stationary or moved across the interior. The point has zero localization capability.
  
  ---
### B. Edges & The Aperture Problem (Slides 19–20)
  * **The Visual Test:** Place the window $W$ over a straight boundary separating the blue polygon from the white background.
  * **Perturbation Response:**
  * **Perpendicular Shift (Orthogonal to edge):** Moving the window across the edge produces a sharp, immediate change in intensity (high contrast).
  * **Parallel Shift (Tangential along edge):** Moving the window along the direction of the contour produces **zero change** in pixel intensity.
  
  ```
                           The Aperture Problem
                                     ^ Perpendicular Shift:
                                     │ LARGE intensity change!
                                     │
                    ─────────────────┼───────────────── Edge Line
                               ◄─────┼─────►
                               Parallel Shift:
                               ZERO intensity change! (Ambiguous)
  ```
  
  * **The Aperture Problem:** Through a local aperture (small window), one cannot perceive motion or determine correspondences parallel to a straight line. 
  * Slide 20 notes: *"Better, but still not great. Hard to distinguish between these. We can slide along the edge and not know the difference."* 
  * Straight edges suffer from a **1D ambiguity**: their location is constrained in one dimension, but unconstrained in the orthogonal dimension.
  
  ---
### C. Corners / Vertices (Slides 21–24)
  * **The Visual Test:** Place the window $W$ directly over a vertex where two non-parallel boundary edges intersect.
  * **Perturbation Response:** Shift the window in **any direction**:
  * Shifting horizontally alters the vertical boundary edge.
  * Shifting vertically alters the horizontal/slanted boundary edge.
  * Shifting diagonally crosses both contours simultaneously.
  * **Tracking Diagnostic:** **Optimal.** A corner exhibits **significant, multi-directional intensity gradients**:
  
  $$I(x + u, \, y + v) \neq I(x, y) \quad \forall (u, v) \neq (0, 0)$$
  
  * **Localization Accuracy:** Corners can be localized down to a single unique pixel coordinate $(x, y)$—and with sub-pixel interpolation, down to fractions of a pixel.
  
  > **Conclusion (Slide 24):** 
  > Features must be as unique as reasonably achievable. **Corners provide stable 2D point constraints**, making them the ideal primitive for geometric tracking, registration, and correspondence matching.
  
  ---
## 5. Summary Matrix: Region Characteristics
  
  | Region Type | Spatial Gradient Profile | Shift Response $E(u, v)$ | Localization Capability | Suitability as a Feature |
  | :--- | :--- | :--- | :--- | :--- |
  | **Flat Region** | $\nabla I \approx [0, 0]^T$ | Zero in all directions | $0\text{D}$ (Completely unconstrained) | **Unusable** |
  | **Edge Region** | $\nabla I$ points in $1$ direction | High perpendicular, Zero parallel | $1\text{D}$ (Subject to Aperture Problem) | Poor (ambiguous along contour) |
  | **Corner / Blob** | $\nabla I$ spans $\ge 2$ directions | **High in ALL directions** | **$2\text{D}$ (Uniquely constrained)** | **Ideal for tracking & matching** |
  
  ---