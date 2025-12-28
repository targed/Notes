### **Lecture 3: The Role of the ER Model in Database Design**

This lecture dives into **conceptual design**, which is the critical first step in creating a blueprint for a database.
- #### **Recap: The Database Design Process**
  
  As a quick refresher from the previous lectures, the design of a new database follows a structured process:
  
  1.  **Requirements Collection and Analysis:** Designers interview users and stakeholders to understand the data requirements (what information needs to be stored) and the functional requirements (what operations will be performed on the data). This phase defines the "miniworld" the database will represent.
  2.  **Conceptual Design:** The requirements are translated into a high-level, DBMS-independent description of the data. This is where the **Entity-Relationship (ER) Model** is used to create a conceptual schema. The goal is to create a model that is easy to understand and can be discussed with non-technical users.
  3.  **Logical Design (Data Model Mapping):** The conceptual schema (the ER diagram) is mapped to an implementation data model (e.g., the relational model). This results in a set of tables and constraints that can be implemented in a specific DBMS.
  4.  **Physical Design:** The final phase, where low-level decisions about file organization, indexing, and storage are made to ensure performance.
- #### **The COMPANY Database: A Sample Application**
  
  Throughout this lecture, a running example of a **COMPANY database** will be used to illustrate the ER modeling concepts. The requirements for this database are:
  *   The company is organized into **departments**.
  *   Each department has a unique name, a unique number, and may have several locations.
  *   A department controls a number of **projects**.
  *   The database stores information about **employees**, including their salary, supervisor, and personal details.
  *   Each employee works for one department but can work on several projects.
  *   The database must keep track of the **dependents** of each employee.
- ### **Core ER Model Concepts: Entities and Attributes**
  
  The ER model describes the world in terms of three core concepts: entities, attributes, and relationships.
- #### **Entities**
  
  An **entity** is a "thing" or object in the real world with an independent existence that can be distinctly identified.
  *   **Physical Existence:** A person, a car, a specific employee.
  *   **Conceptual Existence:** A company, a job, a university course.
  
  In an ER diagram, an **entity set** (a collection of similar entities, like all employees) is represented by a **rectangle**.
- #### **Attributes**
  
  Each entity is described by a set of **attributes**, which are the properties or characteristics of that entity. For example, an `EMPLOYEE` entity could have attributes like `Name`, `Age`, `Address`, and `Salary`.
  
  *   In an ER diagram, attributes are represented by **ovals** connected to their respective entity rectangle.
  *   A specific entity (like one particular employee) will have a **value** for each of its attributes (e.g., Name = "John Smith", Age = 55).
- #### **Types of Attributes**
  
  Attributes are not all the same; they can be classified into different types based on their structure and characteristics.
  
  1.  **Simple (Atomic) vs. Composite Attributes**
    *   **Simple Attributes:** Are not divisible into smaller parts. Examples: `ID`, `Gender`, `Age`.
    *   **Composite Attributes:** Can be divided into smaller sub-parts that represent more basic attributes. For example, the `Address` attribute can be a composite of `Street_address`, `City`, `State`, and `Zip`. This is useful when you need to query a specific part of the attribute, like finding all employees in a particular city.
  
  2.  **Single-Valued vs. Multivalued Attributes**
    *   **Single-Valued Attributes:** An attribute that holds a single value for a particular entity. For most entities, `Age` or `Birth_date` would be single-valued.
    *   **Multivalued Attributes:** An attribute that can hold multiple values for a single entity. For example, a `Car` entity might have a `Colors` attribute, which could hold multiple values if the car is two-toned. A `STUDENT` entity might have multiple `Phone` numbers. In an ER diagram, these are represented by a **double-lined oval**.
  
  3.  **Stored vs. Derived Attributes**
    *   **Stored Attributes:** These are the base attributes that are physically stored in the database (e.g., `birth_date`).
    *   **Derived Attributes:** An attribute whose value can be calculated or derived from another stored attribute. For example, an `Age` attribute can be derived from the `birth_date` and the current date. Similarly, the `number_of_employees` in a `DEPARTMENT` can be derived by counting how many employees are related to that department. Derived attributes are shown in a **dashed oval** on an ER diagram.
  
  4.  **Complex Attributes**
    *   These are attributes that are both multivalued and composite. They can be nested to represent complex data. For example, a `STUDENT` might have a multivalued attribute called `PreviousDegrees`. Each degree is a composite attribute consisting of `College`, `Year`, `Degree`, and `Field`.