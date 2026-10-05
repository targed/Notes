#### Quiz 2.1: Searching One Array
**The Code:**
```text
for i := 1 to n do
 if A[i] = t then return TRUE
```
*   **The Question:** What is the asymptotic running time?
*   **Correct Answer:** **c) $O(n)$**
*   **The "Why":**
  This is a standard iteration. The loop runs $N$ times. The instructions inside the loop (checking if `A[i] == t`) take constant time, regardless of how big the array is.
  *   Total operations $\approx 1 \times N$.
  *   In Big O, we ignore coefficients of 1.
  *   Therefore: $O(n)$.
- #### Quiz 2.2: Searching Two Arrays
  **The Code:**
  ```text
  for i := 1 to n do        // Loop A
   if A[i] = t ...
  for i := 1 to n do        // Loop B
   if B[i] = t ...
  ```
  *   **The Question:** What is the running time of this longer piece of code?
  *   **Correct Answer:** c) $O(n)$
  *   **The "Why":**
    Many students guess $O(n^2)$ here because they see two loops. However, the loops are **sequential** (side-by-side), not nested (one inside the other).
    *   Loop A finishes completely ($N$ steps).
    *   *Then* Loop B starts ($N$ steps).
    *   Total operations $= N + N = 2N$.
    *   In Big O, we drop constants (the 2).
    *   Therefore: $O(n)$.
- #### Quiz 2.3: Common Element (Nested Loops)
  **The Code:**
  ```text
  for i := 1 to n do            // Outer Loop
   for j := 1 to n do         // Inner Loop
      if A[i] = B[j] ...
  ```
  *   **The Question:** What is the running time?
  *   **Correct Answer:** **d) $O(n^2)$**
  *   **The "Why":**
    The loops are **nested**. For every single iteration of the outer loop $i$, the inner loop $j$ runs fully from 1 to $N$.
    *   Total operations $= N \text{ (outer)} \times N \text{ (inner)} = N^2$.
    *   Therefore: $O(n^2)$.
- #### Quiz 2.4: Checking Duplicates (Dependent Loops)
  **The Code:**
  ```text
  for i := 1 to n do
   for j := i + 1 to n do     // Note the start index!
      if A[i] = A[j] ...
  ```
  *   **The Question:** What is the running time?
  *   **Correct Answer:** **d) $O(n^2)$**
  *   **The "Why":**
    This is the "triangle" sum.
    *   When $i=1$, inner loop runs $N-1$ times.
    *   When $i=N$, inner loop runs 0 times.
    *   Total steps $= (N-1) + (N-2) + \dots + 1$.
    *   Formula for sum of integers: $\frac{N(N-1)}{2} = \frac{N^2 - N}{2}$.
    *   Big O Rule: We only care about the highest power ($N^2$) and we ignore constants ($1/2$).
    *   Therefore: $O(n^2)$.
  
  ---
- ### Concepts Check
- #### 1. The "Small N vs. Large N" Check
  **The Scenario:** Algorithm A is $100N$. Algorithm B is $2N^2$.
  *   **Question:** Which is faster for small $N$ ($5$)? Which is faster for large $N$ ($1000$)?
  *   **Answer:**
    *   **Small Input ($N=5$):** Algorithm B is faster.
        *   $100(5) = 500$ steps.
        *   $2(5^2) = 50$ steps.
    *   **Large Input ($N=1000$):** Algorithm A is faster.
        *   $100(1000) = 100,000$ steps.
        *   $2(1000^2) = 2,000,000$ steps.
  *   **The Lesson:** Big O measures **scalability**, not raw speed. An $O(N^2)$ algorithm might actually be faster for tiny inputs, but it will eventually crash your system as inputs get massive.
- #### 2. The "Fixed Loop" Check
  **The Scenario:**
  ```text
  for i := 1 to N do
    for j := 1 to 1000 do
        print(i, j)
  ```
  *   **Question:** Is this $O(N)$ or $O(N^2)$?
  *   **Answer:** **$O(N)$**
  *   **The "Why":**
    The inner loop runs exactly 1000 times, regardless of how large $N$ is. In math terms, 1000 is a **constant coefficient**, not a variable.
    *   Total operations $= 1000 \times N$.
    *   Drop the constant ($1000$).
    *   Result: $O(N)$.
    *   *Note:* If the inner loop went to $N$, it would be $O(N^2)$. If it goes to a fixed number, it is $O(N)$.
- #### 3. The "GPS Sales" Check
  **The Scenario:** A GPS algorithm finds a path instantly on a simple map (Case A) but takes forever on a complex grid (Case B).
  *   **Question 1:** Which is Best/Worst case?
    *   **Answer:** Case A is Best Case. Case B is Worst Case.
  *   **Question 2:** Which do you advertise to an ambulance service?
    *   **Answer:** **Worst Case.**
  *   **The "Why":**
    In critical systems (life or death, or real-time systems), you need a **guarantee**. You cannot tell an ambulance driver, "Ideally, the route calculates in 1 second, but sometimes it takes 10 minutes." They need to know the *maximum* time it will take to ensure they can do their job safely.