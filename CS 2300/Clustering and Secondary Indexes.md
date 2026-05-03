### **Part 3: Clustering Indexes**

A Clustering Index is very similar to a Primary Index, but with one major difference regarding uniqueness.

**1. Definition**
*   Created on a data file that is **physically ordered** by a specific field.
*   **Crucial Difference:** The ordering field is **NOT unique** (it is a non-key field). This field is called the **Clustering Field**.
*   *Example:* A file of `EMPLOYEE` records sorted by `DepartmentID`. Multiple employees belong to Department 5, so they are stored consecutively on the disk.

**2. Structure and Sparsity**
*   **Structure:** `<Clustering Field Value, Disk Block Pointer>`.
*   **Sparse Index:** It maintains **one entry per distinct value** of the clustering field (not one per record, nor strictly one per block).
*   **Pointer Behavior:** The pointer points to the **first disk block** that contains a record with that specific value. Because the file is sorted, all other records with that same value follow immediately after.

**3. Constraints**
*   Since a file can only be physically sorted in one way, a table can have **at most one** physical ordering index.
*   Therefore, a table can have a Primary Index **OR** a Clustering Index, but **never both** (Slide 23).

---
- ### **Part 4: Secondary Indexes**
  
  Secondary indexes are the most flexible type. They allow us to look up data based on fields that do *not* determine the physical order of the file.
  
  **1. Definition**
  *   An index created on a field that is **NOT** the ordering field of the data file.
  *   The data file can be ordered by something else (e.g., ID) or unordered (Heap).
  *   **Cardinality:** A table can have **many** secondary indexes (e.g., one for `Name`, one for `Email`, one for `ZipCode`).
  
  **2. Secondary Index on a Key Field (Unique Values)**
  *   *Example:* Indexing `SocialSecurityNumber` (Unique) when the file is sorted by `EmployeeID`.
  *   **Dense Index:** This is the critical difference from Primary/Clustering indexes. Because the underlying data is scattered (not sorted by SSN), the index **MUST** contain an entry for **every single record** in the data file. You cannot use Block Anchors.
  *   **Efficiency:** Searching the index is fast ($log_2 N$), but reading the records is slower because each record might be on a different disk block.
  
  **3. Secondary Index on a Non-Key Field (Duplicate Values)**
  *   *Example:* Indexing `JobTitle` (many people are "Engineers").
  *   **Structure:** The index entry for "Engineer" cannot point to just one record.
  *   **Solution:** The index entry points to a "bucket" or a block of pointers. That bucket contains the list of pointers to all the "Engineer" records.
  
  **4. Summary Comparison**
  *   **Primary Index:** Ordered File + Unique Key (Sparse).
  *   **Clustering Index:** Ordered File + Non-Unique Field (Sparse).
  *   **Secondary Index:** Unordered Field + Unique/Non-Unique (Dense).