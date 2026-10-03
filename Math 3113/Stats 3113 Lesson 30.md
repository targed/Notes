### **Stat 3113: Lesson 30 - ANOVA Examples**
  
  This lesson is dedicated to applying the concepts of One-Way ANOVA, Two-Way ANOVA, and 2³ Factorial Experiments to real-world scenarios. The focus is on recognizing the type of experiment, setting up the correct analysis, interpreting the results (especially p-values), checking assumptions, and drawing meaningful conclusions.
  
  ---
### **Example 1: Electrical Costs (Two-Way ANOVA)**
  
  *   **Scenario:** A contractor wants to know how the **type of outer covering** (4 levels: brick, wood, steel, vinyl) and the **type of air conditioning** (3 levels: heat pump, regular, none) affect the **electrical costs** (response variable).
  *   **Design:** Two-Way ANOVA (Balanced design with 4 replicates per treatment).
  
  **Analysis Steps:**
  1.  **Check Assumptions:**
    *   **Normality:** The Normal Probability Plot of residuals shows points hugging the line. **Assumption met.**
    *   **Equal Variance:** The Residuals vs. Predicted plot shows a roughly constant vertical spread (band) across the fitted values. **Assumption met.**
  2.  **Hypothesis Testing (α = 0.05):**
    *   **Step 1: Test Interaction (AC*Covering):** The p-value is **< .0001**. Since this is less than 0.05, we **reject H₀**. There is a **significant interaction**.
    *   **Step 2: Stop.** Because the interaction is significant, we do **not** proceed to test the main effects individually. The effect of AC type depends on the covering type.
  3.  **Conclusion:** The type of air conditioning and the type of outer covering interact to significantly impact the average electrical cost.
  
  ---
### **Example 2: Cholesterol Medication (One-Way ANOVA)**
  
  *   **Scenario:** Testing the effect of a new cholesterol medication at different doses.
  *   **Factor:** Dosage amount.
  *   **Levels:** 3 levels (0mg, 50mg, 100mg).
  *   **Response:** Cholesterol level.
  *   **Design:** One-Way ANOVA.
  
  **Analysis Steps:**
  1.  **Check Assumptions:**
    *   **Normality:** The Normal Quantile Plot shows a straight line pattern. **Assumption met.**
    *   **Equal Variance:** The "Oneway Analysis" plot shows similar spreads for each group. A test for equal variances (not shown, but implied) would likely pass. **Assumption met.**
  2.  **Hypothesis Testing (α = 0.05):**
    *   **Global F-test:** The p-value (Prob > F) is **0.0424**. Since 0.0424 < 0.05, we **reject H₀**. At least one dosage level results in a different mean cholesterol level.
  3.  **Post-Hoc Analysis (Tukey-Kramer HSD):**
    *   **Connecting Letters:** 0mg is level A, 100mg is level B. 50mg shares letters with both.
    *   **Ordered Differences:** The comparison "0 vs 100" has a p-value of **0.0417**. This is the only significant difference.
  4.  **Conclusion:** There is a significant difference in mean cholesterol levels between patients taking 0mg and 100mg. Specifically, the 100mg dose results in significantly lower cholesterol than the 0mg dose.
  
  ---
### **Example 3: Paper Strength (2³ Factorial Experiment)**
  
  *   **Scenario:** A paper company investigates factors affecting paper strength.
  *   **Factors:**
    *   A: Hardwood concentration (4%, 8%)
    *   B: Vat pressure (500, 650)
    *   C: Cooking time (3 hrs, 4 hrs)
  *   **Response:** Paper strength.
  *   **Design:** 2³ Factorial with 2 replicates.
  
  **Analysis Steps:**
  1.  **Check Assumptions:**
    *   **Normality:** The Normal Quantile Plot shows deviations from the line (points falling outside the confidence bands). **Normality assumption is suspect/violated.**
    *   **Equal Variance:** The Residual vs Predicted plot shows relatively constant spread. **Assumption met.**
    *   *Note:* We proceed with the analysis but note that the results should be interpreted with caution due to the normality issue.
  2.  **Hypothesis Testing (α = 0.05):**
    *   **Step 1: Test 3-Way Interaction (Concentration*Pressure*Time):** The p-value is **0.0197**.
    *   Since 0.0197 < 0.05, we **reject H₀**. There is a **significant 3-way interaction**.
    *   **Step 2: STOP.** Because the highest-order interaction is significant, we stop immediately. We do not look at 2-way interactions or main effects.
  3.  **Conclusion:** There is a significant three-way interaction between hardwood concentration, vat pressure, and cooking time. All three factors interact in a complex way to significantly impact the average paper strength. No main effects can be interpreted in isolation.