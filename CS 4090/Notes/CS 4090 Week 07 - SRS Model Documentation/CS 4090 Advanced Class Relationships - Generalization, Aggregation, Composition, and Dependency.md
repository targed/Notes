## 1. Generalization: The Inheritance Relationship
  
  Slide 43 and 45 define **generalization** as a taxonomic relationship where a specific child element inherits attributes, operations, and associations from a more general parent element. In object-oriented programming, this corresponds to **class inheritance** and **interface implementation**.
  
  ```
  +--------------------------------------------------------------------------+
  |                     GENERALIZATION SYNTAX RULES (SLIDE 45)               |
  +-------------------+------------------------------------------------------+
  | Relationship Type | Visual Line and Arrowhead Standard                   |
  +-------------------+------------------------------------------------------+
  | Class             | Solid line with a closed triangular arrowhead        |
  | Inheritance       | pointing upward to the parent class.                 |
  +-------------------+------------------------------------------------------+
  | Abstract Class    | Solid line with a hollow/white triangular arrowhead  |
  | Inheritance       | pointing upward to the abstract superclass.          |
  +-------------------+------------------------------------------------------+
  | Interface         | Dashed line with a hollow/white triangular arrowhead |
  | Realization       | pointing upward to the «interface».                  |
  +-------------------+------------------------------------------------------+
  ```
  
  ```
                       +-------------------------------+
                       |          «interface»          |
                       |             Shape             |
                       +---------------+---------------+
                                       ^
                                       | (Dashed line, hollow arrow)
                                       |
                       +---------------+---------------+
                       |       RectangularShape        |  <-- Abstract Class
                       |          {abstract}           |      (Italicized Name)
                       +---------------+---------------+
                                       ^
                                       | (Solid line, hollow arrow)
                                       |
                       +---------------+---------------+
                       |           Rectangle           |  <-- Concrete Subclass
                       +-------------------------------+
  ```
### The Rule of Trivial Generalization (Slide 45)
  Slide 45 highlights an important modeling convention: **Do not draw trivial or obvious generalization relationships.**
  * In languages like Java or C#, every object implicitly inherits from a root `Object` class.
  * Drawing an `Object` superclass above every domain entity adds zero architectural value and clutters the diagram. Generalization should only be modeled when it introduces domain-specific polymorphic behavior or shared business attributes.
### Examples of Domain Generalization (Slide 48)
  * **Entity Specialization:** A base `Flight` class specializes into `DomesticFlight` and `InternationalFlight` (which adds customs and passport verification attributes).
  * **Catalog Specialization:** A generic `LibraryItem` specializes into `Book`, `VideoRecord`, and `AudioRecord`.
  * **Actor Generalization:** In user modeling, specialized actors inherit associations from general roles (e.g., `Dentist`, `Cardiologist`, and `EyeDoctor` all inherit base associations from `Doctor`).
  
  ---
## 2. Aggregation vs. Composition: The Whole-Part Continuum
  
  A common point of confusion in software design is distinguishing between a standard association, an aggregation, and a composition. Both aggregation and composition represent **whole-part relationships**, but they differ fundamentally in **lifecycle ownership**.
  
  Slide 46 contrasts these two models:
  
  ```
  +--------------------------------------------------------------------------+
  |                 AGGREGATION VS. COMPOSITION (SLIDE 46)                   |
  +---------------------+----------------------------------------------------+
  | Dimension           | Aggregation ("Has-A" / "Is Part Of")               |
  +---------------------+----------------------------------------------------+
  | Semantic Meaning    | Weak whole-part relationship.                      |
  +---------------------+----------------------------------------------------+
  | Visual Notation     | Solid line with a hollow (clear white) diamond at  |
  |                     | the whole/aggregate end.                           |
  +---------------------+----------------------------------------------------+
  | Lifecycle Binding   | Independent Lifecycles: If the whole is destroyed, |
  |                     | the constituent parts continue to exist.           |
  +---------------------+----------------------------------------------------+
  | Code Equivalent     | Passed by reference or pointer. The parts are      |
  |                     | created outside and passed into the container.     |
  +---------------------+----------------------------------------------------+
  | Dimension           | Composition ("Is Entirely Made Of")                |
  +---------------------+----------------------------------------------------+
  | Semantic Meaning    | Strong whole-part relationship.                    |
  +---------------------+----------------------------------------------------+
  | Visual Notation     | Solid line with a filled black diamond at the      |
  |                     | whole/composite end.                               |
  +---------------------+----------------------------------------------------+
  | Lifecycle Binding   | Tied Lifecycles ("Death Relationship"): The parts  |
  |                     | live and die with the whole. If the whole is       |
  |                     | destroyed, the parts are destroyed simultaneously. |
  +---------------------+----------------------------------------------------+
  | Code Equivalent     | Passed by value or instantiated internally. The    |
  |                     | parts cannot exist without the parent container.   |
  +---------------------+----------------------------------------------------+
  ```
  
  ---
### Detailed Analysis of Slide 46 Examples
#### Aggregation Example: Car and Engine
  ```
  +-------+ 1                                                1 +--------+
  |  Car  | <>------------------------------------------------ | Engine |
  +-------+                                                    +--------+
  ```
  * A `Car` has an `Engine`.
  * If the car is decommissioned or sent to a junkyard, the engine can be removed, refurbished, and installed into an entirely different vehicle.
  * The engine exists independently of the specific car instance. Therefore, the relationship is **Aggregation** (clear white diamond).
#### Composition Example: Book and Page
  ```
  +--------+ 1                                               * +--------+
  |  Book  | *------------------------------------------------ |  Page  |
  +--------+                                                   +--------+
  ```
  * A `Book` is composed of `Pages`.
  * The pages physically and conceptually belong strictly to that specific book. A page cannot be shared simultaneously across two different books.
  * If the book is burned or deleted from disk, all of its constituent pages are destroyed with it.
  * Therefore, the relationship is **Composition** (filled black diamond).
  
  ---
## 3. Dependency: Temporary Operational Usage
  
  Slide 46 and 47 define **dependency** as a relationship where one class uses another class temporarily without storing a permanent structural reference.
  
  * **Notation:** A **dashed line with an open arrowhead** (`- - - >`) pointing from the dependent class to the independent class it relies upon.
  * **Semantic Definition:** A relationship between two modeling elements where a change to the independent element will force a change in the dependent element (Slide 47).
  
  ```
  +-----------------+                                       +----------------+
  |  LotteryTicket  | - - - - - - - - - - - - - - - - - - > |     Random     |
  +-----------------+                                       +----------------+
  ```
### Why LotteryTicket Depends on Random (Slide 46):
  * The `LotteryTicket` class has a method `generateLuckyNumbers()` that accepts a `Random` generator instance as a parameter or instantiates it locally on the stack.
  * The ticket does not store `Random` as an instance attribute (a lottery ticket does not "have" a random generator as part of its permanent state).
  * However, if the public interface of the `Random` class changes (e.g., `nextInt()` is renamed or deprecated), `LotteryTicket` will fail to compile.
  * **Architectural Rule:** Keep dependencies minimal. A high number of dependencies across subsystems indicates **tight coupling**, making refactoring and testing difficult.
  
  ---
## 4. Master Comparison: Core Structural Relationships
  
  Slide 47 provides a consolidated reference table comparing all four foundational UML structural constructs:
  
  ```
  +--------------------------------------------------------------------------+
  |                  CORE STRUCTURAL RELATIONSHIPS (SLIDE 47)                |
  +---------------+-------------------------------------+--------------------+
  | Construct     | Formal Description                  | UML Syntax         |
  +---------------+-------------------------------------+--------------------+
  | Association   | A relationship between two or more  | Solid line         |
  |               | classifiers that involves           | ------------------ |
  |               | connections among their instances.  |                    |
  +---------------+-------------------------------------+--------------------+
  | Aggregation   | A special form of association that  | Solid line with    |
  |               | specifies a weak whole-part         | hollow diamond     |
  |               | relationship (independent life).    | <>---------------- |
  +---------------+-------------------------------------+--------------------+
  | Generalization| A taxonomic relationship between a  | Solid line with    |
  |               | more general element and a more     | hollow triangle    |
  |               | specific element (inheritance).     | -------|>          |
  +---------------+-------------------------------------+--------------------+
  | Dependency    | A relationship where a change to an | Dashed line with   |
  |               | independent element affects the     | open arrow         |
  |               | dependent element ("uses").         | - - - - - - - - -> |
  +---------------+-------------------------------------+--------------------+
  ```
  
  ---
## 5. Architectural Synthesis: The Video Rental System (Slide 52)
  
  Slide 52 presents a comprehensive class diagram combining all major relationships into a unified business domain model. Analyzing this diagram provides a template for capstone architectural design:
  
  ```
  +--------------------------------------------------------------------------+
  |            VIDEO RENTAL SYSTEM MASTER CLASS MODEL (SLIDE 52)             |
  +--------------------------------------------------------------------------+
  |                                                                          |
  |  +------------+                                                          |
  |  |  Customer  |                                                          |
  |  +-----+------+                                                          |
  |        | 1                                                               |
  |        |                                                                 |
  |        | <> (Simple Aggregation)                                         |
  |        v                                                                 |
  |  +-----+--------------+ 1                   0..1 +--------------------+  |
  |  |   RentalInvoice    | ------------------------ |   CheckoutScreen   |  |
  |  +-----+--------------+    (Simple Association)  +--------------------+  |
  |        | 1                                                               |
  |        |                                                                 |
  |        | * (Composition)                                                 |
  |        v                                                                 |
  |  +-----+--------------+                                                  |
  |  |    RentalItem      |  <-- {Abstract Class}                            |
  |  +-----+--------------+                                                  |
  |        ^                                                                 |
  |        | (Generalization / Inheritance)                                  |
  |   +----+--------------------+                                            |
  |   |                         |                                            |
  | +-+------------+     +------+-------+     +---------------+              |
  | |   DVDMovie   |     |   VHSMovie   |     |   VideoGame   |              |
  | +--------------+     +--------------+     +---------------+              |
  +--------------------------------------------------------------------------+
  ```
### Architectural Deconstruction of the Model:
  1. **Customer to RentalInvoice (Aggregation):**
   * Multiplicity: `1` Customer links to multiple invoices over time.
   * Notation: Hollow diamond (`<>`).
   * Rationale: The customer owns their invoices, but if a customer account is archived or deactivated, the financial invoices cannot be erased from the accounting database. They retain independent existence for tax compliance.
  2. **RentalInvoice to RentalItem (Composition):**
   * Multiplicity: `1` Invoice contains `1..*` Rental Items.
   * Notation: Filled black diamond (`*`).
   * Rationale: A specific line item on a receipt cannot exist without the invoice itself. If an unsubmitted checkout invoice is aborted, its transaction line items are purged from memory immediately.
  3. **RentalItem to Subclasses (Generalization):**
   * Structure: `RentalItem` is an **abstract superclass** (rendered in italics).
   * Subclasses: `DVDMovie`, `VHSMovie`, and `VideoGame` inherit the base attributes (`rentalPrice`, `itemID`, `barcode`) while implementing media-specific checkout rules.
  4. **RentalInvoice to CheckoutScreen (Simple Association):**
   * Multiplicity: `1` Invoice is linked to `0..1` Checkout Screen.
   * Rationale: During active scanning, an invoice is bound to a physical POS checkout terminal. Once checkout finishes, the link resets to zero.
  
  ---