## 1. The Paradigm Shift: Foundation Models (Slides 3–7)
  Previously, we used different architectures for different data: CNNs for images, RNNs/LSTMs for text and time-series. 
  *   **The Transformer (2017):** Originally designed for translation ("Attention Is All You Need"), it quickly proved to be a "universal" architecture. 
  *   **Foundation Models:** You train one gigantic Transformer on massive amounts of broad, unlabeled data (like the entire internet). It learns the general structure of logic, grammar, and reality. Then, you slightly fine-tune it for specific tasks.
## 2. A High-Level Look at the Architecture (Slides 8–13)
  The original Transformer is a **Sequence-to-Sequence (Seq2Seq)** model, typically used for translation (e.g., English to Chinese).
  It consists of two main halves:
  1.  **The Encoder:** Reads the entire input sentence at once and processes it into a rich, contextualized mathematical representation.
  2.  **The Decoder:** Takes that representation and generates the output sentence one word at a time.
## 3. Input/Output Embedding (Slides 14–18)
  Computers cannot read the string "dog." We must convert words into numbers. 
  
  **Why not just use IDs (dog = 3, cat = 4)?**
  *   IDs have no semantic meaning. Is "cat (4)" mathematically closer to "car (5)" than to "dog (3)"? No, but the numbers imply that.
  *   One-Hot Encoding solves the math problem but creates massive, sparse, inefficient vectors that also lack semantic relationships.
  
  **The Solution: Dense Embeddings**
  *   We map every word to a dense vector of floating-point numbers (e.g., $d_{model} = 512$ dimensions). 
  *   *The Magic:* In this embedding space, words with similar meanings physically group together! The vector for "cat" will be mathematically very close to the vector for "dog," and far away from "car."
  *   **Takeaway:** Embedding tells the network **"what the word is."**
## 4. Positional Encoding (Slides 19–22)
  Here lies the first major difference between an LSTM and a Transformer. 
  *   **The Problem:** LSTMs read words sequentially (Word 1, then Word 2, then Word 3). They inherently know the order of the sentence. Transformers process **all words at the exact same time (in parallel)**. 
  *   If you feed a Transformer "The dog chased the cat" and "The cat chased the dog," the raw attention mechanism sees the exact same "bag of words" and gets confused. It is **Permutation Invariant**.
  
  **The Solution:** 
  We must artificially inject the concept of "time" or "position" into the data.
  *   We generate a **Positional Encoding Vector** of the exact same size as the Word Embedding Vector (e.g., 512 dimensions).
  *   We use alternating **sine and cosine functions** of different frequencies to create a unique mathematical "fingerprint" for Position 1, Position 2, etc.
  *   **The Operation:** We literally **add** the Positional Encoding vector to the Word Embedding vector before feeding it to the network.
  *   **Takeaway:** Positional Encoding tells the network **"where the word is."**
  
  ***
# Problem Solving & Concept Check
  
  **1. The Flaw of Parallelization:**
  Why do Transformers require Positional Encoding, while Convolutional Neural Networks (CNNs) and Recurrent Neural Networks (RNNs) do not?
  *   *Answer:* RNNs process data strictly sequentially (one time step at a time), so the order is built into the architecture. CNNs use localized filters ($3 \times 3$ sliding windows) that preserve spatial order. Transformers process the entire sequence simultaneously in parallel. Without positional encoding, a Transformer just sees an unordered "bag of words."
  
  **2. Embedding Math:**
  Suppose your vocabulary has 10,000 words. You use an embedding dimension of $d_{model} = 512$. What is the shape of the mathematical matrix representing a sentence with 10 words?
  *   *Answer:* **$10 \times 512$**. (10 words, each represented by a 512-dimensional vector. The 10,000 vocabulary size just defines the dictionary you are looking up the vectors in).
  
  **3. Vector Addition:**
  Let the word embedding for "Robot" be $V_{robot} =[0.5, -0.2]$. Let the positional encoding for Position 3 be $P_3 = [0.1, 0.9]$. What is the final input vector fed into the Transformer for the word "Robot" at position 3?
  *   *Answer:* We simply add them element-wise: $[0.5 + 0.1, -0.2 + 0.9] = \mathbf{[0.6, 0.7]}$.
  
  ***