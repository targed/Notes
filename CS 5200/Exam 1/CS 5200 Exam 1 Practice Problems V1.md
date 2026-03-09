### **Topic: Parallel Laws & Brent's Theorem**

**Q1. The Basic Definitions**
You analyze a parallel computation DAG and find that it has **100 total nodes** and the longest path from start to finish has **20 nodes**.
1.  What is the Work ($T_1$)?
2.  What is the Span ($T_\infty$)?
3.  What is the Parallelism?

**Q2. The Speed Limit (Span Law)**
An algorithm has a Span ($T_\infty$) of **50 seconds**. Even if you buy a supercomputer with **1,000,000 processors**, what is the absolute fastest this algorithm can ever run?
a) 0.00005 seconds
b) 50 seconds
c) 1 second
d) It depends on the Work.

**Q3. The Efficiency Limit (Work Law)**
An algorithm performs **500 units of Work** ($T_1 = 500$). You have **10 processors**. According to the Work Law, what is the lower bound on the running time ($T_{10}$)?
a) 50 seconds
b) 500 seconds
c) 10 seconds
d) 5000 seconds

**Q4. Calculating Parallelism**
Algorithm A has $T_1 = n^3$ and $T_\infty = n$.
Algorithm B has $T_1 = n^3$ and $T_\infty = \log^2 n$.
Which algorithm has higher parallelism, and what is its parallelism in terms of $n$?

**Q5. Brent's Theorem Calculation**
Given a computation with:
*   Work ($T_1$) = 1000
*   Span ($T_\infty$) = 100
*   Processors ($P$) = 10

Using Brent's Theorem ($T_P \le \frac{T_1 - T_\infty}{P} + T_\infty$), what is the upper bound on the execution time $T_{10}$?

**Q6. Refuting a Claim (Like the Homework)**
A startup claims their new parallel video encoder has the following performance stats:
*   Running on **1 processor** takes **100 seconds**.
*   Running on **infinite processors** takes **20 seconds**.
*   They claim running on **4 processors** takes **20 seconds**.

Is this claim mathematically possible? Use the Work Law to prove your answer.

**Q7. Linear Speedup**
If an algorithm has perfect **Linear Speedup**, which of the following is true?
a) $T_P = T_1$
b) $T_P = T_\infty$
c) $T_P = T_1 / P$
d) $T_P = T_1 / T_\infty$

**Q8. Amdahl's Logic (Dependency Chains)**
You have a program that is 90% parallelizable, but 10% of the work **must** be done sequentially (a long chain of dependencies). As you add more and more processors ($P \to \infty$), what value does the total runtime approach?
a) 0
b) The time taken by the 10% sequential part.
c) The time taken by the 90% parallel part.
d) Infinity.

**Q9. Greedy Scheduler Bound**
Your professor mentioned a Corollary to Brent's Theorem: A greedy scheduler (one that never leaves a processor idle if work is available) is always within a factor of **What** of the optimal scheduler?
a) 1.5
b) 2
c) 4
d) $\log P$

**Q10. The "Slackness" Concept**
If your Parallelism ($T_1 / T_\infty$) is **10,000**, and you are running on a machine with only **4 processors**, will you likely achieve good speedup (close to 4x)? Why or why not?

---
- ### **Solutions & Explanations**
  
  **Q1:**
  1.  **Work ($T_1$) = 100** (Total nodes).
  2.  **Span ($T_\infty$) = 20** (Longest path).
  3.  **Parallelism = 5** ($100 / 20$).
  
  **Q2: b) 50 seconds**
  *   **Why:** The Span Law states $T_p \ge T_\infty$. You can never go faster than the critical path, no matter how much hardware you throw at it.
  
  **Q3: a) 50 seconds**
  *   **Why:** The Work Law states $T_p \ge T_1 / P$. $500 / 10 = 50$.
  
  **Q4: Algorithm B**
  *   **Parallelism A:** $n^3 / n = n^2$.
  *   **Parallelism B:** $n^3 / \log^2 n$.
  *   Since $\log^2 n$ is much smaller than $n$, the ratio for B is much larger. Algorithm B scales better.
  
  **Q5: 190 seconds**
  *   $T_{10} \le \frac{1000 - 100}{10} + 100$
  *   $T_{10} \le \frac{900}{10} + 100$
  *   $T_{10} \le 90 + 100 = 190$.
  
  **Q6: No, it is impossible.**
  *   **Check Work Law:** $T_4 \ge T_1 / 4$.
  *   $20 \ge 100 / 4$.
  *   $20 \ge 25$. **FALSE.**
  *   You cannot finish 100 seconds of work in 20 seconds using only 4 processors (even with 0 overhead, it would take 25s).
  
  **Q7: c) $T_P = T_1 / P$**
  *   **Why:** Linear speedup means if you double the processors, you halve the time. The speedup factor is exactly $P$.
  
  **Q8: b) The time taken by the 10% sequential part.**
  *   **Why:** The Span ($T_\infty$) includes that sequential chain. As $P \to \infty$, the parallel part shrinks to near-zero, but the sequential part remains fixed. (This is Amdahl's Law in action).
  
  **Q9: b) 2**
  *   **Why:** Theorem 26.1 (and Corollary 26.2 in your slides) states that a greedy scheduler executes a computation in time $T_P \le T_1/P + T_\infty$. The optimal time $T^*_P$ is at least $\max(T_1/P, T_\infty)$. Therefore, the greedy time is at most twice the optimal time.
  
  **Q10: Yes.**
  *   **Why:** You have massive **Slackness** (Parallelism $\gg$ Processors). Since there is so much independent work available (10,000 units per step on average), it is very easy to keep 4 processors busy 100% of the time, achieving near-perfect linear speedup.
-
-
-
-
- ### **Topic: Writing Parallel Algorithms**
  
  **Q1. Parallel Array Sum (Divide & Conquer)**
  Write a recursive parallel algorithm `P-SUM(A, low, high)` that computes the sum of all elements in array $A$ between indices `low` and `high`.
  *   *Hint:* Use the Divide and Conquer strategy. Base case is a single element.
  
  **Q2. Vector Addition (Data Parallelism)**
  Write a parallel algorithm `P-VECTOR-ADD(A, B, C, n)` that adds two arrays $A$ and $B$ element-wise and stores the result in $C$.
  *   $C[i] = A[i] + B[i]$.
  *   *Hint:* This doesn't need recursion; a parallel loop is sufficient.
  
  **Q3. Identifying the Bug (Sync Placement)**
  Analyze the following code for parallel Fibonacci. What is the performance problem?
  ```text
  P-FIB(n)
   if n <= 1 return n
   x = spawn P-FIB(n-1)
   sync  // <--- Sync is here
   y = P-FIB(n-2)
   return x + y
  ```
  
  **Q4. Fixing a Race Condition (Dot Product)**
  The following code computes the Dot Product of two vectors but has a **determinacy race**.
  ```text
  P-DOT-BAD(A, B, n)
   global sum = 0
   parallel for i = 1 to n
       sum = sum + (A[i] * B[i])  // RACE!
   return sum
  ```
  Rewrite this using a **Recursive Divide & Conquer** approach (`P-DOT-RECURSIVE`) to calculate the sum without a race condition.
  
  **Q5. Parallel Find Max**
  Write a recursive parallel algorithm `P-MAX(A, low, high)` that finds the maximum element in an array.
  *   *Hint:* Similar to P-SUM, but use `max()` instead of `+`.
  
  **Q6. Parallel Matrix Multiplication (Simple)**
  Write the pseudocode for **Simple Parallel Matrix Multiplication** ($C = A \times B$) using `parallel for` loops.
  *   Recall: You can parallelize the outer loops ($i$ and $j$), but keep the inner loop ($k$) sequential to avoid races on $C[i,j]$.
  
  **Q7. Parallel Merge Sort (Top Level)**
  Write the `P-MERGE-SORT(A, p, r)` function.
  *   It should sort the left half and right half in parallel.
  *   You can assume a sequential function `MERGE(A, p, q, r)` already exists.
  
  **Q8. The Hybrid Logic (Grain Size)**
  Modify your `P-SUM` algorithm from Q1 to use a **Coarsening / Grain Size** optimization.
  *   If the range `(high - low)` is less than a constant `GRAIN_SIZE`, run a sequential loop instead of spawning.
  
  **Q9. Parallel Prefix (Scan) Logic**
  Why can’t we simply use a `parallel for` loop to calculate the prefix sum array `Y` where `Y[i] = Y[i-1] + X[i]`?
  *   (Short answer question regarding dependencies).
  
  **Q10. Matrix Transpose**
  Write a parallel algorithm to transpose a matrix ($A[j][i] = A[i][j]$).
  *   Use `parallel for` loops. Be careful with the loop bounds to ensure you don't swap elements twice!
  
  ---
- ### **Solutions & Explanations**
  
  **A1. Parallel Array Sum**
  ```text
  P-SUM(A, low, high)
    if low == high
        return A[low]
    mid = (low + high) / 2
    left_sum = spawn P-SUM(A, low, mid)
    right_sum = P-SUM(A, mid + 1, high)
    sync
    return left_sum + right_sum
  ```
  
  **A2. Vector Addition**
  ```text
  P-VECTOR-ADD(A, B, C, n)
    parallel for i = 0 to n-1
        C[i] = A[i] + B[i]
  ```
  *(Since every iteration `i` writes to a unique location `C[i]`, there are no race conditions).*
  
  **A3. The Bug: Sync too early**
  By placing `sync` immediately after the `spawn`, the parent waits for the child to finish **before** doing any of its own work (`P-FIB(n-2)`).
  *   **Result:** The algorithm executes sequentially. Parallelism = 1. The `spawn` overhead makes it slower than a standard loop.
  
  **A4. Recursive Dot Product (Reduction)**
  ```text
  P-DOT-RECURSIVE(A, B, low, high)
    if low == high
        return A[low] * B[low]
    mid = (low + high) / 2
    left_val = spawn P-DOT-RECURSIVE(A, B, low, mid)
    right_val = P-DOT-RECURSIVE(A, B, mid + 1, high)
    sync
    return left_val + right_val
  ```
  *(This creates a tree of additions, avoiding the race on a global variable).*
  
  **A5. Parallel Find Max**
  ```text
  P-MAX(A, low, high)
    if low == high
        return A[low]
    mid = (low + high) / 2
    left_max = spawn P-MAX(A, low, mid)
    right_max = P-MAX(A, mid + 1, high)
    sync
    return max(left_max, right_max)
  ```
  
  **A6. Parallel Matrix Multiplication (Simple)**
  ```text
  P-MAT-MUL(A, B, C, n)
    parallel for i = 0 to n-1
        parallel for j = 0 to n-1
            // Inner loop must be serial to update C[i,j] safely
            for k = 0 to n-1
                C[i,j] = C[i,j] + A[i,k] * B[k,j]
  ```
  
  **A7. Parallel Merge Sort**
  ```text
  P-MERGE-SORT(A, p, r)
    if p >= r return
    q = (p + r) / 2
    spawn P-MERGE-SORT(A, p, q)
    P-MERGE-SORT(A, q + 1, r)  // Parent does right half
    sync
    MERGE(A, p, q, r)
  ```
  
  **A8. Grain Size Optimization**
  ```text
  P-SUM-OPTIMIZED(A, low, high)
    if (high - low) < 1000  // Threshold
        seq_sum = 0
        for i = low to high
            seq_sum += A[i]
        return seq_sum
        
    mid = (low + high) / 2
    left_sum = spawn P-SUM-OPTIMIZED(A, low, mid)
    right_sum = P-SUM-OPTIMIZED(A, mid + 1, high)
    sync
    return left_sum + right_sum
  ```
  
  **A9. Dependency Chain**
  In a prefix sum, `Y[i]` depends on the result of `Y[i-1]`.
  *   You cannot calculate iteration `i` until iteration `i-1` is totally finished.
  *   This is a **Loop-Carried Dependency**, which prevents simple `parallel for` execution. (It requires a specialized "Parallel Scan" algorithm involving two passes).
  
  **A10. Matrix Transpose**
  ```text
  P-TRANSPOSE(A, n)
    parallel for i = 0 to n-1
        // Start j at i+1 to only process upper triangle
        // preventing double-swapping
        parallel for j = i + 1 to n-1
            swap(A[i,j], A[j,i])
  ```
-
-
-
-
- ### **Topic: Hashing & Collision Resolution**
  
  **Q1. The Basic Hash Index**
  You have a hash table of size $m = 11$. The hash function is $h(k) = 3k + 5$.
  Where does the key $k = 15$ get stored?
  a) Index 4
  b) Index 6
  c) Index 0
  d) Index 5
- **Q2. Linear Probing Trace**
  You have an empty hash table of size 10 (Indices 0–9).
  Hash function: $h(x) = x \pmod{10}$.
  Collision Resolution: **Linear Probing**.
  Insert the keys: `12, 22, 32, 42`.
  At which index is the key **42** stored?
  a) 2
  b) 3
  c) 4
  d) 5
- **Q3. Primary Clustering**
  Which of the following statements best describes **Primary Clustering** in Linear Probing?
  a) Keys map to random locations, keeping the table balanced.
  b) Long runs of occupied slots build up, increasing the average search time for subsequent insertions.
  c) Two keys hash to the same primary index, but different secondary indices.
  d) It occurs when the load factor exceeds 1.0.
- **Q4. Double Hashing Logic**
  In Double Hashing, we use the formula $idx = (h_1(k) + i \cdot h_2(k)) \pmod m$.
  Why is it critical that $h_2(k)$ **never evaluates to 0**?
  a) It would cause a divide-by-zero error.
  b) It would make the step size 0, causing an infinite loop probing the exact same index.
  c) It would effectively turn Double Hashing into Linear Probing.
  d) It would violate the load factor limit.
- **Q5. Double Hashing Trace**
  Table size $m = 13$.
  $h_1(k) = k \pmod{13}$.
  $h_2(k) = 1 + (k \pmod{11})$.
  Insert key $k = 27$.
  Assume index 1 (where $27$ wants to go first) is **occupied**.
  What is the **next** index probed?
  a) 2
  b) 6
  c) 7
  d) 8
- **Q6. Cuckoo Hashing Guarantee**
  What is the primary advantage of Cuckoo Hashing over Linear Probing and Double Hashing?
  a) It supports a Load Factor of 100%.
  b) It guarantees O(1) Worst-Case Lookup time.
  c) It requires only one hash function.
  d) It never requires resizing (rehashing) the table.
- **Q7. Cuckoo Hashing Eviction**
  You have two tables ($T_1, T_2$) using $h_1(x) = x \pmod 3$ and $h_2(x) = (x/3) \pmod 3$.
  *   $T_1$ currently holds `{6}` at index 0. ($6 \pmod 3 = 0$).
  *   $T_2$ is empty.
  You insert **$9$**:
  1.  $h_1(9) = 0$. $T_1[0]$ is occupied by 6. Evict 6! Put 9 there.
  2.  Where does the evicted key **6** go?
  a) $T_2[0]$
  b) $T_2[1]$
  c) $T_2[2]$
  d) It is discarded.
- **Q8. Load Factor Limit**
  Open Addressing strategies (Linear Probing, Double Hashing) degrade rapidly as the table fills up. Cuckoo Hashing is even more sensitive. What is the generally accepted maximum **Load Factor ($\alpha$)** for a Cuckoo Hash table before you *must* resize?
  a) 25%
  b) 50%
  c) 75%
  d) 99%
- **Q9. Worst-Case Lookup**
  In a standard Hash Table using **Linear Probing**, what is the **Worst-Case** time complexity for a `contains(k)` operation (assuming the table is nearly full)?
  a) $O(1)$
  b) $O(\log n)$
  c) $O(n)$
  d) $O(n^2)$
- **Q10. The Infinite Loop**
  In Cuckoo Hashing, if an insertion causes a "cycle" of evictions that exceeds a specific threshold (e.g., $MAX\_LOOP$), what does the algorithm do?
  a) Returns "Table Full" error.
  b) Deletes the oldest item.
  c) Forces the item into the secondary table using Linear Probing.
  d) Performs a **Rehash** (creates new tables with new hash functions and re-inserts everything).
  
  ---
- ### **Solutions & Explanations**
  
  **Q1: b) Index 6**
  *   **Math:** $3(15) + 5 = 45 + 5 = 50$.
  *   $50 \pmod{11} = 6$. (Since $11 \times 4 = 44$, remainder is 6).
  
  **Q2: d) 5**
  *   **Insert 12:** $12 \% 10 = 2$. Goes to Index 2.
  *   **Insert 22:** $22 \% 10 = 2$. Collision! Probe +1 $\to$ Index 3.
  *   **Insert 32:** $32 \% 10 = 2$. Collision! Probe +1 $\to$ 3 (Occupied). Probe +1 $\to$ Index 4.
  *   **Insert 42:** $42 \% 10 = 2$. Collision! Probe $\to$ 3 (Occupied) $\to$ 4 (Occupied) $\to$ **Index 5**.
  
  **Q3: b) Long runs of occupied slots...**
  *   **Concept:** This is the defining characteristic of Linear Probing. As a block grows, the probability of a new key hitting that block increases, making the block grow even faster.
  
  **Q4: b) Infinite loop probing the same index.**
  *   **Why:** The probe index is `start + i * step`. If step (`h2`) is 0, you check `start + 0`, then `start + 0`... you never look anywhere else.
  
  **Q5: c) 7**
  *   **Step 1 ($h_1$):** $27 \pmod{13} = 1$. (Occupied).
  *   **Step 2 (Calculate Step Size $h_2$):** $1 + (27 \pmod{11}) = 1 + 5 = 6$.
  *   **Step 3 (Probe):** $(1 + 6) \pmod{13} = 7$. Index 7 is the next spot checked.
  
  **Q6: b) Guaranteed $O(1)$ Worst-Case Lookup**
  *   **Why:** In Cuckoo hashing, a key is *either* at $T_1[h_1(x)]$ *or* $T_2[h_2(x)]$. You only ever have to check 2 spots. There is no probing chain to follow.
  
  **Q7: c) $T_2[2]$**
  *   **Logic:** The key 6 was evicted. It must move to its alternate location in **Table 2**.
  *   **Math:** $h_2(6) = (6/3) \pmod 3 = 2 \pmod 3 = 2$.
  *   It goes to $T_2$ index 2.
  
  **Q8: b) 50%**
  *   **Why:** Mathematically, once a Cuckoo table is half full, the probability of forming a cycle (infinite eviction loop) spikes dramatically.
  
  **Q9: c) $O(n)$**
  *   **Why:** In the worst case (Primary Clustering), you might have to scan every single slot in the array to find the empty spot that proves the key doesn't exist.
  
  **Q10: d) Performs a Rehash**
  *   **Why:** An infinite cycle implies the current hash functions map the current keys into a closed loop graph. The only way to fix this is to pick completely new hash functions and rebuild the table.
-
-
-
-
- ### **Topic: Bloom Filters & Probability Math**
  
  **Q1. The "Bits per Key" Variable**
  You have a Bloom filter with a bit array of size $n = 10,000$ bits. You insert $|S| = 2,000$ items. What is the value of **$b$** (bits per key)?
  a) 0.2
  b) 5
  c) 20
  d) 2000
  
  **Q2. Probability of a "0" Bit**
  Using the Dartboard analogy, if you throw one dart at a board with $n$ slots, the probability it hits a specific slot is $1/n$.
  If you insert 1 item using $m$ hash functions, what is the probability that a specific bit in the array **remains 0** (is NOT hit)?
  a) $1/n$
  b) $(1 - 1/n)$
  c) $(1 - 1/n)^m$
  d) $1 - (1 - 1/n)^m$
  
  **Q3. The Calculus Approximation**
  In the derivation of the False Positive rate, we use the limit definition of $e$ to simplify the expression $(1 - 1/n)^{m|S|}$.
  What is the simplified probability that a specific bit is a **1**?
  a) $e^{-m/b}$
  b) $1 - e^{-m/b}$
  c) $(1 - e^{-m/b})^m$
  d) $e^{-n}$
  
  **Q4. Calculating False Positive Rate**
  Given:
  *   $m = 3$ (3 hash functions)
  *   $b = 10$ (10 bits per key)
  *   $e^{-3/10} \approx 0.74$
  What is the approximate **False Positive Rate**?
  *(Hint: Use the formula $(1 - e^{-m/b})^m$)*
  a) $0.74$
  b) $0.26$
  c) $(0.26)^3 \approx 0.017$
  d) $(0.74)^3 \approx 0.40$
  
  **Q5. Optimal Hash Functions**
  You are designing a Bloom Filter and you have allocated $b = 8$ bits per key. According to the optimization formula using calculus, what is the **optimal number of hash functions ($m$)** to use?
  *(Hint: $\ln 2 \approx 0.69$)*
  a) $m \approx 4$
  b) $m \approx 6$
  c) $m \approx 8$
  d) $m \approx 12$
  
  **Q6. The Effect of Doubling Space**
  If you double the bits-per-key ($b$) from 8 to 16, what happens to the False Positive Rate (assuming you also optimize $m$)?
  a) It is cut in half (50% reduction).
  b) It decreases linearly.
  c) It is squared (e.g., $2\% \to 0.04\%$).
  d) It stays the same.
  
  **Q7. The "No False Negatives" Logic**
  Why can a Bloom Filter **never** produce a False Negative?
  a) Because the hash functions are perfect.
  b) Because if an element was inserted, its bits were definitely set to 1. They don't flip back to 0.
  c) Because we use multiple hash functions.
  d) Because of the collision resolution strategy.
  
  **Q8. Deletion Problem**
  Why can't you simply delete an item from a standard Bloom Filter by setting its hashed bits back to 0?
  a) It takes $O(n)$ time.
  b) You don't know which bits belong to the item.
  c) It might accidentally set a bit to 0 that *another* item was relying on, creating a False Negative for that other item.
  d) The hardware doesn't support writing 0s.
  
  **Q9. Optimal False Positive Rate Formula**
  If you choose the optimal number of hash functions ($m = b \ln 2$), the False Positive rate formula simplifies to a very clean expression dependent only on $b$. Which one is it?
  a) $(1/2)^b$
  b) $(1/2)^{b \ln 2}$
  c) $e^{-b}$
  d) $b^2$
  
  **Q10. Concrete Calculation**
  You have a Bloom Filter with $b=16$ bits per key. You use the optimal number of hash functions. What is the approximate False Positive rate?
  *(Use the formula from Q9. Note that $\ln 2 \approx 0.7$ and $2^{10} \approx 1000$)*.
  a) ~1%
  b) ~0.1%
  c) ~0.05%
  d) ~0.0001%
  
  ---
- ### **Solutions & Explanations**
  
  Q1: b) 5
  *   **Formula:** $b = n / |S|$.
  *   $b = 10,000 / 2,000 = 5$.
  
  Q2: c) $(1 - 1/n)^m$
  *   **Reasoning:**
    *   Prob of missing once: $(1 - 1/n)$.
    *   Since the $m$ hash functions are independent, we multiply the probability $m$ times.
  
  Q3: b) $1 - e^{-m/b}$
  *   **Reasoning:**
    *   Prob bit is 0: $e^{-m|S|/n} = e^{-m/b}$.
    *   Prob bit is 1: **$1 - (\text{Prob bit is 0})$**.
  
  Q4: c) $(0.26)^3 \approx 0.017$
  *   **Math:**
    *   $e^{-0.3} \approx 0.74$.
    *   Prob bit is 1: $1 - 0.74 = 0.26$.
    *   False Positive: (All 3 bits are 1) $= (0.26)^3 \approx 0.0175$ ($1.7\%$).
  
  Q5: b) $m \approx 6$
  *   **Formula:** $m = b \ln 2$.
  *   $m = 8 \times 0.693 \approx 5.54$.
  *   Since you can't have partial functions, you round to **6** (or 5).
  
  Q6: c) It is squared
  *   **Deep Dive:** The optimal rate is $(1/2)^{b \ln 2}$.
  *   If $b \to 2b$, the exponent doubles.
  *   $(0.5)^{2x} = ((0.5)^x)^2$. The rate is squared.
  
  Q7: b) Because if an element was inserted...
  *   **Concept:** A False Negative says "Item is NOT there" when it IS there.
  *   "Not there" means checking the bits and finding a 0.
  *   But if we inserted it, we set those bits to 1. They stay 1 forever. So we will never find a 0 for that item.
  
  Q8: c) It might accidentally set a bit to 0 that another item was relying on...
  *   **Concept:** This introduces False Negatives, breaking the one guarantee the filter has.
  
  Q9: b) $(1/2)^{b \ln 2}$
  *   **Math:**
    *   Optimal case means prob of bit being 1 is exactly $1/2$.
    *   Formula is $(1/2)^m$.
    *   Substitute optimal $m = b \ln 2$. Result: $(1/2)^{b \ln 2}$.
  
  Q10: c) ~0.05%
  *   **Math:**
    *   Rate $= (0.5)^{16 \times 0.7} = (0.5)^{11.2}$.
    *   $(0.5)^{10} \approx 1/1000 = 0.1\%$.
    *   $(0.5)^{11}$ is half of that $\approx 0.05\%$.
-
-
-
- ### **Part 1: Double Hashing**
  
  **Formula:** $idx = (h_1(k) + i \cdot h_2(k)) \pmod m$
  *   $i$ is the probe number ($0, 1, 2...$).
  
  **Problem 1:**
  You have a hash table of size $m = 13$.
  *   $h_1(k) = k \pmod{13}$
  *   $h_2(k) = 1 + (k \pmod{11})$
  
  Insert the following keys in order: **14, 27, 79**.
  Show the probe sequence and final index for each key.
  
  ***
  
  **Solution 1:**
  
  1.  **Insert 14:**
    *   $i=0$: $h_1(14) = 14 \pmod{13} = \mathbf{1}$.
    *   Index 1 is empty. Insert 14 at **Index 1**.
  
  2.  **Insert 27:**
    *   $i=0$: $h_1(27) = 27 \pmod{13} = \mathbf{1}$.
    *   **Collision!** Index 1 is taken by 14.
    *   Calculate Step Size: $h_2(27) = 1 + (27 \pmod{11}) = 1 + 5 = \mathbf{6}$.
    *   $i=1$: $(1 + 1 \cdot 6) \pmod{13} = 7$.
    *   Index 7 is empty. Insert 27 at **Index 7**.
  
  3.  **Insert 79:**
    *   $i=0$: $h_1(79) = 79 \pmod{13} = \mathbf{1}$.
    *   **Collision!** (Index 1 has 14).
    *   Calculate Step Size: $h_2(79) = 1 + (79 \pmod{11}) = 1 + 2 = \mathbf{3}$.
    *   $i=1$: $(1 + 1 \cdot 3) \pmod{13} = \mathbf{4}$.
    *   Index 4 is empty. Insert 79 at **Index 4**.
  
  **Final Table:** `{1: 14, 4: 79, 7: 27}`.
  
  ---
- ### **Part 2: Cuckoo Hashing**
  
  **Problem 2:**
  You have two hash tables, $T_1$ and $T_2$, each of size **11**.
  *   $h_1(k) = k \pmod{11}$
  *   $h_2(k) = (k / 11) \pmod{11}$ (Integer division)
  
  **Current State:**
  *   $T_1$ contains: `{20}` at index 9. (Since $20 \pmod{11} = 9$).
  *   $T_2$ contains: `{26}` at index 2. (Since $26/11 = 2$).
  
  **Task:** Insert the key **53**.
  Trace the evictions. Which key ends up where?
  
  ***
  
  **Solution 2:**
  
  1.  **Insert 53 into $T_1$:**
    *   $h_1(53) = 53 \pmod{11} = 9$.
    *   **Collision!** Index 9 is occupied by **20**.
    *   **Evict 20.** Place **53** in $T_1[9]$.
  
  2.  **Relocate 20 to $T_2$:**
    *   $h_2(20) = (20 / 11) \pmod{11} = 1$.
    *   Index 1 in $T_2$ is empty.
    *   Place **20** in $T_2[1]$.
  
  **Final State:**
  *   $T_1[9] = 53$
  *   $T_2[1] = 20$
  *   $T_2[2] = 26$ (Unchanged)
  
  ---
- ### **Part 3: Bloom Filter Math**
  
  **Problem 3 (The "Parameters" Question):**
  You are designing a Bloom Filter to store $|S| = 10,000$ items. You have allocated a bit array of size $n = 80,000$ bits.
  1.  What is $b$ (bits per key)?
  2.  What is the **optimal number of hash functions ($m$)** you should use? (Use $\ln 2 \approx 0.693$, round to nearest integer).
  
  **Problem 4 (The False Positive Calculation):**
  Using the values from Problem 3 ($b$ and optimal $m$):
  Calculate the theoretical **False Positive Rate**.
  *   Hint: Use the simplified formula for optimal $m$: $(1/2)^{m}$.
  
  ***
  
  **Solution 3:**
  1.  **Calculate $b$:**
    *   $b = n / |S| = 80,000 / 10,000 = \mathbf{8}$ bits per key.
  2.  **Calculate Optimal $m$:**
    *   Formula: $m = b \ln 2$.
    *   $m = 8 \times 0.693 = 5.544$.
    *   Round to nearest integer: **$m = 6$**.
  
  **Solution 4:**
  *   **Formula:** The optimal False Positive rate is roughly $(0.5)^m$ (or more accurately $(0.5)^{b \ln 2}$).
  *   Using $m=6$: $(0.5)^6 = 1 / 64$.
  *   **Result:** $1/64 \approx \mathbf{0.0156}$ (or **1.56%**).
  
  ---
- ### **Part 4: Bloom Filter Mechanics (The "Bit Trace")**
  
  **Problem 5:**
  You have a tiny Bloom Filter with $n = 8$ bits (indices 0-7), initialized to all 0s. You have $m = 2$ hash functions.
  *   $h_1(x) = x \pmod 8$
  *   $h_2(x) = (2x + 1) \pmod 8$
  
  1.  Insert key $X = 3$. Which bits flip?
  2.  Insert key $Y = 6$. Which bits flip?
  3.  Query key $Z = 7$.
    *   $h_1(7) = ?$
    *   $h_2(7) = ?$
    *   What does the filter return? (True/False)
    *   Is this a False Positive?
  
  ***
  
  **Solution 5:**
  
  1.  **Insert X = 3:**
    *   $h_1(3) = 3 \pmod 8 = \mathbf{3}$.
    *   $h_2(3) = (6+1) \pmod 8 = \mathbf{7}$.
    *   **Bits set:** `{3, 7}`.
  
  2.  **Insert Y = 6:**
    *   $h_1(6) = 6 \pmod 8 = \mathbf{6}$.
    *   $h_2(6) = (12+1) \pmod 8 = 13 \pmod 8 = \mathbf{5}$.
    *   **Bits set:** `{3, 7, 6, 5}`.
  
  3.  **Query Z = 7:**
    *   $h_1(7) = 7 \pmod 8 = \mathbf{7}$.
    *   $h_2(7) = (14+1) \pmod 8 = 15 \pmod 8 = \mathbf{7}$.
    *   **Check:** Is bit 7 set to 1? **Yes** (It was set by X).
    *   **Filter Returns:** **True** ("Possibly in set").
    *   **Is it a False Positive?** **Yes.** We never inserted 7, but the filter says it might be there because the bits collided.
-
-
-
-
- ### **Part 1: Manual Greedy Scheduling (The "Gantt Chart" Style)**
  
  **Problem 1:**
  You have **3 Processors** ($P=3$).
  You have **7 independent tasks** (no dependencies between them) with the following execution times:
  *   $t_1 = 4$s
  *   $t_2 = 2$s
  *   $t_3 = 2$s
  *   $t_4 = 5$s
  *   $t_5 = 1$s
  *   $t_6 = 3$s
  *   $t_7 = 5$s
  
  **Goal:** Create a "Greedy Schedule." Assign tasks to processors to finish as fast as possible.
  
  1.  What is the Total Work ($T_1$)?
  2.  What is the **perfectly ideal** time ($T_1/P$)?
  3.  Draw (or list) a schedule. Which processor runs what?
  4.  What is your final makespan (the time the last processor finishes)?
  
  ***
  
  **Solution 1:**
  
  1.  **Work ($T_1$):** $4+2+2+5+1+3+5 = \mathbf{22 \text{ seconds}}$.
  2.  **Ideal Time:** $22 / 3 = \mathbf{7.33 \text{ seconds}}$. (So we know we can't beat 7.33s).
  3.  **Sample Greedy Schedule:**
    *   **P1:** Takes $t_4$ (5s) + $t_2$ (2s) $\rightarrow$ Total: 7s.
    *   **P2:** Takes $t_7$ (5s) + $t_3$ (2s) $\rightarrow$ Total: 7s.
    *   **P3:** Takes $t_1$ (4s) + $t_6$ (3s) + $t_5$ (1s) $\rightarrow$ Total: 8s.
  4.  **Final Makespan:** The system finishes when P3 finishes. Time = **8 seconds**.
  This is very close to the ideal 7.33s, showing why Greedy is efficient
  
  ---
- ### **Part 2: Applying Brent's Theorem**
  
  **Formula:** $T_P \le \frac{T_1 - T_\infty}{P} + T_\infty$
  (Or the simplified upper bound: $T_P \le T_1/P + T_\infty$).
  
  **Problem 2:**
  You analyze a DAG and find:
  *   Total Work ($T_1$) = 500 seconds.
  *   Span ($T_\infty$) = 60 seconds.
  *   You have $P = 4$ processors.
  
  1.  Calculate the **Lower Bound** (the fastest it could possibly be).
  2.  Calculate the **Upper Bound** (the slowest a Greedy Scheduler is allowed to be).
  
  ***
  
  **Solution 2:**
  
  1.  **Lower Bound:** Max of (Work Law, Span Law).
    *   Work Law: $T_1/P = 500/4 = 125$.
    *   Span Law: $T_\infty = 60$.
    *   **Lower Bound:** **125 seconds**.
  
  2.  **Upper Bound (Brent's Theorem):**
    *   $T_4 \le \frac{500 - 60}{4} + 60$
    *   $T_4 \le \frac{440}{4} + 60$
    *   $T_4 \le 110 + 60$
    *   **Upper Bound:** **170 seconds**.
  
  *(So the actual runtime will be somewhere between 125s and 170s).*
  
  ---
- ### **Part 3: Dependency Graph Scheduling**
  
  **Problem 3:**
  Look at this simple dependency graph:
  *   **Task A (Start)** takes 10s.
  *   **Task B** (depends on A) takes 20s.
  *   **Task C** (depends on A) takes 20s.
  *   **Task D** (depends on B and C) takes 10s.
  
  1.  What is $T_1$?
  2.  What is $T_\infty$?
  3.  If you have $P=2$ processors, what is the completion time? Walk through the schedule.
  
  ***
  
  **Solution 3:**
  
  1.  Work ($T_1$): $10 + 20 + 20 + 10 = \mathbf{60}$.
  2.  Span ($T_\infty$): Critical path is $A \to B \to D$ (or $A \to C \to D$). $10 + 20 + 10 = \mathbf{40}$.
  3.  **Schedule on $P=2$:**
    *   **Time 0-10:** Only Task A is ready. P1 runs A. P2 is **Idle**. (Both wait).
    *   **Time 10:** A finishes. Now B and C are ready.
    *   **Time 10-30:** P1 runs B (20s). P2 runs C (20s).
    *   **Time 30:** Both finish. Task D is ready.
    *   **Time 30-40:** P1 runs D. P2 is **Idle**.
    *   **Total Time:** **40 seconds**.
  
  Even with 2 processors, we hit the Span time exactly ($T_\infty = 40$). We couldn't go faster because P2 was forced to sit idle during A and D).
-
-
-
-
-
- ### Refuting Processor Claims
  
  This problem tests if you can catch a researcher lying about their data using three hard laws of physics/math:
  1.  **Work Law:** $T_p \ge T_1 / P$ (You can't do work faster than dividing it evenly).
  2.  **Span Law:** $T_p \ge T_\infty$ (You can't beat the critical path, ever).
  3.  **Brent’s Theorem:** $T_p \le \frac{T_1 - T_\infty}{P} + T_\infty$ (The absolute maximum time a greedy scheduler should take).
  
  **The 3-Step Strategy to solve these:**
  1.  **Find the absolute MAXIMUM Work ($T_1$):** Look at all the data points. Multiply $P \times T_p$ for each. The **smallest** result is the maximum possible Work. ($T_1 \le \text{smallest } P \times T_p$).
  2.  **Find the absolute MAXIMUM Span ($T_\infty$):** Look at all the data points. The Span can never be higher than the fastest time recorded. ($T_\infty \le \text{smallest } T_p$).
  3.  **Brent's Theorem:** Take a middle data point. Plug your Max Work and Max Span into the right side of Brent's equation. If the math says the program should have finished in *40 seconds*, but the researcher claimed it took *45 seconds*, **they are lying** (or their scheduler is broken), because Brent's theorem dictates the *worst-case* upper bound.
  
  ---
- ### Practice Problems: Refuting Claims
- #### **Problem 1: The Cloud Startup**
  A startup claims their new parallel sorting algorithm achieved the following execution times on AWS:
  *   2 processors took **100 seconds**.
  *   10 processors took **28 seconds**.
  *   100 processors took **4 seconds**.
  
  Use the Work Law, Span Law, and Brent's Theorem to prove these results are mathematically inconsistent.
- #### **Problem 2: The Graphics Render**
  A graphics rendering engine reports the following benchmark results:
  *   1 processor took **500 seconds**.
  *   50 processors took **10 seconds**.
  *   10 processors took **60 seconds**.
  
  Use the Work Law, Span Law, and Brent's Theorem to prove these results are mathematically inconsistent.
  
  ***
- ### **Solutions**
  
  **Solution to Problem 1 (The Cloud Startup):**
  1.  **Find Max Work ($T_1 \le P \times T_p$):**
    *   $P=2: T_1 \le 2 \times 100 = 200$
    *   $P=10: T_1 \le 10 \times 28 = 280$
    *   $P=100: T_1 \le 100 \times 4 = 400$
    *   *Tightest bound:* $T_1 \le 200$
- 2.  Find Max Span ($T_\infty \le T_p$):
    *   Fastest time is 4 seconds (on 100 processors).
    *   *Tightest bound:* $T_\infty \le 4$.
- 3.  Spring the Trap (Brent's Theorem on $P=10$):
    *   $T_{10} \le \frac{T_1 - T_\infty}{10} + T_\infty$
    *   $T_{10} \le \frac{200 - 4}{10} + 4$
    *   $T_{10} \le \frac{196}{10} + 4$
    *   $T_{10} \le 19.6 + 4$
    *   $T_{10} \le \mathbf{23.6 \text{ seconds}}$.
- 4.  **Conclusion:** The math guarantees it should take *at most* 23.6 seconds on 10 processors. The startup claimed it took 28 seconds. The results are impossible.
- **Solution to Problem 2 (The Graphics Render):**
  1.  **Find Max Work ($T_1 \le P \times T_p$):**
    *   $P=1: T_1 \le 1 \times 500 = \mathbf{500}$.
- 2.  **Find Max Span ($T_\infty \le T_p$):**
    *   Fastest time is 10 seconds (on 50 processors).
    *   *Tightest bound:* **$T_\infty \le 10$**.
- 3.  **Spring the Trap (Brent's Theorem on $P=10$):**
    *   $T_{10} \le \frac{T_1 - T_\infty}{10} + T_\infty$
    *   $T_{10} \le \frac{500 - 10}{10} + 10$
    *   $T_{10} \le \frac{490}{10} + 10$
    *   $T_{10} \le 49 + 10$
    *   $T_{10} \le \mathbf{59 \text{ seconds}}$.
- 4.  **Conclusion:** Brent's theorem proves that for 10 processors, the maximum time allowed is 59 seconds. The engine reported 60 seconds. The results are inconsistent.