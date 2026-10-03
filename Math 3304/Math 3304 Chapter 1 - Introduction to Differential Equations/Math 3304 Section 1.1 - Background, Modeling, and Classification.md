### 1. Motivation: What is a Differential Equation?
#### A. Conceptual Foundation
  * **Geometrically:** A derivative $\frac{dy}{dx}$ represents the **slope of the tangent line** to a curve at any given point.
  * **Physically:** A derivative represents an **instantaneous rate of change** (e.g., velocity $v = \frac{ds}{dt}$, acceleration $a = \frac{dv}{dt}$, rate of cooling $\frac{dT}{dt}$, population growth rate $\frac{dp}{dt}$).
  * **Real-World Laws:** Fundamental physical, biological, and economic principles are typically formulated in terms of *rates of change* (Newton’s Laws, conservation laws, mass-action kinetics).
#### B. Definition: Differential Equation (DE)
  > **Differential Equation:** An equation involving an unknown function and one or more of its derivatives.
  
  ---
### 2. Mathematical Modeling: Translating the Real World into DEs
  
  A mathematical model uses equations to approximate a physical phenomenon.
#### **General 3-Step Modeling Procedure:**
  1. **Identify the governing law/principle** (e.g., Newton’s Second Law: $F_{\text{net}} = ma$; Newton’s Law of Cooling; conservation of mass).
  2. **Identify the variables and parameters:**
   * **Independent variable:** The variable acting on its own (frequently time $t$ or position $x$).
   * **Dependent variable:** The unknown state/quantity we wish to solve for (e.g., velocity $v(t)$, temperature $T(t)$).
   * **Parameters/Constants:** Physical constants (e.g., mass $m$, drag coefficient $\gamma$, gravitational acceleration $g$).
  3. **Translate rates and relations into calculus:** Replace physical rates with derivatives and set up the equation.
  
  ---
### 3. Canonical Modeling Examples
#### **Example A: A Falling Object with Air Drag**
  * **Governing Principle:** Newton’s Second Law:
  $$F_{\text{net}} = m a = m \frac{dv}{dt}$$
  * **Forces Acting on the Body:**
  * Choosing **downward** as the positive direction:
    * Gravity acts downwards: $F_g = +mg$
    * Drag/damping resists motion (acts upwards): $F_d = -\gamma v$ (where $\gamma > 0$ is the drag coefficient).
  * **Differential Equation:**
  $$m \frac{dv}{dt} = mg - \gamma v \implies \frac{dv}{dt} = g - \frac{\gamma}{m}v$$
  * *With concrete parameters ($m = 10\text{ kg}$, $\gamma = 2\text{ kg/s}$, $g = 9.8\text{ m/s}^2$):*
  $$\frac{dv}{dt} = 9.8 - 0.2v$$
  
  ---
#### **Example B: Newton’s Law of Cooling (from your handwritten notes)**
  * **Governing Principle:** The rate at which an object's temperature changes is directly proportional to the difference between its temperature $T(t)$ and the ambient temperature $T_m(t)$:
  $$\underbrace{T'(t)}_{\text{rate of change}} = k \left( T(t) - T_m(t) \right)$$
  * **Physical Insight into the sign of $k$:**
  * If coffee is hotter than the room ($T > T_m$), it cools down ($T' < 0$).
  * Therefore, the proportionality constant must be negative ($k < 0$), often written as $-\frac{1}{10}$ or $-k_0$ where $k_0 > 0$.
  * For room temperature $T_m = 70^\circ\text{F}$ and $k = -0.1$:
    $$\frac{dT}{dt} = -\frac{1}{10}(T - 70)$$
  
  ---
### 4. Classification of Differential Equations
  
  Differential equations are classified by **Type**, **Order**, **Linearity**, and **Homogeneity**. Mastering these distinctions is vital for knowing which analytical or numerical method will solve a given equation.
  
  ---
#### **Classification 1: Type (ODE vs. PDE)**
  
  1. **Ordinary Differential Equation (ODE):**
   * Contains derivatives with respect to a **single independent variable**.
   * Uses ordinary derivative notation: $\frac{dy}{dt}$, $y'$, $\dot{y}$.
   * *Example:* $\frac{d^2y}{dx^2} + 4y = 0$ (independent variable: $x$; dependent variable: $y$).
  
  2. **Partial Differential Equation (PDE):**
   * Involves partial derivatives with respect to **two or more independent variables**.
   * Uses partial notation: $\frac{\partial u}{\partial t}$, $\frac{\partial^2 u}{\partial x^2}$.
   * *Example (Heat Equation):* $\frac{\partial u}{\partial t} = \alpha \frac{\partial^2 u}{\partial x^2}$ (independent variables: $t, x$; dependent variable: $u$).
  
  3. **System of Differential Equations:**
   * Involves two or more differential equations containing two or more unknown functions.
  
  ---
#### **Classification 2: Order**
  > **Order:** The order of a differential equation is the order of the **highest derivative** appearing anywhere in the equation.
  
  * **Crucial distinction:** Do **not** confuse the order of a derivative with an algebraic exponent/power!
  * $\left(\frac{dy}{dx}\right)^4$: First-order derivative raised to the 4th power $\implies$ **Order 1**.
  * $\frac{d^3y}{dx^3}$: Third derivative $\implies$ **Order 3**.
  
  ---
#### **Classification 3: Linearity (Linear vs. Nonlinear)**
  
  An $n$-th order ODE is **linear** if it can be written in the form:
  $$a_n(t)y^{(n)} + a_{n-1}(t)y^{(n-1)} + \dots + a_1(t)y' + a_0(t)y = g(t)$$
##### **The 3 Golden Rules of Linearity:**
  1. The dependent variable $y$ and all its derivatives ($y', y'', \dots, y^{(n)}$) must appear **only to the first power** (degree 1).
  2. **No products** of the dependent variable and/or its derivatives (e.g., $y \cdot y'$, $(y')^2$, $y \cdot y''$ are strictly nonlinear).
  3. **No nonlinear functions of the dependent variable or its derivatives** (e.g., $\sin(y)$, $e^{y'}$, $\ln(y)$, $\sqrt{y'}$).
  
  > **Important Observation:** Coefficients $a_i(t)$ and the driving term $g(t)$ can be completely nonlinear functions of the **independent** variable $t$ (e.g., $\cos^3(t)y$ is completely linear in $y$). Linearity is tested **strictly with respect to the dependent variable and its derivatives**.
  
  ---
#### **Classification 4: Homogeneity (Homogeneous vs. Non-homogeneous)**
  
  Once all terms involving the dependent variable $y$ and its derivatives are moved to one side:
  $$F(t, y, y', \dots, y^{(n)}) = g(t)$$
  * **Homogeneous:** If $g(t) = 0$ (every single nonzero term contains $y$ or at least one derivative of $y$).
  * **Non-homogeneous:** If $g(t) \neq 0$ (there is at least one isolated driving term consisting solely of constants or the independent variable $t$).
  
  ---
### 5. Exam & Practice Examples from the Slides
  
  | # | Differential Equation | Dependent Var. | Indep. Var. | Order | Linear? | Homogeneous? | Reason for Nonlinearity / Non-homogeneity |
  |---|---|:---:|:---:|:---:|:---:|:---:|---|
  | **1** | $y''' + ty' + \cos^3(t)y - t^3 = 0$ | $y$ | $t$ | **3** | **Linear** | **Non-homogeneous** | $g(t) = t^3 \neq 0$. Linear because $y''', y', y$ are degree 1. |
  | **2** | $x'' + t\ln(x)x = 0$ | $x$ | $t$ | **2** | **Nonlinear** | **Homogeneous** | Nonlinear due to $\ln(x)$. Every term has $x$. |
  | **3** | $\frac{d^3y}{dx^3} + \left(\frac{dy}{dx}\right)^4 + y = 0$ | $y$ | $x$ | **3** | **Nonlinear** | **Homogeneous** | Derivative $\frac{dy}{dx}$ is raised to the 4th power. |
  | **4** | $y'' + 10y - \delta(t-2) = 0$ | $y$ | $t$ | **2** | **Linear** | **Non-homogeneous** | $g(t) = \delta(t-2) \neq 0$ (Dirac delta). $y'', y$ are linear. |
  | **5** | $\frac{dp}{dt} + p(p-1) = 0$ | $p$ | $t$ | **1** | **Nonlinear** | **Homogeneous** | $p(p-1) = p^2 - p$; presence of $p^2$ makes it nonlinear. |
  | **6** | $(1-x)y'' - 4xy' + 5y = \cos(x)$ | $y$ | $x$ | **2** | **Linear** | **Non-homogeneous** | $g(x) = \cos(x) \neq 0$. Coefficients only depend on $x$. |
  | **7** | $\ln(x)\frac{d^3y}{dx^3} - \left(\frac{dy}{dx}\right)^4 + y = 0$ | $y$ | $x$ | **3** | **Nonlinear** | **Homogeneous** | Power 4 on $(y')^4$. Note: $\ln(x)$ is fine because $x$ is indep. |
  | **8** | $\sin(t)y'' - \cos(t)y' - y = 0$ | $y$ | $t$ | **2** | **Linear** | **Homogeneous** | Every term has $y$ or its derivatives with linear powers. |
  | **9** | $\frac{d^2y}{dx^2} = \sqrt{\frac{dy}{dx}}$ | $y$ | $x$ | **2** | **Nonlinear** | **Homogeneous** | Derivative inside square root: $(y')^{1/2}$. |
  | **10** | $y''' + ty + \cos(y)y = 0$ | $y$ | $t$ | **3** | **Nonlinear** | **Homogeneous** | Transcendental function $\cos(y)$ applied to dependent var. |
  | **11** | $(1+y)y'' + ty' + y = e^t$ | $y$ | $t$ | **2** | **Nonlinear** | **Non-homogeneous** | Product of dependent variable and derivative: $y \cdot y''$. |
  | **12** | $x' + t\ln(t)x = e^{-t}$ | $x$ | $t$ | **1** | **Linear** | **Non-homogeneous** | $t\ln(t)$ is purely a function of indep. var. $t$. |
  | **13** | $x' + t\ln(x)x = e^{-t}$ | $x$ | $t$ | **1** | **Nonlinear** | **Non-homogeneous** | Contains $\ln(x)$, where $x$ is the dependent variable. |
  | **14** | $\left(\frac{d^2y}{dx^2}\right)^3 + \frac{dy}{dx} + y = 0$ | $y$ | $x$ | **2** | **Nonlinear** | **Homogeneous** | Highest derivative is order 2, but raised to power 3. |
  | **15** | $y''' + ty + \cos^2(t)y = 0$ | $y$ | $t$ | **3** | **Linear** | **Homogeneous** | $\cos^2(t)$ is a valid coefficient function of $t$. |
  
  ---
### Quick Self-Check / Summary Checklist for Section 1.1
  * [x] Did you check what the **independent variable** is before deciding linearity? (e.g., $t\ln(t)x$ is linear, but $t\ln(x)x$ is nonlinear).
  * [x] Did you distinguish the **order of the derivative** from the **power of the expression**?
  * [x] Are all $y, y', y'', \dots$ terms on the left when checking whether $g(t) = 0$ for homogeneity?
  
  ---