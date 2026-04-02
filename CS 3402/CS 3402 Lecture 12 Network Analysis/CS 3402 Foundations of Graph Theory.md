### **Part 1: Foundations of Graph Theory & Network Analysis**
**(Covering Slides 2–13)**

Until now, we’ve analyzed entities in isolation (e.g., predicting a single house's price based on its square footage). Network Analysis allows us to model the **relationships** *between* entities.
- #### **1. The Core Vocabulary (Slides 3, 10)**
  A **Graph** is the mathematical representation of a network. It consists of two things:
  *   **Nodes (or Vertices):** The actual entities or "things". (e.g., People, Airports, Animals, Neurons).
  *   **Edges (or Links):** The relationships connecting those entities. (e.g., Friendships, Flight paths, "Who eats whom").
- #### **2. Real-World Applications (Slides 4–8)**
  The professor lists several examples to show how universal this math is. If you can define the Nodes and Edges, you can build a graph:
  *   **Flight Routes:** Nodes = Airports. Edges = Direct flights.
  *   **Food Chain:** Nodes = Animals. Edges = Energy transfer (predation).
  *   **Social Networks:** Nodes = People. Edges = Friendships/Interactions.
  *   **Infrastructure:** Nodes = Power stations. Edges = Power lines.
- #### **3. The Two Types of Relationships: Directed vs. Undirected (Slides 11–13)**
  This is the most important fundamental distinction when building a graph.
  
  *   **Undirected Graphs (Slide 11):**
    *   *The Concept:* The relationship is **symmetric** or mutual. There are no arrows on the lines.
    *   *The Analogy:* "A person shook hands with another person." If Person A shook Person B's hand, then Person B definitively shook Person A's hand. 
    *   *Real-World Example:* Facebook friends. (You cannot be friends with someone unless they are also friends with you).
  *   **Directed Graphs (Slide 12):**
    *   *The Concept:* The relationship is **asymmetric** or one-way. The lines have arrows pointing from one node to another.
    *   *The Analogy:* "A person knows another person." Person A might know Taylor Swift, but Taylor Swift does not know Person A. 
    *   *Real-World Example:* Twitter/X followers, Instagram followers, or the Food Chain (A penguin eats fish, but fish do not eat penguins—the energy flows one way).
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Self-Loops:** While not explicitly named on these slides, in directed graphs, a node can have an edge pointing back to itself (e.g., a webpage linking to itself).
  *   **Why use Graphs?** In traditional Machine Learning (like predicting wine quality), we assume every row in our dataset is independent. In Network Analysis, we assume the opposite: that the connections between data points are actually the most valuable pieces of information!
  
  ---