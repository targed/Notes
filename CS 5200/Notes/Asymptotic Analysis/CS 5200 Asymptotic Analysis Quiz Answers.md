### Simplifying Functions (The "Eye Test")
- #### Quiz 1: Exponents vs. Polynomials
  **Question:** Simplify $5 \cdot 2^N + 1000 \cdot N^{100}$.
  *   **Correct Answer:** $O(2^N)$
  *   **The "Why":**
    1.  **Rule of Dominance:** We must find the term that grows fastest.
    2.  This is a battle between an **Exponential** term ($2^N$) and a **Polynomial** term ($N^{100}$).
    3.  Exponential growth *always* beats polynomial growth eventually. Even though $N^{100}$ looks huge, if you plug in a large enough $N$, $2^N$ will dwarf it.
    4.  Therefore, we drop $1000N^{100}$ as "lower order" noise. We also drop the constant $5$.
- #### Quiz 2: Polynomial Simplification
  **Question:** What is the time complexity of $N^3/10 + 2N + 4$?
  *   **Correct Answer:** $O(N^3)$
  *   **The "Why":**
    1.  **Identify Highest Power:** The terms are $N^3$, $N^1$, and $N^0$ (constant). $N^3$ is the highest.
    2.  **Drop Lower Order Terms:** $2N$ and $4$ are insignificant compared to $N^3$ for large $N$.
    3.  **Drop Constants:** The coefficient $1/10$ is hardware dependent. We ignore it.
    4.  Result: $O(N^3)$.
  
  ---
- ### Code Analysis Patterns
- #### Quiz 3: The Square Root Loop
  **Question:** What is the complexity of this function?
  ```cpp
  for(int guess = 1; (guess * guess) <= n; guess++) ...
  ```
  *   **Correct Answer:** **$O(\sqrt{n})$**
  *   **The "Why":**
    1.  The loop condition is `guess * guess <= n`.
    2.  Mathematically, this means the loop runs as long as `guess` $\le \sqrt{n}$.
    3.  If $N = 100$, the loop runs 10 times. If $N = 10,000$, it runs 100 times.
    4.  The number of steps corresponds directly to the square root of the input.
- #### Quiz 4 (Slides 11–12): The Doubling Loop
  **Question:** What is the complexity of this function?
  ```cpp
  for (count = 1; count < N; count = count * 2) ...
  ```
  *   **Correct Answer:** $O(\log N)$
  *   **The "Why":**
    1.  The variable `count` does not increment by 1 (`+1`); it doubles (`*2`).
    2.  The sequence goes: 1, 2, 4, 8, 16, 32...
    3.  This asks the question: "How many times do I multiply 2 to get $N$?"
    4.  This is the definition of a logarithm ($2^x = N$).
  
  ---
- ### Theory & Definitions (True/False)
- #### Quiz 5: Upper Bounds
  **Question:** "n log n function is an upper bound for a function n²." (True/False)
  *   **Correct Answer:** **False**
  *   **The "Why":**
    1.  "Upper Bound" means the first function must be **bigger** (or equal) to the second function.
    2.  Is $n \log n \ge n^2$? **No.**
    3.  $n^2$ grows much faster. $n \log n$ is actually smaller than $n^2$, so it cannot act as a "ceiling" (upper bound) for $n^2$.
- #### Quiz 6: Constant in Exponent
  **Question:** Is $T(n) = 2^{n+10}$ equal to $O(2^n)$?
  *   **Correct Answer:** **True**
  *   **The "Why":**
    1.  Use algebra to split the exponent: $2^{n+10} = 2^{10} \times 2^n$.
    2.  $2^{10}$ is just a number (1024).
    3.  In Big O, we drop constant multipliers. $1024 \times 2^n \rightarrow O(2^n)$.
- #### Quiz 7: Variable in Exponent
  **Question:** Is $T(n) = 2^{2n}$ equal to $O(2^n)$?
  *   **Correct Answer:** **False**
  *   **The "Why":**
    1.  Use algebra: $2^{2n} = (2^2)^n = 4^n$.
    2.  Is $4^n = O(2^n)$? **No.**
    3.  $4^n$ grows exponentially faster than $2^n$. There is no constant you can multiply $2^n$ by to make it catch up to $4^n$.
- #### Quiz 8: Lower Bounds
  **Question:** "n log n function is a lower bound for a function n²." (True/False)
  *   **Correct Answer:** **True**
  *   **The "Why":**
    1.  "Lower Bound" means the first function is **smaller** (or equal) to the second.
    2.  Is $n \log n \le n^2$? **Yes.**
    3.  Since $n^2$ is larger, $n \log n$ acts as a "floor" (lower bound) beneath it.
  
  ---
- ### Multiple Choice & Bounds Checks
- #### Quiz 9: Multiple Bounds
  **Question:** Let $T(n) = \frac{1}{2}n^2 + 3n$. Which statements are true?
  *   a) $T(n) = O(n)$
  *   b) $T(n) = \Omega(n)$
  *   c) $T(n) = \Theta(n^2)$
  *   d) $T(n) = O(n^3)$
  
  **Correct Answers & Explanations:**
  *   **a) False.** ($n^2$ is too big to fit under $n$).
  *   **b) True.** ($\Omega$ is a lower bound. $n^2$ is "at least" $n$. It is a loose lower bound, but valid).
  *   **c) True.** ($\Theta$ is the tight bound. The dominant term is $n^2$).
  *   **d) True.** ($O$ is an upper bound. $n^3$ is "above" $n^2$, so it is a valid loose upper bound).
- #### Quiz 10: The Summation
  **Question:** An algorithm takes $n(n-1)/2$ steps. Choose the correct option(s).
  *   **Correct Answer:** **All three options are valid**, but **$\Theta(n^2)$** is the most precise.
  *   **The "Why":**
    1.  Expand the math: $\frac{n^2 - n}{2} = \frac{1}{2}n^2 - \frac{1}{2}n$.
    2.  Dominant term: $n^2$.
    3.  Therefore, it is $\Theta(n^2)$.
    4.  By definition, if it is $\Theta(n^2)$, it is automatically also $O(n^2)$ and $\Omega(n^2)$.
  
  ---
- ### "Crossover" Math Problems
- #### Quiz 11: Insertion vs. Merge Sort
  **Question:** Insertion sort is $8n^2$. Merge sort is $64n \log n$. For which values of $n$ is Insertion Sort faster?
  *   **Correct Answer:** **$n \le 43$**
  *   **The "Why":**
    1.  Set up the inequality: $8n^2 < 64n \log n$.
    2.  Simplify: Divide by $8n \rightarrow n < 8 \log n$.
    3.  Test values (Trial and Error):
        *   $n=32$: $32 < 8 \times 5$ (32 < 40) $\rightarrow$ True.
        *   $n=64$: $64 < 8 \times 6$ (64 < 48) $\rightarrow$ False.
    4.  The exact point where they swap is roughly 43.
- #### Quiz 12: Polynomial vs. Exponential
  **Question:** Smallest value of $n$ such that $100n^2$ runs faster than $2^n$?
  *   **Correct Answer:** **$n = 15$**
  *   **The "Why":**
    1.  We want the point where the Exponential ($2^n$) finally "overtakes" the Polynomial ($100n^2$).
    2.  Test $n=14$:
        *   $100(14^2) = 19,600$.
        *   $2^{14} = 16,384$.
        *   (Polynomial is still bigger/slower).
    3.  Test $n=15$:
        *   $100(15^2) = 22,500$.
        *   $2^{15} = 32,768$.
        *   (Exponential is now bigger/slower).
    4.  Wait, the phrasing is "100n^2 runs faster".
        *   Actually, usually "runs faster" means "smaller number".
        *   At $n=14$, Poly(19k) > Exp(16k). So Exp is faster.
        *   At $n=15$, Poly(22k) < Exp(32k). So Poly is faster.
    5.  Therefore, at $n=15$, the polynomial algorithm becomes the better choice.