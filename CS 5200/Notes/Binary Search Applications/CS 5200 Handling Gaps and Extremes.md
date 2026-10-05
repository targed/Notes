### 1. Sparse Search (Slide 16)
**Problem:** Given a sorted array of strings that is interspersed with empty strings (`""`), find the location of a specific target string.

**Example:**
`Input: ["at", "", "", "", "ball", "", "", "car", "", "", "dad", "", ""]`
`Target: "ball"`
- #### A. The Failure of Standard Binary Search
  If we calculate `mid` and `arr[mid]` happens to be `""` (empty string), we hit a wall.
  *   Is `""` less than "ball"? Yes, technically (lexicographically).
  *   **But:** Does that mean "ball" is to the Right? Not necessarily. The empty strings are just placeholders/gaps. "ball" could be to the Left (index 4) or Right (index 7).
  *   **Result:** We lose the ability to discard half the array immediately.
- #### B. The Fix: Linear Search for Pivot
  If `arr[mid]` is empty, we cannot use it as a pivot. We must find the **nearest non-empty string** to use as our comparison point.
  
  **The Algorithm:**
  1.  Calculate `mid`.
  2.  **Gap Handling:**
    *   If `arr[mid] == ""`: Start two pointers, `left = mid - 1` and `right = mid + 1`.
    *   Move them outwards (`left--`, `right++`) until you hit a non-empty string or run out of bounds.
    *   Update `mid` to be the index of that non-empty string.
  3.  **Standard Binary Search:**
    *   Now that `arr[mid]` is a real word ("car", for example), compare it to "ball".
    *   "car" > "ball"? Go Left.
    *   "car" < "ball"? Go Right.
- #### C. The Deep Dive: Complexity Analysis
  This is a "hybrid" complexity, much like the Insertion/Merge sort hybrid.
  
  *   **Best/Average Case:** **$O(\log N)$**.
    *   If the empty strings are scattered thinly (e.g., every other slot), finding the non-empty pivot takes $O(1)$ time. The Binary Search logic dominates.
  *   **Worst Case:** **$O(N)$**.
    *   *Scenario:* `["", "", "", ..., "ball", "", ..., ""]`.
    *   If `mid` lands in a sea of empty strings, the "Gap Handling" loop might have to scan to the very edge of the array just to find a pivot.
    *   In this specific case, Divide and Conquer offers no advantage over a standard Linear Search.
  
  ---
- ### 2. Finding Surrounding Numbers (Floor & Ceiling)
  This wasn't on a specific slide, but it was Problem 3 in your homework. It is a critical theoretical concept for Binary Search.
  
  **The "Crossed Pointers" Property:**
  If you run Iterative Binary Search for a target `X` that **does not exist** in the array, the loop terminates when `low > high`.
  
  At this exact moment of termination:
  1.  **`high`** points to the largest number **smaller** than `X` (The Floor).
  2.  **`low`** points to the smallest number **larger** than `X` (The Ceiling).
  
  **Why this matters:**
  This property allows you to solve "Closest Element" or "Range" problems without writing complex new logic. Just run standard Binary Search, let it fail, and look at where the pointers ended up.
  
  ---
- ### Module Summary: Binary Search Applications
  
  You have now covered the four main variations of Binary Search required for a graduate-level algorithms course:
  
  1.  **Standard:** Finding a value in a sorted array. $O(\log N)$.
  2.  **Index-Value:** Finding Fixed Points ("Serendipity"). Requires distinct integers.
  3.  **Structural:** Search in Rotated Arrays. Requires checking "Which side is sorted?".
  4.  **Gaps:** Sparse Search. Requires linear fix for empty pivots.
  
  ---
- ### Part 4 Practice Questions
  
  **Q1: Sparse Search Recursion**
  In Sparse Search, if `arr[mid]` was empty and you found a new pivot at `mid + 5` (which was "car"), and "car" > "ball" (Target), you need to search Left.
  *   What should your new `high` variable be for the recursive call?
    *   A) The original `mid - 1`?
    *   B) The new `mid - 1` (where "car" was found)?
    *   *(Hint: If you choose B, are you re-checking the empty strings between original mid and car? Does it matter?)*
  
  **Q2: The "Closest" Element**
  Given array `[1, 3, 8, 10, 15]` and target `9`.
  Binary search will finish with `high` at index 2 (value 8) and `low` at index 3 (value 10).
  *   Write the mathematical condition to decide whether 8 or 10 is the "closest" element to 9.
  
  **Q3: Infinite Loop Risk**
  In Sparse Search, if `mid` is empty, we search left/right. What happens if the **entire** array is empty strings? How do you ensure your "Gap Handling" loop terminates?