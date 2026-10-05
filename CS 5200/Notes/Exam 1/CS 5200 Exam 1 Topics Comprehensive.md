### **1. Asymptotic Analysis (Time & Space Complexity)**
*   **Formal Definitions & Proofs:** Big-O (Upper), Big-$\Omega$ (Lower), and Big-$\Theta$ (Tight). Knowing how to prove them using constants $c_1, c_2,$ and $n_0$.
*   **Time Complexity Pattern Matching:**
  *   Standard loops ($O(n)$, $O(n^2)$).
  *   Logarithmic loops (multiplying/dividing index by 2).
  *   Square root loops (`i * i <= n`).
  *   Arithmetic boundaries (like Quiz Q1: `sum += b` up to `a` $\rightarrow \Theta(a/b)$).
*   **Space Complexity:**
  *   Iterative (Auxiliary variables, usually $O(1)$).
  *   Recursive (Determined by **maximum call stack depth**, e.g., Quiz Q1 `traverse` function $\rightarrow \Theta(a)$).
*   **Growth Rate Comparisons:** Knowing the hierarchy (Constant $<$ Logarithmic $<$ Root $<$ Linear $<$ Linearithmic $<$ Polynomial $<$ Exponential $<$ Factorial). Recognizing exponent traps ($2^{n+10}$ vs $2^{2n}$).
- ### **2. Data Movement Complexity (External Memory Model)**
  *   **The Variables:** $N$ (Total items), $M$ (RAM capacity), $B$ (Block size), and $M/B$ (Blocks that fit in RAM).
  *   **Scanning:** Complexity of a contiguous array scan ($\lceil N/B \rceil + 1$) vs. a fragmented Linked List scan ($O(N)$).
  *   **Searching:**
    *   Standard Binary Search on disk: $O(\log_2(N/B))$ transfers.
    *   B-Tree Search: $O(\log_B N)$ transfers (and understanding why this is mathematically faster).
  *   **Sorting:** $M/B$-way External Merge Sort: $O(\frac{N}{B} \log_{M/B} \frac{N}{B})$.
  *   **Cache Eviction:** LRU (Online/Realistic) vs. Belady's Optimal (Offline). The Competitiveness Lemma (LRU with $M$ memory is as good as Optimal with $M/2$ memory).
  *   **Code Analysis:** Looking at a `for` loop (like Quiz Q2) and determining how many *cache lines* it touches.
- ### **3. Divide & Conquer Strategy and Applications**
  *   **Standard Merge Sort:** Divide, Conquer, Combine steps. $O(n \log n)$ time, $O(n)$ space.
  *   **Hybrid Merge Sort:** Switching to Insertion Sort for sub-arrays of size $k$. Formula: $O(nk + n \log(n/k))$.
  *   **Tournament Trees:** Finding Max, Min, or Second Max. Knowing the exact comparison counts ($N-1$ for max, $1.5N$ for max/min combined).
  *   **Inversions:**
    *   Defining Left, Right, and Split inversions.
    *   **Manual Tracing:** (Like Quiz Q3) Showing how the "Piggyback" trick works during the merge step (`count += mid - i + 1`).
  *   **Binary Search Variants (The "Tricks"):**
    *   Finding a Fixed Point / Serendipity ($A[i] = i$).
    *   Rotated Arrays (finding the pivot/local maxima, or finding a target value).
    *   Sparse Search (handling empty strings `""` by scanning for a pivot).
    *   Finding Surrounding Numbers (letting pointers cross to find floor/ceiling).
- ### **4. Solving Recurrences**
  *   **The Master Theorem:**
    *   Identifying $a$, $b$, and $f(n)$.
    *   Calculating the Critical Value ($n^{\log_b a}$).
    *   **Case 1 (Leaf-Heavy):** $n^{\log_b a} > f(n) \rightarrow \Theta(n^{\log_b a})$.
    *   **Case 2 (Balanced):** $n^{\log_b a} == f(n) \rightarrow \Theta(n^{\log_b a} \log n)$.
    *   **Case 3 (Root-Heavy):** $f(n) > n^{\log_b a} \rightarrow \Theta(f(n))$. (And checking the Regularity Condition).
    *   **The Gap:** Recognizing when the theorem fails (e.g., $f(n) = n \log n$ vs $n$).
  *   **The Recursion Tree Method:** (Like Quiz Q5)
    *   Drawing the nodes.
    *   Calculating work per level.
    *   Calculating tree height ($\log_b n$).
    *   Calculating number of leaves ($a^h$).
    *   Using Geometric Series formulas to sum the total work.
- ### **5. Dynamic Task Parallelism**
  *   **Concepts & Keywords:** DAGs, Strands, `spawn`, `sync`, `parallel for`.
  *   **The Metrics:** Work ($T_1$), Span/Critical Path ($T_\infty$), Parallelism ($T_1 / T_\infty$).
  *   **The Laws (Calculations):**
    *   Work Law: $T_p \ge T_1 / P$.
    *   Span Law: $T_p \ge T_\infty$.
    *   Brent’s Theorem (Greedy Bounds): $T_p \le \frac{T_1 - T_\infty}{P} + T_\infty$.
    *   **Refuting Claims:** Using the laws to prove a set of benchmarks is mathematically impossible (like HW Q4).
  *   **Parallel Matrix Math:**
    *   Simple parallel loops vs. Recursive block vs. Optimal reduction (and their respective parallelisms).
  *   **Parallel Merge Sort:**
    *   Naive (parallel sort, serial merge) vs. Optimal (parallel sort, parallel merge using binary search partitioning).
  *   **Race Conditions:** Identifying Determinacy Races (lost updates) and understanding why parallelizing inner loops dangerously shares memory.
- ### **6. Hashing & Bloom Filters**
  *   **Hashing Fundamentals:** Hash functions, compression (modulo), HashMaps, Load Factor ($\alpha$).
  *   **2-SUM and 3-SUM:** Comparing Brute Force vs. D&C vs. Hashing time complexities.
  *   **Open Addressing (Collisions):**
    *   **Linear Probing:** Manual tracing, understanding "Primary Clustering."
    *   **Double Hashing:** Manual tracing, calculating jump steps, rule that $h_2(k) \neq 0$.
  *   **Cuckoo Hashing:**
    *   Manual tracing of evictions between $T_1$ and $T_2$.
    *   Understanding the strict $O(1)$ worst-case lookup guarantee.
    *   Handling infinite loops (thresholds and rehashing).
  *   **Bloom Filters:**
    *   The Rules: No false negatives, possible false positives, no deletions.
    *   **The Math:** Calculating bits per key ($b = n/|S|$), False positive rate $(1 - e^{-m/b})^m$, and the Optimal Hash Functions formula ($m = b \ln 2$).
  
  ---