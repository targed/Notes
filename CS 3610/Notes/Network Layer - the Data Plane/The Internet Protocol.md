### **4.3 The Internet Protocol (IP)**
  
  The network layer in the internet is composed of several key components. The **Internet Protocol (IP)** is the main data plane protocol, responsible for addressing and forwarding datagrams. It is supported by other protocols:
  *   **Routing Protocols (Control Plane):** Determine the paths packets take (e.g., OSPF, BGP). They build the forwarding tables that IP uses.
  *   **ICMP (Internet Control Message Protocol):** Used for error reporting and network-level signaling between hosts and routers.
#### **IPv4 Datagram Format**
  
  The IPv4 datagram is the fundamental unit of data at the network layer. It consists of a header and a payload. The header contains all the information a router needs to forward the packet.
  
  ```
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
  |Ver|Hlen| Type of Service |         Total Length (in bytes)         |
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
  |         Identifier      |Flags|       Fragment Offset         |
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
  |   Time To Live (TTL)    | Protocol |         Header Checksum         |
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
  |                           Source IP Address                           |
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
  |                         Destination IP Address                        |
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
  |                    Options (if any) ...                           |
  +--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
  |                               Data (Payload)                          |
  ...
  ```
  
  **Key Header Fields:**
  *   **Version:** `4` for IPv4, `6` for IPv6.
  *   **Header Length:** Length of the IP header in 32-bit words. A typical value is `5` (for a 20-byte header).
  *   **Total Length:** Total length of the entire IP datagram (header + payload) in bytes. Max size is 65,535 bytes.
  *   **Time To Live (TTL):** An 8-bit field that is **decremented by one at every router**. If the TTL reaches zero, the packet is discarded. This prevents packets from getting stuck in infinite routing loops.
  *   **Protocol:** This field tells the destination host's network layer which transport protocol to pass the payload to. `6` = TCP, `17` = UDP. This is the key to network-layer demultiplexing.
  *   **Header Checksum:** A value calculated only over the IP header. It is used to detect bit errors in the header. It must be **recalculated at every router** because the TTL field changes.
  *   **Source and Destination IP Addresses:** The 32-bit addresses of the sending and receiving interfaces.
  *   **Fragmentation Fields (Identifier, Flags, Fragment Offset):** If a datagram is larger than the **Maximum Transmission Unit (MTU)** of an outgoing link, a router must fragment it into smaller datagrams. These fields are used by the destination host to reassemble the original datagram.
  
  **Overhead:** The standard overhead for a TCP segment is **40 bytes**: 20 bytes for the IP header + 20 bytes for the TCP header.
#### **IP Addressing and Subnets**
  
  *   **IP Address:** A 32-bit identifier for a host or router **interface**. A router has multiple interfaces, each with its own IP address.
  *   **Subnet (Subnetwork):** A group of device interfaces that can physically reach each other without passing through an intervening router.
  *   **Structure of an IP Address:** An IP address is divided into two parts: a **subnet part** (or network prefix) and a **host part**.
    *   The subnet part identifies the network the device is on. It consists of the high-order bits of the address.
    *   The host part identifies the specific device on that network. It consists of the low-order bits.
  
  *   **CIDR (Classless Inter-Domain Routing):** The modern notation for representing the subnet/host split.
    *   **Format:** `a.b.c.d/x`, where `x` is the number of bits in the subnet part.
    *   **Example:** In the address `200.23.16.1/23`, the first 23 bits (`200.23.16.0`) represent the subnet, and the remaining 9 bits represent the host.
#### **Obtaining IP Addresses: DHCP and ISPs**
  
  There are two main questions when it comes to getting an IP address:
  
  1.  **How does a HOST get an IP address?**
    *   **DHCP (Dynamic Host Configuration Protocol):** The most common method. It allows a host to "plug-and-play" and automatically obtain an address from a DHCP server on the network.
    *   **The DHCP Four-Step Process (D-O-R-A):**
        1.  **Discover:** A new host broadcasts a `DHCP Discover` message to find a DHCP server.
        2.  **Offer:** A DHCP server responds with a `DHCP Offer` message, offering an IP address.
        3.  **Request:** The host formally requests the offered address with a `DHCP Request` message.
        4.  **ACK:** The server confirms the assignment with a `DHCP ACK` message.
    *   DHCP provides more than just an IP address; it also provides the host with its **subnet mask**, the IP address of its **first-hop router (default gateway)**, and the IP address of its **local DNS server**.
  
  2.  **How does a NETWORK get its block of addresses?**
    *   Networks get their address blocks from their **Internet Service Provider (ISP)**.
    *   The ISP, in turn, gets its larger address blocks from a **Regional Internet Registry (RIR)**.
    *   The RIRs get their address blocks from **ICANN (Internet Corporation for Assigned Names and Numbers)**, the global authority.
    *   This hierarchical allocation allows for **route aggregation**. An ISP can advertise a single, large prefix (e.g., `200.23.16.0/20`) to the global internet, which summarizes all the smaller address blocks assigned to its customers. This keeps the size of global routing tables manageable.
#### **Network Address Translation (NAT)**
  
  *   **Motivation:** A strategy to mitigate the exhaustion of the 32-bit IPv4 address space.
  *   **Core Idea:** All devices in a local network (like a home network) share a **single public IP address**. Within the local network, devices use **private IP addresses** (from ranges like `10.0.0.0/8` or `192.168.0.0/16`) that are not routable on the public internet.
  
  *   **How NAT Works:** The NAT-enabled router acts as a translator.
    1.  **Outgoing Datagram:** When a host (e.g., `10.0.0.1`) sends a datagram to the outside world, the NAT router replaces the private source IP (`10.0.0.1`) and source port (`3345`) with its own public IP (`138.76.29.7`) and a new, arbitrary port (`5001`).
    2.  **NAT Translation Table:** The router stores this mapping (`10.0.0.1, 3345` <-> `138.76.29.7, 5001`) in its translation table.
    3.  **Incoming Datagram:** When a reply comes back from the server, it is addressed to the router's public IP and the new port (`138.76.29.7, 5001`).
    4.  The router looks this pair up in its table, finds the corresponding private IP and port, and rewrites the destination fields of the datagram to (`10.0.0.1, 3345`) before forwarding it into the local network.
#### **IPv6**
  
  *   **Primary Motivation:** The 32-bit IPv4 address space is exhausted. IPv6 uses **128-bit addresses**, providing a virtually limitless supply.
  *   **Additional Motivations:**
    *   **Simplified Header:** The IPv6 header is a fixed 40-bytes long. This simplifies and speeds up router processing.
    *   **No Fragmentation:** IPv6 routers are not allowed to fragment packets. If a packet is too large, the router drops it and sends an ICMP error message back to the sender. This pushes the work of figuring out the path MTU to the end hosts.
    *   **No Header Checksum:** This was removed to reduce processing overhead at routers, as link-layer technologies are now highly reliable.
  
  *   **Transition from IPv4 to IPv6: Tunneling**
    *   Since not all routers can be upgraded to IPv6 at once, a mechanism is needed for IPv6 datagrams to travel through IPv4-only sections of the internet.
    *   **Tunneling:** An entire IPv6 datagram is placed inside the **payload field** of an IPv4 datagram.
    *   The IPv4 datagram is addressed to the entry and exit points of the "tunnel." When the packet reaches the end of the tunnel, the IPv4 header is stripped off, and the original IPv6 datagram is forwarded on its way.