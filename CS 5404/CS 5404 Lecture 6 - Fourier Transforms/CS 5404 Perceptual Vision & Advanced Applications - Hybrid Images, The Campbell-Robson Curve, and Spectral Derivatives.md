## 1. The Human Visual Perception System & Spatial Frequency (Slides 77–79)
  
  Before constructing computational frequency-domain applications, we must examine how the biological human visual system (HVS) perceives spatial frequencies.
  
  Slide 78 and 79 introduce the classic **Campbell-Robson contrast sensitivity chart** (1968) [4]:
  
  ```
                   The Campbell-Robson Contrast Sensitivity Chart
                   
      ^ 1 / Contrast (Log Scale)
      │
  Low │                       ╭────────────────╮
  Cont- │                     ╭─╯  PEAK HUMAN    ╰─╮
  rast  │                   ╭─╯     SENSITIVITY    ╰─╮
      │                 ╭─╯      (~3-6 cycles/deg) ╰─╮
      │               ╭─╯                             ╰─╮
  High │             ╭─╯                                 ╰─╮
  Cont- │           ╭─╯                                     ╰─╮
  rast  │ ──────────┴─────────────────────────────────────────┴─────────►
      │ Low Frequencies                               High Frequencies
      0 (Broad illumination)                          (Fine details)
                                Spatial Frequency (Log Scale)
  ```
  
  ---
### A. The Contrast Sensitivity Function (CSF)
  The chart varies spatial frequency along the horizontal axis (increasing from left to right) and contrast along the vertical axis (decreasing from bottom to top). 
  
  The visible boundary of the stripes forms an **inverted-U curve**:
  1. **Low-Frequency Cutoff (Left):**
   Human contrast sensitivity falls off significantly at very low spatial frequencies. This attenuation is caused by **retinal lateral inhibition**—horizontal cells in the retina subtract neighboring photoreceptor signals, acting as an analog biological band-pass filter that ignores uniform ambient illumination.
  2. **High-Frequency Cutoff (Right):**
   Sensitivity drops sharply at high spatial frequencies due to physical optics:
   * Diffraction and optical aberrations across the cornea, pupil, and lens.
   * The finite spatial packing density of photoreceptor cones in the fovea centralis.
  3. **The Visual Sweet Spot (The Peak):**
   Human eyes are maximally sensitive to **mid-range spatial frequencies**—specifically between **$3$ and $6\text{ cycles per degree (cpd)}$** of visual angle. 
  
  > **Core Perceptual Takeaway (Slide 79):** 
  > Human visual cognition does not weight all Fourier frequencies equally. When processing complex natural scenes, **mid-range spatial frequency cues dominate perception**.
  
  ---
## 2. Multi-Scale Art & The Science of Hybrid Images (Slides 80–84)
  
  ---
### A. Artistic Precedents: Salvador Dalí (Slides 80–81)
  Decades before computer vision formalized multi-frequency synthesis, Surrealist artist Salvador Dalí exploited the frequency-dependent nature of human vision in masterpieces like *Skull of Zurbaran* (Slide 80) and *Gala Contemplating the Mediterranean Sea...*:
  * **Viewed Up Close (High Spatial Frequencies):** The retina resolves fine lines, sharp edges, and individual figures (monks dressed in robes praying inside an arched cathedral hall).
  * **Viewed From a Distance (Low Spatial Frequencies):** High frequencies exceed the eye's angular resolution limit and vanish; only broad, diffuse luminance fields survive, revealing a giant human skull.
  
  ---
### B. The Hybrid Image Formulation: Oliva, Torralba, & Schyns (2006) (Slides 82–84)
  Slide 82 references the landmark SIGGRAPH 2006 paper by **Aude Oliva, Antonio Torralba, and Philippe G. Schyns** [5]:
  
  ```
    Image 1 (Low-Pass Filtered)                 Image 2 (High-Pass Filtered)
          Low Frequencies Only                        High Frequencies Only
    ┌──────────────────────────────┐            ┌──────────────────────────────┐
    │     ╭──╮                     │            │    \ | /                     │
    │   ╭─╯  ╰─╮ (e.g., Happy Face)│     +      │   - * -  (e.g., Sad Face)   │
    │   ╰──────╯                   │            │    / | \                     │
    └──────────────────────────────┘            └──────────────────────────────┘
                   │                                           │
                   └─────────────────────┬─────────────────────┘
                                         ▼
                           [ Composite Hybrid Image ]
                           
      • Viewed Up Close (Small Distance d):
        High frequencies fall into peak human sensitivity.
        Result: Observer perceives the SAD face!
        
      • Viewed From Afar (Large Distance d):
        High frequencies exceed retinal resolution cutoff.
        Low frequencies shift into peak human sensitivity.
        Result: Observer perceives the HAPPY face!
  ```
#### The Physics of Viewing Distance and Retinal Frequency:
  Let an image contain a physical spatial frequency $\omega_{\text{image}}$ (measured in cycles per pixel). When an observer views the image at a distance $d$ (meters), the **angular spatial frequency $\omega_{\text{retina}}$** projected onto the human retina (measured in cycles per degree of visual angle) is:
  
  $$\omega_{\text{retina}} \approx \omega_{\text{image}} \cdot \frac{d \cdot \pi}{180 \cdot p}$$
  
  where $p$ is the physical pixel pitch on the display.
  * **As viewing distance $d$ increases**, all frequency components shift linearly toward higher retinal frequencies.
  * High-frequency details (such as Einstein's wrinkles and mustache in Slide 84) are pushed beyond the human optical cutoff ($\omega_{\text{retina}} > 40\text{ cpd}$) and become completely invisible.
  * Meanwhile, low-frequency shapes (such as Marilyn Monroe's soft hair contours and smile) shift into the eye's optimal sensitivity band ($3\text{--}6\text{ cpd}$), transforming the perceived identity.
  
  ---
## 3. Engineering Hybrid Images: Programming Assignment 1 Guide (Slide 85)
  
  Slide 85 outlines the requirements for **Programming Assignment 1**:
  
  $$\mathbf{I}_{\text{hybrid}} = \mathbf{I}_{\text{low}} + \mathbf{I}_{\text{high}}$$
  
  $$\mathbf{I}_{\text{hybrid}} = \big(\mathbf{I}_1 * G_{\sigma_{\text{low}}}\big) + \Big(\mathbf{I}_2 - \big(\mathbf{I}_2 * G_{\sigma_{\text{high}}}\big)\Big)$$
  
  ```
                            Hybrid Image Pipeline (PA1)
                            
   Image 1 (Base/Coarse) ──► [ Gaussian Blur G_σ_low ] ──────┐
                                                              │
                                                              ▼
                                                             (+) ──► I_hybrid
                                                              ▲
   Image 2 (Detail/Fine) ──► [ Original - G_σ_high ] ─────────┘
                              High-Pass Residual
  ```
  
  ---
### Step-by-Step Implementation Architecture:
  1. **Geometric Alignment (Rigid Registration):**
   The two images must be aligned before filtering. Eye centers, mouth lines, and facial aspect ratios must match. If faces are misaligned, the low-frequency and high-frequency components will clash rather than fuse perceptually.
  2. **Low-Pass Channel Selection ($\mathbf{I}_1$):**
   Convolve Image 1 with a Gaussian kernel parameterized by standard deviation $\sigma_{\text{low}}$:
  
  $$\mathbf{I}_{\text{low}} = \mathbf{I}_1 * G_{\sigma_{\text{low}}}$$
  
   * Cutoff guideline: $\sigma_{\text{low}}$ should be chosen large enough to eliminate all sharp edges while preserving broad illumination and facial volume (typically $\sigma_{\text{low}} \approx 7\text{--}15\text{ pixels}$).
  3. **High-Pass Channel Selection ($\mathbf{I}_2$):**
   Extract the high-frequency detail of Image 2 via unsharp masking:
  
  $$\mathbf{I}_{\text{high}} = \mathbf{I}_2 - (\mathbf{I}_2 * G_{\sigma_{\text{high}}})$$
  
   * Cutoff guideline: $\sigma_{\text{high}}$ must be smaller than $\sigma_{\text{low}}$ (typically $\sigma_{\text{high}} \approx 3\text{--}6\text{ pixels}$) to retain only crisp contours (hair strands, wrinkles, glasses frames).
  4. **Photometric Blending & Dynamic Range Management:**
   Because $\mathbf{I}_{\text{high}}$ contains negative values (it is a zero-centered residual), adding it directly to $\mathbf{I}_{\text{low}}$ can cause pixel underflow ($<0$) or overflow ($>1$ or $>255$). 
   * Always compute in `float32`.
   * Apply final saturation clamping:
  
  $$\mathbf{I}_{\text{hybrid}} = \text{clamp}\big(\mathbf{I}_{\text{low}} + \mathbf{I}_{\text{high}}, \, 0.0, \, 1.0\big)$$
  
  > **Instructor Note from Slide 85:** 
  > You do **not** need to compute explicit Forward and Inverse 2D FFTs to build a hybrid image. By the Convolution Theorem, convolving directly in the spatial domain with Gaussian kernels achieves the exact same mathematical result.
  
  ---
## 4. Spectral Differentiation: The Fourier Derivative Theorem (Slides 92–96)
  
  In **Module M1.4**, we approximated spatial derivatives using discrete finite differences:
  
  $$f'(x) \approx \frac{f(x + 1) - f(x - 1)}{2}$$
  
  While finite differences are computationally cheap, they suffer from truncation error ($\mathcal{O}(h^2)$) and high-frequency noise sensitivity. 
  Fourier analysis provides an exact, analytical method for evaluating continuous derivatives of band-limited discrete signals.
  
  ---
### A. Mathematical Derivation of the Derivative Property (Slide 92)
  Let $f(x)$ be a continuous function with Fourier transform $F(\omega)$. By the definition of the Inverse Continuous Fourier Transform:
  
  $$f(x) = \int_{-\infty}^\infty F(\omega) \, e^{j 2\pi \omega x} \, d\omega$$
  
  Differentiating both sides with respect to spatial coordinate $x$ using Leibniz's integral rule:
  
  $$\frac{d}{dx} f(x) = \frac{d}{dx} \left[ \int_{-\infty}^\infty F(\omega) \, e^{j 2\pi \omega x} \, d\omega \right] = \int_{-\infty}^\infty F(\omega) \left[ \frac{\partial}{\partial x} e^{j 2\pi \omega x} \right] d\omega$$
  
  Taking the spatial derivative of the complex exponential basis function:
  
  $$\frac{\partial}{\partial x} e^{j 2\pi \omega x} = \big(j 2\pi \omega\big) \cdot e^{j 2\pi \omega x}$$
  
  Substituting back into the inverse transform integral:
  
  $$\frac{d}{dx} f(x) = \int_{-\infty}^\infty \Big( j 2\pi \omega \cdot F(\omega) \Big) e^{j 2\pi \omega x} \, d\omega$$
  
  Recognizing the right-hand side as an Inverse Fourier Transform:
  
  $$\mathcal{F}\left\{ \frac{d}{dx} f(x) \right\} = j 2\pi \omega \cdot F(\omega)$$
  
  $$\frac{d^n}{dx^n} f(x) = \mathcal{F}^{-1}\left\{ (j 2\pi \omega)^n \cdot F(\omega) \right\}$$
  
  ```
                   The Spectral Derivative Principle
                   
   Spatial Domain: Differentiate d/dx (Complex spatial calculus)
   ─────────────────────────────────────────────────────────────►
   Frequency Domain: Multiply by jω (Simple algebraic multiplication!)
  ```
  
  ---
### B. Physical and Geometric Interpretation
  Multiplying the Fourier spectrum by the differential operator factor $j 2\pi \omega$ applies two distinct transformations:
  1. **Magnitude Scaling ($2\pi |\omega|$):**
   The magnitude is multiplied linearly by frequency $\omega$. Low frequencies (near $\omega = 0$) are attenuated, while high frequencies are amplified proportionally to their oscillation rate.
  2. **Phase Rotation ($j = e^{j \pi / 2}$):**
   Multiplying by the imaginary unit $j$ introduces a **constant $+90^\circ$ phase shift** across all spectral components. This matches trigonometric calculus:
  
  $$\frac{d}{dx} \sin(\omega x) = \omega \cos(\omega x) = \omega \sin\left(\omega x + \frac{\pi}{2}\right)$$
  
  ---
## 5. Discrete Spectral Derivatives: The "k-Vector" Implementation (Slides 97–100)
  
  Slide 93–96 establishes the discrete counterpart for an $N$-point sequence $f[x]$ sampled over a domain of physical length $L$:
  
  $$f[x] = \frac{1}{N} \sum_{k=0}^{N-1} F[k] \, e^{j \frac{2\pi k x}{N}}$$
  
  $$\frac{d}{dx} f[x] = \frac{1}{N} \sum_{k=0}^{N-1} \left( j \frac{2\pi k}{N \cdot \Delta x} \right) F[k] \, e^{j \frac{2\pi k x}{N}}$$
  
  Slide 98 issues an important warning:
  > *"It seems simple, but there are some idiosyncrasies in practice you will need to think about... for example: how do you define the 'k' vector?"*
  
  ---
### The Frequency Ordering Trap (Slide 97, 98)
  In standard software implementations (like `np.fft.fft`), the frequency array is **not** monotonically increasing from $0$ to $N-1$:
  
  ```
        Naive (Incorrect) Vector:   [ 0,  1,  2,  3, ..., N-2, N-1 ]  <-- WRONG!
        
        True FFT Frequency Order:   [ 0,  1,  2, ..., N/2, -(N/2-1), ..., -2, -1 ]
                                    └────────┬──────────┘  └──────────┬───────────┘
                                     Positive Frequencies     Negative Frequencies
  ```
  
  If you multiply negative-frequency bins by positive integers $k \in [\frac{N}{2}+1, N-1]$, you invert the mathematical direction of differentiation, yielding garbage outputs.
  
  ---
### Constructing the Correct Angular Frequency Vector $\mathbf{\omega}$ (Slide 100)
  For a signal of $N$ samples spanning physical domain length $L$, the physical sampling interval is $\Delta x = \frac{L}{N}$. 
  
  To construct the correct angular frequency vector $\omega$:
  
  ```python
  import numpy as np
  
  # 1. Normalized frequencies in cycles per sample: [-0.5, +0.5)
  freqs_cycles_per_sample = np.fft.fftfreq(N, d=1.0)
  
  # 2. Scale by sampling rate (N / L) and 2*pi to obtain radians per unit length
  omega = freqs_cycles_per_sample * (N / L) * 2.0 * np.pi
  ```
  
  ---
### The Nyquist Singularity for Even $N$
  When $N$ is even, the Nyquist frequency bin at index $k = N/2$ represents the frequency $\omega_{\text{Nyquist}} = \pm \frac{\pi}{\Delta x}$. 
  * Because this component lies on the boundary between positive and negative realms, its imaginary derivative weight must be set to zero:
  
  $$\omega\left[\frac{N}{2}\right] = 0.0$$
  
  Setting this term to zero prevents an asymmetrical phase error that would otherwise introduce spurious imaginary residual artifacts into real-valued derivatives.
  
  ---
### Breakout Demo Code Walkthrough (Slide 100)
  Slide 100 presents the complete verified demo solution comparing the analytical derivative of a test sinusoid against its Fourier-computed derivative:
  
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  
  # 1. Define Signal Parameters
  L = 1.0                           # Total domain length (meters)
  N = 101                           # Number of spatial discrete samples (Odd N)
  x = np.arange(0, L, L / N)        # Spatial coordinate array
  
  # 2. Generate Ground-Truth Test Signal: 4 Hz Sinusoid
  signal_x = np.sin(2.0 * np.pi * 4.0 * x / L)
  
  # 3. Ground-Truth Analytical Derivative: d/dx sin(ωx) = ω cos(ωx)
  d_signal_x_analytic = (2.0 * np.pi * 4.0 / L) * np.cos(2.0 * np.pi * 4.0 * x / L)
  
  # 4. Construct Exact Fourier Angular Frequency Vector
  # np.fft.fftfreq(N) generates: [0, 1, ..., (N-1)/2, -(N-1)/2, ..., -1] / N
  w = np.fft.fftfreq(N) * N * (2.0 * np.pi / L)
  
  # 5. Compute Derivative via Fourier Multiplication
  # Steps: Forward FFT -> Multiply by (1j * w) -> Inverse FFT
  F_signal = np.fft.fft(signal_x)
  d_signal_x_fft = np.fft.ifft(1j * w * F_signal)
  
  # 6. Extract Real Component (Discard floating-point machine epsilon imaginary noise)
  d_signal_x_fft = np.real(d_signal_x_fft)
  
  # 7. Verification: Numerical match
  max_absolute_error = np.max(np.abs(d_signal_x_analytic - d_signal_x_fft))
  print(f"Maximum Error between Analytic and FFT Derivative: {max_absolute_error:.2e}")
  assert np.isclose(max_absolute_error, 0.0, atol=1e-12)
  ```
  
  ---
## 6. Complete Mathematical & Algorithmic Summary of Module M2.3
  
  ```
                             The Fourier Transforms Landscape
                                             │
      ┌──────────────────────────────────────┼──────────────────────────────────────┐
      ▼                                      ▼                                      ▼
  [ Foundations (Part 1) ]             [ Filtering (Part 2) ]                 [ Applications (Part 3) ]
  • Basis: e^{jωx}                     • Convolution Theorem:                 • Human Vision (CSF):
  • Orthogonality: No half-freqs         F{f * g} = F · G                       Peak at 3–6 cycles/deg
  • Nyquist: f_s ≥ 2f_max              • Reciprocal Scaling:                  • Hybrid Images:
  • Real signals: Symmetric ±f           Wide in space = Narrow in freq         Low-pass(A) + High-pass(B)
  • Centering via fftshift             • Low-Pass (Blur) vs High-Pass (Detail)• Spectral Differentiation:
                                     • Sinc interpolation via zero-padding    d/dx ──► Multiply by jω
  ```
  
  ---
## Complete Unit 1 Synthesis: The Computer Vision Toolchain
  
  With Module M2.3 complete, we have assembled the foundational mathematical toolchain for 2D image analysis:
  
  ```
  ┌───────────────────────────┬───────────────────────────────┬─────────────────────────────────────────┐
  │ Module                    │ Mathematical Foundation       │ Core Operational Capability             │
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **M1.3: Image Basics**    │ Point operators $T(f(x, y))$  │ Dynamic range, gamma, contrast, LUTs    │
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **M1.4: Filtering**       │ Spatial convolution $f * h$   │ Smoothing (Gaussian), edges (Sobel, LoG)│
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **M2.1: Image Pyramids**  │ Burt & Adelson REDUCE/EXPAND  │ Multi-scale analysis, lossless residuals│
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **M2.2: Upsampling**      │ Continuous reconstruction $h$ │ Bilinear, bicubic, and pull-resampling  │
  ├───────────────────────────┼───────────────────────────────┼─────────────────────────────────────────┤
  │ **M2.3: Fourier Analysis**│ Spectral duality $F(\omega)$  │ Bandwidth, anti-aliasing, hybrid images │
  └───────────────────────────┴───────────────────────────────┴─────────────────────────────────────────┘
  ```
  
  This concludes our comprehensive study notes for **Module M2.3 (Fourier Transforms)** and completes the core analytical toolset for 2D image signals in Unit 1. 
  
  Whenever you are ready, upload the slides for the next topic!
  
  ---