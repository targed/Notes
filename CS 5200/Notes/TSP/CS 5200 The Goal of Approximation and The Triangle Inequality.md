# Part 1: Approximation & The Triangle Inequality
*(Covering Slide 7 / Textbook Section 35.2)*
- ### 1. What is an Approximation Algorithm?
  When a problem is NP-Hard, we abandon the goal of finding the *perfect* optimal solution. Instead, we trade **Accuracy for Speed**.
  *   **The Goal:** We want an algorithm that runs fast (in Polynomial Time, like $O(n^2)$) and gives us a solution that is "close enough" to the optimal answer.
  *   **The Guarantee (Approximation Ratio):** We don't just want a random guess; we want a mathematical guarantee. If an algorithm is a **2-Approximation**, it guarantees that the cost of the tour it generates will **never be more than twice the cost of the absolute perfect optimal tour**. 
    *   *Example:* If the true optimal route is 100 miles, a 2-approximation algorithm will run instantly and guarantee a route that is $\le 200$ miles.
- ### 2. The TSP Problem Definition
  As a quick refresher from the text, the Traveling-Salesman Problem gives us:
  *   A **complete undirected graph** $G = (V, E)$. (Every city is connected to every other city).
  *   A nonnegative integer cost $c(u,v)$ for every edge.
  *   **The Objective:** Find a Hamiltonian cycle (a tour that visits every vertex exactly once and returns to the start) with the absolute minimum total cost. Let's call this perfect optimal tour **$H^*$**.
- ### 3. The Crucial Assumption: The Triangle Inequality
  The algorithm we are about to learn *only* works if the graph obeys a strict mathematical rule called the **Triangle Inequality**.
  
  **The Formula:** For all vertices $u, v, w \in V$:
  $$ c(u, w) \le c(u, v) + c(v, w) $$
  
  **The "Deep Dive" Plain English Translation:**
  *   It means that **going directly from Place A to Place C is always cheaper (or equal to) taking a detour through Place B.**
  *   *Why is this realistic?* If you are looking at cities on a physical map, the shortest distance between two points is a straight line (Euclidean distance). A straight line from A to C will always be shorter than going from A to B, and then B to C. 
  *   *When does this fail?* If the "costs" represent airline ticket prices instead of physical distance, the Triangle Inequality often breaks! (Sometimes buying a ticket from NY $\to$ Atlanta $\to$ LA is actually cheaper than a direct flight from NY $\to$ LA).
  
  If the Triangle Inequality holds, it unlocks a massive algorithmic "cheat code": **Shortcutting never increases your cost.** Skipping a middleman will always save you money (or at least break even). We will use this exact trick to build our approximation!
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1: The Approximation Guarantee**
  You run a 2-approximation algorithm for the Traveling Salesperson Problem on a map of 50 cities. The algorithm outputs a tour with a total distance of **800 miles**.
  What can you mathematically conclude about the absolute perfect, optimal tour ($H^*$)?
  A) The optimal tour is exactly 400 miles.
  B) The optimal tour is at least 400 miles.
  C) The optimal tour is at most 400 miles.
  D) The optimal tour is exactly 800 miles.
  
  **Q2: The Triangle Inequality Breakdown**
  Look at a graph with 3 vertices: $X, Y, Z$. 
  The costs are: $c(X,Y) = 5$, $c(Y,Z) = 4$, and $c(X,Z) = 12$. 
  Does this graph satisfy the Triangle Inequality? If not, why?
  
  **Q3: The Consequence of NP-Hardness**
  Why don't we just find the absolute optimal tour $H^*$ instead of settling for an approximation? What would happen if we tried to do that on a graph of 100 cities?
  
  ---