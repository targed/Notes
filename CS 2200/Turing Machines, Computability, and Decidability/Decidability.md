- **1. Introduction: Limits of Algorithmic Solvability (Sipser)**
  
  *   Having established the Turing Machine as a formal model for algorithms, we investigate the **power and limits** of what algorithms can solve.
  *   Some problems can be solved algorithmically (decidable), while others provably cannot (undecidable).
  *   Understanding this boundary is crucial in computer science.
  ---
- **2. Key Definitions**
  
  *   **Algorithm:** A precise, step-by-step procedure that is guaranteed to halt on all inputs and produce a correct result. In theoretical terms, often modeled by a Turing Machine that **always halts**.
  *   **Decision Problem:** A problem that requires a **yes/no** answer for each instance (input). (Sunitha).
    *   *Example:* "Given two numbers $x$ and $y$, does $x$ evenly divide $y$?" The answer depends on $x, y$ and is either 'yes' or 'no'.
  *   **Decision Procedure:** An algorithm that solves a decision problem. Given an instance of the problem, the algorithm halts and outputs the correct 'yes' or 'no' answer. (Sunitha).
  *   **Decidable Problem:** A decision problem for which a **decision procedure exists**. If a problem can be solved by an algorithm that *always halts* with the correct yes/no answer, it is decidable. (Sipser/Sunitha).
    *   *Example:* The problem "does $x$ evenly divide $y$?" is decidable because the long division algorithm is a decision procedure for it.
  *   **Undecidable Problem:** A decision problem for which **no decision procedure exists**. No algorithm can solve *all* instances of the problem correctly and be guaranteed to halt.
  ---
- **3. Decidability vs. Recognizability (Relating to TMs)**
  
  *   A language $L$ is **decidable** (or **recursive**) if there exists a Turing Machine $M$ (called a **decider**) such that:
    *   If $w \in L$, $M$ accepts $w$ (halts in an accept state).
    *   If $w \notin L$, $M$ rejects $w$ (halts in a reject state).
    *   Crucially, $M$ **halts on all inputs**.
  *   A language $L$ is **Turing-recognizable** (or **recursively enumerable**) if there exists a Turing Machine $M$ (called a **recognizer**) such that:
    *   If $w \in L$, $M$ accepts $w$ (halts in an accept state).
    *   If $w \notin L$, $M$ either rejects $w$ (halts in reject state) OR **loops forever**.
  *   **Relationship:** Every decidable language is Turing-recognizable, but not every Turing-recognizable language is decidable (e.g., the language corresponding to the Halting Problem).
  ---
- **4. Why Study Undecidability? (Sipser)**
  
  *   **Practical Reason:** Knowing a problem is undecidable prevents wasting effort trying to find a general algorithmic solution. It indicates the problem must be **simplified or altered** (e.g., by adding constraints, looking for approximate solutions, or handling only a subset of cases) before an algorithmic solution can be found. It highlights the limitations of computers as tools.
  *   **Cultural Reason:** Understanding the limits of computation provides perspective and stimulates imagination, even when dealing with solvable problems. It deepens our understanding of the nature of computation.
  ---
- **5. Decision Problems Formally (Languages & Encodings)**
  
  *   Theoretically, decision problems are often formalized as **language recognition problems**.
  *   **Formal Definition (Sunitha Def 1 Equiv.):** A decision problem can be viewed as the **set of inputs for which the answer is 'yes'**.
  *   **Encoding:** Since TMs take strings as input, problem instances (like numbers, graphs, grammars) must be **encoded** as strings (e.g., $\langle G \rangle$ for a graph $G$).
  *   **Language Formulation:** A decision problem about objects of type $O$ can be formulated as the language $L = \{ \langle O \rangle \mid \text{The answer for object } O \text{ is 'yes'} \}$.
  *   **Decidability (Language View):** A decision problem is decidable if its corresponding language $L$ is a decidable language (i.e., there's a TM that halts on all inputs, accepting strings in $L$ and rejecting strings not in $L$).
  ---
- **6. Decidable Problems Concerning Regular Languages (Section 7.2.1)**
  
  *   Many fundamental questions about regular languages (represented by DFAs, NFAs, or REs) *are* decidable. Algorithms (TMs) exist to answer them.
  
    *   **Acceptance Problem for DFAs ($A_{DFA}$):** Given a DFA $B$ and a string $w$, does $B$ accept $w$?
        *   Language: $A_{DFA} = \{ \langle B, w \rangle \mid B \text{ is a DFA that accepts } w \}$.
        *   **Decidable:** Yes. Simulate the DFA $B$ on input $w$. DFAs always halt. (Example 7.1).
    *   **Acceptance Problem for NFAs ($A_{NFA}$):** Given an NFA $B$ and a string $w$, does $B$ accept $w$?
        *   Language: $A_{NFA} = \{ \langle B, w \rangle \mid B \text{ is an NFA that accepts } w \}$.
        *   **Decidable:** Yes. Convert the NFA $B$ to an equivalent DFA $C$ (using subset construction). Then run the decider for $A_{DFA}$ on $\langle C, w \rangle$. (Example 7.2).
    *   **Acceptance Problem for Regular Expressions ($A_{REX}$):** Given a RE $R$ and a string $w$, does $R$ generate $w$?
        *   Language: $A_{REX} = \{ \langle R, w \rangle \mid R \text{ is an RE that generates } w \}$.
        *   **Decidable:** Yes. Convert the RE $R$ to an equivalent NFA $A$ (using Thompson's construction). Then run the decider for $A_{NFA}$ on $\langle A, w \rangle$. (Example 7.3 Solution).
    *   **Emptiness Problem for DFAs ($E_{DFA}$):** Given a DFA $A$, is its language $L(A)$ empty?
        *   Language: $E_{DFA} = \{ \langle A \rangle \mid A \text{ is a DFA and } L(A) = \emptyset \}$.
        *   **Decidable:** Yes. Check if there is any path from the start state to any final state using graph reachability (e.g., BFS/DFS starting from the start state). If no final state is reachable, the language is empty. (Example 7.4).
    *   **Equivalence Problem for DFAs ($EQ_{DFA}$):** Given two DFAs $A$ and $B$, is $L(A) = L(B)$?
        *   Language: $EQ_{DFA} = \{ \langle A, B \rangle \mid A, B \text{ are DFAs and } L(A) = L(B) \}$.
        *   **Decidable:** Yes. Construct a DFA $C$ that accepts the symmetric difference of $L(A)$ and $L(B)$: $L(C) = (L(A) \cap \overline{L(B)}) \cup (\overline{L(A)} \cap L(B))$. This can be done using cross-product construction and swapping final states for complements. Then, check if $L(C)$ is empty using the decider for $E_{DFA}$. $L(A) = L(B)$ if and only if $L(C) = \emptyset$. (Example 7.5).
  ---
- **7. Decidable Problems Concerning Context-Free Languages (Section 7.2.2)**
  
  *   Some problems about CFLs (represented by CFGs or PDAs) are also decidable.
  
    *   **Acceptance Problem for CFGs ($A_{CFG}$):** Given a CFG $G$ and a string $w$, does $G$ generate $w$?
        *   Language: $A_{CFG} = \{ \langle G, w \rangle \mid G \text{ is a CFG that generates } w \}$.
        *   **Decidable:** Yes.
            *   *Method 1 (Inefficient):* Try all possible derivations. This might not halt if the grammar has loops.
            *   *Method 2 (Requires CNF):* Convert $G$ to Chomsky Normal Form (CNF). For a string $w$ of length $n$, any derivation in CNF takes exactly $2n-1$ steps. Generate all derivations of length $2n-1$. If any produces $w$, accept; otherwise reject. This halts. (Example 7.6 Solution).
            *   *Method 3 (Efficient):* Use dynamic programming parsing algorithms like CYK or Earley's algorithm. These run in polynomial time.
    *   **Emptiness Problem for CFGs ($E_{CFG}$):** Given a CFG $G$, is $L(G) = \emptyset$?
        *   Language: $E_{CFG} = \{ \langle G \rangle \mid G \text{ is a CFG and } L(G) = \emptyset \}$.
        *   **Decidable:** Yes. Determine which variables can generate *any* terminal string. Start by marking terminals. Then mark any variable $A$ if there is a rule $A \to \gamma$ where all symbols in $\gamma$ are already marked. Repeat until no new variables can be marked. If the start symbol $S$ gets marked, then $L(G)$ is not empty; otherwise, it is empty. (Example 7.7 Solution).
  
  *   **Important Note:** While $A_{CFG}$ and $E_{CFG}$ are decidable, other problems like the **equivalence problem for CFGs ($EQ_{CFG}$)** (Is $L(G_1) = L(G_2)$?) are **undecidable**.
  ---
- **8. Language Class Relationship Diagram (Sipser Fig 4.10)**
  
  *   This diagram visually shows the hierarchy:
    *   Regular Languages $\subset$ Context-Free Languages $\subset$ Decidable Languages $\subset$ Turing-Recognizable Languages $\subset$ All Languages.
    *   This illustrates that as we move up the hierarchy, the language classes become more expressive, but fewer problems about them are decidable.
  
  ---
- **Helpful Resources:**
  
  1.  **Wikipedia - Decidability (computability theory):** [https://en.wikipedia.org/wiki/Decidability_(logic)](https://en.wikipedia.org/wiki/Decidability_(logic)) (Note: formal logic perspective, but related)
  2.  **Wikipedia - Undecidable problem:** [https://en.wikipedia.org/wiki/Undecidable_problem](https://en.wikipedia.org/wiki/Undecidable_problem)
  3.  **Sipser Textbook:** Chapter 4 - Decidability.
  4.  **TutorialsPoint - Automata Theory - Decidability:** [https://www.tutorialspoint.com/automata_theory/decidability.htm](https://www.tutorialspoint.com/automata_theory/decidability.htm)
  5.  **Stanford CS103 - Decidability and Recognizability:** [https://web.stanford.edu/class/cs103/notes/Lecture%2019.pdf](https://web.stanford.edu/class/cs103/notes/Lecture%2019.pdf)