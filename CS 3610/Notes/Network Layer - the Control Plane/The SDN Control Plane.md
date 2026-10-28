### **5.5 The SDN Control Plane**
  
  In the traditional routing architecture we've been discussing, the control plane is **distributed**. Each and every router runs a routing protocol and communicates with its peers to collectively compute the network's forwarding tables.
  
  **Software-Defined Networking (SDN)** fundamentally changes this architecture by:
  1.  **Separating** the control plane from the data plane.
  2.  **Centralizing** the control plane logic.
#### **The SDN Architecture**
  
  The SDN architecture consists of three main components that communicate over well-defined APIs:
  
  1.  **Network-Control Applications (The "Brain"):**
    *   These are the high-level applications that define the network's behavior. They are the "brains" of the control plane.
    *   Examples include applications for routing, access control (firewalling), load balancing, and traffic engineering.
    *   These applications are written by network administrators to express their desired policies (e.g., "all video traffic should use the high-bandwidth path").
    *   They operate on a centralized, network-wide view of the network state.
  
  2.  **The SDN Controller (or Network Operating System):**
    *   The controller is the crucial intermediary between the applications and the network hardware.
    *   It maintains up-to-date, centralized information about the state of the network (e.g., topology, link status, statistics).
    *   It provides a northbound API that allows the network-control applications to read the network state and to write their desired forwarding policies.
    *   It translates these high-level policies into the low-level flow rules that the network switches can understand.
    *   It uses a southbound API (like OpenFlow) to install these flow rules into the data plane of the switches.
  
  3.  **The Data Plane Switches:**
    *   These are the simple, hardware-based forwarding devices.
    *   Their primary job is to execute the "match plus action" rules in their **flow tables** at line speed.
    *   The flow tables are populated and managed by the central SDN controller.
    *   They provide a southbound API (like OpenFlow) that allows the controller to manage their state.
#### **The SDN Controller in Detail**
  
  The SDN controller is the core of the architecture and performs three key functions:
  
  1.  **Communication Layer:** It communicates with the data plane switches via a southbound protocol like OpenFlow. It is responsible for collecting state information from the switches and installing flow rules into them.
  2.  **Network-Wide State-Management Layer:** This is the "crown jewel" of the controller. It aggregates all the state information from all the switches to create a single, consistent, and up-to-date view of the entire network.
  3.  **Network-Control Application Layer:** It provides the northbound API that allows applications to access the centralized network view and to specify their desired behavior.
#### **Example: A Simple SDN Routing Application**
  
  Let's see how a routing algorithm like Dijkstra's would be implemented in an SDN environment.
  
  1.  **State Collection:** The SDN controller uses a discovery protocol (often running on top of OpenFlow) to learn the network topology. It discovers all the switches, the links between them, and their costs. This information is stored in its network state database.
  2.  **Computation:** The Dijkstra's algorithm application, running on top of the controller, reads this complete network graph from the controller's state database.
  3.  **Path Calculation:** The application runs Dijkstra's algorithm to compute the least-cost paths between all source-destination pairs in the network.
  4.  **Flow Rule Generation:** Based on these calculated paths, the application generates the corresponding set of flow rules for each switch. For example, for a path from Host A to Host B that goes through Switch S1 and S2, it would generate a rule for S1 that says "any packet with destination B should be forwarded out port 3," and a similar rule for S2.
  5.  **Installation:** The application uses the controller's northbound API to pass these flow rules to the controller. The controller then uses the southbound API (OpenFlow) to install these rules into the flow tables of the respective switches (S1 and S2).
  
  The key difference is that the computation is done **once** by a central application with a global view, rather than by each router independently with replicated information. This makes the system easier to manage, debug, and evolve.
#### **Advantages of the SDN Approach**
  
  *   **Ease of Network Management:** Network administrators can specify and change complex policies from a single, central point.
  *   **Programmability:** The network becomes programmable. New services and applications can be developed and deployed much more rapidly.
  *   **Open Implementation:** The open nature of protocols like OpenFlow promotes competition and innovation, as applications written for one vendor's controller can work with another vendor's switches.
  *   **Centralized View:** The centralized view of the network allows for more optimal and sophisticated traffic engineering and load balancing than is possible with traditional, distributed protocols.