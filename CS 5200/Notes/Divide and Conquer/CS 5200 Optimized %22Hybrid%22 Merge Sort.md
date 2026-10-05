### 1. The Motivation (Slide 12)
Standard Merge Sort is asymptotically optimal ($N \log N$), but it has high "constant factors" (overhead):
*   **Recursion Overhead:** Pushing/popping stack frames takes CPU cycles.
*   **Memory Allocation:** Creating temp arrays is expensive.

**Insertion Sort** is asymptotically slow ($N^2$), but it has very low overhead.
*   **Fact:** For very small arrays (e.g., $N < 10$ or $N < 50$), Insertion Sort is faster than Merge Sort because the $N^2$ curve hasn't shot up yet, while Merge Sort is bogged down by function calls.

**The Strategy:**
Run Merge Sort, but **change the base case**. Instead of stopping at $N=1$, stop when $N \le k$ (where $k$ is a small number like ~43). Run Insertion Sort on these small chunks, then Merge them back up.

---
- ### 2. The Deep Dive Analysis (Slides 17–21)
  This is the mathematical core of this module. You need to derive the complexity of this new hybrid algorithm in terms of $N$ and $k$.
  
  We calculate the total time as: **(Time to Sort Leaves) + (Time to Merge)**.
- #### A. The Leaves (Insertion Sort Phase)
  *   We stop the recursion when sub-arrays are size $k$.
  *   How many sub-arrays (chunks) are there? $\frac{N}{k}$.
  *   What is the cost to sort **one** chunk of size $k$ using Insertion Sort? $O(k^2)$.
  *   **Total Sorting Cost:**
    $$ \text{Chunks} \times \text{Cost per Chunk} = \frac{N}{k} \times O(k^2) = O(Nk) $$
- #### B. The Internal Nodes (Merge Phase)
  *   We still have to merge these sorted chunks back together.
  *   In standard Merge Sort, the tree height is $\log N$.
  *   In this Hybrid Sort, we stop early. The tree is shorter.
  *   **New Tree Height:** We divide $N$ by 2 until we reach $k$.
    $$ \frac{N}{2^h} = k \implies 2^h = \frac{N}{k} \implies h = \log\left(\frac{N}{k}\right) $$
  *   The work done at each level of merging is still $O(N)$.
  *   **Total Merging Cost:**
    $$ \text{Work per Level} \times \text{Height} = O\left(N \cdot \log\left(\frac{N}{k}\right)\right) $$
- #### C. Total Complexity Formula
  $$ T(N) = O(Nk + N \log(N/k)) $$
  
  ---
- ### 3. The "Limit" Questions (Slides 19–20)
  These are specific questions from the textbook (CLRS Exercise 2.1) that appear in the slides.
  
  **Question 1: If $k$ is a constant (e.g., $k=50$), is this asymptotically faster?**
  *   **Math:** Substitute $k=O(1)$.
    *   $O(N(1) + N \log(N/1)) = O(N + N \log N)$.
    *   Dominant term: **$O(N \log N)$**.
  *   **Answer:** Asymptotically, it is the **same** complexity class. But in the real world (wall-clock time), it is faster because we reduced the overhead.
  
  **Question 2: What is the largest value of $k$ allowed to preserve $O(N \log N)$?**
  *   We want the Insertion Sort part ($Nk$) to be "absorbed" by the Merge Sort part ($N \log N$).
  *   We need $Nk \le O(N \log N)$.
  *   Divide by $N$: $k \le O(\log N)$.
  *   **Answer:** If we pick $k = \log N$, the algorithm stays efficient.
    *   *Check:* $O(N \log N + N \log(N / \log N))$. This works.
  *   **Trap:** If we pick a larger $k$, like $k = \sqrt{N}$, the complexity becomes $O(N \sqrt{N})$, which is worse than $N \log N$.
  
  ---
- ### 4. Practical Implementation Nuance
  In Slide 15, notice how the code changes:
  
  **Old Code:**
  ```java
  if (low < high) { ... recurse ... }
  ```
  
  **New Code:**
  ```java
  if (high - low < 50) { 
    InsertionSort(arr, low, high); 
  } else { 
    ... recurse ... 
  }
  ```
  *Note:* The Insertion Sort implementation must be able to sort a specific **range** `(low, high)` of an array, not just a whole array starting at 0.
  
  ---
- ### Part 2 Practice Questions
  
  **Q1: The Algebra Check**
  Using the logarithm quotient rule, rewrite the complexity $O(N \log(N/k))$ into two terms. Does this reveal why increasing $k$ reduces the merge work?
  
  **Q2: The "Bad" Tuning**
  Suppose you set the threshold $k = N$ (i.e., you stop recursion immediately).
  1.  What does the algorithm effectively become?
  2.  Plug $k=N$ into the formula $O(Nk + N \log(N/k))$. What is the resulting complexity?
  
  **Q3: Space Complexity**
  Does this optimization improve the **Space Complexity** (Auxiliary Memory) of Merge Sort? Why or why not?