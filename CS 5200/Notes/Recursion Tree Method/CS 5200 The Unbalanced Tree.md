### 1. The Challenge (Slide 23)
Consider the recurrence:
$$T(n) = T(n/4) + T(n/2) + n^2$$

**Why Master Theorem fails:**
The subproblems are $n/4$ and $n/2$. The "reduction factor" ($b$) is not a single constant. You cannot plug this into the standard formula. We **must** draw the tree.

---
- ### 2. Constructing the Unbalanced Tree
  
  **Level 0 (The Root):**
  *   Input size: $n$
  *   Work: $n^2$
  
  **Level 1:**
  *   The root splits into two children: one with size $n/4$ and one with size $n/2$.
  *   Work for Child 1: $(n/4)^2 = n^2/16$
  *   Work for Child 2: $(n/2)^2 = n^2/4$
  *   **Total Level 1 Work:** $n^2/16 + n^2/4 = \frac{1+4}{16}n^2 = \mathbf{\frac{5}{16}n^2}$
  
  **Level 2:**
  *   The $n/4$ node splits into $(n/16)$ and $(n/8)$.
  *   The $n/2$ node splits into $(n/8)$ and $(n/4)$.
  *   **Total Level 2 Work:** $(n/16)^2 + (n/8)^2 + (n/8)^2 + (n/4)^2$
    $$ \frac{1}{256}n^2 + \frac{1}{64}n^2 + \frac{1}{64}n^2 + \frac{1}{16}n^2 = \frac{1+4+4+16}{256}n^2 = \frac{25}{256}n^2 $$
  *   Notice the pattern: $\frac{25}{256}n^2$ is exactly **$(5/16)^2 n^2$**.
  
  ---
- ### 3. The Deep Dive: Path Lengths
  In a balanced tree, all branches hit the ground (input size 1) at the same time. In an unbalanced tree, the "tree" is actually **lopsided**.
  
  *   **The Shortest Path:** $n \to n/4 \to n/16 \dots$ 
    *   Height = $\log_4 n$.
  *   **The Longest Path:** $n \to n/2 \to n/4 \dots$ 
    *   Height = $\log_2 n$.
  
  **Quiz Insight:** Between depth $\log_4 n$ and $\log_2 n$, the tree starts to "thin out" because the shorter branches have already reached their base cases.
  
  ---
- ### 4. Deriving the Complexity
  We sum the levels:
  $$ T(n) = n^2 + \frac{5}{16}n^2 + \left(\frac{5}{16}\right)^2 n^2 + \dots + \text{Leaf Cost} $$
  $$ T(n) = n^2 \sum_{i=0}^{\infty} \left(\frac{5}{16}\right)^i $$
  
  **The Math:**
  This is a **Decreasing Geometric Series**. Because the ratio ($5/16$) is less than 1, the sum is dominated by the very first term (the Root).
  The infinite sum formula is $1 / (1 - r)$:
  $$ \text{Sum} = \frac{1}{1 - 5/16} = \frac{1}{11/16} = \frac{16}{11} \approx 1.45 $$
  
  **Final Result:**
  $$ T(n) = 1.45 n^2 = \Theta(n^2) $$
  
  ---
- ### Part 2 Practice Questions
  
  **Q1: The Constant Ratio**
  In the example above, the work decreased at every level ($n^2 \to \frac{5}{16}n^2$). 
  *   If we changed $n^2$ to just $n$ in the recurrence ($T(n) = T(n/4) + T(n/2) + n$), what would the total work per level be? 
  *   Would the work increase, decrease, or stay the same as you go deeper?
  
  **Q2: The "Spindle" Case**
  What happens to the tree if $T(n) = T(n-1) + n$? 
  *   Is this still a "tree"? 
  *   What is the height of this structure? 
  *   *(Hint: Think back to the space complexity of recursive sum!)*
  
  **Q3: Lower Bounds**
  For the unbalanced tree ($T(n) = T(n/4) + T(n/2) + n^2$), we proved an upper bound of $O(n^2)$. 
  *   How do we know the lower bound ($\Omega$) is also $n^2$? 
  *   *(Hint: Look at the root of the tree).*
  
  **Think about Q1 specifically. If the levels don't decrease, the "Root Dominance" logic fails.**