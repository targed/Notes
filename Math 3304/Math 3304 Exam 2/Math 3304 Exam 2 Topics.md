## **Unit 1: Advanced Second-Order Linear Equations & Modeling**
### **§4.6: Variation of Parameters (VofP)**
  * **The Method & Formulas:**
  * Setting up the trial solution: $y_p(t) = u_1(t)y_1(t) + u_2(t)y_2(t)$.
  * The Wronskian determinant: $W(y_1, y_2)(t) = y_1 y_2' - y_1' y_2$.
  * Integrating formulas:
    $$u_1(t) = -\int \frac{y_2(t)g(t)}{W(t)}\,dt, \qquad u_2(t) = \int \frac{y_1(t)g(t)}{W(t)}\,dt$$
  * Why arbitrary constants of integration $+C$ can be set to zero.
  * **The Mandatory "Step 0":**
  * Putting the equation into standard form ($1\cdot y'' + py' + qy = g(t)$) by dividing through by the leading coefficient $a(t)$ before identifying $g(t)$.
  * **Variable-Coefficient Problems:**
  * Using VofP to solve nonhomogeneous equations with non-constant coefficients when $y_1$ and $y_2$ are provided.
  * **When to use VofP vs. MUC:**
  * Using VofP for non-MUC forcing terms: $\tan(t)$, $\sec(t)$, $\ln(t)$, $\frac{1}{t}$, $\frac{1}{1+t^2}$, and fractions.
  * *(YouTube Search: `"Variation of parameters second order differential equations"` or `"Variation of parameters when solutions are given"`)*
  
  ---
### **§4.7: Variable-Coefficient Equations — Cauchy-Euler Equations & Reduction of Order**
  * **Cauchy-Euler (Equidimensional) Equations:**
  * Recognizing the form: $a x^2 y'' + b x y' + c y = 0$ (assuming $x > 0$).
  * The power trial guess: $y = x^m$.
  * **The Characteristic Equation:**
    $$a m^2 + (b - a)m + c = 0 \quad \text{(Watch the critical } (b - a) \text{ middle term!)}$$
  * **The 3 Root Cases:**
    * **Case 1 (Distinct Real):** $y = c_1 x^{m_1} + c_2 x^{m_2}$.
    * **Case 2 (Repeated Real):** $y = c_1 x^m + c_2 x^m \ln(x)$.
    * **Case 3 (Complex Conjugates $\alpha \pm i\beta$):** $y = x^\alpha \left[ c_1\cos(\beta\ln x) + c_2\sin(\beta\ln x) \right]$.
  * **The Method of Reduction of Order:**
  * Using one known solution $y_1(x)$ to find the second independent solution: $y_2(x) = u(x)y_1(x)$.
  * Proving linear independence via $W(y_1, u y_1) = u' [y_1]^2 \neq 0$.
  * **The Sanity Check:** Verifying that all terms containing un-differentiated $u(x)$ cancel out to zero.
  * Substituting $v = u'$ to reduce the 2nd-order ODE to a 1st-order separable/linear ODE for $v(x)$, then integrating $u = \int v\,dx$.
  * *(YouTube Search: `"Cauchy Euler differential equations"` or `"Reduction of order differential equations"`)*
  
  ---
### **§4.1, 4.9, 4.10: Mechanical Vibrations & Mass-Spring Systems**
  * **Formulating the Equation of Motion:**
  * Newton’s Second Law balance: $m x'' + b x' + k x = F_{\text{ext}}(t)$.
  * Hooke’s Law and Static Equilibrium: $W = mg = kL \implies k = \frac{mg}{L}$.
  * **Unit Traps & Conversions (English vs. Metric):**
    * English: Weight is in lbs $\implies$ Mass is $m = \frac{W}{32}$ slugs. Convert inches to feet ($L = \frac{\text{inches}}{12}$).
    * Metric: Mass is in kg, $g = 9.8\text{ m/s}^2$.
    * Signs: Downward is positive ($+$), upward displacement is negative ($-$). "Released from rest" means $x'(0) = 0$.
  * **Free Undamped Motion ($b = 0, F_{\text{ext}} = 0$):**
  * Natural angular frequency $\omega_0 = \sqrt{\frac{k}{m}}$, frequency $f_0 = \frac{\omega_0}{2\pi}$, period $T = \frac{2\pi}{\omega_0}$.
  * Converting $c_1\cos(\omega_0 t) + c_2\sin(\omega_0 t)$ to **Amplitude-Phase form**:
    $$x(t) = A\sin(\omega_0 t + \phi) \quad \text{where } A = \sqrt{c_1^2 + c_2^2}, \quad \tan\phi = \frac{c_1}{c_2}$$
  * **Free Damped Motion ($b > 0, F_{\text{ext}} = 0$):**
  * Classifying by the discriminant $\Delta = b^2 - 4mk$:
    * **Overdamped ($b^2 - 4mk > 0$):** Two distinct negative real roots; exponential decay without oscillation.
    * **Critically Damped ($b^2 - 4mk = 0$):** Repeated negative root $r = -\frac{b}{2m}$; fastest non-oscillatory return to equilibrium ($x = (c_1 + c_2 t)e^{rt}$).
    * **Underdamped ($b^2 - 4mk < 0$):** Complex roots $\alpha \pm i\beta$; decaying oscillations inside envelope $\pm A e^{\alpha t}$, quasi-frequency $\beta = \frac{\sqrt{4mk - b^2}}{2m}$, quasi-period $T_d = \frac{2\pi}{\beta}$.
  * **Forced Vibrations & Resonance (§4.10):**
  * Damped Forced ($b > 0$): Homogeneous solution dies out as $t \to \infty$ (**Transient Solution**); particular solution persists (**Steady-State Solution**).
  * **Pure Resonance ($b = 0, \omega = \omega_0$):** Driving at natural frequency produces overlap in MUC:
    $$x_p(t) = \frac{F_0}{2m\omega_0} t\sin(\omega_0 t)$$
    Amplitude grows linearly without bound ($|x_p| \to \infty$ as $t \to \infty$).
  * *(YouTube Search: `"Mass spring damper differential equation"` or `"Overdamped critically damped underdamped"` or `"Resonance differential equations undamped"`)*
  
  ---
## **Unit 2: Higher-Order Linear Differential Equations**
### **§6.1: Properties of Solutions & Theory**
  * **Existence and Uniqueness for $n$-th Order IVPs:**
  * Standard form: $y^{(n)} + p_{n-1}(t)y^{(n-1)} + \dots + p_0(t)y = g(t)$.
  * Exactly $n$ initial conditions required: $y(t_0), y'(t_0), \dots, y^{(n-1)}(t_0)$.
  * Finding the largest interval of definition $I$ by locating points where any $p_i(t)$ or $g(t)$ is discontinuous.
  * **Linear Independence for $n \ge 3$ Functions:**
  * Definition: $c_1 f_1 + \dots + c_n f_n = 0 \iff c_1 = \dots = c_n = 0$.
  * Why pairwise ratios fail for $n \ge 3$ (e.g., $\{1, t, 2+3t\}$).
  * Evaluating at sample points $t$ to prove independence algebraically.
  * **The $n \times n$ Wronskian Determinant:**
  * Definition of $W(y_1, \dots, y_n)(t)$ with rows of derivatives up to order $n - 1$.
  * Computing $3 \times 3$ determinants using **Cofactor Expansion** along row 1 (remember $+ \; - \; +$).
  * Equivalence: $W \neq 0 \iff$ linearly independent $\iff$ Fundamental Set of Solutions (FSS).
  * General solution: $y = \sum_{i=1}^n c_i y_i(t)$.
  * *(YouTube Search: `"Wronskian 3x3 determinant linear independence"` or `"Higher order differential equations existence and uniqueness"`)*
  
  ---
### **§6.2: Homogeneous Higher-Order Equations ($a_n y^{(n)} + \dots + a_0 y = 0$)**
  * **The Characteristic Equation:**
  $$a_n r^n + a_{n-1} r^{n-1} + \dots + a_1 r + a_0 = 0$$
  An $n$-th degree polynomial produces exactly $n$ roots.
  * **Factoring Strategies for Degree 3, 4, and 5 Polynomials:**
  * Factoring out $r^k$ (identifying repeated roots at $r = 0$).
  * Biquadratics / Quadratics in disguise: letting $u = r^2$ for $r^4 + 5r^2 + 4 = 0$ or $(r^2 + 2)^2 = 0$.
  * Factoring by grouping 4 terms: $(r^3 - r^2) - (r - 1) = (r - 1)^2(r + 1)$.
  * Difference and sum of cubes: $r^3 \pm a^3 = (r \pm a)(r^2 \mp ar + a^2)$.
  * **Constructing Basis Solutions from Roots:**
  * Distinct real root $r \implies e^{rt}$.
  * Repeated real root $r$ of multiplicity $k \implies e^{rt}, \; t e^{rt}, \; t^2 e^{rt}, \; \dots, \; t^{k-1}e^{rt}$.
  * Distinct complex pair $\alpha \pm i\beta \implies e^{\alpha t}\cos(\beta t), \; e^{\alpha t}\sin(\beta t)$.
  * Repeated complex pair of multiplicity $k \implies$ multiply basic sinusoids by $1, t, t^2, \dots, t^{k-1}$.
  * Repeated zero root ($r = 0$ of multiplicity $k$) $\implies 1, t, t^2, \dots, t^{k-1}$.
  * *(YouTube Search: `"Higher order homogeneous differential equations constant coefficients"` or `"Characteristic equation repeated complex roots"`)*
  
  ---
### **§6.3: Nonhomogeneous Higher-Order Equations & MUC**
  * **Solution Structure:** $y(t) = y_h(t) + y_p(t)$.
  * **Formulating Trial Guesses for Higher Orders:**
  * Polynomials: $P_m(t) \implies A_m t^m + \dots + A_1 t + A_0$ (must include all lower powers).
  * Exponentials: $e^{\alpha t} \implies A e^{\alpha t}$.
  * Sinusoids: $\cos(\beta t)$ or $\sin(\beta t) \implies A\cos(\beta t) + B\sin(\beta t)$.
  * **The Higher-Order Multiplier Rule ($t^s$):**
  * If the trial guess duplicates any term in $y_h(t)$, multiply the entire guess by $t^s$, where $s$ is the **multiplicity** of the characteristic root associated with the forcing term.
  * **The Zero-Root Trap:** If $r = 0$ is a root of multiplicity $s$ (e.g., the ODE ends in $y'''$), a polynomial forcing term must be multiplied by $t^s$:
    $$y_p(t) = t^s (A_m t^m + \dots + A_0)$$
  * **Superposition for Sums:** Breaking $g(t) = g_1(t) + g_2(t)$ into separate subproblems and adding $y_p = y_{p1} + y_{p2}$.
  * *(YouTube Search: `"Method of undetermined coefficients higher order differential equations"` or `"Undetermined coefficients repeated roots t^s"`)*
  
  ---
## **Unit 3: The Laplace Transform**
### **§7.2: Definition, Linearity, and Existence**
  * **Definition of the Improper Integral:**
  $$\mathcal{L}\{f(t)\} = F(s) = \int_0^\infty e^{-st} f(t)\,dt = \lim_{T \to \infty} \int_0^T e^{-st} f(t)\,dt$$
  * **Deriving Standard Transforms from the Integral Definition:**
  * $\mathcal{L}\{1\} = \frac{1}{s}$ for $s > 0$.
  * $\mathcal{L}\{e^{at}\} = \frac{1}{s - a}$ for $s > a$.
  * $\mathcal{L}\{\sin(bt)\} = \frac{b}{s^2 + b^2}$, $\quad \mathcal{L}\{\cos(bt)\} = \frac{s}{s^2 + b^2}$.
  * **Transforming Piecewise Discontinuous Functions:**
  * Splitting the improper integral into sub-intervals (e.g., $\int_0^1 e^{-st}f_1(t)dt + \int_1^\infty e^{-st}f_2(t)dt$).
  * **Linearity of $\mathcal{L}$:** $\mathcal{L}\{c_1 f + c_2 g\} = c_1\mathcal{L}\{f\} + c_2\mathcal{L}\{g\}$.
  * **Existence Theorem:**
  * Sufficient conditions: $f(t)$ is **piecewise continuous** on $[0, T]$ and of **exponential order $c$** ($|f(t)| \le M e^{ct}$).
  * Non-examples: functions without transforms ($e^{t^2}$ grows too fast; $\frac{1}{t}$ diverges at 0).
  * *(YouTube Search: `"Laplace transform definition integral"` or `"Laplace transform piecewise function"` or `"Existence of Laplace transform exponential order"`)*
  
  ---
### **§7.3: Operational Properties of the Laplace Transform**
  * **Transforms of Derivatives (The Engine of ODE Solving):**
  $$\mathcal{L}\{f'(t)\} = s F(s) - f(0)$$
  $$\mathcal{L}\{f''(t)\} = s^2 F(s) - s f(0) - f'(0)$$
  $$\mathcal{L}\{f^{(n)}(t)\} = s^n F(s) - s^{n-1}f(0) - s^{n-2}f'(0) - \dots - f^{(n-1)}(0)$$
  *(Notice how initial conditions are automatically injected at Step 1).*
  * **First Translation / Frequency Shifting Theorem:**
  $$\mathcal{L}\{e^{at} f(t)\} = F(s - a)$$
  * Damped powers: $\mathcal{L}\{e^{at} t^n\} = \frac{n!}{(s - a)^{n+1}}$.
  * Damped sinusoids: $\mathcal{L}\{e^{at}\sin(bt)\} = \frac{b}{(s - a)^2 + b^2}$, $\quad \mathcal{L}\{e^{at}\cos(bt)\} = \frac{s - a}{(s - a)^2 + b^2}$.
  * **Differentiation in the $s$-Domain (Multiplication by $t^n$):**
  $$\mathcal{L}\{t^n f(t)\} = (-1)^n \frac{d^n}{ds^n}F(s)$$
  * Transforming $t\cos(bt) \implies -\frac{d}{ds}\left[\frac{s}{s^2 + b^2}\right] = \frac{s^2 - b^2}{(s^2 + b^2)^2}$.
  * Transforming $t\sin(bt) \implies -\frac{d}{ds}\left[\frac{b}{s^2 + b^2}\right] = \frac{2bs}{(s^2 + b^2)^2}$.
  * **Transforming Full IVPs:**
  * Applying $\mathcal{L}$ to both sides of $a y'' + b y' + c y = g(t)$ and solving algebraically for $Y(s)$:
    $$(a s^2 + b s + c)Y(s) = \text{Initial Condition terms} + G(s)$$
  * *(YouTube Search: `"Laplace transform of derivatives"` or `"Laplace transform frequency shift theorem"` or `"Laplace transform multiply by t"`)*
  
  ---
### **§7.4: The Inverse Laplace Transform & Inversion Techniques**
  * **Linearity & Table Reading:**
  * $\mathcal{L}^{-1}$ is linear.
  * Adjusting numerators for sine: $\frac{1}{s^2 + b^2} = \frac{1}{b}\left(\frac{b}{s^2 + b^2}\right) \implies \frac{1}{b}\sin(bt)$.
  * **Completing the Square for Quadratic Denominators:**
  * For irreducible quadratics $s^2 + ps + q = (s - a)^2 + b^2$:
  * **Numerator Adjustment:** If the numerator contains $s$, group it as $(s - a)$ to match the damped cosine shift:
    $$\frac{s + C}{(s - a)^2 + b^2} = \frac{(s - a) + (C + a)}{(s - a)^2 + b^2} = \frac{s - a}{(s - a)^2 + b^2} + \frac{C + a}{b}\left[\frac{b}{(s - a)^2 + b^2}\right]$$
    Inverts to: $e^{at}\cos(bt) + \frac{C+a}{b}e^{at}\sin(bt)$.
  * **Partial Fraction Decomposition Setup Rules:**
  * Distinct Linear: $\frac{A}{s - a} \implies A e^{at}$.
  * Repeated Linear: $\frac{A}{s - a} + \frac{B}{(s - a)^2} \implies A e^{at} + B t e^{at}$.
  * Irreducible Quadratic: $\frac{Bs + C}{s^2 + b^2} \implies B\cos(bt) + \frac{C}{b}\sin(bt)$.
  * **Solving Full IVPs via Inversion:**
  * Combining algebraic isolation of $Y(s)$, partial fractions / completing the square, and applying $\mathcal{L}^{-1}$ to yield $y(t)$.
  * *(YouTube Search: `"Inverse Laplace transform partial fractions"` or `"Inverse Laplace transform completing the square"` or `"Solve IVP using Laplace transform second order"`)*