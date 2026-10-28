### **3.6 Principles of Congestion Control**
#### **The Problem of Congestion**
  
  Congestion occurs when **too many sources are sending too much data too fast for the network to handle.** This is a network-wide problem.
  
  *   **Manifestations:** When a network is congested, two things happen:
    1.  **Long Delays:** Packets get stuck in router queues (buffers) for long periods, leading to high latency.
    2.  **Packet Loss:** Router queues are finite. When a queue is full and a new packet arrives, the router has no choice but to drop it. This is called buffer overflow.
  
  *   **Costs of Congestion:**
    *   **Decreased Throughput:** When a packet is lost, the sender must retransmit it. This retransmission is wasted work; the bandwidth used to send the original packet was for nothing. As congestion increases, the effective throughput (the rate of useful data delivery) can actually decrease.
    *   **Wasted Upstream Bandwidth:** If a packet is successfully transmitted across several links but is then dropped at a congested router downstream, all the bandwidth it consumed on the upstream links was wasted.
#### **Congestion Control vs. Flow Control**
  
  It is absolutely essential to understand the difference between these two mechanisms.
  *   **Flow Control:** An **end-to-end** mechanism that addresses the problem of a **fast sender overwhelming a slow receiver**. The receiver tells the sender how much buffer space it has. It is a one-on-one interaction.
    *   **Analogy:** You are pouring water (data) into a funnel (the receiver's buffer) held by a friend. Flow control is you watching the funnel and slowing down your pouring so it doesn't overflow.
  *   **Congestion Control:** A mechanism that addresses the problem of **senders collectively overwhelming the network** (the routers). The network itself signals congestion (implicitly or explicitly), and the senders react by slowing down.
    *   **Analogy:** You and hundreds of other people are all trying to merge onto a single highway lane (a router). Congestion control is everyone slowing down to prevent a massive traffic jam.
#### **Approaches to Congestion Control**
  
  There are two general approaches to how the network signals congestion to the end systems.
  
  1.  **End-to-End Congestion Control:**
    *   **How it works:** The network provides **no explicit feedback** to the end systems.
    *   The end systems must **infer** that congestion is occurring by observing network behavior.
    *   **Inference signals:**
        *   **Packet Loss:** A lost packet (detected by a timeout or duplicate ACKs) is treated as a primary indicator of congestion.
        *   **Increased Delay:** A rising Round-Trip Time (RTT) can also indicate that queues are building up in the network.
    *   **This is the approach taken by TCP.** TCP assumes that any loss event is caused by congestion and reacts accordingly.
  
  2.  **Network-Assisted Congestion Control:**
    *   **How it works:** Network routers provide **explicit, direct feedback** to the senders about the state of congestion.
    *   **Feedback Mechanisms:**
        *   A router can send a special "choke" packet directly back to a sender.
        *   A router can mark a bit in the header of a packet as it passes through. The receiver sees this mark and signals it back to the sender in its ACK. This is called **Explicit Congestion Notification (ECN)** and is an optional feature used by TCP.
        *   A router can explicitly tell the sender what transmission rate it should use.
  
  While network-assisted control can be more precise, the end-to-end approach used by TCP has proven to be incredibly robust and is the foundation of the internet's stability.