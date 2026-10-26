### **Questions**
#### **Question 1 [Numerical / Fill-in-the-Blank]**
  According to the ISO 9000-3 standard cited in the course materials, how many mandatory components are listed as necessary to assure the quality of the software development process and years of maintenance?
  
  Your Answer: _________
  
  ---
#### **Question 2 [Multiple Choice]**
  Which of the following equations accurately reflects the relationship between programming and software engineering presented in the lecture materials?
  
  * A. Software Engineering = Programming + Hardware Interlocks + System Administration
  * B. Software Engineering = Programming + Design + Testing + Deployment + Maintenance + Documentation + Collaboration
  * C. Software Engineering = Requirements Analysis + Programming + Version Control
  * D. Software Engineering = Programming + Continuous Integration + Customer Contracting
  
  ---
#### **Question 3 [Multiple Choice]**
  Within the Software Engineering Framework triad (People, Process, Technology), which component is considered the most critical determinant of project success, and why?
  
  * A. Technology, because selecting an advanced programming language prevents architectural defects.
  * B. Process, because rigid adherence to documentation eliminates shifting requirements.
  * C. People, because technical tools and structured workflows cannot compensate for poor team communication, lack of domain understanding, or unassigned responsibilities.
  * D. Process, because automated testing tools eliminate the need for human code inspections.
  
  ---
#### **Question 4 [Multiple Answer]**
  Which of the following are explicitly identified in the course slides as core reasons why formal software engineering practices are necessary? (Select all that apply.)
  
  * [ ] Managing the complexity of large software systems through modularity and abstraction
  * [ ] Ensuring quality assurance so software meets user requirements reliably
  * [ ] Eliminating the possibility that software requirements will change during development
  * [ ] Increasing economic efficiency by saving time and reducing downstream defect rework
  * [ ] Supporting software evolution and adaptability across changing operational environments
  * [ ] Guaranteeing that code contains zero latent runtime defects before deployment
  
  ---
#### **Question 5 [Multiple Choice]**
  According to Roger Pressman's definition, software quality is evaluated based on conformance to which three criteria?
  
  * A. Functional requirements, compiler optimization flags, and customer budget limits
  * B. User interface aesthetic guidelines, test execution logs, and project schedule milestones
  * C. Explicitly stated functional and performance requirements, explicitly documented development standards, and implicit characteristics expected of all professionally developed software
  * D. Explicit system requirements, database third-normal-form rules, and customer hardware specifications
  
  ---
#### **Question 6 [Multiple Choice]**
  Which technical and organizational failure was identified as a primary contributor to the Therac-25 radiation therapy disaster?
  
  * A. The operating system kernel crashed due to a lack of virtual memory pagination.
  * B. Physical micro-switch hardware interlocks were removed, and engineers relied entirely on unverified software containing concurrency race conditions.
  * C. Radiation technicians intentionally modified the machine's assembly source code to speed up treatment delivery.
  * D. The system underwent exhaustive clinical testing, but external power fluctuations damaged the electron beam emitter.
  
  ---
#### **Question 7 [True / False]**
  While generative AI tools can support tasks across the software lifecycle, the engineering team remains fully accountable for software correctness, system architecture, security, and user impact.
  
  * True
  * False
  
  ---
#### **Question 8 [Multiple Answer]**
  Which of the following statements correctly represent the steps of the "Practical Human + AI Engineering Loop" presented in the course? (Select all that apply.)
  
  * [ ] Think before prompting.
  * [ ] Trust generated code immediately if it compiles cleanly.
  * [ ] Verify before trusting.
  * [ ] Build before claiming understanding.
  * [ ] Outsource architectural decisions before defining constraints.
  * [ ] Test before calling it complete.
  * [ ] Reflect so the next decision is better.
  
  ---
#### **Question 9 [Matching]**
  Match each Team Charter policy with its primary operational objective:
  
  1. Attendance Expectations
  2. Definition of Done (DoD)
  3. Conflict Resolution Protocol
  4. Version Control Protocol
  
  * **A.** Enforces branch protection rules, requires descriptive branch names, and mandates peer code reviews before merging pull requests.
  * **B.** Provides a structured three-tier escalation model (1-on-1 $\rightarrow$ Team Mediation $\rightarrow$ Faculty Intervention) to address non-performance objectively.
  * **C.** Requires at least 24 hours of advance notice prior to an absence and mandates reviewing recorded meeting minutes within 12 hours.
  * **D.** Establishes a concrete quality checklist (linting, automated tests passing, acceptance criteria met) before any feature ticket is closed.
  
  ---
#### **Question 10 [Multiple Choice]**
  Why does the course emphasize that "some decisions cannot wait for a chat window"?
  
  * A. Because commercial software organizations prohibit developers from using generative AI tools under all circumstances.
  * B. Because large language models are incapable of writing unit test scripts or syntax explanations.
  * C. Because during high-pressure situations such as live production incidents or security events, developers lack the time, connectivity, or context to prompt external tools, requiring prior learning to serve as immediate judgment.
  * D. Because external chat tools introduce excessive network packet latency into local compiler toolchains.
  
  ---
### **Answers and Explanations**
#### **Question 1**
  * **Answer:** **4**
  * **Explanation:** ISO 9000-3 explicitly lists four components required to assure software quality: (1) Computer programs (code), (2) Procedures, (3) Documentation, and (4) Data necessary to operate the system.
#### **Question 2**
  * **Answer:** **B**
  * **Explanation:** The slides define software engineering as more than programming: $\text{Software Engineering} = \text{Programming} + \text{Design} + \text{Testing} + \text{Deployment} + \text{Maintenance} + \text{Documentation} + \text{Collaboration}$.
#### **Question 3**
  * **Answer:** **C**
  * **Explanation:** People are the primary pillar. A team with poor communication, conflicting priorities, or unclear responsibilities will build a failed system regardless of how advanced their technology stack or development process is.
#### **Question 4**
  * **Answer:**
  * **Managing the complexity of large software systems through modularity and abstraction**
  * **Ensuring quality assurance so software meets user requirements reliably**
  * **Increasing economic efficiency by saving time and reducing downstream defect rework**
  * **Supporting software evolution and adaptability across changing operational environments**
  * **Explanation:** Software engineering manages complexity, ensures quality, improves efficiency, and supports evolution. It never eliminates changing requirements, nor does it guarantee zero defects.
#### **Question 5**
  * **Answer:** **C**
  * **Explanation:** Roger Pressman defines software quality verbatim as: *"Conformance to explicitly stated functional and performance requirements, explicitly documented development standards, and implicit characteristics that are expected of all professionally developed software."*
#### **Question 6**
  * **Answer:** **B**
  * **Explanation:** In the Therac-25, mechanical safety interlocks present on the Therac-20 were removed to cut costs. Engineers assumed software checks were infallible, but race conditions in the software allowed the high-power beam to fire without the protective attenuation target in place.
#### **Question 7**
  * **Answer:** **True**
  * **Explanation:** The course emphasizes that "The work still belongs to you." AI can support development stages, but it does not own risk, architecture, integration, or outcomes. The human engineer is accountable for the system.
#### **Question 8**
  * **Answer:**
  * **Think before prompting.**
  * **Verify before trusting.**
  * **Build before claiming understanding.**
  * **Test before calling it complete.**
  * **Reflect so the next decision is better.**
  * **Explanation:** The six-step loop is Think $\rightarrow$ Ask $\rightarrow$ Verify $\rightarrow$ Build $\rightarrow$ Test $\rightarrow$ Reflect. Blindly trusting generated code and outsourcing architecture violate core engineering principles.
#### **Question 9**
  * **Answer:**
  * **1 $\rightarrow$ C** (Attendance: 24-hour notice, async catch-up).
  * **2 $\rightarrow$ D** (Definition of Done: quality checklist before closing tickets).
  * **3 $\rightarrow$ B** (Conflict Resolution: 3-tier escalation model).
  * **4 $\rightarrow$ A** (Version Control: branch protection, PR reviews).
#### **Question 10**
  * **Answer:** **C**
  * **Explanation:** During production incidents, system crashes, or security emergencies, engineers do not have the luxury of time, network connectivity, or perfect prompts. Foundational knowledge must be internal so it translates into immediate operational judgment.
  
  ---