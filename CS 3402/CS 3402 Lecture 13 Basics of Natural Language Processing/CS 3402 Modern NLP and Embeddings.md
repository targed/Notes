### **1. The Flaw of "Bag of Words" & TF-IDF (Slide 26)**
In Part 2, we learned that classical NLP creates a "Master Dictionary" of every word in existence.
*   **The Problem:** If your dictionary has 500,000 words, every single sentence you process becomes a vector of 500,000 numbers (mostly zeros). This is called a **Sparse Matrix**. It wastes massive amounts of RAM and compute power. Furthermore, if the model sees a brand-new word (a typo, slang, or a new tech term), it throws an "Out of Vocabulary" error and crashes.
- ### **2. The Modern Solution: Subword Tokenization (Slides 27–29)**
  Modern LLMs do **NOT** operate on full words. They operate on syllables or "chunks" of words.
  
  *   **How it works (Slide 27):** 
    *   The word `"Don't"` is split into `"Do"` and `"n't"`.
    *   After splitting the text into these chunks, the tokenizer looks up their unique ID number in a much smaller dictionary. (e.g., `"food"` becomes ID `2057`).
  *   **The 3 Reasons We Use Subwords (Slide 28 - Highly likely to be an exam question!):**
    1.  **Handle Rare Words:** Instead of memorizing a rare word, the model pieces it together from common chunks.
    2.  **Handle New/Unseen Words:** If you invent the word `"bioinformaticsAItool"`, a Bag of Words model crashes. A subword tokenizer just breaks it into `["bio", "informatics", "AI", "tool"]`, all of which it already knows!
    3.  **Reduce Vocabulary Size:** Instead of a master dictionary of 5,000,000 English words, modern LLMs only need a dictionary of about **30,000 to 50,000 tokens**. This makes the math infinitely faster.
- ### **3. The Magic of Dense Embeddings (Slide 30)**
  We now have Token IDs (like `3987` for `"Do"`). But if we just feed the number 3987 into a neural network, the network thinks `"Do"` is mathematically greater than `"food"` (ID 2057). That makes no sense.
  
  **The Solution: Embedding Vectors**
  *   Instead of a single ID, every single token is assigned a **Dense Vector** of real, floating-point numbers.
  *   *Example (GPT-2):* Every token is expanded into an array of **768 numbers** (e.g., `[0.0473, -0.0659, 0.1134...]`).
  *   **Why is it called "Dense"?** Because unlike Bag of Words (which is 99% zeros), every single one of these 768 slots has a highly specific decimal value.
  *   **"Trainable" (The Secret Sauce):** These 768 numbers are not random. They are **weights**. During backpropagation (Gradient Descent), the model adjusts these decimals. 
    *   *Fill-in Concept:* Eventually, the mathematical vector for `"King"` minus the vector for `"Man"` plus the vector for `"Woman"` will literally equal the vector for `"Queen"`. The model actually maps the *meaning* of words into a 768-dimensional geometric space!
- ### **4. Tying it all together: The MLP (Slide 31)**
  Now that we have successfully turned human language into dense mathematical vectors, what do we do with them? We feed them into a Neural Network!
  
  Slide 31 shows a classic **Multi-Layer Perceptron (MLP)** architecture to remind you of the ultimate goal:
  1.  **Input Layer:** You take your data (the slide shows a flattened 28x28 image, resulting in 784 inputs, but for NLP, this would be your flattened Embedding Vectors).
  2.  **Hidden Layers:** The data passes through hidden units (256 $\rightarrow$ 128). Notice the orange boxes labeled **ReLU**—these are the Activation Functions (non-linearities) we discussed in Lecture 12 that allow the network to learn complex patterns.
  3.  **Output Layer:** The final layer has 10 nodes (representing categories 0-9). The node with the highest mathematical score becomes the model's final prediction!
  
  ---
- ### **Action Items for Section 3:**
  *   **Vocabulary Check:** Make sure you can clearly articulate the difference between a **Sparse Vector** (Bag of Words, mostly zeros) and a **Dense Vector** (Embeddings, all decimals representing semantic meaning).
  *   **Conceptual Challenge:** Why couldn't ELIZA (the 1960s chatbot from Slide 4) use dense embeddings? 
    *   *Answer:* Dense embeddings require calculating gradients across billions of parameters. Computers in the 1960s physically did not have the RAM or Processing speed to do this, forcing them to use hard-coded `If/Else` rules instead.
  
  ***