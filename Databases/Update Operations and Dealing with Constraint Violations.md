### **Update Operations and Dealing with Constraint Violations**

There are three basic operations that can modify the state of a database:
1.  **INSERT:** Adds a new tuple to a relation.
2.  **DELETE:** Removes an existing tuple from a relation.
3.  **MODIFY (or UPDATE):** Changes the value of one or more attributes in an existing tuple.

A core responsibility of the DBMS is to ensure that none of these operations result in an invalid database state. If an operation would violate a constraint, the DBMS must take action.
- #### **1. The INSERT Operation**
  
  An `INSERT` operation can potentially violate any of the four main schema-based constraints:
  
  *   **Domain Constraint:** Violated if a new attribute value is not of the specified data type or format (e.g., inserting "John" into an `Age` attribute).
  *   **Key Constraint:** Violated if the primary key value of the new tuple already exists in another tuple in the relation.
  *   **Entity Integrity:** Violated if the primary key of the new tuple is `NULL`.
  *   **Referential Integrity:** Violated if the value of a foreign key in the new tuple does not match any primary key value in the referenced table.
  
  **Default Action:** If an `INSERT` violates any of these constraints, the default action for the DBMS is to **reject the insertion** entirely. The operation fails, and the database state remains unchanged.
- #### **2. The DELETE Operation**
  
  A `DELETE` operation is simpler in terms of potential violations. It cannot violate primary key, entity integrity, or domain constraints. However, it can violate **referential integrity**.
  
  *   **Problem:** This occurs if the primary key of the tuple being deleted is referenced by foreign keys in other tuples. If the deletion were allowed, those foreign keys would now be pointing to a non-existent record, making the database inconsistent.
  
  **Possible Actions to Handle Violation:**
  When a referential integrity violation is detected during a delete, the DBMS can be configured to take one of several actions:
  
  1.  **RESTRICT (or REJECT):** The `DELETE` operation is rejected if the tuple is referenced by any other tuples. This is the default and safest option.
  2.  **CASCADE:** The `DELETE` operation is permitted, and the DBMS automatically **deletes all tuples that reference it** in other tables. This can cascade through multiple tables if there are chains of references. This is a powerful but potentially dangerous option if not used carefully.
  3.  **SET NULL or SET DEFAULT:** The `DELETE` operation is permitted, and the referencing foreign key values are automatically changed to `NULL` or to a predefined default value. This is only possible if the foreign key attributes are allowed to be `NULL`. This is a more conservative approach than cascading.
- #### **3. The MODIFY (or UPDATE) Operation**
  
  A `MODIFY` operation can violate constraints depending on which attribute is being changed.
  
  *   **Updating a Non-Key Attribute:** This can only violate a **domain constraint** (if the new value is of the wrong type).
  *   **Updating a Primary Key Attribute:** This is more complex. It is treated like a `DELETE` followed by an `INSERT`. It can violate all the same constraints as an `INSERT` (key, entity integrity) and can also cause a **referential integrity** violation, just like a `DELETE`, if the old primary key value was referenced by other tuples. The same options (RESTRICT, CASCADE, SET NULL) apply.
  *   **Updating a Foreign Key Attribute:** This can violate **referential integrity** if the new value does not exist as a primary key in the referenced table. It can also violate a domain constraint.