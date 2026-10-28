### **3.3 Connectionless Transport: UDP (User Datagram Protocol)**
  
  UDP is the "bare-bones," "no-frills" transport protocol. It provides the minimum necessary services for process-to-process communication and little else. It is defined in **RFC 768**.
#### **Characteristics of UDP**
  
  *   **"Best-Effort" Service:** UDP tries its best to deliver segments, but it makes no guarantees. Segments may be:
    *   **Lost:** Packets can be dropped in the network and UDP will not try to recover them.
    *   **Delivered out-of-order:** Segments are not numbered, so they may arrive at the destination in a different order than they were sent.
  *   **Connectionless:** There is no "handshake" or connection establishment phase. When an application has data to send, it can immediately create a UDP segment and pass it to the network layer. Each UDP segment is handled independently of all others.
#### **Why Does UDP Exist?**
  
  With TCP providing robust, reliable service, why would any application choose to use the unreliable UDP? There are several key advantages:
  
  1.  **No Connection Establishment Delay:** TCP requires a three-way handshake before any data can be sent, which adds at least one RTT of delay. UDP avoids this, making it ideal for latency-sensitive applications where quick communication is critical (like DNS).
  2.  **Simplicity (No State):** UDP is a stateless protocol. Neither the sender nor the receiver maintains any connection state information (like sequence numbers or window sizes). This reduces overhead and complexity in the end systems.
  3.  **Small Header Size:** The UDP header is only 8 bytes, compared to the TCP header which is 20 bytes or more. This means less overhead per segment.
  4.  **No Congestion Control:** UDP does not have a built-in mechanism to slow down its sending rate when the network is congested. An application can send data as fast as it wants.
    *   **Advantage:** For real-time applications (like live video), the application may prefer to drop a few packets rather than slow down the entire stream. It can tolerate loss better than it can tolerate delay.
    *   **Disadvantage:** A poorly behaved UDP application can flood the network with traffic, causing severe congestion and degrading performance for all other users (including TCP users).
#### **Common Use Cases for UDP**
  
  *   **Streaming Multimedia:** Applications like voice-over-IP (VoIP) and live video streaming are often loss-tolerant but highly sensitive to delay. UDP's lack of retransmissions and congestion control is a benefit here.
  *   **DNS (Domain Name System):** DNS lookups are simple, fast request-response transactions. The overhead of setting up a TCP connection is unnecessary.
  *   **SNMP (Simple Network Management Protocol):** Used for monitoring network devices.
  *   **HTTP/3:** The newest version of the web protocol runs on QUIC, which itself is built on top of UDP. In this case, reliability and congestion control are implemented at the application layer, giving developers more control than TCP allows.
#### **UDP Segment Structure**
  
  The UDP segment consists of a short header and the application data (payload). The header has four 16-bit fields, for a total of 8 bytes.
  
  ```
  +------------------------+------------------------+
  |   Source Port # (16)   | Destination Port # (16)|
  +------------------------+------------------------+
  |      Length (16)       |      Checksum (16)     |
  +------------------------+------------------------+
  |                                                 |
  |               Application Data (payload)        |
  |                                                 |
  +-------------------------------------------------+
  ```
  
  *   **Source Port Number:** Identifies the sending process's socket.
  *   **Destination Port Number:** Identifies the receiving process's socket. This is used for demultiplexing.
  *   **Length:** The length of the entire UDP segment (header + data) in bytes.
  *   **Checksum:** An optional field used for error detection.
#### **UDP Checksum**
  
  The checksum's goal is to detect errors (i.e., flipped bits) that may have occurred during the segment's transmission.
  
  **Sender-Side Calculation:**
  1.  The sender treats the contents of the UDP segment (and a "pseudo-header" containing the source/destination IP addresses) as a sequence of 16-bit integers.
  2.  It sums all of these 16-bit integers using **1's complement arithmetic**.
  3.  In 1's complement addition, any carry-out from the most significant bit must be added back to the result (this is called a "wraparound").
  4.  The final checksum value is the **1's complement** of this sum (i.e., all the bits are flipped). This value is placed in the checksum field.
  
  **Receiver-Side Check:**
  1.  The receiver computes the 1's complement sum of all the 16-bit integers in the received segment, *including the checksum field itself*.
  2.  If no errors have occurred, the result of this sum will be a 16-bit value of all 1s (binary `1111111111111111`).
  3.  If any of the bits are 0, the receiver knows that an error has occurred.
  
  **Example of 1's Complement Addition:**
  
  Let's add two 16-bit numbers:
  `1110011001100110`
  `+ 1101010101010101`
  --------------------
  `1 1011101110111011` -> A carry-out of 1 occurred.
  
  Now, we perform the **wraparound**:
  `1011101110111011`
  `+                1`
  --------------------
  `1011101110111100`  <- This is the sum.
  
  To get the checksum, we take the 1's complement (flip the bits):
  `0100010001000011` <- This value is placed in the checksum field.
  
  **Weakness of the Internet Checksum:**
  The checksum provides a basic level of error detection, but it is not foolproof. It is possible for multiple bit errors to occur in a way that they cancel each other out, resulting in an unchanged checksum. More robust error-checking mechanisms (like CRCs) are used at the link layer.