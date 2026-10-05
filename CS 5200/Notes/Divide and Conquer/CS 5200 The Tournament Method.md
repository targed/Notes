### 1. The Concept (Slides 23–25)
Instead of scanning the array linearly like a `for` loop (`max = A[0]; for x in A...`), we organize the comparisons into a **Binary Tree**.

**The Algorithm:**
1.  **Divide:** Split the array into two halves.
2.  **Conquer:** Recursively find the winner (Max) of the left half and the winner (Max) of the right half.
3.  **Combine:** Compare the two semi-finalists. Return the winner.

**Pseudocode (Slide 25):**
```java
int findMax(arr, low, high) {
  // Base Case: Only 1 player
  if (low == high) return arr[low];

  mid = (low + high) / 2;
  leftMax = findMax(arr, low, mid);
  rightMax = findMax(arr, mid + 1, high);

  // The actual comparison "Match"
  if (leftMax > rightMax) return leftMax;
  else return rightMax;
}
```

---
- ### 2. The Deep Dive Analysis (Comparisons vs. Time)
  This is where the "Deep Dive" happens. A linear scan takes $O(N)$. This recursive method takes $O(N)$. Why bother?
- #### A. Counting Comparisons (Slides 26–28)
  Let's count exactly how many comparisons happen.
  *   **Leaf Level:** We compare pairs. $N/2$ comparisons.
  *   **Next Level:** We compare the winners. $N/4$ comparisons.
  *   **...**
  *   **Root:** 1 comparison.
  
  **Summation (Geometric Series):**
  $$ \frac{N}{2} + \frac{N}{4} + \frac{N}{8} + \dots + 1 = N - 1 $$
  
  **Conclusion:** The Tournament Method performs **exactly** $N-1$ comparisons. This is identical to the Linear Scan.
- #### B. Total Nodes (Reduction Method - Slide 28)
  Slide 28 uses a specific math trick to count the total execution steps (nodes in the recursion tree).
  *   A tournament tree for $N$ leaves is a **Full Binary Tree**.
  *   Total Nodes = (Number of Leaves) + (Number of Internal Nodes).
  *   Property of Full Binary Trees: $\text{Internal Nodes} = \text{Leaves} - 1$.
  *   Total Nodes = $N + (N - 1) = 2N - 1$.
  *   **Complexity:** Since we touch $2N-1$ nodes, $T(n) = O(N)$.
  
  ---
- ### 3. The "Why" (Professor Nuance)
  If the work is the same as a `for` loop, why use Divide and Conquer?
  
  1.  **Parallelism:**
    *   In a `for` loop, iteration $i$ cannot happen until $i-1$ is done (dependency).
    *   In a Tournament, all $N/2$ leaf comparisons can happen **simultaneously** on different processor cores. This reduces the *span* (longest path) from $N$ to $\log N$.
  2.  **Finding Min and Max Simultaneously:**
    *   **Naive Way:** Scan for Max ($N-1$), then Scan for Min ($N-1$). Total: $2N - 2$ comparisons.
    *   **Tournament Optimization:**
        *   Pair up elements. Compare them ($N/2$ comparisons).
        *   Put the "Winners" in one bucket and "Losers" in another.
        *   Find Max *only* among Winners ($N/2$).
        *   Find Min *only* among Losers ($N/2$).
    *   **Total:** $1.5N$. This is 25% faster in terms of comparisons!
  
  ---
- ### 4. Space Complexity (Slide 30)
  This is a recursive algorithm.
  *   **Space:** It depends on the height of the tree.
  *   **Height:** $\log_2 N$.
  *   **Complexity:** $O(\log N)$ (Auxiliary Stack Space).
  *   *Note:* A linear scan takes $O(1)$ space. So, the Tournament method loses on memory efficiency unless parallelized.
  
  ---
- ### Part 3 Practice Questions
  
  **Q1: The Second Largest Element**
  This is a classic interview/exam extension of this topic.
  Using the Tournament Tree structure we just built, how can you efficiently find the **Second Largest** number?
  *   *Hint:* The second largest number *must* have lost to the Max at some point. Do you need to search the whole array again?
  
  **Q2: The "Reduction" Concept**
  Slide 3 mentions "Reduction." If you have a black-box function `FindMax(arr)` that works in $O(N)$, can you use it to sort an array?
  *   What would be the complexity of that sort? (Finding max, swapping to end, repeat).
  
  **Q3: Base Case Nuance**
  In `findMax`, the base case is `low == high` (1 element). What happens if the array size is 0? Or what if you change the base case to `low == high - 1` (2 elements)? Does the complexity change?