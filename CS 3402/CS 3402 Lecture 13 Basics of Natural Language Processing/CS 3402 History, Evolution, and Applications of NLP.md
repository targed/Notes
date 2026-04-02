### **Part 1: History, Evolution, and Applications of NLP**
**(Covering Slides 2–13)**

Before we can build ChatGPT, we have to understand how we got here. This section traces the history of teaching computers to "read."
- #### **1. The Early Eras: Rules and Disappointment (Slides 3–5)**
  *   **The Turing Test (1950):** Alan Turing proposed that instead of asking "Can machines think?", we should ask "Can a machine exhibit behavior indistinguishable from a human?" This set the ultimate goal for NLP.
  *   **ELIZA (1960s):** Developed at MIT, ELIZA was the world's first chatbot. 
    *   *How it worked:* It didn't actually "understand" anything. It used **Pattern Matching** (if the user types "I feel X", respond with "Why do you feel X?"). 
  *   **The First "AI Winter" (1960s–1970s):** Researchers were highly overly optimistic, claiming they would solve Machine Translation in 3 years using "hand-coded linguistic rules." When they failed to account for the complexity and ambiguity of human language, funding dried up, and a "Dark Era" began.
- #### **2. The Statistical Revolution (Slide 6)**
  *   **Late 1980s & 1990s:** Computers finally got faster. Instead of paying linguists to write millions of grammar rules (e.g., "i before e except after c"), engineers started using **Data-driven, Statistical Approaches**.
  *   *The Paradigm Shift:* You feed a computer a million translated documents, and it calculates the *probability* that the English word "Dog" translates to the Spanish word "Perro".
  *   *Famous Quote:* "Whenever I fire a linguist, our machine translation performance improves" (IBM, 1988). This proved that data beats hand-crafted rules.
- #### **3. The Modern Era: Deep Learning & LLMs (Slides 7–8)**
  *   **2010s (Embeddings):** The invention of neural network representations like `Word2Vec` and `BERT`. Instead of counting words, words were turned into dense mathematical vectors (which we will cover in Part 3).
  *   **2020s (Foundation Models):** The era of ChatGPT, Gemini, and LLaMA. 
    *   *Scaling Laws:* The current philosophy is simply: **Bigger models + More data + More compute = Better performance.**
  *   **2026 and Beyond (Slide 8):** You are taking this class in 2026, so the slides point toward the future: **Agentic Workflows** (AIs that don't just chat, but actively use tools, browse the web, and complete multi-step tasks) and **World Models**.
- #### **4. NLP Application: Sentiment Analysis (Slides 9–13)**
  The professor highlights **Sentiment Analysis** (also called Opinion Mining or Subjectivity Analysis) as a prime example of an NLP task.
  
  *   **The Goal:** Identifying the attitude (positive, negative, or neutral) expressed in a text.
  *   **The Setup (Slide 10):** We typically treat this as a **Binary Classification** problem (0 = Negative, 1 = Positive).
    *   *Example:* "This movie was fantastic!" $\rightarrow$ Positive (1).
  *   **Why is it useful? (Slide 12):** The graph shows Twitter (X) Sentiment mapped against the Gallup Poll of Consumer Confidence during the 2008 financial crash. 
    *   *The Insight:* Traditional polls take weeks to conduct and cost millions. NLP allows you to scrape millions of tweets in real-time and perfectly track public sentiment for free.
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Heuristics vs. ML:** The ELIZA chatbot used "Heuristics" (hard-coded `If/Else` statements). This is *not* Machine Learning. ML requires the machine to learn the rules from the data itself.
  *   **Ambiguity:** The main reason "hand-coded" rules failed in the 1970s is because human language is ambiguous. For example, the sentence "I saw a man on a hill with a telescope" has multiple completely valid interpretations. Statistical models handle ambiguity much better than rigid code.
  
  ---