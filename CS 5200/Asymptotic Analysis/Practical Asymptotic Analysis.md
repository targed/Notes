## 1. The Rules of Simplification
When analyzing a function like $T(N) = 5N^2 + 100N + 3$, we want to distill it down to its "Order of Growth."
- ### Rule A: Drop the Constants
  We ignore coefficients because they are hardware/implementation dependent.
  *   $5N \rightarrow O(N)$
  *   $100N \rightarrow O(N)$
  *   $\frac{1}{2}N^2 \rightarrow O(N^2)$
- ### Rule B: Drop Lower Order Terms (Dominance)
  As $N$ approaches infinity, the term with the highest growth rate completely dominates the result. The other terms become mathematical noise.
  *   $N^2 + N \rightarrow O(N^2)$
  *   $N^3 + 1000N^2 \rightarrow O(N^3)$
  
  **The Tricky Case:**
  $$ 5 \cdot 2^N + 1000 \cdot N^{100} $$
  *   Here, you have an Exponential term ($2^N$) and a Polynomial term ($N^{100}$).
  *   **Concept:** Exponential growth *always* beats Polynomial growth eventually. Even though $N^{100}$ looks massive, $2^N$ will eventually crush it.
  *   **Result:** $O(2^N)$.
- ### Rule C: Add vs. Multiply
  *   **Sequential Loops (Add):** If Loop A runs, finishes, and *then* Loop B runs, you add their complexities.
    *   $O(A) + O(B) = O(A + B) \rightarrow \text{max}(O(A), O(B))$.
  *   **Nested Loops (Multiply):** If Loop B runs *inside* Loop A, you multiply them.
    *   $O(A) \times O(B) = O(A \cdot B)$.
  
  ---
- ## 2. Recognizing Code Patterns
  The slides introduce two very important loop patterns that are **not** $O(N)$ or $O(N^2)$.
- ### Pattern A: The Square Root Loop
  **Code:**
  ```cpp
  for(int guess = 1; (guess * guess) <= n; guess++) { ... }
  ```
  **Analysis:**
  *   The loop continues as long as $guess^2 \le n$.
  *   To find the stopping point, solve for `guess`:
    $$ guess \le \sqrt{n} $$
  *   **Complexity:** $O(\sqrt{N})$.
  *   *Note:* This is common in primality testing (checking if a number is prime).
- ### Pattern B: The Logarithmic Loop
  **Code:**
  ```cpp
  for (count = 1; count < N; count = count * 2) { ... }
  ```
  **Analysis:**
  *   The variable `count` doubles every step: 1, 2, 4, 8, 16...
  *   The question is: "How many times can I multiply 1 by 2 to reach N?"
    $$ 2^x = N $$
  *   Solving for $x$: $x = \log_2 N$.
  *   **Complexity:** $O(\log N)$.
  *   *Note:* Any loop that cuts the problem in half ($/2$) or doubles the counter ($*2$) is Logarithmic.
  
  ---
- ## 3. Hierarchy of Growth Rates
  You must memorize the ranking of these complexities. From fastest (best) to slowest (worst):
  
  1.  $O(1)$ - Constant (Instant access, hash maps)
  2.  $O(\log N)$ - Logarithmic (Binary search)
  3.  $O(\sqrt{N})$ - Root (Primality tests)
  4.  $O(N)$ - Linear (Iterating an array)
  5.  $O(N \log N)$ - Linearithmic (Merge Sort, Heap Sort, efficient Quicksort)
  6.  $O(N^2)$ - Quadratic (Bubble/Insertion Sort, nested loops)
  7.  $O(N^3)$ - Cubic (Matrix multiplication, triple nested loops)
  8.  $O(2^N)$ - Exponential (Recursive Fibonacci, brute-forcing passwords)
  9.  $O(N!)$ - Factorial (Traveling Salesperson brute force)
  
  **Key Takeaway:**
  The slide compares $5N \log N$ vs. $N^2$.
  *   For small inputs (like $N=8$), the $N^2$ algorithm might actually be faster because of the constant $5$.
  *   However, as $N$ gets large, $N^2$ shoots up vertically. $N \log N$ is significantly better for massive data.
  
  ---
- #### **1. The Two-Variable Rule (The "N" Trap)**
  *   **The Nuance:** In class, we often assume every input is $N$. In the real world (and on tricky exams), algorithms often have multiple independent inputs.
  *   **The Addition:** If an algorithm iterates through array $A$ (size $a$) and then array $B$ (size $b$), the complexity is **$O(a + b)$**, not $O(N)$. If they are nested, it is **$O(a \cdot b)$**.
  *   **Example for Notes:** 
    ```cpp
    for (int x : arrayA) { ... } // O(a)
    for (int y : arrayB) { ... } // O(b)
    // Result: O(a + b). You cannot simplify this to O(N) 
    // unless you are told a == b.
    ```
- #### **2. Hidden Complexity: String Operations**
  *   **The Nuance:** Many students treat string concatenation or comparison as $O(1)$. It is almost never $O(1)$.
  *   **The Addition:** 
    *   **Comparison:** Comparing two strings of length $S$ takes **$O(S)$** time because you must compare each character until you find a difference.
    *   **Concatenation:** In many languages (like Java or Python), strings are immutable. Doing `string A + string B` takes **$O(A + B)$** time because a new string must be built and every character copied.
  *   **Example for Notes:** Sorting an array of $N$ strings where each string has length $S$.
    *   Total time: $O(S \cdot N \log N)$. (The $N \log N$ comes from the sort, the $S$ comes from the cost of each comparison).
- #### **3. Recursive Space vs. Iterative Space**
  *   **The Nuance:** A "deep dive" professor will ask you to compare the memory usage of two functions that do the same thing.
  *   **The Addition:** 
    *   **Iterative:** A loop that runs $N$ times but only uses a few variables is **$O(1)$ space**.
    *   **Recursive:** A function that calls itself $N$ times (like a recursive Factorial or Sum) is **$O(N)$ space** because each call sits on the stack simultaneously.
    *   **The "Pop" Exception:** If you call a helper function inside a loop, like `for (i=0 to n) { helper(); }`, the space is **$O(1)$** (or whatever the helper uses), because the helper's stack frame is destroyed/popped before the next iteration starts.
- #### **4. Logarithmic Base insignificance**
  *   **The Nuance:** Why don't we write $O(\log_2 N)$ or $O(\log_{10} N)$?
  *   **The Addition:** Because of the **Change of Base Formula**: $\log_a N = \frac{\log_b N}{\log_b a}$.
    *   Since $\frac{1}{\log_b a}$ is just a **constant number**, and Big O drops constants, all logarithmic bases are equivalent in asymptotic terms. 
    *   *Exception:* This does **not** apply to exponents. $O(2^N)$ is fundamentally different from $O(8^N)$.
- # Check for Understanding
  
  **Simplify the following to their Big O notation:**
  
  1.  $T(N) = N \log N + N + 5000$
  2.  $T(N) = 2^{N} + N!$  *(Hint: Refer to the hierarchy list)*
  3.  $T(N) = N^3 + 100N^4$
  
  **Code Analysis Question:**
  What is the Big O of this loop structure?
  ```cpp
  for (int i = 0; i < N; i++) {       // Runs N times
    for (int j = 1; j < N; j = j * 2) { // Doubles j each time
        print(i, j);
    }
  }
  ```