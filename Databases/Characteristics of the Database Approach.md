### **The Database Approach vs. Traditional File Processing**

Before diving into the characteristics, it's helpful to understand what the database approach replaced. The older way was **traditional file processing**.

In that model, each application was responsible for defining and managing its own data files. The data structure (like the size of a field or the order of data) was hard-coded directly into the application program. This led to several problems:
*   **Data Redundancy:** The same information (like a student's name and address) might be stored in multiple different files for different applications (one for the registrar, one for the bursar's office), leading to wasted space and inconsistency.
*   **Data-Program Dependence:** If you needed to change the structure of a data file (e.g., increase the length of the 'Last Name' field), you had to find and rewrite every single program that accessed that file. This made maintenance a nightmare.

The database approach was developed to solve these problems. Its key characteristics are as follows:
- ### **1. Self-Describing Nature of a Database System**
  
  A fundamental feature of a database system is that it stores not only the user data but also a description of that data. This description is called **meta-data** (data about data).
  
  This meta-data is stored in a **DBMS catalog** or **data dictionary**. The catalog contains information such as:
  *   The names of the tables in the database (e.g., `STUDENT`, `COURSE`).
  *   The names and data types of the columns in each table (e.g., `Name` is a `Character(30)`).
  *   Constraints on the data (e.g., `Student_number` must be unique).
  *   Relationships between tables.
  
  Because the database contains its own definition, it is **self-describing**. This allows the DBMS and other programs to understand the structure of the data without it being hard-coded into an application. A perfect example of this is where the tables on the left contain the actual student data, and the tables on the right (the catalog) describe the structure of that data.
- ### **2. Insulation Between Programs and Data (Data Abstraction)**
  
  This is a direct solution to the data-program dependence problem of file processing. The database approach creates a separation between how data is stored and how it is accessed by applications. This is achieved through two key types of independence:
  
  *   **Program-Data Independence:** This allows the structure of the physical data files to be changed without impacting the application programs that use the data. For example, the database administrator could decide to store the `STUDENT` table in a different file format to improve performance. As long as the logical representation of the table remains the same, the student registration application does not need to be rewritten.
  
  *   **Data Abstraction:** The DBMS hides the complex details of physical data storage from most users. Users interact with a simplified, logical view of the data. For example, an application developer works with concepts like "tables" and "columns," not with the low-level details of how those tables are stored on a disk.
- ### **3. Support for Multiple Views of the Data**
  
  A database can be accessed by many different users and applications, each with its own needs. The database approach allows for the creation of **views** to cater to these different requirements.
  
  A **view** is a subset of the database or a virtual table that is derived from the base data. For example:
  *   **View 1 (Student Transcript):** This view could be for a student and combines data from the `STUDENT`, `COURSE`, `SECTION`, and `GRADE_REPORT` tables into a single, easy-to-read transcript. The student doesn't need to know how the underlying tables are structured.
  *   **View 2 (Course Prerequisites):** This view could be for an advisor, showing which courses are prerequisites for others.
  
  Views are powerful because they:
  *   Simplify data access for users.
  *   Provide a level of security by hiding data that a particular user should not see.
  *   Are typically virtual, meaning they are generated when requested and always show the most up-to-date data.
- ### **4. Sharing of Data and Multiuser Transaction Processing**
  
  Databases are inherently designed to be shared. A **multiuser DBMS** must allow multiple users to access and update the database simultaneously without corrupting the data. This is managed through **concurrency-control** software.
  
  This is a fundamental requirement for **Online Transaction Processing (OLTP)** systems, such as airline reservation systems, banking systems, and e-commerce sites.
  
  To ensure data integrity during concurrent access, the DBMS guarantees key properties for transactions:
  *   **Isolation:** Ensures that transactions do not interfere with each other. From each user's perspective, it appears as though they are the only one using the system.
  *   **Atomicity:** Guarantees that a transaction is an "all or nothing" operation. If a transaction involves multiple steps (e.g., withdrawing money from one account and depositing it into another), either all steps are completed successfully, or none of them are. This prevents the database from ending up in an inconsistent state.