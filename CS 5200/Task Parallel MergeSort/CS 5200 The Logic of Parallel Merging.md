### 1. The Core Idea: Divide and Conquer the Merge
In a sequential merge, you compare the smallest elements of two lists and pick one. This is inherently serial.
To make it parallel, we use a **Divide and Conquer** approach **on the sorted data itself**:
1.  Pick an element from one array to act as a **Pivot** ($x$).
2.  Use that Pivot to split **both** arrays into two parts (elements $\le x$ and elements $> x$).
3.  Recursively merge the "small" parts and the "large" parts in parallel.

---
- ### 2. Choosing the Pivot (The Median Trick)
  Slide 29 of the supplementary text and Slide 11 of the PPT explain how to pick the pivot $x$ to ensure the work is balanced.
  
  *   **The Rule:** Always pick the **Median** of the **larger** of the two subarrays.
  *   **Why the larger one?** By picking the median of the larger array, we guarantee that at least half of *that* array is discarded in the sub-problems. This prevents a "lopsided" recursion tree (similar to why we want a good pivot in Quicksort).
  
  ---
- ### 3. Finding the Split Point (Slides 10 & 12)
  Once we have our pivot $x$ from the first array, we need to know where it would fit in the second array.
  
  **Example from Slide 10:**
  *   **Sorted Array:** `[1, 3, 5, 7, 13, 15, 20, 21, 25, 30]`
  *   **Target ($x$):** `10`
  *   **Goal:** Find the index where `10` belongs.
  *   **The Tool:** **Binary Search**.
    *   We don't need to scan the whole array. Because it is sorted, Binary Search finds the "Split Point" in **$O(\lg n)$** time.
    *   In this case, 10 would fall between 7 and 13. The split index returned would be the index of `13`.
  
  ---
- ### 4. Visualizing the "Split" (Slide 11 - Figure 26.6)
  This is the most important diagram in the module. Let's break down what is happening:
  
  1.  **Array A** is divided into two halves at $q_1$ (the median). The value at $q_1$ is our pivot **$x$**.
  2.  **Binary Search** is performed on the other half of the data to find $q_2$ (where $x$ fits).
  3.  **Now we have four pieces:**
    *   Left side of Array 1 (all $\le x$)
    *   Left side of Array 2 (all $\le x$)
    *   Right side of Array 1 (all $\ge x$)
    *   Right side of Array 2 (all $\ge x$)
  4.  **The Parallel Move:**
    *   Task 1: Merge (Left 1 + Left 2) $\rightarrow$ put into the bottom array $B$.
    *   Task 2: Merge (Right 1 + Right 2) $\rightarrow$ put into the bottom array $B$.
    *   **These two tasks can run at the exact same time.**
  
  ---
- ### 5. The "3/4" Guarantee (Supplementary Deep Dive)
  On a quiz, the professor might ask: **"What is the worst-case size of a sub-problem in Parallel Merge?"**
  
  *   **The Logic:** If we always pick the median of the larger array, even if that pivot is "useless" for the second array (e.g., the pivot is smaller than everything in the second array), we still know that:
    *   We definitely split the large array in half ($n_1/2$).
    *   The other array is at most the same size ($n_1$).
  *   **The Math:** $\frac{1}{2}n_1 + n_2 \le \frac{1}{2}n_1 + n_1 = \frac{3}{2}n_1$.
  *   Since $n_1$ is at least $n/2$ (because it was the larger half), the math works out so that **no sub-problem is ever larger than 3/4 of the total size ($3n/4$)**.
  *   **Result:** This ensures the recursion tree depth is logarithmic ($O(\lg n)$).
  
  ---
- ### Practice Questions
  
  **Q1: The Split Point**
  You are merging `Array 1: [10, 20, 30, 40]` and `Array 2: [5, 15, 25, 35]`.
  1.  Which array is "larger"? (They are equal, so pick Array 1).
  2.  What is the median pivot $x$ of Array 1?
  3.  Perform a "binary search" for $x$ in Array 2. What is the split index?
  
  **Q2: The Parallel Gain**
  In the sequential merge, we had one processor doing $N$ work. In this parallel merge, we have two processors. Each processor is now merging two lists that are roughly what size?
  *   *Hint:* Look at Figure 26.6. Are the two "spawned" merges significantly smaller than $N$?
  
  **Q3: Recursive Base Case**
  What happens in the parallel merge if one of the subarrays is empty? Look at `P-MERGE-AUX` in Slide 13/14. How does the code handle it?