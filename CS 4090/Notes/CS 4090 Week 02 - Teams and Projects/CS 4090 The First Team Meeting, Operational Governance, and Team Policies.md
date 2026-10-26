## 1. Establishing the Team Workspace and Operational Hub
  
  Before an engineering team writes specifications or code, they must establish their shared digital infrastructure. Slides 4 and 9 list four essential elements of group setup:
  
  ```
  +-------------------------------------------------------------------------+
  |                       SHARED TEAM INFRASTRUCTURE                        |
  +----------------------+--------------------------------------------------+
  | Platform / Tool      | Operational Purpose                              |
  +----------------------+--------------------------------------------------+
  | Canvas Group         | Official university grade portal, formal group   |
  |                      | submissions, and institutional roster records.   |
  +----------------------+--------------------------------------------------+
  | Synchronous Chat     | Day-to-day team coordination, rapid technical    |
  | (Discord / Slack)    | troubleshooting, and real-time meeting syncs.    |
  +----------------------+--------------------------------------------------+
  | Version Control      | Centralized code hosting, issue tracking, peer   |
  | (GitHub Organization)| code reviews, and automated CI/CD workflows.     |
  +----------------------+--------------------------------------------------+
  | Document Repository  | Cloud document store (Google Drive / OneDrive)   |
  |                      | for SRS drafts, design diagrams, and meeting     |
  |                      | minutes.                                         |
  +----------------------+--------------------------------------------------+
  ```
  
  A common failure mode in capstone projects is fragmented communication (e.g., technical discussions split across text messages, personal emails, and verbal conversations). Establishing a single official communications server with designated channels (such as `#announcements`, `#frontend`, `#backend`, `#testing`, and `#resources`) ensures all design decisions remain documented and searchable.
  
  ---
## 2. The Eight Mandatory Team Policies (The Team Charter)
  
  Slide 10 specifies eight operational policies that every capstone team must define during their first formal meeting. In software engineering management, this document is known as the **Team Charter** or **Working Agreement**. It acts as a binding internal contract designed to prevent misunderstandings and eliminate friction before high-pressure deadlines occur.
  
  ```
                             +------------------------+
                             |   THE TEAM CHARTER     |
                             |  (Working Agreement)   |
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  |  COMMUNICATION   |            |   DEVELOPMENT    |            |    ACCOUNTABILITY|
  |  & ATTENDANCE    |            |   & STANDARDS    |            |    & RESOLUTION  |
  +------------------+            +------------------+            +------------------+
  | • Meeting Sched  |            | • GitHub Rules   |            | • Task Ownership |
  | • Absence Notice |            | • Quality / DoD  |            | • Deadline Mgmt  |
  | • Response SLAs  |            | • Code Reviews   |            | • Conflict Escal |
  +------------------+            +------------------+            +------------------+
  ```
  
  ---
### Policy 1: Meeting Schedule
  * **Purpose:** Establish a predictable, non-negotiable rhythm for team synchronization.
  * **Implementation Standards:**
  * Define at least one weekly in-person or high-bandwidth video meeting outside of scheduled class time.
  * Establish fixed 10-minute weekly standup sessions (either synchronous or asynchronous) where each member states:
    1. What did I complete since the last sync?
    2. What am I working on next?
    3. What technical blockers are in my way?
  * Designate a rotating note-taker to record meeting minutes, action items, and assigned owners.
### Policy 2: Attendance Expectations
  * **Purpose:** Ensure all members contribute to architectural decisions and prevent passive disengagement.
  * **Implementation Standards:**
  * **Advance Notice Rule:** If an engineer cannot attend a scheduled meeting, they must provide at least 24 hours of advance notice in the team chat.
  * **Asynchronous Obligation:** Absent members must review recorded meeting minutes and post their status updates within 12 hours of the missed session.
  * **Unexcused Absences:** Establish a clear threshold (e.g., two unexcused absences trigger an internal review with the Project Manager).
### Policy 3: Communication Expectations
  * **Purpose:** Eliminate communication blackouts where team members disappear for days.
  * **Implementation Standards:**
  * **Response Time SLA (Service Level Agreement):** Define clear response windows (e.g., within 24 hours on weekdays, within 12 hours during sprint deadline weeks).
  * **Platform Discipline:** Technical discussions must occur in open channels, not direct messages, so the entire team retains visibility into architecture changes.
  * **Acknowledging Messages:** Establish a convention (such as reacting with a checkmark emoji) to confirm that an announcement has been read and understood.
### Policy 4: Task Assignment
  * **Purpose:** Prevent ambiguous task ownership where everyone assumes someone else is handling a deliverable.
  * **Implementation Standards:**
  * All work must be represented as a trackable ticket on a Kanban board (GitHub Projects, Jira, or Trello).
  * **Single Assignee Rule:** Every ticket must have exactly one primary owner. Multiple people can collaborate, but one person remains accountable for driving the task to completion.
  * Tasks must include clear descriptions, references to functional requirement IDs (e.g., `FR-AUTH-01`), and estimated completion times.
### Policy 5: Conflict Resolution
  * **Purpose:** Provide a systematic escalation path when interpersonal or technical disputes arise, rather than letting resentment build.
  * **The Three-Tier Escalation Model:**
  
  ```
  [ Step 1: Direct 1-on-1 Resolution ]
  • The affected parties meet privately to address the specific issue objectively.
  • Focus on code, behaviors, or missed milestones, not personal attacks.
                  |
                  v  (If unresolved within 48 hours)
  [ Step 2: Internal Team Mediation ]
  • The Project Manager and Product Owner facilitate a structured team discussion.
  • Review the working agreement, examine objective contribution data (commits, PRs).
  • Establish an explicit performance remediation plan with a 7-day timeline.
                  |
                  v  (If non-compliance persists)
  [ Step 3: Faculty Escalation ]
  • The Project Manager brings documented evidence of non-performance to the instructor.
  • Formal grading intervention or administrative team adjustments occur.
  ```
### Policy 6: GitHub and Version Control Usage
  * **Purpose:** Protect the integrity of the codebase and establish production-level Git hygiene.
  * **Implementation Standards:**
  * **Branch Protection:** Direct commits and force-pushes to the `main` or `production` branch are strictly disabled in repository settings.
  * **Branch Naming Standard:** All work must occur on descriptive feature branches (e.g., `feat/webrtc-stream`, `fix/login-null-pointer`, `docs/srs-section-3`).
  * **Pull Request (PR) Requirements:**
    * Every PR must describe changes made and link to the relevant issue ticket.
    * A minimum of one peer review approval is required before merging.
    * The author cannot approve their own pull request.
  * **Merge Strategy:** Use Squash and Merge or rebase to keep a clean, linear git commit history.
### Policy 7: Deadline Management
  * **Purpose:** Prevent deadline slippage from cascading through the two-semester timeline.
  * **Implementation Standards:**
  * **Internal Buffers (Soft Deadlines):** Set internal team completion dates 48 to 72 hours ahead of the official course deadline. This provides a safety margin for unexpected integration errors or document formatting issues.
  * **Early Warning Rule:** If an engineer realizes a task will take longer than estimated, they must flag the delay on the task board at least 48 hours before the soft deadline so the team can reallocate resources.
### Policy 8: Quality Standards (Definition of Done)
  * **Purpose:** Ensure that code is truly production-ready, not merely running on an individual developer's laptop.
  * **The Definition of Done (DoD) Checklist:**
  A feature is only considered "Done" when:
  1. The code adheres to agreed style guides and passes static analysis / linting without errors.
  2. The code compiles cleanly on all supported target platforms (Windows, Linux, macOS).
  3. Accompanying unit tests are written and pass automated CI checks.
  4. The feature satisfies all documented Acceptance Criteria in the user story.
  5. The API or module documentation is updated.
  6. The pull request has been reviewed, approved, and merged into the main branch.
  
  ---
## 3. Operational Roles and Day-to-Day Responsibilities
  
  Slide 10 outlines seven critical functional areas. While every team member writes code, role leads ensure that their technical domain remains organized and complete throughout the SDLC.
  
  ```
  +--------------------------------------------------------------------------+
  |                  TEAM ROLES AND SPRINT RESPONSIBILITIES                  |
  +-------------------------+------------------------------------------------+
  | Role                    | Primary Focus During Development Sprints       |
  +-------------------------+------------------------------------------------+
  | Project Manager /       | Tracks sprint burndown, manages board tickets, |
  | Scrum Master            | identifies bottlenecks, and leads standups.    |
  +-------------------------+------------------------------------------------+
  | Product Owner /         | Writes acceptance criteria, clarifies user     |
  | Requirements Lead       | stories, defends scope against bloat.          |
  +-------------------------+------------------------------------------------+
  | System Architect /      | Enforces module boundaries, defines API        |
  | Design Lead             | contracts, audits system diagrams and schemas. |
  +-------------------------+------------------------------------------------+
  | Frontend Developer /    | Builds user interfaces, handles client state,  |
  | UI Lead                 | optimizes user experience and accessibility.   |
  +-------------------------+------------------------------------------------+
  | Backend Developer /     | Implements business logic, data persistence,   |
  | Database Lead           | server endpoints, and query optimization.      |
  +-------------------------+------------------------------------------------+
  | QA / Testing Lead       | Builds test harnesses, writes test cases,      |
  |                         | manages the defect log, audits edge cases.     |
  +-------------------------+------------------------------------------------+
  | Documentation /         | Configures CI/CD runners, manages environment  |
  | Deployment Lead         | builds, formats formal course deliverables.    |
  +-------------------------+------------------------------------------------+
  ```
  
  ---
## 4. First Team Meeting Agenda Template
  
  To ensure the first formal meeting yields concrete results, teams should follow a structured agenda:
  
  1. **Role Confirmation (10 min):** Review members' skills and confirm leadership assignments.
  2. **Project Scope Discussion (15 min):** Review the project title, identify the primary problem statement, and establish a high-level MVP feature list.
  3. **Drafting Team Policies (25 min):** Step through the eight mandatory policies, reach consensus, and draft the written working agreement.
  4. **Tooling Setup (15 min):** Verify that every member has joined the GitHub organization, the communication server, the shared cloud drive, and the Canvas group.
  5. **Action Item Allocation (5 min):** Assign initial Phase 1 research and requirements tasks with explicit owners and completion dates.
  
  ---