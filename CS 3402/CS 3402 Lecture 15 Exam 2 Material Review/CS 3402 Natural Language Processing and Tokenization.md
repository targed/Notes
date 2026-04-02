### **1. True/False Concepts (Slides 7 & 10)**

Let's quickly rapid-fire the conceptual True/False questions. These are guaranteed easy points on the quiz!

*   **Tokenization is the process of splitting text into smaller units such as words, subwords, or characters.** $\rightarrow$ **True.**
*   **Bag-of-words representation captures the order of words in a sentence.** $\rightarrow$ **False.** (It only counts *frequency*. "The dog bit the man" and "The man bit the dog" have the exact same Bag-of-Words vector).
*   **One-hot encoding assigns a unique vector with a single 1 for each token.** $\rightarrow$ **True.** (e.g., `[0, 0, 1, 0, 0]`).
*   **NLP pipelines always include translation as a required step.** $\rightarrow$ **False.** (Translation is an *application* of NLP, not a required pipeline step. The pipeline is: Clean $\rightarrow$ Tokenize $\rightarrow$ Represent).
*   **Word-level tokenization always handles out-of-vocabulary (OOV) words effectively.** $\rightarrow$ **False.** (If it sees a brand new word, it crashes or replaces it with `<UNK>`).
*   **Subword tokenization can help handle unseen words during inference.** $\rightarrow$ **True.** (It breaks unknown words down into smaller, known chunks).
*   **In GPT-style tokenizers, each token always corresponds to a single word.** $\rightarrow$ **False.** (GPT uses Subword tokenization, so a single word like "Don't" might be 2 or 3 tokens).
*   **Tokenizers can also convert tokens into numerical IDs for models.** $\rightarrow$ **True.**
*   **Tokenization is only required for text classification tasks.** $\rightarrow$ **False.** (It is required for *every* NLP task, including translation, chatbots, and sentiment analysis).

---
- ### **2. Multiple Choice Deep-Dive (Slides 8 & 11)**
  
  *   **Which of the following is an example of tokenization?**
    *   *Answer:* **B. Splitting "I love NLP" into `["I", "love", "NLP"]`.** 
  *   **Which text representation method preserves word order?**
    *   *Answer:* **D. None of the above.** (Bag-of-words and basic Word Embeddings lose the sequential order of the sentence. To preserve order, you *must* use **Positional Encodings**—like in a Transformer!).
  *   **What is the main function of a tokenizer in NLP?**
    *   *Answer:* **B. Split text into tokens and map them to numerical IDs.**
  *   **Which is a key advantage of subword tokenization (like BPE or WordPiece)?**
    *   *Answer:* **A. Reduces vocabulary size and handles unseen words.** (Instead of memorizing 5 million English words, it memorizes 50,000 syllables/chunks).
  
  ---
- ### **3. Calculation Practice: Building a Bag-of-Words (Slide 16)**
  
  This is highly likely to be a "Short Answer" or calculation question on your quiz. 
  
  **The Prompt:**
  Given two sentences:
  *   S1: "I love AI"
  *   S2: "I love data science"
  
  **Task 1: Build a bag-of-words vocabulary.**
  *   *How to do it:* Write down every unique word across both sentences, ignoring duplicates.
  *   *Answer:* **`['I', 'love', 'AI', 'data', 'science']`** *(This is your 5-dimensional feature space).*
  
  **Task 2: Represent each sentence as a vector.**
  *   *How to do it:* Count how many times each word in your vocabulary appears in the sentence.
  *   *Answer for S1:* `"I"` (1), `"love"` (1), `"AI"` (1), `"data"` (0), `"science"` (0). $\rightarrow$ **`[1, 1, 1, 0, 0]`**
  *   *Answer for S2:* `"I"` (1), `"love"` (1), `"AI"` (0), `"data"` (1), `"science"` (1). $\rightarrow$ **`[1, 1, 0, 1, 1]`**
  
  **Task 3: Does this representation capture word order? Explain briefly.**
  *   *Answer:* **No.** Because Bag-of-Words only counts the *frequency* of words. If S1 was reversed to "AI love I", the vector would still be exactly `[1, 1, 1, 0, 0]`.
  
  ---
- ### **4. Calculation Practice: Tokenization & IDs (Slide 17)**
  
  **The Prompt:**
  Sentence: "AI models are powerful."
  Dictionary Provided: `AI:1, models:2, are:3, powerful:4, .:5`
  
  **Task 1: Tokenize the sentence.**
  *   *Answer:* **`["AI", "models", "are", "powerful", "."]`** *(Note: Don't forget the punctuation mark! Tokenizers treat punctuation as separate tokens).*
  
  **Task 2: Convert tokens into IDs.**
  *   *Answer:* **`[1, 2, 3, 4, 5]`**
  
  ---
- ### **5. Short Answer Practice (Slide 19)**
  
  **The Prompt:** What is tokenization and why is it necessary in NLP?
  *   *Answer:* Tokenization is the process of chopping a raw string of text into smaller, distinct pieces (words or subwords) and assigning them a numerical ID. It is necessary because **Machine Learning models (like Neural Networks) cannot read text; they can only perform mathematical operations on numbers.**
  
  ---