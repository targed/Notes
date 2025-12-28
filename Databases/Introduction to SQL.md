#### **What is SQL?**

SQL is a comprehensive language for relational database management. While it is famous for querying data, its scope is much broader. It is a **declarative language**, meaning you specify *what* you want the database to do, and the DBMS figures out the most efficient way to do it.

SQL is composed of several sub-languages for different tasks:

*   **Data Definition Language (DDL):** For defining and managing the database schema (e.g., creating, altering, and dropping tables).
*   **Data Manipulation Language (DML):** For retrieving, inserting, deleting, and updating the data within the tables.
*   **Data Control Language (DCL):** For managing security, such as granting and revoking user permissions.
*   **Transaction Control Language (TCL):** For managing transactions to ensure data integrity.

SQL has evolved significantly since its creation in the 1970s and is standardized by ISO/IEC, with the most recent standard being SQL:2023.

Let's begin with the Data Definition Language (DDL), which is used to build the database structure.
- #### **The `CREATE TABLE` Command**
  
  This is the fundamental DDL command used to create a new relation (table) in the database.
  
  *   **Basic Syntax:**
      ```sql
      CREATE TABLE TableName (
          Column1 DataType1 [ColumnConstraint],
          Column2 DataType2 [ColumnConstraint],
          ...
          [TableConstraint1],
          [TableConstraint2]
      );
      ```
  *   **Components:**
      *   `TableName`: The name of the new table.
      *   `ColumnName`: The name of an attribute.
      *   `DataType`: The data type for the column's values.
      *   `ColumnConstraint`: An integrity rule that applies to a single column (e.g., `NOT NULL`).
      *   `TableConstraint`: An integrity rule that applies to one or more columns (e.g., a composite primary key).
  
  The full `CREATE TABLE` statements for the COMPANY database schema we will use to explore the different components.
- #### **Attribute Data Types and Domains in SQL**
  
  SQL supports a rich set of data types for defining columns.
  
  *   **Numeric Types:**
      *   `INTEGER` or `INT`: For whole numbers.
      *   `SMALLINT`: For smaller-range whole numbers.
      *   `DECIMAL(p, s)`: For exact numbers with a fixed precision (`p` total digits) and scale (`s` digits after the decimal point). Example: `DECIMAL(10, 2)` for currency.
      *   `FLOAT` or `REAL`: For floating-point (approximate) numbers.
  
  *   **Character-String Types:**
      *   `CHAR(n)`: A **fixed-length** string of `n` characters. If a shorter string is stored, it is padded with spaces.
      *   `VARCHAR(n)`: A **variable-length** string with a maximum length of `n`. It only uses as much storage as needed for the actual string.
  
  *   **Date and Time Types:**
      *   `DATE`: Stores year, month, and day in `'YYYY-MM-DD'` format.
      *   `TIMESTAMP`: Stores date and time, often with fractional seconds.
  
  *   **Domains:** SQL allows you to create a user-defined domain with the `CREATE DOMAIN` command. This lets you define a custom data type with a specific constraint that can be reused for multiple attributes, improving schema readability and maintainability. For example: `CREATE DOMAIN SSN_TYPE AS CHAR(9);`