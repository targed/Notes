### **Key Attributes of an Entity Type**

A fundamental requirement for a database is the ability to distinguish one entity from another. We can't have two employees who are completely indistinguishable. This is where key attributes come in.
- #### **Uniqueness Constraint (Key)**
  
  A **key** is an attribute, or a set of attributes, whose values are guaranteed to be unique for each entity in an entity set. This uniqueness constraint ensures that no two entities have the same value for their key attribute(s).
  
  *   **Notation:** In an ER diagram, a key attribute is shown with its name **underlined** inside the oval.
  *   **Example:** For the `Students` entity set, the `ID` attribute is a key because no two students have the same ID. Similarly, `SSN` (Social Security Number) would also be a key.
  
  An entity type can have **more than one key**. For example, `Students` could have both `ID` and `SSN` as potential keys. Each of these is called a **candidate key**, and the database designer chooses one to be the **primary key**.
- #### **Composite Keys**
  
  Sometimes, a single attribute is not enough to guarantee uniqueness. In these cases, a combination of several attributes can be used as a key.
  
  *   A **composite key** is a key that is composed of two or more attributes. The *combination* of these attribute values must be unique for each entity.
  *   **Example:** Consider a `CAR` entity. The `Vehicle_Id` (VIN) would be a simple key. However, if we didn't have a VIN, we might use the combination of (`State`, `Number`) from the car's `Registration` to uniquely identify it. No two cars in the same state can have the same license plate number.
  *   **Minimality:** A composite key must be **minimal**. This means you cannot remove any attribute from the key and still have it guarantee uniqueness. For example, if (`State`, `Number`, `issue_date`) uniquely identifies a registration, but (`State`, `Number`) *also* uniquely identifies it, then the three-attribute key is not minimal. The minimal key is (`State`, `Number`).
- #### **What if an Entity Type has No Key?**
  
  "Can an entity type have no key?". The answer is yes, and this leads to the concept of **weak entities**, which we will cover in a later section. A weak entity cannot be uniquely identified on its own and depends on another entity for its identity.