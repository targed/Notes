## I. The OSI Model vs. Internet Protocols (TCP/IP) (Slides 2–4)
To standardize how computers talk to each other, the International Organization for Standardization (ISO) created the **OSI (Open Systems Interconnection) 7-Layer Model**. 

*   **The 7 Layers:**
  1.  **Physical:** Cables, radio waves, raw bits (1s and 0s).
  2.  **Data Link:** MAC addresses, switches, Ethernet.
  3.  **Network:** IP addresses, routers (How data gets from network A to network B).
  4.  **Transport:** TCP/UDP, port numbers (How data gets to the specific application).
  5.  **Session:** Establishing and maintaining communication sessions.
  6.  **Presentation:** Data formatting, encryption, and compression.
  7.  **Application:** HTTP, FTP, SMTP (The software the user interacts with).

**The Reality (Slide 4):**
The internet doesn't strictly use the 7-layer OSI model; it uses the simpler **TCP/IP Model (Internet Protocols)**. 
*   In TCP/IP, the top three OSI layers (Application, Presentation, Session) are mashed together into one big "Application" layer. 
*   *Why this matters for TLS:* Because the "Presentation" and "Session" layers don't natively exist in TCP/IP, there was no built-in security for internet traffic. **SSL/TLS was invented to fill this exact gap.** It sits right between the Application layer (HTTP) and the Transport layer (TCP), effectively acting as a secure "Presentation" layer.
- ## II. What is SSL/TLS? (Slide 5)
  *   **SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** are essentially the same protocol. TLS is simply the modern, standardized successor to SSL.
  *   **The Goal:** To provide privacy (Confidentiality) and data integrity between two communicating applications.
  *   **Ubiquity:** It is the de facto standard for the internet. When you see `https://` or a padlock icon in your browser, you are using TLS. It also secures VoIP, email (SMTPS), and VPNs.
- ## III. The Threat Model & Guarantees (Slide 6)
  What exact threats does TLS protect you from? Cryptographers use a specific threat model (often called the *Dolev-Yao model*) to define the attacker's power.
  
  *   **The Attacker's Power:** We assume the attacker is omnipotent on the network. They own the coffee shop Wi-Fi, they control the ISP routers, they can intercept DNS requests, and they can read, modify, drop, or inject *any* packet they want.
  *   **The Guarantee:** Even against this "god-like" network attacker, TLS guarantees **End-to-End Secure Communications**.
    *   *Confidentiality:* The attacker cannot read the data (it's encrypted).
    *   *Integrity:* The attacker cannot alter the data in transit (MACs prevent tampering).
    *   *Authentication:* You are guaranteed to be talking to the real server, not an imposter (via Certificates/PKI).
  *   **The Limitation:** "End-to-End" means TLS only protects data *while it is in transit on the wire*. If the attacker hacks your physical laptop (e.g., via a keylogger) or hacks the destination web server's database, TLS cannot protect you.
- ## IV. The History of the Protocol (Slide 7)
  Understanding the history helps explain why we use specific versions today.
  *   **SSL 1.0 & 2.0 (1994):** Created by Netscape (the creators of the first major web browser). They were quickly found to have catastrophic cryptographic flaws.
  *   **SSL 3.0 (1996):** A complete redesign by Netscape and cryptographer Paul Kocher. This was the first truly usable version. *(Note: SSL 3.0 was completely deprecated in 2014 due to the "POODLE" vulnerability).*
  *   **TLS 1.0 (1999):** The IETF (Internet Engineering Task Force) took over the protocol from Netscape to make it an open international standard. They changed the name to TLS to avoid the Netscape trademark.
  *   **TLS 1.1 (2006) & TLS 1.2 (2008):** TLS 1.2 became the gold standard for over a decade, introducing highly secure algorithms like AES-GCM and SHA-256.
  *   **TLS 1.3 (2018):** The current, cutting-edge standard. 
    *   *Fill-in info:* TLS 1.3 was a massive upgrade. It completely removed support for old, vulnerable cryptographic algorithms (like MD5, SHA-1, and older RSA key exchanges) and made the initial handshake much faster (requiring fewer round-trips to establish a connection).
  
  ---
- ### Study Questions for Part 1
  1. **OSI vs. TCP/IP:** Why was it necessary to invent a protocol like SSL/TLS to sit between HTTP and TCP, rather than relying on the fundamental TCP/IP stack to provide security?
  2. **The Threat Model:** An attacker successfully installs a rootkit on a bank's backend database server and steals customer records. Does this mean the bank's TLS implementation failed? Why or why not based on the TLS "End-to-End" guarantee?
  3. **Nomenclature:** Are SSL 3.0 and TLS 1.0 vastly different systems, or did the name change primarily represent a shift in who managed the standard?
  
  ---
-