## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Total Model Fits in Cartesian Grid Search:**
   * Grid dimensions: $8$ neighborhood sizes ($k$), $3$ distance metrics ($p \in \{1, 2, \infty\}$), and $2$ weighting schemes (`'uniform'`, `'distance'`).
   * Total hyperparameter combinations:
     $$|\mathcal{G}| = 8 \times 3 \times 2 = \mathbf{48 \text{ unique configurations}}$$
   * Under $5$-fold cross-validation ($k_{\text{fold}} = 5$), every configuration is trained across each rotation:
     $$N_{\text{fits}} = 5 \times 48 = \mathbf{240 \text{ total model fits}}$$
  2. **Why Precision Picks $k = 23$ While Recall Picks $k = 3$ (Breast Cancer):**
   * **Recall ($k = 3$):** Recall ($\frac{TP}{TP + FN}$) prioritizes catching every positive instance. A small neighborhood ($k = 3$) makes the decision boundary sensitive to local minority clusters. If an ambiguous tumor is surrounded by even 2 malignant neighbors out of 3, the model flags it as positive, driving False Negatives toward zero.
   * **Precision ($k = 23$):** Precision ($\frac{TP}{TP + FP}$) prioritizes alert purity by suppressing false alarms. A large neighborhood ($k = 23$) smooths out boundary noise; the model only predicts malignant when an overwhelming majority of the 23 neighbors agree, driving False Positives toward zero.
  3. **The Two Stages Automated by `GridSearchCV(refit=True)`:**
   * **Step 2 (Tune):** Iterates over all Cartesian grid configurations, evaluates them across $K$ folds, and identifies the best hyperparameter setting $\lambda^*$.
   * **Step 3 (Refit):** Automatically retrains a fresh instance of the winning model architecture using $\lambda^*$ on **$100\%$ of the training data** (`X_train`), creating a single fitted model ready for out-of-sample evaluation (`gs.score(X_test, y_test)`).
  
  ---
## 2. Two Philosophies of Test Error Estimation (Slide 11)
  
  Slide 11 presents a comparison of model selection methods on the Diabetes benchmark ($N = 442, d = 10$ continuous features):
  
  $$\mathbf{\text{"Diabetes data: add features one at a time (forward stepwise). How many should we keep?"}}$$
  $$\mathbf{\text{"Adjust the training error (BIC) or estimate test error directly (CV). Both pick 6 of 10; training error picks all 10."}}$$
  
  ```
                       Estimating Generalization Error (Slide 11)
      Training MSE (Monotonic Drop)            BIC (Training + Penalty)             10-Fold CV MSE (Empirical)
    MSE                                     BIC                                  MSE
   3750 ┼ ●                                3800 ┼ ●                             3750 ┼ ●
        │  \                                    │  \                                 │  \
   3500 ┼   \                              3600 ┼   \                           3500 ┼   \
        │    \                                  │    \                               │    \
   3250 ┼     ●                            3400 ┼     ●                         3250 ┼     ●
        │      \                                │      \                             │      \
   3000 ┼       ●───────● Picks 10         3200 ┼       ●──●──● Picks 6         3000 ┼       ●──●──● Picks 6
        │                                       │          ▲                         │          ▲
        └───────┬───────┬──────▶ Features       └──────────┼──────────▶ Features     └──────────┼──────────▶ Features
        1       5      10                       1          6          10             1          6          10
  ```
  
  Slide 11 contrasts two distinct paradigms for estimating generalization error:
  1. **Empirical Resampling (Cross-Validation):** Explicitly holds out validation folds to measure out-of-sample prediction error directly without distributional assumptions.
  2. **Analytical In-Sample Penalization (Information Criteria):** Fits the model once on the full training set and applies a mathematical penalty proportional to the number of free parameters to offset optimism bias.
  
  ---
## 3. Mathematical Derivation of Information Criteria (AIC & BIC)
  
  Why does training error continuously fall while true test error forms a U-shape? 
  When fitting an Ordinary Least Squares model with $k$ parameters, the empirical training Mean Squared Error underestimates the true expected prediction error by approximately $\frac{2k}{n} \sigma^2$ (optimism of the training error rate, Efron 1986).
  
  Information criteria counteract this by adding an analytical penalty to the in-sample log-likelihood.
  
  ---
### The General Likelihood Formulations:
  Let $\hat{L}$ denote the maximized value of the likelihood function for an estimated model with $k$ parameters trained on $n$ observations:
  * **Akaike Information Criterion (AIC, Akaike 1974):**
  $$\mathbf{\text{AIC} = 2k - 2\ln(\hat{L})}$$
  * **Bayesian Information Criterion (BIC, Schwarz 1978):**
  $$\mathbf{\text{BIC} = k\ln(n) - 2\ln(\hat{L})}$$
  
  ---
### Derivation for Linear Regression Under Gaussian Noise:
  Assume targets follow $y_i = x_i^T w + \epsilon_i$ with independent Gaussian noise $\epsilon_i \sim \mathcal{N}(0, \sigma^2)$. The maximized likelihood is:
  
  $$L(w, \sigma^2) = \left( \frac{1}{2\pi\sigma^2} \right)^{n/2} \exp\left( -\frac{1}{2\sigma^2} \|y - Xw\|_2^2 \right)$$
  
  Evaluating the Maximum Likelihood Estimate for variance:
  $$\hat{\sigma}^2 = \frac{1}{n} \|y - X\hat{w}\|_2^2 = \frac{\text{RSS}}{n} = \text{MSE}_{\text{train}}$$
  
  Substituting $\hat{\sigma}^2$ back into the log-likelihood function:
  $$\ln(\hat{L}) = -\frac{n}{2}\ln(2\pi) - \frac{n}{2}\ln(\text{MSE}_{\text{train}}) - \frac{n}{2\hat{\sigma}^2} \left( n \hat{\sigma}^2 \right)$$
  $$\ln(\hat{L}) = -\frac{n}{2}\ln(\text{MSE}_{\text{train}}) - \frac{n}{2}\Big( \ln(2\pi) + 1 \Big)$$
  
  Multiplying by $-2$ and dropping constants independent of model complexity $k$:
  
  $$-2\ln(\hat{L}) = n \ln(\text{MSE}_{\text{train}})$$
  
  Substituting this result into the AIC and BIC definitions yields the standard regression formulations:
  
  $$\mathbf{\text{AIC} = n \ln(\text{MSE}_{\text{train}}) + 2k}$$
  $$\mathbf{\text{BIC} = n \ln(\text{MSE}_{\text{train}}) + k \ln(n)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Deconstructing the Information Criteria:                               │
  │                                                                        │
  │ • Goodness-of-Fit Term:   n · ln(MSE_train)                            │
  │   Measures how well the model minimizes in-sample residuals.           │
  │   Decreases monotonically as parameters k are added.                   │
  │                                                                        │
  │ • Complexity Penalty Term: +2k (AIC)  vs.  +k · ln(n) (BIC)            │
  │   Penalizes the degrees of freedom k to prevent over-parameterization. │
  │   Increases linearly as parameters k are added.                        │
  │                                                                        │
  │ • The Resulting Curve: The sum forms a U-shape, identifying the        │
  │   optimal balance where additional features cease to pay for their     │
  │   complexity cost.                                                     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Comparing Penalties: AIC vs. BIC
  
  ```
  ┌─────────────────────┬──────────────────────────┬──────────────────────────┐
  │ Criterion           │ Complexity Penalty       │ Asymptotic Behavior      │
  ├─────────────────────┼──────────────────────────┼──────────────────────────┤
  │ AIC                 │ + 2k                     │ Asymptotically Efficient │
  │                     │ (Constant weight: 2.0)   │ (Minimizes prediction    │
  │                     │                          │ error; tends to overfit) │
  ├─────────────────────┼──────────────────────────┼──────────────────────────┤
  │ BIC                 │ + k · ln(n)              │ Asymptotically Consistent│
  │                     │ (Grows with sample size) │ (Recovers the TRUE data- │
  │                     │                          │ generating model as n──▶∞│
  └─────────────────────┴──────────────────────────┴──────────────────────────┘
  ```
### Why BIC Penalizes Complexity More Heavily Than AIC:
  * Compare the penalty coefficients: $2$ versus $\ln(n)$.
  * Whenever sample size satisfies:
  $$\ln(n) > 2 \iff n > e^2 \approx 7.389 \implies \mathbf{n \ge 8}$$
  the multiplier $\ln(n)$ exceeds $2$.
  * In the Diabetes dataset ($n = 442$):
  $$\ln(442) \approx \mathbf{6.091}$$
  * **The Comparison:** For $n = 442$, BIC penalizes each additional feature with a weight of **$6.091 \cdot k$**, which is **more than three times heavier than AIC's penalty ($2 \cdot k$)**. As a result, BIC prefers more parsimonious (smaller) models than AIC.
  
  ---
## 5. Forward Stepwise Selection on the Diabetes Benchmark (Slide 11)
  
  Slide 11 implements **Forward Stepwise Feature Selection**:
  1. Begin with the null model containing only the intercept: $\hat{y} = w_0$.
  2. At each iteration, evaluate all unselected candidate features and add the single feature that produces the largest reduction in training Mean Squared Error.
  3. Repeat until all $d = 10$ features are included in the model.
  
  ```
                   Forward Stepwise Progression on Diabetes (Slide 11)
  ┌──────────────┬────────────────────┬────────────────────┬────────────────────┐
  │ Features (k) │ Training MSE       │ BIC Penalty Curve  │ 10-Fold CV MSE     │
  ├──────────────┼────────────────────┼────────────────────┼────────────────────┤
  │ 1 Feature    │ 3,750              │ 3,920              │ 3,850              │
  │ 2 Features   │ 3,200              │ 3,380              │ 3,300              │
  │ 3 Features   │ 3,050              │ 3,240              │ 3,180              │
  │ 4 Features   │ 2,950              │ 3,160              │ 3,090              │
  │ 5 Features   │ 2,900              │ 3,120              │ 3,020              │
  │ 6 Features   │ 2,880              │ 3,105  <-- MINIMUM │ 2,950  <-- MINIMUM │
  │ 7 Features   │ 2,870              │ 3,125              │ 2,960              │
  │ 8 Features   │ 2,860              │ 3,150              │ 2,970              │
  │ 9 Features   │ 2,855              │ 3,185              │ 2,980              │
  │ 10 Features  │ 2,850 <-- MINIMUM  │ 2,230              │ 3,000              │
  └──────────────┴────────────────────┴────────────────────┴────────────────────┘
  ```
  
  ---
### Why BIC and 10-Fold CV Agree (Picking 6 Features):
  * **Features 1 through 6:** Introduce strong predictive biomarkers (`bmi`, `s5` [triglycerides], `bp`, etc.). The drop in training residual error ($\Delta \text{MSE}$) is large enough to overcome the $+k\ln(n)$ complexity penalty in BIC, while genuinely improving generalization on validation folds in CV.
  * **Features 7 through 10:** Adding redundant or weakly correlated blood serum measurements yields only minor reductions in training MSE. 
  * In the BIC equation, the marginal reduction in $n\ln(\text{MSE})$ is smaller than the $+6.09$ penalty incurred by adding another parameter, causing the BIC curve to slope upward.
  * In 10-fold CV, the extra parameters increase estimation variance, causing out-of-fold MSE to rise from $2{,}950$ to $3{,}000$.
  * Both analytical penalization (BIC) and empirical resampling (10-fold CV) converge on the exact same parsimonious subset: **6 features**.
  
  ---
## 6. Information Criteria vs. Cross-Validation: The Systems Trade-Off
  
  Slide 11 demonstrates that both methods reach identical conclusions on linear models. How should an engineer decide which approach to deploy in production?
  
  ```
  ┌───────────────────────┬────────────────────────────┬────────────────────────────┐
  │ Operational Metric    │ Information Criteria (BIC) │ Cross-Validation (k-Fold)  │
  ├───────────────────────┼────────────────────────────┼────────────────────────────┤
  │ Computational Cost    │ Minimal (𝒪(1) Single Fit): │ Heavy (𝒪(k) Multiple Fits):│
  │                       │ Fit model once on all data │ Fits k separate models on  │
  │                       │ and evaluate formula.      │ rotating fold partitions.  │
  ├───────────────────────┼────────────────────────────┼────────────────────────────┤
  │ Distributional Priors │ High (Parametric):         │ None (Non-Parametric):     │
  │                       │ Requires explicit log-     │ Assumption-free; works for │
  │                       │ likelihood & noise models. │ any arbitrary loss metric. │
  ├───────────────────────┼────────────────────────────┼────────────────────────────┤
  │ Model Applicability   │ Restricted to parametric   │ Universal: works on k-NN,  │
  │                       │ models (OLS, GLMs).        │ Random Forests, XGBoost,   │
  │                       │ Undefined for k-NN/trees!  │ and deep neural networks.  │
  ├───────────────────────┼────────────────────────────┼────────────────────────────┤
  │ Small Sample Scaling  │ Strong performance when N  │ Pessimistic bias if N is   │
  │                       │ is too small to split.     │ too small to partition.    │
  └───────────────────────┴────────────────────────────┴────────────────────────────┘
  ```
  
  **The Core Rule:** If working with parametric Generalized Linear Models where computing multiple training folds is computationally expensive, use **BIC**. When working with non-parametric algorithms (e.g., $k$-NN, Random Forests) or non-standard operational loss metrics (e.g., $F_1$, expected financial utility), **Cross-Validation** is the only viable method.
  
  ---
## Summary Review Questions for Section 3
  
  1. *Why does the training set Mean Squared Error continually decrease as features are added in forward stepwise selection, while the Bayesian Information Criterion (BIC) forms a U-shaped curve?*
  2. *Given a linear regression model trained on $n = 1{,}000$ samples, calculate the exact numerical weight assigned to each parameter $k$ in the BIC penalty ($k\ln(n)$). How many times larger is this penalty than the AIC penalty ($2k$)?*
  3. *Why is it impossible to evaluate the Bayesian Information Criterion (BIC) on a $k$-Nearest Neighbors classifier? Why does cross-validation succeed in that setting?*
  
  ---