# CS 5420 — Introduction to Machine Learning
## Exam 2: Full-Length Practice Examination
  
  * **Coverage:** Week 7–10 / Modules 7–9 (Lectures 15, 18, 19, 20, 21, 22, 24, 25 & PCA Foundations)
  * **Duration:** 50 Minutes
  * **Total Points:** 100 Points
  * **Format:** Closed-book, closed-notes, closed-device. Simple paper-and-pencil arithmetic.
  * **Instructions:** Answer all questions directly on the exam paper. For calculation problems, show all intermediate formulas and work. Round final decimal answers to **2 decimal places** (or 3 decimals where specified).
  
  ---
## Part A: Multiple Choice (8 Questions $\times$ 2 Points = 16 Points)
#### P1. Imbalanced Base Rates & The Accuracy Paradox
  An automated fraud detection system evaluates $10{,}000$ banking transactions, exactly $50$ of which are fraudulent. A naive baseline model predicts "legitimate" for every transaction. The accuracy of this baseline model is:
  * **A.** $0.50\%$
  * **B.** $50.00\%$
  * **C.** $99.50\%$
  * **D.** $100.00\%$
  
  ---
#### P2. Operational Metric Selection
  A hospital infection control team reviews suspected sepsis alerts. Sepsis is rare ($1.5\%$ prevalence), missed infections are fatal, and investigating false alarms exhausts nursing staff. Which single evaluation metric is most appropriate for tuning the classification model?
  * **A.** Accuracy
  * **B.** Training MSE
  * **C.** Specificity
  * **D.** $F_1$-score
  
  ---
#### P3. $k$-Fold Cross-Validation Allocation
  An engineer runs $8$-fold cross-validation on a training dataset containing $N = 1{,}600$ observations. During each individual fold rotation, how many samples does the surrogate model train on and validate on?
  * **A.** Trains on $200$ rows, validates on $1{,}400$ rows
  * **B.** Trains on $1{,}400$ rows, validates on $200$ rows
  * **C.** Trains on $1{,}600$ rows, validates on $200$ rows
  * **D.** Trains on $800$ rows, validates on $800$ rows
  
  ---
#### P4. Leave-One-Out Computational Complexity
  A researcher evaluates a clinical dataset containing $n = 350$ patients using Leave-One-Out Cross-Validation (LOOCV). Exactly how many total model fits are executed during the evaluation?
  * **A.** 1
  * **B.** 10
  * **C.** 349
  * **D.** 350
  
  ---
#### P5. Data Leakage Prevention in Pipelines
  When setting up cross-validation with `cross_val_score`, which of the following operations can safely be performed on the **entire dataset before splitting** without causing data leakage?
  * **A.** Fitting `SimpleImputer(strategy="median")`
  * **B.** Fitting `StandardScaler()`
  * **C.** Dropping an uninformative database index column (e.g., `patient_uuid`)
  * **D.** Feature selection with `SelectKBest(f_classif, k=20)`
  
  ---
#### P6. Bias-Variance Trade-Off in High-Degree Models
  An unregularized degree-15 polynomial regression model fit to 25 sample points achieves a training MSE of $0.002$ and a test MSE of $341.5$. This model is suffering from:
  * **A.** High variance (overfitting)
  * **B.** High bias (underfitting)
  * **C.** Irreducible aleatoric noise $\text{Var}(\epsilon)$
  * **D.** Data leakage
  
  ---
#### P7. Hyperparameters in Support Vector Machines
  Which of the following is a **hyperparameter** (set before `.fit()`) rather than a learned parameter in an RBF-kernel SVM (`SVC(kernel="rbf")`)?
  * **A.** The dual Lagrange multipliers $\alpha_i$
  * **B.** The coordinates of the support vectors
  * **C.** The kernel bandwidth parameter $\gamma$ (gamma)
  * **D.** The scalar bias / intercept $b$
  
  ---
#### P8. Mercer's Condition for Valid Kernels
  Under Mercer's Theorem, which of the following candidate functions $K(x, z)$ is **NOT** a valid Mercer kernel for $x, z \in \mathbb{R}^d$?
  * **A.** $K(x, z) = 4 \cdot (x^T z)$
  * **B.** $K(x, z) = -2 \cdot (x^T z)$
  * **C.** $K(x, z) = (x^T z + 3)^2$
  * **D.** $K(x, z) = \exp(-0.5 \|x - z\|_2^2)$
  
  ---
## Part B: True / False (8 Questions $\times$ 2 Points = 16 Points)
  
  *Write **T** (True) or **F** (False) next to each statement.*
  
  * **T1. [ $\quad$ ]** The $F_1$-score formula directly incorporates the count of True Negatives ($TN$) in its denominator.
  * **T2. [ $\quad$ ]** Calling `cross_val_score(clf, X, y, cv=5)` on a discrete classification problem uses stratified folds by default in scikit-learn.
  * **T3. [ $\quad$ ]** Evaluating multiple candidate hyperparameter settings directly against the test set and reporting the best test score yields an unbiased estimate of generalization error.
  * **T4. [ $\quad$ ]** A hard-margin Support Vector Machine is robust against mislabeled outliers near the decision boundary.
  * **T5. [ $\quad$ ]** In a soft-margin SVM, decreasing the regularization hyperparameter $C$ from $100$ to $0.01$ results in a wider margin street and a greater number of support vectors.
  * **T6. [ $\quad$ ]** For an RBF-kernel SVM, inference time per query sample scales directly with the number of active support vectors.
  * **T7. [ $\quad$ ]** Running $k$-means clustering from different random centroid seeds is mathematically guaranteed to always converge to the identical final cluster assignments.
  * **T8. [ $\quad$ ]** In Principal Component Analysis (PCA), the sample covariance between Principal Component 1 ($\text{PC}_1$) and Principal Component 2 ($\text{PC}_2$) is identically zero.
  
  ---
## Part C: Calculation and Application (4 Problems = 68 Points)
### Problem C1: Confusion Matrix From a Description (14 Points)
  A commercial credit card company evaluates $1{,}000$ mortgage applications, $100$ of which will default (positive class). A machine learning risk model flags $150$ applications as high-risk defaults; $75$ of those flagged truly default.
  
  * **(a) [4 pts]** Construct the complete $2 \times 2$ confusion matrix by determining the integer counts for **$TP$**, **$FP$**, **$FN$**, and **$TN$**.
  * **(b) [2 pts]** Compute the **Accuracy**.
  * **(c) [2 pts]** Compute the **Precision**.
  * **(d) [2 pts]** Compute the **Recall (Sensitivity)**.
  * **(e) [2 pts]** Compute the **Specificity (True Negative Rate)**.
  * **(f) [2 pts]** Compute the **$F_1$-score**.
  
  ---
### Problem C2: Cross-Validation Bookkeeping & Diagnostics (16 Points)
  
  * **(a) [4 pts]** A training set contains $n = 800$ rows.
  1. Under $4$-fold cross-validation, how many rows does each surrogate model train on and validate on?
  2. Exactly how many individual model fits are required to perform Leave-One-Out Cross-Validation (LOOCV)?
  
  * **(b) [6 pts]** For each empirical scenario below, name the problem (**Overfitting**, **Underfitting**, or **Working Well**) and state **one concrete fix**:
  1. A $1$-NN classifier achieves a training error of $0.00$ and a cross-validation error of $0.24$.  
     *Problem:* ___________________________ | *Fix:* ___________________________________________
  2. A diagnostic learning curve shows that at $n = 100$, the gap between training and validation error is $0.18$. At $n = 2{,}000$, the gap has narrowed to $0.08$, and the validation error curve is still actively sloping downward ($\frac{d S_{\text{val}}}{dn} > 0$).  
     *Problem:* ___________________________ | *Fix:* ___________________________________________
  
  * **(c) [6 pts]** Label each quantity as **P** (Parameter learned from data by `.fit()`) or **H** (Hyperparameter set before `.fit()`):
  1. [ $\quad$ ] $C$ in a Support Vector Machine
  2. [ $\quad$ ] The weight vector $w$ of a linear SVM
  3. [ $\quad$ ] The cluster centroids $\mu_k$ in $k$-means
  4. [ $\quad$ ] $k$ (number of clusters) in $k$-means
  5. [ $\quad$ ] `n_components` in Principal Component Analysis (PCA)
  6. [ $\quad$ ] The dual multipliers $\alpha_i$ of a kernel SVM
  
  ---
### Problem C3: Hard-Margin SVM & Kernel Calculations by Hand (18 Points)
#### Part 1: Linear SVM from Support Vectors (10 Points)
  The support vectors of a hard-margin linear SVM separating two classes in $\mathbb{R}^2$ are:
  $$\text{Point } A = (1, 1), \quad y_A = -1$$
  $$\text{Point } B = (1, 5), \quad y_B = -1$$
  $$\text{Point } C = (5, 3), \quad y_C = +1$$
  *(All other training points lie strictly outside the margin).*
  
  * **(a) [4 pts]** Using the support vector boundary condition $y_i(w^T x_i + b) = 1$, write out the system of linear equations and solve for $w = [w_1, w_2]^T$ and $b$.
  * **(b) [2 pts]** Compute the Euclidean norm $\|w\|_2$ and the margin width $\gamma = \frac{2}{\|w\|_2}$.
  * **(c) [3 pts]** Using the KKT conditions $w = \sum \alpha_i y_i x_i$ and $\sum \alpha_i y_i = 0$, solve for the dual multipliers $\alpha_A, \alpha_B,$ and $\alpha_C$.
  * **(d) [1 pt]** Classify the new query point $x_{\text{test}} = (4, 1)$.
#### Part 2: Kernel Evaluations by Hand (8 Points)
  Given 2D vectors $x = (1, 2)$ and $z = (2, 1)$, and explicit mapping $\phi(u) = (u_1^2, \; \sqrt{2}u_1 u_2, \; u_2^2)$:
  * **(e) [2 pts]** Compute the homogeneous quadratic kernel $(x^T z)^2$ using the algebraic shortcut.
  * **(f) [3 pts]** Construct $\phi(x)$ and $\phi(z)$, and compute their dot product in $\mathbb{R}^3$ to verify your answer to (e).
  * **(g) [3 pts]** Compute the Gaussian RBF kernel $K(x, z) = \exp(-\gamma \|x - z\|_2^2)$ for bandwidth $\gamma = 0.50$. *(Note: $e^{-1} \approx 0.368$)*.
  
  ---
### Problem C4: Unsupervised Clustering by Hand (20 Points)
#### Part 1: $k$-Means in 1-D (10 Points)
  A 1-dimensional dataset consists of five points:
  $$X = [2, \; 4, \; 10, \; 12, \; 14]$$
  We cluster the data using $k = 2$ with initial centroid seeds:
  $$\mu_1^{(0)} = 2.0, \quad \mu_2^{(0)} = 4.0$$
  
  * **(a) [4 pts]** **Iteration 1:** Assign each point to its nearest centroid. State cluster partitions $C_1$ and $C_2$. Recompute the new centroid means $\mu_1^{(1)}$ and $\mu_2^{(1)}$.
  * **(b) [4 pts]** **Iteration 2:** Reassign all points using the updated centroids. State the new partitions and recompute centroids $\mu_1^{(2)}$ and $\mu_2^{(2)}$. Show that point $4$ switches clusters.
  * **(c) [2 pts]** Check for convergence on Iteration 3. Calculate the final **Inertia / WCSS ($J$)**:
  $$J = \sum_{j=1}^2 \sum_{x_i \in C_j} (x_i - \mu_j)^2$$
#### Part 2: Agglomerative Hierarchical Clustering (10 Points)
  Four 1-D points are positioned on a line:
  $$A = 0.0, \quad B = 2.0, \quad C = 5.0, \quad D = 9.0$$
  
  * **(d) [4 pts]** Trace **Single Linkage** ($\min$ pairwise distance). State the merge order and the exact merge height for each of the $n - 1 = 3$ merge steps.
  * **(e) [4 pts]** Trace **Complete Linkage** ($\max$ pairwise distance). State the merge order and the exact merge height for each of the $n - 1 = 3$ merge steps.
  * **(f) [2 pts]** Cut each tree into **$2$ clusters** (by undoing the final merge). State the resulting cluster partitions for Single Linkage versus Complete Linkage.
  ---
  ---
# Exam 2 Solutions
  
  ---
## Part A: Multiple Choice Solutions (16 Points)
  
  * **P1: C ($99.50\%$)** [2 pts]  
  *Derivation:* Total $N = 10{,}000$, non-fraud $= 10{,}000 - 50 = 9{,}950$. Predicting "legitimate" for all cases correctly classifies all negatives:
  $$\text{Accuracy} = \frac{TN}{N} = \frac{9{,}950}{10{,}000} = \mathbf{0.9950 \quad (99.50\%)}$$ `[L15]`
  * **P2: D ($F_1$-score)** [2 pts]  
  *Derivation:* Under extreme class imbalance ($1.5\%$), accuracy is uninformative. Because review capacity is limited (false alarms waste time $\implies$ needs Precision) and missed cases are catastrophic (needs Recall), the $F_1$-score is the standard harmonic mean metric to balance both errors without inflation from large $TN$ counts. `[L15]`
  * **P3: B (Trains on 1,400 rows, validates on 200 rows)** [2 pts]  
  *Derivation:* Validation fold size $= \frac{N}{k} = \frac{1{,}600}{8} = 200$ rows. Training fold size $= N - \frac{N}{k} = 1{,}600 - 200 = \mathbf{1{,}400 \text{ rows}}$. `[L18]`
  * **P4: D (350)** [2 pts]  
  *Derivation:* In Leave-One-Out CV, $k = n$. Each of the $n = 350$ samples serves as a hold-out validation point once, requiring exactly $\mathbf{350 \text{ individual fits}}$. `[L18]`
  * **P5: C (Dropping an arbitrary non-predictive metadata column)** [2 pts]  
  *Derivation:* Dropping an identifier like `patient_uuid` relies strictly on structural metadata and does not calculate statistics from data values. Imputers, scalers, and feature selectors learn statistics from data coordinates and must be placed inside the `Pipeline` to prevent leakage. `[L09, L18]`
  * **P6: B (High bias / underfitting)** [2 pts]  
  *Derivation:* In Model 1, both Train MSE ($0.48$) and Test MSE ($0.51$) are high, and the generalization gap is small. The linear model is too simple to capture the sinusoidal wave. `[L19]`
  * **P7: C (The kernel bandwidth parameter $\gamma$)** [2 pts]  
  *Derivation:* $\gamma$ (as well as $C$) is an architectural hyperparameter chosen before fitting. The dual weights $\alpha_i$, the bias $b$, and the support vectors are learned from training data during `.fit()`. `[L20, L21, L22]`
  * **P8: B ($K(x, z) = -2(x^T z)$)** [2 pts]  
  *Derivation:* Evaluating $K(x, x)$ on non-zero inputs yields $-2\|x\|_2^2 < 0$. By Mercer's theorem, $K(x, x) = \|\phi(x)\|_2^2 \ge 0$; a negative self-norm is mathematically impossible in a Hilbert space. `[L22]`
  
  ---
## Part B: True / False Solutions (16 Points)
  
  * **T1: FALSE** [2 pts]  
  *Explanation:* $F_1 = \frac{2TP}{2TP + FP + FN}$. True Negatives ($TN$) do not appear in the formula, which allows $F_1$ to evaluate rare positive classes without inflation from large negative counts. `[L15]`
  * **T2: TRUE** [2 pts]  
  *Explanation:* In scikit-learn, passing a classifier to `cross_val_score` automatically defaults to `StratifiedKFold` to preserve class proportions across folds. `[L18]`
  * **T3: FALSE** [2 pts]  
  *Explanation:* Tuning on the test set is data snooping (meta-overfitting). The test set ceases to be an independent out-of-sample proxy, producing an overly optimistic performance estimate. `[L18, L20]`
  * **T4: FALSE** [2 pts]  
  *Explanation:* Hard-margin constraints require strict linear separability ($y_i(w^T x_i + b) \ge 1$ for all $i$). A single overlapping outlier makes the constraints mutually exclusive, causing the QP solver to fail. `[L21]`
  * **T5: TRUE** [2 pts]  
  *Explanation:* Decreasing $C$ reduces the penalty on margin violations, allowing the optimizer to prioritize a smaller $\|w\|_2$, which widens the margin street ($\frac{2}{\|w\|_2}$) and encompasses more support vectors. `[L21]`
  * **T6: TRUE** [2 pts]  
  *Explanation:* For an RBF SVM, $w$ resides in an infinite-dimensional space and cannot be precomputed as a single vector. Predictions must evaluate the kernel against every active support vector ($\mathcal{O}(N_{\text{SVs}} \cdot d)$). `[L22]`
  * **T7: FALSE** [2 pts]  
  *Explanation:* The $k$-means inertia objective is non-convex. Lloyd’s algorithm is a greedy descent procedure that converges to local minima depending on initial centroid seeds. `[L24]`
  * **T8: TRUE** [2 pts]  
  *Explanation:* In PCA, principal directions are orthogonal eigenvectors of the symmetric covariance matrix $\Sigma$. Projecting onto them diagonalizes the covariance matrix, making $\text{Cov}(\text{PC}_1, \text{PC}_2) = 0$. `[L10, PCA]`
  
  ---
## Part C: Calculation and Application Solutions (68 Points)
### Problem C1 Solution: Confusion Matrix (14 Points)
  * **(a) Count Derivations:** [4 pts, 1 pt each]
  * Total $N = 1{,}000$; Actual Defaults $P = 100$; Actual Non-Defaults $N_{\text{neg}} = 1{,}000 - 100 = 900$.
  * Predicted Defaults $\hat{P} = 150$; True Defaults $TP = \mathbf{75}$.
  * $FP = \hat{P} - TP = 150 - 75 = \mathbf{75}$
  * $FN = P - TP = 100 - 75 = \mathbf{25}$
  * $TN = N_{\text{neg}} - FP = 900 - 75 = \mathbf{825}$
  * *Check:* $75 + 75 + 25 + 825 = 1{,}000$.
  * **(b) Accuracy:** [2 pts]
  $$\text{Accuracy} = \frac{TP + TN}{N} = \frac{75 + 825}{1{,}000} = \frac{900}{1{,}000} = \mathbf{0.90 \quad (90.00\%)}$$
  * **(c) Precision:** [2 pts]
  $$\text{Precision} = \frac{TP}{TP + FP} = \frac{75}{75 + 75} = \frac{75}{150} = \mathbf{0.50 \quad (50.00\%)}$$
  * **(d) Recall (Sensitivity):** [2 pts]
  $$\text{Recall} = \frac{TP}{TP + FN} = \frac{75}{75 + 25} = \frac{75}{100} = \mathbf{0.75 \quad (75.00\%)}$$
  * **(e) Specificity:** [2 pts]
  $$\text{Specificity} = \frac{TN}{TN + FP} = \frac{825}{825 + 75} = \frac{825}{900} \approx \mathbf{0.92 \quad (91.67\%)}$$
  * **(f) $F_1$-Score:** [2 pts]
  $$F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} = 2 \cdot \frac{0.50 \times 0.75}{0.50 + 0.75} = \frac{0.75}{1.25} = \mathbf{0.60}$$
  
  ---
### Problem C2 Solution: Bookkeeping & Diagnostics (16 Points)
  
  * **(a) Fold & LOOCV Allocation ($n = 800, k = 4$):** [4 pts]
  1. $\text{Validation Rows per Fold} = \frac{800}{4} = \mathbf{200 \text{ rows}}$; $\text{Training Rows per Fold} = 800 - 200 = \mathbf{600 \text{ rows}}$. [2 pts]
  2. $\text{LOOCV Fits} = n = \mathbf{800 \text{ fits}}$. [2 pts]
  
  * **(b) Diagnostic Signatures & Fixes:** [6 pts]
  1. Train Error = $0.00$, CV Error = $0.24$:
     * *Problem:* **Overfitting (High Variance)**. [1.5 pts]
     * *Fix:* Increase $k$ (e.g., $k = 7$ or $15$), gather more data, or apply feature selection. [1.5 pts]
  2. Gap narrows from $0.18 \to 0.08$ and CV error is still sloping downward:
     * *Problem:* **Overfitting (High Variance)**. [1.5 pts]
     * *Fix:* Collect more training data ($n > 2{,}000$) or apply data augmentation; the downward validation slope proves additional data will continue closing the gap. [1.5 pts]
  
  * **(c) Parameter vs. Hyperparameter Classification:** [6 pts, 1 pt each]
  1. $C$ in an SVM $\implies$ **H**
  2. $w$ of a linear SVM $\implies$ **P**
  3. Centroids $\mu_k$ in $k$-means $\implies$ **P**
  4. $k$ in $k$-means $\implies$ **H**
  5. `n_components` in PCA $\implies$ **H**
  6. Dual multipliers $\alpha_i$ of an SVM $\implies$ **P**
  
  ---
### Problem C3 Solution: SVM & Kernel Calculations (18 Points)
#### Part 1: Linear SVM from Support Vectors [10 pts]
  * **(a) Solve for $w$ and $b$:** [4 pts]
  * Point $A(1, 1), y = -1 \implies w_1(1) + w_2(1) + b = -1 \implies \mathbf{w_1 + w_2 + b = -1}$ (Eq 1)
  * Point $B(1, 5), y = -1 \implies w_1(1) + w_2(5) + b = -1 \implies \mathbf{w_1 + 5w_2 + b = -1}$ (Eq 2)
  * Point $C(5, 3), y = +1 \implies w_1(5) + w_2(3) + b = +1 \implies \mathbf{5w_1 + 3w_2 + b = 1}$ (Eq 3)
  * Subtract Eq 1 from Eq 2:
    $$4w_2 = 0 \implies \mathbf{w_2 = 0.00}$$
  * Substitute $w_2 = 0$ into Eq 1 and Eq 3:
    $$\begin{cases} w_1 + b = -1 \\ 5w_1 + b = 1 \end{cases}$$
  * Subtract: $4w_1 = 2 \implies \mathbf{w_1 = 0.50}$.
  * $b = -1 - w_1 = -1 - 0.50 = \mathbf{-1.50}$.
  * **Answer:** $\mathbf{w = [0.50, \; 0.00]^T, \quad b = -1.50}$ (Decision boundary: $x_1 = 3.0$).
  * **(b) Margin Width:** [2 pts]
  $$\|w\|_2 = \sqrt{0.50^2 + 0.00^2} = 0.50 \implies \gamma = \frac{2}{\|w\|_2} = \frac{2}{0.50} = \mathbf{4.00}$$
  * **(c) Find Dual Multipliers $\alpha_A, \alpha_B, \alpha_C$:** [3 pts]
  * Balance Rule: $-\alpha_A - \alpha_B + \alpha_C = 0 \implies \mathbf{\alpha_C = \alpha_A + \alpha_B}$.
  * Weight Rule:
    $$w = -\alpha_A \begin{bmatrix} 1 \\ 1 \end{bmatrix} - \alpha_B \begin{bmatrix} 1 \\ 5 \end{bmatrix} + \alpha_C \begin{bmatrix} 5 \\ 3 \end{bmatrix} = \begin{bmatrix} 0.50 \\ 0.00 \end{bmatrix}$$
  * Coordinate 2: $-\alpha_A - 5\alpha_B + 3(\alpha_A + \alpha_B) = 2\alpha_A - 2\alpha_B = 0 \implies \mathbf{\alpha_A = \alpha_B}$.
  * Substitute into Coordinate 1 (with $\alpha_C = 2\alpha_A$):
    $$-\alpha_A - \alpha_A + 5(2\alpha_A) = 8\alpha_A = 0.50 \implies \mathbf{\alpha_A = \frac{1}{16} \approx 0.06}$$
    $$\mathbf{\alpha_B = \frac{1}{16} \approx 0.06}, \quad \mathbf{\alpha_C = \frac{1}{8} = 0.125 \approx 0.13}$$
  * **(d) Classify $x_{\text{test}} = (4, 1)$:** [1 pt]
  $$f(x) = 0.50(4) + 0.00(1) - 1.50 = 2.0 - 1.50 = \mathbf{+0.50 > 0 \implies \text{Class } +1}$$
#### Part 2: Kernel Calculations [8 pts]
  * **(e) Shortcut Evaluation:** [2 pts]
  $$x^T z = 1(2) + 2(1) = 4.00$$
  $$(x^T z)^2 = (4.00)^2 = \mathbf{16.00}$$
  * **(f) Explicit Mapping $\phi(u) = (u_1^2, \; \sqrt{2}u_1 u_2, \; u_2^2)$:** [3 pts]
  $$\phi(x) = [1^2, \; \sqrt{2}(1)(2), \; 2^2]^T = [1, \; 2\sqrt{2}, \; 4]^T$$
  $$\phi(z) = [2^2, \; \sqrt{2}(2)(1), \; 1^2]^T = [4, \; 2\sqrt{2}, \; 1]^T$$
  $$\phi(x)^T \phi(z) = 1(4) + (2\sqrt{2})(2\sqrt{2}) + 4(1) = 4 + 8 + 4 = \mathbf{16.00 \quad (\text{Matches!})}$$
  * **(g) Gaussian RBF Kernel ($\gamma = 0.50$):** [3 pts]
  $$\|x - z\|_2^2 = (1 - 2)^2 + (2 - 1)^2 = (-1)^2 + 1^2 = 1 + 1 = 2.00$$
  $$K(x, z) = \exp(-\gamma \|x - z\|^2) = \exp(-0.50 \times 2.00) = e^{-1} \approx \mathbf{0.37 \text{ (or } 0.368\text{)}}$$
  
  ---
### Problem C4 Solution: Unsupervised Clustering by Hand (20 Points)
#### Part 1: $k$-Means in 1-D [10 pts]
  Points: $2, \; 4, \; 10, \; 12, \; 14$. Initial: $\mu_1 = 2.0, \; \mu_2 = 4.0$.
  
  * **(a) Iteration 1:** [4 pts]
  * Midpoint: $\frac{2.0 + 4.0}{2} = 3.0$.
    * Point $2 < 3.0 \implies \mathbf{C_1}$
    * Points $4, 10, 12, 14 > 3.0 \implies \mathbf{C_2}$
  * Partitions: $\mathbf{C_1 = \{2\}, \quad C_2 = \{4, 10, 12, 14\}}$
  * Centroids:
    $$\mu_1^{(1)} = \mathbf{2.00}$$
    $$\mu_2^{(1)} = \frac{4 + 10 + 12 + 14}{4} = \frac{40}{4} = \mathbf{10.00}$$
  * **(b) Iteration 2:** [4 pts]
  * Midpoint: $\frac{2.0 + 10.0}{2} = 6.0$.
    * Point $2 < 6.0 \implies C_1$
    * Point $4 < 6.0 \implies \mathbf{C_1}$ **(Point 4 switches from $C_2$ to $C_1$!)**
    * Points $10, 12, 14 > 6.0 \implies C_2$
  * Partitions: $\mathbf{C_1 = \{2, 4\}, \quad C_2 = \{10, 12, 14\}}$
  * Centroids:
    $$\mu_1^{(2)} = \frac{2 + 4}{2} = \mathbf{3.00}$$
    $$\mu_2^{(2)} = \frac{10 + 12 + 14}{3} = \frac{36}{3} = \mathbf{12.00}$$
  * **(c) Iteration 3 & Final Inertia:** [2 pts]
  * Midpoint: $\frac{3.0 + 12.0}{2} = 7.5$.
  * Points $\{2, 4\} < 7.5 \implies C_1$; Points $\{10, 12, 14\} > 7.5 \implies C_2$.
  * Assignments do not change $\implies$ **CONVERGED!**
  * Inertia $J$:
    $$J_{C_1} = (2 - 3)^2 + (4 - 3)^2 = 1 + 1 = 2.00$$
    $$J_{C_2} = (10 - 12)^2 + (12 - 12)^2 + (14 - 12)^2 = 4 + 0 + 4 = 8.00$$
    $$\mathbf{J = 2.00 + 8.00 = 10.00}$$
  
  ---
#### Part 2: Agglomerative Hierarchical Clustering [10 pts]
  Points: $A = 0.0, \; B = 2.0, \; C = 5.0, \; D = 9.0$.
  
  Initial distances:
  $$d(A, B) = 2.0, \quad d(B, C) = 3.0, \quad d(C, D) = 4.0$$
  $$d(A, C) = 5.0, \quad d(B, D) = 7.0, \quad d(A, D) = 9.0$$
  
  * **(d) Single Linkage ($\min$ pairwise distance):** [4 pts]
  * **Merge 1:** Smallest overall is $d(A, B) = 2.0 \implies \mathbf{\{A, B\} \text{ at height } h = 2.00}$.
  * Distances from $\{A, B\}$: to $C = \min(5, 3) = 3.0$; to $D = \min(9, 7) = 7.0$. Singleton distance $d(C, D) = 4.0$.
  * **Merge 2:** Smallest is $d(\{A, B\}, C) = 3.0 \implies \mathbf{\{A, B, C\} \text{ at height } h = 3.00}$.
  * Distance from $\{A, B, C\}$ to $D = \min(9, 7, 4) = 4.0$.
  * **Merge 3:** $\mathbf{\{A, B, C, D\} \text{ at height } h = 4.00}$.
  * **(e) Complete Linkage ($\max$ pairwise distance):** [4 pts]
  * **Merge 1:** Smallest overall is $d(A, B) = 2.0 \implies \mathbf{\{A, B\} \text{ at height } h = 2.00}$.
  * Distances from $\{A, B\}$: to $C = \max(5, 3) = 5.0$; to $D = \max(9, 7) = 9.0$.
  * Singleton distance: $d(C, D) = 4.0$.
  * Smallest available distance is $d(C, D) = 4.0 < 5.0 \implies \mathbf{\{C, D\} \text{ at height } h = 4.00}$.
  * Distance between $\{A, B\}$ and $\{C, D\}$ is $\max(d) = d(A, D) = 9.0$.
  * **Merge 3:** $\mathbf{\{A, B, C, D\} \text{ at height } h = 9.00}$.
  * **(f) 2-Cluster Cut:** [2 pts]
  * **Single Linkage (Undo Merge 3 at $h = 4.00$):**
    $$\mathbf{\{A, B, C\} \quad \mid \quad \{D\}}$$
  * **Complete Linkage (Undo Merge 3 at $h = 9.00$):**
    $$\mathbf{\{A, B\} \quad \mid \quad \{C, D\}}$$