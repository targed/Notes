## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why Slack Variables ($\xi_i$) Cannot Solve Non-Linear Manifolds ("Slack Handles Noise, Not Shape"):**
   * Slack variables $\xi_i \ge 0$ introduce an arithmetic penalty into the objective function to permit boundary overlaps and accommodate label noise:
     $$y_i(w^T x_i + b) \ge 1 - \xi_i$$
   * However, the decision boundary $\{x \in \mathbb{R}^d \mid w^T x + b = 0\}$ remains an **affine linear hyperplane** (a flat line in 2D, a flat plane in 3D).
   * Slack variables allow individual points to cross this flat street, but they **cannot bend or curve the hyperplane itself**. Topologically non-linear manifolds (such as concentric circles or alternating XOR quadrants) require non-linear spatial transformations, not margin boundary relaxation.
  2. **Deriving a 2D Circle from a 3D Paraboloid Slicing Plane:**
   * Given the mapping $\phi(x) = [x_1, \; x_2, \; x_1^2 + x_2^2]^T = [x_1, \; x_2, \; z]^T \in \mathbb{R}^3$.
   * A flat linear decision plane in $\mathbb{R}^3$ satisfies:
     $$w_1 x_1 + w_2 x_2 + w_3 z + b = 0$$
   * For symmetric concentric rings centered at the origin, the optimal normal vector sets horizontal weights to zero ($w_1 = 0, w_2 = 0$), reducing the equation to:
     $$w_3 (x_1^2 + x_2^2) + b = 0 \implies x_1^2 + x_2^2 = -\frac{b}{w_3}$$
   * Setting $R^2 = -\frac{b}{w_3}$ projects the 3D plane back into the 2D input space as the standard equation of a circle:
     $$\mathbf{x_1^2 + x_2^2 = R^2}$$
  3. **Polynomial Dimension Expansion for $d = 4, M = 3$:**
   * Applying the combinations with replacement formula:
     $$D = \binom{d + M}{M} = \binom{4 + 3}{3} = \binom{7}{3} = \frac{7!}{3! \, (7 - 3)!} = \frac{7 \times 6 \times 5}{3 \times 2 \times 1} = \mathbf{35 \text{ dimensions}}$$
  
  ---
## 2. Scikit-Learn Systems Architecture: `SVC` vs. `LinearSVC` (Slide 16)
  
  Slide 16 details the two primary implementations of linear Support Vector Machines in `scikit-learn`:
  
  $$\mathbf{\text{"Put StandardScaler and the SVM in one pipeline, so the scaling is learned inside each CV fold."}}$$
  
  ```
                      The Two Linear SVM Implementations (Slide 16)
  ┌───────────────────────────────────────────────┬───────────────────────────────────────────────┐
  │ SVC(kernel="linear", C=...)                   │ LinearSVC(C=...)                              │
  ├───────────────────────────────────────────────┼───────────────────────────────────────────────┤
  │ • Underlying C++ Library: libsvm              │ • Underlying C++ Library: liblinear           │
  │ • Algorithmic Formulation: Dual Solver (SMO)  │ • Algorithmic Formulation: Primal Coordinate  │
  │ • Scales: 𝒪(n² · d) to 𝒪(n³)                  │   Descent / Trust Region Newton               │
  │ • Exposes Support Vectors: YES                │ • Scales: 𝒪(n · d) (Linear with sample size)  │
  │   (Stores .support_ and .support_vectors_)    │ • Exposes Support Vectors: NO                 │
  │ • Capacity: Optimal for small n (n ≤ 10,000)  │   (Optimizes primal directly; no αᵢ list)     │
  │ • Loss Default: Standard L1-Hinge Loss        │ • Capacity: Large datasets (n >> 100,000)     │
  │                                               │ • Loss Default: Squared Hinge Loss (L2-Hinge) │
  └───────────────────────────────────────────────┴───────────────────────────────────────────────┘
  ```
  
  ---
### A. The Algorithmic Divergence: Dual vs. Primal
  * **`SVC(kernel="linear")` (Dual Solver via LibSVM):**
  * Solves the **Lagrangian Dual** using Sequential Minimal Optimization (SMO):
    $$\max_\alpha \sum_{i=1}^n \alpha_i - \frac{1}{2}\sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y_i y_j x_i^T x_j$$
  * Because it computes the dual multipliers $\alpha_i$, it explicitly identifies and stores which training observations are active **Support Vectors** (`svm.support_`).
  * *The Computational Bottleneck:* Requires evaluating an $n \times n$ kernel Gram matrix. Memory and compute scale quadratically or cubically with sample size ($\mathcal{O}(n^2)\text{–}\mathcal{O}(n^3)$), making it slow when $n > 20{,}000$.
  * **`LinearSVC` (Primal Solver via LibLinear):**
  * Bypasses the dual formulation and solves the **Primal Problem** directly using coordinate descent:
    $$\min_{w, b} \frac{1}{2}\|w\|_2^2 + C \sum_{i=1}^n \max\Big(0, \; 1 - y_i(w^T x_i + b)\Big)^2$$
  * Its time complexity scales linearly with data size: $\mathcal{O}(n \cdot d)$.
  * *Trade-off:* Because it optimizes $w$ and $b$ directly without calculating the dual variables $\alpha_i$, **it does not store or expose a list of support vectors**.
  
  ---
### B. Why Feature Scaling Is Mandatory for SVMs (Slide 16)
  Slide 16 emphasizes:
  $$\mathbf{\text{"Put StandardScaler and the SVM in one pipeline."}}$$
  
  Recall the geometric margin width:
  $$\gamma = \frac{2}{\|w\|_2} = \frac{2}{\sqrt{\sum_{j=1}^d w_j^2}}$$
  
  * An SVM maximizes margin distance using the **Euclidean Norm in $\mathbb{R}^d$**.
  * If feature $x_1$ has a scale of $10^4$ and feature $x_2$ has a scale of $10^{-1}$, a unit change in $x_1$ represents a massive geometric distance.
  * The normal vector $w$ will shrink $w_1$ to near zero, aligning the decision boundary almost entirely along the high-variance feature axis.
  * **The Invariant:** Wrapping `StandardScaler()` and `SVC()` inside `make_pipeline()` ensures the feature space is isotropic ($\sigma_j = 1$) and prevents data leakage across cross-validation folds.
  
  ---
## 3. Deconstructing the Quick Check Quizzes (Slide 17)
  
  Slide 17 presents three diagnostic conceptual checks:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Q1: You delete a training point that is not a support vector and refit.│
  │     The boundary...                                                    │
  │     A: moves a little    B: does not move  ✔                           │
  │     C: gets wider margin D: flips                                      │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Q2: A fitted linear SVM has w = (3, 4). The margin width 2/||w|| is... │
  │     A: 0.2               B: 0.4  ✔                                     │
  │     C: 2.5               D: 5                                          │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Q3: You raise C from 0.01 to 100. Typically the margin...              │
  │     A: widens, more SVs  B: narrows, fewer SVs  ✔                      │
  │     C: does not change   D: disappears                                 │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Diagnostic Justifications:
  * **Q1 (Answer: B — Does Not Move):**
  * By KKT complementary slackness:
    $$\alpha_i^* \Big[ y_i(w^T x_i + b) - 1 + \xi_i \Big] = 0$$
  * Any observation lying strictly outside the margin has $\xi_i = 0$ and $y_i(w^T x_i + b) > 1$, forcing its dual multiplier to zero ($\alpha_i^* = 0$).
  * Since the parameter weights are defined as $w^* = \sum_{i \in \text{SVs}} \alpha_i^* y_i x_i$, deleting non-support vectors removes terms with zero coefficients. The Quadratic Program finds the exact same solution $(w^*, b^*)$.
  * **Q2 (Answer: B — $0.4$):**
  * Given $w = [3, 4]^T$.
  * Compute the Euclidean norm:
    $$\|w\|_2 = \sqrt{w_1^2 + w_2^2} = \sqrt{3^2 + 4^2} = \sqrt{9 + 16} = \sqrt{25} = \mathbf{5.0}$$
  * Evaluate the margin width formula:
    $$\text{Margin Width} = \frac{2}{\|w\|_2} = \frac{2}{5.0} = \mathbf{0.4}$$
  * **Q3 (Answer: B — Narrows, Fewer SVs):**
  * The hyperparameter $C$ scales the penalty on margin violations ($C \sum \xi_i$).
  * Raising $C$ from $0.01 \to 100$ penalizes slack violations heavily, forcing the optimizer toward hard-margin behavior.
  * To eliminate violations, the margin street contracts ($\|w\|_2$ grows, so $\gamma = \frac{2}{\|w\|_2}$ shrinks).
  * A narrower street covers less volume of the feature space, engulfing fewer observations inside its boundaries and resulting in **fewer active Support Vectors**.
  
  ---
## 4. Practice P1: Validating Support Vector Invariance (Slides 18–19)
  
  Slide 19 executes an experiment on a synthetic 40-point dataset to prove that Support Vectors alone govern the classifier:
  
  ```python
  import numpy as np
  from sklearn.datasets import make_blobs
  from sklearn.svm import SVC
  
  # 1. Generate 40 Linearly Separable Points (2 Classes)
  X, y = make_blobs(n_samples=40, centers=2, random_state=6)
  
  # 2. Fit Linear SVM with large C (Approximating a Hard Margin)
  svm = SVC(kernel="linear", C=1000).fit(X, y)
  w, b = svm.coef_[0], svm.intercept_[0]
  
  print("w:              ", w.round(3), " b:", round(b, 3))
  print("margin 2/||w||: ", round(2 / np.linalg.norm(w), 3))
  print("support vectors:", svm.support_)
  print("f(x) at SVs:    ", svm.decision_function(X[svm.support_]).round(3))
  
  # 3. Refit Model on the Support Vectors ALONE
  sv_indices = svm.support_
  svm2 = SVC(kernel="linear", C=1000).fit(X[sv_indices], y[sv_indices])
  print("w from SVs only:", svm2.coef_[0].round(3))
  print("b from SVs only:", round(svm2.intercept_[0], 3))
  ```
  
  ```
                   Diagnostic Output Profile (Slide 19)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ w:               [-0.252, -0.838]   b: -3.208                          │
  │ margin 2/||w||:  2.285                                                 │
  │ support vectors: [ 4, 19, 32]  (Exactly 3 points out of 40!)           │
  │ f(x) at SVs:     [-1.000,  1.000,  1.000]  (Sit EXACTLY on boundaries!) │
  ├────────────────────────────────────────────────────────────────────────┤
  │ RE-FITTING ON THE 3 SUPPORT VECTORS ALONE:                             │
  │ w from SVs only: [-0.252, -0.838]                                      │
  │ b from SVs only: -3.208                                                │
  │                                                                        │
  │ Result: w and b match to MACHINE PRECISION!                            │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Systems Deconstruction of Slide 19:
  1. **Decision Function Values at Support Vectors:**
   Notice the output of `svm.decision_function(X[svm.support_])`:
   $$f(x_{\text{sv}}) = w^T x_{\text{sv}} + b = \mathbf{[-1.000, \; +1.000, \; +1.000]}$$
   This confirms the canonical scaling convention derived in Section 1: the support vectors sit **exactly on the canonical margin boundaries** $w^T x + b = \pm 1.0$.
  2. **The 3-Point Refit Invariance:**
   When the dataset is reduced from $40$ rows down to the $3$ support vector rows, the resulting weights $w$ and bias $b$ are identical. The remaining 37 data points have zero influence on the decision boundary.
  
  ---
## 5. Practice P2: Turning the $C$ Dial on Real Data (Slide 20)
  
  Slide 20 evaluates the effect of $C$ on the Breast Cancer Wisconsin dataset ($N_{\text{tr}} = 455, d = 30$):
  
  ```python
  from sklearn.datasets import load_breast_cancer
  from sklearn.linear_model import LogisticRegression
  from sklearn.model_selection import StratifiedKFold, cross_val_score, train_test_split
  from sklearn.pipeline import make_pipeline
  from sklearn.preprocessing import StandardScaler
  from sklearn.svm import SVC
  
  # 1. Setup Data & Stratified Holdout Split
  X, y = load_breast_cancer(return_X_y=True)
  y = 1 - y  # 1 = Malignant, 0 = Benign
  
  X_tr, X_te, y_tr, y_te = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
  )
  
  cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=0)
  
  # 2. Sweep C Across Five Orders of Magnitude
  for C in [0.001, 0.01, 0.1, 1, 10, 100]:
    pipe = make_pipeline(StandardScaler(), SVC(kernel="linear", C=C))
    acc = cross_val_score(pipe, X_tr, y_tr, cv=cv).mean()
  
    # Extract support vector count from fitted model
    fitted_svm = pipe.fit(X_tr, y_tr)[-1]
    n_sv = len(fitted_svm.support_)
  
    print(f"C={C:<6} CV acc {acc:.3f}   support vectors {n_sv}")
  ```
  
  ```
                   Empirical C-Dial Profile (Slide 20)
  ┌──────────┬──────────────┬────────────────────────┬─────────────────────────────┐
  │ Value C  │ CV Accuracy  │ Support Vectors (n_sv) │ Qualitative Diagnostic State│
  ├──────────┼──────────────┼────────────────────────┼─────────────────────────────┤
  │ C = 0.001│ 0.945        │ 246 SVs (54.1% of data)│ UNDERFIT (Wide margin/Soft) │
  │ C = 0.01 │ 0.967        │ 128 SVs (28.1% of data)│ Transition zone             │
  │ C = 0.1  │ 0.976        │  58 SVs (12.7% of data)│ BEST C (Optimal trade-off)  │
  │ C = 1.0  │ 0.974        │  42 SVs ( 9.2% of data)│ Balanced fit                │
  │ C = 10.0 │ 0.965        │  34 SVs ( 7.5% of data)│ Variance begins to emerge   │
  │ C = 100.0│ 0.960        │  31 SVs ( 6.8% of data)│ OVERFIT (Narrow margin/Hard)│
  └──────────┴──────────────┴────────────────────────┴─────────────────────────────┘
  ```
  
  ---
### Empirical Insights from Practice P2:
  1. **The Support Vector Compression:**
   * At $C = 0.001$, the model tolerates large slack violations. The margin street is wide, encompassing **246 points (over half the dataset)** as active support vectors.
   * At $C = 100.0$, the penalty on violations is severe. The margin contracts, and only **31 boundary points** remain active support vectors.
  2. **The Accuracy Trajectory:**
   * Accuracy peaks at **$C = 0.1$ ($\text{Accuracy} = 0.976$)**. 
   * Pushing $C$ too high ($C = 100$) forces the boundary to narrow and chase individual noisy training points, causing validation accuracy to degrade from $97.6\%$ down to $96.0\%$.
  
  ---
## Complete Lecture 21 Synthesis Reference
  
  | Mathematical Concept | Algorithmic Formulation | Systems & Production Impact |
  | :--- | :--- | :--- |
  | **Maximum Margin** | $\max_{w, b} \frac{2}{\|w\|_2} \iff \min_{w, b} \frac{1}{2}\|w\|_2^2$ | Maximizes geometric tolerance to noise; eliminates the infinite-separator ambiguity of ERM. |
  | **Normal Vector $w$** | $w^T(x_A - x_B) = 0$ for points on boundary | $w$ is orthogonal to the decision hyperplane, pointing directly into the positive class region. |
  | **Distance to Plane** | $d_\perp(x) = \frac{\|w^T x + b\|}{\|w\|_2}$ | Perpendicular Euclidean distance; foundation of margin derivation. |
  | **Canonical Scaling** | $y_i(w^T x_i + b) \ge 1$ for closest points | Resolves scale invariance; fixes positive margin at $+1$ and negative margin at $-1$. |
  | **Support Vectors** | Points where $\alpha_i^* > 0 \iff y_i(w^T x_i + b) \le 1$ | Points off the margin have $\alpha_i^* = 0$. Deleting non-SVs leaves $w$ and $b$ completely unchanged. |
  | **SVM vs. Logistic Loss** | Hinge: $\max(0, 1 - y_i z_i)$ vs. Log: $\ln(1 + e^{-y_i z_i})$ | Easy points outside margin have zero hinge loss/gradient; all points exert non-zero gradient in LogReg. |
  | **Hard Margin Breakdown**| Constraints infeasible if data overlaps | A single outlier can shrink the margin by $88\%$ and rotate the boundary by $45^\circ$. |
  | **Soft Margin & Slack $\xi_i$** | $\min \frac{1}{2}\|w\|_2^2 + C\sum \xi_i$ s.t. $y_i z_i \ge 1 - \xi_i$ | $\xi_i = 0$ (clean), $0 < \xi_i \le 1$ (margin violation), $\xi_i > 1$ (misclassified). $\sum \xi_i \ge \text{errors}$. |
  | **The $C$ Hyperparameter** | Balances margin width ($\frac{1}{2}\|w\|^2$) vs. violations ($C\sum \xi_i$) | Small $C \implies$ wide margin, many SVs, high bias. Large $C \implies$ narrow margin, few SVs, high variance. |
  | **Non-Linear Lift** | Map $\phi(x): \mathbb{R}^d \to \mathbb{R}^D$ | $z = x_1^2 + x_2^2$ turns a 2D circle into a flat 3D slicing plane. Linear in $\phi(x)$, curved in $x$. |
  | **`SVC` vs. `LinearSVC`** | `SVC` uses LibSVM (dual, $\mathcal{O}(n^2)$, exposes SVs) | `LinearSVC` uses LibLinear (primal, $\mathcal{O}(n \cdot d)$, no SV list; scales to large $n$). |
  
  ---
## Summary Review Questions for Section 4
  
  1. *Why does `SVC(kernel="linear")` store the attribute `.support_vectors_`, while `LinearSVC` cannot provide a list of support vectors?*
  2. *In Practice P1 (Slide 19), why did evaluating `svm.decision_function()` on the support vectors yield values of exactly $+1.000$ and $-1.000$?*
  3. *If an SVM has a weight vector of $w = [6.0, \; 8.0]^T$, calculate its margin width $\gamma = \frac{2}{\|w\|_2}$.*
  
  ---