## I. The Core Concepts (Slides 2 & 6)
**Authentication (AuthN)** is the process of reliably verifying certain information—most commonly, verifying that someone or something is who they claim to be. 
*   *Note the difference:* Authentication (AuthN) proves *identity*. Authorization (AuthZ) determines *permissions* (what you are allowed to do once authenticated).

**Two Main Types of Authentication:**
1.  **User Authentication:** A person (or system) proving their identity to another entity. 
  *   *Terminology:* The **Supplicant** is the entity asking for access (Alice). The **Verifier** is the system checking the credentials (the Server).
2.  **Message Authentication:** Verifying that a message has not been altered in transit and genuinely came from the stated sender. (We previously saw this achieved using MACs—Message Authentication Codes).
- ## II. Primary Authentication Mechanisms (Slides 3–5)
  How does a supplicant actually prove their identity to a verifier? The slides outline three fundamental mechanisms:
- ### 1. Password-Based Authentication
  *   **The Mechanism:** The supplicant simply transmits a shared secret (the password) to the verifier.
  *   **The Flaw:** If sent in plaintext over a network, it is vulnerable to eavesdropping. Furthermore, it is highly susceptible to guessing and dictionary attacks.
- ### 2. Address-Based Authentication
  *   **The Mechanism:** The verifier assumes the identity of the user based on the network address (e.g., IP address or MAC address) the packets are coming from. 
    *   *Example:* A corporate firewall that automatically trusts any traffic coming from the CEO's static IP address.
  *   **The Flaw:** **Spoofing.** As we learned in earlier networking modules, IP addresses are just text fields in a packet header. An attacker can easily forge a packet to make it look like it came from a trusted IP. IP addresses were designed for *routing*, not for *identity verification*.
- ### 3. Cryptographic Authentication Protocols
  *   **The Mechanism (Challenge-Response):** Instead of sending a password over the network, cryptographic authentication relies on proving *knowledge* of a secret without revealing the secret itself.
    *   *How it works (Filling in the gap):* The verifier sends a random, one-time value called a **Challenge** (a nonce). The supplicant takes that Challenge, mathematically encrypts or hashes it using their Secret Key, and sends back the **Response**. The verifier checks the math. If it matches, the supplicant must own the key.
  *   **The Keys Used:**
    *   *Symmetric Key:* Both Alice and the Server share the same secret key.
    *   *Asymmetric (Public/Private) Key:* Alice signs the challenge with her Private Key; the Server verifies it using her Public Key.
- ## III. The Factors of Authentication (Slides 7–8)
  When a system asks you to authenticate, it challenges you based on specific "Factors." A highly secure system uses Multi-Factor Authentication (MFA), requiring a combination of at least two *different* categories.
  
  1.  **Something you KNOW:**
    *   *Examples:* Passwords, PINs, the answer to a security question ("What is your mother's maiden name?").
    *   *Weakness:* Can be guessed, socially engineered, or stolen via keyloggers.
  2.  **Something you HAVE (Possession):**
    *   *Examples:* A physical smart card, a USB token (like a YubiKey), a mobile phone (receiving an SMS code or using an Authenticator app), or a physical key.
    *   *Weakness:* Can be physically stolen or lost.
  3.  **Something you ARE (Physical Characteristics / Biometrics):**
    *   *Examples:* Fingerprints, facial recognition, iris scans, voiceprints, or even signature dynamics (how hard you press the pen when signing).
    *   *Weakness:* Cannot be easily revoked or changed. If your fingerprint data is stolen from a database, you cannot grow new fingers.
  4.  **Somewhere you ARE / can be reached (Location-Based):**
    *   *Examples:* IP address geolocation, GPS coordinates on a phone, or requiring a user to click a confirmation link sent to a specific email address. 
  
  ---
- ### Study Questions for Part 1
  1. **Mechanism Flaws:** Why is Address-Based Authentication inherently dangerous to use on the public internet?
  2. **Challenge-Response:** How does a Cryptographic Authentication Protocol (like Challenge-Response) defeat a network eavesdropper who is trying to sniff a user's password? 
  3. **MFA Categorization:** A banking application requires you to enter a password and then answer the security question "What was the name of your first pet?" Does this qualify as Two-Factor Authentication (2FA)? Why or why not?
  
  ---