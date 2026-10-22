### 1. The Standard Form of an $n$-th Order Linear Equation
  
  An $n$-th order linear differential equation involves derivatives of the unknown function $y(t)$ up to order $n$, where each derivative appears strictly to the first power with no products of $y$ or its derivatives:
  
  $$a_n(t)\frac{d^n y}{dt^n} + a_{n-1}(t)\frac{d^{n-1}y}{dt^{n-1}} + \dots + a_1(t)\frac{dy}{dt} + a_0(t)y = f(t)$$
  
  Just as in second-order equations, the **mandatory first step** is to divide through by the leading coefficient $a_n(t)$ to put the equation into **standard form**:
  
  $$\mathbf{y^{(n)} + p_{n-1}(t)y^{(n-1)} + \dots + p_1(t)y' + p_0(t)y = g(t)} \quad (\ast)$$
  
  where $p_i(t) = \frac{a_i(t)}{a_n(t)}$ and $g(t) = \frac{f(t)}{a_n(t)}$.
  * If $g(t) = 0$, the equation is **homogeneous**.
  * If $g(t) \neq 0$, the equation is **nonhomogeneous (forced)**.
  
  ---
### 2. The Existence and Uniqueness Theorem for $n$-th Order Linear IVPs
  
  For an $n$-th order differential equation, an **Initial Value Problem (IVP)** requires prescribing the function value and all derivatives up to order $n - 1$ at a **single point $t_0$**:
  
  $$\begin{cases}
  y^{(n)} + p_{n-1}(t)y^{(n-1)} + \dots + p_0(t)y = g(t) \\
  y(t_0) = y_0, \quad y'(t_0) = y_1, \quad y''(t_0) = y_2, \quad \dots, \quad y^{(n-1)}(t_0) = y_{n-1}
  \end{cases}$$
  
  > **Theorem:** If the coefficient functions $p_{n-1}(t), \dots, p_0(t)$ and the forcing term $g(t)$ are **continuous** on an open interval $I = (a, b)$ that contains the point $t_0$, then there exists **one and only one** solution $y = \phi(t)$ to the initial value problem, and that solution exists on the **entire interval $I$**.
  
  ---
#### **Canonical Exam Example: Finding the Interval of Validity (Notes PDF 2, Page 3)**
  *Problem Statement:* Determine the largest interval $I$ for which the IVP is guaranteed to have a unique solution:
  $$t y''' + \sin(t) y'' + (t^2 - 1)y' + 3y = \tan(t), \quad y(1) = 0, \; y'(1) = 5, \; y''(1) = -1$$
##### **Step 1: Put in Standard Form (Divide by the leading coefficient $t$)**
  $$y''' + \left(\frac{\sin(t)}{t}\right)y'' + \left(\frac{t^2 - 1}{t}\right)y' + \left(\frac{3}{t}\right)y = \frac{\tan(t)}{t}$$
##### **Step 2: Identify Discontinuities of Each Term**
  * **Coefficients on the left:**
  * $p_2(t) = \frac{\sin(t)}{t}$: undefined at $t = 0$.
  * $p_1(t) = \frac{t^2 - 1}{t}$: undefined at $t = 0$.
  * $p_0(t) = \frac{3}{t}$: undefined at $t = 0$.
  *(All left-hand coefficients are continuous everywhere except $t = 0$).*
  * **Forcing term on the right:**
  * $g(t) = \frac{\tan(t)}{t} = \frac{\sin(t)}{t \cos(t)}$:
    * Discontinuous when $t = 0$.
    * Discontinuous wherever $\cos(t) = 0$, which occurs at all **odd multiples of $\frac{\pi}{2}$**:
      $$t = \dots, \; -\frac{3\pi}{2}, \; -\frac{\pi}{2}, \; \frac{\pi}{2}, \; \frac{3\pi}{2}, \dots$$
##### **Step 3: Plot Discontinuities on a Real Number Line**
  ```
  ---|---------*---------*---------*---------*---------|---> t
   -3pi/2    -pi/2       0        pi/2     3pi/2
                                    ^
                           Interval: (0, pi/2) contains t0 = 1!
  ```
  The open intervals of mutual continuity are:
  $$\dots, \; \left(-\frac{3\pi}{2}, -\frac{\pi}{2}\right), \; \left(-\frac{\pi}{2}, 0\right), \; \mathbf{\left(0, \frac{\pi}{2}\right)}, \; \left(\frac{\pi}{2}, \frac{3\pi}{2}\right), \dots$$
##### **Step 4: Locate the Initial Time $t_0$**
  The initial conditions are specified at $t_0 = 1$.
  * Notice that $\frac{\pi}{2} \approx \frac{3.14159}{2} \approx 1.571$.
  * Since $0 < 1 < 1.571$, the initial point $t_0 = 1$ falls cleanly inside the interval $\left(0, \frac{\pi}{2}\right)$.
  
  **Final Conclusion:** The theorem guarantees a unique solution on:
  $$\mathbf{I = \left(0, \frac{\pi}{2}\right)}$$
  
  ---
### 3. Linear Independence for $n$ Functions
#### A. Formal Definition
  A set of functions $\{f_1(t), f_2(t), \dots, f_n(t)\}$ is **linearly dependent** on an interval $I$ if there exist constants $c_1, c_2, \dots, c_n$, **not all zero**, such that:
  $$c_1 f_1(t) + c_2 f_2(t) + \dots + c_n f_n(t) = 0 \quad \text{for all } t \in I$$
  
  If the **only** constants that satisfy this identity are:
  $$c_1 = c_2 = \dots = c_n = 0$$
  then the functions are **linearly independent** on $I$.
  
  ---
#### ⚠️ **THE CRITICAL HIGHER-ORDER EXAM TRAP:**
  * For **two** functions, you can check linear independence simply by taking their ratio: if $\frac{f_2(t)}{f_1(t)} = \text{constant}$, they are dependent; otherwise, they are independent.
  * For **three or more functions ($n \ge 3$), checking ratios DOES NOT WORK!**
  * *Example:* $f_1(t) = 1, \; f_2(t) = t, \; f_3(t) = 2 + 3t$.
  * No single function is a simple constant multiple of another ($\frac{t}{1} \neq \text{const}$, $\frac{2+3t}{t} \neq \text{const}$).
  * But notice:
    $$(2)f_1 + (3)f_2 - (1)f_3 = 2(1) + 3(t) - (2 + 3t) = 0$$
  * Because non-zero constants ($c_1 = 2, c_2 = 3, c_3 = -1$) produce zero, these three functions are **linearly dependent**!
  
  ---
#### **Worked Example: Testing Linear Independence Directly (Notes PDF 3, Page 3)**
  *Determine whether $f_1(t) = t$ and $f_2(t) = t^2$ are linearly independent.*
  
  1. Set a linear combination equal to zero for all $t$:
   $$c_1 t + c_2 t^2 = 0$$
  2. Since this equation must hold for **all** $t$, evaluate it at two convenient nonzero points:
   * Let $t = 1$:
     $$c_1(1) + c_2(1)^2 = 0 \implies c_1 + c_2 = 0 \quad \text{(Equation 1)}$$
   * Let $t = -1$:
     $$c_1(-1) + c_2(-1)^2 = 0 \implies -c_1 + c_2 = 0 \quad \text{(Equation 2)}$$
  3. Add the two equations:
   $$(c_1 + c_2) + (-c_1 + c_2) = 0 \implies 2c_2 = 0 \implies \mathbf{c_2 = 0}$$
  4. Substitute $c_2 = 0$ into Equation 1:
   $$c_1 + 0 = 0 \implies \mathbf{c_1 = 0}$$
  5. Since $c_1 = 0$ and $c_2 = 0$ is the **only** solution, $f_1(t) = t$ and $f_2(t) = t^2$ are **linearly independent**.
  
  ---
### 4. The $n \times n$ Wronskian Determinant
  
  For $n$ differentiable functions $\{y_1, y_2, \dots, y_n\}$, their **Wronskian** is the determinant of an $n \times n$ matrix whose rows are formed by successive derivatives up to order $n - 1$:
  
  $$W(y_1, y_2, \dots, y_n)(t) = \begin{vmatrix}
  y_1(t) & y_2(t) & \dots & y_n(t) \\
  y_1'(t) & y_2'(t) & \dots & y_n'(t) \\
  y_1''(t) & y_2''(t) & \dots & y_n''(t) \\
  \vdots & \vdots & \ddots & \vdots \\
  y_1^{(n-1)}(t) & y_2^{(n-1)}(t) & \dots & y_n^{(n-1)}(t)
  \end{vmatrix}$$
  
  ---
#### **Calculating a $3 \times 3$ Determinant via Cofactor Expansion (Notes PDF 3, Page 2)**
  For a $3 \times 3$ matrix:
  $$\det(A) = \begin{vmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{vmatrix} = a_{11}\begin{vmatrix} a_{22} & a_{23} \\ a_{32} & a_{33} \end{vmatrix} - a_{12}\begin{vmatrix} a_{21} & a_{23} \\ a_{31} & a_{33} \end{vmatrix} + a_{13}\begin{vmatrix} a_{21} & a_{22} \\ a_{31} & a_{32} \end{vmatrix}$$
  *(Remember the alternating signs: $+ \; - \; +$ across row 1).*
#### **Numerical Practice Problem from Notes:**
  Evaluate:
  $$\det \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 10 \end{pmatrix}$$
  * Expand along the first row:
  $$= 1 \begin{vmatrix} 5 & 6 \\ 8 & 10 \end{vmatrix} - 2 \begin{vmatrix} 4 & 6 \\ 7 & 10 \end{vmatrix} + 3 \begin{vmatrix} 4 & 5 \\ 7 & 8 \end{vmatrix}$$
  * Compute each $2 \times 2$ determinant ($ad - bc$):
  $$= 1(5 \cdot 10 - 6 \cdot 8) - 2(4 \cdot 10 - 6 \cdot 7) + 3(4 \cdot 8 - 5 \cdot 7)$$
  $$= 1(50 - 48) - 2(40 - 42) + 3(32 - 35)$$
  $$= 1(2) - 2(-2) + 3(-3)$$
  $$= 2 + 4 - 9 = \mathbf{-3}$$
  
  ---
### 5. Fundamental Set of Solutions (FSS) & The General Solution
  
  > **Fundamental Theorem for $n$-th Order Homogeneous Linear ODEs:**
  > Let $y_1(t), y_2(t), \dots, y_n(t)$ be $n$ solutions to the homogeneous equation:
  > $$y^{(n)} + p_{n-1}(t)y^{(n-1)} + \dots + p_0(t)y = 0$$
  > on an interval $I$. The following statements are **equivalent**:
  > 1. $\{y_1, y_2, \dots, y_n\}$ is **linearly independent** on $I$.
  > 2. The Wronskian $W(y_1, y_2, \dots, y_n)(t_0) \neq 0$ at some point $t_0 \in I$.
  > 3. $\{y_1, y_2, \dots, y_n\}$ forms a **Fundamental Set of Solutions (FSS)**.
  > 
  > When this holds, the **General Solution** is:
  > $$\mathbf{y_h(t) = c_1 y_1(t) + c_2 y_2(t) + \dots + c_n y_n(t)}$$
  > where $c_1, c_2, \dots, c_n$ are arbitrary real constants.
  
  ---
### 6. The Nonhomogeneous (Forced) Structure Theorem
  
  Just as with second-order equations:
  
  $$\mathbf{y(t) = y_h(t) + y_p(t)}$$
  
  * **$y_h(t) = \sum_{i=1}^n c_i y_i(t)$:** Complementary (homogeneous) solution containing $n$ arbitrary constants.
  * **$y_p(t)$:** Any single particular solution to the nonhomogeneous equation $L[y] = g(t)$.
  
  ---
### Section 6.1 Pro-Tips & Exam Traps Checklist
  * [x] **Standard Form First:** Always divide by the coefficient of $y^{(n)}$ before analyzing the domain of validity for an IVP.
  * [x] **Number of Initial Conditions:** An $n$-th order ODE requires exactly **$n$ initial conditions**: from $y(t_0)$ up to $y^{(n-1)}(t_0)$.
  * [x] **Linear Independence for 3+ functions:** Never just test ratios pairwise; use the definition $c_1 f_1 + \dots + c_n f_n = 0$ or the Wronskian determinant.
  * [x] **Alternating Signs in $3 \times 3$ Wronskian:** Remember $+ - +$ along the top row when computing determinants.
  * [x] **Interval Selection:** The solution interval $I$ must be a single continuous interval that contains the initial time $t_0$.
  
  ---