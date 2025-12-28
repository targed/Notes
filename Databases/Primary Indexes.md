### **Part 1: Introduction to Indexing**

**1. What is an Index?**
*   An index is an **auxiliary access structure**. It is a separate file stored on disk that speeds up record retrieval.
*   **Analogy:** It works exactly like the index at the back of a textbook. Instead of reading every page to find a keyword, you look in the index, find the page number, and jump directly there.
*   **Trade-offs:**
  *   **Pros:** drastically speeds up reading/searching.
  *   **Cons:** Uses extra disk space; slows down writing (Insertion/Deletion) because the index must be updated every time the data changes.

**2. Structure of an Index Entry**
*   A single-level ordered index file contains records with just two fields:
  *   **Indexing Field:** The value you are searching for (e.g., Student ID).
  *   **Pointer:** The physical address (disk block address) where the actual data record is stored.
*   **Efficiency:** Because the index file only holds these two small fields, it is much smaller than the actual data file and can be searched much faster (often using Binary Search).

---
- ### **Part 2: Primary Indexes**
  
  There are three main types of single-level indexes: **Primary**, **Clustering**, and **Secondary**. The distinction depends on whether the data file is sorted physically and whether the search key is unique.
  
  **1. Definition of a Primary Index**
  *   A Primary Index is created on a data file that is **physically ordered (sorted)** by a specific field.
  *   **Constraint:** The ordering field must be a **Key** (every value is unique).
  *   **Structure:** `<Primary Key Value, Disk Block Pointer>`.
  
  **2. Sparse (Non-Dense) Nature**
  *   A Primary Index is a **Sparse Index**. This is a critical concept.
  *   It does **NOT** contain an index entry for every single record in the database.
  *   Instead, it maintains **one index entry per Disk Block**.
  *   **Block Anchor:** The index stores the key value of the **first record** in each block (called the Anchor Record).
    *   *Why?* Since the file is sorted, if you know the first record in Block 1 is "ID 10" and the first record in Block 2 is "ID 20", you know that "ID 15" *must* be in Block 1. You don't need a specific pointer for "ID 15".
  
  **3. How Searching Works**
  To find a record with value $K$:
  1.  Perform a **Binary Search** on the Index file to find the appropriate entry $i$ where $K(i) \le K < K(i+1)$.
  2.  Follow the pointer $P(i)$ to the data block.
  3.  Load that block into memory and search linearly within that specific block to find the record.
  
  **4. Problems with Primary Indexes**
  *   **Insertion/Deletion:** Because the underlying data file is physically sorted, inserting a new record usually requires shifting other records to make room.
  *   If records move to different blocks, the **Anchor Records** change, forcing updates to the Index file as well.
  *   *Solution:* Databases often use linked lists of overflow blocks or deletion markers to handle this without constantly reorganizing the file.