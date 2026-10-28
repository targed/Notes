## 1. From Perceptron to Multilayer Network (Slides 5–7)
  We previously learned that a single **Perceptron** is a linear classifier. It takes inputs, multiplies them by weights, adds a bias, and passes the result through a step function. 
  *   **The Flaw:** It fails on non-linear problems (like the XOR problem).
  *   **The Solution:** We chain them together! The output of one layer of perceptrons becomes the input to the next layer. This is called a **Multilayer Perceptron (MLP)** or a Feed-Forward Neural Network.
  
  **Network Architecture (Slide 7 \& 11):**
  *   **Input Layer:** Your raw data features ($x_1, x_2, ...$).
  *   **Hidden Layers:** The "magic" layers in the middle. They learn abstract representations of the data. 
    *   *Math:* $z = W^T x + b$
  *   **Output Layer:** The final prediction ($\hat{y}$).
## 2. Activation Functions (Slide 8)
  If you just stack linear equations on top of each other (e.g., $y = 2(3x)$), the math simplifies back into a single linear equation ($y = 6x$). To allow the network to learn complex, curved, non-linear boundaries, we must inject non-linearity after every summation using an **Activation Function**.
  
  **The Big Three:**
  1.  **Sigmoid:** $\sigma(z) = \frac{1}{1 + e^{-z}}$
    *   Squashes numbers between 0 and 1. 
    *   *Old standard, but suffers from "Vanishing Gradients" (we'll cover this in Part 4).*
  2.  **Tanh:** $\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$
    *   Squashes numbers between -1 and 1. Zero-centered.
  3.  **ReLU (Rectified Linear Unit):** $\text{ReLU}(z) = \max(0, z)$
    *   If the number is negative, it becomes 0. If it's positive, it stays the exact same. 
    *   *The Modern Standard:* It is computationally extremely fast and fixes gradient problems.
## 3. The Universal Approximation Theorem (Slide 9)
  Why do Neural Networks work so well? 
  *   **The Theorem:** A neural network with **at least one hidden layer** and a non-linear activation function can approximate *any* continuous mathematical function, no matter how complex, given enough neurons (arbitrary precision).
  *   *Caveat:* It can learn any function, but that doesn't mean Gradient Descent will actually *find* the right weights, or that it won't severely overfit.
  
  ---
## 4. Walkthrough: Forward Propagation (Slide 13)
  This is a prime candidate for a hand-calculation exam question. "Forward Propagation" just means pushing the numbers from left to right through the network to get a prediction.
  
  **The Setup (from the slide visual):**
  *   **Inputs:** $x_1 = 0.1$, $x_2 = 0.5$, $x_3 = 0.3$
  *   **Hidden Layer:** 2 Neurons. Activation Function = **ReLU**.
  *   **Weights to Hidden Neuron 1 ($z_1$):** $w_{1,1} = 1$, $w_{2,1} = 1$, $w_{3,1} = 1$. (Notice they are all 1s).
  *   **Weights to Hidden Neuron 2 ($z_2$):** Not explicitly labeled in the slide graphic, but the math implies they are also 1s.
  *   **Weights from Hidden to Output ($\hat{y}$):** $w_{out1} = 0.5$, $w_{out2} = 0.3$.
  *   *Note:* Bias ($b$) is assumed to be 0 here for simplicity.
  
  **Step 1: Calculate the Raw Score ($z$) for the Hidden Layer**
  *   $z_1 = (0.1 \times 1) + (0.5 \times 1) + (0.3 \times 1) = \mathbf{0.9}$
  *   $z_2 = (0.1 \times 1) + (0.5 \times 1) + (0.3 \times 1) = \mathbf{0.9}$
  
  **Step 2: Apply the Activation Function (ReLU)**
  Remember, $\text{ReLU}(x) = \max(0, x)$.
  *   $a_1 = \text{ReLU}(z_1) = \max(0, 0.9) = \mathbf{0.9}$
  *   $a_2 = \text{ReLU}(z_2) = \max(0, 0.9) = \mathbf{0.9}$
  *(If any of these $z$ values were negative, they would instantly become 0).*
  
  **Step 3: Calculate the Final Output ($\hat{y}$)**
  Multiply the activated hidden neurons by the final output weights.
  *   $\hat{y} = (a_1 \times 0.5) + (a_2 \times 0.3)$
  *   $\hat{y} = (0.9 \times 0.5) + (0.9 \times 0.3)$
  *   $\hat{y} = 0.45 + 0.27 = \mathbf{0.72}$
  
  The network predicts **0.72**.
  
  ***
# Problem Solving & Concept Check
  
  **1. Activation Function Logic:**
  If you pass the raw score $z = -5.5$ into a ReLU activation function, what is the output? What if you pass it into a Sigmoid function?
  *   *Answer:* 
    *   **ReLU:** $max(0, -5.5) = \mathbf{0}$.
    *   **Sigmoid:** It will be a tiny positive number very close to $\mathbf{0}$ (because Sigmoid bounds outputs between 0 and 1, and heavily negative numbers approach 0).
  
  **2. Architecture Vocabulary:**
  If a network has an Input Layer with 4 features, two Hidden Layers with 10 neurons each, and an Output Layer with 1 neuron. How many "layers" deep is this network considered to be in standard Deep Learning terminology?
  *   *Answer:* It is a **3-Layer** network. (We do not count the input layer when stating the depth of a network, only the hidden layers and the output layer).
  
  **3. Linear Stacking Problem:**
  You build a Neural Network with 50 hidden layers, but you forget to include an activation function (i.e., you leave it strictly linear: $f(x) = x$). Can this network solve the non-linear XOR problem?
  *   *Answer:* **No.** Stacking linear layers just creates one giant linear transformation ($W_1 \times W_2 \times W_3... = W_{combined}$). Without a non-linear activation function (like ReLU or Sigmoid), it behaves exactly like a single, flat Perceptron.
  
  ***