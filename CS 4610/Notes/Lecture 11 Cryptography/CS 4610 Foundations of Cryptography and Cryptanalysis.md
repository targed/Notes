## I. Core Definitions (Slides 2–3)
Cryptography translates to the "art of secret writing." Its primary goal is to convert data into an unintelligible (random-looking) form to protect its confidentiality.

*   **Plaintext:** The original, readable message (e.g., "Hello World").
*   **Ciphertext:** The transformed, unrecognizable message (e.g., "Khoor Zruog").
*   **Encryption:** The algorithm/process of converting Plaintext $\rightarrow$ Ciphertext.
*   **Decryption:** The algorithm/process of converting Ciphertext $\rightarrow$ Plaintext.
*   **Reversibility:** A core requirement of encryption. Unlike *hashing* (which is a one-way street), encryption *must* be reversible so the authorized receiver can recover the exact original data without loss.
*   **Key:** The secret value used to control the encryption and decryption processes. 

*   **Expanded Context (Kerckhoffs's Principle):** In modern cryptography, the encryption algorithm itself (e.g., AES, RSA) is public and known to everyone. The security of the system must rely entirely on the secrecy of the **Key**, not the secrecy of the algorithm (which would be "Security by Obscurity").
- ## II. Cryptanalysis: Breaking the Code (Slides 4–7)
  Cryptanalysis is the art of revealing the secret—defeating cryptographic systems without knowing the key. The difficulty of breaking a cipher depends heavily on how much information the attacker (Eve) has.
  
  There are three primary models of attack:
- ### 1. Ciphertext-Only Attack (Slide 5)
  *   **What Eve has:** Only a set of intercepted ciphertexts. 
  *   **The Strategy:** Eve must analyze patterns in the ciphertext to guess the key or plaintext. 
  *   **Example:** Frequency analysis. If the letter "X" appears 12% of the time in a ciphertext, Eve can guess that "X" translates to "E" (the most common letter in the English language).
  *   **Difficulty:** This is the hardest attack to pull off. Modern encryption algorithms are designed to output ciphertext that looks completely statistically random, defeating pattern analysis.
- ### 2. Known Plaintext Attack (Slide 6)
  *   **What Eve has:** Samples of *both* the plaintext and its matching ciphertext. 
  *   **The Strategy:** Eve uses the pairs to mathematically deduce the secret key. Once she has the key, she can decrypt all *future* messages sent between Alice and Bob.
  *   **Example:** During WWII, the Allies broke the German Enigma code using Known Plaintext. They knew the Germans ended every message with "Heil Hitler" (Plaintext), and they intercepted the encrypted radio broadcasts (Ciphertext).
- ### 3. Chosen Plaintext Attack (Slide 7)
  *   **What Eve has:** The ability to choose arbitrary plaintexts and force the encryption machine to encrypt them, giving her the resulting ciphertexts.
  *   **The Strategy:** Eve acts as an "oracle." She feeds specific, carefully crafted messages (like a string of all "A"s) into the system to see exactly how the algorithm scrambles them, looking for mathematical weaknesses to extract the key.
- ## III. Secret Key (Symmetric) Cryptography (Slides 8–9)
  There are two main branches of cryptography. The first is **Secret Key Cryptography**.
  
  *   **The Concept:** The exact same key is used for both Encryption and Decryption. 
  *   **Also Known As:** Symmetric cryptography, Conventional cryptography.
  *   **The Formula:** 
    *   $Ciphertext = Encrypt(Plaintext, Key)$
    *   $Plaintext = Decrypt(Ciphertext, Key)$
  *   **Pros & Cons (Expanded Context):** Symmetric cryptography (like AES today) is incredibly fast and efficient, making it great for encrypting large files or hard drives. The massive drawback is the **Key Distribution Problem**: How do Alice and Bob securely share the secret key in the first place without Eve intercepting it?
- ### The Shift Cipher Exercise (Slide 9)
  The slide demonstrates the oldest symmetric cipher in history: The Caesar Cipher (a shift cipher).
  *   **The Rule:** Replace each letter with the one $x$ letters later in the alphabet.
  *   **The Key:** $x = 3$.
  *   **Plaintext:** `sell the business`
  *   **Ciphertext:** `vhoo wkh exvlqhvv` (s + 3 = v, e + 3 = h, etc.)
  *   **Decryption:** Bob receives the ciphertext. Because it is symmetric, he uses the *same key* (3) and does the reverse operation (shifts backward by 3).
  *   **Cryptanalysis Vulnerability:** This cipher is terribly weak. A *Ciphertext-Only attack* easily defeats it using frequency analysis. A *Known Plaintext attack* defeats it instantly (if you know 'v' maps to 's', you instantly know the key is 3). Furthermore, there are only 25 possible keys, so Eve could just brute-force all 25 shifts in a few seconds.
  
  ---
- ### Study Questions for Part 1
  1. **Cryptanalysis:** An attacker captures a network packet and sees the encrypted payload. They also know that every packet sent by this specific application always starts with the standard header `HTTP/1.1 200 OK`. Which of the three cryptanalysis attack models does this scenario describe?
  2. **Symmetric Crypto:** In Secret Key Cryptography, if Alice wants to communicate securely with 5 different people, and they all want to communicate securely with each other, how many unique secret keys must be generated in total so that no two pairs share the same key? (Hint: The formula for connections is $n(n-1)/2$).
  3. **Principles:** Why does modern cryptography demand that the encryption *algorithm* be public, rather than keeping it a secret from attackers? 
  
  ---
-