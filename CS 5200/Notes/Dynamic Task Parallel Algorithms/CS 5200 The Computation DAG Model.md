### 1. What is a Computation DAG? (Slide 19)
A parallel program can be represented as a **Directed Acyclic Graph (DAG)**, denoted as $G(V, E)$. 
*   Vertex ($V$): Represents an instruction or a group of instructions (called a **Strand**).
*   Edge ($E$): Represents a **Dependency**. If there is an edge $(u, v)$, it means instruction $u$ must finish before instruction $v$ can start.
*   **Acyclic:** It must be acyclic (no loops) because an instruction cannot depend on its own future result.
- ### 2. The "Strand" (Slide 20)
  A **Strand** is a sequence of instructions that contains no parallel control (no `spawn`, `sync`, `call`, or `return`). 
  *   Essentially, a strand is a "block of sequential code." 
  *   In the graph, we treat each strand as a single node. 
  *   **Visualizing the Fibonacci DAG:** Look at the colored nodes in Slide 20.
    *   **Blue circles:** Code leading up to a `spawn`.
    *   **Orange circles:** Code that executes in parallel with the spawned child.
    *   **White circles:** Code that executes after a `sync`.
  
  ---
- ### 3. Key Metrics: Work ($T_1$) and Span ($T_\infty$)
  This is the most important concept in parallel complexity. You must be able to calculate these two numbers by looking at a graph on a quiz.
- #### A. Work ($T_1$)
  *   **Definition:** The total time to execute the algorithm on a **single processor**.
  *   **In the DAG:** It is the **total number of nodes** (assuming each node takes 1 unit of time).
  *   **Example (Slide 21):** In the Fibonacci(4) trace, there are **17 strands**. Therefore, **Work = 17**.
- #### B. Span ($T_\infty$)
  *   **Definition:** The minimum time to execute the algorithm on a machine with **infinite processors**.
  *   **In the DAG:** It is the **Longest Path** from the start node to the end node. This is also called the **Critical Path**.
  *   **Example (Slide 21):** In the Fibonacci(4) trace, the longest sequence of dependencies is **8 strands** long. Therefore, **Span = 8**.
  
  ---
- ### 4. The Professor Deep Dive: Why Span Matters
  Think of a construction project (building a house):
  *   **Work:** The total "man-hours" required (e.g., 1000 hours).
  *   **Span:** The things that *must* happen in order (e.g., you cannot paint the walls until the frame is built). Even if you hire 1 million workers, the house still takes a minimum amount of time to dry the concrete and build the frame.
  *   **The Lesson:** The Span ($T_\infty$) is the absolute theoretical limit of how fast your algorithm can ever be. No amount of hardware can beat the Span.
  
  ---
- ### 5. Calculating Parallelism (Slide 30 Preview)
  Once you have Work ($T_1$) and Span ($T_\infty$), you can calculate the **Parallelism** of the algorithm:
  $$\text{Parallelism} = \frac{T_1}{T_\infty}$$
  
  **Fibonacci(4) Example:**
  *   $T_1 = 17$
  *   $T_\infty = 8$
  *   $\text{Parallelism} = 17/8 = \mathbf{2.12}$
  *   **What this means:** This specific algorithm can only effectively use about **2 processors**. If you try to run P-Fib(4) on a 64-core supercomputer, 62 cores will be sitting idle because there isn't enough independent work to do.
  
  ---
- ### Practice Questions
  
  **Q1: The Linear DAG**
  Suppose you have a task with 10 strands, but every strand depends on the one before it (a straight line graph).
  1. What is the Work ($T_1$)?
  2. What is the Span ($T_\infty$)?
  3. What is the Parallelism?
  
  **Q2: The Perfectly Parallel DAG**
  You have 10 strands. A "Master" node spawns all 10 at once, they run independently, and then a "Sync" node joins them.
  1. What is the Work? (Count the Master, the 10 tasks, and the Sync).
  2. What is the Span? (How many nodes are on the longest path from Start to End?)
  3. What is the Parallelism?
  
  **Q3: Recursive Tree Height**
  In a Divide and Conquer algorithm like Parallel Merge Sort, the **Span** is usually equal to the **Height** of the recursion tree multiplied by the work done in the `merge` step. 
  *   If the tree is $\log N$ high, and merging is $O(N)$, why does the Span of a parallel merge sort look like $O(N)$ and not $O(\log N)$? (Think about dependencies).