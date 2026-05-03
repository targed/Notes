## I. Database and SQL Basics (Slides 48–57)
Before you can hack a database, you must understand how it organizes data.
*   **SQL (Structured Query Language):** The standard language used to communicate with Relational Databases (like MySQL, PostgreSQL, Oracle). 
*   **DDL vs. DML:**
  *   **DDL (Data Definition Language):** Commands used to define the structure (e.g., `CREATE TABLE`, `DROP TABLE`).
  *   **DML (Data Manipulation Language):** Commands used to query or modify the data inside the tables (e.g., `SELECT`, `INSERT`, `UPDATE`).
*   **Table Structure:**
  *   **Schema:** The definition of the table (Table Name + Attribute Names). Example: `Product(PName, Price, Category)`.
  *   **Tuple:** A single row or record in the table. 
  *   **Key:** A unique identifier for a row (e.g., a User ID).
  *   *Note:* A database table is mathematically a *set*. It is unordered; there is no "first" or "last" row unless you explicitly sort it.

**The Basic SQL Query:**
```sql
SELECT PName, Price    -- The Attributes you want to see (Projection)
FROM Product           -- The Table you are looking in
WHERE Price > 100;     -- The Filter (Selection)
```
- ## II. The Mechanics of SQL Injection (Slides 58–66)
  SQL Injection happens when a web application takes untrusted user input and concatenates (glues) it directly into a backend database query.
- ### 1. The Flawed Code (Slide 63)
  Imagine a simple login script:
  ```sql
  SELECT * FROM Users WHERE user=' $user_input ' AND pwd=' $pwd_input ';
  ```
  *   If the user behaves, they type `Alice` and `password123`. The query is safe.
  *   **The Golden Rule Violated:** The developer *trusted* the input and didn't sanitize it.
- ### 2. The Authentication Bypass (Slide 65)
  The attacker types the following string into the Username field: `' or 1=1 --`
  *   **The Quote (`'`):** This breaks out of the developer's intended string.
  *   **The Logic (`or 1=1`):** Since 1 always equals 1, this makes the `WHERE` clause globally True for every row in the database.
  *   **The Comment (`--`):** In SQL, `--` means "ignore everything after this." It comments out the password check entirely!
  *   **The Resulting Query executed by the server:**
    ```sql
    SELECT * FROM Users WHERE user='' or 1=1 --' AND pwd='...';
    ```
  *   **Impact:** The database returns the first record in the `Users` table (which is usually the Admin). The attacker logs in as Admin without knowing the password.
- ### 3. The Destructive Attack (Slide 66)
  If the database allows multiple statements per execution, the attacker can input:
  `'; DROP TABLE Users --`
  *   **The Semicolon (`;`):** Tells the SQL engine "this command is finished, prepare for a new command."
  *   **Impact:** The server executes the login check, and then completely deletes the `Users` table, destroying the application (as joked about in the famous "Little Bobby Tables" XKCD comic on Slide 47).
- ## III. Data Exfiltration: The UNION Attack (Slides 67–69)
  Attackers don't just want to log in; they want to steal private data (like credit cards). 
  *   **The `UNION` Operator:** In SQL, `UNION` allows you to combine the results of two completely different `SELECT` queries into one output table.
  *   **The Attack:** 
    1.  The victim site has a search box: "View pizza order history for Month: [Input]".
    2.  The attacker inputs: `0 AND 1=0 UNION SELECT name, CC_num, exp_mon, exp_year FROM creditcards`
    3.  The first part (`0 AND 1=0`) purposely returns nothing. 
    4.  The second part dumps the entire `creditcards` table to the web page!
  *   **Impact:** The attacker sees thousands of credit card numbers printed on the web page instead of pizza orders.
- ## IV. Real-World Impact & Prevention (Slides 60, 70)
  *   **CardSystems Attack (2005):** A prime example. A payment processor was hit with an SQLi attack. Attackers stole 263,000 credit cards and exposed 43 million. They were put out of business. *Crucial failure: The credit cards were stored unencrypted in the DB!*
  *   **April 2008 Mass Attack (Slide 61):** Automated SQLi bots searched Google for vulnerable `.asp` sites, injecting malicious JavaScript links into their databases (a blend of SQLi leading to a Stored XSS attack).
- ### The Ultimate Defense (Slide 70)
  **Never build SQL commands yourself using string concatenation.**
  *   **Parameterized Queries (Prepared Statements):** The database engine compiles the SQL query structure *before* inserting the user input. The input is strictly treated as a literal string (data), not as executable code. If the user types `' or 1=1 --`, the database literally looks for a user whose actual name is `' or 1=1 --`.
  *   **Object-Relational Mapping (ORM):** Frameworks (like Hibernate or Entity Framework) handle the database translation safely behind the scenes, preventing developers from writing raw SQL.
  
  ---
- ### Study Questions for Part 4
  1. **The Mechanics:** In the classic SQL injection payload `' OR 1=1 --`, what is the specific purpose of the `--` characters at the end of the string?
  2. **Data Exfiltration:** If an attacker wants to use an SQL injection vulnerability in a "Search" box to steal credit card numbers from a completely different table, what specific SQL operator do they use to combine the queries?
  3. **The Defense:** Why do "Parameterized Queries" successfully stop SQL injection attacks even if the user inputs malicious commands like `DROP TABLE`?
  
  ---