### 1. Context: When the Characteristic Roots are Complex
  
  For the second-order homogeneous linear equation with constant real coefficients:
  $$ay'' + by' + cy = 0 \quad (a \neq 0)$$
  
  Assuming solutions of the form $y = e^{rt}$ yields the **characteristic equation**:
  $$ar^2 + br + c = 0$$
  
  Using the quadratic formula:
  $$r = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$
  
  When the discriminant is strictly negative:
  $$b^2 - 4ac < 0$$
  the roots are a pair of **complex conjugates**:
  $$r_{1, 2} = \alpha \pm i\beta \quad \text{(or } \lambda \pm i\mu\text{)}$$
  where:
  $$\alpha = -\frac{b}{2a} \quad (\text{Real Part}), \qquad \beta = \frac{\sqrt{4ac - b^2}}{2a} > 0 \quad (\text{Imaginary Part})$$
  
  ---
### 2. The Theoretical Derivation: Why Sines and Cosines Appear
  
  *(Filling in all intermediate steps from Page 2 of your handwritten notes)*
  
  The characteristic equation formally produces two complex-valued solutions:
  $$z_1(t) = e^{(\alpha + i\beta)t} = e^{\alpha t} e^{i\beta t}, \qquad z_2(t) = e^{(\alpha - i\beta)t} = e^{\alpha t} e^{-i\beta t}$$
  
  However, in physical applications (circuits, springs, structures), observed quantities are **real-valued**. We need real-valued fundamental solutions.
  
  ---
#### **Step A: Proving Euler’s Formula via Taylor Series**
  Recall the Maclaurin series for $e^x$, $\cos(\theta)$, and $\sin(\theta)$:
  $$e^x = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \frac{x^4}{4!} + \frac{x^5}{5!} + \dots$$
  
  Let $x = i\theta$, noting the powers of $i$:
  $$i^1 = i, \quad i^2 = -1, \quad i^3 = -i, \quad i^4 = 1, \quad i^5 = i, \dots$$
  
  Substitute into the exponential series:
  $$e^{i\theta} = 1 + i\theta + \frac{(i\theta)^2}{2!} + \frac{(i\theta)^3}{3!} + \frac{(i\theta)^4}{4!} + \frac{(i\theta)^5}{5!} + \dots$$
  $$e^{i\theta} = 1 + i\theta - \frac{\theta^2}{2!} - i\frac{\theta^3}{3!} + \frac{\theta^4}{4!} + i\frac{\theta^5}{5!} - \dots$$
  
  Group real and imaginary terms separately:
  $$e^{i\theta} = \underbrace{\left(1 - \frac{\theta^2}{2!} + \frac{\theta^4}{4!} - \dots\right)}_{\cos(\theta)} + i \underbrace{\left(\theta - \frac{\theta^3}{3!} + \frac{\theta^5}{5!} - \dots\right)}_{\sin(\theta)}$$
  
  $$\mathbf{e^{i\theta} = \cos(\theta) + i\sin(\theta)} \quad \text{\textbf{(Euler's Formula)}}$$
  
  ---
#### **Step B: Decomposing the Complex Solution**
  Substitute Euler’s Formula into $z_1(t)$ with $\theta = \beta t$:
  $$z_1(t) = e^{\alpha t}[\cos(\beta t) + i\sin(\beta t)] = \underbrace{e^{\alpha t}\cos(\beta t)}_{y_1(t)} + i \underbrace{e^{\alpha t}\sin(\beta t)}_{y_2(t)}$$
  
  ---
#### **Step C: Proof that the Real and Imaginary Parts are Independently Solutions**
  Why are $y_1(t)$ and $y_2(t)$ solutions on their own?
  
  Let $L[y] = ay'' + by' + cy$. Since $z_1 = y_1 + i y_2$ solves $L[z_1] = 0$:
  $$L[y_1 + i y_2] = a(y_1 + i y_2)'' + b(y_1 + i y_2)' + c(y_1 + i y_2) = 0$$
  $$(ay_1'' + by_1' + cy_1) + i(ay_2'' + by_2' + cy_2) = 0 + 0i$$
  
  A complex number equals zero **if and only if both its real and imaginary parts are independently zero**:
  $$ay_1'' + by_1' + cy_1 = 0 \implies L[y_1] = 0 \quad \checkmark$$
  $$ay_2'' + by_2' + cy_2 = 0 \implies L[y_2] = 0 \quad \checkmark$$
  
  Alternatively, by superposition:
  $$y_1(t) = \frac{z_1(t) + z_2(t)}{2} = e^{\alpha t}\cos(\beta t), \qquad y_2(t) = \frac{z_1(t) - z_2(t)}{2i} = e^{\alpha t}\sin(\beta t)$$
  
  ---
#### **Step D: Proving Linear Independence via the Wronskian**
  Let us verify that $W(y_1, y_2)(t) \neq 0$:
  $$y_1 = e^{\alpha t}\cos(\beta t) \implies y_1' = \alpha e^{\alpha t}\cos(\beta t) - \beta e^{\alpha t}\sin(\beta t)$$
  $$y_2 = e^{\alpha t}\sin(\beta t) \implies y_2' = \alpha e^{\alpha t}\sin(\beta t) + \beta e^{\alpha t}\cos(\beta t)$$
  
  $$W(y_1, y_2) = y_1 y_2' - y_1' y_2$$
  $$= e^{\alpha t}\cos(\beta t)\left[\alpha e^{\alpha t}\sin(\beta t) + \beta e^{\alpha t}\cos(\beta t)\right] - \left[\alpha e^{\alpha t}\cos(\beta t) - \beta e^{\alpha t}\sin(\beta t)\right]e^{\alpha t}\sin(\beta t)$$
  $$= e^{2\alpha t}\left[\alpha\cos(\beta t)\sin(\beta t) + \beta\cos^2(\beta t) - \alpha\cos(\beta t)\sin(\beta t) + \beta\sin^2(\beta t)\right]$$
  $$= e^{2\alpha t} \beta \underbrace{\left(\cos^2(\beta t) + \sin^2(\beta t)\right)}_{= 1} = \beta e^{2\alpha t}$$
  
  Since $\beta > 0$ and $e^{2\alpha t} > 0$ for all real $t$:
  $$W(y_1, y_2)(t) = \beta e^{2\alpha t} \neq 0$$
  Therefore, $y_1(t)$ and $y_2(t)$ are **linearly independent** and form a **fundamental set of solutions**.
  
  ---
### 3. The General Solution & Geometric Behavior
  
  $$\mathbf{y(t) = c_1 e^{\alpha t}\cos(\beta t) + c_2 e^{\alpha t}\sin(\beta t) = e^{\alpha t}\left(c_1\cos(\beta t) + c_2\sin(\beta t)\right)}$$
#### **What the Parameters Represent Physically:**
  * **$\beta$ (Frequency):** Dictates how rapidly the solution oscillates. The quasi-period of oscillation is $T = \frac{2\pi}{\beta}$.
  * **$\alpha$ (Damping / Envelope Growth):**
  * **$\alpha < 0$ (Damped Oscillations):** Amplitude decays exponentially ($e^{\alpha t} \to 0$ as $t \to \infty$). Solutions spiral into equilibrium.
  * **$\alpha = 0$ (Undamped Sustained Oscillations):** Pure sinusoidal motion $y(t) = c_1\cos(\beta t) + c_2\sin(\beta t)$ with constant amplitude.
  * **$\alpha > 0$ (Unstable Oscillations):** Oscillations blow up exponentially in amplitude as $t \to \infty$.
  
  ```
  y ^            Exponential Envelope: +C*e^(alpha*t)
   |    /\          /\
   |   /  \        /  \
   |--/----\------/----\------------------------> t
   | /      \    /      \
   |/        \  /        \
   |          \/          \/
                Exponential Envelope: -C*e^(alpha*t)
  ```
  
  ---
### 4. Fully Worked Case Studies & Exam Problems
  
  ---
#### **Example 1: Handwritten Notes Walkthrough (Page 1)**
  Find the general solution of:
  $$y'' + y' + y = 0$$
  
  * **Step 1: Characteristic Equation:**
  $$r^2 + r + 1 = 0$$
  * **Step 2: Solve for $r$:**
  $$r = \frac{-1 \pm \sqrt{1^2 - 4(1)(1)}}{2(1)} = \frac{-1 \pm \sqrt{-3}}{2} = -\frac{1}{2} \pm \frac{\sqrt{3}}{2}i$$
  * **Step 3: Identify $\alpha$ and $\beta$:**
  $$\alpha = -\frac{1}{2}, \qquad \beta = \frac{\sqrt{3}}{2}$$
  * **Step 4: General Solution:**
  $$y(t) = c_1 e^{-\frac{1}{2}t}\cos\left(\frac{\sqrt{3}}{2}t\right) + c_2 e^{-\frac{1}{2}t}\sin\left(\frac{\sqrt{3}}{2}t\right)$$
  
  ---
#### **Example 2: The Large-Coefficient IVP (Slides 128–143, Example 5)**
  Solve the initial value problem:
  $$16y'' - 8y' + 145y = 0, \quad y(0) = -2, \quad y'(0) = 1$$
  
  * **Step 1: Characteristic equation:**
  $$16r^2 - 8r + 145 = 0$$
  * **Step 2: Solve using quadratic formula with smart factoring:**
  $$r = \frac{-(-8) \pm \sqrt{(-8)^2 - 4(16)(145)}}{2(16)} = \frac{8 \pm \sqrt{64 - 64(145)}}{32}$$
  Factor out $64$:
  $$r = \frac{8 \pm \sqrt{64(1 - 145)}}{32} = \frac{8 \pm \sqrt{64(-144)}}{32} = \frac{8 \pm 8(12)i}{32} = \frac{8 \pm 96i}{32}$$
  $$r = \frac{1}{4} \pm 3i \implies \alpha = \frac{1}{4}, \quad \beta = 3$$
  
  * **Step 3: General solution:**
  $$y(t) = c_1 e^{\frac{1}{4}t}\cos(3t) + c_2 e^{\frac{1}{4}t}\sin(3t)$$
  
  * **Step 4: Compute $y'(t)$ using Product Rule:**
  $$y'(t) = \left[\frac{1}{4}c_1 e^{\frac{1}{4}t}\cos(3t) - 3c_1 e^{\frac{1}{4}t}\sin(3t)\right] + \left[\frac{1}{4}c_2 e^{\frac{1}{4}t}\sin(3t) + 3c_2 e^{\frac{1}{4}t}\cos(3t)\right]$$
  Group by $\cos(3t)$ and $\sin(3t)$:
  $$y'(t) = e^{\frac{1}{4}t}\left[\left(\frac{1}{4}c_1 + 3c_2\right)\cos(3t) + \left(-3c_1 + \frac{1}{4}c_2\right)\sin(3t)\right]$$
  
  * **Step 5: Apply Initial Conditions at $t = 0$:**
  * Recall $\cos(0) = 1$, $\sin(0) = 0$, $e^0 = 1$:
    $$y(0) = c_1 = -2$$
  * Use $y'(0) = 1$:
    $$y'(0) = \frac{1}{4}c_1 + 3c_2 = 1$$
    $$\frac{1}{4}(-2) + 3c_2 = 1 \implies -\frac{1}{2} + 3c_2 = 1 \implies 3c_2 = \frac{3}{2} \implies c_2 = \frac{1}{2}$$
  
  * **Step 6: Final Particular Solution:**
  $$y(t) = -2e^{\frac{1}{4}t}\cos(3t) + \frac{1}{2}e^{\frac{1}{4}t}\sin(3t)$$
  
  ---
#### **Example 3: Standard Exam Problem (Slides 144–152, Example 6)**
  Find the general solution of:
  $$y'' + 4y' + 5y = 0$$
  
  1. Characteristic equation: $r^2 + 4r + 5 = 0$
  2. Quadratic formula:
   $$r = \frac{-4 \pm \sqrt{16 - 20}}{2} = \frac{-4 \pm \sqrt{-4}}{2} = \frac{-4 \pm 2i}{2} = -2 \pm i$$
  3. $\alpha = -2$, $\beta = 1$.
  4. **General Solution:**
   $$y(t) = c_1 e^{-2t}\cos(t) + c_2 e^{-2t}\sin(t)$$
  
  ---
#### **Example 4: Spring 2012 Exam 1 #6(b) (Slide 168)**
  Find the general solution of:
  $$y'' - 6y' + 25y = 0$$
  
  1. Characteristic equation: $r^2 - 6r + 25 = 0$
  2. Quadratic formula:
   $$r = \frac{6 \pm \sqrt{36 - 100}}{2} = \frac{6 \pm \sqrt{-64}}{2} = \frac{6 \pm 8i}{2} = 3 \pm 4i$$
  3. $\alpha = 3$, $\beta = 4$.
  4. **General Solution:**
   $$y(t) = c_1 e^{3t}\cos(4t) + c_2 e^{3t}\sin(4t)$$
  
  ---
#### **Example 5: Fall 2011 Exam 1 #4(a) (Slide 168)**
  Find the general solution of:
  $$2y'' + 6y' + 5y = 0$$
  
  1. Characteristic equation: $2r^2 + 6r + 5 = 0$
  2. Quadratic formula:
   $$r = \frac{-6 \pm \sqrt{36 - 4(2)(5)}}{2(2)} = \frac{-6 \pm \sqrt{36 - 40}}{4} = \frac{-6 \pm \sqrt{-4}}{4} = \frac{-6 \pm 2i}{4} = -\frac{3}{2} \pm \frac{1}{2}i$$
  3. $\alpha = -\frac{3}{2}$, $\beta = \frac{1}{2}$.
  4. **General Solution:**
   $$y(t) = c_1 e^{-\frac{3}{2}t}\cos\left(\frac{1}{2}t\right) + c_2 e^{-\frac{3}{2}t}\sin\left(\frac{1}{2}t\right)$$
  
  ---
### Comprehensive Summary of All Three Roots Cases for $ay'' + by' + cy = 0$
  
  | Case | Discriminant | Roots of $ar^2 + br + c = 0$ | Fundamental Set $\{y_1, y_2\}$ | General Solution $y(t)$ |
  |:---:|:---:|:---:|:---:|:---:|
  | **Case 1** | $b^2 - 4ac > 0$ | Two real distinct: $r_1 \neq r_2$ | $\{e^{r_1 t}, e^{r_2 t}\}$ | $c_1 e^{r_1 t} + c_2 e^{r_2 t}$ |
  | **Case 2** | $b^2 - 4ac = 0$ | One real repeated: $r = -\frac{b}{2a}$ | $\{e^{rt}, t e^{rt}\}$ | $c_1 e^{rt} + c_2 t e^{rt}$ |
  | **Case 3** | $b^2 - 4ac < 0$ | Complex conjugate: $\alpha \pm i\beta$ | $\{e^{\alpha t}\cos(\beta t), e^{\alpha t}\sin(\beta t)\}$ | $e^{\alpha t}(c_1\cos(\beta t) + c_2\sin(\beta t))$ |
  
  ---
### Section 4.3 Pro-Tips & Exam Pitfalls
  * [x] **Never include $i$ in the general solution:** The constants $c_1$ and $c_2$ are real numbers multiplying real basis functions $\cos(\beta t)$ and $\sin(\beta t)$. Writing $i\sin(\beta t)$ in your general solution is an immediate point deduction.
  * [x] **Always choose $\beta > 0$:** Because $\cos(-\beta t) = \cos(\beta t)$ and $\sin(-\beta t) = -\sin(\beta t)$, any negative sign is absorbed into the arbitrary constants. Always take the positive square root for $\beta$.
  * [x] **Differentiating for IVPs:** At $t = 0$:
  $$y(0) = c_1$$
  $$y'(0) = \alpha c_1 + \beta c_2$$
  *(Memorizing this shortcut saves massive time and prevents algebraic errors when applying initial conditions!)*
  
  ---