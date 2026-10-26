## 1. What is Requirements Engineering?
  
  Requirements engineering (RE) is the systematic process of establishing the services that a customer requires from a system and the constraints under which it operates and is developed. 
  
  ```
  +--------------------------------------------------------------------------+
  |                     REQUIREMENTS DEFINITIONS (SLIDE 5)                   |
  +--------------------------------------------------------------------------+
  | Requirements Engineering Process:                                        |
  |   The process of discovering, analyzing, documenting, and validating     |
  |   the services and operational constraints of a system.                  |
  |                                                                          |
  | System Requirements:                                                     |
  |   The formal descriptions of the system services and constraints         |
  |   produced during the requirements engineering process.                  |
  +--------------------------------------------------------------------------+
  ```
### The Dual Function of Requirements (Slide 10)
  A requirement can range from a high-level abstract statement to a detailed mathematical specification. This variation exists because requirements must serve two distinct commercial functions:
  1. **The Basis for a Bid for a Contract:** Early in procurement, requirements must be stated at an abstract level so multiple contractors can bid on the contract, proposing different architectural solutions and competitive pricing.
  2. **The Basis for the Contract Itself:** Once a contractor is selected, the requirements must be detailed, unambiguous, and mathematically verifiable so both client and contractor share an enforceable legal baseline of what will be delivered.
  
  ---
## 2. Problem Domain vs. Solution Domain
  
  Slide 8 presents the requirements hierarchy as a pyramid bridging the customer's operational reality with the engineer's technical build.
  
  ```
                           / \
                          /   \
                         /     \
                        / Needs \          <-- PROBLEM DOMAIN (Customer Space)
                       +---------+
                      /  Features \        \
                     +-------------+        +-- SOLUTION DOMAIN (Technical Space)
                    /    Software   \      /
                   /  Requirements   \    /
                  +-------------------+
  ```
### 1. The Problem Domain (Needs)
  * **What it represents:** The real-world operational environment, pain points, and business objectives of the customer.
  * **Characteristics:** Exists independently of any software system. Needs describe *why* something is broken or inefficient (e.g., "Patients are waiting too long for prescriptions" or "Student organizations double-book campus auditoriums").
  * **Target Audience:** Business stakeholders, clinic directors, end users.
### 2. The Solution Domain (Features and Software Requirements)
  * **Features (Mid-Level):** High-level capabilities that the system must provide to satisfy user needs (e.g., "Automated drug cost reporting" or "Real-time room availability calendar").
  * **Software Requirements (Base Level):** Formal, testable specifications that describe exactly what developers must build. A software requirement is a capability that:
  1. Is needed by a user to solve a problem or achieve an objective.
  2. Must be met or possessed by a system or system component to satisfy a contract, standard, or formally imposed specification (Slide 9).
  
  ---
## 3. The Requirements Lifecycle
  
  Slide 7 outlines the lifecycle through which requirements evolve from rough ideas into binding engineering agreements:
  
  ```
  [ Start ]
    |
    v
  [ Elicitation ] -----------> [ Analysis ]
      ^                           |
      |                           v
  [ Negotiation / Agreement ] <-- [ Documentation ]
      |
      v
  [ End ]
  ```
  
  1. **Elicitation:** Gathering information from stakeholders, legacy documents, and domain research.
  2. **Analysis:** Deconstructing requirements, identifying ambiguities, uncovering missing scenarios, and checking for technical feasibility.
  3. **Documentation:** Writing down the findings in formal specifications (User and System Requirements).
  4. **Negotiation and Agreement:** Resolving conflicts between competing stakeholder desires and establishing an agreed-upon baseline.
  
  Because requirements are inherently volatile, this lifecycle is an **iterative spiral** (Slide 6). As the system is documented and analyzed, new stakeholder constraints inevitably emerge, requiring subsequent rounds of elicitation.
  
  ---
## 4. User Requirements vs. System Requirements
  
  Slides 15 through 18 establish the two operational tiers of requirements specifications.
  
  ```
  +--------------------------------------------------------------------------+
  |                  USER REQUIREMENTS VS. SYSTEM REQUIREMENTS               |
  +--------------------+-----------------------------------------------------+
  | Dimension          | User Requirements                                   |
  +--------------------+-----------------------------------------------------+
  | Primary Definition | Statements in natural language plus diagrams of the |
  |                    | services the system provides and its operational    |
  |                    | constraints. (Focus: Business Features & Services). |
  +--------------------+-----------------------------------------------------+
  | Target Audience    | Written for customers, client managers, end users,  |
  |                    | and contractor managers (Slide 19).                 |
  +--------------------+-----------------------------------------------------+
  | Language Style     | High-level, understandable by non-technical readers.|
  +--------------------+-----------------------------------------------------+
  | Dimension          | System Requirements                                 |
  +--------------------+-----------------------------------------------------+
  | Primary Definition | A structured document setting out detailed          |
  |                    | descriptions of system functions, services, and     |
  |                    | operational constraints. (Focus: Technical Build).  |
  +--------------------+-----------------------------------------------------+
  | Target Audience    | Written for system end users, client engineers,     |
  |                    | system architects, and software developers (Slide 19)|
  +--------------------+-----------------------------------------------------+
  | Language Style     | Precise, formal, testable, and contract-ready.      |
  +--------------------+-----------------------------------------------------+
  ```
### The Readers Matrix (Slide 19)
  Notice the structural overlap in who reads each document:
  
  ```
  User Requirements Readers:
  • Client managers
  • System end-users
  • Client engineers
  • Contractor managers
  • System architects
  
  System Requirements Readers:
  • System end-users
  • Client engineers
  • System architects
  • Software developers
  ```
  
  *Key Insight:* **Software developers do not read User Requirements to write code.** User requirements are too abstract for implementation. Developers implement against **System Requirements**, which provide exact inputs, outputs, conditions, and constraints. Conversely, business and contract managers rarely read low-level System Requirements.
  
  ---
## 5. Case Study: Mentcare User vs. System Requirements
  
  Slides 17 and 18 demonstrate how a single user requirement decomposes into multiple, testable system requirements within the Mentcare clinic management system.
  
  ```
  +-------------------------------------------------------------------------+
  |                       USER REQUIREMENT DEFINITION                       |
  +-------------------------------------------------------------------------+
  | 1. The Mentcare system shall generate monthly management reports        |
  |    showing the cost of drugs prescribed by each clinic during that      |
  |    month.                                                               |
  +-------------------------------------------------------------------------+
                                    |
                                    v Decomposed into
  +-------------------------------------------------------------------------+
  |                     SYSTEM REQUIREMENTS SPECIFICATION                   |
  +-------------------------------------------------------------------------+
  | 1.1 On the last working day of each month, a summary of the drugs       |
  |     prescribed, their cost, and the prescribing clinics shall be        |
  |     generated.                                                          |
  |                                                                         |
  | 1.2 The system shall generate the report for printing after 17:30 on    |
  |     the last working day of the month.                                  |
  |                                                                         |
  | 1.3 A report shall be created for each clinic and shall list the        |
  |     individual drug names, the total number of prescriptions, the       |
  |     number of doses prescribed, and the total cost of the prescribed    |
  |     drugs.                                                              |
  |                                                                         |
  | 1.4 If drugs are available in different dose units (e.g., 10mg, 20mg),  |
  |     separate reports shall be created for each dose unit.               |
  |                                                                         |
  | 1.5 Access to drug cost reports shall be restricted to authorized users |
  |     as listed on a management access control list.                      |
  +-------------------------------------------------------------------------+
  ```
### Why this decomposition is critical:
  * **Requirement 1.1 and 1.2** define explicit **temporal scheduling** (last working day, post 17:30) so the process does not lock database tables during operating clinic hours.
  * **Requirement 1.3** specifies the **exact data fields** required in the report, eliminating developer guesswork.
  * **Requirement 1.4** handles a **domain edge case**: drugs sold in multiple dosages cannot have their costs lumped together under a single name.
  * **Requirement 1.5** attaches a **security boundary** (Access Control List), converting a general report into an authorized administrative asset.
  
  ---
## 6. System Stakeholders
  
  Slide 20 defines a stakeholder as:
  > **Any person or organization who is affected by the system in some way and so who has a legitimate interest.**
  
  ```
                             +------------------------+
                             |   SYSTEM STAKEHOLDERS  |
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  |    END USERS     |            | SYSTEM MANAGERS  |            |  SYSTEM OWNERS   |
  | • Daily operators|            | • Operational    |            | • Pay for system |
  | • Focus: UI, task|            |   supervisors    |            | • Focus: ROI,    |
  |   efficiency     |            | • Focus: Reports |            |   compliance     |
  +------------------+            +------------------+            +------------------+
                                         |
                                         v
                                +------------------+
                                |     EXTERNAL     |
                                | • Regulators,    |
                                |   certification  |
                                |   boards, public |
                                +------------------+
  ```
### Stakeholder Classes
  1. **End Users:** The direct operators of the software (e.g., nurses entering patient vitals, students registering for classes). They care primarily about usability, speed, and workflow convenience.
  2. **System Managers:** Individuals who supervise the daily operation of the system (e.g., clinic administrators, registrar officers). They care about system availability, staff efficiency, audit reports, and error tracking.
  3. **System Owners:** The business entities or executives who fund the development of the system. They care about financial return on investment (ROI), strategic capability, and system lifecycle cost.
  4. **External Stakeholders:** Parties who never touch the system directly, but whose legal, ethical, or technical constraints dictate system behavior (e.g., health privacy regulators enforcing HIPAA, safety boards, trade unions).
  
  ---
## 7. Introduction to Functional Requirements
  
  Slides 11 through 13 introduce the core division between functional and non-functional requirements.
  
  * **Functional Requirement:** Specifies an **action** that the software product must be able to perform. It defines the core behavior in terms of explicit inputs, processing, and outputs.
### University Registration Example (Slide 13)
  Slide 13 illustrates functional requirements using a standard course registration system:
  
  | Feature Area | Functional Requirement Statement |
  | :--- | :--- |
  | **Login** | The system shall allow students to log in using university credentials. |
  | **Search** | The system shall allow students to search courses by course number, instructor, and department. |
  | **Register** | The system shall allow students to register for an available course. |
  | **Drop** | The system shall allow students to drop a registered course before the deadline. |
  | **Prerequisites** | The system shall verify prerequisites before allowing registration. |
  | **Capacity** | The system shall prevent enrollment when the course reaches maximum capacity. |
  | **Waitlist** | The system shall allow students to join a waitlist for a full course. |
  | **Schedule** | The system shall display the student's current class schedule. |
  
  Notice the standard formal syntax: every requirement uses the imperative verb **"shall"** to denote a mandatory contract deliverable, identifies the **actor**, and specifies the **observable behavior**.
  
  ---