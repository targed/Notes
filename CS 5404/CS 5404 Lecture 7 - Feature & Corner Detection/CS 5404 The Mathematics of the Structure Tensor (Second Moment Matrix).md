## 1. The Naive Gradient Trap: Why $I_x$ and $I_y$ Alone Cannot Detect Corners (Slides 25–26)
  
  In **Module M1.4**, we derived directional image derivatives using Sobel filters:
  * $I_x = \frac{\partial I}{\partial x}$ (measures horizontal intensity changes).
  * $I_y = \frac{\partial I}{\partial y}$ (measures vertical intensity changes).
  
  Slide 25 raises an intuitive hypothesis:
  > **"Can we detect where a corner exists simply by finding regions where both $I_x$ and $I_y$ have large magnitudes?"**
  > 
  > *Answer:* **No.** Thresholding both derivatives simultaneously ($|I_x| > \tau$ and $|I_y| > \tau$) fundamentally fails.
  
  ```
     The Diagonal Edge Counterexample (Slide 26)
     
            y ^               Diagonal Edge Contour (45°)
              │              /
              │             /   Intensity = 0 (Background)
              │            / 
              │  Normal n / 
              │     ▲    / 
              │      \  / 
              │       \/  Intensity = 255 (Polygon)
              │       /\
              │      /  \
              └─────┴────\──────────────────► x
                         /
  ```
  
  ---
### Why the Naive Test Fails on Diagonal Edges
  Consider a straight edge tilted at $45^\circ$ across the image plane:
  1. The unit surface normal vector pointing across the edge is:
  
  $$\mathbf{n} = \begin{bmatrix} \cos(45^\circ) \\ \sin(45^\circ) \end{bmatrix} = \begin{bmatrix} \frac{1}{\sqrt{2}} \\ \frac{1}{\sqrt{2}} \end{bmatrix}$$
  
  2. Evaluating spatial gradients along this straight line gives:
  
  $$I_x = \|\nabla I\| \cos(45^\circ) = \frac{\|\nabla I\|}{\sqrt{2}} \gg 0$$
  
  $$I_y = \|\nabla I\| \sin(45^\circ) = \frac{\|\nabla I\|}{\sqrt{2}} \gg 0$$
  
  3. Both $I_x$ and $I_y$ are large simultaneously along every pixel of the line.
  4. **The False Positive:** A naive detector would classify the **entire length of a straight diagonal line as a continuous chain of corners**.
  5. **The Aperture Problem Remains:** If you shift a local window parallel to the diagonal line (along unit tangent $\mathbf{t} = [-\frac{1}{\sqrt{2}}, \frac{1}{\sqrt{2}}]^T$), the directional derivative is zero:
  
  $$\nabla I \cdot \mathbf{t} = I_x \left(-\frac{1}{\sqrt{2}}\right) + I_y \left(\frac{1}{\sqrt{2}}\right) = -\frac{\|\nabla I\|}{2} + \frac{\|\nabla I\|}{2} = \mathbf{0}$$
  
  There is zero intensity change along the edge tangent. The feature remains 1D-ambiguous and cannot be localized down to a single point.
  
  > **Key Rule (Slide 26):** Having large horizontal and vertical derivatives is a **necessary but insufficient** condition for a corner. A true corner requires that moving a local patch in **any arbitrary direction** produces significant intensity variations.
  
  ---
## 2. The Auto-Correlation / Sum of Squared Differences (SSD) Formulation (Slides 27–28)
  
  To determine whether a local patch centered at $(x, y)$ contains a corner, we evaluate how much the patch changes when shifted by an arbitrary 2D displacement vector $\mathbf{u} = \begin{bmatrix} u & v \end{bmatrix}^T$.
  
  ```
                            Window Shift Perturbation Test
                            
        Original Window at (x, y)                  Shifted Window at (x+u, y+v)
          ┌───┬───┬───┐                              ┌───┬───┬───┐
          │   │   │   │                              │   │   │   │
          ├───┼───┼───┤       Displace by (u, v)     ├───┼───┼───┤
          │   │ • │   │      ──────────────────►     │   │ • │   │
          ├───┼───┼───┤                              ├───┼───┼───┤
          │   │   │   │                              │   │   │   │
          └───┴───┴───┘                              └───┴───┴───┘
              W(x, y)                                    W(x+u, y+v)
                 │                                          │
                 └────────────────────┬─────────────────────┘
                                      ▼
                      Sum of Squared Differences (SSD):
                      E(u, v) = Σ [ I(x+u, y+v) - I(x, y) ]²
  ```
  
  ---
### A. The Auto-Correlation Error Metric (Slide 28)
  Let $W$ be a local spatial window (typically $3 \times 3$, $5 \times 5$, or Gaussian-weighted). The **Sum of Squared Differences (SSD)** error surface $E(u, v)$ is defined as:
  
  $$E(u, v) = \sum_{(x, y) \in W} w(x, y) \cdot \big[ I(x + u, \, y + v) - I(x, y) \big]^2$$
  
  where $w(x, y)$ is a weighting window:
  * **Rectangular Window:** $w(x, y) = 1$ inside $W$, $0$ outside.
  * **Gaussian Window:** $w(x, y) = \exp\left(-\frac{x^2 + y^2}{2\sigma_w^2}\right)$ (provides smooth, rotationally isotropic weighting).
  
  ---
### B. Analyzing $E(u, v)$ for Canonical Topologies
  1. **Flat Region:** 
  
  $$I(x + u, y + v) \approx I(x, y) \implies E(u, v) \approx 0 \quad \forall (u, v)$$
  
   $E(u, v)$ remains near zero for all displacements.
  2. **Straight Edge Region:** 
   * If $(u, v)$ points perpendicular to the edge $\implies E(u, v)$ is large.
   * If $(u, v)$ points along the edge tangent $\implies E(u, v) \approx 0$.
   * $E(u, v)$ forms a **trough / valley** with zero resistance along one direction.
  3. **Corner Region:** 
  
  $$E(u, v) \gg 0 \quad \forall (u, v) \neq (0, 0)$$
  
   Shifting the window in **any direction** causes the edge boundaries to cross, driving $E(u, v)$ to large positive values.
  
  ---
### C. The Computational Problem
  Evaluating $E(u, v)$ directly requires shifting the window over a grid of displacements (e.g., $u \in \{-2, \dots, +2\}, v \in \{-2, \dots, +2\}$) for **every single pixel** in an image. For a $1000 \times 1000$ image, this brute-force approach requires hundreds of millions of memory fetches, making real-time execution impossible.
  
  ---
## 3. First-Order Taylor Series Linearization (Slides 29–30)
  
  Slide 29 introduces a classic applied mathematics technique:
  > **Taylor Approximation:** Instead of physically sliding the window to measure $I(x + u, y + v)$, we approximate the shifted intensity analytically using the image's spatial derivatives.
  
  ```
       Continuous 1D Analogy: Linearizing a Curve
       
       Intensity I(x)
            ^
            │                                  True Curve I(x + u)
            │                                      ╭─────────
            │                                  ╭───╯
        I(x)│───────────────────────•───────--/  Linear Approximation:
            │                      /│        /   I(x + u) ≈ I(x) + I_x · u
            │                     / │       /
            │           Slope I_x/  │      /
            └───────────────────┴───┴─────┴──────────────────► Spatial x
                                x   x+u
  ```
  
  ---
### A. The 2D Taylor Expansion (Slide 29)
  The multivariable Taylor expansion of $I(x + u, y + v)$ around $(u, v) = (0, 0)$ is:
  
  $$I(x + u, \, y + v) = I(x, y) + \frac{\partial I}{\partial x} u + \frac{\partial I}{\partial y} v + \frac{1}{2} \left( \frac{\partial^2 I}{\partial x^2} u^2 + 2\frac{\partial^2 I}{\partial x \partial y} u v + \frac{\partial^2 I}{\partial y^2} v^2 \right) + \dots$$
  
  Assuming the displacement $(u, v)$ is small (sub-pixel or single-pixel shifts), higher-order terms become negligible:
  
  $$I(x + u, \, y + v) \approx I(x, y) + I_x u + I_y v$$
  
  Using vector notation:
  
  $$I(x + u, \, y + v) \approx I(x, y) + \begin{bmatrix} I_x & I_y \end{bmatrix} \begin{bmatrix} u \\ v \end{bmatrix}$$
  
  where $I_x = \frac{\partial I}{\partial x}$ and $I_y = \frac{\partial I}{\partial y}$.
  
  ---
### B. Substituting into the Auto-Correlation Function (Slide 30)
  Substitute the linear Taylor expansion into the SSD equation:
  
  $$E(u, v) = \sum_{(x, y) \in W} \Big[ I(x + u, \, y + v) - I(x, y) \Big]^2$$
  
  $$E(u, v) \approx \sum_{(x, y) \in W} \Big[ \big( I(x, y) + I_x u + I_y v \big) - I(x, y) \Big]^2$$
  
  The baseline intensity $I(x, y)$ cancels out:
  
  $$E(u, v) \approx \sum_{(x, y) \in W} \big[ I_x u + I_y v \big]^2$$
  
  > **The Critical Algebraic Step:** 
  > The unknown displacement variables $(u, v)$ are now decoupled from the spatial image coordinates $(x, y)$, allowing them to be factored outside the summation.
  
  ---
## 4. Derivation of the Structure Tensor (Second Moment Matrix) (Slides 31–32)
  
  Slide 31 expands the squared term algebraically:
  
  $$\big[ I_x u + I_y v \big]^2 = I_x^2 u^2 + 2 I_x I_y u v + I_y^2 v^2$$
  
  Distributing the summation over the window $W$:
  
  $$E(u, v) \approx \sum_{(x, y) \in W} \left( I_x^2 u^2 + 2 I_x I_y u v + I_y^2 v^2 \right)$$
  
  $$E(u, v) \approx u^2 \left( \sum_{(x, y) \in W} I_x^2 \right) + 2 u v \left( \sum_{(x, y) \in W} I_x I_y \right) + v^2 \left( \sum_{(x, y) \in W} I_y^2 \right)$$
  
  Let:
  
  $$A = \sum_{(x, y) \in W} I_x^2$$
  
  $$B = \sum_{(x, y) \in W} I_x I_y$$
  
  $$C = \sum_{(x, y) \in W} I_y^2$$
  
  The error equation simplifies to a classical **quadratic polynomial**:
  
  $$E(u, v) \approx A u^2 + 2 B u v + C v^2$$
  
  ---
### Converting to Matrix Quadratic Form (Slide 32)
  We can rewrite this polynomial as a symmetric matrix quadratic form:
  
  $$E(u, v) \approx \begin{bmatrix} u & v \end{bmatrix} \begin{bmatrix} A & B \\ B & C \end{bmatrix} \begin{bmatrix} u \\ v \end{bmatrix}$$
  
  $$E(u, v) \approx \mathbf{u}^T \mathbf{H} \, \mathbf{u}$$
  
  where $\mathbf{u} = \begin{bmatrix} u \\ v \end{bmatrix}$ is the displacement vector, and $\mathbf{H}$ is the **Structure Tensor** (also referred to as the **Second Moment Matrix** $\mathbf{M}$):
  
  $$\mathbf{H} = \begin{bmatrix} A & B \\ B & C \end{bmatrix} = \begin{bmatrix} \sum_{W} I_x^2 & \sum_{W} I_x I_y \\[6pt] \sum_{W} I_x I_y & \sum_{W} I_y^2 \end{bmatrix}$$
  
  ```
                   The Anatomy of the Structure Tensor H
                   
          ┌────────────────────────────────────────────────────────┐
          │                                                        │
          │     H = Σ  [  Ix²       Ix·Iy  ]                       │
          │         W  [  Ix·Iy      Iy²   ]                       │
          │                                                        │
          └──────────────────────────┬─────────────────────────────┘
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
    A = Σ Ix²                   B = Σ Ix·Iy                 C = Σ Iy²
  Total horizontal            Cross-correlation           Total vertical
  gradient energy             between gradient axes       gradient energy
  in window W                 in window W                 in window W
  ```
  
  ---
### Tensor Product Formulation: Sum of Outer Products
  Using vector outer products, the Structure Tensor can be written concisely as:
  
  $$\mathbf{H} = \sum_{(x, y) \in W} \nabla I(x, y) \cdot \nabla I(x, y)^T = \sum_{(x, y) \in W} \begin{bmatrix} I_x \\ I_y \end{bmatrix} \begin{bmatrix} I_x & I_y \end{bmatrix}$$
#### Linear Algebra Insight:
  * At an individual pixel $(x, y)$, the matrix $\nabla I \, \nabla I^T$ is an outer product of a vector with itself. Its determinant is zero, meaning it has a **rank of exactly 1**:
  
  $$\det\left( \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix} \right) = I_x^2 I_y^2 - (I_x I_y)^2 = \mathbf{0}$$
  
  * An isolated single pixel can only measure gradients in one direction.
  * However, when we sum $\nabla I \, \nabla I^T$ over a neighborhood window $W$ containing intersecting contours, **the sum of multiple rank-1 matrices can produce a full-rank ($\text{rank} = 2$) matrix**:
  
  $$\text{rank}(\mathbf{H}) = 2 \iff \text{True Corner Region}$$
  
  ---
## 5. Geometry of the Quadratic Error Surface $E(u, v)$ (Slides 32–34)
  
  The equation $E(u, v) \approx \begin{bmatrix} u & v \end{bmatrix} \mathbf{H} \begin{bmatrix} u \\ v \end{bmatrix}$ defines an implicit 3D surface $z = E(u, v)$ over displacement coordinates $(u, v)$. 
  
  Analyzing the shape of this surface reveals whether a patch is a flat region, an edge, or a corner.
  
  ```
       Surface Shapes of E(u, v)
       
       1. Flat Region:                  2. Straight Edge (Ridge):       3. Corner (Elliptic Paraboloid):
             ^ z                              ^ z                              ^ z
             │                                │      /│                        │        )
             │                                │     / │                        │       (   )
             │                                │    /  │                        │      (     )
       ──────┴──────► u                 ──────┴───/───┴──► u             ──────┴─────(_______)──► u
            /                                /   /                            /      Bowl shape:
           v                                v   /                            v       Steep increase
         Flat Plane                      Parabolic Gutter                            in ALL directions!
         (Zero curvature)                (Curved in 1D, flat in 1D)
  ```
  
  ---
### A. Case 1: Flat Homogeneous Surface
  * $I_x \approx 0$ and $I_y \approx 0$ across all pixels in window $W$.
  * Matrix entries: $A \approx 0, B \approx 0, C \approx 0 \implies \mathbf{H} \approx \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}$.
  * **Surface Shape:** A completely flat horizontal plane resting at $z = 0$.
  * **Physical Meaning:** Moving the window in any direction generates zero change ($E(u, v) = 0$).
  
  ---
### B. Case 2: Horizontal Edge (Slide 33)
  Consider a horizontal boundary separating a light region above from a dark region below:
  * Intensity varies vertically ($I_y \neq 0$), but remains constant horizontally ($I_x = 0$).
  * Matrix entries:
  
  $$A = \sum I_x^2 = 0, \quad B = \sum I_x I_y = 0, \quad C = \sum I_y^2 > 0$$
  
  $$\mathbf{H} = \begin{bmatrix} 0 & 0 \\ 0 & C \end{bmatrix}$$
  
  * Substituting $\mathbf{H}$ into the quadratic form:
  
  $$E(u, v) \approx \begin{bmatrix} u & v \end{bmatrix} \begin{bmatrix} 0 & 0 \\ 0 & C \end{bmatrix} \begin{bmatrix} u \\ v \end{bmatrix} = C v^2$$
  
  ```
                       Horizontal Edge Error Surface: E(u, v) = C·v²
                                            ^ z (Error)
                                            │       /│
                                            │      / │
                                            │     /  │  <-- Parabola along v-axis
                                     ───────┼────/───┼────────► v
                                           /│   /
                                          / └──/──────────────► u (Horizontal Shift)
                                         v   Trough along u-axis:
                                             E(u, 0) = 0 for ANY u!
  ```
  
  * **Surface Shape (Slide 33):** A **parabolic gutter / ridge** running parallel to the $u$-axis.
  * **The Aperture Problem Visualized:** If we slide the window horizontally along the edge ($v = 0$), then for any displacement $u$:
  
  $$E(u, 0) = C(0)^2 = \mathbf{0}$$
  
  The error remains zero, meaning horizontal shifts are undetectable.
  
  ---
### C. Case 3: Vertical Edge (Slide 34)
  Consider a vertical boundary separating a light region on the left from a dark region on the right:
  * Intensity varies horizontally ($I_x \neq 0$), but remains constant vertically ($I_y = 0$).
  * Matrix entries:
  
  $$A = \sum I_x^2 > 0, \quad B = \sum I_x I_y = 0, \quad C = \sum I_y^2 = 0$$
  
  $$\mathbf{H} = \begin{bmatrix} A & 0 \\ 0 & 0 \end{bmatrix}$$
  
  * Substituting into the quadratic form:
  
  $$E(u, v) \approx A u^2$$
  
  * **Surface Shape (Slide 34):** A parabolic gutter aligned with the $v$-axis. Shifting vertically along the edge produces zero error: $E(0, v) = 0$.
  
  ---
### D. Case 4: True Corner (Slide 32)
  At an intersecting vertex, gradients exist along multiple distinct orientations:
  * Both $A = \sum I_x^2$ and $C = \sum I_y^2$ are strictly positive.
  * The determinant $\det(\mathbf{H}) = AC - B^2 > 0$.
  * The matrix $\mathbf{H}$ is **strictly positive definite**.
  * **Surface Shape:** An **elliptic paraboloid (a steep bowl)** with its global minimum anchored at the origin $(0, 0)$.
  * **Physical Meaning:** Any displacement $(u, v) \neq (0, 0)$ moves up the steep walls of the bowl, producing a large error. The point is localized in both dimensions.
  
  ---
## Summary Matrix: The Second Moment Matrix & Surface Topologies
  
  | Local Region Type | Gradient Energy Profile | Structure Tensor $\mathbf{H}$ Structure | Error Surface Geometry $E(u, v)$ | Mathematical Rank |
  | :--- | :--- | :--- | :--- | :--- |
  | **Flat** | $I_x \approx 0, \, I_y \approx 0$ | $\mathbf{H} \approx \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}$ | Flat horizontal plane ($z \approx 0$) | $\text{rank}(\mathbf{H}) = 0$ |
  | **Horizontal Edge** | $I_x = 0, \, I_y \gg 0$ | $\mathbf{H} = \begin{bmatrix} 0 & 0 \\ 0 & C \end{bmatrix}$ | Parabolic gutter along $u$-axis ($z = C v^2$) | $\text{rank}(\mathbf{H}) = 1$ |
  | **Vertical Edge** | $I_x \gg 0, \, I_y = 0$ | $\mathbf{H} = \begin{bmatrix} A & 0 \\ 0 & 0 \end{bmatrix}$ | Parabolic gutter along $v$-axis ($z = A u^2$) | $\text{rank}(\mathbf{H}) = 1$ |
  | **Diagonal Edge ($45^\circ$)** | $I_x = I_y \gg 0$ | $\mathbf{H} = \begin{bmatrix} A & A \\ A & A \end{bmatrix}$ | Parabolic gutter along diagonal tangent | $\text{rank}(\mathbf{H}) = 1$ |
  | **Corner / Vertex** | Multi-directional gradients | $\mathbf{H} = \begin{bmatrix} A & B \\ B & C \end{bmatrix} \succ 0$ | **Elliptic Paraboloid (Steep Bowl)** | $\mathbf{\text{rank}(\mathbf{H}) = 2}$ |
  
  ---