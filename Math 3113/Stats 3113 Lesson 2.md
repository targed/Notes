### **Stat 3113: Lesson 2 - Study Design and Confounding**
  
  This lesson transitions from *how* data is classified to *how* it is collected, focusing on the methods that allow us to draw valid conclusions, especially about cause and effect.
#### **1. Observation versus Experiment**
  
  The way data is collected determines the types of conclusions we can draw. This is one of the most important distinctions in statistics.
  
  *   **Observational Study:** A study in which the researcher **observes** individuals and measures variables of interest but does **not attempt to influence the responses**. The researcher is a passive observer, simply recording what is already happening.
    *   **Purpose:** To describe a group or situation, or to find associations between variables.
    *   **Example:** To study the link between smoking and lung cancer, an observational study would find people who *already* smoke and people who don't, and then compare the rates of lung cancer between the two groups.
  
  *   **Experiment:** A study in which the researcher **deliberately imposes a treatment** on individuals and then observes their responses. The researcher actively intervenes and manipulates variables.
    *   **Purpose:** To determine if a treatment **causes** a change in the response.
    *   **Example:** To study the link between smoking and lung cancer, a designed experiment would find a group of non-smokers and **randomly assign** half to a "smoking" group and the other half to a "non-smoking" control group, and then track them over time. (Note: This would be unethical, which is why observational studies are sometimes the only option).
  
  **Key Takeaway:** If the goal is to understand **cause and effect**, a well-designed experiment is the only fully convincing method.
#### **2. The Vocabulary of Experiments**
  
  *   **Experimental Units (or Subjects):** The individuals (people, animals, objects) on which the experiment is performed.
  *   **Response Variable:** The variable that measures the outcome or result of the study (the "effect"). In the plant example, this is the *height of the plant*.
  *   **Explanatory Variable (or Factor):** A variable that we think explains or causes changes in the response variable. In the plant example, the factors are *location*, *water type*, and *soil*.
  *   **Levels:** The specific values or categories of a factor. For the "location" factor, the levels are *full sun* and *partial sun*.
  *   **Treatment:** A specific experimental condition applied to the units.
    *   If there is only one factor, the treatments are the same as the levels of that factor.
    *   If there are multiple factors, a treatment is a **combination** of the levels of each factor.
  
  **Example 1: Plant Growth**
  *   **Experimental Unit:** A single plant seed/pot.
  *   **Response:** Plant height after two months.
  *   **Factors:** Location, Water Type, Soil.
  *   **Levels:**
    *   Location: {Full Sun, Partial Sun}
    *   Water Type: {Bottled, Tap}
    *   Soil: {Top, Potting}
  *   **Treatments:** The 8 possible combinations (e.g., Full Sun + Bottled Water + Top Soil is one treatment; Partial Sun + Tap Water + Potting Soil is another). This is a **2x2x2 = 2³ factorial experiment**.
#### **3. The Problem of Confounding in Observational Studies**
  
  Observational studies are prone to a major issue that prevents them from proving causation.
  
  *   **Lurking Variable:** A variable that is not included as an explanatory or response variable in a study but could still influence the interpretation of the relationship between them. It is "lurking" in the background.
    *   **Classic Example:** There is a strong positive association between children's foot size and their reading ability. However, larger feet do not *cause* better reading. The lurking variable is **age**. As children get older, both their feet and their reading skills grow.
  
  *   **Confounding:** A situation where the effects of two or more variables on a response variable cannot be distinguished from each other. The variables are "mixed up."
    *   **Example:** If you test a fertilizer by applying it to the sunny half of your lawn, the variables **fertilizer** and **sunshine** are confounded. You cannot tell if the greener grass is due to the fertilizer, the extra sun, or a combination of both.
  
  **Example 3: Job Training Program**
  This is an **observational study** because the mothers *chose* whether or not to participate; they were not randomly assigned.
  *   **Response:** Income.
  *   **Factor:** Participation in the job training program.
  *   **Conclusion:** Participants had higher incomes.
  *   **Can we say the program *caused* the higher income?** **No.** **Correlation does not imply causation.**
  *   **Problem:** There are many potential **lurking variables** that are confounded with "participation." The women who chose to participate might have been more motivated, had more prior work experience, or a better educational background. Any of these factors could be the real cause of their higher income.
#### **4. The Solution: Randomized Comparative Experiments**
  
  A well-designed experiment can eliminate the problem of confounding and allow us to establish causation.
  
  *   **Comparative Experiment:** The experiment compares two or more treatments. This is essential to control for the effects of lurking variables. Often, one of these is a **control group**, which receives an inactive treatment (placebo) or a baseline treatment.
  
  *   **Random Assignment:** Using a chance process (like a coin flip or random number generator) to assign experimental units to treatments.
    *   **Why is this critical?** Randomization does not eliminate lurking variables, but it spreads their effects approximately evenly across all treatment groups. This ensures that the only systematic difference between the groups is the treatment they receive, allowing for a fair comparison.
  
  *   **Completely Randomized Design (CRD):** A design in which all experimental units are allocated among all the treatments completely at random.
#### **5. The Three Principles of Experimental Design**
  
  To design a successful experiment, you must incorporate the following principles:
  
  1.  **Control:** Control for the effects of lurking variables by comparing two or more treatments. Create conditions that are as similar as possible for all groups, except for the treatments being tested.
  2.  **Randomize:** Use chance to assign subjects to treatments. This eliminates bias from the assignment process.
  3.  **Replicate:** Use a large enough number of subjects in each group. Replication reduces the impact of chance variation on the results, making it easier to detect a true treatment effect.
  
  When these principles are followed, an observed effect that is too large to have occurred by chance is deemed **statistically significant**, and we can conclude that the treatment **caused** the effect.