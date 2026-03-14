## 1. Bayes' Theorem
**The Concept:** Bayes' Theorem allows us to **invert** conditional probabilities.
*   **The Problem:** In the real world, we often know $P(\text{Symptoms}|\text{Disease})$ (if you have the flu, you likely have a fever).
*   **The Goal:** We want to know $P(\text{Disease}|\text{Symptoms})$ (if you have a fever, do you have the flu?).

**The Formula:**
$$P(A|B) = \frac{P(B|A) \times P(A)}{P(B)}$$

**The Terminology:**
1.  Prior ($P(A)$): Your initial belief before seeing any data. (e.g., "Flu is common in winter").
2.  Likelihood ($P(B|A)$): The probability of seeing this evidence *if* the hypothesis is true. (e.g., "The probability of fever given you have the flu").
3.  Evidence ($P(B)$): The total probability of the data occurring under all possible scenarios (The Normalizer).
4.  Posterior ($P(A|B)$): Your updated belief after seeing the data.

---
- ## 2. Solved Exercise: The Medical Paradox
  **Context:** A test for Disease A (DA) is very accurate, but the disease is rare.
  *   **Prior:** $P(DA) = 0.001$ (Only 0.1% of people have it).
    *   *Implied:* $P(\text{Healthy}) = 0.999$.
  *   **Likelihood (Sensitivity):** $P(+|DA) = 1.0$ (It catches 100% of infections).
  *   **False Positive Rate:** $P(+|\text{Healthy}) = 0.01$ (1% error rate on healthy people).
  
  **Question:** A patient tests positive ($+$). What is the probability they actually have the disease ($P(DA|+)$)?
  
  **Calculation:**
  $$P(DA|+) = \frac{P(+|DA) \times P(DA)}{P(+)}$$
  
  1.  **Numerator:** $1.0 \times 0.001 = 0.001$
  2.  **Denominator ($P(+)$):** We must use the **Law of Total Probability** (from Slide 8).
    $$P(+) = P(+|DA)P(DA) + P(+|\text{Healthy})P(\text{Healthy})$$
    $$P(+) = (1.0 \times 0.001) + (0.01 \times 0.999)$$
    $$P(+) = 0.001 + 0.00999 = 0.01099$$
  3.  **Result:**
    $$P(DA|+) = \frac{0.001}{0.01099} \approx \mathbf{0.09} \text{ or } 9\%$$
  
  **Takeaway:** Even with a highly accurate test, if the disease is very rare, a positive result is more likely to be a False Positive than a True Positive. This is known as the **Base Rate Fallacy**.
  
  ---
- ## 3. Random Variables & Distributions
  A **Random Variable (RV)** maps outcomes to numbers.
- ### Types of Distributions
  *   **Discrete RV (PMF):** Probability Mass Function. $P(X=x)$. Used for counts, categories. (Sum of probabilities = 1).
  *   **Continuous RV (PDF):** Probability Density Function. $f(x)$. Used for height, temperature.
    *   *Note:* For continuous variables, $P(X=x) = 0$. We calculate the probability of a *range* using the area under the curve (Integral).
  *   **CDF:** Cumulative Distribution Function. $F(x) = P(X \le x)$. It accumulates probability from left to right.
- ### Joint Probability
  $P(x, y)$ is the probability of $X$ and $Y$ happening at the same time.
  *   **Marginalization:** To get $P(x)$ back from $P(x,y)$, you **sum** over all possible values of $y$.
    $$P(x) = \sum_y P(x, y)$$
  
  ---
- ## 4. Solved Exercises: The Joint Table
  **The Data:**
  A grid representing $P(X, Y)$.
  | | $X=1$ | $X=2$ | $X=3$ |
  |---|---|---|---|
  | **Y=1** | 0.32 | 0.03 | 0.01 |
  | **Y=2** | 0.06 | 0.24 | 0.02 |
  | **Y=3** | 0.02 | 0.03 | 0.27 |
  
  Exercise 3.1: Marginal Probability $P(X=1)$
  *   Sum the entire column for $X=1$.
  *   $0.32 + 0.06 + 0.02 = \mathbf{0.40}$.
  
  Exercise 3.2: Conditional Probability $P(X=2 | Y=1)$
  *   Formula: $P(X=2 \cap Y=1) / P(Y=1)$.
  *   Numerator (Intersection): $0.03$ (The cell where row 1 and col 2 meet).
  *   Denominator (Marginal Y=1): Sum of Row 1 ($0.32 + 0.03 + 0.01 = 0.36$).
  *   Result: $0.03 / 0.36 = 1/12 \approx \mathbf{0.083}$.
  
  Exercise 4.1: Marginal Probability $P(Y=3)$
  *   Sum of Row 3.
  *   $0.02 + 0.03 + 0.27 = \mathbf{0.32}$.
  
  Exercise 4.2: Conditional Probability $P(Y=2 | X=2)$
  *   Intersection ($X=2, Y=2$): $0.24$.
  *   Marginal ($X=2$): $0.03 + 0.24 + 0.03 = 0.30$.
  *   Result: $0.24 / 0.30 = \mathbf{0.80}$.
  
  ---
- ## 5. Statistical Moments
  These summarize distributions into single numbers.
  *   **Mean ($E[X]$):** The weighted average. "Center of Gravity."
    *   $\sum x P(x)$
  *   **Variance ($Var(X)$):** The spread. "Average squared distance from the mean."
    *   $E[(X - \mu)^2]$
  *   **Covariance ($Cov(X,Y)$):** How two variables change together.
    *   Positive = They move in the same direction.
    *   Zero = Uncorrelated (Linearly independent).
  
  ***
- # Self-Check Questions
  1.  **Bayes Logic:** If $P(A|B)$ increases, does that mean $P(B|A)$ must also increase? (Answer: No, it depends on the Priors $P(A)$ and $P(B)$).
  2.  **Table Math:** Look at the table in Section 4. Are X and Y independent?
    *   *Hint:* Check if $P(X=1, Y=1) = P(X=1) \times P(Y=1)$.
    *   $0.32 \ne (0.40 \times 0.36)$. No, they are **Dependent**.