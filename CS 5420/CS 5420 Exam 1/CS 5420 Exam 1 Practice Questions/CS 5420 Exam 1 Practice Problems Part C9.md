# Part C9 — $k$-NN by Hand Practice Problems
#### Problem 1 (Standard Coordinate Mirror)
  A 2D spatial training set contains five observations:
  ```
  Point:      A       B       C       D       E
  (x1, x2):   (2, 1)  (3, 3)  (5, 4)  (6, 6)  (2, 5)
  Class:      Red     Red     Blue    Blue    Blue
  ```
  A query instance arrives at $q = (3, 2)$.
  * **(a)** Compute the exact Euclidean distance from $q$ to each of the five points.
  * **(b)** Rank the neighbors from nearest to farthest, and predict the class label for $k = 1$, $k = 3$, and $k = 5$.
  * **(c)** What is the Manhattan ($L_1$) distance from $q$ to Point $D$?
  
  ---
#### Problem 2 (Handling Exact Distance Ties)
  Consider five training instances in $\mathbb{R}^2$:
  ```
  Point:      A       B       C       D       E
  (x1, x2):   (1, 2)  (3, 2)  (2, 1)  (2, 3)  (4, 4)
  Class:      Red     Red     Blue    Blue    Blue
  ```
  A query instance is evaluated at $q = (2, 2)$.
  * **(a)** Compute the Euclidean distance from $q$ to each point. Notice the distance ties among the closest points.
  * **(b)** For $k = 1$, what is the voting tally, and why does nearest-neighbor classification experience a deadlock?
  * **(c)** What happens if an algorithm selects $k = 3$ without an explicit spatial tie-breaking rule?
  * **(d)** What is the predicted class for $k = 5$?
  
  ---
#### Problem 3 (Inverse-Distance Weighted $k$-NN)
  Three training points are located near query point $q = (1, 2)$:
  * Point $A = (1, 1)$, Class: **Positive ($+$)**
  * Point $B = (2, 4)$, Class: **Negative ($-$)**
  * Point $C = (4, 3)$, Class: **Negative ($-$)**
  * **(a)** Compute the Euclidean distance from $q$ to points $A$, $B$, and $C$.
  * **(b)** Predict the class label using standard **unweighted $3$-NN** majority voting.
  * **(c)** Predict the class label using **inverse-distance-squared weighted $3$-NN** ($w_i = \frac{1}{d_i^2}$). Compute the exact voting score for each class and show that distance weighting flips the prediction.
  
  ---
#### Problem 4 (The Unscaled Feature Distortion in $k$-NN)
  An employee retention model uses two continuous features:
  * $x_1 \in [1, 5]$ (Job Level, integer rank)
  * $x_2 \in [20, 100]$ (Annual Salary, in thousands of dollars)
  
  We have three training points and a query instance $q = (4, 32)$:
  ```
  Point A: (2, 30), Class: Low
  Point B: (3, 80), Class: High
  Point C: (5, 35), Class: Low
  ```
  * **(a)** Compute the **unscaled** Euclidean distance from $q$ to points $A$, $B$, and $C$. Which point is selected as the $1$-NN?
  * **(b)** Min-Max scale both features into $[0, 1]$ using training bounds ($x_1 \in [2, 5] \implies \text{range} = 3$; $x_2 \in [30, 80] \implies \text{range} = 50$). Find the scaled coordinates $A'$, $B'$, $C'$, and $q'$.
  * **(c)** Compute the Euclidean distances in the **scaled space**. Which point is the true $1$-NN now?
  
  ---
#### Problem 5 (Multiclass 3-Way $k$-NN Classification)
  A dataset contains three distinct classes: **Circle ($C$)**, **Square ($S$)**, and **Triangle ($T$)**:
  ```
  Point:      A       B       C       D       E       F
  (x1, x2):   (1, 1)  (2, 2)  (3, 4)  (4, 3)  (1, 4)  (2, 5)
  Class:      C       C       S       S       T       T
  ```
  Query instance: $q = (2, 3)$.
  * **(a)** Compute the Euclidean distance from $q$ to all six points.
  * **(b)** Predict the class label for $k = 1$.
  * **(c)** Predict the class label for $k = 3$. What issue arises in multiclass voting when $k = 3$?
  
  ---
#### Problem 6 ($k$-NN Regression by Hand)
  In a real estate valuation model, the target variable $y$ is continuous house price (in $\$100{,}000\text{s}$):
  ```
  Point A: (1, 2)  ──▶  y = 3.0  ($300,000)
  Point B: (2, 1)  ──▶  y = 2.5  ($250,000)
  Point C: (3, 4)  ──▶  y = 6.0  ($600,000)
  Point D: (4, 2)  ──▶  y = 5.0  ($500,000)
  ```
  A new house arrives with features $q = (2, 3)$.
  * **(a)** Compute Euclidean distances from $q$ to $A, B, C, D$. Identify the 3 nearest neighbors.
  * **(b)** Predict the price $\hat{y}$ using **unweighted $3$-NN regression** (arithmetic average of 3 nearest targets).
  * **(c)** Predict the price $\hat{y}$ using **inverse-distance weighted $3$-NN regression** ($w_i = \frac{1}{d_i}$).
  
  ---
#### Problem 7 (Metric Comparison: $L_1$ vs. $L_2$ vs. $L_\infty$)
  Consider query point $q = (0, 0)$ and two candidate training instances:
  * Point $A = (2, 2)$, Class: **Red**
  * Point $B = (0, 2.5)$, Class: **Blue**
  * **(a)** Compute the Manhattan ($L_1$), Euclidean ($L_2$), and Chebyshev ($L_\infty$) distances from $q$ to $A$.
  * **(b)** Compute the Manhattan ($L_1$), Euclidean ($L_2$), and Chebyshev ($L_\infty$) distances from $q$ to $B$.
  * **(c)** Who is the nearest neighbor ($1$-NN) under the Manhattan ($L_1$) metric, and what class is predicted?
  * **(d)** Who is the nearest neighbor ($1$-NN) under the Chebyshev ($L_\infty$) metric, and what class is predicted?
  
  ---
#### Problem 8 (3D Feature Space $k$-NN)
  Four training observations reside in $\mathbb{R}^3$ $(x_1, x_2, x_3)$:
  ```
  Point A: (1, 0, 2), Class: 0
  Point B: (2, 1, 0), Class: 0
  Point C: (0, 2, 2), Class: 1
  Point D: (3, 1, 2), Class: 1
  ```
  Query instance: $q = (1, 1, 1)$.
  * **(a)** Compute the exact Euclidean distance from $q$ to each point.
  * **(b)** Predict the class label for $k = 1$ and $k = 3$.
  
  ---
#### Problem 9 (Leave-One-Out Cross-Validation (LOOCV) of $1$-NN)
  A toy training set contains four points in $\mathbb{R}^2$:
  ```
  P1: (1, 1), Class: Red
  P2: (2, 1), Class: Blue
  P3: (1, 3), Class: Red
  P4: (2, 3), Class: Red
  ```
  * **(a)** For each point $P_i$, identify its nearest neighbor among the remaining three points and determine whether $1$-NN classifies it correctly.
  * **(b)** Calculate the exact **Leave-One-Out Cross-Validation error rate** ($\text{Error} = \frac{\text{Misclassified}}{N}$).
  
  ---
#### Problem 10 (Deriving the $1$-NN Decision Boundary Analytically)
  Two training points form a binary classification dataset:
  $$\text{Point } A = (2, 2) \quad [\text{Class 1}], \qquad \text{Point } B = (6, 4) \quad [\text{Class 0}]$$
  * **(a)** Find the coordinates of the midpoint $M$ between $A$ and $B$.
  * **(b)** Compute the slope $m_{AB}$ of the line segment connecting $A$ and $B$.
  * **(c)** The decision boundary of a $1$-NN classifier between two points is the **perpendicular bisector**. Derive the explicit linear equation ($x_2 = m_\perp x_1 + c$) of this decision boundary.
  * **(d)** Using your equation, classify the query point $(2, 5)$ without computing distances.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C9
### Problem 1 Solution
  * **(a) Euclidean Distances from $q = (3, 2)$ ($D = \sqrt{\Delta x_1^2 + \Delta x_2^2}$):**
  * $d(q, A) = \sqrt{(3 - 2)^2 + (2 - 1)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$
  * $d(q, B) = \sqrt{(3 - 3)^2 + (2 - 3)^2} = \sqrt{0 + 1} = \sqrt{1} = \mathbf{1.000}$
  * $d(q, C) = \sqrt{(3 - 5)^2 + (2 - 4)^2} = \sqrt{4 + 4} = \sqrt{8} \approx \mathbf{2.828}$
  * $d(q, D) = \sqrt{(3 - 6)^2 + (2 - 6)^2} = \sqrt{9 + 16} = \sqrt{25} = \mathbf{5.000}$
  * $d(q, E) = \sqrt{(3 - 2)^2 + (2 - 5)^2} = \sqrt{1 + 9} = \sqrt{10} \approx \mathbf{3.162}$
  * **(b) Ranking & Voting:**
  * **Sorted Order:** 1: $B$ (1.000, Red), 2: $A$ (1.414, Red), 3: $C$ (2.828, Blue), 4: $E$ (3.162, Blue), 5: $D$ (5.000, Blue).
  * **$k = 1$:** Nearest is $B \implies \mathbf{Red}$
  * **$k = 3$:** Nearest are $\{B, A, C\} \implies 2 \text{ Red}, 1 \text{ Blue} \implies \mathbf{Red}$
  * **$k = 5$:** All five points $\implies 2 \text{ Red}, 3 \text{ Blue} \implies \mathbf{Blue}$
  * **(c) Manhattan Distance to $D(6, 6)$:**
  $$D_1(q, D) = |3 - 6| + |2 - 6| = |-3| + |-4| = 3 + 4 = \mathbf{7.000}$$ `[L14, Review C9]`
  
  ---
### Problem 2 Solution
  * **(a) Compute Distances from $q = (2, 2)$:**
  * $d(q, A) = \sqrt{(2 - 1)^2 + (2 - 2)^2} = \sqrt{1 + 0} = \mathbf{1.000}$
  * $d(q, B) = \sqrt{(2 - 3)^2 + (2 - 2)^2} = \sqrt{1 + 0} = \mathbf{1.000}$
  * $d(q, C) = \sqrt{(2 - 2)^2 + (2 - 1)^2} = \sqrt{0 + 1} = \mathbf{1.000}$
  * $d(q, D) = \sqrt{(2 - 2)^2 + (2 - 3)^2} = \sqrt{0 + 1} = \mathbf{1.000}$
  * $d(q, E) = \sqrt{(2 - 4)^2 + (2 - 4)^2} = \sqrt{4 + 4} = \sqrt{8} \approx \mathbf{2.828}$
  * **(b) $k = 1$ Deadlock:**
  * Four points ($A, B, C, D$) are tied at distance $1.000$.
  * Two are Red ($A, B$) and two are Blue ($C, D$).
  * The model faces a **$50/50$ deadlock** and must flip a coin or rely on arbitrary index order.
  * **(c) $k = 3$ Sensitivity:**
  * The algorithm must select 3 points out of the 4 tied at distance $1.000$.
  * If it picks $\{A, B, C\}$, it predicts **Red** ($2 \text{ Red}, 1 \text{ Blue}$).
  * If it picks $\{A, C, D\}$, it predicts **Blue** ($1 \text{ Red}, 2 \text{ Blue}$).
  * Without an explicit tie-breaking rule, predictions depend on dataset row ordering.
  * **(d) $k = 5$ Prediction:**
  * Neighborhood includes all 5 points: $\{A (\text{R}), B (\text{R}), C (\text{B}), D (\text{B}), E (\text{B})\}$.
  * Tally: $2 \text{ Red}$ vs. $3 \text{ Blue} \implies \mathbf{Blue}$. `[L14]`
  
  ---
### Problem 3 Solution
  * **(a) Euclidean Distances from $q = (1, 2)$:**
  * $d(q, A) = \sqrt{(1 - 1)^2 + (2 - 1)^2} = \sqrt{0 + 1} = \mathbf{1.000}$
  * $d(q, B) = \sqrt{(1 - 2)^2 + (2 - 4)^2} = \sqrt{1 + 4} = \sqrt{5} \approx \mathbf{2.236}$
  * $d(q, C) = \sqrt{(1 - 4)^2 + (2 - 3)^2} = \sqrt{9 + 1} = \sqrt{10} \approx \mathbf{3.162}$
  * **(b) Unweighted $3$-NN Majority Voting:**
  * Neighbors: $\{A (+), B (-), C (-)\} \implies 1 (+)$ vs. $2 (-) \implies \mathbf{\text{Class } - \quad (\text{Negative})}$
  * **(c) Inverse-Distance-Squared Weighted $3$-NN ($w_i = 1/d_i^2$):**
  $$w_A = \frac{1}{1.000^2} = \mathbf{1.000}$$
  $$w_B = \frac{1}{(\sqrt{5})^2} = \frac{1}{5} = \mathbf{0.200}$$
  $$w_C = \frac{1}{(\sqrt{10})^2} = \frac{1}{10} = \mathbf{0.100}$$
  * Accumulate scores by class:
    $$\text{Score}(+) = w_A = \mathbf{1.000}$$
    $$\text{Score}(-) = w_B + w_C = 0.200 + 0.100 = \mathbf{0.300}$$
  * **Result:** Because $1.000 > 0.300$, the weighted vote predicts **$\mathbf{\text{Class } +}$**. Neighbor $A$ is so close that its vote outweighs the two farther negative points combined. `[L14]`
  
  ---
### Problem 4 Solution
  * **(a) Unscaled Distances from $q = (4, 32)$:**
  * $d(q, A) = \sqrt{(4 - 2)^2 + (32 - 30)^2} = \sqrt{4 + 4} = \sqrt{8} \approx \mathbf{2.828}$
  * $d(q, B) = \sqrt{(4 - 3)^2 + (32 - 80)^2} = \sqrt{1 + 48^2} = \sqrt{1 + 2{,}304} = \sqrt{2{,}305} \approx \mathbf{48.010}$
  * $d(q, C) = \sqrt{(4 - 5)^2 + (32 - 35)^2} = \sqrt{1 + 9} = \sqrt{10} \approx \mathbf{3.162}$
  * Closest unscaled point is **Point $A$** ($2.828 < 3.162$).
  * **(b) Min-Max Scaling ($x_1' = \frac{x_1 - 2}{3}, \; x_2' = \frac{x_2 - 30}{50}$):**
  * $A' = \left(\frac{2-2}{3}, \frac{30-30}{50}\right) = \mathbf{(0.000, \; 0.000)}$
  * $B' = \left(\frac{3-2}{3}, \frac{80-30}{50}\right) = \mathbf{(0.333, \; 1.000)}$
  * $C' = \left(\frac{5-2}{3}, \frac{35-30}{50}\right) = \mathbf{(1.000, \; 0.100)}$
  * $q' = \left(\frac{4-2}{3}, \frac{32-30}{50}\right) = \mathbf{(0.667, \; 0.040)}$
  * **(c) Scaled Euclidean Distances:**
  * $d(q', A') = \sqrt{(0.667 - 0)^2 + (0.040 - 0)^2} = \sqrt{0.4449 + 0.0016} = \sqrt{0.4465} \approx \mathbf{0.668}$
  * $d(q', C') = \sqrt{(0.667 - 1.0)^2 + (0.040 - 0.100)^2} = \sqrt{(-0.333)^2 + (-0.060)^2} = \sqrt{0.1109 + 0.0036} = \sqrt{0.1145} \approx \mathbf{0.338}$
  * **Result:** In the scaled space, **Point $C'$ is the true nearest neighbor** ($0.338 < 0.668$). Unscaled distance allowed feature $x_2$ to dominate, incorrectly selecting $A$. `[L08, L14]`
  
  ---
### Problem 5 Solution
  * **(a) Euclidean Distances from $q = (2, 3)$:**
  * $d(q, A) = \sqrt{(2 - 1)^2 + (3 - 1)^2} = \sqrt{1 + 4} = \sqrt{5} \approx \mathbf{2.236}$ [C]
  * $d(q, B) = \sqrt{(2 - 2)^2 + (3 - 2)^2} = \sqrt{0 + 1} = \mathbf{1.000}$ [C]
  * $d(q, C) = \sqrt{(2 - 3)^2 + (3 - 4)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$ [S]
  * $d(q, D) = \sqrt{(2 - 4)^2 + (3 - 3)^2} = \sqrt{4 + 0} = \mathbf{2.000}$ [S]
  * $d(q, E) = \sqrt{(2 - 1)^2 + (3 - 4)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$ [T]
  * $d(q, F) = \sqrt{(2 - 2)^2 + (3 - 5)^2} = \sqrt{0 + 4} = \mathbf{2.000}$ [T]
  * **(b) $k = 1$ Prediction:**
  * Closest point is $B$ ($d = 1.000$) $\implies \mathbf{\text{Class } C \quad (\text{Circle})}$.
  * **(c) $k = 3$ Prediction:**
  * The three closest points are $B$ (1.000, $C$), and the tied pair $C$ (1.414, $S$) and $E$ (1.414, $T$).
  * Neighborhood: $\{B (\text{Circle}), C (\text{Square}), E (\text{Triangle})\}$.
  * **Voting Result:** $1 \text{ Circle}, 1 \text{ Square}, 1 \text{ Triangle}$. 
  * A **three-way tie occurs**. While odd $k$ prevents deadlocks in binary classification, it does not guarantee a unique winner in multiclass settings ($C \ge 3$). `[L14]`
  
  ---
### Problem 6 Solution
  * **(a) Euclidean Distances from $q = (2, 3)$:**
  * $d(q, A) = \sqrt{(2 - 1)^2 + (3 - 2)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$
  * $d(q, B) = \sqrt{(2 - 2)^2 + (3 - 1)^2} = \sqrt{0 + 4} = \mathbf{2.000}$
  * $d(q, C) = \sqrt{(2 - 3)^2 + (3 - 4)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$
  * $d(q, D) = \sqrt{(2 - 4)^2 + (3 - 2)^2} = \sqrt{4 + 1} = \sqrt{5} \approx \mathbf{2.236}$
  * Three nearest neighbors are **$A, C,$ and $B$** ($1.414, 1.414, 2.000$).
  * **(b) Unweighted $3$-NN Regression:**
  $$\hat{y} = \frac{y_A + y_C + y_B}{3} = \frac{3.0 + 6.0 + 2.5}{3} = \frac{11.5}{3} \approx \mathbf{3.833 \quad (\$383{,}333)}$$
  * **(c) Inverse-Distance Weighted $3$-NN Regression ($w_i = 1/d_i$):**
  * $w_A = \frac{1}{\sqrt{2}} \approx 0.7071, \quad w_C = \frac{1}{\sqrt{2}} \approx 0.7071, \quad w_B = \frac{1}{2.0} = 0.5000$
  * $\sum w_i = 0.7071 + 0.7071 + 0.5000 = 1.9142$
  $$\hat{y} = \frac{w_A y_A + w_C y_C + w_B y_B}{\sum w_i} = \frac{0.7071(3.0) + 0.7071(6.0) + 0.5000(2.5)}{1.9142}$$
  $$\hat{y} = \frac{2.1213 + 4.2426 + 1.2500}{1.9142} = \frac{7.6139}{1.9142} \approx \mathbf{3.978 \quad (\$397{,}800)}$$ `[L14]`
  
  ---
### Problem 7 Solution
  * **(a) Distances to Point $A(2, 2)$ from $q(0, 0)$:**
  * $L_1 = |2 - 0| + |2 - 0| = 2 + 2 = \mathbf{4.000}$
  * $L_2 = \sqrt{2^2 + 2^2} = \sqrt{8} \approx \mathbf{2.828}$
  * $L_\infty = \max(|2 - 0|, |2 - 0|) = \mathbf{2.000}$
  * **(b) Distances to Point $B(0, 2.5)$ from $q(0, 0)$:**
  * $L_1 = |0 - 0| + |2.5 - 0| = \mathbf{2.500}$
  * $L_2 = \sqrt{0^2 + 2.5^2} = \mathbf{2.500}$
  * $L_\infty = \max(|0 - 0|, |2.5 - 0|) = \mathbf{2.500}$
  * **(c) $1$-NN Under Manhattan ($L_1$):**
  * $d_1(B) = 2.500 < d_1(A) = 4.000 \implies \mathbf{\text{Point } B \implies \text{Blue}}$
  * **(d) $1$-NN Under Chebyshev ($L_\infty$):**
  * $d_\infty(A) = 2.000 < d_\infty(B) = 2.500 \implies \mathbf{\text{Point } A \implies \text{Red}}$
  * *Takeaway:* Changing the metric from $L_1$ to $L_\infty$ flips the classification decision from Blue to Red. `[L14]`
  
  ---
### Problem 8 Solution
  * **(a) 3D Euclidean Distances from $q = (1, 1, 1)$:**
  * $d(q, A) = \sqrt{(1 - 1)^2 + (1 - 0)^2 + (1 - 2)^2} = \sqrt{0 + 1 + 1} = \sqrt{2} \approx \mathbf{1.414}$
  * $d(q, B) = \sqrt{(1 - 2)^2 + (1 - 1)^2 + (1 - 0)^2} = \sqrt{1 + 0 + 1} = \sqrt{2} \approx \mathbf{1.414}$
  * $d(q, C) = \sqrt{(1 - 0)^2 + (1 - 2)^2 + (1 - 2)^2} = \sqrt{1 + 1 + 1} = \sqrt{3} \approx \mathbf{1.732}$
  * $d(q, D) = \sqrt{(1 - 3)^2 + (1 - 1)^2 + (1 - 2)^2} = \sqrt{4 + 0 + 1} = \sqrt{5} \approx \mathbf{2.236}$
  * **(b) Predictions:**
  * **$k = 1$:** Points $A$ and $B$ are tied at distance $1.414$. Because both belong to **Class 0**, the tie is resolved $\implies \mathbf{\text{Class 0}}$.
  * **$k = 3$:** The three nearest neighbors are $\{A, B, C\}$. Tally: $2 \text{ Class 0}, 1 \text{ Class 1} \implies \mathbf{\text{Class 0}}$. `[L14]`
  
  ---
### Problem 9 Solution
  * **(a) Leave-One-Out Evaluation for Each Point:**
  1. *Leave out $P_1(1, 1)$ [Red]:*
     Distances to remaining points: $d(P_2) = 1.0$, $d(P_3) = 2.0$, $d(P_4) = \sqrt{5} \approx 2.236$.
     Nearest is $P_2$ [Blue] $\implies$ Predicts **Blue** (Actual: Red) $\implies \mathbf{\text{Error!}}$
  2. *Leave out $P_2(2, 1)$ [Blue]:*
     Distances: $d(P_1) = 1.0$, $d(P_4) = 2.0$, $d(P_3) = \sqrt{5} \approx 2.236$.
     Nearest is $P_1$ [Red] $\implies$ Predicts **Red** (Actual: Blue) $\implies \mathbf{\text{Error!}}$
  3. *Leave out $P_3(1, 3)$ [Red]:*
     Distances: $d(P_4) = 1.0$, $d(P_1) = 2.0$, $d(P_2) = \sqrt{5} \approx 2.236$.
     Nearest is $P_4$ [Red] $\implies$ Predicts **Red** (Actual: Red) $\implies \mathbf{\text{Correct!}}$
  4. *Leave out $P_4(2, 3)$ [Red]:*
     Distances: $d(P_3) = 1.0$, $d(P_2) = 2.0$, $d(P_1) = \sqrt{5} \approx 2.236$.
     Nearest is $P_3$ [Red] $\implies$ Predicts **Red** (Actual: Red) $\implies \mathbf{\text{Correct!}}$
  * **(b) Compute LOOCV Error Rate:**
  $$\text{LOOCV Error Rate} = \frac{\text{Number of Misclassifications}}{N} = \frac{2}{4} = \mathbf{0.500 \quad (50.0\%)}$$ `[L02, L14]`
  
  ---
### Problem 10 Solution
  * **(a) Midpoint $M$:**
  $$M = \left( \frac{2 + 6}{2}, \; \frac{2 + 4}{2} \right) = \left( \frac{8}{2}, \; \frac{6}{2} \right) = \mathbf{(4.000, \; 3.000)}$$
  * **(b) Slope $m_{AB}$:**
  $$m_{AB} = \frac{4 - 2}{6 - 2} = \frac{2}{4} = \mathbf{0.500}$$
  * **(c) Perpendicular Bisector Equation:**
  * The perpendicular slope is the negative reciprocal:
    $$m_\perp = -\frac{1}{m_{AB}} = -\frac{1}{0.500} = \mathbf{-2.000}$$
  * Using point-slope form through midpoint $(4, 3)$:
    $$x_2 - 3.0 = -2.0(x_1 - 4.0) \implies x_2 - 3.0 = -2.0x_1 + 8.0 \implies \mathbf{x_2 = -2.000x_1 + 11.000}$$
    *(or in standard linear form: $2x_1 + x_2 - 11 = 0$).*
  * **(d) Classify Query $(2, 5)$ via Boundary Evaluation:**
  Substitute $(x_1 = 2, x_2 = 5)$ into $2x_1 + x_2 - 11$:
  $$2(2) + 5 - 11 = 4 + 5 - 11 = -2.000 < 0$$
  Because Point $A(2, 2)$ yields $2(2) + 2 - 11 = -5.000 < 0$, the query point falls on the same side as Point $A$.  
  **Prediction:** $\mathbf{\text{Class 1}}$. `[L13, L14]`
  
  ---