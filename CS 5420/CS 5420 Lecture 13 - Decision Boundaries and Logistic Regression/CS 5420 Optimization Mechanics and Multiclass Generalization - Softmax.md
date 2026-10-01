## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Analytical Proof of the Binary Cross-Entropy Derivative:**
  * Evaluating the loss for a single observation under $z = w^T x + b$:
   $$\mathcal{L}_{\text{BCE}}(z) = -\Big[ y \ln \sigma(z) + (1 - y) \ln(1 - \sigma(z)) \Big]$$
  * Differentiating with respect to $z$ via the chain rule:
   $$\frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z} = -\left[ \frac{y}{\sigma(z)} \cdot \sigma'(z) - \frac{1 - y}{1 - \sigma(z)} \cdot \sigma'(z) \right]$$
  * Substituting the sigmoid derivative identity $\sigma'(z) = \sigma(z)(1 - \sigma(z))$:
   $$\frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z} = -\left[ \frac{y}{\sigma(z)} - \frac{1 - y}{1 - \sigma(z)} \right] \sigma(z)(1 - \sigma(z))$$
   $$= -\Big[ y(1 - \sigma(z)) - (1 - y)\sigma(z) \Big] = -\Big[ y - y\sigma(z) - \sigma(z) + y\sigma(z) \Big] = \mathbf{\sigma(z) - y \equiv \hat{p} - y}$$
  2. **Why Mean Squared Error (MSE) Stalls on Confident Errors:**
  * Under Mean Squared Error $\mathcal{L}_{\text{MSE}} = (\hat{p} - y)^2$, the derivative with respect to logit $z$ is:
   $$\frac{\partial \mathcal{L}_{\text{MSE}}}{\partial z} = 2(\hat{p} - y) \cdot \sigma'(z) = 2(\hat{p} - y) \cdot \hat{p}(1 - \hat{p})$$
  * When the model is **confidently wrong** (e.g., true label $y = 1$, but logit $z \to -\infty \implies \hat{p} \to 0$):
   $$\lim_{z \to -\infty} \frac{\partial \mathcal{L}_{\text{MSE}}}{\partial z} = 2(0 - 1) \cdot (0)(1) = \mathbf{0.0}$$
  * The term $\hat{p}(1 - \hat{p})$ acts as a vanishing dampener. As predictions saturate near $0$ or $1$, the loss surface flattens into an expansive plateau with near-zero slope. The gradient update $\Delta w = -\eta \nabla J$ collapses, leaving the optimizer stranded on an uncorrected error plateau.
  3. **Convexity of Binary Cross-Entropy via the Hessian:**
  * Differentiating the gradient $\nabla_w J = \frac{1}{n} X^T (\hat{p} - y)$ with respect to $w$ yields:
   $$H_{\text{BCE}} = \nabla_w^2 J(w) = \frac{1}{n} X^T D X$$
   where $D \in \mathbb{R}^{n \times n}$ is a diagonal weighting matrix with entries $D_{ii} = \hat{p}_i (1 - \hat{p}_i)$.
  * Because $\hat{p}_i \in (0, 1)$ for all finite logits, $D_{ii} > 0$ strictly. 
  * For any arbitrary non-zero vector $v \in \mathbb{R}^{d+1}$:
   $$v^T H_{\text{BCE}} v = \frac{1}{n} (Xv)^T D (Xv) = \frac{1}{n} \sum_{i=1}^n D_{ii} (Xv)_i^2 \ge 0$$
  * Because the quadratic form is non-negative everywhere, $H_{\text{BCE}}$ is **positive semi-definite** ($H \succeq 0$). The loss surface is strictly convex; local minima do not exist, and any stationary point where $\nabla J = \mathbf{0}$ is guaranteed to be a global minimum.
  
  ---
## 2. The Unified Gradient Equation: Linear vs. Logistic Regression (Slide 7)
  
  Slide 7 presents a comparative synthesis of linear regression and logistic regression:
  
  $$\mathbf{\text{"Swap } \hat{y} \text{ for } \hat{p} \text{ and the gradient you wrote in L11 is unchanged. The } \sigma \text{ is what costs us the closed form."}}$$
  
  ```
                       The Regression vs. Classification Dichotomy (Slide 7)
  ┌──────────────────────┬──────────────────────────────┬──────────────────────────────┐
  │ Dimension            │ Linear Regression (L10)      │ Logistic Regression (L13)    │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────────┤
  │ Prediction Model     │ ŷ = Xw                       │ p̂ = σ(Xw)                    │
  │                      │ [Continuous, Unbounded]      │ [Probability, Squashed (0,1)]│
  ├──────────────────────┼──────────────────────────────┼──────────────────────────────┤
  │ Loss Function        │ Mean Squared Error (MSE)     │ Binary Cross-Entropy (BCE)   │
  │                      │ ½n ||y - Xw||²               │ -1/n ∑ [y log p̂ + (1-y)log(1-p̂)]│
  ├──────────────────────┼──────────────────────────────┼──────────────────────────────┤
  │ Parameter Gradient   │ 1/n Xᵀ(ŷ - y)                │ 1/n Xᵀ(p̂ - y)                │
  │ ∇_w J(w)             │ [EXACT SAME MATRIX SHAPE!]   │ [EXACT SAME MATRIX SHAPE!]   │
  ├──────────────────────┼──────────────────────────────┼──────────────────────────────┤
  │ Closed-Form Solution?│ YES                          │ NO                           │
  │                      │ Normal Equations:            │ Must use iterative solvers:  │
  │                      │ w* = (XᵀX)⁻¹ Xᵀy             │ Gradient Descent, L-BFGS     │
  └──────────────────────┴──────────────────────────────┴──────────────────────────────┘
  ```
  
  ---
### A. Matrix Derivation of the Logistic Regression Gradient
  We express the vector gradient of Binary Cross-Entropy across all $n$ samples using matrix calculus:
  
  $$J(w) = -\frac{1}{n} \sum_{i=1}^n \Big[ y_i \ln \sigma(x_i^T w) + (1 - y_i) \ln(1 - \sigma(x_i^T w)) \Big]$$
  
  Applying the multivariable chain rule:
  
  $$\nabla_w J(w) = \frac{1}{n} \sum_{i=1}^n \left( \frac{\partial \mathcal{L}_i}{\partial z_i} \right) \nabla_w z_i$$
  
  From Section 2, the scalar logit derivative is $\frac{\partial \mathcal{L}_i}{\partial z_i} = \hat{p}_i - y_i$. Since $z_i = x_i^T w$, its gradient with respect to $w$ is simply the feature vector $x_i$:
  
  $$\nabla_w J(w) = \frac{1}{n} \sum_{i=1}^n (\hat{p}_i - y_i) x_i$$
  
  Rewriting this linear combination of row vectors in compact matrix form:
  
  $$\mathbf{\nabla_w J(w) = \frac{1}{n} X^T (\hat{p} - y)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Why Is the Gradient Shape Identical Across Both Models?                │
  │                                                                        │
  │ In both Linear and Logistic Regression, the gradient is the dot        │
  │ product of the input features X with the residual error vector:        │
  │                                                                        │
  │          Linear Residual:   e = ŷ - y   (Continuous Error)             │
  │          Logistic Residual: e = p̂ - y   (Probability Error)            │
  │                                                                        │
  │ This identity is not a coincidence; it is a general mathematical       │
  │ property of all Generalized Linear Models (GLMs) operating under       │
  │ their canonical exponential family link functions (McCullagh & Nelder).│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Why Logistic Regression Lacks a Closed-Form Solution (Slide 7)
  
  If the gradient formula $\frac{1}{n} X^T (\hat{p} - y) = \mathbf{0}$ matches linear regression, why can't we invert it to find $w^*$ directly?
### The Transcendental Obstacle:
  To find the stationary point, we set the gradient vector to zero:
  
  $$\frac{1}{n} X^T (\hat{p} - y) = \mathbf{0} \implies X^T \hat{p} = X^T y$$
  
  Substitute the sigmoid definition $\hat{p} = \sigma(Xw)$:
  
  $$\mathbf{X^T \left( \frac{1}{1 + e^{-Xw}} \right) = X^T y}$$
  
  ```
                   The Algebraic Inversion Breakdown
          Linear Regression (Normal Eq)             Logistic Regression (Transcendental)
    ┌──────────────────────────────────────┐    ┌──────────────────────────────────────┐
    │ Xᵀ(Xw - y) = 0                       │    │ Xᵀ(σ(Xw) - y) = 0                    │
    │ XᵀX w = Xᵀy                          │    │ Xᵀ [ 1 / (1 + e⁻ˣʷ) ] = Xᵀy          │
    │                                      │    │                                      │
    │ w* = (XᵀX)⁻¹ Xᵀy                     │    │ IMPOSSIBLE TO ISOLATE w VIA MATRICES!│
    │ (Closed-form matrix inverse exists)  │    │ (Parameters trapped in denominator)  │
    └──────────────────────────────────────┘    └──────────────────────────────────────┘
  ```
  
  * In linear regression, $w$ enters the equation linearly. Factoring out $w$ yields $X^T X w = X^T y$, which is an ordinary linear system.
  * In logistic regression, $w$ is trapped inside the non-linear transcendental function $e^{-Xw}$ in the denominator.
  * There is no algebraic rearrangement of matrices that can isolate $w$. We cannot take the matrix logarithm or invert through element-wise non-linearities.
  * **The Engineering Consequence:** Logistic regression **must be solved iteratively** using numerical optimization:
  1. **First-Order Methods:** Gradient Descent ($w \leftarrow w - \eta \nabla J$).
  2. **Second-Order Methods:** **Iteratively Reweighted Least Squares (IRLS) / Newton-Raphson**:
     $$w^{(t+1)} = w^{(t)} - H^{-1} \nabla J(w^{(t)}) = (X^T D_t X)^{-1} X^T D_t \tilde{z}_t$$
  3. **Quasi-Newton Methods:** **L-BFGS** (the standard solver in `sklearn.linear_model.LogisticRegression`).
  
  ---
## 4. Multiclass Generalization: The Softmax Function (Slide 8)
  
  Slide 8 generalizes logistic regression from binary classification ($K = 2$) to multiclass classification ($K > 2$):
  
  $$\mathbf{\text{"One score per class, then softmax turns the scores into probabilities that sum to 1."}}$$
  
  $$\mathbf{\hat{p}_k = \text{softmax}(z)_k = \frac{e^{z_k}}{\sum_{j=1}^K e^{z_j}}}$$
  
  where $z_k = w_k^T x + b_k$ represents the linear score (logit) for class $k \in \{1, \dots, K\}$.
  
  ```
                     The Softmax Architecture (Slide 8)
    Input Features x               Linear Class Logits z                Softmax Probabilities p̂
  ┌──────────────────┐             ┌─────────────────────┐              ┌──────────────────────┐
  │ x₁: Petal Length │ ──[W₁, b₁]─▶│ z_setosa     = -3.94│ ──[Softmax]─▶│ p̂_setosa     = 0.001 │
  │ x₂: Petal Width  │ ──[W₂, b₂]─▶│ z_versicolor = +2.18│ ──[Softmax]─▶│ p̂_versicolor = 0.601 │
  └──────────────────┘ ──[W₃, b₃]─▶│ z_virginica  = +1.76│ ──[Softmax]─▶│ p̂_virginica  = 0.397 │
                                   └─────────────────────┘              └──────────────────────┘
                                                                           Total Sum = 1.000
  ```
  
  ---
### Core Mathematical Properties of Softmax:
  1. **Positivity:**
   Because the exponential function is strictly positive for all real inputs ($e^{z_k} > 0$), every class receives a strictly positive probability:
   $$\hat{p}_k \in (0, 1) \quad \forall \; k$$
  2. **Partition of Unity (Sum-to-One Normalization):**
   The denominator normalizes the scores, ensuring that the outputs form a valid probability distribution:
   $$\sum_{k=1}^K \hat{p}_k = \frac{\sum_{k=1}^K e^{z_k}}{\sum_{j=1}^K e^{z_j}} \equiv \mathbf{1.0}$$
  3. **Reduction to Sigmoid for $K = 2$:**
   When $K = 2$, Softmax collapses algebraically back to the logistic sigmoid function:
   $$\hat{p}_1 = \frac{e^{z_1}}{e^{z_1} + e^{z_2}} = \frac{1}{1 + e^{-(z_1 - z_2)}} = \sigma(z_1 - z_2)$$
  
  ---
## 5. Dissecting the Iris Softmax Classification Example (Slide 8)
  
  Slide 8 evaluates a flower from the Iris dataset with measurements:
  $$\text{Petal Length} = 4.8\text{ cm}, \quad \text{Petal Width} = 1.6\text{ cm}$$
  
  The model computes three raw class logits:
  $$z_{\text{setosa}} = -3.94, \quad z_{\text{versicolor}} = +2.18, \quad z_{\text{virginica}} = +1.76$$
  
  ```
                   Step-by-Step Softmax Computation (Slide 8)
  ┌──────────────────┬─────────────────┬────────────────────┬────────────────────────┐
  │ Class Category   │ Linear Score z  │ Raw Exponent eᶻ    │ Normalized Softmax p̂   │
  ├──────────────────┼─────────────────┼────────────────────┼────────────────────────┤
  │ 1. Setosa        │ -3.94           │ e⁻³·⁹⁴ ≈ 0.0194    │ 0.0194 / 14.678 = 0.001│
  │ 2. Versicolor    │ +2.18           │ e⁺²·¹⁸ ≈ 8.8463    │ 8.8463 / 14.678 = 0.601│
  │ 3. Virginica     │ +1.76           │ e⁺¹·⁷⁶ ≈ 5.8124    │ 5.8124 / 14.678 = 0.397│
  ├──────────────────┴─────────────────┼────────────────────┼────────────────────────┤
  │ Normalization Denominator ∑ eᶻⱼ:   │ 14.6781            │ Total Sum = 1.000      │
  └────────────────────────────────────┴────────────────────┴────────────────────────┘
  ```
  
  **The Prediction:** The model assigns a **$60.1\%$ probability** to Versicolor, a **$39.7\%$ probability** to Virginica, and a **$0.1\%$ probability** to Setosa. The instance is assigned to the argmax class: **Versicolor**.
  
  ---
## 6. Piecewise Linear Decision Boundaries in Multiclass Space (Slide 8)
  
  Slide 8 displays a 2D scatter plot showing the decision boundaries separating the three Iris classes:
  
  $$\mathbf{\text{"Iris: 3 classes, 3 linear pieces."}}$$
  
  ```
                 Piecewise Linear Boundaries in 2D Space (Slide 8)
    Petal Width (cm)
        ▲
    2.5 │                   \  Boundary: z_versicolor = z_virginica
        │                    \          (Virginica Region)
    2.0 │                     \      ●      ●   ●
        │                      \  ●       ●
    1.5 │                       \    ●  ●   (New Flower: 4.8, 1.6)
        │                        \  ★
    1.0 │         (Versicolor)    \
        │       ○   ○   ○  ○       \
    0.5 │     ○   ○  ○   ○
        │   ──────────────────── Boundary: z_setosa = z_versicolor
    0.0 │  ■ ■ ■  (Setosa Region)
        └────────────────────────────────────────────────────────▶ Petal Length (cm)
        0       1       2       3       4       5       6       7
  ```
  
  ---
### The Geometric Derivation of Multiclass Boundaries:
  In multiclass logistic regression, an instance $x$ is assigned to class $k$ over class $j$ if:
  $$P(Y = k \mid x) > P(Y = j \mid x) \iff \frac{e^{z_k}}{\sum_l e^{z_l}} > \frac{e^{z_j}}{\sum_l e^{z_l}} \iff e^{z_k} > e^{z_j} \iff \mathbf{z_k > z_j}$$
  
  The decision boundary separating any pair of classes $j$ and $k$ occurs at the exact tie-breaker threshold where their probabilities are identical:
  
  $$P(Y = k \mid x) = P(Y = j \mid x) \iff z_k = z_j$$
  $$(w_k^T x + b_k) = (w_j^T x + b_j)$$
  $$\mathbf{(w_k - w_j)^T x + (b_k - b_j) = 0}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Geometric Reality:                                                 │
  │                                                                        │
  │ • The boundary separating any two classes is an affine hyperplane      │
  │   governed by the difference vector: w_diff = w_k - w_j.               │
  │                                                                        │
  │ • The global decision boundary across K classes is a Voronoi-like      │
  │   tessellation composed of piecewise linear hyperplane segments.       │
  │                                                                        │
  │ • Each class occupies a convex polyhedral region in feature space.     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 8 concludes with a key roadmap note:
  $$\mathbf{\text{"Softmax is exactly the output layer you will build in PyTorch."}}$$
  
  In deep neural networks (Module 10–13), the final classification layer uses this same Softmax operator to convert high-dimensional hidden representations into normalized categorical distributions.
  
  ---
## Summary Review Questions for Section 3
  
  1. *Why does the parameter gradient for Logistic Regression ($\frac{1}{n} X^T (\hat{p} - y)$) have the exact same mathematical form as the gradient for Ordinary Least Squares ($\frac{1}{n} X^T (\hat{y} - y)$)?*
  2. *Prove that the pairwise decision boundary between class $j$ and class $k$ in Softmax regression is a flat, linear hyperplane.*
  3. *If class logits are given by $z = [1.0, 2.0, 3.0]$, calculate the Softmax probabilities. What happens to those probabilities if we add a constant $c = 10$ to all three scores ($z = [11.0, 12.0, 13.0]$)?*
  
  ---