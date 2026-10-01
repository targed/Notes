# Part B — True / False Practice Questions
#### Q1. Optimization vs. Tuning (Mirror of T1)
  **[ $\quad$ ]** In Ridge regression, the parameter weights $w$ and the regularization penalty parameter $\alpha$ are both learned automatically by the optimizer minimizing the training cost function $J(w, \alpha) = \|y - Xw\|_2^2 + \alpha \|w\|_2^2$.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q2. Random Seeds & Reproducibility (Mirror of T2)
  **[ $\quad$ ]** If an engineer sets `random.seed(42)`, `np.random.seed(42)`, and `torch.manual_seed(42)`, training a deep neural network on an NVIDIA GPU is guaranteed to produce bitwise identical model weights across any two runs.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q3. NumPy Tensor Reductions (Mirror of T3)
  **[ $\quad$ ]** For a 3D NumPy array $A$ with shape `(5, 8, 3)`, the reduction operation `A.mean(axis=0)` outputs an array with shape `(8, 3)`.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q4. Feature Scaling & Distribution Shape (Mirror of T4)
  **[ $\quad$ ]** Applying `StandardScaler` to an exponential, heavily right-skewed feature transforms the distribution into a symmetric, bell-shaped Gaussian distribution.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q5. Production Categorical Encoding (Mirror of T5)
  **[ $\quad$ ]** When `OneHotEncoder(handle_unknown='ignore')` transforms a test instance containing a category never observed in training, it encodes that feature as an all-zero vector $[0, 0, \dots, 0]$ without raising an error.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q6. Preprocessing Data Leakage (Mirror of T6)
  **[ $\quad$ ]** Imputing missing values using the global median computed across the combined training and test rows before splitting is completely leak-free as long as the target column $y$ is excluded from the median calculation.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q7. Gradient Descent Step Counts (Mirror of T7)
  **[ $\quad$ ]** In Online Stochastic Gradient Descent (SGD) on a training dataset of $n = 5{,}000$ samples, the optimizer executes exactly $5{,}000$ parameter updates in one complete epoch.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q8. Mini-Batch Equivalence (Mirror of T8)
  **[ $\quad$ ]** A mini-batch gradient descent algorithm configured with batch size $B = 1$ is mathematically and operationally identical to Stochastic Gradient Descent.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q9. $k$-NN Complexity & Model Capacity (Mirror of T9 & T10)
  **[ $\quad$ ]** In $k$-Nearest Neighbors, increasing the neighborhood size $k$ from $1$ to $n$ (the total sample size) increases the variance of the model and leads to severe overfitting.  
  *Justification:* _________________________________________________________________
  
  ---
#### Q10. Linear Collinearity & Invertibility (Mirror of T12)
  **[ $\quad$ ]** If a design matrix contains three features where column $x_3$ is an exact linear combination of the first two ($x_3 = 2x_1 - 4x_2$), the Gram matrix $X^T X$ is singular and has no matrix inverse.  
  *Justification:* _________________________________________________________________
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for Part B
### Q1. **FALSE**
  * **Justification:** $\alpha$ is a **hyperparameter** chosen externally by the practitioner (ideally via cross-validation on training data); minimizing training loss with respect to $\alpha$ would trivially set $\alpha = 0$.
  * **Deep Dive:** As proven in Lecture 11, $\frac{\partial J}{\partial \alpha} = \|w\|_2^2 \ge 0$. As long as the weights are non-zero, training error is strictly increasing in $\alpha$. The training set will always favor an unconstrained OLS model ($\alpha = 0$). Parameters ($w$) are learned internally by `.fit()`; hyperparameters ($\alpha$) must be tuned across hold-out validation sets. `[L02, L11]`
  
  ---
### Q2. **FALSE**
  * **Justification:** Parallel atomic addition operations (`atomicAdd`) in CUDA kernels execute in non-deterministic order across GPU warps, and IEEE 754 floating-point addition is non-associative.
  * **Deep Dive:** PRNG seeds only fix the sequence of pseudorandom numbers. They do not control thread scheduling across GPU streaming multiprocessors. Because $(a + b) + c \neq a + (b + c)$ in floating-point arithmetic, differing thread accumulation orders produce numerical drift across runs. Bitwise GPU determinism requires setting strict flags like `torch.use_deterministic_algorithms(True)` and disabling cuDNN benchmarking. `[L04]`
  
  ---
### Q3. **TRUE**
  * **Justification:** The axis argument names the axis that disappears; collapsing axis 0 (length 5) leaves axes 1 and 2 with shape `(8, 3)`.
  * **Deep Dive:** In NumPy reduction algebra (`mean`, `sum`, `std`), specifying `axis=0` computes the average across the 5 slices along the first dimension:
  $$\text{Shape: } (5, 8, 3) \xrightarrow{\text{axis}=0} (8, 3)$$
  Axes 1 and 2 remain intact. `[L05]`
  
  ---
### Q4. **FALSE**
  * **Justification:** `StandardScaler` is an affine linear transformation ($z = \frac{x - \mu}{\sigma}$); it changes the scale and center, not the shape or skewness ($\gamma_1$).
  * **Deep Dive:** As proven in Lecture 8, standardized moments are invariant under positive linear scaling:
  $$\gamma_1(aX + b) \equiv \gamma_1(X)$$
  Standardizing shifts the mean to $0$ and scales variance to $1$, but the long positive tail remains. Reshaping an exponential or skewed distribution into a Gaussian bell curve requires a **non-linear power transformation** (such as $\ln(x)$, Box-Cox, or Yeo-Johnson). `[L08]`
  
  ---
### Q5. **TRUE**
  * **Justification:** Setting `handle_unknown='ignore'` instructs the encoder to emit an all-zero binary vector ($[0, 0, \dots, 0]$) for any category not observed during `.fit()`, preventing a runtime crash.
  * **Deep Dive:** By default, `handle_unknown='error'` raises a fatal `ValueError` if an unseen category appears during inference. With `'ignore'`, the transformer outputs an all-zero vector, ensuring the pipeline completes inference without throwing an unhandled exception. `[L08]`
  
  ---
### Q6. **FALSE**
  * **Justification:** Computing the median across both splits contaminates the training set with test set distribution statistics, which is Preprocessing Leakage.
  * **Deep Dive:** The test set must remain completely unobserved until evaluation. If the test set happens to have higher values than the training set, the global median shifts upward. The training features will incorporate distributional knowledge of the held-out test data. Imputers must be fit strictly on `X_train` and applied to `X_test` via `.transform()` (or wrapped in a `Pipeline`). `[L07, L09]`
  
  ---
### Q7. **TRUE**
  * **Justification:** In single-sample Stochastic Gradient Descent ($B = 1$), parameters are updated after processing every individual training point, resulting in exactly $n$ updates per epoch.
  * **Deep Dive:** An epoch is one complete pass through the $n$ samples. 
  * Batch GD ($B = n$): $1$ update per epoch.
  * Mini-Batch GD ($B = 250$): $\lceil 5{,}000 / 250 \rceil = 20$ updates per epoch.
  * SGD ($B = 1$): $5{,}000 / 1 = \mathbf{5{,}000 \text{ updates per epoch}}$. `[L11]`
  
  ---
### Q8. **TRUE**
  * **Justification:** When batch size is $B = 1$, each mini-batch consists of a single sample, which matches the mathematical definition of Stochastic Gradient Descent.
  * **Deep Dive:** Mini-batch gradient descent evaluates $\frac{1}{B}\sum_{i \in \mathcal{B}} \nabla \mathcal{L}_i$. When $B = 1$, the summation collapses to the single-sample loss gradient $\nabla \mathcal{L}_i$, which is identically the Online SGD update step. `[L11]`
  
  ---
### Q9. **FALSE**
  * **Justification:** Increasing $k$ decreases model capacity, which increases bias and decreases variance (leading to underfitting, not overfitting).
  * **Deep Dive:** In $k$-NN, the effective degrees of freedom scale inversely with neighborhood size:
  $$\text{DoF} \approx \frac{n}{k}$$
  * Small $k$ ($k=1$): Complex, jagged decision boundaries with zero training error (low bias, high variance $\implies$ **overfitting**).
  * Large $k$ ($k \to n$): Smooth, rigid boundaries. At $k = n$, the model predicts the global majority class everywhere (high bias, zero variance $\implies$ **underfitting**). `[L14]`
  
  ---
### Q10. **TRUE**
  * **Justification:** Linearly dependent feature columns cause the design matrix $X$ to have rank strictly less than $d+1$, meaning $\det(X^T X) = 0$ (singular and non-invertible).
  * **Deep Dive:** By linear algebra rank theorems:
  $$\text{rank}(X^T X) = \text{rank}(X)$$
  If $x_3 = 2x_1 - 4x_2$, the columns of $X$ are linearly dependent, so $\text{rank}(X) < d+1$. The square $(d+1) \times (d+1)$ Gram matrix $X^T X$ has at least one eigenvalue equal to zero ($\lambda_{\min} = 0$), making its determinant zero. It cannot be inverted to compute the unregularized OLS solution $(X^T X)^{-1} X^T y$. `[L10]`
  
  ---