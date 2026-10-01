## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **When `MinMaxScaler` Outputs Values Greater Than 1.0:**
  * `MinMaxScaler` computes:
   $$x' = \frac{x - x_{\min, \, \text{train}}}{x_{\max, \, \text{train}} - x_{\min, \, \text{train}}}$$
  * If a test observation contains an unseen extreme value that exceeds the maximum observed during training ($x_{\text{test}} > x_{\max, \, \text{train}}$), the numerator exceeds the denominator. The transformer outputs a normalized value strictly greater than $1.0$ (as demonstrated in P7, where $x_{\text{test}} = 60$ mapped to $1.250$).
  2. **Why Normal Equations Guarantee That Residuals Sum to Zero ($\sum r_i = 0$):**
  * The Normal Equations state:
   $$X^T (y - Xw^*) = X^T r = \mathbf{0}$$
  * When an intercept term $w_0$ is included, the first column of the augmented design matrix is the constant vector of ones ($\mathbf{1} = [1, 1, \dots, 1]^T$).
  * The first row of the matrix product $X^T r$ evaluates the dot product between $\mathbf{1}$ and the residual vector $r$:
   $$\mathbf{1}^T r = \sum_{i=1}^n r_i = 0 \implies \mathbf{\bar{r} = 0}$$
  * The presence of an intercept mathematically forces the sample mean of the residuals to be identically zero.
  3. **Why Standardization Protects Features Under $L_1$ Lasso Regularization:**
  * In linear models, the parameter magnitude scales inversely with feature magnitude: $w_j \propto \frac{1}{\text{Scale}(X_j)}$.
  * A feature measured in small units (e.g., millimeters) requires an artificially large parameter weight to influence predictions. Because the $L_1$ penalty $\lambda \sum |w_j|$ applies a uniform linear shrinkage cost, large coefficients are heavily penalized and driven to zero by the soft-thresholding operator. 
  * Standardizing all features to unit variance ($\sigma_j = 1$) removes arbitrary unit scaling, ensuring features are selected based on predictive correlation rather than measurement magnitude.
  
  ---
# Part B: True / False Diagnostic Verification (T1–T12)
  
  ---
### T1. Hyperparameters vs. Fitted Parameters
  > **Statement:** *Hyperparameters (e.g., $k$ in $k$-NN, $\alpha$ in Ridge) are learned from the training data by the fitting algorithm.*
  
  * **Verdict:** **FALSE**
  * **The Mathematical & Systems Proof:**
  * In machine learning, we distinguish strictly between **Parameters** ($\theta$) and **Hyperparameters** ($\lambda$):
    * **Parameters ($\theta$):** Internal coefficients (e.g., weight vector $w$, intercept $b$, decision tree split thresholds). They are optimized directly by the learning algorithm during `.fit()` via empirical risk minimization:
      $$\theta^* = \arg\min_\theta \frac{1}{n}\sum_{i=1}^n \mathcal{L}(y_i, f_\theta(x_i))$$
    * **Hyperparameters ($\lambda$):** External architectural configurations (e.g., neighborhood size $k$, regularization strength $\alpha$, maximum tree depth, learning rate $\eta$). They dictate the structure of the hypothesis space $\mathcal{H}$ and cannot be learned by simple loss minimization on the training set.
  * **The Failure Mode:** If an algorithm attempted to learn $\alpha$ in Ridge regression by minimizing training error, it would always choose $\alpha = 0$ (unregularized OLS). Hyperparameters must be tuned externally across hold-out validation folds via grid search or Bayesian optimization.
  
  ---
### T2. Random Seeds Across Library Toolchains
  > **Statement:** *Setting random seeds guarantees identical results across different library versions.*
  
  * **Verdict:** **FALSE**
  * **The Mathematical & Systems Proof:**
  * A random seed initializes the internal state register of a **Pseudorandom Number Generator (PRNG)** (e.g., Mersenne Twister MT19937 or PCG64).
  * However, software libraries (such as NumPy, PyTorch, and scikit-learn) frequently refactor underlying C/C++ routines, alter default convergence tolerances, update numerical BLAS/LAPACK backends, or re-order parallel thread loops between releases.
  * **Empirical Reality (Lecture 4):** A script seeded with `seed_all(42)` on scikit-learn `v1.4` is not guaranteed to produce identical numerical splits, tree node allocations, or floating-point values on scikit-learn `v1.6`. Bitwise repeatability across time requires pinning complete software dependency lockfiles (`requirements.txt`, Docker containers) in addition to seeding PRNGs.
  
  ---
### T3. Tensor Axis Reduction Mechanics
  > **Statement:** *For $X$ with shape $(6, 4)$, $X.\text{sum}(\text{axis}=1)$ has shape $(6,)$.*
  
  * **Verdict:** **TRUE**
  * **The Mathematical & Systems Proof:**
  * By the **Axis Principle** (Lecture 5): *"An axis argument names the axis that disappears."*
  * The matrix $X \in \mathbb{R}^{6 \times 4}$ has two axes:
    * $\text{axis}=0$: Rows (length 6)
    * $\text{axis}=1$: Columns (length 4)
  * Invoking `.sum(axis=1)` collapses dimension 1 by summing horizontally across all 4 columns for each row:
    $$s_i = \sum_{j=1}^4 X_{ij} \quad \text{for } i \in \{1, \dots, 6\}$$
  * The operation collapses the second dimension, producing a 1D NumPy array of length 6: **shape `(6,)`**.
  
  ---
### T4. Distribution Reshaping: Log Transformations
  > **Statement:** *A log transform is a reasonable choice for a right-skewed feature such as income.*
  
  * **Verdict:** **TRUE**
  * **The Mathematical & Systems Proof:**
  * Right-skewed distributions (e.g., personal wealth, housing values, website traffic) possess positive skewness ($\gamma_1 > 0$), where a dense cluster of observations sits near a lower bound while a long, sparse tail stretches toward positive infinity.
  * The natural logarithm $g(x) = \ln(x)$ (or $g(x) = \ln(x + 1)$ for non-negative domains) is a strictly concave monotonic transformation:
    $$\frac{d}{dx} \ln(x) = \frac{1}{x} \implies \lim_{x \to \infty} g'(x) = 0$$
  * It compresses large values in the positive tail far more aggressively than smaller values near the origin, pulling the extreme tail inward, stabilizing variance, and making the distribution more symmetric and Gaussian-like for linear models.
  
  ---
### T5. Production Encoders: `handle_unknown='ignore'`
  > **Statement:** *`OneHotEncoder(handle_unknown='ignore')` prevents a crash when a category unseen in training appears in the test set.*
  
  * **Verdict:** **TRUE**
  * **The Mathematical & Systems Proof:**
  * By default, `OneHotEncoder(handle_unknown='error')` raises a fatal `ValueError` if it encounters a category at inference time that was not present in the training set vocabulary.
  * Setting `handle_unknown='ignore'` instructs the transformer to output an **all-zero binary vector** ($[0, 0, \dots, 0]$) for any novel categorical string (Lecture 8).
  * This allows the pipeline to complete inference without throwing an unhandled exception, treating the novel observation as possessing zero activation across known training categories.
  
  ---
### T6. Pre-Split Feature Selection Leakage
  > **Statement:** *Running `SelectKBest` on the full dataset (using all labels) before the train/test split leaks information.*
  
  * **Verdict:** **TRUE**
  * **The Mathematical & Systems Proof:**
  * Supervised feature selectors like `SelectKBest(score_func=f_classif)` calculate statistical associations (such as ANOVA $F$-ratios or mutual information) between feature vectors $X_j$ and target labels $y$.
  * If computed over the full dataset ($N_{\text{train}} + N_{\text{test}}$), the algorithm evaluates which features correlate with the **labels of the held-out test set**.
  * As proven in Lecture 9's simulation (where $d = 10{,}000$ and $n = 1{,}000$), pre-split feature selection picks random noise features that happen to correlate with test labels by chance alone, fabricating an artificial $95\%$ accuracy on pure white noise that collapses to $50\%$ in production. The split must occur before feature selection.
  
  ---
### T7. Optimization Regimes: Batch Gradient Descent Steps
  > **Statement:** *With $n = 1{,}000$ samples, batch gradient descent performs $1{,}000$ parameter updates per epoch.*
  
  * **Verdict:** **FALSE**
  * **The Mathematical & Systems Proof:**
  * An **epoch** is defined as one complete pass through the entire training dataset.
  * In **Batch Gradient Descent (BGD)**, the algorithm aggregates the loss gradient across **all $n$ samples** simultaneously to execute a single step:
    $$w^{(t+1)} = w^{(t)} - \eta \cdot \frac{1}{n} \sum_{i=1}^n \nabla_w \mathcal{L}(y_i, f(x_i; w^{(t)}))$$
  * Therefore, Batch Gradient Descent performs exactly **1 parameter update per epoch**.
  * Performing $1{,}000$ parameter updates on $1{,}000$ samples describes **Stochastic Gradient Descent (SGD)**, which updates parameters after every individual sample ($B = 1$).
  
  ---
### T8. Mini-Batch Equivalence to Full Batch
  > **Statement:** *Mini-batch gradient descent with `batch_size` equal to the training-set size is identical to batch gradient descent.*
  
  * **Verdict:** **TRUE**
  * **The Mathematical & Systems Proof:**
  * The Mini-Batch Gradient Descent update rule evaluates a subset $\mathcal{B}$ of size $B$:
    $$w^{(t+1)} = w^{(t)} - \eta \cdot \frac{1}{B} \sum_{i \in \mathcal{B}} \nabla_w \mathcal{L}(y_i, f(x_i; w^{(t)}))$$
  * When $B = n$, the mini-batch $\mathcal{B}$ contains all $n$ observations in the training set:
    $$\frac{1}{B} \sum_{i \in \mathcal{B}} \nabla_w \mathcal{L}_i \equiv \frac{1}{n} \sum_{i=1}^n \nabla_w \mathcal{L}_i$$
  * The mini-batch gradient estimator becomes identical in expectation and variance to the full-batch gradient, executing one update per epoch over the full dataset.
  
  ---
### T9. Decision Surface Geometry of Logistic Regression
  > **Statement:** *Without feature engineering, logistic regression has a linear decision boundary.*
  
  * **Verdict:** **TRUE**
  * **The Mathematical & Systems Proof:**
  * While the logistic sigmoid function $\sigma(z) = \frac{1}{1 + e^{-z}}$ is a non-linear S-shaped curve in probability space, class assignments are determined by thresholding the logit score at $P(Y=1 \mid x) = 0.50$:
    $$\sigma(w^T x + b) = 0.50 \iff w^T x + b = 0$$
  * In the raw feature space $\mathbb{R}^d$, the equation $w^T x + b = 0$ is a **flat, $(d-1)$-dimensional affine hyperplane** (Lecture 13).
  * Logistic regression can only construct non-linear decision boundaries if an engineer explicitly expands the design matrix with non-linear basis features (such as polynomials, interactions, or splines).
  
  ---
### T10. Computational Complexity Profile of $k$-NN
  > **Statement:** *$k$-NN has an expensive training phase but very fast predictions.*
  
  * **Verdict:** **FALSE**
  * **The Mathematical & Systems Proof:**
  * The statement inverts the true computational profile of $k$-NN.
  * $k$-NN is a **lazy learner (instance-based model)** (Lecture 14):
    * **Training Phase:** Instantaneous / Free ($\mathcal{O}(1)$ time complexity). The algorithm does not optimize parameters; it simply stores the data matrix in memory.
    * **Prediction Phase:** Computationally expensive and memory-bound ($\mathcal{O}(n \cdot d)$ time complexity per query point). Generating a prediction requires an exhaustive linear scan calculating distances to all $n$ stored training samples.
  
  ---
### T11. Classification Metric Definitions: Precision vs. Recall
  > **Statement:** *$\text{Precision} = \frac{TP}{TP + FN}$.*
  
  * **Verdict:** **FALSE**
  * **The Mathematical & Systems Proof:**
  * The formula provided in the statement is the definition of **Recall** (also known as Sensitivity or True Positive Rate):
    $$\mathbf{\text{Recall} = \frac{TP}{TP + FN}}$$
    which measures the proportion of actual positive cases successfully identified.
  * **Precision** (Positive Predictive Value) evaluates the purity of positive predictions by dividing True Positives by all predicted positives:
    $$\mathbf{\text{Precision} = \frac{TP}{TP + FP}}$$
  
  ---
### T12. Multicollinearity and Gram Matrix Singularity
  > **Statement:** *If column $x_2$ is an exact copy of $x_1$, $X^T X$ is singular.*
  
  * **Verdict:** **TRUE**
  * **The Mathematical & Systems Proof:**
  * If feature column $x_2$ is identical to $x_1$ ($x_2 = 1 \cdot x_1$), the columns of the design matrix $X \in \mathbb{R}^{n \times (d+1)}$ are linearly dependent.
  * The rank of $X$ is strictly less than its feature dimension:
    $$\text{rank}(X) < d+1$$
  * By fundamental linear algebra, the rank of the Gram matrix equals the rank of $X$:
    $$\text{rank}(X^T X) = \text{rank}(X) < d+1$$
  * Because $X^T X$ is an $(d+1) \times (d+1)$ square matrix with rank strictly less than its dimension, it is **rank-deficient, non-invertible, and singular**:
    $$\det(X^T X) = 0$$
  * The unregularized Normal Equations cannot be solved via direct matrix inversion $(X^T X)^{-1}$, triggering a `LinAlgError`.
  
  ---
## Summary Review Table: Part B Answers
  
  ```
  ┌────────┬─────────┬─────────────────────────────────────────────────────────────┐
  │ Item   │ Verdict │ Core Machine Learning Principle Tested                      │
  ├────────┼─────────┼─────────────────────────────────────────────────────────────┤
  │ T1     │ FALSE   │ Hyperparameters are set externally; parameters are learned. │
  │ T2     │ FALSE   │ Seeds do not prevent library version or hardware drift.     │
  │ T3     │ TRUE    │ The axis argument names the axis that disappears (4 cols).  │
  │ T4     │ TRUE    │ Concave log transforms compress heavy positive skewness.    │
  │ T5     │ TRUE    │ handle_unknown='ignore' outputs all-zero vectors on novel.  │
  │ T6     │ TRUE    │ Supervised feature selection pre-split leaks test labels.   │
  │ T7     │ FALSE   │ Batch GD takes exactly ONE parameter update per epoch.      │
  │ T8     │ TRUE    │ When batch_size = n, mini-batch equals batch gradient desc. │
  │ T9     │ TRUE    │ Decision boundary is wᵀx + b = 0 (linear hyperplane).       │
  │ T10    │ FALSE   │ k-NN training is O(1) free; inference is O(n·d) expensive.  │
  │ T11    │ FALSE   │ Precision = TP / (TP + FP); TP / (TP + FN) is Recall.       │
  │ T12    │ TRUE    │ Collinear duplicate columns cause det(XᵀX) = 0 (singular).  │
  └────────┴─────────┴─────────────────────────────────────────────────────────────┘
  ```
  
  ---