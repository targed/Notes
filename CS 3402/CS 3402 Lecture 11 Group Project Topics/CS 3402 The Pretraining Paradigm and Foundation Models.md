### **Part 1: The Pretraining Paradigm & Foundation Models**
**(Covering Slides 2–7)**

Up until now, you have been taught the "Traditional" Machine Learning pipeline: Collect labeled data $\rightarrow$ Train a model from scratch $\rightarrow$ Make predictions. 
This section introduces the modern **Foundation Model** workflow, which completely flips that script.
- #### **1. What is a Foundation Model? (Slide 2)**
  *   **The Old Way (Task-Specific):** If you wanted an AI to detect spam, you built a small model and trained it *only* on spam emails. If you wanted to translate English to French, you built a totally different model. 
  *   **The New Way (Foundation Models):** You build one **massive** model, train it on a diverse, gigabyte-to-terabyte scale dataset (essentially the whole internet), and then "adapt" it to do almost anything.
  *   **Self-Supervised Learning:** How do you label the whole internet? You don't. The model plays a game with itself. 
    *   *Example:* It takes a sentence, hides a word ("The cat sat on the [BLANK]"), and tries to guess it. It does this billions of times until it learns the underlying rules of human language.
- #### **2. The Three Flavors of Foundation Models (Slides 4–6)**
  
  **A. Language Foundation Models (LLMs) (Slide 4)**
  *   Models like GPT-4, LLaMA, and PaLM. 
  *   *The Trend:* **Scaling**. The slide shows a bubble chart representing parameter counts (the "weights" we calculated in previous lectures). We have gone from millions of parameters (BERT) to hundreds of billions (GPT-3/4). This requires massive GPU clusters.
  
  **B. Vision Foundation Models (Slide 5)**
  *   Models like **SAM (Segment Anything Model)** and **DINOv2**.
  *   *The Concept:* Instead of learning text, these models look at billions of images without labels and learn how to extract "Visual Features" (like depth, object boundaries, and textures). SAM can outline any object in a picture, even if it has never seen that specific object before.
  
  **C. Multimodal Foundation Models (Slide 6)**
  *   Models like **ChatGPT, Gemini, and Claude**.
  *   *The Concept:* These models map text, images, and audio into the *same mathematical space*. This means you can hand the AI an image of a math problem and ask it in English to solve it.
- #### **3. Why is this a Paradigm Shift? (Slide 7)**
  If these models cost millions of dollars to train, why do we use them? 
  *   **Transfer Learning:** A Foundation Model learns **general patterns** (like English grammar, logic, or edge detection). 
  *   **Reduced Data Cost:** In traditional ML, if you want to build a Medical AI, you need 100,000 labeled medical records. With a Foundation Model, it already knows how to read. You only need to give it maybe 500 labeled medical records (called **Fine-Tuning**) to turn it into a world-class Medical AI. 
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Zero-Shot vs. Few-Shot vs. Fine-Tuning:** The slides mention "prompting" or "fine-tuning" for best performance. 
    *   *Zero-Shot:* Asking the model a question directly.
    *   *Few-Shot:* Giving it 3 examples in the prompt, then asking the question.
    *   *Fine-Tuning:* Actually changing the model's math (weights) using Gradient Descent on a small, specific dataset. 
  
  ---
-