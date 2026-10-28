### **3.4 Principles of Reliable Data Transfer (RDT)**
  
  **The Core Problem and Goal**
  The fundamental challenge is that the network layer (IP) on which we build is **unreliable**. It offers a "best-effort" service, which means packets can be:
  *   **Corrupted:** Bits can be flipped due to noise in the channel.
  *   **Lost:** Packets can be dropped by congested routers.
  *   **Reordered:** Packets may take different paths and arrive out of sequence.
  
  The goal is to create a **reliable data transfer protocol** that provides a service abstraction of a **perfectly reliable channel** to the application layer. The application should be able to send data and be confident that it will arrive at the destination, intact and in order.
  
  **The Implementation Approach**
  We will incrementally build the sender and receiver sides of a reliable data transfer protocol (`rdt`). To formally define the protocol's behavior, we will use **Finite State Machines (FSMs)**. An FSM defines:
  *   The **states** a protocol can be in (e.g., "waiting for data from application").
  *   The **events** that can occur (e.g., "a packet arrives from the network").
  *   The **actions** taken in response to an event (e.g., "send an ACK").
  
  ---
### **RDT 1.0: Reliable Transfer over a Perfectly Reliable Channel**
  
  *   **Assumption:** The underlying channel is perfect. It never loses or corrupts packets.
  *   **Sender Logic:**
    1.  Wait for a call from the application layer with data to send (`rdt_send(data)`).
    2.  Create a packet containing the data.
    3.  Send the packet into the channel (`udt_send(packet)`).
  *   **Receiver Logic:**
    1.  Wait for a packet to arrive from the channel (`rdt_rcv(packet)`).
    2.  Extract the data from the packet.
    3.  Pass the data up to the application layer (`deliver_data(data)`).
  *   **Conclusion:** This is a trivial case. No error-handling mechanisms are needed because no errors can occur.
  
  ---
### **RDT 2.0: A Channel with Bit Errors**
  
  *   **New Assumption:** The underlying channel can now corrupt packets (flip bits), but it will *not* lose them.
  *   **New Mechanisms Needed:**
    1.  **Error Detection:** We need a way to detect if a packet has been corrupted. The **checksum** is used for this.
    2.  **Feedback:** The receiver must provide feedback to the sender.
        *   **Acknowledgements (ACKs):** A positive feedback message indicating the packet was received correctly.
        *   **Negative Acknowledgements (NAKs):** A negative feedback message indicating the packet was received with errors (corrupt).
    3.  **Retransmission:** If the sender receives a NAK, it must re-send the last packet.
  
  This protocol is known as a **Stop-and-Wait** protocol because the sender sends one packet, then stops and waits for the receiver's response (ACK or NAK) before proceeding to the next packet.
  
  *   **The Fatal Flaw of RDT 2.0:** What if the ACK or NAK feedback message gets corrupted? The sender has no idea what happened at the receiver. If it just retransmits, it might be sending a **duplicate packet**.
  
  ---
### **RDT 2.1: Handling Garbled ACKs/NAKs**
  
  *   **Assumption:** The channel can corrupt both data packets and feedback (ACK/NAK) packets.
  *   **New Mechanism Needed:**
    *   **Sequence Numbers:** To handle potential duplicate packets, the sender adds a sequence number to each packet. For a stop-and-wait protocol, a single bit is sufficient (sequence numbers 0 and 1).
  *   **How it Works:**
    *   **Sender:** Sends packet with sequence number 0. Waits for ACK/NAK.
    *   **Receiver:** Expects packet 0. If it receives a correct packet 0, it sends an ACK for 0 and now expects packet 1. If it receives a corrupted packet, it sends a NAK. If it receives a packet with sequence number 1 when it's expecting 0, it knows this is a duplicate and discards it, but re-sends the ACK for 0.
    *   **Sender:** If it receives a garbled ACK/NAK, it retransmits the current packet (e.g., packet 0). The receiver's logic will correctly handle this duplicate.
  
  ---
### **RDT 2.2: A NAK-Free Protocol**
  
  This version provides the same functionality as RDT 2.1 but simplifies the protocol by eliminating NAKs. This is a key design choice used in TCP.
  
  *   **New Mechanism:**
    *   Instead of sending a NAK, the receiver now sends a **duplicate ACK** for the last packet that it *did* receive correctly.
  *   **How it Works:**
    *   Receiver expects packet 0. It receives packet 0 correctly and sends ACK(0).
    *   Sender sends packet 1. It gets corrupted in transit.
    *   Receiver receives the corrupted packet. Instead of a NAK, it sends another **ACK(0)**.
    *   When the sender receives this duplicate ACK(0), it knows that the receiver did not correctly receive the subsequent packet (packet 1) and therefore retransmits packet 1.
  
  ---
### **RDT 3.0: A Channel with Errors and Loss**
  
  *   **New Assumption:** The channel can now corrupt packets *and* lose them entirely. This is our most realistic model yet.
  *   **The Problem with Packet Loss:** If a data packet is lost, the receiver never sends an ACK. If an ACK is lost, the sender never receives it. In both cases, the sender will wait forever for a reply that will never come—a deadlock.
  *   **New Mechanism Needed:**
    *   **Countdown Timer:** The sender needs a mechanism to detect a lost packet.
  *   **How it Works:**
    1.  The sender sends a packet and **starts a timer**.
    2.  If an ACK for that packet arrives before the timer expires, the sender stops the timer and moves on to the next packet.
    3.  If the timer expires (**a timeout event occurs**), the sender assumes the packet (or its ACK) was lost.
    4.  The sender **retransmits** the packet and restarts the timer.
  *   This combination of checksums, sequence numbers, ACKs, and timeouts creates a functional reliable data transfer protocol.
  
  ---
### **Performance of RDT 3.0 (Stop-and-Wait)**
  
  While `rdt3.0` works, its performance is terrible on modern networks. The stop-and-wait nature is extremely inefficient.
  
  *   **Sender Utilization ($U_{sender}$):** The fraction of time the sender is actually busy transmitting data.
  
  **Example Calculation:**
  *   A **1 Gbps** link with a **15 ms** one-way propagation delay.
  *   Packet size (L) = 8000 bits.
  *   **Transmission Delay ($D_{trans}$):**
    $ D_{trans} = \frac{L}{R} = \frac{8000 \text{ bits}}{10^9 \text{ bits/sec}} = 8 \text{ microseconds} $
  *   **Round-Trip Time (RTT):**
    $ \text{RTT} = 2 \times D_{prop} = 2 \times 15 \text{ ms} = 30 \text{ milliseconds} = 30,000 \text{ microseconds} $
  
  The sender sends for 8 µs and then sits idle, waiting for the ACK, for 30,000 µs.
  
  *   **Utilization Calculation:**
    $ U_{sender} = \frac{D_{trans}}{RTT + D_{trans}} = \frac{8}{30000 + 8} \approx 0.00027 $
  *   **Conclusion:** The sender is only busy **0.027%** of the time. This is abysmal performance.
### **The Solution: Pipelining**
  
  To fix the poor performance of stop-and-wait, we must use **pipelining**. The sender is allowed to send multiple, "in-flight," yet-to-be-acknowledged packets without stopping to wait for an ACK for each one.
  
  Pipelining requires:
  *   A larger range of sequence numbers.
  *   Buffering at both the sender and receiver.
  
  Two generic forms of pipelined protocols are:
  1.  **Go-Back-N (GBN):** The sender can have up to N unacknowledged packets in the pipeline. The receiver only sends cumulative ACKs. If a packet is lost, the sender retransmits that packet and *all subsequent packets*.
  2.  **Selective Repeat (SR):** The sender can have up to N unacknowledged packets. The receiver individually acknowledges each correctly received packet. If a packet is lost, the sender only retransmits that single lost packet.