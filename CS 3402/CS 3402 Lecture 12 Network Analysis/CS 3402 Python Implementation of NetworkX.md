### **1. The Library: NetworkX (Slide 21)**
*   **What it is:** A Python package for the creation, manipulation, and study of complex networks.
*   **How to get it:** You install it in your virtual environment using `pip install networkx`.
*   *(Colab Note: If you are using Google Colab or a Jupyter Notebook, you often add an exclamation point to run terminal commands directly in a cell: `!pip install networkx` as seen on Slide 23).*
- ### **2. Building a Graph in Code (Slide 22)**
  The slide provides a very clean code snippet to generate the exact 5-node graph we analyzed in Part 2. Let's break down what each line actually does:
  
  ```python
  import networkx as nx
  ```
  *   Just like `import numpy as np`, we alias NetworkX as `nx` to save typing.
  
  ```python
  G = nx.Graph()
  ```
  *   This initializes an empty **Undirected Graph**. 
  *   *Fill-in info:* If you wanted to make a Directed Graph (with one-way arrows), you would use `G = nx.DiGraph()` instead!
  
  ```python
  G.add_edge('A', 'B')
  G.add_edge('A', 'C')
  G.add_edge('B', 'D')
  G.add_edge('B', 'E')
  G.add_edge('D', 'E')
  ```
  *   *The Magic of NetworkX:* Notice that you never explicitly told Python to "Create Node A." In NetworkX, if you add an edge between two things that don't exist yet, it automatically creates the nodes for you.
- ### **3. Visualizing the Graph (Slide 23)**
  ```python
  nx.draw_networkx(G)
  ```
  *   This command calculates the best layout for the nodes (so they don't overlap too much) and draws the picture.
  *   *Fill-in info (The Hidden Dependency):* NetworkX doesn't actually draw the picture itself! Under the hood, it passes the coordinates to **Matplotlib** (which you installed back in Lecture 3). If you don't have Matplotlib installed, this command will fail.
- ### **4. Google Colab (Slide 23)**
  The professor explicitly suggests running this in **Google Colab**.
  *   *Why?* Colab is essentially a free Jupyter Notebook hosted on Google's cloud servers. It comes with NetworkX and Matplotlib pre-installed, so you can literally just type the code and hit "Play" to see the graph without worrying about your `.venv` or local setups.
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Weighted Edges in Code:** If you wanted to add weights (like the distance between cities from Slide 17), you would simply add a third parameter to the edge command: 
    `G.add_edge('San Jose', 'San Francisco', weight=15)`
  *   **Calculating Centrality:** Once you build this graph `G`, NetworkX can do all the math for you. If you type `nx.degree_centrality(G)`, Python will instantly spit out the math we did by hand in the last section!
  
  ---