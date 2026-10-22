- Here is a set of **10 comprehensive practice problems** covering **Cauchy–Euler Equations** (distinct, repeated, and complex roots, negative domains, higher-order, and nonhomogeneous cases) and the **Method of Reduction of Order**, complete with step-by-step solutions.
  
  ---
### **Problem 1 (Cauchy–Euler: Distinct Real Roots, IVP)**
  **Problem:**  
  Solve the initial value problem for $t > 0$:
  $$t^2 y'' - 4t y' + 6y = 0; \quad y(1) = 4, \quad y'(1) = 11$$
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Indicial Equation:**  
   Substitute $y(t) = t^r$:
   $$r(r - 1) - 4r + 6 = 0 \implies r^2 - 5r + 6 = 0$$
   $$(r - 2)(r - 3) = 0 \implies r_1 = 2, \quad r_2 = 3$$
  
  2. **General Solution:**
   $$y(t) = c_1 t^2 + c_2 t^3$$
  
  3. **Derivative:**
   $$y'(t) = 2c_1 t + 3c_2 t^2$$
  
  4. **Apply Initial Conditions at $t = 1$:**
   * $y(1) = c_1 + c_2 = 4$
   * $y'(1) = 2c_1 + 3c_2 = 11$
  
   Multiply the first equation by $2$ and subtract from the second:
   $$(2c_1 + 3c_2) - (2c_1 + 2c_2) = 11 - 8 \implies c_2 = 3$$
   $$c_1 = 4 - 3 = 1$$
  
  **Answer:**
  $$y(t) = t^2 + 3t^3$$
  </details>
  
  ---
### **Problem 2 (Cauchy–Euler: Repeated Real Root)**
  **Problem:**  
  Find the general solution to the differential equation for $t > 0$:
  $$t^2 y'' + 5t y' + 4y = 0$$
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Indicial Equation:**  
   Substitute $y(t) = t^r$:
   $$r(r - 1) + 5r + 4 = 0 \implies r^2 + 4r + 4 = 0$$
   $$(r + 2)^2 = 0 \implies r = -2 \quad (\text{multiplicity } 2)$$
  
  2. **Two Linearly Independent Solutions:**  
   When a root $r$ repeats, the second solution gains a factor of $\ln(t)$:
   $$y_1(t) = t^{-2}, \quad y_2(t) = t^{-2}\ln(t)$$
  
  3. **General Solution:**
   $$y(t) = c_1 t^{-2} + c_2 t^{-2}\ln(t) \quad \left(\text{or } \frac{c_1 + c_2\ln(t)}{t^2}\right)$$
  
  **Answer:**
  $$y(t) = c_1 t^{-2} + c_2 t^{-2}\ln(t)$$
  </details>
  
  ---
### **Problem 3 (Cauchy–Euler: Complex Conjugate Roots)**
  **Problem:**  
  Find the general solution to the differential equation for $t > 0$:
  $$t^2 y'' - t y' + 5y = 0$$
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Indicial Equation:**  
   Substitute $y(t) = t^r$:
   $$r(r - 1) - r + 5 = 0 \implies r^2 - 2r + 5 = 0$$
  
  2. **Solve for the Roots:**
   $$r = \frac{2 \pm \sqrt{(-2)^2 - 4(1)(5)}}{2} = \frac{2 \pm \sqrt{-16}}{2} = 1 \pm 2i$$
   The roots are $\alpha \pm \beta i$ with $\alpha = 1$ and $\beta = 2$.
  
  3. **Linearly Independent Solutions:**
   $$y_1(t) = t^1 \cos(2\ln(t)), \quad y_2(t) = t^1 \sin(2\ln(t))$$
  
  **Answer:**
  $$y(t) = t\left[c_1 \cos(2\ln(t)) + c_2 \sin(2\ln(t))\right]$$
  </details>
  
  ---
### **Problem 4 (Cauchy–Euler on a Negative Domain $t < 0$)**
  **Problem:**  
  Find the general solution to the differential equation for $t < 0$:
  $$t^2 y'' + 3t y' + 5y = 0$$
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Substitution for $t < 0$:**  
   Let $u = -t > 0$. Using the chain rule:
   $$\frac{dy}{dt} = -\frac{dy}{du}, \quad \frac{d^2y}{dt^2} = \frac{d^2y}{du^2}$$
   Substituting into the equation:
   $$(-u)^2 \frac{d^2y}{du^2} + 3(-u)\left(-\frac{dy}{du}\right) + 5y = 0 \implies u^2 \frac{d^2y}{du^2} + 3u\frac{dy}{du} + 5y = 0$$
  
  2. **Indicial Equation:**
   $$r(r - 1) + 3r + 5 = 0 \implies r^2 + 2r + 5 = 0$$
   $$r = \frac{-2 \pm \sqrt{4 - 20}}{2} = -1 \pm 2i$$
  
  3. **Solution in Terms of $u$:**
   $$y(u) = u^{-1}\left[c_1 \cos(2\ln(u)) + c_2 \sin(2\ln(u))\right]$$
  
  4. **Substitute Back $u = -t$:**
   $$y(t) = (-t)^{-1}\left[c_1 \cos(2\ln(-t)) + c_2 \sin(2\ln(-t))\right]$$
  
  **Answer:**
  $$y(t) = (-t)^{-1}\left[c_1 \cos(2\ln(-t)) + c_2 \sin(2\ln(-t))\right] \quad \left(\text{or } \frac{c_1 \cos(2\ln(-t)) + c_2 \sin(2\ln(-t))}{-t}\right)$$
  </details>
  
  ---
### **Problem 5 (Third-Order Cauchy–Euler Equation)**
  **Problem:**  
  Find the general solution to the third-order equation for $x > 0$:
  $$x^3 y''' + 3x^2 y'' - 2x y' + 2y = 0$$
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Indicial Equation:**  
   Recall:
   $$x y' = r x^r, \quad x^2 y'' = r(r - 1)x^r, \quad x^3 y''' = r(r - 1)(r - 2)x^r$$
   Substituting into the differential equation:
   $$r(r - 1)(r - 2) + 3r(r - 1) - 2r + 2 = 0$$
   $$(r^3 - 3r^2 + 2r) + (3r^2 - 3r) - 2r + 2 = 0$$
   $$r^3 - 3r + 2 = 0$$
  
  2. **Factor the Polynomial:**  
   Notice that $r = 1$ is a root ($1 - 3 + 2 = 0$):
   $$r^3 - 3r + 2 = (r - 1)(r^2 + r - 2) = (r - 1)^2 (r + 2) = 0$$
   The roots are:
   * $r = 1$ with multiplicity $2$
   * $r = -2$ with multiplicity $1$
  
  3. **General Solution:**
   $$y(x) = c_1 x + c_2 x \ln(x) + c_3 x^{-2}$$
  
  **Answer:**
  $$y(x) = c_1 x + c_2 x \ln(x) + c_3 x^{-2}$$
  </details>
  
  ---
### **Problem 6 (Nonhomogeneous Cauchy–Euler via Variation of Parameters)**
  **Problem:**  
  Find the general solution to the nonhomogeneous equation for $t > 0$:
  $$t^2 y'' - 2t y' + 2y = 4t^3$$
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Solve the Homogeneous Equation:**
   $$r(r - 1) - 2r + 2 = 0 \implies r^2 - 3r + 2 = (r - 1)(r - 2) = 0$$
   $$y_1(t) = t, \quad y_2(t) = t^2$$
   $$y_h(t) = c_1 t + c_2 t^2$$
  
  2. **Put into Standard Form:**  
   Divide by $t^2$:
   $$y'' - \frac{2}{t}y' + \frac{2}{t^2}y = 4t \implies g(t) = 4t$$
  
  3. **Compute the Wronskian $W(y_1, y_2)$:**
   $$W(t) = \begin{vmatrix} t & t^2 \\ 1 & 2t \end{vmatrix} = 2t^2 - t^2 = t^2$$
  
  4. **Variation of Parameters:**
   $$u_1'(t) = -\frac{y_2(t) g(t)}{W(t)} = -\frac{t^2 (4t)}{t^2} = -4t \implies u_1(t) = \int -4t \, dt = -2t^2$$
   $$u_2'(t) = \frac{y_1(t) g(t)}{W(t)} = \frac{t(4t)}{t^2} = 4 \implies u_2(t) = \int 4 \, dt = 4t$$
  
  5. **Particular Solution:**
   $$y_p(t) = u_1(t) y_1(t) + u_2(t) y_2(t) = (-2t^2)(t) + (4t)(t^2) = -2t^3 + 4t^3 = 2t^3$$
  
  **Answer:**
  $$y(t) = c_1 t + c_2 t^2 + 2t^3$$
  </details>
  
  ---
### **Problem 7 (Reduction of Order: Rational Basis)**
  **Problem:**  
  Given that $y_1(x) = x$ is a nontrivial solution to
  $$x^2 y'' + 2x y' - 2y = 0, \quad x > 0$$
  use the method of reduction of order to find a second linearly independent solution $y_2(x)$, and state the general solution.
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Standard Form:**
   $$y'' + \frac{2}{x}y' - \frac{2}{x^2}y = 0 \implies P(x) = \frac{2}{x}$$
  
  2. **Reduction of Order Formula:**
   $$y_2(x) = y_1(x) \int \frac{e^{-\int P(x) \, dx}}{\left[y_1(x)\right]^2} \, dx$$
  
  3. **Evaluate the Exponent and Integrand:**
   $$-\int P(x) \, dx = -\int \frac{2}{x} \, dx = -2\ln(x) = \ln(x^{-2})$$
   $$e^{-\int P(x) \, dx} = x^{-2}$$
   $$\int \frac{x^{-2}}{(x)^2} \, dx = \int x^{-4} \, dx = -\frac{1}{3}x^{-3}$$
  
  4. **Find $y_2(x)$:**
   $$y_2(x) = x \left(-\frac{1}{3}x^{-3}\right) = -\frac{1}{3}x^{-2}$$
   Taking the canonical basis solution (ignoring the scalar multiple $-\frac{1}{3}$):
   $$y_2(x) = x^{-2} \quad \left(\text{or } \frac{1}{x^2}\right)$$
  
  **Answer:**
  $$y_2(x) = x^{-2}, \quad y(x) = c_1 x + c_2 x^{-2}$$
  </details>
  
  ---
### **Problem 8 (Reduction of Order: Exponential Solution)**
  **Problem:**  
  Given that $y_1(t) = e^{2t}$ is a solution to the variable-coefficient differential equation
  $$t y'' - (2t + 1)y' + 2y = 0, \quad t > 0$$
  find a second linearly independent solution $y_2(t)$ using reduction of order. *(Assume constants of integration are zero).*
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Standard Form:**
   $$y'' - \left(\frac{2t + 1}{t}\right)y' + \frac{2}{t}y = 0 \implies P(t) = -2 - \frac{1}{t}$$
  
  2. **Evaluate the Integrating Term:**
   $$-\int P(t) \, dt = \int \left(2 + \frac{1}{t}\right) dt = 2t + \ln(t)$$
   $$e^{-\int P(t) \, dt} = e^{2t + \ln(t)} = t e^{2t}$$
  
  3. **Set up the Integral for $v(t)$:**
   $$v(t) = \int \frac{e^{-\int P(t) \, dt}}{\left(y_1(t)\right)^2} \, dt = \int \frac{t e^{2t}}{\left(e^{2t}\right)^2} \, dt = \int t e^{-2t} \, dt$$
  
  4. **Integrate by Parts ($\int u \, dv$):**
   * $u = t \implies du = dt$
   * $dv = e^{-2t}dt \implies v = -\frac{1}{2}e^{-2t}$
  
   $$v(t) = -\frac{1}{2}t e^{-2t} - \int \left(-\frac{1}{2}e^{-2t}\right) dt = -\frac{1}{2}t e^{-2t} - \frac{1}{4}e^{-2t} = -\frac{2t + 1}{4}e^{-2t}$$
  
  5. **Form $y_2(t) = y_1(t) v(t)$:**
   $$y_2(t) = e^{2t} \left(-\frac{2t + 1}{4}e^{-2t}\right) = -\frac{2t + 1}{4}$$
   *(or simplified as a linear function: $-\frac{1}{2}t - \frac{1}{4}$)*
  
  **Answer:**
  $$y_2(t) = -\frac{2t + 1}{4} \quad \left(\text{or linearly independent basis: } 2t + 1\right)$$
  </details>
  
  ---
### **Problem 9 (Reduction of Order: Integration with Factored Derivatives)**
  **Problem:**  
  Given that $y_1(x) = x + 1$ is a solution to
  $$x y'' - (x + 1)y' + y = 0, \quad x > 0$$
  find a second linearly independent solution $y_2(x)$ using reduction of order.
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Standard Form:**
   $$y'' - \left(\frac{x + 1}{x}\right)y' + \frac{1}{x}y = 0 \implies P(x) = -1 - \frac{1}{x}$$
  
  2. **Evaluate the Numerator:**
   $$-\int P(x) \, dx = \int \left(1 + \frac{1}{x}\right) dx = x + \ln(x)$$
   $$e^{-\int P(x) \, dx} = x e^x$$
  
  3. **Set up the Integral:**
   $$v(x) = \int \frac{x e^x}{(x + 1)^2} \, dx$$
  
  4. **Integrate by Parts:**
   * Let $u = x e^x \implies du = (x + 1)e^x \, dx$
   * Let $dv = \frac{1}{(x + 1)^2} \, dx \implies v = -\frac{1}{x + 1}$
  
   $$v(x) = uv - \int v \, du = -\frac{x e^x}{x + 1} - \int \left(-\frac{1}{x + 1}\right)(x + 1)e^x \, dx$$
   $$v(x) = -\frac{x e^x}{x + 1} + \int e^x \, dx = -\frac{x e^x}{x + 1} + e^x$$
   $$v(x) = \frac{-x e^x + (x + 1)e^x}{x + 1} = \frac{e^x}{x + 1}$$
  
  5. **Form $y_2(x) = y_1(x) v(x)$:**
   $$y_2(x) = (x + 1) \cdot \frac{e^x}{x + 1} = e^x$$
  
  **Answer:**
  $$y_2(x) = e^x$$
  </details>
  
  ---
### **Problem 10 (Nonhomogeneous Cauchy–Euler with Logarithmic Forcing)**
  **Problem:**  
  Find the general solution to the following differential equation for $x > 0$:
  $$x^2 y'' - 3x y' + 3y = 2x^4 \ln(x)$$
  
  <details>
  <summary><b>Click for Step-by-Step Solution</b></summary>
  
  1. **Solve the Homogeneous Equation:**
   $$r(r - 1) - 3r + 3 = 0 \implies r^2 - 4r + 3 = (r - 1)(r - 3) = 0$$
   $$y_1(x) = x, \quad y_2(x) = x^3$$
   $$y_h(x) = c_1 x + c_2 x^3$$
  
  2. **Standard Form:**
   Divide by $x^2$:
   $$y'' - \frac{3}{x}y' + \frac{3}{x^2}y = 2x^2 \ln(x) \implies g(x) = 2x^2 \ln(x)$$
  
  3. **Compute the Wronskian:**
   $$W(x) = \begin{vmatrix} x & x^3 \\ 1 & 3x^2 \end{vmatrix} = 3x^3 - x^3 = 2x^3$$
  
  4. **Variation of Parameters:**
   * **For $u_1(x)$:**
     $$u_1'(x) = -\frac{y_2(x) g(x)}{W(x)} = -\frac{x^3\left(2x^2 \ln(x)\right)}{2x^3} = -x^2 \ln(x)$$
     Integrate by parts:
     $$u_1(x) = -\left(\frac{x^3}{3}\ln(x) - \frac{x^3}{9}\right) = -\frac{x^3}{3}\ln(x) + \frac{x^3}{9}$$
  
   * **For $u_2(x)$:**
     $$u_2'(x) = \frac{y_1(x) g(x)}{W(x)} = \frac{x\left(2x^2 \ln(x)\right)}{2x^3} = \ln(x)$$
     Integrate:
     $$u_2(x) = x\ln(x) - x$$
  
  5. **Construct $y_p(x) = u_1(x)y_1(x) + u_2(x)y_2(x)$:**
   $$y_p(x) = \left(-\frac{x^3}{3}\ln(x) + \frac{x^3}{9}\right)x + \left(x\ln(x) - x\right)x^3$$
   $$y_p(x) = -\frac{x^4}{3}\ln(x) + \frac{x^4}{9} + x^4 \ln(x) - x^4$$
   $$y_p(x) = \left(1 - \frac{1}{3}\right)x^4 \ln(x) + \left(\frac{1}{9} - 1\right)x^4 = \frac{2}{3}x^4 \ln(x) - \frac{8}{9}x^4$$
  
  **Answer:**
  $$y(x) = c_1 x + c_2 x^3 + \frac{2}{3}x^4 \ln(x) - \frac{8}{9}x^4$$
  </details>