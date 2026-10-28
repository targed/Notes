#### Problem 1 (Credit Card Fraud Detection)
  A bank evaluates $2{,}000$ credit card transactions, $50$ of which are fraudulent (positive). An automated anomaly detector flags $80$ transactions as suspicious; $40$ of those flagged are truly fraudulent.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 2 (Clinical Rare Disease Screening)
  A clinical screening program evaluates $1{,}500$ patients, $60$ of whom have a rare autoimmune disorder (positive). The screening test flags $150$ patients as positive; $45$ of those flagged truly have the disorder.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 3 (Industrial Manufacturing Quality Control)
  A factory inspects $500$ structural components, $40$ of which possess hairline weld cracks (positive). An ultrasonic sensor flags $60$ components as cracked; $36$ of those flagged truly have cracks.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 4 (Cybersecurity Malware Detection)
  A network firewall evaluates $10{,}000$ executable files, $120$ of which contain ransomware payloads (positive). The antivirus scanner flags $200$ files as suspicious; $90$ of those flagged are confirmed ransomware.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 5 (Subscription Customer Churn)
  A telecom provider analyzes $800$ subscribers, $160$ of whom will cancel their service next month (positive). A churn prediction model flags $120$ accounts as at risk of cancellation; $96$ of those flagged actually cancel.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 6 (Airport Baggage Contraband Screening)
  An airport baggage scanner screens $4{,}000$ checked bags, $80$ of which contain undeclared hazardous items (positive). The scanner flags $160$ bags for physical inspection; $64$ of those flagged truly contain hazardous items.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 7 (Emergency Department Sepsis Alert)
  An early-warning monitor tracks $600$ ICU patients, $90$ of whom develop sepsis (positive). The monitor triggers a bedside alert for $120$ patients; $72$ of those alerted truly develop sepsis.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 8 (Search Engine Document Relevance)
  A legal discovery search engine indexes $1{,}200$ documents, $150$ of which are relevant to a subpoena (positive). The semantic search query retrieves $180$ documents; $135$ of the retrieved documents are truly relevant.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 9 (University Dropout Early Intervention)
  A university tracks $2{,}500$ incoming freshmen, $250$ of whom are at risk of dropping out before sophomore year (positive). An advisor triage system flags $300$ students as high-risk; $200$ of those flagged were truly at risk.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
#### Problem 10 (E-Commerce Return Fraud)
  An online retailer evaluates $5{,}000$ return claims, $100$ of which are abusive return fraud (positive). A risk model flags $125$ claims; $75$ of those flagged are confirmed fraud.  
  * **Task:** Determine $TP$, $FP$, $FN$, $TN$, then compute **Accuracy**, **Precision**, **Recall**, **Specificity**, and **$F_1$**.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C1
### Standard Formulas:
  * $\text{Actual Positives } P = TP + FN \quad \implies \quad FN = P - TP$
  * $\text{Actual Negatives } N = TN + FP \quad \implies \quad TN = N - FP$
  * $\text{Predicted Positives } \hat{P} = TP + FP \quad \implies \quad FP = \hat{P} - TP$
  * $\text{Accuracy} = \frac{TP + TN}{P + N}$
  * $\text{Precision} = \frac{TP}{TP + FP}$
  * $\text{Recall (Sensitivity)} = \frac{TP}{TP + FN}$
  * $\text{Specificity} = \frac{TN}{TN + FP}$
  * $F_1 = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}} = \frac{2TP}{2TP + FP + FN}$
  
  ---
### Problem 1 Solution (Credit Card Fraud Detection)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 2{,}000$; Actual Fraud $P = 50$; Actual Legit $N = 2{,}000 - 50 = 1{,}950$.
  * Predicted Fraud $\hat{P} = 80$; Truly Fraudulent $TP = \mathbf{40}$.
  * $FP = \hat{P} - TP = 80 - 40 = \mathbf{40}$
  * $FN = P - TP = 50 - 40 = \mathbf{10}$
  * $TN = N - FP = 1{,}950 - 40 = \mathbf{1{,}910}$
  * *Check:* $40 + 40 + 10 + 1{,}910 = 2{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{40 + 1{,}910}{2{,}000} = \frac{1{,}950}{2{,}000} = \mathbf{0.98} \text{ (or } 0.975\text{)}$
  * **Precision:** $\frac{40}{40 + 40} = \frac{40}{80} = \mathbf{0.50}$
  * **Recall:** $\frac{40}{40 + 10} = \frac{40}{50} = \mathbf{0.80}$
  * **Specificity:** $\frac{1{,}910}{1{,}910 + 40} = \frac{1{,}910}{1{,}950} \approx \mathbf{0.98} \text{ (or } 0.979\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.50 \times 0.80}{0.50 + 0.80} = \frac{0.80}{1.30} = \frac{8}{13} \approx \mathbf{0.62} \text{ (or } 0.615\text{)}$
  
  ---
### Problem 2 Solution (Clinical Rare Disease Screening)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 1{,}500$; Actual Disease $P = 60$; Actual Healthy $N = 1{,}500 - 60 = 1{,}440$.
  * Predicted Positive $\hat{P} = 150$; Truly Diseased $TP = \mathbf{45}$.
  * $FP = 150 - 45 = \mathbf{105}$
  * $FN = 60 - 45 = \mathbf{15}$
  * $TN = 1{,}440 - 105 = \mathbf{1{,}335}$
  * *Check:* $45 + 105 + 15 + 1{,}335 = 1{,}500$.
  * **Metrics:**
  * **Accuracy:** $\frac{45 + 1{,}335}{1{,}500} = \frac{1{,}380}{1{,}500} = \mathbf{0.92}$
  * **Precision:** $\frac{45}{45 + 105} = \frac{45}{150} = \mathbf{0.30}$
  * **Recall:** $\frac{45}{45 + 15} = \frac{45}{60} = \mathbf{0.75}$
  * **Specificity:** $\frac{1{,}335}{1{,}335 + 105} = \frac{1{,}335}{1{,}440} \approx \mathbf{0.93} \text{ (or } 0.927\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.30 \times 0.75}{0.30 + 0.75} = \frac{0.45}{1.05} = \frac{3}{7} \approx \mathbf{0.43} \text{ (or } 0.429\text{)}$
  
  ---
### Problem 3 Solution (Industrial Quality Control)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 500$; Actual Cracked $P = 40$; Actual Good $N = 500 - 40 = 460$.
  * Predicted Cracked $\hat{P} = 60$; Truly Cracked $TP = \mathbf{36}$.
  * $FP = 60 - 36 = \mathbf{24}$
  * $FN = 40 - 36 = \mathbf{4}$
  * $TN = 460 - 24 = \mathbf{436}$
  * *Check:* $36 + 24 + 4 + 436 = 500$.
  * **Metrics:**
  * **Accuracy:** $\frac{36 + 436}{500} = \frac{472}{500} = \mathbf{0.94} \text{ (or } 0.944\text{)}$
  * **Precision:** $\frac{36}{36 + 24} = \frac{36}{60} = \mathbf{0.60}$
  * **Recall:** $\frac{36}{36 + 4} = \frac{36}{40} = \mathbf{0.90}$
  * **Specificity:** $\frac{436}{436 + 24} = \frac{436}{460} \approx \mathbf{0.95} \text{ (or } 0.948\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.60 \times 0.90}{0.60 + 0.90} = \frac{1.08}{1.50} = \mathbf{0.72}$
  
  ---
### Problem 4 Solution (Cybersecurity Malware Detection)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 10{,}000$; Actual Ransomware $P = 120$; Actual Benign $N = 10{,}000 - 120 = 9{,}880$.
  * Predicted Ransomware $\hat{P} = 200$; Confirmed Ransomware $TP = \mathbf{90}$.
  * $FP = 200 - 90 = \mathbf{110}$
  * $FN = 120 - 90 = \mathbf{30}$
  * $TN = 9{,}880 - 110 = \mathbf{9{,}770}$
  * *Check:* $90 + 110 + 30 + 9{,}770 = 10{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{90 + 9{,}770}{10{,}000} = \frac{9{,}860}{10{,}000} = \mathbf{0.99} \text{ (or } 0.986\text{)}$
  * **Precision:** $\frac{90}{90 + 110} = \frac{90}{200} = \mathbf{0.45}$
  * **Recall:** $\frac{90}{90 + 30} = \frac{90}{120} = \mathbf{0.75}$
  * **Specificity:** $\frac{9{,}770}{9{,}770 + 110} = \frac{9{,}770}{9{,}880} \approx \mathbf{0.99} \text{ (or } 0.989\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.45 \times 0.75}{0.45 + 0.75} = \frac{0.675}{1.20} \approx \mathbf{0.56} \text{ (or } 0.563\text{)}$
  
  ---
### Problem 5 Solution (Subscription Customer Churn)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 800$; Actual Churn $P = 160$; Actual Retained $N = 800 - 160 = 640$.
  * Predicted Churn $\hat{P} = 120$; Confirmed Churn $TP = \mathbf{96}$.
  * $FP = 120 - 96 = \mathbf{24}$
  * $FN = 160 - 96 = \mathbf{64}$
  * $TN = 640 - 24 = \mathbf{616}$
  * *Check:* $96 + 24 + 64 + 616 = 800$.
  * **Metrics:**
  * **Accuracy:** $\frac{96 + 616}{800} = \frac{712}{800} = \mathbf{0.89}$
  * **Precision:** $\frac{96}{96 + 24} = \frac{96}{120} = \mathbf{0.80}$
  * **Recall:** $\frac{96}{96 + 64} = \frac{96}{160} = \mathbf{0.60}$
  * **Specificity:** $\frac{616}{616 + 24} = \frac{616}{640} = \frac{77}{80} \approx \mathbf{0.96} \text{ (or } 0.963\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.80 \times 0.60}{0.80 + 0.60} = \frac{0.96}{1.40} = \frac{24}{35} \approx \mathbf{0.69} \text{ (or } 0.686\text{)}$
  
  ---
### Problem 6 Solution (Airport Baggage Contraband Screening)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 4{,}000$; Actual Hazardous $P = 80$; Actual Safe $N = 4{,}000 - 80 = 3{,}920$.
  * Flagged for Inspection $\hat{P} = 160$; Truly Hazardous $TP = \mathbf{64}$.
  * $FP = 160 - 64 = \mathbf{96}$
  * $FN = 80 - 64 = \mathbf{16}$
  * $TN = 3{,}920 - 96 = \mathbf{3{,}824}$
  * *Check:* $64 + 96 + 16 + 3{,}824 = 4{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{64 + 3{,}824}{4{,}000} = \frac{3{,}888}{4{,}000} = \mathbf{0.97} \text{ (or } 0.972\text{)}$
  * **Precision:** $\frac{64}{64 + 96} = \frac{64}{160} = \mathbf{0.40}$
  * **Recall:** $\frac{64}{64 + 16} = \frac{64}{80} = \mathbf{0.80}$
  * **Specificity:** $\frac{3{,}824}{3{,}824 + 96} = \frac{3{,}824}{3{,}920} \approx \mathbf{0.98} \text{ (or } 0.976\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.40 \times 0.80}{0.40 + 0.80} = \frac{0.64}{1.20} = \frac{8}{15} \approx \mathbf{0.53} \text{ (or } 0.533\text{)}$
  
  ---
### Problem 7 Solution (Emergency Department Sepsis Alert)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 600$; Actual Sepsis $P = 90$; Actual Non-Sepsis $N = 600 - 90 = 510$.
  * Alert Triggered $\hat{P} = 120$; Confirmed Sepsis $TP = \mathbf{72}$.
  * $FP = 120 - 72 = \mathbf{48}$
  * $FN = 90 - 72 = \mathbf{18}$
  * $TN = 510 - 48 = \mathbf{462}$
  * *Check:* $72 + 48 + 18 + 462 = 600$.
  * **Metrics:**
  * **Accuracy:** $\frac{72 + 462}{600} = \frac{534}{600} = \mathbf{0.89}$
  * **Precision:** $\frac{72}{72 + 48} = \frac{72}{120} = \mathbf{0.60}$
  * **Recall:** $\frac{72}{72 + 18} = \frac{72}{90} = \mathbf{0.80}$
  * **Specificity:** $\frac{462}{462 + 48} = \frac{462}{510} \approx \mathbf{0.91} \text{ (or } 0.906\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.60 \times 0.80}{0.60 + 0.80} = \frac{0.96}{1.40} \approx \mathbf{0.69} \text{ (or } 0.686\text{)}$
  
  ---
### Problem 8 Solution (Search Engine Document Relevance)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 1{,}200$; Relevant $P = 150$; Irrelevant $N = 1{,}200 - 150 = 1{,}050$.
  * Retrieved Documents $\hat{P} = 180$; Truly Relevant $TP = \mathbf{135}$.
  * $FP = 180 - 135 = \mathbf{45}$
  * $FN = 150 - 135 = \mathbf{15}$
  * $TN = 1{,}050 - 45 = \mathbf{1{,}005}$
  * *Check:* $135 + 45 + 15 + 1{,}005 = 1{,}200$.
  * **Metrics:**
  * **Accuracy:** $\frac{135 + 1{,}005}{1{,}200} = \frac{1{,}140}{1{,}200} = \mathbf{0.95}$
  * **Precision:** $\frac{135}{135 + 45} = \frac{135}{180} = \mathbf{0.75}$
  * **Recall:** $\frac{135}{135 + 15} = \frac{135}{150} = \mathbf{0.90}$
  * **Specificity:** $\frac{1{,}005}{1{,}005 + 45} = \frac{1{,}005}{1{,}050} \approx \mathbf{0.96} \text{ (or } 0.957\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.75 \times 0.90}{0.75 + 0.90} = \frac{1.35}{1.65} = \frac{9}{11} \approx \mathbf{0.82} \text{ (or } 0.818\text{)}$
  
  ---
### Problem 9 Solution (College Student Dropout Prevention)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 2{,}500$; Actual At-Risk $P = 250$; Not At-Risk $N = 2{,}500 - 250 = 2{,}250$.
  * Flagged by Model $\hat{P} = 300$; Truly At-Risk $TP = \mathbf{200}$.
  * $FP = 300 - 200 = \mathbf{100}$
  * $FN = 250 - 200 = \mathbf{50}$
  * $TN = 2{,}250 - 100 = \mathbf{2{,}150}$
  * *Check:* $200 + 100 + 50 + 2{,}150 = 2{,}500$.
  * **Metrics:**
  * **Accuracy:** $\frac{200 + 2{,}150}{2{,}500} = \frac{2{,}350}{2{,}500} = \mathbf{0.94}$
  * **Precision:** $\frac{200}{200 + 100} = \frac{200}{300} = \frac{2}{3} \approx \mathbf{0.67} \text{ (or } 0.667\text{)}$
  * **Recall:** $\frac{200}{200 + 50} = \frac{200}{250} = \mathbf{0.80}$
  * **Specificity:** $\frac{2{,}150}{2{,}150 + 100} = \frac{2{,}150}{2{,}250} = \frac{43}{45} \approx \mathbf{0.96} \text{ (or } 0.956\text{)}$
  * **$F_1$:** $2 \cdot \frac{(2/3) \times 0.80}{(2/3) + 0.80} = \frac{3.2/3}{2.2/1.5} = \frac{16}{22} = \frac{8}{11} \approx \mathbf{0.73} \text{ (or } 0.727\text{)}$
  
  ---
### Problem 10 Solution (E-Commerce Return Fraud)
  * **Count Derivation:**
  * Total $N_{\text{total}} = 5{,}000$; Actual Fraudulent Returns $P = 100$; Legitimate Returns $N = 5{,}000 - 100 = 4{,}900$.
  * Flagged Claims $\hat{P} = 125$; Confirmed Fraud $TP = \mathbf{75}$.
  * $FP = 125 - 75 = \mathbf{50}$
  * $FN = 100 - 75 = \mathbf{25}$
  * $TN = 4{,}900 - 50 = \mathbf{4{,}850}$
  * *Check:* $75 + 50 + 25 + 4{,}850 = 5{,}000$.
  * **Metrics:**
  * **Accuracy:** $\frac{75 + 4{,}850}{5{,}000} = \frac{4{,}925}{5{,}000} = \mathbf{0.99} \text{ (or } 0.985\text{)}$
  * **Precision:** $\frac{75}{75 + 50} = \frac{75}{125} = \mathbf{0.60}$
  * **Recall:** $\frac{75}{75 + 25} = \frac{75}{100} = \mathbf{0.75}$
  * **Specificity:** $\frac{4{,}850}{4{,}850 + 50} = \frac{4{,}850}{4{,}900} = \frac{97}{98} \approx \mathbf{0.99} \text{ (or } 0.990\text{)}$
  * **$F_1$:** $2 \cdot \frac{0.60 \times 0.75}{0.60 + 0.75} = \frac{0.90}{1.35} = \frac{2}{3} \approx \mathbf{0.67} \text{ (or } 0.667\text{)}$
  
  ---