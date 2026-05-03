### **SQL in Real-World Programs**

So far in this course, we have likely used a **Generic Query Interface** (like a command-line terminal or a tool like MySQL Workbench). This is useful for:
*   Creating schemas and constraints.
*   Running occasional, one-off (ad hoc) queries to check data.

However, in the real world, users do not type SQL commands. Instead, they interact with **conventional programs** (mobile apps, websites, desktop software) that generate SQL queries behind the scenes.
- ### **The Three-Tier Client/Server Architecture**
  
  Most modern database applications follow a **Three-Tier Architecture**. This separates the user interface from the business logic and the data storage.
  
  1.  **The Client Tier (Presentation Layer):**
    *   This is what the user sees (e.g., a PC, mobile device, or web browser).
    *   It handles the Graphical User Interface (GUI) and displays information to the user.
    *   It sends user requests to the middle tier.
  
  2.  **The Application/Web Server Tier (Middle Tier):**
    *   This is the "brain" of the application.
    *   **Main Responsibilities:**
        *   **Business Logic:** Implements the rules of the business (e.g., "A student cannot register for more than 18 credit hours").
        *   **Security:** Checks credentials (login/password) before passing requests to the database.
        *   **Formatting:** Takes raw data from the database and formats it into dynamic web pages (HTML/CSS) for the client.
    *   It acts as an intermediary; the client never talks directly to the database.
  
  3.  **The Database Server Tier (Data Tier):**
    *   This is the DBMS (e.g., MySQL, Oracle, MongoDB).
    *   It is responsible solely for storing, accessing, and updating the data.
  
  **The Interaction Sequence:**
  1.  **Connect:** The application program must first establish (open) a connection to the database server.
  2.  **Interact:** The application submits queries, updates, and commands to the DB server.
  3.  **Disconnect:** When the task is done, the application terminates (closes) the connection to free up resources.
- ### **Modern Architectures: Monoliths vs. Microservices**
  
  *   **Monolithic Application (Traditional):**
    *   A single, large application handles everything (Web Apps, Mobile Apps).
    *   It typically connects to a **Single, Shared Database**.
    *   *Pros:* Simpler to develop initially.
    *   *Cons:* Hard to scale; if the database goes down, everything goes down.
  
  *   **Cloud-Native / Microservices Application (Modern):**
    *   The application is broken down into small, independent "services" (e.g., a Payment Service, a User Profile Service, an Inventory Service).
    *   **Database per Service:** Each microservice often owns its **own** data store.
    *   *Pros:* Highly scalable, better fault tolerance.
- ### **Approaches to Database Programming**
  
  There are several ways to bridge the gap between a programming language (like Python or C++) and SQL.
- #### **Method 1: Using an API (Application Programming Interface)**
  This is currently one of the most common approaches.
  
  *   **How it works:** The host programming language uses a **library** of pre-written functions (classes/objects) to talk to the database.
  *   **No Special Syntax:** The code looks like standard Python/Java/C# code. You pass SQL strings to these functions.
  *   **Example:** Using Python with the `sqlite3` library.
    ```python
    import sqlite3
    conn = sqlite3.connect('example.db') # Function to connect
    cursor = conn.execute('SELECT * FROM students') # Function to query
    ```
  *   The database developers usually provide these drivers/APIs.
- #### **Method 2: Embedded SQL**
  This is an older but still used technique, particularly in C, C++, and COBOL.
  
  *   **How it works:** Database commands (SQL) are written **directly** inside the general-purpose programming language code.
  *   **The "Precompiler" Requirement:** Since a C++ compiler doesn't understand SQL syntax, a special step called **precompilation** is required.
    1.  The Precompiler finds the SQL commands (often marked with `EXEC SQL`).
    2.  It converts them into function calls that the host language *can* understand.
    3.  The standard compiler then compiles the code.
  *   **Example Code:**
    ```c
    EXEC SQL
      SELECT Fname, Lname, Salary
      INTO :fname, :lname, :salary
      FROM EMPLOYEE WHERE Ssn = :ssn;
    ```
    *   *Note the use of variables (like `:ssn`) to pass data between the C program and the SQL query.*