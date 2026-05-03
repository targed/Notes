### 1. The Pseudocode (Slide 8)
Here is the exact algorithm provided in the textbook. It runs in just 4 steps:

```text
APPROX-TSP-TOUR(G, c)
1. select a vertex r ∈ G.V to be a "root" vertex
2. compute a minimum spanning tree T for G from root r using MST-PRIM(G, c, r)
3. let H be a list of vertices, ordered according to when they are first visited 
 in a preorder tree walk of T
4. return the hamiltonian cycle H
```

Let's do a "Deep Dive" into exactly what these steps are doing physically.
- ### 2. Step 1 & 2: The Minimum Spanning Tree (MST)
  *   **The Logic:** If we want to visit every city as cheaply as possible, a great starting point is to just connect all the cities as cheaply as possible without forming any closed loops. This is the exact definition of a **Minimum Spanning Tree (MST)**.
  *   **The Tool:** The algorithm uses Prim's Algorithm (or Kruskal's) to find this MST. 
  *   **The Result:** We now have a "skeleton" or "spine" of roads connecting every city. However, an MST is a tree, not a cycle. It has dead ends (leaves). We can't just drive along the MST and expect to end up back where we started without turning around.
- ### 3. Step 3: The "Full Walk" vs. "Preorder Walk" (Figure 35.2)
  This is where the magic happens. Look closely at the visual example in **Figure 35.2 (Slide 9 & 10)**.
  
  **The "Full Walk" (Tracing the edges):**
  Imagine you trace your finger around the outside of the MST, hugging the edges. 
  *   You start at the root ($a$). 
  *   You go down to $b$, then down to $c$. It's a dead end, so you backtrack to $b$. 
  *   You go down to $h$, backtrack to $b$, backtrack to $a$... and so on.
  *   The "Full Walk" visits cities multiple times: **$a, b, c, b, h, b, a, d, e, f, e, g, e, d, a$**.
  *   *The Problem:* A Hamiltonian Cycle is only allowed to visit each city **exactly once**. The Full Walk is illegal.
  
  **The "Preorder Walk" (Shortcutting):**
  To make it a legal TSP tour, we extract a list of the vertices based *only on when they are first visited* (which is the exact definition of a Preorder Traversal in a tree). 
  *   We cross out all the "backtracking" duplicates from the Full Walk.
  *   If we cross out the duplicates in our list above, we get: **$a, b, c, h, d, e, f, g$**.
  *   *The Physical Action:* By crossing out the duplicates, we are telling the salesperson to take a **shortcut**. Instead of backtracking from $c \to b \to h$, we just draw a straight line directly from $c \to h$. 
  *   *Why this is safe:* Because of the **Triangle Inequality** we learned in Part 1! Taking a direct shortcut from $c \to h$ will *always* be cheaper (or equal to) taking the long way through $b$.
- ### 4. Step 4: The Final Hamiltonian Cycle
  Finally, we just connect the very last city in our Preorder list ($g$) back to the starting root city ($a$) to close the loop. 
  *   We output the final tour $H$.
- ### 5. Time Complexity (Slide 6)
  Why do we love this algorithm? Because it is incredibly fast.
  *   Finding the MST (using Prim or Kruskal) takes polynomial time: $O((V+E) \log V)$.
  *   A Preorder walk takes $O(V)$ time.
  *   **Total Time Complexity:** Dominated by the MST algorithm, meaning it runs in fast, strictly **Polynomial Time**. We completely bypassed the $O(2^n)$ brute-force nightmare!
  
  ---
- ### Part 2 Practice Questions (Tracing the Algorithm)
  
  **Q1: Preorder Traversal Trace**
  Imagine an MST that looks like this:
  *   Root is 1.
  *   1 connects to 2 and 3.
  *   2 connects to 4 and 5.
  If you run `APPROX-TSP-TOUR` on this graph starting at root 1, what is the exact order of the cities in the final Hamiltonian cycle (assuming you visit the left child before the right child)?
  A) 1 $\to$ 2 $\to$ 4 $\to$ 5 $\to$ 3 $\to$ 1
  B) 1 $\to$ 2 $\to$ 3 $\to$ 4 $\to$ 5 $\to$ 1
  C) 4 $\to$ 5 $\to$ 2 $\to$ 3 $\to$ 1 $\to$ 4
  D) 1 $\to$ 2 $\to$ 4 $\to$ 2 $\to$ 5 $\to$ 2 $\to$ 1 $\to$ 3 $\to$ 1
  
  **Q2: The Role of the Triangle Inequality**
  In Step 3, the algorithm "shortcuts" past vertices it has already visited to create a valid tour. If the graph did *not* satisfy the Triangle Inequality, what would happen to the cost of the tour during this shortcutting phase?
  A) The cost would definitely stay the same.
  B) The cost would decrease.
  C) The cost might massively increase, completely ruining our approximation guarantee.
  D) The algorithm would enter an infinite loop.
  
  **Q3: MST Weight vs. Tour Weight**
  Let $W(MST)$ be the total weight of the edges in the Minimum Spanning Tree. 
  Let $W(H)$ be the total weight of the final Hamiltonian cycle produced by `APPROX-TSP-TOUR`. 
  Which of the following must be mathematically true?
  A) $W(H) < W(MST)$
  B) $W(H) \ge W(MST)$
  C) $W(H) = W(MST)$
  
  ---