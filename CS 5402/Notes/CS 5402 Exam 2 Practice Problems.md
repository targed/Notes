### **Practice Questions: Lecture 15 (Neural Networks)**
  
  **Section A: True / False**
  
  **Q1. Universal Approximation**
  **True or False:** To approximate a highly complex, non-linear mathematical function with arbitrary precision, a Feed-Forward Neural Network mathematically requires at least 10 hidden layers.
  
  **Q2. Neural Network Limitations**
  **True or False:** Standard Multilayer Perceptrons (MLPs) inherently handle missing features (e.g., NaN values) well without needing imputation, because the network can simply learn to set the weights for those missing features to zero.
  
  ---
  
  **Section B: Multiple Choice**
  
  **Q3. Diagnosing Network Issues**
  You are training a 20-layer deep neural network. After 100 epochs, you notice that the weights in Layers 18, 19, and 20 are updating significantly, but the weights in Layers 1, 2, and 3 have barely changed from their initial random values. The overall model performance is very poor. What is the most likely cause, and what is the best architectural fix?
  A) Exploding Gradients; fix by adding Gradient Clipping.
  B) Vanishing Gradients; fix by replacing the ReLU activation functions with Sigmoid functions.
  C) Vanishing Gradients; fix by implementing Skip Connections (Residuals) and using ReLU.
  D) Overfitting; fix by increasing the depth of the network.
  
  **Q4. The "Multiple Paths" Chain Rule**
  In a standard Feed-Forward Neural Network, the input node $x_1$ connects to three hidden nodes: $h_1, h_2,$ and $h_3$. All three hidden nodes ultimately connect to the final loss $L$. When calculating the gradient of the loss with respect to the input ($\frac{\partial L}{\partial x_1}$), how must you handle the multiple paths?
  A) Multiply the gradients from all three paths together.
  B) Sum the gradients from all three paths together.
  C) Take the average of the gradients from all three paths.
  D) Only calculate the gradient for the path with the largest weight.
  
  ---
  
  **Section C: Short Answer & Open-Ended**
  
  **Q5. Activation Functions**
  Explain mathematically why the **Sigmoid** activation function ($\sigma(x)$) contributes heavily to the Vanishing Gradient problem in deep networks, compared to the **ReLU** activation function. *(Hint: Think about the maximum possible value of their derivatives).*
  
  **Q6. Architecture Design (Width vs. Depth)**
  When designing the architecture of a Neural Network, what is the conceptual difference between adding more **Width** (more neurons per layer) versus adding more **Depth** (more hidden layers) in terms of what the network "learns"?
  
  ---
  
  **Section D: Hand-Calculating Problems**
  
  **Q7. Forward Propagation**
  You are calculating the forward pass for a small Neural Network. 
  *   **Input Layer:** $x_1 = 0.4$, $x_2 = -0.2$
  *   **Hidden Layer:** Two nodes, $z_1$ and $z_2$, using the **ReLU** activation function.
  *   **Weights to Hidden Node 1 ($z_1$):** $w_{11} = 1.0$, $w_{21} = 0.5$
  *   **Weights to Hidden Node 2 ($z_2$):** $w_{12} = -1.0$, $w_{22} = 2.0$
  *   **Weights to Output ($\hat{y}$):** From $z_1$ to output is $w_{out1} = 2.0$. From $z_2$ to output is $w_{out2} = 1.0$.
  *   *(Assume all biases $b = 0$).*
  
  Calculate the final predicted output $\mathbf{\hat{y}}$. Show the intermediate raw scores and activated scores for the hidden layer.
  
  **Q8. Backward Propagation (The Chain Rule)**
  Continuing from **Q7**, the true target label for this data point is **$y = 1.6$**. 
  The loss function is standard squared error: $L = (y - \hat{y})^2$.
  Calculate the gradient of the loss with respect to the weight $w_{11}$ (the weight connecting $x_1$ to $z_1$).
  Find $\frac{\partial L}{\partial w_{11}}$. Show all steps of the chain rule.
  
  **Q9. Loss Functions (Cross-Entropy)**
  You are training a neural network for binary classification using Log-Loss (Cross-Entropy). The target label is $y = 1$.
  *   **Model A** predicts $\hat{y} = 0.99$
  *   **Model B** predicts $\hat{y} = 0.01$
  Write out the uncalculated Log-Loss equation for BOTH models based on this sample. Briefly explain why Model B will generate a massive gradient update.
  
  **Q10. Multi-Class Classification (SoftMax)**
  Your neural network is classifying an image into one of three classes: [Cat, Dog, Bird]. The network outputs the following raw linear scores (logits):
  *   $z_{cat} = 2$
  *   $z_{dog} = 1$
  *   $z_{bird} = 0$
  Calculate the exact **SoftMax probability** that the image is a Cat. *(You may leave your final answer as a fraction in terms of $e$).*
  
  ***
  ***
  *(Scroll down for the answers when you are ready!)*
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
### **Answer Key**
  
  **Q1. Universal Approximation**
  *   **Answer: False.** According to Cybenko's Universal Approximation Theorem (Slide 9), a neural network requires **at least one** hidden layer with a non-linear activation function to act as a universal approximator (assuming it has an arbitrary/sufficient width). It does not strictly require 10 layers.
  
  **Q2. Neural Network Limitations**
  *   **Answer: False.** Neural networks strictly require numerical matrices to do math. They are notoriously bad at handling missing attributes (NaNs) and will crash or propagate NaNs (Slide 42). Missing values must be explicitly imputed during preprocessing.
  
  **Q3. Diagnosing Network Issues**
  *   **Answer: C.** The phenomenon where early layers stop learning but later layers update fine is the **Vanishing Gradient** problem. Replacing ReLU with Sigmoid would actually make it *worse*. The correct fix is to use Skip Connections (ResNets) to give the gradient a "highway" backward, and use ReLU activation functions.
  
  **Q4. The "Multiple Paths" Chain Rule**
  *   **Answer: B.** You must **sum** the gradients from all paths. (Slide 23 explicitly states that local derivatives are shared and summed: $\frac{\partial L}{\partial x_1} = \frac{\partial L}{\partial h_1} + \frac{\partial L}{\partial h_2} \dots$).
  
  **Q5. Activation Functions**
  *   **Answer:** The derivative of the Sigmoid function evaluates to a maximum of **0.25**. During backpropagation, the Chain Rule forces us to multiply the local derivatives together. Multiplying numbers smaller than 0.25 repeatedly across many layers causes the gradient to shrink exponentially toward 0. ReLU, on the other hand, has a derivative of exactly **1.0** (for positive inputs), allowing the gradient to flow backward without shrinking.
  
  **Q6. Architecture Design (Width vs. Depth)**
  *   **Answer:** Increasing **Width** allows the network to "remember" more raw features (processing many patterns simultaneously at the same level of abstraction). Increasing **Depth** allows the network to learn a **hierarchy of abstractions** (e.g., Layer 1 learns edges $\to$ Layer 2 learns shapes $\to$ Layer 3 learns objects). (Slide 34).
  
  **Q7. Forward Propagation (Hand-Calculating)**
  *   **Hidden Node 1:**
    *   Raw score: $h_1 = (0.4 \times 1.0) + (-0.2 \times 0.5) = 0.4 - 0.1 = \mathbf{0.3}$
    *   Activation: $a_1 = \max(0, 0.3) = \mathbf{0.3}$
  *   **Hidden Node 2:**
    *   Raw score: $h_2 = (0.4 \times -1.0) + (-0.2 \times 2.0) = -0.4 - 0.4 = \mathbf{-0.8}$
    *   Activation: $a_2 = \max(0, -0.8) = \mathbf{0}$ *(The ReLU turns negative inputs to 0!)*
  *   **Final Output:**
    *   $\hat{y} = (a_1 \times w_{out1}) + (a_2 \times w_{out2}) = (0.3 \times 2.0) + (0 \times 1.0) = \mathbf{0.6}$
  
  **Q8. Backward Propagation (Hand-Calculating)**
  *   **Step 1: Set up the Chain Rule:** 
    $\frac{\partial L}{\partial w_{11}} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial a_1} \cdot \frac{\partial a_1}{\partial h_1} \cdot \frac{\partial h_1}{\partial w_{11}}$
  *   **Step 2: Find Local Derivatives:**
    1.  Loss: $\frac{\partial L}{\partial \hat{y}} = -2(y - \hat{y}) = -2(1.6 - 0.6) = -2(1.0) = \mathbf{-2}$
    2.  Output: $\frac{\partial \hat{y}}{\partial a_1} = w_{out1} = \mathbf{2}$
    3.  Activation: Because the raw score $h_1$ (0.3) was positive, the derivative of ReLU is **1**.
    4.  Input: $\frac{\partial h_1}{\partial w_{11}} = x_1 = \mathbf{0.4}$
  *   **Step 3: Multiply:**
    $\frac{\partial L}{\partial w_{11}} = -2 \times 2 \times 1 \times 0.4 = \mathbf{-1.6}$
  
  **Q9. Loss Functions (Cross-Entropy)**
  *   **Formula:** $L = -[y \log(\hat{y}) + (1 - y)\log(1 - \hat{y})]$
  *   Since $y=1$, the right half cancels out: $L = -\log(\hat{y})$.
  *   **Model A:** $L = -\log(0.99)$ *(Very close to 0 loss).*
  *   **Model B:** $L = -\log(0.01)$
  *   **Explanation:** Model B is *confidently wrong*. The natural logarithm of a number very close to 0 is a massive negative number. The negative sign in the formula flips this to a massive positive loss. This huge error generates a massive gradient, violently forcing the network to update its weights so it never makes that mistake again.
  
  **Q10. Multi-Class Classification (SoftMax)**
  *   **Formula:** $P(z_i) = \frac{e^{z_i}}{\sum e^{z_j}}$
  *   **Step 1 (Exponentiate):** $e^2$, $e^1$, $e^0 = 1$.
  *   **Step 2 (Sum):** Denominator $= e^2 + e^1 + 1$.
  *   **Step 3 (Normalize):** The probability of Cat is $\mathbf{\frac{e^2}{e^2 + e^1 + 1}}$ (which mathematically is roughly $0.705$ or 70.5%).
- ---
- ---
- ---
### **Practice Questions: Lecture 16 (CNNs)**
  
  **Section A: True / False**
  
  **Q1. Pooling Layers**
  **True or False:** The primary purpose of a Max Pooling layer is to reduce the "depth" (number of channels/filters) of the network while perfectly preserving the exact spatial pixel coordinates of the detected features.
  
  **Q2. Weight Sharing**
  **True or False:** Because of "Weight Sharing," a CNN layer with ten $3 \times 3$ filters requires the exact same number of learnable parameters whether it is processing a $28 \times 28$ thumbnail or a $4000 \times 4000$ 4K-resolution image.
  
  ---
  
  **Section B: Multiple Choice**
  
  **Q3. The CNN Hall of Fame**
  Which breakthrough Convolutional Neural Network architecture famously solved the "Vanishing Gradient" problem, allowing engineers to successfully train networks that were 152 layers deep?
  A) AlexNet
  B) VGG
  C) GoogLeNet (Inception)
  D) ResNet
  
  **Q4. Transfer Learning Strategy**
  You are a researcher building a model to detect rare tumors in Medical X-Rays. You only have a **very small dataset** of 200 X-Rays. You download a massive ResNet-50 model that was pre-trained on ImageNet (a dataset of natural images like dogs, cars, and trees). 
  According to standard transfer learning practices, what is your best strategy?
  A) Freeze the entire network and only train a new linear classifier on the top layer.
  B) Fine-tune the top few layers of the network and freeze the rest.
  C) Fine-tune the entire network with a very low learning rate.
  D) You are in trouble. You cannot easily fine-tune this because the dataset is small and the domain is completely different. You must use data augmentation or collect more data.
  
  ---
  
  **Section C: Short Answer & Open-Ended**
  
  **Q5. The "Parameter Explosion" Problem**
  If you try to process a standard $700 \times 700$ pixel grayscale image using a traditional Fully Connected Neural Network (MLP) with just 500 hidden neurons in the first layer, what mathematical catastrophe occurs? Briefly explain how a Convolutional Layer prevents this.
  
  **Q6. Unboxing the Black Box**
  Your CNN predicts that an image contains an "Afghan Hound" with 99% confidence. However, you want to prove that the model actually looked at the dog, rather than just recognizing the grass in the background. Explain how the **Heat Map (Occlusion)** technique can visually prove this.
  
  **Q7. Translation Invariance**
  A CNN is said to have "Translation Invariance" (meaning it can recognize a cat whether it is in the top-left corner or the bottom-right corner of a photo). Which **two** specific operations/layers inside a CNN work together to create this property?
  
  ---
  
  **Section D: Hand-Calculating Problems**
  
  **Q8. The Convolution Operation**
  You are applying a $2 \times 2$ filter to a specific $2 \times 2$ patch of an image. 
  *   **Image Patch:** Top row `[3, 5]`, Bottom row `[1, 2]`.
  *   **Filter Weights:** Top row `[1, 0]`, Bottom row `[0, -2]`.
  Perform the element-wise convolution operation. What is the single scalar output value for this patch?
  
  **Q9. Output Size Calculation**
  An input image is $64 \times 64$ pixels. You apply a Convolutional Layer with:
  *   Filter Size ($F$) = $5 \times 5$
  *   Stride ($S$) = $3$
  *   Padding ($P$) = $2$
  **A)** Calculate the spatial size (Width $\times$ Height) of the resulting output feature map.
  **B)** If this layer contains **32 distinct filters**, what is the final 3D volume (Width $\times$ Height $\times$ Depth) of the output?
  
  **Q10. Parameter Counting**
  You are designing a Convolutional Layer. 
  *   The **input** to this layer is an RGB image with **3 channels**.
  *   You use **64 filters** in this layer.
  *   Each filter has a size of **$3 \times 3$**.
  Calculate the total number of learnable parameters (weights and biases) in this single Convolutional Layer. Show your work.
  
  ***
  ***
  *(Scroll down for the answers when you are ready!)*
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
### **Answer Key**
  
  **Q1. Pooling Layers**
  *   **Answer: False.** Max pooling does the exact opposite. It preserves the *depth* (number of filters) but shrinks the *spatial* dimensions (width and height). By taking the maximum value in a window and throwing away the rest, it inherently *loses* exact spatial coordinates in exchange for translation invariance.
  
  **Q2. Weight Sharing**
  *   **Answer: True.** A $3 \times 3$ filter always contains exactly 9 weights (+1 bias), regardless of the size of the image it slides across. This is why CNNs are infinitely scalable to large images.
  
  **Q3. The CNN Hall of Fame**
  *   **Answer: D (ResNet).** ResNet (Residual Networks) introduced "Skip Connections" (adding the input to the output of a layer), which created a highway for gradients to flow backward, solving the vanishing gradient problem.
  
  **Q4. Transfer Learning Strategy**
  *   **Answer: D.** This maps to the "Very Little Data / Very Different Dataset" quadrant on Slide 53. If you just train a linear classifier (A), it will fail because the "dog/car" feature extractors don't understand X-Rays. If you try to train the deep layers (B or C), the model will severely overfit because you only have 200 images. You are in a tough spot and must augment the data.
  
  **Q5. The "Parameter Explosion" Problem**
  *   **Answer:** A $700 \times 700$ image contains $490,000$ pixels. In a Fully Connected network, every pixel connects to every neuron. $490,000 \text{ inputs} \times 500 \text{ neurons} = \mathbf{245,000,000}$ weights in just the first layer! This causes instant overfitting and crushes computer memory. A CNN solves this by using a small sliding window (e.g., $5 \times 5$), so the layer only has to learn a handful of shared parameters.
  
  **Q6. Unboxing the Black Box**
  *   **Answer:** The occlusion (Heat Map) technique works by sliding a grey square over different parts of the image and checking the model's confidence. When the grey square covers the grass, the confidence stays at 99%. But when the grey square slides over the dog's face, the confidence plummets. A heat map is generated by plotting these confidence drops, literally glowing red over the most critical features (the dog's face) to prove the model is looking at the right thing.
  
  **Q7. Translation Invariance**
  *   **Answer:** 
    1. **Weight Sharing / Convolution:** Because the exact same filter scans the whole image, if it knows how to detect a cat ear in the top left, it will detect it in the bottom right.
    2. **Max Pooling:** By selecting the maximum value in a small window, it allows a feature to shift by a pixel or two in any direction without changing the output of the layer.
  
  **Q8. The Convolution Operation (Hand-Calculating)**
  *   **Step 1 (Element-wise multiplication):**
    *   Top Left: $3 \times 1 = 3$
    *   Top Right: $5 \times 0 = 0$
    *   Bottom Left: $1 \times 0 = 0$
    *   Bottom Right: $2 \times -2 = -4$
  *   **Step 2 (Sum):** $3 + 0 + 0 - 4 = \mathbf{-1}$.
  
  **Q9. Output Size Calculation (Hand-Calculating)**
  *   **Part A Formula:** $O = \lfloor\frac{N - F + 2P}{S}\rfloor + 1$
    *   $O = \lfloor\frac{64 - 5 + 2(2)}{3}\rfloor + 1$
    *   $O = \lfloor\frac{64 - 5 + 4}{3}\rfloor + 1$
    *   $O = \lfloor\frac{63}{3}\rfloor + 1 = 21 + 1 = \mathbf{22}$.
    *   The spatial size is **$22 \times 22$**.
  *   **Part B:** Since there are 32 filters, the depth of the output is 32. The final volume is **$22 \times 22 \times 32$**.
  
  **Q10. Parameter Counting (Hand-Calculating)**
  *   **Step 1 (Weights per filter):** Filter Height $\times$ Filter Width $\times$ Input Channels.
    *   $3 \times 3 \times 3 = \mathbf{27}$ weights.
  *   **Step 2 (Add Bias):** Every filter gets exactly 1 bias term.
    *   $27 + 1 = \mathbf{28}$ parameters per filter.
  *   **Step 3 (Multiply by total filters):** We are using 64 filters.
    *   $28 \times 64 = \mathbf{1,792}$ **total parameters.**
  
  ***
- ---
- ---
- ---
### **Practice Questions: Lecture 17 (LSTMs & RNNs)**
  
  **Section A: True / False**
  
  **Q1. LSTM Memory Structures**
  **True or False:** Standard LSTM networks use only one type of memory vector to store information and pass it across time steps.
  
  **Q2. Real-Time Applications**
  **True or False:** Because Bidirectional LSTMs (BiLSTMs) process data both forward and backward, they are the optimal architecture choice for real-time, live speech-to-text translation systems.
  
  ---
  
  **Section B: Multiple Choice**
  
  **Q3. Architecture Selection**
  You are building an AI model that reads a single photograph and generates a descriptive sentence (e.g., "A dog is running in the grass"). Which sequential modeling architecture should you use?
  A) Many-to-One
  B) One-to-Many
  C) Many-to-Many (Aligned)
  D) Many-to-Many (Encoder-Decoder)
  
  **Q4. The Vanishing Gradient in RNNs**
  In a standard Recurrent Neural Network (RNN), the hidden state is updated using the formula $h_t = \tanh(U x_t + W h_{t-1})$. During Backpropagation Through Time (BPTT), the gradient often vanishes to zero for long sequences. What is the fundamental mathematical cause of this?
  A) The use of the $\tanh$ activation function instead of the Sigmoid function.
  B) The fact that the weights $U$, $V$, and $W$ change at every single time step.
  C) The repeated multiplication of the exact same weight matrix $W$ across many time steps, where the eigenvalues of $W$ are less than 1.
  D) The network lacks a dedicated output gate.
  
  **Q5. Activation Functions in LSTMs**
  Inside an LSTM cell, the Forget, Input, and Output gates exclusively use the **Sigmoid** activation function, while the Candidate Cell state ($\tilde{C}_t$) uses the **$\tanh$** activation function. Why?
  A) Sigmoid prevents the exploding gradient problem better than $\tanh$.
  B) $\tanh$ is computationally faster for generating new data than Sigmoid.
  C) Sigmoid outputs values between 0 and 1 (acting as a percentage/valve), while $\tanh$ outputs values between -1 and 1 (allowing for both positive and negative state updates).
  D) They are mathematically identical; the distinction is just a historical convention.
  
  ---
  
  **Section C: Short Answer & Open-Ended**
  
  **Q6. The "Highway" of the Cell State**
  Explain mathematically or conceptually how the LSTM's **Cell State ($C_t$)** prevents the Vanishing Gradient problem compared to a standard RNN. Be sure to mention the specific mathematical operation used to update the cell state, and how the Forget Gate ($f_t$) plays a role during backpropagation.
  
  **Q7. The Extreme Forget Gate**
  Suppose your LSTM network learns weights such that the Forget Gate ($f_t$) outputs exactly **1.0** at every single time step for a very long document. 
  A) What does this mean conceptually for the network's memory? 
  B) What is the negative consequence (limitation) of this behavior?
  
  **Q8. The Three Gates**
  Briefly state the primary role/function of the three gates inside an LSTM cell:
  1. The Forget Gate
  2. The Input Gate
  3. The Output Gate
  
  ---
  
  **Section D: Hand-Calculating & Matrix Math**
  
  **Q9. Matrix Dimensions in an RNN**
  You are building a standard RNN. 
  *   Your input data ($x_t$) at each time step is a vector of size **10**.
  *   You design your hidden state memory vector ($h_t$) to be of size **25**.
  Recall the hidden state formula: $h_t = \tanh(U x_t + W h_{t-1})$.
  To make the matrix multiplication mathematically possible, what must the dimensions (Rows $\times$ Columns) of the weight matrix **$U$** and the weight matrix **$W$** be?
  
  **Q10. Tracing an RNN Forward Pass**
  You are hand-calculating the first time step ($t=1$) of a standard RNN.
  *   The initial hidden state is $h_0 =[0, 0]^T$.
  *   The input at time step 1 is $x_1 =[2, -1]^T$.
  *   The input weight matrix is $U = \begin{bmatrix} 0.5 & 0 \\ 0 & 1 \end{bmatrix}$.
  *   The hidden weight matrix is $W = \begin{bmatrix} 0.1 & 0.2 \\ 0.3 & 0.4 \end{bmatrix}$.
  *(Assume the bias $b = [0, 0]^T$)*.
  
  Calculate the **raw linear score** vector ($z_1$) *before* it is passed through the $\tanh$ activation function. (i.e., Calculate $z_1 = U x_1 + W h_0$). Show your work.
  
  ***
  ***
  *(Scroll down for the answers when you are ready!)*
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
### **Answer Key**
  
  **Q1. LSTM Memory Structures**
  *   **Answer: False.** LSTMs explicitly use **two** types of memory: the Short-Term memory (the Hidden State $h_t$) and the Long-Term memory (the Cell State $C_t$, also known as the conveyor belt).
  
  **Q2. Real-Time Applications**
  *   **Answer: False.** BiLSTMs require reading the sequence forward *and* backward. You cannot read a sequence backward until the speaker has completely finished talking. Therefore, BiLSTMs cannot be used for live, real-time streaming tasks. 
  
  **Q3. Architecture Selection**
  *   **Answer: B (One-to-Many).** The input is a single, non-sequential item (one image vector). The output is a sequence of vectors (words in a sentence).
  
  **Q4. The Vanishing Gradient in RNNs**
  *   **Answer: C.** Unlike Feed-Forward networks which have different weights for every layer, an RNN uses the *exact same* weight matrix $W$ at every time step. During Backpropagation Through Time (BPTT), the chain rule requires multiplying $W$ by itself $T$ times ($W^T$). If the eigenvalues of $W$ are $<1$, this repeated multiplication decays exponentially to zero, destroying the gradient.
  
  **Q5. Activation Functions in LSTMs**
  *   **Answer: C.** The Forget, Input, and Output gates act as physical "valves." A Sigmoid function bounds the output between 0 (completely shut/erase) and 1 (completely open/keep). The Candidate Cell generates the actual *data* to be written to memory. Because data can have a positive or negative impact on the network's understanding, it must use $\tanh$ (bounded between -1 and 1) to allow for subtraction from the cell state.
  
  **Q6. The "Highway" of the Cell State**
  *   **Answer:** In an LSTM, the long-term Cell State ($C_t$) is updated using element-wise **addition**: $C_t = (f_t \odot C_{t-1}) + \dots$ 
  Because of this addition, when the gradient flows backward during backpropagation ($\frac{\partial C_t}{\partial C_{t-1}}$), the derivative is simply the Forget Gate ($f_t$). As long as the network learns to set the Forget Gate close to 1 ($f_t \approx 1$), the gradient is multiplied by 1 at each step, allowing it to flow backward through time perfectly intact without vanishing.
  
  **Q7. The Extreme Forget Gate**
  *   **A)** If $f_t = 1.0$ at every step, the network never erases anything. It remembers the entire history of the document perfectly forever.
  *   **B)** The limitation is that the Cell State will become overly saturated with obsolete, irrelevant information from the distant past. If the topic of the document suddenly changes, the model will struggle to adapt because its memory is clogged with old data.
  
  **Q8. The Three Gates**
  *   **1. Forget Gate:** Decides what information from the past long-term memory ($C_{t-1}$) is no longer relevant and should be erased.
  *   **2. Input Gate:** Decides which specific parts of the newly generated candidate information ($\tilde{C}_t$) are important enough to be written onto the long-term memory conveyor belt.
  *   **3. Output Gate:** Decides which parts of the updated long-term memory ($C_t$) are relevant *right now* to produce the current short-term Hidden State ($h_t$).
  
  **Q9. Matrix Dimensions in an RNN**
  *   *Logic:* $U x_t$ must result in a vector of size 25. Since $x_t$ is size 10, $U$ must be **$25 \times 10$**.
  *   *Logic:* $W h_{t-1}$ must result in a vector of size 25. Since $h_{t-1}$ is size 25, $W$ must be **$25 \times 25$**.
  
  **Q10. Tracing an RNN Forward Pass (Hand-Calculating)**
  *   **Step 1:** Calculate $W h_0$.
    Because $h_0 = [0, 0]^T$, multiplying it by any matrix results in $[0, 0]^T$. The network has no past memory yet.
  *   **Step 2:** Calculate $U x_1$.
    $$ U x_1 = \begin{bmatrix} 0.5 & 0 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 2 \\ -1 \end{bmatrix} = \begin{bmatrix} (0.5 \times 2) + (0 \times -1) \\ (0 \times 2) + (1 \times -1) \end{bmatrix} = \begin{bmatrix} 1 \\ -1 \end{bmatrix} $$
  *   **Step 3:** Add them together.
    $z_1 = U x_1 + W h_0 = \begin{bmatrix} 1 \\ -1 \end{bmatrix} + \begin{bmatrix} 0 \\ 0 \end{bmatrix} = \mathbf{\begin{bmatrix} 1 \\ -1 \end{bmatrix}}$
    *(To finish the step in a real network, you would pass $[1, -1]^T$ through the $\tanh$ function to get the final $h_1$ vector).*
  
  ***
- ---
- ---
- ---
### **Practice Questions: Lecture 18 (Self-Supervised Learning)**
  
  **Section A: True / False**
  
  **Q1. Yann LeCun's Analogy**
  **True or False:** In Yann LeCun’s "Cake of Learning" analogy, Self-Supervised Learning (SSL) represents the "icing" on the cake because it provides a small, highly concentrated amount of human-provided information to refine the model's accuracy.
  
  **Q2. BYOL Architecture**
  **True or False:** The BYOL (Bootstrap Your Own Latent) architecture successfully avoids representation collapse by using an incredibly large batch size to generate millions of negative sample pairs.
  
  ---
  
  **Section B: Multiple Choice**
  
  **Q3. SimCLR Mechanics**
  In the SimCLR framework, how does the algorithm naturally generate the "Negative Pairs" required for the contrastive loss function without needing human intervention?
  A) By generating entirely random noise vectors and feeding them into the network.
  B) By treating every other augmented image in the current training minibatch as a negative sample.
  C) By using an Exponential Moving Average (EMA) teacher network to repel the student network.
  D) By downloading a separate dataset of known negative images.
  
  **Q4. BERT Pretext Tasks**
  Which of the following pretext tasks is/are used to train BERT (a purely Self-Supervised language model)?
  A) Next Sentence Prediction (NSP)
  B) Translating English sentences to French sentences
  C) Masked Language Modeling (MLM)
  D) Both A and C
  
  **Q5. Evaluating Representations**
  You have trained an SSL feature extractor on millions of images. You now want to evaluate the quality of the learned representations using the **Linear Evaluation Protocol**. What does this process entail?
  A) Unfreezing all layers of the network and training it on a massive labeled dataset.
  B) Freezing the entire network, attaching a single new linear layer to the end, and training *only* that new layer on a small labeled dataset.
  C) Passing the features through a linear PCA algorithm to compress them to 2D for visual inspection.
  D) Calculating the linear reconstruction error of the images.
  
  ---
  
  **Section C: Short Answer \& Math Intuition**
  
  **Q6. Contrastive Loss Geometry (Concept \& Math)**
  In Contrastive Learning, the model maps an image $x$ and its augmented positive pair $x^+$ into feature vectors, and calculates their similarity using **Cosine Similarity**. 
  A) In a perfectly trained model, what is the theoretical maximum value of this Cosine Similarity score for a positive pair? 
  B) Geometrically, what does this specific maximum value mean about the angle between the two feature vectors in the high-dimensional space?
  
  **Q7. The SSL Pipeline**
  The standard Self-Supervised Learning pipeline consists of two distinct phases. Briefly name and describe Phase 1 and Phase 2. Be sure to mention what type of data (labeled vs. unlabeled) is used in each phase.
  
  **Q8. The Role of the Projection Head**
  In the SimCLR architecture, the data flows: Input $\to$ Encoder ($f$) $\to$ Projection Head ($g$). 
  After the model is fully pre-trained and you are ready to use it for a downstream classification task, what do you do with the Projection Head ($g$)? Why?
  
  ---
  
  **Section D: Open-Ended \& Design**
  
  **Q9. The Flaw of Handcrafted Pretext Tasks**
  Before Contrastive Learning, researchers used handcrafted pretext tasks (like rotating an image and making the CNN guess the rotation angle, or cutting an image up and making the CNN solve the Jigsaw puzzle). 
  Why did the AI community largely abandon these handcrafted tasks in favor of Contrastive Learning? What specific flaw did these models suffer from?
  
  **Q10. Designing an SSL Task (From Slide 50 - Highly Testable!)**
  You have a massive, unlabeled **time-series dataset** of sensor readings (e.g., continuous heart rate and movement tracking from a smartwatch). 
  Design a self-supervised pretext task to learn useful features from this data. Explain your reasoning for why this task would force the model to learn meaningful representations of the sensor data.
  
  ***
  ***
  *(Scroll down for the answers when you are ready!)*
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
### **Answer Key**
  
  **Q1. Yann LeCun's Analogy**
  *   **Answer: False.** SSL represents the **Cake Base**, not the icing. Because SSL predicts parts of the data from other parts of the data, it provides *millions* of bits of feedback per sample (a massive foundation of knowledge). Supervised Learning is the "icing" (only 10-10,000 bits of info per label). 
  
  **Q2. BYOL Architecture**
  *   **Answer: False.** BYOL's massive breakthrough was that it relies on **NO negative pairs**. Instead of repelling negative samples to prevent collapse, it uses an asymmetric Student-Teacher architecture where the Teacher's weights are updated using a slow-moving average (EMA) of the Student's weights. (SimCLR is the architecture that requires large batch sizes for negative pairs).
  
  **Q3. SimCLR Mechanics**
  *   **Answer: B.** SimCLR uses the other images in the current batch. If the batch size is $N$, SimCLR augments them to create $2N$ images. For any given image, there is $1$ positive pair (its own augmentation) and $2N - 2$ negative pairs (the rest of the batch).
  
  **Q4. BERT Pretext Tasks**
  *   **Answer: D.** BERT uses both Masked Language Modeling (hiding 15% of the words and predicting them) and Next Sentence Prediction (guessing if Sentence B naturally follows Sentence A). 
  
  **Q5. Evaluating Representations**
  *   **Answer: B.** The Linear Evaluation Protocol proves that the SSL encoder successfully untangled the raw data into a highly organized, linearly separable format. If a "dumb" linear classifier can achieve high accuracy on top of your frozen features, your features are excellent.
  
  **Q6. Contrastive Loss Geometry (Concept \& Math)**
  *   **A)** The theoretical maximum for Cosine Similarity is **1.0**.
  *   **B)** A Cosine Similarity of 1.0 means the angle between the two high-dimensional vectors is exactly **0 degrees**. This means the network has learned to map both augmented versions of the image to the exact same directional location in the semantic space.
  
  **Q7. The SSL Pipeline**
  *   **Phase 1: Pre-training.** The model trains on a massive **unlabeled** dataset by solving a pretext task (using pseudo-labels generated from the data itself). This learns the general "visual commonsense" feature extractor.
  *   **Phase 2: Fine-Tuning.** A shallow classification head is attached to the trained feature extractor. The model is trained on the specific downstream task using a small amount of **labeled** data.
  
  **Q8. The Role of the Projection Head**
  *   **Answer:** You **throw it away**. Research showed that the Projection Head ($g$) learns features that are too specific to the contrastive pre-training task (e.g., it learns how to perfectly identify color distortions applied during augmentation). The Encoder ($f$) contains the pure, generalizable "visual commonsense," so we keep the Encoder and discard the Projection Head for downstream tasks.
  
  **Q9. The Flaw of Handcrafted Pretext Tasks**
  *   **Answer:** They suffer from **"Shortcut Learning."** Instead of learning true semantic meaning (e.g., what a dog actually is), the neural network takes the easiest mathematical path to solve the puzzle. For example, to solve a Jigsaw puzzle, the network might just look for continuous edge-pixel textures (like lines of fur) to match the tiles, rather than learning the actual shape of the animal. These shortcut features do not generalize well to real downstream tasks.
  
  **Q10. Designing an SSL Task (From Slide 50 - Highly Testable!)**
  *   *(Note: Any logical task works here, as long as you justify it. Here are two perfect exam answers based on the slides).*
  *   **Design 1 (Predictive/Masking):** I would mask out a 5-second window in the middle of the continuous smartwatch sensor reading and train an Autoencoder/LSTM to predict the missing waveform based on the surrounding data. 
    *   *Reasoning:* To accurately rebuild the missing heartbeat/movement data, the model is forced to learn the natural rhythmic cycles, periodicity, and expected resting baseline of the patient.
  *   **Design 2 (Contrastive):** I would take a 10-second window of the sensor data. I would create a "Positive Pair" by applying two augmentations: adding slight random static (noise) to one, and slightly scaling the amplitude of the other. I would treat a window from a completely different hour as the "Negative Pair."
    *   *Reasoning:* Using a contrastive loss like SimCLR forces the model to learn the core, underlying "shape" of a specific activity (like walking) while learning to ignore irrelevant sensor noise or slight variations in intensity. 
  
  ***
- ---
- ---
- ---
### **Practice Questions: Lecture 19 (Transformers)**
  
  **Section A: True / False**
  
  **Q1. Positional Encoding**
  **True or False:** The Self-Attention mechanism inside a Transformer natively understands the order of words in a sentence. We only add Positional Encoding to help the Feed-Forward layer process the output faster.
  
  **Q2. Decoder Masking**
  **True or False:** During the training of a Transformer for machine translation, removing the "Look-Ahead Mask" in the Decoder will cause the model to suffer from data leakage, rendering it useless for real-world inference.
  
  ---
  
  **Section B: Multiple Choice**
  
  **Q3. The Purpose of Scaling ($\sqrt{d_k}$)**
  In the Self-Attention formula, we divide the dot-product scores by $\sqrt{d_k}$. What specific mathematical crisis does this prevent?
  A) It prevents the dot products from growing too large, which would push the Softmax function into flat regions and cause vanishing gradients.
  B) It prevents the matrix dimensions from mismatching before they are multiplied by the Value ($V$) matrix.
  C) It prevents the Exploding Gradient problem by artificially clipping the weights of the network.
  D) It converts the raw scores into percentages that sum to 1.0.
  
  **Q4. Multi-Head vs. Single-Head Attention**
  Why do Transformers use Multi-Head Attention instead of just relying on one massive Single-Head Attention mechanism?
  A) It allows the model to process sequences of variable lengths.
  B) It allows the network to project words into multiple different semantic spaces simultaneously (e.g., one head learns grammar, another learns coreferences).
  C) It is the primary mechanism that prevents the model from looking at padding tokens.
  D) It allows the model to process data sequentially rather than in parallel.
  
  **Q5. Architectural Complexity**
  Compared to standard Recurrent Neural Networks (RNNs), how does the computational complexity of a Transformer's Self-Attention mechanism scale as the sequence length ($N$) grows?
  A) Both RNNs and Transformers scale linearly: $O(N)$.
  B) Transformers scale linearly $O(N)$, while RNNs scale quadratically $O(N^2)$.
  C) Transformers scale quadratically $O(N^2)$, making them highly expensive for very long documents.
  D) Transformers have a constant complexity $O(1)$ regardless of sequence length.
  
  ---
  
  **Section C: Short Answer & Open-Ended**
  
  **Q6. The "Dating App" Analogy (Q, K, V)**
  During Self-Attention, a word embedding is multiplied by three different weight matrices to create a **Query (Q)**, a **Key (K)**, and a **Value (V)**. Conceptually, what do each of these three vectors represent for a specific word?
  
  **Q7. The "Add & Norm" Block**
  Every sub-layer in the Transformer (like Self-Attention and the Feed Forward Network) is wrapped in an "Add & Norm" step. 
  A) What is the "Add" (Residual Connection) doing, and what specific problem does it solve?
  B) What does the "Norm" (Layer Normalization) do to help the network train?
  
  **Q8. Text Generation Strategies**
  When the Decoder is generating a translated sentence, it must pick one word at a time. Explain the difference between using a **Greedy Strategy** versus using **Beam Search**, and state which one generally produces better sentences.
  
  ---
  
  **Section D: Hand-Calculating Problems**
  
  **Q9. Self-Attention Math (Just like HW5)**
  You are given a tiny sequence of 2 words. The input embedding matrix $X$ is:
  $$ X = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} $$
  The learned weight matrices for Queries ($W_q$) and Keys ($W_k$) are:
  $$ W_q = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} \quad W_k = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} $$
  **Your Tasks:**
  A) Calculate the Query matrix $Q$ ($Q = X W_q$).
  B) Calculate the Key matrix $K$ ($K = X W_k$).
  C) Calculate the unscaled Attention Scores matrix $S$ ($S = Q K^T$). Show your work.
  
  **Q10. Cross-Attention vs. Self-Attention**
  In the Decoder block of a Transformer, there is a special layer called **Cross-Attention**. How does the origin of the $Q$, $K$, and $V$ matrices in Cross-Attention differ from standard Self-Attention? Briefly explain *why* it is structured this way for a task like translation.
  
  ***
  ***
  *(Scroll down for the answers when you are ready!)*
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
### **Answer Key**
  
  **1. Positional Encoding**
  *   **Answer: False.** Self-Attention is strictly **permutation-invariant**; it treats sentences like a "bag of words" and calculates dot products simultaneously. It has zero concept of order. We *must* inject Positional Encoding directly into the input embeddings so the model knows where the words physically sit in the sentence.
  
  **2. Decoder Masking**
  *   **Answer: True.** During training, we feed the entire target sentence into the Decoder at once for efficiency. If we do not apply the triangular "Look-Ahead" mask, the self-attention mechanism will just look at the next word in the sequence to predict the next word. This is catastrophic data leakage, and the model will fail at inference time when future words haven't been generated yet.
  
  **3. The Purpose of Scaling ($\sqrt{d_k}$)**
  *   **Answer: A.** In high dimensions, the dot product of two vectors can result in massive numbers. If you feed massive numbers (e.g., 100, -50) into a Softmax function, it gets "pushed into the corners," outputting probabilities of exactly 1.0 and 0.0. The gradients in these flat corners are exactly 0, which completely kills backpropagation. Dividing by $\sqrt{d_k}$ keeps the numbers small and the Softmax "soft."
  
  **4. Multi-Head vs. Single-Head Attention**
  *   **Answer: B.** A single attention head can only focus on one type of relationship (e.g., matching a pronoun "it" to a noun "animal"). Multi-head attention slices the vectors into chunks and lets the model look for multiple distinct semantic relationships (syntax, local context, global context) entirely in parallel.
  
  **5. Architectural Complexity**
  *   **Answer: C.** Because every single word must calculate a dot-product attention score with *every other word* in the sequence, the complexity grows quadratically: **$O(N^2)$**. This is why Transformers require massive amounts of VRAM to process very long books or documents. 
  
  **6. The "Dating App" Analogy (Q, K, V)**
  *   **Query (Q):** "What am I looking for?" (e.g., the word "it" is searching for a noun).
  *   **Key (K):** "What do I offer? / How can I be matched?" (e.g., the word "animal" advertises its profile as a singular noun).
  *   **Value (V):** "What is my actual underlying meaning/content?" (This is the payload that gets extracted and blended into the final output vector if the Query and Key match well).
  
  **7. The "Add & Norm" Block**
  *   **A) Add (Residual Connection):** It takes the input and adds it directly to the output of the layer ($x + \text{layer}(x)$). This provides a "highway" for gradients to flow backward through time, completely preventing the **Vanishing Gradient** problem in networks with dozens of layers.
  *   **B) Norm (Layer Normalization):** It standardizes the outputs so they have a stable mean and variance. This prevents the numbers from wildly shifting, allowing the model to train faster and generalize better.
  
  **8. Text Generation Strategies**
  *   **Greedy Strategy:** At each step, it simply picks the single word with the highest Softmax probability. It is fast but cannot "look ahead," often leading to grammatically dead-end sentences.
  *   **Beam Search:** It keeps multiple candidate sentences (e.g., the top 3 paths) alive in memory at the same time. It explores all 3 paths, calculates their total sequence probabilities, and keeps the best ones. It generally produces much better, more coherent translations.
  
  **9. Self-Attention Math (Hand-Calculating)**
  *   **A) Calculate Q:** 
    $$ Q = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} = \mathbf{\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}} $$ *(Because $W_q$ is the identity matrix, $Q=X$).*
  *   **B) Calculate K:**
    $$ K = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} (1\cdot1+1\cdot0) & (1\cdot1+1\cdot1) \\ (0\cdot1+1\cdot0) & (0\cdot1+1\cdot1) \end{bmatrix} = \mathbf{\begin{bmatrix} 1 & 2 \\ 0 & 1 \end{bmatrix}} $$
  *   **C) Calculate Scores ($S = QK^T$):**
    First, transpose K: $K^T = \begin{bmatrix} 1 & 0 \\ 2 & 1 \end{bmatrix}$.
    $$ S = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 \\ 2 & 1 \end{bmatrix} = \begin{bmatrix} (1\cdot1+1\cdot2) & (1\cdot0+1\cdot1) \\ (0\cdot1+1\cdot2) & (0\cdot0+1\cdot1) \end{bmatrix} = \mathbf{\begin{bmatrix} 3 & 1 \\ 2 & 1 \end{bmatrix}} $$
  
  **10. Cross-Attention vs. Self-Attention**
  *   **Answer:** In standard Self-Attention, Q, K, and V all come from the exact same sequence (e.g., the English sentence looks at itself). In **Cross-Attention**, the **Query (Q)** comes from the *Decoder* (representing what the partially translated French sentence needs next), while the **Keys (K)** and **Values (V)** come from the output of the *Encoder* (representing the rich context of the original English sentence). This is how the Decoder "looks across" to read the Encoder's mind during translation!
  
  ***
- ---
- ---
- ---
### **Practice Questions: Lecture 20 (Outlier Detection)**
  
  **Section A: True / False**
  
  **Q1. Noise vs. Anomalies**
  **True or False:** In data mining, "noise" and "anomalies" are interchangeable terms. Both represent rare, meaningful deviations from the majority of the data that must be carefully detected and investigated.
  
  **Q2. Video Predictability Modeling**
  **True or False:** When using Deep Learning to detect anomalies in video feeds (Predictability Modeling), the model triggers an alarm when it successfully and perfectly predicts the exact pixels of the next frame.
  
  ---
  
  **Section B: Multiple Choice**
  
  **Q3. Types of Outliers**
  A bank customer typically spends about \$50 to \$100 at the grocery store every week. While travelling abroad, their card is charged \$5,000 for a luxury watch. Which specific type of anomaly does this single transaction represent?
  A) Contextual Anomaly
  B) Point Anomaly
  C) Collective Anomaly
  D) Algorithmic Anomaly
  
  **Q4. One-Class SVM Geometry**
  When using the Support Vector Data Description (SVDD) method for One-Class SVM anomaly detection, what is the geometric objective of the algorithm during training?
  A) To find a linear hyperplane that maximizes the distance between the normal class and the origin.
  B) To cluster the data into exactly $K$ dense regions.
  C) To find the smallest possible mathematical sphere that completely encloses the normal data.
  D) To find a line that minimizes the sum of squared errors.
  
  **Q5. Evaluation Metrics**
  You are evaluating a model that detects factory machine failures. 99.9% of the historical logs are "Normal" and 0.1% are "Failures." According to your professor's slides, which metric is the **most preferred** to evaluate the model's success threshold given this extreme imbalance?
  A) Accuracy
  B) ROC-AUC (Receiver Operating Characteristic)
  C) PR-AUC (Precision-Recall Area Under Curve)
  D) Mean Squared Error (MSE)
  
  ---
  
  **Section C: Short Answer & Open-Ended**
  
  **Q6. Learning Settings**
  Most real-world anomaly detection systems (like fraud detection or predictive maintenance) use a **Semi-Supervised** learning setting rather than a Fully Supervised setting. 
  A) In a Semi-Supervised anomaly setup, what kind of labels/data do you have access to during training? 
  B) Why is it usually impossible to use a Fully Supervised setting for anomaly detection?
  
  **Q7. The Reconstruction Trap (Autoencoders)**
  You train a Deep Autoencoder to detect fraudulent network traffic. To make sure the model is "smart enough," you make the bottleneck (latent space) incredibly wide—almost the same size as the input data. During testing, the model fails to flag obvious cyberattacks. 
  Explain the concept of **"Over-generalization"** and why making the bottleneck too wide caused the anomaly detector to fail.
  
  ---
  
  **Section D: Hand-Calculating Problems**
  
  **Q8. The IQR Method**
  You are analyzing server response times (in milliseconds) to detect anomalous spikes. You calculate the 25th percentile ($Q_1$) to be **20 ms** and the 75th percentile ($Q_3$) to be **50 ms**. 
  A new request takes **100 ms** to process. 
  A) Calculate the Interquartile Range (IQR).
  B) Calculate the Upper Bound for normal data. 
  C) Mathematically determine if the 100 ms request is an outlier.
  
  **Q9. Local Outlier Factor (LOF)**
  You are using the LOF algorithm to find anomalies in a dataset of overlapping clusters. You are evaluating a specific data point, $X$.
  *   The Local Reachability Density (LRD) of point $X$ is calculated to be **0.2** (meaning the area around $X$ is very sparse).
  *   The *average* LRD of $X$'s $K$-nearest neighbors is calculated to be **2.0** (meaning $X$'s neighbors are sitting in a very dense cluster).
  A) Calculate the final LOF score for point $X$.
  B) Based on this score, is point $X$ considered a normal point or an outlier? Why?
  
  **Q10. Time-Series Moving Average**
  You use a Moving Average with a window size of $k=4$ to detect time-series anomalies in a patient's heart rate. The heart rates for the last 4 minutes were: **[70, 72, 75, 71]**. 
  At minute 5, the patient's heart rate spikes to **95**.
  A) Calculate the expected (smoothed) value ($\hat{x}_5$) for minute 5.
  B) Calculate the Residual Error ($e_5$) for minute 5.
  
  ***
  ***
  *(Scroll down for the answers when you are ready!)*
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
### **Answer Key**
  
  **1. Noise vs. Anomalies**
  *   **Answer: False.** They are completely different. Noise consists of random, meaningless fluctuations (like sensor static) that should be ignored or smoothed out. Anomalies are rare but *meaningful* deviations (like a cyberattack or heart attack) that must be flagged.
  
  **2. Video Predictability Modeling**
  *   **Answer: False.** The model triggers an alarm when it **fails** to predict the next frame. The core assumption of predictability modeling is that *normal data is predictable*. If a car drives on a sidewalk (anomaly), the model will predict pedestrian movement, completely failing to reconstruct the car. The resulting high reconstruction error triggers the alarm.
  
  **3. Types of Outliers**
  *   **Answer: B (Point Anomaly).** A single, isolated data point (one transaction) is vastly different from the rest of the historical distribution. (If they spent \$5,000 buying 5,000 individual \$1 candy bars in two hours, that would be a Collective Anomaly).
  
  **4. One-Class SVM Geometry**
  *   **Answer: C.** The SVDD method for One-Class SVMs seeks to find the smallest possible mathematical sphere that completely encloses the normal training data. During inference, points falling outside the sphere are flagged as outliers.
  
  **5. Evaluation Metrics**
  *   **Answer: C (PR-AUC).** While ROC-AUC is decent, PR-AUC is heavily preferred for severe imbalance. ROC includes False Positives divided by True Negatives; if you have millions of normal samples (True Negatives), the ROC curve looks artificially perfect. Precision-Recall focuses strictly on how well you find the rare positive class.
  
  **6. Learning Settings**
  *   **A)** You only have access to **Normal** data during training. You train the model to understand what "normality" looks like.
  *   **B)** Fully Supervised learning requires an equal, perfectly labeled dataset of Normal and Anomaly classes. Anomalies are historically too rare, too ambiguous, and too difficult/expensive to label to build a balanced dataset.
  
  **7. The Reconstruction Trap (Autoencoders)**
  *   **Answer:** An Autoencoder detects anomalies by failing to reconstruct them (resulting in high error). However, if the bottleneck is too wide, the network doesn't actually learn the *patterns* of the data; it just memorizes an "identity function" (copying inputs directly to outputs). This causes **over-generalization**: the model becomes so powerful it can perfectly reconstruct *everything*, including the cyberattacks! Because it reconstructs the anomaly perfectly, the error is 0, and the attack slips right past the detector.
  
  **8. The IQR Method (Hand-Calculating)**
  *   **A) IQR:** $Q_3 - Q_1 = 50 - 20 = \mathbf{30}$.
  *   **B) Upper Bound:** $Q_3 + 1.5(\text{IQR}) = 50 + 1.5(30) = 50 + 45 = \mathbf{95}$.
  *   **C) Conclusion:** Since the 100 ms request is greater than the Upper Bound of 95 ms ($100 > 95$), it is mathematically classified as an **outlier**.
  
  **9. Local Outlier Factor (Hand-Calculating)**
  *   **A) Score:** $\text{LOF} = \frac{\text{Average LRD of Neighbors}}{\text{LRD of Point X}} = \frac{2.0}{0.2} = \mathbf{10.0}$.
  *   **B) Conclusion:** Point $X$ is a massive **Outlier**. An LOF score $\approx 1$ means a point is normal (same density as its neighbors). An LOF $\gg 1$ (like 10) means the point is sitting in a sparse, empty region, while its nearest neighbors are clumped tightly together far away.
  
  **10. Time-Series Moving Average (Hand-Calculating)**
  *   **A) Expected Value ($\hat{x}_5$):** The average of the window $k=4$.
    $\hat{x}_5 = (70 + 72 + 75 + 71) / 4 = 288 / 4 = \mathbf{72}$.
  *   **B) Residual Error ($e_5$):** The difference between the actual value and the expected value.
    $e_5 = x_5 - \hat{x}_5 = 95 - 72 = \mathbf{23}$. *(This massive residual of 23 would trigger the anomaly alert).*
  
  ***