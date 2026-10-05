### 1. The Core Problem (Slides 2-6)
Imagine you are a burglar who has broken into a house. You have a knapsack (backpack) that can hold a maximum weight/size capacity of **$C$**.
There are **$n$ items** scattered around the house.
*   Each item $i$ has a **value ($v_i$)** (e.g., how much it sells for).
*   Each item $i$ has a **size/weight ($s_i$)** (e.g., how much space it takes up in the bag).

**The Goal:** You want to maximize the total value of the items you steal, but the total size of the items must be $\le C$. You cannot take fractions of an item (you either take the whole TV or you leave it). This is why it is called the **0/1 Knapsack Problem**.
- ### 2. Quiz 16.5: A Manual Attempt (Slides 3, 7, 8)
  Your professor asks you to solve a small instance manually to build intuition.
  *   **Knapsack Capacity ($C$):** 6
  *   **Items:**
    1.  Value = 3, Size = 4
    2.  Value = 2, Size = 3
    3.  Value = 4, Size = 2
    4.  Value = 4, Size = 3
  
  **Let's try to solve it intuitively:**
  *   *Attempt A (Take the highest value first):* Items 3 and 4 have the highest value (4 each). Can we take both? Sizes are 2 + 3 = 5. Since $5 \le 6$, yes! Total Value = **8**.
  *   *Attempt B (Take the smallest sizes first):* Items 3 and 4 are also the smallest.
  *   *Attempt C (Take Item 1):* If we take Item 1 (Size 4), we only have 2 capacity left. We can only fit Item 3. Total Value = $3 + 4 = $ **7**.
  
  **Conclusion:** The maximum value is **8** (by taking items 3 and 4). This matches the correct answer **(c)** from the slides.
- ### 3. Why Greedy Algorithms Fail Here
  While the manual example above was easy, what if we had 10,000 items? A common beginner mistake is to try and use a **Greedy Algorithm** (like we used for fractional/continuous problems). 
  *   *The Greedy Logic:* Calculate a "value per pound" ratio ($v_i / s_i$) for every item. Sort them from highest to lowest ratio. Stuff the backpack until it's full.
  *   *Why it fails for 0/1 Knapsack:* Imagine a knapsack of capacity 50. 
    *   Item A: Value 60, Size 30 (Ratio: 2.0)
    *   Item B: Value 40, Size 25 (Ratio: 1.6)
    *   Item C: Value 40, Size 25 (Ratio: 1.6)
    *   A greedy algorithm grabs Item A first because of its great ratio. It now has 20 capacity left. Items B and C don't fit. Total value = **60**.
    *   The *Optimal* solution is to ignore the ratios, skip Item A, and take both Item B and Item C. Total value = **80**.
  
  Because greedy algorithms leave "empty gaps" in the bag that could have been optimized, they cannot guarantee the correct answer. We *must* use Dynamic Programming to evaluate combinations systematically.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1. The 0/1 Restriction**
  Why is this specific variation called the "0/1" Knapsack problem, and how does this restriction specifically break the Greedy "Value-to-Weight Ratio" approach?
  
  **Q2. Brute Force Time Complexity**
  If you decided to ignore Dynamic Programming and just use an exhaustive search (checking every single possible combination of items to see which valid combination gives the highest value), what would the Time Complexity be in terms of $n$?
  A) $O(n^2)$
  B) $O(n^3)$
  C) $O(2^n)$
  D) $O(n!)$
  
  **Q3. Manual Trace**
  You have a knapsack with capacity $C = 5$.
  Item 1: $v=10, s=2$
  Item 2: $v=10, s=3$
  Item 3: $v=15, s=4$
  What is the optimal total value you can fit in the knapsack?
  
  ---
-