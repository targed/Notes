### **1. Intro to DNNs: True/False Concepts (Slide 9)**
These questions test your understanding of how a neural network is built and trained.

*   **A neuron in a neural network applies a linear transformation followed by a non-linear activation function.** 
  *   *Answer:* **True.** (The math is $z = wx + b$ [linear], followed by $y = \text{ReLU}(z)$ [non-linear]).
*   **Adding more layers to a neural network *always* improves its performance.** 
  *   *Answer:* **False.** (This is a classic trap. Adding too many layers can cause **Overfitting** or the **Vanishing Gradient** problem, actually making the model perform *worse* on test data).
*   **The ReLU activation function outputs zero for negative inputs.** 
  *   *Answer:* **True.** (The rule is: if $x < 0$, output $0$. If $x > 0$, output $x$).
*   **Backpropagation is used to update the weights of a network using the gradient of the loss function.** 
  *   *Answer:* **True.** (This is the exact definition of how networks learn).
*   **The loss function measures how well the neural network predicts the training data.** 
  *   *Answer:* **True.** (High loss = bad prediction. Low loss = good prediction).

---
- ### **2. Calculation Practice: The Toy MLP (Slide 18)**
  This is a guaranteed calculation question for the quiz. It tests if you know how a single artificial neuron processes data.
  
  **The Prompt:**
  Given a toy MLP equation: $y = \text{ReLU}(w_1x_1 + w_2x_2 + b)$
  *   **Inputs:** $x_1 = 1, \; x_2 = 2$
  *   **Weights:** $w_1 = 0.5, \; w_2 = -1$
  *   **Bias:** $b = 1$
  
  **Task 1: Compute the linear output.**
  *   *Formula:* $z = (w_1 \times x_1) + (w_2 \times x_2) + b$
  *   *Math:* $z = (0.5 \times 1) + (-1 \times 2) + 1$
  *   *Math:* $z = 0.5 - 2 + 1$
  *   *Answer:* **$-0.5$**
  
  **Task 2: Apply ReLU.**
  *   *Rule:* $\text{ReLU}(x) = \max(0, x)$. (If the number is negative, turn it into 0).
  *   *Math:* $\max(0, -0.5)$
  *   *Answer:* **$0$**
  
  **Task 3: What is the final output?**
  *   *Answer:* **$0$**
  
  ---
- ### **3. Transformers: Concepts & Multiple Choice (Slides 12–14)**
  Transformers (the "T" in ChatGPT) revolutionized AI. These questions test *why* they are so powerful.
  
  *   **Transformers process input sequences in parallel rather than sequentially like RNNs.** $\rightarrow$ **True.** (This is why they are so fast to train on GPUs).
  *   **In self-attention, the Query, Key, and Value vectors are all derived from the same input.** $\rightarrow$ **True.** (That is exactly why it is called *Self*-Attention!).
  *   **Positional encoding is used to give the model information about token order.** $\rightarrow$ **True.** (Because Transformers read everything in parallel, they have no concept of left-to-right without these position tags).
  *   **Transformers require a fixed sequence length for training and inference.** $\rightarrow$ **False.** (They can handle variable-length sentences).
  
  **Multiple Choice:**
  *   **What is the primary purpose of self-attention in a transformer?**
    *   *Answer:* **B. To allow each token to attend to all other tokens in the sequence.** (e.g., Letting the word "it" figure out if it refers to "animal" or "street").
  *   **In self-attention, which statement is correct about Q, K, and V?**
    *   *Answer:* **A. Q determines the token to attend, K stores information about other tokens, V is used to compute the output.**
  *   **Why is positional encoding needed in transformers?**
    *   *Answer:* **B. To give the model information about the order of tokens.**
  
  ---
- ### **4. Short Answer Practice (Slide 19)**
  Be prepared to write 1-2 sentences for these on the exam!
  
  **Prompt 1: What is the role of an activation function in a neural network?**
  *   *Answer:* An activation function introduces **non-linearity** into the network. Without it, a neural network with 100 layers would mathematically collapse into a single, simple linear regression model, making it impossible to learn complex patterns in data.
  
  **Prompt 2: Explain the roles of Query (Q), Key (K), and Value (V) in attention.**
  *   *Answer:* Think of a database search. 
    *   The **Query (Q)** is what a word is *looking for* (e.g., "I am a pronoun looking for a noun"). 
    *   The **Key (K)** is the *label/description* of the other words (e.g., "I am a noun"). 
    *   The **Value (V)** is the *actual meaning/content* of the word. 
    *   The network multiplies Q and K to find a match, and if they match, it pulls the information from V to give the original word context!
  
  ***