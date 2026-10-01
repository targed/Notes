#### Problem 1 (Credit Card Fraud Detection)
  A fraud detection system evaluates $1{,}000$ credit card transactions, $40$ of which are fraudulent (positive). The model flags $50$ transactions as fraud; $30$ of those flagged are truly fraudulent.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 2 (Oncology Biopsy Screening)
  A clinical screening program evaluates $2{,}000$ patient mammograms, $80$ of which have malignant tumors (positive). The automated vision pipeline flags $120$ mammograms as suspicious (positive); $60$ of those flagged are confirmed malignant.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 3 (Industrial Manufacturing Quality Control)
  A semiconductor plant inspects $800$ silicon wafers. Exactly $700$ are defect-free (negative) and $100$ are defective (positive). The automated optical inspection system clears $680$ wafers as defect-free; $650$ of those cleared are truly defect-free.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 4 (Cybersecurity Intrusion Detection)
  A network firewall monitors $10{,}000$ incoming packets, $200$ of which are malicious exploit payloads (positive). The intrusion detection filter successfully detects $180$ of the attacks, but it also raises false alarms on $300$ benign packets.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 5 (Subscription Customer Churn)
  A streaming service analyzes $1{,}200$ subscribers, $300$ of whom will cancel their accounts next month (positive). A churn prediction algorithm correctly identifies $240$ churners, while failing to flag $60$ churners. It also incorrectly flags $160$ loyal subscribers as churners.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 6 (Airport Security Checkpoint)
  An airport baggage scanner screens $5{,}000$ passenger bags, $50$ of which contain prohibited items (positive). The scanner flags $250$ bags for manual inspection; exactly $45$ of those flagged contain prohibited items.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 7 (Emergency Department Sepsis Alert)
  An early-warning clinical monitor screens $600$ ICU patients, $60$ of whom develop sepsis (positive). The monitoring system triggers an audible alert for $150$ patients; among those alerted, $45$ genuinely developed sepsis.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 8 (Search Engine Document Retrieval)
  An information retrieval engine indexes $10{,}000$ documents, $400$ of which are relevant to a legal discovery query (positive). The search algorithm retrieves $500$ documents; $350$ of the retrieved documents are relevant.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 9 (Rare Viral Pathogen Screening)
  A rapid antigen screening test is administered to $2{,}500$ clinic patients, only $25$ of whom are infected with a rare respiratory virus (positive). The test correctly identifies $20$ infected patients, but issues false positive alerts for $48$ healthy patients.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 10 (Bank Loan Default Prediction from Proportions)
  A retail bank evaluates $1{,}500$ mortgage applicants, $150$ of whom will default on their loan (positive). An automated risk model flags $200$ applicants as high-risk defaults. The model achieves an **$80\%$ Recall** on the defaulting applicants.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C3
### Problem 1 Solution (Credit Card Fraud Detection)
  * **Count Derivation:**
  * Total $N = 1{,}000$; Actual Positives $P = 40$; Actual Negatives $N_{\text{neg}} = 1{,}000 - 40 = 960$.
  * Predicted Positives $\hat{P} = 50$; True Positives $TP = 30$.
  * $FP = \hat{P} - TP = 50 - 30 = \mathbf{20}$
  * $FN = P - TP = 40 - 30 = \mathbf{10}$
  * $TN = N_{\text{neg}} - FP = 960 - 20 = \mathbf{940}$
  * *Check:* $30 + 20 + 10 + 940 = 1{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{30 + 940}{1{,}000} = \frac{970}{1{,}000} = \mathbf{0.970}$
  * **Precision:** $\frac{30}{30 + 20} = \frac{30}{50} = \mathbf{0.600}$
  * **Recall:** $\frac{30}{30 + 10} = \frac{30}{40} = \mathbf{0.750}$
  * **Specificity:** $\frac{940}{940 + 20} = \frac{940}{960} \approx \mathbf{0.979}$
  * **$F_1$:** $2 \cdot \frac{0.600 \times 0.750}{0.600 + 0.750} = \frac{0.900}{1.350} = \mathbf{0.667}$
  
  ---
### Problem 2 Solution (Oncology Biopsy Screening)
  * **Count Derivation:**
  * Total $N = 2{,}000$; Actual Positives $P = 80$; Actual Negatives $N_{\text{neg}} = 2{,}000 - 80 = 1{,}920$.
  * Predicted Positives $\hat{P} = 120$; True Positives $TP = 60$.
  * $FP = 120 - 60 = \mathbf{60}$
  * $FN = 80 - 60 = \mathbf{20}$
  * $TN = 1{,}920 - 60 = \mathbf{1{,}860}$
  * *Check:* $60 + 60 + 20 + 1{,}860 = 2{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{60 + 1{,}860}{2{,}000} = \frac{1{,}920}{2{,}000} = \mathbf{0.960}$
  * **Precision:** $\frac{60}{60 + 60} = \frac{60}{120} = \mathbf{0.500}$
  * **Recall:** $\frac{60}{60 + 20} = \frac{60}{80} = \mathbf{0.750}$
  * **Specificity:** $\frac{1{,}860}{1{,}860 + 60} = \frac{1{,}860}{1{,}920} \approx \mathbf{0.969}$
  * **$F_1$:** $2 \cdot \frac{0.500 \times 0.750}{0.500 + 0.750} = \frac{0.750}{1.250} = \mathbf{0.600}$
  
  ---
### Problem 3 Solution (Industrial Quality Control)
  * **Count Derivation:**
  * Total $N = 800$; Actual Defective (Pos) $P = 100$; Actual Good (Neg) $N_{\text{neg}} = 700$.
  * System clears $680$ as good $\implies$ Predicted Negatives $\hat{N} = 680$.
  * Truly good among cleared $\implies True Negatives } TN = \mathbf{650}$.
  * $FN = \hat{N} - TN = 680 - 650 = \mathbf{30}$ (cleared, but defective!).
  * $FP = N_{\text{neg}} - TN = 700 - 650 = \mathbf{50}$ (flagged, but good).
  * $TP = P - FN = 100 - 30 = \mathbf{70}$.
  * *Check:* $70 + 50 + 30 + 650 = 800$.
  * **Metrics:**
  * **Accuracy:** $\frac{70 + 650}{800} = \frac{720}{800} = \mathbf{0.900}$
  * **Precision:** $\frac{70}{70 + 50} = \frac{70}{120} \approx \mathbf{0.583}$
  * **Recall:** $\frac{70}{70 + 30} = \frac{70}{100} = \mathbf{0.700}$
  * **Specificity:** $\frac{650}{650 + 50} = \frac{650}{700} \approx \mathbf{0.929}$
  * **$F_1$:** $2 \cdot \frac{0.5833 \times 0.700}{0.5833 + 0.700} = \frac{0.8167}{1.2833} \approx \mathbf{0.636}$
  
  ---
### Problem 4 Solution (Cybersecurity Intrusion Detection)
  * **Count Derivation:**
  * Total $N = 10{,}000$; Actual Attacks $P = 200$; Actual Benign $N_{\text{neg}} = 9{,}800$.
  * Detects $180$ attacks $\implies TP = \mathbf{180}$.
  * $FN = P - TP = 200 - 180 = \mathbf{20}$.
  * False alarms on benign $\implies FP = \mathbf{300}$.
  * $TN = N_{\text{neg}} - FP = 9{,}800 - 300 = \mathbf{9{,}500}$.
  * *Check:* $180 + 300 + 20 + 9{,}500 = 10{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{180 + 9{,}500}{10{,}000} = \frac{9{,}680}{10{,}000} = \mathbf{0.968}$
  * **Precision:** $\frac{180}{180 + 300} = \frac{180}{480} = \mathbf{0.375}$
  * **Recall:** $\frac{180}{180 + 20} = \frac{180}{200} = \mathbf{0.900}$
  * **Specificity:** $\frac{9{,}500}{9{,}500 + 300} = \frac{9{,}500}{9{,}800} \approx \mathbf{0.969}$
  * **$F_1$:** $2 \cdot \frac{0.375 \times 0.900}{0.375 + 0.900} = \frac{0.675}{1.275} \approx \mathbf{0.529}$
  
  ---
### Problem 5 Solution (Subscription Customer Churn)
  * **Count Derivation:**
  * Total $N = 1{,}200$; Actual Churn $P = 300$; Actual Loyal $N_{\text{neg}} = 900$.
  * Correctly identifies $240$ churners $\implies TP = \mathbf{240}$.
  * Fails to flag $60$ churners $\implies FN = \mathbf{60}$.
  * Incorrectly flags $160$ loyal $\implies FP = \mathbf{160}$.
  * $TN = N_{\text{neg}} - FP = 900 - 160 = \mathbf{740}$.
  * *Check:* $240 + 160 + 60 + 740 = 1{,}200$.
  * **Metrics:**
  * **Accuracy:** $\frac{240 + 740}{1{,}200} = \frac{980}{1{,}200} \approx \mathbf{0.817}$
  * **Precision:** $\frac{240}{240 + 160} = \frac{240}{400} = \mathbf{0.600}$
  * **Recall:** $\frac{240}{240 + 60} = \frac{240}{300} = \mathbf{0.800}$
  * **Specificity:** $\frac{740}{740 + 160} = \frac{740}{900} \approx \mathbf{0.822}$
  * **$F_1$:** $2 \cdot \frac{0.600 \times 0.800}{0.600 + 0.800} = \frac{0.960}{1.400} \approx \mathbf{0.686}$
  
  ---
### Problem 6 Solution (Airport Security Checkpoint)
  * **Count Derivation:**
  * Total $N = 5{,}000$; Actual Contraband $P = 50$; Actual Safe $N_{\text{neg}} = 4{,}950$.
  * Flags $250$ bags $\implies \hat{P} = 250$; Contraband among flagged $\implies TP = \mathbf{45}$.
  * $FP = \hat{P} - TP = 250 - 45 = \mathbf{205}$.
  * $FN = P - TP = 50 - 45 = \mathbf{5}$.
  * $TN = N_{\text{neg}} - FP = 4{,}950 - 205 = \mathbf{4{,}745}$.
  * *Check:* $45 + 205 + 5 + 4{,}745 = 5{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{45 + 4{,}745}{5{,}000} = \frac{4{,}790}{5{,}000} = \mathbf{0.958}$
  * **Precision:** $\frac{45}{45 + 205} = \frac{45}{250} = \mathbf{0.180}$
  * **Recall:** $\frac{45}{45 + 5} = \frac{45}{50} = \mathbf{0.900}$
  * **Specificity:** $\frac{4{,}745}{4{,}745 + 205} = \frac{4{,}745}{4{,}950} \approx \mathbf{0.959}$
  * **$F_1$:** $2 \cdot \frac{0.180 \times 0.900}{0.180 + 0.900} = \frac{0.324}{1.080} = \mathbf{0.300}$
  
  ---
### Problem 7 Solution (Emergency Department Sepsis Alert)
  * **Count Derivation:**
  * Total $N = 600$; Actual Sepsis $P = 60$; Actual Non-Sepsis $N_{\text{neg}} = 540$.
  * Alerts on $150$ patients $\implies \hat{P} = 150$; Sepsis among alerted $\implies TP = \mathbf{45}$.
  * $FP = 150 - 45 = \mathbf{105}$.
  * $FN = P - TP = 60 - 45 = \mathbf{15}$.
  * $TN = N_{\text{neg}} - FP = 540 - 105 = \mathbf{435}$.
  * *Check:* $45 + 105 + 15 + 435 = 600$.
  * **Metrics:**
  * **Accuracy:** $\frac{45 + 435}{600} = \frac{480}{600} = \mathbf{0.800}$
  * **Precision:** $\frac{45}{45 + 105} = \frac{45}{150} = \mathbf{0.300}$
  * **Recall:** $\frac{45}{45 + 15} = \frac{45}{60} = \mathbf{0.750}$
  * **Specificity:** $\frac{435}{435 + 105} = \frac{435}{540} \approx \mathbf{0.806}$
  * **$F_1$:** $2 \cdot \frac{0.300 \times 0.750}{0.300 + 0.750} = \frac{0.450}{1.050} \approx \mathbf{0.429}$
  
  ---
### Problem 8 Solution (Search Engine Document Retrieval)
  * **Count Derivation:**
  * Total $N = 10{,}000$; Relevant $P = 400$; Irrelevant $N_{\text{neg}} = 9{,}600$.
  * Retrieves $500 \implies \hat{P} = 500$; Relevant retrieved $\implies TP = \mathbf{350}$.
  * $FP = 500 - 350 = \mathbf{150}$.
  * $FN = P - TP = 400 - 350 = \mathbf{50}$.
  * $TN = N_{\text{neg}} - FP = 9{,}600 - 150 = \mathbf{9{,}450}$.
  * *Check:* $350 + 150 + 50 + 9{,}450 = 10{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{350 + 9{,}450}{10{,}000} = \frac{9{,}800}{10{,}000} = \mathbf{0.980}$
  * **Precision:** $\frac{350}{350 + 150} = \frac{350}{500} = \mathbf{0.700}$
  * **Recall:** $\frac{350}{350 + 50} = \frac{350}{400} = \mathbf{0.875}$
  * **Specificity:** $\frac{9{,}450}{9{,}450 + 150} = \frac{9{,}450}{9{,}600} \approx \mathbf{0.984}$
  * **$F_1$:** $2 \cdot \frac{0.700 \times 0.875}{0.700 + 0.875} = \frac{1.225}{1.575} \approx \mathbf{0.778}$
  
  ---
### Problem 9 Solution (Rare Viral Pathogen Screening)
  * **Count Derivation:**
  * Total $N = 2{,}500$; Actual Infected $P = 25$; Actual Healthy $N_{\text{neg}} = 2{,}475$.
  * Correctly identifies infected $\implies TP = \mathbf{20}$.
  * $FN = P - TP = 25 - 20 = \mathbf{5}$.
  * Positive on healthy $\implies FP = \mathbf{48}$.
  * $TN = N_{\text{neg}} - FP = 2{,}475 - 48 = \mathbf{2{,}427}$.
  * *Check:* $20 + 48 + 5 + 2{,}427 = 2{,}500$.
  * **Metrics:**
  * **Accuracy:** $\frac{20 + 2{,}427}{2{,}500} = \frac{2{,}447}{2{,}500} \approx \mathbf{0.979}$
  * **Precision:** $\frac{20}{20 + 48} = \frac{20}{68} \approx \mathbf{0.294}$
  * **Recall:** $\frac{20}{20 + 5} = \frac{20}{25} = \mathbf{0.800}$
  * **Specificity:** $\frac{2{,}427}{2{,}427 + 48} = \frac{2{,}427}{2{,}475} \approx \mathbf{0.981}$
  * **$F_1$:** $2 \cdot \frac{0.2941 \times 0.800}{0.2941 + 0.800} = \frac{0.4706}{1.0941} \approx \mathbf{0.430}$
  
  ---
### Problem 10 Solution (Bank Loan Default Prediction)
  * **Count Derivation:**
  * Total $N = 1{,}500$; Actual Defaults $P = 150$; Actual Non-Defaults $N_{\text{neg}} = 1{,}350$.
  * Flags $200$ applicants $\implies \hat{P} = 200$.
  * $80\%$ Recall on Defaulters $\implies TP = 0.80 \times 150 = \mathbf{120}$.
  * $FN = P - TP = 150 - 120 = \mathbf{30}$.
  * $FP = \hat{P} - TP = 200 - 120 = \mathbf{80}$.
  * $TN = N_{\text{neg}} - FP = 1{,}350 - 80 = \mathbf{1{,}270}$.
  * *Check:* $120 + 80 + 30 + 1{,}270 = 1{,}500$.
  * **Metrics:**
  * **Accuracy:** $\frac{120 + 1{,}270}{1{,}500} = \frac{1{,}390}{1{,}500} \approx \mathbf{0.927}$
  * **Precision:** $\frac{120}{120 + 80} = \frac{120}{200} = \mathbf{0.600}$
  * **Recall:** $\frac{120}{120 + 30} = \frac{120}{150} = \mathbf{0.800}$
  * **Specificity:** $\frac{1{,}270}{1{,}270 + 80} = \frac{1{,}270}{1{,}350} \approx \mathbf{0.941}$
  * **$F_1$:** $2 \cdot \frac{0.600 \times 0.800}{0.600 + 0.800} = \frac{0.960}{1.400} \approx \mathbf{0.686}$
  
  ---