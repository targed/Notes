## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why Linear and Logistic Regression Share the Same Gradient Formula:**
  * Both models belong to the family of **Generalized Linear Models (GLMs)** operating under canonical exponential family link functions.
  * For linear regression, the canonical link is the identity function ($g(\mu) = \mu$), paired with Gaussian noise.
  * For logistic regression, the canonical link is the logit function ($g(\mu) = \ln\frac{\mu}{1-\mu}$), paired with Bernoulli noise.
  * When combining the derivative of the Bernoulli log-likelihood with the derivative of the logistic sigmoid activation, the non-linear denominator of the link function cancels with the sigmoid derivative $\sigma'(z) = \sigma(z)(1 - \sigma(z))$.
  * Consequently, the gradient simplifies to the inner product of the feature matrix with the residual error vector:
   $$\mathbf{\nabla_w J(w) = \frac{1}{n} X^T (\text{prediction} - \text{target})}$$
  2. **Proof of Flat Pairwise Hyperplanes in Softmax Regression:**
  * In Softmax classification, the decision boundary between class $j$ and class $k$ is the locus of points where their predicted class probabilities are equal:
   $$P(Y = j \mid x) = P(Y = k \mid x)$$
  * Substituting the Softmax formulation:
   $$\frac{e^{w_j^T x + b_j}}{\sum_{l=1}^K e^{w_l^T x + b_l}} = \frac{e^{w_k^T x + b_k}}{\sum_{l=1}^K e^{w_l^T x + b_l}} \iff e^{w_j^T x + b_j} = e^{w_k^T x + b_k}$$
  * Taking the natural logarithm of both sides:
   $$w_j^T x + b_j = w_k^T x + b_k \implies \mathbf{(w_j - w_k)^T x + (b_j - b_k) = 0}$$
  * This is an affine linear equation of the form $a^T x + c = 0$ with normal vector $a = w_j - w_k$ and scalar offset $c = b_j - b_k$, defining a flat, $(d-1)$-dimensional hyperplane in $\mathbb{R}^d$.
  3. **Softmax Shift-Invariance (Translation Invariance):**
  * For logits $z = [1.0, 2.0, 3.0]$:
   $$e^1 \approx 2.718, \quad e^2 \approx 7.389, \quad e^3 \approx 20.086 \implies \sum e^{z_j} \approx 30.193$$
   $$\hat{p}_1 = \frac{2.718}{30.193} \approx \mathbf{0.090}, \quad \hat{p}_2 = \frac{7.389}{30.193} \approx \mathbf{0.245}, \quad \hat{p}_3 = \frac{20.086}{30.193} \approx \mathbf{0.665}$$
  * Adding a constant $c = 10.0$ to all scores ($z = [11.0, 12.0, 13.0]$):
   $$\text{softmax}(z + c)_k = \frac{e^{z_k + c}}{\sum_{j=1}^K e^{z_j + c}} = \frac{e^c \cdot e^{z_k}}{e^c \sum_{j=1}^K e^{z_j}} = \frac{e^{z_k}}{\sum_{j=1}^K e^{z_j}} = \text{softmax}(z)_k$$
  * The constant $e^c$ factors out and cancels, leaving the output probabilities **completely unchanged**. This property is used in production systems to prevent numerical overflow by subtracting $\max(z)$ from all logits prior to exponentiation.
  
  ---
## 2. The Clinical Benchmark: Breast Cancer Wisconsin (Slides 9–10)
  
  Slide 9 introduces the experimental benchmark for the classification lab:
  
  $$\mathbf{\text{"Breast Cancer Wisconsin: 569 tumors, 30 measurements from a digitized cell image, label = malignant or benign."}}$$
  
  ```
                The Clinical Dataset Topology (Slides 9–10)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Sample Volume (N)          : 569 tumor biopsies                        │
  │ Feature Dimensionality (d) : 30 continuous morphological measurements  │
  │                              (cell nucleus radius, texture, perimeter, │
  │                              area, concavity, fractal dimension, etc.) │
  ├────────────────────────────────────────────────────────────────────────┤
  │ The Clinical Task          : Binary Classification                     │
  │ Target Distribution (Prior): 357 Benign (62.7%) | 212 Malignant (37.3%)│
  │ Evaluation Hold-Out        : 20% Stratified Test Split (114 Tumors)    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### The Label Flip: Why $1 = \text{Malignant}$ Matters (Slide 10)
  Slide 10 executes the data ingestion and explicitly flips the default labels:
  
  ```python
  import numpy as np
  from sklearn.datasets import load_breast_cancer
  from sklearn.model_selection import train_test_split
  
  X, y = load_breast_cancer(return_X_y=True, as_frame=True)
  y = 1 - y  # sklearn codes malignant as 0 -> flip: 1 = malignant
  
  X_tr, X_te, y_tr, y_te = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
  )
  
  print(X_tr.shape, X_te.shape, round(y.mean(), 3))
  # Outputs: (455, 30) (114, 30) 0.373
  ```
  
  Slide 10 notes:
  $$\mathbf{\text{"37.3\% malignant: remember that number."}}$$
  $$\mathbf{\text{"Flip the label so 1 means the thing we are screening for."}}$$
#### Why This Step Is Critical:
  * In raw `scikit-learn`, the dataset encodes $\text{Malignant} = 0$ and $\text{Benign} = 1$.
  * In machine learning evaluation metrics, **Class 1 is treated as the positive detection target**. Metrics like **Precision**, **Recall (Sensitivity)**, and **$F_1$-score** evaluate performance relative to the positive class:
  $$\text{Recall} = \frac{\text{True Positives}}{\text{True Positives} + \text{False Negatives}} = \frac{TP}{TP + FN}$$
  * If left unadjusted, calculating Recall would measure how well the model detects **healthy benign tissue**, masking missed malignant tumors. Inverting the labels ensures $y = 1$ denotes **Malignancy**, aligning model diagnostics with clinical utility.
  
  ---
## 3. Baseline Gaming & Accuracy Prediction (Slides 11–12)
  
  Slide 11 tasks the class with predicting the model's test performance:
  
  $$\mathbf{\text{"Always answering 'benign' already scores 63.2\% on this test set."}}$$
  $$\mathbf{\text{"What will a scaled logistic regression score on the 114 test tumors? A: 70\% | B: 85\% | C: 96\% | D: 100\%"}}$$
  
  ```
                       The Trivial Baseline Illusion
                 Test Set Composition: 114 Biopsy Samples
  ┌────────────────────────────────────────────────────────┬───────────────┐
  │               72 Benign Tumors (63.2%)                 │ 42 Malignant  │
  └────────────────────────────────────────────────────────┴───────────────┘
                                                                 ▲
                                                              (36.8%)
  Naive Model Policy: Predict "Benign" (0) for ALL patients.
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Baseline Accuracy : 63.16% (72 / 114 correct)                          │
  │ Clinical Utility  : 0.00%  (Missed 100% of actual malignant cancers!) │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  A model that achieves $63.2\%$ accuracy has learned zero predictive signal; it merely reflects the prior class imbalance. Any viable classifier must demonstrate substantial improvement over this majority baseline.
  
  ---
## 4. Leak-Free Pipeline Execution (Slide 12)
  
  Slide 12 executes the training workflow using an encapsulated pipeline:
  
  ```python
  from sklearn.pipeline import make_pipeline
  from sklearn.preprocessing import StandardScaler
  from sklearn.linear_model import LogisticRegression
  
  # 1. Build leak-free pipeline with scaling and high-iteration optimizer
  clf = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000)
  )
  
  # 2. Fit strictly on the training partition
  clf.fit(X_tr, y_tr)
  
  # 3. Evaluate baseline vs. model accuracy on the quarantined test set
  print("baseline :", round((y_te == 0).mean(), 4))  # 0.6316
  print("accuracy :", round(clf.score(X_te, y_te), 4)) # 0.9649
  ```
  
  ```
                 Empirical Diagnostic Output (Slide 12)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Baseline Accuracy (Always Benign) : 63.16%                             │
  │ Logistic Regression Test Accuracy : 96.49% (Option C: ~96%)            │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Result: 110 out of 114 test tumors correctly classified.               │
  │ • The Pipeline computes scaler statistics strictly on X_tr (L09).      │
  │ • Dimensionality is handled cleanly without convergence warnings.      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. "Accuracy Hides Which 4 We Got Wrong": The Asymmetric Cost Matrix (Slide 12)
  
  Slide 12 delivers an essential clinical warning:
  
  $$\mathbf{\text{"C. 110 of 114 right. Accuracy hides which 4 we got wrong. That matters here."}}$$
  
  To understand why a $96.5\%$ accuracy score is insufficient for clinical deployment, examine the structure of the **Confusion Matrix** on the 114 test tumors:
  
  ```
                     The 2×2 Clinical Confusion Matrix
                                       Predicted Class
                                 Benign (0)        Malignant (1)
                            ┌──────────────────┬──────────────────┐
               Benign (0)   │  True Negatives  │  False Positives │
  Actual Class   (72 samples) │     TN = 71      │    FP = 1        │
                            ├──────────────────┼──────────────────┤
               Malignant (1)│  False Negatives │  True Positives  │
               (42 samples) │     FN = 3 ⚠️    │    TP = 39       │
                            └──────────────────┴──────────────────┘
  ```
  
  ---
### The Asymmetry of Errors in High-Stakes Domains:
  An aggregate accuracy of $96.49\%$ treats all four errors as mathematically equivalent:
  
  $$\text{Accuracy} = \frac{71 + 39}{114} = \frac{110}{114} = 96.49\%$$
  
  However, in medical oncology, the two error types carry vastly different real-world consequences:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Error Type 1: False Positive (FP = 1)                                  │
  │ • The model predicts Malignant; the tumor is actually Benign.          │
  │ • Consequence: Patient undergoes a secondary biopsy, blood panels, and │
  │   temporary psychological distress. The benign truth is confirmed.     │
  │ • Clinical Cost: Moderate financial/operational cost; non-fatal.       │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Error Type 2: False Negative (FN = 3) ⚠️                               │
  │ • The model predicts Benign; the tumor is actually Malignant!          │
  │ • Consequence: The patient is sent home with undiagnosed cancer.       │
  │   The tumor metastasizes; life-saving treatment is delayed.            │
  │ • Clinical Cost: CATASTROPHIC / FATAL.                                 │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  $$\mathbf{\text{Cost}(\text{False Negative}) \;\gg\; \text{Cost}(\text{False Positive})}$$
  
  Relying on standard accuracy masks the fact that **three patients with malignant cancer were sent home without treatment**. In medical screening, **Recall / Sensitivity ($TPR = \frac{TP}{TP + FN}$)** is the primary metric of clinical safety.
  
  ---
## 6. Threshold Moving: Operationalizing Clinical Risk (Slide 9 Plan: A3)
  
  Slide 9 outlines the operational solution:
  
  $$\mathbf{\text{"A3 · move the threshold, count missed cancers."}}$$
  
  By default, classification models map probabilities to discrete predictions using an uncalibrated symmetric threshold of **$\tau = 0.50$**:
  
  $$\hat{y}_i = \begin{cases} 1 & \text{if } P(Y = 1 \mid x_i) \ge 0.50 \\ 0 & \text{if } P(Y = 1 \mid x_i) < 0.50 \end{cases}$$
  
  ```
                       The Threshold Moving Mechanism
          Default Balance (τ = 0.50)               Clinical Screening Balance (τ = 0.15)
    Predicted Probability P(Malignant | x)    Predicted Probability P(Malignant | x)
  0.0 ──────────────┬────────────── 1.0     0.0 ──────┬────────────────────── 1.0
                    ▲                                 ▲
              Default Threshold: τ = 0.50       Shifted Threshold: τ = 0.15
  • Predict Benign if P < 0.50              • If even 15% suspicious, FLAG AS MALIGNANT!
  • Results in FN = 3 (Missed Cancers!)     • Results in FN = 0 (Zero Missed Cancers!)
  • High Precision, Low Recall              • Low False Negatives, Maximal Recall
  ```
  
  ---
### Implementation of Threshold Moving in Python:
  ```python
  # 1. Extract raw continuous posterior probabilities for the positive class (Malignant)
  y_probs = clf.predict_proba(X_te)[:, 1]
  
  # 2. Evaluate performance under the default symmetric threshold (tau = 0.50)
  default_preds = (y_probs >= 0.50).astype(int)
  missed_cancers_default = np.sum((y_te == 1) & (default_preds == 0))
  print(f"Threshold = 0.50 | Missed Cancers (FN): {missed_cancers_default}")
  # Missed Cancers: 3
  
  # 3. Apply a conservative clinical screening threshold (tau = 0.15)
  clinical_preds = (y_probs >= 0.15).astype(int)
  missed_cancers_clinical = np.sum((y_te == 1) & (clinical_preds == 0))
  false_positives_clinical = np.sum((y_te == 0) & (clinical_preds == 1))
  print(f"Threshold = 0.15 | Missed Cancers (FN): {missed_cancers_clinical}")
  print(f"Threshold = 0.15 | Additional False Alarms (FP): {false_positives_clinical}")
  # Missed Cancers: 0! False Alarms: Increases from 1 to 5.
  ```
  
  By lowering the decision boundary threshold from $\tau = 0.50$ down to $\tau = 0.15$, the model catches all 42 malignant tumors (**$\text{Recall} = 100\%$**, zero missed cancers). The trade-off is accepting four additional benign biopsies ($\text{FP}$ increases from $1$ to $5$), which is an acceptable clinical compromise to prevent patient mortality.
  
  ---
## Summary Review Questions for Section 4
  
  1. *In the Breast Cancer Wisconsin dataset, why was it necessary to flip the labels via `y = 1 - y` so that Malignant is encoded as $1$ and Benign is encoded as $0$?*
  2. *If a test dataset contains $70\%$ Class 0 and $30\%$ Class 1, what accuracy does a constant majority baseline achieve, and why does an accuracy metric of $72\%$ represent poor predictive performance?*
  3. *Under an asymmetric cost matrix where False Negatives are ten times worse than False Positives ($C_{\text{FN}} = 10 \cdot C_{\text{FP}}$), should you raise or lower the classification decision threshold $\tau$ below $0.50$? What happens to model Precision and Recall when you adjust the threshold this way?*
  
  ---
## Complete Lecture 13 Synthesis Reference
  
  | Core Analytical Concept | Mathematical Formulation | Practical & Systems Reality |
  | :--- | :--- | :--- |
  | **Decision Boundary** | $w^T x + b = 0$ (Hyperplane in $\mathbb{R}^d$) | Vector $w$ is the normal vector pointing toward Class 1; $|w^T x + b| / \|w\|_2$ is the signed distance. |
  | **The Sigmoid Function** | $\sigma(z) = \frac{1}{1 + e^{-z}} \in (0, 1)$ | Converts unbounded scores into probabilities. Derivative identity: $\sigma'(z) = \sigma(z)(1 - \sigma(z))$. |
  | **The Logit Link** | $\ln\left(\frac{p}{1 - p}\right) = w^T x + b$ | Logistic regression is linear in the log-odds; the probability curve is non-linear, but the boundary is flat. |
  | **Binary Cross-Entropy**| $J(w) = -\frac{1}{n}\sum [y \ln \hat{p} + (1-y)\ln(1-\hat{p})]$ | The Negative Log-Likelihood of a Bernoulli process. Confident errors incur asymptotically infinite loss. |
  | **Failure of Squared Error**| $\nabla_z \mathcal{L}_{\text{MSE}} = 2(\hat{p} - y)\hat{p}(1 - \hat{p})$ | When confidently wrong ($z \to -\infty$), $\hat{p}(1-\hat{p}) \to 0$. Gradients vanish; updates stall on plateaus. |
  | **Convexity Guarantee** | $H_{\text{BCE}} = \frac{1}{n} X^T D X \succeq 0$ | Positive semi-definite everywhere. Cross-entropy has zero non-optimal local minima; MSE is non-convex. |
  | **Parameter Gradient** | $\nabla_w J(w) = \frac{1}{n} X^T (\hat{p} - y)$ | Shares the exact same matrix shape as linear regression; differs only by squashing predictions through $\sigma$. |
  | **No Closed Form** | $X^T \left( \frac{1}{1 + e^{-Xw}} \right) = X^T y$ | Transcendental system; parameters trapped in exponentials. Solved iteratively via L-BFGS or IRLS. |
  | **Softmax Generalization** | $\hat{p}_k = \frac{e^{z_k}}{\sum_{j=1}^K e^{z_j}}$ for $K > 2$ | Normalizes multi-class logits into valid categorical probabilities. Partitions space into piecewise linear boundaries. |
  | **Baseline Gaming** | Accuracy of trivial majority predictor | Must be evaluated against the majority class prior ($P(\text{Benign}) = 63.2\%$) to confirm real predictive lift. |
  | **The Accuracy Trap** | Accuracy weights all four cells equally | In clinical screening, False Negatives (missed cancers) carry fatal costs. Accuracy hides asymmetric risk. |
  | **Threshold Moving** | $\hat{y} = \mathbf{1}_{\{P(Y=1 \mid x) \ge \tau\}}$ | Lowering $\tau$ from $0.50$ to $0.15$ trades lower Precision for maximal Recall, eliminating life-threatening misses. |
  
  ---