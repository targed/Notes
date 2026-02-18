## 1. Best, Average, and Worst Case (Slides 20–21)
Just saying "This algorithm is $O(N)$" isn't always the whole story. Data sensitivity matters.

*   **Best Case** ($\Omega$ - Omega):** The minimum steps required under ideal conditions.
  *   *Example:* Running a Linear Search for the number `5` in the list `[5, 10, 20]`. You find it instantly. $O(1)$.
  *   *Utility:* Rarely used for comparison because we can't bank on luck.
*   **Average Case** ($\Theta$ - Theta): The expected number of steps over all possible inputs.
  *   *Example:* In Linear Search, on average, the item is in the middle. $N/2$ steps.
  *   *Utility:* Very useful for real-world performance (e.g., Quicksort is usually fast), but mathematically hard to calculate because you need to know the probability distribution of the input data.
*   **Worst Case** ($O$ - Big O): The maximum steps allowed.
  *   *Example:* Linear Search where the item is at the very end or not in the list at all.
  *   *Utility:* This is the standard. It provides a **guarantee**. If your worst case is acceptable, your algorithm is safe to use.
- ### Specific Examples:
  *   **Insertion Sort:**
    *   *Best:* $O(N)$ (Input is already sorted; it just walks through once verifying order).
    *   *Worst:* $O(N^2)$ (Input is reverse sorted; it has to shift every element).
  *   **Quicksort:**
    *   *Average:* $O(N \log N)$. This is why it's the default sort in many languages (C++, Java, Python).
    *   *Worst:* $O(N^2)$. This happens if you pick a bad "pivot" (e.g., always picking the first element in a sorted list).
  
  ---
- ## 2. Algorithmic Paradigms
  An "Algorithmic Paradigm" is a general strategy or pattern for solving problems.
- ### A. Divide and Conquer
  *   **The Strategy:**
    1.  **Divide** the problem into smaller, non-overlapping sub-problems.
    2.  **Conquer** the sub-problems by solving them recursively.
    3.  **Combine** the solutions to solve the original problem.
  *   **Classic Examples:**
    *   **Merge Sort:** Splits list in half, sorts halves, merges them ($O(N \log N)$).
    *   **Quicksort:** Partitions list around a pivot, sorts low/high sides.
  *   **Question Answered:**
    *   *Question:* "Does Selection sort or Insertion sort follow Divide and Conquer?"
    *   *Answer:* **No.**
    *   *Reasoning:* These are **Iterative** (or Incremental) algorithms. They process the list one element at a time, expanding the "sorted section" by one slot. They do not break the problem into independent recursive sub-problems that can be solved in isolation.
- ### B. Greedy Algorithms
  *   **The Strategy:** At every step, make the choice that looks best **at that moment** (locally optimal). Do not worry about the future.
  *   **Classic Example:** **Prim's Algorithm** (Minimum Spanning Tree).
    *   *Goal:* Connect all cities with the cheapest fiber optic cable.
    *   *Greedy Move:* From the city you are currently in, simply pick the cheapest connection to a city you haven't visited yet.
  *   **The Catch:** Greedy algorithms are fast, but they don't always work for every problem (e.g., Chess—taking a pawn now might lose you the game later). We will learn *when* Greedy is safe to use.
  
  ---
- ## 3. Parallel Algorithms
  *   **Sequential:** One instruction at a time (standard CPU coding).
  *   **Parallel:** Multiple instructions at once.
  *   **Context:** Moore's Law (processors getting faster) has slowed down. Now, processors are getting *wider* (more cores).
  *   **Note:** Not everything can be parallelized. If Step B requires the result of Step A, they must run sequentially.
  
  ---
- ## 4. Hardness and Approximation
  How do we classify the "difficulty" of a problem?
- ### A. Tractable vs. Intractable
  *   **Tractable (Easy):** Can be solved in **Polynomial Time**.
    *   $O(N)$, $O(N^2)$, $O(N^{100})$.
    *   As $N$ grows, time grows, but manageable.
  *   **Intractable (Hard):** Solved in **Exponential Time**.
    *   $O(2^N)$, $O(N!)$.
    *   If $N=100$, $2^{100}$ is greater than the number of atoms in the universe. These are effectively unsolvable for large inputs.
- ### B. NP-Hard & Approximation
  *   **The Problem:** Many real-world problems (like the Traveling Salesperson Problem or efficient Routing) are NP-Hard (Exponential). We cannot wait 100 years for the perfect answer.
  *   **The Solution:** **Approximation Algorithms**.
  *   **Trade-off:** We sacrifice **Accuracy** for **Speed**.
    *   Instead of finding the *absolute shortest* path (which takes 100 years), we find a path that is *guaranteed to be within 10%* of the shortest path (which takes 1 second).
  
  ---
- # Summary of Introduction
  We have established the ground rules for the course:
  1.  We define algorithms formally.
  2.  We measure efficiency using **Big O** (Worst Case) because it provides a guarantee.
  3.  We calculate Big O by counting the dominant fundamental operations (usually loops).
  4.  We classify problems into generic strategies (Divide and Conquer, Greedy) and difficulty levels (Polynomial vs Exponential).
  
  ---
- # Final Check for Understanding 
  
  **Scenario:**
  You are analyzing a generic pathfinding algorithm for a GPS.
  
  1.  **Case A:** The map has only 1 road. The algorithm finishes instantly.
  2.  **Case B:** The map is a complex grid. The algorithm explores every possible wrong turn before finding the destination.
  
  **Questions:**
  1.  Which Case represents the **Best Case** and which represents the **Worst Case**?
  2.  When selling this GPS software to a client (e.g., an ambulance service), which Case's run-time should you advertise, and why?