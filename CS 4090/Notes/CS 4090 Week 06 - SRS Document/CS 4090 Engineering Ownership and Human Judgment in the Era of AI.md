## 1. The Core Paradox: Acceleration vs. Responsibility
  
  Document 3 establishes the pedagogical framework for using modern Generative AI tools (e.g., ChatGPT, Copilot, Claude) within an engineering capstone. 
  
  Slide 1 defines the governing objective:
  > **The goal is not to compete with AI. The goal is to become an engineer who knows how to use it.**
  
  While AI can generate code, draft documentation, and suggest architectures in seconds, it cannot assume engineering responsibility. 
  
  ```
  +--------------------------------------------------------------------------+
  |                     GENERATED CODE VS. UNDERSTOOD CODE                   |
  +--------------------------------------------------------------------------+
  | AI Generates (Slide 6):                                                  |
  |   • Source code files                                                    |
  |   • Design suggestions and architecture options                          |
  |   • Unit test cases                                                      |
  |   • API documentation and explanations                                   |
  |                                                                          |
  | The Engineer Must Still Understand (Slide 6):                            |
  |   • WHY the solution works (algorithmic correctness).                    |
  |   • WHEN the solution fails (boundary limits and edge cases).             |
  |   • WHAT assumptions the generated code makes about the environment.     |
  |   • WHETHER the code fits the broader system architecture.               |
  |   • HOW the code will be maintained, patched, and debugged five years on.|
  +--------------------------------------------------------------------------+
  ```
### The Culinary Analogy (Slide 2)
  Slide 2 compares AI tools to recipe books:
  * A large language model can suggest ingredients, procedural steps, cooking temperatures, and substitute spices.
  * The human chef must still physically prepare the ingredients, combine them, taste the dish, adjust seasoning, and monitor heat.
  * In software engineering, **instructions are not execution**. Having an automated tool generate lines of code does not mean the system functions reliably in production under network latency, memory pressure, and concurrent user load.
  
  ---
## 2. A Prompt is a Mini-Requirements Document
  
  Slides 3 and 4 connect artificial intelligence directly back to the core principles of requirements engineering: **Prompt engineering is simply requirements specification at micro-scale.**
  
  If an engineer writes a vague, single-sentence prompt (e.g., *"Write a Python script for user authentication"*), the language model is forced to hallucinate missing constraints. It will make arbitrary assumptions about the database, hashing algorithms, session management, and error handling.
  
  ```
  +--------------------------------------------------------------------------+
  |         PROMPTING AS REQUIREMENTS SPECIFICATION (SLIDES 3 & 4)           |
  +-------------------+------------------------------------------------------+
  | Prompt Parameter  | Corresponding SRS Section                            |
  +-------------------+------------------------------------------------------+
  | WHAT?             | Functional Requirements: What exact task must the    |
  |                   | system perform?                                      |
  +-------------------+------------------------------------------------------+
  | WHO?              | Stakeholders & Actors: Who is the user, and what are |
  |                   | their permission boundaries?                         |
  +-------------------+------------------------------------------------------+
  | INPUTS / OUTPUTS  | Interface Specification: What data structures enter  |
  |                   | the function, and what format leaves it?             |
  +-------------------+------------------------------------------------------+
  | CONSTRAINTS       | Non-Functional Requirements: What memory, speed,     |
  |                   | platform, and library limitations exist?             |
  +-------------------+------------------------------------------------------+
  | EXCEPTIONS        | Alternative Flows: How should the code behave when   |
  |                   | network calls drop, disks fill, or inputs are null?  |
  +-------------------+------------------------------------------------------+
  | DONE?             | Acceptance Criteria: How will the engineer verify    |
  |                   | that the returned code is mathematically correct?    |
  +-------------------+------------------------------------------------------+
  ```
  
  *The Takeaway (Slide 4):* **If you cannot explain the requirement clearly to another human, AI cannot reliably infer your intention.** Requirements engineering remains the foundational skill that makes AI tools useful.
  
  ---
## 3. The Hierarchy of Engineering Questions
  
  Slide 5 illustrates how technical maturity changes the way an engineer interacts with external tools, mentors, and AI models:
  
  ```
                          / \
                         /   \
                        /     \
                       / Judg- \       <-- LEVEL 4: KNOWLEDGEABLE ENGINEER
                      /  ment   \          Evaluates trade-offs, failure modes,
                     +-----------+         security, maintainability, and tests.
                    / Trade-offs  \
                   +---------------+   <-- LEVEL 3: ADVANCING PRACTITIONER
                  /    Options      \      Asks for alternatives, trade studies,
                 +-------------------+     and architectural comparisons.
                /       Answer        \
               +-----------------------+ <-- LEVEL 1 & 2: BEGINNER
                                            Simply asks for "the answer" or syntax.
  ```
  
  1. **The Beginner:** Asks for an immediate answer (e.g., *"How do I send a WebRTC packet in C++?"*). Accepts the output without verification.
  2. **The Developing Engineer:** Asks for structural options and concrete code examples (e.g., *"What libraries exist for WebRTC data channels in C++, and how do they differ?"*).
  3. **The Knowledgeable Engineer:** Asks about architectural tradeoffs, constraints, failure recovery, thread safety, and testability (e.g., *"What are the CPU utilization trade-offs between libdatachannel and standard Google WebRTC on low-end dual-core hardware when pushing 60 FPS video?"*).
  4. **Engineering Judgment:** Uses deep domain knowledge to direct, challenge, and rigorously audit the generated solution against system constraints.
  
  ---
## 4. Why Internal Knowledge Matters: Decisions Beyond the Chat Window
  
  Slide 8 challenges the assumption that engineers no longer need to memorize fundamentals because "everything can be looked up in a chat prompt":
  
  ```
  +--------------------------------------------------------------------------+
  |          DECISIONS THAT CANNOT WAIT FOR A CHAT WINDOW (SLIDE 8)          |
  +--------------------------------------------------------------------------+
  | Operational Context:                                                     |
  | Imagine an active production outage, a live security breach, a hardware  |
  | crash during a client demonstration, or a high-pressure team debate.     |
  |                                                                          |
  | The Operational Reality:                                                 |
  | • You may not have an internet connection or access to an LLM.          |
  | • You do not have 20 minutes to type an elaborate context prompt.        |
  | • In high-pressure moments, prior learning becomes immediate judgment.  |
  |                                                                          |
  | The Engineering Rule:                                                    |
  | You cannot outsource every decision. Foundational architectural          |
  | principles, networking concepts, and debugging instincts must already    |
  | live inside your mind.                                                   |
  +--------------------------------------------------------------------------+
  ```
  
  Slide 9 expands this concept by distinguishing between **external information** and **internal mental models**:
  * **Concepts:** Knowing core fundamentals (e.g., memory pointers, race conditions, UDP vs. TCP).
  * **Patterns:** Recognizing familiar architectural structures (e.g., Model-View-Controller, Client-Server, Publisher-Subscriber).
  * **Tradeoffs:** Understanding what is sacrificed when an architectural choice is made (e.g., choosing WebRTC UDP achieves ultra-low latency but sacrifices packet-delivery guarantees).
  * **Failure Modes:** Anticipating where systems break before running code.
  * **Judgment:** Choosing the specific tool that fits the operational context.
  
  ---
## 5. The Build-Break-Debug-Improve Cycle
  
  Slide 10 details the learning engine that converts conceptual understanding into engineering intuition:
  
  ```
  [ 1. BUILD ] ---------> [ 2. BREAK ] ---------> [ 3. DEBUG ] ---------> [ 4. IMPROVE ]
      ^                                                                        |
      |                                                                        v
      +========================================================================+
                    (Repeat until understanding becomes intuition)
  ```
  
  1. **Build:** Write code and implement the architecture.
  2. **Break:** Deliberately stress-test the implementation. Feed it negative inputs, disconnect network cables, terminate processes unexpectedly, and observe how it fails.
  3. **Debug:** Trace through logs, attach debuggers, read stack traces, and isolate the underlying root cause.
  4. **Improve:** Refactor the design, add defensive guard clauses, and write regression tests to guarantee the bug cannot reoccur.
  
  *The AI Role (Slide 10):* Use AI as a **tutor, code reviewer, debugger, and brainstorming partner**, never as a replacement for hands-on practice.
  
  ---
## 6. Dynamic Tool Selection Across Situations
  
  Slide 11 provides a decision matrix demonstrating that true technical expertise is not about mastering a single tool, but knowing which tool fits the operational context:
  
  ```
  +--------------------------------------------------------------------------+
  |              DYNAMIC TOOL SELECTION MATRIX (SLIDE 11)                    |
  +----------------------+--------------------+------------------------------+
  | Operational Situation| What Matters Most  | Appropriate Tools            |
  +----------------------+--------------------+------------------------------+
  | Ambiguous client     | Clarification and  | Stakeholder interviews,      |
  | request              | scope boundaries   | rapid prototypes, AI ideas.  |
  +----------------------+--------------------+------------------------------+
  | Active production    | Diagnostic speed   | System logs, monitoring      |
  | failure              | and system recovery| dashboards, internal team    |
  |                      |                    | knowledge, debuggers.        |
  +----------------------+--------------------+------------------------------+
  | Critical security    | Risk reduction and | Threat modeling, manual code |
  | concern              | empirical evidence | reviews, fuzz testing,       |
  |                      |                    | trusted reference standards. |
  +----------------------+--------------------+------------------------------+
  | Evaluating new       | Learning velocity  | Official documentation,      |
  | technology           | and experimentation| sandboxes, small throwaway   |
  |                      |                    | proof-of-concept prototypes. |
  +----------------------+--------------------+------------------------------+
  ```
  
  ---
## 7. The Practical Human + AI Engineering Loop
  
  Slide 14 formalizes the six-step iterative protocol that engineering teams should use when integrating AI assistants into their daily development:
  
  ```
  +--------------------------------------------------------------------------+
  |            THE HUMAN + AI ENGINEERING LOOP (SLIDE 14)                    |
  +--------------------------------------------------------------------------+
  | 1. THINK:                                                                |
  |    Define the problem, identify constraints, and structure the question  |
  |    before typing a single prompt.                                        |
  |                                                                          |
  | 2. ASK:                                                                  |
  |    Provide explicit context, constraints, inputs, outputs, and edge      |
  |    cases to the model.                                                   |
  |                                                                          |
  | 3. VERIFY:                                                               |
  |    Read the generated output critically. Check assumptions, look for     |
  |    deprecated library calls, and inspect security logic before trusting. |
  |                                                                          |
  | 4. BUILD:                                                                |
  |    Integrate the verified logic into the system codebase yourself before |
  |    claiming that you understand it.                                      |
  |                                                                          |
  | 5. TEST:                                                                 |
  |    Execute comprehensive unit, integration, and boundary tests before    |
  |    declaring the task complete.                                          |
  |                                                                          |
  | 6. REFLECT:                                                              |
  |    Analyze what worked, why a specific defect occurred, and how to      |
  |    structure the next engineering decision better.                       |
  +--------------------------------------------------------------------------+
  ```
  
  ---
## 8. Progression Across the Capstone Sequence
  
  Slides 12, 13, and 15 map how these engineering principles transition across the two-semester capstone:
  
  ```
  +--------------------------------------------------------------------------+
  |                  CAPSTONE I VS. CAPSTONE II PROGRESSION                  |
  +--------------------------------------------------------------------------+
  | Capstone I: Turning Ambiguity into a Plan (Slide 12)                     |
  |   • Focus: Problem Definition -> Stakeholder Elicitation ->              |
  |            Requirements Specification -> System Design -> Prototyping.   |
  |   • AI Role: Exploring design alternatives, generating test ideas, and   |
  |     brainstorming edge cases.                                            |
  |   • Core Skill: Learning to define what belongs in a system.             |
  |                                                                          |
  | Capstone II: Making the System Survive Reality (Slide 13)                |
  |   • Focus: Subsystem Integration -> System Testing -> Security ->        |
  |            Deployment -> Long-Term Maintenance.                          |
  |   • AI Role: Accelerating boilerplate implementation and debugging.      |
  |   • Core Skill: Validating that software works in production under real  |
  |     client constraints, changing requirements, and unscripted data.      |
  +--------------------------------------------------------------------------+
  ```
### The Final Engineering Mandate (Slide 15)
  * **LEARN:** Build fundamental concepts, recognize architectural patterns, and hone professional judgment.
  * **USE:** Use AI tools boldly to explore alternatives, accelerate boilerplate creation, and review code.
  * **OWN:** Assume absolute personal and team responsibility for every architectural decision, security vulnerability, and software outcome delivered.
  
  ---