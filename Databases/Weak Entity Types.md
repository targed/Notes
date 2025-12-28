### **Weak Entity Types**

So far, we have been discussing **strong entity types**. A strong entity is the "normal" type of entity that has its own unique key attribute that can be used to identify each instance.

A **weak entity type**, however, is an entity that **does not have a key attribute of its own**. Its instances cannot be uniquely identified by their attributes alone.
- #### **Key Characteristics of a Weak Entity**
  
  1.  **Dependence on an Owner:** A weak entity can only be identified by its relationship to a specific entity from another entity type. This related entity is called the **owner entity** or **identifying entity**.
  2.  **Identifying Relationship:** The relationship that associates the weak entity with its owner is called the **identifying relationship**.
  3.  **Partial Key:** While a weak entity doesn't have a full primary key, it often has a **partial key** (also called a **discriminator**). This is an attribute (or set of attributes) that can uniquely identify weak entities *that are related to the same owner entity*.
- #### **Example: `DEPENDENT`**
  
  The `COMPANY` database schema provides a classic example of a weak entity.
  *   The `DEPENDENT` entity stores information about an employee's dependents.
  *   **Problem:** Can we uniquely identify a dependent by their `Name`? No, because different employees might have dependents with the same name (e.g., two employees could each have a child named "Jane"). Therefore, `Name` alone cannot be a primary key.
  *   **Solution:** A `DEPENDENT` entity is only meaningful in its relationship to a specific `EMPLOYEE`. The full identity of a dependent is the combination of the owner `EMPLOYEE`'s key and the dependent's partial key.
    *   **Weak Entity Type:** `DEPENDENT`
    *   **Owner Entity Type:** `EMPLOYEE`
    *   **Identifying Relationship:** `DEPENDENTS_OF`
    *   **Partial Key:** `Name` (assuming no employee has two dependents with the exact same name).
- #### **Notation for Weak Entities in ER Diagrams**
  
  Weak entities have a special notation in ER diagrams to distinguish them from strong entities:
  
  *   The weak entity type itself is drawn as a **double-lined rectangle**.
  *   The identifying relationship is drawn as a **double-lined diamond**.
  *   The partial key is underlined with a **dashed line**.
- #### **Design Choices: Weak Entity vs. Complex Attribute**
  
  Sometimes, there's a design choice between modeling a concept as a weak entity or as a composite, multivalued attribute. For example, an employee's dependents could be modeled as a weak entity `DEPENDENT` or as a complex attribute `Dependents` on the `EMPLOYEE` entity.
  
  The choice often comes down to how the concept will be used.
  *   Model it as a **weak entity** if the concept has many of its own attributes or if it participates in relationships with other entities besides its owner. This is the more common and flexible approach.
  *   Model it as a **complex attribute** for simpler cases where the concept is just a collection of values with no other relationships.
  
  Ultimately, the choice is made by the **database designer** based on the requirements of the application.