# Part C8 — Logistic Regression by Hand Practice Problems
#### Problem 1 (Standard Negative Logit Mirror)
  A binary logistic regression classifier has parameters $w = [3.0, \; -2.0]^T$ and $b = -2.0$. An observation is evaluated at $x = [1.0, \; 1.0]^T$.
  * **(a)** Compute the linear score $z$ and predicted probability $p = \sigma(z)$.
  * **(b)** Predict the discrete class label at threshold $\tau = 0.50$. What is the predicted class if the threshold is lowered to $\tau = 0.20$?
  * **(c)** Write the explicit slope-intercept equation ($x_2 = m x_1 + c$) of the decision boundary at threshold $\tau = 0.50$.
  
  ---
#### Problem 2 (Boundary Intersection & Zero Distance)
  A classifier in $\mathbb{R}^2$ has weights $w = [4.0, \; -2.0]^T$ and bias $b = -6.0$. A query instance is located at $x = [2.0, \; 1.0]^T$.
  * **(a)** Compute $z$ and $p = \sigma(z)$.
  * **(b)** Classify this instance at threshold $\tau = 0.50$ (using the standard rule: predict Class 1 if $p \ge \tau$).
  * **(c)** Compute the Euclidean norm of the weight vector $\|w\|_2$, and calculate the perpendicular distance from $x$ to the decision boundary. Where does $x$ lie relative to the boundary?
  
  ---
#### Problem 3 (Threshold Moving & Boundary Translation)
  A medical diagnostic model separating malignant tumors ($Y = 1$) from benign tumors ($Y = 0$) has parameters $w_1 = 1.0, \; w_2 = 2.0,$ and $b = -4.0$.
  * **(a)** Write the equation of the decision boundary line at the standard threshold $\tau = 0.50$.
  * **(b)** To prioritize cancer recall, an oncologist shifts the decision threshold to $\tau = \frac{1}{1 + e^1} \approx 0.269$ ($z = -1.0$). Derive the new decision boundary line equation.
  * **(c)** Did the slope of the decision boundary change? How did the boundary shift physically in feature space?
  
  ---
#### Problem 4 (Binary Cross-Entropy Loss on Single Instances)
  A logistic regression model has weights $w = [2.0, \; -1.0]^T$ and $b = 0.0$. An observation arrives at $x = [1.0, \; 2.0]^T$.
  * **(a)** Compute $z$ and the predicted probability $\hat{p} = \sigma(z)$.
  * **(b)** If the true ground-truth label is $y = 1$, compute the Binary Cross-Entropy loss $\mathcal{L} = -\ln(\hat{p})$.
  * **(c)** If the true ground-truth label is $y = 0$, compute the Binary Cross-Entropy loss $\mathcal{L} = -\ln(1 - \hat{p})$.
  
  ---
#### Problem 5 (One Step of Logistic Gradient Descent by Hand)
  A training set consists of a single observation: $x = [1.0, \; 0.0]^T$ with true label $y = 1$.  
  The model is initialized with weights $w = [0.0, \; 0.0]^T$ and bias $b = 0.0$.  
  Let the learning rate be $\eta = 0.20$.
  * **(a)** Compute the initial logit $z$ and predicted probability $\hat{p}$.
  * **(b)** Compute the parameter gradients $\nabla_w \mathcal{L} = (\hat{p} - y)x$ and $\nabla_b \mathcal{L} = (\hat{p} - y)$.
  * **(c)** Perform **ONE parameter update step**, computing the new weights $w^{(1)}$ and bias $b^{(1)}$.
  
  ---
#### Problem 6 (Translating Between Odds, Probability, and Logit)
  An epidemiological model predicts the risk of cardiac disease. For a specific patient, the model outputs a predicted probability of $\hat{p} = 0.800$.
  * **(a)** Compute the patient's odds of cardiac disease ($\frac{p}{1 - p}$).
  * **(b)** Compute the patient's logit score $z = \ln(\text{Odds})$. (Note: $\ln(4.0) \approx 1.386$).
  * **(c)** If the underlying model equation is $z = 0.05 \cdot \text{age} + 0.02 \cdot \text{glucose} - 4.0$, and the patient's glucose level is $120$, what is the patient's age?
  
  ---
#### Problem 7 (Signed Distance to the Decision Surface)
  A linear classifier in $\mathbb{R}^2$ has parameters $w = [3.0, \; 4.0]^T$ and $b = -5.0$. A query instance is located at $x = [3.0, \; 4.0]^T$.
  * **(a)** Compute the Euclidean norm $\|w\|_2$.
  * **(b)** Compute the perpendicular Euclidean distance from $x$ to the decision boundary: $d_\perp = \frac{|w^T x + b|}{\|w\|_2}$.
  * **(c)** Does $x$ lie in the Class 1 or Class 0 half-space?
  
  ---
#### Problem 8 (3D Feature Space Decision Plane)
  A classifier operates on a 3-dimensional feature space ($x \in \mathbb{R}^3$) with parameters $w = [1.0, \; -2.0, \; 3.0]^T$ and $b = -6.0$.
  * **(a)** Compute the logit $z$ and probability $p$ for query point $x = [2.0, \; 1.0, \; 1.0]^T$. (Note: $e^3 \approx 20.086$).
  * **(b)** Write the algebraic equation of the 2D decision plane separating the classes at threshold $\tau = 0.50$.
  * **(c)** Test whether the point $(x_1 = 4.0, \; x_2 = 2.0, \; x_3 = 2.0)$ lies on the boundary, in Class 1, or in Class 0.
  
  ---
#### Problem 9 (Confident Error: BCE vs. MSE Gradient Comparison)
  Consider a positive training observation ($y = 1$).
  * In **Case 1**, the model is confident and correct: $z_1 = +3.0 \implies \hat{p}_1 = \sigma(3.0) \approx 0.953$.
  * In **Case 2**, the model is confident and wrong: $z_2 = -3.0 \implies \hat{p}_2 = \sigma(-3.0) \approx 0.047$.
  * **(a)** For both cases, compute the Binary Cross-Entropy gradient magnitude: $|\nabla_z \mathcal{L}_{\text{BCE}}| = |\hat{p} - y|$.
  * **(b)** For both cases, compute the Mean Squared Error gradient magnitude: $|\nabla_z \mathcal{L}_{\text{MSE}}| = |2(\hat{p} - y)\hat{p}(1 - \hat{p})|$.
  * **(c)** Compare the gradient magnitudes in Case 2. Why does gradient descent fail when optimizing MSE on saturated errors?
  
  ---
#### Problem 10 (Reverse-Engineering Parameters from Boundary Points)
  A linear decision boundary in $\mathbb{R}^2$ passes through the two intercepts $(x_1 = 0, \; x_2 = 2)$ and $(x_1 = 4, \; x_2 = 0)$. The positive class region ($Y = 1$) is defined by points lying above and to the right of the line.
  * **(a)** Write the standard line equation $w_1 x_1 + w_2 x_2 + b = 0$ assuming normalized integer weights ($w_1 = 1.0$).
  * **(b)** For a query point $q = [2.0, \; 2.0]^T$, compute the logit $z$ and probability $p = \sigma(z)$.
  * **(c)** If an engineer multiplies all parameters by a constant factor of $2$ ($w = [2.0, \; 4.0]^T, \; b = -8.0$), does the decision boundary change? What happens to the predicted probability $p(q)$?
  
  ---
  
  $$\mathbf{=== \text{ STOP! SOLUTIONS BELOW } ===}$$
  
  ---
# Solutions & Detailed Explanations for C8
### Problem 1 Solution
  * **(a) Linear Score and Probability:**
  $$z = w^T x + b = 3.0(1.0) + (-2.0)(1.0) - 2.0 = 3.0 - 2.0 - 2.0 = \mathbf{-1.000}$$
  $$p = \sigma(-1.0) = \frac{1}{1 + e^{-(-1.0)}} = \frac{1}{1 + e^1} \approx \frac{1}{1 + 2.718} = \frac{1}{3.718} \approx \mathbf{0.269 \quad (26.9\%)}$$
  * **(b) Threshold Classifications:**
  * At $\tau = 0.50$: $p = 0.269 < 0.50 \implies \mathbf{\hat{y} = 0 \quad (\text{Class 0})}$
  * At $\tau = 0.20$: $p = 0.269 \ge 0.20 \implies \mathbf{\hat{y} = 1 \quad (\text{Class 1})}$
  * **(c) Decision Boundary Equation at $\tau = 0.50$ ($z = 0$):**
  $$3x_1 - 2x_2 - 2 = 0 \implies 2x_2 = 3x_1 - 2 \implies \mathbf{x_2 = 1.500x_1 - 1.000}$$
  *(Slope $m = 1.500$, Intercept $c = -1.000$).* `[L13, Review C8]`
  
  ---
### Problem 2 Solution
  * **(a) Compute $z$ and $p$:**
  $$z = w^T x + b = 4.0(2.0) + (-2.0)(1.0) - 6.0 = 8.0 - 2.0 - 6.0 = \mathbf{0.000}$$
  $$p = \sigma(0.0) = \frac{1}{1 + e^0} = \frac{1}{1 + 1} = \mathbf{0.500 \quad (50.0\%)}$$
  * **(b) Classification at $\tau = 0.50$:**
  $$p = 0.500 \ge 0.50 \implies \mathbf{\hat{y} = 1 \quad (\text{Class 1})}$$
  * **(c) Weight Norm & Distance to Boundary:**
  $$\|w\|_2 = \sqrt{w_1^2 + w_2^2} = \sqrt{4.0^2 + (-2.0)^2} = \sqrt{16 + 4} = \sqrt{20} = 2\sqrt{5} \approx \mathbf{4.472}$$
  $$d_\perp = \frac{|z|}{\|w\|_2} = \frac{|0.000|}{4.472} = \mathbf{0.000}$$
  *Location:* Because $z = 0$ and distance is $0$, the observation lies **directly on the decision boundary hyperplane**. `[L13]`
  
  ---
### Problem 3 Solution
  * **(a) Decision Boundary at $\tau = 0.50$ ($z = 0$):**
  $$w_1 x_1 + w_2 x_2 + b = 0 \implies 1.0x_1 + 2.0x_2 - 4.0 = 0$$
  $$2.0x_2 = -x_1 + 4.0 \implies \mathbf{x_2 = -0.500x_1 + 2.000}$$
  * **(b) Shifted Boundary at $\tau \approx 0.269$ ($z = -1.0$):**
  $$z = -1.0 \implies 1.0x_1 + 2.0x_2 - 4.0 = -1.0$$
  $$2.0x_2 = -x_1 + 3.0 \implies \mathbf{x_2 = -0.500x_1 + 1.500}$$
  * **(c) Physical Geometric Shift:**
  * The slope remains identical ($m = -0.500$), meaning the boundary line remains **parallel** to the original.
  * The intercept drops from $2.000 \to 1.500$. The line translates downward into the negative territory, expanding the spatial volume assigned to the positive Class 1. This catches more positive instances at the cost of more false alarms. `[L13]`
  
  ---
### Problem 4 Solution
  * **(a) Compute $z$ and $\hat{p}$:**
  $$z = 2.0(1.0) + (-1.0)(2.0) + 0.0 = 2.0 - 2.0 = \mathbf{0.000}$$
  $$\hat{p} = \sigma(0.0) = \mathbf{0.500}$$
  * **(b) Loss when True Label $y = 1$:**
  $$\mathcal{L} = -\ln(\hat{p}) = -\ln(0.500) = \ln(2.0) \approx \mathbf{0.693}$$
  * **(c) Loss when True Label $y = 0$:**
  $$\mathcal{L} = -\ln(1 - \hat{p}) = -\ln(1 - 0.500) = -\ln(0.500) \approx \mathbf{0.693}$$
  *(When maximally uncertain at $\hat{p} = 0.50$, the model incurs a baseline penalty of $\ln(2) \approx 0.693$ regardless of class).* `[L13]`
  
  ---
### Problem 5 Solution
  * **(a) Initial Score and Probability:**
  $$z = w_1(1.0) + w_2(0.0) + b = 0(1) + 0(0) + 0 = \mathbf{0.000}$$
  $$\hat{p} = \sigma(0.0) = \mathbf{0.500}$$
  * **(b) Compute Gradients ($y = 1$):**
  The scalar residual error is:
  $$\hat{p} - y = 0.500 - 1.0 = \mathbf{-0.500}$$
  $$\nabla_w \mathcal{L} = (\hat{p} - y)x = (-0.500)\begin{bmatrix} 1.0 \\ 0.0 \end{bmatrix} = \mathbf{\begin{bmatrix} -0.500 \\ 0.000 \end{bmatrix}}$$
  $$\nabla_b \mathcal{L} = \hat{p} - y = \mathbf{-0.500}$$
  * **(c) Execute Parameter Update ($\eta = 0.20$):**
  $$w^{(1)} = w^{(0)} - \eta \nabla_w \mathcal{L} = \begin{bmatrix} 0.0 \\ 0.0 \end{bmatrix} - 0.20\begin{bmatrix} -0.500 \\ 0.000 \end{bmatrix} = \mathbf{\begin{bmatrix} +0.100 \\ 0.000 \end{bmatrix}}$$
  $$b^{(1)} = b^{(0)} - \eta \nabla_b \mathcal{L} = 0.0 - 0.20(-0.500) = \mathbf{+0.100}$$
  *Updated model: $\hat{p} = \sigma(0.1x_1 + 0.1)$.* `[L11, L13]`
  
  ---
### Problem 6 Solution
  * **(a) Odds Calculation:**
  $$\text{Odds} = \frac{p}{1 - p} = \frac{0.800}{1 - 0.800} = \frac{0.800}{0.200} = \mathbf{4.000}$$
  *(The patient is 4 times more likely to develop disease than not).*
  * **(b) Logit Score ($z$):**
  $$z = \ln(\text{Odds}) = \ln(4.000) \approx \mathbf{1.386}$$
  * **(c) Solve for Patient Age:**
  Substitute $z = 1.386$ and $\text{glucose} = 120$ into the model:
  $$1.386 = 0.05 \cdot \text{age} + 0.02(120) - 4.0$$
  $$1.386 = 0.05 \cdot \text{age} + 2.40 - 4.0$$
  $$1.386 = 0.05 \cdot \text{age} - 1.60$$
  $$0.05 \cdot \text{age} = 1.386 + 1.60 = 2.986$$
  $$\text{age} = \frac{2.986}{0.05} = \mathbf{59.720 \text{ years}}$$ `[L13]`
  
  ---
### Problem 7 Solution
  * **(a) Euclidean Norm of $w$:**
  $$\|w\|_2 = \sqrt{3.0^2 + 4.0^2} = \sqrt{9 + 16} = \sqrt{25} = \mathbf{5.000}$$
  * **(b) Perpendicular Distance to Boundary:**
  First evaluate logit $z$ at $x = [3.0, \; 4.0]^T$:
  $$z = w^T x + b = 3.0(3.0) + 4.0(4.0) - 5.0 = 9.0 + 16.0 - 5.0 = \mathbf{20.000}$$
  $$d_\perp = \frac{|z|}{\|w\|_2} = \frac{|20.000|}{5.000} = \mathbf{4.000 \text{ units}}$$
  * **(c) Class Region:**
  Because $z = +20.000 > 0$, the point lies deeply in the positive **Class 1 region** ($\hat{p} = \sigma(20) \approx 1.000$). `[L13]`
  
  ---
### Problem 8 Solution
  * **(a) Compute $z$ and $p$:**
  $$z = 1.0(2.0) + (-2.0)(1.0) + 3.0(1.0) - 6.0 = 2.0 - 2.0 + 3.0 - 6.0 = \mathbf{-3.000}$$
  $$p = \sigma(-3.0) = \frac{1}{1 + e^3} \approx \frac{1}{1 + 20.086} = \frac{1}{21.086} \approx \mathbf{0.047 \quad (4.7\%)}$$
  * **(b) Decision Plane Equation at $\tau = 0.50$ ($z = 0$):**
  $$\mathbf{x_1 - 2x_2 + 3x_3 - 6 = 0}$$
  * **(c) Location of Point $(4.0, 2.0, 2.0)$:**
  Evaluate $z$ at $(4, 2, 2)$:
  $$z = 1.0(4.0) - 2.0(2.0) + 3.0(2.0) - 6.0 = 4.0 - 4.0 + 6.0 - 6.0 = \mathbf{0.000}$$
  Because $z = 0.000$, the point lies **directly on the decision boundary plane** ($p = 0.500$). `[L13]`
  
  ---
### Problem 9 Solution
  * **(a) Binary Cross-Entropy Gradients ($|\hat{p} - y|$ with $y = 1$):**
  * Case 1 ($z = +3.0$):
    $$|\nabla_z \mathcal{L}_{\text{BCE}}| = |0.953 - 1| = |-0.047| = \mathbf{0.047}$$
  * Case 2 ($z = -3.0$):
    $$|\nabla_z \mathcal{L}_{\text{BCE}}| = |0.047 - 1| = |-0.953| = \mathbf{0.953}$$
  * **(b) Mean Squared Error Gradients ($|2(\hat{p} - y)\hat{p}(1 - \hat{p})|$ with $y = 1$):**
  * Case 1 ($z = +3.0$):
    $$|\nabla_z \mathcal{L}_{\text{MSE}}| = 2|0.953 - 1|(0.953)(1 - 0.953) = 2(0.047)(0.953)(0.047) \approx \mathbf{0.004}$$
  * Case 2 ($z = -3.0$):
    $$|\nabla_z \mathcal{L}_{\text{MSE}}| = 2|0.047 - 1|(0.047)(1 - 0.047) = 2(0.953)(0.047)(0.953) \approx \mathbf{0.085}$$
  * **(c) Why MSE Optimization Fails on Saturated Errors:**
  * In Case 2, the model is catastrophically wrong ($y = 1$, but $z = -3.0$).
  * Under BCE, the gradient magnitude is **$0.953$ (a strong, near-maximal corrective pull)**.
  * Under MSE, the gradient is **$0.085$ (over $11\times$ weaker)**.
  * As $z \to -\infty$, the MSE gradient vanishes entirely to $0$ because of the dampening factor $\hat{p}(1 - \hat{p})$. The worst mistakes receive the weakest gradient updates, stranding gradient descent on flat error plateaus. `[L13]`
  
  ---
### Problem 10 Solution
  * **(a) Line Equation from Intercepts:**
  The line passes through $(0, 2)$ and $(4, 0)$.
  $$\text{Slope } m = \frac{0 - 2}{4 - 0} = -\frac{2}{4} = -0.5$$
  Equation: $x_2 = -0.5x_1 + 2 \implies 0.5x_1 + x_2 - 2 = 0$.
  Normalizing so $w_1 = 1.0$ (multiply through by $2$):
  $$\mathbf{x_1 + 2x_2 - 4 = 0} \implies \mathbf{w_1 = 1.0, \; w_2 = 2.0, \; b = -4.0}$$
  * **(b) Evaluate Point $q = [2.0, \; 2.0]^T$:**
  $$z = 1.0(2.0) + 2.0(2.0) - 4.0 = 2.0 + 4.0 - 4.0 = \mathbf{2.000}$$
  $$p(q) = \sigma(2.0) = \frac{1}{1 + e^{-2}} \approx \frac{1}{1 + 0.135} = \frac{1}{1.135} \approx \mathbf{0.881 \quad (88.1\%)}$$
  * **(c) Scaling Parameters by $c = 2$:**
  * **Boundary Check:**
    $$2x_1 + 4x_2 - 8 = 0 \iff x_1 + 2x_2 - 4 = 0$$
    The decision boundary line is **completely unchanged**.
  * **Probability Shift:**
    $$z_{\text{new}}(q) = 2.0(2.0) + 4.0(2.0) - 8.0 = 4.0 + 8.0 - 8.0 = \mathbf{4.000}$$
    $$p_{\text{new}}(q) = \sigma(4.0) = \frac{1}{1 + e^{-4}} \approx \frac{1}{1 + 0.0183} \approx \mathbf{0.982 \quad (98.2\%)}$$
  * *Takeaway:* Scaling weights by a positive constant steepens the sigmoid transition, increasing the model's confidence without altering the physical boundary in feature space. `[L13]`
  
  ---