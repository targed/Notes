## 1. Context & Motivation: The Single-Split Lottery (Slide 2)
  
  Slide 2 introduces an empirical demonstration that challenges the common practice of relying on a single train/test split:
  
  $$\mathbf{\text{"Why } k\text{-Fold? One Split Is Mostly Luck."}}$$
  $$\mathbf{\text{"Same model, same 569 tumors. Only random\_state changes in an 80/20 split, 100 times."}}$$
  
  ```
                      The Single-Split Variance Hazard (Slide 2)
  Single 80/20 Split (100 Seeds):
      93.0%                                              100.0%
  ─────●───────●────────●──────●────────|──────|───────|────●───▶ Accuracy
                                      96.8%  97.8%   98.4%
                                         ▲      ▲      ▲
  5-Fold CV Mean (100 Reshuffles):       └───┬──┴──┬───┘
                                       Stays tightly bounded
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Empirical Results on Breast Cancer Wisconsin (N = 569, Scaled LogReg): │
  │                                                                        │
  │ • Single 80/20 Split:  Accuracy spans from 93.0% to 100.0%             │
  │                        (A 7.0-PERCENTAGE-POINT SWING!)                 │
  │                                                                        │
  │ • 5-Fold CV Mean:      Mean accuracy spans from 96.8% to 98.4%         │
  │                        (Compressed into a stable 1.6% interval)        │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### The Statistical Pathology of a Single Split:
  When an engineer partitions a dataset $\mathcal{D}$ of size $N$ into an $80\%$ training set ($N_{\text{tr}} = 455$) and a $20\%$ test set ($N_{\text{te}} = 114$), the reported test accuracy $\hat{R}_{\text{te}}$ is a **random variable conditioned on the random seed of the split generator**:
  
  $$\hat{R}_{\text{te}}(\text{seed}) = \frac{1}{N_{\text{te}}} \sum_{i \in \mathcal{D}_{\text{test}}(\text{seed})} \mathbf{1}_{\{y_i = \hat{f}(x_i)\}}$$
  
  1. **The "Lucky" Split ($\text{Accuracy} = 100.0\%$):**
   * By random chance, the sampling algorithm routes all $10$ boundary edge-case tumors (ambiguous nuclear atypia) into the training set, while populating the test partition with easily separable cases.
   * **Result:** The engineer reports near-perfect performance, leading to false confidence.
  2. **The "Unlucky" Split ($\text{Accuracy} = 93.0\%$):**
   * By random chance, a disproportionate number of noisy, unrepresentative outliers are concentrated into the test partition.
   * **Result:** A competitive model architecture is discarded because of an unfavorable test split draw.
  3. **The Core Takeaway:** A single hold-out score on small-to-moderate datasets ($N < 5{,}000$) measures **sample partition noise** as much as it measures true model quality.
  
  ---
## 2. Defining $k$-Fold Cross-Validation (Slide 3)
  
  Slide 3 presents the formal definition of the $k$-fold cross-validation algorithm:
  
  $$\mathbf{\text{"A way to estimate generalization from the training set alone."}}$$
  
  ```
                       The 3-Step k-Fold Algorithm (Slide 3)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. SPLIT:                                                              │
  │    Partition the training set randomly into k disjoint, equal-sized   │
  │    subsets (folds): 𝒟_train = 𝒟₁ ∪ 𝒟₂ ∪ ... ∪ 𝒟_k                      │
  │                                                                        │
  │ 2. ROTATE:                                                             │
  │    For fold index i = 1, 2, ..., k:                                    │
  │      • Surrogate Training Set:  𝒟_(-i) = 𝒟_train \ 𝒟_i   (k - 1 folds) │
  │      • Validation Fold:         𝒟_i                     (1 fold)      │
  │      • Fit model:               f̂_(-i) = Train(𝒟_(-i))                 │
  │      • Score performance:       s_i = Metric(f̂_(-i), 𝒟_i)              │
  │                                                                        │
  │ 3. AVERAGE:                                                            │
  │    Aggregate validation scores across all k out-of-fold iterations:    │
  │                        CV = (1 / k) ∑_{i=1}^k s_i                      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 3 specifies the mathematical estimator:
  
  $$\mathbf{\text{CV} = \frac{1}{k} \sum_{i=1}^k s_i}$$
  
  where $s_i$ is the empirical score evaluated on fold $\mathcal{D}_i$ by a surrogate model that **never saw fold $\mathcal{D}_i$ during training**.
  
  Slide 3 concludes with a fundamental rule of data hygiene:
  $$\mathbf{\text{"The test set stays locked. Cross-validation happens entirely inside the training data."}}$$
  
  ---
## 3. Geometric Execution Graph of $k$-Fold CV (Slide 4)
  
  Slide 4 diagrams the rotation of validation splits across $k = 5$ folds on the Wisconsin dataset ($N_{\text{tr}} = 455$ training tumors, partitioned into $5$ folds of $91$ instances each):
  
  ```
                     The 5-Fold Cross-Validation Geometry (Slide 4)
  Fold 1 Held Out: [ VALIDATE ] [   TRAIN  ] [   TRAIN  ] [   TRAIN  ] [   TRAIN  ] ──▶ s₁ = 0.967
  Fold 2 Held Out: [   TRAIN  ] [ VALIDATE ] [   TRAIN  ] [   TRAIN  ] [   TRAIN  ] ──▶ s₂ = 0.978
  Fold 3 Held Out: [   TRAIN  ] [   TRAIN  ] [ VALIDATE ] [   TRAIN  ] [   TRAIN  ] ──▶ s₃ = 0.989
  Fold 4 Held Out: [   TRAIN  ] [   TRAIN  ] [   TRAIN  ] [ VALIDATE ] [   TRAIN  ] ──▶ s₄ = 0.989
  Fold 5 Held Out: [   TRAIN  ] [   TRAIN  ] [   TRAIN  ] [   TRAIN  ] [ VALIDATE ] ──▶ s₅ = 0.967
  ```
### Under the Hood of the Rotation:
  * **Total Training Iterations:** Exactly **$k$ independent models** are trained from scratch.
  * **Sample Coverage:** Every single training observation appears in a training set exactly $k - 1$ times, and appears in an evaluation hold-out set **exactly once**.
  * **Evaluating the 5 Folds from Slide 4:**
  $$\text{Mean Score } (\hat{\mu}_{\text{CV}}) = \frac{0.967 + 0.978 + 0.989 + 0.989 + 0.967}{5} = \frac{4.890}{5} = \mathbf{0.978}$$
  $$\text{Sample Std Dev } (\hat{\sigma}_{\text{CV}}) = \sqrt{\frac{1}{k - 1}\sum_{i=1}^k (s_i - \hat{\mu})^2} = \mathbf{0.010}$$
  $$\mathbf{\text{Reported Metric: } 0.978 \pm 0.010}$$
  
  ---
## 4. Why Variance Reporting Matters: Standard Deviation of CV Scores (Slide 4)
  
  Slide 4 emphasizes an essential principle of model evaluation:
  
  $$\mathbf{\text{"Report mean } \pm \text{ std. } \mathbf{0.98 \pm 0.01} \text{ and } \mathbf{0.98 \pm 0.10} \text{ are not the same model."}}$$
  
  ```
                       The Reliability Profile Comparison
          Model A: 0.98 ± 0.01 (Stable)                 Model B: 0.98 ± 0.10 (Fragile)
      Accuracy                                      Accuracy
        ▲                                             ▲
    1.0 │          (Narrow, tight band)           1.0 │       ● (Fold 3 = 1.00)
        │      ● s₃ = 0.99                            │
    0.9 │   ●─●──● s₂, s₄, s₅ = 0.98              0.9 │   ●───● s₁ = 0.98 (Mean)
        │      ● s₁ = 0.97                            │
    0.8 │                                         0.8 │
        │                                             │
    0.7 │                                         0.7 │       ● (Fold 5 = 0.88!)
        └───────────────────────────                  └───────────────────────────
        Predictable across sub-populations            Hypersensitive to data partition
  ```
  
  ---
### What Does the Standard Deviation of CV Scores Actually Measure?
  A common misconception is that the standard deviation $\hat{\sigma}_{\text{CV}}$ represents the standard error of the mean. In machine learning, $\hat{\sigma}_{\text{CV}}$ measures **algorithmic stability**:
  
  $$\hat{\sigma}_{\text{CV}} = \sqrt{\frac{1}{k - 1}\sum_{i=1}^k (s_i - \bar{s})^2}$$
  
  * **Case 1: Low Variance ($0.98 \pm 0.01$):**
  * The model is **robust to training sample perturbations**.
  * Dropping $20\%$ of the data and retraining produces virtually the same decision boundaries. The model captures structural invariants that generalize uniformly across subpopulations.
  * **Case 2: High Variance ($0.98 \pm 0.10$):**
  * The model is **unstable**.
  * On some folds it scores $100\%$, while on others it drops to $88\%$. This indicates that the algorithm relies on specific subset samples to learn its parameters, or that the feature representation is susceptible to local noise.
  * In production, Model B is a high-risk candidate: when exposed to real-world distribution fluctuations, its operational accuracy can drop significantly below the mean.
  
  ---
## Summary Review Questions for Section 1
  
  1. *Why does reporting a single 80/20 train/test split score produce a 7-percentage-point swing ($93\%$ to $100\%$) across different random seeds on the Breast Cancer dataset, while 5-fold cross-validation remains bounded within a 1.6% interval?*
  2. *In $k$-fold cross-validation, how many times is each training observation used to train a model, and how many times is it used to evaluate a model?*
  3. *If Model 1 achieves a 5-fold CV score of $0.85 \pm 0.01$ and Model 2 achieves $0.87 \pm 0.09$, why might an engineer select Model 1 for safety-critical deployment despite its slightly lower mean score?*
  
  ---