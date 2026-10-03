### **Stat 3113: Lesson 34 - Simple Linear Regression**
  
  This lesson builds upon the concept of correlation to formally model the relationship between a response variable `Y` and an explanatory variable `X`.
  
  ---
### **1. The Simple Linear Regression Model**
  
  We aim to model the linear relationship between `X` and `Y`. The true relationship in the population is described by the **Simple Linear Regression Model**:
  
  > \( Y_i = \beta_0 + \beta_1 X_i + \epsilon_i \)
  
  *   **\(Y_i\):** The response variable for the i-th observation. (Random variable).
  *   **\(X_i\):** The explanatory variable for the i-th observation. (Considered fixed/constant).
  *   **\(\beta_0\):** The **y-intercept** (population parameter). The expected value of `Y` when `X=0`.
  *   **\(\beta_1\):** The **slope** (population parameter). The expected change in `Y` for a one-unit increase in `X`.
  *   **\(\epsilon_i\):** The **random error term**. It accounts for the fact that the points do not fall perfectly on the line.
  
  ---
### **2. The Fitted Regression Line (Least-Squares)**
  
  Since we don't know the true parameters \(\beta_0\) and \(\beta_1\), we estimate them using sample data to create a **fitted regression line**:
  
  > \( \hat{y} = b_0 + b_1 x \)
  
  *   **\(\hat{y}\):** The **predicted value** of the response variable.
  *   **\(b_0\):** The estimated y-intercept.
  *   **\(b_1\):** The estimated slope.
  *   **Residual (\(e\)):** The difference between the observed value and the predicted value for a specific data point.
    > \( e = y - \hat{y} \)
  
  **Least-Squares Regression:**
  The method used to find the "best" line is **Least-Squares Regression**. This method chooses the values of \(b_0\) and \(b_1\) that **minimize the sum of the squared residuals (SSE)**:
  > Minimize \( \sum (y_i - \hat{y}_i)^2 \)
  
  ---
### **3. Hypothesis Testing for the Slope**
  
  The most important question in simple linear regression is: **"Is there a significant linear relationship between X and Y?"**
  
  If there is *no* linear relationship, the slope of the line would be zero (a horizontal line). Therefore, we test the slope parameter \(\beta_1\).
  
  *   **Null Hypothesis (H₀):** \(\beta_1 = 0\) (There is no linear relationship).
  *   **Alternative Hypothesis (Hₐ):** \(\beta_1 \ne 0\) (There is a linear relationship).
  
  **Finding the P-value:**
  The p-value for this test can be found in software output (like JMP) in two places, which will give the same result for simple linear regression:
  1.  **ANOVA Table:** Look at the "Prob > F" value.
  2.  **Parameter Estimates Table:** Look at the "Prob > |t|" value for the *slope term* (e.g., "Sales").
  
  **Decision Rule:**
  *   If **p-value < α**: Reject H₀. Conclude that there is a significant linear relationship between X and Y (the slope is not zero).
  *   If **p-value > α**: Fail to reject H₀. There is insufficient evidence of a linear relationship.
  
  ---
### **4. Coefficient of Determination (\(R^2\))**
  
  The hypothesis test tells us *if* there is a relationship, but not *how strong* it is. The **Coefficient of Determination**, denoted \(R^2\) (or `r²`), measures the "goodness of fit."
  
  *   **Definition:** \(R^2\) is the proportion of the variation in the response variable (`y`) that is explained by the linear relationship with the explanatory variable (`x`).
  *   **Calculation:** For simple linear regression, \(R^2 = (correlation)^2 = r^2\).
  *   **Interpretation:** An \(R^2\) of 0.886 means that **88.6% of the variation** in the response variable can be explained by the linear model.
  
  ---
### **5. Assumptions of Linear Regression**
  
  For the hypothesis tests and p-values to be valid, specific assumptions about the error term (\(\epsilon\)) must be met. We check these using **Residual Plots**.
  
  1.  **Linearity:** The relationship between X and Y is linear. (Check scatterplot for curves).
  2.  **Independence:** The errors are independent of each other. (Check residual plot for random scatter).
  3.  **Normality of Errors:** The errors follow a Normal distribution. (Check a **Normal Probability Plot of the residuals**).
  4.  **Constant Variance (Homoscedasticity):** The variability of the errors is constant across all values of X. (Check the **Residuals vs. Predicted** plot for a "funnel" shape).
  
  **Diagnosing Problems:**
  *   **Curve in Residual Plot:** Indicates the linear model is not appropriate; a non-linear model might be needed.
  *   **Funnel Shape in Residual Plot:** Indicates non-constant variance (heteroscedasticity). A transformation (like taking the log of Y) might fix this.
  *   **Outliers/Influential Points:** Points far from the line (large residuals) or points with extreme X values that tilt the line significantly. These should be investigated.