## 1. The Out-of-Plane Rotation Problem: Why Affine Maps Fail (Slide 32)
  
  In **Part 1**, we saw that **Affine Transformations** (with 6 degrees of freedom) represent any combination of 2D translation, in-plane rotation, scaling, and shear:
  
  $$\mathbf{T}_{\text{affine}} = \begin{bmatrix} a & b & c \\ d & e & f \\ \mathbf{0} & \mathbf{0} & \mathbf{1} \end{bmatrix}$$
  
  Because the bottom row is strictly $\begin{bmatrix} 0 & 0 & 1 \end{bmatrix}$, the denominator when converting back to Cartesian coordinates is always $w' = 1$. 
  
  Slide 32 shows a hallway and an indoor atrium to illustrate why affine transformations fall short:
  > **"Aligning images typically requires an out-of-plane rotation of some kind. This is not, in general, an affine transformation."**
  
  ```
     Planar Affine Transformation                      Projective Perspective Distortion
    (Parallelism strictly preserved)                  (Lines converge to vanishing points)
    ┌───────────────────────────────┐                  ┌───────────────────────────────┐
    │          /──────────/         │                  │               /\ Vanishing Pt │
    │         /          /          │                  │              /  \             │
    │        /          /           │                  │             /    \ Receding   │
    │       /──────────/            │                  │            /      \ Walls     │
    │   Opposite edges remain       │                  │           /________\          │
    │   strictly parallel!          │                  │   Parallel lines CONVERGE!    │
    └───────────────────────────────┘                  └───────────────────────────────┘
  ```
### The Geometric Barrier:
  * Real camera lenses capture the world through **central perspective projection**.
  * When a surface tilts away from the camera (an out-of-plane rotation around the physical $X$ or $Y$ axis), elements farther away appear smaller than elements close to the lens.
  * Parallel lines (such as hallway floorboards, ceiling lights, or opposite edges of a book) **converge toward a vanishing point**.
  * Because an affine transformation preserves parallelism ($L_1 \parallel L_2 \implies T(L_1) \parallel T(L_2)$), **no affine transformation can model perspective convergence or vanishing points**.
  
  ---
## 2. Planar Homographies: Projective Transformations in $\mathbb{P}^2$ (Slides 33–36, 41)
  
  To model perspective foreshortening and out-of-plane rotations, we populate the **bottom row** of our $3 \times 3$ matrix with non-zero parameters.
  
  Slide 33 introduces the **Planar Homography** (also known as a **Projective Transformation**, **Collineation**, or **Planar Perspective Map**):
  
  $$\mathbf{H} = \begin{bmatrix} a & b & c \\ d & e & f \\ \mathbf{g} & \mathbf{h} & \mathbf{1} \end{bmatrix} = \begin{bmatrix} h_{00} & h_{01} & h_{02} \\ h_{10} & h_{11} & h_{12} \\ h_{20} & h_{21} & h_{22} \end{bmatrix} \in \mathbb{R}^{3 \times 3}$$
  
  ```
                   The Anatomical Structure of a Homography Matrix
                   
         ┌───────────────────────────────────────────────────────────────┐
         │                                                               │
         │       H = ┌── a     b   │   c ──┐   ◄── Affine / Linear Part  │
         │           │   d     e   │   f   │                             │
         │           └── g     h   │   1 ──┘   ◄── Projective Part       │
         │               └─────┬───┘   └──┬┘                             │
         └─────────────────────┼──────────┼──────────────────────────────┘
                               │          │
                 ┌─────────────┘          └──────────────┐
                 ▼                                       ▼
       [ g,  h ] = [ h₂₀,  h₂₁ ]                      h₂₂ = 1
     Perspective Tilt Parameters              Overall Global Scale Factor
     Controls out-of-plane rotation           (Normalizes projective ambiguity)
     and vanishing point locations!
  ```
  
  ---
### A. The Non-Linear Inhomogeneous Coordinate Mapping (Slides 35–36)
  Applying a homography $\mathbf{H}$ to a 2D point expressed in homogeneous coordinates:
  
  $$\begin{bmatrix} x' \\ y' \\ w' \end{bmatrix} = \begin{bmatrix} a & b & c \\ d & e & f \\ g & h & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix} = \begin{bmatrix} ax + by + c \\ dx + ey + f \\ gx + hy + 1 \end{bmatrix}$$
  
  Notice that the third homogeneous coordinate $w'$ is no longer $1$:
  
  $$w' = gx + hy + 1$$
  
  Converting from homogeneous space back to 2D Cartesian coordinates requires dividing by $w'$ (**Perspective Division**):
  
  $$(x', y') = \left( \frac{x'}{w'}, \, \frac{y'}{w'} \right) = \left( \frac{ax + by + c}{gx + hy + 1}, \, \frac{dx + ey + f}{gx + hy + 1} \right)$$
  
  > **Core Observation (Slide 36):** 
  > Although homography mapping is a **linear matrix multiplication in 3D projective space $\mathbb{P}^2$**, it acts as a **rational non-linear transformation in 2D Euclidean space $\mathbb{R}^2$**. 
  > The spatial position $(x, y)$ appears in the denominator, which allows coordinate scaling to vary continuously across the image.
  
  ---
### B. Degrees of Freedom (DOF) and Scale Invariance (Slide 40–41)
  Although $\mathbf{H}$ contains 9 matrix entries ($3 \times 3$), multiplying the entire matrix by an arbitrary non-zero scalar $\lambda \neq 0$ does not change the physical Cartesian coordinates:
  
  $$\mathbf{p}' \sim (\lambda \mathbf{H}) \mathbf{p} \implies \left( \frac{\lambda (ax + by + c)}{\lambda (gx + hy + 1)}, \, \frac{\lambda (dx + ey + f)}{\lambda (gx + hy + 1)} \right) = \left( \frac{ax + by + c}{gx + hy + 1}, \, \frac{dx + ey + f}{gx + hy + 1} \right)$$
  
  * A homography is defined only **up to an arbitrary global scale**.
  * Subtracting 1 degree of freedom for this scale ambiguity:
  
  $$\text{Degrees of Freedom (DOF)} = 9 - 1 = \mathbf{8 \text{ DOF}}$$
#### Normalization Conventions:
  To remove the scale ambiguity, literature adopts one of three standard constraints:
  1. **Anchor Bottom-Right Entry to 1 (Slides 33, 36):** $h_{22} = 1$ (valid whenever the transformation does not map the origin to infinity).
  2. **Unit Frobenius Norm Constraint:** $\|\mathbf{H}\|_F^2 = \sum_{i=0}^2 \sum_{j=0}^2 h_{ij}^2 = 1$.
  3. **Entries Sum to One (Slide 41):** $\sum_{i,j} h_{ij} = 1$.
  
  ---
## 3. Vanishing Points & The Projective Horizon (Slides 36–37)
  
  Slide 36 demonstrates the perspective warping of a bumblebee photograph:
  > **"Homographies allow us to have vanishing points (at infinity)."**
  
  ```
       Original Image (Rectangular)                    Homography Warp (Trapezoidal)
     ┌───────────────────────────┐                         • Vanishing Point (Finite Intersection)
     │                           │                        / \
     │         /───────\         │                       /   \   Receding Edges
     │        │ Bumble- │        │                      /     \  Converge!
     │        │   bee   │        │     ────────►       /───────\
     │         \_______/         │                    / │Bumble-│\
     │                           │                   /  │  bee  │ \
     └───────────────────────────┘                  /____\_____/____\
       Parallel Vertical Edges                      Lines converge upward!
  ```
  
  ---
### A. The Geometry of the Vanishing Line
  In classical Euclidean geometry, parallel lines never intersect. In projective geometry, parallel lines intersect at a point on the **Line at Infinity** $\mathbf{l}_\infty = \begin{bmatrix} 0 & 0 & 1 \end{bmatrix}^T$.
  
  Under an affine transformation $\mathbf{T}_A$, the line at infinity maps back to itself:
  
  $$\mathbf{l}'_\infty = \mathbf{T}_A^{-T} \mathbf{l}_\infty \propto \mathbf{l}_\infty$$
  
  Under a homography $\mathbf{H}$, however, the line at infinity is mapped to a **finite line in the image plane**:
  
  $$\mathbf{l}_{\text{vanishing}} = \mathbf{H}^{-T} \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix} = \begin{bmatrix} g \\ h \\ 1 \end{bmatrix}$$
  
  * Any point $(x, y)$ that satisfies the denominator equation:
  
  $$gx + hy + 1 = 0$$
  
  maps to $w' = 0$, sending its Cartesian coordinate to infinity. This line represents the **horizon (vanishing line)** of the receding plane.
  
  ---
### B. Planar Rectification (Slide 37)
  Slide 37 illustrates the inverse operation: using a homography to **rectify** a scene.
  
  ```
       Perspective Hallway Image                       Top-Down Rectified View (H₁)
     ┌───────────────────────────┐                   ┌───────────────────────────┐
     │          /│   │\          │                   │ │                       │ │
     │         / │   │ \         │                   │ │                       │ │
     │        /  │ • │  \ Vanish │   ──► H₁ ──►      │ │       Hallway         │ │
     │       /   │   │   \ Point │                   │ │       Floor           │ │
     │      /____│___│____\      │                   │ │ (Parallel Floor Tiles)│ │
     └───────────────────────────┘                   └───────────────────────────┘
  ```
  
  * **The Rectification Process:**
  If an image observes a tilted plane (such as a tiled hallway floor receding into depth), parallel floor lines converge toward a vanishing point.
  By computing a homography $\mathbf{H}_1$ that maps the vanishing line back to infinity:
  
  $$\mathbf{H}_1 \begin{bmatrix} x_{\text{vanish}} \\ y_{\text{vanish}} \\ 1 \end{bmatrix} \sim \begin{bmatrix} x' \\ y' \\ 0 \end{bmatrix}$$
  
  the floor is transformed into an **orthographic bird’s-eye view** where opposite wall lines run parallel, and rectangular tiles regain $90^\circ$ angles.
  * **Left-View Wall Rectification ($\mathbf{H}_2$):** Similarly, an alternative homography $\mathbf{H}_2$ rectifies the left wall into a flat frontal facade.
  
  ---
## 4. When Does a Homography Hold in the Real World?
  
  A common question in computer vision is: *When is a 2D homography physically valid for relating two different photographic images?*
  
  A 2D homography is an exact geometric model in two specific physical configurations:
  
  ```
                    When Does a 2D Homography Exactly Hold?
                                       │
         ┌─────────────────────────────┴─────────────────────────────┐
         ▼                                                           ▼
  [ CONDITION 1: PLANAR SCENE ]                       [ CONDITION 2: PURE ROTATION ]
  The physical 3D scene is FLAT                       The camera undergoes PURE ROTATION
  (All points lie on a plane π)                       around its optical center (t = 0)
  
         Camera 1        Camera 2                            Camera Frame 1
            \                /                                     \
             \              /                                       \ Rotate by R
              ▼            ▼                                         ▼
         ┌──────────────────────┐                             Camera Frame 2
         │ Physical 3D Plane π  │                             (Both share EXACT SAME
         │ (Wall, Floor, Book)  │                              optical center C!)
         └──────────────────────┘                             Scene can have ARBITRARY 3D depth!
  ```
  
  ---
### Condition 1: The Planar Scene Configuration (Slides 3–7)
  If all observed physical 3D points lie on a common plane $\boldsymbol{\pi}$ in the world (e.g., the book cover on the desk in Slides 3–7, a whiteboard, or an aerial view of flat terrain):
  * The mapping between the 3D plane and Camera 1 is a homography $\mathbf{H}_1$.
  * The mapping between the 3D plane and Camera 2 is a homography $\mathbf{H}_2$.
  * The composite mapping between Image 1 and Image 2 is:
  
  $$\mathbf{H}_{1 \to 2} = \mathbf{H}_2 \mathbf{H}_1^{-1}$$
  
  This relationship holds **regardless of camera translation or baseline distance**.
  
  ---
### Condition 2: Pure Camera Rotation / Panoramic Stitching (Slides 8–10, 38)
  If the camera rotates around its optical center without any spatial translation ($\mathbf{t} = \mathbf{0}$):
  
  $$\mathbf{x}_2 \sim \mathbf{K}_2 \, \mathbf{R} \, \mathbf{K}_1^{-1} \, \mathbf{x}_1$$
  
  where $\mathbf{K}_1, \mathbf{K}_2$ are the camera intrinsic calibration matrices and $\mathbf{R}$ is the 3D rotation matrix.
  * Defining $\mathbf{H} = \mathbf{K}_2 \mathbf{R} \mathbf{K}_1^{-1}$, the transformation between views is an exact homography.
  * **Why this matters for Panoramas (Slides 8–10, 38):** 
  Because there is zero translation between camera views ($\mathbf{t} = \mathbf{0}$), there is **zero motion parallax**. Distant background buildings and nearby foreground railings shift identically, allowing two images to be stitched into a seamless panorama using a single global homography.
  
  > **When Homographies Break Down:** 
  > If a camera translates ($\mathbf{t} \neq \mathbf{0}$) in a general 3D scene containing objects at varying depths, nearby objects shift faster than distant objects (**motion parallax**). 
  > A single 2D homography cannot align the entire scene simultaneously; alignment requires **dense depth estimation and Epipolar Geometry** (Unit 2).
  
  ---
## 5. The 2D Geometric Transformation Hierarchy (Slides 39–40)
  
  Slide 40 presents the master classification hierarchy of 2D planar transformations, ordered from most constrained to most general:
  
  $$\text{Translation} \subset \text{Rigid (Euclidean)} \subset \text{Similarity} \subset \text{Affine} \subset \text{Projective (Homography)}$$
  
  ```
                   The 2D Transformation Invariance Hierarchy
                   
   ┌────────────────────────────────────────────────────────────────────────┐
   │ PROJECTIVE (Homography, 8 DOF)                                         │
   │ Preserves: Collinearity, Cross-Ratios, Conic Intersections             │
   │ ┌────────────────────────────────────────────────────────────────────┐ │
   │ │ AFFINE (6 DOF)                                                     │ │
   │ │ Preserves: Parallelism, Area Ratios, Center of Mass                │ │
   │ │ ┌────────────────────────────────────────────────────────────────┐ │ │
   │ │ │ SIMILARITY (4 DOF)                                             │ │ │
   │ │ │ Preserves: Angles, Shape Ratios (Conformal Map)                │ │ │
   │ │ │ ┌────────────────────────────────────────────────────────────┐ │ │ │
   │ │ │ │ RIGID / EUCLIDEAN (3 DOF)                                  │ │ │ │
   │ │ │ │ Preserves: Absolute Lengths, Absolute Areas                │ │ │ │
   │ │ │ │ ┌────────────────────────────────────────────────────────┐ │ │ │ │
   │ │ │ │ │ TRANSLATION (2 DOF)                                    │ │ │ │ │
   │ │ │ │ │ Preserves: Absolute Vector Orientation                 │ │ │ │ │
   │ │ │ │ └────────────────────────────────────────────────────────┘ │ │ │ │
   │ │ │ └────────────────────────────────────────────────────────────┘ │ │ │
   │ │ └────────────────────────────────────────────────────────────────┘ │ │
   │ └────────────────────────────────────────────────────────────────────┘ │
   └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Comprehensive Master Hierarchy Table (Slide 40)
  
  | Transformation | Matrix Structure (Homogeneous) | D.O.F | Min. Points | Invariant Properties Preserved | Icon Deformation Shape |
  | :--- | :--- | :--- | :--- | :--- | :--- |
  | **Translation** | $\begin{bmatrix} \mathbf{I}_{2\times 2} & \mathbf{t} \\ \mathbf{0}^T & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}$ | **2** | 1 Point | Absolute Orientation, Lengths, Angles, Areas, Parallelism | Rigid Square, shifted in $(x, y)$ |
  | **Rigid (Euclidean)** | $\begin{bmatrix} \mathbf{R}_{2\times 2} & \mathbf{t} \\ \mathbf{0}^T & 1 \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & t_x \\ \sin\theta & \cos\theta & t_y \\ 0 & 0 & 1 \end{bmatrix}$ | **3** | 2 Points (1.5 pts) | Absolute Lengths, Angles, Areas, Parallelism | Rigid Square, rotated and translated |
  | **Similarity** | $\begin{bmatrix} s\mathbf{R}_{2\times 2} & \mathbf{t} \\ \mathbf{0}^T & 1 \end{bmatrix} = \begin{bmatrix} s\cos\theta & -s\sin\theta & t_x \\ s\sin\theta & s\cos\theta & t_y \\ 0 & 0 & 1 \end{bmatrix}$ | **4** | 2 Points | Angles, Shape Proportions, Area Ratios, Parallelism | Square scaled, rotated, and shifted |
  | **Affine** | $\begin{bmatrix} \mathbf{A}_{2\times 2} & \mathbf{t} \\ \mathbf{0}^T & 1 \end{bmatrix} = \begin{bmatrix} a & b & t_x \\ c & d & t_y \\ 0 & 0 & 1 \end{bmatrix}$ | **6** | 3 Points | Parallelism, Midpoints, Area Ratios, Collinearity | Square deformed into a **Parallelogram** |
  | **Projective (Homography)** | $\begin{bmatrix} h_{00} & h_{01} & h_{02} \\ h_{10} & h_{11} & h_{12} \\ h_{20} & h_{21} & h_{22} \end{bmatrix}$ | **8** | **4 Points** | **Collinearity, Cross-Ratios** | Square deformed into an arbitrary **Quadrilateral** |
  
  ---
### The Fundamental Projective Invariant: The Cross-Ratio
  While projective homographies break absolute lengths, angles, and parallelism, they preserve a fundamental geometric quantity: the **Cross-Ratio of Four Collinear Points**.
  
  ```
              A             B             C             D
       ───────•─────────────•─────────────•─────────────•───────► Line L
              │◄─── AC ────►│
                            │◄──── BD ───►│
              │◄─────── AD ──────────────►│
                            │◄─── BC ────►│
  ```
  
  For four collinear points $A, B, C, D$:
  
  $$\text{Cross-Ratio}(A, B, C, D) = \frac{(x_A - x_C)(x_B - x_D)}{(x_B - x_C)(x_A - x_D)}$$
  
  Under any arbitrary 2D or 3D projective transformation $\mathbf{H}$:
  
  $$\text{Cross-Ratio}\big(T(A), T(B), T(C), T(D)\big) \equiv \text{Cross-Ratio}(A, B, C, D)$$
  
  This projective invariant allows computer vision systems to verify matches and detect coplanar geometric patterns under severe perspective tilt.
  
  ---
## 6. Mathematical Formulation for Fitting a Homography (Slide 41)
  
  Slide 41 previews how homographies are estimated from matched features:
  
  $$\begin{bmatrix} x_i' \\ y_i' \\ 1 \end{bmatrix} \sim \begin{bmatrix} h_{00} & h_{01} & h_{02} \\ h_{10} & h_{11} & h_{12} \\ h_{20} & h_{21} & h_{22} \end{bmatrix} \begin{bmatrix} x_i \\ y_i \\ 1 \end{bmatrix}$$
  
  ---
### A. How Many Points are Required?
  * A homography has **8 degrees of freedom**.
  * Each 2D point correspondence $(\mathbf{x}_i \leftrightarrow \mathbf{x}_i')$ supplies two independent equations (one for $x'$, one for $y'$).
  * Solving for the 8 unknowns requires a minimum of:
  
  $$\frac{8 \text{ unknowns}}{2 \text{ equations / point}} = \mathbf{4 \text{ Point Correspondences}}$$
  
  > **Rule:** To compute a homography, you must identify **four pairs of matching points**, with the constraint that **no three points are collinear** (Slide 41).
  
  ---
### B. Algebraic Derivation of Direct Linear Transform (DLT) Equations
  From the inhomogeneous mapping equations:
  
  $$x_i' = \frac{h_{00}x_i + h_{01}y_i + h_{02}}{h_{20}x_i + h_{21}y_i + h_{22}}, \quad y_i' = \frac{h_{10}x_i + h_{11}y_i + h_{12}}{h_{20}x_i + h_{21}y_i + h_{22}}$$
  
  Multiply through by the denominator $(h_{20}x_i + h_{21}y_i + h_{22})$:
  
  $$x_i'(h_{20}x_i + h_{21}y_i + h_{22}) = h_{00}x_i + h_{01}y_i + h_{02}$$
  
  $$y_i'(h_{20}x_i + h_{21}y_i + h_{22}) = h_{10}x_i + h_{11}y_i + h_{12}$$
  
  Rearranging into homogeneous linear form $\mathbf{a}_i^T \mathbf{h} = 0$:
  
  $$\begin{bmatrix} 
  -x_i & -y_i & -1 & 0 & 0 & 0 & x_i' x_i & x_i' y_i & x_i' \\[6pt]
  0 & 0 & 0 & -x_i & -y_i & -1 & y_i' x_i & y_i' y_i & y_i' 
  \end{bmatrix} \begin{bmatrix} h_{00} \\ h_{01} \\ h_{02} \\ h_{10} \\ h_{11} \\ h_{12} \\ h_{20} \\ h_{21} \\ h_{22} \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$$
  
  Stacking equations across $N \ge 4$ point correspondences yields the linear system:
  
  $$\mathbf{A}_{2N \times 9} \, \mathbf{h}_{9 \times 1} = \mathbf{0}$$
  
  This homogeneous system is solved via **Singular Value Decomposition (SVD)**, where $\mathbf{h}$ is the singular vector corresponding to the smallest singular value of $\mathbf{A}$.
  
  ---
## 7. Python Implementation: Projective Homography Coordinate Transformation
  
  ```python
  import numpy as np
  
  def apply_homography(H: np.ndarray, points: np.ndarray) -> np.ndarray:
    """
    Applies a 3x3 homography matrix to an array of 2D points.
    
    Parameters:
        H: (3, 3) projective transformation matrix.
        points: (N, 2) array of Cartesian coordinates [x, y].
        
    Returns:
        (N, 2) array of transformed Cartesian coordinates [x', y'].
    """
    N = points.shape[0]
    
    # Step 1: Convert Cartesian coordinates to homogeneous coordinates (N, 3)
    # Augment with column of ones: [x, y, 1]
    homogeneous_points = np.hstack([points, np.ones((N, 1), dtype=points.dtype)])
    
    # Step 2: Apply linear transformation via matrix dot product
    # (3, 3) @ (3, N) -> (3, N) -> transpose back to (N, 3)
    transformed_homo = (H @ homogeneous_points.T).T
    
    # Step 3: Perform Perspective Division by w' = transformed_homo[:, 2]
    w_prime = transformed_homo[:, 2:3]
    
    # Numerical safeguard against points mapped to infinity (vanishing points)
    w_prime = np.where(np.abs(w_prime) < 1e-12, 1e-12, w_prime)
    
    # Convert back to Cartesian space: [x'/w', y'/w']
    cartesian_transformed = transformed_homo[:, :2] / w_prime
    
    return cartesian_transformed
  
  # -------------------------------------------------------------
  # Verification: Rectifying a Trapezoid into a Square
  # -------------------------------------------------------------
  # Example Homography: Pure perspective tilt
  H_example = np.array([
    [1.2,  0.1,  -20.0],
    [0.0,  1.1,  -15.0],
    [0.001, 0.0005, 1.0]   # Non-zero bottom row creates perspective!
  ])
  
  test_corners = np.array([
    [0.0, 0.0],
    [100.0, 0.0],
    [100.0, 100.0],
    [0.0, 100.0]
  ])
  
  warped_corners = apply_homography(H_example, test_corners)
  print("Original Square Corners:\n", test_corners)
  print("\nProjected Quadrilateral Corners:\n", np.round(warped_corners, 2))
  ```
  
  ---
## Summary Matrix: The Complete 2D Transformation Landscape
  
  | Transformation Group | Unknown Parameters | Preserves Parallelism? | Preserves Angles? | Can Model Vanishing Points? |
  | :--- | :--- | :--- | :--- | :--- |
  | **Translation** | 2 ($t_x, t_y$) | **Yes** | **Yes** | No |
  | **Euclidean (Rigid)** | 3 ($\theta, t_x, t_y$) | **Yes** | **Yes** | No |
  | **Similarity** | 4 ($s, \theta, t_x, t_y$) | **Yes** | **Yes** | No |
  | **Affine** | 6 ($a, b, c, d, e, f$) | **Yes** | No | No |
  | **Projective (Homography)** | **8** ($h_{00} \dots h_{22}$) | **No (Converges)** | **No** | **Yes (Line at Infinity $\to$ Finite Horizon)** |
  
  ---