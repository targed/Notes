- **1. The Four Types of Joins**
  *   **`(INNER) JOIN`**: The standard join. It returns **only** rows where there is a match in **both** tables. If a row in Table A has no match in Table B, it is discarded.
  *   **`LEFT (OUTER) JOIN`**: Returns **all** rows from the **Left** table, and matched rows from the Right table. If there is no match, the Right side columns are filled with `NULL`.
  *   **`RIGHT (OUTER) JOIN`**: Returns **all** rows from the **Right** table, and matched rows from the Left table. (Reverse of Left Join).
  *   **`FULL (OUTER) JOIN`**: Returns **all** rows from **both** tables. If there is a match, they are combined. If not, the missing side is filled with `NULL`.
  
  **2. Syntax**
  ```sql
  SELECT attributes
  FROM TableA
  JOIN TableB ON TableA.Column = TableB.Column
  WHERE condition;
  ```
  *   **`ON`**: Specifies how the tables are linked.
  *   **`WHERE`**: Specifies filters on the data *after* the link is established.
  
  ---
- ### ** Advanced SQL Constructs (CASE and Assertions)**
  
  **1. The `CASE` Expression**
  This allows you to use **If-Then-Else** logic directly inside a query. It is useful for customizing output or performing conditional updates.
  
  *   **Usage:** Can be used in `SELECT`, `UPDATE`, or anywhere a value is expected.
  *   **Example (Conditional Update):** Giving different raises based on the department.
    ```sql
    UPDATE Employee
    SET Salary = CASE
        WHEN Dno = 5 THEN Salary + 2000
        WHEN Dno = 4 THEN Salary + 1500
        ELSE Salary + 1000
    END;
    ```
  
  **2. General Constraints with `ASSERTION`**
  Standard constraints like `NOT NULL` or `CHECK` usually apply to a single row or a single table. **Assertions** allow you to define constraints that must hold true for the **entire database** (involving multiple tables or complex logic).
  
  *   **Concept:** `CREATE ASSERTION Name CHECK (Condition)`
  *   **Rule:** The condition inside the assertion must **always** evaluate to `TRUE`. If any transaction tries to make it False, the transaction fails.
  *   **Example:** "An employee's salary must not be greater than their manager's salary." (This involves joining `Employee` and `Department` and `Employee` again, which is too complex for a standard table constraint).
  *   *Note:* Assertions are part of the SQL standard but are computationally expensive to check, so not all DBMSs support them fully.
  
  ---
- ### **Part: Views**
  
  A **View** is a **Virtual Table**. It looks like a table and acts like a table, but it doesn't necessarily store data itself. It is defined by a query.
  
  **1. Creating and Using Views**
  *   **Syntax:** `CREATE VIEW ViewName AS SELECT ...`
  *   **Purpose:**
    *   **Simplification:** Hides complex `JOIN`s or logic behind a simple table name.
    *   **Security:** Gives users access to specific data (e.g., Employee Name) without giving access to the underlying sensitive table (e.g., Salary).
  *   **Usage:** You can query a view exactly like a regular table: `SELECT * FROM ViewName`.
  
  **2. Implementation Strategies (How the DBMS handles Views)**
  How does the database give you data from a table that doesn't exist? There are two main strategies:
  
  *   **Strategy A: Query Modification**
    *   **How it works:** When you query the view, the DBMS intercepts your query, "pastes" the view's definition into your query, and executes the combined query against the base tables.
    *   **Pros:** Always up-to-date; saves storage space.
    *   **Cons:** Inefficient if the view definition is very complex (e.g., massive joins), as the join must be re-calculated every single time you look at the view.
  
  *   **Strategy B: View Materialization**
    *   **How it works:** The DBMS actually runs the view query once and **stores the physical result** as a real table on the disk.
    *   **Pros:** Extremely fast to query (the work is already done).
    *   **Cons:** The "Materialized View" can become **stale** (out of date) if the base tables change.
  
  **3. Updating Materialized Views**
  If using materialization, the DBMS must decide when to refresh the data:
  *   **Immediate Update:** Update the view immediately whenever the base table changes. (Good for data freshness, slows down updates).
  *   **Lazy Update:** Update the view only when someone tries to query it.
  *   **Periodic Update:** Update the view on a schedule (e.g., every night). Common in Data Warehousing or Banks where slight delays in data are acceptable.