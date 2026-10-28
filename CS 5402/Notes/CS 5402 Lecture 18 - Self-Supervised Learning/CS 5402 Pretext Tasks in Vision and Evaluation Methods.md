## 1. Classic Image Pretext Tasks (Slides 14–21)
  Before modern Contrastive Learning (like SimCLR), researchers invented clever "puzzles" for CNNs to solve. To solve these puzzles, the network was forced to learn the semantic meaning of objects.
  
  *   **Task 1: Predicting Rotations (Slides 14–16)**
    *   *The Setup:* Take an unlabeled image, rotate it by 0°, 90°, 180°, or 270°. Pass it to the CNN and ask it to predict which rotation was applied.
    *   *The Logic:* To know a picture of a dog is upside down, the network must first learn what a dog is, where its legs are, and that legs belong on the ground (visual commonsense).
    *   *The Result (Slide 16):* The "Attention Maps" show that the SSL model learns to focus on the exact same important features (eyes, faces) as a fully Supervised model, without ever being given a label!
  *   **Task 2: Relative Position (Slide 17)**
    *   *The Setup:* Extract two random patches from an image (e.g., a cat's ear and a cat's nose). Ask the network to predict the spatial relationship between the two patches (e.g., is the ear above, below, or next to the nose?).
  *   **Task 3: Solving Jigsaw Puzzles (Slide 18)**
    *   *The Setup:* Cut an image into a $3 \times 3$ grid, shuffle the tiles, and ask the network to predict the correct permutation to reassemble the original image.
  *   **Task 4: Inpainting / Missing Pixels (Slide 19)**
    *   *The Setup:* Black out a large square in the center of an image. Ask an Autoencoder to generate the missing pixels based on the surrounding context.
  *   **Task 5: Masked Autoencoders (MAE) (Slide 21)**
    *   *The Modern Approach:* Mask out a massive percentage (e.g., 75%) of the image patches. Pass only the visible 25% through the Encoder. Then, use a Decoder to reconstruct the full image. (This is highly efficient because the Encoder only processes 25% of the data).
## 2. The Flaw of Pretext Tasks (Slides 25–26)
  While clever, these early SSL methods had major limitations.
  *   **The Problem:** The network often learned "shortcuts" instead of true semantic meaning. 
    *   *Example:* When solving a Jigsaw puzzle, instead of learning what a tiger looks like, the network might just learn to match the edge pixels of the tiles (like finding a continuous line of fur across a cut).
  *   **The Conclusion:** Learned representations were often tied specifically to the pretext task and didn't generalize well to other downstream tasks.
  
  ---
## 3. How to Evaluate an SSL Model (Slides 22–24)
  If we don't have labels during training, how do we grade the model's performance? 
  We don't care how well the model solves the Jigsaw puzzle; we care how useful the extracted features are for a real-world task (like Image Classification).
  
  **The Evaluation Protocol (Slide 22):**
  1.  **Pretext Task Performance:** A basic sanity check to ensure the model isn't just guessing randomly.
  2.  **Representation Quality (The Gold Standard):**
    *   *Linear Evaluation Protocol:* This is the most common metric in SSL papers. You freeze the entire trained Feature Extractor. You attach a single, simple Linear Classifier on top. You train *only* that linear layer on a labeled dataset (like ImageNet). If the linear classifier achieves high accuracy, it proves the frozen features were linearly separable and highly meaningful.
    *   *Clustering / t-SNE:* Visualize the frozen features. Do images of dogs naturally cluster together in the vector space?
  3.  **Transfer Learning Performance:** Take the frozen feature extractor trained on ImageNet (natural photos) and see how well it performs when fine-tuned on a completely different dataset (e.g., Medical X-Rays or Satellite Imagery).
  
  ***
# Problem Solving & Concept Check
  
  **1. Identifying the Shortcut:**
  You design an SSL pretext task where you convert colored images to black-and-white, and ask the CNN to predict the original colors (Image Colorization). 
  *   What is the intended "Visual Commonsense" you want the model to learn? 
  *   What "Shortcut" might the model learn instead that ruins its usefulness?
  *   *Answer:* You want it to learn that the sky is blue, grass is green, and apples are red (semantic meaning). A shortcut would be learning that a specific texture of pixels almost always has a specific hex code associated with it, regardless of what the object actually is.
  
  **2. Linear Probing Protocol:**
  Why do researchers use a simple *Linear Classifier* (like Logistic Regression) instead of a complex Multi-Layer Perceptron (MLP) to evaluate the quality of the frozen SSL representations?
  *   *Answer:* A complex MLP is powerful enough to learn new, non-linear relationships on its own. If you use an MLP, you can't tell if the high accuracy is because the SSL features were good, or because the MLP was just really smart. A simple Linear Classifier has very low capacity; it can *only* achieve high accuracy if the SSL features it received were already perfectly organized and linearly separable. 
  
  **3. Masked Autoencoders (MAE):**
  In the MAE architecture (Slide 21), why is the Decoder discarded after the pre-training phase?
  *   *Answer:* The Decoder's only job is to translate the abstract, compressed feature vectors back into raw pixels to calculate the reconstruction loss. For downstream tasks (like classification), we don't care about raw pixels; we only care about the rich, compressed understanding of the image. The Encoder holds all that understanding, so we keep the Encoder and throw away the Decoder.
  
  ***