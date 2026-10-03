### **Stat 3113: Lesson 33 - Correlation**
  
  This lesson dives into how we quantify the relationship between two quantitative variables. Scatterplots give us a visual idea, but **correlation** gives us a precise number.
  
  ---
### **1. What is Correlation?**
  
  *   **Definition:** Correlation (specifically **Pearson's correlation coefficient**, denoted by **`r`**) is a numerical measure that quantifies the **strength** and **direction** of the **linear** relationship between two quantitative variables.
  
  *   **Key Takeaway:** It only measures straight-line relationships. It does not work for curved relationships.
  
  ---
### **2. Properties of the Correlation Coefficient (r)**
  
  The value of `r` has specific properties that help us interpret it:
  
  1.  **Range:** The value of `r` is always between -1 and +1.
    > \( -1 \le r \le 1 \)
  
  2.  **Direction:**
    *   **Positive `r`:** Indicates a positive association (as `x` increases, `y` tends to increase).
    *   **Negative `r`:** Indicates a negative association (as `x` increases, `y` tends to decrease).
  
  3.  **Strength:**
    *   **`r = 0`:** No **linear** relationship. (Note: There could still be a strong curved relationship, as shown in the example with the parabola).
    *   **`r` close to 0:** Weak linear relationship (a diffuse cloud of points).
    *   **`r` close to ±1:** Strong linear relationship.
    *   **`r = +1` or `r = -1`:** Perfect linear relationship (all points lie exactly on a straight line).
  
  4.  **Sensitivity:** Like the mean and standard deviation, the correlation coefficient is **not resistant to outliers**. A single extreme outlier can drastically change the value of `r`, making a weak relationship look strong or a strong one look weak.
  
  ---
### **3. Interpretation Examples**
  
  *   **Scatterplot 1 (Linearity):**
    *   Comparing two scatterplots with positive trends. The one where the points are tighter to the line is **more linear** and has a **stronger positive association** (higher `r`).
  
  *   **Parabola Example (r = 0):**
    *   A scatterplot showing a perfect U-shape (parabola) has a correlation of **`r = 0`**.
    *   **Why?** Because correlation only measures *linear* relationships. Even though `x` and `y` are perfectly related, the relationship isn't a straight line, so Pearson's `r` cannot detect it.
  
  *   **Outlier Example (r = -0.81774):**
    *   A scatterplot shows a near-perfect negative linear trend for almost all points, but one outlier is far away.
    *   The calculated `r` is **-0.81**, which indicates a strong negative relationship, but the outlier has pulled it away from -1.0.
    *   **Lesson:** Always look at the scatterplot *and* the correlation value together. Don't rely on `r` alone.
  
  ---
### **4. Correlation vs. Causation**
  
  This is the golden rule of statistics:
  > **Correlation does not imply causation.**
  
  Just because two variables are strongly correlated (high `r`) does not mean that changes in one variable *cause* changes in the other. There could be a lurking variable influencing both.
  
  **Example:** Ice cream sales and shark attacks are positively correlated. Does buying ice cream cause shark attacks? No. The lurking variable is **temperature**. Hot days cause more people to buy ice cream *and* more people to swim in the ocean.