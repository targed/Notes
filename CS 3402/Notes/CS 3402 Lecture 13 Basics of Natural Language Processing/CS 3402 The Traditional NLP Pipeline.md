### **1. The Universal NLP Pipeline (Slide 14)**
Every NLP project—whether it's a simple 1990s spam filter or ChatGPT—follows the same basic architectural pipeline:
1.  **Text (Raw Data):** The raw string (e.g., `"I LOVED this movie!!!"`).
2.  **Clean:** Removing noise (e.g., `"i loved this movie"`).
3.  **Tokenize:** Chopping the string into pieces (e.g., `["i", "loved", "this", "movie"]`).
4.  **Normalize/Represent:** Turning those pieces into numbers (e.g., `[0, 1, 1, 1]`).
5.  **AI Model:** Passing the numbers through a Neural Network or Logistic Regression.
6.  **Output:** The prediction (e.g., `1` for Positive).

---
- ### **2. Step 1: Data Cleaning (Slide 18)**
  Human text is incredibly messy. We use **Regular Expressions (Regex)** or string libraries to clean it.
  *   **Lowercasing:** Computers treat `"Apple"` and `"apple"` as two completely different words. Converting everything to lowercase halves the size of the vocabulary the computer has to learn.
  *   **Removing Punctuation:** Does an exclamation mark `!` change the *topic* of a sentence? Usually no. (Slide 18 shows removing punctuation from `"I love AI, it's amazing!"`).
  *   *Fill-in Info (Stop Words):* Though not explicitly named on the slide, cleaning usually involves removing "Stop Words"—common words like *the, is, at, which, on* that take up memory but add almost zero predictive value to the model.
  
  ---
- ### **3. Step 2: Tokenization (Slides 15–17)**
  **Tokenization** is the process of splitting a long string into smaller units called **Tokens**. 
  
  *   **The Old Way (NLTK):** The Natural Language Toolkit (Slide 15) splits text by spaces and punctuation. A token is usually just a whole word.
    *   `"works_ on *very"` $\rightarrow$ `['works_', 'on', '*', 'very']`
  *   **The Modern Way (LLM Tokenizers):** Slides 16 and 17 preview how OpenAI's GPT models do it.
    *   Look closely at the GPT-2 tokens on Slide 17: `['The', 'Ġpoint', 'Ġof']`.
    *   *What is the `Ġ` symbol?* Modern models don't just split by spaces; they use **Byte-Pair Encoding (BPE)**. The `Ġ` is a special character the tokenizer uses to remember that there was a "space" before the word. This allows the model to perfectly reconstruct the exact spacing of the original sentence when generating text.
  
  ---
- ### **4. Step 3: Text Representation (Slides 19–23)**
  Once we have tokens, we must convert them into numbers. The classical approach is called **Bag of Words (BoW)**.
  
  **A. Bag of Words (Slide 20)**
  *   *Concept:* You create a massive master dictionary of every word you've ever seen. Then, for a new sentence, you just count how many times each word appears.
  *   *Example:* 
    *   Master Dictionary: `[I, like, tomatoes, more, than, apples, reading]`
    *   Text 1 ("I like tomatoes more than apples"): `[1, 1, 1, 1, 1, 1, 0]`
    *   Text 2 ("I like reading"): `[1, 1, 0, 0, 0, 0, 1]`
  *   *The Problem:* It loses word order! "The dog bit the man" and "The man bit the dog" result in the exact same mathematical vector.
  
  **B. The Upgrade: TF-IDF (Slide 21)**
  If you just count words (BoW), words like "the" will have a massive score, but they aren't important. We fix this with **TF-IDF (Term Frequency - Inverse Document Frequency)**.
  *   **TF (Term Frequency):** How often the word appears in *this specific document*. (High score = important to this doc).
  *   **IDF (Inverse Document Frequency):** This penalizes words that appear in *every document* across your whole dataset. 
  *   **Result:** A word like "the" gets a high TF, but a massive IDF penalty, reducing its final score to nearly 0. A rare word like "bioinformatics" gets a high TF and no IDF penalty, making it the most important mathematical feature in the vector.
  
  **C. Solving the Slide 23 Practice Question:**
  The slide gives a Master Vocabulary of 11 words: 
  `[me, basketball, Mireia, football, likes, loves, Sergio, Hector, He, than, more]`
  
  *Question: What is the Term Frequency (TF) vector for the 3rd doc: "He likes basketball more than football"?*
  Let's count them mapped to the vocabulary list:
  *   me: 0
  *   basketball: 1
  *   Mireia: 0
  *   football: 1
  *   likes: 1
  *   loves: 0
  *   Sergio: 0
  *   Hector: 0
  *   He: 1
  *   than: 1
  *   more: 1
  **Answer:** `[0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1]`
  
  ---
- ### **5. Vector Normalization (Slide 24)**
  After creating these vectors, some documents might be 10 words long, and others might be 1,000 words long. A 1,000-word document will naturally have massive TF counts.
  
  *   **L2 Normalization:** We divide every number in the vector by the "length" (Euclidean norm) of the vector: $\sqrt{\sum x_i^2}$.
  *   *Why?* This squashes every value to be between `0` and `1`. Now, a short tweet and a massive 50-page book can be compared apples-to-apples by a machine learning model, because their mathematical vectors operate on the exact same scale.
  
  ---