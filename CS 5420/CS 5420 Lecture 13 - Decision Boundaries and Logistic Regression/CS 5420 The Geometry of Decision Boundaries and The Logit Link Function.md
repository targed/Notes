## 1. The Geometry of Linear Classification (Slide 2)
  
  Slide 2 introduces the fundamental geometric definition of linear classification:
  
  $$\mathbf{\text{"A Decision Boundary Is a Line in Feature Space."}}$$
  $$\mathbf{\text{"On one side the model says class 1, on the other class 0. Fitting a classifier means choosing where that line goes."}}$$
  
  ```
                    The Decision Boundary Geometry (Slide 2)
  Feature x₂
    ▲
  4 ┼             wᵀx + b = 0  (Decision Boundary)
    │          \
  2 ┼           \             ● (Class 1 / Malignant)
    │            \       ●       ●
  0 ┼             \   ●  ▲ w (Normal Vector)
    │    ○  ○      \     │
  -2 ┼  ○   ○   ○    \    │
    │    ○   ○       \   ●
  -4 ┼─────────────────\─────────────────────────▶ Feature x₁
   -4        -2        0        2        4
       ○ = Class 0 (Benign)
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Linear Partitioning Rules:                                         │
  │                                                                        │
  │ • z = wᵀx + b > 0  ──▶  Predict Class 1 (Positive Side)                │
  │ • z = wᵀx + b = 0  ──▶  The Decision Boundary Itself                   │
  │ • z = wᵀx + b < 0  ──▶  Predict Class 0 (Negative Side)                │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### A. Dimensionality of Decision Boundaries
  The decision surface separating two classes is always a subspace of dimension **$d - 1$** embedded within the ambient $d$-dimensional feature space $\mathbb{R}^d$:
  * **In 2-D Space ($d = 2$):** The boundary is a **1-D line** ($w_1 x_1 + w_2 x_2 + b = 0$).
  * **In 3-D Space ($d = 3$):** The boundary is a **2-D plane** ($w_1 x_1 + w_2 x_2 + w_3 x_3 + b = 0$).
  * **In $d$-D Space ($d > 3$):** The boundary is a **$(d - 1)$-dimensional affine hyperplane**.
  
  ---
### B. The Geometric Meaning of the Weight Vector ($w$) and Bias ($b$)
  Slide 2 notes: *"w points across the line, toward class 1."*
  
  1. **The Normal Vector ($w$):**
   * Let $x_A$ and $x_B$ be any two distinct points lying directly on the decision boundary:
     $$w^T x_A + b = 0 \quad \text{and} \quad w^T x_B + b = 0$$
   * Subtracting these two equations:
     $$w^T (x_A - x_B) = 0$$
   * Because the vector $(x_A - x_B)$ represents an arbitrary displacement vector parallel to the decision boundary, the weight vector $w$ is **strictly orthogonal (perpendicular / normal)** to the decision boundary.
   * Furthermore, $w$ points in the direction of the positive half-space, toward Class 1.
  2. **The Bias Parameter ($b$):**
   * Dictates the orthogonal translation of the hyperplane relative to the origin.
   * The perpendicular Euclidean distance from the origin to the decision boundary is:
     $$\text{Distance}(\mathbf{0}, \text{Hyperplane}) = \frac{|b|}{\|w\|_2}$$
   * If $b = 0$, the decision boundary passes directly through the origin ($\mathbf{0}$).
  
  ---
### C. Signed Euclidean Distance to the Boundary
  Slide 2 specifies: *"Distance from the line: $\frac{|z|}{\|w\|}$."*
  
  For an arbitrary point $x \in \mathbb{R}^d$, the scalar linear score $z = w^T x + b$ is not just an ungrounded number; it is directly proportional to the **signed geometric distance** ($d_{\perp}$) from the observation to the decision boundary:
  
  $$d_{\perp}(x) = \frac{w^T x + b}{\|w\|_2} = \frac{z}{\|w\|_2}$$
  
  * Points far into the Class 1 region have large positive distances ($d_{\perp} \gg 0$).
  * Points far into the Class 0 region have large negative distances ($d_{\perp} \ll 0$).
  * Points near the boundary have $d_{\perp} \approx 0$, reflecting high classification uncertainty.
  
  Slide 2 concludes:
  $$\mathbf{\text{"Model complexity = how flexible this boundary is allowed to be."}}$$
  
  Linear models constrain the boundary to be flat ($w^T x + b = 0$). Increasing model capacity (via polynomial expansions, kernel tricks, or deep neural networks) allows this boundary to bend, wrap, and form non-linear topological manifolds.
  
  ---
## 2. The Sigmoid Activation Function (Slide 3)
  
  Slide 3 presents the fundamental mechanism that converts unbounded regression scores into calibrated probabilities:
  
  $$\mathbf{\text{"Keep the linear score } z = w^T x + b\text{, then squash it into } (0, 1)\text{."}}$$
  
  $$\mathbf{\sigma(z) = \frac{1}{1 + e^{-z}} = \frac{e^z}{1 + e^z}}$$
  
  ```
                       The Logistic Sigmoid Function (Slide 3)
    Probability p = σ(z)
      ▲
  1.0 │                                             ╭────────────── σ(z) ──▶ 1.0 as z ──▶ +∞
      │                                         ╭───╯
  0.8 │                                     ╭───╯  ● σ(2) = 0.88
      │                                  ╭──╯
  0.6 │                                ╭─╯
  0.5 ┼──────────────────────────────●────────────────────────────────
      │                           ╭──╯   σ(0) = 0.50 (Boundary Level)
  0.4 │                         ╭─╯
      │                      ╭──╯
  0.2 │                  ╭───╯  ● σ(-2) = 0.12
      │             ╭────╯
  0.0 ┴─────────────┴────────────────────────────────────────────────▶ Score z = wᵀx + b
     -6    -4      -2       0       2       4       6
      Bounded in (0, 1) | Steep in the middle | Flat at the extremes
  ```
  
  ---
### Mathematical Properties of the Sigmoid:
  1. **Strictly Bounded Output:**
   $$\lim_{z \to -\infty} \sigma(z) = 0.0, \quad \lim_{z \to +\infty} \sigma(z) = 1.0 \implies \sigma(z) \in (0, 1)$$
   The function guarantees that outputs conform strictly to the axioms of probability ($P \in [0, 1]$).
  2. **Symmetry About the Origin:**
   $$1 - \sigma(z) = 1 - \frac{1}{1 + e^{-z}} = \frac{e^{-z}}{1 + e^{-z}} = \frac{1}{1 + e^z} = \mathbf{\sigma(-z)}$$
  3. **The Elegant Derivative Identity:**
   Evaluating the derivative of $\sigma(z)$ with respect to $z$:
   $$\sigma'(z) = \frac{d}{dz} (1 + e^{-z})^{-1} = -(1 + e^{-z})^{-2} (-e^{-z}) = \frac{e^{-z}}{(1 + e^{-z})^2}$$
   $$= \left(\frac{1}{1 + e^{-z}}\right) \left(\frac{e^{-z}}{1 + e^{-z}}\right) = \sigma(z) \Big(1 - \sigma(z)\Big)$$
  
  $$\mathbf{\sigma'(z) = \sigma(z)(1 - \sigma(z))}$$
  
  * This identity is central to optimization: the derivative of the activation function can be computed directly from its forward-pass output without evaluating additional transcendental exponentials.
  
  ---
## 3. The Triad of Representations: Probability, Odds, and Log-Odds (Slide 3)
  
  Slide 3 presents an equivalence table across three distinct mathematical representations:
  
  $$\mathbf{\text{"Three names for the same number."}}$$
  
  ```
  ┌───────────────────────┬──────────────────────────┬─────────────────────────┐
  │ Probability p         │ Odds: p / (1 - p)        │ Log-Odds / Logit: z     │
  │ [Bounded in (0, 1)]   │ [Bounded in (0, ∞)]      │ [Bounded in (-∞, +∞)]   │
  ├───────────────────────┼──────────────────────────┼─────────────────────────┤
  │ 0.12                  │ 0.14  (1 to 7 against)   │ -2                      │
  │ 0.50 (Uncertain)      │ 1.00  (1 to 1 / Even)    │  0 (Decision Threshold) │
  │ 0.88                  │ 7.39  (7.4 to 1 for)     │ +2                      │
  └───────────────────────┴──────────────────────────┴─────────────────────────┘
  ```
  
  ---
### Deconstructing the Three Formulations:
#### 1. Probability ($p$):
  The probability of observing the positive class ($Y = 1$):
  $$p = P(Y = 1 \mid x) = \sigma(z) = \frac{1}{1 + e^{-z}}$$
  * **Domain:** Strictly restricted to the unit interval $p \in (0, 1)$.
  * **Limitation:** Probability is bounded, meaning linear combinations ($w^T x + b$) cannot model $p$ directly without producing invalid predictions like $p = 1.4$ or $p = -0.3$.
#### 2. Odds ($\text{Odds}$):
  The ratio of the probability that the event occurs to the probability that it does not occur:
  $$\text{Odds} = \frac{p}{1 - p} = \frac{\frac{1}{1 + e^{-z}}}{\frac{e^{-z}}{1 + e^{-z}}} = \frac{1}{e^{-z}} = \mathbf{e^z}$$
  * **Domain:** Non-negative real numbers: $\text{Odds} \in (0, \infty)$.
  * **Interpretation:** If $p = 0.80$, $\text{Odds} = \frac{0.80}{0.20} = 4.0$ (the event is 4 times more likely to happen than not).
#### 3. Log-Odds / The Logit Link Function ($z$):
  The natural logarithm of the odds:
  $$\text{Logit}(p) = \ln(\text{Odds}) = \ln\left(\frac{p}{1 - p}\right) = \ln(e^z) = \mathbf{z}$$
  
  $$\mathbf{\ln\left(\frac{P(Y = 1 \mid x)}{1 - P(Y = 1 \mid x)}\right) = w^T x + b}$$
  
  ---
## 4. Why Logistic Regression Is a Linear Model (Slide 3)
  
  Slide 3 delivers the foundational conceptual definition:
  
  $$\mathbf{\text{"The model is linear in the LOG-ODDS. } \sigma \text{ only converts it back to a probability."}}$$
  $$\mathbf{\text{"Logistic regression = a linear model for the log-odds, followed by } \sigma\text{."}}$$
  $$\mathbf{\text{"S-shaped nonlinear curve, but decision Boundary is linear."}}$$
  
  ```
                         The Two Faces of Logistic Regression
         In Probability Space (S-Shaped Curve)             In Log-Odds Space (Strictly Linear)
    P(Y = 1 | x)                                      Logit z = ln(p / (1 - p))
      ▲                                                  ▲
  1.0 │                    ╭───────────               +4 │                     /  z = wᵀx + b
      │                ╭───╯                             │                   /
  0.5 ┼──────────────● (Boundary: z = 0)               0 ┼─────────────────●─ (Boundary: z = 0)
      │          ╭───╯                                   │               /
  0.0 ┴──────────┴─────────────────────▶ x            -4 │             /
      Non-linear S-curve squashed into (0, 1)            └────────────/────────────────▶ x
                                                         Linear hyper-plane in ℝᵈ
  ```
### The Architectural Resolution:
  1. **Linearity of the Latent Representation:**
   * Logistic regression belongs to the family of **Generalized Linear Models (GLMs)**.
   * It is linear because the **link function** applied to the target parameter is an affine linear function of the inputs:
     $$g(p) = \text{logit}(p) = w^T x + b$$
  2. **Linearity of the Decision Boundary:**
   * Classification predictions are determined by comparing predicted probability against a threshold $\tau = 0.50$:
     $$\hat{y} = 1 \iff P(Y = 1 \mid x) \ge 0.50$$
   * In probability space: $P \ge 0.50$.
   * In odds space: $\frac{p}{1-p} \ge 1.0$.
   * In log-odds space: $\ln(1.0) \ge 0 \implies \mathbf{w^T x + b \ge 0}$.
  * The decision boundary is the set of points where the model is maximally uncertain ($P = 0.50$), which occurs precisely when:
  $$w^T x + b = 0$$
  * Despite the non-linear sigmoid curve in probability space, the decision boundary in feature space is **strictly a flat, linear hyperplane**.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Prove that the weight vector $w$ in logistic regression is orthogonal to the decision boundary $w^T x + b = 0$.*
  2. *If an observation has a linear score of $z = -2.197$, calculate its odds ($\frac{p}{1-p}$) and its predicted probability ($p$).*
  3. *Why is Logistic Regression classified as a "linear classifier" even though its output probability function $\sigma(w^T x + b)$ is non-linear?*
  
  ---