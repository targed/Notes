### 1. The Modeling Paradigm: How Models Are Built
  
  The handwritten notes categorize mathematical models into three deterministic approaches:
  1. **Law-Based Models:** Constructed directly from established physical laws (e.g., Newton’s Second Law $\sum F = ma$, conservation of mass, Newton’s Law of Cooling).
  2. **Empirical Models:** Formulated to mimic observed qualitative behaviors or experimental trends when fundamental physical laws are unknown (e.g., population dynamics, economic forecasts).
  3. **Mixed Models:** Combine physical balance laws with empirical rate laws (e.g., fluid mixing tanks with empirically determined reaction rates).
  
  ---
### 2. Compartmental Analysis: Mixing / Tank Problems
  
  Compartmental models track the amount of a substance (salt, pollutant, chemical, drug) flowing in and out of a well-defined container (tank, lake, bloodstream, organ).
  
  ```
   Inflow: Rate = r_in, Concentration = C_in
             |
             v
      +--------------+
      |  Tank: V(t)  | ----> Well-stirred mixture
      |  Salt: Q(t)  |
      +--------------+
             |
             v
   Outflow: Rate = r_out, Concentration = C_out(t) = Q(t) / V(t)
  ```
#### A. The Universal Conservation Law
  $$\frac{dQ}{dt} = \text{Rate In} - \text{Rate Out}$$
  
  To formulate $\text{Rate In}$ and $\text{Rate Out}$ in terms of physical quantities:
  $$\text{Rate of Substance} = \left(\text{Volumetric Fluid Flow Rate}\right) \times \left(\text{Substance Concentration}\right)$$
  
  $$\frac{dQ}{dt} = \left(r_{\text{in}} \cdot C_{\text{in}}\right) - \left(r_{\text{out}} \cdot C_{\text{out}}(t)\right)$$
  
  * **Unit Consistency Check (Always do this!):**
  $$\left[\frac{\text{mass}}{\text{time}}\right] = \left[\frac{\text{volume}}{\text{time}}\right] \times \left[\frac{\text{mass}}{\text{volume}}\right] - \left[\frac{\text{volume}}{\text{time}}\right] \times \left[\frac{\text{mass}}{\text{volume}}\right]$$
  *(e.g., $\frac{\text{kg}}{\text{min}} = \frac{\text{L}}{\text{min}} \cdot \frac{\text{kg}}{\text{L}}$ or $\frac{\text{lb}}{\text{min}} = \frac{\text{gal}}{\text{min}} \cdot \frac{\text{lb}}{\text{gal}}$).*
#### B. The "Well-Stirred" Assumption
  The phrase **"well-stirred mixture"** is a critical mathematical assumption. It means that incoming substance is instantaneously and uniformly distributed throughout the entire tank. Therefore:
  $$C_{\text{out}}(t) = \frac{\text{Total mass of substance in tank at time } t}{\text{Total volume of liquid in tank at time } t} = \frac{Q(t)}{V(t)}$$
  
  ---
### 3. Tracking Tank Volume: Constant vs. Variable Volume
  
  The total volume of liquid $V(t)$ in the tank satisfies its own simple rate equation:
  $$\frac{dV}{dt} = r_{\text{in}} - r_{\text{out}} \implies V(t) = V_0 + (r_{\text{in}} - r_{\text{out}})t$$
  where $V_0$ is the initial volume of liquid.
#### **Case 1: Constant Volume ($r_{\text{in}} = r_{\text{out}} = r$)**
  When liquid enters and leaves at the exact same flow rate:
  $$\frac{dV}{dt} = 0 \implies V(t) = V_0 = \text{constant}$$
  The ODE becomes:
  $$\frac{dQ}{dt} = r C_{\text{in}} - r \frac{Q}{V_0} \iff \frac{dQ}{dt} + \frac{r}{V_0}Q = r C_{\text{in}}$$
  *(This is both **separable** and **linear**).*
  
  ---
#### **Case Study 1: Complete Solution for Constant Volume (Handwritten Notes)**
  A tank holds $1000\text{ L}$ of pure water. Water containing $1\text{ kg/L}$ of salt enters at $6\text{ L/min}$. The well-stirred solution drains out at $6\text{ L/min}$. Find the amount of salt $Q(t)$ for $t \ge 0$.
  
  * **Step 1: Identify Parameters:**
  * $V(t) = 1000\text{ L}$ (since $r_{\text{in}} = r_{\text{out}} = 6$)
  * $C_{\text{in}} = 1\text{ kg/L}$
  * Initial Condition: $Q(0) = 0$ (starts with pure water)
  * **Step 2: Set up the IVP:**
  $$\frac{dQ}{dt} = (6)(1) - (6)\left(\frac{Q}{1000}\right) = 6 - \frac{3}{500}Q, \quad Q(0) = 0$$
  * **Step 3: Solve via Integrating Factor or Separation:**
  $$\frac{dQ}{dt} + \frac{3}{500}Q = 6$$
  $$\mu(t) = e^{\int \frac{3}{500}dt} = e^{\frac{3}{500}t}$$
  $$\frac{d}{dt}\left[e^{\frac{3}{500}t} Q\right] = 6e^{\frac{3}{500}t}$$
  $$e^{\frac{3}{500}t} Q = 6 \left(\frac{500}{3}\right)e^{\frac{3}{500}t} + C = 1000e^{\frac{3}{500}t} + C$$
  $$Q(t) = 1000 + C e^{-\frac{3}{500}t}$$
  * **Step 4: Apply $Q(0) = 0$:**
  $$0 = 1000 + C \implies C = -1000$$
  $$Q(t) = 1000\left(1 - e^{-\frac{3}{500}t}\right)\text{ kg}$$
  * **Step 5: Physical Sanity Check ($t \to \infty$):**
  $$\lim_{t \to \infty} Q(t) = 1000(1 - 0) = 1000\text{ kg}$$
  Does this make sense? Yes: after a very long time, the tank will be entirely replaced by the inflow solution:
  $$\text{Final Mass} = \text{Volume} \times C_{\text{in}} = 1000\text{ L} \times 1\text{ kg/L} = 1000\text{ kg} \quad \checkmark$$
  
  ---
#### **Case 2: Variable Volume ($r_{\text{in}} \neq r_{\text{out}}$)**
  When flow in does not equal flow out, the volume changes with time.
  
  ---
#### **Case Study 2: Tank Overflow Problem (Fall 2012 Exam 1 #4)**
  A $1000\text{ gal}$ tank originally holds $300\text{ gal}$ of solution containing $100\text{ lb}$ of salt. Water containing $2\text{ lb/gal}$ of salt enters at $5\text{ gal/min}$, and the well-stirred mixture leaves at $3\text{ gal/min}$.
##### **Part (a): When does the tank overflow?**
  * Track volume over time:
  $$\frac{dV}{dt} = r_{\text{in}} - r_{\text{out}} = 5 - 3 = 2\text{ gal/min}, \quad V(0) = 300\text{ gal}$$
  $$V(t) = 300 + 2t$$
  * The tank overflows when $V(t) = 1000\text{ gal}$:
  $$300 + 2t = 1000 \implies 2t = 700 \implies t_{\text{overflow}} = 350\text{ minutes}$$
##### **Part (b): Set up the IVP prior to overflow:**
  * Inflow rate of salt: $r_{\text{in}} \cdot C_{\text{in}} = (5\text{ gal/min}) \times (2\text{ lb/gal}) = 10\text{ lb/min}$
  * Outflow concentration: $C_{\text{out}}(t) = \frac{Q(t)}{V(t)} = \frac{Q(t)}{300 + 2t}$
  * Outflow rate of salt: $r_{\text{out}} \cdot C_{\text{out}}(t) = 3 \times \frac{Q(t)}{300 + 2t} = \frac{3Q(t)}{300 + 2t}$
  * **The Complete Initial Value Problem:**
  $$\frac{dQ}{dt} = 10 - \frac{3Q}{300 + 2t}, \quad Q(0) = 100, \quad 0 \le t \le 350$$
##### **Step-by-Step Explicit Solution (Going beyond the slide setup):**
  In standard linear form:
  $$\frac{dQ}{dt} + \frac{3}{2t + 300}Q = 10$$
  1. **Integrating Factor:**
   $$\mu(t) = \exp\left(\int \frac{3}{2t + 300}\,dt\right) = \exp\left(\frac{3}{2}\ln(2t + 300)\right) = (2t + 300)^{3/2}$$
  2. **Multiply and Integrate:**
   $$\frac{d}{dt}\left[(2t + 300)^{3/2} Q\right] = 10(2t + 300)^{3/2}$$
   $$(2t + 300)^{3/2} Q = 10 \cdot \frac{(2t + 300)^{5/2}}{\frac{5}{2} \cdot 2} + C = 2(2t + 300)^{5/2} + C$$
  3. **Solve for $Q(t)$:**
   $$Q(t) = 2(2t + 300) + \frac{C}{(2t + 300)^{3/2}}$$
  4. **Apply $Q(0) = 100$:**
   $$100 = 2(300) + \frac{C}{(300)^{3/2}} \implies -500 = \frac{C}{(300)^{3/2}} \implies C = -500(300)^{3/2}$$
   $$Q(t) = 2(2t + 300) - 500\left(\frac{300}{2t + 300}\right)^{3/2}$$
  
  ---
### 4. Financial Models: Continuous Compounding
#### Continuous Growth Equation (Slides 38–51)
  Let $S(t)$ be the balance of an investment with initial principal $S_0$ earning an annual interest rate $r$ compounded continuously:
  $$\frac{dS}{dt} = r S(t), \quad S(0) = S_0$$
  * Separating variables:
  $$\int \frac{1}{S}\,dS = \int r\,dt \implies \ln|S| = rt + C_1 \implies S(t) = S_0 e^{rt}$$
#### **Filling the Slide Gap: Continuous Deposits / Withdrawals**
  If money is deposited or withdrawn continuously at a constant annual dollar rate $k$:
  $$\frac{dS}{dt} = rS + k \quad (+k \text{ for deposits}, -k \text{ for withdrawals})$$
  * This is a first-order linear equation readily solved using the integrating factor $\mu(t) = e^{-rt}$.
  
  ---
### 5. Population Dynamics Models
  
  Population growth models describe how species populations change over time.
  
  ---
#### **Model 1: Malthusian Exponential Growth (Unrestricted)**
  $$\frac{dy}{dt} = ry, \quad y(0) = y_0 \implies y(t) = y_0 e^{rt}$$
  * **Assumption:** The per-capita growth rate (relative growth rate) $\frac{y'}{y} = r$ is constant.
  * **Limitation:** Predicts infinite, unbounded exponential explosion ($y \to \infty$ as $t \to \infty$). Realistic only for small populations with unlimited resources over short time windows.
  
  ---
#### **Model 2: The Logistic Model (Verhulst Growth)**
  To correct the Malthusian model, assume the relative growth rate declines linearly as population increases due to crowding and resource depletion:
  $$\frac{1}{y}\frac{dy}{dt} = r\left(1 - \frac{y}{K}\right) \implies \frac{dy}{dt} = r\left(1 - \frac{y}{K}\right)y = ry - \frac{r}{K}y^2$$
  * $r > 0$: Intrinsic growth rate.
  * $K > 0$: Environmental **carrying capacity** (saturation level).
##### **Step-by-Step Analytical Solution via Partial Fractions (Slides 63–66):**
  $$\frac{dy}{\left(1 - \frac{y}{K}\right)y} = r\,dt \implies \frac{K}{y(K - y)}\,dy = r\,dt$$
  Using partial fractions: $\frac{K}{y(K - y)} = \frac{1}{y} + \frac{1}{K - y}$
  $$\int \left(\frac{1}{y} + \frac{1}{K - y}\right)dy = \int r\,dt$$
  $$\ln|y| - \ln|K - y| = rt + c \implies \ln\left|\frac{y}{K - y}\right| = rt + c$$
  $$\frac{y}{K - y} = C_0 e^{rt}$$
  Applying initial condition $y(0) = y_0 \implies C_0 = \frac{y_0}{K - y_0}$:
  $$\frac{y}{K - y} = \left(\frac{y_0}{K - y_0}\right)e^{rt}$$
  Solving algebraically for $y(t)$:
  $$y(t) = \frac{y_0 K}{y_0 + (K - y_0)e^{-rt}}$$
##### **Key Qualitative & Calculus Features of the Logistic S-Curve:**
  * **Equilibria:** $y = 0$ (unstable) and $y = K$ (asymptotically stable).
  * **Long-Term Limit:** For any initial population $y_0 > 0$:
  $$\lim_{t \to \infty} y(t) = \frac{y_0 K}{y_0 + 0} = K$$
  * **Fastest Growth Point (Inflection Point):**
  * Differentiating $y' = r\left(y - \frac{y^2}{K}\right)$ with respect to $t$:
    $$y'' = r\left(1 - \frac{2y}{K}\right)y'$$
  * Setting $y'' = 0 \implies 1 - \frac{2y}{K} = 0 \implies y = \frac{K}{2}$.
  * The population grows at its maximum rate exactly when it reaches **half of its carrying capacity**.
  
  ---
#### **Model 3: Critical Threshold Model (Slide 67)**
  $$\frac{dy}{dt} = -r\left(1 - \frac{y}{T}\right)y, \quad y(0) = y_0$$
  * $T > 0$: The **critical threshold**.
  * If $y < T \implies y' < 0$ (population drops to $0$, leading to **extinction**).
  * If $y > T \implies y' > 0$ (population grows unboundedly).
  * The threshold $y = T$ represents the minimum population required to survive (e.g., finding mates, defense against predators).
  
  ---
#### **Model 4: Logistic Growth with Threshold / Allee Effect (Slide 68)**
  Combining carrying capacity $K$ and survival threshold $T$ (where $0 < T < K$):
  $$\frac{dy}{dt} = -r\left(1 - \frac{y}{T}\right)\left(1 - \frac{y}{K}\right)y$$
  
  ```
   y-axis
     ^
     |   y' < 0  (Pops above K die off back to K)
  ---+-------------------------------------------- y = K (Stable Carrying Capacity)
     |   y' > 0  (Pops grow up to K)
  ---+-------------------------------------------- y = T (Unstable Threshold)
     |   y' < 0  (Pops below T decline to extinction)
  ---+-------------------------------------------- y = 0 (Extinction Equilibrium)
  ```
  
  * **Three Equilibrium States:**
  1. $y = 0$: Stable (Extinction).
  2. $y = T$: Unstable (Threshold line; below it means doom, above it allows survival).
  3. $y = K$: Stable (Carrying Capacity attractor).
  
  ---
#### **Model 5: Population with Harvesting (Spring 2011 Exam 1 #4(a))**
  Let $P(t)$ denote fish population in a lake.
  * Birth rate is twice the population: $+2P(t)$
  * Death rate is proportional to the square of population: $-[P(t)]^2$
  * Harvesting occurs at a constant rate $h$: $-h$
  * **Differential Equation:**
  $$\frac{dP}{dt} = \text{Rate In} - \text{Rate Out} = 2P(t) - [P(t)]^2 - h$$
  
  ---
### 6. Physical Modeling: Motion on an Inclined Plane (Handwritten Notes)
  
  A box of mass $m$ slides **down** an inclined plane of angle $\theta$ with kinetic friction $\mu$ and linear air resistance $F_d = kv$.
  
  ```
               ^ y (normal)
               |
          +----+----+
          |    |    |
   F_f <--|    *--->|  (down the incline is +x)
   F_d <--|   /|    |
          +--/-+----+
            /  |
           /   v W = mg
          / \theta
         +-----------------
  ```
#### **Step 1: Set up a Tilted Coordinate System**
  * Let the $+x$-axis point **down along the incline** in the direction of motion.
  * Let the $+y$-axis point **perpendicular (normal) to the incline**.
#### **Step 2: Balance Forces in the $y$-Direction (Perpendicular)**
  * No acceleration occurs off the ramp: $a_y = 0$.
  $$\sum F_y = 0 \implies F_N - W_y = 0$$
  $$W_y = W \cos\theta = mg\cos\theta \implies F_N = mg\cos\theta$$
#### **Step 3: Balance Forces in the $x$-Direction (Parallel)**
  * The component of gravity pulling the box down the ramp:
  $$W_x = W \sin\theta = mg\sin\theta$$
  * Forces opposing motion (directed up the ramp, in the $-x$ direction):
  * Kinetic friction: $F_f = \mu F_N = \mu mg\cos\theta$
  * Air resistance / drag: $F_d = kv$
  * Apply Newton’s Second Law:
  $$\sum F_x = m \frac{dv}{dt} = W_x - F_f - F_d$$
  $$m \frac{dv}{dt} = mg\sin\theta - \mu mg\cos\theta - kv$$
  $$m\frac{dv}{dt} = -kv + mg(\sin\theta - \mu\cos\theta)$$
#### **Bonus Insight: Terminal Velocity on the Incline**
  As $t \to \infty$, acceleration ceases ($v' \to 0$). The terminal velocity is:
  $$0 = -kv_{\text{term}} + mg(\sin\theta - \mu\cos\theta) \implies v_{\text{term}} = \frac{mg(\sin\theta - \mu\cos\theta)}{k}$$
  
  ---
### Section 3.2 Summary & Exam Checklist
  * [x] **Mixing Tanks:** Always write $\frac{dQ}{dt} = \text{rate in} - \text{rate out}$. Check your units to make sure every term is in $[\text{mass}] / [\text{time}]$.
  * [x] **Variable Volume Tanks:** Find $V(t) = V_0 + (r_{\text{in}} - r_{\text{out}})t$ first. Plug it into the denominator of $C_{\text{out}}(t) = \frac{Q(t)}{V(t)}$.
  * [x] **Time of Overflow:** Set $V(t) = \text{Tank Capacity}$ and solve for $t$.
  * [x] **Logistic Models:** Memorize that the carrying capacity $K$ is the stable equilibrium ($\lim_{t \to \infty} P = K$), and the maximum growth rate occurs at $P = \frac{K}{2}$.
  * [x] **Thresholds:** If an equation has $-(1 - y/T)$, populations below $T$ decay to zero.
  * [x] **Mechanics:** Always decompose gravity into $mg\sin\theta$ (parallel to incline) and $mg\cos\theta$ (perpendicular to incline).
  
  ---