### 1. The Standard Form (The Mandatory "Step 0")
  
  A first-order linear differential equation involves the unknown function $y$ and its derivative $y'$ strictly to the first power, with no products of $y$ and $y'$:
  $$a_1(t)\frac{dy}{dt} + a_0(t)y = b(t)$$
  
  Before applying any formula or technique, you **must** divide the entire equation by the leading coefficient $a_1(t)$ so that the coefficient of $y'$ is **identically 1**:
  
  $$\frac{dy}{dt} + p(t)y = g(t)$$
  
  where:
  $$p(t) = \frac{a_0(t)}{a_1(t)} \quad \text{and} \quad g(t) = \frac{b(t)}{a_1(t)}$$
  
  > **Exam Trap #1:** Identifying $p(t)$ before putting the equation in standard form is the single most common student error. If you start with $t y' + 2y = 4t^2$, $p(t)$ is **not** $2$; it is $\frac{2}{t}$.
  
  ---
### 2. Theoretical Derivation: The Reverse Product Rule
  
  Why does this method work? We want to transform the left-hand side of:
  $$y' + p(t)y = g(t)$$
  into the derivative of a single product $[u \cdot v]'$.
  
  1. Multiply the entire equation by an unknown, strictly positive function $\mu(t)$ (called the **Integrating Factor**):
   $$\mu(t)y' + \mu(t)p(t)y = \mu(t)g(t)$$
  2. Recall the **Product Rule** for differentiation:
   $$\frac{d}{dt}[\mu(t)y] = \mu(t)y' + \mu'(t)y$$
  3. Compare the two equations term by term:
   $$\underbrace{\mu(t)y'}_{\text{matches}} + \underbrace{\mu'(t)y}_{\text{must match}} = \mu(t)y' + \underbrace{\mu(t)p(t)y}_{\text{must match}}$$
   For this equality to hold, we require:
   $$\mu'(t) = p(t)\mu(t)$$
  4. This is a separable differential equation for $\mu(t)$:
   $$\frac{1}{\mu} \frac{d\mu}{dt} = p(t) \implies \int \frac{1}{\mu}\,d\mu = \int p(t)\,dt$$
   $$\ln|\mu(t)| = \int p(t)\,dt \implies \mu(t) = e^{\int p(t)\,dt}$$
#### **Why do we omit the constant of integration $+C$ here?**
  If we included a constant $+C$, we would have $\mu(t) = e^{\int p(t)dt + C} = e^C e^{\int p(t)dt} = k \cdot e^{\int p(t)dt}$. When multiplying both sides of the ODE by $\mu(t)$, the nonzero constant $k$ would appear on both sides and cancel out immediately. Thus, we choose the simplest integrating factor by setting $C = 0$.
  
  ---
### 3. Logarithmic Simplification Rules *(Crucial Algebra Toolbox)*
  
  The integrating factor almost always involves an integral that evaluates to $\ln|t|$. You must simplify $\mu(t)$ algebraically before multiplying it through:
  
  | Form | Correct Simplification | Common Mistake |
  |---|---|---|
  | $e^{\ln(f(t))}$ | $= f(t)$ | — |
  | $e^{k \ln(t)}$ | $= e^{\ln(t^k)} = t^k$ | $\neq k \cdot t$ |
  | $e^{-\ln(t)}$ | $= e^{\ln(t^{-1})} = t^{-1} = \frac{1}{t}$ | $\neq -t$ |
  | $e^{3\ln(t) + t}$ | $= e^{\ln(t^3)} \cdot e^t = t^3 e^t$ | $\neq t^3 + e^t$ |
  
  ---
### 4. General Solution Formula & Step-by-Step Procedure
  
  Once $\mu(t) = e^{\int p(t)dt}$ is computed:
  1. Multiply the standard form equation by $\mu(t)$:
   $$\frac{d}{dt}[\mu(t)y] = \mu(t)g(t)$$
  2. Integrate both sides with respect to $t$:
   $$\mu(t)y = \int \mu(t)g(t)\,dt + C \quad \leftarrow \text{\textbf{Add the constant $+C$ HERE!}}$$
  3. Divide through by $\mu(t)$ to obtain the explicit solution:
   $$y(t) = \frac{1}{\mu(t)}\left[\int \mu(t)g(t)\,dt + C\right]$$
  
  > **Exam Trap #2:** The $+C$ **must** be inside the brackets! It gets divided by $\mu(t)$, producing a term of the form $\frac{C}{\mu(t)}$. If you write $y(t) = \frac{1}{\mu(t)}\int \mu g\,dt + C$, your answer is completely incorrect.
  
  ---
### 5. Detailed Step-by-Step Examples
  
  ---
#### **Example 1: The Introductory Linear Equation (Handwritten Notes)**
  $$y' - \frac{1}{2}y = e^{-t}$$
  
  * **Step 0:** Standard form is already satisfied ($p(t) = -\frac{1}{2}$, $g(t) = e^{-t}$).
  * **Step 1: Compute $\mu(t)$:**
  $$\mu(t) = e^{\int -\frac{1}{2}\,dt} = e^{-\frac{1}{2}t}$$
  * **Step 2: Multiply equation by $\mu(t)$:**
  $$e^{-\frac{1}{2}t}y' - \frac{1}{2}e^{-\frac{1}{2}t}y = e^{-\frac{1}{2}t}e^{-t} = e^{-\frac{3}{2}t}$$
  * **Step 3: Collapse into product rule derivative:**
  $$\frac{d}{dt}\left[e^{-\frac{1}{2}t}y\right] = e^{-\frac{3}{2}t}$$
  * **Step 4: Integrate both sides:**
  $$e^{-\frac{1}{2}t}y = \int e^{-\frac{3}{2}t}\,dt = -\frac{2}{3}e^{-\frac{3}{2}t} + C$$
  * **Step 5: Isolate $y(t)$ (multiply by $e^{\frac{1}{2}t}$):**
  $$y(t) = -\frac{2}{3}e^{-\frac{3}{2}t}e^{\frac{1}{2}t} + Ce^{\frac{1}{2}t} = Ce^{\frac{1}{2}t} - \frac{2}{3}e^{-t}$$
  
  ---
#### **Example 2: Exam IVP with Exponent Power Rules (Slides 80–98)**
  Solve the IVP:
  $$4t^2 - ty' = 2y, \quad y(1) = 2$$
  
  * **Step 0: Put in standard form:**
  $$ty' + 2y = 4t^2 \implies y' + \frac{2}{t}y = 4t$$
  Here, $p(t) = \frac{2}{t}$ and $g(t) = 4t$.
  * **Step 1: Compute $\mu(t)$:**
  $$\mu(t) = e^{\int \frac{2}{t}\,dt} = e^{2\ln|t|} = e^{\ln(t^2)} = t^2$$
  * **Step 2: Multiply standard form by $t^2$:**
  $$t^2 y' + 2ty = 4t^3$$
  * **Step 3: Collapse left side:**
  $$\frac{d}{dt}\left[t^2 y\right] = 4t^3$$
  * **Step 4: Integrate:**
  $$t^2 y = \int 4t^3\,dt = t^4 + C$$
  * **Step 5: Solve for $y(t)$:**
  $$y(t) = t^2 + \frac{C}{t^2}$$
  * **Step 6: Apply initial condition $y(1) = 2$:**
  $$2 = (1)^2 + \frac{C}{(1)^2} \implies 2 = 1 + C \implies C = 1$$
  * **Final Solution:**
  $$y(t) = t^2 + \frac{1}{t^2}$$
  
  ---
#### **Example 3: Spring 2010 Exam 1 #3 (Slides 99–114)**
  Find the general solution of $y' = \frac{1 + xy}{x^2}$ for $x > 0$.
  
  * **Step 0: Rewrite into standard form:**
  $$x^2 y' = 1 + xy \implies x^2 y' - xy = 1 \implies y' - \frac{1}{x}y = \frac{1}{x^2}$$
  Here, $p(x) = -\frac{1}{x}$ and $g(x) = \frac{1}{x^2}$.
  * **Step 1: Compute $\mu(x)$:**
  $$\mu(x) = e^{\int -\frac{1}{x}\,dx} = e^{-\ln(x)} = e^{\ln(x^{-1})} = x^{-1} = \frac{1}{x} \quad (\text{since } x > 0)$$
  * **Step 2 & 3: Multiply and collapse:**
  $$\frac{1}{x}y' - \frac{1}{x^2}y = \frac{1}{x^3} \implies \frac{d}{dx}\left[\frac{1}{x}y\right] = x^{-3}$$
  * **Step 4: Integrate:**
  $$\frac{1}{x}y = \int x^{-3}\,dx = -\frac{1}{2}x^{-2} + C = -\frac{1}{2x^2} + C$$
  * **Step 5: Solve for $y(x)$ (multiply by $x$):**
  $$y(x) = -\frac{1}{2x} + Cx$$
  
  ---
#### **Example 4: Double Exponential Integrating Factor (Handwritten Notes)**
  Solve the IVP:
  $$e^t y' = 2 - y, \quad y(0) = -1$$
  
  * **Step 0: Standard form:**
  $$y' = 2e^{-t} - ye^{-t} \implies y' + e^{-t}y = 2e^{-t}$$
  $p(t) = e^{-t}$ and $g(t) = 2e^{-t}$.
  * **Step 1: Compute $\mu(t)$:**
  $$\mu(t) = e^{\int e^{-t}\,dt} = e^{-e^{-t}}$$
  * **Step 2 & 3: Multiply and collapse:**
  $$\frac{d}{dt}\left[e^{-e^{-t}}y\right] = 2e^{-t}e^{-e^{-t}}$$
  * **Step 4: Integrate using substitution:**
  $$\int 2e^{-t}e^{-e^{-t}}\,dt$$
  Let $u = -e^{-t} \implies du = e^{-t}\,dt$:
  $$\int 2e^u\,du = 2e^u + C = 2e^{-e^{-t}} + C$$
  So:
  $$e^{-e^{-t}}y = 2e^{-e^{-t}} + C$$
  * **Step 5: Solve for $y(t)$ (multiply by $e^{e^{-t}}$):**
  $$y(t) = 2 + C e^{e^{-t}}$$
  * **Step 6: Apply $y(0) = -1$:**
  $$-1 = 2 + C e^{e^0} = 2 + Ce^1 \implies Ce = -3 \implies C = -\frac{3}{e}$$
  * **Final Solution:**
  $$y(t) = 2 - \frac{3}{e} e^{e^{-t}} = 2 - 3e^{e^{-t} - 1}$$
  
  ---
### 6. Theory: Existence, Uniqueness, and Interval of Definition
  
  ```
        Singularity (p or g blows up)      Singularity
  -------------(------------------*--------------)------------> t
                     Interval of Validity I        t0
  ```
#### **Theorem: Existence and Uniqueness for Linear Equations (Slides 131–133)**
  If the coefficient functions $p(t)$ and $g(t)$ are **continuous** on an open interval $I = (a, b)$ that contains the initial point $t_0$, then there exists a **unique** solution $y = \phi(t)$ to the initial value problem:
  $$y' + p(t)y = g(t), \quad y(t_0) = y_0$$
  defined on the **entire interval $I$**.
  
  ---
#### **Crucial Contrast: Linear vs. Nonlinear Intervals of Validity**
  
  | Feature | First-Order Linear Equations | First-Order Nonlinear Equations |
  |---|---|---|
  | **Interval of validity $I$** | Determined **completely** by the domain of continuity of $p(t)$ and $g(t)$. | Cannot be known in advance; depends heavily on the initial value $y_0$. |
  | **Need to solve first?** | **No!** You can state $I$ before solving. | **Yes!** Must solve to see where the solution blows up. |
  | **Spontaneous blow-up?** | **Never.** Solutions only blow up where $p(t)$ or $g(t)$ has a singularity. | **Yes!** Can blow up in finite time even if $f(t,y)$ is smooth everywhere. |
#### **The Canonical Comparison Example (Slides 154–167):**
  * **Nonlinear ODE:** $y' = y^2, \quad y(0) = 1$
  * The function $f(t,y) = y^2$ is continuous everywhere on the entire plane $\mathbb{R}^2$. There are no points where $f$ is undefined.
  * Yet, solving by separation of variables:
    $$\int y^{-2}dy = \int dt \implies -\frac{1}{y} = t + c \implies y(0)=1 \implies c = -1$$
    $$y(t) = \frac{1}{1 - t}$$
  * The solution blows up to $\infty$ as $t \to 1^-$.
  * Since $t_0 = 0 < 1$, the interval of validity is strictly **$I = (-\infty, 1)$**, which could **not** have been predicted just by looking at $y^2$!
  
  ---
#### **Example: Finding the Largest Interval of Validity Without Solving (Slide 134)**
  Find the largest interval in which the IVP is guaranteed a unique solution:
  $$ty' + 2y = 4t^2, \quad y(1) = 2$$
  
  1. Put into standard form:
   $$y' + \frac{2}{t}y = 4t$$
  2. Identify $p(t) = \frac{2}{t}$ and $g(t) = 4t$.
  3. Check continuity:
   * $g(t) = 4t$ is continuous on $(-\infty, \infty)$.
   * $p(t) = \frac{2}{t}$ is discontinuous at $t = 0$.
   * The points of discontinuity split the real line into two intervals: $(-\infty, 0)$ and $(0, \infty)$.
  4. Locate the initial time $t_0$:
   * $y(1) = 2 \implies t_0 = 1$.
   * Since $1 \in (0, \infty)$, the theorem guarantees a unique solution on:
     $$I = (0, \infty)$$
   *(Note: If the initial condition had been $y(-3) = 5$, the interval would instead be $I = (-\infty, 0)$).*
  
  ---
### 7. Advanced Exam Problems from the Archive (Slides 128–129)
#### **Example A: Order Reduction via Substitution (Fall 2012 Final #10)**
  Solve the IVP:
  $$ty'' + y' = 1 + \frac{1}{(t-2)^2}, \quad y(1) = 0, \quad y'(1) = 3$$
  
  * **Step 1: Reduce order with $u = y'$:**
  Then $u' = y''$. Substituting gives:
  $$tu' + u = 1 + \frac{1}{(t-2)^2}$$
  * **Step 2: Recognize the reverse product rule immediately:**
  Notice that the left side is already $[t \cdot u]' = t u' + (1)u$!
  $$\frac{d}{dt}[tu] = 1 + (t-2)^{-2}$$
  * **Step 3: Integrate with respect to $t$:**
  $$tu = \int \left[1 + (t-2)^{-2}\right]dt = t - (t-2)^{-1} + C_1 = t - \frac{1}{t-2} + C_1$$
  * **Step 4: Use initial condition $y'(1) = u(1) = 3$:**
  $$(1)(3) = 1 - \frac{1}{1-2} + C_1 \implies 3 = 1 - (-1) + C_1 \implies 3 = 2 + C_1 \implies C_1 = 1$$
  $$tu = t - \frac{1}{t-2} + 1 \implies u(t) = y'(t) = 1 - \frac{1}{t(t-2)} + \frac{1}{t}$$
  * **Step 5: Integrate once more to find $y(t)$:**
  Using partial fractions: $\frac{1}{t(t-2)} = -\frac{1/2}{t} + \frac{1/2}{t-2}$
  $$y'(t) = 1 - \left(-\frac{1}{2t} + \frac{1}{2(t-2)}\right) + \frac{1}{t} = 1 + \frac{3}{2t} - \frac{1}{2(t-2)}$$
  $$y(t) = t + \frac{3}{2}\ln|t| - \frac{1}{2}\ln|t-2| + C_2$$
  * **Step 6: Apply $y(1) = 0$:**
  $$0 = 1 + \frac{3}{2}\ln(1) - \frac{1}{2}\ln|1-2| + C_2 \implies 0 = 1 + 0 - 0 + C_2 \implies C_2 = -1$$
  $$y(t) = t + \frac{3}{2}\ln|t| - \frac{1}{2}\ln|t-2| - 1$$
  
  ---
#### **Example B: Long-Term Behavior / Parameter Tuning (Fall 2015 Exam 1 #1)**
  Find the value of $y_0$ for which the solution of $ty' - y = t^2 e^{-t}, \quad y(1) = y_0$ approaches $0$ as $t \to \infty$.
  
  * **Step 1: Standard form & Integrating Factor:**
  $$y' - \frac{1}{t}y = t e^{-t} \implies \mu(t) = e^{\int -\frac{1}{t}dt} = \frac{1}{t}$$
  * **Step 2: Multiply & Integrate:**
  $$\frac{d}{dt}\left[\frac{1}{t}y\right] = \frac{1}{t}(t e^{-t}) = e^{-t}$$
  $$\frac{1}{t}y = -e^{-t} + C \implies y(t) = -t e^{-t} + Ct$$
  * **Step 3: Analyze $\lim_{t \to \infty} y(t)$:**
  $$\lim_{t \to \infty} y(t) = \lim_{t \to \infty} \left(-\frac{t}{e^t} + Ct\right)$$
  * By L'Hôpital's Rule, $\lim_{t \to \infty} \frac{t}{e^t} = 0$.
  * However, the second term is $Ct$:
    * If $C > 0 \implies Ct \to +\infty$.
    * If $C < 0 \implies Ct \to -\infty$.
    * **If and only if $C = 0$**, then $y(t) \to 0$ as $t \to \infty$.
  * **Step 4: Solve for $y_0$:**
  $$y(t) = -t e^{-t} \implies y_0 = y(1) = -(1)e^{-1} = -e^{-1} = -\frac{1}{e}$$
  
  ---
### Section 2.3 Pro-Tips & Exam Traps Checklist
  * [x] **Coefficient of $y'$ must be 1:** Always divide by $a_1(t)$ before labeling $p(t)$.
  * [x] **Sign of $p(t)$:** If the equation is $y' - 3y = g(t)$, $p(t) = -3$, not $+3$. A missing negative sign completely flips the integrating factor.
  * [x] **Simplify exponentials correctly:** $e^{-\ln(x)} = \frac{1}{x}$, **not** $-x$.
  * [x] **Distribute $\frac{1}{\mu(t)}$ to $C$:** The general solution is $y(t) = \frac{\int \mu g}{\mu} + \frac{C}{\mu(t)}$.
  * [x] **Interval of validity for linear equations:** Found immediately by locating the discontinuities of $p(t)$ and $g(t)$ without solving!
  
  ---