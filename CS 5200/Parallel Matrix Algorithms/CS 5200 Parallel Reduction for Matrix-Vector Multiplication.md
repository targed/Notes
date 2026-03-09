### 1. The Strategy: Parallel Reduction (Slide 13)
To avoid the "lost update" race condition, we must stop every thread from trying to write to the same `y[i]` variable at once. 

**The Concept:**
Instead of a serial loop that adds elements one by one, we use **Reduction**. We pair up the numbers and add them in a tree structure (like a sports tournament).
*   **Step 1:** Multiply all pairs ($a_{ij} \cdot x_j$) in parallel.
*   **Step 2:** Add pairs of results in parallel.
*   **Step 3:** Repeat until you have one final sum.

---
- ### 2. The Algorithm: `P-PROD` (Slide 14–15)
  This is the recursive "Reduction" function used for a single row’s dot product.
  
  ```text
  Algorithm 1: P-PROD(a, x, j, j')
  1: if j == j' then
  2:    return a[j] * x[j]    // Base case: multiply the single pair
  3: end if
  4: mid = (j + j') / 2
  5: a' = spawn P-PROD(a, x, j, mid)      // Parallelize Left half
  6: x' = P-PROD(a, x, mid + 1, j')        // Run Right half
  7: sync                                 // Wait for both
  8: return a' + x'                       // Combine (safe from races!)
  ```
  
  **Why is this race-free?**
  In the "Wrong" version, threads shared a single memory location. In this version, `a'` and `x'` are **local variables** on the function's stack frame. They don't exist in the same memory space. The addition `a' + x'` only happens *after* the `sync`, ensuring one thread handles that specific addition.
  
  ---
- ### 3. Complexity Analysis: The "Deep Dive" Math
  This is the "Improved Version" promised on Slide 12.
- #### A. Work ($T_1$)
  *   We are still performing the same number of multiplications ($n$) and additions ($n-1$) per row.
  *   For $n$ rows: **$T_1 = \Theta(n^2)$**.
- #### B. Span ($T_\infty$)
  1.  **Outer Parallel Loop:** Spawning $n$ rows takes **$\Theta(\lg n)$**.
  2.  **Inner `P-PROD`:** The depth of the recursive reduction tree for $n$ elements is **$\Theta(\lg n)$**.
  3.  **Total Span:** Since these are nested (we run the tree *inside* the loop), we add the spans.
    *   $T_\infty = \Theta(\lg n) + \Theta(\lg n) = \mathbf{\Theta(\lg n)}$.
- #### C. Parallelism
  $$\text{Parallelism} = \frac{T_1}{T_\infty} = \frac{n^2}{\lg n}$$
  *   This is a massive improvement over the simple version ($\Theta(n)$). For a matrix of size 1 million, this version can effectively utilize roughly **50,000 processors** ($10^{12} / 20$).
  
  ---
- ### 4. Professor Deep Dive: The $O(n^2)$ Work Constraint
  Slide 12 explicitly mentions: *"Maintaining $\Theta(n^2)$ work."*
  *   **The Nuance:** Sometimes, when you make an algorithm parallel, you accidentally increase the total amount of work (e.g., by duplicating calculations). 
  *   In this reduction model, we only do exactly $n$ multiplications and $n-1$ additions. We haven't added any "extra" math, just reorganized it. Therefore, we didn't waste any energy/computation.
  
  ---
- ### Practice Questions
  
  **Q1: The `sync` logic**
  In `P-PROD`, why is the `sync` located on line 7 and not on line 6? What would happen if we put `sync` on line 5.5 (immediately after the spawn)?
  *   *Hint:* Does the parent wait or continue?
  
  **Q2: Comparing Spans**
  We have three versions of Matrix-Vector multiplication now:
  1.  **Sequential:** Span $= n^2$.
  2.  **Parallel Outer Loop / Serial Inner:** Span $= n$.
  3.  **Parallel Outer / Parallel Reduction Inner:** Span $= \lg n$.
  *   If $n = 1024$, calculate the Span for all three.
  
  **Q3: Hardware Limits**
  If your computer only has **4** cores, will the "Improved Version" actually be 50,000 times faster? (Think back to the **Work Law**).