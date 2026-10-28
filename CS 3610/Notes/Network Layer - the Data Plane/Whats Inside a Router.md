### **4.2 What's Inside a Router?**
  
  A router's primary purpose is to switch packets from an input link to an output link. To understand how it accomplishes this at incredible speeds, we need to look at its high-level architecture, which is composed of four main components.
#### **Router Architecture Overview**
  
  A router is divided into two main functional planes:
  
  1.  **Data Plane (Forwarding Plane):**
    *   **Function:** Handles the forwarding of datagrams from input ports to output ports.
    *   **Implementation:** Almost always implemented in **hardware** for speed.
    *   **Timescale:** Operates on a per-packet basis, in **nanoseconds**.
  
  2.  **Control Plane (Management Plane):**
    *   **Function:** Executes the routing protocols (e.g., OSPF, BGP) to build the forwarding table, responds to management requests, and handles other "thinking" tasks.
    *   **Implementation:** Implemented in **software** on the **routing processor** (the router's CPU).
    *   **Timescale:** Operates on a slower timescale, in **milliseconds or seconds**.
  
  These four components are:
  *   Input Ports
  *   Switching Fabric
  *   Output Ports
  *   Routing Processor
  
  The first three components belong to the data plane and are optimized for speed.
#### **1. Input Port Functions**
  
  An input port is the first place a packet arrives. It has several functions that must be performed at "line speed" (the arrival rate of the link).
  
  1.  **Physical Layer:** The physical termination of the link. It receives the bits from the physical medium.
  2.  **Link Layer:** Decapsulates the packet. For example, it removes the Ethernet header and trailer to extract the IP datagram.
  3.  **Network Layer (Lookup, Forwarding, and Queuing):** This is the core data plane function.
    *   The port must look up the destination IP address of the arriving datagram in its local copy of the **forwarding table**.
    *   The forwarding table provides the destination output port.
    *   This "match plus action" (match the address, send to output port) decision must be made in nanoseconds.
    *   The datagram is then sent to the switching fabric.
  
  **Longest Prefix Matching**
  Modern forwarding tables do not contain an entry for every possible destination IP address. Instead, they contain prefixes. A router must find the entry that provides the most specific match for a destination address.
  
  *   **Rule:** When looking for a forwarding table entry for a given destination address, use the **longest address prefix** that matches the destination.
  *   **Example:** Consider this forwarding table:
    | Prefix Match | Link Interface |
    | :--- | :--- |
    | `11001000 00010111 00010...` | 0 |
    | `11001000 00010111 00011000` | 1 |
    | `11001000 00010111 00011...` | 2 |
  
    *   **Case 1: Destination `11001000 00010111 00011000 10101010`**
        *   This address matches prefix 1 (`...00011000`).
        *   It also matches prefix 2 (`...00011...`).
        *   The **longest match** is prefix 1 (24 bits vs 21 bits).
        *   **Action:** Forward the packet to **Link Interface 1**.
    *   **Hardware Implementation:** This high-speed lookup is performed using specialized, fast memory called **Ternary Content Addressable Memory (TCAM)**.
  
  **Input Port Queuing and HOL Blocking**
  If the switching fabric is not fast enough to handle all arriving packets, queues can form at the input ports. This can lead to a serious performance problem:
  
  *   **Head-of-the-Line (HOL) Blocking:** A queued datagram at the front of a queue is prevented from moving forward because its designated output port is busy. This **blocks all the datagrams behind it**, even if their output ports are free.
    *   **Analogy:** A single-lane road where the first car wants to turn left but must wait for oncoming traffic. All cars behind it are stuck, even those that want to go straight or turn right.
#### **2. Switching Fabric**
  
  The switching fabric is the heart of the router. It is a network inside the router that connects the input ports to the output ports. There are three major types:
  
  1.  **Switching via Memory (First Generation):**
    *   Packets are copied from an input port into the main system memory (RAM) under the control of the CPU. The CPU looks up the destination and copies the packet to the appropriate output port's buffer.
    *   **Bottleneck:** The speed is limited by the memory bandwidth. Each packet must cross the system bus twice.
  
  2.  **Switching via a Bus:**
    *   The input ports are connected to the output ports via a shared, high-speed bus.
    *   An input port transfers a packet directly to an output port over this bus.
    *   **Bottleneck:** The bus is a shared resource. Only one packet can be transferred at a time, leading to **bus contention**.
  
  3.  **Switching via an Interconnection Network (Crossbar):**
    *   A sophisticated network of switch points (a "crossbar") that can connect any input port to any output port simultaneously, as long as they are destined for different output ports.
    *   This architecture is **non-blocking** and highly parallel, enabling massive switching rates (terabits per second in modern routers).
#### **3. Output Port Functions**
  
  The output port receives packets from the switching fabric, stores them, and transmits them onto the outgoing link.
  
  1.  **Buffering (Output Port Queuing):** Queuing is required when packets arrive from the switching fabric faster than the output link can transmit them. This is a very common scenario and is the primary location for packet loss due to congestion in a router.
  2.  **Buffer Management:** When the output buffer is full, a **drop policy** decides which packet to drop. The most common policy is **tail drop** (drop the newly arriving packet).
  3.  **Packet Scheduling:** The **scheduling discipline** decides which packet to send next from the queue.
    *   **First-Come, First-Served (FCFS/FIFO):** The simplest policy. Packets are transmitted in the order they arrive.
    *   **Priority Scheduling:** Packets are classified into priority classes. The scheduler always sends a packet from the highest-priority queue that has packets. This can lead to starvation of low-priority traffic.
    *   **Round Robin (RR) and Weighted Fair Queuing (WFQ):** These are more "fair" scheduling disciplines. The scheduler cycles through the queues, sending one packet (RR) or a weighted number of packets (WFQ) from each class in turn. WFQ can be used to provide minimum bandwidth guarantees to certain classes of traffic.
  
  4.  **Link and Physical Layer:** The output port performs the necessary encapsulation (e.g., adding an Ethernet header) and then transmits the bits onto the link.