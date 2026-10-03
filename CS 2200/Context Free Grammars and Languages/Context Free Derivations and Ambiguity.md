- **1. Derivations Revisited**
  
  *   A derivation is a sequence of applications of production rules, starting from the start symbol $S$, transforming it into a string of terminals $w \in L(G)$.
  *   Each intermediate string in the sequence, possibly containing both variables and terminals, is called a **sentential form**.
  ---
- **2. Leftmost and Rightmost Derivations (Section 4.3.1)**
  
  *   In a general derivation step, if a sentential form contains multiple variables, we have a choice of which variable to replace next.
  *   To standardize derivations and relate them to parse trees, we define specific derivation strategies:
  
    *   **Leftmost Derivation (LMD):**
        *   **Definition:** At each step of the derivation, the **leftmost** variable in the current sentential form is the one chosen to be replaced using one of its production rules.
        *   **Example 4.16 (w = baaaababaab):**
            *   Grammar: $S \to baXaS \mid ab$, $X \to Xab \mid aa$
            *   LMD:
                1.  $S \Rightarrow \underline{S} \to ba\underline{X}aS$ (Replace leftmost S)
                2.  $ba\underline{X}aS \Rightarrow ba\underline{X}abaS$ (Replace leftmost X using $X \to Xab$)
                3.  $ba\underline{X}abaS \Rightarrow ba\underline{X}ababaS$ (Replace leftmost X using $X \to Xab$)
                4.  $ba\underline{X}ababaS \Rightarrow ba a a ababa\underline{S}$ (Replace leftmost X using $X \to aa$)
                5.  $baaaababa\underline{S} \Rightarrow baaaababaab$ (Replace leftmost S using $S \to ab$)
  
    *   **Rightmost Derivation (RMD) (also called Canonical Derivation):**
        *   **Definition:** At each step of the derivation, the **rightmost** variable in the current sentential form is the one chosen to be replaced using one of its production rules.
        *   **Example 4.16 (w = baaaababaab):**
            *   RMD:
                1.  $S \Rightarrow baXa\underline{S}$ (Replace rightmost S)
                2.  $baXa\underline{S} \Rightarrow baX aab$ (Replace rightmost S using $S \to ab$)
                3.  $ba\underline{X} aab \Rightarrow ba\underline{X}abaab$ (Replace rightmost X using $X \to Xab$)
                4.  $ba\underline{X}abaab \Rightarrow ba\underline{X}ababaab$ (Replace rightmost X using $X \to Xab$)
                5.  $ba\underline{X}ababaab \Rightarrow ba aa ababaab$ (Replace rightmost X using $X \to aa$)
  
    *   **Significance:** For any string derivable in a CFG, there exists at least one LMD and at least one RMD.
  ---
- **3. Derivation Trees (Parse Trees) (Section 4.3.2)**
  
  *   **Purpose:** Derivation trees provide a graphical representation of how a string is derived from the start symbol according to the grammar's rules. They clearly show the hierarchical structure and how substrings are grouped according to variables.
  *   **Properties:**
    1.  **Root:** The root node is labeled with the start symbol $S$.
    2.  **Leaves:** Each leaf node is labeled with a terminal symbol ($a \in T$) or the empty string $\epsilon$.
    3.  **Internal Nodes:** Each internal node (non-leaf) is labeled with a variable ($A \in V$).
    4.  **Production Representation:** If an internal node is labeled $A$ and its children, read from left to right, are labeled $X_1, X_2, \dots, X_k$, then $A \to X_1 X_2 \dots X_k$ must be a production in $P$. If a node $A$ has a single child $\epsilon$, then $A \to \epsilon$ must be a production.
  *   **Yield:** Concatenating the labels of the leaves of a parse tree from left to right produces a string. This string is called the **yield** of the tree. The yield is always a sentential form. If the yield consists only of terminals, it is a string in the language $L(G)$.
  
  *   **Example 4.18 (String "baaba"):**
    *   Grammar: $S \to AAA \mid AA$, $A \to AA \mid aA \mid Ab \mid a \mid b$.
    *   (The tree in Fig 4.1 shows *one possible* derivation structure leading to "baaba").
  ---
- **4. Equivalence of Parse Trees and Derivations (Section 4.3.3)**
  
  *   There is a direct correspondence between derivations and parse trees:
    *   Every valid parse tree corresponds to at least one LMD and at least one RMD (and possibly other derivations).
    *   Every derivation (LMD, RMD, or mixed) corresponds to a unique parse tree.
  *   A string $w$ is in $L(G)$ if and only if there exists at least one parse tree whose root is $S$ and whose yield is $w$.
  ---
- **5. Ambiguity (Section 4.4)**
  
  *   **Definition 3:** A Context-Free Grammar $G$ is **ambiguous** if there exists at least one string $w \in L(G)$ that has **more than one distinct parse tree**.
  *   **Equivalent Conditions:** A grammar $G$ is ambiguous if and only if there exists some string $w \in L(G)$ that has:
    *   More than one Leftmost Derivation (LMD).
    *   More than one Rightmost Derivation (RMD).
  *   **Significance:** Ambiguity is often undesirable, especially in programming languages, because it means a single program (string) could be interpreted in multiple ways (different parse trees imply different structures and potentially different meanings/evaluations). Compilers need unambiguous grammars to parse code uniquely.
  
  *   **Example 4.19: Ambiguous Expression Grammar**
    *   Grammar: $E \to E + E \mid E * E \mid \text{id}$ (assuming `id` is a terminal).
    *   String: `id + id * id`
    *   *Parse Tree 1 (LMD 1):* Corresponds to `(id + id) * id`
        1.  $E \Rightarrow E * E$
        2.  $\underline{E} * E \Rightarrow E + E * E$
        3.  $\underline{E} + E * E \Rightarrow \text{id} + E * E$
        4.  $\text{id} + \underline{E} * E \Rightarrow \text{id} + \text{id} * E$
        5.  $\text{id} + \text{id} * \underline{E} \Rightarrow \text{id} + \text{id} * \text{id}$
    *   *Parse Tree 2 (LMD 2):* Corresponds to `id + (id * id)`
        1.  $E \Rightarrow E + E$
        2.  $\underline{E} + E \Rightarrow \text{id} + E$
        3.  $\text{id} + \underline{E} \Rightarrow \text{id} + E * E$
        4.  $\text{id} + \underline{E} * E \Rightarrow \text{id} + \text{id} * E$
        5.  $\text{id} + \text{id} * \underline{E} \Rightarrow \text{id} + \text{id} * \text{id}$
    *   Since the string `id + id * id` has two different LMDs (and corresponding parse trees, shown in Fig 4.2), the grammar is ambiguous.
  
  *   **Example 4.20: Ambiguous Grammar $S \to aS \mid Sa \mid a$**
    *   String: `aa`
    *   *LMD 1:* $S \Rightarrow aS \Rightarrow aa$
    *   *LMD 2:* $S \Rightarrow Sa \Rightarrow aa$
    *   Two LMDs (and corresponding parse trees, Fig 4.3) exist for "aa", so the grammar is ambiguous.
  
  *   **Example 4.22: Palindrome Grammar $S \to aSa \mid bSb \mid a \mid b \mid \epsilon$**
    *   String: `babbab` (Yield of tree in Fig 4.5).
    *   Consider the derivation: $S \Rightarrow bSb \Rightarrow baSab \Rightarrow babSbab \Rightarrow babbab$. This derivation seems unique.
    *   Is the grammar ambiguous? Let's try deriving "a": $S \Rightarrow a$. Let's try deriving "aba": $S \Rightarrow aSa \Rightarrow aba$. These seem unique. The text claims this grammar is *unambiguous* (referencing Fig 4.5 only shows one tree for "babbab", and states "Since there is only one parse tree..."). For palindromes, ambiguity usually doesn't arise directly from structure unless rules overlap unnecessarily.
  
  *   **Example 4.23: Dangling Else Grammar (Classic Ambiguity)**
    *   Grammar: $S \to i C t S \mid i C t S e S \mid a$, $C \to b$ (i=if, C=condition, t=then, e=else, S=statement, a=other statement, b=boolean condition).
    *   String: `ibtibtaea` (if b then if b then a else a)
    *   *Parse Tree 1:* Associates the `else` with the *inner* `if`. Structure: `if b then (if b then a else a)`
    *   *Parse Tree 2:* Associates the `else` with the *outer* `if`. Structure: `if b then (if b then a) else a`
    *   Since there are two parse trees (Fig 4.6), the grammar is ambiguous. This specific ambiguity is known as the "dangling else" problem.
  
  ---
- **Helpful Resources:**
  
  1.  **Wikipedia - Ambiguous grammar:** [https://en.wikipedia.org/wiki/Ambiguous_grammar](https://en.wikipedia.org/wiki/Ambiguous_grammar)
  2.  **Wikipedia - Parse tree:** [https://en.wikipedia.org/wiki/Parse_tree](https://en.wikipedia.org/wiki/Parse_tree)
  3.  **TutorialsPoint - Automata Theory - Ambiguity in Grammar:** [https://www.tutorialspoint.com/automata_theory/ambiguity_in_grammar.htm](https://www.tutorialspoint.com/automata_theory/ambiguity_in_grammar.htm)
  4.  **JFLAP Tutorial - Parse Trees & Ambiguity:** [https://www.jflap.org/tutorial/grammar/parse/index.html](https://www.jflap.org/tutorial/grammar/parse/index.html)