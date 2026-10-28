### **1. Descriptive Statistics Practice (Slide 7)**
- #### **Problem A: The Array Calculation**
  You are given the dataset: `[2, 4, 4, 6, 10]`
  *   **(a) Compute the mean:**
    *   *Math:* $\mu = \frac{2 + 4 + 4 + 6 + 10}{5} = \frac{26}{5} = \mathbf{5.2}$
  *   **(b) Compute the variance:**
    *   *Formula:* $\frac{1}{n} \sum (x_i - \mu)^2$
    *   *Step 1 (Subtract Mean):* $(2-5.2)=-3.2$, $(4-5.2)=-1.2$, $(4-5.2)=-1.2$, $(6-5.2)=0.8$, $(10-5.2)=4.8$
    *   *Step 2 (Square them):* $10.24, 1.44, 1.44, 0.64, 23.04$
    *   *Step 3 (Average them):* $\frac{10.24 + 1.44 + 1.44 + 0.64 + 23.04}{5} = \frac{36.8}{5} = \mathbf{7.36}$
  *   **(c) What does variance measure?**
    *   *Answer:* It measures the **spread or dispersion** of the data points around the mean.
  *   **(d) What is the median?**
    *   *Answer:* The data is already sorted. The middle number is **4**.
- #### **Problem B: Multiple Choice**
  *   **If variance is large, it means:**
    *   *Answer:* **B. Data points are widely spread.**
  *   **Which statement is TRUE about a Probability Mass Function (PMF)?**
    *   *Answer:* **B. The sum of all probabilities equals 1.** *(Note: A is wrong because PMF is for discrete variables. C is wrong because "area under the curve" refers to continuous PDFs. D is wrong because probability can never be negative).*
- #### **Problem C: The PMF Algebra Problem**
  A startup tracks daily complaints. The probabilities are $P(X=x) = kx$, for $x = 1, 2, 3, 4$.
  *   **(a) Find the value of k:**
    *   *Rule:* All probabilities must sum to 1.
    *   $k(1) + k(2) + k(3) + k(4) = 1$
    *   $10k = 1 \rightarrow \mathbf{k = 0.1}$
  *   **(b) Compute $P(X > 2)$:**
    *   This means the probability of 3 *or* 4 complaints.
    *   $P(3) + P(4) = (0.1 \times 3) + (0.1 \times 4) = 0.3 + 0.4 = \mathbf{0.7}$ (or 70%).
  *   **(c) Compute the Variance of X:**
    *   *Formula:* $Var(X) = E[X^2] - (E[X])^2$
    *   *Expected Value (Mean):* $(1 \times 0.1) + (2 \times 0.2) + (3 \times 0.3) + (4 \times 0.4) = 0.1 + 0.4 + 0.9 + 1.6 = \mathbf{3.0}$
    *   *Expected Value of Squares:* $(1^2 \times 0.1) + (2^2 \times 0.2) + (3^2 \times 0.3) + (4^2 \times 0.4) = 0.1 + 0.8 + 2.7 + 6.4 = \mathbf{10.0}$
    *   *Variance:* $10.0 - (3.0)^2 = 10.0 - 9.0 = \mathbf{1.0}$
  
  ---
- ### **2. Derivatives in Deep Learning Practice (Slide 8)**
  
  *   **Basic Derivative:** If $f(x) = x^2 + x$, compute $\frac{df}{dx}$
    *   *Answer:* Use the Power Rule. $2x + 1$.
  *   **Gradient Chain Rule:** If $f(x) = (bx - a)^2$, compute $\frac{df}{dx}$
    *   *Answer:* You must use the Chain Rule (Outside derivative $\times$ Inside derivative).
    *   *Outside:* $2(bx - a)^1$
    *   *Inside:* The derivative of $(bx - a)$ with respect to $x$ is just $b$.
    *   *Result:* $2(bx - a) \times b = \mathbf{2b(bx - a)}$
  *   **Why are derivatives important in training neural networks?**
    *   *Answer:* They tell the model which direction (and how much) to adjust the weights in order to move "downhill" and **minimize the loss function**.
  
  ---
- ### **3. Supervised Learning Intuition (Slide 9)**
  
  *   **What is a loss function? Give one example.**
    *   *Answer:* A mathematical formula that calculates the error (difference) between the model's prediction and the actual correct answer. An example is **Mean Squared Error (MSE)**.
  *   **A model gets 99% training accuracy and 60% test accuracy. Is this a good model? What is happening?**
    *   *Answer:* **No.** The model is suffering from **Overfitting**. It has memorized the training data perfectly, but failed to learn the underlying patterns, causing it to perform poorly on new, unseen data.
  
  ---
- ### **4. The Gradient Descent Loop Calculation (Slides 12–14)**
  This question asks you to update a 3-parameter model: $\hat{y} = w_1x_1 + w_2x_2 + w_3x_3$
  Initial weights: $(1, 1, 1)$. Learning rate: $0.1$.
  
  Iteration 1: Training Sample $(x_1=2, x_2=1, x_3=1)$, Target $y=4$
  1.  **Prediction:** $\hat{y} = (1 \times 2) + (1 \times 1) + (1 \times 1) = 4$
  2.  **Loss:** $\frac{1}{2}(4 - 4)^2 = 0$
  3.  **Gradients:** Because the prediction perfectly matched the target, the error $(\hat{y} - y)$ is 0. Therefore, all gradients are $0$.
  4.  **Update:** $w_{new} = 1 - 0.1(0) = 1$. The weights do not change. **$(1, 1, 1)$**.
  
  Iteration 2: Training Sample $(x_1=0, x_2=1, x_3=3)$, Target $y=2$ *(Using weights from iteration 1)*
  1.  **Prediction:** $\hat{y} = (1 \times 0) + (1 \times 1) + (1 \times 3) = 4$
  2.  **Loss:** $\frac{1}{2}(4 - 2)^2 = 2$
  3.  **Gradients:** The error term $(\hat{y} - y)$ is $(4 - 2) = \mathbf{2}$.
    *   $\frac{\partial L}{\partial w_1} = \text{Error} \times x_1 = 2 \times 0 = \mathbf{0}$
    *   $\frac{\partial L}{\partial w_2} = \text{Error} \times x_2 = 2 \times 1 = \mathbf{2}$
    *   $\frac{\partial L}{\partial w_3} = \text{Error} \times x_3 = 2 \times 3 = \mathbf{6}$
  4.  **Update:**
    *   $w_1 = 1 - 0.1(0) = \mathbf{1.0}$
    *   $w_2 = 1 - 0.1(2) = 1 - 0.2 = \mathbf{0.8}$
    *   $w_3 = 1 - 0.1(6) = 1 - 0.6 = \mathbf{0.4}$
  *   *Final Weights after Iteration 2:* $(1.0, 0.8, 0.4)$
  
  ---
- ### **Action Items for Section 2:**
  *   **Write these down:** Grab a piece of paper and write out the PMF variance problem and the Iteration 2 Gradient Descent update. Writing it manually builds the muscle memory you will need since you cannot use AI on the quiz.
  *   **Memorize the Gradient Formula:** For a simple linear model and squared error, the gradient for any weight $w_i$ is always $(\text{Prediction} - \text{Actual}) \times x_i$.