## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Equivalence of Laplace Noise MLE and Mean Absolute Error (MAE):**
  * Assume observations follow $y_i = w^T x_i + \epsilon_i$ with independent Laplace noise $\epsilon_i \sim \text{Laplace}(0, b)$:
   $$P(y_i \mid x_i; w) = \frac{1}{2b} \exp\left(-\frac{|y_i - w^T x_i|}{b}\right)$$
  * The joint likelihood across $N$ i.i.d. observations is $L(w) = \prod_{i=1}^N \frac{1}{2b} \exp\left(-\frac{|y_i - w^T x_i|}{b}\right)$.
  * Evaluating the Negative Log-Likelihood (NLL):
   $$\text{NLL}(w) = -\ln L(w) = N \ln(2b) + \frac{1}{b} \sum_{i=1}^N |y_i - w^T x_i|$$
  * Because $N$ and $b$ are strictly positive constants, minimizing $\text{NLL}(w)$ with respect to $w$ is mathematically identical to minimizing $\sum_{i=1}^N |y_i - w^T x_i|$, which is the $L_1$ Mean Absolute Error objective.
  2. **Why Squared Loss Yields Conditional Mean and Absolute Loss Yields Conditional Median:**
  * **Squared Loss:** Let $L(c) = \mathbb{E}\left[(Y - c)^2\right]$. Differentiating with respect to $c$:
   $$\frac{d}{dc} \mathbb{E}\left[(Y - c)^2\right] = -2 \, \mathbb{E}[Y - c] = -2(\mathbb{E}[Y] - c) = 0 \implies \mathbf{c^* = \mathbb{E}[Y]}$$
  * **Absolute Loss:** Let $L(c) = \mathbb{E}[|Y - c|] = \int_{-\infty}^c (c - y)p(y)dy + \int_c^\infty (y - c)p(y)dy$. Applying Leibniz's integral rule:
   $$\frac{d}{dc} L(c) = \int_{-\infty}^c p(y)dy - \int_c^\infty p(y)dy = P(Y \le c) - P(Y > c) = 0$$
   $$P(Y \le c) = P(Y > c) = 0.50 \implies \mathbf{c^* = \text{Median}(Y)}$$
  * Conditioning on $X$ extends these point estimators directly to the conditional expectations $\mathbb{E}[Y \mid X]$ and $\text{Median}(Y \mid X)$.
  3. **How Huber Loss Mitigates Leverage Rotations:**
  * The derivative of the Huber penalty with respect to residual $r$ is:
   $$\frac{\partial L_\delta(r)}{\partial r} = \begin{cases} r & \text{if } |r| \le \delta \\ \delta \cdot \text{sign}(r) & \text{if } |r| > \delta \end{cases}$$
  * Under Ordinary Least Squares, an outlier with residual $r = 30$ exerts a gradient restoring force proportional to $2r = 60$.
  * Under Huber loss with threshold $\delta = 1.35$, that same outlier's gradient is capped at $\delta = 1.35$. The influence of extreme points on the parameter gradient $\nabla_w J$ is bounded by a constant, preventing a single high-leverage anomaly from pulling the regression hyperplane away from the inlier distribution.
  
  ---
## 2. The Convex Geometry of Ordinary Least Squares (Slide 10)
  
  Slide 10 establishes the optimization landscape of linear models under squared error loss:
  
  $$\mathbf{\text{"Squared loss + linear model }\Longrightarrow\text{ a convex bowl with exactly one bottom."}}$$
  
  ```
                      The Quadratic Loss Bowl (Slide 10)
         3D Surface: J(w)                              Contour Projection
      J(w)                                          w_1
       ▲                                             ▲
       │        /─────────\                          │        ( ( ( ● ) ) )
       │       /           \                         │        Concentric Ellipses
       │      │      ●      │                        │        Unique Minimum:
       │       \    min    /                         │        w* = (XᵀX)⁻¹Xᵀy
       │        \─────────/                          │
       └────────────────────────▶ w_0                └────────────────────────▶ w_0
  ```
  
  Slide 10 delivers a central theoretical conclusion:
  $$\mathbf{\text{"Convex means no local minima, no initialisation to worry about, no learning rate to tune."}}$$
  $$\mathbf{\text{"There is one answer and we can write it down."}}$$
  
  ---
### Mathematical Proof of Strict Convexity
  Let $J(w) = \|y - Xw\|_2^2$. To prove that $J(w)$ forms a bowl with a unique global minimum, we inspect its second-order derivative (**Hessian Matrix**):
  
  $$H = \nabla_w^2 J(w) = 2 X^T X$$
  
  1. **Positive Semi-Definiteness:** For any non-zero vector $v \in \mathbb{R}^{d+1}$:
   $$v^T H v = v^T (2 X^T X) v = 2 (Xv)^T (Xv) = 2 \|Xv\|_2^2 \ge 0$$
   Because the Euclidean norm squared $\|Xv\|_2^2$ is non-negative for all vectors, $H$ is **positive semi-definite** ($H \succeq 0$).
  2. **Strict Convexity (Full Column Rank):**
   If the design matrix $X \in \mathbb{R}^{n \times (d+1)}$ has full column rank ($\text{rank}(X) = d+1$, meaning features are linearly independent and $n > d+1$), then $Xv = 0 \iff v = 0$.
   $$v^T H v = 2 \|Xv\|_2^2 > 0 \quad \forall \; v \neq 0 \implies \mathbf{H \succ 0 \text{ (Strictly Positive Definite)}}$$
  * **Geometric Guarantee:** The eigenvalues of $H$ are strictly positive real numbers ($\lambda_i > 0$). The loss surface is a strictly convex paraboloid. Saddle points and local minima are mathematically impossible; any stationary point where $\nabla_w J(w) = 0$ is guaranteed to be the **unique global minimum**.
  
  ---
## 3. Deriving the One-Feature Scalar Case (Slides 12–13)
  
  Slide 12 prompts the class to derive the simple linear regression parameters on paper:
  
  $$J(w_0, w_1) = \sum_{i=1}^n (y_i - w_0 - w_1 x_i)^2$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ a. Take ∂J/∂w₀ and set it to zero. What does it tell you about the     │
  │    residuals?                                                          │
  │ b. Take ∂J/∂w₁ and set it to zero.                                     │
  │ c. Solve the two equations together for w₁, then for w₀.               │
  │ d. Look at your w₁. Where have you seen that expression before?        │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Step A: The Intercept Derivative & The Zero-Residual Invariant
  Differentiate $J(w_0, w_1)$ with respect to the intercept $w_0$ using the chain rule:
  
  $$\frac{\partial J}{\partial w_0} = \sum_{i=1}^n 2(y_i - w_0 - w_1 x_i)(-1) = \mathbf{-2 \sum_{i=1}^n (y_i - w_0 - w_1 x_i) = 0}$$
  
  Since residual $r_i = y_i - \hat{y}_i = y_i - w_0 - w_1 x_i$, setting this derivative to zero yields:
  
  $$-2 \sum_{i=1}^n r_i = 0 \implies \mathbf{\sum_{i=1}^n r_i = 0} \quad \text{and} \quad \mathbf{\bar{r} = 0}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Fundamental Property 1: The Zero-Sum Residual Invariant                │
  │ The sum and empirical mean of Ordinary Least Squares residuals are     │
  │ identically zero! The model's positive errors cancel its negative     │
  │ errors perfectly across the training set.                              │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Dividing by sample size $n$:
  
  $$\frac{1}{n}\sum_{i=1}^n y_i - w_0 - w_1 \left(\frac{1}{n}\sum_{i=1}^n x_i\right) = 0 \implies \bar{y} - w_0 - w_1 \bar{x} = 0$$
  
  $$\mathbf{w_0 = \bar{y} - w_1 \bar{x}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Fundamental Property 2: The Centroid Passing Theorem (Slide 13)        │
  │ Because w₀ = ȳ - w₁x̄, evaluating the fitted line at x = x̄ yields:      │
  │ ŷ(x̄) = w₀ + w₁x̄ = (ȳ - w₁x̄) + w₁x̄ = ȳ                                  │
  │ The OLS regression line is mathematically guaranteed to pass directly  │
  │ through the center of mass of the data: (x̄, ȳ).                       │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Step B: The Slope Derivative & Orthogonality
  Differentiate $J(w_0, w_1)$ with respect to the slope $w_1$:
  
  $$\frac{\partial J}{\partial w_1} = \sum_{i=1}^n 2(y_i - w_0 - w_1 x_i)(-x_i) = \mathbf{-2 \sum_{i=1}^n x_i (y_i - w_0 - w_1 x_i) = 0}$$
  
  $$-2 \sum_{i=1}^n x_i r_i = 0 \implies \mathbf{\sum_{i=1}^n x_i r_i = 0}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Fundamental Property 3: Feature-Residual Orthogonality                 │
  │ The inner product between the input feature vector x and the residual  │
  │ vector r is zero (x ⊥ r). The residuals contain zero linear correlation│
  │ with the input features.                                               │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Steps C & D: Solving for $w_1$ (Slide 13)
  Substitute $w_0 = \bar{y} - w_1 \bar{x}$ into the slope equation:
  
  $$\sum_{i=1}^n x_i \Big( y_i - (\bar{y} - w_1 \bar{x}) - w_1 x_i \Big) = 0$$
  $$\sum_{i=1}^n x_i \Big( (y_i - \bar{y}) - w_1 (x_i - \bar{x}) \Big) = 0$$
  $$\sum_{i=1}^n x_i (y_i - \bar{y}) = w_1 \sum_{i=1}^n x_i (x_i - \bar{x})$$
  
  Applying the standard algebraic centering identities:
  $$\sum_{i=1}^n x_i (y_i - \bar{y}) = \sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})$$
  $$\sum_{i=1}^n x_i (x_i - \bar{x}) = \sum_{i=1}^n (x_i - \bar{x})^2$$
  
  Solving for $w_1$:
  
  $$\mathbf{w_1 = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^n (x_i - \bar{x})^2} = \frac{\widehat{\text{Cov}}(x, y)}{\widehat{\text{Var}}(x)} = r_{xy} \frac{s_y}{s_x}}$$
  
  Slide 13 concludes:
  $$\mathbf{\text{"The general case is the same two moves with matrices instead of sums. Nothing new happens when } d > 1\text{."}}$$
  
  ---
## 4. The 3-Step Matrix Calculus Derivation (Slide 11)
  
  Slide 11 generalizes the derivation from 1D scalar summations to $(d+1)$-dimensional matrix algebra:
  
  ```
                  From Loss to Solution, in Three Steps (Slide 11)
  ┌───────────────────────┐     ┌────────────────────────┐     ┌───────────────────────┐
  │  1. Write the Loss    │     │  2. Take the Gradient  │     │  3. Set It to Zero    │
  │  J(w) = ||y - Xw||²   │ ──▶ │  ∇J(w) = -2Xᵀ(y - Xw)  │ ──▶ │  XᵀX w = Xᵀy          │
  └───────────────────────┘     └────────────────────────┘     └───────────────────────┘
  ```
  
  ---
### Step 1: Write the Loss in Quadratic Form
  Let $y \in \mathbb{R}^n$ and $X \in \mathbb{R}^{n \times (d+1)}$. Express the sum of squared errors as an inner product:
  
  $$J(w) = \|y - Xw\|_2^2 = (y - Xw)^T (y - Xw)$$
  
  Expand the matrix transpose product:
  
  $$J(w) = \Big( y^T - (Xw)^T \Big) (y - Xw) = \Big( y^T - w^T X^T \Big) (y - Xw)$$
  $$J(w) = y^T y - y^T X w - w^T X^T y + w^T X^T X w$$
  
  Notice that $y^T X w$ is a $1 \times 1$ scalar. The transpose of a scalar equals itself:
  $$(y^T X w)^T = w^T X^T y$$
  
  Combining the two identical inner product cross-terms:
  
  $$\mathbf{J(w) = y^T y - 2 w^T X^T y + w^T X^T X w}$$
  
  ---
### Step 2: Differentiate with Respect to the Weight Vector $w$
  We apply standard vector-matrix calculus identities:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Matrix Derivative Identities:                                          │
  │ 1. Constant Vector:     ∂/∂w (yᵀy) = 0                                 │
  │ 2. Linear Inner Form:   ∂/∂w (wᵀ a) = a  ──▶  ∂/∂w (2wᵀ Xᵀy) = 2Xᵀy    │
  │ 3. Quadratic Form:      ∂/∂w (wᵀ A w) = (A + Aᵀ)w                      │
  │    Here A = XᵀX is symmetric ((XᵀX)ᵀ = XᵀX):                          │
  │                         ∂/∂w (wᵀ XᵀX w) = 2XᵀX w                       │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Differentiating $J(w)$ term-by-term:
  
  $$\nabla_w J(w) = 0 - 2 X^T y + 2 X^T X w$$
  
  $$\mathbf{\nabla_w J(w) = -2 X^T (y - Xw)}$$
  
  ---
### Step 3: Set Gradient to Zero (The Normal Equations)
  Because the loss surface is strictly convex, setting the gradient vector to the zero vector ($\mathbf{0} \in \mathbb{R}^{d+1}$) locates the unique global minimum:
  
  $$\nabla_w J(w) = \mathbf{0}$$
  $$-2 X^T (y - Xw) = \mathbf{0}$$
  $$X^T (y - Xw) = \mathbf{0}$$
  $$X^T y - X^T X w = \mathbf{0}$$
  
  $$\mathbf{X^T X w = X^T y}$$
  
  If $X^T X$ has full column rank and is invertible:
  
  $$\mathbf{w^* = (X^T X)^{-1} X^T y}$$
  
  ---
## 5. Unifying the 1D Scalar and Matrix Solutions
  
  To verify that the matrix Normal Equations reproduce the 1D scalar derivation, evaluate $X^T X w = X^T y$ for $d = 1$ with augmented design matrix $X = [\mathbf{1}, \; x]$:
  
  $$X^T X = \begin{bmatrix} \mathbf{1}^T \\ x^T \end{bmatrix} \begin{bmatrix} \mathbf{1} & x \end{bmatrix} = \begin{bmatrix} \mathbf{1}^T \mathbf{1} & \mathbf{1}^T x \\ x^T \mathbf{1} & x^T x \end{bmatrix} = \begin{bmatrix} n & \sum x_i \\ \sum x_i & \sum x_i^2 \end{bmatrix}$$
  
  $$X^T y = \begin{bmatrix} \mathbf{1}^T y \\ x^T y \end{bmatrix} = \begin{bmatrix} \sum y_i \\ \sum x_i y_i \end{bmatrix}$$
  
  Writing the matrix equation $X^T X w = X^T y$ as a system of linear equations:
  
  $$\begin{bmatrix} n & \sum x_i \\ \sum x_i & \sum x_i^2 \end{bmatrix} \begin{bmatrix} w_0 \\ w_1 \end{bmatrix} = \begin{bmatrix} \sum y_i \\ \sum x_i y_i \end{bmatrix}$$
  
  $$\begin{cases} n w_0 + w_1 \sum x_i = \sum y_i & \implies w_0 = \bar{y} - w_1 \bar{x} \\ w_0 \sum x_i + w_1 \sum x_i^2 = \sum x_i y_i & \implies w_1 = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2} \end{cases}$$
  
  The $2 \times 2$ matrix system maps directly to the scalar covariance and variance equations derived on Slide 13.
  
  ---
## Summary Review Questions for Section 3
  
  1. *Prove that the residual vector $r = y - Xw^*$ from Ordinary Least Squares is orthogonal to every column of the design matrix $X$. What does this imply about the correlation between predictions $\hat{y}$ and residuals $r$?*
  2. *Under what algebraic condition on the design matrix $X \in \mathbb{R}^{n \times (d+1)}$ does the Hessian matrix $H = 2 X^T X$ fail to be strictly positive definite? What does this mean for the uniqueness of $w^*$?*
  3. *Why is the OLS regression line mathematically guaranteed to pass through the empirical centroid $(\bar{x}, \bar{y})$, and which specific derivative condition enforces this property?*
  
  ---