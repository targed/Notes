#### Q1. Model Diagnostics & The Generalization Gap (Mirror of P1)
  A linear regression baseline model evaluated on a complex housing dataset yields $54\%$ $R^2$ on the training set and $52\%$ $R^2$ on the held-out test set. Both errors are unacceptably high for production. This model is suffering from:
  * **A.** Overfitting
  * **B.** Underfitting
  * **C.** Target leakage
  * **D.** Severe variance explosion
  
  ---
#### Q2. Real-World Failure Modes & Taxonomy (Mirror of P2)
  Which of the following scenarios is an example of **Distribution Shift**, rather than Data Leakage or Metric Blindness?
  * **A.** A customer churn model includes `account_termination_reason` as an input predictor.
  * **B.** A chest X-ray classifier trained on scans from Hospital A (Siemens scanner) suffers a massive accuracy drop when deployed at Hospital B (GE scanner).
  * **C.** A credit card fraud model predicts "legitimate" for $100\%$ of transactions and reports $99.8\%$ accuracy.
  * **D.** A standard scaler is fit on the combined training and test sets prior to running cross-validation.
  
  ---
#### Q3. Jupyter/Colab Kernel Hidden State (Mirror of P3)
  In an interactive notebook, you execute the following two cells in order:
  * **Cell 1:** `x = 10`
  * **Cell 2:** `x += 5; print(x)`
  
  You then immediately re-run **Cell 2** two more times *without* re-running Cell 1. What is printed on the screen on the third execution of Cell 2?
  * **A.** `15`
  * **B.** `20`
  * **C.** `25`
  * **D.** `NameError`
  
  ---
#### Q4. Pandas Indexing Slicing Mechanics (Mirror of P4)
  A Pandas DataFrame `df` has the default integer index $0, 1, 2, 3, 4, 5, 6$. How many rows are returned by evaluating `df.iloc[1:5]`?
  * **A.** 5
  * **B.** 4
  * **C.** 6
  * **D.** 3
  
  ---
#### Q5. NumPy Tensor Broadcasting Rules (Mirror of P5)
  In NumPy, array `A` has shape `(6, 1, 4)` and array `B` has shape `(5, 1)`. What is the resulting shape of the arithmetic operation `A * B`?
  * **A.** `(6, 5, 4)`
  * **B.** `(6, 1, 4)`
  * **C.** `(30, 4)`
  * **D.** `ValueError: operands could not be broadcast together`
  
  ---
#### Q6. Missing Data Mechanisms & Rubin's Taxonomy (Mirror of P6)
  In an intensive care unit (ICU) clinical registry, severely ill patients suffering acute respiratory distress were too unstable to be moved to a scale, so their `body_weight` was not recorded. Wealthier, ambulatory patients in general recovery had their weight recorded reliably. 
  
  Under Donald Rubin’s taxonomy, missingness in the `body_weight` column is:
  * **A.** Missing Completely at Random (MCAR)
  * **B.** Missing at Random (MAR)
  * **C.** Missing Not at Random (MNAR)
  * **D.** Permutation Leakage
  
  ---
#### Q7. Scaler Extrapolation Beyond Training Bounds (Mirror of P7)
  A scikit-learn `MinMaxScaler()` is fit on training set values $[20, \; 40, \; 100]$. During inference, an unseen test observation arrives with feature value $x_{\text{test}} = 10$. Assuming default settings (no clipping), what numerical value is emitted by `scaler.transform()`?
  * **A.** `0.000`
  * **B.** `-0.125`
  * **C.` `0.100`
  * **D.** It raises an exception because $10$ is strictly below the minimum training value.
  
  ---
#### Q8. Nominal vs. Ordinal Categorical Representation (Mirror of P8)
  An employee dataset contains the feature `education_level` $\in \{\text{'High School', 'Bachelors', 'Masters', 'PhD'}\}$. An engineer maps this column to integers $0, 1, 2, 3$ and feeds it to an unregularized linear regression model. 
  
  Why is integer encoding justifiable here, whereas integer encoding `department` $\in \{\text{'HR', 'IT', 'Legal'}\}$ was identified as a major modeling error?
  * **A.** Linear regression can only process categorical features if they have at least four levels.
  * **B.** Education possesses a natural, monotonic physical hierarchy ($\text{High School} < \text{Bachelors} < \text{Masters} < \text{PhD}$).
  * **C.** Integer encoding ordinal features automatically prevents the Gram matrix $X^T X$ from becoming singular.
  * **D.** It converts the feature into orthogonal standard basis vectors.
  
  ---
#### Q9. Geometric Orthogonality of Ordinary Least Squares (Mirror of P9)
  In multiple linear regression fitted via Ordinary Least Squares, what is the geometric relationship between the model prediction vector $\hat{y} = Xw^*$ and the residual error vector $r = y - \hat{y}$?
  * **A.** They are parallel to each other.
  * **B.** They are strictly orthogonal ($\langle \hat{y}, \, r \rangle = 0$).
  * **C.** They lie in the exact same subspace $\text{Col}(X)$.
  * **D.** $\hat{y}$ has an identical Euclidean norm to $y$.
  
  ---
#### Q10. Asymmetric Clinical Costs & Metric Selection (Mirror of P12)
  An automated airport security checkpoint uses computer vision to screen passenger carry-on luggage for concealed firearms ($Y=1$). Missing a firearm allows a weapon into a terminal ($\text{Cost} = \text{Catastrophic}$), while falsely flagging a harmless laptop bag merely incurs a brief manual inspection by an agent ($\text{Cost} = \text{Minor inconvenience}$).
  
  The evaluation metric that **most directly penalizes missing a concealed firearm** is:
  * **A.** Precision
  * **B.** Specificity
  * **C.** Recall (Sensitivity)
  * **D.** False Positive Rate (FPR)
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for Part A
### Q1. Correct Answer: **B. Underfitting**
  * **Why:** Underfitting occurs when a model lacks capacity or the hypothesis class $\mathcal{H}$ is too restrictive (high bias). Both training error and test error are high, and the train–test gap is small ($\Delta = 54\% - 52\% = 2\%$). The model simply fails to capture the underlying structure.
  * **Why others fail:** 
  * Overfitting (A) requires low training error and high test error (a large gap, e.g., $99\%$ train vs. $71\%$ test). 
  * Target leakage (C) typically causes both train and test scores to be suspiciously high ($\approx 99\%$). `[L02, L11]`
  
  ---
### Q2. Correct Answer: **B. Chest X-ray scanner site differences**
  * **Why:** Distribution shift occurs when the underlying data-generating distribution changes between training and deployment ($P_{\text{train}}(X) \neq P_{\text{deploy}}(X)$). The biology of pneumonia is invariant, but scanner noise, contrast, and hospital acquisition artifacts shift between Siemens and GE machines.
  * **Why others fail:** 
  * A is **Target Leakage** (`account_termination_reason` is born after the churn event).
  * C is **Metric Blindness** (accuracy is uninformative on imbalanced data).
  * D is **Preprocessing Leakage** (fitting a scaler on test rows). `[L02]`
  
  ---
### Q3. Correct Answer: **C. 25**
  * **Why:** In Jupyter/Colab, the Python kernel maintains a single global memory heap (`globals()`). 
  * Run 1 (Cell 2): $x = 10 + 5 = 15$
  * Run 2 (Cell 2): $x = 15 + 5 = 20$
  * Run 3 (Cell 2): $x = 20 + 5 = 25$
  * **Why others fail:** Variables persist across cell runs; they do not reset to their original values unless the defining cell (`x = 10`) is explicitly re-evaluated or the runtime is restarted. `[L03, L04]`
  
  ---
### Q4. Correct Answer: **B. 4**
  * **Why:** `.iloc[]` uses standard Python integer positional slicing over the half-open interval $[start, stop)$, where the stop index is **strictly exclusive**. Slicing `1:5` extracts offsets $1, 2, 3, 4$, which is exactly $5 - 1 = \mathbf{4 \text{ rows}}$.
  * **Why others fail:** `.loc[1:5]` is label-based and inclusive of the endpoint, returning 5 rows. Slicing with `.iloc[1:5]` returns 4 rows. `[L05]`
  
  ---
### Q5. Correct Answer: **A. `(6, 5, 4)`**
  * **Why:** Compare shapes from right to left (trailing axes):
  $$\text{Array } A: \quad 6 \quad \times \quad 1 \quad \times \quad 4$$
  $$\text{Array } B: \quad \mathbf{1} \quad \times \quad 5 \quad \times \quad 1$$
  * Trailing axis: $4 \text{ vs } 1 \implies 4$
  * Middle axis: $1 \text{ vs } 5 \implies 5$
  * Leading axis: $6 \text{ vs } 1 \implies 6$
  * The dimensions stretch along the singletons to produce `(6, 5, 4)`.
  * **Why others fail:** Mismatched axes broadcast if at least one array has a dimension of $1$ on that axis. `[L05]`
  
  ---
### Q6. Correct Answer: **C. Missing Not at Random (MNAR)**
  * **Why:** Missingness is MNAR when the probability of a value being missing depends directly on the unobserved value itself (or the underlying severe health state that prevented measuring it). Because severely ill, unstable patients were the ones missing weight measurements, missingness cannot be eliminated simply by conditioning on observed features.
  * **Why others fail:** 
  * MCAR (A) requires missingness to be completely independent of all variables. 
  * MAR (B) requires missingness to be fully explained by observed variables (e.g., if missingness depended *only* on the recorded insurance type). `[L07]`
  
  ---
### Q7. Correct Answer: **B. -0.125**
  * **Why:** The training bounds are $x_{\min} = 20$ and $x_{\max} = 100$.
  $$\text{Range} = x_{\max} - x_{\min} = 100 - 20 = 80$$
  Transforming the test point:
  $$x'_{\text{test}} = \frac{x_{\text{test}} - x_{\min}}{x_{\max} - x_{\min}} = \frac{10 - 20}{80} = \frac{-10}{80} = \mathbf{-0.125}$$
  * **Why others fail:** `MinMaxScaler` does not clip values to $[0, 1]$ unless explicitly parameterized with `clip=True`. Emitting negative values or values $> 1.0$ on out-of-bounds test points is expected and is not an error. `[L08]`
  
  ---
### Q8. Correct Answer: **B. Education possesses a natural monotonic order**
  * **Why:** `education_level` is **ordinal**: higher values correspond to greater education attainment. A linear model can assign a single slope $w_j$ where an increase in rank reflects an increase in education level. 
  * **Why others fail:** Nominal categories (like `department`) have no natural order; encoding them as integers imposes arbitrary false spacing (e.g., asserting that $\text{Legal} = 3 \times \text{HR}$ or that Legal is "further" from HR than IT is). Nominal features require One-Hot Encoding. `[L08]`
  
  ---
### Q9. Correct Answer: **B. They are strictly orthogonal ($\langle \hat{y}, \, r \rangle = 0$)**
  * **Why:** Predictions $\hat{y} = Xw^*$ lie in the column space $\text{Col}(X)$. From the normal equations, the residual vector $r = y - \hat{y}$ is perpendicular to every column of $X$ ($X^T r = \mathbf{0}$). 
  Therefore:
  $$\langle \hat{y}, \, r \rangle = (Xw^*)^T r = (w^*)^T (X^T r) = (w^*)^T \mathbf{0} = \mathbf{0}$$
  * **Why others fail:** 
  * They are perpendicular, not parallel (A). 
  * Residuals lie in the orthogonal complement $\text{Col}(X)^\perp$, not in $\text{Col}(X)$ (C). `[L10, L12]`
  
  ---
### Q10. Correct Answer: **C. Recall (Sensitivity)**
  * **Why:** Missing a concealed weapon is a **False Negative (FN)** (predicting safe when a weapon is present). The metric that directly penalizes False Negatives in its denominator is:
  $$\text{Recall} = \frac{TP}{TP + \mathbf{FN}}$$
  Maximizing Recall drives False Negatives toward zero. Lowering the decision threshold $\tau$ below $0.50$ flags luggage even when the model has low suspicion, protecting safety.
  * **Why others fail:** 
  * Precision (A) penalizes **False Positives** ($\frac{TP}{TP + FP}$), which would prioritize reducing harmless manual bag checks at the expense of letting weapons slip through. `[L02, L13, L14]`
  
  ---