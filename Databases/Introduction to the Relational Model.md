### **Introduction to the Relational Model**

The relational model was first proposed in 1970 by Dr. E. F. Codd of IBM Research in his groundbreaking paper, "A Relational Model for Large Shared Data Banks". This model revolutionized database management by providing a simple, intuitive way to represent data while being grounded in the solid mathematical foundations of set theory and predicate logic. Its basic building block is the **relation**, which we will see is conceptually equivalent to a table.
- ### **Relational Model Concepts**
  
  The relational model represents a database as a collection of relations. Informally, a **relation** is a table of values. Each relation has a name and is composed of rows and columns.
- #### **Formal Terminology**
  
  The relational model uses precise, formal terms for its components:
  
  *   **Attribute:** A column header of the table. It is the name of a role played by some domain (e.g., `Name`, `Ssn`, `Age`).
  *   **Domain:** The set of all possible atomic values for an attribute. For example, the domain for the `Age` attribute might be integers from 16 to 100. The domain for `GPA` would be real numbers between 0.0 and 4.0.
  *   **Tuple:** A row in the table. Each tuple represents a single entity or a single relationship instance. It is an ordered list of values.
  *   **Relation:** The table as a whole. Formally, it is a set of tuples.
  
  **Schema vs. State**
  
  It's crucial to distinguish between the database's structure and its content:
  
  *   **Relation Schema, R(A₁, A₂, ..., Aₙ):** This is the *description* of the relation. It consists of the relation name (R) and a list of its attributes (A₁, A₂, ...). For example: `STUDENT(Name:string, Ssn:string, Age:integer, Gpa:real)`. The schema is relatively static and doesn't change often.
  *   **Relation State, r(R):** This is the *data* in the relation at a specific moment in time. It is a set of tuples `{t₁, t₂, ..., tₘ}`. The state is dynamic and changes frequently as data is added, deleted, or updated. A state is considered **valid** only if it satisfies all the constraints defined in the schema.
- #### **Characteristics of Relations**
  
  Because a relation is formally a **set** of tuples, it has several important characteristics derived from set theory:
  
  1.  **Tuple Ordering is Not Important:** Since a relation is a set, the order of the rows is not part of the relation's definition.
  2.  **No Duplicate Tuples:** A set, by definition, cannot contain identical elements. Therefore, every tuple in a relation must be distinct from every other tuple.
  3.  **Attribute Ordering is Not Important (in theory):** At a formal level, the order of columns doesn't matter because each value is tied to a specific attribute name. `(Name: 'John', Age: 20)` is the same as `(Age: 20, Name: 'John')`. In practice, most SQL implementations use a defined order for columns.
  4.  **Values are Atomic:** Each cell at the intersection of a row and column must hold a single, indivisible (atomic) value. Composite and multivalued attributes (from the ER model) are not directly allowed and must be handled differently.
- ### **Relational Model Constraints**
  
  Constraints are the rules that ensure data integrity. They restrict the values that can be stored in a database state.
- #### **1. Schema-Based Constraints**
  
  These are explicit rules that are defined in the database schema and are enforced by the DBMS.
  
  **A. Key Constraints**
  
  Keys are fundamental to ensuring that tuples are unique.
  *   **Superkey:** A set of attributes that uniquely identifies a tuple within a relation. A superkey may contain extra, redundant attributes. For example, `{Ssn, Name}` is a superkey for `STUDENT` because `{Ssn}` alone is already unique.
  *   **Key (or Candidate Key):** A **minimal** superkey. This means it is a superkey, but if you remove any attribute from it, it is no longer a superkey. A relation can have multiple candidate keys. For the `CAR` relation, both `{License_number}` and `{Engine_serial_number}` could be candidate keys.
  *   **Primary Key:** The candidate key that is chosen by the database designer to be the primary means of identifying tuples. Its attributes are usually **underlined** in the schema diagram. The general rule is to choose the smallest (fewest attributes) candidate key.
  
  **B. Entity Integrity Constraint**
  
  This is a simple but critical rule: **The primary key of a relation cannot have a NULL value.**
  *   **Why?** The primary key is used to uniquely identify each tuple. If it were NULL, we wouldn't be able to identify that specific row, defeating its purpose. If the primary key is composite, none of its constituent attributes can be NULL.
  
  **C. Referential Integrity and Foreign Keys**
  
  This constraint is used to maintain consistency *between* tables. It ensures that a value that appears in one relation for a given set of attributes also appears for a certain set of attributes in another relation.
  *   **Foreign Key (FK):** A set of attributes in one relation (the **referencing relation**) that refers to the primary key of another relation (the **referenced relation**).
  *   **The Rule:** The value of the foreign key in the referencing relation must either:
    1.  Match an existing primary key value in the referenced relation.
    2.  Be NULL (if the foreign key attribute allows NULLs).
  *   **Example:** In the COMPANY database, the `Dno` attribute in the `EMPLOYEE` table is a foreign key that references the `Dnumber` (the primary key) of the `DEPARTMENT` table. This ensures that no employee can be assigned to a department that does not exist.
- #### **2. Application-Based Constraints (Business Rules)**
  
  These are more complex rules that cannot be directly expressed by the schema. They are often called **semantic constraints** and must be enforced by the application programs or through advanced database features like triggers and assertions.
  *   **Example:** "The salary of an employee should not exceed the salary of their supervisor." This requires comparing two different tuples, which is beyond the scope of a simple key or domain constraint.