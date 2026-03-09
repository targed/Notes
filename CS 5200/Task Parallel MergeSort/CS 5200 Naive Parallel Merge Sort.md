### 1. The Starting Point: Sequential Merge Sort (Slides 2–5)
We recall the standard sequential version of Merge Sort:
1.  **Divide:** Find the midpoint ($O(1)$).
2.  **Conquer:** Recursively sort the left and right halves ($2T(n/2)$).
3.  **Combine:** Use the `MERGE` function to zip the two sorted lists together ($\Theta(n)$).

**Recurrence:** $T(n) = 2T(n/2) + \Theta(n)$.
**Solution:** $T(n) = \Theta(n \lg n)$.

---
- ### 2. The "Naive" Parallel Version (Slides 6–7)
  The most obvious way to parallelize Merge Sort is to use the `spawn` keyword for the recursive calls. Because the left half and the right half of the array are **disjoint** (they don't overlap), they can be sorted at the same time without any race conditions.
  
  ```c
  P-NAIVE-MERGE-SORT(A, p, r)
  1 if p >= r return
  2 q = (p + r) / 2
  3 spawn P-NAIVE-MERGE-SORT(A, p, q)       // Spawn Left
  4 P-NAIVE-MERGE-SORT(A, q + 1, r)         // Run Right
  5 sync                                   // Wait for both
  6 MERGE(A, p, q, r)                      // SEQUENTIAL MERGE
  ```
  
  ---
- ### 3. The Deep Dive Analysis: Why is this bad? (Slide 8)
  Let's analyze the **Work** ($T_1$) and **Span** ($T_\infty$) of this naive version.
  
  **Work ($T_1$):**
  The work is identical to the sequential version because we are doing the exact same number of operations.
  *   **$T_1(n) = \Theta(n \lg n)$**.
  
  **Span ($T_\infty$):**
  This is where the calculation gets interesting. 
  1.  The two recursive calls run in parallel, so we only count one of them toward the critical path ($T_\infty(n/2)$).
  2.  **Crucially**, the `MERGE` step is still **sequential**. It takes $\Theta(n)$ time.
  3.  **Span Recurrence:** $T_\infty(n) = T_\infty(n/2) + \Theta(n)$.
    *   This is Master Theorem **Case 3** (The work at the root dominates).
    *   **$T_\infty(n) = \Theta(n)$**.
  
  **Parallelism Result:**
  $$\text{Parallelism} = \frac{T_1}{T_\infty} = \frac{n \lg n}{n} = \mathbf{\Theta(\lg n)}$$
  
  **The "Professor Deep Dive" Realization:**
  If you sort **1 million elements** ($n = 10^6$):
  *   $\lg(1,000,000) \approx \mathbf{20}$.
  *   Even if you have 1,000 processors, this algorithm can only effectively use **20** of them. The other 980 processors will sit idle because they are waiting for the sequential `MERGE` function to finish its linear scan.
  *   **Conclusion:** This version does not scale. To get real speedup, we must create a **Parallel Merge**.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1: The Recurrence Match**
  Why does the Span recurrence $T_\infty(n) = T_\infty(n/2) + \Theta(n)$ result in $\Theta(n)$ and not $\Theta(n \lg n)$? 
  *   *Hint:* Compare the work done at the root ($n$) vs the work done at the leaves ($n^0=1$).
  
  **Q2: Amdahl's Law Intuition**
  In this naive algorithm, which part of the code represents the "sequential portion" that limits our speedup?
  
  **Q3: Recursive Overhead**
  In Slide 7, it says sequential merge is "slow, less parallelism." If we have an array of size 8, how many total times is the sequential `MERGE` function called? Does parallelizing the `spawn` lines reduce this count?