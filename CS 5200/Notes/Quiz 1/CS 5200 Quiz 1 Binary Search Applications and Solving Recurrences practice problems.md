### **Topic 3: Binary Search Variants & Solving Recurrences**

**Q1. Rotated Array Logic**
You are searching for the target **5** in the rotated sorted array: `A = [10, 15, 20, 0, 5]`.
Algorithm state: `low = 0`, `high = 4`, `mid = 2` (Value: 20).
Which logic branch executes next?
a) `A[low] <= A[mid]`, Left is sorted. Target **5** is inside range. Search Left.
b) `A[low] <= A[mid]`, Left is sorted. Target **5** is **not** inside range. Search Right.
c) `A[low] > A[mid]`, Right is sorted. Target **5** is inside range. Search Right.
d) `A[low] > A[mid]`, Right is sorted. Target **5** is **not** inside range. Search Left.

**Q2. Sparse Search Complexity**
In "Sparse Search" (searching a sorted array interspersed with empty strings), why is the **Worst Case** time complexity $O(n)$?
a) Because string comparisons take $O(n)$ time.
b) Because finding a non-empty pivot might require scanning to the edge of the array.
c) Because the recurrence relation is $T(n) = 2T(n/2) + n$.
d) It isn't $O(n)$; it is always $O(\log n)$.

**Q3. Finding "Serendipity" (Fixed Point)**
You are looking for an index $i$ such that $A[i] == i$ in a sorted array of distinct integers.
If `A[mid] > mid`, where does the fixed point lie?
a) It must be to the Right.
b) It must be to the Left.
c) It could be on either side.
d) It doesn't exist.

**Q4. Recursion Tree Height**
For the recurrence relation $T(n) = 3T(n/4) + n^2$, what is the **height** of the recursion tree?
a) $\log_3 n$
b) $\log_4 n$
c) $\log_{3/4} n$
d) $n \log n$

**Q5. Recursion Tree Work**
Consider $T(n) = 2T(n/2) + n^2$. What is the total work done at **Level 1** (the level below the root)?
*(Note: Root is Level 0)*
a) $n^2$
b) $n^2 / 2$
c) $n^2 / 4$
d) $2n$

**Q6. Master Theorem: Case identification**
Which case of the Master Theorem applies to $T(n) = 8T(n/2) + n^2$?
a) **Case 1** (Leaves dominate)
b) **Case 2** (Balanced/Tie)
c) **Case 3** (Root dominates)
d) Master Theorem does not apply.

**Q7. Master Theorem: The Critical Value**
To solve $T(n) = 9T(n/3) + n$, you must compare $f(n)$ to the critical value $n^{\log_b a}$. What is the value of the exponent $\log_b a$ here?
a) $\log_3 9 = 2$
b) $\log_9 3 = 0.5$
c) $\log_3 3 = 1$
d) $\log_2 9 \approx 3.17$

**Q8. The "Gap" Trap**
Why does the Master Theorem **fail** for the recurrence $T(n) = 2T(n/2) + n \log n$?
a) Because $n \log n$ is not a polynomial function.
b) Because $a$ must be greater than $b$.
c) Because $f(n)$ is larger than $n^{\log_b a}$, but not **polynomially** larger (the difference is only logarithmic).
d) It doesn't fail; this is standard Case 3.

**Q9. Regularity Condition**
For Master Theorem Case 3 (Root Heavy), you must verify the "Regularity Condition": $a f(n/b) \le c f(n)$.
For $T(n) = T(n/2) + n$, does this hold?
a) No, it fails for all $c$.
b) Yes, for $c = 1/2$.
c) Yes, for $c = 1$.
d) Yes, for $c = 2$.

**Q10. Strassen's Algorithm**
Strassen's Matrix Multiplication has the recurrence $T(n) = 7T(n/2) + n^2$. What is the runtime complexity?
a) $\Theta(n^2)$
b) $\Theta(n^2 \log n)$
c) $\Theta(n^{\log_2 7}) \approx \Theta(n^{2.81})$
d) $\Theta(n^3)$

---
- ### **Solutions & Explanations**
  
  **Q1: b) Left is sorted. Target not inside. Search Right.**
  *   **Analysis:**
    *   Check Sorted Side: `A[0]` (10) <= `A[2]` (20). **Left is sorted.**
    *   Check Range: Is target (5) between 10 and 20? **No.**
    *   Action: Therefore, search the **Right** half (the unsorted side).
  
  **Q2: b) Because finding a non-empty pivot might require scanning...**
  *   **Why:** If `A[mid]` is empty, you search left and right linearly (`mid--`, `mid++`) to find a string to compare against. In the worst case (e.g., array is all empty except ends), this is linear work.
  
  **Q3: b) It must be to the Left.**
  *   **Why:** If `A[mid] > mid` (e.g., Value 10 at Index 5), the value is "ahead" of the index. Since integers are distinct and sorted, they must grow by at least 1 at every step. Going right, the value will stay ahead of the index forever. The crossing point must be to the left.
  
  **Q4: b) $\log_4 n$**
  *   **Why:** The problem size is divided by $b=4$ at every step. The height of the tree is always $\log_b n$.
  
  **Q5: b) $n^2 / 2$**
  *   **Why:**
    *   Level 0: $n^2$.
    *   Level 1: 2 nodes. Each has size $n/2$.
    *   Work per node: $(n/2)^2 = n^2/4$.
    *   Total Level 1: $2 \times (n^2/4) = n^2/2$.
  
  **Q6: a) Case 1 (Leaves dominate)**
  *   **Why:**
    *   $a=8, b=2$. Critical exponent $\log_2 8 = 3$. Critical value $n^3$.
    *   $f(n) = n^2$.
    *   $n^3$ is polynomially larger than $n^2$. Leaves win.
  
  **Q7: a) $\log_3 9 = 2$**
  *   **Why:** $a=9, b=3$. Exponent is $\log_b a = \log_3 9 = 2$.
    *   This compares against $f(n) = n^1$. Since $n^2 > n^1$, Case 1 applies.
  
  **Q8: c) The "Gap" Trap**
  *   **Why:**
    *   Critical value: $n^{\log_2 2} = n^1$.
    *   $f(n) = n \log n$.
    *   $f(n)$ is bigger, but $n \log n / n = \log n$. This is not a polynomial difference ($n^\epsilon$). It falls into the "Gap."
  
  **Q9: b) Yes, for $c = 1/2$**
  *   **Why:**
    *   Condition: $1 \cdot (n/2) \le c \cdot n$.
    *   $n/2 \le cn$.
    *   Divide by $n$: $1/2 \le c$.
    *   Since we need $c < 1$, we can pick $c=1/2$. Regularity holds.
  
  **Q10: c) $\Theta(n^{\log_2 7}) \approx \Theta(n^{2.81})$**
  *   **Why:**
    *   $a=7, b=2$. Critical value $n^{\log_2 7}$.
    *   $f(n) = n^2$.
    *   $\log_2 7 \approx 2.81$.
    *   $n^{2.81} > n^2$. Leaves win (Case 1).