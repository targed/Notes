### **Stat 3113: Lesson 31 - Professor Koob’s Projects (Factorial Experiments)**
  
  This lesson explores two specific types of factorial experiments often used in projects: the **2³ Factorial Experiment** and the **3² Factorial Experiment**.
  
  ---
### **1. The 2³ Factorial Experiment**
  
  A **2³ factorial experiment** involves **3 factors**, each tested at **2 levels**.
  *   **Total Runs:** \(2^3 = 8\) unique treatment combinations.
  *   **Goal:** To determine the main effects of the three factors and their interactions on a response variable.
#### **Example: Popcorn Yield**
  *   **Response Variable:**
    *   **Actual Response:** `% yield popped = (weight popped / package weight) * 100`
    *   **Measured Response:** `weight popped (g)`
  *   **Factors and Levels:**
    1.  **Brand:** Orville Redenbacher vs. Act II
    2.  **Butter Amount:** Low vs. High
    3.  **Time Popped:** 1.5 minutes vs. 2.5 minutes
  
  **Analysis Steps:**
  1.  **Check Assumptions:**
    *   **Normality:** Use a Normal Quantile Plot of the residuals. In the example, points fall close to the line, so normality is met.
    *   **Constant Variance:** Use a Residual by Predicted Plot. In the example, the spread is relatively constant across the fitted values, so this assumption is met.
  
  2.  **Hypothesis Testing (Step-by-Step):**
    *   **Step 1: Test Highest-Order Interaction (3-Way).**
        *   Look at the p-value for `Brand*Butter*Time`.
        *   **Result:** The p-value is **<.0001**.
        *   **Decision:** Since p < 0.05, we **reject H₀**. There is a significant three-way interaction.
    *   **Step 2: STOP.** Do not interpret 2-way interactions or main effects. The effect of any one factor depends on the specific levels of the other two.
  
  **Conclusion:** All three factors (Brand, Butter Amount, and Time) interact together to significantly impact the average percentage yield of the popcorn. You cannot say one brand is simply "better" than the other without specifying the time and butter amount.
  
  ---
### **2. The 3² Factorial Experiment**
  
  A **3² factorial experiment** involves **2 factors**, each tested at **3 levels**.
  *   **Total Runs:** \(3^2 = 9\) unique treatment combinations.
  *   **Goal:** Similar to the 2³ design, but allows for detecting non-linear effects because factors have 3 levels (Low, Medium, High).
#### **Example: Toy Car Ramp Distance**
  *   **Response Variable:** Distance the car travels from the bottom of the ramp (inches).
  *   **Factors and Levels:**
    1.  **Car Type:** Sport car, Stock car, Big truck/bus.
    2.  **Length of Ramp:** High, Middle, Low.
  
  **Analysis Steps:**
  1.  **Check Assumptions:**
    *   **Normality:** The Normal Quantile Plot shows points close to the line. Normality is fine.
    *   **Constant Variance:** The Residual by Predicted Plot shows a potential "megaphone" or funnel shape (spread increases as predicted value increases). **Constant variance is NOT met.**
    *   *Note:* In a real analysis, you might need to transform the data (e.g., take the log of the response) to fix this. For this example, we proceed with caution.
  
  2.  **Hypothesis Testing:**
    *   **Step 1: Test Interaction (Car Type*Ramp Height).**
        *   Look at the p-value.
        *   **Result:** The p-value is **<.0001**.
        *   **Decision:** Reject H₀. There is a significant interaction.
    *   **Step 2: STOP.** Do not test main effects.
  
  **Visualizing the Interaction:**
  *   The **Interaction Profiles plot** is crucial here.
  *   We see the lines for "Sports" and "Stock" cars behave differently than the line for the "Bus."
  *   *Example Observation:* The "Sports" car distance increases significantly when going from Low to High ramp height. However, the "Bus" distance stays relatively flat regardless of ramp height. This non-parallel behavior confirms the strong interaction.
  
  **Conclusion:** Both factors (Car Type and Ramp Height) interact together to impact the average distance traveled. The effect of increasing the ramp height depends heavily on which type of car you are using.