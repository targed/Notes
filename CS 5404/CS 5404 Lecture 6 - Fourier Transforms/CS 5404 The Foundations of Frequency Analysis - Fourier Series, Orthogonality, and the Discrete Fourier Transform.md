## 1. What is "Frequency" in an Image? (Slides 2–7)
  
  In everyday audio processing, frequency describes the rate of acoustic vibration over **time** (cycles per second, or Hertz). In computer vision, an image is a spatial signal; therefore, we operate in terms of **spatial frequency** (cycles per unit distance, or cycles per image width).
  
  ```
  Low Spatial Frequency                          High Spatial Frequency
  (Slow, gradual luminance change)              (Rapid, abrupt luminance change)
  ┌──────────────────────────────┐              ┌──────────────────────────────┐
  │ ░░░░░▒▒▒▒▓▓▓████▓▓▓▒▒▒▒░░░░░ │              │ █ █ █ █ █ █ █ █ █ █ █ █ █ █  │
  │ ░░░░░▒▒▒▒▓▓▓████▓▓▓▒▒▒▒░░░░░ │              │ █ █ █ █ █ █ █ █ █ █ █ █ █ █  │
  │ ░░░░░▒▒▒▒▓▓▓████▓▓▓▒▒▒▒░░░░░ │              │ █ █ █ █ █ █ █ █ █ █ █ █ █ █  │
  └──────────────────────────────┘              └──────────────────────────────┘
  • Broad illumination                          • Fine fabric textures, hair, fur
  • Smooth background skies                     • Sharp step edges and silhouettes
  • Coarse object structure                     • High-frequency sensor shot noise
  ```
  
  ---
### A. The Semantic Role of Frequencies (Slides 3–5)
  Slides 3–5 establish three primary heuristics for image understanding:
  1. **Edges are High-Frequency Discontinuities:** 
   A sharp boundary between foreground and background represents an abrupt step change in radiance across adjacent pixels. Representing a step edge requires high-order spatial harmonics.
  2. **Noise is Predominantly High-Frequency:** 
   Independent identically distributed (i.i.d.) sensor noise (such as thermal or photon shot noise) fluctuates randomly from pixel to pixel, concentrating its spectral energy in high frequencies.
  3. **Blurring is Low-Pass Filtering:** 
   Neighborhood averaging operators (such as Box or Gaussian filters) attenuate rapid spatial fluctuations while leaving broad, slowly varying illumination gradients intact.
  
  ---
### B. Why Study Fourier Transforms in Computer Vision? (Slide 6)
  Slide 6 includes a practical disclaimer:
  > *"We won't be using Fourier Transforms directly very much during the rest of the course, but we will constantly be talking about frequency-space representations of images as we learn about more techniques for image manipulation."*
  
  Although real-time vision pipelines often compute small convolutions ($3 \times 3$ or $5 \times 5$) directly in the spatial domain for cache efficiency, **the Fourier domain is the analytical language of computer vision**:
  * It explains why the Box filter introduces ringing artifacts (sinc sidelobes).
  * It proves why downsampling without blurring creates Moiré patterns (aliasing).
  * It mathematically justifies unsharp masking, scale-space pyramids, and steerable filters.
  * For massive kernels (e.g., $64 \times 64$ in deconvolution or scientific imaging), the Fast Fourier Transform (FFT) reduces computational complexity from $\mathcal{O}(M^2 N^2)$ to $\mathcal{O}(M^2 \log M)$.
  
  ---
## 2. Summing Sine Waves: The Fourier Series Decomposition (Slides 8–17)
  
  In 1807, French mathematician Jean-Baptiste Joseph Fourier introduced a radical hypothesis to solve the heat conduction equation:
  > **Fourier's Claim:** Any periodic function can be rewritten as an infinite weighted sum of simple sinusoidal basis functions of varying frequencies and phases.
  
  ---
### A. The Anatomy of a Sinusoid (Slide 8)
  A basic continuous 1D sinusoidal wave is parameterized by three properties:
  
  $$f(x) = A \sin(\omega x + \phi)$$
  
  ```
     Amplitude A: Peak height from zero-baseline
     Frequency ω: Oscillation rate (rad/unit length), where ω = 2π / T
     Phase     ϕ: Horizontal spatial shift of the origin
  ```
  
  ---
### B. Case Study: Decomposing a Square Wave (Slides 10–16)
  To illustrate Fourier synthesis, Slides 10–14 construct a discontinuous square wave $f_{\text{square}}(x)$ with period $T = 2\pi$ using strictly odd sinusoidal harmonics.
  
  ```
       Step-by-Step Fourier Synthesis of a Square Wave
       
       N = 1 (Fundamental Only):                 N = 3 (Adding 3rd & 5th Harmonics):
             ^                                         ^
         1.0 │      ╭─────╮                        1.0 │    ╭─╮ ╭─╮ ╭─╮
             │     ╭╯     ╰╮                           │   ╭╯ ╰─╯ ╰─╯ ╰╮
         0.0 ┼────╭╯───────╰╮───► x                0.0 ┼──╭╯───────────╰╮──► x
             │   ╭╯         ╰╮                         │ ╭╯             ╰╮
        -1.0 │───╯           ╰──                      -1.0 │─╯               ╰─
             Pure fundamental sinusoid                 Flattens top, steepens transitions
             
       N = 30 (30 Harmonics Combined):            The Gibbs Phenomenon (Zoom at Edge):
             ^                                         ^
         1.0 │    ┌─────────┐                      1.0 │   /\ ┌─────────┐
             │    │         │                          │  /  \│         │  <-- 8.95% Overshoot!
         0.0 ┼────┤         ├───► x                0.0 ┼─/────┤         ├──► x
             │    │         │                          │/     │         │
        -1.0 │────┘         └──                       -1.0   │         │
             Near-perfect square wave                  Ringing near discontinuity
  ```
#### The Analytical Fourier Series Formula (Slide 15):
  Because a square wave centered at the origin exhibits **odd anti-symmetry** ($f(-x) = -f(x)$) and **half-wave symmetry**, all even harmonics and cosine terms drop to zero:
  
  $$f_{\text{square}}(x) = \sin(x) + \frac{1}{3}\sin(3x) + \frac{1}{5}\sin(5x) + \dots = \sum_{i=0}^\infty \frac{\sin\big((2i + 1)x\big)}{2i + 1}$$
  
  * **Harmonic Frequencies:** Increase by odd integer multiples: $\omega, 3\omega, 5\omega, \dots$
  * **Harmonic Amplitudes:** Decay inversely with frequency: $A_k = \frac{1}{k}$. High-frequency ripples are necessary to construct the sharp, infinite-slope vertical jump of the step edge.
  
  ---
### C. The Gibbs Phenomenon: An Inevitable Consequence of Truncation
  Notice in Slide 14 that even with $N = 30$ harmonics, sharp spikes remain at the corner discontinuities. 
  * **Mathematical Fact:** Truncating an infinite Fourier series to a finite number of terms $N$ produces **Gibbs ringing** near jump discontinuities.
  * **The Invariant:** As $N \to \infty$, the width of the ringing spikes shrinks to zero, but the peak amplitude of the overshoot **does not vanish**. It converges to a fixed limit:
  
  $$\lim_{N \to \infty} \text{Overshoot} \approx 8.949\% \text{ of the jump height}$$
  
  This explains why sharp, un-smoothed filters in image processing produce ringing fringes around crisp object silhouettes.
  
  ---
## 3. Visualizing Frequency Space & The Harmonic Basis (Slides 18–24)
  
  Instead of visualizing an image as an intensity array over spatial coordinates $(x, y)$, the Fourier transform maps the signal into an alternative coordinate system: **Frequency Space**.
  
  ```
       Spatial Domain (Time/Space)                       Frequency Domain
       f(x) = sin(2πkx) + ⅓ sin(2π·3kx)                  Amplitude vs. Spatial Frequency
            ^                                                  ^ Amplitude
        1.0 │     /\    /\                                 1.0 │       |
            │    /  \  /  \                                    │       | (Amplitude 1.0 at freq k)
        0.0 ┼───/────\/────\────► x                        0.33│       |       | (Amp 0.33 at 3k)
            │  /            \                                  │       |       |
       -1.0 │─/              \──                           0.0 ┴───────┴───────┴───────► Frequency
                                                               0       k      3k
  ```
  
  ---
### The Fallacy of the "Half-Frequency" (Slides 22–24)
  Slide 22 poses a question:
  > **"Can we have 'half-frequencies' in a Fourier series?"**
  > 
  > *Answer:* **No.** In a standard Fourier Series over a finite window of length $T$, non-integer harmonic frequencies are mathematically invalid.
  
  ```
       Valid Fundamental f₀ (Fits Exactly 1 Cycle):
       ┌────────────────────────────────────────────────────────┐
       │             ╭──────────────╮                           │
       │            ╭╯              ╰╮                          │
       ├───────────╭╯────────────────╰╮─────────────────────────┤
       │          ╭╯                  ╰╮                        │
       │          ╰────────────────────╯                        │
       └────────────────────────────────────────────────────────┘
       x = 0                                                x = T
       
       Invalid Half-Frequency 0.5f₀ (Cannot Complete Cycle):
       ┌────────────────────────────────────────────────────────┐
       │             ╭────────────────────────────╮             │
       │           ╭─╯                            ╰─╮           │  <-- Does NOT match
       ├──────────╭╯────────────────────────────────╰───────────┤      periodic boundary!
       │         ╭╯                                             │      Introduces massive
       │        ╭╯                                              │      spectral leakage!
       └────────────────────────────────────────────────────────┘
       x = 0                                                x = T
  ```
#### The Mathematical Proof:
  1. **Periodic Boundary Condition:**
   A Fourier series assumes the observation window of length $T$ repeats infinitely: $f(x + T) = f(x)$.
  2. **Harmonic Basis Requirement:**
   For a sinusoid $\sin(2\pi f x)$ to satisfy this periodic boundary condition:
  
  $$\sin\big(2\pi f (x + T)\big) = \sin(2\pi f x) \implies 2\pi f T = 2\pi k \implies f = \frac{k}{T} = k \cdot f_0, \quad k \in \mathbb{Z}$$
  
  3. **Orthogonality of Basis Functions:**
   Two sinusoids of frequencies $m f_0$ and $n f_0$ are mutually orthogonal over $[0, T]$ if and only if $m$ and $n$ are integers:
  
  $$\int_0^T \sin\left(\frac{2\pi m x}{T}\right) \sin\left(\frac{2\pi n x}{T}\right) dx = \begin{cases} \frac{T}{2} & m = n \\[4pt] 0 & m \neq n \end{cases}$$
  
  If you inject an arbitrary fractional frequency (e.g., $0.5 f_0$), the wave cannot close its cycle within the window. The Fourier Transform is forced to approximate this broken wave as an infinite series of integer harmonics, creating an artifact known as **spectral leakage**.
  
  ---
## 4. Sampling Limits, Temporal Aliasing, and the Nyquist Rate (Slides 25–39)
  
  While continuous signals support infinite frequencies, digital computers capture signals via discrete sensor sampling.
  
  ---
### A. The Limits of Discrete Sampling (Slides 25–33)
  When we sample a continuous sine wave at discrete spatial steps $\Delta x$, we capture only discrete points:
  
  $$x[n] = \sin(2\pi f \cdot n\Delta x)$$
  
  * If the sampling rate is high (Slides 28–29), the underlying wave is reconstructed accurately.
  * If the sampling rate is too low (Slides 30–32), **aliasing** occurs: the discrete sample points can be fitted by an entirely different, slower sine wave. 
  
  ```
       High-Frequency True Signal:  /\  /\  /\  /\  /\  /\  /\  /\
       Coarse Sample Points:        •       •       •       •       •
       Aliased Low-Frequency Wave:  ╭───────╮       ╭───────╮       ╭
                                    ╰───────╯       ╰───────╯       ╰
       The discrete samples are identical, but the perceived signal is completely wrong!
  ```
  
  ---
### B. The Wagon-Wheel Effect: Aliasing in Time (Slides 34–35)
  Slide 34 illustrates temporal aliasing using a rotating spoked wagon wheel recorded by a video camera operating at frame rate $f_s = 24\text{ FPS}$ (or $30\text{ FPS}$):
  
  ```
       Frame 0             Frame 1             Frame 2             Frame 3
         (•)                 ( )                 ( )                 ( )
       ──┼──               ──┼──•              ──┼──               ──┼──
         │                   │                   │   •           •   │
       Spoke at 12:00      Rotated 110° CW     Rotated 220° CW     Rotated 330° CW
                           (Looks like -70°)   (Looks like -140°)  (Appears near top!)
       
       Apparent Motion: The wheel appears to be slowly spinning BACKWARD (Counter-Clockwise)!
  ```
  
  * **The Physical Mechanism:** The camera shutter opens briefly at fixed time intervals $\Delta t = 1/f_s$. If the rotational velocity exceeds half the camera's frame rate ($\omega > \frac{f_s}{2}$), the visual position of a spoked wheel advances nearly an entire cycle per frame.
  * **Perceptual Ambiguity:** The human visual system matches features using the **principle of shortest path**; it interprets the wheel as moving backward by a small angle rather than forward by a large angle. 
  * Adding an asymmetric marker (the black dot on Slide 34) breaks the rotational symmetry, allowing the visual cortex to track the true forward trajectory.
  
  ---
### C. The Nyquist Rate Defined (Slides 36–37)
  To reconstruct any continuous signal without aliasing:
  
  $$f_{\text{sampling}} \ge 2 \cdot f_{\max}$$
  
  $$f_{\text{Nyquist}} = \frac{f_{\text{sampling}}}{2}$$
  
  * **Physical Intuition (Slide 37):** You must capture **at least two samples per cycle**—one sample near the crest (positive peak) and one sample near the trough (negative valley). If you capture fewer than two samples per cycle, the high-frequency wave aliases into an artificial lower frequency.
  
  ---
## 5. The Complex Fourier Transform & Negative Frequencies (Slides 40–47)
  
  Slide 40 presents a simple Python snippet taking the Fast Fourier Transform (FFT) of a pure sine wave:
  
  ```python
  import numpy as np
  import math
  
  L = 1.0
  N = 101
  x = np.arange(0, L, L / N)
  signal_x = np.sin(2 * math.pi * 4 * x)   # Pure 4 Hz sinusoid
  signal_w = np.fft.fft(signal_x)
  ```
  
  Slide 41 displays the frequency output and asks an important question:
  > **"Why are there two non-zero points in the frequency spectrum for a single sine wave?"**
  
  ```
                  Real Space                                Frequency Space (FFT)
            ^ Intensity                                   ^ Magnitude
        1.0 │  •   •   •   •                         40   │  • (+f₀)            • (-f₀ / N-k)
            │ • • • • • • • •                             │
        0.0 ┼──────────────────► x                    0.0 ┼──┴──────────────────┴──► k
            │•   •   •   •   •                            0  4                N-4
       -1.0 │                                             Two distinct spikes appear!
  ```
  
  ---
### A. The Phasor Model: Euler's Identity (Slide 43, 46)
  To understand why two spikes appear, we turn to **Leonhard Euler’s identity** (1748), which connects complex exponentials to trigonometry:
  
  $$e^{j\theta} = \cos(\theta) + j\sin(\theta) \quad (\text{where } j = \sqrt{-1})$$
  
  ```
                            The Complex Phasor Plane
                                       ^ Imaginary (j)
                                       │        e^{jθ} = cos θ + j sin θ
                                       │       /|
                                       │      / |
                                   1.0 │     /  | sin θ
                                       │    /θ  |
                                       │   /____|
                        ───────────────┼──┴─────┴────────► Real
                                       │    cos θ
                                       │
  ```
  
  * In the complex plane, $e^{j\theta}$ represents a **rotating unit vector (a phasor)** with angle $\theta$.
  * If $\theta = 2\pi f t$, the vector rotates around the unit circle at a constant angular velocity of $2\pi f$ radians per second.
  
  ---
### B. Positive vs. Negative Frequencies: The Direction of Rotation (Slide 46)
  In mechanical vibrations or audio, a "negative frequency" sounds physically impossible. In complex analysis, however, the sign of frequency denotes the **direction of rotation**:
  * **Positive Frequency ($+f$):** 
  
  $$e^{+j 2\pi f t} = \cos(2\pi f t) + j\sin(2\pi f t) \implies \text{Rotates \bf Counter-Clockwise (CCW)}$$
  
  * **Negative Frequency ($-f$):** 
  
  $$e^{-j 2\pi f t} = \cos(-2\pi f t) + j\sin(-2\pi f t) = \cos(2\pi f t) - j\sin(2\pi f t) \implies \text{Rotates \bf Clockwise (CW)}$$
  
  ---
### C. Constructing a Real Sine Wave: Counter-Rotating Vectors (Slide 43, 47)
  How does a 1D real-world physical signal (which has zero imaginary energy) emerge from complex numbers? 
  
  Solving Euler's formulas for $\cos(\theta)$ and $\sin(\theta)$:
  
  $$\cos(2\pi f t) = \frac{e^{+j 2\pi f t} + e^{-j 2\pi f t}}{2}$$
  
  $$\sin(2\pi f t) = \frac{e^{+j 2\pi f t} - e^{-j 2\pi f t}}{2j}$$
  
  ```
   Counter-Rotating Phasor Decomposition of a Real Sinusoid:
   
        Positive Frequency Phasor                     Negative Frequency Phasor
             (Rotates CCW)                                  (Rotates CW)
                  ^ Imag                                         ^ Imag
                  │   ╭─►                                        │   ◄─╮
                  │  /                                           │    \
                  │ / +θ                                         │ -θ  \
       ───────────┼/───────► Real                     ───────────┼──────\─► Real
                  │                                              │
                  
                  │                                              │
                  └──────────────────────┬───────────────────────┘
                                         ▼
                 Sum of Vectors Cancels Imaginary Components!
                 The vertical projection oscillates as a pure real sinusoid:
                 sin(2πft) = ½j [ e^{+j2πft} - e^{-j2πft} ]
  ```
  
  > **Why the Spectrum Has Two Spikes:**
  > A real sine wave is not a single rotation. It is the superposition of **two counter-rotating complex phasors**: one spinning counter-clockwise at $+f$, and an identical conjugate partner spinning clockwise at $-f$.
  > 
  > The imaginary components cancel identically everywhere, leaving a strictly real-valued signal. Therefore, the Fourier transform of any real-valued signal **must be symmetric across the zero-frequency DC axis**.
  
  ---
### D. Conjugate Symmetry for Real Signals
  For any strictly real signal $f[n] \in \mathbb{R}$:
  
  $$F[-k] = F^*[k]$$
  
  where $*$ denotes the complex conjugate ($(a + jb)^* = a - jb$).
  * **Magnitude Spectrum is Even (Symmetric):** $|F[-k]| = |F[k]|$
  * **Phase Spectrum is Odd (Anti-Symmetric):** $\angle F[-k] = -\angle F[k]$
  
  ---
## 6. Spectrum Coordinate Conventions: Uncentered vs. Centered FFT (Slides 44–45)
  
  Slides 44 and 45 show two standard ways of arranging the output of a discrete Fourier transform:
  
  ```
        Standard NumPy FFT Output                         Centered Spectrum via fftshift
             (Slides 40, 44)                                     (Slides 44, 45)
             
   ┌──────┬──────────────────────┬──────┐               ┌──────────────────────┬──────────────────────┐
   │  DC  │  Positive Freqs      │ Neg  │               │  Negative Freqs      │  DC  │  Positive Freqs │
   │ k=0  │  k = 1 ... N/2       │Freqs │               │  k = -N/2 ... -1     │ k=0  │  k = 1 ... N/2  │
   └──────┴──────────────────────┴──────┘               └──────────────────────┴──────────────────────┘
   Index 0                       Index N-1              Index -N/2            Index 0         Index +N/2
   DC at far-left (Raw machine format)                  DC at center (Human-interpretable format)
  ```
  
  1. **The Raw Discrete Fourier Transform (`np.fft.fft`):**
   * Computes indices over $k \in [0, N-1]$.
   * Index $0$ stores the **DC component** (average signal intensity).
   * Indices $1 \dots \lfloor N/2 \rfloor$ store positive frequencies $[0, +f_{\text{Nyquist}}]$.
   * Indices $\lfloor N/2 \rfloor + 1 \dots N-1$ wrap around to store negative frequencies $[-f_{\text{Nyquist}}, 0)$.
  2. **The Centered Spectrum (`np.fft.fftshift`):**
   * Uses modulo arithmetic to circular-shift the array by $N/2$, placing the **zero-frequency DC origin at the exact physical center**.
   * Low frequencies cluster around the center; high frequencies extend symmetrically outward toward the boundaries.
  
  ---
## Summary Matrix: Core Foundations of Frequency Analysis
  
  | Concept | Continuous Formulation | Discrete Domain | Practical Computer Vision Meaning |
  | :--- | :--- | :--- | :--- |
  | **Fourier Basis** | $e^{j \omega x} = \cos(\omega x) + j\sin(\omega x)$ | $W_N^{kn} = e^{-j \frac{2\pi}{N} k n}$ | Decomposes images into orthogonal harmonic gratings |
  | **Harmonic Spacing** | Any $\omega \in \mathbb{R}$ | Integer multiples $k \cdot \frac{2\pi}{N}$ | No "half-frequencies" exist in a periodic window |
  | **Nyquist Criterion**| $\omega_s > 2 \omega_{\max}$ | $N \ge 2 k_{\max}$ | Sampling rate must be at least twice the maximum frequency to prevent aliasing |
  | **Negative Frequencies** | Direction of phasor rotation | Complex conjugate indices | Real images always produce symmetric dual-lobe spectra |
  | **DC Component** | $F(0) = \int f(x) dx$ | $F[0] = \sum_{n} f[n]$ | Proportional to the global mean image brightness |
  | **Gibbs Phenomenon** | $\sim 8.95\%$ overshoot at jumps | High-frequency ringing ripples | Finite truncation of high frequencies causes visual halos around sharp edges |
  
  ---