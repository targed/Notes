### 1. The Two Constraints (The Subproblems)
In the Weighted Independent Set (WIS) problem, our subproblems only shrank in one dimension: the number of items we were allowed to look at ($i$).
In the Knapsack problem, our subproblems shrink in **two dimensions**:
1.  **$i$:** The number of items we are currently considering (from 1 to $n$).
2.  **$c$:** The amount of capacity currently available in the knapsack (from 0 to $C$).

Our subproblem is $V[i, c]$. This represents the **maximum value** we can get using only the first $i$ items, given a knapsack of capacity $c$.
- ### 2. The Thought Experiment (Slide 9 & 11-13)
  Let's look at the very last item in our list: **Item $n$**. (It has value $v_n$ and size $s_n$).
  There is an absolute mathematical tautology for the optimal solution $S$: **Item $n$ is either IN the optimal knapsack, or it is NOT in the optimal knapsack.**
  
  Let's explore both cases to see how they reduce the problem.
- #### **Case 1: Item $n$ is NOT in the optimal solution (Slide 11)**
  If we look at our perfect knapsack $S$ and Item $n$ is nowhere to be found, what does that mean?
  *   **The Logic:** If we didn't use Item $n$, then the optimal solution must be made entirely out of the first $n-1$ items.
  *   **The Subproblem:** The total value is exactly the same as the optimal solution for the first $n-1$ items, using the *exact same* capacity $C$.
  *   **Value of Case 1 = $V[n-1, C]$**
- #### **Case 2: Item $n$ IS in the optimal solution (Slide 12 & 13)**
  If we look at our perfect knapsack $S$ and Item $n$ is sitting right there, what does that mean?
  *   **The Logic:** We gained the value of Item $n$ ($v_n$). BUT, because we put it in the bag, it took up space ($s_n$).
  *   **The Subproblem:** The *rest* of the items in the bag must be the absolute best combination of the first $n-1$ items that can fit into the **remaining, reduced capacity** ($C - s_n$).
  *   **Value of Case 2 = $V[n-1, C - s_n] + v_n$**
  
  *Crucial Caveat:* We can only choose Case 2 if the item actually fits in the current capacity ($s_n \le C$). If the item is heavier than the bag can hold, Case 1 is our *only* option.
- ### 3. The Recurrence Relation (Corollary 16.5)
  We just narrowed down an infinite number of combinations into exactly **two candidates**. Which case is the correct one? **Whichever one gives us the bigger number!**
  
  To find the max value for *any* item $i$ and *any* capacity $c$, we write the following DP Recurrence:
  
  $$ V[i, c] = \begin{cases} 
      V[i-1, c] & \text{if } s_i > c \text{ (Item too heavy, forced Case 1)} \\
      \max(\underbrace{V[i-1, c]}_{\text{Exclude (Case 1)}}, \underbrace{V[i-1, c-s_i] + v_i}_{\text{Include (Case 2)}}) & \text{if } s_i \le c
   \end{cases} $$
- ### 4. The Quiz 16.6 Trap (Slides 14–16)
  Your professor includes a quiz here that perfectly tests if you understand Case 2.
  
  **Question:** If $S$ is the optimal set of items for capacity $C$, and we know Item $n$ is in $S$, which statement holds for the set $S - \{n\}$ (the set without item $n$)?
  
  *   a) It is an optimal solution to the subproblem consisting of the first $n-1$ items and knapsack capacity $C$.
  *   b) It is an optimal solution for the first $n-1$ items and capacity $C - v_n$.
  *   c) It is an optimal solution for the first $n-1$ items and capacity $C - s_n$.
  *   d) It might not be feasible if the capacity is only $C - s_n$.
  
  **The Analysis:**
  *   (a) is wrong. If we take an item out, we free up space. The remaining items don't use the full capacity $C$.
  *   (b) is nonsense. You don't subtract *value* ($v_n$) from *capacity* ($C$). Units don't match.
  *   (d) is wrong. $S$ fit in $C$. If we remove $s_n$, the rest *must* fit in $C - s_n$.
  *   **(c) is Correct.** This is the exact definition of our Case 2 subproblem! The remaining items must be the optimal packing for the remaining space.
  
  ---
- ### Part 2 Practice Questions (DP Recurrence)
  
  **Q1: Translating the Math**
  You are writing a recursive function `knapsack(i, c)`. You are currently evaluating item `i=4` (Value = 10, Size = 5). The current capacity passed into the function is `c=8`.
  Write the exact `max()` function call with the substituted numbers that the algorithm will evaluate to find the best choice.
  
  **Q2: The "Too Heavy" Edge Case**
  You are evaluating item `i=2` (Value = 50, Size = 10). The current capacity is `c=5`.
  According to the recurrence relation, what is the value of $V[2, 5]$?
  A) $V[1, 5] + 50$
  B) $V[1, -5] + 50$
  C) $V[1, 5]$
  D) $0$
  
  **Q3: Overlapping Subproblems**
  If you implemented the recurrence relation $V[i, c] = \max(V[i-1, c], \ V[i-1, c-s_i] + v_i)$ using pure top-down recursion without a cache, the time complexity would be $O(2^n)$.
  Why does standard Divide & Conquer (like Merge Sort) run in $O(n \log n)$ while this runs in $O(2^n)$? What specific property of this recurrence causes the exponential explosion?
  
  ---
-