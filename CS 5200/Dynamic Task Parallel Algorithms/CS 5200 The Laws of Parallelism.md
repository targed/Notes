### 1. The Two Fundamental Laws (Slides 22–25)
If we run an algorithm on $P$ processors, the execution time is denoted as **$T_p$**. Two physical limits govern this time:
- #### A. The Work Law: $T_p \ge \frac{T_1}{P}$
  *   **Intuition:** If you have 100 hours of work ($T_1$) and 4 workers ($P$), the fastest they can possibly finish is 25 hours. 
  *   **Deep Dive:** You cannot finish faster than the total work divided by the number of workers. This assumes "Perfect Load Balancing" (no worker is ever idle).
- #### B. The Span Law: $T_p \ge T_\infty$
  *   **Intuition:** No matter how many workers you hire, you cannot finish faster than the time it takes to complete the longest sequence of dependent tasks (the critical path). 
  *   **Deep Dive:** Adding 1,000 processors to a 10-node sequential chain doesn't help. The time stays at 10.
  
  ---
- ### 2. Speedup and Parallelism (Slides 26–30)
  We use the laws above to measure how "worthwhile" it is to parallelize an algorithm.
  
  *   **Speedup:** The ratio $\frac{T_1}{T_p}$. 
    *   If Speedup $= P$, we call it **Linear Speedup**. (e.g., 4 processors make it 4x faster).
    *   *The Limit:* Speedup can **never** be greater than $P$ (Work Law).
  *   **Parallelism:** The ratio $\frac{T_1}{T_\infty}$.
    *   This is the **Theoretical Maximum Speedup**. 
    *   It tells you the maximum number of processors the algorithm can effectively use. 
  
  **Professor Deep Dive (Slide 30):**
  If Fibonacci(4) has $T_1 = 17$ and $T_\infty = 8$:
  *   $\text{Parallelism} = 2.12$. 
  *   This means if you use **2** processors, you might get close to a 2x speedup. 
  *   If you use **100** processors, you still only get a 2.12x speedup. **98 processors are wasted.**
  
  ---
- ### 3. Brent’s Theorem (The Greedy Scheduler) (Slides 37–44)
  In the real world, tasks aren't perfectly balanced. We use a **Greedy Scheduler**, which follows one rule: **"Never leave a processor idle if there is a task ready to run."**
  
  **Brent’s Law Formula:**
  $$T_p \le \frac{T_1}{P} + T_\infty$$
  
  **Why this is a "Deep Dive" point:**
  This formula provides an **Upper Bound** (Worst Case) for how long a greedy scheduler will take. 
  *   It shows that as $P$ gets very large, the $T_1/P$ term goes to zero, and the time approaches the Span ($T_\infty$).
  *   **The Corollary (Slide 47):** A Greedy Scheduler is always **within a factor of 2 of Optimal**. Even if you had a "Perfect/God-mode" scheduler, it would at most be twice as fast as the simple Greedy one.
  
  ---
- ### 4. Scheduling Quiz: Static vs. Dynamic (Slides 31–33)
  Your slides present 7 tasks ($t_1 \dots t_7$) with times: $1, 2, 3, 4, 5, 5, 10$.
  **Goal:** Run these on 2 processors to get the "Greatest Speedup" (Minimum $T_2$).
  
  *   Total Work ($T_1$): $1+2+3+4+5+5+10 = \mathbf{30 \text{ seconds}}$.
  *   Ideal Time ($T_1/P$): $30 / 2 = \mathbf{15 \text{ seconds}}$.
  *   The Strategy: To hit 15 seconds, we must balance the load so both CPUs finish at the same time.
    *   **CPU 1:** $t_7 (10) + t_6 (5) = 15$
    *   **CPU 2:** $t_5 (5) + t_4 (4) + t_3 (3) + t_2 (2) + t_1 (1) = 15$
  *   **Result:** $T_2 = 15$. Speedup $= 30 / 15 = \mathbf{2}$. (Perfect Linear Speedup).
  
  ---
- ### 5. Application: The MIT Chess Example (Slides 40–43)
  This is a common exam word problem.
  *   **Scenario:** You designed a program for 512 processors, but you can only test it on 32. 
  *   **Data:** $T_1 = 2048s$, $T_\infty = 1s$.
  *   **Question:** What is the predicted time on 32 processors ($T_{32}$)?
  *   **Calculation:**
    *   $T_{32} \le \frac{2048}{32} + 1$
    *   $T_{32} \le 64 + 1 = \mathbf{65s}$.
  
  ---
- ### Practice Questions
  
  **Q1: The Span Limit**
  An algorithm has $T_1 = 100$ and $T_\infty = 20$.
  1. What is the Parallelism?
  2. If you have 10 processors, what is the fastest possible time ($T_{10}$) according to the Work Law?
  3. If you have 100 processors, what is the fastest possible time ($T_{100}$) according to the Span Law?
  
  **Q2: Speedup Calculation**
  A program takes 60 minutes to run on 1 CPU. When run on 4 CPUs, it takes 20 minutes.
  1. What is the Speedup?
  2. Is this "Linear Speedup"? 
  3. Based on Brent’s Law ($T_p \le T_1/P + T_\infty$), what is the estimated Span ($T_\infty$) of this program?
  
  **Q3: Fibonacci Formula (Slide 48)**
  Your professor notes that for Fibonacci, $\text{Parallelism} = O(2^n / n)$. 
  *   As $n$ (the input) gets larger, does the parallelism increase or decrease?
  *   *Deep Dive:* Why does a larger $n$ make the algorithm "more parallelizable"?
  
  ---