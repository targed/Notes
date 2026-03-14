## 1. Regression Metrics
When predicting continuous numbers (like house prices), we measure the **Error** ($e_t = y_i - \hat{y}_i$), which is the difference between the Actual value ($y$) and the Predicted value ($\hat{y}$).

*   **MAE (Mean Absolute Error):** The average of the absolute errors. It treats all errors linearly (an error of \$10 is twice as bad as \$5).
*   **MSE (Mean Squared Error):** The average of squared errors. 
  *   *Crucial Logic:* Because it squares the error, it **penalizes large outliers** heavily.
*   **RMSE (Root Mean Squared Error):** The square root of MSE. It brings the error unit back to the original scale (e.g., from "dollars squared" back to "dollars").
*   **MAPE (Mean Absolute Percentage Error):** Useful when you care about the *relative* error (e.g., being off by \$100 on a \$1,000 car is worse than being off by \$100 on a \$100,000 house).

---
- ## 2. Classification: The Confusion Matrix
  This is the "Godfather" of classification metrics. For a binary problem (0/1), there are four outcomes:
  
  1.  **True Positive (TP):** You predicted 1, and it was 1. (Correct!)
  2.  **True Negative (TN):** You predicted 0, and it was 0. (Correct!)
  3.  **False Positive (FP):** You predicted 1, but it was 0.
    *   *Synonym:* **Type I Error** (A "False Alarm").
  4.  **False Negative (FN):** You predicted 0, but it was 1.
    *   *Synonym:* **Type II Error** (A "Miss").
  
  ---
- ## 3. The Core Four Metrics
- ### A. Accuracy
  *   **Formula:** $\frac{TP + TN}{Total}$
  *   **When it fails (The Medical Paradox):** If 99.9% of people are healthy, a model that always says "Healthy" is 99.9% accurate but useless. **Accuracy is dangerous on imbalanced data.**
- ### B. Precision (Purity)
  *   **Formula:** $\frac{TP}{TP + FP}$
  *   **Question:** "Of all the times I predicted Positive, how often was I right?"
  *   **Use Case:** When the cost of a **False Positive** is high (e.g., Spam filters—you don't want a job offer going to the spam folder).
- ### C. Recall (Completeness/Sensitivity)
  *   **Formula:** $\frac{TP}{TP + FN}$
  *   **Question:** "Of all the actual Positive cases, how many did I catch?"
  *   **Use Case:** When the cost of a **False Negative** is high (e.g., Cancer screening—missing one sick patient is a disaster).
- ### D. F1-Score (The Balancer)
  *   **Formula:** $2 \times \frac{Precision \times Recall}{Precision + Recall}$
  *   **Logic:** This is the **Harmonic Mean**. Unlike a regular average, if either Precision or Recall is close to zero, the F1-score will crash. It forces the model to be good at both.
  
  ---
- ## 4. Multi-class Confusion Matrices
  In a multi-class problem (e.g., classifying 4 types of fruit), the matrix is $4 \times 4$.
  *   **Diagonal Cells:** Represent correct predictions.
  *   **Off-Diagonal Cells:** Represent specific "confusions" (e.g., the model keeps thinking Lemons are Oranges).
  
  ---
- ## 5. ROC Curves and AUC
  Most models don't just output "Yes/No"; they output a **Probability** (e.g., 0.85 chance of Cat).
  
  *   **The Threshold:** You must choose a cutoff. If threshold = 0.5, everything > 0.5 is "Cat."
    *   *Trade-off:* Lowering the threshold increases **Recall** (you catch more cats) but decreases **Precision** (you start calling dogs "cats").
  *   **The ROC Curve:** A plot of the **True Positive Rate (Recall)** vs. the **False Positive Rate**. It shows the model's performance across *every possible threshold*.
  *   **AUC (Area Under Curve):** A single number from 0 to 1 summarizing the ROC plot.
    *   **AUC = 0.5:** The model is as good as a coin flip (Useless).
    *   **AUC = 1.0:** The "Perfect" model.
    *   *Advantage:* AUC is **threshold-independent**. It tells you how good the model is at separating classes, regardless of where you set the cutoff.
  
  ***
- # Problem Solving & Practice
  
  **1. The Medical Scenario:**
  An Airport Security scanner identifies weapons. 
  *   Which is worse: A False Positive (searching an innocent person) or a False Negative (letting a weapon through)?
  *   **Goal:** Optimize for **Recall**. We want to catch 100% of weapons, even if we have to search a few extra innocent people.
  
  **2. Metric Math:**
  $TP=120, TN=170, FP=70, FN=40$.
  *   **Precision:** $120 / (120+70) = 120/190 = \mathbf{0.63}$
  *   **Recall:** $120 / (120+40) = 120/160 = \mathbf{0.75}$
  *   **F1:** $2 \times (0.63 \times 0.75) / (0.63 + 0.75) \approx \mathbf{0.68}$ (Option C on Slide 32).
  
  **3. Interpreting ROC:**
  Model A has an AUC of 0.91. Model B has an AUC of 0.72.
  *   Which is better "overall"? **Model A**.
  *   *Challenge:* If your boss says "We MUST have a Recall of 0.8," you look at the point on the graph where TPR=0.8. Whichever model has the **lowest False Positive Rate** at that specific height is the winner for that specific task.
  
  ***
- # Summary of Lectures 4 & 5
  1.  **Distance** (L1, L2, Cosine, Jaccard) is how algorithms "see."
  2.  **Generalization** is the goal; **Cross-Validation** is the tool to prove it.
  3.  **Accuracy** is a lie for imbalanced data.
  4.  **Precision vs. Recall** is a business decision, not just a math one.
  5.  **ROC/AUC** evaluates how well your model can distinguish between classes, independent of your specific threshold.