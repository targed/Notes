# Lecture 15: Neural Networks (NN)
### 1. Example: Forward Propagation
  *   **Slide:** 13
  *   **Category:** Network Architecture / Forward Pass
  *   **Problem Statement:** 
    Given a neural network with 3 inputs ($x_1=0.1, x_2=0.5, x_3=0.3$) and 2 hidden nodes ($z_1, z_2$) using the ReLU activation function. The weights connecting all inputs to the hidden nodes are $1$. The weights connecting $z_1$ and $z_2$ to the output node are $0.5$ and $0.3$, respectively. What is the final output $\hat{y}$?
  *   **Solution / Steps:**
    1.  **Calculate raw scores for the hidden layer ($z_1$ and $z_2$):**
        *   $z_1 = (0.1 \times 1) + (0.5 \times 1) + (0.3 \times 1) = 0.9$
        *   $z_2 = (0.1 \times 1) + (0.5 \times 1) + (0.3 \times 1) = 0.9$
    2.  **Apply the ReLU Activation Function:**
        *   $\text{ReLU}(z_1) = \max(0, 0.9) = 0.9$
        *   $\text{ReLU}(z_2) = \max(0, 0.9) = 0.9$
    3.  **Calculate the Final Output ($\hat{y}$):**
        *   $\hat{y} = (z_1 \times 0.5) + (z_2 \times 0.3)$
        *   $\hat{y} = (0.9 \times 0.5) + (0.9 \times 0.3) = 0.45 + 0.27 = \mathbf{0.72}$
### 2. Loss Function (Cont’d) - Binary Classification
  *   **Slide:** 15
  *   **Category:** Cross-Entropy Loss / Log Loss
  *   **Problem Statement:** 
    Using the Cross-Entropy Loss formula $L = -[y \log(\hat{y}) + (1 - y)\log(1 - \hat{y})]$, evaluate the loss for the following scenarios:
    1. Ground Truth $y=1$. What is the loss if $\hat{y} = 0.9$? What if $\hat{y} = 0.1$?
    2. Ground Truth $y=0$. What is the loss if $\hat{y} = 0.9$? What if $\hat{y} = 0.1$?
  *   **Solution / Steps:**
    1.  **For $y = 1$:** The formula simplifies to $L = -\log(\hat{y})$.
        *   If $\hat{y} = 0.9 \implies L = -\log(0.9) \approx \mathbf{0.05}$ (Low penalty)
        *   If $\hat{y} = 0.1 \implies L = -\log(0.1) \approx \mathbf{2.3}$ (High penalty)
    2.  **For $y = 0$:** The formula simplifies to $L = -\log(1 - \hat{y})$.
        *   If $\hat{y} = 0.9 \implies L = -\log(1 - 0.9) = -\log(0.1) \approx \mathbf{2.3}$ (High penalty)
        *   If $\hat{y} = 0.1 \implies L = -\log(1 - 0.1) = -\log(0.9) \approx \mathbf{0.05}$ (Low penalty)
### 3. Example (1): Full Forward Pass & Loss Calculation
  *   **Slide:** 29
  *   **Category:** Forward Propagation & Loss
  *   **Problem Statement:** 
    Given the input $x =[1, 0.5]^T$ and ground truth label $y = 4$. The activation function is $\sigma(x) = \text{ReLU}(x)$. The weights are:
    $w_1 = 0.5, w_2 = 1.5, w_3 = 2.3, w_4 = 3, w_5 = 1, w_6 = 1$.
    Calculate the intermediate hidden states ($h_1, h_2$), their activations ($O_1, O_2$), the final prediction $\hat{y}$, and the final Loss defined as $L = (y - \hat{y})^2$.
  *   **Solution / Steps:**
    1.  $h_1 = w_1 x_1 + w_2 x_2 = (0.5 \times 1) + (1.5 \times 0.5) = 0.5 + 0.75 = \mathbf{1.25}$
    2.  $O_1 = \sigma(h_1) = \text{ReLU}(1.25) = \mathbf{1.25}$
    3.  $h_2 = w_3 x_1 + w_4 x_2 = (2.3 \times 1) + (3 \times 0.5) = 2.3 + 1.5 = \mathbf{3.8}$
    4.  $O_2 = \sigma(h_2) = \text{ReLU}(3.8) = \mathbf{3.8}$
    5.  $h_3 = w_5 O_1 + w_6 O_2 = (1 \times 1.25) + (1 \times 3.8) = \mathbf{5.05}$
    6.  $\hat{y} = h_3 = \mathbf{5.05}$
    7.  $Loss = (y - \hat{y})^2 = (4 - 5.05)^2 = (-1.05)^2 = \mathbf{1.1025}$
### 4. Example (2): Backward Propagation (Output Layer Weight)
  *   **Slide:** 30
  *   **Category:** Backpropagation / Chain Rule
  *   **Problem Statement:** 
    Using the network values calculated in Example (1), use the Chain Rule to calculate the gradient of the loss with respect to $w_5$ ($\frac{\partial L}{\partial w_5}$).
  *   **Solution / Steps:**
    1.  **Chain Rule Setup:** $\frac{\partial L}{\partial w_5} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h_3} \cdot \frac{\partial h_3}{\partial w_5}$
    2.  **Calculate Local Derivatives:**
        *   $\frac{\partial L}{\partial \hat{y}} = \frac{\partial}{\partial \hat{y}} (y - \hat{y})^2 = -2(y - \hat{y})$
        *   $\frac{\partial \hat{y}}{\partial h_3} = 1$ (Since $\hat{y} = h_3$)
        *   $\frac{\partial h_3}{\partial w_5} = O_1$ (Since $h_3 = w_5 O_1 + w_6 O_2$)
    3.  **Plug in the numbers:**
        *   $-2(4 - 5.05) \times 1 \times 1.25$
        *   $-2(-1.05) \times 1.25 = 2.1 \times 1.25 = \mathbf{2.625}$
### 5. Example (3): Backward Propagation (Input Layer Weight)
  *   **Slide:** 31
  *   **Category:** Backpropagation / Chain Rule
  *   **Problem Statement:** 
    Using the network values from Example (1) and the chain rule logic from Example (2), calculate the gradient of the loss with respect to an input-layer weight $w_1$ ($\frac{\partial L}{\partial w_1}$).
  *   **Solution / Steps:**
    1.  **Chain Rule Setup (5 links):** 
        $\frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h_3} \cdot \frac{\partial h_3}{\partial O_1} \cdot \frac{\partial O_1}{\partial h_1} \cdot \frac{\partial h_1}{\partial w_1}$
    2.  **Calculate Local Derivatives:**
        *   $\frac{\partial L}{\partial \hat{y}} = -2(y - \hat{y}) = -2(4 - 5.05) = \mathbf{2.1}$
        *   $\frac{\partial \hat{y}}{\partial h_3} = \mathbf{1}$
        *   $\frac{\partial h_3}{\partial O_1} = w_5 = \mathbf{1}$
        *   $\frac{\partial O_1}{\partial h_1} = \text{ReLU}'(h_1)$. Since $h_1 (1.25) > 0$, the slope is $\mathbf{1}$.
        *   $\frac{\partial h_1}{\partial w_1} = x_1 = \mathbf{1}$
    3.  **Multiply them together:**
        *   $2.1 \times 1 \times 1 \times 1 \times 1 = \mathbf{2.1}$
  
  ***
- ---
- ---
- ---
# Lecture 16: Convolutional Neural Networks (CNN)
### 1. Example: Convolution Operation (Element-wise Math)
  *   **Slides:** 10 \& 16
  *   **Category:** Convolutional Layer Mechanics
  *   **Problem Statement:** 
    You apply a $3 \times 3$ edge-detection filter to an image. The filter values are:
    Top row: $[+1, +2, +1]$
    Middle row: $[0, 0, 0]$
    Bottom row: $[-1, -2, -1]$
    The filter is currently hovering over a $3 \times 3$ patch of the image where the top and middle rows are pure black (pixel value `0`) and the bottom row is pure white (pixel value `255`). What is the single output value of this convolution step?
  *   **Solution / Steps:**
    Perform element-wise multiplication and sum the results.
    1.  **Top row:** $(0 \times 1) + (0 \times 2) + (0 \times 1) = 0$
    2.  **Middle row:** $(0 \times 0) + (0 \times 0) + (0 \times 0) = 0$
    3.  **Bottom row:** $(255 \times -1) + (255 \times -2) + (255 \times -1) = -255 - 510 - 255 = \mathbf{-1020}$
    *   **Output:** The final value is **-1020**, indicating a strong horizontal edge was detected.
### 2. Example: Pooling Layers (Max and Average)
  *   **Slides:** 33 \& 35
  *   **Category:** Pooling Mechanics
  *   **Problem Statement:** 
    You have a $2 \times 2$ patch of a feature map with the following values:
    Top row: $[1, 1]$
    Bottom row: $[5, 6]$
    What is the output of this patch if you apply **Average Pooling**? What if you apply **Max Pooling**?
  *   **Solution / Steps:**
    *   **Average Pooling:** Sum the values and divide by 4. $(1 + 1 + 5 + 6) / 4 = 13 / 4 = \mathbf{3.25}$.
    *   **Max Pooling:** Select the absolute largest value in the window. The maximum is $\mathbf{6}$.
### 3. Example: Transfer Learning / Fine-Tuning Scenarios
  *   **Slide:** 53
  *   **Category:** Transfer Learning
  *   **Problem Statement:** 
    You are using a pre-trained model (like ResNet trained on ImageNet) for a new task. What is the correct fine-tuning strategy for the following four scenarios?
    1. Very little data, very similar dataset.
    2. Quite a lot of data, very similar dataset.
    3. Very little data, very different dataset.
    4. Quite a lot of data, very different dataset.
  *   **Solution:**
    1.  **Fine-tune linear classifier on top layer:** Freeze the whole network and only train a new classification head.
    2.  **Fine-tune a few layers:** Unfreeze the top few layers of the CNN to adapt to your specific data, freeze the early layers.
    3.  **You're in trouble:** Try data augmentation or collecting more data (you can't train a deep network on a tiny, vastly different dataset without severe overfitting).
    4.  **Fine-tune a larger number of layers:** Unfreeze most/all of the network, but train with a low learning rate so you don't destroy the generic edge-detecting weights.
### 4. After-Class Question 5: Output Size Formula
  *   **Slide:** 66
  *   **Category:** CNN Architecture Math
  *   **Problem Statement:** 
    Given an Input image of $28 \times 28$, a Filter of size $3 \times 3$, a Stride of $1$, and a Padding of $1$. 
    (1) Compute the output size. 
    (2) If there are 10 filters, what is the output dimension?
  *   **Solution / Steps:**
    1.  **Formula:** $\text{Output Size} = \frac{\text{Input} - \text{Filter} + 2(\text{Padding})}{\text{Stride}} + 1$
        *   Calculation: $\frac{28 - 3 + 2(1)}{1} + 1 = \frac{27}{1} + 1 = \mathbf{28 \times 28}$.
    2.  Each filter produces exactly 1 feature map (channel). Therefore, 10 filters produce a depth of 10. The final dimension is $\mathbf{28 \times 28 \times 10}$.
### 5. After-Class Question 6: Parameter Counting (Single Layer)
  *   **Slide:** 66
  *   **Category:** CNN Parameter Math
  *   **Problem Statement:** 
    A convolutional layer has 16 filters of size $3 \times 3$. The input to this layer has 3 channels (RGB). Compute the total number of learnable parameters for this layer.
  *   **Solution / Steps:**
    1.  **Formula:** $\text{Params} = (\text{Filter Height} \times \text{Filter Width} \times \text{Input Channels} + \text{Bias}) \times \text{Number of Filters}$
    2.  Calculate weights per filter: $(3 \times 3 \times 3) = 27$ weights.
    3.  Add the bias term (1 per filter): $27 + 1 = 28$ parameters per filter.
    4.  Multiply by total filters: $28 \times 16 = \mathbf{448 \text{ parameters}}$.
### 6. After-Class Question 7: Parameter Counting (Full Network)
  *   **Slide:** 66
  *   **Category:** CNN Parameter Math
  *   **Problem Statement:** 
    Assume a CNN model with the following structure:
    *   **Input:** $28 \times 28 \times 1$
    *   **Conv\_1:** $n1=16$ filters of size $5 \times 5$.
    *   **Max-Pooling**
    *   **Conv\_2:** $n2=32$ filters of size $5 \times 5$.
    *   **Max-Pooling**
    *   **Flatten:** Flattens to a 1D vector of size $n3 = 4 \times 4 \times n2$.
    *   **Output Layer:** Fully connected layer to $10$ output units.
    Compute the total number of parameters in this CNN model.
  *   **Solution / Steps:**
    1.  **Conv\_1:** Input has 1 channel.
        $((5 \times 5 \times 1) + 1) \times 16 = (25 + 1) \times 16 = \mathbf{416}$ parameters.
    2.  **Conv\_2:** Input has 16 channels (from $n1$).
        $((5 \times 5 \times 16) + 1) \times 32 = (400 + 1) \times 32 = \mathbf{12,832}$ parameters.
    3.  **Output Layer (FC):** Input size is $n3 = 4 \times 4 \times 32 = 512$ neurons. Output size is $10$ neurons.
        $(\text{Input Neurons} \times \text{Output Neurons}) + \text{Biases}$
        $(512 \times 10) + 10 = 5,120 + 10 = \mathbf{5,130}$ parameters.
    4.  **Total:** $416 + 12,832 + 5,130 = \mathbf{18,378 \text{ parameters}}$. *(Note: Pooling layers and Flatten operations have 0 learnable parameters).*
### 7. After-Class Questions (Concepts Q1–Q4, Q8–Q9)
  *   **Slides:** 65 \& 67
  *   **Category:** CNN Theory Review
  *   **Q1: Key difference between Fully Connected and CNN? Why are CNNs good for images?**
    *   *Answer:* Fully connected layers connect every input to every neuron, destroying 2D spatial relationships and causing a parameter explosion. CNNs use sliding filters that process small patches of pixels, preserving spatial structure and drastically reducing parameters.
  *   **Q2: What is weight sharing? Why does it reduce parameters?**
    *   *Answer:* A single filter (e.g., $3 \times 3 = 9$ weights) is slid across the entire image. Instead of learning thousands of different weights for every pixel, the network "shares" those same 9 weights everywhere, drastically reducing the total parameter count.
  *   **Q3: Purpose of pooling layers? What is lost?**
    *   *Answer:* Pooling reduces the spatial dimensions (width and height), which reduces computational load, prevents overfitting, and provides slight translation invariance. It loses *exact spatial location* (e.g., it knows an eye exists in a $2 \times 2$ patch, but forgets exactly which pixel it was on).
  *   **Q4: What is a feature map? How does it evolve?**
    *   *Answer:* A feature map is the 2D output grid generated by applying a filter to an input. Across deeper layers, feature maps usually shrink spatially (due to pooling) but grow in depth (channels). Early feature maps detect simple edges; deep feature maps detect complex objects (like faces).
  *   **Q8: High training accuracy, low test accuracy. Likely issue? Three solutions?**
    *   *Answer:* The issue is **Overfitting**. Solutions: 1) Data Augmentation (flip/crop images), 2) Dropout layers, 3) Early Stopping (or using a pre-trained model / Transfer Learning).
  *   **Q9: What is translation invariance? Which components contribute?**
    *   *Answer:* It means the network can recognize a feature (like a cat) regardless of where it is shifted in the image. Both **Weight Sharing (Convolution)** and **Pooling Layers** contribute to this property.
  
  ***
- ---
- ---
- ---
# Lecture 17: Long Short-Term Memory (LSTM)
### 1. Conceptual Example: Contextual Dependency
  *   **Slide:** 3
  *   **Category:** Sequential Data Properties
  *   **Problem Statement:** 
    Consider the sentence: "I am very hungry, and I can eat \_\_\_\_\_\_". The choices to fill in the blank are {"a lot", "nothing"}. How do we know which is correct, and why do traditional Feed Forward Networks (FFNs) fail at this?
  *   **Solution / Takeaway:** 
    The correct answer ("a lot") depends entirely on the earlier context ("very hungry"). Traditional FFNs fail because they process inputs in isolation and have **no memory** of past inputs. Recurrent Neural Networks (RNNs) solve this by maintaining a hidden state that carries historical context forward in time.
### 2. Mathematical Example: Exploding/Vanishing Gradients in RNNs
  *   **Slide:** 16
  *   **Category:** Backpropagation Through Time (BPTT)
  *   **Problem Statement:** 
    Assume a simple RNN without inputs $x$, where the hidden state is just updated by multiplying the previous state by the weight matrix $W$: $h_t = W h_{t-1}$. 
    If you unroll this over $t$ time steps, the equation becomes $h_t = (W^t) h_0$. If we decompose $W$ using its eigenvalues ($\lambda$), what happens to the gradient over long sequences?
  *   **Solution / Steps:**
    *   If the absolute value of the eigenvalues $\lambda > 1$ (e.g., 1.1), multiplying it by itself $t$ times ($1.1^{100}$) causes the value to **Explode** to infinity.
    *   If the absolute value of the eigenvalues $\lambda < 1$ (e.g., 0.9), multiplying it by itself $t$ times ($0.9^{100}$) causes the value to **Vanish** to zero.
    *   *Conclusion:* Standard RNNs physically cannot learn long-distance dependencies because the error signal during backpropagation shrinks to zero (vanishes) before it can reach the early words in a sentence.
### 3. Mathematical Example: How LSTM Fixes the Gradient
  *   **Slide:** 23
  *   **Category:** LSTM Architecture Math
  *   **Problem Statement:** 
    In an LSTM, the long-term memory (Cell State $C_t$) is updated using the formula: $C_t = (f_t \otimes C_{t-1}) + (i_t \otimes \tilde{C}_t)$. 
    Look at the gradient flow $\frac{\partial C_t}{\partial C_{t-1}}$ and explain mathematically why LSTMs prevent the vanishing gradient problem.
  *   **Solution / Steps:**
    *   If you take the derivative of the $C_t$ equation with respect to $C_{t-1}$, the $+ (i_t \otimes \tilde{C}_t)$ term drops out, leaving exactly **$f_t$** (the Forget Gate).
    *   Therefore, the gradient flowing back in time over $T$ steps is roughly $\prod_{t=2}^T f_t$. 
    *   Because $f_t$ is a dynamically learned parameter bounded by a Sigmoid function ($0 \le f_t \le 1$), the model can choose to set $f_t \approx 1$.
    *   If $f_t \approx 1$, the gradient is preserved perfectly backward through time ($1 \times 1 \times 1 = 1$), completely avoiding the vanishing gradient problem caused by standard matrix multiplication in RNNs.
### 4. Example: LSTM Application Architectures
  *   **Slides:** 29–32
  *   **Category:** Sequential Modeling Designs
  *   **Problem Statement:** Match the following real-world applications to their correct LSTM input-output architecture:
    1. Image Captioning
    2. Sentiment Classification
    3. Time Series Forecasting (e.g., Temperature)
    4. Machine Translation (e.g., English to French)
  *   **Solution:**
    1.  **Image Captioning:** *One-to-Many*. (Input: 1 image vector. Output: Sequence of words).
    2.  **Sentiment Classification:** *Many-to-One*. (Input: Sequence of words. Output: 1 label like "Positive").
    3.  **Time Series Forecasting:** *Many-to-Many (Aligned)*. (Input: Sequence of past observations. Output: Sequence of future predictions).
    4.  **Machine Translation:** *Many-to-Many (Encoder-Decoder)*. (Input: Sequence of English words $\to$ hidden state $\to$ Output: Sequence of French words).
### 5. Example: Bidirectional LSTM (BiLSTM)
  *   **Slide:** 35
  *   **Category:** NLP Classification
  *   **Problem Statement:** You want to classify the sentiment of the sentence "I love this movie" using a BiLSTM. How does the architecture process this?
  *   **Solution / Steps:**
    1.  **Forward LSTM:** Reads the sequence left-to-right: "I $\to$ love $\to$ this $\to$ movie."
    2.  **Backward LSTM:** Reads the sequence right-to-left: "I $\leftarrow$ love $\leftarrow$ this $\leftarrow$ movie."
    3.  **Concatenation:** At the final step, it glues the final hidden state of the forward pass and the final hidden state of the backward pass together: $h_t = [\overrightarrow{h_t} ; \overleftarrow{h_t}]$.
    4.  **Output:** Passes this massive concatenated context vector to a classifier to predict "Positive".
  
  ---
### 6. In-Class Participation Question (True/False Quiz)
  *   **Slides:** 27 \& 28
  *   **Category:** LSTM Concepts
  *   **Problem Statement \& Solutions:** 
    1.  **[True]** LSTM networks can process sequences of variable lengths. *(Because the unrolled loop just runs for $N$ time steps, it can adapt to 5 words or 500 words using the exact same shared weight matrices).*
    2.  **[False]** LSTM cells are only useful for natural language processing tasks. *(They are widely used for sensor signals, stock prices, video frames, etc.).*
    3.  **[False]** The vanishing gradient problem is *completely* solved by LSTM networks. *(It heavily alleviates it, but for infinitely long sequences, gradients can still eventually decay. Modern Transformers solved this more definitively).*
    4.  **[True]** The forget gate in an LSTM cell decides what information should be removed from the cell state. *(It outputs a 0 to erase, or a 1 to keep).*
    5.  **[False]** LSTM networks use only one type of memory to store information across time steps. *(They use TWO: the Cell State $C_t$ for long-term memory, and the Hidden State $h_t$ for short-term output).*
  
  ---
### 7. After-Class Questions
  *   **Slide:** 37
  *   **Category:** Theory Review
  *   **Q1: Explain the roles of the following gates in LSTM: Forget gate, Input gate, and Output gate.**
    *   *Forget Gate:* Uses a Sigmoid function to decide what historical information on the long-term cell state conveyor belt should be erased.
    *   *Input Gate:* Uses a Sigmoid function to decide what *new* candidate information (generated by a $\tanh$ layer) is relevant enough to be written onto the long-term cell state.
    *   *Output Gate:* Uses a Sigmoid function to filter the long-term cell state and decide which specific parts of it should be revealed as the short-term Hidden State ($h_t$) for the current time step.
  *   **Q2: Why can LSTM alleviate the vanishing gradient problem?**
    *   *Answer:* Because the Cell State ($C_t$) updates via addition rather than matrix multiplication. The gradient flowing backwards depends only on the Forget Gate ($f_t$). By learning to keep the Forget Gate near 1, the gradient is preserved across many time steps.
  *   **Q3: Compare RNN and LSTM in terms of gradient propagation.**
    *   *Answer:* Standard RNNs propagate gradients by repeatedly multiplying by the weight matrix $W$, which causes exponential decay (vanishing) or growth (exploding). LSTMs propagate gradients through the Cell State "highway" controlled by the Forget Gate, bypassing the continuous matrix multiplications.
  *   **Q4: What happens if the forget gate is always close to 1?**
    *   *Answer:* The LSTM will remember *everything* from the past infinitely. While this perfectly stops the vanishing gradient, the cell state will eventually become saturated with irrelevant, obsolete information from the distant past, harming its ability to adapt to new contexts.
  *   **Q5: In what types of tasks is LSTM more suitable than CNN? Why?**
    *   *Answer:* LSTMs are suited for **Sequential Data** (Time-series, NLP, Audio). CNNs look at fixed-size spatial grids (like a $28 \times 28$ image). LSTMs are required when the *order* of the inputs fundamentally defines the meaning of the data (e.g., grammar in a sentence, or temporal patterns in stock prices).
  
  ***
- ---
- ---
- ---
# Lecture 18: Self-Supervised Learning (SSL)
### 1. Mathematical Example: The Cost of Supervised Learning
  *   **Slides:** 4–5
  *   **Category:** SSL Motivation
  *   **Problem Statement:** Assume you want to label 1 Million images using human annotators. Assume it takes 10 seconds per image, and you pay a low wage of \$15/hour. How much will it cost? What if you want to label 1 Billion images?
  *   **Solution / Steps:**
    *   **1M Images:** $(1,000,000 \text{ images}) \times (10 \text{ sec/image}) \times (\frac{1}{3600} \text{ hours/sec}) \times (\$15 / \text{hour}) = \mathbf{\$41,667}$.
    *   **1B Images:** $(1,000,000,000 \text{ images}) \times (10 \text{ sec/image}) \times (\frac{1}{3600} \text{ hours/sec}) \times (\$15 / \text{hour}) = \mathbf{\$41,666,667}$.
    *   *Takeaway:* Fully supervised learning does not scale to modern "Big Data" levels financially. (It's even worse for medical data, where radiologist annotators average \$218/hour). This necessitates SSL, which learns from free, unannotated data.
### 2. Conceptual Example: Yann LeCun's "Cake of Learning"
  *   **Slide:** 7
  *   **Category:** Learning Paradigms
  *   **Problem Statement:** How does Yann LeCun categorize Reinforcement Learning (RL), Supervised Learning, and Self-Supervised Learning (SSL) in terms of how much information the machine is given during training?
  *   **Solution:** 
    *   **The Cherry (RL):** Given only a few bits of information per sample (a scalar reward once in a while).
    *   **The Icing (Supervised):** Given 10 to 10,000 bits per sample (a specific human-supplied category/label).
    *   **The Cake Base (SSL):** Given millions of bits per sample. The machine predicts any part of the input from any observed part (e.g., predicting future video frames). *Conclusion: The future of AI relies on the cake base.*
### 3. Examples: Classic Pretext Tasks
  *   **Slides:** 14–21
  *   **Category:** Phase 1 SSL Pretraining
  *   **Problem Statement:** What are some examples of "Pretext Tasks" that allow a CNN to learn "visual commonsense" without human labels?
  *   **Solutions (Examples Provided in Class):**
    1.  **Predict Rotations:** Rotate an image by 0, 90, 180, or 270 degrees. Force the model to predict the rotation angle.
    2.  **Relative Position:** Take two patches of an image (e.g., a cat's eye and ear) and predict their spatial relationship.
    3.  **Jigsaw Puzzle:** Cut an image into 9 tiles, shuffle them, and make the model predict the correct order.
    4.  **Inpainting (Missing Pixels):** Black out the center of an image and force the model to generate the missing pixels.
    5.  **Masked Autoencoder (MAE):** Mask out 75% of an image's patches, encode the remaining 25%, and force a decoder to rebuild the original image.
### 4. Conceptual Example: Evaluating SSL (Linear Evaluation Protocol)
  *   **Slides:** 22–24
  *   **Category:** SSL Evaluation Phase 2
  *   **Problem Statement:** Once you have pre-trained a model on a pretext task (like predicting image rotations), how do you evaluate if the extracted features are actually good for downstream tasks?
  *   **Solution / Steps:**
    1.  **Freeze the Encoder:** Take the trained feature extractor and lock its weights so they cannot be updated.
    2.  **Attach a Linear Classifier:** Add a simple, shallow layer (like Logistic Regression) to the end of the frozen encoder.
    3.  **Train on Target Task:** Train *only* the new linear layer using a small amount of labeled data (e.g., classifying birds). If the linear classifier achieves high accuracy, the SSL model successfully untangled the data into a high-quality, linearly separable representation.
### 5. Example: Contrastive Learning & SimCLR
  *   **Slides:** 27–38
  *   **Category:** Modern SSL
  *   **Problem Statement:** How does SimCLR use Contrastive Learning to learn representations without relying on specific, handcrafted pretext tasks (like Jigsaws)?
  *   **Solution / Steps:**
    1.  Take a single image $x$ and apply two random augmentations (crop, blur, color distort) to create a **Positive Pair**.
    2.  Pass both through the encoder to get representations. 
    3.  Treat all other images in the current training batch as **Negative Samples**.
    4.  Use the **InfoNCE Loss Function** to maximize the cosine similarity between the positive pair (attract) while minimizing the similarity to the negative samples (repel).
### 6. Example: BYOL (Bootstrap Your Own Latent)
  *   **Slides:** 39–41
  *   **Category:** Contrastive Learning without Negatives
  *   **Problem Statement:** Contrastive learning (like SimCLR) usually requires massive batches of negative samples to prevent the model from collapsing. How does BYOL avoid the need for negative pairs?
  *   **Solution:** BYOL uses an asymmetric **Student-Teacher** architecture. The Student model learns via standard backpropagation to predict the output of the Teacher model. The Teacher model does *not* use backpropagation; its weights are updated slowly using an **Exponential Moving Average (EMA)** of the Student's weights. This slow-moving target prevents the network from collapsing to a constant trivial output.
### 7. Examples: Multimodal & Text SSL
  *   **Slides:** 42–47
  *   **Category:** Advanced SSL Applications
  *   **Problem Statement:** Give examples of how SSL and Contrastive Learning are applied outside of standard image classification.
  *   **Solutions Provided:**
    1.  **CLIP (Image-Text):** Contrastive learning between an image and its natural language caption. Allows for "Zero-Shot" prediction (classifying new images based purely on text descriptions).
    2.  **Medical Data:** Contrastive learning between a Chest X-ray image and the radiologist's written clinical report.
    3.  **BERT (Text):** 
        *   *Masked Language Modeling (MLM):* Randomly replace 15% of words in a sentence with a `[MASK]` token and force the model to predict them.
        *   *Next Sentence Prediction (NSP):* Feed the model two sentences and predict if they are sequential in the original document.
  
  ---
### 8. After-Class Questions
  *   **Slide:** 50
  *   **Category:** Exam Review
  
  **Q1: What is the key difference between supervised, unsupervised, and self-supervised learning? Provide an example of each.**
  *   *Supervised:* Learns from manually provided human labels (e.g., Image classification with 1M images explicitly labeled "dog" or "cat").
  *   *Unsupervised:* Finds hidden patterns in unlabeled data without any specific target variable (e.g., K-Means clustering customer purchasing data).
  *   *Self-Supervised:* Uses unlabeled data, but defines a pre-text task where the data provides its own "pseudo-label" to train the model using supervised techniques (e.g., Hiding a patch of an image and training the model to predict the missing pixels).
  
  **Q2: [True or False] Self-supervised learning does not require any labels.**
  *   *Answer:* **False.** (Or "True, but with a caveat"). It does not require *human-annotated* labels, but it absolutely requires *pseudo-labels* generated automatically from the data itself (e.g., the exact angle the image was rotated). 
  
  **Q3: Many SSL methods rely on predicting part of the data from other parts (e.g., masked autoencoders). (1) How does this help the model learn useful representations? (2) Can this approach fail in some types of data?**
  *   *(1)* To successfully predict missing parts (like a missing word in a sentence or a missing dog's ear in a photo), the model is forced to learn the underlying semantic structure, grammar, and "visual commonsense" of the world. 
  *   *(2)* Yes, it fails if the data lacks inherent spatial/temporal correlation (e.g., trying to mask and predict entirely independent variables like randomly generated lottery numbers).
  
  **Q4: You have a time-series dataset with sensor readings but no labels. Design a self-supervised pretext task to learn useful features. Explain your reasoning.**
  *   *Answer (Example 1):* **Masking.** Mask out a 1-second segment of the sensor reading and train the model to predict the missing waveform. *Reasoning:* Forces the model to learn the normal cyclical rhythms of the machine.
  *   *Answer (Example 2):* **Contrastive.** Take two overlapping 5-second windows of the sensor data, apply noise/scaling, and treat them as positive pairs. Treat sensor data from a completely different day as negative pairs. *Reasoning:* Forces the model to learn the core "fingerprint" of the signal while ignoring random sensor noise.
  
  **Q5: Suppose you train an SSL model on a large image dataset. How could you evaluate the quality of learned representations without fine-tuning on a downstream task?**
  *   *Answer 1:* **Linear Evaluation Protocol.** Freeze the learned encoder, attach a single linear layer (like Logistic Regression), and train *only* that linear layer on a small labeled dataset. High accuracy proves the features are deeply meaningful and linearly separable.
  *   *Answer 2:* **Clustering/Visualization.** Pass images through the frozen encoder to get feature vectors, and use t-SNE or PCA to visualize them. If the representations are high-quality, images of similar objects will naturally cluster together.
  
  ***
- ---
- ---
- ---
# Lecture 19: Transformer
### 1. In-Class Participation Question (Warm-up from SSL)
  *   **Slide:** 2
  *   **Category:** SSL Theory Review
  *   **Problem Statement:** Evaluate the following True/False statements regarding Self-Supervised Learning (SSL).
  *   **Solutions:**
    *   **[True]** SSL creates supervisory signals directly from the data itself. *(It creates pseudo-labels, like hiding a word and predicting it).*
    *   **[False]** SSL representations are typically less useful for downstream tasks than supervised learning. *(They are often more robust and generalize better).*
    *   **[False]** The performance on a pretext task always directly reflects downstream task performance. *(We only care about the quality of the feature representations, not how well the model solves the fake puzzle).*
    *   **[False]** Contrastive learning requires both positive and negative sample pairs. *(Algorithms like BYOL proved you only need positive pairs).*
    *   **[False]** BYOL requires negative samples to avoid collapse. *(It uses a slow-moving Teacher network to avoid collapse).*
### 2. Conceptual Example: Input/Output Embedding
  *   **Slides:** 14–16
  *   **Category:** Word Representations
  *   **Problem Statement:** Computers only understand numbers. Should we use simple IDs (e.g., `I = 1`, `love = 2`, `dog = 3`, `cat = 4`) to feed words into a Transformer? Why or why not?
  *   **Solution:** No, simple IDs are terrible for machine learning. They create an artificial mathematical order (implying "cat" is twice as large as "love") and fail to capture semantic similarity. Instead, we use **Embeddings** (dense vectors of floats like `[0.25, ..., 0.85]`). In embedding space, the vector for "cat" naturally lies close to the vector for "dog," allowing the model to mathematically understand synonyms.
### 3. Mathematical Example: Positional Encoding
  *   **Slides:** 19–20
  *   **Category:** Injecting Sequence Order
  *   **Problem Statement:** The Transformer processes all words simultaneously (in parallel). Therefore, "The mouse ate the cat" looks mathematically identical to "The cat ate the mouse." How do we fix this?
  *   **Solution / Steps:**
    *   We use **Positional Encoding**. We generate a matrix of sine and cosine waves of varying frequencies.
    *   Each row in this matrix represents a position in the sentence (Position 1, Position 2, etc.).
    *   We mathematically **add** this positional vector to the original word embedding vector. 
    *   *Takeaway:* Word Embedding tells the network *"What the word is."* Positional Encoding tells the network *"Where the word is."*
### 4. Mathematical Walkthrough: Self-Attention
  *   **Slides:** 29–34
  *   **Category:** The Attention Mechanism
  *   **Problem Statement:** Calculate the Self-Attention output for a word in a sentence using the formula: $\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^T}{\sqrt{d_k}}\right) V$
  *   **Solution / Steps (The "Thinking Machines" Example):**
    1.  **Generate Q, K, V:** Multiply the input embedding by three learned weight matrices ($W_Q, W_K, W_V$) to create the **Query** (What am I looking for?), **Key** (What do I offer?), and **Value** (What is my actual meaning?).
    2.  **Calculate Score (Dot Product):** To see how much the word "Thinking" ($q_1$) relates to "Machines" ($k_2$), take the dot product $q_1 \cdot k_2 = 96$. (Similarly, $q_1 \cdot k_1 = 112$).
    3.  **Scale:** Divide the scores by $\sqrt{d_k}$ (the square root of the dimension of the Key vectors). In the slides, $d_k = 64$, so $\sqrt{64} = \mathbf{8}$. 
        *   *Scores become:* $112 / 8 = \mathbf{14}$ and $96 / 8 = \mathbf{12}$.
        *   *(Note: This prevents the scores from getting so huge that the Softmax gradient vanishes).*
    4.  **Softmax:** Pass the scaled scores through a Softmax function to turn them into percentages that sum to 1.0. (e.g., $0.88$ for "Thinking" and $0.12$ for "Machines").
    5.  **Weighted Sum of Values:** Multiply these Softmax percentages by the respective **Value ($V$)** vectors and sum them up. The output is a new, context-aware vector!
### 5. Conceptual Example: Creating Masks
  *   **Slides:** 42–43
  *   **Category:** Attention Masking
  *   **Problem Statement:** Masking forces certain attention outputs to be exactly zero. What are the two types of masks used in a Transformer, and what is their purpose?
  *   **Solution:**
    1.  **Padding Mask (Used in Encoder & Decoder):** Sentences are padded with `<PAD>` tokens to make them all the same length in a batch. We apply a mask of `0`s to ensure the network does not pay any attention to these useless filler tokens.
    2.  **Look-Ahead Mask (Used ONLY in Decoder):** During training, we feed the entire target sentence in at once. We apply a triangular mask to hide future words. When predicting word #3, it is only allowed to look at words #1 and #2. This prevents the decoder from "peaking ahead" at the answer.
### 6. Conceptual Example: Decoding Strategies
  *   **Slides:** 47–48
  *   **Category:** Text Generation
  *   **Problem Statement:** What is the difference between a "Greedy Strategy" and "Beam Search" when the Decoder generates text?
  *   **Solution:**
    *   **Greedy Strategy:** At every time step, simply pick the single word with the highest Softmax probability. (Fast, but can lead to grammatically poor sentences because it doesn't plan ahead).
    *   **Beam Search:** Keep the top $K$ (e.g., top 3) most likely partial sentences in memory at all times. At each step, expand all 3 branches, calculate the total probabilities, and keep the new top 3. (Slower, but yields globally better sentences).
  
  ---
### 7. After-Class Practice Questions (Exam Goldmine)
  *   **Slide:** 52
  *   **Category:** Transformer Theory Review
  
  **Q1: Explain why self-attention is permutation-invariant. (a) why does this create a problem? (b) how does positional encoding resolve it?**
  *   **Answer:** Self-attention simply calculates dot products between a "bag of words." If you shuffle the words, the dot products remain identical. (a) This is a problem because human language relies on word order (syntax) for meaning. (b) Positional encoding injects unique mathematical sine/cosine waves into the embedding vectors, altering their values based on their position in the sequence. The model can then differentiate between "cat" at position 2 and "cat" at position 5.
  
  **Q2: Compare self-attention vs RNN/LSTM in terms of: dependency modeling, computational complexity and parallelization.**
  *   **Answer:**
    *   *Dependency Modeling:* RNNs suffer from vanishing gradients over long distances. Self-attention connects *every word to every other word* in exactly 1 step (via dot product), perfectly capturing long-range dependencies.
    *   *Complexity:* RNNs are $O(N \cdot d^2)$. Self-attention is $O(N^2 \cdot d)$. If a sequence length ($N$) gets massive (like a whole book), self-attention becomes exponentially more computationally expensive than an RNN.
    *   *Parallelization:* RNNs are strictly sequential ($O(N)$ sequential steps). Transformers process the entire sequence simultaneously in massive matrices ($O(1)$ sequential steps), making them incredibly fast to train on modern GPUs.
  
  **Q3: In the Transformer encoder, why are residual connections + layer normalization necessary? (a) what problems do they solve? (b) what would happen if we remove them?**
  *   **Answer:** (a) Residual connections ($x + \text{layer}(x)$) provide a highway for gradients to flow backward, solving the **Vanishing Gradient** problem. Layer Normalization keeps the activations stable and prevents values from exploding during deep matrix multiplications. (b) If removed, a deep Transformer (e.g., 100 layers) would suffer catastrophic gradient vanishing/exploding and completely fail to train.
  
  **Q4: Why does the Transformer decoder use masking? (a) what would happen if masking is removed during training? (b) how does this relate to inference?**
  *   **Answer:** The decoder uses a "Look-Ahead" mask. (a) During training, we feed the whole target sentence in at once for speed. If we removed the mask, the Self-Attention mechanism would "cheat" by looking at the very word it is trying to predict, meaning it would learn nothing. (b) During real-world inference, the mask happens naturally because the future words literally have not been generated yet. The mask during training simulates this real-world constraint.
  
  **Q5: Implement the self-attention module.**
  *   **Answer:** (This is asking for the mathematical steps of the formula).
    1. Linearly project inputs into $Q, K, V$ matrices.
    2. Compute the attention scores by taking the dot product of Queries and Keys: $Q K^T$.
    3. Scale the scores by dividing by $\sqrt{d_k}$.
    4. Apply the Softmax function to convert scores into probability weights.
    5. Multiply the weights by the Value matrix $V$ to get the final contextualized representation.
  
  ***
- ---
- ---
- ---
# Lecture 20: Outlier Detection
### 1. Conceptual Example: Noise vs. Anomalies
  *   **Slide:** 9
  *   **Category:** Definitions & Distinctions
  *   **Problem Statement:** You are monitoring an ECG signal for a hospital patient. There are constant, tiny, random fluctuations in the signal. Suddenly, there is a massive spike. Distinguish the "Noise" from the "Anomaly."
  *   **Solution:** 
    *   **Noise:** The tiny, random fluctuations. They have no underlying pattern, are caused by sensor errors/environmental randomness, and should typically be **ignored or smoothed out**.
    *   **Anomaly:** The massive spike. It is a meaningful deviation that indicates a specific underlying cause (a medical abnormality/arrhythmia) and must be **detected and analyzed**.
### 2. Conceptual Example: Three Learning Settings
  *   **Slides:** 11–13
  *   **Category:** ML Problem Formulation
  *   **Problem Statement:** Identify whether the following anomaly detection scenarios are Supervised, Semi-Supervised, or Unsupervised.
    1. You have a dataset of 1,000,000 credit card transactions where every transaction is labeled as "Fraud" or "Legit."
    2. You have a dataset of server logs with no labels whatsoever.
    3. You have 5 years of ECG data from perfectly healthy patients, and you want to use it to flag future sick patients.
  *   **Solution:**
    1.  **Supervised:** (Slide 11). Extremely rare in practice due to the high cost of labels and severe class imbalance.
    2.  **Unsupervised:** (Slide 13). Assumes anomalies are rare and statistically different from the dense majority clusters.
    3.  **Semi-Supervised (Most Common):** (Slide 12). You train a model (like an Autoencoder or One-Class SVM) on *only normal data* to learn a "model of normality." If new data deviates from this model, it's flagged.
### 3. Mathematical Example: Statistical Methods (Z-Score & IQR)
  *   **Slides:** 18–19
  *   **Category:** Statistical Anomaly Detection
  *   **Problem Statement:** Given a normal distribution of data, what are the mathematical rules used by the Z-Score method and the IQR method to flag an outlier?
  *   **Solution:**
    *   **Z-Score:** Calculates how many standard deviations a point is from the mean ($Z = \frac{x - \mu}{\sigma}$). **Rule:** If $Z < -3$ or $Z > 3$, it is an outlier.
    *   **IQR:** Calculates the Interquartile Range ($Q3 - Q1$). **Rule:** If $X < Q1 - 1.5(IQR)$ or $X > Q3 + 1.5(IQR)$, it is an outlier. *(IQR is more robust because means and variances in the Z-score are heavily distorted by outliers).*
### 4. Mathematical Concept: Local Outlier Factor (LOF)
  *   **Slide:** 26
  *   **Category:** Density-Based Approaches
  *   **Problem Statement:** A simple distance-based KNN approach fails when a dataset has one highly dense cluster and one very sparse cluster. How does the LOF algorithm solve this, and how do you interpret its final score?
  *   **Solution / Steps:**
    *   LOF solves this by comparing a point's local density to the local density of its neighbors. 
    *   It calculates the **Local Reachability Density (LRD)** (the inverse of the average distance to its $K$ neighbors).
    *   **The Formula:** $\text{LOF} = \frac{\text{Average LRD of Neighbors}}{\text{LRD of Point}}$
    *   **Interpretation:**
        *   **$\text{LOF} \approx 1$:** The point has the same density as its neighbors. It is a **Normal** point.
        *   **$\text{LOF} \gg 1$:** The neighbors are tightly packed (High LRD), but the point is far away (Low LRD). It is an **Outlier**.
### 5. Conceptual Example: Reconstruction-Based Approaches
  *   **Slides:** 32–38
  *   **Category:** Neural Network Anomaly Detection
  *   **Problem Statement:** Explain the step-by-step pipeline of using an Autoencoder (or PCA) to detect an anomaly.
  *   **Solution / Steps:**
    1.  **Train:** Train an Autoencoder strictly on **normal** data to compress it into a small latent space and reconstruct it.
    2.  **Test:** Pass a new data point $x$ through the network to get the reconstructed output $\hat{x}$.
    3.  **Score:** Calculate the **Reconstruction Error**: $E = ||x - \hat{x}||$.
    4.  **Rule:** Because the model only knows how to rebuild normal data, if you feed it an anomaly, the reconstruction will be terrible. If $E > \text{Threshold}$, flag it as an outlier.
### 6. Mathematical Example: Time-Series Moving Average
  *   **Slide:** 40
  *   **Category:** Time-Series Detection
  *   **Problem Statement:** You have a time-series dataset of server traffic. You use a moving average with a window size $k$. How do you calculate the expected value $\hat{x}_t$, and how do you use it to find an anomaly?
  *   **Solution:**
    *   **Expected Value:** $\hat{x}_t = \frac{1}{k} \sum_{i=t-k+1}^{t} x_i$ (The simple average of the last $k$ steps).
    *   **Anomaly Score:** Calculate the Residual Error $e_t = x_t - \hat{x}_t$. If this difference is massive, the current point heavily deviates from recent history and is an anomaly.
### 7. Conceptual Example: Predictability Modeling (Video)
  *   **Slides:** 42–43
  *   **Category:** Advanced Temporal Detection
  *   **Problem Statement:** You want to detect anomalies in security camera footage (e.g., a car driving on a sidewalk). How do you set up the model?
  *   **Solution:** 
    *   *Assumption:* Normal instances are temporally predictable.
    *   Train a CNN/LSTM to look at frames $I_1, I_2, \dots, I_t$ and predict the exact pixels of the very next frame $\hat{I}_{t+1}$. 
    *   If a car suddenly drives onto the sidewalk, the model (which has only ever seen pedestrians) will predict a pedestrian. The difference between the prediction and the actual frame (the car) will result in a massive pixel reconstruction error, flagging the event.
### 8. Evaluation Example: The Accuracy Trap
  *   **Slides:** 48–50
  *   **Category:** Evaluating Anomaly Detectors
  *   **Problem Statement:** You build an anomaly detector for a dataset with 99.9% normal data and 0.1% anomalies. Why is evaluation challenging, and which metrics should you use?
  *   **Solution:**
    *   *The Challenge:* Severe class imbalance. A model that blindly guesses "Normal" gets 99.9% accuracy but is entirely useless.
    *   *The Solution:* You must use metrics that focus on the minority class: **Precision, Recall, F1-Score**.
    *   *For Thresholds:* **PR-AUC** (Precision-Recall Area Under Curve) is strictly preferred over ROC-AUC for severe imbalance, because ROC incorporates True Negatives (which massively distorts the curve when normal data is in the millions).
  
  ***
# Problem Solving & Concept Check
  
  **1. Types of Outliers (Slide 15):**
  A patient's heart rate monitor reads 60 BPM (beats per minute). Usually, 60 BPM is a perfectly healthy, normal resting heart rate. However, the patient is currently in the middle of a high-intensity treadmill stress test. 
  *   What type of outlier is this?
  *   *Answer:* A **Contextual Anomaly**. The raw value (60) is globally normal, but given the specific context (running on a treadmill), it is a dangerous anomaly.
  
  **2. SVDD / One-Class SVM (Slide 31):**
  When training a One-Class SVM on normal data, the algorithm tries to find the smallest possible hypersphere that encloses the data. 
  *   If a new test point falls *outside* this sphere, what is it classified as?
  *   *Answer:* An outlier/anomaly.
  
  **3. Autoencoder Bottleneck Trap (Slide 44):**
  You train an Autoencoder to detect fraudulent bank transactions. To make sure the model is "powerful," you use a very deep network with a massive bottleneck layer. During testing, the model fails to catch the fraud.
  *   What weakness of reconstruction-based approaches did you trigger?
  *   *Answer:* **Risk of over-generalization.** By making the bottleneck too wide/powerful, the model became essentially an "identity function." It learned how to perfectly reconstruct *everything*, including the anomalies. Because the anomaly was reconstructed perfectly, the Reconstruction Error was 0, and the alarm didn't trigger. (You must keep the bottleneck small to force the network to only learn normal, compressible patterns).
  
  ***