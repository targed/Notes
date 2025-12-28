### **SQL DDL: Specifying Constraints in `CREATE TABLE`**

Constraints are the rules that the database system enforces to ensure the data is always valid and consistent. Adding constraints is a trade-off: it ensures data integrity but can slightly slow down data insertion and updates because the DBMS must perform checks.
- #### **Attribute-Level Constraints**
  
  These are constraints that apply to individual columns.
  
  *   **`NOT NULL`:** This is the most common constraint. It ensures that a column cannot contain a `NULL` value. Every row in the table must have an actual value for this attribute. This is essential for primary keys but is also useful for other mandatory fields (e.g., a last name).
  
  *   **`DEFAULT`:** This clause provides a default value for a column if one is not explicitly specified during an `INSERT` operation. For example, `Gender CHAR(1) DEFAULT 'F'` would automatically assign 'F' to the `Gender` column if a new row is inserted without a value for `Gender`.
  
  *   **`CHECK` (Attribute-Based):** This constraint specifies a condition that the value of the attribute must satisfy. The condition is checked whenever a value in that column is inserted or updated.
    *   **Example:** `Dnumber INT CHECK (Dnumber > 0 AND Dnumber < 21)` ensures that the department number is always within a valid range. `Salary INTEGER CHECK(Salary >= 0)` prevents negative salaries.
- #### **Table-Level Constraints (Key and Referential Integrity)**
  
  These constraints often involve one or more columns and are typically defined at the end of the `CREATE TABLE` statement.
  
  **Key Constraints**
  
  *   **`UNIQUE`:** This constraint specifies that the values in a column (or a set of columns) must be unique across all rows in the table. `UNIQUE` defines a **candidate key**. A key difference from a primary key is that a `UNIQUE` column **can** allow `NULL` values (and often, it can allow multiple `NULL`s, as they are not considered equal to each other).
    *   **Syntax 1 (Column):** `ID INTEGER UNIQUE NOT NULL`
    *   **Syntax 2 (Table):** `UNIQUE(ID)`
    *   **Composite Unique Key:** `UNIQUE(bar, beer)` ensures that the *combination* of bar and beer is unique, which is different from making each column unique individually.
  
  *   **`PRIMARY KEY`:** This constraint uniquely identifies each record in a table. It has two fundamental properties enforced by the DBMS: the primary key column(s) must be both **`UNIQUE`** and **`NOT NULL`**. A table can have only **one** primary key.
    *   **Syntax 1 (Column, single attribute):** `ID INTEGER PRIMARY KEY`
    *   **Syntax 2 (Table, composite key):** `PRIMARY KEY(Fname, Lname)`
  
  **Referential Integrity Constraints**
  
  *   **`FOREIGN KEY`:** This constraint links two tables together. It ensures that a value in a column (or set of columns) in one table (the referencing table) matches a value in the `PRIMARY KEY` or `UNIQUE` key of another table (the referenced table).
    *   **Syntax:** `FOREIGN KEY (referencing_column) REFERENCES ReferencedTableName (referenced_column)`
    *   **Example:** In the `CREATE TABLE EMPLOYEE` statement, `FOREIGN KEY (Dno) REFERENCES DEPARTMENT(Dnumber)` ensures that every employee's `Dno` corresponds to an existing `Dnumber` in the `DEPARTMENT` table.
  
  **Handling Foreign Key Violations (Referential Triggered Actions)**
  
  When an action (like `DELETE` or `UPDATE`) on the referenced table would break the link, the `FOREIGN KEY` constraint can be configured with specific rules:
  *   **`ON DELETE ...` / `ON UPDATE ...`**
    *   **`RESTRICT` or `NO ACTION` (Default):** The `DELETE` or `UPDATE` operation on the parent table (e.g., `DEPARTMENT`) is rejected.
    *   **`CASCADE`:** The change is cascaded. If a department is deleted, all employees in that department are also automatically deleted.
    *   **`SET NULL`:** The foreign key column in the referencing table (e.g., `EMPLOYEE.Dno`) is set to `NULL`.
    *   **`SET DEFAULT`:** The foreign key is set to its predefined default value.
  
    The choice of which action to use is a critical design decision based on the business rules.