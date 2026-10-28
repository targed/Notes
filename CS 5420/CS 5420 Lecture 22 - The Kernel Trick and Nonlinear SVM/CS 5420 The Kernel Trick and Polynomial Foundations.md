## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Analytical Proof of 1D Parabolic Dual-Threshold Separation:**
   * In the lifted space $\phi(x) = [x, \; x^2]^T$, the linear decision hyperplane is:
     $$w_1 x + w_2 x^2 + b = 0$$
   * Rearranging into standard quadratic form:
     $$x^2 + \left(\frac{w_1}{w_2}\right)x + \left(\frac{b}{w_2}\right) = 0$$
   * Applying the quadratic formula:
     $$x = \frac{-\frac{w_1}{w_2} \pm \sqrt{\left(\frac{w_1}{w_2}\right)^2 - 4\left(\frac{b}{w_2}\right)}}{2}$$
   * Provided the discriminant is strictly positive, the equation yields **two distinct real roots** ($x_{\text{left}}, x_{\text{right}}$). In the original 1D input space, this single straight line in $\mathbb{R}^2$ defines an interior interval $[x_{\text{left}}, x_{\text{right}}]$ for Class $+1$ and exterior intervals $(-\infty, x_{\text{left}}) \cup (x_{\text{right}}, \infty)$ for Class $-1$.
  2. **The Three KKT Stationarity and Slackness Conditions:**
   * **Rule 1 (Weight Construction):** $\nabla_w \mathcal{L} = \mathbf{0} \implies w^* = \sum_{i=1}^n \alpha_i^* y_i x_i$  
     *(The weight vector is a linear combination of training observations weighted by their dual multipliers).*
   * **Rule 2 (Label Balance):** $\frac{\partial \mathcal{L}}{\partial b} = 0 \implies \sum_{i=1}^n \alpha_i^* y_i = 0$  
     *(The sum of positive-class multipliers must equal the sum of negative-class multipliers).*
   * **Rule 3 (Complementary Slackness):** $\alpha_i^* \Big[ y_i(w^T x_i + b) - 1 \Big] = 0$  
     *(Observations outside the margin have $\alpha_i^* = 0$; only boundary support vectors have $\alpha_i^* > 0$).*
  3. **Why the Dual Formulation Outperforms the Primal in High Dimensions:**
   * In the **Primal Problem**, the optimization algorithm must solve for the weight vector $w \in \mathbb{R}^D$. If the feature mapping $\phi(x)$ maps into a $1{,}000{,}000$-dimensional space (or infinite dimensions), finding and storing $w$ is computationally impossible.
   * In the **Dual Problem**, the optimization solves for the Lagrange multipliers $\alpha \in \mathbb{R}^n$. There are **exactly $n$ multipliers** (one per training observation), completely independent of the dimensionality $D$ of the lifted space.
  
  ---
## 2. The Kernel Trick: Computing in $x$-Space, Classifying in $\phi$-Space (Slide 9)
  
  Slide 9 presents the core identity that revolutionized non-linear machine learning:
  
  $$\mathbf{f(x) = w^T x + b = \sum_{i \in \text{SVs}} \alpha_i y_i (x_i^T x) + b \quad \Longrightarrow \quad \text{Replace every dot product } x_i^T x \text{ with a kernel } K(x_i, x)}$$
  
  $$\mathbf{\text{"A linear SVM in } \phi\text{-space, computed in } x\text{-space. That is the kernel trick."}}$$
  
  ```
                       The Kernel Trick Architecture (Slide 9)
  ┌─────────────────────────────────┬─────────────────────────────────┬─────────────────────────────────┐
  │ Kernel Function                 │ Prediction Decision Rule        │ Computational Payoff            │
  ├─────────────────────────────────┼─────────────────────────────────┼─────────────────────────────────┤
  │ K(x, z) = φ(x)ᵀ φ(z)            │ f(x) = ∑_{i=1}ⁿ αᵢ yᵢ K(xᵢ, x)  │ φ(x) is NEVER explicitly built! │
  │                                 │        + b                      │                                 │
  │ A dot product in a high-dim     │ Exact same linear SVM. Only the │ Cost depends on the input       │
  │ feature space, computed         │ inner product calculation       │ dimension d, NOT on the massive │
  │ directly as a scalar function   │ changed.                        │ or infinite size of φ.          │
  │ of raw inputs x and z.          │                                 │                                 │
  └─────────────────────────────────┴─────────────────────────────────┴─────────────────────────────────┘
  ```
  
  ---
### A. The Dual Representation of Predictions
  Recall Rule 1 from the KKT derivation (Section 1):
  $$w^* = \sum_{i=1}^n \alpha_i^* y_i x_i$$
  
  Substitute this expression directly into the linear classifier's decision rule $\hat{y} = \text{sign}(w^T x + b)$:
  
  $$f(x) = \left( \sum_{i=1}^n \alpha_i^* y_i x_i \right)^T x + b = \sum_{i=1}^n \alpha_i^* y_i (\mathbf{x_i^T x}) + b$$
  
  By KKT complementary slackness, $\alpha_i^* = 0$ for all non-support vectors. The summation runs strictly over the support vectors:
  
  $$f(x) = \sum_{i \in \text{SVs}} \alpha_i^* y_i (\mathbf{x_i^T x}) + b$$
  
  Notice that **the input features appear exclusively inside an inner product** $\langle x_i, x \rangle = x_i^T x$.
  
  ---
### B. The Mapping to Lifted Space
  Now, project the inputs into an arbitrary high-dimensional feature space using an explicit non-linear basis map:
  $$x \longrightarrow \phi(x) \in \mathbb{R}^D$$
  
  The decision function in the lifted space becomes:
  
  $$f(x) = \sum_{i \in \text{SVs}} \alpha_i^* y_i \mathbf{\langle \phi(x_i), \; \phi(x) \rangle} + b$$
  
  Similarly, the dual training objective depends entirely on pairwise inner products:
  
  $$\max_\alpha \sum_{i=1}^n \alpha_i - \frac{1}{2}\sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y_i y_j \mathbf{\langle \phi(x_i), \; \phi(x_j) \rangle} \quad \text{s.t. } 0 \le \alpha_i \le C, \; \sum \alpha_i y_i = 0$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Core Discovery:                                                    │
  │ Neither training nor prediction ever requires knowing individual       │
  │ coordinates of φ(x). They require only the SCALAR INNER PRODUCT:       │
  │                                                                        │
  │                         K(x, z) = ⟨φ(x), φ(z)⟩                         │
  │                                                                        │
  │ If an algebraic identity exists that computes ⟨φ(x), φ(z)⟩ directly     │
  │ using low-dimensional inputs x and z, we can evaluate a linear SVM in  │
  │ a billion-dimensional space in a few microseconds!                     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Worked Algebraic Proof: $K(x, z) = (x^T z)^2$ (Slide 10)
  
  Slide 10 provides an explicit algebraic proof showing that a simple polynomial scalar formula computes an inner product in $\mathbb{R}^3$ without constructing the 3D vectors:
  
  $$\mathbf{\text{"You choose } K\text{; } \phi \text{ stays hidden. } (x_1 z_1 + x_2 z_2)^2 \text{ expands to exactly } \phi(x)^T \phi(z)\text{."}}$$
  
  Let $x = [x_1, x_2]^T$ and $z = [z_1, z_2]^T$ be two vectors in $\mathbb{R}^2$.
  
  ---
### Step 1: Expand the Shortcut Kernel: $K(x, z) = (x^T z)^2$
  Evaluate the inner product in $\mathbb{R}^2$ and square the resulting scalar:
  
  $$x^T z = x_1 z_1 + x_2 z_2$$
  
  Square the binomial:
  
  $$K(x, z) = (x_1 z_1 + x_2 z_2)^2 = (x_1 z_1)^2 + 2(x_1 z_1)(x_2 z_2) + (x_2 z_2)^2$$
  $$K(x, z) = \mathbf{x_1^2 z_1^2 + 2 x_1 x_2 z_1 z_2 + x_2^2 z_2^2}$$
  
  ---
### Step 2: Factor into a 3D Dot Product: $\phi(x)^T \phi(z)$
  Rewrite each of the three terms as a product of an $x$-dependent term and a $z$-dependent term:
  1. $x_1^2 z_1^2 = (x_1^2) \cdot (z_1^2)$
  2. $2 x_1 x_2 z_1 z_2 = (\sqrt{2} x_1 x_2) \cdot (\sqrt{2} z_1 z_2)$
  3. $x_2^2 z_2^2 = (x_2^2) \cdot (z_2^2)$
  
  Express this sum as an inner product of two vectors in $\mathbb{R}^3$:
  
  $$K(x, z) = \begin{bmatrix} x_1^2 \\ \sqrt{2} x_1 x_2 \\ x_2^2 \end{bmatrix}^T \begin{bmatrix} z_1^2 \\ \sqrt{2} z_1 z_2 \\ z_2^2 \end{bmatrix} = \mathbf{\phi(x)^T \phi(z)}$$
  
  where the implicit high-dimensional mapping is:
  $$\mathbf{\phi(x) = \begin{bmatrix} x_1^2 \\ \sqrt{2} x_1 x_2 \\ x_2^2 \end{bmatrix} \in \mathbb{R}^3}$$
  
  ---
### Step 3: Numerical Verification on Slide 10 Data
  Let $x = (1, 2)^T$ and $z = (3, 1)^T$.
  
  ```
                 The Two Computational Pathways (Slide 10)
    THE LONG WAY (Explicit Expansion)             THE SHORTCUT (The Kernel Trick)
  ┌────────────────────────────────────────┐    ┌────────────────────────────────────────┐
  │ 1. Map x to ℝ³:                        │    │ 1. Evaluate inner product in ℝ²:       │
  │    φ(x) = [1², √2(1)(2), 2²]           │    │    xᵀz = 1(3) + 2(1) = 3 + 2 = 5       │
  │         = [1, 2√2, 4] ≈ [1, 2.83, 4]   │    │                                        │
  │                                        │    │ 2. Square the scalar result:           │
  │ 2. Map z to ℝ³:                        │    │    K(x, z) = (5)² = 25                 │
  │    φ(z) = [3², √2(3)(1), 1²]           │    │                                        │
  │         = [9, 3√2, 1] ≈ [9, 4.24, 1]   │    │                                        │
  │                                        │    │                                        │
  │ 3. Compute dot product in ℝ³:          │    │                                        │
  │    φ(x)ᵀφ(z) = 1(9) + (2√2)(3√2) + 4(1)│    │                                        │
  │              = 9 + 12 + 4 = 25         │    │                                        │
  ├────────────────────────────────────────┤    ├────────────────────────────────────────┤
  │ Complexity: Computes six powers/roots, │    │ Complexity: Exactly two scalar mults   │
  │ allocates two 3D vectors, computes     │    │ and one scalar square.                 │
  │ three multiplications.                 │    │ φ(x) NEVER TOUCHES MEMORY.             │
  └────────────────────────────────────────┘    └────────────────────────────────────────┘
  ```
  
  Both pathways yield identically **$25$**.
  
  ---
## 4. The Polynomial Kernel Family (Slide 11)
  
  Slide 11 introduces the generalized polynomial kernel implemented in scikit-learn:
  
  $$\mathbf{K(x, z) = (\gamma \, x^T z + r)^d}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Polynomial Hyperparameters in SVC(kernel="poly"):                     │
  │                                                                        │
  │ • d (degree):    Degree of the polynomial basis expansion.             │
  │ • γ (gamma):     Scaling coefficient on the inner product (default: 1/p)│
  │ • r (coef0):     Independent constant term; controls interaction rank. │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Homogeneous vs. Inhomogeneous Kernels:                                 │
  │                                                                        │
  │ 1. Homogeneous Kernel (r = 0):  K(x, z) = (xᵀz)ᵈ                       │
  │    Contains ONLY monomials of degree EXACTLY d (e.g., x₁², x₁x₂, x₂²). │
  │                                                                        │
  │ 2. Inhomogeneous Kernel (r > 0):  K(x, z) = (xᵀz + r)ᵈ                 │
  │    Expands into all cross-product terms of degree d AND lower degrees  │
  │    down to degree 0 (constant intercept).                              │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ```
                   Solving Non-Linear Benchmarks (Slide 11)
  Concentric Rings: degree d = 2       XOR Quadrants: degree d = 2          Two Moons: degree d = 3
  Training Accuracy: 1.00              Training Accuracy: 0.99              Training Accuracy: 0.96
  ```
  
  Slide 11 concludes:
  $$\mathbf{\text{"The same curved boundaries as last class, without building a single feature."}}$$
  
  ---
## 5. What Makes a Function a Valid Kernel? Mercer’s Theorem (Slide 14)
  
  Slide 14 asks:
  
  $$\mathbf{\text{"A valid kernel must be a dot product } \phi(x)^T \phi(z) \text{ for some } \phi\text{. Thumbs up or thumbs down for each candidate."}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Candidate Functions (Slide 14):                                        │
  │                                                                        │
  │ A. K(x, z) = 3 · xᵀz                                                   │
  │ B. K(x, z) = (xᵀz + 1)³                                                │
  │ C. K(x, z) = -xᵀz                                                      │
  │ D. K(x, z) = exp(xᵀz)                                                  │
  │ E. K(x, z) = ||x - z||²                                                │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### A. Mercer’s Theorem (1909) & Positive Semi-Definite Gram Matrices
  A arbitrary function $K(x, z)$ corresponds to an inner product in a Hilbert feature space $\mathcal{H}$ if and only if it satisfies **Mercer's Condition**:
  
  For any finite dataset of $n$ points $\{x_1, \dots, x_n\}$, the **Kernel Gram Matrix** $K \in \mathbb{R}^{n \times n}$ defined by entries:
  $$K_{ij} = K(x_i, x_j)$$
  must be **Symmetric and Positive Semi-Definite (PSD)** ($K \succeq 0$):
  
  $$v^T K v \ge 0 \quad \forall \; v \in \mathbb{R}^n \iff \lambda_i(K) \ge 0 \quad \forall \; i \in \{1, \dots, n\}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Necessary Diagnostic Condition (Slide 14 Hint):                   │
  │ Evaluate the kernel on identical inputs (x = z):                       │
  │                                                                        │
  │            K(x, x) = ⟨φ(x), φ(x)⟩ = ||φ(x)||₂² ≥ 0                     │
  │                                                                        │
  │ Because the squared Euclidean norm of any real vector is non-negative, │
  │ K(x, x) MUST be non-negative for all x!                                │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### B. Valid Kernel Construction / Closure Rules
  Mercer's theorem establishes algebraic closure properties. Given valid kernels $K_1(x, z)$ and $K_2(x, z)$:
  1. **Positive Scaling:** $c \cdot K_1(x, z)$ is a valid kernel for any constant $c > 0$.
  2. **Sum of Kernels:** $K_1(x, z) + K_2(x, z)$ is a valid kernel.
  3. **Product of Kernels:** $K_1(x, z) \cdot K_2(x, z)$ is a valid kernel.
  4. **Exponential Transform:** $\exp(K_1(x, z))$ is a valid kernel.
  5. **Polynomial Transform:** $P(K_1(x, z))$ is valid for any polynomial $P(\cdot)$ with non-negative coefficients.
  
  ---
### C. Analysis of the Slide 14 Candidates:
  
  ```
  ┌──────────┬──────────────────────────┬─────────────┬──────────────────────────────────────────┐
  │ Choice   │ Candidate Function       │ Valid / Not │ Mathematical Justification               │
  ├──────────┼──────────────────────────┼─────────────┼──────────────────────────────────────────┤
  │ A        │ K(x, z) = 3 · xᵀz        │ VALID (✔)   │ Positive scaling (c = 3) of standard     │
  │          │                          │             │ linear dot product kernel xᵀz.           │
  ├──────────┼──────────────────────────┼─────────────┼──────────────────────────────────────────┤
  │ B        │ K(x, z) = (xᵀz + 1)³     │ VALID (✔)   │ Inhomogeneous polynomial kernel of       │
  │          │                          │             │ degree d = 3 with non-negative weights.  │
  ├──────────┼──────────────────────────┼─────────────┼──────────────────────────────────────────┤
  │ C        │ K(x, z) = -xᵀz           │ INVALID (✘) │ Negative scaling! Violates K(x, x) ≥ 0:  │
  │          │                          │             │ K(x, x) = -||x||² < 0 for all x ≠ 0.     │
  ├──────────┼──────────────────────────┼─────────────┼──────────────────────────────────────────┤
  │ D        │ K(x, z) = exp(xᵀz)       │ VALID (✔)   │ Taylor series exp(u) = ∑ uᵏ / k! is a sum│
  │          │                          │             │ of valid polynomial kernels with cₖ > 0. │
  ├──────────┼──────────────────────────┼─────────────┼──────────────────────────────────────────┤
  │ E        │ K(x, z) = ||x - z||²     │ INVALID (✘) │ Distance metric, NOT an inner product!   │
  │          │                          │             │ Gram matrix has negative eigenvalues;    │
  │          │                          │             │ fails positive semi-definiteness.        │
  └──────────┴──────────────────────────┴─────────────┴──────────────────────────────────────────┘
  ```
#### Detailed Proof of Failure for Candidate E: $K(x, z) = \|x - z\|^2$
  Evaluate Candidate E on identical inputs:
  $$K(x, x) = \|x - x\|^2 = 0 \quad \forall \; x$$
  This implies that every vector in the lifted space must have length zero ($\|\phi(x)\|_2 = 0 \implies \phi(x) = \mathbf{0}$).
  
  Now construct the Gram matrix for two distinct points $x = 0$ and $z = 1$:
  $$K = \begin{bmatrix} K(0, 0) & K(0, 1) \\ K(1, 0) & K(1, 1) \end{bmatrix} = \begin{bmatrix} 0 & (0 - 1)^2 \\ (1 - 0)^2 & 0 \end{bmatrix} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$$
  
  Calculate the eigenvalues of $K$:
  $$\det(K - \lambda I) = \det\begin{bmatrix} -\lambda & 1 \\ 1 & -\lambda \end{bmatrix} = \lambda^2 - 1 = 0 \implies \mathbf{\lambda = \pm 1}$$
  
  Because one eigenvalue is negative ($\lambda = -1$), the Gram matrix is **not positive semi-definite**. Candidate E cannot represent an inner product in any real Hilbert space.
  
  ---
## Summary Review Questions for Section 2
  
  1. *In your own words, what is the "Kernel Trick," and why does it eliminate the computational cost of evaluating high-dimensional feature vectors $\phi(x)$?*
  2. *Show that the kernel $K(x, z) = (x^T z + 1)^2$ for 2D inputs $x = [x_1, x_2]^T$ and $z = [z_1, z_2]^T$ corresponds to an inner product in a 6-dimensional feature space $\phi(x) \in \mathbb{R}^6$. Write out the explicit coordinates of $\phi(x)$.*
  3. *Why is $K(x, z) = -x^T z$ mathematically disqualified from being a valid kernel function? What basic property of vector norms does it violate?*
  
  ---