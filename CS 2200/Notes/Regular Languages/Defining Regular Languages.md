- **1. Background: Languages and Automata**
  
  *   In formal language theory, a **language** is defined as a set of strings, where each string is a finite sequence of symbols drawn from a specific alphabet $\Sigma$.
  *   **Finite Automata (FA)** are simple mathematical models of computation with finite memory. They are used to recognize (or "accept") specific sets of strings, thus defining a language.
  
  **2. Finite Automaton - Formal Definition Recap**
  
  *   As introduced previously, a finite automaton is formally defined as a 5-tuple:
    $M = (Q, \Sigma, \delta, q_0, F)$
    where:
    *   $Q$: A finite set of **states**.
    *   $\Sigma$: A finite set called the **alphabet** (the symbols allowed in input strings).
    *   $\delta$: The **transition function**. In a Deterministic Finite Automaton (DFA), which Sipser uses for this formal definition of computation, $\delta: Q \times \Sigma \to Q$. It takes the current state and the next input symbol and returns the *single* next state.
    *   $q_0 \in Q$: The **start state** (or initial state).
    *   $F \subseteq Q$: The set of **final states** (or accept states).
  
  **3. Formal Definition of Computation (How an FA Accepts a String)**
  
  *   The core idea is to formalize how an FA processes an input string and decides whether to accept or reject it.
  *   Let $M = (Q, \Sigma, \delta, q_0, F)$ be an FA.
  *   Let $w = w_1 w_2 \dots w_n$ be an input string where each $w_i \in \Sigma$.
  *   $M$ **accepts** the string $w$ if and only if there exists a sequence of states $r_0, r_1, \dots, r_n$ in $Q$ that satisfies the following three conditions:
  
    1.  **Start Condition:** $r_0 = q_0$
        *   *Explanation:* The sequence of states *must* begin with the automaton's designated start state before processing any input symbols.
    2.  **Transition Condition:** $\delta(r_i, w_{i+1}) = r_{i+1}$ for all $i = 0, \dots, n-1$
        *   *Explanation:* This is the core processing step. For each symbol $w_{i+1}$ in the string (from the first to the last), the transition function $\delta$, when applied to the current state $r_i$ and the current symbol $w_{i+1}$, *must* yield the next state in the sequence, $r_{i+1}$. This ensures the sequence of states correctly follows the path defined by the input string and the automaton's rules.
    3.  **Accept Condition:** $r_n \in F$
        *   *Explanation:* After processing the *entire* input string (all $n$ symbols), the *final state* reached in the sequence, $r_n$, *must* be one of the states designated as a final (or accept) state in the set $F$.
  
  *   **Summary:** An FA accepts a string if it starts in the start state, follows the transitions dictated by the input symbols one by one, and ends up in an accept state after reading the whole string.
  
  **4. Definition of a Regular Language**
  
  *   **Recognition:** We say that a finite automaton $M$ **recognizes** a language $A$ if the set of all strings that $M$ accepts is precisely the language $A$. Formally:
    $A = \{w \mid M \text{ accepts } w\}$
  *   **Sipser Definition 1.16:** A language is called a **regular language** if some **finite automaton recognizes it**.
    *   *Explanation:* This is the fundamental definition connecting finite automata and the class of languages they characterize. If you can build *any* FA (either a DFA or an NFA, since they are equivalent in power) that accepts exactly the set of strings in a language L, then L is, by definition, a regular language. Conversely, any language accepted by an FA is a regular language.
  
  **5. Key Takeaways**
  
  *   Regular languages form a specific class of formal languages.
  *   The defining characteristic of this class is that its members can be recognized by computational models with strictly *finite* memory (Finite Automata).
  *   The formal definition of FA computation provides a precise way to determine if a given string belongs to the language recognized by a specific FA.
  
  ---
  
  **Helpful Resources:**
  
  1.  **Wikipedia - Regular Language:** [https://en.wikipedia.org/wiki/Regular_language](https://en.wikipedia.org/wiki/Regular_language) (Provides a good overview and different formalisms).
  2.  **Wikipedia - Deterministic Finite Automaton:** [https://en.wikipedia.org/wiki/Deterministic_finite_automaton](https://en.wikipedia.org/wiki/Deterministic_finite_automaton) (Details on DFAs).
  3.  **JFLAP:** [https://www.jflap.org/](https://www.jflap.org/) (Software for experimenting with automata, grammars, and more. Excellent for visualizing FA computation).
  4.  **Stanford CS103 - Regular Languages Notes:** [https://web.stanford.edu/class/cs103/notes/Lecture%2008.pdf](https://web.stanford.edu/class/cs103/notes/Lecture%2008.pdf) (University course notes often offer alternative explanations).
  5.  **TutorialsPoint - Theory of Computation - Regular Languages:** [https://www.tutorialspoint.com/automata_theory/regular_languages.htm](https://www.tutorialspoint.com/automata_theory/regular_languages.htm) (A tutorial-style explanation).