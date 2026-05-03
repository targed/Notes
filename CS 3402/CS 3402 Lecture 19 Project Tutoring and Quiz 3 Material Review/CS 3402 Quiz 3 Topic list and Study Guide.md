### **1. NLP & Tokenization Concepts**
*Focus: How we prepare text for modern AI.*

*   **Tokenization:**
  *   *Word-level:* Splits by spaces. **Flaw:** Crashes on new or misspelled words (Out-of-Vocabulary / OOV errors).
  *   *Subword-level (BPE/WordPiece):* Splits words into syllables/chunks. **Advantage:** Dramatically reduces vocabulary size (saves RAM) and handles unseen words by building them from known chunks. Preferred for modern LLMs.
*   **Text Representation:**
  *   *Bag-of-Words (BoW):* Counts word frequency. **Flaw:** Loses word order (creates sparse matrices).
  *   *TF-IDF:* Down-weights common words (like "the") and up-weights rare, important words.
  *   *Embeddings:* Modern approach. Converts tokens into **dense vectors** (real numbers) that capture semantic meaning.
- ### **2. The Transformer Architecture (NLP)**
  *Focus: Why it beat older models and how it works under the hood.*
  
  *   **Parallelization:**
    *   Transformers process all tokens in a sequence **in parallel** (all at once), unlike older RNNs that process sequentially. This makes them incredibly fast on GPUs but requires a lot of memory.
  *   **Positional Encoding:**
    *   *Why it's needed:* Because processing is parallel, the model has no idea what order the words are in. Positional encodings mathematically add a "timestamp" to the embedding so the model knows the word order.
  *   **Self-Attention Mechanism:**
    *   *Purpose:* Allows every token to look at (attend to) all other tokens in the sequence to gather **context** (e.g., figuring out what a pronoun refers to).
    *   *Q, K, V Vectors:* All derived from the *same* input.
        *   **Query (Q):** What the token is looking for.
        *   **Key (K):** The label/metadata of other tokens.
        *   **Value (V):** The actual content/meaning pulled to compute the output.
    *   *The Formula:* $Attention(Q, K, V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V$
    *   *Why divide by $\sqrt{d_k}$?* To scale the numbers down for **numerical stability** so the Softmax function doesn't break due to exploding values.
  *   **Multi-Head Attention:**
    *   Instead of one attention mechanism, we use multiple "heads" to learn different relationships simultaneously (e.g., one head learns grammar, another learns emotion, another learns pronoun resolution).
  *   **Add & Norm:**
    *   *Add:* Residual (skip) connections that bypass the math blocks. They prevent the **vanishing gradient problem**.
    *   *Norm:* Layer normalization stabilizes training.
- ### **3. Vision Transformers (ViT)**
  *Focus: Forcing images into a text-based architecture.*
  
  *   **How it processes images:**
    *   It **does NOT use convolutions**. 
    *   It splits the image into a grid of fixed-size squares called **Patches** (usually 16x16).
    *   It **flattens** those 3D patches into 1D vectors so the Transformer can read them exactly like word embeddings.
  *   **The `[CLS]` Token:** A special, extra fake "word" added to the beginning of the sequence. By the end of the network, this token absorbs the global context of the entire image and is used to make the final prediction.
- ### **4. Convolutional Neural Networks (CNNs)**
  *Focus: How traditional computer vision works.*
  
  *   **Why not use MLPs (Fully Connected Networks) for images?**
    *   They require way too many parameters (millions of weights for one image), which leads to massive **overfitting** and destroys the 2D spatial layout of the image.
  *   **Convolutional Layers:**
    *   Use small grids called **Kernels (or Filters)** that slide across the image.
    *   They use **Shared Weights** (the same filter looks for the same pattern everywhere in the image).
    *   *What they learn:* Local spatial patterns. Layer 1 learns edges; Layer 2 learns shapes; Layer 3 learns objects.
  *   **Pooling Layers (Max Pooling):**
    *   *Purpose:* To **reduce spatial dimensions** (shrink the width/height of the image).
    *   This reduces computational cost and helps prevent overfitting.
  *   **ResNet (Residual Networks):**
    *   *The Degradation Problem:* Stacking too many CNN layers makes the model perform *worse* due to vanishing gradients.
    *   *The Solution:* **Residual Connections** ($F(x) + x$). By giving the gradient a "shortcut" to skip layers, researchers could train networks that are hundreds of layers deep.
  
  ---
- ###  **The Math & Calculations You Must Memorize:**
  
  You will need a calculator for these. Practice the steps from Lecture 19!
  
  **1. ViT Patch Math:**
  *   *Number of Patches:* $(\frac{\text{Image Width}}{\text{Patch Width}}) \times (\frac{\text{Image Height}}{\text{Patch Height}})$.
  *   *Flattened Patch Dimension:* $\text{Patch Height} \times \text{Patch Width} \times \text{Color Channels}$. (Remember, RGB = 3 channels).
  *   *Sequence Length:* Number of Patches + 1 (for the `[CLS]` token).
  
  **2. Self-Attention Math:**
  *   You will be given $Q, K_1, K_2, V_1, V_2$.
  *   *Step 1:* Find the dot product of $Q \cdot K_1$ and $Q \cdot K_2$.
  *   *Step 2:* Put those answers into the Softmax formula (it's okay to leave $e$ in your answer).
  *   *Step 3:* Multiply your Softmax answers by $V_1$ and $V_2$ respectively, and add them together to get the final vector.
  
  **3. CNN Feature Map Math:**
  *   *Output Size Formula:* $\frac{N - F + 2P}{S} + 1$
    *   $N$ = Image Size
    *   $F$ = Filter (Kernel) Size
    *   $P$ = Padding
    *   $S$ = Stride
  *   *Number of Parameters per Filter:* $(F \times F \times \text{Channels}) + 1 \text{ (for the bias)}$. 
  
  **4. CNN Manual Convolution (Dot Product):**
  *   Be able to overlay a $2\times2$ or $3\times3$ filter onto a grid of numbers, multiply the overlapping squares, and sum them up to find the feature map output.
  
  ---
- ### **Common Professor Traps (Based on the Review Slides):**
  *   *Trap 1:* "Transformers process input sequences sequentially like RNNs." $\rightarrow$ **FALSE. They process in parallel.**
  *   *Trap 2:* "CNNs use self-attention." $\rightarrow$ **FALSE. They use shared convolution filters.**
  *   *Trap 3:* "Adding more layers to a neural network always improves its performance." $\rightarrow$ **FALSE. It causes the degradation problem (which ResNet fixes).**
  *   *Trap 4:* "Vision Transformers downsample images using pooling layers." $\rightarrow$ **FALSE. They split images into patches.**
  
  ***
-