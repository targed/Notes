### 1. Motivation: Why Do We Need the Laplace Transform?
  
  In Chapters 4 and 6, you solved forced linear differential equations:
  $$a y'' + b y' + c y = f(t)$$
  using **Undetermined Coefficients (MUC)** and **Variation of Parameters (VofP)**. 
  
  Both methods require $f(t)$ to be reasonably smooth and continuous. However, modern engineering and physical systems routinely involve:
  1. **Discontinuous Forcing Functions:** Switches turning currents on and off, thermostats cycling, or piecewise steps.
  2. **Impulsive Forcing Functions:** Instantaneous blows with high peak forces over microscopic durations (e.g., a hammer hitting a mass, a lightning strike on a circuit, a baseball bat hitting a ball).
  
  ```
  DISCONTINUOUS ("ON/OFF") SWITCHING           IMPULSIVE STRIKE (DIRAC DELTA)
     f(t) ^   ON                                  f(t) ^   Instantaneous blow
          |--------+                                   |   |
          |        |                                   |   | (infinite spike,
          |        |  OFF                              |   |  finite impulse)
     -----+--------+--------> t                   -----+---+-----------------> t
          0        T                                   0   t0
  ```
  
  Classical calculus methods struggle with these discontinuous inputs. The **Laplace Transform** solves them systematically by converting calculus into algebra.
  
  ---
### 2. The Global Workflow: The Laplace Transform Machine
  
  Instead of solving differential equations directly in the **Time Domain ($t$)**, we transform the problem into the **Frequency / Complex Domain ($s$)**:
  
  ```
  +-----------------------------------------------------------------------------------------+
  |                              THE LAPLACE SOLVING MACHINE                                |
  |                                                                                         |
  |      TIME DOMAIN (t)                                     FREQUENCY DOMAIN (s)           |
  |                                                                                         |
  |   Differential Equation                           Algebraic Equation                    |
  |   ay'' + by' + cy = f(t)       --- Laplace --->   (as^2 + bs + c)Y(s) = F(s) + I.C.     |
  |   with y(0), y'(0)              Transform (L)                                           |
  |                                                              |                          |
  |         ^                                                    | Solve algebraically      |
  |         |                                                    v                          |
  |   Solution y(t)                <-- Inverse ---        Transformed Solution              |
  |                                   Laplace                    Y(s)                       |
  |                                Transform (L^-1)                                         |
  +-----------------------------------------------------------------------------------------+
  ```
  
  * **The Core Benefit:** Differential equations with initial conditions become **simple linear algebraic equations** in terms of $Y(s)$. Once you solve for $Y(s)$, applying the **Inverse Laplace Transform ($\mathcal{L}^{-1}$)** yields the solution $y(t)$ directly—**with initial conditions already incorporated automatically!**
  
  ---
### 3. Formal Definition of the Laplace Transform
  
  > **Definition:** Let $f(t)$ be a function defined for $t \ge 0$. The **Laplace Transform** of $f(t)$, denoted by $\mathcal{L}\{f(t)\}$ or $F(s)$, is defined by the improper integral:
  > 
  > $$\mathbf{\mathcal{L}\{f(t)\} = F(s) = \int_0^\infty e^{-st} f(t)\,dt}$$
  > 
  > for all values of $s$ for which the improper integral converges.
#### **Mathematical Notes on the Definition:**
  1. **Improper Integral Limit:** By definition:
   $$\int_0^\infty e^{-st} f(t)\,dt = \lim_{T \to \infty} \int_0^T e^{-st} f(t)\,dt$$
  2. **The "Damping" Factor $e^{-st}$:** The kernel $e^{-st}$ acts as an exponential weight that suppresses $f(t)$ as $t \to \infty$, ensuring the integral converges (provided $s$ is sufficiently large).
  3. **Domain Shift:** The operator $\mathcal{L}$ takes a function of **time $t$** (with units of seconds) and outputs a function of **frequency $s$** (with units of $\text{sec}^{-1}$).
  
  ---
### 4. Derivations from First Principles (Improper Integrals)
  
  Exam questions often ask you to compute a Laplace transform **directly from the integral definition** rather than using the table.
  
  ---
#### **Derivation 1: Transform of a Constant ($f(t) = 1$)**
  $$\mathcal{L}\{1\} = \int_0^\infty e^{-st}(1)\,dt = \lim_{T \to \infty} \int_0^T e^{-st}\,dt$$
  Evaluate the antiderivative:
  $$\lim_{T \to \infty} \left[ -\frac{1}{s} e^{-st} \right]_0^T = \lim_{T \to \infty} \left( -\frac{1}{s} e^{-sT} - \left(-\frac{1}{s} e^0\right) \right) = \lim_{T \to \infty} \left( -\frac{1}{s} e^{-sT} + \frac{1}{s} \right)$$
  * If $s > 0 \implies \lim_{T \to \infty} e^{-sT} = 0$:
  $$= -\frac{1}{s}(0) + \frac{1}{s} = \frac{1}{s}$$
  * If $s \le 0$, the integral diverges.
  
  $$\mathbf{\mathcal{L}\{1\} = \frac{1}{s}, \quad s > 0}$$
  
  ---
#### **Derivation 2: Transform of an Exponential ($f(t) = e^{at}$)**
  $$\mathcal{L}\{e^{at}\} = \int_0^\infty e^{-st} e^{at}\,dt = \int_0^\infty e^{-(s - a)t}\,dt = \lim_{T \to \infty} \int_0^T e^{-(s - a)t}\,dt$$
  Evaluate the integral:
  $$\lim_{T \to \infty} \left[ -\frac{1}{s - a} e^{-(s - a)t} \right]_0^T = \lim_{T \to \infty} \left( -\frac{1}{s - a} e^{-(s - a)T} + \frac{1}{s - a} \right)$$
  * For the exponential to decay to zero as $T \to \infty$, we require the exponent to be strictly negative:
  $$-(s - a) < 0 \iff s - a > 0 \iff \mathbf{s > a}$$
  When $s > a$, $\lim_{T \to \infty} e^{-(s - a)T} = 0$:
  $$= 0 + \frac{1}{s - a} = \frac{1}{s - a}$$
  
  $$\mathbf{\mathcal{L}\{e^{at}\} = \frac{1}{s - a}, \quad s > a}$$
  
  ---
#### **Derivation 3: Transform of Sines and Cosines via Euler’s Formula**
  Instead of tedious integration by parts twice, use Euler's formula $e^{i b t} = \cos(bt) + i\sin(bt)$:
  $$\mathcal{L}\{\cos(bt) + i\sin(bt)\} = \mathcal{L}\{e^{i b t}\} = \frac{1}{s - ib} \quad (\text{replacing } a \text{ with } ib)$$
  Multiply numerator and denominator by the complex conjugate $(s + ib)$:
  $$\frac{1}{s - ib} \cdot \frac{s + ib}{s + ib} = \frac{s + ib}{s^2 - (ib)^2} = \frac{s + ib}{s^2 + b^2} = \mathbf{\frac{s}{s^2 + b^2}} + i \mathbf{\frac{b}{s^2 + b^2}}$$
  Equating real and imaginary parts:
  $$\mathbf{\mathcal{L}\{\cos(bt)\} = \frac{s}{s^2 + b^2}}, \qquad \mathbf{\mathcal{L}\{\sin(bt)\} = \frac{b}{s^2 + b^2}} \quad (s > 0)$$
  
  ---
### 5. Table of Standard Elementary Laplace Transforms
  
  These are the primary transforms from your table that you must master:
  
  | Entry | Time Function $f(t)$ | Laplace Transform $F(s) = \mathcal{L}\{f(t)\}$ | Convergence Domain |
  |:---:|:---:|:---:|:---:|
  | **1** | $1$ | $\mathbf{\frac{1}{s}}$ | $s > 0$ |
  | **2** | $t^n \quad (n = 1, 2, 3, \dots)$ | $\mathbf{\frac{n!}{s^{n+1}}}$ | $s > 0$ |
  | **3** | $e^{at}$ | $\mathbf{\frac{1}{s - a}}$ | $s > a$ |
  | **4** | $\sin(bt)$ | $\mathbf{\frac{b}{s^2 + b^2}}$ | $s > 0$ |
  | **5** | $\cos(bt)$ | $\mathbf{\frac{s}{s^2 + b^2}}$ | $s > 0$ |
  | **6** | $\sinh(bt)$ | $\mathbf{\frac{b}{s^2 - b^2}}$ | $s > \|b\|$ |
  | **7** | $\cosh(bt)$ | $\mathbf{\frac{s}{s^2 - b^2}}$ | $s > \|b\|$ |
  | **8** | $t^\alpha \quad (\alpha > -1)$ | $\mathbf{\frac{\Gamma(\alpha + 1)}{s^{\alpha + 1}}}$ | $s > 0$ |
  
  *(Note: $\Gamma(\alpha) = \int_0^\infty t^{\alpha - 1} e^{-t}\,dt$ is the Gamma function; for integers, $\Gamma(n + 1) = n!$).*
  
  ---
### 6. Linearity of the Laplace Transform
  
  Because integration is linear, the Laplace transform is a **linear operator**:
  
  $$\mathbf{\mathcal{L}\{c_1 f(t) + c_2 g(t)\} = c_1 \mathcal{L}\{f(t)\} + c_2 \mathcal{L}\{g(t)\}}$$
#### **Proof from Definition:**
  $$\begin{aligned}
  \mathcal{L}\{c_1 f(t) + c_2 g(t)\} &= \int_0^\infty e^{-st}\left[c_1 f(t) + c_2 g(t)\right]\,dt \\
  &= \int_0^\infty \left[c_1 e^{-st} f(t) + c_2 e^{-st} g(t)\right]\,dt \\
  &= c_1 \int_0^\infty e^{-st} f(t)\,dt + c_2 \int_0^\infty e^{-st} g(t)\,dt \\
  &= c_1 \mathcal{L}\{f(t)\} + c_2 \mathcal{L}\{g(t)\} \quad \blacksquare
  \end{aligned}$$
#### **Example from Notes (Page 4):**
  Compute $\mathcal{L}\{3t^2 + 9\sin(2t)\}$:
  $$\begin{aligned}
  \mathcal{L}\{3t^2 + 9\sin(2t)\} &= 3\mathcal{L}\{t^2\} + 9\mathcal{L}\{\sin(2t)\} \\
  &= 3\left(\frac{2!}{s^{2+1}}\right) + 9\left(\frac{2}{s^2 + 2^2}\right) \\
  &= 3\left(\frac{2}{s^3}\right) + 9\left(\frac{2}{s^2 + 4}\right) \\
  &= \mathbf{\frac{6}{s^3} + \frac{18}{s^2 + 4}}
  \end{aligned}$$
  
  ---
### 7. Theoretical Conditions for Existence
  
  Does every function have a Laplace transform? **No.** The improper integral must converge.
  
  > **Theorem (Sufficient Conditions for Existence — Notes Page 5):**
  > If a function $f(t)$ satisfies:
  > 1. **Piecewise Continuity:** $f(t)$ is continuous on every finite interval $[0, T]$, except possibly for a finite number of jump discontinuities.
  > 2. **Exponential Order $c$:** There exist positive constants $M > 0$, $c$, and $T_0 > 0$ such that:
  >    $$\mathbf{|f(t)| \le M e^{ct} \quad \text{for all } t > T_0}$$
  > 
  > Then the Laplace transform $\mathcal{L}\{f(t)\} = F(s)$ **exists (converges) for all $s > c$**.
  
  ```
    |f(t)| ^                          Grows too fast! (e.g., e^(t^2))
           |                         /    --> Laplace Transform DOES NOT EXIST!
           |                        /
           |       Exponential     /
           |        Bound:        /
           |       M * e^(ct)   /
           |          . - ' ' ' 
           |      . '
           |  . '   |f(t)| is trapped beneath the exponential bound!
           +---------------------------------------------> t
           0          T0
  ```
#### **Functions That Do NOT Have a Laplace Transform:**
  * **$f(t) = e^{t^2}$:** Grows much faster than any linear exponential $M e^{ct}$. The integral $\int_0^\infty e^{-st} e^{t^2} dt = \int_0^\infty e^{t^2 - st} dt$ diverges to $\infty$ for **every** real value of $s$.
  * **$f(t) = \frac{1}{t}$ or $\frac{1}{t^2}$:** Blows up to infinity at $t = 0$. The integral diverges at the lower limit $t = 0$.
  
  ---
### 8. Master Worked Example: Direct Integration of a Piecewise Function
  
  *(Notes PDF 1, Page 5 — Fully Solved Step-by-Step)*
  
  Find the Laplace transform of the piecewise discontinuous function directly from the definition:
  $$f(t) = \begin{cases} e^t, & 0 \le t < 1 \\ 1 + e^{2t}, & t \ge 1 \end{cases}$$
#### **Step 1: Set up the Split Integral**
  By the definition of the improper integral:
  $$\mathcal{L}\{f(t)\} = \int_0^\infty e^{-st} f(t)\,dt = \int_0^1 e^{-st} (e^t)\,dt + \int_1^\infty e^{-st}(1 + e^{2t})\,dt$$
#### **Step 2: Combine Exponentials**
  $$\mathcal{L}\{f(t)\} = \underbrace{\int_0^1 e^{(1 - s)t}\,dt}_{I_1} + \underbrace{\int_1^\infty e^{-st}\,dt}_{I_2} + \underbrace{\int_1^\infty e^{(2 - s)t}\,dt}_{I_3}$$
#### **Step 3: Evaluate Each Integral Carefully**
  * **Integral $I_1$ (Finite limits):**
  $$I_1 = \left[ \frac{e^{(1 - s)t}}{1 - s} \right]_0^1 = \frac{e^{1 - s}}{1 - s} - \frac{e^0}{1 - s} = \mathbf{\frac{e^{1 - s} - 1}{1 - s}}$$
  
  * **Integral $I_2$ (Improper integral, requires $s > 0$):**
  $$I_2 = \lim_{T \to \infty} \left[ \frac{e^{-st}}{-s} \right]_1^T = \lim_{T \to \infty} \left( -\frac{e^{-sT}}{s} - \left(-\frac{e^{-s(1)}}{s}\right) \right) = 0 + \frac{e^{-s}}{s} = \mathbf{\frac{e^{-s}}{s}}$$
  
  * **Integral $I_3$ (Improper integral, requires $s > 2$):**
  $$I_3 = \lim_{T \to \infty} \left[ \frac{e^{(2 - s)t}}{2 - s} \right]_1^T = \lim_{T \to \infty} \left( \frac{e^{(2 - s)T}}{2 - s} - \frac{e^{(2 - s)(1)}}{2 - s} \right)$$
  For $s > 2$, $(2 - s) < 0$, so $\lim_{T \to \infty} e^{(2 - s)T} = 0$:
  $$I_3 = 0 - \frac{e^{2 - s}}{2 - s} = -\frac{e^{2 - s}}{2 - s} = \mathbf{\frac{e^{2 - s}}{s - 2}}$$
#### **Step 4: Sum the Three Pieces**
  $$\mathbf{F(s) = \frac{e^{1 - s} - 1}{1 - s} + \frac{e^{-s}}{s} + \frac{e^{2 - s}}{s - 2}, \quad s > 2}$$
  
  ---
### Section 7.2 Pro-Tips & Exam Traps Checklist
  * [x] **Watch the $s$ Variable:** The result of a Laplace transform must contain **only $s$**, never $t$.
  * [x] **Linearity Rule:** Remember that $\mathcal{L}\{f(t) \cdot g(t)\} \neq \mathcal{L}\{f(t)\} \cdot \mathcal{L}\{g(t)\}$. You cannot multiply transforms!
  * [x] **Convergence Conditions:** When asked where $\mathcal{L}\{e^{at}\}$ converges, state $s > a$. For sines/cosines, state $s > 0$.
  * [x] **Split Integrals for Piecewise Functions:** Integrate piecewise functions segment-by-segment using exact calculus limits.
  
  ---