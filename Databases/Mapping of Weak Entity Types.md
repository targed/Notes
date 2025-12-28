### **Step 7: Mapping of Weak Entity Types**

Weak entities require a special mapping rule because they do not have a primary key of their own. Their identity is dependent on their owner entity.

*   **The Rule:** For each weak entity type `W` with an owner entity type `E`, create a **new relation** `R` and include all the simple attributes of `W` in `R`.
*   **Foreign Key:** Add the primary key of the owner entity `E` as a **foreign key** in the new relation `R`.
*   **Primary Key of the New Relation:** The primary key of the new relation `R` is the **combination** of:
  1.  The primary key of the owner entity (the new foreign key).
  2.  The **partial key** of the weak entity type `W`.
*   **Example (`DEPENDENT` weak entity):**
  *   The `DEPENDENT` weak entity is owned by the `EMPLOYEE` entity through the `DEPENDENTS_OF` relationship.
  *   We create a new relation called `DEPENDENT`.
  *   We include all the simple attributes of the weak entity: `Dependent_name`, `Sex`, `Bdate`, and `Relationship`.
  *   We include the primary key of the owner (`EMPLOYEE`), which is `Ssn`, as a foreign key. We will name this `Essn`.
  *   The primary key for the `DEPENDENT` relation is the composite key `(Essn, Dependent_name)`, combining the owner's primary key and the weak entity's partial key.

After these 7 steps, we have a complete relational database schema  that corresponds to our original ER diagram.

---
- ### **Part 2: Mapping EER Model Constructs**
  
  Now we will look at the additional rules needed to map the concepts from the Enhanced ER (EER) model, namely specialization and generalization hierarchies.
- #### **Mapping Specialization/Generalization**
  
  Mapping a superclass/subclass hierarchy to a set of relational tables is a crucial design decision, and there are several different strategies. The best choice depends on the specific constraints of the specialization (disjoint/overlapping, total/partial) and the expected query patterns.
  
  The lecture presents four main options for this mapping.
- ##### **Option 8A: Multiple Relations - Superclass and Subclasses**
  
  *   **The Rule:** Create a relation for the superclass and a separate relation for *each* of its subclasses.
    *   **Superclass Relation:** Contains the primary key and all the shared attributes.
    *   **Subclass Relations:** Each contains the primary key of the superclass (which acts as both the primary key and a foreign key for this relation) and only the specific, local attributes of that subclass.
  *   **How it works:** To get the full information for a subclass entity (e.g., a `SECRETARY`), you would need to `JOIN` the `EMPLOYEE` superclass table with the `SECRETARY` subclass table using the shared primary key (`Ssn`).
  *   **Best for:** This is the most general and flexible option. It works for **any type** of specialization (disjoint, overlapping, total, or partial).
- ##### **Option 8B: Multiple Relations - Subclasses Only**
  
  *   **The Rule:** Create a relation for *each subclass only*. Do not create a relation for the superclass. Each subclass relation includes all the shared attributes from the superclass plus its own local attributes.
  *   **How it works:** All the information for a given entity is in one table. There is no need to join to a superclass table.
  *   **Best for:** This option works well for **total and disjoint** specializations. If it were partial, there would be nowhere to store superclass entities that don't belong to a subclass. If it were overlapping, an entity belonging to multiple subclasses would have its shared information (like name and address) duplicated across multiple tables, which is undesirable.
- ### **ER-to-Relational Mapping Algorithm (Continued)**
- #### **Step 2: Mapping of Weak Entity Types**
  
  Weak entities require special handling because they do not have their own primary key and depend on an owner entity for their identification.
  
  *   **Rule:** For each weak entity type `W` with an owner entity type `E`, create a new relation (table).
  *   **Attributes:**
    *   Include all the **simple** attributes of the weak entity `W` as columns in the new relation.
  *   **Foreign Key:**
    *   Include the primary key of the owner entity `E` as columns in the new relation for `W`. These columns act as a **foreign key** that references the primary key of the owner's relation.
  *   **Primary Key:**
    *   The primary key of the new relation `W` is the **combination** of the foreign key (the primary key of the owner `E`) and the **partial key** of the weak entity `W`.
  
  **Important Consideration: Referential Integrity (CASCADE)**
  Because a weak entity's existence is completely dependent on its owner, it is common to specify the `CASCADE` option for the foreign key constraint. This means:
  *   `ON DELETE CASCADE`: If the owner entity (e.g., an employee) is deleted from the database, all of its corresponding weak entities (e.g., that employee's dependents) are automatically deleted as well.
  *   `ON UPDATE CASCADE`: If the primary key of the owner entity is updated (which is rare but possible), the corresponding foreign key values in the weak entity's table are automatically updated to match.
  This `CASCADE` behavior correctly models the "existence dependency" of the weak entity.
  
  **Example:**
  
  Mapping the `DEPENDENT` weak entity from the COMPANY diagram:
  *   **Weak Entity:** `DEPENDENT`
  *   **Owner Entity:** `EMPLOYEE`
  *   **Partial Key of Dependent:** `Dependent_name`
  *   **Primary Key of Employee (Owner):** `Ssn`
  
  Following the rule:
  1.  Create a new relation: `DEPENDENT`.
  2.  Add its simple attributes: `Dependent_name`, `Sex`, `Bdate`, `Relationship`.
  3.  Add the primary key of the owner (`EMPLOYEE`) as a foreign key. The primary key of `EMPLOYEE` is `Ssn`. So, we add an `Essn` column to the `DEPENDENT` table.
  4.  Define the primary key of the `DEPENDENT` relation. It is the combination of the owner's key and the partial key: **(`Essn`, `Dependent_name`)**.
  
  The final relational schema for the weak entity is:
  *   **DEPENDENT**(<u>Essn</u>, <u>Dependent_name</u>, Sex, Bdate, Relationship)