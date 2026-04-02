### **Part 1: The Transformer Architecture & The Input Pipeline**
**(Covering Slides 13–26)**

Before Transformers were invented by Google in 2017, AI translated text word-by-word (from left to right). It was slow and often forgot the beginning of a long sentence by the time it reached the end. 

The Transformer changed everything by processing **all words at the exact same time**.
- #### **1. The Macro View of a Transformer (Slides 13–17)**
  Look at the giant diagram on Slide 13. It looks terrifying, but it is just two main blocks:
  *   **The Encoder (Left side):** Its job is to read the input (e.g., English text) and turn it into a massive, contextually rich mathematical representation.
  *   **The Decoder (Right side):** Its job is to take that math and generate the output (e.g., German text) one token at a time.
  *   *Note:* Models like ChatGPT use *only* the Decoder. Models like BERT use *only* the Encoder. The slide deck will focus primarily on the **Encoder** side.
- #### **2. The Input Pipeline (Slides 18–21)**
  Before the Transformer can do any "thinking," we have to prep the data. As we learned in Lecture 13, this is a two-step process:
  1.  **Tokenization (Slide 19):** `"I ate an apple"` becomes `["I", "ate", "an", "apple", "<eos>"]`. (*`<eos>` means End of Sentence*).
  2.  **Input Embeddings (Slides 20–21):** Each token is converted into a dense vector (e.g., an array of 768 floating-point numbers). 
    *   *Visual:* On Slide 21, you see the blue rectangles turn into colored columns. This is the AI's internal "dictionary definition" of each word.
- #### **3. The Missing Link: Position Encodings (Slides 22–24)**
  This is the **first brand-new concept** of this lecture. 
  
  *   **The Problem (Slide 23):** As I mentioned earlier, Transformers don't read left-to-right. They process the entire sentence simultaneously (in parallel). Because of this, the Transformer has no idea what order the words are in! 
    *   To a Transformer without position encodings, `"I ate an apple"` and `"an apple ate I"` look mathematically identical.
  *   **The Solution (Slide 24):** **Position Encodings**. 
    *   We generate a *second* vector that acts as a "timestamp" or "index number" for the word (e.g., Vector 1 says "I am in position 1", Vector 2 says "I am in position 2").
  *   **The Addition:** We literally **add** the Position Vector to the Embedding Vector ($Embedding + Position$). 
    *   *Result:* The new vector now contains the *meaning* of the word AND its *location* in the sentence.
- #### **4. The "Where is the Context?" Problem (Slide 26)**
  At this point in the diagram, we have our vectors ready. But we have a major problem:
  *   The vector for `"apple"` knows it means a fruit, and it knows it is the 4th word in the sentence.
  *   **BUT**, it doesn't know that `"I"` is the one eating it!
  *   At the bottom of the Encoder, words are completely isolated from each other. They lack **Context**. 
  
  This perfectly sets up the next section: **Attention**.
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Parallelization:** The reason Transformers overtook older models (like RNNs) is because of how they process inputs. Because we inject the position encoding directly into the math, we can feed all 5 words into the GPU at the exact same time. This is why you need massive GPUs to train them, but it's also why they train so fast.
  *   **The `<eos>` Token:** Why include "End of Sentence"? It tells the model's math to stop expecting more context and prepare to finalize its thoughts.
  
  ---