# Part C10 — Naive Bayes by Hand Practice Problems
#### Problem 1 (Standard Email Classifier Mirror)
  A spam filter is trained on $N = 12$ emails: $4$ spam and $8$ ham.  
  * The word `"offer"` appears in $3$ of the $4$ spam emails, and in $2$ of the $8$ ham emails.  
  * The word `"urgent"` appears in $2$ of the $4$ spam emails, and in $1$ of the $8$ ham emails.  
  
  An unclassified email arrives containing **both words**: $x_{\text{test}} = [\text{"offer"} = 1, \; \text{"urgent"} = 1]$.
  * **(a)** Compute the class priors $P(\text{Spam})$ and $P(\text{Ham})$.
  * **(b)** Compute the unnormalized joint probabilities $P(\text{Spam}, x_{\text{test}})$ and $P(\text{Ham}, x_{\text{test}})$.
  * **(c)** Compute the total marginal evidence $P(x_{\text{test}})$ and the final posterior probability $P(\text{Spam} \mid x_{\text{test}})$. State the classification decision.
  
  ---
#### Problem 2 (Handling Absent Features / Inverted Booleans)
  Using the same 10-email training dataset from the exam review packet ($4$ spam, $6$ ham):
  * `"free"` appears in $3$ of the $4$ spam emails, and in $1$ of the $6$ ham emails.
  * `"meeting"` appears in $1$ of the $4$ spam emails, and in $3$ of the $6$ ham emails.
  
  A new email arrives that **contains `"free"` but does NOT contain `"meeting"`** ($x_1 = 1, x_2 = 0$).
  * **(a)** Compute the conditional likelihoods $P(\text{meeting} = 0 \mid \text{Spam})$ and $P(\text{meeting} = 0 \mid \text{Ham})$.
  * **(b)** Compute the unnormalized joint probabilities for Spam and Ham.
  * **(c)** Calculate the normalized posterior probability $P(\text{Spam} \mid \text{free} = 1, \text{meeting} = 0)$ and classify the email.
  
  ---
#### Problem 3 (The Zero-Frequency Trap without Smoothing)
  A clinic tracks $N = 50$ patients: $20$ diagnosed with `Flu` and $30$ diagnosed with a common `Cold`.
  * `Fever`: Present in $18$ of the $20$ Flu patients, and in $3$ of the $30$ Cold patients.
  * `Loss_of_Smell`: Present in **$0$ of the $20$ Flu patients**, and in $6$ of the $30$ Cold patients.
  
  A patient presents with **both symptoms**: $\text{Fever} = 1, \; \text{Loss\_of\_Smell} = 1$.
  * **(a)** Compute the empirical likelihoods $P(\text{Loss\_of\_Smell} = 1 \mid \text{Flu})$ and $P(\text{Loss\_of\_Smell} = 1 \mid \text{Cold})$ under standard Maximum Likelihood Estimation (MLE).
  * **(b)** Compute the unnormalized joint likelihoods $P(\text{Flu}, x)$ and $P(\text{Cold}, x)$.
  * **(c)** What is the final posterior probability $P(\text{Flu} \mid x)$? Explain why this prediction represents a critical failure of un-smoothed Naive Bayes.
  
  ---
#### Problem 4 (Laplace Smoothing Resolution)
  Apply **Laplace Smoothing ($\alpha = 1$, binary cardinality $K = 2$)** to the clinical scenario in Problem 3:
  $$\hat{P}(X_j = 1 \mid Y = c) = \frac{N_{jc} + 1}{N_c + 2}$$
  * **(a)** Compute the smoothed likelihoods:
  * $P_{\text{Lap}}(\text{Fever} = 1 \mid \text{Flu})$
  * $P_{\text{Lap}}(\text{Loss\_of\_Smell} = 1 \mid \text{Flu})$
  * $P_{\text{Lap}}(\text{Fever} = 1 \mid \text{Cold})$
  * $P_{\text{Lap}}(\text{Loss\_of\_Smell} = 1 \mid \text{Cold})$
  * **(b)** Using the original class priors ($P(\text{Flu}) = 0.40, \; P(\text{Cold}) = 0.60$), compute the smoothed joint probabilities $P(\text{Flu}, x)$ and $P(\text{Cold}, x)$.
  * **(c)** Calculate the updated posterior probability $P(\text{Flu} \mid x)$. How does Laplace smoothing restore clinical plausibility?
  
  ---
#### Problem 5 (Multiclass 3-Way Naive Bayes)
  A text classification dataset categorizes documents into three topics ($N = 100$):  
  $$\text{Sports } (N_S = 30), \quad \text{Politics } (N_P = 40), \quad \text{Technology } (N_T = 30)$$
  Word frequencies are observed as follows:
  * Word `"win"`: appears in $24$ Sports, $4$ Politics, and $6$ Technology articles.
  * Word `"vote"`: appears in $3$ Sports, $32$ Politics, and $3$ Technology articles.
  
  A newly published article contains **both words**: $x_{\text{test}} = [\text{"win"} = 1, \; \text{"vote"} = 1]$.
  * **(a)** Compute the unnormalized joint scores $P(\text{Sports}, x)$, $P(\text{Politics}, x)$, and $P(\text{Tech}, x)$.
  * **(b)** Compute the total evidence $P(x)$.
  * **(c)** Calculate the normalized posterior probabilities for all three topics and identify the predicted class.
  
  ---
#### Problem 6 (Multi-Level Categorical Features: $K_j = 3$)
  A weather dataset predicts whether to play golf ($Y \in \{\text{Yes}, \text{No}\}$):
  * Total training days $N = 15$: $10$ Yes days ($P(\text{Yes}) = \frac{10}{15}$), $5$ No days ($P(\text{No}) = \frac{5}{15}$).
  * Feature 1 (`Sky` $\in$ {Sunny, Overcast, Rainy}):
  * For Yes ($10$ days): Sunny ($2$), Overcast ($5$), Rainy ($3$).
  * For No ($5$ days): Sunny ($3$), Overcast ($0$), Rainy ($2$).
  * Feature 2 (`Temperature` $\in$ {Hot, Cool}):
  * For Yes ($10$ days): Hot ($6$), Cool ($4$).
  * For No ($5$ days): Hot ($4$), Cool ($1$).
  
  A test day arrives with conditions: $\text{Sky} = \text{Sunny}$ and $\text{Temperature} = \text{Cool}$.
  * **(a)** Compute the conditional likelihoods for both classes without smoothing.
  * **(b)** Compute the unnormalized joint probabilities $P(\text{Yes}, x)$ and $P(\text{No}, x)$.
  * **(c)** Compute the normalized posterior probability $P(\text{Yes} \mid x)$ and predict whether to play.
  
  ---
#### Problem 7 (Gaussian Naive Bayes by Hand with Continuous Features)
  A financial fraud model evaluates a continuous transaction feature $X_1$ (`amount_hundreds`).
  The classes are $\text{Fraud } (Y = 1)$ and $\text{Legitimate } (Y = 0)$.  
  Priors: $P(Y = 1) = 0.400, \; P(Y = 0) = 0.600$.  
  The continuous class-conditional Gaussian parameters are:
  * $\text{Fraud } (Y = 1): \quad \mu_1 = 10.000, \quad \sigma_1^2 = 4.000 \implies \sigma_1 = 2.000$
  * $\text{Legitimate } (Y = 0): \quad \mu_0 = 16.000, \quad \sigma_0^2 = 9.000 \implies \sigma_0 = 3.000$
  
  A transaction arrives with $x_1 = 12.000$.
  * **(a)** Evaluate the Gaussian probability density $P(X_1 = 12.0 \mid Y = 1)$ and $P(X_1 = 12.0 \mid Y = 0)$.
  * **(b)** Compute the unnormalized joint probabilities $P(Y = 1, x_1 = 12.0)$ and $P(Y = 0, x_1 = 12.0)$.
  * **(c)** Compute the normalized posterior probability $P(Y = 1 \mid X_1 = 12.0)$ and classify the transaction.
  
  ---
#### Problem 8 (Log-Space Probabilities & Underflow Prevention)
  In a high-dimensional text classification problem with $d = 500$ words, computing products of probabilities $\prod_{j=1}^{500} P(X_j \mid Y)$ causes floating-point numbers to underflow to exact zero (`0.0`). The system operates in log-space:
  $$\text{Score}(c) = \ln P(Y = c) + \sum_{j=1}^d \ln P(X_j = x_j \mid Y = c)$$
  For a binary classification task ($C_1$ vs. $C_2$) with equal priors ($P(C_1) = 0.5, P(C_2) = 0.5$), the log-likelihood sums are evaluated as:
  $$\sum_{j=1}^{500} \ln P(X_j \mid C_1) = -14.200, \quad \sum_{j=1}^{500} \ln P(X_j \mid C_2) = -11.900$$
  * **(a)** Compute the total log-posterior scores $\text{Score}(C_1)$ and $\text{Score}(C_2)$.
  * **(b)** Using the identity $P(C_2 \mid x) = \frac{1}{1 + e^{\text{Score}(C_1) - \text{Score}(C_2)}}$, compute the exact normalized posterior probability $P(C_2 \mid x)$.
  
  ---
#### Problem 9 (Laplace Smoothing with High Feature Cardinality: $K_j = 3$)
  An automated intersection camera predicts vehicle collisions ($Y \in \{\text{Accident}, \text{Safe}\}$).  
  The training set contains $20$ Accident cases and $80$ Safe cases.  
  A categorical feature `traffic_light_state` has $K = 3$ levels: $\{\text{Red}, \text{Yellow}, \text{Green}\}$.
  * In Accident cases ($N = 20$): $\text{Green}$ was observed in exactly $1$ instance.
  * In Safe cases ($N = 80$): $\text{Green}$ was observed in $60$ instances.
  * **(a)** What is the empirical Maximum Likelihood estimate of $P(\text{Green} \mid \text{Accident})$?
  * **(b)** Apply Laplace smoothing ($\alpha = 1$). Show the exact formula and compute $P_{\text{Lap}}(\text{Green} \mid \text{Accident})$.
  * **(c)** Apply Laplace smoothing to compute $P_{\text{Lap}}(\text{Green} \mid \text{Safe})$. Explain why the denominator uses $+3$ rather than $+1$.
  
  ---
#### Problem 10 (Three-Feature Clinical Diagnosis & Prior Overcoming)
  A clinical screening model evaluates a rare genetic autoimmune disorder ($D = 1$) vs. healthy controls ($D = 0$) in a cohort of $N = 100$ patients:
  * Prior: $10$ Disease patients ($P(D=1) = 0.100$), $90$ Healthy controls ($P(D=0) = 0.900$).
  * Three binary biomarkers are tracked: $S_1$ (`Fatigue`), $S_2$ (`Joint_Pain`), and $S_3$ (`Rash`).
  
  Training data frequencies:
  * In Disease ($N = 10$): $S_1 = 1$ in $9$ patients; $S_2 = 1$ in $8$ patients; $S_3 = 1$ in $7$ patients.
  * In Healthy ($N = 90$): $S_1 = 1$ in $18$ patients; $S_2 = 1$ in $9$ patients; $S_3 = 1$ in $9$ patients.
  
  A patient presents with **all three symptoms**: $x_{\text{test}} = [S_1 = 1, \; S_2 = 1, \; S_3 = 1]$.
  * **(a)** Compute the conditional likelihood products $P(x_{\text{test}} \mid D = 1)$ and $P(x_{\text{test}} \mid D = 0)$.
  * **(b)** Compute the unnormalized joint probabilities $P(D = 1, x_{\text{test}})$ and $P(D = 0, x_{\text{test}})$.
  * **(c)** Calculate the normalized posterior probability $P(D = 1 \mid x_{\text{test}})$. How does the likelihood ratio overcome the $10 : 90$ prior?
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C10
### Problem 1 Solution
  * **(a) Compute Class Priors:**
  $$P(\text{Spam}) = \frac{4}{12} = \frac{1}{3} \approx \mathbf{0.333}, \quad P(\text{Ham}) = \frac{8}{12} = \frac{2}{3} \approx \mathbf{0.667}$$
  * **(b) Compute Unnormalized Joint Probabilities:**
  $$P(\text{Spam}, x) = P(\text{Spam}) \cdot P(\text{offer} = 1 \mid \text{Spam}) \cdot P(\text{urgent} = 1 \mid \text{Spam})$$
  $$P(\text{Spam}, x) = \left(\frac{1}{3}\right) \times \left(\frac{3}{4}\right) \times \left(\frac{2}{4}\right) = \frac{6}{48} = \frac{1}{8} = \mathbf{0.1250}$$
  $$P(\text{Ham}, x) = P(\text{Ham}) \cdot P(\text{offer} = 1 \mid \text{Ham}) \cdot P(\text{urgent} = 1 \mid \text{Ham})$$
  $$P(\text{Ham}, x) = \left(\frac{2}{3}\right) \times \left(\frac{2}{8}\right) \times \left(\frac{1}{8}\right) = \frac{4}{192} = \frac{1}{48} \approx \mathbf{0.0208}$$
  * **(c) Evidence Normalization & Classification:**
  $$P(x) = P(\text{Spam}, x) + P(\text{Ham}, x) = \frac{6}{48} + \frac{1}{48} = \frac{7}{48} \approx \mathbf{0.1458}$$
  $$P(\text{Spam} \mid x) = \frac{P(\text{Spam}, x)}{P(x)} = \frac{6/48}{7/48} = \frac{6}{7} \approx \mathbf{0.857 \quad (85.7\%)}$$
  $$P(\text{Ham} \mid x) = \frac{P(\text{Ham}, x)}{P(x)} = \frac{1/48}{7/48} = \frac{1}{7} \approx \mathbf{0.143 \quad (14.3\%)}$$
  * **Answer:** $\mathbf{P(\text{Spam} \mid x) = 0.857 \implies \text{Predict Spam}}$ `[L14, Review C10]`
  
  ---
### Problem 2 Solution
  * **(a) Evaluate Likelihoods for Absent Feature (`meeting = 0`):**
  Using the complement rule $P(X_2 = 0 \mid Y) = 1 - P(X_2 = 1 \mid Y)$:
  $$P(\text{meeting} = 0 \mid \text{Spam}) = 1 - \frac{1}{4} = \mathbf{\frac{3}{4} = 0.750}$$
  $$P(\text{meeting} = 0 \mid \text{Ham}) = 1 - \frac{3}{6} = \mathbf{\frac{3}{6} = 0.500}$$
  * **(b) Compute Unnormalized Joint Probabilities:**
  $$P(\text{Spam}, x) = P(\text{Spam}) \cdot P(\text{free}=1 \mid \text{Spam}) \cdot P(\text{meeting}=0 \mid \text{Spam}) = 0.40 \times \left(\frac{3}{4}\right) \times \left(\frac{3}{4}\right) = 0.40 \times \frac{9}{16} = \mathbf{0.2250}$$
  $$P(\text{Ham}, x) = P(\text{Ham}) \cdot P(\text{free}=1 \mid \text{Ham}) \cdot P(\text{meeting}=0 \mid \text{Ham}) = 0.60 \times \left(\frac{1}{6}\right) \times \left(\frac{1}{2}\right) = 0.10 \times 0.50 = \mathbf{0.0500}$$
  * **(c) Evidence Normalization & Decision:**
  $$P(x) = 0.2250 + 0.0500 = \mathbf{0.2750}$$
  $$P(\text{Spam} \mid x) = \frac{0.2250}{0.2750} = \frac{225}{275} = \frac{9}{11} \approx \mathbf{0.818 \quad (81.8\%)}$$
  $$P(\text{Ham} \mid x) = \frac{0.0500}{0.2750} = \frac{50}{275} = \frac{2}{11} \approx \mathbf{0.182 \quad (18.2\%)}$$
  * **Answer:** $\mathbf{P(\text{Spam} \mid x) = 0.818 \implies \text{Predict Spam}}$ `[L14]`
  
  ---
### Problem 3 Solution
  * **(a) Empirical Likelihoods (MLE):**
  $$P(\text{Loss\_of\_Smell} = 1 \mid \text{Flu}) = \frac{0}{20} = \mathbf{0.000}$$
  $$P(\text{Loss\_of\_Smell} = 1 \mid \text{Cold}) = \frac{6}{30} = \mathbf{0.200}$$
  * **(b) Compute Unnormalized Joint Likelihoods:**
  $$P(\text{Flu}, x) = P(\text{Flu}) \cdot P(\text{Fever}=1 \mid \text{Flu}) \cdot P(\text{LoS}=1 \mid \text{Flu}) = 0.40 \times \left(\frac{18}{20}\right) \times \mathbf{0.000} = \mathbf{0.000}$$
  $$P(\text{Cold}, x) = P(\text{Cold}) \cdot P(\text{Fever}=1 \mid \text{Cold}) \cdot P(\text{LoS}=1 \mid \text{Cold}) = 0.60 \times \left(\frac{3}{30}\right) \times \left(\frac{6}{30}\right) = 0.60 \times 0.10 \times 0.20 = \mathbf{0.0120}$$
  * **(c) Posterior Probability & Pathological Failure:**
  $$P(\text{Flu} \mid x) = \frac{0.000}{0.000 + 0.0120} = \mathbf{0.000 \quad (0.0\%)}, \quad P(\text{Cold} \mid x) = \mathbf{1.000 \quad (100.0\%) \implies \text{Predict Cold}}$$
  * **Failure Explanation:** The patient has a high fever ($90\%$ prevalence in flu vs. $10\%$ in cold). However, because `Loss_of_Smell` never occurred in the 20 historical flu cases, the raw MLE likelihood is zero. This single zero multiplies across the entire likelihood chain, setting the flu probability to exactly $0.0\%$. `[L14]`
  
  ---
### Problem 4 Solution
  * **(a) Compute Laplace-Smoothed Likelihoods ($\alpha = 1, K = 2$):**
  $$\hat{P} = \frac{N_{jc} + 1}{N_c + 2}$$
  * $P_{\text{Lap}}(\text{Fever}=1 \mid \text{Flu}) = \frac{18 + 1}{20 + 2} = \frac{19}{22} \approx \mathbf{0.864}$
  * $P_{\text{Lap}}(\text{LoS}=1 \mid \text{Flu}) = \frac{0 + 1}{20 + 2} = \frac{1}{22} \approx \mathbf{0.045}$
  * $P_{\text{Lap}}(\text{Fever}=1 \mid \text{Cold}) = \frac{3 + 1}{30 + 2} = \frac{4}{32} = \frac{1}{8} = \mathbf{0.125}$
  * $P_{\text{Lap}}(\text{LoS}=1 \mid \text{Cold}) = \frac{6 + 1}{30 + 2} = \frac{7}{32} \approx \mathbf{0.219}$
  * **(b) Compute Smoothed Joint Probabilities:**
  $$P(\text{Flu}, x) = 0.40 \times \left(\frac{19}{22}\right) \times \left(\frac{1}{22}\right) = 0.40 \times \frac{19}{484} = \frac{7.6}{484} \approx \mathbf{0.0157}$$
  $$P(\text{Cold}, x) = 0.60 \times \left(\frac{1}{8}\right) \times \left(\frac{7}{32}\right) = 0.60 \times \frac{7}{256} = \frac{4.2}{256} \approx \mathbf{0.0164}$$
  * **(c) Compute Posteriors:**
  $$\text{Evidence } P(x) = 0.01570 + 0.01641 = \mathbf{0.03211}$$
  $$P(\text{Flu} \mid x) = \frac{0.01570}{0.03211} \approx \mathbf{0.489 \quad (48.9\%)}, \quad P(\text{Cold} \mid x) = \frac{0.01641}{0.03211} \approx \mathbf{0.511 \quad (51.1\%)}$$
  * **Clinical Restoration:** Instead of assigning an artificial certainty of $0\%$ vs. $100\%$, Laplace smoothing recognizes that the patient exhibits a mixed presentation, yielding a balanced diagnosis ($\approx 49\%$ Flu vs. $51\%$ Cold). `[L14]`
  
  ---
### Problem 5 Solution
  * **(a) Compute Unnormalized Joint Scores:**
  Priors: $P(S) = 0.30, \; P(P) = 0.40, \; P(T) = 0.30$.
  * For Sports ($S$):
    $$P(S, x) = 0.30 \times \left(\frac{24}{30}\right) \times \left(\frac{3}{30}\right) = 0.30 \times 0.80 \times 0.10 = \mathbf{0.0240}$$
  * For Politics ($P$):
    $$P(P, x) = 0.40 \times \left(\frac{4}{40}\right) \times \left(\frac{32}{40}\right) = 0.40 \times 0.10 \times 0.80 = \mathbf{0.0320}$$
  * For Tech ($T$):
    $$P(T, x) = 0.30 \times \left(\frac{6}{30}\right) \times \left(\frac{3}{30}\right) = 0.30 \times 0.20 \times 0.10 = \mathbf{0.0060}$$
  * **(b) Compute Total Evidence:**
  $$P(x) = 0.0240 + 0.0320 + 0.0060 = \mathbf{0.0620}$$
  * **(c) Compute Posteriors and Predict:**
  $$P(\text{Sports} \mid x) = \frac{0.0240}{0.0620} = \frac{24}{62} \approx \mathbf{0.387 \quad (38.7\%)}$$
  $$P(\text{Politics} \mid x) = \frac{0.0320}{0.0620} = \frac{32}{62} \approx \mathbf{0.516 \quad (51.6\%)}$$
  $$P(\text{Tech} \mid x) = \frac{0.0060}{0.0620} = \frac{6}{62} \approx \mathbf{0.097 \quad (9.7\%)}$$
  * **Answer:** $\mathbf{P(\text{Politics} \mid x) = 0.516 \implies \text{Predict Politics}}$ `[L14]`
  
  ---
### Problem 6 Solution
  * **(a) Conditional Likelihoods ($x_{\text{test}} = [\text{Sunny}, \text{Cool}]$):**
  * For Yes ($N = 10$):
    $$P(\text{Sunny} \mid \text{Yes}) = \frac{2}{10} = \mathbf{0.200}, \quad P(\text{Cool} \mid \text{Yes}) = \frac{4}{10} = \mathbf{0.400}$$
  * For No ($N = 5$):
    $$P(\text{Sunny} \mid \text{No}) = \frac{3}{5} = \mathbf{0.600}, \quad P(\text{Cool} \mid \text{No}) = \frac{1}{5} = \mathbf{0.200}$$
  * **(b) Compute Unnormalized Joint Likelihoods:**
  $$P(\text{Yes}, x) = P(\text{Yes}) \cdot P(\text{Sunny}\mid\text{Yes}) \cdot P(\text{Cool}\mid\text{Yes}) = \left(\frac{10}{15}\right) \times 0.200 \times 0.400 = \left(\frac{2}{3}\right) \times 0.080 = \frac{0.160}{3} \approx \mathbf{0.0533}$$
  $$P(\text{No}, x) = P(\text{No}) \cdot P(\text{Sunny}\mid\text{No}) \cdot P(\text{Cool}\mid\text{No}) = \left(\frac{5}{15}\right) \times 0.600 \times 0.200 = \left(\frac{1}{3}\right) \times 0.120 = \frac{0.120}{3} = \mathbf{0.0400}$$
  * **(c) Evidence Normalization & Prediction:**
  $$P(x) = \frac{0.160}{3} + \frac{0.120}{3} = \frac{0.280}{3} \approx \mathbf{0.0933}$$
  $$P(\text{Yes} \mid x) = \frac{0.160 / 3}{0.280 / 3} = \frac{160}{280} = \frac{4}{7} \approx \mathbf{0.571 \quad (57.1\%)}$$
  $$P(\text{No} \mid x) = \frac{0.120 / 3}{0.280 / 3} = \frac{120}{280} = \frac{3}{7} \approx \mathbf{0.429 \quad (42.9\%)}$$
  * **Answer:** $\mathbf{P(\text{Yes} \mid x) = 0.571 \implies \text{Predict Play (Yes)}}$ `[L14]`
  
  ---
### Problem 7 Solution
  * **(a) Evaluate Gaussian Likelihood Densities:**
  The univariate Gaussian density formula is:
  $$P(x \mid \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$
  1. *For Fraud ($Y = 1, \mu_1 = 10, \sigma_1 = 2$):*
     $$P(12.0 \mid Y = 1) = \frac{1}{2\sqrt{2\pi}} \exp\left(-\frac{(12 - 10)^2}{2(4)}\right) = \frac{0.3989}{2} e^{-4/8} = 0.1995 \times e^{-0.5} \approx 0.1995 \times 0.6065 \approx \mathbf{0.1210}$$
  2. *For Legitimate ($Y = 0, \mu_0 = 16, \sigma_0 = 3$):*
     $$P(12.0 \mid Y = 0) = \frac{1}{3\sqrt{2\pi}} \exp\left(-\frac{(12 - 16)^2}{2(9)}\right) = \frac{0.3989}{3} e^{-16/18} = 0.1330 \times e^{-0.8889} \approx 0.1330 \times 0.4111 \approx \mathbf{0.0547}$$
  * **(b) Compute Unnormalized Joint Probabilities:**
  $$P(Y = 1, x) = P(Y = 1) \cdot P(x \mid Y = 1) = 0.400 \times 0.1210 \approx \mathbf{0.0484}$$
  $$P(Y = 0, x) = P(Y = 0) \cdot P(x \mid Y = 0) = 0.600 \times 0.0547 \approx \mathbf{0.0328}$$
  * **(c) Normalize and Classify:**
  $$\text{Evidence } P(x) = 0.0484 + 0.0328 = \mathbf{0.0812}$$
  $$P(Y = 1 \mid x) = \frac{0.0484}{0.0812} \approx \mathbf{0.596 \quad (59.6\%)}$$
  $$P(Y = 0 \mid x) = \frac{0.0328}{0.0812} \approx \mathbf{0.404 \quad (40.4\%)}$$
  * **Answer:** $\mathbf{P(\text{Fraud} \mid x) = 0.596 \implies \text{Predict Fraud } (Y = 1)}$ `[L14]`
  
  ---
### Problem 8 Solution
  * **(a) Compute Total Log-Posterior Scores:**
  $$\text{Score}(c) = \ln P(Y = c) + \sum_{j=1}^{500} \ln P(X_j \mid Y = c)$$
  $$\text{Score}(C_1) = \ln(0.500) + (-14.200) = -0.693 - 14.200 = \mathbf{-14.893}$$
  $$\text{Score}(C_2) = \ln(0.500) + (-11.900) = -0.693 - 11.900 = \mathbf{-12.593}$$
  * **(b) Exact Posterior via Log-Sum-Exp Trick:**
  $$P(C_2 \mid x) = \frac{e^{\text{Score}(C_2)}}{e^{\text{Score}(C_1)} + e^{\text{Score}(C_2)}} = \frac{1}{1 + e^{\text{Score}(C_1) - \text{Score}(C_2)}}$$
  Evaluate exponent difference:
  $$\Delta = \text{Score}(C_1) - \text{Score}(C_2) = -14.893 - (-12.593) = -2.300$$
  $$P(C_2 \mid x) = \frac{1}{1 + e^{-2.300}} \approx \frac{1}{1 + 0.1003} = \frac{1}{1.1003} \approx \mathbf{0.909 \quad (90.9\%)}$$
  $$P(C_1 \mid x) = 1 - 0.909 = \mathbf{0.091 \quad (9.1\%) \implies \text{Predict Class } C_2}$$ `[L13, L14]`
  
  ---
### Problem 9 Solution
  * **(a) Empirical MLE Likelihood:**
  $$P(\text{Green} \mid \text{Accident}) = \frac{1}{20} = \mathbf{0.050 \quad (5.0\%)}$$
  * **(b) Laplace Smoothed Likelihood ($\alpha = 1, K = 3$):**
  $$\hat{P} = \frac{N_{jc} + 1}{N_c + K} = \frac{1 + 1}{20 + 3} = \mathbf{\frac{2}{23} \approx 0.087 \quad (8.7\%)}$$
  * **(c) Smoothed Likelihood for Safe Cases:**
  $$P_{\text{Lap}}(\text{Green} \mid \text{Safe}) = \frac{60 + 1}{80 + 3} = \mathbf{\frac{61}{83} \approx 0.735 \quad (73.5\%)}$$
  * **Why Denominator is $+3$:** The denominator must account for pseudocounts added to **every possible state of the categorical variable**. Because `traffic_light_state` has $K = 3$ levels (Red, Yellow, Green), adding $\alpha = 1$ to each of the 3 outcomes adds $3$ total pseudocounts ($N_c + 3$). This guarantees that the smoothed probabilities across all three colors sum to $1.0$:
    $$\sum_{k=1}^3 \hat{P}_k = \frac{\sum (N_{kc} + 1)}{N_c + K} = \frac{N_c + K}{N_c + K} \equiv 1.0$$ `[L14]`
  
  ---
### Problem 10 Solution
  * **(a) Compute Conditional Likelihood Products:**
  * For Disease ($D = 1$):
    $$P(x_{\text{test}} \mid D = 1) = \left(\frac{9}{10}\right) \times \left(\frac{8}{10}\right) \times \left(\frac{7}{10}\right) = 0.900 \times 0.800 \times 0.700 = \mathbf{0.5040}$$
  * For Healthy Control ($D = 0$):
    $$P(x_{\text{test}} \mid D = 0) = \left(\frac{18}{90}\right) \times \left(\frac{9}{90}\right) \times \left(\frac{9}{90}\right) = 0.200 \times 0.100 \times 0.100 = \mathbf{0.0020}$$
  * **(b) Compute Unnormalized Joint Probabilities:**
  $$P(D = 1, x) = P(D = 1) \cdot P(x \mid D = 1) = 0.100 \times 0.5040 = \mathbf{0.0504}$$
  $$P(D = 0, x) = P(D = 0) \cdot P(x \mid D = 0) = 0.900 \times 0.0020 = \mathbf{0.0018}$$
  * **(c) Evidence Normalization & Classification:**
  $$P(x) = 0.0504 + 0.0018 = \mathbf{0.0522}$$
  $$P(D = 1 \mid x) = \frac{0.0504}{0.0522} = \frac{504}{522} = \frac{28}{29} \approx \mathbf{0.966 \quad (96.6\%)}$$
  $$P(D = 0 \mid x) = \frac{0.0018}{0.0522} = \frac{18}{522} = \frac{1}{29} \approx \mathbf{0.034 \quad (3.4\%)}$$
  * **Answer:** $\mathbf{P(\text{Disease} \mid x) = 0.966 \implies \text{Predict Disease } (D = 1)}$
  * *Bayesian Prior Overcoming:* Despite the disease having a low prior base rate ($10\%$), observing all three symptoms provides an evidence likelihood ratio of $\frac{0.5040}{0.0020} = \mathbf{252\times \text{ in favor of disease}}$. This evidence overwhelms the prior, driving the posterior probability to $96.6\%$. `[L10, L14]`
  
  ---