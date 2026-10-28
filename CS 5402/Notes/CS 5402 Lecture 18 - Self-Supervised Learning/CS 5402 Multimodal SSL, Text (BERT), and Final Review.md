## 1. Multimodal Contrastive Learning (CLIP) (Slides 42–44)
  What if we want an AI that understands both vision *and* language simultaneously? 
  
  **The CLIP Architecture (Slide 42):**
  Instead of taking an image and creating two augmented versions (like SimCLR), CLIP takes an **Image** and its corresponding **Text Caption** from the internet.
  *   **The Encoders:** Pass the image through an Image Encoder (CNN/ViT) and the text through a Text Encoder (Transformer).
  *   **The Contrastive Matrix:** You create a massive batch of Image-Text pairs.
    *   **Positive Pairs (The Diagonal):** Image 1 should have high cosine similarity with Text 1. 
    *   **Negative Pairs (Off-Diagonal):** Image 1 should have low similarity with Text 2, Text 3, etc.
  *   **The Magic (Zero-Shot Prediction):** Because the model perfectly aligns text and images in the same mathematical space, you can hand it a completely new image, give it a text list (`["plane", "car", "dog"]`), and it will classify the image *without ever being explicitly trained on those labels!* 
  
  **Other Modalities:**
  *   **Medical (Slide 43):** Contrastive learning between a Chest X-ray and the radiologist's written clinical report.
  *   **Time-Series (Slide 44):** Contrastive learning between a 1D raw EEG signal and a 2D Spectrogram image of that exact same signal (two different "views" of the same data).
  
  ---
## 2. SSL for Text: BERT (Slides 45–46)
  Before ChatGPT, there was **BERT**. BERT is a pure Self-Supervised Learning engine for text. It uses two massive Pretext Tasks to learn human language from reading Wikipedia and books:
  
  *   **Pretext Task 1: Masked Language Modeling (MLM)**
    *   *The Setup:* Take a sentence, and randomly replace **15%** of the words with a `[MASK]` token.
    *   *The Task:* Force the model to predict the missing words based on the surrounding context (e.g., "The cat `[MASK]` on the mat" $\to$ "sat"). 
    *   *Why it works:* It forces the model to learn grammar, syntax, and semantic meaning perfectly.
  *   **Pretext Task 2: Next Sentence Prediction (NSP)**
    *   *The Setup:* Feed the model two sentences (A and B). 50% of the time, B is the actual next sentence. 50% of the time, B is a random sentence from a different document.
    *   *The Task:* Binary classification: Is `IsNext == True` or `False`?
    *   *Why it works:* It forces the model to learn long-term logical coherence and topic flow across a whole paragraph.
  
  ---
# Answers to After-Class Questions (Slide 50)
  These are guaranteed conceptual exam questions. Here is exactly how to answer them:
  
  **Q1: What is the key difference between supervised, unsupervised, and self-supervised learning? Provide an example of each.**
  *   **Supervised:** Learns from human-provided labels. (Ex: Training a CNN on a dataset where humans tagged images as "Dog" or "Cat").
  *   **Unsupervised:** Finds hidden structures in completely unlabelled data. (Ex: K-Means clustering grouping customers by spending habits).
  *   **Self-Supervised:** Uses unlabeled data, but *automatically generates its own pseudo-labels* from the data structure to solve a pretext task using supervised algorithms. (Ex: Masking out a word in a sentence and training the model to predict it).
  
  **Q2: [True or False] Self-supervised learning does not require any labels.**
  *   **True (with a caveat).** It does not require any *human-annotated* labels. It generates its own "pseudo-labels" internally from the data (e.g., it knows the answer to the Jigsaw puzzle because it cut the image up itself). 
  
  **Q3: Many SSL methods rely on predicting part of the data from other parts (e.g., masked autoencoders).** 
  **1. How does this help the model learn useful representations?** 
  *   To successfully fill in the blanks (like reconstructing a dog's missing face), the model must develop a deep "visual commonsense" regarding object shapes, textures, and real-world continuity. 
  **2. Can this approach fail in some types of data?**
  *   Yes. If the data is purely random noise or lacks inherent spatial/temporal relationships (e.g., predicting a missing row in a totally unorganized database of independent users), the model cannot learn meaningful context.
  
  **Q4: You have a time-series dataset with sensor readings but no labels. Design a self-supervised pretext task to learn useful features. Explain your reasoning.**
  *   *Design 1 (Masking):* Mask out a 5-second window in the middle of the sensor data and ask the model to predict the missing waveform. *Reasoning:* Forces the model to learn the cyclical patterns/rhythms of the sensor.
  *   *Design 2 (Contrastive):* Take a 10-second window. Apply random noise to it (Augmentation A) and scale its amplitude slightly (Augmentation B). Treat these as a Positive Pair, and a window from a different hour as a Negative Pair. *Reasoning:* Forces the model to learn the core "shape" of the signal while ignoring random noise and scale changes.
  
  **Q5: Suppose you train an SSL model on a large image dataset. How could you evaluate the quality of learned representations without fine-tuning on a downstream task?**
  *   **Method 1 (Clustering / t-SNE):** Pass the data through the frozen encoder, extract the feature vectors, and plot them using t-SNE. If the representations are high-quality, images of the same class (e.g., all cars) will naturally cluster together without any labels guiding them.
  *   **Method 2 (Linear Probing):** Freeze the encoder completely. Attach a single, simple Linear Classifier (like Logistic Regression) to the end and train *only* that linear layer on a small labeled dataset. If the simple linear model achieves high accuracy, it proves the SSL encoder successfully untangled the data into a linearly separable format.
  
  ***