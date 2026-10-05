### 1. Setting Up the Subproblems (Slides 18 & 20)
In the Weighted Independent Set (WIS) problem, we only had one parameter: the prefix of the graph. 
Here, as your slides point out, we parameterize by two indices:
1.  **$i$**: The prefix of available items ($0$ to $n$).
2.  **$c$**: The available knapsack capacity ($0$ to $C$).

To store the answers to these subproblems, we will initialize a 2D array (let's call it `A`). 
*   It will have **$n + 1$ rows** (from $0$ to $n$).
*   It will have **$C + 1$ columns** (from $0$ to $C$).
- ### 2. The Dynamic Programming Algorithm (Slide 21)
  The algorithm systematically fills in this matrix, starting from the top-left (the base cases) and working its way down to the bottom-right.
  
  **The Base Cases:**
  *   If you have **0 items** to choose from ($i=0$), your max value is 0. (Fill the entire top row with 0s).
  *   If your knapsack has **0 capacity** ($c=0$), you can't hold anything. (Fill the entire first column with 0s).
  
  **The Core Loop:**
  ```text
  for i = 1 to n do:
    for c = 0 to C do:
        if s_i > c then:
            // Case 1: Item is too heavy for the current capacity 'c'.
            // Just inherit the optimal value from the row above.
            A[i][c] = A[i-1][c]
        else:
            // Case 2: It fits! Take the max of excluding it vs. including it.
            // Notice how inclusion looks UP one row, and LEFT by 's_i' columns.
            A[i][c] = max(A[i-1][c], A[i-1][c - s_i] + v_i)
  
  return A[n][C] // The final answer!
  ```
- ### 3. Visualizing the Matrix (Slide 22 / Textbook Page 130)
  Let's trace exactly how a cell gets filled in using the example from the slides:
  *   Item 1: $v_1=3, s_1=4$
  *   Item 2: $v_2=2, s_2=3$
  *   Item 3: $v_3=4, s_3=2$
  *   Item 4: $v_4=4, s_4=3$
  *   Max Capacity $C = 6$
  
  Imagine we are currently trying to fill in the cell for **Item 3 ($i=3$) with Capacity 5 ($c=5$)**. 
  We need to calculate `A[3][5]`.
  1.  Item 3 has size 2 and value 4. Does it fit in capacity 5? **Yes.**
  2.  **Option 1 (Exclude it):** Look directly UP one cell. `A[2][5]`. (The value is **3**).
  3.  **Option 2 (Include it):** Look UP one row to `i=2`, and LEFT by the item's size ($5 - 2 = 3$). We look at `A[2][3]`, which has a value of **0**. Add the current item's value ($0 + 4 = \mathbf{4}$).
  4.  **The Max:** $\max(3, 4) = \mathbf{4}$. We write `4` into cell `A[3][5]`.
  
  *You literally just move left-to-right, top-to-bottom, looking "up" and "up-left" to make your decisions!*
- ### 4. Complexity & The "Pseudo-Polynomial" Trap (Slide 23 & Page 129 Footnote)
  
  *   **Time Complexity:** We have a double `for` loop. The outer loop runs $n$ times. The inner loop runs $C$ times. The work inside is an $O(1)$ max calculation. Total Time = **$O(nC)$**.
  *   **Space Complexity:** We allocate a matrix of size $n \times C$. Total Space = **$O(nC)$**.
  
  **The Professor Deep Dive (The Trap!):**
  Is $O(nC)$ a polynomial-time algorithm? 
  **NO.** It is called **Pseudo-Polynomial**.
  
  Why? In computer science, polynomial time means the runtime scales with the *size of the input data* (e.g., how many items there are). 
  Here, the runtime scales based on the **numeric value** of $C$. 
  *   If $n=100$ items and $C=50$ lbs, the matrix is $100 \times 50$ (5,000 cells). Very fast!
  *   If $n=100$ items, but the capacity $C$ is **10 Billion pounds**, the matrix becomes $100 \times 10,000,000,000$ (1 Trillion cells). The program will crash your computer and run out of RAM, *even though you still only have 100 items*.
  *   **Conclusion:** Knapsack is actually an NP-Hard problem. There is no true polynomial-time solution for it, but this DP approach works incredibly well as long as $C$ isn't astronomically large.
  
  ---
- ### Part 3 Practice Questions (Matrix Mechanics)
  
  **Q1: The "Look-Back" Trace**
  You are filling out the DP matrix `A[i][c]` for the Knapsack problem. 
  You are evaluating Item $i=4$, which has a size of **$s_4 = 3$** and a value of **$v_4 = 10$**. The current column capacity is **$c = 8$**. 
  If you choose to *include* Item 4, which exact cell in the matrix do you check to find the remaining optimal value?
  A) `A[3][8]`
  B) `A[4][5]`
  C) `A[3][5]`
  D) `A[3][10]`
  
  **Q2: The Capacity Check**
  In the inner loop of the algorithm, why is the condition `if (s_i > c)` necessary? What would happen if we didn't include this check and simply ran `A[i-1][c - s_i] + v_i`?
  
  **Q3: Space Optimization (Challenge)**
  Notice that to fill in row $i$, the algorithm **only ever looks at row $i-1$**. It never looks at row $i-2$ or $i-3$.
  If you only care about getting the final maximum value (and don't care about reconstructing the actual list of items), how can you drastically improve the **Space Complexity** of this algorithm?
  
  ---
-