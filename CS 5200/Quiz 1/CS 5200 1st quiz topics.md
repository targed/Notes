- Graded Quiz on Feb 19th: 2026SP-COMP_SCI-5200-101
- Topics:
- Time, Space and Data Movement complexity
- Divide and Conquer Chapter
- ---
  
  ---
- ### I. Complexity Fundamentals (Time & Space)
  You must be able to look at a function or code snippet and provide the **Tight Bound ($\Theta$)**.
  
  *   **Growth Hierarchy:** Know the order: $1 < \log n < \sqrt{n} < n < n \log n < n^2 < n^3 < 2^n < n!$.
  *   **Simplification Rules:** 
    *   Drop constant coefficients ($5n^2 \to n^2$).
    *   Drop lower-order terms ($n^2 + n \log n \to n^2$).
  *   **Loop Analysis Patterns:**
    *   Standard loops: $O(n)$.
    *   Nested loops: $O(n^2)$.
    *   **The Doubling Loop:** `i = i * 2` $\to O(\log n)$.
    *   **The Square Root Loop:** `i * i <= n` $\to O(\sqrt{n})$.
  *   **Formal Definitions:** 
    *   Be ready to define $O$ (Upper), $\Omega$ (Lower), and $\Theta$ (Tight) using $c$ and $n_0$.
    *   **The Exponent Trap:** $2^{n+10} = O(2^n)$ (**True**), but $2^{2n} \neq O(2^n)$ (**False**).
  *   **Space Complexity:**
    *   **Iterative:** Usually $O(1)$ unless an array is created.
    *   **Recursive:** Determined by the **Max Stack Depth** (e.g., recursive sum is $O(n)$, but recursive Fibonacci is $O(n)$ despite the time being $O(2^n)$).
  
  ---
- ### II. Data Movement Complexity (External Memory)
  This focuses on the "I/O bottleneck" when data doesn't fit in RAM.
  
  *   **Variables:** Master $N$ (elements), $M$ (RAM size), and $B$ (Block size).
  *   **Scanning:** $O(N/B)$. (Understand why the $+1$ alignment exists).
  *   **Searching:** 
    *   Binary Search: $O(\log_2(N/B))$.
    *   **B-Tree Search:** $O(\log_B N)$. (Know the math: $\lg N / \lg B$).
  *   **Sorting:**
    *   Binary Merge Sort on disk: $O((N/B) \log_2 (N/M))$.
    *   **Multi-way Merge Sort:** $O((N/B) \log_{M/B} (N/B))$.
  *   **Eviction:** Know the difference between **LRU** (Online/Realistic) and **Belady’s Optimal** (Offline/Future-seeing).
  
  ---
- ### III. Divide and Conquer: Algorithms & Applications
  You should be able to write **pseudocode** for these and explain their logic.
  
  *   **Standard Merge Sort:** 3 steps (Divide, Conquer, Combine). $O(n \log n)$ time, $O(n)$ space.
  *   **Hybrid Merge Sort:** Using Insertion Sort for sub-arrays of size $k$. 
    *   Formula: $O(nk + n \log(n/k))$.
  *   **Tournament Tree:** Finding Max/Min in $O(n)$ with $n-1$ comparisons. Good for parallelism.
  *   **Inversion Counting:** Finding "out of order" pairs.
    *   The "Piggyback" trick: Incrementing count by `(mid - i + 1)` when a right-side element is smaller than a left-side element during Merge.
  *   **Binary Search Variants:**
    *   **Fixed Point (Serendipity):** Finding $A[i] = i$.
    *   **Rotated Arrays:** Finding a target or the Maxima (pivot) in $O(\log n)$.
    *   **Sparse Search:** Handling empty strings by scanning for the nearest non-empty pivot.
  
  ---
- ### IV. Solving Recurrences (The Math Toolbox)
  You will almost certainly have to solve at least one recurrence using both methods.
  
  *   **Recursion Tree Method:** 
    *   Draw the levels.
    *   Calculate **Work per Level**.
    *   Calculate **Tree Height**.
    *   Calculate **Leaf Work** ($a^h$).
  *   **Master Theorem:** $T(n) = aT(n/b) + f(n)$.
    *   **Case 1:** $f(n)$ is smaller than $n^{\log_b a} \to T(n) = \Theta(n^{\log_b a})$.
    *   **Case 2:** $f(n)$ matches $n^{\log_b a} \to T(n) = \Theta(n^{\log_b a} \log n)$.
    *   **Case 3:** $f(n)$ is larger than $n^{\log_b a} \to T(n) = \Theta(f(n))$ (Check Regularity!).
    *   **The Gap:** Master Theorem fails if the gap is only logarithmic (e.g., $n \lg n$ vs $n$).
  
  ---
- ### V. Parallel Algorithms
  *   **Keywords:** `spawn` (fork), `sync` (join), `parallel for`.
  *   **Metrics:** 
    *   **Work ($T_1$):** Total nodes in DAG.
    *   **Span ($T_\infty$):** Longest path in DAG (Critical Path).
    *   **Parallelism:** $T_1 / T_\infty$.
  *   **Laws:** 
    *   Work Law ($T_p \ge T_1/P$) and Span Law ($T_p \ge T_\infty$).
    *   **Brent’s Theorem:** $T_p \le T_1/P + T_\infty$.
  *   **Race Conditions:** Definition of a **Determinacy Race** (two parallel tasks, same memory, at least one write).
  
  ---
- ### Suggested Study Strategy
  1.  **Algebra Check:** Can you find the pivot $x$ and $n_0$ for a $\Theta$ proof?
  2.  **Tree Check:** Can you solve $T(n) = 3T(n/4) + n^2$ by drawing the tree?
  3.  **D&C Check:** Can you explain why we pick the median of the **larger** array in Parallel Merge?
  4.  **IO Check:** Can you calculate why a B-tree is faster than Binary Search if $N=1$ Billion and $B=1000$