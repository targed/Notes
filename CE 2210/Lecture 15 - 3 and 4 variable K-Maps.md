## 3-Variable K-Maps:
  
  * **Grid:** Similar to 2-variable K-maps, but arranged differently.  The most significant bit (MSB) is typically placed at the top, and Gray code is used to ensure adjacent cells differ by only one bit.
  * **Example:**
  
  |     | BC=00 | BC=01 | BC=11 | BC=10 |
  |---|---|---|---|---|
  | A=0 |   |   |   |   |
  | A=1 |   |   |   |   |
  
  
  * **Reduction:** Adjacent 1's indicate variable reduction. Groups must be powers of 2 (1, 2, 4, 8). Horizontal, vertical, and square groupings are allowed. Edges wrap around.
  * **Writing Terms:** Each circled group is a term. A group of 4 eliminates two variables, a group of 2 eliminates one.
  
  
  * **Example:** Find F for the truth table:
  
  | A | B | C | F |
  |---|---|---|---|
  | 0 | 0 | 0 | 0 |
  | 0 | 0 | 1 | 0 |
  | 0 | 1 | 0 | 1 |
  | 0 | 1 | 1 | 1 |
  | 1 | 0 | 0 | 1 |
  | 1 | 0 | 1 | 0 |
  | 1 | 1 | 0 | 1 |
  | 1 | 1 | 1 | 1 |
  
  * **K-Map:**
  
  |     | BC=00 | BC=01 | BC=11 | BC=10 |
  |---|---|---|---|---|
  | A=0 | 0 | 0 | 1 | 1 |
  | A=1 | 1 | 0 | 1 | 1 |
  
  
  * **Circled Groups and Simplified Expression:**
  
  The groupings are !AB, BC, and A!C. Therefore, F = !AB + BC + A!C. The simplified expression is F = !AB + BC + A!C
  
  * **Alternate Map and Don't Cares:**  The Gray code sequence can be altered, but 00, 01, 11, 10 is standard. "Don't cares" (X) represent irrelevant values and can be 0 or 1 for simplification. For example, if F cannot have A=B=C=0:
  
  
  |     | BC=00 | BC=01 | BC=11 | BC=10 |
  |---|---|---|---|---|
  | A=0 | X | 0 | 1 | 1 |
  | A=1 | 1 | 0 | 1 | 1 |
  
  If X=0, F = !AB + C. If X=1, F = !A+C.
## 4-Variable K-Maps:
  
  * **Grid:** A 4x4 grid. Each row/column represents two variables (Gray code). Variable order is consistent.
  
  * **Example:**
  
  |     | CD=00 | CD=01 | CD=11 | CD=10 |
  |---|---|---|---|---|
  | AB=00 |   |   |   |   |
  | AB=01 |   |   |   |   |
  | AB=11 |   |   |   |   |
  | AB=10 |   |   |   |   |
  
  
  * **Example: Find F = ΣABCD m(0, 1, 4, 5, 6, 8, 9, 12, 13, 14)**
  
  * **K-Map and Groupings:**  (Filled with 1's at the specified minterms). The simplified expression is F = !C + B!D.
## Key Points:
  
  * K-maps visually and efficiently simplify Boolean expressions.
  * Group 1's to eliminate variables.
  * Don't cares aid simplification.
  * Essential for understanding and minimizing circuits.
  * Not generally effective beyond 4 variables.