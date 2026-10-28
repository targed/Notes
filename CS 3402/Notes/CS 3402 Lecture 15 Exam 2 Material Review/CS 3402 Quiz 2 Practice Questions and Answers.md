### **Practice Quiz: Part I - Network Analysis (Medium)**

**Question 1 (True/False)**
When calculating Degree Centrality, the formula inherently gives a higher score to a node if it is connected to other highly important, influential nodes.

**Question 2 (Multiple Choice)**
Which of the following real-world scenarios is best modeled using a **Directed, Weighted** graph?
A) A network of Facebook friends, where an edge exists if two people have accepted a friend request.
B) A map of city intersections, where the edges represent one-way streets and the values represent the driving time in minutes.
C) A graph of actors who have co-starred in a movie together.
D) A network of Twitter (X) users, where an edge represents someone hitting the "Follow" button on another user's profile.

**Question 3 (Calculation)**
You are given a network with 5 nodes (V, W, X, Y, Z). It is an **unweighted, undirected** graph with the following connections:
*   V is connected to W, X, Y, and Z.
*   W is connected to V and X.
*   X is connected to V, W, and Y.
*   Y is connected to V and X.
*   Z is connected only to V.

**(a)** What is the **degree** of Node X?
**(b)** Calculate the **Degree Centrality** of Node V. 
**(c)** Calculate the **Degree Centrality** of Node Z.

**Question 4 (Short Answer)**
In a few sentences, explain the difference between a **Connected Component** and a **Complete (Fully Connected) Graph**.

**Question 5 (True/False)**
Removing the node with the highest degree centrality will always cause the network to break into multiple disconnected subgraphs.

**Question 6 (Multiple Choice)**
Which of the following statements is **TRUE** regarding Network Analysis?
A) Degree centrality measures how central a node is based on the shortest paths through the network.
B) Subgraphs are, by definition, disconnected portions of a larger network.
C) In a directed graph, the "In-degree" refers to the number of incoming edges pointing at a node.
D) Unweighted networks consider the strength of connections when computing node importance.

**Question 7 (Calculation/Logic)**
Imagine a **Fully Connected (Complete)** undirected graph that contains exactly 8 nodes. 
**(a)** What is the exact degree of every single node in this graph?
**(b)** What is the Degree Centrality score for every node?

**Question 8 (Short Answer)**
What is a **Small-World Network**? Provide one brief real-world example of this phenomenon.

***
***
*(Stop here! Complete the questions before scrolling down to the Answer Key)*
***
***
<br><br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & DETAILED EXPLANATIONS**
  
  **Question 1 Answer: False**
  *   **Explanation:** This is a classic trick question from your professor's slides. Degree centrality *only* cares about the raw quantity of connections (edges) a node has. It does not care how "important" those neighbors are. (Algorithms like Google's PageRank or Eigenvector Centrality measure neighbor importance, but Degree Centrality does not).
  
  **Question 2 Answer: B**
  *   **Explanation:** Let's break down the requirements: "Directed" (one-way arrows) and "Weighted" (numerical values on the edges). 
    *   A is Undirected and Unweighted.
    *   *B is Directed (one-way streets) and Weighted (driving time in minutes).*
    *   C is Undirected and Unweighted.
    *   D is Directed (you can follow someone without them following you back), but it is Unweighted (a follow is just a follow).
  
  **Question 3 Answer:**
  *   **(a) Degree of Node X: 3** (It connects to V, W, and Y).
  *   **(b) Centrality of Node V: 1.0** 
    *   *Math:* Node V connects to 4 things. The formula is $\frac{\text{Degree}}{n-1}$. Since there are 5 total nodes, the denominator is $5-1 = 4$. $4 / 4 = 1.0$. (This means V is connected to 100% of the other nodes).
  *   **(c) Centrality of Node Z: 0.25**
    *   *Math:* Node Z only connects to V (Degree = 1). $1 / 4 = 0.25$. 
  
  **Question 4 Answer:**
  *   **Explanation:** A **Connected Component** is simply an "island" in a network where every node has *some path* to reach every other node, even if it takes many hops. A **Complete Graph** is a much stricter specific structure where every single node has a *direct, immediate edge* connecting it to every other node in the network.
  
  **Question 5 Answer: False**
  *   **Explanation:** While removing a highly central node (like a major airport) *might* disrupt a network, it doesn't *always* disconnect it. If the network has many redundant paths or is highly interconnected, the remaining nodes can still reach each other through alternative routes.
  
  **Question 6 Answer: C**
  *   **Explanation:** 
    *   A is false (Degree centrality only looks at immediate neighbors, not shortest paths).
    *   B is false (Subgraphs are just smaller sections of a graph; they aren't necessarily disconnected from the main graph).
    *   **C is true.** (In-degree = arrows pointing IN; Out-degree = arrows pointing OUT).
    *   D is false (Unweighted networks treat all edges equally; Weighted networks consider strength).
  
  **Question 7 Answer:**
  *   **(a) Degree: 7** (In a fully connected graph, you are connected to everyone except yourself. Therefore, $8 \text{ nodes} - 1 = 7$ connections per node).
  *   **(b) Centrality: 1.0** (Since the degree is 7, and the formula is $\frac{\text{Degree}}{n-1} \rightarrow \frac{7}{7} = 1.0$. In a fully connected graph, every node has a perfect centrality score!).
  
  **Question 8 Answer:**
  *   **Explanation:** A Small-World Network is a graph where most nodes are not direct neighbors, but almost any node can be reached from any other node in a very small number of steps. 
  *   *Example:* The "Six Degrees of Kevin Bacon" game among actors, or the global human social network where you are roughly 6 handshakes away from anyone on Earth.
  
  ***
- ---
- ---
- ---
- ### **Practice Quiz: Part II - Basics of NLP & Tokenization (Easy-Medium)**
  
  **Question 1 (True/False)**
  In natural language processing, word-level tokenization is considered superior to subword tokenization because it always handles new, out-of-vocabulary (unseen) words effectively.
  
  **Question 2 (Multiple Choice)**
  What is the primary function of a **Tokenizer** in the NLP pipeline?
  A) To translate text from one human language to another automatically.
  B) To train dense embeddings for words so the model understands their meaning.
  C) To split raw text into smaller units (like words or subwords) and map them to numerical IDs.
  D) To normalize punctuation and convert all characters to lowercase.
  
  **Question 3 (Calculation: Bag of Words)**
  You are given the following two short reviews for a restaurant:
  *   **S1:** "food is great"
  *   **S2:** "great food is good"
  
  **(a)** Build the complete **Bag-of-Words vocabulary** for this dataset. (Keep them lowercase, ignore punctuation).
  **(b)** Represent sentence **S1** as a mathematical vector based on your vocabulary.
  **(c)** Represent sentence **S2** as a mathematical vector based on your vocabulary.
  
  **Question 4 (Short Answer)**
  Looking at the Bag-of-Words vectors you just generated in Question 3, does this specific text representation method capture **word order**? Briefly explain why or why not.
  
  **Question 5 (Multiple Choice)**
  During the "Data Cleaning" phase of an NLP pipeline for **Sentiment Analysis**, data scientists frequently use regular expressions to remove punctuation symbols (like commas, periods, and exclamation marks). Why is this done?
  A) Because punctuation symbols usually do not carry sentiment meaning, so removing them cleans the text and reduces noise.
  B) Because punctuation symbols require their own separate neural network to process.
  C) Because subword tokenizers like BPE are incapable of processing punctuation.
  D) Because removing them preserves the exact word order of the sentence.
  
  **Question 6 (Calculation: Tokens to IDs)**
  You are given the following sentence: `"NLP models are smart."`
  You are also given the following Token ID dictionary:
  `models: 1`
  `smart: 2`
  `.: 3`
  `are: 4`
  `NLP: 5`
  
  **(a)** Tokenize the sentence exactly as a basic tokenizer would (separating words and punctuation).
  **(b)** Convert the sequence of tokens into a final array of **numerical IDs**.
  
  **Question 7 (True/False)**
  One-hot encoding is a text representation method that assigns a unique vector to each token, where the vector contains a single `1` and all other values are `0`s.
  
  **Question 8 (Multiple Choice)**
  Modern Large Language Models (LLMs) like GPT-4 and Gemini do not split text into whole words. Instead, they use **Subword Tokenization**. Which of the following is a major advantage of this approach?
  A) It eliminates the need to assign numerical IDs to tokens.
  B) It reduces the master vocabulary size and allows the model to handle rare or unseen words by building them from known chunks.
  C) It guarantees that the model will perfectly understand the semantic meaning of the text.
  D) It ensures that every single token corresponds exactly to one English word.
  
  ***
  ***
  *(Stop here! Complete the questions before scrolling down to the Answer Key)*
  ***
  ***
  <br><br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & DETAILED EXPLANATIONS**
  
  **Question 1 Answer: False**
  *   **Explanation:** Word-level tokenization *fails* at handling out-of-vocabulary (OOV) words. If a word-level tokenizer sees a brand new word it hasn't been trained on (like a typo or new slang), it crashes or replaces it with an `<UNK>` (unknown) token. **Subword tokenization** is the method that handles unseen words effectively by breaking them down into known syllables/chunks.
  
  **Question 2 Answer: C**
  *   **Explanation:** The Tokenizer's only job is to chop the text into pieces (tokens) and look up their corresponding numbers (IDs) in a dictionary. Training embeddings (B) happens later in the model. Normalizing/lowercasing (D) happens earlier during Data Cleaning. Translation (A) is an end-goal application, not a pipeline step.
  
  **Question 3 Answer (Bag of Words Calculation):**
  *   **(a) Vocabulary:** `['food', 'is', 'great', 'good']` *(Note: The order you put the words in your dictionary doesn't matter, as long as it is consistent. I will use this order for the vectors below).*
  *   **(b) S1 Vector:** `"food is great"` $\rightarrow$ **`[1, 1, 1, 0]`** (1 food, 1 is, 1 great, 0 good).
  *   **(c) S2 Vector:** `"great food is good"` $\rightarrow$ **`[1, 1, 1, 1]`** (1 food, 1 is, 1 great, 1 good).
  
  **Question 4 Answer:**
  *   **Explanation: No, it does not capture word order.** Bag-of-Words only counts the *frequency* (how many times a word appears). The sentence "food is great" and the sentence "great is food" would produce the exact same mathematical vector (`[1, 1, 1, 0]`), rendering the model blind to the sequential grammar of the sentence.
  
  **Question 5 Answer: A**
  *   **Explanation:** This is straight from Lecture 13, Slide 18. Punctuation usually does not carry sentiment. By stripping it out, we reduce the "noise" in the data and shrink the vocabulary size (so the computer doesn't think `"amazing!"` and `"amazing."` are two completely different words).
  
  **Question 6 Answer (Tokens to IDs):**
  *   **(a) Tokens:** **`["NLP", "models", "are", "smart", "."]`** *(Trap check: Did you separate the period at the end? Punctuation marks are their own tokens!)*
  *   **(b) Numerical IDs:** **`[5, 1, 4, 2, 3]`** *(Using the provided dictionary to map the strings to numbers).*
  
  **Question 7 Answer: True**
  *   **Explanation:** This is the exact definition of One-hot encoding. If your vocabulary has 5 words, the 3rd word is represented as `[0, 0, 1, 0, 0]`.
  
  **Question 8 Answer: B**
  *   **Explanation:** This was highlighted heavily in Lecture 13 (Slide 28) and Lecture 15. Subword tokenization (like BPE) prevents the model from needing a 5-million-word dictionary. It shrinks the vocabulary to around 30k–50k tokens, saving massive amounts of RAM, while allowing the model to piece together unseen words (like "bioinformaticsAItool") from familiar chunks.
  
  ***
- ---
- ---
- ---
- ### **Practice Quiz: Part III - Intro to DNNs & Transformers (Medium)**
  
  **Question 1 (True/False)**
  When designing an Artificial Neural Network, adding more layers to the architecture will *always* improve the model's performance and accuracy.
  
  **Question 2 (Calculation: Toy MLP)**
  You are given a toy Multi-Layer Perceptron (MLP) with a single artificial neuron. The equation for the neuron is:
  $y = \text{ReLU}(w_1x_1 + w_2x_2 + b)$
  
  You are given the following values:
  *   **Inputs:** $x_1 = 1$, $x_2 = 2$
  *   **Weights:** $w_1 = 0.5$, $w_2 = -1$
  *   **Bias:** $b = 1$
  
  **(a)** Compute the raw linear output (before the activation function is applied).
  **(b)** Apply the ReLU activation function to your answer from (a). What is the final output ($y$)?
  
  **Question 3 (Short Answer)**
  What is the mathematical role of an **activation function** (like ReLU, Sigmoid, or Tanh) in a neural network? What would happen to a 100-layer neural network if you removed all the activation functions?
  
  **Question 4 (Multiple Choice)**
  Which of the following statements is **TRUE** regarding how Deep Neural Networks learn?
  A) The loss function updates the weights directly without needing gradients.
  B) Backpropagation is used to update the weights of a network using the gradient of the loss function.
  C) The ReLU activation function outputs a negative number for negative inputs.
  D) The loss function measures how well the neural network predicts the *testing* data during the training loop.
  
  **Question 5 (True/False)**
  One of the main reasons Transformers are so much faster to train than older Recurrent Neural Networks (RNNs) is because Transformers process input sequences sequentially (one word at a time).
  
  **Question 6 (Multiple Choice)**
  What is the primary purpose of **Self-Attention** in a Transformer model?
  A) To translate text automatically into different languages.
  B) To compute the final output token probabilities directly using Softmax.
  C) To normalize the input tokens so they don't cause gradients to explode.
  D) To allow each token to attend to (look at) all other tokens in the sequence to gather context.
  
  **Question 7 (Short Answer)**
  In the Self-Attention mechanism, the mathematical formula uses $Q$, $K$, and $V$. Briefly explain the conceptual roles of **Query (Q)**, **Key (K)**, and **Value (V)**.
  
  **Question 8 (Multiple Choice)**
  Why is **Positional Encoding** absolutely necessary in the Transformer architecture?
  A) To prevent vanishing gradients in extremely deep networks.
  B) To reduce the total number of parameters the model has to learn.
  C) Because Transformers process all tokens in parallel, they need positional encodings to give the model information about the order of the words.
  D) To normalize the token embeddings so their values stay between 0 and 1.
  
  ***
  ***
  *(Stop here! Complete the questions before scrolling down to the Answer Key)*
  ***
  ***
  <br><br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & DETAILED EXPLANATIONS**
  
  **Question 1 Answer: False**
  *   **Explanation:** Adding more layers makes a model more complex, but it does *not* always improve performance. If the dataset is small, a highly complex model will just memorize the noise (Overfitting). Furthermore, adding too many layers can cause mathematical issues like the Vanishing Gradient problem.
  
  **Question 2 Answer (Toy MLP Calculation):**
  *   **(a) Linear Output: -0.5**
    *   *Math:* $z = (w_1 \times x_1) + (w_2 \times x_2) + b$
    *   $z = (0.5 \times 1) + (-1 \times 2) + 1$
    *   $z = 0.5 - 2 + 1 = \mathbf{-0.5}$
  *   **(b) Final Output with ReLU: 0**
    *   *Explanation:* The rule for ReLU is $\max(0, x)$. If the input is negative, ReLU blocks it and outputs exactly `0`. If the input is positive, it passes it through unchanged. Since $-0.5$ is negative, the final output is **$0$**.
  
  **Question 3 Answer:**
  *   **Explanation:** An activation function introduces **non-linearity** into the network, allowing it to model highly complex, curved boundaries and functions. If you removed all activation functions, the entire 100-layer network would mathematically collapse into a single, basic linear transformation (a straight line), destroying its ability to solve complex problems like image recognition or NLP.
  
  **Question 4 Answer: B**
  *   **Explanation:** 
    *   A is false (you *must* have gradients to update weights).
    *   **B is true.** (This is the definition of the training loop).
    *   C is false (ReLU outputs *zero* for negative inputs, not a negative number).
    *   D is false (The loss function during training measures predictions against the *training* data, not the testing data).
  
  **Question 5 Answer: False**
  *   **Explanation:** This is a classic trap. Transformers process sequences in **parallel** (all tokens at the exact same time), which is why they are so fast on GPUs. Older RNNs processed them sequentially (word by word), which created massive bottlenecks.
  
  **Question 6 Answer: D**
  *   **Explanation:** Self-Attention exists to give words **context**. By looking at all the other tokens in a sentence simultaneously, the model can figure out grammar, sentiment, and coreference resolution (e.g., figuring out that the pronoun "it" refers to "the street" instead of "the animal").
  
  **Question 7 Answer:**
  *   **Explanation:** Think of a database retrieval system:
    *   **Query (Q):** What the current token is *looking for* (e.g., "I am an adjective looking for the noun I modify").
    *   **Key (K):** The *labels or tags* that describe the other tokens in the sentence (e.g., "I am a noun").
    *   **Value (V):** The actual *meaning/content* of the token that gets pulled to compute the final output. 
    *   *(The model multiplies Q and K to see if there is a match, and if so, it absorbs the information from V).*
  
  **Question 8 Answer: C**
  *   **Explanation:** Because the Transformer processes the entire sentence in parallel (all at once), it inherently has no concept of what order the words were typed in. Without positional encodings physically adding a "timestamp" to the vectors, the sentence "The dog bit the man" and "The man bit the dog" would look mathematically identical to the AI.
  
  ***
- ---
- ---
- ---
- # **CS 3402: Intro to Data Science - Quiz 2 (Practice Version C)**
  **Time Limit: 60 Minutes | Total Points: 100**
- ### **Part I: Network Analysis (Medium) [20 Points]**
  
  **1. (True/False) [2 pts]**
  In a *directed* graph, the "out-degree" of a node refers to the number of incoming edges pointing towards it.
  
  **2. (Multiple Choice) [4 pts]**
  Which of the following real-world networks is best modeled using an **unweighted, undirected** graph?
  A) A GPS navigation map where edges represent driving time in minutes.
  B) A co-authorship network where an edge exists if two scientists have written a paper together.
  C) The food chain, where an edge points from the prey to the predator.
  D) A Twitter network where an edge points from a follower to the account they are following.
  
  **3. (Calculation)[8 pts]**
  You are analyzing a small computer server network with 4 nodes ($A, B, C, D$). The graph is **unweighted and undirected**.
  The connections are as follows:
  *   **A** is connected to **B**, **C**, and **D**.
  *   **B** is connected to **A**.
  *   **C** is connected to **A** and **D**.
  *   **D** is connected to **A** and **C**.
  
  **(a)** What is the degree of Node C?
  **(b)** Calculate the **Degree Centrality** of Node A. *(Show your fraction/math).*
  **(c)** Calculate the **Degree Centrality** of Node B. *(Show your fraction/math).*
  
  **4. (Short Answer) [6 pts]**
  What is a **Small-World Network**, and what are the two main characteristics that define it according to the lecture?
  
  ---
- ### **Part II: Basics of NLP (Easy-Medium) [20 Points]**
  
  **5. (True/False) [2 pts]**
  Representing a sentence using the **TF-IDF** (Term Frequency-Inverse Document Frequency) method preserves the grammatical word order of the original sentence perfectly.
  
  **6. (Multiple Choice) [4 pts]**
  In the TF-IDF representation formula, what is the primary purpose of the **IDF (Inverse Document Frequency)** component?
  A) To completely remove punctuation from the text.
  B) To severely down-weight extremely common words (like "the" or "is") that appear in almost every document.
  C) To highlight words that appear very rarely in the current document but often everywhere else.
  D) To split long words into smaller subword chunks.
  
  **7. (Calculation) [8 pts]**
  You are building an NLP pipeline. You have the following two sentences (already lowercased and cleaned):
  *   **S1:** `"ai is fast"`
  *   **S2:** `"ai is smart and fast"`
  
  **(a)** Build the complete **Bag-of-Words vocabulary** array for this mini-dataset.
  **(b)** Represent sentence **S1** as a mathematical vector based on your vocabulary.
  **(c)** Represent sentence **S2** as a mathematical vector based on your vocabulary.
  
  **8. (Short Answer) [6 pts]**
  During the "Data Cleaning" phase of an NLP pipeline for **Sentiment Analysis**, why is it standard practice to use regular expressions to remove punctuation symbols (like `!`, `?`, `,`)? 
  
  ---
- ### **Part III: Intro to DNNs (Easy-Medium) [20 Points]**
  
  **9. (True/False) [2 pts]**
  The primary goal of training an Artificial Neural Network via Gradient Descent is to *maximize* the loss function.
  
  **10. (Multiple Choice) [4 pts]**
  What happens to a deep, 50-layer neural network if you build it *without* any non-linear activation functions?
  A) It runs out of memory (RAM) and crashes.
  B) It learns non-linear patterns much faster.
  C) It mathematically collapses into a single, basic linear transformation, rendering the hidden layers useless.
  D) It automatically converts itself into a Support Vector Machine.
  
  **11. (Calculation: Toy MLP with Sigmoid) [10 pts]**
  You are given a toy neural network with a single neuron. Instead of ReLU, this neuron uses the **Sigmoid** activation function: $\sigma(z) = \frac{1}{1 + e^{-z}}$.
  The neuron equation is: $y = \text{Sigmoid}(w_1x_1 + w_2x_2 + b)$
  
  You are given the following values:
  *   **Inputs:** $x_1 = 2$, $\; x_2 = -1$
  *   **Weights:** $w_1 = 0.5$, $\; w_2 = 1.0$
  *   **Bias:** $b = 0$
  
  **(a)** Compute the raw linear output ($z$) before the activation function is applied.
  **(b)** Apply the Sigmoid function to your answer from (a). What is the final output ($y$)? *(Hint: $e^0 = 1$)*.
  
  **12. (Short Answer) [4 pts]**
  In an MLP (Multi-Layer Perceptron), what does it mean for a layer to be **"fully connected"** (Dense)?
  
  ---
- ### **Part IV: Tokenizer (Medium)[20 Points]**
  
  **13. (True/False) [2 pts]**
  Modern Large Language Models (like GPT-4) use whole-word tokenization because it perfectly maps exactly one English word to exactly one numerical ID.
  
  **14. (Multiple Choice) [4 pts]**
  How does a **Subword Tokenizer** (like BPE) handle a completely new, made-up word it has never seen before, such as `"Cyberthon2026"`?
  A) It crashes and throws an "Out of Vocabulary" (OOV) error.
  B) It replaces the entire word with a single `<UNK>` (unknown) token.
  C) It breaks the new word down into smaller, known subword chunks (e.g., `"Cyber"`, `"th"`, `"on"`, `"2026"`).
  D) It ignores the word and moves to the next sentence.
  
  **15. (Calculation) [8 pts]**
  You are given the following sentence: `"Data Science is fun!"`
  You have the following Token ID dictionary:
  `Science: 10`
  `is: 11`
  `fun: 12`
  `!: 13`
  `Data: 14`
  
  **(a)** Tokenize the sentence exactly as a basic tokenizer would (separating words and punctuation).
  **(b)** Convert your sequence of tokens into a final array of numerical IDs.
  
  **16. (Short Answer)[6 pts]**
  Give two distinct mathematical or computational advantages of using **Subword Tokenization** over traditional whole-word tokenization.
  
  ---
- ### **Part V: Transformer (Medium) [20 Points]**
  
  **17. (True/False) [2 pts]**
  In a Transformer model, the Self-Attention mechanism reads the tokens sequentially (one-by-one from left to right) just like a human reading a book.
  
  **18. (Multiple Choice)[4 pts]**
  In the Self-Attention equation $Attention(Q, K, V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V$, what is the specific purpose of dividing by $\sqrt{d_k}$?
  A) To add positional information to the vectors.
  B) To scale the numbers down, providing numerical stability so the Softmax function doesn't break due to exploding values.
  C) To convert the continuous vectors into discrete Bag-of-Words IDs.
  D) To filter out negative numbers, similar to a ReLU function.
  
  **19. (Matching / Short Answer) [8 pts]**
  In the Self-Attention mechanism, words look at each other using **Q, K, and V** vectors. Briefly define the conceptual role of each letter in the context of an information retrieval/database analogy.
  *   **Query (Q):** 
  *   **Key (K):** 
  *   **Value (V):** 
  
  **20. (Short Answer) [6 pts]**
  Because Transformers process all tokens in parallel, they inherently have no concept of word order. How does the Transformer architecture solve this problem so it can tell the difference between `"The man bit the dog"` and `"The dog bit the man"`?
  
  ***
  ***
  ***
  *(Stop here! Complete the exam before scrolling down to the Answer Key)*
  ***
  ***
  ***
  
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & GRADING RUBRIC**
  
  **Part I: Network Analysis**
  1.  **False.** (Out-degree is the number of edges pointing *away* from the node. In-degree points towards it). (2 pts)
  2.  **B.** (Co-authorship has no direction [if A wrote with B, B wrote with A] and no weight [just a binary yes/no]). (4 pts)
  3.  **(a) Degree of C:** **2** (Connected to A and D). (2 pts)
    **(b) Centrality of A:** Degree is 3. Formula is $3 / (4-1) = 3 / 3 = \mathbf{1.0}$. (3 pts)
    **(c) Centrality of B:** Degree is 1. Formula is $1 / (4-1) = \mathbf{0.333}$ (or 1/3). (3 pts)
  4.  **Answer:** A small-world network is a graph where **1) most nodes are not direct neighbors** of each other, but **2) most nodes can be reached from any other node in a very few number of steps** (e.g., Six Degrees of Kevin Bacon). (6 pts)
  
  **Part II: Basics of NLP**
  5.  **False.** (TF-IDF, like Bag of Words, is a frequency-based method and completely destroys/loses the sequential order of words). (2 pts)
  6.  **B.** (It down-weights words that appear in many documents, so rare/important words get higher mathematical focus). (4 pts)
  7.  **(a) Vocabulary:** `['ai', 'is', 'fast', 'smart', 'and']` *(Order may vary, but must have these 5 unique words).* (2 pts)
    **(b) S1 Vector:** **`[1, 1, 1, 0, 0]`** (2 pts)
    **(c) S2 Vector:** **`[1, 1, 1, 1, 1]`** (4 pts)
  8.  **Answer:** Punctuation symbols (usually) do not carry sentiment meaning (e.g., "amazing" and "amazing!" mean the same thing). Removing them **cleans the text, reduces noise**, and significantly **shrinks the vocabulary size**. (6 pts)
  
  **Part III: Intro to DNNs**
  9.  **False.** (The goal is to *minimize* the loss function, reducing the error to zero). (2 pts)
  10. **C.** (Without non-linear activation functions, the whole network mathematically collapses into a basic linear model). (4 pts)
  11. **(a) Linear Output ($z$):** $z = (0.5 \times 2) + (1.0 \times -1) + 0 = 1.0 - 1.0 = \mathbf{0}$. (5 pts)
    **(b) Sigmoid Output ($y$):** $\sigma(0) = \frac{1}{1 + e^0}$. Since $e^0 = 1$, the math is $\frac{1}{1+1} = \frac{1}{2} = \mathbf{0.5}$. (5 pts)
  12. **Answer:** "Fully connected" means that every single neuron in the current layer has an edge (weighted connection) to **every single neuron** in the subsequent layer. (4 pts)
  
  **Part IV: Tokenizer**
  13. **False.** (Modern LLMs use *subword* tokenization, not whole-word, meaning one word can be split into multiple tokens). (2 pts)
  14. **C.** (It breaks the unknown word down into known subword chunks). (4 pts)
  15. **(a) Tokens:** **`["Data", "Science", "is", "fun", "!"]`** *(Must have the exclamation point as a separate token!)* (4 pts)
    **(b) Numerical IDs:** **`[14, 10, 11, 12, 13]`** (4 pts)
  16. **Answer:** 1) It vastly **reduces the vocabulary size** (from millions of words down to ~30k-50k tokens), saving massive amounts of RAM and compute. 2) It gracefully **handles rare or unseen words** (OOV words) by building them from known subword fragments instead of crashing. (6 pts)
  
  **Part V: Transformer**
  17. **False.** (Transformers process input sequences in **parallel**, all at once. RNNs process them sequentially). (2 pts)
  18. **B.** (To scale the numbers down and allow for numerical stability so the Softmax function works properly). (4 pts)
  19. **Answer:**
    *   **Query (Q):** What the current token is *looking for* (e.g., a search prompt).
    *   **Key (K):** The *labels/metadata* of the other tokens (e.g., the video titles in a database).
    *   **Value (V):** The actual *meaning/content* of the token that is retrieved if the Query and Key match. (8 pts)
  20. **Answer:** Transformers solve this by using **Positional Encodings**. They mathematically add a unique "timestamp" or "index" vector to the word's embedding vector before it enters the network, allowing the model to know exactly where each word sits in the sequence. (6 pts)
- ---
- ---
- ---
- # **CS 3402: Intro to Data Science - Quiz 2 (Practice Version D)**
  **Time Limit: 60 Minutes | Total Points: 100**
- ### **Part I: Network Analysis (Medium) [20 Points]**
  
  **1. (True/False) [2 pts]**
  In an *undirected* graph, the relationship between nodes is asymmetric, meaning traffic or influence only flows in one direction.
  
  **2. (Multiple Choice) [4 pts]**
  A graph where *every single pair of nodes* has a direct path connecting them is formally known as a:
  A) Small-World Network
  B) Directed Graph
  C) Connected Graph
  D) Subgraph
  
  **3. (Calculation) [8 pts]**
  You are analyzing a **Directed Graph** representing a small social media network (where an edge means "Node X follows Node Y"). 
  *   **User A** follows User B and User C.
  *   **User B** follows User C.
  *   **User C** follows User A.
  
  **(a)** What is the **Out-Degree** of User A? 
  **(b)** What is the **In-Degree** of User C? 
  **(c)** If we ignore the arrows and treat this as an *Undirected* graph, what is the **Degree Centrality** of User B? *(Show your math/fraction).*
  
  **4. (Short Answer) [6 pts]**
  Briefly explain the difference between a **weighted** and an **unweighted** network, and provide one real-world example of a weighted network.
  
  ---
- ### **Part II: Basics of NLP (Easy-Medium) [20 Points]**
  
  **5. (True/False) [2 pts]**
  In the NLP pipeline, tokenization must happen *before* the text is converted into numerical vectors.
  
  **6. (Multiple Choice) [4 pts]**
  You are using the **TF-IDF** (Term Frequency - Inverse Document Frequency) method to represent a dataset of 5,000 medical journals. The word `"the"` appears 50,000 times in the dataset, appearing in every single journal. What will happen to the final TF-IDF mathematical score for the word `"the"`?
  A) It will have the highest score in the vector because it appears the most frequently.
  B) It will be severely penalized (close to zero) because the IDF component down-weights words that appear in many documents.
  C) It will crash the program because "the" is a stop word.
  D) It will automatically be converted into a subword token.
  
  **7. (Calculation: Bag of Words) [8 pts]**
  You have the following master vocabulary list (in this exact order):
  `["data", "science", "is", "hard", "fun"]`
  
  You are given two sentences:
  *   **S1:** `"Data science is fun"`
  *   **S2:** `"Science is hard"`
  
  **(a)** Represent sentence **S1** as a mathematical vector.
  **(b)** Represent sentence **S2** as a mathematical vector.
  
  **8. (Short Answer) [6 pts]**
  A traditional Bag-of-Words vector is often described as a **"Sparse"** matrix, while modern LLM embeddings are described as **"Dense."** In plain English, what is the difference between a sparse vector and a dense vector?
  
  ---
- ### **Part III: Intro to DNNs (Medium) [20 Points]**
  
  **9. (True/False)[2 pts]**
  Without non-linear activation functions (like ReLU or Sigmoid), a neural network with 10 hidden layers would mathematically collapse into a single linear regression model.
  
  **10. (Multiple Choice) [4 pts]**
  What is the mathematical formula for the raw linear output ($z$) of a single artificial neuron *before* the activation function is applied?
  A) $z = (w \times x) + b$
  B) $z = \max(0, x)$
  C) $z = \frac{\text{Degree}}{n-1}$
  D) $z = \text{softmax}(x)$
  
  **11. (Calculation: Network Edge Complexity) [8 pts]**
  According to Lecture 12, an Artificial Neural Network can be modeled as a graph where the edges represent the model's weights (parameters). 
  You build a Multi-Layer Perceptron (MLP) with:
  *   **Input Layer:** 4 neurons
  *   **Hidden Layer:** 3 neurons
  *   **Output Layer:** 2 neurons
  
  Assuming it is a "fully connected" network:
  **(a)** How many weighted edges connect the Input layer to the Hidden layer?
  **(b)** How many weighted edges connect the Hidden layer to the Output layer?
  **(c)** What is the **total number of parameters (weights)** in this network? 
  
  **12. (Short Answer) [6 pts]**
  During the training of a neural network, what is the specific role of the **Loss Function**?
  
  ---
- ### **Part IV: Tokenizer (Medium) [20 Points]**
  
  **13. (True/False) [2 pts]**
  If a traditional word-level tokenizer encounters a completely new, misspelled word it has never seen before, it will easily process it without errors.
  
  **14. (Multiple Choice) [4 pts]**
  Why do modern LLMs (like ChatGPT and Gemini) use **Subword Tokenization** instead of splitting text by whole words?
  A) It eliminates the need to use numbers to represent text.
  B) It dramatically reduces the vocabulary size (e.g., to ~50k tokens) while allowing the model to piece together unknown words from known syllables.
  C) It allows the model to process images and audio simultaneously.
  D) It preserves the exact grammatical order of the original sentence.
  
  **15. (Calculation: Tokens to IDs) [8 pts]**
  You have a subword tokenizer that splits hyphenated words into separate tokens. 
  Sentence: `"The AI-model is smart."`
  
  Dictionary: 
  `The: 1, AI: 2, -: 3, model: 4, is: 5, smart: 6, .: 7`
  
  **(a)** List the exact tokens (strings) this sentence will be split into.
  **(b)** List the final array of numerical IDs.
  
  **16. (Short Answer) [6 pts]**
  According to Lecture 13, modern LLMs operate on **Token IDs**, not raw text. Briefly explain the very next pipeline step: How are these isolated Token IDs turned into **Embedding Vectors**? *(Hint: Are they given random numbers, or trainable numbers?)*
  
  ---
- ### **Part V: Transformer (Medium-Hard)[20 Points]**
  
  **17. (True/False) [2 pts]**
  In the Self-Attention mechanism, the Query (Q), Key (K), and Value (V) vectors all come from the exact same input sequence.
  
  **18. (Multiple Choice) [4 pts]**
  In the Transformer architecture, what is the purpose of the **Add & Norm** (Residual/Skip Connection) block that surrounds the Attention mechanism?
  A) To translate the input language into the output language.
  B) To provide a "shortcut" for gradients during backpropagation, helping to prevent the vanishing gradient problem in deep networks.
  C) To split the text into smaller subwords.
  D) To calculate the final Loss of the model.
  
  **19. (Short Answer) [8 pts]**
  Why do modern Transformers use **Multi-Head Attention** instead of just a single Self-Attention mechanism? What is the model able to do with multiple heads that it couldn't do with one?
  
  **20. (Short Answer) [6 pts]**
  The computational time complexity of the Self-Attention mechanism is **$O(T^2 \times d_{model})$**, where $T$ is the number of tokens in your sentence. 
  In plain English, what does this squared ($T^2$) term mean for your computer's hardware if you decide to **double** the length of the document you feed into the model?
  
  ***
  ***
  ***
  *(Stop here! Complete the exam before scrolling down to the Answer Key)*
  ***
  ***
  ***
  
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & GRADING RUBRIC**
  
  **Part I: Network Analysis**
  1.  **False.** (Undirected graphs are symmetric/two-way. Directed graphs are asymmetric). (2 pts)
  2.  **C.** (A connected graph means every pair of nodes has *a path* to each other. Note: A *Complete* graph means every pair has a *direct* edge). (4 pts)
  3.  **(a) In-Degree of C:** **2** (Arrows point to C from A and B). (2 pts)
    **(b) Out-Degree of A:** **2** (Arrows point away from A to B and C). (3 pts)
    **(c) Centrality of B:** If undirected, B connects to A and C. Degree = 2. Total nodes $n=3$. Formula: $2 / (3-1) = 2/2 = \mathbf{1.0}$. (3 pts)
  4.  **Answer:** An unweighted network treats all connections as equal (binary: connected or not). A weighted network assigns a numerical value/strength to the connection. **Example:** A flight map where edges are weighted by the cost of the ticket or flight time in hours. (6 pts)
  
  **Part II: Basics of NLP**
  5.  **False.** (TF-IDF, like Bag-of-Words, loses the original sequence/order of the words). (2 pts)
  6.  **B.** (The IDF penalizes words that appear in too many documents, dropping their score so the model ignores them as "noise"). (4 pts)
  7.  **(a) S1 Vector:** **`[1, 1, 1, 0, 1]`** (3 pts)
    **(b) S2 Vector:** **`[0, 1, 1, 1, 0]`** (5 pts)
  8.  **Answer:** A **Sparse** vector (Bag-of-Words) is filled almost entirely with zeros, taking up massive amounts of memory with no semantic meaning. A **Dense** vector (Embeddings) contains highly specific floating-point numbers in every slot, where the numbers mathematically represent the actual *meaning* and context of the word. (6 pts)
  
  **Part III: Intro to DNNs**
  9.  **True.** (Activation functions provide the required non-linearity to learn complex patterns). (2 pts)
  10. **A.** ($z = wx + b$. B is the ReLU activation function. C is degree centrality). (4 pts)
  11. *(Math from Lecture 12, Slide 32: $n_l \times n_{l+1}$)*
    **(a)** $4 \times 3 = \mathbf{12 \text{ edges}}$. (2 pts)
    **(b)** $3 \times 2 = \mathbf{6 \text{ edges}}$. (2 pts)
    **(c) Total Parameters:** $12 + 6 = \mathbf{18 \text{ weights}}$. (4 pts)
  12. **Answer:** The Loss Function mathematically measures the error (the difference) between the neural network's prediction and the actual correct target. It tells the network how "wrong" it is so it can update its weights via Gradient Descent. (6 pts)
  
  **Part IV: Tokenizer**
  13. **False.** (Word-level tokenizers crash on new words [OOV errors]. This is why we use Subwords). (2 pts)
  14. **B.** (Reduces vocab size and handles rare/unseen words by piecing them together). (4 pts)
  15. **(a) Tokens:** **`["The", "AI", "-", "model", "is", "smart", "."]`** *(Must split the hyphen and the period!)* (4 pts)
    **(b) IDs:** **`[1, 2, 3, 4, 5, 6, 7]`** (4 pts)
  16. **Answer:** The isolated Token IDs are mapped to a matrix of **Dense Embedding Vectors**. These vectors are initially filled with random numbers, but they are **trainable** parameters. During training, the model mathematically adjusts these decimals until they represent the true semantic meaning of the token. (6 pts)
  
  **Part V: Transformer**
  17. **True.** (This is the definition of *Self*-Attention). (2 pts)
  18. **B.** (Residual/Skip connections allow gradients to flow backwards smoothly, preventing the vanishing gradient problem). (4 pts)
  19. **Answer:** A single attention head might focus all its math on one task (like figuring out which noun a pronoun belongs to). **Multi-Head Attention** splits the math up, allowing the model to assign different "heads" to learn different grammatical rules (e.g., tense, emotion, structure, semantics) all at the exact same time. (8 pts)
  20. **Answer:** Because the time complexity scales **quadratically** ($T^2$), if you double the length of the document (e.g., from 1,000 words to 2,000 words), the computational cost doesn't double—it **quadruples** (takes 4 times as much math/processing power). (6 pts)
  
  ***
-