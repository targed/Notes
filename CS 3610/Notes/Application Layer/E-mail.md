### E-mail
  
  Email is one of the oldest and most fundamental internet applications. Its architecture consists of three major components: User Agents, Mail Servers, and the Simple Mail Transfer Protocol (SMTP).
#### 1. User Agents (UA)
  
  The User Agent is the application that users directly interact with to manage their email.
  
  *   **Function:** It is essentially a "mail reader" that allows users to compose, edit, and read email messages.
  *   **Examples:** Common user agents include desktop clients like Microsoft Outlook and Apple Mail, as well as mobile clients like the Mail app on an iPhone.
  *   **Interaction with Servers:** The UA retrieves incoming messages from a mail server and sends outgoing messages to a mail server.
#### 2. Mail Servers
  
  Mail servers are the workhorses of the email system. They are always-on systems that store and forward messages.
  
  *   **Core Components:**
    *   **Mailbox:** Each user has a mailbox on their mail server where incoming messages are stored and wait to be retrieved by the user's UA.
    *   **Message Queue:** When a user sends an email, it is first placed in a message queue on their mail server. The server then attempts to transmit the message to the recipient's mail server. If the recipient's server is down, the sending server will retry periodically.
  *   **The Role of SMTP:** The communication protocol used *between mail servers* to send and receive email messages is SMTP. When one mail server sends a message to another, the sending server acts as the SMTP client, and the receiving server acts as the SMTP server.
#### 3. SMTP (Simple Mail Transfer Protocol)
  
  SMTP is the application-layer protocol for **sending (pushing)** email messages.
  
  *   **Transport Protocol:** SMTP uses **TCP** on **port 25** to ensure reliable transfer of email messages from the client (sending mail server) to the server (receiving mail server).
  *   **Transfer Process:** The transfer occurs in three phases:
    1.  **Handshaking:** The client establishes a TCP connection and performs a greeting with the server.
    2.  **Message Transfer:** The client sends the email message (or multiple messages).
    3.  **Closure:** The connection is closed.
  *   **Command/Response Interaction:** Like HTTP, SMTP uses a simple, human-readable ASCII text format for its commands (e.g., `HELO`, `MAIL FROM`, `RCPT TO`) and server responses (status codes and phrases, e.g., `250 OK`).
#### Comparison: SMTP vs. HTTP
  
  While both are text-based protocols, they have a key conceptual difference:
  
  *   **Push vs. Pull:** SMTP is primarily a **push protocol**. The sending mail server (client) *pushes* the message to the receiving mail server. HTTP is a **pull protocol**; the client *pulls* information from the server.
  *   **Message Format:** HTTP encapsulates each object in its own response message. SMTP can place multiple objects (e.g., attachments) into one message using a multipart message format.
#### Mail Message Format vs. SMTP Protocol
  
  It is important to distinguish between the SMTP protocol commands and the email message format itself.
  
  *   **SMTP Protocol:** The `MAIL FROM` and `RCPT TO` commands are part of the SMTP "envelope," used by the mail servers to route the message.
  *   **Mail Message Format (RFC 2822):** The email message that the user sees contains a header (with `To:`, `From:`, `Subject:` lines) and a body. This entire message is treated as the data payload during the SMTP transfer.
#### Retrieving Email: Mail Access Protocols
  
  SMTP's job ends once the message is successfully delivered to the recipient's mail server. A different type of protocol is needed for the user agent to **retrieve** the messages from the server's mailbox.
  
  *   **IMAP (Internet Mail Access Protocol):** This is a modern and powerful mail access protocol.
    *   **Key Feature:** Messages are stored and managed on the server. The user agent can view, delete, and organize messages into folders, and these changes are reflected on the server.
    *   **Benefit:** This allows a user to access their email from multiple devices (phone, laptop, etc.) and see a consistent, synchronized view of their mailbox.
  *   **HTTP for Webmail:** Web-based email services like Gmail and Hotmail use **HTTP** as the protocol between the user's browser (the user agent) and the mail server. However, in the background, these services still use **SMTP** to send mail to other mail servers across the internet and an IMAP-like protocol to manage the user's mailbox on their servers.