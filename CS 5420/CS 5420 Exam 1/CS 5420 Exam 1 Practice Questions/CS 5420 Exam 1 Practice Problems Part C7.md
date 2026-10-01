# Part C7 — Least Squares & Gradient Steps Practice Problems
#### Problem 1 (Standard Clean Coordinate Mirror)
  Given $n = 3$ data points:
  $$(x_1, y_1) = (1, 2), \quad (x_2, y_2) = (2, 3), \quad (x_3, y_3) = (3, 7)$$
  Model: $\hat{y} = w_0 + w_1 x$  
  Cost function: $J(w_0, w_1) = \frac{1}{n} \sum_{i=1}^n (\hat{y}_i - y_i)^2$
  * **(a)** Compute the sample means $\bar{x}$ and $\bar{y}$. Find the analytical OLS slope $w_1$ and intercept $w_0$, and write the fitted equation $\hat{y}$.
  * **(b)** Starting from $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$ with learning rate $\eta = 0.10$, compute **ONE parameter update step of Batch Gradient Descent**.
  * **(c)** Starting from $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$ with $\eta = 0.10$, compute **ONE parameter update step of Stochastic Gradient Descent** using only the third sample $(x_3 = 3, y_3 = 7)$.
  
  ---
#### Problem 2 (Non-Zero Initial Parameter Weights)
  Given $n = 4$ observations:
  $$(x_1, y_1) = (1, 2), \quad (x_2, y_2) = (2, 3), \quad (x_3, y_3) = (3, 5), \quad (x_4, y_4) = (4, 6)$$
  * **(a)** Solve analytically for the Ordinary Least Squares parameters $w_0$ and $w_1$.
  * **(b)** Starting from non-zero weights **$w_0^{(0)} = 1.0, \; w_1^{(0)} = 1.0$** with learning rate $\eta = 0.05$, compute **ONE parameter update step of Batch Gradient Descent**.
  * **(c)** Starting from $w_0^{(0)} = 1.0, \; w_1^{(0)} = 1.0$ with $\eta = 0.05$, compute **ONE parameter update step of Stochastic Gradient Descent** using only the first sample $(x_1 = 1, y_1 = 2)$.
  
  ---
#### Problem 3 (Regression Through the Origin — No Intercept)
  A physical model dictates that when $x = 0$, $y$ must strictly equal $0$. The model has no intercept: $\hat{y} = w_1 x$.  
  We observe $n = 3$ data points:
  $$(x_1, y_1) = (1, 2), \quad (x_2, y_2) = (2, 5), \quad (x_3, y_3) = (3, 8)$$
  Cost function: $J(w_1) = \frac{1}{n} \sum_{i=1}^n (w_1 x_i - y_i)^2$
  * **(a)** Derive the analytical least-squares closed-form formula for $w_1^*$, and calculate its numerical value.
  * **(b)** Starting from $w_1^{(0)} = 0.0$ with $\eta = 0.10$, compute **ONE Batch Gradient Descent step**.
  * **(c)** Starting from $w_1^{(0)} = 0.0$ with $\eta = 0.10$, compute **ONE Stochastic Gradient Descent step** using only sample 2 $(x_2 = 2, y_2 = 5)$.
  
  ---
#### Problem 4 (Mini-Batch Gradient Step — $B = 2$)
  A dataset has $n = 4$ points:
  $$(x_1, y_1) = (1, 3), \quad (x_2, y_2) = (2, 3), \quad (x_3, y_3) = (3, 5), \quad (x_4, y_4) = (4, 9)$$
  Model: $\hat{y} = w_0 + w_1 x$
  * **(a)** Solve analytically for the OLS parameters $w_0$ and $w_1$.
  * **(b)** Starting from $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$ with learning rate $\eta = 0.05$, compute **ONE Mini-Batch Gradient Descent step** evaluated strictly on the 2-sample batch $\mathcal{B} = \{(1, 3), \; (2, 3)\}$.
  * **(c)** State the mathematical formula for the mini-batch gradient and explain why it divides by $B = 2$ rather than $n = 4$.
  
  ---
#### Problem 5 (Matrix Normal Equations $X^T X w = X^T y$ by Hand)
  Given $n = 3$ observations:
  $$(x_1, y_1) = (1, 1), \quad (x_2, y_2) = (2, 2), \quad (x_3, y_3) = (3, 4)$$
  * **(a)** Construct the augmented design matrix $X \in \mathbb{R}^{3 \times 2}$ and target vector $y \in \mathbb{R}^3$.
  * **(b)** Compute the Gram matrix $X^T X$ and projection vector $X^T y$ explicitly by matrix multiplication.
  * **(c)** Invert $X^T X$ using the $2 \times 2$ matrix inverse formula, and compute $w^* = (X^T X)^{-1} X^T y$.
  * **(d)** Verify that $w_1 = \frac{\widehat{\text{Cov}}(x, y)}{\widehat{\text{Var}}(x)}$ yields the identical slope.
  
  ---
#### Problem 6 (Ridge Regularization Gradient Step by Hand)
  A regularized linear model penalizes slope complexity:
  $$J(w_0, w_1) = \frac{1}{n} \sum_{i=1}^n (\hat{y}_i - y_i)^2 + \lambda w_1^2$$
  We observe $n = 2$ training points: $(x_1, y_1) = (1, 2)$ and $(x_2, y_2) = (2, 5)$.  
  Let regularization parameter $\lambda = 0.50$.
  * **(a)** Derive the gradient expressions $\nabla_{w_0} J$ and $\nabla_{w_1} J$.
  * **(b)** Starting from initial parameters $w_0^{(0)} = 0.0, \; w_1^{(0)} = 1.0$ with learning rate $\eta = 0.10$, compute **ONE parameter update step of Batch Gradient Descent**.
  
  ---
#### Problem 7 (Residual Verification & The Centroid Property)
  Given $n = 3$ observations:
  $$(x_1, y_1) = (2, 3), \quad (x_2, y_2) = (4, 7), \quad (x_3, y_3) = (6, 8)$$
  * **(a)** Fit the analytical OLS line $\hat{y} = w_0 + w_1 x$.
  * **(b)** Calculate the individual residuals $r_i = y_i - \hat{y}_i$ for all three observations.
  * **(c)** Prove numerically that the residuals sum to zero ($\sum_{i=1}^3 r_i = 0$) and that the residuals are orthogonal to the feature coordinates ($\sum_{i=1}^3 x_i r_i = 0$).
  
  ---
#### Problem 8 (Negative Slopes & Inverse Relationships)
  An inverse relationship has $n = 4$ observations:
  $$(x_1, y_1) = (1, 9), \quad (x_2, y_2) = (2, 7), \quad (x_3, y_3) = (3, 6), \quad (x_4, y_4) = (4, 2)$$
  * **(a)** Find the analytical OLS line $\hat{y} = w_0 + w_1 x$.
  * **(b)** Starting from initial parameters $w_0^{(0)} = 10.0, \; w_1^{(0)} = 0.0$ with learning rate $\eta = 0.05$, compute **ONE Batch Gradient Descent step**.
  * **(c)** Starting from $w_0^{(0)} = 10.0, \; w_1^{(0)} = 0.0$ with $\eta = 0.05$, compute **ONE Stochastic Gradient Descent step** using only sample 4 $(x_4 = 4, y_4 = 2)$.
  
  ---
#### Problem 9 (Step-Size Disparity Between Samples in SGD)
  A simple dataset consists of two points: $(x_1, y_1) = (2, 4)$ and $(x_2, y_2) = (4, 8)$ ($n = 2$).  
  Model: $\hat{y} = w_0 + w_1 x$.  
  Starting weights: $w_0^{(0)} = 0.0, \; w_1^{(0)} = 0.0$, learning rate $\eta = 0.05$.
  * **(a)** Compute updated weights after **ONE Batch GD step**.
  * **(b)** Compute updated weights after **ONE SGD step on sample 1** $(x=2, y=4)$.
  * **(c)** Compute updated weights after **ONE SGD step on sample 2** $(x=4, y=8)$.
  * **(d)** Compare the magnitude of the slope update $\Delta w_1$ between sample 1 and sample 2. Why is sample 2's update four times larger?
  
  ---
#### Problem 10 (Hessian Eigenvalues & Theoretical Stability Bounds)
  Consider an OLS problem with $n = 2$ samples where the design matrix is:
  $$X = \begin{bmatrix} 1 & 1 \\ 1 & 3 \end{bmatrix}$$
  The cost function is $J(w) = \frac{1}{n} \|Xw - y\|_2^2$.
  * **(a)** Compute the Hessian matrix $H = \nabla_w^2 J(w) = \frac{2}{n} X^T X$.
  * **(b)** Find the eigenvalues $\lambda_{\max}$ and $\lambda_{\min}$ of the Hessian $H$.
  * **(c)** What is the exact theoretical maximum learning rate $\eta_{\max} = \frac{2}{\lambda_{\max}}$ before Batch Gradient Descent diverges?
  * **(d)** What is the theoretically optimal learning rate $\eta^* = \frac{2}{\lambda_{\max} + \lambda_{\min}}$?
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C7
### Problem 1 Solution
  * **(a) OLS Analytical Solve:**
  1. *Centroids:*
     $$\bar{x} = \frac{1 + 2 + 3}{3} = \mathbf{2.000}, \quad \bar{y} = \frac{2 + 3 + 7}{3} = \mathbf{4.000}$$
  2. *Deviations & Products:*
     * $(x_i - \bar{x}) \in \{-1.0, \; 0.0, \; +1.0\} \implies S_{xx} = (-1)^2 + 0^2 + 1^2 = \mathbf{2.000}$
     * $(y_i - \bar{y}) \in \{-2.0, \; -1.0, \; +3.0\}$
     * $S_{xy} = (-1.0)(-2.0) + (0.0)(-1.0) + (1.0)(3.0) = 2.0 + 0.0 + 3.0 = \mathbf{5.000}$
  3. *Parameters:*
     $$w_1 = \frac{S_{xy}}{S_{xx}} = \frac{5.000}{2.000} = \mathbf{2.500}$$
     $$w_0 = \bar{y} - w_1 \bar{x} = 4.000 - (2.500 \times 2.000) = \mathbf{-1.000}$$
     $$\mathbf{\hat{y} = -1.000 + 2.500x}$$
  
  * **(b) One Batch GD Step ($w_0 = 0, w_1 = 0, \eta = 0.10, n = 3$):**
  * Predictions: $\hat{y}_i = 0 \implies e_i = \hat{y}_i - y_i = -y_i$: $e = [-2, \; -3, \; -7]^T$
  * Gradients:
    $$\nabla_{w_0} J = \frac{2}{3}(-2 - 3 - 7) = \frac{2}{3}(-12) = \mathbf{-8.000}$$
    $$\nabla_{w_1} J = \frac{2}{3}[1(-2) + 2(-3) + 3(-7)] = \frac{2}{3}[-2 - 6 - 21] = \frac{2}{3}(-29) \approx \mathbf{-19.333}$$
  * Updates:
    $$w_0^{(1)} = 0 - 0.10(-8.000) = \mathbf{+0.800}$$
    $$w_1^{(1)} = 0 - 0.10(-19.333) = \mathbf{+1.933}$$
  
  * **(c) One SGD Step on Sample 3 $(x_3 = 3, y_3 = 7)$:**
  * Prediction: $\hat{y}_3 = 0 \implies e_3 = 0 - 7 = -7$
  * Gradients:
    $$\nabla_{w_0} \mathcal{L}_3 = 2(e_3) = 2(-7) = \mathbf{-14.000}$$
    $$\nabla_{w_1} \mathcal{L}_3 = 2 x_3 (e_3) = 2(3)(-7) = \mathbf{-42.000}$$
  * Updates:
    $$w_0^{(1)} = 0 - 0.10(-14.000) = \mathbf{+1.400}$$
    $$w_1^{(1)} = 0 - 0.10(-42.000) = \mathbf{+4.200}$$
  
  ---
### Problem 2 Solution
  * **(a) OLS Analytical Solve:**
  * $\bar{x} = \frac{10}{4} = 2.500, \quad \bar{y} = \frac{16}{4} = 4.000$
  * $S_{xx} = (-1.5)^2 + (-0.5)^2 + (0.5)^2 + (1.5)^2 = 2.25 + 0.25 + 0.25 + 2.25 = \mathbf{5.000}$
  * $(y_i - \bar{y}) \in \{-2, \; -1, \; +1, \; +2\}$
  * $S_{xy} = (-1.5)(-2) + (-0.5)(-1) + (0.5)(1) + (1.5)(2) = 3.0 + 0.5 + 0.5 + 3.0 = \mathbf{7.000}$
  * $w_1 = \frac{7.000}{5.000} = \mathbf{1.400}, \quad w_0 = 4.000 - 1.400(2.500) = \mathbf{0.500} \implies \mathbf{\hat{y} = 0.500 + 1.400x}$
  
  * **(b) One Batch GD Step from $w_0 = 1.0, w_1 = 1.0$ ($\eta = 0.05$):**
  * Predictions: $\hat{y}_i = 1.0 + 1.0 x_i$:
    * $\hat{y}_1 = 2.0 \implies e_1 = 2 - 2 = 0.0$
    * $\hat{y}_2 = 3.0 \implies e_2 = 3 - 3 = 0.0$
    * $\hat{y}_3 = 4.0 \implies e_3 = 4 - 5 = -1.0$
    * $\hat{y}_4 = 5.0 \implies e_4 = 5 - 6 = -1.0$
  * Gradients:
    $$\nabla_{w_0} J = \frac{2}{4}(0 + 0 - 1 - 1) = \frac{1}{2}(-2) = \mathbf{-1.000}$$
    $$\nabla_{w_1} J = \frac{2}{4}[1(0) + 2(0) + 3(-1) + 4(-1)] = \frac{1}{2}(-7) = \mathbf{-3.500}$$
  * Updates:
    $$w_0^{(1)} = 1.0 - 0.05(-1.000) = 1.0 + 0.050 = \mathbf{1.050}$$
    $$w_1^{(1)} = 1.0 - 0.05(-3.500) = 1.0 + 0.175 = \mathbf{1.175}$$
  
  * **(c) One SGD Step on Sample 1 $(x_1 = 1, y_1 = 2)$ from $w_0 = 1.0, w_1 = 1.0$:**
  * $\hat{y}_1 = 1.0 + 1.0(1) = 2.0 \implies e_1 = 2 - 2 = 0.0$
  * Gradients:
    $$\nabla_{w_0} \mathcal{L}_1 = 2(0) = 0.0, \quad \nabla_{w_1} \mathcal{L}_1 = 2(1)(0) = 0.0$$
  * Updates:
    $$w_0^{(1)} = 1.0 - 0.05(0.0) = \mathbf{1.000}, \quad w_1^{(1)} = 1.0 - 0.05(0.0) = \mathbf{1.000}$$
  * *(Sample 1 is predicted with zero error, so SGD executes zero update).*
  
  ---
### Problem 3 Solution
  * **(a) Derivation for Regression Through the Origin:**
  $$J(w_1) = \frac{1}{n}\sum_{i=1}^n (w_1 x_i - y_i)^2$$
  $$\frac{dJ}{dw_1} = \frac{2}{n}\sum_{i=1}^n x_i (w_1 x_i - y_i) = 0 \implies w_1 \sum_{i=1}^n x_i^2 = \sum_{i=1}^n x_i y_i \implies \mathbf{w_1^* = \frac{\sum x_i y_i}{\sum x_i^2}}$$
  * $\sum x_i^2 = 1^2 + 2^2 + 3^2 = 1 + 4 + 9 = 14.0$
  * $\sum x_i y_i = 1(2) + 2(5) + 3(8) = 2 + 10 + 24 = 36.0$
  * $w_1^* = \frac{36.0}{14.0} = \frac{18}{7} \approx \mathbf{2.571}$
  
  * **(b) One Batch GD Step ($w_1^{(0)} = 0, \eta = 0.10$):**
  * $e_i = 0 - y_i \implies e = [-2, \; -5, \; -8]^T$
  * $\nabla_{w_1} J = \frac{2}{3}[1(-2) + 2(-5) + 3(-8)] = \frac{2}{3}(-36) = \mathbf{-24.000}$
  * $w_1^{(1)} = 0 - 0.10(-24.000) = \mathbf{+2.400}$
  
  * **(c) One SGD Step on Sample 2 $(x_2 = 2, y_2 = 5)$:**
  * $e_2 = 0 - 5 = -5$
  * $\nabla_{w_1} \mathcal{L}_2 = 2 x_2 e_2 = 2(2)(-5) = \mathbf{-20.000}$
  * $w_1^{(1)} = 0 - 0.10(-20.000) = \mathbf{+2.000}$
  
  ---
### Problem 4 Solution
  * **(a) Analytical OLS Parameters:**
  * $\bar{x} = 2.500, \quad \bar{y} = \frac{3 + 3 + 5 + 9}{4} = \frac{20}{4} = 5.000$
  * $S_{xx} = 5.000$
  * $(y_i - \bar{y}) \in \{-2, \; -2, \; 0, \; +4\}$
  * $S_{xy} = (-1.5)(-2) + (-0.5)(-2) + (0.5)(0) + (1.5)(4) = 3.0 + 1.0 + 0.0 + 6.0 = 10.000$
  * $w_1 = \frac{10.000}{5.000} = \mathbf{2.000}, \quad w_0 = 5.000 - 2.000(2.500) = \mathbf{0.000} \implies \mathbf{\hat{y} = 2.000x}$
  
  * **(b) One Mini-Batch GD Step on $\mathcal{B} = \{(1, 3), (2, 3)\}$ ($B = 2, \eta = 0.05$):**
  * Predictions: $\hat{y}_1 = 0 \implies e_1 = -3, \quad \hat{y}_2 = 0 \implies e_2 = -3$
  * Gradients:
    $$\nabla_{w_0} J_{\mathcal{B}} = \frac{2}{B}\sum_{i \in \mathcal{B}} e_i = \frac{2}{2}(-3 - 3) = \mathbf{-6.000}$$
    $$\nabla_{w_1} J_{\mathcal{B}} = \frac{2}{B}\sum_{i \in \mathcal{B}} x_i e_i = \frac{2}{2}[1(-3) + 2(-3)] = -3 - 6 = \mathbf{-9.000}$$
  * Updates:
    $$w_0^{(1)} = 0 - 0.05(-6.000) = \mathbf{+0.300}$$
    $$w_1^{(1)} = 0 - 0.05(-9.000) = \mathbf{+0.450}$$
  
  * **(c) Formula Justification:**
  $$\nabla J_{\mathcal{B}} = \frac{2}{B}\sum_{i \in \mathcal{B}} e_i \tilde{x}_i$$
  It divides by batch size $B$ because the cost function is defined as the **mean loss across that mini-batch**: $J_{\mathcal{B}} = \frac{1}{B}\sum_{i \in \mathcal{B}} \mathcal{L}_i$.
  
  ---
### Problem 5 Solution
  * **(a) Augmented Design Matrix and Target Vector:**
  $$X = \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix}, \quad y = \begin{bmatrix} 1 \\ 2 \\ 4 \end{bmatrix}$$
  
  * **(b) Compute $X^T X$ and $X^T y$:**
  $$X^T X = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix} \begin{bmatrix} 1 & 1 \\ 1 & 2 \\ 1 & 3 \end{bmatrix} = \begin{bmatrix} 1+1+1 & 1+2+3 \\ 1+2+3 & 1+4+9 \end{bmatrix} = \mathbf{\begin{bmatrix} 3 & 6 \\ 6 & 14 \end{bmatrix}}$$
  $$X^T y = \begin{bmatrix} 1 & 1 & 1 \\ 1 & 2 & 3 \end{bmatrix} \begin{bmatrix} 1 \\ 2 \\ 4 \end{bmatrix} = \begin{bmatrix} 1+2+4 \\ 1(1) + 2(2) + 3(4) \end{bmatrix} = \mathbf{\begin{bmatrix} 7 \\ 17 \end{bmatrix}}$$
  
  * **(c) Invert and Solve:**
  $$\det(X^T X) = (3)(14) - (6)(6) = 42 - 36 = 6.000$$
  $$(X^T X)^{-1} = \frac{1}{6}\begin{bmatrix} 14 & -6 \\ -6 & 3 \end{bmatrix}$$
  $$w^* = \frac{1}{6}\begin{bmatrix} 14 & -6 \\ -6 & 3 \end{bmatrix} \begin{bmatrix} 7 \\ 17 \end{bmatrix} = \frac{1}{6}\begin{bmatrix} 14(7) - 6(17) \\ -6(7) + 3(17) \end{bmatrix} = \frac{1}{6}\begin{bmatrix} 98 - 102 \\ -42 + 51 \end{bmatrix} = \frac{1}{6}\begin{bmatrix} -4 \\ 9 \end{bmatrix} = \mathbf{\begin{bmatrix} -0.667 \\ 1.500 \end{bmatrix}}$$
  $$\mathbf{w_0 = -0.667, \quad w_1 = 1.500}$$
  
  * **(d) Scalar Verification:**
  * $\bar{x} = 2.000, \quad \bar{y} = \frac{7}{3} \approx 2.333$
  * $S_{xx} = (-1)^2 + 0^2 + 1^2 = 2.000$
  * $(y_i - \bar{y}) \in \left\{ -\frac{4}{3}, \; -\frac{1}{3}, \; +\frac{5}{3} \right\}$
  * $S_{xy} = (-1)\left(-\frac{4}{3}\right) + (0)\left(-\frac{1}{3}\right) + (1)\left(\frac{5}{3}\right) = \frac{4}{3} + \frac{5}{3} = \frac{9}{3} = 3.000$
  * $w_1 = \frac{3.000}{2.000} = \mathbf{1.500}, \quad w_0 = \frac{7}{3} - 1.500(2.000) = \frac{7}{3} - 3 = -\frac{2}{3} \approx \mathbf{-0.667}$ (Matches).
  
  ---
### Problem 6 Solution
  * **(a) Deriving Ridge Gradient Expressions ($n = 2$):**
  $$J = \frac{1}{n}\sum_{i=1}^n (\hat{y}_i - y_i)^2 + \lambda w_1^2$$
  $$\nabla_{w_0} J = \mathbf{\frac{2}{n}\sum_{i=1}^n (\hat{y}_i - y_i)}$$
  $$\nabla_{w_1} J = \mathbf{\frac{2}{n}\sum_{i=1}^n x_i (\hat{y}_i - y_i) + 2\lambda w_1}$$
  
  * **(b) One Batch GD Step ($w_0 = 0.0, w_1 = 1.0, \lambda = 0.50, \eta = 0.10$):**
  * Predictions:
    * $\hat{y}_1 = 0 + 1.0(1) = 1.0 \implies e_1 = 1 - 2 = -1.0$
    * $\hat{y}_2 = 0 + 1.0(2) = 2.0 \implies e_2 = 2 - 5 = -3.0$
  * Gradients:
    $$\nabla_{w_0} J = \frac{2}{2}(-1.0 - 3.0) = \mathbf{-4.000}$$
    $$\nabla_{w_1} J = \frac{2}{2}[1(-1.0) + 2(-3.0)] + 2(0.50)(1.0) = [-1.0 - 6.0] + 1.0 = -7.0 + 1.0 = \mathbf{-6.000}$$
  * Updates:
    $$w_0^{(1)} = 0.0 - 0.10(-4.000) = \mathbf{+0.400}$$
    $$w_1^{(1)} = 1.0 - 0.10(-6.000) = \mathbf{+1.600}$$
  
  ---
### Problem 7 Solution
  * **(a) Fit OLS Line ($n = 3$):**
  * $\bar{x} = \frac{2 + 4 + 6}{3} = 4.000, \quad \bar{y} = \frac{3 + 7 + 8}{3} = 6.000$
  * $S_{xx} = (-2)^2 + 0^2 + 2^2 = 4 + 0 + 4 = \mathbf{8.000}$
  * $(y_i - \bar{y}) \in \{-3.0, \; +1.0, \; +2.0\}$
  * $S_{xy} = (-2.0)(-3.0) + (0.0)(1.0) + (2.0)(2.0) = 6.0 + 0.0 + 4.0 = \mathbf{10.000}$
  * $w_1 = \frac{10.000}{8.000} = \mathbf{1.250}, \quad w_0 = 6.000 - 1.250(4.000) = \mathbf{1.000} \implies \mathbf{\hat{y} = 1.000 + 1.250x}$
  
  * **(b) Individual Residuals:**
  * $r_1 = y_1 - \hat{y}(2) = 3 - (1.0 + 1.25(2)) = 3 - 3.5 = \mathbf{-0.500}$
  * $r_2 = y_2 - \hat{y}(4) = 7 - (1.0 + 1.25(4)) = 7 - 6.0 = \mathbf{+1.000}$
  * $r_3 = y_3 - \hat{y}(6) = 8 - (1.0 + 1.25(6)) = 8 - 8.5 = \mathbf{-0.500}$
  
  * **(c) Invariant Verification:**
  $$\sum r_i = -0.500 + 1.000 - 0.500 = \mathbf{0.000 \quad (\text{Zero-Sum Verified})}$$
  $$\sum x_i r_i = 2(-0.500) + 4(1.000) + 6(-0.500) = -1.0 + 4.0 - 3.0 = \mathbf{0.000 \quad (\text{Orthogonality Verified})}$$
  
  ---
### Problem 8 Solution
  * **(a) Analytical Fit ($n = 4$):**
  * $\bar{x} = 2.500, \quad \bar{y} = \frac{9 + 7 + 6 + 2}{4} = 6.000$
  * $S_{xx} = 5.000$
  * $(y_i - \bar{y}) \in \{+3.0, \; +1.0, \; 0.0, \; -4.0\}$
  * $S_{xy} = (-1.5)(3.0) + (-0.5)(1.0) + (0.5)(0.0) + (1.5)(-4.0) = -4.5 - 0.5 + 0.0 - 6.0 = \mathbf{-11.000}$
  * $w_1 = \frac{-11.000}{5.000} = \mathbf{-2.200}, \quad w_0 = 6.000 - (-2.200)(2.500) = 6.0 + 5.5 = \mathbf{11.500}$
  * $\mathbf{\hat{y} = 11.500 - 2.200x}$
  
  * **(b) One Batch GD Step from $w_0 = 10.0, w_1 = 0.0$ ($\eta = 0.05$):**
  * Predictions $\hat{y}_i = 10.0 \implies e_i = 10.0 - y_i$: $e = [+1, \; +3, \; +4, \; +8]^T$
  * Gradients:
    $$\nabla_{w_0} J = \frac{2}{4}(1 + 3 + 4 + 8) = \frac{1}{2}(16) = \mathbf{+8.000}$$
    $$\nabla_{w_1} J = \frac{2}{4}[1(1) + 2(3) + 3(4) + 4(8)] = \frac{1}{2}[1 + 6 + 12 + 32] = \frac{1}{2}(51) = \mathbf{+25.500}$$
  * Updates:
    $$w_0^{(1)} = 10.0 - 0.05(8.000) = \mathbf{9.600}$$
    $$w_1^{(1)} = 0.0 - 0.05(25.500) = \mathbf{-1.275}$$
  
  * **(c) One SGD Step on Sample 4 $(x_4 = 4, y_4 = 2)$:**
  * $\hat{y}_4 = 10.0 \implies e_4 = 10 - 2 = +8.0$
  * $\nabla_{w_0} \mathcal{L}_4 = 2(8.0) = \mathbf{+16.000}, \quad \nabla_{w_1} \mathcal{L}_4 = 2(4)(8.0) = \mathbf{+64.000}$
  * Updates:
    $$w_0^{(1)} = 10.0 - 0.05(16.000) = \mathbf{9.200}$$
    $$w_1^{(1)} = 0.0 - 0.05(64.000) = \mathbf{-3.200}$$
  
  ---
### Problem 9 Solution
  * **(a) One Batch GD Step ($n = 2, w_0=0, w_1=0, \eta=0.05$):**
  * $e_1 = -4, \; e_2 = -8$
  * $\nabla_{w_0} J = \frac{2}{2}(-4 - 8) = -12.000 \implies w_0^{(1)} = 0 - 0.05(-12.0) = \mathbf{0.600}$
  * $\nabla_{w_1} J = \frac{2}{2}[2(-4) + 4(-8)] = -40.000 \implies w_1^{(1)} = 0 - 0.05(-40.0) = \mathbf{2.000}$
  
  * **(b) One SGD Step on Sample 1 $(2, 4)$:**
  * $e_1 = -4$
  * $\nabla_{w_0} = 2(-4) = -8.000 \implies w_0^{(1)} = 0 - 0.05(-8.0) = \mathbf{0.400}$
  * $\nabla_{w_1} = 2(2)(-4) = -16.000 \implies w_1^{(1)} = 0 - 0.05(-16.0) = \mathbf{0.800}$
  
  * **(c) One SGD Step on Sample 2 $(4, 8)$:**
  * $e_2 = -8$
  * $\nabla_{w_0} = 2(-8) = -16.000 \implies w_0^{(1)} = 0 - 0.05(-16.0) = \mathbf{0.800}$
  * $\nabla_{w_1} = 2(4)(-8) = -64.000 \implies w_1^{(1)} = 0 - 0.05(-64.0) = \mathbf{3.200}$
  
  * **(d) Why Sample 2's Slope Update Is $4\times$ Larger:**
  * The single-sample gradient is:
    $$\nabla_{w_1} \mathcal{L}_i = 2 x_i (\hat{y}_i - y_i) = 2 x_i e_i$$
  * Sample 2 has **$2\times$ the residual error** ($e_2 = -8$ vs. $e_1 = -4$) AND sits at **$2\times$ the feature coordinate** ($x_2 = 4$ vs. $x_1 = 2$).
  * The gradient scales as the product $x_i \cdot e_i$, compounding to:
    $$2 \times 2 = \mathbf{4\times \text{ larger update step}} \quad (-64.0 \text{ vs. } -16.0)$$
  * This illustrates the extreme update variance of SGD when encountering high-leverage samples.
  
  ---
### Problem 10 Solution
  * **(a) Compute Hessian Matrix ($n = 2$):**
  $$X^T X = \begin{bmatrix} 1 & 1 \\ 1 & 3 \end{bmatrix}^T \begin{bmatrix} 1 & 1 \\ 1 & 3 \end{bmatrix} = \begin{bmatrix} 1+1 & 1+3 \\ 1+3 & 1+9 \end{bmatrix} = \begin{bmatrix} 2 & 4 \\ 4 & 10 \end{bmatrix}$$
  $$H = \frac{2}{n} X^T X = \frac{2}{2}\begin{bmatrix} 2 & 4 \\ 4 & 10 \end{bmatrix} = \mathbf{\begin{bmatrix} 2 & 4 \\ 4 & 10 \end{bmatrix}}$$
  
  * **(b) Eigenvalues of $H$:**
  Evaluate the characteristic polynomial $\det(H - \lambda I) = 0$:
  $$\det\begin{bmatrix} 2 - \lambda & 4 \\ 4 & 10 - \lambda \end{bmatrix} = (2 - \lambda)(10 - \lambda) - 16 = 0$$
  $$\lambda^2 - 12\lambda + 20 - 16 = \lambda^2 - 12\lambda + 4 = 0$$
  Using the quadratic formula:
  $$\lambda = \frac{12 \pm \sqrt{(-12)^2 - 4(1)(4)}}{2} = \frac{12 \pm \sqrt{144 - 16}}{2} = \frac{12 \pm \sqrt{128}}{2} = 6 \pm 4\sqrt{2}$$
  Since $4\sqrt{2} \approx 5.657$:
  $$\lambda_{\max} = 6 + 5.657 = \mathbf{11.657}$$
  $$\lambda_{\min} = 6 - 5.657 = \mathbf{0.343}$$
  
  * **(c) Maximum Stable Learning Rate:**
  $$\eta_{\max} = \frac{2}{\lambda_{\max}} = \frac{2}{11.657} \approx \mathbf{0.172}$$
  *(Any learning rate $\eta \ge 0.172$ will cause Batch Gradient Descent to diverge).*
  
  * **(d) Theoretically Optimal Learning Rate:**
  $$\eta^* = \frac{2}{\lambda_{\max} + \lambda_{\min}} = \frac{2}{11.657 + 0.343} = \frac{2}{12.000} = \frac{1}{6} \approx \mathbf{0.167}$$
  
  ---