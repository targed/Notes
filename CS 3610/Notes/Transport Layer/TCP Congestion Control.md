### **3.7 TCP Congestion Control**
  
  TCP's approach to congestion control is an **end-to-end** mechanism. It infers congestion from observed packet loss and adjusts its sending rate accordingly.
#### **The Core Principle: AIMD (Additive Increase, Multiplicative Decrease)**
  
  TCP's strategy is to gently "probe" the network for available bandwidth and then react quickly when it finds the limit.
  
  *   **Additive Increase:** As long as no packets are being lost, TCP assumes the network is not congested and slowly increases its sending rate. It increases its rate by **1 Maximum Segment Size (MSS)** every **RTT**. This is a gentle, linear increase.
  *   **Multiplicative Decrease:** When a packet loss event occurs, TCP assumes it has hit the network's capacity and caused congestion. It reacts drastically by **cutting its sending rate in half**.
  
  This combination creates a characteristic "sawtooth" pattern in TCP's sending rate over time. It continuously probes for bandwidth, backs off, and then starts probing again. This algorithm has been proven to be stable and fair, allowing multiple TCP connections to share a bottleneck link effectively.
#### **How TCP Controls its Sending Rate: The Congestion Window (`cwnd`)**
  
  TCP does not directly control its sending rate in "Mbps." Instead, it manages the number of unacknowledged ("in-flight") bytes it is allowed to have in the network. This limit is controlled by a new sender-side variable called the **Congestion Window**, or **`cwnd`**.
  
  *   **The Rule:** The sender ensures that the amount of unacknowledged data is always less than or equal to the `cwnd`.
    $ \text{LastByteSent} - \text{LastByteAcked} \le \text{cwnd} $
  *   **Relationship to Sending Rate:** By controlling `cwnd`, TCP indirectly controls its sending rate.
    $ \text{TCP sending rate} \approx \frac{\text{cwnd}}{\text{RTT}} $
    A larger `cwnd` allows more packets to be in flight, resulting in a higher sending rate.
  
  **Important Note:** The *actual* amount of data a TCP sender can have in flight is the **minimum** of the congestion window (`cwnd`) and the flow control window (`rwnd`).
#### **The Three Phases of TCP Congestion Control**
  
  TCP's congestion control algorithm is not a single mechanism but a state machine that operates in three distinct phases.
  
  **1. Slow Start**
  *   **Goal:** To quickly ramp up the sending rate at the beginning of a connection to find the available bandwidth.
  *   **Mechanism:**
    *   The connection begins with a very small `cwnd`, typically **1 MSS**.
    *   For every ACK that is received, the sender **increases `cwnd` by 1 MSS**.
    *   **The Result:** This causes the `cwnd` (and thus the sending rate) to **double every RTT**. The rate increases *exponentially*.
  *   **Why is it called "Slow Start"?** Because it *starts* from a very slow rate (1 MSS), even though it grows exponentially fast.
  
  **When does the exponential growth stop?** It stops when `cwnd` reaches a threshold value.
  
  *   **`ssthresh` (Slow Start Threshold):** A variable that determines when to switch from the aggressive exponential growth of Slow Start to the more cautious linear growth of Congestion Avoidance.
  *   Initially, `ssthresh` is set to a very large value.
  
  **2. Congestion Avoidance**
  *   **Goal:** To gently probe for additional bandwidth once the sending rate is near the network's estimated capacity.
  *   **Mechanism:**
    *   This phase begins when `cwnd` becomes greater than or equal to `ssthresh`.
    *   In this phase, `cwnd` is increased by just **1 MSS per RTT**. This is a **linear increase**, much more cautious than the exponential growth of Slow Start.
  
  **3. Fast Recovery**
  This is the phase TCP enters in response to a "mild" congestion event.
  
  *   **How Congestion is Detected:** TCP has two ways of detecting packet loss:
    1.  **Timeout:** The sender's timer for a segment expires. This is considered a **major congestion event**, indicating that the network is heavily congested.
    2.  **Triple Duplicate ACKs:** The sender receives three duplicate ACKs. This is considered a **mild congestion event**, indicating that only a single packet was likely lost.
  
  *   **TCP's Reaction to a Loss Event:**
    *   **If a timeout occurs (major congestion):**
        1.  The `ssthresh` is set to half of the `cwnd` value at the time of the loss.
        2.  `cwnd` is reset all the way back down to **1 MSS**.
        3.  The sender re-enters the **Slow Start** phase.
        (This behavior is from an older version called TCP Tahoe).
    *   **If three duplicate ACKs are received (mild congestion):**
        1.  The `ssthresh` is set to half of the `cwnd` value.
        2.  `cwnd` is set to `ssthresh` + 3 MSS.
        3.  The sender retransmits the missing segment and enters the **Fast Recovery** phase, where it continues to operate with a reduced, but still high, `cwnd`. It bypasses the Slow Start phase.
        (This optimized behavior is from a more modern version called TCP Reno).
#### **Summary of the TCP Congestion Control FSM**
  *   A connection starts in **Slow Start**, with `cwnd` growing exponentially.
  *   When `cwnd` reaches `ssthresh`, the connection transitions to **Congestion Avoidance**, with `cwnd` growing linearly.
  *   If a **timeout** occurs in either phase, `ssthresh` is halved, `cwnd` is reset to 1, and the connection returns to **Slow Start**.
  *   If **three duplicate ACKs** are received, `ssthresh` is halved, `cwnd` is also halved, and the connection enters **Fast Recovery**.
  
  This sophisticated algorithm allows TCP to be both aggressive in finding bandwidth and cautious in avoiding network collapse, making it the workhorse of the modern internet.