- **1. Introduction**
  
  *   An NFA is the other main type of Finite Automaton.
  *   Its key characteristic is **non-determinism**: for a given state and input symbol (or even without input - using $\epsilon$), there can be *zero, one, or multiple* possible next states.
  *   This allows an NFA to conceptually be in multiple states simultaneously.
  
  **2. Formal Definition (Definition 3)**
  
  *   A Non-deterministic Finite Automaton (NFA) is formally defined as a 5-tuple:
    $M = (Q, \Sigma, \delta, q_0, F)$
    where:
    *   $Q$: A finite, non-empty set of **states**.
    *   $\Sigma$: A finite set called the **input alphabet**.
    *   $\delta$: The **transition function**. $\delta: Q \times (\Sigma \cup \{\epsilon\}) \to \mathcal{P}(Q)$
        *   *Key Property:* This function takes a state and an input symbol *or the empty string $\epsilon$* and returns a **set** of possible next states (the power set $\mathcal{P}(Q)$). This set might be empty ($\emptyset$), contain one state, or contain multiple states.
    *   $q_0 \in Q$: The **initial state** (or start state).
    *   $F \subseteq Q$: The set of **final states** (or accept states).
  
  **3. How NFAs Work**
  
  *   An NFA processes an input string starting from the initial state $q_0$.
  *   At each step (either consuming an input symbol or taking an $\epsilon$-transition), the NFA can transition to *any* of the states specified by the transition function $\delta$ for its *current* state(s).
  *   Conceptually, the NFA explores *all possible computation paths* in parallel. If it's in state $q$ and $\delta(q, a)$ yields $\{p_1, p_2\}$, then upon reading 'a', the NFA is now considered to be in *both* state $p_1$ and state $p_2$. $\epsilon$-transitions allow state changes without reading input.
  
  **4. Acceptance of NFA (Section 2.3.3)**
  
  *   An NFA **accepts** an input string $w$ if, after processing the entire string, ***at least one*** of the possible computation paths ends in a state that belongs to the set of final states $F$.
  *   It rejects the string only if *all* possible paths end in non-final states or get "stuck" (reach a state with no valid transition for the next input/epsilon).
  *   **Example (Fig 2.20): Check if 0100 is accepted.**
    *   The NFA has states $q_0, q_1, q_2$ with $q_0$ as start and $q_2$ as final.
    *   Transitions: $\delta(q_0, 0) = \{q_0, q_1\}$, $\delta(q_0, 1) = \{q_0\}$, $\delta(q_1, 0) = \{q_2\}$, $\delta(q_1, 1) = \emptyset$, $\delta(q_2, 0) = \emptyset$, $\delta(q_2, 1) = \emptyset$.
    *   Let's trace "0100":
        1.  Start: $\{q_0\}$
        2.  Read '0': Apply $\delta(q_0, 0) = \{q_0, q_1\}$. Current states: $\{q_0, q_1\}$.
        3.  Read '1': Apply $\delta$ to *each* current state:
            *   $\delta(q_0, 1) = \{q_0\}$
            *   $\delta(q_1, 1) = \emptyset$
            *   Union = $\{q_0\}$. Current state: $\{q_0\}$.
        4.  Read '0': Apply $\delta(q_0, 0) = \{q_0, q_1\}$. Current states: $\{q_0, q_1\}$.
        5.  Read '0': Apply $\delta$ to *each* current state:
            *   $\delta(q_0, 0) = \{q_0, q_1\}$
            *   $\delta(q_1, 0) = \{q_2\}$
            *   Union = $\{q_0, q_1, q_2\}$. Current states: $\{q_0, q_1, q_2\}$.
    *   End of string. The set of possible final states is $\{q_0, q_1, q_2\}$. Does this set contain an accept state? Yes, $q_2 \in F$.
    *   Therefore, the string "0100" is **accepted**. (The text shows a diagrammatic trace leading to $q_2$).
  
  **5. NFA vs DFA Notes**
  
  *   **Ease of Construction:** NFAs are often easier and more intuitive to design than DFAs for a given language (as stated in the Note and Example 2.17). The ability to have multiple transitions or $\epsilon$-moves simplifies representing choices or optional parts.
  *   **Processing Time:** Processing a string with an NFA can be computationally more complex than with a DFA. While a DFA follows one path, simulating an NFA might involve tracking multiple active states (like in the subset construction) or using backtracking, potentially leading to longer processing times in naive implementations (as stated in the Note). However, this relates to *simulation*; their theoretical *expressive power* is the same.
  
  **6. Examples**
  
  *   **Example 2.15: NFA for $\{abab^n \mid n \ge 0\}$ over $\Sigma=\{a, b\}$.** (This means "ab" followed by zero or more 'b's).
    *   *States:* Need states to track the "ab" prefix and then loop for the $b^n$.
        *   $q_0$: Start state.
        *   $q_1$: After 'a'.
        *   $q_2$: After "ab".
        *   $q_3$: After "aba" (Final state, also handles $b^n$).
    *   *Transitions:*
        *   $\delta(q_0, a) = \{q_1\}$
        *   $\delta(q_1, b) = \{q_2\}$
        *   $\delta(q_2, a) = \{q_3\}$
        *   $\delta(q_3, b) = \{q_3\}$ (Self-loop for $b^n$).
    *   *Final State:* $F = \{q_3\}$.
    *   *Reasoning:* The structure forces the "aba" sequence. The self-loop on the final state $q_3$ allows for any number (including zero) of 'b's at the end, matching $b^n$.
  
  *   **Example 2.16: NFA for strings where the 5th symbol from the right end is '1' over $\Sigma=\{0, 1\}$.**
    *   *States:* Use states to "guess" when the 5th symbol from the end might be occurring and then count down. Need 6 states ($q_0$ to $q_5$).
        *   $q_0$: Start state. "Guessing" phase.
        *   $q_1$: Possible 5th symbol from end was seen ('1').
        *   $q_2$: Possible 4th symbol from end was seen.
        *   $q_3$: Possible 3rd symbol from end was seen.
        *   $q_4$: Possible 2nd symbol from end was seen.
        *   $q_5$: Possible 1st symbol from end was seen (Final state).
    *   *Transitions:*
        *   $\delta(q_0, 0) = \{q_0\}$
        *   $\delta(q_0, 1) = \{q_0, q_1\}$ (Stay in $q_0$ OR guess this '1' is the 5th from end and move to $q_1$).
        *   $\delta(q_1, 0) = \{q_2\}$, $\delta(q_1, 1) = \{q_2\}$ (Consumed 4th from end).
        *   $\delta(q_2, 0) = \{q_3\}$, $\delta(q_2, 1) = \{q_3\}$ (Consumed 3rd from end).
        *   $\delta(q_3, 0) = \{q_4\}$, $\delta(q_3, 1) = \{q_4\}$ (Consumed 2nd from end).
        *   $\delta(q_4, 0) = \{q_5\}$, $\delta(q_4, 1) = \{q_5\}$ (Consumed last symbol).
    *   *Final State:* $F = \{q_5\}$.
    *   *Reasoning:* The non-determinism happens at $q_0$ on input '1'. The NFA guesses if this '1' is the crucial 5th-from-last symbol. If it guesses correctly and the string indeed ends after 4 more symbols, the path reaches $q_5$. If it guesses wrong, that path dies out. If the 5th symbol from the end is '0', the NFA never transitions to $q_1$ at the right time.
  
  *   **Example 2.17: NFA for strings ending in "01" over $\Sigma=\{0, 1\}$.**
    *   *States:* Track the potential ending.
        *   $q_0$: Start state, also means the potential ending isn't "0" or "01".
        *   $q_1$: The last symbol seen was '0'.
        *   $q_2$: The last two symbols seen were "01" (Final state).
    *   *Transitions:*
        *   $\delta(q_0, 0) = \{q_0, q_1\}$ (Stay unsure, or potential start of "01").
        *   $\delta(q_0, 1) = \{q_0\}$ (Ends in 1, but not 01).
        *   $\delta(q_1, 0) = \{q_0, q_1\}$ (Sequence becomes "...00", restart check or treat as potential start).
        *   $\delta(q_1, 1) = \{q_2\}$ (Sequence ends in "01").
        *   $\delta(q_2, 0) = \{q_0, q_1\}$ (Sequence becomes "...010", restart check or potential start).
        *   $\delta(q_2, 1) = \{q_0\}$ (Sequence becomes "...011").
    *   *Final State:* $F = \{q_2\}$.
    *   *Comparison to DFA:* The text contrasts this NFA (Fig 2.23) with the corresponding DFA (Fig 2.24), noting the NFA is simpler to draw/conceptualize.
  
  ---
  
  **Helpful Resources:**
  
  1.  **Wikipedia - Nondeterministic Finite Automaton:** [https://en.wikipedia.org/wiki/Nondeterministic_finite_automaton](https://en.wikipedia.org/wiki/Nondeterministic_finite_automaton)
  2.  **JFLAP Tutorial - Nondeterminism:** [https://www.jflap.org/tutorial/fa/nfa2dfa/nfa.html](https://www.jflap.org/tutorial/fa/nfa2dfa/nfa.html) (Explains and allows building NFAs).
  3.  **TutorialsPoint - Theory of Computation - Non Deterministic Finite Automaton:** [https://www.tutorialspoint.com/automata_theory/non_deterministic_finite_automaton.htm](https://www.tutorialspoint.com/automata_theory/non_deterministic_finite_automaton.htm)