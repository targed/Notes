## I. The "CIA Triad"
The CIA Triad is the industry standard model for information security. If you are securing a system, you are trying to satisfy one or more of these three goals.
- ### 1. Confidentiality
  *   **Goal:** Secrecy. Only authorized people should see the data.
  *   **The Threat:** **Eve** (Passive Eavesdropper).
  *   **The Mechanism:** **Encryption.**
    *   *Concept:* Alice and Bob share a "Key." Without this key, the data looks like random noise (ciphertext).
    *   *Other Tools:* Steganography (hiding data inside images/audio), Access Controls (preventing file open), Database Views.
- ### 2. Integrity
  *   **Goal:** No Corruption. The data received must be exactly identical to the data sent.
  *   **The Threat:** **Mallory** (Active Man-in-the-Middle).
    *   *Attack:* Mallory intercepts a banking request "$100 transfer" and changes it to "$1000 transfer" before forwarding it.
  *   **The Mechanism:** **Redundancy & Hashing.**
    *   **Hashing (MD5, SHA-1):** A mathematical "fingerprint" of the file. If Mallory changes even one bit of the file, the fingerprint changes completely.
    *   **MAC (Message Authentication Code):** A hash that uses a secret key. This proves not only that the file hasn't changed, but that it came from someone who holds the secret key.
- ### 3. Availability
  *   **Goal:** Uptime. The system must be accessible when the user needs it.
  *   **The Threat:** **DoS (Denial of Service)** or **DDoS (Distributed Denial of Service)**.
    *   *Attack:* Flooding a server with so much junk traffic that it cannot process legitimate requests (like a traffic jam blocking an ambulance).
  *   **The Mechanism:** **Redundancy.**
    *   Eliminate "Single Points of Failure." If one server crashes, another takes over.
    *   **Quotas/Limits:** Prevent one user from hogging all the storage or bandwidth.
  
  ---
- ## II. The "Plus" Goals
  While CIA covers the basics, modern commerce needs two more attributes.
- ### 4. Accountability
  *   **Goal:** Traceability. If a breach occurs, we must know *who* did it and *when*.
  *   **The Mechanism:** **Logging & Audit Trails.**
  *   **Critical Requirement:** The logs themselves must be secure.
    *   *The Risk:* A smart attacker's first move is often to delete the logs (cover their tracks).
    *   *Secure Timestamping:* Using a synchronized clock (NTP) so we know the exact order of events.
- ### 5. Non-Repudiation
  *   **Goal:** Undeniability. Alice cannot send a message to buy stock and then later claim, "I never sent that."
  *   **The Context:** This is crucial for legal and financial transactions.
  *   **The Mechanism:** **Digital Signatures.**
    *   If a message is signed with Alice's Private Key, only Alice could have created it. Even she cannot deny it later.
    *   *Note:* This requires a Trusted Third Party (Trent/CA) to verify that the key actually belongs to Alice.
  
  ---
- ## III. Concepts at Work: The DVD Factory Example
  The slides use a B2B (Business-to-Business) scenario to illustrate these concepts in action.
  *   **Scenario:** A PC store (Bob) orders parts from a Factory (Server).
  *   **Availability:** The Factory ensures the web server is running 24/7.
  *   **Confidentiality:** They use SSL/TLS (HTTPS) to encrypt the connection so credit card numbers aren't sniffed.
  *   **Authentication:** Bob logs in with a username/password.
  *   **Authorization:** The Factory database checks if Bob is actually allowed to order widgets (Role-Based Access).
  *   **Integrity:** Each packet sent over the network has a check-sum/MAC to ensure no data corruption occurred during transit.
  *   **Accountability:** The Factory logs the transaction ID, time, and user.
  
  ---
- ## IV. Security "Anti-Patterns" (Strategies to Avoid)
  The course warns against two common but flawed ways of thinking about security.
- ### 1. Security by Obscurity 
  *   **The Fallacy:** "If I hide how my system works (e.g., hiding the source code, using a secret custom protocol), hackers won't figure out how to break it."
  *   **The Reality:** Hackers *always* figure it out (Reverse Engineering).
  *   **The Better Path:** **Open Design.**
    *   Systems like TCP/IP, AES Encryption, and Linux are open source. Everyone knows how they work. They are secure because the *mathematics and logic* are sound, not because the logic is hidden. "The only secret should be the Key."
- ### 2. Security by Legislation
  *   **The Fallacy:** "If we make a rule saying 'Do not write passwords on sticky notes', we are secure."
  *   **The Reality:** Users are human. They prioritize convenience over security. If a policy is too hard to follow, users will find a way around it.
  *   **The Lesson:** You cannot rely solely on user awareness. You must enforce policy with technology (e.g., instead of asking users to choose strong passwords, *force* the system to reject weak ones).
  
  ---
- ### Study Questions for Part 4
  1.  **CIA Triad:** A hospital's patient database is encrypted (Confidentiality). However, a ransomware virus hits, scrambling the data so doctors cannot read it. Which part of the CIA triad has been compromised? (Hint: It's not Confidentiality, because the data wasn't leaked—it's just unusable).
  2.  **Integrity vs. Confidentiality:** Can a message have Integrity without Confidentiality? (e.g., A public announcement signed by the President. Everyone can read it, but we need to know it wasn't changed).
  3.  **Obscurity:** Why is open-source encryption (like AES) generally considered more secure than a "proprietary" encryption algorithm invented by a specific company?
  
  ---