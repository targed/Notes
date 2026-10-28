## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Why Supervised Metrics Are Impossible in Clustering:**
   * In supervised learning, error metrics (Accuracy, MSE, ROC-AUC) evaluate the difference between predictions and observed ground-truth labels: $\mathcal{L}(y_i, \hat{y}_i)$. 
   * In unsupervised clustering, target labels $Y$ do not exist in the data. There is no external oracle to confirm whether an instance was assigned to the "correct" cluster. 
   * Consequently, model quality must be evaluated using **intrinsic geometric proxies**: intra-cluster compactness (e.g., Within-Cluster Sum of Squares) and inter-cluster separation (e.g., centroid margin distance, Silhouette coefficients).
  2. **Why Cosine Distance Is Preferred Over Euclidean Distance in Text Mining:**
   * Euclidean distance $d_2(x, z) = \|x - z\|_2$ is sensitive to **vector magnitude (document length)**. A 100-word summary and a 5,000-word book chapter covering the exact same topic have vastly different term frequency counts, resulting in a large Euclidean distance.
   * Cosine distance evaluates strictly the **angular displacement** between the vectors:
     $$d_{\cos}(x, z) = 1 - \frac{x^T z}{\|x\|_2 \|z\|_2}$$
   * Because it divides by vector norms, it is invariant to document length. Two documents with identical word proportions point in the same direction ($\theta \approx 0 \implies d_{\cos} \approx 0$), correctly grouping them into the same semantic cluster.
  3. **Mathematical Proof of Weight vs. Cylinder Dominance in Unscaled $k$-Means:**
   * Given $\sigma_{\text{weight}} \approx 800\text{ lbs}$ and $\sigma_{\text{cylinders}} \approx 1.5$.
   * Evaluating the squared Euclidean distance between two cars that differ by $400\text{ lbs}$ and $4\text{ cylinders}$:
     $$\Delta_{\text{weight}}^2 = (400)^2 = \mathbf{160{,}000}$$
     $$\Delta_{\text{cylinders}}^2 = (4)^2 = \mathbf{16}$$
     $$\|x_1 - x_2\|_2^2 = 160{,}000 + 16 = 160{,}016$$
   * The cylinder difference contributes only $\frac{16}{160{,}016} \approx \mathbf{0.01\%}$ to the distance metric. 
   * The optimization algorithm clusters almost entirely along the weight axis, projecting the data into an effective 1D space and ignoring cylinder counts entirely.
  
  ---
## 2. The $k$-Means Objective: Inertia / Within-Cluster Sum of Squares (Slide 7)
  
  Slide 7 defines the mathematical objective function that $k$-means optimizes:
  
  $$\mathbf{\text{"Goal: } k \text{ centers, so that every point is close to its own center."}}$$
  $$\mathbf{\text{Objective: } \quad J = \sum_{j=1}^k \sum_{x_i \in C_j} \|x_i - \mu_j\|_2^2}$$
  
  $$\mathbf{\text{"}J\text{ is called inertia, or the within-cluster sum of squares (WCSS)."}}$$
  
  ```
                       The k-Means Objective Geometry (Slide 7)
    Feature x₂
        ▲
        │           x_a ●       ● x_b
        │                \     /
        │                 \   /
        │                  ▼ ▼
        │             ●───▶ ★ μ₁ ◀───●       Cluster C₁
        │                  ▲ ▲
        │                 /   \
        │           x_c ●       ● x_d
        │
        │                                  x_e ●       ● x_f
        │                                       \     /
        │                                        ▼   ▼
        │                                    ●──▶ ★ μ₂ ◀──●       Cluster C₂
        │                                        ▲   ▲
        │                                       /     \
        │                                 x_g ●         ● x_h
        └────────────────────────────────────────────────────────▶ Feature x₁
           J = Sum of squared line lengths connecting points to their centroid
  ```
  
  ---
### A. Formal Notation
  * Dataset of $n$ observations: $\mathcal{D} = \{x_1, x_2, \dots, x_n\} \subset \mathbb{R}^d$.
  * Number of clusters: $k$ (chosen by the user before fitting).
  * Cluster partition: $\mathcal{C} = \{C_1, C_2, \dots, C_k\}$, such that $\bigcup_{j=1}^k C_j = \mathcal{D}$ and $C_a \cap C_b = \emptyset$ for $a \neq b$.
  * Cluster Centroid $\mu_j \in \mathbb{R}^d$: The geometric center of mass of cluster $C_j$.
### B. Matrix Formulation with Binary Assignment Indicators
  To analyze the optimization mechanics, we can write the inertia $J$ using a binary assignment indicator variable $r_{ij} \in \{0, 1\}$:
  
  $$r_{ij} = \begin{cases} 1 & \text{if } x_i \in C_j \quad (\text{Point } i \text{ is assigned to cluster } j) \\ 0 & \text{otherwise} \end{cases}$$
  
  Subject to the constraint that each point is assigned to exactly one cluster: $\sum_{j=1}^k r_{ij} = 1$.
  
  $$\mathbf{J(R, \mu) = \sum_{i=1}^n \sum_{j=1}^k r_{ij} \|x_i - \mu_j\|_2^2}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Optimization Nature of k-Means:                                    │
  │ • Mixed-Integer Non-Convex Optimization:                               │
  │   We must simultaneously optimize continuous centroid coordinates      │
  │   μ ∈ ℝ^(k × d) and discrete binary cluster assignments R ∈ {0, 1}^(n×k)│
  │                                                                        │
  │ • NP-Hard Problem:                                                     │
  │   Finding the global minimum of J over all possible partitions is      │
  │   NP-hard even for k = 2 in general dimension d (Aloise et al., 2009). │
  │                                                                        │
  │ • Lloyd's Algorithm:                                                   │
  │   A coordinate descent heuristic that alternates between optimizing R  │
  │   and optimizing μ, guaranteed to converge to a local minimum.         │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Lloyd’s Algorithm: Alternating Optimization (Slides 8–9)
  
  Slide 8 outlines the standard algorithmic loop:
  
  $$\mathbf{\text{"Two easy steps, repeated until nothing changes."}}$$
  $$\mathbf{\text{"Assign each point to its nearest center; move each center to the mean. Stop when nothing changes."}}$$
  
  ```
                        The Alternating Optimization Cycle
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Step 0: INITIALIZE                                                     │
  │         Select k initial centroid seeds: μ₁, μ₂, ..., μₖ ∈ ℝᵈ.        │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 1: ASSIGNMENT STEP (Optimize R holding μ fixed)                   │
  │         Assign each observation to its closest Euclidean centroid:     │
  │             r_ij = 1  if j = argmin_m ||xᵢ - μ_m||₂²  (else 0)          │
  │         Geometrically partitions ℝᵈ into a Voronoi diagram.           │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 2: UPDATE STEP (Optimize μ holding R fixed)                       │
  │         Recompute each centroid as the empirical arithmetic mean       │
  │         of all points currently assigned to it:                        │
  │                     μⱼ = (1 / |Cⱼ|) ∑_{xᵢ ∈ Cⱼ} xᵢ                     │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 3: CHECK CONVERGENCE                                              │
  │         If no point changes cluster assignment (or ||Δμ|| < ε), STOP.  │
  │         Otherwise, return to Step 1.                                   │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Mathematical Proof: Why the Loop Always Converges (Slide 9)
  
  Slide 9 addresses a core systems and theoretical question:
  
  $$\mathbf{\text{"Each step can only lower } J\text{, the total squared distance. } J \ge 0 \text{ and never goes up, so the loop must stop."}}$$
  $$\mathbf{\text{"}k\text{-means always converges, but only to a local minimum of } J\text{."}}$$
  
  ```
                       The Monotonic Descent of Inertia (Slide 9)
    Inertia J
       ▲
  1686 ┼──● (Initial Random Seeding: J = 1686)
       │   \
  1500 ┼    \
       │     \
  1000 ┼      \
       │       \
   500 ┼        \──● (Iteration 1: J = 333)
       │            \
       │             ●───● (Iteration 2 & 3: J = 163 ──▶ J = 143)
     0 ┴─────────────────┴───────┴───────┴───────▶ Iteration Step
                         1       2       3
         J is bounded from below (J ≥ 0) and strictly non-increasing!
  ```
  
  ---
### Step-by-Step Mathematical Proof of Convergence:
#### 1. The Assignment Step Strictly Decreases (or Conserves) $J$:
  In Step 1, the centroid positions $\mu_1, \dots, \mu_k$ are held fixed. We optimize $J$ with respect to the assignments $r_{ij}$:
  
  $$\min_{r_{i1}, \dots, r_{ik}} \sum_{j=1}^k r_{ij} \|x_i - \mu_j\|_2^2 \quad \text{subject to } r_{ij} \in \{0, 1\}, \; \sum_{j=1}^k r_{ij} = 1$$
  
  * Since points are partitioned independently, the sum is minimized for each observation $x_i$ by setting $r_{ij} = 1$ for whichever centroid $\mu_j$ minimizes the Euclidean distance $\|x_i - \mu_j\|_2^2$.
  * Reassigning an observation to a closer centroid strictly reduces its contribution to $J$. If no observation changes assignment, $J$ remains unchanged.
  * Therefore:
  $$J(R^{(t+1)}, \; \mu^{(t)}) \le J(R^{(t)}, \; \mu^{(t)})$$
#### 2. The Update Step Strictly Decreases (or Conserves) $J$:
  In Step 2, the cluster memberships $R$ are held fixed. We minimize $J$ with respect to the centroid vectors $\mu_j$:
  
  $$\min_{\mu_j} \sum_{x_i \in C_j} \|x_i - \mu_j\|_2^2$$
  
  Differentiating this quadratic function with respect to the vector $\mu_j$ and setting the gradient to zero:
  
  $$\frac{\partial}{\partial \mu_j} \sum_{x_i \in C_j} \|x_i - \mu_j\|_2^2 = -2 \sum_{x_i \in C_j} (x_i - \mu_j) = \mathbf{0}$$
  
  $$\sum_{x_i \in C_j} x_i - \sum_{x_i \in C_j} \mu_j = \mathbf{0} \implies \sum_{x_i \in C_j} x_i - |C_j| \mu_j = \mathbf{0}$$
  
  Solving for $\mu_j$:
  
  $$\mathbf{\mu_j^* = \frac{1}{|C_j|} \sum_{x_i \in C_j} x_i}$$
  
  * Because the objective is convex quadratic in $\mu_j$, setting the centroid to the empirical sample mean is the unique global minimizer of that cluster's sum of squared distances.
  * Any shift in $\mu_j$ toward the empirical mean strictly reduces (or conserves) $J$:
  $$J(R^{(t+1)}, \; \mu^{(t+1)}) \le J(R^{(t+1)}, \; \mu^{(t)})$$
#### 3. Termination via the Monotone Convergence Theorem:
  * Combining both steps:
  $$J(R^{(t+1)}, \; \mu^{(t+1)}) \le J(R^{(t)}, \; \mu^{(t)})$$
  Inertia $J$ is a **weakly monotonically decreasing sequence**.
  * Boundedness: Because squared Euclidean distances are non-negative, the cost function is bounded from below:
  $$J(R, \mu) \ge 0 \quad \forall \; R, \mu$$
  * Finite State Space: A dataset of $n$ points can be partitioned into $k$ clusters in at most **$k^n$ distinct ways**.
  * **Conclusion:** Because $J$ strictly decreases at every step where assignments change, the algorithm cannot visit the same partition twice. In a finite number of steps, the assignment matrix must stop changing ($R^{(t+1)} = R^{(t)}$), guaranteeing that **$k$-means always converges to a local minimum**.
  
  ---
## 5. Tracing $k$-Means by Hand in 1-D (Slide 10)
  
  Slide 10 provides a step-by-step hand calculation on a 1-dimensional dataset ($n = 6$):
  
  $$\mathbf{\text{1-D Data: } [2, \; 4, \; 5, \; 10, \; 11, \; 12], \quad k = 2. \quad \text{Initial Seeds: } \mu_1 = 2.0, \; \mu_2 = 4.0}$$
  
  ```
                       The 1D Data Geometry (Slide 10)
  ───────●───────────●─────●─────────────────────────●─────●─────●───────▶ Coordinate x
         2           4     5                         10    11    12
       (Seed μ₁)   (Seed μ₂)
  ```
  
  ---
### Step-by-Step Iteration Trace:
#### Iteration 1:
  1. **Assign Points to Nearest Seed ($\mu_1 = 2.0, \; \mu_2 = 4.0$):**
   * Point $2$: $|2 - 2| = 0 < |2 - 4| = 2 \implies \mathbf{C_1}$
   * Point $4$: $|4 - 2| = 2 > |4 - 4| = 0 \implies \mathbf{C_2}$
   * Point $5$: $|5 - 2| = 3 > |5 - 4| = 1 \implies \mathbf{C_2}$
   * Point $10$: $|10 - 2| = 8 > |10 - 4| = 6 \implies \mathbf{C_2}$
   * Point $11$: $|11 - 2| = 9 > |11 - 4| = 7 \implies \mathbf{C_2}$
   * Point $12$: $|12 - 2| = 10 > |12 - 4| = 8 \implies \mathbf{C_2}$
   * **Assignments:** $C_1 = \{2\}, \quad C_2 = \{4, 5, 10, 11, 12\}$
  2. **Recompute Centroids (Means):**
   * $\mu_1 = \frac{2}{1} = \mathbf{2.000}$
   * $\mu_2 = \frac{4 + 5 + 10 + 11 + 12}{5} = \frac{42}{5} = \mathbf{8.400}$
  3. **Compute Inertia $J^{(1)}$:**
   * $J_{C_1} = (2 - 2.0)^2 = 0.0$
   * $J_{C_2} = (4 - 8.4)^2 + (5 - 8.4)^2 + (10 - 8.4)^2 + (11 - 8.4)^2 + (12 - 8.4)^2$
     $$= (-4.4)^2 + (-3.4)^2 + (1.6)^2 + (2.6)^2 + (3.6)^2$$
     $$= 19.36 + 11.56 + 2.56 + 6.76 + 12.96 = 53.20$$
   * **Total Inertia:** $J = 0.0 + 53.20 = \mathbf{53.200}$
  
  ---
#### Iteration 2:
  1. **Reassign Points to Updated Centroids ($\mu_1 = 2.000, \; \mu_2 = 8.400$):**
   * Point $2$: $|2 - 2| = 0 < |2 - 8.4| = 6.4 \implies \mathbf{C_1}$
   * Point $4$: $|4 - 2| = 2 < |4 - 8.4| = 4.4 \implies \mathbf{C_1}$
   * **Point $5$:** $|5 - 2| = \mathbf{3.0} < |5 - 8.4| = \mathbf{3.4} \implies \mathbf{C_1}$  
     *(Slide 10 Callout: Point 5 switches from Cluster 2 to Cluster 1 because $3.0 < 3.4$!)*
   * Point $10$: $|10 - 2| = 8 > |10 - 8.4| = 1.6 \implies \mathbf{C_2}$
   * Point $11$: $|11 - 2| = 9 > |11 - 8.4| = 2.6 \implies \mathbf{C_2}$
   * Point $12$: $|12 - 2| = 10 > |12 - 8.4| = 3.6 \implies \mathbf{C_2}$
   * **Assignments:** $C_1 = \{2, 4, 5\}, \quad C_2 = \{10, 11, 12\}$
  2. **Recompute Centroids (Means):**
   * $\mu_1 = \frac{2 + 4 + 5}{3} = \frac{11}{3} \approx \mathbf{3.667}$
   * $\mu_2 = \frac{10 + 11 + 12}{3} = \frac{33}{3} = \mathbf{11.000}$
  3. **Compute Inertia $J^{(2)}$:**
   * $J_{C_1} = (2 - 3.667)^2 + (4 - 3.667)^2 + (5 - 3.667)^2 = (-1.667)^2 + (0.333)^2 + (1.333)^2 \approx 2.778 + 0.111 + 1.778 = 4.667$
   * $J_{C_2} = (10 - 11)^2 + (11 - 11)^2 + (12 - 11)^2 = (-1)^2 + 0^2 + (1)^2 = 1 + 0 + 1 = 2.000$
   * **Total Inertia:** $J = 4.667 + 2.000 = \mathbf{6.667}$  
     *(Inertia dropped drastically from $53.2 \to 6.67$).*
  
  ---
#### Iteration 3:
  1. **Reassign Points to Centroids ($\mu_1 = 3.667, \; \mu_2 = 11.000$):**
   * Distance to $\mu_1 = 3.67$ vs. $\mu_2 = 11.0$:
     * Point $2$: $1.67 < 9.0 \implies C_1$
     * Point $4$: $0.33 < 7.0 \implies C_1$
     * Point $5$: $1.33 < 6.0 \implies C_1$
     * Point $10$: $6.33 > 1.0 \implies C_2$
     * Point $11$: $7.33 > 0.0 \implies C_2$
     * Point $12$: $8.33 > 1.0 \implies C_2$
   * **Assignments:** $C_1 = \{2, 4, 5\}, \quad C_2 = \{10, 11, 12\}$
  2. **Convergence Check:**
   * **Assignments did not change.** Centroids remain $\mu_1 = 3.667, \mu_2 = 11.000$.
   * **The loop terminates.**
  
  ```
                     Summary Table of 1D Trace (Slide 10)
  ┌───────────┬───────────────┬──────────────────┬─────────────────┬─────────────────┐
  │ Iteration │ Cluster 1     │ Cluster 2        │ New μ₁, μ₂      │ Inertia J       │
  ├───────────┼───────────────┼──────────────────┼─────────────────┼─────────────────┤
  │ 1         │ {2}           │ {4, 5, 10, 11, 12}│ 2.00,  8.40     │ 53.200          │
  │ 2         │ {2, 4, 5}     │ {10, 11, 12}     │ 3.67, 11.00     │ 6.667           │
  │ 3         │ {2, 4, 5}     │ {10, 11, 12}     │ No change       │ 6.667 (STOP)    │
  └───────────┴───────────────┴──────────────────┴─────────────────┴─────────────────┘
  ```
  
  ---
## 6. Sensitivity to Initialization & The $k$-Means++ Seeding Strategy (Slide 11)
  
  Slide 11 presents a fundamental failure mode of local optimization:
  
  $$\mathbf{\text{"Same 200 points, } k = 4\text{. Only the starting centers differ."}}$$
  $$\mathbf{\text{"Unlucky seeds: } J = 530\text{. Best of 20 runs: } J = 137\text{. Run } k\text{-means several times; in scikit-learn: n\_init."}}$$
  
  ```
                       The Local Minimum Hazard (Slide 11)
         Unlucky Random Seeding (J = 530)                  Optimal Global Seeding (J = 137)
    x₂                                                x₂
     ▲                                                 ▲
     │       ■ ■■ ■                                    │       ■ ■■ ■
     │     ■ ■■■ ■   ★ μ₁                              │     ■ ■■■ ■  ★ μ₁
     │       ■ ■■ ■                                    │       ■ ■■ ■
     │                  ★ μ₂                           │
     │      ★ μ₃                                       │                    ★ μ₂
     │    ● ●● ●   ▲ ▲▲ ▲                              │    ● ●● ●         ▲ ▲▲ ▲
     │  ● ●●● ●  ▲ ▲▲▲ ▲                               │  ● ●●● ●  ★ μ₃  ▲ ▲▲▲ ▲  ★ μ₄
     └────────────────────────▶ x₁                     └────────────────────────▶ x₁
     Two natural clusters forced into ONE;             Four natural clusters correctly
     one natural cluster split into TWO!               captured by four distinct centroids.
  ```
  
  Because Lloyd's algorithm is a greedy gradient-like procedure, it converges to the local basin of attraction of whichever initial seeds were chosen.
  
  ---
### Mitigations for Local Minima:
#### 1. Multiple Random Restarts (`n_init` in Scikit-Learn):
  * Run the entire algorithm $M$ independent times (default in scikit-learn: `n_init=10`) with different initial seeds.
  * Track the final converged inertia $J^{(m)}$ for each run:
  $$m^* = \arg\min_{m \in \{1, \dots, M\}} J^{(m)}$$
  * Keep the clustering partition corresponding to the **lowest global inertia**.
#### 2. The $k$-Means++ Seeding Algorithm (Arthur & Vassilvitskii, 2007):
  Instead of choosing all $k$ seeds uniformly at random, $k$-means++ spreads initial seeds far apart:
  1. Select the first centroid $\mu_1$ uniformly at random from the dataset $\mathcal{D}$.
  2. For every remaining data point $x_i$, compute $D(x_i)$, the shortest Euclidean distance from $x_i$ to the closest already-chosen centroid:
   $$D(x_i) = \min_{j} \|x_i - \mu_j\|_2$$
  3. Choose the next centroid $\mu_{m+1}$ from the remaining points with probability proportional to the **squared distance**:
   $$P(\mu_{m+1} = x_i) = \frac{D(x_i)^2}{\sum_{l=1}^n D(x_l)^2}$$
  4. Repeat Steps 2–3 until all $k$ centroids are selected.
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The k-Means++ Theoretical Guarantee:                                   │
  │ Points that sit far away from existing centroids have a high           │
  │ probability of being selected as new seeds; points close to existing   │
  │ centroids have near-zero probability.                                  │
  │                                                                        │
  │ • Guarantees that the expected inertia is bounded within               │
  │   𝒪(log k) of the global optimal solution:                             │
  │                      𝔼[J] ≤ 8(ln k + 2) · J_optimal                    │
  │ • Default initialization in scikit-learn: init="k-means++".            │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 7. Model Selection: The Elbow Method (Slide 12)
  
  Slide 12 addresses the hyperparameter question: *How do we choose $k$ in unsupervised settings?*
  
  $$\mathbf{\text{"}k \text{ is your choice. Plot } J \text{ for } k = 1, 2, \dots, 9 \text{ and look for the bend."}}$$
  $$\mathbf{\text{"}J \text{ always drops as } k \text{ grows (} k = n \text{ gives } J = 0\text{), so pick the } k \text{ where the drop slows down."}}$$
  
  ```
                         The Elbow Detection Curve (Slide 12)
    Inertia J
       ▲
  20000 ┼──● k = 1
       │   \
       │    \
  10000 ┼     \
       │      ● k = 2
       │       \
  5000 ┼        \
       │         \
  1000 ┼          ● k = 3
       │           \
       │            ● k = 4  ◀── THE ELBOW (Sharp bend / Diminishing returns!)
   200 ┼─────────────\────────●────────●────────●────────●────────●──▶ Number of Clusters k
       0              1       2        3        4        5        6
  ```
  
  ---
### Mathematical Properties of the Elbow Curve:
  1. **Monotonicity with Respect to $k$:**
   $$\text{Inertia}(k + 1) \le \text{Inertia}(k) \quad \forall \; k \ge 1$$
   Adding an extra centroid provides an additional degree of freedom to reduce squared distance.
  2. **The Extreme Limit ($k = n$):**
   If $k = n$, every observation becomes its own centroid ($\mu_i = x_i$). The distance from every point to its centroid is zero:
   $$J = \sum_{i=1}^n \|x_i - x_i\|_2^2 \equiv \mathbf{0.0}$$
   Therefore, **we cannot minimize $J$ directly to choose $k$**; training inertia will always pick $k = n$.
  3. **The Elbow Heuristic:**
   * For $k < k^*$, adding centroids captures major between-cluster variance, causing $J$ to drop precipitously.
   * For $k > k^*$, adding centroids merely splits natural, cohesive clusters into arbitrary sub-clusters, yielding minor reductions in $J$.
   * We select the parameter $k^*$ located at the **point of maximum curvature (the elbow)**, representing the optimal balance between cluster compactness and model complexity.
  
  ---
## 8. Application: Color Quantization (Slide 13)
  
  Slide 13 presents an applied engineering use case of $k$-means: **Lossy Image Compression via Color Quantization**.
  
  $$\mathbf{\text{"Each pixel is a point (R, G, B). Cluster the colors; repaint each pixel with its center."}}$$
  $$\mathbf{\text{"16 colors instead of 61,072, and the photo still reads. Clustering as compression."}}$$
  
  ```
                       Color Quantization Architecture (Slide 13)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Vector Representation:                                              │
  │    An image of height H and width W is an array of H × W pixels.       │
  │    Each pixel is an unboxed 3D coordinate vector:                      │
  │                    x_i = [Red, Green, Blue]ᵀ ∈ ℝ³                      │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Unsupervised Clustering:                                            │
  │    Fit k-means with k = 16 clusters on the N = H × W color points.     │
  │    The algorithm learns 16 optimal RGB centroid vectors: μ₁, ..., μ₁₆. │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. Quantization / Codebook Mapping:                                    │
  │    Replace each pixel's original color with the RGB values of its      │
  │    nearest cluster centroid: x_i ──▶ μ_{c_i}.                          │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 4. Memory Compression Ratio:                                           │
  │    • Original: 24 bits per pixel (8 bits R + 8 bits G + 8 bits B)      │
  │                Can represent 2²⁴ ≈ 16.7 million distinct colors.       │
  │    • Compressed: Each pixel stores a 4-bit integer index (0 to 15)     │
  │                  pointing to a 16-color lookup table (codebook).       │
  │    • Bitrate drops from 24 bits/pixel ──▶ 4 bits/pixel (6× reduction)! │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 9. Deconstructing the Quick Check Quizzes (Slide 14)
  
  Slide 14 presents three diagnostic conceptual checks:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Q1: 1-D points 1, 3, 8. Start μ₁ = 1, μ₂ = 4.                          │
  │     After one assign + update, μ₂ = …                                  │
  │     A: 4        B: 5.5  ✔      C: 6        D: 8                        │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Q2: With the best clustering for each k, going from k = 3 to k = 6     │
  │     makes J…                                                           │
  │     A: larger   B: smaller or equal  ✔   C: exactly 0   D: same always │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Q3: Why run k-means from several starts (n_init)?                      │
  │     A: to choose k                                                     │
  │     B: each run finds only a local minimum of J  ✔                     │
  │     C: to scale the features                                           │
  │     D: to compute the cluster centers                                  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Step-by-Step Derivations:
  
  * **Q1 (Answer: B — 5.5):**
  * Training points: $\{1, 3, 8\}$. Initial seeds: $\mu_1 = 1, \mu_2 = 4$.
  * **Assignment Step:**
    * Point $1$: $|1 - 1| = 0 < |1 - 4| = 3 \implies \mathbf{C_1}$
    * Point $3$: $|3 - 1| = 2 > |3 - 4| = 1 \implies \mathbf{C_2}$
    * Point $8$: $|8 - 1| = 7 > |8 - 4| = 4 \implies \mathbf{C_2}$
    * Cluster sets: $C_1 = \{1\}, \quad C_2 = \{3, 8\}$.
  * **Update Step for $\mu_2$:**
    $$\mu_2 = \frac{3 + 8}{2} = \frac{11}{2} = \mathbf{5.5}$$
  
  * **Q2 (Answer: B — Smaller or Equal):**
  * Adding cluster centers provides additional degrees of freedom to minimize squared distances. 
  * Any clustering achievable with $k = 3$ can be replicated or improved with $k = 6$ (e.g., by placing redundant centroids directly on top of existing ones or splitting an existing cluster). 
  * Therefore, the global optimal inertia satisfies $J(k=6) \le J(k=3)$. It cannot be larger.
  
  * **Q3 (Answer: B — Each run finds only a local minimum of $J$):**
  * The $k$-means cost function $J$ is non-convex. Lloyd’s algorithm is a greedy descent procedure that is sensitive to initial seed locations.
  * Different starting centroids converge to different local minima (as shown in Slide 11, where an unlucky start produced $J = 530$, while a better start produced $J = 137$).
  * Running multiple restarts (`n_init=10`) explores different basins of attraction and retains the run with the lowest final inertia.
  
  ---
## Summary Review Questions for Section 2
  
  1. *In Lloyd's algorithm, prove that the Update Step $\mu_j = \frac{1}{|C_j|}\sum_{x_i \in C_j} x_i$ is the unique mathematical minimizer of the cluster's within-cluster sum of squares $\sum_{x_i \in C_j} \|x_i - \mu_j\|_2^2$.*
  2. *Given 1-D data points $\{1, 2, 9, 12\}$ and initial centroids $\mu_1 = 2.0, \mu_2 = 10.0$, perform one assignment step and one centroid update step. State the new centroids $\mu_1^{(1)}$ and $\mu_2^{(1)}$.*
  3. *Why is it impossible to select the optimal number of clusters $k$ by minimizing the inertia $J$ directly on the training data?*
  
  ---