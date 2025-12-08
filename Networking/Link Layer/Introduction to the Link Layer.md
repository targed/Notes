### **Chapter 6: The Link Layer and LANs**
- #### **6.1 Introduction to the Link Layer**
  
  **Core Terminology**
  *   **Nodes:** The devices that participate in the network. This includes both hosts (end systems) and routers.
  *   **Links:** The communication channels that connect adjacent nodes. Links can be wired (e.g., Ethernet, fiber optic) or wireless (e.g., Wi-Fi, cellular). A Local Area Network (LAN) is also considered a type of link.
  *   **Frame:** The Layer-2 packet. A link-layer frame encapsulates a network-layer datagram.
  
  **The Fundamental Responsibility of the Link Layer**
  
  The link layer has the responsibility of transferring a datagram from one node to a **physically adjacent** node over a single link. This is a crucial distinction:
  *   The **Network Layer**'s job is to get a datagram from the original source host to the final destination host. This is a global, end-to-end task.
  *   The **Link Layer**'s job is to get that same datagram from one node to the *very next node* in the path. This is a local, hop-by-hop task.
  
  **Context: A Hop-by-Hop Journey**
  A datagram's journey from a source to a destination almost always involves traversing multiple different links. For example:
  1.  Your laptop to your home's Wi-Fi router (a Wi-Fi link).
  2.  Your home's router to the ISP's equipment (an Ethernet link).
  3.  Across the ISP's network (fiber optic links).
  
  The link layer is responsible for the transfer across *each one* of these individual links. Importantly, the specific link-layer protocol used on each link can be different. The network layer remains unaware of and untroubled by these differences.
  
  **Services Provided by the Link Layer**
  
  The link layer provides several key services to the network layer:
  
  1.  **Framing:**
    *   The link layer takes a network-layer datagram and encapsulates it within a link-layer **frame**.
    *   This involves adding a **header** and a **trailer** to the datagram. These headers contain control information, such as the source and destination **MAC addresses**.
  
  2.  **Link Access:**
    *   If a link is a **shared medium** (meaning multiple nodes are connected to the same channel, like in Wi-Fi or old bus-style Ethernet), a protocol is needed to coordinate which node gets to transmit and when.
    *   This is managed by **Multiple Access Protocols**, which are a key part of the link layer.
  
  3.  **Reliable Delivery (Hop-by-Hop):**
    *   Similar to the transport layer, the link layer can provide a reliable delivery service. However, this reliability is confined to a single link between two adjacent nodes.
    *   This service is seldom used on low bit-error links like fiber optic cables, where errors are rare.
    *   It is very important on high bit-error links like **wireless links**, where interference can easily corrupt frames.
    *   **Question:** Why have both link-level reliability and end-to-end (TCP) reliability?
    *   **Answer:** Efficiency. It is much faster to detect and retransmit a corrupted frame over a single wireless hop than it is to wait for the entire end-to-end TCP timeout and retransmission mechanism to kick in. Correcting errors locally is faster.
  
  4.  **Flow Control:**
    *   Pacing the transmission rate between adjacent sending and receiving nodes to prevent the sender from overwhelming the receiver's buffers.
  
  5.  **Error Detection and Correction:**
    *   **Error Detection:** Errors can be introduced by signal attenuation or electromagnetic noise. The link layer adds error detection bits (e.g., a checksum or CRC) to the frame. The receiving node checks these bits and can detect if an error has occurred, usually causing the frame to be dropped.
    *   **Error Correction:** Some protocols allow the receiver to identify and correct bit errors without having to request a retransmission.
  
  6.  **Half-Duplex and Full-Duplex:**
    *   **Half-Duplex:** Nodes at both ends of the link can transmit, but not at the same time.
    *   **Full-Duplex:** Nodes at both ends of the link can transmit and receive simultaneously.
  
  **Implementation of the Link Layer**
  
  The link layer is implemented in a device's **Network Interface Card (NIC)**, also known as a network adapter.
  *   The NIC is a piece of hardware (e.g., an Ethernet card, a Wi-Fi chip) that connects a device to the network.
  *   It is a combination of hardware, software (drivers), and firmware.
  *   The NIC is responsible for implementing both the link layer and the physical layer.
  *   **On the sending side:** The NIC takes a datagram from the host's memory, encapsulates it in a frame, and transmits it.
  *   **On the receiving side:** The NIC receives a frame, checks for errors, decapsulates the datagram, and passes it up to the host's network layer.