### 1. The Sequential Baseline (Slide 16)
To compute $C = A \cdot B$:
*   There are $n^2$ entries in matrix $C$.
*   To calculate **each** entry $c_{ij}$, you perform a dot product of the $i$-th row of $A$ and the $j$-th column of $B$, which takes $n$ multiplications.
*   Sequential Work ($T_1$): $n^2 \times n = \mathbf{\Theta(n^3)}$.
- ### 2. Version 1: Simple Parallel Loops (Slides 18–19)
  The easiest way to parallelize this is to make the row and column loops parallel, but keep the dot product serial.
  
  ```c
  P-MATRIX-MULTIPLY(A, B, C, n)
  1 parallel for i = 1 to n
  2     parallel for j = 1 to n
  3         for k = 1 to n           // Serial inner loop
  4             cij = cij + aik * bkj
  ```
  
  **Deep Dive Analysis:**
  *   Work ($T_1$): $\Theta(n^3)$.
  *   Span ($T_\infty$): 
    1.  The two `parallel for` controls take $\Theta(\lg n) + \Theta(\lg n) = \Theta(\lg n)$.
    2.  The inner serial loop takes $\Theta(n)$.
    3.  **Total Span:** $\Theta(n) + \Theta(\lg n) = \mathbf{\Theta(n)}$.
  *   **Parallelism:** $n^3 / n = \mathbf{\Theta(n^2)}$.
  
  ---
- ### 3. Version 2: Recursive Block Multiplication (Slides 20–32)
  This uses the **Divide and Conquer** method we saw in the Recursion Tree module. We split the matrices into 8 sub-matrix multiplications.
  
  **The Recurrence (Slide 29):**
  *   Work ($T_1$): $M_1(n) = 8M_1(n/2) + \Theta(n^2)$.
    *   Using Master Theorem (Case 1): $a=8, b=2 \implies n^{\log_2 8} = n^3$. Since $n^3 > n^2$, **Work = $\Theta(n^3)$**.
  *   **Span ($T_\infty$):** (Slide 31)
    *   If we `spawn` all 8 multiplications, they run in the time of 1 multiplication.
    *   We must add the resulting matrices, which can be done in parallel in $\Theta(\lg n)$ time.
    *   **Span Recurrence:** $M_\infty(n) = M_\infty(n/2) + \Theta(\lg n)$.
    *   Using Master Theorem (Case 2 with $k=1$): **Span = $\Theta(\lg^2 n)$**.
  
  **Parallelism of Recursive Version:**
  $$\frac{n^3}{\lg^2 n}$$
  *   This is much better than the simple version!
  
  ---
- ### 4. Version 3: The "Optimal" Parallel Algorithm (Algorithm 4, Slides 33–39)
  Your professor introduces a "3rd algorithm" that is even faster. It combines the `parallel for` loops with the **Parallel Reduction** (`P-DOT-PROD`) we learned in the last section.
  
  **Algorithm 4 Logic:**
  1.  Use `parallel for` for the $i$ rows.
  2.  Use `parallel for` for the $j$ columns.
  3.  **Instead of a serial loop for $k$**, use `P-DOT-PROD` (Reduction tree) to calculate the entry.
  
  **The Math (Slide 38–39):**
  *   **Outer Control Span:** $\Theta(\lg n)$ (to spawn $n^2$ entries).
  *   **Inner `P-DOT-PROD` Span:** $\Theta(\lg n)$.
  *   **Total Span ($T_\infty$):** $\Theta(\lg n) + \Theta(\lg n) = \mathbf{\Theta(\lg n)}$.
  *   **Total Parallelism:**
    $$\frac{T_1}{T_\infty} = \mathbf{\Theta\left(\frac{n^3}{\lg n}\right)}$$
  
  ---
- ### 5. Professor Deep Dive: Comparing the Three
  A common "Paper and Pen" question will ask you to rank these by **Parallelism** (Ability to scale):
  
  1.  **Algorithm 4 (Reduction):** Parallelism $= n^3 / \lg n$. (**BEST**)
  2.  **Recursive Block:** Parallelism $= n^3 / \lg^2 n$. (**Great**)
  3.  **Simple Loop:** Parallelism $= n^2$. (**Good for small machines**)
  
  **The Determinacy Race Warning (Slide 11 repeated):**
  If you use `parallel for` for all three loops $(i, j, k)$ and just do `cij = cij + ...`, you will have a race condition because multiple threads will try to write to the same `cij` at once. You **must** use either the Recursive approach or the Reduction (`P-DOT-PROD`) approach to be safe and fast.
  
  ---
- ### Part 4 Practice Questions (Final Set)
  
  **Q1: The Master Theorem Gap**
  Look at the Span recurrence for the Recursive version: $M_\infty(n) = M_\infty(n/2) + \Theta(\lg n)$. 
  1.  Identify $a, b,$ and $f(n)$.
  2.  Prove why this is Case 2 and why the result is $\lg^2 n$.
  
  **Q2: The "Triple Parallel" Error**
  Why is `parallel for i, parallel for j, parallel for k` with a simple addition line considered "Wrong" on a quiz? 
  *   Which of the three variables ($i, j,$ or $k$) is the one causing the conflict?
  
  **Q3: Strassen Parallelism**
  Strassen's sequential work is $O(n^{2.81})$. If we parallelized it using the same method as Algorithm 4, what would its **Parallelism** be?
  *   *Hint:* Divide the work by the span ($\lg n$).