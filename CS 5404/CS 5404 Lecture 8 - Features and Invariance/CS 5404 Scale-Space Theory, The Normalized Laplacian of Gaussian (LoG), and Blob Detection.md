## 1. The Principle of Automatic Scale Selection (Slides 18–29)
  
  In **Part 1**, we saw that the classical Harris detector fails when an object changes size because its integration window $W$ is fixed in pixel coordinates.
  
  To achieve true scale equivariance, an algorithm must not only detect the $(x, y)$ spatial location of an interest point, but also determine its **intrinsic scale** $\sigma$ (the physical size of the feature).
  
  ```
                    The Automatic Scale Selection Framework
                    
  Input Image I(x, y) ──► Evaluate Feature Response across Continuous Scales σ
                                            │
                                            ▼
                           Scale Signature Curve: f(σ)
                                            │
                                            ▼
                      Find Scale-Space Local Extremum:
                         σ_characteristic = argmax_σ |f(σ)|
  ```
  
  ---
### A. The Concept of "Characteristic Scale" (Slides 19–20, 27–28)
  Slide 19 poses an intuitive question:
  > *"If we evaluate a feature across multiple window sizes simultaneously, which scale is 'best'—the most corner-like or feature-like?"*
  
  Slide 20 answers:
  > **The Scale-Space Extremum Principle:**
  > For a given image point $(x, y)$, its **characteristic scale** $\sigma^*$ is the scale that **maximizes a scale-normalized feature response function $f(x, y, \sigma)$**:
  > 
  > $$\sigma^* = \arg\max_\sigma \big| f(x, y, \sigma) \big|$$
  
  ```
       Image A (Zoomed-Out Target)                     Image B (Zoomed-In Target)
    ┌───────────────────────────┐                   ┌───────────────────────────┐
    │          ╭───╮            │                   │       ╭───────────╮       │
    │          │ • │ r_A = 10px │                   │       │     •     │ r_B = 20px
    │          ╰───╯            │                   │       │           │       │
    └───────────────────────────┘                   │       ╰───────────╯       │
                  │                                 └───────────────────────────┘
                  ▼                                               │
        Scale Signature Curve f_A(σ)                              ▼
              ^ Response                                Scale Signature Curve f_B(σ)
              │       • Peak at σ_A = 7                       ^ Response
              │      / \                                      │             • Peak at σ_B = 14
          0.0 ┴─────┴───┴─────────► Scale σ               0.0 ┴─────────────┴───┴─────► Scale σ
                    σ_A                                                     σ_B
  ```
  
  ---
### B. Patch Normalization via Detected Scale (Slide 27–28)
  Slides 27 and 28 show graffiti art captured at two different focal scales:
  1. At the corresponding physical keypoints, the scale signature curves peak at different values ($\sigma_A$ for the distant view, $\sigma_B$ for the close-up view).
  2. The ratio of the peaks matches the physical magnification factor:
  
  $$s = \frac{\sigma_B}{\sigma_A}$$
  
  3. **Patch Resampling / Normalization:**
   Instead of extracting fixed-size pixel windows (e.g., $16 \times 16$), the algorithm crops a circular region proportional to the detected characteristic scale:
  
  $$\text{Window Radius: } R = c \cdot \sigma^* \quad (\text{typically } c = 3 \text{ to } 6)$$
  
  4. Resampling both cropped patches to a standardized canonical resolution (e.g., $32 \times 32\text{ pixels}$) makes them **visually identical**, enabling robust descriptor matching (Slide 28).
  
  ---
### C. Implementation Strategy: Scaling the Kernel vs. Resizing the Image (Slides 20, 29)
  To evaluate responses across scales, there are two computationally equivalent approaches:
  
  ```
     Approach 1: Scale the Filter Kernel             Approach 2: Scale the Image (Pyramid)
     (Keep image fixed, grow filter size)            (Keep filter fixed, downsample image)
     
        Image (Fixed: 1000×1000)                        Image Stack (Gaussian Pyramid)
     ┌───────────────────────────┐                   ┌───────┐  Level 2 (250×250)
     │                           │                   │       │
     │      ┌───┐ (3×3)          │                   ├───────┼───────┐ Level 1 (500×500)
     │      ├───┼───┐ (15×15)    │                   │               │
     │      │   │   │            │                   ├───────────────┼───────────────┐
     │      └───┴───┘ (31×31)    │                   │                               │ Level 0
     └───────────────────────────┘                   └───────────────────────────────┘
     • Large kernels become slow                     • Fixed small filter (e.g., 5×5)
       to evaluate in spatial domain                   convolved at each octave
     • O(M² N²) complexity                           • O(1) filter footprint; fast!
  ```
  
  * Slide 29 notes: *"Large window size is expensive."*
  * In practice, modern pipelines build an octave-spaced **Gaussian scale-space pyramid** (Module M2.1) and evaluate a compact, fixed-size filter across downsampled image levels.
  
  ---
## 2. From Corners to Blobs: The Anatomy of an Isotropic Feature (Slides 30–34, 46)
  
  While the Harris detector targets **corners** (junctions of intersecting directional edges), Slides 30–32 introduce a complementary visual primitive: **Blobs**.
  
  ```
        Corner Feature (Harris Operator)                 Blob Feature (Laplacian Operator)
     ┌──────────────────────────────────┐             ┌──────────────────────────────────┐
     │                │                 │             │                                  │
     │                │ Corner Vertex   │             │            ╭────────╮            │
     │          ──────┘ (1D Boundary    │             │            │  BLOB  │            │
     │                   Intersection)  │             │            │ (Center│            │
     │                                  │             │            ╰────────╯            │
     └──────────────────────────────────┘             └──────────────────────────────────┘
     • Localized by high gradient variance            • Localized by isotropic regional contrast
     • Directional: Sensitive to edge angles          • Omnidirectional: Circularly symmetric
  ```
  
  ---
### A. What is a "Blob"?
  A **blob** is an image region that is either significantly brighter or darker than its surrounding background. 
  * Unlike corners (which require sharp geometric edges), blobs represent **regions of central extremum** in intensity.
  * Examples include the centers of sunflowers (Slide 30), spots on a butterfly wing (Slides 51–54), biological cell nuclei, and traffic light signals.
  
  ---
### B. The 2D Laplacian as an Isotropic Curvature Detector (Slide 46)
  Recall from **Module M1.4** that the 2D continuous Laplacian operator is the sum of unmixed second partial derivatives:
  
  $$\nabla^2 I = \frac{\partial^2 I}{\partial x^2} + \frac{\partial^2 I}{\partial y^2}$$
  
  Slide 46 illustrates the geometric meaning of the second derivative:
  * $\frac{\partial^2 I}{\partial x^2} < 0$: The intensity profile curves downward along $x$ (concave downward, like a hilltop).
  * $\frac{\partial^2 I}{\partial x^2} > 0$: The intensity profile curves upward along $x$ (concave upward, like a valley).
  
  ```
        Bright Blob Cross-Section                       Dark Blob Cross-Section
              ^ Intensity I                                   ^ Intensity I
              │     ╭───╮ (Hilltop)                           │ ──╮       ╭──
              │   ╭─╯   ╰─╮                                   │   │       │   (Valley)
              │ ──╯       ╰──                                 │   ╰───────╯
              └───────────────► x                             └───────────────► x
              At Center:                                      At Center:
              First Derivative  dI/dx  = 0                    First Derivative  dI/dx  = 0
              Second Derivative d²I/dx² < 0                   Second Derivative d²I/dx² > 0
              Overall ∇²I < 0 (Negative Peak)                 Overall ∇²I > 0 (Positive Peak)
  ```
  
  ---
### C. The Polarity Convention: Why the Negative Sign Matters (Slide 46)
  By standard mathematical definition, a bright blob on a dark background produces a **negative Laplacian response** ($\nabla^2 I < 0$).
  
  To maintain the convention that a feature's presence yields a **positive detection peak**, computer vision defines the **Laplacian of Gaussian (LoG)** filter with an explicit leading negative sign (Slide 46):
  
  $$\text{LoG} = -\nabla^2 G$$
  
  With this sign inversion:
  * **Bright Blobs** produce large **positive** central peaks.
  * **Dark Blobs** produce large **negative** central troughs.
  
  ---
## 3. Matching the LoG Filter to Blobs: The Resonance Condition (Slides 35–43)
  
  Slide 35 illustrates what happens when the Laplacian of Gaussian operator is applied across a 1D pulse (a bright bar defined by two opposite step edges):
  
  ```
       Input Signal: Pulse of Width 2r                  LoG Kernel: Mexican Hat
            ^ Intensity                                          ^ LoG(x)
        1.0 │     ┌─────────┐                                1.0 │       ╭─╮ (Positive Center)
            │     │         │                                    │     ╭─╯ ╰─╮
            │     │         │                                0.0 ┼───╭─╯─────╰─╮───► x
        0.0 └─────┴─────────┴─────► x                            │ ╭─╯         ╰─╮ (Negative
                 -r        +r                               -0.5 ├─╯             ╰─ Rings)
  ```
  
  ---
### A. Edge Interference and Constructive Resonance (Slide 35)
  When the LoG kernel is centered directly over the pulse:
  * The left edge of the pulse (at $x = -r$) generates an oscillatory edge response.
  * The right edge of the pulse (at $x = +r$) generates an identical, mirrored edge response.
  * **Constructive Interference:** When the positive central peak of the LoG kernel matches the width of the pulse, the two edge responses **add together constructively**, producing a global maximum response at the center.
  * If the kernel is too small, it detects the two boundaries as separate, isolated edges.
  * If the kernel is too large, the pulse is smoothed into the background, attenuating the response.
  
  ---
### B. Mathematical Derivation: Finding the Zero-Crossing Radius (Slides 39–42)
  Slide 40 establishes the matching condition:
  > **The Boundary Alignment Condition:**
  > The filter response is maximized when the **zero-crossing contour of the Laplacian kernel aligns with the physical boundary of the circular blob**.
  
  ```
                   Aligning the Zero-Crossing to the Blob Edge (Slide 40)
                   
          Circular Blob of Radius r                     LoG Radial Profile
          ┌───────────────────────────┐                         ^ LoG(ρ)
          │           ╭───╮           │                         │       ╭─╮
          │         ╭─╯   ╰─╮         │                         │     ╭─╯ ╰─╮
          │        │    •    │ Radius │                         │    ╭╯     ╰╮
          │         ╰─╮   ╭─╯    r    │                     0.0 ┼───╭┴───────┴╮───► Radius ρ
          │           ╰───╯           │                         │  ╭╯         ╰╮
          └───────────────────────────┘                         └──╯           ╰──
                                                                   │◄─ ρ_zero ─►│
                                                                    Zero-Crossing must 
                                                                    match Blob Radius r!
  ```
  
  Let's derive the exact mathematical relationship step-by-step (Slide 42):
#### Step 1: Write the LoG in Radial Form
  The continuous 2D Gaussian in Cartesian coordinates is:
  
  $$G(x, y, \sigma) = \frac{1}{2\pi\sigma^2} \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right)$$
  
  Switching to polar coordinates where $\rho^2 = x^2 + y^2$:
  
  $$G(\rho, \sigma) = \frac{1}{2\pi\sigma^2} \exp\left( -\frac{\rho^2}{2\sigma^2} \right)$$
  
  Applying the radial Laplacian operator $\nabla^2 f = \frac{\partial^2 f}{\partial \rho^2} + \frac{1}{\rho}\frac{\partial f}{\partial \rho}$:
  
  $$\text{LoG}(\rho, \sigma) = -\nabla^2 G = \frac{1}{\pi\sigma^4} \left( 1 - \frac{\rho^2}{2\sigma^2} \right) \exp\left( -\frac{\rho^2}{2\sigma^2} \right)$$
  
  ---
#### Step 2: Solve for the Zero-Crossing $\rho_{\text{zero}}$
  A zero-crossing occurs at the radius $\rho$ where $\text{LoG}(\rho, \sigma) = 0$:
  
  $$\frac{1}{\pi\sigma^4} \left( 1 - \frac{\rho^2}{2\sigma^2} \right) \exp\left( -\frac{\rho^2}{2\sigma^2} \right) = 0$$
  
  Because the exponential term $\exp\left(-\frac{\rho^2}{2\sigma^2}\right)$ is strictly positive for all finite $\rho$:
  
  $$1 - \frac{\rho^2}{2\sigma^2} = 0$$
  
  $$\frac{\rho^2}{2\sigma^2} = 1 \implies \rho^2 = 2\sigma^2$$
  
  $$\rho_{\text{zero}} = \sqrt{2} \cdot \sigma$$
  
  ---
#### Step 3: Match the Filter Zero-Crossing to the Physical Blob Radius
  To maximize response amplitude, the zero-crossing radius must align with the physical boundary of the blob ($\rho_{\text{zero}} = r$):
  
  $$r = \sqrt{2} \cdot \sigma$$
  
  Solving for the optimal filter scale $\sigma$:
  
  $$\sigma = \frac{r}{\sqrt{2}} \approx 0.707 \cdot r$$
  
  $$r = \sqrt{2} \cdot \sigma \approx 1.414 \cdot \sigma$$
  
  ---
### C. Verification of the Numerical Experiment (Slides 36–38, 43)
  Slides 36–38 present an experiment:
  * An image contains synthetic circular bright disks of varying radii: $r \in \{5, 10, 15, 20\}\text{ pixels}$.
  * An unnormalized LoG filter with fixed standard deviation $\mathbf{\sigma = 7}$ is convolved across all circles.
  * Slide 38 asks: *"The peak occurs around $r = 10$: why?"*
#### Numerical Verification:
  Substitute $\sigma = 7$ into our derived formula:
  
  $$r_{\text{peak}} = \sqrt{2} \cdot \sigma = \sqrt{2} \cdot 7 = 1.4142 \times 7 = \mathbf{9.8995\text{ pixels}} \approx \mathbf{10\text{ pixels}}$$
  
  The theoretical derivation matches the experimental peak shown on Slide 38.
  
  ---
## 4. The Scale Normalization Imperative: $\sigma^2$-Normalized Laplacian (Slides 44–47)
  
  In realistic vision pipelines, we do not know the radius $r$ of objects in advance. We are given an image and must search over a range of filter scales $\sigma$ to discover feature sizes automatically.
  
  Slide 44 highlights a critical failure mode:
  > **The Decay Problem:**
  > As we increase the standard deviation $\sigma$ of the LoG filter, **the response of the unnormalized filter decays rapidly toward zero** across all scales.
  
  ```
       Unnormalized LoG Response vs. Scale σ (Slide 44)
       
       ^ Response Amplitude
       │  | (Sharp peak at σ=1)
       │  │
       │  │   ╭─╮ (Much lower at σ=2)
       │  │ ╭─╯ ╰─╮
       │  │╭╯     ╰╮   ╭───╮ (Near zero at σ=4)
       │  ││       │ ╭─╯   ╰─╮      ──────── (Completely flat at σ=8, 16!)
       └──┴┴───────┴─┴───────┴──────┴────────────────────────────────► Scale σ
          σ=1         σ=2           σ=4      σ=8      σ=16
          
          The unnormalized filter ALWAYS prefers tiny scales (σ → 0),
          making it impossible to detect large features!
  ```
  
  ---
### A. Mathematical Root Cause of Response Decay
  Why does an unnormalized Laplacian decay as $\sigma$ increases?
  
  1. **Dimensional Analysis of Derivatives:**
   Consider a 1D step transition scaled spatially by factor $\sigma$: $x \to \frac{x}{\sigma}$.
   By the chain rule, each spatial derivative scales inversely with $\sigma$:
  
  $$\frac{\partial}{\partial x} \sim \frac{1}{\sigma}, \quad \frac{\partial^2}{\partial x^2} \sim \frac{1}{\sigma^2}$$
  
   As a feature expands in scale, its spatial gradient slope flattens proportionally.
  2. **The Gaussian Amplitude Normalization:**
   The 2D continuous Gaussian kernel incorporates a leading normalization factor of $\frac{1}{2\pi\sigma^2}$ to maintain unit energy ($\iint G \, dx dy = 1$).
   When we compute its second derivatives analytically:
  
  $$\nabla^2 G_\sigma = \frac{\partial^2 G_\sigma}{\partial x^2} + \frac{\partial^2 G_\sigma}{\partial y^2} \sim \mathcal{O}\left(\frac{1}{\sigma^4}\right)$$
  
   Because the kernel values scale as $\frac{1}{\sigma^4}$ while the 2D spatial integration area grows as $\sigma^2$, **the peak response of an unnormalized LoG filter decays as $\mathcal{O}\left(\frac{1}{\sigma^2}\right)$**.
  
  As shown on Slide 44, a large circular blob of radius $r = 8$ will fail to produce a peak at $\sigma \approx 5.6$; the unnormalized response simply decreases monotonically as $\sigma$ increases.
  
  ---
### B. Lindeberg's Scale-Normalized Laplacian ($\text{LoG}_{\text{norm}}$) (Slide 45)
  In his foundational 1998 paper [1], **Tony Lindeberg** introduced **$\gamma$-scale-normalized derivatives** to ensure scale invariance:
  
  $$\partial_{x, \text{norm}} = \sigma^\gamma \frac{\partial}{\partial x}$$
  
  For second-order derivatives in 2D space, setting $\gamma = 1$ balances energy across scales:
  
  $$\nabla_{\text{norm}}^2 = \sigma^2 \nabla^2 = \sigma^2 \left( \frac{\partial^2}{\partial x^2} + \frac{\partial^2}{\partial y^2} \right)$$
  
  Multiplying the standard Laplacian by $\mathbf{\sigma^2}$ defines the **Scale-Normalized Laplacian of Gaussian ($\text{LoG}_{\text{norm}}$)**:
  
  $$\text{LoG}_{\text{norm}}(x, y, \sigma) = \sigma^2 \cdot \big(-\nabla^2 G_\sigma(x, y)\big)$$
  
  $$\text{LoG}_{\text{norm}}(x, y, \sigma) = \frac{\sigma^2}{\pi\sigma^4} \left( 1 - \frac{x^2 + y^2}{2\sigma^2} \right) \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right) = \frac{1}{\pi\sigma^2} \left( 1 - \frac{x^2 + y^2}{2\sigma^2} \right) \exp\left( -\frac{x^2 + y^2}{2\sigma^2} \right)$$
  
  ---
### C. Experimental Verification: Normalized vs. Unnormalized Response (Slide 47)
  Slide 47 displays the response curves for a 1D pulse of radius $r = 8$ evaluated across scales $\sigma \in [1.0, 16.0]$:
  
  ```
        Unnormalized Response (Slide 47 Top):
        ^ Response
        │  |
        │  │
        │  │
        0.0 └──┴────────────────────────────────► Scale σ
            σ=1.0  2.0   4.0   8.0   16.0
            (Monotonically decays toward zero!)
        
        Scale-Normalized Response (Slide 47 Bottom):
        ^ Response
        │                     • MAXIMUM PEAK at σ = 8.0!
        │                   ╭─╯ ╰─╮
        │                 ╭─╯     ╰─╮
        │  ╭─╮          ╭─╯         ╰─╮
        0.0 ┴─┴──────────┴─────────────┴────────► Scale σ
            σ=1.0  2.0   4.0   8.0   16.0
            (Clean, distinctive scale extremum achieved!)
  ```
  
  * **The Result:** Multiplying by $\sigma^2$ balances the $1/\sigma^2$ amplitude decay. 
  * The response curve achieves a clear, distinct **local maximum precisely at the characteristic scale of the feature**.
  
  ---
## Summary Matrix: Unnormalized vs. Scale-Normalized Blob Operators
  
  | Property | Unnormalized LoG ($-\nabla^2 G$) | Scale-Normalized LoG ($\sigma^2 \nabla^2 G$) |
  | :--- | :--- | :--- |
  | **Mathematical Formula** | $\frac{1}{\pi\sigma^4} \left( 1 - \frac{\rho^2}{2\sigma^2} \right) e^{-\frac{\rho^2}{2\sigma^2}}$ | $\frac{\mathbf{\sigma^2}}{\pi\sigma^4} \left( 1 - \frac{\rho^2}{2\sigma^2} \right) e^{-\frac{\rho^2}{2\sigma^2}} = \frac{1}{\pi\mathbf{\sigma^2}} \left( 1 - \frac{\rho^2}{2\sigma^2} \right) e^{-\frac{\rho^2}{2\sigma^2}}$ |
  | **Amplitude Scaling** | Decays as $\mathcal{O}(1/\sigma^2)$ | Invariant across all spatial scales ($\mathcal{O}(1)$) |
  | **Zero-Crossing Radius** | $\rho_{\text{zero}} = \sqrt{2} \cdot \sigma$ | $\rho_{\text{zero}} = \sqrt{2} \cdot \sigma$ (Unchanged) |
  | **Optimal Blob Scale** | N/A (Always decays toward 0) | $\mathbf{\sigma^* = \frac{r}{\sqrt{2}}}$ |
  | **Scale Selection Capability** | **Fails** (Cannot find characteristic scale) | **Succeeds** (Produces prominent peak at true scale) |
  
  ---