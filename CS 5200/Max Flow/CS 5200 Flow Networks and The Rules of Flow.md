### 1. What is a Flow Network?
A flow network is a directed graph $G = (V, E)$ used to model material coursing through a system. 
*   **The Source ($s$):** The vertex where material is produced at a steady rate.
*   **The Sink ($t$):** The vertex where the material is consumed.
*   **Capacity ($c(u,v)$):** Every directed edge $(u,v)$ has a strictly non-negative capacity representing the maximum rate at which material can flow through it (e.g., 200 gallons of water per hour, or 10 megabytes of data per second). If an edge doesn't exist, its capacity is 0.
- ### 2. The Two Sacred Rules of Flow
  To solve the Maximum Flow problem, we must assign a **flow value $f(u,v)$** to every edge. A valid flow assignment *must* obey two absolute laws of physics:
  
  1.  **Capacity Constraint:** 
    $$ 0 \le f(u,v) \le c(u,v) $$
    *Translation:* You cannot have negative flow, and you cannot push 15 gallons of water through a 10-gallon pipe.
  2.  **Flow Conservation:**
    $$ \sum_{v \in V} f(v,u) = \sum_{v \in V} f(u,v) $$
    *Translation:* For every single vertex in the graph *except* the source and the sink, the total flow entering the vertex must perfectly equal the total flow leaving it. "Flow in equals Flow out." Nodes do not magically create or store water.
  
  **The Goal:** Find a valid flow assignment that maximizes **$|f|$** (the total value of the flow). $|f|$ is defined as the total flow leaving the source minus any flow returning to the source.
- ### 3. Graph Modifications (The "Deep Dive" Tricks)
  Real-world problems don't always look like perfect flow networks. The textbook highlights two specific scenarios where we must mathematically "hack" the graph to make the algorithms work.
- #### A. Modeling Antiparallel Edges (Page 673)
  Standard flow networks strictly forbid **antiparallel edges** (where an edge goes from $u \to v$, and another edge goes straight back from $v \to u$). 
  *   *Why?* Because it breaks the math of the "Residual Network" we will build in Part 2.
  *   *The Fix:* If a company wants to ship 10 crates from Edmonton to Calgary, and 10 crates from Calgary to Edmonton, we use **Vertex Splitting**. We add a new, dummy vertex $v'$ in the middle of one of the edges. 
  *   We replace $(v_1, v_2)$ with $(v_1, v')$ and $(v', v_2)$. Both new edges get the exact same capacity as the original edge. This eliminates the antiparallel conflict without changing the bottleneck!
- #### B. Multiple Sources and Sinks (Page 674)
  What if the "Lucky Puck Company" has 3 factories (sources) and 2 warehouses (sinks)? The math assumes exactly *one* source $s$ and *one* sink $t$.
  *   *The Fix:* We create a **Supersource** and a **Supersink**.
  *   We draw a directed edge from the Supersource to the 3 actual factories, giving each edge a capacity of $\infty$ (infinity).
  *   We draw a directed edge from the 2 actual warehouses to the Supersink, also with a capacity of $\infty$. 
  *   *Result:* The algorithm now operates on a single source and sink, and the infinite capacity ensures the dummy edges never become bottlenecks.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1: Flow Conservation**
  In a flow network, Vertex $X$ (which is not the source or sink) has three incoming edges with flows of 5, 10, and 2. It has two outgoing edges. One of the outgoing edges currently has a flow of 8. What *must* the flow of the second outgoing edge be?
  
  **Q2: Antiparallel Edges**
  You are writing a maximum flow algorithm and notice the graph has edges $A \to B$ (Capacity 15) and $B \to A$ (Capacity 5). 
  Describe exactly how you would modify the graph by adding a dummy vertex $X$ so that the algorithm can process it. (State the new edges and their capacities).
  
  **Q3: Problem 24.1-3 (From the Textbook Exercises)**
  Suppose a flow network $G$ has a vertex $u$ for which there is absolutely no path from the source $s$ to $u$. What can you mathematically conclude about the flow $f(u,v)$ for any edge leaving $u$? Why?