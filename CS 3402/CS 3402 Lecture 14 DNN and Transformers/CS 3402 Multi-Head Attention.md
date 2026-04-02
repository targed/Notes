### **1. The Problem with "Single" Attention (Slides 54 & 60)**
Imagine you are reading the sentence from the slides: *"The animal didn't cross the street because it was too wide."*

*   If the model only has **one** attention mechanism, it might use all of its mathematical focus to figure out that `"it"` refers to `"street"` (Coreference Resolution). 
*   **The Flaw:** What about the grammar? What about the tense? What about the emotion? If the network spends 100% of its math figuring out pronouns, it completely ignores that `"didn't"` modifies `"cross"`, or that `"too"` modifies `"wide"`.
- ### **2. The Solution: Multi-Head Attention (Slides 56 & 60)**
  Instead of having one giant attention mechanism trying to do everything, the Transformer splits the job up among multiple "Heads" (like a team of specialists).
  
  Look at the checklist on **Slide 60**. By using Multi-Head Attention, the model can assign different heads to learn different rules of human language simultaneously:
  *   **Head 1:** Focuses on *Coreference resolution* (Pronouns $\rightarrow$ Nouns).
  *   **Head 2:** Focuses on *Sentence boundaries* (Where do clauses start and stop?).
  *   **Head 3:** Focuses on *Part of speech* (Is this word a verb or a noun?).
  *   **Head 4:** Focuses on *Semantic relationships* (Positive vs. Negative sentiment).
- ### **3. The Math: How do we split the Heads? (Slide 56)**
  If we just copy/pasted the entire math process 8 times, the model would be 8 times slower and require 8 times more RAM. The creators of the Transformer were smarter than that.
  
  *   **The Splitting Formula:** $$d_h = \frac{d_{model}}{h}$$
  *   **Translation:** Let's say our word embedding ($d_{model}$) has **512** numbers in it. If we want **8** Attention Heads ($h = 8$), we don't multiply 512 by 8. Instead, we chop the 512-number vector into 8 smaller chunks of **64** numbers each ($512 / 8 = 64$).
  *   **The Matrices ($W_{Q1}, W_{Q2} ... W_{QH}$):** Instead of one giant Query, Key, and Value matrix, we create 8 miniature ones. 
    *   Head 1 gets $W_{Q1}$, Head 2 gets $W_{Q2}$, etc.
- ### **4. The Parallel Process & Concatenation (Slides 57–59)**
  Now that we have split the math into smaller chunks, what happens next?
  
  1.  **Independent Processing (Slides 57-58):** Every single head runs the exact same $Softmax(\frac{QK^T}{\sqrt{d_k}})V$ formula we learned in Part 2. But because they all started with different random weights ($W_{Q1}$ vs $W_{Q2}$), they all "pay attention" to completely different things in the sentence.
  2.  **The Output ($Z_1, Z_2 ... Z_h$) (Slide 59):** Each head spits out its own mini context vector ($Z$).
  3.  **CONCAT (Slide 59):** We can't pass 8 separate vectors to the next layer of the neural network. So, we **Concatenate** (glue them back together side-by-side). 
    *   *Result:* Because we chopped them up at the beginning, when we glue them back together, the final matrix ($Z$) is the exact same size as the original input! The computational cost remains perfectly stable, but the model is now 8 times smarter.
- ### **5. The Big $O$ Bottleneck (Slide 53)**
  The professor snuck a very important mathematical notation onto Slide 53: **$O(T^2 \times d_{model})$**.
  *   This is Big $O$ notation for Time/Space Complexity.
  *   $T$ is the number of Tokens (words) in your sentence.
  *   $T^2$ means the math scales **quadratically**.
  *   *Why this is a massive deal in 2026:* If you double the length of the document you feed into ChatGPT, the math doesn't double—it **quadruples**. If you feed it a 100,000-word book, $100,000^2 = 10,000,000,000$ (10 Billion) operations *per layer*. This quadratic bottleneck is the main reason companies struggle to build AI that can read entire textbooks at once!
  
  ---