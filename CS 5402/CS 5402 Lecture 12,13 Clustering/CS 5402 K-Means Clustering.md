## 1. The Algorithm (Slides 37–40, 47-54)
**Type:** **Hard Clustering** (Partitioning). Every point belongs to exactly one cluster.
**Goal:** Partition $N$ data points into $K$ non-overlapping clusters to minimize the "within-cluster" variance.

**The Iterative Process:**
1.  **Initialization:** Pick $K$ random points as the initial **Centroids** ($\mu$).
2.  **Assignment (E-Step):** For every data point $x_i$, calculate the distance to all $K$ centroids. Assign $x_i$ to the closest one.
  *   *Concept:* This creates **Voronoi Regions** (polygons of influence) around the centroids.
3.  **Update (M-Step):** Calculate the **Mean** (average) of all points currently assigned to cluster $k$. Move the centroid to this new mean.
  *   $\mu_k = \frac{1}{|C_k|} \sum_{x \in C_k} x$
4.  **Repeat:** Loop steps 2 and 3 until **Convergence** (Centroids stop moving or assignments don't change).
- ## 2. The Mathematical Objective (Slides 42–46)
  K-Means is an optimization problem. It tries to minimize a specific Cost Function called **Sum of Squared Errors (SSE)** or **Inertia**.
  
  **The Cost Function $J$ (or $L$):**
  $$ J = \sum_{k=1}^{K} \sum_{x_i \in C_k} ||x_i - \mu_k||^2 $$
  *   *Translation:* Sum up the squared Euclidean distances between every point and its assigned cluster center.
  *   *Goal:* We want clusters to be "tight" (low variance).
  
  **Optimization Method: Coordinate Descent**
  *   You cannot solve for the assignments and the centroids simultaneously (it's a "Chicken and Egg" problem).
  *   **Solution:** We fix one, optimize the other, then switch.
    *   **E-Step (Expectation):** Fix Centroids $\to$ Optimize Assignments (minimize distance).
    *   **M-Step (Maximization):** Fix Assignments $\to$ Optimize Centroids (minimize variance by moving to the mean).
  *   **Convergence Guarantee:** Since both steps are guaranteed to reduce (or maintain) the Cost Function $J$, and $J$ is bounded below by 0, the algorithm **must** converge. It cannot loop forever.
  
  ---
- ## 3. The Initialization Problem (Local Optima)
  **(Slides 55–62)**
  
  While K-Means guarantees convergence, it does **not** guarantee finding the **Global Minimum**.
  *   **The Issue:** The Cost Function $J$ is **non-convex**. It has many "valleys" (Local Minima).
  *   **The Cause:** If you pick bad starting points (e.g., two centroids in the same natural cluster), the algorithm might get "stuck" in a bad configuration that splits a natural cluster in half and merges two others.
  
  **The Slide 69 Paradox (Q1):**
  *   *Scenario:* You run K-Means with $K=3$ and get Cost=100. You run it with $K=5$ and get Cost=150.
  *   *Theoretical Truth:* Increasing $K$ should *always* reduce (or equal) the cost. (Putting a centroid on top of every point gives Cost=0).
  *   *Conclusion:* If Cost($K=5$) > Cost($K=3$), the $K=5$ run got stuck in a terrible **Local Minimum**.
  *   **The Solution:** **Multiple Random Initializations.** Run the algorithm 50 times with different starting seeds and keep the result with the lowest Cost $J$.
  
  ---
- ## 4. Choosing K: The Elbow Method (Slides 65–68)
  Since K-Means cannot determine the number of clusters automatically, we must pick $K$.
  
  **The Method:**
  1.  Run K-Means for $K=1, 2, 3, \dots, 10$.
  2.  Plot the Cost ($J$) vs. $K$.
  3.  **Observation:** The cost always goes down.
    *   When $K <$ True Clusters, splitting a cluster drops cost massively.
    *   When $K >$ True Clusters, splitting a cluster only drops cost slightly.
  4.  **The Elbow:** Look for the point where the curve bends (the rate of decrease slows down). That is the optimal $K$.
  
  ---
- ## 5. Weaknesses of K-Means (Slides 51, 54, 70–73)
  K-Means makes strong assumptions about the data geometry. It fails when these assumptions are violated.
  
  1.  **Assumption: Spherical Clusters.**
    *   K-Means uses Euclidean distance, which spreads out in circles (spheres). It fails on **non-convex** shapes (e.g., a "U" shape or concentric circles/spirals).
  2.  **Assumption: Similar Densities.**
    *   It struggles if one cluster is very dense and another is very sparse. The centroid of the sparse cluster might get pulled toward the dense one.
  3.  **Assumption: Similar Sizes.**
    *   It tends to produce clusters of roughly equal physical size. It will split a large cluster to "feed" a smaller adjacent one.
  4.  **Sensitivity to Outliers:**
    *   Since it minimizes **Squared** error (Mean), a single outlier far away will pull the centroid drastically toward it. (Solution: K-Medoids, which uses the median).
  
  ***
- # Self-Check Questions
  
  **1. Convergence Logic:**
  Does K-Means always find the best possible clustering solution?
  *   *Answer:* **No.** It always converges, but often to a **Local Minimum**. This is why we use restarts.
  
  **2. Complexity Analysis:**
  If you have $N$ points, $K$ clusters, $d$ dimensions, and it takes $t$ iterations to converge. What is the Big-O complexity?
  *   *Answer:* **$O(t \cdot K \cdot N \cdot d)$**. It is linear with respect to the number of data points ($N$), making it very fast/efficient for large datasets compared to Hierarchical Clustering ($O(N^2)$ or $O(N^3)$).
  
  **3. Geometric Failure:**
  Why does K-Means fail to cluster two concentric circles (a donut shape)?
  *   *Answer:* K-Means draws straight lines (Voronoi boundaries) to separate clusters. You cannot separate an inner circle from an outer ring using straight lines.
  
  ***