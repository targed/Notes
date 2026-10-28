### **3.2 Multiplexing and Demultiplexing**
#### **The Core Problem**
  
  A host on the internet (like your computer or a server) can be running many different network applications simultaneously. For example, you might have a web browser, a video streaming app, and a video game all open at the same time. All incoming data for these different applications arrives at the host over a single network connection (e.g., your Wi-Fi).
  
  The key question is: When a data segment arrives at the host, how does the operating system know which application process to deliver it to? How does it know to give the web data to the browser and not the video game?
  
  This is the job of **demultiplexing**. Its counterpart, **multiplexing**, handles the data on the sending side.
#### **Definitions**
  
  *   **Multiplexing (at the Sender):** The process of gathering data chunks from different application sockets, encapsulating each with a transport-layer header (which adds the source and destination port numbers), and then passing the resulting segments to the network layer.
    *   **Analogy:** This is like the mailroom clerk (the transport layer) collecting letters from many different employees (sockets/processes) and putting each one into a separate, correctly addressed envelope before handing them all over to the postal service (the network layer).
  
  *   **Demultiplexing (at the Receiver):** The process of receiving segments from the network layer and using the information in the transport-layer header to deliver each segment to the correct application socket.
    *   **Analogy:** This is like the receiving mailroom clerk looking at the "To:" address on each envelope and delivering it to the correct employee's desk (socket). The highway signs (IP addresses) got the mail truck to the right building, but the specific employee name on the envelope (the port number) is needed for the final delivery.
#### **How Demultiplexing Works: Sockets and Port Numbers**
  
  The mechanism for demultiplexing relies entirely on the header fields in each transport-layer segment.
  
  1.  A host receives an IP datagram.
  2.  Inside each IP datagram is one transport-layer segment (either TCP or UDP).
  3.  Each segment has a **source port number** and a **destination port number**.
  4.  The receiving host uses the IP addresses from the network-layer header and the port numbers from the transport-layer header to direct the segment to the appropriate socket.
  
  A socket is uniquely identified by this set of values. The exact values used for identification depend on whether the protocol is connectionless (UDP) or connection-oriented (TCP).
#### **Connectionless Demultiplexing (UDP)**
  
  *   **How it Works:** When an application creates a UDP socket, it specifies a port number for that socket to "listen" on.
  *   When a UDP segment arrives at the host, the operating system looks *only* at the **destination port number** in the segment.
  *   It then delivers the segment to the