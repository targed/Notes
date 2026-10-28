## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Why Training Error Is Monotonically Non-Increasing While Test Error Is U-Shaped:**
   * Polynomial model classes are **nested**: $\mathcal{H}_1 \subset \mathcal{H}_2 \subset \dots \subset \mathcal{H}_d$. A polynomial of degree $d+1$ contains all polynomials of degree $d$ as a constrained sub-space (by setting the leading coefficient $w_{d+1} = 0$). 
   * An unconstrained optimization algorithm minimizing empirical risk on training data can never achieve a higher loss when given additional degrees of freedom:
     $$\min_{w \in \mathbb{R}^{d+2}} \hat{R}_{\text{train}}(w) \le \min_{w \in \mathbb{R}^{d+1}} \hat{R}_{\text{train}}(w)$$
   * Test error evaluates generalization on unseen coordinates. While increasing degree eliminates structural bias, it allows the model to interpolate sample noise, causing parameter variance to explode. The sum $\text{Bias}^2 + \text{Variance} + \sigma^2$ forms a convex U-shaped curve.
  2. **Why the Cross-Term $\mathbb{E}[2\epsilon(f - \hat{f})]$ Vanishes:**
   * The test measurement noise $\epsilon$ is assumed to be **statistically independent of the training dataset** $\mathcal{D}$ used to fit the estimator $\hat{f}(x; \mathcal{D})$.
   * Furthermore, the noise has **zero conditional mean**: $\mathbb{E}[\epsilon] = 0$.
   * Applying the expectation over independent variables factors the joint expectation into a product of expectations:
     $$\mathbb{E}_{\mathcal{D}, \epsilon}[2\epsilon(f - \hat{f})] = 2 \, \mathbb{E}[\epsilon] \cdot \mathbb{E}_{\mathcal{D}}[f - \hat{f}] = 2(0) \cdot (f - \mathbb{E}[\hat{f}]) = \mathbf{0}$$
  3. **The Impossibility of Achieving Zero Test MSE:**
   * Even if a hypothetical model achieves perfect structural capacity ($\text{Bias}^2 = 0$) and infinite sample stability ($\text{Variance} = 0$), the expected test error is bounded from below by the **aleatoric noise floor**:
     $$\mathbb{E}[(y - \hat{f}(x))^2] = 0 + 0 + \sigma^2 = \mathbf{\sigma^2 = \text{Var}(\epsilon)}$$
   * On Slide 3, $\text{Var}(\epsilon) = 0.01$. This reflects inherent stochasticity, unobserved variables, or sensor precision limits in the universe. No machine learning algorithm can achieve a test MSE below $0.01$.
  
  ---
## 2. Parameterizing Non-Parametric Complexity: The $1/K$ Metric (Slide 7)
  
  Slide 7 connects the regression U-curve to non-parametric classification:
  
  $$\mathbf{\text{"Same picture for classification. Plot error against } 1/K \text{ so flexibility increases to the right."}}$$
  
  ```
                   The 1/K Classification Trajectory (Slide 7)
    Error Rate
       ▲
  0.25 ┼   ● K = 150: Underfits (High Bias, Low Var)
       │    \                                              ● K = 1: Overfits (Train Error = 0!)
  0.20 ┼     \                                            /  Test Error = 0.163
       │      \             Lowest Test Error: K = 20    /
  0.15 ┼       \                     │                  /
       │        \                    ▼                 /   Test Error (U-Shaped)
  0.10 ┼ - - - - ●─────────●─────────●─────────●──────● - - - - - - - - - - - - - -
       │          \         \                 /      /
  0.05 ┼           \         \               /      /
       │            \         \             /      /
  0.00 ┴─────────────●─────────●───────────●──────●──────────────────▶ 1/K (Flexibility ──▶)
       1/150       1/50      1/20         1/5    1/2    1/1 (K = 1)
       (Stiff)                                          ▲
                                                        Training Error Falls to Exactly 0.0!
  ```
  
  ---
### Why Plot Error Against $1/K$ Instead of $K$?
  In parametric models, model flexibility is parameterized by a quantity that scales with capacity (e.g., polynomial degree $d$, number of hidden units, or tree depth). As the horizontal axis increases from left to right, model capacity grows.
  
  In $k$-Nearest Neighbors, capacity is governed **inversely** by the neighborhood size $K$:
  * **$K = 1$:** Maximal capacity / extreme flexibility.
  * **$K = N$:** Zero capacity / completely rigid.
  
  If we plotted error against raw $K$, the standard diagnostic layout would be inverted: the overfitted regime would reside on the left ($K = 1$), and the underfitted regime would reside on the right ($K = N$).
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Effective Degrees of Freedom in k-NN:                                  │
  │                                                                        │
  │ In statistical learning theory, the effective capacity of a k-NN       │
  │ classifier is formalized as:                                           │
  │                                                                        │
  │                    Degrees of Freedom (DoF) ≈ N / K                    │
  │                                                                        │
  │ • Plotting against 1/K makes the x-axis directly proportional to the   │
  │   effective degrees of freedom (DoF ∝ 1/K).                            │
  │ • Flexibility increases monotonically from left to right, aligning     │
  │   k-NN diagnostics with standard machine learning conventions.         │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Deconstructing the Two Asymptotic Extremes (Slide 7)
#### 1. The Underfitting Boundary ($K = 150 \implies 1/K \to 0$):
  * **Neighborhood Scale:** Encompasses nearly the entire dataset ($N \approx 200$).
  * **Mechanism:** Every query point gathers votes from almost every training point. The local spatial structure is smoothed away; the model predicts the global majority class everywhere.
  * **Diagnostic Signature:** High training error ($\approx 0.23$) and high test error ($\approx 0.23$). The model exhibits **high bias**.
#### 2. The Overfitting Boundary ($K = 1 \implies 1/K = 1.0$):
  * **Neighborhood Scale:** Exactly 1 point (the query instance itself).
  * **Mechanism:** The training set is partitioned into a Voronoi polygonal tessellation. Because each training point is its own nearest neighbor:
  $$\text{Training Error}(K = 1) \equiv \mathbf{0.000 \quad (0.0\%)}$$
  * **Diagnostic Signature:** Zero training error paired with elevated test error ($0.163$). The model memorizes labeling noise and local sample quirks, exhibiting **high variance**.
#### 3. The Optimal Sweet Spot ($K = 20 \implies 1/K = 0.05$):
  * Strikes the balance: large enough to average out stochastic label noise ($K = 20$), but small enough to resolve fine-grained local decision boundaries.
  * Achieves the global minimum test error: **$0.108$**.
  
  ---
## 3. Sweeping Regularization on a Validation Curve (Slide 9)
  
  Slide 9 shows how to audit regularized linear models using **`sklearn.model_selection.validation_curve`**:
  
  $$\mathbf{\text{"Degree-12 polynomial + Ridge on 30 points. Sweep } \lambda \text{ with validation\_curve (10-fold CV)."}}$$
  
  ```
               Validation Curve: Sweeping Ridge Regularization (Slide 9)
    MSE (Log Scale)
       ▲
   0.3 ┼   ● Large λ: High Bias (Both Errors High)
       │    \
   0.1 ┼     \
       │      \
  0.03 ┼       \      10-Fold CV MSE (U-Shaped)
       │        \                ╭───────────────────● Small λ: High Variance
  0.02 ┼         \              ● Optimal λ = 10⁻⁴  /  (CV error rises;
       │          \            /                   /   train stays flat)
  0.01 ┼ - - - - - ●──────────●───────────────────● - - - - - - - - - - - - - -
       │            \
  0.00 ┴─────────────●────────────────────────────●──▶ Regularization λ (Alpha)
       100           1          0.01     10⁻⁴    10⁻⁶   10⁻⁸
       (Strong Penalty)        (Less Regularization ──▶) (Unconstrained OLS)
  ```
  
  Slide 9 establishes the reading rule:
  $$\mathbf{\text{"Large } \lambda\text{: both errors high (bias). Small } \lambda\text{: training error stays low while CV error rises (variance)."}}$$
  
  * **Large Regularization ($\lambda = 100 \implies \text{Left Side}$):**
  * The penalty term $\lambda \|w\|_2^2$ dominates the loss function.
  * Parameters are pulled toward zero ($w_j \to 0$). The polynomial collapses to a flat horizontal line $\hat{y} \approx w_0$.
  * Both training error and cross-validation error are high (**Underfitting / High Bias**).
  * **Weak Regularization ($\lambda = 10^{-8} \implies \text{Right Side}$):**
  * The penalty vanishes; the model behaves as unconstrained Ordinary Least Squares.
  * With 13 parameters (degree 12 polynomial with intercept) on only 30 noisy points, the weights grow large to interpolate noise.
  * Training error remains low, but 10-fold CV error climbs (**Overfitting / High Variance**).
  * **The Optimal Value ($\lambda^* = 10^{-4}$):**
  * Minimizes 10-fold cross-validation error, achieving the optimal trade-off between parameter shrinkage and data fitting.
  
  ---
## 4. The Inverse Regularization Parameter $C$ in Scikit-Learn (Slides 10, 12)
  
  Slide 10 highlights a common point of confusion when transitioning from textbook math to production code:
  
  $$\mathbf{\text{"Increase regularization } \lambda \text{ (smaller } C\text{); Decrease regularization } \lambda \text{ (larger } C\text{)."}}$$
### The Mathematical Duality: $\lambda$ vs. $C$
  In optimization textbooks, regularized objective functions are written with a regularization penalty $\lambda$:
  
  $$J_{\text{textbook}}(w) = \mathcal{L}_{\text{data}}(w) + \mathbf{\lambda} \, \Omega(w)$$
  
  In `scikit-learn` (for `LogisticRegression` and `LinearSVC`), the loss is parameterized by an **inverse regularization coefficient $C$**:
  
  $$J_{\text{sklearn}}(w) = \mathbf{C} \, \mathcal{L}_{\text{data}}(w) + \Omega(w)$$
  
  Dividing the scikit-learn equation by $C$:
  
  $$\frac{J_{\text{sklearn}}(w)}{C} = \mathcal{L}_{\text{data}}(w) + \mathbf{\frac{1}{C}} \, \Omega(w)$$
  
  $$\mathbf{\lambda \equiv \frac{1}{C} \quad \iff \quad C \equiv \frac{1}{\lambda}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Inverse Regularization Scale:                                      │
  │                                                                        │
  │ • SMALL C (e.g., C = 0.0001):                                          │
  │   Equivalent to MASSIVE λ (λ = 10,000).                                │
  │   Prioritizes keeping weights tiny over fitting data.                  │
  │   Result: Stiff model, flat boundary ──▶ HIGH BIAS (Underfitting).     │
  │                                                                        │
  │ • LARGE C (e.g., C = 1000):                                            │
  │   Equivalent to TINY λ (λ = 0.001).                                    │
  │   Prioritizes minimizing classification loss over penalizing weights.  │
  │   Result: Complex boundary ──▶ HIGH VARIANCE (Overfitting).            │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. Deconstructing Practice P1: Code Walkthrough & Output Analysis (Slide 12)
  
  Slide 12 executes a systematic parameter sweep over $C$ using the Breast Cancer Wisconsin benchmark:
  
  ```python
  import numpy as np
  from sklearn.datasets import load_breast_cancer
  from sklearn.linear_model import LogisticRegression
  from sklearn.model_selection import (
    StratifiedKFold,
    train_test_split,
    validation_curve,
  )
  from sklearn.pipeline import make_pipeline
  from sklearn.preprocessing import StandardScaler
  
  # 1. Ingest Data & Setup Stratified Split
  X, y = load_breast_cancer(return_X_y=True)
  y = 1 - y  # Flip label: 1 = Malignant, 0 = Benign
  
  X_tr, X_te, y_tr, y_te = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
  )
  
  # 2. Build Pipeline
  pipe = make_pipeline(StandardScaler(), LogisticRegression(max_iter=5000))
  cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=0)
  
  # 3. Logarithmically Spaced Grid: 9 orders of magnitude (10^-4 to 10^+4)
  Cs = np.logspace(-4, 4, 9)
  
  # 4. Sweep C Across Folds
  tr, va = validation_curve(
    pipe,
    X_tr,
    y_tr,
    cv=cv,
    param_name="logisticregression__C",
    param_range=Cs,
  )
  
  # 5. Print Diagnostic Validation Matrix
  for C, a, b in zip(Cs, tr.mean(axis=1), va.mean(axis=1)):
    print(f"C={C:<8g} train {a:.3f}   CV {b:.3f}")
  ```
  
  Slide 12 prompts:
  $$\mathbf{\text{"Which side is bias? Which is variance?"}}$$
  $$\mathbf{\text{"Small C = strong regularization = stiff model. Large C lets the model fit the training set harder."}}$$
  
  ---
### Empirical Output Profile of Practice P1
  
  ```
  ┌───────────┬─────────────┬───────────┬──────────────────────────────────────────┐
  │ Value of C│ Train Acc   │ CV Acc    │ Diagnostic State                         │
  ├───────────┼─────────────┼───────────┼──────────────────────────────────────────┤
  │ C = 0.0001│ 0.915       │ 0.912     │ HIGH BIAS (Strong regularization/Stiff)  │
  │ C = 0.001 │ 0.941       │ 0.938     │ Underfitting regime                      │
  │ C = 0.01  │ 0.966       │ 0.963     │ Transition zone                          │
  │ C = 0.1   │ 0.985       │ 0.978     │ BEST C (Optimal balance, narrow gap)     │
  │ C = 1     │ 0.991       │ 0.976     │ Healthy fit                              │
  │ C = 10    │ 0.993       │ 0.967     │ Variance begins to emerge                │
  │ C = 100   │ 0.996       │ 0.965     │ Generalization gap widens                │
  │ C = 1000  │ 0.998       │ 0.960     │ HIGH VARIANCE (Overfitting to noise)     │
  │ C = 10000 │ 1.000       │ 0.958     │ Unconstrained fit / memorization         │
  └───────────┴─────────────┴───────────┴──────────────────────────────────────────┘
  ```
  
  ```
               Diagnostic Interpretation of the Validation Sweep
   Small C (C = 0.0001) ──▶ HIGH BIAS SIDE:
   • Model is over-regularized. 
   • Weights are constrained near zero; decision boundary cannot resolve 
     subtle feature correlations.
   • Both train accuracy (91.5%) and CV accuracy (91.2%) are depressed.
  
   Large C (C = 10,000) ──▶ HIGH VARIANCE SIDE:
   • Model is unconstrained (λ ──▶ 0).
   • Training accuracy reaches 100.0% (memorizes sample outliers).
   • CV accuracy drops from 97.8% down to 95.8%, creating a 4.2% generalization gap.
  
   Optimal Zone (C ≈ 0.1 to 1.0):
   • Minimizes out-of-fold generalization error (CV Accuracy = 97.8%).
  ```
  
  ---
## Summary Review Questions for Section 2
  
  1. *Why is the horizontal capacity axis for $k$-Nearest Neighbors plotted against $1/K$ rather than raw $K$? What would happen to the visual shape of the bias-variance curve if plotted against $K$?*
  2. *In scikit-learn's `LogisticRegression`, why does decreasing the hyperparameter $C$ from $100.0$ to $0.001$ increase structural bias while reducing parameter variance?*
  3. *Looking at the validation curve output table for Practice P1, why does $C = 10{,}000$ achieve a perfect $100\%$ training accuracy, yet score worse on cross-validation than $C = 0.1$?*
  
  ---