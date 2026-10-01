### Question 1: Ordinary Least Squares by Hand from Centered Moments
  A regression dataset contains $n = 5$ observations:
  $$X = [1, \; 2, \; 3, \; 4, \; 5], \quad y = [2, \; 4, \; 5, \; 9, \; 10]$$
  The model is parameterized as $\hat{y} = w_0 + w_1 x$.
  
  **(a)** Calculate the sample means $\bar{x}$ and $\bar{y}$.  
  **(b)** Construct the deviation table and compute $\sum (x_i - \bar{x})^2$ and $\sum (x_i - \bar{x})(y_i - \bar{y})$.  
  **(c)** Solve for the optimal slope $w_1$ and intercept $w_0$.  
  **(d)** Compute the Residual Sum of Squares ($\text{SS}_{\text{res}}$) and the Coefficient of Determination ($R^2$).
  
  ---
#### Worked Solution:
  * **(a) Compute Sample Means:**
  $$\bar{x} = \frac{1 + 2 + 3 + 4 + 5}{5} = \frac{15}{5} = \mathbf{3.000}$$
  $$\bar{y} = \frac{2 + 4 + 5 + 9 + 10}{5} = \frac{30}{5} = \mathbf{6.000}$$
  
  * **(b) Construct Moment Deviation Table:**
  ```
  ┌──────┬─────┬─────┬─────────────┬─────────────┬────────────────┬─────────────────────┐
  │ i    │ x_i │ y_i │ (x_i - x̄)   │ (y_i - ȳ)   │ (x_i - x̄)²     │ (x_i - x̄)(y_i - ȳ)  │
  ├──────┼─────┼─────┼─────────────┼─────────────┼────────────────┼─────────────────────┤
  │ 1    │ 1   │ 2   │ 1 - 3 = -2  │ 2 - 6 = -4  │ (-2)² = 4.0    │ (-2)(-4) = +8.0     │
  │ 2    │ 2   │ 4   │ 2 - 3 = -1  │ 4 - 6 = -2  │ (-1)² = 1.0    │ (-1)(-2) = +2.0     │
  │ 3    │ 3   │ 5   │ 3 - 3 =  0  │ 5 - 6 = -1  │ ( 0)² = 0.0    │ ( 0)(-1) =  0.0     │
  │ 4    │ 4   │ 9   │ 4 - 3 = +1  │ 9 - 6 = +3  │ (+1)² = 1.0    │ (+1)(+3) = +3.0     │
  │ 5    │ 5   │ 10  │ 5 - 3 = +2  │ 10 - 6 = +4 │ (+2)² = 4.0    │ (+2)(+4) = +8.0     │
  ├──────┴─────┴─────┴─────────────┴─────────────┼────────────────┼─────────────────────┤
  │ Sums (∑):                                    │ ∑(x - x̄)²= 10.0│ ∑(x - x̄)(y - ȳ)=21.0│
  └──────────────────────────────────────────────┴────────────────┴─────────────────────┘
  ```
  
  * **(c) Compute Slope ($w_1$) and Intercept ($w_0$):**
  $$w_1 = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sum (x_i - \bar{x})^2} = \frac{21.0}{10.0} = \mathbf{2.100}$$
  $$w_0 = \bar{y} - w_1 \bar{x} = 6.000 - (2.100 \times 3.000) = 6.000 - 6.300 = \mathbf{-0.300}$$
  $$\mathbf{\hat{y} = -0.300 + 2.100x}$$
  
  * **(d) Compute $\text{SS}_{\text{res}}$ and $R^2$:**
  Evaluate predictions $\hat{y}_i = -0.3 + 2.1 x_i$ and residuals $r_i = y_i - \hat{y}_i$:
  * $x_1 = 1 \implies \hat{y}_1 = 1.8 \implies r_1 = 2 - 1.8 = +0.2 \implies r_1^2 = 0.04$
  * $x_2 = 2 \implies \hat{y}_2 = 3.9 \implies r_2 = 4 - 3.9 = +0.1 \implies r_2^2 = 0.01$
  * $x_3 = 3 \implies \hat{y}_3 = 6.0 \implies r_3 = 5 - 6.0 = -1.0 \implies r_3^2 = 1.00$
  * $x_4 = 4 \implies \hat{y}_4 = 8.1 \implies r_4 = 9 - 8.1 = +0.9 \implies r_4^2 = 0.81$
  * $x_5 = 5 \implies \hat{y}_5 = 10.2 \implies r_5 = 10 - 10.2 = -0.2 \implies r_5^2 = 0.04$
  $$\text{SS}_{\text{res}} = \sum r_i^2 = 0.04 + 0.01 + 1.00 + 0.81 + 0.04 = \mathbf{1.900}$$
  $$\text{SS}_{\text{tot}} = \sum (y_i - \bar{y})^2 = (-4)^2 + (-2)^2 + (-1)^2 + (3)^2 + (4)^2 = 16 + 4 + 1 + 9 + 16 = \mathbf{46.000}$$
  $$R^2 = 1 - \frac{\text{SS}_{\text{res}}}{\text{SS}_{\text{tot}}} = 1 - \frac{1.900}{46.000} = 1 - 0.0413 = \mathbf{0.959 \quad (95.9\%)}$$
  
  ---
### Question 2: Batch vs. Stochastic Gradient Descent Steps
  Given a three-sample training dataset ($n = 3$):
  $$(x_1, y_1) = (1, 2), \quad (x_2, y_2) = (2, 3), \quad (x_3, y_3) = (3, 7)$$
  A linear model $\hat{y} = w_0 + w_1 x$ is trained with cost function $J = \frac{1}{n}\sum_{i=1}^n (\hat{y}_i - y_i)^2$.
  
  **(a)** Starting from initial parameters $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$ with step size $\eta = 0.10$, compute **ONE parameter update step of Batch Gradient Descent (BGD)**.  
  **(b)** Starting from the same initial parameters $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$ with $\eta = 0.10$, compute **ONE parameter update step of Stochastic Gradient Descent (SGD)** using only the first sample $(x_1, y_1) = (1, 2)$.
  
  ---
#### Worked Solution:
  * **(a) Batch Gradient Descent Step ($n = 3$):**
  1. *Initial Predictions and Errors:*
     Because $w_0 = w_1 = 0$, $\hat{y}_i = 0$ for all samples:
     $$e_1 = \hat{y}_1 - y_1 = 0 - 2 = -2$$
     $$e_2 = \hat{y}_2 - y_2 = 0 - 3 = -3$$
     $$e_3 = \hat{y}_3 - y_3 = 0 - 7 = -7$$
  2. *Compute Batch Gradients:*
     $$\nabla_{w_0} J = \frac{2}{n}\sum_{i=1}^3 e_i = \frac{2}{3}(-2 - 3 - 7) = \frac{2}{3}(-12) = \mathbf{-8.000}$$
     $$\nabla_{w_1} J = \frac{2}{n}\sum_{i=1}^3 x_i e_i = \frac{2}{3}\Big[ 1(-2) + 2(-3) + 3(-7) \Big] = \frac{2}{3}[-2 - 6 - 21] = \frac{2}{3}(-29) \approx \mathbf{-19.333}$$
  3. *Update Parameters ($\eta = 0.10$):*
     $$w_0^{(1)} = w_0^{(0)} - \eta \nabla_{w_0} J = 0 - 0.10(-8.000) = \mathbf{+0.800}$$
     $$w_1^{(1)} = w_1^{(0)} - \eta \nabla_{w_1} J = 0 - 0.10(-19.333) = \mathbf{+1.933}$$
  * **Answer (a):** $\mathbf{w_0^{(1)} = 0.800, \; w_1^{(1)} = 1.933}$
  
  * **(b) Stochastic Gradient Descent Step on Sample 1 $(x=1, y=2)$:**
  Per-sample loss: $\mathcal{L}_1 = (\hat{y}_1 - y_1)^2 = (w_0 + w_1(1) - 2)^2$.
  1. *Compute Stochastic Gradients:*
     $$\nabla_{w_0} \mathcal{L}_1 = 2(\hat{y}_1 - y_1) = 2(0 - 2) = \mathbf{-4.000}$$
     $$\nabla_{w_1} \mathcal{L}_1 = 2 x_1 (\hat{y}_1 - y_1) = 2(1)(-2) = \mathbf{-4.000}$$
  2. *Update Parameters ($\eta = 0.10$):*
     $$w_0^{(1)} = 0 - 0.10(-4.000) = \mathbf{+0.400}$$
     $$w_1^{(1)} = 0 - 0.10(-4.000) = \mathbf{+0.400}$$
  * **Answer (b):** $\mathbf{w_0^{(1)} = 0.400, \; w_1^{(1)} = 0.400}$
  
  ---
### Question 3: Logistic Regression Forward Pass & Probability Calibration
  A fitted binary logistic regression classifier has parameter weights $w = [3.0, \; -2.0]^T$ and bias $b = 1.0$. A query instance is given by $x = [0.5, \; 2.0]^T$.
  
  **(a)** Compute the linear logit score $z$.  
  **(b)** Compute the predicted posterior probability $p = \sigma(z)$.  
  **(c)** Compute the odds ($\frac{p}{1-p}$) and verify that $\ln(\text{Odds}) = z$.  
  **(d)** Predict the discrete class label at threshold $\tau = 0.25$ and at threshold $\tau = 0.75$.
  
  ---
#### Worked Solution:
  * **(a) Compute Logit Score ($z$):**
  $$z = w^T x + b = w_1 x_1 + w_2 x_2 + b = 3.0(0.5) + (-2.0)(2.0) + 1.0$$
  $$z = 1.5 - 4.0 + 1.0 = \mathbf{-1.500}$$
  
  * **(b) Compute Probability ($p = \sigma(z)$):**
  $$p = \frac{1}{1 + e^{-z}} = \frac{1}{1 + e^{-(-1.5)}} = \frac{1}{1 + e^{1.5}} \approx \frac{1}{1 + 4.481689} = \frac{1}{5.481689} \approx \mathbf{0.182 \quad (18.2\%)}$$
  
  * **(c) Compute Odds and Verify Log-Odds:**
  $$\text{Odds} = e^z = e^{-1.5} \approx \mathbf{0.223}$$
  $$\frac{p}{1 - p} = \frac{0.1824}{1 - 0.1824} = \frac{0.1824}{0.8176} \approx \mathbf{0.223}$$
  $$\ln(\text{Odds}) = \ln(0.2231) = \mathbf{-1.500} \equiv z \quad \text{(Verified)}$$
  
  * **(d) Threshold Classification Decisions:**
  * **At Threshold $\tau = 0.25$:**
    $$p = 0.182 < 0.25 \implies \mathbf{\hat{y} = 0 \quad (\text{Class 0})}$$
  * **At Threshold $\tau = 0.75$:**
    $$p = 0.182 < 0.75 \implies \mathbf{\hat{y} = 0 \quad (\text{Class 0})}$$
  
  ---
### Question 4: Decision Boundary Hyperplane Equations in $\mathbb{R}^2$
  A trained logistic regression model separating two classes in $\mathbb{R}^2$ has parameters:
  $$w_1 = -4.0, \quad w_2 = +2.0, \quad b = +6.0$$
  
  **(a)** Derive the explicit slope-intercept equation ($x_2 = m x_1 + c$) of the decision boundary under the standard symmetric decision threshold $\tau = 0.50$.  
  **(b)** Determine whether the origin $(x_1 = 0, x_2 = 0)$ lies in the Class 1 or Class 0 half-space.  
  **(c)** Derive the new decision boundary line equation if the threshold is adjusted to $\tau = \frac{1}{1 + e^2} \approx 0.119$ to prioritize detection of the positive class.
  
  ---
#### Worked Solution:
  * **(a) Decision Boundary at $\tau = 0.50$:**
  The boundary occurs where probability is $0.50 \iff z = 0$:
  $$w_1 x_1 + w_2 x_2 + b = 0 \implies -4x_1 + 2x_2 + 6 = 0$$
  $$2x_2 = 4x_1 - 6 \implies \mathbf{x_2 = 2x_1 - 3}$$
  *Slope $m = 2.0$, Intercept $c = -3.0$.*
  
  * **(b) Class Assignment of Origin $(0, 0)$:**
  Evaluate the linear score at $(0, 0)$:
  $$z(0, 0) = -4(0) + 2(0) + 6 = +6.0$$
  $$p(0, 0) = \sigma(6.0) \approx 0.9975 > 0.50 \implies \mathbf{\text{Class 1 (Positive Side)}}$$
  
  * **(c) Decision Boundary at $\tau \approx 0.119$ ($z = -2$):**
  $$P(Y = 1 \mid x) = \frac{1}{1 + e^2} \iff \sigma(z) = \sigma(-2) \iff z = -2$$
  $$-4x_1 + 2x_2 + 6 = -2$$
  $$2x_2 = 4x_1 - 8 \implies \mathbf{x_2 = 2x_1 - 4}$$
  *Lowering the threshold shifts the boundary downward parallel to itself ($c = -4.0$), expanding the Class 1 decision region.*
  
  ---
### Question 5: $k$-NN Distance Ranking & Spatial Metric Sensitivity
  A 2D spatial dataset contains five labeled training points:
  * $A = (2, 2)$, Class: $+$
  * $B = (1, 4)$, Class: $-$
  * $C = (4, 3)$, Class: $-$
  * $D = (0, 1)$, Class: $+$
  * $E = (5, 6)$, Class: $-$
  
  A query instance is situated at $q = (2, 3)$.
  
  **(a)** Compute the exact Euclidean distance from $q$ to all five points.  
  **(b)** Rank the neighbors from nearest to farthest, and predict the class label for $k = 1$, $k = 3$, and $k = 5$.  
  **(c)** Compute the Manhattan ($L_1$) distance from $q$ to Point $B$ and Point $C$. Does the nearest-neighbor ranking change under $L_1$?
  
  ---
#### Worked Solution:
  * **(a) Compute Euclidean Distances ($D_2 = \sqrt{\Delta x_1^2 + \Delta x_2^2}$):**
  * $d(q, A) = \sqrt{(2 - 2)^2 + (3 - 2)^2} = \sqrt{0 + 1} = \mathbf{1.000}$
  * $d(q, B) = \sqrt{(2 - 1)^2 + (3 - 4)^2} = \sqrt{1 + 1} = \sqrt{2} \approx \mathbf{1.414}$
  * $d(q, C) = \sqrt{(2 - 4)^2 + (3 - 3)^2} = \sqrt{4 + 0} = \sqrt{4} = \mathbf{2.000}$
  * $d(q, D) = \sqrt{(2 - 0)^2 + (3 - 1)^2} = \sqrt{4 + 4} = \sqrt{8} \approx \mathbf{2.828}$
  * $d(q, E) = \sqrt{(2 - 5)^2 + (3 - 6)^2} = \sqrt{9 + 9} = \sqrt{18} \approx \mathbf{4.243}$
  
  * **(b) Neighbor Ranking & Mode Voting:**
  * **Rankings:** 1: $A$ (1.000), 2: $B$ (1.414), 3: $C$ (2.000), 4: $D$ (2.828), 5: $E$ (4.243).
  * **$k = 1$:** Nearest point is $A \implies \mathbf{\text{Class } +}$
  * **$k = 3$:** Nearest points are $\{A (+), B (-), C (-)\} \implies 1 (+)$ vs. $2 (-) \implies \mathbf{\text{Class } -}$
  * **$k = 5$:** All five points $\{A (+), B (-), C (-), D (+), E (-)\} \implies 2 (+)$ vs. $3 (-) \implies \mathbf{\text{Class } -}$
  
  * **(c) Manhattan Distance ($D_1 = |\Delta x_1| + |\Delta x_2|$):**
  * $D_1(q, B) = |2 - 1| + |3 - 4| = 1 + 1 = \mathbf{2.000}$
  * $D_1(q, C) = |2 - 4| + |3 - 3| = 2 + 0 = \mathbf{2.000}$
  * **Yes, the metric alters topology:** Under Euclidean distance, $B$ is strictly closer than $C$ ($1.414 < 2.000$). Under Manhattan distance, $B$ and $C$ are tied at distance $2.000$.
  
  ---
### Question 6: Inverse-Distance Weighted $k$-NN
  In an instance-based classification task with $k = 3$, the three nearest neighbors to query $q$ have the following properties:
  * Neighbor 1: Distance $d_1 = 1.0$, Class: **Negative ($-$)**
  * Neighbor 2: Distance $d_2 = 2.0$, Class: **Positive ($+$)**
  * Neighbor 3: Distance $d_3 = 2.0$, Class: **Positive ($+$)**
  
  **(a)** What class label is predicted by standard unweighted majority voting?  
  **(b)** What class label is predicted if votes are weighted inversely by squared Euclidean distance ($w_i = \frac{1}{d_i^2}$)? Show the exact voting tallies for both classes.
  
  ---
#### Worked Solution:
  * **(a) Unweighted Majority Voting:**
  * Positive votes: $2$ (Neighbors 2 and 3).
  * Negative votes: $1$ (Neighbor 1).
  * **Prediction:** $\mathbf{\text{Class } + \quad (\text{Positive})}$
  
  * **(b) Inverse-Distance Squared Weighted Voting:**
  Compute weights:
  $$w_1 = \frac{1}{d_1^2} = \frac{1}{1.0^2} = \mathbf{1.000}$$
  $$w_2 = \frac{1}{d_2^2} = \frac{1}{2.0^2} = \mathbf{0.250}$$
  $$w_3 = \frac{1}{d_3^2} = \frac{1}{2.0^2} = \mathbf{0.250}$$
  
  Accumulate voting weights by class:
  $$\text{Score}(-) = w_1 = \mathbf{1.000}$$
  $$\text{Score}(+) = w_2 + w_3 = 0.250 + 0.250 = \mathbf{0.500}$$
  $$\arg\max \{ \text{Score}(-), \; \text{Score}(+) \} = \arg\max \{ 1.000, \; 0.500 \} = \mathbf{\text{Class } -}$$
  * **Answer:** Under distance weighting, the prediction flips to **Class $-$** because Neighbor 1 is close enough to outvote the two more distant positive instances.
  
  ---
### Question 7: Gaussian Naive Bayes by Hand
  A continuous feature $X_1$ (`serum_protein`) is modeled via Gaussian Naive Bayes across two medical classes: $\text{Disease } (Y = 1)$ and $\text{Healthy } (Y = 0)$. 
  
  The priors are equal: $P(Y = 1) = 0.50, \; P(Y = 0) = 0.50$.  
  The class-conditional parametric moments are:
  * $\text{Disease } (Y = 1): \quad \mu_1 = 2.0, \quad \sigma_1^2 = 1.0$
  * $\text{Healthy } (Y = 0): \quad \mu_0 = 5.0, \quad \sigma_0^2 = 4.0$
  
  A patient presents with measurement $x_1 = 3.0$.
  
  **(a)** Evaluate the Gaussian likelihood density $P(X_1 = 3.0 \mid Y = 1)$ and $P(X_1 = 3.0 \mid Y = 0)$.  
  **(b)** Compute the unnormalized joint probabilities $P(Y = 1, x_1 = 3.0)$ and $P(Y = 0, x_1 = 3.0)$.  
  **(c)** Compute the normalized posterior probability $P(Y = 1 \mid X_1 = 3.0)$ and assign the predicted class label.
  
  ---
#### Worked Solution:
  * **(a) Compute Univariate Gaussian Likelihood Densities:**
  The univariate Gaussian probability density function is:
  $$P(x \mid \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left( -\frac{(x - \mu)^2}{2\sigma^2} \right)$$
  1. *For Disease ($Y = 1$):*
     $$P(3.0 \mid Y = 1) = \frac{1}{\sqrt{2\pi(1.0)}} \exp\left( -\frac{(3.0 - 2.0)^2}{2(1.0)} \right) = \frac{1}{\sqrt{2\pi}} e^{-0.5} \approx 0.39894 \times 0.60653 \approx \mathbf{0.242}$$
  2. *For Healthy ($Y = 0$):*
     $$P(3.0 \mid Y = 0) = \frac{1}{\sqrt{2\pi(4.0)}} \exp\left( -\frac{(3.0 - 5.0)^2}{2(4.0)} \right) = \frac{1}{2\sqrt{2\pi}} \exp\left(-\frac{4.0}{8.0}\right) = \frac{1}{2\sqrt{2\pi}} e^{-0.5} \approx \frac{0.24197}{2} \approx \mathbf{0.121}$$
  
  * **(b) Compute Unnormalized Joint Likelihoods:**
  $$P(Y = 1, x) = P(Y = 1) \cdot P(x \mid Y = 1) = 0.50 \times 0.24197 \approx \mathbf{0.121}$$
  $$P(Y = 0, x) = P(Y = 0) \cdot P(x \mid Y = 0) = 0.50 \times 0.12099 \approx \mathbf{0.060}$$
  
  * **(c) Compute Normalized Posteriors:**
  $$\text{Evidence } P(x) = 0.12099 + 0.06049 = \mathbf{0.18148}$$
  $$P(Y = 1 \mid x) = \frac{0.12099}{0.18148} = \frac{2}{3} \approx \mathbf{0.667 \quad (66.7\%)}$$
  $$P(Y = 0 \mid x) = \frac{0.06049}{0.18148} = \frac{1}{3} \approx \mathbf{0.333 \quad (33.3\%)}$$
  * **Answer:** $\mathbf{P(\text{Disease} \mid x) = 0.667 \implies \text{Predict Disease } (Y = 1)}$
  
  ---
### Question 8: Discrete Naive Bayes with Laplace Smoothing
  An epidemiological study of $N = 100$ patients tracks two binary symptoms: $S_1$ (`Fever`) and $S_2$ (`Cough`) to detect infection ($D = 1$ vs. $D = 0$).  
  The training manifest contains:
  * $N_{\text{disease}} = 20$ patients ($P(D=1) = 0.20$)
  * $N_{\text{healthy}} = 80$ patients ($P(D=0) = 0.80$)
  * In Disease ($D=1$): $\text{Fever}=1$ in $18$ patients; $\text{Cough}=1$ in $16$ patients.
  * In Healthy ($D=0$): $\text{Fever}=1$ in $8$ patients; $\text{Cough}=1$ in $24$ patients.
  
  A patient presents with **Fever = Yes ($S_1 = 1$)** and **Cough = No ($S_2 = 0$)**.
  
  **(a)** Using standard Maximum Likelihood Estimation (un-smoothed frequencies), compute the unnormalized posteriors and classify the patient.  
  **(b)** Recompute the conditional likelihoods and posterior probability using **Laplace Smoothing ($\alpha = 1$)**.
  
  ---
#### Worked Solution:
  * **(a) Un-Smoothed MLE Classification:**
  1. *Likelihoods for Disease ($D = 1$):*
     $$P(S_1 = 1 \mid D = 1) = \frac{18}{20} = 0.900, \quad P(S_2 = 0 \mid D = 1) = \frac{20 - 16}{20} = \frac{4}{20} = 0.200$$
     $$P(D = 1, x) = P(D = 1) \cdot P(S_1 = 1 \mid D = 1) \cdot P(S_2 = 0 \mid D = 1) = 0.20 \times 0.90 \times 0.20 = \mathbf{0.036}$$
  2. *Likelihoods for Healthy ($D = 0$):*
     $$P(S_1 = 1 \mid D = 0) = \frac{8}{80} = 0.100, \quad P(S_2 = 0 \mid D = 0) = \frac{80 - 24}{80} = \frac{56}{80} = 0.700$$
     $$P(D = 0, x) = P(D = 0) \cdot P(S_1 = 1 \mid D = 0) \cdot P(S_2 = 0 \mid D = 0) = 0.80 \times 0.10 \times 0.70 = \mathbf{0.056}$$
  3. *Posterior Normalization:*
     $$P(D = 1 \mid x) = \frac{0.036}{0.036 + 0.056} = \frac{0.036}{0.092} = \frac{9}{23} \approx \mathbf{0.391 \quad (39.1\%)}$$
     $$P(D = 0 \mid x) = \frac{0.056}{0.092} = \frac{14}{23} \approx \mathbf{0.609 \quad (60.9\%) \implies \text{Predict Healthy } (D = 0)}$$
  
  * **(b) With Laplace Smoothing ($\alpha = 1$, Binary Features $K_j = 2$):**
  $$\hat{P}(S_j = v \mid D = c) = \frac{N_{jc} + 1}{N_c + 2}$$
  1. *Smoothed Likelihoods:*
     $$P_{\text{Lap}}(S_1 = 1 \mid D = 1) = \frac{18 + 1}{20 + 2} = \frac{19}{22} \approx 0.8636$$
     $$P_{\text{Lap}}(S_2 = 0 \mid D = 1) = \frac{4 + 1}{20 + 2} = \frac{5}{22} \approx 0.2273$$
     $$P_{\text{Lap}}(S_1 = 1 \mid D = 0) = \frac{8 + 1}{80 + 2} = \frac{9}{82} \approx 0.1098$$
     $$P_{\text{Lap}}(S_2 = 0 \mid D = 0) = \frac{56 + 1}{80 + 2} = \frac{57}{82} \approx 0.6951$$
  2. *Smoothed Priors:*
     $$P_{\text{Lap}}(D = 1) = \frac{20 + 1}{100 + 2} = \frac{21}{102}, \quad P_{\text{Lap}}(D = 0) = \frac{80 + 1}{100 + 2} = \frac{81}{102}$$
  3. *Smoothed Posteriors:*
     $$P(D = 1, x) = \left(\frac{21}{102}\right) \times \left(\frac{19}{22}\right) \times \left(\frac{5}{22}\right) \approx 0.20588 \times 0.19628 \approx \mathbf{0.04041}$$
     $$P(D = 0, x) = \left(\frac{81}{102}\right) \times \left(\frac{9}{82}\right) \times \left(\frac{57}{82}\right) \approx 0.79412 \times 0.07632 \approx \mathbf{0.06061}$$
     $$P_{\text{Lap}}(D = 1 \mid x) = \frac{0.04041}{0.04041 + 0.06061} = \frac{0.04041}{0.10102} \approx \mathbf{0.400 \quad (40.0\%) \implies \text{Predict Healthy}}$$
  
  ---
### Question 9: Non-Linear Parametric Wage Modeling
  An econometric labor study fits a polynomial wage model across $10{,}000$ workers:
  $$\widehat{\text{wage}} \; (\$/\text{hour}) = 12.50 + 2.40 \cdot \text{educ} + 0.80 \cdot \text{exper} - 0.02 \cdot (\text{exper}^2) + 4.50 \cdot \text{is\_union}$$
  where `educ` is years of education, `exper` is years of professional experience, and `is_union` is a binary union membership indicator.
  
  **(a)** Predict the hourly wage for a union worker with $16$ years of education and $10$ years of experience.  
  **(b)** Derive the marginal return to experience ($\frac{\partial \widehat{\text{wage}}}{\partial \text{exper}}$), and evaluate the marginal value of an additional year of experience at $\text{exper} = 5$ years versus $\text{exper} = 25$ years.  
  **(c)** At what level of experience does predicted wage peak?
  
  ---
#### Worked Solution:
  * **(a) Compute Point Prediction:**
  Substitute $\text{educ} = 16, \; \text{exper} = 10, \; \text{is\_union} = 1$:
  $$\widehat{\text{wage}} = 12.50 + 2.40(16) + 0.80(10) - 0.02(10^2) + 4.50(1)$$
  $$\widehat{\text{wage}} = 12.50 + 38.40 + 8.00 - 2.00 + 4.50 = \mathbf{\$61.40 \text{ per hour}}$$
  
  * **(b) Derive Marginal Return to Experience:**
  Compute the partial derivative with respect to $\text{exper}$:
  $$\frac{\partial \widehat{\text{wage}}}{\partial \text{exper}} = 0.80 - 2(0.02) \cdot \text{exper} = \mathbf{0.80 - 0.04 \cdot \text{exper}}$$
  1. *At $\text{exper} = 5$ years:*
     $$\frac{\partial \widehat{\text{wage}}}{\partial \text{exper}} = 0.80 - 0.04(5) = 0.80 - 0.20 = \mathbf{+\$0.60/\text{hour per additional year}}$$
  2. *At $\text{exper} = 25$ years:*
     $$\frac{\partial \widehat{\text{wage}}}{\partial \text{exper}} = 0.80 - 0.04(25) = 0.80 - 1.00 = \mathbf{-\$0.20/\text{hour per additional year}}$$
  *(Demonstrates diminishing and eventually depreciating returns to experience).*
  
  * **(c) Peak Earnings Experience:**
  Set the partial derivative to zero:
  $$0.80 - 0.04 \cdot \text{exper}^* = 0 \implies \text{exper}^* = \frac{0.80}{0.04} = \mathbf{20.0 \text{ years}}$$
  
  ---
### Question 10: Causal vs. Observational Policy Auditing
  A health insurance analytics division fits an observational multiple linear regression on $50{,}000$ members:
  $$\widehat{\text{annual\_cost}} \; (\$) = 3{,}500 - 400 \cdot \text{gym\_visits\_per\_month} + 75 \cdot \text{bmi} + 110 \cdot \text{age}$$
  
  An executive proposes:  
  *"Our model proves that each gym visit reduces annual claims by $\$400$. We should subsidize gym memberships for all members. If we mandate 5 visits per month, we will save $5 \times \$400 = \$2{,}000$ per member annually."*
  
  **(a)** State the exact observational interpretation of the $-400$ coefficient.  
  **(b)** Using the causal DAG framework (Lecture 12), identify the unobserved confounder $Z$ that undermines the executive’s intervention plan.  
  **(c)** Why does an observational regression coefficient fail to predict the interventional outcome of a policy change ($P(Y \mid X) \neq P(Y \mid do(X))$)?
  
  ---
#### Worked Solution:
  * **(a) Rigorous Observational Interpretation:**
  *"Comparing two policyholders with the **exact same BMI and age**, an individual who attends the gym one additional time per month is associated with a predicted **$\$400$ lower annual medical cost** on average."*
  * **(b) Confounding Mechanism ($Z$):**
  * The variable `gym_visits_per_month` is heavily confounded by unmeasured factors: **baseline personal health consciousness, intrinsic physical vitality, socioeconomic status, and discretionary free time**.
  * A person who attends the gym five times a month also tends to prepare nutritious meals, avoid tobacco, schedule preventative screenings, and possess higher disposable income ($Z$).
  * The variable `gym_visits` acts as a proxy for healthy lifestyle choices ($X \leftarrow Z \to Y$).
  * **(c) Why the Policy Fails ($do$-Calculus Distinction):**
  * The model evaluates observational data: $\mathbb{E}[\text{Cost} \mid \text{Gym} = 5]$.
  * The policy proposal represents an **active external intervention**: $\mathbb{E}[\text{Cost} \mid do(\text{Gym} = 5)]$.
  * Giving a sedentary individual a subsidized gym membership does not automatically give them the genetic resistance, dietary habits, or baseline cardiovascular health of an athlete. 
  * Because the backdoor path through lifestyle confounders ($Z$) is not blocked, the observational coefficient cannot predict the cost savings of the intervention.
  
  ---
## Exam 1 Review Mastery Checklist
  
  ```
  ┌────────────────────┬──────────────────────────────────────┬──────────────────────────────────────────┐
  │ Competency Area    │ Analytical Formula                   │ Common Exam Pitfall to Avoid             │
  ├────────────────────┼──────────────────────────────────────┼──────────────────────────────────────────┤
  │ OLS Slope          │ w₁ = Cov(x, y) / Var(x)              │ Forgetting to center variables around x̄. │
  │ Gradient Descent   │ w ← w - η ∇J                         │ Using n in denominator for single SGD.   │
  │ Logistic Sigmoid   │ p = 1 / (1 + e⁻ᶻ)                    │ Confusing log-odds (z) with prob (p).    │
  │ Decision Boundary  │ wᵀx + b = 0                          │ Boundary in ℝᵈ is flat, not S-shaped.    │
  │ k-NN Distances     │ D_p(x, y) = (∑ |Δx|ᵖ)^(1/p)          │ Missing square root in Euclidean norm.   │
  │ Naive Bayes Rule   │ argmax P(Y) ∏ P(X_j | Y)             │ Forgetting to normalize joint posteriors.│
  │ Laplace Smoothing  │ (Count + 1) / (N_c + K)              │ Dividing by N_c + 1 instead of N_c + K.  │
  │ Marginal Returns   │ Evaluate ∂ŷ / ∂x_j                   │ Treating non-linear slopes as constants. │
  │ Causal Language    │ Use "is associated with"             │ Never write "causes" on observational.   │
  └────────────────────┴──────────────────────────────────────┴──────────────────────────────────────────┘
  ```