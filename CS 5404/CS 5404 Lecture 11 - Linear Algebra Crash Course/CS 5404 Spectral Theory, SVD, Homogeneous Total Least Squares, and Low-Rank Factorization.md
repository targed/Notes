## 1. Spectral Theory: Eigenvalues and Eigendecomposition (Slides 17–22)
  
  In **Part 1**, we solved overdetermined inhomogeneous linear systems ($\mathbf{X}\boldsymbol{\beta} \approx \mathbf{y}$) using the pseudo-inverse. However, many geometric problems in computer vision—such as fitting homographies, estimating fundamental matrices, and performing principal component analysis—require analyzing the **intrinsic spectral properties** of a matrix.
  
  Slide 18 introduces the fundamental eigenvalue relationship:
  
  $$\mathbf{A} \mathbf{v} = \lambda \mathbf{v}$$
  
  where $\mathbf{A} \in \mathbb{R}^{n \times n}$ is a square matrix, $\mathbf{v} \in \mathbb{R}^n \setminus \{\mathbf{0}\}$ is an **eigenvector**, and $\lambda \in \mathbb{C}$ is its associated **eigenvalue**.
  
  ```
                         Geometric Action of a Matrix
                         
       Generic Vector x (Rotates & Scales):            Eigenvector v (Pure Scaling Only!):
               ^ A·x                                           ^ A·v = λv
               │                                               │
               │   / x                                         │   / v
               │  /                                            │  /
               │ /                                             │ /
               └─────────────►                                 └─────────────►
        Direction changes!                              Direction is INVARIANT!
        x and A·x are NOT collinear.                    v and A·v lie on the EXACT SAME line.
  ```
  
  ---
### A. The Geometric Meaning of Eigenvectors (Slide 18)
  When an arbitrary matrix $\mathbf{A}$ multiplies a generic vector $\mathbf{x}$, it simultaneously **rotates** and **scales** that vector.
  * **Eigenvectors** represent the special invariant axes of the transformation: multiplying $\mathbf{v}$ by $\mathbf{A}$ produces **zero rotation**.
  * The matrix simply dilates or contracts $\mathbf{v}$ by the scalar factor $\lambda$.
  * If $\lambda > 1$: Space stretches along direction $\mathbf{v}$.
  * If $0 < \lambda < 1$: Space compresses along direction $\mathbf{v}$.
  * If $\lambda < 0$: Vector reverses direction ($180^\circ$ flip).
  * If $\lambda = 0$: The entire dimension along $\mathbf{v}$ collapses into the origin (nullspace).
  
  ---
### B. Matrix Diagonalization / Eigendecomposition (Slides 19–22)
  Slide 20 derives the matrix factorization form:
  Let $\mathbf{A} \in \mathbb{R}^{n \times n}$ possess $n$ linearly independent eigenvectors $\{\mathbf{q}_1, \mathbf{q}_2, \dots, \mathbf{q}_n\}$ with corresponding eigenvalues $\{\lambda_1, \lambda_2, \dots, \lambda_n\}$.
  
  Writing the individual eigenvalue equations side-by-side:
  
  $$\mathbf{A} \begin{bmatrix} \mathbf{q}_1 & \mathbf{q}_2 & \dots & \mathbf{q}_n \end{bmatrix} = \begin{bmatrix} \lambda_1 \mathbf{q}_1 & \lambda_2 \mathbf{q}_2 & \dots & \lambda_n \mathbf{q}_n \end{bmatrix}$$
  
  Defining the **eigenvector matrix** $\mathbf{Q} \in \mathbb{R}^{n \times n}$ and the **diagonal eigenvalue matrix** $\mathbf{\Lambda} \in \mathbb{R}^{n \times n}$:
  
  $$\mathbf{Q} = \begin{bmatrix} \mathbf{q}_1 & \mathbf{q}_2 & \dots & \mathbf{q}_n \end{bmatrix}, \quad \mathbf{\Lambda} = \begin{bmatrix} \lambda_1 & 0 & \dots & 0 \\ 0 & \lambda_2 & \dots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \dots & \lambda_n \end{bmatrix}$$
  
  We obtain the matrix relation (Slide 20):
  
  $$\mathbf{A} \mathbf{Q} = \mathbf{Q} \mathbf{\Lambda}$$
  
  Multiplying on the right by $\mathbf{Q}^{-1}$:
  
  $$\mathbf{A} = \mathbf{Q} \mathbf{\Lambda} \mathbf{Q}^{-1}$$
  
  ---
### C. The Spectral Theorem for Real Symmetric Matrices
  In computer vision, the matrices we decompose most frequently are **symmetric** ($\mathbf{A} = \mathbf{A}^T$), such as the Harris Structure Tensor $\mathbf{H} = \sum \nabla I \nabla I^T$ or normal matrices $\mathbf{X}^T \mathbf{X}$.
  
  > **The Spectral Theorem:**
  > If $\mathbf{A} \in \mathbb{R}^{n \times n}$ is real and symmetric ($\mathbf{A} = \mathbf{A}^T$):
  > 1. All $n$ eigenvalues $\lambda_i$ are **strictly real** ($\lambda_i \in \mathbb{R}$).
  > 2. Eigenvectors corresponding to distinct eigenvalues are **mutually orthogonal** ($\mathbf{q}_i \perp \mathbf{q}_j$).
  > 3. $\mathbf{Q}$ is an **orthogonal matrix** ($\mathbf{Q}^{-1} = \mathbf{Q}^T$), yielding an orthogonal decomposition:
  > 
  > $$\mathbf{A} = \mathbf{Q} \mathbf{\Lambda} \mathbf{Q}^T = \sum_{i=1}^n \lambda_i \, \mathbf{q}_i \mathbf{q}_i^T$$
  
  The matrix decomposes into a weighted sum of rank-1 projection matrices ($\mathbf{q}_i \mathbf{q}_i^T$).
  
  ---
## 2. Matrix Rank and Low-Rank Approximations (Slides 23–26)
  
  Slide 23–24 reviews the concept of rank:
  * **Row Rank:** The number of linearly independent rows.
  * **Column Rank:** The number of linearly independent columns.
  * **Fundamental Theorem:** $\text{Row Rank} \equiv \text{Column Rank} = r \le \min(m, n)$.
  * For a square diagonalizable matrix, **rank equals the number of non-zero eigenvalues**:
  
  $$\text{rank}(\mathbf{A}) = \# \{ \lambda_i \mid \lambda_i \neq 0 \}$$
  
  ---
### A. Constructing a Low-Rank Matrix (Slides 25–26)
  Slide 25 asks a classic linear algebra question:
  > *"If I gave you a $5 \times 5$ matrix of rank 5, how would you compute a $5 \times 5$ matrix of rank 3?"*
  
  Slide 26 provides the procedure:
  1. Compute the eigendecomposition: $\mathbf{A} = \mathbf{Q} \mathbf{\Lambda} \mathbf{Q}^{-1}$, sorting eigenvalues in descending magnitude: $|\lambda_1| \ge |\lambda_2| \ge \dots \ge |\lambda_5|$.
  2. **Zero out the smallest $5 - 3 = 2$ eigenvalues**:
  
  $$\mathbf{\Lambda}_{\text{rank } 3} = \text{diag}(\lambda_1, \, \lambda_2, \, \lambda_3, \, \mathbf{0}, \, \mathbf{0})$$
  
  3. Reconstruct the matrix using the modified diagonal matrix:
  
  $$\mathbf{A}_3 = \mathbf{Q} \, \mathbf{\Lambda}_{\text{rank } 3} \, \mathbf{Q}^{-1} = \sum_{i=1}^3 \lambda_i \, \mathbf{q}_i \mathbf{q}_i^T$$
  
  ```
       Original Full-Rank Spectrum (Rank 5)              Truncated Spectrum (Rank 3)
            ^ λ                                              ^ λ
        1.0 │  █                                         1.0 │  █
        0.8 │  █   █                                     0.8 │  █   █
        0.5 │  █   █   █                                 0.5 │  █   █   █
        0.1 │  █   █   █   ░                             0.0 └──┴───┴───┴───┴───┴──► Index
        0.05│  █   █   █   ░   ░                                λ₁  λ₂  λ₃  0   0
            └──┴───┴───┴───┴───┴──► Index
               λ₁  λ₂  λ₃  λ₄  λ₅                             Smallest 2 eigenvalues 
                                                              set strictly to ZERO!
  ```
  
  ---
### B. Why Perform Low-Rank Approximations? (Slide 26)
  Slide 26 outlines three primary engineering motivations:
  1. **Noise Reduction:** In experimental measurements, meaningful physical structure concentrates in the dominant, high-energy eigenvalues, while sensor noise scatters across small eigenvalues. Discarding smaller eigenvalues removes noise.
  2. **Data Compression:** Storing $\mathbf{A} \in \mathbb{R}^{n \times n}$ requires $n^2$ values. Storing rank-$k$ factors requires storing only $k$ vectors of length $n$ plus $k$ scalars ($k(2n + 1)$ values). When $k \ll n$, this yields significant storage savings.
  3. **Principal Component Analysis (PCA):** Identifying the directions of maximum variance in visual data manifolds (e.g., Eigenfaces for face recognition).
  
  ---
## 3. Singular Value Decomposition (SVD): The General Engine (Slides 27–29, 32–33)
  
  While eigendecomposition is powerful, it has three critical limitations:
  1. It is defined **only for square matrices** ($n \times n$).
  2. Defective matrices do not possess a full set of $n$ independent eigenvectors.
  3. For non-symmetric matrices, eigenvectors are not mutually orthogonal.
  
  Slide 27 introduces the general matrix factorization tool of computer vision:
  > **Singular Value Decomposition (SVD):**
  > Every real matrix $\mathbf{M} \in \mathbb{R}^{m \times n}$ (regardless of whether it is square, tall, wide, full-rank, or singular) can be factored into the product of three matrices:
  > 
  > $$\mathbf{M} = \mathbf{U} \mathbf{\Sigma} \mathbf{V}^T$$
  
  ```
                       The Full Singular Value Decomposition
                       
         M (m × n)   =     U (m × m)      ·     Σ (m × n)      ·      V^T (n × n)
       ┌───────────┐     ┌─────────────┐     ┌───────────┐          ┌─────────────┐
       │           │     │             │     │ σ₁        │          │             │
       │           │     │             │     │    σ₂     │          │             │
       │           │  =  │             │  ·  │       ... │     ·    ├─────────────┤
       │           │     │             │     │           │          │             │
       │           │     │             │     │           │          │             │
       └───────────┘     └─────────────┘     └───────────┘          └─────────────┘
                          Left Singular        Singular Values       Right Singular
                             Vectors              (Diagonal)            Vectors
  ```
  
  ---
### A. Detailed Examination of the SVD Factors (Slide 28–29)
  1. **$\mathbf{U} \in \mathbb{R}^{m \times m}$ (Left Singular Vectors):**
   * An orthogonal matrix ($\mathbf{U}^T \mathbf{U} = \mathbf{U} \mathbf{U}^T = \mathbf{I}_m$).
   * Its columns $\mathbf{u}_i$ form an orthonormal basis for the **output space $\mathbb{R}^m$**.
   * The columns of $\mathbf{U}$ are the eigenvectors of the symmetric outer-product matrix $\mathbf{M} \mathbf{M}^T \in \mathbb{R}^{m \times m}$.
  2. **$\mathbf{V} \in \mathbb{R}^{n \times n}$ (Right Singular Vectors):**
   * An orthogonal matrix ($\mathbf{V}^T \mathbf{V} = \mathbf{V} \mathbf{V}^T = \mathbf{I}_n$).
   * Its columns $\mathbf{v}_i$ form an orthonormal basis for the **input space $\mathbb{R}^n$**.
   * The columns of $\mathbf{V}$ are the eigenvectors of the symmetric inner-product matrix $\mathbf{M}^T \mathbf{M} \in \mathbb{R}^{n \times n}$.
  3. **$\mathbf{\Sigma} \in \mathbb{R}^{m \times n}$ (Singular Value Matrix):**
   * A diagonal matrix containing non-negative real numbers arranged in non-increasing order:
  
  $$\sigma_1 \ge \sigma_2 \ge \dots \ge \sigma_r > \sigma_{r+1} = \dots = 0$$
  
   * **The Singular Values ($\sigma_i$):** Equal to the positive square roots of the eigenvalues of $\mathbf{M}^T \mathbf{M}$ (Slide 29):
  
  $$\sigma_i = \sqrt{\lambda_i(\mathbf{M}^T \mathbf{M})}$$
  
  ---
### B. Geometric Action: Rotation, Scaling, Rotation (Slide 27, 28)
  Slide 27 and 28 illustrate what SVD does geometrically:
  > **The SVD decomposes any linear mapping $\mathbf{M}$ into three consecutive geometric actions:**
  > 1. $\mathbf{V}^T$: An initial **orthogonal rotation/reflection** in the input domain $\mathbb{R}^n$.
  > 2. $\mathbf{\Sigma}$: An axis-aligned **scaling/stretching** by factors $\sigma_i$.
  > 3. $\mathbf{U}$: A second **orthogonal rotation/reflection** into the output domain $\mathbb{R}^m$.
  
  ```
       Unit Sphere in R^n              Rotated Domain                     Stretched Ellipsoid              Rotated Ellipsoid in R^m
            ╭─────╮                         ╭─────╮                             ╭───────────╮                     /───────────/
          ╭─╯     ╰─╮     V^T (Rotate)    ╭─╯     ╰─╮     Σ (Stretch)         ╭─╯           ╰─╮     U (Rotate)   /           /
          │   •   │   ─────────────►    │   •   │   ─────────────►      │       •       │   ───────────► /     •     /
          ╰─╮     ╭─╯                   ╰─╮     ╭─╯                         ╰─╮           ╭─╯               /           /
            ╰─────╯                         ╰─────╯                             ╰───────────╯              /___________/
          Input vectors v_i              Basis vectors e_i                  Semi-axes = σ_i                 Output axes u_i
  ```
  
  The fundamental relationship mapping input singular vectors to output singular vectors is (Slide 29):
  
  $$\mathbf{M} \mathbf{v}_i = \sigma_i \mathbf{u}_i$$
  
  ---
### C. Compact (Thin) SVD vs. Full SVD (Slides 27, 32)
  When $\mathbf{M}$ is a tall rectangular matrix ($m > n$, typical in overdetermined vision problems with $m$ measurements and $n$ unknowns):
  * **Full SVD:** $\mathbf{U}$ is $m \times m$, $\mathbf{\Sigma}$ is $m \times n$, $\mathbf{V}$ is $n \times n$.
  * **Compact / Thin SVD (Slide 27):** 
  Because at most $r = \text{rank}(\mathbf{M}) \le \min(m, n)$ singular values are non-zero, columns of $\mathbf{U}$ beyond index $r$ multiply zeros in $\mathbf{\Sigma}$. 
  We discard the unused columns, retaining only an $m \times r$ semi-unitary matrix $\mathbf{U}_r$, an $r \times r$ diagonal matrix $\mathbf{\Sigma}_r$, and an $n \times r$ semi-unitary matrix $\mathbf{V}_r$:
  
  $$\mathbf{M} = \mathbf{U}_r \mathbf{\Sigma}_r \mathbf{V}_r^T \quad \text{where } \mathbf{U}_r^T \mathbf{U}_r = \mathbf{I}_{r \times r} \quad \text{and} \quad \mathbf{V}_r^T \mathbf{V}_r = \mathbf{I}_{r \times r}$$
  
  ---
### D. The Eckart-Young-Mirsky Low-Rank Approximation Theorem (Slides 32–33)
  Slide 33 notes: *"We can build a new matrix $\mathbf{M}'$ using $\mathbf{U}$ and $\mathbf{V}$, but use only the top non-zero singular values of $\mathbf{\Sigma}$."*
  
  > **The Eckart-Young-Mirsky Theorem (1936):**
  > For any matrix $\mathbf{M} \in \mathbb{R}^{m \times n}$ of rank $r$, the optimal rank-$k$ matrix $\mathbf{M}_k$ ($k < r$) that minimizes the approximation error under both the **Frobenius norm** and the **Spectral (2-norm)** is obtained by **truncating the SVD to its top $k$ terms**:
  > 
  > $$\mathbf{M}_k = \sum_{i=1}^k \sigma_i \, \mathbf{u}_i \mathbf{v}_i^T$$
  > 
  > The minimal reconstruction error equals the first discarded singular value:
  > 
  > $$\min_{\text{rank}(\mathbf{B}) = k} \| \mathbf{M} - \mathbf{B} \|_2 = \| \mathbf{M} - \mathbf{M}_k \|_2 = \mathbf{\sigma_{k+1}}$$
  
  * **Use in Computer Vision:**
  * **Essential Matrix Projective Enforcement:** Reconstructed essential matrices $\mathbf{E}$ must have two equal singular values and one zero singular value ($\mathbf{\Sigma} = \text{diag}(\sigma, \sigma, 0)$). We enforce this by taking the SVD of the raw estimate and setting $\sigma_3 = 0$.
  * **Fundamental Matrix Singularity:** Epipolar geometry requires $\det(\mathbf{F}) = 0$ ($\text{rank} = 2$). We enforce this using SVD by zeroing out the smallest singular value $\sigma_3$.
  
  ---
## 4. Homogeneous Linear Systems & Total Least Squares (Slides 30–31)
  
  Slide 30 introduces an equation that appears across calibration, homography fitting, and multi-view geometry:
  
  $$\min_{\mathbf{h}} \| \mathbf{A}\mathbf{h} - \mathbf{0} \|_2^2 \iff \min_{\mathbf{h}} \| \mathbf{A}\mathbf{h} \|_2^2$$
  
  ```
       Inhomogeneous Least Squares (Part 1)              Homogeneous Least Squares (Slides 30–31)
       ───────────────────────────────────              ────────────────────────────────────────
       • System: A·x ≈ b  (b ≠ 0)                       • System: A·x ≈ 0
       • Solved via Pseudo-Inverse:                     • Trivial Solution: x = 0 (Useless!)
         x = (A^T A)⁻¹ A^T b                            • Must enforce Unit-Norm constraint: ||x||² = 1
       • Minimizes VERTICAL algebraic error             • Solved via SVD / Smallest Singular Vector!
                                                        • Minimizes ORTHOGONAL geometric error (TLS)
  ```
  
  ---
### A. The Trivial Solution Trap and the Unit-Norm Constraint (Slide 31)
  If we simply minimize $\|\mathbf{A}\mathbf{h}\|_2^2$ without constraints, the optimizer chooses $\mathbf{h} = \mathbf{0}$, yielding an error of $0$.
  To find non-trivial solutions, we constrain the parameter vector to have **unit Euclidean length**:
  
  $$\min_{\mathbf{h}} \| \mathbf{A}\mathbf{h} \|_2^2 \quad \text{subject to} \quad \| \mathbf{h} \|_2^2 = 1$$
  
  ---
### B. Analytical Derivation via Lagrange Multipliers and Rayleigh Quotients (Slide 31)
  Formulating the objective function using a Lagrange multiplier $\lambda$:
  
  $$\mathcal{L}(\mathbf{h}, \lambda) = \| \mathbf{A}\mathbf{h} \|_2^2 - \lambda \big( \| \mathbf{h} \|_2^2 - 1 \big) = \mathbf{h}^T \mathbf{A}^T \mathbf{A} \mathbf{h} - \lambda (\mathbf{h}^T \mathbf{h} - 1)$$
  
  Taking the vector derivative with respect to $\mathbf{h}$ and setting it to $\mathbf{0}$:
  
  $$\frac{\partial \mathcal{L}}{\partial \mathbf{h}} = 2 \mathbf{A}^T \mathbf{A} \mathbf{h} - 2 \lambda \mathbf{h} = \mathbf{0}$$
  
  $$\mathbf{A}^T \mathbf{A} \mathbf{h} = \lambda \mathbf{h}$$
#### Key Observations:
  1. $\mathbf{h}$ must be an **eigenvector of the symmetric matrix $\mathbf{A}^T \mathbf{A}$**.
  2. Substituting $\mathbf{A}^T \mathbf{A} \mathbf{h} = \lambda \mathbf{h}$ back into the cost function:
  
  $$\| \mathbf{A}\mathbf{h} \|_2^2 = \mathbf{h}^T (\mathbf{A}^T \mathbf{A} \mathbf{h}) = \mathbf{h}^T (\lambda \mathbf{h}) = \lambda (\mathbf{h}^T \mathbf{h}) = \lambda (1) = \mathbf{\lambda}$$
  
  The cost equals the eigenvalue $\lambda$. 
  * To **maximize** the objective: Pick the eigenvector with the largest eigenvalue $\lambda_{\max}$.
  * To **minimize** the objective: **Pick the eigenvector corresponding to the SMALLEST eigenvalue $\lambda_{\min}$ of $\mathbf{A}^T \mathbf{A}$**!
  
  ---
### C. The SVD Equivalence (Slide 31)
  Slide 31 highlights the operational connection:
  Let the SVD of $\mathbf{A}$ be $\mathbf{A} = \mathbf{U} \mathbf{\Sigma} \mathbf{V}^T$. 
  
  Compute $\mathbf{A}^T \mathbf{A}$:
  
  $$\mathbf{A}^T \mathbf{A} = (\mathbf{U} \mathbf{\Sigma} \mathbf{V}^T)^T (\mathbf{U} \mathbf{\Sigma} \mathbf{V}^T) = \mathbf{V} \mathbf{\Sigma}^T \mathbf{U}^T \mathbf{U} \mathbf{\Sigma} \mathbf{V}^T = \mathbf{V} (\mathbf{\Sigma}^T \mathbf{\Sigma}) \mathbf{V}^T$$
  
  * The eigenvectors of $\mathbf{A}^T \mathbf{A}$ are the **columns of $\mathbf{V}$ (the right singular vectors of $\mathbf{A}$)**.
  * The eigenvalues of $\mathbf{A}^T \mathbf{A}$ are the squared singular values: $\lambda_i = \sigma_i^2$.
  * Because singular values are sorted in descending order ($\sigma_1 \ge \sigma_2 \ge \dots \ge \sigma_n$), the **smallest eigenvalue corresponds to the smallest singular value $\sigma_n$**.
  
  > **The Fundamental SVD Theorem for Homogeneous Systems (Slide 31):**
  > The unit vector $\mathbf{h}^*$ that minimizes $\|\mathbf{A}\mathbf{h}\|_2^2$ subject to $\|\mathbf{h}\|_2 = 1$ is **identically the LAST COLUMN OF $\mathbf{V}$ (corresponding to the smallest singular value $\sigma_{\min}$)** in the SVD of $\mathbf{A} = \mathbf{U}\mathbf{\Sigma}\mathbf{V}^T$.
  
  ---
### D. Why This is Called "Total Least Squares" (TLS)
  
  ```
        Ordinary Least Squares (OLS)                      Total Least Squares (TLS)
        (Vertical Errors Minimized)                       (Perpendicular Orthogonal Errors Minimized)
             ^ y                                               ^ y
             │        •                                        │        •
             │        │ Vertical                               │       / Perpendicular
             │        ▼ Error                                  │      ▼  Error
             │    ────X──────── Line                           │    ──X──────── Line
             │       /                                         │     /
             └──────┴──────────► x                             └────┴────────────► x
             Assumes X is noise-free,                          Assumes BOTH X and y 
             all error is in y!                                contain noise! (Symmetric)
  ```
  
  * **Ordinary Least Squares (OLS):** Assumes measurements $x$ are known exactly and noise occurs only in $y$. It minimizes vertical residuals.
  * **Total Least Squares (TLS):** Assumes **both $x$ and $y$ coordinates contain measurement noise**. It minimizes the true **orthogonal (perpendicular) Euclidean distances** from data points to the model.
  
  ---
## 5. Cholesky Decomposition (Slide 34)
  
  Slide 34 concludes the survey of linear algebra factorizations with a brief note:
  > **Cholesky Factorization:**
  > Any symmetric positive-definite (SPD) matrix $\mathbf{A} \in \mathbb{R}^{n \times n}$ ($\mathbf{A} = \mathbf{A}^T$ and $\mathbf{x}^T \mathbf{A} \mathbf{x} > 0$ for all $\mathbf{x} \neq \mathbf{0}$) can be factored uniquely into:
  > 
  > $$\mathbf{A} = \mathbf{L} \mathbf{L}^T$$
  > 
  > where $\mathbf{L}$ is a **lower triangular matrix** with strictly positive diagonal entries.
  
  ```
       Matrix A (Symmetric Positive-Definite)             Lower Triangular L
               ┌───┬───┬───┐                              ┌───┬───┬───┐
               │ a │ b │ c │                              │ l₁│ 0 │ 0 │
           A = ├───┼───┼───┤                        L =   ├───┼───┼───┤
               │ b │ d │ e │                              │ l₂│ l₃│ 0 │
               ├───┼───┼───┤                              ├───┼───┼───┤
               │ c │ e │ f │                              │ l₄│ l₅│ l₆│
               └───┴───┴───┘                              └───┴───┴───┘
  ```
### Why it Matters:
  1. **Computational Speed:** Solving the Normal Equations $(\mathbf{X}^T \mathbf{X})\boldsymbol{\beta} = \mathbf{X}^T \mathbf{y}$ via Cholesky decomposition ($\mathbf{X}^T \mathbf{X} = \mathbf{L}\mathbf{L}^T$) is **roughly twice as fast as LU decomposition** and requires half the memory.
  2. **Positive-Definiteness Verification:** An algorithm attempting a Cholesky factorization will fail (requiring square roots of negative numbers) if and only if the matrix is not positive-definite. It serves as a numerical test for positive-definiteness in optimization loops.
  
  ---
## 6. Python Implementation: SVD, Total Least Squares, and Low-Rank Approximation
  
  The following self-contained NumPy program demonstrates the core linear algebra operations discussed in this module:
  
  ```python
  import numpy as np
  
  def solve_total_least_squares_svd(A: np.ndarray) -> np.ndarray:
    """
    Solves the homogeneous linear system: min ||A * h||^2  s.t.  ||h||^2 = 1.
    Used for DLT homography estimation, fundamental matrix, and line fitting.
    """
    # Compute Singular Value Decomposition
    # U: (M, M), S: (min(M, N),), Vt: (N, N)
    U, S, Vt = np.linalg.svd(A)
    
    # The solution is the last row of Vt (last column of V)
    # corresponding to the smallest singular value S[-1]
    h_optimal = Vt[-1, :]
    
    return h_optimal
  
  def compute_low_rank_approximation(M: np.ndarray, target_rank: int) -> np.ndarray:
    """
    Computes the optimal rank-k approximation of an (m x n) matrix M
    using the Eckart-Young-Mirsky truncated SVD theorem.
    """
    U, S, Vt = np.linalg.svd(M, full_matrices=False)
    
    # Zero out singular values beyond the target rank
    S_truncated = S.copy()
    S_truncated[target_rank:] = 0.0
    
    # Reconstruct matrix: M_k = U * diag(S_truncated) * Vt
    M_low_rank = U @ np.diag(S_truncated) @ Vt
    
    reconstruction_error = S[target_rank] if target_rank < len(S) else 0.0
    return M_low_rank, reconstruction_error
  
  # -------------------------------------------------------------
  # Demonstration: Fitting an Orthogonal Line via Total Least Squares
  # -------------------------------------------------------------
  # Generate points along line 2x - 3y + 5 = 0 with 2D Gaussian noise
  np.random.seed(42)
  true_coeffs = np.array([2.0, -3.0, 5.0]) # [a, b, c]
  true_coeffs /= np.linalg.norm(true_coeffs[:2]) # Normalize normal vector
  
  t = np.linspace(-10, 10, 50)
  x_clean = t * 3.0 - (5.0 / 2.0)
  y_clean = t * 2.0
  noise = np.random.normal(0, 0.5, size=(50, 2))
  
  points_2d = np.column_stack([x_clean, y_clean]) + noise
  
  # Build measurement matrix A where each row is [x_i, y_i, 1]
  # We want to solve [x_i, y_i, 1] @ [a, b, c]^T = 0
  A_matrix = np.hstack([points_2d, np.ones((50, 1))])
  
  # Solve homogeneous TLS via SVD
  h_fit = solve_total_least_squares_svd(A_matrix)
  h_fit /= np.linalg.norm(h_fit[:2]) # Normalize for comparison
  
  print("True Line Coefficients [a, b, c]:     ", np.round(true_coeffs, 4))
  print("Fitted TLS Coefficients via SVD:     ", np.round(h_fit, 4))
  
  # Verify fit residual is near zero
  mean_residual = np.mean(np.abs(A_matrix @ h_fit))
  print(f"Mean Orthogonal Residual: {mean_residual:.4e}")
  ```
  
  ---
## Summary Matrix: The Linear Algebra Decomposition Toolkit
  
  | Factorization | Applies To | Mathematical Form | Primary Properties | Computer Vision Use Case |
  | :--- | :--- | :--- | :--- | :--- |
  | **Normal Equations** | Overdetermined ($m > n$) | $\mathbf{X}^T \mathbf{X} \boldsymbol{\beta} = \mathbf{X}^T \mathbf{y}$ | Minimizes vertical residual errors | Inhomogeneous regression, 2D affine warping |
  | **Eigendecomposition**| Square ($n \times n$) | $\mathbf{A} = \mathbf{Q} \mathbf{\Lambda} \mathbf{Q}^{-1}$ | Diagonalizes invariant dilation axes | Structure Tensor, Principal Component Analysis |
  | **Spectral Theorem** | Real Symmetric ($\mathbf{A} = \mathbf{A}^T$) | $\mathbf{A} = \mathbf{Q} \mathbf{\Lambda} \mathbf{Q}^T$ | Real eigenvalues, orthogonal eigenvectors | Error ellipses, Harris corner scoring |
  | **Singular Value Decomposition** | **Any Matrix** ($m \times n$) | $\mathbf{M} = \mathbf{U} \mathbf{\Sigma} \mathbf{V}^T$ | Orthogonal input/output bases, singular values | **Homography DLT, Total Least Squares, Essential Matrix** |
  | **Eckart-Young Truncation** | Any Matrix ($m \times n$) | $\mathbf{M}_k = \sum_{i=1}^k \sigma_i \mathbf{u}_i \mathbf{v}_i^T$ | Optimal low-rank approximation | Enforcing $\text{rank}(\mathbf{F}) = 2$ on Fundamental matrices |
  | **Cholesky Factorization** | Symmetric Positive-Definite | $\mathbf{A} = \mathbf{L} \mathbf{L}^T$ | Fast $\mathcal{O}(n^3/3)$ triangular decomposition | Fast normal equation solving, bundle adjustment |
  
  ---