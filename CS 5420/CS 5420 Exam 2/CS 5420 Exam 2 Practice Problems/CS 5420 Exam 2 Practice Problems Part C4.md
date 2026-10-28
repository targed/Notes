#### Problem 1 (Ridge and Lasso Regression)
  * **(a)** The regularization penalty strength $\alpha$ (`alpha`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The weight vector coefficients $w^*$ resulting from solving $(X^T X + \alpha I)^{-1} X^T y$ (`coef_`).  
  *Classification:* [ $\quad$ ]
  * **(c)** The tolerance threshold `tol` determining when iterative coordinate descent stops in Lasso.  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 2 (Logistic Regression & Classification Decisions)
  * **(a)** The inverse regularization strength $C$ (`C = 1 / \lambda`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The scalar bias / intercept $b$ (`intercept_`).  
  *Classification:* [ $\quad$ ]
  * **(c)** The classification decision threshold $\tau$ (e.g., setting $\hat{y} = 1$ if $\hat{p} \ge 0.15$ for clinical screening).  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 3 (Gaussian Naive Bayes)
  * **(a)** The empirical class-conditional feature variances $\sigma_{jc}^2$ (`var_`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The variance smoothing stability floor `var_smoothing` (added to variances to prevent zero-division).  
  *Classification:* [ $\quad$ ]
  * **(c)** The empirical class prior probabilities $P(Y = c)$ (`class_prior_`).  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 4 ($k$-Nearest Neighbors)
  * **(a)** The neighborhood size $k$ (`n_neighbors`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The Minkowski distance metric power parameter $p$ (e.g., $p = 1$ vs. $p = 2$).  
  *Classification:* [ $\quad$ ]
  * **(c)** The neighbor weighting scheme (`weights='uniform'` vs. `weights='distance'`).  
  *Classification:* [ $\quad$ ]
  * **(d)** The stored reference training data matrix $X_{\text{train}}$ retained in memory to serve predictions.  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 5 (Support Vector Machines — RBF & Linear)
  * **(a)** The Gaussian RBF kernel bandwidth parameter $\gamma$ (`gamma`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The dual Lagrange multipliers $\alpha_i y_i$ solved via Quadratic Programming (`dual_coef_`).  
  *Classification:* [ $\quad$ ]
  * **(c)** The coordinate vectors of the active support vectors (`support_vectors_`).  
  *Classification:* [ $\quad$ ]
  * **(d)** The polynomial kernel degree $d$ (`degree`).  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 6 ($k$-Means Clustering)
  * **(a)** The number of clusters $k$ (`n_clusters`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The number of random centroid restarts `n_init`.  
  *Classification:* [ $\quad$ ]
  * **(c)** The converged centroid coordinates $\mu_1, \dots, \mu_k$ (`cluster_centers_`).  
  *Classification:* [ $\quad$ ]
  * **(d)** The cluster assignment labels $r_{ij} \in \{0, 1\}$ for each training instance (`labels_`).  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 7 (Agglomerative Hierarchical Clustering)
  * **(a)** The linkage criterion (e.g., `linkage='complete'` vs. `linkage='ward'`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The horizontal cut height `distance_threshold` used to extract flat clusters from the tree.  
  *Classification:* [ $\quad$ ]
  * **(c)** The $(n - 1) \times 4$ linkage matrix $Z$ recording the sequential merge heights and cluster pairings.  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 8 (Principal Component Analysis — PCA)
  * **(a)** The number of dimensions to retain `n_components`.  
  *Classification:* [ $\quad$ ]
  * **(b)** The empirical feature mean vector $\mu$ subtracted during centering (`mean_`).  
  *Classification:* [ $\quad$ ]
  * **(c)** The principal direction eigenvectors $V_k$ defining the projection axes (`components_`).  
  *Classification:* [ $\quad$ ]
  * **(d)** The eigenvalues $\lambda_k$ measuring the variance explained by each principal component (`explained_variance_`).  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 9 (First-Order Optimization / Gradient Descent)
  * **(a)** The learning rate step-size multiplier $\eta$ (`learning_rate`).  
  *Classification:* [ $\quad$ ]
  * **(b)** The mini-batch size $B$ (`batch_size`).  
  *Classification:* [ $\quad$ ]
  * **(c)** The maximum number of training iterations `max_iter`.  
  *Classification:* [ $\quad$ ]
  * **(d)** The model parameter weights $w^{(t)}$ updated at each iteration step.  
  *Classification:* [ $\quad$ ]
  
  ---
#### Problem 10 (Decision Trees & Polynomial Feature Expansion)
  * **(a)** The polynomial expansion degree $d$ in `PolynomialFeatures(degree=d)`.  
  *Classification:* [ $\quad$ ]
  * **(b)** The maximum allowable tree depth `max_depth`.  
  *Classification:* [ $\quad$ ]
  * **(c)** The feature indices and numerical threshold split values selected at each internal decision node (`tree_.threshold`, `tree_.feature`).  
  *Classification:* [ $\quad$ ]
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C4
### Core Classification Rules (from Lectures 2, 20):
  * **Parameters (P):** Estimated automatically from the data during `.fit()`. In `scikit-learn`, these attributes **always end with a trailing underscore** (e.g., `coef_`, `cluster_centers_`, `components_`).
  * **Hyperparameters (H):** Specified by the engineer **before** calling `.fit()`. They dictate model capacity, regularization strength, or algorithmic optimization settings. In `scikit-learn`, they are passed as constructor arguments.
  
  ---
### Problem 1 Solution (Ridge and Lasso Regression)
  * **(a) `alpha` $\implies$ H (Hyperparameter)**  
  *Reasoning:* Sets the capacity of the model. Evaluated across validation splits; cannot be learned from training data because training loss minimization would trivially select $\alpha = 0$.
  * **(b) `coef_` ($w^*$) $\implies$ P (Parameter)**  
  *Reasoning:* Learned from the training data by evaluating the analytical Normal Equations $(X^T X + \alpha I)^{-1} X^T y$.
  * **(c) `tol` $\implies$ H (Hyperparameter)**  
  *Reasoning:* An optimization stopping threshold specified before execution to terminate coordinate descent.
  
  ---
### Problem 2 Solution (Logistic Regression & Classification Decisions)
  * **(a) `C` $\implies$ H (Hyperparameter)**  
  *Reasoning:* The inverse regularization parameter ($C = 1/\lambda$) controls the capacity of the decision boundary; configured before `.fit()`.
  * **(b) `intercept_` ($b$) $\implies$ P (Parameter)**  
  *Reasoning:* The scalar bias term learned from data during numerical optimization (L-BFGS).
  * **(c) Decision Threshold $\tau$ $\implies$ H (Hyperparameter)**  
  *Reasoning:* The decision threshold is an external operational policy knob set by the engineer (e.g., shifting from $\tau = 0.50$ to $\tau = 0.15$ to prioritize Recall in medical screening); it is not learned by the loss function.
  
  ---
### Problem 3 Solution (Gaussian Naive Bayes)
  * **(a) `var_` ($\sigma_{jc}^2$) $\implies$ P (Parameter)**  
  *Reasoning:* Learned directly from training samples by computing the empirical variance of feature $j$ within class $c$: $\frac{1}{N_c}\sum (x_{ij} - \mu_{jc})^2$.
  * **(b) `var_smoothing` $\implies$ H (Hyperparameter)**  
  *Reasoning:* A stability hyperparameter (default: $10^{-9}$) chosen before fitting to prevent zero-variance divisions.
  * **(c) `class_prior_` ($P(Y=c)$) $\implies$ P (Parameter)**  
  *Reasoning:* Learned from the relative class frequencies in the training data: $N_c / N$.
  
  ---
### Problem 4 Solution ($k$-Nearest Neighbors)
  * **(a) `n_neighbors` ($k$) $\implies$ H (Hyperparameter)**  
  *Reasoning:* Dictates the effective capacity ($\text{DoF} \approx N/k$). Setting $k$ is an architectural choice tuned via cross-validation.
  * **(b) Distance Metric Power $p$ $\implies$ H (Hyperparameter)**  
  *Reasoning:* Chooses the Minkowski metric topology ($p=1$ Manhattan, $p=2$ Euclidean); set before inference.
  * **(c) `weights` $\implies$ H (Hyperparameter)**  
  *Reasoning:* Determines whether neighbor votes are counted equally (`'uniform'`) or weighted inversely by distance (`'distance'`).
  * **(d) Stored Reference Matrix $X_{\text{train}}$ $\implies$ P (Parameter / Non-Parametric Representation)**  
  *Reasoning:* In non-parametric lazy learning, there is no weight vector $w$; the stored training instances themselves constitute the empirical parameters of the model.
  
  ---
### Problem 5 Solution (Support Vector Machines — RBF & Linear)
  * **(a) `gamma` ($\gamma$) $\implies$ H (Hyperparameter)**  
  *Reasoning:* Sets the Gaussian RBF bandwidth ($\gamma = \frac{1}{2\sigma^2}$). Controls the curvature and reach of the decision boundary; set before `.fit()`.
  * **(b) `dual_coef_` ($\alpha_i y_i$) $\implies$ P (Parameter)**  
  *Reasoning:* The dual Lagrange multipliers solved by the Quadratic Programming optimizer (SMO) during `.fit()`.
  * **(c) `support_vectors_` $\implies$ P (Parameter)**  
  *Reasoning:* The specific subset of training data points that lie on or violate the margin boundaries (where $\alpha_i^* > 0$), identified automatically by the solver.
  * **(d) `degree` ($d$) $\implies$ H (Hyperparameter)**  
  *Reasoning:* The degree of the polynomial kernel basis expansion; chosen before training.
  
  ---
### Problem 6 Solution ($k$-Means Clustering)
  * **(a) `n_clusters` ($k$) $\implies$ H (Hyperparameter)**  
  *Reasoning:* The number of clusters to partition the data into; chosen before fitting (e.g., via the Elbow Method).
  * **(b) `n_init` $\implies$ H (Hyperparameter)**  
  *Reasoning:* The number of random initial seed restarts to run; set before fitting to avoid poor local minima.
  * **(c) `cluster_centers_` ($\mu_k$) $\implies$ P (Parameter)**  
  *Reasoning:* The centroid coordinate vectors computed iteratively as empirical cluster means during Lloyd's algorithm.
  * **(d) `labels_` ($r_{ij}$) $\implies$ P (Parameter)**  
  *Reasoning:* The discrete cluster assignments learned from data during the assignment step of Lloyd's algorithm.
  
  ---
### Problem 7 Solution (Agglomerative Hierarchical Clustering)
  * **(a) `linkage` $\implies$ H (Hyperparameter)**  
  *Reasoning:* The inter-cluster distance definition (`'single'`, `'complete'`, `'average'`, `'ward'`); selected by the practitioner before building the tree.
  * **(b) `distance_threshold` $\implies$ H (Hyperparameter)**  
  *Reasoning:* The horizontal cut height chosen to prune the dendrogram into flat clusters.
  * **(c) Linkage Matrix $Z$ $\implies$ P (Parameter / Structural Output)**  
  *Reasoning:* Computed from the dataset through the $n - 1$ sequential minimum-distance merges.
  
  ---
### Problem 8 Solution (Principal Component Analysis — PCA)
  * **(a) `n_components` $\implies$ H (Hyperparameter)**  
  *Reasoning:* The target low-dimensional subspace rank $k$ chosen by the user before decomposition.
  * **(b) `mean_` ($\mu$) $\implies$ P (Parameter)**  
  *Reasoning:* The empirical sample mean vector computed from the data to center coordinates: $\bar{x} = \frac{1}{n}\sum x_i$.
  * **(c) `components_` ($V_k$) $\implies$ P (Parameter)**  
  *Reasoning:* The orthonormal eigenvectors of the covariance matrix $\Sigma = \frac{1}{n-1}X^T X$, computed via SVD during `.fit()`.
  * **(d) `explained_variance_` ($\lambda_k$) $\implies$ P (Parameter)**  
  *Reasoning:* The eigenvalues corresponding to the principal components, measuring the variance along each axis.
  
  ---
### Problem 9 Solution (First-Order Optimization / Gradient Descent)
  * **(a) `learning_rate` ($\eta$) $\implies$ H (Hyperparameter)**  
  *Reasoning:* The step-size scalar chosen before training; must satisfy $\eta < \frac{2}{\lambda_{\max}}$ to ensure convergence.
  * **(b) `batch_size` ($B$) $\implies$ H (Hyperparameter)**  
  *Reasoning:* The sample slice size ($B = 1, 64, \dots, n$) configured in the data loader.
  * **(c) `max_iter` $\implies$ H (Hyperparameter)**  
  *Reasoning:* The hard computational iteration budget set to terminate optimization.
  * **(d) Weights $w^{(t)}$ $\implies$ P (Parameter)**  
  *Reasoning:* The model coefficients adjusted iteratively at each step via $-\eta \nabla J(w)$.
  
  ---
### Problem 10 Solution (Decision Trees & Polynomial Features)
  * **(a) `degree` $\implies$ H (Hyperparameter)**  
  *Reasoning:* Controls the non-linear capacity of the feature expansion; set before transformation.
  * **(b) `max_depth` $\implies$ H (Hyperparameter)**  
  *Reasoning:* A structural pruning threshold set before training to prevent decision trees from overfitting.
  * **(c) Split Thresholds and Features $\implies$ P (Parameter)**  
  *Reasoning:* The exact feature column $j$ and numerical cut value $\theta$ chosen at each internal node are learned from training data by maximizing Information Gain or minimizing Gini Impurity.
  
  ---