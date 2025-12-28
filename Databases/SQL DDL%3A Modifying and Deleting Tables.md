### **SQL DDL: Modifying and Deleting Tables**

Once a database schema is created, it is not set in stone. Over time, application requirements change, and the database schema must evolve. SQL provides commands to alter and drop tables to accommodate this.
- #### **The `DROP TABLE` Command**
  
  The `DROP` command is a DDL command used to completely remove a table from the database.
  
  *   **Action:** `DROP TABLE` removes **everything** related to the table: all the data records within it and the table's definition from the schema catalog. This action is permanent and generally cannot be undone.
  
  *   **Syntax and Options:**
    ```sql
    DROP TABLE Table_name [RESTRICT | CASCADE];
    ```
    *   **`RESTRICT` (Default in many systems):** This is the "safe" option. The command will only succeed if there are no other database objects (like foreign key constraints from other tables or views) that refer to this table. If there are dependencies, the command will fail.
    *   **`CASCADE`:** This is the "powerful" option. It drops the table and **automatically drops all dependent objects** as well. For example, if you `DROP TABLE DEPARTMENT CASCADE`, the command will also remove the foreign key constraint in the `EMPLOYEE` table that refers to it. This is useful but must be used with extreme caution.
- #### **The `ALTER TABLE` Command**
  
  The `ALTER TABLE` command is used to change the structure of an existing table. This allows for schema evolution without needing to drop and recreate the table (which would lose all the data).
  
  *   **Common Alteration Options:**
    *   **Add a Column:** Adds a new attribute to the table. Existing rows will have a `NULL` or `DEFAULT` value for this new column.
        ```sql
        ALTER TABLE Employee ADD COLUMN mname VARCHAR(20);
        ```
    *   **Drop a Column:** Removes a column and all its data from the table. This also requires a `RESTRICT` or `CASCADE` option to handle dependencies.
        ```sql
        ALTER TABLE Employee DROP COLUMN Address CASCADE;
        ```
    *   **Change a Column's Definition:** You can modify a column's data type, size, or default value.
        
  ```sql
        -- Change data type
        ALTER TABLE Employee MODIFY mname VARCHAR(10);
  
        -- Add or remove a default value
        ALTER TABLE DEPARTMENT ALTER COLUMN Mgr_ssn SET DEFAULT '333445555';
        ALTER TABLE DEPARTMENT ALTER COLUMN Mgr_ssn DROP DEFAULT;
        ```
    *   **Add or Drop Constraints:** You can add or remove constraints like `CHECK`, `FOREIGN KEY`, or `UNIQUE` after a table has been created. This is particularly useful when you need to load data first and then apply the rules.
        
  ```sql
        -- Add a check constraint
        ALTER TABLE Employee ADD CHECK(age > 0);
  
        -- Add a foreign key constraint (self-referencing)
        ALTER TABLE EMPLOYEE ADD FOREIGN KEY (Super_ssn) REFERENCES EMPLOYEE(Ssn);
        ```
  
  Giving constraints explicit names using the `CONSTRAINT` keyword is very helpful here, as it allows you to easily `DROP` a specific constraint later by name: `ALTER TABLE Employee DROP CONSTRAINT Emp_Primarykey;`