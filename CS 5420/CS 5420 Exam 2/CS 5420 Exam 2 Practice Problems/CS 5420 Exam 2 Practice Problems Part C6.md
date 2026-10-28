#### Problem 1 (Direct Review Mirror: 2D Points)
  Given two points in $\mathbb{R}^2$:
  $$x = (2, 1), \quad z = (1, 3)$$
  Let the explicit quadratic feature mapping be defined as:
  $$\phi(x) = (x_1^2, \; \sqrt{2}x_1 x_2, \; x_2^2)$$
  * **(a)** Compute the homogeneous quadratic kernel $(x^T z)^2$ using the scalar shortcut, and then evaluate the dot product $\phi(x)^T \phi(z)$ to verify they match.
  * **(b)** Compute the inhomogeneous quadratic kernel $(x^T z + 1)^2$.
  * **(c)** Compute the Gaussian RBF kernel $K(x, z) = \exp(-\gamma \|x - z\|_2^2)$ for $\gamma = 0.50$ and $\gamma = 2.00$. Which $\gamma$ gives the narrower bump?
  
  ---
#### Problem 2 (Explicit 6D Inhomogeneous Quadratic Mapping)
  Given $x = (1, 1)$ and $z = (2, 3)$, we evaluate the kernel $K(x, z) = (x^T z + 1)^2$.
  * **(a)** Compute $K(x, z)$ using the algebraic shortcut.
  * **(b)** State the 6-dimensional explicit mapping $\phi(x) \in \mathbb{R}^6$ corresponding to this kernel.
  * **(c)** Construct vectors $\phi(x)$ and $\phi(z)$, and compute their dot product in $\mathbb{R}^6$ to verify your answer to (a).
  
  ---
#### Problem 3 (3D Input Vectors under Polynomial & RBF Kernels)
  Two observations lie in $\mathbb{R}^3$:
  $$x = (1, 0, 2), \quad z = (0, 2, 1)$$
  * **(a)** Compute the homogeneous quadratic kernel $(x^T z)^2$.
  * **(b)** Compute the inhomogeneous quadratic kernel $(x^T z + 2)^2$.
  * **(c)** Compute the squared Euclidean distance $\|x - z\|_2^2$.
  * **(d)** Evaluate the RBF kernel $K(x, z) = \exp(-\gamma \|x - z\|_2^2)$ for $\gamma = 0.20$.
  
  ---
#### Problem 4 (RBF Bandwidth Parameter $\gamma$ vs. Gaussian Variance $\sigma^2$)
  The Gaussian RBF kernel can be parameterized using bandwidth $\gamma$ or Gaussian variance $\sigma^2$:
  $$K(x, z) = \exp(-\gamma \|x - z\|_2^2) = \exp\left( -\frac{\|x - z\|_2^2}{2\sigma^2} \right)$$
  * **(a)** If a data scientist sets the Gaussian standard deviation to $\sigma = 2.00$, what is the exact equivalent value of $\gamma$?
  * **(b)** If two points have Euclidean distance $\|x - z\|_2 = 3.00$, compute the RBF similarity score $K(x, z)$ under $\sigma = 2.00$.
  * **(c)** What happens to $K(x, z)$ as the distance between points approaches zero ($\|x - z\| \to 0$)? What happens as points move infinitely far apart ($\|x - z\| \to \infty$)?
  
  ---
#### Problem 5 (Constructing a $2 \times 2$ Gram Matrix by Hand)
  Two training observations in $\mathbb{R}^2$ are given by $x_1 = (1, 2)$ and $x_2 = (3, 0)$.
  * **(a)** Construct the complete $2 \times 2$ Kernel Gram Matrix $K_{\text{poly}}$ using $K(x, z) = (x^T z)^2$.
  * **(b)** Construct the complete $2 \times 2$ Kernel Gram Matrix $K_{\text{RBF}}$ using $K(x, z) = \exp(-0.10 \|x - z\|_2^2)$.
  * **(c)** Explain why the diagonal entries of $K_{\text{RBF}}$ are guaranteed to equal $1.000$ for any dataset.
  
  ---
#### Problem 6 (Cubic Polynomial Kernel & Combinatorial Dimension)
  Given 2D input vectors $x = (1, 2)$ and $z = (2, -1)$:
  * **(a)** Compute the homogeneous cubic kernel $(x^T z)^3$.
  * **(b)** Compute the inhomogeneous cubic kernel $(x^T z + 1)^3$.
  * **(c)** Using the combinations formula $D = \binom{d + M}{M}$, calculate the exact dimensionality $D$ of the feature space $\phi(x)$ represented by $(x^T z + 1)^3$ for 2D inputs ($d = 2, M = 3$).
  
  ---
#### Problem 7 (Verifying Mercer's Condition on Small Matrices)
  Consider two points $x = (2, 0)$ and $z = (0, 2)$.
  * **(a)** Evaluate candidate function $K_A(u, v) = -u^T v$ on identical inputs: $K_A(x, x)$. Why does this immediately prove $K_A$ is an invalid kernel?
  * **(b)** Consider candidate function $K_B(u, v) = \|u - v\|_2^2$. Construct the $2 \times 2$ Gram matrix for $\{x, z\}$, compute its eigenvalues, and prove that it violates Mercer's condition.
  * **(c)** Consider candidate function $K_C(u, v) = \exp(u^T v)$. Evaluate $K_C(x, z)$. Is $K_C$ a valid kernel?
  
  ---
#### Problem 8 (Kernel Algebra & Closure Properties)
  Let $K_1(x, z) = x^T z$ (linear kernel) and $K_2(x, z) = (x^T z)^2$ (homogeneous quadratic kernel).
  * **(a)** Define composite kernel $K_3(x, z) = 2 K_1(x, z) + 3 K_2(x, z)$. Evaluate $K_3(x, z)$ for $x = (1, 2)$ and $z = (2, 1)$.
  * **(b)** Define product kernel $K_4(x, z) = K_1(x, z) \cdot K_2(x, z)$. Which standard polynomial kernel is $K_4$ mathematically equivalent to?
  * **(c)** Under Mercer's closure rules, why are $K_3$ and $K_4$ guaranteed to be valid kernels without constructing their explicit $\phi$ mappings?
  
  ---
#### Problem 9 (Evaluating an SVM Decision Function by Hand with Kernels)
  A trained non-linear Support Vector Machine has bias $b = -1.50$ and two support vectors:
  * $x_1 = (1, 1), \quad y_1 = -1, \quad \alpha_1 = 0.50$
  * $x_2 = (3, 3), \quad y_2 = +1, \quad \alpha_2 = 0.50$
  
  The model uses the polynomial kernel $K(x, z) = (x^T z)^2$.  
  The decision function is:
  $$f(x) = \sum_{i=1}^2 \alpha_i y_i K(x_i, x) + b$$
  * **(a)** For an unclassified test point $x_{\text{test}} = (2, 2)$, evaluate $K(x_1, x_{\text{test}})$ and $K(x_2, x_{\text{test}})$.
  * **(b)** Compute $f(x_{\text{test}})$ and predict the discrete class label $\hat{y} \in \{-1, +1\}$.
  * **(c)** Did this evaluation require constructing or storing any high-dimensional vectors?
  
  ---
#### Problem 10 (RBF Decision Function & Landmark Influence)
  A fitted RBF-kernel SVM has bandwidth $\gamma = 0.50$, bias $b = -0.20$, and two support vectors:
  * $x_1 = (1, 0), \quad y_1 = +1, \quad \alpha_1 = 1.00$
  * $x_2 = (0, 1), \quad y_2 = -1, \quad \alpha_2 = 1.00$
  * **(a)** Write out the explicit algebraic decision function $f(x)$ in terms of Euclidean distances to the support vectors.
  * **(b)** Evaluate $f(x)$ for query point $x = (1, 0)$ (the first support vector itself).
  * **(c)** Evaluate $f(x)$ for query point $x = (2, 0)$, and assign the predicted class label.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C6
### Standard Formulas & Identities:
  * **Homogeneous Polynomial Kernel:** $K(x, z) = (x^T z)^d$
  * **Inhomogeneous Polynomial Kernel:** $K(x, z) = (x^T z + c)^d$
  * **Gaussian RBF Kernel:** $K(x, z) = \exp(-\gamma \|x - z\|_2^2) = \exp\left(-\frac{\|x - z\|_2^2}{2\sigma^2}\right)$
  * **Squared Euclidean Distance:** $\|x - z\|_2^2 = \sum_{j=1}^d (x_j - z_j)^2$
  * **SVM Decision Rule:** $f(x) = \sum_{i \in \text{SVs}} \alpha_i y_i K(x_i, x) + b, \quad \hat{y} = \text{sign}(f(x))$
  
  ---
### Problem 1 Solution
  * **(a) Homogeneous Quadratic Kernel $(x^T z)^2$:**
  1. *Scalar Shortcut:*
     $$x^T z = x_1 z_1 + x_2 z_2 = 2(1) + 1(3) = 2 + 3 = 5.000$$
     $$K(x, z) = (x^T z)^2 = (5.000)^2 = \mathbf{25.000}$$
  2. *With Explicit $\phi(u) = (u_1^2, \; \sqrt{2}u_1 u_2, \; u_2^2)$:*
     $$\phi(x) = \begin{bmatrix} 2^2 \\ \sqrt{2}(2)(1) \\ 1^2 \end{bmatrix} = \begin{bmatrix} 4 \\ 2\sqrt{2} \\ 1 \end{bmatrix}, \quad \phi(z) = \begin{bmatrix} 1^2 \\ \sqrt{2}(1)(3) \\ 3^2 \end{bmatrix} = \begin{bmatrix} 1 \\ 3\sqrt{2} \\ 9 \end{bmatrix}$$
     $$\phi(x)^T \phi(z) = 4(1) + (2\sqrt{2})(3\sqrt{2}) + 1(9) = 4 + 6(2) + 9 = 4 + 12 + 9 = \mathbf{25.000} \quad \text{[Verified!]}$$
  
  * **(b) Inhomogeneous Quadratic Kernel $(x^T z + 1)^2$:**
  $$(x^T z + 1)^2 = (5.000 + 1)^2 = (6.000)^2 = \mathbf{36.000}$$
  
  * **(c) Gaussian RBF Kernel:**
  Compute squared Euclidean distance:
  $$\|x - z\|_2^2 = (2 - 1)^2 + (1 - 3)^2 = 1^2 + (-2)^2 = 1 + 4 = \mathbf{5.000}$$
  * For $\gamma = 0.50$:
    $$K(x, z) = \exp(-0.50 \times 5.000) = \exp(-2.5) \approx \mathbf{0.082}$$
  * For $\gamma = 2.00$:
    $$K(x, z) = \exp(-2.00 \times 5.000) = \exp(-10.0) \approx \mathbf{0.000} \quad (4.54 \times 10^{-5})$$
  * **Narrower Bump:** **$\gamma = 2.00$**. (Higher $\gamma$ causes similarity to drop to zero much faster with distance, creating a narrow spike). `[L22, Review C6]`
  
  ---
### Problem 2 Solution
  * **(a) Scalar Shortcut:**
  $$x^T z = 1(2) + 1(3) = 2 + 3 = 5.000$$
  $$K(x, z) = (x^T z + 1)^2 = (5.000 + 1)^2 = 6^2 = \mathbf{36.000}$$
  
  * **(b) 6D Explicit Mapping $\phi(x)$:**
  Expand $(x_1 z_1 + x_2 z_2 + 1)^2$:
  $$(x_1 z_1)^2 + (x_2 z_2)^2 + 1^2 + 2(x_1 z_1)(x_2 z_2) + 2(x_1 z_1)(1) + 2(x_2 z_2)(1)$$
  $$= (x_1^2)(z_1^2) + (x_2^2)(z_2^2) + (\sqrt{2}x_1 x_2)(\sqrt{2}z_1 z_2) + (\sqrt{2}x_1)(\sqrt{2}z_1) + (\sqrt{2}x_2)(\sqrt{2}z_2) + (1)(1)$$
  $$\mathbf{\phi(x) = \begin{bmatrix} x_1^2, & x_2^2, & \sqrt{2}x_1 x_2, & \sqrt{2}x_1, & \sqrt{2}x_2, & 1 \end{bmatrix}^T \in \mathbb{R}^6}$$
  
  * **(c) Evaluate Dot Product in $\mathbb{R}^6$:**
  $$\phi(x) = \begin{bmatrix} 1^2, & 1^2, & \sqrt{2}(1)(1), & \sqrt{2}(1), & \sqrt{2}(1), & 1 \end{bmatrix}^T = \begin{bmatrix} 1, & 1, & \sqrt{2}, & \sqrt{2}, & \sqrt{2}, & 1 \end{bmatrix}^T$$
  $$\phi(z) = \begin{bmatrix} 2^2, & 3^2, & \sqrt{2}(2)(3), & \sqrt{2}(2), & \sqrt{2}(3), & 1 \end{bmatrix}^T = \begin{bmatrix} 4, & 9, & 6\sqrt{2}, & 2\sqrt{2}, & 3\sqrt{2}, & 1 \end{bmatrix}^T$$
  $$\phi(x)^T \phi(z) = 1(4) + 1(9) + (\sqrt{2})(6\sqrt{2}) + (\sqrt{2})(2\sqrt{2}) + (\sqrt{2})(3\sqrt{2}) + 1(1)$$
  $$= 4 + 9 + 12 + 4 + 6 + 1 = \mathbf{36.000} \quad \text{[Matches (a)!]}$$ `[L22]`
  
  ---
### Problem 3 Solution
  * **(a) Homogeneous Quadratic Kernel in 3D:**
  $$x^T z = 1(0) + 0(2) + 2(1) = 0 + 0 + 2 = 2.000$$
  $$(x^T z)^2 = (2.000)^2 = \mathbf{4.000}$$
  
  * **(b) Inhomogeneous Quadratic Kernel:**
  $$(x^T z + 2)^2 = (2.000 + 2)^2 = (4.000)^2 = \mathbf{16.000}$$
  
  * **(c) Squared Euclidean Distance:**
  $$\|x - z\|_2^2 = (1 - 0)^2 + (0 - 2)^2 + (2 - 1)^2 = 1^2 + (-2)^2 + 1^2 = 1 + 4 + 1 = \mathbf{6.000}$$
  
  * **(d) RBF Kernel ($\gamma = 0.20$):**
  $$K(x, z) = \exp(-0.20 \times 6.000) = \exp(-1.20) \approx \mathbf{0.301}$$ `[L22]`
  
  ---
### Problem 4 Solution
  * **(a) Equivalent $\gamma$ for $\sigma = 2.00$:**
  $$\gamma = \frac{1}{2\sigma^2} = \frac{1}{2(2.00)^2} = \frac{1}{2(4.00)} = \frac{1}{8} = \mathbf{0.125}$$
  
  * **(b) Evaluate RBF for $\|x - z\|_2 = 3.00$:**
  $$\|x - z\|_2^2 = (3.00)^2 = 9.000$$
  $$K(x, z) = \exp(-\gamma \|x - z\|^2) = \exp(-0.125 \times 9.000) = \exp(-1.125) \approx \mathbf{0.325}$$
  
  * **(c) Asymptotic Behavior:**
  * When $\|x - z\|_2 \to 0$: $K(x, z) \to e^0 = \mathbf{1.000}$ (identical points achieve maximum similarity).
  * When $\|x - z\|_2 \to \infty$: $K(x, z) \to e^{-\infty} = \mathbf{0.000}$ (infinitely distant points exert zero influence). `[L22]`
  
  ---
### Problem 5 Solution
  * **(a) Construct $K_{\text{poly}} = (x^T z)^2$:**
  * $K(x_1, x_1) = (x_1^T x_1)^2 = (1^2 + 2^2)^2 = (1 + 4)^2 = 5^2 = 25.000$
  * $K(x_2, x_2) = (x_2^T x_2)^2 = (3^2 + 0^2)^2 = (9 + 0)^2 = 9^2 = 81.000$
  * $K(x_1, x_2) = K(x_2, x_1) = (x_1^T x_2)^2 = (1(3) + 2(0))^2 = 3^2 = 9.000$
  $$\mathbf{K_{\text{poly}} = \begin{bmatrix} 25.000 & 9.000 \\ 9.000 & 81.000 \end{bmatrix}}$$
  
  * **(b) Construct $K_{\text{RBF}} = \exp(-0.10 \|x - z\|^2)$:**
  * $\|x_1 - x_1\|^2 = 0 \implies K(x_1, x_1) = e^0 = 1.000$
  * $\|x_2 - x_2\|^2 = 0 \implies K(x_2, x_2) = e^0 = 1.000$
  * $\|x_1 - x_2\|^2 = (1 - 3)^2 + (2 - 0)^2 = (-2)^2 + 2^2 = 4 + 4 = 8.000$
  * $K(x_1, x_2) = K(x_2, x_1) = \exp(-0.10 \times 8.000) = \exp(-0.80) \approx 0.449$
  $$\mathbf{K_{\text{RBF}} = \begin{bmatrix} 1.000 & 0.449 \\ 0.449 & 1.000 \end{bmatrix}}$$
  
  * **(c) Why RBF Diagonals Equal 1.000:**
  For any point $x_i$, the Euclidean distance to itself is identically zero:
  $$\|x_i - x_i\|_2^2 = 0 \implies K(x_i, x_i) = \exp(-\gamma \cdot 0) = e^0 \equiv \mathbf{1.000}$$ `[L22]`
  
  ---
### Problem 6 Solution
  * **(a) Homogeneous Cubic Kernel:**
  $$x^T z = 1(2) + 2(-1) = 2 - 2 = 0.000$$
  $$(x^T z)^3 = (0.000)^3 = \mathbf{0.000}$$
  
  * **(b) Inhomogeneous Cubic Kernel:**
  $$(x^T z + 1)^3 = (0.000 + 1)^3 = 1^3 = \mathbf{1.000}$$
  
  * **(c) Dimension of Feature Space $\phi(x)$ ($d = 2, M = 3$):**
  $$D = \binom{d + M}{M} = \binom{2 + 3}{3} = \binom{5}{3} = \frac{5!}{3! \, 2!} = \frac{5 \times 4}{2 \times 1} = \mathbf{10 \text{ dimensions}}$$
  *The explicit basis is: $\phi(x) = [1, x_1, x_2, x_1^2, x_1 x_2, x_2^2, x_1^3, x_1^2 x_2, x_1 x_2^2, x_2^3]^T$.* `[L21, L22]`
  
  ---
### Problem 7 Solution
  * **(a) Test Candidate $K_A(u, v) = -u^T v$ on $x = (2, 0)$:**
  $$K_A(x, x) = -x^T x = -(2^2 + 0^2) = \mathbf{-4.000 < 0}$$
  *Invalidity:* Under Mercer's theorem, an inner product of any vector with itself must equal its squared norm: $K(x, x) = \|\phi(x)\|_2^2 \ge 0$. A negative self-norm is mathematically impossible in a real Hilbert space.
  
  * **(b) Test Candidate $K_B(u, v) = \|u - v\|_2^2$ on $x = (2, 0)$ and $z = (0, 2)$:**
  * $K_B(x, x) = \|x - x\|^2 = 0.000, \quad K_B(z, z) = \|z - z\|^2 = 0.000$
  * $K_B(x, z) = (2 - 0)^2 + (0 - 2)^2 = 4 + 4 = 8.000$
  $$K = \begin{bmatrix} 0 & 8 \\ 8 & 0 \end{bmatrix}$$
  Compute eigenvalues:
  $$\det(K - \lambda I) = \det\begin{bmatrix} -\lambda & 8 \\ 8 & -\lambda \end{bmatrix} = \lambda^2 - 64 = 0 \implies \mathbf{\lambda = \pm 8.000}$$
  *Invalidity:* The Gram matrix has a strictly negative eigenvalue ($\lambda = -8$). It violates **Positive Semi-Definiteness (PSD)**, so it cannot represent an inner product.
  
  * **(c) Candidate $K_C(u, v) = \exp(u^T v)$:**
  $$x^T z = 2(0) + 0(2) = 0 \implies K_C(x, z) = \exp(0) = \mathbf{1.000}$$
  *Validity:* **VALID**. By Mercer's closure rules, the exponential of a valid kernel (the linear dot product $u^T v$) is guaranteed to produce a valid Mercer kernel. `[L22]`
  
  ---
### Problem 8 Solution
  * **(a) Composite Kernel $K_3(x, z) = 2 K_1 + 3 K_2$ for $x = (1, 2), z = (2, 1)$:**
  $$x^T z = 1(2) + 2(1) = 2 + 2 = 4.000$$
  $$K_1(x, z) = x^T z = 4.000$$
  $$K_2(x, z) = (x^T z)^2 = 4^2 = 16.000$$
  $$K_3(x, z) = 2(4.000) + 3(16.000) = 8.000 + 48.000 = \mathbf{56.000}$$
  
  * **(b) Product Kernel $K_4(x, z) = K_1 \cdot K_2$:**
  $$K_4(x, z) = (x^T z) \cdot (x^T z)^2 = \mathbf{(x^T z)^3 \quad (\text{Homogeneous Cubic Polynomial Kernel})}$$
  
  * **(c) Closure Rationale:**
  * By Mercer’s Theorem, if $K_1$ and $K_2$ are positive semi-definite Gram operators:
    * Their non-negative linear combination $c_1 K_1 + c_2 K_2$ has Gram matrix $c_1 K_1 + c_2 K_2 \succeq 0$.
    * Their element-wise product (Hadamard product) has Gram matrix $K_1 \circ K_2$, which is guaranteed to be positive semi-definite by the **Schur Product Theorem**. `[L22]`
  
  ---
### Problem 9 Solution
  * **(a) Evaluate Polynomial Kernels Against Support Vectors:**
  $$x_{\text{test}} = (2, 2)$$
  * Against $x_1 = (1, 1)$:
    $$x_1^T x_{\text{test}} = 1(2) + 1(2) = 4.000 \implies K(x_1, x_{\text{test}}) = (4.000)^2 = \mathbf{16.000}$$
  * Against $x_2 = (3, 3)$:
    $$x_2^T x_{\text{test}} = 3(2) + 3(2) = 6 + 6 = 12.000 \implies K(x_2, x_{\text{test}}) = (12.000)^2 = \mathbf{144.000}$$
  
  * **(b) Compute Decision Function and Predict:**
  $$f(x_{\text{test}}) = \alpha_1 y_1 K(x_1, x_{\text{test}}) + \alpha_2 y_2 K(x_2, x_{\text{test}}) + b$$
  $$f(x_{\text{test}}) = (0.50)(-1)(16.000) + (0.50)(+1)(144.000) - 1.50$$
  $$f(x_{\text{test}}) = -8.000 + 72.000 - 1.50 = \mathbf{+62.500}$$
  $$\hat{y} = \text{sign}(+62.500) = \mathbf{+1 \quad (\text{Class } +1)}$$
  
  * **(c) Computational Payoff:**
  **No.** The evaluation required only scalar dot products in the 2D input space and scalar multiplications. No 3D vectors were constructed. `[L22]`
  
  ---
### Problem 10 Solution
  * **(a) Explicit Decision Function:**
  $$f(x) = \alpha_1 y_1 \exp(-\gamma \|x - x_1\|^2) + \alpha_2 y_2 \exp(-\gamma \|x - x_2\|^2) + b$$
  $$\mathbf{f(x) = \exp\Big(-0.50 \|x - (1, 0)\|_2^2\Big) - \exp\Big(-0.50 \|x - (0, 1)\|_2^2\Big) - 0.20}$$
  
  * **(b) Evaluate at Support Vector $x = (1, 0)$:**
  * Distance to $x_1(1, 0)$: $\|(1, 0) - (1, 0)\|^2 = 0 \implies \exp(-0.5 \times 0) = e^0 = 1.000$
  * Distance to $x_2(0, 1)$: $\|(1, 0) - (0, 1)\|^2 = (1 - 0)^2 + (0 - 1)^2 = 1 + 1 = 2 \implies \exp(-0.5 \times 2) = e^{-1} \approx 0.368$
  $$f(1, 0) = 1.000 - 0.368 - 0.200 = \mathbf{+0.432 > 0 \implies \text{Class } +1}$$
  
  * **(c) Evaluate at Query Point $x = (2, 0)$:**
  * Distance to $x_1(1, 0)$: $\|(2, 0) - (1, 0)\|^2 = (2 - 1)^2 + 0^2 = 1 \implies \exp(-0.5 \times 1) = e^{-0.5} \approx 0.607$
  * Distance to $x_2(0, 1)$: $\|(2, 0) - (0, 1)\|^2 = (2 - 0)^2 + (0 - 1)^2 = 4 + 1 = 5 \implies \exp(-0.5 \times 5) = e^{-2.5} \approx 0.082$
  $$f(2, 0) = 0.607 - 0.082 - 0.200 = \mathbf{+0.325 > 0 \implies \text{Class } +1}$$ `[L22]`
  
  ---