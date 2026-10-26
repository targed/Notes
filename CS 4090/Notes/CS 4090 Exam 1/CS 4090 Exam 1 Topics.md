## Module 1: Foundations of Software Engineering, Ethics, and Team Dynamics
### 1.1 The Definition and Nature of Software
  * **Software versus Source Code:** Source code is merely the executable artifact. Software is an integrated sociotechnical system.
  * **The Software Program Equation:**
  $$\text{Program} = \text{Instructions} + \text{Data Structures} + \text{Documentation}$$
  * **Formal Definitions:**
  * **IEEE Definition:** Computer programs, procedures, and possibly associated documentation and data pertaining to the operation of a computer system.
  * **ISO Definition (ISO 9000-3):** Explicitly lists **four mandatory components** required to assure quality during development and maintenance:
    1. *Computer programs (Code)*: Executable binaries, source code, scripts.
    2. *Procedures*: Operational runbooks, deployment guides, business rules.
    3. *Documentation*: Requirements specifications (SRS), architecture diagrams, user manuals.
    4. *Data*: Configuration parameters, initialization files, seed databases.
### 1.2 Software Engineering versus Programming
  * **The Engineering Equation:**
  $$\text{Software Engineering} = \text{Programming} + \text{Design} + \text{Testing} + \text{Deployment} + \text{Maintenance} + \text{Documentation} + \text{Collaboration}$$
  * **Core Differences:**
  * *Scale*: Programming solves immediate algorithmic problems; Software Engineering builds systems that survive platform changes, scaling, and team turnover.
  * *Time*: Writing code for today versus designing maintainable code for the next five to ten years.
  * *Collaboration*: Moving from solo development to multi-engineer version-controlled systems.
### 1.3 The Software Engineering Triad Framework
  * **The Three Pillars:**
  1. *People*: Roles, leadership, team policies, stakeholder communication.
  2. *Process*: SDLC models, structured workflows, review milestones, quality gates.
  3. *Technology*: Languages, toolchains, frameworks, IDEs, CI/CD runners, cloud hosting.
  * **Primary Determinant:** **People** are the most critical component. Technology and processes cannot compensate for poor team communication or misunderstood requirements.
### 1.4 Why Software Engineering Exists (Four Core Imperatives)
  1. **Manage Complexity:** Using abstraction, modularity, and encapsulation to divide large systems into manageable subsystems.
  2. **Quality Assurance:** Ensuring software reliably satisfies functional and non-functional requirements.
  3. **Cost and Schedule Efficiency:** Catching defects early in the lifecycle to avoid exponential downstream rework costs.
  4. **Adaptability and Evolution:** Designing software architectures that can absorb changing business rules, OS updates, and dependency deprecations.
### 1.5 Software Quality Definitions
  * **IEEE Quality Definition:**
  1. The degree to which a system conforms to specified requirements.
  2. The degree to which a system meets customer or user expectations.
  * **Pressman's Three Pillars of Software Quality:**
  > *Conformance to explicitly stated functional and performance requirements, explicitly documented development standards, and implicit characteristics expected of all professionally developed software.*
  * *Explicit Requirements*: Meeting written contract specifications.
  * *Documented Standards*: Adhering to code style, security baselines, and test coverage rules.
  * *Implicit Characteristics*: Intuitive usability, maintainability, and clean error recovery.
### 1.6 Safety-Critical Systems and Engineering Ethics (Therac-25 Case Study)
  * **The Disaster:** Medical radiation linear accelerator administered massive overdoses (up to 100 times the intended dose) to cancer patients between 1985 and 1987, resulting in fatalities.
  * **Technical Root Causes:**
  * Concurrency bug (software race condition) in PDP-11 assembly code.
  * **Removal of Physical Hardware Interlocks:** Mechanical safety switches from the earlier Therac-20 were removed to save costs, under the assumption that software was bug-free.
  * Inadequate integration testing; system was never tested with real operators typing at high speeds.
  * Cryptic error messages (e.g., "Malfunction 54") that operators bypassed because documentation did not define them.
  * **Core Engineering Lessons:**
  * Software is a professional and ethical responsibility; in safety-critical systems, software directly affects human life.
  * Never rely on software alone for physical safety; maintain independent hardware failsafes.
### 1.7 Human Learning and Engineering Ownership in the Era of AI
  * **Acceleration versus Responsibility:** AI tools generate text and code, but they do not own architecture, security, integration, or outcomes. The engineering team remains accountable.
  * **A Prompt is a Mini-SRS:** High-quality prompts mirror requirements specifications (What, Who, Inputs, Outputs, Constraints, Error Handling, Acceptance Criteria).
  * **Generated $\neq$ Understood:** An engineer must understand why the solution works, when it fails, what assumptions it makes, and how to maintain it.
  * **Decisions Beyond the Chat Window:** High-pressure situations (production outages, live security incidents) require internal mental models, not external chat queries.
  * **The Human + AI Engineering Loop:**
  $$\text{Think} \rightarrow \text{Ask} \rightarrow \text{Verify} \rightarrow \text{Build} \rightarrow \text{Test} \rightarrow \text{Reflect}$$
### 1.8 Team Governance and the Team Charter
  * **The Eight Mandatory Team Policies:**
  1. *Meeting Schedule*: Predictable synchronous cadence and standing agendas.
  2. *Attendance Expectations*: 24-hour advance notice rules and asynchronous status updates.
  3. *Communication Standards*: Platforms used and response time Service Level Agreements (SLAs).
  4. *Task Assignment*: Single-assignee ownership, ticket estimation, and issue tracking.
  5. *Conflict Resolution*: Three-tier escalation (1-on-1 $\rightarrow$ Team Mediation $\rightarrow$ Faculty Intervention).
  6. *GitHub and Version Control*: Branch protection, descriptive branch naming, and pull request review rules.
  7. *Deadline Management*: Internal soft deadlines set 48 to 72 hours before hard course deadlines.
  8. *Quality Standards (Definition of Done)*: Code passing static analysis, compilation, tests, and peer review.
  * **Seven Leadership Roles:** Project Manager / Scrum Master, Product Owner / Requirements Lead, System Architect, Frontend Lead, Backend Lead, QA Lead, DevOps/Docs Lead. (Roles represent leadership domains, not coding limitations).
  
  ---
## Module 2: The Software Development Lifecycle (SDLC) and Process Models
### 2.1 The Nature of Software Process Models
  * **Definition:** An abstract representation of a development process presented from a particular perspective.
  * **The Four Universal Process Activities:**
  1. *Software Specification*: Establishing required services and operational constraints.
  2. *Software Design and Implementation*: Structuring architecture and programming the solution.
  3. *Software Validation*: Verifying that software satisfies specifications and customer needs.
  4. *Software Evolution*: Modifying software to adapt to changing environments and business rules.
  * **Fundamental Process Assumptions:**
  * Good processes lead to good software, reduce risk, enhance visibility, and enable teamwork.
  * Good processes **do not** eliminate changing requirements.
### 2.2 Plan-Driven versus Agile Processes
  * **Plan-Driven:** All activities are planned in advance; progress is measured against documented milestones.
  * **Agile / Incremental:** Planning is continuous and iterative; software adapts rapidly to feedback and technical discoveries.
  * **The Hybrid Reality:** "There are no right or wrong software processes." Most industrial projects blend plan-driven architecture with agile sprint execution.
### 2.3 The Waterfall Model
  * **Characteristics:** Linear, sequential, plan-driven process.
  * **The Five Classical Phases:**
  $$\text{Requirements Analysis} \rightarrow \text{System/Software Design} \rightarrow \text{Implementation/Unit Testing} \rightarrow \text{Integration/System Testing} \rightarrow \text{Operation/Maintenance}$$
  * **The Modified Waterfall Model:** Introduces formal feedback loops between immediately adjacent phases.
  * **Major Drawbacks:**
  * *Inflexible Partitioning*: Difficult to accommodate requirements changes once a phase is complete.
  * *Late Working Software*: Working software appears late in the lifecycle; major integration bugs surface late.
  * *Unrealistic Assumptions*: Assumes requirements can be completely frozen upfront.
  * **Appropriate Contexts:** Stable, well-understood requirements; regulated or safety-critical systems; large projects distributed across multiple subcontracting sites.
### 2.4 Iterative versus Incremental Development
  * **Iterative Development:**
  * *Core Concept*: **Repeated refinement.** Builds the system through repeated cycles, continually refining and improving an existing solution based on feedback.
  * *Example*: Building a basic scheduling tool, then improving its workflow across successive cycles.
  * **Incremental Development:**
  * *Core Concept*: **Adding new functional pieces.** Delivers the system in usable portions over time.
  * *Example*: Releasing login in Month 1, scheduling in Month 2, and reporting in Month 3.
  * **Iterative + Incremental (Modern Agile):** Delivering new functional slices in increments while simultaneously refining earlier increments based on stakeholder feedback.
### 2.5 Tradeoffs of Incremental Development
  * **Benefits:**
  * Reduced cost of accommodating changing requirements.
  * Early, continuous customer feedback on working software.
  * Rapid delivery and early realization of business value.
  * **Structural Problems:**
  * *The Process Visibility Problem*: Rapid builds make it cost-ineffective to write exhaustive documentation for every intermediate version, making progress hard for traditional managers to track.
  * *Architecture Degradation (Code Rot)*: Continual feature additions corrupt internal code structure unless teams explicitly practice **Refactoring** (improving internal structure without changing external behavior).
### 2.6 Component-Based, API-Based, and Tool-Based Development
  * **Core Concepts:**
  * *Component-Based*: Assembling systems from pre-built, reusable software modules (COTS).
  * *API-Based*: Communicating with external services over networks (e.g., Stripe, Google Maps).
  * *Tool-Based*: Using low-code engines, code generators, and AI coding assistants.
  * **Benefits:** Faster time to market, reduced custom coding, lower development costs, and reuse of field-tested capabilities.
  * **Risks:**
  * Vendor lock-in and dependency on third-party service availability.
  * Unannounced API deprecations or pricing changes.
  * Reduced architectural control and security/privacy concerns.
  * **Testing Misconception:** Reusing components **does not** eliminate testing; integration and failure testing remain mandatory.
  * **Build vs. Buy vs. Integrate Tradeoffs:**
  * *Build*: Maximum control; highest time and cost.
  * *Buy*: Fast availability; low customization.
  * *Integrate*: Fast and flexible; introduces external integration dependencies.
  
  ---
## Module 3: Requirements Engineering: Typology, Elicitation, and Specification
### 3.1 The Problem Domain versus the Solution Domain
  * **The Abstraction Pyramid:**
  * *Needs (Problem Domain)*: Real-world operational desires and pain points of stakeholders.
  * *Features (Solution Domain)*: High-level user-facing capabilities addressing needs.
  * *Software Requirements (Solution Domain)*: Detailed, testable specifications implemented by developers and verified by QA.
  * **Formal Requirement Definition:** A capability needed by a user to solve a problem or achieve an objective, which must be possessed by a system to satisfy a contract or standard.
### 3.2 User Requirements versus System Requirements
  * **User Requirements:**
  * High-level natural language statements plus diagrams describing services and constraints.
  * Written for system customers, client managers, end users, and contractor managers.
  * **System Requirements:**
  * Structured document setting out detailed, testable specifications of functions and constraints.
  * Written as a contract baseline for software developers, system architects, and test engineers.
  * **The Readers Matrix:** Developers read System Requirements, not User Requirements, to write code.
  * **Case Study (Mentcare System):**
  * *User Requirement*: The system shall generate monthly management reports showing the cost of drugs prescribed by each clinic.
  * *System Requirements*: Decomposed into 1.1 (end of month summary), 1.2 (print after 17:30), 1.3 (itemized fields: drug name, doses, costs), 1.4 (breakdown by dosage units like 10mg/20mg), and 1.5 (access restricted via ACL).
### 3.3 Functional Requirements (FR)
  * **Definition:** Statements of capabilities, behaviors, and services the system must provide.
  * **Scope:** Defines what the system shall do, how it reacts to specific inputs, and what it **shall not do**.
  * **Standard Syntax:**
  $$\text{The system shall } + \text{action/capability } + \text{object/condition } + \text{qualifier}$$
### 3.4 Non-Functional Requirements (NFR)
  * **Definition:** Constraints on services, system-wide properties, or development standards describing **how well** the system must operate.
  * **Criticality:** NFRs can be **more critical** than functional requirements. If an NFR fails (e.g., severe latency, frequent crashes, security leaks), the entire system becomes unusable.
  * **Cross-Cutting Nature:** NFRs apply across multiple features or dictate overall system architecture. A single NFR can generate multiple concrete functional requirements.
  * **The Complete Sommerville NFR Taxonomy Tree:**
  1. **Product Requirements:**
     * *Usability*: Ease of use, learning curve, accessibility.
     * *Efficiency*: Performance (speed, response time) and Space (memory, disk).
     * *Dependability*: Reliability (MTBF, failure rate) and Availability (uptime %).
     * *Security*: Data protection, access control, encryption.
  2. **Organizational (Process) Requirements:**
     * *Environmental*: Hosting infrastructure, hardware constraints.
     * *Operational*: Administrative runbooks, backup schedules.
     * *Development*: Mandated programming languages, coding standards, IDEs, version control.
  3. **External Requirements:**
     * *Regulatory*: Accounting rules, safety/security certifications.
     * *Legislative*: Privacy laws (HIPAA, GDPR), digital accessibility (ADA).
     * *Ethical*: Unbiased operation, transparent user consent.
### 3.5 Quantifying NFRs: Verifiable Metrics
  * **The Rule of Testability:** Avoid vague, untestable adjectives (*fast, secure, user-friendly, reliable, robust, efficient*). All NFRs must have measurable acceptance criteria:
  * *Speed*: Processed transactions per second, response time (ms), screen refresh rate (FPS).
  * *Size*: Megabytes of memory, disk storage limits, binary package size.
  * *Usability*: Training time (hours), task completion time, help frames accessed.
  * *Reliability*: Mean Time Between Failures (MTBF), availability percentage (e.g., 99.9%).
  * *Robustness*: Restart time after failure (seconds), probability of data corruption.
  * *Portability*: Percentage of target-dependent statements, number of supported operating systems.
### 3.6 Requirements Elicitation and Fact-Finding Techniques
  * **The Elicitation Spiral:**
  $$\text{Discovery} \rightarrow \text{Classification/Organization} \rightarrow \text{Prioritization/Negotiation} \rightarrow \text{Specification}$$
  * **The Five Classic Elicitation Challenges:**
  1. Stakeholders do not know what they really want (tacit knowledge problem).
  2. Stakeholders express requirements in their own domain jargon.
  3. Conflicting requirements exist among different stakeholder classes.
  4. Organizational and political factors influence system boundaries.
  5. Requirements change dynamically during development.
  * **The Pentagon Wars Case Study (Bradley Fighting Vehicle):**
  * Demonstrates feature creep, lack of boundary control, and failure to resolve conflicting stakeholder demands (troop transport $\rightarrow$ adding cannon $\rightarrow$ adding missiles $\rightarrow$ adding armor $\rightarrow$ capacity dropped from 11 men to 6 men).
  * Highlights that requirements engineering requires prioritization and the discipline to say "No."
  * **Fact-Finding Techniques Toolkit:**
  * *Background Reading & Document Sampling*: Reviewing legacy forms, logs, and procedures.
  * *Stakeholder Interviews*: Structured vs. open-ended questions. Best for early discovery.
  * *Observation (Ethnography)*: Watching actual work. Uncovers tacit knowledge, manual workarounds, handoff errors, and process bottlenecks that interviews miss.
  * *Questionnaires / Surveys*: Collecting quantitative data from large user populations.
  * *Workshops & Brainstorming*: Resolving conflicting priorities across departments.
  * *Use Cases & User Stories*: Translating user needs into functional flows.
  * *Storyboarding & Prototyping*: Visualizing workflows to elicit early critique.
### 3.7 Requirements Specification Styles and Formats
  * **Five Specification Notations:**
  1. *Natural Language*: Universal, expressive, but prone to ambiguity.
  2. *Structured Natural Language*: Form-based templates restricting writer freedom.
  3. *Design Description Languages (PDLs)*: Pseudo-code interfaces.
  4. *Graphical Notations*: UML models supplemented by text.
  5. *Mathematical Specifications*: Unambiguous formal logic; difficult for clients to read.
  * **Rules for Natural Language Requirements:**
  * Use a consistent standard format.
  * Use **"shall"** for mandatory contract requirements; use **"should"** for desirable goals.
  * Avoid computer jargon. Include a rationale explaining the "why."
  * **The Three Flaws of Natural Language:**
  * *Lack of Clarity*: Precision is difficult without making text unreadable.
  * *Requirements Confusion*: Mixing functional features with non-functional quality constraints.
  * *Requirements Amalgamation*: Combining multiple discrete requirements into a single run-on sentence.
  * **Structured Scenarios:** Must include:
  1. Starting situation
  2. Normal flow of events
  3. What can go wrong (exception handling)
  4. Concurrent activities
  5. Final state on completion
  * **Structured Forms and Tables:**
  * Form-based fields: Function name, Inputs/Source, Outputs/Destination, Action logic, Pre/Post-conditions, Side effects. (Insulin pump dose computation case study).
  * Tabular specifications: Condition-Action tables used to model branching logic without nested sentences.
### 3.8 The Software Requirements Specification (SRS) Document
  * **The Boundary Principle:** An SRS is **NOT a design document**. It defines **WHAT** the system shall do, not **HOW** it is implemented.
  * **The IEEE Standard 10-Chapter Structure:**
  1. Preface
  2. Introduction
  3. Glossary (Unambiguous acronyms and terms)
  4. User Requirements Definition
  5. System Architecture (High-level modular overview; highlights reused components)
  6. System Requirements Specification (Detailed FRs and NFRs)
  7. System Models (UML diagrams)
  8. System Evolution (Assumptions and anticipated change vectors)
  9. Appendices (Hardware minimums, database schemas)
  10. Index
  
  ---
## Module 4: Storyboards, Prototypes, and Requirements Traceability
### 4.1 Storyboarding Foundations
  * **Purpose:** Address the **"Yes, But" syndrome** early. Stakeholders often cannot evaluate requirements until they see a concrete visual workflow.
  * **The Three Essential Elements of a Storyboard:**
  1. *Who the players are* (User roles, personas).
  2. *What happens to them* (System events, actions, screens).
  3. *How it happens* (Navigation sequence, user input, system feedback).
  * **The Storyboarding Continuum:**
  * *Passive*: Static screenshots, paper sketches, Post-it notes, output reports. Tells a story.
  * *Active*: Slide decks, animations, automated video simulations. Shows a simulated movie.
  * *Interactive*: Clickable wireframes (Figma), HTML mockups. Requires user participation.
  * **The Four Rules of Storyboarding:**
  1. *Don't invest too much*: Keep it sketchy, informal, and visibly unpolished so stakeholders feel comfortable critiquing it.
  2. *If you don't change anything, you don't learn anything*: A storyboard that produces no revision feedback failed to provoke thought.
  3. *Don't make it too functional*: Avoid writing production code or live database hooks.
  4. *Make it interactive whenever possible*: Hands-on interaction reveals cognitive friction and navigation errors.
### 4.2 Prototyping across the Lifecycle
  * **Definition:** An early representation of a proposed system or feature used to explore, validate, and improve ideas.
  * **Fidelity Levels:**
  * *Low-Fidelity*: Paper sketches, simple wireframes. Explores structure quickly.
  * *Mid-Fidelity*: Complete wireframes, clear navigation flows. Refines layouts.
  * *High-Fidelity*: Clickable UI, realistic content. Demonstrates near-final interaction.
  * **Prototype Heuristic:** A prototype is **not the final product**. It may contain incomplete logic, simulated data, and mocked responses. Visual quality alone does not prove software correctness.
### 4.3 End-to-End User Journeys and Error Paths
  * **Standard Workflow Sequence:**
  $$\text{Sign In / Login} \rightarrow \text{Dashboard} \rightarrow \text{Search / Select} \rightarrow \text{Details / Action} \rightarrow \text{Review / Confirmation} \rightarrow \text{Logout}$$
  * **Modeling Beyond the Happy Path:** Real prototypes must demonstrate non-happy paths: validation failures, rejected payments, unavailable resources, and cancellation branches.
### 4.4 Requirements Traceability
  * **Definition:** The ability to link a requirement to its source stakeholder need and track it downstream through design, implementation, and verification artifacts.
  * **The Master Traceability Chain:**
  $$\text{Problem} \rightarrow \text{Need} \rightarrow \text{Requirement (FR/NFR)} \rightarrow \text{Use Case} \rightarrow \text{Storyboard / Prototype} \rightarrow \text{Design} \rightarrow \text{Code} \rightarrow \text{Test Case}$$
  * **The Requirements Traceability Matrix (RTM):** A table mapping Requirement IDs to User Goals, Prototype Screens, Priority levels (MoSCoW), and Test Verification Methods.
  
  ---
## Module 5: UML Use Case Modeling (Interaction Perspective)
### 5.1 Use Case Fundamentals
  * **Formal Definition:** A sequence of actions performed by the system that yields an observable result of value to a particular actor.
  * **The Dual Nature:**
  1. *UML Use Case Diagram*: The high-level visual map of actors and boundaries.
  2. *Use Case Specification (Table / Text)*: The detailed step-by-step contractual flow.
### 5.2 Actor Definitions and Cardinality
  * **Definition:** A role played by a person, organization, device, or external system interacting with the software. Actors remain **outside the system boundary**.
  * **Actor Categories:** Human Users, External Systems (APIs, payment gateways), Hardware Devices (sensors, dispensers, timers).
  * **The Initiation Cardinality Rule:**
  * **Only ONE actor can initiate a use case.**
  * **Many actors can be involved or participate in that use case.**
### 5.3 Use Case Naming Standards
  * Must begin with an **active verb** followed by a specific **noun phrase** describing the actor's goal.
  * *Good Names*: `Schedule Appointment`, `Place Order`, `Withdraw Cash`.
  * *Poor Names*: `Click Schedule Button` (GUI detail), `Appointment Screen` (noun phrase), `Process Data` (vague).
### 5.4 Use Case Relationships
  * **Association Line (Solid Line):** Connects an actor to a use case, denoting participation.
  * **Include Relationship (`<<include>>`):**
  * Mandatory, unconditional insertion of shared behavior.
  * Base use case points a dashed arrow labeled `<<include>>` **toward** the included use case.
  * Represents required, reusable functionality (e.g., `Place Order` includes `Make Payment`; `Withdraw Cash` includes `Validate PIN`).
  * Base case cannot complete without executing the included case.
  * **Extend Relationship (`<<extend>>`):**
  * Optional or conditional augmentation of base behavior.
  * Extending use case points a dashed arrow labeled `<<extend>>` **backward toward** the base use case at an explicit Extension Point.
  * Represents optional functionality (e.g., `Apply Coupon` extends `Checkout`; `Defer Payment` extends `Pay Bill`).
  * Base case is complete on its own and can execute without the extension.
  * **Actor Generalization (Inheritance):**
  * Solid line with a hollow triangular arrowhead pointing to the generalized parent actor.
  * Child actor inherits all associations of the parent (e.g., `Tour Guide` specializes `Passenger`).
### 5.5 Use Case Specification Anatomy
  * *Use Case Name & ID*
  * *Primary Actor & Stakeholders*
  * *Preconditions*: System state that must be true before the use case can start.
  * *Trigger*: The event initiating execution.
  * *Main Success Scenario (Basic Flow)*: Numbered sequence of steps assuming no errors occur.
  * *Extensions (Alternative Flows)*: Branching logic for errors and user options (e.g., `2a. Card declined`, `2a1. Prompt retry`).
  * *Postconditions*:
  * *Success Guarantee*: State when the goal is achieved.
  * *Minimal Guarantee*: Rollback and logging guarantees when the transaction fails.
### 5.6 Scope Heuristics: The Recycling Machine Paradox
  * **Scenario:** Customer inserts cans, presses button, receives cash receipt.
  * **Resolution:** This is **ONE use case**, not three. Inserting a can alone or pressing a button alone provides no independent value to the customer. A use case must represent a complete Elementary Business Process (EBP) delivering an observable result of value.
  * **Completeness Benchmark:** A use case model is complete when it describes all possible ways the system can be used at a level sufficient to drive design, implementation, and testing.
  
  ---
## Module 6: UML Class Modeling (Structural Perspective)
### 6.1 Class Modeling Fundamentals
  * **Definition:** A class is a template, blueprint, or specification for a collection of runtime objects that share common attributes and operations. Represents **static modeling**.
  * **Three Modeling Perspectives:**
  1. *Conceptual*: Domain analysis; language-independent concepts in the real world.
  2. *Specification*: Software design; focus on public interfaces and operational contracts.
  3. *Implementation*: Code construction; concrete language types, access modifiers, and internal data structures.
### 6.2 Class Anatomy: The Three Compartments
  1. **Top Compartment (Name):**
   * Class name in bold UpperCamelCase singular noun.
   * *Abstract Class*: Name written in *italics* or decorated with `{abstract}`.
   * *Interface*: Decorated with the stereotype `<<interface>>` above the name.
  2. **Middle Compartment (Attributes):**
   * Format: `[visibility] name : Type [= default]`
   * *Visibility Markers*:
     * `+` Public
     * `-` Private
     * `#` Protected
     * `~` Package / Default
     * `/` Derived Attribute (computed dynamically at runtime, e.g., `/ age : int` computed from `birthDate`)
   * *Static Attributes*: Underlined (`<u>totalCount : int</u>`).
  3. **Bottom Compartment (Operations / Methods):**
   * Format: `[visibility] name(paramName : ParamType) : returnType`
   * Omit return type on constructors and void methods.
   * Trivial getters/setters may be omitted; **interface operations must never be omitted**.
### 6.3 Candidate Class Extraction
  * **The Noun Extraction Method:** Underline nouns in requirements to identify candidate classes.
  * **Filtering Heuristics:**
  * Reject nouns that are attributes of another class (e.g., `GPA` belongs to `Student`).
  * Reject nouns outside system scope.
  * Reject transient states, events, or time periods (e.g., `Tuesday`, `Login Time`).
  * Normalize synonyms.
### 6.4 Structural Relationships Between Classes
  * **Association (Solid Line):**
  * Communication link between classes.
  * Multiplicities: `1` (exactly one), `*` or `0..*` (zero or more), `1..*` (one or more), `0..1` (zero or one), `2..4` (inclusive range).
  * Adornments: Association names, role names, dual associations between the same classes.
  * **Navigability (Open Arrowhead `->`):**
  * Unidirectional association. Class A holds a reference to Class B and invokes its operations, but Class B has zero knowledge of Class A (e.g., `Router -> DomainNameServer`). Enforces loose coupling.
  * **Association Class / Link Class:**
  * A class attached via a dashed line to an association line. Used when attributes belong to the relationship itself rather than either class (e.g., `Registration` between `Product` and `Warranty`).
  * **Self-Association (Reflexive Association):**
  * An association looping back to the same class with distinct role names (e.g., `LinkedListNode` with roles `next` and `previous`).
  * **Aggregation (Hollow Diamond `<>` at Whole End):**
  * Weak whole-part relationship ("has-a" / "is part of").
  * **Independent Lifecycles**: If the whole is destroyed, the parts continue to exist (e.g., `Team` $\diamond$- `Player`; `Car` $\diamond$- `Engine`).
  * **Composition (Filled Black Diamond `*` at Whole End):**
  * Strong whole-part relationship ("is entirely made of" / "death relationship").
  * **Tied Lifecycles**: The parts live and die with the whole. If the whole is destroyed, the parts are destroyed simultaneously (e.g., `Order` $\blacklozenge$- `OrderItem`; `Book` $\blacklozenge$- `Page`). Multiplicity at the whole end is strictly `1`.
  * **Generalization (Solid Line with Hollow Triangle `—|>`);**
  * Inheritance ("is-a" relationship). Subclass inherits structure and operations from superclass.
  * *Anti-Pattern*: Sharing one or two similar attributes (e.g., `name`, `address`) does **not** justify generalization unless a genuine conceptual is-a hierarchy exists.
  * **Dependency (Dashed Line with Open Arrow `- - - >`):**
  * Temporary "uses" relationship. Class A receives Class B as a method parameter or instantiates it locally on the stack (e.g., `ReportGenerator` relies on `EmailService`).
### 6.5 Class Model Anti-Patterns
  * *God Classes*: One class owning too many responsibilities instead of having a focused purpose.
  * *Conflating Class Models with ER Diagrams*: Class models represent object-oriented behavior, visibility, interfaces, and operations; ER diagrams represent passive database storage.
  * *Modeling Buttons/Screens*: Modeling UI buttons as classes instead of domain entities.
  
  ---
## Module 7: UML State Models (Behavioral Perspective)
### 7.1 State Model Fundamentals
  * **Purpose:** Describes how an object changes condition dynamically over time in response to events.
  * **When to Use:** Objects with distinct lifecycles where allowed actions depend on current status, where the same event produces different outcomes in different states, and where business rules dictate valid transitions. (Less useful for simple, stateless CRUD entities).
  * **State versus Action:**
  * A **State** is a condition or status (Adjective / Past Participle: `Draft`, `Submitted`, `Approved`, `Active`, `Locked`).
  * An **Action** is an operation or event (Active Verb: `Approve Application`, `Click Button`). Never name a state with an action verb.
### 7.2 State Machine Anatomy
  * **State Node:** Rectangle with rounded corners.
  * **Internal Behavior Labels:**
  * `entry / action`: Work executed upon entering the state.
  * `do / activity`: Ongoing background work performed while in the state.
  * `exit / action`: Work executed upon leaving the state.
  * **Initial Pseudostate (Solid Black Circle):** Starting point; has no incoming transitions.
  * **Final State (Bull's-Eye Symbol):** Lifecycle completion. (Long-running services and daemon controllers do not require a final state).
### 7.3 The Universal Transition Syntax
  $$\text{event } [\text{guard}] \text{ / action}$$
  * **event (Trigger):** Stimulus causing the transition (e.g., `pay`, `submit`, `timeout`).
  * **[guard] (Boolean Condition):** Evaluated when the event occurs. Must be true for the transition to fire (e.g., `[amount >= total]`, `[paymentConfirmed]`).
  * **action (Effect):** Atomic work performed during the transition (e.g., `/ sendReceipt()`, `/ triggerAlarm()`).
  * **Rule:** A transition does **not** require both a guard and an action; an event alone can trigger a transition.
### 7.4 Self-Transitions and Choice Points
  * **Self-Transition:** A transition departing a state and looping back to re-enter the same state (e.g., `Editing` looping on `saveDraft / autosave()`). Executes exit and entry actions.
  * **Choice / Decision Point (Diamond `◇`):** Models dynamic branching based on guards evaluated after an event.
### 7.5 Advanced State Constructs
  * **Composite (Compound) States:**
  * A state containing nested sub-states.
  * Reduces diagram clutter by grouping a sub-lifecycle (e.g., `Processing Order` containing `Validate` $\rightarrow$ `Charge` $\rightarrow$ `Reserve Stock`).
  * *Shared Outgoing Transitions*: A transition departing the outer boundary applies to all internal sub-states (e.g., a global `cancel` event).
  * **Concurrent / Orthogonal Regions:**
  * Dashed lines dividing a composite state into independent sub-regions that execute in parallel.
  * Models multiple aspects of an object evolving at the same time (e.g., `Active Order` managing `Fulfillment` and `Payment` concurrently; `Applying Brakes` activating front and rear wheel controllers in parallel).
### 7.6 State Machine Case Studies
  * **Microwave Oven Control:** States (`Waiting`, `Half power`, `Full power`, `Set time`, `Disabled`, `Enabled`, `Operation`). Demonstrates safety interlocks (`Door open` forcing `Disabled`) and nested operational sub-states (`Checking` $\rightarrow$ `Cook` $\rightarrow$ `Done`).
  * **Support Ticket Lifecycle:** `Open` $\rightarrow$ `Assigned` $\rightarrow$ `InProgress` $\rightarrow$ `Resolved` $\rightarrow$ `Closed`. Highlights the necessity of the **recovery path** (`Resolved` transitioning back to `Open` / `Reopened` via `issueReturns`).
  * **User Authentication Lockout:** `Logged Out` $\rightarrow$ `Authenticating` $\rightarrow$ `Authenticated`, with transition to `Locked` guarded by `[failedAttempts >= 5]`.
  * **Git Pull Request:** `Draft` $\rightarrow$ `Open` $\rightarrow$ `Approved` $\rightarrow$ `Merged` (Success Final), with alternate branches for `Changes Requested` and `Closed` (Abandonment Final).
### 7.7 Common State Modeling Pitfalls
  * Using actions or screen names as states.
  * Modeling every minor UI action or CPU computation instead of business-relevant conditions.
  * Leaving transitions unlabeled.
  * Forgetting alternative, failure, cancellation, or recovery paths.
  * Confusing events, guards, and actions.
  * Trying to model an entire multi-class system inside a single state diagram instead of focusing on one object's lifecycle.
  
  ---
## Master Traceability Summary for Exam 1
  
  ```
  +---------------------------------------------------------------------------------------------------------+
  |                                    END-TO-END ENGINEERING TRACEABILITY                                  |
  +---------------------------------------------------------------------------------------------------------+
  | 1. Stakeholder Problem Domain: Needs, Elicitation, Observation, Interviews                              |
  |                                       |                                                                 |
  |                                       v                                                                 |
  | 2. Requirements Specification: Functional (FR) & Non-Functional (NFR) Statements                        |
  |                                       |                                                                 |
  |                                       v                                                                 |
  | 3. Interaction Scenarios: User Stories (Acceptance Criteria) & UML Use Cases (Include/Extend)           |
  |                                       |                                                                 |
  |                                       v                                                                 |
  | 4. Visual Verification: Storyboards (User Journey) & Prototypes (Low/Mid/High-Fidelity)                 |
  |                                       |                                                                 |
  |                                       v                                                                 |
  | 5. Static Structural Design: UML Class Diagrams (Attributes, Operations, Associations, Composition)      |
  |                                       |                                                                 |
  |                                       v                                                                 |
  | 6. Dynamic Behavioral Design: UML State Machine Models (Lifecycle States, Triggers, Guards, Actions)    |
  |                                       |                                                                 |
  |                                       v                                                                 |
  | 7. Quality Assurance: Master Test Plan, Traceability Matrix, Unit/Integration/System Test Cases         |
  +---------------------------------------------------------------------------------------------------------+
  ```