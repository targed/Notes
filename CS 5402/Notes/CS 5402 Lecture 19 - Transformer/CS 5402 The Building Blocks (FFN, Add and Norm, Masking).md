## 1. The Feed Forward Layer (FFN) (Slides 37–38)
  After the Multi-Head Attention layer figures out the context of a word (e.g., mixing 88% "animal" with 12% "street"), that new representation is passed into a **Feed Forward Network (FFN)**.
  
  *   **The Structure:** It is a very simple, standard Multi-Layer Perceptron (MLP) applied to *each word individually*.
    *   Linear Layer 1 $\to$ ReLU Activation $\to$ Dropout $\to$ Linear Layer 2.
  *   **The Purpose:** Self-Attention aggregates information from *other* words. The FFN processes that aggregated information to figure out what it actually means. Because it uses a non-linear activation function (ReLU), it allows the network to learn complex, non-linear representations.
## 2. Add & Norm (Residuals & Normalization) (Slides 39–40)
  Look at the architecture diagram (Slide 39): every Attention layer and every FFN layer is wrapped in an "Add & Norm" block.
### A. The "Add" (Residual Connection)
  *   **What it is:** The input to a layer is added directly to its output: $\text{Output} = x + \text{Layer}(x)$.
  *   **Why we need it:** Just like we learned in **ResNet** (Lecture 16), deep networks suffer from the **Vanishing Gradient Problem**. By providing a "highway" that skips the math of the layer, gradients can flow backward through 100+ layers unimpeded. 
  *   It also prevents the network from "forgetting" the original word embedding as it gets processed through dozens of layers.
### B. The "Norm" (Layer Normalization)
  *   **What it is:** It normalizes the numbers in the vector so they have a mean of 0 and a standard deviation of 1.
  *   **Why we need it:** In deep networks, multiplying numbers repeatedly can cause values to explode or shrink. Normalization keeps the scale of the values stable. This makes the model train significantly faster and helps it generalize better to new data.
  
  ---
## 3. Creating Masks (Slides 41–43)
  Masking is a crucial mathematical trick used to block the Self-Attention mechanism from looking at things it shouldn't see. It essentially forces the SoftMax probability for certain words to be exactly **0%**.
  
  There are two main types of masks:
### A. The Padding Mask (Used in Encoder & Decoder)
  *   **The Problem:** Neural networks process data in fixed-size batches (e.g., 512 tokens). If a sentence is only 5 tokens long ("I am a student END"), the rest of the space is filled with meaningless `<PAD>` or `N/A` tokens.
  *   **The Solution:** We don't want the word "student" paying attention to `<PAD>`. We apply a mask (a matrix of 1s and 0s) to zero-out the attention scores for padding tokens.
### B. The Look-Ahead Mask (Used ONLY in the Decoder)
  *   **The Problem:** During training, we feed the entire target translation (e.g., the French sentence) into the Decoder all at once for parallel efficiency. But during real-world inference, the model has to generate words one by one. If we let the Attention mechanism look at the whole French sentence during training, it will just "cheat" by looking at the next word to predict the next word!
  *   **The Solution:** We apply a triangular mask. When predicting word #3, the Self-Attention mechanism is allowed to look at word #1 and #2, but words #4, #5, etc., are masked out (probabilities set to 0). This prevents the Decoder from **"peaking ahead."**
  
  ***
# Problem Solving & Concept Check
  
  **1. The "Add" Logic (Short Answer):**
  If the input to a Multi-Head Attention layer is a vector $x = [1.2, 0.5]$, and the output of the Attention calculation is $\text{Attention}(x) = [0.1, -0.2]$, what is the final vector passed to the Normalization step in the "Add & Norm" block?
  *   *Answer:* You simply add the original input (the residual) to the output: $[1.2 + 0.1, 0.5 - 0.2] = \mathbf{[1.3, 0.3]}$.
  
  **2. Masking Mechanics (Multiple Choice):**
  You are training a Transformer to translate English to Spanish. You are currently processing the Decoder layer. Which of the following best describes how the Look-Ahead Mask modifies the Attention scores before the SoftMax function is applied?
  A) It multiplies the future word scores by 1.
  B) It sets the future word scores to $-\infty$ (Negative Infinity).
  C) It sets the future word scores to $0$.
  D) It averages the future word scores.
  *   *Answer:* **B**. If you set a raw score to $-\infty$, the SoftMax function ($e^{-\infty}$) will turn it into exactly **0 probability**. (If you just multiplied the raw score by 0, $e^0 = 1$, which would still give it some probability mass!).
  
  **3. Architectural Necessity:**
  If you removed the "Add" (Residual Connections) from a 100-layer Transformer like GPT-3, what mathematical crisis would immediately occur during Backpropagation?
  *   *Answer:* The **Vanishing Gradient Problem**. Without the residual highways, the gradient would have to multiply through 100 layers of Attention and FFN weights, shrinking to zero and stopping the early layers from learning.
  
  ***