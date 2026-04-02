### **1. The Goal of Attention (Slides 44–48)**
To understand the math, we first need to understand the problem it solves: **Context**.

*   **The Problem:** Look at the sentence on Slide 48: *"The animal didn't cross the street because **it** was too wide."*
*   If you ask a traditional computer what "**it**" refers to, it gets confused. Is the *animal* too wide? Or is the *street* too wide? 
*   **The Solution:** Humans use context. We know streets are wide. **Self-Attention** is the mathematical process of letting the word "**it**" look at every single other word in the sentence and ask, *"How strongly am I related to you?"* 
*   As shown by the curved arrows on Slide 48, the model learns to draw a strong mathematical connection between "**it**" and "**street**". (This specific NLP task is called **Coreference Resolution**).
- ### **2. The Database Analogy: Q, K, and V (Slides 27–30)**
  To make words look at each other, the Transformer uses a retrieval system exactly like a Database or a YouTube search. It uses three vectors: **Queries (Q), Keys (K), and Values (V)**.
  
  *   **The Query (Q):** What a word is *looking for*. (e.g., The word "it" asks: "I am a pronoun looking for a noun").
  *   **The Key (K):** What a word *has*. (e.g., The word "street" says: "I am a noun, and I am wide").
  *   **The Value (V):** The *actual meaning* of the word. (e.g., The underlying embedding vector of the word "street").
- ### **3. The Math: Step-by-Step (Slides 33–39)**
  How do we calculate this? The famous equation is on Slide 27:
  $$Attention(Q, K, V) = softmax(\frac{QK^T}{\sqrt{d_k}})V$$
  
  Let's break down exactly what happens in the neural network to get this:
  
  *   **Step 1: Create Q, K, and V (Slide 33)**
    *   We take our Input embedding ($I$) and multiply it by three separate, trainable weight matrices ($W_Q, W_K, W_V$). This gives every single word its own unique Q, K, and V vectors.
  *   **Step 2: The Compatibility Score ($Q \times K^T$) (Slide 34)**
    *   To find out how much the word "it" cares about the word "street", we take the **Dot Product** of "it's" Query and "street's" Key. 
    *   *Math translation:* If the Query and Key are very similar, the dot product results in a huge number. If they are unrelated, the number is close to zero.
  *   **Step 3: Scale and Softmax ($\alpha$) (Slides 35–38)**
    *   **Scale:** We divide the score by $\sqrt{d_k}$ (the square root of the vector's size). Why? If the vectors are huge, the dot product becomes a massive number, which breaks the gradients. Dividing keeps the numbers stable.
    *   **Softmax:** We pass the scores through a Softmax function. This turns the raw scores into **Probabilities (or Percentages)** that all add up to 1.0. These are called the **Attention Weights ($\alpha$)**. 
    *   *Example:* The word "it" might give 80% of its attention ($\alpha_{1,4}$) to "street", 15% to "animal", and 5% to the rest of the words.
  *   **Step 4: Get the Context ($Z$) (Slide 39)**
    *   Finally, we multiply those percentages ($\alpha$) by the **Values ($V$)** of the other words and sum them all up. 
    *   *The Result:* The new vector for the word "it" ($Z_1$) is no longer just the dictionary definition of "it". It is now a **Contextually Rich Embedding** made up of 80% "street" and 15% "animal".
- ### **4. "Self" Attention vs. Regular Attention (Slide 49)**
  *   Why is it called **Self**-Attention? 
  *   Because the Queries, Keys, and Values all come from the **exact same sentence**. The sentence is literally paying attention to *itself*. Every word looks at every other word (including itself) simultaneously.
- ### **5. Solving the Poll (Slide 42)**
  Your professor put a quiz question right in the middle of the lecture. Here are the correct answers for your notes:
  *   **Question 1:** To calculate attention weights for input $I_2$, what do you use?
    *   *Answer:* **b. You would use query $q_2$, and ALL keys.** (To find out what word 2 cares about, you take word 2's Query and test it against the Keys of every other word in the sentence).
  *   **Question 2:** Why do we scale the $QK^T$ product?
    *   *Answer:* **d. To allow for numerical stability.** (Dividing by $\sqrt{d_k}$ stops the numbers from exploding and ruining the Softmax function).
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Parallelization (Slide 40):** The slide stamps the word **Parallelized** in giant red letters. Because we are just doing matrix multiplication (Dot Products), we don't have to calculate word 1, then word 2, then word 3. A GPU can calculate the attention scores for a 10,000-word document in a fraction of a second simultaneously. This is the true power of the Transformer.
  
  ---