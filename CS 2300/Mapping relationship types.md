### **Step 4: Mapping of Binary 1:1 Relationship Types**

Mapping a 1:1 relationship gives you a few options, but one is generally preferred.

*   **The Rule:** For a 1:1 relationship between two entity types `S` and `T`, you can use the **Foreign Key Approach**. Add the primary key of `T` as a foreign key in `S`. It is also possible to add the primary key of `S` as a foreign key in `T`.
*   **Which side to choose?** The best choice is often to add the foreign key to the relation that has **total participation** in the relationship. This avoids having `NULL` values in the foreign key column. If both sides have total participation, you could even merge them into a single table (the Merged Relation Approach), but this is less common.
*   **Relationship Attributes:** Any attributes of the relationship are added to the table that receives the foreign key.
*   **Example `MANAGES` relationship:**
  *   This is a 1:1 relationship between `EMPLOYEE` and `DEPARTMENT`.
  *   An employee can manage one department, and a department is managed by one employee.
  *   The `DEPARTMENT` side has total participation (every department must have a manager). The `EMPLOYEE` side has partial participation (not every employee is a manager).
  *   Therefore, the best approach is to add the primary key of `EMPLOYEE` (`Ssn`) as a foreign key in the `DEPARTMENT` relation. We'll call this new attribute `Mgr_ssn`.
  *   The relationship attribute `Mgr_start_date` is also added to the `DEPARTMENT` relation.
- ### **Step 5: Mapping of Binary 1:N Relationship Types**
  
  This is the most common type of relationship, and the mapping is very straightforward.
  
  *   **The Rule:** For a 1:N relationship, always identify the relation on the **"N" side**. Add the primary key of the "1" side entity as a **foreign key** in the "N" side's relation.
  *   **Why this way?** The table on the "N" side is where the foreign key belongs because each "N" entity is related to exactly one "1" entity. Placing the foreign key here perfectly represents this link without redundancy.
  *   **Relationship Attributes:** Any attributes of the relationship are also added to the relation on the "N" side.
  *   **Example (`WORKS_FOR` relationship):**
    *   This is a 1:N relationship between `DEPARTMENT` (1 side) and `EMPLOYEE` (N side).
    *   We go to the "N" side relation, which is `EMPLOYEE`.
    *   We add the primary key of the `DEPARTMENT` relation (`Dnumber`) as a foreign key. We'll name this attribute `Dno` in the `EMPLOYEE` table.
- ### **Step 6: Mapping of Binary M:N Relationship Types**
  
  You cannot use the foreign key approach for an M:N relationship because it would lead to redundancy and violate the atomic value rule. Therefore, a new table must be created.
  
  *   **The Rule:** For each M:N relationship, create a **new relation** (often called an *associative* or *join* table).
  *   **Attributes:** This new relation must include:
    1.  The primary key of the first participating entity as a foreign key.
    2.  The primary key of the second participating entity as a foreign key.
  *   **Relationship Attributes:** Any attributes of the M:N relationship become attributes in this new relation.
  *   **Primary Key:** The primary key of this new relation is typically the **combination of both foreign keys**.
  *   **Example (`WORKS_ON` relationship):**
    *   This is an M:N relationship between `EMPLOYEE` and `PROJECT`.
    *   We create a new relation called `WORKS_ON`.
    *   We add the primary key of `EMPLOYEE` (`Ssn`) as a foreign key, which we name `Essn`.
    *   We add the primary key of `PROJECT` (`Pnumber`) as a foreign key, which we name `Pno`.
    *   We add the relationship attribute `Hours`.
    *   The primary key for the `WORKS_ON` relation is the composite key `(Essn, Pno)`.