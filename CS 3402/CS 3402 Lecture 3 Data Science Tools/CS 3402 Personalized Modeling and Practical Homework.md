#### **1. General vs. Personalized AI (Slides 19–22)**
In Part 2, we looked at a "General" spam filter (one that works for everyone). Slides 19–22 introduce **Personalized Spam Classification**.

*   **The Concept:** One person’s "Spam" is another person’s "Weekly Deal." 
  *   *Example:* If you are a professional baker, an email about "Discount Flour" is a useful business lead. If you are a software engineer, that same email is Spam.
*   **The Data Process (Slide 20):** 
  *   The model now relies on **User Feedback**. When you click "Report Spam" or "Looks Safe" in Gmail, you are effectively acting as the **Labeler** for your own private dataset.
*   **The Loop (Slides 21–22):** 
  *   The instructor repeats the flowchart for "Training" and "Testing."
  *   **Key Insight:** Notice the structure of the math is identical to the general model. This is the beauty of Data Science: **The algorithm doesn't care what the data is.** Whether it's house prices, spam emails, or cancer cells, the process of `Data -> Learning Algorithm -> Prediction Rule` remains exactly the same.

---
- #### **2. Homework Problem: Real Estate Regression (Slide 25)**
  Your first assignment is a **Regression** problem: predicting House Price based on Square Footage. 
  
  **The Dataset (Copy this for your code):**
  | Sample ID | Square Feet ($x$) | House Price ($y$) |
  | :--- | :--- | :--- |
  | 1 | 600 | 89,900 |
  | 2 | 1,576 | 300,300 |
  | 3 | 3,882 | 379,000 |
  | 4 | 11,396 | 1,700,000 |
  
  **"Fill-in" Context for the Data:**
  Look at Sample #3 and #4. 
  *   Sample 3 is almost 4,000 sq ft but only costs \$379k. 
  *   Sample 4 is 11,000 sq ft (a mansion) and costs \$1.7M.
  *   *Observation:* The relationship isn't perfectly linear. This is "Real World Data." A house might be huge but located in a cheaper area or be in poor condition. Your model has to find the "best fit" through these messy points.
  
  ---
- #### **3. Technical Requirements (Slides 24 & 26)**
  The instructor wants to see two specific skills: **Environment Setup** and **Visualization**.
  
  **Task A: The Environment (Slide 24)**
  1.  Install **Anaconda** (or Miniconda).
  2.  Create an environment: `conda create --name ds_course`
  3.  Install: `numpy`, `matplotlib`, `pandas`.
  
  **Task B: The Code (Slide 26)**
  You are required to do two things with the House Price data:
  1.  **Get Statistics:** Use Python to find the **Mean** and **Median** price per square foot.
    *   *Formula:* `(Price / SqFt)` for each house, then find the average.
  2.  **Visualization:** Use **Matplotlib** to create a scatter plot.
    *   **X-axis:** Square Feet.
    *   **Y-axis:** House Price.
  
  ---
- ### **Student Guide: How to complete this Homework**
  
  To get full credit, your Python script should look something like this (conceptually):
  
  ```python
  import numpy as np
  import matplotlib.pyplot as plt
  
  # 1. Define the data
  sq_ft = np.array([600, 1576, 3882, 11396])
  prices = np.array([89900, 300300, 379000, 1700000])
  
  # 2. Get Statistics (Requirement 1)
  price_per_sqft = prices / sq_ft
  print(f"Mean Price per SqFt: {np.mean(price_per_sqft)}")
  
  # 3. Visualization (Requirement 2)
  plt.scatter(sq_ft, prices, color='green')
  plt.xlabel('Square Feet')
  plt.ylabel('House Price ($)')
  plt.title('House Price vs. Square Footage in Rolla')
  plt.show()
  ```
  
  ---
- ### **Summary of Lecture 3**
  *   **Logistics:** Get your Python environment (Conda) set up now. Do not wait until the night before the homework is due.
  *   **Personalization:** AI becomes more powerful when it learns from *your* specific behavior (labels).
  *   **The Workflow:** Data Science is a repeatable loop. You clean data, you train a model, you evaluate its success, and you visualize the results to make sure they make sense.