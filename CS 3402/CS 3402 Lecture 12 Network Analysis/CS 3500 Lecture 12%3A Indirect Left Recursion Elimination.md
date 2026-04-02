### 1. The Problem: The Cycle
**Referencing PDF Page 1 (Top):**
*   **Grammar:**
  1.  $A \to Ba \mid d$
  2.  $B \to Cb$
  3.  $C \to Ac$
*   **The Loop:** `parse_A()` calls `parse_B()`, which calls `parse_C()`, which calls `parse_A()`.
*   **Result:** Stack Overflow / Infinite Loop.
*   **Goal:** Turn this cycle into a standard tree structure (Right Recursion) that a parser can handle.

---
- ### 2. The Algorithm
  **Referencing PDF Page 1 (Middle):**
  We enforce a strict **Order** on the non-terminals ($A_1, A_2, ... A_n$). The rule is: $A_i$ can only start with terminals or non-terminals that come *after* it in the list. If it tries to point backwards to a previous non-terminal ($A_j$ where $j < i$), we **substitute** it.
  
  **The Logic:**
  1.  **Outer Loop ($i$ from 1 to $n$):** Pick the current Non-Terminal $A_i$.
  2.  **Inner Loop ($j$ from 1 to $i-1$):** Look at all previous Non-Terminals.
    *   **Substitution:** If $A_i$ has a rule starting with $A_j$ (e.g., $A_i \to A_j \gamma$), replace $A_j$ with its body.
    *   *Effect:* This drags the definition forward, essentially deleting the backward link.
  3.  **Clean Up:** After the inner loop, $A_i$ might now have **Direct Left Recursion** (because of the substitutions). Run the **EDLR** (Eliminate Direct Left Recursion) algorithm from the previous chapter.
  
  ---
- ### 3. Trace #1: The Simple Cycle
  **Referencing PDF Pages 1 & 2:**
  *   **Order:** 1: $A$, 2: $B$, 3: $C$.
  
  **Step $i=1$ (A):**
  *   $A \to Ba \mid d$.
  *   No previous non-terminals. No direct recursion. **Done.**
  
  **Step $i=2$ (B):**
  *   $B \to Cb$.
  *   Does it start with $A$? No.
  *   No direct recursion. **Done.**
  
  **Step $i=3$ (C):**
  *   $C \to Ac$.
  *   **Check $j=1$ (A):**
    *   Rule starts with $A$. Substitute $A$ ($Ba \mid d$) into $C$.
    *   New $C$: $C \to Bac \mid dc$.
  *   **Check $j=2$ (B):**
    *   Rule now starts with $B$ ($Bac$). Substitute $B$ ($Cb$) into $C$.
    *   New $C$: $C \to Cbac \mid dc$.
  *   **Eliminate Direct Left Recursion:**
    *   Now $C$ calls $C$ directly!
    *   $\alpha = bac$, $\beta = dc$.
    *   **Final C:**
        *   $C \to dcC'$
        *   $C' \to bacC' \mid \lambda$
  
  **Result:** The cycle $A \to B \to C \to A$ is broken. The grammar is now parseable.
  
  ---
- ### 4. Trace #2: The Complex Example
  **Referencing PDF Pages 2 & 3:**
  This demonstrates how the grammar "explodes" in size as you substitute.
  *   **Grammar:**
    1.  $S \to Sx \mid Ay \mid \lambda$
    2.  $A \to Sa \mid Bb \mid Az$
    3.  $B \to Bp \mid Sq \mid r$
  
  **Step $i=1$ (S):**
  *   Has Direct Recursion ($S \to Sx$).
  *   **EDLR Fix:**
    *   $S \to AyS' \mid S'$
    *   $S' \to xS' \mid \lambda$
  
  **Step $i=2 (A)$:**
  *   $A \to Sa \mid \dots$
  *   **Check $j=1$ (S):**
    *   Replace $S$ with its new body ($AyS' \mid S'$).
    *   $A \to AyS'a \mid S'a \mid Bb \mid Az$
    *   *Note:* Now $A$ starts with $A$ ($AyS'a$). This is Direct Recursion.
  *   **EDLR Fix:**
    *   Identify recursive parts ($\alpha$): $yS'a, z$
    *   Identify base parts ($\beta$): $S'a, Bb$
    *   **Result:**
        *   $A \to S'aA' \mid BbA'$
        *   $A' \to yS'aA' \mid zA' \mid \lambda$
  
  **Step $i=3 (B)$:** (The Monster Step on Page 3)
  *   $B \to Sq \dots$
  *   **Substitute S:** $B \to AyS'q \mid S'q \dots$
  *   **Substitute A:** (Replace the $A$ in $AyS'q$ with the massive body of $A$ we just built).
  *   **EDLR Fix:**
    *   After all that substitution, $B$ will eventually start with $B$.
    *   Factor it out into $B$ and $B'$.
  
  **Final Grammar (PDF Page 3 Bottom):**
  The final grammar is huge, involving $S, S', A, A', B, B'$.
  *   *Takeaway:* While this algorithm works, manual implementation is painful. It is usually done by generator tools (like ANTLR) if they need LL parsers.