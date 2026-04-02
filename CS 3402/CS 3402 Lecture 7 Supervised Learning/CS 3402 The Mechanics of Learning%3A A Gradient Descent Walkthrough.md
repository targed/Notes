#### **1. The Hierarchy (Slide 2)**
Before the math, understanding the scope is crucial:
*   **AI:** The broad umbrella.
*   **Machine Learning (ML):** A subset of AI.
  *   *Supervised:* You have the Answer Key (Labels).
  *   *Unsupervised:* No Answer Key (Clustering).
*   **Deep Learning (DL):** A subset of ML using Neural Networks.
- #### **2. The "Toy Example" Setup (Slide 4)**
  We are building a tiny neural network without hidden layers (Linear Regression).
  
  *   **The Model:** $\hat{y} = w_1x_1 + w_2x_2$ (A simple weighted sum).
  *   **The Data (Sample):**
    *   Inputs: $x_1 = 2, x_2 = 1$
    *   Target (Actual Answer): $y = 4$
  *   **The Weights (Parameters):** $w_1 = 1, w_2 = 1$ (Initialized randomly).
  *   **The Hyperparameters:** Learning Rate $\eta = 0.1$.
- #### **3. Step 1: The Forward Pass (Slides 10–11)**
  We make a prediction based on our current (dumb) weights.
  *   **Calculation:**
    $$ \hat{y} = (1 \times 2) + (1 \times 1) = 3 $$
  *   **The Error:** We predicted **3**, but the answer is **4**.
  *   **The Loss (MSE):** $\mathcal{L} = \frac{1}{2}(3 - 4)^2 = 0.5$.
    *   *Note:* The $\frac{1}{2}$ is often added to Squared Error so that when you take the derivative, the exponent $2$ cancels out the fraction $\frac{1}{2}$.
- #### **4. Step 2: The Backward Pass (Calculating Gradients) (Slides 6–8, 12)**
  Now we use the **Chain Rule** to find out *who is to blame* for the error. We need $\frac{\partial \mathcal{L}}{\partial w_1}$ (How much did $w_1$ contribute to the error?).
  
  *   **The Chain Rule Logic:**
    $$ \frac{\partial \mathcal{L}}{\partial w_1} = \frac{\partial \mathcal{L}}{\partial \hat{y}} \times \frac{\partial \hat{y}}{\partial w_1} $$
    1.  **Derivative of Loss w.r.t Prediction:** $(\hat{y} - y) = (3 - 4) = -1$.
    2.  **Derivative of Prediction w.r.t Weight 1:** $x_1 = 2$.
  *   **The Resulting Gradient:**
    $$ \frac{\partial \mathcal{L}}{\partial w_1} = -1 \times 2 = -2 $$
    *   *Interpretation:* The slope is negative (-2). If we increase $w_1$, the loss will go down.
- #### **5. Step 3: The Update (Gradient Descent) (Slides 9, 13)**
  Now we change the weights to make them "smarter."
  $$ w_{new} = w_{old} - (\eta \times \text{Gradient}) $$
  
  *   **Update $w_1$:**
    $$ 1 - (0.1 \times -2) = 1 + 0.2 = \mathbf{1.2} $$
  *   **Update $w_2$:**
    $$ 1 - (0.1 \times -1) = 1 + 0.1 = \mathbf{1.1} $$
- #### **6. Validation: Did it work? (Slide 15)**
  We run the Forward Pass again with the **new weights** (1.2 and 1.1).
  *   **New Prediction:** $(1.2 \times 2) + (1.1 \times 1) = 2.4 + 1.1 = \mathbf{3.5}$.
  *   **Result:** 3.5 is closer to 4 than our old prediction (3.0).
  *   **New Loss:** $\frac{1}{2}(3.5 - 4)^2 = 0.125$.
  *   **Conclusion:** The Loss dropped from **0.5** to **0.125**. The model learned!
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **The "Black Box" of Deep Learning:** Slide 17 lists modern models like **GPT-4**.
    *   In our toy example, we updated **2 parameters** ($w_1, w_2$) by hand.
    *   GPT-4 has roughly **1.7 Trillion parameters**.
    *   *The Concept:* The math is *exactly the same*. We just need massive GPUs (like the Missouri S&T "Mill" cluster) to perform these simple multiplication/subtraction steps billions of times in parallel.
  *   **Convexity:** Slide 25 (from the previous deck, referenced here in the gradient diagrams) implies we slide down a curve. In Linear Regression (like this toy example), the curve is a perfect bowl (Convex). You are guaranteed to find the bottom. In Deep Learning, the surface is rugged mountains; you might get stuck in a "Local Minima" (a fake bottom).
  
  ---
- ### **Action Items for Section 1:**
  *   **Math Check:** Can you replicate the calculation on Slide 12 manually? If the Learning Rate ($\eta$) was **1.0** instead of **0.1**, what would happen?
    *   *Answer:* $w_1 = 1 - (1.0 \times -2) = 3$. New pred = $3(2)+2(1) = 8$. The prediction jumped from 3 to 8 (overshot the target of 4). This explains why Learning Rate must be small.