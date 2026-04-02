### **Part 1: Quiz 2 Logistics & Network Analysis Review**
- #### **1. The Logistics (Slides 2–4)**
  *   **Time & Weight:** 60 minutes, 100 points.
  *   **Scope:** Everything since the last quiz: Network Analysis, Basics of NLP, Intro to DNNs, Tokenizer, and Transformers.
  *   **Structure:** 5 parts ranging from Easy to Medium. You will see Yes/No, Multiple Choice, Short Answer, and Calculations.
- #### **2. True/False & Multiple Choice: Network Analysis (Slides 5–6)**
  
  Let's solve the conceptual questions from the slides.
  
  *   **Degree centrality considers not only the number of connections but also the importance of connected nodes.**
    *   *Answer:* **False.** Degree centrality *only* counts raw connections (edges). (Google's PageRank algorithm is what considers the *importance* of connected nodes).
  *   **In an unweighted network, the degree of a node is the number of edges connected to it.**
    *   *Answer:* **True.**
  *   **Weighted networks consider the strength of connections when computing node importance.**
    *   *Answer:* **True.** (e.g., A 15-minute road vs. a 60-minute road).
  *   **Degree centrality measures how central a node is based on shortest paths through the network.**
    *   *Answer:* **False.** Degree centrality only cares about direct, immediate neighbors.
  *   **A fully connected network means every node is connected to every other node.**
    *   *Answer:* **True.** (Also called a Complete Graph).
  *   **Subgraphs are always disconnected portions of a network.**
    *   *Answer:* **False.** A subgraph is just a smaller piece of a larger graph; it can still be perfectly connected internally.
  *   **Removing a node with the highest degree centrality always disconnects the network.**
    *   *Answer:* **False.** It *might* disconnect the network, but not *always*. If the network is fully connected (like a web), removing one popular node just forces traffic to take a different route.
  *   **Which of the following is true about degree centrality?**
    *   *Answer:* **B. It is based on the number of edges connected to a node.**
- #### **3. Calculation Practice: Degree & Centrality (Slide 15)**
  
  **The Prompt:**
  Given the unweighted graph:
  *   A connected to B, C
  *   B connected to A, C, D
  *   C connected to A, B
  *   D connected to B
  
  **Task 1: Draw the graph.**
  *   *Mental Image:* A, B, and C form a triangle (they are all connected to each other). Node D is a "tail" sticking out, only connected to B. Total Nodes ($n$) = 4.
  
  **Task 2: Compute the degree of each node.**
  *   *Answer:* Just count the connections!
    *   **Degree of A:** 2
    *   **Degree of B:** 3
    *   **Degree of C:** 2
    *   **Degree of D:** 1
  
  **Task 3: Compute degree centrality for each node.**
  *   *Formula:* $\frac{\text{Degree}}{n - 1}$
  *   Since there are $n=4$ nodes, the denominator is $(4 - 1) = \mathbf{3}$.
    *   **Centrality of A:** $2 / 3 \approx \mathbf{0.67}$
    *   **Centrality of B:** $3 / 3 = \mathbf{1.0}$
    *   **Centrality of C:** $2 / 3 \approx \mathbf{0.67}$
    *   **Centrality of D:** $1 / 3 \approx \mathbf{0.33}$
  
  **Task 4: Which node is most central based on degree?**
  *   *Answer:* **Node B**. (It has a centrality of 1.0, meaning it is directly connected to 100% of the other nodes in the network).
- #### **4. Short Answer Practice (Slide 19)**
  
  *   **What is the difference between a weighted and an unweighted network? Give one example of each.**
    *   *Answer:* An **unweighted network** treats all connections as perfectly equal (e.g., Facebook friends—you are either friends or you aren't). A **weighted network** assigns a numerical value or "strength" to the edges (e.g., A GPS map where the edge weights represent the driving time between cities).
  
  ---