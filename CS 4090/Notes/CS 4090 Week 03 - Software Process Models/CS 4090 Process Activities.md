## 1. The Four Universal Process Activities
  
  Regardless of whether a team follows a plan-driven Waterfall process or an iterative Agile framework, all software lifecycles consist of four fundamental, generic activities:
  
  ```
  +--------------------------------------------------------------------------+
  |                 FOUR UNIVERSAL SOFTWARE PROCESS ACTIVITIES               |
  +-------------------+------------------------------------------------------+
  | Activity          | Core Engineering Question                            |
  +-------------------+------------------------------------------------------+
  | 1. Specification  | What services must the system provide, and under     |
  |                   | what operational constraints must it function?       |
  +-------------------+------------------------------------------------------+
  | 2. Design &       | How will the software be architectured, structured,  |
  |    Implementation | and converted into an executable program?            |
  +-------------------+------------------------------------------------------+
  | 3. Validation     | Does the software conform to its specification and   |
  |                   | satisfy the customer's actual operational needs?     |
  +-------------------+------------------------------------------------------+
  | 4. Evolution      | How must the software be modified as business rules, |
  |                   | user requirements, and technical platforms change?   |
  +-------------------+------------------------------------------------------+
  ```
  
  ---
## 2. Activity 1: Software Specification
  
  **Software specification** is the process of establishing what services are required from the system and identifying the constraints on the system's operation and development.
  
  Slide 21 breaks requirements engineering down into three sub-activities:
  
  ```
  [ Requirements Elicitation & Analysis ]
  • What do stakeholders require or expect?
  • Uncovers user goals, workflows, and pain points.
                   |
                   v
  [ Requirements Specification ]
  • Defining requirements in detail.
  • Formalizes needs into structured, testable statements.
                   |
                   v
  [ Requirements Validation ]
  • Checking the validity of requirements.
  • Confirms realism, consistency, and customer alignment.
  ```
### Categorization: Functional vs. Non-Functional Requirements (Slide 22)
  Slide 22 illustrates the distinction using an Automated Teller Machine (ATM):
  
  ```
  +--------------------------------------------------------------------------+
  |                  FUNCTIONAL VS. NON-FUNCTIONAL MATRIX                    |
  +---------------------+----------------------------------------------------+
  | Dimension           | ATM Example from Slides                            |
  +---------------------+----------------------------------------------------+
  | Functional          | Concrete features, user actions, and services:     |
  | Requirements        | • The system shall allow users to debit funds.     |
  | (What it does)      | • The system shall allow users to credit accounts. |
  |                     | • The system shall check account balances.         |
  +---------------------+----------------------------------------------------+
  | Non-Functional      | System-wide behavioral properties and constraints: |
  | Requirements        | • Performance: Process ATM transactions in < 2 sec |
  | (How it behaves)    | • Security: Encrypt PIN transmission using AES-256 |
  |                     | • Scalability: Support 500 concurrent ATM terminals|
  |                     | • Availability: 99.9% uptime during banking hours  |
  +---------------------+----------------------------------------------------+
  ```
  
  ---
## 3. Activity 2: Software Design
  
  Slide 23 defines **software design** as: *The process of converting the system specification into a structured software architecture that realizes the specification.*
  
  Slide 24 outlines four primary design activities that teams must complete:
  
  ```
  +--------------------------------------------------------------------------+
  |                         THE FOUR DESIGN ACTIVITIES                       |
  +---------------------+----------------------------------------------------+
  | Design Stage        | Engineering Responsibility                         |
  +---------------------+----------------------------------------------------+
  | 1. Architectural    | Identify the high-level system structure, major    |
  |    Design           | subsystems, their relationships, and how they are  |
  |                     | distributed (e.g., client-server, microservices).  |
  +---------------------+----------------------------------------------------+
  | 2. Database         | Design data structures, schemas, entity            |
  |    Design           | relationships (ERDs), indices, and relational      |
  |                     | table mappings.                                    |
  +---------------------+----------------------------------------------------+
  | 3. Interface        | Define unambiguous communication contracts between |
  |    Design           | components (e.g., REST endpoints, WebSocket data   |
  |                     | payloads, function signatures).                    |
  +---------------------+----------------------------------------------------+
  | 4. Component        | Search for reusable third-party components or      |
  |    Selection        | libraries. If none exist, design custom components |
  |                     | from scratch.                                      |
  +---------------------+----------------------------------------------------+
  ```
  
  ---
## 4. Activity 3: System Implementation
  
  Slide 25 defines **implementation** as the translation of structural design models into executable programs.
### Implementation Pathways
  Software is implemented through two distinct paths:
  1. **Custom Program Development:** Writing new code in programming languages (e.g., C++, Python, TypeScript) to realize components.
  2. **Application Configuration:** Configuring pre-existing COTS packages, database engines, or cloud services to behave according to specification.
### Programming vs. Debugging (Slide 25)
  The slides draw an explicit boundary between coding and error correction:
  * **Programming:** An individual, creative activity with no single standard process. Two engineers given the same specification will produce different code implementations that are both correct.
  * **Debugging:** A systematic, analytical activity focused on discovering the root cause of program faults and modifying code to remove those faults.
  
  ```
  [ Code Execution ] ---> [ Failure Observed ] ---> [ Defect Located ] ---> [ Fault Corrected ]
                              (Bug)                 (Root Cause)              (Patch/Fix)
  ```
  
  *Important Note:* Debugging is not testing. Testing is the process of establishing the existence of defects; debugging is the process of locating and repairing the underlying fault that caused the defect.
  
  ---
## 5. Activity 4: Software Validation (Testing)
  
  Slide 26 defines **Verification and Validation (V&V)**:
  > **Verification and validation is intended to show that a system conforms to its specification and meets the requirements of the system customer.**
  
  * **Verification:** *"Are we building the product right?"* Ensures the software conforms strictly to its documented technical specifications.
  * **Validation:** *"Are we building the right product?"* Ensures the software meets the real operational expectations of the customer.
  
  Testing is the most commonly used V&V activity. System testing involves executing software with deterministic test cases derived from specifications of real data.
### The Three Testing Stages (Slides 27 and 28)
  
  ```
  +--------------------+        +--------------------+        +--------------------+
  | Component Testing  |------->|   System Testing   |------->| Acceptance Testing |
  | (Unit / Isolation) |        | (Emergent Behavior)|        |  (Customer Data)   |
  +--------------------+        +--------------------+        +--------------------+
          ^                                                           |
          |                      (Defects Found)                      |
          +-----------------------------------------------------------+
  ```
#### 1. Component Testing (Unit Testing)
  * Individual software units, functions, classes, or coherent groupings are tested in total isolation.
  * Relies on mock objects, stubs, and synthetic test harnesses to isolate the component from databases, networks, and external file systems.
  * Focuses on boundary conditions, loop limits, null handling, and algorithmic accuracy.
#### 2. System Testing
  * Tests the fully integrated system as a unified whole.
  * Focuses heavily on verifying **emergent properties**: characteristics of the system that cannot be evaluated at the component level because they only appear when all parts interact.
  * *Examples of emergent properties:*
  * Overall system throughput and response latency under high load.
  * Memory leaks across long-running background threads.
  * Race conditions during concurrent network operations.
  * Reliability of failover mechanisms when database connections drop.
#### 3. Customer Testing (Acceptance Testing)
  * Conducted using real or representative customer data in an operational environment.
  * Evaluates whether the system satisfies operational needs rather than just passing unit tests.
  * Reveals unstated customer assumptions, missing edge-case workflows, and user interface friction.
  
  ---
## 6. Activity 5: Software Evolution
  
  Slide 29 highlights a critical operational reality: **Software is inherently flexible and can change.**
### Why Software Must Evolve
  As business circumstances, legal regulations, operating systems, and user habits change, the software supporting the organization must change alongside them. 
  
  Slide 29 notes: *Although there has historically been a demarcation between development and evolution (maintenance), this distinction is increasingly irrelevant because fewer and fewer software systems are completely new.* Most modern engineering involves modifying and evolving pre-existing "brownfield" codebases.
### The System Evolution Cycle (Slide 30)
  System evolution operates as a closed continuous improvement loop:
  
  ```
                     +----------------------------+
                     |  Define System Requirements |<===============\
                     +--------------+-------------+                 |
                                    |                               |
                                    v                               |
  +-------------------+  +----------------------------+               |
  | Existing Systems  |->|   Assess Existing Systems  |               |
  +-------------------+  +--------------+-------------+               |
          ^                         |                               |
          |                         v                               |
          |            +----------------------------+               |
          |            |    Propose System Changes  |               |
          |            +--------------+-------------+               |
          |                         |                               |
          |                         v                               |
          |            +----------------------------+               |
          |            |       Modify Systems       |               |
          |            +--------------+-------------+               |
          |                         |                               |
          |                         v                               |
          |            +----------------------------+               |
          +------------|         New System         |---------------/
                       +----------------------------+
  ```
  
  1. **Define System Requirements:** New operational needs, bug reports, or performance bottlenecks are identified.
  2. **Assess Existing Systems:** The existing architecture is analyzed to determine which modules can support the changes and what structural refactoring is required.
  3. **Propose System Changes:** Architectural adjustments, interface modifications, and code updates are planned.
  4. **Modify Systems:** Engineers implement changes, run regression test suites, and deploy updates.
  5. **New System Baseline:** The updated system becomes the new operational foundation, which will eventually be assessed in subsequent evolution cycles.
  
  ---