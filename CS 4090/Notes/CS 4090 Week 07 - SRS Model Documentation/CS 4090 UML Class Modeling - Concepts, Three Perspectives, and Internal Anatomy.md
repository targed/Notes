## 1. Defining the Class: Blueprint vs. Runtime Object
  
  Slide 8 provides the formal object-oriented definition of a class:
  > **A CLASS is a template (specification, blueprint) for a collection of objects that share a common set of attributes and operations.**
  
  Class modeling represents **static modeling**. Unlike sequence diagrams or state machines that model transient messages and temporal states, a class model defines the invariant, structural reality of the system: what types exist, what data they hold, and how they associate with other types.
  
  ```
  +--------------------------------------------------------------------------+
  |                       CLASS VS. RUNTIME OBJECT (SLIDE 8)                 |
  +---------------------+----------------------------------------------------+
  | Dimension           | Characteristics                                    |
  +---------------------+----------------------------------------------------+
  | Class (Blueprint)   | • The compile-time specification or data type.     |
  |                     | • Defines attributes (fields) and operations.      |
  |                     | • Example: `HealthClubMember`                      |
  +---------------------+----------------------------------------------------+
  | Object (Instance)   | • A concrete, runtime entity in memory.            |
  |                     | • Holds specific state values for its attributes.  |
  |                     | • Examples: Alice (Member #101), Bob (Member #102) |
  +---------------------+----------------------------------------------------+
  ```
  
  ---
## 2. The Three Modeling Perspectives
  
  Class diagrams are not static across the software lifecycle. Slide 10 establishes that the meaning of a class diagram shifts depending on the **engineering perspective** used at that moment in time:
  
  ```
                             +------------------------+
                             |   CLASS PERSPECTIVES   |
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  |    CONCEPTUAL    |            |  SPECIFICATION   |            |  IMPLEMENTATION  |
  |   PERSPECTIVE    |            |   PERSPECTIVE    |            |   PERSPECTIVE    |
  +------------------+            +------------------+            +------------------+
  | Requirements     |            | Software Design  |            | Production Code  |
  | Domain concepts  |            | Interface focus  |            | Direct blueprint |
  | Language neutral |            | Operation specs  |            | Language bound   |
  +------------------+            +------------------+            +------------------+
  ```
### 1. Conceptual Perspective (Requirements Analysis)
  * **Focus:** Represents real-world concepts in the application domain.
  * **Characteristics:** Drawn with zero regard for programming languages, database structures, or software frameworks. It models domain rules (e.g., *"A Course has Students"*), not pointers or foreign keys.
### 2. Specification Perspective (Software Design)
  * **Focus:** Defines software interfaces and operational boundaries.
  * **Characteristics:** Outlines what operations and data types exist without committing to specific algorithmic implementations. It establishes contracts between components so frontend and backend developers can build in parallel.
### 3. Implementation Perspective (Code Construction)
  * **Focus:** Serves as a direct blueprint for writing source code in a specific language (e.g., Java, C++, Python).
  * **Characteristics:** Shows exact language types (e.g., `std::vector<int>`, `ArrayList<String>`), access modifiers, memory ownership semantics, and design patterns.
### Iterative Evolution Across Sprints (Slide 9)
  Class diagrams evolve iteratively:
  * **Sprint 1 (Early Inception):** Rough sketches containing candidate class names and general associations. Multiplicities, attributes, and method signatures are intentionally omitted to avoid getting bogged down in low-level details.
  * **Sprint 2–3 (Elaboration & Build):** Attributes, visibility indicators, return types, and interface implementations are formalized as architecture stabilizes.
  
  ---
## 3. Class Anatomy: The Three Compartments
  
  Slides 11 and 40 define the visual standard for drawing an individual class box in UML. A class is represented by a rectangle partitioned into **three distinct horizontal compartments**:
  
  ```
  +---------------------------------------------------------+
  |                      Class Name                         |  <-- Top Compartment
  +---------------------------------------------------------+
  | visibility attributeName : Type = defaultValue          |  <-- Middle Compartment
  +---------------------------------------------------------+
  | visibility operationName(param: Type) : returnType      |  <-- Bottom Compartment
  +---------------------------------------------------------+
  ```
  
  ```
  +---------------------------------------------------------+
  |                       Member                            |
  +---------------------------------------------------------+
  | - memberNumber : String                                 |
  | - firstName : String                                    |
  | - lastName : String                                     |
  | - telephone : String                                    |
  | / membershipDuration : int                              |
  +---------------------------------------------------------+
  | + checkOutVideo(videoID : String) : boolean             |
  | + checkInVideo(videoID : String) : boolean              |
  | + buyItem(itemID : String, qty : int) : Invoice         |
  +---------------------------------------------------------+
  ```
  
  ---
### Compartment 1: The Top Compartment (Class Identification)
  * **Class Name:** Centered in bold text using singular nouns in UpperCamelCase (e.g., `Student`, `RentalInvoice`).
  * **Abstract Classes:** The class name must be rendered in **italics**, or marked with the `{abstract}` property tag.
  * **Interfaces:** The stereotype **`<<interface>>`** must be placed directly above the interface name.
  
  ---
### Compartment 2: The Middle Compartment (Attributes)
  Attributes represent the named data properties or instance variables that describe the object (Slide 14).
#### Formal Attribute Syntax (Slide 15 & 41)
  $$\text{[visibility] } \text{attributeName : Type } \text{ [= defaultValue]}$$
  
  ```
  +--------------------------------------------------------------------------+
  |                  UML ATTRIBUTE VISIBILITY MARKERS (SLIDE 16 & 41)        |
  +--------+------------------+----------------------------------------------+
  | Symbol | Visibility       | Operational Scope                            |
  +--------+------------------+----------------------------------------------+
  |   +    | Public           | Accessible by any class in the system.       |
  +--------+------------------+----------------------------------------------+
  |   #    | Protected        | Accessible only within the class and its     |
  |        |                  | subclasses (generalizations).                |
  +--------+------------------+----------------------------------------------+
  |   -    | Private          | Encapsulated; accessible only within the     |
  |        |                  | defining class.                              |
  +--------+------------------+----------------------------------------------+
  |   ~    | Package / Default| Accessible by any class residing in the same |
  |        |                  | package or namespace.                        |
  +--------+------------------+----------------------------------------------+
  |   /    | Derived          | Not stored directly; calculated dynamically  |
  |        |                  | from other attributes at runtime.            |
  +--------+------------------+----------------------------------------------+
  ```
#### Special Attribute Notations:
  * **Derived Attributes (`/`):** Slide 15 highlights that a derived attribute is not physically stored in memory or a database column, but is computed on demand.  
  *Example:* `/ age : Date` (or `/ age : int`), which is calculated on the fly by subtracting `birthdate` from the current system clock.
  * **Static Attributes (Underlined):** Slide 41 specifies that class-level variables (shared across all instances of the class) must be **underlined**.  
  *Example:* `<u>totalStudents : int</u>`.
  
  ---
### Compartment 3: The Bottom Compartment (Operations)
  Operations describe the behavioral methods, services, and responsibilities that the class executes (Slide 17).
#### Formal Operation Syntax (Slide 42)
  $$\text{[visibility] } \text{operationName(parameterName : ParameterType) : return\_type}$$
#### Rules for Operations (Slide 40 & 42):
  1. **Omission on Constructors:** Constructors omit the return type entirely (e.g., `+ Rectangle(width: int, height: int)`).
  2. **Omission on Void:** Methods that return nothing omit the return type (e.g., `+ resetTimer()`).
  3. **Omitting Trivial Getters and Setters:** Slide 40 explicitly recommends omitting standard `get()` and `set()` accessors unless they contain specialized validation logic. Cluttering diagrams with twenty basic accessors obscures architectural relationships.
  4. **Interface Mandate:** While trivial methods can be omitted from standard classes, **never omit operations from an interface**. An interface exists solely to publish an operational contract; omitting an operation invalidates the contract.
  5. **Inherited Methods:** Do not redraw inherited methods in subclasses unless the subclass explicitly overrides the behavior.
  
  ---
## 4. Identifying Candidate Classes: The Noun Extraction Technique
  
  Slides 12, 13, and 36 address how engineers identify classes from raw requirements.
  
  Slide 36 outlines the **Noun Extraction Method**:
  1. Take a concise textual description of the system requirements.
  2. Underline all nouns and noun phrases.
  3. Treat identified nouns as **candidate classes**.
  4. Filter candidate nouns through a systematic evaluation matrix.
  
  ```
                             +------------------------+
                             |    UNDERLINE NOUNS     |
                             |   (Candidate Pool)     |
                             +-----------+------------+
                                         |
         +-------------------------------+-------------------------------+
         |                               |                               |
         v                               v                               v
  +------------------+            +------------------+            +------------------+
  |    NOT A CLASS   |            |    NOT A CLASS   |            |    VALID CLASS   |
  | (Is an Attribute)|            | (Outside Scope)  |            | (Independent     |
  |                  |            |                  |            |  Domain Entity)  |
  +------------------+            +------------------+            +------------------+
  | • GPA            |            | • Campus Town    |            | • Course         |
  | • Birthdate      |            | • Government Tax |            | • Student        |
  | • Car Color      |            |   Auditor        |            | • Instructor     |
  +------------------+            +------------------+            +------------------+
  ```
### The Candidate Class Filter Questions (Slide 36)
  * **Is it inside our system scope?** If a noun represents an external entity that our software does not track or manage, discard it.
  * **Is it an attribute of another object?** A noun that has no independent behaviors and merely measures or describes another entity is an attribute, not a class.
  * *Example (Slide 13):* `GPA` is a numeric attribute belonging to `Student`. It cannot stand alone as a class.
  * *Example (Slide 13):* `Car` is a class because it has state (VIN, make, model, odometer) and behaviors (`start()`, `drive()`, `brake()`).
  * **Is it a transient event, state, or time period?** Nouns like `Tuesday`, `Login Time`, or `Success` are usually states or attribute values, not classes.
  * **Is it a synonym?** If the requirements refer to `Tutor`, `Teacher`, and `Instructor` interchangeably, normalize them into a single class (`Instructor`).
  
  ---
### Case Study: University Course Catalog (Slide 37–39)
  Consider the following requirements snippet:
  > *"Departments offer many courses... Courses are taught by instructors... A course has title, description, name, credit hours, and prerequisite."*
  
  ```
  +--------------------------------------------------------------------------+
  |                  NOUN FILTERING ANALYSIS (SLIDES 37–39)                  |
  +-------------------+-------------------+----------------------------------+
  | Identified Noun   | Classification    | Engineering Rationale            |
  +-------------------+-------------------+----------------------------------+
  | Department        | Class             | Independent entity; holds        |
  |                   |                   | courses and employs instructors. |
  +-------------------+-------------------+----------------------------------+
  | Course            | Class             | Core business entity; possesses  |
  |                   |                   | multiple attributes and methods. |
  +-------------------+-------------------+----------------------------------+
  | Instructor        | Class             | Human actor/entity with roles    |
  |                   |                   | (Professor vs. Adjunct).         |
  +-------------------+-------------------+----------------------------------+
  | Title, Name,      | Attributes        | Atomic descriptive properties    |
  | Description       |                   | belonging directly to `Course`.  |
  +-------------------+-------------------+----------------------------------+
  | Credit Hours      | Attribute         | Numeric integer field on `Course`|
  +-------------------+-------------------+----------------------------------+
  | Prerequisite      | Association       | A self-referential relationship  |
  |                   | (Relationship)    | linking one `Course` to another. |
  +-------------------+-------------------+----------------------------------+
  ```
  
  ---