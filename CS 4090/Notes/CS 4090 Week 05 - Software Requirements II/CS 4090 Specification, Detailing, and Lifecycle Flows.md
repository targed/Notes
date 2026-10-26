## 1. Step 4: Outlining the Individual Use Cases
  
  Once the actors and use cases are named and mapped on a high-level diagram, engineers must specify the actual behavioral interaction. A diagram only shows that an actor interacts with a system; it does not explain how the interaction unfolds.
  
  Slide 32 divides the narrative inside a use case into two primary categories:
  
  ```
  +--------------------------------------------------------------------------+
  |                  THE DUAL ANATOMY OF USE CASE FLOWS                      |
  +-------------------+------------------------------------------------------+
  | Flow Type         | Operational Definition                               |
  +-------------------+------------------------------------------------------+
  | 1. Basic Flow     | The most common path from start to finish through    |
  |    (Happy Path /  | the system. Assumes all inputs are valid, external   |
  |    Main Success)  | services respond instantly, and no errors occur.     |
  +-------------------+------------------------------------------------------+
  | 2. Alternative    | Other possible paths through the system triggered by |
  |    Flows          | regular variations, optional user choices, business  |
  |    (Extensions)   | rule failures, or hardware/network exceptions.       |
  +-------------------+------------------------------------------------------+
  ```
  
  ---
### The Diagnostic Elicitation Questions (Slide 33)
  To extract both paths from stakeholders without missing edge cases, engineers ask two sets of targeted questions:
  
  ```
  +--------------------------------------------------------------------------+
  |           DIAGNOSTIC QUESTIONS FOR OUTLINING FLOWS (SLIDE 33)            |
  +--------------------------------------------------------------------------+
  | Basic Flow Questions:                                                    |
  | • What event starts the use case? (The initial trigger).                 |
  | • How does the use case end? (The termination point and final state).    |
  | • How does the use case repeat some behavior? (Loops, pagination,        |
  |   repeated item entry).                                                  |
  |                                                                          |
  | Alternative Flow Questions:                                              |
  | • Are there optional situations in the use case? (User elects to pay     |
  |   later, skips a step, requests special delivery).                       |
  | • What odd cases or variants might happen? (Card expired, international  |
  |   phone format, session timeout).                                        |
  | • What may go wrong? (Network disconnect, insufficient funds, database   |
  |   lock, printer out of paper).                                           |
  +--------------------------------------------------------------------------+
  ```
  
  ---
## 2. Step 5: Refining the Use Cases
  
  Slide 36 addresses the question: *When is a use case ready for development and testing?* 
  
  Refinement is complete when the specification captures all operational boundaries and guarantees:
  1. **Preconditions are Explicit:** The exact system state that must be verified before the use case is permitted to execute.
  2. **All Alternative and Exception Flows are Resolved:** Every point of failure has a documented recovery path or graceful termination step.
  3. **Postconditions (Guarantees) are Defined:** The exact state of the system after the flow concludes, both on success and on failure.
  
  ```
                  +----------------------------------+
                  |         PRECONDITIONS            |
                  | • Required system state to begin |
                  +----------------+-----------------+
                                   |
                                   v
             +---------------------------------------------+
             |         EXECUTION OF THE DIALOGUE           |
             |                                             |
             |  [ Basic Flow: Steps 1 -> 2 -> 3 -> 4 ]     |
             |       |               |                     |
             |       +-> Alt Flow 2a +-> Alt Flow 3a       |
             +---------------------+-----------------------+
                                   |
         +-------------------------+-------------------------+
         |                                                   |
         v (If Success)                                      v (If Failure)
  +----------------------------------+       +----------------------------------+
  |        SUCCESS GUARANTEE         |       |        MINIMAL GUARANTEE         |
  | • Database state persisted       |       | • Rollback transaction           |
  | • Business value delivered       |       | • Audit log recorded             |
  | • Output delivered to actor      |       | • Safe neutral state restored    |
  +----------------------------------+       +----------------------------------+
  ```
### Preconditions vs. System Checks
  A common beginner error is confusing a precondition with a basic flow validation step:
  * *Precondition (System State Baseline):* Must be true **before the actor even touches the system**. The use case assumes this condition is already met.
  * *Example:* "User possesses an active account and has logged into the portal."
  * *Validation Step (Inside Flow):* A check that the system performs during the interaction.
  * *Example:* "Step 3: The system verifies that the entered transfer amount does not exceed the daily limit."
### Postconditions: Success vs. Minimal Guarantees (Slide 34)
  Formal engineering specifications (such as Alistair Cockburn's format) distinguish between two kinds of postconditions:
  * **Success Guarantee:** What state the system must be in if the use case completes the Main Success Scenario (e.g., "The payment is processed, inventory count is decremented by 1, and an email receipt is dispatched").
  * **Minimal Guarantee:** What the system promises to do even if the use case fails, crashes, or is aborted midway through execution (e.g., "Any partial financial transaction is rolled back, no funds are deducted, and the failure reason is written to the security audit log").
  
  ---
## 3. Specification Formats and Templates
  
  The slides present three standard formats used to document use case specifications in industry and academic capstone documents:
  
  ---
### Format 1: The Formal Cockburn Template (Slide 34 Case Study)
  This format is used for mission-critical and financial software where stakeholder interests and recovery guarantees must be rigorously specified.
  
  ```
  +--------------------------------------------------------------------------+
  | FORMAL USE CASE SPECIFICATION: BUY STOCKS OVER THE WEB (SLIDE 34)        |
  +--------------------------------------------------------------------------+
  | Use Case: 1. Buy Stocks over the Web                                     |
  | Primary Actor: Purchaser                                                 |
  | Scope: Personal Advisors / Finance package (PAF)                         |
  | Level: User goal                                                         |
  |                                                                          |
  | Stakeholders and Interests:                                              |
  | • Purchaser: Wants to buy stocks and get them added to the PAF portfolio |
  |   automatically.                                                         |
  | • Stock Agency: Wants full purchase, authorization, and billing info.    |
  |                                                                          |
  | Precondition: User already has the PAF application open and authenticated.|
  |                                                                          |
  | Minimal Guarantee: Sufficient logging information will exist so that PAF |
  | can detect that something went wrong and ask the user to provide details.|
  |                                                                          |
  | Success Guarantee: Remote website has acknowledged the purchase; the     |
  | transaction logs and the user's local portfolio are updated.             |
  |                                                                          |
  | Main Success Scenario (Basic Flow):                                      |
  | 1. Purchaser selects option to buy stocks over the web.                  |
  | 2. PAF gets the name of the web broker site (E*Trade, Schwab) from user. |
  | 3. PAF opens web connection to the broker site, retaining UI control.    |
  | 4. Purchaser browses and executes stock purchase from the web broker.    |
  | 5. PAF intercepts responses from the web site and updates portfolio.     |
  | 6. PAF displays the updated portfolio standing to the user.              |
  |                                                                          |
  | Extensions (Alternative Flows):                                          |
  | 2a. Purchaser selects a web broker that PAF does not support:            |
  |     2a1. System prompts user with supported options or allows cancel.    |
  | 3a. Web failure of any sort during network connection setup:             |
  |     3a1. System reports failure to purchaser with troubleshooting advice.|
  |     3a2. Purchaser either aborts use case or retries connection.         |
  | 4a. Computer crashes or power is lost during purchase transaction:       |
  |     4a1. On reboot, PAF reads recovery log to detect incomplete buy.     |
  | 4b. Web broker site does not acknowledge purchase, puts order on delay:  |
  |     4b1. PAF logs delay, sets polling timer to query broker status later.|
  | 5a. Web broker does not return necessary trade details:                  |
  |     5a1. PAF logs missing data, flags trade as "Pending Verification."   |
  +--------------------------------------------------------------------------+
  ```
### The Extension Numbering Convention:
  Notice the formal numbering standard used in Slide 34:
  * If step `2` branches, the alternative flow is labeled **`2a`**.
  * The specific sub-steps of that alternative branch are labeled **`2a1`**, **`2a2`**, etc.
  * Once the extension finishes, it either rejoins the main flow or terminates the use case.
  
  ---
### Format 2: The Structured Use Case Table (Slide 35 Case Study)
  This format is widely used in business systems and capstone Software Requirements Specifications (SRS) for its clean readability.
  
  ```
  +--------------------------------------------------------------------------+
  |                  USE CASE TABLE: SUBMIT PROMOTION ORDER (SLIDE 35)       |
  +-------------------+------------------------------------------------------+
  | USE CASE NAME     | Submit Promotion Order                               |
  +-------------------+------------------------------------------------------+
  | ACTOR             | Club Member                                          |
  +-------------------+------------------------------------------------------+
  | DESCRIPTION       | Describes the process when a club member submits a   |
  |                   | promotional order to indicate products desired or    |
  |                   | decline ordering during the promotional campaign.    |
  +-------------------+------------------------------------------------------+
  | NORMAL COURSE     | 1. Initiated when club member submits order online.  |
  | (Basic Flow)      | 2. Member's address is validated against records.    |
  |                   | 3. System verifies that products are selected.       |
  |                   | 4. Member credit status checked with Accounts        |
  |                   |    Receivable to confirm no delinquent balance.      |
  |                   | 5. System validates product catalog numbers.         |
  |                   | 6. Inventory availability checked; ordered items are |
  |                   |    recorded with initial status "open."              |
  |                   | 7. Picking ticket created for all open items.        |
  |                   | 8. Picking ticket routed to warehouse distribution.  |
  +-------------------+------------------------------------------------------+
  | PRECONDITION      | Use case "Send Club Promotion" has been processed.   |
  +-------------------+------------------------------------------------------+
  | POST CONDITION    | Promotion order recorded; picking ticket routed to   |
  |                   | the warehouse queue.                                 |
  +-------------------+------------------------------------------------------+
  | ASSUMPTIONS       | Warehouse network link is operational.               |
  +-------------------+------------------------------------------------------+
  ```
  
  ---
### Format 3: The Standard Capstone Specification Template (Slide 37)
  Slide 37 illustrates the standard, concise template recommended for rapid specification in academic and agile engineering environments:
  
  ```
  +--------------------------------------------------------------------------+
  |               STANDARD USE CASE TEMPLATE: ACCEPT PAYMENT (SLIDE 37)      |
  +-------------------+------------------------------------------------------+
  | Use Case          | Accept Payment                                       |
  +-------------------+------------------------------------------------------+
  | Brief Description | Allows an Employee to accept payment from a          |
  |                   | Customer for a video rental transaction.             |
  +-------------------+------------------------------------------------------+
  | Actors            | Employee (Primary Operator), Customer (Beneficiary). |
  +-------------------+------------------------------------------------------+
  | Preconditions     | Customer expresses readiness to rent; customer holds |
  |                   | a valid membership card; video is marked available.  |
  +-------------------+------------------------------------------------------+
  | Main Flow         | 1. Use case begins when Customer selects payment     |
  |                   |    method (cash, debit, or credit card).             |
  |                   | 2. Employee requests system to display charge total. |
  |                   | 3. System displays calculated rental fees and taxes. |
  |                   | 4. Customer tenders payment; employee enters amount. |
  |                   | 5. System verifies transaction and prints receipt.   |
  +-------------------+------------------------------------------------------+
  | Alternative Flows | 4a. Customer has insufficient cash and card declines:|
  |                   |     4a1. Employee prompts for alternative card.      |
  |                   |     4a2. If none provided, transaction is cancelled  |
  |                   |          and video is returned to stock inventory.   |
  +-------------------+------------------------------------------------------+
  | Postconditions    | If successful, payment is recorded in the store      |
  |                   | database and video status changes to "Rented."       |
  +-------------------+------------------------------------------------------+
  ```
  
  ---
## 4. Case Study: Automated Teller Machine Withdrawal (Slide 38)
  
  Slide 38 reviews the step-by-step construction of an ATM Cash Withdrawal to highlight why **precise postconditions** are mandatory:
  
  ```
  [ Customer ] ---> ( ATM System: Withdraw Cash )
  ```
  
  * **Actor:** ATM Customer.
  * **Preconditions:** ATM has network connection to bank switch; card reader operational; cash dispenser mechanism loaded with currency notes.
  * **Basic Flow (Main Success Path):**
  1. Customer inserts debit card into ATM card slot.
  2. System prompts for and customer enters Personal Identification Number (PIN).
  3. System validates PIN with central banking network.
  4. Customer selects "Withdraw Cash" and inputs requested currency amount.
  5. System validates requested amount against account balance and daily ATM limits.
  6. System debits customer account balance and logs transaction.
  7. Dispenser delivers cash to customer.
  8. System ejects card and prints optional receipt.
  * **Alternative Flows (Exceptions):**
  * *2a. Invalid PIN entered:* System increments failure counter. If counter $< 3$, prompt retry. If counter $= 3$, retain card and lock session.
  * *5a. Requested amount exceeds account balance:* Display error message, offer balance inquiry, prompt for new amount or cancellation.
  * *7a. Cash tray mechanical jam:* Rollback account debit, log hardware fault, present apologetic error, return customer card.
  * **Precise Postcondition (Slide 38):**  
  *Vague Postcondition:* "Customer gets money."  
  *Engineering Postcondition:* "The customer's checking account balance has been decremented by the exact dispensed amount, a unique transaction ID is recorded in the bank's core ledger, the physical cash dispenser tally has been decremented, and the card has been safely ejected."
  
  ---