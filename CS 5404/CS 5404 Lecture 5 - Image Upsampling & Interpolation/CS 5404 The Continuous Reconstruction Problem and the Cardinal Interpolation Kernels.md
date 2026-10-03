## 1. The Upsampling Dilemma: Why Simply "Duplicating Pixels" Fails (Slides 4–6)
  
  Slide 4 presents a low-resolution image patch. When asked how to enlarge it, the most straightforward approach is **pixel replication** (repeating each row and column $K$ times):
  
  ```
     Original 2×2 Image                      Pixel Replication 4×4 (K = 2)
        ┌─────┬─────┐                             ┌─────┬─────┬─────┬─────┐
        │ 10  │ 90  │                             │ 10  │ 10  │ 90  │ 90  │
        ├─────┼─────┤        Duplicate            ├─────┼─────┼─────┼─────┤
        │ 30  │ 60  │        Rows & Cols          │ 10  │ 10  │ 90  │ 90  │
        └─────┴─────┘       ─────────────►        ├─────┼─────┼─────┼─────┤
                                                  │ 30  │ 30  │ 60  │ 60  │
                                                  ├─────┼─────┼─────┼─────┤
                                                  │ 30  │ 30  │ 60  │ 60  │
                                                  └─────┴─────┴─────┴─────┘
  ```
  
  Slide 6 shows the resulting upsampled image:
  * **Visual Artifacts:** The image appears **blocky**, dominated by jagged, staircase edges (**pixelation** or the *checkerboard effect*).
  * **Mathematical Cause:** The reconstructed signal has artificial $C^0$ step discontinuities at every pixel boundary. These discontinuities introduce spurious high-frequency energy that was never present in the physical scene.
  
  ---
## 2. The Core Theory: "Un-Discretizing" via Continuous Reconstruction (Slides 7–10)
  
  Slide 7 outlines the theoretical goal of image resampling:
  
  > **The Continuous Hypothesis:**
  > A digital image $F[x, y]$ is merely a discrete point-sampling of an underlying continuous, physical irradiance function $f(x, y)$:
  > 
  > $$F[x, y] = \text{quantize}\big\{ f(x \cdot \Delta x, \, y \cdot \Delta y) \big\}$$
  > 
  > If we can reconstruct that continuous function $\tilde{f}(x, y)$ from our discrete samples, we can re-sample it at **any arbitrary spatial coordinate or resolution**.
  
  ```
    Discrete Samples F[n]                   Continuous Reconstruction f̃(x)               Continuous Resampling
              ^                                           ^                                           ^
              │   •       •                               │       ╭───────╮                           │   •   •   •   •
              │   │   •   │   •                           │     ╭─╯       ╰─╮                         │   │ • │ • │ • │
              └───┴───┴───┴───┴───► x                     └───┬─┴───────────┴─┬─► x                   └───┴─┴─┴─┴─┴─┴─┴─┴─► x
                  1   2   3   4                               1               4                           New Fine Grid
            Discrete Lattice Z                          Continuous Manifold R                         Arbitrary Spacing Δx'
  ```
  
  ---
### The Mathematical Model: Convolving an Impulse Train with a Continuous Filter (Slides 10–12)
  To reconstruct a continuous function from discrete measurements, we model the discrete samples as an analog **Dirac impulse train** (a sequence of 1D spatial spikes scaled by the sample intensities):
  
  $$f_s(x) = \sum_{n=-\infty}^\infty f[n] \cdot \delta(x - n)$$
  
  where $\delta(x)$ is the continuous Dirac delta function:
  
  $$\delta(x) = 0 \quad \forall x \neq 0, \quad \int_{-\infty}^\infty \delta(x) \, dx = 1$$
  
  To bridge the gaps between these discrete spikes, we convolve the impulse train with a **continuous interpolation kernel** $h(x)$:
  
  $$\tilde{f}(x) = (f_s * h)(x) = \int_{-\infty}^\infty f_s(\tau) \cdot h(x - \tau) \, d\tau$$
  
  Substituting the impulse train equation:
  
  $$\tilde{f}(x) = \int_{-\infty}^\infty \left( \sum_{n=-\infty}^\infty f[n] \cdot \delta(\tau - n) \right) h(x - \tau) \, d\tau$$
  
  Applying the sifting property of the Dirac delta ($\int g(\tau)\delta(\tau - n)d\tau = g(n)$):
  
  $$\tilde{f}(x) = \sum_{n=-\infty}^\infty f[n] \cdot h(x - n)$$
  
  > **The Reconstruction Identity:**
  > Reconstructing a continuous function from discrete samples is mathematically equivalent to **centering a continuous kernel $h(x)$ at every sample point $n$, scaling it by the sample intensity $f[n]$, and summing all overlapping curves**.
  
  ---
## 3. The Classical Interpolation Kernels (Slides 11–16)
  
  The choice of reconstruction kernel $h(x)$ determines the smoothness, sharpness, and computational complexity of the interpolated image.
  
  ```
                     Comparison of 1D Continuous Interpolation Kernels
                     
        Nearest-Neighbor (Box)            Linear (Triangle)                 Ideal Sinc
                 ^                               ^                               ^
             1.0 │  ┌───────┐                1.0 │      /\                   1.0 │       ╭─╮
                 │  │       │                    │     /  \                      │     ╭─╯ ╰─╮
                 │  │       │                    │    /    \                     │    ╭╯     ╰╮
             0.0 ┴──┴───────┴──► x           0.0 ┴───┴──────┴───► x          0.0 ┼───╭┴───────┴╮───► x
                   -0.5    +0.5                     -1.0   +1.0                  │  ╭╯         ╰╮
                    Box Π(x)                        Triangle Λ(x)                └──╯           ╰──
                                                                                     sinc(x)
  ```
  
  ---
### A. Nearest-Neighbor Interpolation: The Box Kernel $\Pi(x)$ (Slides 11–12)
  Nearest-neighbor interpolation assigns to any continuous point $x$ the intensity of the closest discrete sample.
#### 1. Spatial Formulation:
  The continuous reconstruction kernel is a centered rectangular pulse (Box function $\Pi(x)$):
  
  $$h_{\text{NN}}(x) = \Pi(x) = \begin{cases} 1 & |x| < \frac{1}{2} \\[4pt] 0 & |x| \ge \frac{1}{2} \end{cases}$$
  
  Convolving the sample train with $\Pi(x)$ widens each discrete spike into a flat, constant-value horizontal slab of width $1.0$:
  
  $$\tilde{f}(x) = f\big[\text{round}(x)\big]$$
#### 2. Mathematical Properties:
  * **Spatial Support:** Compact, $[-0.5, +0.5]$ (width $= 1$). Only $1$ sample influences any point.
  * **Continuity Class:** $C^{-1}$ (discontinuous). The reconstructed function contains jump discontinuities at half-integer coordinates $x = n + 0.5$.
  * **Visual Profile:** Severe blocking and pixelation artifacts (Slide 6).
  
  ---
### B. Linear Interpolation: The Triangle Kernel $\Lambda(x)$ (Slide 13)
  Linear interpolation connects adjacent discrete samples with straight line segments.
#### 1. Spatial Formulation:
  The reconstruction kernel is the symmetric continuous triangle function $\Lambda(x)$ (the tent function):
  
  $$h_{\text{linear}}(x) = \Lambda(x) = \begin{cases} 1 - |x| & |x| < 1 \\[4pt] 0 & |x| \ge 1 \end{cases}$$
  
  Notice that the triangle function is the self-convolution of two unit box functions: $\Lambda(x) = \Pi(x) * \Pi(x)$.
#### 2. Evaluating Between Two Neighbors:
  For any non-integer target coordinate $x$ located between discrete integers $n$ and $n+1$:
  * Let fractional offset $\alpha = x - n$, where $\alpha \in [0, 1)$.
  * The distance from $x$ to sample $n$ is $\alpha$, so $h(x - n) = 1 - \alpha$.
  * The distance from $x$ to sample $n+1$ is $1 - \alpha$, so $h(x - (n+1)) = \alpha$.
  
  $$\tilde{f}(x) = (1 - \alpha) \cdot f[n] + \alpha \cdot f[n+1]$$
  
  ```
          f[n]
           ^
           │   • (1 - α)
           │   │\
           │   │ \        • f̃(x) = (1 - α)f[n] + αf[n+1]
           │   │  \      /│
           │   │   \    / │
           │   │    \  /  │
           │   │     \/   │   • α
           │   │          │   │
           └───┴──────────┴───┴──────► Spatial x
               n          x  n+1
               │◄─── α ──►│
  ```
#### 3. Mathematical Properties:
  * **Spatial Support:** $[-1, +1]$ (width $= 2$). Exactly $2$ neighboring samples contribute to any point.
  * **Continuity Class:** $C^0$ (continuous in amplitude, but its first derivative $f'(x)$ is discontinuous at integer grid points).
  * **Visual Profile:** Eliminates the hard blockiness of nearest-neighbor, but introduces **piecewise-linear facets** and a noticeable softening/blurring of fine textures.
  
  ---
### C. Cubic Spline Interpolation: $C^1$ Smoothness & Sharpening (Slide 15)
  To ensure that both the reconstructed function $\tilde{f}(x)$ and its slope $\tilde{f}'(x)$ vary smoothly across pixel boundaries, we use piecewise cubic polynomials over $[-2, +2]$.
#### 1. The Keys Cubic Convolution Kernel:
  A widely used formulation in image processing is **Keys' parametric cubic convolution kernel** [3]:
  
  $$h_{\text{cubic}}(x) = \begin{cases} (a + 2)|x|^3 - (a + 3)|x|^2 + 1 & |x| \le 1 \\[4pt] a|x|^3 - 5a|x|^2 + 8a|x| - 4a & 1 < |x| < 2 \\[4pt] 0 & |x| \ge 2 \end{cases}$$
  
  where $a$ is a tuning parameter controlling derivative matching at boundaries. Setting $a = -0.5$ matches the 3rd-order Taylor series expansion of the original signal:
  
  $$h_{\text{Keys}}(x) = \begin{cases} 1.5|x|^3 - 2.5|x|^2 + 1 & |x| \le 1 \\[4pt] -0.5|x|^3 + 2.5|x|^2 - 4|x| + 2 & 1 < |x| < 2 \\[4pt] 0 & |x| \ge 2 \end{cases}$$
  
  ```
                          The Keys Cubic Kernel (a = -0.5)
                                         ^
                                     1.0 │       ╭─╮
                                         │     ╭─╯ ╰─╮
                                         │   ╭─╯     ╰─╮
                                     0.0 ┼───╯─────────╰───► Radius |x|
                                         │ ╭─╮         ╭─╮
                                    -0.1 ├─╯ ╰─────────╯ ╰─  <-- Negative Sidelobes
                                         └─────────────────►
                                         0        1.0      2.0
  ```
#### 2. The Role of Negative Sidelobes:
  Unlike the box and triangle filters (which are non-negative everywhere), the cubic kernel dips below zero between $|x| = 1$ and $|x| = 2$.
  * These negative lobes subtract a fraction of more distant neighbors.
  * As we established in **Module M1.4** (Unsharp Masking), subtracting neighboring values enhances high frequencies.
  * Consequently, cubic interpolation **preserves edge sharpness** and compensates for the smoothing blur typical of linear interpolation.
#### 3. Mathematical Properties:
  * **Spatial Support:** $[-2, +2]$ (width $= 4$). Evaluates $4$ discrete samples per 1D point ($16$ samples in 2D).
  * **Continuity Class:** $C^1$ (both function values and first derivatives match continuously at boundaries).
  * **Visual Profile:** Sharp edges and smooth curves, but high-contrast boundaries can exhibit slight overshoot or undershoot ringing (halos).
  
  ---
### D. The "Ideal" Sinc Reconstruction Filter (Slides 14, 16)
  Slide 14 and 16 show the Whittaker-Shannon interpolation formula using the **sinc** function:
  
  $$h_{\text{ideal}}(x) = \text{sinc}(x) = \frac{\sin(\pi x)}{\pi x}$$
  
  Slide 16 asks:
  > **"Why might this function be considered 'ideal'? Think of the Fourier Transform."**
  
  ```
          Spatial Domain: sinc(x)                  Frequency Domain: Ideal Brick-Wall
                   ^                                                ^ H(ω)
               1.0 │       ╭─╮                                  1.0 │  ┌──────────────┐
                   │     ╭─╯ ╰─╮                                    │  │              │
                   │    ╭╯     ╰╮                                   │  │              │
               0.0 ┼───╭┴───────┴╮───► x                        0.0 ┴──┴──────────────┴──► ω
                   │  ╭╯         ╰╮                                   -ω_c           +ω_c
                   └──╯           ╰──                                 Passband      Stopband
  ```
  
  ---
#### 1. Frequency-Domain Proof of Optimality:
  By the Fourier Transform pair:
  
  $$\mathcal{F}\left\{ \frac{\sin(\pi x)}{\pi x} \right\} = \text{rect}\left(\frac{\omega}{2\pi}\right) = \begin{cases} 1 & |\omega| \le \pi \\[4pt] 0 & |\omega| > \pi \end{cases}$$
  
  * The frequency response is an **ideal "brick-wall" low-pass filter**.
  * **Passband ($|\omega| \le \pi$):** Passes all frequencies below the original Nyquist limit with unity gain and zero phase distortion.
  * **Stopband ($|\omega| > \pi$):** Completely attenuates all spectral replicas created by the sampling process ($0\%$ aliasing leakage).
  * If the continuous signal $f(x)$ was strictly band-limited below the Nyquist limit, convolving its discrete samples with a continuous sinc filter reconstructs the continuous signal **identically, with zero mathematical error**:
  
  $$\tilde{f}(x) = \sum_{n=-\infty}^\infty f[n] \cdot \frac{\sin\big(\pi(x - n)\big)}{\pi(x - n)}$$
  
  ---
#### 2. Why the Sinc Filter is Impractical in Real Vision Systems:
  Despite being theoretically optimal, the sinc filter is rarely used directly in spatial-domain imaging:
  1. **Infinite Spatial Support:** The sinc function decays slowly at a rate of $\frac{1}{x}$. Computing a single interpolated pixel requires summing over every pixel in the entire image ($N = \infty$).
  2. **Truncation Ringing (The Gibbs Phenomenon):** Truncating the sinc filter to a finite window (e.g., $7 \times 7$) creates a sharp frequency cutoff with high ripple, causing visible ringing artifacts (halos) near sharp edges.
  3. **Negative Ringing / Photometric Violation:** The oscillations of the sinc function can yield negative interpolated intensity values, which are physically meaningless for radiance measurements.
  
  ---
## 4. Analytical Comparison of Cardinal Interpolation Kernels
  
  | Kernel Function | Formula $h(x)$ | Support Width | Continuity | Frequency-Domain Profile | Practical Application |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | **Nearest-Neighbor** | $\Pi(x)$ (Box) | $1$ sample | $C^{-1}$ (Discontinuous) | $\text{sinc}(\omega)$ (High spectral leakage) | Real-time preview, label/mask resizing |
  | **Linear** | $\Lambda(x)$ (Triangle) | $2$ samples | $C^0$ (Continuous) | $\text{sinc}^2(\omega)$ (Moderate blur) | Hardware-accelerated GPU bilinear filtering |
  | **Bicubic (Keys)** | Piecewise Cubic ($a=-0.5$) | $4$ samples | $C^1$ (Smooth slope) | Near-flat passband + subtle boost | Default photographic resampling standard |
  | **Ideal Sinc** | $\frac{\sin(\pi x)}{\pi x}$ | $\infty$ (Infinite) | $C^\infty$ (Analytic) | $\text{rect}(\omega)$ (Ideal brick-wall low-pass) | Theoretical benchmark, FFT upsampling |
  
  ---
## Visual Summary & Transition to Part 2
  
  We have established the continuous formulation for reconstructing an image from discrete points, as well as the behavior of the four cardinal 1D interpolation kernels.
  
  ```
       Image Upsampling Concept Map
       
       [ Discrete Samples ] ──► Convolve with Kernel h(x) ──► [ Continuous Surface f̃(x) ]
                                            │
               ┌────────────────────────────┼────────────────────────────┐
               ▼                            ▼                            ▼
           Box Π(x)                   Triangle Λ(x)                 Cubic Spline
         Fastest, but                Continuous C⁰,               Smooth C¹, sharp,
         harsh blocks                blurs textures              industry standard
  ```