### **8.5 Securing TCP Connections: TLS (Transport Layer Security)**

While we can build security into an application like email, a more powerful approach is to provide a secure channel that *any* application can use. This is the goal of **Transport Layer Security (TLS)**.

*   **What it is:** TLS is a widely deployed security protocol that operates in the application layer but sits logically "on top of" TCP. It provides a secure communication channel for any application that would normally use TCP.
*   **Most Common Use:** Securing web traffic with **HTTPS** (HTTP Secure), which runs over port 443. Almost all modern browsers and web servers support it.
*   **History:** TLS is the successor to the now-deprecated **Secure Sockets Layer (SSL)** protocol. The current modern version is TLS 1.3, defined in RFC 8446.

**Services Provided by TLS**

TLS uses the cryptographic techniques we've studied to provide a comprehensive set of security services:
1.  **Confidentiality:** Achieved via **symmetric encryption** of all application data exchanged between the client and server.
2.  **Integrity:** Achieved via **cryptographic hashing** (using a Message Authentication Code, or MAC) of all application data.
3.  **Authentication:** Achieved via **public key cryptography**, where the client can (and should) verify the identity of the server by checking its digital certificate.

**A High-Level View of a TLS Session**

A complete TLS session consists of several phases:

1.  **Handshake Phase:** This is the initial setup phase where the client and server introduce themselves and establish the secure channel.
  *   They authenticate each other (or at least the client authenticates the server).
  *   They agree on a set of cryptographic algorithms to use (the "cipher suite").
  *   They use public key cryptography to securely create or exchange a set of shared **symmetric keys**. These keys will be used for the actual data transfer.
2.  **Key Derivation:** The client and server use the initial shared secret (the "master secret") to derive a set of four keys:
  *   Encryption and MAC keys for data from client to server.
  *   Encryption and MAC keys for data from server to client.
3.  **Data Transfer Phase:**
  *   The application data is broken into records.
  *   Each record is authenticated with a MAC, then encrypted using the symmetric keys derived in the previous step.
  *   These secure records are then passed down to the TCP layer to be sent over the internet.
4.  **Connection Closure:** Special messages are used to securely terminate the connection.
- #### **The TLS 1.3 Handshake: Optimizing for Speed**
  
  A major goal of the latest TLS version (1.3) was to reduce the latency of the handshake. The standard handshake now completes in just **one Round-Trip Time (1-RTT)**.
  
  1.  **Client Hello (Client → Server):**
    *   The client sends the first message. It includes:
        *   A list of the **cipher suites** it supports.
        *   Its **Diffie-Hellman (DH) key exchange parameters**. The client makes a "guess" about which key exchange algorithm the server will want to use and sends its part of the key exchange information upfront.
  2.  **Server Hello & Certificate (Server → Client):**
    *   The server receives the client's hello. It chooses a cipher suite from the client's list.
    *   It sends back its part of the **DH key agreement**, which allows both sides to independently compute the shared secret.
    *   Crucially, it also sends its **server-signed digital certificate** so the client can authenticate it.
  3.  **Client Finishes and Sends Data:**
    *   The client receives the server's message.
    *   It **checks the server's certificate** to verify its identity.
    *   It uses the server's DH parameters to compute the shared secret and derive the session keys.
    *   At this point, the handshake is complete from the client's perspective. It can **immediately start sending encrypted application data** (e.g., the HTTPS GET request).
  
  This process is much faster than previous versions of TLS, which could take 2 or even 3 RTTs before any application data could be sent.
  
  **TLS 1.3 0-RTT Handshake**
  TLS 1.3 also supports an optional "0-RTT" mode for **resuming** an earlier connection between a client and a server.
  *   **Mechanism:** In the very first `Client Hello` message, the client can include encrypted application data. This data is encrypted using a "resumption master secret" that was established during a previous session.
  *   **Benefit:** This allows the client to send data without waiting for any reply from the server, reducing latency even further.
  *   **Risk:** This mode is vulnerable to **replay attacks**, as an attacker could capture and re-send this initial message. Therefore, it is only safe to use for application requests that do not change the server's state (e.g., idempotent requests like an HTTP GET).