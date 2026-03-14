## 1. Entropy: Measuring Uncertainty
**Definition:** Entropy ($H(X)$) measures the average amount of "surprise" or "uncertainty" in a random variable.
*   **High Entropy:** High uncertainty. The distribution is flat/uniform. (e.g., A fair coin toss).
*   **Low Entropy:** Low uncertainty. The distribution is peaked. (e.g., A rigged coin that lands on Heads 99% of the time).

**The Formula:**
$$H(X) = - \sum_{x \in X} p(x) \log_2 p(x)$$
*(Note: We use $\log_2$ to measure information in **bits**).*
- ### Example Calculation
  Scenario: Sunny (0.5), Cloudy (0.3), Rainy (0.2).
  $$H(X) = - [ (0.5 \log_2 0.5) + (0.3 \log_2 0.3) + (0.2 \log_2 0.2) ]$$
  $$H(X) = - [ (-0.5) + (-0.52) + (-0.46) ]$$
  $$H(X) \approx \mathbf{1.485} \text{ bits}$$
  
  ---
- ## 2. Joint & Conditional Entropy
  When dealing with two variables ($X$ and $Y$), we can measure their combined or relative uncertainty.
  
  *   **Joint Entropy ($H(X, Y)$):** The total uncertainty of both variables occurring together.
  *   **Conditional Entropy ($H(X|Y)$):** The uncertainty remaining in $X$ *after* we know $Y$.
    *   *Formula:* $H(X|Y) = - \sum \sum p(x,y) \log_2 p(x|y)$
    *   *Identity (Slide 29):* $$H(X|Y) = H(X, Y) - H(Y)$$
    *   *Intuition:* The remaining mystery of X is the "Total Mystery" minus the "Mystery of Y."
  
  ---
- ## 3. Mutual Information (MI)
  **Definition:** Measures how much information $Y$ tells us about $X$. It quantifies the **dependence** between variables.
  *   **Formula:** $$I(X; Y) = H(X) - H(X|Y)$$
    *(Total Uncertainty of X minus The Uncertainty remaining after knowing Y).*
  *   **Symmetry:** $I(X; Y) = I(Y; X)$.
  *   **Independence:** If $X$ and $Y$ are independent, knowing $Y$ tells you nothing about $X$. Thus, $H(X|Y) = H(X)$, and **$I(X; Y) = 0$**.
  
  **Use Case:** Feature Selection. If $I(\text{Feature}, \text{Target})$ is high, that feature is a strong predictor.
  
  ---
- ## 4. Relative Entropy (KL Divergence)
  **Definition:** Measures the "distance" or difference between two probability distributions, $P$ (Actual) and $Q$ (Predicted).
  *   **Formula:** $$D_{KL}(P || Q) = \sum P(x) \log_2 \frac{P(x)}{Q(x)}$$
  *   **Key Property:** It is **Asymmetric**. The distance from $P \to Q$ is *not* the same as $Q \to P$.
    *   $D_{KL}(P || Q) \neq D_{KL}(Q || P)$
  *   **Non-negative:** It is always $\ge 0$. It is 0 only if the distributions are identical.
  
  **Why it matters:** In Deep Learning and Classification, we often minimize the KL Divergence (or Cross-Entropy) between the predicted probability distribution and the true label distribution.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Entropy Intuition:**
  Which distribution has higher entropy?
  *   A: $P(Heads)=0.5, P(Tails)=0.5$
  *   B: $P(Heads)=0.9, P(Tails)=0.1$
  *   *Answer:* **A**. A is less predictable (maximum uncertainty). B is very predictable (you'd bet on Heads).
  
  **2. Mutual Information Logic:**
  Let $X$ be the roll of a die. Let $Y$ be the color of the sky.
  *   What is $I(X; Y)$?
  *   *Answer:* **Zero**. They are independent. Knowing the sky is blue doesn't reduce the uncertainty of the die roll.
  
  **3. KL Divergence Trap:**
  True distribution $P = [0.5, 0.5]$. Model prediction $Q = [0.9, 0.1]$.
  *   If you calculate $D_{KL}(P || Q)$, are you measuring how well $Q$ approximates $P$, or how well $P$ approximates $Q$?
  *   *Answer:* How much information is lost when $Q$ is used to approximate $P$. (Usually, $P$ is the ground truth).
  
  ***