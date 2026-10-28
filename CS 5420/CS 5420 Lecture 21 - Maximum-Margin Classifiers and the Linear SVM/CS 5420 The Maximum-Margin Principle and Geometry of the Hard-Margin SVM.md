## 1. The Separator Ambiguity Problem (Slides 2–3)
  
  Slide 2 presents a fundamental dilemma in binary classification:
  
  $$\mathbf{\text{"Three lines, same training error: A, B, and C classify all 40 training points correctly."}}$$
  $$\mathbf{\text{"Zero training error cannot pick the winner. Something else has to."}}$$
  
  ```
                      The Separator Ambiguity (Slide 2)
  Feature x₂
      ▲
      │           ■   ■ ■
      │        ■  ■ ■   ■   ■             Line A (Hugs Negative Class)
      │     ■   ■   ■   ■   ■          ─── Line C (Centered in Empty Space)
      │                               ─── Line B (Hugs Positive Class)
      │         ●   ●   ●
      │      ● ●  ●   ●
      │    ●  ●   ●
      │  ●  ●   ●
      └────────────────────────────────────────────────────────▶ Feature x₁
         ● = Class +1          ■ = Class -1
  ```
### Why Empirical Risk Minimization (ERM) Is Insufficient Here:
  * When a dataset is **linearly separable**, there are not just three lines that achieve zero classification error; there are **infinitely many hyperplanes** that satisfy:
  $$\hat{R}_n(w, b) = \frac{1}{n}\sum_{i=1}^n \mathbf{1}_{\{y_i \neq \text{sign}(w^T x_i + b)\}} = 0$$
  * Traditional optimization methods (like Rosenblatt's Perceptron) stop as soon as they find *any* separating hyperplane. If initialized near Class $+1$, the Perceptron terminates with Line B; if initialized near Class $-1$, it terminates with Line A.
  * **The Generalization Flaw:** Lines A and B pass extremely close to individual training samples. Even minor test-set noise or measurement jitter will cause out-of-sample points to cross the boundary, resulting in poor generalization.
  
  ---
### The Maximum-Margin Criterion (Slide 3)
  Slide 3 reveals the selection principle:
  
  $$\mathbf{\text{"The margin: the widest empty band around each line (picture a 'street'). Then test on new points."}}$$
  $$\mathbf{\text{"C has the widest margin and the fewest errors on new data. The SVM picks C on purpose."}}$$
  
  ```
                       The Widest Margin Criterion (Slide 3)
         Line A (Narrow Street)                        Line C (Maximal Street Margin)
  ─────────────────────────────────             - - - - - - - - - - - - - - - - - - - - - - - -
  ───────────────────────────────── Boundary    ─────────────────────────────────────────────── Boundary
  - - - - - - - - - - - - - - - - -             - - - - - - - - - - - - - - - - - - - - - - - -
  Margin is paper-thin. A tiny perturbation    Wide empty street. Maximizes geometric tolerance
  causes a test point to cross.                 to out-of-sample perturbations.
  ```
  
  The **Support Vector Machine (SVM)** introduces a geometric inductive bias: among all hyperplanes that achieve zero training error, select the unique hyperplane that maximizes the **distance to the closest training points of either class**.
  
  ---
## 2. Geometry of a Linear Classifier (Slide 4)
  
  Slide 4 formalizes the coordinate geometry of linear separators:
  
  $$\mathbf{\text{Decision Rule: } \quad \hat{y} = \text{sign}(w^T x + b) = \begin{cases} +1 & \text{if } w^T x + b \ge 0 \\ -1 & \text{if } w^T x + b < 0 \end{cases}}$$
  
  *(Note: SVM literature standardizes target labels as $y_i \in \{-1, +1\}$, rather than the $\{0, 1\}$ convention used in Logistic Regression).*
  
  ```
                       Hyperplane Coordinate Geometry (Slide 4)
  Feature x₂
      ▲                     wᵀx + b = 0  (Decision Boundary Hyperplane)
      │                  \
      │                   \             ● Query x (Class +1)
      │                    \          . '
      │                     \   w   . ' Perpendicular Distance:
      │                      \  ▲ . '   d = |wᵀx + b| / ||w||
      │                       \ │'
      │                        \
      │    ■                    \
      │  ■   ■ (Class -1)        \
      └───────────────────────────\─────────────────────────▶ Feature x₁
  ```
  
  Slide 4 presents two foundational geometric theorems:
  
  ---
### Theorem 1: $w$ is the Normal Vector to the Hyperplane
  Let $x_A$ and $x_B$ be any two points lying directly on the decision boundary:
  $$w^T x_A + b = 0 \quad \text{and} \quad w^T x_B + b = 0$$
  
  Subtracting these equations:
  $$(w^T x_A + b) - (w^T x_B + b) = 0 \implies \mathbf{w^T (x_A - x_B) = 0}$$
  
  * The displacement vector $(x_A - x_B)$ represents a vector lying parallel to the hyperplane.
  * Because the dot product is zero, the weight vector $w$ is **orthogonal (normal)** to every vector within the decision surface.
  * $w$ points directly into the positive half-space ($y = +1$), while the scalar bias $b$ controls the orthogonal translation of the plane relative to the coordinate origin.
  
  ---
### Theorem 2: Perpendicular Distance from a Point to a Hyperplane
  Slide 4 asks:
  $$\mathbf{\text{"Why } d = \frac{|w^T x + b|}{\|w\|_2}\text{? The shortest distance from a point to a plane is always perpendicular."}}$$
#### Proof:
  Let $x$ be an arbitrary point in feature space. Let $x_p$ be its orthogonal projection onto the hyperplane $w^T x_p + b = 0$.
  The vector $(x - x_p)$ is parallel to the normal vector $w$. We can express $x$ as:
  
  $$x = x_p + d_\perp \frac{w}{\|w\|_2}$$
  
  where $d_\perp$ is the signed perpendicular distance. Multiply both sides by $w^T$ and add $b$:
  
  $$w^T x + b = w^T \left( x_p + d_\perp \frac{w}{\|w\|_2} \right) + b$$
  $$w^T x + b = \underbrace{(w^T x_p + b)}_{= \, 0 \text{ (since } x_p \text{ on plane)}} + \, d_\perp \frac{w^T w}{\|w\|_2}$$
  
  Because $w^T w = \|w\|_2^2$:
  $$w^T x + b = d_\perp \frac{\|w\|_2^2}{\|w\|_2} = d_\perp \|w\|_2$$
  
  Solving for the absolute physical distance $d = |d_\perp|$:
  
  $$\mathbf{d = \frac{|w^T x + b|}{\|w\|_2}}$$
  
  ---
## 3. The Margin & Canonical Hyperplane Scaling (Slides 5–6)
  
  Slide 5 addresses an algebraic degrees-of-freedom redundancy:
  
  $$\mathbf{\text{"Fix the scale so the closest points score exactly } \pm 1\text{. Then the margin width is easy to read off."}}$$
  
  ```
                       Canonical Margin Geometry (Slides 5–6)
  Feature x₂
      ▲
      │           ■   ■ ■                  wᵀx + b = -1  (Negative Margin Boundary)
      │        ■  ■ ■   ■   ■
      │        ───────○───────             Support Vector (xₙ): yₙ(wᵀxₙ + b) = 1
      │              / \                   Margin Width: γ = 2 / ||w||
      │             /   \
      │            /     \                 wᵀx + b = 0   (Decision Boundary)
      │           /       \
      │       ───○─────────○──             Support Vectors (xₚ): yₚ(wᵀxₚ + b) = 1
      │         ●   ●   ●                  wᵀx + b = +1  (Positive Margin Boundary)
      │      ● ●  ●   ●
      └────────────────────────────────────────────────────────▶ Feature x₁
         ○ = Support Vectors (Circled Points)
  ```
  
  ---
### A. The Scaling Invariance Trap
  Notice that multiplying both $w$ and $b$ by any positive scalar $c > 0$ yields the exact same physical decision boundary:
  $$\{x \mid w^T x + b = 0\} \equiv \{x \mid (cw)^T x + (cb) = 0\}$$
  
  The distance formula is also unaffected:
  $$d = \frac{|c(w^T x + b)|}{\|cw\|_2} = \frac{c |w^T x + b|}{c \|w\|_2} = \frac{|w^T x + b|}{\|w\|_2}$$
  
  Because $(w, b)$ can be multiplied by an arbitrary scalar without changing the physical geometry, we can set the scale of $w$ and $b$ freely.
  
  ---
### B. The Canonical Hyperplane Convention
  We fix the scale by setting the **functional margin of the closest training points to exactly $1.0$**:
  
  $$\min_{i=1, \dots, n} |w^T x_i + b| \equiv 1.0$$
  
  * For the closest positive sample $x_+$:
  $$w^T x_+ + b = +1$$
  * For the closest negative sample $x_-$:
  $$w^T x_- + b = -1$$
  * Combining both cases using the label $y_i \in \{-1, +1\}$:
  $$\mathbf{y_i (w^T x_i + b) \ge 1 \quad \forall \; i = 1, \dots, n}$$
  
  ---
### C. Mathematical Derivation of Margin Width ($\gamma = \frac{2}{\|w\|}$)
  Slide 5 states:
  $$\mathbf{\text{"Margin width: } \text{margin} = \frac{2}{\|w\|_2}\text{"}}$$
#### Proof:
  Let $x_+$ be a support vector on the positive margin boundary ($w^T x_+ + b = +1$), and let $x_-$ be a support vector on the negative margin boundary ($w^T x_- + b = -1$).
  
  The total width of the margin ($\gamma$) is the perpendicular projection of the displacement vector $(x_+ - x_-)$ onto the unit normal vector $\frac{w}{\|w\|_2}$:
  
  $$\gamma = (x_+ - x_-)^T \left( \frac{w}{\|w\|_2} \right) = \frac{w^T x_+ - w^T x_-}{\|w\|_2}$$
  
  From the boundary definitions:
  $$w^T x_+ = 1 - b \quad \text{and} \quad w^T x_- = -1 - b$$
  
  Substitute these values:
  $$\gamma = \frac{(1 - b) - (-1 - b)}{\|w\|_2} = \frac{1 - b + 1 + b}{\|w\|_2} = \mathbf{\frac{2}{\|w\|_2}}$$
  
  Slide 5 emphasizes:
  $$\mathbf{\text{Wide Margin } \Longleftrightarrow \text{ Small } \|w\|_2}$$
  
  To make the empty street as wide as possible, the optimization algorithm must make the Euclidean norm of the weight vector $\|w\|_2$ as small as possible.
  
  ---
## 4. The Hard-Margin Optimization Problem (Slide 7)
  
  Slide 7 states the optimization problem mathematically:
  
  $$\mathbf{\text{"Objective: minimize } \frac{1}{2}\|w\|^2 \quad \text{Constraints: } y_i(w^T x_i + b) \ge 1 \text{ for every } i\text{"}}$$
  
  ```
                       The Hard-Margin Primal Formulation (Slide 7)
  ┌────────────────────────────────────────────────────────────────────────┐
  │                      minimize       ½ ||w||₂²                          │
  │                      w, b                                              │
  │                                                                        │
  │                      subject to     yᵢ(wᵀxᵢ + b) ≥ 1   ∀ i = 1, ..., n │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 7 notes:
  $$\mathbf{\text{"Why it is solvable: a convex quadratic program. One global optimum."}}$$
  
  1. **Why Minimize $\frac{1}{2}\|w\|^2$ instead of Maximizing $\frac{2}{\|w\|}$?**
   * Maximizing $\frac{2}{\|w\|}$ is mathematically equivalent to minimizing $\frac{\|w\|}{2}$.
   * Squaring the objective ($\frac{1}{2}\|w\|_2^2 = \frac{1}{2} w^T w$) transforms the problem into a smooth, strictly convex function that is differentiable everywhere.
   * The constraints $y_i(w^T x_i + b) \ge 1$ are a system of linear inequalities.
  2. **Convex Quadratic Programming (QP):**
   * A strictly convex quadratic objective subject to linear inequality constraints forms a **Quadratic Program**.
   * It possesses a **unique global minimum** with zero local minima. Standard interior-point solvers can compute the optimal parameters $(w^*, b^*)$ efficiently.
  
  ---
## 5. Support Vectors: Only Boundary Points Matter (Slides 6, 8)
  
  Slide 6 and Slide 8 establish a defining property of Support Vector Machines:
  
  $$\mathbf{\text{"Only the support vectors fix } w \text{ and } b\text{. Every other point could move without changing the classifier."}}$$
  
  ```
                     The Support Vector Invariance Experiment (Slide 8)
       All 40 Training Points                 Only the 3 Support Vectors           Other 37 Non-SVs Moved
  w = (-0.25, -0.84), b = -3.21              w = (-0.25, -0.84), b = -3.21      w = (-0.25, -0.84), b = -3.21
  ┌──────────────────────────────┐           ┌──────────────────────────────┐   ┌──────────────────────────────┐
  │        ■  ■  ■   ■           │           │                              │   │                 ■  ■   ■     │
  │          ───○───             │           │          ───○───             │   │          ───○───             │
  │        ────────────          │           │        ────────────          │   │        ────────────          │
  │          ───○───○───         │           │          ───○───○───         │   │          ───○───○───         │
  │        ●   ●   ●             │           │                              │   │       ●    ●                 │
  └──────────────────────────────┘           └──────────────────────────────┘   └──────────────────────────────┘
  All 40 points define problem.              Only 3 circled SVs retained.       37 non-SVs moved arbitrarily.
  w and b are IDENTICAL.                     w and b are IDENTICAL.             w and b are IDENTICAL.
  ```
  
  ---
### The Karush-Kuhn-Tucker (KKT) Complementary Slackness Proof
  To see why non-support vectors exert zero influence on the decision boundary, examine the **Lagrangian dual formulation** of the hard-margin problem:
  
  $$\mathcal{L}(w, b, \alpha) = \frac{1}{2}\|w\|_2^2 - \sum_{i=1}^n \alpha_i \Big[ y_i(w^T x_i + b) - 1 \Big]$$
  
  where $\alpha_i \ge 0$ are the Lagrange multipliers.
  
  By the **KKT Complementary Slackness Condition**, at the optimal solution $(w^*, b^*, \alpha^*)$, the product of each multiplier and its constraint must equal zero:
  
  $$\mathbf{\alpha_i^* \Big[ y_i(w^{*T} x_i + b^*) - 1 \Big] = 0 \quad \forall \; i = 1, \dots, n}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Complementary Slackness Deductions:                                │
  │                                                                        │
  │ Case 1: Point sits strictly OUTSIDE the margin (yᵢ(wᵀxᵢ + b) > 1):     │
  │   • The term in brackets is non-zero: [ yᵢ(wᵀxᵢ + b) - 1 ] > 0.        │
  │   • To satisfy the condition, the multiplier MUST be zero:             │
  │                               αᵢ* = 0.0                                │
  │                                                                        │
  │ Case 2: Point sits directly ON the margin boundary (yᵢ(wᵀxᵢ + b) = 1): │
  │   • The term in brackets is zero: [ yᵢ(wᵀxᵢ + b) - 1 ] = 0.            │
  │   • The multiplier can be strictly positive:                           │
  │                               αᵢ* > 0.0                                │
  │   • These points are the SUPPORT VECTORS.                              │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Differentiating the Lagrangian with respect to $w$ reveals that the optimal weight vector is an explicit linear combination of the training points:
  
  $$\nabla_w \mathcal{L} = w - \sum_{i=1}^n \alpha_i y_i x_i = \mathbf{0} \implies \mathbf{w^* = \sum_{i=1}^n \alpha_i^* y_i x_i}$$
  
  Because $\alpha_i^* = 0$ for every sample lying outside the margin, non-support vectors drop out of the summation:
  
  $$\mathbf{w^* = \sum_{i \in \text{Support Vectors}} \alpha_i^* y_i x_i}$$
  
  * **Practical Takeaway:** You could delete $95\%$ of your training data (all non-support vectors), refit the model on the remaining support vectors alone, and recover the **exact same boundary $(w^*, b^*)$ to machine precision**.
  
  ---
## Summary Review Questions for Section 1
  
  1. *In a linearly separable binary classification dataset, why does the Perceptron algorithm find an arbitrary separating line, while the Support Vector Machine is guaranteed to find a unique, optimal line?*
  2. *Prove that the width of the margin between the two canonical bounding planes $w^T x + b = +1$ and $w^T x + b = -1$ is given by $\gamma = \frac{2}{\|w\|_2}$.*
  3. *Using the KKT complementary slackness condition $\alpha_i^* [y_i(w^T x_i + b) - 1] = 0$, explain why moving a training point that sits deep inside the correct class region has zero impact on the fitted weights $w$ and bias $b$.*
  
  ---