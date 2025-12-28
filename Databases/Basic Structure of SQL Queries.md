### **The Structure of a SQL Query**

**1. The Six Clauses of a Query**
A standard SQL retrieval query can consist of up to six specific clauses. The order in which you write them is fixed and mandatory:

1.  **`SELECT`**: Specifies which columns (attributes) you want to retrieve.
2.  **`FROM`**: Specifies which tables (relations) the data comes from.
3.  **`WHERE`**: Filters the rows based on a condition.
4.  **`GROUP BY`**: Groups rows that have the same values into summary rows.
5.  **`HAVING`**: Filters the *groups* (similar to how `WHERE` filters rows).
6.  **`ORDER BY`**: Sorts the final result.

*   **Crucial Note:** Only **`SELECT`** and **`FROM`** are mandatory. The others are optional.

**2. Tables as Sets vs. Bags**
This is a major distinction between Relational Algebra (theory) and SQL (practice).
*   **Relational Algebra (Theory):** Treats relations as **Sets**. Duplicate tuples are *never* allowed.
*   **SQL (Practice):** Treats tables as **Bags** (or Multisets). Duplicate tuples *are* allowed.
  *   **Why?** Eliminating duplicates is computationally expensive (requires sorting or hashing). SQL defaults to keeping duplicates for performance reasons.

**3. The `DISTINCT` Keyword**
If you want to force SQL to behave like a Set (i.e., remove duplicates), you must explicitly use the `DISTINCT` keyword.
*   **Syntax:** `SELECT DISTINCT AttributeName FROM TableName`
*   **Example:**
  *   If you have a table of 100 students, and 50 of them are 'Math' majors.
  *   `SELECT StudentDept FROM Students` returns 100 rows (lots of 'Math' duplicates).
  *   `SELECT DISTINCT StudentDept FROM Students` returns only the unique department names (e.g., 'Math', 'Computers').

**4. Basic Projection**
The `SELECT` clause performs the **Projection** operation ($\pi$) from Relational Algebra.
*   It creates a new table containing only the columns listed.
*   **Example:**
  *   Original Table: Contains `StudentID`, `StudentDept`, `StudentName`, `StudentAge`.
  *   Query: `SELECT studentID, studentName FROM students`
  *   Result: A table with only the `studentID` and `studentName` columns. The `StudentDept` and `StudentAge` columns are discarded for this specific result set.