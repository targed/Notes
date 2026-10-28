### **3.8 Evolution of Transport Layer Functionality**
  
  The original TCP congestion control algorithms (Tahoe and Reno's AIMD) were designed decades ago for networks that were much slower and had different characteristics than today's internet. As network speeds increased, new challenges emerged that required an evolution in transport-layer functionality.
#### **High-Speed TCP and TCP CUBIC**
  
  *   **The Problem with Standard AIMD on High-Speed Networks:**
    *   On a very fast "fat pipe" (a link with high bandwidth and long delay), the linear additive increase of standard TCP takes a very long time to recover after a packet loss.
    *   For example, on a 10 Gbps link with a 100 ms RTT, after a packet loss cuts the rate in half, it could take over an hour for the standard AIMD algorithm to ramp back up and fully utilize the link again. This is extremely inefficient.
  
  *   **The Solution: TCP CUBIC**
    *   TCP CUBIC is the default congestion control algorithm used in Linux, and thus is one of the most widely deployed versions on the internet today.
    *   **Insight/Intuition:** When a packet loss occurs, the congestion state of the network probably hasn't changed dramatically. Instead of slowly and linearly probing all the way back up, we can be more aggressive initially and more cautious as we approach the last known point of congestion.
    *   **Mechanism:**
        1.  Let **W_max** be the congestion window size right before the last loss event occurred.
        2.  After a loss, the window is cut in half.
        3.  CUBIC then increases its window size (W) as a function of the **cube of the time** since the last loss event. This results in a window growth curve that is:
            *   **Aggressive at first:** It grows very quickly when it is far away from W_max.
            *   **Cautious later:** The growth slows down significantly as it gets closer to W_max, carefully probing to avoid causing another loss event.
    *   **Benefit:** TCP CUBIC can recover and utilize high-speed links much more effectively than traditional TCP Reno.
#### **Delay-Based Congestion Control**
  
  *   **The Problem with Loss-Based Control (AIMD/CUBIC):**
    *   These algorithms are *reactive*. They work by increasing their sending rate until they **induce packet loss**, and only then do they back off.
    *   The goal is to "keep the pipe full," but this often leads to a state of "keeping the pipe just full *enough to cause buffer overflow*." This means router buffers are consistently full, which leads to high latency (**bufferbloat**).
  
  *   **A Different Approach: Delay-Based Control**
    *   **Goal:** To keep the end-to-end pipe "just full, but not fuller." This means keeping the bottleneck link busy, but without filling up its buffers and causing high delays.
    *   **Mechanism:**
        1.  The sender continuously measures the **RTT**.
        2.  It also keeps track of the **RTT_min**, the minimum RTT ever observed on the path (which represents the uncongested, propagation-only delay).
        3.  Congestion is inferred not from loss, but from an **increase in the measured RTT**.
            *   If RTT is close to RTT_min, the path is not congested -> increase sending rate.
            *   If RTT starts to increase significantly, the buffers must be filling up -> decrease sending rate *before* packet loss occurs.
    *   **Benefit:** This approach can achieve high throughput while maintaining low latency.
    *   **Example:** **TCP BBR (Bottleneck Bandwidth and Round-trip propagation time)**, developed by Google, is a prominent delay-based congestion control algorithm used within Google's backbone network.
#### **TCP Fairness**
  
  *   **Fairness Goal:** If *K* TCP sessions are sharing the same bottleneck link of bandwidth *R*, each session should ideally get an average rate of **R/K**.
  *   **Is AIMD Fair?**
    *   Yes, under idealized assumptions (all sessions have the same RTT and are always in the congestion avoidance phase), the AIMD sawtooth mechanism naturally converges to a fair sharing of the bandwidth. Connections with less bandwidth will increase their share, and connections with more will decrease theirs, until they meet at an equal share.
  *   **Real-World Fairness Challenges:**
    1.  **UDP Traffic:** Multimedia applications that use UDP often do not have built-in congestion control. A high-rate UDP stream can take up an unfair share of bandwidth and "starve" the fair-playing TCP connections. There is no "internet police" to enforce fairness.
    2.  **Parallel TCP Connections:** A single application (like a web browser) can open multiple parallel TCP connections to the same server to download objects. If there are 9 existing connections on a link (each getting R/10), and a new application opens 11 parallel connections, it will unfairly grab R/2 of the bandwidth, leaving the original 9 users to share the other half. This gives an unfair advantage to applications that use many parallel connections.