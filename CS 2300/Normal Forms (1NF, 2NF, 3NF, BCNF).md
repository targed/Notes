### **Types of Functional Dependencies**

Normalization is essentially the process of systematically removing "bad" dependencies. Here are the three main types you need to identify:

**1. Full Functional Dependency**
This is the "Good" kind.
*   **Definition:** In an FD $X \rightarrow Y$, if you remove *any* attribute from $X$, the dependency no longer holds. $Y$ depends on the **entirety** of $X$, not just a piece of it.
*   *Example:* In `{OrderID, ProductID} \rightarrow OrderedQuantity`, you need *both* IDs to know the quantity. You can't know the quantity from just the `OrderID`. This is a Full Dependency.

**2. Partial Functional Dependency**
This is the "Bad" kind (Violates 2NF).
*   **Definition:** In an FD $X \rightarrow Y$, there exists a **subset** of $X$ that can determine $Y$ all by itself.
*   **Context:** This **only** happens when the Primary Key is **Composite** (made of multiple columns).
*   *Example:* Key is `{OrderID, ProductID}`.
  *   FD: `{OrderID, ProductID} \rightarrow OrderDate`.
  *   *Problem:* The `OrderDate` is determined solely by the `OrderID`. The `ProductID` is irrelevant.
  *   *Result:* This is a Partial Dependency.

**3. Transitive Functional Dependency**
This is the "Bad" kind (Violates 3NF).
*   **Definition:** A dependency chain $X \rightarrow Z \rightarrow Y$.
*   **Context:** A non-key attribute determines another non-key attribute.
*   *Example:* Key is `OrderID`.
  *   FD: `OrderID` $\rightarrow$ `CustomerID` $\rightarrow$ `CustomerAddress`.
  *   *Problem:* `OrderID` determines the Customer, and the Customer determines the Address. The Address is only related to the Order *through* the Customer.

---
- ### **Normalization Process and 1NF**
  
  **1. The Goal of Normalization**
  We decompose large tables into smaller ones to:
  1.  Minimize redundancy.
  2.  Eliminate Update Anomalies (Insertion, Deletion, Modification).
  3.  Ensure stability and scalability.
  
  **2. Denormalization**
  *   The process of deliberately introducing redundancy (joining tables back together) to improve read performance for **reporting** purposes. It trades write-integrity for read-speed.
  
  **3. Key Definitions Review - *Crucial for Exam***
  *   **Prime Attribute:** An attribute that is part of **any** Candidate Key.
  *   **Non-Prime Attribute:** An attribute that is not part of any Candidate Key.
  
  **4. First Normal Form (1NF)**
  *   **The Rule:** The "Atomicity" rule.
    *   No **repeating groups** (lists or arrays) in a single cell.
    *   No nested tables.
    *   Every cell must contain exactly **one** value.
  *   **The Fix:** If you have a list of values (e.g., `Locations: {Houston, Stafford}`), you must either:
    *   Create a separate row for each location (expands the table).
    *   Move the list to a new table (better design).