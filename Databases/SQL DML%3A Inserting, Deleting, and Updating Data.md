### **SQL DML: Inserting, Deleting, and Updating Data**

There are three main DML commands for modifying the data in a database: `INSERT`, `DELETE`, and `UPDATE`.
- #### **1. The `INSERT` Statement**
  
  The `INSERT` statement is used to add a new tuple (row) to a table.
  
  *   **Syntax 1: Inserting a complete row**
    *   If you are providing values for **all** columns in the exact order they were defined in the `CREATE TABLE` statement, you don't need to specify the column names.
    *   **Example:**
        ```sql
        INSERT INTO Employee
        VALUES (122, 'Bob', 'M', 12, 110000);
        ```
  
  *   **Syntax 2: Inserting a partial row**
    *   This is the more common and recommended approach. You explicitly list the columns you are providing data for. Any columns not listed will be assigned their `DEFAULT` value, or `NULL` if no default is specified and the column allows nulls.
    *   **Example:**
        ```sql
        INSERT INTO Employee (id, deptnumber)
        VALUES (123, 13);
        ```
  
  *   **Syntax 3: Inserting from a query result**
    *   You can use a `SELECT` statement to retrieve data from other tables and insert the results directly into the target table. The columns and data types from the `SELECT` list must match the columns specified in the `INSERT` list.
    *   **Example:**
        ```sql
        INSERT INTO Employee (id, fname)
        SELECT student_id, student_name FROM OldStudentRecords WHERE status = 'hired';
        ```
- #### **2. The `DELETE` Statement**
  
  The `DELETE` statement removes one or more tuples from a table.
  
  *   **General Syntax:**
    ```sql
    DELETE FROM TableName
    WHERE Condition;
    ```
  *   **The `WHERE` Clause:** This is the most critical part. It specifies a condition to identify which rows should be deleted.
    *   **If you omit the `WHERE` clause, all rows in the table will be deleted!** (Slide 43)
  *   The condition can be a simple comparison or a complex condition involving a subquery.
  
  *   **Examples:**
    *   **Delete a single record:**
        ```sql
        DELETE FROM Employee WHERE id = 121;
        ```
    *   **Delete multiple records:**
        ```sql
        DELETE FROM Employee WHERE Salary > 100000;
        ```
    *   **Delete using a subquery:** Delete all employees who work in the 'Research' department.
        ```sql
        DELETE FROM Employee
        WHERE Deptnumber IN (SELECT Dnumber FROM Department WHERE Dname = 'Research');
        ```
- #### **3. The `UPDATE` Statement**
  
  The `UPDATE` statement is used to modify the attribute values of one or more existing tuples.
  
  *   **General Syntax:**
    ```sql
    UPDATE TableName
    SET Column1 = value1, Column2 = value2, ...
    WHERE Condition;
    ```
  *   **Components:**
    *   `SET` clause: Specifies which columns to change and their new values. The new value can be a constant or an expression (e.g., `Salary = Salary * 1.1`).
    *   `WHERE` clause: Selects which rows to update. Just as with `DELETE`, if you omit the `WHERE` clause, **all rows in the table will be updated!**
  
  *   **Examples:**
    *   **Update a specific set of records:** Give a 10% raise to all employees in the 'Research' department.
        ```sql
        UPDATE Employee
        SET Salary = Salary * 1.1
        WHERE Deptnumber IN (SELECT Dnumber FROM Department WHERE Dname = 'Research');
        ```
    *   **Update a single record:** Change the location of project 10.
        ```sql
        UPDATE PROJECT
        SET PLOCATION = 'Bellaire', DNUM = 5
        WHERE PNUMBER = 10;
        ```