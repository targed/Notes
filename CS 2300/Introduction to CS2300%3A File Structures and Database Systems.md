### **Introduction to CS2300: File Structures and Database Systems**
title:: Introduction to CS2300: File Structures and Database Systems

This course, CS2300, is an introduction to file structures and database systems. The instructor is San Yeung, an Assistant Teaching Professor at the Missouri University of Science and Technology (Missouri S&T).

A key component of the course is a database project that requires students to build a user interface (UI). While a basic command-line UI is standard, there's an opportunity to earn bonus credit by developing a more advanced Graphical User Interface (GUI).
- ### **Core Question: Database Systems vs. Information Retrieval Systems**
  
  A fundamental concept introduced early on is the distinction between a **database system** and an **information retrieval (IR) system**. While they both manage and retrieve data, they are designed for different purposes and handle data in fundamentally different ways.
  
  A comparison:
  
  | **Database Systems** | **Information Retrieval (IR) Systems** |
  | :--- | :--- |
  | **Data Type** | **Structured Data**: This data adheres to a predefined model and is often stored in tables with rows and columns. Think of a spreadsheet of student information with columns for Student ID, Name, and GPA. | **Unstructured Data**: This data has no predefined format. It includes things like the text of a web page, audio files, videos, and social media posts. |
  | **Schema** | **Schema-Driven**: A database is built around a "schema," which is a formal blueprint that defines the tables, fields, and relationships between data. This structure is defined *before* any data is entered. | **No Fixed Schema**: IR systems do not require a predefined structure. This allows them to quickly collect and store vast amounts of varied data in its native format. |
  | **Query Model** | **Structured Query Language (SQL)**: Uses a formal, structured language for requests. SQL queries are precise and unambiguous. | **Free-form & Natural Language**: Queries are often keywords or questions, similar to how you would use a web search engine. The system tries to understand the *intent* behind the query. |
  | **Matching** | **Exact Matching**: The system retrieves data that exactly matches the precise conditions laid out in the query. The result is always considered correct and complete based on the query logic. | **Approximate Matching**: The system returns results that are *likely* relevant to the query. Results are often ranked by a relevance score, as there may not be a single "correct" answer. |
  | **Example Query** | "Find the most expensive pair of shoes in the SHOES category." | "What is the current fashion trend for shoes in 2025?" |
  | **Typical Use Case**| E-commerce inventory management, banking systems, student registration systems. | Web search engines, document search within a library, digital assistants. |
  
  In summary, you use a **database system** when you need to store well-defined pieces of information and retrieve them with 100% accuracy. You use an **information retrieval system** when you need to search through a vast collection of unstructured documents to find the most relevant information for a less precise query.