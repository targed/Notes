## 1. Motivation: The Problem of Scale in Computer Vision (Slides 2–5)
  
  In physical three-dimensional reality, objects maintain fixed metric dimensions (size constancy). However, under central perspective projection, an object's footprint on a camera sensor scales inversely with its distance $Z$ along the optical axis:
  
  $$s \propto \frac{f}{Z}$$
  
  ```
    Proximal Object (Z = 1 m)                      Distal Object (Z = 4 m)
  ┌───────────────────────────┐                  ┌───────────────────────────┐
  │          ╭─────╮          │                  │                           │
  │          │  *  │          │                  │          ╭─╮              │
  │          │ / \ │          │                  │          ╰─╯              │
  │          ╰─────╯          │                  │                           │
  └───────────────────────────┘                  └───────────────────────────┘
       Large Sensor Footprint                         Small Sensor Footprint
       High-Frequency Texture                         Texture Attenuated / Lost
  ```
  
  ---
### A. The Matching Dilemma: Why Fixed Differential Features Fail (Slides 3–5)
  Slides 3 and 4 present a scenario: two images contain the exact same physical objects (a set of decorative vases on a ledge), but the camera has moved significantly farther away in the second image. 
  
  Can we match these objects across images using the differential operators we developed in **Module M1.4** (derivatives, gradients, curvatures)?
#### 1. Why Image Derivatives are NOT Scale-Invariant:
  Recall the definition of the continuous image gradient magnitude:
  
  $$\|\nabla f(x, y)\| = \sqrt{\left(\frac{\partial f}{\partial x}\right)^2 + \left(\frac{\partial f}{\partial y}\right)^2}$$
  
  * If an image is scaled down spatially by factor $s$ such that $f_{\text{scaled}}(x, y) = f(s \cdot x, s \cdot y)$ where $s < 1$:
  
  $$\frac{\partial f_{\text{scaled}}}{\partial x} = s \cdot \frac{\partial f}{\partial x}(sx, sy)$$
  
  * The gradient magnitude scales directly with $s$. A sharp intensity transition that spans a width of $10\text{ pixels}$ up close will span only $2\text{ pixels}$ at a distance. 
  * A fixed $3 \times 3$ Sobel filter computes derivatives across completely different physical surface areas when applied to the two images.
#### 2. Why Curvature / Second Derivatives are NOT Scale-Invariant:
  The second-order Hessian matrix $\mathbf{H} = \begin{bmatrix} f_{xx} & f_{xy} \\ f_{xy} & f_{yy} \end{bmatrix}$ scales with $s^2$:
  
  $$\frac{\partial^2 f_{\text{scaled}}}{\partial x^2} = s^2 \cdot \frac{\partial^2 f}{\partial x^2}(sx, sy)$$
  
  Differential edge detectors, corner detectors, and template matchers designed at a single fixed pixel radius fail whenever the camera changes distance.
  
  > **Takeaway (Slide 5):** Handling arbitrary geometric scale directly inside single-scale image space is mathematically difficult. Instead of designing complex scale-invariant operators for every possible scale, it is often simpler and more computationally efficient to **resample the image across multiple scales**.
  
  ---
## 2. Naive Downsampling: Sub-Sampling / Decimation (Slides 8–10)
  
  What is the most direct way to shrink an image of size $M \times N$ by an integer factor $K$?
### A. The Sub-Sampling (Decimation) Algorithm
  The simplest approach is **uniform sub-sampling**: discard all rows and columns except those that are integer multiples of $K$:
  
  $$I_{\text{sub}}[r, c] = I_{\text{orig}}[K \cdot r, \, K \cdot c]$$
  
  For a downsampling factor of $K = 2$, this operation retains only $1$ out of every $4$ pixels (discarding $75\%$ of the data):
  
  ```
       Original 4×4 Grid                         Sub-Sampled 2×2 Grid (K=2)
     ┌─────┬─────┬─────┬─────┐
  r=0 │ P00 │ P01 │ P02 │ P03 │
     ├─────┼─────┼─────┼─────┤                      ┌─────┬─────┐
   1 │ P10 │ P11 │ P12 │ P13 │    Keep (2r, 2c)     │ P00 │ P02 │
     ├─────┼─────┼─────┼─────┤   ──────────────►    ├─────┼─────┤
   2 │ P20 │ P21 │ P22 │ P23 │    Discard Rest      │ P20 │ P22 │
     ├─────┼─────┼─────┼─────┤                      └─────┴─────┘
   3 │ P30 │ P31 │ P32 │ P33 │
     └─────┴─────┴─────┴─────┘
  ```
  
  ```python
  import numpy as np
  
  # Naive 2x downsampling using slice striding
  img_downsampled = img_orig[::2, ::2]
  
  # Naive 4x downsampling
  img_downsampled_4x = img_orig[::4, ::4]
  ```
  
  ---
### B. The Breakdown: Graininess and Degradation (Slides 11–12)
  Slides 11 and 12 show Vincent van Gogh’s *Self-Portrait* decimated by factors of $1/2$, $1/4$, and $1/8$:
  * At $1/2$, the image appears slightly softer.
  * At $1/4$ and $1/8$, zooming back in reveals **pixelation, high-frequency noise, and a speckled "grainy" texture** across Van Gogh’s jacket and beard.
  * Instead of smoothly fading, fine brushstrokes turn into chaotic, disconnected clusters of light and dark pixels.
  
  ---
## 3. Visual Artifacts: Moiré Patterns and Aliasing (Slides 13–15)
  
  The failure of naive downsampling is even more pronounced on high-frequency, periodic patterns.
  
  ---
### A. Moiré Patterns on Architectural Facades (Slides 13, 15)
  Slide 13 shows a high-resolution photograph of a modern skyscraper covered in thousands of fine horizontal window louvers:
  1. **Full-Resolution Image:** The facade features uniform, repeating vertical and horizontal architectural mullions.
  2. **Sub-Sampled Image:** The uniform architectural grid is replaced by wavy, curving, low-frequency geometric ripples called **Moiré patterns**.
  3. **The Artifact:** The camera / downsampling pipeline introduced **large, macro-scale ripples that do not exist in the physical scene**.
  
  ```
   High-Frequency Scene Pattern:   ||||||||||||||||||||||||||||||||  (Fine stripes)
   Coarse Sampling Grid:           ·   ·   ·   ·   ·   ·   ·   ·  (Samples miss peaks)
                                   ────────────────────────────────
   Spurious Reconstructed Pattern: ╭───╮   ╭───╮   ╭───╮   ╭───╮  (False low-frequency wave!)
                                   Moiré / Aliased Low Frequency
  ```
  
  ---
### B. Perspective Aliasing: The Checkerboard Illusion (Slide 14)
  Slide 14 displays an infinite 3D checkerboard plane receding into the distance:
  * **Foreground:** Individual black and white squares are large relative to the pixel grid. They sample cleanly.
  * **Horizon (Background):** Perspective projection compresses hundreds of alternating black and white tiles into a fraction of a millimeter. 
  * **The Glitch:** Under naive sampling, pixels randomly sample either a black or white square, producing chaotic, jagged artifacts near the horizon rather than a smooth, uniform gray.
  
  ---
## 4. The Signal Processing Theory: The Nyquist-Shannon Sampling Theorem (Slide 16)
  
  To understand why these patterns appear, we must examine the **Nyquist-Shannon Sampling Theorem** [1].
  
  ```
                     Continuous Signal f(x)
                                │
                                ▼
                   [ Fourier Transform F(ω) ]
                                │
               ┌────────────────┴────────────────┐
               ▼                                 ▼
   Case 1: ω_max < ω_s / 2           Case 2: ω_max > ω_s / 2
   (Nyquist Criterion Satisfied)     (Under-sampled: ALIASING)
   
        ^ Amplitude                       ^ Amplitude
        │   ╭─╮       ╭─╮                 │   ╭─╮   ╭─╮
     ───┼──╭╯ ╰╮─────╭╯ ╰╮───► ω       ───┼──╭╯ ╰X━X╯ ╰╮───► ω
        │ ╭╯   ╰╮   ╭╯   ╰╮               │ ╭╯  ╱ █ ╲  ╰╮
       -ω_s/2  +ω_s/2    ω_s             -ω_s/2   █   +ω_s/2
       Clean spectral replicas            Spectral Overlap! High frequencies 
       No overlap = Perfect recovery      fold back as false low frequencies!
  ```
  
  ---
### A. The Theorem Defined
  Let a continuous 1D spatial signal $f(x)$ be band-limited with a maximum spatial frequency $\omega_{\max}$ (measured in cycles per unit length). 
  
  > **The Nyquist Sampling Criterion:**
  > To capture and reconstruct a continuous signal without loss of information or geometric distortion, the spatial sampling rate $\omega_s$ (samples per unit length) must be **strictly greater than twice the highest frequency present in the signal**:
  
  $$\omega_s > 2 \cdot \omega_{\max}$$
  
  The threshold $\omega_{\text{Nyquist}} = \frac{\omega_s}{2}$ is the **Nyquist Frequency**.
  
  ---
### B. The Frequency-Domain View of Sampling
  Sampling a continuous signal $f(x)$ at spatial intervals of $\Delta x = \frac{1}{\omega_s}$ is mathematically equivalent to multiplying $f(x)$ by a Dirac comb (an infinite train of impulse functions) $\text{III}_{\Delta x}(x)$:
  
  $$f_s(x) = f(x) \cdot \sum_{n=-\infty}^\infty \delta(x - n\Delta x)$$
  
  By the Convolution Theorem of Fourier transforms, multiplication in the spatial domain corresponds to **periodic convolution in the spatial-frequency domain**:
  
  $$F_s(\omega) = \frac{1}{\Delta x} \sum_{k=-\infty}^\infty F\left(\omega - k \cdot \omega_s\right)$$
  
  Sampling causes the continuous baseband spectrum $F(\omega)$ to **replicate infinitely along the frequency axis**, spaced at integer multiples of the sampling frequency $\omega_s$.
  
  ---
### C. The Origin of Aliasing: Spectral Overlap & Frequency Folding
  What happens when downsampling violates the Nyquist limit ($\omega_s < 2 \omega_{\max}$)?
  
  1. The spectral copies centered at $\omega = 0$ and $\omega = \omega_s$ **overlap**.
  2. Frequencies higher than the Nyquist limit ($\omega > \omega_{\text{Nyquist}}$) fold back across the Nyquist boundary:
  
  $$\omega_{\text{alias}} = |\omega - k \cdot \omega_s|$$
  
  3. A high-frequency signal component $\omega_{\text{high}}$ masquerades as an artificial, non-existent low-frequency component $\omega_{\text{alias}}$ (an **alias**).
  4. **Physical Manifestation:** The fine, rapid stripes of the skyscraper louvers fold down into the broad, curving waves of the Moiré pattern.
  
  ---
## 5. Physical Sensor Integration vs. Algorithmic Subsampling (Slides 17–20)
  
  Slide 17 asks:
  > **"If the smaller image were taken by a real camera, would we see the same moiré effect?"**
  > 
  > *Answer:* **No (or significantly less so)**, because physical camera sensors integrate light over a finite photosensitive area.
  
  ```
       Ideal Point Subsampling                         Physical CCD/CMOS Photodiode
     (What numpy [::2, ::2] does)                      (What a real digital camera does)
            ┌───┬───┬───┬───┐                                 ┌───────────────┐
            │   │   │   │   │                                 │ ░░░░░░░░░░░░░ │  Integrates ALL photons
            ├───┼───┼───┼───┤                                 │ ░░ Photodiode░│  arriving across the 
            │   │ • │   │   │ ◄── Point Sample                │ ░░ Active Area│  entire pixel surface!
            ├───┼───┼───┼───┤     Single coordinate           │ ░░░░░░░░░░░░░ │  Acts as an analog
            │   │   │   │   │                                 └───────────────┘  low-pass box filter!
            └───┴───┴───┴───┘
  ```
  
  1. **Algorithmic Sub-Sampling:** Reads only a single infinitesimal coordinate $(2r, 2c)$, discarding all light and information in the surrounding $K \times K$ neighborhood.
  2. **Physical Sensor Integration:** A physical camera pixel has a finite surface area $\mathcal{A}$. During the exposure interval, it integrates all incident photons landing across $\mathcal{A}$:
  
  $$I_{\text{sensor}}[r, c] = \iint_{\mathcal{A}_{r,c}} E(x, y) \, dx \, dy$$
  
  This spatial integration acts as an **analog box filter**, smoothing out frequencies higher than the sensor's physical pixel pitch before sampling occurs.
  
  3. **Optical Low-Pass Filters (OLPF):** Most professional DSLRs include a physical birefringent anti-aliasing filter (lithium niobate crystal) directly in front of the silicon sensor. This optical element splits light rays slightly, blurring detail above the sensor's Nyquist limit to prevent Moiré patterns in fabrics and architecture.
  
  ---
## 6. The Anti-Aliasing Solution: Gaussian Pre-filtering (Slides 21–24)
  
  To resize digital images cleanly, software must emulate the physics of optical sensors by **filtering before downsampling**.
  
  ```
                   Anti-Aliasing Image Downsampling Pipeline
                   
   Input Image f ──► [ Low-Pass Filter G_σ ] ──► [ Sub-Sample by 2 ] ──► Output Image g
                     Attenuates frequencies      Discards every other
                     above new Nyquist limit     row and column safely!
  ```
  
  ---
### A. The Two-Step Anti-Aliasing Algorithm
  To reduce an image by an integer factor of $K$:
  1. **Low-Pass Filter (Anti-Aliasing Stage):** Convolve the original image with a low-pass smoothing kernel (such as a Gaussian) whose cutoff frequency matches the new, lower Nyquist limit:
  
  $$\tilde{f}[m, n] = (f * G_\sigma)[m, n]$$
  
  2. **Downsample (Decimation Stage):** Subsample the filtered image without introducing aliasing:
  
  $$g[r, c] = \tilde{f}[K \cdot r, \, K \cdot c]$$
  
  ---
### B. Visual Comparison: Prefiltering vs. Naive Subsampling (Slides 21–23)
  Slides 22 and 23 compare the Van Gogh portrait under both approaches:
  
  ```
  ┌───────────────────────────┬─────────────────────────────────────────────────────────┐
  │ Method                    │ Visual Characteristics                                  │
  ├───────────────────────────┼─────────────────────────────────────────────────────────┤
  │ Naive Subsampling         │ • High-frequency brushstrokes alias into dark speckles. │
  │ (Slides 12, 23)           │ • Facial features appear pixelated and harsh.           │
  │                           │ • Hair and eyes contain noise artifacts.                │
  ├───────────────────────────┼─────────────────────────────────────────────────────────┤
  │ Gaussian Prefiltered      │ • Preserves smooth tonal transitions.                   │
  │ (Slides 21, 22)           │ • Eliminates grainy noise spikes.                       │
  │                           │ • Produces a clean, soft multi-resolution rendering.    │
  └───────────────────────────┴─────────────────────────────────────────────────────────┘
  ```
  
  ---
### C. The Anti-Aliased Checkerboard: Computer Graphics Supersampling (Slide 24)
  Slide 24 shows the infinite checkerboard processed with Gaussian prefiltering:
  
  ```
        Naive Decimation (Slide 14)               Proper Anti-Aliasing (Slide 24)
     ┌─────────────────────────────────┐       ┌─────────────────────────────────┐
     │ █ █ █ █ █ █ █ █ █ █ █ █ █ █ █ █ │       │ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │ <-- Turns uniform gray!
     │  █ █ █  █  █ █  █ █ █  █ █  █   │       │ ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │     (Equal black & white
     │ █   █   █   █   █   █   █   █   │       │ ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒ │      tiles average out)
     │ █   █       █       █       █   │       │ █   █   █   █   █   █   █   █   │
     └─────────────────────────────────┘       └─────────────────────────────────┘
      Moiré mozaic & jagged horizon             Smooth transition to horizon
  ```
  
  * **The Graphic Truth:** As alternating black ($0$) and white ($1$) squares approach the horizon, each pixel footprint covers hundreds of tiles. 
  * Rather than oscillating randomly between black and white, proper anti-aliasing averages these elements to **mid-gray ($0.5$)**:
  
  $$\lim_{Z \to \infty} I_{\text{checkerboard}} = \frac{1 + 0}{2} = 0.5$$
  
  This is the basis of **Supersampling Anti-Aliasing (SSAA)** in graphics rendering: render at $2\times$ or $4\times$ the target resolution, apply a low-pass filter, and downsample.
  
  ---
## 7. Python Implementation: Naive Decimation vs. Anti-Aliased Downsampling
  
  ```python
  import numpy as np
  import scipy.ndimage as ndimage
  import cv2
  
  def naive_downsample(image: np.ndarray, factor: int = 2) -> np.ndarray:
    """
    Naive decimation: drops rows and columns.
    Prone to severe aliasing and moiré artifacts.
    """
    return image[::factor, ::factor]
  
  def antialiased_downsample(image: np.ndarray, factor: int = 2) -> np.ndarray:
    """
    Proper anti-aliased downsampling:
    1. Applies a low-pass Gaussian filter matched to the downsampling factor.
    2. Decimates the smoothed result.
    """
    # Rule of thumb: sigma roughly matches the downsampling factor
    # to attenuate frequencies above the new Nyquist limit (pi / factor)
    sigma = factor / 2.0
    
    # Step 1: Low-pass Gaussian filtering (separable implementation)
    blurred = ndimage.gaussian_filter(image, sigma=sigma, mode='reflect')
    
    # Step 2: Sub-sample
    return blurred[::factor, ::factor]
  
  # OpenCV alternative: cv2.pyrDown uses a 5x5 Gaussian filter followed by 2x decimation
  opencv_downsampled = cv2.pyrDown(img_orig)
  ```
  
  ---
## Conceptual Summary: Part 1
  
  1. **Scale breaks single-scale features:** Derivatives, curvatures, and template patches change value when the camera distance changes, making multi-scale processing necessary.
  2. **Naive sub-sampling violates the Nyquist criterion:** Discarding rows and columns without pre-filtering folds high-frequency energy back into false low frequencies, producing Moiré patterns and visual grain.
  3. **Real sensors integrate photons:** Physical camera pixels avoid severe aliasing by averaging light across their photosensitive surface areas.
  4. **Anti-Aliasing = Low-Pass Filter + Decimation:** Pre-filtering with a Gaussian kernel suppresses frequencies above the new Nyquist limit, ensuring clean multi-resolution downsampling.
  
  ---