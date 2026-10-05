# **CS 5200: Practice Quiz 2**
**Topics:** Stable Marriage, Vector Search / LSH, Dynamic Programming
- ### **Section 1: Stable Marriage Problem**
  **Question 1: Algorithmic Guarantees**
  In the Gale-Shapley algorithm where the Men propose to the Women, the algorithm is mathematically guaranteed to be **Proposer-Optimal** and **Accepter-Pessimal**. 
  Briefly explain what "Accepter-Pessimal" means in the context of the Women's final matches. Does it mean every woman gets her last choice?
  
  **Question 2: Tracing Gale-Shapley**
  Trace the Gale-Shapley algorithm (Men propose) for the following 2x2 setup.
  *   **Men's Preferences:**
    *   M1 prefers: W2 > W1
    *   M2 prefers: W1 > W2
  *   **Women's Preferences:**
    *   W1 prefers: M1 > M2
    *   W2 prefers: M2 > M1
  
  Write out the sequence of proposals, rejections, and the final matching. Is there a "Rogue Couple" in your final matching?
  
  ---
- ### **Section 2: Jaccard, MinHashing, and LSH**
  **Question 3: Jaccard Similarity and MinHash Expectation**
  User A's set of liked movies is: `{Matrix, Inception, Dune, Alien}`.
  User B's set of liked movies is: `{Dune, Alien, Terminator}`.
  1. Calculate the exact Jaccard Similarity between User A and User B.
  2. If you compress these sets using MinHashing with **350 independent hash functions**, exactly how many integers in User A's signature do you expect to perfectly match the integers in User B's signature?
  
  **Question 4: Tuning the S-Curve (LSH)**
  You are implementing an LSH system for document similarity. Your current configuration divides signatures into $b = 20$ bands of $r = 4$ rows. 
  You notice that the system has an unacceptably high **False Negative** rate (it is missing many genuinely similar documents).
  To fix this, you decide to change the configuration to **$b = 40$ bands of $r = 2$ rows**. 
  Explain mathematically and conceptually why this change will fix your False Negative problem, and state the trade-off (what new problem might you introduce?).
  
  ---
- ### **Section 3: Dynamic Programming (Theory & WIS)**
  **Question 5: The "Distinct Subproblems" Concept**
  If you run a naive, un-memoized recursive algorithm to find the Maximum Weight Independent Set (WIS) on a path graph of $N$ nodes, the time complexity is $O(2^N)$. 
  However, using Dynamic Programming reduces this to $O(N)$. 
  What specific property regarding the **number of distinct subproblems** allows DP to achieve this massive speedup?
  
  **Question 6: WIS Manual Trace & Reconstruction**
  You are given a path graph with the following node weights: `w = [5, 8, 4, 3, 9]`.
  *(Assume 1-based indexing where $w_1 = 5$. $A[0] = 0$.)*
  1. Trace the Bottom-Up DP algorithm to fill in the array `A` from $A[1]$ to $A[5]$.
  2. Using your completed array `A`, trace backward from $A[5]$ to reconstruct the optimal solution. Which specific nodes are included in the Maximum Weight Independent Set?
  
  ---
- ### **Section 4: Dynamic Programming (Combinations & Knapsack)**
  **Question 7: Combinations ($nCr$) State Space**
  In the DP algorithm for Combinations, the recurrence relation is:
  $C(n, r) = C(n-1, r-1) + C(n-1, r)$.
  If you are calculating `C(10, 4)` using a Bottom-Up 2D Tabulation approach, what is the exact **Space Complexity** (auxiliary memory) required to build the DP cache? Why?
  
  **Question 8: The Pseudo-Polynomial Trap**
  The time complexity for the 0/1 Knapsack DP algorithm is $O(nC)$. 
  If you have $n = 100$ items, and the capacity of the knapsack is $C = 1,000,000,000$ (1 Billion), the algorithm will likely crash your computer. 
  Explain why $O(nC)$ is considered **Pseudo-Polynomial** rather than strictly Polynomial time.
  
  **Question 9: Knapsack Manual Trace**
  You are filling out the 0/1 Knapsack DP matrix `A[i][c]`. 
  You are currently on **Item 3** (Value $v_3 = 6$, Size $s_3 = 4$). 
  The current column capacity is **$c = 7$**. 
  You look at the previous row (`i = 2`) and see the following values:
  *   `A[2][7] = 10`
  *   `A[2][4] = 5`
  *   `A[2][3] = 4`
  
  Using the Knapsack recurrence relation, calculate the exact integer value that will be placed in `A[3][7]`. Did Case 1 (Exclude) or Case 2 (Include) win?
  
  ---
- ### **Section 5: Dynamic Programming (Sequence Alignment)**
  **Question 10: Sequence Alignment Matrix Trace**
  You are using the Needleman-Wunsch algorithm to align String X and String Y.
  *   Mismatch Penalty = 3
  *   Gap Penalty = 2
  *   Match = 0
  
  You are currently comparing the character **'A'** from String X against the character **'T'** from String Y. 
  The adjacent cells in your DP matrix are:
  *   Diagonal (`A[i-1][j-1]`) = 4
  *   Up (`A[i-1][j]`) = 7
  *   Left (`A[i][j-1]`) = 5
  
  Calculate the penalty score for all three recurrence cases. What is the final minimum penalty score placed in the current cell `A[i][j]`?
  
  ---
  ---
  ---
- # **Answer Key & Deep-Dive Explanations**
- ### **Section 1: Stable Marriage Problem**
  **A1:** "Accepter-Pessimal" does *not* mean every woman gets her absolute last choice. It means that out of all the mathematically possible *stable* matchings that could exist for the entire group, the Gale-Shapley algorithm (when men propose) will assign every woman the worst valid partner she could possibly have without breaking the stability of the system. 
  
  **A2:** 
  *   **Round 1:** M1 proposes to his top choice (W2). W2 is free, says yes. M2 proposes to his top choice (W1). W1 is free, says yes.
  *   **Final Matching:** (M1, W2) and (M2, W1). 
  *   **Rogue Couple Check:** Is there a Rogue Couple? Let's check M1 and W1. M1 prefers W2 (his current match), so he would never run away with W1. Let's check M2 and W2. M2 prefers W1 (his current match), so he would never run away with W2. 
  *   **Result:** The matching is stable. No Rogue Couples exist.
- ### **Section 2: Jaccard, MinHashing, and LSH**
  **A3:** 
  1.  **Jaccard Similarity:** Intersection = `{Dune, Alien}` (Size 2). Union = `{Matrix, Inception, Dune, Alien, Terminator}` (Size 5). Similarity = **$2/5$ (or 0.4)**.
  2.  **MinHash Expectation:** The fundamental property of MinHashing is that $Pr[h(A) = h(B)] = \text{Jaccard}(A, B)$. Since the similarity is 0.4, each of the 350 hash functions has a 40% chance of colliding. $350 \times 0.4 = \mathbf{140 \text{ matches}}$.
  
  **A4:** 
  *   **Mathematical/Conceptual fix:** By decreasing the rows ($r=4 \to r=2$), you are making the "AND" condition much easier to pass. By increasing the bands ($b=20 \to b=40$), you are increasing the "OR" condition, giving documents twice as many chances to find a lucky bucket. This shifts the S-Curve to the **LEFT**, lowering the similarity threshold.
  *   **The Trade-off:** By making the filter so forgiving, you have successfully fixed the False Negatives, but you will now introduce a massive amount of **False Positives** (unrelated documents getting flagged as candidates), which will cost you heavy CPU time in the exact-matching verification phase.
- ### **Section 3: Dynamic Programming (Theory & WIS)**
  **A5:** In WIS, the problem size shrinks by either 1 node or 2 nodes. Because we only ever chop nodes off the end of the graph, the only subproblems that ever exist are the **prefixes** of the graph ($G_0, G_1, \dots, G_N$). Therefore, there are exactly **$N+1$ distinct subproblems**. The naive $O(2^N)$ algorithm evaluates these same $N+1$ prefixes millions of times. DP just solves them once and caches them.
  
  **A6:** 
  1.  **Array Trace:** `w =[5, 8, 4, 3, 9]`
    *   `A[0] = 0`
    *   `A[1] = 5`
    *   `A[2] = max(A[1], A[0] + w_2) = max(5, 0 + 8) = 8`
    *   `A[3] = max(A[2], A[1] + w_3) = max(8, 5 + 4) = 9`
    *   `A[4] = max(A[3], A[2] + w_4) = max(9, 8 + 3) = 11`
    *   `A[5] = max(A[4], A[3] + w_5) = max(11, 9 + 9) = 18`
    *   **Array A:** `[0, 5, 8, 9, 11, 18]`
  2.  **Reconstruction:** Start at $i=5$ (`A[5]=18`). 
    *   Does $18$ come from $A[4]$ (11) or $A[3] + w_5$ ($9+9=18$)? Case 2 wins. **Include Node 5**. Jump to $i=3$.
    *   At $i=3$ (`A[3]=9`). Does $9$ come from $A[2]$ (8) or $A[1] + w_3$ ($5+4=9$)? Case 2 wins. **Include Node 3**. Jump to $i=1$.
    *   At $i=1$. Base case. **Include Node 1**.
    *   **Final Set:** **{Node 1, Node 3, Node 5}**. (Check weights: $5 + 4 + 9 = 18$).
- ### **Section 4: Dynamic Programming (Combinations & Knapsack)**
  **A7:** 
  *   **Space Complexity:** $O(n \cdot r)$. 
  *   **Why:** Because the recurrence depends on two changing variables ($n$ and $r$), you must construct a 2D matrix to hold the cached answers. The matrix will have $10+1$ rows and $4+1$ columns, resulting in $O(n \cdot r)$ memory cells.
  
  **A8:** 
  *   A true polynomial-time algorithm must scale based on the *number of bits* required to represent the input. Writing the number $1,000,000,000$ takes only about 30 bits of memory, but it forces the algorithm's inner `for` loop to execute 1 Billion times. Because the runtime explodes exponentially relative to the size of the *bits* of the input, it is Pseudo-Polynomial.
  
  **A9:**
  *   **Recurrence:** $\max(A[i-1][c], \ A[i-1][c - s_i] + v_i)$
  *   **Case 1 (Exclude):** Look straight up to `A[2][7]`, which is **10**.
  *   **Case 2 (Include):** Item size is 4. Look up and left to `A[2][7 - 4]` $\to$ `A[2][3]`. The value there is **4**. Add the current item's value ($v_3 = 6$). $4 + 6 = \mathbf{10}$.
  *   **Max:** $\max(10, 10) = \mathbf{10}$. 
  *   *Note on Ties:* The value placed is **10**. In the standard reconstruction algorithm (which uses `>=`), Case 2 (Include) wins the tie.
- ### **Section 5: Dynamic Programming (Sequence Alignment)**
  **A10:**
  *   **Case 1 (Match/Mismatch - Diagonal):** 'A' vs 'T' is a Mismatch. Cost = Mismatch Penalty (3). Diagonal value is 4. $4 + 3 = \mathbf{7}$.
  *   **Case 2 (Gap in Y - Up):** Value is 7. Gap Penalty is 2. $7 + 2 = \mathbf{9}$.
  *   **Case 3 (Gap in X - Left):** Value is 5. Gap Penalty is 2. $5 + 2 = \mathbf{7}$.
  *   **Result:** $\min(7, 9, 7) = \mathbf{7}$. The final score placed in the cell is 7.
- ---
- ---
- ---
- # **CS 5200: Practice Quiz 3**
  **Topics:** LSH/PolyMinHash, MinHash Implementation, DP Theory, Boundary Cases
- ### **Section 1: Vector Search & PolyMinHash**
  **Question 1: Shingling Parameter Tuning**
  You are building an LSH system. You test it on a database of short Tweets, and then on a database of 50-page academic research papers. 
  Why must the shingle size ($k$) be significantly larger for the research papers than for the Tweets? What catastrophic failure occurs if you use $k=2$ (2-character shingles) for the research papers?
  
  **Question 2: PolyMinHash (Dart Sampling) Centering**
  In the PolyMinHash algorithm used to find the Jaccard Similarity of 2D geographic shapes, the very first step is to shift the shapes so their centroids sit exactly at `(0,0)`. 
  What would happen to the estimated Jaccard Similarity if you skipped this step and compared a shape in New York directly against an identical shape in London?
  
  **Question 3: Rejection Sampling Efficiency**
  You are using PolyMinHash on two polygons. You draw a Global Minimum Bounding Rectangle (MBR) that has an area of **10,000**. 
  The two polygons have an overlapping intersection area of **50** and a combined union area of **200**. 
  1. What is the mathematical probability that a random "dart" thrown inside the MBR hits the *union* of the polygons?
  2. If a dart *does* successfully hit the union, what is the exact probability that it gives the two polygons the same hash value (i.e., it hits the intersection)?
  
  ---
- ### **Section 2: LSH & MinHash Implementation**
  **Question 4: The 1-Pass MinHash Simulation**
  In a production environment, we do not physically shuffle a Boolean matrix of 1 million rows. We simulate it in a single pass. 
  You are generating a signature of length 2 for a document. Your signature array is currently initialized to: `SIG =[15, 8]`.
  The document has a `1` at row index **`R = 4`**. 
  You run $R=4$ through your two hash functions:
  *   $h_1(4) = (5 \times 4 + 2) \pmod{17} = 5$
  *   $h_2(4) = (3 \times 4 + 1) \pmod{17} = 13$
  
  What is the updated `SIG` array after processing Row 4? Briefly explain the logic of this update.
  
  **Question 5: One-Hot Encoded Sparsity**
  Your global vocabulary contains $250,000$ unique shingles. You process a document that contains exactly $400$ unique shingles. 
  If you create a One-Hot Encoded boolean vector for this document, exactly how many `0`s (zeros) will be in the vector?
  
  ---
- ### **Section 3: Dynamic Programming Edge Cases**
  **Question 6: Combinations ($nCr$) Matrix Size**
  You are writing the Bottom-Up DP algorithm to calculate `C(group, members)`. You are asked to compute `C(8, 3)`. 
  1. What are the exact dimensions (Rows $\times$ Columns) of the 2D cache matrix you must allocate to solve this without out-of-bounds errors? 
  2. According to the base cases, what exact integer value is pre-filled at `cache[5][5]`?
  
  **Question 7: Sequence Alignment to Edit Distance**
  Your textbook notes that "Edit Distance" (the minimum number of insertions, deletions, or substitutions to change String X into String Y) is a specific variation of Sequence Alignment. 
  To force the Needleman-Wunsch DP algorithm to perfectly calculate the standard Edit Distance, exactly what integer values must you assign to the following penalties?
  *   Match Penalty = ?
  *   Mismatch Penalty ($\alpha_{xy}$) = ?
  *   Gap Penalty ($\alpha_{gap}$) = ?
  
  **Question 8: WIS Reconstruction Tie-Breaking**
  While tracing backward to reconstruct the Maximum Weight Independent Set, you are at node $i=5$. 
  You evaluate the condition: `if A[i-1] >= A[i-2] + w[i]`. 
  Assume `A[4] = 20` and `A[3] + w[5] = 20`. 
  According to this specific `if` statement, does the algorithm **Include** or **Exclude** node 5? Does this choice yield a mathematically optimal Independent Set?
  
  **Question 9: The Dependency Graph (DP vs. D&C)**
  If you draw the subproblem dependency graph for a Divide and Conquer algorithm (like Merge Sort), it forms a strict **Tree**. 
  If you draw the subproblem dependency graph for a Dynamic Programming algorithm (like Knapsack or Fibonacci), what specific graph structure does it form? 
  A) A strict Tree.
  B) A Directed Acyclic Graph (DAG).
  C) A cyclic graph.
  D) A disjoint set.
  
  **Question 10: The "Knapsack" Capacity Zero Edge Case**
  In the 0/1 Knapsack DP loop, what happens if the weight of an item is $s_i = 0$ and its value is $v_i = 10$? 
  Assume the current capacity is $c = 5$. 
  According to the recurrence relation $V[i,c] = \max(V[i-1, c], \ V[i-1, c-s_i] + v_i)$, what exactly will the algorithm evaluate, and what is the logical outcome?
  
  ---
  ---
- # **Answer Key & Deep-Dive Explanations**
- ### **Section 1: Vector Search & PolyMinHash**
  **A1:** 
  *   **Why larger $k$:** Long documents share a massive amount of common words ("the", "and", "is"). A larger $k$ (e.g., $k=9$) groups words into longer, unique phrases, capturing the actual *semantic structure* of the document.
  *   **The $k=2$ Failure:** If you use 2-character shingles on 50-page papers, every single paper will contain almost every possible 2-letter combination (aa, ab, ac...). Their boolean vectors will be nearly identical (all `1`s), resulting in a Jaccard Similarity near 1.0 for completely unrelated papers.
  
  **A2:** 
  *   **Result:** The Jaccard Similarity would be calculated as **0**. 
  *   **Why:** Because the "dartboard" is based on geometric coordinates. If Shape A is at coordinates `(1000, 1000)` and Shape B is at `(-500, -500)`, they will literally never overlap on the grid. The intersection area is 0. Centering them at `(0,0)` strips away their "location" data so you are strictly comparing their "shape".
  
  **A3:** 
  1.  **Hit the Union:** $200 / 10,000 = \mathbf{0.02}$ (or 2%).
  2.  **Hit the Intersection (Given it hit the union):** This is exactly the Jaccard Similarity! Intersection / Union = $50 / 200 = \mathbf{0.25}$ (or 25%). *(This proves why the number of darts it takes to hit either shape serves as a perfect hash for similarity).*
- ### **Section 2: LSH & MinHash Implementation**
  **A4:** 
  *   **Updated SIG:** `[5, 8]`
  *   **Explanation:** The one-pass algorithm keeps the **minimum** hash value seen so far for each slot. 
    *   For slot 1: $\min(15, 5) = \mathbf{5}$. (Replaced).
    *   For slot 2: $\min(8, 13) = \mathbf{8}$. (Kept original).
    *   This perfectly simulates scanning a shuffled matrix and stopping at the very first `1` you encounter!
  
  **A5:** 
  *   **Math:** $250,000 - 400 = \mathbf{249,600}$ zeros. 
  *   **Explanation:** The One-Hot vector's length is strictly equal to the global vocabulary size. Since only 400 shingles are present (marked as `1`), the remaining 249,600 slots must be `0`. This illustrates extreme sparsity.
- ### **Section 3: Dynamic Programming Edge Cases**
  **A6:** 
  1.  **Dimensions:** `9 x 4` (or `[8+1][3+1]`). Because we need to access indices from 0 up to 8 (for the group) and 0 up to 3 (for the members), the arrays must be sized $N+1$ and $R+1$.
  2.  **Base Case:** `cache[5][5] = 1`. The base case is when `group == members`, meaning you are picking 5 people from a group of 5. There is exactly **1** way to do that.
  
  **A7:** 
  *   **Match Penalty = 0** (If the letters are the same, no edit is required. Cost is 0).
  *   **Mismatch Penalty ($\alpha_{xy}$) = 1** (If the letters differ, it represents a "Substitution" edit. Cost is 1 edit).
  *   **Gap Penalty ($\alpha_{gap}$) = 1** (Inserting a gap in either string represents an "Insertion" or "Deletion" edit. Cost is 1 edit).
  
  **A8:** 
  *   **Action:** The code evaluates `if 20 >= 20`, which is **True**. Because it is True, the `if` block executes, meaning Case 1 won. The algorithm **Excludes** node 5.
  *   **Optimality:** **Yes**, it is still mathematically optimal. Since both choices resulted in a max weight of 20, there are multiple valid Maximum Weight Independent Sets in this graph. Excluding the node is just as correct as including it.
  
  **A9: B) A Directed Acyclic Graph (DAG).**
  *   **Explanation:** In Divide and Conquer, subproblems never touch, forming a tree. In Dynamic Programming, multiple higher-level problems rely on the exact same lower-level problem (e.g., `fib(4)` and `fib(3)` both point to `fib(2)`). This merging of paths forms a DAG.
  
  **A10:** 
  *   **Evaluation:** It evaluates $\max(V[i-1, 5], \ V[i-1, 5 - 0] + 10) \implies \max(V[i-1, 5], \ V[i-1, 5] + 10)$.
  *   **Outcome:** Since adding 10 to the exact same subproblem will always be larger, Case 2 wins. The item is **Included**. 
  *   **Logic:** A zero-weight item with positive value is essentially "free points." It takes up no space in the bag, so the DP algorithm will always grab it.