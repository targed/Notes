## 1. The Five Notations for Writing System Requirements
  
  Slide 12 outlines the five primary notations used in software engineering to document system requirements specifications. Each notation balances a fundamental tradeoff between **human readability** and **mathematical precision**.
  
  ```
  +--------------------------------------------------------------------------+
  |            NOTATIONS FOR SPECIFYING SYSTEM REQUIREMENTS (SLIDE 12)       |
  +-------------------+------------------------------------------------------+
  | Notation          | Description and Engineering Tradeoffs                |
  +-------------------+------------------------------------------------------+
  | 1. Natural        | Requirements are written as numbered sentences in    |
  |    Language       | plain text. Expressive and universally understood,   |
  |    (NL)           | but prone to ambiguity and misinterpretation.        |
  +-------------------+------------------------------------------------------+
  | 2. Structured     | Requirements are written in natural language inside  |
  |    Natural        | a standard form or template. Enforces consistency    |
  |    Language       | while remaining accessible to non-technical clients. |
  +-------------------+------------------------------------------------------+
  | 3. Design         | Uses programming-like syntax (Program Design         |
  |    Description    | Languages, or PDLs) with abstract constructs. Rare   |
  |    Languages      | in modern practice, but useful for defining low-level|
  |                   | hardware interface specifications.                   |
  +-------------------+------------------------------------------------------+
  | 4. Graphical      | Visual models supplemented by text (e.g., UML Use    |
  |    Notations      | Case, Sequence, and State Machine diagrams). Clear   |
  |                   | for showing component interactions and workflows.    |
  +-------------------+------------------------------------------------------+
  | 5. Mathematical   | Formal notations based on set theory, propositional  |
  |    Specifications | logic, or finite-state machines. Completely          |
  |                   | unambiguous, but customers cannot understand them    |
  |                   | and resist signing them as contracts.                |
  +-------------------+------------------------------------------------------+
  ```
  
  ---
## 2. Requirements and Design: In Principle vs. In Practice
  
  Slide 13 addresses a central dilemma in requirements engineering:
  * **In Principle:** Requirements should state **what** the system shall do, while design should describe **how** the system accomplishes it.
  * **In Practice:** Requirements and design are **inseparable**.
  
  ```
                             +------------------------+
                             |   WHY REQUIREMENTS &   |
                             |  DESIGN ARE INSEPARABLE|
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  |   ARCHITECTURAL  |            |  INTEROPERABILITY|            |   NON-FUNCTIONAL |
  |    STRUCTURING   |            |   REQUIREMENTS   |            |    CONSTRAINTS   |
  +------------------+            +------------------+            +------------------+
  | High-level system|            | Connecting to an |            | Strict latency,  |
  | architecture is  |            | existing legacy  |            | privacy, or      |
  | often needed just|            | system or API    |            | security goals   |
  | to organize and  |            | generates rigid  |            | force a specific |
  | decompose the    |            | design-level     |            | architectural    |
  | requirements.    |            | constraints.     |            | topology.        |
  +------------------+            +------------------+            +------------------+
  ```
### The Four Drivers of Practical Design Coupling (Slide 13)
  1. **Architectural Partitioning:** To partition a massive specification into coherent subsystem documents, architects must outline the high-level system architecture first.
  2. **Interoperability Constraints:** If a new system must integrate with an existing relational database or a proprietary payment gateway, the interfaces and data formats of the existing system dictate design constraints directly to the requirements.
  3. **Non-Functional Performance Drivers:** To satisfy an NFR of sub-50ms glass-to-glass latency in game streaming, the system cannot use an arbitrary client-server model. It is forced to adopt a peer-to-peer UDP WebRTC architecture as a domain constraint.
  4. **Regulatory and Legal Requirements:** Government regulations (such as HIPAA in healthcare or DO-178C in avionics) mandate specific design implementations, such as physical separation of sensitive records or hardware watchdog timers.
  
  ---
## 3. Natural Language Specifications: Guidelines and Pitfalls
  
  Slide 14 notes that natural language remains the dominant format for writing requirements because it is **expressive, intuitive, and universal**. It allows non-technical stakeholders, clients, lawyers, and end users to read and comment on specifications without specialized training.
  
  However, natural language is inherently dangerous for engineering contracts if not rigorously disciplined.
### The Five Guidelines for Writing Natural Language Requirements (Slide 16)
  Slide 16 establishes five structural rules to prevent ambiguity:
  
  ```
  +--------------------------------------------------------------------------+
  |               RULES FOR WRITING REQUIREMENTS (SLIDE 16)                  |
  +--------------------------------------------------------------------------+
  | 1. Invent a standard format and use it for ALL requirements.             |
  |    • Every requirement must follow an identical sentence structure.      |
  |                                                                          |
  | 2. Use language consistently (The Shall vs. Should Standard):            |
  |    • Use "SHALL" for mandatory requirements (contractual baseline).      |
  |    • Use "SHOULD" for desirable or non-critical requirements.            |
  |                                                                          |
  | 3. Use text highlighting to identify key elements.                       |
  |    • Bold actors, underline system components, and italicize operations. |
  |                                                                          |
  | 4. Avoid computer jargon.                                                |
  |    • Use domain terminology understood by the client, not coding slang.  |
  |                                                                          |
  | 5. Include an explanation (rationale) for why the requirement exists.    |
  |    • Explaining the "why" prevents future developers from deleting       |
  |      obscure but essential safety or legal constraints.                  |
  +--------------------------------------------------------------------------+
  ```
### The Three Classic Flaws of Natural Language (Slide 17)
  Slide 17 identifies three major structural failures that occur when natural language requirements are poorly written:
  
  ```
  +--------------------------------------------------------------------------+
  |                  NATURAL LANGUAGE PITFALLS (SLIDE 17)                    |
  +---------------------+----------------------------------------------------+
  | Pitfall             | Description and Concrete Example                   |
  +---------------------+----------------------------------------------------+
  | 1. Lack of Clarity  | Precision is difficult without making the sentence |
  |                     | so dense and convoluted that it becomes unreadable.|
  |                     | • Flaw: "The stream shall be fast and clean."      |
  |                     | • Fix: "The video stream shall maintain 60 FPS at  |
  |                     |   1080p resolution with < 60ms latency."           |
  +---------------------+----------------------------------------------------+
  | 2. Requirements     | Functional capabilities (actions) and              |
  |    Confusion        | non-functional qualities (constraints) are         |
  |                     | mixed together in the same paragraph.              |
  |                     | • Flaw: "The user shall log in securely using AES  |
  |                     |   encryption and see a reliable dashboard."        |
  +---------------------+----------------------------------------------------+
  | 3. Requirements     | Several distinct, independent functional rules     |
  |    Amalgamation     | are crammed into a single run-on sentence.         |
  |                     | • Flaw: "The system shall validate the user, log   |
  |                     |   them into the room, send an email confirmation,  |
  |                     |   and decrement the session timer."                |
  |                     | • Rule: One sentence shall express ONE requirement.|
  +---------------------+----------------------------------------------------+
  ```
  
  ---
## 4. Stories and Scenarios: Grounding Requirements in Reality
  
  Slides 18 through 21 explore **scenarios** as a structured mechanism to bring abstract requirements down to concrete operational examples that stakeholders can evaluate.
  
  Slide 19 defines the **five mandatory components of a formal scenario**:
  1. **Starting Situation:** The system state, user roles, and assumptions before the action begins.
  2. **Normal Flow of Events:** The sequential, step-by-step description of typical execution.
  3. **What Can Go Wrong:** The specific failure points, invalid inputs, and error conditions.
  4. **Concurrent Activities:** Other operations running on the system at the same time.
  5. **State on Completion:** The exact state of data, files, and users when the scenario concludes.
  
  ---
### Case Study: Photo Uploading in iLearn / KidsTakePics (Slides 15, 20, 21)
  The slides present a classroom scenario where primary school teacher Jack has pupils upload historic fishing photos. 
  
  ```
  +--------------------------------------------------------------------------+
  |               STRUCTURED SCENARIO: PHOTO UPLOAD (SLIDES 20–21)           |
  +--------------------------------------------------------------------------+
  | 1. Starting Situation (Initial Assumptions):                             |
  |    • User or group has digital photos saved on a tablet or laptop.       |
  |    • Users have successfully authenticated into KidsTakePics.            |
  |                                                                          |
  | 2. Normal Flow:                                                          |
  |    • User selects "Upload Photos" and is prompted to choose local files. |
  |    • User selects target project name and inputs descriptive keywords.   |
  |    • System renames files by combining user name with original filename. |
  |    • System uploads files and dispatches an automated notification email |
  |      to the project moderator requesting review.                         |
  |                                                                          |
  | 3. What Can Go Wrong (Exception Paths):                                  |
  |    • Condition A: No moderator is assigned to the selected project.      |
  |      Action: System generates an alert email to the school administrator |
  |      to assign a moderator; displays an on-screen warning of delays.     |
  |    • Condition B: A photo with an identical name was already uploaded.   |
  |      Action: System prompts user to overwrite, rename, or cancel. If     |
  |      rename is chosen, system appends an incrementing counter suffix.    |
  |                                                                          |
  | 4. Other Concurrent Activities:                                          |
  |    • A project moderator may be actively logged in, reviewing and        |
  |      approving other uploads simultaneously.                             |
  |                                                                          |
  | 5. System State on Completion:                                           |
  |    • User remains logged in.                                             |
  |    • Photos are persisted with status 'Awaiting Moderation'.             |
  |    • Photos are visible only to the uploader and the moderator.          |
  +--------------------------------------------------------------------------+
  ```
  
  ---
## 5. Structured Specifications: Form-Based Requirements
  
  Slide 22 explains that in safety-critical, medical, or embedded systems, natural language is too loose. Engineers use **structured specifications** where the writer's freedom is intentionally restricted by a standard form or template.
### The Standard Form-Based Fields (Slide 23)
  * **Function / Entity Definition:** Unique identifier and concise title.
  * **Description:** Narrative summary of what the service accomplishes.
  * **Inputs and Sources:** Data passed to the function and which sensor, device, or user provided it.
  * **Outputs and Destinations:** Calculated data produced and where it is sent.
  * **Action to be Taken:** Step-by-step mathematical logic or business rules.
  * **Pre- and Post-Conditions:** System state before and after execution.
  * **Side Effects:** Incidental changes to system state (e.g., updating a cache, writing a log).
  
  ---
### Case Study: Automated Insulin Pump (Slides 24–25)
  This case study illustrates how an embedded medical control system specifies safety-critical computation using a structured form:
  
  ```
  +--------------------------------------------------------------------------+
  |             FORM-BASED SPECIFICATION: INSULIN PUMP (SLIDES 24–25)        |
  +-------------------+------------------------------------------------------+
  | Reference         | Insulin Pump/Control Software/SRS/3.3.2              |
  +-------------------+------------------------------------------------------+
  | Function          | Compute insulin dose: safe sugar level.              |
  +-------------------+------------------------------------------------------+
  | Description       | Computes dose of insulin to deliver when the current |
  |                   | measured sugar level is in safe zone (3–7 units).    |
  +-------------------+------------------------------------------------------+
  | Inputs / Source   | Current reading (r2); previous two readings (r0, r1).|
  |                   | Source: Sensor hardware and persistent memory.       |
  +-------------------+------------------------------------------------------+
  | Outputs / Dest    | CompDose (insulin dose to deliver). Dest: Pump motor.|
  +-------------------+------------------------------------------------------+
  | Action            | 1. CompDose is zero if sugar is stable or falling.   |
  |                   | 2. CompDose is zero if sugar is rising but rate of   |
  |                   |    increase is decreasing: ((r2 - r1) < (r1 - r0)).  |
  |                   | 3. If sugar is rising and rate of increase is stable |
  |                   |    or accelerating, CompDose = round((r2 - r1) / 4). |
  |                   | 4. If rounded result = 0, CompDose = MinimumDose.    |
  +-------------------+------------------------------------------------------+
  | Pre-Condition     | Insulin reservoir contains at least the maximum      |
  |                   | allowed single dose.                                 |
  +-------------------+------------------------------------------------------+
  | Post-Condition    | Rolling window update: r0 is replaced by r1; r1 is   |
  |                   | replaced by r2.                                      |
  +-------------------+------------------------------------------------------+
  | Side Effects      | None.                                                |
  +-------------------+------------------------------------------------------+
  ```
  
  ---
## 6. Tabular Specifications: Handling Complex Alternative Logic
  
  Slides 26 and 27 introduce **tabular specifications**. While natural language struggles to express dense mathematical branches without run-on sentences, tables present mutually exclusive decision logic clearly.
### Insulin Pump Decision Table (Slide 27)
  Instead of writing nested "if-else" paragraphs, Slide 27 maps the insulin calculation directly into a condition-action table:
  
  ```
  +------------------------------------------------+---------------------------------+
  | Condition (Blood Sugar Trend)                  | Action (Dose Calculation)       |
  +------------------------------------------------+---------------------------------+
  | Sugar level falling: (r2 < r1)                 | CompDose = 0                    |
  +------------------------------------------------+---------------------------------+
  | Sugar level stable: (r2 = r1)                  | CompDose = 0                    |
  +------------------------------------------------+---------------------------------+
  | Sugar increasing, rate of increase decreasing: | CompDose = 0                    |
  | ((r2 - r1) < (r1 - r0))                        |                                 |
  +------------------------------------------------+---------------------------------+
  | Sugar increasing, rate of increase stable or   | CompDose = round((r2 - r1) / 4) |
  | accelerating: ((r2 - r1) >= (r1 - r0))         | If rounded result = 0:          |
  |                                                |   CompDose = MinimumDose        |
  +------------------------------------------------+---------------------------------+
  ```
### Why Tables are Superior for Complex Logic:
  1. **Exposes Missing Logic:** A table makes gaps immediately visible. If an engineer forgets a condition (e.g., $r_2 = r_1$), an empty row appears in the matrix.
  2. **Deterministic Test Mapping:** QA engineers can convert each row of a specification table directly into an automated unit test case without ambiguity.
  
  ---