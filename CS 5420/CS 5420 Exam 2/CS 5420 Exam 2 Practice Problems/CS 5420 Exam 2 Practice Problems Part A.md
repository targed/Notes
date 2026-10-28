#### P1. (Mirror of P1 — Imbalanced Accuracy Paradox)
  An automated fraud screening system evaluates $5{,}000$ credit card transactions, exactly $25$ of which are fraudulent. A naive rule-based system flags zero transactions as suspicious, predicting every transaction to be "legitimate". The accuracy of this system is:
  * **A.** $0.50\%$
  * **B.** $50.00\%$
  * **C.** $99.50\%$
  * **D.** $100.00\%$
  
  ---
#### P2. (Mirror of P2 — Metric Selection Under Operational Constraints)
  A cybersecurity triage team receives thousands of intrusion alerts daily but has the engineering capacity to manually investigate only 20 alerts per shift. Network intrusions are rare ($<0.1\%$), missed intrusions lead to severe data breaches, and investigating false alarms wastes analyst hours. Which single evaluation metric is most appropriate for ranking and tuning this system?
  * **A.** Accuracy
  * **B.** Specificity
  * **C.** $F_1$-score
  * **D.** Training Loss
  
  ---
#### P3. (Mirror of P3 — $k$-Fold Cross-Validation Row Bookkeeping)
  An engineer performs $5$-fold cross-validation on a training dataset containing $N = 2{,}500$ rows. During each fold iteration, exactly how many rows are used to train the surrogate model, and how many rows are used to validate it?
  * **A.** Trains on $500$ rows, validates on $2{,}000$ rows
  * **B.** Trains on $2{,}000$ rows, validates on $500$ rows
  * **C.** Trains on $2{,}500$ rows, validates on $500$ rows
  * **D.** Trains on $1{,}250$ rows, validates on $1{,}250$ rows
  
  ---
#### P4. (Mirror of P4 — LOOCV Computational Complexity)
  An analyst evaluates a dataset with $n = 450$ clinical trial records using Leave-One-Out Cross-Validation (LOOCV). How many total individual model fits are executed across the entire validation run?
  * **A.** 1
  * **B.** 5
  * **C.** 449
  * **D.** 450
  
  ---
#### P5. (Mirror of P5 — Pipeline Leakage Prevention)
  When setting up a cross-validation experiment with `cross_val_score`, which of the following operations can safely be performed on the **entire dataset before splitting** without causing data leakage?
  * **A.** Feature imputation with `SimpleImputer(strategy="median")`
  * **B.** Feature scaling with `StandardScaler()`
  * **C.** Dropping an arbitrary non-predictive metadata column (e.g., `transaction_uuid`)
  * **D.** Supervised feature selection with `SelectKBest(f_classif, k=10)`
  
  ---
#### P6. (Mirror of P6 — Diagnosing Bias vs. Variance)
  An engineer fits three polynomial regression models to a noisy sinusoidal physical process. 
  * **Model 1 (Degree 1):** Train MSE = $0.48$, Test MSE = $0.51$
  * **Model 2 (Degree 4):** Train MSE = $0.02$, Test MSE = $0.03$
  * **Model 3 (Degree 15):** Train MSE = $0.004$, Test MSE = $2.14$
  
  The **Degree 1 model** is suffering from:
  * **A.** High variance (overfitting)
  * **B.** High bias (underfitting)
  * **C.** Data leakage
  * **D.** Near-zero irreducible error $\text{Var}(\epsilon)$
  
  ---
#### P7. (Mirror of P7 — Parameters vs. Hyperparameters in SVMs)
  Which of the following is a **learned parameter** (computed from data by `.fit()`), rather than a hyperparameter, in an RBF-kernel Support Vector Classifier (`SVC(kernel="rbf")`)?
  * **A.** The inverse regularization parameter $C$
  * **B.** The kernel bandwidth parameter $\gamma$ (gamma)
  * **C.** The dual Lagrange multiplier coefficients $\alpha_i$
  * **D.** The degree $d$
  
  ---
#### P8. (Mirror of P8 — Mercer's Condition for Valid Kernels)
  Under Mercer's Theorem, a function $K(x, z)$ is a valid kernel if and only if its Gram matrix is symmetric positive semi-definite, which implies $K(x, x) \ge 0$ for all $x$. Which of the following is **NOT** a valid Mercer kernel?
  * **A.** $K(x, z) = 5 \cdot (x^T z)$
  * **B.** $K(x, z) = (2 x^T z + 1)^2$
  * **C.** $K(x, z) = \|x - z\|_2^2$
  * **D.** $K(x, z) = \exp(-3 \|x - z\|_2^2)$
  
  ---
#### P9. (Mirror of P9 — Scale Sensitivity in Distance & Variance Methods)
  A customer segmentation dataset contains two features: `customer_age` (measured in years, range $18\text{--}75$) and `annual_income` (measured in dollars, range $\$15{,}000\text{--}\$250{,}000$). If $k$-means clustering or PCA is applied to the raw, unstandardized features:
  * **A.** `customer_age` will dominate the distance metric because age values are smaller.
  * **B.** The algorithm will project and cluster the data almost entirely based on `annual_income`.
  * **C.** Nothing happens; both $k$-means and PCA are mathematically invariant to linear feature scaling.
  * **D.** The Gram matrix will become singular and cause the solver to crash.
  
  ---
#### P10. (Synthesis Mirror — The SVM Regularization Dial)
  In a soft-margin Support Vector Machine, decreasing the regularization parameter $C$ from $C = 100$ to $C = 0.001$:
  * **A.** Narrows the margin and decreases the number of support vectors.
  * **B.** Widens the margin and increases the number of support vectors.
  * **C.** Forces the model to behave as a hard-margin SVM.
  * **D.** Turns all slack variables $\xi_i$ strictly to zero.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for Part A
### P1. Correct Answer: **C. 99.50%**
  * **Derivation:**
  * Total samples $N = 5{,}000$.
  * Legitimate emails: $5{,}000 - 25 = 4{,}975$.
  * Phishing emails: $25$.
  * Predicting "legitimate" for every email correctly classifies all $4{,}975$ non-phishing emails:
    $$\text{Accuracy} = \frac{TN}{N} = \frac{4{,}975}{5{,}000} = \mathbf{0.995 \quad (99.50\%)}$$
  * **Lecture Context:** `[L15]`. This is the classic **Accuracy Paradox**: on imbalanced datasets, a completely uninformative model (the majority zero-rule baseline) achieves deceptively high accuracy while having zero clinical/operational utility (catching $0\%$ of phishing attacks).
  
  ---
### P2. Correct Answer: **C. $F_1$**
  * **Derivation:**
  * Accuracy collapses under class imbalance ($<0.1\%$).
  * Specificity only measures the proportion of legitimate traffic correctly left alone, ignoring how many attacks were missed.
  * Because the triage queue has fixed review capacity (false alarms cost money $\implies$ needs **Precision**) and undetected breaches cause major damage (missed attacks cost money $\implies$ needs **Recall**), the system requires a metric that balances both without being inflated by the massive True Negative count.
  * **$F_1$-score** is the harmonic mean of Precision and Recall, directly balancing alert purity against coverage:
    $$F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}$$
  * **Lecture Context:** `[L15]`. When positives are rare and both Type I (FP) and Type II (FN) errors carry severe costs, $F_1$ (or $F_\beta$) is the standard metric.
  
  ---
### P3. Correct Answer: **B. Trains on 2,000 rows, validates on 500 rows**
  * **Derivation:**
  * In $k$-fold cross-validation with $k = 5$:
    $$\text{Validation Fold Size} = \frac{N}{k} = \frac{2{,}500}{5} = \mathbf{500 \text{ rows}}$$
    $$\text{Training Set Size per Fit} = N - \frac{N}{k} = \left(\frac{k - 1}{k}\right)N = \left(\frac{4}{5}\right) \times 2{,}500 = \mathbf{2{,}000 \text{ rows}}$$
  * **Lecture Context:** `[L18]`. Each surrogate fit trains on $k-1$ folds and evaluates on the remaining single held-out fold.
  
  ---
### P4. Correct Answer: **D. 450**
  * **Derivation:**
  * Leave-One-Out Cross-Validation (LOOCV) sets the number of folds equal to the number of training instances: $k = n$.
  * Each observation serves as a single validation point exactly once while the model trains on the remaining $n - 1$ points.
  * Thus, $n = 450 \implies \mathbf{450 \text{ individual fits}}$.
  * **Lecture Context:** `[L18]`. LOOCV eliminates pessimistic sample-size bias, but requires $n$ separate training fits and yields binary ($0$ or $1$) validation scores on each fold.
  
  ---
### P5. Correct Answer: **C. Dropping an arbitrary non-predictive metadata column**
  * **Derivation:**
  * Dropping an identifier (like `transaction_uuid` or `patient_id`) relies strictly on structural metadata and domain knowledge. It does not calculate statistics, distributions, or parameters from the data values.
  * In contrast:
    * `SimpleImputer` learns the median from the data.
    * `StandardScaler` learns the mean $\mu$ and standard deviation $\sigma$.
    * `SelectKBest` evaluates correlations between features and labels $y$.
  * If (A), (B), or (D) are applied before splitting, test/validation fold statistics leak into the training features (Preprocessing/Selection Leakage). They **must** be placed inside the `Pipeline`.
  * **Lecture Context:** `[L09, L18]`. Everything that learns from data coordinates must be encapsulated inside the `Pipeline` so it re-fits strictly on each training fold.
  
  ---
### P6. Correct Answer: **B. High bias (underfitting)**
  * **Derivation:**
  * **Model 1 (Degree 1):** Both Train MSE ($0.48$) and Test MSE ($0.51$) are high and close together. The model class is too simple to capture the underlying sinusoidal wave. This is the definition of **High Bias (Underfitting)**.
  * *Contrast:* Model 3 (Degree 15) has near-zero Train MSE ($0.004$) but massive Test MSE ($2.14$), which represents **High Variance (Overfitting)**.
  * **Lecture Context:** `[L19]`. High bias $\implies J_{\text{train}}$ is high and $J_{\text{cv}} \approx J_{\text{train}}$. High variance $\implies J_{\text{train}}$ is low and $J_{\text{cv}} \gg J_{\text{train}}$.
  
  ---
### P7. Correct Answer: **C. The dual Lagrange multiplier coefficients $\alpha_i$**
  * **Derivation:**
  * Hyperparameters ($C$, $\gamma$, polynomial degree $d$) are configured by the practitioner **before** fitting begins.
  * Parameters ($w, b$, and the dual variables $\alpha_i$) are **learned automatically from data** during `.fit()` by solving the dual quadratic program:
    $$w^* = \sum_{i=1}^n \alpha_i^* y_i x_i$$
  * **Lecture Context:** `[L20, L21, L22]`. In `scikit-learn`, learned parameters are stored with a trailing underscore (e.g., `model.dual_coef_` stores $\alpha_i y_i$, `model.intercept_` stores $b$).
  
  ---
### P8. Correct Answer: **C. $K(x, z) = \|x - z\|_2^2$**
  * **Derivation:**
  * A valid Mercer kernel must correspond to an inner product in some Hilbert space: $K(x, z) = \langle \phi(x), \phi(z) \rangle$.
  * This imposes the necessary condition:
    $$K(x, x) = \|\phi(x)\|_2^2 \ge 0$$
  * For $K(x, z) = \|x - z\|_2^2$:
    * $K(x, x) = \|x - x\|_2^2 = 0$ for all $x$.
    * If we construct the Gram matrix for two distinct points $x = 0$ and $z = 1$:
      $$K = \begin{bmatrix} K(0,0) & K(0,1) \\ K(1,0) & K(1,1) \end{bmatrix} = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$$
      The eigenvalues of this matrix are $\lambda = \pm 1$. Because it has a negative eigenvalue ($\lambda = -1$), the matrix is **not Positive Semi-Definite (PSD)**.
    * Euclidean distance squared is a metric, **not an inner product**.
  * **Lecture Context:** `[L22]`. (A) is a positive scaling of a linear kernel, (B) is an inhomogeneous polynomial kernel, and (D) is an exponential kernel—all of which are valid under Mercer's closure rules.
  
  ---
### P9. Correct Answer: **B. The algorithm will project and cluster the data almost entirely based on `annual_income`.**
  * **Derivation:**
  * Euclidean distance sums squared feature differences:
    $$d(x, z)^2 = (x_{\text{age}} - z_{\text{age}})^2 + (x_{\text{income}} - z_{\text{income}})^2$$
  * A substantial age difference of $30$ years contributes $(30)^2 = 900$ to the squared distance sum.
  * A modest income difference of $\$10{,}000$ contributes $(10{,}000)^2 = 100{,}000{,}000$ to the sum.
  * Income differences contribute over $99.99\%$ of the distance metric, rendering age invisible to $k$-means and causing PCA to align the first principal component directly along the income axis.
  * **Lecture Context:** `[L08, L24]`. Features with large numerical ranges completely dominate distance- and variance-based algorithms; standardization (`StandardScaler`) is mandatory.
  
  ---
### P10. Correct Answer: **B. Widens the margin and increases the number of support vectors.**
  * **Derivation:**
  * In soft-margin SVMs, the objective balances margin width against margin violations:
    $$\min_{w, b, \xi} \frac{1}{2}\|w\|_2^2 + C \sum_{i=1}^n \xi_i$$
  * When $C$ is decreased ($C \to 0$):
    * The penalty on slack violations ($\xi_i$) becomes weak.
    * The optimizer prioritizes minimizing $\frac{1}{2}\|w\|_2^2$, which maximizes the margin width $\gamma = \frac{2}{\|w\|_2}$.
    * A wide margin street encompasses a larger physical volume of space, causing more training points to fall inside the margin ($y_i z_i \le 1 \implies \xi_i \ge 0$).
    * By the KKT conditions, every point on or inside the margin has an active multiplier $\alpha_i^* > 0$, **increasing the total number of support vectors**.
  * **Lecture Context:** `[L21]`. Small $C \implies$ wide margin, many support vectors, higher bias (underfit). Large $C \implies$ narrow margin, fewer support vectors, higher variance (overfit).