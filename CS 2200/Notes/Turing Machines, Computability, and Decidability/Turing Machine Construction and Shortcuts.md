- **1. Motivation**
  
  *   Programming TMs directly using only the formal 7-tuple definition (states, transitions) is tedious and akin to low-level assembly programming.
  *   To manage complexity and design TMs for more sophisticated tasks, higher-level conceptual tools and techniques are useful, similar to how high-level programming languages help manage computer programming.
  *   These techniques don't increase the *fundamental power* of TMs but make design and description much easier.
  ---
- **2. Technique 1: Storage in Finite Control (Section 6.3.1)**
  
  *   **Concept:** Utilize the finite state of the TM not just to represent the stage of computation, but also to store a **finite amount of extra data**. This is analogous to registers in a CPU.
  *   **Mechanism:** Define states as pairs or tuples, e.g., $[q, \text{data}]$, where $q$ represents the control aspect and 'data' represents the stored information (e.g., a symbol read, a counter value up to a fixed limit).
  *   **Formalism Adaptation:** If states are pairs $[q, a]$ where $a \in \Sigma$ (or some finite set), the transition function becomes $\delta([q, a_{stored}], b_{read}) = ([p, c_{stored}], d_{write}, \text{Move})$. The transition depends on both the control state $q$ and stored data $a_{stored}$, and it defines the next control state $p$, the next stored data $c_{stored}$, the symbol $d_{write}$ to write on tape, and the head movement.
  *   **Example 6.4: TM accepting $ab^* + ba^*$** (strings starting with 'a' followed by any number of 'b's, OR starting with 'b' followed by any number of 'a's).
  *   **Standard Approach (Fig 6.4 Diagram):** Uses multiple states ($q_0, q_1, q_2, q_A$) to track whether the first symbol was 'a' or 'b' and then loop appropriately.
  *   **Storage-in-Control Approach:** Use fewer control states but store the *first symbol read* in the state itself.
      *   States: Let states be pairs $[q, \text{symbol}]$, where $q \in \{q_{start}, q_{loop}, q_{final}\}$ and symbol $\in \{a, b, B\}$ (B=Blank, initial symbol). Initial state: $[q_{start}, B]$.
      *   **Step 1 (Read first symbol):**
          *   $\delta([q_{start}, B], a) = ([q_{loop}, a], a, R)$ -> Read 'a', store 'a' in state, rewrite 'a', move R.
          *   $\delta([q_{start}, B], b) = ([q_{loop}, b], b, R)$ -> Read 'b', store 'b' in state, rewrite 'b', move R.
      *   **Step 2 (Loop based on stored symbol):**
          *   If state is $[q_{loop}, a]$ (first symbol was 'a'):
              *   $\delta([q_{loop}, a], b) = ([q_{loop}, a], b, R)$ -> Read 'b', stay in loop state, rewrite 'b', move R. (Accepts $ab^*$)
              *   $\delta([q_{loop}, a], B) = ([q_{final}, B], B, L)$ -> Read Blank, go to final state.
              *   $\delta([q_{loop}, a], a) = \text{Reject/Trap}$ (Invalid sequence 'aa...').
          *   If state is $[q_{loop}, b]$ (first symbol was 'b'):
              *   $\delta([q_{loop}, b], a) = ([q_{loop}, b], a, R)$ -> Read 'a', stay in loop state, rewrite 'a', move R. (Accepts $ba^*$)
              *   $\delta([q_{loop}, b], B) = ([q_{final}, B], B, L)$ -> Read Blank, go to final state.
              *   $\delta([q_{loop}, b], b) = \text{Reject/Trap}$ (Invalid sequence 'bb...').
      *   *(Note: The text's state description {q0, qA} x {a, b, B} and transitions match this logic).*
  *   **Advantage:** Can potentially reduce the number of core control states ($q$) by encoding information within the state representation.
  ---
- **3. Technique 2: Multi-track Tape (Section 6.3.2)**
  
  *   **Concept:** Imagine the single TM tape is divided horizontally into a fixed number ($k$) of parallel **tracks**.
  *   **Mechanism:** The tape head reads and writes a $k$-tuple of symbols at each position, one symbol for each track.
  *   **Formalism:** The tape alphabet $\Gamma$ becomes a set of $k$-tuples, e.g., $\Gamma' = \Gamma^k$. A transition might look like $\delta(q, [a, b, c]) = (p, [x, y, z], R)$.
  *   **Application:** Useful for storing related information side-by-side. E.g., one track holds input, another holds scratch work, another holds markers.
  *   **Special Symbols:** Often use boundary markers (like \Phi, \ in the text) on one track to delimit the input or working area. Blank symbol becomes a tuple of blanks, e.g., $[B, B, B]$.
  *   **Example 6.5: Primality Test** (Determine if a number $N$ is prime).
  *   **Setup (3 tracks suggested):**
      *   Track 1: Input number $N$ in binary (e.g., $47 = 101111$), bounded by \Phi, \. \Phi 101111 B B \dots$
      *   Track 2: Current divisor $D$, starting with 2 (binary 10). $B B 10 B B B B B B \dots$
      *   Track 3: Working copy of $N$ used for repeated subtraction. $B 101111 B B B \dots$
  *   **Algorithm:**
      1.  Initialize: Place $N$ on track 1, $D=2$ on track 2, copy $N$ to track 3.
      2.  **Repeated Subtraction:** Repeatedly subtract $D$ (from track 2) from the number on track 3. (This itself is a TM sub-process).
      3.  **Check Remainder:** After subtraction, check the value left on track 3.
          *   If track 3 is exactly 0: $N$ is divisible by $D$. If $D \neq N$, then $N$ is **not prime**. Halt/Reject. (If $D=N$, continue or accept if only prime check needed).
          *   If track 3 is non-zero (and less than $D$): $N$ is not divisible by $D$.
      4.  **Increment Divisor:** Increment the divisor $D$ on track 2 by one.
      5.  **Check Completion:** If $D$ (track 2) equals $N$ (track 1), then no smaller divisors were found. $N$ is **prime**. Halt/Accept.
      6.  **Loop:** If $D < N$, copy $N$ (track 1) to track 3 again, go back to Step 2.
  *   **Example (Fig 6.6 for input 7 (binary 111)):**
      *   Initial: Trk1=\Phi 111, Trk2=$10$, Trk3=$111$.
      *   Subtract 2 (10) from 7 (111) -> Remainder 1. Not divisible.
      *   Increment D: Trk2=$11$. Copy N: Trk3=$111$.
      *   Subtract 3 (11) from 7 (111) -> Remainder 1. Not divisible.
      *   Increment D: Trk2=$100$. Copy N: Trk3=$111$.
      *   Subtract 4 (100) from 7 (111) -> Remainder 3. Not divisible.
      *   Increment D: Trk2=$101$. Copy N: Trk3=$111$.
      *   Subtract 5 (101) from 7 (111) -> Remainder 2. Not divisible.
      *   Increment D: Trk2=$110$. Copy N: Trk3=$111$.
      *   Subtract 6 (110) from 7 (111) -> Remainder 1. Not divisible.
      *   Increment D: Trk2=$111$. Copy N: Trk3=$111$.
      *   Subtract 7 (111) from 7 (111) -> Remainder 0. Divisible.
      *   Check D=N? Yes ($111 = 111$). No smaller divisor found. **Accept (Prime).**
  ---
- **4. Technique 3: Checking off Symbols (Section 6.3.3)**
  
  *   **Concept:** Use an extra track or modify symbols in the alphabet to mark parts of the tape that have been processed or accounted for.
  *   **Mechanism:** Often use a special symbol (like '√' or 'X') on a second track, or replace original symbols (e.g., 'a' becomes 'A', 'b' becomes 'B') to indicate they've been "checked off".
  *   **Application:** Useful for comparing parts of a string or matching counts, common in non-regular language recognition (e.g., $ww$, $a^n b^n$, $ww^R$).
  *   **Example 6.6: Language $L = \{a^i b^i \mid i \ge 1\}$**
  *   **Setup (2 Tracks):**
      *   Track 1: Input string (e.g., `aaabbb`)
      *   Track 2: Blank initially, use '√' as check mark.
      *   Tape symbols are pairs: $[a, B], [b, B], [B, B], [a, \surd], [b, \surd]$.
  *   **Algorithm:**
      1.  Scan right from the start, find the first $[a, B]$. Change it to $[a, \surd]$ (check off first 'a'). Move right.
      2.  Scan right over any $[a, B]$'s and $[a, \surd]$'s until the first $[b, B]$ is found.
      3.  Change $[b, B]$ to $[b, \surd]$ (check off corresponding 'b'). Move left.
      4.  Scan left over any $[b, \surd]$'s, $[a, B]$'s, $[a, \surd]$'s until the rightmost $[a, \surd]$ is found.
      5.  Move one step right. If the symbol is $[a, B]$, go back to Step 1 (repeat the process).
      6.  If the symbol found in Step 5 is $[b, \surd]$, it means all 'a's have been checked off. Scan right over remaining $[b, \surd]$'s. If a $[b, B]$ is found, reject (too many 'b's). If $[B, B]$ is found, Accept (counts matched).
      7.  Handle errors (e.g., finding $[B, B]$ in step 2 before finding a 'b' means not enough 'b's - reject).
  *   *(The diagrams show the state changes and tape modifications for input `aaabbb`)*.
  ---
- **5. Technique 4: Subroutines (Section 6.3.4)**
  
  *   **Concept:** Design TMs in a modular way, like functions or procedures in programming. A TM can act as a subroutine called by another TM.
  *   **Mechanism:**
  *   Define a TM subroutine with a specific **initial state** and one or more **return states**.
  *   The "calling" TM transitions to the subroutine's initial state to "call" it.
  *   The subroutine performs its task and halts in a designated return state.
  *   The calling TM has transitions defined from the subroutine's return state(s) to resume its own computation.
  *   Parameter passing can be simulated by placing values on the tape in agreed locations before calling.
  *   **Example 6.7: Balanced Parentheses `()`** (Accept strings like `()`, `(())`, `()()`, reject `(`, `)(`, etc.)
  *   **Logic:** Match every closing parenthesis ')' with a preceding opening parenthesis '('.
  *   **Algorithm using Subroutine Idea:**
      1.  **Main Loop (State $q_0$):** Scan right looking for ')'. If found, replace it with 'X'. Enter "Find Left Paren" subroutine state ($q_1$). If Blank found first, go to "Verify" state ($q_2$).
      2.  **Subroutine (State $q_1$):** Scan left looking for '('. Ignore 'X's and already matched '('.
      3.  If '(' found (in state $q_1$), replace it with 'X'. Return to main loop state $q_0$, move right. ($q_1$ acts as entry, $q_0$ acts as return point after success).
      4.  If scan left reaches beginning of tape without finding '(', reject (unmatched ')').
      5.  **Verify (State $q_2$):** When Blank was found in $q_0$. Scan left across tape over 'X's. If any '(' is encountered, reject (unmatched '('). If beginning of tape reached, Accept.
  *   *(The text traces `(())()`, showing the process of matching pairs by replacing with 'X'. The state diagram shows states $q_0, q_1, q_2, q_3, q_A$ implementing this logic. $q_1$ is the 'find left paren' state, $q_0$ continues scan right after match, $q_2$ verifies at the end).*
  ---
- **6. Technique 5: Shifting Over (Section 6.3.5)**
  
  *   **Concept:** Create space on the tape (e.g., at the beginning or in the middle) by shifting existing non-blank content to the right.
  *   **Mechanism:** Requires temporary storage in the finite control to hold symbols while shifting.
  *   To shift one symbol right: Read symbol $a$, store $a$ in state, write Blank, move right. Read symbol $b$, store $b$ in state, write $a$ (from state), move right. Repeat.
  *   To shift multiple cells, need to store multiple symbols in the state.
  *   **Example 6.8: Shift data right by two spaces.**
  *   **Logic:** Need to remember two symbols at a time. Use states like $[q, S_1, S_2]$ where $S_1, S_2$ hold symbols. Use 'X' to mark newly created blank space.
  *   **Algorithm (Input 110...):**
      1.  Start state $[q_0, B, B]$. Head on first '1'.
      2.  Read '1'. Store in $S_2$. State $[q_0, B, 1]$. Write 'X'. Move R. (Tape: X 1 0...)
      3.  Head on second '1'. Read '1'. Move $S_2 \to S_1$ (state holds '1'). Store new '1' in $S_2$. State $[q_0, 1, 1]$. Write 'X'. Move R. (Tape: X X 0...)
      4.  Head on '0'. Read '0'. Move $S_2 \to S_1$ (state holds '1'). Store '0' in $S_2$. State $[q_0, 1, 0]$. Write $S_1$ ('1') from previous step. Move R. (Tape: X X 1 0...)
      5.  Head on 'B'. Read 'B'. Move $S_2 \to S_1$ (state holds '0'). Store 'B' in $S_2$. State $[q_0, 0, B]$. Write $S_1$ ('1') from previous step. Move R. (Tape: X X 1 1 B...)
      6.  Head on 'B'. Read 'B'. State $[q_0, 0, B]$. Write $S_1$ ('0') from previous step. Move R. Change state to signify end (e.g., $[q_1, B, B]$). (Tape: X X 1 1 0 B...)
  *   Result: Original "110" is now preceded by "XX" (two blanks) and tape is "XX110". The diagrams trace these state/tape changes.
  
  ---
- **Helpful Resources:**
  
  1.  **Textbooks (Sipser, Hopcroft/Ullman):** These usually cover TM variants and programming techniques.
  2.  **Wikipedia - Turing machine equivalents:** [https://en.wikipedia.org/wiki/Turing_machine_equivalents](https://en.wikipedia.org/wiki/Turing_machine_equivalents) (Discusses multi-tape TMs etc., showing power isn't increased).
  3.  **Online TM Simulators (JFLAP, turingmachine.io):** Experimenting with these techniques helps understanding.
  4.  **University Lecture Notes:** Search for "Turing machine programming techniques", "multi-tape Turing machine", "Turing machine subroutines".