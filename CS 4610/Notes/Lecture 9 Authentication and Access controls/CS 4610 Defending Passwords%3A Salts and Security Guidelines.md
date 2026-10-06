## I. What is a Password Salt? (Slides 17–19)
A **Salt** is a random string of data (an $n$-bit number) generated uniquely for each user when they create an account or change their password. 
*   **Generation:** It is derived from unpredictable sources, like the exact microsecond of the system clock combined with the process identifier.
*   **Storage (Slide 18):** Instead of just hashing the password, the server glues the Salt and the Password together and hashes the combination: $F(\text{Password} + \text{Salt})$. 
  *   The database stores three things in plaintext: `[Username]`, `[Salt]`, and `[Hash]`.
  *   *Crucial Concept:* The salt is **not a secret**. It is stored in plain text right next to the hash. Its power comes from its uniqueness, not its secrecy.
*   **Verification (Slide 19):** When Alice tries to log in:
  1. The server fetches Alice's unique `[Salt]` from the database.
  2. It appends that salt to the password Alice just typed in.
  3. It runs the hash function $F$ on the combined string.
  4. It compares the result to the stored `[Hash]`.
- ## II. Does a Salt Actually Help? (Slides 20–22)
  Let's evaluate how adding a Salt impacts the three dictionary attacks we learned in Part 2.
- ### 1. Attack 1 (Online Attack): Does Salt help? (Slide 20)
  *   **Answer: NO.** 
  *   *Why?* In an online attack, the attacker is interacting with the live web server (e.g., typing "password123" into the login box). The server automatically fetches the salt and does the math for the attacker. The salt provides zero protection against someone just guessing passwords on the login page.
- ### 2. Attack 2 (Offline Attack): Does Salt help? (Slide 21)
  *   **Answer: YES, it slows the attacker down.**
  *   *Why?* Without a salt, the attacker hashes the dictionary word "Eagle" once and compares it against *every* user in the stolen database. With a salt, the attacker must hash "Eagle" + Alice's Salt, check Alice's account, then hash "Eagle" + Bob's Salt, check Bob's account, etc. It multiplies the attacker's workload by the number of users in the database.
- ### 3. Attack 3 (Pre-Computed / Rainbow Tables): Does Salt help? (Slide 22)
  *   **Answer: YES, it completely destroys the attack.**
  *   *Why?* A Rainbow Table relies on pre-computing the hash of "Eagle" *before* stealing the database. But if the system uses a 64-bit salt, there are $2^{64}$ possible variations of the word "Eagle". The attacker would have to pre-compute and store a massive dictionary for *every single possible salt value in existence*. It is computationally and physically impossible to store a Rainbow Table that large. The attacker is forced to fall back to the much slower Attack 2.
- ## III. General Password Guidelines (Slide 23)
  Because mathematical defenses (like Salts) cannot stop humans from choosing bad passwords, systems must enforce strict guidelines:
  1.  **System-generated initial passwords:** Force users to change them immediately upon first login.
  2.  **Periodic changes:** Force expiration (e.g., every 90 days) to limit the usefulness of a stolen password. *(Note: NIST recently updated their guidelines to advise against this unless a breach is suspected, but it remains a classic exam concept!).*
  3.  **Reject dictionary words:** Use a password complexity filter to block "password123" or "admin".
  4.  **No reuse:** Users should never use the same password across multiple sites. If a low-security forum is breached, attackers will immediately try those same email/password combinations on high-security banking sites (Credential Stuffing).
- ## IV. Other Password Attacks (Slide 24)
  Hashing and Salting only protect passwords *at rest* (in the database). They do not protect passwords *in transit* or *in use*.
  *   **Technical Attacks:**
    *   *Eavesdropping:* Sniffing network traffic (e.g., HTTP without SSL/TLS) to read passwords in plaintext before they reach the server.
    *   *Trojans/Keyloggers:* Malware sitting on the user's computer that records the keystrokes as the user types the password. The malware steals the password before the hashing process even begins.
  *   **Social Attacks:**
    *   *Careless handling:* Writing passwords on sticky notes attached to the monitor.
    *   *Phishing:* Tricking the user into typing their password into a fake website (as discussed in the Web Security module).
  
  ---
- ### Study Questions for Part 3
  1. **The Secret Myth:** Is a password salt considered a cryptographic "secret key"? If an attacker steals a database and sees the salt values in plain text, is the security of the hashed passwords ruined?
  2. **Rainbow Tables:** Explain precisely why adding a 32-bit random salt to user passwords makes the use of pre-computed Rainbow Tables mathematically infeasible for an attacker.
  3. **Attack Vectors:** You implement SHA-256 hashing with a 64-bit unique salt for every user in your database. Will this stop an attacker who has installed a Trojan Keylogger on the CEO's laptop? Why or why not?
  
  ---
-