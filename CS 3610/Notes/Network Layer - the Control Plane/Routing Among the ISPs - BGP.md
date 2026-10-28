### **5.4 Routing Among the ISPs: BGP (Border Gateway Protocol)**
  
  BGP is the de facto **inter-AS** (or inter-domain) routing protocol of the internet. It is often described as the "glue that holds the internet together."
  
  *   **Intra-AS vs. Inter-AS:**
    *   **Intra-AS (e.g., OSPF):** Focuses on performance and finding the optimal, least-cost path *within* a single, trusted administrative domain.
    *   **Inter-AS (BGP):** Focuses on **policy**. An AS administrator needs to control how their traffic is routed and who is allowed to route traffic *through* their network. Performance is a secondary concern to policy.
#### **The Role of BGP**
  
  BGP provides each Autonomous System (AS) with the means to:
  1.  **Obtain reachability information:** Learn which destination networks (prefixes) are reachable through its neighboring ASes. This is done via **eBGP** sessions.
  2.  **Propagate reachability information:** Distribute this external reachability information to all routers *inside* its own AS. This is done via **iBGP** sessions.
  3.  **Determine "good" routes:** Based on the reachability information and its own economic and security **policies**, choose the best route to each destination network.
  
  A BGP router advertises a route by saying: **"I am here, here is a list of networks I can reach, and here is the path to get there."**
#### **BGP Sessions**
  
  BGP does not communicate over UDP. Instead, two BGP routers, called **peers**, exchange BGP messages over a semi-permanent **TCP connection** on port 179.
  
  *   **eBGP (External BGP):** A BGP session between two routers in *different* ASes. This is used to exchange reachability information between providers.
  *   **iBGP (Internal BGP):** A BGP session between two routers in the *same* AS. This is used to ensure all routers within an AS have a consistent view of external routes.
#### **BGP Path Advertisement: The Path Vector Protocol**
  
  BGP is a **Path Vector** protocol. When a gateway router advertises a route to a destination prefix, it includes several **path attributes**. The two most important are:
  
  1.  **AS-PATH:** This is the list of Autonomous Systems that the advertisement has traversed to reach this point.
    *   **Example:** When a gateway in AS3 advertises a route to prefix X, it sends the message `(AS3, X)` to its neighbor in AS2.
    *   The gateway in AS2 receives this, adds its own AS number, and advertises the route `(AS2, AS3, X)` to its neighbor in AS1.
    *   **Loop Detection:** The AS-PATH attribute provides a simple and effective way to detect and prevent routing loops. If a router receives a BGP advertisement that already contains its own AS number in the AS-PATH, it knows this route is a loop and discards it.
  
  2.  **NEXT-HOP:** The IP address of the router interface that begins the AS-PATH. This tells the internal routers where to send traffic destined for that prefix.
  
  **Policy-Based Routing**
  When a gateway router receives a route advertisement, it uses its **import policy** to decide whether to accept or reject the route. This policy is configured by the network administrator and is often based on economic agreements.
  *   **Example:** An ISP typically does not want to provide "transit" service for traffic between two other, larger ISPs. Its policy would be to *not* advertise a route from one provider to another, preventing its network from being used as a bridge between them.
#### **BGP Route Selection**
  
  A gateway router may learn about multiple different paths to the same destination prefix. It must have a process to select the single "best" path. This is a multi-step process that combines both policy and performance:
  
  1.  **Local Preference:** First, the router chooses the route with the highest **local preference value**. This is a policy-based attribute set by the administrator to prefer one path over another (e.g., "always prefer the cheaper provider").
  2.  **Shortest AS-PATH:** If local preference values are equal, the router chooses the route with the shortest AS-PATH (i.e., the one that traverses the fewest Autonomous Systems).
  3.  **Closest NEXT-HOP Router (Hot Potato Routing):** If the AS-PATH lengths are also equal, the router uses **hot potato routing**. It chooses the path that has the closest NEXT-HOP router according to its *intra-domain* routing protocol (OSPF). The idea is to get the packet "out of my network" as quickly and cheaply as possible.
  4.  **Additional Criteria:** If there is still a tie, additional criteria are used to break it.
#### **How BGP Populates Forwarding Tables**
  
  Let's trace how a route learned via BGP makes its way into the forwarding table of a regular, non-gateway router inside an AS.
  
  **Scenario:** Router `1d` inside `AS1` needs to forward a packet to destination `X`, which is in `AS3`.
  
  1.  **eBGP:** The gateway of `AS3` (`3a`) advertises the path `(AS3, X)` to the gateway of `AS2` (`2c`).
  2.  **iBGP:** Router `2c` propagates this information to `2a` via iBGP.
  3.  **eBGP:** Router `2a` advertises the path `(AS2, AS3, X)` to the gateway of `AS1` (`1c`).
  4.  **iBGP:** Now, `1c` (the gateway) knows how to reach X. It uses **iBGP** to advertise this route to all other routers inside `AS1`, including `1d`. The message essentially says, **"To get to X, the path is through me (1c)."**
  5.  **Intra-AS Protocol (OSPF):** Router `1d` now knows it needs to send the packet to `1c`. But how does it get to `1c`? It uses its **intra-domain routing protocol (OSPF)** to find the shortest path *within AS1* to reach gateway `1c`.
  6.  **Forwarding Table Entry:** Based on the OSPF calculation, `1d` determines that to get to `1c`, it must send the packet out of its local interface `1`. Therefore, it creates the forwarding table entry: `(Destination X, Output Interface 1)`.
  
  This illustrates how the inter-AS (BGP) and intra-AS (OSPF) routing protocols work together to populate the forwarding tables. BGP finds the external path, and OSPF finds the internal path to the gateway router that can reach that external path.