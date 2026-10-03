### 1. What is a Solution to a Differential Equation?
#### A. Formal Definition: Explicit Solution
  > **Explicit Solution:** A function $\phi(x)$ that is differentiable on an interval $I$ is called an **explicit solution** to an $n$-th order differential equation if, when substituting $y = \phi(x), y' = \phi'(x), \dots, y^{(n)} = \phi^{(n)}(x)$, the equation reduces to an identity for all $x \in I$.
  
  *Two key requirements:*
  1. $\phi(x)$ must possess at least $n$ derivatives on the interval $I$.
  2. Substituting $\phi(x)$ and its derivatives into the equation makes the left-hand side identically equal to the right-hand side.
  
  ---
#### B. What about Implicit Solutions? *(Filling the slide gap)*
  * An **implicit solution** is a relation $G(x, y) = 0$ that defines one or more explicit solutions implicitly on an interval $I$.
  * *Example:* $x^2 + y^2 = C$ is an implicit solution to $y' = -\frac{x}{y}$. Solving for $y$ yields explicit solutions $y = \pm\sqrt{C - x^2}$.
  
  ---
#### C. The Interval of Validity / Definition ($I$) *(Critical Concept)*
  A solution is **meaningless without its interval of definition**. An interval must be a **single, connected open or closed interval** on the real line (no gaps or isolated points).
  
  * **From your handwritten notes:**
  * **Equation:** $xy' + y = 0$
  * **Proposed solution:** $y(x) = \frac{1}{x}$
  * **Check:** $y'(x) = -\frac{1}{x^2} \implies x\left(-\frac{1}{x^2}\right) + \frac{1}{x} = -\frac{1}{x} + \frac{1}{x} = 0 \quad \checkmark$
  * **Valid intervals:** The function $\frac{1}{x}$ is undefined and discontinuous at $x = 0$. Therefore, the interval of definition cannot cross $0$. A valid solution must specify either $I = (0, \infty)$ or $I = (-\infty, 0)$, **not** $(-\infty, 0) \cup (0, \infty)$.
  
  ---
### 2. General Solutions vs. Particular Solutions & Initial Value Problems (IVPs)
  
  * **General Solution:** A family of solutions containing arbitrary constants ($c_1, c_2, \dots, c_n$). An $n$-th order ODE typically has an $n$-parameter family of solutions.
  * **Particular Solution:** A single specific solution obtained by choosing specific values for the arbitrary constants, usually determined by **initial conditions**.
  * **Initial Value Problem (IVP):** A differential equation paired with auxiliary conditions prescribed at a single point $t_0$:
  $$y' = f(t, y), \quad y(t_0) = y_0$$
  *(For an $n$-th order equation, you need $n$ conditions: $y(t_0), y'(t_0), \dots, y^{(n-1)}(t_0)$).*
  
  ---
### 3. Verification Examples
#### **Example 1: Verifying a 2nd-Order IVP (Slide 70)**
  Show that $\phi(x) = \sin(x) - \cos(x)$ satisfies the IVP:
  $$\frac{d^2y}{dx^2} + y = 0, \quad y(0) = -1, \quad y'(0) = 1$$
  
  * **Step 1: Compute the necessary derivatives:**
  $$\phi'(x) = \cos(x) - (-\sin(x)) = \cos(x) + \sin(x)$$
  $$\phi''(x) = -\sin(x) + \cos(x)$$
  
  * **Step 2: Substitute into the ODE:**
  $$\phi''(x) + \phi(x) = [-\sin(x) + \cos(x)] + [\sin(x) - \cos(x)] = 0 \quad \checkmark$$
  
  * **Step 3: Check initial conditions at $x = 0$:**
  $$\phi(0) = \sin(0) - \cos(0) = 0 - 1 = -1 \quad \checkmark$$
  $$\phi'(0) = \cos(0) + \sin(0) = 1 + 0 = 1 \quad \checkmark$$
  * **Conclusion:** Both the ODE and the initial conditions are satisfied.
  
  ---
#### **Example 2: Verification from Handwritten Notes**
  Verify that $y(t) = 3t + t^2$ is an explicit solution to $ty' - y = t^2$ on $(-\infty, \infty)$.
  * **Derivative:** $y'(t) = 3 + 2t$
  * **Substitute:** 
  $$t(3 + 2t) - (3t + t^2) = 3t + 2t^2 - 3t - t^2 = t^2 \quad \checkmark$$
  * Since $y(t)$ and $y'(t)$ are polynomials, they are differentiable everywhere, so $I = (-\infty, \infty)$.
  
  ---
### 4. First Solving Technique: Separation of Variables / Chain Rule Method
  
  Both Example 2 in the slides and the coffee cooling problem in your handwritten notes use the same technique to solve a first-order autonomous ODE.
#### **Walkthrough: Population Equation (Slides 81–101)**
  Solve the IVP:
  $$\frac{dp}{dt} = 0.5p - 450, \quad p(0) = 850$$
  
  1. **Factor the right-hand side:**
   $$\frac{dp}{dt} = 0.5(p - 900)$$
  2. **Separate variables (assuming $p \neq 900$):**
   $$\frac{1}{p - 900} \frac{dp}{dt} = 0.5 \implies \frac{1}{p - 900}\,dp = 0.5\,dt$$
  3. **Integrate both sides:**
   $$\int \frac{1}{p - 900}\,dp = \int 0.5\,dt \implies \ln|p - 900| = 0.5t + c_1$$
  4. **Exponentiate to solve for $p(t)$:**
   $$|p - 900| = e^{0.5t + c_1} = e^{c_1}e^{0.5t} \implies p - 900 = \pm e^{c_1}e^{0.5t}$$
   Let $c = \pm e^{c_1}$. Notice that if $c = 0$, $p(t) = 900$, which is the constant **equilibrium solution** (where $\frac{dp}{dt} = 0$). Thus, $c$ can be any real constant:
   $$p(t) = 900 + c e^{0.5t} \quad (\text{General Solution})$$
  5. **Apply the initial condition $p(0) = 850$:**
   $$850 = 900 + c e^0 \implies c = 850 - 900 = -50$$
  6. **Final Particular Solution:**
   $$p(t) = 900 - 50e^{0.5t}$$
  
  *(Note: The exact same steps yield $T(t) = 70 + 130e^{-t/10}$ for the coffee problem with $T(0) = 200$.)*
  
  ---
### 5. Exam Technique: The Power Function Ansatz ($y = t^r$)
  
  When dealing with Euler-Cauchy type equations (where the power of $t$ matches the order of the derivative, such as $t^2 y''$, $t y'$), we guess solutions of the form **$y = t^r$** for $t > 0$.
#### **Example 3: Fall 2013 Exam Problem (Slide 102)**
  Find all values of $r$ for which $9t^2 y'' - 3t y' + 4y = 0$ has solutions of the form $y = t^r$ ($t > 0$).
  * Compute derivatives:
  $$y = t^r \implies y' = r t^{r-1} \implies y'' = r(r-1)t^{r-2}$$
  * Substitute into the ODE:
  $$9t^2 [r(r-1)t^{r-2}] - 3t [r t^{r-1}] + 4[t^r] = 0$$
  $$9r(r-1)t^r - 3rt^r + 4t^r = 0$$
  * Since $t > 0$, factor out $t^r \neq 0$:
  $$9r(r-1) - 3r + 4 = 0$$
  $$9r^2 - 9r - 3r + 4 = 0 \implies 9r^2 - 12r + 4 = 0$$
  * Factor the characteristic polynomial:
  $$(3r - 2)^2 = 0 \implies r = \frac{2}{3}$$
  
  ---
#### **Example 4: Fall 2012 Exam Problem (Slide 114 — Fully Solved)**
  Find all values of $r$ for which $4t^2 y'' + 12t y' + 3y = 0$ has solutions of the form $y = t^r$ ($t > 0$).
  * Substitute $y = t^r$, $y' = rt^{r-1}$, $y'' = r(r-1)t^{r-2}$:
  $$4t^2 [r(r-1)t^{r-2}] + 12t [r t^{r-1}] + 3[t^r] = 0$$
  $$[4r(r-1) + 12r + 3] t^r = 0$$
  * Divide by $t^r \neq 0$:
  $$4r^2 - 4r + 12r + 3 = 0 \implies 4r^2 + 8r + 3 = 0$$
  * Factor:
  $$(2r + 1)(2r + 3) = 0 \implies r = -\frac{1}{2}, \quad r = -\frac{3}{2}$$
  
  ---
### 6. Theory: Existence and Uniqueness Theorem (Picard-Lindelöf)
  
  This is one of the most heavily tested theoretical concepts in first-order ODEs.
  
  ```
       d +-------------------------------+
         |       Rectangle R             |
         |                               |
         |           (t0, y0)            |
         |              *                |
         |                               |
       c +-------------------------------+
         a              t0               b
  ```
#### **The Theorem (Slide 115):**
  Consider the initial value problem:
  $$y' = f(t, y), \quad y(t_0) = y_0$$
  Suppose there is an open rectangle $R = \{(t, y) : a < t < b,\; c < y < d\}$ containing the point $(t_0, y_0)$ such that:
  1. **Condition 1 (Existence):** $f(t, y)$ is **continuous** on $R$.
  2. **Condition 2 (Uniqueness):** $\frac{\partial f}{\partial y}$ is **continuous** on $R$.
  
  **Conclusion:** 
  Then there exists some open interval $(t_0 - \delta, t_0 + \delta) \subseteq (a, b)$ on which there exists a **unique** solution $y = \phi(t)$ satisfying the IVP.
  
  ---
#### **Crucial Clarifications:**
  * **Local Theorem:** It only guarantees a solution exists **near** $t_0$ (within $\delta$), not for all $t$.
  * **Sufficient, Not Necessary:** 
  * If the hypotheses hold $\implies$ existence and uniqueness are **guaranteed**.
  * If the hypotheses fail $\implies$ **the theorem says nothing**! The IVP might have no solution, a unique solution, or infinitely many solutions.
  
  ---
### 7. Core Examples for Existence & Uniqueness
#### **Case 1: Standard Application (Example 5, Slide 116)**
  Determine where the IVP has a unique solution:
  $$\frac{dy}{dt} = \frac{3t^2 + 4t + 2}{2(y - 1)}, \quad y(0) = -1$$
  
  1. Identify $f(t, y)$ and $(t_0, y_0)$:
   $$f(t, y) = \frac{3t^2 + 4t + 2}{2(y - 1)}, \quad (t_0, y_0) = (0, -1)$$
  2. Compute $\frac{\partial f}{\partial y}$:
   $$\frac{\partial f}{\partial y} = -\frac{3t^2 + 4t + 2}{2(y - 1)^2}$$
  3. Check continuity:
   * Both $f$ and $\frac{\partial f}{\partial y}$ are rational functions. They are continuous everywhere **except along the horizontal line $y = 1$**.
  4. Check the initial point:
   * $(t_0, y_0) = (0, -1)$ does not lie on $y = 1$.
  5. **Conclusion:** Any rectangle $R$ containing $(0, -1)$ that does **not** cross the line $y = 1$ (for instance, the lower half-plane $-\infty < t < \infty, -\infty < y < 1$) guarantees a unique solution.
  
  ---
#### **Case 2: When the Point Lies on the Singularity (Example 6, Slide 127)**
  What if the initial condition is changed to $y(0) = 1$?
  * Here, $(t_0, y_0) = (0, 1)$ lies directly on the line $y = 1$ where $f$ and $\frac{\partial f}{\partial y}$ are undefined/discontinuous.
  * It is impossible to draw any rectangle around $(0, 1)$ without containing points where $y = 1$.
  * Therefore, the theorem **does not guarantee uniqueness or existence**.
  * *What actually happens?* 
  Separating variables:
  $$2(y - 1)\,dy = (3t^2 + 4t + 2)\,dt \implies (y - 1)^2 = t^3 + 2t^2 + 2t + C$$
  Applying $y(0) = 1 \implies C = 0 \implies (y - 1)^2 = t^3 + 2t^2 + 2t$:
  $$y(t) = 1 \pm \sqrt{t^3 + 2t^2 + 2t}$$
  There are **two distinct solutions** branching from $t = 0$. Uniqueness fails!
  
  ---
#### **Case 3: Infinite Solutions / Failure of $\frac{\partial f}{\partial y}$ (Example 7, Slide 132)**
  Consider:
  $$y' = y^{1/3}, \quad y(0) = 0$$
  
  1. Identify $f$ and $\frac{\partial f}{\partial y}$:
   $$f(t, y) = y^{1/3} \quad (\text{continuous everywhere on }\mathbb{R}^2)$$
   $$\frac{\partial f}{\partial y} = \frac{1}{3}y^{-2/3} = \frac{1}{3 y^{2/3}} \quad (\text{undefined and discontinuous at } y = 0)$$
  2. Since $(t_0, y_0) = (0, 0)$ lies on $y = 0$, $\frac{\partial f}{\partial y}$ is not continuous on any rectangle containing the origin. The theorem provides no guarantee.
  3. *What actually happens?*
   * Solution 1: By inspection, the constant function $y(t) \equiv 0$ is a valid solution ($0' = 0^{1/3}$).
   * Solution 2: Separation of variables:
     $$\int y^{-1/3}\,dy = \int dt \implies \frac{3}{2}y^{2/3} = t + c$$
     Since $y(0) = 0 \implies c = 0$, we get:
     $$y(t) = \pm \left(\frac{2}{3}t\right)^{3/2}$$
   * Solution 3 (Infinitely many!): You can stay at $y = 0$ until any time $t_0 > 0$, and then branch off:
     $$y(t) = \begin{cases} 0, & 0 \le t < t_0 \\ \pm\left[\frac{2}{3}(t - t_0)\right]^{3/2}, & t \ge t_0 \end{cases}$$
     Every choice of $t_0 > 0$ gives a valid, differentiable solution satisfying $y(0) = 0$. There are **infinitely many solutions**!
  
  ---
### Section 1.2 Summary Checklist
  * [x] **Verification:** To check if $\phi(x)$ is a solution, calculate its derivatives and substitute both sides to see if they are identical.
  * [x] **Interval of Definition:** Watch out for division by zero (e.g., $y = 1/x$ cannot include $x = 0$).
  * [x] **Ansatz $y = t^r$:** Plug in, factor out $t^r$, and solve the resulting polynomial in $r$.
  * [x] **Picard Theorem:** 
  * $f$ continuous $\implies$ at least one solution exists.
  * Both $f$ and $\frac{\partial f}{\partial y}$ continuous $\implies$ the solution is unique.
  * If $\frac{\partial f}{\partial y}$ blows up at the initial point $\implies$ uniqueness typically fails (expect multiple or infinitely many solutions).
  
  ---