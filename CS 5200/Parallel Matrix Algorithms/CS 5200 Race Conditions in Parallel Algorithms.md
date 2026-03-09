### 1. The Definitions (Slide 7)
Before diagnosing a bug, we must define the expected behavior of a program:
*   **Deterministic:** The algorithm always produces the same output for the same input, every single time it is run.
*   **Non-deterministic:** The output can vary between runs even if the input is identical. This is usually a sign of a bug in parallel computing.

**The Determinacy Race:**
A race occurs when two logically parallel tasks (tasks that *could* run at the same time) access the same memory location, and **at least one of those tasks performs a "Write" (modification).**

---
- ### 2. Deep Dive: The "Lost Update" Problem (Slides 9–10)
  Why is `x = x + 1` dangerous in parallel? Even though it looks like one line of code, the CPU sees **three distinct steps**:
  1.  **LOAD:** Read the current value of $x$ from RAM into a processor register ($r_1$).
  2.  **INCREMENT:** Add 1 to the value inside the register ($r_1 = r_1 + 1$).
  3.  **STORE:** Write the new value from the register back into RAM.
  
  **The Race Scenario (Slide 10):**
  Imagine $x = 0$ and we run two parallel tasks to increment it. We expect $x = 2$.
  *   **Thread 1** LOADS $x$ (gets 0).
  *   **Thread 2** LOADS $x$ (also gets 0).
  *   **Thread 1** increments its register (gets 1).
  *   **Thread 1** STORES 1 back to $x$.
  *   **Thread 2** increments its register (gets 1).
  *   **Thread 2** STORES 1 back to $x$.
  *   **Final Result:** $x = 1$. One update was **completely lost**.
  
  ---
- ### 3. The "Wrong" Matrix-Vector Multiplication (Slide 11)
  Your professor presents `P-MAT-VEC-WRONG`. Let's look at why it is broken:
  
  ```c
  P-MAT-VEC-WRONG(A, x, y, n)
  1 parallel for i = 1 to n
  2     parallel for j = 1 to n    // ERROR: Parallelizing the inner loop
  3         y[i] = y[i] + A[i][j] * x[j] 
  ```
  
  **The Deep Dive Diagnosis:**
  *   In Line 1, we spawn different rows. This is safe because each row $i$ writes to a **unique** result index $y[i]$.
  *   In Line 2, we spawn different $j$ iterations for the **same row**.
  *   All these parallel tasks are now trying to update **the exact same variable** $y[i]$ at the same time.
  *   **The Result:** Because of the "Lost Update" problem explained above, the final sum in $y[i]$ will be mathematically incorrect.
  
  ---
- ### 4. Why Race Bugs are "Pernicious" (Slide 8)
  Your professor highlights two famous examples (Therac-25 and the 2003 Blackout) to emphasize that these aren't just "math errors"—they are life-threatening.
  *   **Heisenbugs:** Race conditions are notoriously hard to debug. You can test your code 1,000 times on your laptop and it might work (because the timing was lucky). As soon as you deploy it to a faster server, the timing shifts, the race triggers, and the system crashes.
  *   **The Takeaway:** You cannot "test" your way out of a race condition; you must **design** your way out of it.
  
  ---
- ### 5. Professor Deep Dive: Avoiding the Race
  To fix `P-MAT-VEC-WRONG`, you have two choices:
  1.  **Mutual Exclusion (Locks):** Force threads to take turns writing to $y[i]$. (Problem: This makes the code slow and sequential again).
  2.  **Parallel Reduction:** Use a Divide and Conquer tree to sum the numbers. Instead of everyone writing to one variable, pairs of numbers are added together in a tree structure. This is the **correct** way to achieve a $O(\lg n)$ span. (This is the topic of the next section).
  
  ---
- ### Part 2 Practice Questions
  
  **Q1: Identifying the Race**
  Suppose you have an array `A` and you want to count how many even numbers are in it using a parallel loop.
  ```c
  count = 0
  parallel for x in A:
    if x % 2 == 0:
        count = count + 1
  ```
  1. Is there a race condition?
  2. Which memory location is the "target" of the race?
  3. What are the potential final values of `count` if there are 10 even numbers in the array?
  
  **Q2: The "Read-Only" Exception**
  If two parallel tasks both read from the same memory location but **neither** of them modifies it, is it a race condition? Why or why not?
  
  **Q3: Interleaving**
  Look at Slide 10. If Step 4 (Thread 2 Load) happened **after** Step 7 (Thread 1 Store), would the answer be correct? 
  *   *Deep Dive:* What does this tell you about the relationship between "Timing" and "Correctness" in parallel programs?