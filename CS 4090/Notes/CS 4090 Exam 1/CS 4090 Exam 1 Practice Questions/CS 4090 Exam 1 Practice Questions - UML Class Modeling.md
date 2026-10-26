### **Questions**
#### **Question 1 [Multiple Choice]**
  What does a UML Class Model primarily describe?
  
  * A. The sequence and timing of network messages exchanged across an untrusted network.
  * B. The static structure of a software system, including classes, attributes, operations, and relationships.
  * C. The runtime conditions and event transitions an object experiences over its lifecycle.
  * D. The physical server racks and container clusters used to host the production application.
  
  ---
#### **Question 2 [Matching]**
  Match each UML visibility symbol and attribute marker with its formal meaning:
  
  1. `+`
  2. `-`
  3. `#`
  4. `~`
  5. `/`
  
  * **A.** Private (accessible only within the defining class)
  * **B.** Derived (computed dynamically from other attributes; not physically stored)
  * **C.** Public (accessible by any class in the system)
  * **D.** Package / Default (accessible within the same namespace or package)
  * **E.** Protected (accessible within the defining class and its subclasses)
  
  ---
#### **Question 3 [Multiple Choice]**
  Which of the following method declarations strictly follows the formal UML operation syntax presented in the lecture slides?
  
  * A. `public double : calculateTotal(int orderId)`
  * B. `+ calculateTotal(orderId: int): double`
  * C. `calculateTotal + (orderId: int) -> double`
  * D. `+ double calculateTotal(int: orderId)`
  
  ---
#### **Question 4 [Multiple Choice]**
  An `Order` contains multiple `OrderItem` objects. If the customer deletes the unsubmitted `Order`, its constituent `OrderItem` line items must be removed from memory simultaneously because they have no independent existence outside the order. Which UML relationship best communicates this behavior?
  
  * A. Aggregation (hollow diamond)
  * B. Dependency (dashed arrow)
  * C. Composition (filled black diamond)
  * D. Generalization (hollow triangular arrow)
  
  ---
#### **Question 5 [Multiple Choice]**
  In a network architecture diagram, an association line is drawn with an open arrowhead pointing from `Router` to `DomainNameServer` (`Router -> DomainNameServer`). What does this navigability arrow indicate?
  
  * A. The `DomainNameServer` initiates connections to the `Router`, but the `Router` is unaware of the server.
  * B. The `Router` requests address resolution services from the `DomainNameServer`, while the `DomainNameServer` stores no reference to the `Router`.
  * C. `Router` inherits all public attributes and operations from `DomainNameServer`.
  * D. `DomainNameServer` is a physical component permanently embedded inside the `Router` hardware.
  
  ---
#### **Question 6 [True / False]**
  Generalization should be used whenever two classes share one or two similar attributes (such as `name` and `address`), even if no true conceptual "is-a" relationship exists between them.
  
  * True
  * False
  
  ---
#### **Question 7 [Multiple Choice]**
  When modeling a retail system, a customer purchases a `Product` and registers a `Warranty`. The attributes `serialNumber` and `warrantyCode` describe the specific transaction linking the two entities, rather than belonging intrinsically to the catalog product or the generic warranty policy. How should this be modeled in UML?
  
  * A. As a reflexive self-association looping back to `Product`.
  * B. As an Association Class (Link Class) connected via a dashed line to the association line between `Product` and `Warranty`.
  * C. By converting `Warranty` into a private attribute inside `Product`.
  * D. By creating a multiple inheritance generalization hierarchy.
  
  ---
#### **Question 8 [Matching]**
  Match each UML structural relationship to its visual notation:
  
  1. Generalization
  2. Association
  3. Composition
  4. Aggregation
  5. Dependency
  
  * **A.** Solid line with a filled black diamond at the whole end
  * **B.** Solid line with a hollow triangular arrowhead pointing to the parent
  * **C.** Plain solid line, optionally decorated with multiplicity and role names
  * **D.** Dashed line with an open arrowhead pointing to the independent provider
  * **E.** Solid line with a hollow (clear white) diamond at the whole end
  
  ---
#### **Question 9 [Multiple Answer]**
  When applying the Noun Extraction method to raw requirements text, which of the following candidate nouns should typically be **rejected** as domain classes? (Select all that apply.)
  
  * [ ] `GPA` (because it is an atomic numeric attribute belonging to `Student`)
  * [ ] `Course` (because it represents a core independent business entity with behaviors)
  * [ ] `Click Submit Button` (because it represents an interface action rather than a domain entity)
  * [ ] `Tuesday` (because it represents a transient calendar value or time attribute)
  * [ ] `Instructor` (because it represents an operational actor managing courses)
  
  ---
#### **Question 10 [Multiple Choice]**
  Why is directly modeling relational database tables as domain classes identified as an architectural mistake?
  
  * A. Database tables cannot be exported to SQL files if they are drawn in UML.
  * B. A class model is an object-oriented structural design model representing behaviors, encapsulated operations, and runtime responsibilities, not merely a passive Entity-Relationship (ER) data schema.
  * C. Database tables support only one-to-one multiplicity relationships.
  * D. UML class diagrams prohibit the use of primary keys.
  
  ---
### **Answers and Explanations**
#### **Question 1**
  * **Answer:** **B**
  * **Explanation:** A UML Class Model describes the static structure of a software system. It specifies the classes that exist, their attributes (data), their operations (behaviors), and the structural relationships linking them.
#### **Question 2**
  * **Answer:**
  * **1 $\rightarrow$ C** (`+` Public: accessible everywhere).
  * **2 $\rightarrow$ A** (`-` Private: encapsulated within class).
  * **3 $\rightarrow$ E** (`#` Protected: accessible in class and subclasses).
  * **4 $\rightarrow$ D** (`~` Package/Default: accessible within package).
  * **5 $\rightarrow$ B** (`/` Derived: dynamically computed, not physically stored).
#### **Question 3**
  * **Answer:** **B**
  * **Explanation:** Slide 42 defines the formal operation syntax as: `visibility methodName(parameterName: ParameterType): ReturnType`. Thus, `+ calculateTotal(orderId: int): double` is correct.
#### **Question 4**
  * **Answer:** **C**
  * **Explanation:** Composition represents a strong whole-part relationship where the parts live and die with the whole (tied lifecycles). Because `OrderItem` cannot exist independently of its parent `Order`, it is modeled using composition (filled black diamond).
#### **Question 5**
  * **Answer:** **B**
  * **Explanation:** Slide 29 explains navigability: an open arrowhead indicates unidirectional association. The `Router` knows about and invokes methods on the `DomainNameServer`, but the server has no reference to or knowledge of the `Router`, enforcing loose coupling.
#### **Question 6**
  * **Answer:** **False**
  * **Explanation:** Sharing attributes alone does not justify generalization. Generalization requires a true conceptual "is-a" relationship (e.g., `Student` is a `User`). Sharing attributes like `name` or `address` without an is-a relationship should be modeled using aggregation or composition (e.g., holding a reference to an `Address` object).
#### **Question 7**
  * **Answer:** **B**
  * **Explanation:** Slide 30 explains that when attributes belong to the relationship itself rather than either participating class, UML models the connection as an Association Class (Link Class) attached via a dashed line.
#### **Question 8**
  * **Answer:**
  * **1 $\rightarrow$ B** (Generalization: solid line with hollow triangular arrowhead).
  * **2 $\rightarrow$ C** (Association: plain solid line).
  * **3 $\rightarrow$ A** (Composition: solid line with filled black diamond).
  * **4 $\rightarrow$ E** (Aggregation: solid line with hollow white diamond).
  * **5 $\rightarrow$ D** (Dependency: dashed line with open arrow).
#### **Question 9**
  * **Answer:**
  * **`GPA` (because it is an atomic numeric attribute belonging to `Student`)**
  * **`Click Submit Button` (because it represents an interface action rather than a domain entity)**
  * **`Tuesday` (because it represents a transient calendar value or time attribute)**
  * **Explanation:** Slide 36 outlines the candidate class filters: nouns that represent single attributes, UI actions, or transient time values are filtered out. `Course` and `Instructor` are valid domain classes.
#### **Question 10**
  * **Answer:** **B**
  * **Explanation:** Class models and ER diagrams are not interchangeable. An ER diagram describes passive data storage and tables. A Class Model describes active object-oriented systems, including encapsulated methods, dynamic polymorphism, interface contracts, and behavioral responsibilities.
  
  ---