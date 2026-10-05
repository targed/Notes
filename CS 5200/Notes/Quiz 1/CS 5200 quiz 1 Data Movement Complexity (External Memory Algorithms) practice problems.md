### **Topic 4: Data Movement & External Memory**
*(Relevant Slides: Data movement complexity.pptx)*

**Q1. The Scanning Formula**
You need to scan an array of $N$ elements stored contiguously on disk. The block size is $B$. What is the **worst-case** number of memory transfers required?
a) $N/B$
b) $\lceil N/B \rceil$
c) $\lceil N/B \rceil + 1$
d) $N$

**Q2. Linked List vs. Array**
In the External Memory model ($N$ elements, Block size $B$), why is traversing a standard Linked List significantly slower than traversing an Array?
a) Linked Lists require extra memory for pointers.
b) Linked Lists have $O(N)$ time complexity while Arrays have $O(1)$ access.
c) Linked List nodes might be scattered across different blocks, potentially causing **$N$** I/Os instead of **$N/B$**.
d) CPU caching does not work for pointers.

**Q3. Binary Search in External Memory**
If you perform a standard Binary Search on a sorted array of size $N$ on disk, what is the I/O complexity?
a) $\Theta(\log_2 N)$
b) $\Theta(\log_2 (N/B))$
c) $\Theta(\log_B N)$
d) $\Theta(N/B)$

**Q4. The B-Tree Advantage**
Why is a B-Tree search ($\log_B N$) faster than Binary Search ($\log_2 N$) for external memory?
a) B-Trees are perfectly balanced, whereas Binary Search trees might not be.
b) The base of the logarithm is $B$ (e.g., 1000) instead of 2, making the tree height much shorter.
c) B-Trees store data in leaves only.
d) B-Trees use compression to fit more data in a block.

**Q5. Real-World Calculation**
Given $N = 1,000,000$ and Block Size $B = 1,000$.
How many I/Os does **B-Tree Search** take versus **Binary Search**?
*(Approximation is fine)*
a) B-Tree: ~2, Binary: ~10
b) B-Tree: ~2, Binary: ~1,000
c) B-Tree: ~2, Binary: ~20
d) B-Tree: ~1,000, Binary: ~20

**Q6. External Merge Sort**
In External Merge Sort, we don't just merge 2 lists at a time. We merge $k$ lists at a time. What is the optimal value for $k$ to minimize disk I/O, given RAM size $M$ and block size $B$?
a) $k = 2$
b) $k = \log N$
c) $k = M/B$
d) $k = N/B$

**Q7. LRU Competitiveness**
The "Sleator-Tarjan" competitiveness property (mentioned in slides as Lemma 1) states that LRU on a cache of size $M$ is competitive with the Optimal (Belady) algorithm on a cache of size:
a) $M$
b) $M/2$
c) $2M$
d) $M - B$

**Q8. Matrix Traversal (Spatial Locality)**
Assume a matrix is stored in **Row-Major** order (row by row). If you iterate through the matrix by **Columns** (accessing `A[0][0]`, `A[1][0]`, `A[2][0]`), what is the I/O complexity in the worst case?
a) $O(N^2 / B)$
b) $O(N^2)$ (One miss per element)
c) $O(N^2 / M)$
d) $O(N / B)$

**Q9. The "Memory Wall"**
Why do we study Data Movement Complexity separately from Time Complexity?
a) Because hard drives are getting faster than CPUs.
b) Because modern CPUs are so fast that fetching data from disk (I/O) is often the actual bottleneck (10ms vs 1ns).
c) Because Big O notation cannot handle variables like $B$ and $M$.
d) Because parallel algorithms don't use caching.

**Q10. Reversing an Array**
To reverse a massive array on disk efficiently (minimizing I/Os), what is the best strategy?
a) Load the entire array into RAM, reverse it, write it back.
b) Swap `A[i]` and `A[N-i]` one by one.
c) Read a block from the start and a block from the end, swap their contents in RAM, and write both blocks back.
d) Use a stack data structure on disk.

---
- ### **Solutions & Explanations**
  
  **Q1: c) $\lceil N/B \rceil + 1$**
  *   **Why:** The $+1$ accounts for alignment. If your array starts in the middle of a block, you have to load that block just to read the first few items. You also might need an extra block for the tail end.
  
  **Q2: c) Linked List nodes might be scattered...**
  *   **Why:** In an array, `A[i]` and `A[i+1]` are neighbors in memory (Spatial Locality). In a linked list, `Node -> Next` points to a random address. If every node is in a different disk block, you pay 1 I/O per node ($N$ total), whereas the array pays 1 I/O per $B$ nodes ($N/B$ total).
  
  **Q3: b) $\Theta(\log_2 (N/B))$**
  *   **Why:** Standard Binary Search cuts the range by 2 every step.
    *   Phase 1: The search range is huge. Every jump lands in a new block. Cost = 1 I/O per step.
    *   Phase 2: The search range becomes $\le B$. The entire range fits in one block. Cost = 0 I/Os (Cache Hit).
    *   Total steps: $\log N$. Steps inside last block: $\log B$.
    *   Result: $\log N - \log B = \log(N/B)$.
  
  **Q4: b) The base of the logarithm is $B$...**
  *   **Why:** A B-Tree node fits $B$ keys. When you load one node (1 I/O), you can split the search space into $B$ parts, not just 2. The height of the tree drops from $\log_2 N$ to $\log_B N$.
  
  **Q5: a) B-Tree: ~2, Binary: ~10**
  *   **Calculations:**
    *   **Binary Search:** $\log_2 (1,000,000 / 1,000) = \log_2 (1,000) \approx 10$ I/Os.
    *   **B-Tree:** $\log_{1000} (1,000,000) = 2$ I/Os. (Because $1000^2 = 1,000,000$).
    *   *Note:* Slide 14 uses specific numbers, but the magnitude difference is the key takeaway.
  
  **Q6: c) $k = M/B$**
  *   **Why:** To merge lists, you need to keep the "head" block of each list in RAM. If RAM can hold $M/B$ blocks, you can merge $M/B$ lists simultaneously. This maximizes the branching factor of the merge tree, minimizing the height (passes over disk).
  
  **Q7: b) $M/2$**
  *   **Why:** The Lemma states that LRU with size $M$ makes at most $2\times$ the mistakes of an Optimal algorithm running with half the memory ($M/2$). This proves LRU is "good enough."
  
  **Q8: b) $O(N^2)$ (One miss per element)**
  *   **Why:**
    *   Matrix size is $N \times N = N^2$ elements.
    *   If stored by rows, accessing by columns means you jump $N$ spots in memory for every step.
    *   Unless $N$ is very small, each jump lands in a new block. You effectively load a whole block to read 1 integer, then throw it away.
    *   Total transfers = Total elements = $N^2$. (Compared to $N^2/B$ for row-major traversal).
  
  **Q9: b) Because modern CPUs are so fast...**
  *   **Why:** This is the motivation for the entire chapter. The "Memory Wall" means improvements in CPU speed don't help if we are waiting for the disk.
  
  **Q10: c) Read a block from start and end...**
  *   **Why:**
    *   (a) fails because $N$ is "massive" (doesn't fit in RAM).
    *   (b) is correct logic but slow; it implies 2 I/Os per swap if not buffered.
    *   (c) describes **Blocking**. You load a chunk of the start and a chunk of the end into the limited RAM ($M$), swap everything you can between those buffers, and write them back. This achieves $O(N/B)$ efficiency.