### The Network Core
  
  The network core is the mesh of interconnected routers that forms the backbone of the internet. Its primary function is to switch and route traffic between different parts of the network. The core is designed to be fast, reliable, and scalable to handle the massive volume of data that traverses it.
#### Packet Switching
  
  Unlike traditional circuit-switched networks that establish a dedicated path for communication, the internet uses packet switching. Here's how it works:
  
  *   **Breaking Down Messages:** When a host sends a message, it's broken down into smaller, manageable chunks called packets.
  *   **Independent Travel:** Each packet is labeled with control information, including the source and destination addresses. These packets then travel independently across the network.
  *   **Reassembly at Destination:** At the receiving end, the packets are reassembled into the original message.
  *   **Efficient Resource Utilization:** This method allows multiple users to share network resources efficiently, as packets can take different routes to avoid congestion and choose faster paths.
#### Store-and-Forward
  
  Packet switches (routers) use a technique called store-and-forward.
  
  *   **Receiving the Entire Packet:** The router must receive the entire packet before it can begin to transmit it on the next link.
  *   **Error Checking:** After storing the packet, the router checks it for errors, typically by verifying a checksum. If the packet is error-free, it is forwarded. If not, it is discarded.
  *   **Latency:** While this process ensures the integrity of the data, it introduces a small delay, known as latency, at each router. The cumulative delay across multiple routers can become significant for real-time applications.
#### Two Key Network Core Functions: Routing and Forwarding
  
  While often used interchangeably, routing and forwarding are distinct but related functions:
  
  *   **Routing (The "Global" Action):** This is the network-wide process of determining the end-to-end paths that packets will take from a source to a destination.
    *   Routing algorithms are used to calculate these paths and create routing tables.
    *   This is considered a control plane function, operating on a longer timescale (minutes or hours).
  *   **Forwarding (The "Local" Action):** This is the local action within a single router of moving an arriving packet from an input link to the appropriate output link.
    *   The router uses a forwarding table (created by the routing process) to make this decision quickly.
    *   This is a data plane function and happens at very high speeds.
  
  Think of it like planning a road trip. **Routing** is like using a map to determine the entire route from your starting city to your destination city. **Forwarding** is like being at a specific intersection (a router) and reading the road signs to decide which turn to take next to stay on your planned route.
#### Packet Queuing and Loss
  
  When a router receives packets faster than it can forward them, queuing occurs.
  
  *   **Waiting in Line:** Arriving packets are placed in a queue (or buffer) to wait for their turn to be transmitted on the output link. Most routers use a First-In, First-Out (FIFO) queuing method by default, where packets are processed in the order they arrive.
  *   **Congestion and Packet Loss:** If the arrival rate of packets continues to exceed the transmission rate, the router's buffer can fill up. When the queue is full, any newly arriving packets will be dropped (lost). This packet loss can trigger retransmissions from the sender, further impacting network performance.