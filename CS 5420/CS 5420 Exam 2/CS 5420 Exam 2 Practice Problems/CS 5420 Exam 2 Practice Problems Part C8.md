#### Problem 1 (Direct Review Mirror: 4 Points with Divergent Cuts)
  Four 1-D points are situated on a line:
  $$A = 0.0, \quad B = 2.5, \quad C = 5.5, \quad D = 10.5$$
  * **(a)** Run agglomerative clustering using **Single Linkage**. List the merge order and the exact merge heights.
  * **(b)** Run agglomerative clustering using **Complete Linkage**. List the merge order and the exact merge heights.
  * **(c)** Cut each tree into **2 clusters**. State the resulting cluster partitions for both linkages.
  
  ---
#### Problem 2 (Five Points with an Outlier)
  Five 1-D observations:
  $$A = 1.0, \quad B = 2.0, \quad C = 4.0, \quad D = 7.0, \quad E = 15.0$$
  * **(a)** Construct the initial $5 \times 5$ pairwise distance matrix.
  * **(b)** Trace the complete merge order and merge heights for **Single Linkage**.
  * **(c)** Trace the complete merge order and merge heights for **Complete Linkage**.
  * **(d)** Cut both dendrograms into **2 clusters**. Do the two linkage methods produce the same 2-cluster partition?
  
  ---
#### Problem 3 (2-D Coordinate Clustering with Euclidean Distances)
  Four observations lie in $\mathbb{R}^2$:
  $$A = (0, 0), \quad B = (0, 2), \quad C = (3, 2), \quad D = (3, 7)$$
  * **(a)** Compute the exact Euclidean distance between all 6 unique pairs.
  * **(b)** Run **Single Linkage**: give the merge order and merge heights.
  * **(c)** Run **Complete Linkage**: give the merge order and merge heights.
  * **(d)** Cut each hierarchy into **2 clusters**.
  
  ---
#### Problem 4 (Identical Topology, Different Merge Heights)
  Four 1-D points form two isolated, compact pairs:
  $$A = 0.0, \quad B = 1.0, \quad C = 10.0, \quad D = 11.0$$
  * **(a)** Execute the merges under **Single Linkage**.
  * **(b)** Execute the merges under **Complete Linkage**.
  * **(c)** What are the 2-cluster cuts for both linkages?
  * **(d)** Explain why the two methods produce identical cluster memberships, but differ in the height of the final merge.
  
  ---
#### Problem 5 (The Chaining Pathology in Action)
  Five 1-D points are spaced as follows:
  $$A = 0.0, \quad B = 1.8, \quad C = 3.6, \quad D = 5.4, \quad E = 12.0$$
  * **(a)** Using **Single Linkage**, show step-by-step how the algorithm sequentially chains $A \to B \to C \to D$. State the merge heights.
  * **(b)** Cut the single linkage tree into **2 clusters**.
  * **(c)** Using **Complete Linkage**, determine whether points merge as a chain or form balanced subsets. State the 2-cluster partition.
  
  ---
#### Problem 6 (Determining the Valid Cut Height Range)
  Using the dataset from Problem 1 ($A = 0.0, B = 2.5, C = 5.5, D = 10.5$):
  * **(a)** For **Single Linkage**, what is the valid numerical range of horizontal cut heights $h_{\text{cut}} \in [h_{\min}, h_{\max})$ that yields exactly **$k = 2$ clusters**?
  * **(b)** For **Complete Linkage**, what is the valid numerical range of horizontal cut heights $h_{\text{cut}} \in [h_{\min}, h_{\max})$ that yields exactly **$k = 2$ clusters**?
  * **(c)** Which linkage provides a wider vertical gap for separating the data into 2 clusters?
  
  ---
#### Problem 7 (Average Linkage Hand Solve)
  Three clusters have been formed:
  $$C_1 = \{0.0, \; 2.0\}, \quad C_2 = \{6.0\}, \quad C_3 = \{10.0\}$$
  * **(a)** Calculate the **Average Linkage (UPGMA)** distance between $C_1$ and $C_2$:
  $$d_{\text{avg}}(C_1, C_2) = \frac{1}{|C_1||C_2|} \sum_{x \in C_1}\sum_{y \in C_2} |x - y|$$
  * **(b)** Calculate the Average Linkage distance between $C_1$ and $C_3$.
  * **(c)** Which pair will merge next under Average Linkage: $(C_1, C_2)$, $(C_1, C_3)$, or $(C_2, C_3)$?
  
  ---
#### Problem 8 (Ward’s Minimum Variance Linkage Hand Solve)
  Two 1-D clusters are given by:
  $$A = \{1.0, \; 3.0\} \quad \text{and} \quad B = \{7.0, \; 9.0\}$$
  * **(a)** Calculate the cluster sizes $|A|$ and $|B|$, and their centroids $\mu_A$ and $\mu_B$.
  * **(b)** Calculate the Ward’s linkage distance between $A$ and $B$:
  $$\Delta J(A, B) = \frac{|A||B|}{|A| + |B|} \|\mu_A - \mu_B\|_2^2$$
  * **(c)** What does $\Delta J(A, B)$ physically represent with respect to the $k$-means inertia objective?
  
  ---
#### Problem 9 (2-D Rectangle Corners)
  Four points sit at the vertices of a 2D rectangle:
  $$A = (0, 0), \quad B = (4, 0), \quad C = (0, 2), \quad D = (4, 2)$$
  * **(a)** Calculate all pairwise Euclidean distances.
  * **(b)** Determine the merge order and merge heights for **Single Linkage**.
  * **(c)** Determine the merge order and merge heights for **Complete Linkage**.
  * **(d)** State the 2-cluster cut for both linkages.
  
  ---
#### Problem 10 (Reconstructing Partitions from a Linkage Table)
  An automated clustering algorithm outputs the following 3-step merge history for points $\{P_1, P_2, P_3, P_4\}$:
  * Merge 1: $\{P_1, P_2\}$ at height $h = 1.20$
  * Merge 2: $\{P_3, P_4\}$ at height $h = 1.50$
  * Merge 3: $\{P_1, P_2, P_3, P_4\}$ at height $h = 6.80$
  * **(a)** What cluster partition is obtained by slicing the dendrogram at height $h_{\text{cut}} = 4.00$?
  * **(b)** What cluster partition is obtained by slicing the dendrogram at height $h_{\text{cut}} = 1.35$?
  * **(c)** How many clusters are produced if the cut is placed at $h_{\text{cut}} = 0.50$?
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C8
### Governing Rules:
  * **Number of Merges:** A dataset of $n$ instances always requires exactly **$n - 1$ merges** to reach 1 root cluster.
  * **Single Linkage:** $d_{\text{single}}(A, B) = \min_{x \in A, y \in B} d(x, y)$
  * **Complete Linkage:** $d_{\text{complete}}(A, B) = \max_{x \in A, y \in B} d(x, y)$
  * **The $k$-Cluster Cut Rule:** Cutting a dendrogram into $k$ flat clusters is mathematically equivalent to **undoing the final $k - 1$ merges**. To get 2 clusters, undo the last merge ($h_{\max}$).
  
  ---
### Problem 1 Solution
  Points: $A = 0.0, \; B = 2.5, \; C = 5.5, \; D = 10.5$.
#### Initial Distance Matrix:
  * $d(A, B) = 2.5$
  * $d(B, C) = 3.0$
  * $d(C, D) = 5.0$
  * $d(A, C) = 5.5$
  * $d(B, D) = 8.0$
  * $d(A, D) = 10.5$
  
  *Smallest pairwise distance:* $d(A, B) = 2.5 \implies$ **Merge 1: $\{A, B\}$ at height $h = 2.50$** for both linkages.
  
  ---
#### (a) Single Linkage ($\min$):
  * Distances from $\{A, B\}$:
  * to $C$: $\min(d(A, C), d(B, C)) = \min(5.5, 3.0) = \mathbf{3.0}$
  * to $D$: $\min(d(A, D), d(B, D)) = \min(10.5, 8.0) = \mathbf{8.0}$
  * Between unmerged singletons: $d(C, D) = \mathbf{5.0}$
  * *Smallest distance:* $d(\{A, B\}, C) = 3.0 \implies$ **Merge 2: $\{A, B, C\}$ at height $h = 3.00$**.
  * Distance from $\{A, B, C\}$ to $D$:
  $$\min(d(A, D), d(B, D), d(C, D)) = \min(10.5, 8.0, 5.0) = \mathbf{5.0}$$
  * **Merge 3: $\{A, B, C, D\}$ at height $h = 5.00$**.
  * **Single Linkage Merge Summary:**
  1. $\{A, B\}$ at $h = 2.50$
  2. $\{A, B, C\}$ at $h = 3.00$
  3. $\{A, B, C, D\}$ at $h = 5.00$
  
  ---
#### (b) Complete Linkage ($\max$):
  * After Merge 1 ($\{A, B\}$ at $h = 2.50$), recompute distances using $\max$:
  * $d_{\text{complete}}(\{A, B\}, C) = \max(d(A, C), d(B, C)) = \max(5.5, 3.0) = \mathbf{5.5}$
  * $d_{\text{complete}}(\{A, B\}, D) = \max(d(A, D), d(B, D)) = \max(10.5, 8.0) = \mathbf{10.5}$
  * Between unmerged singletons: $d(C, D) = |10.5 - 5.5| = \mathbf{5.0}$
  * Compare all available pairs:
  $$\{ (\{A, B\}, C): 5.5, \quad (\{A, B\}, D): 10.5, \quad \mathbf{(C, D): 5.0} \}$$
  * *Smallest distance:* $d(C, D) = 5.0 < 5.5 \implies$ **Merge 2: $\{C, D\}$ at height $h = 5.00$**.
  * Distance between $\{A, B\}$ and $\{C, D\}$:
  $$d_{\text{complete}}(\{A, B\}, \{C, D\}) = \max(d(A, C), d(A, D), d(B, C), d(B, D)) = d(A, D) = |0 - 10.5| = \mathbf{10.5}$$
  * **Merge 3: $\{A, B\} + \{C, D\} \implies \{A, B, C, D\}$ at height $h = 10.50$**.
  * **Complete Linkage Merge Summary:**
  1. $\{A, B\}$ at $h = 2.50$
  2. $\{C, D\}$ at $h = 5.00$
  3. $\{A, B, C, D\}$ at $h = 10.50$
  
  ---
#### (c) 2-Cluster Cuts (Undo Final Merge):
  * **Single Linkage 2-Cluster Cut:** Undo Merge 3 ($h = 5.00$):
  $$\mathbf{\{A, B, C\} \quad \mid \quad \{D\}}$$
  * **Complete Linkage 2-Cluster Cut:** Undo Merge 3 ($h = 10.50$):
  $$\mathbf{\{A, B\} \quad \mid \quad \{C, D\}}$$
  
  ---
### Problem 2 Solution
  Points: $A = 1, B = 2, C = 4, D = 7, E = 15$.
  
  * **(a) Pairwise Distance Matrix:**
  * $d(A, B) = 1.0$ (Smallest overall)
  * $d(B, C) = 2.0$
  * $d(C, D) = 3.0$
  * $d(D, E) = 8.0$
  * $d(A, C) = 3.0, \; d(B, D) = 5.0, \; d(A, D) = 6.0$
  * $d(C, E) = 11.0, \; d(B, E) = 13.0, \; d(A, E) = 14.0$
  
  * **(b) Single Linkage Trace ($\min$):**
  * **Merge 1:** $\{A, B\}$ at $h = \mathbf{1.00}$.
  * Distances from $\{A, B\}$: to $C = \min(3, 2) = 2.0$; to $D = \min(6, 5) = 5.0$; to $E = 13.0$.
  * **Merge 2:** $\{A, B\}$ with $C$ at $h = \mathbf{2.00} \implies \{A, B, C\}$.
  * Distances from $\{A, B, C\}$: to $D = \min(5, 3) = 3.0$; to $E = 11.0$.
  * **Merge 3:** $\{A, B, C\}$ with $D$ at $h = \mathbf{3.00} \implies \{A, B, C, D\}$.
  * Distance to $E$: $\min(14, 13, 11, 8) = 8.0$.
  * **Merge 4:** $\{A, B, C, D\}$ with $E$ at $h = \mathbf{8.00}$.
  
  * **(c) Complete Linkage Trace ($\max$):**
  * **Merge 1:** $\{A, B\}$ at $h = \mathbf{1.00}$.
  * Distances from $\{A, B\}$: to $C = \max(3, 2) = 3.0$; to $D = \max(6, 5) = 6.0$; to $E = 14.0$.
  * Available distances: $(\{A, B\}, C) = 3.0, \; (C, D) = 3.0, \; (D, E) = 8.0$.
  * Tie between $(\{A, B\}, C)$ and $(C, D)$ at 3.0. Breaking tie to $\{A, B, C\}$:
  * **Merge 2:** $\{A, B\}$ with $C$ at $h = \mathbf{3.00} \implies \{A, B, C\}$.
  * Distances from $\{A, B, C\}$: to $D = \max(6, 5, 3) = 6.0$; to $E = \max(14, 13, 11) = 14.0$.
  * Remaining singleton distance: $d(D, E) = 8.0$.
  * **Merge 3:** $\{A, B, C\}$ with $D$ at $h = \mathbf{6.00} \implies \{A, B, C, D\}$.
  * **Merge 4:** $\{A, B, C, D\}$ with $E$ at $h = \max(d) = d(A, E) = \mathbf{14.00}$.
  
  * **(d) 2-Cluster Cut:**
  * For both linkages, undoing Merge 4 isolates $E$:
    $$\mathbf{\{A, B, C, D\} \quad \mid \quad \{E\}}$$
  * **Yes, both produce the identical 2-cluster partition** because $E = 15$ is a distant isolated outlier.
  
  ---
### Problem 3 Solution
  Points: $A(0, 0), B(0, 2), C(3, 2), D(3, 7)$.
  
  * **(a) Pairwise Euclidean Distances:**
  * $d(A, B) = \sqrt{0^2 + 2^2} = \mathbf{2.000}$ (Smallest)
  * $d(B, C) = \sqrt{3^2 + 0^2} = \mathbf{3.000}$
  * $d(A, C) = \sqrt{3^2 + 2^2} = \sqrt{13} \approx \mathbf{3.606}$
  * $d(C, D) = \sqrt{0^2 + 5^2} = \mathbf{5.000}$
  * $d(B, D) = \sqrt{3^2 + 5^2} = \sqrt{34} \approx \mathbf{5.831}$
  * $d(A, D) = \sqrt{3^2 + 7^2} = \sqrt{58} \approx \mathbf{7.616}$
  
  * **(b) Single Linkage:**
  * **Merge 1:** $\{A, B\}$ at $h = \mathbf{2.000}$.
  * Distances from $\{A, B\}$: to $C = \min(3.606, 3.000) = \mathbf{3.000}$; to $D = \min(7.616, 5.831) = 5.831$.
  * Distance between $C$ and $D$: $d(C, D) = 5.000$.
  * **Merge 2:** $\{A, B\}$ with $C$ at $h = \mathbf{3.000} \implies \{A, B, C\}$.
  * Distance from $\{A, B, C\}$ to $D$: $\min(7.616, 5.831, 5.000) = \mathbf{5.000}$.
  * **Merge 3:** $\{A, B, C\}$ with $D$ at $h = \mathbf{5.000}$.
  
  * **(c) Complete Linkage:**
  * **Merge 1:** $\{A, B\}$ at $h = \mathbf{2.000}$.
  * Distances from $\{A, B\}$: to $C = \max(3.606, 3.000) = \mathbf{3.606}$; to $D = \max(7.616, 5.831) = 7.616$.
  * Distance between $C$ and $D$: $d(C, D) = \mathbf{5.000}$.
  * Smallest available: $d(\{A, B\}, C) = 3.606 < 5.000$.
  * **Merge 2:** $\{A, B\}$ with $C$ at $h = \sqrt{13} \approx \mathbf{3.606} \implies \{A, B, C\}$.
  * Distance from $\{A, B, C\}$ to $D$: $\max(7.616, 5.831, 5.000) = d(A, D) = \sqrt{58} \approx \mathbf{7.616}$.
  * **Merge 3:** $\{A, B, C\}$ with $D$ at $h = \sqrt{58} \approx \mathbf{7.616}$.
  
  * **(d) 2-Cluster Cut:**
  * For both linkages, undoing Merge 3 leaves $D$ isolated:
    $$\mathbf{\{A, B, C\} \quad \mid \quad \{D\}}$$
  
  ---
### Problem 4 Solution
  Points: $A = 0, B = 1, C = 10, D = 11$.
  
  * **(a) Single Linkage:**
  * Merge 1: $\{A, B\}$ at $h = 1.0$.
  * Merge 2: $\{C, D\}$ at $h = 1.0$.
  * Merge 3: $\{A, B\}$ with $\{C, D\}$ at $\min = d(B, C) = |10 - 1| = \mathbf{9.00}$.
  * **(b) Complete Linkage:**
  * Merge 1: $\{A, B\}$ at $h = 1.0$.
  * Merge 2: $\{C, D\}$ at $h = 1.0$.
  * Merge 3: $\{A, B\}$ with $\{C, D\}$ at $\max = d(A, D) = |11 - 0| = \mathbf{11.00}$.
  * **(c) 2-Cluster Cut:**
  * For both linkages, undoing Merge 3 yields the exact same clean clusters:
    $$\mathbf{\{A, B\} \quad \mid \quad \{C, D\}}$$
  * **(d) Why Heights Differ:**
  * The merge sequence is identical because within-cluster gaps ($1.0$) are much smaller than between-cluster gaps ($9.0$).
  * The final merge height differs because Single Linkage records the **closest bridge gap** ($d(B, C) = 9.0$), whereas Complete Linkage records the **total diameter of the union** ($d(A, D) = 11.0$).
  
  ---
### Problem 5 Solution
  Points: $A = 0.0, B = 1.8, C = 3.6, D = 5.4, E = 12.0$.
  
  * **(a) Single Linkage Chaining Trace:**
  * $d(A, B) = 1.8, \; d(B, C) = 1.8, \; d(C, D) = 1.8, \; d(D, E) = 6.6$.
  * Merge 1: $\{A, B\}$ at $h = \mathbf{1.80}$.
  * Distance to $C$: $\min(d(A, C), d(B, C)) = \min(3.6, 1.8) = 1.80$.
  * Merge 2: $\{A, B, C\}$ at $h = \mathbf{1.80}$.
  * Distance to $D$: $\min(5.4, 3.6, 1.8) = 1.80$.
  * Merge 3: $\{A, B, C, D\}$ at $h = \mathbf{1.80}$.
  * Merge 4: $\{A, B, C, D\}$ with $E$ at $\min = d(D, E) = \mathbf{6.60}$.
  * **(b) Single Linkage 2-Cluster Cut:**
  $$\mathbf{\{A, B, C, D\} \quad \mid \quad \{E\}}$$
  * **(c) Complete Linkage Trace & Cut:**
  * Merge 1: $\{A, B\}$ at $h = 1.80$.
  * $d_{\text{complete}}(\{A, B\}, C) = \max(3.6, 1.8) = 3.60$.
  * But unmerged pair $d(C, D) = \mathbf{1.80} < 3.60$!
  * Merge 2: $\{C, D\}$ at $h = \mathbf{1.80}$.
  * Now clusters are: $\{A, B\}$, $\{C, D\}$, and $\{E\}$.
  * $d_{\text{complete}}(\{A, B\}, \{C, D\}) = d(A, D) = |5.4 - 0| = \mathbf{5.40}$.
  * $d_{\text{complete}}(\{C, D\}, E) = d(C, E) = |12.0 - 3.6| = \mathbf{8.40}$.
  * Merge 3: $\{A, B\}$ with $\{C, D\}$ at $h = \mathbf{5.40} \implies \{A, B, C, D\}$.
  * Merge 4: with $E$ at $h = d(A, E) = \mathbf{12.00}$.
  * **2-Cluster Cut for Complete:**
    $$\mathbf{\{A, B, C, D\} \quad \mid \quad \{E\}}$$
    *(Complete linkage avoids chaining early by pairing $\{C, D\}$ before merging with $\{A, B\}$, but the extreme distance of $E$ makes the final cut identical).*
  
  ---
### Problem 6 Solution
  From Problem 1:
  * Single Linkage heights: $h_1 = 2.50, \; h_2 = 3.00, \; h_3 = 5.00$.
  * Complete Linkage heights: $h_1 = 2.50, \; h_2 = 5.00, \; h_3 = 10.50$.
  
  * **(a) Single Linkage Cut Range for $k = 2$:**
  To obtain 2 clusters, the cut must slice above Merge 2 ($h_2 = 3.00$) and below Merge 3 ($h_3 = 5.00$):
  $$\mathbf{h_{\text{cut}} \in [3.00, \; 5.00)}$$
  * **(b) Complete Linkage Cut Range for $k = 2$:**
  Must slice above Merge 2 ($h_2 = 5.00$) and below Merge 3 ($h_3 = 10.50$):
  $$\mathbf{h_{\text{cut}} \in [5.00, \; 10.50)}$$
  * **(c) Wider Vertical Gap:**
  * Single Linkage Gap: $5.00 - 3.00 = 2.00$ units.
  * Complete Linkage Gap: $10.50 - 5.00 = \mathbf{5.50 \text{ units}}$.
  * **Complete Linkage provides a much wider vertical gap ($5.50$ vs. $2.00$)**, indicating more distinct separation between the two macro-groups.
  
  ---
### Problem 7 Solution
  Clusters: $C_1 = \{0, 2\}, \; C_2 = \{6\}, \; C_3 = \{10\}$.
  
  * **(a) Average Distance $d_{\text{avg}}(C_1, C_2)$:**
  $$|C_1| = 2, \quad |C_2| = 1 \implies |C_1||C_2| = 2 \text{ pairs}$$
  $$\text{Pairs: } |0 - 6| = 6.0, \quad |2 - 6| = 4.0$$
  $$d_{\text{avg}}(C_1, C_2) = \frac{6.0 + 4.0}{2} = \frac{10.0}{2} = \mathbf{5.000}$$
  
  * **(b) Average Distance $d_{\text{avg}}(C_1, C_3)$:**
  $$\text{Pairs: } |0 - 10| = 10.0, \quad |2 - 10| = 8.0$$
  $$d_{\text{avg}}(C_1, C_3) = \frac{10.0 + 8.0}{2} = \frac{18.0}{2} = \mathbf{9.000}$$
  
  * **(c) Next Merge Decision:**
  * $d(C_2, C_3) = |6 - 10| = \mathbf{4.000}$.
  * Compare all pairs:
    $$d(C_2, C_3) = 4.000 < d(C_1, C_2) = 5.000 < d(C_1, C_3) = 9.000$$
  * **Winner:** Clusters **$(C_2, C_3)$** merge next at height $h = 4.000$.
  
  ---
### Problem 8 Solution
  Clusters: $A = \{1.0, 3.0\}, \; B = \{7.0, 9.0\}$.
  
  * **(a) Sizes and Centroids:**
  $$|A| = 2, \quad \mu_A = \frac{1.0 + 3.0}{2} = \mathbf{2.000}$$
  $$|B| = 2, \quad \mu_B = \frac{7.0 + 9.0}{2} = \mathbf{8.000}$$
  
  * **(b) Ward's Linkage Distance:**
  $$\Delta J(A, B) = \frac{|A||B|}{|A| + |B|} \|\mu_A - \mu_B\|_2^2 = \frac{2 \times 2}{2 + 2} (2.000 - 8.000)^2$$
  $$\Delta J(A, B) = \frac{4}{4} (-6.000)^2 = 1 \times 36.000 = \mathbf{36.000}$$
  
  * **(c) Physical Interpretation:**
  $\Delta J(A, B)$ represents the **exact increase in total within-cluster sum of squares (Inertia)** that would occur if clusters $A$ and $B$ were merged into a single cluster.
  
  ---
### Problem 9 Solution
  Points: $A(0, 0), B(4, 0), C(0, 2), D(4, 2)$.
  
  * **(a) Pairwise Distances:**
  * $d(A, C) = 2.000, \quad d(B, D) = 2.000$ (Vertical edges — smallest)
  * $d(A, B) = 4.000, \quad d(C, D) = 4.000$ (Horizontal edges)
  * $d(A, D) = \sqrt{4^2 + 2^2} = \sqrt{20} \approx 4.472$ (Diagonal)
  * $d(B, C) = \sqrt{4^2 + 2^2} = \sqrt{20} \approx 4.472$ (Diagonal)
  
  * **(b) Single Linkage:**
  * Merge 1: $\{A, C\}$ at $h = \mathbf{2.000}$.
  * Merge 2: $\{B, D\}$ at $h = \mathbf{2.000}$.
  * Distance between $\{A, C\}$ and $\{B, D\}$:
    $$\min(d(A, B), d(C, D), d(A, D), d(B, C)) = \min(4.0, 4.0, 4.472, 4.472) = \mathbf{4.000}$$
  * Merge 3: $\{A, C\}$ with $\{B, D\}$ at $h = \mathbf{4.000}$.
  
  * **(c) Complete Linkage:**
  * Merge 1: $\{A, C\}$ at $h = \mathbf{2.000}$.
  * Merge 2: $\{B, D\}$ at $h = \mathbf{2.000}$.
  * Distance between $\{A, C\}$ and $\{B, D\}$:
    $$\max(d(A, B), d(C, D), d(A, D), d(B, C)) = \max(4.0, 4.0, 4.472, 4.472) = \sqrt{20} \approx \mathbf{4.472}$$
  * Merge 3: $\{A, C\}$ with $\{B, D\}$ at $h = \mathbf{4.472}$.
  
  * **(d) 2-Cluster Cut:**
  * For both linkages, undoing the final merge yields:
    $$\mathbf{\{A, C\} \quad \mid \quad \{B, D\}}$$
    *(Partitions the rectangle into its left vertical edge vs. right vertical edge).*
  
  ---
### Problem 10 Solution
  Merge history:
  * Merge 1: $\{P_1, P_2\}$ at $h = 1.20$
  * Merge 2: $\{P_3, P_4\}$ at $h = 1.50$
  * Merge 3: $\{P_1, P_2, P_3, P_4\}$ at $h = 6.80$
  
  * **(a) Cut at $h_{\text{cut}} = 4.00$:**
  * $4.00$ sits between Merge 2 ($1.50$) and Merge 3 ($6.80$).
  * Merge 3 has not occurred yet.
  * **Result:** **2 Clusters:** $\mathbf{\{P_1, P_2\} \quad \mid \quad \{P_3, P_4\}}$
  * **(b) Cut at $h_{\text{cut}} = 1.35$:**
  * $1.35$ sits between Merge 1 ($1.20$) and Merge 2 ($1.50$).
  * Merge 1 has occurred, but Merge 2 has not.
  * **Result:** **3 Clusters:** $\mathbf{\{P_1, P_2\} \quad \mid \quad \{P_3\} \quad \mid \quad \{P_4\}}$
  * **(c) Cut at $h_{\text{cut}} = 0.50$:**
  * $0.50$ sits below the first merge ($1.20$).
  * Zero merges have occurred.
  * **Result:** **4 Clusters (All Singletons):** $\mathbf{\{P_1\} \quad \mid \quad \{P_2\} \quad \mid \quad \{P_3\} \quad \mid \quad \{P_4\}}$