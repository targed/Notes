### **1. Confidence Intervals (Recap)**
  
  **What it is:**
  A method used to estimate an unknown population parameter (like the true mean $\mu$) by providing a range of plausible values rather than just a single number.
  
  **Interpretation (Crucial Concept):**
  *   **Correct:** "We are 95% confident that the true mean is in this interval."
  *   **What it means:** It refers to the **reliability of the method**. If you took 100 different samples and built 100 intervals, about 95 of them would capture the true parameter.
  *   **Incorrect:** "There is a 95% probability the mean is in *this specific* interval." (Once the interval is calculated, the mean is either inside or it isn't; probability is 0 or 1).
  
  **Precision Trade-offs (How to make the interval narrower/better):**
  *   **Increase Sample Size ($n$):** More data reduces the standard error ($SE = s/\sqrt{n}$). A smaller error makes the interval narrower (more precise).
  *   **Decrease Confidence:** If you only need to be 90% confident instead of 99%, you can use a smaller critical value (z or t), which makes the interval narrower.
  *   **Decrease Variability ($s$):** If the data is less consistent (high $s$), the interval must be wider.
  
  **Connection to Hypothesis Testing:**
  This is a shortcut. If you test $H_0: \mu = 50$ and your 95% CI is $(52, 60)$, you **Reject $H_0$** because 50 is not inside the interval.
  ---
### **2. One-Way ANOVA (Analysis of Variance)**
  
  **What it is:**
  A statistical test used to compare the means of **3 or more** groups (e.g., Drug A vs. Drug B vs. Placebo).
  
  **Why not just use t-tests?**
  If you compare Group A vs B, B vs C, and A vs C separately, you increase the chance of a Type I error (false positive). ANOVA does it all at once.
  
  **The F-Statistic (The Logic):**
  It compares two types of "noise" or variance:
  1.  **Signal (Between Groups):** Are the group averages far apart? (We want this to be big).
  2.  **Noise (Within Groups):** Is the data inside the groups scattered? (We want this to be small).
  *   $F = \frac{\text{Signal}}{\text{Noise}}$. A large $F$ means the difference *between* groups is much larger than the random scatter *inside* groups, suggesting a real effect.
  
  **The Assumptions (Residuals):**
  You check these *after* fitting the model using residuals (observed - predicted).
  *   **Normality:** The Normal Quantile Plot of residuals should be a straight line.
  *   **Equal Variance:** The "Residual vs. Predicted" plot should look like a uniform band of dots. NO "megaphones" or "funnels" (where the spread gets wider as you go right).
  
  **Post-Hoc Tests (Tukey's HSD):**
  *   **When to use:** ONLY after you get a significant F-test ($p < 0.05$).
  *   **How to read Connecting Letters:**
    *   If two groups share a letter (e.g., A and A), they are **not** significantly different.
    *   If they do not share a letter (e.g., A and B), they **are** significantly different.
  
  ---
### **3. Two-Way ANOVA & Factorial Experiments**
  
  **What it is:**
  Testing the effects of **two different factors** (e.g., Temperature AND Pressure) on a response variable at the same time.
  
  **The Interaction Effect (The most important concept):**
  An interaction exists if the effect of one factor depends on the level of the other.
  *   *Example:* High Temperature increases yield *only if* Pressure is low. If Pressure is high, Temperature decreases yield.
  *   **Visual Check:** Interaction Plot.
    *   **Parallel lines:** No interaction.
    *   **Crossing or non-parallel lines:** Interaction exists.
  
  **The Decision Hierarchy (The "Stop Sign" Rule):**
  1.  **Test Interaction First:** Look at the p-value for the interaction term ($A*B$).
    *   **If Significant ($p < 0.05$):** STOP. Do not talk about Main Effects (A or B separately). The story is complex; the factors depend on each other. You report the interaction.
    *   **If NOT Significant:** Remove the interaction from your mind. Now you can look at the p-values for Factor A and Factor B individually to see if they matter.
  
  **Factorial Designs ($2^3$):**
  *   3 Factors, 2 Levels each (8 runs).
  *   The hierarchy is the same, just deeper.
    1.  Check 3-way interaction (ABC). If significant, Stop.
    2.  Check 2-way interactions (AB, AC, BC). If significant, don't test the main effects involved.
    3.  Check Main effects (A, B, C).
  
  ---
  ### **4. Simple Linear Regression**
    
    **What it is:**
    Finding the equation of the straight line ($\hat{y} = b_0 + b_1x$) that best fits the data points.
    
    **Least Squares Method:**
    The computer finds the line that minimizes the sum of the squared vertical distances (residuals) between the dots and the line.
    
    **Interpreting Coefficients:**
    *   **Slope ($b_1$):** "For every 1-unit increase in $X$, $Y$ is predicted to change by $b_1$."
    *   **Intercept ($b_0$):** "When $X$ is 0, the predicted value of $Y$ is $b_0$." (Be careful: if $X=0$ doesn't make sense in context, this number is just a placeholder).
    
    **Hypothesis Test for Slope:**
    We usually test $H_0: \beta_1 = 0$.
    *   If the slope is 0, the line is flat horizontal. This means changing $X$ doesn't change $Y$ at all (no relationship).
    *   If $p < 0.05$, we reject $H_0$. We conclude there **is** a linear relationship.
    
    **$R^2$ (Coefficient of Determination):**
    *   This tells you "how good" the line is.
    *   *Example:* $R^2 = 0.80$. "80% of the variation in $Y$ is explained by the linear relationship with $X$." The other 20% is random noise.
    
    ---
### **5. Correlation & Diagnostics**
  
  **Correlation ($r$):**
  *   Measures **Linearity** only.
  *   A parabola (U-shape) is a strong relationship, but $r$ will be near 0 because it's not a line.
  *   **Outliers:** One bad point can ruin $r$ (make a strong correlation look weak, or vice versa).
  
  **Diagnostics (Checking Assumptions):**
  You look at plots of the **Residuals** to see if the linear model was appropriate.
  *   **Linearity:** Plot Residuals vs. X. You want random scatter. If you see a **curve (U-shape)**, you used a line to model a curve. Bad.
  *   **Constant Variance:** Plot Residuals vs. Predicted. You want a uniform band. If you see a **Funnel/Megaphone**, the error is getting bigger as X gets bigger. Bad.
  *   **Normality:** Normal Quantile Plot. You want a straight line.
  
  **Pitfalls:**
  *   **Extrapolation:** Using your equation to predict for an $X$ value far outside your original data. (e.g., Using data from 1990-2000 to predict the year 2050). This is dangerous and usually wrong.
  *   **Correlation $\ne$ Causation:** Just because ice cream sales and shark attacks are correlated doesn't mean ice cream attracts sharks. (Lurking variable: It's Summer).