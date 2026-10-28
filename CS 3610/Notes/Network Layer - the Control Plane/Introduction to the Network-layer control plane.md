### **Chapter 5: Network Layer: Control Plane**
#### **5.1 Introduction to the Network-layer Control Plane**
  
  **Recap: Data Plane vs. Control Plane**
  
  To understand the control plane, we must first recall its relationship to the data plane.
  *   **Data Plane:** Responsible for the local, per-router action of **forwarding** a datagram from an input port to an output port. It operates in hardware on a nanosecond timescale.
  *   **Control Plane:** Responsible for the global, network-wide logic of **routing**. It determines the end-to-end paths that datagrams take from source to destination. The primary output of the control plane is the **forwarding table** used by the data plane. It operates in software on a millisecond or second timescale.
  
  **The Goal of the Control Plane: Routing**
  
  The goal of a routing protocol is to determine "good" paths (or routes) for packets to travel from a sending host to a receiving host through the network of routers.
  
  *   **Path:** A path is the sequence of routers a packet traverses to get from source to destination.
  *   **"Good" Path:** The definition of a "good" path is based on a **cost metric**. A good path is typically the one with the least cost. The cost of a link can be defined by the network administrator to represent different things:
    *   A constant value of 1 (to find the path with the fewest hops).
    *   A value inversely related to the link's bandwidth (to prioritize high-speed links).
    *   A value related to the current congestion on the link.
  
  **The Graph Abstraction**
  To solve the problem of finding the least-cost path, the network is modeled as a **graph**, G = (N, E):
  *   **N:** The set of nodes in the graph, which represent the **routers**.
  *   **E:** The set of edges in the graph, which represent the **physical links** between routers.
  *   **Cost:** Each edge (a, b) in E has an associated cost, c(a, b). The cost of a path is the sum of the costs of all edges in that path.
  
  The core task of a routing algorithm is to find the least-cost path between any two nodes in this graph.
  
  **Classification of Routing Algorithms**
  
  Routing algorithms can be classified along two main axes:
  
  1.  **Global vs. Decentralized (Based on Information Scope):**
    *   **Global Algorithms (Link-State):**
        *   In this approach, every router has a **complete picture** of the entire network topology and all link costs.
        *   This global information is typically gathered by having each router broadcast its local link information to all other routers in the network.
        *   These are often called **Link State (LS)** algorithms.
        *   **Example:** Dijkstra's Algorithm.
    *   **Decentralized Algorithms (Distance-Vector):**
        *   In this approach, a router only knows the cost of the links to its directly connected neighbors. It has no initial knowledge of the rest of the network.
        *   It calculates the least-cost path through an iterative process of exchanging information with its neighbors.
        *   These are often called **Distance Vector (DV)** algorithms.
        *   **Example:** Bellman-Ford Algorithm.
  
  2.  **Static vs. Dynamic (Based on How They Adapt):**
    *   **Static Routing:** Routes are configured manually by a network administrator and change very slowly over time.
    *   **Dynamic Routing:** Routers automatically adapt to changes in the network. They can detect a change in a link's cost or a link failure and re-compute their routing paths. The vast majority of routing protocols used today are dynamic.
  
  **Control Plane Implementation Approaches**
  
  As mentioned in the previous chapter, there are two ways the control plane can be implemented:
  
  1.  **Per-Router Control Plane (Traditional):**
    *   The routing algorithms (both LS and DV) run as software components within each and every router.
    *   The routers communicate with each other to exchange control plane information (e.g., broadcasting link states or exchanging distance vectors).
    *   The "brains" of the network are distributed across all the routers.
  
  2.  **Logically Centralized Control Plane (SDN):**
    *   A remote controller computes the forwarding tables for all routers in the network.
    *   The routers become simple forwarding devices that are told what to do by the central controller.
    *   This approach simplifies network management and allows for more flexible and programmable routing policies.