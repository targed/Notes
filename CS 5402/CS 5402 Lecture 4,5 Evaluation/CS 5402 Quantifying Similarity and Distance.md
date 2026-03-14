## 1. What makes a "Metric"? (The Formal Axioms)
**Slide Context:** Slide 4 lists three properties: non-negative, zero if identical, and larger value = more different.
**Comprehensive Notes:**
In advanced algorithms, a distance function $d(x, y)$ is only considered a **Metric** if it satisfies four specific mathematical axioms:
1.  **Non-negativity:** $d(x, y) \ge 0$. (Distance cannot be negative).
2.  **Identity of Indiscernibles:** $d(x, y) = 0$ if and only if $x = y$.
3.  **Symmetry:** $d(x, y) = d(y, x)$. (The distance from A to B is the same as B to A).
4.  **Triangle Inequality:** $d(x, z) \le d(x, y) + d(y, z)$. (The direct path is never longer than a path through an intermediate point).

**Similarity vs. Distance:**
*   **Distance ($d$):** Smaller is better (0 = identical).
*   **Similarity ($s$):** Larger is better (1 = identical).
*   *Conversion:* For normalized data, you can often convert them using $s = 1 - d$ or $s = \frac{1}{1+d}$.
- ## 2. Geometric Distances (L1 vs. L2)
  These are both specific cases of the **Minkowski Distance** ($L_p$ norm).
- ### A. L2 Distance (Euclidean)
  *   **Formula:** $d(x, y) = \sqrt{\sum (x_i - y_i)^2}$
  *   **Intuition:** "As the crow flies." The straight-line distance in physical space.
  *   **Weakness:** **Extremely sensitive to outliers** because it squares the differences. A single massive difference in one dimension will dominate the total distance.
- ### B. L1 Distance (Manhattan/Taxicab)
  *   **Formula:** $d(x, y) = \sum |x_i - y_i|$
  *   **Intuition:** Walking blocks in a city grid.
  *   **Strength:** Much more **robust to outliers** than L2 because it doesn't square the differences. It is preferred in high-dimensional spaces where "distances" start to lose meaning (The Curse of Dimensionality).
- ## 3. Directional Similarity (Cosine)
  **Slide Context:** Slide 5 shows an angle $\theta$ between vectors.
  *   **Formula:** $\cos(x, y) = \frac{x \cdot y}{\|x\| \|y\|}$
  *   **Key Concept:** It measures the **angle**, not the magnitude. 
  *   **Use Case:** Essential for **Text Mining**. 
    *   *Example:* Document A mentions "Data" 10 times. Document B mentions "Data" 1,000 times. In Euclidean space, they are very "far apart." In Cosine space, the angle is 0° because they are pointing in the same "direction" (topic).
- ## 4. Set-Based Similarity (Jaccard)
  *   **Formula:** $J(A, B) = \frac{|A \cap B|}{|A \cup B|}$ (Intersection over Union).
  *   **Use Case:** **Binary Asymmetric Data**.
    *   *Example (Shopping Baskets):* A store has 10,000 items. Alice buys Milk and Bread. Bob buys Milk and Eggs. 
    *   We don't care about the 9,997 items they *didn't* buy (the zeros). Jaccard ignores "mutual absences" and only looks at what they actually did (the ones).
- ## 5. Mixed Data (Gower’s Distance)
  **Slide Context:** Slide 6 mentions "Gower" for mixed data.
  **Fill-in Knowledge:**
  Most real datasets have a mix of Numeric, Nominal, and Ordinal data. You cannot calculate Euclidean distance on a dataset that contains both "Salary" and "Eye Color."
  *   **Gower’s Distance** calculates a similarity score for each attribute separately based on its type and then takes a weighted average. It is the "Swiss Army Knife" of distance metrics.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Euclidean vs. Manhattan:**
  Point A = (0, 0), Point B = (3, 4).
  *   Calculate **L2 (Euclidean)**: $\sqrt{(3-0)^2 + (4-0)^2} = \sqrt{9+16} = \mathbf{5}$.
  *   Calculate **L1 (Manhattan)**: $|3-0| + |4-0| = 3 + 4 = \mathbf{7}$.
  
  **2. Jaccard Logic:**
  Transaction 1: {Bread, Milk, Diapers}
  Transaction 2: {Bread, Milk, Beer}
  *   What is the Intersection ($A \cap B$)? (Answer: {Bread, Milk} -> Size 2).
  *   What is the Union ($A \cup B$)? (Answer: {Bread, Milk, Diapers, Beer} -> Size 4).
  *   **Jaccard Similarity:** $2 / 4 = \mathbf{0.5}$.
  
  **3. Metric Selection:**
  You are building a recommendation engine for a library. You want to find books that have a similar vocabulary, regardless of whether the book is a short story or a long novel.
  *   Which metric should you use? (Answer: **Cosine Similarity**, because it ignores the "length" or magnitude of the word counts and only looks at the word distribution).