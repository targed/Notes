### **Mapping Specialization/Generalization (Continued)**

Here are the last two options, both of which involve collapsing the hierarchy into a single table.
- ##### **Option 8C: Single Relation with One Type Attribute**
  
  *   **The Rule:** Create a **single relation** (table) that includes:
    1.  The primary key of the superclass.
    2.  All the attributes of the superclass.
    3.  All the specific attributes of *every* subclass.
    4.  A new **type attribute** is added to the table. This attribute indicates which subclass the tuple belongs to (e.g., a `Job_Type` column could hold values like 'Secretary', 'Engineer', etc.).
  *   **How it works:** All information for every entity is in one wide table. For any given tuple, the attributes that are not applicable to its type will have `NULL` values. For example, a tuple for an `ENGINEER` would have `NULL` in the `Typing_speed` column.
  *   **Best for:** This option works well for **disjoint** specializations. Because an entity can only belong to one subclass, a single type attribute is sufficient to identify it. It avoids the need for `JOIN` operations, which can be faster for queries. However, it can be inefficient in terms of storage if there are many subclasses with many specific attributes, leading to a large number of `NULL` values.
- ##### **Option 8D: Single Relation with Multiple Type Attributes**
  
  *   **The Rule:** This is a variation of Option 8C, again using a single relation. However, instead of one type attribute, it uses multiple **boolean flag attributes**—one for each subclass.
    *   Each flag attribute (e.g., `Is_Secretary`, `Is_Engineer`) indicates whether a tuple belongs to that specific subclass.
  *   **How it works:** Similar to Option 8C, it can lead to many `NULL` values in the specific attribute columns.
  *   **Best for:** This option is designed specifically for **overlapping** specializations. Since an entity can be a member of more than one subclass, multiple boolean flags can be set to `true` for a single tuple.
- #### **Summary of Mapping Options for Specialization**
  
  | **Option** | **Relations Created** | **Best For** | **Pros** | **Cons** |
  | :--- | :--- | :--- | :--- | :--- |
  | **8A** | Superclass + Each Subclass | All cases (most flexible) | No redundancy; handles all constraints. | Requires `JOIN`s to get full entity info. |
  | **8B** | Each Subclass Only | Total & Disjoint | No `JOIN`s needed; info is self-contained. | Data redundancy (shared attributes are repeated); cannot represent partial specializations. |
  | **8C** | Single Relation (with type attribute) | Disjoint specializations | No `JOIN`s needed. | Wasted space due to `NULL`s; cannot easily handle overlapping specializations. |
  | **8D** | Single Relation (with boolean flags) | Overlapping specializations | No `JOIN`s needed. | Wasted space due to `NULL`s; more complex type management. |
- ### **Mapping Union Types (Categories)**
  
  The final EER construct to map is the **UNION type**, or **category**. This is used when a subclass is a collection of entities from *different* superclasses that do not share the same primary key.
  
  *   **The Challenge:** You cannot use the standard superclass/subclass mapping because the parent entities (e.g., `PERSON`, `BANK`, `COMPANY` for the `OWNER` category) have different primary keys (`Ssn`, `Bname`, `Cname`). A single foreign key column wouldn't work.
  *   **The Rule:**
    1.  Create a new relation (table) for the category itself (e.g., `OWNER`).
    2.  The primary key for this new relation will be a new, artificially created key called a **surrogate key**. This key is usually a simple integer (`OwnerId`) whose only purpose is to uniquely identify an owner, regardless of whether it's a person, bank, or company.
    3.  Any relationship that the category participates in (like `OWNS` with `REGISTERED_VEHICLE`) will now use this surrogate key as the foreign key.
  *   **Example (`OWNER` category):**
    *   A new `OWNER` table is created.
    *   A new primary key, `OwnerId`, is created for this table.
    *   The `REGISTERED_VEHICLE` table would have a foreign key `OwnerId` that refers to the `OWNER` table.
    *   To find out the details of the owner, you would first look up the `OwnerId` in the `OWNER` table, which would likely have a `OwnerType` attribute and a foreign key pointing to the appropriate superclass table (`PERSON`, `BANK`, or `COMPANY`).