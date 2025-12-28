#### **1. The Five Aggregate Operators**
SQL provides five standard functions to summarize a column of data:
1.  **`COUNT(*)`**: Returns the total number of rows (including duplicates and NULLs).
2.  **`COUNT([DISTINCT] A)`**: Returns the number of non-null values in attribute A.
3.  **`SUM`**: Returns the total sum of a numeric attribute.
4.  **`AVG`**: Returns the average value of a numeric attribute.
5.  **`MAX` / `MIN`**: Returns the maximum or minimum value.

**Key Rule:** Aggregate operators ignore `NULL` values (except for `COUNT(*)`).
- #### **2. The "Oldest Sailor" Problem**
  A very common mistake is trying to mix aggregate values with raw row values in a single `SELECT` clause without grouping.
  
  *   **The Mistake:** `SELECT S.sname, MAX(S.age) FROM Sailors S;`
    *   **Why it fails:** The database calculates a single scalar number for `MAX(age)`. However, `S.sname` represents a list of many names. The database doesn't know how to match one number to one specific name in this context.
  *   **The Solution:** You must use a **Nested Query** (which we will cover in detail next) or multiple steps.
    *   *Correct approach:* Find the sailor WHERE their age is equal to the result of `(SELECT MAX(age)...)`.
- #### **3. Grouping Data (`GROUP BY`)**
  Often, we don't want the average age of *all* sailors; we want the average age *per specific category* (e.g., for each rating level).
  
  *   **`GROUP BY rating`**: This instructs the database to partition the rows into buckets, where every row in a bucket has the same `rating`.
  *   **Evaluation:** The aggregate function (e.g., `AVG(age)`) is then applied **separately** to each bucket.
  *   **Result:** The query returns **one row per group**, not one row per original tuple.
- #### **4. Filtering Groups (`HAVING`)**
  Just as `WHERE` filters rows, `HAVING` filters **groups**.
  
  *   **Rule:** `WHERE` applies to raw data *before* grouping. `HAVING` applies to aggregated data *after* grouping.
  *   **Example:** `GROUP BY rating HAVING COUNT(*) > 2`
    *   This groups sailors by rating, counts how many sailors are in each group, and discards any group that has 2 or fewer sailors.
- #### **5. Updated Evaluation Order**
  With these new clauses, the execution order of a query becomes:
  
  1.  **`FROM`**: Compute cross product of tables.
  2.  **`WHERE`**: Filter individual **rows** (tuples).
  3.  **`GROUP BY`**: Partition the remaining rows into **groups**.
  4.  **`HAVING`**: Filter the **groups** based on aggregate conditions.
  5.  **`SELECT`**: Generate the output columns (one per group).
- #### **6. Visual Walkthrough and Constraints**
  A visual trace of the query:
  `SELECT AVG(age) FROM Sailors WHERE age < 50 GROUP BY rating HAVING COUNT(*) > 2`
  
  *   **Step 1 (WHERE):** Eliminate anyone older than 50.
  *   **Step 2 (GROUP BY):** Sort the remaining sailors into piles based on their rating (e.g., a '7' pile, an '8' pile, a '10' pile).
  *   **Step 3 (HAVING):** Count the size of each pile. If a pile has 2 or fewer sailors, throw the whole pile away.
  *   **Step 4 (SELECT):** Calculate the average age of the surviving piles.
  
  **Critical Restriction:**
  If you use `GROUP BY`, the `SELECT` clause (and the `ORDER BY` clause) can **only** contain:
  1.  Attributes present in the `GROUP BY` list.
  2.  Aggregates (e.g., `SUM`, `COUNT`).
  *   **Why?** You cannot `SELECT sname` if you grouped by `rating`, because there might be five different names in the 'rating 10' group, and the database can only output one row per group.