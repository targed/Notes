## 1. The Curve Fitting Problem: From 2D Points to Linear Regression (Slides 2–5)
  
  Slide 2 introduces the fundamental problem of **Least Squares Solving**. 
  
  In physical measurement, computer vision, and experimental data acquisition, we rarely observe pristine mathematical functions. Sensor noise, optical distortions, and quantization errors corrupt measurements.
  
  ```
     Exact Determinant System (2 Points)                Overdetermined Noisy System (N Points)
  ┌──────────────────────────────────┐               ┌──────────────────────────────────┐
  │                                  │               │                                  │
  │                      • (x₂, y₂)  │               │                      •           │
  │                     /            │               │                   • /            │
  │                    /             │               │            •       /  •          │
  │                   /              │               │             \  •  /              │
  │        • (x₁, y₁)                │               │       •      \   /               │
  │                                  │               │               • •                │
  └──────────────────────────────────┘               └──────────────────────────────────┘
     Unique Line Exists:                              NO single straight line can pass
     2 Equations, 2 Unknowns (m, b)                   through all N > 2 points simultaneously!
  ```
  
  ---
### A. The Setup (Slide 5)
  Suppose we collect a set of $N$ paired spatial observations:
  
  $$\mathcal{D} = \big\{ \mathbf{p}_i = (x_i, \, y_i) \big\}_{i=1}^N$$
  
  We want to fit a parametric linear model (a straight line) that describes the dependent variable $y$ as an affine function of the independent variable $x$:
  
  $$f(x_i) = \beta_0 + \beta_1 x_i$$
  
  * $\beta_0$: The vertical intercept ($y$-axis baseline).
  * $\beta_1$: The slope of the line.
  
  ---
### B. Defining the Residual Error (Slides 4–5)
  Because real-world data points do not fall on an exact line, each observation has a vertical discrepancy called a **residual** $r_i$:
  
  $$r_i = y_i - f(x_i) = y_i - (\beta_0 + \beta_1 x_i)$$
  
  ```
                           The Residual Vector r_i (Slide 4, 5)
                                        ^ y
                                        │                      • (x_i, y_i) Observed Data
                                        │                      │
                                        │                      │  Residual:
                                        │                      │  r_i = y_i - f(x_i)
                                        │                      ▼
                                        │                  ────X──── Model: f(x) = β₀ + β₁x
                                        │                     /
                                        │                    /
                                        └───────────────────┴──────────────► x
                                                           x_i
  ```
  
  * The residual $r_i$ measures the **vertical geometric distance** between the true observed measurement $y_i$ and the model's prediction $f(x_i)$.
  
  ---
## 2. The Objective Function: Sum of Squared Errors (Slides 6–7)
  
  Slide 6 poses the optimization goal:
  > **How do we find the parameters $\boldsymbol{\beta} = \begin{bmatrix} \beta_0 & \beta_1 \end{bmatrix}^T$ that achieve the "best" possible fit across all $N$ noisy measurements?**
  
  ```
                       Choosing an Error Objective Metric
                                        │
           ┌────────────────────────────┴────────────────────────────┐
           ▼                                                         ▼
  Sum of Residuals: Σ r_i                                   Sum of Squared Errors: Σ r_i²
  • Disastrous! Positive and negative                       • Penalizes all errors strictly positive
    errors cancel each other out:                           • Strongly penalizes large outliers
    (+10) + (-10) = 0!                                      • Smooth, continuously differentiable (C∞)
  ```
  
  To prevent errors of opposite signs from canceling out, we minimize the **Sum of Squared Errors (SSE)**, or equivalently the **Mean Squared Error (MSE)**:
  
  $$J(\beta_0, \beta_1) = \sum_{i=1}^N r_i^2 = \sum_{i=1}^N \big( y_i - (\beta_0 + \beta_1 x_i) \big)^2$$
  
  $$\text{MSE} = \frac{1}{N} \sum_{i=1}^N r_i^2$$
  
  ---
## 3. Matrix Vectorization: The Standard Linear Form (Slides 8–12)
  
  Solving summations by taking partial derivatives with respect to each parameter scalar by scalar becomes tedious when models scale to dozens of variables (e.g., in homographies or bundle adjustment). 
  
  Slides 8–12 convert the scalar summation into a compact matrix-vector equation:
  
  $$\mathbf{y} \approx \mathbf{X} \boldsymbol{\beta}$$
  
  ---
### A. Constructing the Vectors and Matrices (Slides 9–11)
#### 1. The Observation Vector $\mathbf{y} \in \mathbb{R}^{N \times 1}$ (Slide 9):
  The stacked column vector containing all $N$ measured outputs:
  
  $$\mathbf{y} = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_n \end{bmatrix}$$
#### 2. The Parameter Vector $\boldsymbol{\beta} \in \mathbb{R}^{P \times 1}$ (Slide 9):
  The vector of unknown model parameters to solve for ($P = 2$ for a 2D line):
  
  $$\boldsymbol{\beta} = \begin{bmatrix} \beta_0 \\ \beta_1 \end{bmatrix}$$
#### 3. The Design Matrix (Data Matrix) $\mathbf{X} \in \mathbb{R}^{N \times P}$ (Slide 10):
  A rectangular matrix containing the input coordinates, augmented with a leading column of ones:
  
  $$\mathbf{X} = \begin{bmatrix} 1 & x_1 \\ 1 & x_2 \\ \vdots & \vdots \\ 1 & x_n \end{bmatrix}$$
  
  > **Why the First Column of $\mathbf{X}$ is All Ones (Slide 13):**
  > In matrix multiplication, the first column weights $\beta_0$, and the second column weights $\beta_1$:
  > 
  > $$\mathbf{X} \boldsymbol{\beta} = \begin{bmatrix} 1 & x_1 \\ 1 & x_2 \\ \vdots & \vdots \\ 1 & x_n \end{bmatrix} \begin{bmatrix} \beta_0 \\ \beta_1 \end{bmatrix} = \begin{bmatrix} \beta_0 \cdot 1 + \beta_1 \cdot x_1 \\ \beta_0 \cdot 1 + \beta_1 \cdot x_2 \\ \vdots \\ \beta_0 \cdot 1 + \beta_1 \cdot x_n \end{bmatrix} = \begin{bmatrix} f(x_1) \\ f(x_2) \\ \vdots \\ f(x_n) \end{bmatrix}$$
  > 
  > The column of ones acts as an **intercept carrier**, turning the affine offset $\beta_0$ into a linear matrix operation (analogous to the homogeneous coordinates we introduced in Module M3.2).
  
  ---
### B. The Residual Vector $\mathbf{e}(\boldsymbol{\beta})$ (Slide 12)
  The difference between the true observation vector $\mathbf{y}$ and the predicted output vector $\mathbf{X}\boldsymbol{\beta}$ is the **residual error vector** $\mathbf{e} \in \mathbb{R}^{N \times 1}$:
  
  $$\mathbf{e}(\boldsymbol{\beta}) = \mathbf{y} - \mathbf{X}\boldsymbol{\beta} = \begin{bmatrix} y_1 - (\beta_0 + \beta_1 x_1) \\ y_2 - (\beta_0 + \beta_1 x_2) \\ \vdots \\ y_n - (\beta_0 + \beta_1 x_n) \end{bmatrix}$$
  
  ---
## 4. Analytical Derivation of the Normal Equations (Slides 13–14)
  
  Slide 14 derives the closed-form least squares solution using matrix calculus.
  
  ---
### A. Matrix Formulation of the Cost Function
  The Sum of Squared Errors is the squared Euclidean $L_2$ norm of the residual vector $\mathbf{e}$:
  
  $$J(\boldsymbol{\beta}) = \| \mathbf{e}(\boldsymbol{\beta}) \|_2^2 = \| \mathbf{y} - \mathbf{X}\boldsymbol{\beta} \|_2^2$$
  
  Using the vector inner product identity $\|\mathbf{v}\|_2^2 = \mathbf{v}^T \mathbf{v}$:
  
  $$J(\boldsymbol{\beta}) = (\mathbf{y} - \mathbf{X}\boldsymbol{\beta})^T (\mathbf{y} - \mathbf{X}\boldsymbol{\beta})$$
  
  Expanding the transpose product:
  
  $$J(\boldsymbol{\beta}) = (\mathbf{y}^T - \boldsymbol{\beta}^T \mathbf{X}^T)(\mathbf{y} - \mathbf{X}\boldsymbol{\beta})$$
  
  $$J(\boldsymbol{\beta}) = \mathbf{y}^T \mathbf{y} - \mathbf{y}^T \mathbf{X}\boldsymbol{\beta} - \boldsymbol{\beta}^T \mathbf{X}^T \mathbf{y} + \boldsymbol{\beta}^T \mathbf{X}^T \mathbf{X} \boldsymbol{\beta}$$
  
  Because the scalar transpose of a real number equals itself:
  
  $$(\mathbf{y}^T \mathbf{X}\boldsymbol{\beta})^T = \boldsymbol{\beta}^T \mathbf{X}^T \mathbf{y}$$
  
  The middle two terms combine:
  
  $$J(\boldsymbol{\beta}) = \mathbf{y}^T \mathbf{y} - 2 \boldsymbol{\beta}^T \mathbf{X}^T \mathbf{y} + \boldsymbol{\beta}^T (\mathbf{X}^T \mathbf{X}) \boldsymbol{\beta}$$
  
  ---
### B. Matrix Differentiation: Finding the Global Minimum (Slide 14)
  To find the parameter vector $\hat{\boldsymbol{\beta}}$ that minimizes cost $J(\boldsymbol{\beta})$, take the gradient with respect to $\boldsymbol{\beta}$ and equate it to the zero vector $\mathbf{0}$:
  
  $$\nabla_{\boldsymbol{\beta}} J(\boldsymbol{\beta}) = \frac{\partial J}{\partial \boldsymbol{\beta}} = \mathbf{0}$$
#### Applying Standard Vector Calculus Identities:
  1. $\frac{\partial}{\partial \boldsymbol{\beta}} (\mathbf{y}^T \mathbf{y}) = \mathbf{0}$ (Constant with respect to $\boldsymbol{\beta}$).
  2. $\frac{\partial}{\partial \boldsymbol{\beta}} (\boldsymbol{\beta}^T \mathbf{a}) = \mathbf{a} \implies \frac{\partial}{\partial \boldsymbol{\beta}} (2 \boldsymbol{\beta}^T \mathbf{X}^T \mathbf{y}) = 2 \mathbf{X}^T \mathbf{y}$.
  3. $\frac{\partial}{\partial \boldsymbol{\beta}} (\boldsymbol{\beta}^T \mathbf{A} \boldsymbol{\beta}) = 2 \mathbf{A} \boldsymbol{\beta}$ (when $\mathbf{A} = \mathbf{X}^T \mathbf{X}$ is symmetric).
  
  Differentiating:
  
  $$\frac{\partial J}{\partial \boldsymbol{\beta}} = \mathbf{0} - 2 \mathbf{X}^T \mathbf{y} + 2 (\mathbf{X}^T \mathbf{X}) \boldsymbol{\beta} = \mathbf{0}$$
  
  Dividing by $2$ and rearranging:
  
  $$\mathbf{X}^T \mathbf{X} \boldsymbol{\beta} = \mathbf{X}^T \mathbf{y}$$
  
  > **The Normal Equations (Slide 14):**
  > The system of linear equations:
  > 
  > $$\mathbf{X}^T \mathbf{X} \boldsymbol{\beta} = \mathbf{X}^T \mathbf{y}$$
  > 
  > is known as the **Normal Equations**. It transforms an overdetermined, rectangular system ($N \times P$) into a compact, square, invertible system ($P \times P$).
  
  ---
### C. The Closed-Form Least Squares Solution (Slides 13–14)
  Assuming the columns of $\mathbf{X}$ are linearly independent, the symmetric matrix $\mathbf{X}^T \mathbf{X} \in \mathbb{R}^{P \times P}$ is full rank and invertible. 
  
  Multiplying both sides by $(\mathbf{X}^T \mathbf{X})^{-1}$:
  
  $$\hat{\boldsymbol{\beta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$$
#### Verifying the Minimum via the Hessian:
  Taking the second derivative (Hessian matrix) with respect to $\boldsymbol{\beta}$:
  
  $$\nabla_{\boldsymbol{\beta}}^2 J(\boldsymbol{\beta}) = \frac{\partial^2 J}{\partial \boldsymbol{\beta} \partial \boldsymbol{\beta}^T} = 2 \mathbf{X}^T \mathbf{X}$$
  
  Because $\mathbf{X}$ has full column rank, for any non-zero vector $\mathbf{v} \neq \mathbf{0}$:
  
  $$\mathbf{v}^T (2 \mathbf{X}^T \mathbf{X}) \mathbf{v} = 2 (\mathbf{X}\mathbf{v})^T (\mathbf{X}\mathbf{v}) = 2 \| \mathbf{X}\mathbf{v} \|_2^2 > 0$$
  
  The Hessian is **strictly positive definite** ($\nabla^2 J \succ 0$), confirming that the cost surface $J(\boldsymbol{\beta})$ is a convex quadratic paraboloid with a unique global minimum at $\hat{\boldsymbol{\beta}}$.
  
  ---
## 5. The Geometry of Least Squares: Orthogonal Projection
  
  Why are these called the *Normal* Equations? 
  The answer lies in the **subspace geometry of linear projections**.
  
  ```
                       The Geometry of Orthogonal Projection
                       
                                     ^ y (Observed Data Vector ∈ R^N)
                                     │ \
                                     │  \ Residual Error Vector:
                                     │   \ e = y - Xβ̂  (Orthogonal!)
                                     │    \
                       ──────────────┼─────▼───────────────────────
                                    /│     p = Xβ̂ (Projection of y)
                                   / │    /
                                  /  └───/─────────────────────────
                                 /      /   Subspace: Column Space col(X)
                                v      /    (Hyperplane spanned by columns of X)
                               (0,0)  /
  ```
  
  1. **The Column Space ($\text{col}(\mathbf{X})$):**
   The product $\mathbf{X}\boldsymbol{\beta}$ is a linear combination of the columns of matrix $\mathbf{X}$. The set of all possible model predictions forms a $P$-dimensional subspace (a plane) embedded within the larger $N$-dimensional space $\mathbb{R}^N$.
  2. **The Insoluble Vector:**
   Because the measurements contain noise, the observed target vector $\mathbf{y} \in \mathbb{R}^N$ almost never lies inside $\text{col}(\mathbf{X})$. No parameter choice can satisfy $\mathbf{X}\boldsymbol{\beta} = \mathbf{y}$ exactly.
  3. **The Shortest Distance Principle:**
   The point in the subspace $\text{col}(\mathbf{X})$ closest to $\mathbf{y}$ is its **orthogonal projection** $\mathbf{p} = \mathbf{X}\hat{\boldsymbol{\beta}}$.
  4. **The Orthogonality Condition (Normalcy):**
   The residual error vector $\mathbf{e} = \mathbf{y} - \mathbf{X}\hat{\boldsymbol{\beta}}$ must be **perpendicular (normal)** to every basis vector spanning the column space:
  
  $$\mathbf{X} \perp \mathbf{e} \iff \mathbf{X}^T \mathbf{e} = \mathbf{0}$$
  
  $$\mathbf{X}^T (\mathbf{y} - \mathbf{X}\hat{\boldsymbol{\beta}}) = \mathbf{0} \implies \mathbf{X}^T \mathbf{X} \hat{\boldsymbol{\beta}} = \mathbf{X}^T \mathbf{y}$$
  
  The algebraic minimum of the squared residuals coincides with the geometric orthogonal projection of $\mathbf{y}$ onto $\text{col}(\mathbf{X})$.
  
  ---
## 6. The Moore-Penrose Pseudo-Inverse (Slides 15–16)
  
  Slide 16 introduces an essential tool of numerical linear algebra:
  > **The Pseudo-Inverse:**
  > For an overdetermined linear system $\mathbf{X}\boldsymbol{\beta} \approx \mathbf{y}$ where matrix $\mathbf{X}$ is rectangular ($N \times P$ with $N > P$), $\mathbf{X}$ has no standard inverse $\mathbf{X}^{-1}$.
  > 
  > We define the **Moore-Penrose Left Pseudo-Inverse** $\mathbf{X}^+$:
  > 
  > $$\mathbf{X}^+ = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \in \mathbb{R}^{P \times N}$$
  > 
  > The least squares solution simplifies to a linear matrix multiplication:
  > 
  > $$\hat{\boldsymbol{\beta}} = \mathbf{X}^+ \mathbf{y}$$
  
  ```
  ┌───────────────────────────┬───────────────────────┬───────────────────────────┬─────────────────────────────┐
  │ Case                      │ Matrix Dimensions     │ Inverse Operator          │ Mathematical Meaning        │
  ├───────────────────────────┼───────────────────────┼───────────────────────────┼─────────────────────────────┤
  │ **Square Invertible**     │ $N = P$ (e.g., $3\times 3$)│ $\mathbf{X}^{-1}$         │ **Exact Solution**          │
  │ (Slide 16)                │ Full Rank             │                           │ (Line passes through points)│
  ├───────────────────────────┼───────────────────────┼───────────────────────────┼─────────────────────────────┤
  │ **Overdetermined System** │ $N > P$ (e.g., $100\times 2$)│ $\mathbf{X}^+ = (\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T$│ **Least-Squares Best Fit**  │
  │ (Slide 16)                │ Full Column Rank      │                           │ (Minimizes residual norm)   │
  └───────────────────────────┴───────────────────────┴───────────────────────────┴─────────────────────────────┘
  ```
  
  ---
### The Four Moore-Penrose Conditions
  Formally, for any matrix $\mathbf{X} \in \mathbb{R}^{N \times P}$, its pseudo-inverse $\mathbf{X}^+ \in \mathbb{R}^{P \times N}$ is the unique matrix satisfying the four Moore-Penrose equations:
  1. $\mathbf{X} \mathbf{X}^+ \mathbf{X} = \mathbf{X}$
  2. $\mathbf{X}^+ \mathbf{X} \mathbf{X}^+ = \mathbf{X}^+$
  3. $(\mathbf{X} \mathbf{X}^+)^T = \mathbf{X} \mathbf{X}^+$ (The orthogonal projection matrix onto $\text{col}(\mathbf{X})$)
  4. $(\mathbf{X}^+ \mathbf{X})^T = \mathbf{X}^+ \mathbf{X}$ (The identity matrix $\mathbf{I}_P$ when $\mathbf{X}$ has full column rank)
  
  ---
### Numerical Note: The Condition Squaring Problem
  While the formula $\hat{\boldsymbol{\beta}} = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T \mathbf{y}$ is analytically clear, computing $\mathbf{X}^T \mathbf{X}$ explicitly on a computer can introduce numerical instability:
  * The **Condition Number** $\kappa(\mathbf{X})$ measures how sensitive a matrix is to floating-point roundoff errors:
  
  $$\kappa(\mathbf{X}^T \mathbf{X}) = \big( \kappa(\mathbf{X}) \big)^2$$
  
  * If $\mathbf{X}$ has condition number $10^4$ (poorly conditioned data), $\mathbf{X}^T \mathbf{X}$ has condition number $10^8$. In single-precision floating point (`float32`), this leads to catastrophic cancellation and loss of numerical precision.
  * **Production Alternative:** As we will see in **Part 2**, production-grade numerical libraries (such as `numpy.linalg.lstsq`) solve least squares via **QR Decomposition** or **Singular Value Decomposition (SVD)**, bypassing the explicit computation of $\mathbf{X}^T \mathbf{X}$ altogether.
  
  ---
## 7. Direct Computer Vision Applications of Least Squares
  
  Where does this linear least squares machinery appear in upcoming computer vision modules?
  
  ```
                         Computer Vision Applications of Least Squares
                                               │
         ┌─────────────────────────────────────┼─────────────────────────────────────┐
         ▼                                     ▼                                     ▼
   [ AFFINE WARPING ]                   [ LINE FITTING ]                    [ CAMERA RESECTIONING ]
  Estimate 2D affine maps              Fit boundaries to detected          Solve for projection matrix P
  from N ≥ 3 point correspondences     edge pixels (Sobel / Canny)         from 3D-to-2D correspondences:
  Matrix size: (2N × 6)                Matrix size: (N × 2)                x_i ~ P X_i
  ```
  
  ---
### Example: Fitting a 2D Affine Transformation (Module M3.2 revisited)
  Recall from the previous module that an affine transformation maps points via:
  
  $$\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} a & b \\ d & e \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} c \\ f \end{bmatrix}$$
  
  Given $N \ge 3$ noisy matched feature pairs $(x_i, y_i) \leftrightarrow (x'_i, y'_i)$, we set up an overdetermined linear system:
  
  $$\begin{bmatrix} 
  x_1 & y_1 & 1 & 0 & 0 & 0 \\
  0 & 0 & 0 & x_1 & y_1 & 1 \\
  x_2 & y_2 & 1 & 0 & 0 & 0 \\
  0 & 0 & 0 & x_2 & y_2 & 1 \\
  \vdots & \vdots & \vdots & \vdots & \vdots & \vdots \\
  x_n & y_n & 1 & 0 & 0 & 0 \\
  0 & 0 & 0 & x_n & y_n & 1
  \end{bmatrix} \begin{bmatrix} a \\ b \\ c \\ d \\ e \\ f \end{bmatrix} = \begin{bmatrix} x'_1 \\ y'_1 \\ x'_2 \\ y'_2 \\ \vdots \\ x'_n \\ y'_n \end{bmatrix}$$
  
  $$\mathbf{A}_{2N \times 6} \, \mathbf{p}_{6 \times 1} \approx \mathbf{b}_{2N \times 1}$$
  
  Solving using the Moore-Penrose pseudo-inverse yields the optimal affine parameters in a single step:
  
  $$\mathbf{p} = (\mathbf{A}^T \mathbf{A})^{-1} \mathbf{A}^T \mathbf{b}$$
  
  ---
## 8. Python Implementation: Closed-Form Least Squares and Pseudo-Inverse
  
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  
  def fit_line_least_squares(x: np.ndarray, y: np.ndarray):
    """
    Fits a 2D line y = beta_0 + beta_1 * x using the closed-form normal equations
    and compares it to the Moore-Penrose pseudo-inverse.
    
    Parameters:
        x: (N,) array of independent coordinates.
        y: (N,) array of observed noisy dependent measurements.
        
    Returns:
        beta_normal: (2,) parameter vector from normal equations [beta_0, beta_1]
        beta_pinv:   (2,) parameter vector from pseudo-inverse
    """
    N = x.shape[0]
    
    # -------------------------------------------------------------
    # Step 1: Construct the Design Matrix X (N x 2)
    # Augment with a column of ones for intercept beta_0
    # -------------------------------------------------------------
    ones_column = np.ones((N, 1), dtype=np.float64)
    x_column = x.reshape(-1, 1).astype(np.float64)
    X = np.hstack([ones_column, x_column])  # Shape: (N, 2)
    y_vec = y.reshape(-1, 1).astype(np.float64) # Shape: (N, 1)
    
    # -------------------------------------------------------------
    # Step 2: Method A - The Normal Equations: beta = (X^T X)^(-1) X^T y
    # -------------------------------------------------------------
    XT_X = X.T @ X                           # Shape: (2, 2)
    XT_y = X.T @ y_vec                       # Shape: (2, 1)
    beta_normal = np.linalg.inv(XT_X) @ XT_y # Shape: (2, 1)
    
    # -------------------------------------------------------------
    # Step 3: Method B - The Moore-Penrose Pseudo-Inverse: beta = X^+ y
    # -------------------------------------------------------------
    X_plus = np.linalg.pinv(X)               # Shape: (2, N)
    beta_pinv = X_plus @ y_vec               # Shape: (2, 1)
    
    # -------------------------------------------------------------
    # Step 4: Compute Residual Vector and Mean Squared Error
    # -------------------------------------------------------------
    predictions = X @ beta_normal
    residuals = y_vec - predictions
    mse = np.mean(residuals ** 2)
    
    return beta_normal.flatten(), beta_pinv.flatten(), mse
  
  # -------------------------------------------------------------
  # Verification on Synthetic Noisy Line Data
  # -------------------------------------------------------------
  np.random.seed(42)
  true_beta_0 = 3.5  # Intercept
  true_beta_1 = 1.8  # Slope
  
  x_data = np.linspace(0.0, 10.0, 50)
  noise = np.random.normal(loc=0.0, scale=1.5, size=50)
  y_data = true_beta_0 + true_beta_1 * x_data + noise
  
  beta_norm, beta_p, error = fit_line_least_squares(x_data, y_data)
  
  print(f"True Model:      y = {true_beta_0:.2f} + {true_beta_1:.2f} * x")
  print(f"Normal Solution: y = {beta_norm[0]:.4f} + {beta_norm[1]:.4f} * x")
  print(f"Pinv Solution:   y = {beta_p[0]:.4f} + {beta_p[1]:.4f} * x")
  print(f"Mean Squared Error (MSE): {error:.4f}")
  
  # Verification of equivalence
  assert np.allclose(beta_norm, beta_p), "Normal equations and pseudo-inverse must match!"
  ```
  
  ---
## Summary Matrix: Overdetermined Linear Systems
  
  | Concept | Mathematical Formulation | Geometric Interpretation | Computer Vision Role |
  | :--- | :--- | :--- | :--- |
  | **Observation Vector** | $\mathbf{y} \in \mathbb{R}^N$ | Point in $N$-dimensional measurement space | Noisy target coordinates ($y_i$ or $x'_i$) |
  | **Design Matrix** | $\mathbf{X} \in \mathbb{R}^{N \times P}$ | Column space $\text{col}(\mathbf{X})$ spans a $P$-dim subspace | Features, coordinates, polynomial basis |
  | **Residual Vector** | $\mathbf{e} = \mathbf{y} - \mathbf{X}\boldsymbol{\beta}$ | Vector pointing from prediction to measurement | Discrepancy between observation and model |
  | **Normal Equations** | $\mathbf{X}^T \mathbf{X} \boldsymbol{\beta} = \mathbf{X}^T \mathbf{y}$ | Enforces residual $\mathbf{e} \perp \text{col}(\mathbf{X})$ | Solves overdetermined linear regressions |
  | **Pseudo-Inverse** | $\mathbf{X}^+ = (\mathbf{X}^T \mathbf{X})^{-1} \mathbf{X}^T$ | Orthogonal projector onto $\text{col}(\mathbf{X})$ | Reversible map for rectangular systems |
  | **Hessian** | $\nabla^2 J = 2 \mathbf{X}^T \mathbf{X}$ | Curvature of the quadratic error bowl | Guarantees uniqueness of global minimum |
  
  ---