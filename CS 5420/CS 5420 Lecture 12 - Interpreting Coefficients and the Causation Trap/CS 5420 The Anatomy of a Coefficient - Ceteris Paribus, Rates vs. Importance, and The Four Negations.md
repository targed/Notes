## 1. Reading a Coefficient Correctly: The *Ceteris Paribus* Condition (Slide 2)
  
  Slide 2 provides the strict, non-negotiable definition of a multiple linear regression parameter:
  
  $$\mathbf{w_j = \text{the predicted change in } y \text{ per one-unit increase in } x_j\text{, HOLDING ALL OTHER FEATURES FIXED.}}$$
  
  Slide 2 warns:
  $$\mathbf{\text{"That conditional clause is the entire content of the sentence — never drop it."}}$$
  
  ```
                     The Ceteris Paribus Geometry (Slide 2)
  Prediction y
    ▲
  50 ┼                                                          ●
    │                                                ●       ●
  40 ┼                                           ●         ●
    │                                 ●       ●   ┌───┐ wⱼ = +3.2 in y
  30 ┼                        ●     ●            ──┼───┤ (Slope along xⱼ axis)
    │               ●     ●                       └───┘
  20 ┼          ●       ●                      +1 unit of xⱼ
    │     ●
  12 ┼◀── Intercept w₀ = 12
    │    "Prediction when every x = 0"
  0 ┴─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────▶ Feature x_j
    0     1     2     3     4     5     6     7     8     9     10
  ```
  
  ---
### A. Mathematical Formalization via Partial Derivatives
  In multiple linear regression, the conditional expectation of $Y$ given feature vector $x \in \mathbb{R}^d$ is:
  
  $$\mathbb{E}[Y \mid X_1 = x_1, X_2 = x_2, \dots, X_d = x_d] = w_0 + w_1 x_1 + w_2 x_2 + \dots + w_d x_d$$
  
  To evaluate the mathematical meaning of an individual weight $w_j$, compute the **partial derivative** of the conditional expectation function with respect to $x_j$:
  
  $$\mathbf{\frac{\partial \, \mathbb{E}[Y \mid X]}{\partial x_j} = w_j}$$
  
  * By fundamental multivariable calculus, a partial derivative $\frac{\partial}{\partial x_j}$ computes the instantaneous rate of change along the $x_j$ coordinate axis **while treating all other independent variables ($x_k$ for $k \neq j$) as mathematical constants**.
  * This is the mathematical definition of **ceteris paribus** (*"all else held equal"*).
  * **The Linguistic Trap:** Stating *"an increase in education yields a $2.4\text{k}$ increase in income"* is false. The correct statement is: *"Comparing two individuals with the **exact same age, experience, and job tier**, an individual with one additional year of education is predicted to earn $2.4\text{k}$ more."*
  
  ---
### B. Dimensional Analysis: Units Matter (Slide 2)
  Slide 2 notes: *"A coefficient of 2.4 on 'years of education' means 2.4 units of $y$ per year."*
  
  A regression coefficient is not a pure unitless scalar; it carries physical units defined by the ratio of the target to the feature:
  
  $$\text{Units}(w_j) = \frac{\text{Units}(Y)}{\text{Units}(X_j)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Scale Illusion:                                                    │
  │ Suppose we predict annual salary in USD ($) using two features:       │
  │                                                                        │
  │ 1. Feature x₁: Education measured in YEARS.                            │
  │    w₁ = +2,400  ──▶ Predicted change: +$2,400 per 1 year of school.   │
  │                                                                        │
  │ 2. Feature x₂: Commute distance measured in MILLIMETERS.               │
  │    w₂ = +0.00005 ──▶ Predicted change: +$0.00005 per 1 mm of commute.  │
  │                                                                        │
  │ • Looking at raw magnitudes: |w₁| = 2,400 >> |w₂| = 0.00005.          │
  │ • Naive Trap: "Education is millions of times more important than      │
  │   commute distance!"                                                   │
  │ • Reality: If we measure commute in kilometers, w₂ becomes +$50.00     │
  │   per km. If measured in light-years, w₂ explodes to billions.         │
  │   Raw coefficient magnitudes reflect arbitrary unit choices!           │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### C. Standardized Coefficients ($\beta^*$) (Slide 2)
  Slide 2 states:
  $$\mathbf{\text{"On standardized features: the change in } y \text{ per one standard deviation of } x_j\text{."}}$$
  $$\mathbf{\text{"Magnitudes are only comparable across features if the features were standardized."}}$$
  
  When features are preprocessed via `StandardScaler` ($z_j = \frac{x_j - \bar{x}_j}{s_{x_j}}$) and the target is standardized ($z_y = \frac{y - \bar{y}}{s_y}$), the fitted **Standardized Coefficients** ($\beta_j^*$) become unitless:
  
  $$\beta_j^* = w_j \cdot \frac{s_{x_j}}{s_y}$$
  
  * **Interpretation:** A one **standard deviation ($1\sigma$) increase** in $x_j$, holding all other features fixed, predicts a $\beta_j^*$ standard deviation change in $y$.
  * Only after removing physical units through standardization can an engineer directly compare $|\beta_j^*|$ to $|\beta_k^*|$ to determine which feature contributes more variance to the linear predictor.
  
  ---
## 2. A Coefficient Is a Rate, Not an Importance Score (Slide 3)
  
  Slide 3 presents two dynamic coordinate comparisons to clarify parameter mechanics:
  
  $$\mathbf{\text{"Move the intercept and the whole line shifts. Move the slope and it pivots."}}$$
  $$\mathbf{\text{"A coefficient is a rate, not an importance score."}}$$
  
  ```
                     Intercept Shift vs. Slope Pivot (Slide 3)
         Effect of Intercept (β₀): Shift                   Effect of Slope (β₁): Pivot
    y                                                 y
   20 ─┼                     /  β₀ = 5 (Up)          30 ─┼                     /  β₁ = 3.0 (Steep)
       │                   /                             │                   /
   15 ─┼                 /                           20 ─┼                 /   β₁ = 1.5 (Moderate)
       │               /    β₀ = 2 (Baseline)            │               /
   10 ─┼             /                               10 ─┼             /
       │           /        β₀ = -3 (Down)               │           /       β₁ = 0.5 (Flat)
    5 ─┼         /                                    5 ─┼         /
       │       /                                         │       /
    0 ─┴──────/──────────────────▶ x                  0 ─┴──────/──────────────▶ x
       0      2      4      6                         0      2      4      6
  ```
### The Mechanical Distinction:
  1. **The Intercept ($w_0$):**
   * Acts as a **rigid vertical translation operator**.
   * It moves the entire regression hyperplane parallel to itself along the $y$-axis without altering the angle of the plane relative to any feature axis.
   * Physically represents the predicted expected value when all input features equal zero ($\mathbb{E}[Y \mid X = \mathbf{0}]$).
  2. **The Slopes ($w_1, \dots, w_d$):**
   * Act as **rotational / pivoting operators**.
   * Each $w_j$ anchors the line at the $y$-intercept and rotates the hyperplane around the orthogonal subspace formed by the remaining $d-1$ features.
   * **Why Slope $\neq$ Importance:** A feature with a massive slope ($w_j = 10{,}000$) might vary by only $\pm 0.0001$ in the real world, contributing almost no variance to the final prediction. Conversely, a feature with a tiny slope ($w_k = 0.05$) might span millions of units, driving $90\%$ of the model's total output variance. **A slope is an exchange rate, not an importance metric.**
  
  ---
## 3. The Four Things a Coefficient Is Not (Slide 4)
  
  Slide 4 formalizes the four foundational traps in applied linear modeling:
  
  ```
  ┌───────────────────────────────────┬───────────────────────────────────┐
  │ 1. NOT a Causal Effect            │ 2. NOT an Importance Score        │
  │                                   │                                   │
  │ An association conditional on     │ Depends entirely on feature scale │
  │ whatever else happens to be in    │ and on which other covariates are │
  │ the model design matrix.          │ included in the model.            │
  ├───────────────────────────────────┼───────────────────────────────────┤
  │ 3. NOT Stable Under Collinearity  │ 4. NOT Meaningful If Fit Is Bad   │
  │                                   │                                   │
  │ Add a correlated feature and the  │ Interpreting a model that does    │
  │ coefficient can shrink, explode,  │ not fit the data is interpreting  │
  │ or flip sign entirely!            │ random noise with decimals.       │
  └───────────────────────────────────┴───────────────────────────────────┘
  ```
  
  ---
### In-Depth Systems Deconstruction of the Four Negations:
#### 1. "Not a Causal Effect" (Observational vs. Interventional)
  * Regression estimates an **observational conditional expectation**:
  $$\mathbb{E}[Y \mid X_j = x_j]$$
  * Causation asks an **interventional question** under Judea Pearl’s $do$-calculus:
  $$\mathbb{E}[Y \mid do(X_j = x_j)]$$
  * A coefficient $w_j = +5.2$ does **not** mean that if a policymaker actively intervenes to increase feature $x_j$ by one unit, $y$ will increase by $5.2$. It merely states that in an unperturbed historical dataset, individuals who happen to have an extra unit of $x_j$ (and identical values on all other observed features) had, on average, $5.2$ units higher $y$.
#### 2. "Not an Importance Score on Its Own"
  * The numerical value of $w_j$ depends on:
  1. The physical measurement scale of $X_j$ (as shown in the dollars vs. millimeters example).
  2. The specific set of co-features included in the design matrix $X$.
  * Removing or adding a single covariate alters the projections of all other features onto the column space $\text{Col}(X)$, shifting their partial slopes.
#### 3. "Not Stable Under Collinearity"
  * When two features are correlated ($\text{Corr}(X_1, X_2) > 0$), they share overlapping information.
  * As will be proven in Section 2, introducing a collinear feature can cause $w_1$ to drop to zero, double in magnitude, or **completely invert its algebraic sign** (e.g., flipping from $+0.45$ to $-1.28$). A parameter that changes sign based on covariate presence cannot be treated as an intrinsic physical constant.
#### 4. "Not Meaningful If the Fit Is Bad"
  * Slide 4 emphasizes: *"Interpreting a model that does not fit is interpreting noise with decimals."*
  * If a linear model is fitted to non-linear data (such as Anscombe’s quadratic curve, where $R^2$ is low or residuals exhibit severe curvature), the OLS normal equations will still output numerical values with sixteen decimals of precision:
  $$w_1 = 0.5000000000000000$$
  * Those decimals are mathematical artifacts of projecting a line onto a parabolic manifold. Quoting and interpreting coefficients from a model that fails residual validation is meaningless.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why is the phrase "holding all other features fixed" mathematically required when interpreting the coefficient $w_j$ in a multiple linear regression model?*
  2. *If a linear model predicts hospital patient recovery time ($Y$ in days) using `dosage` ($X_1$ in milligrams) with $w_1 = -0.04$, what are the exact units of $w_1$, and what does this number physically claim?*
  3. *Why is it invalid to rank feature importance by sorting raw regression coefficients by absolute magnitude ($|w_1| \ge |w_2| \ge \dots \ge |w_d|$)? How does standardization resolve this issue?*
  
  ---