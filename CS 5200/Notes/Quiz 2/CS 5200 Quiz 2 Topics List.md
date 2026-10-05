### **2. Vector Search, Jaccard Similarity & Advanced LSH**
*   **Jaccard Similarity:** $\frac{\text{Intersection}}{\text{Union}}$. (Used for sets, music preferences, documents).
  *   *Distance:* $1 - \text{Similarity}$.
*   **Weighted Jaccard Similarity:** $\frac{\sum \min(A_i, B_i)}{\sum \max(A_i, B_i)}$. (Used for histograms or feature vectors).
*   **MinHashing:** Compresses massive sparse vectors (One-Hot Encoded shingles) into short dense Signatures.
  *   *The Magic Property:* The probability of a MinHash collision is strictly equal to the Jaccard Similarity.
*   **PolyMinHash (Dart Sampling):** Used for 2D geographic shapes. 
  *   Center the polygons, draw a Global Bounding Box, and throw random "darts" (coordinates). The number of darts thrown until you hit the polygon becomes the hash value.
*   **LSH (Bands & Rows):** 
  *   Divides signatures into $b$ bands of $r$ rows. 
  *   Probability of becoming a candidate pair: $\mathbf{P = 1 - (1 - s^r)^b}$.
  *   **AND logic (rows, $r$):** Reduces False Positives.
  *   **OR logic (bands, $b$):** Reduces False Negatives.

---
- ### **3. Dynamic Programming (Theory & Fundamentals)**
  You must know the difference between DP and Divide & Conquer (D&C).
  *   **D&C:** Subproblems are strictly independent/disjoint (e.g., left half and right half of Merge Sort).
  *   **DP:** Subproblems **overlap**. You do the same math over and over. DP fixes this by saving the answers.
  *   **Memoization (Top-Down):** Using recursion but checking a `cache` array first.
  *   **Tabulation (Bottom-Up):** The textbook method. Using iterative `for` loops to build an array from the base cases up.
  *   **The 3-Step Recipe:** (1) Identify Subproblems, (2) Write the Recurrence, (3) Build the Algorithm.
  
  ---
- ### **4. 1D Dynamic Programming: Weighted Independent Set (WIS)**
  *   **The Problem:** Find the set of non-adjacent nodes in a path graph that yields the maximum weight.
  *   **Why Greedy Fails:** Picking the heaviest node might force you to cross out two highly valuable neighbors, resulting in a sub-optimal total.
  *   **The Recurrence:** You either *Exclude* the node (Case 1) or *Include* the node (Case 2).
    *   **$A[i] = \max(A[i-1], A[i-2] + w_i)$**
  *   **Complexity:** Time is $\mathbf{O(n)}$, Space is $\mathbf{O(n)}$.
  *   **Reconstruction:** To find the actual nodes, trace backward from $A[n]$. If $A[i-1] \ge A[i-2] + w_i$, skip node $i$. Else, keep node $i$ and jump back 2 steps. You **must** keep the $O(n)$ array to do this; you cannot use the $O(1)$ space optimization.
  
  ---
- ### **5. 2D Dynamic Programming: Combinations ($nCr$)**
  *   **The Problem:** Choosing $r$ members from a group of $n$.
  *   **The Recurrence:** A specific person is either IN the group or OUT of the group.
    *   **$C(n, r) = C(n-1, r-1) + C(n-1, r)$**
  *   **Base Cases:** $C(n, n) = 1$ and $C(n, 1) = n$.
  *   **Complexity:** Time and Space are both $\mathbf{O(n \cdot r)}$ using a 2D matrix cache.
  
  ---
- ### **6. 2D Dynamic Programming: 0/1 Knapsack Problem**
  *   **The Problem:** Maximize value without exceeding capacity $C$. You cannot take fractions of items.
  *   **Why Greedy Fails:** Sorting by Value/Weight ratio leaves "empty gaps" in the bag that could have been optimized.
  *   **The 2D State:** Subproblems are parameterized by $i$ (items available) and $c$ (current capacity).
  *   **The Recurrence:** 
    *   If item $i$ is too heavy ($s_i > c$): $V[i, c] = V[i-1, c]$
    *   If it fits: **$V[i, c] = \max(V[i-1, c], \ V[i-1, c-s_i] + v_i)$**
  *   **The Pseudo-Polynomial Trap:** 
    *   Time Complexity is $\mathbf{O(nC)}$. 
    *   It is NOT polynomial time because the runtime scales with the numeric magnitude of the capacity $C$, not just the number of items. (e.g., $C = 10,000,000$ will crash your computer).
  
  ---
- ### **7. 2D Dynamic Programming: Sequence Alignment**
  *   **The Problem:** Aligning two strings (X of length $m$, Y of length $n$) with gaps to minimize a penalty score. (Needleman-Wunsch algorithm).
  *   **The 2D State:** Subproblem $P[i, j]$ is aligning the first $i$ characters of X with the first $j$ characters of Y.
  *   **The Recurrence (3 Cases):**
    *   Diagonal (Match/Mismatch): $P[i-1, j-1] + \alpha_{xy}$
    *   Up (Gap in Y): $P[i-1, j] + \alpha_{gap}$
    *   Left (Gap in X): $P[i, j-1] + \alpha_{gap}$
    *   **$P[i, j] = \min(\text{Diagonal}, \text{Up}, \text{Left})$**
  *   **Complexity:** Time and Space are both $\mathbf{O(mn)}$.
  
  ---
- ---
- ---
- ---
- ### **Practice Exam: 2D Dynamic Programming (Knapsack & Sequence Alignment)**
  
  **Question 1: The Pseudo-Polynomial Trap (Knapsack)**
  You design a 0/1 Knapsack algorithm that builds an $n \times C$ matrix, yielding a time complexity of $O(nC)$. 
  If your input consists of $n = 50$ items, but the knapsack capacity $C$ is $10^{12}$ (1 Trillion), your program will likely crash or run for hours. 
  Why is $O(nC)$ technically **not** considered a true polynomial-time algorithm in theoretical computer science?
  A) Because it requires a 2D array instead of a 1D array.
  B) Because $C$ represents the *numeric magnitude* of the capacity, not the *size of the input data* (it only takes a few bits to type the number 1 Trillion).
  C) Because the items are not sorted beforehand.
  D) Because it is impossible to reconstruct the items.
  
  **Question 2: Manual Matrix Trace (Knapsack Calculation)**
  You are filling out the Knapsack DP matrix `A[i][c]`. 
  You are currently evaluating **Item 3** (Value $v_3 = 5$, Size $s_3 = 3$). 
  The current column capacity you are checking is **$c = 7$**. 
  Looking at the previous row (`i=2`), you see the following values:
  *   `A[2][7] = 8`
  *   `A[2][4] = 6`
  *   `A[2][3] = 4`
  
  Using the Knapsack recurrence relation, what is the exact integer value that will be placed in `A[3][7]`?
  A) 8
  B) 11
  C) 13
  D) 9
  
  **Question 3: The "Too Heavy" Edge Case (Knapsack)**
  You are evaluating **Item 4** (Value $v_4 = 100$, Size $s_4 = 10$). The current column capacity is **$c = 5$**.
  According to the DP recurrence, what value will be placed in `A[4][5]`?
  A) 100
  B) `A[3][5]`
  C) `A[3][0] + 100`
  D) 0
  
  **Question 4: Manual Trace (Knapsack Reconstruction)**
  You have finished building the matrix `A` for 4 items and capacity $C=5$. The final value at `A[4][5]` is **20**.
  You are tracing backward to find which items were stolen.
  *   `A[4][5] = 20`. Size of Item 4 is $s_4 = 2$.
  *   `A[3][5] = 20`. Size of Item 3 is $s_3 = 3$.
  *   `A[3][3] = 15`. 
  According to the reconstruction logic (where ties `A[i-1][c-si]+vi >= A[i-1][c]` favor inclusion), is **Item 4** included in the optimal set? Why or why not?
  
  **Question 5: Fractional vs. 0/1 Knapsack**
  A friend suggests that instead of $O(nC)$ Dynamic Programming, you should just calculate the "Value-to-Weight" ratio of each item, sort them, and greedily put the highest ratio items into the bag first. 
  For which variation of the Knapsack problem does this Greedy algorithm actually yield the mathematically perfect optimal answer?
  A) The 0/1 Knapsack Problem.
  B) The Fractional Knapsack Problem (where you can take 50% of an item).
  C) The Unbounded Knapsack Problem (where you have infinite copies of each item).
  D) It never yields the optimal answer.
  
  **Question 6: Sequence Alignment Base Cases**
  You are using the Needleman-Wunsch algorithm to align String X of length $m=5$ and String Y of length $n=4$. The penalty for inserting a Gap is $\alpha_{gap} = 2$.
  When you initialize the 2D matrix `P[i][j]`, what exact value will be placed in `P[3][0]` (Row 3, Column 0), and what does it represent?
  A) 0 (Because Column 0 means the string is empty).
  B) 6 (Representing the penalty of matching 3 characters of X against 3 gaps).
  C) 2 (Just the standard gap penalty).
  D) Infinity.
  
  **Question 7: Sequence Alignment Recurrence (The 3 Cases)**
  In the Sequence Alignment DP matrix, cell `P[i][j]` takes the **minimum** of three options. Which of the following correctly pairs the geometric direction you look in the matrix with its logical meaning?
  A) Look Diagonal = Match/Mismatch; Look Up/Left = Insert a Gap.
  B) Look Left = Match; Look Up = Mismatch; Look Diagonal = Gap.
  C) Look Up = Match; Look Diagonal = Insert a Gap.
  D) Look Diagonal = Gap; Look Up/Left = Match/Mismatch.
  
  **Question 8: Sequence Alignment Manual Trace**
  You are aligning X="CAT" and Y="CAR".
  You are calculating the cell for the final letters: 'T' vs 'R'. (`i=3, j=3`).
  *   Mismatch penalty = 3. 
  *   Gap penalty = 2.
  *   `P[2][2]` (CA vs CA) = 0
  *   `P[2][3]` (CA vs CAR) = 2
  *   `P[3][2]` (CAT vs CA) = 2
  
  What is the final minimum penalty score placed in `P[3][3]`?
  
  **Question 9: Space Optimization (Knapsack & Sequence Alignment)**
  In both Knapsack and Sequence Alignment, to calculate the values for row `i`, you only ever need to look at row `i-1`. 
  If you optimize the algorithm to only store 2 rows in memory (reducing Space Complexity from $O(nC)$ to $O(C)$), what capability do you permanently lose?
  A) The ability to find the maximum possible value / minimum penalty score.
  B) The ability to process strings/items of different lengths.
  C) The ability to reconstruct the actual items stolen / the actual string alignment.
  D) You lose no capabilities; it is a strict upgrade.
  
  **Question 10: 2D State Space Dependencies**
  In a 2D Dynamic Programming matrix (like Knapsack), you can safely write a program that fills the matrix out row-by-row (top to bottom) OR column-by-column (left to right). 
  Why is this true?
  A) Because the recursive base cases are all 0.
  B) Because cell `[i][c]` only depends on data from `[i-1]` (above it) and `[c - s_i]` (to the left of it), meaning as long as you move forward and down, the data you need is always already computed.
  C) Because the matrix is symmetrical.
  D) Because matrix multiplication is commutative.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: B) Because $C$ represents the numeric magnitude...**
  *   **Explanation:** In theoretical computer science, polynomial time $O(n^k)$ must scale with the *number of bits* required to represent the input. Writing the number `1,000,000,000,000` takes only about 40 bits of data, but it forces your `for` loop to run 1 Trillion times! Because the runtime explodes exponentially relative to the input's bit-size, it is "Pseudo-Polynomial." (This is why 0/1 Knapsack remains NP-Hard).
  
  **Answer 2: B) 11**
  *   **Explanation:** The recurrence is $\max(A[i-1][c], A[i-1][c - s_i] + v_i)$. 
    *   Case 1 (Exclude): Look straight up to `A[2][7]`, which is **8**.
    *   Case 2 (Include): Look up and left by size 3. Go to `A[2][7 - 3]` $\to$ `A[2][4]`. The value there is **6**. Add the current item's value ($v_3 = 5$). $6 + 5 = \mathbf{11}$.
    *   $\max(8, 11) = \mathbf{11}$. 
  
  **Answer 3: B) `A[3][5]`**
  *   **Explanation:** The item has a size of 10, but the current column capacity is only 5. The item is physically too heavy to fit in the bag. Therefore, you are *forced* into Case 1 (Exclude). You simply inherit the optimal value from the row directly above it at the exact same capacity: `A[3][5]`.
  
  **Answer 4: No, Item 4 is EXCLUDED.**
  *   **Explanation:** Look at the values. `A[4][5]` is 20. Look at the row above it: `A[3][5]` is *also* 20. 
    *   Did Item 4 improve our score? No. 
    *   Mathematically, $A[i-1][c - s_i] + v_i$ evaluates to $A[3][3] + v_4 \to 15 + (\text{unknown value, but it didn't beat 20})$.
    *   Because the value didn't change from the row above it, the algorithm knows Item 4 was bypassed. We skip it, capacity remains 5, and we move up to $i=3$. 
  
  **Answer 5: B) The Fractional Knapsack Problem.**
  *   **Explanation:** If you are allowed to take "half a television," the greedy Value/Weight ratio method works perfectly! You just grind the items into dust and pour them in until the bag is exactly full to the brim. It is the restriction of taking *whole* items (0/1) that breaks the Greedy algorithm by leaving empty gaps, mandating Dynamic Programming.
  
  **Answer 6: B) 6**
  *   **Explanation:** Row 3, Column 0 means aligning the first 3 characters of String X against an *empty* String Y (0 characters). The only way to align 3 characters against nothing is to insert 3 gaps in Y. Since each gap costs 2, $3 \times 2 = \mathbf{6}$. 
  
  **Answer 7: A) Look Diagonal = Match/Mismatch; Look Up/Left = Insert a Gap.**
  *   **Explanation:** 
    *   Looking **Diagonal** (`[i-1][j-1]`) means you consumed a character from *both* strings. This implies you either matched them or swallowed the mismatch penalty.
    *   Looking **Up** (`[i-1][j]`) means you consumed a character from X, but Y stayed the same. You inserted a gap in Y.
    *   Looking **Left** (`[i][j-1]`) means you consumed a character from Y, but X stayed the same. You inserted a gap in X.
  
  **Answer 8: 3**
  *   **Explanation:** We calculate all 3 cases and take the `min()`.
    *   **Case 1 (Diagonal/Mismatch):** 'T' vs 'R' is a mismatch. $A[2][2] + \alpha_{mismatch} = 0 + 3 = \mathbf{3}$.
    *   **Case 2 (Up/Gap in Y):** $A[2][3] + \alpha_{gap} = 2 + 2 = \mathbf{4}$.
    *   **Case 3 (Left/Gap in X):** $A[3][2] + \alpha_{gap} = 2 + 2 = \mathbf{4}$.
    *   $\min(3, 4, 4) = \mathbf{3}$.
  
  **Answer 9: C) The ability to reconstruct the actual items stolen / the actual string alignment.**
  *   **Explanation:** If you only keep the last two rows, you have permanently deleted the history of the matrix. When you get to the bottom-right corner and find your max value of 100, you have no way to trace your steps backward to row 0 to see *which decisions* led to that 100. 
  
  **Answer 10: B) Because cell `[i][c]` only depends on data from `[i-1]` (above) and `[c - s_i]` (left).**
  *   **Explanation:** Dynamic Programming is all about topological sorting of dependencies. You cannot calculate a cell until its "prerequisites" are calculated. Since a cell only looks Up and Left, as long as your `for` loops generally progress downward and rightward, the prerequisite cells will magically always be waiting for you, fully calculated!