#### **1. The Conceptual Stages of a Join**
Walk through a query finding the names of sailors who have reserved a boat.
`SELECT sname FROM sailor, reserve WHERE sailor.sid = reserve.sid`

Although modern databases optimize this heavily, logically, the system follows these three specific stages (matching the **Evaluation Order** we discussed earlier):

*   **Stage 1: The Cartesian Product (`FROM`)**
  *   The system takes every row from the `Sailor` table and pairs it with every row from the `Reserve` table.
  *   If `Sailor` has 6 rows and `Reserve` has 6 rows, the intermediate result has **36 rows**. Most of these pairings are nonsense (e.g., Sailor 22 paired with a reservation made by Sailor 58).
*   **Stage 2: Filtering (`WHERE`)**
  *   The system applies the condition `sailor.sid = reserve.sid`.
  *   It scans the 36 rows and discards those where the IDs don't match.
  *   This leaves only the meaningful pairings (e.g., Sailor 22 paired with Sailor 22's reservation).
*   **Stage 3: Projection (`SELECT`)**
  *   The system discards all columns except the one requested: `sname`.
  *   The final result is returned.
- #### **2. Set Operations**
  SQL allows you to treat the results of two separate queries as mathematical sets and combine them.
  
  **The Three Operators:**
  1.  **`UNION`**: Combines results from two queries. (Logical **OR**).
  2.  **`INTERSECT`**: Returns only rows found in *both* queries. (Logical **AND**).
  3.  **`EXCEPT`** (or `MINUS` in Oracle): Returns rows found in the first query but *not* the second. (Logical **NOT**).
  
  **Key Rules for Set Operations:**
  *   **Union Compatibility:** The two queries being combined must have the **same number of attributes** and **compatible data types** in the same order. You cannot `UNION` a list of Names with a list of IDs.
  *   **Duplicate Elimination:** Unlike a standard `SELECT` query, set operators **automatically remove duplicates** by default.
    *   To keep duplicates, you must append `ALL` (e.g., `UNION ALL`).
  
  **Examples:**
  *   **`UNION`:** "Find sailors who reserved a red boat **OR** a green boat."
    *   (Set of Red Reservers) `UNION` (Set of Green Reservers).
  *   **`INTERSECT`:** "Find sailors who reserved a red boat **AND** a green boat."
    *   (Set of Red Reservers) `INTERSECT` (Set of Green Reservers).
    *   *Note:* You cannot do this with a simple `WHERE color='red' AND color='green'` because a single reservation row cannot be two colors at once. You *must* use Set operations (or self-joins) here.
  *   **`EXCEPT`:** "Find sailors who reserved a red boat **BUT DID NOT** reserve a green boat."
    *   (Set of Red Reservers) `EXCEPT` (Set of Green Reservers).