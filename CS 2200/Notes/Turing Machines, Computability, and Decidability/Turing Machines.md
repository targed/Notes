- **1. Introduction**
  
  *   **Purpose:** The Turing Machine (TM) is a fundamental, abstract model of computation conceived by Alan Turing in 1936-1937. It serves as a theoretical benchmark or "yardstick" for what can be computed algorithmically.
  *   **Motivation:** Turing aimed to formalize the intuitive notion of an "effective procedure" or "algorithm" – a sequence of simple, mechanical instructions that can be followed to solve a problem or compute a function. He wanted to explore the limits of what is mechanically computable.
  *   **Significance:** TMs are central to the theory of computation, computability theory (what problems can be solved), and complexity theory (how efficiently problems can be solved). They predate modern digital computers but capture their essential computational capabilities.
  ---
- **2. Turing's Assumptions for Computation (Section 6.1)**
  
  *   Turing based his model on analyzing the process of a human "computer" performing calculations. His key assumptions led to the formal TM model:
  1.  **No Creativity/Full Specification:** Each step must be fully spelled out; no intuition required.
  2.  **Finite Instructions:** The set of rules (the program) must be finite.
  3.  **Finite Step Time:** Each individual step must take a finite amount of time.
  4.  **Scratch Pad:** Intermediate results might need to be stored and referenced (leading to the tape).
  5.  **State Tracking:** There must be a way to keep track of the current stage or step of the calculation (leading to finite states).
  6.  **Viewable State:** There must be a way to determine the complete current configuration of the computation (state + tape content + head position).
  ---
- **3. The Turing Machine Model**
  
  *   Based on the assumptions, the model consists of:
  *   **Infinite Tape:** A tape divided into cells, conceptually infinite in one direction (usually right) and bounded on the left. This serves as the machine's memory (the "scratch pad").
  *   **Tape Cells:** Each cell can hold exactly one symbol from a predefined tape alphabet.
  *   **Input:** The input string $w = w_1 w_2 \dots w_n$ is initially written on the leftmost cells of the tape.
  *   **Blank Symbol (B):** All cells beyond the input are initially filled with a special blank symbol $B$.
  *   **Read/Write Head:** A head positioned over one tape cell at a time. It can:
      *   **Read** the symbol in the current cell.
      *   **Write** (or overwrite) a symbol into the current cell.
      *   **Move** one cell to the Left (L) or one cell to the Right (R).
  *   **Finite Control:** A central unit containing a finite set of states ($Q$) and the machine's program (the transition function $\delta$). It determines the machine's actions based on the current state and the symbol read by the head.
  ---
- **4. Formal Definition (Definition 1)**
  
  *   A Turing Machine (TM) is formally defined as a 7-tuple:
  $M = (Q, \Sigma, \Gamma, \delta, q_0, B, F)$
  where:
  *   $Q$: A finite set of **states**.
  *   $\Sigma$: The **input alphabet**, a finite set of symbols allowed in the initial input string. The blank symbol $B$ is *not* in $\Sigma$.
  *   $\Gamma$: The **tape alphabet**, a finite set of symbols that can be written on the tape. It includes the input alphabet ($\Sigma \subseteq \Gamma$) and the blank symbol ($B \in \Gamma$).
  *   $\delta$: The **transition function**: $\delta: Q \times \Gamma \to Q \times \Gamma \times \{L, R\}$.
      *   *Input:* Takes the current state (from $Q$) and the symbol currently under the tape head (from $\Gamma$).
      *   *Output:* Returns a triple $(q', \gamma', d)$:
          *   $q' \in Q$: The next state to transition into.
          *   $\gamma' \in \Gamma$: The symbol to write onto the tape cell currently under the head (replacing the old symbol).
          *   $d \in \{L, R\}$: The direction to move the tape head (Left or Right) one cell.
      *   *Note:* $\delta$ might be a *partial* function; if $\delta(q, a)$ is undefined for some state $q$ and symbol $a$, the machine halts if it enters that configuration.
  *   $q_0 \in Q$: The **start state**.
  *   $B \in \Gamma \setminus \Sigma$: The **blank symbol**.
  *   $F \subseteq Q$: The set of **final states** (or **accept states**). If the machine halts in a state $q \in F$, it accepts the input.
  ---
- **5. How a TM Computes (The Transition Process)**
  
  *   The TM starts in state $q_0$ with the input string $w$ on the leftmost cells of the tape, followed by blanks, and the head positioned over the first (leftmost) symbol of $w$.
  *   At each step, if the machine is in state $q$ and the head reads symbol $a$:
  1.  It looks up the transition $\delta(q, a) = (p, b, d)$.
  2.  It **writes** symbol $b$ in the cell under the head (overwriting $a$).
  3.  It transitions to the **next state** $p$.
  4.  It **moves** the head one cell in direction $d$ (L or R).
  *   This sequence of actions (read, write, move, change state) constitutes one **step** of the computation.
  *   The computation continues step-by-step until the machine **halts**.
  ---
- **6. Instantaneous Description (ID) / Configuration (Section 6.1.1)**
  
  *   An ID captures the complete state of the TM at one instant.
  *   A common way to represent it is $\alpha q \beta$, where:
  *   $q \in Q$ is the current state.
  *   The tape contents consist of the string $\alpha\beta$ (surrounded by infinite blanks, which are usually omitted).
  *   The tape head is positioned over the **first symbol of $\beta$**. $\alpha$ is the string to the left of the head.
  *   *(Text's representation `<l, q, r>`: $q$ is state, $l$ is tape left of head, $r$ is tape at/right of head. Equivalent concept).*
  *   **Initial ID:** For input $w$, the initial ID is $q_0 w$.
  *   **Moves between IDs:** We use $\vdash$ (turnstile) to denote a single move.
  *   **Right Move:** If $\delta(q, a) = (p, b, R)$ and the current ID is $u q a v$, the next ID is $u b p v$. (Head moves right over $v$). Special case: if $v=\epsilon$, ID is $u q a$, next ID is $u b p B$ (a blank is revealed/written).
  *   **Left Move:** If $\delta(q, a) = (p, b, L)$ and the current ID is $u c q a v$, the next ID is $u p c b v$. (Head moves left onto $c$). Special case: If head is on leftmost cell ($u=\epsilon$), ID $q a v$. If $\delta(q,a)=(p,b,L)$, the head cannot move left off the tape; behavior might be defined as halting or staying put, depending on the TM variant. Often assumed not to happen or to cause a halt/crash.
  ---
- **7. Halting Conditions**
  
  *   A TM $M$ **halts** on input $w$ if its sequence of configurations reaches a point where:
  1.  The current state $q$ is a **final state** ($q \in F$). This is usually considered **acceptance**.
  2.  The transition function $\delta(q, a)$ is **undefined** for the current state $q$ and the symbol $a$ under the head. This is usually considered **rejection**.
  *   **Looping:** If the TM never enters a halting configuration, it **loops** (runs forever).
  ---
- **8. Turing Machine as Language Acceptor (Section 6.1.2)**
  
  *   A TM $M$ **accepts** a string $w$ if, starting from the initial configuration $q_0 w$, the computation eventually halts in a state $q \in F$.
  *   A TM $M$ **rejects** a string $w$ if, starting from $q_0 w$, the computation eventually halts in a state $q \notin F$ (or because $\delta$ is undefined).
  *   A TM $M$ **loops** on string $w$ if the computation never halts.
  *   The **language accepted by a TM $M$**, denoted $L(M)$, is the set of all strings $w$ that $M$ accepts:
  $L(M) = \{w \in \Sigma^* \mid M \text{ accepts } w\}$
  *   A language is called **Turing-recognizable** (or recursively enumerable) if there exists a TM that accepts it. (Note: For strings not in the language, the TM might reject or loop).
  *   A language is called **Turing-decidable** (or recursive) if there exists a TM that halts on *all* inputs, accepting strings in the language and rejecting strings not in the language (i.e., it never loops).
  ---
- **9. Turing Machine as Function Computer (Section 6.2)**
  
  *   TMs can also model the computation of functions $f: \Sigma^* \to \Gamma^*$.
  *   **Input:** The input $w$ is placed on the tape ($q_0 w$).
  *   **Output:** If the TM halts (usually required to halt in an accepting state for function computation), the string remaining on the tape, starting from the leftmost cell up to the point where blanks begin, is considered the output $f(w)$.
  *   **Example:** Adding $m$ and $n$ (represented as $0^m$ and $0^n$).
  *   Input: $0^m 1 0^n$ (Unary representation, '1' as separator).
  *   Desired Output: $0^{m+n}$.
  *   A TM could achieve this by, e.g., replacing the '1' with '0', moving to the right end, erasing the last '0'.
  ---
- **10. Representing TMs**
  
  *   **Transition Diagrams:** States are nodes, transitions are edges labeled `read_symbol -> write_symbol, direction`. Example: $a \to b, R$. (See Fig 6.1, 6.2, 6.3).
  *   **Transition Tables:** Rows correspond to states, columns to tape symbols ($\Gamma$). Each cell $(q, a)$ contains the triple $(\delta(q,a))$. (See tables in Fig 6.1, 6.2).
  ---
- **11. Simple Examples**
  
  *   **Example 6.1: Accept $(0+1)^*$** (Accepts any finite string of 0s and 1s).
  *   Logic: Scan right over 0s and 1s until a Blank is found. Accept.
  *   Diagram/Table: Fig 6.1. Start $q_0$. Read 0, write 0, move R, stay $q_0$. Read 1, write 1, move R, stay $q_0$. Read B, write B, move L, go to $q_A$ (final). *(The final L move is technically unnecessary for acceptance but included in the example).*
  
  *   **Example 6.2: 1's Complement** (Flips 0s to 1s and 1s to 0s).
  *   Logic: Scan right, rewriting each 0 as 1 and each 1 as 0. Halt when Blank is encountered.
  *   Diagram/Table: Fig 6.2. Start $q_0$. Read 0, write 1, move R, stay $q_0$. Read 1, write 0, move R, stay $q_0$. Read B, write B, move L, go to $q_A$.
  
  *   **Example 6.3: Adding $m$ and $n$ (as $0^m 1 0^n \to 0^{m+n}$)**
  *   Logic (derived from diagram Fig 6.3): Scan right changing the first '0' to 'B'. Continue right over any more '0's. Change the '1' separator to a '0'. Continue right over the final '0's until Blank. Move left and halt. (Requires $m \ge 1$).
  *   Trace for $B\underline{0}01000B$ ($m=2, n=3$):
      1.  $q_0 B\underline{0}01000B \vdash B q_1 \underline{0}1000B$ (Write B, R)
      2.  $B q_1 \underline{0}1000B \vdash B0 q_1 \underline{1}000B$ (Write 0, R)
      3.  $B0 q_1 \underline{1}000B \vdash B00 q_1 \underline{0}00B$ (Write 0, R)
      4.  $B00 q_1 \underline{0}00B \vdash B000 q_1 \underline{0}0B$ (Write 0, R)
      5.  $B000 q_1 \underline{0}0B \vdash B0000 q_1 \underline{0}B$ (Write 0, R)
      6.  $B0000 q_1 \underline{0}B \vdash B00000 q_1 \underline{B}$ (Write 0, R)
      7.  $B00000 q_1 \underline{B} \vdash B0000 q_A 0 \underline{B}$ (Write B, L)
      8.  Halt (State $q_A$). Tape contains $B00000B$ (which is $B 0^{2+3} B$).
  
  ---
- **Helpful Resources:**
  
  1.  **Wikipedia - Turing machine:** [https://en.wikipedia.org/wiki/Turing_machine](https://en.wikipedia.org/wiki/Turing_machine)
  2.  **Stanford Encyclopedia of Philosophy - Turing Machines:** [https://plato.stanford.edu/entries/turing-machine/](https://plato.stanford.edu/entries/turing-machine/) (More philosophical/foundational perspective).
  3.  **TutorialsPoint - Automata Theory - Turing Machine Introduction:** [https://www.tutorialspoint.com/automata_theory/turing_machine_introduction.htm](https://www.tutorialspoint.com/automata_theory/turing_machine_introduction.htm)
  4.  **JFLAP Tutorial - Turing Machines:** [https://www.jflap.org/tutorial/tm/definition/index.html](https://www.jflap.org/tutorial/tm/definition/index.html) (Allows building and simulating TMs).
  5.  **TuringMachine.io:** [https://turingmachine.io/](https://turingmachine.io/) (Online TM simulator mentioned in source).