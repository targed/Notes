### 1. Case 3: The Root Wins (Root-Heavy)
**Mathematical Definition:** $f(n) = \Omega(n^{\log_b a + \epsilon})$ for some constant $\epsilon > 0$.
**Translation:** $f(n)$ is **polynomially larger** than the critical value.

**The Logic:**
The work done at the very first step (the root) is so massive that all subsequent sub-problems and leaves combined are negligible.

**The "Catch": The Regularity Condition (Slide 18)**
For Case 3 to be valid, you must prove that the function "behaves" as it gets smaller. You must show:
$$a \cdot f(n/b) \le c \cdot f(n) \quad \text{for some constant } c < 1$$

**Example: (Slide 19)**
$$T(n) = 4T(n/2) + n^3$$
*   $a = 4, b = 2, f(n) = n^3$.
*   Critical Value: $n^{\log_2 4} = \mathbf{n^2}$.
*   **Comparison:** Is $n^3$ polynomially larger than $n^2$? **Yes** (Gap is $n^1$).
*   **Regularity Check:** 
  *   $4 \cdot (n/2)^3 \le c \cdot n^3$
  *   $4 \cdot (n^3 / 8) \le c \cdot n^3$
  *   $(1/2) \cdot n^3 \le c \cdot n^3$
  *   If we pick $c = 1/2$ (which is $< 1$), the condition holds.
*   **Result:** **$T(n) = \Theta(n^3)$**.

---
- ### 2. The "Gap" - When it Fails (Slide 24–25)
  A common quiz question is: **"Apply the Master Theorem to $T(n) = 2T(n/2) + n \lg n$."**
  Many students see $n \lg n$ is larger than $n$ and jump to Case 3. **This is wrong.**
  
  **The Deep Dive Explanation:**
  *   $n^{\log_2 2} = \mathbf{n}$.
  *   $f(n) = \mathbf{n \lg n}$.
  *   $n \lg n$ is indeed larger than $n$, but it is not **polynomially** larger. 
  *   To be polynomially larger, the gap must be at least $n^\epsilon$ (like $n^{1.0001}$). A $\lg n$ gap is too small.
  *   **Result:** The Master Theorem is **Not Applicable**. 
  *   *How to solve it?* Use a Recursion Tree. The actual result is $\Theta(n \lg^2 n)$.
  
  ---
- ### 3. Strassen vs. Caesar: An Application
  This is a classic "design" problem.
  *   **Strassen's Runtime:** $\Theta(n^{\log_2 7}) \approx \Theta(n^{2.81})$.
  *   **Caesar's Goal:** Divide into $n/4 \times n/4$ sub-problems ($b=4$) with combine cost $O(n^2)$. 
  *   **The Recurrence:** $T(n) = aT(n/4) + \Theta(n^2)$.
  *   **Question:** What is the largest integer $a$ to beat Strassen?
  
  **The Logic:**
  1.  To be faster than $n^{2.81}$, Caesar's critical value ($n^{\log_4 a}$) must have an exponent smaller than $2.81$.
  2.  $\log_4 a < 2.81$
  3.  $a < 4^{2.81}$
  4.  $4^{2.81} \approx 49.35$
  5.  **Answer:** The largest integer is **$a = 49$**.
  
  ---
- ### Final Practice Questions
  
  **Q1: The Regularity Failure**
  Given $T(n) = T(n/2) + n(2 - \cos n)$. 
  $f(n)$ is roughly linear, so it seems like Case 3. Why would the Regularity Condition likely fail here? 
  *(Hint: Think about the shape of a Cosine wave).*
  
  **Q2: The Logarithmic Gap**
  Which of the following can be solved by the Master Theorem?
  1.  $T(n) = 2T(n/2) + n^2$
  2.  $T(n) = 2T(n/2) + n / \lg n$
  3.  $T(n) = 2T(n/4) + \sqrt{n}$
  
  **Q3: The Branching Limit**
  If an algorithm reduces the problem size by $1/3$ ($b=3$) and the combination work is linear ($f(n)=n$), what is the maximum number of subproblems ($a$) allowed to keep the algorithm in the $O(n \log n)$ range?
  
  ---