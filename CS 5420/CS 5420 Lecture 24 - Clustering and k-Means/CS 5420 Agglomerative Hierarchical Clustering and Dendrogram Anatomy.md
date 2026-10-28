## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Proof that the Update Step Minimizes Within-Cluster Sum of Squares:**
   * For an individual cluster $C_j$, the local inertia objective is:
     $$J_j(\mu_j) = \sum_{x_i \in C_j} \|x_i - \mu_j\|_2^2$$
   * Differentiating with respect to the centroid vector $\mu_j$:
     $$\nabla_{\mu_j} J_j = -2 \sum_{x_i \in C_j} (x_i - \mu_j) = \mathbf{0}$$
     $$\sum_{x_i \in C_j} x_i - \sum_{x_i \in C_j} \mu_j = \mathbf{0} \implies \sum_{x_i \in C_j} x_i - |C_j|\mu_j = \mathbf{0}$$
     $$\mathbf{\mu_j^* = \frac{1}{|C_j|}\sum_{x_i \in C_j} x_i}$$
   * The second derivative matrix (Hessian) is:
     $$\nabla_{\mu_j}^2 J_j = 2 |C_j| I \succ \mathbf{0}$$
   * Because the Hessian is strictly positive definite, setting $\mu_j$ to the arithmetic mean is the **unique global minimizer** of the cluster's squared distance.
  2. **1D Tracing: Points $\{1, 2, 9, 12\}$ with Seeds $\mu_1 = 2.0, \mu_2 = 10.0$:**
   * **Assignment Step:**
     * Point $1$: $|1 - 2| = 1 < |1 - 10| = 9 \implies \mathbf{C_1}$
     * Point $2$: $|2 - 2| = 0 < |2 - 10| = 8 \implies \mathbf{C_1}$
     * Point $9$: $|9 - 2| = 7 > |9 - 10| = 1 \implies \mathbf{C_2}$
     * Point $12$: $|12 - 2| = 10 > |12 - 10| = 2 \implies \mathbf{C_2}$
     * Partitions: $C_1 = \{1, 2\}, \quad C_2 = \{9, 12\}$
   * **Centroid Update Step:**
     $$\mu_1^{(1)} = \frac{1 + 2}{2} = \mathbf{1.500}, \quad \mu_2^{(1)} = \frac{9 + 12}{2} = \mathbf{10.500}$$
  3. **Why Inertia Cannot Select the Number of Clusters $k$:**
   * Inertia $J = \sum_{j=1}^k \sum_{x_i \in C_j} \|x_i - \mu_j\|_2^2$ is monotonically non-increasing in $k$.
   * At the extreme limit $k = n$, every data point becomes its own centroid ($\mu_i = x_i$). The distance from every point to its assigned center is zero:
     $$J(k = n) \equiv \mathbf{0.0}$$
   * Directly minimizing $J$ with respect to $k$ trivially selects $k = n$ (a lookup table of individual points with zero clustering utility).
  
  ---
## 2. The Hierarchical Paradigm: Resolving "Clusters Within Clusters" (Slides 16–17)
  
  Slide 16 presents a fundamental limitation of flat clustering algorithms like $k$-means:
  
  $$\mathbf{\text{"120 points, no labels. Make the case for } k = 2\text{, then for } k = 4\text{. Which one is right?"}}$$
  $$\mathbf{\text{"}k\text{-means forces one } k \text{ up front. Sometimes the honest answer is: both."}}$$
  
  ```
                       The Multi-Scale Dilemma (Slide 16)
  Feature x₂
      ▲
      │       ● ●●             ● ●●
      │     ● ●●● ●          ● ●●● ●
      │       ● ●●             ● ●●
      │                                       Is it 2 Macro-Clusters?
      │                                       Or 4 Micro-Clusters?
      │       ● ●●             ● ●●           k-means forces you to choose ONE k
      │     ● ●●● ●          ● ●●● ●          before fitting!
      │       ● ●●             ● ●●
      └────────────────────────────────▶ Feature x₁
  ```
  
  Slide 17 delivers the architectural solution:
  
  $$\mathbf{\text{"A tree of nested groups. Cut it high for 2 clusters, low for 4."}}$$
  $$\mathbf{\text{"No } k \text{ up front: build the tree once, then choose } k \text{ by where you cut it."}}$$
  
  ```
                      One Tree, Every k (Slide 17)
         Cut High (Distance = 31)                      Cut Low (Distance = 7)
    x₂                                            x₂
     ▲                                             ▲
     │    ● ●●            ■ ■■                     │    ● ●●            ■ ■■
     │  ● ●●● ●         ■ ■■■ ■                    │  ● ●●● ●         ■ ■■■ ■
     │    ● ●●            ■ ■■                     │    ● ●●            ■ ■■
     │                                             │
     │    ● ●●            ■ ■■                     │    ▲ ▲▲            ◆ ◆◆
     │  ● ●●● ●         ■ ■■■ ■                    │  ▲ ▲▲▲ ▲         ◆ ◆◆◆ ◆
     │    ● ●●            ■ ■■                     │    ▲ ▲▲            ◆ ◆◆
     └────────────────────────▶ x₁                 └────────────────────────▶ x₁
          2 Macro-Clusters                              4 Micro-Clusters
  ```
  
  ---
### Flat Partitions vs. Nested Hierarchies:
  * **Flat Clustering ($k$-Means):** Partitions data into a single, fixed set of $k$ clusters. It assumes all clusters exist at the **same scale of granularity**.
  * **Hierarchical Clustering:** Discards the requirement of fixing $k$ before training. It organizes data into a **continuous nested tree of clusters** where small, tight clusters are successively nested inside larger, broader clusters.
  * **The Operational Advantage:** An engineer fits the model **once**. The resulting tree contains every possible grouping from $k = 1$ to $k = n$. Domain experts can inspect the multi-scale hierarchy and choose the clustering resolution after fitting.
  
  ---
## 3. The Agglomerative Clustering Algorithm (Slide 18)
  
  Slide 18 outlines the bottom-up construction algorithm:
  
  $$\mathbf{\text{"Start: every point is its own cluster. Repeat: merge the two closest clusters. Stop: one cluster."}}$$
  $$\mathbf{n \text{ points }\longrightarrow\text{ } n - 1 \text{ merges. The dendrogram draws each merge at the distance where it happened."}}$$
  
  ```
                  The Agglomerative Bottom-Up Loop (Slide 18)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Step 0: INITIALIZATION                                                 │
  │         Start with n singleton clusters: Cᵢ = {xᵢ} for i = 1, ..., n.  │
  │         Compute initial n × n pairwise distance matrix D.              │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 1: FIND CLOSEST PAIR                                              │
  │         Search distance matrix D for the minimum inter-cluster value:  │
  │                     (A*, B*) = argmin_{A, B} d(A, B)                   │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 2: MERGE                                                          │
  │         Combine clusters A* and B* into a single union cluster:        │
  │                         C_new = A* ∪ B*                                │
  │         Record the merge height: h = d(A*, B*).                        │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 3: UPDATE DISTANCES                                               │
  │         Delete rows/columns for A* and B* in matrix D.                 │
  │         Compute distance from C_new to all remaining clusters using    │
  │         the chosen linkage criterion (Single, Complete, Average, Ward).│
  ├────────────────────────────────────────────────────────────────────────┤
  │ Step 4: TERMINATION                                                    │
  │         Repeat Steps 1–3 exactly n - 1 times until all points are      │
  │         merged into a single root cluster containing all n samples.    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Step-by-Step Visual Example ($n = 6$ points: $A, B, C, D, E, F$) (Slide 18)
  
  ```
                         Spatial Merges vs. Dendrogram Tree (Slide 18)
         Spatial Proximity Merges                            Dendrogram Recording
  x₂                                               Merge Height (Distance)
   ▲          ● F                                      ▲
   │         /                                     2.5 ┼──┐ (Merge 5: F joins {A..E} at d = 2.5)
   │        / Merge 5                             │  │
   │       /                                       2.0 ┼  │
   │   ● B                                             │  │
   │  / Merge 2                                    1.5 ┼  │
   │ ● A                                               │  │
   │   \─────── Merge 4 ───────● D                 1.0 ┼  │   ┌───┐ (Merge 3: D joins {C,E} at d=0.9)
   │                           │ Merge 3               │  │   │   │
   │                           ● C                 0.5 ┼  ├───┤   ├───┐ (Merge 2: A,B at d=0.7)
   │                            \ Merge 1              │  │   │   │   │ (Merge 1: C,E at d=0.3)
   │                             ● E               0.0 ┴──┴───┴───┴───┴───┴──▶ Leaves (Samples)
   └────────────────────────────────▶ x₁                  F   A   B   D   C   E
  ```
  
  ```
  ┌──────┬────────────────────────┬──────────────┬────────────────────────────────────────┐
  │ Step │ Clusters Merged        │ Distance (h) │ Resulting Cluster Population           │
  ├──────┼────────────────────────┼──────────────┼────────────────────────────────────────┤
  │ 1    │ {C} and {E}            │ h₁ = 0.3     │ {C, E}                                 │
  │ 2    │ {A} and {B}            │ h₂ = 0.7     │ {A, B}, {C, E}                         │
  │ 3    │ {D} and {C, E}         │ h₃ = 0.9     │ {A, B}, {C, D, E}                      │
  │ 4    │ {A, B} and {C, D, E}   │ h₄ = 2.4     │ {A, B, C, D, E}                        │
  │ 5    │ {F} and {A, B, C, D, E}│ h₅ = 2.5     │ {A, B, C, D, E, F} (All merged!)       │
  └──────┴────────────────────────┴──────────────┴────────────────────────────────────────┘
  ```
  Notice: For $n = 6$ points, the algorithm executes exactly $n - 1 = \mathbf{5 \text{ merges}}$.
  
  ---
## 4. Reading a Dendrogram: Anatomy & Topology (Slide 19)
  
  Slide 19 details the three geometric components of a hierarchical tree:
  
  $$\mathbf{\text{"Leaves = points. Height = distance. A cut = a clustering."}}$$
  $$\mathbf{\text{"Cut inside the biggest gap: those clusters are the most clearly separated."}}$$
  
  ```
                       Anatomy of a Dendrogram (Slide 19)
    Merge Height (Distance)
       ▲
    20 ┼──────────────────┐
       │                  │
    15 ┼                  │
       │                  │
    10 ┼                  ├────────────────────────┐
       │                  │                        │
       │  - - - - - - - - │ - - - - - - - - - - - -│ - - - - - - - - - ──▶ Cut at height h = 6.5
     5 ┼─────────┬────────┘                        ├────────────┐      Yields k = 3 Clusters!
       │         │       ▲                         │            │
       │         │       │ WIDEST VERTICAL GAP     │            │
       │         │       │ (Best natural cut!)     │            │
     0 ┴───┴─┴─┴─┴─┴──┴──┴─────────────────────────┴─┴──┴─┴──┴──┴─┴─┴──▶ Leaves (Individual Data Points)
  ```
  
  ---
### The Three Structural Rules:
#### 1. Leaves ($h = 0$):
  * Every vertical line at the bottom baseline represents an individual training instance ($x_i$).
  * At height zero, every point is isolated in its own singleton cluster ($k = n$).
#### 2. Height of Horizontal Bars ($h = d(A, B)$):
  * The vertical axis measures the **inter-cluster distance** at the moment two branches merge.
  * Where two branches connect horizontally, the vertical height of that crossbar tells you **how far apart the two clusters were** according to the chosen linkage criterion.
  * High crossbars represent merges between distant clusters; low crossbars represent merges between close points.
#### 3. The Horizontal Cut Rule (Slide 19, Slide 26 Q2):
  * Slicing a dendrogram with a horizontal line at height $h_{\text{cut}}$ produces a flat clustering partition.
  * **The Counting Rule:** The number of clusters $k$ produced by a horizontal cut is **exactly equal to the number of vertical lines the cut crosses**.
  * Slide 26 Q2: *"A horizontal cut crosses 3 vertical lines of a dendrogram. You get..."* $\implies \mathbf{3 \text{ clusters}}$.
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Widest Vertical Gap Heuristic:                                     │
  │                                                                        │
  │ • How do we choose the optimal number of clusters k from a dendrogram? │
  │                                                                        │
  │ • Inspect the vertical lines and identify the LONGEST VERTICAL GAP     │
  │   where no horizontal merge lines occur.                               │
  │                                                                        │
  │ • A large vertical gap means clusters persisted over a wide range of   │
  │   distances before being forced to merge.                              │
  │                                                                        │
  │ • Slicing through this widest gap produces the most natural,           │
  │   cohesive, and well-separated clusters.                               │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. Ultrametric Properties of Hierarchical Trees (The Graduate Fill-In)
  
  Why does a dendrogram provide a valid geometric representation of metric space?
  
  A dendrogram represents an **Ultrametric Space**. For any three data points $x, y, z$, let $d_{\text{tree}}(x, y)$ be the height of their lowest common ancestral node in the dendrogram. The tree distance satisfies the **Ultrametric Inequality**:
  
  $$\mathbf{d_{\text{tree}}(x, y) \le \max\Big( d_{\text{tree}}(x, z), \; d_{\text{tree}}(y, z) \Big)}$$
  
  * **Geometric Consequence:** In an ultrametric tree, every triangle is **isosceles with the two longer sides equal**.
  * If points $A$ and $B$ merge at height $1.0$, and point $C$ merges with cluster $\{A, B\}$ at height $3.0$:
  $$d_{\text{tree}}(A, C) = 3.0 \quad \text{and} \quad d_{\text{tree}}(B, C) = 3.0$$
  * The tree distance approximates the continuous input space with a discrete hierarchical topology.
  
  ---
## Summary Review Questions for Section 3
  
  1. *Why does $k$-means clustering require specifying the parameter $k$ before fitting, while Agglomerative Hierarchical clustering allows the user to select $k$ after fitting?*
  2. *If an agglomerative clustering algorithm executes on a dataset of $n = 250$ observations, exactly how many merge steps are computed to build the complete dendrogram?*
  3. *If a horizontal cut across a dendrogram at height $h = 4.2$ intersects 5 vertical branches, how many clusters are formed, and what does the height of each branch's parent node represent?*
  
  ---