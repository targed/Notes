## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Proof of Weight Vector Orthogonality to the Decision Boundary:**
  * Let $x_A$ and $x_B$ be any two distinct points lying directly on the decision boundary:
   $$w^T x_A + b = 0 \quad \text{and} \quad w^T x_B + b = 0$$
  * Subtracting these two linear equations:
   $$(w^T x_A + b) - (w^T x_B + b) = 0 \implies w^T (x_A - x_B) = 0$$
  * The vector $(x_A - x_B)$ represents an arbitrary displacement vector parallel to the decision surface. Because the inner product between $w$ and $(x_A - x_B)$ is identically zero, the parameter vector $w$ is **strictly orthogonal (normal)** to the decision boundary.
  2. **Calculating Odds and Probability from $z = -2.197$:**
  * **Odds:**
   $$\text{Odds} = e^z = e^{-2.197} \approx \mathbf{0.1111} \quad \left(\approx \frac{1}{9}\right)$$
  * **Probability:**
   $$p = \sigma(z) = \frac{\text{Odds}}{1 + \text{Odds}} = \frac{0.1111}{1 + 0.1111} = \frac{0.1111}{1.1111} \approx \mathbf{0.1000} \quad (\mathbf{10\%})$$
  3. **Why Logistic Regression Is Classified as a Linear Classifier:**
  * A classifier is defined as "linear" based on the geometry of its **decision boundary in feature space**, not the shape of its activation function.
  * Class predictions are assigned by thresholding the probability at $\tau = 0.50$:
   $$\hat{y} = 1 \iff P(Y = 1 \mid x) \ge 0.50 \iff \sigma(w^T x + b) \ge 0.50 \iff \mathbf{w^T x + b \ge 0}$$
  * The boundary separating the classes is the locus of points where $w^T x + b = 0$, which is a **flat, $(d-1)$-dimensional affine hyperplane**.
  * In the language of Generalized Linear Models (GLMs), it is linear because the link function (logit) maps the parameter space linearly: $\text{logit}(p) = w^T x + b$.
  
  ---
## 2. The Loss: Binary Cross-Entropy (Slide 4)
  
  Slide 4 defines the canonical loss function for binary classification:
  
  $$\mathbf{\text{"Charge each point } -\log \text{ of the probability the model gave to the truth."}}$$
  
  ```
                      Single-Sample Penalty Landscapes (Slide 4)
    Loss for One Point
      ▲
    5 │  ✖ Confident & Wrong (p̂ = 0.01 when y = 1): Loss = 4.61!
      │  \
    4 │   \                                             / True y = 0: -log(1 - p̂)
      │    \                                           /
    3 │     \   True y = 1: -log(p̂)                   /
      │      \                                       /
    2 │       \                                     /
      │        \                                   /
    1 │         \                                 /
      │          \                               /
    0 ┼───────────\─────────────────────────────●────────▶ Predicted Probability p̂
     0.0          0.2          0.4   0.5   0.6  0.8 1.0   Confident & Right: Loss ≈ 0.01
  ```
  
  Slide 4 provides the full empirical risk formulation:
  
  $$\mathbf{J(w) = -\frac{1}{n} \sum_{i=1}^n \Big[ y_i \log \hat{p}_i + (1 - y_i) \log(1 - \hat{p}_i) \Big]}$$
  
  where $\hat{p}_i = \sigma(w^T x_i + b) = P(Y = 1 \mid x_i)$.
  
  ---
### A. The Case-Switch Mechanism
  Slide 4 notes: *"Only one of the two terms is ever switched on."*
  
  Because binary ground-truth labels are discrete ($y_i \in \{0, 1\}$), the loss function acts as an algebraic conditional switch:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Case 1: When True Label y_i = 1:                                       │
  │   Loss(1, p̂_i) = -[ 1 · log(p̂_i) + (1 - 1) · log(1 - p̂_i) ]            │
  │                = -log(p̂_i)                                             │
  │                                                                        │
  │ • If p̂_i ──▶ 1.0 (Confident & Right) : Loss = -log(1.0) = 0.0          │
  │ • If p̂_i ──▶ 0.0 (Confident & Wrong) : Loss = -log(0.0) ──▶ +∞ (FATAL!)│
  ├────────────────────────────────────────────────────────────────────────┤
  │ Case 2: When True Label y_i = 0:                                       │
  │   Loss(0, p̂_i) = -[ 0 · log(p̂_i) + (1 - 0) · log(1 - p̂_i) ]            │
  │                = -log(1 - p̂_i)                                         │
  │                                                                        │
  │ • If p̂_i ──▶ 0.0 (Confident & Right) : Loss = -log(1.0) = 0.0          │
  │ • If p̂_i ──▶ 1.0 (Confident & Wrong) : Loss = -log(0.0) ──▶ +∞ (FATAL!)│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  The penalty is asymmetric and asymptotically infinite: as a prediction deviates toward the incorrect class with high confidence ($\hat{p} \to 0$ for $y=1$), the penalty grows without bound, forcing the optimizer to prioritize resolving confident errors.
  
  ---
### B. Statistical Derivation: Maximum Likelihood of a Bernoulli Process
  Slide 4 states:
  $$\mathbf{\text{"It is the negative log-likelihood of a coin flip (Bernoulli) with probability } \hat{p}\text{: maximum likelihood."}}$$
  
  Let the target variable $Y_i \in \{0, 1\}$ be an independent Bernoulli random variable parameterized by success probability $p_i = P(Y_i = 1 \mid x_i)$. The probability mass function for a single observation is written compactly as:
  
  $$P(Y_i = y_i \mid x_i; w) = \hat{p}_i^{y_i} (1 - \hat{p}_i)^{1 - y_i}$$
  
  Taking the joint likelihood over an independent dataset of $n$ observations:
  
  $$L(w) = \prod_{i=1}^n P(Y_i = y_i \mid x_i; w) = \prod_{i=1}^n \hat{p}_i^{y_i} (1 - \hat{p}_i)^{1 - y_i}$$
  
  To find the Maximum Likelihood Estimate (MLE), evaluate the **Negative Log-Likelihood (NLL)**:
  
  $$\text{NLL}(w) = -\ln L(w) = -\sum_{i=1}^n \ln \left( \hat{p}_i^{y_i} (1 - \hat{p}_i)^{1 - y_i} \right)$$
  $$\text{NLL}(w) = -\sum_{i=1}^n \Big[ y_i \ln(\hat{p}_i) + (1 - y_i) \ln(1 - \hat{p}_i) \Big]$$
  
  Dividing by sample size $n$ recovers the empirical **Binary Cross-Entropy Loss** exactly:
  
  $$\mathbf{\arg\max_w L(w) \equiv \arg\min_w J(w)}$$
  
  $$\mathbf{\text{Minimizing Binary Cross-Entropy is the Maximum Likelihood Estimator for binary classification.}}$$
  
  ---
## 3. Why Not Just Use Squared Error? (Slides 5–6)
  
  Slide 5 poses the diagnostic question:
  
  $$\mathbf{\text{"We could minimise } \sum (y_i - \hat{p}_i)^2 \text{ with } \hat{p} = \sigma(w^T x + b)\text{. Why does nobody do that?"}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Vote Options (Slide 5):                                                │
  │                                                                        │
  │ A. No reason. It works just as well; cross-entropy is tradition.       │
  │ B. The loss is no longer convex in w: many valleys, GD can get stuck.  │
  │ C. When the model is confidently wrong, the gradient almost vanishes.  │
  │ D. Both B and C.  <-- THE VERIFIED ANSWER (Slide 6)                    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 6 delivers the verdict:
  
  $$\mathbf{\text{"D. At } z = -6 \text{ the push toward the truth is } 0.998 \text{ with cross-entropy and } 0.005 \text{ with squared error:}}$$
  $$\mathbf{\text{the worst mistakes get the weakest correction, and the loss is non-convex in } w\text{."}}$$
  
  ---
## 4. "Squared Error Goes Silent When It Matters Most" (Slide 6)
  
  Slide 6 contrasts the mathematical behavior of Cross-Entropy vs. Squared Error when evaluating a misclassified positive instance ($y = 1$):
  
  ```
                   The Diagnostic Comparison for True y = 1 (Slide 6)
      Loss Surface: MSE Plateaus at 1.0               Gradient Magnitude: MSE Goes Silent
    Loss                                          |Gradient| w.r.t. z
      ▲                                             ▲
    6 │  \  Binary Cross-Entropy                    1.2 │  ──● Cross-Entropy: |p̂ - y| ──▶ 1.0
      │   \ (Grows to infinity)                     1.0 │    │
    4 │    \                                        0.8 │    │
      │     \                                       0.6 │    │
    2 │      \                                      0.4 │    │   Squared Error:
      │       \                                     0.2 │    │   |2(p̂ - y)p̂(1 - p̂)|
    1 ┼───────-╰─── Squared Error flattens at 1.0   0.0 ┼────●────────────────────────▶ Score z
      │                                                -8   -6   -4   -2   0   2   4
      └──────────────────────────────▶ Score z              ▲
     -8   -6   -4   -2   0   2   4                         At z = -6:
      [Confidently Wrong Basin: z << 0]                     • BCE Push = 0.998 (Maximum Force!)
                                                            • MSE Push = 0.005 (SILENT / STALLED!)
  ```
  
  ---
### Mathematical Proof: Deriving the Gradients with Respect to Logit $z$
  
  Let target $y = 1$, and let $z = w^T x + b$ such that $\hat{p} = \sigma(z)$. Recall the sigmoid derivative identity:
  $$\sigma'(z) = \sigma(z)(1 - \sigma(z)) = \hat{p}(1 - \hat{p})$$
  
  ---
#### 1. The Binary Cross-Entropy Gradient
  $$\mathcal{L}_{\text{BCE}}(z) = -\ln \sigma(z)$$
  
  Differentiating with respect to $z$ via the chain rule:
  
  $$\frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z} = -\frac{1}{\sigma(z)} \cdot \sigma'(z) = -\frac{1}{\sigma(z)} \cdot \sigma(z)(1 - \sigma(z)) = -(1 - \sigma(z)) = \sigma(z) - 1$$
  
  For general $y \in \{0, 1\}$:
  
  $$\mathbf{\frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z} = \hat{p} - y}$$
  
  Now, evaluate the gradient when the model is **confidently wrong** ($y = 1$, but $z = -6 \implies \hat{p} = \sigma(-6) \approx 0.00247$):
  
  $$\left| \frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z} \right| = |\hat{p} - y| = |0.00247 - 1| = \mathbf{0.99753 \approx 0.998}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Cross-Entropy Property:                                            │
  │ When the model is catastrophically wrong (z ──▶ -∞), the gradient      │
  │ magnitude reaches its MAXIMUM theoretical limit:                       │
  │                                                                        │
  │                lim_{z ──▶ -∞} |∂ℒ_BCE / ∂z| = |0 - 1| = 1.0            │
  │                                                                        │
  │ The optimization engine receives an aggressive, linear restoring force │
  │ that pulls parameters out of error space.                              │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
#### 2. The Mean Squared Error Gradient
  $$\mathcal{L}_{\text{MSE}}(z) = (y - \sigma(z))^2 = (1 - \sigma(z))^2$$
  
  Differentiating with respect to $z$ via the chain rule:
  
  $$\frac{\partial \mathcal{L}_{\text{MSE}}}{\partial z} = 2(\sigma(z) - y) \cdot \sigma'(z) = \mathbf{2(\hat{p} - y) \cdot \hat{p}(1 - \hat{p})}$$
  
  Now, evaluate the gradient under the identical failure condition ($y = 1$, $z = -6 \implies \hat{p} \approx 0.00247$):
  
  $$\left| \frac{\partial \mathcal{L}_{\text{MSE}}}{\partial z} \right| = 2 |0.00247 - 1| \cdot (0.00247) \cdot (1 - 0.00247)$$
  $$= 2(0.99753) \cdot (0.00247) \cdot (0.99753) \approx \mathbf{0.0049 \approx 0.005}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Squared Error Failure (Gradient Vanishing):                        │
  │ When the model is catastrophically wrong (z ──▶ -∞), the gradient      │
  │ goes SILENT:                                                           │
  │                                                                        │
  │             lim_{z ──▶ -∞} |∂ℒ_MSE / ∂z| = 2|-1| · (0)(1) = 0.0        │
  │                                                                        │
  │ • The derivative contains the term p̂(1 - p̂).                          │
  │ • As the model becomes confident (p̂ ──▶ 0 or p̂ ──▶ 1), p̂(1 - p̂) ──▶ 0.│
  │ • The gradient vanishes entirely!                                      │
  │ • The parameter update Δw = -η ∇J collapses to near-zero.              │
  │ • The worst mistakes receive the weakest corrective update, causing    │
  │   gradient descent to stall indefinitely on flat plateaus.             │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. Mathematical Proof of Convexity vs. Non-Convexity (The Graduate Fill-In)
  
  Slide 5 cites Reason B: *"The loss is no longer convex in $w$: many valleys, GD can get stuck."*
  
  To prove this formally, we examine the **Hessian matrix ($\nabla_w^2 J$)** of both loss formulations.
### A. Convexity Proof for Binary Cross-Entropy
  Recall the parameter gradient of BCE:
  
  $$\nabla_w J_{\text{BCE}}(w) = \frac{1}{n} \sum_{i=1}^n (\hat{p}_i - y_i) x_i = \frac{1}{n} X^T (\hat{p} - y)$$
  
  Differentiating a second time with respect to $w$:
  
  $$\nabla_w^2 J_{\text{BCE}}(w) = \frac{1}{n} X^T \left( \nabla_w \hat{p} \right) = \frac{1}{n} X^T \left( \text{diag}\left( \frac{\partial \hat{p}_i}{\partial z_i} \right) X \right)$$
  
  Using $\frac{\partial \hat{p}_i}{\partial z_i} = \hat{p}_i (1 - \hat{p}_i)$:
  
  $$\mathbf{H_{\text{BCE}} = \nabla_w^2 J_{\text{BCE}}(w) = \frac{1}{n} X^T D X}$$
  
  where $D \in \mathbb{R}^{n \times n}$ is a diagonal weighting matrix with entries:
  
  $$D_{ii} = \hat{p}_i (1 - \hat{p}_i)$$
  
  * Because the sigmoid output is strictly bounded ($\hat{p}_i \in (0, 1)$), the variance product is strictly positive:
  $$D_{ii} = \hat{p}_i (1 - \hat{p}_i) > 0 \quad \forall \; i \in \{1, \dots, n\}$$
  * Evaluating the quadratic form for any arbitrary non-zero vector $v \in \mathbb{R}^{d+1}$:
  $$v^T H_{\text{BCE}} v = \frac{1}{n} v^T (X^T D X) v = \frac{1}{n} (Xv)^T D (Xv) = \frac{1}{n} \sum_{i=1}^n D_{ii} (Xv)_i^2 \ge 0$$
  * **Conclusion:** The Hessian $H_{\text{BCE}}$ is **positive semi-definite everywhere** ($H \succeq 0$). The loss surface is strictly convex; local minima do not exist, and gradient descent is guaranteed to converge to the global minimum.
  
  ---
### B. Non-Convexity Proof for Squared Error
  For Mean Squared Error, the second derivative of the loss with respect to $z$ is:
  
  $$\frac{\partial^2 \mathcal{L}_{\text{MSE}}}{\partial z^2} = \frac{\partial}{\partial z} \Big[ 2(\sigma(z) - y) \sigma(z)(1 - \sigma(z)) \Big]$$
  
  Applying the product rule introduces the second derivative of the sigmoid ($\sigma''(z) = \sigma(z)(1 - \sigma(z))(1 - 2\sigma(z))$):
  
  $$\frac{\partial^2 \mathcal{L}_{\text{MSE}}}{\partial z^2} = 2 \sigma'(z)^2 + 2(\sigma(z) - y) \sigma''(z)$$
  
  * When $y = 1$ and $z$ is negative, the second term $2(\sigma(z) - 1)\sigma''(z)$ becomes negative and overpowers the first term, causing:
  $$\frac{\partial^2 \mathcal{L}_{\text{MSE}}}{\partial z^2} < 0$$
  * **Conclusion:** The Hessian possesses negative eigenvalues in regions where the model is incorrect. The loss surface is **non-convex**, riddled with inflection points, saddle surfaces, and non-optimal local minima where gradient descent stalls.
  
  ---
## Summary Review Questions for Section 2
  
  1. *Using the derivative identity $\sigma'(z) = \sigma(z)(1 - \sigma(z))$, prove that the derivative of Binary Cross-Entropy loss with respect to the logit $z$ is $\frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z} = \hat{p} - y$.*
  2. *Why does the Mean Squared Error loss curve plateau at a constant value of $1.0$ when the model is confidently wrong, and why does this plateau cause gradient descent updates to stall?*
  3. *Show that the Hessian matrix of Binary Cross-Entropy can be written as $H = \frac{1}{n} X^T D X$, and explain why this guarantees that the loss surface has no non-optimal local minima.*
  
  ---