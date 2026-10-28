## 1. The Encoder vs. The Decoder (Slides 44–46)
  Up to this point, we’ve looked at the layers inside the Transformer. Now, let's look at the two main "Blocks" that contain those layers.
  
  *   **The Encoder (Slide 44):** 
    *   *Goal:* Read the entire input sequence (e.g., the English sentence) and create a "heavily processed," highly contextualized mathematical representation of it.
    *   *Mechanics:* You can stack $N$ Encoder blocks on top of each other (e.g., $N=6$ for the original paper, or $N=96$ for GPT-3). 
  *   **The Decoder (Slide 45–46):**
    *   *Goal:* Generate the output sequence (e.g., the French translation) **one token at a time** (Autoregressively).
    *   *Mechanics:* The Decoder looks at what it has generated so far (e.g., "Le détective"), and looks at the Encoder's output to decide what the next word should be.
## 2. The Bridge: "Cross-Attention" (Slide 45)
  How does the Decoder know what the Encoder read? Through a specialized layer in the Decoder called **Cross-Attention**.
  
  *   *The Setup:* In standard Self-Attention, $Q$, $K$, and $V$ all come from the exact same sentence. 
  *   *In Cross-Attention:* 
    *   **Query ($Q$):** Comes from the **Decoder** (What the translated sentence needs right now).
    *   **Key ($K$):** Comes from the **Encoder** (The English words advertising their meaning).
    *   **Value ($V$):** Comes from the **Encoder** (The actual context of the English words).
  *   *Analogy:* The Decoder says, "I just translated the word 'The', I need a noun next" ($Q$). The Encoder's output says, "I have a noun over here, it's the word 'animal'" ($K, V$).
## 3. Text Generation Strategies (Slides 47–48)
  Because the Decoder generates one word at a time, it faces a branching path of possibilities. How does it choose the best sentence?
  
  *   **Greedy Strategy (Slide 47):** At every single step, just pick the word with the highest SoftMax probability. 
    *   *Flaw:* A word that looks like the best choice right now might force the sentence into a grammatical dead-end later.
  *   **Beam Search (Slide 48):** Instead of picking just the top 1 word, keep the **Top $K$** most likely paths alive in memory (e.g., a "Beam Width" of 3). At the next step, expand all 3 paths, calculate the total probabilities of the whole sequences, and keep the best 3. 
    *   *Benefit:* It finds a globally better sentence by exploring multiple timelines simultaneously.
## 4. Why the Transformer is so Powerful (Slides 49–51)
  Why did this architecture crush LSTMs and take over AI?
  1.  **Global Context:** Every word attends to *every other word* directly. There is no "vanishing gradient" across long sentences because the mathematical distance between word #1 and word #100 is exactly 1 step (a dot product).
  2.  **Massive Parallelization:** LSTMs process word 2 only after word 1. Transformers process a 2,000-word document simultaneously using massive matrix multiplication on GPUs. This makes training exponentially faster and infinitely scalable.
  3.  **Multimodal (Slide 51):** Because Transformers just process embeddings (vectors), they don't care if the vector is a word, a patch of an image (Vision Transformers), or an audio waveform. "Anything you can tokenize, you can feed to a Transformer."
  
  ***
# Problem Solving & Concept Check (After-Class Questions)
  
  Here are the detailed answers to the rigorous exam-style questions from the beginning and end of the slide deck.
### **In-Class True/False Quiz (Slide 2 - SSL Review)**
  *   **T / F: SSL creates supervisory signals directly from the data itself.** $\rightarrow$ **TRUE.** (It creates pseudo-labels, like hiding a word and predicting it).
  *   **T / F: SSL representations are typically less useful for downstream tasks than supervised learning.** $\rightarrow$ **FALSE.** (They are often *more* robust and generalize better).
  *   **T / F: The performance on a pretext task always directly reflects downstream task performance.** $\rightarrow$ **FALSE.** (We don't care how good the model is at solving jigsaw puzzles; we only care how rich the learned feature map is).
  *   **T / F: Contrastive learning requires both positive and negative sample pairs.** $\rightarrow$ **FALSE.** (BYOL proved you can do it with only positive pairs using a Teacher/Student momentum network).
  *   **T / F: BYOL requires negative samples to avoid collapse.** $\rightarrow$ **FALSE.** (BYOL specifically avoids collapse *without* negative samples).
### **After-Class Practice Questions (Slide 52)**
  
  **Q1. Explain why self-attention is permutation-invariant. (a) why does this create a problem? (b) how does positional encoding resolve it?**
  *   **Answer:** Self-attention just calculates dot products between sets of vectors. It treats sentences like a "bag of words"—it doesn't inherently know which word came first. (a) This is a problem because grammar and order define meaning ("Dog bites man" vs "Man bites dog"). (b) Positional Encoding resolves this by injecting a unique mathematical wave (sine/cosine) into each word's embedding, giving it a permanent spatial "timestamp."
  
  **Q2. Compare self-attention vs RNN/LSTM in terms of: dependency modeling, computational complexity and parallelization.**
  *   **Answer:** 
    *   *Dependency Modeling:* RNNs struggle with long-range dependencies (vanishing gradient) because the signal must pass through many sequential steps. Self-attention links all words directly (1 step), perfectly capturing long-range context.
    *   *Computational Complexity:* RNNs are $O(N)$ per layer (where $N$ is sequence length). Self-attention is $O(N^2)$ because every word must calculate a dot product with every other word. (Transformers require massive memory for very long documents!).
    *   *Parallelization:* RNNs are strictly sequential ($O(N)$ sequential operations). Self-attention matrix multiplications can be executed all at once ($O(1)$ sequential operations), making them incredibly fast on GPUs.
  
  **Q3. In the Transformer encoder, why are residual connections + layer normalization necessary? (a) what problems do they solve? (b) what would happen if we remove them?**
  *   **Answer:** (a) Residual connections (adding the input to the output) provide a "highway" for gradients to flow backward, solving the Vanishing Gradient problem. Layer Norm stabilizes the massive matrix multiplications, ensuring the numbers don't explode. (b) If removed, a deep Transformer (e.g., 100 layers) would suffer catastrophic vanishing/exploding gradients and would completely fail to train.
  
  **Q4. Why does the Transformer decoder use masking? (a) what would happen if masking is removed during training? (b) how does this relate to inference?**
  *   **Answer:** The decoder uses a "Look-Ahead Mask" to hide future words. (a) If removed during training, the Self-Attention mechanism would simply "cheat" by looking at the target word it is supposed to predict next, resulting in a model that learns nothing. (b) During real-world inference, the mask isn't strictly necessary because the future words literally haven't been generated yet—but training with the mask ensures the model learns to predict strictly from *past* context.
  
  **Q5. Implement the self-attention module.**
  *   *Conceptual Code/Math:*
    1.  Generate $Q = X W_Q$, $K = X W_K$, $V = X W_V$.
    2.  Calculate Scores = $Q \cdot K^T$.
    3.  Scale = $\text{Scores} / \sqrt{d_k}$.
    4.  Apply Softmax(Scale).
    5.  Output = $\text{Softmax(Scale)} \cdot V$.
  
  ***