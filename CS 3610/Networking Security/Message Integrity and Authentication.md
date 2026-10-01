### **8.3 Message Integrity and Authentication**

While encryption provides confidentiality, it doesn't solve two other critical problems:
*   **Authentication:** How does Bob know that a message actually came from Alice and not from an impostor, Trudy?
*   **Message Integrity:** How does Bob know that the message's contents weren't altered in transit by Trudy?

This section covers the cryptographic techniques used to solve these two problems.

---
- ### **Authentication**
  
  Authentication is the process of proving one's identity. In a network, where you can't see the other person, this is a significant challenge. We'll build up an authentication protocol incrementally.
  
  *   **Protocol ap1.0: Alice says "I am Alice".**
    *   **Flaw:** Trudy can simply intercept this message and send her own message to Bob, claiming to be Alice. This fails completely.
  
  *   **Protocol ap2.0: Alice sends a packet with her source IP address.**
    *   **Flaw:** This relies on the source IP address being trustworthy, but Trudy can easily create a packet with a "spoofed" (fake) source IP address.
  
  *   **Protocol ap3.0: Alice sends her secret password in plaintext.**
    *   **Flaw:** This is vulnerable to a **playback attack**. Trudy can use a packet sniffer to record Alice's packet (containing her IP and password). At a later time, Trudy can "play back" this captured packet to Bob, successfully impersonating Alice.
  
  *   **Protocol ap3.1: Alice sends her secret password, but it is encrypted.**
    *   **Flaw:** This is still vulnerable to a playback attack. Trudy doesn't need to know the password; she just needs to record the encrypted version and play it back.
  
  *   **Protocol ap4.0: Challenge-Response using a Nonce.**
    This is the first functional authentication protocol. It defends against playback attacks.
    *   **Nonce (R):** A number that is used only **once** in a lifetime. It's a random, unpredictable value.
    *   **Mechanism:**
        1.  Bob wants to authenticate Alice. He first sends a randomly generated nonce `R` to Alice. This is the "challenge".
        2.  Alice must receive `R` and encrypt it using a secret symmetric key ($K_{A-B}$) that only she and Bob share.
        3.  Alice sends the encrypted nonce, $K_{A-B}(R)$, back to Bob. This is the "response".
        4.  Bob decrypts the response with the shared key. If the result matches the original nonce `R` he sent, he knows he is talking to Alice.
    *   **Why it Works:** Trudy can record this exchange, but she cannot impersonate Alice later because Bob will issue a *new, different nonce*, and Trudy won't be able to encrypt it correctly.
  
  *   **Protocol ap5.0: Authentication using Public Key Crypto.**
    This version avoids the need for a pre-shared symmetric key.
    *   **Mechanism:**
        1.  Bob challenges Alice with a nonce `R`.
        2.  Alice "signs" the nonce by encrypting it with her **private key** ($K_A^-$) and sends $K_A^-(R)$ back to Bob.
        3.  Bob looks up Alice's **public key** ($K_A^+$) and uses it to decrypt the response.
        4.  If $K_A^+(K_A^-(R))$ equals the original nonce `R`, Bob knows he is talking to Alice, because only the holder of Alice's private key could have encrypted the nonce in a way that is successfully decrypted by her public key.
    *   **The Flaw (Man-in-the-Middle Attack):** This protocol is vulnerable to a **man-in-the-middle attack**. Trudy can intercept the initial communication. She can tell Bob "I am Alice" and tell Alice "I am Bob." She performs a separate key exchange with each of them, decrypts messages from Alice, re-encrypts them with her own key, and forwards them to Bob (and vice-versa). Alice and Bob think they are talking to each other, but Trudy is in the middle, reading and modifying everything. This highlights the need to verify that a public key truly belongs to the person it claims to.
  
  ---
- ### **Message Integrity and Digital Signatures**
- #### **Cryptographic Hash Functions**
  
  Public key encryption is too slow to encrypt/sign entire large messages. Instead, we use a **cryptographic hash function** to create a short, fixed-length "fingerprint" of the message, called a **message digest**.
  
  *   **Hash Function (H):** A mathematical function that takes an arbitrary-length message `m` and produces a fixed-length output `H(m)`.
  *   **Key Properties:**
    1.  **Fixed-size output:** MD5 produces a 128-bit hash; SHA-1 produces a 160-bit hash.
    2.  **Easy to compute:** It is computationally easy to compute H(m) from m.
    3.  **Pre-image resistance (One-way):** Given a hash value `x`, it is computationally infeasible to find a message `m` such that `H(m) = x`.
    4.  **Collision resistance:** It is computationally infeasible to find any two different messages `m1` and `m2` such that `H(m1) = H(m2)`.
  *   **Examples:** MD5 (older, now considered insecure), SHA-1 (being phased out), SHA-256 (strong, widely used).
- #### **Digital Signatures**
  
  A digital signature is a cryptographic technique that provides **message integrity**, **authentication**, and **non-repudiation**. It uses a hash function combined with public key cryptography.
  
  *   **How Bob signs a message `m`:**
    1.  Bob calculates the hash of the message: `H(m)`.
    2.  Bob then **encrypts the hash** with his own **private key** ($K_B^-$). This encrypted hash is the **digital signature**.
    3.  Bob sends both the original plaintext message `m` and the signature $K_B^-(H(m))$ to Alice.
  
  *   **How Alice verifies the signature:**
    1.  Alice receives `m` and the signature.
    2.  Alice computes the hash of the message she received: `H(m)`.
    3.  Alice uses Bob's **public key** ($K_B^+$) to **decrypt the signature**. This reveals the original hash that Bob computed.
    4.  **Verification:** Alice compares the hash she computed with the hash she got from decrypting the signature.
        *   If they are **equal**, the signature is valid. This proves two things:
            *   **Authentication:** The message must have come from Bob, because only he could have encrypted something that is decryptable with his public key.
            *   **Integrity:** The message was not altered in transit. If it had been, the hash Alice computed would not match the hash in the signature.
  
  *   **Non-repudiation:** Because only Bob could have created the signature, he cannot later deny having sent the message. This provides a legally binding property similar to a handwritten signature.
  
  ---
- ### **Public Key Certification**
  
  *   **The Problem:** How does Alice know that the public key she is using actually belongs to Bob and not to Trudy (the man-in-the-middle)?
  *   **The Solution: Certification Authority (CA)**
    *   A **Certification Authority (CA)** is a trusted third party (like VeriSign or Let's Encrypt) whose job is to bind a public key to a specific entity (a person, a website, etc.).
    *   **Process:**
        1.  Bob provides "proof of identity" to the CA and registers his public key with them.
        2.  The CA creates a **digital certificate** that contains Bob's identity information (like `www.bob.com`) and Bob's public key.
        3.  The CA then uses its own **private key** to **digitally sign** this certificate.
    *   **Verification:**
        1.  When Alice wants to communicate with Bob, Bob sends her his certificate.
        2.  Alice uses the CA's well-known **public key** to verify the signature on the certificate.
        3.  If the signature is valid, Alice now trusts that the public key contained within that certificate truly belongs to Bob. She can now use this certified public key to communicate securely with him.