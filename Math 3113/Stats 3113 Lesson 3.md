### **Stat 3113: Lesson 3 - Practical Examples of Experimental Design**
  
  This lesson applies the principles of experimental design (Control, Randomization, and Replication) to two detailed, real-world scenarios. The focus is on **Factorial Experiments**, which are efficient designs for studying the effects of multiple explanatory variables (factors) at once.
  
  A factorial experiment is one in which all possible combinations of the levels of the factors are investigated. The notation `L^F` is often used, where `L` is the number of levels and `F` is the number of factors.
  
  ---
### **Example 1: Steel Ball Falling Through Fluid**
  
  This experiment is a **3² Factorial Experiment**: It has **2 factors**, each with **3 levels**.
  
  *   **Objective:** To determine how the diameter of a steel ball and the type of fluid affect the time it takes for the ball to fall through the fluid.
#### **A. Deconstructing the Experiment**
  
  *   **Response Variable:** The time (in seconds) for the steel ball to travel from the top to the bottom of the container.
  *   **Factor 1:** Diameter of the steel ball.
    *   **Levels:** {3 mm, 6 mm, 12 mm}
  *   **Factor 2:** Type of fluid.
    *   **Levels:** {Water, Vegetable Oil, Laundry Detergent}
  *   **Treatments:** There are 3 levels × 3 levels = **9 total treatments** (unique combinations of diameter and fluid).
  
  | Treatment | Diameter | Liquid |
  | :--- | :--- | :--- |
  | 1 | 3mm | water |
  | 2 | 3mm | veg oil |
  | 3 | 3mm | detergent |
  | 4 | 6mm | water |
  | 5 | 6mm | veg oil |
  | 6 | 6mm | detergent |
  | 7 | 12mm | water |
  | 8 | 12mm | veg oil |
  | 9 | 12mm | detergent |
#### **B. Applying the Principles of Experimental Design**
  
  The experimental process shows how to apply Control, Randomization, and Replication:
  
  *   **CONTROL (Constants):** These are steps taken to reduce the effect of outside variables and ensure a fair comparison.
    *   Using the *same type* of each liquid for all runs (e.g., same brand of oil).
    *   Using one specific steel ball for each size to eliminate manufacturing differences.
    *   Filling the container to the *same set volume* for every run.
    *   Dropping the ball from the *exact center* of the container each time.
    *   Having a consistent procedure for starting and stopping the stopwatch.
    *   **Crucially, cleaning the container with soap, water, and alcohol, and fully drying it between runs.** This prevents cross-contamination of the fluids, which would introduce bias.
  
  *   **RANDOMIZATION:**
    *   The 9 treatments are entered into statistical software (JMP), which will **randomize the experiment order**. This prevents any time-related lurking variables (like experimenter fatigue or changes in ambient temperature) from systematically affecting one treatment more than another.
  
  *   **REPLICATION:**
    *   Each of the 9 treatments will be **replicated 4 times**.
    *   This results in a total of 9 × 4 = **36 observations (or runs)**. Replication provides more reliable data and helps average out the effects of random error. Because each treatment has the same number of replicates, this is a **balanced experiment**.
  
  ---
### **Example 2: Boiling Water**
  
  This experiment is a **2³ Factorial Experiment**: It has **3 factors**, each with **2 levels**.
  
  *   **Objective:** To determine how pot size, the addition of salt, and the stove temperature setting affect the time it takes to boil a set amount of water.
#### **A. Deconstructing the Experiment**
  
  *   **Response Variable:** The time (in seconds) for 4 cups of water to reach a "simple boil."
  *   **Factor 1:** Pot used.
    *   **Levels:** {large pot, small pot}
  *   **Factor 2:** Amount of salt added.
    *   **Levels:** {1 tablespoon, none}
  *   **Factor 3:** Stove temperature setting.
    *   **Levels:** {high, medium}
  *   **Treatments:** There are 2 levels × 2 levels × 2 levels = **8 total treatments**.
  
  | Treatment | Pot | Salt | Temp |
  | :--- | :--- | :--- | :--- |
  | 1 | large | none | high |
  | 2 | large | none | low |
  | 3 | large | 1 tbsp | high |
  | 4 | large | 1 tbsp | low |
  | 5 | small | none | high |
  | 6 | small | none | low |
  | 7 | small | 1 tbsp | high |
  | 8 | small | 1 tbsp | low |
#### **B. Applying the Principles of Experimental Design**
  
  *   **CONTROL (Constants):**
    *   The starting water temperature is the same for every trial.
    *   The stove top is allowed to return to **room temperature** before the next trial begins. This is critical to prevent residual heat from biasing the next run.
    *   The volume of water is fixed at **4 cups**.
    *   The endpoint is clearly defined: a **"simple boil," not a "rolling boil."** This specificity makes the measurement of the response variable more consistent.
    *   The pot is cleaned and fully dried between runs.
  
  *   **RANDOMIZATION:**
    *   Just as in the first example, the **run order of the 8 treatments will be randomized** by software to prevent bias.
  
  *   **REPLICATION:**
    *   Each of the 8 treatments will be **replicated 4 times**.
    *   This results in a total of 8 × 4 = **32 observations**. This replication ensures the results are more robust against random chance.