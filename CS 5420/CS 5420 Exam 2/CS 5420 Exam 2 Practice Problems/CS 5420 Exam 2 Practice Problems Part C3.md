#### Scenario (a) (High-Degree Polynomial on Small Sample)
  A degree-12 polynomial regression model without regularization is trained on $N = 35$ data points:  
  $$\text{Training } R^2 = 0.99, \quad \text{5-Fold CV } R^2 = -0.42$$
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (b) (Over-Regularized Linear Classifier)
  A linear Support Vector Classifier is fit with an extremely small penalty parameter $C = 10^{-5}$ on a balanced binary dataset ($50\%$ positive, $50\%$ negative):  
  $$\text{Training Accuracy} = 0.512, \quad \text{5-Fold CV Accuracy} = 0.510$$
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (c) (Learning Curve Plateau with Flat Slopes)
  A diagnostic learning curve for an automated credit scoring model reveals:
  * At $n = 500$: $\text{Train MSE} = 0.320, \quad \text{CV MSE} = 0.335$
  * At $n = 5{,}000$: $\text{Train MSE} = 0.320, \quad \text{CV MSE} = 0.325$
  * Both curves have flattened into horizontal parallel lines ($\frac{d S_{\text{val}}}{dn} \approx 0$), while the human expert error floor on this task is $\text{MSE} \approx 0.050$.
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (d) (Unpruned Decision Tree)
  An unconstrained decision tree (`max_depth=None`, `min_samples_split=2`) is trained on a customer attrition dataset:  
  $$\text{Training Accuracy} = 1.000, \quad \text{10-Fold CV Accuracy} = 0.735$$
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (e) (Proximity to the Noise Floor)
  A regularized neural network achieves:  
  $$J_{\text{train}} = 0.042, \quad J_{\text{cv}} = 0.045$$
  where the estimated Bayes optimal irreducible noise floor for the problem is $\sigma^2 = \text{Var}(\epsilon) = 0.040$.
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (f) ($k$-NN Neighborhood Size Nearing $N$)
  A $k$-NN classifier is evaluated on a dataset of $N = 400$ patient records using neighborhood size $k = 360$:  
  $$\text{Training Error} = 0.380, \quad \text{CV Error} = 0.385$$
  The dataset has an empirical majority class baseline error rate of $38.0\%$.
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (g) (Learning Curve with a Shrinking Gap)
  An image recognition pipeline shows that at $n = 2{,}000$ images, the gap between training accuracy ($99.0\%$) and cross-validation accuracy ($81.0\%$) is $18.0\%$. At $n = 5{,}000$ images, the gap narrows to $10.0\%$ (training is $97.5\%$, validation is $87.5\%$), and the validation curve still has a steep positive upward slope ($\frac{d S_{\text{val}}}{dn} > 0$).
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (h) (Linear Model on Non-Linear Topology)
  A simple linear regression model is fit to the Concentric Rings benchmark:  
  $$\text{Train MSE} = 0.490, \quad \text{CV MSE} = 0.510$$
  where predicting the mean target value yields an error of $0.500$.
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (i) (High-Dimensional Unregularized Regression)
  A Ridge regression model fit with an infinitesimal penalty $\alpha = 10^{-7}$ on $d = 2{,}500$ features and $n = 120$ samples yields:  
  $$\text{Train MSE} = 0.008, \quad \text{CV MSE} = 5.240$$
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
#### Scenario (j) (Tuned Clinical Pipeline)
  A tuned pipeline combining `StandardScaler` and `SVC(kernel="rbf", C=1.0, gamma="scale")` on the Breast Cancer Wisconsin dataset achieves:  
  $$\text{Training Accuracy} = 0.982, \quad \text{5-Fold CV Accuracy} = 0.976 \pm 0.012$$
  The estimated biopsy lab measurement error rate is $\approx 2.0\%$.
  * **Problem:** ___________________________  
  * **One Fix:** ______________________________________________________________
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C3
### Diagnostic Decision Rules (from Lecture 19):
  * **High Variance (Overfitting):** $J_{\text{train}}$ is low, $J_{\text{cv}} \gg J_{\text{train}}$ (large generalization gap).  
  *Fixes:* Collect more data ($N \uparrow$), use fewer features ($d \downarrow$), increase regularization ($\lambda \uparrow, C \downarrow$), simplify model class.
  * **High Bias (Underfitting):** $J_{\text{train}}$ is high, $J_{\text{cv}} \approx J_{\text{train}}$ (gap is small, error floor is unacceptably high).  
  *Fixes:* Add domain features ($d \uparrow$), add polynomial/interaction terms, decrease regularization ($\lambda \downarrow, C \uparrow$), use a more flexible/expressive model. *(Note: Collecting more data fails).*
  * **Working Well:** $J_{\text{train}}$ is low, $J_{\text{cv}}$ is low and close to $J_{\text{train}}$, nearing the noise floor $\text{Var}(\epsilon)$.  
  *Action:* Stop tuning / model is calibrated.
  
  ---
### Scenario (a)
  * **Problem:** **Overfitting (High Variance)**
  * **Why:** $\text{Train } R^2 = 0.99$ (near-perfect fit), but $\text{CV } R^2 = -0.42$ (worse than predicting the mean!). A degree-12 polynomial on 35 points has 13 degrees of freedom; it interpolates sample noise and oscillates wildly.
  * **One Fix (any of the following):**
  * Decrease polynomial degree (e.g., from degree 12 to degree 2 or 3).
  * Add an $L_2$ Ridge penalty ($\alpha \|w\|_2^2$) with cross-validated $\alpha$.
  * Collect more training data ($N \gg 35$). `[L11, L19]`
  
  ---
### Scenario (b)
  * **Problem:** **Underfitting (High Bias)**
  * **Why:** Training accuracy ($51.2\%$) and CV accuracy ($51.0\%$) are both barely above the $50\%$ random coin-flip baseline, and the gap between them is nearly zero ($\Delta = 0.2\%$). In SVMs, $C = \frac{1}{\lambda}$; setting $C = 10^{-5}$ imposes an overwhelming penalty on the weights ($\lambda = 10^5$), forcing $w \to \mathbf{0}$ and creating a completely rigid, flat model.
  * **One Fix (any of the following):**
  * Increase $C$ (e.g., to $C = 1.0$ or $C = 10.0$) to reduce regularization.
  * Add non-linear features (or use an RBF kernel). `[L19, L20, L21]`
  
  ---
### Scenario (c)
  * **Problem:** **Underfitting (High Bias)**
  * **Why:** The gap between Train MSE ($0.320$) and CV MSE ($0.325$) is tiny, but both curves have flatlined at an error floor of $0.32$, far above the achievable human baseline of $0.050$. The flat slope ($\frac{dS_{\text{val}}}{dn} \approx 0$) proves that gathering $10\times$ more data ($500 \to 5{,}000$) failed to improve performance. The model class lacks the expressive capacity to capture the true underlying signal.
  * **One Fix (any of the following):**
  * Add new domain features or non-linear interaction terms.
  * Switch to a higher-capacity model class (e.g., Random Forest or Gradient Boosted Trees).
  * *(Note: Collecting more data is explicitly wrong here).* `[L19]`
  
  ---
### Scenario (d)
  * **Problem:** **Overfitting (High Variance)**
  * **Why:** Training accuracy is $100\%$ while cross-validation accuracy drops to $73.5\%$, creating a massive $26.5\%$ generalization gap. An unconstrained decision tree splits until every leaf node contains a single pure sample, perfectly memorizing training noise.
  * **One Fix (any of the following):**
  * Constrain tree depth by setting `max_depth` (e.g., `max_depth=5`).
  * Increase `min_samples_split` or `min_samples_leaf` to prevent single-sample leaves.
  * Switch to an ensemble method (Random Forest). `[L02, L19]`
  
  ---
### Scenario (e)
  * **Problem:** **Working Well (Calibrated / Optimal)**
  * **Why:** $J_{\text{train}} = 0.042$ and $J_{\text{cv}} = 0.045$ are both low, the generalization gap is small ($\Delta = 0.003$), and both errors are approaching the theoretical irreducible noise floor ($\sigma^2 = 0.040$). No algorithm can achieve an expected error below $\sigma^2$.
  * **One Fix:**
  * **Stop tuning** / no fix needed (model has achieved optimal balance near the Bayes error limit). `[L19]`
  
  ---
### Scenario (f)
  * **Problem:** **Underfitting (High Bias)**
  * **Why:** Both training error ($38.0\%$) and CV error ($38.5\%$) match the trivial majority baseline error rate ($38.0\%$). In $k$-NN, model capacity scales as $\text{DoF} \approx \frac{N}{k}$. Setting $k = 360$ on $N = 400$ samples forces the neighborhood to encompass $90\%$ of the entire dataset, smoothing away all local decision boundaries and predicting the global majority class.
  * **One Fix:**
  * Decrease neighborhood size $k$ (e.g., from $k = 360$ down to $k = 7$ or $k = 15$) to restore local boundary resolution. `[L14, L19]`
  
  ---
### Scenario (g)
  * **Problem:** **Overfitting (High Variance)**
  * **Why:** There is a significant gap ($10.0\%$) between training accuracy ($97.5\%$) and validation accuracy ($87.5\%$), but the validation curve is **still actively sloping upward** ($\frac{dS_{\text{val}}}{dn} > 0$). This positive slope proves that adding training samples is actively constraining parameter variance and closing the generalization gap.
  * **One Fix (any of the following):**
  * Collect and label more training images ($N > 5{,}000$).
  * Apply data augmentation to synthetically expand training volume.
  * Increase weight decay (regularization) or apply dropout. `[L19]`
  
  ---
### Scenario (h)
  * **Problem:** **Underfitting (High Bias)**
  * **Why:** Both train MSE ($0.49$) and CV MSE ($0.51$) are near $0.50$, which matches random guessing on centered concentric circles. A linear model is constrained to a flat hyperplane; it cannot separate concentric rings regardless of how many training iterations are run.
  * **One Fix (any of the following):**
  * Add the non-linear paraboloid feature $z = x_1^2 + x_2^2$ (squared distance from center).
  * Use an SVM with an RBF or quadratic polynomial kernel (`kernel="rbf"`). `[L21, L22]`
  
  ---
### Scenario (i)
  * **Problem:** **Overfitting (High Variance)**
  * **Why:** In high dimensions ($d = 2{,}500$) with a small sample size ($n = 120$), the feature count vastly exceeds observations ($p \gg n$). An infinitesimal penalty $\alpha = 10^{-7}$ allows the weights to grow large and fit the $n=120$ points with near-zero training error ($0.008$), but the unconstrained parameters fail completely on out-of-fold validation data ($\text{MSE} = 5.240$).
  * **One Fix (any of the following):**
  * Increase the regularization parameter $\alpha$ (e.g., to $\alpha = 1.0$ or $\alpha = 10.0$).
  * Perform feature selection (pruning features from $2{,}500$ down to the most predictive subset). `[L11, L19]`
  
  ---
### Scenario (j)
  * **Problem:** **Working Well (Calibrated / Optimal)**
  * **Why:** Training accuracy ($98.1\%$) and cross-validation accuracy ($97.6\%$) are both high, the generalization gap is narrow ($0.5\%$), and the validation performance is within the $2.0\%$ irreducible measurement noise floor of the laboratory.
  * **One Fix:**
  * **Stop tuning** / proceed to final evaluation on the vaulted test set. `[L18, L19, L22]`
  
  ---