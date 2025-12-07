### **Chapter 8: Security in Computer Networks**
- #### **8.1 What is Network Security?**
  
  Network security is the field concerned with how to protect information and services from attack, damage, or unauthorized access as they are transmitted over a network. It encompasses a broad set of techniques and protocols designed to provide specific guarantees to the communicating parties.
  
  **The Four Core Properties of Secure Communication**
  
  1.  **Confidentiality:**
    *   **Goal:** To ensure that only the sender and the intended receiver can "understand" the contents of a transmitted message.
    *   **Mechanism:** This is achieved through **encryption**. The sender encrypts the message (scrambles it), and only the intended receiver has the key to decrypt it (unscramble it). This prevents an eavesdropper from reading the message's contents.
  
  2.  **Authentication:**
    *   **Goal:** To enable the sender and receiver to confirm each other's identity. It answers the question, "How do I know I'm really talking to who I think I'm talking to?"
    *   **Mechanism:** This is achieved through protocols that allow one party to prove they know a secret (like a password or a private key) without necessarily revealing the secret itself.
  
  3.  **Message Integrity:**
    *   **Goal:** To ensure that a message has not been altered (either accidentally or maliciously) during transit.
    *   **Mechanism:** This is typically achieved using cryptographic **hash functions** or message authentication codes (MACs). The sender computes a digital "fingerprint" of the message, and the receiver can verify that the message it received produces the exact same fingerprint.
  
  4.  **Access and Availability:**
    *   **Goal:** To ensure that network services are accessible and available to legitimate users.
    *   **Mechanism:** This involves protecting services from attacks that aim to make them unusable, such as **Denial of Service (DoS)** attacks.
  
  **Friends and Enemies: The Cast of Characters**
  
  To make discussions about security protocols clearer, the field uses a standard set of characters:
  *   **Alice and Bob:** The two legitimate parties who want to communicate securely. They can be real people, but in networking, they often represent processes like a web browser (Alice) and a web server (Bob), or two BGP routers exchanging updates.
  *   **Trudy (the Intruder):** The adversary or "bad guy" who wants to attack the communication between Alice and Bob. Trudy has control over the network between them.
  
  **The Threat Model: What Can Trudy Do?**
  
  Trudy, by controlling the network, can perform a wide range of attacks. It is against these threats that we must design our security protocols.
  
  *   **Eavesdropping:** Trudy can intercept and make copies of all messages sent between Alice and Bob. This attack violates **confidentiality**.
  *   **Insertion / Modification:** Trudy can actively insert new, fake messages into the connection or modify the contents of legitimate messages. This violates **message integrity**.
  *   **Impersonation (Spoofing):** Trudy can fake the source address in a packet to pretend to be Alice when communicating with Bob. This attack violates **authentication**.
  *   **Hijacking:** Trudy can "take over" an ongoing connection by removing the sender or receiver and inserting herself in the middle.
  *   **Denial of Service (DoS):** Trudy can prevent Bob's service from being used by legitimate users by flooding it with a massive amount of bogus traffic, overwhelming its resources. This violates **availability**.