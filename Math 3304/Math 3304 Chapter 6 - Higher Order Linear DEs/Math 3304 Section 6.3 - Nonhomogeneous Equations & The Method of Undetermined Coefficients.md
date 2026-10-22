### 1. The Big Picture: Forced Higher-Order Equations
  
  We now consider the forced (nonhomogeneous) $n$-th order linear equation with **constant real coefficients**:
  
  $$a_n y^{(n)} + a_{n-1}y^{(n-1)} + \dots + a_1 y' + a_0 y = g(t) \quad (g(t) \neq 0)$$
  
  Just as in second-order equations, the general solution is partitioned into two distinct parts:
  
  $$\mathbf{y(t) = y_h(t) + y_p(t)}$$
  
  * **$y_h(t) = \sum_{i=1}^n c_i y_i(t)$:** The complementary solution to the unforced equation $L[y] = 0$ (found using the methods of Section 6.2).
  * **$y_p(t)$:** Any single particular solution satisfying $L[y] = g(t)$.
  
  ---
### 2. Why MUC Over Variation of Parameters for Higher Orders?
  
  *(Notes PDF 1, Page 1)*
  
  While **Variation of Parameters (VofP)** can theoretically solve any $n$-th order nonhomogeneous linear ODE:
  $$y_p(t) = \sum_{i=1}^n u_i(t) y_i(t), \quad \text{where } u_i(t) = \int \frac{g(t) W_i(t)}{W(t)}\,dt$$
  it requires computing an $n \times n$ Wronskian determinant $W(t)$, $n$ separate sub-determinants $W_i(t)$, and evaluating $n$ complicated integrals. 
  
  For $n \ge 3$, VofP is algebraically punishing. Therefore:
  > **Golden Rule for Higher-Order Exams:** If the coefficients are constant and $g(t)$ consists of polynomials, exponentials, sines, cosines, or their sums/products, **ALWAYS use the Method of Undetermined Coefficients (MUC)**.
  
  ---
### 3. The Generalized Multiplier Rule for Higher-Order Equations
  
  The foundational trial guesses for MUC are identical to those in Chapter 4:
  * Exponential $k e^{\alpha t} \implies A e^{\alpha t}$
  * Sinusoids $k\cos(\beta t)$ or $k\sin(\beta t) \implies A\cos(\beta t) + B\sin(\beta t)$
  * Polynomial $P_m(t) \implies A_m t^m + A_{m-1}t^{m-1} + \dots + A_0$
  
  However, for higher-order equations ($n \ge 3$), roots can have algebraic multiplicities of **$s = 1, 2, 3, 4, \dots$**.
#### **The $t^s$ Multiplication Law:**
  If any term in your initial trial guess for $y_p(t)$ duplicates a term already present in the complementary solution $y_h(t)$, you must multiply that entire guess by:
  
  $$\mathbf{t^s}$$
  
  where **$s$ is the smallest positive integer ($s = 1, 2, 3, \dots$) such that no term in $t^s y_p(t)$ duplicates any term in $y_h(t)$**.
  * Equivalently: $s$ is the **multiplicity** of the characteristic root associated with the forcing term.
  
  ---
#### ⚠️ **THE CRITICAL HIGHER-ORDER TRAP: Polynomials and $r = 0$**
  Look at what happens when the characteristic equation has a root at $r = 0$ with multiplicity $s$:
  * The homogeneous solution already contains:
  $$c_1(1) + c_2(t) + c_3(t^2) + \dots + c_s(t^{s-1})$$
  * If the forcing term is a polynomial (which corresponds to $e^{0t}$):
  * **Do not guess $A t + B$!**
  * You must multiply the **entire polynomial** by $t^s$:
    $$\mathbf{y_p(t) = t^s (A_m t^m + \dots + A_1 t + A_0) = A_m t^{m+s} + \dots + A_0 t^s}$$
  * Notice that the lowest power of $t$ in your guess will be **$t^s$**, completely clearing the $t^{s-1}$ term from $y_h$.
  
  ---
### 4. Fully Worked Master Examples from Your Notes
  
  ---
#### **Example 1: Fourth-Order with No Duplication (Notes PDF 1, Page 1–2)**
  Solve:
  $$y'''' - 16y = e^t$$
  
  * **Step 1: Complementary Solution $y_h(t)$ (from Section 6.2):**
  $$r^4 - 16 = 0 \implies (r - 2)(r + 2)(r^2 + 4) = 0 \implies r = 2, -2, \pm 2i$$
  $$y_h(t) = c_1 e^{2t} + c_2 e^{-2t} + c_3 \cos(2t) + c_4 \sin(2t)$$
  
  * **Step 2: Formulate Particular Guess $y_p(t)$:**
  * Forcing term: $g(t) = e^t$ (root is $\alpha = 1$).
  * Check for overlap: $r = 1$ is **not** a root of the characteristic equation (roots are $2, -2, \pm 2i$).
  * Initial trial guess is valid:
    $$\mathbf{y_p(t) = A e^t}$$
  
  * **Step 3: Differentiate and Substitute:**
  $$y_p' = y_p'' = y_p''' = y_p'''' = A e^t$$
  Substitute into $y'''' - 16y = e^t$:
  $$A e^t - 16(A e^t) = e^t \implies -15A e^t = e^t \implies \mathbf{A = -\frac{1}{15}}$$
  $$y_p(t) = -\frac{1}{15}e^t$$
  
  * **Step 4: Full General Solution:**
  $$\mathbf{y(t) = c_1 e^{2t} + c_2 e^{-2t} + c_3 \cos(2t) + c_4 \sin(2t) - \frac{1}{15}e^t}$$
  
  ---
#### **Example 2: Fifth-Order with Triple Zero Root Overlap (Notes PDF 1, Pages 2–3)**
  Solve:
  $$y^{(5)} - 2y^{(4)} + 5y''' = 3t + 6$$
  
  * **Step 1: Complementary Solution $y_h(t)$:**
  $$r^5 - 2r^4 + 5r^3 = 0 \implies r^3(r^2 - 2r + 5) = 0$$
  * $r = 0$ is a root with **multiplicity $s = 3$** ($r = 0, 0, 0$).
  * $r^2 - 2r + 5 = 0 \implies r = 1 \pm 2i$.
  $$y_h(t) = \underbrace{c_1 + c_2 t + c_3 t^2}_{r=0 \text{ part}} + c_4 e^t \cos(2t) + c_5 e^t \sin(2t)$$
  
  * **Step 2: Formulate Particular Guess $y_p(t)$:**
  * Forcing term: $g(t) = 3t + 6$ (a degree-1 polynomial).
  * Let's trace why lower powers fail:
    * Try $y_p = At + B$: Both $At$ and $B$ are in $y_h$ ($c_1 + c_2 t$).
    * Multiply by $t \implies At^2 + Bt$: Both terms are still in $y_h$ ($c_2 t + c_3 t^2$).
    * Multiply by $t^2 \implies At^3 + Bt^2$: $Bt^2$ is still in $y_h$ ($c_3 t^2$).
    * Multiply by $t^3$ (since $r = 0$ has multiplicity $s = 3$):
      $$\mathbf{y_p(t) = t^3 (At + B) = At^4 + Bt^3}$$
      *(Now no term duplicates $y_h$!).*
  
  * **Step 3: Differentiate up to Order 5:**
  $$y_p' = 4At^3 + 3Bt^2$$
  $$y_p'' = 12At^2 + 6Bt$$
  $$y_p''' = 24At + 6B$$
  $$y_p^{(4)} = 24A$$
  $$y_p^{(5)} = 0$$
  
  * **Step 4: Substitute into $y^{(5)} - 2y^{(4)} + 5y''' = 3t + 6$:**
  $$(0) - 2(24A) + 5(24At + 6B) = 3t + 6$$
  $$-48A + 120At + 30B = 3t + 6$$
  $$(120A)t + (30B - 48A) = 3t + 6$$
  
  * **Step 5: Equate Coefficients:**
  1. **$t^1$ term:**
     $$120A = 3 \implies \mathbf{A = \frac{3}{120} = \frac{1}{40}}$$
  2. **Constant term:**
     $$30B - 48A = 6$$
     $$30B = 6 + 48A = 6 + 48\left(\frac{1}{40}\right) = 6 + \frac{6}{5} = \frac{36}{5}$$
     $$\mathbf{B = \frac{36}{5 \cdot 30} = \frac{36}{150} = \frac{6}{25} = 0.24}$$
  
  * **Step 6: Particular and General Solutions:**
  $$y_p(t) = \frac{1}{40}t^4 + \frac{6}{25}t^3$$
  $$\mathbf{y(t) = c_1 + c_2 t + c_3 t^2 + c_4 e^t \cos(2t) + c_5 e^t \sin(2t) + \frac{1}{40}t^4 + \frac{6}{25}t^3}$$
  
  ---
#### **Example 3: Fourth-Order with Polynomial Forcing (Earlier Notes, Page 2)**
  Solve:
  $$y^{(4)} + 5y'' + 4y = 8t^2 + t$$
  
  * **Step 1: Complementary Solution:**
  $$r^4 + 5r^2 + 4 = 0 \implies (r^2 + 1)(r^2 + 4) = 0 \implies r = \pm i, \; \pm 2i$$
  $$y_h(t) = c_1 \cos(t) + c_2 \sin(t) + c_3 \cos(2t) + c_4 \sin(2t)$$
  
  * **Step 2: Formulate Particular Guess:**
  * Forcing term: $g(t) = 8t^2 + t$ (degree 2 polynomial).
  * Root is $\alpha = 0$. Since $r = 0$ is **not** a root of the characteristic equation, there is zero duplication:
    $$\mathbf{y_p(t) = At^2 + Bt + C}$$
  
  * **Step 3: Compute Derivatives:**
  $$y_p' = 2At + B, \quad y_p'' = 2A, \quad y_p''' = 0, \quad y_p^{(4)} = 0$$
  
  * **Step 4: Substitute into $y^{(4)} + 5y'' + 4y = 8t^2 + t$:**
  $$(0) + 5(2A) + 4(At^2 + Bt + C) = 8t^2 + t$$
  $$4At^2 + 4Bt + (10A + 4C) = 8t^2 + 1t + 0$$
  
  * **Step 5: Match Coefficients:**
  * $t^2$ term: $4A = 8 \implies \mathbf{A = 2}$
  * $t^1$ term: $4B = 1 \implies \mathbf{B = \frac{1}{4}}$
  * Constant term: $10A + 4C = 0 \implies 10(2) + 4C = 0 \implies 4C = -20 \implies \mathbf{C = -5}$
  
  * **Step 6: General Solution:**
  $$y_p(t) = 2t^2 + \frac{1}{4}t - 5$$
  $$\mathbf{y(t) = c_1 \cos(t) + c_2 \sin(t) + c_3 \cos(2t) + c_4 \sin(2t) + 2t^2 + \frac{1}{4}t - 5}$$
  
  ---
#### **Example 4: Third-Order with Triple Root Overlap (Earlier Notes, Pages 3–4)**
  Solve:
  $$y''' - 3y'' + 3y' - y = 4e^t$$
  
  * **Step 1: Complementary Solution:**
  $$r^3 - 3r^2 + 3r - 1 = 0 \implies (r - 1)^3 = 0 \implies \mathbf{r = 1 \text{ (multiplicity } s = 3\text{)}}$$
  $$y_h(t) = c_1 e^t + c_2 t e^t + c_3 t^2 e^t$$
  
  * **Step 2: Formulate Particular Guess:**
  * Forcing term: $g(t) = 4e^t$.
  * Base guess is $A e^t$.
  * But $e^t$, $t e^t$, and $t^2 e^t$ are **all in $y_h$**.
  * Multiplicity is $s = 3$. Multiply by $t^3$:
    $$\mathbf{y_p(t) = A t^3 e^t}$$
  
  * **Step 3: Differentiate (factoring $e^t$ as you go):**
  $$y_p' = (3At^2 + At^3)e^t$$
  $$y_p'' = (6At + 6At^2 + At^3)e^t$$
  $$y_p''' = (6A + 18At + 9At^2 + At^3)e^t$$
  
  * **Step 4: Substitute into $y''' - 3y'' + 3y' - y = 4e^t$:**
  $$\begin{aligned}
  y_p''' &= \left(6A + 18At + 9At^2 + At^3\right)e^t \\
  -3y_p'' &= \left(-18At - 18At^2 - 3At^3\right)e^t \\
  +3y_p' &= \left(9At^2 + 3At^3\right)e^t \\
  -y_p &= \left(-At^3\right)e^t \\
  \hline
  \text{Sum} &= \mathbf{6A e^t = 4e^t}
  \end{aligned}$$
  *(All $t^3, t^2, t$ terms cancel out identically!).*
  
  * **Step 5: Solve for $A$:**
  $$6A = 4 \implies \mathbf{A = \frac{4}{6} = \frac{2}{3}}$$
  $$y_p(t) = \frac{2}{3}t^3 e^t$$
  
  * **Step 6: General Solution:**
  $$\mathbf{y(t) = c_1 e^t + c_2 t e^t + c_3 t^2 e^t + \frac{2}{3}t^3 e^t}$$
  
  ---
### Section 6.3 Summary & Exam Checklist
  * [x] **Match the Root Multiplicity ($s$):** If the forcing function's characteristic root $\alpha$ (or $\alpha \pm i\beta$) appears $s$ times in the characteristic equation, multiply the base guess by $t^s$.
  * [x] **Watch for $r = 0$ with Polynomials:** If the ODE has terms ending in $y'''$ or $y''$ (no bare $y$), $r = 0$ is a root! Multiply your polynomial guess by $t^s$.
  * [x] **Cancellation Check:** When substituting $y_p = A t^s e^{\alpha t}$, all intermediate powers ($t^s, t^{s-1}, \dots, t^1$) **must cancel out completely**. Only the leading constant term will remain to balance the forcing function.
  * [x] **Superposition for Sums:** If $g(t) = e^t + 4t^2$, solve for $y_{p1}$ and $y_{p2}$ independently, then add them: $y_p = y_{p1} + y_{p2}$.
  
  ---