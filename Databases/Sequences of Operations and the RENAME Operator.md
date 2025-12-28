#### **Combining Operations**

Since the output of any relational algebra operation is always another relation, we can use the result of one operation as the input to the next. This allows us to "chain" or "nest" operations to express complex queries.

**Example: "How would you find the names of the third-year students?"**

This query requires two steps:
1.  First, select the rows for third-year students.
2.  Then, from those rows, project only the name column.

This can be written in two ways:

1.  **As a single in-line expression (nested):**
  *   π<sub>sname</sub> (σ<sub>year=3</sub>(S))
  *   This is evaluated from the inside out: first, the `SELECT` operation (σ) is performed on the `S` table, producing an intermediate table of all third-year students. Then, the `PROJECT` operation (π) is performed on that intermediate result.

2.  **As a sequence of assignments:**
  *   THIRD_YEAR_STUDENTS ← σ<sub>year=3</sub>(S)
  *   RESULT ← π<sub>sname</sub>(THIRD_YEAR_STUDENTS)
  *   This approach is often clearer for very complex queries, as it breaks the problem down into named, intermediate steps.
- #### **The RENAME (ρ) Operation**
  
  The `RENAME` operation is a simple but essential utility. It doesn't change the data within a relation, but it changes the "metadata"—either the name of the relation itself or the names of its attributes.
  
  *   **Notation:** ρ<sub>S(B₁, B₂, ..., Bₙ)</sub>(R)
    *   `ρ` (rho) is the symbol for the RENAME operation.
    *   `R` is the input relation.
    *   `S` is the new name for the relation.
    *   `(B₁, B₂, ..., Bₙ)` is the optional new list of attribute names.
  
  **Why is RENAME needed?**
  
  1.  **Clarity:** It allows you to give meaningful names to the results of intermediate operations.
  2.  **Avoiding Ambiguity in Joins:** It is absolutely critical when you need to join a table with itself (a self-join) or when you are joining two tables that have columns with the same name. As we saw in the `CARTESIAN PRODUCT` example, if the result has two columns named `tid`, the result is undefined. We must use `RENAME` to give them distinct names (e.g., `tid1` and `tid2`) before we can proceed.
  
  **Example:**
  Continuing the previous example, we can rename the final result:
  *   RESULT ← π<sub>FNAME, LNAME, SALARY</sub> (σ<sub>DNO=5</sub>(EMPLOYEE))
  *   RENAMED_RESULT ← ρ<sub>R(First_name, Last_name, Salary)</sub>(RESULT)
    *   This takes the `RESULT` relation and renames it to `R`, and also renames the attributes from `FNAME`, `LNAME`, `SALARY` to `First_name`, `Last_name`, `Salary`.