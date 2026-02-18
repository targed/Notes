### 1. The Arithmetic Series (Slide 27)
**Pattern:** The work decreases or increases by a **constant amount** at each step (e.g., $N + (N-1) + (N-2) \dots$).
*   **Recurrence Example:** $T(n) = T(n-1) + n$ (like Selection Sort).
*   **Formula:** $\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$
*   **Big O Result:** **$\Theta(n^2)$**
*   **Deep Dive Note:** If you see a "spindle" tree (one branch, decreasing by 1), it is almost always quadratic.
- ### 2. The Geometric Series (Slides 27–28)
  This is the most common result for Divide and Conquer trees. It occurs when the work at each level changes by a **constant ratio ($r$)**.
  
  **The Formula (Slide 27):**
  $$ \text{Sum} = \frac{x^{n+1} - 1}{x - 1} $$
- #### Case A: The Decreasing Series ($r < 1$)
  *   **Scenario:** The Root does the most work, and every level below it gets significantly smaller.
  *   **Example:** $T(n) = 2T(n/2) + n^2$ (Work per level: $n^2, n^2/2, n^2/4 \dots$)
  *   **The "Deep Dive" Rule:** If the series decreases, the **Root Dominates**. The total work is simply $O(\text{Root Cost})$.
  *   **Result:** $O(n^2)$.
- #### Case B: The Increasing Series ($r > 1$)
  *   **Scenario:** The work at the bottom (the leaves) is much larger than the work at the top.
  *   **Example:** $T(n) = 4T(n/2) + n$ (Work per level: $n, 2n, 4n, 8n \dots$)
  *   **The "Deep Dive" Rule:** If the series increases, the **Leaves Dominate**. The total work is $O(\text{Number of Leaves})$.
  *   **Math Check:** Number of leaves = $a^h = 4^{\log_2 n} = n^{\log_2 4} = n^2$.
  *   **Result:** $O(n^2)$.
- #### Case C: The Steady Series ($r = 1$)
  *   **Scenario:** Every level of the tree does the exact same amount of work.
  *   **Example:** Merge Sort: $T(n) = 2T(n/2) + n$. (Work per level: $n, n, n, n \dots$)
  *   **The "Deep Dive" Rule:** Total work = $(\text{Work per level}) \times (\text{Height})$.
  *   **Result:** $O(n \log n)$.
  
  ---
- ### 3. The "Reduction" Shortcut (Slide 28)
  Slide 28 shows a specific proof for the Tournament Tree (`findMax` function).
  *   **Work per level:** $1 + 2 + 4 + 8 \dots + 2^{\log n}$.
  *   This is an increasing geometric series where $x = 2$.
  *   Using the formula: $\frac{2^{\log n + 1} - 1}{2 - 1} = 2 \cdot 2^{\log n} - 1 = \mathbf{2n - 1}$.
  *   **Quiz Conclusion:** This proves that even though the algorithm is recursive, it performs exactly the same number of "nodes" of work as a linear scan.
  
  ---
- ### 4. Space Complexity Summary (Slide 30)
  Professors love to ask for both Time and Space on the same recurrence.
  *   **Time:** Look at the **Total Work** in all nodes.
  *   **Space:** Look at the **Tree Height** (Stack Depth).
  *   **Example:** In a Tournament Tree findMax:
    *   Time: $O(n)$ (Sum of all nodes).
    *   Space: $O(\log n)$ (Maximum depth of the recursion stack).
  
  ---
- ### Module Summary: Recursion Tree Method
  1.  **Draw the Tree:** Root cost, branching factor, subproblem size.
  2.  **Calculate Levels:** Find the work done at Level 0, Level 1, and Level $i$.
  3.  **Identify Series Type:** Decreasing (Root wins), Increasing (Leaves win), or Steady (Work $\times$ Height).
  4.  **Height:** $n / b^h = 1 \implies h = \log_b n$.
  
  ---
- ### Final Practice Questions (Recursion Trees)
  
  **Q1: The Steady Level**
  Draw the tree for $T(n) = 3T(n/3) + n$.
  1. What is the work at Level 0?
  2. What is the work at Level 1?
  3. What is the height?
  4. What is the Big O?
  
  **Q2: The Root Dominance**
  Draw the tree for $T(n) = 2T(n/2) + n^3$.
  1. Does the work increase or decrease as you go down?
  2. Based on the "Dominance" rule, what is the Big O?
  
  **Q3: The Leaf Count**
  For the recurrence $T(n) = 9T(n/3) + n$:
  1. What is the height of the tree?
  2. How many leaves are there at the bottom?
  3. Does the work at the root ($n$) or the work at the leaves dominate?
  
  **Think about Q2. If you see $n^3$ at the root, do you even need to do the math for the leaves?**