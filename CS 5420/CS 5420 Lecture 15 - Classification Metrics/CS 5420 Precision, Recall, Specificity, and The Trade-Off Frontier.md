## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Accuracy of the Trivial Baseline on Rare Fraud:**
   * In a dataset with $N = 10{,}000$ transactions and only $N_+ = 20$ frauds, the majority negative class makes up $N_- = 9{,}980$ instances ($99.8\%$).
   * A trivial baseline predicting $\hat{y}_i = 0$ for all instances achieves:
     $$\text{Accuracy} = \frac{TN}{N} = \frac{9{,}980}{10{,}000} = \mathbf{0.998 \quad (99.8\%)}$$
   * **Zero Utility:** It detects zero fraud ($TP = 0, \text{Recall} = 0.0\%$), allowing $100\%$ of financial theft to pass undetected. It optimizes the evaluation metric without solving the underlying classification task.
  2. **Type I vs. Type II Errors:**
   * **Type I Error (False Positive / False Alarm):** Predicting positive when the true state is negative ($\hat{Y} = 1 \mid Y = 0$).
   * **Type II Error (False Negative / Miss):** Predicting negative when the true state is positive ($\hat{Y} = 0 \mid Y = 1$).
   * *Example where Type II dominates:* In oncology screening, a Type I error leads to an unnecessary confirmatory biopsy (temporary emotional distress, non-fatal operational cost). A Type II error sends a patient home with an undiagnosed, rapidly metastasizing malignant tumor (fatal).
  3. **Multiclass False Negatives for Class 2 ($FN_2$):**
   * False Negatives represent instances where the true label is Class 2 ($i = 2$), but the model assigned them to any other predicted class ($j \neq 2$).
   * In a $3 \times 3$ matrix $M$, this is the **row sum excluding the diagonal entry**:
     $$\mathbf{FN_2 = M_{21} + M_{23} = \sum_{j \neq 2} M_{2j}}$$
  
  ---
## 2. Row-Conditioned vs. Column-Conditioned Metrics (Slide 5)
  
  Slide 5 illustrates how the three core classification metrics extract signal from different marginals of the confusion matrix:
  
  $$\mathbf{\text{"Same box, three questions. Each one reads a different row or column."}}$$
  
  ```
                       The Structural Marginality of Metrics (Slide 5)
                                          Predicted Class
                                  pred +                   pred -
                            ┌────────────────────────┬────────────────────────┐
                   actual + │         TP = 9         │         FN = 1         │ ──▶ RECALL (Sensitivity)
  Actual Class                ├────────────────────────┼────────────────────────┤     Row 1 Marginal (Conditioned on Y=1)
                   actual - │        FP = 21         │        TN = 969        │ ──▶ SPECIFICITY (TNR)
                            └────────────────────────┴────────────────────────┘     Row 2 Marginal (Conditioned on Y=0)
                                         │
                                         ▼
                                     PRECISION
                                 Column 1 Marginal
                               (Conditioned on Ŷ=1)
  ```
### The Epistemological Distinction:
  * **Row-Conditioned Metrics ($P(\hat{Y} \mid Y)$):** Condition on the **ground-truth state of nature**. They answer: *"Given what truly happened in the real world, how did the model behave?"*
  * *Recall / Sensitivity:* Conditioned on $Y = 1$.
  * *Specificity:* Conditioned on $Y = 0$.
  * **Column-Conditioned Metrics ($P(Y \mid \hat{Y})$):** Condition on the **model's algorithmic output**. They answer: *"Given that the model emitted an alert, what is the probability that it reflects reality?"*
  * *Precision (Positive Predictive Value):* Conditioned on $\hat{Y} = 1$.
  * *Negative Predictive Value (NPV):* Conditioned on $\hat{Y} = 0$.
  
  ---
## 3. Deconstructing the Three Canonical Questions (Slide 5)
  
  ---
### Metric 1: Precision (Positive Predictive Value / PPV)
  Slide 5 poses the diagnostic question:
  $$\mathbf{\text{"Of what I flagged, how much was real?"}}$$
  
  $$\mathbf{\text{Precision} = \frac{TP}{TP + FP} = P(Y = 1 \mid \hat{Y} = 1)}$$
  
  Evaluating Model B from Slide 5:
  $$\text{Precision} = \frac{9}{9 + 21} = \frac{9}{30} = \mathbf{0.300 \quad (30.0\%)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Operational Meaning of Precision:                                      │
  │                                                                        │
  │ • When the model sounds an alarm (flags a transaction as fraud), there │
  │   is only a 30% chance that it is genuine fraud.                      │
  │ • 70% of the alert queue (21 out of 30 flagged accounts) consists of  │
  │   harmless, false alarms on legitimate users.                          │
  │ • Precision directly quantifies ALERT FATIGUE and investigative waste. │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Metric 2: Recall / Sensitivity (True Positive Rate / TPR)
  Slide 5 poses the complementary question:
  $$\mathbf{\text{"Of what was real, how much did I find?"}}$$
  
  $$\mathbf{\text{Recall} = \text{Sensitivity} = \text{TPR} = \frac{TP}{TP + FN} = P(\hat{Y} = 1 \mid Y = 1)}$$
  
  Evaluating Model B from Slide 5:
  $$\text{Recall} = \frac{9}{9 + 1} = \frac{9}{10} = \mathbf{0.900 \quad (90.0\%)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Operational Meaning of Recall:                                         │
  │                                                                        │
  │ • The model successfully detected 90% of all fraudulent activities.   │
  │ • It missed only 10% (1 out of 10 fraud events escaped through FN = 1).│
  │ • Recall quantifies SAFETY and SYSTEM COVERAGE.                        │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Metric 3: Specificity (True Negative Rate / TNR)
  Slide 5 poses the baseline question:
  $$\mathbf{\text{"Of the negatives, how many did I clear?"}}$$
  
  $$\mathbf{\text{Specificity} = \text{TNR} = \frac{TN}{TN + FP} = P(\hat{Y} = 0 \mid Y = 0)}$$
  
  Evaluating Model B from Slide 5:
  $$\text{Specificity} = \frac{969}{969 + 21} = \frac{969}{990} \approx \mathbf{0.9788 \approx 0.980 \quad (98.0\%)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Operational Meaning of Specificity:                                    │
  │                                                                        │
  │ • The model correctly cleared 98% of all legitimate transactions       │
  │   without interrupting normal account holders.                         │
  │ • Specificity quantifies INLIER PRESERVATION and NON-INTERFERENCE.     │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. The Duality: Specificity vs. False Positive Rate (FPR) (Slide 5)
  
  Slide 5 notes:
  $$\mathbf{\text{Recall} = \text{Sensitivity} = \text{True Positive Rate (TPR)}}$$
  $$\mathbf{1 - \text{Specificity} = \text{False Positive Rate (FPR)}}$$
### Mathematical Derivation of the Complement:
  The False Positive Rate measures the fraction of actual negative instances that were incorrectly flagged as positive:
  
  $$\text{FPR} = \frac{FP}{N_-} = \frac{FP}{TN + FP}$$
  
  Since $N_- = TN + FP$:
  
  $$\text{FPR} = \frac{(TN + FP) - TN}{TN + FP} = \frac{TN + FP}{TN + FP} - \frac{TN}{TN + FP} = 1 - \text{Specificity}$$
  
  $$\mathbf{\text{FPR} = 1 - \text{TNR} = 1 - \text{Specificity}}$$
  
  For Model B:
  $$\text{FPR} = 1 - 0.9788 = \mathbf{0.0212 \quad (2.12\%)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Foundation of the ROC Space:                                       │
  │                                                                        │
  │ The Receiver Operating Characteristic (ROC) curve is nothing more      │
  │ than a parametric plot of these two row-normalized metrics across all  │
  │ possible decision thresholds τ ∈ [0, 1]:                               │
  │                                                                        │
  │           Y-Axis: Sensitivity (TPR)   vs.   X-Axis: 1 - Specificity (FPR)│
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 5. The Base-Rate Fallacy: Why High Specificity Masks Abysmal Precision
  
  Look at the metrics for Model B on Slide 5:
  * **Specificity is exceptionally high:** $\mathbf{97.9\%}$ ($0.979$)
  * **Recall is exceptionally high:** $\mathbf{90.0\%}$ ($0.900$)
  * **Yet Precision is abysmal:** $\mathbf{30.0\%}$ ($0.300$)
  
  Why does a model that correctly clears nearly $98\%$ of negative instances end up being wrong $70\%$ of the time when it flags an alert?
  
  ---
### Mathematical Proof via Bayes' Theorem:
  Express Precision ($\text{PPV}$) as a conditional probability and expand using Bayes' Rule:
  
  $$\text{Precision} = P(Y = 1 \mid \hat{Y} = 1) = \frac{P(\hat{Y} = 1 \mid Y = 1) P(Y = 1)}{P(\hat{Y} = 1)}$$
  
  Applying the Law of Total Probability to the denominator:
  
  $$P(\hat{Y} = 1) = P(\hat{Y} = 1 \mid Y = 1) P(Y = 1) + P(\hat{Y} = 1 \mid Y = 0) P(Y = 0)$$
  
  Substitute the definitions of **Prevalence ($\pi$)**, **Sensitivity ($Se$)**, and **False Positive Rate ($1 - Sp$)**:
  
  $$\mathbf{\text{Precision} = \frac{\pi \cdot Se}{\pi \cdot Se + (1 - \pi)(1 - Sp)}}$$
  
  ```
  Evaluating the Formula for Model B:
  • Prevalence π = P(Y = 1) = 10 / 1,000 = 0.010 (1.0% Rare Base Rate)
  • Sensitivity Se = 0.900
  • Specificity Sp = 0.9788  ──▶  FPR (1 - Sp) = 0.0212
  
                  (0.010) · (0.900)
  Precision = ─────────────────────────────────────────
            (0.010) · (0.900) + (0.990) · (0.0212)
  
                 0.0090                0.0090
          = ───────────────── = ─────────────────── = 0.300 (30.0%)
            0.0090 + 0.02099          0.02999
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Base-Rate Fallacy Principle:                                       │
  │                                                                        │
  │ • Because negative samples vastly outnumber positive samples           │
  │   ((1 - π) >> π, here 990 vs. 10), even a microscopic False Positive   │
  │   Rate (1 - Sp = 2.1%) operates on a massive pool (990 instances).     │
  │                                                                        │
  │ • The absolute count of false alarms (FP = 21) outnumbers the absolute │
  │   count of true detections (TP = 9).                                   │
  │                                                                        │
  │ • Specificity measures false alarms relative to the massive negative   │
  │   pool (21 / 990 = 2.1%), making performance look clean.               │
  │                                                                        │
  │ • Precision measures false alarms relative to the actual alert queue   │
  │   (9 / 30 = 30%), revealing that the queue is dominated by noise!      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 6. The Precision-Recall Trade-Off & Threshold Moving
  
  In probabilistic models (such as Logistic Regression), discrete class labels are assigned by applying a cutoff threshold $\tau \in [0, 1]$ to the continuous posterior probability:
  
  $$\hat{y}_i = \begin{cases} 1 & \text{if } \hat{p}_i \ge \tau \\ 0 & \text{if } \hat{p}_i < \tau \end{cases}$$
  
  ```
                   The Precision-Recall Pareto Frontier
    Precision
       ▲
   1.0 │  ● Threshold τ ──▶ 1.0 (Conservative Policy)
       │   \ • Predicts positive only when absolute certainty exists.
       │    \ • FP ──▶ 0  ⟹  Precision ──▶ 1.0
   0.8 │     \ • But FN explodes  ⟹  Recall drops near 0.0!
       │      \
   0.6 │       \
       │        \  ● Threshold τ = 0.50 (Default Symmetric Cutoff)
   0.4 │         \
       │          \
   0.2 │           \  ● Threshold τ ──▶ 0.0 (Aggressive Screening Policy)
       │            \ • Flags almost everything as positive.
   0.0 ┴─────────────┴────────────────────────────────────────────▶ Recall (TPR)
      0.0           0.4           0.6           0.8           1.0
       • FN ──▶ 0  ⟹  Recall ──▶ 1.0
       • But FP explodes  ⟹  Precision collapses to prevalence π!
  ```
### The Operational Mechanism:
  * **Raising $\tau$ ($\tau \to 1.0$):**
  * The model requires overwhelming evidence before sounding an alarm.
  * False Positives decrease ($FP \downarrow$), driving **Precision up**.
  * However, borderline positive cases are missed ($FN \uparrow$), causing **Recall to collapse**.
  * **Lowering $\tau$ ($\tau \to 0.0$):**
  * The model flags instances even under slight suspicion.
  * False Negatives decrease ($FN \downarrow$), driving **Recall up toward $100\%$**.
  * However, harmless inliers are swept into the alert queue ($FP \uparrow$), causing **Precision to plummet**.
  
  **The Fundamental Law:** You cannot maximize both Precision and Recall simultaneously by adjusting thresholds alone. Moving $\tau$ shifts your operating point along the Pareto frontier; improving both requires engineering richer features or selecting higher-capacity model architectures.
  
  ---
## Summary Review Questions for Section 2
  
  1. *In your own words, what is the conceptual difference between the question answered by Precision and the question answered by Recall?*
  2. *Using the Bayes' Rule formulation for Precision ($\text{PPV}$), explain why a medical test with $99\%$ sensitivity and $99\%$ specificity can still produce a Precision of under $50\%$ when applied to a rare disease with a prevalence of $\pi = 0.001$ ($0.1\%$).*
  3. *If you lower the classification probability threshold $\tau$ from $0.50$ to $0.15$ in an automated cancer detection system, what happens to the count of False Positives ($FP$), the count of False Negatives ($FN$), and the model's Precision and Recall?*
  
  ---