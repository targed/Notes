### **8.2 Principles of Cryptography**

Cryptography is the science of secure communication. It provides the fundamental building blocks—the ciphers and algorithms—that allow us to build secure protocols.

**The Language of Cryptography**
*   **Plaintext (m):** The original, unencrypted message.
*   **Ciphertext (K(m)):** The encrypted, scrambled message.
*   **Encryption Algorithm:** The procedure used to convert plaintext into ciphertext.
*   **Decryption Algorithm:** The procedure used to convert ciphertext back into plaintext.
*   **Key (K):** A piece of secret information. The encryption and decryption algorithms are public knowledge; the security of the communication relies entirely on the secrecy of the key.

**Two Main Types of Cryptography**
- #### **1. Symmetric Key Cryptography**
  
  *   **Core Idea:** The sender (Alice) and the receiver (Bob) use the **same single secret key** (let's call it $K_S$) for both encryption and decryption.
  *   **How it Works:**
    1.  Alice encrypts her plaintext message `m` using the shared key $K_S$ to produce ciphertext: $c = K_S(m)$.
    2.  Bob receives the ciphertext `c` and uses the exact same key $K_S$ to decrypt it and recover the original plaintext: $m = K_S(c)$.
  *   **Example (Substitution Cipher):** A simple (and insecure) example is a monoalphabetic substitution cipher, where the "key" is the mapping of one letter to another (e.g., 'a' maps to 'm', 'b' maps to 'n', etc.).
  *   **Modern Algorithms:**
    *   **DES (Data Encryption Standard):** An older standard from the 1970s that used a 56-bit key. It is now considered insecure because its key is too short and can be broken by modern computers via brute force (trying every possible key) in less than a day.
    *   **3DES (Triple DES):** An enhancement that applies the DES algorithm three times with three different keys to increase its effective key length and security.
    *   **AES (Advanced Encryption Standard):** The modern, dominant standard. It is a block cipher that processes data in 128-bit blocks and can use key sizes of 128, 192, or 256 bits. It is extremely secure; a brute force attack against a 128-bit AES key is computationally infeasible.
  *   **The Fundamental Challenge:** The biggest problem with symmetric key cryptography is **key distribution**. How do Alice and Bob securely agree on the shared secret key in the first place, especially if they have never met and are communicating over an insecure channel?
- #### **2. Public Key (Asymmetric) Cryptography**
  
  This is a radically different and revolutionary approach that solves the key distribution problem.
  
  *   **Core Idea:** Each participant has a **pair of keys**: a **public key** ($K^+$) and a **private key** ($K^-$).
    *   The **public key** is made known to everyone in the world.
    *   The **private key** is kept secret and is known only to its owner.
  *   **How it Works (for Confidentiality):**
    1.  Alice wants to send a secret message to Bob. She looks up Bob's **public key** ($K_B^+$).
    2.  Alice encrypts her message `m` using Bob's public key: $c = K_B^+(m)$.
    3.  Bob receives the ciphertext `c`. He uses his own **private key** ($K_B^-$) to decrypt it: $m = K_B^-(c)$.
  *   **Key Properties:**
    1.  What is encrypted with the public key can *only* be decrypted with the corresponding private key.
    2.  It must be computationally impossible to derive the private key from the public key.
  
  **The RSA Algorithm**
  RSA (Rivest, Shamir, Adelson) is the most widely used public key algorithm.
  
  *   **Prerequisite (Modular Arithmetic):** RSA is based on number theory, specifically modular arithmetic, where we deal with the remainders of division. The key operation is modular exponentiation: $c = m^e \pmod{n}$.
  *   **Key Generation:**
    1.  Choose two very large prime numbers, `p` and `q`.
    2.  Compute `n = p * q` and `z = (p-1) * (q-1)`.
    3.  Choose an encryption exponent `e` that has no common factors with `z`.
    4.  Compute the decryption exponent `d` such that `(e * d) mod z = 1`.
    5.  The **public key** is the pair `(n, e)`.
    6.  The **private key** is the pair `(n, d)`.
  *   **Encryption and Decryption:**
    *   To encrypt a message `m`, compute: $c = m^e \pmod{n}$.
    *   To decrypt a ciphertext `c`, compute: $m = c^d \pmod{n}$.
  *   **Why it's Secure:** The security of RSA relies on the fact that it is extremely difficult to factor a very large number `n` back into its original primes `p` and `q`. Without knowing `p` and `q`, an attacker cannot compute `z` and therefore cannot derive the private key `d` from the public key `(n, e)`.
  
  **Hybrid Approach: Session Keys**
  *   **The Problem:** Public key cryptography (like RSA) is mathematically intensive and much slower than symmetric key cryptography (like AES). It is not practical to encrypt large amounts of data with RSA.
  *   **The Solution:** Use both! This is the standard practice on the internet.
    1.  Alice and Bob use **public key crypto** (slow but good for key exchange) to securely negotiate or exchange a **symmetric session key** ($K_S$). This is a temporary key that will only be used for this one communication session.
    2.  Once they both have the shared session key, they switch to **symmetric key crypto** (fast and efficient) to encrypt all the actual data for the rest of the session.