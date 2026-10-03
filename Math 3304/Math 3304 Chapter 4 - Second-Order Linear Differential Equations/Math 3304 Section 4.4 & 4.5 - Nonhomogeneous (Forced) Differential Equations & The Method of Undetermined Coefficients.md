### 1. The Structure of Solutions to Nonhomogeneous Equations
  
  We now consider the forced (nonhomogeneous) linear equation:
  $$ay'' + by' + cy = g(t) \quad (\ast)$$
  where $a, b, c$ are constants and $g(t) \neq 0$ is the external driving/forcing term.
  
  ```
  +---------------------------------------------------------------------------------+
  |                       THE GENERAL SOLUTION THEOREM                              |
  |                                                                                 |
  |                  y(t) = y_h(t) + y_p(t)   [or  y_c(t) + Y(t)]                   |
  |                                                                                 |
  |  * y_h(t) = c_1 y_1(t) + c_2 y_2(t) : General solution to homogeneous eq.       |
  |  * y_p(t)                           : ANY single particular solution to (*)     |
  +---------------------------------------------------------------------------------+
  ```
#### **Theoretical Proof (Slides 175–184):**
  Why does adding *any* single particular solution $y_p$ to $y_h$ produce *every* possible solution?
  1. Suppose $Y_1(t)$ and $Y_2(t)$ are **any two solutions** of the nonhomogeneous equation $L[y] = g(t)$:
   $$L[Y_1] = g(t) \quad \text{and} \quad L[Y_2] = g(t)$$
  2. Subtract the two equations. By linearity of the differential operator $L$:
   $$L[Y_1 - Y_2] = L[Y_1] - L[Y_2] = g(t) - g(t) = 0$$
  3. This proves that their difference **$Y_1 - Y_2$ is a solution to the corresponding homogeneous equation** $L[y] = 0$.
  4. Since $\{y_1, y_2\}$ forms a fundamental set of solutions for the homogeneous equation, there exist unique constants $c_1, c_2$ such that:
   $$Y_1 - Y_2 = c_1 y_1(t) + c_2 y_2(t) = y_h(t)$$
  5. Letting $Y_1 = y(t)$ (an arbitrary solution) and $Y_2 = y_p(t)$ (our known particular solution):
   $$y(t) - y_p(t) = y_h(t) \implies \mathbf{y(t) = y_h(t) + y_p(t)} \quad \blacksquare$$
  
  ---
### 2. The Superposition Principle for Nonhomogeneous Equations (Slide 295)
  
  If the forcing term is a sum of several distinct components:
  $$L[y] = g_1(t) + g_2(t) + \dots + g_m(t)$$
  and $y_{p1}$ solves $L[y] = g_1(t)$, $y_{p2}$ solves $L[y] = g_2(t)$, etc., then:
  $$\mathbf{y_p(t) = y_{p1}(t) + y_{p2}(t) + \dots + y_{pm}(t)}$$
  is a particular solution to the combined equation. You can break a complicated forcing function into simple pieces, solve for each piece independently, and add the results together.
  
  ---
### 3. The 4-Step Master Algorithm
  
  1. **Step 1 (Find $y_h$):** Solve the unforced/homogeneous equation $ay'' + by' + cy = 0$ via the characteristic equation $ar^2 + br + c = 0$ to get $y_h = c_1 y_1 + c_2 y_2$.
  2. **Step 2 (Find $y_p$):** Find a single particular solution using the **Method of Undetermined Coefficients** (or Variation of Parameters).
  3. **Step 3 (Assemble $y$):** Write the full general solution:
   $$y(t) = y_h(t) + y_p(t) = c_1 y_1(t) + c_2 y_2(t) + y_p(t)$$
  4. **Step 4 (Initial Conditions):** If initial conditions are given, solve for $c_1$ and $c_2$ using $y(t)$ and $y'(t)$.
  
  > ⚠️ **THE #1 EXAM TRAP OF CHAPTER 4:**
  > **Never** solve for $c_1$ and $c_2$ before adding $y_p$! 
  > Initial conditions apply to the **total physical motion** $y(t) = y_h(t) + y_p(t)$, not just the complementary part $y_h(t)$. Applying initial conditions to $y_h(t)$ alone guarantees wrong constants and lost points.
  
  ---
### 4. The Method of Undetermined Coefficients (MUC)
#### A. When does MUC work?
  MUC works **only** when:
  1. The ODE has **constant coefficients** $a, b, c$.
  2. The forcing function $g(t)$ is a function whose derivatives cycle within a finite family of similar functions:
   * **Polynomials:** $P_n(t) = a_n t^n + \dots + a_0$
   * **Exponentials:** $e^{\alpha t}$
   * **Sines / Cosines:** $\cos(\beta t), \sin(\beta t)$
   * **Products and sums** of these three types (e.g., $t^2 e^{3t}\cos(4t)$).
  
  * **When does MUC fail?** If $g(t) = \tan(t), \sec(t), \ln(t), \frac{1}{t}$, its derivatives do not terminate or repeat. For these, you must use **Variation of Parameters (Section 4.6)**.
  
  ---
#### B. Master Table of Trial Guesses for $y_p(t)$
  
  | Forcing Term $g(t)$ | Standard Initial Trial Guess $y_p(t)$ | Notes / Common Traps |
  |---|---|---|
  | Constant $k$ | $A$ | |
  | Polynomial $a_n t^n + \dots + a_0$ | $A_n t^n + A_{n-1} t^{n-1} + \dots + A_1 t + A_0$ | Must include **all** lower powers down to the constant term $A_0$! Even if $g(t) = 4t^2 - 1$, guess $At^2 + Bt + C$. |
  | Exponential $k e^{\alpha t}$ | $A e^{\alpha t}$ | Keep the exact same exponent $\alpha$. |
  | Sinusoid $k\cos(\beta t)$ or $k\sin(\beta t)$ | $A\cos(\beta t) + B\sin(\beta t)$ | **Must include both sine and cosine** because the first derivative $y'$ swaps sine and cosine! |
  | Product $P_n(t) e^{\alpha t}$ | $(A_n t^n + \dots + A_0) e^{\alpha t}$ | |
  | Product $e^{\alpha t}\cos(\beta t)$ or $e^{\alpha t}\sin(\beta t)$ | $e^{\alpha t}\left(A\cos(\beta t) + B\sin(\beta t)\right)$ | |
  | Product $P_n(t) e^{\alpha t}\cos(\beta t)$ | $(A_n t^n + \dots)e^{\alpha t}\cos(\beta t) + (B_n t^n + \dots)e^{\alpha t}\sin(\beta t)$ | Requires two full polynomial coefficient sets. |
  
  ---
### 5. The Duplication / Overlap Rule (Multiplication by $t^s$)
#### Why does a naive guess sometimes fail?
  If any term in your trial guess $y_p(t)$ is **already a solution to the homogeneous equation** ($y_p \in y_h$), plugging that guess into the left-hand side will yield:
  $$a y_p'' + b y_p' + c y_p = 0$$
  You get $0 = g(t)$, which makes it impossible to solve for the coefficients.
#### **The Multiplier Rule:**
  If any term in your initial trial guess for $y_p(t)$ duplicates a term in $y_h(t)$, **multiply the entire guess by $t^s$**, where $s$ is the smallest positive integer ($s = 1$ or $s = 2$) that eliminates all duplication:
  * **$s = 1$:** If $\alpha$ (or $\alpha \pm i\beta$) is a **single (unrepeated) root** of the characteristic equation.
  * **$s = 2$:** If $\alpha$ is a **repeated real root** of the characteristic equation (since both $e^{\alpha t}$ and $t e^{\alpha t}$ are already in $y_h$).
  
  ---
### 6. Case-by-Case Detailed Examples
  
  ---
#### **Series A: The Unified Case Studies from Your Handwritten Notes**
  All of these examples share the same homogeneous operator:
  $$y'' + y' - 2y = g(t)$$
  
  * **Step 1 for all:** Solve the homogeneous equation:
  $$r^2 + r - 2 = 0 \implies (r + 2)(r - 1) = 0 \implies r_1 = -2, \; r_2 = 1$$
  $$\mathbf{y_h(t) = c_1 e^{-2t} + c_2 e^t}$$
  
  ---
#### **Case 1: Simple Exponential (Handwritten Notes, Ex 1)**
  $$y'' + y' - 2y = 2e^{-3t}$$
  
  * **Step 2 (Formulate Guess):** $g(t) = 2e^{-3t}$. Since $r = -3$ is **not** a root of the characteristic equation ($r = -2, 1$), there is no duplication.
  $$\text{Guess: } y_p(t) = A e^{-3t}$$
  * **Compute derivatives:**
  $$y_p' = -3A e^{-3t}, \qquad y_p'' = 9A e^{-3t}$$
  * **Substitute into the ODE:**
  $$(9A e^{-3t}) + (-3A e^{-3t}) - 2(A e^{-3t}) = 2e^{-3t}$$
  $$(9 - 3 - 2)A e^{-3t} = 2e^{-3t} \implies 4A e^{-3t} = 2e^{-3t}$$
  $$4A = 2 \implies A = \frac{1}{2}$$
  $$y_p(t) = \frac{1}{2}e^{-3t}$$
  * **Step 3 (General Solution):**
  $$y(t) = c_1 e^{-2t} + c_2 e^t + \frac{1}{2}e^{-3t}$$
  
  ---
#### **Case 2: Pure Cosine (Handwritten Notes, Ex 2)**
  $$y'' + y' - 2y = 3\cos(2t)$$
  
  * **Step 2 (Formulate Guess):** Because $y'$ converts $\cos$ to $\sin$, guessing $A\cos(2t)$ alone fails. We must include both:
  $$y_p(t) = A\cos(2t) + B\sin(2t)$$
  *(Roots $\pm 2i$ do not match $-2, 1$, so no overlap).*
  * **Compute derivatives:**
  $$y_p' = -2A\sin(2t) + 2B\cos(2t)$$
  $$y_p'' = -4A\cos(2t) - 4B\sin(2t)$$
  * **Substitute and organize vertically by term:**
  $$\begin{aligned}
  y_p'' &= -4A\cos(2t) - 4B\sin(2t) \\
  + y_p' &= +2B\cos(2t) - 2A\sin(2t) \\
  - 2y_p &= -2A\cos(2t) - 2B\sin(2t) \\
    \hline
    \text{Sum} &= (-6A + 2B)\cos(2t) + (-2A - 6B)\sin(2t) = 3\cos(2t) + 0\sin(2t)
    \end{aligned}$$
    * **Match coefficients:**
    $$\begin{cases} -6A + 2B = 3 \\ -2A - 6B = 0 \implies A = -3B \end{cases}$$
    Substitute $A = -3B$ into the first equation:
    $$-6(-3B) + 2B = 3 \implies 18B + 2B = 3 \implies 20B = 3 \implies B = \frac{3}{20}$$
    $$A = -3\left(\frac{3}{20}\right) = -\frac{9}{20}$$
    *(Note: In the handwritten note margin, a quick arithmetic slip occurred; the clean matrix reduction yields $A = -\frac{9}{20}, B = \frac{3}{20}$).*
    * **General Solution:**
    $$y(t) = c_1 e^{-2t} + c_2 e^t - \frac{9}{20}\cos(2t) + \frac{3}{20}\sin(2t)$$
    
    ---
#### **Case 3: Duplication / Overlap (Handwritten Notes, Ex 3)**
  $$y'' + y' - 2y = e^t$$
  
  * **Step 2 (Formulate Guess):** 
  * Initial naive guess: $y_p = A e^t$.
  * **Conflict:** $e^t$ is already present in $y_h(t) = c_1 e^{-2t} + c_2 e^t$ (since $r = 1$ is a root of the characteristic equation).
  * If you plug in $A e^t$, you get $0 = e^t$, which is impossible.
  * **Apply the Multiplier Rule ($s = 1$):**
    $$y_p(t) = A t e^t$$
  * **Compute derivatives via Product Rule:**
  $$y_p' = A e^t + A t e^t$$
  $$y_p'' = A e^t + (A e^t + A t e^t) = 2A e^t + A t e^t$$
  * **Substitute into the ODE:**
  $$\underbrace{(2A e^t + A t e^t)}_{y_p''} + \underbrace{(A e^t + A t e^t)}_{y_p'} - 2\underbrace{(A t e^t)}_{y_p} = e^t$$
  Group terms:
  $$(A + A - 2A) t e^t + (2A + A) e^t = e^t$$
  $$0 \cdot t e^t + 3A e^t = e^t \implies 3A = 1 \implies A = \frac{1}{3}$$
  $$y_p(t) = \frac{1}{3}t e^t$$
  * **Step 3 (General Solution):**
  $$y(t) = c_1 e^{-2t} + c_2 e^t + \frac{1}{3}t e^t$$
  
  ---
#### **Case 4: Polynomial Forcing (Handwritten Notes, Ex 4)**
  $$y'' + y' - 2y = t^2 + 1$$
  
  * **Step 2 (Formulate Guess):** $g(t)$ is a degree 2 polynomial. Even though the linear term $t$ is missing from $g(t)$, the derivative $y_p'$ produces linear terms. Thus, you must assume a **complete** second-degree polynomial:
  $$y_p(t) = A t^2 + B t + C$$
  * **Compute derivatives:**
  $$y_p' = 2At + B, \qquad y_p'' = 2A$$
  * **Substitute:**
  $$(2A) + (2At + B) - 2(At^2 + Bt + C) = t^2 + 1$$
  Group by powers of $t$:
  $$(-2A)t^2 + (2A - 2B)t + (2A + B - 2C) = 1\cdot t^2 + 0\cdot t + 1$$
  * **Match coefficients:**
  1. $t^2 \text{ term:} \quad -2A = 1 \implies A = -\frac{1}{2}$
  2. $t^1 \text{ term:} \quad 2A - 2B = 0 \implies B = A = -\frac{1}{2}$
  3. Constant term:
     $$2A + B - 2C = 1 \implies 2\left(-\frac{1}{2}\right) + \left(-\frac{1}{2}\right) - 2C = 1$$
     $$-1 - \frac{1}{2} - 2C = 1 \implies -2C = \frac{5}{2} \implies C = -\frac{5}{4}$$
  * **General Solution:**
  $$y(t) = c_1 e^{-2t} + c_2 e^t - \frac{1}{2}t^2 - \frac{1}{2}t - \frac{5}{4}$$
  
  ---
#### **Case 5: Combined Forcing via Superposition (Handwritten Notes, Ex 5)**
  $$y'' + y' - 2y = 2e^{-3t} + 3\cos(2t)$$
  
  By superposition:
  $$y_p(t) = y_{p1}(t) + y_{p2}(t) = \frac{1}{2}e^{-3t} - \frac{9}{20}\cos(2t) + \frac{3}{20}\sin(2t)$$
  $$y(t) = c_1 e^{-2t} + c_2 e^t + \frac{1}{2}e^{-3t} - \frac{9}{20}\cos(2t) + \frac{3}{20}\sin(2t)$$
  
  ---
#### **Series B: High-Yield Exam Problems from the Slide Archive**
  
  ---
#### **Example 7 (Fall 2013 Final Exam #2 — Double Root Overlap, Slide 311):**
  Solve the IVP:
  $$y'' + 2y' + y = e^{-t}, \quad y(0) = 1, \quad y'(0) = -2$$
  
  1. **Homogeneous Solution:**
   $$r^2 + 2r + 1 = 0 \implies (r + 1)^2 = 0 \implies r = -1 \text{ (repeated root!)}$$
   $$y_h(t) = c_1 e^{-t} + c_2 t e^{-t}$$
  2. **Formulate Particular Guess:**
   * $g(t) = e^{-t}$.
   * Multiplying by $t$ gives $t e^{-t}$, which **still duplicates** $y_h$!
   * We must multiply by $t^2$ ($s = 2$):
     $$\mathbf{y_p(t) = A t^2 e^{-t}}$$
  3. **Compute derivatives:**
   $$y_p' = 2At e^{-t} - At^2 e^{-t}$$
   $$y_p'' = 2A e^{-t} - 4At e^{-t} + At^2 e^{-t}$$
  4. **Substitute into $y'' + 2y' + y = e^{-t}$:**
   $$\left(2Ae^{-t} - 4Ate^{-t} + At^2e^{-t}\right) + 2\left(2Ate^{-t} - At^2e^{-t}\right) + \left(At^2e^{-t}\right) = e^{-t}$$
   Notice that all $t^2 e^{-t}$ and $t e^{-t}$ terms cancel out identically:
   $$2A e^{-t} = e^{-t} \implies 2A = 1 \implies A = \frac{1}{2}$$
   $$y_p(t) = \frac{1}{2}t^2 e^{-t}$$
  5. **General Solution:**
   $$y(t) = c_1 e^{-t} + c_2 t e^{-t} + \frac{1}{2}t^2 e^{-t}$$
  6. **Apply Initial Conditions ($y(0) = 1, y'(0) = -2$):**
   * $y(0) = c_1 = 1$
   * Differentiate the full $y(t)$:
     $$y'(t) = -c_1 e^{-t} + c_2(e^{-t} - t e^{-t}) + \frac{1}{2}(2t e^{-t} - t^2 e^{-t})$$
     $$y'(0) = -c_1 + c_2 = -2 \implies -1 + c_2 = -2 \implies c_2 = -1$$
  7. **Final Unique Solution:**
   $$y(t) = e^{-t} - t e^{-t} + \frac{1}{2}t^2 e^{-t}$$
  
  ---
### Section 4.4 & 4.5 Summary & Exam Checklist
  * [x] **Always find $y_h$ first:** You cannot formulate an accurate guess for $y_p$ without knowing whether your guess overlaps with the roots of $y_h$.
  * [x] **Complete Polynomials:** If $g(t) = t^3$, your guess must be $At^3 + Bt^2 + Ct + D$.
  * [x] **Sines and Cosines go together:** If $g(t) = \sin(3t)$, guess $A\cos(3t) + B\sin(3t)$.
  * [x] **The $t^s$ Rule:**
  * If your guess matches a term in $y_h$ and that root is single $\implies$ multiply by $t$.
  * If that root is repeated ($r_1 = r_2$) $\implies$ multiply by $t^2$.
  * [x] **Initial Conditions:** Only plug in $t_0$ after writing down $y(t) = y_h(t) + y_p(t)$.
  
  ---