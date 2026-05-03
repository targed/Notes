### **Part 1: Storage Hierarchy and Database Organization**

**1. The Memory Hierarchy**
Computers have different types of storage arranged in a hierarchy based on speed, cost, and capacity (Slide 3).
*   **Primary Storage (Volatile):** Fast but expensive and loses data when power is off. Includes **Cache** and **Main Memory (RAM)**.
*   **Secondary Storage (Non-volatile):** Slower but cheaper and permanent. Includes **Magnetic Disks (HDD)** and **Flash Memory (SSD)**. This is where databases live.
*   **Tertiary Storage:** Optical disks and tapes used for archiving.

**2. Why Databases use Secondary Storage (Slide 4)**
Databases are rarely stored entirely in RAM for three reasons:
1.  **Size:** Databases are often too large to fit in RAM.
2.  **Persistence:** RAM is volatile; secondary storage is non-volatile (data survives power loss).
3.  **Cost:** Disk storage is much cheaper per unit of data than RAM.

**3. File Organizations (Slide 6)**
How we store data on the disk determines how fast we can retrieve it.
*   **Primary File Organizations:** Determines how records are *physically* placed on the disk. A table can only have **one** primary organization (e.g., Heap, Sorted, Hashed).
*   **Secondary File Organizations:** Auxiliary structures (Indexes) that allow efficient access based on different fields. A table can have **many** secondary indexes.

---
- ### **Part 2: Disk Hardware Characteristics**
  
  **1. Magnetic Disk Structure**
  *   **Platters:** Circular disks coated with magnetic material.
  *   **Tracks:** Concentric circles on a single surface.
  *   **Cylinders:** The set of all tracks with the same diameter across all stacked surfaces. (Reading data from the same cylinder is faster because the read/write heads don't need to move).
  *   **Sectors:** The smallest addressable block on a track (often 512 bytes).
  
  **2. The Bottleneck (Slide 12)**
  Locating data on a magnetic disk is the major performance bottleneck in databases due to mechanical movements:
  *   **Seek Time:** Moving the arm to the correct cylinder.
  *   **Rotational Delay (Latency):** Waiting for the disk to spin the correct sector under the head.
  
  **3. Addressing**
  A physical address on a disk typically consists of: `Cylinder # + Track # + Block/Sector #`.
  
  ---
- ### **Part 3: Records, Blocks, and Files**
  
  **1. The Hierarchy of Data (Slide 13)**
  *   **Fields** make up **Records**.
  *   **Records** are stored in **Blocks** (the unit of data transfer between disk and RAM).
  *   **Blocks** make up **Files**.
  
  **2. Spanned vs. Unspanned Records (Slides 15–16)**
  *   **Unspanned:** A record must fit entirely within one block. If there is wasted space at the end of a block, it is left empty.
  *   **Spanned:** A record can be split across two blocks.
    *   *Use case:* Necessary if a single record is larger than the block size.
    *   *Mechanism:* A pointer at the end of the first block points to the location of the rest of the record in the next block.
  
  ---
- ### **Part 4: Allocating File Blocks**
  
  How does the Operating System or DBMS keep track of which blocks belong to which file?
  
  1.  **Contiguous Allocation:**
    *   Blocks are stored right next to each other (e.g., blocks 1, 2, 3).
    *   *Pros:* Very fast reading (minimal head movement).
    *   *Cons:* Difficult to expand the file (might need to move the whole thing if the next block is occupied).
  2.  **Linked Allocation:**
    *   Each block contains a pointer to the next block.
    *   *Pros:* Easy to expand.
    *   *Cons:* Slow to read (must jump around the disk).
  3.  **Indexed Allocation:**
    *   An **Index Block** holds a list of pointers to all the data blocks.
    *   *Pros:* Supports direct access to any part of the file.
  
  ---
- ### **Part 5: Primary File Organizations (Heap & Sorted)**
  
  **1. Heap Files (Unordered)**
  *   **Structure:** Records are placed in the file in the order they are inserted (chronological). New records are appended to the end (last block).
  *   **Insertion:** Very Efficient ($O(1)$).
  *   **Search:** Very Expensive (Linear Search). On average, you must scan half the file.
  *   **Deletion:** Can leave unused space ("holes") requiring periodic reorganization (garbage collection).
  
  **2. Sorted Files (Ordered / Sequential)**
  *   **Structure:** Records are physically sorted on disk based on an **Ordering Field**.
  *   **Search:** Very Efficient. Can use **Binary Search** ($log_2 b$).
  *   **Insertion:** Very Expensive. To keep the order, you may have to shift half the records in the file to make room for a new one.
  *   **Note:** If the ordering field is NOT a key (values are not unique), it is called an **Ordered Clustered File**.
  
  ---
- ### **Part 6: Hashing Techniques**
  
  **1. Static External Hashing**
  *   **Concept:** Instead of searching, we calculate the address.
  *   **Mechanism:** A **Hash Function** is applied to the **Hash Key** of a record. The result (h(K)) determines the **Bucket** address where the record is stored.
  *   **Efficiency:** Very fast for **Equality Searches** (e.g., `WHERE ID = 105`).
  *   **Weakness:** Poor for **Range Queries** (e.g., `WHERE ID > 100`) or pattern matching, as the hash function randomizes the location.
  
  **2. Collisions and Overflow**
  *   **Collision:** When two different records hash to the same bucket, and the bucket is full.
  *   **Overflow Handling:** We create an **Overflow Bucket** and link it to the main bucket using a pointer. This creates a linked list of blocks for that bucket (Slide 34).
  
  **3. The Problem with Static Hashing (Slide 35)**
  Static hashing uses a **fixed** number of buckets.
  *   If the file shrinks, space is wasted.
  *   If the file grows, collisions increase, creating long overflow chains that degrade performance.
  *   *Solution:* Dynamic hashing (expandable buckets), though this lecture focuses on static.
  
  ---
- ### **Part 7: Indexes**
  
  An Index is an auxiliary access structure (like a library card catalog) separate from the data file. It allows fast lookup without scanning the whole table.
  
  **1. Primary Index (Slides 38–40)**
  *   **Definition:** An index on the **Ordering Key field** of a sorted data file.
  *   **Structure:** It has two fields: `<Key Value, Block Pointer>`.
  *   **Sparse Index:** It does *not* have an entry for every record. It usually has one entry per **Block** (Anchor Record). This makes the index file very small and fast to search.
  
  **2. Clustering Index (Slide 46)**
  *   **Definition:** An index on an **Ordering Non-Key field**.
  *   *Example:* A file sorted by `DepartmentID` (where many employees share the same ID).
  *   **Requirement:** The data file must be physically sorted by this field.
  
  **3. Secondary Index (Slide 45)**
  *   **Definition:** An index on any field that is **NOT** the ordering field of the data file.
  *   **Structure:** Because the data file is not sorted by this field, the index must be **Dense** (it must have an entry for every single record pointer), making it larger than a primary index.
  *   **Benefit:** Allows fast lookups on columns other than the Primary Key.
  
  **4. Special Indexes**
  *   **Hash Index (Slides 41–43):** Uses hashing for the secondary structure. Excellent for equality lookups, poor for ranges.
  *   **Bitmap Index (Slides 47–48):**
    *   Used for columns with **Low Cardinality** (few unique values, like Gender: M/F, or Status: Active/Inactive).
    *   Uses arrays of bits (0s and 1s). Very compact and fast for logical operations (AND/OR).
  *   **Functional Index (Slide 49):** An index created on the result of a function (e.g., `Index on (Salary * Commission)`), rather than a raw column value.