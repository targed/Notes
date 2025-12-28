### **The Three-Schema Architecture**

The Three-Schema Architecture was proposed to formalize the database design process and to achieve the key characteristics of the database approach we discussed in the first lecture:
1.  **Self-describing nature** (via a catalog).
2.  **Insulation between programs and data** (program-data independence).
3.  **Support for multiple user views**.

The main goal of this architecture is to **separate the user applications from the physical database**. It achieves this by defining the database at three distinct levels, or schemas. A **schema** is a description of a portion of the database using a specific data model.

Here are the three levels of the architecture, from the lowest level of abstraction to the highest:
- #### **1. The Internal Level (or Physical Schema)**
  
  *   **Purpose:** Describes the physical storage structure of the database.
  *   **What it contains:** The **internal schema** contains all the low-level details of how the data is stored on disk. This includes record formats, file organization, storage allocation, and the access paths (e.g., indexes) used for fast data retrieval.
  *   **Data Model Used:** This level uses a **physical data model**.
  *   **Audience:** DBMS implementers and advanced database administrators. Most users are completely unaware of this level.
- #### **2. The Conceptual Level (or Logical Schema)**
  
  *   **Purpose:** Provides a unified, logical view of the entire database.
  *   **What it contains:** The **conceptual schema** describes the structure of the whole database for a community of users. It details all the entities, their attributes, their relationships, and the constraints on the data. However, it hides the details of the physical storage structures. It represents the "what" of the database, not the "how."
  *   **Data Model Used:** This schema is designed using an **implementation (logical) data model** (like the relational model) and is often derived from a **conceptual data model** (like an E-R diagram).
  *   **Audience:** Database designers, developers, and administrators.
- #### **3. The External Level (or View Level)**
  
  *   **Purpose:** Describes the part of the database that a particular user group is interested in, while hiding the rest.
  *   **What it contains:** This level can have multiple **external schemas** (also called user views). Each external schema describes the database from the perspective of a specific group of users. For example, a student might see a view of their transcript, while an accountant might see a view related to student billing.
  *   **Data Model Used:** Each view is typically implemented using an **implementation (logical) data model**.
  *   **Audience:** End-users and application programs.
- ### **Mappings and Independence**
  
  The Three-Schema Architecture relies on **mappings** to connect the different levels. These mappings tell the DBMS how to translate a request at a higher level into an operation at a lower level.
  
  *   **External/Conceptual Mapping:** Connects each external view to the conceptual schema. If the structure of the conceptual schema changes (e.g., a table is split into two), only this mapping needs to be updated, and the external views (and the applications that use them) can remain unchanged. This provides **Logical Data Independence**.
  
  *   **Conceptual/Internal Mapping:** Connects the conceptual schema to the internal schema. If the physical storage of the database is changed (e.g., a new index is added or the file system is reorganized), only this mapping needs to be updated. The conceptual schema and all the external views remain the same. This provides **Physical Data Independence**.
  
  This separation and mapping are what give the database approach its flexibility and robustness, allowing the database to evolve at one level without forcing massive changes at other levels.