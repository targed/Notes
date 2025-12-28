### **Lecture 6: The Goal of Relational Database Design**

The primary objective of this process is to take the rich, graphical blueprint of our database from an ER or EER diagram and convert it into a set of relational schemas (i.e., table definitions). This conversion process is systematic and can be thought of as an algorithm with a series of steps.

The overall process is a **seven-step algorithm** that handles mapping all the constructs we've learned about: entities, attributes (simple, composite, multivalued), relationships (of all cardinalities), and weak entities. After covering the basic ER mapping, we will look at the additional strategies needed to map EER concepts like specialization and generalization.

---
- ### **Part 1: Mapping Basic ER Model Constructs**
  
  We will use the **COMPANY ER diagram**as our running example to see how each step is applied.
- #### **Step 1: Mapping of Regular (Strong) Entity Types**
  
  This is the foundational step of the process.
  
  *   **The Rule:** For each regular (strong) entity type `E` in the ER diagram, create a relation (a table) `R` that includes all the simple attributes of `E`.
  *   **Primary Key:** Choose one of the key attributes of `E` to be the primary key of `R`. If the chosen key is composite, the set of simple attributes that form it will together be the primary key of `R`.
  *   **Example:**
    *   The `EMPLOYEE` entity becomes the `EMPLOYEE` relation.
    *   The `DEPARTMENT` entity becomes the `DEPARTMENT` relation.
    *   The `PROJECT` entity becomes the `PROJECT` relation.
  
  At this stage, the `EMPLOYEE` relation would have attributes like `Fname`, `Minit`, `Lname`, `Ssn`, `Bdate`, `Address`, `Sex`, and `Salary`. `Ssn` would be chosen as the primary key.
- #### **Step 2: Mapping of Composite Attributes**
  
  The relational model requires all attributes to be **atomic** (indivisible). Therefore, we cannot directly map a composite attribute.
  
  *   **The Rule:** For any composite attribute, include only its simple component attributes in the new relation. Do not include an attribute for the composite attribute itself.
  *   **Example:**
    *   In the `EMPLOYEE` entity, the `Name` attribute is composite, made up of `Fname`, `Minit`, and `Lname`.
    *   In our `EMPLOYEE` relation, we include `Fname`, `Minit`, and `Lname` as separate columns. We do not create a `Name` column.
- #### **Step 3: Mapping of Multivalued Attributes**
  
  This is another case where the ER model's flexibility doesn't directly fit into the relational model's atomic value rule.
  
  *   **The Rule:** For each multivalued attribute `A`, create a **new relation** `R`. This new relation must include:
    1.  An attribute corresponding to `A`.
    2.  The primary key of the owner entity type (`K`) as a **foreign key** to link back to the original table.
  *   **Primary Key of the New Relation:** The primary key of this new relation `R` is the combination of `A` and `K`.
  *   **Example:**
    *   The `DEPARTMENT` entity has a multivalued attribute `Locations`.
    *   We create a new relation called `DEPT_LOCATIONS`.
    *   This table has two attributes: `Dlocation` (for the location value) and `Dnumber` (the primary key of `DEPARTMENT`, acting as a foreign key).
    *   The primary key of `DEPT_LOCATIONS` is the combination `(Dnumber, Dlocation)`.