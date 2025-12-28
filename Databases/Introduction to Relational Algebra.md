### **Introduction to Relational Algebra**

**What is Relational Algebra?**
Relational Algebra is a **procedural query language**. It's considered a "formal" language because it has a strong mathematical foundation in set theory. It consists of a set of operations that take one or two relations (tables) as input and produce a new relation as the result.

**Why is it Important?**
1.  **Foundation for SQL:** It provides the theoretical underpinning for data manipulation languages like SQL. Many SQL queries can be directly translated into a sequence of relational algebra operations.
2.  **Query Optimization:** When you submit an SQL query to a database, the internal query optimizer often translates it into a relational algebra expression (or a similar structure like a query tree). The optimizer then uses algebraic rules to transform this expression into an equivalent, but much more efficient, one before executing it.
3.  **Procedural Nature:** It's procedural because you specify *what* you want and *how* to get it step-by-step. This contrasts with a *declarative* language like SQL, where you mainly just specify *what* you want and let the DBMS figure out the "how."

**Core Properties**
*   **Closure:** Every operator takes one or more relations as input and produces a relation as output. This "closure" property is powerful because it allows us to nest or chain operations together to form complex queries.
*   **Operators:** The operators can be **unary** (operating on a single relation) or **binary** (operating on two relations).

---
Let's begin with the two fundamental unary operations.
- ### **Unary Operations: SELECT and PROJECT**
  
  These operations allow you to filter a single table, either by selecting specific rows or by selecting specific columns.
- #### **The SELECT (σ) Operation**
  
  The `SELECT` operation filters a relation **horizontally**. It chooses a subset of the **tuples (rows)** from a relation that satisfy a given condition.
  
  *   **Notation:** σ<sub><selection condition></sub>(R)
    *   `σ` (sigma) is the symbol for the SELECT operation.
    *   `R` is the input relation.
    *   `<selection condition>` is a boolean expression that is evaluated for each individual tuple in `R`. If the condition is true, the tuple is included in the result.
  *   **The Condition:** The selection condition is composed of comparisons (e.g., `=`, `>`, `<`) between attributes and constant values, or between two attributes. These can be combined using boolean operators (`AND`, `OR`, `NOT`).
  *   **Result:** The resulting relation has the *exact same schema* (same columns) as the original relation `R`, but contains only the rows that met the condition. The number of tuples in the result is always less than or equal to the number of tuples in `R`.
  
  **Example:**
  To retrieve all employees who work in department 4 and have a salary greater than 25000, OR who work in department 5 and have a salary greater than 30000:
  *   σ<sub>(Dno=4 AND Salary>25000) OR (Dno=5 AND Salary>30000)</sub>(EMPLOYEE)
    *   The result would include the tuples for 'Jennifer' (Dno 4, Salary 43000) and 'Franklin' and 'Ramesh' (Dno 5, Salaries 40000 and 38000).
- #### **The PROJECT (π) Operation**
  
  The `PROJECT` operation filters a relation **vertically**. It chooses a subset of the **columns (attributes)** from a relation, creating a new relation with only those specified columns.
  
  *   **Notation:** π(R)
    *   `π` (pi) is the symbol for the PROJECT operation.
    *   `R` is the input relation.
    *   `<attribute list>` is a comma-separated list of the columns you want to keep.
  *   **Duplicate Elimination:** This is a critical feature. Since a relation is a *set* of tuples, it cannot contain duplicates. The `PROJECT` operation automatically **removes any duplicate rows** from the result. This is likely to happen if you project out the key attributes of the relation.
  *   **Result:** The result is a new relation with only the columns specified in the attribute list. The number of tuples in the result is less than or equal to the number of tuples in `R`.
  
  **Example:**
  To get a list of the last name, first name, and salary of all employees:
  *   π<sub>Lname, Fname, Salary</sub>(EMPLOYEE)
    *   The result is a new table with just those three columns.