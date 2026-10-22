### 1. Conceptual Foundation: Returning to the Time Domain
  
  Solving a differential equation via Laplace transforms follows a three-step cycle:
  $$\text{Differential Equation (in } t\text{)} \xrightarrow{\quad\mathcal{L}\quad} \text{Algebraic Equation (in } s\text{)} \xrightarrow{\quad\text{Solve for } Y(s)\quad} Y(s) \xrightarrow{\quad\mathbf{\mathcal{L}^{-1}}\quad} \mathbf{y(t)}$$
  
  Section 7.4 is dedicated to the final, critical step: **The Inverse Laplace Transform ($\mathcal{L}^{-1}$)**.
#### **Definition:**
  > If $\mathcal{L}\{f(t)\} = F(s)$, then the **Inverse Laplace Transform** of $F(s)$ is:
  > $$\mathbf{\mathcal{L}^{-1}\{F(s)\} = f(t)}$$
#### **Key Properties and Theoretical Notes (Notes Page 1):**
  1. **Linearity of $\mathcal{L}^{-1}$:** Just like $\mathcal{L}$, the inverse operator is completely linear:
   $$\mathbf{\mathcal{L}^{-1}\{a F(s) + b G(s)\} = a \mathcal{L}^{-1}\{F(s)\} + b \mathcal{L}^{-1}\{G(s)\}}$$
  2. **Uniqueness (Lerch’s Theorem):** Because the Laplace transform is an integral, two functions that differ only at isolated points have the exact same transform. However, if we restrict our attention to **continuous functions**, the inverse transform $\mathcal{L}^{-1}\{F(s)\}$ is **strictly unique**.
  3. **No Complex Integration Needed:** In advanced mathematics, the formal definition of $\mathcal{L}^{-1}$ is a contour line integral in the complex plane (the Bromwich integral):
   $$f(t) = \frac{1}{2\pi i}\int_{\gamma - i\infty}^{\gamma + i\infty} e^{st} F(s)\,ds$$
   In this course, we **never** compute this integral directly. Instead, we use **algebraic manipulation (Partial Fractions and Completing the Square)** to reshape $F(s)$ into recognizable entries from the **Laplace Table**.
  
  ---
### 2. The Four Fundamental Inversion Templates
  
  Every rational function $\frac{P(s)}{Q(s)}$ encountered on exams decomposes into one or more of these four basic patterns:
  
  ```
  +---------------------------------------------------------------------------------------------------+
  |                                 THE 4 CORE INVERSION TEMPLATES                                    |
  |                                                                                                   |
  |  1. Pure Linear Factor:               L^-1 { 1 / (s - a) }            = e^(at)                    |
  |                                                                                                   |
  |  2. Repeated Linear Factor:           L^-1 { n! / (s - a)^(n+1) }     = t^n * e^(at)              |
  |                                                                                                   |
  |  3. Pure Imaginary Quadratic:         L^-1 { s / (s^2 + b^2) }        = cos(bt)                   |
  |                                       L^-1 { b / (s^2 + b^2) }        = sin(bt)                   |
  |                                                                                                   |
  |  4. Shifted / Irreducible Quadratic:  L^-1 { (s - a) / ((s - a)^2 + b^2) } = e^(at) * cos(bt)     |
  |                                       L^-1 {    b    / ((s - a)^2 + b^2) } = e^(at) * sin(bt)     |
  +---------------------------------------------------------------------------------------------------+
  ```
  
  ---
### 3. Basic Inversion Techniques: Adjusting Constants
  
  Before resorting to heavy partial fractions, you often only need to factor out constants or manipulate the numerator.
  
  ---
#### **Example 1: Linearity with Simple Exponentials (Notes Page 2)**
  Find $\mathcal{L}^{-1}\left\{\frac{3}{s} + \frac{9}{s + 12}\right\}$:
  $$\begin{aligned}
  \mathcal{L}^{-1}\left\{\frac{3}{s} + \frac{9}{s + 12}\right\} &= 3\mathcal{L}^{-1}\left\{\frac{1}{s}\right\} + 9\mathcal{L}^{-1}\left\{\frac{1}{s - (-12)}\right\} \\
  &= 3(1) + 9e^{-12t} = \mathbf{3 + 9e^{-12t}}
  \end{aligned}$$
  
  ---
#### **Example 2: Splitting Numerators for Sines and Cosines (Notes Page 2)**
  Find $\mathcal{L}^{-1}\left\{\frac{s - 3}{s^2 + 4}\right\}$:
  * Split the fraction over the common denominator:
  $$\mathcal{L}^{-1}\left\{\frac{s - 3}{s^2 + 4}\right\} = \mathcal{L}^{-1}\left\{\frac{s}{s^2 + 2^2}\right\} - 3\mathcal{L}^{-1}\left\{\frac{1}{s^2 + 2^2}\right\}$$
  * The first term matches $\cos(2t)$ directly.
  * For the second term, the sine transform $\mathcal{L}\{\sin(2t)\} = \frac{2}{s^2 + 2^2}$ **requires a $2$ in the numerator**. Multiply and divide by $2$:
  $$= \cos(2t) - 3\left(\frac{1}{2}\right)\mathcal{L}^{-1}\left\{\frac{2}{s^2 + 2^2}\right\} = \mathbf{\cos(2t) - \frac{3}{2}\sin(2t)}$$
  
  ---
#### **Example 3: Repeated Linear Factor / Damped Power of $t$ (Notes Page 2)**
  Find $\mathcal{L}^{-1}\left\{\frac{2}{(s + 1)^3}\right\}$:
  * Match with Entry 8: $\mathcal{L}^{-1}\left\{\frac{n!}{(s - a)^{n+1}}\right\} = t^n e^{at}$.
  * Here, $(s - a)^{n+1} = (s - (-1))^{2+1} \implies a = -1, \; n = 2$.
  * The numerator requires $n! = 2! = 2$, which is already present!
  $$\mathcal{L}^{-1}\left\{\frac{2}{(s - (-1))^{2+1}}\right\} = \mathbf{t^2 e^{-t}}$$
  
  ---
### 4. Quadratic Denominators: The Completing the Square Technique
  
  When the denominator is an irreducible quadratic $s^2 + ps + q$ (where $p^2 - 4q < 0$), **do not use partial fractions**! Complete the square to match the shifted sine and cosine templates:
  
  $$s^2 + ps + q = \left(s + \frac{p}{2}\right)^2 + \left(q - \frac{p^2}{4}\right) = (s - a)^2 + b^2$$
  
  ---
#### **Example 4: Completing the Square for a Shifted Sine (Notes Page 2)**
  Find $\mathcal{L}^{-1}\left\{\frac{3}{s^2 - 4s + 13}\right\}$:
  
  1. **Complete the square in the denominator:**
   $$s^2 - 4s + 13 = (s^2 - 4s + 4) - 4 + 13 = (s - 2)^2 + 9 = (s - 2)^2 + 3^2$$
  2. **Identify parameters:**
   $$a = 2, \qquad b = 3$$
  3. **Inspect the numerator:**
   $$\mathcal{L}^{-1}\left\{\frac{3}{(s - 2)^2 + 3^2}\right\}$$
   The numerator contains exactly $3 = b$. This is an exact match for the damped sine formula $\mathcal{L}^{-1}\left\{\frac{b}{(s-a)^2 + b^2}\right\} = e^{at}\sin(bt)$:
   $$= \mathbf{e^{2t}\sin(3t)}$$
  
  ---
#### **Crucial Follow-up: What if the numerator has an $s$?**
  Suppose you had $\mathcal{L}^{-1}\left\{\frac{s + 1}{(s - 2)^2 + 9}\right\}$:
  * Because the denominator is shifted by $(s - 2)$, **the numerator MUST also be grouped as $(s - 2)$**:
  $$s + 1 = (s - 2) + 3$$
  * Now split into cosine and sine pieces:
  $$\frac{(s - 2) + 3}{(s - 2)^2 + 3^2} = \underbrace{\frac{s - 2}{(s - 2)^2 + 3^2}}_{\to e^{2t}\cos(3t)} + \underbrace{\frac{3}{(s - 2)^2 + 3^2}}_{\to e^{2t}\sin(3t)}$$
  $$\mathcal{L}^{-1}\left\{\frac{s + 1}{(s - 2)^2 + 9}\right\} = \mathbf{e^{2t}\cos(3t) + e^{2t}\sin(3t)}$$
  
  ---
### 5. Partial Fraction Decompositions for Laplace Transforms
  
  When the denominator factors into distinct or repeated polynomials, use **Partial Fractions**.
  
  ```
  +---------------------------------------------------------------------------------------------------+
  |                              PARTIAL FRACTION SETUP CHEAT SHEET                                   |
  |                                                                                                   |
  |  Denominator Factor                               Setup in Decomposition                          |
  |  ------------------                               ----------------------                          |
  |  1. Distinct Linear:   (s - a)                    A / (s - a)                                     |
  |  2. Repeated Linear:   (s - a)^2                  A / (s - a)  +  B / (s - a)^2                   |
  |  3. Irreducible Quad:  (s^2 + b^2)                (Bs + C) / (s^2 + b^2)                          |
  +---------------------------------------------------------------------------------------------------+
  ```
  
  ---
#### **Example 5: Linear Factor and Irreducible Quadratic (Notes Pages 3–4)**
  Find $\mathcal{L}^{-1}\left\{\frac{1}{(s + 3)(s^2 + 4)}\right\}$:
  
  * **Step 1: Set up the partial fractions:**
  $$\frac{1}{(s + 3)(s^2 + 4)} = \frac{A}{s + 3} + \frac{Bs + C}{s^2 + 4}$$
  
  * **Step 2: Clear denominators:**
  $$1 = A(s^2 + 4) + (Bs + C)(s + 3) \quad (\ast)$$
  
  * **Step 3: Solve for coefficients (Fast Hybrid Method):**
  * **Substitute $s = -3$** (kills the second term):
    $$1 = A((-3)^2 + 4) + 0 \implies 1 = 13A \implies \mathbf{A = \frac{1}{13}}$$
  * **Substitute $s = 0$:**
    $$1 = 4A + 3C \implies 1 = 4\left(\frac{1}{13}\right) + 3C \implies 3C = 1 - \frac{4}{13} = \frac{9}{13} \implies \mathbf{C = \frac{3}{13}}$$
  * **Equate $s^2$ coefficients:**
    Expanding the highest powers: $0\cdot s^2 = A s^2 + B s^2 \implies A + B = 0$:
    $$B = -A \implies \mathbf{B = -\frac{1}{13}}$$
  
  * **Step 4: Rewrite $F(s)$ into invertable pieces:**
  $$F(s) = \frac{1}{13}\left(\frac{1}{s + 3}\right) - \frac{1}{13}\left(\frac{s}{s^2 + 4}\right) + \frac{3}{13}\left(\frac{1}{s^2 + 4}\right)$$
  
  * **Step 5: Adjust sine numerator and invert:**
  For $\frac{1}{s^2 + 4}$, write $1 = \frac{1}{2} \cdot 2$:
  $$f(t) = \frac{1}{13}\mathcal{L}^{-1}\left\{\frac{1}{s + 3}\right\} - \frac{1}{13}\mathcal{L}^{-1}\left\{\frac{s}{s^2 + 2^2}\right\} + \frac{3}{13 \cdot 2}\mathcal{L}^{-1}\left\{\frac{2}{s^2 + 2^2}\right\}$$
  $$\mathbf{f(t) = \frac{1}{13}e^{-3t} - \frac{1}{13}\cos(2t) + \frac{3}{26}\sin(2t)}$$
  
  ---
#### **Example 6: Repeated Linear Factors (Notes Pages 5–6)**
  Find $\mathcal{L}^{-1}\left\{\frac{1}{(s + 2)^2(s - 1)}\right\}$:
  
  * **Step 1: Set up partial fractions:**
  $$\frac{1}{(s + 2)^2(s - 1)} = \frac{A}{s + 2} + \frac{B}{(s + 2)^2} + \frac{C}{s - 1}$$
  > *Why $\frac{A}{s+2} + \frac{B}{(s+2)^2}$?* As shown on Page 6 of your notes, this matches the table entries directly ($e^{-2t}$ and $t e^{-2t}$).
  
  * **Step 2: Clear denominators:**
  $$1 = A(s + 2)(s - 1) + B(s - 1) + C(s + 2)^2 \quad (\ast)$$
  
  * **Step 3: Solve for coefficients:**
  * **Substitute $s = 1$:**
    $$1 = 0 + 0 + C(1 + 2)^2 = 9C \implies \mathbf{C = \frac{1}{9}}$$
  * **Substitute $s = -2$:**
    $$1 = 0 + B(-2 - 1) + 0 = -3B \implies \mathbf{B = -\frac{1}{3}}$$
  * **Substitute $s = 0$:**
    $$1 = A(2)(-1) + B(-1) + C(2)^2$$
    $$1 = -2A - B + 4C = -2A - \left(-\frac{1}{3}\right) + 4\left(\frac{1}{9}\right) = -2A + \frac{3}{9} + \frac{4}{9} = -2A + \frac{7}{9}$$
    $$-2A = 1 - \frac{7}{9} = \frac{2}{9} \implies \mathbf{A = -\frac{1}{9}}$$
  
  * **Step 4: Invert each term:**
  $$F(s) = -\frac{1}{9}\left(\frac{1}{s + 2}\right) - \frac{1}{3}\left(\frac{1}{(s + 2)^2}\right) + \frac{1}{9}\left(\frac{1}{s - 1}\right)$$
  * $\mathcal{L}^{-1}\left\{\frac{1}{s + 2}\right\} = e^{-2t}$
  * $\mathcal{L}^{-1}\left\{\frac{1}{(s + 2)^2}\right\} = t e^{-2t}$ *(using Entry 8 with $n=1, a=-2$)*
  * $\mathcal{L}^{-1}\left\{\frac{1}{s - 1}\right\} = e^t$
  $$\mathbf{f(t) = -\frac{1}{9}e^{-2t} - \frac{1}{3}t e^{-2t} + \frac{1}{9}e^t}$$
  
  ---
### 6. The Complete Master Application: Solving an IVP from Scratch
  
  Let us solve a full second-order Initial Value Problem from beginning to end using everything in Chapter 7:
  
  $$\mathbf{y'' - 4y' + 13y = 0, \quad y(0) = 2, \quad y'(0) = -2}$$
#### **Step 1: Take the Laplace Transform of both sides**
  $$\mathcal{L}\{y''\} - 4\mathcal{L}\{y'\} + 13\mathcal{L}\{y\} = 0$$
  $$\left[s^2 Y(s) - s y(0) - y'(0)\right] - 4\left[s Y(s) - y(0)\right] + 13 Y(s) = 0$$
#### **Step 2: Plug in initial conditions $y(0) = 2$ and $y'(0) = -2$**
  $$\left[s^2 Y(s) - 2s - (-2)\right] - 4\left[s Y(s) - 2\right] + 13 Y(s) = 0$$
  $$s^2 Y(s) - 2s + 2 - 4s Y(s) + 8 + 13 Y(s) = 0$$
  $$(s^2 - 4s + 13)Y(s) - 2s + 10 = 0$$
#### **Step 3: Solve algebraically for $Y(s)$**
  $$(s^2 - 4s + 13)Y(s) = 2s - 10$$
  $$Y(s) = \frac{2s - 10}{s^2 - 4s + 13}$$
#### **Step 4: Prepare the fraction for inversion (Complete the Square)**
  * Denominator: $s^2 - 4s + 13 = (s - 2)^2 + 9 = (s - 2)^2 + 3^2$ ($a = 2, b = 3$).
  * Rewrite numerator in terms of $(s - 2)$:
  $$2s - 10 = 2(s - 2 + 2) - 10 = 2(s - 2) + 4 - 10 = 2(s - 2) - 6$$
  * Split the fraction:
  $$Y(s) = \frac{2(s - 2) - 6}{(s - 2)^2 + 3^2} = 2\left[\frac{s - 2}{(s - 2)^2 + 3^2}\right] - 6\left[\frac{1}{(s - 2)^2 + 3^2}\right]$$
  * Factor the sine numerator ($6 = 2 \cdot 3$):
  $$Y(s) = 2\left[\frac{s - 2}{(s - 2)^2 + 3^2}\right] - 2\left[\frac{3}{(s - 2)^2 + 3^2}\right]$$
#### **Step 5: Take the Inverse Laplace Transform**
  $$\mathbf{y(t) = 2e^{2t}\cos(3t) - 2e^{2t}\sin(3t)}$$
  
  ---
### Section 7.4 Summary & Exam Checklist
  * [x] **Completing the Square vs. Partial Fractions:**
  * If denominator has real roots $\implies$ Factor and use **Partial Fractions**.
  * If denominator has complex roots $\implies$ **Complete the Square** for shifted $\sin / \cos$.
  * [x] **Match the Shift in the Numerator:** If the denominator has $(s - a)^2$, rewrite every $s$ in the numerator as $(s - a)$ before splitting terms!
  * [x] **Sine Constant Check:** Remember that $\mathcal{L}\{\sin(bt)\} = \frac{b}{s^2 + b^2}$. If the numerator doesn't have $b$, multiply and divide by $b$.
  * [x] **Repeated Factors:** For $(s - a)^2$, use the damped power formula $\mathcal{L}^{-1}\left\{\frac{1}{(s-a)^2}\right\} = t e^{at}$.
  
  ---