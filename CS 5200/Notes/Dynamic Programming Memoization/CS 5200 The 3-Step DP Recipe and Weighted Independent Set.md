### 1. The Core DP Philosophy
In the previous sections (Fibonacci, Combinations), we used **Memoization** (Top-Down recursion with a cache).
The textbook prefers **Tabulation** (Bottom-Up iteration). Instead of using recursion and hoping the cache fills up correctly, we proactively build an array (or table) from the smallest subproblems up to the largest.

**The 3-Step Recipe (Page 118):**
1.  **Identify Subproblems:** Find a small collection of smaller problems that, when solved, help solve the main problem.
2.  **The Recurrence:** Write a formula to solve a "larger" subproblem using the answers to the "smaller" ones.
3.  **The Algorithm:** Systematically fill in an array using the recurrence, from smallest to largest.

---
- ### 2. The Case Study: Weighted Independent Set (WIS) in Paths
  To demonstrate this recipe, the book uses the WIS problem on a simple path graph.
  
  **The Problem (Page 105):**
  *   **Input:** A path graph (nodes connected in a single line) where each vertex $v_i$ has a positive weight $w_i$.
  *   **Definition:** An *Independent Set* is a group of vertices where **no two vertices are adjacent** (connected by an edge).
  *   **Goal:** Find the independent set that has the **maximum total weight**.
  
  *Example Graph:* `(1) --- (4) --- (5) --- (4)`
  *   Valid sets: `{1, 5}` (weight 6), `{4, 4}` (weight 8).
  *   The MWIS (Maximum Weight Independent Set) is `{4, 4}`.
- ### 3. Why Greedy Fails (Page 106)
  A natural greedy approach is to pick the heaviest node first, delete its neighbors, and repeat.
  *   In the example above, Greedy picks `5`. This deletes the two `4`s. The only node left is `1`.
  *   Greedy Output: `{1, 5}` (weight 6).
  *   **Failure:** Greedy missed the optimal `{4, 4}` (weight 8). This proves we need DP!
  
  ---
- ### 4. Applying the 3-Step Recipe to WIS
- #### **Step 1: Identify Subproblems (The Tautology)**
  Let's look at the very last node in the path, $v_n$. In the absolute perfect, optimal solution, there are only two possibilities:
  1.  **$v_n$ is NOT in the optimal set.** If this is true, the optimal solution must be the exact same as the optimal solution for the first $n-1$ nodes.
  2.  **$v_n$ IS in the optimal set.** If we include $v_n$, we **cannot** include its neighbor $v_{n-1}$. Therefore, the rest of the optimal solution must come from the first $n-2$ nodes.
  
  This logic gives us our subproblems! The subproblems are simply the prefixes of the graph ($G_1, G_2, \dots G_n$).
- #### **Step 2: The Recurrence (Page 110)**
  Let $W_i$ be the maximum weight of an independent set for the first $i$ nodes.
  Based on the two cases above, we just take the maximum of both options:
  
  $$ W_i = \max(\underbrace{W_{i-1}}_{\text{Exclude } v_i}, \underbrace{W_{i-2} + w_i}_{\text{Include } v_i}) $$
  
  *(Base Cases: $W_0 = 0$, $W_1 = w_1$)*.
- #### **Step 3: The Algorithm (Page 114)**
  Now we just build an array `A` from $i=0$ up to $n$ using a simple `for` loop.
  
  ```text
  WIS(weights w):
    A[0] = 0
    A[1] = w[1]
    
    for i = 2 to n:
        A[i] = max(A[i-1], A[i-2] + w[i])
        
    return A[n]  // The final answer!
  ```
  
  **Complexity Analysis:**
  *   **Time:** We have a single `for` loop running $n$ times. Inside the loop, we do one `max()` operation (which is $O(1)$). Total Time = **$O(n)$**.
  *   **Space:** We create an array `A` of size $n+1$. Total Space = **$O(n)$**.
  *(We just solved a problem that takes $O(2^n)$ brute-force time in linear time!)*
  
  ---
- ### 5. The Reconstruction Step (Pages 116-118)
  The algorithm above only tells us the **Total Weight** of the optimal set (e.g., 8). It doesn't tell us **which nodes** are actually in the set!
  
  To find the actual nodes, we do a **Reconstruction Pass** (tracing backward through the filled-in array `A`).
  
  **The Logic:**
  Start at the end of the array (`A[n]`). How did that number get there? It came from our recurrence. We just ask which of the two cases "won."
  
  1.  If $A[i-1] \ge A[i-2] + w_i$, then Case 1 won. The node $v_i$ was **excluded**. Move backward one step to $i-1$.
  2.  Otherwise, Case 2 won. The node $v_i$ was **included**. Because it was included, we must skip $v_{i-1}$ and move backward two steps to $i-2$.
  
  ```text
  RECONSTRUCT(A, w):
    i = n
    S = empty set
    while i >= 2:
        if A[i-1] >= A[i-2] + w[i]:
            i = i - 1  // Skip v_i
        else:
            S.add(v_i) // Keep v_i
            i = i - 2  // Skip the neighbor
            
    if i == 1: S.add(v_1)
    return S
  ```
  
  ---
- ### Part 3 Practice Questions (WIS & DP Theory)
  
  **Q1: Tracing the WIS Array**
  Given the path graph with weights: `w =[2, 6, 4, 8, 3]`.
  Fill in the DP array `A` from $i=0$ to $i=5$.
  *   `A[0] = 0`
  *   `A[1] = 2`
  *   `A[2] = ?`
  *   `A[3] = ?`
  *   `A[4] = ?`
  *   `A[5] = ?`
  
  **Q2: Reconstructing the Set**
  Using your filled-in array `A` from Q1, trace the `RECONSTRUCT` algorithm backward from $i=5$.
  Which specific nodes (values) are in the Maximum Weight Independent Set?
  
  **Q3: DP vs. Divide & Conquer**
  According to the textbook (Page 121), what is the key difference regarding subproblems between a standard Divide & Conquer algorithm (like Merge Sort) and a Dynamic Programming algorithm?
  A) DP uses recursion, D&C uses iteration.
  B) D&C subproblems are distinct/independent; DP subproblems overlap and are reused.
  C) D&C solves optimization problems; DP sorts data.
  D) DP only works on arrays of size $2^k$.
  
  **Q4: The "Space Optimization" Trick**
  Look at the `WIS` loop: `A[i] = max(A[i-1], A[i-2] + w[i])`.
  To calculate `A[i]`, we only ever look back at the **previous two** elements in the array.
  If we only want to output the final Max Weight (and don't care about reconstructing the path), how can we optimize the **Space Complexity** of this algorithm from $O(n)$ down to $O(1)$?
  
  ---
-