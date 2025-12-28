### **EER-to-Relational Mapping Algorithm (Continued)**
- #### **Step 8: Mapping of Shared Subclasses (Multiple Inheritance)**
  
  A shared subclass is an entity type that inherits from more than one superclass, representing a **lattice** or **multiple inheritance** structure.
  
  *   **Rule:** To map a shared subclass (e.g., `Alicorn` which is a subclass of both `Unicorn` and `Pegasus`), you create a new relation for the shared subclass itself.
    *   **Attributes:**
        *   The attributes of the shared subclass will include any of its **local attributes** (e.g., `SpecialAbility`).
        *   It will also include the **primary keys** of *all* of its superclasses. These will serve as **foreign keys** referencing each superclass table.
    *   **Primary Key:** The primary key of the shared subclass relation is typically the primary key inherited from one of the superclasses, or a new surrogate key if necessary. In the `Alicorn` example, `AlicornID` could be its own key, or it could just be the `UnicornID` (assuming every Alicorn must first be a Unicorn).
  
  *   **General Applicability:** The four options (A, B, C, D) discussed in Step 7 for single inheritance can also be adapted to handle multiple inheritance, though the "multiple relations" approach (similar to Option A) is the most straightforward and common.
  
  **Example:**
  Using the `ENGINEERING_MANAGER` example from a previous lecture, which inherits from `SALARIED_EMPLOYEE` and `MANAGER`:
  1.  Map the superclasses (`SALARIED_EMPLOYEE`, `MANAGER`) using one of the previous methods.
  2.  Create a new relation for the shared subclass:
    `ENGINEERING_MANAGER`
  3.  Include foreign keys to all its parents. The primary key of both superclasses is `Ssn`. So we add `Ssn` as the primary key and foreign key.
  4.  Add any local attributes.
  
  The schema would look something like this:
  *   `SALARIED_EMPLOYEE`(`<u>Ssn</u>`, Salary)
  *   `MANAGER`(`<u>Ssn</u>`, ...)
  *   `ENGINEERING_MANAGER`(`<u>Ssn</u>`)
    *   Here, `ENGINEERING_MANAGER.Ssn` would be a foreign key to `SALARIED_EMPLOYEE.Ssn` and `MANAGER.Ssn`.
  
  ---
- #### **Step 9: Mapping of Categories (UNION Types)**
  
  A **category** is a subclass that represents the **union** of two or more distinct superclasses, which may have different primary keys.
  
  *   **Rule:** Create a new relation for the category itself.
    *   **Primary Key:** The key for a category is a challenge because its members come from different tables with potentially different primary keys (e.g., `Ssn` for `PERSON`, `Bname` for `BANK`). Therefore, it is standard practice to create a new, artificial key, known as a **surrogate key**, for the category relation. Let's call it `Owner_id`. This surrogate key will serve as the primary key of the category table.
    *   **Foreign Keys:** The category relation will hold the foreign keys of the records it is referencing. However, since an `OWNER` can be from any of the superclasses, most of these foreign key columns would be `NULL` for any given tuple. A more common approach is to add the surrogate key of the category (`Owner_id`) as a foreign key *back to each of the superclass tables*.
  *   **Relationship Mapping:** Any relationship in which the category participates is mapped using the surrogate primary key of the category's table.
  
  **Example:**
  Mapping the `OWNER` category, which is a union of `PERSON`, `COMPANY`, and `BANK`.
  1.  Create a relation for the category: `OWNER`.
  2.  Create a new surrogate primary key for it: `Owner_id`. So, **OWNER**(`<u>Owner_id</u>`).
  3.  Add this new `Owner_id` as a foreign key to each of the superclass tables:
    *   `PERSON`(`<u>Ssn</u>`, ..., Owner_id)
    *   `BANK`(`<u>Bname</u>`, ..., Owner_id)
    *   `COMPANY`(`<u>Cname</u>`, ..., Owner_id)
  4.  Map the `OWNS` relationship, which involves the `OWNER` category. This is an M:N relationship, so we create a new table for it. The primary key of the `OWNER` side of this relationship will be the surrogate key, `Owner_id`.
    *   `OWNS`(`<u>Owner_id</u>`, `<u>Vehicle_id</u>`, Purchase_date, ...)
  
  This approach correctly captures the union relationship while maintaining a consistent way to reference an "owner" regardless of its specific type.