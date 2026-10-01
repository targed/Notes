# Part C6 — NumPy Shapes Practice Problems
#### Problem 1 (Direct Review Mirror)
  An array $A$ has shape `(5, 4)`. Give the result shape or write `"error"` for each operation:  
  * **(a)** `A + np.ones(4)`  
  * **(b)** `A + np.ones(5)`  
  * **(c)** `np.ones((5, 1)) + np.ones((1, 6))`  
  * **(d)** `A - A.mean(axis=0)`  
  * **(e)** `A - A.mean(axis=1)`
  
  ---
#### Problem 2 (3D Tensor Reductions & Centering)
  A rank-3 image tensor $T$ has shape `(10, 8, 3)` (10 images, $8 \times 3$ spatial/channel matrix). Give the result shape or write `"error"`:  
  * **(a)** `T - T.mean(axis=0)`  
  * **(b)** `T - T.mean(axis=-1)`  
  * **(c)** `T - T.mean(axis=1)`  
  * **(d)** `T + np.ones((8, 3))`  
  * **(e)** `T + np.ones((10, 8))`
  
  ---
#### Problem 3 (The Unintended Outer-Difference Broadcasting Trap)
  In a linear regression evaluation script, the model prediction array `y_pred` has shape `(100, 1)` and the ground-truth target vector `y_true` has shape `(100,)`.  
  * **(a)** What is the shape of the subtraction `diff = y_pred - y_true`?  
  * **(b)** Why does this operation execute without raising a `ValueError`?  
  * **(c)** What numerical defect does this introduce into the computation of Mean Squared Error: `loss = np.mean(diff ** 2)`?  
  * **(d)** Write the exact NumPy syntax to compute the correct $(100,)$ element-wise residual vector.
  
  ---
#### Problem 4 (Higher-Order Rank Broadcasting)
  Evaluate the resulting shape or write `"error"` for each expression:  
  * **(a)** `np.ones((2, 1, 4)) + np.ones((3, 1))`  
  * **(b)** `np.ones((6, 1, 5, 1)) + np.ones((4, 1, 7))`  
  * **(c)** `np.ones((4, 3, 2)) + np.ones((4, 2))`  
  * **(d)** `np.ones((1, 5, 1)) * np.ones((7, 1, 3))`
  
  ---
#### Problem 5 (Matrix Multiplication `@` vs. Element-Wise Multiplication `*`)
  Let design matrix $X$ have shape `(4, 2)` and weight matrix $W$ have shape `(2, 5)`. Give the result shape or write `"error"`:  
  * **(a)** `X @ W`  
  * **(b)** `X * W`  
  * **(c)** `X.T @ X`  
  * **(d)** `X @ X.T`  
  * **(e)** `W @ X`
  
  ---
#### Problem 6 (The `keepdims` Normalization Invariant)
  Given a 2D matrix $M$ with shape `(6, 4)`:  
  * **(a)** What is the shape of `M.sum(axis=0, keepdims=True)`?  
  * **(b)** What is the shape of `M.sum(axis=1, keepdims=True)`?  
  * **(c)** What is the shape of `M / M.sum(axis=1, keepdims=True)`? What does this operation compute mathematically?  
  * **(d)** What happens if an engineer executes `M / M.sum(axis=1)`? Explain why.
  
  ---
#### Problem 7 (Array Indexing: Rank Reduction vs. Preservation)
  A design matrix $Z$ has shape `(10, 8)` (10 samples, 8 features). State the exact shape of the array returned by each slice:  
  * **(a)** `Z[2, :]`  
  * **(b)** `Z[2:3, :]`  
  * **(c)** `Z[:, 4]`  
  * **(d)** `Z[:, 4:5]`  
  * **(e)** `Z[:, 4, np.newaxis]`
  
  ---
#### Problem 8 (Multi-Axis Reductions on Rank-4 Tensors)
  A convolutional activation tensor $B$ has shape `(2, 3, 4, 5)` (batch, channels, height, width). State the shape resulting from each reduction:  
  * **(a)** `B.mean(axis=(0, 2))`  
  * **(b)** `B.mean(axis=(1, 3))`  
  * **(c)** `B.mean(axis=(1, 2, 3))`  
  * **(d)** `B.mean(axis=(0, 1, 2, 3))`  
  * **(e)** `B - B.mean(axis=(1, 2, 3), keepdims=True)`
  
  ---
#### Problem 9 (Vectorized Pairwise Euclidean Distance Matrix)
  To compute all pairwise distances between two point clouds without Python loops, an engineer formats set $P$ (50 3D points) with shape `(50, 1, 3)` and set $Q$ (20 3D points) with shape `(1, 20, 3)`.  
  * **(a)** What is the shape of the difference tensor `diff = P - Q`?  
  * **(b)** What is the shape of the pairwise distance matrix `dist = np.sqrt(np.sum(diff ** 2, axis=-1))`?  
  * **(c)** What does the scalar entry `dist[i, j]` represent?
  
  ---
#### Problem 10 (Gradient Dimension Tracking in OLS)
  In multiple linear regression, design matrix $X$ has shape `(100, 5)` (including the augmented column of ones), weight vector $w$ has shape `(5,)`, and label vector $y$ has shape `(100,)`.  
  Evaluate the shape of each linear algebra sub-expression:  
  * **(a)** `y_hat = X @ w`  
  * **(b)** `error = y_hat - y`  
  * **(c)** `X.T @ error`  
  * **(d)** `X.T @ X`  
  * **(e)** `(X.T @ X) @ w`
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C6
### Problem 1 Solution
  * **(a) `A + np.ones(4)`:**
  * Shape alignment: `(5, 4)` and `(4,)` $\to$ `(5, 4)` and `(1, 4)`.
  * Trailing axis matches ($4 == 4$). Leading axis broadcasts ($5 \text{ vs } 1 \to 5$).
  * **Result:** **`(5, 4)`**
  * **(b) `A + np.ones(5)`:**
  * Shape alignment: `(5, 4)` and `(5,)` $\to$ `(5, 4)` and `(1, 5)`.
  * Trailing axes: **$4 \text{ vs } 5$**. Neither is $1$, and $4 \neq 5$.
  * **Result:** **`"error"`** (`ValueError: operands could not be broadcast together`)
  * **(c) `np.ones((5, 1)) + np.ones((1, 6))`:**
  * Trailing axis: $1 \text{ vs } 6 \to 6$.
  * Leading axis: $5 \text{ vs } 1 \to 5$.
  * **Result:** **`(5, 6)`**
  * **(d) `A - A.mean(axis=0)`:**
  * `axis=0` collapses rows $\implies \text{shape is } (4,)$.
  * `(5, 4) - (4,)` $\to$ `(5, 4) - (1, 4)`. Trailing axes match ($4 == 4$). This computes column-centering.
  * **Result:** **`(5, 4)`**
  * **(e) `A - A.mean(axis=1)`:**
  * `axis=1` collapses columns $\implies \text{shape is } (5,)$.
  * `(5, 4) - (5,)` $\to$ `(5, 4) - (1, 5)`. Trailing axes **$4 \text{ vs } 5$** mismatch.
  * **Result:** **`"error"`** (`ValueError`) `[L05, Review C6]`
  
  ---
### Problem 2 Solution
  * **(a) `T - T.mean(axis=0)`:**
  * `T.mean(axis=0)` collapses axis 0 $\implies$ shape is `(8, 3)`.
  * Alignment: `(10, 8, 3)` and `(8, 3)` $\to$ `(10, 8, 3)` and `(1, 8, 3)`.
  * Trailing axes match ($8 == 8, 3 == 3$). Leading axis stretches to $10$.
  * **Result:** **`(10, 8, 3)`**
  * **(b) `T - T.mean(axis=-1)`:**
  * `axis=-1` collapses the last axis (size 3) $\implies$ shape is `(10, 8)`.
  * Alignment: `(10, 8, 3)` and `(10, 8)` $\to$ `(10, 8, 3)` and `(1, 10, 8)`.
  * Trailing axes: **$3 \text{ vs } 8$** mismatch.
  * **Result:** **`"error"`** (`ValueError`)
  * **(c) `T - T.mean(axis=1)`:**
  * `axis=1` collapses the middle axis (size 8) $\implies$ shape is `(10, 3)`.
  * Alignment: `(10, 8, 3)` and `(10, 3)` $\to$ `(10, 8, 3)` and `(1, 10, 3)`.
  * Trailing axis matches ($3 == 3$), but middle axes **$8 \text{ vs } 10$** mismatch.
  * **Result:** **`"error"`** (`ValueError`)
  * **(d) `T + np.ones((8, 3))`:**
  * Alignment: `(10, 8, 3)` and `(1, 8, 3)`. Both trailing axes match.
  * **Result:** **`(10, 8, 3)`**
  * **(e) `T + np.ones((10, 8))`:**
  * Alignment: `(10, 8, 3)` and `(1, 10, 8)`. Trailing axes **$3 \text{ vs } 8$** mismatch.
  * **Result:** **`"error"`** (`ValueError`) `[L05]`
  
  ---
### Problem 3 Solution
  * **(a) Shape of `diff`:**
  * Alignment: `y_pred` has shape `(100, 1)`. `y_true` has shape `(100,)`, which right-aligns as `(1, 100)`.
  * Trailing axis: $1 \text{ vs } 100 \implies 100$.
  * Leading axis: $100 \text{ vs } 1 \implies 100$.
  * **Result:** **`(100, 100)`**
  * **(b) Why No Error Occurs:**
  * Both arrays possess a singleton dimension ($1$). NumPy's broadcasting rules allow any axis of size $1$ to stretch to match the complementary array's size.
  * **(c) Numerical Defect:**
  * Instead of calculating the $100$ element-wise prediction residuals $(y_{\text{pred}, i} - y_{\text{true}, i})$, NumPy calculates the **outer subtraction matrix** between every prediction and every target pair ($10{,}000$ values).
  * `np.mean(diff ** 2)` computes the average squared difference across all pairwise combinations, inflating memory usage by $100\times$ and corrupting the loss.
  * **(d) Correct Syntax:**
  ```python
  diff = y_pred.squeeze() - y_true  # Shape: (100,) - (100,) = (100,)
  # Or: diff = y_pred[:, 0] - y_true
  ```
  `[L05, L10]`
  
  ---
### Problem 4 Solution
  * **(a) `np.ones((2, 1, 4)) + np.ones((3, 1))`:**
  $$\text{Array 1: } \quad 2 \quad \times \quad 1 \quad \times \quad 4$$
  $$\text{Array 2: } \quad \mathbf{1} \quad \times \quad 3 \quad \times \quad 1$$
  * Axis 2: $4 \text{ vs } 1 \to 4$. Axis 1: $1 \text{ vs } 3 \to 3$. Axis 0: $2 \text{ vs } 1 \to 2$.
  * **Result:** **`(2, 3, 4)`**
  * **(b) `np.ones((6, 1, 5, 1)) + np.ones((4, 1, 7))`:**
  $$\text{Array 1: } \quad 6 \quad \times \quad 1 \quad \times \quad 5 \quad \times \quad 1$$
  $$\text{Array 2: } \quad \mathbf{1} \quad \times \quad 4 \quad \times \quad 1 \quad \times \quad 7$$
  * Axis 3: $1 \text{ vs } 7 \to 7$. Axis 2: $5 \text{ vs } 1 \to 5$. Axis 1: $1 \text{ vs } 4 \to 4$. Axis 0: $6 \text{ vs } 1 \to 6$.
  * **Result:** **`(6, 4, 5, 7)`**
  * **(c) `np.ones((4, 3, 2)) + np.ones((4, 2))`:**
  $$\text{Array 1: } \quad 4 \quad \times \quad 3 \quad \times \quad 2$$
  $$\text{Array 2: } \quad \mathbf{1} \quad \times \quad 4 \quad \times \quad 2$$
  * Trailing axis matches ($2 == 2$). Middle axes: **$3 \text{ vs } 4$** mismatch (neither is $1$).
  * **Result:** **`"error"`** (`ValueError`)
  * **(d) `np.ones((1, 5, 1)) * np.ones((7, 1, 3))`:**
  $$\text{Array 1: } \quad 1 \quad \times \quad 5 \quad \times \quad 1$$
  $$\text{Array 2: } \quad 7 \quad \times \quad 1 \quad \times \quad 3$$
  * **Result:** **`(7, 5, 3)`** `[L05]`
  
  ---
### Problem 5 Solution
  * **(a) `X @ W`:**
  * Matrix multiplication: `(4, 2) @ (2, 5)`. Inner dimensions match ($2 == 2$).
  * **Result:** **`(4, 5)`**
  * **(b) `X * W`:**
  * Element-wise multiplication requires broadcasting: `(4, 2)` vs. `(2, 5)`. Trailing axes **$2 \text{ vs } 5$** mismatch.
  * **Result:** **`"error"`** (`ValueError`)
  * **(c) `X.T @ X`:**
  * Gram matrix multiplication: `(2, 4) @ (4, 2)`. Inner dimensions match ($4 == 4$).
  * **Result:** **`(2, 2)`**
  * **(d) `X @ X.T`:**
  * Outer covariance matrix: `(4, 2) @ (2, 4)`. Inner dimensions match ($2 == 2$).
  * **Result:** **`(4, 4)`**
  * **(e) `W @ X`:**
  * Matrix multiplication: `(2, 5) @ (4, 2)`. Inner dimensions **$5 \neq 4$** mismatch.
  * **Result:** **`"error"`** (`ValueError: matmul: Input operand 1 has a mismatch`) `[L05, L10]`
  
  ---
### Problem 6 Solution
  * **(a) `M.sum(axis=0, keepdims=True)`:**
  * Collapses rows, but retains reduced axis as a singleton dimension ($1$).
  * **Result:** **`(1, 4)`**
  * **(b) `M.sum(axis=1, keepdims=True)`:**
  * Collapses columns, but retains reduced axis as a singleton dimension ($1$).
  * **Result:** **`(6, 1)`**
  * **(c) `M / M.sum(axis=1, keepdims=True)`:**
  * Broadcasting: `(6, 4) / (6, 1)`. The column vector `(6, 1)` broadcasts across the $4$ columns.
  * **Result:** **`(6, 4)`**
  * *Mathematical Interpretation:* Computes **row-wise sum normalization** (each row now sums to $1.0$, creating probability vectors).
  * **(d) `M / M.sum(axis=1)`:**
  * Without `keepdims=True`, `M.sum(axis=1)` emits a 1D vector of shape **`(6,)`**.
  * Evaluating `(6, 4) / (6,)` right-aligns as `(6, 4) / (1, 6)`. Trailing axes **$4 \text{ vs } 6$** mismatch.
  * **Result:** **`"error"`** (`ValueError`) `[L05, L08]`
  
  ---
### Problem 7 Solution
  * **(a) `Z[2, :]`:**
  * Scalar row indexing collapses rank.
  * **Result:** **`(8,)`**
  * **(b) `Z[2:3, :]`:**
  * Slicing (`2:3`) preserves 2D rank.
  * **Result:** **`(1, 8)`**
  * **(c) `Z[:, 4]`:**
  * Scalar column indexing collapses rank.
  * **Result:** **`(10,)`**
  * **(d) `Z[:, 4:5]`:**
  * Column slicing (`4:5`) preserves 2D rank, extracting a column vector.
  * **Result:** **`(10, 1)`**
  * **(e) `Z[:, 4, np.newaxis]`:**
  * `Z[:, 4]` produces `(10,)`, and `np.newaxis` expands rank into a column vector.
  * **Result:** **`(10, 1)`** `[L05]`
  
  ---
### Problem 8 Solution
  * **(a) `B.mean(axis=(0, 2))` on `(2, 3, 4, 5)`:**
  * Axes 0 (size 2) and 2 (size 4) disappear. Axes 1 (size 3) and 3 (size 5) survive.
  * **Result:** **`(3, 5)`**
  * **(b) `B.mean(axis=(1, 3))` on `(2, 3, 4, 5)`:**
  * Axes 1 (size 3) and 3 (size 5) disappear. Axes 0 (size 2) and 2 (size 4) survive.
  * **Result:** **`(2, 4)`**
  * **(c) `B.mean(axis=(1, 2, 3))` on `(2, 3, 4, 5)`:**
  * Axes 1, 2, 3 disappear. Only axis 0 survives.
  * **Result:** **`(2,)`**
  * **(d) `B.mean(axis=(0, 1, 2, 3))` on `(2, 3, 4, 5)`:**
  * All axes collapsed simultaneously into a single scalar value.
  * **Result:** **`()` (Rank-0 Scalar)**
  * **(e) `B - B.mean(axis=(1, 2, 3), keepdims=True)`:**
  * `keepdims=True` preserves collapsed dimensions as singletons $\implies \text{shape is } (2, 1, 1, 1)$.
  * Broadcasting: `(2, 3, 4, 5) - (2, 1, 1, 1)`. The singletons broadcast across channels, height, and width.
  * **Result:** **`(2, 3, 4, 5)`** (Instance-level centering). `[L05]`
  
  ---
### Problem 9 Solution
  * **(a) Shape of `diff = P - Q`:**
  * Shape alignment: `(50, 1, 3)` and `(1, 20, 3)`.
  * Trailing axis matches ($3 == 3$). Middle axis stretches to $20$. Leading axis stretches to $50$.
  * **Result:** **`(50, 20, 3)`**
  * **(b) Shape of `dist`:**
  * `diff ** 2` retains shape `(50, 20, 3)`.
  * `np.sum(..., axis=-1)` collapses the trailing coordinate axis (size 3) into a sum of squared differences $\implies$ shape becomes `(50, 20)`.
  * `np.sqrt(...)` preserves dimensions.
  * **Result:** **`(50, 20)`**
  * **(c) Mathematical Representation:**
  * Entry `dist[i, j]` is the exact **Euclidean distance** between 3D point $i$ in set $P$ and 3D point $j$ in set $Q$:
    $$\text{dist}[i, j] = \sqrt{\sum_{k=1}^3 (P_{i, k} - Q_{j, k})^2}$$
  * This is the canonical vectorized pattern for computing full pairwise distance matrices in $k$-NN and clustering without nested `for` loops. `[L05, L14]`
  
  ---
### Problem 10 Solution
  * **(a) `y_hat = X @ w`:**
  * Matrix-vector product: `(100, 5) @ (5,)`. Inner dimensions match ($5 == 5$).
  * **Result:** **`(100,)`**
  * **(b) `error = y_hat - y`:**
  * Element-wise subtraction: `(100,) - (100,)`.
  * **Result:** **`(100,)`**
  * **(c) `X.T @ error`:**
  * Matrix-vector product: `(5, 100) @ (100,)`. Inner dimensions match ($100 == 100$).
  * **Result:** **`(5,)`** (The gradient vector $\nabla_w J$).
  * **(d) `X.T @ X`:**
  * Gram matrix multiplication: `(5, 100) @ (100, 5)`.
  * **Result:** **`(5, 5)`**
  * **(e) `(X.T @ X) @ w`:**
  * Matrix-vector product: `(5, 5) @ (5,)`.
  * **Result:** **`(5,)`** `[L05, L10]`
  
  ---