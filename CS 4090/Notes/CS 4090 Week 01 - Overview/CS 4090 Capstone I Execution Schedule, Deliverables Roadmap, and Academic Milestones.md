## 1. Semester Roadmap and Phase Structure
  
  The first semester of the capstone sequence guides teams through a structured progression from an unrefined concept to a verified, working prototype. Capstone I balances plan-driven requirements and design artifacts with iterative implementation sprints.
  
  ```
  +---------------------------------------------------------------------------------------+
  |                               CAPSTONE I TIMELINE                                     |
  +-------------------+--------------------+-----------------------+----------------------+
  | QuickStart        | Phase 1            | Phase 2               | Phase 3              |
  | Week 1            | Weeks 2 - 5        | Weeks 6 - 10          | Weeks 11 - 15        |
  +-------------------+--------------------+-----------------------+----------------------+
  | Team Formation    | Requirements &     | Software Design &     | Implementation,      |
  | Project Title     | Feasibility Study  | Sprint 1 (Early Dev)  | Testing & Sprint 2   |
  | Initial Roles     | SRS Document       | Design Document (SDD) | Working Prototype    |
  | Comms Plan        | Wireframes / Mock  | Architecture & Schema | Test Plan & Demo     |
  +-------------------+--------------------+-----------------------+----------------------+
                                                                            |
                                                                            v
                                                                   [ Final Deliverable ]
                                                                   Document & Presentation
  ```
  
  ---
## 2. Project QuickStart (Week 1)
  
  Week 1 establishes the operational foundation for the team. Teams form, define project boundaries, and establish communication norms before technical execution begins.
### Dual-Process Viewpoint
  * **Waterfall Perspective:** Formally identify the project team, project title, and initial scope baseline to prevent early deviation.
  * **Agile Perspective:** Assemble the cross-functional team, establish the initial product backlog concept, and define the team communication rhythm.
### Required Deliverables and Activities
  * **Team Formation and Name:** Establish the roster and select an engineering team name.
  * **Initial Project Idea and Title:** Formulate a concise problem statement and proposed solution.
  * **Initial Team Roles:** Assign primary leadership domains (Project Manager, Product Owner, Architect, Frontend Lead, Backend Lead, QA Lead, DevOps/Docs Lead).
  * **Team Communication Plan:** Select tools for synchronous chat, code repositories, document storage, and video syncs.
  
  ---
## 3. Phase 1: Requirements and Feasibility Study (Weeks 2–5, Approx. 4 Weeks)
  
  Phase 1 focuses on answering: *What problem are we solving, who are we solving it for, and is it technically possible within our constraints?*
  
  ```
                             +------------------------+
                             | Stakeholder / Problem  |
                             +-----------+------------+
                                         |
                                         v
                             +------------------------+
                             |   Feasibility Study    |
                             | (Tech, Ops, Resources) |
                             +-----------+------------+
                                         |
                                         v
                             +------------------------+
                             |  Requirements Engine   |
                             | (User Stories, Use     |
                             |  Cases, FRs, NFRs)     |
                             +-----------+------------+
                                         |
                        +----------------+----------------+
                        |                                 |
                        v                                 v
             +--------------------+            +--------------------+
             |    SRS Document    |            | Initial Prototype  |
             |   (Specifications) |            |    (Wireframes)    |
             +--------------------+            +--------------------+
  ```
### Dual-Process Viewpoint
  * **Waterfall Perspective:** Gather, analyze, validate, and sign off on a complete Software Requirements Specification (SRS) before moving into system design.
  * **Agile Perspective:** Brainstorm user personas, write high-level user stories with acceptance criteria, explore early throwaway prototypes to de-risk technical unknowns, and establish a prioritized product backlog.
### Core Activities
  1. **Stakeholder and User Analysis:** Identify primary users, secondary users, and system administrators. Build user personas detailing goals, technical familiarity, and operational pain points.
  2. **Project Scope Definition:** Draw explicit boundaries separating in-scope capabilities (MVP) from deferred enhancements (extensions).
  3. **Requirements Discovery:** Elicit requirements using interviews, domain research, and workflow analysis.
  4. **Feasibility Study:** Evaluate three dimensions of feasibility:
   * *Technical Feasibility:* Can this be built using existing hardware, drivers, network bandwidth, and libraries?
   * *Operational Feasibility:* Will target users adopt this workflow, or is the friction too high?
   * *Resource/Schedule Feasibility:* Can six undergraduate engineers complete the core architecture within the two-semester timeline?
  5. **Initial Prototyping:** Build low-fidelity wireframes, UI screen flows, and interactive mockups to validate usability before writing code.
### Required In-Class Activities and Workshops
  * Problem statement workshop and stakeholder identification.
  * User story writing and use case modeling (including UML Use Case Diagrams).
  * Functional and non-functional requirements prioritization (MoSCoW framework).
  * Formal peer review of requirements across teams.
### Deliverables
  * **Software Requirements Specification (SRS) Document:** Complete specification covering functional requirements, measurable quality attributes, business rules, traceability matrix, and interface designs.
  * **Requirements Presentation:** Slide deck summarizing the problem, system scope, use case model, and prototype workflow.
  * **Feasibility Study Report:** Technical evaluation documenting risks, constraints, and platform selections.
  * **Initial Prototype / Wireframes:** Visual walkthrough illustrating the end-to-end user experience.
  
  ---
## 4. Phase 2: Software Design / Sprint 1 (Weeks 6–10, Approx. 5 Weeks)
  
  Phase 2 transforms the approved requirements into a structured, implementable system architecture while initiating early implementation (Sprint 1).
  
  ```
                      +-----------------------------+
                      |   Approved Requirements     |
                      +--------------+--------------+
                                     |
                                     v
                      +-----------------------------+
                      |   Architectural Blueprint   |
                      | (Component / Context Models)|
                      +--------------+--------------+
                                     |
               +---------------------+---------------------+
               |                                           |
               v                                           v
  +-----------------------------+             +-----------------------------+
  |    Structural & Data Models |             | Dynamic & Behavioral Models |
  | • UML Class Diagrams        |             | • UML State Machines        |
  | • Database Schema (ERD)     |             | • Sequence Diagrams         |
  | • API Contract Definitions  |             | • Data Flow Diagrams        |
  +--------------+--------------+             +--------------+--------------+
               |                                           |
               +---------------------+---------------------+
                                     |
                                     v
                      +-----------------------------+
                      |          Sprint 1           |
                      |  • Setup Repo / CI Pipeline |
                      |  • Architectural Spikes     |
                      |  • Core Infrastructure Code |
                      +-----------------------------+
  ```
### Dual-Process Viewpoint
  * **Waterfall Perspective:** Produce an exhaustive Software Design Document (SDD) defining component diagrams, database schemas, class relationships, and network protocols before writing production features.
  * **Agile Perspective:** Execute Sprint 1. Build an architectural "walking skeleton" (a minimal, end-to-end slice connecting frontend, backend, and persistence), refine the design iteratively, and conduct sprint review meetings.
### Core Activities
  1. **System Architecture Design:** Define system boundaries, context models, and multi-tier component divisions (e.g., presentation layer, network transport, native driver layer).
  2. **Object and Data Modeling:**
   * Build UML Class Diagrams displaying class attributes, operations, visibility, and associations.
   * Design relational database schemas (normalized tables, primary/foreign keys, indices) or persistent state stores.
  3. **Dynamic Modeling:**
   * Author UML State Machine Diagrams for stateful entities (e.g., room lifecycle, controller token states).
   * Construct Sequence Diagrams showing message passing and network calls between actors and components.
  4. **Interface and API Design:** Specify REST endpoints, WebSocket message payloads, or WebRTC data channel serialization formats.
  5. **Implementation Planning:** Deconstruct design components into actionable tickets on the project task board.
  6. **Early Development (Sprint 1):** Set up the development environment, configure GitHub Actions pipelines, and complete architectural spikes (testing high-risk integrations like low-level OS drivers or WebRTC handshakes).
### Required In-Class Activities and Workshops
  * Architecture diagramming and component modeling.
  * Database schema design and normalization review.
  * UI/UX wireframe inspection and click-through validation.
  * API planning and serialization contract definition.
  * Risk analysis and design peer review.
### Deliverables
  * **Software Design Document (SDD):** Complete technical architecture manual.
  * **Architecture Diagrams and Data Schemas:** Verified UML models, ERDs, and network topology maps.
  * **UI Mockups and High-Fidelity Wireframes:** Detailed screen layouts with navigation links.
  * **Sprint 1 Execution Plan:** Backlog of prioritized technical tasks assigned to team members.
  * **Design/Sprint Meeting Review:** Formal presentation of system architecture to instructors.
  
  ---
## 5. Phase 3: Software Implementation and Testing / Sprint 2 (Weeks 11–15, Approx. 5 Weeks)
  
  Phase 3 is dedicated to heavy development, feature integration, verification, and bug resolution (Sprint 2).
  
  ```
  +---------------------------------------------------------------------------------+
  |                       PHASE 3: IMPLEMENTATION AND VERIFICATION                  |
  +---------------------------------------------------------------------------------+
  |                                                                                 |
  |  [ Feature Implementation ] ---> [ Code Review ] ---> [ Automated CI Build ]    |
  |              ^                                                  |               |
  |              |                  (Bug Fix Cycle)                 v               |
  |              +<--------------------------------------- [ Integration Testing ]  |
  |                                                                 |               |
  |                                                                 v               |
  |                                                        [ System & UAT Tests ]   |
  |                                                                 |               |
  |                                                                 v               |
  |                                                        [ Verified MVP Demo ]    |
  +---------------------------------------------------------------------------------+
  ```
### Dual-Process Viewpoint
  * **Waterfall Perspective:** Implement code strictly according to SDD specifications, run system verification against the test plan, log formal defect reports, and sign off on acceptance.
  * **Agile Perspective:** Execute Sprint 2. Deliver functional feature increments in short cycles, perform continuous integration and automated testing, adapt to implementation discoveries, and polish the working software for demo day.
### Core Activities
  1. **Major Feature Implementation:** Build the functional core of the application according to documented requirements.
  2. **Subsystem Integration:** Connect frontend user interfaces with backend business logic, database persistence, and network transport layers.
  3. **Quality Assurance and Testing:**
   * Author and execute comprehensive test cases (unit, integration, and end-to-end workflows).
   * Conduct User Acceptance Testing (UAT) simulations with classmates or sample users.
   * Run regression tests to ensure newly added features do not break existing functionality.
  4. **Defect Tracking and Triage:** Log reproducible bug reports (reproduction steps, expected behavior, actual behavior, severity, priority) and resolve defects systematically.
  5. **Demonstration and Handoff Preparation:** Polish user flows, eliminate runtime exceptions, write user installation manuals, and prepare the live presentation.
### Required In-Class Activities and Workshops
  * GitHub workflow and branch protection audits.
  * Peer code review sessions.
  * Test case writing and bug report triage workshops.
  * Regression testing checklists and pre-demo quality audits.
### Deliverables
  * **Working Prototype or System Demo:** Executable, demonstratable application satisfying core MVP requirements.
  * **Master Test Plan and Test Cases:** Documented test specifications with execution records and pass/fail metrics.
  * **Defect Log / Bug Report:** Comprehensive record of identified, resolved, and open software issues.
  * **Final Capstone I Project Document:** Consolidated engineering report compiling requirements, designs, implementation details, and test results.
  * **Final Project Presentation / Live Demo:** In-class demonstration of the working software and architecture.
  
  ---
## 6. Capstone I Completion Checklist
  
  Before completing Capstone I, every engineering team must have verified the following artifacts:
  
  | Phase | Milestone Artifact | Verification Standard |
  | :--- | :--- | :--- |
  | **Foundation** | Team Charter & Governance Plan | Explicit roles, Git workflows, and conflict policies documented. |
  | **Requirements** | Software Requirements Specification (SRS) | Functional requirements uniquely tagged, NFRs quantified, traceability verified. |
  | **Feasibility** | Technical Feasibility Report | Proof of concept verifies critical OS and network assumptions. |
  | **Design** | Software Design Document (SDD) | Component, class, state, database, and API contracts formalized. |
  | **Implementation** | Working Software Core | Application launches, executes the core user loop, and handles basic errors. |
  | **Verification** | Test Plan, Cases & Bug Tracker | Documented test execution proving system meets functional requirements. |
  | **Demonstration**| Live System Presentation | End-to-end user scenario demonstrated without catastrophic failure. |
  
  ---
## 7. The Bridge to Capstone II
  
  Capstone I provides the technical foundation by allowing teams to learn software engineering processes on a controlled, student-proposed project. 
  
  Capstone II shifts the operational model to mirror professional software development:
  
  ```
  +-----------------------------------------------------------------------------+
  |                     TRANSITION: CAPSTONE I TO CAPSTONE II                   |
  +-----------------------------+-----------------------------------------------+
  | Dimension                   | Capstone I            | Capstone II           |
  +-----------------------------+-----------------------+-----------------------+
  | Project Source              | Student-proposed idea | External client / S&T |
  | Stakeholder Interaction     | Internal team / Prof  | Real-world client     |
  | Requirements Stability      | Relatively controlled | Shifting / Ambiguous  |
  | Primary Learning Focus      | Mastering the SDLC    | Client delivery, UAT  |
  | Final Showcase              | In-class demo         | Public demo & poster  |
  +-----------------------------+-----------------------+-----------------------+
  ```