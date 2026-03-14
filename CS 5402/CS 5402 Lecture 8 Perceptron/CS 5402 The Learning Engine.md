## 1. Defining the Loss Function (Slides 8–16)
We want the computer to find the best weights ($w$) and bias ($b$). To do that, we must define a **Loss Function** $L(w,b)$ that represents "how bad" the current model is. The goal is to minimize this loss.
- ### Attempt 1: The "0-1 Loss" (Slide 9)
  *   **Method:** Just count the number of misclassified points.
    *   $L = \sum I(y_{pred} \neq y_{true})$
  *   **Why it fails:** This function is discrete and looks like a staircase.
    *   **The Gradient Problem:** The derivative (slope) is **0** everywhere except at the exact moment a point flips from "Wrong" to "Right."
    *   *Result:* Gradient Descent cannot work. If the slope is 0, the algorithm doesn't know which direction to move to improve. It's like being on a flat plateau in the dark.
- ### Attempt 2: The Perceptron Criterion (Slides 10–16)
  *   **Method:** Instead of asking *if* a point is wrong, we look at **how wrong** it is (the distance from the boundary).
  *   **The Condition:** For a point to be correctly classified, the sign of the output ($w^T x + b$) must match the sign of the label ($y_i$).
    *   Therefore, for a correct prediction: $y_i (w^T x_i + b) > 0$.
    *   For a **misclassified** point: $y_i (w^T x_i + b) < 0$.
  
  **The Loss Formula:**
  We define the loss as the sum of the absolute values of the "scores" of the misclassified points. Since $y(w^Tx+b)$ is negative for errors, we stick a negative sign in front to make the Loss positive (so we can minimize it).
  
  $$ L(w, b) = - \sum_{x_i \in M} y_i (w^T x_i + b) $$
  *(Where $M$ is the set of misclassified points)*.
  
  *   **Why this works:** This function is continuous and (mostly) differentiable.
    *   If a point is slightly on the wrong side, the Loss is small.
    *   If a point is far on the wrong side, the Loss is huge.
    *   The gradient points directly toward the decision boundary, telling the weights exactly how to fix the error.
  
  ---
- ## 2. Gradient Descent: The Intuition (Slides 17–22)
  Once we have a Loss Function, we need an algorithm to minimize it. That algorithm is **Gradient Descent (GD)**.
- ### The Hiker's Analogy (Slide 20-22)
  Imagine you are a hiker stuck on a mountain in a thick fog. You want to get to the lowest point (the valley), but you can't see where it is.
  1.  **Initialize:** You start at a random location ($w_{initial}$).
  2.  **Gradient:** You feel the ground with your feet to find which direction is the steepest **uphill**.
  3.  **Update:** You take a step in the **opposite** direction (downhill).
  4.  **Repeat:** You keep stepping until the ground flattens out (slope = 0).
- ### The Math (Slide 17)
  $$ \mathbf{w} \leftarrow \mathbf{w} - \eta \nabla L(\mathbf{w}) $$
  *   $\mathbf{w}$: The weights (current position).
  *   $\nabla L(\mathbf{w})$: The Gradient (slope). We subtract it to go downhill.
  *   $\eta$ (Eta): The **Learning Rate**.
  
  ---
- ## 3. The Learning Rate ($\eta$) (Slide 25)
  This is the most critical hyperparameter in Gradient Descent. It controls the **Step Size**.
  
  *   **If $\eta$ is too small:** The hiker takes microscopic steps. It will take forever to reach the bottom.
  *   **If $\eta$ is too large:** The hiker takes giant leaps. They might jump *over* the valley and land on the mountain on the other side (Overshooting). The loss might actually increase (Divergence).
  *   **Goldilocks Zone:** You want a rate that is large enough to be fast, but small enough to converge stably.
  
  ---
- ## 4. Convexity (Slide 19)
  The slides briefly mention that GD works best on **Convex Functions**.
  *   **Visual:** A convex function looks like a bowl (Slide 32). It has only one bottom (**Global Minimum**).
  *   **Implication:** If your Loss Function is convex (like Linear Regression or Perceptron), Gradient Descent is **guaranteed** to find the best solution (eventually).
  *   **Non-Convex:** If the landscape looks like a mountain range with many valleys (like Deep Neural Networks), GD might get stuck in a "Local Minimum" (a shallow valley) and miss the "Global Minimum" (the deepest canyon).
  
  ***
- # Problem Solving & Concept Check
  
  **1. Loss Function Logic:**
  Suppose we use the Perceptron Loss.
  *   Point A has label $y=+1$. The model predicts raw score $-5$.
  *   Point B has label $y=+1$. The model predicts raw score $-0.1$.
  *   Which point contributes more to the Loss?
    *   *Calculation:*
        *   Loss A: $-(1 \times -5) = 5$.
        *   Loss B: $-(1 \times -0.1) = 0.1$.
    *   *Answer:* **Point A**. It is "more wrong" (further from the boundary), so the algorithm will try harder to fix it.
  
  **2. Gradient Direction:**
  If the slope of the loss curve is **Positive** (uphill to the right), which way should the weight move?
  *   *Answer:* **Left**.
  *   *Math:* $w_{new} = w_{old} - (\text{Positive number})$. The weight decreases.
  
  **3. The "Zero" Case:**
  What is the Loss for a point that is perfectly on the decision boundary ($w^T x + b = 0$)?
  *   *Answer:* **Zero.** (Technically, the perceptron considers 0 a mistake if the implementation demands strictly $>0$, but mathematically the magnitude of error is 0).
  
  ***