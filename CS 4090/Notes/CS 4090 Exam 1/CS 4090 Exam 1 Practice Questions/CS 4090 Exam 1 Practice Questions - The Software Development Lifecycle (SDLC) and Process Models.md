### **Questions**
#### **Question 1 [Multiple Choice]**
  Which statement best matches the formal definition of a software process model presented in the course?
  
  * A. A detailed, mandatory execution script that every developer must follow without variation.
  * B. An abstract representation of a development process presented from a particular perspective.
  * C. A mathematical proof verifying that system source code satisfies all non-functional requirements.
  * D. A software tool used to automate the deployment of compiled binaries to cloud environments.
  
  ---
#### **Question 2 [Multiple Choice]**
  Which of the following correctly orders the five sequential phases of the classical Waterfall model?
  
  * A. Requirements Analysis $\rightarrow$ Implementation $\rightarrow$ System Design $\rightarrow$ Integration Testing $\rightarrow$ Maintenance
  * B. System Design $\rightarrow$ Requirements Analysis $\rightarrow$ Implementation $\rightarrow$ Maintenance $\rightarrow$ Testing
  * C. Requirements Analysis and Definition $\rightarrow$ System and Software Design $\rightarrow$ Implementation and Unit Testing $\rightarrow$ Integration and System Testing $\rightarrow$ Operation and Maintenance
  * D. Architectural Modeling $\rightarrow$ Code Construction $\rightarrow$ Unit Testing $\rightarrow$ User Acceptance Testing $\rightarrow$ Deployment
  
  ---
#### **Question 3 [Multiple Choice]**
  What is identified in the course materials as the primary structural drawback of the Waterfall model?
  
  * A. The complete absence of formal verification and validation activities.
  * B. The inability to define system requirements before writing source code.
  * C. The difficulty of accommodating requirements changes once development is underway due to inflexible phase partitioning.
  * D. Excessive and continuous customer involvement during every technical phase.
  
  ---
#### **Question 4 [True / False]**
  Incremental development means repeatedly improving and refining an existing solution across cycles, whereas iterative development means delivering software in discrete functional pieces over time.
  
  * True
  * False
  
  ---
#### **Question 5 [Multiple Choice]**
  Why can management visibility become a problem during rapid incremental development?
  
  * A. System managers are barred from accessing version control repositories.
  * B. Incremental development prevents quality assurance teams from executing automated unit tests.
  * C. If increments are developed and released rapidly, it is not cost-effective to produce detailed, formal documentation for every intermediate version.
  * D. Customers refuse to provide feedback until the entire system is completely implemented.
  
  ---
#### **Question 6 [Fill-in-the-Blank]**
  Continual feature additions in incremental development cause code structure to degrade over time. The systematic process of improving the internal structure of software without altering its external functional behavior is called ____________________.
  
  ---
#### **Question 7 [Multiple Answer]**
  Under which of the following project conditions is a plan-driven Waterfall approach considered an appropriate engineering choice? (Select all that apply.)
  
  * [ ] When system requirements are highly ambiguous, unstable, and expected to evolve rapidly.
  * [ ] When requirements are well-understood, thoroughly documented, and unlikely to change significantly.
  * [ ] When developing safety-critical systems subject to strict external regulatory certifications (e.g., avionics, medical control).
  * [ ] When a large-scale engineering system is subcontracted across multiple independent companies at different geographical sites.
  * [ ] When the customer demands an early working prototype within the first two weeks of development.
  
  ---
#### **Question 8 [Matching]**
  Match each strategic implementation choice with its corresponding engineering profile:
  
  1. Build (Custom Development)
  2. Buy (COTS Acquisition)
  3. Integrate (Component / API-Based)
  
  * **A.** Connects reusable software modules, open-source libraries, or external web services; flexible and fast, but introduces dependency and integration risks.
  * **B.** Creates all software capabilities internally from scratch; provides maximum architectural control, but requires the highest time, labor, and financial cost.
  * **C.** Purchases a pre-existing commercial package; provides immediate availability, but limits workflow customization and forces user compromise.
  
  ---
#### **Question 9 [Multiple Choice]**
  A software team decides to use an external payment gateway API (e.g., Stripe) and an existing authentication library rather than writing those capabilities from scratch. Which statement regarding this decision is correct?
  
  * A. Reusing external components eliminates the need for the team to write test cases for payment or authentication workflows.
  * B. External dependencies reduce custom coding effort, but the team must still conduct integration, security, failure, and compatibility testing.
  * C. Third-party APIs eliminate all security risks because external vendors assume legal liability for application bugs.
  * D. Reusing software components guarantees that user requirements will match the external service without compromise.
  
  ---
#### **Question 10 [Multiple Choice / Application Scenario]**
  A capstone team is building a smart campus booking system. The team faces uncertain user requirements, needs to demonstrate a working proof of concept to stakeholders early in the semester, and plans to use an external SMS gateway for automated reminders. Which development strategy is most appropriate?
  
  * A. A strict sequential Waterfall model to lock down all database schemas upfront.
  * B. An entirely unmanaged Big Bang approach where members code independently until the final week.
  * C. A hybrid iterative and incremental approach, releasing functional slices in stages while refining workflows via stakeholder feedback and managing third-party API integration risks.
  * D. A pure throwaway prototyping cycle that avoids writing production code.
  
  ---
### **Answers and Explanations**
#### **Question 1**
  * **Answer:** **B**
  * **Explanation:** A software process model is formally defined as an abstract representation of a process presented from a particular perspective. It provides a simplified framework to guide development activities.
#### **Question 2**
  * **Answer:** **C**
  * **Explanation:** The classical Waterfall model consists of five distinct, sequential phases: (1) Requirements analysis and definition, (2) System and software design, (3) Implementation and unit testing, (4) Integration and system testing, and (5) Operation and maintenance.
#### **Question 3**
  * **Answer:** **C**
  * **Explanation:** The primary drawback of Waterfall is inflexible phase partitioning. Because each phase must be signed off before the next begins, accommodating changes after design or implementation is underway is difficult and expensive.
#### **Question 4**
  * **Answer:** **False**
  * **Explanation:** The statement inverts the definitions. **Iterative** development means repeated cycles of refinement (improving an existing solution). **Incremental** development means delivering functionality in pieces over time (adding new usable slices).
#### **Question 5**
  * **Answer:** **C**
  * **Explanation:** In rapid incremental cycles, producing exhaustive documentation for every single intermediate build is not cost-effective. Managers who rely on document deliverables struggle to measure progress compared to traditional plan-driven gates.
#### **Question 6**
  * **Answer:** **Refactoring**
  * **Explanation:** Slide 15 explicitly defines refactoring as the process of improving the internal structure of software (reducing technical debt and complexity) without changing its external functional behavior.
#### **Question 7**
  * **Answer:**
  * **When requirements are well-understood, thoroughly documented, and unlikely to change significantly.**
  * **When developing safety-critical systems subject to strict external regulatory certifications (e.g., avionics, medical control).**
  * **When a large-scale engineering system is subcontracted across multiple independent companies at different geographical sites.**
  * **Explanation:** Waterfall is appropriate when requirements are stable, when regulators demand exhaustive upfront specifications, or when coordination across multiple remote contractors requires fixed interface baselines. It is inappropriate for rapidly evolving requirements or quick prototyping.
#### **Question 8**
  * **Answer:**
  * **1 $\rightarrow$ B** (Build: internal creation, maximum control, highest time/cost).
  * **2 $\rightarrow$ C** (Buy: COTS purchase, immediate availability, low customization).
  * **3 $\rightarrow$ A** (Integrate: connecting components/APIs, fast and flexible, introduces dependency risk).
#### **Question 9**
  * **Answer:** **B**
  * **Explanation:** Reusing pre-built components and third-party APIs accelerates development, but it never eliminates testing. Teams remain responsible for verifying integration correctness, handling network drops, checking security, and validating data compatibility.
#### **Question 10**
  * **Answer:** **C**
  * **Explanation:** When requirements are uncertain and early feedback is required, an iterative and incremental model allows the team to deliver small working increments, gather feedback, refine features, and isolate third-party API dependencies cleanly.
  
  ---