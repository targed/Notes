## 1. What is a Use Case?
  
  In traditional software development, requirements were often written as disconnected lists of functional statements (e.g., *"The system shall store dates"* or *"The system shall calculate tax"*). While these statements describe technical capabilities, they fail to explain **who** uses the feature, **why** they need it, or **how** multiple steps connect to achieve a concrete user goal.
  
  Slide 10 provides the formal engineering definition of a use case:
  > **A use case describes a sequence of actions the system performs that yield an observable result of value to a particular actor.**
  
  ```
  +--------------------------------------------------------------------------+
  |                  DECONSTRUCTING THE USE CASE DEFINITION                  |
  +---------------------+----------------------------------------------------+
  | Component           | Engineering Meaning                                |
  +---------------------+----------------------------------------------------+
  | Sequence of actions | A structured dialogue of inputs, processing, and   |
  |                     | outputs; not an isolated button click.             |
  +---------------------+----------------------------------------------------+
  | System performs     | Describes automated system behavior in response to |
  |                     | actor stimuli.                                     |
  +---------------------+----------------------------------------------------+
  | Observable result   | The outcome must be visible and verifiable outside |
  |                     | the system; internal CPU operations do not qualify.|
  +---------------------+----------------------------------------------------+
  | Result of value     | The interaction must achieve a meaningful goal for |
  |                     | the user (e.g., withdrawing cash, booking a room). |
  +---------------------+----------------------------------------------------+
  | Particular actor    | The interaction is anchored to a specific role,    |
  |                     | not an anonymous entity.                           |
  +---------------------+----------------------------------------------------+
  ```
  
  ---
## 2. Defining Actors: Taxonomy and Roles
  
  Slide 10 defines an actor as:
  > **Someone or something that interacts with the system.**
  
  An actor is an external entity that lies outside the boundary of the software system. An actor is not a specific person (e.g., "John Smith"), but an **operational role** that an entity plays when interacting with the system. A single human can play multiple actor roles, and multiple humans can play the same actor role.
  
  ```
                                  +-------------------+
                                  |    ACTOR TYPES    |
                                  +---------+---------+
                                            |
         +----------------------------------+----------------------------------+
         |                                  |                                  |
         v                                  v                                  v
  +------------------+               +------------------+               +------------------+
  |   HUMAN USERS    |               |  OTHER SYSTEMS   |               | HARDWARE DEVICES |
  | • Bank Customer  |               | • Credit Bureau  |               | • Barcode reader |
  | • Store Clerk    |               | • Notification   |               | • Sensor / Timer |
  | • Administrator  |               |   Service (SMS)  |               | • Cash dispenser |
  +------------------+               +------------------+               +------------------+
  ```
### The Three Actor Categories
  1. **Human Users:** People operating the software through user interfaces (e.g., clinic patients, system administrators, bank tellers).
  2. **Other Systems (External Software):** External software packages, third-party cloud services, or legacy databases that exchange data with the application (e.g., an external payment gateway, a university LDAP server, or an automated email notification service).
  3. **Devices (Hardware):** Physical hardware components that trigger events or receive outputs (e.g., automated temperature sensors in a fish farm, cash dispensing mechanisms in an ATM, or automated timers).
### Primary vs. Secondary (Supporting) Actors
  * **Primary Actor:** The entity that initiates the use case to achieve a specific personal or business goal (e.g., the *ATM Customer* who initiates `Withdraw Funds`).
  * **Secondary / Supporting Actor:** An entity that the system interacts with to complete the use case on behalf of the primary actor (e.g., the external *Bank Central System* verifying account funds during a withdrawal).
  
  ---
## 3. The Dual Nature of Use Case Documentation
  
  Slide 3, 10, and 15 emphasize that a use case is **not merely a picture**. Complete use case engineering requires two complementary representations:
  
  ```
  +--------------------------------------------------------------------------+
  |                  THE TWO HALVES OF A USE CASE MODEL                      |
  +--------------------+-----------------------------------------------------+
  | Representation     | Engineering Role & Format                           |
  +--------------------+-----------------------------------------------------+
  | 1. UML Use Case    | The visual map / "Elevator Pitch" (Slide 3).        |
  |    Diagram         | Shows system boundaries, actor associations, and    |
  |                    | high-level dependencies at a glance.                |
  +--------------------+-----------------------------------------------------+
  | 2. Use Case        | The detailed behavioral contract.                   |
  |    Specification   | Textual document or table detailing preconditions,  |
  |    (Template/Table)| step-by-step basic flows, alternative paths, and    |
  |                    | postconditions (Slides 4, 5, 34, 37).               |
  +--------------------+-----------------------------------------------------+
  ```
  
  *Common Student Pitfall:* Submitting only a UML diagram with ellipses and stick figures. The diagram is only the table of contents. The actual functional requirements live inside the textual specifications and tables.
  
  ---
## 4. UML Use Case Diagram Notation and Standard Syntax
  
  Slide 11 outlines the visual rules governing UML use case diagrams:
  
  ```
    Actor
  (Stick-man)                               System Boundary (Box)
     \o/               +------------------------------------------------+
      |      Association    (======================)                    |
     / \ -----------------> (   Name of Use Case   )                    |
  Customer             |    ( (Starts with a Verb) )                    |
                       |    (======================)                    |
                       +------------------------------------------------+
  ```
  
  1. **Actors:** Represented by stick figures with their functional role name written directly underneath.
  2. **Use Cases:** Represented by horizontal ellipses. The name of the use case must be written inside or directly below the oval. It must start with an **action verb** followed by a noun phrase (e.g., `Withdraw Funds`, `Submit Promotion Order`).
  3. **System Boundary:** A rectangular box placed around the use cases, representing the perimeter of the software application. Actors are placed **outside** the box; use cases are placed **inside** the box. The name of the system is placed at the top of the box.
  4. **Association Lines:** Solid lines connecting an actor to a use case. This indicates that the actor participates in or interacts with that specific use case. (Arrows are generally omitted on associations unless navigability or primary initiation must be highlighted).
  
  ---
## 5. Architectural Case Studies: Project-Level Diagrams
  
  The lecture slides present three distinct real-world use case architectures:
  
  ---
### Case Study 1: The Automated Teller Machine (Slide 12)
  This diagram illustrates the division of responsibilities across distinct operational user classes:
  
  ```
  +--------------------------------------------------------------------------+
  |                            ATM SYSTEM BOUNDARY                           |
  +--------------------------------------------------------------------------+
  |                                                                          |
  |       ( Withdraw Funds ) ---\                                            |
  |                  |           \                                           |
  |     Association  |            \  <<include>>                             |
  |                  v             v                                         |
  |    ( Query Account ) --------> ( Validate PIN )                          |
  |                  ^             ^                                         |
  |     Association  |            /  <<include>>                             |
  |                  |           /                                           |
  |       ( Transfer Funds ) ---/                                            |
  |                                                                          |
  |                                             ( Add Cash )                 |
  |                                                  ^                       |
  |                                                  | Association           |
  |                                             ( Startup ) <--- \o/         |
  |                                                  |         Operator      |
  |                                             ( Shutdown )                 |
  +--------------------------------------------------------------------------+
  ```
  
  * **Customer Domain:** The `ATM Customer` interacts strictly with account-servicing use cases (`Withdraw Funds`, `Query Account`, `Transfer Funds`).
  * **Operator Domain:** The `Operator` interacts strictly with system maintenance and cash management use cases (`Add Cash`, `Startup`, `Shutdown`).
  * **Reusability via Include:** All three customer actions mandate PIN authorization before processing, modeled via the `<<include>>` dependency pointing to `Validate PIN`.
  
  ---
### Case Study 2: Physics Facility Control System (Slide 13)
  This diagram illustrates multiple specialized engineering actors operating on a shared scientific instrument:
  
  ```
  +--------------------------------------------------------------------------+
  |                               CONTROL SYSTEM                             |
  +--------------------------------------------------------------------------+
  |                                                                          |
  |  \o/ ------------------------> ( Scan ) --------------------> \o/        |
  |   |  \                                                         |         |
  |   |   \----------------------> ( Set Limits )             Liaison        |
  | Experimental                                             Physicist       |
  |  Physicist \                                                             |
  |             \----------------> ( Take Profile ) <------------/           |
  |                                                                          |
  |                                ( Calibrate ) <---------------- \o/       |
  |                                                                 |        |
  |                                                              Hardware    |
  |                                                             Specialist   |
  +--------------------------------------------------------------------------+
  ```
  
  * **Experimental Physicist:** Drives operational data gathering (`Scan`, `Set Limits`, `Take Profile`).
  * **Liaison Physicist:** Collaborates on data output analysis (`Scan`, `Take Profile`).
  * **Hardware Specialist:** Focuses on underlying instrumentation accuracy (`Calibrate`).
  
  ---
### Case Study 3: Hospital Clinic Management (Slide 14, 22, 26)
  This diagram introduces advanced UML relationships across medical workflows:
  
  ```
  +--------------------------------------------------------------------------+
  |                                CLINIC SYSTEM                             |
  +--------------------------------------------------------------------------+
  |                                                                          |
  |   \o/ --------> ( Cancel Appointment ) <----------------- \o/            |
  | Patient                                                 Scheduler        |
  |    |                                                       |             |
  |    | ---------> ( Make Appointment ) ----------------------/             |
  |    |                    |                                                |
  |    |                    | <<include>>                                    |
  |    |                    v                                                |
  |    |           ( Check Patient Record ) <----------------- \o/           |
  |    |                    ^                                Doctor          |
  |    |                    | <<include>>                      |             |
  |    | ---------> ( Request Medication ) --------------------/             |
  |    |                                                                     |
  |    | ---------> ( Pay Bill ) <====== [Extension Point: more treatment]   |
  |    |                ^    \                                 |             |
  |    |   Generalization     \ <<extend>>                     |             |
  |    |         |             v                               v             |
  |    |  ( Bill Insurance ) ( Defer Payment ) <-------------- \o/           |
  |    |                                                      Clerk          |
  +--------------------------------------------------------------------------+
  ```
  
  * **Include Dependencies:** Both `Make Appointment` and `Request Medication` unconditionally require reading medical history via `Check Patient Record`.
  * **Extend Dependency:** When paying a bill, if the patient requires additional treatment, the normal flow is optionally extended by `Defer Payment`.
  * **Generalization:** `Bill Insurance` is a specialized child use case of the general parent use case `Pay Bill`.
  
  ---
## 6. The Complete Use Case Model
  
  Slides 16 and 17 define the relationship between individual use cases and system architecture:
  
  ```
  +--------------------------------------------------------------------------+
  |                      THE USE CASE MODEL COMPOSITION                      |
  +--------------------------------------------------------------------------+
  | 1. Individual Use Case:                                                  |
  |    • Describes how a particular actor interacts with the system to       |
  |      achieve a single result of value.                                   |
  |                                                                          |
  | 2. Complete Use Case Model:                                              |
  |    • The set of ALL use cases, all actors, and their interactions.       |
  |    • Describes the complete functional behavior of the software system.  |
  +--------------------------------------------------------------------------+
  ```
### Context-First vs. Isolated Use Cases (Slide 17)
  Slide 17 warns against a common development error: **Writing random, disconnected use cases and simply adding them together.**
  * *The Problem:* Writing use cases in isolation produces overlapping requirements, inconsistent actor terminology, and uncoordinated system boundaries.
  * *The Correct Practice:* Build a **context model** of the system first. Establish external actors and operational boundaries, then systematically refine and decompose the system into coherent use cases through iterative development.
  
  ---