## 1. When Should You Build a State Model?
  
  Beginners often make the mistake of creating a state model for every single class in their architecture. This results in massive over-modeling. A `User` class with only `firstName`, `lastName`, and `email` has no meaningful lifecycle and should never have a state model.
  
  Deck 3 (Slide 29) establishes the **applicability threshold**:
  
  ```
  +--------------------------------------------------------------------------+
  |                 WHEN TO USE A STATE MODEL (SLIDE 29)                     |
  +--------------------------------------------------------------------------+
  | BUILD A STATE MODEL WHEN:                                                |
  | 1. The object has a distinct, multi-stage business lifecycle             |
  |    (e.g., Order, Ticket, Account, Request, Session, Document).          |
  |                                                                          |
  | 2. What is allowed to happen next depends strictly on current status     |
  |    (e.g., you cannot refund an order before it has been paid).           |
  |                                                                          |
  | 3. The SAME event produces DIFFERENT results depending on state          |
  |    (e.g., pressing "Power" while Off boots the system; pressing "Power"  |
  |    while Active enters Sleep mode).                                      |
  |                                                                          |
  | 4. There are critical invalid transitions that must be prevented         |
  |    (e.g., preventing a student from enrolling after registration closes).|
  |                                                                          |
  | 5. You need to clarify status-driven business logic before coding.       |
  |                                                                          |
  | DO NOT BUILD A STATE MODEL WHEN:                                         |
  | • The object represents simple, static data structures.                  |
  | • Behavior is simple CRUD (Create, Read, Update, Delete) with no status  |
  |   dependencies.                                                          |
  | • "Do not create state models merely because UML offers them" (Slide 29).|
  +--------------------------------------------------------------------------+
  ```
### State Diagrams vs. Activity Diagrams (Deck 1, Question 19)
  A common exam and interview question asks when to choose a State Diagram versus an Activity Diagram:
  * **State Diagram:** Focuses on the **lifecycle of a single object or entity** as it changes condition over time in response to events (e.g., the lifecycle of an `Order`).
  * **Activity Diagram:** Focuses on the **procedural workflow and sequence of activities** across multiple components or business actors in a process (e.g., the checkout process involving the Customer, Warehouse, and Shipping Carrier).
  
  ---
## 2. Extracting States and Events from Requirements Text
  
  Slide 30 details the analytical method used to translate natural language requirements into formal states and transitions:
  
  ```
  +--------------------------------------------------------------------------+
  |          FROM REQUIREMENTS TO STATE MODELS: THE 3-STEP METHOD            |
  +--------------------------------------------------------------------------+
  | Step 1: Extract Candidate States                                         |
  |   • Scan requirements text for adjectives, past participles, or status   |
  |     nouns that describe an ongoing condition.                            |
  |                                                                          |
  | Step 2: Extract Candidate Events                                         |
  |   • Scan text for active verbs that trigger a change in status.          |
  |                                                                          |
  | Step 3: Challenge the Model with Critical Edge Questions                 |
  |   • What transitions are strictly invalid?                               |
  |   • What guard conditions are required?                                  |
  |   • What happens on failure or cancellation?                             |
  +--------------------------------------------------------------------------+
  ```
  
  ---
## 3. Applied Case Studies in State Engineering
  
  ---
### Case Study 1: The Support Ticket Lifecycle (Slide 30 & Deck 1, Q15)
  * **Requirement:** *"A support ticket can be opened, assigned, resolved, reopened, or closed."*
  
  ```
                 assign                             resolve
  +--------+ -------------> +----------+ ----------------------------> +----------+
  |  Open  |                | Assigned |                               | Resolved |
  +--------+ <------------- +----------+                               +-----+----+
      ^         unassign                                                     |
      |                                                                      |
      |                              reopen                                  |
      +----------------------------------------------------------------------+
                                                                             |
                                                                             | close
                                                                             v
                                                                        +----------+
                                                                        |  Closed  |
                                                                        +----+-----+
                                                                             |
                                                                             v
                                                                           ( X )
  ```
#### Why "Good State Models Include More Than the Happy Path" (Deck 1, Q15):
  * A naive developer only models: `Open` $\rightarrow$ `Assigned` $\rightarrow$ `Resolved` $\rightarrow$ `Closed`.
  * In real-world customer support, a user frequently reports that the issue is still occurring after a technician marks it resolved.
  * **The Engineering Fix:** The model must provide a recovery transition: `Resolved` $\rightarrow$ `Open` (or `Reopened`) triggered by the event `reopen`. Omitting recovery paths creates brittle software that forces users to create duplicate tickets.
  
  ---
### Case Study 2: E-Commerce Order Lifecycle and Cancellation (Slide 31)
  Slide 31 illustrates how state machines make business rules visually enforceable:
  
  ```
        pay              pack             ship              deliver
  ( * ) -------> [ Paid ] ------> [ Packed ] ------> [ Shipped ] ---------> ( X )
  |               |                |
  | cancel        | cancel         | cancel
  v               v                v
  +-------------------------------------+
  |              Cancelled              |
  +------------------+------------------+
                   |
                   v
                 ( X )
  ```
#### Enforcing Business Rules via Topology:
  * Notice where the `cancel` event transitions exist: from the initial state, `Paid`, and `Packed`.
  * Notice where `cancel` transitions **do not exist**: there is no transition from `Shipped` to `Cancelled`.
  * **The Business Rule:** Once a package has physically departed the warehouse with a carrier, the customer cannot cancel the order online; they must wait for delivery and initiate a return. The diagram enforces this rule structurally without needing lines of explanatory text.
  
  ---
### Case Study 3: User Authentication and Brute-Force Lockout (Slide 32)
  Slide 32 models state-driven security behaviors during login:
  
  ```
                     login [validCredentials]
  +------------+ -------------------------------------> +---------------+
  | Logged Out |                                        | Authenticated |
  +-----+------+                                        +-------+-------+
      |                                                       |
      | login [invalidCredentials && attempts < 3]           | sessionTimeout
      | / attempts++                                          | or logout
      v                                                       |
  +----------------+                                            |
  | Authenticating | <==========================================+
  +-----+----------+
      |
      | login [attempts >= 3]
      v
  +----------------+             adminReset [verified]
  |     Locked     | ----------------------------------> [ Logged Out ]
  +----------------+
  ```
#### Critical Edge Questions Answered by this Model:
  * *What happens on failed authentication?* The system loops internally, incrementing an attempt counter.
  * *What prevents brute-force attacks?* When the guard condition `[attempts >= 3]` evaluates to true, the system transitions to `Locked`.
  * *What resets the lock?* The object cannot return to `Logged Out` through user login; it requires an explicit `adminReset` event or a 24-hour expiration guard.
  
  ---
### Case Study 4: Git Pull Request Lifecycle (Slides 33–34)
  Slides 33 and 34 challenge students to model a developer code review pipeline:
  * **States:** `Draft`, `Open`, `Approved`, `Changes Requested`, `Merged`, `Closed`.
  
  ```
                    publish
      ( * ) ---------------------> +-------+ <========================\
        |                          | Open  |                          |
        | createDraft              +---+---+                          |
        v                              |                              | update [commitsAdded]
    +-------+                          +------------------------\     |
    | Draft |                          | requestChanges         |     |
    +---+---+                          v                        |     |
        | publish             +-------------------+             |     |
        +-------------------> | Changes Requested |-------------/     |
                              +-------------------+                   |
                                       |                              |
                                       | approve                      |
                                       v                              |
                                 +-----------+                        |
                                 | Approved  | -----------------------/
                                 +-----+-----+
                                       |
                     +-----------------+-----------------+
                     | merge                             | close
                     v                                   v
             +---------------+                   +---------------+
             |    Merged     |                   |    Closed     |
             +-------+-------+                   +-------+-------+
                     |                                   |
                     v                                   v
                   ( X )                               ( X )
  ```
#### Key Architectural Takeaways:
  1. **Multiple Terminal States:** The lifecycle has two distinct final states representing two completely different outcomes:
   * `Merged`: Success termination. Code is integrated into the mainline.
   * `Closed`: Abandonment termination. Pull request is rejected without merging.
  2. **The Feedback Loop:** If changes are requested, the PR does not die. Pushing new commits (`update [commitsAdded]`) transitions the object back to `Open` for re-review.
  
  ---
## 4. The Seven Common State Modeling Mistakes
  
  Slide 37 catalogs the seven most frequent structural errors found in student design documents:
  
  ```
  +--------------------------------------------------------------------------+
  |                  COMMON STATE MODELING MISTAKES (SLIDE 37)               |
  +---------------------+----------------------------------------------------+
  | Mistake             | Why It Breaks the Architecture                     |
  +---------------------+----------------------------------------------------+
  | 1. Using actions or | A screen (e.g., `PaymentScreen`) is a UI element;  |
  |    screens as states| an action (`calculateTax`) is an operation. States |
  |                     | must represent persistent conditions (`Pending`).  |
  +---------------------+----------------------------------------------------+
  | 2. Modeling every   | Creating states for transient CPU operations       |
  |    tiny detail      | (e.g., `ReadingDatabase`) creates unreadable       |
  |                     | diagrams. Focus only on business-relevant states.  |
  +---------------------+----------------------------------------------------+
  | 3. Ignoring failure | Real systems fail. Forgetting error states,        |
  |    paths            | network disconnects, rejection branches, and       |
  |                     | recovery loops makes software brittle.             |
  +---------------------+----------------------------------------------------+
  | 4. Confusing events,| Placing an event in square brackets or writing an   |
  |    guards, & actions| action as a trigger causes syntax errors and       |
  |                     | incorrect code generation.                         |
  +---------------------+----------------------------------------------------+
  | 5. Leaving          | An arrow without a label fails to explain WHAT     |
  |    transitions      | causes the state change. Every non-automatic       |
  |    unlabeled        | transition must have an identifiable trigger.      |
  +---------------------+----------------------------------------------------+
  | 6. Creating         | A state with no incoming transitions can never be  |
  |    disconnected     | reached; a state with no outgoing transitions (and |
  |    states           | not marked final) is an unintended dead end.       |
  +---------------------+----------------------------------------------------+
  | 7. Modeling whole   | State machines are designed to model ONE object's  |
  |    system in one    | lifecycle. Trying to put five different subsystems |
  |    diagram          | on one state diagram produces unreadable clutter.  |
  +---------------------+----------------------------------------------------+
  ```
  
  *Core Motto (Slide 37):* **A state model should explain behavior, not create more confusion than the code.**
  
  ---
## 5. State Model Review Checklist
  
  Slide 38 provides a formal seven-point verification checklist to audit a state diagram before committing it to a Software Design Document (SDD):
  
  ```
  +--------------------------------------------------------------------------+
  |                  STATE MODEL REVIEW CHECKLIST (SLIDE 38)                 |
  +--------------------------------------------------------------------------+
  | [ ] 1. Meaningful States: Does the modeled object truly have dynamic,     |
  |        status-dependent behavior that warrants a state machine?          |
  |                                                                          |
  | [ ] 2. Proper Naming: Is every state named as a condition or status      |
  |        (e.g., `Approved`, `Active`) rather than an action verb?          |
  |                                                                          |
  | [ ] 3. Clear Triggers: Is every transition triggered by an               |
  |        understandable, unambiguous event?                                |
  |                                                                          |
  | [ ] 4. Justified Guards: Are guard conditions used only when a boolean   |
  |        validation check is strictly necessary to prevent transition?     |
  |                                                                          |
  | [ ] 5. Visible Edge Cases: Are invalid transitions prevented, and are    |
  |        failure/cancellation paths explicitly modeled?                   |
  |                                                                          |
  | [ ] 6. Meaningful Boundaries: Are initial pseudostates and final states   |
  |        used correctly according to lifecycle termination rules?          |
  |                                                                          |
  | [ ] 7. Traceability: Does the model accurately reflect the documented    |
  |        business rules and functional requirements in the SRS?            |
  +--------------------------------------------------------------------------+
  ```
  
  ---