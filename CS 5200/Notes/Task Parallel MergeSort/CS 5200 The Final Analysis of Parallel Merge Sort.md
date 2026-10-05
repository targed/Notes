### 1. Putting it All Together (Slide 19)
Now that we have successfully parallelized the **Merge** subroutine, we can plug its result back into the main sorting algorithm to find the total Work and Span.

**The Full Recurrence for `P-MERGE-SORT`:**
1.  **Divide:** $O(1)$ to find the midpoint.
2.  **Recursive Spawns:** Two parallel calls of size $n/2$.
3.  **Parallel Merge:** The `P-MERGE` call we analyzed in Part 3.

---
- ### 2. Total Work ($T_1$)
  Work is the runtime on 1 processor (sequential projection).
  *   Even though we added complexity to the logic, the total number of comparisons and movements remains the same.
  *   **Recurrence:** $T_1(n) = 2T_1(n/2) + \Theta(n)$
  *   **Result:** $T_1(n) = \Theta(n \lg n)$. 
  *   *Conclusion:* Parallel Merge Sort does the same amount of "total effort" as standard Merge Sort.
  
  ---
- ### 3. Total Span ($T_\infty$)
  This is the critical path. We take the time of one recursive call (since they run in parallel) and add the time of the parallel merge.
  
  *   **Recurrence:** $T_\infty(n) = T_\infty(n/2) + \Theta(\lg^2 n)$
  *   **Solving with Master Theorem (Case 2):**
    *   $a = 1$, $b = 2$, $f(n) = \lg^2 n$.
    *   $n^{\log_b a} = n^0 = 1$.
    *   $f(n) = \lg^2 n$ is a power of $\lg n$ (where $k=2$).
    *   **The Rule:** If $f(n) = \Theta(\lg^k n)$, then $T(n) = \Theta(\lg^{k+1} n)$.
  *   **Result:** $T_\infty(n) = \Theta(\lg^3 n)$.
  
  ---
- ### 4. Parallelism: Naive vs. Optimal (Slide 20)
  
  *   **Naive Version (Sequential Merge):**
    *   Parallelism $= \frac{n \lg n}{n} = \mathbf{\Theta(\lg n)}$.
    *   It scales very poorly.
  *   **Optimal Version (Parallel Merge):**
    *   Parallelism $= \frac{n \lg n}{\lg^3 n} = \mathbf{\Theta\left(\frac{n}{\lg^2 n}\right)}$.
    *   This scales beautifully. As $n$ grows, the ability to use more processors grows almost linearly.
  
  ---
- ### 5. Quiz Application: The "10 Million" Problem (Slide 21)
  Your professor provided a table to calculate the actual parallelism for $n = 10,000,000$.
  
  **Assumptions:** $n = 10^7$. $\lg(10^7) \approx \mathbf{23}$ (since $2^{10} \approx 10^3$).
  
  | Algorithm | Parallelism Formula | Calculation for 10M | Max Processors |
  | :--- | :--- | :--- | :--- |
  | **Naive (Sequential Merge)** | $\lg n$ | $\sim 23$ | **23** |
  | **Optimal (Parallel Merge)** | $n / \lg^2 n$ | $10^7 / 23^2 \approx 10^7 / 529$ | **~18,900** |
  
  **Deep Dive Conclusion:** 
  The Naive version is a waste of hardware. If you buy a 100-core server, 77 cores will sit idle sorting 10 million numbers. The Optimal version can keep **nearly 19,000 cores** busy at the same time.
  
  ---
- ### 6. Advanced Tuning: Grain Size (Slides 22–28)
  Slide 28 introduces a "Freshman Calculus" concept to find the best **Grain Size ($g$)**.
  
  *   **The Problem:** Spawning a task has overhead. If we spawn a task for an array of size 1, we spend more time managing the thread than doing the math.
  *   **The Solution:** Stop parallelizing when the chunk size hits $g$, and finish the work sequentially.
  *   **The Math:** 
    *   Cost to spawn $= n/g$.
    *   Cost to execute the chunk sequentially $= g$.
    *   **Total Span** $= n/g + g$.
  *   **Optimization:** To find the minimum, take the derivative with respect to $g$:
    *   $f'(g) = -n/g^2 + 1 = 0 \implies \mathbf{g = \sqrt{n}}$.
  *   **Result:** Setting the grain size to $\sqrt{n}$ balances the "cost of management" with the "cost of execution" perfectly.
  
  ---
- ### Part 4 Practice Questions
  
  **Q1: The Master Theorem "k" Value**
  In the span recurrence $T_\infty(n) = T_\infty(n/2) + \lg^2 n$, we used Case 2 where $k=2$. If we had a "super-parallel" merge that only took $\Theta(\lg n)$ time, what would the total span of the sort be?
  
  **Q2: Amdahl's Law Reflection**
  Looking at the 10 Million numbers example, why does the "Naive" version's speedup cap at 23? Refer to the **Span Law** in your answer.
  
  **Q3: Grain Size Intuition**
  If $n = 1,000,000$, what is the optimal grain size ($g$)? If you set $g=1$, why does the algorithm perform poorly in a real-world environment?