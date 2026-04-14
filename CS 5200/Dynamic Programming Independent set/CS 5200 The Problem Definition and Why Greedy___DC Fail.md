### 1. What is an Independent Set? (Slides 2–4)
Imagine a straight line of nodes connected by edges, called a **Path Graph**: `v1 — v2 — v3 — v4`.
*   **The Constraint:** You are selecting a "subset" of these nodes. However, you are **not allowed to pick two nodes that are directly connected by an edge**. 
*   If you pick `v2`, you immediately lock yourself out of picking `v1` and `v3`.
*   **Valid Sets:** `{v1, v3}`, `{v2, v4}`, `{v1, v4}`. 
*   **Invalid Set:** `{v1, v2}` (They are neighbors!).

**Real-World Example (Slides 8-9):** 
*   *Courses:* Nodes are college courses, edges exist if they happen at the exact same time (a schedule conflict). You want to take the most classes without double-booking yourself.
*   *Social Network:* Nodes are people, edges are people who hate each other. You want to invite a group to a party where everyone gets along.
- ### 2. Maximum Weight Independent Set (MWIS) (Slides 5–7)
  Now we add a twist: Every node has a **Weight** (e.g., how many credits a course is worth, or how much you want a specific person at your party).
  *   **The Goal:** Find the valid Independent Set that yields the highest possible total weight.
  
  **The Professor's Example (Slide 6):**
  Graph: `(1) — (4) — (5) — (4)`
  *(Numbers represent weights).*
- ### 3. Attempt #1: The Greedy Strategy (Slides 12–14)
  Greedy algorithms are fast and intuitive. 
  *   **The Logic:** Sort the nodes by weight. Pick the heaviest node first. Cross out its neighbors. Repeat.
  *   **Tracing the Example:**
    1. The heaviest node is `5`. We pick it!
    2. Because we picked `5`, we must cross out its neighbors: the two `4`s.
    3. The only node left is `1`. We pick it.
    4. **Greedy Result:** `{1, 5}`. Total Weight = **6**.
  *   **The Failure:** Look closely at the graph. If we picked the two `4`s instead, they don't touch each other, making them a valid independent set. Their total weight is **8**.
  *   **Conclusion:** As your professor notes on Slide 14, *"Greedy algorithms are usually not correct. Your mom is correct: Don't be always greedy."*
- ### 4. Attempt #2: Divide & Conquer (Slides 15–19)
  Okay, Greedy failed. What about Divide and Conquer? 
  
  *   **The Logic:** Cut the path in half. Find the optimal MWIS of the left half. Find the optimal MWIS of the right half. Combine them.
  *   **Tracing the Example:**
    *   Split `(1) — (4) — (5) — (4)` down the middle.
    *   Left half: `(1) — (4)`. Optimal choice is `{4}`.
    *   Right half: `(5) — (4)`. Optimal choice is `{5}`.
  *   **The Failure (The Merge Step):** We try to combine our answers: `{4, 5}`. But wait! In the original graph, the `4` and the `5` are directly connected to each other! This is an invalid set.
  *   **Conclusion:** D&C fails here because the sub-problems **interfere** with each other at the boundaries. If you try to write code to "fix" the boundary conflicts during the merge step, it becomes an absolute nightmare of overlapping edge cases. 
  
  ---
- ### The Pivot to Dynamic Programming
  As Slide 19 says: *"Divide and Conquer: Unite, don't divide."*
  Because splitting the problem strictly down the middle destroys the context of the boundaries, we need a new paradigm. We need to look at the problem incrementally from left to right, building up the solution step-by-step while keeping our options open.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1: Independent Set Rule**
  Consider the path graph: `A — B — C — D — E`. 
  Is `{A, C, E}` a valid independent set? Is it the *largest possible* independent set by number of nodes?
  
  **Q2: Breaking the Greedy Algorithm**
  Create your own small path graph (just 3 or 4 nodes with weights) where the Greedy approach perfectly finds the optimal solution, and then create one where it fails. 
  
  **Q3: The D&C Flaw**
  In the context of the MWIS problem, why does the professor state on Slide 55: *"If merging subproblems looks tedious... try Dynamic Programming"*? What specifically makes merging the sub-problems tedious in MWIS?
-