### **Unit 1: Introduction to First-Order DEs (§1.1 – §1.3)**
  
  * **Mathematical Modeling & Terminology (§1.1)**
  * Concept: Translating real-world rates of change into equations (Newton’s cooling, falling bodies with drag).
  * Classifying by **Type**: Ordinary Differential Equations (ODEs) vs. Partial Differential Equations (PDEs).
  * Identifying **Dependent** vs. **Independent** variables.
  * *(YouTube Search: `"Classifying differential equations ODE vs PDE"`)*
  
  * **Order, Linearity, and Homogeneity (§1.1)**
  * Determining the **Order** (highest derivative, distinguishing power vs. order).
  * Determining **Linearity** (first degree in $y, y', \dots$, no products of dependent variables, no nonlinear functions like $\sin(y)$ or $e^y$).
  * Determining **Homogeneity** (whether isolated forcing term $g(t) = 0$).
  * *(YouTube Search: `"Linear vs nonlinear differential equations"` or `"Order and degree of differential equations"`)*
  
  * **Solutions and Initial Value Problems (§1.2)**
  * **Verifying** explicit and implicit solutions by computing derivatives and plugging them in.
  * Stating the **Interval of Definition / Validity** (avoiding division by zero, e.g., $y = 1/x$).
  * General solution (with arbitrary constant $C$) vs. Particular solution (using $y(t_0) = y_0$).
  * Solving Euler-Cauchy forms using the power guess: $y = t^r$.
  * *(YouTube Search: `"Verify solution to differential equation"` or `"Find interval of validity differential equation"`)*
  
  * **Picard’s Theorem (Existence & Uniqueness for First-Order ODEs) (§1.2)**
  * Checking continuity of $f(t, y)$ for **existence**.
  * Checking continuity of $\frac{\partial f}{\partial y}$ for **uniqueness**.
  * Finding the rectangle $R$ around $(t_0, y_0)$ where a unique solution is guaranteed.
  * What happens when $\frac{\partial f}{\partial y}$ blows up (vertical tangent / infinite solutions, e.g., $y' = y^{1/3}$).
  * *(YouTube Search: `"Existence and uniqueness theorem for first order differential equations"`)*
  
  * **Direction Fields & Qualitative Behavior (§1.3)**
  * Constructing and interpreting slope/direction fields.
  * **Autonomous Equations** ($y' = f(y)$): finding equilibrium solutions ($f(y) = 0$).
  * Long-term behavior ($\lim_{t \to \infty} y(t)$) and stability (attractors vs. repellers).
  * Why distinct solution curves can never intersect (consequence of uniqueness).
  * **The Method of Isoclines**: finding curves where slope is constant ($f(t, y) = c$).
  * *(YouTube Search: `"Direction fields differential equations"` or `"Method of isoclines differential equations"`)*
  
  ---
### **Unit 2: First-Order Solving Methods & Applications (§2.2 – §2.3, §3.2)**
  
  * **Separation of Variables (§2.2)**
  * Algebraic separation: $\frac{1}{h(y)}\,dy = g(x)\,dx$ and integrating both sides.
  * Recognizing and recovering **lost/singular constant solutions** ($h(y) = 0$).
  * Choosing the correct sign ($\pm\sqrt{\dots}$) to match initial conditions $y(t_0) = y_0$.
  * Algebraic techniques: Partial fractions, exponent rules ($e^{a-b} = e^a / e^b$), and completing the square.
  * *(YouTube Search: `"Separation of variables differential equations"` or `"Separable differential equations initial value problem"`)*
  
  * **First-Order Linear Equations & The Integrating Factor Method (§2.3)**
  * Step 0: Converting to standard form $y' + p(t)y = g(t)$ (dividing by the leading coefficient).
  * Deriving and calculating the integrating factor: $\mu(t) = e^{\int p(t)\,dt}$.
  * Logarithm simplifications: $e^{k\ln(t)} = t^k$, $e^{-\ln(t)} = \frac{1}{t}$.
  * The reverse product rule: $\frac{d}{dt}[\mu(t)y] = \mu(t)g(t)$ and dividing by $\mu(t)$ (remembering to divide $+C$).
  * **Linear Existence & Uniqueness Theorem**: finding the interval of validity by inspecting discontinuities of $p(t)$ and $g(t)$ *without solving*.
  * Finite-time blow-up in nonlinear vs. linear equations.
  * *(YouTube Search: `"Integrating factor method differential equations"` or `"First order linear differential equations interval of validity"`)*
  
  * **Compartmental Analysis: Mixing / Tank Problems (§3.2)**
  * Conservation balance law: $\frac{dQ}{dt} = \text{Rate In} - \text{Rate Out} = (r_{\text{in}} C_{\text{in}}) - (r_{\text{out}} C_{\text{out}})$.
  * Well-stirred assumption: $C_{\text{out}}(t) = \frac{Q(t)}{V(t)}$.
  * **Constant Volume** ($r_{\text{in}} = r_{\text{out}}$): setting up and solving via integrating factor or separation.
  * **Variable Volume** ($r_{\text{in}} \neq r_{\text{out}}$): volume equation $V(t) = V_0 + (r_{\text{in}} - r_{\text{out}})t$, finding time to overflow.
  * *(YouTube Search: `"Mixing tank differential equations"` or `"Tank mixing problem variable volume"`)*
  
  * **Population Dynamics Models (§3.2)**
  * Exponential / Malthusian growth: $P' = rP$.
  * **Logistic growth**: $P' = r(1 - P/K)P$, environmental carrying capacity $K$, inflection point at $P = K/2$.
  * Critical threshold model: $P' = -r(1 - P/T)P$ (extinction vs. explosion).
  * Logistic growth with threshold (Allee effect): $P' = -r(1 - P/T)(1 - P/K)P$.
  * Logistic models with constant harvesting: $P' = f(P) - h$.
  * *(YouTube Search: `"Logistic growth differential equations"` or `"Population models differential equations"`)*
  
  * **Mechanical Modeling: Inclined Plane with Friction & Drag (§3.2)**
  * Newton’s 2nd Law on a tilted axis: $m v' = \sum F_x$.
  * Normal force balance: $F_N = mg\cos\theta$.
  * Parallel forces: gravity $mg\sin\theta$, kinetic friction $\mu mg\cos\theta$, and air drag $kv$.
  * Determining terminal velocity ($v' = 0$).
  * *(YouTube Search: `"Differential equations inclined plane with friction and drag"`)*
  
  ---
### **Unit 3: Second-Order Linear Homogeneous Equations (§4.2 – §4.3)**
  
  * **Theory of Second-Order Linear Equations (§4.2)**
  * Standard form: $y'' + p(t)y' + q(t)y = 0$.
  * Existence and uniqueness for 2nd-order IVPs (requires 2 initial conditions: $y(t_0) = y_0$ and $y'(t_0) = y_1$).
  * **The Superposition Principle**: linear combinations $c_1 y_1 + c_2 y_2$ are solutions.
  * **The Wronskian Determinant**: $W(y_1, y_2)(t) = y_1 y_2' - y_1' y_2$.
  * Linear Independence vs. Dependence ($W \neq 0 \iff$ linearly independent).
  * Fundamental Set of Solutions and writing the General Solution: $y(t) = c_1 y_1(t) + c_2 y_2(t)$.
  * **Abel’s Theorem**: $W(t) = C e^{-\int p(t)\,dt}$ ($W$ is either never zero or always zero).
  * Finding an unknown function $g(t)$ when given $W(f, g)$ and $f(t)$ via 1st-order linear ODEs.
  * *(YouTube Search: `"Wronskian and linear independence differential equations"` or `"Abels theorem differential equations"`)*
  
  * **Constant Coefficients: Real Roots (§4.2)**
  * Setting up the **Characteristic Equation**: $ar^2 + br + c = 0$.
  * **Case 1: Real Distinct Roots ($b^2 - 4ac > 0$)**:
    * Roots $r_1 \neq r_2 \implies y(t) = c_1 e^{r_1 t} + c_2 e^{r_2 t}$.
  * **Case 2: Real Repeated Roots ($b^2 - 4ac = 0$)**:
    * Root $r = -\frac{b}{2a} \implies y(t) = c_1 e^{rt} + c_2 t e^{rt}$.
    * Theoretical justification of the extra factor of $t$ via **Reduction of Order** ($y_2 = v(t)y_1$).
  * Solving initial value problems by taking the derivative $y'(t)$ and setting up a $2 \times 2$ system for $c_1, c_2$.
  * *(YouTube Search: `"Second order homogeneous differential equations constant coefficients"` or `"Repeated roots characteristic equation differential equations"`)*
  
  * **Constant Coefficients: Complex Roots (§4.3)**
  * **Case 3: Complex Conjugate Roots ($b^2 - 4ac < 0$)**:
    * Roots $r = \alpha \pm i\beta$ where $\alpha = -b/(2a)$ and $\beta = \frac{\sqrt{4ac - b^2}}{2a} > 0$.
  * Derivation using Euler’s Formula: $e^{i\theta} = \cos\theta + i\sin\theta$.
  * General Solution: $y(t) = e^{\alpha t}\left(c_1\cos(\beta t) + c_2\sin(\beta t)\right)$.
  * Physical interpretation: $\alpha$ controls exponential decay/growth (damping), $\beta$ controls oscillation frequency.
  * Computing $y'(t)$ using the Product Rule for complex conjugate IVPs.
  * *(YouTube Search: `"Complex roots characteristic equation differential equations"` or `"Second order differential equations complex roots"`)*
  
  ---
### **Unit 4: Nonhomogeneous Equations & Undetermined Coefficients (§4.4 – §4.5)**
  
  * **Structure of Nonhomogeneous Solutions (§4.4)**
  * General solution structure: $y(t) = y_h(t) + y_p(t)$ (complementary solution + particular solution).
  * Proof that difference of two nonhomogeneous solutions solves the homogeneous equation.
  * Superposition for forcing functions: if $g(t) = g_1(t) + g_2(t)$, then $y_p(t) = y_{p1}(t) + y_{p2}(t)$.
  * *(YouTube Search: `"Nonhomogeneous differential equations complementary and particular solution"`)*
  
  * **The Method of Undetermined Coefficients (MUC) — Basic Forms (§4.4)**
  * When MUC works (polynomials, exponentials, sines/cosines).
  * Formulating trial guesses:
    * Exponential: $g(t) = k e^{\alpha t} \implies y_p(t) = A e^{\alpha t}$.
    * Trigonometric: $g(t) = k\cos(\beta t)$ or $k\sin(\beta t) \implies y_p(t) = A\cos(\beta t) + B\sin(\beta t)$ (both must be included!).
    * Polynomial: $g(t) = a_n t^n + \dots + a_0 \implies y_p(t) = A_n t^n + \dots + A_0$ (complete polynomial with all lower powers).
  * Substituting $y_p, y_p', y_p''$ into the ODE, matching like terms, and solving for the undetermined coefficients.
  * *(YouTube Search: `"Method of undetermined coefficients second order differential equations"`)*
  
  * **The Method of Undetermined Coefficients Revisited — Duplication Rule (§4.5)**
  * **The Overlap / Duplication Problem**: what happens when the initial trial guess for $y_p(t)$ is already part of $y_h(t)$ ($L[y_p] = 0$).
  * **The Multiplier Rule**: multiplying the trial guess by $t^s$:
    * Multiply by $t$ ($s = 1$) if the forcing frequency/exponent matches a **single root** of the characteristic equation.
    * Multiply by $t^2$ ($s = 2$) if it matches a **repeated root** ($r_1 = r_2$).
  * Product forcing terms (e.g., $g(t) = t e^{\alpha t}$ or $g(t) = e^{\alpha t}\cos(\beta t)$).
  * **Applying Initial Conditions correctly**: only evaluating at $t = t_0$ **after** writing out the complete $y(t) = y_h(t) + y_p(t)$.
  * *(YouTube Search: `"Undetermined coefficients duplication rule"` or `"Undetermined coefficients multiply by t"`)*