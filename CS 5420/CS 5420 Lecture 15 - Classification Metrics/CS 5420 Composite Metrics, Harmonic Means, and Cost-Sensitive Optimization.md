## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **Conceptual Difference: Precision vs. Recall:**
   * **Precision ($P(Y = 1 \mid \hat{Y} = 1)$):** Evaluates the **credibility of the model's alerts**. It asks: *"Given that the model sounded an alarm, what is the probability that a real event occurred?"*
   * **Recall ($P(\hat{Y} = 1 \mid Y = 1)$):** Evaluates the **completeness of the model's coverage**. It asks: *"Given that an event truly exists in the population, what is the probability that the model caught it?"*
  2. **The Base-Rate Fallacy Calculation ($\pi = 0.001$, $Se = 0.99$, $Sp = 0.99$):**
   * Applying Bayes' Rule for Positive Predictive Value:
     $$\text{Precision} = \frac{\pi \cdot Se}{\pi \cdot Se + (1 - \pi)(1 - Sp)}$$
     $$\text{Precision} = \frac{(0.001)(0.99)}{(0.001)(0.99) + (0.999)(1 - 0.99)} = \frac{0.00099}{0.00099 + (0.999)(0.01)}$$
     $$\text{Precision} = \frac{0.00099}{0.00099 + 0.00999} = \frac{0.00099}{0.01098} \approx \mathbf{0.0902 \quad (9.02\%)}$$
   * **Takeaway:** Even with an exceptional $99\%$ sensitivity and $99\%$ specificity, a positive test result corresponds to only a **$9.0\%$ probability** that the patient actually has the disease. Over $90\%$ of all positive alerts are false alarms due to the massive negative base rate ($99.9\%$).
  3. **Impact of Lowering the Threshold ($\tau = 0.50 \to 0.15$):**
   * Lowering the threshold causes the model to classify samples as positive under lower suspicion.
   * **Confusion Matrix Shifts:** False Positives increase ($FP \uparrow$), False Negatives decrease ($FN \downarrow$).
   * **Metric Impact:** Precision drops (more false alarms in the alert queue), while Recall increases (fewer real cases are missed).
  
  ---
## 2. $F_1$: The Harmonic Mean of Precision and Recall (Slide 6)
  
  Slide 6 introduces the standard single-number summary for imbalanced evaluation:
  
  $$\mathbf{\text{"}F_1 \text{ is the harmonic mean: it is high only when precision and recall are both high."}}$$
  
  $$\mathbf{F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} = \frac{2TP}{2TP + FP + FN}}$$
  
  Evaluating Model B from Slide 5:
  $$F_1 = 2 \cdot \frac{0.30 \times 0.90}{0.30 + 0.90} = 2 \cdot \frac{0.27}{1.20} = \frac{0.54}{1.20} = \mathbf{0.450}$$
  
  ```
                Arithmetic Mean vs. Harmonic Mean Comparison (Slide 6)
      Score
        ▲
    1.0 │                                             0.80  0.80
        │                         0.55                ┌───┐ ┌───┐
    0.8 │     0.60                ┌───┐               │   │ │   │
        │     ┌───┐               │   │               │   │ │   │
    0.6 │     │   │   0.45        │   │               │   │ │   │
        │     │   │   ┌───┐       │   │               │   │ │   │
    0.4 │     │   │   │   │       │   │   0.18        │   │ │   │
        │     │   │   │   │       │   │   ┌───┐       │   │ │   │
    0.2 │     │   │   │   │       │   │   │   │       │   │ │   │
        │     │   │   │   │       │   │   │   │       │   │ │   │
    0.0 ┴─────┴───┴───┴───┴───────┴───┴───┴───┴───────┴───┴─┴───┴──▶ Model Type
             Model B              Flags 1 Sure Case      Balanced Model
          (P=0.30, R=0.90)        (P=1.00, R=0.10)      (P=0.80, R=0.80)
  
         [Light Bar] = Arithmetic Mean: (P + R) / 2
         [Dark Bar]  = Harmonic Mean (F1): 2PR / (P + R)
  ```
  
  Slide 6 emphasizes two structural properties:
  1. **Ignores True Negatives ($TN$):** Because $TN$ does not appear anywhere in the formula, a massive negative class cannot artificially inflate the metric.
  2. **The Penalty on Asymmetry:** The weaker of the two metrics heavily drags $F_1$ downward.
  
  ---
### Why the Arithmetic Mean Fails on Rates
  Why can't we simply average Precision and Recall using the standard arithmetic mean $\frac{P + R}{2}$?
  
  Consider the pathological model in Slide 6 (**"Flags 1 Sure Case"**):
  * In a dataset with $100$ fraud cases, the model flags exactly **$1$ transaction** that it is $100\%$ certain is fraud ($TP = 1, FP = 0$).
  * It ignores the remaining $99$ fraud cases ($FN = 99$).
  * Evaluating the rates:
  $$\text{Precision} = \frac{1}{1 + 0} = \mathbf{1.000 \quad (100\%)}, \quad \text{Recall} = \frac{1}{1 + 99} = \mathbf{0.010 \quad (1\%) }$$
  
  ```
  Evaluating the Means:
  • Arithmetic Mean: (1.000 + 0.010) / 2 = 1.010 / 2 = 0.505 (Looks decent!)
  • Harmonic Mean (F1): 2 · (1.000 · 0.010) / (1.000 + 0.010) = 0.020 / 1.010 = 0.0198 ≈ 0.020
  ```
  
  The arithmetic mean rewards a model that missed $99\%$ of fraud cases with an acceptable passing score of $0.505$. The harmonic mean severely penalizes the near-zero recall, dropping the score to **$0.020$** and exposing the model's total failure.
  
  ---
### Mathematical Definition of the Harmonic Mean
  The harmonic mean of two non-zero positive numbers $x_1$ and $x_2$ is the **reciprocal of the arithmetic mean of their reciprocals**:
  
  $$H = \frac{2}{\frac{1}{x_1} + \frac{1}{x_2}} = \frac{2}{\frac{x_2 + x_1}{x_1 x_2}} = \mathbf{\frac{2 x_1 x_2}{x_1 + x_2}}$$
  
  Because the harmonic mean evaluates the sum of reciprocals ($\frac{1}{P} + \frac{1}{R}$):
  $$\lim_{P \to 0} \frac{1}{P} = \infty \implies \lim_{P \to 0} F_1 = \mathbf{0.0}$$
  $$\lim_{R \to 0} \frac{1}{R} = \infty \implies \lim_{R \to 0} F_1 = \mathbf{0.0}$$
  If *either* Precision or Recall drops to zero, $F_1$ drops strictly to zero. A high $F_1$-score is achievable **if and only if both precision and recall are simultaneously high**.
  
  ---
## 3. The Generalized $F_\beta$-Score (Slide 6)
  
  Slide 6 notes:
  $$\mathbf{\text{"The weaker of the two drags } F_1 \text{ down. Need recall to count more? Use } F_\beta \text{ with } \beta > 1\text{."}}$$
  
  In real-world systems, false alarms ($FP$) and missed detections ($FN$) rarely carry identical operational costs. The **$F_\beta$-score** generalizes $F_1$ by introducing a positive weighting coefficient $\beta \in (0, \infty)$:
  
  $$\mathbf{F_\beta = (1 + \beta^2) \frac{\text{Precision} \cdot \text{Recall}}{\beta^2 \text{Precision} + \text{Recall}}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Role of the β Hyperparameter:                                      │
  │                                                                        │
  │ • β measures how many times more important RECALL is than PRECISION.   │
  │                                                                        │
  │ • β = 1.0  ──▶  F₁-Score (Balanced Weighting):                         │
  │   Equal emphasis on false alarms (FP) and misses (FN).                 │
  │                                                                        │
  │ • β = 2.0  ──▶  F₂-Score (Recall-Biased / Safety First):               │
  │   Weights Recall twice as heavily as Precision.                        │
  │   Domains: Cancer screening, sepsis detection, airport security.      │
  │                                                                        │
  │ • β = 0.5  ──▶  F₀.₅-Score (Precision-Biased / Purity First):           │
  │   Weights Precision twice as heavily as Recall.                        │
  │   Domains: Spam filtering, search engines, automated customer emails.  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Cost-Sensitive Optimization: Ask Which Mistake Costs More (Slide 7)
  
  Slide 7 presents the core engineering heuristic that governs metric selection:
  
  $$\mathbf{\text{"Name the costlier error first; the metric follows."}}$$
  
  ```
                      Domain-to-Metric Mapping (Slide 7)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Cancer Screening                                                    │
  │    A miss (FN) is far worse than a follow-up test                      │
  │    ──▶ RECALL / SENSITIVITY                                            │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 2. Spam Filter                                                         │
  │    A real email in the spam folder (FP) is far worse                   │
  │    ──▶ PRECISION                                                       │
  ├────────────────────────────────────────────────────────────────────────┤
  │ 3. Fraud Alerts with a Small Review Team                               │
  │    Positives are rare and both errors carry concrete costs             │
  │    ──▶ F₁-SCORE (or cost-weighted utility)                             │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Mathematical Formalization: The Operational Cost Matrix
  Rather than using arbitrary heuristic metrics, production machine learning systems assign explicit economic or clinical dollar penalties to each outcome:
  
  $$\mathbf{\text{Expected Operational Cost} = C_{\text{FP}} \cdot FP + C_{\text{FN}} \cdot FN}$$
  
  where:
  * $C_{\text{FP}}$ is the unit cost of a **False Alarm** (e.g., salary paid to an investigator to review a false alert; cost of an unnecessary secondary biopsy).
  * $C_{\text{FN}}$ is the unit cost of a **Missed Event** (e.g., financial loss from an unrecovered fraudulent wire transfer; malpractice liability and patient mortality).
  * We assume correct classifications carry zero cost ($C_{\text{TP}} = C_{\text{TN}} = 0$).
  
  ---
### Deriving the Bayes Optimal Decision Threshold ($\tau^*$)
  Given a calibrated probabilistic model that outputs posterior risk $\hat{p} = P(Y = 1 \mid x)$:
  * If we predict **Positive ($\hat{Y} = 1$)**, we risk incurring a False Positive cost if the true state is negative:
  $$\mathbb{E}[\text{Cost} \mid \hat{Y} = 1] = C_{\text{FP}} \cdot P(Y = 0 \mid x) = C_{\text{FP}} \cdot (1 - \hat{p})$$
  * If we predict **Negative ($\hat{Y} = 0$)**, we risk incurring a False Negative cost if the true state is positive:
  $$\mathbb{E}[\text{Cost} \mid \hat{Y} = 0] = C_{\text{FN}} \cdot P(Y = 1 \mid x) = C_{\text{FN}} \cdot \hat{p}$$
  
  By Bayes Decision Theory, the optimal decision rule chooses $\hat{Y} = 1$ if and only if the expected cost of predicting positive is less than the expected cost of predicting negative:
  
  $$C_{\text{FP}} \cdot (1 - \hat{p}) \le C_{\text{FN}} \cdot \hat{p}$$
  $$C_{\text{FP}} - C_{\text{FP}} \hat{p} \le C_{\text{FN}} \hat{p}$$
  $$C_{\text{FP}} \le (C_{\text{FP}} + C_{\text{FN}}) \hat{p}$$
  
  $$\mathbf{\hat{p} \ge \frac{C_{\text{FP}}}{C_{\text{FP}} + C_{\text{FN}}}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Bayes Optimal Threshold Theorem:                                   │
  │                                                                        │
  │                      τ* = C_FP / (C_FP + C_FN)                         │
  │                                                                        │
  │ • Symmetric Costs (C_FP = C_FN = $100):                                │
  │   τ* = 100 / (100 + 100) = 0.50 (Standard textbook threshold)         │
  │                                                                        │
  │ • Fatal Misses (C_FN = $100,000, C_FP = $1,000):                       │
  │   τ* = 1,000 / (1,000 + 100,000) ≈ 0.0099 ──▶ Set threshold to 1.0%!  │
  │   Flag patient if risk exceeds 1% to drive Recall to 100%.             │
  │                                                                        │
  │ • Catastrophic False Alarms (C_FP = $10,000, C_FN = $100):             │
  │   τ* = 10,000 / (10,000 + 100) ≈ 0.990 ──▶ Set threshold to 99.0%!    │
  │   Filter email only if model is 99% confident to protect Precision.    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## Complete Lecture 15 Synthesis Reference
  
  ```
  ┌─────────────────┬──────────────────────────┬───────────────────────┬────────────────────────────────────────┐
  │ Metric          │ Mathematical Formula     │ Confusion Matrix Axis │ Core Domain Question Answered          │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ Accuracy        │ (TP + TN) / N            │ Diagonal Trace / Sum  │ "Overall, what fraction was correct?"  │
  │                 │                          │                       │ (Unreliable on imbalanced datasets).   │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ Precision (PPV) │ TP / (TP + FP)           │ Column 1 Marginal     │ "Of what I flagged, how much was real?"│
  │                 │                          │ (Conditioned on Ŷ=1)  │ (Controls false alarms & fatigue).     │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ Recall / Sens.  │ TP / (TP + FN)           │ Row 1 Marginal        │ "Of what was real, how much did I find?│
  │ (TPR)           │                          │ (Conditioned on Y=1)  │ (Controls misses in safety screening). │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ Specificity     │ TN / (TN + FP)           │ Row 2 Marginal        │ "Of the negatives, how many cleared?"  │
  │ (TNR)           │                          │ (Conditioned on Y=0)  │ (Controls inlier non-interference).    │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ False Positive  │ FP / (TN + FP)           │ Row 2 Marginal        │ "What fraction of healthy samples were │
  │ Rate (FPR)      │ = 1 - Specificity        │ (Complement of TNR)   │ incorrectly flagged as positive?"      │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ F₁-Score        │ 2PR / (P + R)            │ Combined (Ignores TN) │ "What is the balanced harmonic score   │
  │                 │ = 2TP / (2TP + FP + FN)  │                       │ between alert purity and coverage?"    │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ F_β Score       │ (1+β²)PR / (β²P + R)     │ Weighted Combined     │ Allows tuning relative value of Recall │
  │                 │                          │                       │ (β > 1) vs. Precision (β < 1).         │
  ├─────────────────┼──────────────────────────┼───────────────────────┼────────────────────────────────────────┤
  │ Optimal Cutoff  │ τ* = C_FP / (C_FP + C_FN)│ Decision Boundary     │ Translates economic/clinical cost      │
  │                 │                          │ Shift                 │ directly into an operating threshold.  │
  └─────────────────┴──────────────────────────┴───────────────────────┴────────────────────────────────────────┘
  ```
  
  ---
## Summary Review Questions for Section 3
  
  1. *A classification model outputs $\text{Precision} = 0.90$ and $\text{Recall} = 0.10$. Calculate both the arithmetic mean and the harmonic mean ($F_1$-score). Why is the harmonic mean a much more honest assessment of the model's utility?*
  2. *If an automated quality control camera on a high-speed assembly line incurs an operational cost of $\$10$ to manually inspect a false alarm ($C_{\text{FP}} = \$10$) and a cost of $\$90$ if a defective part ships to a customer ($C_{\text{FN}} = \$90$), what is the Bayes optimal probability threshold $\tau^*$ for flagging defects?*
  3. *Why does the $F_1$-score completely ignore the count of True Negatives ($TN$), and how does this property protect practitioners when evaluating models trained on rare events (such as credit card fraud or rare diseases)?*
  
  ---