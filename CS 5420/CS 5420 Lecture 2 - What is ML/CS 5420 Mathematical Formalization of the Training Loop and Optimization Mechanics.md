## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Why $A^*$ Pathfinding is AI, but Not ML:**
  * $A^*$ evaluates an exact, hand-coded heuristic function $f(n) = g(n) + h(n)$ to traverse a graph. Its performance does not change through repeated trials on the same graph topology. 
  * It lacks parameter adaptation from empirical data. It satisfies the definition of Artificial Intelligence (heuristic goal-directed search), but fails Mitchell's operational definition of Machine Learning (no experience $E$ modifies a policy or hypothesis space to improve performance measure $P$ on task $T$).
  2. **Trade-offs of Learned Representations vs. Hand-Crafted Features:**
  * **Learned Representations ($\phi(x; \theta)$):** Highly expressive, capable of capturing non-linear cross-feature interactions directly from unstructured high-dimensional inputs. However, they introduce non-convex loss surfaces filled with saddle points, require high sample complexity ($N \gg d$) to prevent memorization, and demand significant accelerator hardware.
  * **Hand-Crafted Features ($\phi(x)$):** Injects domain physics and human inductive bias directly into the feature space. This allows simple convex estimators (e.g., linear models, SVMs) to converge with small sample sizes ($N$), but creates a hard performance ceiling determined by human feature engineering.
  3. **The Ill-Posed Nature of Unsupervised Evaluation:**
  * In supervised learning, there is a ground-truth oracle $y$ that provides a clear metric (e.g., Cross-Entropy, Mean Squared Error, ROC-AUC).
  * In unsupervised learning, there is no target label. Clustering metrics (e.g., Silhouette score, Davies-Bouldin index) or density metrics (e.g., log-likelihood) measure proxy mathematical geometric properties, not semantic correctness. A clustering algorithm grouping images by dominant background color is mathematically valid, even if human intent sought object semantic categories.
  
  ---
## 2. Linear Algebraic Data Representation (Slide 7)
  
  Slide 7 introduces the standard mathematical conventions for tabular and multidimensional data matrices:
  
  ```
                            Data Matrix Representation
                            d Features (Columns)
                         x_1     x_2     x_3    ...    x_d         Target (y)
                    ┌──────────────────────────────────────┐     ┌───────────┐
     Instance 1  ──▶│  x_11    x_12    x_13   ...   x_1d   │     │    y_1    │
     Instance 2  ──▶│  x_21    x_22    x_23   ...   x_2d   │     │    y_2    │
  N Samples          │   :       :       :             :    │ ──▶ │     :     │
  (Rows)             │   :       :       :             :    │     │     :     │
     Instance N  ──▶│  x_N1    x_N2    x_N3   ...   x_Nd   │     │    y_N    │
                    └──────────────────────────────────────┘     └───────────┘
                               Design Matrix X                       Vector y
                               (N × d)                              (N × 1)
  ```
### A. The Feature Vector ($x$)
  * An individual instance is represented as a column vector in a $d$-dimensional real coordinate space:
  $$x = \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_d \end{bmatrix} \in \mathbb{R}^d \quad \implies \quad x^T = [x_1, x_2, \dots, x_d] \in \mathbb{R}^{1 \times d}$$
  * **Component Semantics:** Each scalar $x_j$ represents a continuous measurement, an ordinal rank, or a binary indicator variable.
### B. The Design Matrix ($X$)
  * Stacking $N$ observed instances yields the design matrix $X \in \mathbb{R}^{N \times d}$:
  $$X = \begin{bmatrix} x^{(1)T} \\ x^{(2)T} \\ \vdots \\ x^{(N)T} \end{bmatrix} = \begin{bmatrix} x_{11} & x_{12} & \cdots & x_{1d} \\ x_{21} & x_{22} & \cdots & x_{2d} \\ \vdots & \vdots & \ddots & \vdots \\ x_{N1} & x_{N2} & \cdots & x_{Nd} \end{bmatrix}$$
  * **Memory Contiguity (Engineering Reality):**
  * In `NumPy` and `PyTorch`, $X$ is stored by default in **Row-Major order (`order='C'`)**, meaning values of instance $x^{(1)}$ are stored in contiguous memory addresses, followed immediately by $x^{(2)}$.
  * Vectorized matrix multiplication $X w$ takes advantage of CPU/GPU cache locality by loading consecutive features of a single sample into fast cache lines simultaneously.
### C. The Target Vector ($y$)
  * The true values to predict form an $N$-dimensional vector:
  $$y = [y_1, y_2, \dots, y_N]^T \in \mathbb{R}^N$$
  * For continuous regression: $y_i \in \mathbb{R}$.
  * For binary classification: $y_i \in \{0, 1\}$ or $\{-1, +1\}$.
  * For multiclass classification: $y_i \in \{1, 2, \dots, C\}$ or encoded as a one-hot matrix $Y \in \{0, 1\}^{N \times C}$.
  
  ---
## 3. Parameterization vs. Hyperparameters vs. Loss (Slide 8)
  
  Slide 8 establishes the formal vocabulary of model construction and optimization:
  
  $$\hat{y} = f_\theta(x)$$
  
  ```
                                 Vocabulary Taxonomy
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │ Parameters (θ):       Learned internally by the optimization algorithm via data       │
  │                       Examples: Weight matrix W, bias vector b, split points in trees  │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Hyperparameters (λ):  Pre-set externally by the ML engineer prior to training          │
  │                       Examples: Learning rate α, epochs E, batch size B, penalty λ_reg │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Loss Function ℒ(y, ŷ): Pointwise scalar metric measuring error on a single sample      │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ Cost Function J(θ):   The empirical expectation of the loss over all N training samples │
  └────────────────────────────────────────────────────────────────────────────────────────┘
  ```
### A. Pointwise Loss ($\mathcal{L}$) vs. Empirical Risk ($J(\theta)$)
  * **Pointwise Loss $\mathcal{L}(y_i, \hat{y}_i)$:** Measures discrepancy for an individual sample.
  * **Empirical Risk Objective $J(\theta)$:** The sample average across the dataset, often augmented with a regularization penalty $\Omega(\theta)$ to prevent overfitting:
  $$J(\theta) = \frac{1}{N} \sum_{i=1}^N \mathcal{L}\left(y_i, f_\theta(x_i)\right) + \lambda_{\text{reg}} \Omega(\theta)$$
### B. Mathematical Formulations of Canonical Loss Functions
#### 1. Mean Squared Error (Continuous Targets / Gaussian Noise Assumption):
  $$\mathcal{L}_{\text{MSE}}(y_i, \hat{y}_i) = \frac{1}{2} (y_i - \hat{y}_i)^2$$
  * **Derivative:** $\frac{\partial \mathcal{L}}{\partial \hat{y}_i} = - (y_i - \hat{y}_i) = (\hat{y}_i - y_i)$.
  * *Probabilistic Insight:* Minimizing MSE is mathematically equivalent to finding the Maximum Likelihood Estimate (MLE) under the assumption that the target contains additive zero-mean Gaussian noise: $y = f_\theta(x) + \epsilon$, where $\epsilon \sim \mathcal{N}(0, \sigma^2)$.
#### 2. Binary Cross-Entropy / Log Loss (Binary Classification / Bernoulli Assumption):
  $$\mathcal{L}_{\text{BCE}}(y_i, \hat{y}_i) = - \Big[ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) \Big], \quad \text{where } \hat{y}_i = \sigma(z_i) = \frac{1}{1 + e^{-z_i}}$$
  * **Derivative with respect to logit $z_i$:** 
  $$\frac{\partial \mathcal{L}_{\text{BCE}}}{\partial z_i} = \hat{y}_i - y_i$$
  * *Mathematical Grace:* Despite the logarithms and exponentials, the gradient of cross-entropy with respect to the raw logit is simply the linear prediction residual $(\hat{y} - y)$.
  
  ---
## 4. The Closed-Loop Optimization Mechanics (Slide 9)
  
  Slide 9 illustrates the iterative feedback loop that defines machine learning optimization:
  
  ```
                            The Training Pipeline
                      Hyperparameters (α, epochs, B)
                             [Set by Engineer]
                                     │
                                     ▼
        Features (x) ──────▶ [ Model f_θ(x) ] ──────▶ Prediction (ŷ)
                                     ▲                       │
                                     │                       │
                                 [Update]                    ▼
                              Parameters (θ) ◀────── [ Loss ℒ(y, ŷ) ] ◀────── Label (y)
                                                     [Minimization Engine]
  ```
### The Gradient Descent Update Step:
  Parameters are iteratively adjusted in parameter space along the vector direction of steepest descent on the loss surface:
  
  $$\theta^{(t+1)} = \theta^{(t)} - \alpha \nabla_\theta J(\theta^{(t)})$$
  
  Where:
  * $\theta^{(t)}$ is the parameter vector at iteration $t$.
  * $\alpha \in \mathbb{R}^+$ is the **learning rate** (hyperparameter).
  * $\nabla_\theta J(\theta)$ is the **gradient vector** containing all partial derivatives:
  $$\nabla_\theta J(\theta) = \left[ \frac{\partial J}{\partial \theta_1}, \frac{\partial J}{\partial \theta_2}, \dots, \frac{\partial J}{\partial \theta_p} \right]^T$$
### Computational Regimes of Gradient Descent:
  
  ```
  Full-Batch GD (N samples)           Mini-Batch SGD (B samples)          Stochastic GD (1 sample)
  ─────────────────────────           ──────────────────────────          ────────────────────────
  • Exact gradient ∇J(θ)              • Stochastically estimates ∇J(θ)    • High gradient variance
  • Expensive: O(N · d) per step      • Balanced: O(B · d) per step       • Noisy updates
  • Stable; smooth loss curve         • Vectorized for GPU parallelism    • Escapes local minima
  ```
  
  ---
## 5. Duality of Inquiry: Same Data, Different Objectives (Slide 13)
  
  Slide 13 displays a grid of 35 natural images containing apples, pears, tomatoes, cows, dogs, and horses:
  
  ```
                     The Same Observations (Pixels X)
                                     │
             ┌───────────────────────┴───────────────────────┐
             ▼                                               ▼
   Supervised Formulation                         Unsupervised Formulation
   Inputs paired with labels:                     No external labels provided:
   y ∈ {apple, pear, tomato, cow, dog, horse}     Target: Discover intrinsic structure
             │                                               │
             ▼                                               ▼
   Classification Objective:                      Clustering Objective:
   Learn conditional probability                  Partition space via pixel manifolds.
   distribution P(Y | X) to partition             Algorithm might group by color:
   semantic classes cleanly.                      Cluster 1 = {apples, tomatoes} (Red)
                                                  Cluster 2 = {pears} (Green)
                                                  Cluster 3 = {cows, dogs, horses} (Shapes)
  ```
  
  **Key Conceptual Insight:** Machine learning algorithms do not possess semantic common sense. A clustering algorithm given these images will cluster by low-level visual variance (e.g., color histograms, background greens, dominant spatial frequencies). It requires **supervised labels** to force the model to prioritize biological taxonomy over color geometry.
  
  ---
## 6. Analytical Dissection of Slide 14: Supervised, Unsupervised, or Neither?
  
  Slide 14 presents six real-world tasks to classify by learning paradigm:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────────────────────┐
  │ Scenario                                          Category        Justification        │
  ├────────────────────────────────────────────────────────────────────────────────────────┤
  │ 1. Predicting tomorrow's electricity demand from   SUPERVISED      Continuous label     │
  │    10 years of labeled history                    (Regression)    y ∈ ℝ^+ available.   │
  │                                                                                        │
  │ 2. Grouping 50,000 news articles into topics      UNSUPERVISED    No predefined ground │
  │    nobody has named                               (Clustering)    truth labels y.      │
  │                                                                                        │
  │ 3. Computing the average salary in a spreadsheet  NEITHER         Deterministic scalar │
  │                                                   (Arithmetic)    reduction (no ML).   │
  │                                                                                        │
  │ 4. Flagging fraudulent transactions from a        SUPERVISED      Categorical ground   │
  │    labeled fraud database                         (Classification)truth y ∈ {0, 1}.    │
  │                                                                                        │
  │ 5. Reducing 200 sensor readings to 3 dimensions   UNSUPERVISED    Manifold projection  │
  │    for a plot                                     (Dim Reduction) X ∈ ℝ^(N×200) ──▶ ℝ^3│
  │                                                                                        │
  │ 6. A thermostat that turns on below 68°F          NEITHER         Deterministic control│
  │                                                   (Rule Logic)    system (no training).│
  └────────────────────────────────────────────────────────────────────────────────────────┘
  ```
### In-Depth Mathematical Breakdown of Non-ML Edge Cases:
  
  * **Scenario 3 (Average Salary):**
  * Formula: $\bar{s} = \frac{1}{N}\sum_{i=1}^N s_i$.
  * *Why Neither:* There is no parameter vector $\theta$, no objective loss minimization, no capacity for generalization, and no predictive inference on unobserved instances. It is exact descriptive arithmetic.
  * **Scenario 6 (Thermostat):**
  * Control Law: $u(t) = \mathbf{1}_{\{T(t) < 68^\circ\text{F}\}}$.
  * *Why Neither:* This is a deterministic, memoryless piecewise step function. No statistical inference is occurring, no parameters are updated via data feedback, and the threshold is hardcoded by a human designer rather than learned from error minimization.
  
  ---
## Summary Review Questions for Section 2
  
  1. *Why does using Mean Squared Error (MSE) on a binary classification problem ($y \in \{0, 1\}$) with a sigmoid output $\sigma(z)$ lead to poor optimization compared to Binary Cross-Entropy?*
  2. *If a design matrix $X \in \mathbb{R}^{N \times d}$ has $d > N$, what happens algebraically to the Ordinary Least Squares normal equation $\hat{\beta} = (X^T X)^{-1} X^T y$?*
  3. *How does the gradient update step in Mini-Batch SGD approximate the true gradient of the full dataset, and why does this stochastic noise often help in non-convex optimization landscapes?*
  
  ---