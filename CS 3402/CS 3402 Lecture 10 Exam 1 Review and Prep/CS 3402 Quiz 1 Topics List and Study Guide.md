### **Part I: Data Science Foundations (Easy)**
*Focus: High-level concepts, terminology, and the big picture.*

*   **The AI/ML Hierarchy:**
  *   **Artificial Intelligence (AI):** The broadest concept of machines mimicking human intelligence.
  *   **Machine Learning (ML):** A subset of AI where systems learn from data without being explicitly programmed.
  *   **Deep Learning (DL):** A subset of ML that uses multi-layered Artificial Neural Networks (ANNs).
*   **Traditional CS vs. Machine Learning:**
  *   *Traditional CS:* Data + Rules (Code) = Answers.
  *   *Machine Learning:* Data + Answers (Labels) = Rules (The Model).
*   **The Data Science Venn Diagram (Drew Conway):**
  *   DS requires three things: **Hacking Skills** (coding), **Math/Statistics**, and **Domain Expertise**.
  *   *The Danger Zone:* Hacking Skills + Domain Expertise (but no Math/Stats). This leads to false conclusions.
*   **Types of Machine Learning:**
  *   **Supervised Learning:** Learning from *labeled* data (you have an answer key). 
  *   **Unsupervised Learning:** Learning from *unlabeled* data (finding hidden patterns on your own).
*   **Types of Supervised Learning Tasks:**
  *   **Regression:** Predicting a continuous number (e.g., House Prices, Stock Market).
  *   **Classification:** Predicting a discrete category (e.g., Dog vs. Cat, Spam vs. Safe).
*   **Types of Unsupervised Learning Tasks:**
  *   **Clustering:** Grouping similar items together (e.g., Customer segmentation, detecting social network communities).

---
- ### **Part II: Python & Tools (Easy-Medium)**
  *Focus: Knowing what libraries do and understanding basic coding environments.*
  
  *   **IDEs (Integrated Development Environments):**
    *   *What they do:* Help organize, write, run, and debug code efficiently (e.g., VS Code, PyCharm, Jupyter).
  *   **Virtual Environments (Conda / `.venv`):**
    *   *Why use them?* To manage dependencies and prevent different projects from breaking each other due to conflicting library versions.
  *   **The Core Python Libraries:**
    *   **NumPy:** Used for numerical arrays and fast math.
    *   **Pandas:** Used for structured data (DataFrames, loading CSVs).
    *   **Matplotlib:** Used for static visualizations (graphs, scatter plots, histograms).
    *   **Scikit-Learn (sklearn):** Used for traditional ML algorithms (Logistic Regression) and utilities like `train_test_split`.
  *   **PyTorch vs. NumPy:**
    *   *Similarity:* Both use multidimensional arrays (Tensors = Arrays).
    *   *Differences:* PyTorch Tensors can run on **GPUs** (for massive parallel acceleration via CUDA) and have built-in **Autograd** (automatic gradient calculation for calculus).
  *   **Basic Python Syntax:** Know how to read simple code. E.g., `np.mean([1,2,3,4])` outputs `2.5`.
  
  ---
- ### **Part III: Descriptive Statistics (Medium)**
  *Focus: Interpreting data shapes and knowing the formulas.*
  
  *   **Population vs. Sample:**
    *   *Population:* The entire group you want to study.
    *   *Sample:* A subset of the population actually measured.
  *   **Measures of Central Tendency:**
    *   **Mean (Average):** $\mu = \frac{1}{n} \sum x_i$. Uses all data, but highly sensitive to outliers.
    *   **Median:** The middle value of a sorted list. Highly robust/resistant to outliers (e.g., used for house prices).
  *   **Measures of Spread:**
    *   **Variance ($\sigma^2$):** Measures how widely spread the data points are from the mean. Formula: $\frac{1}{n} \sum (x_i - \mu)^2$. (Know how to calculate this by hand for a small list!).
    *   **Standard Deviation ($\sigma$):** The square root of variance.
  *   **Distributions:**
    *   **PMF (Probability Mass Function):** Used for *discrete* variables. Sum of all probabilities = 1.
    *   **PDF (Probability Density Function):** Used for *continuous* variables. Area under the curve = 1.
    *   **CDF (Cumulative Distribution Function):** The probability that a value is *less than or equal to* $X$.
  *   **Z-Scores (Standardization):**
    *   Formula: $Z = \frac{x - \mu}{\sigma}$. 
    *   Meaning: How many standard deviations a point is away from the mean. Normalizes data for deep learning.
  *   **Covariance vs. Pearson Correlation:**
    *   *Covariance:* Shows if two variables move together, but the scale depends on the units.
    *   *Correlation ($\rho$):* Normalizes covariance to strictly sit between **-1 and 1**. (1 is perfect positive, -1 is perfect negative, 0 is no linear relation).
  
  ---
- ### **Part IV: Derivatives in Deep Learning (Medium)**
  *Focus: The calculus that allows models to learn.*
  
  *   **Why Derivatives?**
    *   They calculate the gradient (slope), which tells the model how to update its weights to **minimize the loss function**.
  *   **Basic Power Rule:** 
    *   If $f(x) = x^2 + x$, then $\frac{df}{dx} = 2x + 1$.
  *   **The Chain Rule:**
    *   Used when functions are nested. Derivative of the outside $\times$ derivative of the inside. 
    *   If $f(x) = (bx - a)^2$, then $\frac{df}{dx} = 2b(bx - a)$.
  *   **Gradient Descent Formula:**
    *   $w_{new} = w_{old} - \eta \frac{\partial L}{\partial w}$
    *   $\eta$ (eta) is the **Learning Rate**. (If slope is positive, we decrease the weight. If slope is negative, we increase the weight).
  
  ---
- ### **Part V: Supervised Learning (Medium)**
  *Focus: Training loops, evaluating models, and manual weight updates.*
  
  *   **Training vs. Testing Data:**
    *   *Training Set:* Used to teach the model (calculate gradients and update weights).
    *   *Testing Set:* Used to evaluate the model on data it has never seen.
  *   **Overfitting vs. Underfitting:**
    *   *Overfitting:* Model memorizes the training data perfectly (99% train accuracy) but fails on new data (60% test accuracy).
    *   *Underfitting:* Model fails to learn anything useful (poor accuracy on both).
  *   **Loss Function:**
    *   A formula measuring the error between the model's prediction and the true answer.
    *   *Example:* Mean Squared Error (MSE), which is $\mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$.
  *   **The "Forward Pass & Backward Pass" Calculation:**
    *   *You must know how to do this by hand! (See Lecture 7 / Lecture 10 Slide 12).*
    *   **Step 1 (Prediction):** $\hat{y} = w_1x_1 + w_2x_2 + w_3x_3$
    *   **Step 2 (Loss):** $\frac{1}{2}(\hat{y} - y)^2$
    *   **Step 3 (Gradients):** For squared error, the gradient for any weight $w_i$ is always the Error $\times$ the Input: $(\hat{y} - y) \times x_i$.
    *   **Step 4 (Update):** Use the gradient descent formula to find the new weights.
  
  ***
- ### **Professor Yu's "Tricks" to watch out for:**
  1.  **"Does Supervised Learning require labeled data?"** Yes. (Unsupervised does not).
  2.  **"If Variance is high, what does it mean?"** Do not say the mean is high. It strictly means the data is widely spread out.
  3.  **The PMF math problem:** Remember that all probabilities must add up to $1.0$. If $P(X=x) = kx$ for $x=[1,2,3,4]$, then $1k+2k+3k+4k = 10k = 1.0$, so $k = 0.1$.
  4.  **Learning Rate:** If asked what happens if the learning rate ($\eta$) is too high, the answer is "the model will overshoot the minimum." If too low, "the model learns too slowly."