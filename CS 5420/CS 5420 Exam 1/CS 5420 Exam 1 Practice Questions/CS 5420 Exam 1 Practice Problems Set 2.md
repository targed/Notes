### Question 1: Hyperparameter Optimization vs. Empirical Risk Minimization
  > **Diagnostic Statement:**  
  > *In regularized linear regression, the optimal regularization strength $\alpha^*$ can be estimated alongside the parameter weights $w$ by minimizing the joint training loss function $J(w, \alpha) = \frac{1}{n}\|y - Xw\|_2^2 + \alpha \|w\|_2^2$ with respect to both $w$ and $\alpha$ simultaneously during training.*
  
  * **Verdict:** **FALSE**
#### In-Depth Graduate Explanation:
  * **The Mathematical Proof of Collapse:**
  Consider differentiating the regularized objective function $J(w, \alpha)$ directly with respect to the hyperparameter $\alpha$:
  $$\frac{\partial J(w, \alpha)}{\partial \alpha} = \|w\|_2^2$$
  * For any non-trivial parameter vector ($w \neq \mathbf{0}$), the squared Euclidean norm $\|w\|_2^2$ is strictly positive ($\|w\|_2^2 > 0$).
  * Consequently, the gradient with respect to $\alpha$ is strictly positive everywhere on the domain:
  $$\frac{\partial J}{\partial \alpha} > 0 \quad \forall \; w \neq \mathbf{0}$$
  * If an optimization algorithm (such as gradient descent) attempts to minimize $J(w, \alpha)$ with respect to $\alpha$, it will step in the direction of the negative gradient ($-\nabla_\alpha J$), driving $\alpha \to 0$ (or $-\infty$ if unconstrained).
  * **The Inductive Boundary:** Training error is a strictly monotonically increasing function of $\alpha$. A model evaluated on training data always prefers $\alpha = 0$ (unconstrained OLS). Hyperparameters control hypothesis class capacity; they cannot be learned via empirical risk minimization on training data and must be tuned externally across hold-out validation folds via cross-validation (`RidgeCV`).
  
  ---
### Question 2: Hardware-Level Reproducibility & Cross-Platform Drift
  > **Diagnostic Statement:**  
  > *Enforcing global PRNG seeding (`random.seed(42)`, `np.random.seed(42)`, `torch.manual_seed(42)`) in an identical Python script guarantees bitwise identical floating-point outputs when executed on an Intel x86 CPU versus an NVIDIA CUDA GPU.*
  
  * **Verdict:** **FALSE**
#### In-Depth Graduate Explanation:
  * **The Systems & Numerical Reality:**
  Global PRNG seeding guarantees reproducibility *only within the same hardware architecture, instruction set, and compiled driver stack*.
  * When execution shifts from an **Intel x86 CPU** to an **NVIDIA GPU**:
  1. **Floating-Point Non-Associativity:** Under IEEE 754 standards, floating-point addition is non-associative:
     $$(a + b) + c \neq a + (b + c)$$
     On CPUs, vectorized operations execute via serial loops or SIMD registers (AVX-512) in deterministic order. On GPUs, thousands of CUDA threads execute in parallel warps; the order of atomic summation reductions (`atomicAdd`) across Streaming Multiprocessors (SMs) depends on dynamic hardware latencies.
  2. **Fused Multiply-Add (FMA) Divergence:** Modern GPUs utilize hardware FMA units computing $x \cdot y + z$ with a single rounding step at the end. Many CPU toolchains execute a separate multiplication followed by a separate addition (two round-offs).
  * Tiny microscopic differences at the 16th decimal place compound across iterative optimization steps, causing training trajectories to diverge slightly across hardware platforms despite identical random seeds.
  
  ---
### Question 3: Multi-Axis Tensor Reduction Mechanics
  > **Diagnostic Statement:**  
  > *Given a rank-3 tensor $T$ with shape `(8, 12, 16)`, computing the operation `T.mean(axis=(0, 2))` results in a 1D vector of shape `(12,)`.*
  
  * **Verdict:** **TRUE**
#### In-Depth Graduate Explanation:
  * **The Axis Principle (Lecture 5):** *"An axis argument names the axis that disappears."*
  * The tensor $T \in \mathbb{R}^{d_0 \times d_1 \times d_2}$ has dimensions:
  * $\text{axis}=0$: Size $8$
  * $\text{axis}=1$: Size $12$
  * $\text{axis}=2$: Size $16$
  * Passing a tuple of axes `axis=(0, 2)` instructs NumPy to iterate across and collapse both axis 0 and axis 2 simultaneously:
  $$\mu_j = \frac{1}{8 \times 16} \sum_{i=0}^7 \sum_{k=0}^{15} T_{i, j, k} \quad \text{for } j \in \{0, 1, \dots, 11\}$$
  * Axes 0 and 2 are eliminated from the tensor geometry, leaving only axis 1. The resulting array is a 1-dimensional vector of shape **`(12,)`**.
  
  ---
### Question 4: Distribution Moments & Affine Standardization Invariance
  > **Diagnostic Statement:**  
  > *Applying `StandardScaler` to an exponential, right-skewed feature compresses its long positive tail, driving the sample skewness coefficient to zero ($\gamma_1 \approx 0$).*
  
  * **Verdict:** **FALSE**
#### In-Depth Graduate Explanation:
  * **The Mathematical Proof of Shape Invariance:**
  Lecture 8 formalizes that **Standardization changes scale, not shape**.
  * `StandardScaler` executes an affine linear transformation:
  $$z = ax + b, \quad \text{where } a = \frac{1}{\sigma} > 0 \text{ and } b = -\frac{\mu}{\sigma}$$
  * The **Fisher-Pearson Skewness Coefficient** is defined as the standardized third central moment:
  $$\gamma_1(X) = \mathbb{E}\left[ \left(\frac{X - \mu}{\sigma}\right)^3 \right]$$
  * Evaluating the skewness of the transformed variable $Z$:
  $$\gamma_1(Z) = \mathbb{E}\left[ \left(\frac{(aX + b) - (a\mu + b)}{a\sigma}\right)^3 \right] = \mathbb{E}\left[ \left(\frac{a(X - \mu)}{a\sigma}\right)^3 \right] \equiv \mathbf{\gamma_1(X)}$$
  * The scalar $a$ cancels out entirely. Standardizing shifts the empirical mean to $0.0$ and scales the variance to $1.0$, but the **skewness and tail proportions remain identical**. Compressing heavy positive skewness requires a **non-linear, concave monotonic transformation** (e.g., $\ln(x)$ or Box-Cox).
  
  ---
### Question 5: Dummy Variable Traps Under `handle_unknown='ignore'`
  > **Diagnostic Statement:**  
  > *When `OneHotEncoder(handle_unknown='ignore')` transforms a test observation containing an unseen category, it outputs an all-zero vector, which mathematically triggers the Dummy Variable Trap in an unregularized linear model with an intercept.*
  
  * **Verdict:** **FALSE**
#### In-Depth Graduate Explanation:
  * **The Linear Algebraic Mechanics:**
  The **Dummy Variable Trap** occurs when all $K$ categories of a nominal feature are included in a model that already contains a constant intercept column ($\mathbf{1}$):
  $$\sum_{k=1}^K v_{i, k} = \mathbf{1} \quad \forall \; i$$
  Because the sum of the dummy columns equals the intercept column, the columns of the design matrix are linearly dependent, making $X^T X$ singular and non-invertible.
  * When an unseen category appears under `handle_unknown='ignore'`, the encoder emits an **all-zero vector**:
  $$v_{\text{unseen}} = [0, \; 0, \; \dots, \; 0]$$
  * The sum across the dummy columns for this row is $0$, **not $1$**:
  $$\sum_{k=1}^K v_{\text{unseen}, k} = 0 \neq 1$$
  * An all-zero vector does not introduce collinearity with the intercept vector. At prediction time, the model simply drops the categorical contribution ($w^T \mathbf{0} = 0$), relying on the base intercept $w_0$ and remaining features without causing a singular matrix crash.
  
  ---
### Question 6: Unsupervised Preprocessing & Data Leakage
  > **Diagnostic Statement:**  
  > *Applying unsupervised Principal Component Analysis (PCA) to reduce 100 continuous features to 10 components across all data BEFORE splitting into train and test sets does NOT constitute data leakage because PCA does not access the target labels $y$.*
  
  * **Verdict:** **FALSE**
#### In-Depth Graduate Explanation:
  * **The Definition of Preprocessing Leakage (Lecture 9):**
  Data leakage is not restricted to target labels; it includes any information from the test partition entering the training pipeline.
  * Unsupervised PCA calculates the empirical sample covariance matrix across all available rows:
  $$\hat{\Sigma} = \frac{1}{N_{\text{total}}} X^T X = \frac{1}{N_{\text{tr}} + N_{\text{te}}} \left( X_{\text{train}}^T X_{\text{train}} + \mathbf{X_{\text{test}}^T X_{\text{test}}} \right)$$
  * The orthonormal eigenvectors (the projection matrix $V_k$) are computed directly from $\hat{\Sigma}$.
  * **The Leak:** The coordinate axes of the principal components incorporate the variance, spread, and directional orientation of the held-out test set. The training features encode structural knowledge of the test manifold. PCA must be fitted strictly on `X_train` via `pca.fit(X_train)` and applied via `pca.transform(X_test)`.
  
  ---
### Question 7: Gradient Descent Epoch & Batch Algebra
  > **Diagnostic Statement:**  
  > *When training a linear model on $N = 64{,}000$ observations using Mini-Batch Gradient Descent with a batch size of $B = 256$, the optimizer executes exactly $2{,}500$ parameter updates over $10$ complete epochs.*
  
  * **Verdict:** **TRUE**
#### In-Depth Graduate Explanation:
  * **The Mathematical Derivation:**
  An **epoch** is defined as one complete pass through the entire training dataset.
  * 1. **Compute Updates per Epoch:**
     $$\text{Updates per Epoch} = \left\lceil \frac{N}{B} \right\rceil = \frac{64{,}000}{256} = \mathbf{250 \text{ parameter updates per epoch}}$$
  * 2. **Compute Total Updates across 10 Epochs:**
     $$\text{Total Updates} = \text{Updates per Epoch} \times \text{Epochs} = 250 \times 10 = \mathbf{2{,}500 \text{ total updates}}$$
  * Each parameter update evaluates $B = 256$ sample gradients, totaling $2{,}500$ steps across the optimization trajectory.
  
  ---
### Question 8: Topological Limits of Linear Classifiers
  > **Diagnostic Statement:**  
  > *A standard Logistic Regression model without feature engineering can construct a circular decision boundary $x_1^2 + x_2^2 = r^2$ in the raw feature space $\mathbb{R}^2$ if given a sufficiently small learning rate and a large number of training iterations.*
  
  * **Verdict:** **FALSE**
#### In-Depth Graduate Explanation:
  * **The Theoretical Limit:**
  Optimization hyperparameters (learning rate $\eta$, batch size, iteration budget `max_iter`) dictate *how efficiently* an algorithm traverses the loss surface; they **cannot alter the fundamental hypothesis class $\mathcal{H}$**.
  * In standard Logistic Regression, the decision boundary at threshold $\tau = 0.50$ is defined algebraically by:
  $$P(Y = 1 \mid x) = 0.50 \iff \sigma(w_1 x_1 + w_2 x_2 + b) = 0.50 \iff \mathbf{w_1 x_1 + w_2 x_2 + b = 0}$$
  * This equation is strictly an **affine linear equation of degree 1**. In $\mathbb{R}^2$, it can only form a straight line.
  * No amount of training or step-size tuning can bend a straight line into a closed circle ($x_1^2 + x_2^2 = r^2$). Constructing a circular decision boundary requires explicitly expanding the feature space with quadratic basis terms ($\phi(x) = [x_1, x_2, x_1^2, x_2^2]$) or utilizing a non-linear kernel.
  
  ---
### Question 9: Index-Accelerated $k$-NN Complexity
  > **Diagnostic Statement:**  
  > *Building a $k\text{-d}$ tree spatial index over a dataset converts $k$-NN from a lazy learner to an eager learner, reducing its worst-case high-dimensional ($d > 100$) query time complexity to $\mathcal{O}(\log n)$.*
  
  * **Verdict:** **FALSE**
#### In-Depth Graduate Explanation:
  * **The Systems & Algorithmic Reality:**
  * While constructing a $k\text{-d}$ tree introduces a pre-computation indexing phase ($\mathcal{O}(d \cdot n \log n)$), $k$-NN remains an instance-based model: it retains all data points in memory and estimates no parametric global function.
  * In low dimensions ($d < 20$), a $k\text{-d}$ tree prunes distant branches, accelerating average query time to $\mathcal{O}(2^d \log n)$.
  * **The High-Dimensional Breakdown:** As dimensionality expands ($d > 50\text{ to }100$), the **Curse of Dimensionality** dominates. The hyper-spherical query neighborhood intersects nearly every bounding box in the tree. The tree search fails to prune branches, degenerating into an exhaustive search across all leaves:
    $$\lim_{d \to \infty} \text{Time}_{k\text{-d tree}}(\text{Query}) = \mathbf{\mathcal{O}(n \cdot d)}$$
  * A $k\text{-d}$ tree provides zero asymptotic speedup over brute-force linear scanning in high-dimensional spaces.
  
  ---
### Question 10: Multicollinearity & Gram Matrix Invertibility
  > **Diagnostic Statement:**  
  > *If a design matrix $X \in \mathbb{R}^{n \times d}$ has rank $k < d$ due to exact multicollinearity, the Ordinary Least Squares Gram matrix $X^T X$ is singular and non-invertible, but the Ridge regression matrix $(X^T X + \alpha I)$ is strictly positive definite and invertible for any $\alpha > 0$.*
  
  * **Verdict:** **TRUE**
#### In-Depth Graduate Explanation:
  * **The Linear Algebraic Proof:**
  * If features are linearly dependent ($\text{rank}(X) = k < d$), the Gram matrix $X^T X \in \mathbb{R}^{d \times d}$ has rank $k < d$.
  * By the Rank-Nullity Theorem, $X^T X$ has a non-trivial nullspace, meaning it possesses at least $d - k$ eigenvalues equal to zero:
    $$\lambda_{\min}(X^T X) = 0 \implies \det(X^T X) = \prod_{i=1}^d \lambda_i = 0$$
  * $X^T X$ is strictly non-invertible; Ordinary Least Squares fails.
  * **The Ridge Resolution (Lecture 11):**
  Adding the regularization term $\alpha I$ shifts the entire eigenspectrum by $+\alpha$:
  $$\lambda_i(X^T X + \alpha I) = \lambda_i(X^T X) + \alpha$$
  * Because $\lambda_i(X^T X) \ge 0$ and $\alpha > 0$:
  $$\lambda_i(X^T X + \alpha I) \ge \alpha > \mathbf{0} \quad \forall \; i \in \{1, \dots, d\}$$
  * Because all eigenvalues are strictly positive real numbers, the matrix is strictly positive definite ($X^T X + \alpha I \succ 0$) and has a non-zero determinant:
  $$\det(X^T X + \alpha I) = \prod_{i=1}^d (\lambda_i + \alpha) \ge \alpha^d > 0$$
  * The Ridge matrix is guaranteed to have a unique, stable inverse regardless of collinearity.
  
  ---
## Master Checkpoint Summary: Section 2 Invariants
  
  ```
  ┌──────┬─────────┬─────────────────────────────────────────────────────────────┐
  │ Item │ Verdict │ Core Invariant Principle Tested                             │
  ├──────┼─────────┼─────────────────────────────────────────────────────────────┤
  │ Q1   │ FALSE   │ Training loss is monotonically increasing in α; ∂J/∂α > 0.  │
  │ Q2   │ FALSE   │ Hardware FMA, SIMD vs. SIMT, and IEEE 754 break bit-level.  │
  │ Q3   │ TRUE    │ axis=(0, 2) collapses 1st and 3rd axes, leaving shape (12,).│
  │ Q4   │ FALSE   │ Skewness γ₁ is invariant under positive affine scaling.     │
  │ Q5   │ FALSE   │ All-zero vectors do not introduce exact collinearity.       │
  │ Q6   │ FALSE   │ Unsupervised PCA on test data leaks feature covariance.     │
  │ Q7   │ TRUE    │ (64,000 / 256) = 250 updates/epoch; 250 × 10 = 2,500 steps. │
  │ Q8   │ FALSE   │ Logistic regression boundaries are strictly linear planes.  │
  │ Q9   │ FALSE   │ k-d trees degrade to O(n·d) brute force when d > 50.        │
  │ Q10  │ TRUE    │ αI shifts eigenvalues by +α, restoring strict invertibility.│
  └──────┴─────────┴─────────────────────────────────────────────────────────────┘
  ```
  
  ---