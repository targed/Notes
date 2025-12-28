- **1. Why Normalize?**
  The primary motivation for normalization is to organize data to reduce redundancy. As the memes on Slide 2 suggest, un-normalized databases are full of redundant data, which leads to maintenance nightmares and data inconsistency.
  
  **2. Design Methodologies**
  There are two main approaches to designing a database schema:
  *   **Bottom-up Design:** You start with individual attributes (e.g., "Student Name", "Course ID", "Grade") and look for relationships between them to form tables. Normalization uses this approach.
  *   **Top-down Design:** You start by grouping attributes into entities based on real-world concepts (e.g., "Student", "Course") and then refine/decompose those relations. ER Modeling uses this approach.
  
  **3. The Two Main Goals**
  Regardless of the method, a "good" design must achieve:
  *   **Information Preservation:** When you break larger tables into smaller ones (decomposition), you must ensure no information is lost. You must be able to reconstruct the original information perfectly using Joins.
  *   **Minimum Redundancy:** The same piece of information should not be stored in multiple places (unless it's a foreign key). Redundancy leads to inconsistency—if data is stored in two places and you update only one, the database becomes "broken."
  
  ---
- ### **Informal Design Guidelines (Anomalies)**
  
  Before using math to prove a design is good, we can use "Informal Guidelines." The most important of these is **Guideline 1: Clear Semantics** and **Guideline 2: Reduced Redundancy**.
  
  **1. Guideline 1: Clear Semantics**
  *   **Rule:** A table should represent *one* entity or concept. Do not combine attributes from multiple disparate entities into a single table just to save space.
  *   **Bad Example:** A single table `Band_Songs` that stores `Band`, `Album`, `Song`, and `Release Year`. This mixes the concept of a "Band" with the concept of a "Song."
  
  **2. Guideline 2: Reduced Redundancy & Anomalies**
  If a table is poorly designed (has redundancy), it suffers from **Update Anomalies**. Using the example `EMP_DEPT(Ename, SSN, Address, Dnumber, Dname, Dmgr_ssn)`:
  
  *   **Insertion Anomalies:**
    *   *Problem:* To insert a new employee, we must also provide the Department Name and Manager. This is redundant.
    *   *Problem:* We cannot create a new Department (e.g., "Sales") unless we have at least one employee assigned to it, because the primary key likely involves the Employee SSN. If there's no employee, we can't create the department row.
  
  *   **Deletion Anomalies:**
    *   *Problem:* If we delete the *last* employee working for the "Research" department, we inadvertently delete all information about the "Research" department (its name and manager) from the database entirely. We lost the department just because we fired the employee.
  
  *   **Modification Anomalies:**
    *   *Problem:* If the manager of the "Research" department changes, we cannot just update one row. We must find *every single employee* who works in Research and update the manager field in their rows. If we miss one, the database becomes inconsistent.
  
  ---
- ### **NULLs and Spurious Tuples**
  
  **1. Guideline 3: Reduced NULL Values**
  *   **Rule:** Design tables so that columns generally have values.
  *   **Why?** `NULL` values are problematic because:
    *   They waste storage space.
    *   They create ambiguity (Does `NULL` mean "Not Applicable", "Unknown", or "Missing"?).
    *   They complicate aggregation functions (like `COUNT` or `AVG`).
  *   *Example:* If you have a `Musician` table with a `Band` column, and solo artists have `NULL` for Band, it's better to create a separate table for Band memberships.
  
  **2. Guideline 4: Disallow Spurious Tuples**
  *   **The Danger:** If you break a table into two smaller tables but do not include a proper linking attribute (Primary Key/Foreign Key) that guarantees unique matches, joining them back together will create **Spurious Tuples**.
  *   **Definition:** Spurious tuples are fake/invalid rows created when joining tables on non-unique attributes.
  *   **Example:**
    *   Table A has `Student` and `Department`. Table B has `Department` and `Course`.
    *   If you join them on `Department`, you might match Student "Smith" (CS Dept) with Course "CS 330" (CS Dept), even if Smith never took CS 330. The database thinks the connection exists just because they share a department.
  *   **Goal:** Ensure decompositions can be joined via **Lossless (Non-additive) Joins** on Primary/Foreign keys.