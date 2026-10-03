### 1. The General Form & The Linear Operator
  
  The general second-order linear differential equation is written as:
  $$a_2(t)y'' + a_1(t)y' + a_0(t)y = f(t)$$
  
  Dividing by the leading coefficient $a_2(t)$ gives the **standard form**:
  $$y'' + p(t)y' + q(t)y = g(t)$$
  
  * If $g(t) = 0$, the equation is **homogeneous**:
  $$y'' + p(t)y' + q(t)y = 0 \quad (\ast)$$
  * If $g(t) \neq 0$, the equation is **nonhomogeneous**.
  
  ---
### 2. Existence and Uniqueness Theorem for 2nd-Order Linear IVPs
  
  For a first-order equation, an initial condition prescribes a starting position $y(t_0) = y_0$. For a second-order equation (like Newton's $F = ma$), you must specify **both** the initial position and the initial velocity:
  
  $$\begin{cases} y'' + p(t)y' + q(t)y = g(t) \\ y(t_0) = y_0 \\ y'(t_0) = y_1 \end{cases}$$
#### **The Theorem:**
  If the coefficient functions $p(t)$, $q(t)$, and the driving term $g(t)$ are **continuous** on an open interval $I = (a, b)$ containing the point $t_0$, then there exists **one and only one** solution $y = \phi(t)$ to the initial value problem, and that solution is valid on the **entire interval $I$**.
  
  * *Special Case:* If $p, q$ are constant real numbers, they are continuous everywhere on $(-\infty, \infty)$. Therefore, the solution to any constant-coefficient IVP exists uniquely for all time $t \in (-\infty, \infty)$.
  
  ---
### 3. The Superposition Principle (Linearity)
#### **Theorem: Superposition for Homogeneous Equations (Slides 4–9)**
  If $y_1(t)$ and $y_2(t)$ are two solutions to the homogeneous equation:
  $$y'' + p(t)y' + q(t)y = 0$$
  then any linear combination:
  $$y(t) = c_1 y_1(t) + c_2 y_2(t)$$
  is **also** a solution for any constants $c_1, c_2 \in \mathbb{R}$.
#### **Direct Proof (from the lecture slides):**
  Let $L[y] = y'' + p(t)y' + q(t)y$. Because differentiation is a linear operation:
  $$L[c_1 y_1 + c_2 y_2] = (c_1 y_1 + c_2 y_2)'' + p(t)(c_1 y_1 + c_2 y_2)' + q(t)(c_1 y_1 + c_2 y_2)$$
  $$= \left(c_1 y_1'' + c_2 y_2''\right) + p(t)\left(c_1 y_1' + c_2 y_2'\right) + q(t)\left(c_1 y_1 + c_2 y_2\right)$$
  Regrouping terms by constants $c_1$ and $c_2$:
  $$= c_1 \underbrace{\left(y_1'' + p(t)y_1' + q(t)y_1\right)}_{= 0 \text{ since } y_1 \text{ is a solution}} + c_2 \underbrace{\left(y_2'' + p(t)y_2' + q(t)y_2\right)}_{= 0 \text{ since } y_2 \text{ is a solution}}$$
  $$= c_1(0) + c_2(0) = 0 \quad \blacksquare$$
  
  ---
### 4. Linear Independence and The Wronskian
  
  Superposition gives us an infinite family of solutions $y = c_1 y_1 + c_2 y_2$. But does this family include **every possible solution**? Can we always find $c_1$ and $c_2$ to satisfy arbitrary initial conditions $y(t_0) = y_0$ and $y'(t_0) = y_1$?
#### A. The Algebraic Derivation (Slides 10–13)
  To satisfy the initial conditions at $t_0$, we must solve the system:
  $$\begin{cases} c_1 y_1(t_0) + c_2 y_2(t_0) = y_0 \\ c_1 y_1'(t_0) + c_2 y_2'(t_0) = y_1 \end{cases}$$
  
  In matrix form:
  $$\begin{pmatrix} y_1(t_0) & y_2(t_0) \\ y_1'(t_0) & y_2'(t_0) \end{pmatrix} \begin{pmatrix} c_1 \\ c_2 \end{pmatrix} = \begin{pmatrix} y_0 \\ y_1 \end{pmatrix}$$
  
  By linear algebra (or Cramer's Rule), this $2 \times 2$ linear system has a **unique solution $(c_1, c_2)$** if and only if the determinant of the coefficient matrix is **nonzero**:
  
  $$\det \begin{pmatrix} y_1(t_0) & y_2(t_0) \\ y_1'(t_0) & y_2'(t_0) \end{pmatrix} = y_1(t_0)y_2'(t_0) - y_1'(t_0)y_2(t_0) \neq 0$$
  
  ---
#### B. Definition: The Wronskian Determinant (Slide 14)
  > **The Wronskian** of two differentiable functions $y_1(t)$ and $y_2(t)$ is defined as:
  > $$W(y_1, y_2)(t) = \begin{vmatrix} y_1(t) & y_2(t) \\ y_1'(t) & y_2'(t) \end{vmatrix} = y_1(t)y_2'(t) - y_1'(t)y_2(t)$$
#### C. Linear Independence vs. Linear Dependence (Slide 17)
  * **Linearly Dependent:** Two functions are linearly dependent on an interval $I$ if one is a constant multiple of the other:
  $$y_2(t) = k \cdot y_1(t) \quad \text{for all } t \in I$$
  *(If so, $W(y_1, ky_1) = y_1(ky_1') - y_1'(ky_1) = 0$ everywhere).*
  * **Linearly Independent:** Neither function is a constant multiple of the other on $I$ (i.e., their ratio $\frac{y_2(t)}{y_1(t)}$ is **not** constant).
#### D. Fundamental Set of Solutions & General Solution (Slide 37)
  > If $y_1$ and $y_2$ are two solutions to $y'' + p(t)y' + q(t)y = 0$ on an interval $I$, and their Wronskian $W(y_1, y_2)(t) \neq 0$ at some point $t_0 \in I$:
  > 1. $\{y_1, y_2\}$ is called a **Fundamental Set of Solutions**.
  > 2. The linear combination:
  >    $$y(t) = c_1 y_1(t) + c_2 y_2(t)$$
  >    is the **General Solution** (it accounts for *all* possible solutions on $I$).
  
  ---
### 5. Filling the Slide Gap: Abel's Theorem
  
  A natural question arises: *Could $W(y_1, y_2)(t)$ be nonzero at some point $t_0$, but equal zero at another point $t_1$?*
#### **Abel’s Identity:**
  If $y_1$ and $y_2$ are solutions to $y'' + p(t)y' + q(t)y = 0$, their Wronskian satisfies the first-order differential equation:
  $$\frac{dW}{dt} + p(t)W = 0$$
  Solving this by separation of variables / integrating factor:
  $$W(t) = C e^{-\int p(t)\,dt}$$
#### **The Profound Consequence of Abel's Theorem:**
  Because the exponential function $e^{-\int p(t)dt}$ is **strictly positive and never zero**:
  * If $C \neq 0 \implies W(t) \neq 0$ for **all** $t \in I$.
  * If $C = 0 \implies W(t) = 0$ for **all** $t \in I$.
  
  > **Takeaway:** The Wronskian of two solutions to a linear homogeneous ODE is **either never zero, or identically zero everywhere**. You only need to test $W$ at a single convenient point (like $t = 0$ or $t = 1$).
  
  ---
### 6. Worked Exam Problems on the Wronskian
  
  ---
#### **Example 1: Wronskian of Distinct Exponentials (Slides 18–28 & 38–45)**
  Verify that $y_1 = e^{-2t}$ and $y_2 = e^{-3t}$ solve $y'' + 5y' + 6y = 0$, compute their Wronskian, and determine linear independence.
  
  1. **Verify Solutions:**
   * $y_1 = e^{-2t} \implies y_1' = -2e^{-2t} \implies y_1'' = 4e^{-2t}$:
     $$4e^{-2t} + 5(-2e^{-2t}) + 6(e^{-2t}) = (4 - 10 + 6)e^{-2t} = 0 \quad \checkmark$$
   * $y_2 = e^{-3t} \implies y_2' = -3e^{-3t} \implies y_2'' = 9e^{-3t}$:
     $$9e^{-3t} + 5(-3e^{-3t}) + 6(e^{-3t}) = (9 - 15 + 6)e^{-3t} = 0 \quad \checkmark$$
  
  2. **Compute Wronskian:**
   $$W(y_1, y_2) = \begin{vmatrix} e^{-2t} & e^{-3t} \\ -2e^{-2t} & -3e^{-3t} \end{vmatrix}$$
   $$= e^{-2t}(-3e^{-3t}) - (-2e^{-2t})(e^{-3t}) = -3e^{-5t} + 2e^{-5t} = -e^{-5t}$$
  
  3. **Linear Independence:**
   Since $-e^{-5t} \neq 0$ for all real $t$, $y_1$ and $y_2$ are **linearly independent** and form a fundamental set of solutions.
   $$\text{General Solution: } y(t) = c_1 e^{-2t} + c_2 e^{-3t}$$
  
  ---
#### **General Case: $y_1 = e^{r_1 t}$ and $y_2 = e^{r_2 t}$ (Slide 38)**
  $$W(e^{r_1 t}, e^{r_2 t}) = \begin{vmatrix} e^{r_1 t} & e^{r_2 t} \\ r_1 e^{r_1 t} & r_2 e^{r_2 t} \end{vmatrix} = r_2 e^{(r_1+r_2)t} - r_1 e^{(r_1+r_2)t} = (r_2 - r_1)e^{(r_1+r_2)t}$$
  * Since $e^{(r_1+r_2)t} \neq 0$, $W \neq 0$ **if and only if $r_1 \neq r_2$**.
  * Any two distinct exponential roots always form a fundamental set.
  
  ---
#### **Example 2: Spring 2014 Exam 1 #5 (Slides 29–36)**
  The Wronskian of $f(t)$ and $g(t)$ is $t$. If $f(t) = \frac{1}{2}$ and $g(0) = 1$, find $g(t)$.
  
  1. **Set up Wronskian definition:**
   $$W(f, g) = f g' - f' g = t$$
  2. **Compute derivative of $f(t)$:**
   $$f(t) = \frac{1}{2} \implies f'(t) = 0$$
  3. **Substitute into formula:**
   $$\frac{1}{2}g'(t) - (0)g(t) = t \implies \frac{1}{2}g'(t) = t \implies g'(t) = 2t$$
  4. **Integrate:**
   $$g(t) = \int 2t\,dt = t^2 + C$$
  5. **Apply initial condition $g(0) = 1$:**
   $$1 = (0)^2 + C \implies C = 1$$
  6. **Final Answer:**
   $$g(t) = t^2 + 1$$
  
  ---
#### **Example 3: Spring 2012 Final Exam #3 (Slide 48 — Fully Solved)**
  The Wronskian of $f(t)$ and $g(t)$ is $-3$. If $f(t) = t^2$, find $g(t)$.
  
  1. **Set up Wronskian equation:**
   $$W(f, g) = f g' - f' g = -3$$
  2. **Compute $f'(t)$:**
   $$f(t) = t^2 \implies f'(t) = 2t$$
  3. **Substitute:**
   $$t^2 g' - 2t g = -3$$
  4. **Solve for $g(t)$ as a 1st-order linear ODE (connecting to Section 2.3!):**
   * Standard form (divide by $t^2$):
     $$g' - \frac{2}{t}g = -\frac{3}{t^2}$$
   * Integrating Factor:
     $$\mu(t) = e^{\int -\frac{2}{t}dt} = e^{-2\ln|t|} = t^{-2} = \frac{1}{t^2}$$
   * Multiply and collapse:
     $$\frac{d}{dt}\left[\frac{1}{t^2}g\right] = \frac{1}{t^2}\left(-\frac{3}{t^2}\right) = -3t^{-4}$$
   * Integrate:
     $$\frac{1}{t^2}g = \int -3t^{-4}\,dt = t^{-3} + C = \frac{1}{t^3} + C$$
   * Multiply by $t^2$:
     $$g(t) = \frac{1}{t} + C t^2$$
  
  ---
### Section 4.2 (Part 1) Summary Checklist
  * [x] **Superposition:** $c_1 y_1 + c_2 y_2$ is a solution only for **homogeneous linear** equations.
  * [x] **Initial Value Problem:** Needs 2 conditions ($y(t_0) = y_0$ and $y'(t_0) = y_1$).
  * [x] **Wronskian Formula:** $W(y_1, y_2) = y_1 y_2' - y_1' y_2$.
  * [x] **Fundamental Set:** If $W(y_1, y_2) \neq 0$, then $\{y_1, y_2\}$ forms a basis for all solutions.
  * [x] **Abel’s Theorem:** $W(t) = Ce^{-\int p(t)dt}$. $W$ is either never zero or zero everywhere.
  * [x] **Wronskian Exam Trick:** If asked to find an unknown function $g(t)$ given $W(f, g)$ and $f(t)$, expand $f g' - f' g = W$ and solve it as a **Section 2.3 first-order linear ODE**!
  
  ---