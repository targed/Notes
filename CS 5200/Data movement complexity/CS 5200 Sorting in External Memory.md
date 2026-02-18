### 1. The Bottleneck of Binary Merge Sort (Slide 17)
If we use the standard "Binary" Merge Sort (splitting into 2) on disk, the I/O cost is higher than it needs to be.

**The "Deep Dive" Recurrence:**
$$MT(N) = 2 MT(N/2) + O(N/B)$$
*   **$2 MT(N/2)$**: Two recursive calls on half the data.
*   **$O(N/B)$**: The cost to merge two blocks. Remember, merging is just a linear scan, so it costs $N/B$.
*   **The Base Case:** The recursion stops when the sub-array is small enough to fit in RAM. 
  *   Base case: $MT(M) = O(M/B)$.

**The Resulting Complexity:**
*   Height of the tree: Since we stop at size $M$, we divide by 2 until $N/2^h = M$. Height $= \log_2(N/M)$.
*   Total I/Os: $(\text{Work per level}) \times (\text{Height}) = \mathbf{O((N/B) \log_2 (N/M))}$.

---
- ### 2. The Multi-way Merge Strategy (Slides 15–16)
  Why only merge **2** lists at a time? If we have $M$ space in RAM, we should merge as many lists as we can fit.
  
  **The Constraints (Slide 16):**
  *   To merge lists efficiently, you need at least **one block ($B$)** from each list in the cache at the same time.
  *   If we have $M$ total memory, the maximum number of lists we can merge is **$M/B$**.
  *   This is called an **$M/B$-way Merge**.
  
  ---
- ### 3. $M/B$-way Mergesort (Slides 18–20)
  This is the optimal sorting algorithm for external memory. Instead of dividing the problem by 2, we divide it by $M/B$.
  
  **The "Deep Dive" Recurrence:**
  $$T(N) = \frac{M}{B} T\left(\frac{N}{M/B}\right) + O(N/B)$$
  *   **Branching Factor:** $M/B$ (we create many small subproblems).
  *   **Work per Level:** $N/B$ (we still have to read and write every element once to merge them).
  
  **The Tree Dimensions:**
  1.  **Work per level:** $N/B$.
  2.  **Height of the tree:** How many times can we divide by $M/B$ until we reach the base case?
    *   Height $= \log_{M/B} (N/M)$. 
    *   *(Note: Slides often simplify this to $\log_{M/B} (N/B)$).*
  3.  **Total I/O Complexity:**
    $$\mathbf{\Theta\left(\frac{N}{B} \log_{M/B} \frac{N}{B}\right)}$$
  
  ---
- ### 4. Why is this "Optimal"?
  This formula is known as the **Sorting Bound** for external memory.
  *   In the RAM model, the best you can do is $N \log N$.
  *   In the External model, the best you can do is $(N/B) \log_{M/B} (N/B)$.
  
  **Real World Example:**
  If $M/B = 1000$ (you can fit 1000 blocks in RAM):
  *   **Binary Mergesort** would take $\log_2$ levels (approx 10 levels for a certain $N$).
  *   **$M/B$-way Mergesort** would take $\log_{1000}$ levels (only 1 level!).
  *   **Conclusion:** Multi-way mergesort results in **10x fewer disk reads** than binary mergesort.
  
  ---
- ### 5. Summary of Data Movement Complexity
  1.  **Scanning:** $O(N/B)$.
  2.  **Search (B-Tree):** $O(\log_B N)$. (Faster than Binary Search $O(\log N)$).
  3.  **Sorting:** $O((N/B) \log_{M/B} (N/B))$. (Faster than Binary Mergesort).
  
  ---
- ### Part 4 Practice Questions
  
  **Q1: The Fan-out logic**
  Why can't we merge $N$ lists at once in the first step? Why are we limited to $M/B$?
  
  **Q2: The Sorting Comparison**
  You have $10^9$ elements, $B=10^3$, and $M=10^6$.
  1.  Calculate the I/O cost for **Binary Merge Sort**.
  2.  Calculate the I/O cost for **Multi-way ($M/B$-way) Merge Sort**.
  *(Hint: Work per level is $N/B = 10^6$. Calculate the height of the logs).*
  
  **Q3: Reversing an Array (Slide 11)**
  If you want to reverse an array of size $N$ on disk, what is the I/O complexity?
  *   *Hint:* Do you need to sort? Or can you just scan from both ends? If you scan from both ends, how many blocks do you need in the cache at once?