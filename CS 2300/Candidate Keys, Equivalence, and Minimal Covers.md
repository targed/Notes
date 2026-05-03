### **Candidate Keys and Equivalence**

**1. Finding Candidate Keys using Closure**
We previously learned that if the closure of a set of attributes ($X^+$) includes **all** attributes in the relation, then $X$ is a **Superkey**.
*   To determine if $X$ is a **Candidate Key** (a minimal superkey), we must ensure that **no proper subset** of $X$ is also a superkey.
*   *Example:* If $\{Student\_ID, Dept\_ID\}^+$ = All Attributes, it is a Superkey. But if $\{Student\_ID\}^+$ is *also* All Attributes, then the pair is NOT a Candidate Key (it has extra baggage). The Candidate Key is just `{Student_ID}`.

**2. Equivalence of FD Sets**
Sometimes different sets of rules mean the exact same thing.
*   **Definition:** Two sets of FDs, $F$ and $G$, are **equivalent** if:
  1.  Every FD in $G$ can be inferred using the rules in $F$ ($F$ covers $G$).
  2.  Every FD in $F$ can be inferred using the rules in $G$ ($G$ covers $F$).
*   **Why this matters:** We want to find the smallest, simplest set of FDs ($G$) that is equivalent to our messy original set ($F$).

---
- ### **Minimal Set (Minimal Cover)**
  
  A **Minimal Cover** (or Canonical Cover) is the standard, simplified version of a set of FDs. It removes all redundancy. Normalization algorithms require the FDs to be in this format.
  
  **The Three Conditions for Minimality:**
  A set of FDs is minimal if:
  1.  **Singleton RHS:** Every FD has only **one** attribute on the Right-Hand Side.
    *   *Change:* $A \rightarrow \{B, C\}$ to two FDs: $A \rightarrow B$ and $A \rightarrow C$.
  2.  **No Redundant LHS Attributes:** You cannot remove an attribute from the Left-Hand Side and keep the same meaning.
    *   *Check:* If you have $AB \rightarrow C$, but it turns out that $A \rightarrow C$ is already true, then $B$ is redundant.
  3.  **No Redundant FDs:** You cannot remove an entire FD from the set and keep the same meaning.
    *   *Check:* If you have $A \rightarrow B$, $B \rightarrow C$, and $A \rightarrow C$, the rule $A \rightarrow C$ is redundant because it is implied by Transitivity.
  
  **The Algorithm Example:**
  Given $F = \{B \rightarrow AB, D \rightarrow A, AB \rightarrow D\}$
  
  *   **Step 1 (Singleton RHS):**
    *   Break $B \rightarrow AB$ into $B \rightarrow A$ and $B \rightarrow B$.
    *   Remove trivial $B \rightarrow B$.
    *   Current Set: $\{B \rightarrow A, D \rightarrow A, AB \rightarrow D\}$.
  *   **Step 2 (Redundant LHS):**
    *   Look at $AB \rightarrow D$. Is $A$ or $B$ redundant?
    *   We check the closure of $B$ alone ($B^+$). Since $B \rightarrow A$, $B^+ = \{B, A\}$.
    *   Since $B$ determines $A$, knowing $B$ is enough to determine $D$ (via $AB \rightarrow D$). Therefore, $A$ is redundant on the left side.
    *   Current Set: $\{B \rightarrow A, D \rightarrow A, B \rightarrow D\}$.
  *   **Step 3 (Redundant FDs):**
    *   We check if any rule is implied by the others.
    *   Is $B \rightarrow A$ redundant? Calculate $B^+$ using the *other* rules ($\{D \rightarrow A, B \rightarrow D\}$).
        *   $B$ determines $D$. $D$ determines $A$. Therefore $B$ determines $A$.
    *   Yes, $B \rightarrow A$ is redundant via transitivity. Remove it.
  *   **Final Minimal Cover:** $\{D \rightarrow A, B \rightarrow D\}$.
  
  **Important Note:**
  There can be **more than one** valid minimal cover for a relation.