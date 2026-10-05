### 1. The Standard Form
To use the Master Theorem, your recurrence must look exactly like this:
$$T(n) = aT(n/b) + f(n)$$

**The Parameters (The Rules of the Game):**
*   **$a \ge 1$:** The number of subproblems. (You can't have zero subproblems).
*   **$b > 1$:** The factor by which the problem size is reduced. (You must actually be making the problem smaller).
*   **$f(n)$:** The cost of work done outside the recursion (dividing the problem and combining the results).

---
- ### 2. The "Critical Value": $n^{\log_b a}$
  The secret to the Master Theorem is comparing $f(n)$ to a "critical value" derived from the branching factor.
  *   **Formula:** Compute $n^{\log_b a}$.
  *   **What it represents:** In a recursion tree, this value represents the **total number of leaves**.
- ### 3. The Three-Way Race (The Intuition)
  Think of the total runtime as a race between the **Root** (the work $f(n)$ done at the top level) and the **Leaves** (the total number of base cases $n^{\log_b a}$).
- #### **Case 1: The Leaves Win (Leaf-Heavy)**
  *   **Condition:** $n^{\log_b a}$ is "polynomially larger" than $f(n)$.
  *   **Intuition:** Most of the work happens at the bottom of the tree. The overhead at the top ($f(n)$) is negligible.
  *   **Solution:** $T(n) = \Theta(n^{\log_b a})$.
- #### **Case 2: It’s a Tie (Balanced)**
  *   **Condition:** $f(n)$ and $n^{\log_b a}$ are the same size.
  *   **Intuition:** Every level of the recursion tree does roughly the same amount of work (like Merge Sort).
  *   **Solution:** $T(n) = \Theta(n^{\log_b a} \lg n)$.
    *   *Note:* You take the work of one level and multiply it by the height of the tree ($\lg n$).
- #### **Case 3: The Root Wins (Root-Heavy)**
  *   **Condition:** $f(n)$ is "polynomially larger" than $n^{\log_b a}$.
  *   **Intuition:** The work at the very first step is so massive that the subsequent recursive calls are just "noise" by comparison.
  *   **Solution:** $T(n) = \Theta(f(n))$.
  
  ---
- ### 4. The "Polynomial Gap" (The Professor Deep Dive)
  Your professor and the supplementary PDF emphasize a critical technicality: the difference between the functions must be **polynomial**, not just any difference.
  
  *   **Slide 25/PDF Page 9:** If $f(n)$ is larger than $n^{\log_b a}$ by only a logarithmic factor (e.g., $f(n) = n \lg n$ vs. $n^{\log_2 2} = n$), the Master Theorem **fails**.
  *   **Why?** There isn't enough of a "gap" for one side to completely dominate the other. This is a common trap question on written quizzes.
  
  ---
- ### Part 1 Practice: Parameter Extraction
  
  Before we dive into the specific math of Case 1 and 2 in the next response, let's practice identifying the parameters and the "Critical Value."
  
  **For each recurrence, find $a$, $b$, and the critical value $n^{\log_b a}$:**
  
  1.  **Binary Search:** $T(n) = T(n/2) + 1$
  2.  **Merge Sort:** $T(n) = 2T(n/2) + n$
  3.  **Naive Matrix Mult:** $T(n) = 8T(n/2) + n^2$
  4.  **Strassen’s Mult:** $T(n) = 7T(n/2) + n^2$
  
  *Hint: If a is 1, $\log_b 1$ is always 0.*