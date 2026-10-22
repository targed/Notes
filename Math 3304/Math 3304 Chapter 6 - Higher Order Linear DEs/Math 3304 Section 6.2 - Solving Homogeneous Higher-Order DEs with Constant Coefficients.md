### 1. Conceptual Foundation & The Characteristic Equation
  
  We now consider the general $n$-th order linear homogeneous differential equation with **constant real coefficients**:
  $$a_n y^{(n)} + a_{n-1}y^{(n-1)} + \dots + a_1 y' + a_0 y = 0 \quad (a_n \neq 0)$$
  
  Just as with second-order equations, we assume an exponential trial solution:
  $$y(t) = e^{rt}$$
  
  Computing successive derivatives:
  $$y' = r e^{rt}, \quad y'' = r^2 e^{rt}, \quad \dots, \quad y^{(n)} = r^n e^{rt}$$
  
  Substituting these into the differential equation:
  $$\left(a_n r^n + a_{n-1}r^{n-1} + \dots + a_1 r + a_0\right)e^{rt} = 0$$
  
  Since $e^{rt} \neq 0$ for all real $t$, we obtain the **Characteristic Equation (Auxiliary Equation)**:
  
  $$\mathbf{a_n r^n + a_{n-1}r^{n-1} + \dots + a_1 r + a_0 = 0}$$
#### **The Fundamental Theorem of Algebra:**
  An $n$-th degree polynomial with real coefficients has **exactly $n$ roots** (counting multiplicities and complex conjugate pairs). 
  
  Therefore, finding the general solution to an $n$-th order ODE reduces entirely to **finding the $n$ roots of a degree-$n$ polynomial**.
  
  ---
### 2. The Comprehensive Rules for Constructing Solutions
  
  Every root $r$ of the characteristic equation contributes basis solutions to the Fundamental Set of Solutions according to its algebraic multiplicity $k$:
  
  ---
#### **Rule 1: Real Roots with Multiplicity $k$**
  * If $r$ is a **single (unrepeated, $k=1$) real root**, it contributes:
  $$\mathbf{e^{rt}}$$
  * If $r$ is a **repeated real root with multiplicity $k$**, it contributes $k$ linearly independent solutions:
  $$\mathbf{e^{rt}, \quad t e^{rt}, \quad t^2 e^{rt}, \quad \dots, \quad t^{k-1} e^{rt}}$$
  
  > **Special Case: Root at Zero ($r = 0$ with multiplicity $k$):**
  > Since $e^{0t} = 1$, the root $r = 0$ repeated $k$ times produces powers of $t$:
  > $$\mathbf{1, \quad t, \quad t^2, \quad \dots, \quad t^{k-1}}$$
  
  ---
#### **Rule 2: Complex Conjugate Roots with Multiplicity $k$**
  Because the coefficients $a_i$ are real numbers, any complex roots **must occur in conjugate pairs**:
  $$r = \alpha \pm i\beta \quad (\beta > 0)$$
  
  * If $\alpha \pm i\beta$ is an **unrepeated pair ($k=1$)**, it contributes 2 solutions:
  $$\mathbf{e^{\alpha t}\cos(\beta t), \qquad e^{\alpha t}\sin(\beta t)}$$
  * If $\alpha \pm i\beta$ is a **repeated pair with multiplicity $k$**, it contributes $2k$ solutions by multiplying each sinusoid by successive powers of $t$:
  $$\mathbf{\begin{aligned}
  &e^{\alpha t}\cos(\beta t), && e^{\alpha t}\sin(\beta t), \\
  &t e^{\alpha t}\cos(\beta t), && t e^{\alpha t}\sin(\beta t), \\
  &t^2 e^{\alpha t}\cos(\beta t), && t^2 e^{\alpha t}\sin(\beta t), \\
  &\qquad \vdots && \qquad \vdots \\
  &t^{k-1} e^{\alpha t}\cos(\beta t), && t^{k-1} e^{\alpha t}\sin(\beta t)
  \end{aligned}}$$
  
  ---
#### **The General Solution:**
  Once all $n$ linearly independent solutions $\{y_1, y_2, \dots, y_n\}$ are determined:
  $$\mathbf{y(t) = c_1 y_1(t) + c_2 y_2(t) + \dots + c_n y_n(t) = \sum_{i=1}^n c_i y_i(t)}$$
  
  ---
### 3. Essential Algebra: High-Yield Factoring Techniques for Exams
  
  The calculus in Section 6.2 is straightforward; **the challenge is factoring degree 3, 4, and 5 polynomials by hand**. Review these five algebraic patterns before an exam:
  
  ```
  +-------------------------------------------------------------------------------------------------+
  |                                 POLYNOMIAL FACTORING TOOLBOX                                    |
  |                                                                                                 |
  |  1. Factoring out Powers of r:                                                                  |
  |     r^5 - 2r^4 + 5r^3 = r^3(r^2 - 2r + 5)                                                       |
  |                                                                                                 |
  |  2. Difference of Squares:                                                                      |
  |     r^4 - 16 = (r^2 - 4)(r^2 + 4) = (r - 2)(r + 2)(r^2 + 4)                                     |
  |                                                                                                 |
  |  3. Quadratic in Disguise (let u = r^2):                                                        |
  |     r^4 + 5r^2 + 4 = (r^2 + 1)(r^2 + 4)                                                         |
  |     r^4 + 4r^2 + 4 = (r^2 + 2)^2                                                                |
  |                                                                                                 |
  |  4. Sum & Difference of Cubes:                                                                  |
  |     r^3 - a^3 = (r - a)(r^2 + ar + a^2)                                                         |
  |     r^3 + a^3 = (r + a)(r^2 - ar + a^2)                                                         |
  |                                                                                                 |
  |  5. Factoring by Grouping (4 terms):                                                            |
  |     r^3 - r^2 - r + 1 = r^2(r - 1) - (r - 1) = (r - 1)(r^2 - 1) = (r - 1)^2(r + 1)             |
  +-------------------------------------------------------------------------------------------------+
  ```
  
  ---
### 4. Fully Worked Master Examples from Both Sets of Notes
  
  ---
#### **Example 1: Fourth-Order Biquadratic (Notes PDF 1, Page 1)**
  Find the general solution of:
  $$y'''' - 16y = 0$$
  
  * **Characteristic Equation:**
  $$r^4 - 16 = 0$$
  * **Factor as a difference of squares:**
  $$(r^2 - 4)(r^2 + 4) = 0$$
  $$(r - 2)(r + 2)(r^2 + 4) = 0$$
  * **Identify Roots:**
  * $r - 2 = 0 \implies r_1 = 2$
  * $r + 2 = 0 \implies r_2 = -2$
  * $r^2 + 4 = 0 \implies r^2 = -4 \implies r_{3, 4} = \pm 2i$ ($\alpha = 0, \beta = 2$)
  * **General Solution:**
  $$\mathbf{y(t) = c_1 e^{2t} + c_2 e^{-2t} + c_3 \cos(2t) + c_4 \sin(2t)}$$
  
  ---
#### **Example 2: Fifth-Order with Repeated Zero Root (Notes PDF 1, Page 2)**
  Find the general solution of:
  $$y^{(5)} - 2y^{(4)} + 5y''' = 0$$
  
  * **Characteristic Equation:**
  $$r^5 - 2r^4 + 5r^3 = 0$$
  * **Factor out $r^3$:**
  $$r^3(r^2 - 2r + 5) = 0$$
  * **Identify Roots:**
  * $r^3 = 0 \implies r = 0$ with **multiplicity 3** ($r_1 = 0, r_2 = 0, r_3 = 0$).
    * Yields: $y_1 = e^{0t} = 1, \quad y_2 = t e^{0t} = t, \quad y_3 = t^2 e^{0t} = t^2$.
  * $r^2 - 2r + 5 = 0$:
    $$r = \frac{-(-2) \pm \sqrt{(-2)^2 - 4(1)(5)}}{2(1)} = \frac{2 \pm \sqrt{4 - 20}}{2} = \frac{2 \pm 4i}{2} = 1 \pm 2i$$
    * Complex conjugate pair: $\alpha = 1, \beta = 2$.
    * Yields: $y_4 = e^t \cos(2t), \quad y_5 = e^t \sin(2t)$.
  * **General Solution:**
  $$\mathbf{y(t) = c_1 + c_2 t + c_3 t^2 + c_4 e^t \cos(2t) + c_5 e^t \sin(2t)}$$
  
  ---
#### **Example 3: Difference of Cubes (Earlier Notes, Page 3)**
  Find the general solution of:
  $$y''' - y = 0$$
  
  * **Characteristic Equation:**
  $$r^3 - 1 = 0$$
  * **Factor using $a^3 - b^3 = (a - b)(a^2 + ab + b^2)$ with $a = r, b = 1$:**
  $$(r - 1)(r^2 + r + 1) = 0$$
  * **Identify Roots:**
  * $r - 1 = 0 \implies r_1 = 1$.
  * $r^2 + r + 1 = 0$:
    $$r = \frac{-1 \pm \sqrt{1^2 - 4(1)(1)}}{2} = \frac{-1 \pm \sqrt{-3}}{2} = -\frac{1}{2} \pm \frac{\sqrt{3}}{2}i$$
    * $\alpha = -\frac{1}{2}, \quad \beta = \frac{\sqrt{3}}{2}$.
  * **General Solution:**
  $$\mathbf{y(t) = c_1 e^t + c_2 e^{-\frac{1}{2}t}\cos\left(\frac{\sqrt{3}}{2}t\right) + c_3 e^{-\frac{1}{2}t}\sin\left(\frac{\sqrt{3}}{2}t\right)}$$
  
  ---
#### **Example 4: Factoring by Grouping (Earlier Notes, Page 4)**
  Find the general solution of:
  $$y''' - y'' - y' + y = 0$$
  
  * **Characteristic Equation:**
  $$r^3 - r^2 - r + 1 = 0$$
  * **Factor by grouping pairs of terms:**
  $$(r^3 - r^2) - (r - 1) = 0$$
  $$r^2(r - 1) - 1(r - 1) = 0$$
  $$(r - 1)(r^2 - 1) = 0$$
  $$(r - 1)(r - 1)(r + 1) = 0 \implies (r - 1)^2 (r + 1) = 0$$
  * **Identify Roots:**
  * $r = 1$ with **multiplicity 2** ($r_1 = 1, r_2 = 1$).
  * $r = -1$ with **multiplicity 1** ($r_3 = -1$).
  * **General Solution:**
  $$\mathbf{y(t) = c_1 e^t + c_2 t e^t + c_3 e^{-t}}$$
  
  ---
#### **Example 5: Repeated Complex Conjugate Roots (Earlier Notes, Pages 4–5)**
  Find the general solution of:
  $$y^{(4)} + 4y'' + 4y = 0$$
  
  * **Characteristic Equation:**
  $$r^4 + 4r^2 + 4 = 0$$
  * **Factor as a perfect square of a quadratic ($u = r^2$):**
  $$(r^2 + 2)^2 = 0$$
  * **Solve for $r$:**
  $$r^2 + 2 = 0 \implies r^2 = -2 \implies r = \pm i\sqrt{2}$$
  Because of the outer power of $2$, the pair $r = 0 \pm i\sqrt{2}$ is **repeated with multiplicity $k = 2$**:
  $$r_{1, 2} = \pm i\sqrt{2}, \qquad r_{3, 4} = \pm i\sqrt{2}$$
  * **Construct Solutions:**
  * Base unrepeated pair ($\alpha = 0, \beta = \sqrt{2}$):
    $$y_1(t) = \cos(\sqrt{2}t), \qquad y_2(t) = \sin(\sqrt{2}t)$$
  * Multiplicity 2 pair (multiply by $t$):
    $$y_3(t) = t \cos(\sqrt{2}t), \qquad y_4(t) = t \sin(\sqrt{2}t)$$
  * **General Solution:**
  $$\mathbf{y(t) = c_1 \cos(\sqrt{2}t) + c_2 \sin(\sqrt{2}t) + c_3 t \cos(\sqrt{2}t) + c_4 t \sin(\sqrt{2}t)}$$
  
  ---
#### **Example 6: Quadratic in Disguise with Distinct Imaginary Roots (Earlier Notes, Page 3)**
  Find the general solution of:
  $$y^{(4)} + 5y'' + 4y = 0$$
  
  * **Characteristic Equation:**
  $$r^4 + 5r^2 + 4 = 0$$
  * **Factor:**
  $$(r^2 + 1)(r^2 + 4) = 0$$
  * **Roots:**
  * $r^2 + 1 = 0 \implies r = \pm i \implies y_1 = \cos(t), \; y_2 = \sin(t)$
  * $r^2 + 4 = 0 \implies r = \pm 2i \implies y_3 = \cos(2t), \; y_4 = \sin(2t)$
  * **General Solution:**
  $$\mathbf{y(t) = c_1 \cos(t) + c_2 \sin(t) + c_3 \cos(2t) + c_4 \sin(2t)}$$
  
  ---
### Section 6.2 Pro-Tips & Exam Traps Checklist
  * [x] **Check Order vs. Number of Arbitrary Constants:** An $n$-th order ODE must have **exactly $n$ arbitrary constants** ($c_1, \dots, c_n$). If your solution to a 4th-order equation only has 3 constants, you missed a root.
  * [x] **Multiplicity on $r = 0$:** If you factor out $r^3$, do not just write $c_1$. It is a triple root and produces $c_1 + c_2 t + c_3 t^2$.
  * [x] **No $i$ in the General Solution:** Sines and cosines absorb the imaginary units. Never leave an $i$ in real-valued solutions.
  * [x] **Distinguish Single vs. Repeated Complex Pairs:**
  * $(r^2 + 1)(r^2 + 4) = 0 \implies$ Distinct frequencies $\cos(t)$ and $\cos(2t)$.
  * $(r^2 + 2)^2 = 0 \implies$ Same frequency, repeated: $\cos(\sqrt{2}t)$ and $t\cos(\sqrt{2}t)$.
  
  ---