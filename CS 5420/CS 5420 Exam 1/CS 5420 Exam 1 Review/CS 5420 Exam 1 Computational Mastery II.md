## Problem C7: Least Squares and Gradient Steps
  
  > **Problem Statement:**  
  > $$x = [1, \; 2, \; 3, \; 4], \quad y = [3, \; 5, \; 4, \; 8]$$
  > *Model: $\hat{y} = w_0 + w_1 x$ (linear regression with an intercept).*  
  > *(a) Use least squares to find $w_0$ and $w_1$.*  
  > *(b) From $w_0 = w_1 = 0$ with $\eta = 0.05$, do ONE batch gradient-descent step on $J = \frac{1}{n}\sum(\hat{y} - y)^2$.*  
  > *(c) Do ONE stochastic gradient-descent step from $w_0 = w_1 = 0$ using only the last sample ($x = 4, y = 8$), $\eta = 0.05$.*
  
  ---
### (a) Analytical Ordinary Least Squares Solution
  From Lecture 10, the scalar closed-form equations for simple linear regression are:
  $$w_1 = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^n (x_i - \bar{x})^2} = \frac{\widehat{\text{Cov}}(x, y)}{\widehat{\text{Var}}(x)}, \quad w_0 = \bar{y} - w_1 \bar{x}$$
#### 1. Compute Centroids:
  $$\bar{x} = \frac{1 + 2 + 3 + 4}{4} = \frac{10}{4} = \mathbf{2.5}$$
  $$\bar{y} = \frac{3 + 5 + 4 + 8}{4} = \frac{20}{4} = \mathbf{5.0}$$
#### 2. Compute Deviations and Covariance Terms:
  ```
  ┌──────┬─────┬─────┬─────────────┬─────────────┬─────────────────┬───────────────────────┐
  │ i    │ x_i │ y_i │ (x_i - x̄)   │ (y_i - ȳ)   │ (x_i - x̄)²      │ (x_i - x̄)(y_i - ȳ)    │
  ├──────┼─────┼─────┼─────────────┼─────────────┼─────────────────┼───────────────────────┤
  │ 1    │ 1   │ 3   │ 1 - 2.5=-1.5│ 3 - 5.0=-2.0│ (-1.5)² = 2.25  │ (-1.5)(-2.0) = +3.00  │
  │ 2    │ 2   │ 5   │ 2 - 2.5=-0.5│ 5 - 5.0= 0.0│ (-0.5)² = 0.25  │ (-0.5)( 0.0) =  0.00  │
  │ 3    │ 3   │ 4   │ 3 - 2.5=+0.5│ 4 - 5.0=-1.0│ (+0.5)² = 0.25  │ (+0.5)(-1.0) = -0.50  │
  │ 4    │ 4   │ 8   │ 4 - 2.5=+1.5│ 8 - 5.0=+3.0│ (+1.5)² = 2.25  │ (+1.5)(+3.0) = +4.50  │
  ├──────┴─────┴─────┴─────────────┴─────────────┼─────────────────┼───────────────────────┤
  │ Sums (∑):                                    │ ∑(x - x̄)² = 5.00│ ∑(x - x̄)(y - ȳ) = 7.00│
  └──────────────────────────────────────────────┴─────────────────┴───────────────────────┘
  ```
#### 3. Calculate $w_1$ and $w_0$:
  $$w_1 = \frac{7.00}{5.00} = \mathbf{1.400}$$
  $$w_0 = \bar{y} - w_1 \bar{x} = 5.00 - (1.400 \times 2.50) = 5.00 - 3.50 = \mathbf{1.500}$$
  
  * **Answer for (a):** $\mathbf{w_0 = 1.500, \; w_1 = 1.400} \implies \mathbf{\hat{y} = 1.500 + 1.400x}$
  
  ---
### (b) ONE Step of Batch Gradient Descent
  * **Initial State:** $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$
  * **Hyperparameters:** $\eta = 0.05, \; n = 4$
  * **Cost Function:** $J(w_0, w_1) = \frac{1}{n} \sum_{i=1}^n (\hat{y}_i - y_i)^2 = \frac{1}{n} \sum_{i=1}^n (w_0 + w_1 x_i - y_i)^2$
#### 1. Evaluate Current Predictions and Errors:
  Because $w_0 = w_1 = 0$, every prediction is zero: $\hat{y}_i = 0 + 0(x_i) = 0$.
  The residual error vector $e_i = \hat{y}_i - y_i$ is simply $-y_i$:
  $$e = [-3, \; -5, \; -4, \; -8]^T$$
#### 2. Compute Analytical Gradients:
  $$\nabla_{w_0} J = \frac{2}{n} \sum_{i=1}^n (\hat{y}_i - y_i) = \frac{2}{4} (-3 - 5 - 4 - 8) = \frac{1}{2} (-20) = \mathbf{-10.000}$$
  $$\nabla_{w_1} J = \frac{2}{n} \sum_{i=1}^n x_i (\hat{y}_i - y_i) = \frac{2}{4} \Big[ 1(-3) + 2(-5) + 3(-4) + 4(-8) \Big]$$
  $$\nabla_{w_1} J = \frac{1}{2} \Big[ -3 - 10 - 12 - 32 \Big] = \frac{1}{2} (-57) = \mathbf{-28.500}$$
#### 3. Execute Parameter Updates:
  $$w_0^{(1)} = w_0^{(0)} - \eta \nabla_{w_0} J = 0 - 0.05(-10.000) = \mathbf{+0.500}$$
  $$w_1^{(1)} = w_1^{(0)} - \eta \nabla_{w_1} J = 0 - 0.05(-28.500) = \mathbf{+1.425}$$
  
  * **Answer for (b):** $\mathbf{w_0^{(1)} = 0.500, \; w_1^{(1)} = 1.425}$
  
  ---
### (c) ONE Step of Stochastic Gradient Descent (Single Sample)
  * **Active Sample:** $x_4 = 4, \; y_4 = 8$
  * **Initial State:** $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0, \; \eta = 0.05$
  * **Per-Sample Loss:** $\mathcal{L}_4 = (\hat{y}_4 - y_4)^2$
#### 1. Compute Prediction and Error:
  $$\hat{y}_4 = 0 + 0(4) = 0 \implies e_4 = \hat{y}_4 - y_4 = 0 - 8 = -8$$
#### 2. Compute Sample Gradients:
  $$\nabla_{w_0} \mathcal{L}_4 = 2(\hat{y}_4 - y_4) = 2(-8) = \mathbf{-16.000}$$
  $$\nabla_{w_1} \mathcal{L}_4 = 2 x_4 (\hat{y}_4 - y_4) = 2(4)(-8) = \mathbf{-64.000}$$
#### 3. Execute Stochastic Update:
  $$w_0^{(1)} = 0 - 0.05(-16.000) = \mathbf{+0.800}$$
  $$w_1^{(1)} = 0 - 0.05(-64.000) = \mathbf{+3.200}$$
  
  * **Answer for (c):** $\mathbf{w_0^{(1)} = 0.800, \; w_1^{(1)} = 3.200}$
  
  ---
## Problem C8: Logistic Regression by Hand
  
  > **Problem Statement:**  
  > $$w = [2, \; -1]^T, \quad b = -1, \quad x = [1.5, \; 1]^T$$
  > *(a) Compute $z$ and $p = \sigma(z)$.*  
  > *(b) Predicted class at threshold 0.5? At 0.8?*  
  > *(c) Write the equation of the decision boundary at threshold 0.5.*
  
  ---
### (a) Compute Linear Score ($z$) and Probability ($p$)
  1. **Compute Logit Score $z$:**
   $$z = w^T x + b = w_1 x_1 + w_2 x_2 + b = 2(1.5) + (-1)(1.0) - 1$$
   $$z = 3.0 - 1.0 - 1.0 = \mathbf{1.000}$$
  2. **Compute Sigmoid Probability $p$:**
   $$p = \sigma(z) = \frac{1}{1 + e^{-z}} = \frac{1}{1 + e^{-1.0}} = \frac{1}{1 + 0.367879} = \frac{1}{1.367879} \approx \mathbf{0.731}$$
  
  * **Answer for (a):** $\mathbf{z = 1.000, \; p = 0.731}$
  
  ---
### (b) Threshold Predictions
  The decision rule assigns class labels based on threshold $\tau$:
  $$\hat{y} = \begin{cases} 1 & \text{if } p \ge \tau \\ 0 & \text{if } p < \tau \end{cases}$$
  
  1. **At Threshold $\tau = 0.5$:**
   $$p = 0.731 \ge 0.5 \implies \mathbf{\text{Class 1}}$$
  2. **At Threshold $\tau = 0.8$:**
   $$p = 0.731 < 0.8 \implies \mathbf{\text{Class 0}}$$
  
  * **Answer for (b):** **Class 1 at 0.5; Class 0 at 0.8**
  
  ---
### (c) Decision Boundary Equation ($\tau = 0.5$)
  At threshold $\tau = 0.5$, the decision boundary is the locus of points where the model is maximally uncertain ($p = 0.5 \iff z = 0$):
  
  $$w^T x + b = 0 \implies 2x_1 - x_2 - 1 = 0$$
  
  Solving for $x_2$:
  $$\mathbf{x_2 = 2x_1 - 1}$$
  
  * **Answer for (c):** $\mathbf{2x_1 - x_2 - 1 = 0 \quad (\text{or } x_2 = 2x_1 - 1)}$
  
  ---
## Problem C9: $k$-NN by Hand
  
  > **Problem Statement:**  
  > *Training instances in $\mathbb{R}^2$:*  
  > *Point A: $(1, 1)$, Class: red*  
  > *Point B: $(2, 1)$, Class: red*  
  > *Point C: $(4, 3)$, Class: blue*  
  > *Point D: $(5, 4)$, Class: blue*  
  > *Point E: $(1, 3)$, Class: blue*  
  > *Query observation: $q = (2, 2)$.*  
  > *(a) Compute the Euclidean distance from $q$ to each point.*  
  > *(b) Predict with $k = 1, 3,$ and $5$.*  
  > *(c) What is the Manhattan distance from $q$ to C?*
  
  ---
### (a) Compute Euclidean Distances from $q = (2, 2)$
  $$d(q, p) = \sqrt{(q_1 - p_1)^2 + (q_2 - p_2)^2}$$
  
  1. **Point A $(1, 1)$:**
   $$d(q, A) = \sqrt{(2 - 1)^2 + (2 - 1)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$$
  2. **Point B $(2, 1)$:**
   $$d(q, B) = \sqrt{(2 - 2)^2 + (2 - 1)^2} = \sqrt{0 + 1} = \sqrt{1} = \mathbf{1.000}$$
  3. **Point C $(4, 3)$:**
   $$d(q, C) = \sqrt{(2 - 4)^2 + (2 - 3)^2} = \sqrt{(-2)^2 + (-1)^2} = \sqrt{4 + 1} = \sqrt{5} \approx \mathbf{2.236}$$
  4. **Point D $(5, 4)$:**
   $$d(q, D) = \sqrt{(2 - 5)^2 + (2 - 4)^2} = \sqrt{(-3)^2 + (-2)^2} = \sqrt{9 + 4} = \sqrt{13} \approx \mathbf{3.606}$$
  5. **Point E $(1, 3)$:**
   $$d(q, E) = \sqrt{(2 - 1)^2 + (2 - 3)^2} = \sqrt{1^2 + (-1)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$$
  
  * **Answer for (a):**  
  $\mathbf{d(A) = 1.414, \; d(B) = 1.000, \; d(C) = 2.236, \; d(D) = 3.606, \; d(E) = 1.414}$
  
  ---
### (b) Predictions for $k = 1, 3, 5$
  Sort points by distance from query $q$:
  
  ```
  ┌──────┬──────────┬──────────┬──────────┬────────────────────────┐
  │ Rank │ Point    │ Distance │ Class    │ Neighborhood Inclusion │
  ├──────┼──────────┼──────────┼──────────┼────────────────────────┤
  │ 1    │ B        │ 1.000    │ red      │ Included in k=1, 3, 5  │
  │ 2    │ A (tied) │ 1.414    │ red      │ Included in k=3, 5     │
  │ 3    │ E (tied) │ 1.414    │ blue     │ Included in k=3, 5     │
  │ 4    │ C        │ 2.236    │ blue     │ Included in k=5        │
  │ 5    │ D        │ 3.606    │ blue     │ Included in k=5        │
  └──────┴──────────┴──────────┴──────────┴────────────────────────┘
  ```
  
  1. **For $k = 1$:**
   * Nearest neighbor is **Point B** (distance = 1.000).
   * Class: **red**
  2. **For $k = 3$:**
   * Nearest 3 neighbors are **B** (red), **A** (red), and **E** (blue).
   * Vote tally: 2 red vs. 1 blue.
   * Majority Class: **red**
  3. **For $k = 5$:**
   * Nearest 5 neighbors are **all 5 points**: B (red), A (red), E (blue), C (blue), D (blue).
   * Vote tally: 2 red vs. 3 blue.
   * Majority Class: **blue**
  
  * **Answer for (b):** $\mathbf{k=1 \implies \text{red}, \quad k=3 \implies \text{red}, \quad k=5 \implies \text{blue}}$
  
  ---
### (c) Manhattan Distance from $q = (2, 2)$ to Point C $(4, 3)$
  $$D_1(q, C) = |q_1 - C_1| + |q_2 - C_2| = |2 - 4| + |2 - 3| = |-2| + |-1| = 2 + 1 = \mathbf{3}$$
  
  * **Answer for (c):** $\mathbf{3}$
  
  ---
## Problem C10: Naive Bayes by Hand
  
  > **Problem Statement:**  
  > *10 training emails: 4 spam, 6 ham.*  
  > *"free" appears in 3 of the 4 spam and 1 of the 6 ham emails;*  
  > *"meeting" appears in 1 of the 4 spam and 3 of the 6 ham emails.*  
  > *A new email contains both words. Using only these two features, compute the posterior $P(\text{spam} \mid \text{email})$, and the prediction.*
  
  ---
### Step 1: Extract Given Probabilities and Priors
  Let target $Y \in \{\text{spam}, \text{ham}\}$. The features are $X_1 = \mathbf{1}_{\{\text{"free"}\}}$ and $X_2 = \mathbf{1}_{\{\text{"meeting"}\}}$.
  
  1. **Class Priors:**
   $$P(\text{spam}) = \frac{4}{10} = 0.400, \quad P(\text{ham}) = \frac{6}{10} = 0.600$$
  2. **Class-Conditional Likelihoods:**
   * For word "free":
     $$P(\text{free} \mid \text{spam}) = \frac{3}{4} = 0.750, \quad P(\text{free} \mid \text{ham}) = \frac{1}{6} \approx 0.1667$$
   * For word "meeting":
     $$P(\text{meeting} \mid \text{spam}) = \frac{1}{4} = 0.250, \quad P(\text{meeting} \mid \text{ham}) = \frac{3}{6} = 0.500$$
  
  ---
### Step 2: Compute Unnormalized Joint Posteriors
  Under the Naive Bayes conditional independence assumption:
  $$P(Y, x) = P(Y) \cdot P(\text{free} \mid Y) \cdot P(\text{meeting} \mid Y)$$
  
  1. **For Class Spam:**
   $$P(\text{spam}, x) = \left(\frac{4}{10}\right) \times \left(\frac{3}{4}\right) \times \left(\frac{1}{4}\right) = \frac{12}{160} = \frac{3}{40} = \mathbf{0.0750}$$
  2. **For Class Ham:**
   $$P(\text{ham}, x) = \left(\frac{6}{10}\right) \times \left(\frac{1}{6}\right) \times \left(\frac{3}{6}\right) = \frac{18}{360} = \frac{1}{20} = \mathbf{0.0500}$$
  
  ---
### Step 3: Normalize to Compute the Exact Posterior
  Calculate total marginal evidence:
  $$P(\text{email}) = P(\text{spam}, x) + P(\text{ham}, x) = 0.0750 + 0.0500 = \mathbf{0.1250}$$
  
  $$P(\text{spam} \mid \text{email}) = \frac{P(\text{spam}, x)}{P(\text{email})} = \frac{0.0750}{0.1250} = \frac{75}{125} = \frac{3}{5} = \mathbf{0.600 \quad (60.0\%)}$$
  $$P(\text{ham} \mid \text{email}) = \frac{P(\text{ham}, x)}{P(\text{email})} = \frac{0.0500}{0.1250} = \frac{50}{125} = \frac{2}{5} = \mathbf{0.400 \quad (40.0\%)}$$
  
  * **Answer for C10:**  
  $$\mathbf{P(\text{spam} \mid \text{email}) = 0.600 \quad (60.0\%) \implies \text{Prediction: } \mathbf{Spam}}$$
  
  ---
## Problem C11: Interpreting Coefficients
  
  > **Problem Statement:**  
  > *A fitted model:*  
  > $$\text{price } (\$1{,}000\text{s}) = 140 + 1.5 \cdot \text{sqft\_100} - 2 \cdot \text{age} + 25 \cdot \text{has\_garage}$$  
  > *where $\text{sqft\_100}$ is square feet / 100.*  
  > *(a) Predict the price of a 1,800 sq ft, 10-year-old house with a garage.*  
  > *(b) Can we conclude that building a garage will raise a house's value by $25,000? Explain.*
  
  ---
### (a) Compute Point Prediction
  Extract feature inputs:
  * $\text{sqft\_100} = \frac{1{,}800}{100} = 18$
  * $\text{age} = 10$
  * $\text{has\_garage} = 1$
  
  Evaluate the model equation:
  $$\text{Price} = 140 + 1.5(18) - 2(10) + 25(1)$$
  $$\text{Price} = 140 + 27.0 - 20.0 + 25.0 = \mathbf{172.0}$$
  
  Because the target variable is measured in thousands of dollars ($\$1{,}000\text{s}$):
  $$\text{Predicted Valuation} = 172.0 \times \$1{,}000 = \mathbf{\$172{,}000}$$
  
  * **Answer for (a):** $\mathbf{\$172{,}000}$
  
  ---
### (b) Causal Assessment of the Garage Coefficient
  Slide/Page 6 asks: *"Can we conclude that building a garage will raise a house's value by $25,000? Explain."*
  
  * **Answer:** **NO.**
  * **Comprehensive Methodological Explanation:**
  1. **Correlation $\neq$ Causation (Observational Regression):** Regression estimates observational conditional expectations ($P(\text{Price} \mid \text{Features})$), not interventional distributions ($P(\text{Price} \mid do(\text{Garage}))$) (Lecture 12).
  2. **Unobserved Confounders:** The binary indicator `has_garage` is correlated with unmeasured confounding variables: overall property luxury, lot acreage, architectural quality, and neighborhood socioeconomic status. Wealthier neighborhoods have larger homes, higher land values, and garages. The $+25$ coefficient absorbs proxy variance from these omitted variables.
  3. **Strict Ceteris Paribus Interpretation:** The model only claims that comparing two houses with the **exact same square footage and age**, a house that already has a garage sells, on average, for **$\$25{,}000$ more** in this historical dataset. It provides zero causal guarantee that spending money to physically construct a garage will appreciate the home's value by $\$25{,}000$.
  
  ---
## Exam 1 Master Synthesis & Final Strategy Guide
  
  ```
  ┌──────────────────────────────┬────────────────────────────────────────────────────────────────────────┐
  │ Core Competency              │ Exam Day Operational Rule of Thumb                                     │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Indexing Duality             │ .loc[0:3] includes label 3 (4 rows); .iloc[0:3] stops before 3 (3 rows)│
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ NumPy Broadcasting           │ Align shapes from right. Dims match if equal OR one of them is 1.      │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Scaling Math                 │ Z = (x - μ) / σ; MinMax = (x - x_min) / (x_max - x_min). Outliers > 1! │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Pipeline Leakage Rule        │ ALWAYS split before fit! Never compute scalers or selectors pre-split. │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Normal Equations             │ XᵀX w = Xᵀy. "Normal" means residual vector r is orthogonal to Col(X). │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Solvers                      │ solve(A, b) is 3× faster and better conditioned than inv(A) @ b.       │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Gradient Descent Regimes     │ Batch = 1 update/epoch; SGD = n updates/epoch; Mini-batch = ⌈n/B⌉.    │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Regularization Geometry      │ L2 (Ridge) = sphere, smooth decay; L1 (Lasso) = diamond, exact zeros.   │
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Binary Cross-Entropy         │ -[y log p̂ + (1-y)log(1-p̂)]. Gradient is (p̂ - y); MSE gradient vanishes!│
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ k-NN Computational Profile   │ Lazy: Train is O(1) free; Inference is O(n·d) scan. Scaler is mandatory│
  ├──────────────────────────────┼────────────────────────────────────────────────────────────────────────┤
  │ Naive Bayes Rule             │ argmax P(Y) ∏ P(X_j | Y). Use Laplace (+1) smoothing to prevent zeros. │
  └──────────────────────────────┴────────────────────────────────────────────────────────────────────────┘
  ```