## **MODULE 1: Vector Search, MinHashing & LSH**
*(Context: Overcoming the $O(N^2)$ Curse of Dimensionality for finding similar items).*

*   **Similarity Metrics:**
  *   **Jaccard Similarity:** Formula: Intersection / Union. (Used for Sets).
  *   **Jaccard Distance:** Formula: $1 - \text{Similarity}$.
  *   **Weighted Jaccard Similarity:** Formula: $\sum \min / \sum \max$. (Used for histograms/feature vectors).
*   **Data Preparation:**
  *   **Shingling ($k$-grams):** Converting documents to sets to preserve text ordering. (Knowing the trade-off of picking a $k$ that is too small vs. too large).
  *   **One-Hot Encoding:** Creating massive, highly sparse boolean vectors based on a global vocabulary.
*   **MinHashing (Compression):**
  *   **The Magic Property:** $Pr[h_{min}(A) = h_{min}(B)] = \text{Jaccard}(A, B)$.
  *   **One-Pass Implementation Trick:** Simulating a random matrix shuffle using universal hash functions ($ax+b \pmod p$) and updating a signature array initialized to `INFINITY` by keeping the minimum hash value seen.
  *   **PolyMinHash (Dart Sampling):** Adapting MinHash for 2D geographic shapes. Centering the shape, drawing a Global MBR, and counting "dart throws" (rejection sampling) until a hit.
*   **Locality Sensitive Hashing (LSH):**
  *   **The Banding Technique:** Dividing a signature into $b$ bands of $r$ rows. 
  *   **Probability Formula:** $P = 1 - (1 - s^r)^b$.
  *   **AND / OR Logic:** 
      *   Rows ($r$) act as an **AND** operation (Reduces False Positives, pushes curve right).
      *   Bands ($b$) act as an **OR** operation (Reduces False Negatives, pushes curve left).
  *   **The S-Curve Threshold:** Formula: $s \approx (1/b)^{1/r}$.
  *   **Formal LSH Definition:** The $(d_1, d_2, p_1, p_2)$-sensitive rules.
  *   **The Final Step:** Exact matching must be performed on the resulting "Candidate Pairs" to weed out False Positives.
*   **Real-World Applications:**
  *   Uber detecting fraudulent trips (replacing $O(N^2)$ brute force).
  *   FBI fingerprint database matching.
  *   Spotify music recommendation intersections.

---
- ## **MODULE 2: Dynamic Programming (DP)**
  *(Context: Solving overlapping subproblems using Top-Down Memoization or Bottom-Up Tabulation).*
  
  *   **Core DP Theory:**
    *   **DP vs. Divide & Conquer:** DP handles *overlapping* subproblems; D&C handles *disjoint* subproblems. DP evaluates multiple choices before committing; D&C blindly commits to a split.
    *   **Optimal Substructure:** Proving that the optimal solution is built from optimal solutions to smaller subproblems.
    *   **The 3-Step Recipe:** (1) Define Subproblems, (2) Write Recurrence, (3) Build the Iterative Algorithm.
    *   **Time Complexity Formula:** $O(f(n) \cdot g(n) + h(n))$ [Subproblems $\times$ Time per subproblem $+$ Reconstruction time].
  *   **Problem 1: Fibonacci**
    *   Naive recursive $O(2^n)$ vs. Memoized $O(n)$.
  *   **Problem 2: Weighted Independent Set (WIS)**
    *   **Why Greedy Fails:** Picking the heaviest node can lock you out of multiple smaller nodes that sum to a higher weight.
    *   **The Recurrence:** $A[i] = \max(A[i-1], \ A[i-2] + w_i)$. (Case 1: Exclude, Case 2: Include).
    *   **Reconstruction:** Tracing backward in $O(n)$ time. (If $A[i-1] \ge A[i-2] + w_i$, skip node $i$. Else, include $i$ and jump back 2 steps).
  *   **Problem 3: Combinations ($nCr$)**
    *   **The Recurrence:** $C(n, r) = C(n-1, r-1) + C(n-1, r)$. (Alice is either IN the group or OUT of the group).
    *   **Base Cases:** $C(n,n) = 1$, $C(n,1) = n$.
  *   **Problem 4: 0/1 Knapsack**
    *   **Why Greedy Fails:** Value/Weight ratio leaves empty space in the bag.
    *   **The 2D Recurrence:** $V[i, c] = \max(V[i-1, c], \ V[i-1, c-s_i] + v_i)$. 
    *   **The Pseudo-Polynomial Trap:** Time is $O(nC)$. It scales with the numeric magnitude of the capacity, not just the data size.
  *   **Problem 5: Sequence Alignment (Needleman-Wunsch)**
    *   **The 3 Cases:** Match/Mismatch (Diagonal), Gap in Y (Up), Gap in X (Left).
    *   **The Recurrence:** $P[i,j] = \min( P[i-1,j-1] + \alpha_{xy}, \ P[i-1,j] + \alpha_{gap}, \ P[i,j-1] + \alpha_{gap} )$.
  
  ---
- ## **MODULE 3: Maximum Flow**
  *(Context: Optimizing throughput in a network using Ford-Fulkerson and Edmonds-Karp).*
  
  *   **Flow Network Rules:**
    *   Capacity Constraint: $0 \le f(u,v) \le c(u,v)$.
    *   Flow Conservation: Flow entering a node = Flow leaving a node.
    *   *Graph Hacks:* Vertex Splitting (to fix antiparallel edges or handle vertex capacities), Supersource/Supersink.
  *   **Ford-Fulkerson Method:**
    *   **Residual Network ($G_f$):** Tracks remaining capacity (Forward edges) and the ability to "Undo" or cancel flow (Backward edges).
    *   **Augmenting Paths:** A path from $s$ to $t$ in the residual network. The "Bottleneck" is the minimum capacity on this path.
  *   **Max-Flow Min-Cut Theorem (Must know the proofs):**
    *   (1) $f$ is a max flow $\iff$ (2) $G_f$ has no augmenting paths $\iff$ (3) $|f| = c(S,T)$ for some cut.
    *   *Key Concept:* Capacity of a cut ignores backward edges. Net Flow across a cut includes backward edges.
  *   **Edmonds-Karp Algorithm:**
    *   **The Fix:** Uses Breadth-First Search (BFS) to always pick the *shortest* augmenting path, preventing infinite loops on pathological graphs.
    *   **Lemma 24.7:** Shortest-path distances from the source strictly increase (or stay the same) monotonically. (Know the contradiction proof).
    *   **Theorem 24.8:** Max augmentations is bounded by $O(VE)$ because edges become "critical", disappear, and require distances to increase by 2 to reappear.
    *   **Complexity:** Time is $O(VE^2)$, Space is $O(V+E)$.
  *   **Applications:** The Escape Problem (Mapping an $N \times N$ grid with vertex splitting to achieve an $O(n^4)$ flow solution).
  
  ---
- ## **MODULE 4: Computational Complexity & Reductions**
  *(Context: Classifying the absolute limits of computation).*
  
  *   **The Classes:**
    *   **P:** Decision problems solvable in polynomial time.
    *   **NP:** Decision problems *verifiable* in polynomial time (given a certificate).
    *   **NP-Hard:** Problems at least as hard as the hardest in NP (Can be optimization problems).
    *   **NP-Complete:** The intersection (Decision problems that are NP-Hard).
    *   *The Trap:* TSP Optimization is NP-Hard, but *not* in NP (you can't verify optimality quickly without checking all other routes).
  *   **Reductions ($A \le_p B$):**
    *   If you know A is hard, and you can translate A to B in polynomial time, B is also hard.
    *   Pipeline: Preprocessor (translate input) $\to$ Subroutine (solve) $\to$ Postprocessor (translate output).
  *   **Specific Reductions to Memorize:**
    *   **Hamiltonian Cycle $\to$ TSP:** Draw real edges with weight 1, fake edges with weight 2. Ask TSP to find a tour of total weight $V$. (If it uses a fake edge, weight becomes $> V$).
    *   **3-SAT $\to$ Independent Set:** Draw $k$ triangles (one for each clause) to force the IS solver to pick at most 1 literal per clause. Draw red conflict edges between $x$ and $\neg x$ to prevent paradoxes. Ask the solver to find a set of size $k$. (Vertex count is $3k$).
  
  ---
- ## **MODULE 5: Approximation Algorithms**
  *(Context: Trading exactness for speed in NP-Hard problems).*
  
  *   **The Goal:** Find a solution in polynomial time that is guaranteed to be within a specific ratio ($\rho$) of the true Optimal answer ($OPT$).
  *   **Vertex Cover (Factor-2 Approximation):**
    *   **The Algorithm:** Arbitrarily pick an edge, add *both* endpoints to the cover, delete all edges touching those two endpoints, repeat.
    *   **The Proof:** The edges picked form a "Maximal Matching" (disjoint edges). The optimal cover $C^*$ must pick at least 1 vertex per disjoint edge. Our algorithm picked exactly 2. Therefore, our cover $|C| = 2|A| \le 2|C^*|$.
  *   **Set Cover (Greedy Approximation):**
    *   **The Algorithm:** Pick the set that covers the most remaining uncovered elements.
    *   **The Ratio:** $\ln |X| + 1$ (or the Harmonic number $H(\max |S|)$). It is not a constant factor!
    *   **The Proof:** Amortized cost analysis. Assign a cost of $1 / (\text{newly covered elements})$ to each element. 
  *   **Minimum Makespan Scheduling (Load Balancing):**
    *   **Graham's Algorithm:** Assign the next job to the least loaded machine. (Factor-2 approximation).
    *   **Lower Bounds:** $OPT$ must be $\ge$ Average Load, and $OPT \ge$ Length of the largest single job.
    *   **LPT (Longest Processing Time first):** Sorting the jobs largest-to-smallest improves the guarantee to Factor 4/3 (1.5).
  *   **Subset Sum (PTAS vs FPTAS):**
    *   **PTAS vs FPTAS:** FPTAS ensures runtime is polynomial in *both* the input size $n$ and the error margin $1/\epsilon$.
    *   **The Trimming Trick:** Given a parameter $\delta$, delete a number $y$ if an existing number $z$ approximates it ($\frac{y}{1+\delta} \le z \le y$). 
    *   **The FPTAS:** Set $\delta = \epsilon / 2n$ so that the compounded error after $n$ merges does not exceed the user's requested error margin $\epsilon$.
  
  ---