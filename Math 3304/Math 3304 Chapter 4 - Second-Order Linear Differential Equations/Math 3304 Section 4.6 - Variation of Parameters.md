### 1. Conceptual Motivation: Why Do We Need Variation of Parameters?
  
  In Sections 4.4 and 4.5, you learned the **Method of Undetermined Coefficients (MUC)**. MUC is fast, but it is severely limited:
  1. It **only** works when the differential equation has **constant coefficients** ($a, b, c \in \mathbb{R}$).
  2. It **only** works when the forcing term $g(t)$ is built from polynomials, exponentials, sines, and cosines.
  
  If the forcing term contains anything else—such as:
  $$g(t) = \tan(t), \quad \sec(t), \quad \frac{1}{t}, \quad \ln(t), \quad \frac{1}{1 + t^2}, \quad \frac{e^t}{t}$$
  or if the differential equation has **variable coefficients** (e.g., $t^2 y'' + t y' + y = g(t)$), **MUC completely fails**.
  
  ```
  +-----------------------------------------------------------------------------------------+
  |                                    DECISION MATRIX                                      |
  |                                                                                         |
  |  * Does g(t) have sin, cos, e^(at), polynomials, and constant coefficients?             |
  |      --> USE MUC (§4.4/§4.5)  (Much faster, purely algebraic)                           |
  |                                                                                         |
  |  * Does g(t) have fractions, tan, sec, ln, 1/t, or variable coefficients?               |
  |      --> USE VARIATION OF PARAMETERS (§4.6)  (Universal, works on everything)           |
  +-----------------------------------------------------------------------------------------+
  ```
  
  ---
  ### 2. The Theoretical Derivation: Why the Formulas Work
    
    *(Filling in the full derivation from Pages 4 & 5 of your handwritten notes)*
    
    Consider a general second-order linear differential equation already written in **standard form**:
    $$y'' + p(t)y' + q(t)y = g(t) \quad (\ast)$$
    
    Suppose we know the general solution to the associated homogeneous equation $y'' + p(t)y' + q(t)y = 0$:
    $$y_h(t) = c_1 y_1(t) + c_2 y_2(t)$$
    where $\{y_1, y_2\}$ is a fundamental set of solutions ($W(y_1, y_2) \neq 0$).
#### **The Core Idea (Lagrange's Method):**
  Instead of treating the parameters $c_1$ and $c_2$ as constants, let them **vary as functions of $t$**:
  $$y_p(t) = u_1(t) y_1(t) + u_2(t) y_2(t)$$
  We need to find two unknown functions: $u_1(t)$ and $u_2(t)$. 
  
  ---
#### **Step A: Compute the First Derivative $$y_p'$$ and Impose a Constraint**
  Using the Product Rule:
  $$y_p'(t) = \left[u_1'(t)y_1(t) + u_2'(t)y_2(t)\right] + \left[u_1(t)y_1'(t) + u_2(t)y_2'(t)\right]$$
  
  Since we introduced two unknown functions ($u_1, u_2$) but only have one equation to satisfy (equation $\ast$), we have the freedom to impose **one convenient constraint**. 
  To prevent second derivatives $u_1''$ and $u_2''$ from appearing in the next step, Lagrange set the first bracketed term to zero:
  
  $$\mathbf{u_1'(t)y_1(t) + u_2'(t)y_2(t) = 0} \quad \text{\textbf{(Constraint 1)}}$$
  
  With this constraint, the first derivative simplifies to:
  $$y_p'(t) = u_1(t)y_1'(t) + u_2(t)y_2'(t)$$
  
  ---
  #### **Step B: Compute the Second Derivative $y_p''$ and Substitute into the ODE**
    Differentiate $y_p'$:
    $$y_p''(t) = u_1'(t)y_1'(t) + u_1(t)y_1''(t) + u_2'(t)y_2'(t) + u_2(t)y_2''(t)$$
    
    Now substitute $y_p$, $y_p'$, and $y_p''$ into the standard ODE $y'' + p(t)y' + q(t)y = g(t)$:
    $$\begin{aligned}
    & \left[u_1' y_1' + u_1 y_1'' + u_2' y_2' + u_2 y_2''\right] \\
    + & p(t)\left[u_1 y_1' + u_2 y_2'\right] \\
    + & q(t)\left[u_1 y_1 + u_2 y_2\right] = g(t)
    \end{aligned}$$
    
    Regroup by $u_1(t)$ and $u_2(t)$:
    $$(u_1' y_1' + u_2' y_2') + u_1 \underbrace{\left[y_1'' + p(t)y_1' + q(t)y_1\right]}_{= 0 \text{ (since } y_1 \text{ solves homog.)}} + u_2 \underbrace{\left[y_2'' + p(t)y_2' + q(t)y_2\right]}_{= 0 \text{ (since } y_2 \text{ solves homog.)}} = g(t)$$
    
    Everything in the brackets vanishes, leaving our second condition:
    $$\mathbf{u_1'(t)y_1'(t) + u_2'(t)y_2'(t) = g(t)} \quad \text{\textbf{(Condition 2)}}$$
    
    ---
#### **Step C: Solve the $2 \times 2$ System via Cramer’s Rule**
  Combining Constraint 1 and Condition 2 gives a linear system for the unknown derivatives $u_1'$ and $u_2'$:
  $$\begin{cases} y_1 u_1' + y_2 u_2' = 0 \\ y_1' u_1' + y_2' u_2' = g(t) \end{cases} \iff \begin{pmatrix} y_1 & y_2 \\ y_1' & y_2' \end{pmatrix} \begin{pmatrix} u_1' \\ u_2' \end{pmatrix} = \begin{pmatrix} 0 \\ g(t) \end{pmatrix}$$
  
  The determinant of the coefficient matrix is the **Wronskian**:
  $$\det \begin{pmatrix} y_1 & y_2 \\ y_1' & y_2' \end{pmatrix} = y_1 y_2' - y_1' y_2 = W(y_1, y_2)(t)$$
  
  Applying Cramer’s Rule:
  $$u_1'(t) = \frac{\begin{vmatrix} 0 & y_2 \\ g(t) & y_2' \end{vmatrix}}{W(y_1, y_2)} = \frac{0 - y_2(t)g(t)}{W(y_1, y_2)} \implies \mathbf{u_1'(t) = -\frac{y_2(t)g(t)}{W(y_1, y_2)(t)}}$$
  
  $$u_2'(t) = \frac{\begin{vmatrix} y_1 & 0 \\ y_1' & g(t) \end{vmatrix}}{W(y_1, y_2)} = \frac{y_1(t)g(t) - 0}{W(y_1, y_2)} \implies \mathbf{u_2'(t) = \frac{y_1(t)g(t)}{W(y_1, y_2)(t)}}$$
  
  ---
### 3. The Master Formulas
  
  To find the particular solution $y_p(t) = u_1(t)y_1(t) + u_2(t)y_2(t)$, integrate $u_1'$ and $u_2'$:
  
  $$\mathbf{u_1(t) = -\int \frac{y_2(t) g(t)}{W(y_1, y_2)(t)}\,dt}, \qquad \mathbf{u_2(t) = \int \frac{y_1(t) g(t)}{W(y_1, y_2)(t)}\,dt}$$
  
  > ⚠️ **THE FATAL "STEP 0" TRAP:**
  > The equation **MUST be in standard form** ($1 \cdot y'' + p y' + q y = g(t)$) before you identify $g(t)$!
  > If you are given $a(t)y'' + b(t)y' + c(t)y = f(t)$, you **must divide by $a(t)$ first**:
  > $$g(t) = \frac{f(t)}{a(t)}$$
  > Using $f(t)$ instead of $g(t)$ in the integral formula will ruin the entire problem.
  
  ---
### 4. Why You Can Drop the $+C$ in $u_1$ and $u_2$
  
  *(Proof from Pages 1 & 2 of your handwritten notes)*
  
  If you included arbitrary constants $+c_1$ and $+c_2$ when integrating:
  $$\tilde{u}_1(t) = u_1(t) + c_1, \qquad \tilde{u}_2(t) = u_2(t) + c_2$$
  Then the particular solution would become:
  $$\begin{aligned}
  y_p(t) &= (u_1(t) + c_1)y_1(t) + (u_2(t) + c_2)y_2(t) \\
  &= \left[u_1(t)y_1(t) + u_2(t)y_2(t)\right] + \left[c_1 y_1(t) + c_2 y_2(t)\right]
  \end{aligned}$$
  The second piece $\left[c_1 y_1 + c_2 y_2\right]$ is simply the complementary solution $y_h(t)$, which is already added in the full general solution $y = y_h + y_p$. 
  
  Therefore, you can always choose the constants of integration to be zero: **$c_1 = 0, c_2 = 0$**.
  
  ---
### 5. Step-by-Step Problem-Solving Recipe
  
  1. **Step 0 (Standard Form):** Divide through by the leading coefficient so $y''$ has a coefficient of $1$. Read off $g(t)$.
  2. **Step 1 (Complementary Solution):** Find $y_h = c_1 y_1 + c_2 y_2$ by solving the homogeneous equation.
  3. **Step 2 (Compute Wronskian):** 
   $$W(y_1, y_2)(t) = y_1 y_2' - y_1' y_2$$
  4. **Step 3 (Integrate to find $u_1$ and $u_2$):**
   $$u_1(t) = -\int \frac{y_2 g}{W}\,dt, \qquad u_2(t) = \int \frac{y_1 g}{W}\,dt$$
  5. **Step 4 (Form $y_p$):** 
   $$y_p(t) = u_1(t) y_1(t) + u_2(t) y_2(t)$$
  6. **Step 5 (Write Full General Solution):**
   $$y(t) = y_h(t) + y_p(t)$$
   *(If initial conditions are given, solve for $c_1, c_2$ now using $y(t_0) = y_0$ and $y'(t_0) = y_1$).*
  
  ---
### 6. Fully Worked Master Examples
  
  ---
#### **Example 1: The Classic Non-MUC Fraction (Handwritten Notes, Pages 3–5)**
  Solve:
  $$4y'' - 4y' + y = \frac{8e^{t/2}}{t}, \quad t > 0$$
  
  * **Step 0: Put in Standard Form!**
  Divide everything by $4$:
  $$y'' - y' + \frac{1}{4}y = \frac{2e^{t/2}}{t} \implies \mathbf{g(t) = \frac{2e^{t/2}}{t}}$$
  
  * **Step 1: Solve Homogeneous Equation:**
  $$4r^2 - 4r + 1 = 0 \implies (2r - 1)^2 = 0 \implies r = \frac{1}{2} \text{ (repeated root)}$$
  $$y_1(t) = e^{t/2}, \qquad y_2(t) = t e^{t/2}$$
  $$y_h(t) = c_1 e^{t/2} + c_2 t e^{t/2}$$
  
  * **Step 2: Compute Wronskian:**
  $$y_1 = e^{t/2} \implies y_1' = \frac{1}{2}e^{t/2}$$
  $$y_2 = t e^{t/2} \implies y_2' = e^{t/2} + \frac{1}{2}t e^{t/2}$$
  $$W = y_1 y_2' - y_1' y_2 = e^{t/2}\left(e^{t/2} + \frac{1}{2}t e^{t/2}\right) - \left(\frac{1}{2}e^{t/2}\right)(t e^{t/2}) = e^t + \frac{1}{2}t e^t - \frac{1}{2}t e^t = \mathbf{e^t}$$
  
  * **Step 3: Integrate for $u_1$ and $u_2$:**
  * **For $u_1(t)$:**
    $$u_1(t) = -\int \frac{y_2(t) g(t)}{W}\,dt = -\int \frac{\left(t e^{t/2}\right) \left(\frac{2e^{t/2}}{t}\right)}{e^t}\,dt = -\int \frac{2e^t}{e^t}\,dt = -\int 2\,dt = \mathbf{-2t}$$
  * **For $u_2(t)$:**
    $$u_2(t) = \int \frac{y_1(t) g(t)}{W}\,dt = \int \frac{\left(e^{t/2}\right) \left(\frac{2e^{t/2}}{t}\right)}{e^t}\,dt = \int \frac{\frac{2e^t}{t}}{e^t}\,dt = \int \frac{2}{t}\,dt = 2\ln|t| = \mathbf{2\ln(t)} \quad (t > 0)$$
  
  * **Step 4: Form $y_p(t)$:**
  $$y_p(t) = u_1 y_1 + u_2 y_2 = (-2t)\left(e^{t/2}\right) + (2\ln t)\left(t e^{t/2}\right) = -2t e^{t/2} + 2t\ln(t)e^{t/2}$$
  
  * **Step 5: Assemble General Solution:**
  $$y(t) = c_1 e^{t/2} + c_2 t e^{t/2} - 2t e^{t/2} + 2t\ln(t)e^{t/2}$$
  *Notice that $-2t e^{t/2}$ has the exact same functional form as $c_2 t e^{t/2}$. It can be absorbed into the arbitrary constant: $(c_2 - 2) \to \tilde{c}_2$.*
  $$\mathbf{y(t) = c_1 e^{t/2} + \tilde{c}_2 t e^{t/2} + 2t\ln(t)e^{t/2}}$$
  
  ---
  #### **Example 2: Advanced Trigonometric Integral (Handwritten Notes PDF 5, Pages 2–3)**
    Solve:
    $$y'' + y = \frac{1}{1 + \cos^2(t)}$$
    
    * **Step 0:** Standard form is already satisfied ($g(t) = \frac{1}{1 + \cos^2(t)}$).
    * **Step 1: Homogeneous Solution:**
    $$r^2 + 1 = 0 \implies r = \pm i \implies y_1(t) = \cos(t), \quad y_2(t) = \sin(t)$$
    * **Step 2: Wronskian:**
    $$W = \begin{vmatrix} \cos(t) & \sin(t) \\ -\sin(t) & \cos(t) \end{vmatrix} = \cos^2(t) + \sin^2(t) = 1$$
    * **Step 3: Integrate for $u_1$ and $u_2$:**
    * **For $u_1(t)$:**
      $$u_1(t) = -\int \frac{\sin(t) \cdot \frac{1}{1 + \cos^2(t)}}{1}\,dt = \int \frac{-\sin(t)}{1 + \cos^2(t)}\,dt$$
      Let $u = \cos(t) \implies du = -\sin(t)\,dt$:
      $$u_1(t) = \int \frac{1}{1 + u^2}\,du = \arctan(u) = \mathbf{\arctan(\cos(t))}$$
    * **For $u_2(t)$:**
      $$u_2(t) = \int \frac{\cos(t) \cdot \frac{1}{1 + \cos^2(t)}}{1}\,dt = \int \frac{\cos(t)}{1 + (1 - \sin^2(t))}\,dt = \int \frac{\cos(t)}{2 - \sin^2(t)}\,dt$$
      Let $u = \sin(t) \implies du = \cos(t)\,dt$:
      $$\int \frac{1}{2 - u^2}\,du = \int \frac{1}{(\sqrt{2} - u)(\sqrt{2} + u)}\,du$$
      Using Partial Fractions:
      $$\frac{1}{(\sqrt{2} - u)(\sqrt{2} + u)} = \frac{1}{2\sqrt{2}}\left(\frac{1}{\sqrt{2} - u} + \frac{1}{\sqrt{2} + u}\right)$$
      $$u_2(t) = \frac{1}{2\sqrt{2}}\left[-\ln|\sqrt{2} - u| + \ln|\sqrt{2} + u|\right] = \mathbf{\frac{1}{2\sqrt{2}}\ln\left|\frac{\sqrt{2} + \sin(t)}{\sqrt{2} - \sin(t)}\right|}$$
    * **Step 4: Form $y_p(t)$:**
    $$y_p(t) = \cos(t)\arctan(\cos(t)) + \frac{\sin(t)}{2\sqrt{2}}\ln\left|\frac{\sqrt{2} + \sin(t)}{\sqrt{2} - \sin(t)}\right|$$
    * **Step 5: General Solution:**
    $$\mathbf{y(t) = c_1\cos(t) + c_2\sin(t) + \cos(t)\arctan(\cos(t)) + \frac{\sin(t)}{2\sqrt{2}}\ln\left|\frac{\sqrt{2} + \sin(t)}{\sqrt{2} - \sin(t)}\right|}$$
    
    ---
  #### **Example 3: Variable-Coefficient Equation when $y_1, y_2$ are Given (Major Exam 2 Problem!)**
    *Solve the nonhomogeneous equation:*
    $$t^2 y'' - 2t y' + 2y = t^3\ln(t), \quad t > 0$$
    *given that $y_1(t) = t$ and $y_2(t) = t^2$ are linearly independent solutions of the homogeneous equation.*
    
    * **Step 0: STANDARD FORM (MANDATORY!):**
    Divide the entire equation by $t^2$:
    $$y'' - \frac{2}{t}y' + \frac{2}{t^2}y = \frac{t^3\ln(t)}{t^2} = t\ln(t)$$
    $$\mathbf{g(t) = t\ln(t)}$$
    *(If you had used $g(t) = t^3\ln(t)$, you would get the entire problem wrong!)*
    
    * **Step 1: Complementary Solution:**
    $$y_h(t) = c_1 t + c_2 t^2$$
    
    * **Step 2: Wronskian:**
    $$W(y_1, y_2)(t) = \begin{vmatrix} t & t^2 \\ 1 & 2t \end{vmatrix} = t(2t) - (1)(t^2) = 2t^2 - t^2 = \mathbf{t^2}$$
    
    * **Step 3: Integrate for $u_1$ and $u_2$:**
    * **For $u_1(t)$:**
      $$u_1(t) = -\int \frac{y_2(t) g(t)}{W}\,dt = -\int \frac{t^2 \cdot (t\ln t)}{t^2}\,dt = -\int t\ln(t)\,dt$$
      Use Integration by Parts ($u = \ln t, dv = t\,dt \implies du = \frac{1}{t}dt, v = \frac{1}{2}t^2$):
      $$u_1(t) = -\left[\frac{1}{2}t^2\ln(t) - \int \frac{1}{2}t^2\left(\frac{1}{t}\right)dt\right] = -\frac{1}{2}t^2\ln(t) + \frac{1}{4}t^2$$
    
    * **For $u_2(t)$:**
      $$u_2(t) = \int \frac{y_1(t) g(t)}{W}\,dt = \int \frac{t \cdot (t\ln t)}{t^2}\,dt = \int \ln(t)\,dt$$
      Integration by Parts:
      $$u_2(t) = t\ln(t) - t$$
    
    * **Step 4: Form $y_p(t)$:**
    $$y_p(t) = u_1 y_1 + u_2 y_2 = \left(-\frac{1}{2}t^2\ln(t) + \frac{1}{4}t^2\right)t + \left(t\ln(t) - t\right)t^2$$
    $$y_p(t) = -\frac{1}{2}t^3\ln(t) + \frac{1}{4}t^3 + t^3\ln(t) - t^3 = \mathbf{\frac{1}{2}t^3\ln(t) - \frac{3}{4}t^3}$$
    
    * **Step 5: Full General Solution:**
    $$y(t) = c_1 t + c_2 t^2 + \frac{1}{2}t^3\ln(t) - \frac{3}{4}t^3$$
    
    ---
### Section 4.6 Pro-Tips & Exam Pitfalls Checklist
  * [x] **Did you divide by the leading coefficient?** Always check that $y''$ has coefficient $1$ before identifying $g(t)$.
  * [x] **Sign check on $u_1$ vs $u_2$:** Remember that **$u_1$ has a NEGATIVE sign** ($-\int \frac{y_2 g}{W} dt$) while **$u_2$ is POSITIVE** ($\int \frac{y_1 g}{W} dt$).
  * [x] **Wronskian order matters:** If you compute $W(y_2, y_1)$ instead of $W(y_1, y_2)$, the sign flips. Keep the order $(y_1, y_2)$ consistent everywhere.
  * [x] **Don't add $+C$ in $u_1, u_2$:** It is unnecessary and just clutters your algebra.
  * [x] **Absorption into $y_h$:** If any term in $y_p$ is a constant multiple of $y_1$ or $y_2$, it can be absorbed into $c_1$ or $c_2$.
  
  ---