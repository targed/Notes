## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Parameters vs. Hyperparameters:**
   * **Parameters ($\theta$):** Internal variables (e.g., weights $w$, bias $b$, split thresholds) learned directly from training data by minimizing an empirical loss function:
     $$\theta^*(\lambda) = \arg\min_{\theta} \mathcal{L}_{\text{train}}(\theta; \lambda)$$
   * **Hyperparameters ($\lambda$):** External architectural constraints (e.g., neighborhood size $k$, penalty $\alpha$, polynomial degree $d$) that dictate the capacity of the hypothesis space $\mathcal{H}_\lambda$. They cannot be learned from training data and must be selected using held-out validation data:
     $$\lambda^* = \arg\min_{\lambda \in \Lambda} \mathcal{L}_{\text{val}}(\theta^*(\lambda))$$
  2. **The Scikit-Learn Trailing Underscore Invariant:**
   * In `scikit-learn`, any attribute ending with a **trailing underscore** (e.g., `model.coef_`, `model.intercept_`, `model.classes_`, `model.n_features_in_`) represents a **learned parameter** estimated from training data during `.fit()`.
   * Attributes without an underscore (e.g., `model.C`, `model.n_neighbors`, `model.alpha`) are **pre-set hyperparameters** passed into the constructor before fitting begins.
  3. **Why Training Error Maximizes Complexity:**
   * Model classes are typically **nested**: $\mathcal{H}_1 \subset \mathcal{H}_2 \subset \dots \mathcal{H}_d$. 
   * A model with higher flexibility contains all simpler models as constrained special cases. An unconstrained optimization algorithm minimizing training error will always exploit extra degrees of freedom to interpolate training noise ($\hat{R}_{\text{train}} \to 0$). Training error is a monotonically non-increasing function of capacity and will always select the most complex configuration available.
  
  ---
## 2. Exhaustive Cartesian Grid Search Mechanics (Slide 8)
  
  Slide 8 details the standard brute-force search technique for hyperparameter optimization:
  
  $$\mathbf{\text{"Try every combination on a grid; score each one with } k\text{-fold CV."}}$$
  $$\mathbf{12 \text{ degrees} \times 11 \text{ values of } \lambda \times 10 \text{ folds} = \mathbf{1{,}320 \text{ fits. Search }} \lambda \mathbf{\text{ on a log scale."}}}$$
  
  ```
                The 2D Hyperparameter Grid Space (Slide 8)
    Ridge λ (Alpha)
       ▲
   100 │  0.32  0.31  0.30  0.28  0.28  0.27  0.27  0.27  0.27  0.27  0.27  0.27  (High Bias)
    10 │  0.31  0.29  0.27  0.25  0.25  0.24  0.24  0.24  0.24  0.24  0.24  0.24
     1 │  0.28  0.24  0.20  0.18  0.18  0.17  0.17  0.17  0.17  0.17  0.17  0.17
   0.1 │  0.25  0.18  0.12  0.08  0.08  0.08  0.08  0.08  0.08  0.08  0.08  0.08
  0.01 │  0.20  0.12  0.06  0.03  0.03  0.03  0.03  0.03  0.03  0.03  0.03  0.03
  1e-3 │  0.18  0.09  0.04  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01
  1e-4 │  0.16  0.08  0.03  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01
  1e-5 │  0.15  0.07  0.02  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01
  1e-6 │  0.14  0.07  0.02  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01
  1e-7 │  0.14  0.07  0.02  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01  0.01
  1e-8 │  0.14  0.07  0.02 ┌──────┐ 0.01  0.01  0.01  0.01  0.01  0.01  0.01  (Low Bias)
       └───────────────────│0.0085│───────────────────────────────────────────▶ Degree
         1     2     3     └──────┘  5     6     7     8     9    10    11    12
                           Lowest CV MSE: Degree 4, λ = 1e-8
  ```
  
  ---
### A. The Combinatorial Scaling of Grid Search
  Let an engineer search across $M$ distinct hyperparameters, where each hyperparameter $\lambda_m$ is discretized into a candidate set of values $G_m$:
  
  $$\mathcal{G} = G_1 \times G_2 \times \dots \times G_M$$
  
  * **Total Hyperparameter Configurations ($|\mathcal{G}|$):**
  $$|\mathcal{G}| = \prod_{m=1}^M |G_m|$$
  * **Total Training Cycles ($N_{\text{fits}}$):**
  When evaluating each configuration using $k$-fold cross-validation:
  $$\mathbf{N_{\text{fits}} = k \times \prod_{m=1}^M |G_m|}$$
  * In Slide 8's case study:
  $$N_{\text{fits}} = 10 \text{ folds} \times (12 \text{ degrees}) \times (11 \text{ penalties}) = 10 \times 132 = \mathbf{1{,}320 \text{ distinct model fits}}$$
  
  ---
### B. Analyzing the 2D Capacity Interaction (Slide 8 Heatmap)
  The heatmap reveals how two independent hyperparameters interact to control total model capacity:
  1. **The High-Bias Region (Top Left):** Low polynomial degree ($d = 1$) combined with strong regularization ($\lambda = 100$) produces the highest cross-validation error ($\text{MSE} \approx 0.32$). The model is doubly constrained.
  2. **The Diagonal Equivalence Manifold:** Notice that a higher-degree polynomial ($d = 12$) paired with moderate regularization ($\lambda = 0.01$) achieves a low error comparable to a degree-4 polynomial. Strong regularization can neutralize excess polynomial degrees by shrinking high-order coefficients to zero.
  3. **The Global Minimum:** The lowest 10-fold CV error ($\text{MSE} = 0.0085$) occurs at **$\text{Degree} = 4$ and $\lambda = 10^{-8}$**, aligning with the true underlying quartic function derived in Lecture 19.
  
  ---
## 3. Metric-Driven Tuning: Why the Winner Depends on the Metric (Slide 9)
  
  Slide 9 demonstrates that hyperparameter selection is **fundamentally metric-dependent**:
  
  $$\mathbf{\text{"Same } k\text{-NN grid, three different scoring= choices. The winner depends on the metric."}}$$
  $$\mathbf{\text{"Precision picks } k = 23\text{, Recall picks } k = 3\text{. Choose scoring= before you search."}}$$
  
  ```
               Cross-Validation Score vs. k Across Metrics (Slide 9)
    5-Fold CV Score
       ▲
  1.00 │                                            ● Best for Precision: k = 23 (Score ≈ 0.99)
       │         ● Best for Accuracy: k = 3          \
  0.95 │       ╭─╯\                                   ╰────────── Precision Curve
       │      ╭╯   ╰───────────────────────────────────────────── Accuracy Curve
  0.90 │     ● Best for Recall: k = 3
       │      \
  0.85 │       ╰───────────────────────────────────────────────── Recall Curve
       └─────────────────────────────────────────────────────────▶ Neighborhood Size k
       0         5         10        15        20        25        30
  ```
  
  ---
### Why Precision Favors Large $k$ ($k = 23$) while Recall Favors Small $k$ ($k = 3$):
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Why Recall Peaks at Small k (k = 3):                                │
  │    • Recall = TP / (TP + FN) measures coverage.                        │
  │    • Small neighborhoods are sensitive to local minority clusters.     │
  │    • If an ambiguous tumor is surrounded by even 2 malignant neighbors │
  │      out of 3, the model flags it as positive.                         │
  │    • False Negatives decrease (FN ──▶ 0), maximizing Recall.           │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Why Precision Peaks at Large k (k = 23):                            │
  │    • Precision = TP / (TP + FP) measures alert purity.                 │
  │    • Large neighborhoods require broad, consensus-level agreement.     │
  │    • An instance is flagged as malignant only if the local density is  │
  │      overwhelmingly positive across 23 surrounding samples.            │
  │    • Spurious false alarms are suppressed (FP ──▶ 0), maximizing       │
  │      Precision at the cost of missing isolated malignant cases.        │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **The Systems Rule:** If an engineer tunes $k$-NN using the default setting (`scoring=None`, which defaults to accuracy), the search wrapper will select $k = 3$. If the system is deployed for oncology screening, that choice is acceptable. If deployed in an operational setting where false alarms carry high costs, selecting $k = 3$ represents a failure of metric alignment. **The objective metric must be explicitly declared before executing grid search.**
  
  ---
## 4. The 5-Step Model Selection Workflow (Slide 10)
  
  Slide 10 formalizes the complete lifecycle required to tune, select, and deploy machine learning models in production:
  
  $$\mathbf{\text{"Five steps from raw data to a reported number. Steps 2 and 3 are one GridSearchCV call."}}$$
  
  ```
                       The 5-Stage Selection Pipeline (Slide 10)
  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
  │   1. SPLIT   │     │   2. TUNE    │     │   3. REFIT   │     │ 4. EVALUATE  │     │  5. DEPLOY   │
  ├──────────────┤     ├──────────────┤     ├──────────────┤     ├──────────────┤     ├──────────────┤
  │ Hold out a   │ ──▶ │ k-fold CV on │ ──▶ │ Best setting │ ──▶ │ ONCE, on the │ ──▶ │ (Optional)   │
  │ test set     │     │ the training │     │ on the WHOLE │     │ test set.    │     │ Refit on ALL │
  │ (20% vaulted)│     │ set across   │     │ training set │     │              │     │ data (100%). │
  │              │     │ every grid   │     │ (N_tr rows). │     │ Report this  │     │              │
  │              │     │ point.       │     │              │     │ final number.│     │ Ship to prod.│
  └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                      └─────────────────┬────────────────┘
                                        ▼
                           Executed Automatically by
                            GridSearchCV(refit=True)
  ```
  
  Slide 10 concludes with the core rule of model validation:
  $$\mathbf{\text{"Look at the test set once, at step 4. Going back to tune after that turns it into a validation set."}}$$
  
  ---
### Detailed Mechanics of Each Stage:
#### Step 1: Split (The Vaulting Barrier)
  * Partition raw data immediately: $80\%$ Training Set ($\mathcal{D}_{\text{train}}$) and $20\%$ Test Set ($\mathcal{D}_{\text{test}}$).
  * $\mathcal{D}_{\text{test}}$ is vaulted. It receives no feature transformations, imputation, or exploratory inspections.
#### Step 2: Tune (The Search Phase)
  * Execute $k$-fold cross-validation strictly within $\mathcal{D}_{\text{train}}$ across the Cartesian parameter space $\mathcal{G}$.
  * For each candidate parameter configuration $\lambda_g \in \mathcal{G}$, compute the out-of-fold validation score:
  $$\hat{\mu}_{\text{CV}}(\lambda_g) = \frac{1}{k}\sum_{i=1}^k s_i(\lambda_g)$$
  * Identify the winning hyperparameter configuration:
  $$\lambda^* = \arg\max_{\lambda_g \in \mathcal{G}} \hat{\mu}_{\text{CV}}(\lambda_g)$$
#### Step 3: Refit (Maximizing Statistical Power)
  * The $k$ surrogate models trained during Step 2 are discarded.
  * A fresh model instance initialized with hyperparameter $\lambda^*$ is trained on **$100\%$ of $\mathcal{D}_{\text{train}}$** ($N_{\text{tr}}$ samples). This ensures the deployed model benefits from the largest possible sample volume.
#### Step 4: Evaluate (The Honest Generalization Score)
  * Pass the vaulted test partition $\mathcal{D}_{\text{test}}$ through the refitted model from Step 3 **exactly once**:
  $$\text{Score}_{\text{final}} = \text{Metric}(f_{\lambda^*}, \; \mathcal{D}_{\text{test}})$$
  * This is the definitive number reported in research papers and project deliverables.
#### Step 5: Deploy (Optional Production Retraining)
  * Once the unbiased generalization score is reported in Step 4, an engineering team deploying to live production traffic may optionally execute a final refit of architecture $\lambda^*$ on **$100\%$ of all available data** ($\mathcal{D}_{\text{train}} \cup \mathcal{D}_{\text{test}}$). This maximizes sample volume for real-world serving.
  
  ---
## Summary Review Questions for Section 2
  
  1. *If an engineer searches across a grid containing 8 values of $k$, 3 distance metrics, and 2 weighting schemes using 5-fold cross-validation, exactly how many total models will be fitted during the tuning phase?*
  2. *In the Breast Cancer Wisconsin experiment on Slide 9, why did optimizing for Precision select a large neighborhood size ($k = 23$), while optimizing for Recall selected a small neighborhood size ($k = 3$)?*
  3. *What two specific stages of the 5-Stage Model Selection Workflow are executed automatically by scikit-learn when invoking `GridSearchCV(..., refit=True).fit(X_train, y_train)`?*
  
  ---