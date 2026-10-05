### 1. The Ideal Cache Model
To analyze data movement mathematically, we use an "Ideal Cache." 
*   **Cache Lines (Blocks):** Fast memory is divided into slots. Each slot holds $L$ (or $B$) consecutive words.
*   **The "Rule of Movement":** You can never move just one number. You must move the entire line. 
*   **Cache Hit:** The CPU wants data that is already in RAM. **Cost = 0 I/Os.**
*   **Cache Miss:** The CPU wants data that is NOT in RAM. The system must pause, go to the disk, and fetch the block. **Cost = 1 I/O.**
- ### 2. The Eviction Problem
  Fast memory ($M$) is small. Eventually, it gets full. When you need a new block from the disk, you must "evict" (delete) an old block from the cache to make room. 
  
  **The Question:** Which block do you throw away to minimize future disk trips?
- #### A. The Optimal Strategy (Belady’s Algorithm)
  *   **Rule:** Evict the block whose next access is **farthest in the future**.
  *   **Deep Dive Note:** This is an **Offline Algorithm**. It requires "God Mode"—the ability to see the future of every instruction the program will ever run. It is impossible to implement in a real OS, but we use it as a "Perfect Benchmark" to compare other algorithms against.
- #### B. The Realistic Strategy (LRU - Least Recently Used)
  *   **Rule:** Evict the block that hasn't been touched for the longest amount of time.
  *   **Logic:** If you haven't used it in a while, you probably won't need it soon (Temporal Locality).
  *   **Deep Dive Note:** This is an **Online Algorithm**. It makes decisions based only on past history.
  
  ---
- ### 3. The Competitiveness Lemma (Slide 8 - The "Deep Dive" Math)
  Your professor cites **Lemma 1 (FLPR99)**. This is a vital theoretical result that essentially says: **"Don't worry that we can't see the future; LRU is good enough."**
  
  **The Lemma Statement:**
  If an Optimal algorithm makes $T$ transfers on a cache of size $M/2$, then **LRU** will make at most $2T$ transfers on a cache of size $M$.
  
  **Why is this a "Deep Dive" point?**
  1.  **The Resource War:** It shows that having more memory ($M$ vs $M/2$) compensates for not being able to see the future.
  2.  **The "Constant Factor":** In Big O notation, a factor of 2 is ignored. Therefore, theoretically, **LRU is just as efficient as the "Perfect" algorithm** asymptotically. This justifies why almost every modern operating system and CPU uses LRU (or a variation of it).
  
  ---
- ### 4. Theorem 1: The Cost of a "Scan" (Slide 9)
  This is the most basic building block of external memory complexity.
  
  **The Theorem:**
  Scanning $N$ elements stored contiguously in memory costs at most:
  $$\lceil N/B \rceil + 1 \text{ memory transfers.}$$
  
  **The "Professor Deep Dive" Explanation: Why the "+1"?**
  If $N=10$ and $B=5$, you might think it always takes 2 blocks ($10/5 = 2$).
  **However**, data is not always "aligned" with block boundaries.
  *   Imagine the 10 elements start at the very end of Block 1.
  *   Block 1 contains the first 2 elements.
  *   Block 2 contains the next 5 elements.
  *   Block 3 contains the last 3 elements.
  *   **Total transfers = 3.**
  *   The **"+1"** accounts for this "misalignment" at the start and end of the array.
  
  ---
- ### Part 2 Practice Questions
  
  **Q1: The Eviction Logic**
  Your cache holds 3 blocks. Your program accesses blocks in this order: `A, B, C, D, A, B`.
  1.  Using **LRU**, which block is evicted when `D` is called? 
  2.  Using **Optimal (Belady)**, which block is evicted when `D` is called? (Assume you know the rest of the sequence).
  
  **Q2: The Competitiveness Lemma**
  If a "Perfect" algorithm can sort a file using 500 disk reads on a 1GB machine, what is the *maximum* number of disk reads an LRU-based machine with 2GB would take?
  
  **Q3: Array Alignment**
  If $N = 1,000,000$ and $B = 1,000$, what is the *best-case* number of transfers to read the whole array, and what is the *worst-case* number?