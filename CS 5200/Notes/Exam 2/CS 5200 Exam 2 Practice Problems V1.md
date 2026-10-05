### **Practice Exam: Maximum Flow & Edmonds-Karp**

**Question 1: The Capacity vs. Net Flow Trap**
You are analyzing a cut $(S, T)$ in a flow network. 
*   Edge A goes from $S \to T$ with capacity 20 and current flow 15.
*   Edge B goes from $S \to T$ with capacity 10 and current flow 10.
*   Edge C goes from $T \to S$ (backward) with capacity 12 and current flow 4.
1. What is the Capacity of this cut, $c(S,T)$?
2. What is the Net Flow across this cut, $f(S,T)$?

**Question 2: Complexity Memorization**
A flow network has $V$ vertices and $E$ edges. The maximum flow value is $|f^*|$. 
1. What is the worst-case time complexity of the standard Ford-Fulkerson method?
2. What is the worst-case time complexity of the Edmonds-Karp algorithm?
3. What is the space complexity of both algorithms, assuming an adjacency list representation?

**Question 3: Network Transformation (Vertex Splitting)**
You are given a flow network $G = (V,E)$ where, in addition to edge capacities, each **vertex** $v$ has a strict capacity limit $l(v)$ on how much flow can pass through it. 
To run standard Ford-Fulkerson, you must transform $G$ into $G'$ by splitting every vertex into an $in$-node and an $out$-node.
In terms of $V$ and $E$, exactly how many vertices and edges will the new network $G'$ contain?

**Question 4: The Escape Problem (Complexity)**
In the "Escape Problem" (Problem 24-1), you have an $n \times n$ grid with $m$ starting points. You transform this into a flow network using vertex splitting, adding a supersource and a supersink.
Using the Ford-Fulkerson method on this specific grid, what is the tightest worst-case time complexity in terms of $n$?
A) $O(n^2)$
B) $O(n^3)$
C) $O(n^4)$
D) $O(n^6)$

**Question 5: Edmonds-Karp Mechanics (Critical Edges)**
In the Edmonds-Karp algorithm, an edge becomes "critical" if its residual capacity equals the bottleneck of the augmenting path. Once critical, it disappears from the residual network.
According to the textbook, what is the absolute maximum number of times any specific edge $(u,v)$ can become critical during the entire execution of the algorithm?
A) $O(E)$
B) $|V| / 2$
C) $|V| - 2$
D) $|f^*|$

**Question 6: The Pathological Case**
Consider a flow network with 4 nodes forming a diamond shape (Source $s$, intermediate nodes $u$ and $v$, Sink $t$). 
The edges $(s,u), (s,v), (u,t),$ and $(v,t)$ all have a massive capacity of $1,000,000$. 
There is a cross-edge $(u,v)$ with a capacity of $1$.
If standard Ford-Fulkerson makes the worst possible path choices, exactly how many iterations of the `while` loop will it take to find the maximum flow?

**Question 7: Edmonds-Karp Search Property**
The Edmonds-Karp algorithm prevents the pathological infinite-loop scenario by enforcing one strict rule on the Ford-Fulkerson method. 
1. What exact graph traversal algorithm must be used to find the augmenting path?
2. What specific property does this traversal algorithm guarantee about the chosen augmenting path?

**Question 8: Residual Network Trace**
In a flow network, there is an edge $(A, B)$ with a capacity of 30. Currently, the flow $f(A,B) = 18$.
1. In the residual network $G_f$, what is the residual capacity of the forward edge, $c_f(A, B)$?
2. In the residual network $G_f$, what is the residual capacity of the backward edge, $c_f(B, A)$?
3. If an augmenting path pushes 5 units of flow along the *backward* edge $(B, A)$, what is the new actual flow $f(A,B)$ in the original graph?

**Question 9: Antiparallel Edges**
Standard flow networks do not allow antiparallel edges (e.g., $v_1 \to v_2$ with capacity 10, and $v_2 \to v_1$ with capacity 4). 
To fix this, we insert a dummy vertex $v'$ into one of the edges. If we choose to split the edge $(v_1, v_2)$, what will be the capacities of the two new edges $(v_1, v')$ and $(v', v_2)$?

**Question 10: Max-Flow Min-Cut Theorem**
Which of the following conditions is **NOT** mathematically equivalent to the statement "$f$ is a maximum flow in $G$"?
A) The residual network $G_f$ contains no augmenting paths.
B) $|f| = c(S,T)$ for some cut $(S,T)$ of $G$.
C) For every edge crossing the minimum cut, the flow equals the capacity.
D) Every path from the source $s$ to the sink $t$ in the original graph $G$ is utilizing 100% of its bottleneck capacity.

---
---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: Cut Capacity vs. Flow**
  1.  **Capacity $c(S,T)$ = 30.** (We add the capacities of the forward edges: 20 + 10 = 30. **TRAP:** You *never* include backward edges in cut capacity!).
  2.  **Net Flow $f(S,T)$ = 21.** (We add the flow of the forward edges and *subtract* the flow of the backward edges: 15 + 10 - 4 = 21).
  
  **Answer 2: Complexities**
  1.  **Ford-Fulkerson Time:** $O(E|f^*|)$.
  2.  **Edmonds-Karp Time:** $O(VE^2)$.
  3.  **Space (Both):** $O(V + E)$ (to store the original graph and the residual network).
  
  **Answer 3: Vertex Splitting Network Size**
  *   **New Vertices ($|V'|$):** Every original vertex is split into two (an *in* and *out* node). So, $|V'| = \mathbf{2|V|}$.
  *   **New Edges ($|E'|$):** Every original edge is kept, but we must add a brand new internal edge between the *in* and *out* halves of every vertex. So, $|E'| = \mathbf{|E| + |V|}$.
  
  **Answer 4: C) $O(n^4)$**
  *   **Explanation:** The professor explicitly noted this in the Escape Problem slides/document. The grid has $N = n^2$ vertices, so $V = O(n^2)$ and $E = O(n^2)$. The maximum flow is exactly $m$ (the number of starting points). Since $m \le n^2$, we plug this into Ford-Fulkerson: $O(E \cdot |f^*|) \rightarrow O(n^2 \cdot n^2) = \mathbf{O(n^4)}$.
  
  **Answer 5: B) $|V| / 2$ times**
  *   **Explanation:** This comes directly from the Theorem 24.8 proof. When an edge vanishes, it can only reappear if flow is pushed backward across it, which requires the distance from the source to increase by at least 2. Since the maximum distance without cycles is $|V|-2$, an edge can become critical at most $|V|/2$ times. 
  
  **Answer 6: 2,000,000 iterations**
  *   **Explanation:** If the algorithm chooses the path through the cross edge, it pushes exactly 1 unit of flow. Next iteration, it pushes 1 unit backward across the cross edge, cancelling it. It will alternate this way, increasing the total flow by exactly 1 unit per iteration. Since the max flow is 2,000,000 (1M on top, 1M on bottom), it will take exactly 2,000,000 iterations.
  
  **Answer 7: Edmonds-Karp Property**
  1.  It uses **Breadth-First Search (BFS)**.
  2.  It guarantees that we always choose the **shortest augmenting path** (the path with the fewest number of edges).
  
  **Answer 8: Residual Network Trace**
  1.  **$c_f(A, B)$ = 12.** (Remaining capacity: 30 - 18 = 12).
  2.  **$c_f(B, A)$ = 18.** (Undo capacity: we can push back up to 18 units).
  3.  **New Flow = 13.** (Pushing 5 units backward on $(B,A)$ *cancels* 5 units of forward flow on $(A,B)$. $18 - 5 = 13$).
  
  **Answer 9: 10 and 10**
  *   **Explanation:** When splitting an edge to remove an antiparallel conflict, both new edges receive the exact same capacity as the original edge. This ensures the bottleneck of that route remains identical (10). 
  
  **Answer 10: D) Every path from the source to the sink... is utilizing 100% of its bottleneck capacity.**
  *   **Explanation:** A, B, and C are true statements derived from the Max-Flow Min-Cut theorem. D is a classic trap. A maximum flow does *not* mean every possible path is maxed out. It just means a specific "Cut" (a wall of edges across the graph) is 100% saturated, making it impossible to reach the sink. There could be plenty of half-empty paths sitting uselessly behind that cut. 
  
  ---
- ---
- ---
- ### **Practice Exam: Maximum Flow**
  
  **Q1. Algorithmic Distinction & Complexity (2 points)**
  What is the primary characteristic that distinguishes the **Edmonds-Karp** algorithm from the general Ford-Fulkerson method, and how does this change the worst-case time complexity?
  A. It uses Depth-First Search; changes complexity to $O(E|f^*|)$
  B. It uses Breadth-First Search; changes complexity to $O(VE^2)$
  C. It allows for multiple sources and sinks; changes complexity to $O(V^2E)$
  D. It scales capacities by a factor of 2; changes complexity to $O(V+E)$
  
  **Q2. Residual Network Calculations (2 points)**
  In a flow network, you have an edge $(u, v)$ with a maximum capacity of **15**. Currently, a flow of **10** is being pushed through it. 
  In the **residual network** $G_f$, what are the capacities of the forward edge $c_f(u, v)$ and the backward edge $c_f(v, u)$? What specific algorithmic action does the backward edge allow?
  
  **Q3. The Pathological Case (2 points)**
  The Ford-Fulkerson method can perform terribly on certain graphs, taking $O(E|f^*|)$ time. 
  Briefly describe the specific structure of the "pathological graph" that causes this worst-case scenario. Why does the general Ford-Fulkerson method fail to solve it efficiently?
  
  **Q4. Number of Augmentations (1 point)**
  According to the analysis of the Edmonds-Karp algorithm, what is the strict mathematical upper bound on the **total number of flow augmentations** (augmenting paths) the algorithm will perform before terminating?
  A. $O(E)$
  B. $O(V)$
  C. $O(VE)$
  D. $O(E |f^*|)$
  
  **Q5. Manual Trace: Ford-Fulkerson (4 points)**
  You are given a flow network with a source $s$, sink $t$, and two intermediate nodes $A$ and $B$.
  The edge capacities are as follows:
  *   $s \to A$: 10
  *   $s \to B$: 5
  *   $A \to B$: 15
  *   $A \to t$: 5
  *   $B \to t$: 10
  
  **Show the step-by-step execution.** 
  1. List the augmenting paths you choose and the bottleneck flow you push through each.
  2. What is the final Maximum Flow value?
  
  **Q6. Calculating the Min-Cut (2 points)**
  Using the exact same graph and your final flow from **Q5**, define a cut where Set $S = \{s, A\}$ and Set $T = \{B, t\}$.
  1. What is the **capacity** of this cut $c(S,T)$?
  2. What is the **net flow** across this cut $f(S,T)$?
  *(Show your math. Remember the difference between how capacity and net flow treat backward edges!)*
  
  **Q7. Vertex Splitting Reduction (3 points)**
  You are given a network where the **vertices** themselves have capacity limits (e.g., a router can only process 50 packets per second), in addition to standard edge capacities. 
  Explain the exact graph transformation required to solve this using standard Ford-Fulkerson. If the original graph had $V$ vertices and $E$ edges, exactly how many vertices and edges will the new transformed graph have?
  
  **Q8. Maximum Bipartite Matching (2 points)**
  To solve the Maximum Bipartite Matching problem (e.g., assigning $L$ applicants to $R$ jobs), we reduce it to a Maximum Flow problem. 
  When constructing the flow network, what capacity is assigned to the edges connecting the applicants to the jobs, and what capacity is assigned to the edges leaving the supersource?
  
  **Q9. The Integrality Theorem (1 point)**
  True or False: If all edge capacities in a flow network are integers, the Ford-Fulkerson method is mathematically guaranteed to terminate and produce a maximum flow where the flow on every single edge is an integer.
  
  **Q10. Space Complexity (1 point)**
  What is the space complexity of the Edmonds-Karp algorithm, and what specific data structures account for this space?
  
  ---
  ---
- ### **Answer Key & "Professor Deep Dive" Explanations**
  
  **A1. B (It uses Breadth-First Search; changes complexity to $O(VE^2)$)**
  *   **Why:** Ford-Fulkerson is just a "method" because it doesn't specify how to find the path. Edmonds-Karp strictly dictates using **BFS** to find the *shortest path* (in terms of number of edges). This limits the number of augmentations to $O(VE)$, making the total time $O(E \times VE) = O(VE^2)$. 
  
  **A2. Forward $c_f(u, v) = 5$, Backward $c_f(v, u) = 10$.**
  *   **Why:** Forward capacity is remaining space ($15 - 10 = 5$). Backward capacity is the current flow ($10$). 
  *   **Action:** The backward edge allows the algorithm to perform **cancellation**. It allows the algorithm to push flow "backwards" to undo a previously greedy, sub-optimal routing decision.
  
  **A3. The Pathological "Diamond" Graph.**
  *   **Why:** The graph consists of massive capacities on the outer edges (e.g., 1,000,000) but a tiny cross-edge in the middle with a capacity of **1**. If standard Ford-Fulkerson blindly uses DFS, it might choose a path that zigzags across that middle edge. It pushes 1 unit of flow, then on the next turn, uses the backward edge to cancel it, effectively bouncing back and forth 2,000,000 times. Edmonds-Karp prevents this by forcing the shortest path.
  
  **A4. C ($O(VE)$)**
  *   **Why:** This is a specific memorization fact from Theorem 24.8 in your slides/textbook. (The proof involves tracking "critical edges" that disappear and reappear, taking at most $|V|/2$ steps per edge). 
  
  **A5. Manual Trace**
  *(Note: There are multiple valid path choices, but the final max flow will always be the same. Here is the standard BFS/Edmonds-Karp route).*
  *   **Path 1:** $s \to A \to t$. Bottleneck is $\min(10, 5) = \mathbf{5}$.
    *   *Update Graph:* $s \to A$ has 5 left. $A \to t$ has 0 left. 
  *   **Path 2:** $s \to B \to t$. Bottleneck is $\min(5, 10) = \mathbf{5}$.
    *   *Update Graph:* $s \to B$ has 0 left. $B \to t$ has 5 left.
  *   **Path 3:** $s \to A \to B \to t$. Bottleneck is $\min(5, 15, 5) = \mathbf{5}$.
    *   *Update Graph:* $s \to A$ has 0 left. $A \to B$ has 10 left. $B \to t$ has 0 left.
  *   **No paths remaining.** (Source is cut off).
  *   **Final Maximum Flow = 15.** 
  
  **A6. Min-Cut Calculation**
  *   **Set S:** $\{s, A\}$. **Set T:** $\{B, t\}$.
  *   **Edges crossing S to T:** $s \to B$ (Cap 5, Flow 5), $A \to B$ (Cap 15, Flow 5), $A \to t$ (Cap 5, Flow 5). 
  *   **Edges crossing T to S (Backwards):** None.
  *   1. **Capacity $c(S,T)$:** Sum of capacities of forward edges: $5 + 15 + 5 = \mathbf{25}$.
  *   2. **Net Flow $f(S,T)$:** Sum of flow on forward edges minus flow on backward edges: $5 + 5 + 5 - 0 = \mathbf{15}$.
  *   *(Deep Dive: Notice $|f| = f(S,T) = 15$. The net flow perfectly equals our max flow from Q5, but the capacity is 25. This means this specific cut is NOT the minimum cut).*
  
  **A7. Vertex Splitting**
  *   **The Transformation:** For every vertex $v$, we split it into an "in-node" ($v_{in}$) and an "out-node" ($v_{out}$). We draw a directed edge from $v_{in}$ to $v_{out}$ with a capacity exactly equal to the vertex's capacity. All original incoming edges connect to $v_{in}$, and all outgoing edges leave from $v_{out}$.
  *   **New Vertices:** $2V$ (Every vertex is split in two).
  *   **New Edges:** $E + V$ (We keep all original edges, plus we add 1 new internal edge for every vertex).
  
  **A8. Capacity is 1 for both.**
  *   **Why:** In Bipartite Matching, we want one applicant to match to exactly one job. By putting an edge capacity of **1** leaving the supersource to each applicant, we restrict each applicant to 1 job. By putting a capacity of **1** between the applicant and the job, and **1** into the supersink, we restrict each job to being filled exactly once. 
  
  **A9. True**
  *   **Why:** This is the Integrality Theorem. Because the algorithm only ever adds or subtracts bottlenecks (which are integers), it will never create a fractional flow. 
  
  **A10. Space Complexity: $O(V + E)$**
  *   **Why:** We must store the vertices and edges of the flow network in an Adjacency List. We also must store the Residual Network, which has at most $2E$ edges (one forward, one backward). $O(V + 2E)$ simplifies to $O(V + E)$. 
  
  ---
- ---
- ---
- ### **Practice Set: Tracing Maximum Flow**
  
  **Question 1: The Edmonds-Karp Rule**
  You are given a flow network with the following nodes: $s$ (source), $A$, $B$, $t$ (sink). 
  The edges and capacities are:
  *   $s \to A$: 10
  *   $s \to B$: 10
  *   $A \to B$: 10
  *   $A \to t$: 10
  *   $B \to t$: 10
  
  You are strictly using the **Edmonds-Karp** algorithm. What is the **very first** augmenting path the algorithm will choose, and why? 
  A) $s \to A \to B \to t$
  B) $s \to A \to t$
  C) $s \to B \to A \to t$
  
  **Question 2: The "Cancellation" Trace (Crucial for the Exam)**
  Let's use a graph designed to force a mistake. You have nodes: $s, A, B, t$. 
  The edge capacities are:
  *   $s \to A$: 10
  *   $s \to B$: 5
  *   $A \to B$: 10
  *   $A \to t$: 5
  *   $B \to t$: 10
  
  **Step 1:** You act greedily and choose the path **$s \to A \to B \to t$**. 
  *   What is the bottleneck of this path? 
  *   Push this flow. Write down the new residual capacities (forward and backward) for $s \to A$, $A \to B$, and $B \to t$.
  
  **Step 2:** Look at your newly updated residual network. Can you find another path from $s$ to $t$? 
  *   Write down the exact nodes in this second path. *(Hint: It MUST use a backward edge!)*
  *   What is the bottleneck of this second path?
  *   Push the flow and calculate the **Final Maximum Flow** of the network.
  
  **Question 3: Checking Flow Conservation**
  Let's verify your work from Question 2. In the real-world graph (not the residual one), what is the final, absolute flow traveling across the pipe **$A \to B$** after both steps are completed? Prove that **Node A** obeys the Law of Flow Conservation.
  
  **Question 4: Finding the Min-Cut from the Residual Graph**
  Using the final, completed residual network from Question 2 (after both paths have been pushed):
  1. To find the Minimum Cut, we define Set $S$ as all nodes that can still be reached from the source $s$ in the residual network. What nodes are in Set $S$?
  2. What nodes are in Set $T$?
  3. Calculate the Capacity of the cut $c(S,T)$ based on the original graph. Does it equal your Maximum Flow?
  
  **Question 5: Bipartite Matching to Max Flow**
  You are a professor trying to assign 3 TAs to 3 Lab Sections. 
  *   **TA 1** can teach Lab A.
  *   **TA 2** can teach Lab A or Lab B.
  *   **TA 3** can teach Lab B or Lab C.
  
  1. Describe exactly how you transform this into a Maximum Flow graph. What are the edges and capacities?
  2. By running Ford-Fulkerson in your head, what is the maximum number of labs that can be covered?
  
  **Question 6: The Pathological Update**
  In the classic "pathological" diamond graph (where outer edges have 1,000,000 capacity and the cross-edge $A \to B$ has a capacity of 1), you push 1 unit of flow along $s \to A \to B \to t$. 
  Immediately after this step, what is the residual capacity of the **backward edge** from $B \to A$, and what does this specific number represent?
  
  ---
  ---
- ### **Solutions & Step-by-Step Walkthroughs**
  
  **Answer 1: B) $s \to A \to t$ (or $s \to B \to t$)**
  *   **Explanation:** The defining rule of Edmonds-Karp is that it uses **Breadth-First Search (BFS)** to find the augmenting path. BFS strictly explores paths based on the *fewest number of edges*. 
  *   Paths $s \to A \to t$ and $s \to B \to t$ both require only **2 edges**. 
  *   The path $s \to A \to B \to t$ requires **3 edges**. Edmonds-Karp will *never* choose the 3-edge path on the first turn. (This is how it avoids the infinite zigzag loop).
  
  ---
  
  **Answer 2: The "Cancellation" Trace**
  This is the most important tracing exercise you can do!
  
  **Step 1:**
  *   Path: $s \to A \to B \to t$. Capacities are (10, 10, 10).
  *   **Bottleneck:** 10. 
  *   *Updates:* We push 10 units. 
    *   $s \to A$: Forward capacity becomes $10-10 = \mathbf{0}$. Backward capacity becomes $\mathbf{10}$.
    *   $A \to B$: Forward capacity becomes $10-10 = \mathbf{0}$. Backward capacity becomes $\mathbf{10}$.
    *   $B \to t$: Forward capacity becomes $10-10 = \mathbf{0}$. Backward capacity becomes $\mathbf{10}$.
  
  **Step 2:**
  *   We need a new path from $s$ to $t$. 
    *   Can we go $s \to A$? No, forward capacity is 0.
    *   We must go $s \to B$ (residual capacity is 5). 
    *   From $B$, can we go to $t$? No, forward capacity is 0. 
    *   But wait! Look at edge $A \to B$. We established it has a **backward capacity of 10**. This means in the residual network, we have a valid arrow from **$B \to A$**!
  *   **The New Path:** **$s \to B \to A \to t$**.
  *   Let's check the bottleneck of this path in the residual network:
    *   $s \to B$: capacity 5
    *   $B \to A$: capacity 10 (the undo button)
    *   $A \to t$: capacity 5
  *   **Bottleneck:** $\min(5, 10, 5) = \mathbf{5}$.
  *   We push 5 units.
  *   **Final Maximum Flow:** $10 + 5 = \mathbf{15}$.
  
  ---
  
  **Answer 3: Checking Flow Conservation**
  *   *What is the final flow on the pipe $A \to B$?* 
    *   In Step 1, we pushed 10 units forward. 
    *   In Step 2, we pushed 5 units *backward* (which cancels out 5 units of the forward flow).
    *   Final physical flow on $A \to B$ is **$10 - 5 = 5$ units**.
  *   *Checking Node A's Conservation:*
    *   **Flow IN:** Edge $s \to A$ has 10 units flowing in.
    *   **Flow OUT:** Edge $A \to B$ has 5 units. Edge $A \to t$ has 5 units. Total out = $5 + 5 = 10$.
    *   **10 in = 10 out.** Conservation holds perfectly! The math works.
  
  ---
  
  **Answer 4: Finding the Min-Cut from the Residual Graph**
  *   **1. Find Set S:** Look at the source $s$ in the final residual graph. Can it reach anywhere?
    *   $s \to A$ has 0 forward capacity.
    *   $s \to B$ has 0 forward capacity.
    *   Since $s$ cannot reach *any* other node, **Set $S = \{s\}$**.
  *   **2. Find Set T:** Everything else! **Set $T = \{A, B, t\}$**.
  *   **3. Calculate Cut Capacity $c(S,T)$:** 
    *   Look at the original graph. Which edges cross from $S$ to $T$?
    *   Edge $s \to A$ (Capacity 10).
    *   Edge $s \to B$ (Capacity 5).
    *   Total Capacity = $10 + 5 = \mathbf{15}$. 
    *   **Does it equal Max Flow?** Yes! Max Flow (15) = Min Cut (15). The theorem is proven!
  
  ---
  
  **Answer 5: Bipartite Matching to Max Flow**
  *   **1. Transformation:**
    *   Add a **Supersource ($s$)**. Draw edges from $s$ to TA 1, TA 2, and TA 3. Give each edge a capacity of **1**.
    *   Draw the "matching" edges from TAs to Labs based on the rules (TA 1 $\to$ Lab A, TA 2 $\to$ Lab A, etc.). Give all these edges a capacity of **1**.
    *   Add a **Supersink ($t$)**. Draw edges from Lab A, Lab B, and Lab C to the sink $t$. Give each edge a capacity of **1**.
  *   **2. Tracing the Flow:**
    *   Path 1: $s \to \text{TA 1} \to \text{Lab A} \to t$. (Push 1. Lab A is filled).
    *   Path 2: $s \to \text{TA 2} \to \text{Lab B} \to t$. (Push 1. Lab B is filled).
    *   Path 3: $s \to \text{TA 3} \to \text{Lab C} \to t$. (Push 1. Lab C is filled).
    *   **Max Flow = 3.** All three labs are successfully covered! 
    *   *(Note: The capacity of 1 forces the algorithm to assign exactly one TA to exactly one unique lab).*
  
  ---
  
  **Answer 6: The Pathological Update**
  *   The original forward edge $A \to B$ had a capacity of 1. We pushed 1 unit of flow across it.
  *   The residual capacity of the backward edge $B \to A$ becomes **1**.
  *   **What this represents:** It represents the algorithm's power to **cancel** or **undo** that 1 unit of flow on its next turn. Because the backward edge exists, the algorithm will route flow $s \to B \to A \to t$, utilizing the backward edge to cancel the 1 unit it just pushed, bouncing back and forth forever.
  
  ---