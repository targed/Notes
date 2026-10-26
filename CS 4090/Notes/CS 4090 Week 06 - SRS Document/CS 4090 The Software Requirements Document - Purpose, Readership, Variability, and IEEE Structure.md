## 1. What is the Software Requirements Document?
  
  The **Software Requirements Document** (commonly known as the **Software Requirements Specification**, or **SRS**) is the official statement of what the system developers must implement. It represents the foundational engineering contract between clients, system users, project managers, and software engineers.
  
  Slide 36 establishes the core boundary governing requirements documentation:
  > **The software requirements document is NOT a design document. As far as possible, it should state WHAT the system should do rather than HOW it should do it.**
  
  ```
  +--------------------------------------------------------------------------+
  |                  THE BOUNDARY: REQUIREMENTS VS. DESIGN                   |
  +---------------------+----------------------------------------------------+
  | Software            | • Focuses on WHAT the system must deliver.         |
  | Requirements (SRS)  | • Defines user services, operational constraints,   |
  |                     |   data entities, and external interfaces.          |
  |                     | • Written from an external black-box perspective.  |
  +---------------------+----------------------------------------------------+
  | Software            | • Focuses on HOW the system accomplishes tasks.    |
  | Design (SDD)        | • Defines internal algorithms, class hierarchies,  |
  |                     |   design patterns, and physical file structures.   |
  |                     | • Written from an internal white-box perspective.  |
  +---------------------+----------------------------------------------------+
  ```
### Why Maintaining This Boundary Matters:
  1. **Preserving Implementation Freedom:** If an SRS specifies that a database query must use a specific nested loop algorithm or internal pointer structure, it unnecessarily constrains the developers from using better optimizations during implementation.
  2. **Preventing Premature Optimization:** Focusing on internal coding mechanisms during requirements analysis distracts the team from discovering what the customer actually needs.
  3. **Traceability:** Requirements define the criteria against which quality assurance engineers verify the system. Testers verify whether the system produces the required output for a given input, regardless of internal function implementations.
  
  ---
## 2. The Five User Classes of a Requirements Document
  
  Slide 37 maps the diverse stakeholders who rely on the SRS throughout the software engineering lifecycle. A common mistake is assuming that only programmers read requirements.
  
  ```
                             +------------------------+
                             |   THE SRS READERSHIP   |
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  | SYSTEM CUSTOMERS |            |     MANAGERS     |            | SYSTEM ENGINEERS |
  | • Specify needs  |            | • Plan bids      |            | • Understand the |
  | • Validate scope |            | • Track schedule |            |   system build   |
  | • Approve changes|            | • Allocate staff |            | • Design arch    |
  +------------------+            +------------------+            +------------------+
                                         |
                         +---------------+---------------+
                         |                               |
                         v                               v
              +--------------------+           +--------------------+
              |   TEST ENGINEERS   |           |    MAINTENANCE     |
              | • Write test plans |           |     ENGINEERS      |
              | • Validation tests |           | • Understand parts |
              | • Acceptance tests |           | • Manage evolution |
              +--------------------+           +--------------------+
  ```
### Readership Responsibilities and Usage (Slide 37)
  1. **System Customers:** Read the requirements to verify that the specifications accurately capture their business needs. They use the document to approve or reject proposed changes to the system scope.
  2. **Project Managers:** Use the document to estimate project costs, plan contractual bids, allocate development staff, and schedule milestone deliverables.
  3. **System Engineers (Architects and Developers):** Use the document to understand what software must be constructed and to design the architectural components that satisfy the specified behaviors.
  4. **System Test Engineers (QA Leads):** Use the functional and non-functional requirements as a direct baseline to develop verification and validation test suites, boundary condition tests, and acceptance checklists.
  5. **System Maintenance Engineers:** Consult the document years after initial deployment to understand the original intent of the system, trace dependencies between modules, and ensure new modifications do not violate baseline assumptions.
  
  ---
## 3. Requirements Document Variability: Plan-Driven vs. Agile
  
  Slide 38 highlights that the size, depth, and formality of an SRS vary significantly based on project context:
  
  ```
  +--------------------------------------------------------------------------+
  |                  REQUIREMENTS DOCUMENT VARIABILITY MATRIX                |
  +---------------------+----------------------------------------------------+
  | Factor              | Impact on Documentation                            |
  +---------------------+----------------------------------------------------+
  | Development Method  | • Plan-Driven (Waterfall): Comprehensive, upfront  |
  |                     |   exhaustive document serving as a legal contract. |
  |                     | • Incremental (Agile): Minimal viable document;    |
  |                     |   detailed requirements evolve within user stories |
  |                     |   and sprint backlogs.                             |
  +---------------------+----------------------------------------------------+
  | System Criticality  | • Safety-Critical / Regulated: Highly formal,      |
  |                     |   mathematically verifiable, and audit-ready.      |
  |                     | • Consumer Applications: Focuses on UI workflows,   |
  |                     |   user stories, and rapid interactive mockups.     |
  +---------------------+----------------------------------------------------+
  | Contracting Model   | • External Bidding: Detailed specification required|
  |                     |   to legally bind the contractor to deliverables.  |
  |                     | • In-House Internal Tool: Shorter specification    |
  |                     |   with continuous customer communication.          |
  +---------------------+----------------------------------------------------+
  ```
  
  ---
## 4. The 10-Chapter Standard Structure of a Requirements Document
  
  Slides 39 and 40 outline the formal organization of a comprehensive Software Requirements Specification based on the IEEE standard (IEEE 830 / ISO/IEC/IEEE 29148). This is the exact architectural template used for large-scale systems engineering:
  
  ```
  +--------------------------------------------------------------------------+
  |                 IEEE STANDARD SRS TABLE OF CONTENTS (SLIDES 39–40)       |
  +-------------------+------------------------------------------------------+
  | Chapter           | Engineering Contents and Purpose                     |
  +-------------------+------------------------------------------------------+
  | 1. Preface        | Defines the expected readership of the document.     |
  |                   | Documents version history, change rationales, and a  |
  |                   | summary of modifications across each version.        |
  +-------------------+------------------------------------------------------+
  | 2. Introduction   | Describes the business need for the system. Outlines |
  |                   | the core functions and explains how the product      |
  |                   | interoperates with other systems. Explains how the   |
  |                   | system aligns with organizational strategic goals.   |
  +-------------------+------------------------------------------------------+
  | 3. Glossary       | Unambiguously defines all technical terms, clinical  |
  |                   | or domain-specific jargon, and acronyms. Assumes no  |
  |                   | prior expertise from the reader.                     |
  +-------------------+------------------------------------------------------+
  | 4. User           | High-level description of user services and          |
  |    Requirements   | non-functional constraints using natural language,   |
  |    Definition     | diagrams, and accessible notations. Specifies the    |
  |                   | product and process standards that must be met.      |
  +-------------------+------------------------------------------------------+
  | 5. System         | High-level structural overview showing how functions |
  |    Architecture   | are distributed across system modules. Explicitly    |
  |                   | highlights reusable and off-the-shelf components.    |
  +-------------------+------------------------------------------------------+
  | 6. System         | Detailed, testable specifications of all functional  |
  |    Requirements   | and non-functional requirements. Defines interface   |
  |    Specification  | contracts with external hardware and software.       |
  +-------------------+------------------------------------------------------+
  | 7. System         | Graphical models illustrating relationships between  |
  |    Models         | system components and the external environment       |
  |                   | (e.g., object models, data flows, state machines).   |
  +-------------------+------------------------------------------------------+
  | 8. System         | Documents the fundamental assumptions of the system  |
  |    Evolution      | and anticipated changes due to hardware evolution,   |
  |                   | new regulations, or changing user needs. Prevents    |
  |                   | architects from making premature dead-end decisions. |
  +-------------------+------------------------------------------------------+
  | 9. Appendices     | Detailed technical data: minimal and optimal target  |
  |                   | hardware configurations, database schemas, and data  |
  |                   | dictionaries.                                        |
  +-------------------+------------------------------------------------------+
  | 10. Index         | Navigational indices: alphabetical index, diagram    |
  |                   | index, function table index, and requirements ID map.|
  +-------------------+------------------------------------------------------+
  ```
  
  ---
## 5. Deep Dive: Key Chapters for Capstone Projects
  
  While enterprise systems require all ten chapters, academic capstone projects focus heavily on five critical sections:
### Chapter 3: The Glossary
  The glossary is essential for preventing miscommunication between technical teams and non-technical evaluators.
  * *Rule:* Never assume the reader knows what your acronyms mean.
  * *Example (from your project):* Define terms like **WebRTC**, **STUN/TURN**, **ViGEmBus**, **XInput**, and **CGNAT** early so readers understand technical constraints in later chapters.
### Chapter 5: System Architecture
  This section provides a structural bridge between requirements and design.
  * Must highlight **reused components** (e.g., leveraging open-source virtual gamepad drivers or third-party STUN relays) so stakeholders understand where custom code ends and third-party libraries begin.
### Chapter 8: System Evolution
  This chapter documents technical foresight.
  * It explicitly records: *"We assume the host machine runs Windows 10/11 with DirectX 11+ support. If the operating system deprecates legacy virtual bus drivers, input injection must be refactored to use native Windows HID injection APIs."*
  * Identifying assumptions early prevents system designers from creating architectures that cannot adapt to future platform updates.
  
  ---