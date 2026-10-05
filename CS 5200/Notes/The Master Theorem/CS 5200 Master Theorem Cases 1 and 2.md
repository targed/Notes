### 1. Case 1: The Leaves Dominate (Leaf-Heavy)
**Mathematical Definition:** $f(n) = O(n^{\log_b a - \epsilon})$ for some constant $\epsilon > 0$.
**Translation:** $n^{\log_b a}$ is **polynomially larger** than $f(n)$.

**The Logic:** 
In the recursion tree, the work at the root is $f(n)$. As you go down, the number of subproblems ($a$) grows faster than the work per subproblem shrinks. By the time you reach the bottom, the total number of leaves is so massive that the work done there dwarfs everything above it.

**Example 1: Naive Matrix Multiplication (Slide 8)**
*   $T(n) = 8T(n/2) + O(n^2)$
*   $a = 8, b = 2, f(n) = n^2$
*   Critical Value: $n^{\log_2 8} = \mathbf{n^3}$
*   **Comparison:** Is $n^3$ polynomially larger than $n^2$? **Yes.** (Gap is $n^1$).
*   **Result:** **$T(n) = \Theta(n^3)$**.

**Example 2: Strassen’s Algorithm (Slide 10)**
*   $T(n) = 7T(n/2) + O(n^2)$
*   $a = 7, b = 2, f(n) = n^2$
*   Critical Value: $n^{\log_2 7} \approx \mathbf{n^{2.81}}$
*   **Comparison:** Is $n^{2.81}$ polynomially larger than $n^2$? **Yes.** (Gap is $n^{0.81}$).
*   **Result:** **$T(n) = \Theta(n^{\log_2 7}) \approx O(n^{2.81})$**.

---
- ### 2. Case 2: The Perfect Balance (Tie)
  **Mathematical Definition:** $f(n) = \Theta(n^{\log_b a})$.
  **Translation:** $f(n)$ and $n^{\log_b a}$ are the same size.
  
  **The Logic:**
  Every level of the recursion tree does the **same amount of work**. To get the total, you simply take the work of one level and multiply it by the height of the tree ($\log n$).
  
  **Example 1: Merge Sort (Slide 13)**
  *   $T(n) = 2T(n/2) + n$
  *   $a = 2, b = 2, f(n) = n$
  *   Critical Value: $n^{\log_2 2} = n^1 = \mathbf{n}$
  *   **Comparison:** $f(n) = n$ and Critical Value $= n$. They are **equal**.
  *   **Result:** **$T(n) = \Theta(n \log n)$**.
  
  **Example 2: Binary Search (Slide 16)**
  *   $T(n) = 1T(n/2) + \Theta(1)$
  *   $a = 1, b = 2, f(n) = 1$
  *   Critical Value: $n^{\log_2 1} = n^0 = \mathbf{1}$
  *   **Comparison:** $f(n) = 1$ and Critical Value $= 1$. They are **equal**.
  *   **Result:** **$T(n) = \Theta(1 \cdot \log n) = \Theta(\log n)$**.
  
  ---
- ### 3. The $\epsilon$ Requirement
  On a quiz, you cannot just say "Case 1 because $n^2$ is bigger than $n$." You must show that they are **polynomially** different.
  
  *   **The Check:** Can you find a small number $\epsilon$ such that $n^{\log_b a - \epsilon}$ equals $f(n)$?
  *   **Example from Slide 21:** $T(n) = 9T(n/3) + n$
    *   $a = 9, b = 3, f(n) = n$.
    *   $\log_b a = \log_3 9 = 2$.
    *   Critical Value $= n^2$.
    *   Since $f(n) = n^1$, we can choose $\epsilon = 1$ (because $2 - 1 = 1$).
    *   The gap is polynomial. **Case 1 applies.** Result: $\Theta(n^2)$.
  
  ---
- ### Part 2 Practice: Case 1 vs. Case 2
  
  Determine which case applies and state the final complexity:
  
  1.  $T(n) = 4T(n/2) + n$
  2.  $T(n) = 4T(n/2) + n^2$
  3.  $T(n) = 3T(n/3) + n$
  4.  $T(n) = 16T(n/4) + n$
  
  *Think about the exponents!*