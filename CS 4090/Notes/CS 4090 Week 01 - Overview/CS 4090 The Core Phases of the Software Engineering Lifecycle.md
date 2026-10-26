## 1. Phase 1: Software Requirements Engineering
  
  Requirements engineering (RE) is the process of establishing the services that a customer requires from a system and the constraints under which it operates and is developed. It translates ambiguous real-world problems into clear, testable, and verifiable technical specifications.
  
  ```
  +--------------------------------------------------------------------------+
  |                      PROBLEM DOMAIN VS. SOLUTION DOMAIN                  |
  +--------------------------------------------------------------------------+
  |  Problem Domain (Customer Space):                                        |
  |    • Needs: High-level stakeholder desires, business pain points,        |
  |             and user objectives.                                         |
  |                                                                          |
  |  Solution Domain (Technical Space):                                      |
  |    • Features: User-facing capabilities addressing specific needs.       |
  |    • Software Requirements: Detailed, testable specifications that       |
  |                            developers code and QA engineers verify.      |
  +--------------------------------------------------------------------------+
  ```
### The Requirements Engineering Process
  The RE process consists of four generic, iterative activities:
  1. **Requirements Elicitation and Analysis:** Interacting with stakeholders to discover operational needs, analyze work patterns, and negotiate conflicts.
  2. **Requirements Specification:** Translating elicited information into formal, structured documents (e.g., Software Requirements Specification).
  3. **Requirements Validation:** Demonstrating that the requirements define the system the customer truly wants.
  4. **Requirements Management:** Managing changes to agreed requirements as the project evolves over time.
### Taxonomy of Requirements
#### 1. Functional vs. Non-Functional
  * **Functional Requirements:** Statements of services the system must provide, how it must react to specific inputs, and how it must behave in specific situations. They can also explicitly state what the system **shall not do**.
  * **Non-Functional Requirements (NFRs):** Constraints on the services or functions offered by the system. These define system quality attributes and fall into three broad classes:
  * *Product Requirements:* Specify product behavior (e.g., execution speed, memory footprint, reliability, usability).
  * *Organizational Requirements:* Consequence of organizational policies and standards (e.g., development process standards, implementation language, version control protocols).
  * *External Requirements:* Factors external to the system and development process (e.g., legislative mandates, safety regulations, privacy compliance like HIPAA or FERPA).
  
  ```
  +--------------------------------------------------------------------------+
  |                  NON-FUNCTIONAL REQUIREMENT METRICS                      |
  +-------------------+------------------------------------------------------+
  | Property          | Measurable Metric Units                              |
  +-------------------+------------------------------------------------------+
  | Speed             | Processed transactions/sec; Response time (ms);      |
  |                   | Screen refresh rate (FPS)                            |
  +-------------------+------------------------------------------------------+
  | Size              | Megabytes of storage; ROM/RAM footprint              |
  +-------------------+------------------------------------------------------+
  | Ease of Use       | Training time (hours); Number of help frames         |
  +-------------------+------------------------------------------------------+
  | Reliability       | Mean Time Between Failures (MTBF); Probability of    |
  |                   | unavailability; Failure rate                         |
  +-------------------+------------------------------------------------------+
  | Robustness        | Time to restart after failure; Percentage of events  |
  |                   | causing catastrophic failure                         |
  +-------------------+------------------------------------------------------+
  | Portability       | Target compiler standards; Number of target OSs      |
  +-------------------+------------------------------------------------------+
  ```
#### 2. User Requirements vs. System Requirements
  * **User Requirements:** High-level natural language statements and intuitive diagrams describing system services and operational constraints. Written primarily for clients, managers, and non-technical stakeholders.
  * **System Requirements:** Detailed, structured technical specifications of functions, interfaces, and operational constraints. Serves as a contract baseline for software architects, developers, and testers.
### Requirements Elicitation Techniques
  * **Stakeholder Interviews:** Structured or unstructured discussions with users to identify daily workflows and pain points.
  * **Observation and Workflow Analysis:** Watching users perform their actual work. This reveals implicit tasks, shortcuts, and edge cases that users forget to mention during verbal interviews.
  * **Workshops and Brainstorming:** Joint sessions bringing stakeholders together to resolve conflicting goals and negotiate feature priorities.
  * **Storyboarding and Prototyping:** Providing low-fidelity mockups or interactive wireframes to gather early reactions to screen layouts and task flows.
  
  ---
## 2. Phase 2: Software Design and Modeling
  
  Software design transforms requirements specifications into technical blueprints for implementation. It bridges the gap between *what* the system must do and *how* the code will achieve it.
  
  ```
  +--------------------------------------------------------------------------+
  |                        SYSTEM DESIGN PERSPECTIVES                        |
  +-------------------+------------------------------------------------------+
  | Perspective       | Modeling Purpose                                     |
  +-------------------+------------------------------------------------------+
  | External          | Models context and environment (Context Models)      |
  +-------------------+------------------------------------------------------+
  | Structural        | Models static organization and data structures       |
  |                   | (UML Class Diagrams, Component Diagrams)             |
  +-------------------+------------------------------------------------------+
  | Interaction       | Models communication between actors and components   |
  |                   | (UML Sequence Diagrams, Use Case Diagrams)           |
  +-------------------+------------------------------------------------------+
  | Behavioral        | Models dynamic responses to internal/external events |
  |                   | (UML State Machine Diagrams, Activity Diagrams)      |
  +-------------------+------------------------------------------------------+
  ```
### Static Modeling: UML Class Diagrams
  A class is a blueprint for a collection of objects sharing attributes, operations, and semantics. Good modeling follows **CRC Thinking** (Class, Responsibility, Collaboration), assigning distinct, single responsibilities to classes while avoiding "God classes" that manage too much state.
  
  ```
  +---------------------------------------------------------+
  |                       Account                           |  <-- Top: Class Name
  +---------------------------------------------------------+
  | - accountNumber : String                                |  <-- Middle: Attributes
  | - balance : double                                      |      (- private, + public,
  | # interestRate : double                                 |       # protected, ~ package,
  | / ageInDays : int                                       |       / derived)
  +---------------------------------------------------------+
  | + deposit(amount : double) : boolean                    |  <-- Bottom: Operations
  | + withdraw(amount : double) : boolean                   |      (name(param: Type): Return)
  +---------------------------------------------------------+
  ```
#### Core Structural Relationships
  * **Association (Solid Line):** General structural connection indicating that two classes communicate or hold a reference. Can include multiplicity (`1`, `*`, `0..1`, `1..*`).
  * **Dependency (Dashed Arrow):** A temporary "using" relationship where one class uses an instance of another class as an operation parameter or local variable. A large number of dependencies indicates tight coupling.
  * **Aggregation (Hollow Diamond):** Weak whole-part relationship ("has-a"). The part can exist independently if the container object is destroyed (e.g., a `Department` has `Professors`).
  * **Composition (Filled Black Diamond):** Strong whole-part relationship ("death relationship"). The part's lifecycle is strictly bound to the container object. If the container is destroyed, the parts are destroyed with it (e.g., an `Order` is composed of `OrderLines`).
  * **Generalization (Hollow Triangle Arrow):** Object-oriented inheritance ("is-a"). Subclasses inherit attributes and operations from a superclass.
### Dynamic Modeling: UML State Machine Diagrams
  State machines model the runtime lifecycle of an object as it transitions between discrete conditions in response to external stimuli.
  
  ```
  [*] --> [ Inactive ] --- activate [hasPermissions] / initSession ---> [ Active ]
                                                                           |
                                                                           | terminate
                                                                           v
                                                                        [  *  ]
  ```
  
  * **State:** A distinct condition in an object's lifecycle where it satisfies some condition, performs an activity, or waits for an event. State names should be conditions or adjectives (`Approved`, `Active`), never action verbs (`Approve Application`).
  * **Transition Pattern:** Standardized UML label syntax:  
  $$\text{event } [\text{guard}] \text{ / action}$$
  * *Event:* The trigger that causes the state change.
  * *Guard:* A boolean condition enclosed in brackets that must evaluate to true for the transition to fire.
  * *Action:* An executable operation or output performed during the transition.
  * **Composite States:** States containing nested substates to group lower-level lifecycles and reduce visual clutter.
  * **Orthogonal Regions:** Dashed boundaries within a composite state modeling concurrent, independent sub-lifecycles operating at the same time.
  
  ---
## 3. Phase 3: Software Implementation and Construction
  
  Implementation converts architectural designs and database schemas into clean, executable source code. In collaborative engineering teams, code construction relies on structured processes to prevent merge conflicts and maintain standards.
  
  ```
  +--------------------------------------------------------------------------+
  |                   COLLABORATIVE CONSTRUCTION WORKFLOW                    |
  +--------------------------------------------------------------------------+
  |  1. Task Allocation: Pull discrete issues from the project task board.   |
  |  2. Branching: Create short-lived feature branch off main.               |
  |  3. Development & Local Tests: Write code alongside unit tests.          |
  |  4. Code Review (Pull Request): Static analysis and peer inspection.     |
  |  5. Automated Verification: Continuous Integration (CI) build passes.    |
  |  6. Merge: Branch is squashed or merged back to the mainline.            |
  +--------------------------------------------------------------------------+
  ```
### Peer Code Reviews and Static Analysis
  Code reviews serve as defect prevention before bugs enter the integrated codebase:
  * **Check for Logic Defects:** Off-by-one errors, unhandled null pointers, edge-case mishandling, and unbounded memory consumption.
  * **Enforce Architecture:** Verifies that developers did not bypass abstraction layers (e.g., writing raw database SQL queries directly inside UI components).
  * **Maintain Codebase Uniformity:** Ensures code adheres to documented team formatting standards, naming conventions, and documentation rules.
  
  ---
## 4. Phase 4: Software Testing and Quality Verification
  
  Testing provides empirical evidence regarding the correctness, reliability, and quality of a software system.
  
  ```
                             / \
                            /   \
                           / UAT \          <-- End-User Validation
                          /-------\
                         /  System \        <-- Whole Integrated System
                        /-----------\
                       / Integration \      <-- Subsystem Interfaces & APIs
                      /---------------\
                     /   Unit Testing  \    <-- Individual Classes / Functions
                    +-------------------+
  ```
### Verification vs. Validation (Boehm's Maxim)
  * **Verification ("Are we building the product right?"):** Checks whether the software conforms to its documented specifications and design constraints. Focuses on code inspections, unit tests, and structural reviews.
  * **Validation ("Are we building the right product?"):** Checks whether the delivered software meets the customer's actual operational expectations and real-world needs. Focuses on acceptance testing and user trials.
### The Levels of Testing
  1. **Unit Testing:** Testing individual components, classes, or methods in isolation. Relies on mock objects and stubs to decouple units from external databases or networks.
  2. **Integration Testing:** Testing groups of interacting components to identify interface defects, data serialization errors, and timing issues between modules.
  3. **System Testing:** Testing the fully integrated application as a whole to verify both functional workflows and non-functional requirements (performance, load, security).
  4. **User Acceptance Testing (UAT):** Testing conducted by target end users or clients in a realistic environment to determine whether the system is acceptable for formal delivery.
### Structured Test Case Design
  A valid test case must provide reproducible, deterministic steps:
  * **Test Case ID:** Unique identifier (e.g., `TC-AUTH-01`).
  * **Requirement Reference:** Functional requirement traced to this test (e.g., `FR-01`).
  * **Preconditions:** System state required before test execution (e.g., user account exists, database seeded).
  * **Test Inputs:** Specific data passed to the system.
  * **Execution Steps:** Sequential actions performed by the tester.
  * **Expected Output:** Concrete, unambiguous result indicating success.
  * **Actual Output:** Observed behavior during test execution.
  * **Status:** `Pass`, `Fail`, or `Blocked`.
  
  ---
## 5. Phase 5: Software Maintenance and Evolution
  
  Software does not degrade through physical wear, but it decays through **architectural erosion** and environmental change. Maintenance represents the longest and most resource-intensive phase of the software lifecycle.
  
  ```
  +--------------------------------------------------------------------------+
  |                     THE FOUR TYPES OF MAINTENANCE                        |
  +-------------------+------------------------------------------------------+
  | Maintenance Type  | Primary Focus & Purpose                              |
  +-------------------+------------------------------------------------------+
  | Corrective        | Reactive bug fixes. Repairing undiscovered software   |
  |                   | defects, logical bugs, and system crashes.           |
  +-------------------+------------------------------------------------------+
  | Adaptive          | Modifying software to operate in changing external   |
  |                   | environments (e.g., OS updates, new hardware,        |
  |                   | database upgrades, third-party API deprecations).     |
  +-------------------+------------------------------------------------------+
  | Perfective        | Implementing new functional enhancements, UI         |
  |                   | improvements, and performance optimizations based    |
  |                   | on user feedback.                                    |
  +-------------------+------------------------------------------------------+
  | Preventive        | Refactoring code, improving documentation, and       |
  | (Reengineering)   | cleaning up structural flaws to prevent future bugs  |
  |                   | and reduce technical debt.                           |
  +-------------------+------------------------------------------------------+
  ```
### Technical Debt
  Technical debt occurs when engineers choose quick, expedient code hacks over clean, well-architected solutions to meet short-term deadlines. Like financial debt, technical debt incurs interest: future features take longer to build, modifications cause unexpected regressions, and onboarding new developers becomes difficult. Periodic preventive maintenance and refactoring are required to pay down technical debt before architectural decay forces a complete rewrite.
  
  ---