### **Basic Definitions: Data and Databases**

To understand a database, we must first define its core components.

*   **Data:** At its most basic level, data consists of known facts that can be recorded and have an implicit meaning. It is raw and unorganized.
*   **Examples of Data:** The name of a person ("Victoria Slater"), a student ID number (34933), or the number of credits a student has completed (36).
*   **What isn't easily defined as data?** An interesting question about "precise emotions of a human." This highlights a key challenge: data is most useful in a database when it is concrete and can be clearly recorded. Subjective or abstract concepts are difficult to structure.

*   **Database:** A database is not just any random collection of data; it is a **collection of related data**. This relationship between the data is what gives it structure and meaning. As shown in the presentation, databases can store many types of data, including:
*   **Text/Numerical Data:** Such as student records in a table.
*   **Multimedia Data:** Images, videos, and audio files.
*   **Geospatial Information Systems (GIS) Data:** Maps and geographical data.
*   **Social Media Data:** Posts, user connections, and interactions.
- ### **Key Properties of a Database**
  
  A true database has several "implicit properties" that distinguish it from a simple collection of files.
  
  1.  **Represents a "Miniworld":** A database is a model of some aspect of the real world, often called a "miniworld" or "universe of discourse." For instance, the example database represents the miniworld of a university's course registration system. This miniworld is dynamic; it changes as students enroll, courses are added, and grades are assigned.
  
  2.  **Logically Coherent:** The data within a database is internally consistent and connected in a meaningful way. A random assortment of data, like a grocery list and a list of car parts, would not be considered a database because the items have no inherent relationship. In the university example, the `GRADE_REPORT` table is coherent because it links a `Student_number` from the `STUDENT` table to a `Section_identifier` from the `SECTION` table.
  
  3.  **Designed for a Specific Purpose:** A database is created to serve a particular application or set of users. The university database, for example, is designed for students, faculty, and administrators to manage registration, grades, and course information. It has an intended group of users and a clear purpose.
- ### **An Example: A University Database**
  
  This collection of tables and records illustrates the properties discussed above:
  
  *   It represents the **miniworld** of a university.
  *   The data is **logically coherent**. For example, the `Prerequisite_number` `CS1310` for the course `CS3320` corresponds to the "Intro to Computer Science" course in the `COURSE` table.
  *   It is designed for the **specific purpose** of managing academic records.
  
  This structured set of "data records" is far more powerful than a simple list because the relationships between students, courses, sections, and grades are clearly defined.