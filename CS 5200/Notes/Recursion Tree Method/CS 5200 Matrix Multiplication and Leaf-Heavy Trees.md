### 1. The Standard Iterative Approach (Slides 24–25)
Before we divide and conquer, we must understand the baseline.
To multiply two $n \times n$ matrices $A$ and $B$ to get $C$:
*   Every element $C[i][j]$ is a **dot product** of a row from $A$ and a column from $B$.
*   There are $n^2$ elements in the output matrix.
*   Calculating **each** element requires $n$ multiplications.
*   **Total Work:** $n^2 \times n = \mathbf{O(n^3)}$.

---
- ### 2. The Divide and Conquer Strategy (Slides 26–29)
  To turn this into a recursive algorithm, we partition each $n \times n$ matrix into four smaller quadrants of size $(n/2) \times (n/2)$.
  
  If $C = A \cdot B$, the quadrants of $C$ are calculated as:
  1.  $C_{11} = A_{11}B_{11} + A_{12}B_{21}$
  2.  $C_{12} = A_{11}B_{12} + A_{12}B_{22}$
  3.  $C_{21} = A_{21}B_{11} + A_{22}B_{21}$
  4.  $C_{22} = A_{21}B_{12} + A_{22}B_{22}$
  
  **The Cost Breakdown:**
  *   To solve the problem, we must perform **8 matrix multiplications** of size $n/2$ (look at the terms: $A_{11}B_{11}, A_{12}B_{21}$, etc.).
  *   We must perform **4 matrix additions** of size $n/2$. Matrix addition takes $O(n^2)$ time.
  
  **The Recurrence Relation (Slide 30):**
  $$T(n) = 8T(n/2) + \Theta(n^2)$$
  
  ---
- ### 3. The Recursion Tree Analysis (Slides 31–33)
  Let's see why this algorithm is "Leaf-Heavy" and how the tree behaves.
  
  *   **Level 0 (Root):** One problem of size $n$. Work to add results = $n^2$.
  *   **Level 1:** **8** subproblems of size $n/2$.
    *   Work per node: $(n/2)^2 = n^2/4$.
    *   Total level work: $8 \times (n^2/4) = \mathbf{2n^2}$.
  *   **Level 2:** Each of the 8 nodes splits into 8 more. Total **64** subproblems of size $n/4$.
    *   Work per node: $(n/4)^2 = n^2/16$.
    *   Total level work: $64 \times (n^2/16) = \mathbf{4n^2}$.
  
  **The Pattern:**
  The work is **doubling** at every level ($n^2, 2n^2, 4n^2, 8n^2 \dots$).
  *   This is an **Increasing Geometric Series**. 
  *   In such cases, the work is dominated by the **Leaf Level** (the bottom of the tree).
  
  ---
- ### 4. Solving for the Leaves (Slide 33)
  To find the total work, we just need to count the number of leaves at the bottom.
  
  *   **Tree Height:** $\log_2 n$.
  *   **Number of Leaves:** $(\text{Branching Factor})^{\text{Height}} = 8^{\log_2 n}$.
  *   **The Math Rule:** $a^{\log_b n} = n^{\log_b a}$.
  *   Therefore: $8^{\log_2 n} = n^{\log_2 8} = \mathbf{n^3}$.
  
  **Final Complexity:**
  Since the leaf level has $n^3$ nodes, and each does $O(1)$ work, the total time is:
  $$T(n) = \mathbf{O(n^3)}$$
  
  ---
- ### 5. The "Deep Dive" Realization
  Wait—the iterative loop was $O(n^3)$ and this complex recursive version is *also* $O(n^3)$. **Did we fail?**
  
  **The Answer:** Yes. The "Simple" Divide and Conquer doesn't actually speed up matrix multiplication. It just organizes the work differently.
  
  **The Teaser (Strassen’s Algorithm):**
  Your professor mentions "Strassen’s Algorithm" in the Master Theorem slides. Strassen found a way to calculate those 4 quadrants using only **7 multiplications** instead of 8.
  *   *New Recurrence:* $T(n) = \mathbf{7}T(n/2) + n^2$.
  *   *New Leaves:* $7^{\log_2 n} = n^{\log_2 7} \approx \mathbf{n^{2.81}}$.
  *   **Strassen beats the $n^3$ limit!** This is why we study recursion trees—to identify where we can "prune" a branch (reduce $a$) to lower the exponent of the complexity.
  
  ---
- ### Practice Questions (Final Set)
  
  **Q1: The Branching Factor**
  If we modified the algorithm to split the matrix into **9** smaller blocks instead of 4 quadrants, and the recurrence became $T(n) = 9T(n/3) + n^2$:
  1.  Is this tree root-heavy, leaf-heavy, or balanced?
  2.  What is the complexity?
  
  **Q2: The Combination Cost**
  If matrix addition was somehow slower, say $O(n^3)$ instead of $O(n^2)$, what would the total complexity of the 8-multiplication version ($T(n) = 8T(n/2) + n^3$) become?
  *(Hint: Compare $n^3$ vs $n^{\log_2 8}$).*
  
  **Q3: Space Complexity**
  Every level of the tree creates 8 smaller matrices. Looking at the recursion tree, why is the space complexity for this recursive matrix multiplication much worse than the iterative version?