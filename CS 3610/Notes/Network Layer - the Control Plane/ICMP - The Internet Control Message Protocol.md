### **5.6 ICMP: The Internet Control Message Protocol**
  
  The main job of the IP protocol is simply to forward datagrams from source to destination. By design, IP provides a "best-effort" service and does not have built-in mechanisms to report problems or signal information between hosts and routers.
  
  **The Internet Control Message Protocol (ICMP)** was created to fill this gap.
  
  *   **Purpose:** ICMP is used by hosts and routers to communicate network-layer information with each other.
  *   **Primary Use:** The most common use of ICMP is for **error reporting**. For example, if a router cannot find a path to a destination or if a packet's TTL expires, it uses ICMP to send an error message back to the original source.
  *   **Encapsulation:** Although ICMP messages are about the network layer, they are not sent as raw packets. An ICMP message is created and then encapsulated **inside an IP datagram** just like a TCP or UDP segment would be. However, it is still considered a network-layer protocol because its messages are processed by the network layer, not passed up to a transport protocol.
#### **Common ICMP Message Types**
  
  ICMP messages are identified by a `type` and `code` field in their header. Some of the most well-known types include:
  
  *   **Destination Unreachable (Type 3):**
    *   Sent by a router when it receives a datagram that it cannot forward to its destination.
    *   The `code` field specifies the reason, such as:
        *   `Code 0`: Network Unreachable
        *   `Code 1`: Host Unreachable
        *   `Code 2`: Protocol Unreachable (e.g., the destination host doesn't have a process running UDP)
        *   `Code 3`: Port Unreachable
  
  *   **Time Exceeded (Type 11):**
    *   Sent by a router when it receives a datagram and, after decrementing the TTL, the TTL value becomes zero.
    *   The router discards the original datagram and sends this ICMP message back to the source. This is the fundamental mechanism that allows the `traceroute` program to work.
  
  *   **Echo Request (Type 8) and Echo Reply (Type 0):**
    *   These are used by the `ping` utility to check for connectivity and measure the round-trip time to a destination host.
    *   A host sends an `Echo Request` to a destination.
    *   If the destination is reachable, its network layer will reply with an `Echo Reply`.
  
  *   **Parameter Problem (Type 12):** Indicates an error in the IP header.
  *   **Source Quench (Type 4 - Deprecated):** Was once used for a rudimentary form of congestion control but is no longer used.
  *   **Redirect (Type 5):** Sent by a router to inform a host on the same local network of a better first-hop router for a specific destination.
#### **ICMP in Action: The `traceroute` Program**
  
  The `traceroute` (or `tracert` on Windows) program is a classic network diagnostic tool that uses ICMP to map the path a packet takes from a source to a destination.
  
  *   **Goal:** To identify every router on the path and to measure the RTT to each of those routers.
  
  *   **How it Works:**
    1.  **First Hop:** The source host sends a series of UDP segments (typically three) to the final destination, but with a **TTL field set to 1**.
    2.  The first router on the path receives these datagrams. It decrements the TTL to 0.
    3.  Because the TTL is now zero, the router **discards the datagrams**.
    4.  The router then generates three **ICMP "Time Exceeded" (Type 11)** messages and sends them back to the original source.
    5.  The source host receives these ICMP replies, records the source IP address of the router that sent them (identifying the first hop), and measures the time elapsed since sending the UDP segments (the RTT to the first hop).
  
    6.  **Second Hop:** The source host repeats the process, but this time it sends three UDP segments with the **TTL set to 2**.
    7.  The first router decrements the TTL to 1 and forwards the packets.
    8.  The *second* router on the path receives the datagrams, decrements the TTL to 0, discards them, and sends back three ICMP "Time Exceeded" messages.
    9.  The source host now identifies the second hop and measures the RTT to it.
  
    10. **And so on...:** This process continues, incrementing the TTL by one each time, until one of two things happens:
        *   The UDP segments finally reach the destination host. Since the destination port is an unlikely, unused port, the host's operating system will send back **ICMP "Destination Port Unreachable" (Type 3, Code 3)** messages. When the source receives these, it knows it has reached the end of the path and the trace is complete.
        *   The source receives no reply from a router (indicated by `* * *` in the output), which means the probe packets were lost or a router is configured not to send ICMP replies.