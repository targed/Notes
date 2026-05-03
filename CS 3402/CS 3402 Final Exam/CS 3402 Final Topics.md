### **DOMAIN 1: Data Science Foundations & Tools**
*The basics of the software engineering side of data science.*
*   **The AI Hierarchy:** AI (broadest) $\rightarrow$ Machine Learning (learning from data) $\rightarrow$ Deep Learning (neural networks).
*   **The Data Science Venn Diagram:** Hacking Skills + Math/Stats + Domain Expertise. (Remember: Hacking + Expertise without Math = *The Danger Zone*).
*   **Environment Management:** The purpose of Virtual Environments (`.venv` or Conda) to avoid dependency hell.
*   **IDEs vs. Text Editors:** Why we use VS Code/PyCharm (linting, debugging, execution) instead of basic text editors.
*   **Core Libraries:**
  *   `NumPy`: Fast numerical arrays and math.
  *   `Pandas`: Structured tabular data (DataFrames).
  *   `Matplotlib`: Static visualizations and plotting.
  *   `Scikit-learn`: Traditional ML models and data splitting.
  *   `PyTorch`: Deep learning, GPU acceleration (CUDA), and Autograd (calculating gradients).
- ### **DOMAIN 2: Descriptive Statistics & Probability**
  *Understanding the shape and spread of data.*
  *   **Populations vs. Samples:** Population is the whole group; Sample is the measured subset.
  *   **Central Tendency:**
    *   **Mean:** The average. Sensitive to outliers.
    *   **Median:** The middle value. **Robust** to outliers (e.g., used for house prices).
  *   **Dispersion/Spread:**
    *   **Variance ($\sigma^2$):** How far data points are spread from the mean.
    *   **Standard Deviation ($\sigma$):** The square root of variance (brings it back to original units).
  *   **Probability Functions:**
    *   **PMF (Probability Mass Function):** For *discrete* variables. Probabilities must sum to 1.
    *   **PDF (Probability Density Function):** For *continuous* variables. Area under the curve = 1.
    *   **CDF (Cumulative Distribution Function):** Probability that a value is *less than or equal to* $X$.
  *   **Distributions & Shapes:** Normal/Gaussian (bell curve), Outliers, Skewness (Positive/Right Skew vs. Negative/Left Skew).
  *   **Bi-variate Stats:** 
    *   *Covariance:* How two variables move together (depends on units).
    *   *Pearson Correlation ($\rho$):* Normalized covariance. Always between **-1 and 1**.
- ### **DOMAIN 3: Machine Learning Fundamentals**
  *How computers learn patterns from data.*
  *   **Types of Learning:**
    *   **Supervised Learning:** Requires *labeled* data (an answer key).
        *   *Classification:* Predicting discrete categories (e.g., Dog vs. Cat).
        *   *Regression:* Predicting continuous numbers (e.g., Stock Price).
    *   **Unsupervised Learning:** Uses *unlabeled* data (e.g., Clustering).
  *   **Model Evaluation:**
    *   Training vs. Testing data.
    *   **Overfitting (High Variance):** Memorizes training data, fails on test data.
    *   **Underfitting (High Bias):** Model is too simple to learn the pattern.
    *   **Metrics:** Accuracy, F1-Score (better for imbalanced data/generative text).
  *   **Data Normalization:** Why we use **Z-Scores**. (Centers data around 0 so gradients don't explode/vanish during training).
- ### **DOMAIN 4: Deep Neural Networks (DNNs) & Gradient Descent**
  *The engine of Deep Learning.*
  *   **The Artificial Neuron:** Applies a linear transformation ($z = wx + b$) followed by a non-linear activation function.
  *   **Activation Functions:**
    *   Why we need them: They introduce **non-linearity**. Without them, a 100-layer network collapses into a basic linear equation.
    *   **ReLU:** $\max(0, x)$. Outputs $0$ for negative numbers, $x$ for positive numbers.
    *   **Softmax:** Converts raw scores into probabilities that sum to exactly 1.
  *   **Gradient Descent & Backpropagation:**
    *   *Loss Function:* Measures the error (e.g., Mean Squared Error: $\frac{1}{2}(\hat{y} - y)^2$).
    *   *Chain Rule:* Used during backpropagation to find the slope (gradient).
    *   *Update Rule:* $w_{new} = w_{old} - \eta \frac{\partial L}{\partial w}$. (Subtracting moves us *downhill* to minimize loss. $\eta$ is the learning rate).
- ### **DOMAIN 5: Network Analysis (Graph Theory)**
  *Modeling relationships between entities.*
  *   **Components:** Nodes (vertices) and Edges (links).
  *   **Types of Graphs:** 
    *   Directed (one-way arrows) vs. Undirected (two-way relationships).
    *   Weighted (edges have values/strength) vs. Unweighted.
  *   **Degree:** The number of edges connected to a node (In-degree vs. Out-degree).
  *   **Degree Centrality:** Measures local importance based on raw connection count.
  *   **Topology:** Connected components, complete graphs, and Small-World Networks.
- ### **DOMAIN 6: Natural Language Processing (NLP)**
  *Preparing text for machines.*
  *   **The Pipeline:** Text $\rightarrow$ Clean $\rightarrow$ Tokenize $\rightarrow$ Represent $\rightarrow$ Model.
  *   **Tokenization:**
    *   *Word-level:* Leads to huge vocabularies and crashes on rare/new words (Out of Vocabulary).
    *   *Subword-level (BPE/WordPiece):* Breaks words into chunks. **Preferred for LLMs** because it reduces vocabulary size and easily handles unseen words.
    *   *Output:* Converts text into Tokens and numerical **IDs**.
  *   **Text Representation:**
    *   *Bag-of-Words (BoW):* Counts word frequency. **Fatal Flaw:** Ignores word order.
    *   *TF-IDF:* Down-weights common words ("the") and highlights important rare words.
    *   *Dense Embeddings:* Converts tokens into arrays of decimals that capture actual semantic meaning and context.
- ### **DOMAIN 7: The Transformer Architecture**
  *The architecture behind ChatGPT and modern AI.*
  *   **Parallelization:** Transformers process input sequences in parallel, not sequentially like old RNNs.
  *   **Positional Encodings:** Because processing is parallel, positional embeddings *must* be injected so the model knows the sequential order of the words.
  *   **Self-Attention:** 
    *   *Intuition:* Allows each token to directly attend to **all other tokens** to gather global context (e.g., figuring out what a pronoun refers to).
    *   *Queries (Q), Keys (K), Values (V):* Derived from the same input. Q determines what to look for, K provides the label to match, V is the content pulled to compute the output.
  *   **Multi-Head Attention:** Allows the model to capture multiple different linguistic relationships (grammar, sentiment, syntax) simultaneously.
  *   **Add & Norm:** Residual/Skip connections that mitigate the vanishing gradient problem.
- ### **DOMAIN 8: Computer Vision (CNNs vs. ViTs)**
  *How computers process images.*
  *   **Convolutional Neural Networks (CNNs):**
    *   *Kernel/Filter:* Slides across the image to extract **local spatial features** (edges, shapes).
    *   *Parameter Sharing:* Filters share weights across the whole image, drastically reducing the number of parameters compared to MLPs.
  *   **Pooling Layers:**
    *   Down-samples the image. Directly shrinks the height and width of feature maps to save memory and extract dominant features (Max Pooling).
  *   **ResNet (Residual Networks):**
    *   Solves the "Degradation Problem" where very deep networks fail. Uses $F(x) + x$ skip connections to help gradients flow backward safely.
  *   **Vision Transformers (ViTs):**
    *   *Mechanism:* ViTs do **not** use convolution layers. They split the image into grid **patches**, flatten them into 1D vectors, and treat them like words in a sentence.
    *   *Difference from CNNs:* CNNs encode spatial locality locally through filters. ViTs capture relationships globally via self-attention from layer 1, relying on positional embeddings to know where the patch came from.
  
  ---
  ---
- ### **THE "MUST-KNOW" MANUAL CALCULATIONS**
  *Your final review document confirms these specific math problems will be on the test.*
  
  **1. CNN Output Dimension Formula**
  *   *Formula:* $\text{Output Size} = \frac{W - K + 2P}{S} + 1$
  *   (Where $W$ = Input width, $K$ = Kernel size, $P$ = Padding, $S$ = Stride).
  *   *Example:* Input 64x64, Kernel 3, Stride 1, Padding 1 $\rightarrow \frac{64 - 3 + 2(1)}{1} + 1 = \mathbf{64 \times 64}$.
  
  **2. Max Pooling Dimension Formula**
  *   *Formula:* Same as above, but $P$ is usually 0. $\frac{W - K}{S} + 1$.
  *   *Example:* Input 64x64, Pool Size 2, Stride 2 $\rightarrow \frac{64 - 2}{2} + 1 = 31 + 1 = \mathbf{32 \times 32}$.
  
  **3. Artificial Neuron with ReLU**
  *   *Equation:* $y = \text{ReLU}(w_1x_1 + w_2x_2 + b)$
  *   If $z$ (the inside math) is positive, output $z$. If $z \le 0$, output $0$.
  *   *Inequality Example:* "For what values is the output 0?" Set $w_1x_1 + w_2x_2 + b \le 0$ and solve.
  
  **4. Vision Transformer Patch Math**
  *   *Number of Patches:* $( \frac{\text{Image Width}}{\text{Patch Width}} ) \times ( \frac{\text{Image Height}}{\text{Patch Height}} )$
  *   *Flattened Patch Dimension:* $\text{Patch Width} \times \text{Patch Height} \times 3 \text{ (RGB Channels)}$
  *   *Total Sequence Length:* Number of Patches **+ 1** (for the `[CLS]` token).
  
  **5. Self-Attention (Dot Product & Softmax)**
  *   *Step 1:* Compute dot product of Query and Keys ($Q \cdot K_1$, $Q \cdot K_2$).
  *   *Step 2:* Apply Softmax to get weights ($\alpha$). Formula: $\frac{e^{\text{score}}}{\sum e^{\text{all scores}}}$.
  *   *Step 3:* Multiply weights by Values and sum them up: $\text{Output} = (\alpha_1 \times V_1) + (\alpha_2 \times V_2)$.
  
  **6. Degree Centrality**
  *   *Formula:* $\frac{\text{Degree of Node}}{N - 1}$ (where $N$ is total number of nodes in the graph).
  
  **7. Single-Step Gradient Descent**
  *   *Loss Gradient:* $\frac{\partial L}{\partial w_1} = (\hat{y} - y) \times x_1$
  *   *Weight Update:* $w_{new} = w_{old} - \eta \frac{\partial L}{\partial w}$