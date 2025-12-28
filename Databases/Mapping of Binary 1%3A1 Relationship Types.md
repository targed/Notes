### **ER-to-Relational Mapping Algorithm (Continued)**
- #### **Step 4: Mapping of Binary 1:1 Relationship Types**
  
  For a 1:1 relationship between two entity types, say `S` and `T`, there are three potential ways to map it to a relational schema:
  
  1.  **Foreign Key Approach (Most Common):**
    *   **Rule:** Choose **one** of the relations (let's say `S`) and add the primary key of the other relation (`T`) to it as a **foreign key**.
    *   **Which one to choose?** It is generally better to choose the relation that has **total participation** in the relationship. By placing the foreign key in the table corresponding to the total participation side, you can enforce a `NOT NULL` constraint on the foreign key column, effectively minimizing null values. If both sides have total participation, the choice is arbitrary. If both have partial participation, you will inevitably have null values in the foreign key column, so the choice is also arbitrary.
    *   To enforce the "1" side of the cardinality (ensuring the foreign key value is unique), a **uniqueness constraint** should be placed on the foreign key column in relation `S`.
  
  2.  **Merged Relation Approach:**
    *   If both entities in the 1:1 relationship have **total participation**, it means each entity in `S` is related to exactly one entity in `T`, and vice versa. In this specific case, it might be sensible to merge the two entity types into a single relation. This is less common as it reduces the clarity of the design.
  
  3.  **Relationship Relation Approach:**
    *   Create a separate table just for the relationship. This is the same approach used for M:N relationships (see Step 6) and is generally overkill for a 1:1 relationship, as it creates an extra table.
  
  The lecture focuses on the **Foreign Key Approach**, which is the most practical and widely used method.
  
  **Example:**
  
  Mapping the `MANAGES` 1:1 relationship between `EMPLOYEE` and `DEPARTMENT`:
  *   **Entities:** `EMPLOYEE`, `DEPARTMENT`
  *   **Relationship:** `MANAGES` (1:1)
  *   **Participation:** An `EMPLOYEE` has partial participation (not every employee is a manager). A `DEPARTMENT` has total participation (every department must have a manager).
  
  Following the Foreign Key Approach:
  1.  We have two relations already: `EMPLOYEE` and `DEPARTMENT`.
  2.  We need to add a foreign key to one of them. We should choose the relation on the **total participation** side, which is `DEPARTMENT`.
  3.  We add the primary key of `EMPLOYEE` (`Ssn`) to the `DEPARTMENT` relation. Let's call this new column `Mgr_ssn`.
  4.  We also often include attributes of the relationship itself. The `MANAGES` relationship has an attribute `Mgr_start_date`, so we add that to the `DEPARTMENT` table as well.
  5.  `Mgr_ssn` in `DEPARTMENT` is a foreign key that references `Ssn` in `EMPLOYE`.
  6.  A uniqueness constraint must be placed on `Mgr_ssn` in the `DEPARTMENT` table to ensure that no two departments are managed by the same employee, thus enforcing the 1:1 cardinality.
  
  The updated `DEPARTMENT` relation schema becomes:
  *   **DEPARTMENT**(Dname, <u>Dnumber</u>, Mgr_ssn, Mgr_start_date)