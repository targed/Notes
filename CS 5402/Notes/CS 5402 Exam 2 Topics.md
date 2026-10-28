### **1. Ensemble Learning (Lecture 11)**
  *   **The Bias-Variance Tradeoff:**
    *   How Bagging reduces *Variance* (prevents overfitting).
    *   How Boosting reduces *Bias* (fixes underfitting).
  *   **Bagging \& Random Forests:**
    *   Bootstrapping (Sampling with replacement, Out-Of-Bag samples).
    *   *Random Forest's Extra Trick:* Choosing a random subset of features at every split to decorrelate the trees.
  *   **Boosting \& AdaBoost:**
    *   Sequential training (focusing on previous mistakes).
    *   **Math/Calculations:**
        *   Calculating Weighted Error ($\epsilon$).
        *   Calculating Model Weight / Amount of Say ($\alpha = \frac{1}{2} \ln(\frac{1-\epsilon}{\epsilon})$).
        *   Updating Data Weights ($e^\alpha$ for mistakes, $e^{-\alpha}$ for correct predictions).
        *   Final classification using weighted majority vote.
### **2. Clustering (Lectures 12 \& 13)**
  *   **Distance Metrics:**
    *   Minkowski distances ($L_1$ Manhattan, $L_2$ Euclidean, $L_\infty$ Chebyshev).
    *   Cosine Distance (measures angle, ignores magnitude; best for text).
  *   **K-Means (Hard Clustering):**
    *   The Algorithm: Initialize $\to$ Assign (E-step) $\to$ Recompute Means (M-step).
    *   Minimizing the Cost Function / Sum of Squared Errors (SSE).
    *   *Flaws:* Sensitive to outliers, assumes spherical clusters, gets stuck in Local Minima (Requires multiple random restarts).
    *   Choosing K: The **Elbow Method**.
  *   **Gaussian Mixture Models (GMM / Soft Clustering):**
    *   Outputs probability of membership rather than a strict 1-or-0 assignment.
    *   Learns 3 parameters per cluster: Mean ($\mu$), Covariance Matrix ($\Sigma$), and Prior ($\pi$).
    *   The Expectation-Maximization (EM) Algorithm.
### **3. Dimensionality Reduction (Lecture 14)**
  *   **The Curse of Dimensionality:**
    *   Data becomes exponentially sparse; Euclidean distance loses meaning (closest and farthest neighbors become equidistant).
    *   *Math:* Volume of the small cube equation ($l \approx (K/N)^{1/d}$).
  *   **Principal Component Analysis (PCA):**
    *   **Linear** transformation; maximizes global variance and minimizes projection error.
    *   *Flaw:* Crushes non-linear manifolds (e.g., the Swiss Roll).
  *   **t-SNE:**
    *   **Non-linear** transformation; preserves *local* neighborhoods.
    *   Converts distance to probabilities using Gaussian (High-D) and Student's t-distribution (Low-D) to solve the **Crowding Problem**.
    *   Optimized using KL Divergence.
    *   Hyperparameter: **Perplexity** (too low = shattered clusters; too high = one giant blob).
### **4. Neural Networks Basics (Lecture 15)**
  *   **Architecture:** Input, Hidden, and Output layers. Universal Approximation Theorem.
  *   **Activation Functions:** Sigmoid, Tanh, and ReLU (why ReLU is the modern standard).
  *   **Loss Functions:** MSE (Regression) vs. Cross-Entropy / Log-Loss (Classification).
  *   **Backpropagation:**
    *   **Math:** Using the Chain Rule to calculate gradients from the output back to the input. (Know how to trace a simple path and multiply the local derivatives!).
    *   "Multiple Paths" rule (summing gradients if a node splits).
  *   **Gradient Crises:**
    *   *Vanishing Gradient:* Gradients shrink to zero due to multiplying small derivatives (like Sigmoid's max slope of 0.25).
    *   *Exploding Gradient:* Gradients grow to infinity; fixed via Gradient Clipping.
### **5. Convolutional Neural Networks (CNNs) (Lecture 16)**
  *   **Why CNNs?** Standard MLPs cause a "Parameter Explosion" on images. CNNs use **Weight Sharing** (Filters) to preserve spatial features with tiny parameter counts.
  *   **The Mechanics:**
    *   Convolution (sliding the filter and taking the dot product).
    *   **Math:** The Output Size Formula: $\frac{\text{Input} - \text{Filter} + 2(\text{Padding})}{\text{Stride}} + 1$.
    *   **Math:** Counting Parameters: $(\text{Filter Height} \times \text{Width} \times \text{Input Channels} + 1 \text{ Bias}) \times \text{Number of Filters}$.
  *   **Pooling Layers:** Max Pooling vs. Average Pooling (reduces spatial dimensions, controls overfitting, creates translation invariance).
  *   **Transfer Learning (Fine-tuning):** When to freeze layers vs. train layers based on dataset size and similarity.
### **6. Recurrent Neural Networks \& LSTMs (Lecture 17)**
  *   **Sequential Data:** Why traditional NNs fail (fixed sizes, no memory).
  *   **RNNs:** Processing data through time using shared weight matrices. Trained using BPTT (Backprop Through Time).
  *   *The Crisis:* Standard RNNs suffer from Vanishing Gradients over long sequences (short-term memory only).
  *   **LSTMs (The Solution):**
    *   **Cell State ($C_t$):** The long-term memory "conveyor belt".
    *   **Gates:** Forget Gate, Input Gate, Output Gate. (Use Sigmoid to act as 0-to-1 valves).
    *   *Why it works:* The Cell State uses addition instead of multiplication, allowing the gradient to flow backward via the Forget Gate without vanishing.
  *   **Architectures:** Many-to-One (Sentiment), One-to-Many (Captioning), Many-to-Many (Translation), and Bi-Directional LSTMs (BiLSTMs).
### **7. Self-Supervised Learning (SSL) (Lecture 18)**
  *   **The Motivation:** Human labels are too expensive. SSL uses unlabelled data to create its own supervisory signals (pseudo-labels).
  *   **Pretext vs. Downstream Tasks:** 
    *   *Pretext:* Fake puzzles to learn "visual commonsense" (Predicting rotation, solving Jigsaw, Inpainting).
    *   *Downstream:* The actual task (evaluated using Linear Probing/Classification).
  *   **Contrastive Learning:**
    *   *Goal:* Attract Positive pairs (augmented views of the same image), Repel Negative pairs (different images).
    *   *SimCLR:* Uses data augmentation and the InfoNCE loss function; relies on massive batch sizes to get enough negative samples.
    *   *BYOL:* Uses a Teacher/Student network to learn *without* negative samples (Teacher updates via moving average).
  *   **Multimodal & Text:** CLIP (Images + Text), BERT (Masked Language Modeling and Next Sentence Prediction).
### **8. The Transformer (Lecture 19)**
  *   **The Shift:** Processes entire sequences simultaneously in parallel (unlike LSTMs), allowing for massive scalability.
  *   **Embeddings & Positional Encoding:** Words are dense vectors. Because processing is parallel, we must *add* sine/cosine waves to the vectors so the model knows "where the word is."
  *   **Self-Attention:** 
    *   **Math:** $\text{Softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right) V$.
    *   *Queries (What I want), Keys (What I have), Values (My meaning).*
  *   **Multi-Head Attention:** Multiple parallel attention mechanisms learning different rules (e.g., syntax vs. coreference).
  *   **The Infrastructure:** Add & Norm (Residual connections to prevent vanishing gradients, LayerNorm for stability).
  *   **Masking:** Look-Ahead mask in the Decoder prevents it from "cheating" by seeing future words during training.
### **9. Outlier / Anomaly Detection (Lecture 20)**
  *   **Definitions:** Noise (random) vs. Anomaly (meaningful deviation). Point, Contextual, and Collective anomalies.
  *   **Statistical Methods:** Z-Score and IQR. (Fails on non-Gaussian or high-D data).
  *   **Density-Based Methods:** Local Outlier Factor (**LOF**). 
    *   *Math Concept:* Compares the local density (LRD) of a point to the density of its neighbors. LOF $\gg 1$ = Outlier.
  *   **Model/Reconstruction Methods:** 
    *   One-Class SVM (Drawing the tightest sphere around normal data).
    *   Autoencoders (If a model trained on normal data fails to reconstruct an input, the high Reconstruction Error flags it as an anomaly).
  *   **Temporal Anomalies:** Moving averages, or using an LSTM to predict the next frame/value (high prediction error = anomaly).
  *   **Evaluation:** Cannot use Accuracy due to massive class imbalance. Must use Precision, Recall, F1, or PR-AUC.
  
  ---