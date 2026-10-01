# CS 5420 — Introduction to Machine Learning
## Exam 1: Practice Examination (Modules 1–6 / Lectures 1–15)
  
  * **Time Allowed:** 50 Minutes
  * **Total Points:** 100 Points
  * **Format:** Closed-book, closed-notes, closed-device. Simple paper-and-pencil arithmetic.
  * **Instructions:** Answer all questions directly on the test paper. Where calculations are required, show intermediate steps. Round final decimal answers to **3 decimal places**.
  
  ---
## Part A: Multiple Choice (6 Questions $\times$ 3 Points = 18 Points)
#### Q1. Array Operations & Tensor Broadcasting
  Given two arrays instantiated in NumPy:
  ```python
  A = np.ones((5, 1, 4))
  B = np.zeros((3, 1))
  ```
  What is the shape of the array resulting from the vectorized addition `C = A + B`?
  * **A.** `(5, 3, 4)`
  * **B.** `(5, 1, 4)`
  * **C.** `(15, 4)`
  * **D.** `ValueError: operands could not be broadcast together`
  
  ---
#### Q2. First-Order Convex Optimization
  The Hessian matrix of second derivatives for an Ordinary Least Squares cost function $J(w) = \frac{1}{n}\|Xw - y\|_2^2$ is evaluated as:
  $$H = \nabla^2 J(w) = \begin{bmatrix} 80.0 & 0.0 \\ 0.0 & 0.4 \end{bmatrix}$$
  What is the theoretical maximum learning rate ($\eta_{\text{max}}$) above which Batch Gradient Descent will oscillate with expanding amplitude and diverge?
  * **A.** $\eta = 5.000$
  * **B.** $\eta = 0.025$
  * **C.** $\eta = 0.0125$
  * **D.** $\eta = 2.500$
  
  ---
#### Q3. High-Leverage Outliers & Loss Function Geometry
  An engineer fits three different models to a dataset containing $99$ normal inliers and $1$ extreme measurement error outlier situated far along the feature axis. Which of the following statements correctly characterizes how the estimators respond to this single corrupted point?
  * **A.** Ordinary Least Squares ($L_2$) rotates its hyperplane significantly to accommodate the point, whereas a Huber Regressor bounds the influence of the outlier's residual gradient.
  * **B.** Ordinary Least Squares ($L_2$) is immune to the outlier because it estimates the conditional median.
  * **C.** Mean Absolute Error ($L_1$) rotates its regression line more aggressively than $L_2$ squared error because its penalty grows quadratically.
  * **D.** Ridge regression with $\alpha = 0.01$ eliminates the influence of the outlier completely by forcing its weight to zero.
  
  ---
#### Q4. Pipeline Hygiene & Dimensionality Reduction
  In a genomics classification study with $N = 100$ patient samples and $d = 20{,}000$ gene expression features ($p \gg n$), which of the following workflows will cause **catastrophic selection leakage**?
  * **A.** Splitting data into train/test sets first, and fitting `SelectKBest` strictly on the training partition.
  * **B.** Fitting an unsupervised `StandardScaler` strictly on the training partition, then transforming the test partition.
  * **C.** Running `SelectKBest(f_classif, k=50)` on all 100 patients before performing cross-validation.
  * **D.** Encapsulating a `StandardScaler` and a `LogisticRegression` classifier inside an `sklearn.pipeline.Pipeline`.
  
  ---
#### Q5. Topological Boundaries of Linear Classifiers
  Consider a binary classification problem in $\mathbb{R}^2$ where the positive class ($Y=1$) forms an interior circular disk centered at the origin, and the negative class ($Y=0$) forms an exterior concentric ring:
  $$\text{Class 1: } x_1^2 + x_2^2 \le 1 \quad \text{vs.} \quad \text{Class 0: } x_1^2 + x_2^2 > 1$$
  Assuming **no feature engineering** is performed (the model is fed only raw coordinates $x_1$ and $x_2$), what is the decision boundary produced by standard Logistic Regression?
  * **A.** A smooth circular boundary separating the inner disk from the outer ring.
  * **B.** A flat, linear 1-dimensional hyperplane (a line) cutting across the space.
  * **C.** A piecewise parabolic decision contour that curves around the origin.
  * **D.** The model fails to compile because logistic regression cannot process 2D inputs.
  
  ---
#### Q6. Asymmetric Risk in Clinical Screening
  A machine learning model screens patients for an acute, fatal cardiac anomaly. Failing to identify an infected patient (False Negative) leads to cardiac arrest and death ($\text{Cost} = \$100{,}000$), whereas flagging a healthy patient for follow-up testing (False Positive) incurs an outpatient echocardiogram cost ($\text{Cost} = \$400$). 
  
  To deploy this model safely, which evaluation metric must be maximized, and how should the probability threshold $\tau$ be configured relative to the default symmetric threshold ($\tau = 0.50$)?
  * **A.** Maximize **Precision**; raise decision threshold $\tau$ to $0.85$.
  * **B.** Maximize **Specificity**; raise decision threshold $\tau$ to $0.85$.
  * **C.** Maximize **Recall (Sensitivity)**; lower decision threshold $\tau$ below $0.50$.
  * **D.** Maximize **Accuracy**; keep decision threshold $\tau = 0.50$.
  
  ---
## Part B: True / False with Brief Justification (6 Questions $\times$ 2 Points = 12 Points)
  
  *Write **T** (True) or **F** (False), and provide a **one-sentence technical justification** for each.*
  
  * **Q7. [ $\quad$ ]** Monotonically scaling a continuous feature (e.g., via `StandardScaler` or a logarithmic transform) will fundamentally alter the split thresholds, tree topology, and predictive performance of a Decision Tree.  
  *Justification:* _________________________________________________________________
  
  * **Q8. [ $\quad$ ]** In regularized Ridge regression, adding the diagonal term $\alpha I$ (where $\alpha > 0$) guarantees that the matrix $(X^T X + \alpha I)$ is strictly positive definite and invertible, even if the dataset has far more features than samples ($d \gg n$).  
  *Justification:* _________________________________________________________________
  
  * **Q9. [ $\quad$ ]** A $1$-Nearest Neighbor ($1$-NN) classifier has a time complexity of $\mathcal{O}(1)$ during training, but requires an exhaustive linear scan scaling as $\mathcal{O}(n \cdot d)$ for every query point during inference.  
  *Justification:* _________________________________________________________________
  
  * **Q10. [ $\quad$ ]** If an Ordinary Least Squares linear regression model includes an intercept term $w_0$, the sum of the empirical training residuals $\sum_{i=1}^n (y_i - \hat{y}_i)$ is mathematically guaranteed to equal zero.  
  *Justification:* _________________________________________________________________
  
  * **Q11. [ $\quad$ ]** Conditioning on a collider variable ($X \to S \leftarrow Y$) in a causal DAG blocks the backdoor path and eliminates spurious statistical correlation between two marginally independent variables $X$ and $Y$.  
  *Justification:* _________________________________________________________________
  
  * **Q12. [ $\quad$ ]** Applying `StandardScaler` to a right-skewed, heavy-tailed feature eliminates its skewness, shifting the Fisher-Pearson skewness coefficient $\gamma_1$ to zero.  
  *Justification:* _________________________________________________________________
  
  ---
## Part C: Applied Systems & Leakage Forensics (2 Questions = 20 Points)
### Q13. Leak Detective (2 Scenarios $\times$ 5 Points = 10 Points)
  *For each scenario: State whether there is a **Leak** or **No Leak**. If it leaks, identify the **specific category** (Preprocessing, Target, Temporal, or Test-Set Reuse) and provide a **one-line programmatic fix**.*
  
  * **(a)** A predictive maintenance model forecasts whether an industrial wind turbine will suffer gearbox failure within the next 30 days. The design matrix includes the sensor feature: `maintenance_technician_dispatch_flag` (indicating whether a repair crew has been scheduled).  
  * **Verdict:** ___________________________  
  * **Leakage Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  * **(b)** A quantitative finance team trains an autoregressive model to forecast next week's closing stock price using 5 years of daily trading data (2020–2025). The data scientist partitions the records using:
  ```python
  X_tr, X_te, y_tr, y_te = train_test_split(
      X, y, test_size=0.20, shuffle=True, random_state=42
  )
  ```
  * **Verdict:** ___________________________  
  * **Leakage Category:** ___________________________  
  * **One-Line Fix:** ______________________________________________________________
  
  ---
### Q14. Gradient Descent Optimization Regimes (10 Points)
  A supervised training dataset contains $n = 50{,}000$ tabular instances and $d = 200$ features. 
  
  * **(a) [2 pts]** If an engineer configures Mini-Batch Gradient Descent with batch size $B = 250$, how many parameter update steps are executed per single epoch?  
  *Calculation:* _________________________________________________________________
  
  * **(b) [2 pts]** How many total parameter updates occur if this model is trained for $20$ complete epochs?  
  *Calculation:* _________________________________________________________________
  
  * **(c) [6 pts]** Three loss curves are recorded during optimization:
  * **Curve 1:** Decreases monotonically at every step, forming a smooth curve.
  * **Curve 2:** Exhibits a noisy trajectory with random oscillations, trending downward slowly.
  * **Curve 3:** Stalls immediately after 10 updates, forming an almost horizontal line that barely decreases across 1,000 steps.
  
  *Match each curve to its underlying optimization cause:*
  1. *Curve 1 corresponds to:* _________________________________________________
  2. *Curve 2 corresponds to:* _________________________________________________
  3. *Curve 3 corresponds to:* _________________________________________________
  
  ---
## Part D: Calculations & Hand Solves (3 Questions = 50 Points)
### Q15. Ordinary Least Squares & Gradient Descent Steps by Hand (18 Points)
  Given an empirical dataset of $n = 4$ observations:
  $$(x_1, y_1) = (1, 1), \quad (x_2, y_2) = (2, 4), \quad (x_3, y_3) = (3, 4), \quad (x_4, y_4) = (4, 7)$$
  We fit a simple linear regression model with intercept: $\hat{y} = w_0 + w_1 x$.
  
  * **(a) [8 pts]** Compute the sample means $\bar{x}$ and $\bar{y}$. Calculate $\sum (x_i - \bar{x})^2$ and $\sum (x_i - \bar{x})(y_i - \bar{y})$. Solve analytically for the optimal slope $w_1$ and intercept $w_0$. Write the final prediction line equation.
  
  \
  \
  \
  \
  \
  \
  
  * **(b) [5 pts]** Starting from initial weights $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$ with learning rate $\eta = 0.05$, compute **ONE parameter update step of Batch Gradient Descent** on the mean squared cost $J = \frac{1}{n}\sum_{i=1}^n (\hat{y}_i - y_i)^2$. Find $w_0^{(1)}$ and $w_1^{(1)}$.
  
  \
  \
  \
  \
  \
  \
  
  * **(c) [5 pts]** Using your analytical line from part (a), calculate the residuals $r_i = y_i - \hat{y}_i$ for all $4$ points. Verify explicitly that their sum $\sum_{i=1}^4 r_i$ equals zero.
  
  \
  \
  \
  \
  
  ---
### Q16. Logistic Regression, Probabilities & Metric Evaluation (16 Points)
  
  A fitted binary logistic regression model in $\mathbb{R}^2$ has parameter weights $w = [2.0, \; -1.0]^T$ and scalar bias $b = -2.0$. An unobserved test observation arrives at $x = [2.0, \; 1.0]^T$.
  
  * **(a) [4 pts]** Compute the linear score (logit) $z = w^T x + b$ and the posterior probability $\hat{p} = \sigma(z) = \frac{1}{1 + e^{-z}}$. Round to 3 decimal places. (Note: $e^{-1} \approx 0.368$).
  
  \
  \
  \
  \
  
  * **(b) [4 pts]** Classify this instance under decision threshold $\tau = 0.50$. What is the predicted class if the threshold is shifted to $\tau = 0.80$?
  
  \
  \
  \
  \
  
  * **(c) [8 pts]** A screening test is evaluated on $1{,}000$ patient mammograms. Exactly $50$ patients have malignant tumors ($P = 50$). The classifier flags $80$ mammograms as malignant ($\hat{P} = 80$); among those flagged, $40$ are confirmed malignant.
  1. Determine the counts for $TP$, $FP$, $FN$, and $TN$.
  2. Compute **Precision**.
  3. Compute **Recall (Sensitivity)**.
  4. Compute the **$F_1$-score**.
  
  \
  \
  \
  \
  \
  \
  
  ---
### Q17. Naive Bayes Classification by Hand with Laplace Smoothing (16 Points)
  A spam filter is trained on $N = 12$ historical emails:
  * **Spam ($Y = 1$):** $4$ emails ($P(\text{Spam}) = \frac{4}{12}$)
  * **Ham ($Y = 0$):** $8$ emails ($P(\text{Ham}) = \frac{8}{12}$)
  
  Two binary word features are tracked: $X_1 = \mathbf{1}_{\{\text{"cash"}\}}$ and $X_2 = \mathbf{1}_{\{\text{"project"}\}}$.
  * Word `"cash"` appears in $3$ of the $4$ spam emails, and in $1$ of the $8$ ham emails.
  * Word `"project"` appears in $0$ of the $4$ spam emails, and in $6$ of the $8$ ham emails.
  
  An incoming unclassified email contains **both words**: $x_{\text{test}} = [\text{"cash"} = 1, \; \text{"project"} = 1]$.
  
  * **(a) [4 pts]** Without smoothing, compute the class-conditional likelihood $P(x_{\text{test}} \mid \text{Spam})$ under the Naive Bayes assumption. Explain why Maximum Likelihood Estimation fails on this query.
  
  \
  \
  \
  \
  
  * **(b) [6 pts]** Apply **Laplace Smoothing ($\alpha = 1$, binary cardinality $K = 2$)** to all feature likelihoods:
  $$\hat{P}(X_j = 1 \mid Y = c) = \frac{N_{jc} + 1}{N_c + 2}$$
  Calculate the smoothed likelihoods:
  * $P_{\text{Lap}}(\text{cash} = 1 \mid \text{Spam})$
  * $P_{\text{Lap}}(\text{project} = 1 \mid \text{Spam})$
  * $P_{\text{Lap}}(\text{cash} = 1 \mid \text{Ham})$
  * $P_{\text{Lap}}(\text{project} = 1 \mid \text{Ham})$
  
  \
  \
  \
  \
  \
  \
  
  * **(c) [6 pts]** Using the smoothed feature likelihoods and original class priors ($P(\text{Spam}) = \frac{1}{3}$, $P(\text{Ham}) = \frac{2}{3}$), compute the unnormalized joint probabilities $P(\text{Spam}, x_{\text{test}})$ and $P(\text{Ham}, x_{\text{test}})$. Calculate the final normalized posterior probability $P(\text{Spam} \mid x_{\text{test}})$, and state the predicted label.
  
  \
  \
  \
  \
  \
  \
  
  ---
  $$\mathbf{=== \text{ STOP! END OF EXAM. SOLUTIONS BELOW } ===}$$
  ---
  
  \pagebreak
# Comprehensive Exam 1 Solutions & Grading Rubric
  
  ---
## Part A: Multiple Choice Solutions (18 Points)
  
  * **Q1: A (`(5, 3, 4)`)** [3 pts]  
  *Derivation:* Array $A$ has shape `(5, 1, 4)`. Array $B$ has shape `(3, 1)`, which right-aligns as `(1, 3, 1)`. Trailing dimension: $4 \text{ vs } 1 \to 4$. Middle dimension: $1 \text{ vs } 3 \to 3$. Leading dimension: $5 \text{ vs } 1 \to 5$. Output broadcasts to `(5, 3, 4)`.
  * **Q2: B ($\eta = 0.025$)** [3 pts]  
  *Derivation:* Maximum eigenvalue of the Hessian is $\lambda_{\max} = 80.0$. The mathematical convergence upper bound is $\eta < \frac{2}{\lambda_{\max}} = \frac{2}{80.0} = 0.025$. Any step size $\ge 0.025$ oscillates with growing amplitude and diverges.
  * **Q3: A** [3 pts]  
  *Derivation:* OLS optimizes squared residuals ($r_i^2$), meaning high-leverage outliers exert quadratic influence, pivoting the regression line. A Huber Regressor transitions to linear absolute penalties for $|r| > \delta$, bounding the gradient force to $\pm \delta$.
  * **Q4: C** [3 pts]  
  *Derivation:* `SelectKBest` computes supervised statistical tests across $(X, y)$. Running this before splitting exposes the test labels to the feature selector, selecting noise features that happen to correlate with test labels by chance (selection leakage).
  * **Q5: B** [3 pts]  
  *Derivation:* The decision boundary of logistic regression is defined by $\sigma(w^T x + b) = 0.50 \iff w^T x + b = 0$. In raw feature coordinates, this equation defines a flat 1D line in $\mathbb{R}^2$. Without non-linear basis expansions, it cannot form a circle.
  * **Q6: C** [3 pts]  
  *Derivation:* Severe clinical costs for False Negatives require maximizing Recall ($\frac{TP}{TP + FN}$). Lowering threshold $\tau$ below $0.50$ (e.g., to $0.10$) catches near-$100\%$ of true positives, trading off extra false alarms to prevent patient mortality.
  
  ---
## Part B: True / False Solutions (12 Points)
  
  * **Q7: FALSE** [2 pts]  
  *Justification:* Decision trees evaluate greedy split thresholds based strictly on the rank order of samples ($x_j \le \theta$). Any strictly monotonic transformation preserves relative order, leaving tree partitions, Gini impurity, and predictions identical.
  * **Q8: TRUE** [2 pts]  
  *Justification:* The eigenvalues of $(X^T X + \alpha I)$ equal $\lambda_i(X^T X) + \alpha \ge \alpha > 0$. Since all eigenvalues are strictly positive real numbers, the determinant is non-zero, guaranteeing strict positive definiteness and invertibility.
  * **Q9: TRUE** [2 pts]  
  *Justification:* $k$-NN is a lazy learner: training simply stores pointers in memory ($\mathcal{O}(1)$), while inference requires computing distances to all $n$ training instances across $d$ dimensions ($\mathcal{O}(n \cdot d)$).
  * **Q10: TRUE** [2 pts]  
  *Justification:* The first row of the Normal Equations $X^T (y - \hat{y}) = \mathbf{0}$ corresponds to the augmented intercept column of ones ($\mathbf{1}^T r = \sum r_i = 0$), forcing the sum of residuals to equal zero.
  * **Q11: FALSE** [2 pts]  
  *Justification:* Conditioning on a common collider ($X \to S \leftarrow Y$) opens the pathway between $X$ and $Y$, inducing a spurious correlation between two variables that were marginally independent (Berkson’s Fallacy).
  * **Q12: FALSE** [2 pts]  
  *Justification:* `StandardScaler` is an affine linear transformation ($z = ax + b$); standardized moments such as skewness ($\gamma_1$) are invariant under positive linear scaling ($\gamma_1(aX + b) \equiv \gamma_1(X)$).
  
  ---
## Part C: Applied Systems & Leakage Forensics Solutions (20 Points)
### Q13. Leak Detective Solutions [10 pts]
  * **(a) Wind Turbine Model:**
  * **Verdict:** **LEAK** [1 pt]
  * **Category:** **Target Leakage** (Downstream Operational Proxy) [2 pts]
  * **One-Line Fix:** `df = df.drop(columns=['maintenance_technician_dispatch_flag'])` [2 pts]
  * *Reasoning:* A dispatch flag is populated only *after* operators or monitoring systems have already detected failure risk.
  * **(b) Financial Stock Predictor:**
  * **Verdict:** **LEAK** [1 pt]
  * **Category:** **Temporal Leakage** (Arrow-of-Time Inversion) [2 pts]
  * **One-Line Fix:** Split chronologically using `TimeSeriesSplit` or a date mask without shuffling: `X_tr, X_te = X[date < '2024-01-01'], X[date >= '2024-01-01']` [2 pts]
  * *Reasoning:* Shuffling time-series data allows future prices to predict past dates, violating operational reality.
  
  ---
### Q14. Gradient Descent Regimes Solutions [10 pts]
  * **(a) Updates per Epoch:** [2 pts]
  $$\text{Updates per Epoch} = \frac{n}{B} = \frac{50{,}000}{250} = \mathbf{200 \text{ updates per epoch}}$$
  * **(b) Total Updates across 20 Epochs:** [2 pts]
  $$\text{Total Updates} = 200 \times 20 = \mathbf{4{,}000 \text{ updates}}$$
  * **(c) Loss Curve Matching:** [6 pts, 2 pts each]
  1. *Curve 1:* **Batch Gradient Descent (BGD)**. (Evaluates all $n$ samples; deterministic gradient produces smooth monotonic decrease).
  2. *Curve 2:* **Stochastic Gradient Descent (SGD)**. (Batch size $B=1$; maximal sampling variance produces noisy, jagged oscillations).
  3. *Curve 3:* **Under-Tuned Learning Rate ($\eta$ too small)**. (Infinitesimal step size stalls progress, producing a flat crawl).
  
  ---
## Part D: Calculations & Hand Solves Solutions (50 Points)
### Q15. Ordinary Least Squares & Gradient Steps Solution (18 Points)
#### (a) OLS Analytical Solve [8 pts]
  1. *Compute Means:* [2 pts]
   $$\bar{x} = \frac{1 + 2 + 3 + 4}{4} = \mathbf{2.500}, \quad \bar{y} = \frac{1 + 4 + 4 + 7}{4} = \frac{16}{4} = \mathbf{4.000}$$
  2. *Compute Variance and Covariance Sums:* [3 pts]
   * $(x_i - \bar{x}) \in \{-1.5, \; -0.5, \; +0.5, \; +1.5\} \implies \sum (x_i - \bar{x})^2 = 2.25 + 0.25 + 0.25 + 2.25 = \mathbf{5.000}$
   * $(y_i - \bar{y}) \in \{-3.0, \; 0.0, \; 0.0, \; +3.0\}$
   * $\sum (x_i - \bar{x})(y_i - \bar{y}) = (-1.5)(-3.0) + (-0.5)(0.0) + (0.5)(0.0) + (1.5)(3.0) = 4.5 + 0 + 0 + 4.5 = \mathbf{9.000}$
  3. *Solve Parameters:* [3 pts]
   $$w_1 = \frac{9.000}{5.000} = \mathbf{1.800}$$
   $$w_0 = \bar{y} - w_1 \bar{x} = 4.000 - (1.800 \times 2.500) = 4.000 - 4.500 = \mathbf{-0.500}$$
   $$\mathbf{\hat{y} = -0.500 + 1.800x}$$
  
  ---
#### (b) One Step of Batch Gradient Descent [5 pts]
  * Initial weights: $w_0^{(0)} = 0, \; w_1^{(0)} = 0$. $\eta = 0.05, \; n = 4$.
  * Initial predictions: $\hat{y}_i = 0 + 0(x_i) = 0$.
  * Error residuals $e_i = \hat{y}_i - y_i = -y_i$:
  $$e = [-1, \; -4, \; -4, \; -7]^T$$
  * Compute gradients: [2 pts]
  $$\nabla_{w_0} J = \frac{2}{n}\sum e_i = \frac{2}{4}(-1 - 4 - 4 - 7) = \frac{1}{2}(-16) = \mathbf{-8.000}$$
  $$\nabla_{w_1} J = \frac{2}{n}\sum x_i e_i = \frac{2}{4}\Big[ 1(-1) + 2(-4) + 3(-4) + 4(-7) \Big] = \frac{1}{2}[-1 - 8 - 12 - 28] = \frac{1}{2}(-49) = \mathbf{-24.500}$$
  * Execute parameter updates: [3 pts]
  $$w_0^{(1)} = 0 - 0.05(-8.000) = \mathbf{+0.400}$$
  $$w_1^{(1)} = 0 - 0.05(-24.500) = \mathbf{+1.225}$$
  
  ---
#### (c) Residual Zero-Sum Verification [5 pts]
  Using $\hat{y} = -0.500 + 1.800x$:
  * $x_1 = 1 \implies \hat{y}_1 = 1.300 \implies r_1 = y_1 - \hat{y}_1 = 1 - 1.300 = \mathbf{-0.300}$
  * $x_2 = 2 \implies \hat{y}_2 = 3.100 \implies r_2 = y_2 - \hat{y}_2 = 4 - 3.100 = \mathbf{+0.900}$
  * $x_3 = 3 \implies \hat{y}_3 = 4.900 \implies r_3 = y_3 - \hat{y}_3 = 4 - 4.900 = \mathbf{-0.900}$
  * $x_4 = 4 \implies \hat{y}_4 = 6.700 \implies r_4 = y_4 - \hat{y}_4 = 7 - 6.700 = \mathbf{+0.300}$
  * Sum of residuals:
  $$\sum_{i=1}^4 r_i = -0.300 + 0.900 - 0.900 + 0.300 = \mathbf{0.000} \quad \text{[Verified! Enforced by } \partial J/\partial w_0 = 0\text{]}$$
  
  ---
### Q16. Logistic Regression & Contingency Calculations Solution (16 Points)
#### (a) Logit and Probability [4 pts]
  $$z = w^T x + b = 2.0(2.0) + (-1.0)(1.0) - 2.0 = 4.0 - 1.0 - 2.0 = \mathbf{1.000}$$ [2 pts]
  $$\hat{p} = \sigma(1.000) = \frac{1}{1 + e^{-1.0}} \approx \frac{1}{1 + 0.368} = \frac{1}{1.368} \approx \mathbf{0.731}$$ [2 pts]
  
  ---
#### (b) Decision Thresholding [4 pts]
  * At $\tau = 0.50$: $\hat{p} = 0.731 \ge 0.50 \implies \mathbf{\hat{y} = 1 \text{ (Positive)}}$ [2 pts]
  * At $\tau = 0.80$: $\hat{p} = 0.731 < 0.80 \implies \mathbf{\hat{y} = 0 \text{ (Negative)}}$ [2 pts]
  
  ---
#### (c) Mammography Screening Metrics [8 pts]
  1. *Confusion Matrix Counts:* [2 pts]
   * $TP = 40$ (flagged and truly malignant)
   * $FP = 80 - 40 = 40$ (flagged, but benign)
   * $FN = 50 - 40 = 10$ (malignant, but missed)
   * $TN = 950 - 40 = 910$ (benign, correctly unflagged)
  2. *Precision:* [2 pts]
   $$\text{Precision} = \frac{TP}{TP + FP} = \frac{40}{40 + 40} = \frac{40}{80} = \mathbf{0.500 \quad (50.0\%)}$$
  3. *Recall (Sensitivity):* [2 pts]
   $$\text{Recall} = \frac{TP}{TP + FN} = \frac{40}{40 + 10} = \frac{40}{50} = \mathbf{0.800 \quad (80.0\%)}$$
  4. *$F_1$-Score:* [2 pts]
   $$F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} = 2 \cdot \frac{0.500 \times 0.800}{0.500 + 0.800} = \frac{0.800}{1.300} = \frac{8}{13} \approx \mathbf{0.615}$$
  
  ---
### Q17. Naive Bayes Classification with Laplace Smoothing Solution (16 Points)
#### (a) Zero-Probability MLE Defect [4 pts]
  Under un-smoothed Maximum Likelihood Estimation:
  $$P(\text{project} = 1 \mid \text{Spam}) = \frac{0}{4} = 0.0$$
  Because the word `"project"` never appeared in training spam emails:
  $$P(x_{\text{test}} \mid \text{Spam}) = P(\text{cash}=1 \mid \text{Spam}) \times P(\text{project}=1 \mid \text{Spam}) = \left(\frac{3}{4}\right) \times 0.0 = \mathbf{0.0}$$
  *Evaluation Failure:* A single zero likelihood forces the entire joint product to zero, completely blinding the model to the fact that `"cash"` is strong evidence for spam.
  
  ---
#### (b) Laplace Smoothing ($\alpha = 1, K = 2$) [6 pts]
  $$\hat{P}(X_j = 1 \mid Y = c) = \frac{N_{jc} + 1}{N_c + 2}$$
  * $P_{\text{Lap}}(\text{cash} = 1 \mid \text{Spam}) = \frac{3 + 1}{4 + 2} = \frac{4}{6} = \mathbf{\frac{2}{3} \approx 0.667}$ [1.5 pts]
  * $P_{\text{Lap}}(\text{project} = 1 \mid \text{Spam}) = \frac{0 + 1}{4 + 2} = \mathbf{\frac{1}{6} \approx 0.167}$ [1.5 pts]
  * $P_{\text{Lap}}(\text{cash} = 1 \mid \text{Ham}) = \frac{1 + 1}{8 + 2} = \frac{2}{10} = \mathbf{\frac{1}{5} = 0.200}$ [1.5 pts]
  * $P_{\text{Lap}}(\text{project} = 1 \mid \text{Ham}) = \frac{6 + 1}{8 + 2} = \frac{7}{10} = \mathbf{0.700}$ [1.5 pts]
  
  ---
#### (c) Joint Probabilities & Posterior Normalization [6 pts]
  Using priors $P(\text{Spam}) = \frac{4}{12} = \frac{1}{3}$ and $P(\text{Ham}) = \frac{8}{12} = \frac{2}{3}$:
  1. *Unnormalized Joint Probabilities:* [2 pts]
   $$P(\text{Spam}, x) = P(\text{Spam}) \cdot P_{\text{Lap}}(\text{cash}\mid\text{Spam}) \cdot P_{\text{Lap}}(\text{project}\mid\text{Spam}) = \frac{1}{3} \times \frac{2}{3} \times \frac{1}{6} = \frac{2}{54} = \mathbf{\frac{1}{27} \approx 0.03704}$$
   $$P(\text{Ham}, x) = P(\text{Ham}) \cdot P_{\text{Lap}}(\text{cash}\mid\text{Ham}) \cdot P_{\text{Lap}}(\text{project}\mid\text{Ham}) = \frac{2}{3} \times \frac{1}{5} \times \frac{7}{10} = \frac{14}{150} = \mathbf{\frac{7}{75} \approx 0.09333}$$
  2. *Marginal Evidence $P(x)$:* [1 pt]
   $$P(x) = \frac{1}{27} + \frac{7}{75} = \frac{25 + 63}{675} = \frac{88}{675} \approx \mathbf{0.13037}$$
  3. *Normalized Posteriors & Decision:* [3 pts]
   $$P(\text{Spam} \mid x) = \frac{25 / 675}{88 / 675} = \frac{25}{88} \approx \mathbf{0.284 \quad (28.4\%)}$$
   $$P(\text{Ham} \mid x) = \frac{63 / 675}{88 / 675} = \frac{63}{88} \approx \mathbf{0.716 \quad (71.6\%)}$$
   $$\mathbf{\arg\max \{ 0.284, \; 0.716 \} \implies \text{Predicted Label: } \mathbf{Ham}}$$