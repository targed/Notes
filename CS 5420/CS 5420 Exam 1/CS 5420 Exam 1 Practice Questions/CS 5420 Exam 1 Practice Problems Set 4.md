### Question 1: Contingency Matrix Construction & The Base-Rate Fallacy
  A diagnostic screening model is evaluated on a clinical cohort of $N = 1{,}000$ patients for a rare disease with a baseline prevalence of $10\%$ ($100$ positive cases, $900$ negative cases). 
  Laboratory evaluation reveals:
  * **Sensitivity (Recall):** $90.0\%$
  * **Specificity (True Negative Rate):** $95.0\%$
  
  **(a)** Reconstruct the complete $2 \times 2$ confusion matrix, finding the exact integer counts for $\text{True Positives (TP)}$, $\text{False Positives (FP)}$, $\text{False Negatives (FN)}$, and $\text{True Negatives (TN)}$.  
  **(b)** Calculate the **Precision (Positive Predictive Value)**.  
  **(c)** Calculate the **Balanced Accuracy** ($\frac{\text{Sensitivity} + \text{Specificity}}{2}$).  
  **(d)** Explain mathematically why the Precision ($\approx 66.7\%$) is substantially lower than both Sensitivity ($90\%$) and Specificity ($95\%$).
  
  ---
#### Worked Solution:
  * **(a) Reconstructing the Matrix:**
  1. *Actual Positives ($P = 100$):*
     $$TP = \text{Sensitivity} \times P = 0.90 \times 100 = \mathbf{90}$$
     $$FN = P - TP = 100 - 90 = \mathbf{10}$$
  2. *Actual Negatives ($N = 900$):*
     $$TN = \text{Specificity} \times N = 0.95 \times 900 = \mathbf{855}$$
     $$FP = N - TN = 900 - 855 = \mathbf{45}$$
  
  ```
                         Populated Clinical Matrix
                                       Predicted Class
                                 Positive (Pred = 1) Negative (Pred = 0)   Row Totals
                            ┌───────────────────────┬───────────────────┐
             Positive (Y = 1)│        TP = 90        │      FN = 10      │ ──▶ P = 100
  Actual Class                ├───────────────────────┼───────────────────┤
             Negative (Y = 0)│        FP = 45        │      TN = 855     │ ──▶ N = 900
                            └───────────────────────┴───────────────────┘
             Column Totals:          P̂ = 135                N̂ = 865       Total = 1,000
  ```
  
  * **(b) Compute Precision:**
  $$\text{Precision} = \frac{TP}{TP + FP} = \frac{90}{90 + 45} = \frac{90}{135} = \frac{2}{3} \approx \mathbf{0.667 \quad (66.7\%)}$$
  
  * **(c) Compute Balanced Accuracy:**
  $$\text{Balanced Accuracy} = \frac{\text{Sensitivity} + \text{Specificity}}{2} = \frac{0.900 + 0.950}{2} = \frac{1.850}{2} = \mathbf{0.925 \quad (92.5\%)}$$
  
  * **(d) The Base-Rate Fallacy Explanation:**
  * Even though the False Positive Rate is small ($\text{FPR} = 1 - 0.95 = 5\%$), it operates on a large population of healthy patients ($900$ individuals):
    $$FP = 0.05 \times 900 = 45 \text{ false alarms}$$
  * By contrast, the $90\%$ sensitivity operates on a small pool of infected patients ($100$ individuals), yielding $90$ true positives.
  * Because healthy individuals vastly outnumber infected individuals, the false positives ($45$) make up one-third of all positive flags.
  
  ---
### Question 2: $F_\beta$-Score & Harmonic Weighting Dynamics
  A financial fraud detection model produces the following performance metrics on a validation set:
  $$\text{Precision} = 0.800, \quad \text{Recall} = 0.400$$
  
  **(a)** Calculate the standard $F_1$-score.  
  **(b)** Calculate the $F_2$-score, where $\beta = 2$ weights Recall higher than Precision:
  $$F_\beta = (1 + \beta^2) \frac{\text{Precision} \cdot \text{Recall}}{\beta^2 \text{Precision} + \text{Recall}}$$  
  **(c)** Why is $F_2$ substantially lower than $F_1$ for this model?
  
  ---
#### Worked Solution:
  * **(a) Compute $F_1$-Score ($\beta = 1$):**
  $$F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} = 2 \cdot \frac{0.800 \times 0.400}{0.800 + 0.400} = 2 \cdot \frac{0.320}{1.200} = \frac{0.640}{1.200} = \frac{8}{15} \approx \mathbf{0.533}$$
  
  * **(b) Compute $F_2$-Score ($\beta = 2$):**
  $$F_2 = (1 + 2^2) \frac{0.800 \times 0.400}{(2^2 \times 0.800) + 0.400} = 5 \cdot \frac{0.320}{(4 \times 0.800) + 0.400}$$
  $$F_2 = 5 \cdot \frac{0.320}{3.200 + 0.400} = \frac{1.600}{3.600} = \frac{16}{36} = \frac{4}{9} \approx \mathbf{0.444}$$
  
  * **(c) Explaining the Asymmetry:**
  * The parameter $\beta$ determines the relative importance of Recall versus Precision: $\beta = 2$ places twice as much emphasis on Recall as on Precision.
  * Because the model has poor Recall ($0.400$) relative to Precision ($0.800$), increasing $\beta$ penalizes the model more heavily for missed fraudulent transactions ($FN$). 
  * The metric drops from $0.533 \to 0.444$, penalizing the low recall score.
  
  ---
### Question 3: Manual Standardization: Population vs. Sample Moments
  A continuous feature `dosage` has the following training values:
  $$X_{\text{train}} = [12, \; 18, \; 24, \; 30, \; 36]$$
  
  **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  **(b)** Explain why scikit-learn’s `StandardScaler` calculates the **population** standard deviation ($\text{ddof} = 0$) rather than the unbiased sample standard deviation ($\text{ddof} = 1$).  
  **(c)** Standardize a training observation of $18$.  
  **(d)** Standardize an unseen out-of-sample test observation of $45$.
  
  ---
#### Worked Solution:
  * **(a) Compute $\mu$ and Population $\sigma$:**
  1. *Mean:*
     $$\mu = \frac{12 + 18 + 24 + 30 + 36}{5} = \frac{120}{5} = \mathbf{24.000}$$
  2. *Deviations $(x_i - \mu)$:*
     $$\{-12, \; -6, \; 0, \; +6, \; +12\}$$
  3. *Squared Deviations:*
     $$\{144, \; 36, \; 0, \; 36, \; 144\} \implies \sum (x_i - \mu)^2 = 360$$
  4. *Population Standard Deviation ($\text{ddof} = 0$):*
     $$\sigma = \sqrt{\frac{1}{N}\sum_{i=1}^N (x_i - \mu)^2} = \sqrt{\frac{360}{5}} = \sqrt{72} = 6\sqrt{2} \approx \mathbf{8.485}$$
  
  * **(b) Why `StandardScaler` Uses Population Standard Deviation:**
  * In statistical inference, Bessel's correction ($\frac{1}{N-1}$) is used to obtain an unbiased estimate of the unobserved population variance.
  * In machine learning preprocessing, scaling is treated as a **deterministic transformation** of the empirical training matrix into an exact zero-mean, unit-variance dataset:
    $$\frac{1}{N}\sum z_i^2 \equiv 1.000$$
  * Using $N$ guarantees that the transformed training matrix has an empirical variance of exactly $1.0$.
  
  * **(c) Standardize Training Value $x = 18$:**
  $$z_{18} = \frac{x - \mu}{\sigma} = \frac{18 - 24}{\sqrt{72}} = \frac{-6}{6\sqrt{2}} = -\frac{1}{\sqrt{2}} = -\frac{\sqrt{2}}{2} \approx \mathbf{-0.707}$$
  
  * **(d) Standardize Test Value $x_{\text{test}} = 45$:**
  Apply the frozen training parameters ($\mu = 24.000, \sigma = 8.485$):
  $$z_{45} = \frac{45 - 24}{\sqrt{72}} = \frac{21}{6\sqrt{2}} = \frac{7}{2\sqrt{2}} = \frac{7\sqrt{2}}{4} \approx \mathbf{2.475}$$
  
  ---
### Question 4: `MinMaxScaler` with Non-Standard Intervals
  A training set feature has values:
  $$X_{\text{train}} = [5, \; 15, \; 25, \; 45]$$
  An engineer instantiates `MinMaxScaler(feature_range=(-1, 1))` to map data symmetrically into the compact interval $[-1, 1]$.
  
  **(a)** Write the general transformation formula mapping $x$ to $[a, b] = [-1, 1]$ using training bounds.  
  **(b)** Transform an in-sample test point $x = 25$.  
  **(c)** Transform an out-of-bounds test point $x = 55$.  
  **(d)** Transform an out-of-bounds test point $x = -5$.
  
  ---
#### Worked Solution:
  * **(a) Transformation Formula:**
  * Training parameters: $x_{\min} = 5, \; x_{\max} = 45 \implies x_{\max} - x_{\min} = 45 - 5 = 40$.
  * General mapping to range $[a, b]$:
    $$x' = a + \frac{x - x_{\min}}{x_{\max} - x_{\min}} (b - a)$$
  * Substituting $a = -1, b = +1, x_{\min} = 5, x_{\max} = 45$:
    $$x' = -1 + \frac{x - 5}{40} (1 - (-1)) = -1 + \frac{x - 5}{40} (2) = \mathbf{-1 + \frac{x - 5}{20}}$$
  
  * **(b) Transform $x = 25$:**
  $$x'_{25} = -1 + \frac{25 - 5}{20} = -1 + \frac{20}{20} = -1 + 1 = \mathbf{0.000}$$
  *(The midpoint of the training data maps directly to the midpoint of the target range).*
  
  * **(c) Transform Out-of-Bounds $x = 55$:**
  $$x'_{55} = -1 + \frac{55 - 5}{20} = -1 + \frac{50}{20} = -1 + 2.500 = \mathbf{+1.500}$$
  
  * **(d) Transform Out-of-Bounds $x = -5$:**
  $$x'_{-5} = -1 + \frac{-5 - 5}{20} = -1 + \frac{-10}{20} = -1 - 0.500 = \mathbf{-1.500}$$
  
  ---
### Question 5: Order-Statistic Scaling (`RobustScaler`) with Extreme Outliers
  A dataset contains severe outliers:
  $$X_{\text{train}} = [10, \; 14, \; 15, \; 18, \; 20, \; 22, \; 25, \; 29, \; 120]$$
  
  **(a)** Determine the sample median ($Q_2$), the first quartile ($Q_1$), the third quartile ($Q_3$), and the Interquartile Range ($\text{IQR}$).  
  **(b)** Write the specific `RobustScaler` equation fitted on this training data.  
  **(c)** Transform an inlier sample $x = 22$.  
  **(d)** Transform the extreme outlier $x = 120$, and compare its value to what `StandardScaler` would produce.
  
  ---
#### Worked Solution:
  * **(a) Calculate Order Statistics ($N = 9$):**
  1. *Median ($Q_2$):* The middle ($5\text{th}$) observation in the sorted array:
     $$\mathbf{Q_2 = \text{Median} = 20.000}$$
  2. *First Quartile ($Q_1$):* Median of lower half $[10, 14, 15, 18]$:
     $$\mathbf{Q_1 = \frac{14 + 15}{2} = 14.500}$$
  3. *Third Quartile ($Q_3$):* Median of upper half $[22, 25, 29, 120]$:
     $$\mathbf{Q_3 = \frac{25 + 29}{2} = 27.000}$$
  4. *Interquartile Range:*
     $$\mathbf{\text{IQR} = Q_3 - Q_1 = 27.000 - 14.500 = 12.500}$$
  
  * **(b) `RobustScaler` Formula:**
  $$x_{\text{robust}} = \frac{x - Q_2}{\text{IQR}} = \mathbf{\frac{x - 20.000}{12.500}}$$
  
  * **(c) Transform Inlier $x = 22$:**
  $$x_{\text{robust}}(22) = \frac{22 - 20}{12.5} = \frac{2}{12.5} = \mathbf{0.160}$$
  
  * **(d) Transform Outlier $x = 120$ & Contrast with `StandardScaler`:**
  * Under `RobustScaler`:
    $$x_{\text{robust}}(120) = \frac{120 - 20}{12.5} = \frac{100}{12.5} = \mathbf{8.000}$$
  * Under `StandardScaler`:
    $$\mu = \frac{273}{9} \approx 30.333, \quad \sigma \approx 32.327$$
    $$z(120) = \frac{120 - 30.333}{32.327} \approx \mathbf{2.774}$$
    Notice that the single outlier inflated the training mean ($\mu \approx 30.3$) well above $8$ of the $9$ actual data points, compressing the z-scores of the inliers. `RobustScaler` preserves the true median ($20.0$) and scales inliers cleanly.
  
  ---
### Question 6: Categorical Encoding Matrix Topologies
  A training partition contains an unordered nominal feature `browser`:
  ```
  Row 1: 'Chrome'
  Row 2: 'Safari'
  Row 3: 'Firefox'
  Row 4: 'Chrome'
  Row 5: 'Edge'
  ```
  
  **(a)** How many columns are instantiated by `OneHotEncoder()`? Write out the full encoded matrix for rows 1–5 in scikit-learn's standard alphabetical column order.  
  **(b)** A test instance arrives with `browser = 'Opera'`. What vector is emitted when the encoder is initialized with `handle_unknown='ignore'`?  
  **(c)** What is the exact sparsity percentage (fraction of zero entries) in the transformed $5$-row training design matrix?
  
  ---
#### Worked Solution:
  * **(a) Encoded Matrix Construction:**
  * Unique vocabulary: $\{\text{'Chrome', 'Edge', 'Firefox', 'Safari'}\} \implies \mathbf{K = 4 \text{ columns}}$.
  * Alphabetical column ordering:
    1. `browser_Chrome`
    2. `browser_Edge`
    3. `browser_Firefox`
    4. `browser_Safari`
  
  ```
  ┌──────┬──────────┬────────┬──────┬─────────┬────────┐
  │ Row  │ Raw Data │ Chrome │ Edge │ Firefox │ Safari │
  ├──────┼──────────┼────────┼──────┼─────────┼────────┤
  │ 1    │ Chrome   │   1    │  0   │    0    │   0    │
  │ 2    │ Safari   │   0    │  0   │    0    │   1    │
  │ 3    │ Firefox  │   0    │  0   │    1    │   0    │
  │ 4    │ Chrome   │   1    │  0   │    0    │   0    │
  │ 5    │ Edge     │   0    │  1   │    0    │   0    │
  └──────┴──────────┴────────┴──────┴─────────┴────────┘
  ```
  
  * **(b) Transformation of Novel Category (`'Opera'`):**
  Under `handle_unknown='ignore'`, unseen categories are mapped to an **all-zero vector**:
  $$\mathbf{[0, \; 0, \; 0, \; 0]}$$
  
  * **(c) Sparsity Calculation:**
  * Total scalar entries in matrix: $5 \text{ rows} \times 4 \text{ columns} = 20 \text{ entries}$.
  * Non-zero entries (ones): Exactly $1$ per row $= 5 \text{ entries}$.
  * Zero entries: $20 - 5 = 15 \text{ entries}$.
  $$\text{Sparsity} = \frac{15}{20} = \mathbf{0.750 \quad (75.0\% \text{ sparse})}$$
  
  ---
### Question 7: Advanced NumPy Broadcasting & Tensor Arithmetic
  Given arrays of varying ranks and dimensions, evaluate the resulting shape of each operation or write `"error"` if the operation violates broadcasting rules:
  
  **(a)** `np.ones((8, 1, 6, 1)) + np.zeros((7, 1, 5))`  
  **(b)** `np.ones((3, 4)) * np.zeros((3, 1))`  
  **(c)** `np.ones((10, 5)) - np.zeros((5,))`  
  **(d)** `np.ones((10, 5)) - np.zeros((10,))`  
  **(e)** `A @ B` where `A = np.ones((2, 3))` and `B = np.ones((3, 4))`
  
  ---
#### Worked Solution:
  * **(a) `(8, 1, 6, 1) + (7, 1, 5)`:**
  Align rightward, padding the shorter shape with a leading singleton ($1$):
  $$\text{Array 1: } \quad 8 \quad \times \quad 1 \quad \times \quad 6 \quad \times \quad 1$$
  $$\text{Array 2: } \quad \mathbf{1} \quad \times \quad 7 \quad \times \quad 1 \quad \times \quad 5$$
  * Axis 3: $1 \text{ vs } 5 \implies \text{Stretches to } 5$
  * Axis 2: $6 \text{ vs } 1 \implies \text{Stretches to } 6$
  * Axis 1: $1 \text{ vs } 7 \implies \text{Stretches to } 7$
  * Axis 0: $8 \text{ vs } 1 \implies \text{Stretches to } 8$
  * **Result Shape:** **`(8, 7, 6, 5)`**
  
  * **(b) `(3, 4) * (3, 1)`:**
  Trailing axis: $4 \text{ vs } 1 \implies 4$. Leading axis: $3 == 3$.
  * **Result Shape:** **`(3, 4)`**
  
  * **(c) `(10, 5) - (5,)`:**
  Right-align shape `(5,)` as `(1, 5)`. Trailing axis matches ($5 == 5$). Leading axis broadcasts ($10 \text{ vs } 1 \implies 10$).
  * **Result Shape:** **`(10, 5)`**
  
  * **(d) `(10, 5) - (10,)`:**
  Right-align shape `(10,)` as `(1, 10)`. Compare trailing axes: **$5 \text{ vs } 10$**. Neither dimension is $1$, and $5 \neq 10$. Broadcasting fails.
  * **Result:** **`"error"`** (`ValueError`)
  
  * **(e) `A @ B` with `(2, 3)` and `(3, 4)`:**
  This is matrix multiplication, not element-wise broadcasting. Inner dimensions match ($3 == 3$). Output dimensions take outer bounds: $(2, 4)$.
  * **Result Shape:** **`(2, 4)`**
  
  ---
### Question 8: Row-Wise vs. Column-Wise Centering & The `keepdims` Trap
  A data matrix $M$ has shape `(5, 4)` (5 samples, 4 features).
  
  **(a)** Why does column-centering `M - M.mean(axis=0)` execute cleanly, while row-centering `M - M.mean(axis=1)` raises a `ValueError`?  
  **(b)** Write two syntactically valid NumPy expressions that perform row-centering without Python loops.
  
  ---
#### Worked Solution:
  * **(a) The Alignment Asymmetry:**
  1. *Column Centering:* `M.mean(axis=0)` collapses rows, producing a 1D vector of shape **`(4,)`**.
     When computing `(5, 4) - (4,)`, NumPy right-aligns the shapes:
     $$(5, 4) \quad \text{and} \quad (1, 4)$$
     The trailing dimension matches ($4 == 4$), so it broadcasts across rows cleanly.
  2. *Row Centering:* `M.mean(axis=1)` collapses columns, producing a 1D vector of shape **`(5,)`**.
     When computing `(5, 4) - (5,)`, NumPy right-aligns the shapes:
     $$\mathbf{(5, 4) \quad \text{and} \quad (1, 5)}$$
     The trailing dimensions ($4 \text{ vs } 5$) fail to broadcast, raising a `ValueError`.
  
  * **(b) Two Valid NumPy Formulations for Row-Centering:**
  * **Solution 1 (Using `keepdims=True`):**
    ```python
    M_centered = M - M.mean(axis=1, keepdims=True)  # Shape: (5, 4) - (5, 1)
    ```
  * **Solution 2 (Using `np.newaxis` or `None` Slicing):**
    ```python
    M_centered = M - M.mean(axis=1)[:, np.newaxis]  # Shape: (5, 4) - (5, 1)
    ```
  
  ---
### Question 9: Divergence of ROC-AUC vs. PR-AUC Under Extreme Imbalance
  A financial transaction monitoring system processes $N = 10{,}000$ wire transfers. Exactly $100$ transactions are money-laundering schemes ($Y = 1$), and $9{,}900$ are legitimate ($Y = 0$). 
  A binary classifier outputs predictions yielding:
  $$TP = 80, \quad FN = 20, \quad FP = 198, \quad TN = 9{,}702$$
  
  **(a)** Compute the **True Positive Rate (TPR / Recall)**.  
  **(b)** Compute the **False Positive Rate (FPR)**.  
  **(c)** Compute the **Precision (PPV)**.  
  **(d)** Explain why the model appears highly performant on an ROC curve ($\text{TPR} = 0.80, \text{FPR} = 0.02$) while exposing poor performance on a Precision-Recall curve.
  
  ---
#### Worked Solution:
  * **(a) True Positive Rate (TPR):**
  $$\text{TPR} = \frac{TP}{TP + FN} = \frac{80}{80 + 20} = \frac{80}{100} = \mathbf{0.800 \quad (80.0\%)}$$
  
  * **(b) False Positive Rate (FPR):**
  $$\text{FPR} = \frac{FP}{TN + FP} = \frac{198}{9{,}702 + 198} = \frac{198}{9{,}900} = \mathbf{0.020 \quad (2.0\%)}$$
  
  * **(c) Precision:**
  $$\text{Precision} = \frac{TP}{TP + FP} = \frac{80}{80 + 198} = \frac{80}{278} \approx \mathbf{0.288 \quad (28.8\%)}$$
  
  * **(d) Why ROC Masks the Performance Deficit:**
  * On an ROC curve ($Y = \text{TPR}, X = \text{FPR}$), the coordinate is $(0.020, 0.800)$, yielding an impressive ROC-AUC ($> 0.95$). 
  * The denominator of the False Positive Rate is the total number of negative samples ($N = 9{,}900$). Because $N$ is massive, $198$ false alarms appear negligible ($\text{FPR} = 2.0\%$).
  * The Precision-Recall curve explicitly evaluates False Positives against True Positives:
    $$\text{Precision} = \frac{80}{80 + 198} = 28.8\%$$
  * In production, **over $71\%$ of all flagged transactions are false alarms** ($198 / 278$). PR-AUC exposes the operational cost, whereas ROC-AUC masks it.
  
  ---
### Question 10: Multi-Metric Diagnostic Evaluation & Negative Predictive Value
  A rapid point-of-care infectious disease test is administered to $N = 2{,}000$ hospital personnel. 
  The observed contingency matrix is:
  * $TP = 180$
  * $FN = 20$
  * $FP = 60$
  * $TN = 1{,}740$
  
  **(a)** Compute **Accuracy**.  
  **(b)** Compute **Precision**.  
  **(c)** Compute **Recall (Sensitivity)**.  
  **(d)** Compute **Negative Predictive Value (NPV)**: $\frac{TN}{TN + FN}$.  
  **(e)** Compute the **$F_1$-score**.  
  **(f)** What is the clinical meaning of the Negative Predictive Value in this screening program?
  
  ---
#### Worked Solution:
  * **(a) Accuracy:**
  $$\text{Accuracy} = \frac{180 + 1{,}740}{2{,}000} = \frac{1{,}920}{2{,}000} = \mathbf{0.960 \quad (96.0\%)}$$
  
  * **(b) Precision:**
  $$\text{Precision} = \frac{180}{180 + 60} = \frac{180}{240} = \mathbf{0.750 \quad (75.0\%)}$$
  
  * **(c) Recall (Sensitivity):**
  $$\text{Recall} = \frac{180}{180 + 20} = \frac{180}{200} = \mathbf{0.900 \quad (90.0\%)}$$
  
  * **(d) Negative Predictive Value (NPV):**
  $$\text{NPV} = \frac{TN}{TN + FN} = \frac{1{,}740}{1{,}740 + 20} = \frac{1{,}740}{1{,}760} \approx \mathbf{0.989 \quad (98.9\%)}$$
  
  * **(e) $F_1$-Score:**
  $$F_1 = 2 \cdot \frac{0.750 \times 0.900}{0.750 + 0.900} = 2 \cdot \frac{0.675}{1.650} = \frac{1.350}{1.650} = \frac{9}{11} \approx \mathbf{0.818}$$
  
  * **(f) Clinical Interpretation of NPV:**
  * An $\text{NPV} = 98.9\%$ means that if a healthcare worker tests **negative**, there is a **$98.9\%$ probability that they are genuinely uninfected**. 
  * Only $1.1\%$ of individuals cleared to work carry an active undetected infection ($FN = 20$), which is a critical safety metric for hospital infection control.
  
  ---
## Master Checkpoint Summary: Section 4 Formulas
  
  ```
  ┌────────────────────┬──────────────────────────────────────┬──────────────────────────────────────────┐
  │ Competency         │ Mathematical Formula                 │ Core Exam Verification Rule              │
  ├────────────────────┼──────────────────────────────────────┼──────────────────────────────────────────┤
  │ Accuracy           │ (TP + TN) / (P + N)                  │ Highly misleading under class imbalance. │
  │ Precision          │ TP / (TP + FP)                       │ Quantifies positive predictive purity.   │
  │ Recall             │ TP / (TP + FN)                       │ Quantifies detection sensitivity (TPR).  │
  │ Specificity        │ TN / (TN + FP)                       │ Quantifies inlier protection (TNR).      │
  │ F1-Score           │ 2 · (Prec · Rec) / (Prec + Rec)      │ Harmonic mean; penalizes extreme skew.   │
  │ StandardScaler     │ z = (x - μ) / σ                      │ Uses population σ (ddof = 0) in sklearn. │
  │ MinMaxScaler       │ x' = a + (x - x_min)/(range) · (b - a)│ Linearly evaluates out-of-bounds inputs. │
  │ RobustScaler       │ x_robust = (x - Q2) / IQR            │ High breakdown point; outlier resistant. │
  │ Broadcasting       │ Align right; match if equal or 1     │ Prepend singleton 1s to shorter ranks.   │
  │ Row Centering      │ M - M.mean(axis=1, keepdims=True)    │ keepdims=True prevents broadcast error.  │
  └────────────────────┴──────────────────────────────────────┴──────────────────────────────────────────┘
  ```
  
  ---