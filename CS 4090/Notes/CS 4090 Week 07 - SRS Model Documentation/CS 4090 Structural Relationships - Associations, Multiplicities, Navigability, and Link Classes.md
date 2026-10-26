## 1. What is a Relationship?
  
  In an object-oriented architecture, classes do not exist in isolation. A system composed entirely of disconnected classes cannot compute results or manage workflows.
  
  Slide 19 provides the core definition:
  > **A RELATIONSHIP is what a class or an object knows about another class or object.**
  
  Slide 44 notes the linguistic rule for finding relationships in requirements documents:
  * **Nouns** identify candidate **classes** or **attributes**.
  * **Verbs and action phrases** identify **relationships** between classes.
  
  ```
  +--------------------------------------------------------------------------+
  |                 THE THREE PURPOSES OF RELATIONSHIPS (SLIDE 21)           |
  +---------------------+----------------------------------------------------+
  | Purpose             | Architectural Impact                               |
  +---------------------+----------------------------------------------------+
  | 1. Show             | Establishes communication pathways. Indicates that |
  |    Relationships    | instances of Class A can send messages or invoke   |
  |                     | operations on instances of Class B.                |
  +---------------------+----------------------------------------------------+
  | 2. Enforce Data     | Manages lifecycle and referential integrity.       |
  |    Integrity        | Dictates cascading deletes and prevents orphan     |
  |                     | records when entities are modified or deleted.     |
  +---------------------+----------------------------------------------------+
  | 3. Help Produce     | Enables collaborative computation. Allows objects  |
  |    Results          | to query related objects for data needed to        |
  |                     | complete a business transaction.                   |
  +---------------------+----------------------------------------------------+
  ```
  
  ---
### Case Study: Referential Integrity in Enrollment (Slide 21)
  Slide 21 illustrates how structural relationships enforce database and domain integrity rules across a university course enrollment system:
  
  ```
  +---------------------+                +---------------------+
  |  UniversityCourse   |                |  StudentInformation |
  +---------------------+                +---------------------+
  | - courseID : String |                | - studentID : String|
  +---------------------+                +---------------------+
           1                                      1
           |                                      |
           |                                      |
         0..m                                   0..m
  +------------------------------------------------------------+
  |                       StudentInCourse                      |
  +------------------------------------------------------------+
  | - grade : LetterGrade                                      |
  +------------------------------------------------------------+
  ```
### The Three Integrity Rules Enforced by This Model:
  1. **Removing a University Course:** If a `UniversityCourse` is dropped from the catalog, all matching `StudentInCourse` enrollment records must be removed, but the underlying `StudentInformation` records must remain untouched.
  2. **Removing a Student:** If a student withdraws from the university, their `StudentInformation` record and their associated `StudentInCourse` enrollment entries are removed, but the `UniversityCourse` catalog offering remains active.
  3. **Dropping a Course:** If a student drops a single course, only that specific `StudentInCourse` junction record is deleted. Neither the `UniversityCourse` nor the `StudentInformation` is affected.
  
  ---
## 2. Association Basics and Multiplicity Adornments
  
  Slide 23 defines an association:
  > **If two classes in a model need to communicate with each other, there must be a link between them. An association denotes that link.**
  
  An association is drawn as a **solid line** connecting two class boxes.
  
  ---
### Multiplicity (Cardinality) Adornments (Slides 24, 25, 49)
  Multiplicity specifies **how many instances** of one class can be linked to a single instance of another class. Multiplicity adornments are placed at the ends of the association line, adjacent to the target class.
  
  ```
  +--------------------------------------------------------------------------+
  |                      MULTIPLICITY SYNTAX (SLIDE 49)                      |
  +--------------------+-----------------------------------------------------+
  | Notation           | Exact Meaning                                       |
  +--------------------+-----------------------------------------------------+
  | 1                  | Exactly one instance (mandatory, non-null).         |
  +--------------------+-----------------------------------------------------+
  | *  (or 0..*)       | Zero, one, or more instances (unbounded list/set).  |
  +--------------------+-----------------------------------------------------+
  | 0..1               | Zero or one instance (optional / nullable link).    |
  +--------------------+-----------------------------------------------------+
  | 1..*               | At least one instance (mandatory collection).       |
  +--------------------+-----------------------------------------------------+
  | 2..4               | Between 2 and 4 instances, inclusive.               |
  +--------------------+-----------------------------------------------------+
  | 3..*               | 3 or more instances.                                |
  +--------------------+-----------------------------------------------------+
  ```
### Structural Multiplicity Patterns (Slide 50)
#### 1. One-to-One Association
  Each instance of Class A is associated with exactly one instance of Class B.
  * *Example (Slide 50):* Each student must carry exactly one ID card.
  
  ```
  +-----------+                    carries                    +-----------+
  |  Student  | 1 ----------------------------------------- 1 |   IDCard  |
  +-----------+                                               +-----------+
  ```
#### 2. One-to-Many Association
  A single instance of Class A manages a collection of instances of Class B.
  * *Example (Slide 50):* One drawing list contains many rectangles.
  
  ```
  +---------------+                   contains                +-----------+
  | RectangleList | 1 -------------------------------------- *| Rectangle |
  +---------------+                                           +-----------+
  ```
#### 3. Many-to-Many Association
  Instances on both sides can relate to multiple instances on the opposite side.
  * *Example (Slides 24–25):* A student has one or more instructors, and every instructor teaches one or more students.
  
  ```
  +-----------+                                               +------------+
  |  Student  | 1..* ----------------------------------- 1..* | Instructor |
  +-----------+                                               +------------+
  ```
  
  ---
## 3. Association Adornments: Names, Roles, and Dual Links
  
  UML allows engineers to decorate association lines with additional semantic metadata to remove ambiguity.
  
  ```
  +--------------------------------------------------------------------------+
  |                  NAMED VS. ROLED ASSOCIATIONS (SLIDES 26–27)             |
  +--------------------------------------------------------------------------+
  | Named Association (Slide 27):                                            |
  |   Labels the relationship itself with a descriptive noun/verb.           |
  |                                                                          |
  |   +-----------+                    membership               +------+     |
  |   |  Student  | 1..* --------------------------------- 1..* | Team |     |
  |   +-----------+                                             +------+     |
  |                                                                          |
  | Role Names (Slide 26):                                                   |
  |   Specifies the operational context/behavior of an object on that end.   |
  |                                                                          |
  |   +-----------+ teaches                     learns from +------------+   |
  |   |  Student  | 1..* ----------------------------- 1..* | Instructor |   |
  |   +-----------+                                         +------------+   |
  +--------------------------------------------------------------------------+
  ```
### Dual Associations Between Identical Classes (Slide 28)
  Slide 28 shows that two classes can be connected by **multiple independent association lines** if instances interact in different semantic contexts:
  
  ```
                +-------------------------------------------+
                | member of                                 |
  +-----------+   | 1..*                                 1..* |   +------+
  |  Student  |---+                                           +---| Team |
  +-----------+   | 1                                    1..* |   +------+
                | president of                              |
                +-------------------------------------------+
  ```
  
  * **The First Association (`member of`):** A many-to-many relationship (`1..*` to `1..*`). A student can be a member of multiple teams, and a team has multiple student members.
  * **The Second Association (`president of`):** A constrained relationship (`1` to `1..*`). A team must have exactly one president (`1`), but an ambitious student could potentially serve as president for multiple different teams (`1..*`).
  
  ---
## 4. Navigability and Asymmetric Knowledge
  
  Slide 29 introduces **navigability**, which constrains the direction of message passing across an association.
  
  * **Bidirectional Association (Plain Line):** Both classes store references to each other. Class A can invoke methods on Class B, and Class B can invoke methods on Class A.
  * **Unidirectional Association (Open Arrowhead `->`):** Message passing is strictly one-way.
  
  ```
  +-----------+                                       +-------------------+
  |  Router   | ------------------------------------> | DomainNameServer  |
  +-----------+                                       +-------------------+
  ```
### Architectural Analysis of Slide 29:
  * The `Router` requests address resolution services from the `DomainNameServer` by invoking its lookup operations.
  * The open arrowhead pointing to `DomainNameServer` indicates that the `Router` stores a reference to the DNS server.
  * **The Server Has No Knowledge of the Router:** The `DomainNameServer` does not store a reference to the router object and cannot initiate calls back to the router.
  * **Why Navigability Matters:** Restricting navigability enforces **loose coupling**. The DNS server component remains completely decoupled from client implementations, meaning it can be reused across entirely different networking subsystems.
  
  ---
## 5. Association Classes (Link Classes)
  
  Slide 30 addresses a common design dilemma: *What happens when an attribute belongs to the relationship itself rather than either of the participating classes?*
  
  Consider a customer purchasing a `Product` and registering a `Warranty`.
  * Where does `warrantyCode` live?
  * It cannot live in `Product`, because the same product model is sold to thousands of people with different warranty codes.
  * It cannot live in the generic `Warranty` policy class, because a warranty policy (e.g., "Gold 2-Year Plan") covers many products.
  * **The Solution:** The data belongs to the **act of registration** linking a product to a warranty.
  
  Slide 30 models this as an **Association Class (Link Class)**:
  
  ```
  +-----------+                                               +-----------+
  |  Product  | --------------------------------------------- |  Warranty |
  +-----------+                       |                       +-----------+
                                    | (Dashed Line)
                                    v
                         +---------------------+
                         |    Registration     |
                         +---------------------+
                         | - modelNumber : Id  |
                         | - serialNumber : Id |
                         | - warrantyCode : Id |
                         +---------------------+
  ```
  
  An association class is rendered as a standard class box connected to the association line via a **dashed line**. It has attributes, operations, and state, but its lifecycle exists only as long as the underlying link between `Product` and `Warranty` exists.
  
  ---
## 6. Self-Associations (Reflexive Associations)
  
  Slide 31 illustrates that an association line can loop back to connect a class to itself. This is called a **self-association** or **reflexive association**.
  
  Self-associations are used to model:
  * Recursive data structures (linked lists, trees, graphs).
  * Organizational hierarchies (e.g., an `Employee` manages other `Employees`).
  * Prerequisite chains (e.g., a `Course` requires another `Course` as a prerequisite).
  
  ```
                      +-------------------+
                      |                   | next
                      v                   |
             +-----------------+          |
             | LinkedListNode  |----------+
             +-----------------+          |
                      ^                   |
                      |                   | previous
                      +-------------------+
  ```
  
  In Slide 31's `LinkedListNode` example:
  * The node links back to instances of its own class using two distinct role names: **`next`** and **`previous`**.
  * At runtime, a single node instance stores pointers or references to its neighbor nodes, creating a doubly-linked chain in memory.
  
  ---