### **8.4 Securing E-mail**

Standard email, which uses SMTP, sends all messages in plaintext. This means anyone who can intercept the traffic (like an ISP or an attacker) can read, modify, or forge emails. Securing email requires providing the core security properties: confidentiality, authentication, and message integrity.

We will look at how to achieve these properties using the cryptographic tools from the previous sections.
- #### **1. Securing Email: Confidentiality**
  
  **Goal:** Alice wants to send an email `m` to Bob such that only Bob can read its contents.
  
  **The Hybrid Approach (Public Key + Symmetric Key):**
  Encrypting a long email message with a public key algorithm like RSA would be extremely slow. Therefore, a more efficient hybrid approach is used, which combines the speed of symmetric encryption with the key-distribution convenience of public key encryption.
  
  *   **Alice's Actions (Sender):**
    1.  **Generate a Session Key:** Alice generates a new, random **symmetric session key** ($K_S$). This key will be used only for this one message.
    2.  **Encrypt the Message:** Alice encrypts the email message `m` using the fast symmetric key algorithm (e.g., AES) with the session key $K_S$. The result is the encrypted message, $K_S(m)$.
    3.  **Encrypt the Session Key:** To securely send the session key to Bob, Alice looks up Bob's **public key** ($K_B^+$) and uses it to encrypt the session key $K_S$. The result is $K_B^+(K_S)$.
    4.  **Send:** Alice sends both the encrypted message and the encrypted session key to Bob.
  
  *   **Bob's Actions (Receiver):**
    1.  **Decrypt the Session Key:** Bob receives the two components. He first uses his **private key** ($K_B^-$) to decrypt the encrypted session key and recover the original session key $K_S$.
        $ K_B^-(K_B^+(K_S)) = K_S $
    2.  **Decrypt the Message:** Now that Bob has the symmetric session key $K_S$, he can use it to decrypt the actual email message and recover the original plaintext `m`.
        $ K_S(K_S(m)) = m $
  
  This process ensures that even if Trudy intercepts the email, she cannot read it because she does not have Bob's private key to unlock the session key.
- #### **2. Securing Email: Authentication and Integrity**
  
  **Goal:** Alice wants to send an email `m` to Bob in a way that Bob can be sure it came from Alice and was not altered in transit.
  
  **The Digital Signature Approach:**
  This is achieved by having Alice digitally sign her message.
  
  *   **Alice's Actions (Sender):**
    1.  **Create a Hash:** Alice takes her plaintext message `m` and runs it through a cryptographic hash function (e.g., SHA-256) to produce a fixed-length message digest, `H(m)`.
    2.  **Sign the Hash:** Alice then uses her **private key** ($K_A^-$) to encrypt the hash. This encrypted hash is the **digital signature**.
    3.  **Send:** Alice sends both the original plaintext message `m` and the digital signature to Bob.
  
  *   **Bob's Actions (Receiver):**
    1.  **Verify the Signature:** Bob receives the message and the signature.
    2.  He re-computes the hash of the received message `m` to get `H(m)`.
    3.  He uses Alice's **public key** ($K_A^+$) to decrypt the signature, which reveals the original hash that Alice computed.
    4.  **Compare:** He compares the hash he just computed with the hash from the signature.
        *   If they match, he has verified both the **authentication** (only Alice could have created the signature) and the **integrity** (the message was not changed).
- #### **3. Achieving All Three Properties: Confidentiality, Authentication, and Integrity**
  
  To get all security properties, we simply combine the two procedures above.
  
  *   **Alice's Actions (Sender):**
    1.  Alice digitally signs her message `m` (by encrypting its hash with her private key).
    2.  Alice then encrypts *both* the original message and her signature using a symmetric session key, $K_S$.
    3.  Finally, she encrypts the session key $K_S$ using Bob's public key.
    4.  She sends the final encrypted package to Bob.
  
  *   **Bob's Actions (Receiver):**
    1.  Bob uses his private key to decrypt and recover the session key $K_S$.
    2.  He uses the session key to decrypt the main payload, revealing the original message `m` and Alice's digital signature.
    3.  He then performs the digital signature verification steps to confirm Alice's identity and the message's integrity.
  
  This comprehensive process requires Alice to use three keys: her own private key, Bob's public key, and a new, temporary symmetric session key.