### **1. What is "Deep" Learning? (Slides 16–17)**
Why do we call it "Deep" Learning? Because it involves stacks of layers, where each layer learns a more abstract concept than the one before it.

*   **The Hierarchy of Abstraction (Slide 16):**
  *   **Input:** Raw pixels (a photo of a dog).
  *   **Low Level:** The first layers might just detect **edges** or **colors** (Red vs Green, Horizontal line vs Vertical line).
  *   **Mid Level:** The network combines edges to find **shapes** (an eye, an ear, a snout).
  *   **High Level:** The network combines shapes to find **concepts** (Dog vs. Wolf).
*   **Multimodal AI (Slide 17):**
  *   The slide shows an image (Vision) generating a caption (Text).
  *   *How?* A **CNN** (Convolutional Neural Network) processes the pixels into a "Math Vector" representing the image content. An **RNN** (Recurrent Neural Network) takes that vector and translates it into English words.

---
- ### **2. The Goal: Minimizing Loss (Slides 18–20)**
  Before we can use calculus, we have to define exactly what "Mistake" means mathematically.
  
  *   **The Prediction ($\hat{y}$ or $f(x)$):** What the model *thinks* the answer is.
  *   **The Target ($y$):** What the answer *actually* is.
  *   **The Loss Function ($\mathcal{L}$):** (Slide 20)
    $$ \mathcal{L}(w) = (y - \hat{y})^2 $$
    *   This is the **Squared Error**.
    *   If the model predicts 0.8 and the answer is 1.0, the error is $(1.0 - 0.8)^2 = 0.04$.
    *   **The Objective:** We want to find the specific Weights ($w$) that make $\mathcal{L}$ as close to **0** as possible.
  
  ---
- ### **3. The Map: Computational Graphs (Slides 22–23)**
  To solve this using computers, we draw the math as a flow diagram. This is exactly how **PyTorch** sees your code.
  
  *   **The Nodes ($h, f$):** These represent mathematical operations (addition, multiplication, activation functions).
  *   **The Edges ($w$):** These represent the **Weights** (the parameters we want to learn).
  *   **The Flow (Forward Pass):**
    1.  Start with Inputs ($x_1, x_2$).
    2.  Calculate Hidden Layer ($h_1, h_2$) using weights ($w^{(1)}$).
    3.  Calculate Output ($f$) using weights ($w^{(2)}$).
    *   *Key Takeaway:* You cannot calculate the output $f$ until you have calculated the middle steps $h$. This is a **Dependency Chain**.
  
  ---
- ### **4. The Engine: Derivatives & Gradient Descent (Slides 21, 24–26)**
  This is the heart of AI. How does the model know how to fix itself?
- #### **A. The Derivative (Slope)**
  The derivative ($\frac{d\mathcal{L}}{dw}$) answers a specific question: **"If I nudge this weight slightly up, does the Error go up or down?"**
  
  *   **Visualizing the Slope (Slide 25):**
    *   Imagine the Loss Function is a U-shaped valley. We want to be at the bottom (Minimum Loss).
    *   **Positive Slope:** We are on the right side of the valley. Going *right* (increasing $w$) makes us go higher (more error). We need to go **Left**.
    *   **Negative Slope:** We are on the left side. Going *right* (increasing $w$) makes us go lower (less error). We need to go **Right**.
- #### **B. The Update Rule (Gradient Descent)**
  (Slide 24 & 26)
  $$ w_{new} \leftarrow w_{old} - \eta \frac{d\mathcal{L}}{dw} $$
  
  Let's break down this formula part-by-part:
  *   $w_{new}$: The updated, smarter weight.
  *   $w_{old}$: The current, dumb weight.
  *   **The Minus Sign ($-$)**: This is the "Descent." It forces us to move in the **opposite** direction of the slope. (If slope is positive, we subtract. If slope is negative, we add).
  *   **$\eta$ (Eta) / Learning Rate:** This determines **Step Size**.
    *   If $\eta$ is too big, you overshoot the bottom of the valley.
    *   If $\eta$ is too small, it takes 100 years to reach the bottom.
  *   **$\frac{d\mathcal{L}}{dw}$:** The Gradient (The steepness of the slope).
- #### **C. Backpropagation (Slide 21)**
  Forward Pass goes $x \to h \to f$ to calculate the **Error**.
  Backpropagation goes $f \to h \to x$ to calculate the **Blame** (Gradient).
  
  *   *The Chain Rule:* To know how to change a weight in the *first* layer ($w^{(1)}$), we have to calculate how it affected the *middle* layer ($h$), which affected the *output* ($f$), which affected the *Loss* ($\mathcal{L}$).
    $$ \frac{\partial \mathcal{L}}{\partial w^{(1)}} = \frac{\partial \mathcal{L}}{\partial f} \times \frac{\partial f}{\partial h} \times \frac{\partial h}{\partial w^{(1)}} $$
  *   PyTorch handles this chain calculation automatically (AutoGrad).
  
  ---
- ### **5. Logistics Update (Slide 27)**
  *   **Spring Break:** March 22 – March 30, 2026.
  *   **Syllabus Impact:** Content will be shifted forward. This implies the schedule is tight; falling behind on Homework 1 (The Housing Price Regression) will make catching up very difficult.
  
  ---