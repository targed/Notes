## 1. What is Self-Attention? (Slides 24–27)
  **The Problem with Old Models (RNNs):** If an RNN reads "The animal didn't cross the street because it was too tired," it has to remember the word "animal" for 6 time steps until it reads the word "it". By then, the memory might have vanished.
  *   **The Transformer Solution:** The Transformer processes the entire sentence at once in a massive matrix. When it processes the word "it," it calculates a mathematical score against *every other word* in the sentence to see which one is most relevant.
  
  **The "Dating App" Analogy (Slide 28):**
  To perform Self-Attention, we don't just use the raw word embedding. We mathematically project the word into three different "semantic spaces" by multiplying the word vector by three learned weight matrices ($W_Q, W_K, W_V$).
  This creates three new vectors for every single word:
  1.  **Query (Q):** "What am I looking for?" (e.g., The word "it" is looking for a noun).
  2.  **Key (K):** "What information do I provide if selected?" (e.g., The word "animal" advertises itself as a noun).
  3.  **Value (V):** "What is my actual underlying meaning/content?" (e.g., The core semantic meaning of "animal").
## 2. The Math of Self-Attention (Slides 29–34)
  This is the famous "Scaled Dot-Product Attention" equation from the "Attention Is All You Need" paper:
  
  $$ \text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right) V $$
  
  **Let's break down the 4 steps (from the perspective of a single word $x_i$):**
  
  **Step 1: Calculate Q, K, V (Slide 30)**
  *   Multiply the word embedding ($x_i$) by the three learned matrices to get $q_i, k_i, v_i$.
  
  **Step 2: Calculate the Attention Score (Slide 32)**
  *   To see how much the word "it" ($q_1$) cares about the word "animal" ($k_2$), we take their **dot product**: $q_1 \cdot k_2$.
  *   *Geometry Intuition:* The dot product measures similarity (like Cosine distance). If the Query and Key align perfectly, the score is huge. If they are unrelated, the score is near zero.
  
  **Step 3: Scale and Softmax (Slide 33)**
  *   **Scale:** Divide the score by $\sqrt{d_k}$ (the square root of the dimension size of the keys). This prevents the dot products from getting so massively large that the SoftMax function completely flattens out (which would kill the gradients during backprop).
  *   **Softmax:** Run the scaled scores through the SoftMax function.
    *   *Result:* All the attention scores for a word now **sum exactly to 1.0** (like percentages).
    *   *Example:* The word "it" pays 88% attention to "animal", 12% attention to "street", and 0% to the rest.
  
  **Step 4: The Weighted Sum of Values (Slide 34)**
  *   Finally, we multiply those SoftMax percentages by the **Value ($V$)** vectors of the respective words, and add them all together.
  *   *Result:* The new, updated mathematical representation for the word "it" is now a blend of 88% "animal" and 12% "street." It has successfully captured its context!
  
  ---
## 3. Multi-Head Attention (Slides 35–36)
  **The Problem:** A single Self-Attention mechanism might focus too heavily on one specific type of relationship (e.g., it only learns to connect pronouns to nouns). But language is complex! We also need to connect subjects to verbs, and adjectives to nouns.
  
  **The Solution:** We run the entire Self-Attention process multiple times in parallel. Each parallel run is called an **Attention Head**.
  *   We initialize 8 (or 16, or 64) different sets of $W_Q, W_K, W_V$ matrices.
  *   Each "Head" learns to look for a completely different type of relationship:
    *   *Head 1:* Learns Coreference ("it" $\to$ "animal").
    *   *Head 2:* Learns Syntax ("animal" $\to$ "cross").
    *   *Head 3:* Learns Local Context ("too" $\to$ "tired").
  *   **Concatenation:** We take the outputs from all 8 heads, glue them together into one massive vector, and multiply it by a final weight matrix ($W^O$) to compress it back down to the original size.
  
  ***
# Problem Solving & Concept Check
  
  **1. The Dot Product Meaning:**
  In Step 2 of Self-Attention, we calculate $q_1 \cdot k_2 = 112$. If we calculated $q_1 \cdot k_3 = 12$, what does this mathematically tell the Transformer?
  *   *Answer:* It tells the Transformer that Word 1 ("it") is highly relevant/similar to Word 2 ("animal"), but mostly irrelevant to Word 3 ("street"). The larger dot product means the vectors point in similar directions in the semantic space.
  
  **2. The Softmax Step:**
  After calculating the dot products for Word 1 against all 5 words in a sentence, you get the scaled scores: `[10, 2, -5, 0, 1]`. You pass this array through a Softmax function.
  *   What mathematical property will the resulting 5 numbers have?
  *   *Answer:* They will all be positive numbers between 0 and 1, and **they will sum exactly to 1.0**. (e.g., `[0.98, 0.01, 0.00, 0.00, 0.01]`).
  
  **3. Multi-Head Parallelism:**
  A Transformer uses 8 Attention Heads. If the input embedding size ($d_{model}$) is 512 dimensions, what is the standard size of the $Q$, $K$, and $V$ vectors for each individual head?
  *   *Answer:* To keep computation efficient, the 512 dimensions are usually sliced evenly among the heads. $512 / 8 = \mathbf{64}$ dimensions per head.
  
  ***