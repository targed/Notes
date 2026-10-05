### 1. The "Greedy" Problem
If you were asked to solve the maximum flow problem intuitively, you might think of a Greedy Algorithm: *"Just find any path from the source to the sink, push as much water as possible through it, and repeat until all paths are blocked."*

**Why does this fail?**
Because a greedy algorithm might make a terrible routing decision early on. It might send flow down a middle pipe that blocks off a route that *could* have carried even more flow if we had just routed things differently. 
To find the true global maximum, we need an **"Undo" button**. We need the algorithm to be able to say, *"I sent 6 crates down this pipe earlier, but I realize now that was a mistake. Let me pull those 6 crates back and send them down a different route."*

Ford-Fulkerson achieves this using a parallel universe called the **Residual Network**.
- ### 2. The Residual Network ($G_f$)
  While our main graph ($G$) tracks the *actual* capacities, the Residual Network ($G_f$) tracks **how much room is left to change things**.
  
  For every edge $(u,v)$ in our real graph, the Residual Network creates **two** edges to represent our options:
  1.  **Forward Edge (Remaining Capacity):** If a pipe holds 10 gallons, and we are currently pushing 6, we have room for 4 more. We draw a forward edge $(u,v)$ with a residual capacity $c_f = 4$.
  2.  **Backward Edge (The Undo Button):** Because we are currently pushing 6 gallons forward, we mathematically have the power to "push back" or "cancel" up to 6 gallons. We draw a backward edge $(v,u)$ with a residual capacity $c_f = 6$.
  
  **The Formal Equation (Page 677):**
  $$ c_f(u,v) = \begin{cases} 
      c(u,v) - f(u,v) & \text{if } (u,v) \in E \\
      f(v,u) & \text{if } (v,u) \in E \\
      0 & \text{otherwise}
   \end{cases} $$
- ### 3. Augmenting Paths and Bottlenecks
  An **Augmenting Path ($p$)** is simply any valid, simple path from the Source ($s$) to the Sink ($t$) inside the *Residual Network* ($G_f$).
  
  *   Once we find a path, we look at the residual capacities of all the edges along that path. 
  *   A path is only as strong as its weakest link. We find the edge with the absolute smallest residual capacity on that path. This minimum value is the **Bottleneck**, denoted mathematically as **$c_f(p)$**.
  *   We can safely increase the total flow of our entire network by exactly $c_f(p)$.
- ### 4. Augmentation (Cancellation)
  When we find an augmenting path, we update the real flow. 
  *   If the path uses a **forward edge** in the residual network, we *add* the bottleneck value to the real flow.
  *   If the path uses a **backward edge** in the residual network, we *subtract* the bottleneck value from the real flow. This is known as **cancellation** (Page 679). By pushing flow backwards, we are effectively rerouting the flow we sent earlier to optimize the whole system!
- ### 5. The Ford-Fulkerson Method (Pseudocode)
  The textbook calls this a "method" rather than an "algorithm" because it doesn't specify *how* you find the augmenting path (you could use DFS, BFS, or random guessing).
  
  **The Logic (Page 686):**
  ```text
  1. Initialize flow f to 0 for all edges.
  2. WHILE there exists an augmenting path p in the residual network G_f:
  3.     Find the bottleneck capacity c_f(p).
  4.     For each edge in the path:
  5.         If it's a forward edge, add flow.
  6.         If it's a backward edge, subtract flow.
  7. Return f
  ```
  The loop just runs over and over, pushing flow and updating the residual network, until the Source is completely cut off from the Sink in the residual network.
  
  ---
- ### Part 2 Practice Questions (Concept Check)
  
  **Q1: Calculating Residual Capacity**
  In your flow network, you have an edge from Node A to Node B. Its capacity is $c(A,B) = 20$. Currently, the flow is $f(A,B) = 15$.
  1. What is the residual capacity $c_f(A,B)$?
  2. What is the residual capacity $c_f(B,A)$?
  3. In plain English, what does the value of $c_f(B,A)$ allow the algorithm to do?
  
  **Q2: Tracing a Bottleneck**
  You find an augmenting path in your residual network: $s \to u \to v \to t$. 
  The residual capacities are:
  *   $c_f(s,u) = 12$
  *   $c_f(u,v) = 4$
  *   $c_f(v,t) = 9$
  1. What is the bottleneck capacity $c_f(p)$ of this path?
  2. By exactly how much will the *total* flow of the network increase after this step?
  
  **Q3: The Cancellation Effect**
  Continuing from Q2, assume the edge $(u,v)$ in the augmenting path is actually a **backward edge** (meaning in the real graph $G$, the pipe goes from $v \to u$). 
  If the current flow in the real graph is $f(v,u) = 10$, what will the new flow $f(v,u)$ be after we augment the path?
  
  ---