### **5.3 Intra-AS Routing in the Internet: OSPF**
#### **The Need for Scalable Routing**
  
  The routing algorithms we've studied (LS and DV) are idealized. They assume a "flat" network where all routers are identical and participate in a single routing process. This is not true in practice for two main reasons:
  
  1.  **Scale:** The internet has billions of destinations. It is completely infeasible for every router to store a routing table entry for every destination. The overhead of exchanging routing information (either LS or DV) would overwhelm all network links.
  2.  **Administrative Autonomy:** The internet is a "network of networks." Each network (e.g., a university, a large corporation, an ISP) is managed by its own administrator who wants to control routing *within* their own network. They don't want to be forced to use a specific routing protocol or have their internal structure dictated by others.
  
  **The Solution: Autonomous Systems (AS)**
  
  To solve these problems, the internet aggregates routers into regions called **Autonomous Systems (AS)**, also known as "domains."
  
  *   **Autonomous System (AS):** A group of routers that are all under the same administrative control (e.g., a single ISP's network).
  *   Routing is then split into a two-level hierarchy:
    1.  **Intra-AS Routing (or Intra-domain Routing):** The routing protocols that run *inside* a single AS. The goal is to find the best paths for traffic within that one network. All routers in an AS must run the same intra-AS protocol.
    2.  **Inter-AS Routing (or Inter-domain Routing):** The protocols that route traffic *between* different ASes.
  
  This section focuses on **Intra-AS routing**, and specifically on the most common protocol, OSPF.
#### **Common Intra-AS Routing Protocols**
  
  *   **RIP (Routing Information Protocol):** An early, classic Distance-Vector protocol. It is no longer widely used due to its slow convergence and other limitations.
  *   **EIGRP (Enhanced Interior Gateway Routing Protocol):** A more advanced Distance-Vector protocol developed by Cisco.
  *   **OSPF (Open Shortest Path First):** The most widely used and popular intra-AS routing protocol. It is a **Link-State** protocol.
#### **OSPF (Open Shortest Path First)**
  
  OSPF is a classic, publicly available ("open") link-state routing protocol.
  
  *   **Core Mechanism:**
    1.  **Flooding:** Each router discovers its neighbors and the cost of the links to them. It then broadcasts this information in **OSPF link-state advertisements (LSAs)** to all other routers in the entire AS.
    2.  **Database Creation:** As a result of this flooding, every router in the AS builds an identical and complete database representing the full topology of the AS.
    3.  **Path Calculation:** Each router then runs **Dijkstra's algorithm** on this local copy of the database to compute the shortest-path tree from itself to all other destinations.
    4.  **Forwarding Table:** The results of the Dijkstra calculation are used to populate the router's forwarding table.
  
  *   **Key Features of OSPF:**
    *   **Security:** All OSPF messages are authenticated to prevent malicious routers from injecting false routing information.
    *   **Multiple Cost Metrics:** The cost of a link (the "metric") can be configured by the administrator to represent different things, such as bandwidth or delay.
    *   **Direct IP Encapsulation:** OSPF messages are not sent over TCP or UDP. They are encapsulated directly in IP datagrams, with the protocol field set to 89.
#### **Hierarchical OSPF**
  
  For a large AS (like a major ISP's network), running a single OSPF process across all routers can still be inefficient. The overhead from flooding link-state advertisements can become too high.
  
  To solve this, OSPF can be configured in a **two-level hierarchy**, dividing the AS into **areas**.
  
  *   **Backbone Area (Area 0):** The central area of the AS. Its primary job is to route traffic between the other areas.
  *   **Local Areas:** The other areas in the AS, which connect to the backbone.
  
  **Router Roles in Hierarchical OSPF:**
  *   **Local Routers:** Routers that are entirely within a single local area. They only know the detailed topology of their own area.
  *   **Area Border Routers (ABRs):** Routers that connect one or more local areas to the backbone area. They run OSPF instances for both their area and the backbone. Their key function is to **summarize** the routing information from their local area and advertise these summaries to the backbone. For example, an ABR might tell the backbone, "To reach any of these 20 subnets in my area, just send the traffic to me."
  *   **Backbone Routers:** Routers that are within the backbone area (this includes the ABRs).
  *   **Boundary Routers:** A special type of router that connects the AS to other external ASes. It is responsible for running the inter-AS protocol (BGP).
  
  **How Hierarchical OSPF Works:**
  1.  **Within an Area:** Link-state advertisements are flooded *only* within their own area. A local router computes its forwarding table based on the detailed topology of its area. To route traffic to a destination *outside* its area, it simply forwards the packet to its nearest Area Border Router.
  2.  **Between Areas:** All traffic going from one local area to another must pass through the backbone area. The ABRs advertise summarized routes, so routers in the backbone know which ABR to use to reach a specific area, but they do not know the detailed topology inside that area.
  
  This hierarchy significantly reduces the amount of LSA flooding and the size of the link-state database that each router must maintain, making OSPF scalable to very large networks.