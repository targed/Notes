- Answers to the **Part 3 Practice Questions**:
  
  **A1: The Min-Heap Optimization**
  1. **Naive:** $n \times m = 1,000,000 \times 1,024 = \mathbf{\approx 1 \text{ Billion operations}}$.
  2. **Min-Heap:** $n \times \log_2 m = 1,000,000 \times 10 = \mathbf{\approx 10 \text{ Million operations}}$. *(The Heap makes the algorithm 100x faster!)*
  
  **A2: Tracing Graham's Algorithm**
  *   **Job 1 (2):** M1 is empty (0). Assign to M1. `M1 = 2`, `M2 = 0`.
  *   **Job 2 (3):** M2 is empty (0). Assign to M2. `M1 = 2`, `M2 = 3`.
  *   **Job 3 (2):** M1 has less work (2 < 3). Assign to M1. `M1 = 2+2=4`, `M2 = 3`.
  *   **Job 4 (2):** M2 has less work (3 < 4). Assign to M2. `M1 = 4`, `M2 = 3+2=5`.
  *   **Result:** Machine 1 load = 4. Machine 2 load = 5. **Makespan = 5**.
  
  **A3: Tracing LPT**
  1.  **Sorted:** `[3, 2, 2, 2]`.
  2.  **Trace:**
      *   Job 1 (3): Assign to M1. `M1 = 3`, `M2 = 0`.
      *   Job 2 (2): Assign to M2. `M1 = 3`, `M2 = 2`.
      *   Job 3 (2): Assign to M2 (2 < 3). `M1 = 3`, `M2 = 2+2=4`.
      *   Job 4 (2): Assign to M1 (3 < 4). `M1 = 3+2=5`, `M2 = 4`.
  3.  **Result:** Makespan is still **5**. (In this specific small example, the optimal answer actually *is* 5 because the sum is 9, and $9/2 = 4.5 \to 5$. So both algorithms found the optimal, but LPT guarantees a mathematically tighter safety net for massive datasets).
  
  **A4: The "Big Job" Bound**
  1.  **Average Load:** Total time = $109$. Machines = $10$. Average = $\mathbf{10.9 \text{ hours}}$.
  2.  **Big Job Bound:** The longest single job is $\mathbf{100 \text{ hours}}$.
  3.  **Optimal ($OPT$):** Since $OPT$ must be $\ge \max(10.9, 100)$, the absolute fastest this batch can finish is **100 hours**. 
  
  ***
- ### 1. PTAS vs. FPTAS (Slides 3 & 47)
  *   **PTAS (Polynomial-Time Approximation Scheme):** An algorithm where the user inputs a dial called $\epsilon$ (epsilon). The algorithm guarantees an answer within $(1 + \epsilon)$ of the optimal. The catch? The time is polynomial in the input size $n$, but it can be exponential relative to $1/\epsilon$ (e.g., $O(n^{1/\epsilon})$). If you demand high accuracy ($\epsilon = 0.01$), the runtime explodes to $n^{100}$.
  *   **FPTAS (Fully Polynomial...):** The holy grail. It is polynomial in **both** $n$ and $1/\epsilon$. If you demand high accuracy, the runtime only grows linearly or quadratically.
- ### 2. The Target: Subset Sum Problem (Slides 48–51)
  *   **The Problem:** You have a set of integers $S$ and a target capacity $t$. Find a subset of $S$ that sums up to be as large as possible, without exceeding $t$. *(Notice how this is basically the Knapsack problem, but Value and Weight are the exact same number).*
  *   **The "Exact" Algorithm (Exponential):**
    1. Start with a list $L_0 = [0]$.
    2. For each number $x_i$ in $S$:
       * Take the previous list $L_{i-1}$, and create a cloned list where $x_i$ is added to every number.
       * Merge the two lists.
       * Delete any numbers $> t$. 
  *   **The Flaw:** Because you clone and merge the list at every step, the list doubles in size. By step $n$, the list has $2^n$ numbers in it. The time complexity is **$O(2^n)$**.
- ### 3. The "Deep Dive" Fix: Trimming the List (Slides 56–58)
  If the list gets too big, why not just delete some numbers? 
  *   **The Trimming Rule:** If two numbers in the list are very close to each other, we don't need both of them. One can "represent" the other. 
  *   **The Math ($\delta$):** We pick a trimming parameter $\delta$ (delta). If we have two numbers $y$ and $z$ such that $\frac{y}{1+\delta} \le z \le y$, we simply **delete $y$**.
  *   **Example (Slide 57):** Let $\delta = 0.1$. 
    *   List = `[10, 11, 12, 15, 20, 21, 22, 23, 24, 29]`
    *   11 is within 10% of 10. Delete 11.
    *   12 is within 10% of 10? No. Keep 12.
    *   21 and 22 are within 10% of 20. Delete them.
    *   24 is within 10% of 23. Delete 24.
    *   **Trimmed List:** `[10, 12, 15, 20, 23, 29]`. We drastically shrank the list while keeping a "representative" for every number we deleted.
- ### 4. The FPTAS Algorithm (Slides 59–60)
  Now we build the final Approximation Scheme.
  1. The user passes in the set $S$, the target $t$, and their desired error rate **$\epsilon$**.
  2. **The "Professor Deep Dive" Trick:** We don't use $\epsilon$ directly to trim. We set our trimming parameter **$\delta = \frac{\epsilon}{2n}$**.
    *   *Why divide by $2n$?* Because we are going to trim the list $n$ times (once for every item in $S$). Every time we trim, the "representation" error compounds. By dividing our error dial by $2n$ up front, we mathematically guarantee that after $n$ rounds of compounding errors, the total final error will not exceed $\epsilon$.
  3. Run the exact algorithm, but call `TRIM(L, \epsilon / 2n)` at the end of every loop iteration.
  4. Return the largest number in the final list.
  
  **The Complexity Miracle (Slide 63):**
  Because we aggressively trim the list, the maximum possible length of the list $L_i$ is mathematically capped. It can never grow larger than:
  $$ \frac{3n \ln t}{\epsilon} + 2 $$
  Since the list length is bounded by a polynomial, the runtime to merge and trim the lists is strictly polynomial! We achieved **FPTAS**.
  
  ---
- ### Part 4 Practice Questions (FPTAS & Subset Sum)
  
  **Q1: Exact Subset Sum Trace**
  You are running the Exact Subset Sum algorithm.
  *   Target $t = 15$.
  *   Set $S = \{4, 6\}$
  *   $L_0 = [0]$
  1. What is $L_1$ after processing the number 4?
  2. What is $L_2$ after processing the number 6? (Remember to prune sums $> t$).
  
  **Q2: Trimming Trace**
  You are running the `TRIM(L, \delta)` function on the following sorted list: `L =[10, 12, 13, 15]`.
  Your trim parameter is $\delta = 0.2$ (meaning $1 + \delta = 1.2$).
  1. Can the number `12` be deleted and represented by `10`? *(Check: Is $12 \le 10 \times 1.2$?)*
  2. Can the number `13` be deleted and represented by `12`?
  3. What is the final trimmed list?
  
  **Q3: The Error Compounding Limit**
  In the `APPROX-SUBSET-SUM` algorithm, if the user asks for an error margin of $\epsilon = 0.10$ (10% error), and the input set $S$ has $n = 5$ items, what exact value is passed into the `TRIM` function as $\delta$?
  A) 0.10
  B) 0.05
  C) 0.01
  D) 0.02
  
  **Q4: Defining FPTAS**
  What is the defining difference between a PTAS and an FPTAS?
  A) FPTAS always finds the exact optimal solution.
  B) FPTAS guarantees a runtime that is polynomial in both the input size $n$ and $1/\epsilon$.
  C) FPTAS can only be used on graphs, while PTAS is for numbers.
  D) FPTAS uses Dynamic Programming.
  
  ---
  ---
- ### **Solutions to Part 4**
  
  **A1: Exact Trace**
  1.  **$L_1$:** Take $L_0 = [0]$. Clone and add 4 $\to [4]$. Merge $\to$ **`[0, 4]`**.
  2.  **$L_2$:** Take $L_1 = [0, 4]$. Clone and add 6 $\to [6, 10]$. Merge $\to$ **`[0, 4, 6, 10]`**.
  
  **A2: Trimming Trace**
  1.  Is $12 \le 10 \times 1.2$? ($12 \le 12$). **Yes.** Delete 12. `10` represents it.
  2.  Now we test `13` against `10` (since `12` is gone and `10` is our last "anchor"). Is $13 \le 10 \times 1.2$? ($13 \le 12$). **No.** Keep 13. It becomes the new anchor.
  3.  Next, test `15` against `13`. Is $15 \le 13 \times 1.2$? ($15 \le 15.6$). **Yes.** Delete 15.
  *   **Final Trimmed List:** **`[10, 13]`**.
  
  **A3: C) 0.01**
  *   **Math:** $\delta = \epsilon / (2n)$.
  *   $\delta = 0.10 / (2 \times 5) = 0.10 / 10 = \mathbf{0.01}$.
  *   *Why:* We must trim much stricter than 10% at each step so the compounded error at the very end doesn't exceed 10%.
  
  **A4: B) FPTAS guarantees a runtime that is polynomial in both the input size $n$ and $1/\epsilon$.**
  *   **Explanation:** As shown on Slide 3 and Slide 47, a standard PTAS could have a runtime like $O(n^{2/\epsilon})$. If $\epsilon = 0.5$, runtime is $O(n^4)$. If $\epsilon = 0.1$, runtime explodes to $O(n^{20})$, which is practically intractable. FPTAS formulas keep $1/\epsilon$ out of the exponent (e.g., $O(n^3 / \epsilon)$), meaning you can demand ultra-high accuracy without crashing your computer!
  
  ---