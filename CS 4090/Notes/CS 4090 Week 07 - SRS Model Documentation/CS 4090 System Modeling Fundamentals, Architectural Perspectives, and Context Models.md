## 1. Why Model? The Purpose of Software Design
  
  Before writing code or defining database tables, software engineers construct models. Beginners often view modeling as administrative overhead, assuming that writing code immediately is faster. However, coding without an architectural model leads to structural rework.
  
  Slides 4 through 14 illustrate why design models are necessary using three foundational analogies:
  
  ```
  +--------------------------------------------------------------------------+
  |                  THE THREE ANALOGIES OF SYSTEM MODELING                  |
  +---------------------+----------------------------------------------------+
  | Analogy             | Core Software Engineering Lesson                   |
  +---------------------+----------------------------------------------------+
  | 1. "Draw an         | An artist draws shading and texture; an engineer   |
  |    Elephant"        | draws orthographic projections (top, front, side   |
  |    (Slides 4–6)     | views with measurements). Models are deliberate    |
  |                     | abstractions tailored to an engineering purpose.   |
  +---------------------+----------------------------------------------------+
  | 2. "The House       | Requirements specify: "3 bedrooms, 2 bathrooms,    |
  |    Project"         | 2 stairs." Both a suburban ranch house (House 1)   |
  |    (Slides 7–10)    | and a three-story alpine cabin (House 2) satisfy   |
  |                     | those exact requirements. Requirements alone do    |
  |                     | not dictate the solution; design bridges the gap.  |
  +---------------------+----------------------------------------------------+
  | 3. "The 1-Liter     | Requirements specify: "Hold 1 liter of liquid."    |
  |    Water Bottle"    | Solutions range from a disposable plastic bottle to|
  |    (Slides 11–12)   | a stainless steel vacuum flask or glass carboy.    |
  |                     | Design narrows down the problem to the next level  |
  |                     | of concrete operational information (Slide 13).    |
  +---------------------+----------------------------------------------------+
  ```
### The Definition of System Modeling (Slide 16)
  > **System modeling is the process of developing abstract models of a system, with each model presenting a different view or perspective of that system.**
  
  In modern practice, system modeling is standardized using the **Unified Modeling Language (UML)**. It serves two distinct communication channels:
  1. It helps the systems analyst understand the internal functionality of the system.
  2. It provides a visual medium to communicate operational boundaries with non-technical customers and stakeholders.
  
  ---
## 2. The Three Uses of Graphical Models
  
  Slide 19 establishes that a graphical model's required level of completeness and correctness depends entirely on how the engineering team intends to use it:
  
  ```
  +--------------------------------------------------------------------------+
  |                 HOW GRAPHICAL MODELS ARE USED (SLIDE 19)                 |
  +--------------------+--------------------------------+--------------------+
  | Intended Usage     | Completeness Standard          | Correctness Rule   |
  +--------------------+--------------------------------+--------------------+
  | 1. Facilitating    | Incomplete models are OK.      | Incorrect models   |
  |    Discussion      | Used as whiteboard sketches to | are acceptable as  |
  |                    | spark debate and explore ideas.| discussion tools.  |
  +--------------------+--------------------------------+--------------------+
  | 2. Documenting an  | Need NOT be complete.          | MUST be an         |
  |    Existing        | Omitting minor helper classes  | accurate           |
  |    System          | keeps architectural documents  | representation of  |
  |                    | readable and maintainable.     | what is running.   |
  +--------------------+--------------------------------+--------------------+
  | 3. Detailed System | MUST be complete.              | MUST be completely |
  |    Description for | Every attribute, type, and     | correct and        |
  |    Implementation  | method signature must be       | executable (code   |
  |    (Code Gen)      | specified without ambiguity.   | generation baseline)|
  +--------------------+--------------------------------+--------------------+
  ```
  
  ---
## 3. The Four Core Architectural Perspectives
  
  A complex software system cannot be understood through a single diagram. An architecture requires multiple distinct viewpoints, each answering a different engineering question.
  
  Slide 17 defines the **four foundational system perspectives**:
  
  ```
                             +------------------------+
                             |   SYSTEM PERSPECTIVES  |
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  |    EXTERNAL      |            |    STRUCTURAL    |            |   INTERACTION    |
  |   PERSPECTIVE    |            |   PERSPECTIVE    |            |   PERSPECTIVE    |
  +------------------+            +------------------+            +------------------+
  | Models context   |            | Models static    |            | Models message   |
  | and environment  |            | organization &   |            | passing between  |
  | outside the app. |            | data structures. |            | actors & parts.  |
  +------------------+            +------------------+            +------------------+
                                         |
                                         v
                                +------------------+
                                |    BEHAVIORAL    |
                                |   PERSPECTIVE    |
                                +------------------+
                                | Models dynamic   |
                                | runtime states & |
                                | event responses. |
                                +------------------+
  ```
### Mapping Perspectives to UML Diagram Types (Slide 18)
  Slide 18 establishes the formal relationship between architectural perspectives and specific UML diagrams:
  
  1. **External Perspective $\rightarrow$ Context Models / Use Case Diagrams:** Shows what lies outside the software perimeter and how the system interfaces with its operational environment.
  2. **Structural Perspective $\rightarrow$ Class Diagrams:** Shows object classes, data attributes, operations, and the static associations linking them together.
  3. **Interaction Perspective $\rightarrow$ Sequence Diagrams & Use Case Diagrams:** Shows how actors communicate with system components and how internal components exchange messages over time.
  4. **Behavioral Perspective $\rightarrow$ State Diagrams & Activity Diagrams:** Shows how the system transitions between runtime states in response to internal and external events, and how data flows through a process.
  
  ---
## 4. Context Models and System Boundaries
  
  Slides 20 through 23 address how an engineering team establishes where their software begins and ends.
### What is a Context Model?
  A **context model** illustrates the operational context of a system. It explicitly shows **what lies outside the system boundaries** (Slide 21).
### The Political Reality of System Boundaries (Slide 22)
  Slide 22 presents a critical insight for practicing engineers:
  > **Defining a system boundary is a political judgment.**
  
  System boundaries are not purely technical decisions:
  * Social, organizational, and managerial pressures dictate where boundaries are drawn.
  * Positioning a boundary changes workloads: If an automated room-booking feature is placed *inside* your system, the campus facilities department loses manual booking oversight but offloads clerical labor.
  * Department managers often push to expand system boundaries to increase their budgetary influence, or push to shrink boundaries to avoid accountability for edge-case failures.
  
  ---
### Case Study: The Mentcare Context Model (Slide 23)
  Slide 23 illustrates an architectural context model for the Mentcare healthcare system. The central application sits at the core, surrounded by external enterprise systems that it must interface with across network boundaries:
  
  ```
  +--------------------------------------------------------------------------+
  |                  THE CONTEXT OF THE MENTCARE SYSTEM (SLIDE 23)           |
  +--------------------------------------------------------------------------+
  |                                                                          |
  |                   +-------------------------------+                      |
  |                   |           «system»            |                      |
  |                   |     Patient Record System     |                      |
  |                   +---------------+---------------+                      |
  |                                   |                                      |
  | +-------------------+             |             +----------------------+ |
  | |     «system»      |             |             |       «system»       | |
  | |    Management     +-------------+-------------+      Admissions      | |
  | | Reporting System  |             |             |        System        | |
  | +-------------------+             |             +----------------------+ |
  |                                   v                                      |
  |                       +-----------------------+                          |
  |                       |       «system»        |                          |
  |                       |       Mentcare        |                          |
  |                       +-----------+-----------+                          |
  |                                   |                                      |
  | +-------------------+             |             +----------------------+ |
  | |     «system»      |             |             |       «system»       | |
  | |   HC Statistics   +-------------+-------------+     Prescription     | |
  | |      System       |             |             |        System        | |
  | +-------------------+             |             +----------------------+ |
  |                                   v                                      |
  |                   +-------------------------------+                      |
  |                   |           «system»            |                      |
  |                   |      Appointments System      |                      |
  |                   +-------------------------------+                      |
  +--------------------------------------------------------------------------+
  ```
  
  * **Architectural Scope:** The Mentcare application does not store primary patient medical histories directly; it integrates with an external `Patient Record System`.
  * **Subsystem Partitioning:** Billing and drug dispensations are offloaded to dedicated `Prescription` and `Admissions` systems. The context model proves that Mentcare is an orchestration hub rather than a monolithic database.
  
  ---
## 5. Transition: Why Use Cases Are Not Enough
  
  Deck 1 (Slides 2 through 6) establishes why a software engineering team cannot transition directly from Use Case Diagrams to source code:
  
  ```
  +--------------------------------------------------------------------------+
  |                  USE CASES VS. CLASS MODELS (DECK 1, SLIDE 4)            |
  +--------------------+-----------------------------------------------------+
  | Dimension          | Use Case Diagrams                                   |
  +--------------------+-----------------------------------------------------+
  | Strengths (Pros)   | • Excellent for capturing functional requirements.  |
  |                    | • Intuitive communication with non-technical users. |
  |                    | • Models user goals and high-level workflows.       |
  +--------------------+-----------------------------------------------------+
  | Weaknesses (Cons)  | • Provides NO information about data structures.    |
  |                    | • Completely hides internal business rules.         |
  |                    | • Fails to specify software components or memory.   |
  +--------------------+-----------------------------------------------------+
  | The Solution:      | Class Diagrams / Structural Models                  |
  | Engineering Role   | • Decomposes the whole system into code components. |
  |                    | • Specifies internal data entities, attributes, and |
  |                    |   low-level type definitions.                       |
  |                    | • Defines programmatic communication links and      |
  |                    |   multiplicities between modules.                   |
  +--------------------+-----------------------------------------------------+
  ```
  
  Use cases explain what the actor wants; **class models define the internal object blueprints required to fulfill those desires.**
  
  ---