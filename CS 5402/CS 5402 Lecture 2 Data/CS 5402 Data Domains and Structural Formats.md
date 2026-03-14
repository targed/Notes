## 1. Record Data (The Foundation)
Most algorithms are designed to ingest **Record Data**.
*   **The Table:** The standard spreadsheet format. Each record (row) has a fixed set of attributes (columns).
*   **The Data Matrix:** If *all* attributes are numeric, the dataset is treated as an $m \times n$ matrix. 
  *   *Algorithmic Intuition:* We view each row as a point in $n$-dimensional space. This is critical for clustering (finding points near each other) and classification (drawing a boundary between groups of points).
- ## 2. Special Types of Data (The "Tricky" Ones)
- ### A. Transaction Data
  *   **Definition:** Each record involves a "set" of items (e.g., a grocery receipt). 
  *   **Difference:** Unlike record data, the number of items can vary per row.
  *   **The Transformation (The "Market Basket Matrix"):** To run algorithms on this, we often convert it into a **Binary Record Matrix**. 
    *   *Columns:* Every possible item in the store.
    *   *Rows:* Each transaction.
    *   *Values:* 1 if the item was bought, 0 if not. This creates a very **Sparse Matrix** (mostly zeros).
- ### B. Text Data
  *   **The Vector Space Model:** To mine text, we treat each document as a **'term' vector**. 
  *   **Document-Term Matrix:** 
    *   *Columns:* Every unique word in the entire collection (the vocabulary).
    *   *Values:* The frequency of that word in the document.
    *   *Fill-in Knowledge:* This is often called the **Bag of Words** model because it ignores the *order* of words and only looks at their count.
- ### C. Graph and Network Data
  *   **Definition:** Used when the **relationships (edges)** are as important as the objects (nodes).
  *   **Algorithmic View:** We represent these as **Adjacency Matrices**.
  *   **Examples:** Social networks (friendships), the World Wide Web (hyperlinks), or Molecular Structures (chemical bonds).
- ### D. Ordered Data
  In most record data, the order of the rows doesn't matter. In these types, **order is information**:
  *   **Sequential Data:** An ordered list of items (e.g., DNA sequences: A-C-G-T).
  *   **Time Series Data:** Measurements taken over time. The "gap" between points (e.g., 1 hour) is constant and meaningful.
  *   **Spatial Data:** Objects have positions in space (GPS, Maps). Here, "order" refers to physical proximity.
  
  ---
- ## 3. Data by Structure (The 80/20 Rule)
  **Slide Context:** Categorize data by its "rigidity."
- ### A. Structured Data
  *   **Format:** Relational databases, CSVs.
  *   **Machine-Friendly:** Algorithms can "read" this immediately.
- ### B. Semi-Structured Data
  *   **Format:** **JSON** and **XML**.
  *   **Characteristics:** Use tags to separate data. It has a hierarchy (nested objects) but doesn't require a rigid table.
  *   **Usage:** This is the standard for **Web APIs** (the way data is sent from a server to an app).
- ### C. Unstructured Data
  *   **The "Wild West":** Images, Video, Audio, and Raw Text.
  *   **The "80% Rule":** It is estimated that 80% of all enterprise data is unstructured.
  *   **The Mining Challenge:** You cannot run k-means on a raw ".jpg" file. You must first use **Feature Extraction** (like a Convolutional Neural Network) to turn the image into a numeric vector (Structured Data).
  
  ***
- # Problem Solving & Concept Check
  
  **1. Dimensionality and Sparsity:**
  Imagine a store with 10,000 different products. You have 1,000 customer transactions.
  *   If you represent this as a **Binary Record Matrix**, how many columns will the matrix have? (Answer: 10,000).
  *   If an average customer only buys 5 items, what percentage of the matrix cells will be "0"? (Answer: 99.95%). 
  *   *Note:* This is a **Sparse Matrix**, and algorithms must be optimized to handle all those zeros efficiently.
  
  **2. Identifying Structure (Slide 39):**
  Match the structure to the example:
  *   A. A **JSON** file from the Twitter API: __________ (Semi-structured).
  *   B. An **Excel** sheet of student grades: __________ (Structured).
  *   C. A **Voice Memo** recording: __________ (Unstructured).
  
  **3. Algorithmic Choice:**
  You are analyzing a protein sequence (A-R-N-D...). Is this **Record Data** or **Sequential Data**? Does the position of "A" relative to "R" matter? (Answer: Sequential; yes, position is vital for biological function).