### **6.4 Switched Local Area Networks (LANs)**
- #### **Recap: Two Types of Addresses**
  
  To understand how LANs work, we must be clear about the two different addresses every device on a LAN has:
  
  1.  **IP Address (Layer 3 - Network Layer):**
    *   A 32-bit (IPv4) logical address (e.g., `192.168.1.100`).
    *   **Function:** Used for delivering datagrams from the original source host to the final destination host, potentially across many networks (the internet).
    *   **Scope:** Global (or at least network-wide).
    *   **Portability:** Not portable. An IP address is tied to the subnet a device is on. If you move your laptop to a new network, you must get a new IP address.
  
  2.  **MAC Address (Layer 2 - Link Layer):**
    *   A 48-bit "physical" address, usually written in hexadecimal (e.g., `1A-2F-BB-76-09-AD`).
    *   **Function:** Used for delivering a **frame** from one interface to another interface on the **same physical network (subnet)**.
    *   **Scope:** Local to the current link/subnet.
    *   **Portability:** Portable. The MAC address is burned into the Network Interface Card (NIC) ROM by the manufacturer and stays with the device wherever it goes.
    *   **Analogy:** A MAC address is like a person's Social Security Number (permanent and unique), while an IP address is like their postal address (changes when they move).
- #### **ARP (Address Resolution Protocol)**
  
  ARP is the critical utility protocol that translates between Layer 3 and Layer 2 addresses. It answers the fundamental question:
  
  > "I need to send a datagram to a host with IP address `137.196.7.14`. What is the MAC address of the interface with that IP?"
  
  *   **ARP Table:** Each host and router on a LAN maintains an **ARP table** in its memory. This table is a cache that maps IP addresses to their corresponding MAC addresses for nodes on the same subnet.
    *   **Entry Format:** `< IP address; MAC address; TTL >`
    *   **TTL (Time To Live):** A timestamp indicating when the entry will expire (typically around 20 minutes). This ensures that mappings don't become stale.
  
  *   **How ARP Works (When an address is NOT in the table):**
    1.  Host A wants to send a datagram to Host B on the same LAN, but B's MAC address is not in A's ARP table.
    2.  Host A creates an **ARP query** packet. This packet says, "Who has the IP address `[B's IP]`?"
    3.  Host A puts this ARP query inside a link-layer frame. The destination MAC address for this frame is the special **broadcast address: `FF-FF-FF-FF-FF-FF`**.
    4.  All nodes on the LAN receive and process this broadcast frame.
    5.  Host B sees that the IP address in the ARP query matches its own. It prepares an **ARP response** packet, which contains its MAC address.
    6.  Host B sends this ARP response in a frame addressed directly to Host A's MAC address (which it learned from the original ARP query). This is a **unicast** frame.
    7.  Host A receives the ARP response and adds the new IP-to-MAC mapping to its ARP table. It can now send its original IP datagram to Host B.
- #### **Ethernet**
  
  Ethernet is the **dominant wired LAN technology** in the world. It has evolved dramatically over the decades, but its core principles remain.
  
  *   **Physical Topology:**
    *   **Bus (Obsolete):** Early Ethernet used a coaxial cable where all nodes were connected to the same shared bus. All nodes were in the same **collision domain**, meaning they used the CSMA/CD multiple access protocol to coordinate access.
    *   **Star / Switched (Modern):** Today, every host connects via a dedicated point-to-point link to a central device called a **link-layer switch** (or Ethernet switch). This topology eliminates collisions.
  
  *   **Ethernet Frame Structure:**
    ```
    +----------+----------+----------+------+-----------------+-----+
    | Preamble | Dest MAC | Src MAC  | Type | Data (Payload)  | CRC |
    +----------+----------+----------+------+-----------------+-----+
    ```
    *   **Preamble:** 8 bytes used to synchronize the clocks of the sender and receiver.
    *   **Destination & Source MAC Addresses:** The 6-byte (48-bit) MAC addresses of the destination and source interfaces.
    *   **Type:** A field that indicates which network-layer protocol the payload contains (e.g., `0x0800` for IP). This is used for demultiplexing at the receiver.
    *   **Data (Payload):** The encapsulated IP datagram.
    *   **CRC (Cyclic Redundancy Check):** A 32-bit field used for robust error detection. If the receiver computes a CRC that doesn't match this field, the frame is dropped.
  
  *   **Ethernet Service Model:** Ethernet is **unreliable and connectionless**.
    *   **Connectionless:** No handshake between sending and receiving NICs.
    *   **Unreliable:** The receiving NIC does not send ACKs or NAKs. If a frame is dropped due to an error, it is up to a higher-layer protocol (like TCP) to recover from the loss.
- #### **Link-Layer Switches**
  
  Switches are the heart of modern Ethernet LANs. They are "smart" devices that have replaced simple hubs.
  
  *   **Role:** A switch's job is to receive incoming frames and **selectively forward** them to the correct outgoing link, without the hosts on the LAN being aware of its presence (it is **transparent**).
  
  *   **Key Characteristics:**
    *   **Store-and-Forward:** A switch buffers the entire incoming frame before making a forwarding decision.
    *   **Dedicated Links:** Each host has a dedicated, full-duplex connection to the switch. This means each link is its **own separate collision domain**, and collisions do not occur. CSMA/CD is not needed.
    *   **Multiple Simultaneous Transmissions:** Because the links are separate, the switch can forward multiple frames between different pairs of ports simultaneously (e.g., Host A can send to Host B at the same time Host C sends to Host D), as long as they don't require the same output port.
  
  *   **The Switch Table (or Forwarding Table):**
    *   **Function:** To make its forwarding decisions, a switch maintains a table that maps destination MAC addresses to the switch's outgoing interfaces.
    *   **Entry Format:** `< MAC Address, Interface, Timestamp >`
  
  *   **How Switches are "Self-Learning" (Plug-and-Play):**
    A switch builds its table automatically without any manual configuration.
    1.  **Learning:** For each incoming frame, the switch looks at the frame's **source MAC address**. It records this MAC address and the interface it arrived on in its switch table. This is how the switch learns which hosts are on which ports.
    2.  **Forwarding/Filtering:** The switch then looks at the frame's **destination MAC address**.
        *   **Case 1 (Entry Found):** If the destination MAC is in the table, the switch forwards the frame *only* to the interface specified in the table entry. (If the destination is on the same interface the frame arrived on, the switch filters/drops the frame).
        *   **Case 2 (Entry Not Found):** If the destination MAC is *not* in the table, the switch does not know where to send it. As a last resort, it **floods** the frame—it broadcasts a copy out of *all interfaces except the one it arrived on*.
        *   The host with that destination MAC will eventually reply, and when it does, the switch will learn its location from the source MAC of the reply frame.
  
  *   **Switches vs. Routers:**
    *   Both are store-and-forward devices and both have forwarding tables.
    *   **Switch (Layer 2):** Forwards based on MAC addresses. Learns its table via flooding and learning. Not visible to hosts.
    *   **Router (Layer 3):** Forwards based on IP addresses. Computes its table using routing protocols (OSPF, BGP). Is visible to hosts as a "hop."