### **Practice Exam: TSP Approximation Algorithm**

**Question 1: The Preorder Trace**
You run the `APPROX-TSP-TOUR` algorithm on a map of cities. The algorithm generates a Minimum Spanning Tree (MST) with the following structure:
*   City A is the root.
*   City A connects to City B and City C.
*   City B connects to City D and City E.
*   City C has no children.
Assuming the DFS/Preorder walk visits children in alphabetical order, what is the exact final Hamiltonian cycle returned by the algorithm?
A) A $\to$ B $\to$ C $\to$ D $\to$ E $\to$ A
B) A $\to$ B $\to$ D $\to$ E $\to$ C $\to$ A
C) A $\to$ D $\to$ E $\to$ B $\to$ C $\to$ A
D) A $\to$ B $\to$ D $\to$ A $\to$ E $\to$ A $\to$ C $\to$ A

**Question 2: The MST Lower Bound**
In the proof of the 2-approximation guarantee, the very first mathematical step establishes that $c(T) \le c(H^*)$ (The cost of the MST is $\le$ the cost of the optimal tour). 
What is the fundamental graph-theory reason this statement is absolutely true?
A) Because the MST uses the triangle inequality.
B) Because deleting any single edge from the optimal cycle $H^*$ creates a valid spanning tree, which must cost at least as much as the *Minimum* Spanning Tree.
C) Because the MST visits every vertex twice, whereas the optimal tour visits them once.
D) Because the optimal tour is NP-Hard.

**Question 3: The "Full Walk" Weight**
Let $W$ be the "Full Walk" that traverses the MST up and down every branch before any shortcuts are taken. 
If the total weight of the Minimum Spanning Tree is $c(T) = 400$, what is the exact cost of the Full Walk $c(W)$?
A) 400
B) 600
C) 800
D) 1200

**Question 4: The Triangle Inequality Trap**
Suppose you are analyzing a graph representing commercial airline flights. 
*   A direct flight from New York to Los Angeles costs \$800. 
*   A flight from New York to Chicago costs \$200, and a flight from Chicago to Los Angeles costs \$300.
If you apply the `APPROX-TSP-TOUR` algorithm to this graph, what will happen?
A) The algorithm will output a valid 2-approximation.
B) The algorithm will crash.
C) The algorithm will shortcut from New York directly to Los Angeles, massively *increasing* the cost of the tour and destroying the 2-approximation guarantee.
D) The algorithm will automatically route through Chicago.

**Question 5: Bounding the Optimal Tour**
You run the 2-approximation algorithm on a graph. The total cost of the MST is $c(T) = 50$. The final Hamiltonian cycle produced by your algorithm has a cost of $c(H) = 85$. 
Based on the mathematical proofs, what is the tightest possible range for the cost of the absolute perfect optimal tour, $c(H^*)$?
A) $50 \le c(H^*) \le 85$
B) $42.5 \le c(H^*) \le 100$
C) $50 \le c(H^*) \le 100$
D) $85 \le c(H^*) \le 170$

**Question 6: The Root Node Selection**
Step 1 of the algorithm states: "select a vertex $r \in V$ to be a 'root' vertex."
If two different students run the exact same `APPROX-TSP-TOUR` algorithm on the exact same graph, but Student 1 picks Vertex A as the root and Student 2 picks Vertex Z as the root, which of the following is true?
A) The 2-approximation mathematical guarantee holds for both students.
B) Student 2's algorithm will fail to produce a Hamiltonian Cycle.
C) They will generate completely different Minimum Spanning Trees.
D) The algorithm requires the root to be the vertex with the lowest degree.

**Question 7: The Shortcut Inequality**
In the final step of the mathematical proof, we state that $c(H) \le c(W)$ (The cost of the shortcut Hamiltonian cycle is $\le$ the cost of the Full Walk). 
Why is this mathematically guaranteed?
A) Because the shortcut skips vertices, meaning it traverses fewer physical edges, and the Triangle Inequality guarantees that a direct edge is never more expensive than an indirect detour.
B) Because the MST is the absolute minimum possible tree.
C) Because we divide the cost of the Full Walk by 2.
D) Because Hamiltonian Cycles do not allow crossing edges.

**Question 8: General TSP vs. Metric TSP**
Why is the Triangle Inequality requirement so crucial for Approximation Algorithms? 
*(Hint: Think back to the Hamiltonian Cycle $\to$ TSP reduction from the previous chapter).*
A) Without the Triangle Inequality, the problem becomes polynomial time (P).
B) Without the Triangle Inequality, the general TSP problem is so hard that finding *any* constant-factor approximation is NP-Hard.
C) Without it, Prim's Algorithm fails to find an MST.
D) It is just an optimization; the algorithm works fine without it.

**Question 9: The Perfect Optimality Trap**
You run the `APPROX-TSP-TOUR` algorithm, and it returns a tour with a cost of 100. You magically discover that the true optimal tour also costs exactly 100. 
A classmate says, *"The algorithm is called a 2-Approximation, so it is mathematically forced to return a cost of 200 if the optimal is 100. The algorithm must be broken."*
Why is your classmate wrong?
A) The algorithm is actually a 1-Approximation.
B) The term "2-Approximation" is an *upper bound*. It guarantees the answer will be $\le 2 \times OPT$, but it can absolutely find the perfect optimal answer!
C) The optimal answer must be 50.
D) The graph did not satisfy the Triangle Inequality.

**Question 10: Eulerian Tour Validation**
The Full Walk $W$ on the Minimum Spanning Tree is essentially an Eulerian Tour (a path that traverses every edge) on a modified graph. How does the algorithm conceptually modify the MST so that an Eulerian Tour is mathematically possible?
A) It adds fake edges with a weight of 2 to all disconnected vertices.
B) It conceptually doubles every edge in the MST, creating a graph where every vertex has an even degree.
C) It converts the undirected edges into directed edges.
D) It deletes the leaf nodes.

---
---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: B) A $\to$ B $\to$ D $\to$ E $\to$ C $\to$ A**
  *   **The Logic:** A Preorder walk visits a node the *first time* it encounters it, then dives down the left subtree, then the right subtree. 
    1. Start at **A**.
    2. Go to left child **B**.
    3. Go to B's left child **D**.
    4. Backtrack to B (ignore, already visited). Go to B's right child **E**.
    5. Backtrack to B, backtrack to A (ignore). Go to A's right child **C**.
    6. Return to start **A** to close the cycle. 
    *(Note: Option D is the "Full Walk", which visits nodes multiple times. The shortcutting step specifically deletes the duplicates to create Option B).*
  
  **Answer 2: B) Because deleting any single edge from the optimal cycle creates a valid spanning tree...**
  *   **The Logic:** This is the bedrock of the entire proof! An optimal tour $H^*$ is a giant loop connecting all $V$ vertices. If you snip any wire in that loop, it straightens out into a path that still touches every vertex. That path is a Spanning Tree. Because the MST is the *cheapest possible* spanning tree in existence, it must be cheaper than (or equal to) the snipped optimal tour. 
  
  **Answer 3: C) 800**
  *   **The Logic:** The Full Walk traces down into a branch, hits a dead end, and traces *back up* the exact same branch. This means every single edge in the MST is traversed exactly twice. Therefore, $c(W) = 2 \times c(T) = 2 \times 400 = \mathbf{800}$. 
  
  **Answer 4: C) The algorithm will shortcut... massively increasing the cost...**
  *   **The Logic:** The Triangle Inequality requires $c(NY, LA) \le c(NY, CHI) + c(CHI, LA)$. Here, $800 \le 200 + 300 \implies 800 \le 500$, which is **FALSE**.
  *   In the algorithm's shortcut phase, it deletes the "layover" (Chicago) to skip a previously visited city. By shortcutting directly from NY to LA, it pays \$800 instead of \$500. This blows up the cost and destroys the factor-2 mathematical guarantee.
  
  **Answer 5: A) $50 \le c(H^*) \le 85$**
  *   **The Logic:** We have two hard mathematical boundaries:
    1.  **Lower Bound:** The absolute optimal tour $H^*$ must be greater than or equal to the MST. So, $c(H^*) \ge 50$.
    2.  **Upper Bound:** The algorithm produced a valid tour $H$ that costs 85. Since $H^*$ is defined as the *absolute best possible tour*, it cannot possibly cost *more* than a tour we literally just found. So, $c(H^*) \le 85$.
    *(Note: The 2-approximation guarantees $c(H) \le 2c(H^*) \implies 85 \le 2c(H^*) \implies 42.5 \le c(H^*)$, but $50$ is a strictly tighter and better lower bound because we know the MST cost!)*
  
  **Answer 6: A) The 2-approximation mathematical guarantee holds for both students.**
  *   **The Logic:** Prim's algorithm finds an MST regardless of which node you start at. While Student 1 and Student 2 might generate slightly different preorder walks depending on the tree's shape, the mathematical bounds ($c(T) \le c(H^*)$ and $c(H) \le 2c(T)$) are universal. Both students are mathematically guaranteed to get a tour $\le 2 \times OPT$. 
  
  **Answer 7: A) Because the shortcut skips vertices... and the Triangle Inequality guarantees a direct edge is never more expensive.**
  *   **The Logic:** The Full Walk $W$ goes $A \to B \to A \to C$. The shortcut goes $A \to B \to C$ (skipping the return to A). Because $c(B, C) \le c(B, A) + c(A, C)$, the shortcut is guaranteed to subtract cost, or at worst, keep it exactly the same. 
  
  **Answer 8: B) Without it, the general TSP problem is so hard that finding *any* constant-factor approximation is NP-Hard.**
  *   **The Logic:** This is a famous complexity theory theorem! Remember the Hamiltonian Cycle $\to$ TSP reduction? We gave fake edges a weight of $\infty$ (or a massive penalty). If a graph doesn't follow the Triangle Inequality, edges can have infinite arbitrary penalties. If an algorithm could approximate that, it could solve the Hamiltonian Cycle problem in polynomial time! Thus, approximating General TSP is NP-Hard.
  
  **Answer 9: B) The term "2-Approximation" is an upper bound.**
  *   **The Logic:** A $\rho$-approximation means the ratio of (Algorithm Cost / Optimal Cost) $\le \rho$. Here, $100 / 100 = 1$. Since $1 \le 2$, the guarantee holds perfectly! The algorithm is just a "safety net"; it doesn't forbid the algorithm from accidentally stumbling upon the perfect answer.
  
  **Answer 10: B) It conceptually doubles every edge in the MST...**
  *   **The Logic:** An Eulerian Tour (visiting every edge exactly once) is only possible if every vertex in the graph has an **even degree** (an even number of edges touching it). A standard tree has leaves with a degree of 1 (odd). By conceptually doubling every single edge in the MST, every vertex suddenly has an even degree, allowing a perfect, continuous looping walk that traverses the "doubled" tree exactly once.
- ---
- ---
- ---
- ### **Practice Exam: Tracing the TSP Approximation Algorithm**
  
  **Problem 1: The Full Walkthrough (The "Bread and Butter" Question)**
  You are given a fully connected map of 4 cities (A, B, C, D) that perfectly satisfies the Triangle Inequality. The edge costs are:
  *   A – B: **5**
  *   A – C: **4**
  *   A – D: **10**
  *   B – C: **8**
  *   B – D: **6**
  *   C – D: **9**
  
  **Your Tasks:**
  1. Find the Minimum Spanning Tree (MST) of this graph. What are the edges in the MST, and what is its total weight $c(T)$?
  2. Assume you select **City A as the root**. Draw the tree. Write out the exact sequence of cities visited in a **Preorder Traversal** (assume you visit alphabetical ties first, e.g., visit B before C if both are children).
  3. Using that preorder traversal, write out the final **Hamiltonian Cycle ($H$)**. Calculate its exact total cost $c(H)$.
  4. Mathematically verify that your final tour satisfies the 2-approximation guarantee.
  
  ---
  
  **Problem 2: The Triangle Inequality Failure Trace**
  Your professor loves testing *why* the Triangle Inequality is mandatory. 
  Imagine a 3-city graph (X, Y, Z) with the following costs:
  *   X – Y: **2**
  *   Y – Z: **3**
  *   X – Z: **100** *(Notice: This severely violates the Triangle Inequality because $2 + 3 < 100$)*.
  
  **Your Tasks:**
  1. What is the MST of this graph, and what is its weight $c(T)$?
  2. What is the exact sequence of cities in the "Full Walk" ($W$) that traverses every edge of the MST twice? What is the cost of this Full Walk $c(W)$?
  3. Generate the "Shortcut" Hamiltonian Cycle ($H$) by skipping duplicate vertices. What is the cost of this shortcut tour $c(H)$?
  4. Did the shortcut step *decrease* or *increase* the total cost compared to the Full Walk? Does the 2-approximation mathematical guarantee ($c(H) \le 2 c(T)$) still hold?
  
  ---
  
  **Problem 3: Bounding the Optimal Answer**
  You are working as an intern for a logistics company. You run the `APPROX-TSP-TOUR` algorithm on a massive graph of 100 warehouses.
  *   The algorithm calculates the Minimum Spanning Tree weight to be **$c(T) = 5,000$**.
  *   The final shortcut tour it outputs has a weight of **$c(H) = 8,200$**.
  
  Your boss asks you: *"What is the absolute tightest mathematical range we can confidently guarantee for the True Optimal Tour ($H^*$)*?"
  A) $5,000 \le c(H^*) \le 10,000$
  B) $5,000 \le c(H^*) \le 8,200$
  C) $4,100 \le c(H^*) \le 8,200$
  D) $8,200 \le c(H^*) \le 10,000$
  
  ---
  
  **Problem 4: Theoretical vs. Actual Approximation Ratio**
  Using the exact same numbers from Problem 3 ($c(T) = 5,000$ and $c(H) = 8,200$):
  By sheer luck, a supercomputer finishes a brute-force search of the graph a week later and confirms that the absolute perfect optimal tour actually costs **$c(H^*) = 6,560$**.
  
  1. What is the **theoretical worst-case approximation ratio** of the algorithm you used?
  2. What was the **actual approximation ratio** achieved in this specific run?
  
  ---
  ---
- ### **Solutions & "Deep Dive" Walkthroughs**
- #### **Solution 1: The Full Walkthrough**
  **1. The MST:**
  *   To build the MST, we pick the cheapest edges without forming a cycle. 
  *   Pick **A-C (4)**.
  *   Pick **A-B (5)**.
  *   Pick **B-D (6)**.
  *   All 4 nodes are connected. The MST edges are `{A-C, A-B, B-D}`.
  *   **$c(T) = 4 + 5 + 6 = \mathbf{15}$**.
  
  **2. The Preorder Traversal:**
  *   Root is **A**. A's children are B and C. 
  *   We visit alphabetical first, so we go down **B**'s branch. B's child is **D**.
  *   The tree shape is: `C -- A -- B -- D`. 
  *   Preorder (Node, Left, Right): Start at A. Go to B. Go to D. Backtrack to A. Go to C. 
  *   **Sequence: A $\to$ B $\to$ D $\to$ C**
  
  **3. The Hamiltonian Cycle and Cost:**
  *   To make it a cycle, return to the start: **A $\to$ B $\to$ D $\to$ C $\to$ A**.
  *   Now, look up the costs of these *actual* edges in the original graph:
    *   A to B = 5
    *   B to D = 6
    *   D to C = 9 *(Notice we use the direct edge C-D here!)*
    *   C to A = 4
  *   **$c(H) = 5 + 6 + 9 + 4 = \mathbf{24}$**.
  
  **4. Verifying the Bounds:**
  *   Theorem states: $c(H) \le 2 \times c(T)$.
  *   $24 \le 2 \times 15 \implies \mathbf{24 \le 30}$. The guarantee holds perfectly!
  
  ---
- #### **Solution 2: The Triangle Inequality Failure Trace**
  **1. The MST:**
  *   Pick X-Y (2) and Y-Z (3). 
  *   **$c(T) = 5$**.
  
  **2. The Full Walk:**
  *   Trace down and up the tree: X $\to$ Y $\to$ Z $\to$ Y $\to$ X.
  *   Cost = $2 + 3 + 3 + 2 = \mathbf{10}$. *(Notice $c(W)$ is exactly $2 \times c(T)$, this is always true!)*
  
  **3. The Shortcut Cycle:**
  *   Preorder removes the duplicate 'Y': **X $\to$ Y $\to$ Z $\to$ X**.
  *   Cost: X-Y (2) + Y-Z (3) + Z-X (100) = **$\mathbf{105}$**.
  
  **4. The Conclusion:**
  *   The shortcut massively **increased** the cost (from 10 up to 105). 
  *   Let's check the guarantee: Is $c(H) \le 2 \times c(T)$? 
    *   Is $105 \le 10$? **FALSE!**
  *   *Professor Takeaway:* This proves exactly why the Triangle Inequality is a strict requirement. If a direct "shortcut" edge (X to Z) is outrageously expensive, the algorithm skips the cheap layover and blows up the tour cost, completely destroying the mathematical bound.
  
  ---
- #### **Solution 3: Bounding the Optimal Answer**
  **Answer: B) $5,000 \le c(H^*) \le 8,200$**
  *   **Lower Bound:** By definition, the optimal tour is a cycle. Dropping one edge makes it a spanning tree. Therefore, the absolute cheapest spanning tree (the MST) must be cheaper than the optimal tour. $5,000 \le c(H^*)$.
  *   **Upper Bound:** The algorithm literally generated a valid tour that costs 8,200. Because $H^*$ is defined as the *absolute best possible route in existence*, it is mathematically impossible for $H^*$ to be worse than a route we already found. Therefore, $c(H^*) \le 8,200$. 
  *   *The Trap:* Students will look at the 2-approximation rule ($c(H) \le 2 \times OPT$) and say $8,200 \le 2 \times OPT \implies OPT \ge 4,100$. While true, $5,000$ is a **tighter and more accurate** lower bound because we physically know the MST costs 5,000. 
  
  ---
- #### **Solution 4: Theoretical vs. Actual Ratio**
  **1. Theoretical Worst-Case Ratio:**
  *   The algorithm is a mathematically proven **2-Approximation**. 
  *   This means the theoretical guarantee is that the ratio will *never* be worse than **2.0**.
  
  **2. Actual Approximation Ratio:**
  *   The formula for the actual ratio achieved on a specific run is simply the output of the algorithm divided by the true optimal answer.
  *   Ratio = $\frac{\text{Algorithm Cost}}{\text{Optimal Cost}}$
  *   Ratio = $8,200 / 6,560 = \mathbf{1.25}$.
  *   *Takeaway:* The algorithm guarantees it will never be worse than 2x as bad, but in reality, it often performs much better than the worst-case scenario. Here, it was only 25% longer than the perfect route.
- ---
- ---
- ---
- ### **Practice Exam: Approximation Algorithms**
  
  **Question 1: The "Greedy Degree" Trap (Vertex Cover)**
  A classmate suggests that the `APPROX-VERTEX-COVER` algorithm is "dumb" because it blindly picks an arbitrary edge and adds *both* endpoints. They suggest a "smarter" greedy algorithm: *"Always pick the single vertex with the highest degree, add it to the cover, remove its edges, and repeat."*
  Why does the standard `APPROX-VERTEX-COVER` mathematically guarantee a factor of 2, while your classmate's "smarter" greedy algorithm does not?
  A) The standard algorithm ensures the edges picked form a Maximal Matching, creating a strict mathematical lower bound.
  B) The standard algorithm runs in $O(1)$ time.
  C) The highest-degree algorithm will enter an infinite loop.
  D) The standard algorithm actually guarantees a factor of 1.5.
  
  **Question 2: Star Graph Trace (Vertex Cover)**
  You are given a "Star Graph" consisting of 1 central Hub vertex connected to exactly 100 Leaf vertices. 
  1. What is the size of the absolute optimal minimum vertex cover ($C^*$)?
  2. If you run the standard 2-approximation `APPROX-VERTEX-COVER` algorithm on this graph, what is the exact size of the vertex cover ($C$) it will return?
  
  **Question 3: Set Cover Amortized Cost (The Math)**
  In the proof of the Set Cover approximation ratio, we assign a "cost" of $\$1$ to every subset we select, and divide that cost evenly among the *newly covered* elements. 
  Suppose you select a subset $S$ that contains 10 elements. However, 6 of those elements were already covered by previously selected sets. 
  What is the exact mathematical cost ($c_x$) assigned to each of the **newly covered** elements in this step?
  A) $1/10$
  B) $1/6$
  C) $1/4$
  D) $0$
  
  **Question 4: Set Cover Approximation Ratio**
  The Greedy Set Cover algorithm does not have a constant approximation ratio like 2 or 3. Instead, its worst-case bound grows logarithmically with the size of the universe $X$.
  If your universe contains $N = 10,000$ elements, which formula represents the theoretical maximum approximation ratio (how many times worse than optimal the greedy algorithm might perform)?
  A) $O(\sqrt{N})$
  B) $\ln(10,000) + 1$
  C) $N / 2$
  D) $\log_2(10,000)$
  
  **Question 5: The Greedy Trap (Set Cover Trace)**
  Universe $X = \{1, 2, 3, 4, 5, 6\}$.
  You are given the following subsets:
  *   $S_1 = \{1, 2, 3\}$
  *   $S_2 = \{4, 5, 6\}$
  *   $S_3 = \{2, 3, 4, 5\}$
  
  1. What is the absolute optimal number of sets required to cover $X$?
  2. Trace the `GREEDY-SET-COVER` algorithm. Exactly how many sets will the Greedy algorithm pick, and what is the sequence?
  
  **Question 6: Makespan Lower Bounds**
  You are using Graham's Algorithm to schedule jobs on **$m = 5$** identical machines.
  You have **20 jobs** that take exactly 1 hour each, and **1 job** that takes exactly 50 hours.
  Based on the two mathematical lower bounds for the optimal makespan ($OPT$), what is the absolute lowest possible time this batch of jobs can be completed?
  A) 14 hours
  B) 50 hours
  C) 70 hours
  D) 100 hours
  
  **Question 7: Graham vs. LPT Scheduling**
  Graham's algorithm processes jobs in whatever arbitrary order they are given. The **Longest Processing Time First (LPT)** heuristic improves this by sorting the jobs from longest to shortest first.
  What is the exact mathematical guarantee (approximation ratio) improvement when moving from standard Graham's Algorithm to LPT?
  A) It improves from 3.0 down to 2.0.
  B) It improves from 2.0 down to exactly 1.0 (Optimal).
  C) It improves from 2.0 down to strictly $\le 1.5$ (specifically $3/2 - 1/2m$).
  D) It improves the time complexity, but the approximation ratio stays at 2.0.
  
  **Question 8: PTAS vs. FPTAS Definition**
  You are evaluating an approximation algorithm for the Subset Sum problem. The user passes in an error parameter $\epsilon > 0$. 
  Algorithm A runs in $O(n^{2/\epsilon})$ time.
  Algorithm B runs in $O(n^3 / \epsilon)$ time.
  Which algorithm is a Fully Polynomial-Time Approximation Scheme (FPTAS), and why?
  
  **Question 9: FPTAS Trimming Trace**
  You are running the `TRIM` subroutine for the Subset Sum FPTAS.
  Your current sorted list is: `L = [100, 105, 110, 120]`.
  Your trimming parameter is **$\delta = 0.10$** (meaning $1 + \delta = 1.10$).
  According to the trimming rule ($\frac{y}{1+\delta} \le z \le y$), which elements will be deleted (trimmed) from this list because they can be safely "represented" by a slightly smaller number?
  A) 105 only
  B) 105 and 110
  C) 110 and 120
  D) 120 only
  
  **Question 10: Compounding Error in Subset Sum**
  In the FPTAS for Subset Sum, the user requests a final error margin of exactly $\epsilon = 0.20$ (20%). There are $n = 10$ items in the set.
  When the algorithm calls the `TRIM` subroutine during each iteration, what exact value is passed in as the trimming parameter $\delta$?
  A) $0.20$
  B) $0.10$
  C) $0.01$
  D) $0.02$
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: A) The standard algorithm ensures the edges picked form a Maximal Matching...**
  *   **The Deep Dive:** To guarantee a strict ratio, you need a hard mathematical lower bound to compare against. By blindly grabbing an edge and deleting its neighbors, the standard algorithm generates a set of completely disconnected edges (a Maximal Matching). We know with 100% mathematical certainty that the true optimal cover *must* pick at least 1 vertex per disconnected edge. Since we picked 2, we are guaranteed to be exactly $\le 2 \times OPT$. The "highest degree" greedy method lacks this structural guarantee and can actually perform logarithmically worse than optimal on specific trap graphs!
  
  **Answer 2: Optimal = 1, Approximation = 2**
  *   **The Deep Dive:** 
    1. The true optimal cover is size **1** (just place a camera on the central hub, and it sees all 100 connecting edges). 
    2. The Approximation Algorithm grabs an arbitrary edge. It adds the Hub and *one* Leaf to the cover. It then crosses off all edges connected to the Hub and the Leaf. Because the Hub is connected to everything, *all* edges in the graph are instantly crossed off. The loop ends. The algorithm returns a cover of size **2**. 
    *   *Check the bound:* $2 \le 2 \times 1$. The 2-approximation guarantee holds perfectly!
  
  **Answer 3: C) 1/4**
  *   **The Deep Dive:** The total cost of picking a subset is always $\$1$. You only divide that dollar among the elements that were *covered for the very first time* during this step. Since 6 were already covered, only 4 are new. $\$1 / 4 = 0.25$. 
  
  **Answer 4: B) $\ln(10,000) + 1$**
  *   **The Deep Dive:** This is the exact theorem proven via the Harmonic Number bounds. Set Cover cannot be approximated to a constant factor (like 2 or 3). Its worst-case approximation ratio scales naturally logarithmically with the size of the universe $X$.
  
  **Answer 5: Optimal = 2, Greedy = 3**
  *   **The Deep Dive Trace:**
    1. **Optimal:** Picking $S_1$ and $S_2$ covers all 6 elements perfectly. $OPT = 2$.
    2. **Greedy Step 1:** Greedy looks for the largest set. $S_3$ has 4 elements. $S_1$ and $S_2$ only have 3. Greedy picks **$S_3$** (covers 2,3,4,5). Remaining uncovered: $\{1, 6\}$.
    3. **Greedy Step 2:** Greedy needs to cover 1. It picks **$S_1$**. Remaining: $\{6\}$.
    4. **Greedy Step 3:** Greedy needs to cover 6. It picks **$S_2$**.
    *   *Result:* Greedy was "trapped" by the overlapping middle set and ended up picking 3 sets instead of 2.
  
  **Answer 6: B) 50 hours**
  *   **The Deep Dive:** $OPT$ has two lower bounds. 
    1. Average load: $(20 \times 1 + 50) / 5 \text{ machines} = 70 / 5 = 14 \text{ hours}$.
    2. Max Job: The single biggest job is 50 hours.
    $OPT$ must be $\ge \max(14, 50)$. Therefore, the absolute fastest the schedule can finish is **50 hours** (because that one massive job has to run somewhere, and it can't be split up!).
  
  **Answer 7: C) It improves from 2.0 down to strictly $\le 1.5$.**
  *   **The Deep Dive:** The exact bound for Longest Processing Time First (LPT) is $3/2 - 1/2m$. If you have a ton of machines, this limit approaches $1.5$. By putting the "big rocks" in the machines first, you save the tiny 1-hour jobs for the very end, allowing you to perfectly smooth out the imbalances and dramatically tighten the mathematical guarantee.
  
  **Answer 8: Algorithm B is the FPTAS.**
  *   **The Deep Dive:** To be a **Fully** Polynomial-Time Approximation Scheme, the variable $\epsilon$ cannot be locked up inside an exponent! 
    *   In Algorithm A, if $\epsilon = 0.01$, the runtime is $n^{200}$. This is computationally impossible.
    *   In Algorithm B, if $\epsilon = 0.01$, the runtime is $n^3 / 0.01 = 100n^3$. This scales smoothly and is a true FPTAS.
  
  **Answer 9: B) 105 and 110**
  *   **The Deep Dive Trace:** $1 + \delta = 1.10$.
    *   Start at anchor `100`. Test `105`: Is $105 \le 100 \times 1.10$? Yes ($105 \le 110$). **Trim 105**.
    *   Test `110`: Is $110 \le 100 \times 1.10$? Yes ($110 \le 110$). **Trim 110**.
    *   Test `120`: Is $120 \le 100 \times 1.10$? No ($120 \not\le 110$). Keep 120. It becomes the new anchor.
    *   *Result:* 105 and 110 are safely deleted.
  
  **Answer 10: C) 0.01**
  *   **The Deep Dive:** If the user wants a final error of $20\%$ ($\epsilon = 0.20$), we cannot trim by 20% at every step, because the error compounds on itself $n$ times during the loop! To mathematically ensure the total compounded error doesn't exceed $\epsilon$, we divide it by $2n$.
    *   $\delta = \epsilon / 2n \implies 0.20 / (2 \times 10) = 0.20 / 20 = \mathbf{0.01}$. We must trim at a strict 1% margin at each step.
- ---
- ---
- ---
- Here are **10 Advanced Practice Questions** focusing exclusively on the **Vertex Cover** and **Set Cover** approximation algorithms. 
  
  Since your professor loves tracing, mathematical lower bounds, and theoretical "traps," these questions target the exact proofs and edge cases shown in your slides (including the ILP formulation and the Harmonic number derivation).
  
  Grab your scratch paper. The "Deep Dive" answer key is at the bottom!
  
  ---
- ### **Practice Exam: Vertex Cover & Set Cover**
  
  **Question 1: The "Name" Trap (Definition Check)**
  In graph theory, a "Vertex Cover" is a subset of vertices $V' \subseteq V$. What exactly must this subset "cover"?
  A) It must ensure every vertex in the graph is connected to $V'$.
  B) It must ensure every edge in the graph has at least one endpoint in $V'$.
  C) It must ensure every edge in the graph has *both* endpoints in $V'$.
  D) It must contain all vertices of maximum degree.
  
  **Question 2: Tracing `APPROX-VERTEX-COVER` (The 2x Worst-Case)**
  Imagine a path graph with 4 vertices connected in a straight line: `A — B — C — D`.
  1. What is the size of the absolute optimal Minimum Vertex Cover ($C^*$)? Which vertices are in it?
  2. If the approximation algorithm arbitrarily picks the edge `(A, B)` first, trace the rest of the algorithm. What is the final size of the vertex cover it returns? 
  3. Does this specific trace perfectly hit the 2-approximation worst-case limit?
  
  **Question 3: The Maximal Matching Proof**
  Let $M$ be the set of edges arbitrarily selected by the `APPROX-VERTEX-COVER` algorithm. 
  In the mathematical proof of the 2-approximation guarantee, we establish that $|C^*| \ge |M|$. 
  Why is this mathematically absolute?
  A) Because $M$ contains every edge in the graph.
  B) Because the edges in $M$ are completely disjoint (they share no endpoints), so any valid cover *must* spend at least 1 vertex per edge in $M$ just to cover them.
  C) Because the optimal cover $C^*$ must pick both endpoints of every edge.
  D) Because the algorithm removes edges incident to $u$ and $v$.
  
  **Question 4: Can we do better? (Slide 10)**
  The standard algorithm guarantees a Factor-2 approximation. However, your slides mention a specific scenario where you can guarantee a tighter **11/6 approximation** ($\approx 1.83$). 
  What is the condition for this tighter approximation, and what algorithm is used?
  A) The graph must be a tree; use Breadth-First Search.
  B) The graph must have no triangles; use Bipartite Matching.
  C) The maximum degree of the graph must be 3; use the greedy "Choose vertex with maximum degree" algorithm.
  D) The graph must be fully connected; use Kruskal's algorithm.
  
  **Question 5: Integer Linear Programming (ILP)**
  Slide 11 notes that Vertex Cover can be solved using Integer Linear Programming. 
  If $x_u$ is a binary variable (1 if vertex $u$ is in the cover, 0 if it is not), what is the mathematical constraint that must be applied to every edge $(u,v)$ in the graph to ensure it is a valid vertex cover?
  A) $x_u + x_v = 2$
  B) $x_u + x_v \ge 1$
  C) $x_u \times x_v = 1$
  D) $x_u + x_v \le 1$
  
  **Question 6: Tracing `GREEDY-SET-COVER`**
  Universe $X = \{1, 2, 3, 4, 5, 6\}$.
  You are given the following subsets:
  *   $S_1 = \{1, 2, 3\}$
  *   $S_2 = \{4, 5, 6\}$
  *   $S_3 = \{1, 2, 4, 5\}$
  1. Trace the Greedy algorithm. Which sets does it pick, and in what order?
  2. What is the size of the greedy cover, and what is the size of the true optimal cover?
  
  **Question 7: The Set Cover Bound Calculation**
  You are running `GREEDY-SET-COVER` on a universe of exactly **$X = 1,000$** elements. 
  The algorithm guarantees an approximation ratio of $\ln|X| + 1$. 
  Assuming $\ln(1000) \approx 6.9$, if the true optimal cover requires exactly **10 sets**, what is the absolute maximum number of sets the greedy algorithm is mathematically allowed to return?
  A) 20
  B) 69
  C) 79
  D) 100
  
  **Question 8: The Amortized Cost Proof (Slides 41-45)**
  In the proof for the Set Cover approximation ratio, we assign a cost of $\$1$ to every set we pick, and divide that $\$1$ evenly among the *newly covered* elements. 
  Suppose a set $S$ has 4 elements. During the algorithm's execution, these elements are covered one by one in different steps. 
  According to the math on Slide 45, the sum of the costs assigned to the elements of $S$ is bounded by the **Harmonic Number** $H(|S|)$. 
  What is the exact fractional sequence that makes up $H(4)$?
  A) $1 + 1 + 1 + 1$
  B) $\frac{1}{4} + \frac{1}{4} + \frac{1}{4} + \frac{1}{4}$
  C) $\frac{1}{1} + \frac{1}{2} + \frac{1}{3} + \frac{1}{4}$
  D) $1 + 2 + 3 + 4$
  
  **Question 9: Vertex Cover to Set Cover Translation**
  Why is the approximation ratio for Set Cover ($\ln N$) so much worse than the ratio for Vertex Cover (2), even though Vertex Cover is just a special case of Set Cover?
  A) Because Set Cover is NP-Hard, but Vertex Cover is in P.
  B) In Vertex Cover, an element (an edge) can only belong to exactly **2** subsets (its endpoints). In Set Cover, an element can belong to an infinite number of subsets, making the greedy "trap" much more severe.
  C) Because Set Cover uses directed graphs.
  D) It isn't worse; $\ln N$ is smaller than 2 for most graphs.
  
  **Question 10: Tie-Breaking (Slide 40)**
  In the word-covering practice problem on Slide 40 (`arid, dash, drain...`), multiple sets might cover the exact same number of uncovered elements at a specific step. 
  How does the `GREEDY-SET-COVER` algorithm handle ties?
  A) It crashes.
  B) It evaluates all tied branches recursively (like DP).
  C) It breaks ties arbitrarily (e.g., picking the one that appears first alphabetically or first in the array).
  D) It picks the set with the fewest total elements.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: B) It must ensure every edge in the graph has at least one endpoint in $V'$.**
  *   **The Trap:** It's called "Vertex Cover," so students assume it covers vertices. It does not! A Vertex Cover is a set of vertices that acts as security cameras to "cover" (observe) every single **edge** in the graph. 
  
  **Answer 2: Optimal = 2, Approximation = 4 (Hits the 2x limit!)**
  *   **Optimal ($C^*$):** Pick `{B, C}`. B covers (A,B) and (B,C). C covers (B,C) and (C,D). Total size = **2**.
  *   **Approx Trace:** 
    1. Algorithm arbitrarily picks edge `(A, B)`. It adds **both** `{A, B}` to the cover.
    2. It deletes edges (A,B) and (B,C) from the graph. 
    3. The only edge left is `(C, D)`. It picks it. It adds **both** `{C, D}` to the cover.
    4. Graph is empty. Algorithm terminates.
  *   **Result:** Cover is `{A, B, C, D}`. Size = **4**. 
  *   **Conclusion:** Yes, $4 \le 2 \times 2$. This perfectly demonstrates the absolute worst-case scenario of the 2-approximation bound!
  
  **Answer 3: B) Because the edges in $M$ are completely disjoint...**
  *   **The Logic:** This is the bedrock of the 2-approximation proof (Slide 9). Because our algorithm actively deletes all neighboring edges whenever it picks an edge, the set of edges it picked ($M$) forms a **Maximal Matching** (no two edges touch). If you have 5 disconnected edges hovering in space, it is physically impossible to cover them with fewer than 5 vertices. Therefore, the perfect optimal cover $C^*$ *must* be $\ge |M|$. Since our algorithm picks 2 vertices per edge, our size is $2|M|$. Therefore, our size is $\le 2|C^*|$.
  
  **Answer 4: C) The maximum degree of the graph must be 3; use the greedy "highest degree" algorithm.**
  *   **The Logic:** This is a specific piece of trivia from Slide 10. While the "pick highest degree" greedy algorithm is notoriously bad for general graphs, if you restrict the graph so that no vertex has more than 3 edges, that specific greedy algorithm is mathematically guaranteed to achieve an $11/6$ approximation.
  
  **Answer 5: B) $x_u + x_v \ge 1$**
  *   **The Logic:** $x_u$ and $x_v$ represent the two endpoints of an edge. To be a valid Vertex Cover, at least one of those endpoints *must* be in the cover (value of 1). 
    *   If neither is in the cover: $0 + 0 = 0$ (Fails constraint).
    *   If one is in the cover: $1 + 0 = 1$ (Passes constraint).
    *   If both are in the cover: $1 + 1 = 2$ (Passes constraint).
  
  **Answer 6: Greedy Picks $S_3, S_1, S_2$ (Greedy=3, Optimal=2)**
  *   **Trace:**
    1. Greedy looks for the largest set. $S_3$ covers 4 elements. $S_1$ and $S_2$ cover 3. Greedy picks **$S_3$** (covers 1,2,4,5). Remaining uncovered: $\{3, 6\}$.
    2. Now, Greedy evaluates the remaining sets. $S_1$ can cover $\{3\}$. $S_2$ can cover $\{6\}$. They tie. Let's say it picks **$S_1$**. Remaining uncovered: $\{6\}$.
    3. Greedy must pick **$S_2$** to cover $\{6\}$. 
  *   **Result:** Greedy picks 3 sets. The Optimal choice was to just pick $S_1$ and $S_2$ to cover all 6 elements perfectly. The overlapping "trap" set ($S_3$) ruined the greedy choice.
  
  **Answer 7: C) 79**
  *   **The Math:** Approximation Ratio = $\ln|X| + 1 = 6.9 + 1 = 7.9$.
  *   This means the Greedy algorithm will return a cover no larger than $7.9 \times OPT$. 
  *   If $OPT = 10$, then $10 \times 7.9 = \mathbf{79}$. (The greedy algorithm might return 79 sets to do a job that could have been done in 10!).
  
  **Answer 8: C) $\frac{1}{1} + \frac{1}{2} + \frac{1}{3} + \frac{1}{4}$**
  *   **The Logic:** This is the definition of the Harmonic Number, which forms the core of the proof on Slides 43-45. As the elements of the set $S$ get covered by the algorithm, the number of uncovered elements in $S$ shrinks: 4, then 3, then 2, then 1. If a set is picked to cover those remaining items, the cost is split among them, creating the exact sequence $1/4 + 1/3 + 1/2 + 1/1$.
  
  **Answer 9: B) In Vertex Cover, an element (an edge) can only belong to exactly 2 subsets...**
  *   **The Logic:** Vertex Cover is literally just Set Cover where the frequency of every element is strictly capped at 2. Because an edge only has 2 ends, the "traps" that the Greedy algorithm can fall into are severely limited, allowing for a hard constant factor of 2. In Set Cover, an element can be in an infinite number of sets, allowing for massive, complex overlaps that trick the greedy algorithm logarithmically.
  
  **Answer 10: C) It breaks ties arbitrarily.**
  *   **The Logic:** Slide 38 specifically notes: "choosing a subset $S$ that covers as many uncovered elements as possible *(breaking ties arbitrarily)*." In your professor's specific practice problem, the tie-breaking rule was set to "alphabetical order." In code, it usually just picks the one that appeared first in the array.