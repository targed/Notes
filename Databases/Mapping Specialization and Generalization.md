### **EER-to-Relational Mapping Algorithm**
- #### **Step 7: Mapping Specialization and Generalization**
  
  Mapping a superclass/subclass hierarchy from an EER diagram to relational tables is a significant design choice. There is no single "correct" way; instead, there are several options, and the best choice depends on the specific constraints of the specialization (disjoint/overlap, total/partial) and the query patterns of the application.
  
  The lecture presents four main strategies:
  
  ---
- #### **Option A: Multiple Relations – Superclass and Subclasses**
  
  This is the most general and flexible approach.
  
  *   **Rule:** Create a separate relation (table) for the superclass and a separate relation for *each* of its subclasses.
    *   The **superclass relation** contains the primary key and all the shared attributes.
    *   Each **subclass relation** contains only the primary key of the superclass (which acts as both its own primary key and as a foreign key referencing the superclass table) and the specific, local attributes of that subclass.
  *   **How it Works:** To retrieve all information for a specific subclass entity (e.g., a `TECHNICIAN`), you would need to `JOIN` the superclass table (`EMPLOYEE`) with the subclass table (`TECHNICIAN`) using their shared primary key.
  *   **Best For:** This is the only option that works for **any** type of specialization, whether it's disjoint or overlapping, total or partial. It is often the default choice.
  
  **Example:**
  *   `EMPLOYEE`(`<u>Ssn</u>`, Fname, Minit, Lname, Birth_date, Address, Job_type)
  *   `SECRETARY`(`<u>Ssn</u>`, Typing_speed)
  *   `TECHNICIAN`(`<u>Ssn</u>`, Tgrade)
  *   `ENGINEER`(`<u>Ssn</u>`, Eng_type)
  
  ---
- #### **Option B: Multiple Relations – Subclass Relations Only**
  
  This approach eliminates the superclass table.
  
  *   **Rule:** Create a separate relation for *each subclass only*. The attributes of the superclass are "pushed down" or integrated into each subclass relation.
    *   Each subclass relation contains all the shared attributes from the superclass plus its own local attributes. The primary key of the superclass becomes the primary key for each subclass relation.
  *   **How it Works:** To find an entity, you know exactly which table to look in. However, to find all `VEHICLE`s, you would need to query both the `CAR` and `TRUCK` tables and combine the results using a `UNION` operation.
  *   **Best For:** This option works well when the specialization is **total and disjoint**. "Total" means there are no entities that are *only* in the superclass, so you don't lose any data by eliminating the superclass table.
  
  **Example:**
  *   `CAR`(`<u>Vehicle_id</u>`, License_plate_no, Price, Max_speed, No_of_passengers)
  *   `TRUCK`(`<u>Vehicle_id</u>`, License_plate_no, Price, No_of_axles, Tonnage)
  
  ---
- #### **Option C: Single Relation with One Type Attribute**
  
  This approach consolidates the entire hierarchy into one large table.
  
  *   **Rule:** Create a single relation that contains the primary key, all the attributes of the superclass, *and* all the attributes of *all* the subclasses.
    *   A special **type attribute** (or discriminating attribute) is added to the table. The value in this column indicates which subclass the tuple belongs to (e.g., 'S' for Secretary, 'E' for Engineer).
  *   **Problem:** This approach can lead to a large number of `NULL` values. For a tuple representing a `SECRETARY`, the attributes `Tgrade` and `Eng_type` (which belong to other subclasses) will be `NULL`.
  *   **Best For:** This option works only for specializations that are **disjoint**. You cannot use it for overlapping subclasses because the single type attribute can only hold one value.
  
  **Example:**
  *   `EMPLOYEE`(`<u>Ssn</u>`, Fname, ..., Job_type, Typing_speed, Tgrade, Eng_type)
  
  ---
- #### **Option D: Single Relation with Multiple Type Attributes**
  
  This is a variation of the single-relation approach, designed for overlapping specializations.
  
  *   **Rule:** Create a single relation with all attributes of the superclass and all subclasses, similar to Option C.
    *   Instead of one type attribute, add multiple **boolean-valued** type fields, one for each subclass. A `TRUE` value in a column indicates that the tuple is a member of that subclass.
  *   **How it Works:** A tuple can have `TRUE` in multiple boolean columns, which allows it to belong to overlapping subclasses.
  *   **Problem:** This can also lead to many `NULL` values for the non-applicable subclass attributes.
  *   **Best For:** This option is used for **overlapping** specializations. Technically it can also work for disjoint specializations (where only one boolean flag would be true for any given tuple).
  
  **Example:**
  *   `PART`(`<u>Part_no</u>`, Description, Mflag, Drawing_no, ..., Pflag, Supplier_name, ...)