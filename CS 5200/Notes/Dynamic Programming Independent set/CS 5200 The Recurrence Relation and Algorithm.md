### 1. The Overlapping Subproblem Flaw (Slides 42–46)
Your professor includes Quiz 16.3 and 16.4 to hammer home a crucial point.
If you draw the recursion tree for `WIS_Recursive(n)`:
*   It calls `WIS_Recursive(n-1)` and `WIS_Recursive(n-2)`.
*   Then `(n-1)` calls `(n-2)` and `(n-3)`.
*   We are calculating the exact same subgraphs over and over again!
*   **Result:** The algorithm runs in **$O(2^n)$** exponential time.

However, Quiz 16.4 asks a brilliant question: How many *distinct* subproblems actually exist?
*   Because we only ever chop off the last node or the last two nodes, the only subproblems we ever look at are the **prefixes** of the graph: $G_0, G_1, G_2 \dots G_n$.
*   There are only **$n+1$** distinct subproblems in the entire universe of this algorithm!
*   Why are we spending exponential time solving $n$ problems? We just need to save the answers!
- ### 2. Approach 1: Top-Down Memoization (Slide 47)
  The quickest fix is to add a "cache" to our recursive function.
  
  ```java
  int[] cache = new int[n + 1]; // Initialize all to -1
  
  int WIS_Memoized(int n) {
    if (n == 0) return 0;
    if (n == 1) return w[1];
  
    // 1. Check the cache FIRST
    if (cache[n] != -1) return cache[n];
  
    // 2. If not found, do the math
    int case1 = WIS_Memoized(n - 1);
    int case2 = WIS_Memoized(n - 2) + w[n];
  
    // 3. Save the answer BEFORE returning
    cache[n] = max(case1, case2);
    return cache[n];
  }
  ```
  *   **Time Complexity:** We only calculate each of the $n$ subproblems exactly once. $O(n)$ time!
  *   **Space Complexity:** The array takes $O(n)$ space, and the recursive call stack takes $O(n)$ space.
- ### 3. Approach 2: Bottom-Up Tabulation (Slides 48–50)
  The textbook (and most professionals) prefer **Bottom-Up Dynamic Programming**.
  If we know we have to solve problems $G_0$ through $G_n$, and we know that $G_i$ depends on $G_{i-1}$ and $G_{i-2}$, why not just start at 0 and count up?
  
  We can build an array `A` iteratively using a simple `for` loop. No recursion required!
  
  **The 3-Step DP Recipe (Slide 49 / Page 118):**
  1.  **Identify Subproblems:** The prefixes of the graph ($i = 0, 1 \dots n$).
  2.  **The Recurrence:** $A[i] = \max(A[i-1], A[i-2] + w_i)$.
  3.  **The Algorithm:**
  
  ```java
  int WIS_BottomUp(int[] w, int n) {
    int[] A = new int[n + 1];
    
    // Base cases
    A[0] = 0;
    A[1] = w[1];
    
    // Iteratively build the array
    for (int i = 2; i <= n; i++) {
        A[i] = max(A[i-1], A[i-2] + w[i]);
    }
    
    return A[n]; // The final answer is sitting at the end!
  }
  ```
  
  *   **Time Complexity:** A single `for` loop running $n$ times. Inside is a single `max()` operation ($O(1)$). Total time = **$O(n)$**.
  *   **Space Complexity:** An array of size $n+1$. Total space = **$O(n)$**.
- ### 4. Visualizing the Array (Slide 48)
  Let's trace the graph from the slide: `(3) — (2) — (1) — (6) — (4) — (5)`
  *(Note: Array indices are 1-based here. $A[0]=0$)*.
  
  *   $A[0] = \mathbf{0}$
  *   $A[1] = \mathbf{3}$ (Base case)
  *   **$i=2$ ($w=2$):** $\max(A[1], A[0] + 2) \to \max(3, 0+2) \to \mathbf{3}$.
  *   **$i=3$ ($w=1$):** $\max(A[2], A[1] + 1) \to \max(3, 3+1) \to \mathbf{4}$.
  *   **$i=4$ ($w=6$):** $\max(A[3], A[2] + 6) \to \max(4, 3+6) \to \mathbf{9}$.
  *   **$i=5$ ($w=4$):** $\max(A[4], A[3] + 4) \to \max(9, 4+4) \to \mathbf{9}$.
  *   **$i=6$ ($w=5$):** $\max(A[5], A[4] + 5) \to \max(9, 9+5) \to \mathbf{14}$.
  
  **Final Array:** `[0, 3, 3, 4, 9, 9, 14]`
  The maximum weight independent set of this graph is **14**.
  
  ---
- ### Part 3 Practice Questions (Tracing DP)
  
  **Q1: Tracing the Array yourself**
  Given the path graph with weights: `[10, 5, 20, 15]`.
  Trace the `WIS_BottomUp` algorithm on scratch paper.
  What are the final values in the array `A` from index 0 to 4?
  
  **Q2: The "Space Optimization" Trick (Professor Deep Dive)**
  Look at the `for` loop: `A[i] = max(A[i-1], A[i-2] + w[i])`.
  To calculate `A[i]`, we only ever need to look at the **previous two** elements in the array. We never look at `A[i-3]` or anything older.
  If the exam asks you to optimize the **Space Complexity** of this algorithm from $O(n)$ down to **$O(1)$**, how would you change the code?
  *(Hint: Do you actually need an array of size $n$, or just two integer variables?)*
  
  **Q3: The Value vs. The Set**
  Our final array gives us the number `14`. But the problem asks us to find the actual *Independent Set* (the list of vertices) that produces that weight. Does our array `A` contain the list of vertices? How do we find them?
  
  ---
-