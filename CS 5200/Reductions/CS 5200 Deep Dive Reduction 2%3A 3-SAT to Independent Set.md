# Part 4: 3-SAT to Independent Set (IS)
- ### 1. The Two Problems (Slides 19–25)
  Before we translate, we must understand the source and the destination.
  
  *   **The "Known Hard" Problem: 3-SAT**
    *   *The Input:* A boolean formula made of $m$ clauses. Each clause has 3 literals (variables like $x_1$ or $\neg x_1$) joined by OR ($\lor$). The clauses are joined by AND ($\land$). 
    *   *The Rule:* To satisfy the entire formula, you must make **at least one literal True in every single clause**, without creating a paradox (you can't make $x_1$ True and $\neg x_1$ True at the same time).
    *   *Status:* **NP-Complete** (The first problem ever proven to be NP-Complete by Stephen Cook!).
  
  *   **The "New" Problem: Independent Set (IS)**
    *   *The Input:* An undirected graph.
    *   *The Rule:* Find a set of vertices where **no two vertices touch each other** (no edges between them).
    *   *Status:* We need to prove it is NP-Hard.
- ### 2. The Reduction Strategy (Slides 28–34)
  We must translate 3-SAT into IS (**3-SAT $\le_p$ IS**). 
  Imagine you only have a software program that solves Independent Set. I hand you a boolean formula with $m$ clauses and ask, "Is this formula satisfiable?" 
  
  **The Preprocessor (Building the Graph $G$):**
  We are going to draw a graph that perfectly physically mimics the rules of boolean logic. 
  
  1.  **The Vertices (Slide 28):** For every single literal in the formula, we draw a vertex. If there are $m$ clauses with 3 literals each, we draw exactly **$3m$ vertices**.
  2.  **Edge Type 1: The Clause Gadgets (Slide 29):** Inside every clause, we connect its 3 vertices together to form a **triangle**. 
    *   *The "Deep Dive" Logic:* In an Independent Set, you cannot pick two connected nodes. Because these 3 nodes form a triangle, the IS solver is physically restricted to picking **at most 1 node** per triangle. This perfectly mimics the 3-SAT rule that we need to find 1 true literal per clause!
  3.  **Edge Type 2: The Conflict Edges (Slide 30):** We look across the entire graph. If we see a node representing $x_1$ and another node representing $\neg x_1$, we draw a red "conflict" edge between them.
    *   *The "Deep Dive" Logic:* We cannot allow the IS solver to pick $x_1$ and $\neg x_1$ at the same time, because that is a logical paradox. By drawing an edge between them, we mathematically ban the solver from picking both.
- ### 3. The Subroutine & The Target (Slide 36)
  Now we have our massive, weirdly connected graph. We feed it into our Independent Set solver.
  
  *   **The Target ($m$):** We ask the IS solver: *"Is there an independent set of size exactly **$m$**?"* (where $m$ is the number of clauses).
- ### 4. The Postprocessor (Why it works)
  When the IS solver comes back with a "Yes" or "No", what does that mean for our 3-SAT logic puzzle? (Slide 35).
  
  *   **If IS says YES (Found a set of size $m$):** 
    Because the graph is made of $m$ disjoint triangles, the absolute maximum number of nodes you can pick without violating the triangle edges is $m$. 
    To reach a size of $m$, the solver *must* have successfully picked exactly 1 node from every single triangle! Furthermore, because it didn't violate the "conflict edges", we know the nodes it picked contain no logical paradoxes. 
    **Conclusion:** The 3-SAT formula is satisfiable! Just set the literals the IS solver picked to True.
  *   **If IS says NO (Max size is $< m$):**
    The solver was unable to pick 1 node from every triangle without triggering a conflict edge. This means it is physically impossible to satisfy every clause without creating a paradox. 
    **Conclusion:** The 3-SAT formula is NOT satisfiable.
  
  **The Final Verdict:** Because we successfully tricked an Independent Set solver into perfectly answering a 3-SAT problem, and building the graph was fast (polynomial time), we have officially proven that **Independent Set is NP-Hard**!
  
  ---
- ### Part 4 Practice Questions (3-SAT to IS)
  
  **Q1: Vertex Count Calculation**
  You are translating a 3-SAT boolean formula that has **100 variables** and **400 clauses**. 
  When your preprocessor finishes building the graph for the Independent Set solver, exactly how many **vertices** will be in the graph?
  A) 100
  B) 400
  C) 1200
  D) 300
  
  **Q2: Target Size Calculation**
  Using the exact same formula from Q1 (100 variables, 400 clauses), what target size ($k$ or $m$) must you ask the Independent Set algorithm to find?
  A) 100
  B) 400
  C) 1200
  D) 300
  
  **Q3: The Purpose of the Triangles**
  In the graph construction, why do we connect the 3 literals of a single clause into a triangle?
  A) To make sure the Independent Set solver picks all 3 literals to be True.
  B) To force the Independent Set solver to pick *at most one* literal per clause, mimicking the requirement to find a satisfying True literal for that clause.
  C) To satisfy the Triangle Inequality for the Traveling Salesperson.
  D) To reduce the number of conflict edges needed.
  
  **Q4: The "Domino Effect" Conclusion**
  You just successfully proved that `3-SAT` $\le_p$ `Independent Set`. 
  You already knew that `3-SAT` is NP-Complete. 
  What exact classification can you now officially give to the `Independent Set` problem?
  A) P
  B) NP-Hard
  C) NP-Complete
  D) Unsolvable
  
  ---
- ### **Solutions & Explanations**
  
  **A1: C) 1200 vertices**
  *   *Explanation:* The number of variables (100) doesn't matter for the vertex count! Every single clause has exactly 3 literals. For 400 clauses, we draw $400 \times 3 = 1200$ vertices. 
  
  **A2: B) 400**
  *   *Explanation:* The target size is strictly equal to the number of clauses ($m$). To satisfy the formula, you need 1 True literal for every clause. Since there are 400 clauses, you need to find an Independent Set of size 400.
  
  **A3: B) To force the Independent Set solver to pick at most one literal per clause.**
  *   *Explanation:* If they weren't connected, the IS solver might just pick 3 nodes from Clause 1, 0 nodes from Clause 2, etc. By locking them in a triangle, if the IS solver picks one, it crosses out the other two, forcing it to look at other clauses to reach the target size $m$.
  
  **A4: B) NP-Hard**
  *   *Explanation:* By reducing an NP-Hard problem to IS, we proved IS is **NP-Hard**. 
  *   *Professor Trap Alert:* Is it NP-Complete? Yes, *but* we haven't actually proven it is NP-Complete until we do **Step 1 of the recipe** (Proving IS is in NP by showing a proposed set can be verified in polynomial time). The reduction *only* proves NP-Hardness!
  
  ---