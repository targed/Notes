### 1. The Reconstruction Logic (Slide 24)
The matrix `A` contains all the "tracks in the mud" we need. We don't need to re-do any complex math; we just start at the final answer in the bottom-right corner (`A[n][C]`) and work our way backward to `A[0][0]`.

**The Algorithm:**
At every item $i$ and current capacity $c$, we ask: *"How did this cell get its value?"*
Remember, the cell either inherited the value from directly above it (Case 1: Excluded), or it calculated a new maximum by looking up and left (Case 2: Included).

We check the exact same condition the original loop used:
*   **Check Case 2:** `if (s_i <= c) AND (A[i-1][c - s_i] + v_i >= A[i-1][c])`
  *   If this is true, Case 2 won. **Item $i$ is IN the knapsack.**
  *   We add $i$ to our solution set.
  *   We subtract its size from our capacity: `c = c - s_i`.
  *   We move up to the previous row: `i = i - 1`.
*   **Check Case 1:** Otherwise, Case 1 won. **Item $i$ is NOT in the knapsack.**
  *   The capacity stays exactly the same.
  *   We just move up to the previous row: `i = i - 1`.
- ### 2. Time Complexity of Reconstruction
  Building the massive matrix took $O(nC)$ time.
  Reconstructing the answer is blazingly fast. We do one single `for` loop that counts backward from $n$ to $1$. In each step, we do $O(1)$ math and move up exactly one row.
  *   **Time Complexity:** $\mathbf{O(n)}$.
  
  ---
- ### 3. Walking Through Practice Problem 16.4 (Slides 26–27)
  
  Your professor provided a full practice problem to trace. Let's verify the answer together using the reconstruction logic.
  
  **The Setup:**
  *   Capacity $C = 9$
  *   Item 1: `v=1, s=1`
  *   Item 2: `v=2, s=3`
  *   Item 3: `v=3, s=2`
  *   Item 4: `v=4, s=5`
  *   Item 5: `v=5, s=4`
  
  **The Matrix (Slide 27):**
  *(Note: In the slide's visual table, the Columns are the items $i$ (0 to 5), and the Rows are the capacities $c$ (9 down to 0)).*
  
  Let's trace backward from the final answer at the top-right corner: **`i=5, c=9`**.
  
  *   **Step 1 (`i=5, c=9`):** 
    *   The value here is **10**. 
    *   Look at the column to the left (excluding item 5): `A[4][9]` is **8**.
    *   Since $10 > 8$, Item 5 MUST have been included!
    *   **Action:** **Include Item 5**. New capacity $c = 9 - s_5 = 9 - 4 = \mathbf{5}$.
  
  *   **Step 2 (`i=4, c=5`):**
    *   The value here is **5**.
    *   Look to the left (excluding item 4): `A[3][5]` is also **5**.
    *   Since they are equal, Item 4 didn't improve our score. It was excluded.
    *   **Action:** **Exclude Item 4**. Capacity remains $\mathbf{5}$.
  
  *   **Step 3 (`i=3, c=5`):**
    *   The value here is **5**.
    *   Look to the left (excluding item 3): `A[2][5]` is **3**.
    *   Since $5 > 3$, Item 3 MUST have been included!
    *   **Action:** **Include Item 3**. New capacity $c = 5 - s_3 = 5 - 2 = \mathbf{3}$.
  
  *   **Step 4 (`i=2, c=3`):**
    *   The value here is **2**.
    *   Look to the left (excluding item 2): `A[1][3]` is **1**.
    *   Since $2 > 1$, Item 2 MUST have been included!
    *   **Action:** **Include Item 2**. New capacity $c = 3 - s_2 = 3 - 3 = \mathbf{0}$.
  
  *   **Step 5 (`i=1, c=0`):**
    *   Capacity is 0. We can't hold anything else. **Exclude Item 1**.
  
  **Final Solution:** We included **Items 5, 3, and 2**. 
  *   Let's check our work: Total value = $5 + 3 + 2 = \mathbf{10}$. Total size = $4 + 2 + 3 = \mathbf{9}$. It fits perfectly and achieves the max value of 10! This perfectly matches Slide 27.
  
  ---
- ### Part 4 Practice Questions (Reconstruction & Theory)
  
  **Q1: The Space Optimization Trade-off (Revisiting an old question)**
  In Part 3, we discussed that you can optimize the Space Complexity of Knapsack from $O(nC)$ down to $O(C)$ by only keeping the "current" row and the "previous" row of the matrix in memory, discarding the older rows as you go.
  If your boss asks you to output the *actual items* the burglar stole (not just the total value), can you still use this $O(C)$ space optimization? Why or why not?
  
  **Q2: Tie-Breaking in Reconstruction**
  Look closely at the pseudo-code on Slide 24:
  `if si <= c and A[i-1][c-si] + vi >= A[i-1][c] then ... include i`
  Notice the **`>=`** operator. 
  If excluding the item gives a max value of 15, and including the item *also* gives a max value of 15, does this specific algorithm choose to include the item or exclude the item? 
  
  **Q3: The Pseudo-Polynomial Trap**
  A standard Divide & Conquer algorithm like Merge Sort takes $O(n \log n)$ time. 
  The Knapsack algorithm takes $O(nC)$ time. 
  If $n = 100$ and $C = 1,000,000$, which algorithm will execute significantly more instructions? Why is Knapsack not considered a true "Polynomial Time" algorithm?
  
  **Q4: Dependency Direction**
  When filling out the 2D DP matrix for Knapsack, do you have to fill it out row-by-row (top to bottom), or can you fill it out column-by-column (left to right)? 
  *(Hint: Think about where `A[i][c]` looks to get its data. Does it look at future rows or columns?)*
  
  ---
- ### **Solutions & Explanations**
  
  **A1:** **No, you cannot use the space optimization.** 
  *   To perform the traceback, you must be able to ask "What was the optimal value at `i-1`, `i-2`, all the way to `i=0`." If you threw away the previous rows to save RAM, you destroyed the "tracks in the mud." You *must* keep the entire $O(nC)$ matrix in memory to reconstruct the item set.
  
  **A2:** **It chooses to INCLUDE the item.** 
  *   Because of the `>=` (greater than or equal to), a tie goes to the inclusion case. Both choices are mathematically optimal (they result in the same final value), but the `>=` dictates the deterministic behavior of the code.
  
  **A3:** **Knapsack will execute millions more instructions.**
  *   Merge Sort depends ONLY on $n$. $100 \log_2(100) \approx 100 \times 6.6 = 660$ operations.
  *   Knapsack depends on the numeric value of $C$. $100 \times 1,000,000 = 100,000,000$ operations. 
  *   It is "Pseudo-Polynomial" because $C$ is not the *size* of the input array (which is 100), but rather the *magnitude* of a number typed into the system. 
  
  **A4:** **You can fill it out either way!**
  *   Because `A[i][c]` only ever looks at `A[i-1][...]` (the previous row) and columns to the *left* (`c` and `c - s_i`), as long as you process items from $1 \to n$ and capacities from $0 \to C$, the required data will always be waiting for you. (Your textbook on page 130 explicitly states that the algorithm can compute entries column by column, working left to right, bottom to top!).
  
  ---
-