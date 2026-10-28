### **3.5 Connection-oriented Transport: TCP (Transmission Control Protocol)**
  
  TCP is the dominant transport-layer protocol on the internet, providing applications with a reliable, connection-oriented data transfer service. It is defined in a series of RFCs, most notably RFC 793 and RFC 5681.
#### **Overview of TCP Features**
  
  *   **Point-to-Point:** A TCP connection is between a single sender and a single receiver.
  *   **Reliable, In-order Byte Stream:**
    *   TCP provides applications with the abstraction of a continuous stream of bytes, not a series of messages. It does not enforce message boundaries.
    *   It guarantees that the stream of bytes the receiver reads will be identical to the stream the sender wrote, and in the same order.
  *   **Pipelining:** TCP is a pipelined protocol. The size of its sending window is managed by its **congestion control** and **flow control** mechanisms.
  *   **Full-Duplex:** Data can flow in both directions within the same TCP connection.
  *   **Connection-Oriented:** A **three-way handshake** must be completed between the client and server to establish a connection before any application data can be exchanged.
  *   **Flow Controlled:** The sender will not overwhelm the receiver's buffer space.
#### **The TCP Segment Structure**
  
  A TCP segment consists of a header (at least 20 bytes) and a data payload. The header contains critical control information.
  
  ```
  +------------------------+------------------------+
  |   Source Port # (16)   | Destination Port # (16)|
  +-------------------------------------------------+
  |              Sequence Number (32)               |
  +-------------------------------------------------+
  |           Acknowledgement Number (32)           |
  +----+----+--+--+--+--+--+--+---------------------+
  | Hdr|Rsvd|CE|UA|PS|RS|FN|  Receive Window (16)  |
  | Len|    |  |CK|HH|ST|IN|                     |
  +----+----+--+--+--+--+--+--+---------------------+
  |      Checksum (16)     |   Urgent Pointer (16)  |
  +-------------------------------------------------+
  |                 Options (if any)                |
  +-------------------------------------------------+
  |                                                 |
  |               Application Data (payload)        |
  |                                                 |
  +-------------------------------------------------+
  ```
  **Key Header Fields:**
  *   **Source & Destination Port Numbers:** Used for multiplexing and demultiplexing.
  *   **Sequence Number:** This is a **byte-stream number**. It is the number of the *first byte* of data in this segment within the overall stream of data. This is different from the packet-based sequence numbers we saw in the RDT principles.
  *   **Acknowledgement Number (ACK number):** This is the sequence number of the *next byte* the host is expecting to receive. TCP uses **cumulative ACKs**, so if a host sends an ACK number of 100, it is implicitly acknowledging all bytes up to 99.
  *   **Header Length:** Specifies the length of the TCP header in 32-bit words (e.g., a value of 5 means a 20-byte header).
  *   **Flags (U, A, P, R, S, F):** Single-bit flags that control the connection.
    *   `ACK`: Indicates that the Acknowledgement Number field is valid.
    *   `SYN`: Used to initiate a connection.
    *   `FIN`: Used to terminate a connection.
  *   **Receive Window:** Used for **flow control**. This field tells the other side how many bytes of buffer space are currently available for receiving data.
  *   **Checksum:** Used for error detection, calculated over the header, data, and an IP pseudo-header.
#### **TCP Reliable Data Transfer**
  
  TCP's reliability is built upon the principles of `rdt3.0`, but with significant enhancements for performance.
  
  *   **The Problem of Setting a Timeout Value:**
    *   The timeout value for retransmissions must be longer than the RTT, but the RTT is highly variable.
    *   **Too short:** Leads to premature timeouts and unnecessary retransmissions.
    *   **Too long:** Leads to slow reaction to packet loss.
  *   **TCP's Solution: Adaptive Timeouts**
    TCP dynamically estimates the RTT and sets its timeout interval based on this estimate.
    1.  It measures the `SampleRTT` for various segments (the time from when a segment is sent until its ACK is received).
    2.  It calculates a smoothed, average RTT called the `EstimatedRTT` using an **Exponential Weighted Moving Average (EWMA)**.
        $ \text{EstimatedRTT} = (1-\alpha) \cdot \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT} $
        (Typical $\alpha = 0.125$)
    3.  It also calculates the deviation of the RTT (`DevRTT`) to measure its variability.
        $ \text{DevRTT} = (1-\beta) \cdot \text{DevRTT} + \beta \cdot |\text{SampleRTT} - \text{EstimatedRTT}| $
        (Typical $\beta = 0.25$)
    4.  The final **Timeout Interval** is set with a safety margin.
        $ \text{TimeoutInterval} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT} $
  
  *   **TCP Fast Retransmit (A Major Optimization):**
    *   Waiting for a timeout to detect a lost packet can be slow. TCP uses a clever heuristic to detect loss much faster.
    *   **Mechanism:** If a sender receives **three duplicate ACKs** for the same sequence number, it takes this as a strong indication that the segment *after* that acknowledged data was lost.
    *   **Action:** The sender immediately **retransmits the missing segment without waiting for its timer to expire**. This dramatically improves performance in the face of occasional packet loss.
#### **TCP Flow Control**
  
  *   **Goal:** To prevent the sender from sending data faster than the receiver's application can read it from the socket buffer, thus preventing the buffer from overflowing.
  *   **Mechanism:**
    1.  The receiver maintains a receive buffer (`RcvBuffer`).
    2.  The receiver advertises the amount of free space in this buffer in the **Receive Window (`rwnd`)** field of every TCP segment it sends back to the sender.
    3.  The sender tracks the amount of unacknowledged ("in-flight") data it has sent. It ensures that this amount is always less than the last `rwnd` value it received.
        $ \text{LastByteSent} - \text{LastByteAcked} \le \text{rwnd} $
  *   This is a simple but powerful feedback mechanism that lets the receiver dynamically control the sender's rate.
#### **TCP Connection Management**
  
  *   **Establishing a Connection (The Three-Way Handshake):**
    A two-way handshake is insufficient because it can lead to "half-open" connections if messages are delayed and retransmitted. TCP uses a robust three-way handshake.
    1.  **SYN:** The client sends a TCP segment with the `SYN` flag set to 1. It also chooses a random initial sequence number (`client_isn`).
    2.  **SYN-ACK:** The server receives the SYN, allocates buffers for the connection, and sends back a segment with the `SYN` flag set to 1 and the `ACK` flag set to 1. It chooses its own random initial sequence number (`server_isn`) and sets the acknowledgement number to `client_isn + 1`.
    3.  **ACK:** The client receives the SYN-ACK. It allocates its own buffers and sends a final segment back to the server with the `ACK` flag set, acknowledging the server's SYN by setting the acknowledgement number to `server_isn + 1`.
  
  *   **Closing a Connection:**
    Each side of the connection must be closed independently.
    1.  The client (or server) that wants to close the connection sends a segment with the `FIN` flag set.
    2.  The other side receives the `FIN` and sends an `ACK` to acknowledge it.
    3.  The other side then sends its own `FIN` segment.
    4.  The first side acknowledges this second `FIN` with a final `ACK`.