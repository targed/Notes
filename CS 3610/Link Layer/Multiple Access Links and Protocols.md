### **6.3 Multiple Access Links and Protocols**

**The Fundamental Problem**
Many network links are a **shared medium**, meaning two or more nodes are connected to the same physical channel (e.g., a single coaxial cable in old Ethernet, or the shared airwaves in Wi-Fi).

*   **The Multiple Access Problem:** How do we coordinate the access of multiple sending and receiving nodes to this shared channel?
*   **Collision:** If two or more nodes transmit frames at the same time, their signals will interfere with each other at the receiver. The receiver will get a garbled, useless signal. This is a **collision**. All frames involved in the collision are lost.

The goal of a **multiple access protocol** is to provide a set of rules that nodes can follow to coordinate their transmissions and handle collisions.

**An Ideal Multiple Access Protocol**
An ideal protocol would have the following characteristics:
1.  When only one node has data to send, it can use the full bandwidth `R` of the channel.
2.  When `M` nodes have data to send, each should get an average throughput of `R/M`.
3.  It should be fully decentralized, with no master node to coordinate things.
4.  It should be simple and inexpensive to implement.

---
- ### **Taxonomy of Multiple Access Protocols**
  
  There are three broad classes of multiple access protocols.
- #### **1. Channel Partitioning Protocols**
  
  These protocols divide the channel's resources (time, frequency) into smaller, dedicated "pieces" and allocate a piece to each node.
  
  *   **Core Idea:** Eliminate collisions by giving each node its own private slice of the channel.
  *   **TDMA (Time Division Multiple Access):**
    *   Access to the channel is divided into repeating rounds. Each round is divided into a fixed number of **time slots**.
    *   Each node is assigned its own dedicated time slot and can only transmit during that slot.
    *   **Disadvantage:** Very inefficient if a node has nothing to send, as its time slot goes unused and cannot be used by other nodes.
  *   **FDMA (Frequency Division Multiple Access):**
    *   The channel's frequency spectrum is divided into narrower **frequency bands**.
    *   Each node is assigned its own dedicated frequency band.
    *   **Disadvantage:** Same as TDMA; if a node is idle, its frequency band is wasted.
- #### **2. Random Access Protocols**
  
  In these protocols, there is no pre-coordination. Each node transmits at the full channel rate `R`. If a collision occurs, the protocol specifies how to detect it and how to recover.
  
  *   **Slotted ALOHA:**
    *   **Assumptions:** Time is divided into equal-sized slots. All frames are the same size. Nodes are synchronized to the slots.
    *   **Rule:** When a node has a frame to send, it waits until the beginning of the *next* time slot and transmits the entire frame.
    *   **Collision:** If two or more nodes transmit in the same slot, a collision occurs. The nodes detect this collision (e.g., by not receiving an ACK).
    *   **Recovery:** On a collision, each colliding node waits a **random number of slots** before attempting to retransmit. The randomness is crucial to avoid repeated collisions.
    *   **Efficiency:** The maximum theoretical efficiency of Slotted ALOHA is only **37%**. The rest of the time, slots are either idle or contain collisions.
  
  *   **Pure (Unslotted) ALOHA:**
    *   This is the original, simpler version. There are no slots.
    *   **Rule:** When a frame arrives, the node transmits it immediately.
    *   **Collisions:** Collisions are much more likely because even a partial overlap of two frames is enough to cause a collision.
    *   **Efficiency:** The maximum efficiency is only **18%**, half that of Slotted ALOHA, demonstrating the benefit of synchronization.
  
  *   **CSMA (Carrier Sense Multiple Access): "Listen Before You Talk"**
    *   This is a significant improvement over ALOHA.
    *   **Rule:** Before transmitting, a node first **senses the channel** (listens) to see if another node is already transmitting.
        *   If the channel is idle, it transmits the frame.
        *   If the channel is busy, it defers its transmission until the channel becomes idle.
    *   **The Problem:** Collisions can still occur due to **propagation delay**. Two nodes at opposite ends of a link might both sense the channel as idle, start transmitting at the same time, and only detect the collision after their frames have traveled partway across the link.
  
  *   **CSMA/CD (Collision Detection): "Listen While You Talk"**
    *   This is the protocol that made classic **Ethernet** famous.
    *   **Rule:** A node follows the CSMA rule, but it also **monitors the channel while it is transmitting**.
    *   **Collision Detection:** If a node detects that another node's signal is interfering with its own transmission, it knows a collision has occurred.
    *   **Action:** It immediately **aborts its transmission** (saving time and bandwidth) and sends a "jam signal" to ensure all other nodes are aware of the collision.
    *   **Recovery:** It then enters an **exponential backoff** phase: it waits a random amount of time, chosen from an exponentially growing interval, before attempting to retransmit.
- #### **3. "Taking Turns" Protocols**
  
  These protocols attempt to combine the best of both worlds: the high efficiency of channel partitioning and the flexibility of random access.
  
  *   **Polling:** A "master" node invites "slave" nodes to transmit in turn. Eliminates collisions but introduces a single point of failure (the master) and polling delay.
  *   **Token Passing:**
    *   A special, small frame called a **token** is passed from node to node in a pre-determined order (e.g., around a logical ring).
    *   **Rule:** A node can only transmit data frames if it is currently holding the token.
    *   **Mechanism:** If a node has data, it holds the token, sends its frame(s), and then passes the token to the next node. If it has no data, it passes the token immediately.
    *   **Pros:** Highly efficient and fair.
    *   **Cons:** Complex, and the loss of the token can bring the entire network down.