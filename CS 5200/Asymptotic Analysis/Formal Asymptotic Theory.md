-
- ### A. Big O ($O$) - The Upper Bound
  *   **Concept:** "At most." The algorithm will **not** get worse than this rate.
  *   **Formal Definition:** $T(n) = O(g(n))$ if there exist constants $c$ and $n_0$ such that:
    $$ T(n) \le c \cdot g(n) \quad \text{for all } n \ge n_0 $$
  *   **Visual:** The function $g(n)$ (scaled by $c$) stays **above** your algorithm's time $T(n)$ forever after the point $n_0$.
  *   **Loose vs. Tight (Slide 26):**
    *   If $T(n) = 2n^2$, it is correct to say $O(n^2)$.
    *   It is *also* mathematically correct to say $O(n^3)$ or $O(2^n)$, because those are also "above" $2n^2$.
    *   However, we usually prefer the **Tightest Upper Bound** ($n^2$) unless stated otherwise.
- ### B. Big Omega ($\Omega$) - The Lower Bound
  *   **Concept:** "At least." The algorithm requires **minimum** this amount of work.
  *   **Formal Definition:** $T(n) = \Omega(g(n))$ if there exist constants $c$ and $n_0$ such that:
    $$ c \cdot g(n) \le T(n) \quad \text{for all } n \ge n_0 $$
  *   **Visual:** The function $g(n)$ stays **below** your algorithm's time.
  *   **Example:** $\sqrt{n} = \Omega(\log n)$.
    *   Why? Because Square Root grows faster than Log. The Log line will always be below the Root line eventually.
- ### C. Big Theta ($\Theta$) - The Tight Bound
  *   **Concept:** "Exactly." The algorithm grows at the same rate as the function.
  *   **Definition:** $T(n)$ is sandwiched between a lower bound and an upper bound of the same function type.
    $$ c_1 \cdot g(n) \le T(n) \le c_2 \cdot g(n) $$
  *   **Implication:** If an algorithm is $O(N)$ **AND** $\Omega(N)$, then it is $\Theta(N)$.
  
  ---
- ## 2. The Exponent Traps
  These slides present two "True/False" questions involving exponents. These are classic exam questions designed to trick you.
- ### Trap 1: Constants in Exponents
  **Question:** Is $2^{n+10} = O(2^n)$?
  **Answer:** **True.**
  *   **Proof:** Use algebra to split the exponent.
    $$ 2^{n+10} = 2^{10} \times 2^n $$
    $$ 2^{10} = 1024 \text{ (This is just a constant number!)} $$
  *   So, is $1024 \cdot 2^n = O(2^n)$? Yes. In Big O, we drop constants. $1024$ is a valid $c$.
- ### Trap 2: Variables in Exponents
  **Question:** Is $2^{2n} = O(2^n)$?
  **Answer:** **False.**
  *   **Proof:** Simplify the expression.
    $$ 2^{2n} = (2^n)^2 \quad \text{OR} \quad 4^n $$
  *   Is $4^n = O(2^n)$? **No.**
  *   $4^n$ grows much, much faster than $2^n$. You cannot multiply $2^n$ by any constant $c$ to catch up to $4^n$ as $n \to \infty$.
  
  ---
- ## 3. How to Prove Big Theta
  The slides show a proof for: $\frac{1}{2}n^2 - 3n = \Theta(n^2)$.
  This looks intimidating, but it is a game of picking numbers for $c_1$, $c_2$, and $n_0$.
  
  **The Goal:**
  Find constants to make this sandwich true:
  $$ c_1 n^2 \le \frac{1}{2}n^2 - 3n \le c_2 n^2 $$
  
  **Step A: Upper Bound ($O$) - Finding $c_2$**
  *   We need $\frac{1}{2}n^2 - 3n \le c_2 n^2$.
  *   Since $-3n$ makes the left side *smaller*, we can simply remove it to make the right side bigger.
  *   $\frac{1}{2}n^2 - 3n \le \frac{1}{2}n^2$.
  *   So, choose $c_2 = \frac{1}{2}$ (or anything bigger, like 1).
  
  **Step B: Lower Bound ($\Omega$) - Finding $c_1$ (The Hard Part)**
  *   We need $c_1 n^2 \le \frac{1}{2}n^2 - 3n$.
  *   We need to pick a $c_1$ small enough, and an $n_0$ big enough so the $-3n$ doesn't ruin it.
  *   Slide 41 suggests dividing by $n^2$:
    $$ c_1 \le \frac{1}{2} - \frac{3}{n} $$
  *   If we pick $n = 7$:
    $$ \frac{1}{2} - \frac{3}{7} = \frac{7}{14} - \frac{6}{14} = \frac{1}{14} $$
  *   So if $c_1 = \frac{1}{14}$ and $n_0 = 7$, the math holds.
  
  **Conclusion:** Since we found valid constants for upper and lower bounds, the function is $\Theta(n^2)$.
  
  ---
- #### **1. The Limit Test (The "Professor's Secret Weapon")**
  *   **The Nuance:** If you get a really weird function on a quiz (e.g., $n^{0.1}$ vs. $\log^2 n$), trying to find $c$ and $n_0$ algebraically is a nightmare.
  *   **The Addition:** Use the Limit Test to determine the relationship between $f(n)$ and $g(n)$:
    $$\lim_{n \to \infty} \frac{f(n)}{g(n)} = L$$
    1.  If **$L = 0$**: $f(n)$ grows slower than $g(n)$. ($f$ is $O(g)$).
    2.  If **$L = \infty$**: $f(n)$ grows faster than $g(n)$. ($f$ is $\Omega(g)$).
    3.  If **$L = \text{constant } (>0)$**: They grow at the same rate. ($f$ is $\Theta(g)$).
  *   *Note:* You may need **L'Hôpital's Rule** (differentiating top and bottom) to solve these limits.
- #### **2. Properties of Asymptotic Notation**
  *   **Transitivity:** If $f = O(g)$ and $g = O(h)$, then $f = O(h)$.
  *   **Reflexivity:** $f = O(f)$, $f = \Omega(f)$, $f = \Theta(f)$.
  *   **Symmetry:** $f = \Theta(g)$ if and only if $g = \Theta(f)$.
  *   **Transpose Symmetry:** $f = O(g)$ if and only if $g = \Omega(f)$. (If I am shorter than you, you are taller than me).
- # Check for Understanding
  
  **1. True or False?**
  $N^2 = O(N^3)$
  
  **2. True or False?**
  $N^3 = O(N^2)$
  
  **3. Pop Quiz from Slide 44:**
  Let $T(n) = \frac{1}{2}n^2 + 3n$. Which of the following are true? (Select all that apply)
  a) $T(n) = O(n)$
  b) $T(n) = \Omega(n)$
  c) $T(n) = \Theta(n^2)$
  d) $T(n) = O(n^3)$