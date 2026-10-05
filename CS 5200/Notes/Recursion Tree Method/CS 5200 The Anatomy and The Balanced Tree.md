### 1. The Concept
A recurrence relation like $T(n) = aT(n/b) + f(n)$ describes an algorithm that breaks a problem into smaller pieces.
*   **$f(n)$**: The "overhead" cost incurred at the current level (Divide + Combine cost).
*   **$a$**: The "Branching Factor" (how many children each node has).
*   **$n/b$**: The problem size reduction.

The Recursion Tree expands this equation into a visual hierarchy where **each node represents the cost ($f(n)$)** of a single sub-problem.
- ### 2. Step-by-Step Construction
  Let's analyze the recurrence from your slides: **$T(n) = 2T(n/2) + n^2$**.
  
  **Step A: The Root (Level 0)**
  *   The top node represents the initial call on input size $n$.
  *   **Cost:** $f(n) = n^2$.
  *   **Children:** It spawns 2 calls.
  
  **Step B: Level 1**
  *   Input size is now $n/2$.
  *   **Cost per node:** $f(n/2) = (n/2)^2 = n^2/4$.
  *   **Total Level Cost:** There are 2 nodes.
    $$ \text{Total} = 2 \times \frac{n^2}{4} = \frac{n^2}{2} $$
  
  **Step C: Level 2**
  *   Input size is now $n/4$.
  *   **Cost per node:** $f(n/4) = (n/4)^2 = n^2/16$.
  *   **Total Level Cost:** There are 4 nodes.
    $$ \text{Total} = 4 \times \frac{n^2}{16} = \frac{n^2}{4} $$
- ### 3. The "Deep Dive" Analysis
  To solve the recurrence, you must answer two specific questions about the tree structure.
- #### Question 1: What is the Tree Height?
  We stop recursion when the input size reaches 1 (the base case).
  *   We divide $n$ by $b$ (in this case, 2) at every step.
  *   $n / 2^h = 1 \implies n = 2^h$.
  *   **Height ($h$):** $\log_2 n$.
  *   *Note:* The levels are numbered 0 to $\log_2 n$.
- #### Question 2: How many Leaves are there?
  *   The number of nodes multiplies by $a$ (in this case, 2) at every step.
  *   At depth $h$, the number of leaves is $a^h$.
  *   **Leaves:** $2^{\log_2 n} = n^{\log_2 2} = n^1 = n$.
  *   *Cost of Leaves:* Usually $O(1)$ per leaf, so total leaf cost is $\Theta(n)$.
  
  ---
- ### 4. Summing the Levels
  The total time $T(n)$ is the sum of costs at every level.
  
  $$ T(n) = \underbrace{n^2}_{\text{Level 0}} + \underbrace{\frac{n^2}{2}}_{\text{Level 1}} + \underbrace{\frac{n^2}{4}}_{\text{Level 2}} + \dots + \underbrace{\Theta(n)}_{\text{Leaves}} $$
  
  Factor out the $n^2$:
  $$ T(n) = n^2 \left( 1 + \frac{1}{2} + \frac{1}{4} + \dots \right) $$
  
  **The Geometric Series:**
  This series $1 + 1/2 + 1/4 \dots$ converges to **2**.
  Even if we add infinite levels, the sum inside the parenthesis will never exceed 2.
  
  **Final Complexity:**
  $$ T(n) = n^2 \times 2 = \Theta(n^2) $$
  
  ---
- ### Part 1 Practice Questions
  
  **Q1: The Branching Factor**
  Draw the first two levels of the recursion tree for **$T(n) = 3T(n/2) + n$**.
  *   How many nodes are at Level 2?
  *   What is the total cost of Level 2?
  
  **Q2: The "Heavy" Leaves**
  In the example above ($2T(n/2) + n^2$), the Root cost ($n^2$) was larger than the Leaf cost ($n$).
  *   What happens if the relation is **$T(n) = 2T(n/2) + \sqrt{n}$**?
  *   Which level dominates the cost now: the Root or the Leaves?
  
  **Q3: Tree Height Nuance**
  If the recurrence is $T(n) = T(n/3) + T(2n/3) + n$, is the tree height uniform? Does every branch reach the bottom at the same depth? (This leads into Part 2).