### **Second Normal Form (2NF)**

**1. The Goal**
2NF is strictly concerned with removing **Partial Functional Dependencies**.

**2. The Rule**
A relation is in 2NF if:
1.  It is already in **1NF**.
2.  Every **non-prime attribute** is **fully functionally dependent** on the Primary Key.

**3. The "Shortcut"**
*   Partial dependencies can **only** exist if the Primary Key is a **Composite Key** (made of multiple columns).
*   **Therefore:** If a table has a **Single-Attribute Primary Key**, it is **automatically in 2NF**.

**4. The General Definition**
While the simple definition focuses on the Primary Key, the rigorous "General Definition" considers **all** Candidate Keys:
*   A relation is in 2NF if no non-prime attribute is partially dependent on **any** candidate key.

**5. Example of a Violation**
*   Table: `EMP_PROJ(Ssn, Pnumber, Hours, Ename, Pname, Plocation)`
*   Key: `{Ssn, Pnumber}`
*   Violations:
  *   `Ssn` determines `Ename` (Partial dependency).
  *   `Pnumber` determines `Pname` and `Plocation` (Partial dependency).
*   **Fix:** Break the table into three: `EMPLOYEE(Ssn, Ename)`, `PROJECT(Pnumber, Pname, Plocation)`, and `WORKS_ON(Ssn, Pnumber, Hours)`.

---
- ### **Third Normal Form (3NF)**
  
  **1. The Goal**
  3NF is concerned with removing **Transitive Dependencies**.
  
  **2. The Rule**
  A relation is in 3NF if:
  1.  It is already in **2NF**.
  2.  No **non-prime attribute** is transitively dependent on the Primary Key.
    *   *Translation:* Non-key columns should not depend on other non-key columns.
  
  **3. The General Definition - *Memorize This***
  A relation is in 3NF if, for **every** valid functional dependency $X \rightarrow Y$, one of the following is true:
  1.  **$X$ is a Superkey** (The left side is a unique identifier).
  2.  **$Y$ is a Prime Attribute** (The right side is part of a Candidate Key).
  
  **4. Example of a Violation**
  *   Table: `EMP_DEPT(Ssn, Ename, Dnumber, Dname, Dmgr_ssn)`
  *   Key: `{Ssn}`
  *   Dependency: `Ssn` $\rightarrow$ `Dnumber` $\rightarrow$ `Dname`.
  *   Violation: `Dname` depends on `Dnumber`. Neither is a key.
  *   **Fix:** Move the department details (`Dnumber`, `Dname`, `Dmgr_ssn`) to a separate `DEPARTMENT` table.
  
  ---
- ### **Boyce-Codd Normal Form (BCNF)**
  
  **1. The Goal**
  BCNF is a "stricter" version of 3NF. It handles rare edge cases that 3NF misses, specifically when a Prime Attribute depends on a Non-Prime Attribute.
  
  **2. The Rule**
  A relation is in BCNF if, for **every** non-trivial functional dependency $X \rightarrow Y$:
  *   **$X$ must be a Superkey.**
  
  **3. Comparison to 3NF**
  *   3NF allowed a "loophole": if $Y$ is a prime attribute, the FD was okay.
  *   BCNF closes this loophole. If $X$ isn't a Superkey, the dependency is illegal.
  
  **4. The "Student-Course-Instructor" Example (Slides 78–82)**
  *   Attributes: `{StudentID, Course, Instructor}`
  *   Rules:
    1.  One student can take many courses.
    2.  Each instructor teaches only one course ($Instructor \rightarrow Course$).
    3.  A student has one instructor per course.
  *   Key: `{StudentID, Course}`.
  *   **The Problem:** We have the FD `Instructor` $\rightarrow$ `Course`.
    *   Is `Instructor` a Superkey? **No** (An instructor teaches many students).
    *   Is `Course` a prime attribute? **Yes** (It's part of the Key `{StudentID, Course}`).
  *   **Result:** This table is in **3NF** (because of the loophole), but **NOT in BCNF** (because `Instructor` is not a superkey).
  
  **5. Decomposition Algorithm**
  To fix a BCNF violation ($X \rightarrow A$):
  1.  Create a new table with the violating FD attributes: $(X, A)$.
  2.  Remove attribute $A$ from the original table.
  3.  Keep repeating until all tables satisfy BCNF.
  
  **6. The Non-Additive (Lossless) Join Property**
  When decomposing tables, it is critical that we don't create "Spurious Tuples" (garbage data) when joining them back.
  *   **The Test:** A decomposition of $R$ into $R1$ and $R2$ is valid/lossless if the **intersection** of attributes ($R1 \cap R2$) is a **Superkey** for either $R1$ or $R2$.
  
  ---
- ### **Higher Normal Forms (4NF, 5NF, DKNF)**
  
  These forms are rarely used in practice but address complex scenarios.
  
  **1. Fourth Normal Form (4NF) (Slide 87)**
  *   **Target:** **Multivalued Dependencies (MVD)**.
  *   **Scenario:** When a table contains two or more *independent* multivalued facts about an entity.
  *   *Example:* `Employee` table stores `Projects` and `Dependents`. If an employee has 3 projects and 2 dependents, you are forced to create $3 \times 2 = 6$ rows to list them all, creating massive redundancy.
  *   **Fix:** Separate them into two tables: `Employee_Projects` and `Employee_Dependents`.
  
  **2. Fifth Normal Form (5NF) (Slides 88–89)**
  *   **Target:** **Join Dependencies**.
  *   **Scenario:** Cases where a relationship is ternary (3-way) and cannot be decomposed into binary (2-way) tables without losing information, yet redundancy exists.
  
  **3. Domain-Key Normal Form (DKNF) (Slide 90)**
  *   **Target:** The "Ultimate" normal form.
  *   **Rule:** A relation is in DKNF if every constraint is a logical consequence of the definition of **Keys** and **Domains**. It is very hard to achieve in practice.