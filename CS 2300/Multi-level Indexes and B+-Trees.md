### **Part 5: Multi-level Indexes**

**1. The Problem with Single-Level Indexes**
*   As the data file grows, the Index file also grows.
*   Eventually, the Index file becomes too large to fit into main memory (RAM).
*   Searching a massive index stored on disk (even using Binary Search) involves too many disk I/O operations ($log_2 b$), which is slow.

**2. The Solution: Indexing the Index**
*   Since the primary index file is already **sorted** by the key, we can treat it just like a data file.
*   We build a **Second-Level Index** on top of the First-Level Index.
*   We can repeat this process until the top-level index fits into a **single disk block**.
*   **Structure:** This creates a hierarchy (a tree).
  *   Top Level: Root.
  *   Middle Levels: Directory nodes.
  *   Base Level: The actual index pointing to data.

**3. Performance**
*   If a multi-level index has $t$ levels, retrieving a record takes roughly **$t + 1$ disk accesses** (one for each index level + one for the data).
*   This is significantly faster than binary searching a massive single file on disk.

**4. The Limitation **
*   Standard multi-level indexes are **Static**. They rely on the physical files being perfectly sorted and contiguous.
*   **Insertion/Deletion is a nightmare:** Adding a record might require shifting data in the base file, which shifts the first-level index, which shifts the second-level index... essentially rewriting the whole structure.
*   **Solution:** We need a **Dynamic** multi-level index that can expand and shrink gracefully. Enter the **B+-Tree**.

---
- ### **Part 6: B+-Trees (Structure and Properties)**
  
  The B+-Tree is the industry-standard data structure for database indexing. It is a **Self-Balancing Search Tree**.
  
  **1. Structure**
  *   **Nodes = Blocks:** Each node in the tree corresponds to exactly one disk block.
  *   **Leaf Nodes:** All pointers to actual data records exist **only** at the leaf level. The leaves are linked together in a linked list to allow fast sequential access (Range Scans).
  *   **Internal Nodes:** These are purely for navigation. They contain keys and pointers to child nodes, but no data.
  
  **2. Properties**
  *   **Balanced:** The tree is height-balanced. Every path from the root to a leaf has the exact same length.
  *   **Fanout ($F$):** The average number of children a node has. Because disk blocks are large (e.g., 4KB) and keys are small, the fanout is usually very high (e.g., 100+ pointers per node).
    *   *Result:* The tree is very short (shallow). Even with millions of records, the height is usually only 3 or 4 levels deep.
  *   **Occupancy ($d$):** The tree ensures space efficiency by enforcing a minimum occupancy.
    *   $d$ is the **Order** of the tree.
    *   Every node (except the root) must be at least **50% full** ($d$ entries).
    *   Every node can hold at most $2d$ entries.
  
  **3. Searching**
  *   **Equality Search:** Start at root, compare key, follow the pointer down to the leaf. (Cost = Height of tree).
  *   **Range Search:** Find the starting value (Equality Search). Once at the leaf, follow the **sibling pointers** (linked list) to retrieve subsequent records until the range end is reached. This is extremely efficient.