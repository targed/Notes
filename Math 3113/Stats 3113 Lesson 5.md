### **Stat 3113: Lesson 5 - Examples of Quantitative Distributions**
  
  This lesson demonstrates how to perform a complete analysis of a quantitative dataset by interpreting graphical and numerical summaries to describe the distribution's shape, outliers, center, and spread.
  
  ---
### **Example 1: Cylinder Diameters**
  
  *   **Scenario:** A company measures the diameters of a random sample of 60 cylinders to understand the distribution of this variable.
  *   **Analysis:** Based on the provided JMP output (boxplot and summary statistics), we analyze the distribution using the five aspects.
  
  1.  **Shape/Symmetry:** The distribution of cylinder diameters is **approximately symmetric**.
    *   *Justification:* The mean (49.999) and the median (50.010) are very close to each other. In the boxplot, the median line is nearly in the center of the box, and the upper and lower whiskers are of similar length.
  
  2.  **Outliers:** There is **one suspected outlier** at the maximum value of **50.36**.
    *   *Justification:* The boxplot shows a single point plotted separately above the top whisker.
  
  3.  **Center:** The center of the distribution is approximately **50.010**.
    *   *Justification:* Because the data is mostly symmetric but contains an outlier, the **median (50.010)** is the most robust and appropriate measure of the center. The mean is 49.999.
  
  4.  **Spread:** The spread of the middle 50% of the data is **0.165**.
    *   *Justification:* Since the median is the preferred measure of center, the **Interquartile Range (IQR = 0.165)** is the corresponding best measure of spread. The standard deviation is 0.134.
  
  5.  **Groups:** The distribution is **unimodal**, indicating that the data comes from a single group.
  
  ---
### **Example 2: Plastic Panel Bending Angles**
  
  *   **Scenario:** A researcher measures the deformity angle for a sample of 80 plastic panels.
  *   **Analysis:** We analyze the distribution of these bending angles.
  
  1.  **Shape/Symmetry:** The distribution of deformity angles is **skewed to the left**.
    *   *Justification:* The **mean (9.229) is noticeably less than the median (9.435)**. The boxplot visually confirms this, with a long tail extending to the lower values and the median line positioned towards the right (higher end) of the box.
  
  2.  **Outliers:** There are **multiple suspected outliers in the lower tail**.
    *   *Justification:* The boxplot shows approximately 6 data points plotted individually below the bottom whisker.
  
  3.  **Center:** The most appropriate measure for the center is the **median, which is 9.435**.
    *   *Justification:* Due to the significant left skew and the presence of outliers, the median gives a more accurate representation of the typical deformity angle than the mean.
  
  4.  **Spread:** The spread of the central 50% of the data is **0.828**.
    *   *Justification:* Corresponding to the median, the **IQR (0.828)** is the best measure of variability for this skewed distribution.
  
  5.  **Groups:** The distribution is **unimodal**.
  
  ---
### **Example 3: Comparing Chick Weight Gain**
  
  *   **Scenario:** An experiment is conducted to see if a new type of corn feed results in more weight gain for baby chicks compared to the current feed. 40 chicks are randomly assigned to two groups of 20.
  *   **Analysis:** We must analyze the distribution for each group and then compare them.
#### **Group 1: Current Corn Feed**
  *   **Shape:** Approximately symmetric (Mean ≈ 366.3, Median = 358).
  *   **Outliers:** None.
  *   **Center:** The mean weight gain is **366.3 ounces**, and the median is **358 ounces**.
  *   **Spread:** The standard deviation is 50.81 ounces, and the IQR is 68.25 ounces.
#### **Group 2: New Corn Feed**
  *   **Shape:** Approximately symmetric (Mean ≈ 403.0, Median = 406.5).
  *   **Outliers:** None.
  *   **Center:** The mean weight gain is **403.0 ounces**, and the median is **406.5 ounces**.
  *   **Spread:** The standard deviation is 42.73 ounces, and the IQR is 50 ounces.
#### **Final Answer and Justification**
  **Question:** Does it appear the new corn is resulting in significant weight gain?
  
  **Answer:** Yes, it appears that the new corn feed results in a significant weight gain.
  
  **Justification:**
  1.  **Comparison of Centers:** The measures of center for the "New Corn" group are substantially higher than for the "Current Corn" group. The mean weight gain increased from 366.3 to 403.0 ounces, and the median weight gain increased from 358 to 406.5 ounces.
  2.  **Visual Comparison:** The side-by-side boxplots show that the entire distribution for the "New Corn" group is shifted to higher values. The median (center line) of the new corn group is higher than the third quartile (the top of the box) of the current corn group, indicating a strong and consistent increase in weight gain.
  3.  **Spread:** The new feed also appears to result in slightly more consistent weight gain, as indicated by its smaller standard deviation (42.7 vs. 50.8) and smaller IQR (50 vs. 68.25).