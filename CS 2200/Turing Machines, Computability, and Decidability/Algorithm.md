- **1. Informal Notion (Sipser, Church-Turing Thesis notes)**
  
  *   An **algorithm** is intuitively understood as a clear, unambiguous, step-by-step procedure or set of instructions for carrying out a task or solving a problem.
  *   Common synonyms include **procedure** or **recipe**.
  *   Examples from mathematics have existed for centuries (e.g., Euclidean algorithm).
  *   Key characteristics (intuitive):
    *   Finite description (the instructions themselves are finite).
    *   Each step is precisely defined and simple (mechanically executable).
    *   The process yields a result or completes a task.
  ---
- **2. Formalization via Turing Machines (Sipser)**
  
  *   While the intuitive notion existed, a precise, formal definition was needed to prove limitations (i.e., that *no* algorithm exists for certain problems).
  *   **Turing Machines as the Formal Model:** In the theory of computation, the Turing Machine serves as the formal, mathematical model corresponding to the informal notion of an algorithm.
  *   **Church-Turing Thesis Connection:** The thesis posits that *any* task solvable by an intuitive algorithm (an "effective procedure") can be solved by some Turing Machine.
  *   **Focus Shift:** With the TM as the precise model, the focus shifts from studying informal algorithms to studying the capabilities and limitations of Turing Machines. The TM *defines* what is considered algorithmically computable.
  ---
- **3. Describing Turing Machine Algorithms (Sipser - Terminology)**
  
  *   Since programming raw TMs (specifying states, transition function $\delta$ explicitly) is very low-level and complex, we need standardized ways to describe the *algorithms* that TMs implement at higher levels of abstraction.
  *   **Levels of Detail:**
    1.  **Formal Description:** The lowest level. Fully specifies the TM's 7-tuple ($Q, \Sigma, \Gamma, \delta, q_0, B, F$), including all states and transitions. Analogous to machine code. *Rarely used for complex algorithms due to extreme detail.*
    2.  **Implementation Description:** A higher level using English prose combined with TM-specific details. Describes how the TM uses its head and tape to store data and execute the algorithm. Mentions head movement (L/R), reading/writing symbols, scanning the tape, but *omits* explicit states or the transition function $\delta$. Analogous to assembly language or detailed pseudocode. *Provides a good balance for understanding the mechanics.*
    3.  **High-Level Description:** The highest level. Uses English prose to describe the algorithm itself, largely ignoring the TM implementation details (states, tape manipulation, head movement). Focuses on the steps the algorithm performs on the input data. Assumes the reader believes such steps *can* be implemented by a TM. Analogous to high-level programming language description or standard pseudocode. *Most common for describing complex algorithms and proving decidability/recognizability.*
  
  *   **Confidence:** Practicing with lower-level descriptions helps build confidence that high-level descriptions are indeed implementable by TMs. Once confident, high-level descriptions are sufficient and preferred for clarity.
  ---
- **4. Input/Output Format for TMs**
  
  *   **Input:** Always a **string** over the TM's input alphabet $\Sigma$.
  *   **Encoding Objects:** If the algorithm operates on objects other than strings (e.g., graphs, polynomials, automata, numbers), these objects must first be **encoded** as strings.
    *   **Notation:** $\langle O \rangle$ denotes the string encoding of a single object $O$. $\langle O_1, O_2, \dots, O_k \rangle$ denotes the encoding of multiple objects into a single string (e.g., using separators).
    *   **Robustness:** The specific encoding method doesn't usually matter theoretically, as a TM can always be programmed to translate between reasonable encoding schemes.
  *   **TM Decoding:** The TM algorithm described often implicitly includes steps to decode the input string $\langle O \rangle$ back into the structure $O$ it needs to work with.
  ---
- **5. Standard Format for Describing Algorithms (Sipser)**
  
  *   Use indented text within quotes.
  *   Often starts with a line describing the expected input format (e.g., "Input: A string $w$", "Input: $\langle G \rangle$, where G is a graph").
  *   Implicitly assumes the TM first **checks if the input is correctly encoded** in the desired format. If not, it rejects immediately.
  *   Break the algorithm into logical **stages**.
  *   Use further indentation to show block structure (loops, conditionals).
  ---
- **6. Algorithm vs. Computability**
  
  *   The existence of an **algorithm** (as defined formally by a TM that always halts) for a problem means the problem is **decidable**.
  *   If a TM exists but might loop on some inputs, the problem is **Turing-recognizable**.
  *   If no TM can even recognize the language associated with the problem, it is **unrecognizable**.
  *   The Halting Problem shows that not all well-defined problems have an algorithmic solution (i.e., are decidable).
  
  ---
- **Helpful Resources:**
  
  1.  **Wikipedia - Algorithm:** [https://en.wikipedia.org/wiki/Algorithm](https://en.wikipedia.org/wiki/Algorithm) (General definition and history).
  2.  **Sipser Textbook:** Chapter 3 introduces TMs, Chapter 4 discusses Decidability and uses the descriptive levels.
  3.  **Khan Academy - Algorithms:** [https://www.khanacademy.org/computing/computer-science/algorithms](https://www.khanacademy.org/computing/computer-science/algorithms) (Introductory concepts).
  4.  **GeeksforGeeks - Introduction to Algorithms:** [https://www.geeksforgeeks.org/introduction-to-algorithms/](https://www.geeksforgeeks.org/introduction-to-algorithms/)