## I. What is a Hash Function? (Slide 21)
A hash function is an algorithm that takes a message of *arbitrary* length (could be a 1-word text file or a 100GB database) and squashes it down into a *fixed-length* short message (usually 128, 160, or 256 bits).
*   **Other Names:** Message Digests, One-way transformations, One-way functions.
*   **Common Algorithms (Slide 22):** MD5 (broken/legacy), SHA-1 (legacy), SHA-256 (modern standard).
- ## II. The Four Desirable Properties of a Hash Function (Slide 22)
  For a hash function to be considered cryptographically secure, it must meet four strict mathematical properties:
  
  1.  **Performance:** It must be fast and computationally easy to calculate $H(m)$ for any given message $m$.
  2.  **One-Way Property (Pre-image Resistance):** It must be a one-way street. If an attacker is given the hash value $H(m)$, it must be computationally infeasible to reverse-engineer it to find the original message $m$. 
  3.  **Weak Collision Free (Second Pre-image Resistance):** If you are given a specific message $m$ and its hash $H(m)$, it must be computationally infeasible to find a *different* message $m'$ that produces the exact same hash ($H(m') = H(m)$). 
    *   *Why this matters:* If an attacker intercepts your signed contract ($m$), they shouldn't be able to craft a fake contract ($m'$) that perfectly matches your original signature.
  4.  **Strong Collision Free (Collision Resistance):** It must be computationally infeasible to find *any two random messages* ($m_1$ and $m_2$) that produce the same hash ($H(m_1) = H(m_2)$).
    *   *Note:* Because the output size is fixed (e.g., 256 bits) but the input size is infinite, collisions *mathematically must exist* (the Pigeonhole Principle). "Strong collision free" just means it would take a supercomputer millions of years to actually find one.
- ## III. Primary Applications of Hashing (Slides 23, 24, 27)
- ### 1. Optimizing Digital Signatures (Slide 23)
  Asymmetric encryption (like RSA) is too slow to sign an entire document. 
  *   **The Process:** Instead of signing the 500-page document, Alice hashes the document to generate a tiny 256-bit fingerprint. She then uses her Private Key to encrypt *only the hash*. This creates the Digital Signature.
  *   **The Verification:** Bob receives the document and the signature. He hashes the document himself. He then decrypts Alice's signature using her Public Key. If the two hashes match perfectly, the document is mathematically proven to be authentic and unaltered.
- ### 2. Password Hashing (Slide 24)
  As covered in previous modules, we store $H(password + salt)$ to protect databases from offline dictionary attacks.
- ### 3. Authentication Tokens / Commitment Schemes (Slide 27)
  *   **The Mechanism:** Alice wants to prove she knows a secret without revealing it right away. She sends $H(m)$ to Bob today. Tomorrow, she reveals $m$. Bob hashes $m$ and sees it matches the hash from yesterday.
  *   **The Property Used:** This relies on the **One-Way Property**. Bob couldn't figure out $m$ yesterday, but Alice was "committed" to her answer because she couldn't change $m$ without changing the hash. (This is how the S/Key Hash Chain from the previous module worked!).
- ## IV. Message Integrity & MACs (Slides 25–26)
  How do we ensure a message isn't altered in transit over an insecure network?
  
  *   **The Threat (Slide 25):** Alice sends a message $m$ ("Transfer \$10"). Trudy the attacker intercepts it, alters it to $m'$ ("Transfer \$10,000"), and forwards it to Bob.
  *   **The Flawed Fix:** Alice sends $m$ and the plain hash $H(m)$. Trudy intercepts them, changes $m$ to $m'$, calculates the *new* hash $H(m')$, and sends both to Bob. Bob verifies the new hash and accepts the fraudulent message!
  *   **The Real Solution: MAC (Message Authentication Code) (Slide 26)**
    *   Alice and Bob securely agree on a **Secret Key ($k$)**.
    *   Alice takes the message $m$, appends the secret key $k$ to it, and hashes them *together*: $H(m||k)$. 
    *   Alice sends Bob the plaintext message $m$ along with the MAC $H(m||k)$. (She does *not* send the key!).
    *   **Why it defeats Trudy:** Trudy intercepts $m$ and changes it to $m'$. However, to generate a valid MAC for the new message, Trudy needs to calculate $H(m'||k)$. Since Trudy **does not know the secret key $k$**, she cannot calculate the correct new MAC. When Bob receives the tampered message, the MAC check will fail, and he will drop the packet.
  
  ---
- ### Study Questions for Part 3
  1. **Collision Resistance:** What is the difference between the "Weak collision free" property and the "Strong collision free" property of a hash function?
  2. **Digital Signatures:** When Alice digitally signs a large PDF file, does she use her Private Key to encrypt the actual PDF file itself? Why or why not?
  3. **Message Authentication Codes (MAC):** In a MAC scheme, why can't a "Man-in-the-Middle" attacker simply intercept a message, alter the plaintext, calculate a new hash, and forward it to the receiver? What piece of information is the attacker missing?
  
  ---