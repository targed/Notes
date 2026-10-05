### 1. The Mathematical Problem (Slides 15–18)
The problem asks: **"How many combinations of size $r$ can be made out of a group of size $n$?"**
*   *Example:* From 20 students, form a panel of 5. How many unique panels are possible?

In probability and statistics, this is known as "$n$ choose $r$" (often written as $\binom{n}{r}$ or $nCr$).
*   **The variables in the slides:**
  *   `group` = $n$ (The total pool to pick from).
  *   `members` = $r$ (The size of the subgroup you want).
- ### 2. The Recursive Insight (Slides 19–20)
  Instead of using the factorial formula ($\frac{n!}{r!(n-r)!}$), we can define this recursively by breaking it into two smaller subproblems. 
  
  Imagine you are choosing $r$ members from a group of $n$ people. Let's focus on one specific person, **Alice**. Every possible combination either **includes Alice** or **excludes Alice**.
  
  1.  **Case 1 (Alice is IN):** Since Alice is already chosen, we need to pick the remaining $(r-1)$ members from the remaining $(n-1)$ people.
    *   Subproblem: `C(group - 1, members - 1)`
  2.  **Case 2 (Alice is OUT):** Since Alice is rejected, we still need to pick all $r$ members, but we only have $(n-1)$ people left to pick from.
    *   Subproblem: `C(group - 1, members)`
  
  **The Total:** The sum of these two cases gives the total number of combinations!
  *   **Recurrence:** $C(n, r) = C(n-1, r-1) + C(n-1, r)$
- ### 3. The Base Cases (Slide 21)
  For the recursion to stop, we need base cases where the answer is obvious.
  1.  **`members == 1`:** If you need to pick 1 person from a group of $n$, there are exactly $n$ ways to do it.
    *   `return group;`
  2.  **`members == group`:** If you need to pick $n$ people from a group of $n$, there is exactly 1 way to do it (pick everyone).
    *   `return 1;`
  
  *(Note: There is a third base case standard to $nCr$ that your slides omit but is good practice: if `members == 0`, `return 1` (there is 1 way to pick nobody). But the two cases provided are sufficient for $r \ge 1$.)*
- ### 4. The Naive Recursive Code (Slide 21)
  ```c
  int combinations(int group, int members) {
    if (members == 1) return group;
    if (members == group) return 1;
    return combinations(group - 1, members - 1) + 
           combinations(group - 1, members);
  }
  ```
- ### 5. The Overlapping Subproblems (Slides 25–26)
  If we trace `combinations(4, 3)` (Slide 25):
  *   Calls `(3, 2)` and `(3, 3)`.
  *   `(3, 2)` calls `(2, 1)` and **`(2, 2)`**.
  *   This is a small tree, but look at the massive tree for `combinations(6, 4)` on Slide 26!
  *   Notice how many times the exact same subproblem is calculated. For example, **`combinations(3, 2)` is calculated three separate times!**
  *   This is identical to the Fibonacci problem. The time complexity explodes exponentially because we are doing redundant work.
- ### 6. The Homework Assignment (Slide 27)
  **"Use extra space to memoize... Make algorithm run faster - by how much?"**
  
  This is exactly what we did for Fibonacci in Part 1! We need to create a cache.
  
  **The "Deep Dive" Solution:**
  Unlike Fibonacci, which only had one changing variable ($n$), this function has **two changing variables** (`group` and `members`). Therefore, a 1D array `cache[i]` won't work. We need a **2D Array** (a matrix)!
  
  ```java
  // 1. Initialize a 2D cache array filled with -1 (meaning "uncalculated").
  // Size should be[group + 1][members + 1]
  int[][] cache = new int[N+1][R+1]; 
  
  int memoized_combinations(int group, int members, int[][] cache) {
    // 2. Base Cases
    if (members == 1) return group;
    if (members == group) return 1;
    
    // 3. CHECK THE CACHE
    if (cache[group][members] != -1) {
        return cache[group][members]; // We already solved this!
    }
    
    // 4. Do the math and SAVE IT to the cache
    cache[group][members] = memoized_combinations(group - 1, members - 1, cache) + 
                            memoized_combinations(group - 1, members, cache);
                            
    // 5. Return the saved answer
    return cache[group][members];
  }
  ```
  
  **"By how much?" (Complexity Analysis):**
  *   **Naive Time:** Exponential (roughly $O(2^n)$ in the worst case where $r \approx n/2$).
  *   **Memoized Time:** We only calculate each unique `(group, members)` pair exactly once. Since `group` goes from 1 to $n$ and `members` goes from 1 to $r$, there are at most $n \times r$ unique subproblems. The time drops to **$O(n \cdot r)$**!
  *   **Space Complexity:** The 2D array takes **$O(n \cdot r)$** space. The recursion stack takes $O(n)$ space. Total space = $O(n \cdot r)$.
  
  ---
- ### Part 2 Practice Questions (Combinations)
  
  **Q1: Tracing the Cache**
  You run `memoized_combinations(5, 3)`.
  It calls `(4, 2)` and `(4, 3)`.
  The `(4, 2)` branch finishes completely, filling out its side of the cache.
  When the `(4, 3)` branch runs, its first recursive call is to `(3, 2)`.
  Will `(3, 2)` execute its base cases/recursive math, or will it hit the cache instantly? Why?
  *(Hint: Trace the left side of the tree for `(4, 2)`).*
  
  **Q2: The Iterative (Bottom-Up) approach**
  If you wanted to build the 2D array without using recursion (a purely iterative DP approach), you would use a nested `for` loop. 
  Which loop would be on the outside, and which would be on the inside, to ensure that `cache[group - 1][members - 1]` is always calculated *before* `cache[group][members]` needs it?
  
  **Q3: The Base Case Flaw**
  If a user calls `combinations(10, 15)` (asking to pick 15 students from a group of 10), the mathematical answer is 0. 
  What will happen if they pass those arguments into your professor's naive recursive code on Slide 21? Will it return 0, or will it crash?
-