### Network Performance
  
  Network performance is crucial for the user experience of any application. We measure it using several key metrics, primarily focusing on how quickly and reliably data can be moved through the network. The main indicators of performance are packet delay (latency), packet loss, and throughput.
#### Packet Delay (Latency)
  
  Packet delay, or latency, is the time it takes for a packet to travel from its source to its destination. This total delay is an accumulation of several smaller delays that occur at each "hop" (i.e., each router) along the path. The four sources of packet delay are:
  
  1.  **Nodal Processing Delay (`dproc`):** This is the time a router takes to process a packet's header. This includes tasks like checking the packet for bit-level errors and looking up the destination address in its forwarding table to determine the correct output link. This delay is typically very short, often on the order of microseconds.
  
  2.  **Queuing Delay (`dqueue`):** This is the time a packet spends waiting in a queue (or buffer) before it can be transmitted onto the next link. Queuing delay occurs when a router receives packets faster than it can send them out on a specific link. This is the most variable type of delay; it can be zero if there is no traffic, or it can become very large if the network is congested.
  
  3.  **Transmission Delay (`dtrans`):** This is the time required to push all of the packet's bits onto the link. It is a function of the packet's size (L, in bits) and the transmission rate of the link (R, in bits per second).
    *   **Formula:** `dtrans = L / R`
    *   **Analogy:** Think of a caravan of 10 cars (the packet) getting onto a single-lane highway (the link) from an on-ramp. The transmission rate is how quickly each car can get on the highway. The transmission delay is the time from when the first car starts entering the highway until the last car is fully on it.
  
  4.  **Propagation Delay (`dprop`):** This is the time it takes for a bit to travel from the beginning to the end of a physical link. It is determined by the physical distance of the link (d) and the propagation speed of the medium (s), which is typically close to the speed of light.
    *   **Formula:** `dprop = d / s`
    *   **Analogy:** Continuing the analogy, the propagation delay is the time it takes for the first car to drive from the on-ramp to the next exit, regardless of how many cars are in the caravan.
  
  **Key Distinction:** It is crucial to understand that **transmission delay** is about the packet's *size* and the link's *bandwidth*, while **propagation delay** is about the physical *distance* of the link.
  
  The total delay at a single router (nodal delay) is the sum of these four components: `dnodal = dproc + dqueue + dtrans + dprop`.
#### Packet Loss
  
  Routers have a finite amount of memory, which means their queues (buffers) have a limited size. Packet loss occurs under these conditions:
  
  *   **Full Queues:** When packets arrive at a router where the corresponding queue is already full, the router has no choice but to drop (discard) the newly arriving packet.
  *   **Consequences of Loss:** A lost packet may be retransmitted by the source (if a reliable protocol like TCP is used), which can further increase the overall delay. High rates of packet loss significantly degrade network performance and are a clear sign of network congestion.
#### Throughput
  
  Throughput is the rate (in bits per second) at which bits are successfully transferred between a sender and a receiver.
  
  *   **Instantaneous vs. Average:** Throughput can be measured at a specific point in time (*instantaneous*) or over a longer period (*average*).
  *   **The Bottleneck Link:** In any path from a sender to a receiver, there are typically multiple links, each with its own transmission rate. The end-to-end throughput is constrained by the link with the *lowest* transmission rate. This is known as the **bottleneck link**.
    *   **Example:** If your server is connected to the internet via a 100 Mbps link, but your home internet connection is only 25 Mbps, your throughput for a download will be, at most, 25 Mbps. Your home connection is the bottleneck link. The overall throughput is determined by the minimum of all link speeds along the path.