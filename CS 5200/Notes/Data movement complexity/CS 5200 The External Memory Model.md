### 1. The Core Problem: The Hierarchy of Memory
In your previous modules, we assumed that accessing `A[0]` and `A[1000000]` took the same amount of time. In reality, modern computers have a hierarchy:
*   **Registers/L1 Cache:** Blazing fast, tiny capacity.
*   **RAM (Main Memory):** Fast, moderate capacity (GBs).
*   **Disk (SSD/HDD):** Very slow, massive capacity (TBs).

**Data Movement Complexity** (also called I/O Complexity) measures how many times we have to move "blocks" of data between the slow levels and the fast levels.
- ### 2. The Standard Parameters (The "Deep Dive" Variables)
  To do the math for these slides, you must memorize these three variables:
  1.  $N$: Total number of elements in the problem (e.g., a 1 Terabyte file).
  2.  $M$: Capacity of the fast memory (RAM). How many elements can fit in RAM at once?
  3.  $B$: The **Block Size**. Data is not moved one integer at a time; it is moved in "pages" or "blocks." $B$ is the number of elements in one block.
  
  **Derived Variable:**
  *   $M/B$: The number of blocks that can fit in RAM at the same time. This is your "workspace" size.
  
  ---
- ### 3. The Rationale: Why $O(N)$ is a Failure (Slides 4 & 10)
  Consider the example on Slide 4: **Sorting 1 TB of data with only 3 GB of RAM.**
  *   $N = 10^{12}$ bytes.
  *   $M = 3 \times 10^9$ bytes.
  
  If you use a standard algorithm that ignores memory (like a naive Linked List traversal), you might trigger a "Page Fault" (Disk Read) for every single element.
  
  **The Math of "N" transfers (Slide 10):**
  *   Assume 1 ms per disk access (very generous for HDD).
  *   If you have 256,000,000 elements and you read them one by one ($N$ transfers):
    *   $256,000,000 \times 0.001 \text{ sec} = 256,000 \text{ seconds} \approx \mathbf{71 \text{ hours}}$.
  *   **The Goal:** We want **$N/B$** transfers. We want to read a block, process all $B$ items in it while they are in the fast cache, and then move to the next block.
  *   If $B = 8000$ (a standard block size):
    *   $32,000 \text{ transfers} \times 0.001 \text{ sec} = \mathbf{32 \text{ seconds}}$.
  
  **Professor's Deep Dive Takeaway:** An algorithm that is $O(N)$ in "Time Complexity" can be a total disaster in "I/O Complexity" if it has bad **Spatial Locality** (jumping around memory instead of reading linearly).
  
  ---
- ### 4. Classification of Algorithms (Slide 2)
  The slides mention three types of IO-efficient approaches:
  1.  **External Memory Algorithms:** Designed specifically knowing the values of $M$ and $B$.
  2.  **Cache-Efficient Algorithms:** Generic term for algorithms that minimize cache misses.
  3.  **Cache-Oblivious Algorithms:** (The most advanced). These are designed to be efficient **without** knowing what $M$ and $B$ are. They work well on any machine, from a phone to a supercomputer, because they use recursive structures (like the Divide and Conquer we just studied) to naturally fit into whatever cache size is available.
  
  ---
- ### Part 1 Practice Questions (Concept Check)
  
  **Q1: The Workspace**
  If your RAM ($M$) is 100 MB and your Block Size ($B$) is 1 MB, how many blocks can you "keep in flight" or compare at once in your internal memory?
  
  **Q2: Complexity Units**
  In internal memory, the unit of complexity is the "Instruction." In the External Memory Model, what is the fundamental unit of cost?
  
  **Q3: The Linked List vs. Array**
  In terms of $N$ and $B$, what is the I/O complexity of scanning a contiguous **Array** of size $N$? What is the I/O complexity of scanning a **Linked List** where every node is scattered in a different memory location?