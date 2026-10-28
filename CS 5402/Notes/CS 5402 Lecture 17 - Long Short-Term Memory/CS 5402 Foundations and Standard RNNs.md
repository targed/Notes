## 1. The Nature of Sequential Data (Slides 2–4)
  **What is Sequential Data?**
  Data where the current data point depends on the data points that came before it. 
  *   *Examples:* Time series (stock prices), Text/Natural Language ("The dog chased the cat"), Video frames, Audio/Speech, DNA sequences.
  
  **Why do traditional MLPs and CNNs fail here?**
  1.  **Fixed Sizes:** A standard neural network requires a fixed input size (e.g., exactly $28 \times 28$ pixels) and a fixed output size. Sentences and videos have variable lengths.
  2.  **Amnesia:** Traditional networks have no memory. If you feed it word #1, and then feed it word #2, it has completely forgotten word #1. It cannot understand *context* (e.g., "I am very hungry, and I can eat ___").
## 2. The Recurrent Neural Network (RNN) (Slides 5–6)
  To solve the amnesia problem, we add a **Loop** (Recurrence) to the network. 
  
  **The Concept:**
  Instead of just taking the current input ($x_t$), the network also takes its own previous thoughts from the last time step ($h_{t-1}$). 
  *   **$x_t$:** The input at the current time step (e.g., the word "hungry").
  *   **$h_{t-1}$:** The hidden state from the previous time step (the network's memory of the words "I", "am", "very").
  *   **$h_t$:** The new, updated hidden state that combines the past memory with the new input.
  
  **"Unrolling" the Network (Slide 6):**
  Because drawing loops is confusing, we usually draw RNNs "unrolled" through time. It looks like a long chain of networks, but it is actually just **one single network** being used over and over again, passing a message ($h$) forward to itself at each step.
  
  ---
## 3. The Math Behind the RNN (Slides 7–12)
  This is critical for understanding how the network actually works under the hood. 
  
  In a standard RNN, there are **three weight matrices** that the network learns. **Crucially, these exact same three matrices are shared across every single time step.**
  *   **$U$:** The weight matrix for the Input ($x_t$).
  *   **$W$:** The weight matrix for the Past Memory ($h_{t-1}$).
  *   **$V$:** The weight matrix for the Output ($\hat{y}_t$).
  
  **Step 1: Update the Memory (Hidden State)**
  To generate the new memory ($h_t$), the network calculates a linear combination of the current input and the old memory, and passes it through a **$\tanh$** activation function (which squashes values between -1 and 1 to keep them from exploding immediately).
  $$ h_t = \tanh(U x_t + W h_{t-1}) $$
  *(Note: There is also a bias term $+b$ here, but it is often omitted in slides for simplicity).*
  
  **Step 2: Make a Prediction (Output)**
  If the network needs to make a prediction at this specific time step, it multiplies the new memory ($h_t$) by the output weights ($V$) and passes it through an activation function $f$ (like SoftMax for classification).
  $$ \hat{y}_t = f(V h_t) $$
### The "Propagation" Example (Slides 9–12)
  Let's say we are doing a Language Translation task: "Nice to meet you."
  1.  **$t=1$:** We feed in $x_1$ ("Nice"). Since there is no past memory, $h_0$ is initialized as zeros. The network calculates $h_1 = \tanh(U x_1 + W \cdot 0)$. 
  2.  **$t=2$:** We feed in $x_2$ ("to") AND the memory from the last step ($h_1$). The network calculates $h_2 = \tanh(U x_2 + W h_1)$. It makes a prediction $\hat{y}_2$.
  3.  **$t=3$:** We feed in $x_3$ ("meet") AND the memory $h_2$. The network calculates $h_3 = \tanh(U x_3 + W h_2)$.
  
  ***
# Problem Solving & Concept Check
  
  **1. The "Weight Sharing" Concept:**
  An RNN processes a sentence with 5 words, and then processes a sentence with 50 words. Does the RNN need to learn more weights (parameters) to process the 50-word sentence?
  *   *Answer:* **No.** The weight matrices $U$, $V$, and $W$ are shared across all time steps. The network just runs its `for` loop 50 times instead of 5 times, using the exact same weights. This allows RNNs to handle sequences of any length!
  
  **2. Matrix Math Intuition:**
  Suppose your input words are converted into vectors of size 10 ($x_t \in \mathbb{R}^{10}$), and your hidden memory state is designed to be a vector of size 20 ($h_t \in \mathbb{R}^{20}$). 
  To make the math $Ux_t + Wh_{t-1}$ work, what must the dimensions of the weight matrices $U$ and $W$ be?
  *   *Answer:* 
    *   $U$ must transform the size 10 input into a size 20 vector. Therefore, $U$ is a **$20 \times 10$** matrix.
    *   $W$ must transform the size 20 past memory into a size 20 vector. Therefore, $W$ is a **$20 \times 20$** matrix.
  
  **3. The Initial State:**
  When calculating the very first hidden state ($h_1$), the formula requires $h_0$. Where does $h_0$ come from?
  *   *Answer:* It doesn't come from the data. It is usually initialized as a vector of absolute zeros, or sometimes as small random noise, because at $t=1$, the network has not read any sequence data yet.
  
  ***