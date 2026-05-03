### **Practice Quiz: NLP, Tokenization & Transformers**

**Question 1 (True/False)**
Word-level tokenization is considered the preferred method for modern Large Language Models (LLMs) because it balances vocabulary size and expressiveness while handling rare words perfectly.

**Question 2 (Multiple Choice)**
Which of the following is **NOT** a primary benefit of using Subword Tokenization over Word-Level Tokenization?
A) It reduces the overall vocabulary size the model has to memorize.
B) It allows the model to process text sequentially rather than in parallel.
C) It handles rare or out-of-vocabulary (OOV) words better by breaking them down into known chunks.
D) It allows for the reuse of common subword units across different contexts.

**Question 3 (Short Answer)**
In the Transformer architecture, why is **Positional Encoding** (or Positional Embeddings) an absolute requirement, whereas it was not required in older Recurrent Neural Network (RNN) architectures?

**4. (Multiple Choice)**
According to your answer key, which of the following model architectures is best suited for **text generation**?
A) Vision Transformer (ViT)
B) Convolutional Neural Network (CNN)
C) Generative Pre-trained Transformer (GPT)
D) Residual Network (ResNet)

**Question 5 (Calculation: Self-Attention Dot Product)**
You are computing the self-attention scores for a sentence with two tokens. 
You have the Query vector for Token 1: $Q_1 = [2, 0]$
You have the Key vectors for both tokens: $K_1 = [1, 1]$ and $K_2 = [0, 2]$

**(a)** Compute the raw attention score (dot product) for $Q_1 \cdot K_1$.
**(b)** Compute the raw attention score (dot product) for $Q_1 \cdot K_2$.
**(c)** Based on your math, which token is Token 1 paying more attention to?

**Question 6 (True/False)**
The Transformer encoder architecture relies heavily on Recurrent Layers to maintain the context of long paragraphs.

**Question 7 (Short Answer)**
In a single sentence, what is the primary conceptual purpose of the **Self-Attention** mechanism in a Transformer? (What does it allow the tokens to do?)

**Question 8 (Multiple Choice)**
In the self-attention mechanism, how are the **Query (Q), Key (K), and Value (V)** vectors generated?
A) Q and K are derived from the input text, while V is derived from the positional encoding.
B) They are all derived from the exact same input sequence using different learned weight matrices.
C) Q comes from the user's prompt, while K and V are pulled from a fixed internet database.
D) They are randomly generated during each forward pass to ensure the model doesn't overfit.

**Question 9 (Short Answer)**
While Bag-of-Words (BoW) and TF-IDF are useful classical NLP techniques for representing text, what is their biggest shared weakness when representing a sentence mathematically? *(Hint: Think about what happens to the sentence "The dog bit the man" vs "The man bit the dog").*

**Question 10 (Multiple Choice)**
What is the main advantage of using **Multi-Head Attention** instead of just a single attention mechanism in a Transformer?
A) It allows the model to process images and text at the exact same time.
B) It allows the model to attend to different types of relationships (e.g., grammar, coreference, sentiment) simultaneously.
C) It prevents the vanishing gradient problem during backpropagation.
D) It converts the continuous embedding vectors into discrete Bag-of-Words IDs.

***
***
*(Stop here! Complete the questions before scrolling down to the Answer Key)*
***
***

<br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & EXPLANATIONS**
  
  **1. False**
  *   **Explanation:** This is a trick question. The statement perfectly describes **Subword** tokenization, not Word-level. As your PDF states: *"Word-level tokenization splits text into full words, leading to large vocabularies and poor handling of rare words."*
  
  **2. B (It allows the model to process text sequentially...)**
  *   **Explanation:** Processing sequentially (one-by-one) is a characteristic of old RNNs, not a benefit of tokenization. A, C, and D are the exact benefits of Subword Tokenization listed in your professor's answer key.
  
  **3. Answer:**
  *   Because Transformers process all tokens in a sequence **in parallel** (simultaneously), they have no inherent understanding of spatial/sequential order. Positional embeddings must be mathematically added to the inputs so the model knows the order of the words.
  
  **4. C (Generative Pre-trained Transformer - GPT)**
  *   **Explanation:** Straight from your answer key. CNNs and ResNets are for vision. ViTs are for vision. GPTs are for text generation.
  
  **5. Self-Attention Calculation:**
  *   **(a)** $Q_1 \cdot K_1 = (2 \times 1) + (0 \times 1) = \mathbf{2}$
  *   **(b)** $Q_1 \cdot K_2 = (2 \times 0) + (0 \times 2) = \mathbf{0}$
  *   **(c)** Token 1 is paying more attention to **Token 1** ($K_1$). (The score of 2 is higher than 0). 
  
  **6. False**
  *   **Explanation:** Transformers completely abandoned Recurrent Layers. The standard Transformer Encoder relies strictly on Multi-Head Attention, Feed-Forward Networks, and Add/Norm layers.
  
  **7. Answer:**
  *   Self-attention allows each token/word to directly attend to (look at) **all other tokens** in the sequence simultaneously, modeling global relationships and gathering context.
  
  **8. B**
  *   **Explanation:** This is why it is called *Self*-Attention. The Query, Key, and Value vectors are all generated from the exact same input sequence by multiplying the input embeddings by different trained weight matrices ($W_Q, W_K, W_V$). 
  
  **9. Answer:**
  *   They both completely lose the **sequential word order** (and grammatical structure) of the sentence. Because they only count the *frequency* of words, "The dog bit the man" and "The man bit the dog" result in the exact same mathematical vector.
  
  **10. B**
  *   **Explanation:** A single attention head might focus entirely on one task (like figuring out which noun a pronoun belongs to). Multi-Head Attention splits the math up, allowing the model to assign different "heads" to learn different linguistic rules (e.g., tense, emotion, sentence structure) all at the exact same time. *(Note: Option C describes the "Add & Norm" residual connections).*
  
  ***
- ---
- ---
- ---
- ### **Practice Quiz: Vision Transformers (ViT)**
  
  **Question 1 (True/False)**
  Vision Transformers (ViTs) require convolution layers to process images before passing the data into the attention mechanism.
  
  **Question 2 (Multiple Choice)**
  Which of the following best describes the fundamental difference in how CNNs and Vision Transformers (ViTs) process an image?
  A) CNNs use global self-attention to read the whole image at once, while ViTs use local convolution filters to read it piece by piece.
  B) CNNs use local convolution filters encoding spatial locality, while ViTs use global self-attention where each patch attends to all others.
  C) CNNs require positional embeddings to understand the image, while ViTs inherently understand spatial order.
  D) CNNs split the image into 16x16 patches, while ViTs pass a sliding window over the individual pixels.
  
  **Question 3 (Calculation: ViT Patch Math)**
  You are building a Vision Transformer to process satellite images. 
  Your input image size is **$128 \times 128$** pixels (RGB color).
  You decide to split the image into patches of size **$16 \times 16$**.
  
  **(a)** How many total patches are created?
  **(b)** If each patch is flattened, what is the dimension (length) of each patch vector?
  **(c)** Assuming you include the optional `[CLS]` token, what is the total input sequence length fed into the Transformer?
  
  **Question 4 (Short Answer)**
  In the ViT pipeline, why must the $16 \times 16$ image patches be **flattened** before they enter the Transformer? 
  
  **Question 5 (Multiple Choice)**
  In a Vision Transformer, what does the Self-Attention mechanism between patches represent intuitively?
  A) It reduces the spatial dimensions of the image to save RAM.
  B) It applies a mathematical filter to sharpen the edges of the objects inside a single patch.
  C) Each patch attends to the others, capturing global relationships across the entire image.
  D) It translates the pixels into English text.
  
  **Question 6 (Short Answer)**
  Why are **Positional Embeddings** strictly required in a Vision Transformer? *(Use the specific reasoning from your professor's answer key).*
  
  **Question 7 (True/False)**
  To classify an image (e.g., "Dog" vs. "Cat"), a Vision Transformer takes the output vector of every single image patch, averages them together, and passes the average to the final classifier.
  
  ***
  ***
  *(Stop here! Complete the questions before scrolling down to the Answer Key)*
  ***
  ***
  
  <br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & EXPLANATIONS**
  
  **1. False**
  *   **Explanation:** Straight from your answer key: *"Vision Transformers do not require convolution layers."* ViTs completely abandon convolutions, instead relying entirely on slicing the image into patches and using linear projections.
  
  **2. B**
  *   **Explanation:** This is the exact wording from your professor's answer key! CNNs focus on **local spatial patterns** (because the filter only looks at a small $3\times3$ neighborhood at a time). ViTs use **global self-attention**, meaning Patch #1 in the top-left corner can instantly look at Patch #196 in the bottom-right corner in a single mathematical step.
  
  **3. ViT Patch Calculation:**
  *   **(a) Number of patches:** $(128 / 16)^2 = 8^2 = \mathbf{64 \text{ patches}}$.
  *   **(b) Patch vector dimension:** Since it is an RGB image, it has 3 color channels. $16 \times 16 \times 3 = \mathbf{768}$.
  *   **(c) Total sequence length:** 64 patches + 1 `[CLS]` token = $\mathbf{65}$.
  
  **4. Answer:**
  *   Each patch becomes a flattened 1D vector because **a Transformer expects sequences** (just like a 1D sequence of word embeddings in NLP). The Transformer architecture does not know how to process a 3D grid of pixels natively.
  
  **5. C**
  *   **Explanation:** Attention in vision works just like attention in text! Instead of a word looking at other words to gather grammatical context, an image patch looks at all other image patches to gather **global relationships** across the image (e.g., a patch containing a tire "attends" to a patch containing a windshield to understand it is looking at a car). 
  
  **6. Answer:**
  *   Positional embeddings are needed because **Transformers do not inherently encode spatial order.** Because they process all patches in parallel, the model wouldn't know if a patch came from the top-left of the image or the bottom-right without a positional "timestamp" added to it.
  
  **7. False**
  *   **Explanation:** ViTs do not average the patches! Instead, they use a special, extra input called the **Classification Token (`[CLS]`)**. The network uses the final output vector of this single `[CLS]` token to make the predicted class scores, ignoring the direct outputs of the other image patches.
  
  ***
- ---
- ---
- ---
- ### **Practice Quiz: CNNs & ResNet**
  
  **Question 1 (True/False)**
  In a Convolutional Neural Network (CNN), pooling layers are primarily used to increase the depth (number of channels) of the feature map.
  
  **Question 2 (Multiple Choice)**
  What is the primary reason we use CNNs instead of standard Fully-Connected MLPs for image processing?
  A) MLPs process all pixels in parallel, whereas CNNs process them sequentially.
  B) MLPs require global positional embeddings, which take up too much memory.
  C) CNNs use shared filters to learn local spatial patterns, drastically reducing the number of parameters and preventing overfitting.
  D) CNNs completely ignore spatial dimensions and treat the image as a Bag-of-Words.
  
  **Question 3 (Calculation: CNN Feature Map Size & Parameters)**
  An RGB image of size **$64 \times 64$** (Width $\times$ Height) is passed through a convolution layer with:
  *   **Filter size (F):** $3 \times 3$
  *   **Stride (S):** $1$
  *   **Padding (P):** $0$
  *   **Number of Filters:** $10$
  
  **(a)** Compute the output width/height of the feature map.
  **(b)** If we change the padding to **$P = 1$**, what is the new output size?
  **(c)** How many parameters (weights + bias) are there in a **single filter**? *(Remember: It's an RGB image!)*
  **(d)** What is the **total number of parameters** in this entire convolution layer?
  
  **Question 4 (Calculation: Manual Convolution)**
  You are given a $3 \times 3$ section of an input image and a $2 \times 2$ Filter (Kernel):
  
  **Image Patch:**
  `[2, 0, 1]`
  `[1, 3, 2]`
  `[0, 1, 1]`
  
  **Filter:**
  `[ 1,  0]`
  `[-1,  1]`
  
  **(a)** Calculate the **top-left** element of the output feature map. *(Stride = 1)*
  
  **Question 5 (Calculation: Max Pooling)**
  You have the following $4 \times 4$ activation map generated by a CNN:
  
  `[1,  3,   2,  4]`
  `[5,  0,   8,  1]`
  `[2,  2,   0,  3]`
  `[1,  4,   1,  2]`
  
  You apply **Max Pooling** with a **$2 \times 2$ pool size** and a **Stride of 2**.
  **(a)** What will be the dimensions of the new output grid?
  **(b)** Write out the exact numbers in the new pooled output grid.
  
  **Question 6 (Short Answer)**
  Before the invention of ResNet, researchers experienced the **"Degradation Problem"** when they tried to build 34-layer or 50-layer CNNs. What is the degradation problem, and what specific mathematical issue causes it?
  
  **Question 7 (Multiple Choice)**
  How does a Residual Network (ResNet) solve the degradation problem mentioned above?
  A) It converts the image into 16x16 patches.
  B) It adds a skip connection (or shortcut) that adds the original input back to the output of the layer: $H(x) = F(x) + x$, mitigating vanishing gradients.
  C) It replaces all convolutional layers with global self-attention mechanisms.
  D) It uses Average-Pooling instead of Max-Pooling to preserve the gradient.
  
  ***
  ***
  *(Stop here! Complete the questions before scrolling down to the Answer Key)*
  ***
  ***
  
  <br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & EXPLANATIONS**
  
  **1. False**
  *   **Explanation:** From the answer key: *"Pooling reduces spatial dimensions."* It shrinks the width and height (e.g., from 32x32 down to 16x16). The *Convolutional filters* are what determine the depth/number of channels.
  
  **2. C**
  *   **Explanation:** Directly from the slides. MLPs require every pixel to connect to every neuron, leading to tens of millions of parameters for a tiny image. CNNs slide a small (e.g., $3 \times 3$) shared filter across the image, drastically cutting down parameter counts and preserving local spatial locality.
  
  **3. CNN Math:**
  *   **(a) Output Size ($P=0$):** Use the formula $(N - F + 2P) / S + 1$. 
    *   $(64 - 3 + 0) / 1 + 1 = 61 + 1 = \mathbf{62 \times 62}$.
  *   **(b) Output Size ($P=1$):** 
    *   $(64 - 3 + 2(1)) / 1 + 1 = (64 - 3 + 2) + 1 = 63 + 1 = \mathbf{64 \times 64}$. *(Notice how Padding=1 perfectly preserves the original 64x64 size when using a 3x3 filter!)*
  *   **(c) Params per filter:** 
    *   Formula: $(F \times F \times \text{Channels}) + 1 \text{ bias}$. 
    *   Since it's an RGB image, Channels = 3. 
    *   $(3 \times 3 \times 3) + 1 = 27 + 1 = \mathbf{28 \text{ parameters per filter}}$.
  *   **(d) Total parameters:** 
    *   Formula: (Params per filter) $\times$ (Number of filters).
    *   $28 \times 10 = \mathbf{280 \text{ total parameters}}$.
  
  **4. Manual Convolution:**
  *   **(a)** Overlay the $2 \times 2$ filter onto the top-left $2 \times 2$ corner of the image:
    `[2, 0]`
    `[1, 3]`
    *   Multiply matching squares: $(2 \times 1) + (0 \times 0) + (1 \times -1) + (3 \times 1)$
    *   Sum: $2 + 0 - 1 + 3 = \mathbf{4}$
  
  **5. Max Pooling Calculation:**
  *   **(a) Dimensions:** A $4 \times 4$ grid pooled with a $2 \times 2$ window and a stride of 2 will shrink exactly in half. Output size is $\mathbf{2 \times 2}$.
  *   **(b) The Grid:** You break the $4 \times 4$ grid into four distinct $2 \times 2$ quadrants and find the maximum number in each:
    *   Top-Left quadrant `[1,3 / 5,0]`: Max is **5**.
    *   Top-Right quadrant `[2,4 / 8,1]`: Max is **8**.
    *   Bottom-Left quadrant `[2,2 / 1,4]`: Max is **4**.
    *   Bottom-Right quadrant `[0,3 / 1,2]`: Max is **3**.
    *   *Final Output Grid:* 
        `[5, 8]`
        `[4, 3]`
  
  **6. Answer:**
  *   The Degradation Problem occurs when a deeper network (e.g., 34 layers) performs *worse* (has higher training and validation error) than a shallower network (e.g., 18 layers). This is caused by the **Vanishing Gradient Problem**, where the error signal shrinks to nearly zero as it propagates backward through many layers, preventing the early layers from learning.
  
  **7. B**
  *   **Explanation:** From the answer key: *"Residual connections mitigate vanishing gradients."* The skip connection gives the gradient a clean, math-free highway to travel backward through the network, allowing researchers to build networks hundreds of layers deep!
  
  ***