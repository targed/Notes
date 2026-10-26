## 1. What is Software? The Dual Definitions
  
  A central theme across the introductory materials is that source code alone does not constitute software. Code is simply the executable artifact. Software is an integrated sociotechnical asset requiring operational context, procedural rules, and structured data.
  
  ```
  +--------------------------------------------------------------------------+
  |                      WHAT ACTUALLY CONSTITUTES SOFTWARE?                 |
  +--------------------------------------------------------------------------+
  |  Common Misconception: Software = Source Code (.cpp, .py, .java)         |
  |                                                                          |
  |  Formal Engineering Reality:                                             |
  |  Software = Instructions + Data Structures + Procedures + Documentation  |
  +--------------------------------------------------------------------------+
  ```
### Formal Standards: IEEE vs. ISO
  Course slides highlight two authoritative definitions that distinguish amateur scripting from professional software engineering:
  
  * **IEEE Definition:**  
  *Software is: Computer programs, procedures, and possibly associated documentation and data pertaining to the operation of a computer system.*
  * **ISO Definition (ISO 9000-3):**  
  Lists **four mandatory components** required to guarantee quality during development and throughout years of maintenance:
  1. **Computer Programs (Code):** The operational instructions, scripts, and compiled binaries executed by processors.
  2. **Procedures:** The business workflows, deployment recipes, administrative runbooks, and operational guides that dictate how the system is safely used and managed.
  3. **Documentation:** The formal specifications (SRS), architectural diagrams, user manuals, and API reference contracts that allow other engineers to understand and evolve the system.
  4. **Data:** Configuration records, seed data, database initialization files, and operational parameters necessary to boot and run the software system correctly.
  
  ---
## 2. Software Engineering vs. Programming
  
  Slide 2 presents the foundational equation: **Software != Programming**.
  
  ```
  +--------------------------------------------------------------------------+
  |                        THE ENGINEERING EQUATION                          |
  +--------------------------------------------------------------------------+
  | Programming:                                                             |
  |   Writing code to solve an immediate, well-defined algorithmic task.     |
  |   Usually performed by a single developer on a single machine.           |
  |                                                                          |
  | Software Engineering:                                                    |
  |   Programming + Design + Testing + Deployment + Maintenance +            |
  |   Documentation + Collaboration                                          |
  |                                                                          |
  |   Translates to: (Technical Solution) + (People over Time)               |
  +--------------------------------------------------------------------------+
  ```
### Why Programming Alone Fails at Scale
  1. **The Time Dimension:** A programmer writes code that works today on their machine. A software engineer designs code that can be maintained, upgraded, and debugged by different engineers five years from now when the operating system and dependencies have changed.
  2. **The Team Dimension:** A single developer does not need interface contracts or branching strategies because all context lives in their own head. Software engineering introduces architectural abstraction, interface boundaries, and code reviews so dozens of developers can write code concurrently without collision.
  3. **The User Dimension:** Programming focuses on getting the machine to compute the correct output. Software engineering focuses on whether the computed output satisfies the real-world operational need of the client.
  
  ---
## 3. The Software Engineering Framework: People, Process, Technology
  
  Slide 17 and 18 define the triad governing all software production.
  
  ```
                             +------------------------+
                             |         PEOPLE         |
                             | • Roles & Leadership   |
                             | • Collaboration Norms  |
                             | • Stakeholder Dialogue |
                             +-----------+------------+
                                         |
                                         |
            +----------------------------+----------------------------+
            |                                                         |
            v                                                         v
  +------------------------+                               +------------------------+
  |        PROCESS         |                               |       TECHNOLOGY       |
  | • SDLC Process Models  |<----------------------------->| • Languages & Compilers|
  | • Structured Workflows |                               | • Toolchains & IDEs    |
  | • Quality Checkpoints  |                               | • CI/CD & Deploy Infra |
  +------------------------+                               +------------------------+
  ```
### Which Component Is Most Important?
  The slides pose the question: *Which do you think is the most important component and why?*
  
  In software engineering literature, **People** are recognized as the primary determinant of project success or failure:
  * **Technology is interchangeable:** Using modern frameworks will not save a project if the team misunderstands user requirements or cannot communicate internally.
  * **Process serves people, not the reverse:** A well-structured process keeps engineers aligned and prevents chaotic regression, but process alone cannot write creative code or make difficult architectural tradeoffs.
  * **The Failure Chain:** Poor communication (People) leads to conflicting requirements, which leads to incomplete workflows (Process), which results in broken code regardless of the programming language chosen (Technology).
  
  ---
## 4. Why Software Engineering Exists: Four Core Imperatives
  
  Slide 9 outlines four structural reasons why formal software engineering practices are required:
  
  1. **Manage Complexity:** As systems scale, interactions between components grow exponentially ($O(n^2)$ communication paths). Engineering provides modularity, encapsulation, and layered architectures so developers can reason about individual subsystems in isolation.
  2. **Quality Assurance:** Assures that software satisfies user requirements reliably under heavy operational load, unexpected network disconnects, and malformed inputs.
  3. **Efficiency:** Reduces cost and schedule overruns. Defects discovered during initial requirements or design phases cost orders of magnitude less to fix than defects discovered during production deployment.
  4. **Adaptability (Software Evolution):** Supports continuous change. Unlike physical structures, software must continually adapt to new business rules, security patches, hardware architectures, and external API deprecations.
  
  ---
## 5. Sociotechnical Impact and Engineering Ethics
  
  Software is no longer confined to isolated research labs or consumer entertainment; it forms the operational fabric of society.
### Economic and Infrastructure Scale (US Data from Slide 15)
  * In 2020, software supported more than **15.8 million total jobs** in the US economy.
  * The software industry directly contributed **$933 billion** to the US GDP, representing double-digit percentage growth year-over-year.
  * More than **27% of all domestic US business research and development (R&D)** is invested directly into software systems.
  * Critical sectors dependent on software reliability include Healthcare, Finance, Power Grids, Telecommunications, and Commercial Transportation.
  
  ```
  +--------------------------------------------------------------------------+
  |               ETHICAL CASE STUDY: THE THERAC-25 MALFUNCTIONS             |
  +--------------------------------------------------------------------------+
  | Context:                                                                 |
  | Medical dual-mode linear accelerator used for radiation therapy in the   |
  | mid-1980s. The machine administered massive radiation overdoses (up to   |
  | 100 times the intended dose) to multiple patients, causing severe        |
  | injuries and deaths.                                                     |
  |                                                                          |
  | Technical Failure Analysis:                                              |
  | 1. Software Race Condition: The system used a single shared variable to  |
  |    track operator keyboard input and machine setup status. If a fast     |
  |    technician corrected a typing error within an 8-second window, the    |
  |    software set the beam intensity to maximum (25 MeV) without rotating  |
  |    the protective tungsten beam-attenuation target into the physical     |
  |    beam path.                                                            |
  | 2. Removal of Physical Hardware Interlocks: The predecessor machine      |
  |    (Therac-20) had hardware micro-switches that physically blocked the   |
  |    beam if the turntable was misaligned. Engineers removed these hardware|
  |    failsafes in the Therac-25, assuming the software was bug-free.       |
  | 3. Lack of System Testing: The assembly code was never tested with real  |
  |    operators typing at high speeds during integrated hardware trials.    |
  | 4. Poor Error Reporting: The software threw generic codes (e.g.,         |
  |    "Malfunction 54") that were not documented in the operating manual.   |
  |    Operators assumed an error meant no radiation had been delivered and  |
  |    re-triggered the beam.                                                |
  |                                                                          |
  | Core Professional Lessons:                                               |
  | • Software is responsibility. In safety-critical contexts, engineering   |
  |   defects directly impact human life.                                    |
  | • Never rely on software alone for physical safety; always maintain      |
  |   independent hardware failsafes where catastrophic harm is possible.    |
  | • Defensive programming, formal race-condition analysis, and error       |
  |   clarity are fundamental engineering duties.                            |
  +--------------------------------------------------------------------------+
  ```
  
  ---
## 6. Defining Software Quality: IEEE vs. Pressman
  
  Slides 31 in Deck 1 provides the formal frameworks used to evaluate software quality.
### 1. IEEE Quality Definition
  Quality is evaluated across two distinct vectors:
  1. *The degree to which a system, component, or process meets specified requirements.* (Conformity to written technical contracts).
  2. *The degree to which a system, component, or process meets customer or user needs or expectations.* (Fitness for purpose in the field).
### 2. Pressman's Three-Tier Quality Definition
  Roger Pressman expands software quality into three concrete criteria:
  > **Conformance to explicitly stated functional and performance requirements, explicitly documented development standards, and implicit characteristics that are expected of all professionally developed software.**
  
  ```
  +--------------------------------------------------------------------------+
  |                  PRESSMAN'S THREE PILLARS OF QUALITY                     |
  +-------------------+------------------------------------------------------+
  | Quality Pillar    | What It Evaluates                                    |
  +-------------------+------------------------------------------------------+
  | Explicit          | Does the system do what the SRS explicitly states it |
  | Requirements      | shall do? (e.g., login works, calculations match).   |
  +-------------------+------------------------------------------------------+
  | Documented        | Did the team adhere to engineering baselines?        |
  | Standards         | (e.g., code linting, security audits, test suites,   |
  |                   | version control rules, architectural boundaries).    |
  +-------------------+------------------------------------------------------+
  | Implicit          | Does the system satisfy unspoken professional norms? |
  | Characteristics   | (e.g., intuitive usability, reasonable responsiveness,|
  |                   | graceful error recovery, clean maintainability).     |
  +-------------------+------------------------------------------------------+
  ```
  
  If software meets written functional specifications but violates basic maintainability or crashes on unexpected inputs, it still fails the threshold of professional quality.
  
  ---