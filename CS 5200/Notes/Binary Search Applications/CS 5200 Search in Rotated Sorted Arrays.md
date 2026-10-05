### 1. The Definition (Slide 11)
A **Rotated Sorted Array** is a normal sorted array that has been shifted right by $K$ positions. The elements that "fall off" the end wrap around to the front.

*   **Original:** `[1, 2, 3, 4, 5, 7, 10, 14]`
*   **Rotated:** `[14, 1, 2, 3, 4, 5, 7, 10]` (Rotated by 1)
*   **Rotated:** `[5, 7, 10, 14, 1, 2, 3, 4]` (Rotated by 4)

**The Key Property:**
In any rotated sorted array, if you pick **any** middle point, **one half of the array will always be perfectly sorted**, and the other half will contain the "rotation pivot" (the break in order).

---
- ### 2. Searching for a Target X (Slides 13–15)
  We want to find index of $X$ in $O(\log N)$. We cannot simply check `if X < mid` because the array isn't fully sorted.
  
  **The Algorithm:**
  1.  Find `mid`.
  2.  **Step A: Identification.** Check which side is sorted.
    *   If `arr[low] <= arr[mid]`, the **Left** side is sorted.
    *   Else, the **Right** side is sorted.
  3.  **Step B: Selection.** Check if $X$ lies within the sorted range.
    *   *Case 1 (Left is Sorted):* Is `low <= X < mid`?
        *   Yes: Go Left.
        *   No: Go Right (The answer must be in the unsorted/pivot side).
    *   *Case 2 (Right is Sorted):* Is `mid < X <= high`?
        *   Yes: Go Right.
        *   No: Go Left.
  
  **Example Walkthrough (Slide 13 - Array 1):**
  Input: `[10, 15, 20, 0, 5]`. Target: `5`.
  *   `low=0` (10), `high=4` (5).
  *   `mid=2`. Value: **20**.
  *   **Identify:** Is `arr[low] (10) <= arr[mid] (20)`? **Yes.** Left is sorted.
  *   **Select:** Is target `5` inside `[10 ... 20]`? **No.**
  *   **Action:** Go Right (`low = mid + 1`).
  *   *New Range:* `[0, 5]`. Standard binary search finds 5.
  
  ---
- ### 3. Finding the Maxima/Pivot (Slide 17)
  Instead of finding a number $X$, we want to find the **largest number** in the array. In a rotated array, this is the element right before the "cliff" drops.
  
  **The Logic:**
  Standard Binary Search compares `mid` to `target`. Here, we compare `mid` to its **neighbors**.
  
  1.  **The "Cliff" Check:**
    *   If `arr[mid] > arr[mid + 1]`, then `mid` is the Max. (Example: `[... 25, 1 ...]`. $25 > 1$).
    *   If `arr[mid] < arr[mid - 1]`, then `mid - 1` is the Max.
  2.  **The Traversal:**
    *   If `arr[low] <= arr[mid]`: The Left side is increasing nicely. The "Peak" must be further to the **Right**.
    *   Else (`arr[low] > arr[mid]`): The Left side is broken (it contains the pivot). Go **Left**.
  
  **Addressing the Slide 17 Error:**
  The slide says: *"Maxima could be 25 or 14... 20 is the maximum"*.
  *   Array: `{15, 16, 19, 20, 25, 1, 3, 4, 5, 7, 10, 14}`
  *   **Correction:** 25 is the Global Maximum (and the Pivot). 20 is just a number. 14 is a local maximum at the end. The algorithm above finds 25.
  
  ---
- ### 4. Deep Dive: The "Duplicate" Trap
  What happens if the array has duplicates?
  Input: `[2, 2, 2, 3, 4, 2]` (Rotated).
  *   `low=2`, `mid=2`, `high=2`.
  *   Is Left sorted? `arr[low] <= arr[mid]` ($2 \le 2$). Yes.
  *   Is Right sorted? `arr[mid] <= arr[high]` ($2 \le 2$). Yes.
  *   **Problem:** We cannot determine which side holds the "weird" part (the 3, 4).
  *   **Solution:** If `arr[low] == arr[mid] == arr[high]`, we cannot use Divide and Conquer. We must increment `low++` and `high--` linearly until they are different.
  *   **Complexity:** Worst case degrades to **$O(N)$**.
  
  ---
- ### Part 3 Practice Questions
  
  **Q1: Small Arrays**
  Trace the "Search for Target" algorithm on this array: `[3, 1]`. Target: `1`.
  *   Does `arr[low] <= arr[mid]` handle the 2-element case correctly?
  
  **Q2: Number of Rotations**
  If you find the index of the Minimum element (the Pivot), how does that index relate to the number of times the array was rotated?
  *   *Hint:* In `[4, 5, 1, 2, 3]`, the min is at index 2.
  
  **Q3: Pivot at the End**
  If the array is *not* rotated (e.g., `[1, 2, 3, 4, 5]`), how does the "Find Maxima" algorithm behave? Does it eventually check the very last element?