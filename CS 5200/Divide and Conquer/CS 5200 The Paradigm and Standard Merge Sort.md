### 1. The Three-Step Process (The "Mantra")
For any Divide and Conquer (D&C) algorithm on a written quiz, you must be able to identify these three distinct phases:

1.  **Divide:** Break the problem into sub-problems.
  *   *Crucial Detail:* These sub-problems must be **disjoint** (non-overlapping) and usually of equal size. (This distinguishes D&C from Dynamic Programming, where sub-problems overlap).
  *   *In Merge Sort:* This is the step where we calculate `mid = (low + high) / 2`. This step takes **$O(1)$** time.
2.  **Conquer:** Solve the sub-problems recursively.
  *   *Base Case:* You stop when the problem is "small enough" (e.g., array size is 1).
  *   *In Merge Sort:* This is the recursive calls: `mergeSort(left)` and `mergeSort(right)`.
3.  **Combine (Merge):** Take the solutions from the sub-problems and combine them into a solution for the original problem.
  *   *Crucial Detail:* This is usually where the **actual work** happens.
  *   *In Merge Sort:* This is the linear scan that zips two sorted arrays together. This takes **$O(N)$** time.

---
- ### 2. Deep Dive: The Merge Subroutine (Slide 10 & 47)
  The "Merge" function is the engine of the algorithm. You must understand *exactly* how it works to answer complexity questions.
  
  **The Logic:**
  Given two sorted arrays, $L$ (Left) and $R$ (Right):
  1.  Set pointers `i` (for L), `j` (for R), and `k` (for the result array).
  2.  Compare `L[i]` and `R[j]`.
  3.  Place the smaller one into `Result[k]`.
  4.  Increment the pointer of the winner.
  5.  Repeat until one array is empty, then copy the remaining elements of the other array.
  
  **The "Hidden" Cost (Auxiliary Space):**
  *   To merge two arrays efficiently, we cannot easily do it "in-place" (shifting elements around is slow).
  *   **We create a temporary array** (often called `temp` or `B` in your slides).
  *   **Quiz Implication:** This is why Merge Sort has a Space Complexity of **$O(N)$**. You need a buffer array the same size as the input to hold the sorted elements before copying them back.
  
  ---
- ### 3. Visualizing the Recursion Tree (Slide 8 & 11)
  To derive the runtime without just memorizing "N log N", look at the tree structure.
  
  *   **Level 0 (Root):** 1 problem of size $N$. Work to merge: $c \cdot N$.
  *   **Level 1:** 2 problems of size $N/2$. Work to merge: $2 \times (c \cdot N/2) = c \cdot N$.
  *   **Level 2:** 4 problems of size $N/4$. Work to merge: $4 \times (c \cdot N/4) = c \cdot N$.
  
  **The Pattern:**
  *   The work done at **every single level** is exactly $c \cdot N$ (linear).
  *   **Question:** How many levels (height) are there?
    *   We divide $N$ by 2 until we reach 1.
    *   Height = $\log_2 N$.
  *   **Total Time:** $(\text{Work per Level}) \times (\text{Height}) = N \times \log N$.
  
  ---
- ### 4. Comparison to Insertion Sort
  Why do we bother with this complex recursion?
  
  *   **Insertion Sort:**
    *   Worst Case: $O(N^2)$ (Reverse sorted).
    *   Best Case: $O(N)$ (Already sorted).
    *   Space: $O(1)$ (In-place).
  *   **Merge Sort:**
    *   Worst Case: $O(N \log N)$ (Guaranteed).
    *   Best Case: $O(N \log N)$ (Still does all the splits and merges).
    *   Space: $O(N)$ (Expensive!).
  
  **The "Crossover" Point:**
  As hinted in Slide 1, for very small inputs, Insertion Sort is actually **faster** than Merge Sort.
  *   *Why?* Merge Sort has high overhead (function calls, allocating the temp array). Insertion Sort is just simple memory swaps.
  *   This leads directly into the "Hybrid" approach we will cover in Part 2.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1: The Recurrence Relation**
  Write the specific Recurrence Relation $T(n)$ for Merge Sort. Define what each term represents.
  
  **Q2: Space Complexity Trap**
  Does the recursive stack depth count towards the $O(N)$ space complexity of Merge Sort?
  *(Hint: Stack depth is $\log N$, auxiliary array is $N$. Which dominates?)*
  
  **Q3: Odd splits**
  Standard Merge Sort splits indices `(low+high)/2`. What if we split the array into a sizes of $1/3$ and $2/3$ instead of $1/2$ and $1/2$?
  1.  Would the algorithm still work?
  2.  Would the Big-O time complexity change? (Use your intuition or the Master Theorem).