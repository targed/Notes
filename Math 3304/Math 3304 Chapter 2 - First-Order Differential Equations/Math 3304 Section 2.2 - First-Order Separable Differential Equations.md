### 1. Definition and Recognition
  
  A first-order ordinary differential equation is called **separable** if the derivative $\frac{dy}{dx}$ can be factored as the product (or quotient) of a function purely of the independent variable and a function purely of the dependent variable:
  
  $$\frac{dy}{dx} = g(x) \, h(y) \quad \text{or} \quad \frac{dy}{dx} = \frac{g(x)}{h(y)}$$
#### **How to spot a separable equation instantly:**
  Ask yourself: *Can I isolate all expressions involving $y$ onto the side with $dy$, and all expressions involving $x$ (or $t$) onto the side with $dx$ using purely multiplication and division?*
  
  * **Separable:**
  * $\frac{dy}{dx} = x^2 y^3 \implies \frac{1}{y^3} \, dy = x^2 \, dx$
  * $y' = t e^{t-y} = t e^t e^{-y} \implies e^y \, dy = t e^t \, dt$
  * $y' = 2t + t y = t(2 + y) \implies \frac{1}{2+y} \, dy = t \, dt$
  * **Not Separable:**
  * $y' = x + y$ (cannot factor addition into a product of single-variable functions)
  * $y' = \sin(x y)$
  
  ---
### 2. The Theoretical Justification: Why the "Leibniz Trick" Works
  
  In introductory calculus, $\frac{dy}{dx}$ is defined as a limit of difference quotients, **not** an algebraic fraction. Why are we allowed to "multiply by $dx$"?
#### **The Formal Derivation via the Chain Rule:**
  Given the equation:
  $$\frac{dy}{dx} = g(x) \, h(y)$$
  Assuming $h(y) \neq 0$, divide both sides by $h(y)$:
  $$\frac{1}{h(y)} \frac{dy}{dx} = g(x)$$
  Now integrate both sides with respect to the independent variable $x$:
  $$\int \frac{1}{h(y(x))} \frac{dy}{dx} \, dx = \int g(x) \, dx$$
  By the **Change of Variables Theorem ($u$-substitution)** from Calculus I, let $u = y(x)$. Then the differential is $du = y'(x) \, dx = \frac{dy}{dx} \, dx$:
  $$\int \frac{1}{h(u)} \, du = \int g(x) \, dx$$
  Replacing the dummy variable $u$ back with $y$ gives:
  $$\int \frac{1}{h(y)} \, dy = \int g(x) \, dx$$
  > **Key Takeaway:** Treating $\frac{dy}{dx}$ as an algebraic fraction and "multiplying by $dx$" is mathematically sound because it is simply shorthand for the **Chain Rule in reverse**.
  
  ---
### 3. Step-by-Step Solution Algorithm
  
  1. **Write derivative in Leibniz notation:** Replace $y'$ with $\frac{dy}{dx}$ (or $\frac{dy}{dt}$).
  2. **Find Equilibrium (Constant) Solutions:**
   * Set $h(y) = 0$. Any constant value $y = c$ that satisfies this is an **equilibrium solution**. 
   * *Note these down immediately!*
  3. **Separate variables:** Move all terms with $y$ to the left side with $dy$, and all terms with $x$ to the right side with $dx$:
   $$\frac{1}{h(y)} \, dy = g(x) \, dx$$
  4. **Integrate both sides:**
   $$\int \frac{1}{h(y)} \, dy = \int g(x) \, dx + C$$
   *(Place a single constant of integration $+C$ on the independent variable side).*
  5. **Determine the Constant $C$ (if an Initial Condition is provided):**
   * **Best Practice:** Plug in the initial values $(x_0, y_0)$ **immediately after integrating**. It is algebraically much cleaner to find $C$ before manipulating complicated expressions.
  6. **Solve explicitly for $y$ (if possible):**
   * If isolating $y$ requires solving transcendental mixtures (like $y^2 + e^y$), leave the answer as an **implicit solution**.
   * If taking a square root ($\pm\sqrt{\dots}$), **choose the sign** that matches the initial condition.
  
  ---
### 4. The Critical Exam Pitfall: "Lost" or Singular Solutions
  
  When you divide by $h(y)$, you are implicitly assuming $h(y) \neq 0$. If $h(k) = 0$ for some number $k$, then the constant function **$y(x) \equiv k$** is an equilibrium solution.
  
  * Sometimes, the constant solution can be recovered from the general formula by setting the constant $C = 0$ (or $C \to \infty$).
  * **However, sometimes it cannot be obtained for ANY value of $C$.** Such a solution is called a **singular solution**.
  * On exams, questions will explicitly ask: *"Find all solutions, including any singular/constant solutions."* If you fail to check $h(y) = 0$, you will lose points.
  
  ---
### 5. In-Depth Worked Examples from Slides & Lecture Notes
  
  ---
#### **Example 1: An Implicit Solution (Slide Deck, Example 1)**
  $$\frac{dy}{dx} = \frac{x - e^x}{y + e^y}$$
  
  * **Step 1: Separate variables:**
  $$(y + e^y) \, dy = (x - e^x) \, dx$$
  * **Step 2: Integrate both sides:**
  $$\int (y + e^y) \, dy = \int (x - e^x) \, dx$$
  $$\frac{1}{2}y^2 + e^y = \frac{1}{2}x^2 - e^x + C$$
  * **Step 3: Can we solve for $y$?**
  * The left side contains both an algebraic term ($\frac{1}{2}y^2$) and an exponential term ($e^y$). It is impossible to isolate $y$ in terms of elementary functions.
  * **Final Answer (Implicit Solution):**
  $$\frac{1}{2}y^2 + e^y - \frac{1}{2}x^2 + e^x = C$$
  
  ---
#### **Example 2: Factoring, $u$-Substitution, and Choosing Signs (Fall 2015 Exam 1 #2)**
  Solve the initial value problem:
  $$y' = \frac{2t}{y + t^2 y}, \quad y(0) = -2$$
  
  * **Step 1: Factor the denominator:**
  $$y + t^2 y = y(1 + t^2) \implies \frac{dy}{dt} = \frac{2t}{y(1 + t^2)}$$
  * **Step 2: Separate variables:**
  $$y \, dy = \frac{2t}{1 + t^2} \, dt$$
  * **Step 3: Integrate both sides:**
  $$\int y \, dy = \int \frac{2t}{1 + t^2} \, dt$$
  * For the right side, let $u = 1 + t^2 \implies du = 2t \, dt$:
    $$\int \frac{1}{u} \, du = \ln|u| = \ln(1 + t^2) \quad (\text{since } 1 + t^2 > 0 \text{ always})$$
  $$\frac{1}{2}y^2 = \ln(1 + t^2) + C_1 \implies y^2 = 2\ln(1 + t^2) + C \quad (\text{where } C = 2C_1)$$
  * **Step 4: Apply Initial Condition ($t = 0, y = -2$):**
  $$(-2)^2 = 2\ln(1 + 0^2) + C \implies 4 = 2(0) + C \implies C = 4$$
  * **Step 5: Solve for $y(t)$:**
  $$y^2 = 2\ln(1 + t^2) + 4 \implies y(t) = \pm\sqrt{2\ln(1 + t^2) + 4}$$
  * Since our initial condition requires $y(0) = -2 < 0$, we **must select the negative branch**:
    $$y(t) = -\sqrt{2\ln(1 + t^2) + 4}$$
  
  ---
#### **Example 3: Radical Powers & Branching / Picard Violation (Handwritten Notes)**
  Solve the IVP:
  $$y' = -(t + 1)y^{1/2}, \quad y(0) = 1$$
  
  * **Step 1: Separate variables:**
  $$y^{-1/2} \, dy = -(t + 1) \, dt$$
  * **Step 2: Integrate:**
  $$\int y^{-1/2} \, dy = -\int (t + 1) \, dt \implies 2y^{1/2} = -\frac{1}{2}t^2 - t + C$$
  * **Step 3: Apply $y(0) = 1$:**
  $$2(1)^{1/2} = 0 - 0 + C \implies C = 2$$
  * **Step 4: Solve for $y(t)$:**
  $$2y^{1/2} = -\frac{1}{2}t^2 - t + 2 \implies y^{1/2} = -\frac{1}{4}t^2 - \frac{1}{2}t + 1$$
  $$y(t) = \left(-\frac{1}{4}t^2 - \frac{1}{2}t + 1\right)^2$$
##### **Critical Follow-Up Questions from Notes:**
  * **What if $y(0) = -1$?**
  * Substituting into $2y^{1/2} = C$ gives $2\sqrt{-1} = C$. Over real numbers, $\sqrt{-1}$ does not exist. **Conclusion:** No real solution exists.
  * **What if $y(0) = 0$?**
  * Substituting gives $C = 0 \implies y_1(t) = \left(-\frac{1}{4}t^2 - \frac{1}{2}t\right)^2$.
  * But notice that setting $y \equiv 0$ in the original ODE gives $0' = -(t+1)(0) = 0 \implies y_2(t) \equiv 0$ is **also** a solution!
  * Thus, at $(0,0)$, uniqueness fails because $\frac{\partial f}{\partial y} = -\frac{t+1}{2\sqrt{y}}$ blows up at $y = 0$ (violating the Picard Theorem).
  
  ---
#### **Example 4: Partial Fractions and The Missed Solution (Handwritten Notes)**
  Find all solutions of:
  $$y' = y^2 - 4$$
  
  * **Step 1: Check for equilibrium solutions ($y' = 0$):**
  $$y^2 - 4 = 0 \implies (y - 2)(y + 2) = 0 \implies y(t) \equiv 2 \quad \text{and} \quad y(t) \equiv -2$$
  * **Step 2: Separate variables:**
  $$\frac{1}{y^2 - 4} \, dy = dt \implies \frac{1}{(y - 2)(y + 2)} \, dy = dt$$
  * **Step 3: Partial Fraction Decomposition:**
  $$\frac{1}{(y - 2)(y + 2)} = \frac{A}{y - 2} + \frac{B}{y + 2}$$
  $$1 = A(y + 2) + B(y - 2)$$
  * Let $y = 2 \implies 1 = 4A \implies A = \frac{1}{4}$
  * Let $y = -2 \implies 1 = -4B \implies B = -\frac{1}{4}$
  * **Step 4: Integrate:**
  $$\frac{1}{4}\int \left(\frac{1}{y - 2} - \frac{1}{y + 2}\right) dy = \int dt$$
  $$\frac{1}{4} \ln\left|\frac{y - 2}{y + 2}\right| = t + c \implies \ln\left|\frac{y - 2}{y + 2}\right| = 4t + 4c$$
  * **Step 5: Exponentiate and absorb constants:**
  $$\left|\frac{y - 2}{y + 2}\right| = e^{4c} e^{4t} \implies \frac{y - 2}{y + 2} = \pm e^{4c} e^{4t} = C_0 e^{4t} \quad (C_0 \neq 0)$$
  * **Step 6: Solve explicitly for $y$:**
  $$y - 2 = C_0 e^{4t}(y + 2) = C_0 e^{4t} y + 2 C_0 e^{4t}$$
  $$y - C_0 e^{4t} y = 2 + 2 C_0 e^{4t} \implies y(1 - C_0 e^{4t}) = 2(1 + C_0 e^{4t})$$
  $$y(t) = 2\left(\frac{1 + C_0 e^{4t}}{1 - C_0 e^{4t}}\right)$$
##### **The Constant Solution Analysis:**
  * If we let $C_0 = 0$, we get $y = 2\left(\frac{1+0}{1-0}\right) = 2$, which recovers the equilibrium solution $y \equiv 2$.
  * However, **no finite value of $C_0$** can ever produce $y(t) \equiv -2$ (it would require $C_0 \to \infty$).
  * **Complete Solution:**
  $$y(t) = 2\left(\frac{1 + C_0 e^{4t}}{1 - C_0 e^{4t}}\right) \quad \text{and the singular solution} \quad y(t) \equiv -2$$
  
  ---
#### **Example 5: Completing the Square to Find $y(t)$ (Handwritten Notes & Exam Problem)**
  Solve the IVP:
  $$y' = \frac{3t^2 + 4t + 2}{2(y - 1)}, \quad y(0) = -1$$
  
  * **Step 1: Separate variables:**
  $$2(y - 1) \, dy = (3t^2 + 4t + 2) \, dt$$
  * **Step 2: Integrate both sides:**
  $$\int (2y - 2) \, dy = \int (3t^2 + 4t + 2) \, dt$$
  $$y^2 - 2y = t^3 + 2t^2 + 2t + C$$
  * **Step 3: Plug in $t = 0, y = -1$ immediately:**
  $$(-1)^2 - 2(-1) = 0 + C \implies 1 + 2 = C \implies C = 3$$
  $$y^2 - 2y = t^3 + 2t^2 + 2t + 3$$
  * **Step 4: Solve for $y$ using Complete the Square:**
  Add $1$ to both sides:
  $$y^2 - 2y + 1 = t^3 + 2t^2 + 2t + 3 + 1$$
  $$(y - 1)^2 = t^3 + 2t^2 + 2t + 4$$
  $$y - 1 = \pm\sqrt{t^3 + 2t^2 + 2t + 4} \implies y(t) = 1 \pm\sqrt{t^3 + 2t^2 + 2t + 4}$$
  * **Step 5: Select the correct sign:**
  * We need $y(0) = -1$:
    $$y(0) = 1 \pm \sqrt{4} = 1 \pm 2$$
  * To get $-1$, we must choose the **minus sign**:
    $$y(t) = 1 - \sqrt{t^3 + 2t^2 + 2t + 4}$$
  
  ---
### 6. Solutions to High-Yield Exam Problems (Slides 34–35)
#### **Example 7 (Spring 2012 Exam 1 #2):**
  Solve $y' - t y^2 = t, \quad y(0) = 1$
  1. Rearrange and factor: $y' = t + t y^2 = t(1 + y^2)$
  2. Separate: $\frac{1}{1 + y^2} \, dy = t \, dt$
  3. Integrate: $\arctan(y) = \frac{1}{2}t^2 + C$
  4. Initial Condition: $\arctan(1) = 0 + C \implies C = \frac{\pi}{4}$
  5. Isolate $y$: 
   $$y(t) = \tan\left(\frac{1}{2}t^2 + \frac{\pi}{4}\right)$$
  
  ---
#### **Example 11 (Fall 2013 Exam 1 #2):**
  Find the general solution of $t y' = y^2 + 1$
  1. Separate: $\frac{1}{y^2 + 1} \, dy = \frac{1}{t} \, dt$
  2. Integrate: $\arctan(y) = \ln|t| + C$
  3. Isolate $y$:
   $$y(t) = \tan(\ln|t| + C)$$
  
  ---
#### **Example 13 (Spring 2014 Exam 1 #2):**
  Find the general solution of $y' = t e^{t - y}$
  1. Split exponent: $\frac{dy}{dt} = t e^t e^{-y} \implies e^y \, dy = t e^t \, dt$
  2. Integrate right-hand side using **Integration by Parts** ($\int u v' dt = uv - \int v u' dt$):
   * $u = t, v' = e^t \implies u' = 1, v = e^t$
   * $\int t e^t \, dt = t e^t - \int e^t \, dt = t e^t - e^t + C$
  3. Equate: $e^y = (t - 1)e^t + C$
  4. Isolate $y$:
   $$y(t) = \ln\left((t - 1)e^t + C\right)$$
  
  ---
#### **Example 15 (Fall 2014 Final Exam #1):**
  Find the explicit solution of $\frac{1}{\sqrt{t}} + \sqrt{y} \, y' = t$
  1. Isolate the derivative term:
   $$\sqrt{y} \, y' = t - t^{-1/2} \implies y^{1/2} \, dy = (t - t^{-1/2}) \, dt$$
  2. Integrate both sides:
   $$\int y^{1/2} \, dy = \int (t - t^{-1/2}) \, dt$$
   $$\frac{2}{3} y^{3/2} = \frac{1}{2}t^2 - 2t^{1/2} + C$$
  3. Isolate $y$:
   $$y^{3/2} = \frac{3}{4}t^2 - 3\sqrt{t} + \frac{3}{2}C$$
   Let $C_1 = \frac{3}{2}C$:
   $$y(t) = \left(\frac{3}{4}t^2 - 3\sqrt{t} + C_1\right)^{2/3}$$
  
  ---
### Section 2.2 Pro-Tips & Exam Traps Checklist
  * [x] **Exponent Laws:** Remember that $e^{A+B} = e^A e^B$ and $e^{A-B} = \frac{e^A}{e^B}$. This is the number-one trick professors use to hide separable equations.
  * [x] **Find $C$ Early:** Calculate $C$ right after integrating. Do not wait until you have completed messy algebraic isolation.
  * [x] **Check Lost Solutions:** Before dividing by $h(y)$, find where $h(y) = 0$. Report those equilibrium lines if they are not included in your general family.
  * [x] **Always Pick the $\pm$ Sign:** Leaving $\pm\sqrt{\dots}$ on an Initial Value Problem will cost you points. Substitute $t_0$ and choose either $+$ or $-$ so that $y(t_0) = y_0$ holds true.
  
  ---