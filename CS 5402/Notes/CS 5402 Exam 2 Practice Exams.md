# **CS 5402: Exam 2 Practice Test**
## **Section A: True / False (10 Points)**
  *Identify whether the following statements are True or False.*
  
  1. **[T / F]** In a Deep Convolutional Neural Network (CNN), if a ReLU activation function receives a negative input, its local derivative becomes exactly 0, meaning the weights preceding that node will not be updated during that specific backpropagation step.
  2. **[T / F]** A standard Recurrent Neural Network (RNN) processes tokens entirely in parallel, whereas a Transformer processes tokens sequentially from left to right.
  3. **[T / F]** When evaluating a model designed to detect rare credit card fraud, "Accuracy" is generally the best metric to use because it accounts for both True Positives and True Negatives.
  4. **[T / F]** In Self-Supervised Learning (SSL), the BYOL (Bootstrap Your Own Latent) architecture prevents representation collapse by generating millions of negative sample pairs to repel the positive pairs.
  5. **[T / F]** If you train an Autoencoder to detect anomalies, making the bottleneck (latent space) extremely wide will likely cause the model to fail, as it will over-generalize and perfectly reconstruct the anomalies.
  
  ---
## **Section B: Multiple Choice (10 Points)**
  *Select the best answer for each question.*
  
  **6. You are using K-Means clustering. You run the algorithm with $K=4$ and get a Sum of Squared Errors (SSE) of 150. You run it again with $K=6$ and get an SSE of 210. What is the most mathematically sound conclusion?**
  A) The true number of natural clusters in the data is exactly 4.
  B) The algorithm got stuck in a bad local minimum during the $K=6$ run due to poor random initialization.
  C) The data contains non-convex clusters, causing K-Means to fail.
  D) Increasing $K$ naturally increases the SSE.
  
  **7. Which of the following accurately describes how Bagging and Boosting handle the Bias-Variance tradeoff?**
  A) Bagging averages high-bias models to reduce bias; Boosting combines high-variance models to reduce variance.
  B) Bagging averages high-variance models (deep trees) to reduce variance; Boosting sequentially trains high-bias models (stumps) to reduce bias.
  C) Both methods are designed exclusively to reduce variance.
  D) Both methods reduce bias, but only Bagging reduces variance.
  
  **8. Which of the following is the PRIMARY reason that Transformers can be trained significantly faster than LSTMs on massive text datasets?**
  A) Transformers do not use activation functions.
  B) Transformers do not require Positional Encoding.
  C) Transformers process the entire sequence simultaneously in parallel via Self-Attention, while LSTMs must process tokens one time-step at a time.
  D) LSTMs suffer from Exploding Gradients, which require the hardware to pause and clip the gradients.
  
  **9. In the Self-Attention formula, $\text{Attention}(Q,K,V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V$, why is the term divided by $\sqrt{d_k}$?**
  A) To convert the raw scores into probabilities that sum to 1.
  B) To inject the sequence order back into the permutation-invariant matrix.
  C) To prevent dot products from growing too large in high dimensions, which would push the Softmax function into flat regions and kill the gradient.
  D) To reduce the number of parameters the network must learn.
  
  **10. You are using the Local Outlier Factor (LOF) algorithm to detect anomalies. For a specific data point $X$, you calculate the LOF score to be 15.3. What does this indicate?**
  A) $X$ is a normal data point sitting deep inside a dense cluster.
  B) $X$ is an outlier sitting in a sparse region, while its nearest neighbors are in a much denser region.
  C) $X$ is an outlier sitting inside a highly dense region.
  D) The algorithm failed to converge.
  
  ---
## **Section C: Short Answer (15 Points)**
  
  **11. Overfitting and Model Characteristics:**
  You train a Soft-Margin Support Vector Machine (SVM) and notice it is severely **overfitting** the training data, resulting in a jagged decision boundary that weaves around noisy outliers. What hyperparameter should you adjust, and in what direction (higher or lower) to fix this overfitting? Explain why mathematically or geometrically.
  
  **12. Pretext Tasks vs. Contrastive Learning:**
  In the early days of Self-Supervised Learning (SSL), researchers used "Pretext Tasks" (like predicting image rotations or solving jigsaw puzzles). Why did the AI community move away from these in favor of Contrastive Learning (like SimCLR)? What specific flaw did pretext tasks suffer from?
  
  **13. LSTM Architecture:**
  Explain the specific mathematical role of the **Forget Gate** in an LSTM. How does this gate (combined with the Cell State) help the LSTM overcome the Vanishing Gradient problem that plagues standard RNNs?
  
  ---
## **Section D: Hand-Calculating Problems (40 Points)**
  
  **14. CNN Convolution \& Parameters (15 pts)**
  You are applying a $3 \times 3$ convolutional filter to a $3 \times 3$ patch of an image.
  *   **Image Patch:** 
    $\begin{bmatrix} 1 & 2 & 1 \\ 0 & 1 & 0 \\ 2 & 1 & 2 \end{bmatrix}$
  *   **Filter Weights:** 
    $\begin{bmatrix} 1 & 0 & -1 \\ 1 & 0 & -1 \\ 1 & 0 & -1 \end{bmatrix}$
  
  **A)** Perform the element-wise convolution operation. What is the single output value for this patch?
  **B)** Assume the full input image is $64 \times 64$ pixels with **3 channels** (RGB). You apply a Convolutional Layer with **32 filters** of size $3 \times 3$, a **stride of 1**, and **padding of 1**. 
  Calculate the spatial size of the output feature map.
  **C)** Calculate the total number of learnable parameters (weights + biases) in this specific Convolutional Layer.
  
  **15. Backpropagation \& The Chain Rule (15 pts)**
  You are doing a manual backward pass on a tiny neural network. 
  *   **Forward Pass Setup:** Input $x_1 = 3.0$. Weight $w_1 = 0.5$.
  *   **Node 1 (Linear):** $h_1 = w_1 \cdot x_1$
  *   **Node 2 (Activation):** $a_1 = \text{ReLU}(h_1)$
  *   **Node 3 (Output):** $\hat{y} = a_1 \cdot w_2$, where $w_2 = 2.0$.
  *   **Loss Function:** $L = (y - \hat{y})^2$, where the true label $y = 5.0$.
  
  **A)** Perform the forward pass to find the exact numerical value of the prediction $\hat{y}$.
  **B)** Use the Chain Rule to calculate the gradient of the loss with respect to the first weight: $\frac{\partial L}{\partial w_1}$. Show your intermediate derivatives.
  
  **16. Self-Attention Math (10 pts)**
  You are calculating Self-Attention for a single word. You have already multiplied the input by the weight matrices and obtained the following vectors:
  *   **Query vector ($q$):** $[2, 0]$
  *   **Key vectors ($k_1, k_2$):** $k_1 = [1, 0]$, $k_2 =[0, 2]$
  *   **Value vectors ($v_1, v_2$):** $v_1 = [10, 0]$, $v_2 = [0, 20]$
  
  **A)** Calculate the raw dot-product attention scores ($q \cdot k_1$ and $q \cdot k_2$).
  **B)** Assume we do *not* scale the scores. Apply the Softmax function to the scores. *(Hint: $e^0 = 1$, $e^2 \approx 7.4$).*
  **C)** Calculate the final contextualized output vector for this word by multiplying the Softmax weights by the Value vectors and summing them.
  
  ---
## **Section E: Open-Ended / Design (25 Points)**
  
  **17. Designing a Self-Supervised Anomaly Detection Framework (15 pts)**
  *(Note: This mirrors the exact question type your professor hinted at).*
  You are given a **large, completely unlabeled** time-series dataset from sensors monitoring a factory manufacturing robotic arm. You need to build a system to detect when the arm is malfunctioning. 
  Design a Self-Supervised Learning (SSL) anomaly detection framework. In your response:
  *   **Define a pretext task or contrastive task** appropriate for time-series data.
  *   **Explain how data augmentation or "views" are generated** for your chosen task.
  *   **Describe how you would evaluate** the anomaly detector during real-world inference (how do you calculate an anomaly score?).
  *   **Discuss one potential limitation** or failure mode of your chosen approach.
  
  **18. Overfitting and Underfitting in Ensembles (10 pts)**
  You are training models on a highly complex, non-linear dataset. 
  *   **Model A** is a Random Forest (Bagging) built using 100 Decision Stumps (trees with a maximum depth of 1).
  *   **Model B** is a Gradient Boosting Machine (Boosting) built using 100 deep, fully-grown, unpruned Decision Trees.
  Both models fail miserably on the Test Set. Explain **why** each model failed, explicitly referencing Bias, Variance, and the mathematical purpose of Bagging vs. Boosting.
  
  ***
  ***
  *(Stop here! Take the test, then scroll down to grade yourself).*
  .
  .
  .
  .
  .
  .
  .
  .
  .
  .
  .
  .
# **Answer Key \& Step-by-Step Solutions**
### **Section A: True / False**
  1. **True.** The derivative of ReLU for negative inputs is exactly 0. By the Chain Rule, multiplying by 0 kills the gradient, so the weights feeding that node don't update.
  2. **False.** It is the exact opposite. RNNs process tokens sequentially (left to right), while Transformers process all tokens entirely in parallel.
  3. **False.** Accuracy is a terrible metric for rare fraud (imbalanced data). A model that always guesses "Not Fraud" gets 99.9% accuracy but catches 0 fraud. You must use Precision, Recall, or PR-AUC.
  4. **False.** BYOL is famous specifically because it does **NOT** use negative samples! It avoids collapse by using an asymmetric Teacher/Student architecture where the Teacher updates via Exponential Moving Average (EMA).
  5. **True.** If the bottleneck is too large, the autoencoder acts as an identity function and perfectly reconstructs everything (including anomalies). Since the reconstruction error is 0, it fails to flag the outlier.
### **Section B: Multiple Choice**
  6. **B.** Increasing $K$ must mathematically decrease or equal the SSE. If it goes up, the algorithm got stuck in a local minimum due to a bad random initialization.
  7. **B.** Bagging averages deep trees (high variance models) to smooth out Variance. Boosting sequentially trains stumps (high bias models) to iteratively reduce Bias.
  8. **C.** Parallelization via Self-Attention is the key to Transformer speed.
  9. **C.** Without dividing by $\sqrt{d_k}$, high-dimensional dot products get massive, pushing Softmax into the flat corners ($1.0$ and $0.0$) where the gradients vanish.
  10. **B.** An LOF $\gg 1$ means the point is in a sparse area but its neighbors are packed tightly together, making it a clear outlier.
### **Section C: Short Answer**
  **11. Overfitting in SVM:**
  You should adjust the **$C$ parameter to be LOWER**. 
  *Why:* $C$ controls the penalty for misclassified/margin-violating points ($\min \frac{1}{2}||w||^2 + C \sum \xi_i$). A high $C$ heavily punishes errors, forcing the model to weave a complex, overfitted boundary to perfectly separate the noise. Lowering $C$ allows the model to accept a few errors (Soft-Margin) in exchange for a wider, smoother, more generalized margin (reducing Variance / Overfitting).
  
  **12. Pretext Tasks vs. Contrastive Learning:**
  The AI community moved away from Pretext tasks (like jigsaw puzzles) because they suffered from **Shortcut Learning**. The models would learn trivial tricks (like matching edge pixels or chromatic aberration) to solve the puzzle, rather than learning true semantic "visual commonsense." Contrastive learning (SimCLR/BYOL) fixed this by forcing the model to learn general similarity structures (attracting positive pairs and repelling negatives) without relying on a handcrafted puzzle.
  
  **13. LSTM Forget Gate:**
  The Forget Gate ($f_t$) uses a Sigmoid function to output a value between 0 and 1, deciding what information to erase from the Long-Term Cell State ($C_{t-1}$). 
  *How it fixes the gradient:* The cell state is updated via addition ($C_t = f_t C_{t-1} + \dots$). During backpropagation, the derivative $\frac{\partial C_t}{\partial C_{t-1}}$ is exactly $f_t$. As long as the network learns to keep $f_t \approx 1$, the gradient is perfectly preserved (multiplied by 1) as it flows backward through time, preventing the Vanishing Gradient caused by standard RNN matrix multiplication.
### **Section D: Hand-Calculating**
  **14. CNN Math**
  *   **A) Convolution:** Multiply element-wise and sum.
    *   Left column: $(1)(1) + (0)(1) + (2)(1) = 3$
    *   Middle column: $(2)(0) + (1)(0) + (1)(0) = 0$
    *   Right column: $(1)(-1) + (0)(-1) + (2)(-1) = -3$
    *   Total Sum: $3 + 0 - 3 = \mathbf{0}$.
  *   **B) Output Size:** $\lfloor\frac{64 - 3 + 2(1)}{1}\rfloor + 1 = \lfloor\frac{63}{1}\rfloor + 1 = \mathbf{64 \times 64}$.
  *   **C) Parameters:** $(Filter \times InputChannels + Bias) \times NumFilters$
    *   $( (3 \times 3 \times 3) + 1 ) \times 32 = (27 + 1) \times 32 = 28 \times 32 = \mathbf{896}$ parameters.
  
  **15. Backpropagation**
  *   **A) Forward Pass:**
    *   $h_1 = 0.5 \times 3.0 = 1.5$
    *   $a_1 = \max(0, 1.5) = 1.5$
    *   $\hat{y} = 1.5 \times 2.0 = \mathbf{3.0}$
  *   **B) Backward Pass (Chain Rule):**
    *   $\frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial a_1} \cdot \frac{\partial a_1}{\partial h_1} \cdot \frac{\partial h_1}{\partial w_1}$
    *   $\frac{\partial L}{\partial \hat{y}} = -2(y - \hat{y}) = -2(5.0 - 3.0) = -2(2.0) = \mathbf{-4.0}$
    *   $\frac{\partial \hat{y}}{\partial a_1} = w_2 = \mathbf{2.0}$
    *   $\frac{\partial a_1}{\partial h_1} = \mathbf{1.0}$ (because $h_1 = 1.5 > 0$, ReLU derivative is 1).
    *   $\frac{\partial h_1}{\partial w_1} = x_1 = \mathbf{3.0}$
    *   Multiply: $-4.0 \times 2.0 \times 1.0 \times 3.0 = \mathbf{-24.0}$.
  
  **16. Self-Attention**
  *   **A) Dot Product Scores:**
    *   $s_1 = q \cdot k_1 = (2\times1) + (0\times0) = \mathbf{2}$
    *   $s_2 = q \cdot k_2 = (2\times0) + (0\times2) = \mathbf{0}$
  *   **B) Softmax:**
    *   Exponentials: $e^2 \approx 7.4$, $e^0 = 1$.
    *   Sum: $7.4 + 1 = 8.4$.
    *   Probabilities: $\alpha_1 = 7.4 / 8.4 \approx \mathbf{0.88}$, $\alpha_2 = 1 / 8.4 \approx \mathbf{0.12}$.
  *   **C) Final Output:**
    *   $Z = (\alpha_1 \cdot v_1) + (\alpha_2 \cdot v_2)$
    *   $Z = (0.88 \cdot [10, 0]) + (0.12 \cdot [0, 20])$
    *   $Z = [8.8, 0] + [0, 2.4] = \mathbf{[8.8, 2.4]}$.
### **Section E: Open-Ended / Design**
  **17. Anomaly Detection Design (ECG / Time-Series)**
  *   **Task Definition:** I will use a **Predictability Modeling** approach (or an Autoencoder reconstruction approach). Using an LSTM Autoencoder, the model will be trained exclusively on normal factory arm sensor data to reconstruct a rolling 10-second window of data. 
  *   **Views/Augmentation (If using Contrastive):** *(Note: If you picked SimCLR, you must answer this)*. I would generate positive pairs by adding minor Gaussian noise (sensor static) or slightly scaling the amplitude of the sensor wave. A 10-second window from a different hour would serve as a negative sample.
  *   **Evaluation (Inference):** During live inference, I pass the live sensor data through the Autoencoder. I calculate the **Reconstruction Error** $E = ||x - \hat{x}||$. If the arm malfunctions, the model will fail to reconstruct the erratic signal. If $E > \text{Threshold}$, I flag it as an anomaly.
  *   **Limitation:** A major limitation is **over-generalization**. If the bottleneck of the Autoencoder is too wide, the network might act as an identity function and perfectly reconstruct the malfunction, resulting in a low error and a missed alarm (False Negative).
  
  **18. Overfitting and Underfitting in Ensembles**
  *   **Model A (Bagged Stumps) failed due to Underfitting (High Bias).** The purpose of Bagging is to reduce *Variance* by averaging independent models. However, Decision Stumps have high Bias (they are too simple to capture complex non-linear data). Averaging 100 underfitted models just results in an average underfitted model. Bagging cannot fix Bias.
  *   **Model B (Boosted Deep Trees) failed due to Overfitting (High Variance).** The purpose of Boosting is to sequentially reduce *Bias* by forcing the model to focus on the errors of the previous step. Deep Trees naturally have extremely Low Bias but High Variance (they memorize training data perfectly). Because a Deep Tree already has ~0 training error, boosting it simply forces the subsequent trees to aggressively model random noise, resulting in catastrophic overfitting on the test set.
- ---
- ---
- ---
# **CS 5402: Exam 2 Practice Test (Version B)**
## **Section A: True / False (10 Points)**
  *Identify whether the following statements are True or False.*
  
  1. **[T / F]** K-Means clustering can be viewed mathematically as a special, restricted case of a Gaussian Mixture Model (GMM) where the covariance matrices are strictly spherical and assignments are hard (0 or 1) instead of probabilistic.
  2. **[T / F]** In a Convolutional Neural Network (CNN), Max Pooling layers contain learnable parameters (weights) that are updated via backpropagation to identify the most important features.
  3. **[T / F]** SimCLR, a Contrastive Learning framework, requires an asymmetric Teacher-Student network architecture to avoid representation collapse because it does not use negative samples.
  4. **[T / F]** In the Local Outlier Factor (LOF) algorithm, if a data point has an LOF score of roughly 0.1, it indicates that the point is sitting in a highly dense region surrounded by a much sparser region.
  5. **[T / F]** In a Transformer, the Self-Attention mechanism processes sequence data one token at a time from left to right, which is why Positional Encoding is required to speed up the matrix multiplications.
  
  ---
## **Section B: Multiple Choice (10 Points)**
  *Select the best answer for each question.*
  
  **6. You are comparing an LSTM to a standard Recurrent Neural Network (RNN). Which of the following best explains mathematically why the LSTM successfully alleviates the Vanishing Gradient problem over long sequences?**
  A) The LSTM uses the ReLU activation function instead of Tanh.
  B) The LSTM Cell State ($C_t$) updates via element-wise addition rather than repeated matrix multiplication, allowing the gradient to flow backward intact if the Forget Gate is near 1.
  C) The LSTM parallelizes the input sequence, completely removing the need for Backpropagation Through Time (BPTT).
  D) The LSTM entirely removes the Hidden State ($h_t$), relying only on the Input Gate.
  
  **7. In AdaBoost, after a weak learner (stump) is evaluated, the algorithm updates the weights of the data points. If a specific data point is MISCLASSIFIED by the current stump, what happens to its weight for the next round?**
  A) It is multiplied by $e^{-\alpha}$ (decreasing its importance).
  B) It is multiplied by $e^{\alpha}$ (increasing its importance).
  C) It is multiplied by 0 to remove the outlier from the dataset.
  D) It is divided by the total number of trees $T$.
  
  **8. You have a highly imbalanced time-series dataset containing millions of normal heartbeats and only a few dozen arrhythmias. You train a One-Class SVM to detect the arrhythmias. Which evaluation metric should you prioritize to measure the success of your model?**
  A) Accuracy
  B) Root Mean Squared Error (RMSE)
  C) Receiver Operating Characteristic (ROC-AUC)
  D) Precision-Recall Area Under Curve (PR-AUC)
  
  **9. When moving from a 2D space to a 10,000D space, the "Curse of Dimensionality" states that:**
  A) The dataset becomes infinitely dense.
  B) The mathematical distance to a point's nearest neighbor becomes almost identical to the distance to its farthest neighbor, making distance metrics less meaningful.
  C) PCA will fail to find any principal components.
  D) Linear models become exponentially faster than non-linear models.
  
  **10. You have a Neural Network where the loss is oscillating wildly and eventually results in `NaN` weights. This is diagnosed as the Exploding Gradient problem. Which of the following is the standard practice to fix this?**
  A) Gradient Clipping
  B) Replacing ReLU with a linear activation function
  C) Removing the Add & Norm layers
  D) Increasing the network depth
  
  ---
## **Section C: Short Answer (15 Points)**
  
  **11. Overfitting vs. Underfitting:**
  You train a Convolutional Neural Network (CNN) for image classification. After 50 epochs, the Training Error is 40% and the Test Error is 45%. 
  A) Does this model suffer from High Bias (Underfitting) or High Variance (Overfitting)?
  B) Provide **two** specific methods/architectural changes you could make to fix this problem.
  
  **12. Reconstruction-Based Outlier Detection:**
  Explain step-by-step how a reconstruction-based method (like an Autoencoder or PCA) identifies an outlier in a dataset consisting entirely of normal data (e.g., normal machine vibrations). How is the "anomaly score" calculated?
  
  **13. LSTM vs. Transformer (Computational Complexity):**
  The Transformer architecture famously replaced LSTMs for Natural Language Processing. Briefly compare the **computational complexity** of an LSTM versus a Transformer's Self-Attention mechanism as the sequence length ($n$) grows. Why does the Transformer require significantly more memory for very long documents?
  
  ---
## **Section D: Hand-Calculating Problems (40 Points)**
  
  **14. CNN Output Matrix Computation (15 pts)**
  You are given a $4 \times 4$ input image patch and a $2 \times 2$ filter.
  *   **Input Image:** 
    $\begin{bmatrix} 1 & 2 & 0 & 1 \\ 0 & 1 & 1 & 0 \\ 2 & 1 & 2 & 1 \\ 1 & 0 & 1 & 1 \end{bmatrix}$
  *   **Filter (Kernel):** 
    $\begin{bmatrix} 1 & -1 \\ 0 & 1 \end{bmatrix}$
  
  You apply a Convolutional Layer with a **Stride of 2** and **No Padding (Padding = 0)**.
  A) Use the output size formula to determine the dimensions (Width $\times$ Height) of the resulting output matrix.
  B) Perform the convolution operation and write out the exact values of the final output matrix. Show your work for at least the top-left value.
  
  **15. Backpropagation (15 pts)**
  You are doing a manual backward pass on a neural network predicting a continuous value. 
  *   **Forward Pass:** 
    *   Input $x = 2.0$. 
    *   Weight $w_1 = 0.5$ connects $x$ to hidden node $h$.
    *   Raw score $h = w_1 \cdot x$.
    *   Activation $a = \text{ReLU}(h)$.
    *   Weight $w_2 = 3.0$ connects $a$ to the output $\hat{y}$.
    *   Prediction $\hat{y} = a \cdot w_2$.
  *   **Loss Function:** $L = (y - \hat{y})^2$, where the true label $y = 1.0$.
  
  A) Perform the forward pass to find the exact numerical value of the prediction $\hat{y}$.
  B) Use the Chain Rule to calculate the gradient of the loss with respect to the first weight: $\frac{\partial L}{\partial w_1}$. Show the exact chain of partial derivatives you used.
  
  **16. AdaBoost Math (10 pts)**
  You are running AdaBoost. In round $t=1$, you have 4 data points. All initial weights $D_1(i) = 0.25$.
  Your first weak learner ($h_1$) classifies 3 points correctly, but misses exactly **1 point**.
  A) Calculate the weighted training error ($\epsilon_1$).
  B) Calculate the "Amount of Say" ($\alpha_1$). *(Formula: $\alpha = 0.5 \ln(\frac{1-\epsilon}{\epsilon})$. Hint: $\ln(3) \approx 1.098$)*.
  C) Calculate the new, **unnormalized** weight for the single **misclassified** point before moving to Round 2. *(Formula: $D_{new} = D_{old} \times e^\alpha$. Hint: $e^{0.549} \approx 1.732$)*.
  
  ---
## **Section E: Open-Ended / Design (25 Points)**
  
  **17. SSL Theory \& Critique (15 pts)**
  *(Note: This is pulled directly from the professor's provided study guide questions).*
  Self-supervised learning (SSL) relies heavily on the assumption that solving a "pretext task" (like predicting image rotations or solving a jigsaw puzzle) leads to useful feature representations for downstream tasks. 
  A) When can this assumption fail? Provide at least **two** concrete failure scenarios (e.g., what the model learns instead of semantics, or domain issues).
  B) Propose **one** improvement or alternative SSL approach that the AI community developed to fix these pretext task failures.
  
  **18. Time-Series Anomaly Detection (10 pts)**
  You are given a streaming time-series dataset of a patient's heart rate (BPM) and are asked to build an anomaly detection rule using a simple **Moving Average**.
  A) Write out the mathematical formula for the expected value $\hat{x}_t$ using a window size of $k$.
  B) Write out the formula for the Residual (Error) $e_t$.
  C) Explain the exact Decision Rule you would use to flag a point as an anomaly based on these calculations.
  
  ***
  ***
  *(Stop here! Take the test, then scroll down to grade yourself).*
  .
  .
  .
  .
  .
  .
  .
  .
  .
  .
  .
  .
# **Answer Key \& Step-by-Step Solutions**
### **Section A: True / False**
  1. **True.** K-Means is a restricted case of GMM where covariance is spherical (identity matrix) and E-step probabilities are forced to 0 or 1 (hard assignment).
  2. **False.** Pooling layers (Max/Avg) have **zero** learnable parameters. They are fixed mathematical operations used purely to downsample the spatial size.
  3. **False.** SimCLR is the contrastive learning model that *requires* large batches to generate negative samples. **BYOL** is the one that uses an asymmetric Teacher-Student moving average to avoid collapse without negatives. 
  4. **True.** An LOF $\ll 1$ (much less than 1) indicates the point's local density is significantly higher than its neighbors' average density. It implies the point is inside a tight, dense cluster while its neighbors sit on the sparser edge. (An LOF $\gg 1$ would be an outlier).
  5. **False.** Self-Attention processes the entire sequence in **parallel** (all tokens simultaneously). This is exactly why it *needs* Positional Encoding—because parallel processing destroys the inherent order of the sequence that RNNs normally capture.
### **Section B: Multiple Choice**
  6. **B.** K-Means cost ($J$) should mathematically decrease or stay flat as $K$ increases. If it goes up, the algorithm converged to a poor local minimum due to a bad random seed.
  7. **B.** Misclassified points are penalized heavily by multiplying their weight by $e^\alpha$, forcing the next sequential stump to focus on them.
  8. **D.** PR-AUC (Precision-Recall Area Under Curve). For severe imbalance, ROC-AUC is misleading because the massive number of True Negatives artificially inflates the curve's success. 
  9. **B.** The distribution of distances narrows to a spike; everything becomes roughly equidistant, which is the definition of the Curse of Dimensionality.
  10. **A.** Gradient Clipping acts as a hard speed-limit on gradient updates, resetting the value if it exceeds an upper bound, preventing the math from exploding to NaN.
### **Section C: Short Answer**
  **11. Overfitting vs. Underfitting:**
  *   **A)** High Bias (**Underfitting**). A 40% error on the training set means the model is too simple to even learn the data it is actively looking at.
  *   **B)** To fix High Bias (Underfitting), you must add complexity to the model. Solutions: 1) Increase the depth of the network (more layers). 2) Increase the width of the network (more filters/neurons). 3) Decrease regularization (lower dropout rate). 4) Train for more epochs.
  
  **12. Reconstruction-Based Outliers:**
  *   You train the Autoencoder or PCA exclusively on the **Normal Data**. The model learns how to compress and reconstruct the normal patterns perfectly.
  *   During inference, you pass a new point $x$ through the model to get its reconstruction $\hat{x}$. 
  *   The anomaly score is the **Reconstruction Error**: $||x - \hat{x}||$. Because the model only knows how to rebuild normal data, anomalies will be reconstructed poorly, resulting in a high error score which triggers the alarm.
  
  **13. LSTM vs. Transformer Complexity:**
  *   **LSTM:** Computational complexity is **$O(n)$** where $n$ is sequence length. It processes step-by-step.
  *   **Transformer:** Computational complexity is **$O(n^2)$**. Because Self-Attention requires every single token to calculate a dot product with *every other token* in the sequence, the memory and compute requirement grows quadratically. Very long documents require massive $n \times n$ attention matrices, consuming huge amounts of VRAM.
### **Section D: Hand-Calculating**
  **14. CNN Output Matrix Computation**
  *   **A) Output Size Formula:** $O = \lfloor \frac{\text{Input} - \text{Filter} + 2P}{S} \rfloor + 1$
    *   $O = \lfloor \frac{4 - 2 + 0}{2} \rfloor + 1 = \lfloor \frac{2}{2} \rfloor + 1 = 1 + 1 = \mathbf{2}$. 
    *   The output matrix is **$2 \times 2$**.
  *   **B) Convolution Math (Stride = 2 means jumping 2 pixels at a time!):**
    *   **Top-Left (Rows 0-1, Cols 0-1):** 
        $(1\times1) + (2\times-1) + (0\times0) + (1\times1) = 1 - 2 + 0 + 1 = \mathbf{0}$.
    *   **Top-Right (Rows 0-1, Cols 2-3):** 
        $(0\times1) + (1\times-1) + (1\times0) + (0\times1) = 0 - 1 + 0 + 0 = \mathbf{-1}$.
    *   **Bottom-Left (Rows 2-3, Cols 0-1):** 
        $(2\times1) + (1\times-1) + (1\times0) + (0\times1) = 2 - 1 + 0 + 0 = \mathbf{1}$.
    *   **Bottom-Right (Rows 2-3, Cols 2-3):** 
        $(2\times1) + (1\times-1) + (1\times0) + (1\times1) = 2 - 1 + 0 + 1 = \mathbf{2}$.
    *   **Final Output Matrix:** $\mathbf{\begin{bmatrix} 0 & -1 \\ 1 & 2 \end{bmatrix}}$
  
  **15. Backpropagation**
  *   **A) Forward Pass:**
    *   $h = 0.5 \times 2.0 = 1.0$
    *   $a = \text{ReLU}(1.0) = 1.0$
    *   $\hat{y} = 1.0 \times 3.0 = \mathbf{3.0}$
  *   **B) Backward Pass (Chain Rule):**
    *   $\frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial a} \cdot \frac{\partial a}{\partial h} \cdot \frac{\partial h}{\partial w_1}$
    *   $\frac{\partial L}{\partial \hat{y}} = -2(y - \hat{y}) = -2(1.0 - 3.0) = -2(-2.0) = \mathbf{4.0}$
    *   $\frac{\partial \hat{y}}{\partial a} = w_2 = \mathbf{3.0}$
    *   $\frac{\partial a}{\partial h} = \mathbf{1.0}$ (because $h=1.0 > 0$, ReLU slope is 1)
    *   $\frac{\partial h}{\partial w_1} = x = \mathbf{2.0}$
    *   **Multiply:** $4.0 \times 3.0 \times 1.0 \times 2.0 = \mathbf{24.0}$.
  
  **16. AdaBoost Math**
  *   **A) Weighted Error ($\epsilon_1$):** 1 mistake $\times$ 0.25 weight = $\mathbf{0.25}$.
  *   **B) Amount of Say ($\alpha_1$):** 
    $\alpha_1 = 0.5 \ln(\frac{1 - 0.25}{0.25}) = 0.5 \ln(\frac{0.75}{0.25}) = 0.5 \ln(3) = 0.5(1.098) = \mathbf{0.549}$.
  *   **C) New Unnormalized Weight:**
    $D_{new} = D_{old} \times e^\alpha = 0.25 \times e^{0.549} = 0.25 \times 1.732 \approx \mathbf{0.433}$.
### **Section E: Open-Ended / Design**
  **17. SSL Theory \& Critique**
  *   **A) Failure Scenarios:** The assumption fails when the model learns "shortcuts" rather than semantic meaning. 
    *   *Scenario 1 (Jigsaw):* Instead of learning what a dog's body looks like to put the tiles in order, the CNN simply learns to match the low-level edge pixel textures across the cuts. 
    *   *Scenario 2 (Colorization):* The model learns that specific greyscale gradients always correspond to the hex code for "blue" (sky) or "green" (grass) without actually learning what grass or skies are.
    *   *Scenario 3 (Domain Mismatch):* The pretext task representations are so tightly coupled to the specific puzzle that they offer no generalized utility for the downstream task.
  *   **B) Alternative Approach:** The community moved to **Contrastive Learning** (e.g., SimCLR, BYOL). Instead of solving a handcrafted puzzle, Contrastive Learning forces the model to mathematically map similar images (positive pairs created via data augmentation) to the same vector space, and dissimilar images to distant vector spaces. This learns holistic, robust, generalized features.
  
  **18. Time-Series Anomaly Detection**
  *   **A) Expected Value:** $\hat{x}_t = \frac{1}{k} \sum_{i=t-k+1}^{t} x_i$ (The mathematical average of the heart rates over the previous $k$ time steps).
  *   **B) Residual Error:** $e_t = x_t - \hat{x}_t$ (The mathematical difference between the actual heart rate right now and the expected moving average).
  *   **C) Decision Rule:** Define a threshold $\tau$. If the absolute value of the residual error $|e_t|$ is strictly greater than $\tau$ (i.e., $|x_t - \hat{x}_t| > \tau$), flag the data point as an anomaly. This indicates the current heart rate deviates too wildly from its immediate recent history.