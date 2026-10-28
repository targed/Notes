### Network Protocol Layers
#### Why Use Layering?
  
  Modern computer networks are incredibly complex systems with many moving parts, including hardware (routers, switches, cables), software (operating systems, applications), and protocols. To manage this complexity, network designers use a **layered architecture**.
  
  The core idea is to break down the complex task of network communication into a series of smaller, more manageable sub-tasks. Each sub-task is handled by a separate "layer."
  
  This approach, demonstrated by the air travel analogy in the slides, provides several key benefits:
  
  *   **Modularity:** Each layer provides a specific service to the layer directly above it and relies on services from the layer directly below it. This makes the system easier to understand, maintain, and update.
  *   **Easier Maintenance:** You can change the implementation of one layer without affecting any of the other layers. For example, you can switch from a Wi-Fi (wireless) link to an Ethernet (wired) link at the Link Layer, and your web browser (in the Application Layer) will continue to work without any changes.
  *   **Standardization:** Layering provides a clear framework for defining the functions and protocols at each level, which promotes interoperability between different vendors' equipment.
#### The 5-Layer Internet Protocol Stack
  
  The Internet's architecture is described by a five-layer model. Data is passed down this stack on the sending computer and up the stack on the receiving computer.
  
  1.  **Layer 5: Application Layer**
    *   **Function:** This is the layer where network applications and their protocols reside. It provides services directly to the user's application.
    *   **Examples:** HTTP (for the Web), SMTP (for email), FTP (for file transfer), DNS (for domain name resolution).
    *   **Data Unit:** Message
  
  2.  **Layer 4: Transport Layer**
    *   **Function:** Provides logical communication between *processes* running on different hosts. Its primary roles are to handle reliability, flow control, and error correction.
    *   **Examples:**
        *   **TCP (Transmission Control Protocol):** A reliable, connection-oriented protocol that guarantees delivery of data.
        *   **UDP (User Datagram Protocol):** An unreliable, connectionless "best-effort" protocol that is faster but does not guarantee delivery.
    *   **Data Unit:** Segment
  
  3.  **Layer 3: Network Layer**
    *   **Function:** Responsible for moving packets from a source *host* to a destination *host*, which may be in different networks. It handles the logical addressing (IP addresses) and routing of packets across the internet.
    *   **Examples:** IP (Internet Protocol), routing protocols (e.g., OSPF, BGP).
    *   **Data Unit:** Datagram
  
  4.  **Layer 2: Link Layer**
    *   **Function:** Responsible for moving data packets between two *adjacent* network nodes (e.g., from a host to a router, or from one router to another). It handles physical addressing (MAC addresses) and error detection on the link.
    *   **Examples:** Ethernet, Wi-Fi (802.11), PPP.
    *   **Data Unit:** Frame
  
  5.  **Layer 1: Physical Layer**
    *   **Function:** Responsible for the physical transmission of raw bits over the communication medium. It defines the hardware specifications, such as cable types, voltages, and radio frequencies.
    *   **Examples:** Ethernet cables, fiber optic cables, radio waves.
    *   **Data Unit:** Bits
#### Encapsulation: The Journey of Data
  
  For data to move from a source to a destination, it must travel down the protocol stack at the source and up the stack at the destination. This process involves **encapsulation**.
  
  Think of it like sending a letter using a series of nested envelopes, or the Matryoshka (stacking) dolls shown in the slides.
  
  1.  The **Application Layer** creates a message (M).
  2.  The message is passed to the **Transport Layer**, which adds a header (Ht) containing information like source and destination port numbers. This new unit is called a **segment**. `[Ht | M]`
  3.  The segment is passed to the **Network Layer**, which adds its own header (Hn) with source and destination IP addresses. This unit is called a **datagram**. `[Hn | [Ht | M]]`
  4.  The datagram is passed to the **Link Layer**, which adds a header (Hl) with source and destination MAC addresses. This unit is now a **frame**. `[Hl | [Hn | [Ht | M]]]`
  5.  The **Physical Layer** transmits the frame as a sequence of bits.
  
  When the data arrives at the next node (a router or the final destination), the reverse process, **decapsulation**, occurs. Each layer strips off its header, reads the information, and passes the remaining data up to the layer above it until the original message reaches the destination application.
#### Comparison: Internet Stack vs. OSI Model
  
  The slides also mention the **ISO/OSI (Open Systems Interconnection) reference model**, which is a more formal, seven-layer model. The Internet stack is a more practical model that maps directly to the protocols used in the real world.
  
  The OSI model includes two additional layers not explicitly found in the Internet stack:
  
  *   **Presentation Layer (Layer 6):** This layer handles data format translation, such as character encoding, compression, and encryption. In the Internet model, these functions are typically handled by the Application Layer.
  *   **Session Layer (Layer 5):** This layer is responsible for establishing, managing, and terminating sessions between applications, including checkpointing and recovery. In the Internet model, these functions are also typically handled by the Application Layer.