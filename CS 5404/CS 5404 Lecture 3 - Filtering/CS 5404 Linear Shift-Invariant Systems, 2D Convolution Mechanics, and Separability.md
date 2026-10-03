## 1. From Point Operations to Neighborhood Filtering (Slides 3–5)
  
  In **Module M1.3**, we established that point operations are mathematically constrained by **spatial independence**:
  
  $$g(x, y) = T\big(f(x, y)\big)$$
  
  Because $T$ depends only on the single scalar value at $(x, y)$, it cannot measure spatial change, smooth out high-frequency noise spikes, or detect directional boundaries. 
  
  To overcome this, **Neighborhood Filtering** extends the input support window from a single coordinate to a localized neighborhood $\mathcal{N}(x, y)$ of dimensions $K \times K$:
  
  ```
      Point Processing                              Neighborhood Filtering
    (Context-Independent)                            (Context-Dependent)
   ┌───┬───┬───┐       ┌───┐                  ┌───┬───┬───┐       ┌───┐
   │   │   │   │       │   │                  │ 1 │ 1 │ 1 │       │   │
   ├───┼───┼───┤       ├───┤                  ├───┼───┼───┤       ├───┤
   │   │ P │   │ ────► │ P'│                  │ 1 │ P │ 1 │ ────► │ P'│
   ├───┼───┼───┤       ├───┤                  ├───┼───┼───┤       ├───┤
   │   │   │   │       │   │                  │ 1 │ 1 │ 1 │       │   │
   └───┴───┴───┘       └───┘                  └───┴───┴───┘       └───┘
     1 Pixel In ──► 1 Pixel Out               3×3 Patch In ──► 1 Pixel Out
  ```
  
  ---
## 2. The Box Filter: Intuition and Worked Example (Slides 6–10, 22–26)
  
  The simplest spatial smoothing operator is the **Mean Filter** (also known as the **Box Filter** or **Rectangular Filter**).
### A. Mathematical Definition
  For a centered $(2k+1) \times (2k+1)$ square window (where $k=1$ yields a $3 \times 3$ window):
  
  $$h[m, n] = \frac{1}{(2k+1)^2} \sum_{i=-k}^k \sum_{j=-k}^k f[m + i, \, n + j]$$
  
  For a $3 \times 3$ window ($k=1$), every output pixel is the unweighted arithmetic mean of $9$ local neighbors:
  
  $$h[m, n] = \frac{1}{9} \sum_{i=-1}^1 \sum_{j=-1}^1 f[m + i, \, n + j]$$
  
  ---
### B. Step-by-Step Numerical Walkthrough (Slides 6–10)
  Consider the discrete image patch $f[\cdot, \cdot]$ from Slide 6, where the background is $0$, an isolated noise spike has value $90$ at $[8, 2]$, and an object region contains values of $90$:
  
  ```
                    Input Image f[·, ·]
      c= 0   1   2   3   4   5   6   7   8   9
  r=0  ┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
   1  │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │
   2  │ 0 │ 0 │ 0 │90 │90 │90 │90 │90 │ 0 │ 0 │
   3  │ 0 │ 0 │ 0 │90 │90 │90 │90 │90 │ 0 │ 0 │
   4  │ 0 │ 0 │ 0 │90 │ 0 │90 │90 │90 │ 0 │ 0 │  <-- Note center hole at [4,4]=0
   5  │ 0 │ 0 │ 0 │90 │90 │90 │90 │90 │ 0 │ 0 │
   6  │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │
   7  │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │
   8  │ 0 │ 0 │90 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │  <-- Isolated noise spike at [8,2]
   9  └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  ```
  
  Let's compute the output $h[m, n]$ for key locations:
#### 1. Evaluation at Flat Background: $h[1, 1]$ (Slide 6)
  The $3 \times 3$ neighborhood centered at $[1, 1]$ spans rows $0 \dots 2$ and columns $0 \dots 2$:
  
  $$\text{Patch} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix} \implies \text{Sum} = 0 \implies h[1, 1] = \frac{0}{9} = \mathbf{0}$$
#### 2. Approaching an Edge: $h[1, 2]$ (Slide 7)
  The neighborhood centered at $[1, 2]$ spans rows $0 \dots 2$ and columns $1 \dots 3$:
  
  $$\text{Patch} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 90 \end{bmatrix} \implies \text{Sum} = 0 + \dots + 90 = 90 \implies h[1, 2] = \frac{90}{9} = \mathbf{10}$$
#### 3. Transition Zone: $h[1, 3]$ (Slide 9)
  The neighborhood centered at $[1, 3]$ spans rows $0 \dots 2$ and columns $2 \dots 4$:
  
  $$\text{Patch} = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 90 & 90 \end{bmatrix} \implies \text{Sum} = 90 + 90 = 180 \implies h[1, 3] = \frac{180}{9} = \mathbf{20}$$
#### 4. Interior Region with a Missing Pixel: $h[4, 4]$ (Slide 10)
  At coordinate $[4, 4]$, the central pixel itself has an intensity of $0$, surrounded by eight $90$s:
  
  $$\text{Patch} = \begin{bmatrix} 90 & 90 & 90 \\ 90 & \mathbf{0} & 90 \\ 90 & 90 & 90 \end{bmatrix} \implies \text{Sum} = 8 \times 90 = 720 \implies h[4, 4] = \frac{720}{9} = \mathbf{80}$$
  
  * **Visual Interpretation:** The hole at $[4, 4]$ is filled in and smoothed out (from $0$ to $80$).
#### 5. Filtering an Isolated Impulse (Noise Spike): Around $[8, 2]$
  The single bright pixel of value $90$ at $[8, 2]$ is surrounded entirely by $0$s.
  * When the $3 \times 3$ box window passes over any neighbor adjacent to $[8, 2]$, that neighbor receives $\frac{90}{9} = 10$.
  * The original noise spike at $[8, 2]$ is reduced from $90$ down to $10$, but its energy is **dispersed across a $3 \times 3$ footprint**.
  
  ---
### C. The Normalization Factor: Why Divide by 9? (Slide 29)
  
  ```
                       ┌───┬───┬───┐
                 1     │ 1 │ 1 │ 1 │
       Kernel = ─── ×  ├───┼───┼───┤
                 9     │ 1 │ 1 │ 1 │
                       ├───┼───┼───┤
                       │ 1 │ 1 │ 1 │
                       └───┴───┴───┘
  ```
  
  > **The Conservation of Energy Principle (Unity Gain):** 
  > To maintain the overall brightness of an image, the sum of all elements in a smoothing kernel must equal exactly $1$:
  
  $$\sum_{i=-k}^k \sum_{j=-k}^k K[i, j] = 1.0$$
  
  * **What happens if $\sum K > 1$ (e.g., omitting the division by 9)?**
  A uniform gray region of value $100$ would become:
  
  $$h[m, n] = 9 \times 100 = 900$$
  
  The image rapidly saturates to pure blown-out white ($255$).
  * **What happens if $\sum K < 1$?**
  The image darkens proportionally with each successive pass of the filter.
  * **DC Gain (Zero-Frequency Response):** A normalized filter has a DC gain of $H(0, 0) = 1$, ensuring that constant, uniform image regions are completely unaltered:
  
  $$\text{If } f[m, n] = C \quad \forall (m, n) \implies h[m, n] = \frac{1}{9}(9C) = C$$
  
  ---
## 3. Generalizing to Matrix Dot Products (Slides 15–21)
  
  Slides 15–21 show how local averaging can be formalized as a **linear weighted inner product**.
### A. Vector Dot Product vs. Matrix Frobenius Product
  * **1D Vector Dot Product:**
  
  $$\mathbf{a} \cdot \mathbf{b} = \begin{bmatrix} a_1 & a_2 & a_3 \end{bmatrix} \begin{bmatrix} b_1 \\ b_2 \\ b_3 \end{bmatrix} = \sum_{i=1}^3 a_i b_i$$
  
  * **2D Matrix Inner Product (Element-wise multiplication followed by summation):**
  For two matrices $\mathbf{A}, \mathbf{B} \in \mathbb{R}^{M \times N}$, this is the Frobenius inner product $\langle \mathbf{A}, \mathbf{B} \rangle_F$:
  
  $$\mathbf{A} : \mathbf{B} = \text{Tr}(\mathbf{A}^T \mathbf{B}) = \sum_{i=1}^M \sum_{j=1}^N A_{ij} B_{ij}$$
  
  ```
   ┌───┬───┬───┐       ┌───┬───┬───┐
   │ a │ b │ c │   *   │ A │ B │ C │  ===>  aA + bB + cC + dD + eE + fF
   ├───┼───┼───┤       ├───┼───┼───┤
   │ d │ e │ f │       │ D │ E │ F │
   └───┴───┴───┘       └───┴───┴───┘
     Kernel G              Patch F          Sum of Element-wise Products
  ```
### B. Python Implementation: Naive Loops vs. Vectorized Kernel Slicing (Slides 12, 19–21)
#### 1. Naive Implementation (Slide 12):
  ```python
  import numpy as np
  
  N = 10
  data = np.zeros((N, N), dtype=np.float32)
  data[2, 3] = 1.0
  data[4:7, 5:8] = 1.0
  
  data_filtered = np.zeros_like(data)
  
  # Hardcoded box summation over 3x3 patch
  for ii in range(1, N - 1):
    for jj in range(1, N - 1):
        # Slicing is [inclusive : non-inclusive]
        data_filtered[ii, jj] = data[ii - 1 : ii + 2, jj - 1 : jj + 2].sum() / 9.0
  ```
#### 2. Generalized Kernel Implementation (Slide 19):
  ```python
  # Define arbitrary filter weights as a matrix
  box_kernel = np.ones((3, 3), dtype=np.float32) / 9.0
  
  data_filtered = np.zeros_like(data)
  
  # Slide the kernel: multiply patch element-wise, then sum
  for ii in range(1, N - 1):
    for jj in range(1, N - 1):
        patch = data[ii - 1 : ii + 2, jj - 1 : jj + 2]
        data_filtered[ii, jj] = np.sum(box_kernel * patch)
  ```
  
  > **Takeaway (Slide 21):** Abstracting the averaging operation into a **weight matrix (kernel)** allows us to swap in different matrices to perform completely different visual tasks—such as blurring, sharpening, shifting, or edge detection—using the exact same algorithmic pipeline.
  
  ---
## 4. Formal Foundations: Linear Shift-Invariant (LSI) Systems (Slide 27)
  
  A filter $\mathcal{H}$ operating on an image $f$ is a **Linear Shift-Invariant (LSI)** system if and only if it satisfies two fundamental properties:
  
  ```
                            [ LSI System Properties ]
                                        │
           ┌────────────────────────────┴────────────────────────────┐
           ▼                                                         ▼
     [ LINEARITY ]                                         [ SHIFT-INVARIANCE ]
  Superposition & Scaling                               Homogeneity Across Space
  H(a·f₁ + b·f₂) = a·H(f₁) + b·H(f₂)                   If  f[m, n] ──► g[m, n]
  • No cross-frequency distortion                      Then f[m-m₀, n-n₀] ──► g[m-m₀, n-n₀]
  • Zero input gives zero output                       • The filter acts everywhere identically
  ```
### A. Linearity (Superposition Principle)
  For any scalars $a, b$ and images $f_1, f_2$:
  
  $$\mathcal{H}\big(a \cdot f_1[m, n] + b \cdot f_2[m, n]\big) = a \cdot \mathcal{H}\big(f_1[m, n]\big) + b \cdot \mathcal{H}\big(f_2[m, n]\big)$$
  
  * **Implication:** You can decompose a complex image into a sum of primitive components (e.g., basis impulses or sinusoids), process each component independently through the filter, and sum the results.
### B. Shift-Invariance (Translation Invariance)
  If shifting the input spatially by $(m_0, n_0)$ shifts the output by the exact same displacement:
  
  $$\text{Let } g[m, n] = \mathcal{H}\big(f[m, n]\big)$$
  
  $$\mathcal{H}\big(f[m - m_0, \, n - n_0]\big) = g[m - m_0, \, n - n_0]$$
  
  * **Implication:** The filter's behavior does not depend on absolute pixel coordinates. A coffee cup detected in the upper-left corner of an image will be processed with the exact same mathematical weights if it appears in the bottom-right corner.
  
  > **Riesz-Fréchet Representation Theorem:** 
  > Any discrete linear shift-invariant (LSI) system can be completely and uniquely characterized by its response to an isolated impulse $\delta[m, n]$ (its **Impulse Response**), and its operation is equivalent to **Convolution**.
  
  ---
## 5. Correlation vs. Convolution: The Flipped Coordinate (Slides 35–38)
  
  The distinction between **Cross-Correlation** and **Convolution** is a frequent source of implementation bugs.
  
  ```
       Cross-Correlation (Slide 35)                      Convolution (Slides 36-38)
       ───────────────────────────                      ─────────────────────────
       Slide the kernel DIRECTLY                        FLIP the kernel horizontally 
       over the image:                                  and vertically, then slide:
  
            ┌───┬───┐                                        ┌───┬───┐
            │ a │ b │                                        │ d │ c │  <-- Rotated
            ├───┼───┤                                        ├───┼───┤      180°
            │ c │ d │                                        │ b │ a │
            └───┴───┘                                        └───┴───┘
  ```
  
  ---
### A. 2D Discrete Cross-Correlation ($\otimes$) (Slide 35)
  Cross-correlation measures the localized template similarity between an image $f$ and a sliding kernel $g$:
  
  $$h[m, n] = (f \otimes g)[m, n] = \sum_{k} \sum_{l} g[k, l] \cdot f[m + k, \, n + l]$$
  
  * **Notice the signs:** Both indices in $f[m + k, n + l]$ are **positive**. The kernel moves across the image without reflection.
  
  ---
### B. 2D Discrete Convolution ($*$) (Slides 36–37)
  Mathematically, convolution introduces a **negative sign** (spatial reflection) on the indexing coordinates:
  
  $$h[m, n] = (f * g)[m, n] = \sum_{k} \sum_{l} g[k, l] \cdot f[m - k, \, n - l]$$
  
  By changing variables ($i = m - k$, $j = n - l$), convolution can be equivalently written as:
  
  $$h[m, n] = \sum_{i} \sum_{j} f[i, j] \cdot g[m - i, \, n - j]$$
  
  ---
### C. Why Does Convolution Flip the Kernel?
  1. **Algebraic Commutativity:**
   Cross-correlation is **not commutative**:
  
  $$f \otimes g \neq g \otimes f$$
  
   Convolution **is commutative**:
  
  $$f * g = g * f$$
  
   Commutativity is essential for formal signal processing: cascading filter $A$ followed by filter $B$ yields the exact same output as cascading filter $B$ followed by filter $A$:
  
  $$\mathcal{H}_2 \big(\mathcal{H}_1(f)\big) = f * (h_1 * h_2) = f * (h_2 * h_1)$$
  
  2. **Associativity:**
  
  $$(f * g) * h = f * (g * h)$$
  
   This allows multi-stage filtering pipelines to be pre-convolved into a single combined kernel.
  
  ---
### D. The Symmetry Simplification (Slide 38)
  If a kernel is **centrally symmetric** (invariant to a $180^\circ$ rotation about its center):
  
  $$g[k, l] = g[-k, -l]$$
  
  $$\text{Then: } \quad \text{Convolution} \equiv \text{Correlation}$$
  
  * **Examples:** The Box Filter and Gaussian Filter are both radially symmetric, meaning convolution and correlation produce **identical outputs**.
  * **Counterexamples:** Directional derivative operators (such as Sobel or right-shift kernels) are anti-symmetric or asymmetric; for these, the $180^\circ$ flip reverses the direction of the operation.
  
  ```python
  from scipy.signal import convolve2d, correlate2d
  
  # Asymmetric kernel
  asym_kernel = np.array([[0, 0, 0],
                        [0, 0, 1],
                        [0, 0, 0]])
  
  # convolve2d flips the kernel 180 degrees before sliding
  conv_res = convolve2d(image, asym_kernel, mode='same')
  
  # correlate2d slides the kernel directly without flipping
  corr_res = correlate2d(image, asym_kernel, mode='same')
  ```
  
  ---
## 6. Separable Filters & Computational Complexity (Slides 39–42)
  
  Applying a 2D convolution directly over large images with sizable kernels is computationally expensive. **Filter Separability** provides an algorithmic optimization that reduces this cost.
  
  ---
### A. Mathematical Definition of Separability
  A 2D kernel $\mathbf{K} \in \mathbb{R}^{N \times N}$ is **separable** if it can be factored as the outer product of two 1D vectors $\mathbf{u} \in \mathbb{R}^{N \times 1}$ and $\mathbf{v} \in \mathbb{R}^{1 \times N}$:
  
  $$\mathbf{K} = \mathbf{u} \cdot \mathbf{v}^T$$
  
  $$K[i, j] = u[i] \cdot v[j]$$
  
  ```
          ┌───┐
          │ 1 │                     ┌───┬───┬───┐
     1    ├───┤        1            │ 1 │ 1 │ 1 │
    ─── × │ 1 │   *   ─── [ 1 1 1 ] = ─── ├───┼───┼───┤
     3    ├───┤        3          9 │ 1 │ 1 │ 1 │
          │ 1 │                     ├───┼───┼───┤
          └───┘                     │ 1 │ 1 │ 1 │
         Vertical                 Horizontal   └───┴───┴───┘
         1D Kernel                 1D Kernel    2D Box Filter
  ```
  
  By the associative property of convolution, convolving an image $f$ with the 2D kernel $\mathbf{K}$ is equivalent to two consecutive 1D convolutions:
  
  $$f * \mathbf{K} = f * (\mathbf{u} \cdot \mathbf{v}^T) = (f * \mathbf{v}^T) * \mathbf{u}$$
  
  1. Convolve every row of the image with the horizontal 1D kernel $\mathbf{v}^T$.
  2. Convolve every column of the intermediate result with the vertical 1D kernel $\mathbf{u}$.
  
  ---
### B. Computational Complexity Analysis (Slides 40–42)
  Let the image dimensions be $M \times M$ and the square filter dimensions be $N \times N$.
  
  ```
  ┌──────────────────────────────────────┬────────────────────────┬────────────────────────┐
  │ Metric                               │ Non-Separable 2D Filter│ Separable 1D Cascade   │
  ├──────────────────────────────────────┼────────────────────────┼────────────────────────┤
  │ Operations Per Output Pixel          │ N² multiplications     │ 2N multiplications     │
  │ Total Operations for M × M Image     │ N² · M²                │ 2N · M²                │
  │ Algorithmic Complexity Class         │ O(N² M²)               │ O(N M²)                │
  └──────────────────────────────────────┴────────────────────────┴────────────────────────┘
  ```
#### Speedup Factor:
  
  $$\text{Speedup} = \frac{N^2 M^2}{2N M^2} = \frac{N}{2}$$
#### Numerical Case Studies:
  * **Small Filter ($N = 3$, Slides 41–42):**
  * Image: $4 \times 4$ ($M = 4$, 16 pixels total).
  * 2D Direct: $3^2 \times 4^2 = 9 \times 16 = \mathbf{144\text{ operations}}$.
  * Separable: $2 \times 3 \times 4^2 = 6 \times 16 = \mathbf{96\text{ operations}}$ ($\sim 33\%$ reduction).
  * **Medium Filter ($N = 11$):**
  
  $$\text{Speedup} = \frac{11}{2} = \mathbf{5.5\times\text{ faster}}$$
  
  * **Large Blur ($N = 31$):**
  
  $$\text{Speedup} = \frac{31}{2} = \mathbf{15.5\times\text{ faster}}$$
  
  For real-time vision pipelines processing $4\text{K}$ video ($3840 \times 2160$), non-separable filtering often drops frame rates below real-time thresholds, whereas separable execution remains performant on standard hardware.
  
  ---
### C. The Linear Algebra Condition for Separability
  How can you determine whether an arbitrary $N \times N$ matrix $\mathbf{K}$ is separable?
  
  > **The Rank-1 Theorem:** 
  > A 2D matrix $\mathbf{K}$ is separable if and only if its **matrix rank is exactly 1**:
  
  $$\text{rank}(\mathbf{K}) = 1$$
#### Proof via Singular Value Decomposition (SVD):
  Any matrix $\mathbf{K} \in \mathbb{R}^{N \times N}$ can be factored using SVD:
  
  $$\mathbf{K} = \sum_{i=1}^R \sigma_i \, \mathbf{u}_i \mathbf{v}_i^T$$
  
  where $\sigma_1 \ge \sigma_2 \ge \dots \ge \sigma_R > 0$ are the singular values, and $R = \text{rank}(\mathbf{K})$.
  * If $\text{rank}(\mathbf{K}) = 1$, then $\sigma_1 > 0$ and $\sigma_2 = \sigma_3 = \dots = 0$:
  
  $$\mathbf{K} = \sigma_1 \mathbf{u}_1 \mathbf{v}_1^T = \big(\sqrt{\sigma_1}\mathbf{u}_1\big) \big(\sqrt{\sigma_1}\mathbf{v}_1^T\big)$$
  
  The filter decomposes into two 1D kernels. If $\text{rank}(\mathbf{K}) > 1$, the filter cannot be separated without error (though low-rank approximations can still be constructed by retaining only $\sigma_1$).
  
  ---
## 7. Boundary Conditions & Padding Strategies
  
  In Slides 6–10 and the Python implementations (Slides 12, 19), the for-loops strictly iterate through interior pixels:
  
  ```python
  for ii in range(1, N - 1):
    for jj in range(1, N - 1):
  ```
  
  This skips the outermost 1-pixel border, causing the filtered output to shrink from $M \times M$ to $(M - 2k) \times (M - 2k)$. To preserve image dimensions, we must apply **boundary padding**:
  
  ```
                       Padding Strategies for an Image Boundary
                       
      Original Border: [ a  b  c  d ... ]
      
      Zero Padding:    [ 0  0  a  b  c  d ... ]  (Introduces dark artificial borders)
      Clamp/Replicate: [ a  a  a  b  c  d ... ]  (Repeats edge pixel value)
      Reflect/Mirror:  [ c  b  a  b  c  d ... ]  (Standard for visual smoothing)
      Circular/Wrap:   [ y  z  a  b  c  d ... ]  (Assumes periodic signal; used in FFT)
  ```
  
  1. **Zero Padding (Constant):** Fills missing values with $0$. Downside: creates artificial high-contrast step edges at image borders, causing dark fringe artifacts during smoothing.
  2. **Replicate (Clamp / Border Extension):** Extends boundary pixels outward. Effective for general photography.
  3. **Reflect (Symmetric Mirroring):** Reflects pixels across the boundary line ($f[-1] = f[0], f[-2] = f[1]$). **Preferred standard for image filtering** because it avoids artificial discontinuities.
  4. **Circular (Periodic / Toroidal):** Wraps around from the opposite edge. Mathematically required when filtering via the Fast Fourier Transform (FFT).
  
  ---
## Summary Matrix: Part 1 Concepts
  
  | Concept | Mathematical Formulation | Operational Property | Computational Impact |
  | :--- | :--- | :--- | :--- |
  | **Point Operator** | $g(x, y) = T(f(x, y))$ | Zero spatial context | $\mathcal{O}(1)$ per pixel via Look-Up Tables |
  | **Cross-Correlation** | $(f \otimes g)[m, n] = \sum_{k,l} g[k, l] f[m+k, n+l]$ | Slides kernel directly | Non-commutative ($f \otimes g \neq g \otimes f$) |
  | **Convolution** | $(f * g)[m, n] = \sum_{k,l} g[k, l] f[m-k, n-l]$ | Flips kernel by $180^\circ$ | Commutative ($f * g = g * f$) and Associative |
  | **Box Filter** | $K = \frac{1}{N^2} \mathbf{1}_{N \times N}$ | Uniform local average | Fast local smoothing, but prone to square artifacts |
  | **Separable Filter** | $\mathbf{K} = \mathbf{u} \cdot \mathbf{v}^T \iff \text{rank}(\mathbf{K}) = 1$ | Two consecutive 1D passes | Reduces operations from $\mathcal{O}(N^2 M^2)$ to $\mathcal{O}(2N M^2)$ |
  
  ---