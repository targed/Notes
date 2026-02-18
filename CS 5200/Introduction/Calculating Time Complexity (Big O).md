## 1. What is Big O Notation?
**Slide Concept:** Big O represents the **Upper Bound** or **Worst Case**.

**Comprehensive Expansion:**
*   **The Definition:** Big O describes the limiting behavior of a function. If an algorithm is $O(N^2)$, it means that as $N$ grows, the run-time grows *at most* as fast as $N^2$.
*   **Why Worst Case?** (Slide 20 preview)
  *   We care about the "guarantee." If you are building a braking system for a car, you don't care about the *average* time it takes to brake; you need to know the *maximum* time it could possibly take.
*   **The Rules of Simplification:**
  1.  **Drop Constants:** $2N$ becomes $O(N)$. $500N$ becomes $O(N)$. $\frac{1}{2}N^2$ becomes $O(N^2)$.
  2.  **Drop Lower Order Terms:** If an algorithm takes $N^2 + N + 10$ steps, we only care about the dominant term ($N^2$). For huge inputs, the $+N$ becomes insignificant compared to the $N^2$.

---
- ## 2. Analyzing Code Snippets
- ### Case A: Simple Loop 
  **Scenario:** Searching one array for a value $t$.
  ```pseudocode
  for i := 1 to n do
    if A[i] = t then
        return TRUE
  return FALSE
  ```
  *   **Analysis:** The loop runs $N$ times. Inside the loop, the comparison (`if A[i] = t`) takes constant time, let's call it $c$.
  *   **Total Work:** $c \times N$.
  *   **Big O:** Drop the constant $c$.
  *   **Result:** **$O(N)$ (Linear Time)**.
- ### Case B: Sequential Loops 
  **Scenario:** Searching array A, *then* searching array B.
  ```pseudocode
  for i := 1 to n do       // Loop 1
    if A[i] = t ...
  for i := 1 to n do       // Loop 2
    if B[i] = t ...
  ```
  *   **Analysis:**
    *   Loop 1 runs $N$ times.
    *   Loop 2 runs $N$ times.
    *   Since they are **not nested** (one finishes before the next starts), we add them.
  *   **Total Work:** $N + N = 2N$.
  *   **Big O:** We drop the coefficient 2.
  *   **Result:** **$O(N)$ (Linear Time)**.
  *   *Note:* Many students mistakenly think "two loops = $N^2$". This is only true if they are nested!
- ### Case C: Nested Loops
  **Scenario:** Checking for a common element between Array A and Array B.
  ```pseudocode
  for i := 1 to n do           // Outer Loop
    for j := 1 to n do       // Inner Loop
        if A[i] = B[j] ...
  ```
  *   **Analysis:**
    *   For every single iteration of $i$ (which happens $N$ times), the inner loop $j$ runs fully ($N$ times).
  *   **Total Work:** $N \times N = N^2$.
  *   **Result:** **$O(N^2)$ (Quadratic Time)**.
- ### Case D: Dependent Nested Loops 
  **Scenario:** Checking for duplicates in a single array.
  ```pseudocode
  for i := 1 to n do
    for j := i + 1 to n do   // Note: j starts at i+1
        if A[i] = A[j] ...
  ```
  *   **Analysis:** This is the tricky one because the inner loop size *shrinks* as the outer loop progresses.
  *   **Step-by-Step Trace:**
    *   When $i = 1$, $j$ runs $(N-1)$ times.
    *   When $i = 2$, $j$ runs $(N-2)$ times.
    *   ...
    *   When $i = N-1$, $j$ runs $1$ time.
  *   **Mathematical Summation:**
    We need to sum the series: $(N-1) + (N-2) + \dots + 1$.
    This is the **Sum of the first $N$ integers**, famously solved by Gauss.
    $$ \text{Sum} = \frac{N(N-1)}{2} $$
    $$ \text{Sum} = \frac{N^2 - N}{2} $$
    $$ \text{Sum} = \frac{1}{2}N^2 - \frac{1}{2}N $$
  *   **Applying Big O Rules:**
    1.  Identify the dominant term: $\frac{1}{2}N^2$.
    2.  Drop the constant ($\frac{1}{2}$): $N^2$.
  *   **Result:** **$O(N^2)$**.
  
  **Takeaway:** Even though this algorithm runs half as many operations as Case C, in Big O notation, they are in the **same complexity class**.
  
  ---
- # Part 3 Preview: Complexity Cases & Difficulty
  
  
  **Question:**
  Determine the Big O complexity of the following snippet. Pay attention to the loop variables!
  
  ```pseudocode
  for i := 1 to N do
    for j := 1 to 1000 do   // Inner loop runs exactly 1000 times
        print(i, j)
  ```
  
  Is this $O(N)$, $O(N^2)$, or something else?