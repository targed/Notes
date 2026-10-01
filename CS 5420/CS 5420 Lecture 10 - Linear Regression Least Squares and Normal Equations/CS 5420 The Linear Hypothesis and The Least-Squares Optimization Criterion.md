## 1. The Warm-Up: What Does "Best" Mean? (Slides 2–3)
  
  Slide 2 presents an intuitive diagnostic problem:
  
  $$\mathbf{\text{"Eight students: hours studied vs. exam score. Which line would you ship? Defend it."}}$$
  
  ```
                     Candidate Regression Lines (Slide 2)
  Exam Score (y)
    ▲
  90 ─┼───────────────────────────●─────── Line A: ŷ = 46 + 5.6x
    │                       ●     ─── Line B: ŷ = 50 + 4.8x
  80 ─┼─────────────────●─●───────- - - - Line C: ŷ = 56 + 3.8x
    │             ●
  70 ─┼───────●───●
    │   ●
  60 ─┼─●
    │
  50 ─┴───┬───┬───┬───┬───┬───┬───┬───┬───▶ Hours Studied (x)
    0   1   2   3   4   5   6   7   8
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Line A:  ŷ = 46 + 5.6x                                                 │
  │ Line B:  ŷ = 50 + 4.8x                                                 │
  │ Line C:  ŷ = 56 + 3.8x                                                 │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Question: How do you mathematically decide which line is optimal?      │
  │ You cannot choose a model without first defining a formal, scalar      │
  │ objective function (a loss rule).                                      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Defining the Residual Vector
  For any candidate model parameterization, the prediction error on the $i$-th observation is quantified by its **residual** ($r_i$):
  
  $$r_i = y_i - \hat{y}_i = y_i - f(x_i)$$
  
  * Graphically, $r_i$ represents the **signed vertical distance** between the true observed point $(x_i, y_i)$ and the candidate regression line $\hat{y}_i$.
  * A positive residual ($r_i > 0$) indicates an **underestimate** ($y_i > \hat{y}_i$).
  * A negative residual ($r_i < 0$) indicates an **overestimate** ($y_i < \hat{y}_i$).
  
  ---
### The Least-Squares Decision Criterion (Slide 3)
  Slide 3 reveals the mathematical rule that defines the optimal line:
  
  $$\mathbf{\text{"Least squares is the line that makes the squared red bars add up to the smallest possible number."}}$$
  
  $$J(w) = \sum_{i=1}^N r_i^2 = \sum_{i=1}^N (y_i - \hat{y}_i)^2$$
  
  ```
            Comparison of Sum of Squared Residuals J(w) (Slide 3)
  ┌──────────────────────────────────────┬─────────────────────────────────┐
  │ Candidate Model                      │ Sum of Squared Residuals J(w)   │
  ├──────────────────────────────────────┼─────────────────────────────────┤
  │ Line A:  ŷ = 46 + 5.6x               │ 37                              │
  │ Line B:  ŷ = 50 + 4.8x               │ 45                              │
  │ Line C:  ŷ = 56 + 3.8x               │ 181                             │
  ├──────────────────────────────────────┼─────────────────────────────────┤
  │ Optimal OLS Line: ŷ = 46.5 + 5.17x   │ 12  <-- NO LINE CAN BEAT IT!    │
  └──────────────────────────────────────┴─────────────────────────────────┘
  ```
  
  Slide 3 delivers an essential philosophical takeaway for applied machine learning:
  $$\mathbf{\text{"Least squares is a CHOICE of rule, not a law of nature. Change the rule and you get a different 'best' line."}}$$
  
  If we switch our loss function from squared error ($\sum r_i^2$) to absolute error ($\sum |r_i|$) or maximum absolute error ($\max |r_i|$), the mathematical definition of "best" shifts, yielding entirely different optimal slopes and intercepts.
  
  ---
## 2. Matrix Representation of the Linear Model (Slide 4)
  
  Slide 4 formalizes the multidimensional linear regression hypothesis:
  
  $$\hat{y} = w_0 + w_1 x_1 + w_2 x_2 + \dots + w_d x_d$$
  
  ```
                           Matrix Formulation (Slide 4)
                  Design Matrix X          Weight Vector w      Predictions ŷ
               ┌─────────────────────┐         ┌──────┐            ┌──────┐
     Sample 1  │  1   x_11 ... x_1d  │         │ w_0  │            │ ŷ_1  │
     Sample 2  │  1   x_21 ... x_2d  │         │ w_1  │            │ ŷ_2  │
        :      │  :    :        :    │    •    │  :   │     =      │  :   │
     Sample n  │  1   x_n1 ... x_nd  │         │ w_d  │            │ ŷ_n  │
               └─────────────────────┘         └──────┘            └──────┘
                 n × (d + 1)                 (d + 1) × 1            n × 1
  ```
  
  $$\mathbf{\hat{y} = Xw}$$
  
  ---
### The Homogeneous Coordinate Trick (Absorbing the Intercept)
  Slide 4 notes: *"A column of 1s in $X$ absorbs the intercept, so $w_0$ needs no special treatment."*
  
  * **The Problem:** The intercept parameter $w_0$ is an affine constant (it does not multiply any feature in $x \in \mathbb{R}^d$). Handling it separately requires carrying an explicit bias term $b \mathbf{1}_n$ through every matrix derivative.
  * **The Algebraic Solution:** Augment the feature vector with a dummy constant coordinate:
  $$\tilde{x} = [1, \; x_1, \; x_2, \; \dots, \; x_d]^T \in \mathbb{R}^{d+1}$$
  $$\tilde{w} = [w_0, \; w_1, \; w_2, \; \dots, \; w_d]^T \in \mathbb{R}^{d+1}$$
  * The affine scalar equation collapses cleanly into a single inner product:
  $$\hat{y}_i = \tilde{x}_i^T \tilde{w} = w_0(1) + \sum_{j=1}^d w_j x_{ij}$$
  * Stacking all $n$ augmented row vectors produces the **Augmented Design Matrix** $X \in \mathbb{R}^{n \times (d+1)}$.
  
  ---
## 3. "Linear in the Parameters, Not in the Features" (Slides 4–5)
  
  Slide 4 introduces a foundational definition:
  
  $$\mathbf{\text{"Linear in the PARAMETERS } w\text{, not necessarily in the features."}}$$
  
  Slide 5 contrasts two models evaluated on non-linear data:
  
  ```
                  Linear vs. Polynomial Basis Expansions (Slide 5)
         Raw Linear Features: [x]                 Polynomial Basis Expansion: [x, x²]
    y                                          y
   12 ─┼                       ●              12 ─┼                       ●
       │                   ●                      │                   ●
    8 ─┼               ●                       8 ─┼               ●
       │           ●                               │           ●
    4 ─┼───────●──────────────────             4 ─┼───●───────────────●───
       │   ●                                      │      ●         ●
    0 ─┴───●───────────────────────▶ x         0 ─┴────────●───●──────────▶ x
      -2   0   2                                 -2   0   2
      • Straight line: Underfits                 • Curved fit: Captures non-linear manifold
      • Model: ŷ = w₀ + w₁x                      • Model: ŷ = w₀ + w₁x + w₂x²
      • STILL LINEAR REGRESSION!                 • STILL LINEAR REGRESSION!
  ```
  
  ---
### The Mathematical Definition of Linearity in Machine Learning
  A model $f(x; w)$ is defined as a **linear model** if and only if it is an affine linear function of its **parameter vector $w$**:
  
  $$\frac{\partial f(x; w)}{\partial w_j} = \phi_j(x) \quad (\text{Independent of the parameter vector } w)$$
  
  The input features $x$ can undergo arbitrary non-linear transformations:
  * **Polynomial Expansions:** $\phi(x) = [1, x, x^2, x^3]$
  * **Logarithmic / Power Warps:** $\phi(x) = [1, \ln(x), \sqrt{x}]$
  * **Trigonometric / Fourier Series:** $\phi(x) = [1, \sin(x), \cos(x)]$
  * **Splines & Radial Basis Kernels:** $\phi(x) = [1, \exp(-\gamma \|x - c_1\|^2), \dots]$
  
  ```python
  # Format of y_hat = Xw for both models:
  # 1. Simple Linear Regression:
  X_simple = np.c_[np.ones(n), x]             # Shape: (n, 2)
  w_simple = [w_0, w_1]                       # Shape: (2, 1)
  
  # 2. Quadratic Polynomial Regression:
  X_poly = np.c_[np.ones(n), x, x**2]        # Shape: (n, 3)
  w_poly = [w_0, w_1, w_2]                    # Shape: (3, 1)
  
  # In BOTH cases: y_hat = X @ w
  ```
  
  **The Core Insight:** As long as the parameters $w$ enter the equation via multiplication without nesting ($\hat{y} = Xw$), the loss surface remains a strictly convex quadratic bowl. We can solve non-linear curves using the exact same closed-form linear algebra machinery.
  
  ---
## 4. What Fitting Actually Decides: Parameter Semantics (Slide 6)
  
  Slide 6 breaks down the physical meaning of the fitted parameters:
  
  $$\mathbf{\text{"Two numbers describe the line. Fitting = choosing those two numbers."}}$$
  
  ```
                       The Fitted Parameters (Slide 6)
  Exam Score (y)
      ▲                                         /
   90 ─┼───────────────────────────────────────●
       │                                     ●
   80 ─┼───────────────────────────────●───●
       │                           ●
   70 ─┼───────────────────●───●                 w₁ = 5.17 (Slope: Marginal Gain)
       │               ●                         "Score gained per extra study hour"
   60 ─┼───────●───●
       │   ●
   50 ─┼─●
  46.5 ┼◀── w₀ = 46.5 (Intercept: Baseline)
       │   "Predicted score at 0 study hours"
   40 ─┴───┬───┬───┬───┬───┬───┬───┬───┬───▶ Hours Studied (x)
       0   1   2   3   4   5   6   7   8
  ```
  
  * **The Intercept ($w_0 = 46.5$):**
  * The baseline value of $\hat{y}$ when all input features are zero ($x = 0$).
  * A student who studies zero hours is predicted to score $46.5$ points.
  * **The Slope / Coefficient ($w_1 = 5.17$):**
  * The marginal rate of change: $\frac{\partial \hat{y}}{\partial x} = w_1$.
  * For each additional hour of study, the predicted exam score increases by exactly $5.17$ points.
  * **Extension to $d > 1$ Dimensions:**
  * For multiple features ($x \in \mathbb{R}^d$), fitting means selecting a single intercept $w_0$ and $d$ partial slopes $w_1, \dots, w_d$, where each $w_j$ represents the marginal change in $\hat{y}$ per unit change in $x_j$ **holding all other features constant** (ceteris paribus).
  
  ---
## Summary Review Questions for Section 1
  
  1. *In Ordinary Least Squares, what is a residual $r_i$, and how does it differ from the statistical error term $\epsilon_i$?*
  2. *Why is a polynomial regression model $\hat{y} = w_0 + w_1 x + w_2 x^2 + w_3 x^3$ classified mathematically as a "linear model," even though its predictions form a cubic curve?*
  3. *How does appending a column of ones ($\mathbf{1}$) to the design matrix $X$ simplify the algebraic derivation of linear regression?*
  
  ---