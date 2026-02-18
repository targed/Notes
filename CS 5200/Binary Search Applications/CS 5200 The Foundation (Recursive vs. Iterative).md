### 1. The Core Philosophy
Binary Search is the quintessential Divide and Conquer algorithm, but it differs from Merge Sort in one key way:
*   **Merge Sort:** Solves **all** sub-problems (Left and Right) and combines them.
*   **Binary Search:** Discards half the problem immediately. It only conquers **one** sub-problem.
*   **Result:** This is why the runtime is $O(\log N)$ instead of $O(N)$.
- ### 2. Recursive Implementation (Slide 2)
  The recursive approach is often more intuitive to write but comes with a hidden cost.
  
  **The Logic:**
  1.  Calculate `mid`.
  2.  Base Case: If `arr[mid] == target`, return `mid`.
  3.  Recursive Step:
    *   If `arr[mid] < target`, call function on `(mid + 1, high)`.
    *   If `arr[mid] > target`, call function on `(low, mid - 1)`.
  
  **The Deep Dive Analysis (Space Complexity):**
  *   **Time:** $O(\log N)$.
  *   **Space:** **$O(\log N)$**.
  *   **Why?** Every recursive call adds a frame to the Call Stack. Even though we are effectively just moving pointers, the recursion preserves the state of the previous calls until the base case is hit.
  *   *Professor Note:* In "Tail Call Optimized" languages, this might be optimized to $O(1)$, but in Java/Python, it remains $O(\log N)$.
- ### 3. Iterative Implementation (Slide 3)
  This is the industry standard and preferred for Space Complexity reasons.
  
  **The Logic:**
  Instead of calling a function, we wrap the logic in a `while (low <= high)` loop and update the `low` and `high` variables in place.
  
  **The Deep Dive Analysis (Space Complexity):**
  *   **Time:** $O(\log N)$.
  *   **Space:** **$O(1)$**.
  *   **Why?** We only need three integer variables (`low`, `high`, `mid`) regardless of whether the array has 10 elements or 10 billion elements.
- ### 4. The "Bugs" of Binary Search (Implementation Nuances)
  Slides 4-5 show code, but there are two famous bugs that professors love to quiz on:
- #### **A. The Overflow Bug**
  *   **Standard Code:** `int mid = (low + high) / 2;`
  *   **The Issue:** If `low` and `high` are both very large positive integers (close to `Integer.MAX_VALUE`), adding them together causes an **Integer Overflow**, resulting in a negative number.
  *   **The Fix:** `int mid = low + (high - low) / 2;`
    *   This is mathematically equivalent but prevents the intermediate sum from exceeding `MAX_VALUE`.
- #### **B. The Loop Condition (Off-by-one errors)**
  *   **Correct:** `while (low <= high)`
    *   This ensures that if the array shrinks to size 1 (where `low == high`), we still check that final element.
  *   **Incorrect:** `while (low < high)`
    *   If the target is the very last element checked, this loop will terminate one step early and return "Not Found."
- ### 5. Deriving Complexity (Slide 16 reference)
  Using the Master Theorem on Binary Search:
  *   Recurrence: $T(N) = 1T(N/2) + O(1)$.
    *   $a = 1$ (We solve 1 subproblem).
    *   $b = 2$ (We divide the input by 2).
    *   $f(n) = O(1)$ (Comparison and index calculation takes constant time).
  *   **Comparison:**
    *   $n^{\log_b a} = n^{\log_2 1} = n^0 = 1$.
    *   $f(n) = 1$.
  *   **Case 2:** $f(n) = \Theta(n^{\log_b a})$.
  *   **Result:** $T(N) = \Theta(n^0 \log N) = \Theta(\log N)$.
  
  ---
- ### Part 1 Practice Questions
  
  **Q1: Loop Termination**
  If you use `high = mid` instead of `high = mid - 1` inside the loop, under what specific condition will the algorithm enter an **Infinite Loop**?
  
  **Q2: Base Cases**
  In the recursive version, what is the specific base case that returns "Not Found" (-1)?
  
  **Q3: Middle Element**
  If the array length is even (e.g., `[1, 2, 3, 4]`), which element is chosen as `mid` using integer division? Does this affect the correctness of the algorithm?
  
  **Thinking about Q1 is crucial for writing bug-free binary search code.**