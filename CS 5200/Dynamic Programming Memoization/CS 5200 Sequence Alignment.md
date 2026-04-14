### 1. The Core Problem (Pages 137–139)
We want to compare two strings of text to see how "similar" they are.
*   **String X:** `AGGGCT` (length $m=6$)
*   **String Y:** `AGGCA` (length $n=5$)
*   **The Goal:** We want to align the two strings so they match as closely as possible. Because they are different lengths, we are allowed to insert "gaps" (represented by `-`) into either string to stretch them out.

**The Penalty System:**
Every alignment gets a "score" based on penalties. We want to find the alignment with the **Minimum Total Penalty**.
*   **Match:** 0 penalty (e.g., 'A' lines up with 'A').
*   **Mismatch Penalty ($\alpha_{xy}$):** A penalty for lining up two different characters (e.g., 'C' lines up with 'T').
*   **Gap Penalty ($\alpha_{gap}$):** A penalty for inserting a gap `-`.

*(Note: The textbook notes that for this algorithm, all penalties must be $\ge 0$. You cannot reward a gap).*

---
- ### 2. Applying the 3-Step DP Recipe
- #### **Step 1: Identify Subproblems (The 2D Grid)**
  Just like Knapsack, we have two different variables that can shrink independently: the length of String X ($m$) and the length of String Y ($n$).
  
  Our subproblem $P[i, j]$ asks: *"What is the minimum penalty to align the first $i$ characters of String X with the first $j$ characters of String Y?"*
  *   $i$ ranges from 0 to $m$.
  *   $j$ ranges from 0 to $n$.
- #### **Step 2: The Recurrence (Pages 141–143)**
  We focus on the very last column of any possible alignment between the first $i$ characters of X and the first $j$ characters of Y. There are exactly **three** possibilities for how that final column can look:
  
  1.  **Match/Mismatch (Case 1):** The last character of X ($x_i$) aligns with the last character of Y ($y_j$).
    *   We pay the penalty $\alpha_{x_i y_j}$ (which is 0 if they match).
    *   The rest of the alignment must be the optimal alignment of the remaining $i-1$ characters of X and $j-1$ characters of Y.
    *   Penalty = **$P[i-1, j-1] + \alpha_{x_i y_j}$**
  2.  **Gap in Y (Case 2):** The character $x_i$ aligns with a gap `-`.
    *   We pay the gap penalty $\alpha_{gap}$.
    *   Because $y_j$ wasn't used, the rest of the alignment is the first $i-1$ characters of X aligned with the *full* first $j$ characters of Y.
    *   Penalty = **$P[i-1, j] + \alpha_{gap}$**
  3.  **Gap in X (Case 3):** A gap `-` aligns with the character $y_j$.
    *   We pay the gap penalty $\alpha_{gap}$.
    *   Because $x_i$ wasn't used, the rest of the alignment is the *full* first $i$ characters of X aligned with the first $j-1$ characters of Y.
    *   Penalty = **$P[i, j-1] + \alpha_{gap}$**
  
  **The Formal Recurrence (Corollary 17.2):**
  To find the optimal alignment, we simply calculate all three cases and take the **minimum**:
  $$ P[i, j] = \min \begin{cases} 
      P[i-1, j-1] + \alpha_{x_i y_j} & \text{(Case 1)} \\
      P[i-1, j] + \alpha_{gap} & \text{(Case 2)} \\
      P[i, j-1] + \alpha_{gap} & \text{(Case 3)}
   \end{cases} $$
- #### **Step 3: The Algorithm (Page 145)**
  We build a 2D matrix `A` of size $(m+1) \times (n+1)$.
  
  **The Base Cases (The Edges of the Matrix):**
  If one string is empty (length 0), the only way to align them is to insert gaps for every character in the other string.
  *   `A[i][0] = i * α_gap` (Aligning $i$ characters to an empty string costs $i$ gaps).
  *   `A[0][j] = j * α_gap` (Aligning an empty string to $j$ characters costs $j$ gaps).
  
  **The DP Loop:**
  ```text
  NW_SCORE(X, Y, α_gap, α_mismatch):
    A = new int[m+1][n+1]
    
    // Fill base cases
    for i = 0 to m: A[i][0] = i * α_gap
    for j = 0 to n: A[0][j] = j * α_gap
    
    // Systematically fill the matrix
    for i = 1 to m:
        for j = 1 to n:
            // Calculate mismatch penalty (0 if characters match)
            cost = (X[i] == Y[j]) ? 0 : α_mismatch
            
            // Take the minimum of the three cases
            A[i][j] = min(
                A[i-1][j-1] + cost,     // Diagonal (Match/Mismatch)
                A[i-1][j] + α_gap,      // Up (Gap in Y)
                A[i][j-1] + α_gap       // Left (Gap in X)
            )
            
    return A[m][n] // The final minimum penalty!
  ```
  
  ---
- ### 3. Complexity Analysis (Page 146)
  
  *   **Time Complexity:** We have nested loops traversing a matrix of size $m \times n$. The work inside the loop is a simple $O(1)$ `min()` calculation of three values.
    *   Total Time = **$O(mn)$**.
  *   **Space Complexity:** We build an $(m+1) \times (n+1)$ matrix.
    *   Total Space = **$O(mn)$**.
  *   **Reconstruction Time:** Tracing backward from `A[m][n]` to `A[0][0]` takes at most $m+n$ steps. Total Time = **$O(m+n)$**.
  
  *(Professor Note: Just like Knapsack, if you only need the final score and not the actual string alignment, you can optimize the space to **$O(n)$** by only keeping the "current" row and the "previous" row in memory, since `A[i][j]` only looks back one row at most!)*
  
  ---
- ### Part 5 Practice Questions (Sequence Alignment)
  
  **Q1: Quiz 17.1 (Page 139)**
  String X: `AGTACG`
  String Y: `ACATAG`
  Gap penalty = 1. Mismatch penalty = 2.
  What is the Needleman-Wunsch (NW) score of the following alignment?
  ```text
  A - G T A C G
  A C A T A - G
  ```
  A) 3
  B) 4
  C) 5
  D) 6
  
  **Q2: The Matrix Trace**
  You are filling out the DP matrix `A[i][j]` for strings `X="CAT"` and `Y="CAR"`.
  Gap penalty = 1. Mismatch penalty = 2.
  You are currently evaluating `i=3` ('T') and `j=3` ('R').
  The values in the matrix are:
  *   `A[2][2]` (Diagonal) = 0
  *   `A[2][3]` (Up) = 1
  *   `A[3][2]` (Left) = 1
  
  What value will be placed in `A[3][3]`? Which of the three cases "won"?
  
  **Q3: The Edge Case**
  According to the base cases on Page 145, if String X is "HELLO" ($m=5$) and String Y is empty ($n=0$), and the gap penalty is 2, what is the value of `A[5][0]`?
  
  **Q4: Edit Distance (Page 100)**
  Your textbook (in an earlier chapter) defines "Edit Distance" as the minimum number of insertions and deletions to convert String X to String Y. 
  How can you use the Sequence Alignment algorithm to perfectly calculate Edit Distance?
  A) Set Match = 0, Mismatch = $\infty$, Gap = 1.
  B) Set Match = 1, Mismatch = 1, Gap = 1.
  C) Set Match = 0, Mismatch = 1, Gap = 1.
  D) You cannot use Sequence Alignment for Edit Distance.
  
  ---
-