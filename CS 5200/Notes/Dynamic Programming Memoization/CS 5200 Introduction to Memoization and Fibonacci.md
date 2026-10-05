### 1. The Core Problem: Overlapping Subproblems
In Divide & Conquer (like Merge Sort), we split an array in half, sort the left, and sort the right. The left half and right half are completely **disjoint** (they don't overlap). 

However, many real-world math and optimization problems do not split cleanly. When you try to use simple recursion, you end up solving the **exact same sub-problem** over and over again.
- ### 2. The Poster Child: Fibonacci (Slides 7–11)
  The Fibonacci sequence is defined as $F(n) = F(n-1) + F(n-2)$, where $F(0) = 0$ and $F(1) = 1$.
  
  **The Naive Recursive Code:**
  ```c
  int fib(int i) {
    if (i == 0) return 0;
    if (i == 1) return 1;
    return fib(i - 1) + fib(i - 2);
  }
  ```
  **The Flaw (Slide 10):**
  If you trace the recursion tree for `fib(5)`, it looks like this:
  *   `fib(5)` calls `fib(4)` and `fib(3)`.
  *   `fib(4)` calls `fib(3)` and `fib(2)`.
  *   *Wait!* We are now calculating `fib(3)` twice from scratch.
  *   If you look at the tree on Slide 10, `fib(2)` is calculated **3 times**, and `fib(1)` is calculated **5 times**!
  
  **The Complexity:**
  *   Because the tree branches by 2 at almost every step down to depth $N$, the **Time Complexity is roughly $O(2^N)$** (Exponential time).
  *   As shown in Slide 9, this exponential curve shoots straight up. Calculating `fib(40)` might take seconds, but `fib(100)` would take longer than the universe has existed!
- ### 3. The Solution: Memoization (Slides 12–14)
  **"Memoization"** comes from the word "memo" (to take notes). It is a **Top-Down** approach to dynamic programming. 
  
  **The Logic:**
  Why do the work twice? The very first time we calculate `fib(3)`, we should write the answer down on a notepad. The next time the computer asks for `fib(3)`, we just read it off the notepad instead of doing the math again!
  
  **The Memoized Code:**
  ```c
  int fib(int i, int[] cache) {
    // 1. Base cases
    if (i == 0) return 0;
    if (i == 1) return 1;
  
    // 2. CHECK THE CACHE
    if (cache[i] == 0) { 
        // 3. We haven't solved it yet. Do the math and SAVE IT.
        cache[i] = fib(i - 1, cache) + fib(i - 2, cache);
    }
  
    // 4. Return the saved answer
    return cache[i];
  }
  ```
- ### 4. The Deep Dive: How the Tree changes (Slide 13)
  Look at the array tracking the cache.
  When `fib(5)` runs, it calls `fib(4)`, which calls `fib(3)`, which calls `fib(2)`, which returns 1. 
  We save `cache[2] = 1`.
  We save `cache[3] = 2`.
  We save `cache[4] = 3`.
  
  When `fib(5)` finally gets around to making its right-hand call to `fib(3)`, it asks the cache: "Do you have index 3?" The cache says "Yes, it's 2." 
  **The entire right side of the recursion tree is instantly pruned!** It takes $O(1)$ time to return.
  
  **The New Complexity:**
  *   **Time Complexity:** We only ever calculate each number from 1 to $N$ exactly once. **$O(N)$**. We turned an impossible exponential algorithm into a blazing-fast linear one!
  *   **Space Complexity:** The call stack goes $N$ deep, and our `cache` array is size $N+1$. Total Space is **$O(N)$**.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1: The DP Requirement**
  Which of the following is the primary reason that Memoization speeds up the Fibonacci algorithm, but does NOT speed up standard Merge Sort?
  A) Merge Sort does not use recursion.
  B) Fibonacci sub-problems overlap, while Merge Sort sub-problems are disjoint.
  C) Merge Sort uses an auxiliary array.
  D) Fibonacci is addition, while Merge Sort is comparisons.
  
  **Q2: Cache Initialization**
  In the provided memoized Fibonacci code, we check `if (cache[i] == 0)` to see if we have done the work yet. 
  If we were calculating a sequence where `0` is a valid mathematical answer to a sub-problem, why would this array initialization cause a bug, and how would you fix it?
  
  **Q3: Complexity Analysis**
  If you run the naive (un-memoized) recursive `fib(n)` algorithm, what is the **Space Complexity**?
  *(Hint: Think back to the Parallel Algorithms chapter regarding recursive stack depth!).*
  
  **Q4: Top-Down vs. Bottom-Up**
  Memoization is a "Top-Down" approach (you start at $N$ and recursively call down to $0$). 
  Can you think of a way to calculate `fib(N)` in $O(N)$ time *without* using recursion at all? *(Hint: Think "Bottom-Up", starting at 0 and 1).*
  
  ---
-