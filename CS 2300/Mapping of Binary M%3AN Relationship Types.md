### **ER-to-Relational Mapping Algorithm (Continued)**
- #### **Step 6: Mapping of Binary M:N Relationship Types**
  
  For a many-to-many (M:N) relationship, we cannot use the foreign key approach by simply adding a column to one of the entity tables. If we tried to add the primary key of `PROJECT` to the `EMPLOYEE` table, an employee working on multiple projects would require multiple `Pnumber` values in a single cell, violating the atomicity rule. The same problem occurs if we try to add `Essn` to the `PROJECT` table.
  
  Therefore, M:N relationships require the creation of a new, separate table.
  
  *   **Rule (Relationship Relation / Cross-Reference Approach):**
    1.  Create a **new relation** (often called a junction table or cross-reference table) to represent the M:N relationship itself.
    2.  **Foreign Keys:** This new relation's attributes will be the **primary keys** of *both* participating entity types. These attributes act as **foreign keys**, referencing the original entity relations.
    3.  **Primary Key:** The **primary key** of this new relationship relation is typically the **combination of both foreign keys**.
    4.  **Relationship Attributes:** If the M:N relationship has any of its own attributes, they are also included as columns in this new relation.
  
  This same approach is also used for mapping **N-ary relationships** (relationships with a degree higher than two). A new relation is created, and its attributes are the primary keys of all participating entity types.
  
  **Example:**
  
  Mapping the `WORKS_ON` M:N relationship between `EMPLOYEE` and `PROJECT`:
  *   **Participating Entities:** `EMPLOYEE`, `PROJECT`
  *   **Primary Key of `EMPLOYEE`:** `Ssn`
  *   **Primary Key of `PROJECT`:** `Pnumber`
  *   **Relationship Attribute:** `Hours`
  
  Following the rule:
  1.  Create a new relation: `WORKS_ON`.
  2.  Add the primary key of `EMPLOYEE` as a foreign key. Let's call it `Essn`.
  3.  Add the primary key of `PROJECT` as a foreign key. Let's call it `Pno`.
  4.  Add the relationship's attribute, `Hours`.
  5.  Define the primary key of the `WORKS_ON` relation as the combination of the foreign keys: **(`Essn`, `Pno`)**.
  
  The final schema for this new relationship relation is:
  *   **WORKS_ON**(<u>Essn</u>, <u>Pno</u>, Hours)
  
  Each tuple in this table represents a single link: one specific employee works on one specific project for a certain number of hours. This structure correctly models the M:N relationship without violating atomicity.