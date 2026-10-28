#### Problem 1 (Direct Mirror of Review Problem C7)
  Points: $1, \; 3, \; 4, \; 10, \; 12, \; 14$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 1.0, \quad \mu_2 = 3.0$$
  Run $k$-means until convergence. Show assignments and centroids after each iteration, identify which points switch clusters, and compute the final inertia $J$.
  
  ---
#### Problem 2 (Multi-Step Migration)
  Points: $0, \; 2, \; 3, \; 8, \; 10$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 0.0, \quad \mu_2 = 2.0$$
  Run $k$-means until convergence. Show all intermediate cluster assignments, centroid updates, and compute the final inertia $J$.
  
  ---
#### Problem 3 (One-Step Rapid Convergence)
  Points: $2, \; 4, \; 10, \; 12, \; 16, \; 20$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 4.0, \quad \mu_2 = 12.0$$
  Run $k$-means until convergence. Show assignments, updated centroids, and compute the final inertia $J$.
  
  ---
#### Problem 4 (Five Points with an Outlier)
  Points: $1, \; 2, \; 5, \; 6, \; 12$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 2.0, \quad \mu_2 = 5.0$$
  Run $k$-means until convergence. Show assignments and centroids after each iteration, and compute the final inertia $J$.
  
  ---
#### Problem 5 ($k = 3$ Clusters in 1-D)
  Points: $1, \; 2, \; 7, \; 8, \; 15, \; 16$.  
  $k = 3$ with initial centroid seeds:
  $$\mu_1 = 1.0, \quad \mu_2 = 7.0, \quad \mu_3 = 15.0$$
  Run $k$-means until convergence. Show all cluster assignments, centroid updates, and compute the final inertia $J$.
  
  ---
#### Problem 6 (Symmetric Centroid Evolution)
  Points: $2, \; 4, \; 6, \; 14, \; 16, \; 18$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 2.0, \quad \mu_2 = 6.0$$
  Run $k$-means until convergence. Show assignments and centroids after each iteration, and compute the final inertia $J$.
  
  ---
#### Problem 7 (Exact Midpoint Tie-Breaking)
  Points: $2, \; 4, \; 6$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 2.0, \quad \mu_2 = 6.0$$
  * **(a)** Calculate the distance from point $4$ to both centroids. What issue arises?
  * **(b)** If ties are broken by assigning to the lower index cluster ($C_1$), find the assignments, updated centroids, and inertia after one iteration.
  * **(c)** If ties are broken by assigning to the higher index cluster ($C_2$), find the assignments, updated centroids, and inertia after one iteration.
  
  ---
#### Problem 8 (Tracking Inertia Reduction Step-by-Step)
  Points: $1, \; 2, \; 3, \; 8, \; 9, \; 10$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 1.0, \quad \mu_2 = 2.0$$
  * **(a)** Execute Iteration 1: find assignments, updated centroids, and compute the intermediate inertia $J^{(1)}$.
  * **(b)** Execute Iteration 2: find assignments, updated centroids, and compute the intermediate inertia $J^{(2)}$.
  * **(c)** Check convergence and state the total reduction in inertia from Iteration 1 to convergence.
  
  ---
#### Problem 9 (Single-Point Isolated Cluster)
  Points: $1, \; 10, \; 11, \; 12$.  
  $k = 2$ with initial centroid seeds:
  $$\mu_1 = 1.0, \quad \mu_2 = 10.0$$
  Run $k$-means until convergence. Show assignments, updated centroids, and compute the final inertia $J$. What is the within-cluster sum of squares for Cluster 1?
  
  ---
#### Problem 10 (Verifying the Centroid Optimality Property)
  Consider the partition $C_1 = \{2, \; 6\}$ and $C_2 = \{8, \; 12\}$.
  * **(a)** Compute the optimal centroids $\mu_1$ and $\mu_2$ for this fixed partition.
  * **(b)** Compute the cluster inertia $J_{C_1}$ using the optimal centroid $\mu_1$.
  * **(c)** Demonstrate mathematically that choosing any other center (e.g., $\mu_1 = 3.0$) strictly increases the inertia of Cluster 1.
  * **(d)** Check if any observation wants to switch clusters under the optimal centroids.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C7
### Standard Formulas & Governing Steps:
  1. **Assignment Rule:**
   $$x_i \in C_j \iff |x_i - \mu_j| < |x_i - \mu_m| \quad \forall m \neq j$$
   *(Midpoint boundary between $\mu_1$ and $\mu_2$ is $\frac{\mu_1 + \mu_2}{2}$).*
  2. **Centroid Update Rule:**
   $$\mu_j = \frac{1}{|C_j|} \sum_{x_i \in C_j} x_i$$
  3. **Inertia / WCSS Formula:**
   $$J = \sum_{j=1}^k \sum_{x_i \in C_j} (x_i - \mu_j)^2$$
  
  ---
### Problem 1 Solution
  Points: $1, \; 3, \; 4, \; 10, \; 12, \; 14$. Initial: $\mu_1 = 1.0, \; \mu_2 = 3.0$.
  
  * **Iteration 1:**
  * Midpoint: $\frac{1.0 + 3.0}{2} = 2.0$. Points $\le 2 \to C_1$; Points $> 2 \to C_2$.
    * Point $1$: $|1 - 1| = 0 < |1 - 3| = 2 \implies \mathbf{C_1}$
    * Point $3$: $|3 - 1| = 2 > |3 - 3| = 0 \implies \mathbf{C_2}$
    * Point $4$: $|4 - 1| = 3 > |4 - 3| = 1 \implies \mathbf{C_2}$
    * Points $10, 12, 14$: closer to $\mu_2 \implies \mathbf{C_2}$
  * Clusters: $C_1 = \{1\}, \quad C_2 = \{3, 4, 10, 12, 14\}$
  * Updated Centroids:
    $$\mu_1 = \frac{1}{1} = \mathbf{1.000}$$
    $$\mu_2 = \frac{3 + 4 + 10 + 12 + 14}{5} = \frac{43}{5} = \mathbf{8.600}$$
  * **Iteration 2:**
  * Midpoint: $\frac{1.0 + 8.6}{2} = 4.800$.
    * Point $1$: closer to $1.0 \implies \mathbf{C_1}$
    * Point $3$: $|3 - 1| = 2.0 < |3 - 8.6| = 5.6 \implies \mathbf{C_1}$ *(Switches to $C_1$!)*
    * Point $4$: $|4 - 1| = 3.0 < |4 - 8.6| = 4.6 \implies \mathbf{C_1}$ *(Switches to $C_1$!)*
    * Point $10$: $|10 - 1| = 9.0 > |10 - 8.6| = 1.4 \implies \mathbf{C_2}$
    * Points $12, 14$: closer to $8.6 \implies \mathbf{C_2}$
  * Clusters: $C_1 = \{1, 3, 4\}, \quad C_2 = \{10, 12, 14\}$
  * Updated Centroids:
    $$\mu_1 = \frac{1 + 3 + 4}{3} = \frac{8}{3} \approx \mathbf{2.667}$$
    $$\mu_2 = \frac{10 + 12 + 14}{3} = \frac{36}{3} = \mathbf{12.000}$$
  * **Iteration 3:**
  * Midpoint: $\frac{2.667 + 12.0}{2} = 7.333$.
    * Points $\{1, 3, 4\} < 7.333 \implies C_1$
    * Points $\{10, 12, 14\} > 7.333 \implies C_2$
  * **Assignments did not change $\implies$ CONVERGED!**
  * **Compute Final Inertia $J$:**
  * $J_{C_1} = (1 - \frac{8}{3})^2 + (3 - \frac{8}{3})^2 + (4 - \frac{8}{3})^2 = (-\frac{5}{3})^2 + (\frac{1}{3})^2 + (\frac{4}{3})^2 = \frac{25 + 1 + 16}{9} = \frac{42}{9} = \frac{14}{3} \approx 4.667$
  * $J_{C_2} = (10 - 12)^2 + (12 - 12)^2 + (14 - 12)^2 = (-2)^2 + 0^2 + 2^2 = 4 + 0 + 4 = 8.000$
  $$\mathbf{J = 4.667 + 8.000 = 12.667}$$
  
  ---
### Problem 2 Solution
  Points: $0, \; 2, \; 3, \; 8, \; 10$. Initial: $\mu_1 = 0.0, \; \mu_2 = 2.0$.
  
  * **Iteration 1:**
  * Midpoint: $\frac{0 + 2}{2} = 1.0$.
  * $C_1 = \{0\}, \quad C_2 = \{2, 3, 8, 10\}$
  * $\mu_1 = \mathbf{0.000}$
  * $\mu_2 = \frac{2 + 3 + 8 + 10}{4} = \frac{23}{4} = \mathbf{5.750}$
  * **Iteration 2:**
  * Midpoint: $\frac{0.0 + 5.75}{2} = 2.875$.
    * Point $2 < 2.875 \implies$ **switches to $C_1$** ($|2 - 0| = 2 < |2 - 5.75| = 3.75$).
    * Point $3 > 2.875 \implies C_2$ ($|3 - 0| = 3 > |3 - 5.75| = 2.75$).
  * $C_1 = \{0, 2\}, \quad C_2 = \{3, 8, 10\}$
  * $\mu_1 = \frac{0 + 2}{2} = \mathbf{1.000}$
  * $\mu_2 = \frac{3 + 8 + 10}{3} = \frac{21}{3} = \mathbf{7.000}$
  * **Iteration 3:**
  * Midpoint: $\frac{1.0 + 7.0}{2} = 4.000$.
    * Point $3 < 4.000 \implies$ **switches to $C_1$** ($|3 - 1| = 2 < |3 - 7| = 4$).
  * $C_1 = \{0, 2, 3\}, \quad C_2 = \{8, 10\}$
  * $\mu_1 = \frac{0 + 2 + 3}{3} = \frac{5}{3} \approx \mathbf{1.667}$
  * $\mu_2 = \frac{8 + 10}{2} = \mathbf{9.000}$
  * **Iteration 4:**
  * Midpoint: $\frac{1.667 + 9.0}{2} = 5.333$.
  * Points $\{0, 2, 3\} < 5.333 \implies C_1$; Points $\{8, 10\} > 5.333 \implies C_2$.
  * **No changes $\implies$ CONVERGED!**
  * **Compute Final Inertia $J$:**
  * $J_{C_1} = (0 - \frac{5}{3})^2 + (2 - \frac{5}{3})^2 + (3 - \frac{5}{3})^2 = \frac{25 + 1 + 16}{9} = \frac{42}{9} \approx 4.667$
  * $J_{C_2} = (8 - 9)^2 + (10 - 9)^2 = 1 + 1 = 2.000$
  $$\mathbf{J = 4.667 + 2.000 = 6.667}$$
  
  ---
### Problem 3 Solution
  Points: $2, \; 4, \; 10, \; 12, \; 16, \; 20$. Initial: $\mu_1 = 4.0, \; \mu_2 = 12.0$.
  
  * **Iteration 1:**
  * Midpoint: $\frac{4.0 + 12.0}{2} = 8.0$.
  * Points $\le 8$: $\{2, 4\} \implies C_1$
  * Points $> 8$: $\{10, 12, 16, 20\} \implies C_2$
  * Updated Centroids:
    $$\mu_1 = \frac{2 + 4}{2} = \mathbf{3.000}$$
    $$\mu_2 = \frac{10 + 12 + 16 + 20}{4} = \frac{58}{4} = \mathbf{14.500}$$
  * **Iteration 2:**
  * Midpoint: $\frac{3.0 + 14.5}{2} = 8.750$.
  * Points $\{2, 4\} < 8.750 \implies C_1$.
  * Points $\{10, 12, 16, 20\} > 8.750 \implies C_2$.
  * **No changes $\implies$ CONVERGED!**
  * **Compute Final Inertia $J$:**
  * $J_{C_1} = (2 - 3)^2 + (4 - 3)^2 = 1 + 1 = 2.000$
  * $J_{C_2} = (10 - 14.5)^2 + (12 - 14.5)^2 + (16 - 14.5)^2 + (20 - 14.5)^2$
    $$= (-4.5)^2 + (-2.5)^2 + (1.5)^2 + (5.5)^2 = 20.25 + 6.25 + 2.25 + 30.25 = 59.000$$
  $$\mathbf{J = 2.000 + 59.000 = 61.000}$$
  
  ---
### Problem 4 Solution
  Points: $1, \; 2, \; 5, \; 6, \; 12$. Initial: $\mu_1 = 2.0, \; \mu_2 = 5.0$.
  
  * **Iteration 1:**
  * Midpoint: $\frac{2 + 5}{2} = 3.5$.
  * Points $< 3.5$: $\{1, 2\} \implies C_1$
  * Points $> 3.5$: $\{5, 6, 12\} \implies C_2$
  * Updated Centroids:
    $$\mu_1 = \frac{1 + 2}{2} = \mathbf{1.500}$$
    $$\mu_2 = \frac{5 + 6 + 12}{3} = \frac{23}{3} \approx \mathbf{7.667}$$
  * **Iteration 2:**
  * Midpoint: $\frac{1.5 + 7.667}{2} = 4.583$.
  * Points $\{1, 2\} < 4.583 \implies C_1$.
  * Points $\{5, 6, 12\} > 4.583 \implies C_2$.
  * **No changes $\implies$ CONVERGED!**
  * **Compute Final Inertia $J$:**
  * $J_{C_1} = (1 - 1.5)^2 + (2 - 1.5)^2 = 0.25 + 0.25 = 0.500$
  * $J_{C_2} = (5 - \frac{23}{3})^2 + (6 - \frac{23}{3})^2 + (12 - \frac{23}{3})^2 = (-\frac{8}{3})^2 + (-\frac{5}{3})^2 + (\frac{13}{3})^2 = \frac{64 + 25 + 169}{9} = \frac{258}{9} = \frac{86}{3} \approx 28.667$
  $$\mathbf{J = 0.500 + 28.667 = 29.167}$$
  
  ---
### Problem 5 Solution ($k = 3$ in 1-D)
  Points: $1, \; 2, \; 7, \; 8, \; 15, \; 16$. Initial: $\mu_1 = 1.0, \; \mu_2 = 7.0, \; \mu_3 = 15.0$.
  
  * **Iteration 1:**
  * Boundaries: $m_{12} = \frac{1 + 7}{2} = 4.0; \quad m_{23} = \frac{7 + 15}{2} = 11.0$.
  * Points $< 4.0$: $\{1, 2\} \implies C_1$
  * Points between $4.0$ and $11.0$: $\{7, 8\} \implies C_2$
  * Points $> 11.0$: $\{15, 16\} \implies C_3$
  * Updated Centroids:
    $$\mu_1 = \frac{1 + 2}{2} = \mathbf{1.500}, \quad \mu_2 = \frac{7 + 8}{2} = \mathbf{7.500}, \quad \mu_3 = \frac{15 + 16}{2} = \mathbf{15.500}$$
  * **Iteration 2:**
  * Boundaries: $m_{12} = \frac{1.5 + 7.5}{2} = 4.5; \quad m_{23} = \frac{7.5 + 15.5}{2} = 11.5$.
  * Points remain in $\{1, 2\}, \{7, 8\}, \{15, 16\} \implies$ **CONVERGED!**
  * **Compute Final Inertia $J$:**
  * $J_{C_1} = (1 - 1.5)^2 + (2 - 1.5)^2 = 0.25 + 0.25 = 0.500$
  * $J_{C_2} = (7 - 7.5)^2 + (8 - 7.5)^2 = 0.25 + 0.25 = 0.500$
  * $J_{C_3} = (15 - 15.5)^2 + (16 - 15.5)^2 = 0.25 + 0.25 = 0.500$
  $$\mathbf{J = 0.500 + 0.500 + 0.500 = 1.500}$$
  
  ---
### Problem 6 Solution
  Points: $2, \; 4, \; 6, \; 14, \; 16, \; 18$. Initial: $\mu_1 = 2.0, \; \mu_2 = 6.0$.
  
  * **Iteration 1:**
  * Midpoint: $\frac{2 + 6}{2} = 4.0$.
  * Point $4$ is tied: distance to $\mu_1$ is $|4-2|=2$; distance to $\mu_2$ is $|4-6|=2$. Breaking tie to $C_1$:
    * $C_1 = \{2, 4\}, \quad C_2 = \{6, 14, 16, 18\}$
  * Updated Centroids:
    $$\mu_1 = \frac{2 + 4}{2} = \mathbf{3.000}$$
    $$\mu_2 = \frac{6 + 14 + 16 + 18}{4} = \frac{54}{4} = \mathbf{13.500}$$
  * **Iteration 2:**
  * Midpoint: $\frac{3.0 + 13.5}{2} = 8.250$.
  * Point $6$ is closer to $3.0$ ($|6-3|=3 < |6-13.5|=7.5$) $\implies$ **Point 6 switches to $C_1$!**
  * $C_1 = \{2, 4, 6\}, \quad C_2 = \{14, 16, 18\}$
  * Updated Centroids:
    $$\mu_1 = \frac{2 + 4 + 6}{3} = \mathbf{4.000}$$
    $$\mu_2 = \frac{14 + 16 + 18}{3} = \mathbf{16.000}$$
  * **Iteration 3:**
  * Midpoint: $\frac{4.0 + 16.0}{2} = 10.0$.
  * Points $\{2, 4, 6\} < 10 \implies C_1$; Points $\{14, 16, 18\} > 10 \implies C_2$.
  * **No changes $\implies$ CONVERGED!**
  * **Compute Final Inertia $J$:**
  * $J_{C_1} = (2 - 4)^2 + (4 - 4)^2 + (6 - 4)^2 = 4 + 0 + 4 = 8.000$
  * $J_{C_2} = (14 - 16)^2 + (16 - 16)^2 + (18 - 16)^2 = 4 + 0 + 4 = 8.000$
  $$\mathbf{J = 8.000 + 8.000 = 16.000}$$
  
  ---
### Problem 7 Solution
  Points: $2, \; 4, \; 6$. Initial: $\mu_1 = 2.0, \; \mu_2 = 6.0$.
  
  * **(a) Distance to Centroids:**
  $$d(4, \mu_1) = |4 - 2| = \mathbf{2.000}, \quad d(4, \mu_2) = |4 - 6| = \mathbf{2.000}$$
  *Issue:* Point $4$ lies exactly on the boundary midpoint between the two centroids.
  * **(b) Tie Break to $C_1$:**
  * $C_1 = \{2, 4\}, \quad C_2 = \{6\}$
  * $\mu_1 = \frac{2 + 4}{2} = \mathbf{3.000}, \quad \mu_2 = \mathbf{6.000}$
  * $J = [(2 - 3)^2 + (4 - 3)^2] + (6 - 6)^2 = [1 + 1] + 0 = \mathbf{2.000}$
  * **(c) Tie Break to $C_2$:**
  * $C_1 = \{2\}, \quad C_2 = \{4, 6\}$
  * $\mu_1 = \mathbf{2.000}, \quad \mu_2 = \frac{4 + 6}{2} = \mathbf{5.000}$
  * $J = (2 - 2)^2 + [(4 - 5)^2 + (6 - 5)^2] = 0 + [1 + 1] = \mathbf{2.000}$
  
  ---
### Problem 8 Solution
  Points: $1, \; 2, \; 3, \; 8, \; 9, \; 10$. Initial: $\mu_1 = 1.0, \; \mu_2 = 2.0$.
  
  * **(a) Iteration 1:**
  * Midpoint: $1.5$.
  * $C_1 = \{1\}, \quad C_2 = \{2, 3, 8, 9, 10\}$
  * $\mu_1 = \mathbf{1.000}, \quad \mu_2 = \frac{2 + 3 + 8 + 9 + 10}{5} = \frac{32}{5} = \mathbf{6.400}$
  * $J^{(1)} = (1 - 1)^2 + [(2 - 6.4)^2 + (3 - 6.4)^2 + (8 - 6.4)^2 + (9 - 6.4)^2 + (10 - 6.4)^2]$
    $$= 0 + [19.36 + 11.56 + 2.56 + 6.76 + 12.96] = \mathbf{53.200}$$
  * **(b) Iteration 2:**
  * Midpoint: $\frac{1.0 + 6.4}{2} = 3.700$.
  * Points $\le 3$: $\{1, 2, 3\} \implies C_1$. Points $\ge 8$: $\{8, 9, 10\} \implies C_2$.
  * $\mu_1 = \frac{1 + 2 + 3}{3} = \mathbf{2.000}, \quad \mu_2 = \frac{8 + 9 + 10}{3} = \mathbf{9.000}$
  * $J^{(2)} = [(1 - 2)^2 + (2 - 2)^2 + (3 - 2)^2] + [(8 - 9)^2 + (9 - 9)^2 + (10 - 9)^2]$
    $$= [1 + 0 + 1] + [1 + 0 + 1] = 2.000 + 2.000 = \mathbf{4.000}$$
  * **(c) Convergence & Reduction:**
  * Midpoint: $\frac{2 + 9}{2} = 5.5$. Assignments do not change $\implies$ Converged at $J = 4.000$.
  $$\text{Total Reduction} = J^{(1)} - J^{(2)} = 53.200 - 4.000 = \mathbf{49.200 \text{ drop in inertia}}$$
  
  ---
### Problem 9 Solution
  Points: $1, \; 10, \; 11, \; 12$. Initial: $\mu_1 = 1.0, \; \mu_2 = 10.0$.
  
  * **Iteration 1:**
  * Midpoint: $\frac{1 + 10}{2} = 5.5$.
  * Points $< 5.5$: $\{1\} \implies C_1$
  * Points $> 5.5$: $\{10, 11, 12\} \implies C_2$
  * Centroids:
    $$\mu_1 = \mathbf{1.000}, \quad \mu_2 = \frac{10 + 11 + 12}{3} = \mathbf{11.000}$$
  * **Iteration 2:**
  * Midpoint: $\frac{1 + 11}{2} = 6.0$.
  * Point $1 < 6 \implies C_1$; Points $\{10, 11, 12\} > 6 \implies C_2$.
  * **No changes $\implies$ CONVERGED!**
  * **Inertia Analysis:**
  * Cluster 1 has a single observation: $J_{C_1} = (1 - 1.0)^2 = \mathbf{0.000}$  
    *(Any singleton cluster contributes exactly zero to inertia).*
  * $J_{C_2} = (10 - 11)^2 + (11 - 11)^2 + (12 - 11)^2 = 1 + 0 + 1 = 2.000$
  $$\mathbf{J = 0.000 + 2.000 = 2.000}$$
  
  ---
### Problem 10 Solution
  Partition: $C_1 = \{2, \; 6\}, \quad C_2 = \{8, \; 12\}$.
  
  * **(a) Optimal Centroids:**
  $$\mu_1^* = \frac{2 + 6}{2} = \mathbf{4.000}, \quad \mu_2^* = \frac{8 + 12}{2} = \mathbf{10.000}$$
  * **(b) Inertia with Optimal Centroid $\mu_1 = 4.0$:**
  $$J_{C_1} = (2 - 4)^2 + (6 - 4)^2 = (-2)^2 + 2^2 = 4 + 4 = \mathbf{8.000}$$
  * **(c) Inertia with Sub-Optimal Centroid $\mu_1 = 3.0$:**
  $$J_{C_1}' = (2 - 3)^2 + (6 - 3)^2 = (-1)^2 + 3^2 = 1 + 9 = \mathbf{10.000}$$
  $$10.000 > 8.000$$
  *Mathematical Property:* Because $J_{C_1}(\mu_1) = 2(\mu_1 - 4)^2 + 8$, the minimum occurs uniquely when $\mu_1 = \text{mean} = 4.0$. Any deviation from the mean strictly increases squared distance.
  * **(d) Reassignment Check:**
  * Midpoint: $\frac{4 + 10}{2} = 7.0$.
  * Points in $C_1$: $2 < 7$ and $6 < 7 \implies$ stay in $C_1$.
  * Points in $C_2$: $8 > 7$ and $12 > 7 \implies$ stay in $C_2$.
  * **No point changes cluster $\implies$ The partition is stable.**
  
  ---