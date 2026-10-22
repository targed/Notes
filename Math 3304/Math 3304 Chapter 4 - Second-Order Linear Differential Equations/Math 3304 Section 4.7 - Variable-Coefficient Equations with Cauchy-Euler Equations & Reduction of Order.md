### 1. The Big Picture: Beyond Constant Coefficients
  
  Up to this point, you have solved equations where the coefficients $a, b, c$ were constants ($a y'' + b y' + c y = g(t)$). 
  
  In Section 4.7, we tackle equations with **variable coefficients**:
  $$a_2(x)y'' + a_1(x)y' + a_0(x)y = g(x)$$
  
  This section covers two key tools:
  1. **Cauchy-Euler Equations:** A special, highly symmetrical class of variable-coefficient equations that can be solved analytically with powers $y = x^m$.
  2. **Reduction of Order:** A general technique that takes **any** linear second-order ODE with **one known solution $y_1(x)$** and constructs the second linearly independent solution $y_2(x)$.
  
  ---
### 2. Theoretical Review: Existence, Uniqueness & Interval of Validity
  
  Before solving, remember the general existence and uniqueness theorem for standard-form second-order IVPs:
  $$y'' + p(t)y' + q(t)y = g(t), \quad y(t_0) = y_0, \; y'(t_0) = y_1$$
  > **Theorem:** If $p(t)$, $q(t)$, and $g(t)$ are continuous on an open interval $I$ containing the initial point $t_0$, then a unique solution exists on the **entire interval $I$**.
#### **Worked Example from Notes (Page 1):**
  Find the largest interval where the IVP is guaranteed to have a unique solution:
  $$y'' + 2y' + \frac{1}{t^2 - 4}y = 0, \quad y(0) = 5, \; y'(0) = 6$$
  
  1. Identify the coefficient functions:
   * $p(t) = 2$ (continuous everywhere)
   * $q(t) = \frac{1}{t^2 - 4} = \frac{1}{(t - 2)(t + 2)}$ (undefined/discontinuous at $t = 2$ and $t = -2$)
   * $g(t) = 0$ (continuous everywhere)
  2. The singularities at $t = \pm 2$ divide the real number line into three intervals:
   $$(-\infty, -2), \quad (-2, 2), \quad (2, \infty)$$
  3. The initial condition is given at $t_0 = 0$.
  4. Since $0 \in (-2, 2)$, the theorem guarantees a unique solution on:
   $$\mathbf{I = (-2, 2)}$$
  
  ---
# PART I: Cauchy-Euler Equations (Equidimensional Equations)
### 1. Form and Properties
  
  A **Cauchy-Euler equation** is an ODE where the power of the independent variable $x$ matches the order of the derivative in every term:
  
  $$\mathbf{a x^2 y'' + b x y' + c y = 0} \quad (a \neq 0)$$
  
  * Notice the pattern:
  * $x^2$ multiplies $y''$ (2nd derivative)
  * $x^1$ multiplies $y'$ (1st derivative)
  * $x^0 = 1$ multiplies $y$ (0th derivative)
  * **Domain Restrictions:**
  In standard form, $y'' + \frac{b}{ax}y' + \frac{c}{ax^2}y = 0$. The coefficients blow up at $x = 0$. 
  Therefore, solutions must be defined on either $(0, \infty)$ or $(-\infty, 0)$. Unless otherwise stated, we assume **$x > 0$**.
  
  ---
### 2. The Characteristic Equation Derivation
  
  For constant-coefficient equations, the trial solution was an exponential: $y = e^{rt}$. 
  
  For Cauchy-Euler equations, notice that taking a derivative reduces the power of $x$ by 1 ($y' \sim x^{m-1}$), but multiplying by $x$ immediately restores the power ($x y' \sim x^m$). 
  
  Therefore, we assume a **power function**:
  $$\mathbf{y = x^m} \quad (x > 0, \; m = \text{constant})$$
#### **Compute Derivatives and Substitute:**
  $$y = x^m \implies y' = m x^{m-1} \implies y'' = m(m - 1)x^{m-2}$$
  
  Substitute into $a x^2 y'' + b x y' + c y = 0$:
  $$a x^2 [m(m - 1)x^{m-2}] + b x [m x^{m-1}] + c [x^m] = 0$$
  $$a m(m - 1)x^m + b m x^m + c x^m = 0$$
  $$x^m \left[ a m(m - 1) + b m + c \right] = 0$$
  
  Since $x > 0 \implies x^m \neq 0$, the bracketed expression must equal zero:
  $$a(m^2 - m) + bm + c = 0$$
  
  $$\mathbf{a m^2 + (b - a)m + c = 0} \quad \text{\textbf{(Cauchy-Euler Characteristic Equation)}}$$
  
  > ⚠️ **THE #1 STUDENT ERROR IN SECTION 4.7:**
  > The middle coefficient is **$(b - a)$**, **NOT** just $b$! 
  > If you forget to subtract $a$, your roots and entire solution will be wrong.
  
  ---
### 3. The Three Root Cases for Cauchy-Euler Equations
  
  Solve $a m^2 + (b - a)m + c = 0$ for $m$ using the quadratic formula:
  
  ---
#### **Case 1: Two Real Distinct Roots ($m_1 \neq m_2$)**
  * Two independent solutions: $y_1(x) = x^{m_1}$ and $y_2(x) = x^{m_2}$.
  * **General Solution:**
  $$\mathbf{y(x) = c_1 x^{m_1} + c_2 x^{m_2}}$$
  
  ---
#### **Case 2: One Real Repeated Root ($m_1 = m_2 = m$)**
  * Just as constant-coefficient equations gain a factor of $t$ for repeated roots ($e^{rt} \to t e^{rt}$), Cauchy-Euler equations gain a factor of **$\ln(x)$**:
  $$y_1(x) = x^m, \qquad y_2(x) = x^m \ln(x)$$
  * **General Solution:**
  $$\mathbf{y(x) = c_1 x^m + c_2 x^m \ln(x)}$$
  *(Derived rigorously in Part II via Reduction of Order).*
  
  ---
#### **Case 3: Complex Conjugate Roots ($m = \alpha \pm i\beta$)**
  * The formal complex solution is $z_1(x) = x^{\alpha + i\beta} = x^\alpha x^{i\beta}$.
  * **Euler’s Trick for Powers:** Rewrite $x$ as $e^{\ln(x)}$:
  $$x^{i\beta} = \left(e^{\ln(x)}\right)^{i\beta} = e^{i(\beta \ln(x))}$$
  * Apply Euler’s Formula:
  $$e^{i(\beta \ln(x))} = \cos(\beta \ln(x)) + i\sin(\beta \ln(x))$$
  * Taking real and imaginary parts gives:
  $$\mathbf{y(x) = c_1 x^\alpha \cos(\beta \ln(x)) + c_2 x^\alpha \sin(\beta \ln(x)) = x^\alpha \left[ c_1 \cos(\beta \ln(x)) + c_2 \sin(\beta \ln(x)) \right]}$$
  
  > *Note for $x < 0$:* If working on $(-\infty, 0)$, simply replace $x$ with $|x|$ everywhere (e.g., $|x|^m$, $\ln|x|$).
  
  ---
### 4. Worked Cauchy-Euler Examples from the Notes
  
  ---
#### **Example 1: Distinct Real Roots (Page 3)**
  Find the general solution of:
  $$x^2 y'' - 2x y' - 4y = 0, \quad x > 0$$
  
  1. Identify coefficients: $a = 1, \; b = -2, \; c = -4$.
  2. Form characteristic equation:
   $$a m^2 + (b - a)m + c = 0$$
   $$1 m^2 + (-2 - 1)m + (-4) = 0 \implies m^2 - 3m - 4 = 0$$
  3. Factor:
   $$(m - 4)(m + 1) = 0 \implies m_1 = 4, \; m_2 = -1$$
  4. **General Solution:**
   $$\mathbf{y(x) = c_1 x^4 + c_2 x^{-1} = c_1 x^4 + \frac{c_2}{x}}$$
  
  ---
#### **Example 2: Repeated Real Root (Page 3)**
  Find the general solution of:
  $$4x^2 y'' + 8x y' + y = 0, \quad x > 0$$
  
  1. Identify coefficients: $a = 4, \; b = 8, \; c = 1$.
  2. Form characteristic equation:
   $$4m^2 + (8 - 4)m + 1 = 0 \implies 4m^2 + 4m + 1 = 0$$
  3. Factor:
   $$(2m + 1)^2 = 0 \implies m = -\frac{1}{2} \text{ (repeated root)}$$
  4. **General Solution:**
   $$\mathbf{y(x) = c_1 x^{-1/2} + c_2 x^{-1/2}\ln(x)}$$
  
  ---
#### **Example 3: Complex Roots (Page 4)**
  Find the general solution of:
  $$4x^2 y'' + 17y = 0, \quad x > 0$$
  
  1. Identify coefficients: $a = 4, \; b = 0, \; c = 17$.
  2. Form characteristic equation:
   $$4m^2 + (0 - 4)m + 17 = 0 \implies 4m^2 - 4m + 17 = 0$$
  3. Quadratic formula:
   $$m = \frac{-(-4) \pm \sqrt{(-4)^2 - 4(4)(17)}}{2(4)} = \frac{4 \pm \sqrt{16 - 272}}{8} = \frac{4 \pm \sqrt{-256}}{8} = \frac{4 \pm 16i}{8} = \frac{1}{2} \pm 2i$$
  4. Identify $\alpha$ and $\beta$:
   $$\alpha = \frac{1}{2}, \qquad \beta = 2$$
  5. **General Solution:**
   $$\mathbf{y(x) = c_1 x^{1/2}\cos(2\ln(x)) + c_2 x^{1/2}\sin(2\ln(x))}$$
  
  ---
# PART II: Reduction of Order
### 1. What Is Reduction of Order and Why Do We Need It?
  
  Suppose you have a general, non-constant coefficient linear equation:
  $$y'' + p(x)y' + q(x)y = 0$$
  and you are **given one nonzero solution $y_1(x)$**. How do you find the second independent solution $y_2(x)$?
  
  We assume the second solution has the form:
  $$\mathbf{y_2(x) = u(x) y_1(x)}$$
  where $u(x)$ is a function that is **not constant** (if $u(x)$ were constant, $y_2$ would be linearly dependent on $y_1$).
#### **Proof that $W(y_1, y_2) \neq 0$ (Page 5):**
  $$W(y_1, u y_1) = \begin{vmatrix} y_1 & u y_1 \\ y_1' & u' y_1 + u y_1' \end{vmatrix} = y_1(u' y_1 + u y_1') - y_1'(u y_1) = u' [y_1]^2$$
  Since $y_1(x) \neq 0$ and $u(x)$ is non-constant ($u'(x) \neq 0$):
  $$W(y_1, y_2)(x) = u'(x)[y_1(x)]^2 \neq 0$$
  This guarantees that $y_1$ and $y_2$ will always be **linearly independent**.
  
  ---
### 2. General Derivation: The Vanishing of $u(x)$
  
  *(Filling in all details from Pages 7 & 8 of your handwritten notes)*
  
  1. Let $y_2 = u(x) y_1(x)$. Compute derivatives using Product Rule:
   $$y_2' = u' y_1 + u y_1'$$
   $$y_2'' = u'' y_1 + 2u' y_1' + u y_1''$$
  2. Substitute $y_2, y_2', y_2''$ into $y'' + p(x)y' + q(x)y = 0$:
   $$(u'' y_1 + 2u' y_1' + u y_1'') + p(x)(u' y_1 + u y_1') + q(x)(u y_1) = 0$$
  3. Regroup terms by the derivatives of $u(x)$:
   $$u'' [y_1] + u' [2y_1' + p(x)y_1] + u \underbrace{[y_1'' + p(x)y_1' + q(x)y_1]}_{= 0 \text{ (because } y_1 \text{ is a solution)}} = 0$$
  
  > ⚡ **THE GOLDEN SANITY CHECK:**
  > The coefficient of $u(x)$ **must always be zero**. 
  > If the term containing $u(x)$ does not completely cancel out when you substitute into the ODE, **you made an algebra error in your derivatives**. Stop and check your work immediately!
  
  4. Since the $u(x)$ term vanished, the equation only contains $u''$ and $u'$:
   $$y_1 u'' + [2y_1' + p(x)y_1]u' = 0$$
  5. **The "Reduction":** Let $\mathbf{v(x) = u'(x)} \implies \mathbf{v'(x) = u''(x)}$:
   $$\mathbf{y_1 v' + [2y_1' + p(x)y_1]v = 0}$$
   The second-order ODE has been **reduced to a first-order linear, separable ODE** for $v(x)$!
  
  ---
### 3. Step-by-Step Reduction of Order Recipe
  
  1. **Step 1:** Set $y_2(x) = u(x) y_1(x)$.
  2. **Step 2:** Calculate $y_2'$ and $y_2''$ and substitute them into the given ODE.
  3. **Step 3:** Collect terms: verify that the $u$ term vanishes, leaving an equation in $u''$ and $u'$.
  4. **Step 4:** Substitute $v = u'$ and $v' = u''$.
  5. **Step 5:** Solve the first-order ODE for $v(x)$ (by separating variables or integrating factor).
  6. **Step 6:** Integrate $v(x)$ to find $u(x)$: 
   $$u(x) = \int v(x)\,dx$$
   *(You can set the integration constants to $C = 1, D = 0$).*
  7. **Step 7:** Write $y_2(x) = u(x) y_1(x)$.
  
  ---
### 4. Fully Worked Reduction of Order Examples
  
  ---
#### **Example 4: Walkthrough from Notes (Pages 6 & 7)**
  *Given that $y_1(x) = x^{-2}$ is a solution of $x^2 y'' + 5x y' + 4y = 0$ for $x > 0$, find another linearly independent solution.*
  
  * **Step 1: Set $y_2 = u y_1 = u x^{-2}$ and differentiate:**
  $$y_2' = u' x^{-2} - 2u x^{-3}$$
  $$y_2'' = (u'' x^{-2} - 2u' x^{-3}) - 2(u' x^{-3} - 3u x^{-4}) = u'' x^{-2} - 4u' x^{-3} + 6u x^{-4}$$
  
  * **Step 2: Substitute into $x^2 y_2'' + 5x y_2' + 4y_2 = 0$:**
  $$x^2 \left[u'' x^{-2} - 4u' x^{-3} + 6u x^{-4}\right] + 5x \left[u' x^{-2} - 2u x^{-3}\right] + 4 \left[u x^{-2}\right] = 0$$
  $$u'' [x^2 x^{-2}] + u' [-4x^2 x^{-3} + 5x x^{-2}] + u [6x^2 x^{-4} - 10x x^{-3} + 4x^{-2}] = 0$$
  $$u'' [1] + u' [-4x^{-1} + 5x^{-1}] + u \underbrace{[6x^{-2} - 10x^{-2} + 4x^{-2}]}_{= 0 \quad \checkmark} = 0$$
  $$u'' + x^{-1}u' = 0$$
  
  * **Step 3: Substitute $v = u' \implies v' = u''$:**
  $$v' + \frac{1}{x}v = 0$$
  
  * **Step 4: Solve for $v(x)$ (Separation of Variables):**
  $$\frac{dv}{dx} = -\frac{1}{x}v \implies \frac{1}{v}\,dv = -\frac{1}{x}\,dx$$
  $$\ln|v| = -\ln(x) = \ln(x^{-1}) \implies v(x) = C x^{-1} = \frac{C}{x}$$
  
  * **Step 5: Integrate $v(x)$ to find $u(x)$:**
  $$u(x) = \int v(x)\,dx = \int \frac{C}{x}\,dx = C \ln(x) + D$$
  *We only need one valid function $u(x)$, so choose $C = 1$ and $D = 0$:*
  $$u(x) = \ln(x)$$
  
  * **Step 6: Form $y_2(x)$ and General Solution:**
  $$\mathbf{y_2(x) = u(x) y_1(x) = x^{-2}\ln(x)}$$
  $$\mathbf{y(x) = c_1 x^{-2} + c_2 x^{-2}\ln(x)}$$
  
  ---
#### **Example 5: Proving the Repeated Root Rule for Constant Coefficients (Page 8)**
  *Use Reduction of Order to prove that if $a y'' + b y' + c y = 0$ has a repeated root $r = -\frac{b}{2a}$, then $y_2(t) = t e^{rt}$.*
  
  1. Known solution: $y_1(t) = e^{rt}$ where $r = -\frac{b}{2a} \implies 2ar + b = 0$.
  2. Standard form: $y'' + \frac{b}{a}y' + \frac{c}{a}y = 0 \implies p = \frac{b}{a}$.
  3. Substitute into the reduced equation for $v = u'$:
   $$y_1 v' + (2y_1' + p y_1)v = 0$$
   Notice the coefficient of $v$:
   $$2y_1' + p y_1 = 2(r e^{rt}) + \frac{b}{a}e^{rt} = \left(2r + \frac{b}{a}\right)e^{rt} = \left(\frac{2ar + b}{a}\right)e^{rt} = 0$$
  4. The equation collapses simply to:
   $$e^{rt} v' = 0 \implies v'(t) = 0$$
  5. Integrate:
   $$v(t) = C_1 \implies u(t) = \int v(t)\,dt = C_1 t + C_2$$
  6. Setting $C_1 = 1, C_2 = 0 \implies u(t) = t$.
  7. Therefore:
   $$\mathbf{y_2(t) = u(t) y_1(t) = t e^{rt}} \quad \blacksquare$$
  
  ---
### Constant Coefficients vs. Cauchy-Euler: Parallel Comparison Table
  
  | Feature | Constant Coefficients: $a y'' + b y' + c y = 0$ | Cauchy-Euler: $a x^2 y'' + b x y' + c y = 0$ |
  |---|---|---|
  | **Trial Guess** | $y = e^{rt}$ | $y = x^m$ |
  | **Characteristic Equation** | $a r^2 + b r + c = 0$ | $a m^2 + \mathbf{(b - a)}m + c = 0$ |
  | **Case 1: Real Distinct** | $y = c_1 e^{r_1 t} + c_2 e^{r_2 t}$ | $y = c_1 x^{m_1} + c_2 x^{m_2}$ |
  | **Case 2: Real Repeated** | $y = c_1 e^{rt} + c_2 \mathbf{t} e^{rt}$ | $y = c_1 x^m + c_2 x^m \mathbf{\ln(x)}$ |
  | **Case 3: Complex $\alpha \pm i\beta$** | $y = e^{\alpha t}[c_1\cos(\beta t) + c_2\sin(\beta t)]$ | $y = x^\alpha [c_1\cos(\beta \mathbf{\ln x}) + c_2\sin(\beta \mathbf{\ln x})]$ |
  
  ---
### Section 4.7 Pro-Tips & Exam Traps Checklist
  * [x] **Cauchy-Euler $(b - a)$ Trap:** Did you subtract $a$ from the middle coefficient? $a m(m-1) + bm = am^2 + (b-a)m$.
  * [x] **Domain of Cauchy-Euler:** Always state $x > 0$ unless told otherwise.
  * [x] **Reduction of Order Sanity Check:** When substituting $y_2 = u y_1$, all terms containing bare $u(x)$ without derivatives **must sum to zero**. If they don't, check your product rule expansions.
  * [x] **Choice of Constants in Reduction of Order:** When integrating $u = \int v\,dx$, you only need *any* one independent solution, so pick $C = 1$ and $D = 0$.
  
  ---