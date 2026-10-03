### **Practice Set: One-Way ANOVA**
  id:: 69386152-81d7-4d1d-bd3d-7b2ea20d7cc6
  
  **1. (Hypotheses)**
  A researcher wants to compare the average battery life of four different brands of smartphones (Brand A, B, C, D). Which of the following is the correct set of hypotheses for the One-Way ANOVA?
  a) $H_0: \mu_A = \mu_B = \mu_C = \mu_D$ vs. $H_a: \mu_A \ne \mu_B \ne \mu_C \ne \mu_D$
  b) $H_0: \mu_A \ne \mu_B \ne \mu_C \ne \mu_D$ vs. $H_a: \mu_A = \mu_B = \mu_C = \mu_D$
  c) $H_0: \mu_A = \mu_B = \mu_C = \mu_D$ vs. $H_a$: At least one mean is different.
  d) $H_0: \mu_A = \mu_B = \mu_C = \mu_D$ vs. $H_a$: All means are different.
  
  **2. (ANOVA Table Calculation)**
  An ANOVA table for an experiment with 3 groups and a total of 15 observations is partially filled out below. What is the value of the **F-ratio**?
  *   $SS_{Model} (Between) = 50$
  *   $SS_{Error} (Within) = 60$
  
  a) 0.83
  b) 5.0
  c) 2.5
  d) 1.2
  
  **3. (Degrees of Freedom)**
  A One-Way ANOVA is performed on a dataset with $k=5$ treatment groups and a total of $N=50$ participants. What are the degrees of freedom for the **Error** term ($df_{Error}$)?
  a) 4
  b) 45
  c) 49
  d) 5
  
  **4. (Assumptions - Residuals)**
  You run a One-Way ANOVA and examine the "Residuals vs. Predicted" plot. You notice that the points form a "funnel" or "megaphone" shape, where the spread of the residuals gets wider as the predicted value increases. Which assumption is violated?
  a) Independence
  b) Normality
  c) Constant Variance (Homoscedasticity)
  d) Linearity
  
  **5. (Post-Hoc Logic)**
  Under which of the following conditions should you proceed to conduct a post-hoc test (like Tukey's HSD)?
  a) You reject $H_0$ and there are exactly 2 groups.
  b) You fail to reject $H_0$ and there are 3 or more groups.
  c) You reject $H_0$ and there are 3 or more groups.
  d) You always run post-hoc tests after ANOVA.
  
  **6. (Interpreting Tukey's HSD)**
  Refer to the "Connecting Letters Report" below from a study comparing the yield of 3 corn varieties.
  *   Variety A: A
  *   Variety B: A B
  *   Variety C: B
  
  Which conclusion is correct?
  a) Variety A is significantly different from Variety B.
  b) Variety B is significantly different from Variety C.
  c) Variety A is significantly different from Variety C.
  d) There are no significant differences.
  
  **7. (F-Distribution Properties)**
  Which of the following statements about the F-distribution used in ANOVA is **FALSE**?
  a) The F-statistic is a ratio of variances.
  b) The F-distribution is right-skewed.
  c) The F-statistic can take on negative values.
  d) The shape of the F-distribution depends on two degrees of freedom ($df_1, df_2$).
  
  **8. (Definitions)**
  In the context of ANOVA, the variation observed **within** each group (SS Error) is attributed to:
  a) The effect of the treatment/factor.
  b) Random sampling error or noise.
  c) The difference between group means.
  d) Interaction effects.
  
  **9. (Scenario Analysis)**
  A teacher wants to see if the average test score differs based on where a student sits in the classroom (Front, Middle, Back). She records the scores of 30 students.
  *   **Response Variable:** \_\_\_\_\_\_\_\_\_\_
  *   **Factor:** \_\_\_\_\_\_\_\_\_\_
  *   **Levels:** \_\_\_\_\_\_\_\_\_\_
  
  **10. (Calculation)**
  Given: $SSTr = 200$, $SSE = 800$, $k=3$, $N=23$.
  Calculate the **Mean Square Error (MSE)**.
  a) 100
  b) 40
  c) 34.78
  d) 80
- ---
### **Practice Set: Two-Way ANOVA**
  
  **1. (Concept Definition)**
  In a Two-Way ANOVA, an "Interaction Effect" implies that:
  a) Both factors have a significant effect on the response variable.
  b) The effect of one factor on the response variable depends on the specific level of the other factor.
  c) The means of the treatment groups are all equal.
  d) There is a correlation between the two independent variables.
  
  **2. (Hierarchy of Testing)**
  You perform a Two-Way ANOVA examining the effects of Temperature and Pressure on chemical yield. The p-value for the **Interaction (Temperature $\times$ Pressure)** is $0.003$. Using $\alpha = 0.05$, what is the correct next step?
  a) Test the Main Effect of Temperature.
  b) Test the Main Effect of Pressure.
  c) Stop and conclude that the factors interact; do not interpret the main effects in isolation.
  d) Run a post-hoc test on the main effects immediately.
  
  **3. (Visual Interpretation)**
  You are looking at an **Interaction Plot**. The x-axis represents Factor A. The y-axis represents the mean response. There are two lines drawn, representing Level 1 and Level 2 of Factor B. If the two lines are perfectly **parallel**, what does this suggest?
  a) There is a strong interaction between Factor A and Factor B.
  b) Factor A has no effect on the response.
  c) Factor B has no effect on the response.
  d) There is NO interaction between Factor A and Factor B.
  
  **4. (Degrees of Freedom Calculation)**
  A Two-Way ANOVA is conducted with **Factor A (4 levels)** and **Factor B (3 levels)**. What are the degrees of freedom for the **Interaction** term ($df_{A \times B}$)?
  a) 7
  b) 12
  c) 6
  d) 5
  
  **5. (True/False)**
  In a Two-Way ANOVA, it is possible to have significant Main Effects for both Factor A and Factor B, but have **NO** significant Interaction effect.
  a) True
  b) False
  
  **6. (F-Statistic Logic)**
  In a standard Two-Way ANOVA, which value is used as the **denominator** for calculating the F-ratios for Factor A, Factor B, and the Interaction term?
  a) $MS_{Total}$
  b) $MS_{Interaction}$
  c) $MS_{Error}$ (or $MSE$)
  d) $MS_{Factor A}$
  
  **7. (Scenario Analysis)**
  A study investigates the effect of **Soil Type** (Clay, Sand, Loam) and **Watering Frequency** (Daily, Weekly) on **Plant Growth** (in cm). Five plants are tested for every combination.
  *   **Factor A:** \_\_\_\_\_\_\_\_\_\_
  *   **Factor B:** \_\_\_\_\_\_\_\_\_\_
  *   **Number of Treatments (Cells):** \_\_\_\_\_\_\_\_\_\_
  
  **8. (Balanced Design)**
  What characterizes a "Balanced Design" in a Two-Way ANOVA?
  a) The number of levels in Factor A equals the number of levels in Factor B.
  b) The sample size ($n$) is the same for every single treatment combination (cell).
  c) The variances of all groups are equal.
  d) The main effects are equal to the interaction effects.
  
  **9. (Interpretation)**
  Refer to the "Effect Tests" output below ($\alpha = 0.05$).
  *   `Source: Factor A` | `Prob > F: 0.02`
  *   `Source: Factor B` | `Prob > F: 0.04`
  *   `Source: A*B` | `Prob > F: 0.65`
  
  Which conclusion is appropriate?
  a) Stop. The interaction is significant.
  b) There is no interaction. Factor A and Factor B both have significant independent effects on the response.
  c) There is no interaction. Only Factor A has a significant effect.
  d) The model is invalid.
  
  **10. (Calculation)**
  Given the following for a Two-Way ANOVA:
  *   $SS_{Model} = 150$
  *   $SS_{A} = 50$
  *   $SS_{B} = 40$
  Calculate the Sum of Squares for the Interaction, $SS_{A \times B}$.
  a) 90
  b) 60
  c) 240
  d) Cannot be determined.
  
  ---
### **Practice Set: $2^3$ Factorial Experiments**
  
  **1. (Basic Structure)**
  In a $2^3$ factorial experiment, what do the numbers "2" and "3" represent?
  a) 2 factors with 3 levels each.
  b) 2 replications of 3 factors.
  c) 3 factors with 2 levels each.
  d) 3 replications of 2 factors.
  
  **2. (Treatment Combinations)**
  How many **unique** treatment combinations (runs) are required to perform a single replicate of a full $2^3$ factorial experiment?
  a) 6
  b) 8
  c) 9
  d) 12
  
  **3. (Standard Notation)**
  In the standard notation for factorial designs (e.g., *a, b, ab, c...*), what does the symbol **"ac"** represent?
  a) Factor A is High, Factor B is Low, Factor C is High.
  b) Factor A is Low, Factor B is High, Factor C is Low.
  c) Factor A is High, Factor B is High, Factor C is High.
  d) All factors are at their Low levels.
  
  **4. (Testing Hierarchy - Step 1)**
  You are analyzing the results of a $2^3$ experiment with factors A, B, and C. You look at the "Effect Tests" table. Which p-value must you examine **first**?
  a) The Main Effect of A.
  b) The Interaction $A \times B$.
  c) The Interaction $A \times B \times C$.
  d) It does not matter; you can check them in any order.
  
  **5. (Testing Hierarchy - Decision)**
  Using $\alpha = 0.05$, you find that the p-value for the **three-way interaction ($A \times B \times C$)** is **0.01**. What is the correct next step?
  a) Test the two-way interactions (AB, AC, BC).
  b) Test the main effects (A, B, C).
  c) Stop testing effects. Conclude that all three factors interact significantly.
  d) Remove the 3-way interaction from the model and re-run it.
  
  **6. (Testing Hierarchy - Step 2)**
  Assume the three-way interaction was **not** significant. You proceed to test the two-way interactions and find that only the **$A \times B$ interaction is significant**. Which Main Effects are you now **allowed** to test and interpret?
  a) Only Factor C.
  b) Factors A and B.
  c) Factors A, B, and C.
  d) None of them.
  
  **7. (Degrees of Freedom)**
  In a $2^3$ factorial design, what are the degrees of freedom ($df$) for the **Main Effect of Factor B**?
  a) 1
  b) 2
  c) 3
  d) Depends on the sample size.
  
  **8. (Sample Size Calculation)**
  You plan to run a $2^3$ factorial experiment with **4 replicates** (observations) for every unique treatment combination. What is the total number of observations ($N$) in your dataset?
  a) 12
  b) 24
  c) 32
  d) 8
  
  **9. (Interpretation)**
  Why do we "Stop" and not test main effects if a higher-order interaction is significant?
  a) Because the math becomes impossible to calculate.
  b) Because main effects are always zero when interactions exist.
  c) Because a "Main Effect" describes an average impact, which is misleading if the factor's impact changes depending on the levels of other factors.
  d) Because the p-values for the main effects will statistically inflate to 1.0.
  
  **10. (Scenario Analysis)**
  Analyze the following JMP Effect Test output ($\alpha = 0.05$):
  *   `Source: A*B*C` | `Prob > F: 0.45`
  *   `Source: A*B` | `Prob > F: 0.03`
  *   `Source: A*C` | `Prob > F: 0.60`
  *   `Source: B*C` | `Prob > F: 0.88`
  
  Which conclusions can be safely drawn?
  a) There is a significant 3-way interaction.
  b) There is a significant interaction between A and B. We should NOT test Main Effects A or B. We CAN test Main Effect C.
  c) There is a significant interaction between A and B. We should test Main Effects A and B anyway.
  d) Nothing is significant.
  
  ---
### **Practice Set: Bivariate Analysis (Scatterplots & Correlation)**
  
  **1. (Variable Roles)**
  A real estate agent wants to study the relationship between the **size of a house** (in square feet) and its **selling price**. She suspects that larger houses sell for higher prices.
  *   **Explanatory Variable ($X$):** \_\_\_\_\_\_\_\_\_\_
  *   **Response Variable ($Y$):** \_\_\_\_\_\_\_\_\_\_
  
  **2. (Scatterplot Analysis)**
  When describing a scatterplot, which of the following is **NOT** one of the four key aspects you should typically look for?
  a) Outliers
  b) Clusters
  c) P-value
  d) Functional Form (Linearity)
  
  **3. (Correlation Definition)**
  Pearson’s correlation coefficient ($r$) measures:
  a) The strength and direction of ANY relationship between two quantitative variables.
  b) The strength and direction of the LINEAR relationship between two quantitative variables.
  c) The likelihood that changes in $X$ cause changes in $Y$.
  d) The percentage of variation in $Y$ explained by $X$.
  
  **4. (Properties of $r$)**
  Which of the following values represents the **strongest** linear relationship?
  a) $r = 0.85$
  b) $r = 0.01$
  c) $r = -0.92$
  d) $r = 1.05$
  
  **5. (Correlation & Curvature)**
  You generate a scatterplot for two variables, $X$ and $Y$. The points form a perfect "U" shape (a parabola). If you calculate the correlation coefficient $r$, what value would you expect to get?
  a) $r \approx 1$
  b) $r \approx -1$
  c) $r \approx 0$
  d) $r \approx 0.5$
  
  **6. (True/False)**
  Correlation is a resistant measure; it is not heavily influenced by extreme outliers.
  a) True
  b) False
  
  **7. (Interpreting Direction)**
  A study finds a correlation of **$r = -0.75$** between the "Number of absences in a semester" and "Final Exam Score." This means that:
  a) Students with more absences tend to have higher exam scores.
  b) Students with more absences tend to have lower exam scores.
  c) There is a weak relationship between absences and scores.
  d) Absences cause students to fail.
  
  **8. (Causation)**
  A strong positive correlation ($r=0.98$) is found between "Ice Cream Sales" and "Number of Shark Attacks." Which is the most statistically sound conclusion?
  a) Buying ice cream causes sharks to attack.
  b) Shark attacks cause people to buy comfort food (ice cream).
  c) There is likely a lurking variable (like Temperature/Summer) influencing both.
  d) The calculation must be wrong; these variables cannot be correlated.
  
  **9. (Visual Matching)**
  Match the Correlation Coefficient to the description of the scatterplot:
  *   **A:** $r = 0.95$
  *   **B:** $r = -0.60$
  *   **C:** $r = 0.05$
  
  1.  \_\_\_\_ A loose cloud of points with a downward trend.
  2.  \_\_\_\_ A tight grouping of points sloping upward.
  3.  \_\_\_\_ A random scatter of points with no discernible direction.
  
  **10. (Range of $r$)**
  Which of the following is a **valid** value for Pearson's correlation coefficient?
  a) 1.3
  b) -2.0
  c) -0.5
  d) 100
  
  ---
### **Practice Set: Simple Linear Regression**
  
  **1. (Interpreting Slope)**
  A regression model is fitted to predict the **Selling Price** ($Y$, in \$1000s) of a car based on its **Age** ($X$, in years). The fitted equation is:
  $$ \hat{y} = 25.5 - 2.3x $$
  Which is the correct interpretation of the slope?
  a) The average price of a new car (0 years old) is \$25,500.
  b) For every 1 year increase in age, the predicted price decreases by \$2,300.
  c) For every 1 year increase in age, the predicted price decreases by \$25,500.
  d) For every \$1,000 decrease in price, the car is 2.3 years older.
  
  **2. (Calculating Residuals)**
  Using the regression equation $\hat{y} = 10 + 4x$, what is the **residual** for an observed data point where $x = 3$ and $y = 20$?
  a) -2
  b) 2
  c) 22
  d) 12
  
  **3. (Least Squares Definition)**
  The "Least Squares" regression line is specifically the line that minimizes:
  a) The sum of the residuals ($\sum e_i$).
  b) The sum of the squared residuals ($\sum e_i^2$).
  c) The sum of the squared X values.
  d) The correlation coefficient ($r$).
  
  **4. (Hypothesis Testing)**
  In a simple linear regression analysis, testing the null hypothesis $H_0: \beta_1 = 0$ is equivalent to testing:
  a) Is the y-intercept significantly different from zero?
  b) Is the correlation coefficient ($r$) significantly different from zero?
  c) Does the line pass through the origin?
  d) Is the mean of $Y$ equal to the mean of $X$?
  
  **5. (Coefficient of Determination)**
  For a specific regression model, the calculation $SSR / SST$ yields a value of **0.72**. This value is known as:
  a) The correlation coefficient ($r$).
  b) The slope ($\beta_1$).
  c) The coefficient of determination ($R^2$).
  d) The standard error ($s$).
  
  **6. (Assumptions - Diagnostics)**
  You examine a "Residuals vs. Predicted" plot and observe a distinct **curved (U-shape)** pattern. Which assumption of linear regression has been violated?
  a) Constant Variance (Homoscedasticity)
  b) Normality of Residuals
  c) Linearity
  d) Independence
  
  **7. (Extrapolation)**
  A study on child growth collects data for children aged 2 to 12. A regression line is fit. Using this line to predict the height of a 25-year-old is an example of:
  a) Interpolation (Safe).
  b) Extrapolation (Dangerous/Unreliable).
  c) Least Squares estimation.
  d) Residual analysis.
  
  **8. (Prediction)**
  Given the regression line $\hat{y} = 50 + 1.5x$, predict the value of $y$ when $x = 10$.
  a) 65
  b) 51.5
  c) 60
  d) 500
  
  **9. (Interpretation of $R^2$)**
  If $R^2 = 0.85$ for a model predicting Exam Score ($Y$) from Hours Studied ($X$), what does this mean?
  a) 85% of students passed the exam.
  b) There is an 85% probability that the next student will pass.
  c) 85% of the variation in Exam Scores is explained by the linear relationship with Hours Studied.
  d) The correlation is 0.85.
  
  **10. (Assumption Checking - Normality)**
  Which plot is best used to check the assumption that the error terms (residuals) follow a Normal distribution?
  a) Scatterplot of Y vs X.
  b) Residuals vs. Predicted Plot.
  c) Normal Quantile (Probability) Plot of Residuals.
  d) Boxplot of X.
  
  ---
### **Solutions & Explanations**
  
  **1. c) $H_0: \mu_A = \mu_B = \mu_C = \mu_D$ vs. $H_a$: At least one mean is different.**
  *   **Explanation:** The null always assumes equality. The alternative is *not* that they are all different, but that *at least one* is different from the others.
  
  **2. b) 5.0**
  *   **Explanation:**
    *   $df_{Model} = k - 1 = 3 - 1 = 2$.
    *   $df_{Error} = N - k = 15 - 3 = 12$.
    *   $MS_{Model} = 50 / 2 = 25$.
    *   $MS_{Error} = 60 / 12 = 5$.
    *   $F = MS_{Model} / MS_{Error} = 25 / 5 = 5.0$.
  
  **3. b) 45**
  *   **Explanation:** $df_{Error} = N - k = 50 - 5 = 45$.
  
  **4. c) Constant Variance (Homoscedasticity)**
  *   **Explanation:** A "funnel" shape in the residuals vs. predicted plot indicates that the variance is changing (usually increasing) as the mean increases, violating the equal variance assumption.
  
  **5. c) You reject $H_0$ and there are 3 or more groups.**
  *   **Explanation:** If you fail to reject, you concluded there are no differences, so looking for them makes no sense. If there are only 2 groups, rejecting $H_0$ already tells you Group 1 $\ne$ Group 2. You only need Tukey's to identify *which* pair is different when there are 3+ options.
  
  **6. c) Variety A is significantly different from Variety C.**
  *   **Explanation:** Rule: "Levels not connected by the same letter are significantly different."
    *   A and B share "A" (Not different).
    *   B and C share "B" (Not different).
    *   A and C share **NO** letters. They are different.
  
  **7. c) The F-statistic can take on negative values.**
  *   **Explanation:** Since $F = MS_{Tr} / MSE$, and Mean Squares are calculated from Sums of Squared numbers (which are always positive), F must always be $\ge 0$.
  
  **8. b) Random sampling error or noise.**
  *   **Explanation:** Variation *within* a group cannot be caused by the treatment (since everyone in that group got the same treatment). It represents natural variation among individuals (error).
  
  **9. Answer:**
  *   **Response Variable:** Test Score (Numerical).
  *   **Factor:** Seat Location (Categorical).
  *   **Levels:** Front, Middle, Back (3 Levels).
  
  **10. b) 40**
  *   **Explanation:**
    *   We need $df_{Error} = N - k = 23 - 3 = 20$.
    *   $MSE = SSE / df_{Error} = 800 / 20 = 40$.
### **Solutions & Explanations**
  
  **1. b) The effect of one factor on the response variable depends on the specific level of the other factor.**
  *   **Explanation:** This is the formal definition. Example: Increasing heat helps baking (positive effect) only if the oven is closed; if open, it does nothing. The effect of heat depends on the door status.
  
  **2. c) Stop and conclude that the factors interact; do not interpret the main effects in isolation.**
  *   **Explanation:** The "Stop Rule." If factors interact, interpreting "Main Effects" (averages) is misleading because the effect changes depending on the conditions.
  
  **3. d) There is NO interaction between Factor A and Factor B.**
  *   **Explanation:** Parallel lines mean the difference between Level 1 and Level 2 of Factor B is the same regardless of what Factor A is doing. Crossing or non-parallel lines indicate interaction.
  
  **4. c) 6**
  *   **Explanation:** Formula: $df_{A \times B} = (Levels_A - 1) \times (Levels_B - 1)$.
  *   $df_{AxB} = (4-1) \times (3-1) = 3 \times 2 = 6$.
  
  **5. a) True**
  *   **Explanation:** Factors can work independently. For example, adding fertilizer helps plants (Main Effect A) and adding water helps plants (Main Effect B), and adding both simply adds the benefits together without any complex interaction (Parallel lines).
  
  **6. c) $MS_{Error}$ (or $MSE$)**
  *   **Explanation:** All F-ratios in a standard ANOVA are $\frac{\text{Effect Variance}}{MSE}$. The "Noise" ($MSE$) is always the baseline for comparison.
  
  **7. Answer:**
  *   **Factor A:** Soil Type (3 levels).
  *   **Factor B:** Watering Frequency (2 levels).
  *   **Treatments:** $3 \times 2 = 6$ combinations.
  
  **8. b) The sample size ($n$) is the same for every single treatment combination (cell).**
  *   **Explanation:** "Balanced" in ANOVA refers to the quantity of data collected being equal across groups.
  
  **9. b) There is no interaction. Factor A and Factor B both have significant independent effects.**
  *   **Explanation:**
  1.  Check Interaction ($p=0.65 > 0.05$). Not significant. PROCEED.
  2.  Check Main Effect A ($p=0.02 < 0.05$). Significant.
  3.  Check Main Effect B ($p=0.04 < 0.05$). Significant.
  
  **10. b) 60**
  *   **Explanation:** For the Model (Between Treatments):
  *   $SS_{Model} = SS_{A} + SS_{B} + SS_{A \times B}$
  *   $150 = 50 + 40 + SS_{A \times B}$
  *   $150 = 90 + SS_{A \times B}$
  *   $SS_{A \times B} = 60$.
### **Solutions & Explanations**
  
  **1. c) 3 factors with 2 levels each.**
  *   **Explanation:** The base (2) is the number of levels (High/Low). The exponent (3) is the number of factors.
  
  **2. b) 8**
  *   **Explanation:** $2^3 = 2 \times 2 \times 2 = 8$. The combinations are (1), a, b, ab, c, ac, bc, abc.
  
  **3. a) Factor A is High, Factor B is Low, Factor C is High.**
  *   **Explanation:** In this notation, the *presence* of a letter means that specific factor is High. The *absence* of a letter means that factor is Low. Since "b" is missing, B is Low.
  
  **4. c) The Interaction $A \times B \times C$.**
  *   **Explanation:** Always start at the bottom of the hierarchy (highest complexity) and work up.
  
  **5. c) Stop testing effects. Conclude that all three factors interact significantly.**
  *   **Explanation:** If the highest-order interaction is significant, the factors are intertwined in a complex way. Interpreting simpler 2-way interactions or main effects "averages out" this complexity and is misleading.
  
  **6. a) Only Factor C.**
  *   **Explanation:** Because $A \times B$ is significant, the effects of A and B depend on each other. They are "locked." However, Factor C is *not* involved in any significant interactions (assuming AC and BC were not significant), so its Main Effect can be tested independently.
  
  **7. a) 1**
  *   **Explanation:** Degrees of freedom for a factor = $(\text{Levels} - 1)$. Since a $2^3$ design has 2 levels for every factor, $df = 2 - 1 = 1$.
  
  **8. c) 32**
  *   **Explanation:**
  *   Unique Combinations = $2^3 = 8$.
  *   Replicates = 4.
  *   Total $N = 8 \times 4 = 32$.
  
  **9. c) Because a "Main Effect" describes an average impact...**
  *   **Explanation:** Example: If "Heat" increases yield when "Pressure" is Low, but decreases yield when "Pressure" is High, the *Main Effect* (average) of Heat might look like zero. Reporting "Heat has no effect" would be wrong.
  
  **10. b) There is a significant interaction between A and B. We should NOT test Main Effects A or B. We CAN test Main Effect C.**
  *   **Explanation:**
  *   3-way (0.45) is NOT significant. Proceed.
  *   2-way A*B (0.03) IS significant. STOP testing A and B.
  *   Other 2-ways are not significant.
  *   Factor C is free to be tested.
### **Solutions & Explanations**
  
  **1. Answer:**
  *   **Explanatory ($X$):** Size of House (Sq Ft).
  *   **Response ($Y$):** Selling Price.
  *   **Explanation:** The agent wants to use the size to *explain* or predict the price.
  
  **2. c) P-value**
  *   **Explanation:** P-values are calculation results from hypothesis tests. When visually inspecting a scatterplot, we look for **Outliers**, **Clusters**, **Association** (Direction/Strength), and **Form** (Linear/Curved).
  
  **3. b) The strength and direction of the LINEAR relationship between two quantitative variables.**
  *   **Explanation:** $r$ is specifically designed for *linear* relationships. It cannot accurately measure curved relationships.
  
  **4. c) $r = -0.92$**
  *   **Explanation:** Strength is determined by the **magnitude** (absolute value). $|-0.92| = 0.92$, which is closer to 1 than 0.85 is. (Note: $r=1.05$ is impossible).
  
  **5. c) $r \approx 0$**
  *   **Explanation:** Pearson's $r$ measures *linear* association. A parabola is a strong *curved* relationship, but it has no linear trend (it goes down then up, canceling out), resulting in an $r$ near 0.
  
  **6. b) False**
  *   **Explanation:** Correlation is **NOT resistant**. A single outlier can drastically change the value of $r$ (e.g., changing it from 0.9 to 0.4).
  
  **7. b) Students with more absences tend to have lower exam scores.**
  *   **Explanation:** A **negative** correlation means that as one variable increases ($X$: Absences), the other variable decreases ($Y$: Score).
  
  **8. c) There is likely a lurking variable (like Temperature/Summer) influencing both.**
  *   **Explanation:** Correlation does not imply causation. The "Ice Cream vs. Shark" scenario is the classic example of a **Lurking Variable** creating a spurious correlation.
  
  **9. Answer:**
  1.  **B** ($r = -0.60$) - Loose cloud (moderate/weak), downward (negative).
  2.  **A** ($r = 0.95$) - Tight grouping (strong), upward (positive).
  3.  **C** ($r = 0.05$) - Random scatter (no association).
  
  **10. c) -0.5**
  *   **Explanation:** The range of $r$ is strictly **$-1 \le r \le 1$**. Any value outside this range is mathematically impossible for a correlation coefficient.
### **Solutions & Explanations**
  
  **1. b) For every 1 year increase in age, the predicted price decreases by \$2,300.**
  *   **Explanation:** The slope ($-2.3$) represents the change in $Y$ per 1-unit change in $X$. Since $Y$ is in thousands, $-2.3$ corresponds to a decrease of \$2,300.
  
  **2. a) -2**
  *   **Explanation:**
  1.  Calculate predicted value: $\hat{y} = 10 + 4(3) = 10 + 12 = 22$.
  2.  Calculate residual: $e = \text{Observed} - \text{Predicted} = 20 - 22 = -2$.
  
  **3. b) The sum of the squared residuals ($\sum e_i^2$).**
  *   **Explanation:** This is the definition of the "Least Squares" method (SSE). Minimizing sum of residuals alone isn't enough because positives and negatives cancel out to zero for many lines.
  
  **4. b) Is the correlation coefficient ($r$) significantly different from zero?**
  *   **Explanation:** Testing if the slope is zero is statistically identical to testing if the linear correlation is zero. If the slope is 0, the line is flat, meaning no linear relationship exists ($r=0$).
  
  **5. c) The coefficient of determination ($R^2$).**
  *   **Explanation:** $R^2$ represents the proportion of Total Sum of Squares ($SST$) that is explained by the Regression model ($SSR$).
  
  **6. c) Linearity**
  *   **Explanation:** If the residuals show a curve, it means a straight line failed to capture the curved nature of the original data. The Linear Model is not appropriate.
  
  **7. b) Extrapolation (Dangerous/Unreliable).**
  *   **Explanation:** Extrapolation is predicting for an $x$-value outside the range of the data used to build the model. Growth patterns change drastically after age 12, so the linear trend likely stops.
  
  **8. a) 65**
  *   **Explanation:** Simply plug in $x$: $\hat{y} = 50 + 1.5(10) = 50 + 15 = 65$.
  
  **9. c) 85% of the variation in Exam Scores is explained by the linear relationship with Hours Studied.**
  *   **Explanation:** This is the standard definition of $R^2$. It quantifies the explanatory power of the model.
  
  **10. c) Normal Quantile (Probability) Plot of Residuals.**
  *   **Explanation:**
  *   Scatterplot checks linearity.
  *   Residuals vs. Predicted checks constant variance.
  *   Normal Quantile Plot checks **normality**. (Ideally, points fall on a diagonal line).