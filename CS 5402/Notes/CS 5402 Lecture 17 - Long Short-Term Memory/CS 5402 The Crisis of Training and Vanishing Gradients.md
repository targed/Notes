## 1. Backpropagation Through Time (BPTT) (Slides 13–15)
  To train an RNN, we need to adjust the shared weight matrices ($U, V, W$) using Gradient Descent. To find the gradients, we use a special version of Backpropagation called **BPTT**.
  
  **The Setup:**
  1.  **Unroll the Network:** We pretend the RNN is just a very deep, standard Feed-Forward network where each "layer" is a time step.
  2.  **Calculate the Loss ($E_t$):** At each time step where we make a prediction ($\hat{y}_t$), we calculate the error compared to the true target ($y_t$). Total Error $E = \sum E_t$.
  3.  **The Chain Rule:** To find out how much the weight matrix $W$ is to blame for the error at step $t=5$, we must trace the error backward from $h_5 \to h_4 \to h_3 \to h_2 \to h_1$. 
  
  **The Mathematical Key (Slide 15):**
  Because the exact same weight matrix $W$ is used at *every* time step, the total gradient for $W$ is the **sum** of the gradients calculated at each individual time step:
  $$ \frac{\partial E}{\partial W} = \sum_{t=1}^{T} \frac{\partial E_t}{\partial W} $$
## 2. The Mathematical Crisis (Slide 16)
  Here is the fatal flaw of the standard RNN. 
  
  To trace the error from time step $t=100$ back to time step $t=1$, the Chain Rule dictates that we must continuously multiply the local derivatives together. 
  Since the hidden state is calculated as $h_t = \tanh(U x_t + W h_{t-1})$, the derivative of $h_t$ with respect to $h_{t-1}$ involves multiplying by the weight matrix **$W$**.
  
  If we go back 100 steps, we are essentially multiplying $W$ by itself 100 times ($W^{100}$). 
  *   **The Eigenvalue Problem:** Think of $W$ as having a scaling factor (eigenvalue $\lambda$).
  *   **Exploding Gradients:** If $\lambda > 1$ (e.g., 1.1), then $1.1^{100}$ becomes a massive number. The gradients **explode** to infinity, the weights swing wildly, and the model crashes (outputs `NaN`).
  *   **Vanishing Gradients:** If $\lambda < 1$ (e.g., 0.9), then $0.9^{100}$ becomes microscopically small ($0.00002$). The gradients **vanish** to zero. The network stops updating the early weights.
## 3. The Practical Consequence: Short-Term Memory (Slide 17)
  Because of the Vanishing Gradient problem, a standard RNN physically cannot transport an error signal across long distances. 
  
  *   **Short Sequences (Success):** "The clouds are in the *sky*." The RNN only has to look back 5 words to know the answer is "sky." The gradient survives 5 multiplications.
  *   **Long Sequences (Failure):** "I grew up in France... [100 words later] ... I speak fluent *French*." The RNN must remember the word "France" from 100 steps ago. Because the gradient vanished to zero, the network completely fails to learn this **Long Distance Dependency**. It has amnesia.
  
  ***
# Problem Solving & Concept Check
  
  **1. BPTT Logic:**
  In standard Backpropagation (from Lecture 15), we update the weights of Layer 1 independently of Layer 2. In BPTT, how do we update the hidden-to-hidden weight matrix $W$?
  *   *Answer:* Because the RNN uses the *exact same* matrix $W$ at every time step, we must calculate the gradient of $W$ at step 1, step 2, step 3, etc., and **sum them all together** to make one single update to $W$.
  
  **2. The Vanishing Gradient Root Cause:**
  Why does the gradient vanish in a standard RNN but not necessarily in a wide, shallow network?
  *   *Answer:* The unrolled RNN acts like an extremely deep neural network. The Chain Rule requires us to multiply the local derivatives at every time step. If the weights are $< 1$ (or the derivative of the $\tanh$ activation function is $< 1$), multiplying a fraction by itself dozens of times shrinks the value exponentially toward zero.
  
  **3. Diagnosing the Symptoms:**
  You are training a standard RNN to translate a paragraph from English to Spanish. You notice that it translates the last few words of the paragraph perfectly, but completely messes up the grammar rules established at the beginning of the paragraph. What mathematical issue is causing this?
  *   *Answer:* The **Vanishing Gradient Problem**. The network cannot learn the **Long Distance Dependencies** because the error signal from the end of the paragraph shrinks to zero before it can adjust the weights responsible for processing the beginning of the paragraph.
  
  ***