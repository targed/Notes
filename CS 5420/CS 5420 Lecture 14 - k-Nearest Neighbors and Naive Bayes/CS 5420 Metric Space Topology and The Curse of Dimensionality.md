## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Lazy Learning Complexities in $k$-NN:**
  * **Training Phase:** $\mathcal{O}(1)$ time complexity. There is no parameter optimization or loss minimization; the algorithm simply loads the design matrix $X$ and label vector $y$ into memory.
  * **Inference Phase:** $\mathcal{O}(n \cdot d)$ time complexity per query point. Evaluating an unobserved instance requires an exhaustive linear scan calculating $n$ $d$-dimensional distances.
  * **Space Complexity:** $\mathcal{O}(n \cdot d)$. The model cannot discard the training data; the entire dataset must remain permanently resident in memory to serve predictions.
  2. **Why Even Values of $k$ Are Avoided in Binary Classification:**
  * In a two-class problem ($C = 2$), choosing an even neighborhood size (e.g., $k = 2, 4, 6$) allows **$50/50$ voting deadlocks** (e.g., two neighbors vote Class 0 and two vote Class 1).
  * Selecting an **odd integer** ($k \in \{1, 3, 5, 7, \dots\}$) mathematically guarantees a strict majority vote, eliminating the need for arbitrary tie-breaking heuristics.
  3. **The Cover-Hart Asymptotic Error Bound ($R^* = 0.08$):**
  * By the Cover-Hart Theorem (1967), the asymptotic risk of a $1$-Nearest Neighbor classifier is bounded by:
   $$R_{1\text{NN}} \le 2 R^* (1 - R^*)$$
  * Substituting $R^* = 0.08$:
   $$R_{1\text{NN}} \le 2(0.08)(1 - 0.08) = 2(0.08)(0.92) = \mathbf{0.1472 \quad (14.72\%)}$$
  * As sample size $n \to \infty$, the error rate of a simple $1$-NN classifier is guaranteed to be under $14.72\%$, without making any parametric assumptions about the underlying distribution.
  
  ---
## 2. "Nearest" Depends on the Distance (Slide 9)
  
  Slide 9 introduces a foundational topological concept in machine learning:
  
  $$\mathbf{\text{"Same pair of points: Euclidean 5, Manhattan 7, Chebyshev 4. 'Nearest' is a choice, not a fact."}}$$
  
  ```
                    Evaluating One Pair Across Three Metrics (Slide 9)
    y-coordinate
        ▲
      3 │                                  ● Point y = (4, 3)
        │                                / │
        │                Euclidean = 5 /   │
        │                            /     │ Manhattan = 4 + 3 = 7
        │                          /       │ (Grid Taxicab Route)
        │                        /         │
        │   Chebyshev = max(4, 3) = 4      │
      0 ┼──────────────────────────────────●────────────────────────▶ x-coordinate
        0        1        2        3       4
      Point x = (0, 0)
  ```
  
  ---
### A. Mathematical Definition of a Metric Space
  To serve as a valid distance function in $k$-NN, a function $d(x, y): \mathcal{X} \times \mathcal{X} \to \mathbb{R}$ must satisfy the **four metric axioms**:
  1. **Non-negativity:** $d(x, y) \ge 0$ for all $x, y$.
  2. **Identity of Indiscernibles:** $d(x, y) = 0 \iff x = y$.
  3. **Symmetry:** $d(x, y) = d(y, x)$.
  4. **Triangle Inequality:** $d(x, z) \le d(x, y) + d(y, z)$ for all $x, y, z$.
  
  ---
### B. The Minkowski Metric Family ($L_p$ Norms)
  Slide 9 notes: *"sklearn default: Minkowski distance with $p = 2$, i.e., Euclidean."*
  
  The **Minkowski Distance** generalizes metric spaces across positive real powers $p \ge 1$:
  
  $$\mathbf{D_p(x, y) = \|x - y\|_p = \left( \sum_{j=1}^d |x_j - y_j|^p \right)^{1/p}}$$
  
  Slide 9 compares the three canonical integer instantiations:
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Manhattan Distance (p = 1, L₁ Norm, Taxicab / City-Block):          │
  │    D₁(x, y) = ∑_{j=1}ᵈ |x_j - y_j|                                     │
  │    • For (0, 0) to (4, 3): |4 - 0| + |3 - 0| = 4 + 3 = 7               │
  │    • Measures distance along orthogonal grid axes.                     │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Euclidean Distance (p = 2, L₂ Norm, Straight-Line / Pythagorean):   │
  │    D₂(x, y) = √( ∑_{j=1}ᵈ (x_j - y_j)² )                               │
  │    • For (0, 0) to (4, 3): √( 4² + 3² ) = √( 16 + 9 ) = √25 = 5        │
  │    • Default metric; invariant to coordinate rotations.                │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. Chebyshev Distance (p ──▶ ∞, L_∞ Norm, Chessboard Maximum):         │
  │    D_∞(x, y) = lim_{p ──▶ ∞} ( ∑ |x_j - y_j|ᵖ )^(1/p) = max_j |x_j - y_j|│
  │    • For (0, 0) to (4, 3): max(|4 - 0|, |3 - 0|) = max(4, 3) = 4       │
  │    • Measures distance by the single worst-case coordinate deviation.  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 3. Unit Ball Geometries: What Does a "Circle" Look Like? (Slide 9)
  
  Slide 9 plots the loci of all points at unit distance ($D_p(x, \mathbf{0}) = 1$) from the origin:
  
  $$\mathbf{\text{"All points at distance 1: the 'circle'."}}$$
  
  ```
                       The Unit Ball Shapes in ℝ² (Slide 9)
                                       y
                                       ▲
                                     1 │      ┌───────────────┐  Chebyshev (L_∞)
                                       │     /│\              │  (Axis-Aligned Square)
                                       │    / │ \             │
                                       │   /  │  \            │
                                       │  /╭──┴──╮\           │  Euclidean (L₂)
                                       │ /╭╯  │  ╰╮\          │  (Smooth Circle)
                                       │ ││   │   ││          │
   -1                                  │ ││   ┼───┼│──────────┼───────────────▶ x
  ───────┬─────────────────────────────┼─┴┴───┼───┴┴──────────┼───────────────┬───────
        -1                             │ ││   │   ││          │               1
                                       │ \╰╮  │  ╭╯/          │  Manhattan (L₁)
                                       │  \╰──┬──╯/           │  (Rotated Diamond)
                                       │   \  │  /            │
                                       │    \ │ /             │
                                       │     \│/              │
                                    -1 │      └───────────────┘
                                       ▼
  ```
### Geometric Interpretations:
  1. **Euclidean Ball ($L_2$):** A smooth, isotropic circle. Every direction from the origin has an identical radius. Rotating the coordinate axes by angle $\theta$ does not change the distance between any points.
  2. **Manhattan Ball ($L_1$):** A diamond (cross-polytope) with sharp vertices at $( \pm 1, 0 )$ and $( 0, \pm 1 )$. Stepping diagonally costs more than stepping parallel to an axis ($|1| + |1| = 2 > \sqrt{2}$).
  3. **Chebyshev Ball ($L_\infty$):** An axis-aligned square with corners at $( \pm 1, \pm 1 )$. Moving diagonally costs no more than moving along a single axis, since only the maximum component matters: $\max(|1|, |1|) = 1$.
  
  Slide 9 emphasizes the practical takeaway:
  $$\mathbf{\text{Changing your metric } p \text{ re-orders the nearest neighbors, fundamentally altering the decision boundary.}}$$
  
  ---
## 4. The Curse of Dimensionality in Metric Spaces (The Graduate Fill-In)
  
  Why does $k$-NN, which performs well in 2D and 3D, degrade in high dimensions ($d > 50$)?
  
  In graduate-level machine learning, this degradation is driven by two geometric phenomena: **The Empty Space Phenomenon** and **Distance Concentration**.
  
  ---
### Phenomenon 1: The Empty Space Phenomenon (Neighborhood Dilution)
  Assume $n$ training data points are distributed uniformly across a $d$-dimensional unit hypercube:
  $$\mathcal{X} = [0, 1]^d$$
  
  Suppose we want to find the $k$ nearest neighbors to a query point, and those $k$ neighbors represent a fraction $r$ of the total dataset (e.g., $r = \frac{k}{n} = 0.10$, capturing $10\%$ of the data).
  
  To capture a volume fraction $r$, what must be the edge length $e$ of the local sub-cube centered at the query point?
  
  $$\text{Volume} = e^d = r \implies \mathbf{e = r^{1/d}}$$
  
  ```
                       The Neighborhood Inflation Curve
  Edge Length e
      ▲
  1.0 │                                             ╭────────────── e ──▶ 1.0
      │                                         ╭───╯
  0.8 │                             ╭───────────╯  d = 10: e = 0.794 (80% of space!)
      │                      ╭──────╯
  0.6 │                ╭─────╯
      │           ╭────╯
  0.4 │        ╭──╯
      │     ╭──╯
  0.2 │  ╭──╯  d = 1: e = 0.100 (Truly local: 10% of space)
      │ ╭╯
  0.0 ┴─┴────────────────────────────────────────────────────────────▶ Dimensions d
      1    5    10    20    30    40    50    60    70    80    90   100
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Dimensional Dilution Trap:                                         │
  │                                                                        │
  │ • In d = 1 Dimension:                                                  │
  │   e = (0.10)^(1/1) = 0.10  ──▶ Neighborhood spans 10% of feature range.│
  │                                (Genuinely local estimate!)             │
  │                                                                        │
  │ • In d = 10 Dimensions:                                                │
  │   e = (0.10)^(1/10) ≈ 0.794 ──▶ Must span 79.4% of EVERY feature axis! │
  │                                                                        │
  │ • In d = 100 Dimensions:                                               │
  │   e = (0.10)^(1/100) ≈ 0.977 ──▶ Must span 97.7% of the ENTIRE space!  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **The Crisis:** To find even $10\%$ of the nearest neighbors in 100 dimensions, your "local" neighborhood must span **almost the entire coordinate volume of the universe**. 
  * The points captured are not "near" the query point; they are located on opposite ends of the space.
  * The local smoothness assumption underlying non-parametric estimation breaks down.
  
  ---
### Phenomenon 2: Distance Concentration (Beyer et al., 1999)
  What happens to Euclidean distances between random vectors as dimensionality increases?
  
  Let $X \in \mathbb{R}^d$ contain independent coordinates. Beyer et al. (1999) proved that under mild distribution conditions, the difference between the distance to the farthest point ($D_{\max}$) and the nearest point ($D_{\min}$) becomes negligible compared to the distance itself:
  
  $$\mathbf{\lim_{d \to \infty} \frac{D_{\max} - D_{\min}}{D_{\min}} \longrightarrow 0}$$
  
  ```
                          Distance Concentration in High Dimensions
         Low-Dimensional Space (d = 2)                   High-Dimensional Space (d = 500)
    Density of Pairwise Distances                   Density of Pairwise Distances
      ▲                                               ▲                 Narrow Spike
      │   ╭─────────╮                                 │                      |
      │ ╭─╯         ╰─╮                               │                     / \
      │╭╯             ╰─╮                             │                    /   \
      └────────────────────────▶ Distance             └───────────────────/─────\────────▶ Distance
       D_min                  D_max                                      D_min ≈ D_max
       Clear distinction between                      Every point is roughly EQUIDISTANT
       "near" and "far" instances.                    from every other point!
  ```
  
  * In high dimensions, pairwise distance distributions concentrate into a narrow spike.
  * The concept of a "nearest neighbor" becomes numerically meaningless because **all points become roughly equidistant from the query**. Small measurement noise can flip which point is selected as nearest.
  
  ---
## 5. Why Feature Scaling Is Mandatory for Distance Metrics
  
  Slide 9 proves: *"Nearest depends on the distance."*
  
  Recall the Euclidean distance formula across $d$ dimensions:
  
  $$D_2(x, y) = \sqrt{\sum_{j=1}^d (x_j - y_j)^2}$$
  
  * If Feature 1 (`Income`) spans $[20{,}000, \; 200{,}000]$ and Feature 2 (`Age`) spans $[18, \; 80]$, a $1\%$ change in income contributes $(1{,}800)^2 = \mathbf{3{,}240{,}000}$ to the sum.
  * A $50\%$ change in age contributes $(30)^2 = \mathbf{900}$.
  * **The Failure Mode:** The distance metric collapses to a 1-dimensional line determined entirely by the largest feature. `Age` has no influence on neighbor selection.
  * **The Absolute Rule:** For distance-based estimators ($k$-NN, $k$-Means, SVMs), **features must always be standardized ($\mu = 0, \sigma = 1$) via `StandardScaler` inside a leak-free `Pipeline`**.
  
  ---
## Summary Review Questions for Section 2
  
  1. *Given two points $x = (1, 2)$ and $y = (4, 6)$, calculate their Manhattan distance ($L_1$), Euclidean distance ($L_2$), and Chebyshev distance ($L_\infty$).*
  2. *Explain the "Empty Space Phenomenon": why does finding the $10\%$ nearest neighbors in a 50-dimensional unit hypercube force the neighborhood to span over $95\%$ of each coordinate axis?*
  3. *What does the distance concentration limit $\lim_{d \to \infty} \frac{D_{\max} - D_{\min}}{D_{\min}} \to 0$ imply about the ability of $k$-NN to distinguish between similar and dissimilar samples in high-dimensional spaces?*
  
  ---