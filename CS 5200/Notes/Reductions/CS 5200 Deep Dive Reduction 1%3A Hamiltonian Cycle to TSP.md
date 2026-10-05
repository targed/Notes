# Part 3: Hamiltonian Cycle to TSP
- ### 1. The Two Problems (Slides 11–13)
  Before we translate, we need to understand the source and the destination.
  
  *   **The "Known Hard" Problem: Hamiltonian Cycle (HC)**
    *   *The Input:* A standard, unweighted graph. 
    *   *The Question:* Is there a continuous path that visits every single node exactly once and returns to the starting node?
    *   *Status:* Mathematically proven to be **NP-Complete**.
  
  *   **The "New" Problem: Decision TSP**
    *   *The Input:* A fully connected graph where every edge has a "cost" or "weight".
    *   *The Question:* Is there a tour that visits every city once, returns to the start, and has a total cost of $\le K$?
    *   *Status:* We suspect it is NP-Hard, but we need to prove it using HC.
- ### 2. The Reduction Strategy (Slides 15 & 17)
  We must translate HC into TSP (**HC $\le_p$ TSP**). 
  Imagine you only have a software program that solves TSP. I hand you an unweighted graph $G$ and ask, "Does this have a Hamiltonian Cycle?" 
  You must trick your TSP software into answering my question.
  
  **The Preprocessor (Building the TSP Graph $G'$):**
  Your TSP software requires weights, but graph $G$ doesn't have any. So, we construct a brand new graph $G'$:
  1.  **Keep the nodes:** Copy all the nodes from $G$ to $G'$.
  2.  **Make it fully connected:** Draw an edge between *every single pair of nodes* in $G'$, making it a "Complete Graph." (TSP solvers expect to be able to travel anywhere).
  3.  **Assign Weights (The Genius Trick on Slide 17):**
    *   If an edge *actually existed* in the original graph $G$, give it a weight of **$0$**.
    *   If an edge is a *fake edge* that we just added to make the graph complete, give it a weight of **$1$**.
- ### 3. The Subroutine & The Target (Slide 17)
  Now that we have a fully connected, weighted graph $G'$, we feed it into our TSP solver. 
  
  *   **The Target ($K$):** We ask the TSP solver: *"Is there a tour in $G'$ with a total cost of exactly **$0$**?"*
- ### 4. The Postprocessor (Why it works)
  When the TSP solver comes back with a "Yes" or "No", what does that actually mean for our original HC problem?
  
  *   **If TSP says YES (Cost = 0):** 
    Because a tour must visit every node, it must use edges. The only way the total sum of those edges can be $0$ is if the solver exclusively used edges with a weight of $0$. What were the weight $0$ edges? **The real edges from the original graph $G$!** 
    Therefore, a Hamiltonian Cycle definitively exists in $G$.
  *   **If TSP says NO (Cost $\ge 1$):**
    To complete the tour, the TSP solver was forced to use at least one edge with a weight of $1$. What were the weight $1$ edges? The fake ones! This means it was physically impossible to complete a loop using only the real edges. 
    Therefore, a Hamiltonian Cycle does NOT exist in $G$.
  
  **The Conclusion:** Because we successfully tricked a TSP solver into perfectly answering the HC problem, and the translation step (assigning 0s and 1s) was fast (polynomial time), we have officially proven that **TSP is NP-Hard**!
  
  ---
- ### Part 3 Practice Questions (HC to TSP)
  
  **Q1: The "Fake" Edges**
  Why did we have to add "fake" edges to the graph $G'$ during the preprocessing step? Why couldn't we just pass the original graph $G$ with weights of 0 directly into the TSP solver?
  A) Because the TSP solver only accepts positive integers.
  B) Because TSP requires a "Complete Graph" where every city is connected to every other city. If paths are missing, a standard TSP solver might throw an error or crash.
  C) To satisfy the Triangle Inequality.
  D) To make the reduction take exponential time.
  
  **Q2: Alternative Weights**
  On your previous quizzes, a similar reduction was shown where real edges were given a weight of **$1$** and fake edges were given a weight of **$2$**. 
  If you used this $1$ and $2$ weighting scheme for a graph with $V=50$ vertices, what target cost ($K$) must you ask the TSP solver to find?
  A) $K = 0$
  B) $K = 50$
  C) $K = 100$
  D) $K = 1$
  
  **Q3: The Direction Check**
  A classmate looks at this proof and says, *"Wow! We just proved that if we find a fast way to solve the Hamiltonian Cycle problem, we can use it to quickly solve the Traveling Salesperson Problem!"*
  Is your classmate mathematically correct based on the $HC \le_p TSP$ reduction we just performed? Why or why not?
  
  ---