### **1. Vision Transformer Patch Calculation (Slide 24)**

**The Prompt:** An image of size $256 \times 256$ is divided into patches of size $32 \times 32$.

**(a) Number of patches:**
*   *How to solve:* You need to find how many $32 \times 32$ squares fit inside a $256 \times 256$ grid.
*   *Width:* $256 / 32 = 8$ patches wide.
*   *Height:* $256 / 32 = 8$ patches high.
*   *Total:* $8 \times 8 = \mathbf{64 \text{ patches}}$.

**(b) If each patch is flattened, what is the dimension of each patch vector (assume RGB image)?**
*   *How to solve:* A single patch is a 3D box of pixels (Height $\times$ Width $\times$ Color Channels). RGB means there are 3 color channels (Red, Green, Blue).
*   *Math:* $32 \text{ (Height)} \times 32 \text{ (Width)} \times 3 \text{ (Channels)}$
*   *Result:* $1024 \times 3 = \mathbf{3072}$. (Each patch becomes a 1D vector with 3,072 numbers).

**(c) What is the total input sequence length to the Transformer?**
*   *How to solve:* The Transformer reads the patches like words in a sentence. How many "words" are there? We calculated 64 patches in part (a).
*   *The TRAP:* Do not forget the **`[CLS]` (Classification) Token**! As we learned in Lecture 16, ViTs *always* prepend one extra fake token to the sequence to absorb the final prediction context.
*   *Result:* 64 patches + 1 `[CLS]` token = **$\mathbf{65}$ sequence length**.

---
- ### **2. Self-Attention Calculation (Slide 22)**
  
  This is the core math of the ChatGPT era: $Attention(Q, K, V) = \text{softmax}(\frac{QK^T}{\sqrt{d_k}})V$.
  
  **The Prompt:** Given a simple self-attention setup with one token attending to two tokens:
  *   $Q = [1, 0]$
  *   $K_1 = [1, 1], \; K_2 =[0, 1]$
  *   $V_1 = [2, 0], \; V_2 =[0, 3]$
  
  **(a) Compute attention scores using dot product: $Q \cdot K_1$ and $Q \cdot K_2$**
  *   *Score 1:* $(1 \times 1) + (0 \times 1) = 1 + 0 = \mathbf{1}$
  *   *Score 2:* $(1 \times 0) + (0 \times 1) = 0 + 0 = \mathbf{0}$
  *   *(Meaning: The Query strongly matches Key 1, but has zero relation to Key 2).*
  
  **(b) Apply softmax to obtain attention weights ($s_i$):**
  *   *Formula provided:* $\text{softmax}(s_i) = \frac{e^{s_i}}{\sum e^{s_j}}$
  *   *Denominator (The Sum):* $e^1 + e^0$. (Remember, $e^0 = 1$, and $e^1 \approx 2.718$).
  *   *Weight 1 ($\alpha_1$):* $\frac{e^1}{e^1 + e^0}$
  *   *Weight 2 ($\alpha_2$):* $\frac{e^0}{e^1 + e^0}$
  *   *(The prompt says you can leave answers in exponential form, so you don't need to calculate the exact decimals on the exam!).*
  
  **(c) Compute the final output vector: $\text{Output} = \alpha_1V_1 + \alpha_2V_2$**
  *   *Setup:* $(\frac{e^1}{e^1 + e^0}) \times [2, 0] \;+\; (\frac{e^0}{e^1 + e^0}) \times [0, 3]$
  *   *Distribute:* $[\frac{2e^1}{e^1 + e^0}, 0] \;+\; [0, \frac{3e^0}{e^1 + e^0}]$
  *   *Add Vectors:* **$[\frac{2e^1}{e^1 + 1}, \frac{3}{e^1 + 1}]$**
  
  ---
- ### **3. CNN Forward Pass Calculation (Slides 23 & 25)**
  
  This tests your understanding of how a Kernel (Filter) slides across an image.
- #### **The Quick Formula (Slide 23):**
  An image of size $32 \times 32$ is passed through a convolution layer with: Filter size = $5 \times 5$; Stride = 1; Padding = 0.
  **(a) Compute output width/height:**
  *   *The Standard Formula:* $\frac{W - F + 2P}{S} + 1$
  *   *Math:* $\frac{32 - 5 + 0}{1} + 1 = 27 + 1 = \mathbf{28 \times 28}$.
  **(b) If we use padding = 2, what is the new output size?**
  *   *Math:* $\frac{32 - 5 + 2(2)}{1} + 1 = \frac{32 - 5 + 4}{1} + 1 = \frac{31}{1} + 1 = \mathbf{32 \times 32}$.
  *   *(Notice how Padding=2 perfectly preserved the original image size!).*
  **(c) How many parameters per filter?**
  *   *Math:* A $5 \times 5$ filter has 25 weights, plus **1 Bias**. $25 + 1 = \mathbf{26 \text{ parameters}}$.
- #### **The Full Manual Calculation (Slide 25):**
  **Part A: Convolution (Stride = 1, Padding = 0)**
  *   **Image ($X$):** A $4 \times 4$ grid.
  *   **Kernel ($K$):** A $2 \times 1$ grid: `[1] / [-1]` (A vertical edge detector).
  *   **Task (a): Compute the output feature map.**
    *   *Top-Left placement:* Overlay `[1] / [-1]` on the first column `[1] / [4]`.
        *   Math: $(1 \times 1) + (4 \times -1) = 1 - 4 = \mathbf{-3}$.
    *   *Move right (Stride 1):* Overlay on `[2] / [1]`.
        *   Math: $(2 \times 1) + (1 \times -1) = 2 - 1 = \mathbf{1}$.
    *   *Move right:* Overlay on `[0] / [3]`.
        *   Math: $(0 \times 1) + (3 \times -1) = \mathbf{-3}$.
    *   *Move right:* Overlay on `[3] / [1]`.
        *   Math: $(3 \times 1) + (1 \times -1) = \mathbf{2}$.
    *   *(You would repeat this for the middle rows and bottom rows to build the full $3 \times 4$ grid).*
  *   **Task (b): What is the output size?**
    *   *Formula:* $\frac{4 - 2 + 0}{1} + 1 = \mathbf{3}$ (Height). The width stays **4** because the filter is only 1 pixel wide. Output is $\mathbf{3 \times 4}$.
  
  **Part C: Max Pooling**
  *   Take your $3 \times 4$ output grid from Part A and apply a $2 \times 2$ Max Pool window with Stride 2.
  *   *Concept:* You just draw a $2 \times 2$ box over the top-left corner of the grid, find the single largest number inside that box, and throw the other three away. Move the box over 2 columns (because Stride=2) and repeat.
  
  ---