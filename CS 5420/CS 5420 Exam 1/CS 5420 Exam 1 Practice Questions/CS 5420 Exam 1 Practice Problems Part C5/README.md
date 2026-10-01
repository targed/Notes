# Part C5 — One-Hot Encoding Practice Problems
#### Problem 1 (Alphabetical Category Ordering & Binary Rows)
  A training set has a nominal categorical feature `animal`:
  ```
  Row 1: Dog
  Row 2: Cat
  Row 3: Bird
  Row 4: Dog
  Row 5: Cat
  ```
  * **(a)** `OneHotEncoder` is fit on `animal`. How many columns does it create?
  * **(b)** List the column names in scikit-learn's standard alphabetical order, and write out the encoded binary vectors for Rows 1–5.
  * **(c)** A test instance arrives with `animal = 'Hamster'`. What vector is emitted when the encoder is initialized with `handle_unknown='ignore'`? What happens under the default setting?
  
  ---
#### Problem 2 (Multi-Feature Column Expansion)
  A customer transaction dataset contains two nominal categorical features:
  * `device` $\in \{\text{'Mobile', 'Desktop'}\}$
  * `payment` $\in \{\text{'PayPal', 'Card', 'Cash'}\}$
  
  A training subset consists of three rows:
  ```
  Row 1: (Mobile, Card)
  Row 2: (Desktop, PayPal)
  Row 3: (Mobile, Cash)
  ```
  * **(a)** How many total columns are created when `OneHotEncoder()` is fit on both features simultaneously? Write the output feature names in order.
  * **(b)** Write the complete transformed $3 \times K$ binary matrix for Rows 1–3.
  * **(c)** A test transaction arrives: `(Tablet, Crypto)`. What does its transformed row vector look like under `handle_unknown='ignore'`?
  
  ---
#### Problem 3 (The Dummy Variable Trap in OLS)
  A dataset contains a nominal categorical feature `city` with categories: $\{\text{'Austin', 'Boston', 'Chicago'}\}$.
  * **(a)** Explain mathematically why feeding all three one-hot encoded columns into an Ordinary Least Squares (OLS) linear model with an intercept ($w_0$) causes the normal equations $(X^T X)w = X^T y$ to fail.
  * **(b)** What specific parameter in scikit-learn’s `OneHotEncoder` eliminates this linear algebra defect for unregularized linear regression?
  * **(c)** Write out the encoded rows for Austin, Boston, and Chicago after applying the fix identified in (b).
  * **(d)** Why is this column-dropping fix **unnecessary** (and often discouraged) when fitting a regularized Ridge regression ($L_2$) or a tree-based model?
  
  ---
#### Problem 4 (Metric Space Geometry: Integer vs. One-Hot in $k$-NN)
  Consider three nominal categories: $A$, $B$, and $C$.
  * **(a)** An engineer label-encodes them as integers: $A = 0, \; B = 1, \; C = 2$. Compute the pairwise Euclidean distances $d(A, B)$, $d(B, C)$, and $d(A, C)$.
  * **(b)** The engineer switches to standard One-Hot Encoding in $\mathbb{R}^3$:
  $$A \to [1, 0, 0]^T, \quad B \to [0, 1, 0]^T, \quad C \to [0, 0, 1]^T$$
  Compute the pairwise Euclidean distances $d(A, B)$, $d(B, C)$, and $d(A, C)$ in this transformed space.
  * **(c)** What geometric structure do the three one-hot encoded vectors form in $\mathbb{R}^3$, and why does this solve the inductive bias flaw of the integer encoding for $k$-NN?
  
  ---
#### Problem 5 (Binary Feature Efficiency: `drop='if_binary'`)
  A feature `subscription_type` has two levels: `['Monthly', 'Annual', 'Monthly', 'Annual']`.
  * **(a)** By default, how many columns does `OneHotEncoder()` create for this feature?
  * **(b)** If configured with `drop='if_binary'`, how many columns are created, and what do the encoded rows look like (assuming alphabetical ordering)?
  * **(c)** From the perspective of model interpretability in Logistic Regression, why is a single binary column preferred over two separate columns?
  
  ---
#### Problem 6 (Sparsity & Memory Calculations in High Dimensions)
  An e-commerce dataset contains $100{,}000$ customer transactions. An engineer encodes three nominal categorical features:
  * `department`: 40 distinct categories
  * `merchant_category`: 150 distinct categories
  * `us_state`: 50 distinct categories
  * **(a)** What is the total column dimension ($d$) of the transformed matrix?
  * **(b)** How many non-zero entries (ones) exist in every single row of the transformed matrix?
  * **(c)** Compute the exact **sparsity percentage** (fraction of zero entries) of the resulting $100{,}000 \times d$ design matrix.
  * **(d)** Why does scikit-learn return a `scipy.sparse.csr_matrix` by default rather than a dense NumPy array?
  
  ---
#### Problem 7 (Logistic Regression Probability Under Unseen Categories)
  A binary logistic regression model predicting customer purchase ($Y = 1$) is trained on a one-hot encoded categorical feature `referral_source` with training categories $\{\text{'Search', 'Social', 'Email'}\}$:
  $$P(Y = 1 \mid x) = \sigma(z) = \frac{1}{1 + e^{-z}}$$
  $$\text{where } z = 1.2 \cdot x_{\text{Email}} + 0.5 \cdot x_{\text{Search}} - 0.9 \cdot x_{\text{Social}} - 0.4$$
  The encoder was configured with `handle_unknown='ignore'`.
  
  A prospective customer arrives via a completely novel referral source: `referral_source = 'Affiliate'`.
  * **(a)** What vector is emitted for the one-hot encoded portion of this customer's feature representation?
  * **(b)** What is the value of the linear logit score $z$ for this customer?
  * **(c)** Compute the predicted probability of purchase $P(Y = 1 \mid \text{'Affiliate'})$. (Round to 3 decimal places).
  
  ---
#### Problem 8 (`ColumnTransformer` Integration & Dimension Tracking)
  An insurance pricing dataset has three raw features:
  * `driver_age` (continuous)
  * `annual_mileage` (continuous)
  * `vehicle_type` (nominal: Sedan, SUV, Truck, Coupe)
  
  An engineer defines the following preprocessor:
  ```python
  preprocessor = ColumnTransformer(transformers=[
    ('num', StandardScaler(), ['driver_age', 'annual_mileage']),
    ('cat', OneHotEncoder(handle_unknown='ignore'), ['vehicle_type'])
  ])
  ```
  * **(a)** What is the total column count of the design matrix output by `preprocessor.fit_transform(X_train)`?
  * **(b)** List the exact feature names returned by `preprocessor.get_feature_names_out()`.
  * **(c)** If an unobserved test record has `vehicle_type = 'Motorcycle'`, write out the full numerical row vector emitted for that test instance if their age and mileage both equal their training set means.
  
  ---
#### Problem 9 (Decision Trees vs. Linear Models on Integer-Encoded Categories)
  An engineer evaluates a customer rating feature: `status` $\in \{\text{'Bronze', 'Silver', 'Gold'}\}$.
  The target variable is continuous customer spending ($Y$), where:
  $$\text{Average Spend: Bronze } = \$100, \quad \text{Silver } = \$500, \quad \text{Gold } = \$600$$
  The engineer label-encodes the categories as: $\text{Bronze} = 0, \; \text{Silver} = 1, \; \text{Gold} = 2$.
  * **(a)** If a **Linear Regression** model $\hat{y} = w_1 x_{\text{status}} + w_0$ is fit, explain why the model cannot fit the step changes between the tiers accurately.
  * **(b)** If a **Decision Tree Regressor** (depth $\ge 2$) is fit on this single integer-encoded column, can it fit the exact group means ($\$100, \$500, \$600$)? Explain why.
  
  ---
#### Problem 10 (Missing Category Hygiene in Pipelines)
  A training column `color` contains missing values:
  ```
  X_train = [['red'], [None], ['blue'], ['red']]
  X_test  = [['green'], ['blue']]
  ```
  * **(a)** If an engineer passes `X_train` directly to `OneHotEncoder()`, what happens under modern scikit-learn versions?
  * **(b)** The engineer builds a leak-free pipeline:
  ```python
  pipe = make_pipeline(
      SimpleImputer(strategy='most_frequent'),
      OneHotEncoder(handle_unknown='ignore')
  )
  ```
  Trace what happens to the training row containing `None` and the test row containing `'green'`. Write their final encoded representations.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C5
### Problem 1 Solution
  * **(a) Column Count:**
  Unique training categories: $\{\text{'Bird', 'Cat', 'Dog'}\} \implies \mathbf{3 \text{ columns}}$.
  * **(b) Alphabetical Order & Encoded Rows:**
  Alphabetical column ordering:
  1. `animal_Bird`
  2. `animal_Cat`
  3. `animal_Dog`
  
  ```
  ┌──────┬──────────┬─────────────┬────────────┬────────────┐
  │ Row  │ Raw Data │ animal_Bird │ animal_Cat │ animal_Dog │
  ├──────┼──────────┼─────────────┼────────────┼────────────┤
  │ 1    │ Dog      │      0      │     0      │     1      │
  │ 2    │ Cat      │      0      │     1      │     0      │
  │ 3    │ Bird     │      1      │     0      │     0      │
  │ 4    │ Dog      │      0      │     0      │     1      │
  │ 5    │ Cat      │      0      │     1      │     0      │
  └──────┴──────────┴─────────────┴────────────┴────────────┘
  ```
  * Row 1: `[0, 0, 1]`
  * Row 2: `[0, 1, 0]`
  * Row 3: `[1, 0, 0]`
  * Row 4: `[0, 0, 1]`
  * Row 5: `[0, 1, 0]`
  
  * **(c) Unseen Test Category (`'Hamster'`):**
  * Under `handle_unknown='ignore'`: Emits an **all-zero vector**:
    $$\mathbf{[0, \; 0, \; 0]}$$
  * Under default `handle_unknown='error'`: Raises a **`ValueError`**:
    $$\texttt{ValueError: Found unknown categories ['Hamster'] in column 0 during transform.}$$
    This halts execution and crashes the production pipeline. `[L08, Review C5]`
  
  ---
### Problem 2 Solution
  * **(a) Total Columns & Feature Names:**
  * `device` has 2 levels: `'Desktop'`, `'Mobile'` $\implies 2$ columns.
  * `payment` has 3 levels: `'Card'`, `'Cash'`, `'PayPal'` $\implies 3$ columns.
  * Total Columns: $2 + 3 = \mathbf{5 \text{ columns}}$.
  * **Alphabetical Column Names:**
    1. `device_Desktop`
    2. `device_Mobile`
    3. `payment_Card`
    4. `payment_Cash`
    5. `payment_PayPal`
  * **(b) Encoded Matrix for Rows 1–3:**
  * Row 1 `(Mobile, Card)`:
    $$\mathbf{[0, \; 1, \; 1, \; 0, \; 0]}$$
  * Row 2 `(Desktop, PayPal)`:
    $$\mathbf{[1, \; 0, \; 0, \; 0, \; 1]}$$
  * Row 3 `(Mobile, Cash)`:
    $$\mathbf{[0, \; 1, \; 0, \; 1, \; 0]}$$
  * **(c) Unseen Category Pair `(Tablet, Crypto)` under `'ignore'`:**
  Both `'Tablet'` and `'Crypto'` are novel categories. Both sub-blocks emit all zeros:
  $$\mathbf{[0, \; 0, \; 0, \; 0, \; 0]}$$
  The instance receives zero active coefficients from both categorical features. `[L08]`
  
  ---
### Problem 3 Solution
  * **(a) The Mathematical Defect (Dummy Variable Trap):**
  For any observation $i$, the sum across all three one-hot columns equals 1:
  $$x_{i, \text{Austin}} + x_{i, \text{Boston}} + x_{i, \text{Chicago}} = 1$$
  When an intercept term $w_0$ is included, the design matrix contains an explicit column of ones ($\mathbf{1}$). Thus:
  $$\mathbf{x}_{\text{Austin}} + \mathbf{x}_{\text{Boston}} + \mathbf{x}_{\text{Chicago}} = \mathbf{1}$$
  The columns of $X$ are **linearly dependent** ($\text{rank}(X) < d + 1$). The Gram matrix $X^T X$ is singular ($\det(X^T X) = 0$) and cannot be inverted to compute $(X^T X)^{-1} X^T y$. `[L08, L10]`
  * **(b) Scikit-Learn Fix:**
  Set **`drop='first'`** in `OneHotEncoder(drop='first')`.
  * **(c) Encoded Rows under `drop='first'`:**
  Alphabetical categories: Austin, Boston, Chicago. The first category (**Austin**) is dropped as the reference baseline ($K - 1 = 2$ columns remaining: `city_Boston`, `city_Chicago`):
  * **Austin** (Reference class): $\mathbf{[0, \; 0]}$
  * **Boston**: $\mathbf{[1, \; 0]}$
  * **Chicago**: $\mathbf{[0, \; 1]}$
  * **(d) Why `drop='first'` Is Unnecessary for Ridge/Trees:**
  * **Ridge Regression ($L_2$):** Solves $(X^T X + \alpha I)^{-1} X^T y$. The addition of $\alpha I$ shifts all eigenvalues strictly positive ($\lambda_i + \alpha > 0$), making the matrix strictly invertible regardless of collinearity. Dropping a column also introduces asymmetric shrinkage on the dropped category.
  * **Decision Trees:** Trees evaluate features one column at a time via orthogonal splits ($x_j \le 0.5$). They do not invert matrices and are unaffected by multicollinearity. `[L08, L11]`
  
  ---
### Problem 4 Solution
  * **(a) Pairwise Distances under Integer Encoding ($A=0, B=1, C=2$):**
  $$d(A, B) = |0 - 1| = \mathbf{1.0}$$
  $$d(B, C) = |1 - 2| = \mathbf{1.0}$$
  $$d(A, C) = |0 - 2| = \mathbf{2.0}$$
  * **(b) Pairwise Distances under One-Hot Encoding:**
  $$d(A, B) = \sqrt{(1 - 0)^2 + (0 - 1)^2 + (0 - 0)^2} = \sqrt{1 + 1 + 0} = \mathbf{\sqrt{2} \approx 1.414}$$
  $$d(B, C) = \sqrt{(0 - 0)^2 + (1 - 0)^2 + (0 - 1)^2} = \sqrt{0 + 1 + 1} = \mathbf{\sqrt{2} \approx 1.414}$$
  $$d(A, C) = \sqrt{(1 - 0)^2 + (0 - 0)^2 + (0 - 1)^2} = \sqrt{1 + 0 + 1} = \mathbf{\sqrt{2} \approx 1.414}$$
  * **(c) Geometric Structure & Resolution of Inductive Bias:**
  * In $\mathbb{R}^3$, the three one-hot basis vectors form an **equilateral triangle (a regular 2-simplex)** centered equidistant from each other on the unit simplex.
  * This resolves the flaw of integer encoding because it eliminates false spatial proximity: $A$ is no longer closer to $B$ than to $C$. Every nominal category is **identically equidistant ($\sqrt{2}$)** from every other category, removing arbitrary distance distortion in $k$-NN. `[L08, L14, Review C5]`
  
  ---
### Problem 5 Solution
  * **(a) Default Column Count:**
  By default, `OneHotEncoder()` creates one column per category $\implies \mathbf{2 \text{ columns}}$ (`subscription_Annual`, `subscription_Monthly`).
  * **(b) Output under `drop='if_binary'`:**
  Creates exactly **$1$ column** ($K - 1 = 1$).
  Alphabetical ordering: `'Annual'` is dropped as reference (0), `'Monthly'` is retained (1):
  * Row 1 (`'Monthly'`): $\mathbf{[1]}$
  * Row 2 (`'Annual'`): $\mathbf{[0]}$
  * Row 3 (`'Monthly'`): $\mathbf{[1]}$
  * Row 4 (`'Annual'`): $\mathbf{[0]}$
  * **(c) Interpretability Advantage:**
  In linear models, two columns create two redundant coefficients ($w_{\text{annual}} = -w_{\text{monthly}}$). A single binary column produces one clean parameter weight ($w_{\text{monthly}}$) representing the log-odds (or change in $y$) of being on a Monthly plan relative to an Annual plan. `[L08, L13]`
  
  ---
### Problem 6 Solution
  * **(a) Total Column Dimension ($d$):**
  $$d = 40 + 150 + 50 = \mathbf{240 \text{ columns}}$$
  * **(b) Non-Zero Entries per Row:**
  Each row contains exactly one active category per feature:
  $$\text{Non-zero entries per row} = 1 + 1 + 1 = \mathbf{3 \text{ non-zero entries (ones)}}$$
  * **(c) Sparsity Percentage:**
  * Total elements in matrix: $100{,}000 \times 240 = 24{,}000{,}000 \text{ elements}$.
  * Total non-zero elements: $100{,}000 \times 3 = 300{,}000 \text{ ones}$.
  * Total zero elements: $24{,}000{,}000 - 300{,}000 = 23{,}700{,}000 \text{ zeros}$.
  $$\text{Sparsity} = \frac{23{,}700{,}000}{24{,}000{,}000} = \mathbf{0.9875 \quad (98.75\% \text{ sparse})}$$
  * **(d) Why `sparse_output=True`:**
  A dense array of $24$ million `float64` elements consumes $\approx 192\text{ MB}$ of memory consisting almost entirely of redundant zeros. Compressed Sparse Row (CSR) format stores only the indices and values of the non-zero entries ($300{,}000$ values $\approx 2.4\text{ MB}$), saving $>98\%$ of RAM and accelerating sparse matrix-vector multiplication. `[L05, L08]`
  
  ---
### Problem 7 Solution
  * **(a) Emitted Vector for `'Affiliate'`:**
  Because `'Affiliate'` was unobserved during training and `handle_unknown='ignore'`, all three training category columns are set to zero:
  $$\mathbf{x_{\text{onehot}} = [x_{\text{Email}} = 0, \; x_{\text{Search}} = 0, \; x_{\text{Social}} = 0]}$$
  * **(b) Linear Score ($z$):**
  Substitute zeros into the logit equation:
  $$z = 1.2(0) + 0.5(0) - 0.9(0) - 0.4 = \mathbf{-0.400}$$
  *(For any unseen category, the score reverts cleanly to the base intercept $b$).*
  * **(c) Predicted Probability:**
  $$P(Y = 1 \mid \text{'Affiliate'}) = \sigma(-0.4) = \frac{1}{1 + e^{-(-0.4)}} = \frac{1}{1 + e^{0.4}} \approx \frac{1}{1 + 1.4918} = \frac{1}{2.4918} \approx \mathbf{0.401 \quad (40.1\%)}$$ `[L08, L13]`
  
  ---
### Problem 8 Solution
  * **(a) Total Output Columns:**
  * Continuous features (`'driver_age'`, `'annual_mileage'`): 2 columns.
  * Categorical feature (`'vehicle_type'` with 4 levels): 4 columns.
  * Total Columns: $2 + 4 = \mathbf{6 \text{ columns}}$.
  * **(b) Feature Names (`get_feature_names_out()`):**
  ```python
  [
      'num__driver_age',
      'num__annual_mileage',
      'cat__vehicle_type_Coupe',
      'cat__vehicle_type_Sedan',
      'cat__vehicle_type_SUV',
      'cat__vehicle_type_Truck'
  ]
  ```
  *(Note: scikit-learn prepends the transformer prefix and sorts categories alphabetically: Coupe, Sedan, SUV, Truck).*
  * **(c) Transformed Vector for `('Motorcycle')` with Mean Age and Mileage:**
  * Since age and mileage equal their training means, standard scaling yields $z = \frac{\mu - \mu}{\sigma} = 0.0$.
  * Since `'Motorcycle'` is novel, the four categorical columns emit zeros under `'ignore'`:
  $$\mathbf{[0.0, \; 0.0, \; 0, \; 0, \; 0, \; 0]}$$ `[L08, Review C5]`
  
  ---
### Problem 9 Solution
  * **(a) Why Linear Regression Fails on Integer Tiers:**
  * In Linear Regression, $\hat{y} = w_1 x_{\text{status}} + w_0$. The slope $w_1$ forces a constant rate of change:
    $$\Delta \hat{y} = w_1 \Delta x$$
  * Going from Bronze ($0$) to Silver ($1$) increases true spend by $\$400$.
  * Going from Silver ($1$) to Gold ($2$) increases true spend by only $\$100$.
  * A single linear slope cannot fit both a $\$400$ jump and a $\$100$ jump with the same step $\Delta x = 1$. The model is forced to fit an average slope ($w_1 \approx \$250$), underpredicting Silver and overpredicting Gold.
  * **(b) Why a Decision Tree Succeeds:**
  * A Decision Tree partitions feature space using independent step thresholds:
    $$\text{Split 1: } \text{if } x_{\text{status}} \le 0.5 \implies \mathbf{\$100 \text{ (Bronze)}}$$
    $$\text{Split 2: } \text{else if } x_{\text{status}} \le 1.5 \implies \mathbf{\$500 \text{ (Silver)}} \quad \text{else } \mathbf{\$600 \text{ (Gold)}}$$
  * Because trees compute independent piecewise-constant leaf averages, they can fit arbitrary non-linear step values along an integer axis without being constrained to a constant slope. `[L08, L10]`
  
  ---
### Problem 10 Solution
  * **(a) Unhandled `None` in `OneHotEncoder()`:**
  * In modern scikit-learn, passing raw `None` or `np.nan` directly to `OneHotEncoder()` treats missingness as an **explicit categorical level**: it creates an extra column named `color_nan`.
  * If the test set contains missing values, they are mapped to this indicator column rather than imputed.
  * **(b) Pipeline Trace with Imputer + Encoder:**
  1. *Training Phase (`pipe.fit()`):*
     * `SimpleImputer(strategy='most_frequent')` observes categories `['red', 'blue', 'red']`. The mode is `'red'`.
     * The missing entry `None` in Row 2 is imputed as `'red'`.
     * The imputed training column is: `['red', 'red', 'blue', 'red']`.
     * `OneHotEncoder()` fits on categories `{'blue', 'red'}` (2 columns: `color_blue`, `color_red`).
     * **Training Row 2 (formerly `None`) is encoded as:** $\mathbf{[0, \; 1]}$ (red).
  2. *Testing Phase (`pipe.transform()`):*
     * Test Row 1 has `color = 'green'`.
     * Since `'green'` was never seen in training and the encoder has `handle_unknown='ignore'`, it emits an all-zero vector:
     * **Test Row 1 (`'green'`) is encoded as:** $\mathbf{[0, \; 0]}$. `[L07, L08]`
  
  ---