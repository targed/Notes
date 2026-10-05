### 1. The Anatomy of a Recurrence
The slides use the standard form: $T(n) = aT(n/b) + f(n)$. In a recursion tree, each part of this equation has a physical location:
*   **$f(n)$ (The Root):** This represents the work done at the **current level** (partitioning the data and combining results).
*   **$a$ (Branching Factor):** The number of child nodes emerging from each parent.
*   **$n/b$ (Problem Size):** The size of the input passed down to the next level.
- ### 2. The Step-by-Step Expansion ($T(n) = 2T(n/2) + cn$)
  This recurrence is the foundation for **Merge Sort**. Let's deconstruct how the tree is built:
  
  *   **Level 0 (The Top):** We have one problem of size $n$. The cost of work done here is $cn$.
  *   **Level 1:** We split into **2** ($a$) subproblems. Each subproblem is size **$n/2$**. 
    *   The work at each node is $c(n/2)$.
    *   Total work at Level 1: $c(n/2) + c(n/2) = \mathbf{cn}$.
  *   **Level 2:** Each of the two nodes splits again. We now have 4 subproblems of size **$n/4$**.
    *   The work at each node is $c(n/4)$.
    *   Total work at Level 2: $4 \times c(n/4) = \mathbf{cn}$.
  
  **The Key Observation:** In this specific "Merge Sort" template, the total work done at **every single level** is identical ($cn$). 
  
  ---
- ### 3. Calculating the Dimensions (The "Professor" Deep Dive)
  To find the total time complexity, we need to know how many levels exist and how much work is at the bottom.
- #### A. Tree Height ($h$)
  How many times can we divide $n$ by $b$ (which is 2 here) until we reach 1?
  *   $n / 2^h = 1 \implies n = 2^h \implies \mathbf{h = \log_2 n}$.
  *   *Note:* There are actually $\log n + 1$ levels if you count the root as Level 0.
- #### B. The Leaf Level (Base Cases)
  At the very bottom of the tree, the subproblems have reached size 1 ($T(1)$).
  *   How many leaves are there? In a tree with branching factor $a$ and height $h$, there are $a^h$ leaves.
  *   For Merge Sort: $2^{\log_2 n} = \mathbf{n}$ leaves.
  *   Each leaf usually costs a constant amount of work ($T(1) = \Theta(1)$).
  *   Total work at the leaf level: $n \times \text{constant} = \mathbf{O(n)}$.
  
  ---
- ### 4. The Final Summation
  Total Work = (Work at all internal levels) + (Work at the leaf level).
  *   Internal levels work: $(\text{Work per Level}) \times (\text{Number of Levels}) = cn \times \log n$.
  *   Leaf level work: $O(n)$.
  *   **Total:** $cn \log n + n \rightarrow \mathbf{\Theta(n \log n)}$.
- ### 5. Why use this over the Master Theorem?
  Your professor mentioned in the intro slides that the Master Theorem has "gaps" (cases where it doesn't work). The Recursion Tree Method is **universal**. It works for:
  *   **Non-symmetric splits:** e.g., $T(n) = T(n/3) + T(2n/3) + n$ (where the Master Theorem fails).
  *   **Varying work per level:** Cases where the work shrinks or grows exponentially as you go deeper.
  
  ---
- ### Deep Dive Practice: "Internal Work vs. Leaf Work"
  Professors often ask: **"Is this algorithm root-heavy, leaf-heavy, or balanced?"**
  
  1.  **Balanced:** Work is the same at every level (like Merge Sort). Result: $f(n) \times \log n$.
  2.  **Root-Heavy:** Work decreases as you go down. The total work is dominated by the very first call. Result: $O(f(n))$.
  3.  **Leaf-Heavy:** Work increases as you go down. The total work is dominated by the number of leaves. Result: $O(\text{Number of Leaves})$.
  
  **Which one do you think Binary Search is?**
  *Recurrence:* $T(n) = 1T(n/2) + 1$.
  *   *Level 0:* 1
  *   *Level 1:* 1
  *   *Level 2:* 1
  *Think about the sum of work and the height.*