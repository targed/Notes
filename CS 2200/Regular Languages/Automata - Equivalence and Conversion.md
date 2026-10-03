- **1. Equivalence of DFAs and NFAs (Section 2.4)**
  
  *   **Fundamental Result:** DFAs and NFAs are **equivalent** in their expressive power. They both recognize exactly the same class of languages: the **regular languages**.
  *   **Theorem 1:** Let L be a set (language) accepted by a Non-deterministic Finite Automaton (NFA). Then there exists a Deterministic Finite Automaton (DFA) that also accepts L.
  *   **Significance:** This means that although NFAs offer more flexibility in design (non-determinism, $\epsilon$-moves), they do not allow us to recognize languages that DFAs cannot. Any language describable by an NFA can also be described by a DFA (though the DFA might have significantly more states).
- **2. Converting NFA to DFA (Subset Construction) (Section 2.5)**
  
  *   **Goal:** To construct a DFA $M_D = (Q_D, \Sigma, \delta_D, q_{D0}, F_D)$ that is equivalent to a given NFA $M_N = (Q_N, \Sigma, \delta_N, q_{N0}, F_N)$.
  *   **The Core Idea (Subset Construction):** Each state in the constructed DFA ($Q_D$) corresponds to a *set* of states from the original NFA ($Q_N$).
  *   **Algorithm Steps:**
  1.  **States ($Q_D$):** The set of states $Q_D$ is the power set of $Q_N$, i.e., $\mathcal{P}(Q_N)$. If $Q_N$ has $n$ states, $Q_D$ can have up to $2^n$ states. *Practical Approach:* Start with the initial state and only generate states that are reachable from it.
  2.  **Input Alphabet ($\Sigma$):** The alphabet remains the same.
  3.  **Start State ($q_{D0}$):** The start state of the DFA is the set containing only the start state of the NFA, $q_{D0} = \{q_{N0}\}$. (If the NFA had $\epsilon$-transitions from the start state, the DFA start state would be the $\epsilon$-closure of $q_{N0}$ - see section on $\epsilon$-transitions below).
  4.  **Final States ($F_D$):** A state $S \in Q_D$ (where $S$ is a set of NFA states, $S \subseteq Q_N$) is a final state in the DFA if $S$ contains *at least one* final state from the original NFA ($S \cap F_N \neq \emptyset$).
  5.  **Transition Function ($\delta_D$):** For each state $S \in Q_D$ (represented as a set $\{q_1, q_2, \dots, q_k\} \subseteq Q_N$) and for each input symbol $a \in \Sigma$, the transition $\delta_D(S, a)$ is computed as follows:
      *   Find the set of all states reachable in the NFA from *any* state $q_i \in S$ on input $a$.
      *   $\delta_D(S, a) = \bigcup_{q_i \in S} \delta_N(q_i, a)$.
      *   The result is a *new set* of NFA states, which corresponds to a *single state* in the DFA. Add this new state to $Q_D$ if it hasn't been processed yet.
  *   **Example 2.18 (Fig 2.25 NFA -> Fig 2.26 DFA):**
  *   NFA states: $\{q_0, q_1\}$. NFA Start: $q_0$. NFA Final: $\{q_1\}$.
  *   DFA states can be: $\emptyset, \{q_0\}, \{q_1\}, \{q_0, q_1\}$.
  *   DFA Start State: $\{q_0\}$.
  *   Transitions:
      *   $\delta_D(\{q_0\}, 0) = \delta_N(q_0, 0) = \{q_0, q_1\}$. New state $\{q_0, q_1\}$.
      *   $\delta_D(\{q_0\}, 1) = \delta_N(q_0, 1) = \{q_1\}$. New state $\{q_1\}$.
      *   $\delta_D(\{q_1\}, 0) = \delta_N(q_1, 0) = \{q_1\}$. State $\{q_1\}$ exists.
      *   $\delta_D(\{q_1\}, 1) = \delta_N(q_1, 1) = \{q_0, q_1\}$. State $\{q_0, q_1\}$ exists.
      *   $\delta_D(\{q_0, q_1\}, 0) = \delta_N(q_0, 0) \cup \delta_N(q_1, 0) = \{q_0, q_1\} \cup \{q_1\} = \{q_0, q_1\}$.
      *   $\delta_D(\{q_0, q_1\}, 1) = \delta_N(q_0, 1) \cup \delta_N(q_1, 1) = \{q_1\} \cup \{q_0, q_1\} = \{q_0, q_1\}$.
  *   DFA Final States: Any state containing $q_1$. So, $F_D = \{\{q_1\}, \{q_0, q_1\}\}$.
  *   *(Note: The $\emptyset$ state is often omitted if unreachable, as in this example).*
  *   **Examples 2.19 & 2.20 follow the same process.** Example 2.19 highlights how many potential states ($2^3=8$) might exist, but only reachable ones are included in the final simplified DFA (Fig 2.28).
- **3. NFA with Epsilon ($\epsilon$) Transitions (Section 2.6)**
  
  *   **Definition:** An extension of the NFA model where transitions can occur *without consuming an input symbol*. These are labeled with $\epsilon$.
  *   **Purpose:** Often used for convenience in constructing NFAs, particularly when combining smaller NFAs or converting from regular expressions. They allow joining states or making parts of an automaton optional.
  *   **Expressive Power:** Adding $\epsilon$-transitions **does not** increase the expressive power of NFAs. Any language recognized by an $\epsilon$-NFA can also be recognized by an NFA without $\epsilon$-transitions (and therefore also by a DFA). The languages they accept are still the regular languages.
  *   **Example 2.21 (Fig 2.30):** An $\epsilon$-NFA accepting $0^*1^*2^*$. The $\epsilon$-moves allow transitioning from processing 0s to processing 1s, and from 1s to 2s, without needing an input symbol.
- **4. Epsilon Closure ($\epsilon$-closure) (Section 2.6.1)**
  
  *   **Definition:** For a state $q$ in an $\epsilon$-NFA, $\epsilon$-closure($q$) is the set of all states reachable from $q$ by following *zero or more* $\epsilon$-transitions. Crucially, $q$ itself is always included in $\epsilon$-closure($q$).
  *   **Purpose:** It captures the set of states an NFA could *potentially* be in at a certain point, just due to $\epsilon$-moves, before processing the next *actual* input symbol.
  *   **Example (Fig 2.30):**
  *   $\epsilon$-closure($q_0$) = $\{q_0, q_1, q_2\}$ (Can reach $q_1$ and $q_2$ from $q_0$ via $\epsilon$).
  *   $\epsilon$-closure($q_1$) = $\{q_1, q_2\}$ (Can reach $q_2$ from $q_1$ via $\epsilon$).
  *   $\epsilon$-closure($q_2$) = $\{q_2\}$ (No outgoing $\epsilon$-transitions).
- **5. Eliminating $\epsilon$-Transitions ($\epsilon$-NFA to NFA) (Section 2.6.3)**
  
  *   **Goal:** Convert an NFA with $\epsilon$-transitions $M_\epsilon = (Q, \Sigma, \delta_\epsilon, q_0, F_\epsilon)$ into an equivalent NFA without $\epsilon$-transitions $M = (Q, \Sigma, \delta, q_0, F)$.
  *   **Algorithm Steps:**
  1.  **States & Alphabet:** $Q$ and $\Sigma$ remain the same.
  2.  **Start State:** $q_0$ remains the same.
  3.  **Final States ($F$):** The new set of final states $F$ includes all original final states $F_\epsilon$, PLUS any state whose $\epsilon$-closure contains a state from $F_\epsilon$. (If $q \in Q$ and $\epsilon$-closure($q$) $\cap F_\epsilon \neq \emptyset$, then $q \in F$).
  4.  **Transition Function ($\delta$):** For each state $q \in Q$ and each *non-epsilon* input symbol $a \in \Sigma$, compute the new transition $\delta(q, a)$ as follows:
      *   Find all states reachable from $q$ using only $\epsilon$-moves: $S = \epsilon$-closure($q$).
      *   Find all states reachable from any state in $S$ by following a transition on symbol $a$: $T = \delta_\epsilon(S, a) = \bigcup_{p \in S} \delta_\epsilon(p, a)$.
      *   Find all states reachable from any state in $T$ using only $\epsilon$-moves: $\delta(q, a) = \epsilon$-closure($T$) = $\bigcup_{r \in T} \epsilon$-closure($r$).
      *   In shorthand: $\delta(q, a) = \epsilon\text{-closure}(\delta_\epsilon(\epsilon\text{-closure}(q), a))$.
  *   **Example (Fig 2.34 -> Fig 2.35):**
  *   States $q_0, q_1, q_2$. Final state $F_\epsilon = \{q_2\}$.
  *   $\epsilon$-closures: $\epsilon\text{-closure}(q_0)=\{q_0,q_1,q_2\}$, $\epsilon\text{-closure}(q_1)=\{q_1,q_2\}$, $\epsilon\text{-closure}(q_2)=\{q_2\}$.
  *   New Final States $F$: $q_2 \in F_\epsilon$. $\epsilon\text{-closure}(q_1)$ contains $q_2$, so $q_1 \in F$. $\epsilon\text{-closure}(q_0)$ contains $q_2$, so $q_0 \in F$. Thus $F = \{q_0, q_1, q_2\}$.
  *   New Transitions (Symbol '0'):
      *   $\delta(q_0, 0) = \epsilon\text{-closure}(\delta_\epsilon(\epsilon\text{-closure}(q_0), 0)) = \epsilon\text{-closure}(\delta_\epsilon(\{q_0,q_1,q_2\}, 0)) = \epsilon\text{-closure}(\delta_\epsilon(q_0,0) \cup \delta_\epsilon(q_1,0) \cup \delta_\epsilon(q_2,0)) = \epsilon\text{-closure}(\{q_0\} \cup \emptyset \cup \emptyset) = \epsilon\text{-closure}(\{q_0\}) = \{q_0, q_1, q_2\}$.
      *   $\delta(q_1, 0) = \epsilon\text{-closure}(\delta_\epsilon(\epsilon\text{-closure}(q_1), 0)) = \epsilon\text{-closure}(\delta_\epsilon(\{q_1,q_2\}, 0)) = \epsilon\text{-closure}(\emptyset \cup \emptyset) = \epsilon\text{-closure}(\emptyset) = \emptyset$.
      *   $\delta(q_2, 0) = \epsilon\text{-closure}(\delta_\epsilon(\epsilon\text{-closure}(q_2), 0)) = \epsilon\text{-closure}(\delta_\epsilon(\{q_2\}, 0)) = \epsilon\text{-closure}(\emptyset) = \emptyset$.
  *   *(The full calculation for symbols 1 and 2 follows this pattern, yielding the transitions in the table/Fig 2.35).*
- **6. Converting $\epsilon$-NFA to DFA (Section 2.6.4)**
  
  *   This combines the ideas of $\epsilon$-closure and subset construction.
  *   **Algorithm Steps:**
  1.  **States ($Q_D$):** Subsets of $Q_\epsilon$.
  2.  **Start State ($q_{D0}$):** The $\epsilon$-closure of the $\epsilon$-NFA's start state: $q_{D0} = \epsilon$-closure($q_{\epsilon 0}$).
  3.  **Final States ($F_D$):** A DFA state $S \in Q_D$ is final if it contains any final state of the original $\epsilon$-NFA ($S \cap F_\epsilon \neq \emptyset$).
  4.  **Transition Function ($\delta_D$):** For a DFA state $S$ (representing a set of $\epsilon$-NFA states) and input symbol $a \in \Sigma$:
      *   Find all states reachable from *any* state $p \in S$ using symbol $a$ in the $\epsilon$-NFA: $T = \bigcup_{p \in S} \delta_\epsilon(p, a)$.
      *   The next DFA state is the $\epsilon$-closure of all states in $T$: $\delta_D(S, a) = \bigcup_{r \in T} \epsilon$-closure($r$).
  *   *Practical Approach:* Start with $q_{D0}$ and compute transitions only for reachable DFA states.
  *   **Example (pages 13-15):** Shows this direct conversion process step-by-step for the $\epsilon$-NFA from page 10 (Fig 2.34). It computes the $\epsilon$-closure of the start state $\{q_0, q_1, q_2\}$ as the initial DFA state, then iteratively computes transitions and $\epsilon$-closures for newly reached sets of states ($\{q_1, q_2\}, \{q_2\}, \emptyset$).
  
  ---
- **Helpful Resources:**
  
  1.  **Wikipedia - Powerset construction:** [https://en.wikipedia.org/wiki/Powerset_construction](https://en.wikipedia.org/wiki/Powerset_construction) (Covers NFA to DFA).
  2.  **Wikipedia - Nondeterministic finite automaton (Section on $\epsilon$-moves):** [https://en.wikipedia.org/wiki/Nondeterministic_finite_automaton#NFA_with_%CE%B5-moves](https://en.wikipedia.org/wiki/Nondeterministic_finite_automaton#NFA_with_%CE%B5-moves)
  3.  **JFLAP Tutorial - NFA to DFA Conversion:** [https://www.jflap.org/tutorial/fa/nfa2dfa/index.html](https://www.jflap.org/tutorial/fa/nfa2dfa/index.html)
  4.  **JFLAP Tutorial - NFA Epsilon-Move Removal:** [https://www.jflap.org/tutorial/fa/removeEps/index.html](https://www.jflap.org/tutorial/fa/removeEps/index.html)
  5.  **TutorialsPoint - Automata Theory - NFA to DFA Conversion:** [https://www.tutorialspoint.com/automata_theory/nfa_to_dfa_conversion.htm](https://www.tutorialspoint.com/automata_theory/nfa_to_dfa_conversion.htm)