### 1. The "Root-Heavy" Case ($T(n) = 2T(n/2) + n^2$)
In Slide 5, your professor asks you to try a variation of Merge Sort where the "Combine" step is much more expensive ($n^2$ instead of $n$).

**Expansion:**
*   **Level 0 (Root):** Work = $n^2$.
*   **Level 1:** 2 subproblems of size $n/2$. 
  *   Work per node: $(n/2)^2 = n^2/4$.
  *   Total work Level 1: $2 \times (n^2/4) = \mathbf{n^2/2}$.
*   **Level 2:** 4 subproblems of size $n/4$.
  *   Work per node: $(n/4)^2 = n^2/16$.
  *   Total work Level 2: $4 \times (n^2/16) = \mathbf{n^2/4}$.

**The Pattern:** 
The work is **decreasing** by a factor of 2 at every level ($n^2, n^2/2, n^2/4 \dots$).
*   **Deep Dive Logic:** This is a **Decreasing Geometric Series**.
*   Total Work $\approx n^2 (1 + 1/2 + 1/4 + 1/8 \dots)$.
*   As we know from the sum of infinite series (Slide 16), $(1 + 1/2 + 1/4 \dots)$ converges to **2**.
*   **Result:** $T(n) = 2n^2 \rightarrow \mathbf{O(n^2)}$.
*   *Professor's "Takeaway":* If the work at the root is significantly larger than the work at the leaves, the root work "swallows" the recursion. The recursion is effectively "free."

---
- ### 2. The Asymmetric Tree ($T(n) = T(n/4) + T(n/2) + n^2$)
  This is the "shaggy" tree shown in **Slides 7–15**. This is a classic "hard" quiz question because you cannot use the standard Master Theorem here (since $n/4$ and $n/2$ are different).
  
  **Expansion:**
  *   **Level 0:** $n^2$.
  *   **Level 1:** $(n/4)^2 + (n/2)^2$
    *   $n^2/16 + n^2/4 = n^2/16 + 4n^2/16 = \mathbf{\frac{5}{16}n^2}$.
  *   **Level 2:** Apply the same logic to the next layer.
    *   Total work = $(\frac{5}{16})^2 n^2$.
  
  **The Summation (Slide 15):**
  $$T(n) = n^2 + \frac{5}{16}n^2 + \left(\frac{5}{16}\right)^2 n^2 + \dots$$
  $$T(n) = n^2 \sum_{i=0}^{\infty} \left(\frac{5}{16}\right)^i$$
  
  **Solving with Slide 16 (Geometric Series Refresher):**
  *   Here, $x$ (the ratio) is $5/16$.
  *   Since $5/16 < 1$, the series converges to a constant.
  *   $T(n) = n^2 \times (\text{some constant}) \rightarrow \mathbf{\Theta(n^2)}$.
  
  ---
- ### 3. Problem Classification Summary (Slide 6)
  Your professor provided a table of three foundational algorithms. Let's look at the tree behavior for each:
  
  | Algorithm | Recurrence | Tree Behavior | Total Complexity |
  | :--- | :--- | :--- | :--- |
  | **Merge Sort** | $2T(n/2) + n$ | **Balanced:** Work is same at every level. | $O(n \log n)$ |
  | **Tournament Tree** | $2T(n/2) + 1$ | **Leaf-Heavy:** Work increases as you go down ($1, 2, 4 \dots$). | $O(n)$ |
  | **Binary Search** | $1T(n/2) + 1$ | **Single Path:** Only 1 node per level. | $O(\log n)$ |
  
  ---
- ### 4. Mathematical Tools: The Geometric Series (Slide 16)
  You **must** memorize this formula for the quiz. It is how you "finish" a recursion tree problem.
  
  **The Finite Sum:** 
  $$\sum_{k=0}^{n} x^k = \frac{1 - x^{n+1}}{1 - x}$$
  
  **The Infinite Sum (if $x < 1$):**
  $$\sum_{k=0}^{\infty} x^k = \frac{1}{1 - x}$$
  
  ---
- ### "Professor-Level" Deep Dive: The Tournament Tree Logic
  Look closely at **Tournament Tree** ($2T(n/2) + 1$).
  1.  Level 0: 1 unit of work.
  2.  Level 1: 2 nodes $\times$ 1 unit = 2.
  3.  Level 2: 4 nodes $\times$ 1 unit = 4.
  4.  **Observation:** The work doubles at every level! 
  5.  **Leaf level:** $n$ nodes.
  6.  **Sum:** $1 + 2 + 4 + \dots + n$. 
  7.  The sum of this powers-of-2 series is roughly $2n - 1$.
  8.  **Result:** $O(n)$.
  *This is why finding a maximum using D&C is no faster than a simple for-loop; the work is dominated by the number of leaves (the elements themselves).*
  
  ---
- ### Part 2 Practice Questions
  
  **Q1: The Convergence Test**
  Given $T(n) = 3T(n/4) + n^2$.
  1.  Calculate the work at Level 0 and Level 1.
  2.  Is this tree root-heavy, leaf-heavy, or balanced?
  3.  What is the final Big-O?
  
  **Q2: The "Shaggy" Tree Height**
  In the asymmetric tree $T(n) = T(n/4) + T(n/2) + n^2$:
  1.  What is the height of the **shortest** branch?
  2.  What is the height of the **longest** branch?
  3.  *Deep Dive:* Why does the "longest branch" determine the upper bound?
  
  **Q3: Constant Work**
  If $T(n) = 2T(n/2) + 5$, what is the complexity? Explain using the "Leaf-Heavy" logic.