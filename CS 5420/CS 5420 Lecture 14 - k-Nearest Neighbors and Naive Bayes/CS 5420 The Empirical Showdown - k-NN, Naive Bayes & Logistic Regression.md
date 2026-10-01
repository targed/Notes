## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Discriminative vs. Generative Classifiers:**
  * **Discriminative Models (e.g., Logistic Regression, SVMs):** Directly model the posterior class distribution $P(Y \mid X)$ or learn a functional decision boundary $f(x)$ mapping inputs to labels. They make no assumptions about how the feature vectors $X$ were generated in nature.
  * **Generative Models (e.g., Naive Bayes, Gaussian Mixture Models):** Model the **joint probability distribution** $P(X, Y) = P(X \mid Y)P(Y)$. They learn a statistical prototype of how each class generates its observed features, and then use Bayes' Rule to invert the joint model to compute the posterior $P(Y \mid X)$.
  2. **Laplace-Smoothed Probability Calculation:**
  * Given $N_0 = 20$ training instances in Class 0, cardinality $K = 4$, observed count $N_{3, 0} = 0$, and smoothing parameter $\alpha = 1$:
   $$\hat{P}(X_j = 3 \mid Y = 0) = \frac{N_{jc} + \alpha}{N_c + \alpha K_j} = \frac{0 + 1}{20 + (1 \times 4)} = \frac{1}{24} \approx \mathbf{0.0417 \quad (4.17\%)}$$
  * Instead of assigning a fatal probability of zero ($0\%$), Laplace smoothing assigns a probability of $4.17\%$, allowing the remaining $d-1$ features to inform the classification.
  3. **The Domingos & Pazzani (1997) Resolution on Naive Bayes:**
  * Violating the class-conditional independence assumption causes feature probabilities to compound, pushing posterior estimates toward overconfident extremes ($0.0$ or $1.0$).
  * However, under **$0-1$ loss (Classification Accuracy)**, the model only cares about the **argmax decision**:
   $$\hat{y} = \arg\max_{c} P(Y = c \mid X)$$
  * Even if the true probability is $P(Y = 1 \mid X) = 0.55$ and Naive Bayes predicts an uncalibrated $\hat{P} = 0.99$, the predicted class remains Class 1. As long as the **rank order** of class probabilities is preserved, classification decisions remain optimal despite probability calibration errors.
  
  ---
## 2. Practice P1: The Empirical Impact of Scaling on $k$-NN (Slide 18)
  
  Slide 18 implements a live-coding diagnostic comparing raw versus standardized feature representations using the Breast Cancer Wisconsin benchmark:
  
  ```python
  from sklearn.datasets import load_breast_cancer
  from sklearn.model_selection import train_test_split
  from sklearn.pipeline import make_pipeline
  from sklearn.preprocessing import StandardScaler
  from sklearn.neighbors import KNeighborsClassifier
  
  # 1. Ingest Dataset & Flip Labels (1 = Malignant, 0 = Benign)
  X, y = load_breast_cancer(return_X_y=True)
  y = 1 - y
  
  # 2. Partition into Stratified Train/Test Splits
  X_tr, X_te, y_tr, y_te = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
  )
  
  # 3. Model A: 5-NN on Raw, Unscaled Features
  raw = KNeighborsClassifier(n_neighbors=5).fit(X_tr, y_tr)
  
  # 4. Model B: 5-NN Inside a Leak-Free Standardized Pipeline
  scl = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=5))
  scl.fit(X_tr, y_tr)
  
  # 5. Evaluate Generalization Accuracy
  print("5-NN raw    :", round(raw.score(X_te, y_te), 4))
  print("5-NN scaled :", round(scl.score(X_te, y_te), 4))
  ```
  
  ```
                 Empirical Output: Raw vs. Scaled k-NN (Slide 18)
  ┌──────────────────────────────────────┬──────────────────┬──────────────┐
  │ Estimator Pipeline                   │ Test Accuracy    │ Metric Delta │
  ├──────────────────────────────────────┼──────────────────┼──────────────┤
  │ 5-NN on Raw Unscaled Features        │ 92.98% (0.9298)  │ Baseline     │
  │ 5-NN Inside StandardScaler Pipeline  │ 95.61% (0.9561)  │ +2.63% Jump! │
  └──────────────────────────────────────┴──────────────────┴──────────────┘
  ```
  
  ---
### The Mathematical Explanation of the Performance Jump:
  Why does `StandardScaler` add $+2.63\%$ to test accuracy without changing a single hyperparameter?
  
  Inspect the raw feature distributions of the Breast Cancer dataset:
  * **`mean area`:** Values range from $143.5$ to $2{,}501.0$ ($\sigma \approx 351.9$).
  * **`mean smoothness`:** Values range from $0.053$ to $0.163$ ($\sigma \approx 0.014$).
  
  Evaluate the squared Euclidean distance between two tumor observations:
  
  $$D_2(x_A, x_B)^2 = \sum_{j=1}^{30} (x_{A, j} - x_{B, j})^2$$
  
  * A minor $10\%$ relative difference in `mean area` contributes:
  $$\Delta_{\text{area}}^2 \approx (150)^2 = \mathbf{22{,}500}$$
  * A massive $50\%$ relative difference in `mean smoothness` contributes:
  $$\Delta_{\text{smoothness}}^2 \approx (0.05)^2 = \mathbf{0.0025}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Consequence:                                                       │
  │ • In the raw unscaled model, Euclidean distance is dominated by        │
  │   features measured in large absolute units (area, perimeter).         │
  │ • The remaining 25+ features (smoothness, symmetry, fractal dimension) │
  │   are treated as mathematical noise.                                   │
  │ • Standardizing features (z = (x - μ) / σ) rescales all coordinates    │
  │   to unit variance, ensuring all 30 morphological features inform the  │
  │   distance metric equally.                                             │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Practice P2: Optimizing Neighborhood Size ($k$) via Cross-Validation (Slide 17 Plan)
  
  Slide 17 lists step P2: *"Pick $k$ by cross-validation."*
  
  In $k$-NN, the neighborhood size $k$ is the primary hyperparameter governing the **bias-variance trade-off**:
  
  ```
                 The Cross-Validation Error Curve vs. Neighborhood Size k
    CV Error Rate
         ▲
    0.10 │   ● k = 1 (Overfitting: Memorizes sample-specific noise)
         │    \
    0.08 │     \
         │      \
    0.06 │       \       Optimal Zone: k ∈ [7, 11]
         │        \      ╭───────────────────────╮
    0.04 │         ╰─────●───────●───────●───────●─────╮
         │               k=7    k=9     k=11           \   k > 25 (Underfitting:
    0.02 │                                              ╰──●────●──── Boundary flattens;
         │                                                 k=25 k=35 class prior dominates)
    0.00 ┴───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───▶ Neighborhood Size k
         1   3   5   7   9   11  13  15  17  19  21  23  25
  ```
### Production Tuning Implementation:
  ```python
  from sklearn.model_selection import GridSearchCV
  
  param_grid = {'kneighborsclassifier__n_neighbors': [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]}
  grid = GridSearchCV(
    make_pipeline(StandardScaler(), KNeighborsClassifier()),
    param_grid,
    cv=5,
    scoring='accuracy'
  )
  grid.fit(X_tr, y_tr)
  print(f"Optimal Neighborhood Size: k = {grid.best_params_['kneighborsclassifier__n_neighbors']}")
  # Selected k = 7 (Balances local boundary resolution with noise smoothing)
  ```
  
  ---
## 4. Practice P3: The Three-Way Model Showdown (Slide 19)
  
  Slide 19 compares three distinct machine learning paradigms on the identical hold-out test set:
  
  ```python
  from sklearn.linear_model import LogisticRegression
  from sklearn.naive_bayes import GaussianNB
  
  # Define model dictionary spanning distinct inductive biases
  models = {
    "LogReg": LogisticRegression(max_iter=1000),
    "7-NN  ": KNeighborsClassifier(n_neighbors=7),
    "GausNB": GaussianNB()
  }
  
  # Evaluate all three within standardized pipelines
  for name, m in models.items():
    pipe = make_pipeline(StandardScaler(), m).fit(X_tr, y_tr)
    pred = pipe.predict(X_te)
    
    # Calculate False Negatives (Missed Malignancies)
    fn = ((pred == 0) & (y_te == 1)).sum()
    print(name, round(pipe.score(X_te, y_te), 4), " missed cancers:", fn)
  ```
  
  ```
                 The Three-Model Empirical Showdown (Slide 19)
  ┌──────────────┬────────────────────────┬───────────────┬────────────────────────┐
  │ Model Name   │ Algorithmic Paradigm   │ Test Accuracy │ Missed Cancers (FN) ⚠️ │
  ├──────────────┼────────────────────────┼───────────────┼────────────────────────┤
  │ LogReg       │ Parametric / Linear    │ 96.49% (0.965)│ 3 Missed Tumors        │
  │ 7-NN         │ Non-Parametric / Lazy  │ 96.49% (0.965)│ 3 Missed Tumors        │
  │ GausNB       │ Generative / Bayes     │ 93.86% (0.939)│ 4 Missed Tumors        │
  └──────────────┴────────────────────────┴───────────────┴────────────────────────┘
  ```
  
  ---
## 5. Deconstructing the Three Competitors: Paradigms and Failure Modes
  
  ---
### Competitor 1: Logistic Regression (`LogReg`)
  * **Paradigm:** Discriminative, parametric, convex empirical risk minimization.
  * **Mechanism:** Solves an unconstrained convex optimization problem using L-BFGS to fit a single flat $(d-1)$-dimensional hyperplane:
  $$P(Y = 1 \mid x) = \sigma(w^T x + b)$$
  * **Why It Wins on Accuracy ($96.49\%$):** The Breast Cancer Wisconsin dataset contains 30 features that are largely linearly separable. A linear hyperplane fits this feature space with minimal variance, avoiding the local noise traps that affect nearest-neighbor algorithms.
  
  ---
### Competitor 2: $k$-Nearest Neighbors (`7-NN`)
  * **Paradigm:** Non-parametric, instance-based, memory-bound lazy learner.
  * **Mechanism:** Retains all 455 training vectors in memory; performs majority voting across the $k = 7$ nearest Euclidean neighbors.
  * **Why It Matches Logistic Regression ($96.49\%$):** Once features are normalized via `StandardScaler`, local neighborhood voting approximates the global decision boundary.
  * **Systems Trade-Off:** While matching Logistic Regression in accuracy, $k$-NN requires significantly more memory ($\mathcal{O}(n \cdot d)$) and computes $114 \times 455$ pairwise distances during inference, whereas Logistic Regression performs a single matrix-vector product ($X_{\text{test}} w$).
  
  ---
### Competitor 3: Gaussian Naive Bayes (`GausNB`)
  * **Paradigm:** Generative, parametric probabilistic modeling.
  * **Mechanism:** Assumes continuous features follow a univariate Gaussian distribution within each class:
  $$P(X_j = x_j \mid Y = c) = \frac{1}{\sqrt{2\pi \sigma_{jc}^2}} \exp\left( -\frac{(x_j - \mu_{jc})^2}{2\sigma_{jc}^2} \right)$$
  * **Why It Trails in Accuracy ($93.86\%$):** 
  * The Wisconsin dataset contains heavy **multicollinearity**. Features like `mean radius`, `mean perimeter`, and `mean area` have pairwise correlations exceeding $r > 0.98$ (they describe the same physical geometry).
  * Naive Bayes violates its core assumption by treating these three collinear features as independent sources of evidence, overcounting the "size" signal three times in the likelihood product. This correlation-induced overconfidence leads to misclassifications near the boundary.
  * **Scale Invariance Note:** Unlike $k$-NN and Logistic Regression, **Gaussian Naive Bayes is mathematically invariant to feature scaling**. Standardizing features transforms each coordinate via $z = \frac{x - \mu}{\sigma}$. In a Gaussian density function, scale factors cancel out, meaning `GausNB` yields the identical result on raw or scaled features.
  
  ---
## 6. The Clinical Metric Dilemma: Analyzing Missed Cancers (Slide 19)
  
  Slide 19 emphasizes:
  ```python
  fn = ((pred == 0) & (y_te == 1)).sum()  # missed cancers
  ```
  
  $$\mathbf{\text{"LogReg: 3 missed cancers | 7-NN: 3 missed cancers | GausNB: 4 missed cancers"}}$$
  
  ```
                      The High-Stakes Clinical Matrix
             72 Benign Patients                  42 Malignant Cancer Patients
  ┌──────────────────────────────────────────┐ ┌──────────────────────────────────┐
  │ False Positives (FP ≈ 1–3):              │ │ False Negatives (FN = 3 to 4): ⚠️│
  │ • Healthy patient flagged as malignant.  │ │ • Cancerous patient sent home    │
  │ • Secondary biopsy reveals benign truth. │ │   undiagnosed!                   │
  │ • Non-fatal operational cost.            │ │ • FATAL / CATASTROPHIC OUTCOME.  │
  └──────────────────────────────────────────┘ └──────────────────────────────────┘
  ```
  
  Both Logistic Regression and 7-NN achieve an overall accuracy of **$96.49\%$**, but both miss **3 malignant tumors**. 
  
  To deploy any of these models safely in a clinical environment, the default threshold ($\tau = 0.50$) must be lowered to **$\tau = 0.15$ or $\tau = 0.10$** (as derived in Lecture 13). We accept more False Positives (unnecessary follow-up biopsies) to drive False Negatives strictly to zero ($\text{Recall} \to 100\%$).
  
  ---
## Summary Review Questions for Section 4
  
  1. *Why did standardizing features with `StandardScaler` improve the test accuracy of the $5$-NN classifier on the Breast Cancer dataset from $92.98\%$ to $95.61\%$?*
  2. *Why is Gaussian Naive Bayes (`GaussianNB`) mathematically invariant to linear feature scaling, while $k$-NN is highly sensitive to it?*
  3. *If two models both achieve an accuracy of $96.5\%$, explain why a clinical screening deployment would prefer a model with $\text{FN} = 0$ and $\text{FP} = 8$ over a model with $\text{FN} = 4$ and $\text{FP} = 0$.*
  
  ---
## Complete Lecture 14 Synthesis Reference
  
  | Core ML Concept | Mathematical / Theoretical Formulation | Systems Reality & Production Impact |
  | :--- | :--- | :--- |
  | **$k$-NN Classifier** | $\hat{y} = \arg\max_c \sum_{i \in \mathcal{N}_k(x)} \mathbf{1}_{\{y_i = c\}}$ | Non-parametric, lazy learner. Training is $\mathcal{O}(1)$; inference requires an exhaustive $\mathcal{O}(n \cdot d)$ linear scan. |
  | **Bias-Variance ($k$)** | Effective $\text{DoF} \approx \frac{n}{k}$. Small $k$ overfits; large $k$ underfits. | $k = 1$ has zero training error but extreme variance. As $k \to n$, predictions collapse to the global majority class. |
  | **Cover-Hart Theorem** | $\lim_{n \to \infty} R_{1\text{NN}} \le 2 R^* (1 - R^*)$ | The asymptotic risk of a $1$-NN classifier is bounded by at most twice the irreducible Bayes optimal error rate $R^*$. |
  | **Minkowski Metrics** | $D_p(x, y) = \left( \sum \|x_j - y_j\|^p \right)^{1/p}$ | $p=1$ (Manhattan diamond), $p=2$ (Euclidean circle), $p=\infty$ (Chebyshev square). Changing $p$ re-orders neighbor rankings. |
  | **Empty Space Trap** | Edge length $e = r^{1/d}$ | To capture $10\%$ of data in $d = 50$, the neighborhood must span over $95.5\%$ of each feature axis. Local estimation collapses. |
  | **Distance Concentration** | $\lim_{d \to \infty} \frac{D_{\max} - D_{\min}}{D_{\min}} \to 0$ | As dimension $d$ grows, all points become approximately equidistant, rendering nearest-neighbor distance metrics uninformative. |
  | **Generative vs. Discrim.**| Generative models $P(X, Y)$; Discriminative models $P(Y \mid X)$. | Naive Bayes learns how each class generates data; Logistic Regression directly optimizes the decision boundary. |
  | **The "Naive" Assumption** | $P(X_1, \dots, X_d \mid Y = c) = \prod_{j=1}^d P(X_j \mid Y = c)$ | Assumes features are independent conditioned on the class label. Cuts parameters from $\mathcal{O}(2^d)$ to $\mathcal{O}(d \cdot C)$. |
  | **Laplace Smoothing** | $\hat{P} = \frac{N_{jc} + \alpha}{N_c + \alpha K_j}$ | Resolves the zero-frequency defect. Injects virtual pseudocounts ($\alpha = 1$) to prevent a single zero from wiping out the likelihood. |
  | **0-1 Loss Robustness** | $\hat{y} = \arg\max_c P(Y = c \mid X)$ | Naive Bayes remains accurate despite correlated features because 0-1 classification loss depends on rank order, not calibration. |
  | **Scale Sensitivity** | $D_2(x, y) = \sqrt{\sum (x_j - y_j)^2}$ | Unscaled features allow large-magnitude variables to dominate distance metrics. `StandardScaler` is mandatory for $k$-NN. |
  | **Clinical Cost Matrix** | $\text{Cost}(\text{False Negative}) \gg \text{Cost}(\text{False Positive})$ | In medical screening, accuracy hides missed cancers (FN). Decision thresholds must be shifted to prioritize Recall. |
  
  ---