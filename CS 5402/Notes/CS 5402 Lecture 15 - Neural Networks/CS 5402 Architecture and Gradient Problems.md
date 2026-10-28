## 1. Network Architecture: Width vs. Depth (Slides 32–34)
  When designing a Neural Network, you must choose its shape (Hyperparameter Tuning). 
  *   **Width (More neurons per layer):** Allows the network to "remember" more raw features. A wider network can pick up on many different simple patterns at once.
  *   **Depth (More layers):** Allows the network to learn **Abstraction**. The 1st layer learns edges, the 2nd layer learns shapes, the 3rd layer learns faces. Depth captures complex, hierarchical features.
  *   **The Trade-off:** Increasing both makes the model a better "Universal Approximator" (Slide 9), but a network that is too large will severely **overfit** the training data and memorize noise.
  
  ---
## 2. The Crisis: Vanishing & Exploding Gradients (Slides 35–38)
  This is one of the most famous mathematical problems in AI history (and the main reason the "AI Winter" happened before the 2010s). 
  
  **The Root Cause: The Chain Rule**
  In Part 2, we learned that to update a weight in Layer 1 of a 5-layer network, you have to multiply 5 local derivatives together.
### Crisis A: The Vanishing Gradient (Slides 35–36)
  If you use the **Sigmoid** activation function, the maximum possible value of its derivative (slope) is exactly **0.25**. 
  *   **The Math:** Imagine a 4-layer network. To get the gradient for Layer 1, you multiply the derivatives of the later layers. 
    $$ \text{Gradient} = \frac{\partial L}{\partial O_4} \times 0.25 \times 0.25 \times 0.25 \times \dots $$
  *   **The Result:** $0.25 \times 0.25 \times 0.25 \approx \mathbf{0.015}$. The gradient shrinks exponentially as it moves backward. 
  *   **The Consequence:** The weights in the early layers receive a gradient of essentially `0.0`. They **stop learning completely**, while the final layers do all the work. The model fails to train.
### Crisis B: The Exploding Gradient (Slide 37)
  What if the weights are initialized to large numbers, and the local derivatives are consistently $> 1$?
  *   **The Math:** $2.0 \times 2.0 \times 2.0 \times 2.0 = \mathbf{16.0}$. The gradient grows exponentially.
  *   **The Consequence:** The weight updates become so massive that they overshoot the minimum completely. The loss function swings wildly and quickly turns into `NaN` (Not a Number) as the computer's memory overflows.
  
  ---
## 3. The Modern Fixes (Slide 39)
  How did the deep learning community solve these problems to build 100-layer networks?
  1.  **ReLU (Rectified Linear Unit):** Replaces Sigmoid. For positive numbers, $f(x) = x$, so the derivative is exactly **1.0**. Multiplying $1.0 \times 1.0 \times 1.0$ never vanishes!
  2.  **Gradient Clipping:** To prevent exploding gradients, you set a hard speed limit. If the gradient calculates to 150, the computer artificially caps it at 5 before updating the weights.
  3.  **Skip Connections (ResNets):** You build physical "highways" that let the gradient bypass layers entirely to reach the early layers intact.
  4.  **Batch Normalization / Proper Weight Initialization:** Keeps the math stable.
  
  ---
## 4. Multi-Class Classification: SoftMax (Slide 40)
  Up until now, our networks have used Sigmoid at the output layer to predict a single probability (e.g., 80% chance it's a dog). What if we want to classify an image as a Dog, Cat, or Bird?
  
  **The Setup:**
  The output layer now has **3 neurons** (one for each class). They output 3 raw linear scores ($z_1, z_2, z_3$).
  
  **The SoftMax Function:**
  We need to convert these 3 raw scores into 3 probabilities that **sum to exactly 1.0**. 
  $$ P(\text{Class } k) = \frac{e^{z_k}}{\sum_{j=1}^{K} e^{z_j}} $$
  
  *   **Step 1:** Exponentiate every score ($e^z$). This makes all numbers positive and exaggerates the differences between them.
  *   **Step 2:** Divide each exponentiated score by the sum of all exponentiated scores (Normalization).
  *   **The Result:** You get a clean probability distribution (e.g.,[Dog: 0.70, Cat: 0.20, Bird: 0.10]).
  
  ***
# Problem Solving & Concept Check
  
  **1. SoftMax Calculation (Hand-Calculating):**
  A neural network is trying to classify an image as a Car, Truck, or Bike. The output layer produces the following raw linear scores ($z$):
  *   Car: $z_1 = 2$
  *   Truck: $z_2 = 1$
  *   Bike: $z_3 = 0$
  Calculate the SoftMax probability that the image is a **Car**. *(Leave your answer in terms of $e$)*.
  *   **Step 1 (Exponentiate):** $e^2$, $e^1$, $e^0$.
  *   **Step 2 (Sum):** Denominator $= e^2 + e^1 + 1$.
  *   **Step 3 (Normalize):** $P(\text{Car}) = \frac{e^2}{e^2 + e + 1}$.
  
  **2. Diagnosing Gradient Problems (Multiple Choice):**
  You are training a 10-layer neural network. After 50 epochs, you notice that the weights in Layers 8, 9, and 10 are updating beautifully, but the weights in Layers 1 and 2 haven't changed at all from their initial random values. What is happening?
  A) The learning rate is too high.
  B) The network is suffering from Exploding Gradients.
  C) The network is suffering from Vanishing Gradients.
  D) The model is overfitting.
  *   **Answer: C.** The error signal is shrinking as it travels backwards through the Chain Rule, hitting zero before it reaches the early layers.
  
  **3. Activation Function Selection:**
  Based on the Vanishing Gradient problem, why is the **ReLU** activation function generally preferred over the **Sigmoid** activation function for the *hidden layers* of a deep network?
  *   *Answer:* Because the derivative of Sigmoid maxes out at 0.25, forcing the gradient to shrink during backpropagation. The derivative of ReLU (for positive inputs) is exactly 1, allowing the gradient to flow backwards through deep networks without shrinking. 
  
  ***