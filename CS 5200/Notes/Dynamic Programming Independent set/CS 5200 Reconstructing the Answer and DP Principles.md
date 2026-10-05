### 1. Reconstructing the Answer (Slide 41)
Our Bottom-Up DP algorithm gave us an array `A` where the last element `A[n]` holds the maximum possible weight. But it doesn't store the actual vertices! 

To find the vertices, we don't need to save complex lists during the algorithm. We just leave "tracks in the mud" and **trace backward** through the filled-in array `A`.

**The Logic:**
Start at the end (`i = n`). Ask yourself: *"How did `A[i]` get its value?"*
Remember our recurrence: `A[i] = max(A[i-1], A[i-2] + w_i)`

1.  **If `A[i-1] >= A[i-2] + w_i`:** This means Case 1 won. Vertex $v_i$ was **excluded**. We simply move backward one step to `i - 1`.
2.  **Otherwise:** Case 2 won. Vertex $v_i$ was **included**. We add $v_i$ to our final answer set, and because we picked it, we must skip its neighbor. We move backward two steps to `i - 2`.

**Deep Dive Walkthrough (Slide 41):**
Weights: `[3, 2, 1, 6, 4, 5]`
Array `A`: `[0, 3, 3, 4, 9, 9, 14]`

*   **Step 1:** Start at $i=6$ (Value 14).
  *   Did it come from `A[5]` (which is 9)? Or `A[4] + w_6` ($9 + 5 = 14$)?
  *   It came from Case 2! **Include $v_6$**. Jump to $i=4$.
*   **Step 2:** Currently at $i=4$ (Value 9).
  *   Did it come from `A[3]` (which is 4)? Or `A[2] + w_4` ($3 + 6 = 9$)?
  *   It came from Case 2! **Include $v_4$**. Jump to $i=2$.
*   **Step 3:** Currently at $i=2$ (Value 3).
  *   Did it come from `A[1]` (which is 3)? Or `A[0] + w_2` ($0 + 2 = 2$)?
  *   It came from Case 1! `3 > 2`. **Exclude $v_2$**. Jump to $i=1$.
*   **Step 4:** Currently at $i=1$ (Value 3).
  *   Base case! **Include $v_1$**.
*   **Final Answer:** The set is **{$v_1, v_4, v_6$}**.

*Complexity of Reconstruction:* Since we just do a single backward pass through the array, it takes **$O(n)$ time**.

---
- ### 2. Dynamic Programming vs. Divide & Conquer (Slides 51–55)
  Your professor dedicated several slides to this comparison. Expect a short-answer question on this!
  
  Here are the 3 major differences:
  1.  **Overlapping vs. Disjoint Subproblems:**
    *   **D&C:** Subproblems are strictly independent (disjoint). Sorting the left half of an array has nothing to do with sorting the right half. Caching answers is useless.
    *   **DP:** Subproblems **overlap** heavily. We evaluate the same subgraphs multiple times. Caching (Memoization/Tabulation) is mandatory.
  2.  **The Size of the "Cut":**
    *   **D&C:** Usually chops the problem by a massive fraction (e.g., $n/2$).
    *   **DP:** Usually nibbles away at the problem just a tiny bit at a time (e.g., $n-1$ or $n-2$) to exhaustively check every valid possibility.
  3.  **The Goal (Polynomial vs. Exponential):**
    *   **D&C** is typically used to make an already polynomial algorithm faster (e.g., taking sorting from $O(n^2)$ down to $O(n \log n)$).
    *   **DP** is the "killer app" for optimization problems. It takes problems that would require $O(2^n)$ exhaustive brute-force search and miraculously crushes them down into polynomial time like $O(n)$ or $O(n^2)$.
  
  ---
- ### 3. Fun Fact: The Story Behind the Name (Slides 56–58)
  Professors love throwing this in as a bonus/trivia question.
  *   **Who:** Richard Bellman (working at the RAND Corporation in the 1950s).
  *   **The Problem:** The Secretary of Defense (Wilson) had a pathological hatred of the words "research" and "mathematics."
  *   **The Solution:** Bellman needed to hide his math research. He chose "Programming" because it meant "planning/scheduling" at the time (like TV programming). He chose "Dynamic" because it sounded active and was impossible to use in a negative, pejorative way. 
  *   *Conclusion:* The name "Dynamic Programming" is essentially a 1950s marketing gimmick to secure government funding!
  
  ---
- ### Part 4 Practice Questions (Reconstruction & Theory)
  
  **Q1: Tracing Reconstruction**
  Let's use the graph from our Part 3 practice question.
  Weights: `w = [10, 5, 20, 15]`
  Let's say your completed DP Array `A` is: `[0, 10, 10, 30, 30]`.
  Trace backward starting from $i=4$. Which specific vertices make up the Maximum Weight Independent Set?
  
  **Q2: The "Space Optimization" Trade-off**
  In Part 3, we learned a trick: if we only care about the final max weight, we can reduce the Space Complexity from $O(n)$ to $O(1)$ by only storing the "previous two" variables instead of an entire array `A`.
  If the exam asks you to **reconstruct the actual set of vertices**, can you still use this $O(1)$ space optimization? Why or why not?
  
  **Q3: The D&C "Commitment" Issue**
  According to the textbook excerpt on Slide 52, a Divide and Conquer algorithm "commits to a single way of dividing the input." 
  How does Dynamic Programming differ in how it handles subproblems?
  A) DP commits to the greedy choice immediately.
  B) DP keeps its options open, evaluating multiple ways to construct the answer and picking the best one.
  C) DP divides the problem into $n$ pieces instead of 2.
  D) DP uses randomized pivots.
  
  ---
- ### **Solutions & Explanations**
  
  **A1: Vertices $v_1$ and $v_3$**
  *   Start at $i=4$, `A[4] = 30`. Compare `A[3]` (30) vs `A[2] + w_4` ($10 + 15 = 25$). Case 1 wins ($30 > 25$). **Exclude $v_4$**. Jump to $i=3$.
  *   At $i=3$, `A[3] = 30`. Compare `A[2]` (10) vs `A[1] + w_3` ($10 + 20 = 30$). Case 2 wins. **Include $v_3$**. Jump to $i=1$.
  *   At $i=1$, Base case. **Include $v_1$**.
  *   The set is **$\{v_1, v_3\}$**.
  
  **A2: No, you cannot use the $O(1)$ space optimization.**
  *   **Why:** To reconstruct the answer, you must trace backward from $n$ all the way to $0$. You need to know the "historical" decisions made at every single step ($A[n-1], A[n-2], A[n-3]$, etc.). If you only save the last two variables, you have erased the "tracks in the mud" and cannot trace your way back to the beginning. You *must* use $O(n)$ space to hold the full array if reconstruction is required.
  
  **A3: B) DP keeps its options open...**
  *   **Why:** D&C blindly cuts the array in half no matter what the data looks like. DP looks at the data and says, "Let's calculate what happens if we *do* use this item, and let's calculate what happens if we *don't* use this item, and then let's `max()` them to see who wins."
  
  ---
-