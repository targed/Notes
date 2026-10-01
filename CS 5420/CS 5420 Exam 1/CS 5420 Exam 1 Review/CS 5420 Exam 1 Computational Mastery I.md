## 1. Quick Review: Answers to Section 3 Checkpoints
  
  1. **Why Hyperparameter Tuning on the Test Set Causes Data Leakage:**
  * The test set’s sole scientific purpose is to serve as an unbiased, independent proxy for unobserved future data ($R_{\text{test}} \approx R(f)$).
  * If an engineer evaluates $k \in \{1, 3, \dots, 31\}$ on the test set and selects the $k$ that maximizes test accuracy, the test set is actively influencing model selection. 
  * This is **Meta-Overfitting (Data Snooping)**. The reported metric overfits the specific stochastic noise profile of that sample partition, losing its validity as an unbiased generalization benchmark. Hyperparameters must be tuned via cross-validation strictly on the training partition.
  2. **When PyTorch’s `DataLoader` Executes Pure Batch Gradient Descent:**
  * By definition, Batch Gradient Descent evaluates the full sample gradient over all $n$ training instances simultaneously before executing an optimizer step:
   $$\nabla J(w) = \frac{1}{n}\sum_{i=1}^n \nabla \mathcal{L}_i$$
  * In PyTorch, if you configure the data loader with:
   ```python
   DataLoader(train_dataset, batch_size=len(train_dataset), shuffle=False)
   ```
   the batch size equals the training set size ($B = n$). The loader yields a single batch containing all $n$ samples, executing exactly one parameter update per epoch.
  3. **Why Stochastic/Mini-Batch Gradient Descent Escapes Shallow Minima:**
  * Batch Gradient Descent computes the exact gradient $\nabla J(w)$. Near a saddle point or shallow local minimum, $\nabla J(w) \to \mathbf{0}$, causing deterministic updates to stall indefinitely.
  * Mini-Batch and Stochastic Gradient Descent introduce gradient sampling noise with covariance proportional to $\frac{\Sigma}{B}$.
  * This noise acts as an internal thermal perturbation (simulated annealing). It provides the kinetic energy required to bounce parameters out of unstable saddle points and shallow, high-variance local basins, allowing them to settle into broader, flatter minima with superior generalization properties.
  
  ---
# Part C: Computational Mastery I (Problems C3–C6)
  
  ---
## Problem C3: Confusion Matrix From a Description
  
  > **Problem Statement:**  
  > *500 emails, 120 of which are spam (positive). The filter flags 100 emails as spam; 90 of those are truly spam. Fill in TP, FP, FN, TN, then compute accuracy, precision, recall, specificity, and F1. (Round to 3 decimals).*
  
  ---
### Step 1: Dissect the Given Counts
  * **Total Population ($N_{\text{total}}$):** $500$
  * **Target Class Definition:** Positive ($Y = 1$) = **Spam**; Negative ($Y = 0$) = **Ham (Non-Spam)**
  * **Actual Positives ($P$):** $120$ actual spam emails
  * **Actual Negatives ($N$):** $500 - 120 = \mathbf{380}$ actual legitimate emails
  * **Total Predicted Positives ($\hat{P}$):** $100$ emails flagged as spam by the filter
  * **True Positives ($TP$):** $90$ flagged emails are truly spam
  
  ---
### Step 2: Calculate the Missing Cells of the $2 \times 2$ Contingency Matrix
  1. **False Positives ($FP$ / Type I Error):**
   Emails flagged as spam that are actually legitimate:
   $$FP = \text{Total Predicted Positives} - TP = 100 - 90 = \mathbf{10}$$
  2. **False Negatives ($FN$ / Type II Error):**
   Spam emails that the filter missed (sent to inbox):
   $$FN = \text{Actual Positives} - TP = 120 - 90 = \mathbf{30}$$
  3. **True Negatives ($TN$):**
   Legitimate emails correctly left in the inbox:
   $$TN = \text{Actual Negatives} - FP = 380 - 10 = \mathbf{370}$$
  
  ```
                         The Populated Confusion Matrix
                                       Predicted Class
                                 Spam (Pred = 1)   Ham (Pred = 0)     Row Totals
                            ┌──────────────────┬──────────────────┐
             Spam (True = 1)│     TP = 90      │     FN = 30      │ ──▶ P = 120
  Actual Class                ├──────────────────┼──────────────────┤
             Ham  (True = 0)│     FP = 10      │     TN = 370     │ ──▶ N = 380
                            └──────────────────┴──────────────────┘
             Column Totals:      P̂ = 100            N̂ = 400         Total = 500
  ```
  *Mathematical Sanity Check:* $90 + 10 + 30 + 370 = 500$. (Exact match).
  
  ---
### Step 3: Compute the Five Diagnostic Metrics
#### 1. Accuracy (Overall Correctness):
  $$\text{Accuracy} = \frac{TP + TN}{TP + FP + FN + TN} = \frac{90 + 370}{500} = \frac{460}{500} = \mathbf{0.920 \quad (92.0\%)}$$
#### 2. Precision / Positive Predictive Value (Filter Purity):
  $$\text{Precision} = \frac{TP}{TP + FP} = \frac{90}{90 + 10} = \frac{90}{100} = \mathbf{0.900 \quad (90.0\%)}$$
  *Interpretation:* When the filter flags an email as spam, it is correct $90\%$ of the time.
#### 3. Recall / Sensitivity / True Positive Rate (Detection Coverage):
  $$\text{Recall} = \frac{TP}{TP + FN} = \frac{90}{90 + 30} = \frac{90}{120} = \frac{3}{4} = \mathbf{0.750 \quad (75.0\%)}$$
  *Interpretation:* The filter catches $75\%$ of all incoming spam emails, missing $25\%$ ($FN = 30$).
#### 4. Specificity / True Negative Rate (In-Box Protection):
  $$\text{Specificity} = \frac{TN}{TN + FP} = \frac{370}{370 + 10} = \frac{370}{380} = \frac{37}{38} \approx \mathbf{0.974 \quad (97.4\%)}$$
  *Interpretation:* The filter correctly leaves $97.4\%$ of legitimate personal emails in the inbox.
#### 5. $F_1$-Score (Harmonic Mean of Precision and Recall):
  $$F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} = 2 \cdot \frac{0.900 \cdot 0.750}{0.900 + 0.750} = \frac{2 \cdot 0.675}{1.650} = \frac{1.350}{1.650} = \frac{9}{11} \approx \mathbf{0.818}$$
  
  ---
## Problem C4: Scaling by Hand
  
  > **Problem Statement:**  
  > *Training values of age: $20, 30, 40, 50, 60$. (StandardScaler uses the population standard deviation).*  
  > *(a) Compute the mean and standard deviation.*  
  > *(b) Standardize a test value of 55.*  
  > *(c) Min-max scale test values 55 and 70.*
  
  ---
### (a) Compute Mean ($\mu$) and Population Standard Deviation ($\sigma$)
#### 1. Sample Mean ($\mu$):
  $$\mu = \frac{1}{N} \sum_{i=1}^5 x_i = \frac{20 + 30 + 40 + 50 + 60}{5} = \frac{200}{5} = \mathbf{40.0}$$
#### 2. Population Variance ($\sigma^2$) and Standard Deviation ($\sigma$):
  Compute deviations from the mean:
  $$\{20 - 40, \; 30 - 40, \; 40 - 40, \; 50 - 40, \; 60 - 40\} = \{-20, \; -10, \; 0, \; +10, \; +20\}$$
  
  Square the deviations:
  $$\{(-20)^2, \; (-10)^2, \; (0)^2, \; (10)^2, \; (20)^2\} = \{400, \; 100, \; 0, \; 100, \; 400\}$$
  
  $$\text{Sum of Squared Deviations} = 400 + 100 + 0 + 100 + 400 = 1{,}000$$
  
  Because `StandardScaler` calculates the **population** standard deviation (dividing by $N$, not $N - 1$):
  $$\sigma^2 = \frac{1}{N} \sum_{i=1}^N (x_i - \mu)^2 = \frac{1{,}000}{5} = 200.0$$
  $$\sigma = \sqrt{200} = 10\sqrt{2} \approx \mathbf{14.142}$$
  
  * **Answers for (a):** $\mathbf{\mu = 40.0}$, $\mathbf{\sigma = 14.142}$
  
  ---
### (b) Standardize a Test Value of $55$
  Evaluate using the frozen training parameters ($\mu = 40.0, \sigma = 14.142$):
  
  $$z = \frac{x_{\text{test}} - \mu}{\sigma} = \frac{55 - 40}{\sqrt{200}} = \frac{15}{14.1421} = \frac{3}{2\sqrt{2}} = \frac{3\sqrt{2}}{4} \approx \mathbf{1.061}$$
  
  * **Answer for (b):** $\mathbf{z = 1.061}$
  
  ---
### (c) Min-Max Scale Test Values $55$ and $70$
  The training set boundaries are:
  $$x_{\min} = 20, \quad x_{\max} = 60 \implies x_{\max} - x_{\min} = 60 - 20 = 40$$
  
  The general MinMax transformation formula is:
  $$x' = \frac{x - x_{\min}}{x_{\max} - x_{\min}} = \frac{x - 20}{40}$$
#### 1. Transform $x_{\text{test}} = 55$:
  $$x'_{55} = \frac{55 - 20}{40} = \frac{35}{40} = \frac{7}{8} = \mathbf{0.875}$$
#### 2. Transform $x_{\text{test}} = 70$:
  $$x'_{70} = \frac{70 - 20}{40} = \frac{50}{40} = \frac{5}{4} = \mathbf{1.250}$$
  
  * **Answers for (c):** $\mathbf{x'_{55} = 0.875}$, $\mathbf{x'_{70} = 1.250}$  
  *(Note: $x'_{70} > 1.0$ because test value 70 exceeds the maximum value observed in the training split).*
  
  ---
## Problem C5: One-Hot Encoding
  
  > **Problem Statement:**  
  > *A training set has a nominal feature `color`:*  
  > *Row 1: red, Row 2: green, Row 3: blue, Row 4: green.*  
  > *(a) OneHotEncoder is fit on color in the training set. How many columns does it create? Write the encoded rows 1–4 (sklearn orders the categories alphabetically: blue, green, red).*  
  > *(b) A test row has color = purple. What does it become with OneHotEncoder(handle_unknown='ignore')? What happens with the default setting?*  
  > *(c) Why is encoding color as red = 0, green = 1, blue = 2 a bad idea for linear regression or k-NN?*
  
  ---
### (a) Column Count & Encoded Matrix
  * **Distinct Vocabulary in Training Set:** $\{\text{'blue', 'green', 'red'}\} \implies \mathbf{K = 3 \text{ columns}}$.
  * **Feature Ordering (Alphabetical):**
  * Column 0: `color_blue`
  * Column 1: `color_green`
  * Column 2: `color_red`
  
  ```
  ┌──────┬──────────┬────────────┬─────────────┬───────────┐
  │ Row  │ Raw Data │ color_blue │ color_green │ color_red │
  ├──────┼──────────┼────────────┼─────────────┼───────────┤
  │ 1    │ red      │     0      │      0      │     1     │
  │ 2    │ green    │     0      │      1      │     0     │
  │ 3    │ blue     │     1      │      0      │     0     │
  │ 4    │ green    │     0      │      1      │     0     │
  └──────┴──────────┴────────────┴─────────────┴───────────┘
  ```
  * Encoded rows 1–4 are:
  * Row 1: `[0, 0, 1]`
  * Row 2: `[0, 1, 0]`
  * Row 3: `[1, 0, 0]`
  * Row 4: `[0, 1, 0]`
  
  ---
### (b) Behavior on Unseen Category (`color = 'purple'`)
  1. **With `handle_unknown='ignore'`:**
   The encoder maps `'purple'` to an **all-zero vector**:
   $$\mathbf{[0, \; 0, \; 0]}$$
   It executes cleanly without throwing an exception, treating the novel observation as possessing zero activation across known training categories.
  2. **With Default Setting (`handle_unknown='error'`):**
   The encoder halts execution and raises a fatal exception:
   $$\mathbf{\texttt{ValueError: Found unknown categories ['purple'] in column 0 during transform.}}$$
  
  ---
### (c) Why Integer Encoding (`red=0, green=1, blue=2`) Fails
  1. **Failure in Linear Regression:**
   Linear regression evaluates an inner product: $\hat{y} = w \cdot x_{\text{color}} + b$. Integer encoding imposes:
   * **A false metric ordering:** $\text{blue} (2) > \text{green} (1) > \text{red} (0)$.
   * **A false equidistant interval:** It asserts that the difference between red and green ($\Delta = 1$) is identical to green and blue ($\Delta = 1$), and that $\text{blue} = 2 \times \text{green}$. The model cannot independently adjust weights for each color without orthogonal columns.
  2. **Failure in $k$-NN:**
   $k$-NN evaluates Euclidean distances: $D(x_A, x_B) = |x_{A} - x_{B}|$.
   $$D(\text{red}, \text{green}) = |0 - 1| = 1.0 \quad \text{vs.} \quad D(\text{red}, \text{blue}) = |0 - 2| = 2.0$$
   It falsely asserts that red is **twice as far** from blue as it is from green. Under One-Hot Encoding, every distinct color pair is equidistant:
   $$D(e_i, e_j) = \sqrt{(1 - 0)^2 + (0 - 1)^2} = \mathbf{\sqrt{2} \approx 1.414} \quad \forall \; i \neq j$$
  
  ---
## Problem C6: NumPy Shapes and Broadcasting
  
  > **Problem Statement:**  
  > *$X$ has shape `(4, 3)`. Give the result shape or write "error":*  
  > *(a) $X + \text{np.ones}(3)$*  
  > *(b) $X + \text{np.ones}(4)$*  
  > *(c) $\text{np.ones}((4, 1)) + \text{np.ones}((1, 5))$*  
  > *(d) $X - X.\text{mean}(\text{axis}=0)$*  
  > *(e) $X - X.\text{mean}(\text{axis}=1)$*
  
  ---
### Step-by-Step Broadcasting Rules:
  Under official NumPy semantics, two shapes are compatible if starting from trailing (rightmost) dimensions:
  1. The dimensions are equal, OR
  2. One of the dimensions is $1$.
  
  ---
#### (a) `X + np.ones(3)`
  * $X$ Shape: `(4, 3)`
  * `np.ones(3)` Shape: `(3,)` $\to$ right-aligned and prepended with $1$: `(1, 3)`
  * Alignment:
  $$\text{Dim 1 (Columns): } 3 == 3 \quad (\text{Compatible})$$
  $$\text{Dim 0 (Rows):    } 4 \text{ vs } 1 \implies \text{Stretches to } 4$$
  * **Result Shape:** **`(4, 3)`**
  
  ---
#### (b) `X + np.ones(4)`
  * $X$ Shape: `(4, 3)`
  * `np.ones(4)` Shape: `(4,)` $\to$ right-aligned and prepended with $1$: `(1, 4)`
  * Alignment:
  $$\text{Dim 1 (Columns): } 3 \text{ vs } 4 \quad (\mathbf{Incompatible!})$$
  Neither dimension is $1$, and $3 \neq 4$.
  * **Result:** **`"error"`** (`ValueError: operands could not be broadcast together with shapes (4,3) (4,)`)
  
  ---
#### (c) `np.ones((4, 1)) + np.ones((1, 5))`
  * Array 1 Shape: `(4, 1)`
  * Array 2 Shape: `(1, 5)`
  * Alignment:
  $$\text{Dim 1: } 1 \text{ vs } 5 \implies \text{Stretches to } 5$$
  $$\text{Dim 0: } 4 \text{ vs } 1 \implies \text{Stretches to } 4$$
  * Both dimensions broadcast across each other, forming a 2D matrix.
  * **Result Shape:** **`(4, 5)`**
  
  ---
#### (d) `X - X.mean(axis=0)`
  * By the Axis Principle: `axis=0` (rows) collapses, leaving the column length:
  $$X.\text{mean}(\text{axis}=0) \implies \text{Shape } \mathbf{(3,)}$$
  * Alignment:
  $$(4, 3) - (3,) \implies (4, 3) - (1, 3) \implies \mathbf{(4, 3)}$$
  * This computes **column-wise mean centering** (subtracting each feature's mean from its column).
  * **Result Shape:** **`(4, 3)`**
  
  ---
#### (e) `X - X.mean(axis=1)`
  * By the Axis Principle: `axis=1` (columns) collapses, leaving the row length:
  $$X.\text{mean}(\text{axis}=1) \implies \text{Shape } \mathbf{(4,)}$$
  * Alignment:
  $$(4, 3) - (4,) \implies (4, 3) - (1, 4) \implies \mathbf{Incompatible!}$$
  Trailing dimensions ($3$ vs $4$) fail to broadcast.
  * *(Note: To center rows, one must use `keepdims=True` to preserve shape `(4, 1)`).*
  * **Result:** **`"error"`** (`ValueError: operands could not be broadcast together with shapes (4,3) (4,)`)
  
  ---
## Summary Review Table: Problem C6 Output
  
  ```
  ┌──────┬─────────────────────────────────┬─────────────────┬──────────────────────────────────────────┐
  │ Item │ Operation                       │ Result Shape    │ NumPy Broadcasting Rationale             │
  ├──────┼─────────────────────────────────┼─────────────────┼──────────────────────────────────────────┤
  │ (a)  │ X + np.ones(3)                  │ (4, 3)          │ (4, 3) + (1, 3) ──▶ Broadcasts rows to 4 │
  │ (b)  │ X + np.ones(4)                  │ "error"         │ (4, 3) + (1, 4) ──▶ Trailing dim mismatch│
  │ (c)  │ np.ones((4,1)) + np.ones((1,5)) │ (4, 5)          │ (4, 1) + (1, 5) ──▶ Outer broadcast      │
  │ (d)  │ X - X.mean(axis=0)              │ (4, 3)          │ (4, 3) - (1, 3) ──▶ Valid column center  │
  │ (e)  │ X - X.mean(axis=1)              │ "error"         │ (4, 3) - (1, 4) ──▶ Trailing dim mismatch│
  └──────┴─────────────────────────────────┴─────────────────┴──────────────────────────────────────────┘
  ```
  
  ---