## I. Authentication (AuthN): "Who are you?"
**Definition:** The process of verifying a user's identity.
*   *The Problem:* In a digital world, Bob cannot "see" Alice. He needs proof.
*   *The Three Factors:* Authentication relies on three fundamental categories (or a combination of them).
- ### 1. Something you KNOW
  *   **Examples:** Passwords, PINs, Security Questions ("What is your mother's maiden name?").
  *   **Pros:** Cheap, easy to implement, no special hardware required.
  *   **Cons:**
    *   **Entropy issues:** Humans pick bad passwords (`123456`, `password`).
    *   **Reuse:** If a user reuses a password and one site is hacked, all their accounts are compromised.
    *   **Sniffing:** If sent over an unencrypted network (HTTP), "Eve" can read it.
  *   **Mitigation:**
    *   **OTP (One-Time Password):** A code that is valid for only one login session. Even if Eve sniffs it, she cannot use it again.
- ### 2. Something you HAVE
  *   **Examples:** Physical tokens (RSA SecurID fob), Smart Cards (CAC cards), your Smartphone (for SMS codes), an ATM card.
  *   **Mechanism:** These devices usually generate a code based on a shared secret key and the current time (Time-based One-Time Password, or TOTP).
  *   **Pros:** Harder to steal than a password (requires physical theft).
  *   **Cons:** Hardware costs money; users lose tokens.
- ### 3. Something you ARE
  *   **Examples:** Biometrics (Fingerprint, Retina scan, FaceID, Voice recognition, Typing rhythm).
  *   **Pros:** You cannot lose your fingerprint; very hard to forge.
  *   **Cons:**
    *   **Privacy:** You cannot "change" your fingerprint if the database is hacked.
    *   **Accuracy Errors (Crucial Concept):**
        *   **False Positive (Type I Error):** The system grants access to an impostor. (Bad for Security).
        *   **False Negative (Type II Error):** The system denies access to the real user. (Bad for Usability/Availability).
        *   *Trade-off:* Tuning a system to have *zero* False Positives usually drives False Negatives up, making the system annoying to use.
- ### Multi-Factor Authentication (MFA/2FA)
  *   **Concept:** Combining two *different* categories.
  *   **Valid 2FA:** ATM Card (Have) + PIN (Know).
  *   **Invalid 2FA:** Password (Know) + Security Question (Know). *This is just two steps of the same factor.*
- ### Authentication Direction
  *   **Client Auth:** Server checks Client (e.g., Corporate VPN checks employee).
  *   **Server Auth:** Client checks Server (e.g., Your browser checks amazon.com's certificate to ensure it isn't a fake site).
  *   **Mutual Auth:** Both check each other.
  
  ---
- ## II. Authorization (AuthZ): "What can you do?"
  **Definition:** Once identity is proven (AuthN), the system checks if that identity has permission to perform an action (AuthZ).
- ### The Access Control Triad
  Access is defined by a "Three-Tuple": **< User, Resource, Privilege >**
  *   **User (Subject):** Alice.
  *   **Resource (Object):** `file.txt`, `/home/alice`, Database Table.
  *   **Privilege (Action):** Read, Write, Execute, Delete.
- ### Access Control Lists (ACLs)
  *   A list attached to a resource (like a file) saying who can do what.
  *   *Example:*
    *   File: `Report.pdf`
    *   ACL: `[Alice: Read/Write], [Bob: Read-Only]`
  
  ---
- ## III. Access Control Models
  There are three major philosophies on how to manage these permissions.
- ### 1. Discretionary Access Control (DAC)
  *   **Philosophy:** The **Owner** decides.
  *   **Example:** Windows/Linux file systems. If Alice creates a file, she owns it. She can grant Bob read access. She can block Eve. The system administrator does not micro-manage every file.
  *   **Weakness:** Trojan Horses. If Alice runs a malicious program, that program runs with her "discretion" and can change permissions on her files without her realizing it.
- ### 2. Mandatory Access Control (MAC)
  *   **Philosophy:** The **System** decides (based on rigid rules).
  *   **Context:** Military/High-Security.
  *   **Mechanism:** Users have "Clearances" (e.g., Secret). Files have "Labels" (e.g., Secret). The system compares Clearance vs. Label. Alice *cannot* change the label of a file even if she created it.
- ### 3. Role-Based Access Control (RBAC)
  *   **Philosophy:** Your **Job Function** decides.
  *   **Context:** Corporate environments.
  *   **Mechanism:**
    *   Alice is assigned the role "Manager."
    *   The "Manager" role has access to `Payroll_DB`.
    *   Alice gets access because she is a Manager.
  *   *Benefit:* If Alice moves to Engineering, IT just changes her role to "Engineer," and she automatically loses "Manager" access and gains "Engineer" access.
  
  ---
- ## IV. The Bell-LaPadula Model
  This is the most famous **Mandatory Access Control (MAC)** model. It focuses strictly on **Confidentiality** (keeping secrets secret).
  
  **The Classifications:** Top Secret > Secret > Confidential > Unclassified.
  
  **The Two Rules (The Arrows in the Diagram):**
  1.  **Simple Security Property (No Read Up):**
    *   A user at "Secret" clearance cannot read a document labeled "Top Secret." (Obvious rule).
  2.  **The *-Property (Star Property) (No Write Down):**
    *   A user at "Top Secret" cannot write to a file labeled "Secret."
    *   *Why?* If a Top Secret user opens a Top Secret file and copies data into a Secret file, they have just leaked classified info to a lower security level.
    *   **Goal:** Enforce a "one-way flow of information" (Data can flow up, but never down).
  
  ---
- ### Study Questions for Part 3
  1.  **MFA Check:** If a website requires a Password and a Voice Print, is that considered Two-Factor Authentication? Why or why not?
  2.  **Bell-LaPadula:** You have "Secret" clearance.
    *   Can you read a "Top Secret" document?
    *   Can you read an "Unclassified" document?
    *   Can you write to a "Top Secret" document? (Hint: Yes, this is "Writing Up", which is allowed to preserve confidentiality, though it might impact integrity).
  3.  **Model Identification:** In a hospital, doctors can view patient records, but nurses can only view patient vitals. When a nurse becomes a doctor, their permissions update automatically based on their new title. Which Access Control Model is this?
  
  ---