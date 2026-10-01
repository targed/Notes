### Question 1: Generalization Diagnostics & Structural Overfitting
  An engineer trains an unregularized polynomial regression model of degree $15$ on a continuous physical dataset with $N = 30$ training observations. The resulting model achieves a training Mean Squared Error ($\text{MSE}_{\text{train}}$) of $0.001$, but when evaluated on an independent test partition ($N_{\text{test}} = 100$), the test error explodes to $\text{MSE}_{\text{test}} = 842.6$. 
  
  Which of the following interventions directly targets the primary mathematical cause of this performance discrepancy without altering the polynomial degree?
  
  * **A.** Switch the loss function from Mean Squared Error ($L_2$) to Mean Absolute Error ($L_1$).
  * **B.** Add an $L_2$ Tikhonov regularization penalty ($\alpha \|w\|_2^2$) to the objective function.
  * **C.** Standardize the target variable $y$ to have zero mean and unit variance.
  * **D.** Train the parameters using Mini-Batch Gradient Descent instead of the analytical Normal Equations.
  
  ---
#### Correct Answer: **B**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** The model suffers from severe **high-variance overfitting**. Because the degree is $15$, the model has $d+1 = 16$ degrees of freedom to fit $N = 30$ noisy observations ($d \approx n$). 
  * Under unconstrained Ordinary Least Squares (OLS), the parameter vector oscillates violently to interpolate stochastic noise:
  $$w_j \to \pm 10^5$$
  While the empirical risk approaches zero on training points ($\hat{R}_N \to 0$), the variance of the estimator explodes ($\text{Var}(\hat{w}) \to \infty$).
  * Adding an $L_2$ Ridge penalty ($\alpha \|w\|_2^2$) directly constrains the Euclidean norm of the parameters by solving:
  $$w^* = (X^T X + \alpha I)^{-1} X^T y$$
  * This shrinks the singular components of the weights by the factor $f_j = \frac{\sigma_j^2}{\sigma_j^2 + \alpha}$. It dampens high-frequency polynomial oscillations, trading a small increase in structural bias for a massive reduction in variance without reducing the polynomial degree.
#### Why Distractors Are Wrong:
  * **A is incorrect:** Switching to MAE alters the noise prior from Gaussian to Laplace and fits the conditional median, but an unregularized 16-parameter polynomial on 30 points will still interpolate noise and overfit.
  * **C is incorrect:** Standardizing $y$ merely scales the vertical coordinate axis; it changes the numerical units of the MSE, but leaves the underlying polynomial oscillations and generalization gap unaffected.
  * **D is incorrect:** On a strictly convex linear regression surface, gradient descent converges to the exact same global minimum as the Normal Equations; changing the optimization algorithm does not alter the hypothesis class or the stationary point.
  
  ---
### Question 2: Pandas Indexing Mechanics & Boundary Semantics
  A data scientist partitions a DataFrame `df` containing $1{,}000$ customer records. The index has been sorted alphabetically by account ID: `['ACC_001', 'ACC_002', 'ACC_003', ..., 'ACC_999']`. 
  
  What is the exact behavioral difference between evaluating:
  $$\text{Query 1: } \texttt{df.loc['ACC\_010':'ACC\_015']}$$
  $$\text{Query 2: } \texttt{df.iloc[10:15]}$$
  
  * **A.** Both queries return exactly 5 rows because both operate on identical half-open intervals $[10, 15)$.
  * **B.** `df.loc` returns 6 rows because label-based indexing is **endpoint inclusive**; `df.iloc` returns 5 rows because positional slicing is **endpoint exclusive**.
  * **C.** `df.loc` raises a `KeyError` because string indices cannot be sliced with a colon (`:`); only integers can be sliced.
  * **D.** `df.iloc` returns 6 rows because positional offsets in Pandas are 1-indexed by default.
  
  ---
#### Correct Answer: **B**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** Pandas enforces a strict indexing duality:
  * `.loc[]` operates strictly on **Index Labels** over a **closed interval** $[start, stop]$. The stop boundary is **INCLUSIVE**. Evaluating `'ACC_010':'ACC_015'` extracts all records matching `'ACC_010'`, `'ACC_011'`, `'ACC_012'`, `'ACC_013'`, `'ACC_014'`, and `'ACC_015'` (yielding **6 rows**).
  * `.iloc[]` operates strictly on **Physical Integer Memory Offsets** over a standard Python **half-open interval** $[start, stop)$. The stop boundary is **EXCLUSIVE**. Evaluating `[10:15]` extracts memory offsets $10, 11, 12, 13, 14$ (yielding $15 - 10 = \mathbf{5 \text{ rows}}$).
#### Why Distractors Are Wrong:
  * **A is incorrect:** It overlooks that `.loc` includes the terminal label boundary.
  * **C is incorrect:** Pandas explicitly supports lexicographical label slicing on sorted string indices.
  * **D is incorrect:** Python and Pandas memory offsets are strictly 0-indexed, not 1-indexed.
  
  ---
### Question 3: Multidimensional NumPy Array Broadcasting
  Given two NumPy arrays $A$ and $B$ instantiated as:
  ```python
  A = np.ones((6, 1, 5))
  B = np.zeros((4, 1))
  ```
  What is the resulting shape of the arithmetic operation `C = A + B`, or what error is raised?
  
  * **A.** Shape `(6, 4, 5)`
  * **B.** Shape `(6, 4, 1)`
  * **C.** `ValueError: operands could not be broadcast together with shapes (6,1,5) (4,1)`
  * **D.** Shape `(24, 5)`
  
  ---
#### Correct Answer: **A**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** NumPy aligns dimensions starting from the **trailing (rightmost) axis** and working leftward. Shorter shapes are padded on the left with singleton dimensions ($1$):
  $$\text{Array } A: \quad 6 \quad \times \quad 1 \quad \times \quad 5$$
  $$\text{Array } B: \quad \mathbf{1} \quad \times \quad 4 \quad \times \quad 1$$
  * Evaluate compatibility axis-by-axis:
  * **Axis 2 (Trailing):** $A$ has dimension $5$; $B$ has dimension $1 \implies$ Compatible. Dimension $1$ stretches to $5$.
  * **Axis 1 (Middle):** $A$ has dimension $1$; $B$ has dimension $4 \implies$ Compatible. Dimension $1$ stretches to $4$.
  * **Axis 0 (Leading):** $A$ has dimension $6$; $B$ has padded dimension $1 \implies$ Compatible. Dimension $1$ stretches to $6$.
  * Both arrays broadcast cleanly across each other’s singleton dimensions, producing a dense rank-3 tensor of shape **`(6, 4, 5)`**.
#### Why Distractors Are Wrong:
  * **C is incorrect:** Shapes are fully compatible because every mismatched dimension contains a singleton ($1$).
  * **B is incorrect:** It fails to broadcast the trailing dimension from 1 to 5.
  * **D is incorrect:** Broadcasting stretches dimensions; it does not compute a flattened 2D matrix product.
  
  ---
### Question 4: Rubin's Missing Data Taxonomy
  In an electronic health record (EHR) registry, researchers observe that the laboratory measurement `blood_glucose_level` is missing for $35\%$ of patients. A statistical audit reveals that the probability of `blood_glucose_level` being missing is completely uncorrelated with the patient's actual glucose concentration, but is strongly correlated with the patient's recorded `insurance_tier` and `admission_ward` (outpatient clinic vs. emergency ICU).
  
  Under Donald Rubin’s classical missing data framework, this missingness mechanism is classified as:
  
  * **A.** Missing Completely at Random (MCAR)
  * **B.** Missing at Random (MAR)
  * **C.** Missing Not at Random (MNAR)
  * **D.** Informative Selection Truncation
  
  ---
#### Correct Answer: **B**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** Let $Y_{\text{obs}}$ denote observed covariates (`insurance_tier`, `admission_ward`), $Y_{\text{mis}}$ denote the unobserved feature (`blood_glucose_level`), and $M$ denote the binary missingness indicator variable.
  * **Missing at Random (MAR)** is defined mathematically as:
  $$P(M \mid Y_{\text{obs}}, Y_{\text{mis}}) = P(M \mid Y_{\text{obs}})$$
  The probability of missingness depends systematically on **observed variables in the dataset**, but is conditionally independent of the missing value itself.
  * Because the missingness is explained by `insurance_tier` and `ward`, conditioning on those observed features makes missingness stochastic. Grouped conditional imputation (e.g., imputing median glucose conditioned on ward) is valid and unbiased under MAR.
#### Why Distractors Are Wrong:
  * **A (MCAR) is incorrect:** Under MCAR, $P(M \mid Y_{\text{obs}}, Y_{\text{mis}}) = P(M)$. Missingness must be completely independent of *all* variables, observed and unobserved. Here, missingness depends on insurance and ward, violating MCAR.
  * **C (MNAR) is incorrect:** Under MNAR, the probability of missingness depends directly on the unobserved value itself (e.g., if diabetic patients with extremely high glucose intentionally avoided taking the test).
  * **D is incorrect:** Non-standard terminology; not part of Rubin’s tri-part statistical taxonomy.
  
  ---
### Question 5: `MinMaxScaler` Coordinate Extrapolation
  A continuous feature $X$ has training set bounds $x_{\min, \, \text{train}} = -40.0$ and $x_{\max, \, \text{train}} = +60.0$. An engineer instantiates `MinMaxScaler()` with default scikit-learn parameters and fits it strictly on the training partition. 
  
  During out-of-sample evaluation, a test observation arrives with value $x_{\text{test}} = +85.0$. What numerical value does `scaler.transform()` emit for this test instance?
  
  * **A.** $1.000$
  * **B.** $0.850$
  * **C.** $1.250$
  * **D.** It raises a `ValueError` because $85.0$ exceeds the maximum training bound.
  
  ---
#### Correct Answer: **C**
#### In-Depth Graduate Explanation:
  * **The Mathematical Derivation:**
  The `MinMaxScaler` transformation formula maps an input scalar $x$ to the default range $[0, 1]$ via:
  $$x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}}$$
  * During `.fit(X_train)`, the scaler learns and freezes the empirical boundaries:
  $$x_{\min} = -40.0, \quad x_{\max} = +60.0 \implies x_{\max} - x_{\min} = 60.0 - (-40.0) = \mathbf{100.0}$$
  * During `.transform(X_test)`, the frozen linear equation is evaluated algebraically on $x_{\text{test}} = 85.0$:
  $$x'_{\text{test}} = \frac{85.0 - (-40.0)}{100.0} = \frac{85.0 + 40.0}{100.0} = \frac{125.0}{100.0} = \mathbf{1.250}$$
  * `MinMaxScaler` does not clip outputs to $[0, 1]$ unless explicitly initialized with `clip=True`. It evaluates the linear affine mapping, cleanly emitting values $> 1.0$ or $< 0.0$ for out-of-bounds inputs.
#### Why Distractors Are Wrong:
  * **A is incorrect:** It assumes clipping is enabled by default.
  * **B is incorrect:** Arises from incorrectly dividing by the absolute maximum ($85 / 100$) while ignoring the non-zero minimum ($x_{\min} = -40$).
  * **D is incorrect:** Scikit-learn transformers do not raise exceptions for out-of-bounds test values.
  
  ---
### Question 6: Hessian Curvature & Maximum Stable Learning Rates
  An unregularized Ordinary Least Squares regression model is optimized via Batch Gradient Descent on a quadratic loss surface $J(w) = \frac{1}{n}\|Xw - y\|_2^2$. The empirical Hessian matrix of second derivatives is computed as:
  $$H = \nabla^2 J(w) = \begin{bmatrix} 50.0 & 0.0 \\ 0.0 & 0.5 \end{bmatrix}$$
  
  What is the theoretical **maximum stable learning rate ($\eta_{\text{diverge}}$)** above which gradient descent is mathematically guaranteed to oscillate with expanding amplitude and diverge?
  
  * **A.** $\eta = 2.000$
  * **B.** $\eta = 0.040$
  * **C.** $\eta = 0.020$
  * **D.** $\eta = 4.000$
  
  ---
#### Correct Answer: **B**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** On a convex quadratic loss surface, gradient updates follow:
  $$w^{(t+1)} - w^* = (I - \eta H)(w^{(t)} - w^*)$$
  * For the error vector to contract toward zero ($\lim_{t \to \infty} \|w^{(t)} - w^*\| = 0$), the spectral radius of the iteration matrix $(I - \eta H)$ must be strictly bounded inside the unit circle for all eigenvalues $\lambda_i(H)$:
  $$|1 - \eta \lambda_i(H)| < 1 \quad \forall \; i$$
  $$-1 < 1 - \eta \lambda_i < 1 \implies 0 < \eta \lambda_i < 2 \implies \mathbf{\eta < \frac{2}{\lambda_{\max}(H)}}$$
  * The eigenvalues of the diagonal Hessian $H$ are $\lambda_1 = 50.0$ and $\lambda_2 = 0.5$. Thus, $\lambda_{\max} = 50.0$.
  * Evaluating the upper divergence threshold:
  $$\eta_{\text{diverge}} = \frac{2}{\lambda_{\max}} = \frac{2}{50.0} = \mathbf{0.040}$$
  * If $\eta \ge 0.040$, updates along the steep coordinate axis bounce across the valley with non-diminishing or expanding oscillations, causing numerical divergence.
#### Why Distractors Are Wrong:
  * **C is incorrect:** $\eta = \frac{1}{\lambda_{\max}} = 0.020$ is the conservative bound that guarantees monotonic non-oscillatory descent, but the system remains stable up to $\frac{2}{\lambda_{\max}} = 0.040$.
  * **A and D are incorrect:** Step sizes of $2.0$ or $4.0$ vastly exceed the stability limit ($0.040$) and will cause floating-point overflow (`NaN`) within a few iterations.
  
  ---
### Question 7: Geometric Properties of the OLS Projection Operator
  In multiple linear regression with full column rank ($X \in \mathbb{R}^{n \times (d+1)}$ with $n > d+1$), the vector of predictions is given by $\hat{y} = H y$, where $H = X(X^T X)^{-1} X^T$ is the Hat Matrix, and the residual vector is $r = y - \hat{y} = (I - H)y$.
  
  Which of the following linear algebraic statements is **FALSE**?
  
  * **A.** The Hat Matrix is idempotent: $H^2 = H$.
  * **B.** The residual vector $r$ is orthogonal to the prediction vector $\hat{y}$ ($\langle r, \hat{y} \rangle = 0$).
  * **C.** The trace of the Hat Matrix equals the total number of observations: $\text{Tr}(H) = n$.
  * **D.** The residual operator $(I - H)$ is symmetric: $(I - H)^T = I - H$.
  
  ---
#### Correct Answer: **C**
#### In-Depth Graduate Explanation:
  * **The Algebraic Proof of Falsehood (Statement C):**
  Using the cyclic property of the matrix trace operator ($\text{Tr}(ABC) = \text{Tr}(BCA)$):
  $$\text{Tr}(H) = \text{Tr}\Big( X(X^T X)^{-1} X^T \Big) = \text{Tr}\Big( (X^T X)^{-1} (X^T X) \Big) = \text{Tr}\Big( I_{(d+1)} \Big) = \mathbf{d + 1}$$
  The trace of the projection matrix $H$ equals the **rank of the subspace**, which is the number of estimated parameters ($d + 1$), **NOT** the sample size $n$.
  * **Why the Other Statements Are True:**
  * **A is True:** $H^2 = X(X^TX)^{-1}X^T X(X^TX)^{-1}X^T = X(X^TX)^{-1}[I]X^T = H$. (Projecting an already-projected point changes nothing).
  * **B is True:** $\langle r, \hat{y} \rangle = r^T \hat{y} = ((I - H)y)^T (Hy) = y^T (I - H) H y = y^T (H - H^2) y = y^T (H - H) y = 0$. Residuals are strictly orthogonal to predictions.
  * **D is True:** Since $H$ is symmetric ($H^T = H$), $(I - H)^T = I^T - H^T = I - H$.
  
  ---
### Question 8: Inductive Bias & Distance Metric Distortion
  A loan applicant dataset contains an unordered categorical feature `employment_sector` taking values in $\{\text{'Agriculture', 'Tech', 'Healthcare'}\}$. An analyst encodes this feature using integer mapping:
  $$\text{Agriculture} \to 0, \quad \text{Tech} \to 1, \quad \text{Healthcare} \to 2$$
  The analyst then includes this column directly into a $k$-Nearest Neighbors ($k$-NN) classifier alongside standardized continuous numerical features.
  
  What specific topological defect has been introduced into the $k$-NN decision space?
  
  * **A.** The distance between Agriculture and Healthcare ($D = 2$) is artificially double the distance between Agriculture and Tech ($D = 1$), altering nearest-neighbor rankings based on arbitrary label ordering.
  * **B.** The $k$-NN classifier will fail to execute because nearest-neighbor algorithms require continuous floating-point inputs and cannot process integers.
  * **C.** The design matrix becomes singular, causing the underlying distance metric computation to raise a linear algebra determinant error.
  * **D.** The model treats all three sectors as equidistant vertices on an equilateral simplex in $\mathbb{R}^3$.
  
  ---
#### Correct Answer: **A**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** The $k$-NN algorithm evaluates instance proximity using a metric such as Euclidean distance:
  $$D(x_A, x_B) = \sqrt{\sum_{j=1}^d (x_{A, j} - x_{B, j})^2}$$
  * In reality, `employment_sector` is **nominal**: there is no physical or economic basis to assert that Agriculture is closer to Tech than it is to Healthcare.
  * By assigning integers $0, 1, 2$, the analyst forces an arbitrary 1D spatial metric:
  $$D(\text{Agri}, \text{Tech}) = |0 - 1| = 1.0$$
  $$D(\text{Agri}, \text{Healthcare}) = |0 - 2| = \mathbf{2.0 \quad (2\times \text{ further away!})}$$
  * When finding the $k$ nearest neighbors for an agricultural worker, the algorithm will artificially favor tech workers over healthcare workers purely because of the arbitrary alphabetical integer assignment. Under **One-Hot Encoding**, all three sectors are mapped to orthogonal basis vectors, guaranteeing that all pairs are equidistant ($D = \sqrt{2}$).
#### Why Distractors Are Wrong:
  * **B is incorrect:** $k$-NN handles integer coordinates natively.
  * **C is incorrect:** $k$-NN does not invert matrices; it computes pairwise point differences.
  * **D is incorrect:** Equidistant representation is achieved *only* via One-Hot Encoding, which was omitted here.
  
  ---
### Question 9: Regularization Penalties & Feature Scale Invariance
  An applied ML engineer fits a regularized linear model predicting housing prices. Due to a data pipeline bug, feature $x_1$ (`interior_area`) is measured in **square millimeters** ($\sigma_1 \approx 2 \times 10^8$), while feature $x_2$ (`lot_size`) is measured in **square kilometers** ($\sigma_2 \approx 2 \times 10^{-3}$). 
  
  If the engineer fits a **Ridge Regression model ($\alpha \|w\|_2^2$)** directly on these unstandardized features, what is the primary consequence on the parameter weights?
  
  * **A.** Feature $x_1$ (`interior_area`) will be penalized and shrunk toward zero millions of times more aggressively than $x_2$.
  * **B.** Feature $x_2$ (`lot_size`) will be penalized and shrunk toward zero millions of times more aggressively than $x_1$.
  * **C.** Both features receive an identical shrinkage factor because Ridge adds a uniform diagonal matrix $\alpha I$.
  * **D.** The optimization problem becomes non-convex, creating local minima that trap gradient descent.
  
  ---
#### Correct Answer: **B**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** To produce an equivalent marginal prediction change ($w_j x_j$), the parameter weight must scale inversely with the feature magnitude:
  $$w_j \propto \frac{1}{\text{Scale}(X_j)}$$
  * Because $x_1$ is measured in square millimeters (massive numbers), its learned weight $w_1$ must be tiny (e.g., $w_1 \approx 10^{-8}$).
  * Because $x_2$ is measured in square kilometers (tiny numbers), its learned weight $w_2$ must be massive (e.g., $w_2 \approx 10^{+3}$) to have any impact on house prices.
  * **The Regularization Penalty:** Ridge penalizes squared weight magnitudes uniformly:
  $$\Omega(w) = \alpha (w_1^2 + w_2^2)$$
  $$\text{Penalty on } w_1 = \alpha (10^{-8})^2 = \mathbf{\alpha \cdot 10^{-16} \quad (\text{Effectively zero penalty!})}$$
  $$\text{Penalty on } w_2 = \alpha (10^{+3})^2 = \mathbf{\alpha \cdot 10^{+6} \quad (\mathbf{10^{22}\times \text{ heavier penalty!}})}$$
  * The regularizer crushes $w_2$ toward zero, stripping `lot_size` of all predictive influence, while leaving `interior_area` virtually unregularized purely as an artifact of physical measurement units.
#### Why Distractors Are Wrong:
  * **A is incorrect:** It reverses the relationship; larger feature values yield smaller weights, evading the penalty.
  * **C is incorrect:** While $\alpha I$ is uniform, its interaction with the unscaled data matrix $(X^T X + \alpha I)^{-1}$ produces highly non-uniform shrinkage across unscaled singular directions.
  * **D is incorrect:** Ridge regression is strictly convex regardless of feature scaling.
  
  ---
### Question 10: Asymmetric Classification Costs & Decision Thresholds
  A hospital deploys a machine learning model to screen patients in the emergency department for impending septic shock. A clinical risk assessment establishes the following cost matrix:
  * **False Negative (FN):** An infected septic patient is classified as healthy and sent home; the infection progresses to septic shock, resulting in severe organ failure or death ($\text{Cost} = \$100{,}000$).
  * **False Positive (FP):** A healthy patient is flagged as high-risk; the patient receives a prophylactic blood culture and temporary bedside monitoring ($\text{Cost} = \$500$).
  
  Which metric must be maximized during model evaluation, and how should the probability decision threshold $\tau$ be configured relative to the default symmetric cutoff ($\tau = 0.50$)?
  
  * **A.** Maximize **Precision**; raise the classification decision threshold to $\tau = 0.85$.
  * **B.** Maximize **Specificity**; raise the classification decision threshold to $\tau = 0.85$.
  * **C.** Maximize **Recall (Sensitivity)**; lower the classification decision threshold to $\tau = 0.10$.
  * **D.** Maximize **Classification Accuracy**; maintain the symmetric threshold at $\tau = 0.50$.
  
  ---
#### Correct Answer: **C**
#### In-Depth Graduate Explanation:
  * **The Mechanism:** In clinical triage, the error costs are highly asymmetric:
  $$\text{Cost}(\text{FN}) = 200 \times \text{Cost}(\text{FP})$$
  * A False Negative (missing a septic patient) carries a fatal outcome. To eliminate False Negatives ($FN \to 0$), the pipeline must maximize **Recall (Sensitivity)**:
  $$\text{Recall} = \frac{TP}{TP + FN}$$
  * In Logistic Regression, the model outputs continuous posterior probabilities $\hat{p} = P(\text{Sepsis} \mid x)$.
  * Under standard symmetric loss, the cutoff is $\tau = 0.50$. Under asymmetric costs, Bayes Decision Theory dictates setting the decision boundary where the expected loss of predicting positive equals the expected loss of predicting negative:
  $$\tau^* = \frac{\text{Cost}(\text{FP})}{\text{Cost}(\text{FP}) + \text{Cost}(\text{FN})} = \frac{500}{500 + 100{,}000} \approx \mathbf{0.005}$$
  * By **lowering the threshold significantly below $0.50$** (e.g., $\tau = 0.10$ or lower), if the model is even $10\%$ suspicious that a patient has sepsis, it flags them for immediate clinical intervention. This trades lower Precision (more false alarms) for near-$100\%$ Recall, preventing patient mortality.
#### Why Distractors Are Wrong:
  * **A and B are incorrect:** Raising the threshold to $0.85$ requires the model to be $85\%$ certain before intervening. This minimizes false alarms (increasing Precision and Specificity), but causes False Negatives to skyrocket, resulting in fatal clinical misses.
  * **D is incorrect:** Accuracy treats all four confusion matrix cells as equal in cost, making it blind to asymmetric clinical risk.
  
  ---
## Master Checkpoint Summary: Section 1 Concepts
  
  ```
  ┌──────┬────────┬─────────────────────────────────────────────────────────────┐
  │ Item │ Answer │ Core Machine Learning Competency Evaluated                  │
  ├──────┼────────┼─────────────────────────────────────────────────────────────┤
  │ Q1   │ B      │ L2 Ridge regularization dampens polynomial parameter variance│
  │ Q2   │ B      │ .loc is label-based inclusive; .iloc is integer exclusive.  │
  │ Q3   │ A      │ NumPy broadcasting aligns from right; singletons stretch.   │
  │ Q4   │ B      │ Missing at Random (MAR) depends on observed covariates.     │
  │ Q5   │ C      │ MinMaxScaler transforms out-of-bounds inputs linearly (>1.0)│
  │ Q6   │ B      │ Gradient descent diverges if step size η ≥ 2 / λ_max(H).    │
  │ Q7   │ C      │ Trace of Hat Matrix H is rank(X) = d+1, NOT sample size n.  │
  │ Q8   │ A      │ Integer encoding on nominal features distorts k-NN metrics. │
  │ Q9   │ B      │ Unscaled features cause Ridge to over-penalize tiny scales. │
  │ Q10  │ C      │ Fatal False Negatives demand maximizing Recall via low τ.   │
  └──────┴────────┴─────────────────────────────────────────────────────────────┘
  ```
  
  ---