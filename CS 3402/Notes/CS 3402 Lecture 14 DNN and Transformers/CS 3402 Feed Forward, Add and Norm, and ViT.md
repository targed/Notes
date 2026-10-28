### **1. The "Add" (Residual Connections) (Slide 62)**
Look at the diagrams on the right side of these slides. Notice the black arrows that bypass the Attention box and the Feed Forward box, pointing directly to the `Add & Norm` boxes? These are called **Skip Connections** (or Residuals).

*   **The Concept:** Instead of just sending the *output* of the Attention mechanism to the next layer, we take the *original input* and **add** it to the output. ($Input + Output$).
*   **Why do we do this?** As neural networks get deeper (e.g., 100 layers), they suffer from the **Vanishing Gradient Problem** (which we briefly discussed in Lecture 12). By the time the calculus (Backpropagation) reaches the first layer, the numbers shrink to $0$ and the model stops learning. 
*   **The Fix:** Skip connections act like a mathematical "highway." They allow the raw gradient to bypass the complex math blocks and flow freely all the way back to the beginning, allowing us to train massively deep networks.
- ### **2. The "Norm" (Layer Normalization) (Slide 62)**
  After we "Add," we "Norm." 
  *   Does "Mean 0, Std dev 1" sound familiar? It should! This is exactly the **Z-Score Normalization** you learned in Lecture 5. 
  *   Instead of just normalizing our data *once* before training, the Transformer actively normalizes the data *inside* the network after every single major step.
  *   **The Benefit:** It prevents the numbers inside the matrix from exploding into the billions, which stabilizes training and acts as a **Regularization effect** (preventing overfitting).
- ### **3. The Feed Forward Network (Slide 61)**
  After the words have looked at each other in the Attention block, the new context-rich vectors are passed into a **Feed Forward Network (FFN)**.
  
  *   **What is it?** This is just a standard, classic Multi-Layer Perceptron (MLP) like the ones we covered in Lecture 12. It consists of Linear layers and a Non-linear Activation function (like ReLU or GELU).
  *   **The Division of Labor:** 
    *   *Self-Attention* allows the words to mix and share information with each other. 
    *   *Feed Forward* looks at each word's new vector **independently** and applies complex, non-linear math to extract deeper meaning from it.
- ### **4. Stacking it up: The Complete Encoder (Slide 63)**
  Everything we just talked about (Input $\rightarrow$ Attention $\rightarrow$ Add/Norm $\rightarrow$ FFN $\rightarrow$ Add/Norm) makes up **ONE** Encoder block.
  
  *   If you look closely at the diagram on the right, you will see an **$N \times$** next to the big gray box. 
  *   Modern AI models don't just do this once. They stack these Encoder blocks on top of each other. The output of Encoder 1 becomes the input to Encoder 2. 
  *   *Example:* The original BERT model stacked **12** of these Encoders on top of each other. GPT-3 stacked **96** of them!
- ### **5. The Vision Transformer (ViT) (Slide 64)**
  The professor drops a massive industry secret at the bottom of Slide 64: **"Vision Transformers (ViT) use essentially the Transformer encoder architecture."**
  
  *   **The Paradigm Shift:** For 10 years, the entire world used **CNNs** (Convolutional Neural Networks) for images. But the Transformer was so mathematically perfect that researchers asked: *"Can we use this for pictures?"*
  *   **How it works:** Instead of chopping a sentence into "word tokens", you take an image and chop it into a grid of 16x16 **"image patches"**. You flatten those patches into vectors, give them position encodings (so the model knows where the patch was in the image), and feed them directly into the exact same Encoder we just studied!
  *   **The Result:** ViTs absolutely crushed CNNs and are now the state-of-the-art for Computer Vision. This is the foundation for your **Group Project Topic 1 and Topic 5**!
  
  ---
- ### **Action Items & Final Review:**
  *   **The Checkmarks (Slide 64):** Look at the green checkmarks on Slide 64. You should now be able to explain what every single one of those terms means. If you can, you understand the architecture of ChatGPT.
  *   **The MLP Practice (Slide 65):** The Colab link here is the same one provided in Lecture 12. Since you now know that a basic MLP lives *inside* the Transformer, mastering that Colab notebook is your best step for writing PyTorch code!
  
  ***