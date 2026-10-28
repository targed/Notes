#### T1. (Mirror of T1 — Metric Formulations & Ignored Cells)
  **[ $\quad$ ]** Precision directly incorporates the number of True Negatives ($TN$) in its denominator, which prevents it from being distorted when negative samples vastly outnumber positive samples.  
  *Justification:* _________________________________________________________________
  
  ---
#### T2. (Mirror of T2 — Cross-Validation Defaults in Scikit-Learn)
  **[ $\quad$ ]** If `cross_val_score(regressor, X, y, cv=5)` is called on a continuous regression task, scikit-learn uses `StratifiedKFold` by default.  
  *Justification:* _________________________________________________________________
  
  ---
#### T3. (Mirror of T3 — Test Set Integrity & Data Snooping)
  **[ $\quad$ ]** If you select the best SVM kernel and bandwidth parameter $\gamma$ by comparing performance on the test set, the resulting test score remains an unbiased estimate of generalization error as long as you do not use the test set to fit the support vector weights $\alpha_i$.  
  *Justification:* _________________________________________________________________
  
  ---
#### T4. (Mirror of T4 — Hard-Margin SVM Feasibility)
  **[ $\quad$ ]** If a training dataset contains even a single mislabeled observation that crosses over into the opposing class region, a hard-margin SVM Quadratic Program will fail to find any valid mathematical solution.  
  *Justification:* _________________________________________________________________
  
  ---
#### T5. (Mirror of T5 — Soft-Margin Regularization Dynamics)
  **[ $\quad$ ]** In a soft-margin SVM, setting the regularization parameter $C \to \infty$ allows the model to tolerate infinite margin violations, maximizing the physical width of the margin street.  
  *Justification:* _________________________________________________________________
  
  ---
#### T6. (Mirror of T6 — SVM Inference Complexity: Linear vs. Kernel)
  **[ $\quad$ ]** For a linear SVM (`LinearSVC`), inference time per test sample depends only on the feature dimension $d$, whereas for an RBF-kernel SVM (`SVC(kernel="rbf")`), inference time scales directly with the number of support vectors.  
  *Justification:* _________________________________________________________________
  
  ---
#### T7. (Mirror of T7 — $k$-Means Initialization & Local Minima)
  **[ $\quad$ ]** Running $k$-means with `n_init=10` runs Lloyd's algorithm 10 separate times from different random centroid seeds and selects the clustering partition that minimizes the Within-Cluster Sum of Squares (Inertia).  
  *Justification:* _________________________________________________________________
  
  ---
#### T8. (Mirror of T8 — Unsupervised Data Leakage in PCA)
  **[ $\quad$ ]** Because Principal Component Analysis (PCA) is an unsupervised method that does not access target labels $y$, fitting PCA on the entire dataset before performing a train/test split does not constitute data leakage.  
  *Justification:* _________________________________________________________________
  
  ---
#### T9. (Mirror of T9 — Orthogonality of Principal Components)
  **[ $\quad$ ]** In Principal Component Analysis, the sample covariance between the first principal component scores ($\text{PC}_1$) and the second principal component scores ($\text{PC}_2$) is identically zero.  
  *Justification:* _________________________________________________________________
  
  ---
#### T10. (Mirror of Clustering / Linkage Properties)
  **[ $\quad$ ]** In Agglomerative Hierarchical Clustering, Single Linkage evaluates the maximum pairwise distance between clusters, making it less prone to the chaining effect than Complete Linkage.  
  *Justification:* _________________________________________________________________
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for Part B
### T1. **FALSE**
  * **Justification:** Precision is defined as $\frac{TP}{TP + FP}$; it ignores True Negatives ($TN$) completely.
  * **Deep Dive:** Precision evaluates only the positive prediction column (alert purity). Because $TN$ is omitted from the formula, Precision is sensitive to false alarms ($FP$) even when the negative class is massive. The metric that directly uses $TN$ in its denominator to measure inlier protection is **Specificity** ($\frac{TN}{TN + FP}$). `[L15]`
  
  ---
### T2. **FALSE**
  * **Justification:** `StratifiedKFold` requires discrete class labels to preserve categorical proportions; for continuous regression targets, scikit-learn defaults to standard unstratified `KFold`.
  * **Deep Dive:** Stratification relies on grouping by discrete targets ($\frac{N_{i, c}}{N_i} \approx \frac{N_c}{N}$). In regression, $y \in \mathbb{R}$ is continuous, so discrete class frequencies do not exist. Therefore, `cross_val_score` defaults to standard `KFold` (or `KFold(shuffle=False)`) when passed a regressor. `[L18]`
  
  ---
### T3. **FALSE**
  * **Justification:** Choosing hyperparameters ($\gamma$ and kernel) based on test-set performance is **Meta-Overfitting (Data Snooping)**, which contaminates the test set and produces an optimistic, biased score.
  * **Deep Dive:** Even if the model's solver does not update $\alpha_i$ on test rows, the human engineer (or search wrapper) uses test-set feedback to make architectural decisions. The test set is no longer an independent out-of-sample proxy. Hyperparameters must be tuned using validation splits or cross-validation strictly within the training set (`GridSearchCV`), and the test set must be evaluated **only once** at the very end. `[L18, L20]`
  
  ---
### T4. **TRUE**
  * **Justification:** Hard-margin constraints require strict linear separability ($y_i(w^T x_i + b) \ge 1$ for all $i$); a single mislabeled or overlapping point makes the constraint set mutually exclusive, leaving the Quadratic Program with an empty feasible region (no solution).
  * **Deep Dive:** A hard-margin SVM allows zero slack ($\xi_i = 0$). If even one point crosses into the opposing class territory, the linear inequality constraints cannot be satisfied simultaneously by any hyperplane. This fatal brittleness led to the development of the **Soft-Margin SVM**, which adds slack variables $\xi_i \ge 0$ to absorb violations. `[L21]`
  
  ---
### T5. **FALSE**
  * **Justification:** Setting $C \to \infty$ heavily penalizes margin violations ($C \sum \xi_i$), forcing the margin to narrow and driving all slack variables $\xi_i \to 0$ (approximating a hard-margin SVM).
  * **Deep Dive:** The soft-margin objective is:
  $$\min_{w, b, \xi} \frac{1}{2}\|w\|_2^2 + C \sum_{i=1}^n \xi_i$$
  * When **$C \to \infty$**, any non-zero slack $\xi_i > 0$ incurs a massive penalty. The optimizer prioritizes eliminating violations over maximizing margin width, resulting in a **narrow margin with zero violations allowed**.
  * When **$C \to 0$ (small $C$)**, the penalty on slack is light, allowing the optimizer to minimize $\frac{1}{2}\|w\|_2^2$, which **widens the margin street ($\frac{2}{\|w\|}$) and increases the number of support vectors**. `[L21]`
  
  ---
### T6. **TRUE**
  * **Justification:** A linear SVM collapses its weights into a single $d$-dimensional vector $w = \sum \alpha_i y_i x_i$, making inference a single $\mathcal{O}(d)$ dot product, whereas an RBF-kernel SVM cannot collapse $w$ and must compute kernel distances against every active support vector ($\mathcal{O}(N_{\text{SVs}} \cdot d)$).
  * **Deep Dive:** 
  * In a linear SVM: $f(x) = w^T x + b$. Once trained, $w \in \mathbb{R}^d$ is precomputed; predicting a new sample takes $\mathcal{O}(d)$ time, regardless of how many support vectors were found.
  * In an RBF-kernel SVM: $w$ lives in an infinite-dimensional Hilbert space ($\mathbb{R}^\infty$) and cannot be precomputed as a numerical vector. Predictions must evaluate the dual expansion:
    $$f(x) = \sum_{i \in \text{SVs}} \alpha_i y_i \exp(-\gamma \|x - x_i\|_2^2) + b$$
    This requires computing the Gaussian distance against all $N_{\text{SVs}}$ support vectors for every query point, directly coupling inference latency to the support vector count. `[L21, L22]`
  
  ---
### T7. **FALSE**
  * **Justification:** $k$-means minimizes a non-convex objective (Inertia $J$); Lloyd's algorithm is a greedy local descent that converges to different local minima depending on initial centroid seeds.
  * **Deep Dive:** The inertia landscape $J = \sum \sum \|x_i - \mu_j\|^2$ has multiple local optima. An unlucky random initialization can split a natural cluster or force two clusters into one (as shown in Lecture 24, where an unlucky start yielded $J = 530$ while a better start yielded $J = 137$). To combat this, scikit-learn runs multiple random restarts (`n_init=10`) and uses $k$-means++ seeding. `[L24]`
  
  ---
### T8. **FALSE**
  * **Justification:** PCA learns the empirical covariance matrix and principal direction vectors from data; fitting PCA on the entire dataset leaks the test set's variance and coordinate orientation into the training pipeline (Preprocessing Leakage).
  * **Deep Dive:** Even though PCA does not use labels $y$, computing $\Sigma = \frac{1}{N}X^T X$ on the pooled data means the principal axes $V_k$ are influenced by the spread and distribution of the test samples. The training features become contaminated with out-of-sample structural information. PCA must be fit strictly on `X_train` (`pca.fit(X_train)`) and applied to `X_test` via `.transform()`. `[L09, PCA]`
  
  ---
### T9. **TRUE**
  * **Justification:** Principal components are the eigenvectors of the symmetric sample covariance matrix $\Sigma = \frac{1}{n-1}X^T X$; eigenvectors of symmetric real matrices corresponding to distinct eigenvalues are strictly orthogonal ($v_1^T v_2 = 0$).
  * **Deep Dive:** Projecting centered data onto these orthonormal eigenvectors produces uncorrelated coordinates:
  $$\text{Cov}(\text{PC}_1, \text{PC}_2) = v_1^T \Sigma v_2 = v_1^T (\lambda_2 v_2) = \lambda_2 (v_1^T v_2) = \lambda_2 (0) = \mathbf{0}$$
  The sample covariance between any two distinct principal components is identically zero. `[L10, PCA]`
  
  ---
### T10. **FALSE**
  * **Justification:** It is the exact reverse: Single Linkage evaluates the **minimum** pairwise distance (closest pair), which causes the chaining effect; Complete Linkage evaluates the **maximum** distance (cluster diameter) and prevents chaining by enforcing compact groups.
  * **Deep Dive:** 
  * **Single Linkage:** $d_{\text{single}}(A, B) = \min_{x \in A, y \in B} d(x, y)$. If a sparse trail of noise points exists between two clusters, single linkage chains along those points, merging distinct clusters prematurely.
  * **Complete Linkage:** $d_{\text{complete}}(A, B) = \max_{x \in A, y \in B} d(x, y)$. It forces all points in the merged cluster to be close to one another, preventing elongated chains and producing compact, equal-diameter clusters. `[L25]`