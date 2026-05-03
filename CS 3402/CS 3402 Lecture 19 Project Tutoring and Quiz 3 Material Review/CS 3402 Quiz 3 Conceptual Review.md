### **Part 1: Quiz 3 Conceptual Review**
**(Covering Slides 13–20)**

This section tests your high-level understanding of the architecture differences between Vision Models and Text Models.
- #### **1. True/False Questions (Slide 13)**
  *   **1. In CNNs, pooling layers reduce spatial dimensions.**
    *   *Answer:* **True.** (Pooling layers like Max-Pooling take a $2 \times 2$ pixel neighborhood and compress it into a single number, shrinking the width and height of the feature map to save memory).
  *   **2. Vision Transformers require convolution layers to process images.**
    *   *Answer:* **False.** (This is the whole point of ViTs! They completely abandon convolutions. Instead, they chop the image into squares, flatten them, and use linear layers).
  *   **3. Residual connections help mitigate vanishing gradient problems.**
    *   *Answer:* **True.** (The $F(x) + x$ "skip connection" gives the gradient a math-free highway to travel backward through deep networks like ResNet without shrinking to zero).
  *   **4. CNNs learn local spatial patterns using shared filters.**
    *   *Answer:* **True.** (The kernel/filter is a small neighborhood that slides across the entire image, *sharing* the same weights at every step).
  *   **5. Self-attention allows each word to directly attend to all other words in a sentence.**
    *   *Answer:* **True.** (This parallel processing is what gives Transformers their context capabilities).
- #### **2. Multiple Choice Questions (Slides 14–18)**
  *   **Which model is best suited for text generation? (Slide 14)**
    *   *Answer:* **B. GPT.** (Generative Pre-trained Transformer. CNNs and ResNets are for vision, and standard Vision Transformers don't generate text).
  *   **What is the primary purpose of attention in Transformers? (Slide 15)**
    *   *Answer:* **C. Model relationships between tokens.** (It allows tokens to look at each other to figure out context, like matching pronouns to nouns).
  *   **Which statement about CNNs is correct? (Slide 16)**
    *   *Answer:* **C. They use shared convolution filters.** (They do *not* use self-attention or positional encoding, and they absolutely *do* process images).
  *   **In Vision Transformers, an image is first: (Slide 17)**
    *   *Answer:* **B. Split into patches.** (Usually $16 \times 16$ pixel squares).
  *   **Which component is NOT part of a Transformer encoder? (Slide 18)**
    *   *Answer:* **C. Recurrent layer.** (Transformers were invented specifically to *replace* Recurrent layers like RNNs).
- #### **3. Short Answer Practice (Slides 19–20)**
  
  **Prompt 1: Explain the difference between Word-level tokenization and Subword tokenization. Also explain why subword tokenization is preferred in modern LLMs. (Slide 19)**
  *   *Answer:* Word-level tokenization splits text purely by spaces (e.g., mapping whole words to IDs). Subword tokenization breaks words down into smaller chunks or syllables (e.g., "unbelievable" $\rightarrow$ "un", "believ", "able"). 
  *   *Why it's preferred:* Subword tokenization drastically shrinks the vocabulary size (saving RAM) and prevents the model from crashing when it sees an Out-Of-Vocabulary (OOV) word, because it can just piece the new word together from known chunks.
  
  **Prompt 2: List two differences between CNNs and Vision Transformers in image processing. (Slide 20)**
  *   *Difference 1 (Mechanism):* CNNs use **sliding convolution filters** to look at local neighborhoods of pixels. ViTs chop the image into **flattened patches** and process them all in parallel using **Self-Attention**.
  *   *Difference 2 (Spatial Awareness):* CNNs inherently understand 2D space because of how the filter slides (Translation Invariance). ViTs have *no* inherent understanding of space and must be artificially fed **Positional Encodings** to know where the patches came from.
  
  ---
- ### **Action Items for Section 1:**
  *   **Mental Checklist:** If a question asks about *Local* features or *Shared Filters*, think **CNN**. If a question asks about *Global* context, *Patches*, or *Positional Encodings*, think **Transformer/ViT**.