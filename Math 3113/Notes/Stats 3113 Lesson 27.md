### **Stat 3113: Lesson 27 - One-Way ANOVA (Part 2)**
  
  This lesson applies the full Analysis of Variance procedure to practical examples. The focus is on executing the complete workflow: stating hypotheses, interpreting the main ANOVA F-test, checking the underlying assumptions, and performing multiple comparisons if necessary.
  
  ---
### **Example 1: Stenosis and Artery Collapse**
  
  *   **Scenario:** A mechanical engineer investigates if the level of artery blockage (stenosis) affects the fluid flowrate at which a model artery collapses.
  *   **Factor (Independent Variable):** Amount of Stenosis.
  *   **Levels (3 Groups):** Level 1 (0.78), Level 2 (0.71), Level 3 (0.65).
  *   **Response Variable (Dependent):** Flowrate in ml/s at collapse.
#### **Step 1: State the Hypotheses**
  We want to test if the *mean* flowrate is the same across all three stenosis levels.
  *   **Null Hypothesis (H₀):** The mean flowrates for all three stenosis levels are equal.
    > H₀: \(\mu_1 = \mu_2 = \mu_3\)
  *   **Alternative Hypothesis (Hₐ):** At least one of the mean flowrates is different.
    > Hₐ: At least one \(\mu_i\) is different.
  *   **Significance Level:** We will test at `α = 0.05`.
#### **Step 2: Perform the ANOVA F-test and Make a Conclusion**
  We look at the "Analysis of Variance" output from JMP.
  *   **F-Ratio (Test Statistic):** 23.5747
  *   **P-value (Prob > F):** <.0001
  
  **Statistical Conclusion:**
  *   We compare the p-value to `α`.
  *   Since the **p-value (< 0.0001) is less than `α` (0.05)**, we **reject the null hypothesis (H₀)**.
  
  **Conclusion in Context:**
  There is statistically significant evidence to conclude that the mean flowrate at which the artery collapses is different for at least one of the stenosis levels.
#### **Step 3: Check the Assumptions**
  We use the residual plots to validate our conclusion.
  1.  **Normality:** The **Normal Probability Plot of Residuals** shows the points falling very close to the straight diagonal line. This indicates that the **normality assumption holds**.
  2.  **Equal Variance:** The **Residuals vs Factor Plot** shows the vertical spread (scatter) of the points for each of the three levels is roughly the same. This indicates that the **equal variance assumption holds**.
  3.  **Independence:** We assume the experiment was conducted in a way that ensures independence.
  
  Since the assumptions are met, we can trust the results of our F-test.
#### **Step 4: Perform Multiple Comparisons (Post-Hoc Test)**
  Because we rejected H₀, we now need to find out *which* specific means are different from each other. We use the Tukey-Kramer HSD output.
  
  *   **Connecting Letters Report:**
    *   Level 3 is in group 'A'.
    *   Level 2 is in group 'B'.
    *   Level 1 is in group 'C'.
    *   **Rule:** Levels not connected by the same letter are significantly different.
    *   **Conclusion:** Since no levels share a letter, **all three groups are statistically different from each other.**
  
  *   **Ordered Differences Report:** This table confirms the conclusion by showing the p-value for each pairwise comparison. All three p-values are marked with an asterisk, indicating they are less than 0.05.
  
  **Final Conclusion for Example 1:** The data provides strong evidence that the mean collapse flowrate is different for all three stenosis levels. As the stenosis level decreases (from 1 to 3), the mean flowrate at which the artery collapses significantly increases.
  
  ---
### **Example 2: Calcium Intake and Bone Density**
  
  *   **Scenario:** A study is designed to see if there is a difference in mean daily calcium intake among three groups of adults: normal bone density, osteopenia, and osteoporosis.
  *   **Factor:** Bone Density Class.
  *   **Levels (3 Groups):** Normal, Osteopenia, Osteoporosis.
  *   **Response Variable:** Daily calcium intake.
#### **Step 1: State the Hypotheses**
  *   **H₀:** The mean calcium intake is the same for all three bone density groups (\(\mu_{normal} = \mu_{osteo} = \mu_{osteo}\)).
  *   **Hₐ:** At least one of the mean calcium intakes is different.
  *   **Significance Level:** `α = 0.05`.
#### **Step 2: Perform the ANOVA F-test and Make a Conclusion**
  From the JMP output:
  *   **F-Ratio:** 1.3949
  *   **P-value:** 0.2782
  
  **Statistical Conclusion:**
  *   Since the **p-value (0.2782) is greater than `α` (0.05)**, we **fail to reject the null hypothesis (H₀)**.
  
  **Conclusion in Context:**
  There is **not sufficient statistical evidence** to conclude that the average daily calcium intake is different among the normal, osteopenia, and osteoporosis groups.
#### **Step 3: Post-Hoc Test?**
  Since we failed to reject H₀, we do not proceed with multiple comparisons. We found no evidence of *any* difference, so there is no need to look for *specific* differences.
#### **Step 4: Check the Assumptions**
  1.  **Normality:** The **Normal Probability Plot of Residuals** shows the points are reasonably close to the straight line. The **normality assumption holds**.
  2.  **Equal Variance:** The **Residuals vs Factor Plot** shows that the vertical spread of the data points for the "osteoporosis" group is much larger than for the "normal" and "osteopenia" groups. This indicates that the **equal variance assumption is violated**.
  
  **Important Implication:** The violation of the equal variance assumption makes the results of this ANOVA less reliable. While our conclusion was to "fail to reject," we should be cautious. A different statistical test that does not require equal variances (like the Welch's ANOVA) might be more appropriate.