### 1. The Setup (The Premise of the Puzzle)
Imagine an $n \times n$ grid of city streets. 
*   There are $m$ starting points inside this grid (let's call them bank robbers). 
*   They all want to escape the city by reaching any point on the boundary of the grid.
*   **The Catch:** The paths they take to the boundary must be **vertex-disjoint**. This means no two robbers can ever pass through the exact same intersection. If one robber uses an intersection, it is locked down.

We need to design an algorithm to find out if it is mathematically possible for all $m$ robbers to escape at the same time.
- ### 2. Solving Part A: Vertex Capacities
  Standard flow networks (like the ones Ford-Fulkerson uses) only have **edge capacities** (e.g., a pipe can hold 10 gallons). 
  
  But in our escape problem, the intersections (vertices) are the bottleneck. A vertex can only handle 1 robber. This means we have a **vertex capacity of 1**. 
  Part A asks: *How do we reduce a network with vertex capacities into an ordinary network with only edge capacities?*
  
  **The Solution: Vertex Splitting**
  We use a brilliant trick called "Vertex Splitting." We literally rip every intersection in half.
  1.  For every original vertex $v$, we delete it and replace it with two new nodes: an **in-node ($v_{in}$)** and an **out-node ($v_{out}$)**.
  2.  We draw a directed edge from $v_{in}$ to $v_{out}$, and we set the capacity of this internal edge to exactly the vertex capacity (which is **1** for our robbers).
  3.  Any edges that originally entered $v$ are now plugged into $v_{in}$.
  4.  Any edges that originally left $v$ are now drawn leaving from $v_{out}$.
  
  *Result:* By forcing all traffic passing through the intersection to travel across this internal edge of capacity 1, we have successfully modeled a vertex constraint using a standard edge capacity!
- ### 3. Solving Part B: Building the Network
  Now we need to build the macroscopic flow network to solve the whole puzzle. 
  
  1.  **Split the Grid:** We apply the Vertex Splitting trick to every single node in the $n \times n$ grid. Internal edges get a capacity of 1. The street edges connecting different grid points can get a capacity of 1 (or $\infty$, since the internal node edge is already bottlenecking it to 1).
  2.  **Add a Supersource ($s$):** We create a master starting node $s$. We draw directed edges from $s$ to the $v_{in}$ nodes of all $m$ starting points, each with a capacity of 1.
  3.  **Add a Supersink ($t$):** We create a master ending node $t$. We draw directed edges from the $v_{out}$ nodes of every single grid boundary point to $t$, each with a capacity of 1.
- ### 4. The Algorithm and Time Complexity
  **The Algorithm:**
  Now that we have a standard flow network with a single source $s$ and a single sink $t$, we simply run the **Ford-Fulkerson algorithm**.
  *   If the resulting maximum flow is exactly equal to **$m$**, then all $m$ robbers successfully escaped! (Because 1 unit of flow successfully reached the sink for every starting point).
  
  **The Time Complexity Analysis (The Math):**
  We know the basic Ford-Fulkerson time complexity is **$O(E \cdot |f^*|)$**. Let's plug in our variables:
  *   **Edges ($E$):** The original grid has $n^2$ vertices. Each vertex connects to at most 4 neighbors. When we split the vertices and add the supersource/sink, the total number of edges scales proportionally to $n^2$. Therefore, $E = O(n^2)$.
  *   **Max Flow ($|f^*|$):** The maximum possible flow is exactly $m$ (the number of starting points). 
  *   **Calculation:** Multiply the edges by the max flow: $O(E \cdot |f^*|) = \mathbf{O(n^2 \cdot m)}$. 
  
  *The "Deep Dive" Final Bound:* The problem states that $m \le n^2$ (you can't have more robbers than intersections). Therefore, in the absolute worst-case scenario where the grid is packed with robbers, $m = n^2$. 
  This gives a final worst-case running time of $O(n^2 \cdot n^2) = \mathbf{O(n^4)}$. 
  
  For a complex routing puzzle, a polynomial time of $O(n^4)$ is incredibly efficient!
  
  ---
- ### Part 5 Practice Questions (Concept Check)
  
  **Q1: The Supersource Goal**
  In the Escape Problem, we connected the Supersource to the $m$ starting locations with an edge capacity of 1. What would happen to the algorithm if we accidentally gave those edges a capacity of $\infty$? Could one robber essentially spawn clones of himself?
  
  **Q2: Vertex Splitting Limits**
  Imagine a computer network where a specific router (a vertex) can only process 50 megabytes of data per second. Using the Vertex Splitting technique, what exactly would be the capacity of the edge $(v_{in}, v_{out})$ for this router?
  
  **Q3: Complexity Comparison**
  In the Escape problem, why did we use the Ford-Fulkerson complexity bound $O(E \cdot |f^*|)$ instead of the Edmonds-Karp bound $O(V \cdot E^2)$? 
  *(Hint: Calculate $O(V \cdot E^2)$ for this grid. Since $V = O(n^2)$ and $E = O(n^2)$, what power of $n$ does Edmonds-Karp give you? Why is Ford-Fulkerson actually faster here?)*
  
  ---