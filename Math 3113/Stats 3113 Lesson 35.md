### **Stat 3113: Lesson 35 - Simple Linear Regression Examples**
  
  This lesson focuses on applying the regression concepts learned in the previous lesson to real datasets. We will look at how to interpret software output, check assumptions using residual plots, and draw meaningful conclusions.
  
  ---
### **Example 1: Hot Dog and Beer Prices**
  
  *   **Scenario:** Can the price of a hot dog at a Major League Baseball park predict the price of a beer at the same park?
  *   **Data:** 2022 data on hot dog (`x`) and beer (`y`) prices.
  
  **Analysis Steps:**
  1.  **Least-Squares Line:** From the software output ("Parameter Estimates"), we find the equation:
    > \( \hat{y} = 4.56 + 0.45x \)
    > *(Beer Price = 4.56 + 0.45 * Hot Dog Price)*
  
  2.  **Hypothesis Test for Slope:**
    *   **H₀:** \(\beta_1 = 0\) (Slope is zero, no relationship).
    *   **Hₐ:** \(\beta_1 \ne 0\) (Slope is not zero, significant relationship).
    *   **P-value:** From the "Analysis of Variance" table (Prob > F) or "Parameter Estimates" (Prob > |t|), the p-value is **0.1071**.
    *   **Conclusion:** Since 0.1071 > 0.05, we **fail to reject H₀**. There is **not sufficient evidence** to conclude that there is a linear relationship between hot dog and beer prices.
  
  3.  **Coefficient of Determination (\(R^2\)):**
    *   `RSquare` = 0.087.
    *   **Interpretation:** Only **8.7%** of the variation in beer prices is explained by the hot dog prices. This confirms a very weak relationship.
  
  4.  **Assumption Checking:**
    *   **Residuals vs X Plot:** The plot shows a "funnel" shape (fanning out). This suggests the assumption of **constant variance is violated**.
    *   **Normal Probability Plot:** The points generally follow the line, suggesting the residuals are **approximately normal**.
  
  5.  **Interpretation of Slope:** For every $1 increase in the price of a hot dog, we expect the price of a beer to increase by roughly $0.45. (However, recall that this relationship was not statistically significant).
  
  ---
### **Example 2: Rotten Tomatoes Scores**
  
  *   **Scenario:** Can the "Critic Score" (`x`) predict the "Audience Score" (`y`) for Hollywood movies?
  
  **Analysis Steps:**
  1.  **Least-Squares Line:**
    > \( \hat{y} = 34.96 + 0.51x \)
  
  2.  **Hypothesis Test:**
    *   **P-value:** < .0001.
    *   **Conclusion:** **Reject H₀.** There is very strong evidence of a significant linear relationship.
  
  3.  **Coefficient of Determination (\(R^2\)):**
    *   `RSquare` = 0.663.
    *   **Interpretation:** Approximately **66.3%** of the variation in Audience Scores is explained by the linear relationship with Critic Scores. This is a moderately strong relationship.
  
  4.  **Assumption Checking:**
    *   **Residual Plot:** Random scatter with consistent spread. **Assumptions met.**
    *   **Normal Plot:** Points follow the line well. **Normality assumption met.**
  
  5.  **Interpretation of Slope:** For every 1 point increase in the Critic Score, the Audience Score increases by approximately 0.51 points on average.
  
  ---
### **Example 3: US Farm Population**
  
  *   **Scenario:** Examining the trend of the US Farm Population (`y`) over time (`x` = Year) from 1935 to 1980.
  
  **Analysis Steps:**
  1.  **Least-Squares Line:**
    > \( \hat{y} = 1166.93 - 0.59x \)
    > *(Note: The slope is negative, indicating the population is decreasing).*
  
  2.  **Hypothesis Test:**
    *   **P-value:** < .0001.
    *   **Conclusion:** **Reject H₀.** There is strong evidence that the farm population is changing over time (specifically, decreasing).
  
  3.  **Coefficient of Determination (\(R^2\)):**
    *   `RSquare` = 0.977.
    *   **Interpretation:** **97.7%** of the variation in farm population is explained by the year. This is an extremely strong linear relationship.
  
  4.  **Assumption Checking:**
    *   **Residual Plot:** The plot shows a distinct **curve** (U-shape). This indicates that a **linear model is not the best fit**. The relationship is non-linear (likely exponential decay or quadratic).
    *   **Normal Plot:** Shows deviation from the line, further suggesting the model is inappropriate.
  
  5.  **Predictions and Extrapolation:**
    *   **Prediction for 1955:** Within the data range (interpolation). The model predicts 19.75 million, which is reasonable.
    *   **Prediction for 2020:** Far outside the data range (extrapolation). The model predicts **-18.4 million**, which is impossible.
    *   **Lesson:** This perfectly illustrates the danger of **extrapolation**. A model that fits well in one era (1935-1980) may completely fail if extended too far into the future.