### **Stat 3113: Lesson 28 - Two-Way ANOVA**
  
  This lesson expands on the concepts of ANOVA by introducing **Two-Way ANOVA**, a method used to analyze experiments with **two independent factors**. While One-Way ANOVA determines if a single factor affects a response variable, Two-Way ANOVA allows us to study the individual effects of two factors simultaneously, as well as the crucial concept of their combined, or **interaction**, effect.
  
  ---
### **1. Key Concepts in Two-Factor Experiments**
  
  In a two-factor (or "full factorial") experiment, we examine how a continuous response variable is affected by two categorical independent variables (factors).
  
  *   **Factors:** The two independent variables being studied (e.g., Catalyst and Reagent).
  *   **Levels:** The different categories within each factor. We denote the number of levels for the row factor as `I` and for the column factor as `J`.
  *   **Treatment Combination:** A specific combination of one level from the row factor and one level from the column factor. There are `I * J` total treatment combinations.
  *   **Replicates (K):** The number of observations (sample size) for each specific treatment combination.
  *   **Balanced Design:** A design where the number of replicates `K` is the same for every treatment combination. The analysis in this lesson applies only to balanced designs.
  
  ---
### **2. Main Effects and the Interaction Effect**
  
  Two-Way ANOVA analyzes three distinct potential effects:
#### **A. Main Effects**
  A **main effect** is the individual effect of one factor on the response variable, averaging across all the levels of the other factor.
  *   **Main Effect of Factor A:** Does changing the levels of Factor A have a significant impact on the response?
  *   **Main Effect of Factor B:** Does changing the levels of Factor B have a significant impact on the response?
#### **B. The Interaction Effect**
  The **interaction effect** is the most important new concept in Two-Way ANOVA.
  
  *   **Definition:** An interaction effect is present when the effect of one factor on the response variable **depends on the level of the other factor**.
  *   **Interpretation:** It means the two factors don't work in isolation; their combined effect is different from just adding their individual effects together. For example, Catalyst A might produce the highest yield with Reagent 1, but the lowest yield with Reagent 2.
  
  ---
### **3. The Interaction Plot: Visualizing Interactions**
  
  The easiest way to check for a potential interaction is to use an **interaction plot**.
  *   **How it works:** The plot shows the mean of the response variable for each level of one factor, with separate lines for each level of the other factor.
  *   **Interpretation:**
    *   **No Interaction:** If there is no interaction, the lines on the plot will be approximately **parallel**. This indicates that the effect of one factor is consistent across all levels of the other.
    *   **Interaction Present:** If an interaction exists, the lines will be **non-parallel**—they may cross or have distinctly different slopes. The more the lines deviate from being parallel, the stronger the interaction effect.
  
  ---
### **4. The Hierarchy of Hypotheses in Two-Way ANOVA**
  
  There is a strict order for testing hypotheses in a Two-Way ANOVA. **You must always test for the interaction effect first.**
#### **Step 1: Test for Interaction**
  *   **H₀:** There is **no interaction** between the two factors.
  *   **Hₐ:** There **is an interaction** between the two factors.
  
  **Decision:**
  *   If the **p-value > α (e.g., 0.05)**, the interaction is **not significant**. You fail to reject H₀.
    *   **Action:** Since there is no interaction, it is meaningful to proceed to test the main effects of each factor individually.
  *   If the **p-value ≤ α**, the interaction **is significant**. You reject H₀.
    *   **Action: STOP.** Do not test the main effects. The presence of a significant interaction means the main effects are not meaningful on their own (because the effect of one factor depends on the other). The interaction itself is the most important conclusion of the study.
#### **Step 2: Test for Main Effects (ONLY if no interaction was found)**
  If the interaction was not significant, you proceed with two separate tests:
  
  *   **Test for Main Effect of Factor A (Row Factor):**
    *   **H₀:** The mean response is the same for all levels of Factor A.
    *   **Hₐ:** The mean response is different for at least one level of Factor A.
  *   **Test for Main Effect of Factor B (Column Factor):**
    *   **H₀:** The mean response is the same for all levels of Factor B.
    *   **Hₐ:** The mean response is different for at least one level of Factor B.
  
  ---
### **5. Worked Examples**
#### **Example 1: Chemical Yields**
  *   **Factor A:** Catalyst (4 levels), **Factor B:** Reagent (3 levels)
  *   **Response:** Chemical Yield
  
  1.  **Test for Interaction (Catalyst*Reagent):**
    *   From the ANOVA output, the p-value is **0.5496**.
    *   Since `0.5496 > 0.05`, we **fail to reject H₀**. There is **no significant interaction**.
    *   **Action:** We can proceed to test the main effects.
  
  2.  **Test for Main Effects:**
    *   **Catalyst:** The p-value is **0.0001**. Since `0.0001 < 0.05`, we conclude there is a **significant main effect of Catalyst** on the mean yield.
    *   **Reagent:** The p-value is **0.0101**. Since `0.0101 < 0.05`, we conclude there is a **significant main effect of Reagent** on the mean yield.
#### **Example 2: Motor Vibration**
  *   **Factor A:** Material (3 levels), **Factor B:** Supply Source (5 levels)
  *   **Response:** Vibration (microns)
  
  1.  **Test for Interaction (Material*Supply Source):**
    *   From the ANOVA output, the p-value is **< .0001**.
    *   Since `< .0001 < 0.05`, we **reject H₀**. There **is a significant interaction**.
    *   **Action: STOP.** We do not test the main effects.
  
  2.  **Conclusion:** There is a significant interaction between the casing material and the bearing supply source. This means the effect of the material on motor vibration *depends on which supplier provides the bearings*. To understand the results, one would need to look at the interaction plot to see which specific combinations of material and supplier lead to high or low vibration.