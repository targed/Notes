### **Stat 3113: Lesson 29 - 2³ Factorial Experiment Analysis**
  
  This lesson focuses on a specific type of multi-factor experiment that is extremely common in engineering and industrial applications because of its efficiency in screening important factors.
  
  ---
### **1. What is a 2³ Factorial Experiment?**
  
  A **2³ factorial experiment** is a design with **three factors**, where each factor is tested at exactly **two levels**.
  
  *   **Three Factors:** Typically labeled A, B, and C.
  *   **Two Levels:** Each factor has a "Low" level and a "High" level.
  *   **Number of Runs:** There are \(2^3 = 8\) unique treatment combinations (runs) required for a single replicate of the full experiment.
#### **Notation for Treatment Combinations**
  Standard notation uses lowercase letters to represent the treatment combinations.
  *   **Presence of a letter** (e.g., `a`) means that specific factor is at its **High** level.
  *   **Absence of a letter** means that factor is at its **Low** level.
  *   **"1"** represents the combination where **all** factors are at their **Low** level.
  
  **The 8 Treatment Combinations:**
  1.  `(1)`: A low, B low, C low
  2.  `a`: A high, B low, C low
  3.  `b`: A low, B high, C low
  4.  `ab`: A high, B high, C low
  5.  `c`: A low, B low, C high
  6.  `ac`: A high, B low, C high
  7.  `bc`: A low, B high, C high
  8.  `abc`: A high, B high, C high
  
  ---
### **2. The ANOVA Table for a 2³ Design**
  
  Just like in Two-Way ANOVA, we analyze the variance to find significant effects. A 2³ design allows us to test for:
  
  1.  **Three Main Effects:** Factor A, Factor B, Factor C.
  2.  **Three Two-Way Interactions:** AB, AC, BC.
  3.  **One Three-Way Interaction:** ABC.
  
  The ANOVA table partitions the total variability into these 7 components plus the Error term.
  
  *   **Degrees of Freedom:**
    *   Each Main Effect and Interaction has **1 degree of freedom** (since there are only 2 levels, `2-1=1`).
    *   Total df = `N - 1` (where N is the total number of observations).
    *   Error df = Total df - 7.
  
  ---
### **3. Hypothesis Testing Procedure**
  
  There is a strict **hierarchy** for testing hypotheses in factorial experiments. We always start with the highest-order interaction and move down.
#### **Step 1: Test the 3-Way Interaction (ABC)**
  *   **H₀:** The ABC interaction is not significant (no 3-way interaction).
  *   **Test:** Check the p-value for ABC.
    *   **If p-value ≤ α:** Reject H₀. The 3-way interaction is present. **STOP.** Do not test any lower-order effects (2-way or main effects). The relationship is complex and depends on the levels of all three factors simultaneously.
    *   **If p-value > α:** Fail to reject H₀. The 3-way interaction is not significant. **Proceed to Step 2.**
#### **Step 2: Test the 2-Way Interactions (AB, AC, BC)**
  Only perform this if the 3-way interaction was absent.
  *   Test each 2-way interaction individually (AB, AC, and BC).
  *   **Decision Rule:**
    *   **If a 2-way interaction (e.g., AB) is significant:** Do **not** test the main effects involved in that interaction (A or B). You *can* still test the main effect of the third factor (C), provided it is not involved in any other significant interactions (AC or BC).
    *   **If a 2-way interaction is NOT significant:** You can proceed to test the main effects of the factors involved.
#### **Step 3: Test the Main Effects (A, B, C)**
  Only test the main effects that are "permitted" by the previous steps (i.e., factors that are not part of any significant higher-order interaction).
  
  **Summary of the Logic:** "Never interpret a lower-order effect when a higher-order interaction involving those factors is significant."
  
  ---
### **4. Worked Example: Chemical Reaction Yield**
  
  *   **Factors:** A (Catalyst), B (Reagent), C (Stirring Rate).
  *   **Design:** 2³ factorial with 3 replicates (Total N = 8 * 3 = 24 runs).
  *   **Response:** Yield.
  
  **Analysis Steps (using JMP output):**
  
  1.  **Check Assumptions:**
    *   **Normality:** The Normal Probability Plot of residuals shows points close to the line. **Normality assumption holds.**
    *   **Equal Variance:** The Residuals vs. Predicted plot shows a random scatter with roughly equal vertical spread. **Equal variance assumption holds.**
  
  2.  **Hypothesis Testing (at α = 0.05):**
    *   **Test 3-way (Catalyst*Reagent*Stir):**
        *   P-value = 0.3647 (from output, usually).
        *   Since 0.3647 > 0.05, **Fail to reject**. No 3-way interaction. **Proceed.**
    *   **Test 2-way Interactions:**
        *   **Catalyst*Reagent:** P-value = 0.5496. **Not significant.**
        *   **Catalyst*Stir:** P-value = 0.2589. **Not significant.**
        *   **Reagent*Stir:** P-value = 0.3669. **Not significant.**
        *   Since *none* of the interactions are significant, we can proceed to test *all* main effects.
    *   **Test Main Effects:**
        *   **Catalyst:** P-value = 0.0155. **Significant.** (Catalyst affects yield).
        *   **Reagent:** P-value = 0.0296. **Significant.** (Reagent affects yield).
        *   **Stir Rate:** P-value = 0.4263. **Not significant.** (Stirring rate does not affect yield).
  
  **Final Conclusion:** The type of catalyst and the type of reagent both significantly affect the chemical yield. However, the stirring rate does not have a significant effect, and there are no significant interactions between any of the factors.