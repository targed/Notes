## 1. The Algorithm Setup (Slides 27–28)
**Goal:** Build a strong classifier $H(x)$ from a sequence of weak classifiers $h_t(x)$.

**Initialization:**
We give every data point an equal weight at the start.
$$ D_1(i) = \frac{1}{N} $$
*(Where $N$ is the total number of training samples).*
- ## 2. The Four-Step Loop
  For each iteration $t$:
- ### Step A: Calculate Weighted Error ($\epsilon_t$)
  We train a weak learner (stump) to minimize the error. However, we don't just count mistakes; we sum the **weights** of the mistakes.
  $$ \epsilon_t = \sum_{\text{mistakes}} D_t(i) $$
  *   *Note:* A mistake on a "heavy" point counts much more than a mistake on a "light" point.
- ### Step B: Calculate "Amount of Say" ($\alpha_t$)
  Once we know how good the model is ($\epsilon_t$), we assign it a voting weight ($\alpha_t$).
  **The Formula:**
  $$ \alpha_t = \frac{1}{2} \ln \left( \frac{1 - \epsilon_t}{\epsilon_t} \right) $$
  
  **The Intuition (Slide 29):**
  *   **If Error $\approx$ 0:** The model is perfect. $\frac{1-\epsilon}{\epsilon} \to \infty$. $\alpha$ becomes huge. The model gets a massive vote.
  *   **If Error $\approx$ 0.5:** The model is guessing randomly. $\frac{0.5}{0.5} = 1$. $\ln(1) = 0$. $\alpha = 0$. The model gets **zero** vote.
  *   **If Error $\approx$ 1.0:** The model is perfectly wrong. $\alpha$ becomes negative. We actually subtract its vote (invert its prediction).
- ### Step C: Update Weights ($D_{t+1}$)
  We need to prepare the data for the *next* round. We increase weights for mistakes and decrease weights for correct predictions.
  $$ D_{t+1}(i) = \frac{D_t(i) \cdot \exp(-\alpha_t y_i h_t(x_i))}{Z_t} $$
  
  *   **If Correct ($y_i = h_t(x_i)$):** $y \cdot h = 1$. We multiply by $e^{-\alpha}$ (a number $<1$). **Weight Decreases.**
  *   **If Wrong ($y_i \neq h_t(x_i)$):** $y \cdot h = -1$. We multiply by $e^{\alpha}$ (a number $>1$). **Weight Increases.**
  *   **$Z_t$:** A normalization factor to ensure all new weights sum up to 1.
- ### Step D: The Final Vote
  After $T$ rounds, the final prediction is the weighted sum of all models:
  $$ H(x) = \text{sign} \left( \sum_{t=1}^{T} \alpha_t h_t(x) \right) $$
  
  ---
- ## 3. Walkthrough: The Numerical Example (Slides 30–40)
  This 10-point example is a classic exam question format.
  
  **Data:** 10 points ($x=0$ to $9$). Initial weights $0.1$ each.
  **Labels:** `+ + + - - - + + + -`
- ### Iteration 1 (Slides 32–35)
  *   **Model:** The best stump splits at $x < 2.5$.
    *   It correctly classifies indices 0,1,2,3,4,5,9.
    *   **Mistakes:** Indices 6, 7, 8 (Values $x=6,7,8$ are "+" but fall in the $\ge 2.5$ "-" zone).
  *   **Calculate Error:** Three mistakes, each weight 0.1.
    $$ \epsilon_1 = 0.3 $$
  *   **Calculate Alpha:**
    $$ \alpha_1 = 0.5 \ln \left( \frac{0.7}{0.3} \right) \approx \mathbf{0.42} $$
  *   **Update Weights:**
    *   **Correct points (x7):** $0.1 \times e^{-0.42} \approx 0.065$
    *   **Wrong points (x3):** $0.1 \times e^{0.42} \approx 0.15$
    *   *Normalize:* Divide by sum so they equal 1.
    *   **Result:** The wrong points (6, 7, 8) are now "heavier" (0.166) than the others (0.07).
- ### Iteration 2 (Slide 36-37)
  *   **Model:** The new model *must* fix the heavy points (6, 7, 8). It chooses a split at $x < 8.5$.
    *   It fixes 6, 7, 8.
    *   **New Mistakes:** Indices 0, 1, 2 (Values $x=0,1,2$ are "+" but fall in the new "-" zone).
  *   **Calculate Error:** The error looks like 3 mistakes again, but we use the **New Weights**. The weights for 0, 1, 2 were shrunk in the previous round to 0.07.
    $$ \epsilon_2 = 3 \times 0.0714 \approx \mathbf{0.21} $$
    *(Note: The error went down because the mistakes are on "low priority" points).*
  *   **Calculate Alpha:**
    $$ \alpha_2 = 0.5 \ln \left( \frac{1-0.21}{0.21} \right) \approx \mathbf{0.65} $$
    *(Note: This model gets a higher vote than the first one because its weighted error was lower).*
  
  ***
- # Problem Solving & Concept Check
  
  **1. Alpha Logic:**
  In Round 1, your model has an error of 0.1 (very good). In Round 10, your model has an error of 0.4 (mediocre).
  *   Which model gets a higher $\alpha$ (voting power)?
  *   *Answer:* **Round 1.** The formula $\frac{1-\epsilon}{\epsilon}$ is large when $\epsilon$ is small. AdaBoost trusts accurate models more.
  
  **2. Weight Dynamics:**
  If a specific data point is extremely hard to classify (e.g., an outlier), what happens to its weight over 100 iterations?
  *   *Answer:* Its weight will **explode**. Every time a model misses it, the weight is multiplied by $e^\alpha$. The algorithm will eventually focus *only* on that outlier, trying desperately to fix it. This is why AdaBoost is sensitive to noise.
  
  **3. The Stop Condition:**
  What happens if a weak learner achieves an error of exactly 0?
  *   *Math:* $\ln(\frac{1}{0}) = \ln(\infty) = \infty$.
  *   *Result:* $\alpha$ becomes infinite. That single model dominates the entire ensemble. The algorithm effectively stops (or requires regularization to handle the infinity).
  
  ***