### **Practice Exam: Asymptotic Analysis**

**Question 1: Time Complexity (The Subtraction Loop)**
Find the tightest asymptotic time complexity ($\Theta$) of the following method.
```cpp
int modulo_sim(int a, int b) {
  int count = 0;
  while (a >= b) {
      a = a - b;
      count++;
  }
  return count;
}
```

**Question 2: Space Complexity (Recursive Tree)**
Find the space complexity of the following method in terms of $n$.
```cpp
void compute(int n) {
  if (n <= 1) 
      return;
  compute(n / 2);
  compute(n / 2);
}
```

**Question 3: Data Movement Complexity (The Exponential Jump)**
Find the data movement (I/O) complexity of the function `getValue`. Assume the array is massive, stored on a disk with block size $B$, and RAM is limited. 
```cpp
int getValue(int Arr[], int n) {
  for (int i = 1; i < n; i = i * 2) {
      Arr[i] = Arr[i] + 1;
  }
  return Arr[n-1];
}
```

**Question 4: Time Complexity (The Nested Trick)**
Find the time complexity of the following method.
```cpp
void analyze(int n) {
  for (int i = 1; i <= n; i = i * 2) {
      for (int j = 0; j < i; j++) {
          System.out.println(i + j);
      }
  }
}
```

**Question 5: Data Movement Complexity (Array vs. Linked List)**
You have a dataset of $N$ integers. You must iterate through every integer and print it. 
What is the theoretical data movement complexity if the data is stored in a contiguous **Array** versus a **Linked List**? Assume a block size of $B$. Briefly explain why they are different.

**Question 6: Space Complexity (Auxiliary Loops)**
Find the space complexity of the following method.
```cpp
void doWork(int n) {
  for (int i = 0; i < n; i++) {
      int[] temp = new int[n];
      temp[0] = i;
      System.out.println(temp[0]);
  }
}
```

**Question 7: Formal Definitions (The Constant Trap)**
Evaluate the following two statements as **True or False**. Briefly justify your answer.
1. $2^{n+5} = \Theta(2^n)$
2. $3^{2n} = \Theta(3^n)$

**Question 8: Data Movement Complexity (Search)**
You are searching for a specific record in a database of $N$ items stored on a hard drive. What is the data movement complexity of using a standard **Binary Search** versus using a **B-Tree Search**? Give the formulas in terms of $N$ and $B$.

**Question 9: Time Complexity (Multiple Variables)**
Find the time complexity of the following method.
```cpp
void printData(int A[], int a, int B[], int b) {
  for(int i = 0; i < a; i++) {
      for(int j = 0; j < 1000; j++) {
          System.out.println(A[i]);
      }
  }
  for(int k = 0; k < b; k++) {
      System.out.println(B[k]);
  }
}
```

**Question 10: Formal Proof (Big-O)**
Use the formal definition of Big-O ($c$ and $n_0$) to prove that $f(n) = 5n^2 + 3n$ is $O(n^2)$.

---
---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: Time Complexity = $\Theta(a/b)$**
  *   **Explanation:** Just like your Quiz 1 question where `sum` increased by `b` up to `a`, here `a` is decreased by `b` until it is less than `b`. The loop runs exactly $a \div b$ times. Therefore, the time complexity is strictly dependent on the ratio of the two inputs.
  
  **Answer 2: Space Complexity = $\Theta(\log n)$**
  *   **Explanation:** This is a classic trick. Even though the function makes $O(n)$ *total* recursive calls (creating a full tree), space complexity is determined by the **maximum depth of the call stack** at any single moment. Because the input size halves at each step, the tree only goes $\log_2 n$ levels deep before returning. 
  
  **Answer 3: Data Movement = $\Theta(\log n)$**
  *   **Explanation:** The variable `i` jumps: 1, 2, 4, 8, 16, 32... up to $n$. There are $\log_2 n$ total loop iterations. Because the jumps grow exponentially, the indices very quickly become spaced out by distances much greater than the block size $B$. This destroys *spatial locality*. Therefore, almost every single access requires fetching a brand new block from memory. Since you make $\log n$ accesses, you trigger $\Theta(\log n)$ block transfers. 
  
  **Answer 4: Time Complexity = $\Theta(n)$**
  *   **Explanation:** Do not blindly guess $n \log n$ just because you see a doubling loop and a nested loop! Let's trace the inner loop:
    *   When $i=1$, inner loop runs 1 time.
    *   When $i=2$, inner loop runs 2 times.
    *   When $i=4$, inner loop runs 4 times.
    *   The total work is the geometric series: $1 + 2 + 4 + 8 + \dots + n$. 
    *   The sum of this series is exactly $2n - 1$. Dropping constants, we get $\Theta(n)$.
  
  **Answer 5: Array = $\Theta(N/B)$, Linked List = $\Theta(N)$**
  *   **Explanation:** An array stores data contiguously. When you load one integer, the OS loads the entire block ($B$ items), meaning the next $B-1$ items are "free" (cache hits). A Linked List scatters nodes randomly in memory. In the worst case, every single `node->next` points to a completely different memory block, causing a cache miss (1 transfer) for every single item.
  
  **Answer 6: Space Complexity = $\Theta(n)$**
  *   **Explanation:** Inside the loop, we allocate an array of size $n$. However, in Java/C++, once the iteration finishes, that specific `temp` array goes out of scope and the memory can be reclaimed. You never hold more than one array of size $n$ in memory *at the exact same time*. Thus, the maximum auxiliary space used at any moment is $\Theta(n)$.
  
  **Answer 7: 1 is True, 2 is False**
  *   **Explanation:** 
    *   1. $2^{n+5} = 2^5 \cdot 2^n = 32 \cdot 2^n$. Since 32 is a constant, we drop it. $\Theta(2^n)$ is **True**.
    *   2. $3^{2n} = (3^2)^n = 9^n$. The function $9^n$ grows infinitely faster than $3^n$. It is **False**.
  
  **Answer 8: Binary Search = $\Theta(\log_2(N/B))$, B-Tree = $\Theta(\log_B N)$**
  *   **Explanation:** Standard binary search halves the search space, but the first several jumps result in cache misses, giving $\log_2(N/B)$. A B-Tree loads a block of size $B$ into memory, allowing the CPU to instantly branch the search space by $B$ instead of 2. This changes the logarithmic base to $B$, resulting in massively fewer disk reads.
  
  **Answer 9: Time Complexity = $\Theta(a + b)$**
  *   **Explanation:** 
    *   The first loop runs $a \times 1000$ times. Since 1000 is a constant, it becomes $\Theta(a)$.
    *   The second loop runs $b$ times $\rightarrow \Theta(b)$.
    *   Because the loops are sequential (one after the other), we add them: $\Theta(a + b)$. You cannot simplify this to $O(N)$ because $a$ and $b$ are entirely separate inputs!
  
  **Answer 10: Formal Big-O Proof**
  *   **Proof:** We need to find $c$ and $n_0$ such that $5n^2 + 3n \le c \cdot n^2$ for all $n \ge n_0$.
    *   Let's assume $n \ge 1$. If $n \ge 1$, then $n \le n^2$.
    *   Therefore, $3n \le 3n^2$.
    *   Substitute this into the original equation: $5n^2 + 3n \le 5n^2 + 3n^2$.
    *   $5n^2 + 3n \le 8n^2$.
    *   We have satisfied the definition with **$c = 8$** and **$n_0 = 1$**.
  
  ---
- ---
- ---
- ### **Practice Exam: Data Movement Complexity**
  
  **Question 1: Data Movement Code Analysis (The Strided Loop)**
  Find the Data Movement (I/O) complexity of the following method. Assume `A` is an array of $N$ integers stored contiguously on disk, and $B$ is the block size.
  ```cpp
  int getSum(int A[], int n) {
    int sum = 0;
    for (int i = 0; i < n; i += 2) {
        sum += A[i];
    }
    return sum;
  }
  ```
  
  **Question 2: Data Movement Code Analysis (Column-Major Traversal)**
  Find the Data Movement complexity of the following method. Assume `Matrix` is a massive $N \times N$ 2D array stored in **Row-Major order** (elements of the same row are adjacent on disk), and $N \gg B$.
  ```cpp
  int sumMatrix(int Matrix[][], int n) {
    int sum = 0;
    for (int col = 0; col < n; col++) {
        for (int row = 0; row < n; row++) {
            sum += Matrix[row][col];
        }
    }
    return sum;
  }
  ```
  
  **Question 3: The Alignment Nuance**
  According to Theorem 1 in your slides, the number of memory transfers required to scan $N$ elements stored contiguously is bounded by $\lceil N/B \rceil + 1$. 
  Why is the "+1" necessary? Briefly explain.
  
  **Question 4: Linked List vs. Array Memory**
  You have an array of size $N$ and a Linked List of size $N$. Both are unsorted. You need to find the maximum value in each. 
  What is the Data Movement complexity for the Array, and what is the Data Movement complexity for the Linked List? Why are they different?
  
  **Question 5: Two-Pointer Data Movement**
  Find the Data Movement complexity of this array reversal method. Assume $N$ is massive and RAM ($M$) is small (but can hold at least 2 blocks).
  ```cpp
  void reverse(int A[], int n) {
    int left = 0;
    int right = n - 1;
    while (left < right) {
        int temp = A[left];
        A[left] = A[right];
        A[right] = temp;
        left++;
        right--;
    }
  }
  ```
  
  **Question 6: B-Tree vs. Binary Search Math**
  You are searching a database with $N = 10^{12}$ (1 Trillion) records. Your block size $B = 10,000$. 
  1. Approximately how many disk transfers does a standard Binary Search require?
  2. Approximately how many disk transfers does a B-Tree search require?
  *(Note: $\log_2(1000) \approx 10$, so $\log_2(10^{12}) \approx 40$)*.
  
  **Question 7: External Merge Sort Height**
  In a standard internal RAM Merge Sort, the height of the recursion tree is $\log_2 N$. 
  In an optimized External Merge Sort with memory capacity $M$ and block size $B$, what is the height of the merge tree?
  
  **Question 8: The Competitiveness Lemma**
  A competitor claims their proprietary "future-predicting" (Optimal) cache eviction algorithm can sort a 10TB dataset using only 50,000 disk transfers on a server with **16 GB of RAM**.
  Using the Competitiveness Lemma (Sleator-Tarjan), what is the maximum number of disk transfers an ordinary **LRU (Least Recently Used)** algorithm would take to process the same data, provided you upgrade the server to **32 GB of RAM**?
  
  **Question 9: The Eviction Trace**
  Assume your cache can hold exactly **3 blocks** ($M = 3B$). You access disk blocks in the following sequence:
  `[Block 1], [Block 2], [Block 3],[Block 4], [Block 1]`
  If your system uses **LRU** eviction, what block is evicted when `Block 4` is requested, and what block is evicted when `Block 1` is requested the second time?
  
  **Question 10: Multi-way Merge Logic**
  In External Memory Mergesort, the algorithm merges lists in batches of $M/B$. Why do we use $M/B$ instead of $N/B$ or just 2?
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: Data Movement = $\Theta(N/B)$**
  *   **Explanation:** The time complexity (CPU instructions) is $N/2 = \Theta(N)$, because you skip every other element. However, the disk doesn't load individual integers; it loads whole blocks of size $B$. When you access `A[0]`, the OS loads `A[0]` through `A[B-1]`. Therefore, reading `A[2]`, `A[4]`, etc., are all "free" cache hits until you cross the block boundary. You still load every block in the array exactly once, making the I/O complexity $\Theta(N/B)$.
  
  **Answer 2: Data Movement = $\Theta(N^2)$**
  *   **Explanation:** The matrix is stored row-by-row on the disk. Because the loops traverse *column-by-column* (the inner loop changes the `row` index), the CPU jumps to a completely different memory block on every single iteration. Assuming the matrix is too big to fit in RAM, every single access results in a Cache Miss. There are $N^2$ elements accessed, resulting in $N^2$ memory transfers. *(Note: If the loops were swapped, it would just be $O(N^2/B)$).*
  
  **Answer 3: Block Misalignment**
  *   **Explanation:** Data stored on a disk is rarely perfectly aligned with the exact start of a block. If you need to read 10 elements, but the first element starts at the very end of Block 1, you must load Block 1 for that single element, and then load Block 2 for the remaining 9 elements. The $+1$ accounts for crossing an extra block boundary due to misalignment.
  
  **Answer 4: Array = $\Theta(N/B)$, Linked List = $\Theta(N)$**
  *   **Explanation:** Arrays have strict *Spatial Locality*. A single disk read grabs $B$ adjacent elements, giving you $B-1$ cache hits. Linked Lists use pointers to memory locations that can be scattered anywhere on the disk. In the worst case, every `node->next` requires fetching a brand new disk block, resulting in 1 memory transfer per node.
  
  **Answer 5: Data Movement = $\Theta(N/B)$**
  *   **Explanation:** We have two pointers: one moving left-to-right, one moving right-to-left. 
    *   The `left` pointer scans linearly, incurring $1$ transfer every $B$ steps.
    *   The `right` pointer scans linearly backwards, incurring $1$ transfer every $B$ steps.
    *   As long as RAM ($M$) can hold at least 2 blocks (one for the front, one for the back), the blocks don't overwrite each other in the cache. Total I/O is $(N/2)/B + (N/2)/B = N/B$. 
  
  **Answer 6: Binary Search $\approx 27$, B-Tree $\approx 3$**
  *   **Explanation:**
    *   **Binary Search:** $\log_2(N/B) = \log_2(10^{12} / 10^4) = \log_2(10^8)$. Since $2^{10} \approx 10^3$, then $2^{26} \approx 10^8$. So, roughly **26 to 27 transfers**.
    *   **B-Tree:** $\log_B N = \log_{10000} (10^{12})$. This is exactly **3 transfers** ($10000^3 = 10^{12}$).
  
  **Answer 7: Height = $\log_{M/B} (N/M)$  (or practically $\log_{M/B} (N/B)$)**
  *   **Explanation:** Instead of dividing the problem by 2 at each level, an external mergesort divides the problem by the number of lists that can be merged simultaneously in RAM, which is $M/B$. Therefore, the base of the logarithm changes from 2 to $M/B$. 
  
  **Answer 8: Maximum 100,000 transfers**
  *   **Explanation:** Lemma 1 states that LRU on a cache of size $M$ makes at most $2T$ transfers compared to the Optimal algorithm running on a cache of size $M/2$.
    *   The Optimal algorithm used $M/2 = 16$ GB and took $T = 50,000$ transfers.
    *   If LRU is given $M = 32$ GB, it will take at most $2 \times 50,000 = \mathbf{100,000}$ transfers.
  
  **Answer 9: Evicts Block 1, then Evicts Block 2**
  *   **Explanation:**
    *   Load B1, B2, B3: Cache holds `[B1, B2, B3]`.
    *   Load B4: Cache is full. LRU looks at the oldest used block, which is **Block 1**. B1 is evicted. Cache holds `[B4, B2, B3]`.
    *   Load B1 (again): Cache is full. The oldest used block is now **Block 2**. B2 is evicted. Cache holds `[B4, B1, B3]`.
  
  **Answer 10: To maximize the "Fan-out" (Branching Factor)**
  *   **Explanation:** To merge lists, you must keep at least 1 block from each list in RAM. Because RAM can hold $M$ total data, and each block is size $B$, the maximum number of blocks (and thus, lists) you can hold at once is exactly $M/B$. Merging more than 2 lists at a time flattens the tree, vastly reducing the number of passes over the disk.
  
  ---
- ---
- ---
- ### **Practice Exam: Divide & Conquer**
  
  **Question 1: Inversion Counting (Manual Trace)**
  You are using the modified Merge Sort algorithm to count inversions. You are currently at the `Merge` step, combining two sorted sub-arrays: 
  `Left = [5, 8, 12]` (indices `i` from 0 to 2)
  `Right = [2, 9, 10]` (indices `j` from 3 to 5)
  Assume `mid = 2`. When the element `2` is copied into the temporary array, exactly how many split inversions are added to the `count` variable?
  A) 1
  B) 2
  C) 3
  D) 4
  
  **Question 2: Rotated Array Logic**
  You are searching for a target $X = 5$ in a rotated sorted array. 
  The current state is: `A =[20, 25, 30, 2, 5, 8, 12]`, `low = 0`, `high = 6`, `mid = 3` (Value at mid is `2`).
  Based on the Divide and Conquer logic for rotated arrays, what happens next?
  A) The algorithm checks if `A[low] <= A[mid]`. It evaluates to True, so it searches the left half.
  B) The algorithm checks if `A[low] <= A[mid]`. It evaluates to False. It then checks if $X$ is between `A[mid]` and `A[high]`. It evaluates to True, so it searches the right half.
  C) The algorithm checks if $X < A[mid]$. Since $5 > 2$, it automatically searches the right half.
  D) The algorithm returns the index of 5.
  
  **Question 3: Hybrid Merge Sort (Complexity Check)**
  In the Hybrid Merge Sort algorithm, you stop recursing when the sub-array size reaches $k$, and you use Insertion Sort on those small arrays. 
  If a programmer mistakenly sets the threshold to $k = \sqrt{n}$, what will the worst-case time complexity of the resulting Hybrid Merge Sort be?
  A) $\Theta(n \log n)$
  B) $\Theta(n \sqrt{n})$
  C) $\Theta(n^2)$
  D) $\Theta(\sqrt{n} \log n)$
  
  **Question 4: Tournament Tree (Code Analysis)**
  Consider the following pseudo-code for finding the maximum element:
  ```cpp
  int findMax(int A[], int low, int high) {
    if (low == high) return A[low];
    int mid = (low + high) / 2;
    int leftMax = findMax(A, low, mid);
    int rightMax = findMax(A, mid + 1, high);
    return max(leftMax, rightMax);
  }
  ```
  What is the **Space Complexity** (auxiliary memory) of this algorithm for an array of size $N$, and why?
  
  **Question 5: Serendipity (Fixed Point) Edge Case**
  The Serendipity algorithm (finding $A[i] == i$) relies on the array containing strictly **sorted, distinct** integers. 
  If the array is sorted but contains **duplicates** (e.g., `[-5, 2, 2, 2, 4, 7]`), what happens to the standard $O(\log n)$ Binary Search logic?
  A) It still works perfectly in $O(\log n)$ time.
  B) It will return the first occurrence of the duplicate.
  C) The mathematical "slope" guarantee is broken; the algorithm cannot safely discard half the array, degrading the worst-case time to $O(n)$.
  D) It will enter an infinite loop.
  
  **Question 6: Surrounding Numbers (Pointer Crossing)**
  You are using a standard iterative Binary Search to find the surrounding numbers for $X = 14$ in the array `A =[2, 5, 8, 12, 16, 23, 38]`. $X$ is not in the array.
  When the `while (low <= high)` loop finally terminates, what values will `A[low]` and `A[high]` point to?
  A) `A[low]` = 12, `A[high]` = 16
  B) `A[low]` = 16, `A[high]` = 12
  C) `A[low]` = 14, `A[high]` = -1
  D) Both will point to 16.
  
  **Question 7: Sparse Search (Worst Case)**
  In Sparse Search (binary search with empty strings `""`), if `A[mid] == ""`, we move pointers outward to find a non-empty string to act as the pivot. 
  In the absolute worst-case scenario, what does the time complexity of this step become, and why?
  
  **Question 8: Inversions (The "Piggyback" Formula)**
  In the `Merge-And-Count` subroutine, you have pointers `i` for the left array and `j` for the right array. The midpoint of the original array is `mid`. 
  When you find that `A[i] > A[j]`, you copy `A[j]` to the temp array. What is the exact mathematical expression added to the inversion `count`?
  
  **Question 9: Tournament Tree (Comparisons)**
  If you have an array of 64 elements, exactly how many comparisons are made to find the maximum element using the recursive Tournament Tree method?
  A) 64
  B) 63
  C) 32
  D) 6
  
  **Question 10: Divide and Conquer Core Concepts**
  Which of the following is a strict requirement for a problem to be solvable efficiently using the pure Divide and Conquer paradigm (like Merge Sort)?
  A) The sub-problems must overlap so their results can be memoized.
  B) The problem must be partitioned into exactly two halves.
  C) The sub-problems must be disjoint (independent and non-overlapping).
  D) The combination step must take $O(1)$ time.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: C) 3**
  *   **Explanation:** The left pointer `i` is at index 0 (value 5). The right pointer `j` is at index 3 (value 2). Since 5 > 2, we have an inversion! Because the left array is sorted, if 5 is greater than 2, then 8 and 12 are *also* greater than 2. The formula is `mid - i + 1`. Here, `mid = 2`, `i = 0`. So, $2 - 0 + 1 = \mathbf{3}$ split inversions. 
  
  **Answer 2: B) Evaluates to False, checks bounds, searches Right.**
  *   **Explanation:** 
    1. Identify the sorted half: `A[low] (20) <= A[mid] (2)`. This is **False**. Therefore, the *Right* half `[2, 5, 8, 12]` is the strictly sorted half.
    2. Check boundaries: Is $X$ (5) between `A[mid]` (2) and `A[high]` (12)? **Yes**.
    3. Action: Discard the left half. Search the Right half.
  
  **Answer 3: B) $\Theta(n \sqrt{n})$**
  *   **Explanation:** The general formula for Hybrid Merge Sort is $O(nk + n \log(n/k))$. If we substitute $k = \sqrt{n}$, the insertion sort portion becomes $n \times \sqrt{n} = n^{1.5}$ (or $n\sqrt{n}$). Since $n\sqrt{n}$ grows much faster than $n \log n$, it dominates the equation. This proves why $k$ must be kept very small (like $k \le \log n$) to preserve the $O(n \log n)$ speed!
  
  **Answer 4: Space Complexity = $O(\log n)$**
  *   **Explanation:** You do not allocate any new arrays (like Merge Sort does), so there is no $O(n)$ data structure overhead. However, the recursive calls go down to single elements. The maximum depth of the call stack for an array divided by 2 at each step is $\log_2 n$. Each stack frame holds a few $O(1)$ variables (`mid`, `leftMax`).
  
  **Answer 5: C) The mathematical guarantee is broken, degrading to $O(n)$.**
  *   **Explanation:** In a distinct array, if `A[mid] < mid` (e.g., `A[5] = 2`), we know everything to the left is even smaller, so the answer *must* be to the right. If duplicates are allowed, the values can "flatline" (e.g., `A[4]=2, A[3]=2, A[2]=2`). The serendipity point could be hiding on *either* side. You lose the ability to safely discard half the array.
  
  **Answer 6: B) `A[low]` = 16, `A[high]` = 12**
  *   **Explanation:** This is the core trick of the "Surrounding Numbers" algorithm! When a standard binary search fails to find a target, the pointers cross. `low` will move past `high` and end up pointing to the **Ceiling** (the smallest number larger than $X$). `high` will point to the **Floor** (the largest number smaller than $X$).
  
  **Answer 7: $O(n)$ because the array might be entirely empty strings.**
  *   **Explanation:** If the array is `["", "", "", "", ""]` and you are searching for "apple", the algorithm looks at `mid`, sees `""`, and starts stepping outward. It will step all the way to the edges of the array without finding a single word to compare against. This linear scan breaks the $O(\log n)$ binary search performance.
  
  **Answer 8: `count = count + (mid - i + 1)`**
  *   **Explanation:** As detailed in Question 1, this formula calculates the number of elements remaining in the left subarray. Because the left array is sorted, if the current left element `A[i]` is greater than `A[j]`, every element after `A[i]` up to `mid` is also greater than `A[j]`.
  
  **Answer 9: B) 63**
  *   **Explanation:** Think of it like a real sports tournament. If there are 64 teams, how many matches do you need to play to crown exactly one champion? Every match eliminates exactly 1 team. To eliminate 63 teams, you must play exactly **63 matches** ($N-1$). 
  
  **Answer 10: C) The sub-problems must be disjoint.**
  *   **Explanation:** This is the fundamental difference between Divide and Conquer and Dynamic Programming. In Divide and Conquer, the sub-problems (like the left half and right half of an array) do not overlap. If they *did* overlap, you would be re-computing the same work over and over, which requires Dynamic Programming (memoization) to be efficient.
  
  ---
- ---
- ### **Practice Exam: Solving Recurrences**
  
  **Question 1: Master Theorem (Case 1)**
  Derive the time complexity using the Master Theorem for $T(n) = 8T(n/2) + O(n^2)$. 
  *(Note: Be sure to explicitly list $a, b, f(n)$, and the critical value).*
  
  **Question 2: The Regularity Condition (Case 3)**
  Given the recurrence $T(n) = 2T(n/2) + n^3$:
  1. Identify which case of the Master Theorem applies.
  2. Mathematically prove the "Regularity Condition" ($a \cdot f(n/b) \le c \cdot f(n)$ for $c < 1$) to justify your answer.
  
  **Question 3: The "Gap" Trap**
  A student tries to use the Master Theorem to solve $T(n) = 2T(n/2) + n \log n$. They state: *"Since $f(n) = n \log n$ is strictly larger than the critical value $n^1$, this is Case 3, and the answer is $\Theta(n \log n)$."*
  Explain exactly why this student is mathematically incorrect.
  
  **Question 4: Recursion Tree (Tracing Level Work)**
  For the recurrence $T(n) = 3T(n/4) + cn^2$:
  Write out the exact mathematical expression for the total work done at **Level 0 (Root)**, **Level 1**, and **Level 2**. 
  *(This directly prepares you for the drawing question from your first quiz).*
  
  **Question 5: Recursion Tree (Finding the Leaves)**
  For the recurrence $T(n) = 5T(n/2) + n$:
  1. What is the height ($h$) of the recursion tree?
  2. What is the exact mathematical formula for the total number of leaves at the bottom of the tree?
  
  **Question 6: Asymmetric Recursion Trees**
  Consider the recurrence $T(n) = T(n/3) + T(2n/3) + n$. 
  Because the tree splits unevenly, the branches hit the base cases at different times.
  1. What is the height of the **shortest** branch in this tree?
  2. What is the height of the **longest** branch in this tree?
  
  **Question 7: Algorithm to Recurrence**
  You design a new Divide and Conquer algorithm. It divides an array of size $n$ into **3 equal-sized segments**. It recursively calls itself on **all 3 segments**. Finally, it takes $\Theta(n^2)$ time to combine the results. 
  Write the recurrence relation $T(n)$ for this algorithm, and solve it using the Master Theorem.
  
  **Question 8: Master Theorem (Case 2 Application)**
  Derive the time complexity using the Master Theorem for $T(n) = 4T(n/2) + cn^2$.
  
  **Question 9: Recursion Tree (Geometric Series)**
  You draw a recursion tree and find that the work at the levels is: $n^3$, $\frac{n^3}{2}$, $\frac{n^3}{4}$, $\frac{n^3}{8} \dots$
  1. Is this tree Root-Heavy, Leaf-Heavy, or Balanced?
  2. When you sum this infinite geometric series, what is the final asymptotic time complexity $\Theta$?
  
  **Question 10: The $a=1$ Edge Case**
  Derive the time complexity for standard Binary Search: $T(n) = T(n/2) + c$ using the Master Theorem.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: Master Theorem Case 1**
  *   **Step 1:** $a = 8$, $b = 2$, $f(n) = n^2$.
  *   **Step 2 (Critical Value):** $n^{\log_b a} = n^{\log_2 8} = \mathbf{n^3}$.
  *   **Step 3 (Compare):** Compare $f(n) = n^2$ to the critical value $n^3$. Since $n^3$ is polynomially larger than $n^2$, the leaves dominate. 
  *   **Result:** This is **Case 1**. $T(n) = \mathbf{\Theta(n^3)}$.
  
  **Answer 2: Regularity Condition**
  *   **Step 1:** $a = 2$, $b = 2$, $f(n) = n^3$. Critical value $n^{\log_2 2} = n^1$. Since $n^3 > n^1$ polynomially, this is **Case 3**.
  *   **Step 2 (Regularity Proof):** We must show $a f(n/b) \le c f(n)$ for some $c < 1$.
    *   Substitute the variables: $2 \cdot (n/2)^3 \le c \cdot n^3$
    *   Simplify: $2 \cdot \frac{n^3}{8} \le c \cdot n^3$
    *   Simplify: $\frac{1}{4}n^3 \le c \cdot n^3$
    *   Divide by $n^3$: $\mathbf{\frac{1}{4} \le c}$.
    *   Since we can pick $c = 1/4$ (which is strictly less than 1), the regularity condition holds. $T(n) = \mathbf{\Theta(n^3)}$.
  
  **Answer 3: The "Gap" Trap**
  *   **Explanation:** The student is wrong because the Master Theorem requires $f(n)$ to be **polynomially** larger than $n^{\log_b a}$ (meaning it must be larger by a factor of at least $n^\epsilon$ for some $\epsilon > 0$). Here, the critical value is $n^1$. The function is $n \log n$. The ratio between them is $\log n$, which is a logarithmic factor, not a polynomial factor. It falls into the "Gap" between Case 2 and Case 3, meaning the Master Theorem **cannot be applied**. *(Note: Using a recursion tree, the actual answer is $\Theta(n \log^2 n)$).*
  
  **Answer 4: Tracing Level Work**
  *   **Level 0 (Root):** 1 node. Work = **$cn^2$**.
  *   **Level 1:** 3 nodes. Each node does $c(n/4)^2$ work. Total = $3 \cdot c(n^2/16) =$ **$\frac{3}{16}cn^2$**.
  *   **Level 2:** 9 nodes ($3^2$). Each node does $c(n/16)^2$ work. Total = $9 \cdot c(n^2/256) =$ **$\frac{9}{256}cn^2$** (or $(3/16)^2 cn^2$).
  *   *(Professor Note: Writing it this way perfectly shows the decreasing geometric series $3/16$, proving it is Root-Heavy).*
  
  **Answer 5: Finding the Leaves**
  *   **Height ($h$):** We divide by 2 until we hit 1. $n/2^h = 1 \implies \mathbf{h = \log_2 n}$.
  *   **Number of Leaves:** The formula is $a^h$. 
    *   $5^{\log_2 n}$
    *   Apply the log swap rule ($x^{\log_y z} = z^{\log_y x}$): **$n^{\log_2 5}$**.
  
  **Answer 6: Asymmetric Trees**
  *   **Shortest Branch:** Follows the $n/3$ path. It shrinks the fastest. Height = $\mathbf{\log_3 n}$.
  *   **Longest Branch:** Follows the $2n/3$ path (which is the same as dividing by $3/2$ or $1.5$). It shrinks the slowest. Height = **$\log_{3/2} n$**.
  
  **Answer 7: Algorithm to Recurrence**
  *   **Recurrence:** $T(n) = \mathbf{3T(n/3) + \Theta(n^2)}$
  *   **Master Theorem:** $a = 3$, $b = 3$, $f(n) = n^2$.
  *   **Critical Value:** $n^{\log_3 3} = n^1$.
  *   **Compare:** $f(n) = n^2$ is polynomially larger than $n^1$. 
  *   **Result:** Case 3 (Root dominates). $T(n) = \mathbf{\Theta(n^2)}$.
  
  **Answer 8: Master Theorem Case 2**
  *   **Step 1:** $a = 4$, $b = 2$, $f(n) = n^2$. (Drop the constant $c$).
  *   **Step 2 (Critical Value):** $n^{\log_2 4} = \mathbf{n^2}$.
  *   **Step 3 (Compare):** $f(n) = n^2$ and critical value is $n^2$. They are exactly equal.
  *   **Result:** This is **Case 2**. We multiply the critical value by $\log n$. $T(n) = \mathbf{\Theta(n^2 \log n)}$.
  
  **Answer 9: Geometric Series**
  *   **1. Root-Heavy:** The work is decreasing by half at every level. Therefore, the Root ($n^3$) does the vast majority of the work.
  *   **2. Final $\Theta$:** The sum of a decreasing geometric series $S = a + ar + ar^2...$ (where $r < 1$) converges to $\frac{a}{1-r}$. Here, $\frac{n^3}{1 - 0.5} = 2n^3$. Dropping the constant gives **$\Theta(n^3)$**.
  
  **Answer 10: The $a=1$ Edge Case**
  *   **Step 1:** $a = 1$, $b = 2$, $f(n) = c$ (which is $n^0$).
  *   **Step 2 (Critical Value):** $n^{\log_2 1} = \mathbf{n^0} = 1$.
  *   **Step 3 (Compare):** $f(n) = 1$ and critical value $= 1$. They are equal.
  *   **Result:** This is **Case 2**. Multiply by $\log n \implies 1 \cdot \log n = \mathbf{\Theta(\log n)}$.
  
  ---
- ---
- ---
- ### **Practice Exam: Hashing & Bloom Filters**
  
  **Question 1: Linear Probing Trace (Primary Clustering)**
  You have a hash table of size $m = 7$ (indices 0 to 6), initialized to empty. 
  Your hash function is $h(k) = k \pmod 7$. 
  Using **Linear Probing**, you insert the following keys in order: `[8, 15, 22]`.
  At which exact index does the key `22` get stored? Show your steps.
  
  **Question 2: Double Hashing Trace**
  You have a hash table of size $m = 7$. 
  $h_1(k) = k \pmod 7$
  $h_2(k) = 1 + (k \pmod 5)$
  Assume that index **3** is already occupied.
  You want to insert the key $k = 17$. Calculate the step size (jump distance) and determine the final index where `17` will be inserted.
  
  **Question 3: The $h_2$ Rule**
  In the Double Hashing formula $idx = (h_1(k) + i \cdot h_2(k)) \pmod m$, what would happen if $h_2(k)$ evaluated to $0$ for a specific key, and the primary index was already occupied? 
  
  **Question 4: Cuckoo Hashing Eviction Trace**
  You have two tables, $T_1$ and $T_2$, both of size 5.
  $h_1(k) = k \pmod 5$
  $h_2(k) = (k / 5) \pmod 5$ *(using integer division)*
  Currently, $T_1[2]$ holds the value `12`. All other slots in both tables are empty.
  You insert the key `7`. 
  Trace the Cuckoo hashing process. What are the final locations of `7` and `12`?
  
  **Question 5: Cuckoo Hashing vs. Open Addressing**
  Why would a high-frequency trading platform or a high-speed internet router choose Cuckoo Hashing over Double Hashing, despite Cuckoo Hashing's risk of infinite eviction loops?
  
  **Question 6: Algorithm Design (3-SUM)**
  The 2-SUM problem can be solved in expected $O(N)$ time using a Hash Map. 
  Briefly describe how you would use that 2-SUM Hash Map approach to solve the **3-SUM problem** ($x + y + z = t$), and state the expected time complexity of your 3-SUM algorithm.
  
  **Question 7: Bloom Filter Variables**
  You are building a Bloom filter for a Bitcoin wallet. You expect to insert $|S| = 500$ transactions. You allocate a bit array of size $n = 4,000$ bits. 
  1. What is the value of $b$ (bits per key)?
  2. According to the calculus optimization formula, approximately how many hash functions ($m$) should you use? *(Assume $\ln 2 \approx 0.7$)*.
  
  **Question 8: The Dartboard Probability**
  In the derivation of the Bloom filter's false positive rate, what does the mathematical expression $(1 - 1/n)^{m|S|}$ represent?
  A) The probability that all bits in the array are 1.
  B) The probability that a specific bit is still 0 after all items are inserted.
  C) The false positive rate.
  D) The load factor of the array.
  
  **Question 9: The Penalty of Deletion**
  A developer decides to implement a "Delete" function for a standard Bloom Filter by simply running the key through the $m$ hash functions and flipping those specific bits back to `0`. 
  What specific, catastrophic mathematical guarantee does this destroy, and why?
  
  **Question 10: Doubling the Space**
  You have a Bloom Filter with $b=8$ bits per key. The False Positive rate is approximately 2%. 
  If you redesign the system to be willing to use **twice as much RAM** ($b=16$ bits per key) and adjust the hash functions optimally, what happens to the False Positive rate?
  A) It halves to roughly 1%.
  B) It decreases linearly to roughly 0.5%.
  C) It is squared, dropping to roughly 0.04%.
  D) It drops to 0%.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: Index 3**
  *   **Step 1 (Insert 8):** $h(8) = 8 \pmod 7 = 1$. Index 1 is empty. Array is `[_, 8, _, _, _, _, _]`.
  *   **Step 2 (Insert 15):** $h(15) = 15 \pmod 7 = 1$. Collision! Probe $+1 \rightarrow$ Index 2. Array is `[_, 8, 15, _, _, _, _]`.
  *   **Step 3 (Insert 22):** $h(22) = 22 \pmod 7 = 1$. Collision! Probe $+1 \rightarrow$ Index 2 (Collision!). Probe $+1 \rightarrow$ Index 3. 
  *   *Deep Dive:* This perfectly illustrates **Primary Clustering**. Even though 15 and 22 have plenty of empty space in the array, they get dragged into the cluster that formed at index 1.
  
  **Answer 2: Jump Distance = 3, Final Index = 6**
  *   **Step 1 (Primary Hash):** $h_1(17) = 17 \pmod 7 = 3$. The problem states index 3 is occupied. Collision!
  *   **Step 2 (Calculate Jump):** $h_2(17) = 1 + (17 \pmod 5) = 1 + 2 = \mathbf{3}$.
  *   **Step 3 (Next Probe):** Index = $(3 + 1 \cdot 3) \pmod 7 = 6 \pmod 7 = \mathbf{6}$.
  
  **Answer 3: Infinite Loop**
  *   If $h_2(k) = 0$, the step size is zero. The probe sequence would be: `start + 0`, `start + 0`, `start + 0`. The algorithm would check the exact same occupied slot forever, resulting in an infinite loop. (This is why your professor's formula adds `1 +` to the modulo).
  
  **Answer 4: `7` is at $T_1[2]$, and `12` is at $T_2[2]$**
  *   **Step 1:** Evaluate `7` for Table 1. $h_1(7) = 7 \pmod 5 = 2$.
  *   **Step 2:** Collision! $T_1[2]$ is occupied by `12`.
  *   **Step 3:** Evict `12`. Put `7` into $T_1[2]$.
  *   **Step 4:** Evaluate new home for `12` in Table 2. $h_2(12) = (12 / 5) \pmod 5 = 2 \pmod 5 = 2$.
  *   **Step 5:** $T_2[2]$ is empty. Place `12` there.
  
  **Answer 5: Guaranteed $O(1)$ worst-case lookup time.**
  *   In Open Addressing, a lookup might have to traverse a long cluster of data, resulting in $O(N)$ worst-case time. In Cuckoo Hashing, a key can *only* exist in exactly two places: $T_1[h_1(k)]$ or $T_2[h_2(k)]$. If it's not in those two specific spots, it's not in the table. Lookups take exactly 2 checks maximum, providing absolute guarantee on latency.
  
  **Answer 6: Loop to fix $x$, then 2-SUM the rest in $\Theta(n^2)$ time.**
  *   **Logic:** Iterate through the array with a loop, picking a number to be $x$. For each $x$, you are looking for two other numbers that sum to $(t - x)$. This reduces the problem to 2-SUM.
  *   **Complexity:** The 2-SUM Hash Map approach takes expected $O(n)$ time. Since you run 2-SUM $n$ times (once for every $x$), the total expected time is $n \times O(n) = \mathbf{\Theta(n^2)}$.
  
  **Answer 7: $b = 8$, Optimal $m = 5$ or $6$**
  *   1. $b = n / |S| = 4000 / 500 = \mathbf{8}$.
  *   2. $m = b \ln 2 = 8 \times 0.7 = \mathbf{5.6}$. (Round to 6, though some tests accept 5).
  
  **Answer 8: B) The probability that a specific bit is still 0.**
  *   $(1 - 1/n)$ is the probability that one "dart" misses the bit.
  *   $m|S|$ is the total number of darts thrown (hash functions $\times$ items).
  *   Raising the miss chance to the total number of darts gives the probability that the bit survives the entire insertion process untouched.
  
  **Answer 9: It introduces False Negatives.**
  *   A Bloom filter guarantees **No False Negatives** (if it says it's not there, it's 100% not there). If you flip bits from 1 back to 0 to delete "Apple", you might inadvertently flip a bit that "Banana" also relies on. Later, if you query "Banana", the filter will see a 0 and confidently claim "Banana is NOT in the set," even though it was inserted. This breaks the fundamental law of the data structure.
  
  **Answer 10: C) It is squared, dropping to roughly 0.04%.**
  *   **Explanation:** The optimal false positive formula simplifies to $(1/2)^{b \ln 2}$. Because $b$ is in the exponent, doubling $b$ mathematically squares the fraction. $(0.02)^2 = 0.0004 = 0.04\%$. (This was specifically highlighted in the CLRS problem from your slides!).
  
  ---
- ---
- ---