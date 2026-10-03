- **1. Equivalence of PDAs and CFGs (Section 5.4)**
  
  *   **Fundamental Result:** Pushdown Automata (PDAs) and Context-Free Grammars (CFGs) are **equivalent** in their descriptive power. They both define exactly the class of **Context-Free Languages (CFLs)**.
  *   This means:
  *   For any CFG $G$, there exists a PDA $M$ such that $L(G) = L(M)$ (or $N(M)$, depending on acceptance type).
  *   For any PDA $M$, there exists a CFG $G$ such that $L(M) = L(G)$ (or $N(M) = L(G)$).
  ---
- **2. Constructing a PDA from a Given CFG (Section 5.4.1)**
  
  *   **Theorem 4:** If $L$ is a context-free language (meaning it has a CFG $G$), then there is a PDA $M$ such that $L = N(M)$ (accepts $L$ by empty stack).
  *   **Proof Idea (Simulation of Leftmost Derivation):**
  *   The PDA simulates the leftmost derivation of a string $w$ according to the grammar $G$.
  *   The PDA's stack holds the current **sentential form** (or rather, the suffix of variables and terminals yet to be processed/matched) of the simulated derivation.
  *   The goal is to match input symbols with terminals produced by the derivation and end with an empty stack when the input is fully consumed.
  *   **Construction Steps (Simplified for Greibach Normal Form - GNF):**
  *   Assume the CFG $G=(V, T, P, S)$ is in **Greibach Normal Form (GNF)**. In GNF, all productions are of the form $A \to a\gamma$, where $a \in T$ is a terminal and $\gamma \in V^*$ is a (possibly empty) string of variables. (Any CFG can be converted to GNF, except if it generates $\epsilon$).
  *   Construct PDA $M = (\{q\}, T, V \cup T, \delta, q, S, \emptyset)$. (Single state $q$, stack alphabet includes variables and terminals, start stack symbol is $S$).
  *   **Transition Function $\delta$:**
      *   For each production $A \to a\gamma$ in $P$:
          Add a transition $\delta(q, a, A) = \{(q, \gamma)\}$.
          *Interpretation:* If the PDA needs to expand variable $A$ (which is on top of the stack) and the next input symbol is $a$, it consumes $a$, pops $A$, and pushes the variable string $\gamma$ onto the stack (replacing $A$).
  *   **How it works:** The PDA maintains the expected sequence of variables (from the derivation's sentential form) on its stack. When it sees a terminal $a$ in the input, it checks if the top stack symbol $A$ has a rule $A \to a\gamma$. If yes, it consumes $a$, pops $A$, pushes $\gamma$, and continues. If the stack becomes empty just as the input finishes, the string is accepted (by empty stack).
  *   **Construction Steps (General Case - Not requiring GNF, as described in the text):**
  *   PDA uses its stack to hold the prediction of terminals and variables yet to be seen.
  *   Start with $S$ (start symbol) on the stack.
  *   **If top of stack is a variable $A$:** Non-deterministically choose a production $A \to \gamma$. Pop $A$, push $\gamma$ (in reverse order so first symbol of $\gamma$ is on top). This is an $\epsilon$-move.
  *   **If top of stack is a terminal $a$:** Read the next input symbol. If it matches $a$, pop $a$ from the stack. If it doesn't match, that computation path fails.
  *   **Acceptance (by Empty Stack):** If the input is fully consumed *and* the stack becomes empty, the original string is accepted.
  *   **Example 5.9:**
  *   Grammar (already GNF): $S \to aAA$, $A \to aS \mid bS \mid a$.
  *   PDA (single state $q$, accepting by empty stack):
      *   From $S \to aAA$: $\delta(q, a, S) = \{(q, AA)\}$.
      *   From $A \to aS$: $\delta(q, a, A) = \{(q, S)\}$.
      *   From $A \to bS$: $\delta(q, b, A) = \{(q, S)\}$.
      *   From $A \to a$: $\delta(q, a, A) = \{(q, \epsilon)\}$. (Pop A, push nothing).
  *   **Trace of "aaa" (Fig 5.12):**
      1.  Initial: State $q$, Input `aaa`, Stack `S`.
      2.  Use $S \to aAA$. Read 'a', pop S, push AA. State $q$, Input `aa`, Stack `AA`. (Top A)
      3.  Use $A \to aS$. Read 'a', pop A, push S. State $q$, Input `a`, Stack `SA`. (Top S)
      4.  Use $S \to aAA$. Read 'a', pop S, push AA. State $q$, Input $\epsilon$, Stack `AAA`. (Top A)
      5.  Need to consume input $\epsilon$ but stack not empty. Problem in trace/acceptance. Let's re-trace carefully simulating empty stack acceptance.
      *   Correct Trace for "aaa":
          1.  $(q, aaa, S)$
          2.  $\vdash (q, aa, AA)$ (using $\delta(q, a, S)$ for $S \to aAA$)
          3.  $\vdash (q, a, SA)$ (using $\delta(q, a, A)$ for $A \to aS$)
          4.  $\vdash (q, \epsilon, SAA)$ (using $\delta(q, a, S)$ for $S \to aAA$ - Error here, should use an A rule. Let's use $A \to a$ for top A)
          *   Restart trace:
          1.  $(q, aaa, S)$
          2.  $\vdash (q, aa, AA)$ (using $\delta(q, a, S)$ for $S \to aAA$)
          3.  $\vdash (q, a, A)$ (using $\delta(q, a, A)$ for $A \to a$)
          4.  $\vdash (q, \epsilon, \epsilon)$ (using $\delta(q, a, A)$ for $A \to a$)
          Input empty, Stack empty. **Accept.** (Fig 5.12 seems to illustrate intermediate stack states differently, perhaps pushing terminals too, which isn't standard for this construction).
  ---
- **3. Constructing a CFG from a Given PDA (Section 5.4.2)**
  
  *   **Theorem 5:** If $L$ is $L(M)$ (or $N(M)$) for some PDA $M$, then $L$ is a context-free language (meaning it has a CFG $G$).
  *   **Proof Idea:**
  *   The CFG's variables will represent the possibility of the PDA going from a state $p$ to a state $q$ while consuming some input and causing a net consumption of a specific stack symbol $X$.
  *   Variables are often denoted as $[pXq]$ or $(p, X, q)$. This variable generates all input strings $w$ such that $(p, w, X) \vdash^* (q, \epsilon, \epsilon)$ (i.e., starting in state $p$ with $X$ on top of stack, consuming $w$ results in state $q$ with $X$ just popped, effectively returning the stack to its state before $X$ was pushed).
  *   **Construction Steps (Acceptance by Empty Stack):** Let $M = (Q, \Sigma, \Gamma, \delta, q_0, Z_0, F)$ accept by empty stack.
  1.  **Variables ($V$):** Create a variable $[pXq]$ for every $p, q \in Q$ and every $X \in \Gamma$. Also include a start symbol $S$.
  2.  **Terminals ($T$):** Same as $\Sigma$.
  3.  **Start Symbol:** $S$.
  4.  **Productions ($P$):**
      *   **Rule 1 (Start):** For every state $q \in Q$, add the production $S \to [q_0 Z_0 q]$. (The language consists of strings that take the PDA from the initial state $q_0$ with $Z_0$ on stack to *any* state $q$ with an empty stack).
      *   **Rule 2 (Popping Moves):** If $\delta(q, a, X)$ contains $(r, \epsilon)$ (where $a \in \Sigma \cup \{\epsilon\}$), add the production $[qXr] \to a$. (Going from $q$ to $r$ popping $X$ is achieved by consuming input $a$).
      *   **Rule 3 (Pushing Moves):** If $\delta(q, a, X)$ contains $(r, Y_1 Y_2 \dots Y_k)$, then for *all possible sequences* of states $r_1, r_2, \dots, r_k \in Q$, add the production:
          $[q X r_k] \to a [r Y_1 r_1] [r_1 Y_2 r_2] \dots [r_{k-1} Y_k r_k]$.
          (Going from $q$ to $r_k$ consuming $X$ is achieved by consuming $a$, then finding paths corresponding to each pushed symbol $Y_i$, going between intermediate states $r_{i-1}$ and $r_i$).
          *   If $k=0$ (push $\epsilon$), this rule implies $[qXr] \to a$.
          *   If $k=1$ (push $Y_1$), rule is $[qXr_1] \to a [r Y_1 r_1]$.
          *   If $k=2$ (push $Y_1Y_2$), rule is $[qXr_2] \to a [r Y_1 r_1] [r_1 Y_2 r_2]$ for all $r_1$.
  *   **Simplification:** The resulting grammar is often huge and needs simplification (removing useless symbols/productions).
  *   **Example 5.10:** (Walks through applying these rules to a specific PDA). The example shows generating the S-productions (Rule 1) and then productions for specific PDA transitions using Rules 2 and 3, followed by simplification. It demonstrates how a sequence push ($ZZ_0$) translates into chained variables in the grammar rule.
  *   **Example 5.12:** (Converts a PDA for non-nested if-else to CFG). Introduces variables $[qXq]$ and $[qZq]$, applies rules similar to above, then renames variables for clarity ($A = [qZq], B = [qXq]$).
  ---
- **4. Equivalence of Acceptance by Final State and Empty Stack (Section 5.2)**
  
  *   **Theorem 1 & 2:** The two methods of PDA acceptance (final state and empty stack) are equivalent.
  *   Given a PDA $M_F$ that accepts by final state, we can construct a PDA $M_E$ that accepts the same language by empty stack.
  *   Given a PDA $M_E$ that accepts by empty stack, we can construct a PDA $M_F$ that accepts the same language by final state.
  *   **Construction $M_F \to M_E$:**
  *   Add a new start state $q_{0E}$ that pushes the original start symbol $Z_{0F}$ onto a new stack bottom marker $X_0$.
  *   Simulate $M_F$.
  *   Add a new "drain" state $q_{drain}$.
  *   Add $\epsilon$-transitions from every original final state of $M_F$ to $q_{drain}$.
  *   Add transitions from $q_{drain}$ to itself that pop any symbol from the stack: $\delta(q_{drain}, \epsilon, Y) = \{(q_{drain}, \epsilon)\}$ for all $Y \in \Gamma \cup \{X_0\}$.
  *   The only way to empty the stack (including $X_0$) is to reach $q_{drain}$, which only happens if $M_F$ reached a final state. $M_E$ accepts by empty stack.
  *   **Construction $M_E \to M_F$:**
  *   Add a new start state $q_{0F}$ and a new stack bottom marker $X_0$. Push the original start symbol $Z_{0E}$ on top of $X_0$.
  *   Simulate $M_E$.
  *   Add a new final state $q_f$.
  *   Add $\epsilon$-transitions from *any* state $q$ of the original $M_E$ to the new final state $q_f$ *if* the stack top is the new bottom marker $X_0$: $\delta(q, \epsilon, X_0) = \{(q_f, \epsilon)\}$.
  *   $q_f$ is the only final state. The PDA only reaches $q_f$ if the original $M_E$ would have emptied its stack down to the special marker $X_0$. $M_F$ accepts by final state.
  
  ---
- **Helpful Resources:**
  
  1.  **Wikipedia:** Articles on Context-free grammar, Pushdown automaton, Greibach normal form.
  2.  **TutorialsPoint:** Sections on CFG to PDA conversion, PDA to CFG conversion, PDA Acceptance Methods.
  3.  **University Course Notes:** Search for "PDA CFG equivalence", "CFG to PDA construction", "PDA to CFG construction". Many courses cover these proofs.
  4.  **JFLAP:** Can perform CFG <-> PDA conversions and simulate different acceptance criteria.