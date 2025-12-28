### **Lecture 6: Introduction to Data Model Mapping**

After creating a conceptual schema using an ER or EER diagram, the next step in the database design process is to map this high-level model into an implementation data model. Since the relational model is the most common, this lecture focuses on a specific set of procedures for converting ER/EER diagrams into a relational database schema (a set of tables). This process is often called **data model mapping**.
- #### **Goals During Mapping**
  
  When performing this translation, we have three primary objectives:
  
  1.  **Preserve all information:** We must ensure that every attribute and entity from the conceptual model is represented in the final relational schema. No data should be lost.
  2.  **Maintain constraints:** We need to preserve the constraints defined in the ER diagram (like keys, cardinality ratios, and participation) to the greatest extent possible within the relational model's capabilities. Some complex ER constraints may not map directly and might need to be enforced by the application instead.
  3.  **Minimize NULL values:** A good relational schema design avoids having an excessive number of NULL values in the tables, as they can complicate queries and waste space.
- ### **The ER-to-Relational Mapping Algorithm**
  
  The lecture outlines a step-by-step algorithm to perform this mapping. We will go through each step in detail.
- #### **Step 1: Mapping of Regular (Strong) Entity Types**
  
  This is the most straightforward step and forms the foundation of the schema.
  
  *   **Rule:** For each regular (strong) entity type in the ER diagram, create a new relation (table). The name of the relation is generally the same as the entity type.
  *   **Attributes:**
    *   Include all the **simple** attributes of the entity as columns in the new relation.
    *   For any **composite** attributes, include only their simple component attributes. For example, if an `EMPLOYEE` entity has a composite `Name` attribute (`Fname`, `Minit`, `Lname`), the `EMPLOYEE` relation will have three separate columns: `Fname`, `Minit`, and `Lname`.
  *   **Primary Key:**
    *   Choose one of the candidate keys of the entity type to be the **primary key** of the relation.
    *   If the chosen key is composite, the set of corresponding columns becomes the primary key. The primary key columns should be underlined in the schema description.
  
  **Example:**
  
  Following this step, the strong entities from the COMPANY diagram are mapped as follows:
  
  *   **EMPLOYEE**(<u>Fname</u>, <u>Minit</u>, <u>Lname</u>, <u>Ssn</u>, Bdate, Address, Sex, Salary)
    *   *Note: This example shows multiple candidate keys being underlined initially. We will refine this.* The final primary key chosen is `Ssn`.
    *   **Final Schema:** EMPLOYEE(Fname, Minit, Lname, <u>Ssn</u>, Bdate, Address, Sex, Salary)
  *   **DEPARTMENT**(<u>Dname</u>, <u>Dnumber</u>)
    *   *Refined with `Dnumber` as PK:* DEPARTMENT(Dname, <u>Dnumber</u>)
  *   **PROJECT**(<u>Pname</u>, <u>Pnumber</u>, Plocation)
    *   *Refined with `Pnumber` as PK:* PROJECT(Pname, <u>Pnumber</u>, Plocation)