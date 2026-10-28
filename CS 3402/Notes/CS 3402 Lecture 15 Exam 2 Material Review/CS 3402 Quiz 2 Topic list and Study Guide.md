### **Part I: Network Analysis (Medium)**
*Focus: Graph terminology, drawing graphs, and calculating centrality.*

*   **Graph Components:**
  *   **Nodes (Vertices):** The entities (e.g., people, airports, neurons).
  *   **Edges (Links):** The relationships between them.
*   **Types of Graphs:**
  *   **Directed vs. Undirected:** Directed has one-way arrows (e.g., Twitter followers, Neural Networks). Undirected has symmetric, two-way lines (e.g., Facebook friends).
  *   **Weighted vs. Unweighted:** Weighted edges have a numerical value representing strength, distance, or capacity. Unweighted treats all connections equally.
  *   **Fully Connected (Complete) Graph:** Every single node connects directly to every other node.
*   **Graph Metrics & Topology:**
  *   **Degree:** The raw number of edges connected to a node. (For directed graphs: split into *In-degree* and *Out-degree*).
  *   **Subgraphs & Components:** A subgraph is a smaller part of a network. A *Connected Component* is an "island" where every node can reach every other node. Subgraphs are **not** always disconnected.
  *   **Small-World Networks:** Most nodes aren't direct neighbors, but you can reach anyone in a few steps (e.g., Six Degrees of Kevin Bacon).
*   **Degree Centrality:**
  *   *Definition:* Measures local importance based *strictly* on the raw number of connections.
  *   *Formula:* $\text{Degree Centrality} = \frac{\text{Degree}}{n - 1}$ (where $n$ is the total number of nodes).
  *   *What it does NOT do:* It does *not* consider the "importance" of the connected nodes, and it does *not* measure the shortest paths through a network.

---
- ### **Part II: Basics of NLP (Easy-Medium)**
  *Focus: The NLP pipeline, history, and how text is mathematically represented.*
  
  *   **The NLP Pipeline:**
    *   Text $\rightarrow$ Clean $\rightarrow$ Tokenize $\rightarrow$ Normalize/Represent $\rightarrow$ AI Model $\rightarrow$ Output.
  *   **Data Cleaning:**
    *   Removing punctuation reduces noise because punctuation usually carries no sentiment meaning. Converting to lowercase shrinks vocabulary size.
  *   **Text Representation Models:**
    *   **Bag-of-Words (BoW):** Counts the frequency of words in a document. **Trap:** It completely loses/ignores word order.
    *   **One-Hot Encoding:** Assigns a unique mathematical vector with a single `1` and the rest `0`s for each token.
    *   **TF-IDF:** Term Frequency $\times$ Inverse Document Frequency. It down-weights common words (like "the") and highlights rare, important words.
    *   **Dense Embeddings (Word2Vec):** Modern approach. Converts words into dense vectors of real numbers (decimals) that capture actual semantic meaning.
  
  ---
- ### **Part III: Tokenizer (Medium)**
  *Focus: How modern LLMs break down text.*
  
  *   **Tokenization Definition:** The process of splitting text into smaller units (words, subwords, or characters) and converting them into **numerical IDs** that a model can read.
  *   **Word-Level vs. Subword Tokenization:**
    *   *Word-level:* Splits by spaces. **Trap:** Fails completely when it sees a new, unseen, or misspelled word (Out-of-Vocabulary error).
    *   *Subword-level (Used by GPT/LLaMA):* Breaks words into syllables or chunks (e.g., "unbelievable" $\rightarrow$ "un", "believ", "able"). 
    *   *Advantages of Subwords:* It dramatically reduces the master vocabulary size (down to ~30k-50k) and can easily handle brand-new/unseen words by building them from known chunks.
  *   **Token to ID:** In GPT-style tokenizers, tokens are mapped to numerical IDs. A single word might be broken into multiple tokens, so one word $\neq$ one token.
  
  ---
- ### **Part IV: Intro to Deep Neural Networks (Easy-Medium)**
  *Focus: The anatomy of a Neural Network and how it learns.*
  
  *   **The Artificial Neuron:**
    *   Applies a **linear transformation** ($z = wx + b$) followed immediately by a **non-linear activation function**.
  *   **Activation Functions:**
    *   *Purpose:* They introduce non-linearity. Without them, a 100-layer network just collapses into a basic linear regression line and cannot learn complex patterns.
    *   *ReLU (Rectified Linear Unit):* Outputs $0$ for any negative input. If positive, it just outputs the number ($\max(0, x)$).
  *   **Network Architecture:**
    *   Adding more layers does **not** always improve performance (it can lead to overfitting or vanishing gradients).
    *   An MLP (Multi-Layer Perceptron) can be modeled as a directed, weighted graph where neurons are nodes and weights are edges.
  *   **Training & Loss:**
    *   **Loss Function:** Measures how well the network predicts the training data (the error).
    *   **Backpropagation:** Uses the gradient of the loss function to update the weights.
  
  ---
- ### **Part V: Transformer Architecture (Medium)**
  *Focus: Why Transformers beat old models, and the mechanics of Self-Attention.*
  
  *   **Parallelization:** Transformers process input sequences in **parallel** (all at once). This is radically different from old RNNs that read left-to-right.
  *   **Positional Encoding:** Because Transformers read everything simultaneously, they have no concept of word order. Positional encoding is mathematically added to the embeddings to give the model information about the sequence order.
  *   **Self-Attention:**
    *   *Purpose:* Allows each token to look at (attend to) **all other tokens** in the sequence to gather context (e.g., figuring out what the pronoun "it" refers to).
  *   **Query, Key, and Value (Q, K, V):**
    *   All three vectors are derived from the *same* input sentence (hence *Self*-Attention).
    *   **Query (Q):** What the current token is looking for.
    *   **Key (K):** The label/information that other tokens hold.
    *   **Value (V):** The actual content used to compute the final output.
    *   *The Match:* The model takes the dot product of $Q$ and $K$ to find compatibility.
  *   **The Attention Formula Scaling:**
    *   We scale the $Q \times K^T$ product by dividing it by $\sqrt{d_k}$. This is done strictly for **numerical stability** (so the numbers don't explode and ruin the Softmax function).
  
  ---
- ###  **Calculations You Must Be Able to Do by Hand:**
  
  1.  **Degree Centrality:** 
    *   Count the connections to a specific node. Divide by (Total Nodes - 1).
  2.  **Building a Bag-of-Words Vector:** 
    *   Given two sentences, create a master vocabulary list (no duplicates). Then, for each sentence, write an array counting how many times each word from the master list appears.
  3.  **Tokenization to IDs:** 
    *   Given a dictionary mapping (e.g., `AI=1, is=2, cool=3`), turn a string into an array of numbers. Don't forget punctuation if it's in the dictionary!
  4.  **Toy Neuron Math (Linear + ReLU):** 
    *   Given $w_1, x_1, w_2, x_2$, and a bias $b$. 
    *   Calculate: $z = (w_1 \times x_1) + (w_2 \times x_2) + b$.
    *   Apply ReLU: If $z$ is negative, the final answer is `0`. If $z$ is positive, the final answer is just $z$.