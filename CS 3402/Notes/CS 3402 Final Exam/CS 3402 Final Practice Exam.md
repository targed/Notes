# **CS 3402: Intro to Data Science - Comprehensive Final Practice Exam**
**Total Points: 100**
- ### **Part I: Data Science Foundations & Stats (15 Points)**
  
  **1. (True/False) [2 pts]**
  When calculating descriptive statistics for a dataset representing city household incomes, the Mean is preferred over the Median because the Mean is mathematically robust and ignores extreme outliers like billionaires.
  
  **2. (Multiple Choice) [4 pts]**
  You train a neural network and find that it achieves **98% accuracy on the Training set**, but only **55% accuracy on the Testing set**. Which of the following best describes this model's state?
  A) High Bias (Underfitting)
  B) High Variance (Overfitting)
  C) Optimal Fit
  D) The Degradation Problem
  
  **3. (Multiple Choice) [4 pts]**
  A company wants to group its customers into 5 distinct "behavior tribes" based on purchasing habits, but they do not have any pre-existing labels or categories to train the model on. What type of machine learning task is this?
  A) Supervised Learning (Classification)
  B) Supervised Learning (Regression)
  C) Unsupervised Learning (Clustering)
  D) Natural Language Processing
  
  **4. (Short Answer) [5 pts]**
  Explain the fundamental difference between a **Probability Mass Function (PMF)** and a **Probability Density Function (PDF)** in terms of the types of data they measure.
  
  ---
- ### **Part II: Network Analysis (15 Points)**
  
  **5. (True/False) [2 pts]**
  In a directed graph, if Node A has a directed edge pointing to Node B, it guarantees that Node B also has a directed edge pointing back to Node A.
  
  **6. (Calculation: Graph Metrics) [8 pts]**
  You are analyzing an **unweighted, undirected** social network graph with 4 nodes ($A, B, C, D$).
  The connections are:
  *   **A** is connected to **B**, **C**, and **D**.
  *   **B** is connected to **A** and **C**.
  *   **C** is connected to **A**, **B**, and **D**.
  *   **D** is connected to **A** and **C**.
  
  **(a)** What is the degree of Node C? 
  **(b)** Calculate the **Degree Centrality** of Node A. *(Show your math/fraction).*
  **(c)** Calculate the **Degree Centrality** of Node B. *(Show your math/fraction).*
  
  **7. (Short Answer) [5 pts]**
  Briefly define the concept of a **"Small-World Network."**
  
  ---
- ### **Part III: NLP & Text Representation (15 Points)**
  
  **8. (True/False) [2 pts]**
  Modern Large Language Models (LLMs) use Subword Tokenization because it drastically reduces the vocabulary size and allows the model to handle rare or unseen words (OOV) effectively.
  
  **9. (Multiple Choice) [4 pts]**
  In the **TF-IDF** text representation method, what happens to words like "the" or "and" that appear in almost every single document in your dataset?
  A) They are assigned the highest numerical value in the vector.
  B) They are heavily down-weighted (penalized) by the Inverse Document Frequency component.
  C) They are automatically converted into `<UNK>` tokens.
  D) They are multiplied by their Z-score.
  
  **10. (Calculation: Bag of Words) [9 pts]**
  You have the following two sentences (already cleaned and lowercased):
  *   **S1:** `"machine learning is fun"`
  *   **S2:** `"deep learning is machine learning"`
  
  **(a)** Build the complete **Bag-of-Words vocabulary list** for this dataset.
  **(b)** Represent sentence **S1** as a mathematical vector based on your vocabulary.
  **(c)** Represent sentence **S2** as a mathematical vector based on your vocabulary.
  
  ---
- ### **Part IV: Transformers & Self-Attention (20 Points)**
  
  **11. (True/False) [2 pts]**
  Unlike old RNNs, Transformers process input sequences in parallel. 
  
  **12. (Multiple Choice) [4 pts]**
  What is the primary purpose of using **Multi-Head Attention** instead of a single self-attention mechanism?
  A) It allows the model to process images and text simultaneously.
  B) It gives the model information about the sequential order of the tokens.
  C) It allows the model to attend to multiple different linguistic relationships (e.g., grammar, coreference, sentiment) at the exact same time.
  D) It reduces the spatial dimensions of the input embeddings.
  
  **13. (Short Answer) [6 pts]**
  Why are **Positional Embeddings (Encodings)** strictly required in the Transformer architecture, whereas they are not required in a CNN? 
  
  **14. (Calculation: Self-Attention) [8 pts]**
  You are calculating self-attention for a token. You are given the Query for the token, and the Keys and Values for two tokens in the sequence.
  *   $Q = [1, 2]$
  *   $K_1 =[1, 0]$ and $K_2 = [0, 1]$
  *   $V_1 =[3, 0]$ and $V_2 = [0, 4]$
  
  **(a)** Compute the dot products (raw attention scores) for $Q \cdot K_1$ and $Q \cdot K_2$.
  **(b)** Apply the softmax function to obtain the attention weights ($\alpha_1$ and $\alpha_2$). *(You may leave your answers in exponential form, e.g., $e^x$).*
  **(c)** Calculate the final output vector.
  
  ---
- ### **Part V: CNNs, ResNets & Vision Transformers (20 Points)**
  
  **15. (True/False) [2 pts]**
  In a CNN, max-pooling layers are used to increase the number of channels (depth) of the feature map.
  
  **16. (Multiple Choice) [4 pts]**
  How does a **Residual Network (ResNet)** solve the "Degradation Problem" associated with extremely deep neural networks?
  A) It uses subword tokenization to shrink the image.
  B) It implements a skip connection $H(x) = F(x) + x$, creating a shortcut that mitigates the vanishing gradient problem.
  C) It replaces all convolutions with global self-attention.
  D) It changes the activation function from ReLU to Softmax.
  
  **17. (Calculation: ViT Patch Math) [8 pts]**
  An RGB image of size **$128 \times 128$** is fed into a Vision Transformer (ViT) that divides the image into **$32 \times 32$ patches**.
  **(a)** How many total patches are created?
  **(b)** If each patch is flattened, what is the dimension (length) of each patch vector?
  **(c)** What is the total sequence length fed into the Transformer? *(Don't forget the special token!)*
  
  **18. (Calculation: CNN Feature Maps) [6 pts]**
  An image of size **$64 \times 64$** is passed through a convolutional layer with:
  *   **Filter size (F):** $3 \times 3$
  *   **Stride (S):** $1$
  *   **Padding (P):** $1$
  
  **(a)** Compute the output width and height of the feature map. 
  **(b)** Assuming this is an RGB image (3 input channels), how many parameters (weights + 1 bias) are in a **single filter**?
  
  ---
- ### **Part VI: Deep Learning Math & Gradient Descent (15 Points)**
  
  **19. (Calculation: Artificial Neuron) [5 pts]**
  You have an artificial neuron with the equation: $y = \text{ReLU}(w_1x_1 + w_2x_2 + b)$.
  Inputs: $x_1 = -1$, $\; x_2 = 1$
  Weights: $w_1 = 2$, $\; w_2 = 1$
  Bias: $b = 0$
  **(a)** Calculate the raw linear output ($z$) before activation.
  **(b)** What is the final output ($y$) after applying the ReLU function?
  
  **20. (Calculation: Gradient Descent Update) [10 pts]**
  You are training a neural network: $\hat{y} = w_1x_1 + w_2x_2$.
  Loss function: $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$.
  *   **Learning Rate ($\eta$):** $0.1$
  *   **Initial Weights:** $w_1 = 0.5$, $\; w_2 = 0.5$
  *   **Training Sample:** $x_1 = 4$, $\; x_2 = 2$
  *   **Target ($y$):** $7$
  
  Perform one step of Gradient Descent. Calculate:
  **(a)** The prediction ($\hat{y}$)
  **(b)** The Loss ($\mathcal{L}$)
  **(c)** The gradients for $w_1$ and $w_2$
  **(d)** The new, updated weights ($w_1^{new}$ and $w_2^{new}$)
  
  ***
  ***
  *(Stop here! Complete the exam before scrolling down to the Answer Key)*
  ***
  ***
  
  <br><br><br><br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & GRADING RUBRIC**
  
  **Part I: Data Science Foundations & Stats**
  1. **False.** (The Median is robust and ignores outliers. The Mean is highly sensitive to outliers). (2 pts)
  2. **B - High Variance (Overfitting).** (The model memorized the training data but failed to generalize). (4 pts)
  3. **C - Unsupervised Learning (Clustering).** (No labels provided). (4 pts)
  4. **Answer:** A PMF is used for **discrete** random variables (where probabilities sum to 1). A PDF is used for **continuous** random variables (where the area under the curve integrates to 1). (5 pts)
  
  **Part II: Network Analysis**
  5. **False.** (Directed edges are asymmetric. A $\rightarrow$ B does not guarantee B $\rightarrow$ A). (2 pts)
  6. **(a) Degree of C:** **3** (Connected to A, B, D). (2 pts)
   **(b) Centrality of A:** Degree is 3. Total nodes ($n$) = 4. Formula: $3 / (4-1) = 3 / 3 = \mathbf{1.0}$. (3 pts)
   **(c) Centrality of B:** Degree is 2 (A, C). Formula: $2 / (4-1) = \mathbf{0.67}$ (or 2/3). (3 pts)
  7. **Answer:** A network where most nodes are not direct neighbors, but almost every node can be reached from any other node in a very small number of steps. (5 pts)
  
  **Part III: NLP & Text Representation**
  8. **True.** (2 pts)
  9. **B.** (IDF penalizes highly frequent words across documents). (4 pts)
  10. **(a) Vocabulary:** `['machine', 'learning', 'is', 'fun', 'deep']` *(Order may vary, but must have these 5 unique words).* (3 pts)
    **(b) S1 Vector:** **`[1, 1, 1, 1, 0]`** (3 pts)
    **(c) S2 Vector:** **`[2, 2, 1, 0, 1]`** (3 pts)
  
  **Part IV: Transformers & Self-Attention**
  11. **True.** (2 pts)
  12. **C.** (Allows the model to attend to different relationship patterns simultaneously). (4 pts)
  13. **Answer:** CNNs encode spatial information implicitly through sliding local filters. Transformers process all tokens globally and in parallel, so they have no inherent understanding of order. Positional embeddings must be added to tell the model *where* the data came from. (6 pts)
  14. **(a) Dot Products:** $Q \cdot K_1 = (1 \times 1) + (2 \times 0) = \mathbf{1}$. $Q \cdot K_2 = (1 \times 0) + (2 \times 1) = \mathbf{2}$. (2 pts)
    **(b) Softmax:** Denominator sum = $e^1 + e^2$. $\alpha_1 = \mathbf{\frac{e^1}{e^1 + e^2}}$, $\alpha_2 = \mathbf{\frac{e^2}{e^1 + e^2}}$. (3 pts)
    **(c) Final Vector:** $(\frac{e^1}{e^1+e^2} \times [3,0]) + (\frac{e^2}{e^1+e^2} \times [0,4]) = \mathbf{\left[ \frac{3e^1}{e^1+e^2} , \frac{4e^2}{e^1+e^2} \right]}$. (3 pts)
  
  **Part V: CNNs, ResNets & Vision Transformers**
  15. **False.** (Pooling reduces spatial dimensions width/height. Filters increase channels). (2 pts)
  16. **B.** (The skip connection $H(x) = F(x) + x$). (4 pts)
  17. **(a) Patches:** $(128 / 32)^2 = 4^2 = \mathbf{16 \text{ patches}}$. (3 pts)
    **(b) Vector Dimension:** $32 \times 32 \times 3 \text{ (RGB channels)} = \mathbf{3072}$. (3 pts)
    **(c) Sequence Length:** 16 patches + 1 `[CLS]` token = $\mathbf{17}$. (2 pts)
  18. **(a) Output Size:** $\frac{64 - 3 + 2(1)}{1} + 1 = 63 + 1 = \mathbf{64 \times 64}$. (3 pts)
    **(b) Parameters:** $(3 \times 3 \times 3) + 1 \text{ bias} = 27 + 1 = \mathbf{28}$. (3 pts)
  
  **Part VI: Deep Learning Math & Gradient Descent**
  19. **(a) Linear Output ($z$):** $(2 \times -1) + (1 \times 1) + 0 = -2 + 1 = \mathbf{-1}$. (2 pts)
    **(b) ReLU Output ($y$):** $\max(0, -1) = \mathbf{0}$. (3 pts)
  20. **(a) Prediction:** $\hat{y} = (0.5 \times 4) + (0.5 \times 2) = 2 + 1 = \mathbf{3}$. (2 pts)
    **(b) Loss:** Error is $(3 - 7) = -4$. Loss = $\frac{1}{2}(-4)^2 = \frac{1}{2}(16) = \mathbf{8}$. (2 pts)
    **(c) Gradients:** (Error $\times$ Input). 
    *   $dw_1 = -4 \times 4 = \mathbf{-16}$
    *   $dw_2 = -4 \times 2 = \mathbf{-8}$ (3 pts)
    **(d) New Weights:**
    *   $w_1 = 0.5 - (0.1 \times -16) \rightarrow 0.5 + 1.6 = \mathbf{2.1}$
    *   $w_2 = 0.5 - (0.1 \times -8) \rightarrow 0.5 + 0.8 = \mathbf{1.3}$ 
    Final weights: **(2.1, 1.3)**. (3 pts)
  
  ***
- ---
- ---
- ---
- # **CS 3402: Intro to Data Science - Comprehensive Final Practice Exam (Version E)**
  **Time Limit: 90 Minutes | Total Points: 100**
- ### **Part I: Statistics, Probability & Data Science (20 Points)**
  
  **1. (True/False)[2 pts]**
  In Kernel Density Estimation (KDE), a small Gaussian "bump" is placed over every single data point, and these are summed together to estimate a smooth Probability Density Function (PDF) for unknown data distributions.
  
  **2. (Multiple Choice) [4 pts]**
  A data scientist computes the **Covariance** between two variables (Hours Studied and Test Score) and gets a result of `1450.0`. They then compute the **Pearson Correlation ($\rho$)**. Which of the following is a possible value for the Pearson Correlation?
  A) 1450.0
  B) 14.5
  C) 0.85
  D) -1.2
  
  **3. (Short Answer)[6 pts]**
  In the "Adult" dataset case study (Lecture 5), you formulate a Null Hypothesis ($H_0$) that "men and women have the exact same probability of earning >$50k." After running a statistical Z-test, you get a **p-value of 0.001**. 
  Do you *accept* or *reject* the Null Hypothesis, and what does this p-value mathematically mean?
  
  **4. (Calculation: Outliers) [8 pts]**
  You have a normally distributed dataset of house prices where the **Mean ($\mu$) is \$200,000** and the **Standard Deviation ($\sigma$) is \$50,000**.
  **(a)** According to the empirical rule (3 standard deviations) discussed in class, what is the maximum price a house can be before it is mathematically flagged as an **outlier**?
  **(b)** You find a house priced at \$400,000. Calculate its Z-Score. Is it an outlier?
  
  ---
- ### **Part II: Network Analysis (15 Points)**
  
  **5. (True/False) [2 pts]**
  In an undirected graph, the "Shortest Path" between Node A and Node B is always the path that visits the fewest number of nodes, even if the graph is heavily weighted.
  
  **6. (Calculation: Weighted Graphs) [8 pts]**
  You are analyzing a **Weighted, Undirected** network representing trade between 4 cities ($A, B, C, D$). The weights represent millions of dollars in trade.
  *   $A$ trades with $B$ (Weight = 5)
  *   $A$ trades with $C$ (Weight = 10)
  *   $B$ trades with $C$ (Weight = 2)
  *   $C$ trades with $D$ (Weight = 8)
  
  **(a)** What is the standard **Degree** of City C?
  **(b)** What is the **Weighted Degree** (sum of connected edge weights) of City C?
  **(c)** What is the standard **Degree Centrality** of City A? *(Remember to divide by $n-1$)*.
  
  **7. (Short Answer) [5 pts]**
  In graph theory, what is a **Connected Component**?
  
  ---
- ### **Part III: NLP & Transformers (25 Points)**
  
  **8. (True/False) [2 pts]**
  In the Transformer architecture, the "Add & Norm" block utilizes skip/residual connections to help prevent the vanishing gradient problem.
  
  **9. (Multiple Choice)[4 pts]**
  The computational time complexity of the Self-Attention mechanism is $O(T^2 \times d_{model})$. If you are feeding a document into a Transformer, what does the **$T$** represent?
  A) The total training time in seconds.
  B) The number of tokens in the sequence.
  C) The number of attention heads in the model.
  D) The dimensions of the embedding vector.
  
  **10. (Calculation: L2 Normalization) [6 pts]**
  In NLP, Bag-of-Words vectors are often normalized using **L2 Normalization** so that long documents and short documents can be compared on the same scale (between 0 and 1).
  You have a Term Frequency vector for a short sentence: **`[3, 4]`**.
  **(a)** Calculate the L2 Norm (the Euclidean length) of this vector. *(Formula: $\sqrt{\sum x_i^2}$)*
  **(b)** Divide the vector by your answer in (a) to find the **Normalized Vector**.
  
  **11. (Short Answer) [7 pts]**
  Inside a Transformer Encoder block, the data passes through **Self-Attention** and then through a **Feed-Forward Network (MLP)**. Briefly explain the different jobs of these two components. *(What does Self-Attention do that the Feed-Forward layer doesn't?)*
  
  **12. (Short Answer) [6 pts]**
  Why are modern Large Language Models (LLMs) trained using **Subword Tokenization** instead of simply splitting text by spaces (Word-level tokenization)? Give two specific reasons.
  
  ---
- ### **Part IV: CNNs, ViTs & Computer Vision (20 Points)**
  
  **13. (True/False) [2 pts]**
  Average-Pooling is generally preferred over Max-Pooling because it strictly preserves the sharpest, most prominent features in an image.
  
  **14. (Multiple Choice) [4 pts]**
  In a CNN, parameter sharing and sparse connectivity are used to:
  A) Flatten an image into a 1D sequence of patches.
  B) Extract local features while drastically reducing the number of weights the model must learn compared to an MLP.
  C) Add positional embeddings to the image.
  D) Generate text captions for the image.
  
  **15. (Calculation: Convolution with Stride) [8 pts]**
  An image of size **$33 \times 33$** is passed through a convolutional layer with:
  *   **Filter size (F):** $3 \times 3$
  *   **Stride (S):** $2$ *(Notice this is 2!)*
  *   **Padding (P):** $0$
  
  **(a)** Compute the output width and height of the feature map. *(Formula: $\frac{N - F + 2P}{S} + 1$)*
  **(b)** If the filter size is $3 \times 3$ and it is operating on an RGB image (3 channels), how many parameters are in a single filter (including the 1 bias term)?
  
  **16. (Short Answer) [6 pts]**
  In a Vision Transformer (ViT), the image is chopped into patches (e.g., $16 \times 16$), flattened, and processed sequentially. Why do we prepend a special **`[CLS]` (Classification) token** to the very beginning of this sequence before it enters the Transformer?
  
  ---
- ### **Part V: Machine Learning & Gradient Descent (20 Points)**
  
  **17. (Multiple Choice) [4 pts]**
  Based on your Group Project 3 readings, what typically happens to a highly complex model (like a Deep Neural Network) if you train it on a very **small** dataset?
  A) It suffers from high bias (underfitting) and fails to learn the patterns.
  B) It suffers from high variance (overfitting) and memorizes the noise in the training data.
  C) It achieves an optimal fit instantly.
  D) It converts the data into a sparse matrix.
  
  **18. (Short Answer) [6 pts]**
  During **Backpropagation**, Neural Networks use the **Chain Rule** from calculus. In simple terms, why is the chain rule necessary for a multi-layer neural network? 
  
  **19. (Calculation: Gradient Descent with a Zero-Input) [10 pts]**
  You are training a simple neural network: $\hat{y} = w_1x_1 + w_2x_2$.
  Loss function: $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$.
  
  *   **Learning Rate ($\eta$):** $0.1$
  *   **Initial Weights:** $w_1 = 2.0$, $\; w_2 = 1.0$
  *   **Training Sample:** $x_1 = 0$, $\; x_2 = 3$   *(Notice $x_1$ is zero!)*
  *   **Target ($y$):** $5$
  
  Perform one step of Gradient Descent. Calculate:
  **(a)** The prediction ($\hat{y}$)
  **(b)** The Loss ($\mathcal{L}$)
  **(c)** The gradients for $w_1$ and $w_2$
  **(d)** The new, updated weights ($w_1^{new}$ and $w_2^{new}$)
  
  ***
  ***
  *(Stop here! Complete the exam before scrolling down to the Answer Key)*
  ***
  ***
  
  <br><br><br><br><br><br><br><br><br><br><br><br>
- ### **ANSWER KEY & GRADING RUBRIC**
  
  **Part I: Statistics, Probability & Data Science**
  1. **True.** (This is the exact definition of KDE). (2 pts)
  2. **C (0.85).** (Pearson Correlation must always be between -1 and 1. Covariance can be any large number, but correlation normalizes it). (4 pts)
  3. **Answer:** We **reject** the Null Hypothesis. A p-value of 0.001 means there is only a 0.1% probability that the observed income difference between men and women happened by random chance. Therefore, the difference is statistically significant. (6 pts)
  4. **(a) Max Price:** $\mu + 3\sigma -> 200,000 + 3(50,000) = \mathbf{350,000}$. (4 pts)
   **(b) Z-Score:** $Z = \frac{400,000 - 200,000}{50,000} = \frac{200,000}{50,000} = \mathbf{4.0}$. **Yes**, it is an outlier because its Z-score is greater than 3. (4 pts)
  
  **Part II: Network Analysis**
  5. **False.** (In a weighted graph, the "shortest path" is the path with the lowest sum of weights, which might actually require hopping through *more* nodes to stay on the "faster/cheaper" edges). (2 pts)
  6. **(a) Degree of C:** **3** (It has edges to A, B, and D). (2 pts)
   **(b) Weighted Degree of C:** $10 + 2 + 8 = \mathbf{20}$. (3 pts)
   **(c) Centrality of A:** Degree is 2. Total nodes $n=4$. Formula: $2 / (4-1) = \mathbf{0.67}$. (3 pts)
  7. **Answer:** A connected component is a maximal subgraph (an "island" in the network) where every pair of nodes has a path to each other. (5 pts)
  
  **Part III: NLP & Transformers**
  8. **True.** (2 pts)
  9. **B.** ($T$ stands for the number of Tokens in the sequence). (4 pts)
  10. **(a) L2 Norm:** $\sqrt{3^2 + 4^2} = \sqrt{9 + 16} = \sqrt{25} = \mathbf{5}$. (3 pts)
    **(b) Normalized Vector:** $[3/5, 4/5] = \mathbf{[0.6, 0.8]}$. (3 pts)
  11. **Answer:** **Self-Attention** allows words to look at *other words* in the sentence to gather global context and relationships. The **Feed-Forward Network (MLP)** processes each word's new, context-rich vector *independently* to extract deeper non-linear patterns. (7 pts)
  12. **Answer:** 1) It vastly reduces the total vocabulary size (saving RAM and compute). 2) It prevents Out-Of-Vocabulary (OOV) errors by allowing the model to process completely new or misspelled words by breaking them down into known syllable chunks. (6 pts)
  
  **Part IV: CNNs, ViTs & Computer Vision**
  13. **False.** (Max-Pooling preserves the sharpest/most prominent features. Average-pooling dilutes them). (2 pts)
  14. **B.** (They extract local features while vastly reducing parameters compared to MLPs). (4 pts)
  15. **(a) Output Size:** $\frac{33 - 3 + 0}{2} + 1 = \frac{30}{2} + 1 = 15 + 1 = \mathbf{16 \times 16}$. (5 pts)
    **(b) Parameters:** $(3 \times 3 \times 3) + 1 = 27 + 1 = \mathbf{28}$. (3 pts)
  16. **Answer:** Because the patches interact with each other via Self-Attention, we need a single vector at the very end of the network to absorb the global context of the entire image. The `[CLS]` token acts as this "sponge," and its final output vector is passed to the MLP to make the final classification (e.g., Cat vs. Dog). (6 pts)
  
  **Part V: Machine Learning & Gradient Descent**
  17. **B.** (High variance/overfitting. Complex models memorize small datasets). (4 pts)
  18. **Answer:** The Chain Rule allows us to calculate how a weight in the very first layer affects the final output Loss. It passes the "blame" backward layer-by-layer by multiplying the local derivatives together. (6 pts)
  19. **(a) Prediction:** $\hat{y} = (2.0 \times 0) + (1.0 \times 3) = 0 + 3 = \mathbf{3}$. (2 pts)
    **(b) Loss:** Error is $(3 - 5) = -2$. Loss = $\frac{1}{2}(-2)^2 = \frac{1}{2}(4) = \mathbf{2}$. (2 pts)
    **(c) Gradients:** (Error $\times$ Input). 
    *   $dw_1 = -2 \times 0 = \mathbf{0}$
    *   $dw_2 = -2 \times 3 = \mathbf{-6}$ (3 pts)
    **(d) New Weights:**
    *   $w_1 = 2.0 - (0.1 \times 0) = \mathbf{2.0}$ *(Weight 1 didn't change because its input was 0!)*
    *   $w_2 = 1.0 - (0.1 \times -6) \rightarrow 1.0 - (-0.6) \rightarrow 1.0 + 0.6 = \mathbf{1.6}$ 
    Final weights: **(2.0, 1.6)**. (3 pts)
  
  ***