## I. The Password Paradox & Secure Storage (Slides 9–10)
*   **The Core Conflict:** A password should be easy for a human to remember, but mathematically difficult for a computer to guess. In practice, this is incredibly difficult to achieve.
*   **The Storage Problem:** How does the server verify your password without putting you at risk?
  *   *The Bad Way:* Storing passwords in plaintext (or reversible encryption). If the server is breached, the attacker instantly gets every user's actual password.
  *   *The Secure Way:* **One-Way Functions (Cryptographic Hashing).** 
*   **How Hashing Works for Verification:**
  1.  When you create an account, the server takes your password, runs it through a cryptographic hash function (like SHA-256), and stores *only the resulting hash* in the database.
  2.  When you log in, you send your password. The server hashes it again.
  3.  The server compares the *new hash* to the *stored hash*. If they match, you are authenticated. The server never actually needs to know your plaintext password to verify it!
- ## II. Human Behavior and Password Entropy (Slides 11–13)
  Why do attackers bother guessing passwords? Because humans are predictable.
  
  *   **The Math of Brute Force (Slide 11):** 
    *   If a password is 1 to 9 characters long and uses *only lowercase letters* (26 possible characters), the total number of combinations is roughly $5 \times 10^{12}$. 
    *   At 1 guess per millisecond, it would take 150 years to try every combination. 
    *   *Expanded Context:* This math assumes a **Brute Force Attack** (trying `aaaaaaa`, then `aaaaaab`, then `aaaaaac`). However, attackers almost *never* use pure brute force because of human nature.
  *   **The Reality of Human Choices (Slides 12–13):**
    *   Studies show that a massive percentage of passwords (up to 86% in older studies) are easily guessed because they are not random.
    *   Humans use: Pet names, common words (dictionary words), dates of birth, and simple variations (like appending a `1` or `!` to the end of a word, or spelling a word backward).
    *   Because humans draw from a tiny pool of "meaningful" words, the attacker doesn't need to try $5 \times 10^{12}$ combinations; they only need to try a few million common dictionary words.
- ## III. The Three Types of Dictionary Attacks (Slides 14–16)
  A **Dictionary Attack** uses a predefined list of likely passwords (the "dictionary") rather than generating random character combinations. The slides break this down into three distinct attack vectors:
- ### Attack 1: Online Dictionary Attack (Slide 14)
  *   **The Mechanism:** The attacker sits at the login screen of the web application and repeatedly types in usernames and password guesses from their dictionary, submitting them over the network (e.g., trying "Eagle", then "Wine", then "Rose").
  *   **The Limitation:** This is very slow. It is bottlenecked by the network speed and the server's processing time. 
  *   *Expanded Context (Defenses):* Online attacks are easily defeated by server-side protections like **Account Lockouts** (locking the account after 5 failed attempts), **Rate Limiting** (delaying responses), or **CAPTCHAs**.
- ### Attack 2: Offline Dictionary Attack (Slide 15)
  *   **The Mechanism:** The attacker breaches the server and steals the backend database containing the hashed passwords. They take this file offline to their own powerful computer. They take their dictionary of words, run each word through the hash function $F$, and see if the resulting hash matches anything in the stolen database.
  *   **The Advantage:** It is exponentially faster than an online attack. The attacker is no longer constrained by the network or server lockouts. They can use specialized hardware (like clusters of GPUs) to compute billions of hashes per second.
- ### Attack 3: Pre-Computed Offline Attack / Rainbow Tables (Slide 16)
  *   **The Mechanism:** To speed up an offline attack even further, the attacker doesn't calculate the hashes on the fly. Instead, *before* they even steal the database, they take a massive dictionary of millions of words and pre-compute the hash for every single one.
  *   **The Advantage:** They build a giant lookup table (Word $\rightarrow$ Hash). When they steal the password file, they don't have to do any math. They just take the stolen hashes and do a simple database lookup in their pre-computed table. What used to take days of computing power now takes seconds.
  *   *Expanded Context:* These pre-computed databases are often referred to as **Rainbow Tables**.
  
  ---
- ### Study Questions for Part 2
  1. **Hashing vs. Encryption:** Why is it considered a critical security failure to store user passwords using two-way encryption (like AES) rather than a one-way hash (like SHA-256)?
  2. **Attack Constraints:** Why is an attacker severely limited when executing an *Online* Dictionary Attack, and how does an *Offline* Dictionary Attack bypass those limits?
  3. **Lookup Speed:** Explain the mechanism behind Attack 3 (Pre-computed dictionary attack). Why is it faster for the attacker to crack a stolen database using this method compared to Attack 2?
  
  ---
-