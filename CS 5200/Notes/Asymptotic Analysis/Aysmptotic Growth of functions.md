### Module 1: The "Sandwich" ($\Theta$-Notation)

The text starts with $\Theta$-notation because it is the most precise. While the world says "Big O" for everything, computer scientists use $\Theta$ when they want to describe the **exact** rate of growth.
- #### 1. The Formal Definition
  $f(n) = \Theta(g(n))$ if there exist positive constants $c_1, c_2,$ and $n_0$ such that:
  $$0 \le c_1g(n) \le f(n) \le c_2g(n) \text{ for all } n \ge n_0$$
- #### 2. The Deep Dive: What does this actually mean?
  *   **The "Sandwich":** This tells us that $f(n)$ is "trapped" between two versions of $g(n)$. One version ($c_1$) pushes from below, and the other ($c_2$) pushes from above. 
  *   **The "Threshold" ($n_0$):** We don't care what happens when $n$ is small (1, 5, or 10). We only care about the "limit" as $n$ goes to infinity. $n_0$ is the point on the graph where the "sandwich" starts working and never breaks again.
  *   **Asymptotic Tight Bound:** If a function is $\Theta(n^2)$, it grows exactly like a parabola. It isn't faster, and it isn't slower.
- #### 3. Proving it (The Pen-and-Paper Skill)
  On a quiz, you might be asked: **"Prove that $\frac{1}{2}n^2 - 3n = \Theta(n^2)$."**
  To prove this, you must find values for $c_1, c_2,$ and $n_0$ that make the inequality true.
  
  *   **Step 1: Upper Bound ($c_2$).** We need $\frac{1}{2}n^2 - 3n \le c_2n^2$. Since $-3n$ is making the left side smaller, we can just ignore it. $\frac{1}{2}n^2 \le \frac{1}{2}n^2$. So, **$c_2 = 1/2$** works for all $n \ge 1$.
  *   **Step 2: Lower Bound ($c_1$).** We need $c_1n^2 \le \frac{1}{2}n^2 - 3n$. This is harder because $-3n$ "hurts" our lower bound. If we pick $n=7$, then $\frac{1}{2}(49) - 3(7) = 24.5 - 21 = 3.5$. We need $c_1(49) \le 3.5$. If we pick **$c_1 = 1/14$**, the math holds.
  *   **Step 3: State the threshold.** For these constants, the proof works for **$n_0 = 7$**.
  
  ---
- ### Module 2: The Ceiling ($O$) and The Floor ($\Omega$)
- #### 1. Big-O ($O$-Notation) - Asymptotic Upper Bound
  This is the **ceiling**. It says the function will *never grow faster* than this, but it could be much slower.
  *   **Deep Dive Note:** Mathematically, if an algorithm is $\Theta(n)$, it is also $O(n^2)$, $O(n^3)$, and $O(2^n)$. Why? Because an upper bound is just a "guarantee." If I say "I have fewer than 100 dollars," and I actually have 5 dollars, I told the truth. 
  *   **In Class:** Professors usually want the *tightest* upper bound, but strictly speaking, $O$ is just a cap.
- #### 2. Big-Omega ($\Omega$-Notation) - Asymptotic Lower Bound
  This is the **floor**. It says the function will *never grow slower* than this.
  *   **Example:** $\Omega(n \log n)$ means the algorithm is at least that slow. It provides a "best-case" guarantee or a lower bound on the complexity of a problem itself (like how comparison sorting is $\Omega(n \log n)$).
- #### 3. The Grand Theorem (Theorem 3.1)
  This is a high-probability quiz question:
  > **Theorem:** For any two functions $f(n)$ and $g(n)$, $f(n) = \Theta(g(n))$ **if and only if** $f(n) = O(g(n))$ and $f(n) = \Omega(g(n))$.
  
  **Translation:** If you can prove that the ceiling is $n^2$ and the floor is $n^2$, then the function is "tightly" $n^2$.
  
  ---
- ### Module 3: Higher-Level Nuances (The "Professor Deep Dive")
- #### 1. Asymptotic Notation in Equations
  When you see $T(n) = 2T(n/2) + \Theta(n)$, what does that $\Theta(n)$ mean?
  It represents an **anonymous function**. It means "there is some function $f(n)$ in the set $\Theta(n)$ that lives here." We don't care about its name or its exact coefficients; we only care about its growth rate.
- #### 2. Comparison of Growth Rates (The Hierarchy)
  You must know the "ranking" of functions. If you are comparing two functions on a quiz, the faster-growing one is the "worse" algorithm.
  1.  **Constant:** $O(1)$
  2.  **Logarithmic:** $O(\log n)$
  3.  **Linear:** $O(n)$
  4.  **Linearithmic:** $O(n \log n)$
  5.  **Polynomial:** $O(n^2), O(n^3)$
  6.  **Exponential:** $O(2^n)$
  7.  **Factorial:** $O(n!)$
  
  **Deep Dive Tip:** To compare any two weird functions (e.g., $n^{0.5}$ vs $\log^2 n$), use the **Limit Test**:
  $$\lim_{n \to \infty} \frac{f(n)}{g(n)}$$
  *   If the result is **$0$**, $g(n)$ grows faster ($f$ is $O(g)$).
  *   If the result is **$\infty$**, $f(n)$ grows faster ($f$ is $\Omega(g)$).
  *   If the result is **a constant ($L > 0$)**, they grow at the same rate ($f$ is $\Theta(g)$).
  
  ---
- ### Practice Questions (Round 1: Conceptual & Formal)
  
  
  1.  **Formal Proof:** Using the formal definition of $O$-notation ($c$ and $n_0$), prove that $7n^2 + 100n = O(n^2)$. (Show your choice of $c$ and $n_0$).
  2.  **True/False & Why:** Is $2^{n+1} = O(2^n)$? Is $2^{2n} = O(2^n)$? (Be careful here, this is a classic "trap" question).
  3.  **Theorem 3.1:** If an algorithm has a Best Case of $\Omega(n)$ and a Worst Case of $O(n^2)$, can we say the algorithm is $\Theta(n \log n)$? Why or why not?
  4.  **Set Theory:** Why do we write $f(n) = O(g(n))$ instead of $f(n) \in O(g(n))$? What does the textbook say about this "abuse of notation"?