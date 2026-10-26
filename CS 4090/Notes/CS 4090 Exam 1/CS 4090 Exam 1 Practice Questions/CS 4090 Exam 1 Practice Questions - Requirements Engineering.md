### **Questions**
#### **Question 1 [Multiple Choice]**
  In the requirements hierarchy diagram bridging the problem domain and the solution domain, which element is associated most directly with the problem domain?
  
  * A. Features
  * B. Software Requirements
  * C. Needs
  * D. Architectural Components
  
  ---
#### **Question 2 [Matching]**
  Match each non-functional requirement statement to its formal classification based on the Sommerville taxonomy tree:
  
  1. "The Mentcare system shall be available to all clinics during normal working hours (Mon–Fri, 08:30–17:30) with downtime not exceeding 5 seconds in any one day."
  2. "Users of the system shall authenticate themselves using their health authority smart identity card."
  3. "The system shall implement patient privacy provisions as set out in national healthcare standard HStan-03-2006-priv."
  
  * **A.** Organizational (Process) Requirement
  * **B.** External (Legislative/Regulatory) Requirement
  * **C.** Product (Dependability/Availability) Requirement
  
  ---
#### **Question 3 [Multiple Choice]**
  According to the readers matrix of requirements specifications, which stakeholder class is identified as a reader of user requirements, but is typically **not** an intended reader of detailed system requirements?
  
  * A. System Architects
  * B. Client Engineers
  * C. Contractor Managers
  * D. System End-Users
  
  ---
#### **Question 4 [Multiple Answer]**
  Which of the following are explicitly identified in the lecture materials as the three classic flaws or structural problems of natural-language requirements? (Select all that apply.)
  
  * [ ] Lack of clarity (difficulty maintaining precision without making the text dense and hard to read)
  * [ ] Premature optimization (specifying compiler registers in requirements)
  * [ ] Requirements confusion (mixing functional features and non-functional quality constraints together)
  * [ ] Excessive formalization (using mathematical set theory that clients reject)
  * [ ] Requirements amalgamation (combining several distinct, independent requirements into a single sentence)
  
  ---
#### **Question 5 [Multiple Choice]**
  Why does the course state that non-functional requirements can often be **more critical** than individual functional requirements?
  
  * A. Non-functional requirements are written in mathematical notation, making them legally binding.
  * B. If a single functional service fails, the system is partially degraded; if an NFR such as performance, reliability, or security fails, the entire system can become completely useless.
  * C. Non-functional requirements must always be implemented before any functional code is constructed.
  * D. Non-functional requirements apply only to database schemas rather than user interfaces.
  
  ---
#### **Question 6 [Multiple Choice]**
  Which of the following requirements is written with the **most testable and verifiable** acceptance criteria?
  
  * A. The system should be fast and responsive under peak load.
  * B. The application search tool shall be intuitive and user-friendly for all student representatives.
  * C. The system shall display course search results within 2.0 seconds for 95% of queries under a simulated load of 500 concurrent users.
  * D. The user database shall be highly secure and reliable against external intrusion.
  
  ---
#### **Question 7 [Multiple Choice]**
  What fundamental lesson does the evolution of the Bradley Fighting Vehicle (*The Pentagon Wars* case study) illustrate from a requirements engineering perspective?
  
  * A. Adding more functional capabilities to an existing design always enhances the vehicle's original mission.
  * B. Changing and conflicting stakeholder demands, if not rigorously managed and prioritized, can cause a system to become compromised, over-engineered, and detached from its core purpose.
  * C. Technical performance metrics are less important than satisfying every general's individual feature request.
  * D. Prototype testing should be avoided until full mass production is underway.
  
  ---
#### **Question 8 [Multiple Answer]**
  According to the course slides, a formal structured scenario (such as the iLearn photo-uploading scenario) must contain which of the following five elements? (Select all that apply.)
  
  * [ ] A description of the starting situation
  * [ ] The complete source code implementation of the upload controller
  * [ ] A description of the normal flow of events
  * [ ] A description of what can go wrong (exception handling)
  * [ ] Information about other concurrent activities in the system
  * [ ] A description of the state when the scenario finishes
  * [ ] The database normalization schema in third normal form
  
  ---
#### **Question 9 [True / False]**
  A functional requirement may explicitly specify actions or behaviors that the system **shall not do**.
  
  * True
  * False
  
  ---
#### **Question 10 [Multiple Choice]**
  Regarding the official Software Requirements Specification (SRS) document, which statement correctly reflects the boundary between requirements and design?
  
  * A. An SRS should describe the internal algorithms, data structures, and class hierarchies in detail.
  * B. An SRS is not a design document; as far as possible, it should state WHAT the system should do rather than HOW it should do it.
  * C. An SRS should only contain database entity-relationship models and UI wireframes.
  * D. An SRS must specify the internal memory management routines used by the operating system.
  
  ---
### **Answers and Explanations**
#### **Question 1**
  * **Answer:** **C**
  * **Explanation:** In the requirements pyramid, **Needs** reside at the top in the **Problem Domain** (the customer's real-world operational space). Features and Software Requirements belong to the **Solution Domain** (the technical implementation space).
#### **Question 2**
  * **Answer:**
  * **1 $\rightarrow$ C** (Product Requirement: specifies operational uptime and max downtime of the delivered software product).
  * **2 $\rightarrow$ A** (Organizational Requirement: derived from health authority IT operational policies).
  * **3 $\rightarrow$ B** (External Requirement: mandated by external healthcare data privacy legislation).
#### **Question 3**
  * **Answer:** **C**
  * **Explanation:** In the readers matrix (Slide 19), Client Managers and **Contractor Managers** read User Requirements to evaluate business scope and high-level deliverables. They do not read low-level System Requirements, which are targeted at developers, architects, and test engineers.
#### **Question 4**
  * **Answer:**
  * **Lack of clarity (difficulty maintaining precision without making the text dense and hard to read)**
  * **Requirements confusion (mixing functional features and non-functional quality constraints together)**
  * **Requirements amalgamation (combining several distinct, independent requirements into a single sentence)**
  * **Explanation:** Slide 17 explicitly defines these three exact flaws of natural language specifications.
#### **Question 5**
  * **Answer:** **B**
  * **Explanation:** If a functional requirement has a defect (e.g., an export button fails), users can often continue using the rest of the application. If a critical NFR fails (e.g., response times take 10 minutes, the database leaks unencrypted records, or the system crashes repeatedly), the entire system is rendered useless.
#### **Question 6**
  * **Answer:** **C**
  * **Explanation:** "Fast," "intuitive," "user-friendly," "secure," and "reliable" are untestable subjective adjectives. Option C defines exact, measurable, and verifiable metrics: response time ($\le 2.0\text{ seconds}$), percentile ($95\%$), and operational conditions ($500\text{ concurrent users}$).
#### **Question 7**
  * **Answer:** **B**
  * **Explanation:** The Bradley Fighting Vehicle evolution demonstrated "feature creep" and conflicting stakeholder demands. By trying to satisfy every stakeholder (troop transport + anti-tank missile platform + heavy armor + firing ports), the vehicle lost its original capability (troop capacity shrank from 11 men to 6) and became an expensive, compromised hybrid.
#### **Question 8**
  * **Answer:**
  * **A description of the starting situation**
  * **A description of the normal flow of events**
  * **A description of what can go wrong (exception handling)**
  * **Information about other concurrent activities in the system**
  * **A description of the state when the scenario finishes**
  * **Explanation:** Slide 19 explicitly lists these five components as mandatory for complete structured scenarios.
#### **Question 9**
  * **Answer:** **True**
  * **Explanation:** Slide 22 explicitly notes that functional requirements define system services, reactions to inputs, behavior in particular situations, and *"may state what the system should not do"* (e.g., "The system shall not permit enrollment if a student has an outstanding academic hold").
#### **Question 10**
  * **Answer:** **B**
  * **Explanation:** Slide 36 establishes that an SRS is **not a design document**. It defines the black-box external requirements (**WHAT** the system shall do) rather than prescribing internal algorithms, data structures, or code mechanics (**HOW** it is implemented).
  
  ---