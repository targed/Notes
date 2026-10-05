### **Topic 1: Asymptotic Analysis (Time & Space)**
*(Relevant Slides: Introduction, Asymptotic Analysis, Space Complexity)*

**Q1. The Dependent Loops**
What is the tightest asymptotic time complexity ($\Theta$) of the following code?
```cpp
void analyzeMe(int n) {
  int count = 0;
  for (int i = 0; i < n; i++) {
      for (int j = i; j < n; j++) {
          count++;
      }
  }
}
```
a) $\Theta(n)$
b) $\Theta(n \log n)$
c) $\Theta(n^2)$
d) $\Theta(n^3)$

**Q2. The Multiplicative Loop**
What is the time complexity of this function?
```cpp
void loops(int n) {
  for (int i = 1; i < n; i = i * 2) {
      // Constant time work
      print(i);
  }
}
```
a) $\Theta(n)$
b) $\Theta(\sqrt{n})$
c) $\Theta(\log n)$
d) $\Theta(n^2)$

**Q3. Recursive Space vs. Time**
Consider the recursive Fibonacci function.
```cpp
int fib(int n) {
  if (n <= 1) return n;
  return fib(n-1) + fib(n-2);
}
```
Which of the following correctly describes its complexity?
a) Time: $O(n)$, Space: $O(1)$
b) Time: $O(2^n)$, Space: $O(2^n)$
c) Time: $O(2^n)$, Space: $O(n)$
d) Time: $O(n^2)$, Space: $O(n)$

**Q4. Multiple Variables**
You are given two arrays, `arrA` of size $A$ and `arrB` of size $B$. What is the runtime of this code?
```cpp
for (int i = 0; i < A; i++) {
  print(arrA[i]);
}
for (int j = 0; j < B; j++) {
  for (int k = 0; k < 1000; k++) {
      print(arrB[j]);
  }
}
```
a) $O(A + B)$
b) $O(A \cdot B)$
c) $O(A + B^2)$
d) $O(N^2)$

**Q5. The "Square Root" Loop**
What is the time complexity of the following?
```cpp
bool isPrime(int n) {
  for (int i = 2; i * i <= n; i++) {
      if (n % i == 0) return false;
  }
  return true;
}
```
a) $O(n)$
b) $O(\log n)$
c) $O(n^2)$
d) $O(\sqrt{n})$

**Q6. Formal Definition Check**
If $f(n) = 3 \cdot 2^n$ and $g(n) = 2^{2n}$, which statement is **TRUE**?
a) $f(n) = O(g(n))$
b) $f(n) = \Omega(g(n))$
c) $f(n) = \Theta(g(n))$
d) $f(n)$ and $g(n)$ are incomparable.

**Q7. Space Complexity (Auxiliary)**
What is the **Auxiliary Space Complexity** of this standard Merge Sort implementation on an array of size $N$?
*(Assume standard recursion and a merge step that creates a temporary array)*
a) $O(1)$
b) $O(\log N)$
c) $O(N)$
d) $O(N \log N)$

**Q8. The "Trick" Exponent**
Which of the following statements about exponents is **FALSE**?
a) $3^{n+2} = \Theta(3^n)$
b) $2^{n} = O(3^n)$
c) $2^{2n} = O(2^n)$
d) $100 \cdot 5^n = \Theta(5^n)$

**Q9. Recursive Logic**
What is the time complexity of this recursive function `fun(n)`?
```cpp
void fun(int n) {
  if (n <= 1) return;
  print(n);
  fun(n - 1);
}
```
a) $O(1)$
b) $O(\log n)$
c) $O(n)$
d) $O(n^2)$

**Q10. Asymptotic Dominance**
Arrange the following functions in increasing order of growth rate (slowest to fastest):
1. $n \log n$
2. $n^2$
3. $\sqrt{n}$
4. $2^n$
5. $n!$

a) $3, 1, 2, 4, 5$
b) $1, 3, 2, 4, 5$
c) $3, 2, 1, 5, 4$
d) $1, 2, 3, 5, 4$

---
- ### **Solutions & Explanations**
  
  **Q1: c) $\Theta(n^2)$**
  *   **Why:** Even though the inner loop depends on $i$ (it runs $n, n-1, \dots, 1$ times), the summation is $n + (n-1) + \dots + 1 = \frac{n(n+1)}{2}$. The dominant term is $n^2/2$, which simplifies to $\Theta(n^2)$.
  
  **Q2: c) $\Theta(\log n)$**
  *   **Why:** The loop variable `i` doubles every iteration ($1, 2, 4, 8 \dots$). The number of steps is the number of times you can multiply 2 to reach $n$, which is $\log_2 n$.
  
  **Q3: c) Time: $O(2^n)$, Space: $O(n)$**
  *   **Why:**
    *   **Time:** Each call branches into 2. The tree height is $n$. Total calls $\approx 2^n$.
    *   **Space:** Space is determined by the **maximum depth** of the recursion stack. Even though there are $2^n$ calls total, the stack only goes $n$ deep (going down the left branch) before popping.
  
  **Q4: a) $O(A + B)$**
  *   **Why:** The loops are **sequential** (not nested).
    *   Loop 1 takes $O(A)$.
    *   Loop 2 takes $O(B \times 1000)$. Since 1000 is a constant, we drop it. $O(B)$.
    *   Total: $O(A + B)$. (Note: We do not say $O(N)$ because $A$ and $B$ are different variables).
  
  **Q5: d) $O(\sqrt{n})$**
  *   **Why:** The loop runs as long as `i * i <= n`. Solving for `i`, we get $i \le \sqrt{n}$.
  
  **Q6: a) $f(n) = O(g(n))$**
  *   **Why:**
    *   $f(n) = 3 \cdot 2^n = \Theta(2^n)$.
    *   $g(n) = 2^{2n} = (2^2)^n = 4^n$.
    *   $2^n$ grows much slower than $4^n$. Therefore, $f(n)$ is upper bounded by $g(n)$.
  
  **Q7: c) $O(N)$**
  *   **Why:**
    *   Merge Sort requires a temporary array of size $N$ during the `merge` step.
    *   Although the stack depth is $O(\log N)$, the auxiliary array $O(N)$ dominates.
    *   Total Space = Input($N$) + Aux($N$) = $O(N)$. (We usually cite Aux space as $O(N)$).
  
  **Q8: c) $2^{2n} = O(2^n)$ is FALSE**
  *   **Why:**
    *   (a) $3^{n+2} = 3^2 \cdot 3^n = 9 \cdot 3^n$. Constant drop $\to$ True.
    *   (b) $2^n$ is smaller than $3^n$ $\to$ True.
    *   (c) $2^{2n} = 4^n$. $4^n$ is NOT $O(2^n)$. It grows infinitely faster. **False.**
  
  **Q9: c) $O(n)$**
  *   **Why:** The function prints a number and decrements $n$ by 1. It does this until $n=1$. This is a simple countdown: $n, n-1, n-2 \dots 1$. Total steps = $n$.
  
  **Q10: a) $3, 1, 2, 4, 5$**
  *   **Order:**
    3.  $\sqrt{n}$ (Root - fastest/smallest)
    1.  $n \log n$ (Linearithmic)
    2.  $n^2$ (Polynomial)
    4.  $2^n$ (Exponential)
    5.  $n!$ (Factorial - slowest/largest)