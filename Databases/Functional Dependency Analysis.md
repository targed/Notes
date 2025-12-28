### **Functional Dependencies (FDs)**

**1. Definition**
A Functional Dependency is a constraint between two sets of attributes in a relation. It describes a relationship where one attribute determines the value of another.
*   **Notation:** $A \rightarrow B$
*   **Meaning:** "A determines B" or "B is functionally dependent on A."
*   **The Rule:** If you know the value of $A$, you can uniquely predict the value of $B$.

**2. Formal Tuple Definition**
Let $X$ and $Y$ be sets of attributes. The FD $X \rightarrow Y$ holds if:
For any two rows (tuples) $t1$ and $t2$ in the table:
*   **IF** $t1[X] = t2[X]$ (The values for column X are the same)
*   **THEN** $t1[Y] = t2[Y]$ (The values for column Y MUST also be the same)

**3. Visualizing the Rule**
*   **Valid FD:** If "Student ID" 100 appears in row 1 and row 5, the "Name" in both row 1 and row 5 must be "Smith".
*   **Invalid FD:** If "Student ID" 100 has "Smith" in row 1 but "Jones" in row 5, then $ID \rightarrow Name$ is **FALSE**.
*   *Note:* It is perfectly fine for two *different* IDs (100 and 101) to have the *same* name ("Smith"). The determining direction only goes one way.

---
- ### **Semantics and Sources of FDs**
  
  **1. Where do FDs come from?**
  You **cannot** determine FDs purely by looking at a snapshot of data in a table.
  *   **Example:** You might look at a class list and see that every student with the last name "Johnson" has a grade of "A". This is a coincidence, not a rule.
  *   **Source:** FDs must be defined by the **semantics** (business rules) of the database. You must know that "SSN identifies a person" to declare $SSN \rightarrow Name$.
  
  **2. Relation to Keys**
  *   If attribute $X$ is a **Candidate Key**, then by definition, $X$ uniquely determines **every other attribute** in the relation.
  *   $X \rightarrow \text{All Attributes}$.
  
  **3. Static Schema vs. Dynamic State**
  FDs are a property of the **Schema** (the design), not the **State** (current data). They must hold true for **all possible legal states** of the database, forever.
  
  ---
- ### **Inference Rules (Armstrong's Axioms)**
  
  If we know a few FDs, we can logically deduce others. We use **Inference Rules** to find the Closure ($F^+$), which is the set of *all* possible FDs.
  
  **The Three Primary Rules (Armstrong's Axioms):**
  1.  **Reflexive Rule (Trivial):** If $Y$ is a subset of $X$, then $X \rightarrow Y$.
    *   *Example:* $\{SSN, Name\} \rightarrow SSN$. (Obvious/Trivial).
  2.  **Augmentation Rule:** If $X \rightarrow Y$, then $XZ \rightarrow YZ$.
    *   *Example:* If $SSN \rightarrow Name$, then $\{SSN, Age\} \rightarrow \{Name, Age\}$.
  3.  **Transitive Rule:** If $X \rightarrow Y$ and $Y \rightarrow Z$, then $X \rightarrow Z$.
    *   *Example:* If $SSN \rightarrow DeptID$ and $DeptID \rightarrow MgrName$, then $SSN \rightarrow MgrName$.
  
  **Secondary (Derived) Rules:**
  *   **Decomposition:** If $X \rightarrow YZ$, then $X \rightarrow Y$ and $X \rightarrow Z$. (You can split the right side).
  *   **Union:** If $X \rightarrow Y$ and $X \rightarrow Z$, then $X \rightarrow YZ$. (You can combine the right side).
  
  ---
- ### **Attribute Closure ($X^+$) Algorithm**
  
  Calculating the full closure of FDs ($F^+$) is computationally expensive. Instead, we often just need the **Closure of a specific attribute set ($X^+$)**.
  
  **1. Definition of $X^+$**
  The set of all attributes that are functionally dependent on $X$.
  
  **2. The Algorithm**
  1.  Start with $X^+ = X$.
  2.  Look at your list of FDs. If the Left-Hand Side (LHS) of an FD is inside your current $X^+$, add the Right-Hand Side (RHS) to $X^+$.
  3.  Repeat until you can't add anything else.
  
  **3. Why is this useful?**
  This is the "Superkey Test."
  *   If you calculate $X^+$ and the result contains **all attributes** in the relation, then **$X$ is a Superkey**.