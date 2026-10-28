## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why Training MSE Continually Drops While BIC Forms a U-Shape:**
   * Adding features expands the dimensionality of the hypothesis space ($\mathcal{H}_k \subset \mathcal{H}_{k+1}$). By the properties of unconstrained least squares, adding parameters can never increase the training sum of squared residuals ($\Delta \text{MSE}_{\text{train}} \le 0$). Training error is a monotonically non-increasing function of feature count $k$.
   * The Bayesian Information Criterion combines in-sample fit with an explicit complexity penalty:
     $$\text{BIC} = n \ln(\text{MSE}_{\text{train}}) + k \ln(n)$$
   * For the first 6 features, the reduction in residual error is large enough to overcome the penalty ($|\Delta n \ln(\text{MSE})| > \ln(n)$). Beyond 6 features, the minor reduction in training error fails to offset the $+k\ln(n)$ penalty. The penalty dominates, forcing the BIC curve to rise and forming a U-shape that identifies the parsimonious model.
  2. **Evaluating the BIC Complexity Penalty for $n = 1{,}000$:**
   * For sample size $n = 1{,}000$, the parameter multiplier is:
     $$\text{Multiplier}_{\text{BIC}} = \ln(1{,}000) \approx \mathbf{6.908}$$
   * The AIC penalty assigns a constant weight of $2.0$ per parameter.
   * Comparing the penalties:
     $$\frac{\text{Penalty}_{\text{BIC}}}{\text{Penalty}_{\text{AIC}}} = \frac{6.908}{2.000} = \mathbf{3.454\times \text{ heavier}}$$
   * For $n = 1{,}000$, BIC penalizes each parameter nearly $3.5\times$ more aggressively than AIC, preferring more conservative, smaller feature subsets.
  3. **Why BIC Cannot Be Evaluated on $k$-Nearest Neighbors:**
   * Information criteria (AIC, BIC) are derived from **likelihood theory** under parametric distribution assumptions:
     $$\text{BIC} = k\ln(n) - 2\ln(\hat{L})$$
   * A $k$-NN classifier is non-parametric and instance-based: it does not estimate parametric likelihood functions $\hat{L}$, nor does it have a fixed integer parameter count $k$.
   * Cross-validation is non-parametric and assumption-free. It evaluates empirical predictive error directly on held-out validation folds, making it universally applicable to any algorithm ($k$-NN, Random Forests, Neural Networks).
  
  ---
## 2. Comparing Models Honestly: Paired Resampling Diagnostics (Slide 12)
  
  Slide 12 presents the gold standard for comparing two competing model families:
  
  $$\mathbf{\text{"Same splits, same preprocessing, same metric."}}$$
  $$\mathbf{\text{"LR leads 5-NN by 0.009 on average; the fold-to-fold spread is about 0.02. Report mean } \pm \text{ std on the same splits."}}$$
  
  ```
                      Model Comparison Across 50 Folds (Slide 12)
    Accuracy per Fold (10 Repeats of 5-Fold CV)
       ▲
  1.00 │      ●● ●●●●●                    ● ●●●●                      ● ●●
       │     ●●●●●●●●                    ●●●●●●●                     ●●●●●●
  0.975┼───[ 0.974 ± 0.016 ]───────[ 0.965 ± 0.018 ]────────────      │
       │         │                         │               [ 0.941 ± 0.026 ]
  0.950┼         │                         │                       │
       │         │                         │                       │
  0.925┼         │                         │                       │
       │         │                         │                       │
  0.875┴─────────┴─────────────────────────┴───────────────────────┴────────▶ Models
            Logistic Regression              5-NN               Gaussian Naive Bayes
  ```
  
  ```
                 Paired Difference Distribution: (LR - 5-NN) (Slide 12)
    Folds Count
       ▲
    15 │                                  ███  Mean Difference: +0.009
       │                                  ███
    10 │                        ███       ███       ███
       │                   ███  ███       ███       ███
     5 │              ███  ███  ███  ███  ███  ███  ███
       │         ███  ███  ███  ███  ███  ███  ███  ███  ███  ███
     0 ┼──────────┴────┴────┴────┴────┴────┴────┴────┴────┴────┴────────▶ Difference
                -0.05                 0.00                +0.05
           ◄── 5-NN Better                        LR Better ──►
  ```
  
  ---
### A. The Methodological Requirement of Identical Splits
  When comparing Logistic Regression against 5-NN, an engineer cannot evaluate Model A on one random seed and Model B on another. 
  * As proven in Lecture 18, a random 80/20 split introduces a $7.0\%$ performance swing purely due to partition noise.
  * If Model A scores $97.4\%$ on Seed 0 and Model B scores $96.5\%$ on Seed 42, the $0.9\%$ lead is indistinguishable from random partitioning luck.
  * **The Controlled Invariant:** Both models must be evaluated on the **exact same cross-validation folds** (`random_state=0` in `RepeatedStratifiedKFold`).
  
  ---
### B. Analyzing the Paired Difference Metric ($d_i$)
  To determine whether Logistic Regression is genuinely superior to 5-NN, we evaluate the **paired difference** on each fold $i \in \{1, \dots, 50\}$:
  
  $$d_i = \text{Score}_{\text{LR}, i} - \text{Score}_{\text{5-NN}, i}$$
  
  * **Mean Paired Lead ($\bar{d}$):**
  $$\bar{d} = \frac{1}{50}\sum_{i=1}^{50} d_i = \mathbf{+0.009 \quad (+0.9\%)}$$
  * **Fold-to-Fold Spread ($s_d$):**
  The standard deviation of the paired difference is approximately **$s_d \approx 0.020$**.
  * **Statistical Interpretation:**
  * Notice the zero line ($d = 0.00$) in Slide 12's histogram. 
  * While the distribution centers at $+0.009$, the spread extends from $-0.04$ to $+0.06$.
  * On roughly **one-third of the folds, 5-NN actually outperformed Logistic Regression** ($d_i < 0$).
  * A lead of $0.009$ with a standard deviation of $0.020$ indicates that the performance difference is marginal. Reporting the mean without the standard deviation conceals this overlap.
  
  ---
## 3. Practice P1: `GridSearchCV` Mechanics with Pipelines (Slides 13–14)
  
  Slide 14 implements a multi-parameter grid search tuning both neighborhood size ($k$) and distance weighting for a $k$-NN classifier:
  
  ```python
  from sklearn.datasets import load_breast_cancer
  from sklearn.neighbors import KNeighborsClassifier
  from sklearn.model_selection import (
    train_test_split,
    StratifiedKFold,
    GridSearchCV,
  )
  from sklearn.pipeline import make_pipeline
  from sklearn.preprocessing import StandardScaler
  
  # 1. Setup Data & Vault 20% Test Partition
  X, y = load_breast_cancer(return_X_y=True)
  y = 1 - y  # 1 = Malignant, 0 = Benign
  
  X_tr, X_te, y_tr, y_te = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
  )
  
  # 2. Build Pipeline: StandardScaler + KNeighborsClassifier
  pipe = make_pipeline(StandardScaler(), KNeighborsClassifier())
  
  # 3. Define Cartesian Parameter Grid (Using Pipeline Step Prefix)
  grid = {
    "kneighborsclassifier__n_neighbors": list(range(1, 32, 2)),  # 16 odd values
    "kneighborsclassifier__weights": ["uniform", "distance"],  # 2 weighting options
  }
  
  # 4. Enforce Stratified 5-Fold Cross-Validation
  cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=0)
  
  # 5. Execute Exhaustive Search Across All Folds
  gs = GridSearchCV(pipe, grid, cv=cv).fit(X_tr, y_tr)
  
  # 6. Audit Diagnostic Outputs
  print("best params:", gs.best_params_)
  print("fits:       ", len(gs.cv_results_["params"]) * cv.get_n_splits())
  print(f"test (once) : {gs.score(X_te, y_te):.3f}")
  ```
  
  ```
                   Empirical Diagnostic Output (Slide 14)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ best params : {'kneighborsclassifier__n_neighbors': 11,               │
  │                'kneighborsclassifier__weights': 'distance'}            │
  │ fits        : 160                                                      │
  │ test (once) : 0.965                                                    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Systems Breakdown of the Execution:
#### 1. The Double-Underscore Parameter Routing Syntax (`step__param`)
  * When an estimator is wrapped inside a scikit-learn `Pipeline`, passing parameter names directly (e.g., `n_neighbors=5`) causes a `ValueError`.
  * Scikit-learn uses the **double-underscore convention**:
  $$\texttt{lowercase\_step\_name} + \texttt{"\_\_"} + \texttt{parameter\_name}$$
  `kneighborsclassifier__n_neighbors` directs `GridSearchCV` to route the hyperparameter directly to the `KNeighborsClassifier` stage of the pipeline without modifying the preceding `StandardScaler`.
#### 2. Total Training Fits Calculation:
  * Values of $k$: Odd integers from $1$ to $31 \implies \mathbf{16 \text{ values}}$.
  * Weight schemes: `['uniform', 'distance']` $\implies \mathbf{2 \text{ values}}$.
  * Total Cartesian configurations: $16 \times 2 = \mathbf{32 \text{ settings}}$.
  * Folds per setting: $5$ folds.
  $$\text{Total Fits} = 32 \times 5 = \mathbf{160 \text{ independent models trained from scratch}}$$
#### 3. Step 4 Final Test Scoring:
  `gs.score(X_te, y_te)` evaluates the **vaulted test set exactly once** using the refitted winning pipeline ($k = 11$, `'distance'`), reporting an unbiased accuracy of **$96.5\%$**.
  
  ---
## 4. Practice P2: Linear vs. Logarithmic Search Grids (Slide 15)
  
  Slide 15 addresses a common grid design error: searching scale-dependent hyperparameters across a linear grid instead of a logarithmic grid.
  
  ```python
  import numpy as np
  from sklearn.linear_model import SGDClassifier
  
  # SGDClassifier with log_loss implements Logistic Regression trained via SGD
  # alpha corresponds directly to the regularization penalty λ
  logreg = SGDClassifier(
    loss="log_loss",
    average=True,
    max_iter=1000,
    tol=None,
    random_state=0,
  )
  pipe = make_pipeline(StandardScaler(), logreg)
  cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=0)
  
  # Compare Linear vs. Logarithmic Grids (7 points each)
  lams = {
    "linear": np.linspace(1e-5, 10, 7),  # 0.00001, 1.67, 3.33, ..., 10.0
    "log": np.logspace(-5, 1, 7),  # 0.00001, 0.0001, 0.001, ..., 10.0
  }
  
  for name, lam in lams.items():
    gs = GridSearchCV(pipe, {"sgdclassifier__alpha": lam}, cv=cv).fit(
        X_tr, y_tr
    )
    print(f"{name:6s} lambda: ", " ".join(f"{v:.3g}" for v in lam))
    acc = gs.cv_results_["mean_test_score"]
    print(f"{name:6s} acc:    ", " ".join(f"{a:.3f}" for a in acc))
  ```
  
  ```
                   Linear vs. Log Grid Execution Profile (Slide 15)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ LINEAR GRID: np.linspace(1e-5, 10, 7)                                  │
  │ λ values: 0.00001   1.67     3.33     5.00     6.67     8.33     10.0  │
  │ CV Acc  : 0.958     0.782    0.741    0.710    0.685    0.662    0.640 │
  │ Best Accuracy: 0.958  (Selected λ = 1e-5)                              │
  ├────────────────────────────────────────────────────────────────────────┤
  │ LOGARITHMIC GRID: np.logspace(-5, 1, 7)                                │
  │ λ values: 1e-05     0.0001   0.001    0.01     0.1      1        10    │
  │ CV Acc  : 0.958     0.971    0.976    0.971    0.932    0.812    0.640 │
  │ Best Accuracy: 0.976  (Selected λ = 0.001)                             │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Why the Linear Grid Fails:
  Look at the candidate points generated by `np.linspace(1e-5, 10, 7)`:
  * **$5$ out of the $7$ candidate values** sit above $1.67$.
  * At $\lambda \ge 1.67$, the regularization penalty is so heavy that weights are forced to near-zero. The model underfits severely, yielding poor accuracies ($64\%\text{–}78\%$).
  * The linear grid tested only **one single point** below $1.0$ ($0.00001$).
  * **The Defect:** It completely missed the critical transition region between $0.0001$ and $0.1$.
  * The logarithmic grid sampled across exponential orders of magnitude ($10^{-5}, 10^{-4}, \dots, 10^1$), successfully identifying the optimal sweet spot at **$\lambda = 0.001$ (Accuracy = $97.6\%$)**.
  
  ---
## 5. Practice P3: Auditing `cv_results_` — Is the Winner Really Better? (Slide 16)
  
  Slide 16 explores what happens under the hood of `GridSearchCV` by inspecting the full diagnostic DataFrame:
  
  ```python
  import pandas as pd
  
  # Extract full results dictionary into a clean Pandas DataFrame
  res = pd.DataFrame(gs.cv_results_).rename(columns=lambda c: c.split("__")[-1])
  cols = [
    "n_neighbors",
    "weights",
    "mean_test_score",
    "std_test_score",
    "rank_test_score",
  ]
  
  # Sort by validation rank and inspect the top 6 models
  print(res[cols].sort_values("rank_test_score").head(6).round(3))
  ```
  
  ```
                   Top 6 Model Configurations (Slide 16)
  ┌──────┬─────────────┬──────────┬─────────────────┬────────────────┬─────────────────┐
  │ Rank │ n_neighbors │ weights  │ mean_test_score │ std_test_score │ Statistical Sig │
  ├──────┼─────────────┼──────────┼─────────────────┼────────────────┼─────────────────┤
  │ 1    │ 11          │ distance │ 0.976           │ 0.014          │ WINNER (Mean)   │
  │ 2    │ 13          │ distance │ 0.974           │ 0.016          │ Overlaps 1-SE   │
  │ 3    │ 9           │ distance │ 0.974           │ 0.015          │ Overlaps 1-SE   │
  │ 4    │ 7           │ distance │ 0.971           │ 0.018          │ Overlaps 1-SE   │
  │ 5    │ 15          │ distance │ 0.971           │ 0.012          │ Overlaps 1-SE   │
  │ 6    │ 11          │ uniform  │ 0.969           │ 0.017          │ Overlaps 1-SE   │
  └──────┴─────────────┴──────────┴─────────────────┴────────────────┴─────────────────┘
  ```
  
  Slide 16 asks:
  $$\mathbf{\text{"Is the winner really better?"}}$$
  
  ---
### The One-Standard-Error Rule (Breiman et al., 1984):
  * Rank 1 ($k = 11$, `'distance'`) achieved a mean score of **$0.976$** with a fold standard deviation of **$\pm 0.014$**.
  * Rank 2 ($k = 13$) achieved a mean score of **$0.974$**.
  * The difference between Rank 1 and Rank 2 is:
  $$\Delta = 0.976 - 0.974 = \mathbf{0.002 \quad (0.2\%)}$$
  * Because this $0.2\%$ margin is far smaller than the fold-to-fold standard deviation ($1.4\%$), the performance gap between Rank 1 and Rank 2 is statistically indistinguishable from sampling noise.
  * **Breiman's One-Standard-Error ($1\text{-SE}$) Rule:**
  $$\text{Select the simplest model whose performance falls within } 1\text{-SE of the empirical winner.}$$
  Instead of automatically picking the #1 ranked model on a fraction of a percent, engineers apply Occam's razor, selecting the configuration that is simpler, more regularized, or computationally faster.
  
  ---
## Complete Lecture 20 Synthesis Reference
  
  | Concept | Systems & Algorithmic Principle | Common Pitfall / Diagnostic Safeguard |
  | :--- | :--- | :--- |
  | **Parameters ($\theta$)** | Internal weights learned via `.fit()` (e.g., OLS weights, split thresholds). | In scikit-learn, learned parameters always end with an underscore: `coef_`, `intercept_`. |
  | **Hyperparameters ($\lambda$)** | External capacity knobs set before `.fit()` (e.g., $k$, $\alpha$, tree depth). | Can never be tuned on training data; training error always chooses maximum capacity. |
  | **The Tripartite Split** | Train (60% parameters) $\to$ Val (20% hyperparameters) $\to$ Test (20% evaluation). | Evaluating multiple hyperparameters on the test set converts it into an uncalibrated validation set. |
  | **Cartesian Grid Search** | Exhaustive search over $|\mathcal{G}| = \prod |G_m|$ configurations across $k$ folds. | Scales combinatorially ($N_{\text{fits}} = k \cdot |\mathcal{G}|$). Discretize only capacity-defining parameters. |
  | **Metric Alignment** | The winning hyperparameter depends on the evaluation metric. | On Breast Cancer, Precision selects $k = 23$ ($FP \downarrow$); Recall selects $k = 3$ ($FN \downarrow$). Always declare `scoring=` explicitly. |
  | **The 5-Step Workflow** | Split $\to$ Tune $\to$ Refit $\to$ Evaluate $\to$ Deploy. | `GridSearchCV(refit=True)` automates tuning on CV folds and refits the winner on $100\%$ of `X_train`. |
  | **BIC vs. 10-Fold CV** | BIC penalizes complexity analytically ($+k\ln(n)$); CV measures error directly. | On Diabetes, both agree on 6 features. Use BIC for fast GLM selection; use CV for non-parametric models. |
  | **Paired Model Tests** | Evaluate competing models on identical cross-validation fold splits. | Compute paired difference $d_i = s_{A, i} - s_{B, i}$. If spread spans zero, the lead may be noise. |
  | **Linear vs. Log Grids** | Scale parameters ($\alpha, \lambda, C$) operate over orders of magnitude. | `np.linspace` wastes evaluations in over-regularized regimes; always use `np.logspace`. |
  | **One-Standard-Error Rule**| Account for validation variance ($\hat{\sigma}_{\text{CV}}$) when selecting winners. | If a simpler, more regularized model sits within $1\text{-SE}$ of the empirical top score, prefer the simpler model. |
  
  ---
## Summary Review Questions for Section 4
  
  1. *Why is comparing two machine learning models using independent random splits invalid, and how does evaluating paired differences ($d_i = \text{Score}_{A, i} - \text{Score}_{B, i}$) on identical cross-validation folds resolve this issue?*
  2. *In Practice P2 (Slide 15), why did searching $\lambda$ across a linear grid from $10^{-5}$ to $10.0$ yield an inferior model ($\text{Accuracy} = 0.958$) compared to searching across a logarithmic grid ($\text{Accuracy} = 0.976$)?*
  3. *State Breiman’s One-Standard-Error ($1\text{-SE}$) Rule in your own words. Why might an engineer select the #2 or #3 ranked model from `cv_results_` instead of the #1 ranked model?*
  ---