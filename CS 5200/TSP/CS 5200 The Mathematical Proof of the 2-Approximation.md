- Answers to the **Part 2 Practice Questions**:
  
  **A1: A) 1 $\to$ 2 $\to$ 4 $\to$ 5 $\to$ 3 $\to$ 1**
  *(Explanation: Preorder traversal visits the node, then goes entirely down the left side before touching the right side. It visits 1, then goes to 2, then to 2's children (4, then 5). Finally, it goes back up to visit 1's right child (3), before returning to the start to close the loop).*
  
  **A2: C) The cost might massively increase, completely ruining our approximation guarantee.**
  *(Explanation: This is why the algorithm strictly requires the Triangle Inequality! If a direct flight from $c \to h$ costs \$5,000, but the layover $c \to b \to h$ only costs \$50, "shortcutting" to save a stop would actually bankrupt the salesperson!)*
  
  **A3: B) $W(H) \ge W(MST)$**
  *(Explanation: A Hamiltonian cycle connects all vertices into a loop. If you delete any single edge from that loop, you get a Spanning Tree. Because edge weights are positive, the Spanning Tree weighs strictly less than the cycle. Since the MST is the absolute minimum spanning tree, it must be $\le$ the cycle).*
  
  ***
- ### The Variables
  Let's define our four key players:
  *   **$H^*$** = The absolute perfect, Optimal Hamiltonian Cycle (which we can't easily find).
  *   **$T$** = The Minimum Spanning Tree (MST) we generated in Step 2.
  *   **$W$** = The "Full Walk" tracing up and down every edge of the tree.
  *   **$H$** = Our final Approximate Tour (the preorder shortcut).
  
  We want to prove that: **$c(H) \le 2 c(H^*)$**.
- ### Step 1: Prove the MST is a lower bound. (Equation 35.4)
  **Formula:** $c(T) \le c(H^*)$
  *   *The Logic:* Imagine the perfect optimal tour $H^*$ (a giant loop connecting all cities). If you delete *any single edge* from this loop, the loop breaks, leaving a single continuous path that still connects all cities. By definition, a connected graph with no cycles is a **Spanning Tree**. 
  *   Since all edge weights are positive, deleting an edge means this new spanning tree costs strictly *less* than the full tour $H^*$. 
  *   Because $T$ is the **Minimum** Spanning Tree, it must be the absolute cheapest way to connect all cities. Therefore, $T$ must cost less than or equal to $H^*$.
- ### Step 2: Calculate the "Full Walk" cost. (Equation 35.5)
  **Formula:** $c(W) = 2 c(T)$
  *   *The Logic:* Imagine walking down every branch of the MST and immediately walking back up it. You traverse every single edge in the MST exactly twice (once down, once up). 
  *   Therefore, the cost of this full walk $W$ is exactly double the cost of the tree.
- ### Step 3: Combine Steps 1 and 2. (Equation 35.6)
  **Formula:** $c(W) \le 2 c(H^*)$
  *   *The Logic:* We know $c(T) \le c(H^*)$ (from Step 1). 
  *   If we multiply both sides by 2, we get $2 c(T) \le 2 c(H^*)$. 
  *   Since $c(W) = 2 c(T)$ (from Step 2), we can just substitute $W$ in. 
  *   *Conclusion:* The Full Walk $W$ is guaranteed to cost less than double the optimal tour!
- ### Step 4: The Shortcut. (Equation 35.7)
  **Formula:** $c(H) \le c(W)$
  *   *The Logic:* The Full Walk $W$ isn't a valid TSP tour because it visits cities multiple times. We create our final tour $H$ by taking "shortcuts" (the preorder traversal) to skip already-visited cities.
  *   Because our graph obeys the **Triangle Inequality**, taking a direct shortcut from City A to City C will *always* cost less than (or equal to) taking the long way backtracking through City B. 
  *   Therefore, the final shortcut tour $H$ must cost less than or equal to the Full Walk $W$.
- ### The Grand Conclusion
  Let's string all our inequalities together in one line:
  $$ c(H) \le c(W) = 2 c(T) \le 2 c(H^*) $$
  **Final Result:** **$c(H) \le 2 c(H^*)$**
  
  We have mathematically proven that the cost of our generated tour $H$ will **never exceed twice the optimal cost**. The 2-approximation is guaranteed! ■
  
  ---
- ### Part 3 Practice Questions (The Proof)
  
  **Q1: The Spanning Tree Logic**
  In Step 1 of the proof, we establish that $c(T) \le c(H^*)$. Why is it mathematically impossible for the Minimum Spanning Tree ($T$) to cost *more* than the Optimal TSP Tour ($H^*$)?
  A) Because the MST has more edges than the optimal tour.
  B) Because deleting one edge from $H^*$ creates a valid spanning tree. If $T$ cost more than $H^*$, it wouldn't be the "Minimum" spanning tree!
  C) Because of the Triangle Inequality.
  D) Because $H^*$ uses negative edge weights.
  
  **Q2: The Weight of the Full Walk**
  Why is the cost of the Full Walk exactly $2 \times c(T)$? 
  A) Because it visits exactly twice as many cities.
  B) Because an Euler tour on a doubled tree traverses every physical edge exactly two times.
  C) Because we multiplied the optimal cost by 2.
  D) Because we added fake edges with a weight of 2 to the graph.
  
  **Q3: The Approximation in the Real World**
  You use this `APPROX-TSP-TOUR` algorithm to plan a delivery route for Amazon. Your algorithm spits out a route that costs **$1,500**.
  According to Theorem 35.2, which of the following statements is a **mathematical certainty**?
  A) The absolute perfect optimal route costs exactly $750.
  B) The absolute perfect optimal route costs exactly $3,000.
  C) The absolute perfect optimal route cannot possibly be cheaper than $750.
  D) The absolute perfect optimal route cannot possibly be cheaper than $1,500.
  
  ---