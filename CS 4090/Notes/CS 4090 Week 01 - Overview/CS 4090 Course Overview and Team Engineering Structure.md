## 1. The Two-Semester Capstone Architecture
  
  The software engineering capstone sequence is modeled after industry development environments. It bridges the gap between individual, assignment-driven academic coursework and collaborative, production-grade engineering.
  
  ```
  +-----------------------------------------------------------------------+
  |                        SE CAPSTONE SEQUENCE                           |
  +-----------------------------------------------------------------------+
                                    |
        +---------------------------+---------------------------+
        v                                                       v
  +-------------------------------+       +-------------------------------+
  |         SE CAPSTONE I         |       |        SE CAPSTONE II         |
  |   (Foundation & Prototype)    |       |   (Client Delivery & Scale)   |
  +-------------------------------+       +-------------------------------+
  | • Team formation & governance |       | • External clients (MS&T/Ind) |
  | • Student-proposed project    |       | • Incomplete/shifting specs   |
  | • Full SDLC walkthrough       |       | • Iterative releases          |
  | • Requirements (SRS) & Design |       | • Long-term maintainability   |
  | • Proof of concept / Demo     |       | • Public showcase / Poster    |
  +-------------------------------+       +-------------------------------+
  ```
### Capstone I: Controlled Engineering Foundation
  * **Project Source:** Student-proposed or instructor-approved project ideas.
  * **Primary Objective:** Build a working prototype from the ground up while establishing formal engineering rigor. Teams practice requirements elicitation, architectural design, testing strategies, project tracking, and version control hygiene without external client interference.
  * **Core Deliverable:** A functional Minimum Viable Product (MVP) accompanied by formal engineering specifications (SRS, Design Documents, Test Plans).
### Capstone II: Client-Facing Realism
  * **Project Source:** Real-world stakeholders from Missouri S&T research groups, university departments, or external industry partners.
  * **Primary Objective:** Deliver software under realistic constraints. Requirements in this phase are often ambiguous, incomplete, or subject to mid-project changes from clients.
  * **Core Deliverable:** Deployment-ready software, formal user acceptance testing (UAT), handoff documentation, and a final public poster/demonstration presentation.
  
  ---
## 2. Engineering Team Organization and Functional Roles
  
  A capstone team operates as an autonomous engineering unit. The most common cause of capstone failure is not technical deficiency, but unassigned responsibility, silent roadblocks, and uneven contribution.
  
  The slides state a core rule: **Roles are leadership areas, not limitations.** An assigned lead is responsible for ensuring their domain is completed correctly, but every member must write code, review work, and contribute across the project.
  
  ```
                             +-------------------------+
                             |     PROJECT MANAGER     |
                             |      (Scrum Master)     |
                             +------------+------------+
                                          |
                   +----------------------+----------------------+
                   |                                             |
                   v                                             v
     +--------------------------+                  +---------------------------+
     |      PRODUCT OWNER       |                  |      SYSTEM ARCHITECT     |
     |   (Requirements Lead)    |                  |        (Design Lead)      |
     +-------------+------------+                  +-------------+-------------+
                   |                                             |
         +---------+---------+                         +---------+---------+
         v                   v                         v                   v
  +-----------------+ +-----------------+       +-----------------+ +-----------------+
  |   FRONTEND/UI   | |   BACKEND/DB    |       |   QA / TESTING  | |  DOCUMENTATION  |
  |      LEAD       | |      LEAD       |       |       LEAD      | |   & DEVOPS      |
  +-----------------+ +-----------------+       +-----------------+ +-----------------+
  ```
### Role Breakdown and Operational Responsibilities
#### 1. Project Manager / Scrum Master
  * **Core Ownership:** Schedule, timeline adherence, velocity, and blocker removal.
  * **Key Tasks:**
  * Runs recurring sync meetings (standups, sprint planning, retrospectives).
  * Tracks milestones against the course calendar.
  * Ensures workload is distributed evenly among team members.
  * Serves as the primary point of contact with the course instructor.
#### 2. Product Owner / Requirements Lead
  * **Core Ownership:** System scope, user needs, and feature prioritization.
  * **Key Tasks:**
  * Translates broad project goals into formal Functional and Non-Functional Requirements.
  * Manages the product backlog and user stories.
  * Defines precise **Acceptance Criteria** for each feature.
  * Prevents scope creep by rejecting or deferring non-essential features.
#### 3. System Architect / Design Lead
  * **Core Ownership:** High-level system structure, interfaces, and technology evaluation.
  * **Key Tasks:**
  * Designs architectural diagrams (component diagrams, class diagrams, sequence flows).
  * Selects the technology stack, libraries, and frameworks based on technical constraints.
  * Defines clean API contracts and data exchange formats between subsystems.
  * Identifies technical risks early (e.g., driver access, latency limits, network barriers).
#### 4. Frontend Developer / UI Lead
  * **Core Ownership:** Client-facing interfaces, user workflows, and accessibility.
  * **Key Tasks:**
  * Builds wireframes, interface mockups, and interactive storyboards.
  * Implements the client UI using the selected desktop or web framework.
  * Manages UI state, client-side input validation, and user feedback (error alerts, loading indicators).
#### 5. Backend Developer / Database Lead
  * **Core Ownership:** Core business logic, data persistence, and server infrastructure.
  * **Key Tasks:**
  * Designs the database schema (ER diagrams, table relationships, index design).
  * Implements secure business logic, validation rules, and network endpoints.
  * Ensures data integrity, session management, and transaction safety.
#### 6. QA / Testing Lead
  * **Core Ownership:** Verification, validation, defect tracking, and software correctness.
  * **Key Tasks:**
  * Authors the Master Test Plan and test cases (unit, integration, system, regression).
  * Enforces testing requirements before code merges to the main branch.
  * Maintains the team bug tracker (logging reproduction steps, severity, and status).
  * Directs User Acceptance Testing (UAT) sessions.
#### 7. Documentation / Deployment Lead (DevOps)
  * **Core Ownership:** Environment consistency, build automation, and documentation quality.
  * **Key Tasks:**
  * Configures version control workflows (branch protection, pull request templates).
  * Sets up automated build pipelines or continuous integration (CI) runners.
  * Organizes and formats formal project documentation (SRS, SDD, User Guides).
  * Manages deployment environments (Docker containers, cloud instances, desktop build artifacts).
  
  ---
## 3. Team Policies and Internal Governance
  
  Before writing software, an engineering team must define its operating policies. Having clear policies written down prevents interpersonal conflict and ambiguity when deadlines approach.
  
  ```
  +--------------------------------------------------------------------------+
  |                        ESSENTIAL TEAM POLICIES                           |
  +--------------------+-----------------------------------------------------+
  | Meeting Schedule   | • At least one weekly in-person/hybrid sync.        |
  |                    | • Fixed weekly agenda: Accomplished, Blocked, Next. |
  +--------------------+-----------------------------------------------------+
  | Attendance &       | • Mandatory notification 24 hours prior to missing. |
  | Accountability     | • Inaction policy: Documented warnings before      |
  |                    |   escalating unresolved non-performance to faculty. |
  +--------------------+-----------------------------------------------------+
  | Communication      | • Primary synchronous tool: Discord / Slack.        |
  | Standards          | • Response SLA: e.g., within 24 hours on weekdays.  |
  |                    | • Professional tone; critical decisions recorded.  |
  +--------------------+-----------------------------------------------------+
  | Version Control    | • Trunk-based or GitHub Flow: No direct main push.  |
  | Protocol           | • PR Requirement: Minimum of 1 peer code review.    |
  |                    | • Clean commit messages tied to issue ticket IDs.   |
  +--------------------+-----------------------------------------------------+
  | Quality Standards  | • "Definition of Done" (DoD) checklist:             |
  |                    |   1. Code passes all automated tests.               |
  |                    |   2. Meets documented acceptance criteria.          |
  |                    |   3. Documented in README / API docs.               |
  |                    |   4. Peer reviewed and approved.                    |
  +--------------------+-----------------------------------------------------+
  ```
### Git Workflow Best Practices
  To maintain repository integrity, capstone teams should avoid committing directly to the default branch.
  
  ```
           [ Feature Branch: feature/controller-input ]
                     o --- o --- o
                    /             \  (Pull Request + Code Review)
  --- main ---------o----------------o------------------------->
  ```
  
  1. **Main Branch:** Always stable and deployable.
  2. **Feature Branches:** Named systematically (e.g., `feat/room-code-gen`, `fix/audio-desync`, `docs/srs-section-1`).
  3. **Pull Requests:** Must document the changes made, the issue number addressed, and manual testing performed.
  
  ---
## 4. Shared Project Infrastructure and Artifacts
  
  Every team is required to maintain organized, accessible resources so that work is transparent to team members, clients, and instructors.
  
  ```
  +-------------------------------------------------------------------------+
  |                      CAPSTONE ARTIFACT ECOSYSTEM                        |
  +-------------------------------------------------------------------------+
  | 1. Code & Tracking   | GitHub Repository, Issue Boards, Milestones      |
  | 2. Documentation     | Google Drive / OneDrive, Markdown in repo        |
  | 3. Architecture      | Figma (UI), PlantUML / Mermaid / Draw.io (UML)   |
  | 4. Formal Deliverables| SRS, Software Design Document, Test Plan, Code   |
  | 5. Public Presence   | Project Website / GitHub Pages Portfolio         |
  +-------------------------------------------------------------------------+
  ```
### The Project Website / Repository Readme
  The public landing page for the project must communicate the engineering value quickly:
  * **Project Title and Summary:** A clear explanation of what the software solves.
  * **Team Roster:** Names, roles, and professional links (LinkedIn, GitHub).
  * **System Architecture Overview:** High-level diagrams explaining how components communicate.
  * **Current Status & Milestones:** Progress updates marking phase transitions.
  * **Visual Demonstration:** Screenshots, UI wireframes, or short video captures of working features.
  
  ---