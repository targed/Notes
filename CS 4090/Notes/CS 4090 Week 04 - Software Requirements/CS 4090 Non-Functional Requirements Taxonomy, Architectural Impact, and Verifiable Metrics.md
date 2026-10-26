## 1. What are Non-Functional Requirements?
  
  While functional requirements specify the concrete services, inputs, and outputs of a system, **non-functional requirements (NFRs)** specify the overarching properties, behavioral characteristics, and operational constraints under which those services must execute.
  
  ```
  +--------------------------------------------------------------------------+
  |                  FUNCTIONAL VS. NON-FUNCTIONAL COMPARISON                |
  +---------------------+----------------------------------------------------+
  | Dimension           | Characteristics                                    |
  +---------------------+----------------------------------------------------+
  | Functional          | • Focuses on individual features and user actions. |
  | Requirements        | • Defines: "The system shall do X."                |
  |                     | • Localized: Can often be traced to specific code  |
  |                     |   modules, functions, or UI buttons.               |
  +---------------------+----------------------------------------------------+
  | Non-Functional      | • Focuses on system properties and constraints.    |
  | Requirements (NFRs) | • Defines: "The system shall behave in manner Y."  |
  |                     | • Global: Applies to the system as a whole rather  |
  |                     |   than individual functions.                       |
  +---------------------+----------------------------------------------------+
  ```
### Domain Requirements (Slide 22)
  Slide 22 also introduces **domain requirements**: constraints derived directly from the application's operating environment rather than user requests. Domain requirements can be functional or non-functional:
  * *Example (Banking):* Legal rules governing account reconciliation and transaction auditing.
  * *Example (Aerospace / Rail):* Deceleration formulas and physical braking distances dictating how train control software must slow a vehicle.
  * *Danger:* If domain experts omit these requirements because they assume the context is "obvious," software developers will build mathematically invalid systems.
### Why NFRs Can Be More Critical Than Functional Requirements
  Slide 23 presents an essential software engineering principle:
  > **Non-functional requirements may be more critical than functional requirements. If these are not met, the system may be useless.**
  
  Consider the operational impact of a failure:
  * If a functional requirement fails (e.g., an "Export to CSV" button crashes in an inventory app), the system is impaired, but users can still perform other core tasks like searching or checking stock.
  * If a non-functional requirement fails (e.g., search queries take 15 minutes to return, the database leaks unencrypted patient passwords, or the application crashes every 20 minutes), the entire product is unusable regardless of how many functional features it possesses.
  
  ---
## 2. The Non-Functional Requirements Taxonomy
  
  Slide 24 presents the complete classification tree for non-functional requirements, dividing them into three primary categories: **Product**, **Organizational**, and **External**.
  
  ```
                                  +---------------------------------+
                                  |   NON-FUNCTIONAL REQUIREMENTS   |
                                  +----------------+----------------+
                                                   |
         +-----------------------------------------+-----------------------------------------+
         |                                         |                                         |
         v                                         v                                         v
  +------------------+                      +------------------+                      +------------------+
  |     PRODUCT      |                      |  ORGANIZATIONAL  |                      |     EXTERNAL     |
  |   REQUIREMENTS   |                      |   REQUIREMENTS   |                      |   REQUIREMENTS   |
  +--------+---------+                      +--------+---------+                      +--------+---------+
         |                                         |                                         |
         +--> Usability                            +--> Environmental                        +--> Regulatory
         +--> Efficiency                           +--> Operational                          |    +--> Accounting
         |    +--> Performance                     +--> Development                          |    +--> Safety/Sec
         |    +--> Space                                                                     +--> Ethical
         +--> Dependability                                                                  +--> Legislative
         +--> Security
  ```
  
  ---
### Category 1: Product Requirements
  Requirements specifying that the delivered software product must behave in a particular, measurable way (Slide 26).
  
  1. **Usability Requirements:** The effort required to learn, operate, and interpret system interfaces. Focuses on error prevention, cognitive load, and accessibility standards.
  2. **Efficiency Requirements:** The system's resource consumption:
   * *Performance Requirements:* Execution speed, transaction throughput, response latency, and screen refresh rates.
   * *Space Requirements:* Megabytes of memory consumed at runtime, persistent disk storage limits, and cache allocations.
  3. **Dependability Requirements:** The trustworthiness of the system over time:
   * *Reliability:* Mean Time Between Failures (MTBF) and probability of failure.
   * *Availability:* Percentage of time the system is operational and accessible.
  4. **Security Requirements:** Protection against unauthorized access, integrity of stored data, and cryptographic requirements for network transit.
  
  ---
### Category 2: Organizational (Process) Requirements
  Requirements that arise as a consequence of internal organizational policies, business procedures, and engineering standards (Slide 26).
  
  1. **Environmental Requirements:** The operational and hosting infrastructure where the software must run (e.g., must deploy on university Linux servers, must run inside Docker containers, must support air-gapped intranet networks).
  2. **Operational Requirements:** Procedures defining how system operators and system administrators manage the running software (e.g., backup schedules, operator shift-handover runbooks, monitoring alert thresholds).
  3. **Development Requirements:** Mandated engineering processes, toolchains, and programming environments (e.g., must be written in C++20, must use the GitHub Actions CI pipeline, must adhere to Google Style Guides, must follow Scrum sprint cadences).
  
  ---
### Category 3: External Requirements
  Requirements derived from factors outside the system, the development team, and the acquiring organization (Slide 26).
  
  1. **Regulatory Requirements:** Compliance with formal industry or governmental oversight boards:
   * *Accounting Requirements:* Tax calculation rules, financial ledger immutability, audit trail retention.
   * *Safety / Security Certifications:* FAA airworthiness certification (DO-178C), FDA medical device approvals.
  2. **Legislative Requirements:** Mandates imposed by national or international law:
   * Data privacy laws (e.g., GDPR, HIPAA, FERPA).
   * Digital accessibility laws (e.g., Americans with Disabilities Act / WCAG 2.1 compliance).
  3. **Ethical Requirements:** Software constraints that prevent harm, ensure user transparency, eliminate demographic bias in automated decisions, and guarantee informed consent.
  
  ---
## 3. Case Study: Mentcare Non-Functional Requirements
  
  Slide 27 illustrates each of the three primary classifications using the Mentcare medical record system:
  
  ```
  +--------------------------------------------------------------------------+
  |            MENTCARE NON-FUNCTIONAL REQUIREMENTS CLASSIFICATIONS          |
  +-------------------+------------------------------------------------------+
  | Classification    | Exact Requirement Statement from Slide 27            |
  +-------------------+------------------------------------------------------+
  | Product           | The Mentcare system shall be available to all        |
  | Requirement       | clinics during normal working hours (Mon–Fri,        |
  |                   | 08:30–17:30). Downtime within normal working hours   |
  |                   | shall not exceed five seconds in any one day.        |
  +-------------------+------------------------------------------------------+
  | Organizational    | Users of the Mentcare system shall authenticate      |
  | Requirement       | themselves using their health authority identity     |
  |                   | card.                                                |
  +-------------------+------------------------------------------------------+
  | External          | The system shall implement patient privacy           |
  | Requirement       | provisions as set out in HStan-03-2006-priv.         |
  +-------------------+------------------------------------------------------+
  ```
### Analysis of the Mentcare Requirements:
  * **The Product Requirement** specifies availability in terms of working hours ($9 \text{ hours/day} = 32,400 \text{ seconds}$). Allowing only $5 \text{ seconds}$ of downtime equates to an availability threshold of:
  $$\frac{32,395}{32,400} \approx 99.985\% \text{ availability}$$
  This is a verifiable, testable metric.
  * **The Organizational Requirement** enforces a hardware-based authentication policy established by the health authority's IT department.
  * **The External Requirement** incorporates an external legal privacy standard (`HStan-03-2006-priv`), binding the software to legislative healthcare compliance.
  
  ---
## 4. Architectural Impact of Non-Functional Requirements
  
  Slide 25 highlights two foundational architectural consequences of NFRs:
### 1. NFRs Dictate the Overall Architecture, Not Individual Classes
  Functional requirements can usually be assigned to specific subsystems (e.g., "The system shall compute sales tax" is isolated to a `BillingService` module). 
  
  Non-functional requirements are **cross-cutting concerns** that influence the foundational structural topology of the entire application:
  * *Performance Constraint Example:* If a streaming system requires sub-50ms glass-to-glass latency, engineers cannot build a standard multi-hop client-server architecture with heavy JSON serialization. They must structure the system to use peer-to-peer UDP protocols (such as WebRTC), minimize network hops, and rely on binary packet structures.
  * *Availability Constraint Example:* To meet a $99.99\%$ uptime NFR, the architecture must incorporate redundant hot-standby servers, automatic database replication, and load balancers.
### 2. NFRs Generate Cascades of Functional Requirements
  A single high-level non-functional requirement routinely spawns multiple concrete functional services:
  
  ```
  [ High-Level NFR: "The system shall comply with patient privacy standard HStan-03-2006-priv." ]
                                          |
                                          v (Generates Functional Services)
  +----------------------------------------------------------------------------------------+
  | • FR-SEC-01: The system shall encrypt all patient data at rest using AES-256.          |
  | • FR-SEC-02: The system shall terminate inactive user sessions after 10 minutes.       |
  | • FR-SEC-03: The system shall record an immutable audit log entry for every patient    |
  |              record access, logging User ID, Patient ID, and Timestamp.                |
  | • FR-SEC-04: The system shall mask Social Security Numbers, displaying only the        |
  |              last 4 digits on clinic UI screens.                                       |
  +----------------------------------------------------------------------------------------+
  ```
  
  ---
## 5. Metrics for Specifying Non-Functional Requirements
  
  The most common mistake in student software specifications is writing vague, unprovable quality statements:
  * *Vague Anti-Pattern:* "The application shall be user-friendly, fast, robust, and secure."
  * *Engineering Reality:* These statements cannot be tested. An engineer cannot write a pass/fail unit test or benchmark for "fast."
  
  Slide 28 provides the standard engineering metrics used to convert subjective quality attributes into **measurable, verifiable contract baselines**:
  
  ```
  +--------------------------------------------------------------------------+
  |            METRICS FOR SPECIFYING NON-FUNCTIONAL REQUIREMENTS            |
  +-------------------+------------------------------------------------------+
  | Quality Property  | Measurable Engineering Units                         |
  +-------------------+------------------------------------------------------+
  | Speed             | • Processed transactions per second                  |
  |                   | • User/event response time (milliseconds)            |
  |                   | • Screen refresh rate (Frames Per Second)            |
  +-------------------+------------------------------------------------------+
  | Size              | • Megabytes / Gigabytes of memory consumed           |
  |                   | • Number of ROM/RAM storage chips                    |
  |                   | • Compiled binary distribution size (MB)             |
  +-------------------+------------------------------------------------------+
  | Ease of Use       | • Training time required for a novice user (hours)   |
  |                   | • Number of help documentation frames accessed       |
  |                   | • Task completion time for standard user scenarios   |
  +-------------------+------------------------------------------------------+
  | Reliability       | • Mean Time to Failure (MTTF / MTBF in hours)        |
  |                   | • Probability of system unavailability               |
  |                   | • Rate of failure occurrence (failures per 1,000 hrs)|
  |                   | • Availability percentage (e.g., 99.9%)              |
  +-------------------+------------------------------------------------------+
  | Robustness        | • Time required to restart after failure (seconds)   |
  |                   | • Percentage of unexpected events causing crash      |
  |                   | • Probability of database corruption upon failure    |
  +-------------------+------------------------------------------------------+
  | Portability       | • Percentage of target-dependent statements (LOC)    |
  |                   | • Number of distinct target operating systems        |
  |                   |   supported without codebase branching               |
  +-------------------+------------------------------------------------------+
  ```
### Examples of Measurable vs. Unmeasurable Requirements:
  1. **Speed:**
   * *Bad:* "The course search tool shall load quickly."
   * *Good:* "The system shall return course search results within 2.0 seconds under a simulated load of 500 concurrent users."
  2. **Reliability:**
   * *Bad:* "The controller streaming connection shall be reliable."
   * *Good:* "The WebRTC video pipeline shall maintain a Mean Time Between Disconnections (MTBD) of $\ge 4.0\text{ hours}$ over network connections exhibiting $\le 2\%\text{ packet loss}$."
  3. **Usability:**
   * *Bad:* "The booking interface shall be easy to use."
   * *Good:* "A first-time student representative shall be able to complete a room reservation request within 3.0 minutes without written instructions."
  
  ---