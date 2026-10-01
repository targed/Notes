# Part C4 — Scaling by Hand Practice Problems
#### Problem 1 (Standard Clean Arithmetic)
  Training values of a sensor feature: $[10, \; 20, \; 30, \; 40, \; 50]$.  
  *(Note: Recall that scikit-learn's `StandardScaler` calculates the population standard deviation).*  
  * **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  * **(b)** Standardize an unseen test value of $45$.  
  * **(c)** Min-max scale test values $45$ and $65$.
  
  ---
#### Problem 2 (Negative and Centered Coordinates)
  Training values of daily temperature anomalies ($^\circ\text{C}$): $[-15, \; -5, \; 0, \; 5, \; 15]$.  
  * **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  * **(b)** Standardize a test value of $10$.  
  * **(c)** Min-max scale test values $-20$ (below minimum) and $+20$ (above maximum).
  
  ---
#### Problem 3 (Small Integer Sample)
  Training values of professional experience (years): $[2, \; 4, \; 6, \; 8, \; 10]$.  
  * **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  * **(b)** Standardize a test value of $9$.  
  * **(c)** Min-max scale test values $7$ and $12$.
  
  ---
#### Problem 4 (Skewed Feature with an Outlier)
  Training values of website latency (ms): $[10, \; 12, \; 14, \; 16, \; 48]$.  
  * **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  * **(b)** Standardize a test value of $15$.  
  * **(c)** Min-max scale test values $15$ and $52$. Explain how the outlier ($48$) affects the scaling of the inlier ($15$).
  
  ---
#### Problem 5 (Custom Target Range: `feature_range=(-1, 1)`)
  Training values of an audio signal amplitude: $[100, \; 200, \; 300, \; 400, \; 500]$.  
  * **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  * **(b)** Standardize a test value of $450$.  
  * **(c)** Apply `MinMaxScaler(feature_range=(-1, 1))` to transform test values $250$ and $600$.
  
  ---
#### Problem 6 (Seven-Point Symmetric Distribution)
  Training values of customer satisfaction ratings: $[1, \; 2, \; 3, \; 4, \; 5, \; 6, \; 7]$.  
  * **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  * **(b)** Standardize a test value of $5$.  
  * **(c)** Min-max scale test values $0$ (below minimum) and $8$ (above maximum).
  
  ---
#### Problem 7 (`RobustScaler` Order Statistics by Hand)
  A training set feature contains the following nine sorted values:  
  $$X_{\text{train}} = [4, \; 6, \; 7, \; 9, \; 10, \; 12, \; 14, \; 18, \; 50]$$
  * **(a)** Determine the sample median ($Q_2$), first quartile ($Q_1$), third quartile ($Q_3$), and the Interquartile Range ($\text{IQR}$).  
  * **(b)** Write the explicit `RobustScaler` transformation equation fitted on this training data.  
  * **(c)** Transform a test observation $x_{\text{test}} = 11$.  
  * **(d)** Transform the extreme training outlier $x = 50$.
  
  ---
#### Problem 8 (Decimal Clinical Vitals)
  Training values of patient arterial blood pressure changes: $[0.2, \; 0.4, \; 0.6, \; 0.8, \; 1.0]$.  
  * **(a)** Compute the training mean ($\mu$) and population standard deviation ($\sigma$).  
  * **(b)** Standardize a test value of $0.9$.  
  * **(c)** Min-max scale test values $0.5$ and $1.2$.
  
  ---
#### Problem 9 (Quantifying Preprocessing Leakage Numerically)
  A dataset has training partition $X_{\text{train}} = [10, \; 20, \; 30]$ and test partition $X_{\text{test}} = [40, \; 50]$.  
  * **(a) Clean Pipeline:** Compute $\mu_{\text{train}}$ and $\sigma_{\text{train}}$, then calculate the standardized $z$-score of test instance $x_{\text{test}} = 40$.  
  * **(b) Leaky Pipeline:** Compute $\mu_{\text{global}}$ and $\sigma_{\text{global}}$ across all five combined observations ($[10, 20, 30, 40, 50]$), then calculate the standardized $z$-score of $x_{\text{test}} = 40$.  
  * **(c)** Compare the two $z$-scores and describe the numerical impact of preprocessing leakage.
  
  ---
#### Problem 10 (2D Feature Vector Scaling by Hand)
  A dataset contains three 2D training observations:  
  $$\text{Sample 1: } (10, \; 100), \quad \text{Sample 2: } (20, \; 200), \quad \text{Sample 3: } (30, \; 300)$$
  * **(a)** Compute the mean vector $\mu = [\mu_1, \; \mu_2]^T$ and population standard deviation vector $\sigma = [\sigma_1, \; \sigma_2]^T$.  
  * **(b)** Standardize an unseen test vector $x_{\text{test}} = [25, \; 150]^T$.  
  * **(c)** Min-max scale $x_{\text{test}} = [25, \; 150]^T$ into the default $[0, 1]$ interval.
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C4
### Problem 1 Solution
  * **(a) Mean and Population Standard Deviation ($N = 5$):**
  $$\mu = \frac{10 + 20 + 30 + 40 + 50}{5} = \frac{150}{5} = \mathbf{30.000}$$
  Deviations $(x_i - \mu)$: $\{-20, \; -10, \; 0, \; 10, \; 20\}$  
  Squared deviations: $\{400, \; 100, \; 0, \; 100, \; 400\} \implies \sum (x_i - \mu)^2 = 1{,}000$  
  $$\sigma^2 = \frac{1{,}000}{5} = 200.000 \implies \sigma = \sqrt{200} = 10\sqrt{2} \approx \mathbf{14.142}$$
  * **(b) Standardize $x_{\text{test}} = 45$:**
  $$z = \frac{x_{\text{test}} - \mu}{\sigma} = \frac{45 - 30}{\sqrt{200}} = \frac{15}{14.1421} = \frac{3\sqrt{2}}{4} \approx \mathbf{1.061}$$
  * **(c) Min-Max Scaling ($x_{\min} = 10, \; x_{\max} = 50 \implies \text{range} = 40$):**
  $$x'_{45} = \frac{45 - 10}{50 - 10} = \frac{35}{40} = \frac{7}{8} = \mathbf{0.875}$$
  $$x'_{65} = \frac{65 - 10}{50 - 10} = \frac{55}{40} = \frac{11}{8} = \mathbf{1.375}$$
  *(Note: $1.375 > 1.0$ is expected because test value $65$ exceeds the maximum training bound).*
  
  ---
### Problem 2 Solution
  * **(a) Mean and Population Standard Deviation ($N = 5$):**
  $$\mu = \frac{-15 - 5 + 0 + 5 + 15}{5} = \frac{0}{5} = \mathbf{0.000}$$
  Deviations: $\{-15, \; -5, \; 0, \; 5, \; 15\} \implies \text{Squares: } \{225, \; 25, \; 0, \; 25, \; 225\}$  
  $$\sigma^2 = \frac{500}{5} = 100.000 \implies \sigma = \sqrt{100} = \mathbf{10.000}$$
  * **(b) Standardize $x_{\text{test}} = 10$:**
  $$z = \frac{10 - 0}{10} = \mathbf{1.000}$$
  * **(c) Min-Max Scaling ($x_{\min} = -15, \; x_{\max} = 15 \implies \text{range} = 15 - (-15) = 30$):**
  $$x'_{-20} = \frac{-20 - (-15)}{30} = \frac{-5}{30} = -\frac{1}{6} \approx \mathbf{-0.167}$$
  $$x'_{20} = \frac{20 - (-15)}{30} = \frac{35}{30} = \frac{7}{6} \approx \mathbf{1.167}$$
  
  ---
### Problem 3 Solution
  * **(a) Mean and Population Standard Deviation ($N = 5$):**
  $$\mu = \frac{2 + 4 + 6 + 8 + 10}{5} = \frac{30}{5} = \mathbf{6.000}$$
  Deviations: $\{-4, \; -2, \; 0, \; 2, \; 4\} \implies \text{Squares: } \{16, \; 4, \; 0, \; 4, \; 16\} \implies \text{Sum} = 40$  
  $$\sigma^2 = \frac{40}{5} = 8.000 \implies \sigma = \sqrt{8} = 2\sqrt{2} \approx \mathbf{2.828}$$
  * **(b) Standardize $x_{\text{test}} = 9$:**
  $$z = \frac{9 - 6}{2.8284} = \frac{3}{2\sqrt{2}} = \frac{3\sqrt{2}}{4} \approx \mathbf{1.061}$$
  * **(c) Min-Max Scaling ($x_{\min} = 2, \; x_{\max} = 10 \implies \text{range} = 8$):**
  $$x'_7 = \frac{7 - 2}{8} = \frac{5}{8} = \mathbf{0.625}$$
  $$x'_{12} = \frac{12 - 2}{8} = \frac{10}{8} = \frac{5}{4} = \mathbf{1.250}$$
  
  ---
### Problem 4 Solution
  * **(a) Mean and Population Standard Deviation ($N = 5$):**
  $$\mu = \frac{10 + 12 + 14 + 16 + 48}{5} = \frac{100}{5} = \mathbf{20.000}$$
  Deviations: $\{-10, \; -8, \; -6, \; -4, \; +28\}$  
  Squares: $\{100, \; 64, \; 36, \; 16, \; 784\} \implies \sum = 1{,}000$  
  $$\sigma^2 = \frac{1{,}000}{5} = 200.000 \implies \sigma = \sqrt{200} \approx \mathbf{14.142}$$
  * **(b) Standardize $x_{\text{test}} = 15$:**
  $$z = \frac{15 - 20}{14.142} = \frac{-5}{14.142} \approx \mathbf{-0.354}$$
  * **(c) Min-Max Scaling ($x_{\min} = 10, \; x_{\max} = 48 \implies \text{range} = 38$):**
  $$x'_{15} = \frac{15 - 10}{38} = \frac{5}{38} \approx \mathbf{0.132}$$
  $$x'_{52} = \frac{52 - 10}{38} = \frac{42}{38} = \frac{21}{19} \approx \mathbf{1.105}$$
  *Impact of Outlier:* The extreme value ($48$) expands the denominator ($38$), compressing the inlier $15$ down to $0.132$ (near zero). All normal inliers $[10, 16]$ are crushed into the narrow interval $[0.000, 0.158]$, destroying variance resolution across standard samples.
  
  ---
### Problem 5 Solution
  * **(a) Mean and Population Standard Deviation ($N = 5$):**
  $$\mu = \frac{100 + 200 + 300 + 400 + 500}{5} = \frac{1{,}500}{5} = \mathbf{300.000}$$
  Deviations: $\{-200, \; -100, \; 0, \; 100, \; 200\} \implies \text{Squares: } \{40{,}000, \; 10{,}000, \; 0, \; 10{,}000, \; 40{,}000\}$  
  $$\sum = 100{,}000 \implies \sigma^2 = \frac{100{,}000}{5} = 20{,}000 \implies \sigma = 100\sqrt{2} \approx \mathbf{141.421}$$
  * **(b) Standardize $x_{\text{test}} = 450$:**
  $$z = \frac{450 - 300}{141.421} = \frac{150}{141.421} = \frac{3\sqrt{2}}{4} \approx \mathbf{1.061}$$
  * **(c) Custom Range $[-1, 1]$ ($x_{\min} = 100, \; x_{\max} = 500 \implies \text{range} = 400$):**
  $$x' = a + \frac{x - x_{\min}}{x_{\max} - x_{\min}}(b - a) = -1 + \frac{x - 100}{400}(1 - (-1)) = -1 + \frac{x - 100}{200}$$
  $$x'_{250} = -1 + \frac{250 - 100}{200} = -1 + \frac{150}{200} = -1 + 0.750 = \mathbf{-0.250}$$
  $$x'_{600} = -1 + \frac{600 - 100}{200} = -1 + \frac{500}{200} = -1 + 2.500 = \mathbf{+1.500}$$
  
  ---
### Problem 6 Solution
  * **(a) Mean and Population Standard Deviation ($N = 7$):**
  $$\mu = \frac{1 + 2 + 3 + 4 + 5 + 6 + 7}{7} = \frac{28}{7} = \mathbf{4.000}$$
  Deviations: $\{-3, \; -2, \; -1, \; 0, \; 1, \; 2, \; 3\} \implies \text{Squares: } \{9, \; 4, \; 1, \; 0, \; 1, \; 4, \; 9\} \implies \text{Sum} = 28$  
  $$\sigma^2 = \frac{28}{7} = 4.000 \implies \sigma = \sqrt{4} = \mathbf{2.000}$$
  * **(b) Standardize $x_{\text{test}} = 5$:**
  $$z = \frac{5 - 4}{2} = \mathbf{0.500}$$
  * **(c) Min-Max Scaling ($x_{\min} = 1, \; x_{\max} = 7 \implies \text{range} = 6$):**
  $$x'_0 = \frac{0 - 1}{6} = -\frac{1}{6} \approx \mathbf{-0.167}$$
  $$x'_8 = \frac{8 - 1}{6} = \frac{7}{6} \approx \mathbf{1.167}$$
  
  ---
### Problem 7 Solution
  * **(a) Calculate Order Statistics ($N = 9$):**
  Sorted array: $[4, \; 6, \; 7, \; 9, \; \mathbf{10}, \; 12, \; 14, \; 18, \; 50]$
  1. *Median ($Q_2$):* 5th value $\implies \mathbf{Q_2 = 10.000}$
  2. *First Quartile ($Q_1$):* Median of lower half $[4, 6, 7, 9] \implies \frac{6 + 7}{2} = \mathbf{6.500}$
  3. *Third Quartile ($Q_3$):* Median of upper half $[12, 14, 18, 50] \implies \frac{14 + 18}{2} = \mathbf{16.000}$
  4. *Interquartile Range:* $\text{IQR} = Q_3 - Q_1 = 16.0 - 6.5 = \mathbf{9.500}$
  * **(b) `RobustScaler` Equation:**
  $$\mathbf{x_{\text{robust}} = \frac{x - 10.000}{9.500}}$$
  * **(c) Transform $x_{\text{test}} = 11$:**
  $$x_{\text{robust}}(11) = \frac{11 - 10}{9.5} = \frac{1}{9.5} \approx \mathbf{0.105}$$
  * **(d) Transform Outlier $x = 50$:**
  $$x_{\text{robust}}(50) = \frac{50 - 10}{9.5} = \frac{40}{9.5} \approx \mathbf{4.211}$$
  
  ---
### Problem 8 Solution
  * **(a) Mean and Population Standard Deviation ($N = 5$):**
  $$\mu = \frac{0.2 + 0.4 + 0.6 + 0.8 + 1.0}{5} = \frac{3.0}{5} = \mathbf{0.600}$$
  Deviations: $\{-0.4, \; -0.2, \; 0.0, \; +0.2, \; +0.4\} \implies \text{Squares: } \{0.16, \; 0.04, \; 0.00, \; 0.04, \; 0.16\}$  
  $$\sum = 0.400 \implies \sigma^2 = \frac{0.400}{5} = 0.080 \implies \sigma = \sqrt{0.08} \approx \mathbf{0.283}$$
  * **(b) Standardize $x_{\text{test}} = 0.9$:**
  $$z = \frac{0.9 - 0.6}{\sqrt{0.08}} = \frac{0.3}{0.2828} \approx \mathbf{1.061}$$
  * **(c) Min-Max Scaling ($x_{\min} = 0.2, \; x_{\max} = 1.0 \implies \text{range} = 0.8$):**
  $$x'_{0.5} = \frac{0.5 - 0.2}{0.8} = \frac{0.3}{0.8} = \frac{3}{8} = \mathbf{0.375}$$
  $$x'_{1.2} = \frac{1.2 - 0.2}{0.8} = \frac{1.0}{0.8} = \frac{5}{4} = \mathbf{1.250}$$
  
  ---
### Problem 9 Solution
  * **(a) Clean Pipeline (Train Statistics Only):**
  * $X_{\text{train}} = [10, 20, 30] \implies \mu_{\text{train}} = \frac{60}{3} = \mathbf{20.000}$
  * Deviations: $\{-10, 0, 10\} \implies \sum = 200 \implies \sigma^2 = \frac{200}{3} \implies \sigma_{\text{train}} = \sqrt{66.667} \approx \mathbf{8.165}$
  * Transform $x_{\text{test}} = 40$:
    $$z_{\text{clean}} = \frac{40 - 20.000}{8.165} = \frac{20}{8.165} \approx \mathbf{2.449}$$
  * **(b) Leaky Pipeline (Combined Train + Test Statistics):**
  * $X_{\text{all}} = [10, 20, 30, 40, 50] \implies \mu_{\text{global}} = \frac{150}{5} = \mathbf{30.000}$
  * $\sigma^2_{\text{global}} = \frac{1000}{5} = 200 \implies \sigma_{\text{global}} = \sqrt{200} \approx \mathbf{14.142}$
  * Transform $x_{\text{test}} = 40$:
    $$z_{\text{leaky}} = \frac{40 - 30.000}{14.142} = \frac{10}{14.142} \approx \mathbf{0.707}$$
  * **(c) Numerical Impact:**
  * Clean: $z_{\text{clean}} = 2.449$ correctly signals that $40$ is an out-of-distribution tail observation ($>2\sigma$ away from the training mean).
  * Leaky: $z_{\text{leaky}} = 0.707$ artificially pulls the test point inward ($<1\sigma$). Including the test data in the scaler inflated the variance and shifted the mean, masking the out-of-sample novelty of the test point.
  
  ---
### Problem 10 Solution
  * **(a) Mean and Standard Deviation Vectors:**
  * Feature 1: $[10, 20, 30] \implies \mu_1 = \mathbf{20.000}, \quad \sigma_1 = \sqrt{\frac{200}{3}} \approx \mathbf{8.165}$
  * Feature 2: $[100, 200, 300] \implies \mu_2 = \mathbf{200.000}, \quad \sigma_2 = \sqrt{\frac{20{,}000}{3}} \approx \mathbf{81.650}$
  * $\mathbf{\mu = [20.000, \; 200.000]^T}, \quad \mathbf{\sigma = [8.165, \; 81.650]^T}$
  * **(b) Standardize $x_{\text{test}} = [25, 150]^T$:**
  $$z_1 = \frac{25 - 20}{8.165} = \frac{5}{8.165} \approx \mathbf{0.612}$$
  $$z_2 = \frac{150 - 200}{81.650} = \frac{-50}{81.650} \approx \mathbf{-0.612}$$
  $$\mathbf{z_{\text{test}} = [0.612, \; -0.612]^T}$$
  * **(c) Min-Max Scaling ($x_{\min} = [10, 100]^T, \; x_{\max} = [30, 300]^T \implies \text{ranges} = [20, 200]^T$):**
  $$x'_1 = \frac{25 - 10}{20} = \frac{15}{20} = \mathbf{0.750}$$
  $$x'_2 = \frac{150 - 100}{200} = \frac{50}{200} = \mathbf{0.250}$$
  $$\mathbf{x'_{\text{test}} = [0.750, \; 0.250]^T}$$
  
  ---