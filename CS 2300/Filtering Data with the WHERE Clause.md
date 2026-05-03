### **Filtering Data with the WHERE Clause**

The `WHERE` clause is responsible for the **Selection** operation ($\sigma$) from Relational Algebra. It filters the *rows* (tuples) of the table, returning only those that satisfy a specific condition.

**1. Basic Comparison Operators (Slide 6)**
You can compare attributes against values or other attributes using standard operators:
*   `=`: Equal to
*   `<`, `<=`: Less than, Less than or equal to
*   `>`, `>=`: Greater than, Greater than or equal to
*   `<>`: Not equal to (standard SQL)

**2. Boolean Logic (Slide 8)**
You can combine multiple conditions using:
*   **`AND`**: Both conditions must be true.
*   **`OR`**: At least one condition must be true.
*   **`NOT`**: Inverts the condition.

**3. Handling NULL Values (Slide 8)**
This is a critical concept in SQL. You **cannot** compare NULLs using standard operators like `=` or `<>`.
*   `WHERE Grade = NULL` will **fail** or return nothing (technically "Unknown").
*   **Correct Usage:** You must use the special keywords:
  *   `IS NULL`
  *   `IS NOT NULL`
*   *Example:* `WHERE StudentDept IS NOT NULL` ensures you only get students assigned to a department.

**4. Set and Range Comparisons (Slides 9-10)**
SQL provides shortcuts for common comparisons:
*   **`IN` / `NOT IN`**: Checks if a value matches any value in a specific list.
  *   *Example:* `WHERE StudentID IN (1123, 2245)` is cleaner than writing `WHERE StudentID = 1123 OR StudentID = 2245`.
*   **`BETWEEN`**: Checks if a value falls within a specific range (inclusive).
  *   *Example:* `WHERE age BETWEEN 20 AND 30`.

**5. Pattern Matching with LIKE (Slide 11)**
For string attributes, you often need to search for patterns rather than exact matches.
*   **Operator:** `LIKE`
*   **Wildcards:**
  *   **`%`**: Represents **zero or more** arbitrary characters.
  *   **`_`**: Represents exactly **one** arbitrary character.
*   **Example:** `WHERE StudentName LIKE 'S%n%a'`
  *   This would match "Sonya" (Starts with S, has 'n' somewhere, ends with 'a').
  *   It would also match "Santa Barbara" (S...n...a).