### 1. The Components of the Algorithm (Slides 12–14)
To analyze the total time, we look at the work done inside one call to `P-MERGE-AUX`:

*   **Finding the Median ($q_1$):** Constant time **$O(1)$**.
*   **Binary Search ($q_2$):** We search for $x$ in the second array. As established in Slide 12, this takes $\Theta(\lg n)$ time.
*   **The Spawns:** We create two recursive calls. Because they are `spawned`, they run in parallel.
*   **The Combine:** There is no combine work! Once the recursive merges are done, the array is already sorted.
- ### 2. The Worst-Case Split: The "3/4" Rule (Slide 17)
  In standard Merge Sort, we always split perfectly: $n/2$ and $n/2$. 
  In Parallel Merging, the split might be lopsided because we are splitting two different arrays using a single pivot from one of them.
  
  **The Math:**
  1.  We pick the median of the larger array (size $n_1$). So we have $n_1/2$ elements.
  2.  We split the smaller array (size $n_2$) at some point $q_2$. In the absolute worst case, $q_2$ could be at the very end (all elements are smaller than the pivot).
  3.  **Total elements in one sub-branch:** $n_1/2 + n_2$.
  4.  Since $n_1$ was the larger array, we know $n_1 \ge n/2$. 
  5.  Through the substitution shown on Slide 17, we prove that even in this "worst" lopsided case, the sub-problem is at most **$3n/4$**.
  
  **Why this matters:** A $3n/4$ split is still "logarithmic." The tree will be slightly deeper than a $n/2$ tree, but it won't become linear.
  
  ---
- ### 3. Deriving the Span ($T_\infty$) of Parallel Merge
  The **Span** is the longest path through the recursion. 
  *   In each step, we do $\Theta(\lg n)$ work (the Binary Search).
  *   Then we recurse into a problem of size (at most) $3n/4$.
  
  **The Recurrence (Slide 17):**
  $$T_\infty(n) = T_\infty(3n/4) + \Theta(\lg n)$$
  
  **Solving with Master Theorem (Slide 18):**
  1.  **Parameters:** $a=1$ (we follow the longest single path), $b=4/3$, $f(n) = \lg n$.
  2.  **Critical Value:** $n^{\log_{4/3} 1} = n^0 = \mathbf{1}$.
  3.  **Comparison:** $f(n) = \lg n$.
  4.  **Case 2 ($k=1$):** When $f(n) = \Theta(n^{\log_b a} \lg^k n)$, the result is $\Theta(n^{\log_b a} \lg^{k+1} n)$.
    *   Here: $1 \cdot \lg^{1+1} n = \mathbf{\lg^2 n}$.
  
  **Result:** The Span of the Parallel Merge is **$\Theta(\lg^2 n)$**.
  
  ---
- ### 4. Deriving the Work ($T_1$) of Parallel Merge (Slide 33)
  **Work** is the total cost on one processor.
  *   Even though we use a complex tree, we are still just moving each of the $n$ elements into the output array exactly once.
  *   **Recurrence:** $T_1(n) = T_1(\alpha n) + T_1((1-\alpha) n) + \lg n$.
  *   **Result:** **$T_1(n) = \Theta(n)$**.
  *   *Professor Deep Dive:* This is perfect. We achieved parallelism without increasing the total amount of work compared to a sequential scan.
  
  ---
- ### 5. Summary of Parallel Merge Performance
  *   **Work:** $\Theta(n)$
  *   **Span:** $\Theta(\lg^2 n)$
  *   **Parallelism:** $\frac{n}{\lg^2 n}$
  *   **Compare to Sequential Merge:** Sequential merge has a span of $O(n)$. Our new version is **exponentially faster** in terms of span!
  
  ---
- ### Part 3 Practice Questions
  
  **Q1: The Span Difference**
  In Part 1, we saw that using a sequential merge resulted in a span of $\Theta(n)$. Now, with Parallel Merge, we have a span of $\Theta(\lg^2 n)$. 
  *   If $n = 1,000,000$, what is the approximate difference in "time units" on the critical path? (Assume $\lg 10^6 \approx 20$).
  
  **Q2: The Master Theorem Check**
  In the span recurrence $T_\infty(n) = T_\infty(3n/4) + \Theta(\lg n)$, what happens to the result if the Binary Search was even faster, say $O(1)$?
  *   *Hint:* If $f(n) = 1$, does Case 2 still apply?
  
  **Q3: Recursive Tree vs. Span**
  Look at the `P-MERGE-AUX` code. It has **two** spawns (Line 12 and Line 14). Why does the Span recurrence only say $1 \cdot T_\infty(3n/4)$ and not $2 \cdot T_\infty$? 
  *   *Hint:* Revisit the definition of Span from the Parallelism module.