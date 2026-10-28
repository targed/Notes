#### Problem 1 (Direct Review Mirror: 1 Negative, 2 Positive SVs)
  The support vectors of a hard-margin linear SVM are:
  $$\text{Point } A = (1, 0), \quad y_A = -1$$
  $$\text{Point } B = (3, 1), \quad y_B = +1$$
  $$\text{Point } C = (3, -1), \quad y_C = +1$$
  *(All other training points lie strictly outside the margin).*
  * **(a)** Using the condition $y_i(w^T x_i + b) = 1$ on each support vector, set up a system of linear equations and solve for $w = [w_1, w_2]^T$ and $b$.
  * **(b)** Compute the Euclidean norm $\|w\|_2$ and the margin width $\gamma = \frac{2}{\|w\|_2}$.
  * **(c)** Using $w = \sum \alpha_i y_i x_i$ and $\sum \alpha_i y_i = 0$, find $\alpha_A, \alpha_B,$ and $\alpha_C$.
  * **(d)** Classify the new test points $x_{\text{test}} = (2.5, 3)$ and $x_{\text{test}} = (0, 0)$.
  
  ---
#### Problem 2 (Vertical Boundary: 2 Negative, 1 Positive SVs)
  A hard-margin linear SVM has three support vectors:
  $$\text{Point } A = (0, 2), \quad y_A = -1$$
  $$\text{Point } B = (0, -2), \quad y_B = -1$$
  $$\text{Point } C = (4, 0), \quad y_C = +1$$
  * **(a)** Solve for $w_1, w_2,$ and $b$.
  * **(b)** Compute the margin width.
  * **(c)** Solve for the dual multipliers $\alpha_A, \alpha_B,$ and $\alpha_C$.
  * **(d)** What is the equation of the decision boundary line?
  
  ---
#### Problem 3 (Horizontal Boundary: $w_1 = 0$)
  A linear SVM separates two classes using three boundary support vectors:
  $$\text{Point } A = (1, 1), \quad y_A = -1$$
  $$\text{Point } B = (3, 1), \quad y_B = -1$$
  $$\text{Point } C = (2, 5), \quad y_C = +1$$
  * **(a)** Set up the three margin equations and solve for $w$ and $b$.
  * **(b)** Compute the margin width.
  * **(c)** Solve for $\alpha_A, \alpha_B,$ and $\alpha_C$.
  * **(d)** Classify $x = (2, 2)$ and $x = (4, 6)$.
  
  ---
#### Problem 4 (Symmetric Diagonal Hyperplane)
  The support vectors for a hard-margin SVM are:
  $$\text{Point } A = (0, 2), \quad y_A = -1$$
  $$\text{Point } B = (2, 0), \quad y_A = -1$$
  $$\text{Point } C = (2, 2), \quad y_C = +1$$
  * **(a)** Solve for the parameters $w$ and $b$.
  * **(b)** Compute the margin width $\gamma$.
  * **(c)** Find the dual multipliers $\alpha_A, \alpha_B,$ and $\alpha_C$.
  * **(d)** Determine whether the query point $q = (1, 1)$ lies on the decision boundary, on the positive side, or on the negative side.
  
  ---
#### Problem 5 (Exact Coordinate Shift Mirror of Practice Exam C5)
  A hard-margin linear SVM has support vectors:
  $$\text{Point } A = (1, 2), \quad y_A = -1$$
  $$\text{Point } B = (2, 4), \quad y_B = +1$$
  $$\text{Point } C = (4, 2), \quad y_C = +1$$
  * **(a)** Solve for $w_1, w_2,$ and $b$.
  * **(b)** Compute the margin width $\frac{2}{\|w\|_2}$.
  * **(c)** Solve for $\alpha_A, \alpha_B,$ and $\alpha_C$.
  * **(d)** Classify the point $x = (3, 3)$.
  
  ---
#### Problem 6 (Two Support Vectors Only: Direct Connection)
  Suppose an SVM is defined by only two support vectors:
  $$\text{Point } A = (1, 1), \quad y_A = -1 \qquad \text{and} \qquad \text{Point } B = (3, 3), \quad y_B = +1$$
  * **(a)** Explain geometrically why the weight vector $w$ must be parallel to $(B - A)$.
  * **(b)** Solve for $w$ and $b$ using the margin equations $w^T A + b = -1$ and $w^T B + b = +1$.
  * **(c)** Compute the margin width $\gamma$. How does the margin width relate to the distance between $A$ and $B$?
  * **(d)** Find the dual multipliers $\alpha_A$ and $\alpha_B$.
  
  ---
#### Problem 7 (Asymmetric Spread Along an Axis)
  A dataset yields three support vectors:
  $$\text{Point } A = (1, 3), \quad y_A = -1$$
  $$\text{Point } B = (1, -1), \quad y_B = -1$$
  $$\text{Point } C = (5, 1), \quad y_C = +1$$
  * **(a)** Solve for $w$ and $b$.
  * **(b)** Compute the margin width.
  * **(c)** Solve for $\alpha_A, \alpha_B,$ and $\alpha_C$.
  
  ---
#### Problem 8 (Perpendicular Distance to Decision Plane)
  A hard-margin linear SVM has support vectors:
  $$\text{Point } A = (2, 1), \quad y_A = -1$$
  $$\text{Point } B = (4, 3), \quad y_B = +1$$
  $$\text{Point } C = (2, 5), \quad y_C = +1$$
  * **(a)** Solve for $w$ and $b$.
  * **(b)** Compute the margin width.
  * **(c)** For query point $x_{\text{test}} = (6, 0)$, evaluate $f(x) = w^T x + b$, classify the point, and calculate its perpendicular Euclidean distance to the decision boundary: $d_\perp = \frac{|w^T x + b|}{\|w\|_2}$.
  
  ---
#### Problem 9 (Reconstructing $w$ and $b$ Directly from Dual Variables)
  Suppose an SVM solver provides the dual solution directly:
  $$\text{Support Vectors: } x_1 = (0, 1), \; y_1 = -1; \quad x_2 = (2, 3), \; y_2 = +1; \quad x_3 = (2, -1), \; y_3 = +1$$
  $$\text{Dual Multipliers: } \alpha_1 = 1.000, \quad \alpha_2 = 0.500, \quad \alpha_3 = 0.500$$
  * **(a)** Verify that KKT Rule 2 ($\sum \alpha_i y_i = 0$) holds.
  * **(b)** Reconstruct the weight vector $w^*$ using KKT Rule 1 ($w = \sum \alpha_i y_i x_i$).
  * **(c)** Using the condition $y_1(w^T x_1 + b) = 1$, solve for the scalar bias $b^*$.
  * **(d)** Calculate the margin width.
  
  ---
#### Problem 10 (Soft-Margin Slack Analysis from a Known Hyperplane)
  A soft-margin linear SVM has fitted parameters $w = [2.0, \; 0.0]^T$ and $b = -4.0$.  
  The canonical margin boundaries are $w^T x + b = \pm 1.0$.
  * **(a)** Write the equations for the decision boundary and the two margin boundaries.
  * **(b)** What is the margin width?
  * **(c)** For each of the following points, calculate the functional margin $y_i(w^T x_i + b)$, determine the slack variable $\xi_i = \max(0, \; 1 - y_i(w^T x_i + b))$, and state whether the point is **correctly classified outside the margin**, a **margin violation**, or **misclassified**:
  * Point 1: $x = (3.0, \; 2.0), \quad y = +1$
  * Point 2: $x = (2.2, \; 5.0), \quad y = +1$
  * Point 3: $x = (1.5, \; 0.0), \quad y = +1$
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C5
### Standard Formulas & Governing Rules:
  1. **Support Vector Margin Equations:**
   $$y_i(w^T x_i + b) = 1 \iff w^T x_i + b = y_i$$
  2. **Margin Width:**
   $$\gamma = \frac{2}{\|w\|_2} = \frac{2}{\sqrt{w_1^2 + w_2^2}}$$
  3. **KKT Rule 2 (Label Balance):**
   $$\sum_{i=1}^n \alpha_i y_i = 0 \implies \sum_{i \in \text{Pos}} \alpha_i = \sum_{j \in \text{Neg}} \alpha_j$$
  4. **KKT Rule 1 (Weight Construction):**
   $$w = \sum_{i=1}^n \alpha_i y_i x_i$$
  5. **Decision Rule:**
   $$\hat{y} = \text{sign}(w^T x + b)$$
  
  ---
### Problem 1 Solution
  * **(a) Solve for $w$ and $b$:**
  * Point $A(1, 0), y = -1 \implies w_1(1) + w_2(0) + b = -1 \implies \mathbf{w_1 + b = -1}$ (Eq 1)
  * Point $B(3, 1), y = +1 \implies w_1(3) + w_2(1) + b = +1 \implies \mathbf{3w_1 + w_2 + b = 1}$ (Eq 2)
  * Point $C(3, -1), y = +1 \implies w_1(3) + w_2(-1) + b = +1 \implies \mathbf{3w_1 - w_2 + b = 1}$ (Eq 3)
  * Subtract Eq 3 from Eq 2:
    $$(3w_1 + w_2 + b) - (3w_1 - w_2 + b) = 1 - 1 \implies 2w_2 = 0 \implies \mathbf{w_2 = 0}$$
  * Substitute $w_2 = 0$ into Eq 2:
    $$3w_1 + b = 1$$
  * Subtract Eq 1 from this result:
    $$(3w_1 + b) - (w_1 + b) = 1 - (-1) \implies 2w_1 = 2 \implies \mathbf{w_1 = 1}$$
  * Substitute $w_1 = 1$ into Eq 1:
    $$1 + b = -1 \implies \mathbf{b = -2}$$
  * **Answer:** $\mathbf{w = [1.0, \; 0.0]^T, \quad b = -2.0}$
  * **(b) Margin Width:**
  $$\|w\|_2 = \sqrt{1^2 + 0^2} = 1.000 \implies \gamma = \frac{2}{\|w\|_2} = \frac{2}{1.0} = \mathbf{2.000}$$
  * **(c) Find Dual Multipliers $\alpha_A, \alpha_B, \alpha_C$:**
  * By Rule 2 ($\sum \alpha_i y_i = 0$):
    $$-\alpha_A + \alpha_B + \alpha_C = 0 \implies \mathbf{\alpha_A = \alpha_B + \alpha_C}$$
  * By Rule 1 ($w = \sum \alpha_i y_i x_i$):
    $$w = -\alpha_A \begin{bmatrix} 1 \\ 0 \end{bmatrix} + \alpha_B \begin{bmatrix} 3 \\ 1 \end{bmatrix} + \alpha_C \begin{bmatrix} 3 \\ -1 \end{bmatrix} = \begin{bmatrix} -\alpha_A + 3\alpha_B + 3\alpha_C \\ \alpha_B - \alpha_C \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}$$
  * From Coordinate 2: $\alpha_B - \alpha_C = 0 \implies \mathbf{\alpha_B = \alpha_C}$.
  * Substitute into Rule 2: $\alpha_A = \alpha_B + \alpha_B = \mathbf{2\alpha_B}$.
  * From Coordinate 1:
    $$-\alpha_A + 3\alpha_B + 3\alpha_C = 1 \implies -(2\alpha_B) + 3\alpha_B + 3\alpha_B = 1 \implies 4\alpha_B = 1$$
    $$\mathbf{\alpha_B = \frac{1}{4} = 0.250}, \quad \mathbf{\alpha_C = \frac{1}{4} = 0.250}, \quad \mathbf{\alpha_A = 2\left(\frac{1}{4}\right) = \frac{1}{2} = 0.500}$$
  * **(d) Classify Test Points:**
  $$f(x) = w^T x + b = 1(x_1) + 0(x_2) - 2 = x_1 - 2$$
  * For $x = (2.5, 3)$: $f(2.5, 3) = 2.5 - 2 = +0.5 > 0 \implies \mathbf{\text{Class } +1}$
  * For $x = (0, 0)$: $f(0, 0) = 0 - 2 = -2.0 < 0 \implies \mathbf{\text{Class } -1}$ `[L21, L22]`
  
  ---
### Problem 2 Solution
  * **(a) Solve for $w$ and $b$:**
  * Point $A(0, 2), y = -1 \implies w_1(0) + w_2(2) + b = -1 \implies \mathbf{2w_2 + b = -1}$ (Eq 1)
  * Point $B(0, -2), y = -1 \implies w_1(0) + w_2(-2) + b = -1 \implies \mathbf{-2w_2 + b = -1}$ (Eq 2)
  * Point $C(4, 0), y = +1 \implies w_1(4) + w_2(0) + b = +1 \implies \mathbf{4w_1 + b = 1}$ (Eq 3)
  * Add Eq 1 and Eq 2:
    $$(2w_2 + b) + (-2w_2 + b) = -1 + (-1) \implies 2b = -2 \implies \mathbf{b = -1}$$
  * Substitute $b = -1$ into Eq 1:
    $$2w_2 - 1 = -1 \implies 2w_2 = 0 \implies \mathbf{w_2 = 0}$$
  * Substitute $b = -1$ into Eq 3:
    $$4w_1 - 1 = 1 \implies 4w_1 = 2 \implies \mathbf{w_1 = \frac{1}{2} = 0.500}$$
  * **Answer:** $\mathbf{w = [0.5, \; 0.0]^T, \quad b = -1.0}$
  * **(b) Margin Width:**
  $$\|w\|_2 = \sqrt{0.5^2 + 0^2} = 0.500 \implies \gamma = \frac{2}{0.500} = \mathbf{4.000}$$
  * **(c) Find Dual Multipliers:**
  * By Rule 2: $-\alpha_A - \alpha_B + \alpha_C = 0 \implies \mathbf{\alpha_C = \alpha_A + \alpha_B}$.
  * By Rule 1:
    $$w = -\alpha_A \begin{bmatrix} 0 \\ 2 \end{bmatrix} - \alpha_B \begin{bmatrix} 0 \\ -2 \end{bmatrix} + \alpha_C \begin{bmatrix} 4 \\ 0 \end{bmatrix} = \begin{bmatrix} 4\alpha_C \\ -2\alpha_A + 2\alpha_B \end{bmatrix} = \begin{bmatrix} 0.5 \\ 0 \end{bmatrix}$$
  * Coordinate 2: $-2\alpha_A + 2\alpha_B = 0 \implies \mathbf{\alpha_A = \alpha_B}$.
  * Coordinate 1: $4\alpha_C = 0.5 \implies \mathbf{\alpha_C = \frac{1}{8} = 0.125}$.
  * Since $\alpha_C = 2\alpha_A$:
    $$\mathbf{\alpha_A = \frac{1}{16} = 0.0625 \approx 0.063}, \quad \mathbf{\alpha_B = \frac{1}{16} = 0.0625 \approx 0.063}$$
  * **(d) Decision Boundary:**
  $$0.5x_1 - 1.0 = 0 \implies \mathbf{x_1 = 2.0}$$
  
  ---
### Problem 3 Solution
  * **(a) Solve for $w$ and $b$:**
  * $A(1, 1), y = -1 \implies \mathbf{w_1 + w_2 + b = -1}$ (Eq 1)
  * $B(3, 1), y = -1 \implies \mathbf{3w_1 + w_2 + b = -1}$ (Eq 2)
  * $C(2, 5), y = +1 \implies \mathbf{2w_1 + 5w_2 + b = 1}$ (Eq 3)
  * Subtract Eq 1 from Eq 2:
    $$2w_1 = 0 \implies \mathbf{w_1 = 0}$$
  * Substitute $w_1 = 0$ into Eq 1 and Eq 3:
    $$w_2 + b = -1 \quad \text{and} \quad 5w_2 + b = 1$$
  * Subtract: $4w_2 = 2 \implies \mathbf{w_2 = 0.500}$.
  * $b = -1 - 0.5 = \mathbf{-1.500}$.
  * **Answer:** $\mathbf{w = [0.0, \; 0.5]^T, \quad b = -1.5}$
  * **(b) Margin Width:**
  $$\|w\|_2 = 0.500 \implies \gamma = \frac{2}{0.500} = \mathbf{4.000}$$
  * **(c) Find Dual Multipliers:**
  * Rule 2: $-\alpha_A - \alpha_B + \alpha_C = 0 \implies \alpha_C = \alpha_A + \alpha_B$.
  * Rule 1:
    $$w = -\alpha_A \begin{bmatrix} 1 \\ 1 \end{bmatrix} - \alpha_B \begin{bmatrix} 3 \\ 1 \end{bmatrix} + \alpha_C \begin{bmatrix} 2 \\ 5 \end{bmatrix} = \begin{bmatrix} -\alpha_A - 3\alpha_B + 2\alpha_C \\ -\alpha_A - \alpha_B + 5\alpha_C \end{bmatrix} = \begin{bmatrix} 0 \\ 0.5 \end{bmatrix}$$
  * Substitute $\alpha_C = \alpha_A + \alpha_B$ into Coordinate 1:
    $$-\alpha_A - 3\alpha_B + 2(\alpha_A + \alpha_B) = \alpha_A - \alpha_B = 0 \implies \mathbf{\alpha_A = \alpha_B}$$
  * Substitute into Coordinate 2:
    $$-2\alpha_A + 5(2\alpha_A) = 8\alpha_A = 0.5 \implies \mathbf{\alpha_A = \frac{1}{16} \approx 0.063}$$
    $$\mathbf{\alpha_B = \frac{1}{16} \approx 0.063}, \quad \mathbf{\alpha_C = \frac{1}{8} = 0.125}$$
  * **(d) Classify Points:**
  $$f(x) = 0.5x_2 - 1.5$$
  * $x = (2, 2) \implies f = 0.5(2) - 1.5 = -0.5 < 0 \implies \mathbf{\text{Class } -1}$
  * $x = (4, 6) \implies f = 0.5(6) - 1.5 = +1.5 > 0 \implies \mathbf{\text{Class } +1}$
  
  ---
### Problem 4 Solution
  * **(a) Solve for $w$ and $b$:**
  * $A(0, 2), y = -1 \implies \mathbf{2w_2 + b = -1}$ (Eq 1)
  * $B(2, 0), y = -1 \implies \mathbf{2w_1 + b = -1}$ (Eq 2)
  * $C(2, 2), y = +1 \implies \mathbf{2w_1 + 2w_2 + b = 1}$ (Eq 3)
  * Subtract Eq 2 from Eq 1: $2w_2 - 2w_1 = 0 \implies \mathbf{w_1 = w_2}$.
  * Subtract Eq 2 from Eq 3: $2w_2 = 1 - (-1) = 2 \implies \mathbf{w_2 = 1.000} \implies \mathbf{w_1 = 1.000}$.
  * From Eq 2: $2(1) + b = -1 \implies \mathbf{b = -3.000}$.
  * **Answer:** $\mathbf{w = [1.0, \; 1.0]^T, \quad b = -3.0}$
  * **(b) Margin Width:**
  $$\|w\|_2 = \sqrt{1^2 + 1^2} = \sqrt{2} \approx 1.414 \implies \gamma = \frac{2}{\sqrt{2}} = \sqrt{2} \approx \mathbf{1.414}$$
  * **(c) Find Dual Multipliers:**
  * Rule 2: $-\alpha_A - \alpha_B + \alpha_C = 0 \implies \alpha_C = \alpha_A + \alpha_B$.
  * Rule 1:
    $$w = -\alpha_A \begin{bmatrix} 0 \\ 2 \end{bmatrix} - \alpha_B \begin{bmatrix} 2 \\ 0 \end{bmatrix} + \alpha_C \begin{bmatrix} 2 \\ 2 \end{bmatrix} = \begin{bmatrix} -2\alpha_B + 2\alpha_C \\ -2\alpha_A + 2\alpha_C \end{bmatrix} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$$
  * Substitute $\alpha_C = \alpha_A + \alpha_B$:
    $$-2\alpha_B + 2(\alpha_A + \alpha_B) = 2\alpha_A = 1 \implies \mathbf{\alpha_A = 0.500}$$
    $$-2\alpha_A + 2(\alpha_A + \alpha_B) = 2\alpha_B = 1 \implies \mathbf{\alpha_B = 0.500}$$
    $$\mathbf{\alpha_C = 0.500 + 0.500 = 1.000}$$
  * **(d) Location of $q = (1, 1)$:**
  $$f(q) = w_1(1) + w_2(1) + b = 1(1) + 1(1) - 3 = -1.000$$
  *Answer:* Because $f(q) = -1.000$, it lies in the **negative class territory** and sits **directly on the negative margin boundary**.
  
  ---
### Problem 5 Solution
  * **(a) Solve for $w$ and $b$:**
  * $A(1, 2), y = -1 \implies \mathbf{w_1 + 2w_2 + b = -1}$ (Eq 1)
  * $B(2, 4), y = +1 \implies \mathbf{2w_1 + 4w_2 + b = 1}$ (Eq 2)
  * $C(4, 2), y = +1 \implies \mathbf{4w_1 + 2w_2 + b = 1}$ (Eq 3)
  * Subtract Eq 2 from Eq 3:
    $$(4w_1 + 2w_2 + b) - (2w_1 + 4w_2 + b) = 0 \implies 2w_1 - 2w_2 = 0 \implies \mathbf{w_1 = w_2}$$
  * Substitute $w_2 = w_1$:
    * From Eq 1: $3w_1 + b = -1 \implies b = -1 - 3w_1$
    * From Eq 2: $6w_1 + b = 1 \implies 6w_1 + (-1 - 3w_1) = 1 \implies 3w_1 = 2 \implies \mathbf{w_1 = \frac{2}{3} \approx 0.667}$
    * $\mathbf{w_2 = \frac{2}{3} \approx 0.667}$
    * $b = -1 - 3(2/3) = \mathbf{-3.000}$
  * **Answer:** $\mathbf{w = [0.667, \; 0.667]^T, \quad b = -3.000}$
  * **(b) Margin Width:**
  $$\|w\|_2 = \sqrt{\left(\frac{2}{3}\right)^2 + \left(\frac{2}{3}\right)^2} = \sqrt{\frac{8}{9}} = \frac{2\sqrt{2}}{3} \approx 0.943$$
  $$\gamma = \frac{2}{\frac{2\sqrt{2}}{3}} = \frac{3}{\sqrt{2}} = \frac{3\sqrt{2}}{2} \approx \mathbf{2.121}$$
  * **(c) Find Dual Multipliers:**
  * Rule 2: $\alpha_A = \alpha_B + \alpha_C$.
  * Rule 1:
    $$w = -\alpha_A \begin{bmatrix} 1 \\ 2 \end{bmatrix} + \alpha_B \begin{bmatrix} 2 \\ 4 \end{bmatrix} + \alpha_C \begin{bmatrix} 4 \\ 2 \end{bmatrix} = \begin{bmatrix} 2/3 \\ 2/3 \end{bmatrix}$$
  * Substitute $\alpha_A = \alpha_B + \alpha_C$:
    * Coordinate 1: $-(\alpha_B + \alpha_C) + 2\alpha_B + 4\alpha_C = \alpha_B + 3\alpha_C = 2/3$
    * Coordinate 2: $-2(\alpha_B + \alpha_C) + 4\alpha_B + 2\alpha_C = 2\alpha_B = 2/3 \implies \mathbf{\alpha_B = \frac{1}{3} \approx 0.333}$
  * From Coordinate 1:
    $$\frac{1}{3} + 3\alpha_C = \frac{2}{3} \implies 3\alpha_C = \frac{1}{3} \implies \mathbf{\alpha_C = \frac{1}{9} \approx 0.111}$$
  * $\mathbf{\alpha_A = \frac{1}{3} + \frac{1}{9} = \frac{4}{9} \approx 0.444}$
  * **(d) Classify $x = (3, 3)$:**
  $$f(3, 3) = \frac{2}{3}(3) + \frac{2}{3}(3) - 3 = 2 + 2 - 3 = +1.000 > 0 \implies \mathbf{\text{Class } +1}$$
  
  ---
### Problem 6 Solution
  * **(a) Geometric Parallelism:**
  * With only two support vectors $A$ (Class $-1$) and $B$ (Class $+1$), the margin boundaries $w^T x + b = \pm 1$ pass through $A$ and $B$.
  * The normal vector $w$ must point directly from $A$ to $B$, meaning $w$ is collinear with the displacement vector:
    $$w \propto (B - A) = \begin{bmatrix} 3 - 1 \\ 3 - 1 \end{bmatrix} = \begin{bmatrix} 2 \\ 2 \end{bmatrix}$$
  * **(b) Solve for $w$ and $b$:**
  * Since $w = c[1, 1]^T$, the midpoint $M = \frac{A + B}{2} = (2, 2)$ must lie on the decision boundary $w^T M + b = 0$:
    $$c(2) + c(2) + b = 0 \implies b = -4c$$
  * Evaluating at $B(3, 3)$:
    $$w^T B + b = c(3) + c(3) + (-4c) = 1 \implies 2c = 1 \implies \mathbf{c = \frac{1}{2} = 0.500}$$
  * **Answer:** $\mathbf{w = [0.5, \; 0.5]^T, \quad b = -2.0}$
  * **(c) Margin Width:**
  $$\|w\|_2 = \sqrt{0.5^2 + 0.5^2} = \sqrt{0.5} = \frac{1}{\sqrt{2}} \implies \gamma = \frac{2}{1/\sqrt{2}} = 2\sqrt{2} \approx \mathbf{2.828}$$
  *(Notice that the Euclidean distance between A and B is $\|B - A\| = \sqrt{2^2 + 2^2} = 2\sqrt{2}$; the margin width equals the distance between the two points).*
  * **(d) Dual Multipliers:**
  * By Rule 2: $-\alpha_A + \alpha_B = 0 \implies \mathbf{\alpha_A = \alpha_B}$.
  * By Rule 1: $w = \alpha_A(-A) + \alpha_B(B) = \alpha_A(B - A) = \alpha_A \begin{bmatrix} 2 \\ 2 \end{bmatrix} = \begin{bmatrix} 0.5 \\ 0.5 \end{bmatrix} \implies 2\alpha_A = 0.5 \implies \mathbf{\alpha_A = 0.250, \; \alpha_B = 0.250}$.
  
  ---
### Problem 7 Solution
  * **(a) Solve for $w$ and $b$:**
  * $A(1, 3), y = -1 \implies \mathbf{w_1 + 3w_2 + b = -1}$ (Eq 1)
  * $B(1, -1), y = -1 \implies \mathbf{w_1 - w_2 + b = -1}$ (Eq 2)
  * $C(5, 1), y = +1 \implies \mathbf{5w_1 + w_2 + b = 1}$ (Eq 3)
  * Subtract Eq 2 from Eq 1:
    $$4w_2 = 0 \implies \mathbf{w_2 = 0}$$
  * From Eq 1: $w_1 + b = -1 \implies b = -1 - w_1$.
  * From Eq 3: $5w_1 + b = 1 \implies 5w_1 - 1 - w_1 = 1 \implies 4w_1 = 2 \implies \mathbf{w_1 = 0.500}$.
  * $b = -1 - 0.500 = \mathbf{-1.500}$.
  * **Answer:** $\mathbf{w = [0.5, \; 0.0]^T, \quad b = -1.5}$
  * **(b) Margin Width:**
  $$\|w\|_2 = 0.500 \implies \gamma = \frac{2}{0.500} = \mathbf{4.000}$$
  * **(c) Find Dual Multipliers:**
  * Rule 2: $\alpha_C = \alpha_A + \alpha_B$.
  * Rule 1:
    $$w = -\alpha_A \begin{bmatrix} 1 \\ 3 \end{bmatrix} - \alpha_B \begin{bmatrix} 1 \\ -1 \end{bmatrix} + \alpha_C \begin{bmatrix} 5 \\ 1 \end{bmatrix} = \begin{bmatrix} 0.5 \\ 0 \end{bmatrix}$$
  * Coordinate 2: $-3\alpha_A + \alpha_B + \alpha_C = 0$.
    Substitute $\alpha_C = \alpha_A + \alpha_B$:
    $$-3\alpha_A + \alpha_B + \alpha_A + \alpha_B = 0 \implies -2\alpha_A + 2\alpha_B = 0 \implies \mathbf{\alpha_A = \alpha_B}$$
  * Since $\alpha_A = \alpha_B$, $\mathbf{\alpha_C = 2\alpha_A}$.
  * Coordinate 1:
    $$-\alpha_A - \alpha_A + 5(2\alpha_A) = 8\alpha_A = 0.5 \implies \mathbf{\alpha_A = \frac{1}{16} \approx 0.063}$$
    $$\mathbf{\alpha_B = \frac{1}{16} \approx 0.063}, \quad \mathbf{\alpha_C = \frac{1}{8} = 0.125}$$
  
  ---
### Problem 8 Solution
  * **(a) Solve for $w$ and $b$:**
  * $A(2, 1), y = -1 \implies \mathbf{2w_1 + w_2 + b = -1}$ (Eq 1)
  * $B(4, 3), y = +1 \implies \mathbf{4w_1 + 3w_2 + b = 1}$ (Eq 2)
  * $C(2, 5), y = +1 \implies \mathbf{2w_1 + 5w_2 + b = 1}$ (Eq 3)
  * Subtract Eq 1 from Eq 3:
    $$4w_2 = 2 \implies \mathbf{w_2 = 0.500}$$
  * Subtract Eq 2 from Eq 3:
    $$-2w_1 + 2w_2 = 0 \implies w_1 = w_2 = \mathbf{0.500}$$
  * From Eq 1:
    $$2(0.5) + 0.5 + b = -1 \implies 1.5 + b = -1 \implies \mathbf{b = -2.500}$$
  * **Answer:** $\mathbf{w = [0.5, \; 0.5]^T, \quad b = -2.5}$
  * **(b) Margin Width:**
  $$\|w\|_2 = \sqrt{0.5^2 + 0.5^2} = \frac{1}{\sqrt{2}} \approx 0.707 \implies \gamma = \frac{2}{1/\sqrt{2}} = 2\sqrt{2} \approx \mathbf{2.828}$$
  * **(c) Evaluate and Distance for $x = (6, 0)$:**
  $$f(6, 0) = w_1(6) + w_2(0) + b = 0.5(6) + 0 - 2.5 = \mathbf{+0.500 > 0 \implies \text{Class } +1}$$
  $$d_\perp = \frac{|f(x)|}{\|w\|_2} = \frac{0.500}{1/\sqrt{2}} = \frac{\sqrt{2}}{2} \approx \mathbf{0.707 \text{ units}}$$
  
  ---
### Problem 9 Solution
  * **(a) Verify Rule 2:**
  $$\sum \alpha_i y_i = 1.0(-1) + 0.5(+1) + 0.5(+1) = -1.0 + 1.0 = \mathbf{0.000 \quad (\text{Verified})}$$
  * **(b) Reconstruct $w^*$:**
  $$w^* = -1.0 \begin{bmatrix} 0 \\ 1 \end{bmatrix} + 0.5 \begin{bmatrix} 2 \\ 3 \end{bmatrix} + 0.5 \begin{bmatrix} 2 \\ -1 \end{bmatrix} = \begin{bmatrix} 0 + 1.0 + 1.0 \\ -1.0 + 1.5 - 0.5 \end{bmatrix} = \mathbf{\begin{bmatrix} 2.0 \\ 0.0 \end{bmatrix}}$$
  * **(c) Solve for Bias $b^*$:**
  Using support vector $x_1 = (0, 1)^T, y_1 = -1$:
  $$y_1(w^T x_1 + b) = 1 \implies -(w_1(0) + w_2(1) + b) = 1$$
  $$-(0 + 0(1) + b) = 1 \implies -b = 1 \implies \mathbf{b^* = -1.000}$$
  * **(d) Margin Width:**
  $$\|w\|_2 = \sqrt{2^2 + 0^2} = 2.000 \implies \gamma = \frac{2}{\|w\|_2} = \frac{2}{2.0} = \mathbf{1.000}$$
  
  ---
### Problem 10 Solution
  * **(a) Boundary Equations ($w = [2, 0]^T, b = -4$):**
  * Decision Boundary: $w^T x + b = 0 \implies 2x_1 - 4 = 0 \implies \mathbf{x_1 = 2.0}$
  * Positive Margin Boundary: $w^T x + b = +1 \implies 2x_1 - 4 = 1 \implies \mathbf{x_1 = 2.5}$
  * Negative Margin Boundary: $w^T x + b = -1 \implies 2x_1 - 4 = -1 \implies \mathbf{x_1 = 1.5}$
  * **(b) Margin Width:**
  $$\|w\|_2 = 2.0 \implies \gamma = \frac{2}{2.0} = \mathbf{1.000}$$
  * **(c) Point-by-Point Slack Analysis:**
  $$\text{Slack Formula: } \xi_i = \max(0, \; 1 - y_i(2x_{i1} - 4))$$
  * **Point 1: $x = (3.0, 2.0), y = +1$:**
    $$y_1(w^T x_1 + b) = 1(2(3) - 4) = +2.0 \implies \mathbf{\xi_1 = \max(0, 1 - 2) = 0.000}$$
    *Status:* $\xi_1 = 0 \implies$ **Correctly classified outside the margin**.
  * **Point 2: $x = (2.2, 5.0), y = +1$:**
    $$y_2(w^T x_2 + b) = 1(2(2.2) - 4) = +0.4 \implies \mathbf{\xi_2 = \max(0, 1 - 0.4) = 0.600}$$
    *Status:* $0 < \xi_2 \le 1 \implies$ **Margin violation** (inside margin street, but on the correct side of the decision boundary; not misclassified).
  * **Point 3: $x = (1.5, 0.0), y = +1$:**
    $$y_3(w^T x_3 + b) = 1(2(1.5) - 4) = -1.0 \implies \mathbf{\xi_3 = \max(0, 1 - (-1)) = 2.000}$$
    *Status:* $\xi_3 > 1 \implies$ **Misclassified** (crossed the decision boundary $x_1 = 2.0$ onto the wrong side). `[L21]`
  
  ---