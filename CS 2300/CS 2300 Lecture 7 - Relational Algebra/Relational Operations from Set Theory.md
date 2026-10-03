### **Relational Operations from Set Theory**
  
  These operations—`UNION`, `INTERSECTION`, and `SET DIFFERENCE`—treat relations as sets of tuples. For these operations to be valid, the two relations involved must be **union-compatible** (or type-compatible).
  
  **Union Compatibility:**
  Two relations, `R` and `S`, are union-compatible if:
  1.  They have the **same number of attributes** (the same degree).
  2.  The **domains** of their corresponding attributes are the same. For example, the first attribute of `R` must have the same data type as the first attribute of `S`, the second must match the second, and so on.
  
  ---
#### **The UNION (∪) Operation**
  
  The `UNION` operation combines the tuples from two compatible relations into a single relation.
  
  *   **Notation:** R ∪ S
  *   **Result:** The resulting relation contains all tuples that appear in `R`, or in `S`, or in both.
  *   **Duplicate Elimination:** As with any set operation, any duplicate tuples (tuples that appear in both `R` and `S`) are automatically eliminated from the final result.
  *   **Schema:** The attribute names of the resulting relation are taken from the first relation, `R`.
  
  **Example:**
  Given two compatible tables, `S` and `G`, `S ∪ G` produces a new table containing all unique rows from both tables. The row `(202, jones, 3)`, which exists in both tables, appears only once in the final result.
  
  ---
#### **The INTERSECTION (∩) Operation**
  
  The `INTERSECTION` operation finds the common tuples between two compatible relations.
  
  *   **Notation:** R ∩ S
  *   **Result:** The resulting relation contains only the tuples that appear in **both** `R` **and** `S`.
  
  **Example:**
  For tables `S` and `G`, `S ∩ G` would produce a table containing only the single row that is present in both: `(202, jones, 3)`.
  
  ---
#### **The SET DIFFERENCE (-) Operation**
  
  The `SET DIFFERENCE` operation (also known as `MINUS` or `EXCEPT`) finds the tuples that are in one relation but not in another.
  
  *   **Notation:** R - S
  *   **Result:** The resulting relation contains all tuples that are in `R` **but not** in `S`.
  *   **Not Commutative:** This operation is **not commutative**. This is a very important property: `R - S` is not the same as `S - R`.
  
  **Example:**
  *   `S - G`: This gives us the rows that are in `S` but not in `G`. The result is `(240, white, 3)` and `(450, adams, 1)`.
  *   `G - S`: This gives us the rows that are in `G` but not in `S`. The result would be `(701, katz, 1)` and `(820, sapir, 2)`.
  
  ---
#### **The CARTESIAN PRODUCT (×) Operation**
  
  The `CARTESIAN PRODUCT` (also called `CROSS PRODUCT`) is a way to combine tuples from two relations in every possible combination. Unlike the other set operators, the relations do **not** need to be compatible.
  
  *   **Notation:** R × S
  *   **Result:** If `R` has `n` attributes and `m` tuples, and `S` has `k` attributes and `p` tuples, the result is a new relation with:
    *   **`n + k` attributes** (all the attributes of `R` followed by all the attributes of `S`).
    *   **`m * p` tuples** (each tuple of `R` is paired with every tuple of `S`).
  *   **Meaningfulness:** By itself, the `CARTESIAN PRODUCT` is rarely a meaningful operation, as it pairs up unrelated tuples and creates a massive amount of data. Its real power comes when it is immediately followed by a `SELECT` operation to filter the results down to only the meaningful combinations. This sequence of `CARTESIAN PRODUCT` followed by `SELECT` is the foundation of the `JOIN` operation.