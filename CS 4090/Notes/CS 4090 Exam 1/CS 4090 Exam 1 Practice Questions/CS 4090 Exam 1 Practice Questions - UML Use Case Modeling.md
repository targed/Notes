### **Questions**
#### **Question 1 [Multiple Choice]**
  Which statement best matches the formal definition of a use case presented in the course materials?
  
  * A. A complete structural diagram showing all database tables and foreign keys.
  * B. A sequence of actions performed by the system that yields an observable result of value to a particular actor.
  * C. A list of internal programming functions executed by the CPU during a transaction.
  * D. A description of user interface styling, color palettes, and button layouts.
  
  ---
#### **Question 2 [Multiple Answer]**
  According to the UML standard, which of the following entities can act as an **actor** in a use case model? (Select all that apply.)
  
  * [ ] A human end-user operating a client terminal
  * [ ] An internal relational database table storing customer records
  * [ ] An external software system (e.g., an automated payment gateway or SMS notification API)
  * [ ] A hardware device or physical sensor (e.g., a barcode reader, cash dispenser, or temperature monitor)
  * [ ] A private helper method defined within a backend class
  
  ---
#### **Question 3 [Multiple Choice]**
  Regarding the initiation of a use case, which cardinality rule is explicitly stated in the lecture slides?
  
  * A. Multiple actors may initiate the same use case simultaneously.
  * B. Only one actor can initiate a use case, although multiple actors can participate or be involved in that use case.
  * C. A use case cannot be initiated by an actor; it must be triggered by an internal database daemon.
  * D. Every use case must have exactly two primary actors initiating execution together.
  
  ---
#### **Question 4 [Multiple Choice]**
  Which of the following represents the **best** name for a use case according to professional UML naming conventions?
  
  * A. Click Schedule Button
  * B. Appointment Table
  * C. Schedule Appointment
  * D. Processed Data Screen
  
  ---
#### **Question 5 [Matching]**
  Match each UML use case relationship to its definition and visual syntax:
  
  1. `<<include>>`
  2. `<<extend>>`
  3. Actor Generalization
  
  * **A.** Represents an inheritance-like relationship between actors; drawn as a solid line with a hollow triangular arrowhead pointing to the more general parent actor.
  * **B.** Represents mandatory, unconditional shared behavior; drawn as a dashed arrow pointing from the base use case toward the reusable included use case.
  * **C.** Represents optional or conditional behavior; drawn as a dashed arrow pointing from the extending use case backward toward the base use case at an extension point.
  
  ---
#### **Question 6 [Multiple Choice / Application Scenario]**
  During an online checkout process, the system **always** calculates the order total and sales tax. Additionally, a customer **may optionally** apply a promotional discount coupon if they have a promo code. How should these two behaviors be modeled in relation to the base use case `Checkout`?
  
  * A. `Checkout` points an `<<extend>>` arrow to `Calculate Total`; `Apply Coupon` points an `<<include>>` arrow to `Checkout`.
  * B. `Checkout` points an `<<include>>` arrow to `Calculate Total`; `Apply Coupon` points an `<<extend>>` arrow to `Checkout`.
  * C. `Checkout` points an `<<include>>` arrow to both `Calculate Total` and `Apply Coupon`.
  * D. `Checkout` generalizes both `Calculate Total` and `Apply Coupon`.
  
  ---
#### **Question 7 [Multiple Choice]**
  In the recycling-machine case study, a customer inserts empty bottles, presses a button, and receives a printed cash receipt. Why does the course treat this entire sequence as **one single use case** rather than three separate use cases?
  
  * A. Because UML tools prohibit drawing more than two use cases on a single diagram.
  * B. Because the three actions form a single dialogue where intermediate steps have no independent value to the customer, while the complete sequence delivers a meaningful result.
  * C. Because actions performed within the same physical machine boundary cannot be split.
  * D. Because the system lacks a relational database to store intermediate transaction logs.
  
  ---
#### **Question 8 [True / False]**
  In a formal UML use case diagram, primary and secondary actors are normally drawn **inside** the rectangular system boundary box.
  
  * True
  * False
  
  ---
#### **Question 9 [Multiple Choice]**
  In an `<<extend>>` relationship between an optional use case (`Defer Payment`) and a base use case (`Pay Bill`), which direction does the dashed arrow point, and why?
  
  * A. From `Pay Bill` to `Defer Payment`, because the base use case calls the extension.
  * B. From `Defer Payment` to `Pay Bill`, because the base use case is complete on its own, and the extending use case hooks back into the base at an extension point.
  * C. It is drawn as a bidirectional solid line with arrows at both ends.
  * D. The arrow direction does not matter as long as the line is labeled with the stereotype.
  
  ---
#### **Question 10 [Multiple Choice]**
  What is a **precondition** in a formal use case specification?
  
  * A. A description of what the system promises to do if the application crashes.
  * B. A list of all database queries executed during the main success scenario.
  * C. A statement of what system conditions and states must evaluate to true before the use case is permitted to begin.
  * D. The final visual screen displayed to the user upon logout.
  
  ---
### **Answers and Explanations**
#### **Question 1**
  * **Answer:** **B**
  * **Explanation:** Slide 10 defines a use case verbatim as: *"A sequence of actions the system performs that yield an observable result of value to a particular actor."* It represents a complete user goal, not an internal algorithm or GUI layout.
#### **Question 2**
  * **Answer:**
  * **A human end-user operating a client terminal**
  * **An external software system (e.g., an automated payment gateway or SMS notification API)**
  * **A hardware device or physical sensor (e.g., a barcode reader, cash dispenser, or temperature monitor)**
  * **Explanation:** Slide 10 and Study Guide 3 define actors as people, external systems, or hardware devices that interact with the system from outside its boundary. Internal classes, database tables, and methods are components *inside* the system, not actors.
#### **Question 3**
  * **Answer:** **B**
  * **Explanation:** Slide 27 explicitly states the initiation rule: *"Only one actor can initiate a use case. However, many actors can be involved in a use case."*
#### **Question 4**
  * **Answer:** **C**
  * **Explanation:** Use case names must start with an active verb followed by a specific noun phrase describing the actor's goal (`Schedule Appointment`). `Click Schedule Button` describes a GUI action, while `Appointment Table` is a noun phrase describing a data structure.
#### **Question 5**
  * **Answer:**
  * **1 $\rightarrow$ B** (`<<include>>`: mandatory, base points dashed arrow to included case).
  * **2 $\rightarrow$ C** (`<<extend>>`: optional, extending case points dashed arrow back to base case).
  * **3 $\rightarrow$ A** (Actor Generalization: inheritance between actors, solid line with hollow triangle).
#### **Question 6**
  * **Answer:** **B**
  * **Explanation:** `Calculate Total` is mandatory and must happen during every transaction; therefore, `Checkout` includes `Calculate Total`. `Apply Coupon` is optional and occurs only under specific conditions; therefore, `Apply Coupon` extends `Checkout`.
#### **Question 7**
  * **Answer:** **B**
  * **Explanation:** Slide 41 resolves the recycling machine paradox by stating that three actions occur, but one without the others is of little value to the customer. A use case must represent a complete Elementary Business Process that leaves data in a consistent state and delivers an observable result of value.
#### **Question 8**
  * **Answer:** **False**
  * **Explanation:** Actors represent entities external to the software; they must always be placed **outside** the system boundary rectangle. Only use case ellipses belong inside the boundary.
#### **Question 9**
  * **Answer:** **B**
  * **Explanation:** In UML, the `<<extend>>` arrow points from the optional extending use case **back to the base use case**. The base use case has no dependencies on the extension and can execute independently; the extension hooks into the base use case at a designated extension point.
#### **Question 10**
  * **Answer:** **C**
  * **Explanation:** A precondition specifies the required system state that must be verified before the use case can start (e.g., "User has an active account and has logged into the portal").
  
  ---