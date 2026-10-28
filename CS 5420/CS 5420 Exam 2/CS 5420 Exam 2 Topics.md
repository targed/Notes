## Part 1: Model Evaluation, Validation & Diagnostic Curves (Lectures 15, 18, 19, 20)
### Lecture 15: Classification Metrics & Evaluation Theory
  * **The Accuracy Paradox on Imbalanced Data:**
  * Why classification accuracy collapses when base rates are skewed ($\pi = P(Y=1) \ll 0.50$).
  * The trivial "zero-rule" baseline: predicting the majority class for all instances achieves $1 - \pi$ accuracy with $0\%$ utility.
  * **The $2 \times 2$ Confusion Matrix:**
  * **True Positives ($TP$):** Predicted positive and actually positive.
  * **False Positives ($FP$, Type I / False Alarm):** Predicted positive, actually negative.
  * **False Negatives ($FN$, Type II / Miss):** Predicted negative, actually positive.
  * **True Negatives ($TN$):** Predicted negative and actually negative.
  * Multiclass $K \times K$ confusion matrix: main diagonal represents correct classifications ($\text{Tr}(M)$); row sums are actual class totals; column sums are predicted class totals.
  * **Marginal Metrics (Row vs. Column Conditioning):**
  * **Precision (Positive Predictive Value / PPV):** $\frac{TP}{TP + FP} = P(Y=1 \mid \hat{Y}=1)$ (measures alert purity / investigative waste; penalizes false alarms).
  * **Recall / Sensitivity (True Positive Rate / TPR):** $\frac{TP}{TP + FN} = P(\hat{Y}=1 \mid Y=1)$ (measures coverage; penalizes misses in safety/clinical screening).
  * **Specificity (True Negative Rate / TNR):** $\frac{TN}{TN + FP} = P(\hat{Y}=0 \mid Y=0)$ (measures inlier non-interference).
  * **False Positive Rate (FPR):** $\text{FPR} = \frac{FP}{TN + FP} = 1 - \text{Specificity}$.
  * **The Base-Rate Fallacy in Screening:**
  * Bayes' formula connecting Precision, Sensitivity ($Se$), Specificity ($Sp$), and Prevalence ($\pi$):
    $$\text{Precision} = \frac{\pi \cdot Se}{\pi \cdot Se + (1 - \pi)(1 - Sp)}$$
  * Why high specificity ($98\%$) can still produce abysmal precision ($30\%$) when the target condition is rare ($1\%$).
  * **Composite Metrics & Harmonic Means:**
  * **$F_1$-Score:** Harmonic mean of precision and recall:
    $$F_1 = 2 \cdot \frac{P \cdot R}{P + R} = \frac{2TP}{2TP + FP + FN}$$
  * *Why harmonic mean?* It is dominated by the smaller argument; if either precision or recall collapses, $F_1 \to 0$. It ignores $TN$, preventing large negative classes from artificially inflating the score.
  * **$F_\beta$-Score:** $(1 + \beta^2)\frac{P \cdot R}{\beta^2 P + R}$ ($\beta > 1$ weights recall higher; $\beta < 1$ weights precision higher).
  * **Cost-Sensitive Decision Thresholds:**
  * Bayes optimal decision threshold: $\tau^* = \frac{C_{\text{FP}}}{C_{\text{FP}} + C_{\text{FN}}}$.
  * Threshold moving: lowering $\tau$ below $0.50$ when missed events are catastrophic ($C_{\text{FN}} \gg C_{\text{FP}}$, e.g., oncology, sepsis).
  
  ---
### Lecture 18: Train/Test Splits, Cross-Validation & Resampling
  * **The Single-Split Lottery:**
  * Why evaluating a single 80/20 train/test split suffers from high variance (the 7% swing on small datasets across different random seeds).
  * **The $k$-Fold Cross-Validation Algorithm:**
  * 3-step cycle: Split into $k$ disjoint folds $\to$ Rotate ($k-1$ folds train, 1 fold validate) $\to$ Average out-of-fold scores ($\text{CV} = \frac{1}{k}\sum_{i=1}^k s_i$).
  * The test set remains locked in cold storage; CV happens entirely inside the training data.
  * **Algorithmic Stability & Standard Deviation:**
  * Reporting mean $\pm$ standard deviation ($\hat{\mu}_{\text{CV}} \pm \hat{\sigma}_{\text{CV}}$) measures how sensitive the model is to training data perturbations ($0.98 \pm 0.01$ is robust; $0.98 \pm 0.10$ is unstable).
  * **The Bias-Variance Trade-Off in Choosing $k$:**
  * *Pessimistic Bias:* Surrogate models train on $\frac{k-1}{k}n$ rows. Small $k$ (e.g., $k=2$) trains on half the data, systematically underestimating model capability.
  * *Estimator Variance:* Large $k$ (e.g., LOOCV, $k=n$) trains on $n-1$ rows; extreme sample overlap ($\approx 99.8\%$) induces positive covariance $\text{Cov}(s_i, s_j)$ that inflates estimator variance.
  * *Leave-One-Out (LOOCV, $k=n$):* Needs $n$ separate fits; validation scores on single points are binary ($s_i \in \{0, 1\}$); computationally expensive.
  * Consensus default: $k = 5$ or $k = 10$.
  * **Stratified $k$-Fold Cross-Validation:**
  * Datasets arriving sorted by label cause unstratified contiguous splits to isolate entire classes into single folds.
  * If a validation fold contains zero positive instances ($P = 0$), Recall ($\frac{0}{0}$) becomes undefined.
  * `StratifiedKFold` forces each fold to maintain the population base rate: $\frac{N_{i, c}}{N_i} \approx \frac{N_c}{N}$.
  * **Cross-Validation Leakage & Pipeline Encapsulation:**
  * Fitting transformers (scalers, imputers, feature selectors) before cross-validation leaks validation statistics.
  * Wrapping transformers inside `sklearn.pipeline.Pipeline` ensures transformations are re-fitted strictly within the $k-1$ training folds of each rotation.
  * *Leakage disparity:* Scaling leaks a little (unsupervised); supervised feature selection (`SelectKBest`) leaks a lot (fabricates $90\%$ accuracy from pure white noise).
  * **Repeated Cross-Validation:**
  * `RepeatedStratifiedKFold(n_splits=5, n_repeats=10)` averages out Monte Carlo partition variance across $10$ distinct fold randomizations.
  
  ---
### Lecture 19: Bias, Variance & Diagnostic Curves
  * **Analytical Decomposition of Mean Squared Error:**
  $$\mathbb{E}[(y - \hat{f}(x))^2] = \underbrace{\Big(f(x) - \mathbb{E}[\hat{f}(x)]\Big)^2}_{\text{Bias}^2} + \underbrace{\mathbb{E}\left[\Big(\hat{f}(x) - \mathbb{E}[\hat{f}(x)]\Big)^2\right]}_{\text{Variance}} + \underbrace{\sigma^2}_{\text{Irreducible Noise}}$$
  * $\text{Bias}^2$: structural error from an oversimplified hypothesis space $\mathcal{H}$ (Underfitting).
  * $\text{Variance}$: model sensitivity to the specific training dataset draw (Overfitting).
  * $\sigma^2 = \text{Var}(\epsilon)$: aleatoric noise floor; no model can cross below it.
  * **Flexibility vs. Error Trajectory:**
  * Training error decreases monotonically as model complexity increases.
  * Test error forms an asymmetric U-shape; optimal capacity sits at the bottom of the U-curve.
  * **Capacity Parameterization in $k$-NN ($1/K$):**
  * $\text{Degrees of Freedom} \approx \frac{N}{K} \propto \frac{1}{K}$.
  * Plotted against $1/K$ so capacity increases from left ($1/N \to 0$, rigid, high bias) to right ($1/1 = 1$, interpolating, zero training error, high variance).
  * **Empirical Diagnostics from Training vs. CV Error:**
  * *High Bias (Underfit):* $J_{\text{train}}$ is high; $J_{\text{cv}} \approx J_{\text{train}}$ (tiny gap, high error floor).
  * *High Variance (Overfit):* $J_{\text{train}}$ is low; $J_{\text{cv}} \gg J_{\text{train}}$ (large generalization gap).
  * *Working Well:* Both are low and close, nearing $\sigma^2$.
  * **Andrew Ng’s Diagnostic Playbook:**
  * Fixes for High Variance: collect more training data, prune features, increase regularization $\lambda$ (smaller $C$), simplify model architecture.
  * Fixes for High Bias: add domain features, add polynomial/interaction terms, decrease regularization $\lambda$ (larger $C$), choose a more expressive model.
  * **The "More Data" Theorem:** More data only cures estimation variance ($\text{Var}(\hat{\theta}) \propto 1/N$). It cannot fix approximation bias ($\inf_{h \in \mathcal{H}} \|f - h\|^2$ is independent of $N$).
  * **Learning Curves:**
  * Plotting performance vs. training sample size $n$.
  * High bias signature: curves plateau early, gap is tiny, validation slope is flat ($\frac{dS_{\text{val}}}{dn} \approx 0$) $\implies$ more data is useless.
  * High variance signature: large gap remains, validation curve still slopes upward ($\frac{dS_{\text{val}}}{dn} > 0$) $\implies$ more data will improve the model.
  
  ---
### Lecture 20: Hyperparameter Tuning & Model Selection
  * **Parameters vs. Hyperparameters:**
  * Parameters ($\theta$): learned by `.fit()` (e.g., $w, b$, tree split thresholds, centroids). Scikit-learn convention: trailing underscore (`coef_`, `intercept_`).
  * Hyperparameters ($\lambda$): set before `.fit()` (e.g., $k$ in $k$-NN, $\alpha$ in Ridge/Lasso, $C$ in SVM).
  * Why training error cannot tune hyperparameters: it trivially picks maximum capacity ($k=1, \alpha=0, d=\infty$).
  * **The Tripartite Split & The Test Vault:**
  * Train 60% (fit parameters) $\to$ Validation 20% (tune hyperparameters) $\to$ Test 20% (evaluate once).
  * Test-set snooping: tuning hyperparameters on the test set converts it into an uncalibrated validation set (meta-overfitting).
  * **Exhaustive Grid Search (`GridSearchCV`):**
  * Combinatorial complexity: $N_{\text{fits}} = k_{\text{folds}} \times \prod |G_m|$.
  * Pipeline parameter syntax: `estimator__parameter` (double underscore).
  * Linear vs. Logarithmic grids: scale-dependent parameters ($\alpha, \lambda, C$) must be sampled across exponential orders of magnitude (`np.logspace(-4, 4, 9)`), not linear grids (`np.linspace`).
  * **Metric Alignment:**
  * The winning hyperparameter depends on the chosen metric.
  * On Breast Cancer, Precision selects $k = 23$ ($FP \downarrow$); Recall selects $k = 3$ ($FN \downarrow$). Always declare `scoring=` explicitly.
  * **The 5-Step Model Selection Workflow:**
  1. Split (vault test set) $\to$ 2. Tune (CV on train) $\to$ 3. Refit (winner on 100% of train) $\to$ 4. Evaluate (once on test) $\to$ 5. Deploy.
  * `GridSearchCV(refit=True)` automates steps 2 and 3.
  * **Analytical Penalization vs. Resampling:**
  * $\text{AIC} = n\ln(\text{MSE}) + 2k$.
  * $\text{BIC} = n\ln(\text{MSE}) + k\ln(n)$.
  * When $n \ge 8$, $\ln(n) > 2 \implies$ BIC penalizes complexity more heavily than AIC.
  * Forward stepwise selection on Diabetes: BIC and 10-fold CV both select the exact same parsimonious subset of 6 features; training MSE picks all 10.
  * **Honest Model Comparison:**
  * Paired difference on identical splits: $d_i = \text{Score}_{A, i} - \text{Score}_{B, i}$.
  * Breiman's One-Standard-Error Rule ($1\text{-SE}$ rule): if a simpler or more regularized model is within $1\text{-SE}$ of the empirical top score, choose the simpler model.
  
  ---
## Part 2: Support Vector Machines & Kernel Methods (Lectures 21, 22)
### Lecture 21: Maximum-Margin Classifiers & Linear SVM
  * **The Separator Ambiguity:** Infinitely many zero-training-error hyperplanes exist for linearly separable data; maximum margin provides a unique geometric inductive bias.
  * **Hyperplane Geometry:**
  * Boundary: $w^T x + b = 0$. Normal vector $w$ is perpendicular to the boundary, pointing toward Class $+1$.
  * Perpendicular Euclidean distance from point $x$ to hyperplane: $d_\perp = \frac{|w^T x + b|}{\|w\|_2}$.
  * **Canonical Hyperplane & Margin Derivation:**
  * Scale invariance: fix scale by setting the functional margin of the closest points to $1$: $\min_i y_i(w^T x_i + b) = 1$.
  * Margin boundaries: $w^T x + b = +1$ and $w^T x + b = -1$.
  * Margin width: $\gamma = \frac{2}{\|w\|_2}$.
  * Maximizing margin $\frac{2}{\|w\|} \iff$ minimizing $\frac{1}{2}\|w\|_2^2$.
  * **Hard-Margin Convex Quadratic Program (QP):**
  $$\min_{w, b} \frac{1}{2}\|w\|_2^2 \quad \text{subject to } y_i(w^T x_i + b) \ge 1 \quad \forall i$$
  * **Support Vectors & KKT Complementary Slackness:**
  * Points on margin boundaries ($y_i(w^T x_i + b) = 1$).
  * KKT condition: $\alpha_i^* [y_i(w^T x_i + b) - 1] = 0$. Points outside the margin have $\alpha_i = 0$.
  * Weight expansion: $w^* = \sum_{i \in \text{SVs}} \alpha_i^* y_i x_i$. Deleting non-support vectors leaves $w^*$ and $b^*$ completely unchanged.
  * **SVM vs. Logistic Regression:**
  * Adding distant easy points shifts Logistic Regression (log-loss tail gradient $\neq 0$), but leaves SVM completely unchanged (hinge loss is flat $0$ for $y_i z_i \ge 1$).
  * **Hard-Margin Outlier Breakdown:** A single outlier can collapse the margin width or render the constraint set mathematically infeasible.
  * **Soft-Margin SVM & Slack Variables ($\xi_i \ge 0$):**
  * Soft constraint: $y_i(w^T x_i + b) \ge 1 - \xi_i, \quad \xi_i \ge 0$.
  * $\xi_i = 0$: outside margin (correct).
  * $0 < \xi_i \le 1$: inside margin street, correct side of boundary.
  * $\xi_i > 1$: wrong side of decision boundary (misclassified).
  * $\sum \xi_i$ is an analytical upper bound on the number of training errors.
  * **Primal Soft-Margin Objective & The $C$ Dial:**
  $$\min_{w, b, \xi} \frac{1}{2}\|w\|_2^2 + C \sum_{i=1}^n \xi_i \quad \text{subject to } y_i(w^T x_i + b) \ge 1 - \xi_i, \; \xi_i \ge 0$$
  * Equivalent to regularized hinge loss: $\min \frac{1}{2}\|w\|_2^2 + C \sum \max(0, 1 - y_i z_i)$.
  * Small $C$: weak violation penalty, wide margin, many support vectors, high bias (underfit).
  * Large $C$: severe violation penalty, narrow margin, few support vectors, high variance (overfit).
  * **Implementation Differences:**
  * `SVC(kernel="linear")`: dual solver (LibSVM), $\mathcal{O}(n^2)\text{--}\mathcal{O}(n^3)$, stores `.support_vectors_`.
  * `LinearSVC`: primal solver (LibLinear), $\mathcal{O}(n \cdot d)$, fast for large $n$, no support vector list.
  
  ---
### Lecture 22: The Kernel Trick & Nonlinear SVM
  * **Limits of Slack:** Slack handles noise, not non-linear topological shapes (cannot separate concentric rings or XOR).
  * **Geometric Lifting:**
  * 1D to 2D paraboloid lift: $\phi(x) = (x, x^2)$ turns a 1D two-threshold problem into a 2D line.
  * 2D to 3D elliptic lift: $\phi(x) = (x_1^2, \sqrt{2}x_1 x_2, x_2^2)$ turns 2D concentric rings into a flat 3D slicing plane ($x_1^2 + x_2^2 = R^2$).
  * **Lagrangian Dual Formulation:**
  $$\max_\alpha \sum_{i=1}^n \alpha_i - \frac{1}{2}\sum_{i=1}^n \sum_{j=1}^n \alpha_i \alpha_j y_i y_j \langle x_i, x_j \rangle \quad \text{s.t. } 0 \le \alpha_i \le C, \; \sum_{i=1}^n \alpha_i y_i = 0$$
  * $w = \sum \alpha_i y_i x_i$ and $\sum \alpha_i y_i = 0$.
  * Dual prediction rule: $f(x) = \sum_{i \in \text{SVs}} \alpha_i y_i \langle x_i, x \rangle + b$.
  * **The Kernel Trick:**
  * Replace $\langle \phi(x), \phi(z) \rangle$ with a scalar function $K(x, z)$.
  * Feature space $\phi(x)$ is **never constructed in memory**; computational cost depends on input dimension $d$, not the dimension of $\phi$.
  * Worked proof: $(x^T z)^2 = \phi(x)^T \phi(z)$ for $\phi(x) = (x_1^2, \sqrt{2}x_1 x_2, x_2^2)$.
  * **Polynomial Kernel:** $K(x, z) = (\gamma x^T z + r)^d$ ($r = 0$ is homogeneous; $r > 0$ contains lower-order interactions).
  * **Valid Kernels & Mercer’s Theorem:**
  * Gram matrix $K_{ij} = K(x_i, x_j)$ must be Symmetric Positive Semi-Definite (PSD) for all datasets.
  * Necessary condition: $K(x, x) = \|\phi(x)\|_2^2 \ge 0$.
  * Why $K(x, z) = -x^T z$ is invalid (negative self-norm); why $\|x - z\|^2$ is invalid (not PSD).
  * Kernel closure properties: positive scaling, sum, product, and exponential of valid kernels are valid.
  * **Gaussian Radial Basis Function (RBF) Kernel:**
  * $K(x, z) = \exp(-\gamma \|x - z\|^2)$; each support vector acts as an automatically learned landmark.
  * Bandwidth $\gamma = \frac{1}{2\sigma^2}$: small $\gamma \implies$ wide reach, smooth boundary, high bias; large $\gamma \implies$ narrow needle-like spikes, isolated circular islands, severe overfitting.
  * Default `gamma='scale'`: $\gamma = \frac{1}{d \cdot \text{Var}(X)}$ (normalizes for feature dimensionality).
  * Taylor expansion proof proves RBF maps data into an **infinite-dimensional Hilbert space** ($\phi(x) \in \mathbb{R}^\infty$).
  * **Kernel Selection Strategy:**
  * $d \gg n$ (e.g., text classification, genomics): use Linear kernel (`LinearSVC`).
  * Small $d$, moderate $n$: use RBF kernel on standardized features.
  
  ---
## Part 3: Unsupervised Learning & Clustering (Lectures 24, 25)
### Lecture 24: Clustering & $k$-Means
  * **Unsupervised Paradigm:**
  * Dataset $\mathcal{D} = \{x_1, \dots, x_n\} \subset \mathbb{R}^d$ contains no target labels $y$.
  * Classification accuracy does not exist; quality is evaluated using geometric compactness and separation proxies.
  * **Cluster Quality Metrics:**
  * Intra-cluster cohesion (compactness around centroid $\mu_j$ minimized).
  * Inter-cluster separation (distance between centroids $\mu_j, \mu_k$ maximized).
  * **Distance Metric Topologies:**
  * Euclidean ($L_2$): straight line; default for $k$-means; aligns with sample mean minimization.
  * Manhattan ($L_1$): taxicab; robust to single-coordinate outliers; aligns with sample median ($k$-medians).
  * Cosine distance: $1 - \frac{x^T z}{\|x\|\|z\|}$; measures angular displacement, strictly length-invariant; essential for text/embeddings.
  * **Scale Disparity Collapse:**
  * Features with large numerical ranges (e.g., income in $\$10{,}000$s vs. age in years) contribute $99.99\%$ to squared Euclidean distance.
  * Raw $k$-means projects space into an effective 1D line along the largest feature; **standardizing first is mandatory**.
  * **The $k$-Means Objective (Inertia / WCSS):**
  $$J = \sum_{j=1}^k \sum_{x_i \in C_j} \|x_i - \mu_j\|_2^2$$
  * **Lloyd’s Alternating Optimization Algorithm:**
  1. *Initialize:* Pick $k$ initial centroid seeds.
  2. *Assignment Step:* Assign each point to its nearest centroid: $r_{ij} = \mathbf{1}_{\{j = \arg\min_m \|x_i - \mu_m\|^2\}}$ (partitions space into Voronoi cells).
  3. *Update Step:* Move each centroid to the empirical mean of points assigned to it: $\mu_j = \frac{1}{|C_j|}\sum_{x_i \in C_j} x_i$.
  4. Repeat until assignments no longer change.
  * **Convergence Proof:**
  * Step 1 strictly decreases or maintains $J$ (closest center minimizes distance).
  * Step 2 strictly decreases or maintains $J$ (setting $\nabla_{\mu_j} J = 0 \implies \mu_j = \text{mean}$ is the global quadratic minimizer).
  * $J \ge 0$ is bounded below, and there are finite partitions ($k^n$) $\implies$ guaranteed convergence to a **local minimum**.
  * **Sensitivity to Initialization:**
  * Different starting centroids converge to different local minima and different $J$.
  * Multiple restarts (`n_init=10` in scikit-learn).
  * $k$-Means++ initialization: selects first seed randomly, then picks subsequent seeds with probability proportional to squared distance from the closest existing centroid: $P(x) \propto D(x)^2$.
  * **The Elbow Method for Choosing $k$:**
  * Plotting $J$ vs. $k$; $J \to 0$ as $k \to n$ (at $k = n$, every point is its own center, $J = 0$).
  * The elbow identifies the point of diminishing marginal returns.
  * **Application:** Color quantization (lossy image compression by clustering 3D RGB pixels into $k=16$ codebook centers).
  
  ---
### Lecture 25: Hierarchical Clustering
  * **The Multi-Scale Paradigm:** Resolves the "2 or 4 clusters" dilemma by building a continuous nested hierarchy instead of forcing a single flat partition up front.
  * **Agglomerative (Bottom-Up) Algorithm:**
  1. Start with $n$ singleton clusters ($C_i = \{x_i\}$).
  2. Compute $n \times n$ pairwise distance matrix $D$.
  3. Merge the two closest clusters $(A^*, B^*) = \arg\min_{A, B} d(A, B)$.
  4. Update distance matrix using linkage criterion.
  5. Repeat exactly $n - 1$ times until 1 root cluster contains all points.
  * **Dendrogram Anatomy & Reading:**
  * Leaves: individual observations at height $h = 0$.
  * Crossbars: binary merge events.
  * **Merge Height ($h$):** Represents the exact distance $d(A, B)$ between the two clusters being merged.
  * **The Horizontal Cut Rule:** Drawing a horizontal line at height $h_{\text{cut}}$ crossing $k$ vertical lines partitions the data into exactly **$k$ flat clusters**.
  * Widest vertical gap heuristic: cutting inside the largest vertical gap where no merges occur yields the most natural separation.
  * **Linkage Criteria (Inter-Cluster Distances):**
  1. **Single Linkage:** $d_{\text{single}}(A, B) = \min_{x \in A, y \in B} d(x, y)$ (closest boundary pair; follows non-convex chains like Two Moons; vulnerable to the **chaining effect** where noise points bridge clusters).
  2. **Complete Linkage:** $d_{\text{complete}}(A, B) = \max_{x \in A, y \in B} d(x, y)$ (farthest boundary pair; equals cluster diameter $\text{diam}(A \cup B)$; produces compact, equal-sized spherical clusters; sensitive to outliers).
  3. **Average Linkage:** $d_{\text{avg}}(A, B) = \frac{1}{|A||B|}\sum_{x \in A}\sum_{y \in B} d(x, y)$ (compromise between single and complete).
  4. **Ward’s Linkage:** $\Delta J(A, B) = \frac{|A||B|}{|A|+|B|}\|\mu_A - \mu_B\|_2^2$ (merges pair that minimizes the increase in total within-cluster variance/inertia; requires Euclidean distance; default in scikit-learn).
  * **$k$-Means vs. Hierarchical Clustering:**
  * Choose $k$: before fitting ($k$-means) vs. after fitting by cutting tree (hierarchical).
  * Output: flat clusters + centroids vs. nested dendrogram tree.
  * Determinism: stochastic local minima ($k$-means) vs. strictly deterministic (hierarchical).
  * New data: `.predict()` assigns to nearest centroid in $\mathcal{O}(k \cdot d)$ ($k$-means) vs. no `.predict()` (hierarchical must refit or train proxy classifier).
  * Scaling: $k$-means is $\mathcal{O}(n \cdot k \cdot d)$ (scales to millions); hierarchical is $\mathcal{O}(n^2 \log n)$ time and $\mathcal{O}(n^2)$ memory (impractical for $n > 20{,}000$).
  
  ---
## Part 4: Bonus Bridge — PCA Concepts on the Practice Exam (P9, T8, T9, C4)
  * **Standardization Before PCA (P9):** Features measured in different units (e.g., millimeters vs. meters) must be standardized; otherwise, the largest unit artificially dominates the covariance matrix and principal components.
  * **PCA Data Leakage (T8):** PCA learns variance axes from data; it must be fitted **strictly on the training partition** (`pca.fit(X_train)`), never on the full dataset before splitting.
  * **Orthogonality of Principal Components (T9):** Because the covariance matrix $\Sigma = \frac{1}{n-1}X^T X$ is symmetric, its eigenvectors are mutually perpendicular: $\text{PC}_1 \perp \text{PC}_2$ (their sample covariance is identically zero).
  * **Parameters vs. Hyperparameters in PCA (C4):** `n_components` ($k$) is a **hyperparameter**; the learned principal directions (eigenvectors) and explained variance ratios are **parameters**.
  
  ---