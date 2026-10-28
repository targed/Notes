## 1. The Loss Functions (Slides 14–15)
  Once the neural network makes a prediction ($\hat{y}$), we need to measure how "wrong" it is compared to the ground truth ($y$). The choice of loss function depends entirely on your task.
  
  *   **Regression (Predicting a continuous number):**
    *   Use **Mean Squared Error (MSE)** or **Mean Absolute Error (MAE)**.
    *   *Formula:* $L = \frac{1}{n} \sum (y_i - \hat{y}_i)^2$
  *   **Classification (Predicting a category/probability):**
    *   Use **Binary Cross-Entropy (Log Loss)**.
    *   *Formula:* $L = - \frac{1}{n} \sum[y_i \log(\hat{y}_i) + (1 - y_i)\log(1 - \hat{y}_i)]$
  
  **The "Confident & Wrong" Penalty (Slide 15):**
  Your professor provided a concrete example of how Cross-Entropy punishes bad predictions.
  *   **Case 1 (Correct):** Ground Truth $y=1$. Prediction $\hat{y}=0.9$.
    *   $L = -1 \cdot \log(0.9) \approx \mathbf{0.05}$ (Tiny penalty).
  *   **Case 2 (Wrong):** Ground Truth $y=1$. Prediction $\hat{y}=0.1$.
    *   $L = -1 \cdot \log(0.1) \approx \mathbf{2.30}$ (Massive penalty!).
  *   *Takeaway:* The network is violently punished if it is highly confident about the wrong answer, forcing the weights to adjust rapidly.
## 2. The Optimization Goal (Slides 16–17)
  We want to update every single weight ($w$) and bias ($b$) in the network using **Gradient Descent**:
  $$ w^{k+1} = w^k - \eta \frac{\partial L}{\partial w} $$
  
  **The Ultimate Problem:** 
  Calculating the gradient ($\frac{\partial L}{\partial w}$) is easy for the weights connected directly to the output layer. But how do we calculate the gradient for a weight buried deep in **Hidden Layer 1**? That weight is separated from the final Loss by multiple layers of math, summations, and activation functions.
## 3. The Chain Rule (Slide 20)
  Calculus gives us the tool to look "through" functions: **The Chain Rule**.
  If $y = f(z)$ and $z = g(x)$, then a change in $x$ affects $z$, which in turn affects $y$. 
  To find how much $x$ affects $y$, we multiply the local derivatives together:
  $$ \frac{dy}{dx} = \frac{dy}{dz} \cdot \frac{dz}{dx} $$
  
  In a Neural Network, this means the gradient of a deep weight is simply the product of all the local gradients leading back to the output!
## 4. The "Multiple Paths" Rule (Slides 21–23)
  This is a highly testable concept for hand-calculations.
  In a fully connected neural network, a single neuron in Hidden Layer 1 (let's call it $h_1$) sends its signal to *every* neuron in Hidden Layer 2. 
  
  **The Rule:** If a variable affects the final Loss through multiple different paths, its total gradient is the **SUM** of the gradients from all those paths.
  
  *   *Slide 22/23 Formula:* If $x_1$ connects to $h_1$ and $h_2$, both of which connect to $f$, then:
    $$ \frac{\partial L}{\partial x_1} = \frac{\partial L}{\partial f} \left( \frac{\partial f}{\partial h_1}\frac{\partial h_1}{\partial x_1} + \frac{\partial f}{\partial h_2}\frac{\partial h_2}{\partial x_1} \right) $$
  *   *Intuition:* If you make a mistake that ruins the recipe in two different bowls, you are held responsible for the sum of the ruin in both bowls.
  
  ***
# Problem Solving & Concept Check
  
  **1. Cross-Entropy Application:**
  You have a neural network predicting if an image is a Dog ($y=0$) or a Cat ($y=1$).
  The image is a Dog ($y=0$). The network outputs a probability $\hat{y} = 0.8$.
  *   Write out the uncalculated Log Loss equation for this specific sample.
  *   *Answer:* Because $y=0$, the left side of the equation ($y \log(\hat{y})$) cancels out. We are left with: $L = -[1 \cdot \log(1 - 0.8)] = \mathbf{-\log(0.2)}$.
  
  **2. The Multiple Paths Concept:**
  Look at the network diagram on **Slide 23**. You want to calculate how much the weight $w_1$ (connecting Input $x_1$ to Hidden Node $h_1$) affects the final Loss $L$. 
  *   Does the gradient for $w_1$ require a summation of multiple paths?
  *   *Answer:* **No.** The weight $w_1$ *only* exists on the single path between $x_1$ and $h_1$. It does not branch. However, if you were asking about the gradient for the *input node $x_1$ itself*, you would have to sum the paths going through $h_1$ and $h_2$. 
  
  **3. Chain Rule Logic:**
  A network has the following sequence: 
  Weight ($w$) $\to$ Linear Score ($z$) $\to$ Activation ($a$) $\to$ Loss ($L$).
  Write the Chain Rule equation to find the gradient of the weight ($\frac{\partial L}{\partial w}$).
  *   *Answer:* $$\frac{\partial L}{\partial w} = \frac{\partial L}{\partial a} \cdot \frac{\partial a}{\partial z} \cdot \frac{\partial z}{\partial w}$$
  
  ***