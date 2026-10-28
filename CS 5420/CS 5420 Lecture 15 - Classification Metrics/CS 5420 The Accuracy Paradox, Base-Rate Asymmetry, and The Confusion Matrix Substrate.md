## 1. The Accuracy Paradox (Slides 2–3)
  
  Slide 2 introduces a diagnostic thought experiment testing the limits of standard classification accuracy:
  
  $$\mathbf{\text{"1,000 card transactions, 10 are fraud. Model A always answers 'legit'."}}$$
  $$\mathbf{\text{"Model B flags 30 transactions and catches 9 of the 10 frauds. Which model has the higher accuracy?"}}$$
  
  ```
                       The Accuracy Paradox Vote (Slide 2)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Candidate Systems:                                                     │
  │                                                                        │
  │ • Model A (Trivial Majority Predictor):                                │
  │   Always predicts "legit" (Negative), completely ignoring the inputs.   │
  │                                                                        │
  │ • Model B (Active Screening Classifier):                               │
  │   Flags 30 suspicious transactions, catching 9 out of 10 fraud events. │
  ├────────────────────────────────────────────────────────────────────────┤
  │ The Intuition Trap:                                                    │
  │ Which model has higher accuracy?                                       │
  │   A: Model A  ✔                                                        │
  │   B: Model B                                                           │
  │   C: They tie                                                          │
  │   D: Can't tell without more information                               │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 3 reveals the counterintuitive result:
  
  $$\mathbf{\text{"A. 99.0\% vs. 97.8\%. Accuracy rewards the model that never looks for fraud."}}$$
  
  ```
                      Deconstructing the Two Models (Slide 3)
         Model A (Trivial Majority Baseline)              Model B (Active Screening Model)
                  pred +       pred -                              pred +       pred -
              ┌────────────┬────────────┐                      ┌────────────┬────────────┐
     actual + │   TP = 0   │  FN = 10   │             actual + │   TP = 9   │   FN = 1   │
              ├────────────┼────────────┤                      ├────────────┼────────────┤
     actual - │   FP = 0   │  TN = 990  │             actual - │  FP = 21   │  TN = 969  │
              └────────────┴────────────┘                      └────────────┴────────────┘
      Accuracy: 990 / 1,000 = 99.0%                    Accuracy: (9 + 969) / 1,000 = 97.8%
      Fraud Caught: 0 / 10 = 0.0%                      Fraud Caught: 9 / 10 = 90.0%
  ```
  
  ---
### A. Mathematical Derivation of the Paradox
  Let $N = N_+ + N_-$ denote the total population size, where $N_+$ is the count of minority positive instances and $N_-$ is the count of majority negative instances.
  * **The Minority Base Rate (Prevalence):**
  $$\pi = P(Y = 1) = \frac{N_+}{N} = \frac{10}{1{,}000} = 0.010 \quad (1.0\%)$$
  * **The Majority Base Rate:**
  $$1 - \pi = P(Y = 0) = \frac{N_-}{N} = \frac{990}{1{,}000} = 0.990 \quad (99.0\%)$$
#### 1. Performance of the Zero-Rule Classifier ($f_{\emptyset}(x) \equiv 0$):
  Model A is an uninformative constant function. It predicts $\hat{y}_i = 0$ for all $x_i$:
  $$\text{Accuracy}(f_{\emptyset}) = \frac{TN}{N} = \frac{N_-}{N} = 1 - \pi = \mathbf{0.990 \quad (99.0\%)}$$
  The model achieves $99\%$ accuracy with zero knowledge of the data, but detects **$0\%$ of actual fraud cases**.
#### 2. Performance of the Active Detector ($f_B$):
  Model B correctly flags $9$ out of $10$ frauds ($TP = 9$), missing only $1$ fraud ($FN = 1$). To achieve this high sensitivity, it raises $21$ false alarms on legitimate accounts ($FP = 21$):
  $$\text{Accuracy}(f_B) = \frac{TP + TN}{N} = \frac{9 + 969}{1{,}000} = \frac{978}{1{,}000} = \mathbf{0.978 \quad (97.8\%)}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Failure of Classification Accuracy:                                │
  │                                                                        │
  │ • Model A has a HIGHER accuracy than Model B (99.0% > 97.8%).          │
  │ • Yet Model A has ZERO utility (it allows 100% of fraud through).      │
  │ • Accuracy measures the fraction of CORRECT PREDICTIONS aggregated     │
  │   over the ENTIRE dataset.                                             │
  │ • Because negative transactions make up 99% of the population, correct │
  │   predictions on the majority class (TN) completely drown out failure  │
  │   on the critical minority class (FN).                                 │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **The Core Law:** Whenever class distributions are imbalanced ($\pi \ll 0.50$), **accuracy is an uncalibrated metric**. It incentivizes models to collapse to the trivial majority baseline.
  
  ---
## 2. The Four Cells of the Confusion Matrix (Slide 4)
  
  Slide 4 presents the foundation of classification performance evaluation:
  
  $$\mathbf{\text{"Every classification metric you will ever use is some arithmetic on these four numbers."}}$$
  $$\mathbf{\text{"Learn the box, not the formulas."}}$$
  
  ```
                       The 2×2 Contingency Matrix (Slide 4)
                                         Predicted Class
                                  Positive (P̂)       Negative (N̂)
                             ┌──────────────────┬──────────────────┐
                Positive (P) │  True Positives  │  False Negatives │ ──▶ Actual Positives
  Actual Class                 │       (TP)       │   (FN / Miss)    │     P = TP + FN
                             ├──────────────────┼──────────────────┤
                Negative (N) │ False Positives  │  True Negatives  │ ──▶ Actual Negatives
                             │  (FP / Alarm)    │       (TN)       │     N = FP + TN
                             └──────────────────┴──────────────────┘
                                      │                  │
                                      ▼                  ▼
                             Predicted Positives Predicted Negatives
                              P̂ = TP + FP        N̂ = FN + TN
  ```
  
  Slide 4 formalizes the error nomenclature:
  * **False Positive ($FP$):** **False Alarm (Type I Error)**. The model predicts positive ($\hat{Y} = 1$), but the true state was negative ($Y = 0$).
  * **False Negative ($FN$):** **Miss (Type II Error)**. The model predicts negative ($\hat{Y} = 0$), but the true state was positive ($Y = 1$).
  * **The Main Diagonal ($TP, TN$):** Correct classifications.
  * **The Off-Diagonal ($FP, FN$):** Classification errors.
  
  ---
## 3. Multiclass Generalization: The $K \times K$ Matrix (Slide 4)
  
  Slide 4 notes:
  $$\mathbf{\text{"}K\text{ classes }\longrightarrow\text{ a } K \times K \text{ matrix; the diagonal is correct."}}$$
  
  When extending beyond binary outcomes to multiclass categorization with $K$ classes ($Y \in \{1, 2, \dots, K\}$), the contingency table generalizes to an **aligned $K \times K$ integer matrix $M$**, where:
  * Row index $i \in \{1, \dots, K\}$ denotes the **True (Actual) Class**.
  * Column index $j \in \{1, \dots, K\}$ denotes the **Predicted Class**.
  * Matrix entry $M_{ij}$ represents the count of instances truly in class $i$ that were assigned by the model to class $j$.
  
  ```
                       The Multi-Class Matrix Architecture
                                        Predicted Class
                                Class 1      Class 2      Class 3    ...   Class K
                             ┌────────────┬────────────┬────────────┬───┬────────────┐
                     Class 1 │   M₁₁ ✔    │    M₁₂     │    M₁₃     │   │    M₁_K    │ ──▶ Total True Class 1
                     Class 2 │    M₂₁     │   M₂₂ ✔    │    M₂₃     │   │    M₂_K    │ ──▶ Total True Class 2
  Actual Class         Class 3 │    M₃₁     │    M₃₂     │   M₃₃ ✔    │   │    M₃_K    │ ──▶ Total True Class 3
                        :    │     :      │     :      │     :      │   │     :      │
                     Class K │    M_K1    │    M_K2    │    M_K3    │   │   M_KK ✔   │ ──▶ Total True Class K
                             └────────────┴────────────┴────────────┴───┴────────────┘
                                  │            │            │                │
                                  ▼            ▼            ▼                ▼
                             Total Pred 1 Total Pred 2 Total Pred 3     Total Pred K
  ```
  
  ---
### Mathematical Deconstruction of Class-Specific Cells:
  For an arbitrary target class $k \in \{1, \dots, K\}$, the four binary cells are extracted directly from the $K \times K$ matrix:
  
  1. **True Positives for Class $k$ ($TP_k$):**
   The diagonal intersection where actual class $k$ is predicted as class $k$:
   $$TP_k = M_{kk}$$
  2. **False Positives for Class $k$ ($FP_k$):**
   Instances belonging to any other class ($i \neq k$) that were incorrectly labeled as class $k$ (the **column sum excluding the diagonal**):
   $$FP_k = \sum_{i \neq k} M_{ik}$$
  3. **False Negatives for Class $k$ ($FN_k$):**
   Instances truly belonging to class $k$ that were incorrectly routed to another class ($j \neq k$) (the **row sum excluding the diagonal**):
   $$FN_k = \sum_{j \neq k} M_{kj}$$
  4. **True Negatives for Class $k$ ($TN_k$):**
   Instances that neither belong to class $k$ nor were predicted as class $k$ (the sum of all entries **outside row $k$ and column $k$**):
   $$TN_k = \sum_{i \neq k} \sum_{j \neq k} M_{ij}$$
  
  ---
### Overall Multiclass Accuracy:
  Global accuracy collapses the entire $K \times K$ matrix into the ratio of the diagonal trace to the total population sum:
  
  $$\mathbf{\text{Accuracy} = \frac{\text{Tr}(M)}{N_{\text{total}}} = \frac{\sum_{k=1}^K M_{kk}}{\sum_{i=1}^K \sum_{j=1}^K M_{ij}}}$$
  
  * **Diagnostic Value of the Off-Diagonal Manifold:**
  Looking at off-diagonal cells reveals systematic patterns of confusion between classes. For example, if $M_{\text{cat, dog}} = 45$, the model struggles to differentiate cats from dogs. If $M_{\text{cat, airplane}} = 0$, the model cleanly separates biological animals from mechanical vehicles.
  
  ---
## Summary Review Questions for Section 1
  
  1. *In a dataset of $10{,}000$ transactions where only $20$ are fraudulent, what is the accuracy of a model that predicts "legitimate" for every transaction? Why does this model provide zero operational utility?*
  2. *In a binary classification problem, what is the fundamental conceptual difference between a Type I Error (False Positive) and a Type II Error (False Negative)? Give a real-world example where Type II is far worse than Type I.*
  3. *Given a $3 \times 3$ multiclass confusion matrix $M$ where rows represent true labels and columns represent predicted labels, write the mathematical expression for the False Negatives of Class 2 ($FN_2$).*
  
  ---