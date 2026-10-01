### P1. Model Generalization Diagnostics
  **Question:** A model scores $99\%$ accuracy on training data and $71\%$ on test data. This is most likely:
  * A. Underfitting
  * B. Overfitting
  * C. Data leakage
  * D. A well-generalized model
  
  **Correct Answer:** **B. Overfitting**
#### Graduate-Level Analysis:
  * **The Mechanism:** Overfitting occurs when a high-capacity model memorizes sample-specific stochastic noise, idiosyncrasies, and non-generalizable correlations in the training partition. 
  * Mathematically, the empirical training risk approaches zero ($\hat{R}_{\text{train}} \to 0$, $99\%$ accuracy), while the true expected risk on unseen test data drawn from the population remains high ($R_{\text{test}} \gg \hat{R}_{\text{train}}$, $71\%$ accuracy). This manifests as a massive **generalization gap**:
  $$\text{Generalization Gap} = \text{Accuracy}_{\text{train}} - \text{Accuracy}_{\text{test}} = 99\% - 71\% = 28\%$$
  * **Why Distractors Are Wrong:**
  * *A (Underfitting):* Underfitting is characterized by **high bias**, where the model fails to capture the underlying signal, producing poor performance on *both* training and test partitions (e.g., $62\%$ train, $60\%$ test).
  * *C (Data Leakage):* Data leakage contaminates the training set with test set information, resulting in **unrealistically high performance on BOTH splits** (e.g., $99\%$ train, $98\%$ test).
  * *D (Well-generalized):* A well-generalized model exhibits small divergence between train and test metrics (e.g., $85\%$ train, $84\%$ test).
  * **Exam Pro-Tip:** A large spread between training and testing performance ($\Delta > 15\%$) is the textbook signature of high-variance overfitting.
  
  ---
### P2. Machine Learning Systems Failure Modes
  **Question:** Which is NOT one of the real-world ML failure modes discussed in class?
  * A. Distribution shift
  * B. Feedback loops
  * C. Metric blindness
  * D. GPU memory fragmentation
  
  **Correct Answer:** **D. GPU memory fragmentation**
#### Graduate-Level Analysis:
  * **The Mechanism:** In Lecture 2, Dr. Yu outlined the four fundamental, conceptual failure modes that cause machine learning systems to fail in real-world deployment:
  1. *Wrong Problem Framing / Proxy Trap* (including feedback loops where model predictions alter future data collection).
  2. *Distribution Shift* ($P_{\text{train}}(X, Y) \neq P_{\text{deploy}}(X, Y)$).
  3. *Data Leakage* (training on information unobservable at inference time).
  4. *Metric Blindness* (evaluating imbalanced datasets using collapsed metrics like accuracy).
  * **Why Distractors Are Wrong:**
  * *A, B, and C* are all primary conceptual ML pathologies covered in Lectures 2 and 12.
  * *D (GPU memory fragmentation):* While a valid hardware runtime issue (managed by PyTorch's caching allocator via `torch.cuda.empty_cache()`), memory fragmentation is an infrastructure/hardware execution detail, not a fundamental statistical failure mode of an ML model.
  
  ---
### P3. Jupyter/Colab Interactive Kernel Mechanics
  **Question:** You edited cell 3 after running cells 4–8. What best confirms the notebook still works top-to-bottom?
  * A. Runtime $\to$ Restart session and run all
  * B. Re-run only cell 3
  * C. Save the notebook to Drive
  * D. Switch to a GPU runtime
  
  **Correct Answer:** **A. Runtime $\to$ Restart session and run all**
#### Graduate-Level Analysis:
  * **The Mechanism:** Computational notebooks operate on a **client-server-kernel architecture** (Lecture 3 & 4). The background Python kernel maintains a single, persistent heap (`globals()`).
  * If you run cells out of order ($1 \to 2 \to 4 \to 8$) and subsequently modify cell $3$, downstream cells ($4$ through $8$) remain executed against the **stale, historical memory state**. 
  * The only way to prove absence of **hidden-state traps** and verify that execution is linear and reproducible is to terminate the Python interpreter process, flush `globals()`, and execute sequentially from cell $0$ to cell $K-1$.
  * **Why Distractors Are Wrong:**
  * *B (Re-run only cell 3):* Mutates cell 3's variable, but cells 4–8 remain un-updated with respect to the new value, leaving memory in an inconsistent state.
  * *C (Save to Drive):* Serializes the `.ipynb` JSON file to disk, but does not execute or validate code.
  * *D (Switch to GPU):* Resets the hardware environment, but does not execute the cells top-to-bottom to verify logical execution.
  
  ---
### P4. Pandas Indexing Semantics: Label vs. Positional
  **Question:** A DataFrame has the default integer index $0, 1, 2, \dots$. How many rows does `df.loc[0:3]` return?
  * A. 3
  * B. 4
  * C. 2
  * D. It raises an error
  
  **Correct Answer:** **B. 4**
#### Graduate-Level Analysis:
  * **The Mechanism:** Lecture 5 established the indexing duality in Pandas:
  * `.iloc[]` uses standard Python **integer positional indexing**, which operates on **half-open intervals** $[start, stop)$, where the stop boundary is **strictly exclusive**.
  * `.loc[]` uses **label-based indexing**, which operates on **closed intervals** $[start, stop]$, where the stop boundary is **strictly INCLUSIVE**.
  * When querying `df.loc[0:3]`, Pandas searches for label `0`, label `1`, label `2`, and label `3`.
  * It returns all **4 rows** (index labels 0, 1, 2, and 3).
  * **Exam Pro-Tip:** Remember: `.loc` is **L**abel-based and **L**oads the endpoint (inclusive). `.iloc` is **I**nteger-based and **I**gnores the endpoint (exclusive). `df.iloc[0:3]` would return 3 rows.
  
  ---
### P5. NumPy Array Broadcasting Rules
  **Question:** `a` has shape `(3, 1)` and `b` has shape `(1, 4)`. What is the shape of `a + b`?
  * A. (3, 1)
  * B. (1, 4)
  * C. Error — shapes are incompatible
  * D. (3, 4)
  
  **Correct Answer:** **D. (3, 4)**
#### Graduate-Level Analysis:
  * **The Mechanism:** Under NumPy’s official broadcasting semantics (Lecture 5), arrays are aligned starting from their **trailing (rightmost) dimensions**:
  $$\text{Array } a: \quad 3 \times \mathbf{1}$$
  $$\text{Array } b: \quad \mathbf{1} \times 4$$
  * Two dimensions are compatible if:
  1. They are equal, OR
  2. One of them is $1$.
  * Dimension 1 (columns): $a$ has $1$, $b$ has $4 \implies$ compatible, stretches to $4$.
  * Dimension 0 (rows): $a$ has $3$, $b$ has $1 \implies$ compatible, stretches to $3$.
  * The arrays stretch along their singleton dimensions to form a full outer-sum matrix of shape **`(3, 4)`**.
  * **Why Distractors Are Wrong:** C is wrong because NumPy does not require identical shapes; singleton dimensions ($1$) are broadcast automatically across the complementary axis.
  
  ---
### P6. Missingness Engineering: Indicator Augmentation
  **Question:** The main purpose of adding a missingness-indicator column (e.g., `age_was_missing`) is to:
  * A. Let the model use the fact that a value was missing, which may itself be predictive
  * B. Remove rows with missing values
  * C. Replace the need for imputation
  * D. Increase the number of training rows
  
  **Correct Answer:** **A. Let the model use the fact that a value was missing, which may itself be predictive**
#### Graduate-Level Analysis:
  * **The Mechanism:** Under Donald Rubin’s missing data framework (Lecture 7), data is frequently **Missing Not at Random (MNAR)** or **Missing at Random (MAR)**. 
  * The very occurrence of a missing entry often encodes strong domain signal (e.g., a patient too unstable to undergo a lab test, or a steerage passenger without a recorded cabin suite).
  * When we impute a missing entry with a median ($\tilde{x}$), we erase the information that the value was originally absent. Adding a binary indicator column:
  $$m_i = \mathbf{1}_{\{x_i \text{ was missing}\}} \in \{0, 1\}$$
  allows the downstream estimator to learn an explicit parameter weight ($w_m$) associated with the event of missingness itself.
  * **Why Distractors Are Wrong:**
  * *B:* Adding a column does not drop rows.
  * *C:* You still need to impute the numerical `NaN` in the original feature so the matrix contains valid floating-point numbers.
  * *D:* Adding a column expands feature width ($d$), not sample rows ($n$).
  
  ---
### P7. `MinMaxScaler` Out-of-Bounds Transformation
  **Question:** `MinMaxScaler` is fit on training values `[10, 20, 50]`. A test value of $60$ is transformed to:
  * A. 1.0
  * B. 0.833
  * C. 1.25
  * D. An error, because 60 is outside the training range
  
  **Correct Answer:** **C. 1.25**
#### Graduate-Level Analysis:
  * **The Mathematical Derivation:**
  The `MinMaxScaler` formula maps values via:
  $$x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$
  * During `.fit()`, the transformer learns its state parameters **strictly from the training data** (Lecture 8):
  $$x_{\min} = 10, \quad x_{\max} = 50 \implies x_{\max} - x_{\min} = 50 - 10 = 40$$
  * During `.transform()`, the test value $x_{\text{test}} = 60$ is evaluated against these **frozen parameters**:
  $$x'_{\text{test}} = \frac{60 - 10}{40} = \frac{50}{40} = \mathbf{1.250}$$
  * **Why Distractors Are Wrong:**
  * *A (1.0):* `MinMaxScaler` does **not clip** values by default unless explicitly parameterized with `clip=True`.
  * *D (Error):* Scikit-learn scalers never raise exceptions for out-of-bounds inputs; they evaluate the linear formula algebraically.
  
  ---
### P8. Categorical Representation: The Nominal Integer Fallacy
  **Question:** $\text{department} \in \{\text{Sales, HR, IT, Legal, Ops}\}$ is label-encoded as $0\text{–}4$ and fed to linear regression. The problem is:
  * A. Linear regression cannot use integer inputs
  * B. It imposes a fake order and fake equal spacing ($\text{Legal} = 3 \times \text{HR}$)
  * C. It creates too many columns
  * D. Nothing — this is the recommended encoding
  
  **Correct Answer:** **B. It imposes a fake order and fake equal spacing ($\text{Legal} = 3 \times \text{HR}$)**
#### Graduate-Level Analysis:
  * **The Mechanism:** `department` is a **nominal categorical feature** with no intrinsic physical or hierarchical order (Lecture 8).
  * Mapping nominal categories to arbitrary integers:
  $$\text{Sales: 0}, \quad \text{HR: 1}, \quad \text{IT: 2}, \quad \text{Legal: 3}, \quad \text{Ops: 4}$$
  forces a linear model ($y = w \cdot x_{\text{dept}} + b$) to evaluate false geometric relationships:
  1. It asserts that $\text{Legal} (3) > \text{HR} (1)$.
  2. It asserts that the difference between Sales and HR ($\Delta = 1$) is identical to the difference between IT and Legal ($\Delta = 1$).
  3. It forces $\text{Legal}$ to equal $3 \times \text{HR}$.
  * Nominal features must be transformed into mutually orthogonal binary vectors via **One-Hot Encoding** (`OneHotEncoder`).
  * **Why Distractors Are Wrong:** Linear regression handles integers natively; the issue is the false metric assumptions imposed on nominal categories.
  
  ---
### P9. Theoretical Foundations: The Etymology of "Normal"
  **Question:** In the normal equations $X^T X w = X^T y$, the word "normal" refers to the fact that:
  * A. The residual vector $y - Xw$ is perpendicular to every column of $X$
  * B. The errors are normally distributed
  * C. The features must be standardized first
  * D. It is the ordinary (normal) way to fit a model
  
  **Correct Answer:** **A. The residual vector $y - Xw$ is perpendicular to every column of $X$**
#### Graduate-Level Analysis:
  * **The Mechanism:** In vector calculus and linear algebra, the word **"normal" is a synonym for perpendicular / orthogonal** ($\perp$) (Lecture 10).
  * The prediction vector $\hat{y} = Xw$ is constrained to lie within the column space of the design matrix: $\text{Col}(X) \subset \mathbb{R}^n$.
  * The residual error vector $r = y - Xw$ represents the vector connecting the true target $y$ to its orthogonal projection $\hat{y}$.
  * To minimize Euclidean distance $\|y - Xw\|_2^2$, $r$ must be **normal (perpendicular)** to every spanning column vector of $X$:
  $$X^T (y - Xw) = \mathbf{0} \implies X^T r = \mathbf{0} \implies X^T X w = X^T y$$
  * **Why Distractors Are Wrong:** B is a common trap. While OLS is the MLE under Gaussian (normal) errors, the derivation of the Normal Equations is purely geometric and does not require distributional assumptions.
  
  ---
### P10. Gradient Descent Optimization Diagnostics
  **Question:** After 1,000 iterations of gradient descent the loss is still decreasing, but extremely slowly and almost linearly. Most likely:
  * A. The learning rate is too large
  * B. The learning rate is too small
  * C. The model has converged
  * D. The data contain leakage
  
  **Correct Answer:** **B. The learning rate is too small**
#### Graduate-Level Analysis:
  * **The Mechanism:** The parameter update step is $\Delta w = -\eta \nabla J(w)$.
  * If the learning rate $\eta$ is set to an excessively small value ($\eta \approx 0$), the optimizer takes microscopic steps down the loss bowl (Lecture 11).
  * Instead of exhibiting a healthy, rapid exponential decay toward the minimum, the loss curve flattens into an agonizingly slow, quasi-linear crawl, failing to reach the minimum within standard iteration budgets (`max_iter=1000`).
  * **Why Distractors Are Wrong:**
  * *A (Too large):* A learning rate that exceeds the stability threshold ($\eta > \frac{2}{\lambda_{\max}}$) produces **wild oscillations, numerical instability, or divergence (`NaN`)**.
  * *C (Converged):* If converged, the loss curve would be completely flat and horizontal because gradients would be near zero ($\nabla J \approx \mathbf{0}$).
  
  ---
### P11. Regularization Scale Sensitivity: Ridge and Lasso
  **Question:** Why must features be standardized before fitting Ridge or Lasso?
  * A. It makes the loss convex
  * B. Regularized models cannot handle negative numbers
  * C. The penalty depends on coefficient size, which depends on each feature's units
  * D. It removes outliers
  
  **Correct Answer:** **C. The penalty depends on coefficient size, which depends on each feature's units**
#### Graduate-Level Analysis:
  * **The Mechanism:** Regularizers penalize parameter weights uniformly:
  $$\Omega_{\text{Ridge}} = \alpha \sum_{j=1}^d w_j^2 \quad \text{and} \quad \Omega_{\text{Lasso}} = \alpha \sum_{j=1}^d |w_j|$$
  * In linear models, the parameter magnitude scales inversely with feature magnitude: $w_j \propto \frac{1}{\text{Scale}(X_j)}$ (Lectures 8 & 11).
  * If a feature is measured in tiny units (e.g., millimeters, where values are large), its learned weight $w_j$ is tiny, meaning it pays almost zero penalty.
  * If a feature is measured in large units (e.g., kilometers, where values are small), its learned weight $w_j$ must be large, causing the regularizer to penalize and shrink it aggressively regardless of its predictive power.
  * **Standardization ($\mu=0, \sigma=1$)** places all features on an identical variance scale, ensuring penalties are applied equitably.
  * **Why Distractors Are Wrong:** OLS, Ridge, and Lasso are convex regardless of scaling. Regularization handles negative numbers natively.
  
  ---
### P12. Asymmetric Classification Costs & Evaluation Metrics
  **Question:** An email filter sends a legitimate job offer to the spam folder. The metric that most directly penalizes this kind of error is:
  * A. Recall
  * B. Precision
  * C. Specificity
  * D. Accuracy
  
  **Correct Answer:** **B. Precision**
#### Graduate-Level Analysis:
  * **The Classification Mapping:**
  * Positive Class ($Y = 1$): **Spam**
  * Negative Class ($Y = 0$): **Ham (Legitimate Email)**
  * An email filter sending a legitimate job offer to the spam folder has predicted $\hat{Y} = 1$ when the true label was $Y = 0$. This is a **False Positive (FP / Type I Error)**.
  * **The Metric Formulation:**
  $$\text{Precision} = \frac{TP}{TP + \mathbf{FP}}$$
  $$\text{Recall} = \frac{TP}{TP + FN}$$
  * As the number of False Positives increases, **Precision directly degrades** in the denominator. A spam filter designed to protect important communications must optimize for high precision.
  * **Why Distractors Are Wrong:**
  * *A (Recall):* Penalizes **False Negatives (FN)** (e.g., letting a spam email slip into your inbox).
  * *C (Specificity):* While $\text{Specificity} = \frac{TN}{TN + FP}$ also includes FP, Precision is the canonical positive predictive metric used to evaluate filter purity. Between Precision and Recall, Precision is the target metric for controlling false alarms.
  
  ---
## Summary Review Checkpoints for Section 1
  
  To confirm complete readiness on conceptual fundamentals before moving to Section 2:
  1. *Under what condition does `MinMaxScaler` output a value strictly greater than $1.0$?*
  2. *Why does the linear equation $X^T X w = X^T y$ guarantee that Ordinary Least Squares residuals sum to zero ($\sum r_i = 0$)?*
  3. *Why does standardizing features prevent $L_1$ Lasso regression from arbitrarily deleting features measured in small physical units?*
  
  ---