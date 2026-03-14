## 1. Deriving the Update Rule (Slide 36)
We established that we want to minimize the loss for misclassified points:
$$ L(w, b) = - \sum_{M} y_i (w^T x_i + b) $$

To find the minimum, we take the derivative (gradient) with respect to the weights $w$:
$$ \nabla_w L = - \sum_{M} y_i x_i $$

**The Gradient Descent Step:**
Recall the general rule: $w \leftarrow w - \eta \nabla L$.
Substituting our specific gradient:
$$ w \leftarrow w - \eta (-y_i x_i) $$
$$ \mathbf{w} \leftarrow \mathbf{w} + \eta y_i x_i $$

**The Bias Update:**
Similarly for the bias $b$:
$$ b \leftarrow b + \eta y_i $$

**The Intuition:**
*   **If the model falsely predicts Negative ($y=+1$, pred $<0$):** We **add** the vector $x$ to the weights. This pulls the weight vector *towards* $x$, making the dot product larger (more positive).
*   **If the model falsely predicts Positive ($y=-1$, pred $>0$):** We **subtract** the vector $x$ from the weights. This pushes the weight vector *away* from $x$, making the dot product smaller (more negative).
- ## 2. Stochastic Gradient Descent (SGD) (Slide 31, 36)
  The standard gradient descent calculates the error for *all* points before taking one step. This is slow.
  **The Perceptron uses SGD:**
  1.  Pick **one** random sample $(x_i, y_i)$.
  2.  Check if it is misclassified.
  3.  If yes, update $w$ and $b$ immediately based *only* on that point.
  4.  Repeat.
  
  *   *Benefit:* It is much faster and helps the model "wiggle" out of bad spots.
  
  ---
- ## 3. Walkthrough: Numerical Example 1 (Slides 38–40)
  This is a classic exam problem format.
  
  **Setup:**
  *   $\eta = 1$
  *   **Data:**
    *   $x_1 = [3, 3]^T, y=+1$
    *   $x_2 = [4, 3]^T, y=+1$
    *   $x_3 = [1, 1]^T, y=-1$
  *   **Initialization:** $w_0 = [2, 1]^T, b_0 = -0.5$.
  
  **Step 1: Check Current Predictions (Slide 38)**
  *   **Check $x_1$:** $w^T x_1 + b = (2\cdot3 + 1\cdot3) - 0.5 = 8.5$. ($>0$, Correct).
  *   **Check $x_2$:** $w^T x_2 + b = (2\cdot4 + 1\cdot3) - 0.5 = 10.5$. ($>0$, Correct).
  *   **Check $x_3$:** $w^T x_3 + b = (2\cdot1 + 1\cdot1) - 0.5 = 2.5$.
    *   **Result:** Predicted Positive ($>0$), but Label is Negative ($-1$). **ERROR.**
  
  **Step 2: Update Weights (Slide 39)**
  We update using the misclassified point $x_3$:
  *   $w_{new} = w_{old} + \eta y_3 x_3$
    *   $w_{new} = [2, 1] + 1 \cdot (-1) \cdot [1, 1]$
    *   $w_{new} = [2, 1] - [1, 1] = \mathbf{[1, 0]}$
  *   $b_{new} = b_{old} + \eta y_3$
    *   $b_{new} = -0.5 + 1 \cdot (-1) = \mathbf{-1.5}$
  
  **Step 3: Verify Convergence (Slide 40)**
  With new weights $w=[1, 0], b=-1.5$:
  *   **Check $x_1$:** $1(3) + 0(3) - 1.5 = 1.5$. ($>0$, Correct).
  *   **Check $x_2$:** $1(4) + 0(3) - 1.5 = 2.5$. ($>0$, Correct).
  *   **Check $x_3$:** $1(1) + 0(1) - 1.5 = -0.5$. ($<0$, Correct).
  *   **Result:** All points correctly classified. Algorithm stops.
  
  ---
- ## 4. Walkthrough: Numerical Example 2 (Slides 41–45)
  **Setup:** Same data, but different initialization.
  *   **Init:** $w_0 = [0.5, -1]^T, b_0 = 0$.
  
  **Iteration 1:**
  *   Check $x_1$: $0.5(3) - 1(3) = 1.5 - 3 = -1.5$.
    *   Predicted Negative, Label Positive. **ERROR.**
  *   **Update using $x_1$:**
    *   $w_{new} = [0.5, -1] + (+1)[3, 3] = \mathbf{[3.5, 2]}$.
    *   $b_{new} = 0 + (+1) = \mathbf{1}$.
  
  **Iteration 2 (Slide 43):**
  *   Now check $x_3$ with new weights ($w=[3.5, 2], b=1$).
    *   $3.5(1) + 2(1) + 1 = 6.5$.
    *   Predicted Positive, Label Negative. **ERROR.**
  *   **Update using $x_3$:**
    *   $w_{new} = [3.5, 2] + (-1)[1, 1] = \mathbf{[2.5, 1]}$.
    *   $b_{new} = 1 + (-1) = \mathbf{0}$.
  
  *(The slides continue this process until all errors are resolved).*
  
  ***
- # Problem Solving & Concept Check
  
  **1. The Update Logic:**
  You have weights $w=[10, 10]$. You misclassify a point $x=[2, 2]$ which has label $y=-1$.
  *   Without doing the math, will the new weights be **larger** or **smaller** than $[10, 10]$?
  *   *Answer:* **Smaller.** Since $y=-1$, we subtract the input vector. We are reducing the weights to make the dot product smaller (more negative).
  
  **2. Learning Rate Impact:**
  In Example 1, we used $\eta = 1$ and converged in 1 step. Suppose we used $\eta = 0.0001$.
  *   What would the new $w$ be after the first update?
    *   $w_{new} = [2, 1] - 0.0001[1, 1] = [1.9999, 0.9999]$.
  *   *Consequence:* The line would barely move. It would likely still misclassify $x_3$ in the next round, requiring thousands of iterations to move the line far enough.
  
  **3. Stopping Condition:**
  When does the Perceptron algorithm stop?
  *   *Answer:* When it completes a full pass through the dataset (an **Epoch**) without making a single error. (Assuming the data is linearly separable).
  
  ***