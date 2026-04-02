### **1. Estimation: Guessing the Truth (Slides 8–11)**
In Data Science, we rarely know the *true* parameters of the universe (Population Mean $\mu$ or Variance $\sigma^2$). We only have our small list of data points.

*   **The Estimator:** We use the **Sample Mean ($\bar{x}$)** to estimate the **Population Mean ($\mu$)**.
*   **The Outlier Problem (Slide 9):**
  *   The slide shows a sample: `{0.33, ..., 0.89, -460}`.
  *   The `-460` is clearly an error or an extreme outlier.
  *   *Slide Question:* "How to avoid this effect?"
  *   *Answer:* You either use the **Median** (which ignores the -460) or you apply an **Outlier Removal** filter (like the "3 Sigma Rule") before calculating the mean.
*   **Mean Squared Error (MSE) (Slide 10):**
  *   How do we know if our guess ($\bar{x}$) is good? We calculate the average squared difference between our data and the true mean. This is the most common loss function in Regression models.

*   **Variance Estimation (Slide 11):**
  *   We use sample variance to estimate population variance.
  *   *Crucial Note:* The slide uses the formula with $\frac{1}{n}$. In strict statistics, this is a **Biased Estimator**. Most software (like Pandas `df.var()`) defaults to $\frac{1}{n-1}$ (Bessel's Correction) to fix this bias. Be aware of which specific formula your professor wants you to use on exams.

---
- ### **2. The Z-Score: Normalization (Slide 12)**
  **This is the most important concept for Deep Learning in this section.**
  
  *   **The Formula:** $$Z = \frac{x_i - \mu}{\sigma}$$
  *   **Translation:** "How many Standard Deviations is this point away from the Mean?"
    *   $Z = 0$: The point is exactly average.
    *   $Z = +2$: The point is way above average (95th percentile).
    *   $Z = -1$: The point is somewhat below average.
  *   **Why for Deep Learning?** The slide notes: *"Very popular operation in vision data preprocessing."*
    *   *Reason:* Neural Networks are bad at math with large numbers (e.g., House Prices in millions). They are great at math with small numbers (between -1 and 1). By converting data to Z-scores (Standardization), we make the model train faster and more stably.
  
  ---
- ### **3. Measuring Relationships: Covariance vs. Correlation (Slides 13–15)**
  We often want to know: "If X goes up, does Y go up?"
- #### **Covariance (Slide 13)**
  *   **Formula:** Average of $(x - \bar{x}) \times (y - \bar{y})$.
  *   **Logic:**
    *   If X is high (positive diff) and Y is high (positive diff), the product is **Positive**.
    *   If X is high (positive) and Y is low (negative), the product is **Negative**.
  *   **The Flaw:** The result depends on the units. Covariance of "Height vs Weight" might be 150. Covariance of "Income vs Age" might be 1,000,000. You cannot compare them.
- #### **Pearson's Correlation ($\rho$) (Slides 14–15)**
  *   **The Solution:** Divide Covariance by the Standard Deviations ($\sigma_x \sigma_y$).
  *   **The Magic:** This scales the result to be strictly between **-1 and 1**.
    *   **+1:** Perfect positive line.
    *   **-1:** Perfect negative line.
    *   **0:** No linear relationship (Random cloud).
  *   **The Insight (Slide 15):** The slide shows that Correlation is actually just the **Average Product of Z-Scores**.
    $$ \rho = \frac{1}{n} \sum (Z_x \times Z_y) $$
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Vectorization:** When you implement `MSE` or `Variance` in Python (NumPy), you don't write a `for` loop. You write `np.mean((x - mu)**2)`. This happens in parallel and is much faster.
  *   **Linearity:** Pearson's Correlation *only* detects straight lines. If your data looks like a "U" shape (Parabola), Pearson's correlation might be 0 even though there is a clear relationship.
  
  ---
- ### **Action Items for Section 2:**
  *   **Code Prep:** In your next homework, you will likely need to "Normalize" data. Memorize the Pandas code for Z-score:
    ```python
    df['z_score'] = (df['value'] - df['value'].mean()) / df['value'].std()
    ```
  *   **Concept Check:** If Covariance is zero, what is the Correlation? (Answer: Zero).