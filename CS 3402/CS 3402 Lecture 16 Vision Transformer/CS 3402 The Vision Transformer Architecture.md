### **The Vision Transformer (ViT) Architecture**
**(Covering Slides 51–57)**

For over a decade, Convolutional Neural Networks (CNNs) were the absolute kings of Computer Vision. In 2021, a team of researchers published the paper *"An Image is Worth 16x16 Words"* and proved that the exact same Transformer built for reading text (ChatGPT/BERT) could crush CNNs at looking at pictures.

Here is the step-by-step breakdown of how you force an image into a text-based model.
- #### **1. The Problem: Images aren't Words (Slide 51-52)**
  A Transformer expects a sequence of 1D vectors (word embeddings). An image is a 3D grid of numbers (Height $\times$ Width $\times$ 3 RGB Color Channels). You cannot feed a 3D grid directly into a standard Transformer.
- #### **2. Step 1: Patch Extraction (Slide 53)**
  How do we turn a picture of a cat into "words"? We chop it up.
  *   The image is sliced into a grid of non-overlapping squares called **Patches**. 
  *   In the famous paper, they used $16 \times 16$ pixel squares (hence the title *"An Image is Worth 16x16 Words"*).
  *   **The Math:** Each patch has 16 pixels of height, 16 pixels of width, and 3 color channels (Red, Green, Blue). So, one patch is a tensor of shape `[3, 16, 16]`. 
  *   If your original image is $224 \times 224$ pixels, you will end up with exactly $196$ of these patches (since $224/16 = 14$, and $14 \times 14 = 196$).
- #### **3. Step 2: Linear Projection (The "Embedding") (Slide 54)**
  The Transformer still can't read a `[3, 16, 16]` square. It needs a flat 1D array.
  *   We flatten the patch into a single long list of numbers ($3 \times 16 \times 16 = 768$ numbers).
  *   We then pass it through a simple linear layer to map it to a standard $D$-dimensional vector.
  *   *Result:* Each image patch is now mathematically identical to an NLP **Word Embedding**. The AI literally treats the top-left corner of the image as "Word 1", the next patch as "Word 2", etc.
- #### **4. Step 3: Positional Encodings (Slide 54)**
  As we learned in Lecture 14, Transformers process everything in parallel. They have no concept of space or order. 
  *   If we just fed the patches in, the AI wouldn't know if the cat's ear was in the top left or the bottom right. The image would be a scrambled jigsaw puzzle.
  *   *The Fix:* Just like with text, we mathematically **add** a learned 1D Positional Embedding to each patch vector so the model knows its original $X, Y$ location in the grid.
- #### **5. Step 4: The Magic `[CLS]` Token (Slide 56)**
  This is a critical concept for how Transformers do classification.
  *   Before feeding the patches into the model, we append one extra, completely fake "word" to the very beginning of the sequence. This is called the **Classification Token** or `[CLS]`.
  *   *Why do we need this?* As the patches pass through the Self-Attention layers, they all share information with each other. By the end of the network, we need *one single vector* that summarizes the entire image to make our final guess. The `[CLS]` token acts as the "sponge" that absorbs the global context of the whole picture.
- #### **6. Step 5: The Transformer Encoder & Output (Slides 55 & 57)**
  *   **The Encoder (Slide 55):** Notice the giant green box? The slide notes: **"Exact same as NLP Transformer!"** The researchers did not change the architecture *at all*. The image patches go through the exact same Multi-Head Attention and Feed Forward layers that text does. 
  *   **The Output Head (Slide 57):** After the final layer, we throw away all the outputs for the image patches. We grab *only* the output vector from the special `[CLS]` token position. 
  *   We pass that single `[CLS]` vector through a final Linear Projection (an MLP head) to map it to our class scores (e.g., 90% Cat, 10% Dog).
  
  ---
- ### **Summary: The ViT Pipeline**
  1. Chop image into $16 \times 16$ patches.
  2. Flatten patches into 1D vectors.
  3. Add a `[CLS]` token to the front.
  4. Add Positional Encodings so the model knows where the patches came from.
  5. Run the sequence through the standard Transformer Encoder.
  6. Use the `[CLS]` output to predict the class.
  
  ***