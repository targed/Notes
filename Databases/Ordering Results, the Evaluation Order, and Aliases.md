- **1. Ordering Query Results**
  SQL tables are sets, meaning they have no inherent order. To sort the output, you must use the `ORDER BY` clause.
  *   **Keywords:**
    *   `ASC`: Ascending order (Default).
    *   `DESC`: Descending order.
  *   **Multiple Columns:** You can sort by primary and secondary attributes.
    *   **Example:** `ORDER BY Dname DESC, Lname ASC`
    *   This first sorts the Departments reverse-alphabetically. Within the same Department, it sorts Employees alphabetically by Last Name.
  
  **2. The Danger of Missing `WHERE`**
  If you select from multiple tables but forget the `WHERE` clause, SQL performs a **Cartesian Product (Cross Product)**.
  *   It pairs **every** row from the first table with **every** row from the second table.
  *   **Result:** Usually a massive amount of meaningless data.
  
  **3. Aliases and Disambiguation**
  *   **Ambiguity:** If two tables have columns with the same name (e.g., both `Student` and `Department` have a `Name` column), you must prefix the column with the table name (e.g., `Student.Name`).
  *   **Aliases (`AS` keyword):** You can give tables temporary, short names (tuple variables) to make the query readable or to enable **self-joins**.
    *   **Example:** `FROM EMPLOYEE AS E, EMPLOYEE AS S`
    *   This treats the single `EMPLOYEE` table as two separate instances (`E` for Employee, `S` for Supervisor), allowing you to compare rows within the same table.
    *   *Note:* The keyword `AS` is optional in many SQL implementations (e.g., `FROM EMPLOYEE E`).
  
  **4. Arithmetic Operations**
  You can perform calculations directly in the `SELECT` clause. The result is a "derived" attribute that exists only in the query output, not in the database.
  *   **Example:** `SELECT Salary * 1.1 AS NewSalary`
  *   This displays a column showing what a 10% raise would look like, without actually changing the data.
  
  **5. The "Conceptual" Evaluation Order – *Critical Concept***
  It is vital to distinguish between the order you **write** a query and the order the system **executes** it.
  
  *   **Written Order:**
    1.  `SELECT`
    2.  `FROM`
    3.  `WHERE`
  
  *   **Execution Order:**
    1.  **`FROM`**: First, identify the tables and compute the cross product.
    2.  **`WHERE`**: Second, filter the rows based on the condition.
    3.  **`SELECT`**: Third, select only the specific columns asked for.
    4.  **`DISTINCT`**: Finally, remove duplicates if requested.
  
  *   **Why does this matter?** You cannot use a column alias defined in `SELECT` inside the `WHERE` clause, because the `WHERE` clause runs *before* the `SELECT` clause creates that alias.