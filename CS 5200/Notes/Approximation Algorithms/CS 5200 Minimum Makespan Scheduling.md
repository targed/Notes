- Answers to the **Part 2 Practice Questions**:
  
  **A1: Vertex Cover to Set Cover Translation**
  1.  **The Universe $X$**: The set of all edges: $\{e1, e2, e3\}$.
  2.  **Subset for Vertex A**: The subset contains the specific edges connected to vertex A. (For example, if A connects to B via $e1$ and C via $e2$, then $S_A = \{e1, e2\}$).
  
  **A2: The Greedy Choice**
  1.  **First pick:** **$S_1$**. It covers 4 new elements $\{1, 2, 3, 4\}$. (No other set covers more than 3).
  2.  **Second pick:** **$S_4$**. The remaining uncovered elements are $\{5, 6\}$. $S_4$ covers both of them. (Note: $S_2$ and $S_3$ would only cover 1 new element each).
  3.  **Did it find the optimal?** **Yes.** It covered the whole universe using 2 sets ($S_1$ and $S_4$). No single set covers all 6 elements, so 2 is the absolute mathematical minimum. 
  
  **A3: B) 1/3, 1/3, 1/3**
  *   *Explanation:* The cost ($c_x$) assigned to an element is exactly $1$ divided by the number of *newly covered* elements in that step. Since the set covered 3 new elements, the cost of \$1 is split 3 ways. 
  
  ***
- ### 1. The Core Problem (Slide 12)
  Imagine you are managing a cluster of servers (AWS/Google Cloud). 
  *   You have **$m$ identical machines**.
  *   You have **$n$ jobs** to run, each taking a specific amount of time (processing length $p_i$).
  *   **The Goal:** Distribute the jobs among the machines so that the entire batch finishes as fast as possible. 
  *   **The Makespan:** The time when the *very last* machine finishes its work. We want to **minimize the makespan**.
  
  Because finding the absolute perfect balance is an NP-Hard problem (it's related to the Subset Sum/Partition problem), we need an approximation.
- ### 2. Graham's Algorithm: The Factor-2 Greedy Approach (Slides 14–15)
  This is one of the oldest approximation algorithms in computer science (invented in 1966).
  
  **The Logic:**
  1.  Take the jobs in any arbitrary order.
  2.  Look at your $m$ machines.
  3.  Assign the next job to the machine that currently has the **least amount of work** assigned to it.
  
  **Time Complexity Optimization (Slides 16–17):**
  *   *Naive approach:* To find the least-loaded machine, you scan all $m$ machines for every single job $n$. Time = **$O(nm)$**.
  *   *The "Deep Dive" Fix:* How do we find the minimum item dynamically without a linear scan? **Use a Min-Heap (Priority Queue)!** 
    *   Keep the $m$ machines in a Min-Heap based on their current load. Extracting the min takes $O(\log m)$. Updating it and putting it back takes $O(\log m)$.
    *   New Time Complexity: **$O(n \log m)$**.
- ### 3. The "Deep Dive" Proof: Why is it a 2-Approximation? (Slides 21–22)
  We need to mathematically prove that Graham's Algorithm will *never* be worse than twice the optimal makespan ($2 \times OPT$).
  
  **Step A: The Two Lower Bounds on $OPT$**
  Before we analyze our algorithm, we must state two absolute laws of physics regarding the perfect optimal answer ($OPT$):
  1.  **The "Average" Law:** The optimal makespan can never be faster than the average workload. If you have 100 hours of total work and 10 machines, $OPT \ge 10$ hours. 
    *   Math: **$OPT \ge \frac{1}{m} \sum p_i$**
  2.  **The "Big Job" Law:** The optimal makespan can never be faster than the single longest job. If one job takes 50 hours, it doesn't matter if you have a million machines, the batch will take at least 50 hours.
    *   Math: **$OPT \ge \max(p_i)$**
  
  **Step B: The Proof (Slide 22)**
  Let's look at the machine that finishes **last** in our Greedy algorithm. Let's call its total time our algorithm's makespan ($M$). 
  Let the very last job assigned to this machine be **Job $j$** (with length $p_j$).
  *   When Job $j$ was assigned to this machine, it started at time $start_j$.
  *   Therefore, our makespan is: **$M = start_j + p_j$**.
  *   *The Genius Insight:* Because of our greedy rule, when Job $j$ was assigned, this machine had the *least* amount of work. This means *every other machine* was busy up until at least $start_j$. 
  *   Therefore, $start_j$ is less than or equal to the average workload of the system! 
    *   $start_j \le \text{Average Load} \le \mathbf{OPT}$ (From Law 1).
  *   We also know the length of Job $j$ cannot exceed the optimal makespan.
    *   $p_j \le \mathbf{OPT}$ (From Law 2).
  *   **The Conclusion:** Substitute these back into our makespan equation:
    *   $M = start_j + p_j$
    *   $M \le OPT + OPT$
    *   **$M \le 2 \times OPT$**. We have proven it is a 2-Approximation!
- ### 4. The Heuristic Upgrade: Longest Processing Time First (LPT) (Slides 23–27)
  Graham's algorithm is great, but we can easily make it better. 
  
  **The Flaw:** If you assign all the tiny jobs first, and then suddenly a massive 100-hour job appears at the very end of your list, it will ruin your perfectly balanced schedule.
  **The Fix (LPT):** **Sort the jobs from longest to shortest first**, then run Graham's algorithm!
  *   By placing the "Big Rocks" into the machines first, you use the tiny jobs at the end like "sand" to perfectly smooth out the imbalances.
  
  **The Math Guarantee (Slide 27):**
  Sorting the jobs improves our mathematical guarantee from a Factor 2 approximation down to a **Factor $4/3$ (or 1.5)** approximation.
  *   *Exact Formula:* LPT is bounded by **$\frac{3}{2} - \frac{1}{2m}$**.
  
  ---
- ### Part 3 Practice Questions (Makespan Scheduling)
  
  **Q1: The Min-Heap Optimization**
  If you have 1,000,000 jobs ($n$) and 1,024 machines ($m$).
  1. Roughly how many operations would the Naive Graham's algorithm take?
  2. Roughly how many operations would the Min-Heap optimized Graham's algorithm take? *(Hint: $\log_2 1024 = 10$)*.
  
  **Q2: Tracing Graham's Algorithm**
  You have **2 machines**. You process the following jobs in this exact order: `[2, 3, 2, 2]`. 
  *   Trace Graham's Algorithm. What is the load on Machine 1, the load on Machine 2, and the final makespan?
  
  **Q3: Tracing LPT (Longest Processing Time First)**
  Take the exact same jobs from Q2: `[2, 3, 2, 2]`. 
  1. Sort them from largest to smallest.
  2. Trace the LPT algorithm on **2 machines**.
  3. What is the final makespan? Did sorting them improve the answer compared to Q2?
  
  **Q4: The "Big Job" Bound**
  You have 10 machines. You have 9 jobs that take 1 hour each, and 1 job that takes 100 hours.
  If you plug this into the two lower bounds equations:
  1. What is the Average Load bound?
  2. What is the Big Job bound?
  3. What is the absolute lowest possible Optimal Makespan ($OPT$)?
  
  ---