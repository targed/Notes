## Quiz 14
- ### **Topic: Outer Joins (Relational Algebra)**
  
  **1. Which of the following relational algebra expressions is equivalent to a Right Outer Join ($R \rightouterjoin S$)?**
  A. $S \leftouterjoin R$
  B. $(R \bowtie S) \cup (R - S)$
  C. $(R \bowtie S) \cup (S - R)$
  D. $S \bowtie R$
  
  **2. Consider two relations $A(id, val)$ and $B(id, val)$. Table A contains ids $\{1, 2\}$ and Table B contains ids $\{2, 3\}$. If you perform a Full Outer Join ($A \fullouterjoin B$) on `id`, how many tuples will be in the result?**
  A. 1 (The matching id 2)
  B. 2 (The original rows)
  C. 3 (1-NULL, 2-2, NULL-3)
  D. 4 (Cartesian product)
  
  **3. If the result of $R \leftouterjoin S$ contains exactly the same number of tuples as $R \bowtie S$ (Natural Join), what can we infer about relation R?**
  A. R is empty.
  B. Every tuple in R has at least one matching tuple in S (Total participation of R).
  C. Every tuple in S has a matching tuple in R.
  D. R and S are disjoint sets.
  
  **4. Which of the following operations creates `NULL` values in the result set that did not exist in the original database tables?**
  A. $\sigma$ (Select)
  B. $\cup$ (Union)
  C. $\times$ (Cross Product)
  D. $\rightouterjoin$ (Right Outer Join)
  
  **5. How would you simulate a Full Outer Join ($R \fullouterjoin S$) using other operators if the Full Join operator was not available?**
  A. $(R \leftouterjoin S) \cap (R \rightouterjoin S)$
  B. $(R \leftouterjoin S) \cup (R \rightouterjoin S)$
  C. $(R \bowtie S) \cup (R \times S)$
  D. $(R - S) \cup (S - R)$
  
  ---
- ### **Topic: SQL Table Definitions (DDL)**
  
  **6. Consider the following SQL snippet. What is syntactically WRONG with this definition?**
  ```sql
  CREATE TABLE Enrollment (
    StudentID INT PRIMARY KEY,
    CourseID INT PRIMARY KEY,
    Grade CHAR(1)
  );
  ```
  A. `CHAR(1)` is too small for a grade.
  B. A table cannot have two separate `PRIMARY KEY` clauses inline; a composite key must be defined at the table level.
  C. `INT` cannot be used for IDs.
  D. There is no error; this correctly creates a composite key.
  
  **7. When defining a Foreign Key constraint in SQL (e.g., `FOREIGN KEY (DeptID) REFERENCES Department(ID)`), what requirement must be met by the `ID` column in the `Department` table?**
  A. It must be of type `VARCHAR`.
  B. It must be a Primary Key or a Unique Key.
  C. It must contain the exact same values as the referencing table at all times.
  D. It must allow NULL values.
  
  **8. You need to store a standard US State abbreviation (e.g., 'MO', 'NY', 'CA') for every user. Which data type is most efficient and appropriate?**
  A. `VARCHAR(2)`
  B. `CHAR(2)`
  C. `TEXT`
  D. `INT`
  
  **9. Which of the following statements about the `UNIQUE` constraint is FALSE?**
  A. A table can have multiple `UNIQUE` constraints.
  B. `UNIQUE` columns can typically accept `NULL` values (depending on the specific DBMS).
  C. `UNIQUE` creates an index on the column.
  D. A column defined as `UNIQUE` is automatically the Primary Key.
  
  **10. You want to create a table `Orders` where the `OrderDate` defaults to the current system date if no date is provided during insertion. Which syntax is correct?**
  A. `OrderDate DATE DEFAULT CURRENT_DATE`
  B. `OrderDate DATE = TODAY`
  C. `OrderDate DATE PRIMARY KEY`
  D. `OrderDate DATE CHECK (CURRENT_DATE)`
  
  ---
- ## Quiz 15
- ---
- ### **Topic: SQL Language & Architecture**
  
  **1. Which of the following SQL commands is categorized as Data Definition Language (DDL)?**
  A. `INSERT INTO`
  B. `UPDATE`
  C. `DROP TABLE`
  D. `SELECT`
  
  **2. SQL is primarily described as what type of programming language?**
  A. Procedural (You specify the exact steps and algorithms to retrieve data).
  B. Declarative (You specify *what* data you want, not *how* to get it).
  C. Object-Oriented (Everything is treated as an object with methods).
  D. Low-level (You manipulate memory addresses and pointers directly).
  
  **3. Which component of the DBMS is responsible for translating a high-level SQL query into a specific, efficient execution plan (Relational Algebra)?**
  A. The Query Optimizer
  B. The Storage Manager
  C. The Transaction Manager
  D. The DDL Compiler
  
  ---
- ### **Topic: Constraints (CHECK, etc.)**
  
  **4. You want to ensure that in the `Project` table, the `EndDate` is always later than the `StartDate`. Which SQL constraint is most appropriate for this?**
  A. `FOREIGN KEY`
  B. `UNIQUE`
  C. `CHECK`
  D. `DEFAULT`
  
  **5. Which of the following statements about the `CHECK` constraint is TRUE?**
  A. It can be used to enforce referential integrity between two different tables.
  B. It acts as a primary key.
  C. It allows you to specify a boolean condition (e.g., `Salary > 0`) that every row must satisfy.
  D. It is only checked when data is deleted, not when it is inserted.
  
  ---
- ### **Topic: Grouping and Aggregation**
  
  **6. Consider the following query: `SELECT DeptID, AVG(Salary) FROM Employees GROUP BY DeptID`. If a specific `DeptID` has 5 employees, how many rows will that department generate in the final output?**
  A. 1 row
  B. 5 rows
  C. 0 rows
  D. It depends on the values of the Salary.
  
  **7. You want to calculate the total sales for each product, but you want to *exclude* any products where the total sales are less than $1,000. Which clause must contain the condition `SUM(Sales) >= 1000`?**
  A. `WHERE`
  B. `HAVING`
  C. `GROUP BY`
  D. `ORDER BY`
  
  **8. Why will the following query generate an error?**
  ```sql
  SELECT DeptName, COUNT(*) 
  FROM Employees 
  WHERE Salary > 50000;
  ```
  A. You cannot use `COUNT(*)` with `DeptName`.
  B. You cannot use `WHERE` with `COUNT(*)`.
  C. The query is missing a `GROUP BY DeptName` clause.
  D. `Salary` must be included in the `SELECT` list.
  
  **9. What is the correct logical order of execution for a SQL query?**
  A. `SELECT` $\to$ `FROM` $\to$ `WHERE`
  B. `FROM` $\to$ `WHERE` $\to$ `GROUP BY` $\to$ `HAVING` $\to$ `SELECT`
  C. `FROM` $\to$ `SELECT` $\to$ `GROUP BY` $\to$ `WHERE`
  D. `SELECT` $\to$ `GROUP BY` $\to$ `HAVING` $\to$ `WHERE`
  
  **10. If you execute a `SELECT DISTINCT` query, what is the database explicitly doing?**
  A. It is treating the table as a **Set** (removing duplicates) rather than a **Bag** (multiset).
  B. It is sorting the data in descending order.
  C. It is grouping the data by the Primary Key.
  D. It is removing NULL values from the result.
  
  ---
-
- ## Quiz 16
- ### **Topic: Handling NULLs and Aggregates**
  
  **1. Consider a table `Orders` with a column `ShipDate`. If 5 orders have been placed but not shipped (so `ShipDate` is NULL) and 10 orders have a valid `ShipDate`, what will `SELECT COUNT(ShipDate) FROM Orders` return?**
  A. 15
  B. 10
  C. 5
  D. NULL
  
  **2. Which logical expression evaluates to `TRUE` in SQL?**
  A. `NULL = NULL`
  B. `NULL != NULL`
  C. `NULL IS NULL`
  D. `NULL = 0`
  
  **3. You have a subquery that selects a single column of values containing `{1, 2, NULL}`. If you run the outer query `SELECT * FROM Table WHERE ID NOT IN (subquery)`, what is the result?**
  A. It returns all rows where ID is not 1 or 2.
  B. It returns rows where ID is NULL.
  C. It returns 0 rows (Empty Set).
  D. It throws a syntax error.
  
  ---
- ### **Topic: Cartesian Products & Joins**
  
  **4. Table `A` has 10 rows. Table `B` has 5 rows. You execute `SELECT * FROM A, B`. How many rows does the result contain?**
  A. 15
  B. 50
  C. 0 (because there is no `WHERE` condition)
  D. 10
  
  **5. Why is the Cartesian Product (Cross Product) considered the "logical base" of a JOIN operation in relational algebra (even if databases optimize it away)?**
  A. Because databases physically store data as cross products.
  B. Because logically, a Join is defined as a Cross Product followed by a Select (Filter) operation.
  C. Because it is the only way to handle NULL values.
  D. Because it removes duplicate rows automatically.
  
  ---
- ### **Topic: WHERE vs. HAVING**
  
  **6. Which of the following queries is syntactically INVALID?**
  A. `SELECT Dept, SUM(Salary) FROM Emp GROUP BY Dept HAVING SUM(Salary) > 10000`
  B. `SELECT Dept, SUM(Salary) FROM Emp WHERE Salary > 50000 GROUP BY Dept`
  C. `SELECT Dept, SUM(Salary) FROM Emp GROUP BY Dept WHERE SUM(Salary) > 10000`
  D. `SELECT Dept, AVG(Salary) FROM Emp GROUP BY Dept HAVING COUNT(*) > 5`
  
  **7. You want to calculate the average GPA of students, but you only want to include Seniors (Year=4) in the calculation. You do NOT want to filter out majors with low GPAs. Where does the condition `Year=4` belong?**
  A. `HAVING`
  B. `WHERE`
  C. `GROUP BY`
  D. `SELECT`
  
  ---
- ### **Topic: Subqueries & Operators**
  
  **8. If a subquery returns multiple rows (e.g., a list of IDs), which operator CANNOT be used to compare the outer query attribute to the subquery result?**
  A. `IN`
  B. `= ANY`
  C. `=`
  D. `NOT IN`
  
  **9. Consider the query: `SELECT * FROM Students WHERE EXISTS (SELECT NULL FROM Enrollment WHERE ...)` . If the inner query returns a row containing `NULL`, what does `EXISTS` return?**
  A. `FALSE` because the value is NULL.
  B. `UNKNOWN`.
  C. `TRUE`.
  D. It causes an error.
  
  **10. When does the database evaluate the `SELECT` clause (e.g., assigning aliases like `SELECT Salary AS Pay`)?**
  A. Before the `WHERE` clause.
  B. After the `FROM` clause but before `GROUP BY`.
  C. After `WHERE` and `GROUP BY`, but before `ORDER BY`.
  D. First, before any other clause.
  
  ---
- ## Quiz 17
- ### **Topic: Aggregate Function Rules**
  
  **1. Which of the following SQL statements causes a syntax error?**
  A. `SELECT Dept, MAX(Salary) FROM Emp GROUP BY Dept`
  B. `SELECT Dept FROM Emp GROUP BY Dept HAVING MAX(Salary) > 10000`
  C. `SELECT * FROM Emp WHERE Salary > (SELECT AVG(Salary) FROM Emp)`
  D. `SELECT * FROM Emp WHERE Salary > AVG(Salary)`
  
  **2. Is it possible to nest aggregate functions in standard SQL (e.g., `SELECT MAX(AVG(Salary))...`)?**
  A. Yes, it calculates the average of the maximums.
  B. No, standard SQL does not allow nesting aggregate functions directly.
  C. Yes, but only if you use a `GROUP BY` clause.
  D. Yes, but only in the `HAVING` clause.
  
  **3. Consider a column `Bonus` containing values `{100, NULL, 200}`. What does `SELECT SUM(Bonus)` return?**
  A. `NULL` (Because `100 + NULL + 200` is unknown).
  B. `300` (NULLs are ignored).
  C. `0` (NULLs are treated as zero).
  D. An error.
  
  **4. Can the `MIN()` and `MAX()` aggregate functions be used on `VARCHAR` (String) columns?**
  A. No, they only work on numeric data.
  B. Yes, they return the shortest and longest strings by length.
  C. Yes, they return the first and last values alphabetically (lexicographically).
  D. No, unless the string contains only numbers.
  
  ---
- ### **Topic: Subquery Placement & Behavior**
  
  **5. You place a subquery inside the `FROM` clause, like this: `SELECT * FROM (SELECT ... ) AS T`. What is this subquery formally called?**
  A. A Correlated Subquery.
  B. A Derived Table (or Inline View).
  C. A Scalar Subquery.
  D. A Common Table Expression (CTE).
  
  **6. If you place a subquery in the `SELECT` list (e.g., `SELECT Name, (SELECT ...) FROM Emp`), what restriction applies to that subquery?**
  A. It must return a table with the same number of columns as the outer query.
  B. It must return exactly one row and one column (a Scalar value).
  C. It must be an uncorrelated subquery.
  D. It must use an aggregate function.
  
  **7. Which operator allows you to compare a value against *every* value returned by a subquery?**
  A. `IN`
  B. `ANY`
  C. `ALL`
  D. `EXISTS`
  
  **8. If a subquery placed in the `SELECT` clause returns **zero** rows for a specific outer row, what value is displayed in the result?**
  A. `0`
  B. `NULL`
  C. An empty string `""`
  D. The query fails with a runtime error.
  
  **9. Why is an alias (e.g., `AS T`) strictly required when using a subquery in the `FROM` clause in most SQL databases?**
  A. Because the database needs a name to refer to the temporary table generated by the subquery.
  B. Because it improves performance.
  C. It is not required; it is optional.
  D. Because subqueries in `FROM` cannot return duplicate rows.
  
  **10. Consider this query using `ANY`: `WHERE Salary > ANY (SELECT Salary FROM Emp WHERE Dept='Sales')`. This is logically equivalent to:**
  A. `WHERE Salary > (SELECT MAX(Salary) FROM Emp WHERE Dept='Sales')`
  B. `WHERE Salary > (SELECT MIN(Salary) FROM Emp WHERE Dept='Sales')`
  C. `WHERE Salary > (SELECT AVG(Salary) FROM Emp WHERE Dept='Sales')`
  D. `WHERE Salary IN (SELECT Salary FROM Emp WHERE Dept='Sales')`
  
  ---
- ## Quiz 18
- ### **Topic: GROUP BY and Selection Rules**
  
  **1. Consider the query: `SELECT Department, JobTitle, COUNT(*) FROM Employees GROUP BY Department`. Why will this generate an error in standard SQL?**
  A. You cannot group by `Department` without also grouping by `JobTitle`.
  B. `JobTitle` is in the SELECT list but not in the GROUP BY list, and it is not aggregated.
  C. `COUNT(*)` cannot be used with multiple columns.
  D. You must use `ORDER BY` when using `GROUP BY`.
  
  **2. If you group by multiple columns (e.g., `GROUP BY City, JobTitle`), what does each row in the result represent?**
  A. Each city gets one row, regardless of job titles.
  B. Each job title gets one row, regardless of the city.
  C. Each unique combination of City and JobTitle gets one row.
  D. It returns the Cartesian product of Cities and JobTitles.
  
  **3. Which of the following queries calculates the number of distinct (unique) job titles within each department?**
  A. `SELECT Dept, COUNT(JobTitle) FROM Emp GROUP BY Dept`
  B. `SELECT Dept, DISTINCT COUNT(JobTitle) FROM Emp GROUP BY Dept`
  C. `SELECT Dept, COUNT(DISTINCT JobTitle) FROM Emp GROUP BY Dept`
  D. `SELECT Dept, JobTitle FROM Emp GROUP BY Dept`
  
  ---
- ### **Topic: HAVING vs. WHERE**
  
  **4. When the database engine processes a query, which of the following happens LAST?**
  A. Rows are filtered by `WHERE`.
  B. Groups are filtered by `HAVING`.
  C. Rows are grouped by `GROUP BY`.
  D. Rows are retrieved from the table (`FROM`).
  
  **5. Is it valid to use the `HAVING` clause without a `GROUP BY` clause?**
  A. No, `HAVING` is syntactically tied to `GROUP BY`.
  B. Yes, but it treats the entire table as one single group (e.g., to check if the total sum of the whole table meets a condition).
  C. Yes, it effectively acts exactly like a `WHERE` clause for individual rows.
  D. No, unless you are using Oracle.
  
  **6. You want to find departments where the *average* salary is greater than $50,000, but you only want to include employees who are Full-Time in that calculation. How do you structure the query?**
  A. `WHERE` Status='FT' `AND` AVG(Salary) > 50000
  B. `HAVING` Status='FT' `AND` AVG(Salary) > 50000
  C. `WHERE` Status='FT' ... `HAVING` AVG(Salary) > 50000
  D. `HAVING` Status='FT' ... `WHERE` AVG(Salary) > 50000
  
  ---
- ### **Topic: Correlated Subqueries**
  
  **7. Which characteristic defines a "Correlated Subquery"?**
  A. The inner query returns multiple rows.
  B. The inner query references a column from the outer query.
  C. The inner query uses an aggregate function.
  D. The inner query is executed exactly once for the entire operation.
  
  **8. In terms of performance complexity (Big O notation), if the outer table has N rows and the inner table has M rows, how many times does the inner logic execute for a Correlated Subquery?**
  A. 1 time.
  B. N times.
  C. M times.
  D. N $\times$ M times (Cartesian product).
  
  **9. Consider the following query. What makes it correlated?**
  ```sql
  SELECT * FROM Employee E 
  WHERE Salary > (SELECT AVG(Salary) FROM Employee S WHERE S.DeptID = E.DeptID)
  ```
  A. It uses the `AVG` function.
  B. It compares Salary using `>`.
  C. The inner `WHERE` clause uses `E.DeptID` from the outer table alias.
  D. Both tables are named `Employee`.
  
  **10. What is the scope of visibility for column aliases in a correlated subquery?**
  A. The inner query can see the outer query's columns, but the outer query cannot see the inner's.
  B. The outer query can see the inner query's columns, but the inner query cannot see the outer's.
  C. Both queries can see all columns from both tables freely.
  D. Neither query can reference the other's columns.
  
  ---
- ## Quiz 19
- ### **Topic: Relational Division ("For All" Queries)**
  
  **1. You want to find students who have taken **every** Biology course offered by the university. Which of the following SQL logic patterns is the standard way to achieve this using **Double Negation**?**
  A. Select Students where `COUNT(Courses) = ALL`.
  B. Select Students where **there does not exist** a Biology course that the student has **not taken**.
  C. Select Students where `Course = 'Biology'` AND `Status = 'All'`.
  D. Select Students where **there exists** a Biology course that the student **has taken**.
  
  **2. To perform Relational Division using the **Aggregation** approach (instead of double negation), which clause is critical?**
  A. `WHERE COUNT(StudentCourses) = COUNT(AllBiologyCourses)`
  B. `HAVING COUNT(DISTINCT CourseID) = (SELECT COUNT(*) FROM BiologyCourses)`
  C. `GROUP BY CourseID HAVING COUNT(*) = ALL`
  D. `WHERE CourseID IN (SELECT CourseID FROM BiologyCourses)`
  
  **3. Does standard SQL include a dedicated operator (like `DIVIDE BY`) for relational division?**
  A. Yes, introduced in SQL:2016.
  B. Yes, but only in Oracle.
  C. No, it must be simulated using other operators.
  D. No, because "For All" queries are impossible in databases.
  
  **4. Consider the set operation `A EXCEPT B` (or `A MINUS B`). How is this useful for Division?**
  A. It finds items in A that are also in B.
  B. It acts as a union of A and B.
  C. It finds items in A that are **missing** from B. (Useful for finding "missing requirements").
  D. It calculates the Cartesian product.
  
  ---
- ### **Topic: Correlated Queries & Decorrelation**
  
  **5. While not *every* correlated query can be turned into a standard subquery, most can be rewritten using which standard SQL construct to improve performance?**
  A. `JOIN` (specifically mapping derived tables).
  B. `UNION`.
  C. `ORDER BY`.
  D. `INSERT INTO`.
  
  **6. Why might a database administrator (DBA) prefer a `JOIN` over a Correlated Subquery?**
  A. Joins remove NULL values automatically.
  B. Joins are easier to write.
  C. Correlated subqueries execute the inner logic $N$ times (once per row), whereas hash/merge joins are set-based and often faster.
  D. Correlated subqueries are not standard SQL.
  
  ---
- ### **Topic: Redundancy, Normalization, & Denormalization**
  
  **7. Which of the following is the primary **disadvantage** of redundancy (storing the same data in multiple places)?**
  A. Slower read speeds.
  B. Update Anomalies (Risk of data inconsistency when modifying data).
  C. Inability to use indexes.
  D. Complex `SELECT` queries.
  
  **8. What is **Denormalization**?**
  A. The process of fixing errors in 1NF tables.
  B. The process of converting a database to NoSQL.
  C. The deliberate introduction of redundancy (combining tables) to improve read performance at the cost of write overhead.
  D. The process of deleting old data to save space.
  
  **9. In a highly Normalized database (3NF/BCNF), read queries often require many `JOIN`s. How does this impact performance?**
  A. It makes reads faster because tables are smaller.
  B. It creates "Spurious Tuples".
  C. It can slow down reads because joining tables is computationally expensive.
  D. It has no impact on performance.
  
  **10. Scenario: You have a "TotalSales" column in the "Customers" table. Every time a customer places an order, you update this column. This design is:**
  A. Normalized (Good design).
  B. Redundant/Denormalized (Derived attribute stored physically).
  C. Impossible in SQL.
  D. Necessary for Referential Integrity.
  
  ---
-
- ## Quiz 20
- ### **Topic: Functional Dependencies (Definition & Axioms)**
  
  **1. Which of the following statements about Functional Dependencies is FALSE?**
  A. FDs are a property of the relation schema, not just a specific state of the relation.
  B. If $X \rightarrow Y$, then for any two tuples $t1$ and $t2$, if $t1[X] = t2[X]$, then $t1[Y]$ must equal $t2[Y]$.
  C. You can prove an FD is universally true by looking at a single snapshot (instance) of a table.
  D. FDs are derived from the semantics (business rules) of the data.
  
  **2. Which of Armstrong's Axioms states: "If $X \rightarrow Y$, then $XZ \rightarrow YZ$"?**
  A. Reflexivity
  B. Augmentation
  C. Transitivity
  D. Pseudotransitivity
  
  **3. An FD is considered **Trivial** if:**
  A. The right-hand side is a subset of the left-hand side (e.g., $AB \rightarrow A$).
  B. The left-hand side contains a primary key.
  C. The right-hand side contains a foreign key.
  D. It violates 2NF.
  
  ---
- ### **Topic: Attribute Closure ($X^+$)**
  
  **4. Given the dependencies $A \rightarrow B$ and $B \rightarrow C$. What is the closure of $\{A\}$ (denoted as $A^+$)?**
  A. $\{A, B\}$
  B. $\{A, B, C\}$
  C. $\{B, C\}$
  D. $\{A\}$
  
  **5. If the closure of an attribute set $X$ (i.e., $X^+$) contains **all** the attributes in the relation $R$, then $X$ is defined as:**
  A. A Foreign Key.
  B. A Candidate Key (only).
  C. A Superkey.
  D. A Minimal Cover.
  
  **6. You want to determine if the dependency $X \rightarrow Y$ is redundant (implied by other dependencies). Which closure calculation allows you to check this?**
  A. Calculate $X^+$ using all FDs *except* $X \rightarrow Y$. If the result contains $Y$, the rule is redundant.
  B. Calculate $Y^+$ using all FDs. If it contains $X$, it is redundant.
  C. Calculate $(X \cup Y)^+$.
  D. Calculate $X^+$ including the rule $X \rightarrow Y$.
  
  ---
- ### **Topic: Minimal Cover (Canonical Cover)**
  
  **7. Which of the following is NOT a requirement for a set of FDs to be a Minimal Cover?**
  A. The right-hand side of every FD must be a single attribute (Singleton RHS).
  B. There must be no redundant FDs in the set.
  C. There must be no extraneous attributes in the left-hand side of any FD.
  D. Every FD must be in Third Normal Form (3NF).
  
  **8. Consider the set $F = \{A \rightarrow B, A \rightarrow C\}$. To convert this to a Minimal Cover, what is the first step?**
  A. Merge them into $A \rightarrow BC$.
  B. Leave them as is; they already satisfy the Singleton RHS rule.
  C. Remove $A \rightarrow C$ because it is redundant.
  D. Add the trivial dependency $A \rightarrow A$.
  
  **9. Consider the FD: $\{A, B\} \rightarrow C$. If we find that $A \rightarrow C$ is also true, then the attribute $B$ in the first FD is considered:**
  A. Prime
  B. Extraneous
  C. Transitive
  D. Minimal
  
  **10. Two sets of functional dependencies, $F$ and $G$, are considered **Equivalent** if:**
  A. They have the same number of FDs.
  B. They cover the exact same attributes.
  C. $F$ covers $G$ (every FD in $G$ can be inferred from $F$) AND $G$ covers $F$.
  D. Their Minimal Covers have the same number of lines.
  
  ---
- ## Quiz 21
- ### **Topic: Data vs. Schema Constraints**
  
  **1. You analyze a database table with 1,000 rows. You find that every time `ZipCode` is '90210', the `State` is 'CA'. Can you conclude that the functional dependency `ZipCode` $\rightarrow$ `State` holds for this relation?**
  A. Yes, the data proves it is true.
  B. No, the dataset might be too small or biased; the FD might be violated by a future insert.
  C. Yes, but only if '90210' is the Primary Key.
  D. No, because FDs cannot exist between non-key attributes.
  
  **2. Which of the following scenarios constitutes a "Counter-Example" that effectively disproves the FD $A \rightarrow B$?**
  A. Two tuples have different values for $A$ and different values for $B$.
  B. Two tuples have different values for $A$ but the same value for $B$.
  C. Two tuples have the same value for $A$ but different values for $B$.
  D. Two tuples have the same value for $A$ and the same value for $B$.
  
  **3. Who or what is the ultimate authority for defining which Functional Dependencies are valid for a specific database?**
  A. The Database Management System (DBMS) software.
  B. The current data residing in the tables.
  C. The database designer/domain expert based on business rules.
  D. Armstrong's Axioms.
  
  **4. If $X$ is a Candidate Key for relation $R$, which of the following statements is guaranteed to be true for *any* attribute $Y$ in $R$?**
  A. $Y \rightarrow X$
  B. $X \rightarrow Y$
  C. $X$ and $Y$ are functionally independent.
  D. $Y$ must be a prime attribute.
  
  ---
- ### **Topic: Armstrong's Axioms & Logic**
  
  **5. Armstrong's Axioms are described as "Sound". What does this mean?**
  A. If the axioms derive an FD $X \rightarrow Y$, then $X \rightarrow Y$ is guaranteed to be valid/true.
  B. The axioms can derive every possible valid FD (they don't miss anything).
  C. The axioms can verify if a set of FDs is minimal.
  D. The axioms can identify primary keys automatically.
  
  **6. Which of the following is an example of the **Augmentation** rule?**
  A. If $A \rightarrow B$ and $B \rightarrow C$, then $A \rightarrow C$.
  B. If $A \rightarrow B$, then $AC \rightarrow BC$.
  C. If $A \rightarrow BC$, then $A \rightarrow B$.
  D. If $AB \rightarrow C$, then $A \rightarrow C$.
  
  **7. Consider the Trivial Dependency rule (Reflexivity). Which of the following FDs is **always** true, regardless of the data?**
  A. `Name` $\rightarrow$ `SSN`
  B. `{Name, SSN}` $\rightarrow$ `Name`
  C. `SSN` $\rightarrow$ `Name`
  D. `SSN` $\rightarrow$ `SSN` is the only trivial dependency.
  
  **8. Given the set of FDs $F = \{A \rightarrow B, A \rightarrow C\}$. Using the Union rule (which is derived from Armstrong's Axioms), what can we infer?**
  A. $B \rightarrow C$
  B. $A \rightarrow BC$
  C. $BC \rightarrow A$
  D. $A \rightarrow A$
  
  **9. If we say a set of inference rules is "Complete", we mean:**
  A. The rules generate no false positives.
  B. The rules can be computed in polynomial time.
  C. The rules are sufficient to derive *every* FD that is logically implied by the starting set.
  D. The rules require the complete database state to function.
  
  **10. You are given $F = \{A \rightarrow B, B \rightarrow C\}$. You want to verify if $A \rightarrow C$ is true. Which approach is valid?**
  A. Scan the database table looking for rows where $A$ matches but $C$ differs.
  B. Use Armstrong's Transitivity rule to derive it logically.
  C. Calculate the closure of $C$ ($C^+$).
  D. Both A and B are valid approaches (one empirical, one logical).
  
  ---
-
- ## Quiz 22
- ### **Topic: Normal Form Violations**
  
  **1. Consider a relation $R(A, B, C)$ with a Composite Primary Key $\{A, B\}$. Which of the following functional dependencies represents a violation of **2NF**?**
  A. $A \to C$
  B. $\{A, B\} \to C$
  C. $C \to A$
  D. $C \to B$
  
  **2. Consider a relation $R(X, Y, Z)$ with Primary Key $\{X\}$. The functional dependencies are $\{X \to Y, Y \to Z\}$. Which of the following statements is TRUE?**
  A. The relation is in 2NF but violates 3NF due to a partial dependency.
  B. The relation is in 2NF but violates 3NF due to a transitive dependency.
  C. The relation is in 3NF.
  D. The relation violates 2NF.
  
  **3. Which of the following conditions allows a relation to be in 3NF even if $X \to Y$ exists where $X$ is not a superkey? (The "3NF Loophole")**
  A. If $Y$ is a numeric data type.
  B. If $Y$ is a Prime Attribute (part of a candidate key).
  C. If $X$ is a subset of the Primary Key.
  D. If $X$ and $Y$ are both non-prime attributes.
  
  ---
- ### **Topic: Lossless Join Property**
  
  **4. You decompose relation $R(A, B, C)$ into $R1(A, B)$ and $R2(B, C)$. The only functional dependency is $A \to C$. Is this decomposition lossless?**
  A. Yes, because $B$ is the common attribute.
  B. Yes, because $A \to C$ is preserved.
  C. No, because the intersection $\{B\}$ is not a key for $R1$ or $R2$.
  D. No, because the dependency $A \to C$ is lost.
  
  **5. Which of the following is the formal condition for a decomposition of $R$ into $R1$ and $R2$ to be **Lossless**?**
  A. $(R1 \cap R2) \to R1$ OR $(R1 \cap R2) \to R2$
  B. $(R1 \cup R2) \to R$
  C. $R1 \cap R2 = \emptyset$ (Empty Set)
  D. The primary key of $R1$ must be the same as the primary key of $R2$.
  
  **6. If a decomposition is "Lossy" (not lossless), what is the specific negative consequence when you join the tables back together?**
  A. You lose data (rows disappear).
  B. You create **Spurious Tuples** (extra, invalid rows appear).
  C. The query becomes slower.
  D. You can no longer perform `UPDATE` operations.
  
  ---
- ### **Topic: Dependency Preservation**
  
  **7. Consider $R(A, B, C, D)$ with FDs $\{A \to B, B \to C, C \to D\}$. You decompose it into $R1(A, B)$, $R2(B, C)$, and $R3(C, D)$. Is this decomposition dependency-preserving?**
  A. Yes, all FDs are covered by the individual tables.
  B. No, $A \to B$ is lost.
  C. No, $B \to C$ is lost.
  D. No, transitive dependencies cannot be preserved.
  
  **8. Consider $R(A, B, C)$ with FD $\{A \to B, AC \to B\}$. You decompose into $R1(A, C)$ and $R2(B, C)$. Is $A \to B$ preserved?**
  A. Yes, because $A$ is in $R1$ and $B$ is in $R2$.
  B. Yes, because of transitivity.
  C. No, because $A$ and $B$ are not in the same table.
  D. No, because $C$ is extraneous.
  
  ---
- ### **Topic: Normalization Logic**
  
  **9. You have a relation `Enrollment(StudentID, CourseID, ProfessorName)` with Primary Key `{StudentID, CourseID}`. You find that `CourseID` uniquely determines `ProfessorName`. Which Normal Form is violated?**
  A. 1NF (Atomic values)
  B. 2NF (Partial Dependency)
  C. 3NF (Transitive Dependency)
  D. BCNF only.
  
  **10. If a relation has a Primary Key consisting of a **single attribute** (Simple Key), which Normal Form is it guaranteed to satisfy by default?**
  A. 2NF
  B. 3NF
  C. BCNF
  D. 4NF
  
  ---
- ## Quiz 23
- ### **Topic: BCNF and Normalization Analysis**
  
  **1. Consider relation $R(A, B, C)$ with functional dependencies $\{A \to B, B \to C\}$. The only Candidate Key is $\{A\}$. Which of the following statements is TRUE?**
  A. The relation is in BCNF.
  B. The relation is in 3NF but violates BCNF.
  C. The relation violates BCNF because $B \to C$ holds, but $B$ is not a superkey.
  D. The relation violates BCNF because $A \to B$ holds, but $A$ is not a superkey.
  
  **2. Consider relation $R(X, Y, Z, W)$ with FDs $\{X \to Y, Y \to Z, Z \to X, Z \to W\}$. Which of the following attributes is NOT a candidate key?**
  A. $X$
  B. $Y$
  C. $Z$
  D. $W$
  
  **3. In a relation $R(A, B, C)$, if the FDs are $\{AB \to C, C \to B\}$, and the candidate keys are $\{AB\}$ and $\{AC\}$, is this relation in BCNF?**
  A. Yes, because all determinants are superkeys.
  B. No, because $C \to B$ exists, and $C$ is not a superkey.
  C. Yes, because $B$ is a prime attribute.
  D. No, because there is a partial dependency.
  
  ---
- ### **Topic: Lossless Join Decomposition**
  
  **4. You decompose $R(A, B, C, D, E)$ into $R1(A, B, C)$ and $R2(C, D, E)$. The functional dependencies are $\{A \to B, C \to D, D \to E\}$. Is this decomposition lossless?**
  A. Yes, because $C$ is the common attribute and $C \to \{D, E\}$ is implied by the FDs.
  B. Yes, because $A \to B$ is preserved.
  C. No, because the intersection is empty.
  D. No, because the common attribute $C$ does not determine $R1$ or $R2$.
  
  **5. Which of the following conditions is **sufficient** to guarantee that a decomposition of $R$ into $R1$ and $R2$ is lossless?**
  A. $R1 \cup R2 = R$
  B. $R1 \cap R2$ is a foreign key in $R$.
  C. $R1 \cap R2 \to R1$
  D. $R1$ and $R2$ are both in BCNF.
  
  **6. Consider $R(A, B, C)$ with $A \to B$. Decomposition: $R1(A, B)$, $R2(A, C)$. Is it lossless?**
  A. Yes, because intersection $A$ determines $R1 (A, B)$.
  B. No, because $A$ does not determine $C$.
  C. Yes, because intersection $A$ determines $R2 (A, C)$.
  D. No, this is a lossy decomposition.
  
  ---
- ### **Topic: Physical File Organization**
  
  **7. Which of the following is a characteristic of a **Heap File**?**
  A. Records are physically stored in order by the Primary Key.
  B. Insertion is very fast ($O(1)$) because new records are placed at the end of the file.
  C. Searching for a specific record is fast ($O(\log N)$) using binary search.
  D. It requires a dense index to function.
  
  **8. In a **Sorted (Sequential) File** organized by the field `EmployeeID`, how does the DBMS typically handle an `INSERT` operation?**
  A. It appends the record to the end of the file immediately.
  B. It must locate the correct physical position for `EmployeeID` and shift subsequent records to make room, making it expensive.
  C. It hashes the `EmployeeID` to find a bucket.
  D. It creates a new overflow file for every new record.
  
  **9. What is a "Deletion Marker" in the context of file organization?**
  A. A log entry indicating a table was dropped.
  B. A flag (bit) stored with a record to indicate it has been deleted, allowing the space to be reused later without reorganizing the file immediately.
  C. A pointer to the next valid block.
  D. A constraint that prevents deletion of parent records.
  
  **10. Which physical file organization allows for the use of Binary Search to locate records?**
  A. Heap File
  B. Hashed File
  C. Ordered (Sorted) File
  D. Linked File
  
  ---
- ## Quiz 24
- ### **Topic: Primary File Organizations (Heap, Sorted, Hash)**
  
  **1. Which file organization provides the fastest possible performance for retrieving a record based on an exact match of its key (Equality Search), assuming no collisions?**
  A. Heap File
  B. Sorted File
  C. Hash File
  D. Linked File
  
  **2. Why is a **Heap File** often chosen for transaction logging or audit trails?**
  A. Because it supports binary search.
  B. Because insertion is extremely fast ($O(1)$) as records are just appended to the end.
  C. Because it automatically sorts data by timestamp.
  D. Because it prevents duplicate records.
  
  **3. In a **Static Hash File**, what happens when a new record hashes to a bucket that is already full?**
  A. The hash function is changed immediately to accommodate the new record.
  B. The record is discarded.
  C. An overflow bucket is created and linked to the main bucket (Chaining).
  D. The entire file is re-sorted.
  
  **4. Which of the following queries would perform **worst** on a file organized using Hashing?**
  A. `SELECT * FROM Employees WHERE ID = 105`
  B. `SELECT * FROM Employees WHERE ID > 100 AND ID < 200`
  C. `DELETE FROM Employees WHERE ID = 50`
  D. `UPDATE Employees SET Name = 'Bob' WHERE ID = 99`
  
  **5. In a **Sorted File** (Sequential), if you want to use Binary Search to find a record, what requirement must be met?**
  A. The file must be stored using Linked Allocation.
  B. The search condition must be on the **Ordering Field**.
  C. The file must be small enough to fit in RAM.
  D. The file must have a secondary index.
  
  ---
- ### **Topic: File Allocation Methods**
  
  **6. Which disk allocation method requires the file to be stored in consecutive blocks, making it very fast to read sequentially but difficult to expand (grow) later?**
  A. Linked Allocation
  B. Indexed Allocation
  C. Contiguous Allocation
  D. Hashed Allocation
  
  **7. How does **Indexed Allocation** solve the random-access problem found in Linked Allocation?**
  A. It stores all block pointers in a single "Index Block," allowing the OS to calculate the location of any block directly without traversing a chain.
  B. It physically moves all blocks to be next to each other.
  C. It sorts the data inside the blocks.
  D. It uses a B+ Tree.
  
  **8. What is the primary disadvantage of **Linked Allocation**?**
  A. It suffers from external fragmentation.
  B. It requires pre-allocation of the file size.
  C. It supports only sequential access efficiently; fetching the "100th block" requires reading the previous 99 blocks.
  D. It wastes significant space due to pointers.
  
  ---
- ### **Topic: General Physical Storage Concepts**
  
  **9. When a record is "Deleted" from a Heap or Sorted file, it is rarely physically erased immediately. Instead, a **Deletion Marker** is used. Why?**
  A. To allow the user to "Undo" the delete later.
  B. Because physical deletion would require shifting all subsequent records to fill the gap, which is too expensive.
  C. Because the disk hardware cannot overwrite data.
  D. To maintain referential integrity.
  
  **10. Which of the following is NOT a component of a physical disk address?**
  A. Cylinder Number
  B. Track Number
  C. Sector (or Block) Number
  D. Tuple ID
  
  ---
- ## Quiz 25
- ### **Topic: Index Definitions (Primary, Clustering, Secondary)**
  
  **1. You have a data file containing `Student` records. The file is physically sorted by `Major` (e.g., all Biology students are stored together, followed by Chemistry). Since `Major` is not unique, what type of index would you create on the `Major` field?**
  A. Primary Index
  B. Secondary Index
  C. Clustering Index
  D. Hash Index
  
  **2. If a data file is sorted by its Primary Key (`ID`), and you create an index on a different non-unique column (`LastName`), what type of index is the `LastName` index?**
  A. Primary Index
  B. Clustering Index
  C. Secondary Index
  D. Sparse Index
  
  **3. Which of the following combinations correctly defines a **Primary Index**?**
  A. Ordering Field + Non-Key Field
  B. Ordering Field + Key Field
  C. Non-Ordering Field + Key Field
  D. Non-Ordering Field + Non-Key Field
  
  **4. Can a single database table have both a Primary Index and a Clustering Index simultaneously?**
  A. Yes, this is common practice for performance.
  B. Yes, but only if they are on the same column.
  C. No, because a file can only be physically sorted by one field (or combination of fields) at a time.
  D. No, because Primary Indexes are dense and Clustering Indexes are sparse.
  
  ---
- ### **Topic: Sparse vs. Dense Indexes**
  
  **5. Which type of index is typically **Dense** (contains an index entry for every single record in the data file)?**
  A. Primary Index
  B. Clustering Index
  C. Secondary Index
  D. Any index on a unique field.
  
  **6. Why is a Primary Index usually much smaller (in disk size) than a Secondary Index for the same table?**
  A. Because Primary Indexes use compression.
  B. Because Primary Indexes are **Sparse** (one entry per block), whereas Secondary Indexes are **Dense** (one entry per record).
  C. Because Primary Indexes do not store pointers.
  D. Because Secondary Indexes store the actual data records.
  
  **7. In a sparse Primary Index, what does the index entry point to?**
  A. It points to the specific record with that key.
  B. It points to the **Anchor Record** (the first record) of the data block containing that key range.
  C. It points to a bucket of pointers.
  D. It points to the file header.
  
  ---
- ### **Topic: Index mechanics**
  
  **8. You have a file sorted by `SSN`. You want to find a record by `EmployeeID` (which is unique, but the file is not sorted by it). What structure is required for the `EmployeeID` index?**
  A. It must be Sparse.
  B. It must be Dense.
  C. It must be a Clustering Index.
  D. It cannot be indexed.
  
  **9. Consider a Secondary Index created on a **Non-Key** field (e.g., `JobTitle`). Since multiple records can have the title "Engineer", what does the index entry for "Engineer" typically point to?**
  A. It points to the first "Engineer" record, and you scan sequentially from there.
  B. It creates multiple index entries for "Engineer", one for each record.
  C. It points to a **block of pointers** (or a bucket) which lists the addresses of all "Engineer" records.
  D. It points to the Primary Key index.
  
  **10. Which of the following statements regarding the physical cost of indexes is TRUE?**
  A. Indexes speed up reads (SELECT) but slow down writes (INSERT/UPDATE/DELETE).
  B. Indexes speed up both reads and writes.
  C. Indexes slow down reads but speed up writes.
  D. Secondary indexes are faster to update than Primary indexes.
  
  ---
- ## Quiz 26
- ### **Topic: Sparse vs. Dense Indexes**
  
  **1. Why can a Primary Index be **Sparse** (contain fewer entries than there are records), whereas a Secondary Index must be **Dense**?**
  A. Because Primary Indexes use hashing.
  B. Because the underlying data file is sorted by the primary key, so the system can infer the location of records between index entries.
  C. Because Secondary Indexes are stored on faster disk drives.
  D. This is false; Secondary Indexes are usually sparse to save space.
  
  **2. In a Sparse Index, what is a "Block Anchor"?**
  A. The last record in a data block.
  B. A pointer to the overflow file.
  C. The first record in a data block, whose key value is stored in the index to represent the range of values in that block.
  D. A locking mechanism to prevent concurrent writes.
  
  **3. If you have a data file with 100,000 records stored in 1,000 blocks (100 records per block), how many entries will a **Sparse Primary Index** contain?**
  A. 100,000
  B. 1,000
  C. 100
  D. 10
  
  ---
- ### **Topic: Performance (Range vs. Equality)**
  
  **4. Why is a **Secondary Index** generally poor for **Range Queries** (e.g., `SELECT * FROM Emp WHERE Salary BETWEEN 50k AND 60k`)?**
  A. Secondary indexes cannot support inequality operators.
  B. Because the data file is not sorted by the secondary key, fetching the records requires "jumping" randomly around the disk for every single match (Random I/O).
  C. Because secondary indexes are dense, making them too large to search.
  D. Because secondary indexes do not store pointers to data.
  
  **5. Which index type provides the absolute best performance for a Range Query?**
  A. Hash Index
  B. Secondary Index on a unique field
  C. Clustering Index (or Primary Index)
  D. Bitmap Index
  
  **6. You run a query searching for a specific value: `WHERE SSN = '123-45-6789'`. Both a Primary Index (Sparse) and a Secondary Index (Dense) exist. Which is theoretically faster to traverse?**
  A. The Secondary Index, because it is dense and points directly to the record.
  B. The Primary Index, because the index file itself is smaller (fewer entries), requiring fewer disk I/O operations to find the target block.
  C. They are exactly the same speed.
  D. The Secondary Index, because it uses hashing.
  
  ---
- ### **Topic: Index Structure & Maintenance**
  
  **7. A **Secondary Index** is created on a field that is **NOT** a candidate key (e.g., `DepartmentName`). Since multiple employees can be in "Sales", how does the index handle the duplicate values?**
  A. It repeats the key value "Sales" for every employee in the index file.
  B. It stores "Sales" once, and the pointer points to a bucket (or list) of record pointers.
  C. It is impossible to create a secondary index on a non-unique field.
  D. It forces the data file to be re-sorted by DepartmentName.
  
  **8. Which of the following is a disadvantage of a **Dense** index compared to a Sparse index?**
  A. It requires more storage space and imposes a higher maintenance cost during insertions/deletions.
  B. It cannot be used for binary search.
  C. It is slower for equality lookups.
  D. It can only be created on the Primary Key.
  
  **9. Multi-level indexing (like B+-Trees) is primarily used to solve which problem?**
  A. The problem of hash collisions.
  B. The problem that a single linear index file eventually becomes too large to store in main memory or search efficiently.
  C. The problem of duplicate data records.
  D. The problem of supporting foreign keys.
  
  **10. When you `INSERT` a new record into a table, what happens to the indexes?**
  A. Only the Primary Index needs to be updated.
  B. No indexes need to be updated until the database is reorganized.
  C. Every single index (Primary, Clustering, Secondary) defined on that table must be updated to include the new record.
  D. Only indexes that are "Dense" need to be updated.
  
  ---
- ---
- ---
- ---
- ---
- # **Answers and Explanations**
- ## Quiz 14 Answers
- **1. A ($S \leftouterjoin R$)**
  *   **Explanation:** A Right Outer Join keeps all rows from the right table ($S$). A Left Outer Join keeps all rows from the left table. If you swap the order of tables ($S$ on the left), a Left Join achieves the same result as the original Right Join (assuming attributes are matched correctly).
  
  **2. C (3 tuples)**
  *   **Explanation:** A Full Outer Join returns matches plus unmatched rows from both sides.
    *   ID 2 matches: (2, 2)
    *   ID 1 (from A) has no match: (1, NULL)
    *   ID 3 (from B) has no match: (NULL, 3)
    *   Total: 3 rows.
  
  **3. B (Total participation of R)**
  *   **Explanation:** A Left Outer Join usually adds rows for unmatched tuples in $R$. If the count is the same as the Inner Join, it means there were zero unmatched tuples in $R$. Every $R$ found a partner in $S$.
  
  **4. D (Right Outer Join)**
  *   **Explanation:** Outer joins are the only standard relational algebra operators that generate NULLs to pad missing data.
  
  **5. B ($(R \leftouterjoin S) \cup (R \rightouterjoin S)$)**
  *   **Explanation:** The Left Join gets all matching rows + unmatched Left. The Right Join gets all matching rows + unmatched Right. The Union combines them (and removes duplicates of the matching rows), resulting in Matches + Unmatched Left + Unmatched Right.
  
  **6. B (A table cannot have two separate PRIMARY KEY clauses...)**
  *   **Explanation:** To define a composite primary key (a key made of multiple columns), you must use a table-level constraint at the bottom, e.g., `PRIMARY KEY (StudentID, CourseID)`. You cannot flag two different columns as `PRIMARY KEY` individually on their own lines.
  
  **7. B (It must be a Primary Key or a Unique Key)**
  *   **Explanation:** Foreign keys must point to a specific, unique record in the parent table. Therefore, the referenced column must have a constraint enforcing uniqueness.
  
  **8. B (`CHAR(2)`)**
  *   **Explanation:** Since state codes are *always* exactly 2 characters, `CHAR` (fixed length) is slightly more performant than `VARCHAR` (variable length) because it doesn't need to store the length byte, and it ensures format consistency.
  
  **9. D (A column defined as UNIQUE is automatically the Primary Key)**
  *   **Explanation:** This is false. A table can have many Unique keys (candidate keys), but only one Primary Key. `UNIQUE` does not imply `PRIMARY KEY`.
  
  **10. A (`OrderDate DATE DEFAULT CURRENT_DATE`)**
  *   **Explanation:** The `DEFAULT` keyword is used in DDL to assign values when the user skips a column during an INSERT statement.
- ## Quiz 15 Answers
- ### **Answers and Explanations**
  
  **1. C (`DROP TABLE`)**
  *   **Explanation:** DDL commands define structure (`CREATE`, `ALTER`, `DROP`). DML commands manipulate data (`INSERT`, `UPDATE`, `DELETE`, `SELECT`).
  
  **2. B (Declarative)**
  *   **Explanation:** In SQL, you declare the desired result ("Give me all employees"), and the DBMS figures out the procedural steps (loops, indexes, file access) to get it.
  
  **3. A (The Query Optimizer)**
  *   **Explanation:** The optimizer looks at valid query plans and selects the one with the lowest estimated cost.
  
  **4. C (`CHECK`)**
  *   **Explanation:** A `CHECK` constraint validates data within a row based on a logical condition (e.g., `CHECK (EndDate > StartDate)`).
  
  **5. C (It allows you to specify a boolean condition...)**
  *   **Explanation:** `CHECK` ensures domain integrity by verifying values meet specific criteria. It generally cannot look at other tables (that's Referential Integrity/Foreign Keys).
  
  **6. A (1 row)**
  *   **Explanation:** `GROUP BY` collapses all rows sharing the grouping value into a single summary row.
  
  **7. B (`HAVING`)**
  *   **Explanation:** You are filtering based on the result of an aggregate (`SUM`). Aggregates are calculated *after* grouping, so the filter must happen in the `HAVING` clause. `WHERE` filters rows before they are summed.
  
  **8. C (The query is missing a `GROUP BY`...)**
  *   **Explanation:** If you mix a non-aggregated column (`DeptName`) with an aggregate (`COUNT`), you must group by the non-aggregated column. Otherwise, the database doesn't know how to map the single count to the multiple department names.
  
  **9. B (`FROM` $\to$ `WHERE` $\to$ `GROUP BY` $\to$ `HAVING` $\to$ `SELECT`)**
  *   **Explanation:** The database first finds the table (`FROM`), filters the rows (`WHERE`), buckets them (`GROUP BY`), filters the buckets (`HAVING`), and finally returns the requested columns (`SELECT`).
  
  **10. A (It is treating the table as a Set...)**
  *   **Explanation:** By default, SQL tables are Bags (Multisets), meaning they allow duplicates. `DISTINCT` forces Set behavior by eliminating duplicate tuples.
- ## Quiz 16 Answers
- ### **Answers and Explanations**
  
  **1. B (10)**
  *   **Explanation:** `COUNT(column_name)` counts non-NULL values. `COUNT(*)` counts rows. Since 5 rows are NULL, they are ignored.
  
  **2. C (`NULL IS NULL`)**
  *   **Explanation:** You cannot compare NULLs using `=`, `!=`, or `<>`. Those always evaluate to `UNKNOWN`. You must use `IS` or `IS NOT`.
  
  **3. C (It returns 0 rows)**
  *   **Explanation:** This is the "NOT IN + NULL" poison. `X NOT IN (1, NULL)` expands to `X!=1 AND X!=NULL`. Since `X!=NULL` is Unknown, the whole AND chain becomes Unknown, and no rows pass the filter.
  
  **4. B (50)**
  *   **Explanation:** A comma in the `FROM` clause without a join condition creates a Cartesian Product. $10 \text{ rows} \times 5 \text{ rows} = 50 \text{ rows}$.
  
  **5. B (Because logically, a Join is defined as a Cross Product followed by a Select...)**
  *   **Explanation:** Conceptually, to find matches, you combine everything against everything ($A \times B$) and then filter ($\sigma$) for the matches.
  
  **6. C (Invalid)**
  *   **Explanation:** You cannot use `WHERE` to filter the result of an aggregate (`SUM`). `WHERE` runs before aggregation. You must use `HAVING`.
  
  **7. B (`WHERE`)**
  *   **Explanation:** You want to filter the *input* rows (Seniors) before calculating the average. `HAVING` would filter the *result* (Majors with high GPAs).
  
  **8. C (`=`)**
  *   **Explanation:** The `=` operator expects a scalar (single value). If the subquery returns a list (set), `=` will fail. You need `IN` or `= ANY`.
  
  **9. C (`TRUE`)**
  *   **Explanation:** `EXISTS` checks for the **presence of a row**, not the **value** inside the row. A row containing `NULL` is still a row (it is not an empty set), so `EXISTS` returns True. This is why `NOT EXISTS` is safer than `NOT IN` when NULLs are involved.
  
  **10. C (After WHERE and GROUP BY, but before ORDER BY)**
  *   **Explanation:** This is why you cannot use an alias defined in `SELECT` inside the `WHERE` clause (the alias doesn't exist yet), but you *can* use it in `ORDER BY`.
- ## Quiz 17 Answers
- ### **Answers and Explanations**
  
  **1. D (`WHERE Salary > AVG(Salary)`)**
  *   **Explanation:** You cannot use an aggregate function directly in the `WHERE` clause. `WHERE` filters rows before aggregates are calculated. You must use a subquery (like option C) or `HAVING`.
  
  **2. B (No, standard SQL does not allow nesting...)**
  *   **Explanation:** An aggregate returns a single value from a group. You cannot aggregate an aggregate in one step. You would need a subquery (calculate average first in a subquery, then find the max of those averages in the outer query).
  
  **3. B (300)**
  *   **Explanation:** Aggregate functions (`SUM`, `AVG`, `MAX`, `MIN`, `COUNT(col)`) ignore NULLs. They do not let a NULL propagate (poison) the result like standard arithmetic (`+`, `-`) does.
  
  **4. C (Yes, they return the first and last values alphabetically)**
  *   **Explanation:** `MIN` and `MAX` work on almost all data types. On strings, `MIN` is 'A' (start of alphabet) and `MAX` is 'Z' (end).
  
  **5. B (A Derived Table)**
  *   **Explanation:** When a subquery is in the `FROM` clause, it acts as a temporary table for the duration of the query.
  
  **6. B (It must return exactly one row and one column)**
  *   **Explanation:** The `SELECT` list expects single values to put into cells. If the subquery returns multiple rows or columns, the database doesn't know how to squeeze them into a single cell, and it throws an error.
  
  **7. C (`ALL`)**
  *   **Explanation:** `> ALL (...)` means "Greater than the maximum value in the list." `> ANY (...)` means "Greater than the minimum value in the list."
  
  **8. B (`NULL`)**
  *   **Explanation:** If a scalar subquery (in the SELECT list) finds no match, it returns `NULL` rather than an empty set or error.
  
  **9. A (Because the database needs a name to refer to...)**
  *   **Explanation:** Just like a real table needs a name so you can say `Table.Column`, a derived table needs an alias so the outer query can reference its columns (e.g., `T.Column`).
  
  **10. B (`WHERE Salary > (SELECT MIN(Salary)...)`)**
  *   **Explanation:** If you are richer than *ANY* person in Sales, you only need to be richer than the *poorest* person in Sales. If you are richer than the poorest person, you have fulfilled the "Any" condition.
- ## Quiz 18 Answers
- ### **Answers and Explanations**
  
  **1. B (`JobTitle` is in the SELECT list but not...)**
  *   **Explanation:** The Single-Value Rule. If you group by Dept, you have one row per Dept. If that Dept has 5 different JobTitles, the database doesn't know which one to display unless you aggregate them or add JobTitle to the grouping bucket.
  
  **2. C (Each unique combination...)**
  *   **Explanation:** `GROUP BY A, B` creates a bucket for every unique pair. {NY, Engineer} is a different bucket from {NY, Manager} and {LA, Engineer}.
  
  **3. C (`COUNT(DISTINCT JobTitle)`)**
  *   **Explanation:** You want to remove duplicates *inside* the count function before counting. Option A counts the total employees with non-null titles.
  
  **4. B (Groups are filtered by HAVING)**
  *   **Explanation:** The pipeline is: `FROM` $\to$ `WHERE` $\to$ `GROUP BY` $\to$ `HAVING`.
  
  **5. B (Yes, but it treats the entire table as one single group)**
  *   **Explanation:** While rare, `SELECT count(*) FROM Table HAVING count(*) > 10` is valid. It returns the count if it's > 10, or nothing if it's not. It operates on the set as a whole.
  
  **6. C (`WHERE` Status='FT' ... `HAVING` AVG(Salary) > 50000)**
  *   **Explanation:**
  *   Step 1: Filter rows (`WHERE`) to get only Full-Time people.
  *   Step 2: Group them.
  *   Step 3: Calculate Average and filter the group (`HAVING`).
  
  **7. B (The inner query references a column from the outer query)**
  *   **Explanation:** This "correlation" (link) forces the database to re-evaluate the inner query for every new row of the outer query.
  
  **8. B (N times)**
  *   **Explanation:** For every single row in the outer table ($N$), the inner query runs one full cycle. This is why correlated subqueries can be slow on large tables compared to joins.
  
  **9. C (The inner WHERE clause uses E.DeptID...)**
  *   **Explanation:** The presence of `E.DeptID` inside the parentheses binds the inner logic to the current row being processed in the outer loop.
  
  **10. A (The inner query can see the outer query's columns...)**
  *   **Explanation:** Scope drills down. A child (inner query) can see variables defined in the parent (outer query), but the parent cannot see variables defined inside the child (unless the child returns them as a result set).
- ## Quiz 19 Answers
- ### **Answers and Explanations**
  
  **1. B (Select Students where there does not exist a Biology course that the student has not taken)**
  *   **Explanation:** This is the classic logical equivalent of division. If there is no course you *haven't* taken, you must have taken *all* of them.
  
  **2. B (`HAVING COUNT(DISTINCT CourseID) = ...`)**
  *   **Explanation:** You group the student's taken courses, count how many unique Biology courses are in that group, and compare it to the total number of Biology courses that exist. If the counts match, they took them all.
  
  **3. C (No, it must be simulated...)**
  *   **Explanation:** Unlike Relational Algebra (which has $\div$), SQL has never implemented a direct division operator.
  
  **4. C (It finds items in A that are missing from B)**
  *   **Explanation:** This is used in the logic: "Find courses required (A) `EXCEPT` courses taken (B)." If the result is empty, the student has met all requirements.
  
  **5. A (`JOIN`)**
  *   **Explanation:** "Decorrelation" is the process of flattening a nested loop (correlated query) into a set-based operation (Join).
  
  **6. C (Correlated subqueries execute the inner logic N times...)**
  *   **Explanation:** This is the algorithmic cost. A nested loop over 1 million rows is slow. A Join optimization algorithm handles 1 million rows much more efficiently.
  
  **7. B (Update Anomalies)**
  *   **Explanation:** If you store a student's address in 5 different tables, and they move, you have to update 5 tables. If you miss one, the database disagrees with itself (inconsistency).
  
  **8. C (The deliberate introduction of redundancy...)**
  *   **Explanation:** Denormalization is a conscious design choice used often in Data Warehousing to avoid expensive joins by pre-calculating or pre-joining data.
  
  **9. C (It can slow down reads because joining tables is computationally expensive)**
  *   **Explanation:** This is the trade-off. Normalization makes *writes* safer and faster (less data to write), but makes *reads* slower (need to glue data back together).
  
  **10. B (Redundant/Denormalized)**
  *   **Explanation:** "TotalSales" can be calculated by summing the "Orders" table. Storing it physically in "Customers" is redundant. It improves read speed (don't need to sum every time) but risks inconsistency (if the update logic fails).
- ## Quiz 20 Answers
- ### **Answers and Explanations**
  
  **1. C (You can prove an FD is universally true by looking at a single snapshot...)**
  *   **Explanation:** You can only prove an FD is *false* (by finding a counter-example) using data. You cannot prove it is *true* for all future data just by looking at current data; FDs must come from business rules.
  
  **2. B (Augmentation)**
  *   **Explanation:** Augmentation allows you to add the same attributes to both sides of an existing dependency.
  
  **3. A (The right-hand side is a subset of the left-hand side)**
  *   **Explanation:** A trivial dependency conveys no new information. Knowing `{Name, Age}` obviously allows you to know `{Name}`.
  
  **4. B (`{A, B, C}`)**
  *   **Explanation:** $A$ determines itself. $A$ determines $B$ (given). Since $A \rightarrow B$ and $B \rightarrow C$, then $A \rightarrow C$ (transitivity). So $A$ reaches $A, B, \text{and } C$.
  
  **5. C (A Superkey)**
  *   **Explanation:** A superkey determines all attributes. A *Candidate Key* is a minimal superkey. Since we don't know if $X$ is minimal, "Superkey" is the most accurate broad definition.
  
  **6. A (Calculate $X^+$ using all FDs except $X \rightarrow Y$...)**
  *   **Explanation:** If you can reach $Y$ starting from $X$ using *only the other rules*, then the rule $X \rightarrow Y$ is unnecessary logic and can be removed.
  
  **7. D (Every FD must be in Third Normal Form)**
  *   **Explanation:** Minimal Cover is a property of the *set of rules*, not the normalization status of the table. You calculate the minimal cover *before* or *during* normalization.
  
  **8. B (Leave them as is...)**
  *   **Explanation:** The Minimal Cover algorithm requires **Singleton RHS** (e.g., $A \rightarrow B$, not $A \rightarrow BC$). Standard notation often groups them, but the strict definition of Minimal Cover usually prefers split RHS or at least permits it. Merging them (A) is the opposite of the standard first step (splitting RHS).
  
  **9. B (Extraneous)**
  *   **Explanation:** If $A$ alone can do the job ($A \rightarrow C$), then adding $B$ to the left side ($\{A, B\} \rightarrow C$) adds no value. $B$ is extraneous.
  
  **10. C ($F$ covers $G$ AND $G$ covers $F$)**
  *   **Explanation:** Equivalence means they generate the exact same closure ($F^+ = G^+$). Logically, this means every rule in one set can be proven using the rules in the other.
- ## Quiz 21 Answers
- ### **Answers and Explanations**
  
  **1. B (No, the dataset might be too small...)**
  *   **Explanation:** Data can only **disprove** an FD. Just because a rule holds for current data doesn't mean it's a business rule. There might be a zip code that crosses state lines that just hasn't been entered into the database yet.
  
  **2. C (Two tuples have the same value for A but different values for B)**
  *   **Explanation:** The definition of $A \rightarrow B$ is: "If $t1.A = t2.A$, then $t1.B$ MUST equal $t2.B$." Option C violates this condition.
  
  **3. C (The database designer/domain expert...)**
  *   **Explanation:** FDs represent semantics (meaning). Only the human who understands the business (e.g., "One student has only one advisor") can define them.
  
  **4. B ($X \rightarrow Y$)**
  *   **Explanation:** By definition, a Key uniquely identifies the entire tuple. Therefore, the key ($X$) functionally determines *every* other attribute ($Y$) in the row.
  
  **5. A (If the axioms derive an FD... it is guaranteed to be valid)**
  *   **Explanation:** **Soundness** = "No Lies" (Generated rules are true). **Completeness** = "No Missing Info" (All true rules are generated).
  
  **6. B (If $A \rightarrow B$, then $AC \rightarrow BC$)**
  *   **Explanation:** Augmentation allows you to add the same attribute ($C$) to both sides of a valid dependency.
  
  **7. B (`{Name, SSN}` $\rightarrow$ `Name`)**
  *   **Explanation:** Reflexivity states that if $Y$ is a subset of $X$, then $X \rightarrow Y$. Since `Name` is part of `{Name, SSN}`, the LHS determines the RHS automatically. This is a trivial FD.
  
  **8. B ($A \rightarrow BC$)**
  *   **Explanation:** The Union rule allows you to combine the RHS of two dependencies if they share the same LHS.
  
  **9. C (The rules are sufficient to derive every FD...)**
  *   **Explanation:** Completeness means the inference engine won't miss any logically implied dependencies.
  
  **10. D (Both A and B are valid approaches)**
  *   **Explanation:** You can verify an FD by checking the data (looking for violations/counter-examples, Option A) OR by proving it logically from existing rules (Option B). Both are valid methods of verification, though B is preferred for design theory.
- ## Quiz 22 Answers
- ### **Answers and Explanations**
  
  **1. A ($A \to C$)**
  *   **Explanation:** The key is $\{A, B\}$. $C$ is a non-prime attribute.
  *   $A \to C$ means $C$ depends only on $A$ (part of the key).
  *   This is a **Partial Functional Dependency**, which is a violation of 2NF.
  
  **2. B (The relation is in 2NF but violates 3NF due to a transitive dependency)**
  *   **Explanation:**
  *   Key is $X$.
  *   $X \to Y$ is fine (Key determines non-key).
  *   $Y \to Z$ means a non-key ($Y$) determines another non-key ($Z$). This is **Transitive**.
  *   It is in 2NF because the key ($X$) is a single attribute, so partial dependencies are impossible.
  
  **3. B (If $Y$ is a Prime Attribute)**
  *   **Explanation:** The strict definition of BCNF requires $X$ to be a superkey. The "weaker" definition of 3NF allows the dependency if $X$ is a superkey OR if **$Y$ is part of a candidate key (Prime)**.
  
  **4. C (No, because the intersection {B} is not a key...)**
  *   **Explanation:**
  *   Intersection = $\{B\}$.
  *   Does $B \to \{A, B\}$? No (no rule $B \to A$).
  *   Does $B \to \{B, C\}$? No (no rule $B \to C$).
  *   Since the intersection isn't a key for either, the join is Lossy.
  
  **5. A ($(R1 \cap R2) \to R1$ OR $(R1 \cap R2) \to R2$)**
  *   **Explanation:** This is the mathematical definition of the Lossless Join test. The shared column(s) must be a unique identifier (Superkey) for at least one of the resulting tables to ensure clean re-joining.
  
  **6. B (You create Spurious Tuples)**
  *   **Explanation:** "Lossy" is a misleading term. You don't actually lose the original data; you "gain" noise. Joining on a non-key attribute creates false associations (garbage rows) that weren't in the original data.
  
  **7. A (Yes, all FDs are covered...)**
  *   **Explanation:**
  *   $A \to B$ is in $R1$.
  *   $B \to C$ is in $R2$.
  *   $C \to D$ is in $R3$.
  *   Since every rule lives happily inside one of the tables, it is dependency-preserving.
  
  **8. C (No, because A and B are not in the same table)**
  *   **Explanation:** To check/enforce $A \to B$, the DBMS needs to see $A$ and $B$ in the same row. If they are in different tables ($R1, R2$), the constraint cannot be enforced without an expensive join. Therefore, the dependency is not preserved.
  
  **9. B (2NF - Partial Dependency)**
  *   **Explanation:** The Key is composite `{StudentID, CourseID}`. The rule `CourseID` $\to$ `ProfessorName` relies on only *part* of that key (`CourseID`). This violates 2NF.
  
  **10. A (2NF)**
  *   **Explanation:** Partial Dependencies (the violation of 2NF) can **only** occur if the Primary Key is composite (made of multiple columns). If the key is a single column, it's impossible to depend on "part" of it. Therefore, all single-key tables are automatically 2NF.
- ## Quiz 23 Answers
- ### **Answers and Explanations**
  
  **1. C (The relation violates BCNF because $B \to C$ holds, but $B$ is not a superkey)**
  *   **Explanation:** Key is $A$. $A \to B$ is fine (Superkey $\to$ ...). However, $B \to C$ is a transitive dependency. $B$ determines $C$, but $B$ is not a superkey (it cannot determine A). Therefore, BCNF is violated.
  
  **2. D ($W$)**
  *   **Explanation:**
  *   $X \to Y \to Z \to X$. This cycle means $X, Y, Z$ are all equivalent.
  *   $Z \to W$ means if you have $Z$ (or $X$ or $Y$), you have $W$.
  *   Therefore, $X, Y, \text{and } Z$ are all keys. $W$ is determined by them but determines nothing itself.
  
  **3. B (No, because $C \to B$ exists, and $C$ is not a superkey)**
  *   **Explanation:** $C$ is part of a key ($AC$), but it is not a key by itself ($C$ cannot determine $A$). Therefore, the rule $C \to B$ violates the BCNF requirement that the LHS must be a superkey. (Note: This relation *is* in 3NF).
  
  **4. A (Yes, because $C$ is the common attribute and $C \to \{D, E\}$...)**
  *   **Explanation:** Intersection is $\{C\}$.
  *   Does $C \to R1 (A, B, C)$? No.
  *   Does $C \to R2 (C, D, E)$? We have $C \to D$ and $D \to E$, so by transitivity $C \to \{D, E\}$.
  *   Since $C$ determines all attributes in $R2$, it is a key for $R2$. Decomposition is lossless.
  
  **5. C ($R1 \cap R2 \to R1$)**
  *   **Explanation:** The intersection must be a superkey for *at least one* of the tables ($R1$ OR $R2$).
  
  **6. A (Yes, because intersection $A$ determines $R1$)**
  *   **Explanation:** Intersection is $\{A\}$. Given $A \to B$, $A$ is a superkey for table $R1(A, B)$. Lossless.
  
  **7. B (Insertion is very fast because new records are placed at the end)**
  *   **Explanation:** Heap files are unordered "piles" of data. No sorting is maintained, so you just dump new data in the last block.
  
  **8. B (It must locate the correct physical position... making it expensive)**
  *   **Explanation:** Sorted files maintain physical order. Inserting 50 into a list `10, 20, ..., 100` requires moving everything after 20 to make a hole.
  
  **9. B (A flag stored with a record...)**
  *   **Explanation:** physically deleting and shifting data is slow. Toggling a "deleted" bit from 0 to 1 is instant. The system filters these out during reads and reclaims space during maintenance.
  
  **10. C (Ordered File)**
  *   **Explanation:** Binary search requires sorted data. You cannot binary search a heap or a hash bucket.
- ## Quiz 24 Answers
- ### **Answers and Explanations**
  
  **1. C (Hash File)**
  *   **Explanation:** Hashing calculates the address directly using a mathematical function. It is $O(1)$ access. Sorted files require Binary Search ($O(\log N)$), and Heap files require Linear Search ($O(N)$).
  
  **2. B (Because insertion is extremely fast...)**
  *   **Explanation:** Heap files are unordered. You place new data at the very end of the file. There is no overhead for sorting or calculating hash buckets.
  
  **3. C (An overflow bucket is created...)**
  *   **Explanation:** In static hashing, the number of primary buckets is fixed. If one fills up (Collision), the standard solution is to create an overflow chain (linked list) off that bucket.
  
  **4. B (Range Query: `ID > 100...`)**
  *   **Explanation:** Hash functions randomize data locations. `101` might be in Bucket 1, and `102` in Bucket 99. There is no physical order, so the database cannot scan a range; it must scan the entire file (linear scan).
  
  **5. B (The search condition must be on the Ordering Field)**
  *   **Explanation:** Binary search relies on the physical order of data. If the file is sorted by `ID`, but you search by `Name`, the physical order doesn't help you, and you cannot use Binary Search.
  
  **6. C (Contiguous Allocation)**
  *   **Explanation:** Contiguous blocks (e.g., 1, 2, 3, 4) are fast to read (head doesn't move), but if block 5 is occupied by another file, you cannot grow the file without moving the whole thing.
  
  **7. A (It stores all block pointers in a single "Index Block"...)**
  *   **Explanation:** Indexed allocation gives you a "map" of the file. To get to Block 50, you just look at the 50th entry in the index block. Linked allocation forces you to "walk the chain."
  
  **8. C (It supports only sequential access efficiently...)**
  *   **Explanation:** In Linked allocation, Block 1 points to 2, which points to 3. You cannot jump to 50 because you don't know where it is until you read Block 49.
  
  **9. B (Because physical deletion would require shifting...)**
  *   **Explanation:** In a packed file, removing a record leaves a hole. Closing that hole requires moving every record that comes after it up one slot. It is much faster to just flag it as "deleted" and skip it during reads.
  
  **10. D (Tuple ID)**
  *   **Explanation:** Physical disk addresses are based on hardware geometry: Cylinder, Track, and Sector/Block. A Tuple ID is a logical concept inside the database software, not a hardware address.
- ## Quiz 25 Answers
- ### **Answers and Explanations**
  
  **1. C (Clustering Index)**
  *   **Explanation:** A Clustering Index is defined as an index on the **Ordering Field** where that field is **Non-Unique** (Non-Key).
  
  **2. C (Secondary Index)**
  *   **Explanation:** Any index on a field that does **not** determine the physical order of the file is a Secondary Index, regardless of whether that field is unique or not.
  
  **3. B (Ordering Field + Key Field)**
  *   **Explanation:** A Primary Index requires the file to be sorted by the indexing field, and that field must be a unique Key.
  
  **4. C (No, because a file can only be physically sorted by one field...)**
  *   **Explanation:** Since Primary and Clustering indexes both rely on the physical order of the data on the disk, and you can't sort a file two different ways simultaneously, you can have at most one of these types per table.
  
  **5. C (Secondary Index)**
  *   **Explanation:** Because the underlying data file is not sorted by the secondary key, you cannot predict where a record is. Therefore, you must have a pointer (index entry) for *every single record* to find them all.
  
  **6. B (Because Primary Indexes are Sparse...)**
  *   **Explanation:** If a block holds 100 records, a Primary Index needs only 1 pointer (to the start of the block). A Secondary Index needs 100 pointers (one for each record). Thus, Secondary indexes are much larger.
  
  **7. B (It points to the Anchor Record...)**
  *   **Explanation:** In a sorted file, the index only tells you "The records starting with Key 100 begin in Block 5." You go to Block 5 and search inside it.
  
  **8. B (It must be Dense)**
  *   **Explanation:** Even though `EmployeeID` is unique, the file is sorted by `SSN`. Therefore, `EmployeeID` is a secondary access path. Since the file isn't sorted by ID, the index must be dense to locate every ID.
  
  **9. C (It points to a block of pointers...)**
  *   **Explanation:** For non-unique secondary keys, a single value maps to multiple records scattered across the disk. To keep the index structure clean, the entry points to a separate bucket that lists all the locations.
  
  **10. A (Indexes speed up reads but slow down writes)**
  *   **Explanation:** Indexes are great for finding data. However, every time you `INSERT` or `DELETE` a row, the DBMS must update the table *and* update every single index attached to that table. This adds overhead to write operations.
- ## Quiz 26 Answers
- ### **Answers and Explanations**
  
  **1. B (Because the underlying data file is sorted...)**
  *   **Explanation:** If I know Block 1 starts with ID 10 and Block 2 starts with ID 20, I know that ID 15 *must* be in Block 1. I don't need a specific pointer for 15. This logic only works if the file is sorted.
  
  **2. C (The first record in a data block...)**
  *   **Explanation:** The anchor tells the index the "starting value" of the block.
  
  **3. B (1,000)**
  *   **Explanation:** A Sparse Primary Index has one entry per **Block**, not per record.
  
  **4. B (Because the data file is not sorted by the secondary key...)**
  *   **Explanation:** While the index is sorted, the data is scattered. Reading 100 consecutive index entries might require the disk arm to move to 100 completely different locations on the disk to fetch the actual data rows. This "thrashing" is very slow.
  
  **5. C (Clustering Index or Primary Index)**
  *   **Explanation:** Because the data is physically sorted, once you find the start of the range, you can just read the next $N$ blocks sequentially. This is the fastest possible disk access method.
  
  **6. B (The Primary Index...)**
  *   **Explanation:** Primary Indexes are sparse (smaller file size). Binary searching a smaller file takes fewer steps (and fewer disk block loads) than binary searching a larger, dense Secondary Index file.
  
  **7. B (It stores "Sales" once, and the pointer points to a bucket...)**
  *   **Explanation:** This is the standard implementation to keep the index structure clean. Some implementations do use Option A, but Option B is generally preferred to handle high duplicates.
  
  **8. A (It requires more storage space and imposes a higher maintenance cost...)**
  *   **Explanation:** If you have 1 million rows, a Dense index has 1 million entries. A Sparse index might only have 10,000 entries (assuming 100 records/block). The Dense index is much heavier to update and store.
  
  **9. B (The problem that a single linear index file eventually becomes too large...)**
  *   **Explanation:** Searching a massive flat file on disk is slow ($O(\log_2 N)$ disk accesses). By building a tree (indexing the index), we reduce the cost to $O(\log_{fanout} N)$, which is much shallower and faster.
  
  **10. C (Every single index... must be updated)**
  *   **Explanation:** This is the "write penalty" of indexing. Data must be consistent. If you add a row, you must update the pointers in every index that references that row.