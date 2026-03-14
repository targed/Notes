## 1. The Paradigm Shift: Supervised vs. Unsupervised
**Slide Context:** Slides 3–5 contrast training sets with labels $\{(x, y)\}$ vs. those without $\{(x)\}$.

*   **Supervised Learning:**
  *   **Goal:** Predict a target $y$ given features $x$.
  *   **Process:** Function approximation ($y = f(x)$). We have a "Ground Truth" to measure error against.
*   **Unsupervised Learning:**
  *   **Goal:** Discover hidden structure or patterns within $x$.
  *   **Process:** Density estimation or manifold learning.
  *   **The Challenge:** There is no "Ground Truth." Validation is often subjective or relies on internal mathematical consistency (intra-cluster similarity).
  *   **Applications:**
      *   **Data Mining:** Customer segmentation, news categorization.
      *   **Compression:** Representing data by a cluster ID is a form of quantization (reducing information).
      *   **Imputation:** Filling in missing data by looking at similar neighbors.
- ## 2. Defining a "Cluster" (Slide 20-22, 28)
  Mathematically, a cluster is a subset of data points. A good clustering $C = \{C_1, C_2, ... C_k\}$ satisfies two properties:
  1.  **High Intra-cluster Similarity:** Points within $C_i$ are close to each other. (Minimizing variance/scatter).
  2.  **Low Inter-cluster Similarity:** Points in $C_i$ are far from points in $C_j$. (Maximizing separation).
  
  **Key Cluster Properties (Slide 22):**
  *   **Centroid ($\bar{x}_{C_i}$):** The geometric center (mean vector) of the cluster.
    $$ \bar{x}_{C_i} = \frac{1}{|C_i|} \sum_{x \in C_i} x $$
  *   **Scatter Matrix ($S_{C_i}$):** This measures the spread (variance) of the cluster. In advanced algorithms, we often try to minimize the **trace** (sum of diagonal elements) or **determinant** of this matrix.
  
  ---
- ## 3. Distance Metrics: The Geometry of Similarity
  **(Slides 13–19)**
  Clustering is entirely dependent on how you measure "closeness." If you change the metric, you change the clusters.
- ### A. Minkowski Distance ($L_p$ Norm)
  The generalized distance between two vectors $x_i$ and $x_j$ in $d$-dimensions:
  $$ d(x_i, x_j) = \left( \sum_{k=1}^{d} |x_{i,k} - x_{j,k}|^p \right)^{1/p} $$
  
  *   **Euclidean Distance ($p=2$, $L_2$ Norm):**
    *   Standard straight-line distance.
    *   *Assumptions:* Implies the data space is isotropic (all directions are equal).
    *   *Weakness:* Sensitive to scale (if one feature is "Salary" and another is "Age," salary dominates). Sensitive to high dimensions (Curse of Dimensionality).
  *   **Manhattan Distance ($p=1$, $L_1$ Norm):**
    *   Sum of absolute differences.
    *   *Usage:* Preferred for sparse data or high-dimensional grids (like city blocks). Robust to outliers compared to $L_2$.
  *   **Chebyshev Distance ($p=\infty$, $L_\infty$ Norm):**
    *   The greatest difference along any single coordinate dimension.
    *   $$ d(x, y) = \max_k |x_{i,k} - x_{j,k}| $$
- ### B. Correlation Coefficient (Slide 17)
  Measures the **linear relationship** between two vectors, rather than geometric distance.
  $$ d(x_i, x_j) = \frac{\text{Covariance}(x_i, x_j)}{\sigma_{x_i} \sigma_{x_j}} $$
  *   *Usage:* Gene expression data or stock market trends. We care if two stocks go up/down together, even if their absolute prices are very different.
- ### C. Cosine Similarity (Slide 18)
  Measures the **angle** between two vectors, ignoring their magnitude.
  $$ \text{Cosine}(x, y) = \frac{x \cdot y}{||x|| ||y||} $$
  $$ \text{Cosine Distance} = 1 - \text{Cosine Similarity} $$
  *   *Usage:* Text Mining (Document Clustering).
  *   *Why:* A long document and a short document about "Sports" will point in the same direction in vector space, even if their Euclidean distance is huge due to word count differences.
  
  ---
- ## 4. The Geometry Trap (Slide 19)
  **Scenario:**
  *   Point B is $(1, 1)$.
  *   Point A is $(2, 2)$.
  *   Point C is $(5, 8)$.
  
  **Visual Analysis:**
  *   **Euclidean:** A is clearly closer to B (distance is small) than to C.
  *   **Cosine:** A and B are on the exact same line from the origin (Angle = 0). A and C have an angle between them.
  *   *However*, if C were at $(10, 10)$, it would be far away in Euclidean terms, but **identical** to A and B in Cosine terms.
  
  **Key Takeaway:**
  *   Use **Euclidean** when magnitude matters (e.g., GPS coordinates, physical measurements).
  *   Use **Cosine** when magnitude doesn't matter, only the profile/distribution of features (e.g., text topics, user preferences).
  
  ---
- ## 5. Linkage Methods (Slide 23)
  When defining the distance between **two clusters** (sets of points), we have options:
  1.  **Single Linkage (Min):** Distance between the two *closest* points. (Result: Long, chain-like clusters).
  2.  **Complete Linkage (Max):** Distance between the two *farthest* points. (Result: Tight, spherical clusters).
  3.  **Average Linkage:** Average distance between all pairs of points.
  4.  **Centroid Linkage:** Distance between the two centroids. (Used in K-Means).
  
  ***
- # Self-Check Questions
  
  1.  **Concept Check:** If you are clustering data regarding "User Spending," and you have features "Total Amount Spent" (Range: \$0-\$1M) and "Number of Transactions" (Range: 1-100), which distance metric is dangerous to use without preprocessing?
    *   *Answer:* **Euclidean.** The "Amount Spent" feature will completely dominate the distance calculation due to its scale. You must Normalize/Standardize the data first.
  2.  **Metric Selection:** You want to cluster text documents based on their topic (e.g., "Sports" vs "Politics"). Should you use Manhattan or Cosine distance?
    *   *Answer:* **Cosine.** Document length varies wildy; we care about the word *distribution* (angle), not the total word count (magnitude).
  3.  **Math Check:** What is the Chebyshev distance between point $(1, 5)$ and $(4, 2)$?
    *   $|1-4| = 3$. $|5-2| = 3$. The max is 3.
  
  ***