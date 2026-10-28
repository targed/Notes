## 1. The Power of "Memorization" (Slide 24 & 25)
  If a network has 100 layers, calculating the chain rule from scratch for the 1st layer would require re-calculating the derivatives of layers 2 through 100 every single time. This is computationally impossible.
  
  *   **The Trick (Dynamic Programming):** As we push data *forward* through the network, we **save (memorize)** the raw scores ($h$) and the activated outputs ($o$) at every single node. 
  *   When we go *backward*, we reuse those saved numbers to instantly calculate the local gradients. 
  
  **The Full Backprop Algorithm (Slide 25):**
  1.  **Initialize** all weights randomly.
  2.  **Forward Pass:** Pass input $x_i$ through the network. Save all intermediate values.
  3.  **Calculate Loss:** Compare prediction $\hat{y}$ to truth $y$.
  4.  **Backward Pass:** Use the Chain Rule to calculate gradients, moving from the output back to the input.
  5.  **Update Weights:** $w_{new} = w_{old} - \eta \cdot \text{gradient}$.
  6.  **Repeat** until convergence.
  
  ---
## 2. Walkthrough: The Forward Pass (Slide 29)
  Let's replicate the math exactly as your professor wrote it on the slide.
  
  **The Setup:**
  *   **Input:** $x = [1, 0.5]^T$
  *   **True Label:** $y = 4$
  *   **Weights (Layer 1):** $w_1 = 0.5, w_2 = 1.5, w_3 = 2.3, w_4 = 3$
  *   **Weights (Layer 2):** $w_5 = 1, w_6 = 1$
  *   **Activation:** ReLU ($f(x) = \max(0, x)$)
  *   **Loss Function:** $L = (y - \hat{y})^2$ *(Note: The slide omits the $\frac{1}{2}$ for simplicity in the derivative later).*
  
  **Step 1: Calculate Hidden Nodes ($h$ and $o$)**
  *   **Node 1:** 
    *   Raw score: $h_1 = w_1 x_1 + w_2 x_2 = (0.5 \times 1) + (1.5 \times 0.5) = 0.5 + 0.75 = \mathbf{1.25}$.
    *   Activation: $o_1 = \text{ReLU}(1.25) = \mathbf{1.25}$.
  *   **Node 2:**
    *   Raw score: $h_2 = w_3 x_1 + w_4 x_2 = (2.3 \times 1) + (3 \times 0.5) = 2.3 + 1.5 = \mathbf{3.8}$.
    *   Activation: $o_2 = \text{ReLU}(3.8) = \mathbf{3.8}$.
  
  **Step 2: Calculate Output ($\hat{y}$)**
  *   Raw score: $h_3 = w_5 o_1 + w_6 o_2 = (1 \times 1.25) + (1 \times 3.8) = \mathbf{5.05}$.
  *   Prediction: $\hat{y} = h_3 = \mathbf{5.05}$.
  
  **Step 3: Calculate Loss**
  *   $L = (y - \hat{y})^2 = (4 - 5.05)^2 = (-1.05)^2 = \mathbf{1.1025}$.
  
  ---
## 3. Walkthrough: The Backward Pass (Slides 30–31)
  Now we must figure out how much to blame the weights for that $1.1025$ error.
### A. Updating a Final-Layer Weight ($w_5$) [Slide 30]
  We need the derivative of Loss with respect to $w_5$ ($\frac{\partial L}{\partial w_5}$). We chain 3 local derivatives together:
  $$ \frac{\partial L}{\partial w_5} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h_3} \cdot \frac{\partial h_3}{\partial w_5} $$
  
  1.  **Loss wrt Prediction:** $\frac{\partial L}{\partial \hat{y}} = -2(y - \hat{y}) = -2(4 - 5.05) = -2(-1.05) = \mathbf{2.1}$.
  2.  **Prediction wrt $h_3$:** $\hat{y} = h_3$, so the derivative is just $\mathbf{1}$.
  3.  **$h_3$ wrt $w_5$:** $h_3 = w_5 o_1 + w_6 o_2$. The derivative with respect to $w_5$ is simply the input to that weight, which is $o_1$. We saved this! $o_1 = \mathbf{1.25}$.
  
  **Multiply them together:**
  $$ \frac{\partial L}{\partial w_5} = 2.1 \times 1 \times 1.25 = \mathbf{2.625} $$
  *(This means increasing $w_5$ by 1 unit would increase the loss by 2.625. So, Gradient Descent will **subtract** this value to fix the error).*
### B. Updating an Input-Layer Weight ($w_1$) [Slide 31]
  Your professor left this slide blank (`= . . .`) for you to solve. Let's do it! 
  Because $w_1$ is deeper in the network, the chain is 5 links long:
  $$ \frac{\partial L}{\partial w_1} = \frac{\partial L}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial h_3} \cdot \frac{\partial h_3}{\partial o_1} \cdot \frac{\partial o_1}{\partial h_1} \cdot \frac{\partial h_1}{\partial w_1} $$
  
  We already calculated the first two links! ($2.1 \times 1$).
  3.  **$h_3$ wrt $o_1$:** $h_3 = w_5 o_1 + w_6 o_2$. The derivative wrt $o_1$ is $w_5$. From our setup, $w_5 = \mathbf{1}$.
  4.  **$o_1$ wrt $h_1$:** This is the derivative of the ReLU function! Since $h_1$ was positive ($1.25$), the slope of ReLU is $\mathbf{1}$. *(If $h_1$ had been negative, the slope would be 0, and the gradient would die immediately!)*
  5.  **$h_1$ wrt $w_1$:** $h_1 = w_1 x_1 + w_2 x_2$. The derivative wrt $w_1$ is $x_1$. From our setup, $x_1 = \mathbf{1}$.
  
  **Multiply them together:**
  $$ \frac{\partial L}{\partial w_1} = 2.1 \times 1 \times 1 \times 1 \times 1 = \mathbf{2.1} $$
  
  *(If you were taking an exam, **2.1** is the number that fills in the blank on Slide 31!)*
  
  ***
# Problem Solving & Concept Check
  
  **1. The Derivative of ReLU:**
  You are backpropagating through a hidden node. During the forward pass, the raw score $h_2$ was $-3.5$, so the activated output $o_2$ was $0$. 
  *   What is the local derivative $\frac{\partial o_2}{\partial h_2}$?
  *   *Answer:* **Zero.** Because the input was negative, it fell on the flat, horizontal part of the ReLU function. A flat line has a slope of 0. 
  *   *Consequence:* Because this link in the chain is 0, any weight feeding into this node will get a final gradient of 0. The weights will not update. This is called the "Dying ReLU" problem.
  
  **2. Tracing the Chain Rule:**
  Imagine a network with a hidden node $h_1$. The weight $w_1$ connects $x_1$ to $h_1$. 
  If the input $x_1 = 5$, what is the local derivative $\frac{\partial h_1}{\partial w_1}$?
  *   *Answer:* **5.** In a linear equation $h = w \cdot x$, the derivative of the output with respect to the weight is always just the value of the input.
  
  **3. Computational Efficiency:**
  Why do we calculate the gradients from the "End of the network to the start" (Backwards) instead of from the inputs to the outputs?
  *   *Answer:* Because the layers near the end (like $\frac{\partial L}{\partial \hat{y}}$) are shared by *every single weight* in the network. If we go backwards, we calculate that number once and pass it down. If we went forwards, we would have to recalculate the exact same end-stage derivatives hundreds of times.
  
  ***