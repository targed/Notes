## 1. Core Philosophy: "Build the Right System, Not Just a Working System"
  
  Slide 2 introduces the fundamental premise of modern requirements engineering:
  > **Build the right system, not just a working system.**
  
  In academic coursework, students are accustomed to receiving pristine, pre-packaged problem descriptions where the only challenge is writing code that compiles and passes test cases. In professional software engineering, **writing code that works is useless if the system solves the wrong problem.**
  
  ```
  +--------------------------------------------------------------------------+
  |                 WHY REQUIREMENTS ENGINEERING MATTERS (SLIDE 2)           |
  +--------------------------------------------------------------------------+
  | 1. Understands the Real Problem: Prevents building features on unverified|
  |    assumptions.                                                          |
  |                                                                          |
  | 2. Reduces Rework and Scope Creep: Fixing an error in requirements costs  |
  |    up to 100 times less than fixing it after deployment.                 |
  |                                                                          |
  | 3. Aligns Competing Groups: Establishes a shared baseline between end     |
  |    users, business clients, and technical development teams.             |
  |                                                                          |
  | 4. Improves Downstream Activities: High-quality requirements directly    |
  |    drive system architecture, test case generation, and sprint planning. |
  |                                                                          |
  | 5. Maximizes Real-World Utility: Ensures the delivered software survives |
  |    operational reality and satisfies actual user workflows.             |
  +--------------------------------------------------------------------------+
  ```
  
  ---
## 2. The Top 5 Requirements Techniques in Depth
  
  Slide 3 outlines the five essential requirements techniques that every engineering student must master:
  
  ```
                             +------------------------+
                             |   TOP 5 RE TECHNIQUES  |
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  | 1. STAKEHOLDER   |            |  2. OBSERVATION  |            | 3. USER STORIES  |
  |    INTERVIEWS    |            |   & WORKFLOW     |            |   & USE CASES    |
  +------------------+            +------------------+            +------------------+
  | Best for: Early  |            | Best for: Hidden |            | Best for: System |
  | needs discovery  |            | process flaws    |            | feature bridge   |
  +------------------+            +------------------+            +------------------+
         |                                                               |
         +-------------------------------+-------------------------------+
                                         |
                         +---------------+---------------+
                         |                               |
                         v                               v
              +--------------------+           +--------------------+
              | 4. STORYBOARDING   |           |   5. WORKSHOPS     |
              |   & PROTOTYPING    |           |  & BRAINSTORMING   |
              +--------------------+           +--------------------+
              | Best for: Early    |           | Best for: Alignment|
              | "Yes, but..." data |           | and prioritization |
              +--------------------+           +--------------------+
  ```
  
  ---
### Technique 1: Stakeholder Interviews (Slide 4)
  * **Core Purpose:** Understand the problem domain directly from users, clients, or domain experts.
  * **Tag:** *Best for discovering needs early.*
  * **Methodology:**
  * Avoid closed yes/no questions. Ask open-ended questions targeting operational friction:
    * *"What problem are you trying to solve, and why does it matter?"*
    * *"What is the most frustrating part of your current daily workflow?"*
    * *"What happens if this process fails or is delayed?"*
  * Engineers must listen actively for four distinct signals: **underlying needs, operational constraints, unstated assumptions, and repeated user complaints**.
  * **Student Outputs:** Formal interview transcripts/notes, a categorized stakeholder register, and an initial catalog of raw requirements.
  
  ---
### Technique 2: Observation and Workflow Analysis (Slide 5)
  * **Core Purpose:** Watch how users actually execute tasks in their real working environment.
  * **Tag:** *Best for discovering hidden process issues.*
  * **Why Interviews Alone Fail (The Tacit Knowledge Problem):**
  * When interviewed, workers describe the *ideal, documented* procedure.
  * During actual execution, workers forget intermediate steps, skip manual verifications, or use undocumented workarounds because the official process is too slow.
  * **What to Observe:**
  * **Handoffs:** Points where data moves from one person or tool to another (the highest source of data corruption).
  * **Bottlenecks:** Stages where work queues up waiting for manual approval.
  * **Repeated Work:** Redundant data re-entry (e.g., typing spreadsheet data into a web form).
  * **Error Points:** Where users make common slips and how they recover.
  * **Student Outputs:** Current-state workflow diagrams (As-Is process models), documented operational pain points, and specific optimization opportunities.
  
  ---
### Technique 3: User Stories and Use Cases (Slide 6)
  * **Core Purpose:** Convert unstructured user needs into clear, actionable, and testable engineering specifications.
  * **Tag:** *Good bridge between user needs and system features.*
  * **The User Story Standard:**
  $$\text{As a } [\text{type of user}], \text{ I want } [\text{an action/goal}], \text{ so that } [\text{a business benefit}].$$
  * *Example:* "As a student, I want to cancel an appointment so that another student can use that time slot."
  * *Analysis:* The "so that" clause is the most critical element; it provides the **rationale** justifying why the engineering team should spend time building the feature.
  * **Connecting Stories to Use Cases:**
  * User stories define the agile feature backlog and acceptance criteria.
  * Use cases provide the step-by-step sequential dialogue, detailing preconditions, primary interactions, alternative branches, and postconditions.
  * **Student Outputs:** Prioritized product backlogs, user stories with explicit acceptance criteria, and formal use case specifications.
  
  ---
### Technique 4: Storyboarding and Prototyping (Slide 7)
  * **Core Purpose:** Provide visual representations of the proposed system to collect concrete reactions before committing to code.
  * **Tag:** *Best for early "Yes, but..." feedback.*
  * **Methodology:**
  * Abstract text specifications allow users to nod along without understanding interface implications. Presenting a visual workflow forces users to evaluate whether the software actually supports their daily tasks.
  * *The Diagnostic Question:* "If the system worked like this, would it support your needs?"
  * **Forms Used:**
  * *Passive:* Static sketches, screen sequences, or paper mockups.
  * *Active:* Animated slide decks or automated walkthrough simulations.
  * *Interactive:* Clickable wireframes (Figma, HTML mockups) requiring active user interaction.
  * **Student Outputs:** UI wireframes, screen layout boards, or interactive prototypes paired with documented stakeholder critique logs.
  
  ---
### Technique 5: Requirements Workshops and Brainstorming (Slide 8)
  * **Core Purpose:** Bring diverse, conflicting stakeholders and technical team members into a structured room to negotiate scope and establish delivery baselines.
  * **Tag:** *Best for alignment and prioritization.*
  * **Methodology:**
  * Conducted after initial requirements have been gathered via interviews and observation.
  * Uses collaborative tools: sticky-note clustering (affinity grouping), feature dot-voting, scenario walkthroughs, and trade-off matrices.
  * **Why Workshops are Essential:**
  * When engineers communicate with stakeholders individually, stakeholders make unbounded demands.
  * In a workshop, stakeholders must defend their feature requests in front of peers, exposing competing priorities (e.g., Security demanding extra validation steps while End Users demand one-click checkouts).
  * **Student Outputs:** Prioritized feature matrices (MoSCoW), architectural decision logs, and an agreed-upon MVP boundary.
  
  ---
## 3. The Integration Pipeline: How the Techniques Fit Together
  
  Slide 9 models how these five techniques form a coherent, continuous engineering pipeline:
  
  ```
  +---------------------------------------------------------------------------------------+
  |                    THE REQUIREMENTS ENGINEERING PIPELINE (SLIDE 9)                    |
  +-------------------+-------------------+-------------------+---------------------------+
  | Stage             | Applied Technique | Primary Objective                             |
  +-------------------+-------------------+-------------------+---------------------------+
  | 1. DISCOVER       | Interviews and    | Uncover real-world operational problems,      |
  |                   | Observation       | stakeholder pain points, and domain rules.    |
  +-------------------+-------------------+-------------------+---------------------------+
  | 2. SPECIFY        | User Stories and  | Translate raw discovery notes into structured,|
  |                   | Use Cases         | actionable, and testable functional baselines.|
  +-------------------+-------------------+-------------------+---------------------------+
  | 3. PROTOTYPE      | Storyboards and   | Convert textual specifications into visual    |
  |                   | Wireframes        | workflows to validate navigation and layout.  |
  +-------------------+-------------------+-------------------+---------------------------+
  | 4. VALIDATE &     | Requirements      | Bring stakeholders together to resolve scope  |
  |    REFINE         | Workshops         | conflicts, prioritize tasks, and sign off.    |
  +-------------------+-------------------+-------------------+---------------------------+
  ```
  
  ---
## 4. Capstone I: Team Execution Checklist
  
  Slide 10 provides the explicit academic requirements that every capstone engineering team must complete during Phase 1:
  
  ```
  +--------------------------------------------------------------------------+
  |                  CAPSTONE I REQUIREMENTS CHECKLIST (SLIDE 10)            |
  +--------------------------------------------------------------------------+
  | [ ] 1. Stakeholder & Problem Definition:                                 |
  |        Explicitly document who experiences the problem and why existing  |
  |        solutions fail.                                                   |
  |                                                                          |
  | [ ] 2. Dual Elicitation Requirement:                                     |
  |        Execute at least TWO distinct elicitation techniques (e.g.,       |
  |        conduct stakeholder interviews AND perform workflow observation). |
  |                                                                          |
  | [ ] 3. Agile Specification:                                              |
  |        Author between 5 and 10 detailed User Stories, each backed by     |
  |        testable Acceptance Criteria (Given/When/Then or checklists).     |
  |                                                                          |
  | [ ] 4. Visual Workflow Prototype:                                        |
  |        Construct a storyboard or low-fidelity prototype modeling at least|
  |        one primary end-to-end user workflow.                             |
  |                                                                          |
  | [ ] 5. Iterative Refinement:                                             |
  |        Validate the requirements with real feedback, log revisions, and  |
  |        treat documentation as an evolving engineering baseline.          |
  +--------------------------------------------------------------------------+
  ```
  
  ---