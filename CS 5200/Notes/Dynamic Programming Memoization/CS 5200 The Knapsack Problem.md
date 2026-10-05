- Here is **Part 4: The Knapsack Problem**.
  
  This section covers **Chapter 16.5 (Pages 123–132)** of your textbook. This is arguably the most famous Dynamic Programming problem in computer science. It forces us to move from a 1D array (like in WIS) to a **2D Matrix** to track our overlapping subproblems.
  
  ---
- # Part 4: The Knapsack Problem
- ### 1. The Core Problem (Page 123)
  You are a thief with a knapsack that can hold a maximum weight capacity of **$C$**.
  You are in a house with **$n$ items**.
  *   Each item $i$ has a **value ($v_i$)** and a **size/weight ($s_i$)**.
  *   **The Goal:** Choose a subset of items that maximizes your total value without exceeding the weight capacity $C$ of your knapsack.
  
  **Why Greedy Fails (Again):**
  You might think: *"Just sort the items by value-to-weight ratio ($v_i / s_i$) and grab the best ones first!"*
  *   **Example:** Knapsack capacity $C = 50$.
    *   Item 1: Value = 60, Size = 10 (Ratio = 6)
    *   Item 2: Value = 100, Size = 20 (Ratio = 5)
    *   Item 3: Value = 120, Size = 30 (Ratio = 4)
  *   **Greedy approach:** Takes Item 1 and Item 2. Total Size = 30, Total Value = **160**. You have 20 capacity left, but nothing fits.
  *   **Optimal approach:** Takes Item 2 and Item 3. Total Size = 50, Total Value = **220**. 
  *   **Conclusion:** The Greedy approach leaves empty space in the knapsack that could have been used more efficiently. We must use Dynamic Programming to explore all combinations efficiently.
  
  ---
- ### 2. Applying the 3-Step Recipe to Knapsack
- #### **Step 1: Identify Subproblems (The Two Dimensions)**
  In the WIS problem, we only had one constraint: the number of items we were allowed to look at ($i$). 
  In Knapsack, we have **two constraints**:
  1.  **$i$:** The number of items we are currently considering (from 1 to $n$).
  2.  **$c$:** The amount of capacity currently available in the knapsack (from 0 to $C$).
  
  Our subproblem $V[i, c]$ asks: *"What is the maximum value I can get using only the first $i$ items, assuming I have a knapsack of capacity $c$?"*
- #### **Step 2: The Recurrence (Page 127)**
  Let's look at the very last item, item $i$. Just like WIS, there are only two possibilities for the optimal solution:
  
  1.  **Case 1 (Item $i$ is excluded):** If we leave item $i$ behind, our total value is exactly the same as the optimal solution for the first $i-1$ items, using the exact same capacity $c$.
    *   Value = **$V[i-1, c]$**
  2.  **Case 2 (Item $i$ is included):** If we take item $i$, we gain its value ($v_i$). BUT, we must reserve space for it in the knapsack. This means the rest of our optimal solution must come from the first $i-1$ items, using a reduced capacity of $c - s_i$.
    *   Value = **$V[i-1, c - s_i] + v_i$**
  
  **The Catch:** We can only choose Case 2 if the item actually fits in the knapsack ($s_i \le c$).
  
  **The Formal Recurrence (Corollary 16.5):**
  $$ V[i, c] = \begin{cases} 
      V[i-1, c] & \text{if } s_i > c \text{ (Item too heavy)} \\
      \max(V[i-1, c], \ V[i-1, c-s_i] + v_i) & \text{if } s_i \le c
   \end{cases} $$
- #### **Step 3: The Algorithm (Page 128)**
  We build a 2D matrix (array of arrays) called `A`.
  *   Rows ($i$): 0 to $n$ (representing the items).
  *   Columns ($c$): 0 to $C$ (representing every possible capacity from 0 up to max).
  
  **The Base Cases:**
  *   If capacity $c = 0$, you can't hold anything. `A[i][0] = 0`.
  *   If you have 0 items ($i = 0$), you can't steal anything. `A[0][c] = 0`.
  
  **The DP Loop:**
  ```text
  KNAPSACK(v, s, C):
    A = new int[n+1][C+1]
    
    // Fill base cases
    for c = 0 to C: A[0][c] = 0
    for i = 0 to n: A[i][0] = 0
    
    // Fill the matrix systematically
    for i = 1 to n:
        for c = 1 to C:
            if s[i] > c:
                A[i][c] = A[i-1][c] // Case 1: Too heavy
            else:
                // Take the max of excluding it vs. including it
                A[i][c] = max(A[i-1][c], A[i-1][c - s[i]] + v[i])
                
    return A[n][C] // The absolute optimal value!
  ```
  
  ---
- ### 3. Complexity Analysis (The "Pseudo-Polynomial" Trap)
  
  *   **Time Complexity:** We have nested loops. The outer loop runs $n$ times. The inner loop runs $C$ times. The math inside the loop is $O(1)$ (a simple `max` comparison). 
    *   Total Time = **$O(nC)$**.
  *   **Space Complexity:** We build an $n \times C$ matrix. 
    *   Total Space = **$O(nC)$**.
  
  **The Professor Deep Dive (Footnote 23, Page 129):**
  Is $O(nC)$ a polynomial time algorithm? **No.** It is called *Pseudo-Polynomial*.
  *   Why? The runtime depends on the *numeric value* of the capacity $C$, not just the *number of items* $n$.
  *   If $n=100$ and $C=50$, it's incredibly fast.
  *   If $n=100$ but the capacity $C$ is 10 Billion pounds, the algorithm will try to create a matrix with 10 Billion columns, crashing your computer and taking forever to run.
  *   This is a famous trap question! Knapsack is an NP-Hard problem. There is no true polynomial-time solution for it.
  
  ---
- ### Part 4 Practice Questions (Knapsack Mechanics)
  
  **Q1: Matrix Tracing (The Heart of the Problem)**
  You are filling out the Knapsack DP matrix `A[i][c]`.
  You are currently on item $i=3$ (Value $v_3 = 4$, Weight $s_3 = 2$).
  The current column capacity is $c=5$.
  You need to calculate `A[3][5]`.
  
  You look at the previous row (item 2) to get your values for the `max` function:
  *   The value of *excluding* the item is at `A[2][5]`, which is **3**.
  *   To find the value of *including* the item, you must look back in row 2 to reserve space. What is the exact index you look up in row 2?
    *   A) `A[2][4]`
    *   B) `A[2][3]`
    *   C) `A[2][2]`
    *   D) `A[2][1]`
  
  **Q2: Matrix Tracing (The Calculation)**
  Following Q1, assume the value at that reserved space index in row 2 is **3**.
  Calculate `A[3][5]`. What is the value placed in the matrix, and does it represent Case 1 (excluding) or Case 2 (including)?
  
  **Q3: The Reconstruction Step**
  Just like WIS, the DP matrix only gives us the final max *value* (in `A[n][C]`). It doesn't tell us which items we stole.
  If you start at `A[n][C]` and want to work backward to reconstruct the list of items, what mathematical check do you perform to see if item $n$ was included?
  *   A) `if A[n][C] > 0:`
  *   B) `if A[n][C] != A[n-1][C]:`
  *   C) `if A[n][C] == A[n][C-1]:`
  *   D) `if s[n] <= C:`
  
  **Q4: Space Optimization**
  In the DP loop, when calculating the values for row `i`, we only ever look at values from row `i-1`.
  If you only care about the final maximum value and don't need to reconstruct the items, how can you radically improve the **Space Complexity** of this algorithm?
  *   A) Change the 2D array to a 1D array of size $C$.
  *   B) Change the 2D array to a 1D array of size $n$.
  *   C) Use recursion instead of iteration.
  *   D) The space complexity cannot be improved.
  
  ---
-