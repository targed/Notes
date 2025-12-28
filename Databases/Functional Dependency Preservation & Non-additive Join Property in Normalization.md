### **Part 1: Functional Dependency Preservation**

**1. The Goal**
When we break a large table (relation) `R` into smaller tables ($R_1, R_2, ... R_n$), we want to ensure that all the original rules (Functional Dependencies) are still enforceable without having to join the tables back together.

**2. Definition**
A decomposition **preserves dependencies** if the union of all the FDs that hold in the individual smaller tables ($R_i$) is logically equivalent to the set of FDs in the original table ($R$).
*   **Formula:** $(F_1 \cup F_2 \cup ... \cup F_n)^+ = F^+$

**3. Why is this important?**
*   **Performance:** If dependencies are preserved, the DBMS can check constraints (like "Manager determines Department") efficiently within a single table during updates.
*   **Cost:** If dependencies are *not* preserved, the DBMS would have to join tables every time a user inserts or updates data to verify the constraint, which is extremely expensive.

**4. Examples**
*   **Preserved (Good):**
  *   Original: $R(A, B, C)$, $F = \{A \rightarrow B, B \rightarrow C\}$.
  *   Decomposed: $R_1(A, B)$ with $\{A \rightarrow B\}$, $R_2(B, C)$ with $\{B \rightarrow C\}$.
  *   Since both original rules are present in the new tables, FDs are preserved. This is typical of **3NF**.
*   **Not Preserved (Bad):**
  *   Original: $R(A, B, C)$, $F = \{AB \rightarrow C, C \rightarrow B\}$.
  *   Decomposed: $R_1(A, C)$, $R_2(C, B)$.
  *   The rule $C \rightarrow B$ is preserved in $R_2$.
  *   However, the rule $AB \rightarrow C$ is **lost** because $A$ and $B$ are now in different tables. We cannot check this rule without joining. This typically happens in **BCNF**.

---
- ### **Part 2: Non-Additive (Lossless) Join Property**
  
  **1. The Goal**
  When we decompose a table $R$ into $R_1$ and $R_2$, we must be able to rejoin them ($R_1 \bowtie R_2$) and get back **exactly** the original table $R$.
  *   We must not lose any data.
  *   We must not generate any fake/garbage data (**Spurious Tuples**).
  
  **2. Definition**
  A decomposition is **Lossless (Non-additive)** if the natural join of the decomposed tables produces the original relation.
  *   If the join produces *more* rows than the original (spurious tuples), the decomposition is **Lossy (Additive)** and is considered **invalid**.
  
  **3. The Test (Crucial for Exams)**
  To verify if a decomposition of $R$ into $R_1$ and $R_2$ is lossless, you check the **intersection** (common attributes).
  The intersection ($R_1 \cap R_2$) must be a **Superkey** for either $R_1$ or $R_2$.
  *   **Formal Condition:** Either $(R_1 \cap R_2) \rightarrow R_1$ OR $(R_1 \cap R_2) \rightarrow R_2$ must be a valid FD.
  
  ---
- ### **Part 3: Examples of Lossless vs. Lossy Joins**
  
  **1. Example: Lossless Join**
  *   **Original:** $R(A, B, C)$, $F = \{C \rightarrow B\}$.
  *   **Decomposition:** $R_1(A, C)$ and $R_2(C, B)$.
  *   **The Test:**
    *   **Intersection:** The common attribute is **C**.
    *   **Check:** Does $C$ determine $R_2 (C, B)$? Yes, because $C \rightarrow B$ is a given rule.
    *   **Result:** Since the common attribute ($C$) is a key for one of the tables ($R_2$), the join is **Lossless**.
    *   *Visual Proof:* The slide shows that joining the tables on $C$ perfectly reconstructs the original data.
  
  **2. Example: Lossy (Additive) Join**
  *   **Original:** $R(A, B, C)$, $F = \{C \rightarrow B\}$.
  *   **Decomposition:** $R_1(A, B)$ and $R_2(C, B)$.
  *   **The Test:**
    *   **Intersection:** The common attribute is **B**.
    *   **Check:** Does $B$ determine $R_1 (A, B)$? No, we have no rule $B \rightarrow A$.
    *   **Check:** Does $B$ determine $R_2 (C, B)$? No, we have no rule $B \rightarrow C$.
    *   **Result:** The common attribute ($B$) is **not a key** for either table. The join is **Lossy**.
    *   *Visual Proof:* The slide shows that joining on $B$ creates extra rows (spurious tuples) that did not exist in the original table (e.g., mixing $a1$ with $c3$ just because they share $b1$). The data is now corrupted.
  
  **Summary:**
  *   **FD Preservation** is desirable (for performance) but not always possible (especially in BCNF).
  *   **Lossless Join** is **mandatory**. A decomposition that is not lossless is incorrect.
  *   **The Golden Rule:** When splitting a table, the linking column(s) must be a primary key (or unique) in at least one of the new tables.