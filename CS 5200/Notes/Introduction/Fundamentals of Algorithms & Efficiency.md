## 1. What is an Algorithm?
**Slide Concept:** A sequence of steps to solve a problem within a finite amount of time, given valid input.

**Comprehensive Expansion:**
While the slides give a high-level definition, in computer science, an algorithm must strictly adhere to five criteria (often referenced from Knuth):
1.  **Input:** It has 0 or more well-defined inputs.
2.  **Output:** It produces 1 or more outputs.
3.  **Definiteness:** Each step must be clear and unambiguous. (e.g., "Add 5 to x" is definite; "Choose a nice number" is not).
4.  **Finiteness:** The algorithm must eventually stop (halt). An infinite loop is not an algorithm; it is a computational procedure.
5.  **Effectiveness:** Each step must be basic enough to be carried out, in principle, by a person using a pencil and paper.
- ## 2. Efficiency Metrics
  We evaluate algorithms based on Run-time, Memory space, and Data movement.
  
  **Comprehensive Expansion:**
  *   **Time Complexity (Run-time):** This is usually the primary concern. We don't measure this in *seconds* (which varies by computer); we measure it in the **number of operations** relative to the input size ($N$).
  *   **Space Complexity (Memory):** How much RAM does the algorithm need?
    *   *In-place algorithms* use constant extra space $O(1)$.
    *   *Out-of-place algorithms* might copy data, requiring $O(N)$ space.
  *   **Data Movement (The Hidden Cost):** The slide mentions "slow memory to fast memory." This refers to the **Memory Hierarchy**.
    *   Moving data from the Hard Drive $\to$ RAM $\to$ L1/L2 Cache $\to$ CPU registers is expensive.
    *   *Cache Locality:* Algorithms that access memory sequentially (like iterating through an array) are faster than those jumping around randomly (like a linked list) because the CPU can predict and load data into the fast "Cache" ahead of time.
- ## 3. The Size of Input ($N$)
  **Slide Concept:** We express complexity as a function of the problem size, $N$.
  
  **Examples from Slides:**
  *   **Sequential Search (Linear Search):** Looking for a number in an unsorted list.
    *   *Scenario:* You have to check every single number until you find the match.
    *   *Complexity:* $O(N)$. If you have 10 items, it takes 10 steps. If you have 1,000,000, it takes 1,000,000 steps.
  *   **Binary Search:** Looking for a number in a **sorted** list (like a dictionary).
    *   *Scenario:* You check the middle. If your target is smaller, you ignore the right half. You keep cutting the problem in half.
    *   *Complexity:* $O(\log_2 N)$.
    *   *The Math:* $2^x = N$.
        *   If $N = 1024$, how many times can you divide by 2 until you reach 1?
        *   Answer: 10 times ($2^{10} = 1024$).
        *   This is drastically faster than $O(N)$. For 1 billion items, Linear search takes 1 billion steps; Binary search takes only ~30 steps.
- ## 4. Measuring Efficiency: Theoretical vs. Experimental
  Why not just run the code (Experimental) vs. using a model (Theoretical)?
  
  **Comprehensive Expansion (The "Why" behind the course):**
- ### A. Experimental Approach (Benchmarking)
  *   **Method:** Implement the algorithm, run it with different inputs, measure time using `time.time()`.
  *   **The Problem (Why we don't rely on this):**
    1.  **Hardware Dependent:** A bad algorithm on a Supercomputer might beat a good algorithm on an old laptop.
    2.  **Implementation Dependent:** A sloppy Python implementation might be slower than a C++ implementation of the same algorithm.
    3.  **Limited Testing:** You cannot test for $N = \infty$. You might only test small inputs and miss that the algorithm crashes or slows down exponentially on massive datasets.
    4.  **Noise:** Background processes (OS updates, Spotify running) can skew results.
- ### B. Theoretical Approach (Asymptotic Analysis)
  *   **Method:** We count the **Fundamental Operations**.
  *   **What is a Fundamental Operation?** We don't count every variable assignment. We identify the operation that happens *the most*.
    *   In Sorting: We count **Comparisons** (is $A > B$?).
    *   In Searching: We count **Comparisons** (is $A == Target$?).
    *   In Matrix Math: We count **Multiplications**.
  *   **Goal:** To determine the "Order of Growth" (Big O). How does the runtime scale as $N$ explodes toward infinity?
  
  ---
- # Check for Understanding
  
  **Question:**
  Imagine you have two algorithms to sort a list of numbers:
  1.  **Algorithm A** takes $100N$ steps (Linear).
  2.  **Algorithm B** takes $2N^2$ steps (Quadratic).
  
  If $N$ (the list size) is very small (e.g., $N=5$), which algorithm is faster?
  If $N$ is very large (e.g., $N=1,000$), which algorithm is faster?