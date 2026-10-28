### Socket Programming
  
  Socket programming is the process of creating network applications that communicate with each other. It involves using a programming interface, or API, to create and manage sockets.
#### The Goal and the Analogy
  
  *   **Goal:** To learn how to build client/server applications that communicate using sockets.
  *   **The Socket:** A socket is the interface (the "door") between an application process and the end-to-end transport protocol (TCP or UDP). It allows the application to send messages into the network and receive messages from it.
  
  There are two primary types of sockets, corresponding to the two main transport services:
  
  1.  **UDP Sockets:** Use the unreliable, connectionless datagram service.
  2.  **TCP Sockets:** Use the reliable, connection-oriented byte-stream service.
### Socket Programming with UDP
  
  Programming with UDP is generally simpler because it is **connectionless**.
  
  *   **No Handshake:** There is no initial connection setup required before data is sent. It's like sending a postcard: you write the destination address on it and drop it in the mail, without first establishing that the recipient is ready to receive it.
  *   **Explicit Addressing:** Because there is no connection, the sender must explicitly attach the destination IP address and port number to *every single packet* it sends.
  *   **Data Boundaries:** UDP provides an unreliable transfer of groups of bytes called "datagrams." The data is sent and received in discrete chunks.
  *   **Unreliability:** Transmitted data may be lost or arrive out of order. The application must handle this if it's a concern.
#### UDP Client/Server Interaction Flow
  
  *   **Server:**
    1.  Creates a UDP socket (`socket()`).
    2.  **Binds** the socket to a specific port number (`bind()`). This is how clients know where to send their messages.
    3.  Waits to receive a datagram from a client (`recvfrom()`). This is a blocking call; the server process will wait here until a packet arrives.
    4.  When a datagram arrives, `recvfrom()` returns the data *and* the source client's IP address and port number.
    5.  The server processes the data and sends a reply back to the client using `sendto()`, specifying the client's address that it just received.
  
  *   **Client:**
    1.  Creates a UDP socket.
    2.  Does *not* need to bind, as the OS will assign a temporary port number.
    3.  Creates a datagram, attaching the server's IP address and port number.
    4.  Sends the datagram into its socket using `sendto()`.
    5.  Waits for a reply using `recvfrom()`.
### Socket Programming with TCP
  
  Programming with TCP is more complex because it is **connection-oriented**.
  
  *   **Connection Setup:** Before any data can be exchanged, a connection must be established between the client and server (the "three-way handshake"). It's like making a phone call: you must dial and have the other person pick up before you can start talking.
  *   **Welcoming vs. Connection Sockets:** The TCP server uses two types of sockets:
    1.  A **welcoming socket** that listens on a well-known port for new connection requests from clients.
    2.  A new **connection socket** that is created specifically for each client that successfully connects. All subsequent communication with that particular client happens over this new, dedicated socket. This allows a single server to communicate with multiple clients simultaneously.
  *   **Reliable Stream:** TCP provides a reliable, in-order "byte-stream" service. It's like having a pipe between the client and server; bytes sent in one end are guaranteed to arrive in the same order at the other end. The application doesn't have to worry about packet boundaries.
#### TCP Client/Server Interaction Flow
  
  *   **Server:**
    1.  Creates the welcoming TCP socket (`socket()`).
    2.  Binds the socket to a well-known port (`bind()`).
    3.  Places the socket in listening mode to await connection requests (`listen()`).
    4.  Calls `accept()`. This is a blocking call that waits for a client to initiate a connection.
    5.  When a client connects, `accept()` creates a *new* connection socket for that client and returns it.
    6.  The server can then use `recv()` and `send()` on this new socket to communicate with the client.
    7.  After the conversation is done, the server closes the *connection* socket (`close()`). The welcoming socket remains open, ready to `accept()` another client.
  
  *   **Client:**
    1.  Creates a TCP socket.
    2.  Calls `connect()`, providing the server's IP address and the welcoming port number. This initiates the three-way handshake.
    3.  Once the connection is established, the client can use `send()` and `recv()` on its socket to communicate with the server. There is no need to attach the destination address to each message because the connection is already established.
    4.  When finished, the client closes its socket (`close()`).