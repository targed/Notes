### **Questions**
#### **Question 1 [Multiple Choice]**
  What is the primary purpose of constructing a UML State Model?
  
  * A. To model the static relationships, attributes, and operations across all classes in the system.
  * B. To describe the dynamic behavior and lifecycle of an object as it transitions from one condition to another over time in response to events.
  * C. To establish database schema normalization and define foreign key constraints.
  * D. To assign development tasks and burndown velocity metrics to software engineers.
  
  ---
#### **Question 2 [Multiple Choice]**
  Which of the following represents the **best** name for a state in a UML state machine diagram?
  
  * A. Click Submit
  * B. Enter Password
  * C. Approved
  * D. Process Transaction
  
  ---
#### **Question 3 [Multiple Choice]**
  Given the transition label:  
  $$\text{submit } [\text{allFieldsValid}] \text{ / sendConfirmation()}$$  
  Which statement correctly identifies the individual components?
  
  * A. `submit` is the state; `allFieldsValid` is the event; `sendConfirmation()` is the guard.
  * B. `submit` is the event; `allFieldsValid` is the guard; `sendConfirmation()` is the action/effect.
  * C. `submit` is the action; `allFieldsValid` is the guard; `sendConfirmation()` is the event.
  * D. `submit` is the guard; `allFieldsValid` is the action; `sendConfirmation()` is the state.
  
  ---
#### **Question 4 [True / False]**
  Every transition in a UML state diagram must always include both an explicit guard condition and an action to be syntactically valid.
  
  * True
  * False
  
  ---
#### **Question 5 [Multiple Choice]**
  In UML state machine notation, how are the initial pseudostate and the final state graphically represented?
  
  * A. Initial: Bull's-eye symbol; Final: Solid black filled circle.
  * B. Initial: Clear white diamond; Final: Shaded rectangle.
  * C. Initial: Solid black filled circle; Final: Bull's-eye symbol (solid circle enclosed by a ring).
  * D. Initial: Rounded rectangle; Final: Dashed circle.
  
  ---
#### **Question 6 [Multiple Choice]**
  What is the primary architectural justification for introducing a **composite state** (such as `Processing Order` containing `Validate`, `Charge`, and `Reserve Stock`) into a state diagram?
  
  * A. To convert an object-oriented class model directly into a relational database schema.
  * B. To group a meaningful sub-lifecycle together, reducing diagram clutter and enabling shared outgoing transitions (e.g., a global cancel event).
  * C. To force all internal sub-states to execute strictly in parallel on multiple threads.
  * D. To eliminate the need for trigger events between states.
  
  ---
#### **Question 7 [Multiple Choice]**
  What situation is modeled when a composite state is divided by a horizontal dashed line into **concurrent (orthogonal) regions**?
  
  * A. Multiple alternative states must be evaluated sequentially in an if-else chain.
  * B. Multiple independent aspects or sub-lifecycles of an object evolve at the exact same time.
  * C. Multiple guards must all evaluate to false before an event can fire.
  * D. The object is deleted from memory and recreated under a new identifier.
  
  ---
#### **Question 8 [Multiple Choice / Case Study]**
  In the microwave oven control system case study, what happens if the event `Door open` occurs while the oven is operating inside the `Operation` state?
  
  * A. The oven enters a deadlock state and must be rebooted.
  * B. The system transitions immediately to `Disabled`, automatically stopping the microwave generator and turning on the interior light as a safety interlock.
  * C. The system transitions directly to `Done` and sounds the 5-second completion buzzer.
  * D. The system ignores the event and continues cooking until the countdown timer expires.
  
  ---
#### **Question 9 [Multiple Choice]**
  A support ticket lifecycle is initially modeled as:  
  $$\text{Open} \rightarrow \text{Assigned} \rightarrow \text{InProgress} \rightarrow \text{Resolved} \rightarrow \text{Closed}$$  
  During review, the team adds an alternate transition departing `Resolved` back to `Open` (or `Reopened`) triggered by the event `issueReturns`. What does this addition demonstrate?
  
  * A. It demonstrates that good state models must account for failure, rejection, and recovery paths rather than modeling only the "happy path."
  * B. It demonstrates an invalid transition that violates UML syntax rules.
  * C. It demonstrates that support tickets should be modeled using inheritance rather than states.
  * D. It demonstrates that the ticket was permanently destroyed upon reaching `Resolved`.
  
  ---
#### **Question 10 [Multiple Answer]**
  Which of the following are explicitly identified in the lecture materials as common state modeling mistakes? (Select all that apply.)
  
  * [ ] Using actions or screen names as states instead of persistent conditions
  * [ ] Modeling every minor UI interaction or transient CPU computation
  * [ ] Leaving important transitions unlabeled so the trigger is unknown
  * [ ] Creating states that represent conditions such as `Draft`, `Submitted`, or `Paid`
  * [ ] Forgetting alternate, cancellation, failure, or recovery paths
  * [ ] Confusing events, guards, and actions within the transition label syntax
  * [ ] Modeling the entire multi-class system inside a single state diagram
  
  ---
### **Answers and Explanations**
#### **Question 1**
  * **Answer:** **B**
  * **Explanation:** A UML State Model describes the dynamic, event-driven lifecycle of an object over time. It specifies what condition an object is in, what events trigger changes, what guards restrict transitions, and what actions occur during transitions.
#### **Question 2**
  * **Answer:** **C**
  * **Explanation:** States must represent conditions or statuses, typically named using adjectives or passive participles (`Approved`). `Click Submit`, `Enter Password`, and `Process Transaction` are active verb phrases representing actions or events, not states.
#### **Question 3**
  * **Answer:** **B**
  * **Explanation:** Standard UML transition syntax is $\text{event } [\text{guard}] \text{ / action}$. `submit` is the trigger event; `[allFieldsValid]` is the boolean guard condition enclosed in brackets; `/ sendConfirmation()` is the executable action following the slash.
#### **Question 4**
  * **Answer:** **False**
  * **Explanation:** Slide 33 and Study Guide 1 explicitly state that transitions do not require both a guard and an action. A transition can fire on an event alone (e.g., `approve`). Guards and actions are included only when business rules or side effects demand them.
#### **Question 5**
  * **Answer:** **C**
  * **Explanation:** The initial pseudostate is drawn as a solid black filled circle (indicating where the lifecycle begins). The final state is drawn as a bull's-eye symbol (a solid black circle inside a ring, indicating completion of the modeled lifecycle).
#### **Question 6**
  * **Answer:** **B**
  * **Explanation:** Composite states group related sub-states into a single high-level abstraction. This avoids the state explosion problem and allows a single outgoing transition (such as a global `cancel` or `timeout`) departing the composite boundary to apply automatically to all nested sub-states.
#### **Question 7**
  * **Answer:** **B**
  * **Explanation:** Slide 28 explains that concurrent (orthogonal) regions model scenarios where multiple aspects of an object evolve at the same time (e.g., an order concurrently executing physical warehouse fulfillment in one region while processing credit card settlement in another).
#### **Question 8**
  * **Answer:** **B**
  * **Explanation:** In the microwave oven case study (Chapter 5, Slide 13), the `Door open` transition connects from `Operation` directly to `Disabled`. This acts as an automated safety interlock, ensuring radiation emission halts immediately whenever the physical door opens.
#### **Question 9**
  * **Answer:** **A**
  * **Explanation:** A major pitfall in requirements and state modeling is assuming the "happy path" is the only path. Real systems experience defects, unverified fixes, and reopened tickets. Adding recovery transitions ensures software accommodates real-world edge cases.
#### **Question 10**
  * **Answer:**
  * **Using actions or screen names as states instead of persistent conditions**
  * **Modeling every minor UI interaction or transient CPU computation**
  * **Leaving important transitions unlabeled so the trigger is unknown**
  * **Forgetting alternate, cancellation, failure, or recovery paths**
  * **Confusing events, guards, and actions within the transition label syntax**
  * **Modeling the entire multi-class system inside a single state diagram**
  * **Explanation:** Slide 37 explicitly catalogs these six anti-patterns. Naming states `Draft`, `Submitted`, or `Paid` is correct practice, not a mistake.
  
  ---