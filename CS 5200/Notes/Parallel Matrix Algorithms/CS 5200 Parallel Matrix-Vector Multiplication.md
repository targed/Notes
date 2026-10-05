### 1. The Mathematical Goal (Slide 2)
We want to multiply an $n \times n$ matrix $A$ by an $n$-length vector $x$ to produce an $n$-length vector $y$.
*   **The Logic:** Each element $y_i$ of the result is the **dot product** of the $i$-th row of $A$ and the vector $x$.
*   **Sequential Complexity:** Since there are $n$ rows, and each row requires $n$ multiplications/additions, the work is $T_1 = \Theta(n^2)$.
- ### 2. Version 1: The Simple Parallel Loop (Slide 2)
  The most basic way to parallelize this is to make the **outer loop** parallel.
  
  ```c
  P-MAT-VEC(A, x, y, n)
  1 parallel for i = 1 to n   // Parallelize the row selection
  2     for j = 1 to n       // Serial dot product calculation
  3         y[i] = y[i] + A[i][j] * x[j]
  ```
  
  **Deep Dive Analysis:**
  *   Work ($T_1$): Still $\Theta(n^2)$. We haven't changed the number of multiplications.
  *   Span ($T_\infty$):
    1.  The `parallel for` control (spawning $n$ tasks) takes $\Theta(\lg n)$ time because it follows a binary tree structure of spawns.
    2.  The **inner loop** is sequential. It does $n$ steps of constant work.
    3.  $T_\infty = \Theta(\lg n) + \Theta(n) = \mathbf{\Theta(n)}$.
  *   **Parallelism:** $\frac{T_1}{T_\infty} = \frac{n^2}{n} = \mathbf{\Theta(n)}$.
    *   *Result:* This algorithm can effectively use $n$ processors.
  
  ---
- ### 3. Version 2: Recursive Divide & Conquer (Slides 3–4)
  To make this scale even better on massive systems, we can write it recursively by splitting the matrix into halves (top half and bottom half).
  
  **The Logic (Slide 3):**
  *   **Base Case:** If we only have one row left ($i == i'$), run the sequential dot product loop.
  *   **Recursive Step:** Find the `mid` row. `spawn` a task for the top half and call the bottom half recursively.
  
  **The Visualization (Slide 4):**
  The DAG for this looks like a **Binary Tree**. 
  *   The **leaves** of the tree are the actual row-vector multiplications.
  *   The **internal nodes** are the logic required to divide the ranges.
  
  ---
- ### 4. The Complexity Formula (Slide 5)
  Your professor highlights a general formula for the Span of a parallel loop:
  $$T_\infty(n) = \Theta(\lg n) + \max\{iter_\infty(i)\}$$
  
  *   $\Theta(\lg n)$: This is the overhead of the recursive spawning process (the height of the spawn tree).
  *   $\max\{iter_\infty(i)\}$: This is the execution time of the single slowest iteration of the inner loop.
  
  Why is it $\Theta(n)$? (Slide 6)
  In our matrix-vector example, every iteration of the inner loop (a dot product) takes exactly $n$ steps.
  $$T_\infty = \lg n + n \approx \mathbf{n}$$
  
  ---
- ### Why isn't this "Fast Enough"?
  On a pen-and-paper quiz, the professor might ask: **"Is Version 1 optimal?"**
  *   **The Answer:** No.
  *   **The Reason:** The Span is still linear ($n$). If $n$ is 1 million, we still have a sequential chain of 1 million operations for every row. 
  *   **The Goal:** We want a span of $\Theta(\lg n)$. To do that, we have to parallelize the **inner loop** (the dot product itself). This is covered later in the slides (Slide 12-15).
  
  ---
- ### Practice Questions
  
  **Q1: The Span Calculation**
  Suppose you have a matrix with $N=1024$. 
  1. How many levels of "spawning" occur in the recursive version?
  2. If the inner loop is sequential, how many total units of time does it take for **one** processor to finish a single row?
  3. What is the bottleneck preventing this from achieving a speedup greater than $N$?
  
  **Q2: The Formula**
  Using the formula $T_\infty(n) = \Theta(\lg n) + \max\{iter_\infty(i)\}$, calculate the span if the inner loop was changed to a recursive search that takes $\lg n$ time instead of a for-loop that takes $n$ time.
  
  **Q3: Task vs. Thread**
  Look at Figure 26.4 (Slide 4). If we had only 2 physical processors, how would the scheduler handle the 8 leaves (rows) of the tree?