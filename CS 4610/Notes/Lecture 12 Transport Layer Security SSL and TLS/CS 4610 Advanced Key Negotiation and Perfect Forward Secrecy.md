## I. The Fatal Flaw of the RSA Handshake (Slides 20, 23–24)
In the classic RSA handshake, the client generates a random secret, encrypts it with the Server's Public Key, and sends it. Only the Server's Private Key can decrypt it. 
*   **The Scenario (Slide 24):** Eve is an intelligence agency or a hacker. She records all the encrypted network traffic between Alice and the bank today. She cannot read it, so she just stores the encrypted hard drives in a data center ("Store Now, Decrypt Later").
*   **The Compromise:** Three years later, Eve successfully hacks the bank and steals the server's Long-Term RSA Private Key.
*   **The Catastrophe:** Eve goes back to the traffic she recorded three years ago. She uses the stolen Private Key to decrypt the Client Key Exchange packet. She recovers the Pre-Master Secret. With that, she generates the symmetric session keys and retroactively decrypts the *entire conversation*. 
*   **The Conclusion:** RSA key exchange lacks **Forward Secrecy**. If the long-term key is compromised in the future, all past communications are destroyed.
- ## II. Perfect Forward Secrecy (PFS) (Slide 23)
  *   **Definition:** "Assurance that session keys will not be compromised even if long-term secrets used in the session key exchange are compromised."
  *   **The Goal:** We want a system where the keys used to encrypt the data exist *only* for the duration of the web session, and are then permanently destroyed. If a hacker steals the server's hard drive years later, there should be no mathematical way to reconstruct those session keys.
- ## III. Diffie-Hellman to the Rescue (Slide 25)
  As we learned in the Cryptography module, Diffie-Hellman (D-H) allows two parties to negotiate a shared secret over a public channel.
  *   **How it provides Forward Secrecy:** In a properly configured TLS connection, the server and client use **Ephemeral Diffie-Hellman (DHE or ECDHE)**. "Ephemeral" means temporary. 
  *   **The Process:** 
    1. For *every single connection*, Alice and Bob generate brand new, random secret exponents ($S_A$ and $S_B$). 
    2. They do the math, agree on a symmetric key, and encrypt the web traffic.
    3. As soon as Alice closes her browser, **they both permanently delete $S_A$ and $S_B$ from their RAM.**
  *   **The Result:** Even if Eve records the traffic and hacks the server years later, it doesn't matter. There is no long-term key on the server's hard drive that can reverse the D-H math. The temporary keys are gone forever.
- ## IV. Authenticated Diffie-Hellman (Slides 26–28)
  But wait—we know from the previous module that raw Diffie-Hellman is vulnerable to a **Man-in-the-Middle (MITM) Attack**, because D-H provides zero authentication. Anyone can pretend to be Bob.
  
  How does TLS fix this? By **combining the best of both worlds.**
  *   We use **Diffie-Hellman** for the Key Exchange (to get Perfect Forward Secrecy).
  *   We use **RSA** (or ECDSA) strictly for Digital Signatures (to get Authentication).
  
  **The Modern TLS Handshake (Slide 27):**
  1.  The Server generates its temporary D-H parameter ($T_S$).
  2.  The Server takes its Long-Term Private Key and **digitally signs** $T_S$.
  3.  The Server sends $T_S$, the signature, and its Certificate to the Client.
  4.  The Client uses the Public Key in the Certificate to verify the signature. 
    *   *Security check passed:* The client knows $T_S$ definitely came from the real server. MITM is thwarted.
  5.  The Client sends their temporary D-H parameter ($T_C$). They both generate the session keys, encrypt the data, and then throw the D-H parameters away.
- ## V. How the Client Proves Themselves (Slide 28)
  In standard TLS (like when you browse to Amazon or your bank), the Server proves its identity using a Certificate. But does the *Client* prove their identity during the TLS handshake?
  *   **Answer:** Usually, **NO.** The TLS connection is established entirely anonymously from the client's side. 
  *   **Application Layer Authentication:** Once the secure, encrypted TLS tunnel is established, the server sends the login page. The client then types in their username and password. The user is authenticated at the *Application Layer* (HTTP/HTML), securely protected inside the TLS tunnel. 
  *   *Note:* TLS *does* support "Mutual Authentication" where the client also sends a certificate, but this is rarely used for regular web browsing; it's mostly used for Server-to-Server API communications.
- ## VI. Module Summary (Slide 29)
  *   **TLS** is the ultimate protocol for secure internet communication.
  *   It is highly flexible, allowing the client and server to negotiate which algorithms they want to use (Cipher Suites).
  *   By using Ephemeral Diffie-Hellman signed by an RSA/DSA Certificate, TLS achieves **Authentication, Confidentiality, Integrity, and Perfect Forward Secrecy.**
    *   *Real-World Context:* Because Forward Secrecy is so vital, **TLS 1.3** actually made it mandatory. The classic RSA key exchange we learned in Part 2 is completely banned in modern internet traffic!
  
  ---
- ### Study Questions for Part 3
  1. **The RSA Flaw:** Explain the "Store Now, Decrypt Later" attack. What specific piece of data must an attacker steal from the server in the future to successfully read past network traffic that used an RSA Key Exchange?
  2. **Perfect Forward Secrecy:** What does the term "ephemeral" mean in the context of an Ephemeral Diffie-Hellman (DHE) key exchange, and why is this property required to achieve Perfect Forward Secrecy?
  3. **Combining Algorithms:** In a modern, secure TLS handshake, the protocol uses *both* RSA and Diffie-Hellman. What specific security goal is RSA responsible for, and what specific security goal is Diffie-Hellman responsible for?
  
  ---