### **ER-to-Relational Mapping Algorithm (Continued)**
- #### **Step 3: Mapping of Multivalued Attributes**
  
  The relational model has a core rule that all attribute values must be **atomic**. This means that a single cell in a table cannot contain a list or set of values. Therefore, multivalued attributes from the ER model cannot be directly placed into the table of their entity. They must be handled by creating a separate table.
  
  *   **Rule:** For each multivalued attribute `A` of an entity type `E`, create a new relation (table).
  *   **Attributes:**
    *   This new relation will have at least two columns.
    *   One column will hold the values of the multivalued attribute `A` itself.
    *   The other column(s) will be the **primary key** of the entity type `E`. This serves as a **foreign key** to link back to the main entity relation.
  *   **Primary Key:**
    *   The primary key of this new relation is the **combination of all its attributes**. This ensures that each combination of an entity's primary key and a value from the multivalued attribute is unique.
  
  **Note on Modern RDBMSs:**
  Some modern relational database systems now support array or list data types. In such systems, it might be possible to store a multivalued attribute directly in the entity's main table, avoiding the need for a separate relation. However, the separate-relation approach is the traditional and most universally compatible method.
  
  **Example:**
  
  Mapping the `Locations` multivalued attribute of the `DEPARTMENT` entity:
  *   **Entity:** `DEPARTMENT`
  *   **Multivalued Attribute:** `Locations`
  *   **Primary Key of Department:** `Dnumber`
  
  Following the rule:
  1.  Create a new relation: `DEPT_LOCATIONS`.
  2.  Add a column for the multivalued attribute itself. Let's call it `Dlocation`.
  3.  Add the primary key of the owner entity (`DEPARTMENT`) as a foreign key. This is `Dnumber`.
  4.  Define the primary key of `DEPT_LOCATIONS`. It is the combination of all attributes: **(`Dnumber`, `Dlocation`)**.
  
  The final relational schema for the multivalued attribute is:
  *   **DEPT_LOCATIONS**(<u>Dnumber</u>, <u>Dlocation</u>)
  
  This structure correctly models the fact that a single department can be associated with multiple locations.