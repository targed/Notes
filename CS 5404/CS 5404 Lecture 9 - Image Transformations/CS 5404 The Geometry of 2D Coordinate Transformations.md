## 1. Motivation: Domain vs. Range Transformations (Slides 2–12)
  
  In **Modules M1.3 and M1.4**, we manipulated images through **Range Transformations**:
  
  $$g(\mathbf{x}) = h\big(f(\mathbf{x})\big) \quad \text{or} \quad g(\mathbf{x}) = (f * h)(\mathbf{x})$$
  
  Range transformations modify **what value** is stored at a coordinate (adjusting intensity, contrast, or frequency bands), but the spatial coordinates $\mathbf{x} = (x, y)$ themselves remain locked to their original grid locations.
  
  ```
     Range Transformation (Filtering)                  Domain Transformation (Warping)
     
     f(x)                                              f(x)
      ^                                                 ^
      │      ╭───────                                   │      ╭───────
      │     ╭╯                                          │     ╭╯
      └─────┴──────────► x                              └─────┴──────────► x
             │                                                 │
             ▼ g(x) = h(f(x))                                  ▼ g(x) = f(h(x))
      ^                                                 ^
      │       ─────── (Attenuates amplitude,            │            ╭─────── (Shifts, stretches,
      │     ╭─╯        modifies output values)          │           ╭╯         rotates the grid!)
      └─────┴──────────► x                              └───────────┴────► x
  ```
  
  Slide 12 formalizes the transition to **Domain Transformations (Image Warping)**:
  > **Image Warping:** Modifies the spatial coordinate grid itself. The value of the output image $g$ at coordinate $\mathbf{x}$ is obtained by querying the original image $f$ at a geometrically transformed coordinate $\mathbf{x}' = h(\mathbf{x})$:
  > 
  > $$g(\mathbf{x}) = f\big(h(\mathbf{x})\big)$$
  
  ---
### A. The Registration Challenge (Slides 3–7)
  Slides 3–7 present a practical scenario: we want to match a front-facing canonical image of Richard Szeliski’s textbook cover with a photograph of the physical book resting obliquely on an office desk.
  
  ```
       Canonical Template (Frontal)                      Photograph on Desk (Oblique)
    ┌───────────────────────────────┐                  ┌────────────────────────────────────────┐
    │ ┌───────────────────────────┐ │                  │                                        │
    │ │      Computer Vision      │ │                  │               /──────/                 │
    │ │ Algorithms & Applications │ │                  │              / Book /                  │
    │ │                           │ │   ────────►      │             /______/                   │
    │ │       Richard Szeliski    │ │                  │                                        │
    │ └───────────────────────────┘ │                  │   Desk Surface (Perspective Skew)      │
    └───────────────────────────────┘                  └────────────────────────────────────────┘
  ```
  
  Slides 4–6 evaluate standard transformation primitives:
  1. **Scale and Translation Alone (Slide 4):** Fails. Shifting the template and adjusting its width/height cannot account for the fact that the book on the desk is angled.
  2. **Adding 2D In-Plane Rotation (Slide 5):** Still fails. Rotating the flat template aligns one edge, but the other edges diverge because the book is resting on a 3D plane receding into depth.
  3. **The Conclusion (Slide 7):** We need a generalized mathematical language capable of modeling **affine shears, out-of-plane tilts, and projective perspective foreshortening**.
  
  ---
## 2. 2D Linear Transformations via $2 \times 2$ Matrices (Slides 13–21)
  
  A spatial coordinate transformation $T$ is **global** if it applies the exact same mapping function to every coordinate in the image plane (Slide 14):
  
  $$\mathbf{p}' = T(\mathbf{p}) \iff \begin{bmatrix} x' \\ y' \end{bmatrix} = T\left(\begin{bmatrix} x \\ y \end{bmatrix}\right)$$
  
  ---
### A. Definition of a Linear Transformation
  In Euclidean linear algebra, a mapping $T: \mathbb{R}^2 \to \mathbb{R}^2$ is **linear** if and only if it preserves vector addition and scalar multiplication:
  
  $$T(\mathbf{u} + \mathbf{v}) = T(\mathbf{u}) + T(\mathbf{v})$$
  
  $$T(\alpha \mathbf{u}) = \alpha T(\mathbf{u})$$
  
  Every linear transformation in 2D space can be represented by a $2 \times 2$ matrix multiplication:
  
  $$\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} a & b \\ c & d \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix}$$
  
  ---
### B. The Classical $2 \times 2$ Linear Transformations
#### 1. Scaling (Slide 15, 18–19):
  Scales coordinates along horizontal and vertical axes:
  
  $$\mathbf{S} = \begin{bmatrix} s_x & 0 \\ 0 & s_y \end{bmatrix} \implies \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} s_x \cdot x \\ s_y \cdot y \end{bmatrix}$$
  
  * If $s_x = s_y = s$, the scaling is **isotropic** (uniform).
  * If $s_x \neq s_y$, the scaling is **anisotropic** (aspect ratio change, Slide 13).
  
  ```
                      Scaling Interpretation Dilemma (Slide 18–19)
                      
   Forward Coordinate Map: p' = S · p                      Continuous Image Function: g(x') = f(x'/s)
   ---------------------------------                      ------------------------------------------
   • Let s = 2.0                                          • Let s = 2.0
   • An input pixel at [1, 1] maps to [2, 2].             • Coordinate [2, 2] in g queries [1, 1] in f.
   • The geometric area of the shape GROWS by 4×!         • The visual image has EXPANDED (Zoomed In).
  ```
  
  ---
#### 2. In-Plane 2D Rotation (Slide 20):
  Rotates points counter-clockwise around the origin $(0, 0)$ by angle $\theta$:
  
  $$\mathbf{R} = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix}$$
  
  * **Properties:** $\mathbf{R}$ is an orthogonal matrix: $\mathbf{R}^T \mathbf{R} = \mathbf{I}$, and $\det(\mathbf{R}) = \cos^2\theta + \sin^2\theta = +1$.
  * Preserves Euclidean lengths, vector norms, and angles between intersecting lines.
  
  ---
#### 3. Shear / Skew (Slide 29–30):
  Displaces coordinates along one axis proportionally to their coordinate along the orthogonal axis:
  
  $$\mathbf{Sh}_x = \begin{bmatrix} 1 & sh_x \\ 0 & 1 \end{bmatrix} \implies \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} x + sh_x \cdot y \\ y \end{bmatrix}$$
  
  $$\mathbf{Sh}_y = \begin{bmatrix} 1 & 0 \\ sh_y & 1 \end{bmatrix} \implies \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} x \\ y + sh_y \cdot x \end{bmatrix}$$
  
  * **Visual Effect:** Slants square windows into parallelograms while preserving surface area ($\det(\mathbf{Sh}) = 1$).
  
  ---
#### 4. Reflection / Mirroring (Slide 21):
  * **Mirror about the Vertical Y-Axis ($x \to -x$):**
  
  $$\mathbf{T}_{\text{mirror-}y} = \begin{bmatrix} -1 & 0 \\ 0 & 1 \end{bmatrix}$$
  
  * **Mirror across the Main Diagonal Line ($y = x$):**
  
  $$\mathbf{T}_{\text{diag}} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} \implies \begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} y \\ x \end{bmatrix}$$
  
  * **Properties:** Reflections have a negative determinant: $\det(\mathbf{T}) = -1$. They invert the chirality (handedness) of the coordinate system.
  
  ---
## 3. The Translation Anomaly: Why $2 \times 2$ Matrices Break Down (Slides 22–23)
  
  Slide 22 asks a central question:
  > **"What about translation? Can we represent translation as a $2 \times 2$ matrix?"**
  
  Translation offsets coordinates by constant shifts $(t_x, t_y)$:
  
  $$x' = x + t_x$$
  
  $$y' = y + t_y$$
  
  Slide 23 delivers the answer:
  > **"No! Translation cannot be represented as a $2 \times 2$ matrix operation because it is NOT linear."**
  
  ```
       Why Translation is Non-Linear: The Origin Invariant
       
       Any linear transformation T must map the origin (0, 0) to itself:
       
                 T( 0 ) = T( 0 · v ) = 0 · T( v ) = 0
       
       In 2x2 matrix multiplication:
       
                 ┌───┬───┐ ┌───┐   ┌─────────────┐   ┌───┐
                 │ a │ b │ │ 0 │ = │ a·0  +  b·0 │ = │ 0 │
                 ├───┼───┤ ├───┤   ├─────────────┤   ├───┤
                 │ c │ d │ │ 0 │   │ c·0  +  d·0 │   │ 0 │
                 └───┴───┘ └───┘   └─────────────┘   └───┘
       
       A 2×2 matrix CANNOT move the origin! 
       Translation requires: [0, 0] ──► [t_x, t_y] ≠ [0, 0].
  ```
  
  Because translation moves the origin whenever $(t_x, t_y) \neq (0, 0)$, it violates the definition of linear maps. It belongs to the broader class of **Affine Transformations** ($\mathbf{p}' = \mathbf{A}\mathbf{p} + \mathbf{t}$). 
  
  However, handling transformations as a mix of matrix multiplications and vector additions prevents chaining operations cleanly:
  
  $$\mathbf{p}'' = \mathbf{A}_2 (\mathbf{A}_1 \mathbf{p} + \mathbf{t}_1) + \mathbf{t}_2 = \mathbf{A}_2 \mathbf{A}_1 \mathbf{p} + (\mathbf{A}_2 \mathbf{t}_1 + \mathbf{t}_2)$$
  
  To compose arbitrary sequences of translations, rotations, and scales using unified matrix multiplication, we must embed our 2D plane into **Projective Space**.
  
  ---
## 4. Projective Geometry & Homogeneous Coordinates (Slides 24–27)
  
  Slide 25 introduces the unifying convention:
  > **The Homogeneous Representation:**
  > Append an extra dummy coordinate $w = 1$ to the 2D Cartesian coordinate vector:
  > 
  > $$(x, y) \in \mathbb{R}^2 \implies \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} \in \mathbb{P}^2$$
  
  ```
                           The Projective Geometry Model (Slide 26)
                           
                                     ^ w
                                     │            Ray through origin: (x, y, w)
                                     │           /
                                     │          /
                                 1.0 ┼─────────X─── The Homogeneous Plane: w = 1
                                     │        /│    Cartesian Point: (x/w, y/w, 1)
                                     │       / │
                                     │      /  │
                        ─────────────┼─────/───┴────────► x
                                    /│    /
                                   / │   /
                                  v  └──/───────────────► y
                                     (0,0,0)
  ```
  
  ---
### A. The Projective Plane $\mathbb{P}^2$
  Mathematically, a 2D projective point is represented by an equivalence class of 3D non-zero vectors. Two homogeneous coordinate vectors represent the **exact same physical point** if they differ only by an arbitrary non-zero scalar $\lambda \neq 0$:
  
  $$\begin{bmatrix} x \\ y \\ w \end{bmatrix} \sim \begin{bmatrix} \lambda x \\ \lambda y \\ \lambda w \end{bmatrix} \quad (\lambda \neq 0)$$
  
  * **Geometric Meaning (Slide 26):** Every point in the 2D projective plane $\mathbb{P}^2$ corresponds to an entire **continuous 3D ray passing through the origin $(0, 0, 0)$** in $(x, y, w)$ space.
  * The physical image plane is the horizontal slice located at height $w = 1$. The ray intersects this plane at:
  
  $$\begin{bmatrix} x \\ y \\ w \end{bmatrix} \implies \left( \frac{x}{w}, \, \frac{y}{w}, \, 1 \right) \iff \text{Cartesian Coordinates: } \left( \frac{x}{w}, \, \frac{y}{w} \right)$$
  
  ---
### B. Translation in Homogeneous Coordinates (Slide 27)
  By expanding to $3 \times 3$ matrices, translation can be formulated as a linear shear operating on the augmented $w=1$ plane:
  
  $$\mathbf{T} = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}$$
  
  Evaluating matrix-vector multiplication (Slide 27):
  
  $$\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} = \begin{bmatrix} 1\cdot x + 0\cdot y + t_x \cdot 1 \\ 0\cdot x + 1\cdot y + t_y \cdot 1 \\ 0\cdot x + 0\cdot y + 1 \cdot 1 \end{bmatrix} = \begin{bmatrix} x + t_x \\ y + t_y \\ 1 \end{bmatrix}$$
  
  Converting back to Cartesian space:
  
  $$\left(\frac{x + t_x}{1}, \, \frac{y + t_y}{1}\right) = (x + t_x, \, y + t_y)$$
  
  Translation is now unified into standard matrix multiplication.
  
  ---
## 5. The Affine Transformation Family (Slides 28–31)
  
  Slide 28 defines the general structure of an **Affine Transformation**:
  
  $$\mathbf{T}_{\text{affine}} = \begin{bmatrix} a & b & c \\ d & e & f \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} \mathbf{A} & \mathbf{t} \\ \mathbf{0}^T & 1 \end{bmatrix}$$
  
  where $\mathbf{A} = \begin{bmatrix} a & b \\ d & e \end{bmatrix}$ is an arbitrary non-singular $2 \times 2$ matrix, and $\mathbf{t} = \begin{bmatrix} c \\ f \end{bmatrix}$ is a 2D translation vector.
  
  ```
                   The Anatomy of an Affine Transformation Matrix
                   
             ┌────────────────────────────────────────────────────────┐
             │                                                        │
             │         T = ┌── a     b   │   c ──┐                    │
             │             │   d     e   │   f   │                    │
             │             └── 0     0   │   1 ──┘                    │
             │                 └─────┬───┘   └──┬─┘                   │
             └───────────────────────┼──────────┼─────────────────────┘
                                     │          │
                  ┌──────────────────┘          └──────────────────┐
                  ▼                                                ▼
         2×2 Matrix A (4 DOF)                             Translation t (2 DOF)
     Encodes Scale, Rotation, Shear                  Encodes horizontal (c = t_x)
     det(A) ≠ 0 preserves orientation                and vertical (f = t_y) shifts
  ```
  
  ---
### A. Canonical Building Blocks of Affine Maps (Slides 29–30)
  
  ```
  ┌───────────────────────────┬───────────────────────────────────────────┬───────┬─────────────────────────┐
  │ Transformation            │ Homogeneous 3×3 Matrix Representation     │ D.O.F │ Invariant Properties    │
  ├───────────────────────────┼───────────────────────────────────────────┼───────┼─────────────────────────┤
  │ **Translation**           │ $\begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}$ │ 2 │ Orientation, Lengths, Angles, Parallelism │
  ├───────────────────────────┼───────────────────────────────────────────┼───────┼─────────────────────────┤
  │ **2D Rotation (Euclidean)**│ $\begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix}$ │ 1 │ Lengths, Areas, Angles, Parallelism │
  ├───────────────────────────┼───────────────────────────────────────────┼───────┼─────────────────────────┤
  │ **Non-Uniform Scaling**   │ $\begin{bmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{bmatrix}$ │ 2 │ Parallelism (Angles NOT preserved) │
  ├───────────────────────────┼───────────────────────────────────────────┼───────┼─────────────────────────┤
  │ **Horizontal/Vertical Shear**│ $\begin{bmatrix} 1 & sh_x & 0 \\ sh_y & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$ │ 2 │ Parallelism, Area (when $sh_x \cdot sh_y = 0$) │
  └───────────────────────────┴───────────────────────────────────────────┴───────┴─────────────────────────┘
  ```
  
  ---
### B. Degrees of Freedom (DOF) of an Affine Map
  An affine transformation has **$6$ Degrees of Freedom**:
  * $2$ for translation ($t_x, t_y$).
  * $1$ for in-plane rotation ($\theta$).
  * $2$ for independent scaling ($s_x, s_y$).
  * $1$ for shear/skew parameter ($sh$).
#### Minimal Correspondence Constraint:
  Because each matched point correspondence $(\mathbf{x}_i \leftrightarrow \mathbf{x}_i')$ provides two independent linear scalar equations ($x'_i = a x_i + b y_i + c$ and $y'_i = d x_i + e y_i + f$), an affine transformation can be solved uniquely using a minimum of **3 non-collinear point correspondences**:
  
  $$2 \text{ equations/point} \times 3 \text{ points} = 6 \text{ equations} \implies \text{Solves 6 unknowns } [a, b, c, d, e, f]^T$$
  
  ---
### C. Geometric Invariants of Affine Transformations (Slide 31)
  Slide 31 lists the geometric properties preserved by affine maps:
  
  1. **Lines Remain Lines (Collinearity Preserved):** 
   If three or more points lie on a straight line in the original image, their transformed points will remain on a straight line.
  2. **Parallel Lines Remain Parallel (Parallelism Preserved):** 
   If lines $L_1 \parallel L_2$, then $T(L_1) \parallel T(L_2)$. 
   * **Consequence:** An affine transformation can deform a rectangle into a parallelogram, but it **cannot model vanishing points or perspective convergence**.
  3. **Ratio of Parallel Lengths Preserved:** 
   If three points $A, B, C$ lie on a line, the ratio of segment lengths is invariant:
  
  $$\frac{\| \mathbf{b} - \mathbf{a} \|}{\| \mathbf{c} - \mathbf{b} \|} = \frac{\| \mathbf{b}' - \mathbf{a}' \|}{\| \mathbf{c}' - \mathbf{b}' \|}$$
  
  4. **Ratio of Areas Preserved:** 
   The ratio between the areas of any two closed geometric shapes is invariant, scaling uniformly by $|\det(\mathbf{A})|$.
  
  ---
## 6. Python Implementation: Composing Affine Transforms in Homogeneous Coordinates
  
  ```python
  import numpy as np
  
  def get_translation_matrix(tx: float, ty: float) -> np.ndarray:
    """Constructs a 3x3 homogeneous translation matrix."""
    return np.array([
        [1.0, 0.0, tx],
        [0.0, 1.0, ty],
        [0.0, 0.0, 1.0]
    ], dtype=np.float32)
  
  def get_rotation_matrix(theta_radians: float) -> np.ndarray:
    """Constructs a 3x3 homogeneous 2D rotation matrix around origin."""
    c = np.cos(theta_radians)
    s = np.sin(theta_radians)
    return np.array([
        [c,   -s,   0.0],
        [s,    c,   0.0],
        [0.0, 0.0, 1.0]
    ], dtype=np.float32)
  
  def get_scale_matrix(sx: float, sy: float) -> np.ndarray:
    """Constructs a 3x3 homogeneous scaling matrix."""
    return np.array([
        [sx,  0.0, 0.0],
        [0.0, sy,  0.0],
        [0.0, 0.0, 1.0]
    ], dtype=np.float32)
  
  # -------------------------------------------------------------
  # Demonstrating Compositional Power (Rotate about center point)
  # -------------------------------------------------------------
  # To rotate around an arbitrary center (cx, cy):
  # 1. Translate center to origin: T(-cx, -cy)
  # 2. Rotate around origin: R(theta)
  # 3. Translate origin back to center: T(+cx, +cy)
  cx, cy = 200.0, 150.0
  theta = np.deg2rad(45.0)
  
  T_to_origin = get_translation_matrix(-cx, -cy)
  R = get_rotation_matrix(theta)
  T_back = get_translation_matrix(cx, cy)
  
  # Matrix composition evaluated right-to-left: M = T_back @ R @ T_to_origin
  M_composite = T_back @ R @ T_to_origin
  
  # Test coordinate: Transform point p = (250, 150)
  p_cartesian = np.array([250.0, 150.0])
  p_homo = np.array([p_cartesian[0], p_cartesian[1], 1.0])
  
  # Transform point via matrix multiplication
  p_prime_homo = M_composite @ p_homo
  p_prime_cartesian = p_prime_homo[:2] / p_prime_homo[2]
  
  print("Original Point:   ", p_cartesian)
  print("Transformed Point:", p_prime_cartesian)
  ```
  
  ---
## Summary Matrix: Linear vs. Affine Operations
  
  | Property | Linear Transformation ($2 \times 2$) | Affine Transformation ($3 \times 3$, Homogeneous) |
  | :--- | :--- | :--- |
  | **Mathematical Form** | $\mathbf{p}' = \mathbf{A}_{2 \times 2} \mathbf{p}$ | $\mathbf{p}' = \mathbf{T}_{3 \times 3} \mathbf{p} = \begin{bmatrix} \mathbf{A} & \mathbf{t} \\ \mathbf{0}^T & 1 \end{bmatrix} \begin{bmatrix} \mathbf{p} \\ 1 \end{bmatrix}$ |
  | **Origin Mapping** | Must map $(0, 0) \to (0, 0)$ | Can map $(0, 0) \to (t_x, t_y)$ |
  | **Supported Operations**| Scale, Rotation, Shear, Reflection | Scale, Rotation, Shear, Reflection, **Translation** |
  | **Degrees of Freedom** | $4$ DOF | **$6$ DOF** |
  | **Minimal Points to Fit**| $2$ Point Correspondences | **$3$ Point Correspondences** |
  | **Parallelism Preserved?**| **Yes** | **Yes** |
  | **Perspective Vanishing?**| **No** (Cannot model out-of-plane tilt) | **No** (Cannot model out-of-plane tilt) |
  
  ---