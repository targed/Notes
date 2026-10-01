- # Practice Questions: Multiple Choice

- **Question 1**
- Which of the following services may be implemented in a link-layer protocol? Select one or more statements.

* [ ] Flow control between directly connected nodes.  
* [x] Coordinated access to a shared physical medium.  
* [x] Multiplexing down from / multiplexing up to a network-layer protocol.  
* [ ] Lookup and forwarding on the basis of an IP destination address.  
* [x] Reliable data transfer between directly connected nodes.  
* [x] TLS security (including authentication) between directly connected nodes.  
* [x] End-end path determination through multiple IP routers.  
* [ ] Bit-level error detection and correction.

- **Question 2**
- Which of the following statements is true about a two-dimensional parity check (2D-parity) computed over a payload?
* [x] 2D-parity can detect and correct any case of two bit flips in the payload.  
* [ ] 2D-parity can detect any case of two bit flips in the payload.  
* [x] 2D-parity can detect any case of a single bit flip in the payload.  
* [x] 2D-parity can detect and correct any case of a single bit flip in the payload.

- **Question 3**
- Which of the following statements is true about channel partitioning protocols?
* [x] There can be times when the channel is idle, when a node has a frame to send, but is prevented from doing so by the medium access protocol.  
* [ ] Channel partitioning protocol can achieve 100% utilization, in the case that there is only one node that always has frames to send  
* [ ] There can be simultaneous transmissions resulting in collisions.  
* [x] Channel partitioning protocols can achieve 100% channel utilization, in the case that all nodes always have frames to send.

- **Question 4**
- Which of the following statements is true about both Pure Aloha, and CSMA (both with and without collision detection?
* [x] Pure Aloha and CSMA can achieve 100% utilization, in the case that there is only one node that always has frames to send  
* [ ] Pure Aloha and CSMA can achieve 100% channel utilization, in the case that all nodes always have frames to send.  
* [ ] There can be times when the channel is idle, when a node has a frame to send, but is prevented from doing so by the medium access protocol.  
* [x] There can be simultaneous transmissions resulting in collisions.

- **Question 5**
- Which of the following statements is true about polling and token-passing protocols?
* [x] These protocol can achieve close to 100% channel utilization, in the case that all nodes always have frames to send  (the fact that the utilization is close to, but not exactly, 100% is due to a small amount of medium access overhead but not due to collisions)  
* [ ] There can be simultaneous transmissions resulting in collisions.  
* [ ] There can be times when the channel is idle for more than a short period of time, when a node has a frame to send, but is prevented from doing so by the medium access protocol.  
* [x] These protocol can achieve close 100% utilization, in the case that there is only one node that always has frames to send  (the fact that the utilization is close to, but not exactly, 100% is due to a small amount of medium access overhead but not due to collisions)

- **Question 6**
- Consider the following multiple access protocols that we've studied: (1) TDMA, and FDMA (2) CSMA (3) Aloha, and (4) polling.  Which of these protocols are collision-free (e.g., collisions will never happen)?
* [x] TDMA and FDMA  
* [ ] CSMA and CSMA/CD  
* [x] Polling  
* [ ] Aloha

- **Question 7**
- Consider the following multiple access protocols that we've studied: (1) TDMA, and FDMA (2) CSMA (3) Aloha, and (4) polling.  Which of these protocols requires some form of centralized control to mediate channel access?

* [ ] CSMA and CSMA/CD  
* [x] TDMA and FDMA  
* [ ] Aloha  
* [x] Polling

- **Question 8**
- Consider the following multiple access protocols that we've studied: (1) TDMA, and FDMA (2) CSMA (3) Aloha, and (4) polling.  For which of these protocols is the maximum channel utilization 1 (or very close to 1)?
* [ ] CSMA and CSMA/CD  
* [ ] Aloha  
* [x] Polling  
* [x] TDMA and FDMA

- **Question 9**
- Consider the following multiple access protocols that we've studied: (1) TDMA, and FDMA (2) CSMA (3) Aloha, and (4) polling.  For which of these protocols is there a maximum amount of time that a node knows that it will have to wait until it can successfully gain access to the channel?
* [ ] CSMA and CSMA/CD  
* [x] Polling  
* [ ] Aloha  
* [x] TDMA and FDMA

- **Question 10**
- We've now learned about both IPv4 addresses and MAC addresses.  Consider the address properties below, and use the pulldown menu to indicate  which of these properties is only a property of MAC addresses (and therefore is not a property of IPv4 addresses - careful!).
* [ ] This is a 32-bit address.  
* [ ] This is a 128-bit address.  
* [x] This address remains the same as a host moves from one network to another.  
* [ ] This address must be unique among all hosts in a subnet.  
* [x] This is a link-layer address.  
* [ ] This is a network-layer address.  
* [ ] This address is allocated by DHCP.  
* [x] This is a 48-bit address

- **Question 11**
- We've now learned about both IPv4 addresses and MAC addresses.  Consider the address properties below, and use the pulldown menu to indicate  which of these properties is only a property of IPv4 addresses (and therefore is not a property of MAC addresses - careful!).
* [ ] This address must be unique among all hosts in a subnet.  
* [ ] This address remains the same as a host moves from one network to another.  
* [x] This address is allocated by DHCP.  
* [ ] This is a 48-bit address.  
* [x] This is a network-layer address.  
* [x] This is a 32-bit address.  
* [ ] This is a 128-bit address.  
* [ ] This is a link-layer address.

- **Question 12**
- We've now learned about both IPv4 addresses and MAC addresses.  Consider the address properties below, and use the pulldown menu to indicate  which of these properties is a property of both IPv4 addresses and MAC addresses.
* [ ] This is a network-layer address.  
* [ ] This is a link-layer address.  
* [ ] This is a 48-bit address.  
* [ ] This address is allocated by DHCP.  
* [x] This address must be unique among all hosts in a subnet.  
* [ ] This is a 128-bit address.  
* [ ] This address remains the same as a host moves from one network to another.  
* [ ] This is a 32-bit address.

- **Question 13**
- Use the pulldown menus below to match the name of the field with the function/purpose of a field within an Ethernet frame.

- Answer List:
* Used to detect and possibly correct bit-level errors in the frame.  
* Used only to detect, but never correct, bit-level errors in the frame.  
* Used to demultiplex the payload up to a higher level protocol at the receiver.  
* 48-bit MAC address of the sending node.  
* This field does not exist in the Ethernet frame  
* Used for flow control.  
* The contents of this field is typically (bit not always) a network-layer IP datagram.

- Question List:

* Cyclic redundancy check (CRC) field:   
* Source address field:   
* Data (payload) field:   
* Type field.  
* Sequence number field

- **Question 14**
- Suppose an Ethernet frame arrives to an Ethernet switch, and the Ethernet switch does not know which of its switch ports leads to the node with the given destination MAC address?  In this case, what does the switch do?
* [ ] Drop the frame without forwarding it.  
* [ ] Use the address resolution protocol (ARP) to determine the appropriate outgoing port.  
* [x] Flood the frame on all ports except the port on which the frame arrived.  
* [ ] Choose a port randomly and forward the frame there.

- **Question 15**
- Which of the following statements are true about a self learning switch?
* [x] A self-learning switch will age-out (forget) a self-learned association of a MAC address x and switch port y if it doesn’t see a frame with MAC address x incoming on switch port y after some amount of time.  
* [ ] A self-learning switch never forgets a self-learned association of a MAC address x and switch port y.  
* [x] A self learning switch associates the source MAC address on an incoming frame with the port on which it arrived, and stores this matching in a table.  The switch has now learned the port that leads to that MAC address.  
* [x] A self-learning switch frees a network manager from a least one configuration task that might be associated with managing a switch

- **Question 16**
- Consider the simple star-connected Ethernet LAN shown below, and suppose the Ethernet switch is a learning switch, and that the switch table is initially empty. Suppose C sends an Ethernet frame address to C' and C ' replies back to C.  How many of these two frames are also received at B's interface?
* [ ] 0  
* [x] 1  
* [ ] 4  
* [ ] 2

- **Question 17**
- Consider the simple star-connected Ethernet LAN shown below, and suppose the switch table contains entries for each of the 6 hosts.  How will those entries be removed from the switch table?
* [ ] A table entry for a host will be removed by the STPP (Switch Table Purge Protocol) which will be used by a host to signal the switch when it (the host) is shutting down or otherwise leaving the network.  
* [x] An entry for a hostwill be removed if that host doesn't transmit any frames for a certain amount of time (that is, table entries will timeout).  
* [ ] The table entry can only be removed by the network manager, who would use the SNMP protocol to remove the entry.  
* [ ] They'll remain in the switch forever (or until it is re-booted).

- **Question 18**
- Which of the following statements are true about MAC (link-layer) addresses?  Select one or more statements below.
* [ ] Has 48 bits.  
* [x] Is contained in a SIM card and used when a device identifies itself and connects to an LTE network.  
* [x] Generally stays unchanged as a device moves from one network to another.  
* [x] Generally does not change, and is associated with a device when it is manufactured/created.  
* [ ] Has 32 bits.  
* [ ] A portion of the address bits are associated with the network to which the device is attached, and so changes as the device moves from one network to another.

- **Question 19**  
- What is the primary function of a MAC address in a Local Area Network (LAN)?
* [ ] To determine the least-cost path to a host on a different network.  
* [ ] To provide a globally unique and permanent identifier for a transport-layer process.  
* [x] To address a frame to a specific interface on the same physical subnet.  
* [ ] To ensure the reliable, in-order delivery of a stream of bytes.

- **Question 20**  
- A host with IP address 192.168.1.10 wants to send an IP datagram to a host with IP address 192.168.1.20 on the same Ethernet LAN. However, it does not have the destination host's MAC address in its cache. What will it do first?
* [ ] Send the datagram in a frame with the destination MAC address set to all zeros.  
* [ ] Send a query to the local DNS server to resolve the IP address to a MAC address.  
* [x] Broadcast an ARP query frame to all devices on the LAN, asking "Who has 192.168.1.20?".  
* [ ] Send the datagram to the default gateway router, which will find the correct MAC address.

- **Question 21**  
- The Ethernet frame includes a CRC (Cyclic Redundancy Check) field. What is the action taken by a receiving network interface card (NIC) if it computes a checksum that does not match the value in the CRC field?

* [ ] It sends a NAK (Negative Acknowledgment) back to the sender.  
* [ ] It attempts to correct the bit errors using the CRC value.  
* [x] It silently drops the frame.  
* [ ] It forwards the corrupted frame to the network layer with a flag indicating an error.

- **Question 22**  
- What is the key improvement of CSMA/CD (Carrier Sense Multiple Access with Collision Detection) over the simpler CSMA protocol?

* [ ] It uses a centralized controller to eliminate all collisions.  
* [ ] It requires nodes to wait a random time before sensing the channel.  
* [x] It allows a transmitting node to detect a collision as it happens and immediately abort its transmission.  
* [ ] It uses a "token" to grant permission to transmit.

- **Question 23**  
- A major drawback of channel partitioning protocols like TDMA (Time Division Multiple Access) is that:

* [ ] They are highly susceptible to collisions when the network is busy.  
* [ ] They require a complex, decentralized algorithm for nodes to agree on who can transmit.  
* [x] The channel bandwidth is wasted if a node has been allocated a time slot but has no data to send.  
* [ ] They cannot support full-duplex communication.

- **Question 24**  
- An Ethernet switch with an empty forwarding table receives a frame on port 1 from Host A, addressed to Host C. What two actions will the switch perform?
* [ ] Drop the frame and send an ARP query for Host C.  
* [x] Learn that Host A is on port 1, and flood the frame to all other ports.  
* [ ] Learn that Host A is on port 1, and send the frame back to Host A.  
* [ ] Flood the frame to all ports, and wait for a reply before creating any table entries.

- **Question 25**  
- Which of the following statements correctly identifies the primary difference between a link-layer switch and a network-layer router?
* [ ] A switch is a "store-and-forward" device, while a router is not.  
* [ ] A router uses a forwarding table, while a switch does not.  
* [ ] A switch operates at Layer 3, and a router operates at Layer 2.  
* [x] A switch makes forwarding decisions based on MAC addresses, while a router makes them based on IP addresses.

- **Question 26**  
- What is the purpose of the "Type" field in an Ethernet frame header?
* [ ] To indicate whether the frame contains an ARP query or an ARP response.  
* [ ] To specify the priority of the frame for scheduling purposes.  
* [x] To allow the receiving link layer to know which network-layer protocol (e.g., IP, IPX) it should pass the payload to.  
* [ ] To distinguish between a wired Ethernet frame and a Wi-Fi frame.

- **Question 27**  
- True or False: The Ethernet protocol provides a reliable, connection-oriented service to the network layer, guaranteeing that all frames are delivered error-free and in order.
* [ ] True  
* [x] **False**

- **Question 28**  
- Host A (IP: 10.0.1.5) is on Subnet 1 and wants to send a datagram to Host B (IP: 10.0.2.6) on Subnet 2. The datagram must first go to Host A's default gateway router, R (MAC: AA-AA...). What will be the source and destination IP and MAC addresses in the frame sent from Host A to the router?
* [ ] Src IP: 10.0.1.5, Dst IP: (R's IP); Src MAC: (A's MAC), Dst MAC: (B's MAC)  
* [ ] Src IP: 10.0.1.5, Dst IP: 10.0.2.6; Src MAC: (A's MAC), Dst MAC: (B's MAC)  
* [x] Src IP: 10.0.1.5, Dst IP: 10.0.2.6; Src MAC: (A's MAC), Dst MAC: (R's MAC)  
* [ ] Src IP: (A's MAC), Dst IP: (B's MAC); Src MAC: 10.0.1.5, Dst MAC: 10.0.2.6

- **Question 29**  
- How does a modern Ethernet switch differ from an older Ethernet hub?
* [ ] A hub has a forwarding table that it learns, while a switch is a simple physical-layer repeater.  
* [ ] A hub reduces the chance of collisions using CSMA/CD, while a switch uses a token-passing scheme.  
* [x] **A switch selectively forwards frames to a specific output port, while a hub broadcasts every incoming frame to all of its ports.**  
* [ ] A switch operates at the network layer, while a hub operates at the transport layer.

- **Question 30**  
- An Ethernet switch has three ports, and its forwarding table is initially empty. Host A is connected to port 1, Host B to port 2, and Host C to port 3. Host C sends a frame to Host A. After this frame is processed, Host A replies with a frame to Host C. What action does the switch take for the reply frame from A to C?
* [ ] It floods the frame to ports 2 and 3.  
* [ ] It drops the frame because it does not know where Host C is.  
* [ ] It forwards the frame only to port 2.  
* [x] It forwards the frame only to port 3.

- **Question 31**  
- What is the primary purpose of the Preamble in an Ethernet frame?
* [ ] To carry the source and destination MAC addresses.  
* [x] To allow the receiver's hardware to synchronize its clock with the sender's clock before the actual data arrives.  
* [ ] To perform error detection on the frame's payload.  
* [ ] To indicate which network-layer protocol is being used.

- **Question 32**  
- Consider a satellite communication system where several ground stations need to share a single channel. Each station requires a guaranteed, fixed amount of time to transmit its data during each cycle. Which multiple access protocol is best suited for this scenario?
* [ ] CSMA/CD  
* [ ] Pure ALOHA  
* [x] TDMA (Time Division Multiple Access)  
* [ ] CSMA

- **Question 33**  
- CRC is a powerful error detection scheme used in Ethernet and Wi-Fi. What is one of its key strengths?
* [ ] It can correct any number of bit errors without retransmission.  
* [x] It is highly effective at detecting burst errors (a sequence of consecutive bit flips).  
* [ ] It is simpler to compute in software than a simple checksum.  
* [ ] It guarantees that no error will ever go undetected.

- **Question 34**  
- A host on a local network wants to send a datagram to a web server on the public internet. To do this, it must first send the packet to its default gateway router. How does the host typically find the MAC address of its default gateway?
* [ ] The gateway's MAC address is programmed into the web server's DNS record.  
* [x] It sends an ARP query for the IP address of the gateway.  
* [ ] The gateway's MAC address is always the broadcast address (FF-FF-FF-FF-FF-FF).  
* [ ] It sends an ARP query for the IP address of the final destination web server.

- **Question 35**  
- Which of the following statements best describes a key difference between the MAC and IP addressing schemes?
* [ ] MAC addresses are 32 bits long, while IP addresses are 48 bits long.  
* [x] IP addresses are hierarchical (containing a network prefix and a host part), while MAC addresses are flat (having no hierarchical structure).  
* [ ] A device's IP address is permanent, while its MAC address changes as it moves between networks.  
* [ ] Only routers have IP addresses; only hosts have MAC addresses.

- **Question 36**  
- Virtual Private Networks (VPNs) create a secure "tunnel" across the public internet. What is the fundamental mechanism used to create this tunnel?
* [ ] Rewriting the MAC addresses of every frame to hide the original source.  
* [ ] Using a special version of the BGP protocol to create a private route.  
* [ ] Assigning temporary public IP addresses to all devices on the private network.  
* [x] Encapsulating the original IP datagram (with private addresses) inside a new IP datagram (with public addresses).

- **Question 37**  
- True or False: In a network using the CSMA/CD protocol, collisions are completely eliminated.
* [ ] True  
* [x] False (Collisions can still occur due to propagation delay; CSMA/CD's purpose is to detect and recover from them, not eliminate them entirely.)

- **Question 38**  
- Why might a link-layer protocol like Wi-Fi implement its own reliability (hop-by-hop retransmission) even when the application above it is using TCP for end-to-end reliability?
* [ ] It is a requirement of the IP protocol.  
* [ ] To ensure that MAC addresses are never duplicated.  
* [x] It is much more efficient to correct an error on a high-error link (like wireless) immediately at the link layer than to wait for TCP's much longer end-to-end timeout.  
* [ ] TCP's checksum is not strong enough to detect errors in wireless frames.

- **Question 39**  
- A host (Host A) has successfully sent an ARP request and received an ARP reply from another host on the same LAN (Host B). What is the most immediate and direct consequence of this exchange?
* [ ] Host A now knows the IP address of Host B.  
* [ ] A TCP connection is now established between Host A and Host B.  
* [x] Host A has added an entry for Host B's IP-to-MAC address mapping in its ARP cache.  
* [ ] The switch connecting them has learned the location of both hosts.

- **Question 40**  
- What is the key capability of a two-dimensional parity scheme that a simple single-bit parity scheme lacks?
* [ ] It can detect all possible burst errors.  
* [ ] It requires fewer redundant bits to be transmitted.  
* [x] It can detect and correct single-bit errors.  
* [ ] It is simpler to implement in hardware.

- **Question 41**  
- An Ethernet switch has four ports with hosts A, B, C, and D connected to ports 1, 2, 3, and 4, respectively. The switch's forwarding table is initially empty. Host A sends a frame to Host B. Immediately after, Host B replies with a frame to Host A. What does the switch do with Host B's reply frame?
* [ ] It floods the frame to ports 1, 3, and 4.  
* [ ] It floods the frame to ports 3 and 4 only.  
* [x] It forwards the frame only to port 1.  
* [ ] It drops the frame because it does not yet know the location of Host A.

- **Question 42**  
- Which of the following analogies best describes the functional difference between an IP address and a MAC address?
* [ ] IP address is like a car's color; MAC address is like its license plate number.  
* [x] IP address is like a person's postal address; MAC address is like their unique Social Security Number.  
* [ ] IP address is like a person's job title; MAC address is like their name.  
* [ ] IP address is like a phone number; MAC address is like the brand of the phone.

- **Question 43**  
- Multiprotocol Label Switching (MPLS) is often used by ISPs for traffic engineering. How does an MPLS router forward a packet?
* [ ] By performing a longest prefix match on the destination IP address.  
* [ ] By looking up the destination port number in a flow table.  
* [x] By using the incoming packet's label to look up an outgoing label and port in a forwarding table (label swapping).  
* [ ] By sending the packet to a centralized SDN controller for a decision.

- **Question 44**  
- If you replace a modern Ethernet switch with an older Ethernet hub, what is the most significant negative consequence for the network's performance?
* [ ] The hub will be unable to learn MAC addresses, so all packets will be dropped.  
* [ ] The hub operates at the network layer, adding unnecessary processing delay.  
* [x] All devices connected to the hub will be in the same collision domain, meaning only one device can successfully transmit at a time.  
* [ ] The hub does not support the ARP protocol.

- **Question 45**  
- A datagram is sent from a host in a university LAN to a server on another continent. The path involves traversing 10 different subnets across multiple routers. How many times is the ARP protocol used during this datagram's journey?
* [ ] Only once, at the very beginning.  
* [ ] Only twice, once at the start and once at the end.  
* [x] It is used on each of the 10 subnets to find the MAC address of the next hop.  
* [ ] It is not used at all; BGP is used instead.

- **Question 46**
- In the context of encapsulation, what is typically found inside the "Data (Payload)" field of an Ethernet frame?
* [ ] A TCP segment.  
* [x] An IP datagram.  
* [ ] An HTTP request message.  
* [ ] Another Ethernet frame.

- **Question 47**  
- True or False: CSMA/CD is an effective multiple access protocol for networks with very large propagation delays, such as satellite networks.
* [ ] True  
* [x] False (With long delays, a node could finish transmitting its entire frame before it detects a collision, defeating the purpose of Collision Detection.)

- **Question 48**
- How does the process of building a forwarding table differ between a link-layer switch and a network-layer router?
* [ ] A switch's table is static and manually configured, while a router's table is dynamic.  
* [ ] Both learn their tables by inspecting the destination addresses of passing packets.  
* [x] A switch passively learns its table by observing the source MAC addresses of incoming frames, while a router actively computes its table by communicating with other routers via a routing protocol.  
* [ ] A switch uses IP addresses for its table, while a router uses MAC addresses.

- # Practice Questions: Math
- #### **Question 1: Cyclic Redundancy Check (CRC)**

A sender wants to transmit the data block D = 101110. The sender and receiver have agreed to use the generator polynomial G = 1001. (This means r = 3).

1. **(5 points)** Show the step-by-step binary long division (using XOR) that the sender performs to calculate the CRC remainder bits (R).  
2. **(3 points)** What is the final frame that the sender transmits?  
3. **(2 points)** Show the calculation that the receiver performs to check if the received frame is error-free.

---

- #### **Question 2: Two-Dimensional Parity**

A sender arranges four 8-bit bytes of data into a grid and computes the even parity for each row and each column. The resulting block is transmitted, but a single bit is flipped during transit. The block received is shown below.

|  | Data Bit 1 | Data Bit 2 | Data Bit 3 | Data Bit 4 | Data Bit 5 | Data Bit 6 | Data Bit 7 | Data Bit 8 | Row Parity |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Byte 1 | 1 | 0 | 1 | 1 | 0 | 0 | 1 | 0 | 0 |
| Byte 2 | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 0 | 0 |
| Byte 3 | 1 | 1 | 0 | 1 | 0 | 1 | 1 | 0 | 1 |
| Byte 4 | 0 | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 |
| Col Parity | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 0 |  |

1. For each row and each column, determine if its parity is correct or incorrect based on the received parity bits.  
2. Identify the row and column number where the single bit error occurred.  
3. What was the original, correct value of Byte 3 before it was corrupted?

---

- #### **Question 3: Slotted ALOHA Performance**

Consider a Slotted ALOHA network with N = 20 active nodes. Each node has a large number of frames to send and attempts to transmit in any given time slot with a probability p = 0.05.

1. What is the probability that any one specific node (e.g., Node A) successfully transmits its frame in a given slot?  
2. What is the probability of a collision occurring in a given slot?  
3. What is the overall efficiency (the probability of a successful transmission by any node) of the channel in this state?

---

- #### **Question 4: Ethernet Switch Self-Learning**

Consider a network with four hosts (A, B, C, D) connected to a 4-port Ethernet switch. The switch's forwarding table is initially empty.

Assume the following sequence of transmissions occurs:

1. **Frame 1:** Host A sends a frame to Host C.  
2. **Frame 2:** Host C sends a frame back to Host A.  
3. **Frame 3:** Host B sends a frame to Host D.

For each of the three steps above, provide the following two pieces of information:

* (a) The state of the switch's forwarding table after the frame has been processed.  
* (b) Whether the frame was selectively forwarded or flooded.

---

- #### **Question 5: Link-Layer Protocol Utilization**

A satellite link connects two ground stations. Due to the high error rate of the link, a stop-and-wait reliable data transfer protocol is used at the link layer for every frame.

* Link bandwidth (R): 10 Mbps  
* One-way propagation delay (DpropD_{prop}Dprop​): 200 ms  
* Frame size (L): 2,000 bytes

1. **(5 points)** Calculate the sender utilization (UsenderU_{sender}Usender​) for this link. (Remember to be careful with units: 1 byte = 8 bits, and be consistent with seconds/milliseconds).  
2. **(5 points)** What is the minimum window size (N) a pipelined protocol would need to achieve a utilization of 100% on this link?
