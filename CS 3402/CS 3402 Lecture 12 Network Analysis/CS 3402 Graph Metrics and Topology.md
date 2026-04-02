### **1. Measuring Connections: Degree (Slides 14–15)**
The simplest way to measure a node is by counting its connections. This is called its **Degree**.

*   **In Undirected Graphs (Slide 14):** 
  *   You simply count the number of lines touching the node.
  *   *Slide Question:* "Degree of node D?" $\rightarrow$ Look at the red graph. Node D is connected to Node E and Node B. Therefore, the **Degree of D is 2**.
*   **In Directed Graphs (Slide 15):**
  *   Because traffic flows one-way, we have to split the degree into two metrics:
  *   **In-degree:** How many arrows point *at* you. (In a social network, this is your "Popularity" or follower count).
  *   **Out-degree:** How many arrows point *away* from you. (This is how many people you follow).
  *   *Neural Network Example:* Look at the green graph on Slide 15. The hidden node `h1` has an In-degree of 1 (from $x_1$) and an Out-degree of 1 (to $\hat{y}$).
- ### **2. Measuring Strength: Weighted Graphs (Slides 16–17)**
  In the real world, not all relationships are equal. My relationship with my best friend is "stronger" than my relationship with a random classmate. 
  
  *   **The Concept:** We assign a number (a **Weight**) to the edge.
  *   **Real-World Examples:**
    *   *Maps (Slide 17):* The weight is the **Distance** or **Time**. A road from San Jose to San Francisco has a weight of "15 min". 
    *   *Neural Networks (Slide 16):* The weight is the **Parameter ($w$)** we spent the last 4 lectures trying to optimize! A high weight means the neural network pays a lot of attention to that specific connection.
- ### **3. Navigating the Network: Paths & Components (Slides 18–19)**
  *   **Paths & Shortest Paths (Slide 18):**
    *   A **Path** is a sequence of edges you traverse to get from Node A to Node B.
    *   The **Shortest Path** is the route with the minimum number of edges (or the lowest sum of Weights, like Google Maps finding the fastest route). 
    *   *Slide Question:* "Shortest path from node E to node C?" $\rightarrow$ You must travel **E $\rightarrow$ B $\rightarrow$ A $\rightarrow$ C**. (Length = 3 hops).
  *   **Connected Components (Slide 19):**
    *   Imagine a graph as a group of islands. A **Connected Component** is a single island where every city (node) can reach every other city by some path.
    *   A **Complete Graph** (Fully Connected) is a special graph where *every single node has a direct line to every other node*. (Like a round-robin sports tournament where every team plays every other team).
- ### **4. Who is the most important? Centrality (Slides 20, 24–25)**
  If you are analyzing a terrorist network or a viral outbreak, you want to find the most "important" node so you can isolate it. But "important" can mean different things (Slide 24).
  
  *   **Degree Centrality (Slide 25):** The most basic measure of importance. Who has the most direct friends?
    *   *The Formula:* $$ \text{Degree Centrality} = \frac{\text{Degree}}{n - 1} $$
    *   *Why $(n-1)$?* If there are $n=5$ people in a network, the maximum number of friends you can possibly have is 4 (because you can't be friends with yourself). Dividing by $(n-1)$ normalizes your score to be a clean percentage between 0 and 1.
    *   *Example:* Look at Node B on Slide 25. It has a degree of 3. There are 5 total nodes. Its Degree Centrality is $3 / (5-1) = 3 / 4 = \mathbf{0.75}$ (or 75%).
  *   **Small-World Networks (Slide 20):**
    *   This is the famous "Six Degrees of Kevin Bacon" phenomenon. 
    *   In a massive network (like 8 billion humans), you don't know most people. However, you can reach almost anyone on earth through a short path of mutual friends (usually 4 to 6 steps). This specific topology is called a "Small-World Network."
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Google PageRank:** While not explicitly named, Slide 24 mentions "Connected to influential nodes?". This is exactly how Google Search works. A website is considered "important" (high centrality) not just if many sites link to it, but if *other important sites* (like Wikipedia or CNN) link to it. 
  *   **Network Robustness:** If you remove the node with the highest Degree Centrality, the network often breaks into smaller, disconnected "islands" (components). 
  
  ---