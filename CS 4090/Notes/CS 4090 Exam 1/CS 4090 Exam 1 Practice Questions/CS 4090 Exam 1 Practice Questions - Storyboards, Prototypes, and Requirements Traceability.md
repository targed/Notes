### **Questions**
#### **Question 1 [Multiple Choice]**
  What is identified in the course study guides as the primary purpose of constructing storyboards during early requirements engineering?
  
  * A. To produce an exhaustive, contractually binding specification of system source code.
  * B. To gain early reactions from users on proposed application concepts and address the "Yes, But" syndrome before full implementation.
  * C. To replace functional and non-functional requirements with visual artwork.
  * D. To verify that database relational tables are normalized to third normal form.
  
  ---
#### **Question 2 [Matching]**
  Match each level of the Storyboarding Continuum to its operational characteristics:
  
  1. Passive Storyboard
  2. Active Storyboard
  3. Interactive Storyboard
  
  * **A.** Operates like an automated movie of a system that has not been produced yet (e.g., timed slide decks, animations, non-interactive visual simulations).
  * **B.** Tells a static story to the user using sketches, screenshots, Post-it notes, or sample reports, accompanied by a verbal walkthrough ("When you do this, this happens").
  * **C.** Allows the user to experience the system realistically by requiring direct user participation (e.g., clickable wireframe mockups, interactive UI walkthroughs).
  
  ---
#### **Question 3 [Multiple Choice]**
  According to the core heuristics of storyboarding presented in the lecture materials, what happens if an engineering team reviews a storyboard with stakeholders and changes nothing?
  
  * A. The storyboard becomes an official contractual baseline.
  * B. The team does not learn anything.
  * C. The system architecture is proven to be completely defect-free.
  * D. The team can skip the prototype and proceed directly to production code.
  
  ---
#### **Question 4 [Multiple Answer]**
  According to the course slides, a complete storyboard must explicitly deal with which **three essential elements** of any user activity? (Select all that apply.)
  
  * [ ] Who the players are (the specific actors and user roles)
  * [ ] Which database indexing algorithm will store the session tokens
  * [ ] What happens to them (the system actions, information presented, and events)
  * [ ] The network latency budget calculated in milliseconds
  * [ ] How it happens (the step-by-step navigation, screen transitions, and physical flow)
  * [ ] The programming language syntax used to implement the UI
  
  ---
#### **Question 5 [Multiple Choice]**
  Which of the following is the best example of a **low-fidelity prototype**?
  
  * A. A fully deployed production application hosted on a public web server.
  * B. A paper sketch or basic wireframe illustrating screen layout and navigation flow.
  * C. A relational database schema containing populated mock data tables.
  * D. An automated end-to-end integration test suite written in Selenium.
  
  ---
#### **Question 6 [True / False]**
  A storyboard must look visually polished, high-resolution, and near-shippable to be useful to stakeholders during requirements elicitation.
  
  * True
  * False
  
  ---
#### **Question 7 [Multiple Choice]**
  A capstone team designs a prototype screen that contains a "Confirm Reservation" button. What question should the team ask first before refining the button's visual design?
  
  * A. Which font color and drop shadow look most attractive on the display?
  * B. Which requirement or user goal does this action support?
  * C. How many CSS animations can be added to the button hover state?
  * D. Can we add three more alternative buttons to fill the screen layout?
  
  ---
#### **Question 8 [Multiple Choice]**
  Which sequence best represents complete requirements traceability from problem definition through software verification?
  
  * A. Code $\rightarrow$ Screen Layout $\rightarrow$ Stakeholder Need $\rightarrow$ Test Plan
  * B. Requirement $\rightarrow$ User Story / Use Case $\rightarrow$ Storyboard / Prototype $\rightarrow$ Design $\rightarrow$ Code $\rightarrow$ Test
  * C. Prototype $\rightarrow$ Code Construction $\rightarrow$ Functional Requirement $\rightarrow$ User Goal
  * D. Test Case $\rightarrow$ Prototype Screen $\rightarrow$ Database Schema $\rightarrow$ User Story
  
  ---
#### **Question 9 [Multiple Choice]**
  Why must software engineering teams explicitly include alternate, failure, and exception paths in their prototypes rather than modeling only the "happy path"?
  
  * A. Because course grading rubrics require every prototype to contain intentional coding errors.
  * B. Because real-world users frequently encounter input validation errors, network drops, cancellation decisions, and resource conflicts.
  * C. Because modeling failure paths eliminates the need to conduct functional testing.
  * D. Because exception paths allow developers to bypass user authentication logic.
  
  ---
#### **Question 10 [Fill-in-the-Blank]**
  Conditions and verifiable statements that define exactly when a user story or functional requirement is considered successfully completed are called ____________________ ____________________.
  
  ---
### **Answers and Explanations**
#### **Question 1**
  * **Answer:** **B**
  * **Explanation:** The primary purpose of storyboarding is to gain early user reactions and address the "Yes, But" syndrome. When non-technical stakeholders see a visual flow, they can identify missing features and workflow flaws before code is written.
#### **Question 2**
  * **Answer:**
  * **1 $\rightarrow$ B** (Passive: static sketches, paper, verbal walkthrough).
  * **2 $\rightarrow$ A** (Active: automated slideshow or animation showing a simulated movie).
  * **3 $\rightarrow$ C** (Interactive: clickable wireframes requiring user participation).
#### **Question 3**
  * **Answer:** **B**
  * **Explanation:** A foundational rule from the storyboarding guide states verbatim: *"If you don't change anything, you don't learn anything."* The explicit objective of reviewing a storyboard is to surface misconceptions and refine requirements.
#### **Question 4**
  * **Answer:**
  * **Who the players are (the specific actors and user roles)**
  * **What happens to them (the system actions, information presented, and events)**
  * **How it happens (the step-by-step navigation, screen transitions, and physical flow)**
  * **Explanation:** Slide 6 of the Storyboarding deck explicitly lists these three elements as the core anchors of any storyboard activity.
#### **Question 5**
  * **Answer:** **B**
  * **Explanation:** A low-fidelity prototype focuses on fast exploration of structure and workflow with limited detail, typically using paper sketches or basic wireframes.
#### **Question 6**
  * **Answer:** **False**
  * **Explanation:** Overly polished storyboards intimidate users, making them assume the software is already completed and causing them to hold back honest criticism. Effective storyboards should be sketchy, easy to modify, and visibly unfinished.
#### **Question 7**
  * **Answer:** **B**
  * **Explanation:** Visual design elements (colors, animations, styling) must always be secondary to requirements. The team must first verify that an interface element traces directly back to a legitimate user goal or functional requirement.
#### **Question 8**
  * **Answer:** **B**
  * **Explanation:** Traceability requires an unbroken chain connecting the high-level requirement to user stories/use cases, visual prototypes, architectural designs, executable code, and verification test cases.
#### **Question 9**
  * **Answer:** **B**
  * **Explanation:** Real software is not used exclusively in ideal conditions. Users enter malformed inputs, networks drop packets, payments decline, and rooms become unavailable. Modeling exception and recovery paths ensures the system architecture handles real-world failure gracefully.
#### **Question 10**
  * **Answer:** **Acceptance Criteria**
  * **Explanation:** Acceptance criteria define the explicit boundary conditions, assertions, and rules that must evaluate to true before a user story or requirement is marked as done.
  
  ---