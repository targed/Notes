### **Practice Exam: NP-Hardness & Reductions**

**Question 1: The Optimization Trap**
You are trying to mathematically categorize the following problem: *"Given a weighted graph, find the absolute longest simple path between node A and node B."*
You know this problem is incredibly difficult to solve. Which of the following is the most accurate classification for this specific phrasing of the problem?
A) NP-Complete
B) NP-Hard, but strictly NOT in NP.
C) In NP, but NOT NP-Hard.
D) P

**Question 2: The Direction of Reduction**
You have invented a new computational puzzle called *GraphTetris*. You want to write a research paper proving that *GraphTetris* is NP-Hard. You already know that the *Vertex Cover* problem is NP-Hard. 
What is the mathematically correct way to prove your claim?
A) Prove that *GraphTetris* $\le_p$ *Vertex Cover* (Reduce GraphTetris to Vertex Cover).
B) Prove that *Vertex Cover* $\le_p$ *GraphTetris* (Reduce Vertex Cover to GraphTetris).
C) Prove that both problems can be solved by brute force in $O(2^n)$ time.
D) Prove that *GraphTetris* can be verified in polynomial time.

**Question 3: 3-SAT to MIS (Vertex Count)**
You are writing the "Preprocessor" to reduce a 3-SAT problem into a Maximum Independent Set (MIS) problem. 
The 3-SAT formula has **80 distinct variables** and **250 clauses**. 
Exactly how many vertices will be in the graph you construct for the MIS solver?
A) 80
B) 250
C) 750
D) 240

**Question 4: 3-SAT to MIS (The Logic)**
In the 3-SAT to MIS reduction, you draw a graph with "triangles" and "conflict edges." 
What specific logical rule of the 3-SAT problem is being enforced by connecting the three literals of a single clause into a **triangle** (clique of 3)?
A) It prevents the solver from assigning contradictory truth values to the same variable.
B) It forces the Independent Set solver to select *at most one* literal per clause.
C) It ensures the graph satisfies the Triangle Inequality.
D) It guarantees the reduction runs in polynomial time.

**Question 5: HC to TSP (Target Cost)**
You are reducing the Hamiltonian Cycle (HC) problem to the Decision Traveling Salesperson Problem (TSP). The original unweighted HC graph has **$V = 50$ vertices**.
You construct a complete graph for the TSP solver, assigning a weight of **1** to real edges and a weight of **2** to fake edges. 
What exact target cost ($K$) must you ask the TSP solver to find to perfectly answer the HC problem?
A) $K = 0$
B) $K = 50$
C) $K = 100$
D) $K = 1$

**Question 6: The "Fake Edge" Flaw**
In the HC $\to$ TSP reduction from Question 5, what would be the catastrophic result if you accidentally assigned a weight of **1** to the "fake" edges instead of 2?
A) The TSP solver would crash.
B) The reduction would take exponential time.
C) The TSP solver might return "Yes" by using a fake edge, falsely claiming a Hamiltonian Cycle exists when it actually doesn't.
D) The target cost $K$ would become undefined.

**Question 7: The Definition of NP**
Which of the following is the strict mathematical definition of the complexity class **NP**?
A) Problems that cannot be solved in polynomial time.
B) Optimization problems that require exponential brute-force search.
C) Decision problems for which a proposed solution (certificate) can be verified as correct in polynomial time.
D) Problems that have been proven to be completely unsolvable by Turing machines.

**Question 8: The Reduction Bottleneck**
A researcher discovers a way to translate the known NP-Complete problem *3-SAT* into a new problem called *SuperSort*. 
However, translating the 3-SAT formula into the *SuperSort* format requires generating all possible truth assignments, which takes $O(2^n)$ time in the preprocessor. 
What can we conclude about *SuperSort*?
A) *SuperSort* is officially NP-Hard.
B) *SuperSort* is officially in P.
C) We can conclude nothing about *SuperSort*'s difficulty because the reduction is invalid.
D) *SuperSort* is NP-Complete.

**Question 9: Transitivity of Reductions**
Assume the following reductions are proven:
*   `Problem A` $\le_p$ `Problem B`
*   `Problem B` $\le_p$ `Problem C`

If a mathematician suddenly discovers a brilliant $O(n^2)$ algorithm to solve **Problem B**, which other problem is now mathematically guaranteed to also have a polynomial-time solution?
A) Problem A only.
B) Problem C only.
C) Both Problem A and Problem C.
D) Neither. 

**Question 10: The Ultimate Consequence**
If someone manages to prove that **P = NP**, which of the following statements becomes TRUE?
A) The Traveling Salesperson Optimization Problem can be solved in polynomial time.
B) Exhaustive brute-force search is no longer possible.
C) All NP-Complete problems can be solved in polynomial time.
D) Both A and C.

---
---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: B) NP-Hard, but strictly NOT in NP.**
  *   **Explanation:** NP *only* contains Decision Problems (Yes/No questions). Finding the "absolute longest path" is an Optimization problem. Furthermore, to *verify* if a path is truly the longest, you would have to check every other possible path in the graph, which takes exponential time. Because it cannot be verified in polynomial time, it is physically banned from the NP bubble. It is strictly NP-Hard.
  
  **Answer 2: B) Prove that *Vertex Cover* $\le_p$ *GraphTetris*.**
  *   **Explanation:** This is the #1 mistake students make. You must reduce the KNOWN HARD problem into the NEW problem. If you can translate Vertex Cover into GraphTetris, you are proving that GraphTetris is capable of solving Vertex Cover. Therefore, GraphTetris must be *at least as hard* as Vertex Cover. 
  
  **Answer 3: C) 750**
  *   **Explanation:** The preprocessor draws exactly one triangle (3 vertices) for every single clause. The number of variables (80) is completely irrelevant to the vertex count; the variables only dictate how the red conflict edges are drawn later. $250 \text{ clauses} \times 3 \text{ vertices/clause} = \mathbf{750 \text{ vertices}}$.
  
  **Answer 4: B) It forces the Independent Set solver to select at most one literal per clause.**
  *   **Explanation:** By definition, an Independent Set cannot contain two nodes that are connected by an edge. Because a triangle connects all 3 literals to each other, the IS solver is mathematically forced to pick at most 1 node from that triangle. This perfectly simulates the 3-SAT rule that you must find a single "True" literal to satisfy a clause. *(Note: Option A describes the purpose of the red "Conflict Edges" drawn across the graph, not the triangles).*
  
  **Answer 5: B) $K = 50$**
  *   **Explanation:** A Hamiltonian Cycle must visit every vertex exactly once, meaning the cycle contains exactly $V$ edges. Since $V = 50$, the tour uses 50 edges. Because we assigned a weight of 1 to all real edges, a valid tour using only real edges will have a total cost of exactly 50 ($50 \times 1$).
  
  **Answer 6: C) The TSP solver might return "Yes" by using a fake edge...**
  *   **Explanation:** The entire point of the reduction is to trick the TSP solver into finding a Hamiltonian Cycle. If fake edges cost 1, the solver could use a fake edge to complete a tour of cost 50. It would output "Yes, a tour exists," but that tour physically does not exist in the original HC graph. By making fake edges cost 2, any tour using a fake edge will cost at least 51, safely failing the test.
  
  **Answer 7: C) Decision problems for which a proposed solution can be verified... in polynomial time.**
  *   **Explanation:** NP stands for Nondeterministic Polynomial time. It is the class of "Graders." It does not mean "Not Polynomial." Sudoku is the perfect example: hard to solve from scratch, but if someone hands you a completed grid (the certificate), you can verify it in polynomial time.
  
  **Answer 8: C) We can conclude nothing... because the reduction is invalid.**
  *   **Explanation:** The "$\le_p$" symbol strictly means **Polynomial-Time Reduction**. The translation process (Preprocessor and Postprocessor) must be fast. If building the shortcut takes $O(2^n)$ time, the proof is invalid. The reduction itself became the bottleneck.
  
  **Answer 9: A) Problem A only.**
  *   **Explanation:** $A \le_p B$ means A translates into B. If B is easy, A is automatically easy because you can just translate A into B and solve it. 
  *   However, $B \le_p C$ means B translates into C. Knowing B is easy tells us absolutely nothing about C. (For example, I can translate the easy problem of "Sorting an Array" into the impossibly hard "Traveling Salesperson Problem", but that doesn't make TSP easy!). 
  
  **Answer 10: D) Both A and C.**
  *   **Explanation:** If P = NP, then every problem in NP (including all NP-Complete problems like 3-SAT) can be solved in polynomial time (C is true). 
  *   What about A? The Optimization TSP is NP-Hard. However, if P = NP, we can solve the *Decision* TSP quickly. If we can solve the Decision TSP quickly, we can just use Binary Search to repeatedly ask the Decision TSP: "Is there a tour $\le 1000$?", "Is there a tour $\le 500$?" until we zero in on the exact optimal route in polynomial time! Thus, Optimization TSP also becomes solvable in polynomial time.
- ---
- ---
- ---
- ### **Practice Exam: Advanced Reductions & Complexity**
  
  **Question 1: The "Easy to Hard" Reduction Trap**
  We know that finding the Shortest Path in a graph is in **P** (using Dijkstra's algorithm). We know that **3-SAT** is **NP-Complete**. 
  Suppose a student writes a polynomial-time reduction translating the Shortest Path problem into 3-SAT (Shortest Path $\le_p$ 3-SAT). 
  What does this reduction mathematically prove?
  A) It proves that P = NP.
  B) It proves that Shortest Path is actually NP-Hard.
  C) It proves absolutely nothing new; this reduction is mathematically guaranteed to exist because Shortest Path is in NP.
  D) It proves that 3-SAT can be solved in polynomial time.
  
  **Question 2: The Time Complexity of a Reduction**
  Problem $A$ reduces to Problem $B$ ($A \le_p B$). 
  The preprocessor translates an instance of $A$ (of size $n$) into an instance of $B$. The translation process takes **$O(n^2)$ time**, and the resulting translated input for Problem B has a size of **$O(n^2)$**. 
  You discover an algorithm to solve Problem $B$ in **$O(k^3)$ time**, where $k$ is the input size to Problem B.
  What is the overall time complexity to solve Problem A using this reduction?
  A) $O(n^3)$
  B) $O(n^5)$
  C) $O(n^6)$
  D) $O(n^8)$
  
  **Question 3: The Forgotten Conflict Edges**
  You are performing the **3-SAT to Maximum Independent Set (MIS)** reduction. You successfully draw the $k$ triangles (one for each clause). However, you completely forget to draw the red "conflict edges" between $x$ and $\neg x$. 
  If you feed this broken graph into the MIS solver and ask if a set of size $k$ exists, what will happen?
  A) The MIS solver will crash.
  B) The MIS solver will always return "Yes," rendering the reduction useless.
  C) The MIS solver will always return "No," rendering the reduction useless.
  D) The reduction will still work, but it will take exponential time.
  
  **Question 4: The Certificate (Verification)**
  To prove that the **Decision Traveling Salesperson Problem (TSP)** is in the class NP, you must prove it can be verified in polynomial time. 
  If someone hands you a "Certificate" claiming that a valid TSP tour exists with a cost of $\le W$, what exactly *is* the certificate they must hand you, and what is the exact time complexity required to verify it?
  A) The certificate is the minimum spanning tree; Verification takes $O(E \log V)$.
  B) The certificate is the sequence of vertices in the tour; Verification takes $O(V)$.
  C) The certificate is the boolean matrix of all paths; Verification takes $O(V^2)$.
  D) The certificate is $W$; Verification takes $O(1)$.
  
  **Question 5: The "Impossible" Reduction**
  Which of the following hypothetical polynomial-time reductions would instantly win you the $1,000,000 Millennium Prize by proving **P = NP**?
  A) 3-SAT $\le_p$ Hamiltonian Cycle
  B) Hamiltonian Cycle $\le_p$ Vertex Cover
  C) TSP (Decision) $\le_p$ Minimum Spanning Tree
  D) Minimum Spanning Tree $\le_p$ TSP (Decision)
  
  **Question 6: The IF and ONLY IF Rule**
  A valid reduction from Problem A to Problem B requires a translation function $f(x)$. For the reduction to be mathematically sound, which of the following logical conditions must be absolutely true regarding the answers?
  A) If the answer to A is "Yes", the answer to B(f(x)) must be "Yes". If the answer to A is "No", the answer to B(f(x)) can be "Yes" or "No".
  B) If the answer to A is "Yes", the answer to B(f(x)) must be "No".
  C) The answer to A is "Yes" **if and only if** the answer to B(f(x)) is "Yes".
  D) The answer to A and B must both be "No".
  
  **Question 7: The Root of the Tree (Cook-Levin Theorem)**
  Look at the reduction tree on your slides (Circuit-SAT $\to$ 3-SAT $\to$ Clique $\dots$). 
  If proving a problem is NP-Complete requires reducing a *known* NP-Complete problem into it, how did Stephen Cook prove that the very first problem (`CIRCUIT-SAT`) was NP-Complete in 1971 when there were no other known problems to reduce from?
  A) He reduced it from the Halting Problem.
  B) He proved it by simulating a non-deterministic Turing machine's state transitions as a massive boolean logic circuit.
  C) He didn't; it is just an unproven assumption that the rest of computer science relies on.
  D) He brute-forced it.
  
  **Question 8: Clique to Independent Set**
  A **Clique** is a set of vertices where *every* vertex is connected to *every other* vertex. An **Independent Set** is a set of vertices where *no* vertices are connected.
  If you have a graph $G$ and you want to reduce the Clique problem to the Independent Set problem, what is the $O(V^2)$ preprocessor translation?
  A) Reverse the direction of all edges.
  B) Create the **Complement Graph** $\bar{G}$ (Draw edges where there are none, and delete edges that currently exist).
  C) Assign a weight of -1 to all edges.
  D) Split every vertex into an "in-node" and an "out-node".
  
  **Question 9: The NP-Hard Boundary**
  Which of the following statements about **NP-Hard** problems is mathematically **TRUE**?
  A) All NP-Hard problems are decision problems.
  B) If an NP-Hard problem is not in NP, it cannot be solved by any computer, even with infinite time.
  C) An NP-Hard problem might require checking an exponentially large search space to verify if a proposed optimal solution is truly the best.
  D) NP-Hard problems are a subset of NP-Complete problems.
  
  **Question 10: The Halting Problem**
  Your professor mentions that some problems are even harder than NP. The famous "Halting Problem" asks: *"Given a computer program and its input, will the program eventually halt (stop), or will it run forever?"*
  What is the complexity classification of the Halting Problem?
  A) P
  B) NP-Complete
  C) Undecidable (Uncomputable)
  D) Polynomial
  
  ---
  ---
- ### **Solutions & "Professor Trap" Explanations**
  
  **Answer 1: C) It proves absolutely nothing new.**
  *   **The Trap:** Students see an easy problem reducing to a hard problem and panic. 
  *   **The Logic:** By definition, **NP-Hard** means *every single problem in the class NP can be reduced to it*. Since Shortest Path is in P, it is also in NP. Therefore, Shortest Path reduces to 3-SAT. This is a known fact! It doesn't make Shortest Path hard; it just means you are taking an easy problem and over-complicating it by using a massive sledgehammer (3-SAT) to crack a tiny nut.
  
  **Answer 2: C) $O(n^6)$**
  *   **The Trap:** A student will quickly look at $O(n^2)$ and $O(n^3)$, add them together, and guess $O(n^3)$. 
  *   **The Logic:** You must be extremely careful with the size of the input! The preprocessor takes the original input $n$ and inflates it to a new input of size $K = n^2$. You then pass this massive $n^2$ input into Algorithm B, which runs in $K^3$ time. 
  *   **The Math:** $(n^2)^3 = n^6$. The total time is the preprocessing time $O(n^2)$ plus the solving time $O(n^6)$, resulting in a final time complexity of **$O(n^6)$**. 
  
  **Answer 3: B) The MIS solver will always return "Yes," rendering the reduction useless.**
  *   **The Logic:** The conflict edges between $x$ and $\neg x$ are the only things stopping the solver from picking contradictory variables. If you forget to draw them, the solver will simply look at the $k$ disconnected triangles and happily pick one node from each. It will effortlessly find an Independent Set of size $k$ every single time, even if the 3-SAT formula was physically unsatisfiable (e.g., $x \text{ AND } \neg x$). A reduction is completely invalid if a "Yes" on the new problem doesn't guarantee a "Yes" on the original problem.
  
  **Answer 4: B) The certificate is the sequence of vertices; Verification takes $O(V)$.**
  *   **The Logic:** To prove a problem is in NP, the certificate must be the proposed answer. For TSP, the proposed answer is the literal list of cities to visit (e.g., $A \to C \to D \to B \to A$). To verify it, you just loop through the list, ensure no duplicates exist (except the start/end), sum the edge weights between them, and check if the sum is $\le W$. Summing $V$ edges takes $O(V)$ time.
  
  **Answer 5: C) TSP (Decision) $\le_p$ Minimum Spanning Tree**
  *   **The Trap:** A, B, and D are all known, standard reductions. (Hard $\le_p$ Hard, or Easy $\le_p$ Hard). 
  *   **The Logic:** Option C reduces a **Known Hard** problem (TSP) to a **Known Easy** problem (MST is solvable in polynomial time using Kruskal's or Prim's). If you can translate a hard problem into an easy problem in polynomial time, it means the hard problem was actually easy all along! This proves P = NP.
  
  **Answer 6: C) The answer to A is "Yes" if and only if the answer to B(f(x)) is "Yes".**
  *   **The Logic:** This is the absolute mathematical law of a valid reduction. It must be a perfect bi-directional mapping. 
    *   If A is Yes $\to$ B must be Yes.
    *   If A is No $\to$ B must be No.
    *   If B returns "Yes" but A was actually "No" (a False Positive), the translation is broken.
  
  **Answer 7: B) He proved it by simulating a Turing machine's state transitions.**
  *   **The Logic:** You can't use a reduction if there are no known NP-Complete problems to reduce from! Stephen Cook had to do this "from scratch." He proved that any generic algorithm running on a Nondeterministic Turing Machine in polynomial time could be mapped into a giant Boolean logic circuit. This established `CIRCUIT-SAT` as the "Root" of the NP-Complete tree. Everything else was reduced from there.
  
  **Answer 8: B) Create the Complement Graph.**
  *   **The Logic:** This is one of the most elegant reductions in graph theory. A Clique requires *all* edges to exist. An Independent Set requires *no* edges to exist. Therefore, if you take a graph and literally invert it (draw edges where there are gaps, and erase edges that are currently drawn), a Clique of size $k$ in the old graph magically transforms into an Independent Set of size $k$ in the new graph! 
  
  **Answer 9: C) An NP-Hard problem might require checking an exponentially large search space to verify if a proposed optimal solution is truly the best.**
  *   **The Logic:** This is the exact reason the *Optimization* version of TSP is NP-Hard but NOT in NP. If someone hands you a route and says "This is the best route," you cannot verify it quickly. You have to check all other routes ($O(n!)$) to prove it is optimal. 
  
  **Answer 10: C) Undecidable (Uncomputable)**
  *   **The Logic:** NP-Hard problems take a ridiculously long time to solve (millions of years), but they *are* solvable by brute force. The Halting Problem is mathematically **impossible** to solve on any computer, no matter how much time you have. Alan Turing proved this using a famous logical paradox. It sits completely outside the P/NP bubbles.
- ---
- ---
- ---
-