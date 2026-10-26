## 1. Overview of the 5-Step Construction Process
  
  Slide 18 outlines the formal engineering methodology used to build a complete, traceable use-case model:
  
  ```
  +--------------------------------------------------------------------------+
  |                  THE 5-STEP USE-CASE MODELING PROCESS                    |
  +--------------------------------------------------------------------------+
  | Step 1: Identify and Describe the Actors                                 |
  | Step 2: Identify the Use Cases and Write a Brief Description             |
  | Step 3: Identify the Actor and Use-Case Relationships                    |
  | Step 4: Outline the Individual Use Cases (Basic & Alternative Flows)     |
  | Step 5: Refine the Use Cases (Pre/Post-Conditions & Exception Handling)  |
  +--------------------------------------------------------------------------+
  ```
  
  Steps 1 through 3 establish the **architectural perimeter and structural associations** of the system. Steps 4 and 5 specify the **step-by-step behavioral dialogues**.
  
  ---
## 2. Step 1: Identify and Describe the Actors
  
  Identifying actors establishes the boundaries of the system by determining who and what lives outside the software perimeter.
  
  Slide 19 emphasizes looking beyond ordinary desktop users by considering specialized industrial domains (e.g., chemical plants, nuclear power generation, renewable energy grids, automated fish farms). In these environments, hardware devices, physical sensors, and regulatory reporting networks often act as primary or supporting actors.
  
  ```
  +--------------------------------------------------------------------------+
  |            THE SIX ACTOR DISCOVERY QUESTIONS (SLIDE 21)                  |
  +--------------------------------------------------------------------------+
  | 1. Who uses the system?                                                  |
  |    • Direct operators carrying out primary business functions.           |
  |                                                                          |
  | 2. Who gets info from the system?                                        |
  |    • Passive recipients, auditors, reporting engines, monitoring staff.  |
  |                                                                          |
  | 3. Who provides info to the system?                                      |
  |    • External data providers, third-party feeds, manual data clerks.     |
  |                                                                          |
  | 4. Where in the company is the system used?                              |
  |    • Identifies operational departments (e.g., billing, logistics, HR). |
  |                                                                          |
  | 5. Who supports and maintains the system?                                |
  |    • Database administrators, backup operators, security technicians.   |
  |                                                                          |
  | 6. What other systems use this system?                                   |
  |    • External payment gateways, legacy databases, cloud auth providers.  |
  +--------------------------------------------------------------------------+
  ```
### Documenting the Actor Description
  Every identified actor must be documented with a brief textual profile:
  * **Role Name:** Unique, singular noun phrase (e.g., `Facility Coordinator`, not `Manage Facilities`).
  * **Operational Scope:** A 1- to 2-sentence summary of what this entity does and why it interacts with the system.
  * **Privilege Level:** Authentication and permission boundaries associated with the role.
  
  ---
## 3. Step 2: Identify Use Cases and Write Brief Descriptions
  
  Once actors are cataloged, engineers identify what those actors need the system to accomplish.
### The Formal Naming Standard (Slide 23–24)
  A use-case name must be concise and adhere to a strict linguistic pattern:
  * Must consist of a few words starting with an **active verb** followed by a specific **noun phrase**.
  * Must communicate what the actor **achieves** (the value delivered), not the technical mechanism.
  * *Correct Names:* `Withdraw Cash`, `Register Student`, `Submit Order`, `Calibrate Instrument`.
  * *Incorrect Names:* `Cash Management` (noun phrase, no verb), `Click Submit Button` (GUI-focused implementation detail), `Process Data` (vague, conveys no observable value).
  
  ```
  +--------------------------------------------------------------------------+
  |            THE FOUR USE-CASE DISCOVERY INQUIRIES (SLIDE 25)              |
  +--------------------------------------------------------------------------+
  | 1. What will the actor use the system for?                               |
  |    • Primary business tasks and user goals.                              |
  |                                                                          |
  | 2. Will the actor create, store, change, remove, or read data (CRUD)?   |
  |    • Foundational data lifecycle events initiated by the user.           |
  |                                                                          |
  | 3. Will the actor need to inform the system about external events?       |
  |    • Inbound stimuli (e.g., flight cancelled, sensor tripped, check-in). |
  |                                                                          |
  | 4. Will the actor need to be informed about system occurrences?          |
  |    • Outbound stimuli (e.g., low-balance alerts, appointment reminders). |
  +--------------------------------------------------------------------------+
  ```
### The Initial Brief Description
  For each candidate use case, write a 2- to 3-sentence summary establishing scope:
  * *Example (`Submit Promotion Order`):* "Describes the process when a Club Member submits a promotional product order, indicating desired items and quantities. The system verifies member standing, checks inventory availability, and creates a warehouse picking ticket." (Slide 5).
  
  ---
## 4. Step 3: Identify Actor and Use-Case Relationships
  
  Slide 27 defines the fundamental rules governing how actors link to use cases:
  
  ```
  +--------------------------------------------------------------------------+
  |                THE INITIATION CARDINALITY RULE (SLIDE 27)                |
  +--------------------------------------------------------------------------+
  | 1. Only ONE actor can INITIATE a use case.                               |
  |    • A use case represents a single goal initiated by a specific role.   |
  |                                                                          |
  | 2. However, MANY actors can be INVOLVED in that use case.                |
  |    • Secondary actors, external APIs, and supervisors can participate,   |
  |      receive notifications, or provide validation services.             |
  |                                                                          |
  | 3. Rule of Thumb: If relationships become complex, USE DIAGRAMS.         |
  +--------------------------------------------------------------------------+
  ```
  
  ---
## 5. Dependency Relationships Between Use Cases: Include vs. Extend
  
  When modeling large systems, patterns of user interaction repeat, and certain workflows branch into optional paths. UML provides two specialized dependency relationships to manage this complexity (Slide 28).
  
  ```
  +--------------------------------------------------------------------------+
  |                  INCLUDE VS. EXTEND DEPENDENCY MATRIX                    |
  +-------------------+------------------------------------------------------+
  | Dimension         | Include (<<include>>)                                |
  +-------------------+------------------------------------------------------+
  | Nature            | Mandatory, unconditional insertion of shared logic.  |
  | Arrow Direction   | Base Use Case points dashed arrow TO included case.  |
  | Execution Trigger | Always runs automatically when base use case runs.   |
  | Purpose           | Reusability; eliminates redundant duplicate text.    |
  +-------------------+------------------------------------------------------+
  | Dimension         | Extend (<<extend>>)                                  |
  +-------------------+------------------------------------------------------+
  | Nature            | Optional, conditional augmentation of base logic.    |
  | Arrow Direction   | Extending Use Case points dashed arrow BACK to base. |
  | Execution Trigger | Runs only if a specific condition/guard evaluates    |
  |                   | to true at a defined Extension Point.                |
  | Purpose           | Simplifies maintenance; isolates optional behavior.  |
  +-------------------+------------------------------------------------------+
  ```
  
  ---
### The Include Relationship (`<<include>>`)
  Slide 30 explains that certain user and system behaviors reoccur across many different functional areas:
  * Examples: entering credentials/passwords, running database integrity checks, searching records.
  * Rather than rewriting "enter username and password" inside twenty separate use case specifications, that common behavior is factored out into a standalone included use case (e.g., `Validate PIN` or `Authenticate User`).
  * **Visual Representation:** The arrow points from the base use case **toward** the reusable inclusion:
  
  ```
  ( Withdraw Funds ) -------------\
                                 \  <<include>>
                                  v
  ( Transfer Funds ) -------------> ( Validate PIN )
  ```
  
  *Execution Law:* The base use case **cannot complete successfully** without executing the included use case.
  
  ---
### The Extend Relationship (`<<extend>>`)
  Slide 28 and 29 define `<<extend>>` as a mechanism where instances of a base use case are augmented by additional behavior in an extending use case.
  
  Slide 29 provides three explicit architectural reasons to use `<<extend>>`:
  1. **Simplifies Maintenance:** Isolates complex, rarely used, or exceptional logic so developers can focus on maintaining the clean baseline workflow.
  2. **Indicates Future Intent:** Engineers can declare **Extension Points** in the base use case during Phase 1, marking exact points where new functional modules will plug in during future development cycles.
  3. **Represents Truly Optional Behavior:** Captures workflows that only execute under specific conditions, rather than forcing them into the normal success path.
  
  ```
                                      <<extend>>
  ( Defer Payment ) -------------------------------------------------> ( Pay Bill )
                                                         [Extension Point: more treatment]
  ```
  
  *Notice the Arrow Direction:* In UML, the arrow points **from the optional extending use case back to the base use case**. The base use case is completely functional on its own and has no direct knowledge of the extension; the extension hooks into the base use case at the named extension point.
  
  ---
## 6. Advanced Diagramming: Actor Generalization (Slide 31 Case Study)
  
  Slide 31 illustrates an Airport Check-In system from `uml-diagrams.org` that introduces **Actor Generalization** alongside `<<include>>` and `<<extend>>`:
  
  ```
  +--------------------------------------------------------------------------+
  |                            «Business» Airport                            |
  +--------------------------------------------------------------------------+
  |                                                                          |
  |         \o/ -------------------> ( Group Check-In )                      |
  |      Tour Guide                      |                                   |
  |          |                           | <<include>>                       |
  |    Generalization                    v                                   |
  |          |                   ( Individual Check-In ) <===========        |
  |          v                           ^                          |        |
  |         \o/ -------------------------/                          |        |
  |      Passenger        1..*                                      |        |
  |          \                                           <<extend>> |        |
  |           \                                                     |        |
  |            \ 1..*                                               |        |
  |             \----------> ( Security Screening )          ( Baggage       |
  |                                1                          Check-In )     |
  +--------------------------------------------------------------------------+
  ```
### Analysis of Architectural Elements in Slide 31:
  1. **Actor Generalization (Inheritance):** Represented by a solid line with a hollow triangular arrowhead pointing from `Tour Guide` to `Passenger`.
   * *Meaning:* A `Tour Guide` **is a** `Passenger`. The `Tour Guide` inherits all permissions and associations that belong to a regular passenger (e.g., `Security Screening`), but has unique privileges unavailable to standard passengers (`Group Check-In`).
  2. **Include Dependency:** `Group Check-In` points an `<<include>>` arrow to `Individual Check-In`. A group check-in is composed of multiple individual passenger check-ins executed sequentially.
  3. **Extend Dependency:** `Baggage Check-In` points an `<<extend>>` arrow back to `Individual Check-In`. Not every passenger checks luggage (many carry only personal items). Thus, luggage intake is optional behavior that extends the basic check-in workflow.
  4. **Multiplicity Labels:** The lines contain cardinality notations (`1..*` meaning one or more, and `1` meaning exactly one), indicating that one or more passengers pass through a single security screening checkpoint.
  
  ---