### **5.2 Routing Algorithms**
  
  We will now examine the two classical approaches to determining the least-cost path in a network graph.
#### **1. The Link-State (LS) Routing Algorithm**
  
  *   **Core Idea:** Every router has a complete map of the network. Each router then independently runs an algorithm to calculate the shortest path from itself to all other routers.
  *   **Information Scope:** **Global**. Every router knows the full network topology (all routers and all links) and the cost of every link. This is achieved by having each router broadcast its local link-state information to all other routers in the network.
  *   **Centralized Computation:** Although the information is global, the computation is **decentralized**. Each router performs the same calculation for itself.
  *   **The Algorithm:** The de facto algorithm for Link-State routing is **Dijkstra's Algorithm**.
  
  **Dijkstra's Algorithm**
  
  Dijkstra's algorithm is an iterative algorithm that finds the least-cost paths from a single source node (`u`) to all other nodes in the graph.
  
  *   **Notation:**
    *   `c(x,y)`: Cost of the link between nodes x and y (infinity if not directly connected).
    *   `D(v)`: The current estimate of the least-cost path from the source `u` to a destination node `v`.
    *   `p(v)`: The predecessor node to `v` on the current best path from the source.
    *   `N'`: The set of nodes for which the least-cost path is definitively known.
  
  *   **The Algorithm Steps (starting from source node `u`):**
    1.  **Initialization:**
        *   Initialize the set `N'` to contain only the source node `u`.
        *   For all other nodes `v`, set `D(v)` to the direct link cost `c(u,v)`. If `u` is not directly connected to `v`, set `D(v)` to infinity.
  
    2.  **Loop (iterate until `N'` includes all nodes):**
        *   **Find the next closest node:** Find the node `w` that is *not* in `N'` and has the minimum `D(w)` value.
        *   **Add it to the set:** Add `w` to the set `N'`.
        *   **Update neighbors:** For each neighbor `v` of `w` that is *not* in `N'`, update its path cost with the following rule:
            $ D(v) = \min( D(v), D(w) + c(w,v) ) $
            This step asks: "Is the path to `v` through our newly found node `w` shorter than the best path we knew about before?"
  
  *   **Output:** After the algorithm terminates, for each node `v`, `D(v)` will be the cost of the least-cost path from the source `u`, and `p(v)` will be its predecessor. By tracing the predecessors back from any destination, we can construct the entire shortest-path tree from `u`. This tree is then used to build the forwarding table for router `u`.
  
  *   **Complexity:**
    *   **Computational Complexity:** A simple implementation has a complexity of **O(n²)**, where n is the number of nodes. More efficient implementations using heaps can achieve **O(n log n)**.
    *   **Message Complexity:** Each of the n routers must broadcast its link-state information to all other routers. This can lead to a large number of messages, especially when links frequently change state.
  
  ---
#### **2. The Distance-Vector (DV) Routing Algorithm**
  
  *   **Core Idea:** "Tell your neighbors what you know about the world." A router knows the cost to its neighbors and receives estimates from its neighbors about the cost to all other destinations. It uses this information to calculate its own best path.
  *   **Information Scope:** **Decentralized**. Each router only knows the costs of its direct links and the information it receives from its neighbors.
  *   **The Algorithm:** The DV algorithm is based on the **Bellman-Ford equation**, a classic dynamic programming principle.
  
  **The Bellman-Ford Equation**
  
  Let $D_x(y)$ be the cost of the least-cost path from node `x` to node `y`. The Bellman-Ford equation states that this cost can be calculated as:
  $ D_x(y) = \min_{v} \{ c(x,v) + D_v(y) \} $
  
  *   **Explanation:** "The cost of my path from `x` to `y` is the minimum of (the cost to get to my neighbor `v`, plus my neighbor `v`'s estimated cost to get to `y`)". This minimization is performed over all of `x`'s neighbors.
  
  **The Distance-Vector Algorithm in Action**
  
  *   **Distance Vector:** Each router `x` maintains its own **distance vector**, which is a list of its current best estimates of the cost to every other destination `y` in the network.
  *   **Iterative and Asynchronous:**
    1.  Periodically, each node sends its own distance vector to all of its directly connected neighbors.
    2.  When a node `x` receives a new distance vector from a neighbor `v`, it saves it.
    3.  It then re-computes its own distance vector by applying the Bellman-Ford equation for every destination `y`, using the known link cost `c(x,v)` and the newly received distance vector from `v`.
    4.  If its own distance vector has changed as a result of this computation, it sends its new, updated distance vector to all of its neighbors.
  *   **Self-Stopping:** If a node's distance vector does not change after a re-computation, it has no new information to share and does not send any messages. The algorithm naturally stops when no more updates are being sent.
  
  **Link Cost Changes and Routing Problems**
  
  *   **"Good News Travels Fast":** When a link cost *decreases*, the update propagates through the network very quickly. The new, shorter path is found rapidly as each node informs its neighbors.
  *   **"Bad News Travels Slow" (The Count-to-Infinity Problem):** When a link cost *increases* or a link fails, the DV algorithm can converge very slowly and can create **routing loops**.
    *   **Scenario:** A path from Y to X has a cost of 4. The direct link from Y to X fails. Y's neighbor, Z, still thinks it has a path to X (through Y) with a cost of 5. Y now sees that it can get to X through Z with a cost of 1 (to Z) + 5 (Z to X) = 6. Y updates its cost to 6. Z receives this update and recalculates its path to X through Y as 1+6=7. This process continues, with the nodes bouncing the bad information back and forth, counting up to infinity (or until the TTL expires).
    *   **Solutions:** Techniques like "poisoned reverse" can help mitigate this problem in some, but not all, cases.
#### **Comparison of LS and DV Algorithms**
  
  | Feature | Link State (LS) | Distance Vector (DV) |
  | :--- | :--- | :--- |
  | **Information Scope** | Global (all routers know the full topology) | Decentralized (each router knows only its neighbors) |
  | **Message Complexity**| High when link states change (`O(n²)` messages) | Can be high on convergence, but low when stable |
  | **Speed of Convergence** | Relatively fast; `O(n log n)` computation | Can be very slow; subject to count-to-infinity |
  | **Robustness** | A misbehaving router can broadcast incorrect link costs, but each router calculates its own table, limiting the damage. | A misbehaving router can advertise incorrect path costs, and this error can propagate through the entire network ("black-holing"). |